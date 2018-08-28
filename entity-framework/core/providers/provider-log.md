---
title: Protokol změn vliv na poskytovatele – EF Core
author: ajcvickers
ms.author: avickers
ms.date: 08/08/2018
ms.assetid: 7CEF496E-A5B0-4F5F-B68E-529609B23EF9
ms.technology: entity-framework-core
uid: core/providers/provider-log
ms.openlocfilehash: ee73940e3c0030b76e73438b1852cc29ebeadb45
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998363"
---
# <a name="provider-impacting-changes"></a><span data-ttu-id="ce3e1-102">Změny s dopadem na poskytovatele</span><span class="sxs-lookup"><span data-stu-id="ce3e1-102">Provider-impacting changes</span></span>

<span data-ttu-id="ce3e1-103">Tato stránka obsahuje odkazy na EF Core úložiště, které můžou vyžadovat autoři ostatní poskytovatelé databází reagovat žádostmi o přijetí změn.</span><span class="sxs-lookup"><span data-stu-id="ce3e1-103">This page contains links to pull requests made on the EF Core repo that may require authors of other database providers to react.</span></span> <span data-ttu-id="ce3e1-104">Záměrem je poskytnout výchozí bod pro autory existující poskytovatelé databází výrobců při aktualizaci jejich zprostředkovatele na novou verzi.</span><span class="sxs-lookup"><span data-stu-id="ce3e1-104">The intention is to provide a starting point for authors of existing third-party database providers when updating their provider to a new version.</span></span>

<span data-ttu-id="ce3e1-105">Začínáme tento protokol se změnami z 2.1 2.2.</span><span class="sxs-lookup"><span data-stu-id="ce3e1-105">We are starting this log with changes from 2.1 to 2.2.</span></span> <span data-ttu-id="ce3e1-106">Před 2.1 jsme použili [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) a [ `providers-fyi` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) popisky na naše problémy a žádosti o přijetí změn.</span><span class="sxs-lookup"><span data-stu-id="ce3e1-106">Prior to 2.1 we used the [`providers-beware`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) and [`providers-fyi`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) labels on our issues and pull requests.</span></span>

### <a name="21-----22"></a><span data-ttu-id="ce3e1-107">2.1---> 2.2</span><span class="sxs-lookup"><span data-stu-id="ce3e1-107">2.1 ---> 2.2</span></span>

#### <a name="test-only-changes"></a><span data-ttu-id="ce3e1-108">Změny jen pro test</span><span class="sxs-lookup"><span data-stu-id="ce3e1-108">Test-only changes</span></span>

* <span data-ttu-id="ce3e1-109">https://github.com/aspnet/EntityFrameworkCore/pull/12057 – Povolte přizpůsobitelné dají SQL v testech</span><span class="sxs-lookup"><span data-stu-id="ce3e1-109">https://github.com/aspnet/EntityFrameworkCore/pull/12057 - Allow customizable SQL delimeters in tests</span></span>
  * <span data-ttu-id="ce3e1-110">Otestovat změny, které umožňují nepřísném plovoucího bodu porovnání v BuiltInDataTypesTestBase</span><span class="sxs-lookup"><span data-stu-id="ce3e1-110">Test changes that allow non-strict floating point comparisons in BuiltInDataTypesTestBase</span></span>
  * <span data-ttu-id="ce3e1-111">Test změny, které umožňují testů dotaz znovu použít se dají jiný SQL</span><span class="sxs-lookup"><span data-stu-id="ce3e1-111">Test changes that allow query tests to be re-used with different SQL delimeters</span></span>
* <span data-ttu-id="ce3e1-112">https://github.com/aspnet/EntityFrameworkCore/pull/12072 -Přidáte DbFunction testy do relační specifikace testů</span><span class="sxs-lookup"><span data-stu-id="ce3e1-112">https://github.com/aspnet/EntityFrameworkCore/pull/12072 - Add DbFunction tests to the relational specification tests</span></span>
  * <span data-ttu-id="ce3e1-113">Tak, aby se dají spustit tyto testy pro všechny poskytovatele databáze</span><span class="sxs-lookup"><span data-stu-id="ce3e1-113">Such that these tests can be run against all database providers</span></span>
* <span data-ttu-id="ce3e1-114">https://github.com/aspnet/EntityFrameworkCore/pull/12362 -Vyčištění testovacího asynchronní</span><span class="sxs-lookup"><span data-stu-id="ce3e1-114">https://github.com/aspnet/EntityFrameworkCore/pull/12362 - Async test cleanup</span></span>
  * <span data-ttu-id="ce3e1-115">Odebrat `Wait` nepotřebné asynchronní volání a přejmenovat některé testovací metody</span><span class="sxs-lookup"><span data-stu-id="ce3e1-115">Remove `Wait` calls, unneeded async, and renamed some test methods</span></span>
