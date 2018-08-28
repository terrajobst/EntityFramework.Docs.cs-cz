---
title: Vytváření DbContext v době návrhu – EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: 66fec7605b6ac2da0af1e801f8a1dca0789aea35
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993715"
---
<a name="design-time-dbcontext-creation"></a><span data-ttu-id="888a2-102">Vytváření DbContext v době návrhu</span><span class="sxs-lookup"><span data-stu-id="888a2-102">Design-time DbContext Creation</span></span>
==============================
<span data-ttu-id="888a2-103">Některé příkazy EF Core Tools (například [migrace] [ 1] příkazy) vyžadují odvozené `DbContext` instance má být vytvořen v době návrhu, aby bylo možné získat podrobnosti o vaší aplikace typy entit a jak jsou mapovány na schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="888a2-103">Some of the EF Core Tools commands (for example, the [Migrations][1] commands) require a derived `DbContext` instance to be created at design time in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="888a2-104">Ve většině případů je žádoucí, který `DbContext` tím vytvořený nakonfigurovat podobným způsobem, jak by být [nakonfigurované v době běhu][2].</span><span class="sxs-lookup"><span data-stu-id="888a2-104">In most cases, it is desirable that the `DbContext` thereby created is configured in a similar way to how it would be [configured at run time][2].</span></span>

<span data-ttu-id="888a2-105">Existují různé způsoby nástroje pokusu o vytvoření `DbContext`:</span><span class="sxs-lookup"><span data-stu-id="888a2-105">There are various ways the tools try to create the `DbContext`:</span></span>

<a name="from-application-services"></a><span data-ttu-id="888a2-106">Z aplikačních služeb</span><span class="sxs-lookup"><span data-stu-id="888a2-106">From application services</span></span>
-------------------------
<span data-ttu-id="888a2-107">Pokud je váš projekt po spuštění aplikace ASP.NET Core, nástroje, pokuste se získat objekt DbContext z vaší aplikace poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="888a2-107">If your startup project is an ASP.NET Core app, the tools try to obtain the DbContext object from the application's service provider.</span></span>

<span data-ttu-id="888a2-108">Nástroje se nejprve pokusí získat poskytovatele služeb vyvoláním `Program.BuildWebHost()` a přístup k `IWebHost.Services` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="888a2-108">The tools first try to obtain the service provider by invoking `Program.BuildWebHost()` and accessing the `IWebHost.Services` property.</span></span>

> [!NOTE]
> <span data-ttu-id="888a2-109">Když vytvoříte novou aplikaci ASP.NET Core 2.0, je standardní součástí tohoto připojení.</span><span class="sxs-lookup"><span data-stu-id="888a2-109">When you create a new ASP.NET Core 2.0 application, this hook is included by default.</span></span> <span data-ttu-id="888a2-110">V předchozích verzích EF Core a ASP.NET Core, nástroje zkusit vyvolat `Startup.ConfigureServices` přímo, aby bylo možné získat aplikace poskytovatele služeb, ale tento model už fungování aplikace ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="888a2-110">In previous versions of EF Core and ASP.NET Core, the tools try to invoke `Startup.ConfigureServices` directly in order to obtain the application's service provider, but this pattern no longer works correctly in ASP.NET Core 2.0 applications.</span></span> <span data-ttu-id="888a2-111">Pokud provádíte upgrade aplikace ASP.NET Core 1.x do 2.0, můžete si [upravit vaše `Program` třídy mají nový tvar][3].</span><span class="sxs-lookup"><span data-stu-id="888a2-111">If you are upgrading an ASP.NET Core 1.x application to 2.0, you can [modify your `Program` class to follow the new pattern][3].</span></span>

