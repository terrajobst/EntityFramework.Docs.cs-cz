---
title: Konfigurace DbContext-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: e04c1b65df96819f3493e0ed34ccf26609511f6a
ms.sourcegitcommit: 37d0e0fd1703467918665a64837dc54ad2ec7484
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/16/2019
ms.locfileid: "72445906"
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="17a6b-102">Konfigurace DbContext</span><span class="sxs-lookup"><span data-stu-id="17a6b-102">Configuring a DbContext</span></span>

<span data-ttu-id="17a6b-103">Tento článek ukazuje základní vzory pro konfiguraci `DbContext` prostřednictvím `DbContextOptions` pro připojení k databázi pomocí konkrétního poskytovatele EF Core a volitelného chování.</span><span class="sxs-lookup"><span data-stu-id="17a6b-103">This article shows basic patterns for configuring a `DbContext` via a `DbContextOptions` to connect to a database using a specific EF Core provider and optional behaviors.</span></span>

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="17a6b-104">Konfigurace DbContext v době návrhu</span><span class="sxs-lookup"><span data-stu-id="17a6b-104">Design-time DbContext configuration</span></span>

<span data-ttu-id="17a6b-105">EF Core nástroje pro návrh, jako třeba [migrace](xref:core/managing-schemas/migrations/index) , musí být schopné zjistit a vytvořit pracovní instanci typu `DbContext`, aby bylo možné shromáždit podrobnosti o typech entit aplikace a jak se mapují ke schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="17a6b-105">EF Core design-time tools such as [migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="17a6b-106">Tento proces může být automatický, dokud může nástroj snadno vytvořit `DbContext` tak, že bude nakonfigurován podobně jako v době běhu.</span><span class="sxs-lookup"><span data-stu-id="17a6b-106">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at run-time.</span></span>

<span data-ttu-id="17a6b-107">I když jakýkoliv model, který poskytuje potřebné informace o konfiguraci `DbContext` může pracovat v době běhu, nástroje, které vyžadují použití `DbContext` v době návrhu, mohou pracovat pouze s omezeným počtem vzorů.</span><span class="sxs-lookup"><span data-stu-id="17a6b-107">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="17a6b-108">Tyto informace jsou podrobněji popsány v části [vytváření kontextu při návrhu](xref:core/miscellaneous/cli/dbcontext-creation) .</span><span class="sxs-lookup"><span data-stu-id="17a6b-108">These are covered in more detail in the [Design-Time Context Creation](xref:core/miscellaneous/cli/dbcontext-creation) section.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="17a6b-109">Konfigurace DbContextOptions</span><span class="sxs-lookup"><span data-stu-id="17a6b-109">Configuring DbContextOptions</span></span>

<span data-ttu-id="17a6b-110">`DbContext` musí mít instanci `DbContextOptions`, aby bylo možné provést jakoukoli práci.</span><span class="sxs-lookup"><span data-stu-id="17a6b-110">`DbContext` must have an instance of `DbContextOptions` in order to perform any work.</span></span> <span data-ttu-id="17a6b-111">Instance `DbContextOptions` obsahuje konfigurační informace, jako například:</span><span class="sxs-lookup"><span data-stu-id="17a6b-111">The `DbContextOptions` instance carries configuration information such as:</span></span>

- <span data-ttu-id="17a6b-112">Poskytovatel databáze, který má být použit, který je obvykle vybrán vyvoláním metody, jako je například `UseSqlServer` nebo `UseSqlite`.</span><span class="sxs-lookup"><span data-stu-id="17a6b-112">The database provider to use, typically selected by invoking a method such as `UseSqlServer` or `UseSqlite`.</span></span> <span data-ttu-id="17a6b-113">Tyto metody rozšíření vyžadují odpovídající balíček poskytovatele, například `Microsoft.EntityFrameworkCore.SqlServer` nebo `Microsoft.EntityFrameworkCore.Sqlite`.</span><span class="sxs-lookup"><span data-stu-id="17a6b-113">These extension methods require the corresponding provider package, such as `Microsoft.EntityFrameworkCore.SqlServer` or `Microsoft.EntityFrameworkCore.Sqlite`.</span></span> <span data-ttu-id="17a6b-114">Metody jsou definovány v oboru názvů `Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="17a6b-114">The methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span>
- <span data-ttu-id="17a6b-115">Jakýkoli potřebný připojovací řetězec nebo identifikátor instance databáze, který se obvykle předává jako argument pro metodu výběru poskytovatele uvedenou výše.</span><span class="sxs-lookup"><span data-stu-id="17a6b-115">Any necessary connection string or identifier of the database instance, typically passed as an argument to the provider selection method mentioned above</span></span>
- <span data-ttu-id="17a6b-116">Jakékoli volitelné selektory chování na úrovni poskytovatele, obvykle také zřetězené uvnitř volání metody výběru poskytovatele</span><span class="sxs-lookup"><span data-stu-id="17a6b-116">Any provider-level optional behavior selectors, typically also chained inside the call to the provider selection method</span></span>
- <span data-ttu-id="17a6b-117">Všechny obecné selektory chování EF Core, obvykle zřetězené po nebo před metodou selektoru poskytovatele</span><span class="sxs-lookup"><span data-stu-id="17a6b-117">Any general EF Core behavior selectors, typically chained after or before the provider selector method</span></span>

<span data-ttu-id="17a6b-118">V následujícím příkladu je nakonfiguruje `DbContextOptions` pro použití poskytovatele SQL Server, připojení obsažené v proměnné `connectionString`, časový limit příkazu na úrovni poskytovatele a selektoru EF Core chování, které provádí všechny dotazy spouštěné v rámci `DbContext` [No-Tracking](xref:core/querying/tracking#no-tracking-queries) . ve výchozím nastavení:</span><span class="sxs-lookup"><span data-stu-id="17a6b-118">The following example configures the `DbContextOptions` to use the SQL Server provider, a connection contained in the `connectionString` variable, a provider-level command timeout, and an EF Core behavior selector that makes all queries executed in the `DbContext` [no-tracking](xref:core/querying/tracking#no-tracking-queries) by default:</span></span>

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> <span data-ttu-id="17a6b-119">Metody selektoru poskytovatele a další výše uvedené metody selektoru chování jsou rozšiřující metody pro třídy možností `DbContextOptions` nebo specifické pro poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="17a6b-119">Provider selector methods and other behavior selector methods mentioned above are extension methods on `DbContextOptions` or provider-specific option classes.</span></span> <span data-ttu-id="17a6b-120">Aby bylo možné mít přístup k těmto metodám rozšíření, možná budete muset mít v oboru obor názvů (obvykle `Microsoft.EntityFrameworkCore`) a zahrnout do projektu další závislosti balíčků.</span><span class="sxs-lookup"><span data-stu-id="17a6b-120">In order to have access to these extension methods you may need to have a namespace (typically `Microsoft.EntityFrameworkCore`) in scope and include additional package dependencies in the project.</span></span>

<span data-ttu-id="17a6b-121">@No__t-0 lze dodávat do `DbContext` přepsáním metody `OnConfiguring` nebo externě prostřednictvím argumentu konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="17a6b-121">The `DbContextOptions` can be supplied to the `DbContext` by overriding the `OnConfiguring` method or externally via a constructor argument.</span></span>

<span data-ttu-id="17a6b-122">Pokud jsou oba použity, `OnConfiguring` se použije jako poslední a může přepsat možnosti zadané v argumentu konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="17a6b-122">If both are used, `OnConfiguring` is applied last and can overwrite options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="17a6b-123">Argument konstruktoru</span><span class="sxs-lookup"><span data-stu-id="17a6b-123">Constructor argument</span></span>

<span data-ttu-id="17a6b-124">Konstruktor může jednoduše přijmout `DbContextOptions`, jak je znázorněno níže:</span><span class="sxs-lookup"><span data-stu-id="17a6b-124">Your constructor can simply accept a `DbContextOptions` as follows:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
        : base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

> [!TIP]  
> <span data-ttu-id="17a6b-125">Základní konstruktor DbContext také přijímá neobecnou verzi `DbContextOptions`, ale použití neobecné verze není doporučeno pro aplikace s více typy kontextu.</span><span class="sxs-lookup"><span data-stu-id="17a6b-125">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`, but using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="17a6b-126">Vaše aplikace teď může předat `DbContextOptions` při vytváření instance kontextu následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="17a6b-126">Your application can now pass the `DbContextOptions` when instantiating a context, as follows:</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="17a6b-127">Probíhá konfigurace</span><span class="sxs-lookup"><span data-stu-id="17a6b-127">OnConfiguring</span></span>

<span data-ttu-id="17a6b-128">Můžete také inicializovat `DbContextOptions` v rámci samotného kontextu.</span><span class="sxs-lookup"><span data-stu-id="17a6b-128">You can also initialize the `DbContextOptions` within the context itself.</span></span> <span data-ttu-id="17a6b-129">I když můžete použít tuto techniku pro základní konfiguraci, obvykle budete potřebovat získat určité podrobnosti o konfiguraci z vnějšku, např. připojovací řetězec databáze.</span><span class="sxs-lookup"><span data-stu-id="17a6b-129">While you can use this technique for basic configuration, you will typically still need to get certain configuration details from the outside, e.g. a database connection string.</span></span> <span data-ttu-id="17a6b-130">To se dá udělat s rozhraním API konfigurace nebo jakýmkoli jiným způsobem.</span><span class="sxs-lookup"><span data-stu-id="17a6b-130">This can be done with a configuration API or any other means.</span></span>

<span data-ttu-id="17a6b-131">Chcete-li v rámci kontextu inicializovat `DbContextOptions`, přepište metodu `OnConfiguring` a zavolejte metody na poskytnuté `DbContextOptionsBuilder`:</span><span class="sxs-lookup"><span data-stu-id="17a6b-131">To initialize `DbContextOptions` within the context, override the `OnConfiguring` method and call the methods on the provided `DbContextOptionsBuilder`:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlite("Data Source=blog.db");
    }
}
```

<span data-ttu-id="17a6b-132">Aplikace může jednoduše vytvořit instanci takového kontextu, aniž by bylo nutné předávat cokoli do konstruktoru:</span><span class="sxs-lookup"><span data-stu-id="17a6b-132">An application can simply instantiate such a context without passing anything to its constructor:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> <span data-ttu-id="17a6b-133">Tento přístup nezpůsobí testování, pokud testy cílí na úplnou databázi.</span><span class="sxs-lookup"><span data-stu-id="17a6b-133">This approach does not lend itself to testing, unless the tests target the full database.</span></span>

### <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="17a6b-134">Použití DbContext se vkládáním závislostí</span><span class="sxs-lookup"><span data-stu-id="17a6b-134">Using DbContext with dependency injection</span></span>

<span data-ttu-id="17a6b-135">EF Core podporuje použití `DbContext` s kontejnerem vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="17a6b-135">EF Core supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="17a6b-136">Typ DbContext lze přidat do kontejneru služby pomocí metody `AddDbContext<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="17a6b-136">Your DbContext type can be added to the service container by using the `AddDbContext<TContext>` method.</span></span>

