---
title: "Konfigurace DbContext - EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
ms.technology: entity-framework-core
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 96abf3b94be3e1d19f833644f1c2f6f13fe0e730
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/05/2017
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="7fb96-102">Konfigurace DbContext</span><span class="sxs-lookup"><span data-stu-id="7fb96-102">Configuring a DbContext</span></span>

<span data-ttu-id="7fb96-103">Tento článek popisuje vzory pro konfiguraci `DbContext` s `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="7fb96-103">This article shows patterns for configuring a `DbContext` with `DbContextOptions`.</span></span> <span data-ttu-id="7fb96-104">Možnosti se primárně používají konfigurace úložiště dat a vyberte.</span><span class="sxs-lookup"><span data-stu-id="7fb96-104">Options are primarily used to select and configure the data store.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="7fb96-105">Konfigurace DbContextOptions</span><span class="sxs-lookup"><span data-stu-id="7fb96-105">Configuring DbContextOptions</span></span>

<span data-ttu-id="7fb96-106">`DbContext`musíte mít instanci `DbContextOptions` provádění.</span><span class="sxs-lookup"><span data-stu-id="7fb96-106">`DbContext` must have an instance of `DbContextOptions` in order to execute.</span></span> <span data-ttu-id="7fb96-107">To se dá nakonfigurovat přepsání `OnConfiguring`, nebo zadaná externě prostřednictvím argument konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="7fb96-107">This can be configured by overriding `OnConfiguring`, or supplied externally via a constructor argument.</span></span>

<span data-ttu-id="7fb96-108">Pokud se používá, `OnConfiguring` se spouští na zadaný možnosti, což znamená, je doplňkové a můžete je zadaný pro argument konstruktoru možnosti přepsání.</span><span class="sxs-lookup"><span data-stu-id="7fb96-108">If both are used, `OnConfiguring` is executed on the supplied options, meaning it is additive and can overwrite  options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="7fb96-109">Argument konstruktoru</span><span class="sxs-lookup"><span data-stu-id="7fb96-109">Constructor argument</span></span>

<span data-ttu-id="7fb96-110">Kontext kódu pomocí konstruktoru</span><span class="sxs-lookup"><span data-stu-id="7fb96-110">Context code with constructor</span></span>

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
> <span data-ttu-id="7fb96-111">Základní konstruktoru DbContext také přijímá neobecnou verzi `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="7fb96-111">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`.</span></span> <span data-ttu-id="7fb96-112">Pro aplikace s více typy kontextu se nedoporučuje používat na neobecnou verzi.</span><span class="sxs-lookup"><span data-stu-id="7fb96-112">Using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="7fb96-113">Kód aplikace k chybě při inicializaci z argument konstruktoru</span><span class="sxs-lookup"><span data-stu-id="7fb96-113">Application code to initialize from constructor argument</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="7fb96-114">OnConfiguring</span><span class="sxs-lookup"><span data-stu-id="7fb96-114">OnConfiguring</span></span>

> [!WARNING]  
> <span data-ttu-id="7fb96-115">`OnConfiguring`Probíhá poslední a můžete přepsat možnosti získaný od DI nebo konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="7fb96-115">`OnConfiguring` occurs last and can overwrite options obtained from DI or the constructor.</span></span> <span data-ttu-id="7fb96-116">Tento přístup není jít o testování (Pokud cílíte databázi úplná).</span><span class="sxs-lookup"><span data-stu-id="7fb96-116">This approach does not lend itself to testing (unless you target the full database).</span></span>

<span data-ttu-id="7fb96-117">Kontext kódu s použitím `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="7fb96-117">Context code with `OnConfiguring`:</span></span>

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

<span data-ttu-id="7fb96-118">Kód aplikace se nezdařila s `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="7fb96-118">Application code to initialize with `OnConfiguring`:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

## <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="7fb96-119">DbContext pomocí vkládání závislostí</span><span class="sxs-lookup"><span data-stu-id="7fb96-119">Using DbContext with dependency injection</span></span>

<span data-ttu-id="7fb96-120">EF podporuje používání `DbContext` s kontejner vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="7fb96-120">EF supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="7fb96-121">Váš typ DbContext můžete přidat do kontejneru služby pomocí `AddDbContext<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="7fb96-121">Your DbContext type can be added to the service container by using `AddDbContext<TContext>`.</span></span>

