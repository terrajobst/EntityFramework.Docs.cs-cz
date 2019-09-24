---
title: Co je nového – EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
uid: ef6/what-is-new/index
ms.openlocfilehash: bb7038764644682c2149a8a500f342804d01f3d2
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71198035"
---
# <a name="whats-new-in-ef6"></a><span data-ttu-id="2941b-102">Co je nového v EF6</span><span class="sxs-lookup"><span data-stu-id="2941b-102">What's new in EF6</span></span>

<span data-ttu-id="2941b-103">Důrazně doporučujeme, abyste používali nejnovější vydanou verzi Entity Framework, abyste měli jistotu, že získáte nejnovější funkce a nejvyšší stabilitu.</span><span class="sxs-lookup"><span data-stu-id="2941b-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="2941b-104">Uvědomujeme si ale, že možná budete muset použít předchozí verzi nebo můžete chtít experimentovat s novými vylepšeními v nejnovější předběžné verzi.</span><span class="sxs-lookup"><span data-stu-id="2941b-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="2941b-105">Chcete-li nainstalovat konkrétní verze EF, přečtěte si téma [Get Entity Framework](~/ef6/fundamentals/install.md).</span><span class="sxs-lookup"><span data-stu-id="2941b-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

## <a name="ef-630"></a><span data-ttu-id="2941b-106">EF 6.3.0</span><span class="sxs-lookup"><span data-stu-id="2941b-106">EF 6.3.0</span></span>

