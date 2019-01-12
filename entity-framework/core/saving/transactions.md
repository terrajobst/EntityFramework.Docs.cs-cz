---
title: Transakce – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
uid: core/saving/transactions
ms.openlocfilehash: 4c50d6694c6678678c0af8defe2601abee923af1
ms.sourcegitcommit: 5f11a5fa5d2cde81a4e4d0d5c3a60aa74b83cbd4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/11/2019
ms.locfileid: "54226189"
---
# <a name="using-transactions"></a>Použití transakcí

Transakce povolit několika databázových operací zpracování atomic způsobem. Pokud je transakce potvrzena, všechny operace úspěšně použita pro databázi. Pokud transakce je vrácena zpět, žádná z operací se použijí k databázi.

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) na Githubu.

## <a name="default-transaction-behavior"></a>Výchozí chování při transakci

Ve výchozím nastavení, pokud poskytovatel databáze podporuje transakce, všechny změny v jednom volání do `SaveChanges()` se použijí v transakci. Pokud selžou i všechny změny, transakce se zrušila a žádná ze změn, jsou použita pro databázi. To znamená, že `SaveChanges()` je zaručeno, že buď zcela úspěšná, nebo ponechat databázi ponechat beze změny, pokud dojde k chybě.

Pro většinu aplikací toto výchozí chování je dostačující. By měl pouze ručně řídit transakce, pokud požadavcích aplikace považují za nezbytné.

## <a name="controlling-transactions"></a>Řízení transakcí

Můžete použít `DbContext.Database` rozhraní API, chcete-li začít, potvrzení a vrácení zpět transakcí. Následující příklad ukazuje dva `SaveChanges()` operace a LINQ dotaz provádí v rámci jedné transakce.

Ne všichni poskytovatelé databáze podporu transakcí. Někteří poskytovatelé může vyvolat nebo no-op při transakci volání rozhraní API.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/ControllingTransaction/Sample.cs?name=Transaction&highlight=3,17,18,19)]

## <a name="cross-context-transaction-relational-databases-only"></a>Mezi kontextu transakce (pouze pro relační databáze)

Můžete také sdílet transakce napříč několika instancemi kontextu. Tato funkce je dostupná jenom při použití zprostředkovatele relační databáze, protože vyžaduje použití `DbTransaction` a `DbConnection`, které jsou potřebné k relačním databázím.

Sdílet transakce, kontexty musí sdílet i `DbConnection` a `DbTransaction`.

### <a name="allow-connection-to-be-externally-provided"></a>Povolit připojení k externě zadat

Sdílení `DbConnection` vyžaduje možnost předávání připojení kontextu při jeho vytváření.

Nejjednodušší způsob, jak povolit `DbConnection` externě poskytnout, je přestat používat `DbContext.OnConfiguring` metoda konfigurace kontextu a externě vytvoření `DbContextOptions` a předat je do konstruktoru kontextu.

> [!TIP]  
> `DbContextOptionsBuilder` je rozhraní API, které jste použili v `DbContext.OnConfiguring` konfigurace kontextu, se teď chystáte externě ho použít k vytvoření `DbContextOptions`.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

Alternativou je můžete dál používat `DbContext.OnConfiguring`, ale přijmout `DbConnection` , který se uloží a použije v `DbContext.OnConfiguring`.

``` csharp
public class BloggingContext : DbContext
{
    private DbConnection _connection;

    public BloggingContext(DbConnection connection)
    {
      _connection = connection;
    }

    public DbSet<Blog> Blogs { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlServer(_connection);
    }
}
```

### <a name="share-connection-and-transaction"></a>Připojení sdílené složky a transakce

Nyní můžete vytvořit více instancí kontextu, které sdílejí stejné připojení. Potom použijte `DbContext.Database.UseTransaction(DbTransaction)` rozhraní API k zařazení i kontextech v rámci jedné transakce.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a>Pomocí externích DbTransactions (pouze pro relační databáze)

Pokud používáte více technologií přístupu k datům získat přístup k relační databázi, můžete sdílet transakce mezi operací provedených metodou tyto různé technologie.

Následující příklad ukazuje, jak provádět operace Sqlclienta ADO.NET a Entity Framework Core operace v rámci jedné transakce.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a>Using System.Transactions

> [!NOTE]  
> Tato funkce je nového v EF Core 2.1.

Je možné použít okolí transakce, pokud je potřeba koordinovat napříč větší rozsah.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,2,3,26,27,28)]

Je také možné zařazení v explicitní transakci.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,15,28,29,30)]

### <a name="limitations-of-systemtransactions"></a>Omezení objektu System.Transactions  

1. EF Core využívá databázi zprostředkovatele pro implementaci podpory System.Transactions. I když je podpora je celkem běžné patří poskytovatelů služeb ADO.NET pro rozhraní .NET Framework, rozhraní API pouze byl nedávno přidán do .NET Core a proto není tak rozsáhlou podporu. Pokud poskytovatel neimplementuje podporu pro System.Transactions, je možné, že volání těchto rozhraní API se zcela ignorovat. SqlClient pro .NET Core nepodporuje z 2.1 a vyšší. SqlClient pro .NET Core 2.0 vyvolá výjimku, pokud se pokusíte použít funkci. 

   > [!IMPORTANT]  
   > Doporučujeme, abyste otestovali, že rozhraní API správné chování u svého poskytovatele předtím, než byste spoléhat pro správu transakce. Jste vyzváni ke kontaktování funkce maintainer poskytovatele databáze, pokud tomu tak není. 

2. Od verze 2.1, implementace System.Transactions v .NET Core nezahrnuje podporu pro distribuované transakce, takže nemůže používat `TransactionScope` nebo `CommittableTransaction` koordinovat transakce napříč několika správci prostředků. 
