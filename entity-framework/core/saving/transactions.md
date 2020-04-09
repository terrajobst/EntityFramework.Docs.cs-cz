---
title: Transakce - ZÁKLADNÍ EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
uid: core/saving/transactions
ms.openlocfilehash: 390d89398ebfdf015804749e71ff0b61d3f278d3
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417553"
---
# <a name="using-transactions"></a>Použití transakcí

Transakce umožňují několik databázových operací, které mají být zpracovány atomickým způsobem. Pokud je transakce potvrzena, všechny operace jsou úspěšně použity v databázi. Pokud je transakce vrácena zpět, žádná z operací jsou použity pro databázi.

> [!TIP]  
> Ukázku tohoto článku [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Transactions/) můžete zobrazit na GitHubu.

## <a name="default-transaction-behavior"></a>Výchozí chování transakce

Ve výchozím nastavení, pokud poskytovatel databáze podporuje transakce, `SaveChanges()` všechny změny v jednom volání, které jsou použity v transakci. Pokud některá ze změn selže, transakce je vrácena zpět a žádná ze změn jsou použity v databázi. To znamená, že je zaručeno, že `SaveChanges()` buď zcela úspěšné, nebo ponechat databázi beze změny, pokud dojde k chybě.

Pro většinu aplikací toto výchozí chování je dostačující. Transakce byste měli řídit pouze v případě, že to požadavky na aplikaci považují za nezbytné.

## <a name="controlling-transactions"></a>Řízení transakcí

`DbContext.Database` Pomocí rozhraní API můžete zahájit, potvrdit a vrátit transakce. Následující příklad ukazuje `SaveChanges()` dvě operace a linq dotaz spouštěný v jedné transakci.

Ne všichni poskytovatelé databáze podporují transakce. Někteří zprostředkovatelé může vyvolat nebo no-op při volání transakční chod.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/ControllingTransaction/Sample.cs?name=Transaction&highlight=3,17,18,19)]

## <a name="cross-context-transaction-relational-databases-only"></a>Transakce mezi kontexty (pouze relační databáze)

Můžete také sdílet transakci mezi více kontextových instancí. Tato funkce je k dispozici pouze při použití zprostředkovatele `DbTransaction` relační databáze, protože vyžaduje použití a `DbConnection`, které jsou specifické pro relační databáze.

Chcete-li sdílet transakci, musí kontexty sdílet jak a `DbConnection` a `DbTransaction`.

### <a name="allow-connection-to-be-externally-provided"></a>Povolit připojení externě k dispozici

Sdílení `DbConnection` vyžaduje schopnost předat připojení do kontextu při jeho vytváření.

Nejjednodušší způsob, jak `DbConnection` umožnit externě za předpokladu, je přestat používat `DbContext.OnConfiguring` `DbContextOptions` metodu ke konfiguraci kontextu a externě vytvořit a předat je konstruktoru kontextu.

> [!TIP]  
> `DbContextOptionsBuilder`je rozhraní API, `DbContext.OnConfiguring` které jste použili v konfigurovat kontext, budete `DbContextOptions`nyní používat externě k vytvoření .

[!code-csharp[Main](../../../samples/core/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

Alternativou je pokračovat `DbContext.OnConfiguring`v používání `DbConnection` , ale přijmout, `DbContext.OnConfiguring`který je uložen a poté použit v .

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

### <a name="share-connection-and-transaction"></a>Sdílení připojení a transakce

Nyní můžete vytvořit více kontextových instancí, které sdílejí stejné připojení. Potom pomocí `DbContext.Database.UseTransaction(DbTransaction)` rozhraní API zařadit oba kontexty ve stejné transakci.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a>Použití externích dbtransactions (pouze relační databáze)

Pokud používáte více technologií přístupu k datům pro přístup k relační databázi, můžete chtít sdílet transakci mezi operacemi prováděnými těmito různými technologiemi.

Následující příklad ukazuje, jak provést operaci ADO.NET SqlClient a základní operace entity framework u stejné transakce.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a>Použití System.Transactions

> [!NOTE]  
> Tato funkce je v EF Core 2.1 nová.

Je možné použít okolní transakce, pokud potřebujete koordinovat napříč větším rozsahem.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,2,3,26,27,28)]

Je také možné zařadit do explicitní transakce.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,15,28,29,30)]

### <a name="limitations-of-systemtransactions"></a>Omezení systému.Transakce  

1. EF Core spoléhá na poskytovatele databází k implementaci podpory system.transactions. Přestože podpora je poměrně běžné mezi zprostředkovateli ADO.NET pro rozhraní .NET Framework, rozhraní API bylo přidáno pouze nedávno do .NET Core a proto podpora není tak rozšířená. Pokud zprostředkovatel neimplementuje podporu pro System.Transactions, je možné, že volání těchto api budou zcela ignorovány. SqlClient pro .NET Core podporuje od 2.1 dále. SqlClient pro rozhraní .NET Core 2.0 vyvolá výjimku, pokud se pokusíte použít tuto funkci.

   > [!IMPORTANT]  
   > Doporučujeme otestovat, že rozhraní API se chová správně s poskytovatelem, než se na něj spoléhat při správě transakcí. Pokud se tak nestane, obraťte se na správce zprostředkovatele databáze.

2. Od verze 2.1 implementace System.Transactions v .NET Core nezahrnuje podporu pro distribuované transakce, proto nelze použít `TransactionScope` nebo `CommittableTransaction` koordinovat transakce mezi více správci prostředků.
