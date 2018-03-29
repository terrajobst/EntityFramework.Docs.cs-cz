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
# <a name="using-transactions"></a><span data-ttu-id="477f8-102">Použití transakcí</span><span class="sxs-lookup"><span data-stu-id="477f8-102">Using Transactions</span></span>

<span data-ttu-id="477f8-103">Transakce povolit několik databázových operací, které mají být zpracovány atomic způsobem.</span><span class="sxs-lookup"><span data-stu-id="477f8-103">Transactions allow several database operations to be processed in an atomic manner.</span></span> <span data-ttu-id="477f8-104">Pokud je transakce potvrzena, všechny operace jsou úspěšně použity k databázi.</span><span class="sxs-lookup"><span data-stu-id="477f8-104">If the transaction is committed, all of the operations are successfully applied to the database.</span></span> <span data-ttu-id="477f8-105">Pokud transakce je vrácena zpět, žádné operace, které se použijí k databázi.</span><span class="sxs-lookup"><span data-stu-id="477f8-105">If the transaction is rolled back, none of the operations are applied to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="477f8-106">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="477f8-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) on GitHub.</span></span>

## <a name="default-transaction-behavior"></a><span data-ttu-id="477f8-107">Výchozí chování transakce</span><span class="sxs-lookup"><span data-stu-id="477f8-107">Default transaction behavior</span></span>

<span data-ttu-id="477f8-108">Ve výchozím nastavení, pokud zprostředkovatel databáze podporuje transakce, všechny změny v jednom volání `SaveChanges()` se použijí v transakci.</span><span class="sxs-lookup"><span data-stu-id="477f8-108">By default, if the database provider supports transactions, all changes in a single call to `SaveChanges()` are applied in a transaction.</span></span> <span data-ttu-id="477f8-109">Pokud selžou všechny změny, transakce je vrácena zpět a žádná ze změn, se použijí k databázi.</span><span class="sxs-lookup"><span data-stu-id="477f8-109">If any of the changes fail, then the transaction is rolled back and none of the changes are applied to the database.</span></span> <span data-ttu-id="477f8-110">To znamená, že `SaveChanges()` záruku, že buď zcela úspěšné, nebo ponechat databázi ponechat beze změny, pokud dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="477f8-110">This means that `SaveChanges()` is guaranteed to either completely succeed, or leave the database unmodified if an error occurs.</span></span>

<span data-ttu-id="477f8-111">Toto výchozí chování pro většinu aplikací, je dostačující.</span><span class="sxs-lookup"><span data-stu-id="477f8-111">For most applications, this default behavior is sufficient.</span></span> <span data-ttu-id="477f8-112">Transakce měli jenom ručně řídí, zda vaše požadavky aplikací považovat za nezbytné.</span><span class="sxs-lookup"><span data-stu-id="477f8-112">You should only manually control transactions if your application requirements deem it necessary.</span></span>

## <a name="controlling-transactions"></a><span data-ttu-id="477f8-113">Řízení transakce</span><span class="sxs-lookup"><span data-stu-id="477f8-113">Controlling transactions</span></span>

<span data-ttu-id="477f8-114">Můžete použít `DbContext.Database` rozhraní API, chcete-li začít, potvrzení a vrácení transakce.</span><span class="sxs-lookup"><span data-stu-id="477f8-114">You can use the `DbContext.Database` API to begin, commit, and rollback transactions.</span></span> <span data-ttu-id="477f8-115">Následující příklad ukazuje dva `SaveChanges()` operace a LINQ dotaz vykonáván v rámci jedné transakce.</span><span class="sxs-lookup"><span data-stu-id="477f8-115">The following example shows two `SaveChanges()` operations and a LINQ query being executed in a single transaction.</span></span>

<span data-ttu-id="477f8-116">Ne všechny databáze zprostředkovatelé podporovat transakce.</span><span class="sxs-lookup"><span data-stu-id="477f8-116">Not all database providers support transactions.</span></span> <span data-ttu-id="477f8-117">Někteří poskytovatelé může vyvolat nebo no-op při transakce jsou volání rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="477f8-117">Some providers may throw or no-op when transaction APIs are called.</span></span>

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

## <a name="cross-context-transaction-relational-databases-only"></a><span data-ttu-id="477f8-118">Mezi kontextu transakce (pouze relační databáze)</span><span class="sxs-lookup"><span data-stu-id="477f8-118">Cross-context transaction (relational databases only)</span></span>

