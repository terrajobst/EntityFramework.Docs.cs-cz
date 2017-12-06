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
# <a name="using-transactions"></a><span data-ttu-id="dc31c-102">Použití transakcí</span><span class="sxs-lookup"><span data-stu-id="dc31c-102">Using Transactions</span></span>

<span data-ttu-id="dc31c-103">Transakce povolit několik databázových operací, které mají být zpracovány atomic způsobem.</span><span class="sxs-lookup"><span data-stu-id="dc31c-103">Transactions allow several database operations to be processed in an atomic manner.</span></span> <span data-ttu-id="dc31c-104">Pokud je transakce potvrzena, všechny operace jsou úspěšně použity k databázi.</span><span class="sxs-lookup"><span data-stu-id="dc31c-104">If the transaction is committed, all of the operations are successfully applied to the database.</span></span> <span data-ttu-id="dc31c-105">Pokud transakce je vrácena zpět, žádné operace, které se použijí k databázi.</span><span class="sxs-lookup"><span data-stu-id="dc31c-105">If the transaction is rolled back, none of the operations are applied to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="dc31c-106">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="dc31c-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) on GitHub.</span></span>

## <a name="default-transaction-behavior"></a><span data-ttu-id="dc31c-107">Výchozí chování transakce</span><span class="sxs-lookup"><span data-stu-id="dc31c-107">Default transaction behavior</span></span>

<span data-ttu-id="dc31c-108">Ve výchozím nastavení, pokud zprostředkovatel databáze podporuje transakce, všechny změny v jednom volání `SaveChanges()` se použijí v transakci.</span><span class="sxs-lookup"><span data-stu-id="dc31c-108">By default, if the database provider supports transactions, all changes in a single call to `SaveChanges()` are applied in a transaction.</span></span> <span data-ttu-id="dc31c-109">Pokud selžou všechny změny, transakce je vrácena zpět a žádná ze změn, se použijí k databázi.</span><span class="sxs-lookup"><span data-stu-id="dc31c-109">If any of the changes fail, then the transaction is rolled back and none of the changes are applied to the database.</span></span> <span data-ttu-id="dc31c-110">To znamená, že `SaveChanges()` záruku, že buď zcela úspěšné, nebo ponechat databázi ponechat beze změny, pokud dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="dc31c-110">This means that `SaveChanges()` is guaranteed to either completely succeed, or leave the database unmodified if an error occurs.</span></span>

<span data-ttu-id="dc31c-111">Toto výchozí chování pro většinu aplikací, je dostačující.</span><span class="sxs-lookup"><span data-stu-id="dc31c-111">For most applications, this default behavior is sufficient.</span></span> <span data-ttu-id="dc31c-112">Transakce měli jenom ručně řídí, zda vaše požadavky aplikací považovat za nezbytné.</span><span class="sxs-lookup"><span data-stu-id="dc31c-112">You should only manually control transactions if your application requirements deem it necessary.</span></span>

## <a name="controlling-transactions"></a><span data-ttu-id="dc31c-113">Řízení transakce</span><span class="sxs-lookup"><span data-stu-id="dc31c-113">Controlling transactions</span></span>

<span data-ttu-id="dc31c-114">Můžete použít `DbContext.Database` rozhraní API, chcete-li začít, potvrzení a vrácení transakce.</span><span class="sxs-lookup"><span data-stu-id="dc31c-114">You can use the `DbContext.Database` API to begin, commit, and rollback transactions.</span></span> <span data-ttu-id="dc31c-115">Následující příklad ukazuje dva `SaveChanges()` operace a LINQ dotaz vykonáván v rámci jedné transakce.</span><span class="sxs-lookup"><span data-stu-id="dc31c-115">The following example shows two `SaveChanges()` operations and a LINQ query being executed in a single transaction.</span></span>

<span data-ttu-id="dc31c-116">Ne všechny databáze zprostředkovatelé podporovat transakce.</span><span class="sxs-lookup"><span data-stu-id="dc31c-116">Not all database providers support transactions.</span></span> <span data-ttu-id="dc31c-117">Někteří poskytovatelé může vyvolat nebo no-op při transakce jsou volání rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="dc31c-117">Some providers may throw or no-op when transaction APIs are called.</span></span>

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

## <a name="cross-context-transaction-relational-databases-only"></a><span data-ttu-id="dc31c-118">Mezi kontextu transakce (pouze relační databáze)</span><span class="sxs-lookup"><span data-stu-id="dc31c-118">Cross-context transaction (relational databases only)</span></span>

