---
title: Odkaz na příkazový řádek – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/06/2017
ms.openlocfilehash: d43b01fc61bb1c9b678e12e41c27d7efe9a59fa5
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490360"
---
<a name="entity-framework-core-tools"></a><span data-ttu-id="f89c5-102">Entity Framework Core Tools</span><span class="sxs-lookup"><span data-stu-id="f89c5-102">Entity Framework Core Tools</span></span>
===========================
<span data-ttu-id="f89c5-103">Entity Framework Core Tools umožňují při vývoji aplikace EF Core.</span><span class="sxs-lookup"><span data-stu-id="f89c5-103">The Entity Framework Core Tools help you during the development of EF Core apps.</span></span> <span data-ttu-id="f89c5-104">Primárně slouží pro generování uživatelského rozhraní a typy kontext databáze a entity ve zpětné analýze schématu databáze a ke správě migrace.</span><span class="sxs-lookup"><span data-stu-id="f89c5-104">They're primarily used to scaffold a DbContext and entity types by reverse engineering the schema of a database, and to manage Migrations.</span></span>

<span data-ttu-id="f89c5-105">[EF Core Package Manageru konzoly (PMC) nástroje] [ 1] poskytovat vynikající prostředí v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f89c5-105">The [EF Core Package Manager Console (PMC) Tools][1] provide a superior experience inside Visual Studio.</span></span> <span data-ttu-id="f89c5-106">Spustit pomocí Nugetu [Konzola správce balíčků][2].</span><span class="sxs-lookup"><span data-stu-id="f89c5-106">Run them using NuGet's [Package Manager Console][2].</span></span> <span data-ttu-id="f89c5-107">Tyto nástroje pracují s projekty rozhraní .NET Framework a .NET Core.</span><span class="sxs-lookup"><span data-stu-id="f89c5-107">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="f89c5-108">[Nástroje příkazového řádku .NET Core EF] [ 3] jsou rozšíření [nástroje rozhraní příkazového řádku (CLI) pro .NET Core] [ 4] , které jsou různé platformy a spustit mimo sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f89c5-108">The [EF Core .NET Command-line Tools][3] are an extension to the [.NET Core command-line interface (CLI) tools][4] that are cross-platform and can run outside of Visual Studio.</span></span> <span data-ttu-id="f89c5-109">Tyto nástroje vyžadují projektu .NET Core SDK (s `Sdk="Microsoft.NET.Sdk"` nebo podobně jako v souboru projektu).</span><span class="sxs-lookup"><span data-stu-id="f89c5-109">These tools require a .NET Core SDK project (one with `Sdk="Microsoft.NET.Sdk"` or similar in the project file).</span></span>

<span data-ttu-id="f89c5-110">Oba nástroje zveřejňují stejné funkce.</span><span class="sxs-lookup"><span data-stu-id="f89c5-110">Both tools expose the same functionality.</span></span> <span data-ttu-id="f89c5-111">Pokud vyvíjíte v sadě Visual Studio, doporučujeme pomocí PMC nástrojů, které poskytují více integrované prostředí.</span><span class="sxs-lookup"><span data-stu-id="f89c5-111">If you're developing in Visual Studio, we recommend using the PMC Tools since they provide a more integrated experience.</span></span>

<a name="frameworks"></a><span data-ttu-id="f89c5-112">Rozhraní</span><span class="sxs-lookup"><span data-stu-id="f89c5-112">Frameworks</span></span>
----------
<span data-ttu-id="f89c5-113">Nástroje podporují projekty cílené na rozhraní .NET Framework nebo .NET Core.</span><span class="sxs-lookup"><span data-stu-id="f89c5-113">The tools support projects targeting .NET Framework or .NET Core.</span></span>

<span data-ttu-id="f89c5-114">Pokud chcete použít knihovnu tříd, zvažte použití knihovny tříd .NET Core nebo .NET Framework, pokud je to možné.</span><span class="sxs-lookup"><span data-stu-id="f89c5-114">If you want to use a class library, then consider using a .NET Core or .NET Framework class library if possible.</span></span> <span data-ttu-id="f89c5-115">Výsledkem bude nejméně problémy s nástroji .NET.</span><span class="sxs-lookup"><span data-stu-id="f89c5-115">This will result in the least issues with .NET tooling.</span></span> <span data-ttu-id="f89c5-116">Pokud chcete místo toho použít knihovnu tříd .NET Standard, je potřeba použít spouštěný projekt, který cílí rozhraní .NET Framework nebo .NET Core tak, aby měl cílovou platformu conrete, do kterého je možné načíst knihovny tříd nástroje.</span><span class="sxs-lookup"><span data-stu-id="f89c5-116">If instead you wish to use a .NET Standard class library, then you will need to use a startup project that targets .NET Framework or .NET Core so that the tooling has a conrete target platform into which it can load your class library.</span></span> <span data-ttu-id="f89c5-117">Tento projekt po spuštění může být fiktivní projektu bez skutečné kódu – je pouze potřebných k poskytování cíl pro nástroje.</span><span class="sxs-lookup"><span data-stu-id="f89c5-117">This startup project can be a dummy project with no real code--it is only needed to provide a target for the tooling.</span></span>

<span data-ttu-id="f89c5-118">Pokud váš projekt cílí na jinou framework (například Universal Windows nebo Xamarin), je potřeba vytvořit samostatné knihovny tříd .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="f89c5-118">If your project targets another framework (for example, Universal Windows or Xamarin), then you will need to create a separate .NET Standard class library.</span></span> <span data-ttu-id="f89c5-119">V takovém případě postupujte podle pokynů výše vytvořte také spouštěný projekt, který je možné pomocí nástroje.</span><span class="sxs-lookup"><span data-stu-id="f89c5-119">In this case, follow the guidance above to also create a startup project that can be used by the tooling.</span></span>

<a name="startup-and-target-projects"></a><span data-ttu-id="f89c5-120">Spuštění a cílové projekty</span><span class="sxs-lookup"><span data-stu-id="f89c5-120">Startup and Target Projects</span></span>
---------------------------
<span data-ttu-id="f89c5-121">Při každém vyvolání příkazu se využívá řada dva projekty: cílový projekt a projekt po spuštění.</span><span class="sxs-lookup"><span data-stu-id="f89c5-121">Whenever you invoke a command, there are two projects involved: the target project and the startup project.</span></span>

<span data-ttu-id="f89c5-122">Cílový projekt je tam, kde se přidají všechny soubory (nebo v některých případech odebrána).</span><span class="sxs-lookup"><span data-stu-id="f89c5-122">The target project is where any files are added (or in some cases removed).</span></span>

<span data-ttu-id="f89c5-123">Projekt po spuštění je emulován nástroji při spuštění kódu projektu.</span><span class="sxs-lookup"><span data-stu-id="f89c5-123">The startup project is the one emulated by the tools when executing your project's code.</span></span>

<span data-ttu-id="f89c5-124">Cílový projekt a projekt po spuštění může být stejný.</span><span class="sxs-lookup"><span data-stu-id="f89c5-124">Both the target project and the startup project can be the same.</span></span>


  [1]: powershell.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: dotnet.md
  [4]: https://docs.microsoft.com/dotnet/core/tools/
