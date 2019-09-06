---
title: Co je nového – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
ms.openlocfilehash: 01dc618954da5dbd12fbd37c2c47701ce251be92
ms.sourcegitcommit: 0cc9578fd49802789a00c0044b4e57325476ca2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/04/2019
ms.locfileid: "70271441"
---
# <a name="whats-new-in-ef6"></a><span data-ttu-id="6eaea-102">Co je nového v EF6</span><span class="sxs-lookup"><span data-stu-id="6eaea-102">What's New in EF6</span></span>

<span data-ttu-id="6eaea-103">Důrazně doporučujeme, abyste používali nejnovější vydanou verzi Entity Framework, abyste měli jistotu, že získáte nejnovější funkce a nejvyšší stabilitu.</span><span class="sxs-lookup"><span data-stu-id="6eaea-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="6eaea-104">Uvědomujeme si ale, že možná budete muset použít předchozí verzi nebo můžete chtít experimentovat s novými vylepšeními v nejnovější předběžné verzi.</span><span class="sxs-lookup"><span data-stu-id="6eaea-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="6eaea-105">Chcete-li nainstalovat konkrétní verze EF, přečtěte si téma [Get Entity Framework](~/ef6/fundamentals/install.md).</span><span class="sxs-lookup"><span data-stu-id="6eaea-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

<span data-ttu-id="6eaea-106">Tato stránka obsahuje funkce, které jsou součástí každé nové vydané verze.</span><span class="sxs-lookup"><span data-stu-id="6eaea-106">This page documents the features that are included on each new release.</span></span>

## <a name="recent-releases"></a><span data-ttu-id="6eaea-107">Poslední verze</span><span class="sxs-lookup"><span data-stu-id="6eaea-107">Recent releases</span></span>

### <a name="ef-tools-update-in-visual-studio-2017-157"></a><span data-ttu-id="6eaea-108">Aktualizace nástrojů EF v aplikaci Visual Studio 2017 15,7</span><span class="sxs-lookup"><span data-stu-id="6eaea-108">EF Tools Update in Visual Studio 2017 15.7</span></span>

<span data-ttu-id="6eaea-109">V květnu 2018 jsme vydali aktualizovanou verzi nástrojů EF jako součást sady Visual Studio 2017 15,7.</span><span class="sxs-lookup"><span data-stu-id="6eaea-109">In May 2018, we released an updated version of the EF Tools as part of Visual Studio 2017 15.7.</span></span>
<span data-ttu-id="6eaea-110">Obsahuje vylepšení některých běžných častých bodů:</span><span class="sxs-lookup"><span data-stu-id="6eaea-110">It includes improvements for some common pain points:</span></span>

