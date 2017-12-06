---
title: "Transakce - EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
ms.technology: entity-framework-core
uid: core/saving/transactions
ms.openlocfilehash: a2f890c0af1e83cbcc1d40d68540ff7132a9bafd
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
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
> `DbContextOptionsBuilder`je rozhraní API, kterou jste použili v `DbContext.OnConfiguring` konfigurace kontextu, teď chcete externě použít k vytvoření `DbContextOptions`.

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?highlight=3,4,5)] -->
``` csharp
    public class BloggingContext : DbContext
    {
        public BloggingContext(DbContextOptions<BloggingContext> options)
            : base(options)
        { }

        public DbSet<Blog> Blogs { get; set; }
    }
```

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

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?highlight=1,2,3,7,16,23,24,25)] -->
``` csharp
        var options = new DbContextOptionsBuilder<BloggingContext>()
            .UseSqlServer(new SqlConnection(connectionString))
            .Options;

        using (var context1 = new BloggingContext(options))
        {
            using (var transaction = context1.Database.BeginTransaction())
            {
                try
                {
                    context1.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/dotnet" });
                    context1.SaveChanges();

                    using (var context2 = new BloggingContext(options))
                    {
                        context2.Database.UseTransaction(transaction.GetDbTransaction());

                        var blogs = context2.Blogs
                            .OrderBy(b => b.Url)
                            .ToList();
                    }

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

## <a name="using-external-dbtransactions-relational-databases-only"></a>Pomocí externích DbTransactions (pouze relační databáze)

Pokud používáte více technologie přístup k datům pro přístup k relační databázi, můžete sdílet transakce mezi operacemi provádí tyto různé technologie.

Následující příklad ukazuje, jak k provedení operace ADO.NET SqlClient a Entity Framework Core operace ve stejné transakci.

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/ExternalDbTransaction/Sample.cs?highlight=4,10,21,26,27,28)] -->
``` csharp
        var connection = new SqlConnection(connectionString);
        connection.Open();

        using (var transaction = connection.BeginTransaction())
        {
            try
            {
                // Run raw ADO.NET command in the transaction
                var command = connection.CreateCommand();
                command.Transaction = transaction;
                command.CommandText = "DELETE FROM dbo.Blogs";
                command.ExecuteNonQuery();

                // Run an EF Core command in the transaction
                var options = new DbContextOptionsBuilder<BloggingContext>()
                    .UseSqlServer(connection)
                    .Options;

                using (var context = new BloggingContext(options))
                {
                    context.Database.UseTransaction(transaction);
                    context.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/dotnet" });
                    context.SaveChanges();
                }

                // Commit transaction if all commands succeed, transaction will auto-rollback
                // when disposed if either commands fails
                transaction.Commit();
            }
            catch (System.Exception)
            {
                // TODO: Handle failure
            }
```
