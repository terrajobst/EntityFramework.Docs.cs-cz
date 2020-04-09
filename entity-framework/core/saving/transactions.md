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
# <a name="using-transactions"></a><span data-ttu-id="67d2a-102">Použití transakcí</span><span class="sxs-lookup"><span data-stu-id="67d2a-102">Using Transactions</span></span>

<span data-ttu-id="67d2a-103">Transakce umožňují několik databázových operací, které mají být zpracovány atomickým způsobem.</span><span class="sxs-lookup"><span data-stu-id="67d2a-103">Transactions allow several database operations to be processed in an atomic manner.</span></span> <span data-ttu-id="67d2a-104">Pokud je transakce potvrzena, všechny operace jsou úspěšně použity v databázi.</span><span class="sxs-lookup"><span data-stu-id="67d2a-104">If the transaction is committed, all of the operations are successfully applied to the database.</span></span> <span data-ttu-id="67d2a-105">Pokud je transakce vrácena zpět, žádná z operací jsou použity pro databázi.</span><span class="sxs-lookup"><span data-stu-id="67d2a-105">If the transaction is rolled back, none of the operations are applied to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="67d2a-106">Ukázku tohoto článku [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Transactions/) můžete zobrazit na GitHubu.</span><span class="sxs-lookup"><span data-stu-id="67d2a-106">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Transactions/) on GitHub.</span></span>

## <a name="default-transaction-behavior"></a><span data-ttu-id="67d2a-107">Výchozí chování transakce</span><span class="sxs-lookup"><span data-stu-id="67d2a-107">Default transaction behavior</span></span>

<span data-ttu-id="67d2a-108">Ve výchozím nastavení, pokud poskytovatel databáze podporuje transakce, `SaveChanges()` všechny změny v jednom volání, které jsou použity v transakci.</span><span class="sxs-lookup"><span data-stu-id="67d2a-108">By default, if the database provider supports transactions, all changes in a single call to `SaveChanges()` are applied in a transaction.</span></span> <span data-ttu-id="67d2a-109">Pokud některá ze změn selže, transakce je vrácena zpět a žádná ze změn jsou použity v databázi.</span><span class="sxs-lookup"><span data-stu-id="67d2a-109">If any of the changes fail, then the transaction is rolled back and none of the changes are applied to the database.</span></span> <span data-ttu-id="67d2a-110">To znamená, že je zaručeno, že `SaveChanges()` buď zcela úspěšné, nebo ponechat databázi beze změny, pokud dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="67d2a-110">This means that `SaveChanges()` is guaranteed to either completely succeed, or leave the database unmodified if an error occurs.</span></span>

<span data-ttu-id="67d2a-111">Pro většinu aplikací toto výchozí chování je dostačující.</span><span class="sxs-lookup"><span data-stu-id="67d2a-111">For most applications, this default behavior is sufficient.</span></span> <span data-ttu-id="67d2a-112">Transakce byste měli řídit pouze v případě, že to požadavky na aplikaci považují za nezbytné.</span><span class="sxs-lookup"><span data-stu-id="67d2a-112">You should only manually control transactions if your application requirements deem it necessary.</span></span>

## <a name="controlling-transactions"></a><span data-ttu-id="67d2a-113">Řízení transakcí</span><span class="sxs-lookup"><span data-stu-id="67d2a-113">Controlling transactions</span></span>

<span data-ttu-id="67d2a-114">`DbContext.Database` Pomocí rozhraní API můžete zahájit, potvrdit a vrátit transakce.</span><span class="sxs-lookup"><span data-stu-id="67d2a-114">You can use the `DbContext.Database` API to begin, commit, and rollback transactions.</span></span> <span data-ttu-id="67d2a-115">Následující příklad ukazuje `SaveChanges()` dvě operace a linq dotaz spouštěný v jedné transakci.</span><span class="sxs-lookup"><span data-stu-id="67d2a-115">The following example shows two `SaveChanges()` operations and a LINQ query being executed in a single transaction.</span></span>

<span data-ttu-id="67d2a-116">Ne všichni poskytovatelé databáze podporují transakce.</span><span class="sxs-lookup"><span data-stu-id="67d2a-116">Not all database providers support transactions.</span></span> <span data-ttu-id="67d2a-117">Někteří zprostředkovatelé může vyvolat nebo no-op při volání transakční chod.</span><span class="sxs-lookup"><span data-stu-id="67d2a-117">Some providers may throw or no-op when transaction APIs are called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/ControllingTransaction/Sample.cs?name=Transaction&highlight=3,17,18,19)]

