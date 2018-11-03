---
title: Konfigurace DbContext – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: f5a9ae17471391442170d8c40264e4db6922cb08
ms.sourcegitcommit: 39080d38e1adea90db741257e60dc0e7ed08aa82
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/03/2018
ms.locfileid: "50979999"
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="b1247-102">Konfigurace DbContext</span><span class="sxs-lookup"><span data-stu-id="b1247-102">Configuring a DbContext</span></span>

<span data-ttu-id="b1247-103">Tento článek ukazuje základní vzorce pro konfiguraci `DbContext` prostřednictvím `DbContextOptions` pro připojení k databázi pomocí konkrétního zprostředkovatele EF Core a volitelné chování.</span><span class="sxs-lookup"><span data-stu-id="b1247-103">This article shows basic patterns for configuring a `DbContext` via a `DbContextOptions` to connect to a database using a specific EF Core provider and optional behaviors.</span></span>

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="b1247-104">Konfigurace DbContext v době návrhu</span><span class="sxs-lookup"><span data-stu-id="b1247-104">Design-time DbContext configuration</span></span>

<span data-ttu-id="b1247-105">EF Core návrhových nástrojů, jako je například [migrace](xref:core/managing-schemas/migrations/index) musí mít možnost zjistit a vytvořit instanci pracovní `DbContext` aby bylo možné shromažďovat údaje o typy entit a jak jsou mapovány na schéma databáze vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="b1247-105">EF Core design-time tools such as [migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="b1247-106">Tento proces může být automatické, tak dlouho, dokud nástroj můžete snadno vytvářet `DbContext` tak, že bude nakonfigurován podobně jak by se nakonfigurovat v době běhu.</span><span class="sxs-lookup"><span data-stu-id="b1247-106">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at run-time.</span></span>

<span data-ttu-id="b1247-107">Při jakékoli vzor, který obsahuje informace související s konfigurací, které `DbContext` můžete pracovat v době běhu, nástroje, které vyžadují použití `DbContext` v době návrhu lze pracovat pouze s omezený počet vzorů.</span><span class="sxs-lookup"><span data-stu-id="b1247-107">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="b1247-108">Ty jsou podrobně popsané v další v [kontextu vytváření návrhu](xref:core/miscellaneous/cli/dbcontext-creation) oddílu.</span><span class="sxs-lookup"><span data-stu-id="b1247-108">These are covered in more detail in the [Design-Time Context Creation](xref:core/miscellaneous/cli/dbcontext-creation) section.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="b1247-109">Konfigurace DbContextOptions</span><span class="sxs-lookup"><span data-stu-id="b1247-109">Configuring DbContextOptions</span></span>

<span data-ttu-id="b1247-110">`DbContext` musíte mít instanci `DbContextOptions` aby bylo možné provést žádnou práci.</span><span class="sxs-lookup"><span data-stu-id="b1247-110">`DbContext` must have an instance of `DbContextOptions` in order to perform any work.</span></span> <span data-ttu-id="b1247-111">`DbContextOptions` Instance přenáší informace o konfiguraci, jako:</span><span class="sxs-lookup"><span data-stu-id="b1247-111">The `DbContextOptions` instance carries configuration information such as:</span></span>

- <span data-ttu-id="b1247-112">Poskytovatel databáze, které chcete použít, je obvykle vybrána vyvoláním metody `UseSqlServer` nebo `UseSqlite`.</span><span class="sxs-lookup"><span data-stu-id="b1247-112">The database provider to use, typically selected by invoking a method such as `UseSqlServer` or `UseSqlite`.</span></span> <span data-ttu-id="b1247-113">Tyto rozšiřující metody vyžadují odpovídající balíček zprostředkovatele, jako například `Microsoft.EntityFrameworkCore.SqlServer` nebo `Microsoft.EntityFrameworkCore.Sqlite`.</span><span class="sxs-lookup"><span data-stu-id="b1247-113">These extension methods require the corresponding provider package, such as `Microsoft.EntityFrameworkCore.SqlServer` or `Microsoft.EntityFrameworkCore.Sqlite`.</span></span> <span data-ttu-id="b1247-114">Metody jsou definovány v `Microsoft.EntityFrameworkCore` oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="b1247-114">The methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span>
- <span data-ttu-id="b1247-115">Všechny nezbytné připojovací řetězec nebo identifikátor instance databáze obvykle předat jako argument výše popsané metody výběru zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="b1247-115">Any necessary connection string or identifier of the database instance, typically passed as an argument to the provider selection method mentioned above</span></span>
- <span data-ttu-id="b1247-116">Libovolné volitelné chování zprostředkovatele úroveň selektory, obvykle také zřetězené uvnitř volání metody výběru zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="b1247-116">Any provider-level optional behavior selectors, typically also chained inside the call to the provider selection method</span></span>
- <span data-ttu-id="b1247-117">Žádné obecné EF Core chování selektory, obvykle zřetězené po nebo před voláním metody výběru zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="b1247-117">Any general EF Core behavior selectors, typically chained after or before the provider selector method</span></span>

<span data-ttu-id="b1247-118">Následující příklad nastaví `DbContextOptions` použití zprostředkovatele SQL Server, součástí připojení `connectionString` proměnnou, časový limit příkazu úrovni zprostředkovatele a výběr chování EF Core, která provádí všechny dotazy spouštěné v `DbContext` [bez sledování](xref:core/querying/tracking#no-tracking-queries) ve výchozím nastavení:</span><span class="sxs-lookup"><span data-stu-id="b1247-118">The following example configures the `DbContextOptions` to use the SQL Server provider, a connection contained in the `connectionString` variable, a provider-level command timeout, and an EF Core behavior selector that makes all queries executed in the `DbContext` [no-tracking](xref:core/querying/tracking#no-tracking-queries) by default:</span></span>

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> <span data-ttu-id="b1247-119">Výběr metody poskytovatele a jiné chování metody selektor uvedených výše jsou metody rozšíření na `DbContextOptions` nebo třídy možností specifickým pro zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="b1247-119">Provider selector methods and other behavior selector methods mentioned above are extension methods on `DbContextOptions` or provider-specific option classes.</span></span> <span data-ttu-id="b1247-120">Pokud chcete mít přístup k tyto rozšiřující metody, budete muset máte obor názvů (obvykle `Microsoft.EntityFrameworkCore`) v oboru a zahrnout další balíčky závislostí do projektu.</span><span class="sxs-lookup"><span data-stu-id="b1247-120">In order to have access to these extension methods you may need to have a namespace (typically `Microsoft.EntityFrameworkCore`) in scope and include additional package dependencies in the project.</span></span>

<span data-ttu-id="b1247-121">`DbContextOptions` Mohou být poskytnuty `DbContext` tak, že přepíšete `OnConfiguring` metoda nebo externě přes argument konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="b1247-121">The `DbContextOptions` can be supplied to the `DbContext` by overriding the `OnConfiguring` method or externally via a constructor argument.</span></span>

<span data-ttu-id="b1247-122">Pokud se oba používají `OnConfiguring` je použito jako poslední a později jej můžete přepsat možnosti zadaný pro argument konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="b1247-122">If both are used, `OnConfiguring` is applied last and can overwrite options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="b1247-123">Argument konstruktoru</span><span class="sxs-lookup"><span data-stu-id="b1247-123">Constructor argument</span></span>

<span data-ttu-id="b1247-124">Kontext kódu pomocí konstruktoru:</span><span class="sxs-lookup"><span data-stu-id="b1247-124">Context code with constructor:</span></span>

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
> <span data-ttu-id="b1247-125">Konstruktor základní třídy DbContext přijímá také obecné verzi `DbContextOptions`, ale pro aplikace s více typy kontextu není doporučeno používat obecné verze.</span><span class="sxs-lookup"><span data-stu-id="b1247-125">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`, but using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="b1247-126">Aplikace kód pro inicializaci z argumentu konstruktoru:</span><span class="sxs-lookup"><span data-stu-id="b1247-126">Application code to initialize from constructor argument:</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="b1247-127">OnConfiguring</span><span class="sxs-lookup"><span data-stu-id="b1247-127">OnConfiguring</span></span>

<span data-ttu-id="b1247-128">Kontext kódu pomocí `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="b1247-128">Context code with `OnConfiguring`:</span></span>

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

<span data-ttu-id="b1247-129">Aplikace kód pro inicializaci `DbContext` , která používá `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="b1247-129">Application code to initialize a `DbContext` that uses `OnConfiguring`:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> <span data-ttu-id="b1247-130">Tento přístup se nepropůjčuje s testováním, není-li testy používat úplnou databázi.</span><span class="sxs-lookup"><span data-stu-id="b1247-130">This approach does not lend itself to testing, unless the tests target the full database.</span></span>

### <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="b1247-131">Pomocí vkládání závislostí DbContext</span><span class="sxs-lookup"><span data-stu-id="b1247-131">Using DbContext with dependency injection</span></span>

<span data-ttu-id="b1247-132">EF Core podporuje používání `DbContext` s kontejner vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="b1247-132">EF Core supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="b1247-133">Váš typ DbContext můžete přidat do kontejneru služby s použitím `AddDbContext<TContext>` metody.</span><span class="sxs-lookup"><span data-stu-id="b1247-133">Your DbContext type can be added to the service container by using the `AddDbContext<TContext>` method.</span></span>

<span data-ttu-id="b1247-134">`AddDbContext<TContext>` provede oba váš typ DbContext `TContext`a odpovídající `DbContextOptions<TContext>` k dispozici pro vkládání z kontejneru služeb.</span><span class="sxs-lookup"><span data-stu-id="b1247-134">`AddDbContext<TContext>` will make both your DbContext type, `TContext`, and the corresponding `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="b1247-135">Zobrazit [další čtení](#more-reading) níže pro další informace o vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="b1247-135">See [more reading](#more-reading) below for additional information on dependency injection.</span></span>

<span data-ttu-id="b1247-136">Přidávání `Dbcontext` pro vkládání závislostí:</span><span class="sxs-lookup"><span data-stu-id="b1247-136">Adding the `Dbcontext` to dependency injection:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="b1247-137">K tomu je potřeba přidat [argument konstruktoru](#constructor-argument) do typu DbContext, který přijímá `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="b1247-137">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions<TContext>`.</span></span>

<span data-ttu-id="b1247-138">Kontext kódu:</span><span class="sxs-lookup"><span data-stu-id="b1247-138">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="b1247-139">Kód aplikace (v ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="b1247-139">Application code (in ASP.NET Core):</span></span>

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

<span data-ttu-id="b1247-140">Kód aplikace (pomocí ServiceProvider přímo, méně běžné):</span><span class="sxs-lookup"><span data-stu-id="b1247-140">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="more-reading"></a><span data-ttu-id="b1247-141">Další čtení</span><span class="sxs-lookup"><span data-stu-id="b1247-141">More reading</span></span>

* <span data-ttu-id="b1247-142">Čtení [Začínáme v ASP.NET Core](../get-started/aspnetcore/index.md) Další informace o používání EF s ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b1247-142">Read [Getting Started on ASP.NET Core](../get-started/aspnetcore/index.md) for more information on using EF with ASP.NET Core.</span></span>
* <span data-ttu-id="b1247-143">Čtení [injektáž závislostí](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) Další informace o používání DI.</span><span class="sxs-lookup"><span data-stu-id="b1247-143">Read [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
* <span data-ttu-id="b1247-144">Čtení [testování](testing/index.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="b1247-144">Read [Testing](testing/index.md) for more information.</span></span>