<span data-ttu-id="2941b-107">Modul runtime EF 6.3.0 byl vydán do NuGet v září 2019.</span><span class="sxs-lookup"><span data-stu-id="2941b-107">The EF 6.3.0 runtime was released to NuGet in September 2019.</span></span> <span data-ttu-id="2941b-108">Hlavním cílem této verze bylo usnadnit migraci stávajících aplikací, které používají EF 6 až .NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="2941b-108">The main goal of this release was to facilitate migrating existing applications that use EF 6 to .NET Core 3.0.</span></span> <span data-ttu-id="2941b-109">Komunita také přispěla k několika opravám chyb a vylepšením.</span><span class="sxs-lookup"><span data-stu-id="2941b-109">The community has also contributed several bug fixes and enhancements.</span></span> <span data-ttu-id="2941b-110">Podrobnosti najdete v tématu problémy uzavřené v jednotlivých 6.3.0 [milníku](https://github.com/aspnet/EntityFramework6/milestones?state=closed) .</span><span class="sxs-lookup"><span data-stu-id="2941b-110">See the issues closed in each 6.3.0 [milestone](https://github.com/aspnet/EntityFramework6/milestones?state=closed) for details.</span></span> <span data-ttu-id="2941b-111">Tady jsou některé z důležitějších verzí:</span><span class="sxs-lookup"><span data-stu-id="2941b-111">Here are some of the more notable ones:</span></span>

- <span data-ttu-id="2941b-112">Podpora pro .NET Core 3,0</span><span class="sxs-lookup"><span data-stu-id="2941b-112">Support for .NET Core 3.0</span></span>
  - <span data-ttu-id="2941b-113">Balíček EntityFramework se teď zaměřuje .NET Standard 2,1, kromě .NET Framework 4. x.</span><span class="sxs-lookup"><span data-stu-id="2941b-113">The EntityFramework package now targets .NET Standard 2.1 in addition to .NET Framework 4.x</span></span>
  - <span data-ttu-id="2941b-114">Příkazy migrace byly přepsány, aby vytvářely mimo proces a pracovaly s projekty ve stylu sady SDK.</span><span class="sxs-lookup"><span data-stu-id="2941b-114">The migrations commands have been rewritten to execute out of process and work with SDK-style projects</span></span>
- <span data-ttu-id="2941b-115">Podpora SQL Server HierarchyId</span><span class="sxs-lookup"><span data-stu-id="2941b-115">Support for SQL Server HierarchyId</span></span>
- <span data-ttu-id="2941b-116">Vylepšená kompatibilita s Roslyn a NuGet PackageReference</span><span class="sxs-lookup"><span data-stu-id="2941b-116">Improved compatibility with Roslyn and NuGet PackageReference</span></span>
- <span data-ttu-id="2941b-117">Přidaný `ef6.exe` Nástroj pro povolování, přidávání, skriptování a instalaci migrace ze sestavení.</span><span class="sxs-lookup"><span data-stu-id="2941b-117">Added `ef6.exe` utility for enabling, adding, scripting, and applying migrations from assemblies.</span></span> <span data-ttu-id="2941b-118">Toto nahrazení`migrate.exe`</span><span class="sxs-lookup"><span data-stu-id="2941b-118">This replaces `migrate.exe`</span></span>

### <a name="ef-designer-support"></a><span data-ttu-id="2941b-119">Podpora návrháře EF</span><span class="sxs-lookup"><span data-stu-id="2941b-119">EF designer support</span></span>

<span data-ttu-id="2941b-120">V tuto chvíli není dostupná podpora pro použití návrháře EF přímo v projektech .NET Core nebo .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="2941b-120">There's currently no support for using the EF designer directly on .NET Core or .NET Standard projects.</span></span> 

<span data-ttu-id="2941b-121">Toto omezení můžete obejít tak, že přidáte soubor EDMX a generované třídy pro entity a DbContext jako propojené soubory do projektu .NET Core 3,0 .NET Standard nebo 2,1 ve stejném řešení.</span><span class="sxs-lookup"><span data-stu-id="2941b-121">You can work around this limitation by adding the EDMX file and the generated classes for the entities and the DbContext as linked files to a .NET Core 3.0 or .NET Standard 2.1 project in the same solution.</span></span>

<span data-ttu-id="2941b-122">Propojené soubory budou vypadat jako v souboru projektu:</span><span class="sxs-lookup"><span data-stu-id="2941b-122">The linked files will look like this in the project file:</span></span>

``` csproj 
&lt;ItemGroup&gt;
  &lt;EntityDeploy Include="..\EdmxDesignHost\Entities.edmx" Link="Model\Entities.edmx" /&gt;
  &lt;Compile Include="..\EdmxDesignHost\Entities.Context.cs" Link="Model\Entities.Context.cs" /&gt;
  &lt;Compile Include="..\EdmxDesignHost\Thing.cs" Link="Model\Thing.cs" /&gt;
  &lt;Compile Include="..\EdmxDesignHost\Person.cs" Link="Model\Person.cs" /&gt;
&lt;/ItemGroup&gt;
```

<span data-ttu-id="2941b-123">Všimněte si, že soubor EDMX je propojený s akcí sestavení EntityDeploy.</span><span class="sxs-lookup"><span data-stu-id="2941b-123">Note that the EDMX file is linked with the EntityDeploy build action.</span></span> <span data-ttu-id="2941b-124">Jedná se o speciální úlohu MSBuild (teď zahrnutou v balíčku EF 6,3), která postará o přidání modelu EF do cílového sestavení jako vložené prostředky (nebo jejich zkopírování jako soubory ve výstupní složce v závislosti na nastavení zpracování artefaktu metadat v EDMX).</span><span class="sxs-lookup"><span data-stu-id="2941b-124">This is a special MSBuild task (now included in the EF 6.3 package) that takes care of adding the EF model into the target assembly as embedded resources (or copying it as files in the output folder, depending on the Metadata Artifact Processing setting in the EDMX).</span></span> <span data-ttu-id="2941b-125">Další podrobnosti o tom, jak toto nastavení získat, najdete v našem [příkladu pro edmx .NET Core Sample](https://aka.ms/EdmxDotNetCoreSample).</span><span class="sxs-lookup"><span data-stu-id="2941b-125">For more details on how to get this set up, see our [EDMX .NET Core sample](https://aka.ms/EdmxDotNetCoreSample).</span></span>

## <a name="past-releases"></a><span data-ttu-id="2941b-126">Předchozí verze</span><span class="sxs-lookup"><span data-stu-id="2941b-126">Past Releases</span></span>

<span data-ttu-id="2941b-127">Stránka [starší](past-releases.md) verze obsahuje archiv všech předchozích verzí EF a hlavní funkce, které byly představeny v každé vydané verzi.</span><span class="sxs-lookup"><span data-stu-id="2941b-127">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span>
