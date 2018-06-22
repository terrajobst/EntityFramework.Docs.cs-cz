---
title: Vytváření návrhu DbContext - EF jádra
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: 8b38d300d31038bdf5cd877aa3c42b7f5f97eabc
ms.sourcegitcommit: 7113e8675f26cbb546200824512078bf360225df
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/28/2018
ms.locfileid: "30202481"
---
<a name="design-time-dbcontext-creation"></a><span data-ttu-id="e4b54-102">Vytváření návrhu DbContext</span><span class="sxs-lookup"><span data-stu-id="e4b54-102">Design-time DbContext Creation</span></span>
==============================
<span data-ttu-id="e4b54-103">Některé příkazy EF základní nástroje (například [migrace] [ 1] příkazy) vyžadují odvozeným `DbContext` instance, která má být vytvořen v době návrhu, chcete-li shromažďovat údaje o aplikace typy entit a jak jsou mapovány na schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="e4b54-103">Some of the EF Core Tools commands (for example, the [Migrations][1] commands) require a derived `DbContext` instance to be created at design time in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="e4b54-104">Ve většině případů je žádoucí, který `DbContext` tím vytvořený je nakonfigurovaný podobným způsobem, jak by být [nakonfigurované v době běhu][2].</span><span class="sxs-lookup"><span data-stu-id="e4b54-104">In most cases, it is desirable that the `DbContext` thereby created is configured in a similar way to how it would be [configured at run time][2].</span></span>

<span data-ttu-id="e4b54-105">Existují různé způsoby nástroje pokuste se vytvořit `DbContext`:</span><span class="sxs-lookup"><span data-stu-id="e4b54-105">There are various ways the tools try to create the `DbContext`:</span></span>

<a name="from-application-services"></a><span data-ttu-id="e4b54-106">Z aplikačních služeb</span><span class="sxs-lookup"><span data-stu-id="e4b54-106">From application services</span></span>
-------------------------
<span data-ttu-id="e4b54-107">Pokud váš projekt po spuštění aplikace ASP.NET Core, nástroje se pokusí získat objekt DbContext z aplikace poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="e4b54-107">If your startup project is an ASP.NET Core app, the tools try to obtain the DbContext object from the application's service provider.</span></span>

<span data-ttu-id="e4b54-108">Tento nástroj se nejprve pokusí získat poskytovatele služeb vyvoláním `Program.BuildWebHost()` a přístup `IWebHost.Services` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="e4b54-108">The tool first try to obtain the service provider by invoking `Program.BuildWebHost()` and accessing the `IWebHost.Services` property.</span></span>

> [!NOTE]
> <span data-ttu-id="e4b54-109">Když vytvoříte novou aplikaci ASP.NET 2.0 jádra, je tato háku zahrnuté ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="e4b54-109">When you create a new ASP.NET Core 2.0 application, this hook is included by default.</span></span> <span data-ttu-id="e4b54-110">V předchozích verzích EF Core a ASP.NET Core, zkuste vyvolání nástroje `Startup.ConfigureServices` přímo za účelem získání aplikace poskytovatele služeb, ale tento vzor už funguje správně v aplikacích ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="e4b54-110">In previous versions of EF Core and ASP.NET Core, the tools try to invoke `Startup.ConfigureServices` directly in order to obtain the application's service provider, but this pattern no longer works correctly in ASP.NET Core 2.0 applications.</span></span> <span data-ttu-id="e4b54-111">Pokud provádíte upgrade aplikace ASP.NET Core 1.x na 2.0, můžete [upravit vaše `Program` třída podle vzoru nové][3].</span><span class="sxs-lookup"><span data-stu-id="e4b54-111">If you are upgrading an ASP.NET Core 1.x application to 2.0, you can [modify your `Program` class to follow the new pattern][3].</span></span>

<span data-ttu-id="e4b54-112">`DbContext` Samostatně a závislostí v jeho konstruktor musí být registrované jako služby ve aplikace poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="e4b54-112">The `DbContext` itself and any dependencies in its constructor need to be registered as services in the application's service provider.</span></span> <span data-ttu-id="e4b54-113">Toho lze snadno dosáhnout tak, že [konstruktor na `DbContext` instanci, která má `DbContextOptions<TContext>` jako argument] [ 4] a pomocí [ `AddDbContext<TContext>` –Metoda] [5].</span><span class="sxs-lookup"><span data-stu-id="e4b54-113">This can be easily achieved by having [a constructor on the `DbContext` that takes an instance of `DbContextOptions<TContext>` as a argument][4] and using the [`AddDbContext<TContext>` method][5].</span></span>

