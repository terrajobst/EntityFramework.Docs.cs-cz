---
title: Vytváření DbContext při návrhu – EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/16/2019
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: f83d4b16227d114a1cac1514667484a908fea4ac
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197572"
---
<a name="design-time-dbcontext-creation"></a><span data-ttu-id="886eb-102">Vytváření DbContext v době návrhu</span><span class="sxs-lookup"><span data-stu-id="886eb-102">Design-time DbContext Creation</span></span>
==============================
<span data-ttu-id="886eb-103">Některé příkazy EF Core nástrojů (například příkazy [migrace][1] ) vyžadují, aby se v době návrhu vytvořila `DbContext` odvozená instance, aby se shromáždily podrobnosti o typech entit aplikace a jak se mapují ke schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="886eb-103">Some of the EF Core Tools commands (for example, the [Migrations][1] commands) require a derived `DbContext` instance to be created at design time in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="886eb-104">Ve většině případů je žádoucí, aby byl vytvořen `DbContext` tak, aby byl nakonfigurován podobným způsobem, jak by byl [nakonfigurován v době běhu][2].</span><span class="sxs-lookup"><span data-stu-id="886eb-104">In most cases, it is desirable that the `DbContext` thereby created is configured in a similar way to how it would be [configured at run time][2].</span></span>

<span data-ttu-id="886eb-105">Existují různé způsoby, jak se nástroje pokoušejí vytvořit `DbContext`:</span><span class="sxs-lookup"><span data-stu-id="886eb-105">There are various ways the tools try to create the `DbContext`:</span></span>

<a name="from-application-services"></a><span data-ttu-id="886eb-106">Z aplikačních služeb</span><span class="sxs-lookup"><span data-stu-id="886eb-106">From application services</span></span>
-------------------------
<span data-ttu-id="886eb-107">Pokud váš projekt po spuštění používá [webového hostitele ASP.NET Core][3] nebo [obecného hostitele .NET Core][4], nástroj se pokusí získat objekt DbContext od poskytovatele služeb aplikace.</span><span class="sxs-lookup"><span data-stu-id="886eb-107">If your startup project uses the [ASP.NET Core Web Host][3] or [.NET Core Generic Host][4], the tools try to obtain the DbContext object from the application's service provider.</span></span>

<span data-ttu-id="886eb-108">Nástroje se nejdřív pokusí získat poskytovatele `Program.CreateHostBuilder()`služby vyvoláním, voláním `Build()`a přístupem `Services` k vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="886eb-108">The tools first try to obtain the service provider by invoking `Program.CreateHostBuilder()`, calling `Build()`, then accessing the `Services` property.</span></span>