<span data-ttu-id="17a6b-137">`AddDbContext<TContext>` nastaví typ DbContext, `TContext` a odpovídající `DbContextOptions<TContext>` k dispozici pro vkládání z kontejneru služby.</span><span class="sxs-lookup"><span data-stu-id="17a6b-137">`AddDbContext<TContext>` will make both your DbContext type, `TContext`, and the corresponding `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="17a6b-138">Další informace o vkládání závislostí najdete v další části o [přečtení](#more-reading) .</span><span class="sxs-lookup"><span data-stu-id="17a6b-138">See [more reading](#more-reading) below for additional information on dependency injection.</span></span>

<span data-ttu-id="17a6b-139">Přidání `DbContext` do injektáže závislosti:</span><span class="sxs-lookup"><span data-stu-id="17a6b-139">Adding the `DbContext` to dependency injection:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="17a6b-140">To vyžaduje přidání [argumentu konstruktoru](#constructor-argument) do typu DbContext, který přijímá `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="17a6b-140">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions<TContext>`.</span></span>

<span data-ttu-id="17a6b-141">Kód kontextu:</span><span class="sxs-lookup"><span data-stu-id="17a6b-141">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="17a6b-142">Kód aplikace (v ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="17a6b-142">Application code (in ASP.NET Core):</span></span>

``` csharp
public class MyController
{
    private readonly BloggingContext _context;

