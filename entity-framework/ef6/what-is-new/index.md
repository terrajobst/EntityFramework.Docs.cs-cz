---
title: Co je nového – EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
uid: ef6/what-is-new/index
ms.openlocfilehash: 9daae787d0cec0ca536413e6263bb363ba76ff2c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419657"
---
# <a name="whats-new-in-ef6"></a><span data-ttu-id="89aef-102">Co je nového v EF6</span><span class="sxs-lookup"><span data-stu-id="89aef-102">What's new in EF6</span></span>

<span data-ttu-id="89aef-103">Důrazně doporučujeme, abyste používali nejnovější vydanou verzi Entity Framework, abyste měli jistotu, že získáte nejnovější funkce a nejvyšší stabilitu.</span><span class="sxs-lookup"><span data-stu-id="89aef-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="89aef-104">Uvědomujeme si ale, že možná budete muset použít předchozí verzi nebo můžete chtít experimentovat s novými vylepšeními v nejnovější předběžné verzi.</span><span class="sxs-lookup"><span data-stu-id="89aef-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="89aef-105">Chcete-li nainstalovat konkrétní verze EF, přečtěte si téma [Get Entity Framework](~/ef6/fundamentals/install.md).</span><span class="sxs-lookup"><span data-stu-id="89aef-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

## <a name="ef-630"></a><span data-ttu-id="89aef-106">EF 6.3.0</span><span class="sxs-lookup"><span data-stu-id="89aef-106">EF 6.3.0</span></span>