``` csharp
public class Program
{
    public static void Main(string[] args)
        => CreateHostBuilder(args).Build().Run();

    // EF Core uses this method at design time to access the DbContext
    public static IHostBuilder CreateHostBuilder(string[] args)
        => Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(
                webBuilder => webBuilder.UseStartup<Startup>());
}

public class Startup
{
    public void ConfigureServices(IServiceCollection services)
        => services.AddDbContext<ApplicationDbContext>();
}

public class ApplicationDbContext : DbContext
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

> [!NOTE]
> <span data-ttu-id="886eb-109">Při vytváření nové aplikace ASP.NET Core je tento zavěšení součástí standardně.</span><span class="sxs-lookup"><span data-stu-id="886eb-109">When you create a new ASP.NET Core application, this hook is included by default.</span></span>

<span data-ttu-id="886eb-110">`DbContext` Samotný a všechny závislosti v konstruktoru musí být registrovány jako služby v poskytovateli služeb aplikace.</span><span class="sxs-lookup"><span data-stu-id="886eb-110">The `DbContext` itself and any dependencies in its constructor need to be registered as services in the application's service provider.</span></span> <span data-ttu-id="886eb-111">To lze snadno dosáhnout tím [ `DbContext` , že máte konstruktor na objektu, který převezme instanci `DbContextOptions<TContext>` ][5] [ `AddDbContext<TContext>` ][6]jako argument a pomocí metody.</span><span class="sxs-lookup"><span data-stu-id="886eb-111">This can be easily achieved by having [a constructor on the `DbContext` that takes an instance of `DbContextOptions<TContext>` as an argument][5] and using the [`AddDbContext<TContext>` method][6].</span></span>

<a name="using-a-constructor-with-no-parameters"></a><span data-ttu-id="886eb-112">Použití konstruktoru bez parametrů</span><span class="sxs-lookup"><span data-stu-id="886eb-112">Using a constructor with no parameters</span></span>
--------------------------------------
<span data-ttu-id="886eb-113">Pokud DbContext nelze získat od poskytovatele služby Application Service, nástroje hledají odvozený `DbContext` typ v rámci projektu.</span><span class="sxs-lookup"><span data-stu-id="886eb-113">If the DbContext can't be obtained from the application service provider, the tools look for the derived `DbContext` type inside the project.</span></span> <span data-ttu-id="886eb-114">Pak se pokusí vytvořit instanci pomocí konstruktoru bez parametrů.</span><span class="sxs-lookup"><span data-stu-id="886eb-114">Then they try to create an instance using a constructor with no parameters.</span></span> <span data-ttu-id="886eb-115">To může být výchozí konstruktor, pokud `DbContext` je nakonfigurován [`OnConfiguring`][7] pomocí metody.</span><span class="sxs-lookup"><span data-stu-id="886eb-115">This can be the default constructor if the `DbContext` is configured using the [`OnConfiguring`][7] method.</span></span>

<a name="from-a-design-time-factory"></a><span data-ttu-id="886eb-116">Z továrny v době návrhu</span><span class="sxs-lookup"><span data-stu-id="886eb-116">From a design-time factory</span></span>
--------------------------
<span data-ttu-id="886eb-117">Můžete také sdělit nástroje, jak vytvořit DbContext implementací `IDesignTimeDbContextFactory<TContext>` rozhraní: Pokud třída implementující toto rozhraní je nalezena v jednom projektu jako odvozená `DbContext` nebo v projektu po spuštění aplikace, nástroje vycházejí z dalších způsobů vytvoření DbContext a místo toho používají továrnu v době návrhu.</span><span class="sxs-lookup"><span data-stu-id="886eb-117">You can also tell the tools how to create your DbContext by implementing the `IDesignTimeDbContextFactory<TContext>` interface: If a class implementing this interface is found in either the same project as the derived `DbContext` or in the application's startup project, the tools bypass the other ways of creating the DbContext and use the design-time factory instead.</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Design;
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

> [!NOTE]
> <span data-ttu-id="886eb-118">`args` Parametr je aktuálně nepoužit.</span><span class="sxs-lookup"><span data-stu-id="886eb-118">The `args` parameter is currently unused.</span></span> <span data-ttu-id="886eb-119">Existuje [problém][8] sledující možnost zadat argumenty pro dobu návrhu z nástrojů.</span><span class="sxs-lookup"><span data-stu-id="886eb-119">There is [an issue][8] tracking the ability to specify design-time arguments from the tools.</span></span>

<span data-ttu-id="886eb-120">Továrna v době návrhu může být obzvláště užitečná, pokud je třeba nakonfigurovat DbContext jinak než v době návrhu, a to v případě, že `DbContext` konstruktor přebírá další parametry nejsou registrovány v di, pokud nepoužíváte di, nebo pokud pro některé důvod, proč nemusíte mít `BuildWebHost` metodu ve `Main` třídě ASP.NET Core vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="886eb-120">A design-time factory can be especially useful if you need to configure the DbContext differently for design time than at run time, if the `DbContext` constructor takes additional parameters are not registered in DI, if you are not using DI at all, or if for some reason you prefer not to have a `BuildWebHost` method in your ASP.NET Core application's `Main` class.</span></span>

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: /aspnet/core/fundamentals/host/web-host
  [4]: /aspnet/core/fundamentals/host/generic-host
  [5]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [6]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [7]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [8]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
