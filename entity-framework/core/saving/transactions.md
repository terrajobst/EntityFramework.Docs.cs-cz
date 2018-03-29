---
title: Transakce - EF jádra
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
ms.technology: entity-framework-core
uid: core/saving/transactions
ms.openlocfilehash: fe4c0d6ad7ccb2e97dc94fbf2eb26a41e7fbcb19
ms.sourcegitcommit: 7113e8675f26cbb546200824512078bf360225df
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/28/2018
---
# <a name="using-transactions"></a>Použití transakcí

Transakce povolit několik databázových operací, které mají být zpracovány atomic způsobem. Pokud je transakce potvrzena, všechny operace jsou úspěšně použity k databázi. Pokud transakce je vrácena zpět, žádné operace, které se použijí k databázi.

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) na Githubu.

## <a name="default-transaction-behavior"></a>Výchozí chování transakce

Ve výchozím nastavení, pokud zprostředkovatel databáze podporuje transakce, všechny změny v jednom volání `SaveChanges()` se použijí v transakci. Pokud selžou všechny změny, transakce je vrácena zpět a žádná ze změn, se použijí k databázi. To znamená, že `SaveChanges()` záruku, že buď zcela úspěšné, nebo ponechat databázi ponechat beze změny, pokud dojde k chybě.

Toto výchozí chování pro většinu aplikací, je dostačující. Transakce měli jenom ručně řídí, zda vaše požadavky aplikací považovat za nezbytné.

## <a name="controlling-transactions"></a>Řízení transakce

Můžete použít `DbContext.Database` rozhraní API, chcete-li začít, potvrzení a vrácení transakce. Následující příklad ukazuje dva `SaveChanges()` operace a LINQ dotaz vykonáván v rámci jedné transakce.

Ne všechny databáze zprostředkovatelé podporovat transakce. Někteří poskytovatelé může vyvolat nebo no-op při transakce jsou volání rozhraní API.

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/ControllingTransaction/Sample.cs?highlight=3,17,18,19)] -->
``` csharp
        using (var context = new BloggingContext())
        {
            using (var transaction = context.Database.BeginTransaction())
            {
                try
                {
                    context.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/dotnet" });
                    context.SaveChanges();

                    context.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/visualstudio" });
                    context.SaveChanges();

                    var blogs = context.Blogs
                        .OrderBy(b => b.Url)
                        .ToList();

                    // Commit transaction if all commands succeed, transaction will auto-rollback
                    // when disposed if either commands fails
                    transaction.Commit();
                }
                catch (Exception)
                {
                    // TODO: Handle failure
                }
            }
        }
```

## <a name="cross-context-transaction-relational-databases-only"></a>Mezi kontextu transakce (pouze relační databáze)

Můžete také sdílet transakce ve více instancích kontextu. Tato funkce je dostupná pouze při použití zprostředkovatele relační databáze, protože vyžaduje použití `DbTransaction` a `DbConnection`, které jsou specifické pro relačních databází.

Pokud chcete sdílet transakci, musí kontexty sdílet i `DbConnection` a `DbTransaction`.

### <a name="allow-connection-to-be-externally-provided"></a>Povolit připojení k externě zadat

Sdílení `DbConnection` vyžaduje možnost předat připojení v kontextu při jeho vytváření.

Nejjednodušší způsob, jak povolit `DbConnection` externě zadat, je přestat používat, `DbContext.OnConfiguring` metoda konfigurace kontextu a externě vytvořit `DbContextOptions` a předat je do kontextu konstruktoru.

> [!TIP]  
> `DbContextOptionsBuilder` je rozhraní API, kterou jste použili v `DbContext.OnConfiguring` konfigurace kontextu, teď chcete externě použít k vytvoření `DbContextOptions`.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

Alternativou je dál používat `DbContext.OnConfiguring`, ale přijmout `DbConnection` , uložit a potom použít v `DbContext.OnConfiguring`.

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

Nyní můžete vytvořit více instancí kontextu, které sdílejí stejné připojení. Potom pomocí `DbContext.Database.UseTransaction(DbTransaction)` rozhraní API zařazení i kontexty ve stejné transakci.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a>Pomocí externích DbTransactions (pouze relační databáze)

Pokud používáte více technologie přístup k datům pro přístup k relační databázi, můžete sdílet transakce mezi operacemi provádí tyto různé technologie.

Následující příklad ukazuje, jak k provedení operace ADO.NET SqlClient a Entity Framework Core operace ve stejné transakci.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a>System.Transactions – použití

> [!NOTE]  
> Tato funkce je nového v EF základní 2.1.

Je možné použít vedlejším transakcí, pokud je potřeba koordinovat ve větší rozsah.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,24,25,26)]

Je také možné uvést v explicitní transakce.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,13,26,27,28)]

### <a name="limitations-of-systemtransactions"></a>System.Transactions – omezení  

1. Základní EF spoléhá na zprostředkovatele databáze pro implementaci podpory System.Transactions. I když podpora je celkem běžné mezi zprostředkovatele ADO.NET pro rozhraní .NET Framework, rozhraní API pouze byl nedávno přidán do .NET Core a proto podporují není možné jako rozšířeným. Pokud zprostředkovatele neimplementuje podporu pro System.Transactions, je možné, že volání tato rozhraní API budou zcela ignorovány. SqlClient pro .NET Core podporuje z 2.1 a vyšší. SqlClient pro rozhraní .NET 2.0 základní vyvolá výjimku z pokusíte použít funkci. 

   > [!IMPORTANT]  
   > Doporučujeme, abyste otestovali, že rozhraní API chovat správně u svého poskytovatele předtím, než byste tedy spoléhat na něm pro správu transakcí. Jste vyzváni ke kontaktování funkce maintainer zprostředkovatele databáze, pokud neexistuje. 

2. Od verze 2.1 System.Transactions – implementace v .NET Core nezahrnuje podpora distribuovaných transakcí, takže nemůže používat `TransactionScope` nebo `CommitableTransaction` koordinovat transakce napříč více správců prostředků. 