    public MyController(BloggingContext context)
    {
      _context = context;
    }

    ...
}
```

<span data-ttu-id="17a6b-143">Kód aplikace (s použitím ServiceProvider přímo, méně časté):</span><span class="sxs-lookup"><span data-stu-id="17a6b-143">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```
## <a name="avoiding-dbcontext-threading-issues"></a><span data-ttu-id="17a6b-144">Předcházení problémům s vlákny DbContext</span><span class="sxs-lookup"><span data-stu-id="17a6b-144">Avoiding DbContext threading issues</span></span>

<span data-ttu-id="17a6b-145">Entity Framework Core nepodporuje spouštění více paralelních operací na stejné instanci `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="17a6b-145">Entity Framework Core does not support multiple parallel operations being run on the same `DbContext` instance.</span></span> <span data-ttu-id="17a6b-146">To zahrnuje paralelní spuštění asynchronních dotazů a jakékoli explicitní souběžné použití z více vláken.</span><span class="sxs-lookup"><span data-stu-id="17a6b-146">This includes both parallel execution of async queries and any explicit concurrent use from multiple threads.</span></span> <span data-ttu-id="17a6b-147">Proto vždy asynchronní volání `await` okamžitě nebo pro operace, které se spouštějí paralelně, použijte samostatné instance `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="17a6b-147">Therefore, always `await` async calls immediately, or use separate `DbContext` instances for operations that execute in parallel.</span></span>

