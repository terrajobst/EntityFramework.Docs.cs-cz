---
title: Co je nového – EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
ms.openlocfilehash: 9eb6a916d36ed41fcaea74564395695c048ab0f5
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996332"
---
# <a name="whats-new-in-ef6"></a><span data-ttu-id="6e7a5-102">Co je nového v EF6</span><span class="sxs-lookup"><span data-stu-id="6e7a5-102">What's New in EF6</span></span>

<span data-ttu-id="6e7a5-103">Důrazně doporučujeme používat nejnovější vydanou verzi sady Entity Framework k zajištění, že máte k dispozici nejnovější funkce a nejvyšší stabilitu.</span><span class="sxs-lookup"><span data-stu-id="6e7a5-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="6e7a5-104">Uvědomujeme si však, že budete možná muset použít předchozí verze, nebo můžete experimentovat s nových vylepšeních v nejnovější předběžnou verzi.</span><span class="sxs-lookup"><span data-stu-id="6e7a5-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="6e7a5-105">Pokud chcete nainstalovat konkrétní verze EF, naleznete v tématu [získat Entity Framework](~/ef6/fundamentals/install.md).</span><span class="sxs-lookup"><span data-stu-id="6e7a5-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

<span data-ttu-id="6e7a5-106">Tato stránka dokumenty funkce, které jsou zahrnuty v každé nové verze.</span><span class="sxs-lookup"><span data-stu-id="6e7a5-106">This page documents the features that are included on each new release.</span></span>

## <a name="recent-releases"></a><span data-ttu-id="6e7a5-107">Nedávno vydané aktualizace</span><span class="sxs-lookup"><span data-stu-id="6e7a5-107">Recent releases</span></span>

### <a name="ef-tools-update-in-visual-studio-2017-157"></a><span data-ttu-id="6e7a5-108">Aktualizace nástroje EF v sadě Visual Studio 2017 15.7</span><span class="sxs-lookup"><span data-stu-id="6e7a5-108">EF Tools Update in Visual Studio 2017 15.7</span></span>

<span data-ttu-id="6e7a5-109">V květnu 2018 jsme vydali aktualizovanou verzi nástroje EF jako součást Visual Studio 2017 15.7.</span><span class="sxs-lookup"><span data-stu-id="6e7a5-109">In May 2018, we released an updated version of the EF Tools as part of Visual Studio 2017 15.7.</span></span>
<span data-ttu-id="6e7a5-110">Obsahuje vylepšení pro některé běžné problémové body:</span><span class="sxs-lookup"><span data-stu-id="6e7a5-110">It includes improvements for some common pain points:</span></span>

