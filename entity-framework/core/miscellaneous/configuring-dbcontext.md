---
title: "Konfigurace DbContext - EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
ms.technology: entity-framework-core
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: de26e3b28851d4dc4e50f0490093dd05ad489b31
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/22/2017
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="9c2c6-102">Konfigurace DbContext</span><span class="sxs-lookup"><span data-stu-id="9c2c6-102">Configuring a DbContext</span></span>

<span data-ttu-id="9c2c6-103">Tento článek ukazuje základní vzory pro konfiguraci `DbContext` prostřednictvím `DbContextOptions` pro připojení k databázi pomocí konkrétního zprostředkovatele EF jádra a volitelné chování.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-103">This article shows basic patterns for configuring a `DbContext` via a `DbContextOptions` to connect to a database using a specific EF Core provider and optional behaviors.</span></span>

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="9c2c6-104">Konfigurace DbContext během návrhu</span><span class="sxs-lookup"><span data-stu-id="9c2c6-104">Design-time DbContext configuration</span></span>

<span data-ttu-id="9c2c6-105">EF základní návrhu nástroje jako [migrace](xref:core/managing-schemas/migrations/index) musí být schopni zjistit a vytvořit funkční instanci `DbContext` typu, aby bylo možné shromáždit informace o typy entit a jak jsou mapovány na schéma databáze aplikace.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-105">EF Core design-time tools such as [migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="9c2c6-106">Tento proces může být automatická, tak dlouho, dokud tento nástroj můžete snadno vytvořit `DbContext` tak, že bude nakonfigurován podobně jak by se nakonfigurovat v o době.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-106">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at runt-time.</span></span>

<span data-ttu-id="9c2c6-107">Při jakékoli vzor, který obsahuje informace nezbytné konfigurace, které `DbContext` můžete pracovat v době spuštění, nástroje, které vyžadují použití `DbContext` v době návrhu můžete pracovat pouze s omezený počet vzory.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-107">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="9c2c6-108">Ty jsou popsané v podrobněji [vytvoření kontextu návrhu](xref:core/miscellaneous/cli/dbcontext-creation) části.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-108">These are covered in more detail in the [Design-Time Context Creation](xref:core/miscellaneous/cli/dbcontext-creation) section.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="9c2c6-109">Konfigurace DbContextOptions</span><span class="sxs-lookup"><span data-stu-id="9c2c6-109">Configuring DbContextOptions</span></span>

<span data-ttu-id="9c2c6-110">`DbContext`musíte mít instanci `DbContextOptions` za účelem provedení veškerou práci.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-110">`DbContext` must have an instance of `DbContextOptions` in order to perform any work.</span></span> <span data-ttu-id="9c2c6-111">`DbContextOptions` Instance představuje informace o konfiguraci, jako například:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-111">The `DbContextOptions` instance carries configuration information such as:</span></span>

- <span data-ttu-id="9c2c6-112">Zprostředkovatel databáze, které chcete použít, je obvykle vybrána vyvoláním metody `UseSqlServer` nebo`UseSqlite`</span><span class="sxs-lookup"><span data-stu-id="9c2c6-112">The database provider to use, typically selected by invoking a method such as `UseSqlServer` or `UseSqlite`</span></span>
- <span data-ttu-id="9c2c6-113">Všechny nezbytné připojovací řetězec nebo identifikátor instance databáze obvykle předat jako argument výše popsané metody výběru zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="9c2c6-113">Any necessary connection string or identifier of the database instance, typically passed as an argument to the provider selection method mentioned above</span></span>
- <span data-ttu-id="9c2c6-114">Budete potřebovat volitelné chování zprostředkovatele úrovni, obvykle se také zřetězené uvnitř volání metody výběru zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="9c2c6-114">Any provider-level optional behavior selectors, typically also chained inside the call to the provider selection method</span></span>
- <span data-ttu-id="9c2c6-115">Obecné základní EF chování budete potřebovat, obvykle zřetězené po nebo před voláním metody selektor zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="9c2c6-115">Any general EF Core behavior selectors, typically chained after or before the provider selector method</span></span>

