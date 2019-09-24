---
title: Transakce – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
uid: core/saving/transactions
ms.openlocfilehash: ff12c4e7ace1f1b9e503cb2353bcdd53efd87cce
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197893"
---
# <a name="using-transactions"></a><span data-ttu-id="e10d1-102">Použití transakcí</span><span class="sxs-lookup"><span data-stu-id="e10d1-102">Using Transactions</span></span>

<span data-ttu-id="e10d1-103">Transakce umožňují zpracovávat několik databázových operací atomovou způsobem.</span><span class="sxs-lookup"><span data-stu-id="e10d1-103">Transactions allow several database operations to be processed in an atomic manner.</span></span> <span data-ttu-id="e10d1-104">Pokud je transakce potvrzena, budou všechny operace v databázi úspěšně provedeny.</span><span class="sxs-lookup"><span data-stu-id="e10d1-104">If the transaction is committed, all of the operations are successfully applied to the database.</span></span> <span data-ttu-id="e10d1-105">Pokud se transakce vrátí zpět, v databázi se nepoužije žádná operace.</span><span class="sxs-lookup"><span data-stu-id="e10d1-105">If the transaction is rolled back, none of the operations are applied to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="e10d1-106">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Transactions/) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="e10d1-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Transactions/) on GitHub.</span></span>

## <a name="default-transaction-behavior"></a><span data-ttu-id="e10d1-107">Výchozí chování transakce</span><span class="sxs-lookup"><span data-stu-id="e10d1-107">Default transaction behavior</span></span>

<span data-ttu-id="e10d1-108">Ve výchozím nastavení platí, že pokud poskytovatel databáze podporuje transakce, všechny změny v jednom volání `SaveChanges()` na jsou v transakci aplikovány.</span><span class="sxs-lookup"><span data-stu-id="e10d1-108">By default, if the database provider supports transactions, all changes in a single call to `SaveChanges()` are applied in a transaction.</span></span> <span data-ttu-id="e10d1-109">Pokud se kterákoli z změn nezdaří, transakce se vrátí zpět a v databázi se nepoužije žádná změna.</span><span class="sxs-lookup"><span data-stu-id="e10d1-109">If any of the changes fail, then the transaction is rolled back and none of the changes are applied to the database.</span></span> <span data-ttu-id="e10d1-110">To znamená, `SaveChanges()` že je zaručená buď úplná úspěch, nebo ponechat databázi nezměněnou, pokud dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="e10d1-110">This means that `SaveChanges()` is guaranteed to either completely succeed, or leave the database unmodified if an error occurs.</span></span>

<span data-ttu-id="e10d1-111">U většiny aplikací je toto výchozí chování dostatečné.</span><span class="sxs-lookup"><span data-stu-id="e10d1-111">For most applications, this default behavior is sufficient.</span></span> <span data-ttu-id="e10d1-112">Transakce byste měli řídit jenom v případě, že požadavky vaší aplikace považují za nezbytné.</span><span class="sxs-lookup"><span data-stu-id="e10d1-112">You should only manually control transactions if your application requirements deem it necessary.</span></span>

## <a name="controlling-transactions"></a><span data-ttu-id="e10d1-113">Řízení transakcí</span><span class="sxs-lookup"><span data-stu-id="e10d1-113">Controlling transactions</span></span>

<span data-ttu-id="e10d1-114">K zahájení, potvrzení `DbContext.Database` a vrácení transakcí můžete použít rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e10d1-114">You can use the `DbContext.Database` API to begin, commit, and rollback transactions.</span></span> <span data-ttu-id="e10d1-115">Následující příklad ukazuje dvě `SaveChanges()` operace a dotaz LINQ spouštěný v rámci jedné transakce.</span><span class="sxs-lookup"><span data-stu-id="e10d1-115">The following example shows two `SaveChanges()` operations and a LINQ query being executed in a single transaction.</span></span>

<span data-ttu-id="e10d1-116">Ne všichni poskytovatelé databáze podporují transakce.</span><span class="sxs-lookup"><span data-stu-id="e10d1-116">Not all database providers support transactions.</span></span> <span data-ttu-id="e10d1-117">Někteří zprostředkovatelé mohou vyvolat nebo no-op při volání rozhraní API pro transakce.</span><span class="sxs-lookup"><span data-stu-id="e10d1-117">Some providers may throw or no-op when transaction APIs are called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/ControllingTransaction/Sample.cs?name=Transaction&highlight=3,17,18,19)]

