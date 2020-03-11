---
title: Vytváření DbContext při návrhu – EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/16/2019
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: f44f0648678af5a70e5171d69692bde1c1d5e0eb
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416740"
---
# <a name="design-time-dbcontext-creation"></a><span data-ttu-id="6ce38-102">Vytváření DbContext v době návrhu</span><span class="sxs-lookup"><span data-stu-id="6ce38-102">Design-time DbContext Creation</span></span>

<span data-ttu-id="6ce38-103">Některé příkazy EF Core nástrojů (například příkazy [migrace][1] ) vyžadují, aby se v době návrhu vytvořila odvozená `DbContext` instance, aby se shromáždily podrobnosti o typech entit aplikace a jak se mapují ke schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="6ce38-103">Some of the EF Core Tools commands (for example, the [Migrations][1] commands) require a derived `DbContext` instance to be created at design time in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="6ce38-104">Ve většině případů je žádoucí, aby vytvořený `DbContext` byl nakonfigurován podobným způsobem, jak by byl [nakonfigurován v době běhu][2].</span><span class="sxs-lookup"><span data-stu-id="6ce38-104">In most cases, it is desirable that the `DbContext` thereby created is configured in a similar way to how it would be [configured at run time][2].</span></span>

<span data-ttu-id="6ce38-105">Existují různé způsoby, jak se nástroje snaží vytvořit `DbContext`:</span><span class="sxs-lookup"><span data-stu-id="6ce38-105">There are various ways the tools try to create the `DbContext`:</span></span>

## <a name="from-application-services"></a><span data-ttu-id="6ce38-106">Z aplikačních služeb</span><span class="sxs-lookup"><span data-stu-id="6ce38-106">From application services</span></span>

<span data-ttu-id="6ce38-107">Pokud váš projekt po spuštění používá [webového hostitele ASP.NET Core][3] nebo [obecného hostitele .NET Core][4], nástroj se pokusí získat objekt DbContext od poskytovatele služeb aplikace.</span><span class="sxs-lookup"><span data-stu-id="6ce38-107">If your startup project uses the [ASP.NET Core Web Host][3] or [.NET Core Generic Host][4], the tools try to obtain the DbContext object from the application's service provider.</span></span>

<span data-ttu-id="6ce38-108">Nástroje se nejprve pokusí získat poskytovatele služby vyvoláním `Program.CreateHostBuilder()`, voláním `Build()`a následným přístupem k vlastnosti `Services`.</span><span class="sxs-lookup"><span data-stu-id="6ce38-108">The tools first try to obtain the service provider by invoking `Program.CreateHostBuilder()`, calling `Build()`, then accessing the `Services` property.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/ApplicationService.cs)]

> [!NOTE]
> <span data-ttu-id="6ce38-109">Při vytváření nové aplikace ASP.NET Core je tento zavěšení součástí standardně.</span><span class="sxs-lookup"><span data-stu-id="6ce38-109">When you create a new ASP.NET Core application, this hook is included by default.</span></span>

<span data-ttu-id="6ce38-110">`DbContext` sám sebe a všechny závislosti ve svém konstruktoru musí být registrovány jako služby v poskytovateli služeb aplikace.</span><span class="sxs-lookup"><span data-stu-id="6ce38-110">The `DbContext` itself and any dependencies in its constructor need to be registered as services in the application's service provider.</span></span> <span data-ttu-id="6ce38-111">To lze snadno dosáhnout vytvořením [konstruktoru na `DbContext`, který převezme instanci `DbContextOptions<TContext>` jako argument][5] a pomocí [metody`AddDbContext<TContext>`][6].</span><span class="sxs-lookup"><span data-stu-id="6ce38-111">This can be easily achieved by having [a constructor on the `DbContext` that takes an instance of `DbContextOptions<TContext>` as an argument][5] and using the [`AddDbContext<TContext>` method][6].</span></span>

## <a name="using-a-constructor-with-no-parameters"></a><span data-ttu-id="6ce38-112">Použití konstruktoru bez parametrů</span><span class="sxs-lookup"><span data-stu-id="6ce38-112">Using a constructor with no parameters</span></span>

<span data-ttu-id="6ce38-113">Pokud DbContext nelze získat od poskytovatele služby Application Service, nástroje hledají odvozený typ `DbContext` v rámci projektu.</span><span class="sxs-lookup"><span data-stu-id="6ce38-113">If the DbContext can't be obtained from the application service provider, the tools look for the derived `DbContext` type inside the project.</span></span> <span data-ttu-id="6ce38-114">Pak se pokusí vytvořit instanci pomocí konstruktoru bez parametrů.</span><span class="sxs-lookup"><span data-stu-id="6ce38-114">Then they try to create an instance using a constructor with no parameters.</span></span> <span data-ttu-id="6ce38-115">To může být výchozí konstruktor, pokud je `DbContext` nakonfigurován pomocí metody [`OnConfiguring`][7] .</span><span class="sxs-lookup"><span data-stu-id="6ce38-115">This can be the default constructor if the `DbContext` is configured using the [`OnConfiguring`][7] method.</span></span>

## <a name="from-a-design-time-factory"></a><span data-ttu-id="6ce38-116">Z továrny v době návrhu</span><span class="sxs-lookup"><span data-stu-id="6ce38-116">From a design-time factory</span></span>

<span data-ttu-id="6ce38-117">Můžete také sdělit nástroje, jak vytvořit DbContext implementací rozhraní `IDesignTimeDbContextFactory<TContext>`: Pokud třída implementující toto rozhraní je nalezena v jednom projektu jako odvozená `DbContext` nebo v projektu po spuštění aplikace, nástroje vycházejí z jiných způsobů vytváření DbContext a místo toho používají továrnu v době návrhu.</span><span class="sxs-lookup"><span data-stu-id="6ce38-117">You can also tell the tools how to create your DbContext by implementing the `IDesignTimeDbContextFactory<TContext>` interface: If a class implementing this interface is found in either the same project as the derived `DbContext` or in the application's startup project, the tools bypass the other ways of creating the DbContext and use the design-time factory instead.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/BloggingContextFactory.cs)]

> [!NOTE]
> <span data-ttu-id="6ce38-118">Parametr `args` se v tuto chvíli nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="6ce38-118">The `args` parameter is currently unused.</span></span> <span data-ttu-id="6ce38-119">Existuje [problém][8] sledující možnost zadat argumenty pro dobu návrhu z nástrojů.</span><span class="sxs-lookup"><span data-stu-id="6ce38-119">There is [an issue][8] tracking the ability to specify design-time arguments from the tools.</span></span>

<span data-ttu-id="6ce38-120">Továrna v době návrhu může být obzvláště užitečná, pokud je třeba nakonfigurovat DbContext jinak než v době návrhu, a to v případě, že konstruktor `DbContext` přebírá další parametry nejsou registrovány v DI, pokud nepoužíváte hodnotu DI, nebo z nějakého důvodu nemusíte mít `BuildWebHost` metodu ve třídě `Main` vaší ASP.NET Core aplikace.</span><span class="sxs-lookup"><span data-stu-id="6ce38-120">A design-time factory can be especially useful if you need to configure the DbContext differently for design time than at run time, if the `DbContext` constructor takes additional parameters are not registered in DI, if you are not using DI at all, or if for some reason you prefer not to have a `BuildWebHost` method in your ASP.NET Core application's `Main` class.</span></span>

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: /aspnet/core/fundamentals/host/web-host
  [4]: /aspnet/core/fundamentals/host/generic-host
  [5]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [6]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [7]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [8]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
