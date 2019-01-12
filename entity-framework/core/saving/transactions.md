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
# <a name="using-transactions"></a><span data-ttu-id="6e138-102">Použití transakcí</span><span class="sxs-lookup"><span data-stu-id="6e138-102">Using Transactions</span></span>

<span data-ttu-id="6e138-103">Transakce povolit několika databázových operací zpracování atomic způsobem.</span><span class="sxs-lookup"><span data-stu-id="6e138-103">Transactions allow several database operations to be processed in an atomic manner.</span></span> <span data-ttu-id="6e138-104">Pokud je transakce potvrzena, všechny operace úspěšně použita pro databázi.</span><span class="sxs-lookup"><span data-stu-id="6e138-104">If the transaction is committed, all of the operations are successfully applied to the database.</span></span> <span data-ttu-id="6e138-105">Pokud transakce je vrácena zpět, žádná z operací se použijí k databázi.</span><span class="sxs-lookup"><span data-stu-id="6e138-105">If the transaction is rolled back, none of the operations are applied to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="6e138-106">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="6e138-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) on GitHub.</span></span>

## <a name="default-transaction-behavior"></a><span data-ttu-id="6e138-107">Výchozí chování při transakci</span><span class="sxs-lookup"><span data-stu-id="6e138-107">Default transaction behavior</span></span>

<span data-ttu-id="6e138-108">Ve výchozím nastavení, pokud poskytovatel databáze podporuje transakce, všechny změny v jednom volání do `SaveChanges()` se použijí v transakci.</span><span class="sxs-lookup"><span data-stu-id="6e138-108">By default, if the database provider supports transactions, all changes in a single call to `SaveChanges()` are applied in a transaction.</span></span> <span data-ttu-id="6e138-109">Pokud selžou i všechny změny, transakce se zrušila a žádná ze změn, jsou použita pro databázi.</span><span class="sxs-lookup"><span data-stu-id="6e138-109">If any of the changes fail, then the transaction is rolled back and none of the changes are applied to the database.</span></span> <span data-ttu-id="6e138-110">To znamená, že `SaveChanges()` je zaručeno, že buď zcela úspěšná, nebo ponechat databázi ponechat beze změny, pokud dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="6e138-110">This means that `SaveChanges()` is guaranteed to either completely succeed, or leave the database unmodified if an error occurs.</span></span>

<span data-ttu-id="6e138-111">Pro většinu aplikací toto výchozí chování je dostačující.</span><span class="sxs-lookup"><span data-stu-id="6e138-111">For most applications, this default behavior is sufficient.</span></span> <span data-ttu-id="6e138-112">By měl pouze ručně řídit transakce, pokud požadavcích aplikace považují za nezbytné.</span><span class="sxs-lookup"><span data-stu-id="6e138-112">You should only manually control transactions if your application requirements deem it necessary.</span></span>

## <a name="controlling-transactions"></a><span data-ttu-id="6e138-113">Řízení transakcí</span><span class="sxs-lookup"><span data-stu-id="6e138-113">Controlling transactions</span></span>

<span data-ttu-id="6e138-114">Můžete použít `DbContext.Database` rozhraní API, chcete-li začít, potvrzení a vrácení zpět transakcí.</span><span class="sxs-lookup"><span data-stu-id="6e138-114">You can use the `DbContext.Database` API to begin, commit, and rollback transactions.</span></span> <span data-ttu-id="6e138-115">Následující příklad ukazuje dva `SaveChanges()` operace a LINQ dotaz provádí v rámci jedné transakce.</span><span class="sxs-lookup"><span data-stu-id="6e138-115">The following example shows two `SaveChanges()` operations and a LINQ query being executed in a single transaction.</span></span>

<span data-ttu-id="6e138-116">Ne všichni poskytovatelé databáze podporu transakcí.</span><span class="sxs-lookup"><span data-stu-id="6e138-116">Not all database providers support transactions.</span></span> <span data-ttu-id="6e138-117">Někteří poskytovatelé může vyvolat nebo no-op při transakci volání rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="6e138-117">Some providers may throw or no-op when transaction APIs are called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/ControllingTransaction/Sample.cs?name=Transaction&highlight=3,17,18,19)]

## <a name="cross-context-transaction-relational-databases-only"></a><span data-ttu-id="6e138-118">Mezi kontextu transakce (pouze pro relační databáze)</span><span class="sxs-lookup"><span data-stu-id="6e138-118">Cross-context transaction (relational databases only)</span></span>