<span data-ttu-id="888a2-112">`DbContext` Samostatně a všechny závislosti ve svých konstruktorech musí být zaregistrovaní jako služby ve zprostředkovateli služby vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="888a2-112">The `DbContext` itself and any dependencies in its constructor need to be registered as services in the application's service provider.</span></span> <span data-ttu-id="888a2-113">Toho lze snadno dosáhnout tím, že [konstruktoru na `DbContext` , která přijímá instanci `DbContextOptions<TContext>` jako argument] [ 4] a použití [ `AddDbContext<TContext>` –Metoda] [5].</span><span class="sxs-lookup"><span data-stu-id="888a2-113">This can be easily achieved by having [a constructor on the `DbContext` that takes an instance of `DbContextOptions<TContext>` as an argument][4] and using the [`AddDbContext<TContext>` method][5].</span></span>

<a name="using-a-constructor-with-no-parameters"></a><span data-ttu-id="888a2-114">Pomocí konstruktoru bez parametrů</span><span class="sxs-lookup"><span data-stu-id="888a2-114">Using a constructor with no parameters</span></span>
--------------------------------------
<span data-ttu-id="888a2-115">Pokud objekt DbContext nelze získat z aplikace poskytovatele služeb, nástrojů Hledat odvozený `DbContext` typu v projektu.</span><span class="sxs-lookup"><span data-stu-id="888a2-115">If the DbContext can't be obtained from the application service provider, the tools look for the derived `DbContext` type inside the project.</span></span> <span data-ttu-id="888a2-116">Pak se pokusí vytvořit instanci pomocí konstruktoru bez parametrů.</span><span class="sxs-lookup"><span data-stu-id="888a2-116">Then they try to create an instance using a constructor with no parameters.</span></span> <span data-ttu-id="888a2-117">To může být výchozí konstruktor, pokud `DbContext` je nakonfigurovaný nástrojem [ `OnConfiguring` ] [ 6] metody.</span><span class="sxs-lookup"><span data-stu-id="888a2-117">This can be the default constructor if the `DbContext` is configured using the [`OnConfiguring`][6] method.</span></span>

<a name="from-a-design-time-factory"></a><span data-ttu-id="888a2-118">Od objektu factory návrhu</span><span class="sxs-lookup"><span data-stu-id="888a2-118">From a design-time factory</span></span>
--------------------------
<span data-ttu-id="888a2-119">Můžete také říct, nástroje pro vytváření vaší DbContext implementací `IDesignTimeDbContextFactory<TContext>` rozhraní: Pokud implementující toto rozhraní se nachází buď stejném projektu jako odvozený `DbContext` nebo v projektu po spuštění aplikace, můžete obejít nástroje Další způsoby vytváření DbContext a použijte příslušný objekt pro vytváření návrhu místo.</span><span class="sxs-lookup"><span data-stu-id="888a2-119">You can also tell the tools how to create your DbContext by implementing the `IDesignTimeDbContextFactory<TContext>` interface: If a class implementing this interface is found in either the same project as the derived `DbContext` or in the application's startup project, the tools bypass the other ways of creating the DbContext and use the design-time factory instead.</span></span>

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
> <span data-ttu-id="888a2-120">`args` Parametr se aktuálně nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="888a2-120">The `args` parameter is currently unused.</span></span> <span data-ttu-id="888a2-121">Je [problém] [ 7] možnost zadat argumenty návrhu z nástrojů pro sledování.</span><span class="sxs-lookup"><span data-stu-id="888a2-121">There is [an issue][7] tracking the ability to specify design-time arguments from the tools.</span></span>

<span data-ttu-id="888a2-122">Objekt pro vytváření návrhu může být obzvláště užitečné, pokud je potřeba nakonfigurovat uvolněn objekt DbContext odlišně pro návrhové než v době běhu, pokud `DbContext` konstruktoru přijímá další parametry nejsou registrovány v DI, pokud nepoužíváte DI vůbec nebo, pokud u některých z důvodu nechcete mít `BuildWebHost` metodu v aplikaci ASP.NET Core `Main` třídy.</span><span class="sxs-lookup"><span data-stu-id="888a2-122">A design-time factory can be especially useful if you need to configure the DbContext differently for design time than at run time, if the `DbContext` constructor takes additional parameters are not registered in DI, if you are not using DI at all, or if for some reason you prefer not to have a `BuildWebHost` method in your ASP.NET Core application's `Main` class.</span></span>

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [4]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [5]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [6]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [7]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