## <a name="cross-context-transaction-relational-databases-only"></a><span data-ttu-id="67d2a-118">Transakce mezi kontexty (pouze relační databáze)</span><span class="sxs-lookup"><span data-stu-id="67d2a-118">Cross-context transaction (relational databases only)</span></span>

<span data-ttu-id="67d2a-119">Můžete také sdílet transakci mezi více kontextových instancí.</span><span class="sxs-lookup"><span data-stu-id="67d2a-119">You can also share a transaction across multiple context instances.</span></span> <span data-ttu-id="67d2a-120">Tato funkce je k dispozici pouze při použití zprostředkovatele `DbTransaction` relační databáze, protože vyžaduje použití a `DbConnection`, které jsou specifické pro relační databáze.</span><span class="sxs-lookup"><span data-stu-id="67d2a-120">This functionality is only available when using a relational database provider because it requires the use of `DbTransaction` and `DbConnection`, which are specific to relational databases.</span></span>

<span data-ttu-id="67d2a-121">Chcete-li sdílet transakci, musí kontexty sdílet jak a `DbConnection` a `DbTransaction`.</span><span class="sxs-lookup"><span data-stu-id="67d2a-121">To share a transaction, the contexts must share both a `DbConnection` and a `DbTransaction`.</span></span>

### <a name="allow-connection-to-be-externally-provided"></a><span data-ttu-id="67d2a-122">Povolit připojení externě k dispozici</span><span class="sxs-lookup"><span data-stu-id="67d2a-122">Allow connection to be externally provided</span></span>

<span data-ttu-id="67d2a-123">Sdílení `DbConnection` vyžaduje schopnost předat připojení do kontextu při jeho vytváření.</span><span class="sxs-lookup"><span data-stu-id="67d2a-123">Sharing a `DbConnection` requires the ability to pass a connection into a context when constructing it.</span></span>

<span data-ttu-id="67d2a-124">Nejjednodušší způsob, jak `DbConnection` umožnit externě za předpokladu, je přestat používat `DbContext.OnConfiguring` `DbContextOptions` metodu ke konfiguraci kontextu a externě vytvořit a předat je konstruktoru kontextu.</span><span class="sxs-lookup"><span data-stu-id="67d2a-124">The easiest way to allow `DbConnection` to be externally provided, is to stop using the `DbContext.OnConfiguring` method to configure the context and externally create `DbContextOptions` and pass them to the context constructor.</span></span>

> [!TIP]  
> <span data-ttu-id="67d2a-125">`DbContextOptionsBuilder`je rozhraní API, `DbContext.OnConfiguring` které jste použili v konfigurovat kontext, budete `DbContextOptions`nyní používat externě k vytvoření .</span><span class="sxs-lookup"><span data-stu-id="67d2a-125">`DbContextOptionsBuilder` is the API you used in `DbContext.OnConfiguring` to configure the context, you are now going to use it externally to create `DbContextOptions`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

<span data-ttu-id="67d2a-126">Alternativou je pokračovat `DbContext.OnConfiguring`v používání `DbConnection` , ale přijmout, `DbContext.OnConfiguring`který je uložen a poté použit v .</span><span class="sxs-lookup"><span data-stu-id="67d2a-126">An alternative is to keep using `DbContext.OnConfiguring`, but accept a `DbConnection` that is saved and then used in `DbContext.OnConfiguring`.</span></span>

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

### <a name="share-connection-and-transaction"></a><span data-ttu-id="67d2a-127">Sdílení připojení a transakce</span><span class="sxs-lookup"><span data-stu-id="67d2a-127">Share connection and transaction</span></span>

<span data-ttu-id="67d2a-128">Nyní můžete vytvořit více kontextových instancí, které sdílejí stejné připojení.</span><span class="sxs-lookup"><span data-stu-id="67d2a-128">You can now create multiple context instances that share the same connection.</span></span> <span data-ttu-id="67d2a-129">Potom pomocí `DbContext.Database.UseTransaction(DbTransaction)` rozhraní API zařadit oba kontexty ve stejné transakci.</span><span class="sxs-lookup"><span data-stu-id="67d2a-129">Then use the `DbContext.Database.UseTransaction(DbTransaction)` API to enlist both contexts in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a><span data-ttu-id="67d2a-130">Použití externích dbtransactions (pouze relační databáze)</span><span class="sxs-lookup"><span data-stu-id="67d2a-130">Using external DbTransactions (relational databases only)</span></span>