- <span data-ttu-id="6e7a5-111">Opravy několika chyb usnadnění přístupu uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="6e7a5-111">Fixes for several user interface accessibility bugs</span></span>
- <span data-ttu-id="6e7a5-112">Alternativní řešení pro systém SQL Server snížení výkonu při generování modely z existujících databází [#4](https://github.com/aspnet/entityframework6/issues/4)</span><span class="sxs-lookup"><span data-stu-id="6e7a5-112">Workaround for SQL Server performance regression when generating models from existing databases [#4](https://github.com/aspnet/entityframework6/issues/4)</span></span>
- <span data-ttu-id="6e7a5-113">Podporu aktualizace modelů pro větší modely na serveru SQL Server [#185](https://github.com/aspnet/EntityFramework6/issues/185)</span><span class="sxs-lookup"><span data-stu-id="6e7a5-113">Support for updating models for larger models on SQL Server [#185](https://github.com/aspnet/EntityFramework6/issues/185)</span></span>

<span data-ttu-id="6e7a5-114">Další vylepšení v tomto této nové verzi nástroje EF je, že se nainstaluje modul runtime EF 6.2 při vytváření modelu v novém projektu.</span><span class="sxs-lookup"><span data-stu-id="6e7a5-114">Another improvement in this this new version of EF Tools is that it installs the EF 6.2 runtime when creating a model in a new project.</span></span> <span data-ttu-id="6e7a5-115">Ve starších verzích sady Visual Studio je možné pomocí nainstalovat odpovídající verzi balíčku NuGet EF 6.2 runtime (stejně jako všechny poslední verze EF).</span><span class="sxs-lookup"><span data-stu-id="6e7a5-115">With older versions of Visual Studio, it is possible to use the EF 6.2 runtime (as well as any past version of EF) by installing the corresponding version of the NuGet package.</span></span>

### <a name="ef-62-runtime"></a><span data-ttu-id="6e7a5-116">EF 6.2 Runtime</span><span class="sxs-lookup"><span data-stu-id="6e7a5-116">EF 6.2 Runtime</span></span>

<span data-ttu-id="6e7a5-117">NuGet EF 6.2 runtime byla vydána. října 2017.</span><span class="sxs-lookup"><span data-stu-id="6e7a5-117">The EF 6.2 runtime was released to NuGet in October of 2017.</span></span>
<span data-ttu-id="6e7a5-118">Děkujeme vám ve velké části úsilí naší komunity open source přispěvatele, EF 6.2 zahrnuje mnoho [opravy chyb](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) a [vylepšení produktu](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20).</span><span class="sxs-lookup"><span data-stu-id="6e7a5-118">Thanks in great part to the efforts our community of open source contributors, EF 6.2 includes numerous [bugs fixes](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) and [product enhancements](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20).</span></span>

<span data-ttu-id="6e7a5-119">Tady je stručný seznam vašich nejdůležitějších změn by to ovlivnilo EF 6.2 runtime:</span><span class="sxs-lookup"><span data-stu-id="6e7a5-119">Here is a brief list of the most important changes affecting the EF 6.2 runtime:</span></span>

- <span data-ttu-id="6e7a5-120">Načítají se snižuje doba spuštění dokončení první modely kódu z trvalá mezipaměť [#275](https://github.com/aspnet/EntityFramework6/issues/275)</span><span class="sxs-lookup"><span data-stu-id="6e7a5-120">Reduce start up time by loading finished code first models from a persistent cache [#275](https://github.com/aspnet/EntityFramework6/issues/275)</span></span>
- <span data-ttu-id="6e7a5-121">Rozhraní Fluent API definovat indexy [#274](https://github.com/aspnet/EntityFramework6/issues/274)</span><span class="sxs-lookup"><span data-stu-id="6e7a5-121">Fluent API to define indexes [#274](https://github.com/aspnet/EntityFramework6/issues/274)</span></span>
- <span data-ttu-id="6e7a5-122">DbFunctions.Like() povolit zápis dotazů LINQ, které přeložit jako v SQL [#241](https://github.com/aspnet/EntityFramework6/issues/241)</span><span class="sxs-lookup"><span data-stu-id="6e7a5-122">DbFunctions.Like() to enable writing LINQ queries that translate to LIKE in SQL [#241](https://github.com/aspnet/EntityFramework6/issues/241)</span></span>
- <span data-ttu-id="6e7a5-123">Migrate.exe teď podporuje – možnost skript [#240](https://github.com/aspnet/EntityFramework6/issues/240)</span><span class="sxs-lookup"><span data-stu-id="6e7a5-123">Migrate.exe now supports -script option [#240](https://github.com/aspnet/EntityFramework6/issues/240)</span></span>
- <span data-ttu-id="6e7a5-124">EF6 teď můžete pracovat s hodnoty klíče, které jsou generovány podle pořadí v systému SQL Server [#165](https://github.com/aspnet/EntityFramework6/issues/165)</span><span class="sxs-lookup"><span data-stu-id="6e7a5-124">EF6 can now work with key values generated by a sequence in SQL Server [#165](https://github.com/aspnet/EntityFramework6/issues/165)</span></span>
- <span data-ttu-id="6e7a5-125">Aktualizace seznamu přechodných chyb strategie provádění SQL Azure [#83](https://github.com/aspnet/EntityFramework6/issues/83)</span><span class="sxs-lookup"><span data-stu-id="6e7a5-125">Update list of transient errors for SQL Azure Execution Strategy [#83](https://github.com/aspnet/EntityFramework6/issues/83)</span></span>
- <span data-ttu-id="6e7a5-126">Chyba: Opakování dotazy nebo příkazy SQL selže s "SqlParameter je již obsažen v jiné SqlParameterCollection" [#81](https://github.com/aspnet/EntityFramework6/issues/81)</span><span class="sxs-lookup"><span data-stu-id="6e7a5-126">Bug: Retrying queries or SQL commands fails with "The SqlParameter is already contained by another SqlParameterCollection" [#81](https://github.com/aspnet/EntityFramework6/issues/81)</span></span>
- <span data-ttu-id="6e7a5-127">Chyba: Vyhodnocení DbQuery.ToString() často vyprší časový limit v ladicím programu [#73](https://github.com/aspnet/EntityFramework6/issues/73)</span><span class="sxs-lookup"><span data-stu-id="6e7a5-127">Bug: Evaluation of DbQuery.ToString() frequently times out in the debugger [#73](https://github.com/aspnet/EntityFramework6/issues/73)</span></span>

## <a name="future-releases"></a><span data-ttu-id="6e7a5-128">Budoucí verze</span><span class="sxs-lookup"><span data-stu-id="6e7a5-128">Future Releases</span></span>

<span data-ttu-id="6e7a5-129">Informace v budoucí verzi systému EF6, podívejte se na naše [plán](roadmap.md).</span><span class="sxs-lookup"><span data-stu-id="6e7a5-129">For information on future version of EF6, please look at our [Roadmap](roadmap.md).</span></span>

## <a name="past-releases"></a><span data-ttu-id="6e7a5-130">Minulých verzích</span><span class="sxs-lookup"><span data-stu-id="6e7a5-130">Past Releases</span></span>

<span data-ttu-id="6e7a5-131">[Minulých vydaných verzích](past-releases.md) stránka obsahuje archiv všechny předchozí verze EF a hlavní funkce, které byly zavedeny v jednotlivých verzích.</span><span class="sxs-lookup"><span data-stu-id="6e7a5-131">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span>