<span data-ttu-id="477f8-119">Můžete také sdílet transakce ve více instancích kontextu.</span><span class="sxs-lookup"><span data-stu-id="477f8-119">You can also share a transaction across multiple context instances.</span></span> <span data-ttu-id="477f8-120">Tato funkce je dostupná pouze při použití zprostředkovatele relační databáze, protože vyžaduje použití `DbTransaction` a `DbConnection`, které jsou specifické pro relačních databází.</span><span class="sxs-lookup"><span data-stu-id="477f8-120">This functionality is only available when using a relational database provider because it requires the use of `DbTransaction` and `DbConnection`, which are specific to relational databases.</span></span>

<span data-ttu-id="477f8-121">Pokud chcete sdílet transakci, musí kontexty sdílet i `DbConnection` a `DbTransaction`.</span><span class="sxs-lookup"><span data-stu-id="477f8-121">To share a transaction, the contexts must share both a `DbConnection` and a `DbTransaction`.</span></span>

### <a name="allow-connection-to-be-externally-provided"></a><span data-ttu-id="477f8-122">Povolit připojení k externě zadat</span><span class="sxs-lookup"><span data-stu-id="477f8-122">Allow connection to be externally provided</span></span>

<span data-ttu-id="477f8-123">Sdílení `DbConnection` vyžaduje možnost předat připojení v kontextu při jeho vytváření.</span><span class="sxs-lookup"><span data-stu-id="477f8-123">Sharing a `DbConnection` requires the ability to pass a connection into a context when constructing it.</span></span>

<span data-ttu-id="477f8-124">Nejjednodušší způsob, jak povolit `DbConnection` externě zadat, je přestat používat, `DbContext.OnConfiguring` metoda konfigurace kontextu a externě vytvořit `DbContextOptions` a předat je do kontextu konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="477f8-124">The easiest way to allow `DbConnection` to be externally provided, is to stop using the `DbContext.OnConfiguring` method to configure the context and externally create `DbContextOptions` and pass them to the context constructor.</span></span>