<span data-ttu-id="6e138-119">Můžete také sdílet transakce napříč několika instancemi kontextu.</span><span class="sxs-lookup"><span data-stu-id="6e138-119">You can also share a transaction across multiple context instances.</span></span> <span data-ttu-id="6e138-120">Tato funkce je dostupná jenom při použití zprostředkovatele relační databáze, protože vyžaduje použití `DbTransaction` a `DbConnection`, které jsou potřebné k relačním databázím.</span><span class="sxs-lookup"><span data-stu-id="6e138-120">This functionality is only available when using a relational database provider because it requires the use of `DbTransaction` and `DbConnection`, which are specific to relational databases.</span></span>

<span data-ttu-id="6e138-121">Sdílet transakce, kontexty musí sdílet i `DbConnection` a `DbTransaction`.</span><span class="sxs-lookup"><span data-stu-id="6e138-121">To share a transaction, the contexts must share both a `DbConnection` and a `DbTransaction`.</span></span>

### <a name="allow-connection-to-be-externally-provided"></a><span data-ttu-id="6e138-122">Povolit připojení k externě zadat</span><span class="sxs-lookup"><span data-stu-id="6e138-122">Allow connection to be externally provided</span></span>

<span data-ttu-id="6e138-123">Sdílení `DbConnection` vyžaduje možnost předávání připojení kontextu při jeho vytváření.</span><span class="sxs-lookup"><span data-stu-id="6e138-123">Sharing a `DbConnection` requires the ability to pass a connection into a context when constructing it.</span></span>

<span data-ttu-id="6e138-124">Nejjednodušší způsob, jak povolit `DbConnection` externě poskytnout, je přestat používat `DbContext.OnConfiguring` metoda konfigurace kontextu a externě vytvoření `DbContextOptions` a předat je do konstruktoru kontextu.</span><span class="sxs-lookup"><span data-stu-id="6e138-124">The easiest way to allow `DbConnection` to be externally provided, is to stop using the `DbContext.OnConfiguring` method to configure the context and externally create `DbContextOptions` and pass them to the context constructor.</span></span>