<span data-ttu-id="89aef-107">Modul runtime EF 6.3.0 byl vydán do NuGet v září 2019.</span><span class="sxs-lookup"><span data-stu-id="89aef-107">The EF 6.3.0 runtime was released to NuGet in September 2019.</span></span> <span data-ttu-id="89aef-108">Hlavním cílem této verze bylo usnadnit migraci stávajících aplikací, které používají EF 6 až .NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="89aef-108">The main goal of this release was to facilitate migrating existing applications that use EF 6 to .NET Core 3.0.</span></span> <span data-ttu-id="89aef-109">Komunita také přispěla k několika opravám chyb a vylepšením.</span><span class="sxs-lookup"><span data-stu-id="89aef-109">The community has also contributed several bug fixes and enhancements.</span></span> <span data-ttu-id="89aef-110">Podrobnosti najdete v tématu problémy uzavřené v jednotlivých 6.3.0 [milníku](https://github.com/aspnet/EntityFramework6/milestones?state=closed) .</span><span class="sxs-lookup"><span data-stu-id="89aef-110">See the issues closed in each 6.3.0 [milestone](https://github.com/aspnet/EntityFramework6/milestones?state=closed) for details.</span></span> <span data-ttu-id="89aef-111">Tady jsou některé z důležitějších verzí:</span><span class="sxs-lookup"><span data-stu-id="89aef-111">Here are some of the more notable ones:</span></span>

- <span data-ttu-id="89aef-112">Podpora pro .NET Core 3,0</span><span class="sxs-lookup"><span data-stu-id="89aef-112">Support for .NET Core 3.0</span></span>
  - <span data-ttu-id="89aef-113">Balíček EntityFramework se teď zaměřuje .NET Standard 2,1, kromě .NET Framework 4. x.</span><span class="sxs-lookup"><span data-stu-id="89aef-113">The EntityFramework package now targets .NET Standard 2.1 in addition to .NET Framework 4.x.</span></span>
  - <span data-ttu-id="89aef-114">To znamená, že EF 6,3 je platforma pro víc platforem a je podporovaná na jiných operačních systémech než Windows, jako je Linux a macOS.</span><span class="sxs-lookup"><span data-stu-id="89aef-114">This means that EF 6.3 is cross-platform and supported on other operating systems besides Windows, like Linux and macOS.</span></span>
  - <span data-ttu-id="89aef-115">Příkazy migrace byly přepsány, aby vytvářely mimo proces a pracovaly s projekty ve stylu sady SDK.</span><span class="sxs-lookup"><span data-stu-id="89aef-115">The migrations commands have been rewritten to execute out of process and work with SDK-style projects.</span></span>
- <span data-ttu-id="89aef-116">Podpora SQL Server HierarchyId.</span><span class="sxs-lookup"><span data-stu-id="89aef-116">Support for SQL Server HierarchyId.</span></span>
- <span data-ttu-id="89aef-117">Vylepšená kompatibilita s Roslyn a NuGet PackageReference.</span><span class="sxs-lookup"><span data-stu-id="89aef-117">Improved compatibility with Roslyn and NuGet PackageReference.</span></span>
- <span data-ttu-id="89aef-118">Přidání nástroje `ef6.exe` pro povolování, přidávání, skriptování a instalaci migrace ze sestavení.</span><span class="sxs-lookup"><span data-stu-id="89aef-118">Added `ef6.exe` utility for enabling, adding, scripting, and applying migrations from assemblies.</span></span> <span data-ttu-id="89aef-119">Tato náhrada nahrazuje `migrate.exe`.</span><span class="sxs-lookup"><span data-stu-id="89aef-119">This replaces `migrate.exe`.</span></span>

### <a name="ef-designer-support"></a><span data-ttu-id="89aef-120">Podpora návrháře EF</span><span class="sxs-lookup"><span data-stu-id="89aef-120">EF designer support</span></span>

<span data-ttu-id="89aef-121">V tuto chvíli není dostupná podpora pro použití návrháře EF přímo v projektech .NET Core nebo .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="89aef-121">There's currently no support for using the EF designer directly on .NET Core or .NET Standard projects.</span></span> 

<span data-ttu-id="89aef-122">Toto omezení můžete obejít tak, že přidáte soubor EDMX a generované třídy pro entity a DbContext jako propojené soubory do projektu .NET Core 3,0 .NET Standard nebo 2,1 ve stejném řešení.</span><span class="sxs-lookup"><span data-stu-id="89aef-122">You can work around this limitation by adding the EDMX file and the generated classes for the entities and the DbContext as linked files to a .NET Core 3.0 or .NET Standard 2.1 project in the same solution.</span></span>

<span data-ttu-id="89aef-123">Propojené soubory budou vypadat jako v souboru projektu:</span><span class="sxs-lookup"><span data-stu-id="89aef-123">The linked files will look like this in the project file:</span></span>

``` csproj 
<ItemGroup>
  <EntityDeploy Include="..\EdmxDesignHost\Entities.edmx" Link="Model\Entities.edmx" />
  <Compile Include="..\EdmxDesignHost\Entities.Context.cs" Link="Model\Entities.Context.cs" />
  <Compile Include="..\EdmxDesignHost\Thing.cs" Link="Model\Thing.cs" />
  <Compile Include="..\EdmxDesignHost\Person.cs" Link="Model\Person.cs" />
</ItemGroup>
```

<span data-ttu-id="89aef-124">Všimněte si, že soubor EDMX je propojený s akcí sestavení EntityDeploy.</span><span class="sxs-lookup"><span data-stu-id="89aef-124">Note that the EDMX file is linked with the EntityDeploy build action.</span></span> <span data-ttu-id="89aef-125">Jedná se o speciální úlohu MSBuild (teď zahrnutou v balíčku EF 6,3), která postará o přidání modelu EF do cílového sestavení jako vložené prostředky (nebo jejich zkopírování jako soubory ve výstupní složce v závislosti na nastavení zpracování artefaktu metadat v EDMX).</span><span class="sxs-lookup"><span data-stu-id="89aef-125">This is a special MSBuild task (now included in the EF 6.3 package) that takes care of adding the EF model into the target assembly as embedded resources (or copying it as files in the output folder, depending on the Metadata Artifact Processing setting in the EDMX).</span></span> <span data-ttu-id="89aef-126">Další podrobnosti o tom, jak toto nastavení získat, najdete v našem [příkladu pro edmx .NET Core Sample](https://aka.ms/EdmxDotNetCoreSample).</span><span class="sxs-lookup"><span data-stu-id="89aef-126">For more details on how to get this set up, see our [EDMX .NET Core sample](https://aka.ms/EdmxDotNetCoreSample).</span></span>

## <a name="past-releases"></a><span data-ttu-id="89aef-127">Předchozí verze</span><span class="sxs-lookup"><span data-stu-id="89aef-127">Past Releases</span></span>

<span data-ttu-id="89aef-128">Stránka [starší](past-releases.md) verze obsahuje archiv všech předchozích verzí EF a hlavní funkce, které byly představeny v každé vydané verzi.</span><span class="sxs-lookup"><span data-stu-id="89aef-128">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span>