> [!TIP]  
> <span data-ttu-id="477f8-125">`DbContextOptionsBuilder` je rozhraní API, kterou jste použili v `DbContext.OnConfiguring` konfigurace kontextu, teď chcete externě použít k vytvoření `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="477f8-125">`DbContextOptionsBuilder` is the API you used in `DbContext.OnConfiguring` to configure the context, you are now going to use it externally to create `DbContextOptions`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

<span data-ttu-id="477f8-126">Alternativou je dál používat `DbContext.OnConfiguring`, ale přijmout `DbConnection` , uložit a potom použít v `DbContext.OnConfiguring`.</span><span class="sxs-lookup"><span data-stu-id="477f8-126">An alternative is to keep using `DbContext.OnConfiguring`, but accept a `DbConnection` that is saved and then used in `DbContext.OnConfiguring`.</span></span>

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

### <a name="share-connection-and-transaction"></a><span data-ttu-id="477f8-127">Připojení sdílené složky a transakce</span><span class="sxs-lookup"><span data-stu-id="477f8-127">Share connection and transaction</span></span>

<span data-ttu-id="477f8-128">Nyní můžete vytvořit více instancí kontextu, které sdílejí stejné připojení.</span><span class="sxs-lookup"><span data-stu-id="477f8-128">You can now create multiple context instances that share the same connection.</span></span> <span data-ttu-id="477f8-129">Potom pomocí `DbContext.Database.UseTransaction(DbTransaction)` rozhraní API zařazení i kontexty ve stejné transakci.</span><span class="sxs-lookup"><span data-stu-id="477f8-129">Then use the `DbContext.Database.UseTransaction(DbTransaction)` API to enlist both contexts in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a><span data-ttu-id="477f8-130">Pomocí externích DbTransactions (pouze relační databáze)</span><span class="sxs-lookup"><span data-stu-id="477f8-130">Using external DbTransactions (relational databases only)</span></span>

<span data-ttu-id="477f8-131">Pokud používáte více technologie přístup k datům pro přístup k relační databázi, můžete sdílet transakce mezi operacemi provádí tyto různé technologie.</span><span class="sxs-lookup"><span data-stu-id="477f8-131">If you are using multiple data access technologies to access a relational database, you may want to share a transaction between operations performed by these different technologies.</span></span>

<span data-ttu-id="477f8-132">Následující příklad ukazuje, jak k provedení operace ADO.NET SqlClient a Entity Framework Core operace ve stejné transakci.</span><span class="sxs-lookup"><span data-stu-id="477f8-132">The following example, shows how to perform an ADO.NET SqlClient operation and an Entity Framework Core operation in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a><span data-ttu-id="477f8-133">System.Transactions – použití</span><span class="sxs-lookup"><span data-stu-id="477f8-133">Using System.Transactions</span></span>

> [!NOTE]  
> <span data-ttu-id="477f8-134">Tato funkce je nového v EF základní 2.1.</span><span class="sxs-lookup"><span data-stu-id="477f8-134">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="477f8-135">Je možné použít vedlejším transakcí, pokud je potřeba koordinovat ve větší rozsah.</span><span class="sxs-lookup"><span data-stu-id="477f8-135">It is possible to use ambient transactions if you need to coordinate across a larger scope.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,24,25,26)]

<span data-ttu-id="477f8-136">Je také možné uvést v explicitní transakce.</span><span class="sxs-lookup"><span data-stu-id="477f8-136">It is also possible to enlist in an explicit transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,13,26,27,28)]

### <a name="limitations-of-systemtransactions"></a><span data-ttu-id="477f8-137">System.Transactions – omezení</span><span class="sxs-lookup"><span data-stu-id="477f8-137">Limitations of System.Transactions</span></span>  

1. <span data-ttu-id="477f8-138">Základní EF spoléhá na zprostředkovatele databáze pro implementaci podpory System.Transactions.</span><span class="sxs-lookup"><span data-stu-id="477f8-138">EF Core relies on database providers to implement support for System.Transactions.</span></span> <span data-ttu-id="477f8-139">I když podpora je celkem běžné mezi zprostředkovatele ADO.NET pro rozhraní .NET Framework, rozhraní API pouze byl nedávno přidán do .NET Core a proto podporují není možné jako rozšířeným.</span><span class="sxs-lookup"><span data-stu-id="477f8-139">Although support is quite common among ADO.NET providers for .NET Framework, the API has only been recently added to .NET Core and hence support is not be as widespread.</span></span> <span data-ttu-id="477f8-140">Pokud zprostředkovatele neimplementuje podporu pro System.Transactions, je možné, že volání tato rozhraní API budou zcela ignorovány.</span><span class="sxs-lookup"><span data-stu-id="477f8-140">If a provider does not implement support for System.Transactions, it is possible that calls to these APIs will be completely ignored.</span></span> <span data-ttu-id="477f8-141">SqlClient pro .NET Core podporuje z 2.1 a vyšší.</span><span class="sxs-lookup"><span data-stu-id="477f8-141">SqlClient for .NET Core does support it from 2.1 onwards.</span></span> <span data-ttu-id="477f8-142">SqlClient pro rozhraní .NET 2.0 základní vyvolá výjimku z pokusíte použít funkci.</span><span class="sxs-lookup"><span data-stu-id="477f8-142">SqlClient for .NET Core 2.0 will throw an exception of you attempt to use the feature.</span></span> 

   > [!IMPORTANT]  
   > <span data-ttu-id="477f8-143">Doporučujeme, abyste otestovali, že rozhraní API chovat správně u svého poskytovatele předtím, než byste tedy spoléhat na něm pro správu transakcí.</span><span class="sxs-lookup"><span data-stu-id="477f8-143">It is recommended that you test that the API behaves correctly with your provider before you rely on it for managing transactions.</span></span> <span data-ttu-id="477f8-144">Jste vyzváni ke kontaktování funkce maintainer zprostředkovatele databáze, pokud neexistuje.</span><span class="sxs-lookup"><span data-stu-id="477f8-144">You are encouraged to contact the maintainer of the database provider if it does not.</span></span> 

2. <span data-ttu-id="477f8-145">Od verze 2.1 System.Transactions – implementace v .NET Core nezahrnuje podpora distribuovaných transakcí, takže nemůže používat `TransactionScope` nebo `CommitableTransaction` koordinovat transakce napříč více správců prostředků.</span><span class="sxs-lookup"><span data-stu-id="477f8-145">As of version 2.1, the System.Transactions implementation in .NET Core does not include support for distributed transactions, therefore you cannot use `TransactionScope` or `CommitableTransaction` to coordinate transactions across multiple resource managers.</span></span> 