- <span data-ttu-id="6eaea-111">Opravy pro několik chyb usnadnění uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="6eaea-111">Fixes for several user interface accessibility bugs</span></span>
- <span data-ttu-id="6eaea-112">Alternativní řešení pro SQL Server regresi výkonu při generování modelů z existujících databází [#4](https://github.com/aspnet/entityframework6/issues/4)</span><span class="sxs-lookup"><span data-stu-id="6eaea-112">Workaround for SQL Server performance regression when generating models from existing databases [#4](https://github.com/aspnet/entityframework6/issues/4)</span></span>
- <span data-ttu-id="6eaea-113">Podpora pro aktualizace modelů pro větší modely na SQL Server [#185](https://github.com/aspnet/EntityFramework6/issues/185)</span><span class="sxs-lookup"><span data-stu-id="6eaea-113">Support for updating models for larger models on SQL Server [#185](https://github.com/aspnet/EntityFramework6/issues/185)</span></span>

<span data-ttu-id="6eaea-114">Dalším vylepšením v této nové verzi nástrojů EF je, že při vytváření modelu v novém projektu nainstaluje modul runtime EF 6,2.</span><span class="sxs-lookup"><span data-stu-id="6eaea-114">Another improvement in this new version of EF Tools is that it installs the EF 6.2 runtime when creating a model in a new project.</span></span> <span data-ttu-id="6eaea-115">Se staršími verzemi sady Visual Studio je možné použít modul runtime EF 6,2 (stejně jako všechny dřívější verze EF) instalací odpovídající verze balíčku NuGet.</span><span class="sxs-lookup"><span data-stu-id="6eaea-115">With older versions of Visual Studio, it is possible to use the EF 6.2 runtime (as well as any past version of EF) by installing the corresponding version of the NuGet package.</span></span>

### <a name="ef-62-runtime"></a><span data-ttu-id="6eaea-116">Modul runtime EF 6,2</span><span class="sxs-lookup"><span data-stu-id="6eaea-116">EF 6.2 Runtime</span></span>

<span data-ttu-id="6eaea-117">Modul runtime EF 6,2 byl vydán do NuGet v říjnu 2017.</span><span class="sxs-lookup"><span data-stu-id="6eaea-117">The EF 6.2 runtime was released to NuGet in October of 2017.</span></span>
<span data-ttu-id="6eaea-118">Děkujeme, že v naší komunitě otevřených přispěvatelů Open sources je naše komunita, EF 6,2 obsahuje řadu [chybových oprav](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) a [vylepšení produktů](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20).</span><span class="sxs-lookup"><span data-stu-id="6eaea-118">Thanks in great part to the efforts our community of open source contributors, EF 6.2 includes numerous [bugs fixes](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) and [product enhancements](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20).</span></span>

<span data-ttu-id="6eaea-119">Tady je stručný seznam nejdůležitějších změn, které mají vliv na modul runtime EF 6,2:</span><span class="sxs-lookup"><span data-stu-id="6eaea-119">Here is a brief list of the most important changes affecting the EF 6.2 runtime:</span></span>

- <span data-ttu-id="6eaea-120">Snižte čas spuštění načtením dokončeného kódu první modely z trvalé mezipaměti [#275](https://github.com/aspnet/EntityFramework6/issues/275)</span><span class="sxs-lookup"><span data-stu-id="6eaea-120">Reduce start up time by loading finished code first models from a persistent cache [#275](https://github.com/aspnet/EntityFramework6/issues/275)</span></span>
- <span data-ttu-id="6eaea-121">Rozhraní Fluent API k definování indexů [#274](https://github.com/aspnet/EntityFramework6/issues/274)</span><span class="sxs-lookup"><span data-stu-id="6eaea-121">Fluent API to define indexes [#274](https://github.com/aspnet/EntityFramework6/issues/274)</span></span>
- <span data-ttu-id="6eaea-122">DbFunctions. like () pro povolení psaní dotazů LINQ, které se překládají jako v SQL [#241](https://github.com/aspnet/EntityFramework6/issues/241)</span><span class="sxs-lookup"><span data-stu-id="6eaea-122">DbFunctions.Like() to enable writing LINQ queries that translate to LIKE in SQL [#241](https://github.com/aspnet/EntityFramework6/issues/241)</span></span>
- <span data-ttu-id="6eaea-123">Migruje. exe teď podporuje-možnost skriptu [#240](https://github.com/aspnet/EntityFramework6/issues/240)</span><span class="sxs-lookup"><span data-stu-id="6eaea-123">Migrate.exe now supports -script option [#240](https://github.com/aspnet/EntityFramework6/issues/240)</span></span>
- <span data-ttu-id="6eaea-124">EF6 může nyní pracovat s hodnotami klíčů generovanými sekvencí v SQL Server [#165](https://github.com/aspnet/EntityFramework6/issues/165)</span><span class="sxs-lookup"><span data-stu-id="6eaea-124">EF6 can now work with key values generated by a sequence in SQL Server [#165](https://github.com/aspnet/EntityFramework6/issues/165)</span></span>
- <span data-ttu-id="6eaea-125">Aktualizace seznamu přechodných chyb pro SQL Azure strategii provádění [#83](https://github.com/aspnet/EntityFramework6/issues/83)</span><span class="sxs-lookup"><span data-stu-id="6eaea-125">Update list of transient errors for SQL Azure Execution Strategy [#83](https://github.com/aspnet/EntityFramework6/issues/83)</span></span>
- <span data-ttu-id="6eaea-126">Problém Opakované pokusy dotazů nebo příkazů SQL selžou s "rozhraní SqlParameter je již obsaženo v jiném SqlParameterCollection" [#81](https://github.com/aspnet/EntityFramework6/issues/81)</span><span class="sxs-lookup"><span data-stu-id="6eaea-126">Bug: Retrying queries or SQL commands fails with "The SqlParameter is already contained by another SqlParameterCollection" [#81](https://github.com/aspnet/EntityFramework6/issues/81)</span></span>
- <span data-ttu-id="6eaea-127">Problém Vyhodnocování DbQuery. ToString () je v ladicím programu často vyprší [#73](https://github.com/aspnet/EntityFramework6/issues/73)</span><span class="sxs-lookup"><span data-stu-id="6eaea-127">Bug: Evaluation of DbQuery.ToString() frequently times out in the debugger [#73](https://github.com/aspnet/EntityFramework6/issues/73)</span></span>

## <a name="future-releases"></a><span data-ttu-id="6eaea-128">Budoucí verze</span><span class="sxs-lookup"><span data-stu-id="6eaea-128">Future Releases</span></span>

<span data-ttu-id="6eaea-129">Informace o budoucích verzích EF6 najdete [v našem plánu](roadmap.md).</span><span class="sxs-lookup"><span data-stu-id="6eaea-129">For information on future version of EF6, please look at our [Roadmap](roadmap.md).</span></span>

## <a name="past-releases"></a><span data-ttu-id="6eaea-130">Minulé verze</span><span class="sxs-lookup"><span data-stu-id="6eaea-130">Past Releases</span></span>

<span data-ttu-id="6eaea-131">Stránka [starší](past-releases.md) verze obsahuje archiv všech předchozích verzí EF a hlavní funkce, které byly představeny v každé vydané verzi.</span><span class="sxs-lookup"><span data-stu-id="6eaea-131">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span>