* <span data-ttu-id="ce3e1-116">https://github.com/aspnet/EntityFrameworkCore/pull/12666 -Sjednocení protokolování testovací infrastrukturu</span><span class="sxs-lookup"><span data-stu-id="ce3e1-116">https://github.com/aspnet/EntityFrameworkCore/pull/12666 - Unify logging test infrastructure</span></span>
  * <span data-ttu-id="ce3e1-117">Přidání `CreateListLoggerFactory` a odebrat některé předchozí infrastruktury protokolování, který bude vyžadovat zprostředkovatelů pomocí tyto testy reagovat</span><span class="sxs-lookup"><span data-stu-id="ce3e1-117">Added `CreateListLoggerFactory` and removed some previous logging infrastructure, which will require providers using these tests to react</span></span>
* <span data-ttu-id="ce3e1-118">https://github.com/aspnet/EntityFrameworkCore/pull/12500 -Spuštění více testů dotazu synchronně i asynchronně</span><span class="sxs-lookup"><span data-stu-id="ce3e1-118">https://github.com/aspnet/EntityFrameworkCore/pull/12500 - Run more query tests both synchronously and asynchronously</span></span>
  * <span data-ttu-id="ce3e1-119">Názvy testů a které budou zohledňovat došlo ke změně, což bude vyžadovat zprostředkovatelů pomocí tyto testy reagovat</span><span class="sxs-lookup"><span data-stu-id="ce3e1-119">Test names and factoring has changed, which will require providers using these tests to react</span></span>
* <span data-ttu-id="ce3e1-120">https://github.com/aspnet/EntityFrameworkCore/pull/12766 -Přejmenování navigaci v modelu ComplexNavigations</span><span class="sxs-lookup"><span data-stu-id="ce3e1-120">https://github.com/aspnet/EntityFrameworkCore/pull/12766 - Renaming navigations in the ComplexNavigations model</span></span>
  * <span data-ttu-id="ce3e1-121">Možná bude nutné zprostředkovatelů tyto testy pomocí react</span><span class="sxs-lookup"><span data-stu-id="ce3e1-121">Providers using these tests may need to react</span></span>
* <span data-ttu-id="ce3e1-122">https://github.com/aspnet/EntityFrameworkCore/pull/12141 -Vrátíte kontext do fondu namísto disposing v funkční testy</span><span class="sxs-lookup"><span data-stu-id="ce3e1-122">https://github.com/aspnet/EntityFrameworkCore/pull/12141 - Return the context to the pool instead of disposing in functional tests</span></span>
  * <span data-ttu-id="ce3e1-123">Tato změna zahrnuje některé refaktoring testů, může být nutné poskytovatelé react</span><span class="sxs-lookup"><span data-stu-id="ce3e1-123">This change includes some test refactoring which may require providers to react</span></span>


#### <a name="test-and-product-code-changes"></a><span data-ttu-id="ce3e1-124">Změny kódu testu a produktu</span><span class="sxs-lookup"><span data-stu-id="ce3e1-124">Test and product code changes</span></span>

* <span data-ttu-id="ce3e1-125">https://github.com/aspnet/EntityFrameworkCore/pull/12109 -Konsolidovat RelationalTypeMapping.Clone metody</span><span class="sxs-lookup"><span data-stu-id="ce3e1-125">https://github.com/aspnet/EntityFrameworkCore/pull/12109 - Consolidate RelationalTypeMapping.Clone methods</span></span>
  * <span data-ttu-id="ce3e1-126">Změny v 2.1, aby RelationalTypeMapping povolené pro zjednodušení v odvozených třídách.</span><span class="sxs-lookup"><span data-stu-id="ce3e1-126">Changes in 2.1 to the RelationalTypeMapping allowed for a simplification in derived classes.</span></span> <span data-ttu-id="ce3e1-127">Nevěříme to byla rozbíjející poskytovatelů, ale zprostředkovatelé můžete využít výhod této změny v jejich odvozený typ mapování třídy.</span><span class="sxs-lookup"><span data-stu-id="ce3e1-127">We don't believe this was breaking to providers, but providers can take advantage of this change in their derived type mapping classes.</span></span>
* <span data-ttu-id="ce3e1-128">https://github.com/aspnet/EntityFrameworkCore/pull/12069 -Označených nebo pojmenovaných dotazů</span><span class="sxs-lookup"><span data-stu-id="ce3e1-128">https://github.com/aspnet/EntityFrameworkCore/pull/12069 - Tagged or named queries</span></span>
  * <span data-ttu-id="ce3e1-129">Přidá infrastruktury pro označování dotazů LINQ a mají tyto visačky se zobrazují jako komentáře v SQL.</span><span class="sxs-lookup"><span data-stu-id="ce3e1-129">Adds infrastructure for tagging LINQ queries and having those tags show up as comments in the SQL.</span></span> <span data-ttu-id="ce3e1-130">To může vyžadovat poskytovatelé reagovat v generování SQL.</span><span class="sxs-lookup"><span data-stu-id="ce3e1-130">This may require providers to react in SQL generation.</span></span>
