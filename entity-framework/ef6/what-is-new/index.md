---
title: Co je nového – EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
uid: ef6/what-is-new/index
ms.openlocfilehash: e0367aeefd682434bf520301776bcff4f0e72e06
ms.sourcegitcommit: c3b8386071d64953ee68788ef9d951144881a6ab
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/24/2020
ms.locfileid: "80136140"
---
# <a name="whats-new-in-ef6"></a><span data-ttu-id="78a94-102">Co je nového v EF6</span><span class="sxs-lookup"><span data-stu-id="78a94-102">What's new in EF6</span></span>

<span data-ttu-id="78a94-103">Důrazně doporučujeme, abyste používali nejnovější vydanou verzi Entity Framework, abyste měli jistotu, že získáte nejnovější funkce a nejvyšší stabilitu.</span><span class="sxs-lookup"><span data-stu-id="78a94-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="78a94-104">Uvědomujeme si ale, že možná budete muset použít předchozí verzi nebo můžete chtít experimentovat s novými vylepšeními v nejnovější předběžné verzi.</span><span class="sxs-lookup"><span data-stu-id="78a94-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="78a94-105">Chcete-li nainstalovat konkrétní verze EF, přečtěte si téma [Get Entity Framework](~/ef6/fundamentals/install.md).</span><span class="sxs-lookup"><span data-stu-id="78a94-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

## <a name="ef-640"></a><span data-ttu-id="78a94-106">EF 6.4.0</span><span class="sxs-lookup"><span data-stu-id="78a94-106">EF 6.4.0</span></span>

