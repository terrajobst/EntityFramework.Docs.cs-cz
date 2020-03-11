---
title: Transakce – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
uid: core/saving/transactions
ms.openlocfilehash: 390d89398ebfdf015804749e71ff0b61d3f278d3
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417553"
---
# <a name="using-transactions"></a>Použití transakcí

Transakce umožňují zpracovávat několik databázových operací atomovou způsobem. Pokud je transakce potvrzena, budou všechny operace v databázi úspěšně provedeny. Pokud se transakce vrátí zpět, v databázi se nepoužije žádná operace.

> [!TIP]  
> [Ukázku](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Transactions/) tohoto článku můžete zobrazit na GitHubu.

## <a name="default-transaction-behavior"></a>Výchozí chování transakce

Ve výchozím nastavení platí, že pokud poskytovatel databáze podporuje transakce, jsou všechny změny v jednom volání `SaveChanges()` aplikovány v transakci. Pokud se kterákoli z změn nezdaří, transakce se vrátí zpět a v databázi se nepoužije žádná změna. To znamená, že `SaveChanges()` je zaručené buď kompletně, nebo ponechat databázi nezměněnou, pokud dojde k chybě.

U většiny aplikací je toto výchozí chování dostatečné. Transakce byste měli řídit jenom v případě, že požadavky vaší aplikace považují za nezbytné.

## <a name="controlling-transactions"></a>Řízení transakcí

K zahájení, potvrzení a vrácení transakcí můžete použít rozhraní `DbContext.Database` API. Následující příklad ukazuje dvě operace `SaveChanges()` a dotaz LINQ prováděný v rámci jedné transakce.

Ne všichni poskytovatelé databáze podporují transakce. Někteří zprostředkovatelé mohou vyvolat nebo no-op při volání rozhraní API pro transakce.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/ControllingTransaction/Sample.cs?name=Transaction&highlight=3,17,18,19)]

## <a name="cross-context-transaction-relational-databases-only"></a>Transakce křížového kontextu (pouze relační databáze)

Můžete také sdílet transakce napříč několika instancemi kontextu. Tato funkce je k dispozici pouze při použití poskytovatele relační databáze, protože vyžaduje použití `DbTransaction` a `DbConnection`, která jsou specifická pro relační databáze.

Chcete-li sdílet transakci, kontexty musí sdílet `DbConnection` i `DbTransaction`.

### <a name="allow-connection-to-be-externally-provided"></a>Umožnění externě dostupného připojení

Sdílení `DbConnection` vyžaduje možnost předat připojení do kontextu při jeho vytváření.

Nejjednodušší způsob, jak `DbConnection` být umožněn externě, je zastavit použití metody `DbContext.OnConfiguring` ke konfiguraci kontextu a externě vytvořeným `DbContextOptions` a jejich předání do konstruktoru kontextu.

> [!TIP]  
> `DbContextOptionsBuilder` je rozhraní API, které jste použili v `DbContext.OnConfiguring` ke konfiguraci kontextu, teď ho budete používat externě k vytvoření `DbContextOptions`.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

Alternativou je dál používat `DbContext.OnConfiguring`, ale přijměte `DbConnection`, který se uloží a pak používá v `DbContext.OnConfiguring`.

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

### <a name="share-connection-and-transaction"></a>Sdílet připojení a transakci

Nyní můžete vytvořit více instancí kontextu, které sdílejí stejné připojení. Pak použijte rozhraní `DbContext.Database.UseTransaction(DbTransaction)` API k zařazení kontextů do stejné transakce.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a>Použití externích DbTransactions (jenom relační databáze)

Pokud pro přístup k relační databázi používáte více technologií pro přístup k datům, může být vhodné sdílet transakci mezi operacemi prováděnými těmito různými technologiemi.

Následující příklad ukazuje, jak provést operaci ADO.NET SqlClient a operaci Entity Framework Core ve stejné transakci.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a>Použití System. Transactions

> [!NOTE]  
> Tato funkce je v EF Core 2,1 novinkou.

Je možné použít ambientní transakce, pokud potřebujete koordinovat větší rozsah.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,2,3,26,27,28)]

Je také možné zařadit do explicitní transakce.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,15,28,29,30)]

### <a name="limitations-of-systemtransactions"></a>Omezení pro System. Transactions  

1. EF Core spoléhá na to, že poskytovatelé databáze implementují podporu pro System. Transactions. I když je podpora poměrně častá mezi poskytovateli ADO.NET pro .NET Framework, rozhraní API se v poslední době přidalo do .NET Core, takže podpora není tak širší. Pokud zprostředkovatel neimplementuje podporu pro System. Transactions, je možné, že volání těchto rozhraní API budou zcela ignorována. SqlClient pro .NET Core ho podporuje od verze 2,1 a vyšší. SqlClient pro .NET Core 2,0 vyvolá výjimku, pokud se pokusíte funkci použít.

   > [!IMPORTANT]  
   > Doporučujeme, abyste před tím, než se spoléháte na správu transakcí, správně spolupracovali s vaším poskytovatelem. Pokud ne, doporučujeme, abyste se obrátili na údržbu poskytovatele databáze.

2. Od verze 2,1 implementace System. Transactions v .NET Core nezahrnuje podporu distribuovaných transakcí, proto nemůžete použít `TransactionScope` ani `CommittableTransaction` k koordinaci transakcí napříč více správci prostředků.