## <a name="cross-context-transaction-relational-databases-only"></a><span data-ttu-id="e10d1-118">Transakce křížového kontextu (pouze relační databáze)</span><span class="sxs-lookup"><span data-stu-id="e10d1-118">Cross-context transaction (relational databases only)</span></span>

<span data-ttu-id="e10d1-119">Můžete také sdílet transakce napříč několika instancemi kontextu.</span><span class="sxs-lookup"><span data-stu-id="e10d1-119">You can also share a transaction across multiple context instances.</span></span> <span data-ttu-id="e10d1-120">Tato funkce je k dispozici pouze při použití poskytovatele relační databáze, protože vyžaduje použití `DbTransaction` a `DbConnection`, které jsou specifické pro relační databáze.</span><span class="sxs-lookup"><span data-stu-id="e10d1-120">This functionality is only available when using a relational database provider because it requires the use of `DbTransaction` and `DbConnection`, which are specific to relational databases.</span></span>

<span data-ttu-id="e10d1-121">Chcete-li sdílet transakci, kontexty musí sdílet `DbConnection` `DbTransaction`a a.</span><span class="sxs-lookup"><span data-stu-id="e10d1-121">To share a transaction, the contexts must share both a `DbConnection` and a `DbTransaction`.</span></span>

### <a name="allow-connection-to-be-externally-provided"></a><span data-ttu-id="e10d1-122">Umožnění externě dostupného připojení</span><span class="sxs-lookup"><span data-stu-id="e10d1-122">Allow connection to be externally provided</span></span>

<span data-ttu-id="e10d1-123">Sdílení a `DbConnection` vyžaduje možnost předat připojení do kontextu při jeho vytváření.</span><span class="sxs-lookup"><span data-stu-id="e10d1-123">Sharing a `DbConnection` requires the ability to pass a connection into a context when constructing it.</span></span>

<span data-ttu-id="e10d1-124">Nejjednodušší způsob `DbConnection` , jak se dá externě poskytnout, je zastavit `DbContext.OnConfiguring` použití metody ke konfiguraci kontextu a externě vytvořit `DbContextOptions` a předat do konstruktoru kontextu.</span><span class="sxs-lookup"><span data-stu-id="e10d1-124">The easiest way to allow `DbConnection` to be externally provided, is to stop using the `DbContext.OnConfiguring` method to configure the context and externally create `DbContextOptions` and pass them to the context constructor.</span></span>

> [!TIP]  
> <span data-ttu-id="e10d1-125">`DbContextOptionsBuilder`je rozhraní API, které jste `DbContext.OnConfiguring` použili při konfiguraci kontextu, teď ho budete používat externě k vytvoření. `DbContextOptions`</span><span class="sxs-lookup"><span data-stu-id="e10d1-125">`DbContextOptionsBuilder` is the API you used in `DbContext.OnConfiguring` to configure the context, you are now going to use it externally to create `DbContextOptions`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

<span data-ttu-id="e10d1-126">Alternativou je použití `DbContext.OnConfiguring`, ale `DbConnection` přijetí a potvrzení, které je uloženo a pak použito v `DbContext.OnConfiguring`.</span><span class="sxs-lookup"><span data-stu-id="e10d1-126">An alternative is to keep using `DbContext.OnConfiguring`, but accept a `DbConnection` that is saved and then used in `DbContext.OnConfiguring`.</span></span>

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

### <a name="share-connection-and-transaction"></a><span data-ttu-id="e10d1-127">Sdílet připojení a transakci</span><span class="sxs-lookup"><span data-stu-id="e10d1-127">Share connection and transaction</span></span>

<span data-ttu-id="e10d1-128">Nyní můžete vytvořit více instancí kontextu, které sdílejí stejné připojení.</span><span class="sxs-lookup"><span data-stu-id="e10d1-128">You can now create multiple context instances that share the same connection.</span></span> <span data-ttu-id="e10d1-129">Pak použijte `DbContext.Database.UseTransaction(DbTransaction)` rozhraní API k zařazení kontextů do stejné transakce.</span><span class="sxs-lookup"><span data-stu-id="e10d1-129">Then use the `DbContext.Database.UseTransaction(DbTransaction)` API to enlist both contexts in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a><span data-ttu-id="e10d1-130">Použití externích DbTransactions (jenom relační databáze)</span><span class="sxs-lookup"><span data-stu-id="e10d1-130">Using external DbTransactions (relational databases only)</span></span>

<span data-ttu-id="e10d1-131">Pokud pro přístup k relační databázi používáte více technologií pro přístup k datům, může být vhodné sdílet transakci mezi operacemi prováděnými těmito různými technologiemi.</span><span class="sxs-lookup"><span data-stu-id="e10d1-131">If you are using multiple data access technologies to access a relational database, you may want to share a transaction between operations performed by these different technologies.</span></span>

