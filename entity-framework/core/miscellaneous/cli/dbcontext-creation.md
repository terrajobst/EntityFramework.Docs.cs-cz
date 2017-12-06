---
title: "Vytváření návrhu DbContext - EF jádra"
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 5fcd9e362d76127e7acadd9e552ef3ac90967a37
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
<a name="design-time-dbcontext-creation"></a><span data-ttu-id="95090-102">Vytváření návrhu DbContext</span><span class="sxs-lookup"><span data-stu-id="95090-102">Design-time DbContext Creation</span></span>
==============================
<span data-ttu-id="95090-103">Některé nástroje EF příkazy vyžadují instance DbContext má být vytvořen v návrhu čas (například při spuštění migrace příkazů).</span><span class="sxs-lookup"><span data-stu-id="95090-103">Some of the EF Tools commands require a DbContext instance to be created at design time (for example, when running Migrations commands).</span></span> <span data-ttu-id="95090-104">Existují různé způsoby nástroje pokuste se jej vytvořit.</span><span class="sxs-lookup"><span data-stu-id="95090-104">There are various ways the tools try to create it.</span></span>

<a name="from-application-services"></a><span data-ttu-id="95090-105">Z aplikačních služeb</span><span class="sxs-lookup"><span data-stu-id="95090-105">From application services</span></span>
-------------------------
<span data-ttu-id="95090-106">Pokud váš projekt po spuštění aplikace ASP.NET Core, nástroje se pokusí získat objekt DbContext z aplikace poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="95090-106">If your startup project is an ASP.NET Core app, the tools try to obtain the DbContext object from the application's service provider.</span></span> <span data-ttu-id="95090-107">Získají vyvoláním `Program.BuildWebHost()` a přístup `IWebHost.Services` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="95090-107">They obtain it by invoking `Program.BuildWebHost()` and accessing the `IWebHost.Services` property.</span></span> <span data-ttu-id="95090-108">Všechny DbContext registrovat pomocí `IServiceCollection.AddDbContext<TContext>()` můžete najít a vytvořené tímto způsobem.</span><span class="sxs-lookup"><span data-stu-id="95090-108">Any DbContext registered using `IServiceCollection.AddDbContext<TContext>()` can be found and created this way.</span></span> <span data-ttu-id="95090-109">Tento vzor byl [byla zavedená v technologii ASP.NET 2.0 jádra][1]</span><span class="sxs-lookup"><span data-stu-id="95090-109">This pattern was [introduced in ASP.NET Core 2.0][1]</span></span>

<a name="using-the-default-constructor"></a><span data-ttu-id="95090-110">Pomocí výchozí konstruktor.</span><span class="sxs-lookup"><span data-stu-id="95090-110">Using the default constructor</span></span>
-----------------------------
<span data-ttu-id="95090-111">Pokud se DbContext nelze získat ze zprostředkovatele služeb aplikací, podívejte se nástroje pro typ DbContext v projektu.</span><span class="sxs-lookup"><span data-stu-id="95090-111">If the DbContext can't be obtained from the application service provider, the tools look for the DbContext type inside the project.</span></span> <span data-ttu-id="95090-112">Se pokusí vytvořit pomocí jeho výchozí konstruktor.</span><span class="sxs-lookup"><span data-stu-id="95090-112">They try to create it using its default constructor.</span></span>

<a name="from-a-design-time-factory"></a><span data-ttu-id="95090-113">Z objekt pro vytváření návrhu</span><span class="sxs-lookup"><span data-stu-id="95090-113">From a design-time factory</span></span>
--------------------------
<span data-ttu-id="95090-114">Můžete zjistit také nástroje vytvoření vaší DbContext implementací `IDesignTimeDbContextFactory`.</span><span class="sxs-lookup"><span data-stu-id="95090-114">You can also tell the tools how to create your DbContext by implementing `IDesignTimeDbContextFactory`.</span></span> <span data-ttu-id="95090-115">Pokud se najde třídu implementace tohoto rozhraní v projektu, nástroje nepoužívat další způsoby vytváření DbContext.</span><span class="sxs-lookup"><span data-stu-id="95090-115">If a class implementing this interface is found inside your project, the tools bypass the other ways of creating the DbContext.</span></span>
<span data-ttu-id="95090-116">Vždy používají objekt factory v době návrhu.</span><span class="sxs-lookup"><span data-stu-id="95090-116">They always use the factory at design time.</span></span> <span data-ttu-id="95090-117">Objekt je obzvláště užitečné, pokud je nutné nakonfigurovat DbContext odlišně pro dobu návrhu, než v době běhu.</span><span class="sxs-lookup"><span data-stu-id="95090-117">A factory is especially useful if you need to configure the DbContext differently for design time than at runtime.</span></span>

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

> [!NOTE]
> <span data-ttu-id="95090-118">`args` Parametr je aktuálně používána.</span><span class="sxs-lookup"><span data-stu-id="95090-118">The `args` parameter is currently unused.</span></span> <span data-ttu-id="95090-119">Je [problém] [ 2] možnost zadejte argumenty návrhu z nástroje pro sledování.</span><span class="sxs-lookup"><span data-stu-id="95090-119">There is [an issue][2] tracking the ability to specify design-time arguments from the tools.</span></span>

  [1]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [2]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