<span data-ttu-id="78a94-107">Modul runtime EF 6.4.0 byl vydán do NuGet v prosinci 2019.</span><span class="sxs-lookup"><span data-stu-id="78a94-107">The EF 6.4.0 runtime was released to NuGet in December  2019.</span></span> <span data-ttu-id="78a94-108">Primárním cílem EF 6,4 je vyleštěnení funkcí a scénářů, které byly doručeny v EF 6,3.</span><span class="sxs-lookup"><span data-stu-id="78a94-108">The primary goal of EF 6.4 is to polish the features and scenarios that was delivered in EF 6.3.</span></span> <span data-ttu-id="78a94-109">Podívejte se na [seznam důležitých oprav](https://github.com/dotnet/ef6/milestone/14?closed=1) na GitHubu.</span><span class="sxs-lookup"><span data-stu-id="78a94-109">See [list of important fixes](https://github.com/dotnet/ef6/milestone/14?closed=1) on Github.</span></span>

## <a name="ef-630"></a><span data-ttu-id="78a94-110">EF 6.3.0</span><span class="sxs-lookup"><span data-stu-id="78a94-110">EF 6.3.0</span></span>

<span data-ttu-id="78a94-111">Modul runtime EF 6.3.0 byl vydán do NuGet v září 2019.</span><span class="sxs-lookup"><span data-stu-id="78a94-111">The EF 6.3.0 runtime was released to NuGet in September 2019.</span></span> <span data-ttu-id="78a94-112">Hlavním cílem této verze bylo usnadnit migraci stávajících aplikací, které používají EF 6 až .NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="78a94-112">The main goal of this release was to facilitate migrating existing applications that use EF 6 to .NET Core 3.0.</span></span> <span data-ttu-id="78a94-113">Komunita také přispěla k několika opravám chyb a vylepšením.</span><span class="sxs-lookup"><span data-stu-id="78a94-113">The community has also contributed several bug fixes and enhancements.</span></span> <span data-ttu-id="78a94-114">Podrobnosti najdete v tématu problémy uzavřené v jednotlivých 6.3.0 [milníku](https://github.com/aspnet/EntityFramework6/milestones?state=closed) .</span><span class="sxs-lookup"><span data-stu-id="78a94-114">See the issues closed in each 6.3.0 [milestone](https://github.com/aspnet/EntityFramework6/milestones?state=closed) for details.</span></span> <span data-ttu-id="78a94-115">Tady jsou některé z důležitějších verzí:</span><span class="sxs-lookup"><span data-stu-id="78a94-115">Here are some of the more notable ones:</span></span>

- <span data-ttu-id="78a94-116">Podpora pro .NET Core 3,0</span><span class="sxs-lookup"><span data-stu-id="78a94-116">Support for .NET Core 3.0</span></span>
  - <span data-ttu-id="78a94-117">Balíček EntityFramework se teď zaměřuje .NET Standard 2,1, kromě .NET Framework 4. x.</span><span class="sxs-lookup"><span data-stu-id="78a94-117">The EntityFramework package now targets .NET Standard 2.1 in addition to .NET Framework 4.x.</span></span>
  - <span data-ttu-id="78a94-118">To znamená, že EF 6,3 je platforma pro víc platforem a je podporovaná na jiných operačních systémech než Windows, jako je Linux a macOS.</span><span class="sxs-lookup"><span data-stu-id="78a94-118">This means that EF 6.3 is cross-platform and supported on other operating systems besides Windows, like Linux and macOS.</span></span>
  - <span data-ttu-id="78a94-119">Příkazy migrace byly přepsány, aby vytvářely mimo proces a pracovaly s projekty ve stylu sady SDK.</span><span class="sxs-lookup"><span data-stu-id="78a94-119">The migrations commands have been rewritten to execute out of process and work with SDK-style projects.</span></span>
- <span data-ttu-id="78a94-120">Podpora SQL Server HierarchyId.</span><span class="sxs-lookup"><span data-stu-id="78a94-120">Support for SQL Server HierarchyId.</span></span>
- <span data-ttu-id="78a94-121">Vylepšená kompatibilita s Roslyn a NuGet PackageReference.</span><span class="sxs-lookup"><span data-stu-id="78a94-121">Improved compatibility with Roslyn and NuGet PackageReference.</span></span>
- <span data-ttu-id="78a94-122">Přidání nástroje `ef6.exe` pro povolování, přidávání, skriptování a instalaci migrace ze sestavení.</span><span class="sxs-lookup"><span data-stu-id="78a94-122">Added `ef6.exe` utility for enabling, adding, scripting, and applying migrations from assemblies.</span></span> <span data-ttu-id="78a94-123">Tato náhrada nahrazuje `migrate.exe`.</span><span class="sxs-lookup"><span data-stu-id="78a94-123">This replaces `migrate.exe`.</span></span>

### <a name="ef-designer-support"></a><span data-ttu-id="78a94-124">Podpora návrháře EF</span><span class="sxs-lookup"><span data-stu-id="78a94-124">EF designer support</span></span>

<span data-ttu-id="78a94-125">V tuto chvíli není k dispozici podpora pro použití návrháře EF přímo v projektech .NET Core nebo .NET Standard nebo v projektu .NET Framework stylu sady SDK.</span><span class="sxs-lookup"><span data-stu-id="78a94-125">There's currently no support for using the EF designer directly on .NET Core or .NET Standard projects or on an SDK-style .NET Framework project.</span></span> 

<span data-ttu-id="78a94-126">Toto omezení můžete obejít tak, že přidáte soubor EDMX a generované třídy pro entity a DbContext jako propojené soubory do projektu .NET Core 3,0 .NET Standard nebo 2,1 ve stejném řešení.</span><span class="sxs-lookup"><span data-stu-id="78a94-126">You can work around this limitation by adding the EDMX file and the generated classes for the entities and the DbContext as linked files to a .NET Core 3.0 or .NET Standard 2.1 project in the same solution.</span></span>

<span data-ttu-id="78a94-127">Propojené soubory budou vypadat jako v souboru projektu:</span><span class="sxs-lookup"><span data-stu-id="78a94-127">The linked files will look like this in the project file:</span></span>

``` csproj 
<ItemGroup>
  <EntityDeploy Include="..\EdmxDesignHost\Entities.edmx" Link="Model\Entities.edmx" />
  <Compile Include="..\EdmxDesignHost\Entities.Context.cs" Link="Model\Entities.Context.cs" />
  <Compile Include="..\EdmxDesignHost\Thing.cs" Link="Model\Thing.cs" />
  <Compile Include="..\EdmxDesignHost\Person.cs" Link="Model\Person.cs" />
</ItemGroup>
```

<span data-ttu-id="78a94-128">Všimněte si, že soubor EDMX je propojený s akcí sestavení EntityDeploy.</span><span class="sxs-lookup"><span data-stu-id="78a94-128">Note that the EDMX file is linked with the EntityDeploy build action.</span></span> <span data-ttu-id="78a94-129">Jedná se o speciální úlohu MSBuild (teď zahrnutou v balíčku EF 6,3), která postará o přidání modelu EF do cílového sestavení jako vložené prostředky (nebo jejich zkopírování jako soubory ve výstupní složce v závislosti na nastavení zpracování artefaktu metadat v EDMX).</span><span class="sxs-lookup"><span data-stu-id="78a94-129">This is a special MSBuild task (now included in the EF 6.3 package) that takes care of adding the EF model into the target assembly as embedded resources (or copying it as files in the output folder, depending on the Metadata Artifact Processing setting in the EDMX).</span></span> <span data-ttu-id="78a94-130">Další podrobnosti o tom, jak toto nastavení získat, najdete v našem [příkladu pro edmx .NET Core Sample](https://aka.ms/EdmxDotNetCoreSample).</span><span class="sxs-lookup"><span data-stu-id="78a94-130">For more details on how to get this set up, see our [EDMX .NET Core sample](https://aka.ms/EdmxDotNetCoreSample).</span></span>

<span data-ttu-id="78a94-131">Upozornění: Zajistěte, aby byl starý styl (tj. jiný typ než sada SDK) .NET Framework projekt definující "reálný" soubor. edmx _předchází_ projektu definujícímu propojení uvnitř souboru. sln.</span><span class="sxs-lookup"><span data-stu-id="78a94-131">Warning: make sure the old style (i.e. non-SDK-style) .NET Framework project defining the "real" .edmx file comes _before_ the project defining the link inside the .sln file.</span></span> <span data-ttu-id="78a94-132">V opačném případě se při otevření souboru EDMX v Návrháři zobrazí chybová zpráva "Entity Framework není k dispozici v cílovém rozhraní aktuálně zadaném pro projekt.</span><span class="sxs-lookup"><span data-stu-id="78a94-132">Otherwise, when you open the .edmx file in the designer, you see the error message "The Entity Framework is not available in the target framework currently specified for the project.</span></span> <span data-ttu-id="78a94-133">Můžete změnit cílovou architekturu projektu nebo upravit model v XmlEditor.</span><span class="sxs-lookup"><span data-stu-id="78a94-133">You can change the target framework of the project or edit the model in the XmlEditor".</span></span>

## <a name="past-releases"></a><span data-ttu-id="78a94-134">Předchozí verze</span><span class="sxs-lookup"><span data-stu-id="78a94-134">Past Releases</span></span>

<span data-ttu-id="78a94-135">Stránka [starší](past-releases.md) verze obsahuje archiv všech předchozích verzí EF a hlavní funkce, které byly představeny v každé vydané verzi.</span><span class="sxs-lookup"><span data-stu-id="78a94-135">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span>