<span data-ttu-id="7fb96-122">`AddDbContext`budou obě váš typ DbContext, `TContext`, a `DbContextOptions<TContext>` k dispozici pro vkládání z kontejneru služby.</span><span class="sxs-lookup"><span data-stu-id="7fb96-122">`AddDbContext` will make both your DbContext type, `TContext`, and `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="7fb96-123">V tématu [další čtení](#more-reading) níže informace o vkládání závislostí.</span><span class="sxs-lookup"><span data-stu-id="7fb96-123">See [more reading](#more-reading) below for information on dependency injection.</span></span>

<span data-ttu-id="7fb96-124">Přidání dbcontext k vkládání závislostí</span><span class="sxs-lookup"><span data-stu-id="7fb96-124">Adding dbcontext to dependency injection</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="7fb96-125">To vyžaduje, přidávání [argument konstruktoru](#constructor-argument) pro váš typ DbContext, který přijímá `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="7fb96-125">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions`.</span></span>

<span data-ttu-id="7fb96-126">Kontext kód:</span><span class="sxs-lookup"><span data-stu-id="7fb96-126">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="7fb96-127">Kód aplikace (v ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="7fb96-127">Application code (in ASP.NET Core):</span></span>

``` csharp
public MyController(BloggingContext context)
```

<span data-ttu-id="7fb96-128">Kód aplikace (pomocí položku ServiceProvider přímo, méně běžné):</span><span class="sxs-lookup"><span data-stu-id="7fb96-128">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="using-idesigntimedbcontextfactorytcontext"></a><span data-ttu-id="7fb96-129">Pomocí`IDesignTimeDbContextFactory<TContext>`</span><span class="sxs-lookup"><span data-stu-id="7fb96-129">Using `IDesignTimeDbContextFactory<TContext>`</span></span>

<span data-ttu-id="7fb96-130">Jako alternativu k výše uvedených možností, může taky poskytnout implementaci `IDesignTimeDbContextFactory<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="7fb96-130">As an alternative to the options above, you may also provide an implementation of `IDesignTimeDbContextFactory<TContext>`.</span></span> <span data-ttu-id="7fb96-131">EF nástroje můžete použít k vytvoření instance vaší DbContext tento objekt pro vytváření.</span><span class="sxs-lookup"><span data-stu-id="7fb96-131">EF tools can use this factory to create an instance of your DbContext.</span></span> <span data-ttu-id="7fb96-132">To může být potřeba Chcete-li povolit konkrétní návrhu činnosti, jako je například migrace.</span><span class="sxs-lookup"><span data-stu-id="7fb96-132">This may be required in order to enable specific design-time experiences such as migrations.</span></span>

<span data-ttu-id="7fb96-133">Implementace tohoto rozhraní povolit návrhu služby pro kontextové typy, které nemají výchozí veřejný konstruktor.</span><span class="sxs-lookup"><span data-stu-id="7fb96-133">Implement this interface to enable design-time services for context types that do not have a public default constructor.</span></span> <span data-ttu-id="7fb96-134">Implementace tohoto rozhraní, které jsou ve stejném sestavení jako kontext odvozené automaticky zjistí použití návrhu služby.</span><span class="sxs-lookup"><span data-stu-id="7fb96-134">Design-time services will automatically discover implementations of this interface that are in the same assembly as the derived context.</span></span>

<span data-ttu-id="7fb96-135">Příklad:</span><span class="sxs-lookup"><span data-stu-id="7fb96-135">Example:</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Infrastructure;

namespace MyProject
{
    public class BloggingContextFactory : IDesignTimeDbContextFactory<BloggingContext>
    {
        public BloggingContext CreateDbContext(string[] args)
        {
            var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
            optionsBuilder.UseSqlite("Data Source=blog.db");

            return new BloggingContext(optionsBuilder.Options);
        }
    }
}
```

## <a name="more-reading"></a><span data-ttu-id="7fb96-136">Další čtení</span><span class="sxs-lookup"><span data-stu-id="7fb96-136">More reading</span></span>

* <span data-ttu-id="7fb96-137">Čtení [Začínáme na ASP.NET Core](../get-started/aspnetcore/index.md) Další informace o používání EF s ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7fb96-137">Read [Getting Started on ASP.NET Core](../get-started/aspnetcore/index.md) for more information on using EF with ASP.NET Core.</span></span>
* <span data-ttu-id="7fb96-138">Čtení [vkládání závislostí](https://docs.asp.net/en/latest/fundamentals/dependency-injection.html) Další informace o používání DI.</span><span class="sxs-lookup"><span data-stu-id="7fb96-138">Read [Dependency Injection](https://docs.asp.net/en/latest/fundamentals/dependency-injection.html) to learn more about using DI.</span></span>
* <span data-ttu-id="7fb96-139">Čtení [testování](testing/index.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="7fb96-139">Read [Testing](testing/index.md) for more information.</span></span>