<span data-ttu-id="dc31c-119">Můžete také sdílet transakce ve více instancích kontextu.</span><span class="sxs-lookup"><span data-stu-id="dc31c-119">You can also share a transaction across multiple context instances.</span></span> <span data-ttu-id="dc31c-120">Tato funkce je dostupná pouze při použití zprostředkovatele relační databáze, protože vyžaduje použití `DbTransaction` a `DbConnection`, které jsou specifické pro relačních databází.</span><span class="sxs-lookup"><span data-stu-id="dc31c-120">This functionality is only available when using a relational database provider because it requires the use of `DbTransaction` and `DbConnection`, which are specific to relational databases.</span></span>

<span data-ttu-id="dc31c-121">Pokud chcete sdílet transakci, musí kontexty sdílet i `DbConnection` a `DbTransaction`.</span><span class="sxs-lookup"><span data-stu-id="dc31c-121">To share a transaction, the contexts must share both a `DbConnection` and a `DbTransaction`.</span></span>

### <a name="allow-connection-to-be-externally-provided"></a><span data-ttu-id="dc31c-122">Povolit připojení k externě zadat</span><span class="sxs-lookup"><span data-stu-id="dc31c-122">Allow connection to be externally provided</span></span>

<span data-ttu-id="dc31c-123">Sdílení `DbConnection` vyžaduje možnost předat připojení v kontextu při jeho vytváření.</span><span class="sxs-lookup"><span data-stu-id="dc31c-123">Sharing a `DbConnection` requires the ability to pass a connection into a context when constructing it.</span></span>

<span data-ttu-id="dc31c-124">Nejjednodušší způsob, jak povolit `DbConnection` externě zadat, je přestat používat, `DbContext.OnConfiguring` metoda konfigurace kontextu a externě vytvořit `DbContextOptions` a předat je do kontextu konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="dc31c-124">The easiest way to allow `DbConnection` to be externally provided, is to stop using the `DbContext.OnConfiguring` method to configure the context and externally create `DbContextOptions` and pass them to the context constructor.</span></span>

> [!TIP]  
> <span data-ttu-id="dc31c-125">`DbContextOptionsBuilder`je rozhraní API, kterou jste použili v `DbContext.OnConfiguring` konfigurace kontextu, teď chcete externě použít k vytvoření `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="dc31c-125">`DbContextOptionsBuilder` is the API you used in `DbContext.OnConfiguring` to configure the context, you are now going to use it externally to create `DbContextOptions`.</span></span>

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

<span data-ttu-id="dc31c-126">Alternativou je dál používat `DbContext.OnConfiguring`, ale přijmout `DbConnection` , uložit a potom použít v `DbContext.OnConfiguring`.</span><span class="sxs-lookup"><span data-stu-id="dc31c-126">An alternative is to keep using `DbContext.OnConfiguring`, but accept a `DbConnection` that is saved and then used in `DbContext.OnConfiguring`.</span></span>

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

### <a name="share-connection-and-transaction"></a><span data-ttu-id="dc31c-127">Připojení sdílené složky a transakce</span><span class="sxs-lookup"><span data-stu-id="dc31c-127">Share connection and transaction</span></span>

<span data-ttu-id="dc31c-128">Nyní můžete vytvořit více instancí kontextu, které sdílejí stejné připojení.</span><span class="sxs-lookup"><span data-stu-id="dc31c-128">You can now create multiple context instances that share the same connection.</span></span> <span data-ttu-id="dc31c-129">Potom pomocí `DbContext.Database.UseTransaction(DbTransaction)` rozhraní API zařazení i kontexty ve stejné transakci.</span><span class="sxs-lookup"><span data-stu-id="dc31c-129">Then use the `DbContext.Database.UseTransaction(DbTransaction)` API to enlist both contexts in the same transaction.</span></span>

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

## <a name="using-external-dbtransactions-relational-databases-only"></a><span data-ttu-id="dc31c-130">Pomocí externích DbTransactions (pouze relační databáze)</span><span class="sxs-lookup"><span data-stu-id="dc31c-130">Using external DbTransactions (relational databases only)</span></span>

<span data-ttu-id="dc31c-131">Pokud používáte více technologie přístup k datům pro přístup k relační databázi, můžete sdílet transakce mezi operacemi provádí tyto různé technologie.</span><span class="sxs-lookup"><span data-stu-id="dc31c-131">If you are using multiple data access technologies to access a relational database, you may want to share a transaction between operations performed by these different technologies.</span></span>

<span data-ttu-id="dc31c-132">Následující příklad ukazuje, jak k provedení operace ADO.NET SqlClient a Entity Framework Core operace ve stejné transakci.</span><span class="sxs-lookup"><span data-stu-id="dc31c-132">The following example, shows how to perform an ADO.NET SqlClient operation and an Entity Framework Core operation in the same transaction.</span></span>

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