> [!TIP]  
> <span data-ttu-id="6e138-125">`DbContextOptionsBuilder` je rozhraní API, které jste použili v `DbContext.OnConfiguring` konfigurace kontextu, se teď chystáte externě ho použít k vytvoření `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="6e138-125">`DbContextOptionsBuilder` is the API you used in `DbContext.OnConfiguring` to configure the context, you are now going to use it externally to create `DbContextOptions`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

<span data-ttu-id="6e138-126">Alternativou je můžete dál používat `DbContext.OnConfiguring`, ale přijmout `DbConnection` , který se uloží a použije v `DbContext.OnConfiguring`.</span><span class="sxs-lookup"><span data-stu-id="6e138-126">An alternative is to keep using `DbContext.OnConfiguring`, but accept a `DbConnection` that is saved and then used in `DbContext.OnConfiguring`.</span></span>

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

### <a name="share-connection-and-transaction"></a><span data-ttu-id="6e138-127">Připojení sdílené složky a transakce</span><span class="sxs-lookup"><span data-stu-id="6e138-127">Share connection and transaction</span></span>

<span data-ttu-id="6e138-128">Nyní můžete vytvořit více instancí kontextu, které sdílejí stejné připojení.</span><span class="sxs-lookup"><span data-stu-id="6e138-128">You can now create multiple context instances that share the same connection.</span></span> <span data-ttu-id="6e138-129">Potom použijte `DbContext.Database.UseTransaction(DbTransaction)` rozhraní API k zařazení i kontextech v rámci jedné transakce.</span><span class="sxs-lookup"><span data-stu-id="6e138-129">Then use the `DbContext.Database.UseTransaction(DbTransaction)` API to enlist both contexts in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a><span data-ttu-id="6e138-130">Pomocí externích DbTransactions (pouze pro relační databáze)</span><span class="sxs-lookup"><span data-stu-id="6e138-130">Using external DbTransactions (relational databases only)</span></span>

<span data-ttu-id="6e138-131">Pokud používáte více technologií přístupu k datům získat přístup k relační databázi, můžete sdílet transakce mezi operací provedených metodou tyto různé technologie.</span><span class="sxs-lookup"><span data-stu-id="6e138-131">If you are using multiple data access technologies to access a relational database, you may want to share a transaction between operations performed by these different technologies.</span></span>

<span data-ttu-id="6e138-132">Následující příklad ukazuje, jak provádět operace Sqlclienta ADO.NET a Entity Framework Core operace v rámci jedné transakce.</span><span class="sxs-lookup"><span data-stu-id="6e138-132">The following example, shows how to perform an ADO.NET SqlClient operation and an Entity Framework Core operation in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a><span data-ttu-id="6e138-133">Using System.Transactions</span><span class="sxs-lookup"><span data-stu-id="6e138-133">Using System.Transactions</span></span>

> [!NOTE]  
> <span data-ttu-id="6e138-134">Tato funkce je nového v EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="6e138-134">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="6e138-135">Je možné použít okolí transakce, pokud je potřeba koordinovat napříč větší rozsah.</span><span class="sxs-lookup"><span data-stu-id="6e138-135">It is possible to use ambient transactions if you need to coordinate across a larger scope.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,2,3,26,27,28)]

<span data-ttu-id="6e138-136">Je také možné zařazení v explicitní transakci.</span><span class="sxs-lookup"><span data-stu-id="6e138-136">It is also possible to enlist in an explicit transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,15,28,29,30)]

### <a name="limitations-of-systemtransactions"></a><span data-ttu-id="6e138-137">Omezení objektu System.Transactions</span><span class="sxs-lookup"><span data-stu-id="6e138-137">Limitations of System.Transactions</span></span>  

1. <span data-ttu-id="6e138-138">EF Core využívá databázi zprostředkovatele pro implementaci podpory System.Transactions.</span><span class="sxs-lookup"><span data-stu-id="6e138-138">EF Core relies on database providers to implement support for System.Transactions.</span></span> <span data-ttu-id="6e138-139">I když je podpora je celkem běžné patří poskytovatelů služeb ADO.NET pro rozhraní .NET Framework, rozhraní API pouze byl nedávno přidán do .NET Core a proto není tak rozsáhlou podporu.</span><span class="sxs-lookup"><span data-stu-id="6e138-139">Although support is quite common among ADO.NET providers for .NET Framework, the API has only been recently added to .NET Core and hence support is not as widespread.</span></span> <span data-ttu-id="6e138-140">Pokud poskytovatel neimplementuje podporu pro System.Transactions, je možné, že volání těchto rozhraní API se zcela ignorovat.</span><span class="sxs-lookup"><span data-stu-id="6e138-140">If a provider does not implement support for System.Transactions, it is possible that calls to these APIs will be completely ignored.</span></span> <span data-ttu-id="6e138-141">SqlClient pro .NET Core nepodporuje z 2.1 a vyšší.</span><span class="sxs-lookup"><span data-stu-id="6e138-141">SqlClient for .NET Core does support it from 2.1 onwards.</span></span> <span data-ttu-id="6e138-142">SqlClient pro .NET Core 2.0 vyvolá výjimku, pokud se pokusíte použít funkci.</span><span class="sxs-lookup"><span data-stu-id="6e138-142">SqlClient for .NET Core 2.0 will throw an exception if you attempt to use the feature.</span></span> 

   > [!IMPORTANT]  
   > <span data-ttu-id="6e138-143">Doporučujeme, abyste otestovali, že rozhraní API správné chování u svého poskytovatele předtím, než byste spoléhat pro správu transakce.</span><span class="sxs-lookup"><span data-stu-id="6e138-143">It is recommended that you test that the API behaves correctly with your provider before you rely on it for managing transactions.</span></span> <span data-ttu-id="6e138-144">Jste vyzváni ke kontaktování funkce maintainer poskytovatele databáze, pokud tomu tak není.</span><span class="sxs-lookup"><span data-stu-id="6e138-144">You are encouraged to contact the maintainer of the database provider if it does not.</span></span> 

2. <span data-ttu-id="6e138-145">Od verze 2.1, implementace System.Transactions v .NET Core nezahrnuje podporu pro distribuované transakce, takže nemůže používat `TransactionScope` nebo `CommittableTransaction` koordinovat transakce napříč několika správci prostředků.</span><span class="sxs-lookup"><span data-stu-id="6e138-145">As of version 2.1, the System.Transactions implementation in .NET Core does not include support for distributed transactions, therefore you cannot use `TransactionScope` or `CommittableTransaction` to coordinate transactions across multiple resource managers.</span></span> 