<span data-ttu-id="17a6b-148">Když EF Core detekuje souběžný pokus o použití instance `DbContext`, zobrazí se `InvalidOperationException` se zprávou, například:</span><span class="sxs-lookup"><span data-stu-id="17a6b-148">When EF Core detects an attempt to use a `DbContext` instance concurrently, you'll see an `InvalidOperationException` with a message like this:</span></span> 

> <span data-ttu-id="17a6b-149">Druhá operace začala v tomto kontextu před dokončením předchozí operace.</span><span class="sxs-lookup"><span data-stu-id="17a6b-149">A second operation started on this context before a previous operation completed.</span></span> <span data-ttu-id="17a6b-150">To je obvykle způsobeno různými vlákny pomocí stejné instance DbContext, ale členy instance nejsou zaručeny jako bezpečné pro přístup z více vláken.</span><span class="sxs-lookup"><span data-stu-id="17a6b-150">This is usually caused by different threads using the same instance of DbContext, however instance members are not guaranteed to be thread safe.</span></span>

<span data-ttu-id="17a6b-151">Pokud se souběžný přístup nedetekuje, může to mít za následek nedefinované chování, chyby aplikace a poškození dat.</span><span class="sxs-lookup"><span data-stu-id="17a6b-151">When concurrent access goes undetected, it can result in undefined behavior, application crashes and data corruption.</span></span>

<span data-ttu-id="17a6b-152">Existují běžné chyby, které můžou neúmyslně způsobit souběžný přístup na stejnou instanci `DbContext`:</span><span class="sxs-lookup"><span data-stu-id="17a6b-152">There are common mistakes that can inadvertently cause concurrent access on the same `DbContext` instance:</span></span>

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a><span data-ttu-id="17a6b-153">Forgetting na čekání na dokončení asynchronní operace před spuštěním jakékoli jiné operace na stejném DbContext</span><span class="sxs-lookup"><span data-stu-id="17a6b-153">Forgetting to await the completion of an asynchronous operation before starting any other operation on the same DbContext</span></span>

