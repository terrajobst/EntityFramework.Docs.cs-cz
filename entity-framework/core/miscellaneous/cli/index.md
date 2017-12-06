---
title: "Reference k příkazovému řádku - EF jádra"
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 076e9251850ba10df323cd25922aa8b95b3a5491
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
<a name="entity-framework-core-tools"></a><span data-ttu-id="5ca0c-102">Základní nástroje Entity Framework</span><span class="sxs-lookup"><span data-stu-id="5ca0c-102">Entity Framework Core Tools</span></span>
===========================
<span data-ttu-id="5ca0c-103">Nástroje Entity Framework Core vám pomohou při vývoji aplikace EF jádra.</span><span class="sxs-lookup"><span data-stu-id="5ca0c-103">The Entity Framework Core Tools help you during the development of EF Core apps.</span></span> <span data-ttu-id="5ca0c-104">Používají se primárně chcete vygenerovat typy DbContext a entity podle zpětnou analýzu schématu databáze a spravovat migrace.</span><span class="sxs-lookup"><span data-stu-id="5ca0c-104">They're primarily used to scaffold a DbContext and entity types by reverse engineering the schema of a database, and to manage Migrations.</span></span>

<span data-ttu-id="5ca0c-105">[EF základní balíček správce konzoly (pomocí PMC) nástroje] [ 1] poskytnout vynikající prostředí v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5ca0c-105">The [EF Core Package Manager Console (PMC) Tools][1] provide a superior experience inside Visual Studio.</span></span> <span data-ttu-id="5ca0c-106">Je spustit pomocí NuGet [Konzola správce balíčků][2].</span><span class="sxs-lookup"><span data-stu-id="5ca0c-106">Run them using NuGet's [Package Manager Console][2].</span></span> <span data-ttu-id="5ca0c-107">Tyto nástroje pro práci s projekty rozhraní .NET Framework a .NET Core.</span><span class="sxs-lookup"><span data-stu-id="5ca0c-107">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="5ca0c-108">[Nástroje příkazového řádku .NET Core EF] [ 3] jsou rozšíření pro [nástrojů rozhraní příkazového řádku (CLI) .NET Core] [ 4] , které jsou napříč platformami a můžete spustit mimo Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5ca0c-108">The [EF Core .NET Command-line Tools][3] are an extension to the [.NET Core command-line interface (CLI) tools][4] that are cross-platform and can run outside of Visual Studio.</span></span> <span data-ttu-id="5ca0c-109">Tyto nástroje vyžadují rozhraní .NET Core SDK projektu (s `Sdk="Microsoft.NET.Sdk"` nebo podobné v souboru projektu).</span><span class="sxs-lookup"><span data-stu-id="5ca0c-109">These tools require a .NET Core SDK project (one with `Sdk="Microsoft.NET.Sdk"` or similar in the project file).</span></span>

<span data-ttu-id="5ca0c-110">Oba nástroje zveřejňují stejné funkce.</span><span class="sxs-lookup"><span data-stu-id="5ca0c-110">Both tools expose the same functionality.</span></span> <span data-ttu-id="5ca0c-111">Pokud vyvíjíte v sadě Visual Studio, doporučujeme pomocí nástroje pomocí PMC vzhledem k tomu, že poskytují lepší integrace.</span><span class="sxs-lookup"><span data-stu-id="5ca0c-111">If you're developing in Visual Studio, we recommend using the PMC Tools since they provide a more integrated experience.</span></span>

<a name="frameworks"></a><span data-ttu-id="5ca0c-112">rozhraní</span><span class="sxs-lookup"><span data-stu-id="5ca0c-112">Frameworks</span></span>
----------
<span data-ttu-id="5ca0c-113">Nástroje pro podporu projektech zacílených na rozhraní .NET Framework nebo .NET Core.</span><span class="sxs-lookup"><span data-stu-id="5ca0c-113">The tools support projects targeting .NET Framework or .NET Core.</span></span>

<span data-ttu-id="5ca0c-114">Pokud je cílem vašeho projektu je jiný framework (například Universal Windows nebo Xamarin), doporučujeme vytvořit samostatnou .NET Standard projektu a cílení na mezi jeden z podporovaných architektur.</span><span class="sxs-lookup"><span data-stu-id="5ca0c-114">If your project targets another framework (for example, Universal Windows or Xamarin), we recommend creating a separate .NET Standard project and cross-targeting one of the supported frameworks.</span></span>

<span data-ttu-id="5ca0c-115">Cíl mezi .NET Core, například klikněte pravým tlačítkem na projekt a vyberte **upravit \*.csproj**.</span><span class="sxs-lookup"><span data-stu-id="5ca0c-115">To cross-target .NET Core, for example, right-click on the project and select **Edit \*.csproj**.</span></span> <span data-ttu-id="5ca0c-116">Aktualizace `TargetFramework` vlastnost následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="5ca0c-116">Update the `TargetFramework` property as follows.</span></span> <span data-ttu-id="5ca0c-117">(Všimněte si, název vlastnosti stane množném čísle.)</span><span class="sxs-lookup"><span data-stu-id="5ca0c-117">(Note, the property name becomes plural.)</span></span>

``` xml
<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>
```

<span data-ttu-id="5ca0c-118">Pokud používáte .NET Standard knihovny tříd, nemusíte mezi cíl, pokud je cílem vašeho projektu spuštění rozhraní .NET Framework nebo .NET Core.</span><span class="sxs-lookup"><span data-stu-id="5ca0c-118">If you're using a .NET Standard class library, you don't need to cross-target if your startup project targets .NET Framework or .NET Core.</span></span>

<a name="startup-and-target-projects"></a><span data-ttu-id="5ca0c-119">Spuštění a cíl projekty</span><span class="sxs-lookup"><span data-stu-id="5ca0c-119">Startup and Target Projects</span></span>
---------------------------
<span data-ttu-id="5ca0c-120">Při každém vyvolání příkazu, se skládá dva projekty: cílový projekt a projekt po spuštění.</span><span class="sxs-lookup"><span data-stu-id="5ca0c-120">Whenever you invoke a command, there are two projects involved: the target project and the startup project.</span></span>

<span data-ttu-id="5ca0c-121">Cílový projekt je tam, kde jsou přidány všechny soubory (nebo v některých případech odebrat).</span><span class="sxs-lookup"><span data-stu-id="5ca0c-121">The target project is where any files are added (or in some cases removed).</span></span>

<span data-ttu-id="5ca0c-122">Projekt po spuštění je emulovaných nástrojů při provádění kódu vašeho projektu.</span><span class="sxs-lookup"><span data-stu-id="5ca0c-122">The startup project is the one emulated by the tools when executing your project's code.</span></span>

<span data-ttu-id="5ca0c-123">Cílový projekt a projekt po spuštění může být stejné.</span><span class="sxs-lookup"><span data-stu-id="5ca0c-123">Both the target project and the startup project can be the same.</span></span>


  [1]: powershell.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: dotnet.md
  [4]: https://docs.microsoft.com/dotnet/core/tools/
