---
title: Co je nového – EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
ms.openlocfilehash: 568790d9c9bb7dd2213907bef8fa090710cd3ba0
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149115"
---
# <a name="whats-new-in-ef6"></a><span data-ttu-id="e380d-102">Co je nového v EF6</span><span class="sxs-lookup"><span data-stu-id="e380d-102">What's New in EF6</span></span>

<span data-ttu-id="e380d-103">Důrazně doporučujeme, abyste používali nejnovější vydanou verzi Entity Framework, abyste měli jistotu, že získáte nejnovější funkce a nejvyšší stabilitu.</span><span class="sxs-lookup"><span data-stu-id="e380d-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="e380d-104">Uvědomujeme si ale, že možná budete muset použít předchozí verzi nebo můžete chtít experimentovat s novými vylepšeními v nejnovější předběžné verzi.</span><span class="sxs-lookup"><span data-stu-id="e380d-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="e380d-105">Chcete-li nainstalovat konkrétní verze EF, přečtěte si téma [Get Entity Framework](~/ef6/fundamentals/install.md).</span><span class="sxs-lookup"><span data-stu-id="e380d-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

## <a name="ef-630"></a><span data-ttu-id="e380d-106">EF 6.3.0</span><span class="sxs-lookup"><span data-stu-id="e380d-106">EF 6.3.0</span></span>

<span data-ttu-id="e380d-107">Modul runtime EF 6.3.0 byl vydán do NuGet v září 2019.</span><span class="sxs-lookup"><span data-stu-id="e380d-107">The EF 6.3.0 runtime was released to NuGet in September 2019.</span></span> <span data-ttu-id="e380d-108">Hlavním cílem této verze bylo usnadnit migraci stávajících aplikací, které používají EF 6 až .NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="e380d-108">The main goal of this release was to facilitate migrating existing applications that use EF 6 to .NET Core 3.0.</span></span> <span data-ttu-id="e380d-109">Komunita také přispěla k několika opravám chyb a vylepšením.</span><span class="sxs-lookup"><span data-stu-id="e380d-109">The community has also contributed several bug fixes and enhancements.</span></span> <span data-ttu-id="e380d-110">Podrobnosti najdete v tématu problémy uzavřené v jednotlivých 6.3.0 [milníku](https://github.com/aspnet/EntityFramework6/milestones?state=closed) .</span><span class="sxs-lookup"><span data-stu-id="e380d-110">See the issues closed in each 6.3.0 [milestone](https://github.com/aspnet/EntityFramework6/milestones?state=closed) for details.</span></span> <span data-ttu-id="e380d-111">Tady jsou některé z důležitějších verzí:</span><span class="sxs-lookup"><span data-stu-id="e380d-111">Here are some of the more notable ones:</span></span>

- <span data-ttu-id="e380d-112">Podpora pro .NET Core 3,0</span><span class="sxs-lookup"><span data-stu-id="e380d-112">Support for .NET Core 3.0</span></span>
  - <span data-ttu-id="e380d-113">Balíček EntityFramework se teď zaměřuje .NET Standard 2,1, kromě .NET Framework 4. x.</span><span class="sxs-lookup"><span data-stu-id="e380d-113">The EntityFramework package now targets .NET Standard 2.1 in addition to .NET Framework 4.x</span></span>
  - <span data-ttu-id="e380d-114">Příkazy migrace byly přepsány, aby vytvářely mimo proces a pracovaly s projekty ve stylu sady SDK.</span><span class="sxs-lookup"><span data-stu-id="e380d-114">The migrations commands have been rewritten to execute out of process and work with SDK-style projects</span></span>
- <span data-ttu-id="e380d-115">Podpora SQL Server HierarchyId</span><span class="sxs-lookup"><span data-stu-id="e380d-115">Support for SQL Server HierarchyId</span></span>
- <span data-ttu-id="e380d-116">Vylepšená kompatibilita s Roslyn a NuGet PackageReference</span><span class="sxs-lookup"><span data-stu-id="e380d-116">Improved compatibility with Roslyn and NuGet PackageReference</span></span>
- <span data-ttu-id="e380d-117">Přidání EF6. exe pro povolení, přidání, skriptování a instalaci migrace ze sestavení.</span><span class="sxs-lookup"><span data-stu-id="e380d-117">Added ef6.exe for enabling, adding, scripting, and applying migrations from assemblies.</span></span> <span data-ttu-id="e380d-118">Tím se nahradí soubor migrovat. exe.</span><span class="sxs-lookup"><span data-stu-id="e380d-118">This replaces migrate.exe</span></span>

## <a name="past-releases"></a><span data-ttu-id="e380d-119">Minulé verze</span><span class="sxs-lookup"><span data-stu-id="e380d-119">Past Releases</span></span>

<span data-ttu-id="e380d-120">Stránka [starší](past-releases.md) verze obsahuje archiv všech předchozích verzí EF a hlavní funkce, které byly představeny v každé vydané verzi.</span><span class="sxs-lookup"><span data-stu-id="e380d-120">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span>
