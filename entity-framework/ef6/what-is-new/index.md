---
title: Co je nového - EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
uid: ef6/what-is-new/index
ms.openlocfilehash: e0367aeefd682434bf520301776bcff4f0e72e06
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "80136140"
---
# <a name="whats-new-in-ef6"></a><span data-ttu-id="afeae-102">Co je nového v EF6</span><span class="sxs-lookup"><span data-stu-id="afeae-102">What's new in EF6</span></span>

<span data-ttu-id="afeae-103">Důrazně doporučujeme použít nejnovější vydanou verzi entity frameworku, abyste zajistili, že získáte nejnovější funkce a nejvyšší stabilitu.</span><span class="sxs-lookup"><span data-stu-id="afeae-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="afeae-104">Uvědomujeme si však, že možná budete muset použít předchozí verzi nebo že budete chtít experimentovat s novými vylepšeními v nejnovější předběžné verzi.</span><span class="sxs-lookup"><span data-stu-id="afeae-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="afeae-105">Chcete-li nainstalovat konkrétní verze EF, naleznete v tématu [Získat entity Framework](~/ef6/fundamentals/install.md).</span><span class="sxs-lookup"><span data-stu-id="afeae-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

## <a name="ef-640"></a><span data-ttu-id="afeae-106">EF 6.4.0</span><span class="sxs-lookup"><span data-stu-id="afeae-106">EF 6.4.0</span></span>