<span data-ttu-id="17a6b-154">Asynchronní metody umožňují EF Core iniciovat operace, které přistupují k databázi neblokujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="17a6b-154">Asynchronous methods enable EF Core to initiate operations that access the database in a non-blocking way.</span></span> <span data-ttu-id="17a6b-155">Pokud však volající neočekává dokončení jedné z těchto metod a pokračuje v provádění dalších operací s `DbContext`, stav `DbContext` může být (a pravděpodobně bude) poškozen.</span><span class="sxs-lookup"><span data-stu-id="17a6b-155">But if a caller does not await the completion of one of these methods, and proceeds to perform other operations on the `DbContext`, the state of the `DbContext` can be, (and very likely will be) corrupted.</span></span> 

<span data-ttu-id="17a6b-156">Vždy čekají EF Core asynchronní metody okamžitě.</span><span class="sxs-lookup"><span data-stu-id="17a6b-156">Always await EF Core asynchronous methods immediately.</span></span>  

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a><span data-ttu-id="17a6b-157">Implicitní sdílení instancí DbContext napříč více vlákny prostřednictvím injektáže závislosti</span><span class="sxs-lookup"><span data-stu-id="17a6b-157">Implicitly sharing DbContext instances across multiple threads via dependency injection</span></span>

<span data-ttu-id="17a6b-158">Metoda rozšíření [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) registruje ve výchozím nastavení typy `DbContext` s [vymezenou životností](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) .</span><span class="sxs-lookup"><span data-stu-id="17a6b-158">The [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) extension method registers `DbContext` types with a [scoped lifetime](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) by default.</span></span> 

<span data-ttu-id="17a6b-159">To je bezpečné před souběžným problémům s přístupem v aplikacích ASP.NET Core, protože v daný okamžik probíhá pouze jedno vlákno, které spouští každou žádost klienta, a protože každý požadavek získá samostatný rozsah vkládání závislostí (a proto samostatnou instanci `DbContext`).</span><span class="sxs-lookup"><span data-stu-id="17a6b-159">This is safe from concurrent access issues in ASP.NET Core applications because there is only one thread executing each client request at a given time, and because each request gets a separate dependency injection scope (and therefore a separate `DbContext` instance).</span></span>

<span data-ttu-id="17a6b-160">Nicméně jakýkoli kód, který explicitně spustí více vláken paralelně, musí zajistit, aby se instance `DbContext` nikdy nepoužily současně.</span><span class="sxs-lookup"><span data-stu-id="17a6b-160">However any code that explicitly executes multiple threads in parallel should ensure that `DbContext` instances aren't ever accessed concurrently.</span></span>

<span data-ttu-id="17a6b-161">Pomocí injektáže závislostí lze dosáhnout toho, že buď zaregistrujete kontext jako obor a vytvoříte obory (s použitím `IServiceScopeFactory`) pro každé vlákno, nebo registrací `DbContext` jako přechodné (pomocí přetížení `AddDbContext`, které používá parametr `ServiceLifetime`).</span><span class="sxs-lookup"><span data-stu-id="17a6b-161">Using dependency injection, this can be achieved by either registering the context as scoped and creating scopes (using `IServiceScopeFactory`) for each thread, or by registering the `DbContext` as transient (using the overload of `AddDbContext` which takes a `ServiceLifetime` parameter).</span></span>

## <a name="more-reading"></a><span data-ttu-id="17a6b-162">Další čtení</span><span class="sxs-lookup"><span data-stu-id="17a6b-162">More reading</span></span>

* <span data-ttu-id="17a6b-163">Přečtěte si [Injektáže závislostí](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) , kde se dozvíte víc o používání di.</span><span class="sxs-lookup"><span data-stu-id="17a6b-163">Read [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
* <span data-ttu-id="17a6b-164">Další informace najdete v tématu [testování](testing/index.md) .</span><span class="sxs-lookup"><span data-stu-id="17a6b-164">Read [Testing](testing/index.md) for more information.</span></span>