<span data-ttu-id="e10d1-132">Následující příklad ukazuje, jak provést operaci ADO.NET SqlClient a operaci Entity Framework Core ve stejné transakci.</span><span class="sxs-lookup"><span data-stu-id="e10d1-132">The following example, shows how to perform an ADO.NET SqlClient operation and an Entity Framework Core operation in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a><span data-ttu-id="e10d1-133">Použití System. Transactions</span><span class="sxs-lookup"><span data-stu-id="e10d1-133">Using System.Transactions</span></span>

> [!NOTE]  
> <span data-ttu-id="e10d1-134">Tato funkce je v EF Core 2,1 novinkou.</span><span class="sxs-lookup"><span data-stu-id="e10d1-134">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="e10d1-135">Je možné použít ambientní transakce, pokud potřebujete koordinovat větší rozsah.</span><span class="sxs-lookup"><span data-stu-id="e10d1-135">It is possible to use ambient transactions if you need to coordinate across a larger scope.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,2,3,26,27,28)]

<span data-ttu-id="e10d1-136">Je také možné zařadit do explicitní transakce.</span><span class="sxs-lookup"><span data-stu-id="e10d1-136">It is also possible to enlist in an explicit transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,15,28,29,30)]

### <a name="limitations-of-systemtransactions"></a><span data-ttu-id="e10d1-137">Omezení pro System. Transactions</span><span class="sxs-lookup"><span data-stu-id="e10d1-137">Limitations of System.Transactions</span></span>  

1. <span data-ttu-id="e10d1-138">EF Core spoléhá na to, že poskytovatelé databáze implementují podporu pro System. Transactions.</span><span class="sxs-lookup"><span data-stu-id="e10d1-138">EF Core relies on database providers to implement support for System.Transactions.</span></span> <span data-ttu-id="e10d1-139">I když je podpora poměrně častá mezi poskytovateli ADO.NET pro .NET Framework, rozhraní API se v poslední době přidalo do .NET Core, takže podpora není tak širší.</span><span class="sxs-lookup"><span data-stu-id="e10d1-139">Although support is quite common among ADO.NET providers for .NET Framework, the API has only been recently added to .NET Core and hence support is not as widespread.</span></span> <span data-ttu-id="e10d1-140">Pokud zprostředkovatel neimplementuje podporu pro System. Transactions, je možné, že volání těchto rozhraní API budou zcela ignorována.</span><span class="sxs-lookup"><span data-stu-id="e10d1-140">If a provider does not implement support for System.Transactions, it is possible that calls to these APIs will be completely ignored.</span></span> <span data-ttu-id="e10d1-141">SqlClient pro .NET Core ho podporuje od verze 2,1 a vyšší.</span><span class="sxs-lookup"><span data-stu-id="e10d1-141">SqlClient for .NET Core does support it from 2.1 onwards.</span></span> <span data-ttu-id="e10d1-142">SqlClient pro .NET Core 2,0 vyvolá výjimku, pokud se pokusíte funkci použít.</span><span class="sxs-lookup"><span data-stu-id="e10d1-142">SqlClient for .NET Core 2.0 will throw an exception if you attempt to use the feature.</span></span> 

   > [!IMPORTANT]  
   > <span data-ttu-id="e10d1-143">Doporučujeme, abyste před tím, než se spoléháte na správu transakcí, správně spolupracovali s vaším poskytovatelem.</span><span class="sxs-lookup"><span data-stu-id="e10d1-143">It is recommended that you test that the API behaves correctly with your provider before you rely on it for managing transactions.</span></span> <span data-ttu-id="e10d1-144">Pokud ne, doporučujeme, abyste se obrátili na údržbu poskytovatele databáze.</span><span class="sxs-lookup"><span data-stu-id="e10d1-144">You are encouraged to contact the maintainer of the database provider if it does not.</span></span> 

2. <span data-ttu-id="e10d1-145">Od verze 2,1 implementace System. Transactions v rozhraní .NET Core nezahrnuje podporu distribuovaných transakcí, proto nemůžete použít `TransactionScope` nebo `CommittableTransaction` pro koordinaci transakcí napříč více správci prostředků.</span><span class="sxs-lookup"><span data-stu-id="e10d1-145">As of version 2.1, the System.Transactions implementation in .NET Core does not include support for distributed transactions, therefore you cannot use `TransactionScope` or `CommittableTransaction` to coordinate transactions across multiple resource managers.</span></span> 