<a name="using-a-constructor-with-no-parameters"></a><span data-ttu-id="e4b54-114">Pomocí konstruktor bez parametrů</span><span class="sxs-lookup"><span data-stu-id="e4b54-114">Using a constructor with no parameters</span></span>
--------------------------------------
<span data-ttu-id="e4b54-115">Pokud DbContext nelze získat ze zprostředkovatele služeb aplikací, nástroje Hledat odvozené `DbContext` typ uvnitř projektu.</span><span class="sxs-lookup"><span data-stu-id="e4b54-115">If the DbContext can't be obtained from the application service provider, the tools look for the derived `DbContext` type inside the project.</span></span> <span data-ttu-id="e4b54-116">Potom se pokuste se vytvořit instanci pomocí konstruktor bez parametrů.</span><span class="sxs-lookup"><span data-stu-id="e4b54-116">Then they try to create an instance using a constructor with no parameters.</span></span> <span data-ttu-id="e4b54-117">Může to být výchozí konstruktor. Pokud `DbContext` nakonfigurovaná pomocí [ `OnConfiguring` ] [ 6] metoda.</span><span class="sxs-lookup"><span data-stu-id="e4b54-117">This can be the default constructor if the `DbContext` is configured using the [`OnConfiguring`][6] method.</span></span>

<a name="from-a-design-time-factory"></a><span data-ttu-id="e4b54-118">Z objekt pro vytváření návrhu</span><span class="sxs-lookup"><span data-stu-id="e4b54-118">From a design-time factory</span></span>
--------------------------
<span data-ttu-id="e4b54-119">Můžete zjistit také nástroje vytvoření vaší DbContext implementací `IDesignTimeDbContextFactory<TContext>` rozhraní: Pokud se nenajde třídu implementace tohoto rozhraní buď stejné projektu jako odvozené `DbContext` nebo v projektu po spuštění aplikace nástroje nepoužívat Další způsoby vytváření DbContext a použití objektu pro vytváření návrhu místo.</span><span class="sxs-lookup"><span data-stu-id="e4b54-119">You can also tell the tools how to create your DbContext by implementing the `IDesignTimeDbContextFactory<TContext>` interface: If a class implementing this interface is found in either the same project as the derived `DbContext` or in the application's startup project, the tools bypass the other ways of creating the DbContext and use the design-time factory instead.</span></span>

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
> <span data-ttu-id="e4b54-120">`args` Parametr je aktuálně používána.</span><span class="sxs-lookup"><span data-stu-id="e4b54-120">The `args` parameter is currently unused.</span></span> <span data-ttu-id="e4b54-121">Je [problém] [ 7] možnost zadejte argumenty návrhu z nástroje pro sledování.</span><span class="sxs-lookup"><span data-stu-id="e4b54-121">There is [an issue][7] tracking the ability to specify design-time arguments from the tools.</span></span>

<span data-ttu-id="e4b54-122">Objekt pro vytváření návrhu může být obzvláště užitečný, pokud je nutné nakonfigurovat DbContext odlišně pro dobu návrhu, než v době běhu, pokud `DbContext` konstruktor trvá další parametry nejsou registrované v DI, pokud nepoužíváte DI vůbec, nebo, pokud pro některé důvodu nechcete mít `BuildWebHost` v aplikaci ASP.NET Core – metoda</span><span class="sxs-lookup"><span data-stu-id="e4b54-122">A design-time factory can be especially useful if you need to configure the DbContext differently for design time than at run time, if the `DbContext` constructor takes additional parameters are not registered in DI, if you are not using DI at all, or if for some reason you prefer not to have a `BuildWebHost` method in your ASP.NET Core application's</span></span>  
<span data-ttu-id="e4b54-123">`Main` Třída.</span><span class="sxs-lookup"><span data-stu-id="e4b54-123">`Main` class.</span></span>

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [4]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [5]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [6]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [7]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
