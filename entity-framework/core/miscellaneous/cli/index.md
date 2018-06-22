---
title: Reference k příkazovému řádku - EF jádra
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: db25ed55e3724ee71743e563f39a6e4b16c17589
ms.sourcegitcommit: fc68321c211aca38f7b9dc3a75677c6ca1b2524b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/08/2018
ms.locfileid: "29769403"
---
<a name="entity-framework-core-tools"></a><span data-ttu-id="bf852-102">Základní nástroje Entity Framework</span><span class="sxs-lookup"><span data-stu-id="bf852-102">Entity Framework Core Tools</span></span>
===========================
<span data-ttu-id="bf852-103">Nástroje Entity Framework Core vám pomohou při vývoji aplikace EF jádra.</span><span class="sxs-lookup"><span data-stu-id="bf852-103">The Entity Framework Core Tools help you during the development of EF Core apps.</span></span> <span data-ttu-id="bf852-104">Používají se primárně chcete vygenerovat typy DbContext a entity podle zpětnou analýzu schématu databáze a spravovat migrace.</span><span class="sxs-lookup"><span data-stu-id="bf852-104">They're primarily used to scaffold a DbContext and entity types by reverse engineering the schema of a database, and to manage Migrations.</span></span>

<span data-ttu-id="bf852-105">[EF základní balíček správce konzoly (pomocí PMC) nástroje] [ 1] poskytnout vynikající prostředí v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bf852-105">The [EF Core Package Manager Console (PMC) Tools][1] provide a superior experience inside Visual Studio.</span></span> <span data-ttu-id="bf852-106">Je spustit pomocí NuGet [Konzola správce balíčků][2].</span><span class="sxs-lookup"><span data-stu-id="bf852-106">Run them using NuGet's [Package Manager Console][2].</span></span> <span data-ttu-id="bf852-107">Tyto nástroje pro práci s projekty rozhraní .NET Framework a .NET Core.</span><span class="sxs-lookup"><span data-stu-id="bf852-107">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="bf852-108">[Nástroje příkazového řádku .NET Core EF] [ 3] jsou rozšíření pro [nástrojů rozhraní příkazového řádku (CLI) .NET Core] [ 4] , které jsou napříč platformami a můžete spustit mimo Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bf852-108">The [EF Core .NET Command-line Tools][3] are an extension to the [.NET Core command-line interface (CLI) tools][4] that are cross-platform and can run outside of Visual Studio.</span></span> <span data-ttu-id="bf852-109">Tyto nástroje vyžadují rozhraní .NET Core SDK projektu (s `Sdk="Microsoft.NET.Sdk"` nebo podobné v souboru projektu).</span><span class="sxs-lookup"><span data-stu-id="bf852-109">These tools require a .NET Core SDK project (one with `Sdk="Microsoft.NET.Sdk"` or similar in the project file).</span></span>

<span data-ttu-id="bf852-110">Oba nástroje zveřejňují stejné funkce.</span><span class="sxs-lookup"><span data-stu-id="bf852-110">Both tools expose the same functionality.</span></span> <span data-ttu-id="bf852-111">Pokud vyvíjíte v sadě Visual Studio, doporučujeme pomocí nástroje pomocí PMC vzhledem k tomu, že poskytují lepší integrace.</span><span class="sxs-lookup"><span data-stu-id="bf852-111">If you're developing in Visual Studio, we recommend using the PMC Tools since they provide a more integrated experience.</span></span>

<a name="frameworks"></a><span data-ttu-id="bf852-112">rozhraní</span><span class="sxs-lookup"><span data-stu-id="bf852-112">Frameworks</span></span>
----------
<span data-ttu-id="bf852-113">Nástroje pro podporu projektech zacílených na rozhraní .NET Framework nebo .NET Core.</span><span class="sxs-lookup"><span data-stu-id="bf852-113">The tools support projects targeting .NET Framework or .NET Core.</span></span>

<span data-ttu-id="bf852-114">Pokud chcete používat knihovny tříd, pak zvažte, pokud je to možné použití knihovny tříd .NET Core nebo rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="bf852-114">If you want to use a class library, then consider using a .NET Core or .NET Framework class library if possible.</span></span> <span data-ttu-id="bf852-115">Tato akce způsobí alespoň problémy s nástrojů .NET.</span><span class="sxs-lookup"><span data-stu-id="bf852-115">This will result in the least issues with .NET tooling.</span></span> <span data-ttu-id="bf852-116">Pokud chcete místo toho použijte rozhraní .NET standardní knihovny tříd, budete muset použít spouštěný projekt s cílem rozhraní .NET Framework nebo .NET Core tak, aby nástroje Cílová platforma conrete, do kterého je možné načíst knihovny tříd.</span><span class="sxs-lookup"><span data-stu-id="bf852-116">If instead you wish to use a .NET Standard class library, then you will need to use a startup project that targets .NET Framework or .NET Core so that the tooling has a conrete target platform into which it can load your class library.</span></span> <span data-ttu-id="bf852-117">Tento projekt po spuštění může být fiktivní projektu žádné skutečné kódem – je pouze potřebných k poskytování cíl pro nástrojů.</span><span class="sxs-lookup"><span data-stu-id="bf852-117">This startup project can be a dummy project with no real code--it is only needed to provide a target for the tooling.</span></span>

<span data-ttu-id="bf852-118">Pokud cílem vašeho projektu je jiný framework (například Universal Windows nebo Xamarin), pak bude muset vytvořit samostatný .NET standardní knihovny tříd.</span><span class="sxs-lookup"><span data-stu-id="bf852-118">If your project targets another framework (for example, Universal Windows or Xamarin), then you will need to create a separate .NET Standard class library.</span></span> <span data-ttu-id="bf852-119">V takovém případě podle pokynů uvedených výše můžete také vytvořit spouštěný projekt, který lze nástroji.</span><span class="sxs-lookup"><span data-stu-id="bf852-119">In this case, follow the guidance above to also create a startup project that can be used by the tooling.</span></span>

<a name="startup-and-target-projects"></a><span data-ttu-id="bf852-120">Spuštění a cíl projekty</span><span class="sxs-lookup"><span data-stu-id="bf852-120">Startup and Target Projects</span></span>
---------------------------
<span data-ttu-id="bf852-121">Při každém vyvolání příkazu, se skládá dva projekty: cílový projekt a projekt po spuštění.</span><span class="sxs-lookup"><span data-stu-id="bf852-121">Whenever you invoke a command, there are two projects involved: the target project and the startup project.</span></span>

<span data-ttu-id="bf852-122">Cílový projekt je tam, kde jsou přidány všechny soubory (nebo v některých případech odebrat).</span><span class="sxs-lookup"><span data-stu-id="bf852-122">The target project is where any files are added (or in some cases removed).</span></span>

<span data-ttu-id="bf852-123">Projekt po spuštění je emulovaných nástrojů při provádění kódu vašeho projektu.</span><span class="sxs-lookup"><span data-stu-id="bf852-123">The startup project is the one emulated by the tools when executing your project's code.</span></span>

<span data-ttu-id="bf852-124">Cílový projekt a projekt po spuštění může být stejné.</span><span class="sxs-lookup"><span data-stu-id="bf852-124">Both the target project and the startup project can be the same.</span></span>


  [1]: powershell.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: dotnet.md
  [4]: https://docs.microsoft.com/dotnet/core/tools/