<span data-ttu-id="afeae-107">Běhový čas EF 6.4.0 byl vydán nugetv prosinci 2019.</span><span class="sxs-lookup"><span data-stu-id="afeae-107">The EF 6.4.0 runtime was released to NuGet in December  2019.</span></span> <span data-ttu-id="afeae-108">Primárním cílem EF 6.4 je vyleštit funkce a scénáře, které byly dodány v EF 6.3.</span><span class="sxs-lookup"><span data-stu-id="afeae-108">The primary goal of EF 6.4 is to polish the features and scenarios that was delivered in EF 6.3.</span></span> <span data-ttu-id="afeae-109">Podívejte se na [seznam důležitých oprav](https://github.com/dotnet/ef6/milestone/14?closed=1) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="afeae-109">See [list of important fixes](https://github.com/dotnet/ef6/milestone/14?closed=1) on Github.</span></span>

## <a name="ef-630"></a><span data-ttu-id="afeae-110">EF 6.3.0</span><span class="sxs-lookup"><span data-stu-id="afeae-110">EF 6.3.0</span></span>

<span data-ttu-id="afeae-111">Běhový čas EF 6.3.0 byl vydán nugetv září 2019.</span><span class="sxs-lookup"><span data-stu-id="afeae-111">The EF 6.3.0 runtime was released to NuGet in September 2019.</span></span> <span data-ttu-id="afeae-112">Hlavním cílem této verze bylo usnadnit migraci existujících aplikací, které používají EF 6 na .NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="afeae-112">The main goal of this release was to facilitate migrating existing applications that use EF 6 to .NET Core 3.0.</span></span> <span data-ttu-id="afeae-113">Komunita také přispěla několika opravami chyb a vylepšeními.</span><span class="sxs-lookup"><span data-stu-id="afeae-113">The community has also contributed several bug fixes and enhancements.</span></span> <span data-ttu-id="afeae-114">Podrobnosti najdete v problémech uzavřených v každém [milníku](https://github.com/aspnet/EntityFramework6/milestones?state=closed) 6.3.0.</span><span class="sxs-lookup"><span data-stu-id="afeae-114">See the issues closed in each 6.3.0 [milestone](https://github.com/aspnet/EntityFramework6/milestones?state=closed) for details.</span></span> <span data-ttu-id="afeae-115">Zde jsou některé z nejvýznamnějších z nich:</span><span class="sxs-lookup"><span data-stu-id="afeae-115">Here are some of the more notable ones:</span></span>

- <span data-ttu-id="afeae-116">Podpora pro rozhraní .NET Core 3.0</span><span class="sxs-lookup"><span data-stu-id="afeae-116">Support for .NET Core 3.0</span></span>
  - <span data-ttu-id="afeae-117">Balíček EntityFramework nyní cílí na standard .NET Standard 2.1 kromě rozhraní .NET Framework 4.x.</span><span class="sxs-lookup"><span data-stu-id="afeae-117">The EntityFramework package now targets .NET Standard 2.1 in addition to .NET Framework 4.x.</span></span>
  - <span data-ttu-id="afeae-118">To znamená, že EF 6.3 je multiplatformní a podporuje v jiných operačních systémech kromě Windows, jako je Linux a macOS.</span><span class="sxs-lookup"><span data-stu-id="afeae-118">This means that EF 6.3 is cross-platform and supported on other operating systems besides Windows, like Linux and macOS.</span></span>
  - <span data-ttu-id="afeae-119">Příkazy migrace byly přepsány tak, aby byly spuštěny mimo proces a pracovaly s projekty ve stylu sady SDK.</span><span class="sxs-lookup"><span data-stu-id="afeae-119">The migrations commands have been rewritten to execute out of process and work with SDK-style projects.</span></span>
- <span data-ttu-id="afeae-120">Podpora pro SQL Server HierarchyId.</span><span class="sxs-lookup"><span data-stu-id="afeae-120">Support for SQL Server HierarchyId.</span></span>
- <span data-ttu-id="afeae-121">Vylepšená kompatibilita s Roslyn a NuGet PackageReference.</span><span class="sxs-lookup"><span data-stu-id="afeae-121">Improved compatibility with Roslyn and NuGet PackageReference.</span></span>
- <span data-ttu-id="afeae-122">Byl `ef6.exe` přidán nástroj pro povolení, přidání, skriptování a použití migrace z sestavení.</span><span class="sxs-lookup"><span data-stu-id="afeae-122">Added `ef6.exe` utility for enabling, adding, scripting, and applying migrations from assemblies.</span></span> <span data-ttu-id="afeae-123">Tím se `migrate.exe`nahradí .</span><span class="sxs-lookup"><span data-stu-id="afeae-123">This replaces `migrate.exe`.</span></span>

### <a name="ef-designer-support"></a><span data-ttu-id="afeae-124">Podpora návrháře EF</span><span class="sxs-lookup"><span data-stu-id="afeae-124">EF designer support</span></span>

<span data-ttu-id="afeae-125">V současné době neexistuje žádná podpora pro použití návrháře EF přímo na projektech .NET Core nebo .NET Standard nebo v projektu rozhraní SDK .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="afeae-125">There's currently no support for using the EF designer directly on .NET Core or .NET Standard projects or on an SDK-style .NET Framework project.</span></span> 

<span data-ttu-id="afeae-126">Toto omezení můžete obejít přidáním souboru EDMX a generovaných tříd pro entity a DbContext jako propojené soubory do projektu .NET Core 3.0 nebo .NET Standard 2.1 ve stejném řešení.</span><span class="sxs-lookup"><span data-stu-id="afeae-126">You can work around this limitation by adding the EDMX file and the generated classes for the entities and the DbContext as linked files to a .NET Core 3.0 or .NET Standard 2.1 project in the same solution.</span></span>

<span data-ttu-id="afeae-127">Propojené soubory budou v souboru projektu vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="afeae-127">The linked files will look like this in the project file:</span></span>

``` csproj 
<ItemGroup>
  <EntityDeploy Include="..\EdmxDesignHost\Entities.edmx" Link="Model\Entities.edmx" />
  <Compile Include="..\EdmxDesignHost\Entities.Context.cs" Link="Model\Entities.Context.cs" />
  <Compile Include="..\EdmxDesignHost\Thing.cs" Link="Model\Thing.cs" />
  <Compile Include="..\EdmxDesignHost\Person.cs" Link="Model\Person.cs" />
</ItemGroup>
```

<span data-ttu-id="afeae-128">Všimněte si, že soubor EDMX je propojen s entityDeploy sestavení akce.</span><span class="sxs-lookup"><span data-stu-id="afeae-128">Note that the EDMX file is linked with the EntityDeploy build action.</span></span> <span data-ttu-id="afeae-129">Toto je speciální úloha MSBuild (nyní součástí balíčku EF 6.3), která se stará o přidání modelu EF do cílového sestavení jako vložené prostředky (nebo kopírování jako soubory ve výstupní složce, v závislosti na nastavení zpracování artefaktů metadat v EDMX).</span><span class="sxs-lookup"><span data-stu-id="afeae-129">This is a special MSBuild task (now included in the EF 6.3 package) that takes care of adding the EF model into the target assembly as embedded resources (or copying it as files in the output folder, depending on the Metadata Artifact Processing setting in the EDMX).</span></span> <span data-ttu-id="afeae-130">Další podrobnosti o tom, jak toto nastavení nastavit, naleznete v našem [vzorku EDMX .NET Core](https://aka.ms/EdmxDotNetCoreSample).</span><span class="sxs-lookup"><span data-stu-id="afeae-130">For more details on how to get this set up, see our [EDMX .NET Core sample](https://aka.ms/EdmxDotNetCoreSample).</span></span>

<span data-ttu-id="afeae-131">Upozornění: ujistěte se, že starý styl (tj. non-SDK-style) .NET Framework projekt definující "skutečný" .edmx soubor je _před_ projektem definující odkaz uvnitř souboru .sln.</span><span class="sxs-lookup"><span data-stu-id="afeae-131">Warning: make sure the old style (i.e. non-SDK-style) .NET Framework project defining the "real" .edmx file comes _before_ the project defining the link inside the .sln file.</span></span> <span data-ttu-id="afeae-132">V opačném případě při otevření souboru .edmx v návrháři se zobrazí chybová zpráva "Entity Framework není k dispozici v cílovém rámci aktuálně určené pro projekt.</span><span class="sxs-lookup"><span data-stu-id="afeae-132">Otherwise, when you open the .edmx file in the designer, you see the error message "The Entity Framework is not available in the target framework currently specified for the project.</span></span> <span data-ttu-id="afeae-133">Můžete změnit cílovou architekturu projektu nebo upravit model v XmlEditor".</span><span class="sxs-lookup"><span data-stu-id="afeae-133">You can change the target framework of the project or edit the model in the XmlEditor".</span></span>

## <a name="past-releases"></a><span data-ttu-id="afeae-134">Předchozí verze</span><span class="sxs-lookup"><span data-stu-id="afeae-134">Past Releases</span></span>

<span data-ttu-id="afeae-135">Stránka [Minulé verze](past-releases.md) obsahuje archiv všech předchozích verzí EF a hlavních funkcí, které byly zavedeny v každé verzi.</span><span class="sxs-lookup"><span data-stu-id="afeae-135">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span>