<span data-ttu-id="67d2a-131">Pokud používáte více technologií přístupu k datům pro přístup k relační databázi, můžete chtít sdílet transakci mezi operacemi prováděnými těmito různými technologiemi.</span><span class="sxs-lookup"><span data-stu-id="67d2a-131">If you are using multiple data access technologies to access a relational database, you may want to share a transaction between operations performed by these different technologies.</span></span>

<span data-ttu-id="67d2a-132">Následující příklad ukazuje, jak provést operaci ADO.NET SqlClient a základní operace entity framework u stejné transakce.</span><span class="sxs-lookup"><span data-stu-id="67d2a-132">The following example, shows how to perform an ADO.NET SqlClient operation and an Entity Framework Core operation in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a><span data-ttu-id="67d2a-133">Použití System.Transactions</span><span class="sxs-lookup"><span data-stu-id="67d2a-133">Using System.Transactions</span></span>

> [!NOTE]  
> <span data-ttu-id="67d2a-134">Tato funkce je v EF Core 2.1 nová.</span><span class="sxs-lookup"><span data-stu-id="67d2a-134">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="67d2a-135">Je možné použít okolní transakce, pokud potřebujete koordinovat napříč větším rozsahem.</span><span class="sxs-lookup"><span data-stu-id="67d2a-135">It is possible to use ambient transactions if you need to coordinate across a larger scope.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,2,3,26,27,28)]

<span data-ttu-id="67d2a-136">Je také možné zařadit do explicitní transakce.</span><span class="sxs-lookup"><span data-stu-id="67d2a-136">It is also possible to enlist in an explicit transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,15,28,29,30)]

### <a name="limitations-of-systemtransactions"></a><span data-ttu-id="67d2a-137">Omezení systému.Transakce</span><span class="sxs-lookup"><span data-stu-id="67d2a-137">Limitations of System.Transactions</span></span>  

1. <span data-ttu-id="67d2a-138">EF Core spoléhá na poskytovatele databází k implementaci podpory system.transactions.</span><span class="sxs-lookup"><span data-stu-id="67d2a-138">EF Core relies on database providers to implement support for System.Transactions.</span></span> <span data-ttu-id="67d2a-139">Přestože podpora je poměrně běžné mezi zprostředkovateli ADO.NET pro rozhraní .NET Framework, rozhraní API bylo přidáno pouze nedávno do .NET Core a proto podpora není tak rozšířená.</span><span class="sxs-lookup"><span data-stu-id="67d2a-139">Although support is quite common among ADO.NET providers for .NET Framework, the API has only been recently added to .NET Core and hence support is not as widespread.</span></span> <span data-ttu-id="67d2a-140">Pokud zprostředkovatel neimplementuje podporu pro System.Transactions, je možné, že volání těchto api budou zcela ignorovány.</span><span class="sxs-lookup"><span data-stu-id="67d2a-140">If a provider does not implement support for System.Transactions, it is possible that calls to these APIs will be completely ignored.</span></span> <span data-ttu-id="67d2a-141">SqlClient pro .NET Core podporuje od 2.1 dále.</span><span class="sxs-lookup"><span data-stu-id="67d2a-141">SqlClient for .NET Core does support it from 2.1 onwards.</span></span> <span data-ttu-id="67d2a-142">SqlClient pro rozhraní .NET Core 2.0 vyvolá výjimku, pokud se pokusíte použít tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="67d2a-142">SqlClient for .NET Core 2.0 will throw an exception if you attempt to use the feature.</span></span>

   > [!IMPORTANT]  
   > <span data-ttu-id="67d2a-143">Doporučujeme otestovat, že rozhraní API se chová správně s poskytovatelem, než se na něj spoléhat při správě transakcí.</span><span class="sxs-lookup"><span data-stu-id="67d2a-143">It is recommended that you test that the API behaves correctly with your provider before you rely on it for managing transactions.</span></span> <span data-ttu-id="67d2a-144">Pokud se tak nestane, obraťte se na správce zprostředkovatele databáze.</span><span class="sxs-lookup"><span data-stu-id="67d2a-144">You are encouraged to contact the maintainer of the database provider if it does not.</span></span>

2. <span data-ttu-id="67d2a-145">Od verze 2.1 implementace System.Transactions v .NET Core nezahrnuje podporu pro distribuované transakce, proto nelze použít `TransactionScope` nebo `CommittableTransaction` koordinovat transakce mezi více správci prostředků.</span><span class="sxs-lookup"><span data-stu-id="67d2a-145">As of version 2.1, the System.Transactions implementation in .NET Core does not include support for distributed transactions, therefore you cannot use `TransactionScope` or `CommittableTransaction` to coordinate transactions across multiple resource managers.</span></span>