<span data-ttu-id="9c2c6-116">Následující příklad konfiguruje `DbContextOptions` použití zprostředkovatele SQL Server, připojení součástí `connectionString` proměnné, vypršení časového limitu příkaz úrovni zprostředkovatele a selektor chování EF jádra, která umožňuje všechny dotazy spouštěné v `DbContext` [ne sledování](xref:core/querying/tracking#no-tracking-queries) ve výchozím nastavení:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-116">The following example configures the `DbContextOptions` to use the SQL Server provider, a connection contained in the `connectionString` variable, a provider-level command timeout, and an EF Core behavior selector that makes all queries executed in the `DbContext` [no-tracking](xref:core/querying/tracking#no-tracking-queries) by default:</span></span>

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> <span data-ttu-id="9c2c6-117">Zprostředkovatel pro výběr metody a jiné chování pro výběr metody uvedené výše jsou rozšiřující metody na `DbContextOptions` nebo třídy možnost specifický pro zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-117">Provider selector methods and other behavior selector methods mentioned above are extension methods on `DbContextOptions` or provider-specific option classes.</span></span> <span data-ttu-id="9c2c6-118">Chcete-li mít přístup k tyto rozšiřující metody, budete muset obor názvů (obvykle `Microsoft.EntityFrameworkCore`) v oboru a obsahovat závislosti, další balíčky v projektu.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-118">In order to have access to these extension methods you may need to have a namespace (typically `Microsoft.EntityFrameworkCore`) in scope and include additional package dependencies in the project.</span></span>

<span data-ttu-id="9c2c6-119">`DbContextOptions` Mohou být poskytnuty `DbContext` přepsáním `OnConfiguring` metoda nebo externě prostřednictvím argument konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-119">The `DbContextOptions` can be supplied to the `DbContext` by overriding the `OnConfiguring` method or externally via a constructor argument.</span></span>

<span data-ttu-id="9c2c6-120">Pokud se používá, `OnConfiguring` je použito jako poslední a můžete přepsat možnosti zadaný pro argument konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-120">If both are used, `OnConfiguring` is applied last and can overwrite options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="9c2c6-121">Argument konstruktoru</span><span class="sxs-lookup"><span data-stu-id="9c2c6-121">Constructor argument</span></span>

<span data-ttu-id="9c2c6-122">Kontext kód s konstruktorem:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-122">Context code with constructor:</span></span>

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
> <span data-ttu-id="9c2c6-123">Základní konstruktoru DbContext také přijímá neobecnou verzi `DbContextOptions`, ale pro aplikace s více typy kontextu se nedoporučuje používat na neobecnou verzi.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-123">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`, but using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="9c2c6-124">Kód aplikace k chybě při inicializaci z argument konstruktoru:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-124">Application code to initialize from constructor argument:</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="9c2c6-125">OnConfiguring</span><span class="sxs-lookup"><span data-stu-id="9c2c6-125">OnConfiguring</span></span>

<span data-ttu-id="9c2c6-126">Kontext kódu s použitím `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-126">Context code with `OnConfiguring`:</span></span>

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

<span data-ttu-id="9c2c6-127">Kód aplikace k chybě při inicializaci `DbContext` používající `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-127">Application code to initialize a `DbContext` that uses `OnConfiguring`:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> <span data-ttu-id="9c2c6-128">Tento přístup není jít o testování, pokud testy používat úplnou databázi.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-128">This approach does not lend itself to testing, unless the tests target the full database.</span></span>

### <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="9c2c6-129">DbContext pomocí vkládání závislostí</span><span class="sxs-lookup"><span data-stu-id="9c2c6-129">Using DbContext with dependency injection</span></span>

<span data-ttu-id="9c2c6-130">Základní EF podporuje používání `DbContext` s kontejner vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-130">EF Core supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="9c2c6-131">Váš typ DbContext můžete přidat do kontejneru služby pomocí `AddDbContext<TContext>` metoda.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-131">Your DbContext type can be added to the service container by using the `AddDbContext<TContext>` method.</span></span>

<span data-ttu-id="9c2c6-132">`AddDbContext<TContext>`budou obě váš typ DbContext, `TContext`a odpovídající `DbContextOptions<TContext>` k dispozici pro vkládání z kontejneru služby.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-132">`AddDbContext<TContext>` will make both your DbContext type, `TContext`, and the corresponding `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="9c2c6-133">V tématu [další čtení](#more-reading) níže Další informace o vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-133">See [more reading](#more-reading) below for additional information on dependency injection.</span></span>

<span data-ttu-id="9c2c6-134">Přidávání `Dbcontext` k vkládání závislostí:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-134">Adding the `Dbcontext` to dependency injection:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="9c2c6-135">To vyžaduje, přidávání [argument konstruktoru](#constructor-argument) pro váš typ DbContext, který přijímá `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-135">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions<TContext>`.</span></span>

<span data-ttu-id="9c2c6-136">Kontext kód:</span><span class="sxs-lookup"><span data-stu-id="9c2c6-136">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="9c2c6-137">Kód aplikace (v ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="9c2c6-137">Application code (in ASP.NET Core):</span></span>

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

<span data-ttu-id="9c2c6-138">Kód aplikace (pomocí položku ServiceProvider přímo, méně běžné):</span><span class="sxs-lookup"><span data-stu-id="9c2c6-138">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="more-reading"></a><span data-ttu-id="9c2c6-139">Další čtení</span><span class="sxs-lookup"><span data-stu-id="9c2c6-139">More reading</span></span>

* <span data-ttu-id="9c2c6-140">Čtení [Začínáme na ASP.NET Core](../get-started/aspnetcore/index.md) Další informace o používání EF s ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-140">Read [Getting Started on ASP.NET Core](../get-started/aspnetcore/index.md) for more information on using EF with ASP.NET Core.</span></span>
* <span data-ttu-id="9c2c6-141">Čtení [vkládání závislostí](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) Další informace o používání DI.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-141">Read [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
* <span data-ttu-id="9c2c6-142">Čtení [testování](testing/index.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="9c2c6-142">Read [Testing](testing/index.md) for more information.</span></span>
