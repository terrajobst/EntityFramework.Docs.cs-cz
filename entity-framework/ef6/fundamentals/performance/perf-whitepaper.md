---
title: Požadavky na výkon pro EF4, EF5 a EF6-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d6d5a465-6434-45fa-855d-5eb48c61a2ea
ms.openlocfilehash: 07eb605f0d39f0c1bcfe781540525180f0dd0b22
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181676"
---
# <a name="performance-considerations-for-ef-4-5-and-6"></a><span data-ttu-id="16dd5-102">Požadavky na výkon pro EF 4, 5 a 6</span><span class="sxs-lookup"><span data-stu-id="16dd5-102">Performance considerations for EF 4, 5, and 6</span></span>
<span data-ttu-id="16dd5-103">Autorem David Obando, Eric Dettinger a ostatními</span><span class="sxs-lookup"><span data-stu-id="16dd5-103">By David Obando, Eric Dettinger and others</span></span>

<span data-ttu-id="16dd5-104">Zveřejněna Duben 2012</span><span class="sxs-lookup"><span data-stu-id="16dd5-104">Published: April 2012</span></span>

<span data-ttu-id="16dd5-105">Poslední aktualizace: Květen 2014</span><span class="sxs-lookup"><span data-stu-id="16dd5-105">Last updated: May 2014</span></span>

------------------------------------------------------------------------

## <a name="1-introduction"></a><span data-ttu-id="16dd5-106">1. Úvod</span><span class="sxs-lookup"><span data-stu-id="16dd5-106">1. Introduction</span></span>

<span data-ttu-id="16dd5-107">Objektově-relační rozhraní mapování jsou pohodlným způsobem, jak poskytnout abstrakci pro přístup k datům v objektově orientované aplikaci.</span><span class="sxs-lookup"><span data-stu-id="16dd5-107">Object-Relational Mapping frameworks are a convenient way to provide an abstraction for data access in an object-oriented application.</span></span> <span data-ttu-id="16dd5-108">V případě aplikací .NET je Entity Framework doporučena technologie Microsoft pro/RM.</span><span class="sxs-lookup"><span data-stu-id="16dd5-108">For .NET applications, Microsoft's recommended O/RM is Entity Framework.</span></span> <span data-ttu-id="16dd5-109">V případě jakékoli abstrakce se může výkon stát problémem.</span><span class="sxs-lookup"><span data-stu-id="16dd5-109">With any abstraction though, performance can become a concern.</span></span>

<span data-ttu-id="16dd5-110">Tento dokument White Paper byl napsán tak, aby zobrazoval požadavky na výkon při vývoji aplikací pomocí Entity Framework, aby vývojáři poskytovali představu o Entity Frameworkch vnitřních algoritmech, které mohou ovlivnit výkon, a poskytnout tipy k vyšetřování a zlepšení výkonu ve svých aplikacích, které používají Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="16dd5-110">This whitepaper was written to show the performance considerations when developing applications using Entity Framework, to give developers an idea of the Entity Framework internal algorithms that can affect performance, and to provide tips for investigation and improving performance in their applications that use Entity Framework.</span></span> <span data-ttu-id="16dd5-111">K dispozici je řada dobrých témat o výkonu, které jsou již na webu k dispozici, a také jsme se na tyto prostředky snažili odkazovat.</span><span class="sxs-lookup"><span data-stu-id="16dd5-111">There are a number of good topics on performance already available on the web, and we've also tried pointing to these resources where possible.</span></span>

<span data-ttu-id="16dd5-112">Výkon je obtížné téma.</span><span class="sxs-lookup"><span data-stu-id="16dd5-112">Performance is a tricky topic.</span></span> <span data-ttu-id="16dd5-113">Tento dokument White Paper je určený jako prostředek, který vám může usnadnit rozhodování související s výkonem pro vaše aplikace, které používají Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="16dd5-113">This whitepaper is intended as a resource to help you make performance related decisions for your applications that use Entity Framework.</span></span> <span data-ttu-id="16dd5-114">Zahrnuli jsme několik testovacích metrik, které demonstrují výkon, ale tyto metriky nejsou určené jako absolutní indikátory výkonu, které se zobrazí ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="16dd5-114">We have included some test metrics to demonstrate performance, but these metrics aren't intended as absolute indicators of the performance you will see in your application.</span></span>

<span data-ttu-id="16dd5-115">Pro praktické účely tento dokument předpokládá, Entity Framework 4 je spuštěný v rozhraní .NET 4,0 a Entity Framework 5 a 6 se spouští pod .NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="16dd5-115">For practical purposes, this document assumes Entity Framework 4 is run under .NET 4.0 and Entity Framework 5 and 6 are run under .NET 4.5.</span></span> <span data-ttu-id="16dd5-116">Mnohé z vylepšení výkonu pro Entity Framework 5 se nacházejí v rámci základních komponent, které se dodávají s .NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="16dd5-116">Many of the performance improvements made for Entity Framework 5 reside within the core components that ship with .NET 4.5.</span></span>

<span data-ttu-id="16dd5-117">Entity Framework 6 je verze mimo IP síť a nezávisí na Entity Frameworkch součástech, které se dodávají s .NET.</span><span class="sxs-lookup"><span data-stu-id="16dd5-117">Entity Framework 6 is an out of band release and does not depend on the Entity Framework components that ship with .NET.</span></span> <span data-ttu-id="16dd5-118">Entity Framework 6 funguje na rozhraní .NET 4,0 i .NET 4,5 a může nabídnout velký výkon pro uživatele, kteří se neupgradovali z .NET 4,0, ale chtějí ve svých aplikacích využívat nejnovější Entity Framework bity.</span><span class="sxs-lookup"><span data-stu-id="16dd5-118">Entity Framework 6 work on both .NET 4.0 and .NET 4.5, and can offer a big performance benefit to those who haven’t upgraded from .NET 4.0 but want the latest Entity Framework bits in their application.</span></span> <span data-ttu-id="16dd5-119">Pokud se tento dokument zmiňuje Entity Framework 6, odkazuje na nejnovější verzi, která je k dispozici v době psaní tohoto dokumentu: verze 6.1.0.</span><span class="sxs-lookup"><span data-stu-id="16dd5-119">When this document mentions Entity Framework 6, it refers to the latest version available at the time of this writing: version 6.1.0.</span></span>

## <a name="2-cold-vs-warm-query-execution"></a><span data-ttu-id="16dd5-120">2. Studená vs. Teplé provádění dotazů</span><span class="sxs-lookup"><span data-stu-id="16dd5-120">2. Cold vs. Warm Query Execution</span></span>

<span data-ttu-id="16dd5-121">Velmi poprvé se každý dotaz provede proti danému modelu, Entity Framework provede spoustu práce na pozadí pro načtení a ověření modelu.</span><span class="sxs-lookup"><span data-stu-id="16dd5-121">The very first time any query is made against a given model, the Entity Framework does a lot of work behind the scenes to load and validate the model.</span></span> <span data-ttu-id="16dd5-122">Často odkazujeme na tento první dotaz jako na studený dotaz.</span><span class="sxs-lookup"><span data-stu-id="16dd5-122">We frequently refer to this first query as a "cold" query.</span></span><span data-ttu-id="16dd5-123">  Další dotazy na již načtený model jsou označovány jako "teplé" dotazy a jsou mnohem rychlejší.</span><span class="sxs-lookup"><span data-stu-id="16dd5-123">  Further queries against an already loaded model are known as "warm" queries, and are much faster.</span></span>

<span data-ttu-id="16dd5-124">Pojďme se podívat na podrobný pohled na čas strávený prováděním dotazu pomocí Entity Framework a zjistit, kde se vylepšit v Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="16dd5-124">Let’s take a high-level view of where time is spent when executing a query using Entity Framework, and see where things are improving in Entity Framework 6.</span></span>

<span data-ttu-id="16dd5-125">**První spuštění dotazu – studený dotaz**</span><span class="sxs-lookup"><span data-stu-id="16dd5-125">**First Query Execution – cold query**</span></span>

| <span data-ttu-id="16dd5-126">Kódování uživatelských zápisů</span><span class="sxs-lookup"><span data-stu-id="16dd5-126">Code User Writes</span></span>                                                                                     | <span data-ttu-id="16dd5-127">Action</span><span class="sxs-lookup"><span data-stu-id="16dd5-127">Action</span></span>                    | <span data-ttu-id="16dd5-128">Dopad na výkon EF4</span><span class="sxs-lookup"><span data-stu-id="16dd5-128">EF4 Performance Impact</span></span>                                                                                                                                                                                                                                                                                                                                                                                                        | <span data-ttu-id="16dd5-129">Dopad na výkon EF5</span><span class="sxs-lookup"><span data-stu-id="16dd5-129">EF5 Performance Impact</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                    | <span data-ttu-id="16dd5-130">Dopad na výkon EF6</span><span class="sxs-lookup"><span data-stu-id="16dd5-130">EF6 Performance Impact</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|:-----------------------------------------------------------------------------------------------------|:--------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `using(var db = new MyContext())` <br/> `{`                                                          | <span data-ttu-id="16dd5-131">Vytvoření kontextu</span><span class="sxs-lookup"><span data-stu-id="16dd5-131">Context creation</span></span>          | <span data-ttu-id="16dd5-132">Střední</span><span class="sxs-lookup"><span data-stu-id="16dd5-132">Medium</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                        | <span data-ttu-id="16dd5-133">Střední</span><span class="sxs-lookup"><span data-stu-id="16dd5-133">Medium</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | <span data-ttu-id="16dd5-134">Nízká</span><span class="sxs-lookup"><span data-stu-id="16dd5-134">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `  var q1 = ` <br/> `    from c in db.Customers` <br/> `    where c.Id == id1` <br/> `    select c;` | <span data-ttu-id="16dd5-135">Vytvoření výrazu dotazu</span><span class="sxs-lookup"><span data-stu-id="16dd5-135">Query expression creation</span></span> | <span data-ttu-id="16dd5-136">Nízká</span><span class="sxs-lookup"><span data-stu-id="16dd5-136">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                           | <span data-ttu-id="16dd5-137">Nízká</span><span class="sxs-lookup"><span data-stu-id="16dd5-137">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="16dd5-138">Nízká</span><span class="sxs-lookup"><span data-stu-id="16dd5-138">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `  var c1 = q1.First();`                                                                             | <span data-ttu-id="16dd5-139">Provádění dotazů LINQ</span><span class="sxs-lookup"><span data-stu-id="16dd5-139">LINQ query execution</span></span>      | <span data-ttu-id="16dd5-140">-Načítání metadat: Vysoká, ale v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="16dd5-140">- Metadata loading: High but cached</span></span> <br/> <span data-ttu-id="16dd5-141">– Generování zobrazení: Potenciálně velmi vysoké, ale v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="16dd5-141">- View generation: Potentially very high but cached</span></span> <br/> <span data-ttu-id="16dd5-142">-Vyhodnocení parametrů: Střední</span><span class="sxs-lookup"><span data-stu-id="16dd5-142">- Parameter evaluation: Medium</span></span> <br/> <span data-ttu-id="16dd5-143">-Překlad dotazů: Střední</span><span class="sxs-lookup"><span data-stu-id="16dd5-143">- Query translation: Medium</span></span> <br/> <span data-ttu-id="16dd5-144">-Materializer generace: Střední, ale v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="16dd5-144">- Materializer generation: Medium but cached</span></span> <br/> <span data-ttu-id="16dd5-145">-Spuštění databázového dotazu: Potenciálně vysoký</span><span class="sxs-lookup"><span data-stu-id="16dd5-145">- Database query execution: Potentially high</span></span> <br/> <span data-ttu-id="16dd5-146">+ Připojení. otevřít</span><span class="sxs-lookup"><span data-stu-id="16dd5-146">+ Connection.Open</span></span> <br/> <span data-ttu-id="16dd5-147">+ Command. ExecuteReader</span><span class="sxs-lookup"><span data-stu-id="16dd5-147">+ Command.ExecuteReader</span></span> <br/> <span data-ttu-id="16dd5-148">+ DataReader. Read</span><span class="sxs-lookup"><span data-stu-id="16dd5-148">+ DataReader.Read</span></span> <br/> <span data-ttu-id="16dd5-149">Materializace objektu: Střední</span><span class="sxs-lookup"><span data-stu-id="16dd5-149">Object materialization: Medium</span></span> <br/> <span data-ttu-id="16dd5-150">-Vyhledávání identity: Střední</span><span class="sxs-lookup"><span data-stu-id="16dd5-150">- Identity lookup: Medium</span></span> | <span data-ttu-id="16dd5-151">-Načítání metadat: Vysoká, ale v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="16dd5-151">- Metadata loading: High but cached</span></span> <br/> <span data-ttu-id="16dd5-152">– Generování zobrazení: Potenciálně velmi vysoké, ale v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="16dd5-152">- View generation: Potentially very high but cached</span></span> <br/> <span data-ttu-id="16dd5-153">-Vyhodnocení parametrů: Nízká</span><span class="sxs-lookup"><span data-stu-id="16dd5-153">- Parameter evaluation: Low</span></span> <br/> <span data-ttu-id="16dd5-154">-Překlad dotazů: Střední, ale v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="16dd5-154">- Query translation: Medium but cached</span></span> <br/> <span data-ttu-id="16dd5-155">-Materializer generace: Střední, ale v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="16dd5-155">- Materializer generation: Medium but cached</span></span> <br/> <span data-ttu-id="16dd5-156">-Spuštění databázového dotazu: Potenciálně vysoké (lepší dotazy v některých situacích)</span><span class="sxs-lookup"><span data-stu-id="16dd5-156">- Database query execution: Potentially high (Better queries in some situations)</span></span> <br/> <span data-ttu-id="16dd5-157">+ Připojení. otevřít</span><span class="sxs-lookup"><span data-stu-id="16dd5-157">+ Connection.Open</span></span> <br/> <span data-ttu-id="16dd5-158">+ Command. ExecuteReader</span><span class="sxs-lookup"><span data-stu-id="16dd5-158">+ Command.ExecuteReader</span></span> <br/> <span data-ttu-id="16dd5-159">+ DataReader. Read</span><span class="sxs-lookup"><span data-stu-id="16dd5-159">+ DataReader.Read</span></span> <br/> <span data-ttu-id="16dd5-160">Materializace objektu: Střední</span><span class="sxs-lookup"><span data-stu-id="16dd5-160">Object materialization: Medium</span></span> <br/> <span data-ttu-id="16dd5-161">-Vyhledávání identity: Střední</span><span class="sxs-lookup"><span data-stu-id="16dd5-161">- Identity lookup: Medium</span></span> | <span data-ttu-id="16dd5-162">-Načítání metadat: Vysoká, ale v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="16dd5-162">- Metadata loading: High but cached</span></span> <br/> <span data-ttu-id="16dd5-163">– Generování zobrazení: Střední, ale v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="16dd5-163">- View generation: Medium but cached</span></span> <br/> <span data-ttu-id="16dd5-164">-Vyhodnocení parametrů: Nízká</span><span class="sxs-lookup"><span data-stu-id="16dd5-164">- Parameter evaluation: Low</span></span> <br/> <span data-ttu-id="16dd5-165">-Překlad dotazů: Střední, ale v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="16dd5-165">- Query translation: Medium but cached</span></span> <br/> <span data-ttu-id="16dd5-166">-Materializer generace: Střední, ale v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="16dd5-166">- Materializer generation: Medium but cached</span></span> <br/> <span data-ttu-id="16dd5-167">-Spuštění databázového dotazu: Potenciálně vysoké (lepší dotazy v některých situacích)</span><span class="sxs-lookup"><span data-stu-id="16dd5-167">- Database query execution: Potentially high (Better queries in some situations)</span></span> <br/> <span data-ttu-id="16dd5-168">+ Připojení. otevřít</span><span class="sxs-lookup"><span data-stu-id="16dd5-168">+ Connection.Open</span></span> <br/> <span data-ttu-id="16dd5-169">+ Command. ExecuteReader</span><span class="sxs-lookup"><span data-stu-id="16dd5-169">+ Command.ExecuteReader</span></span> <br/> <span data-ttu-id="16dd5-170">+ DataReader. Read</span><span class="sxs-lookup"><span data-stu-id="16dd5-170">+ DataReader.Read</span></span> <br/> <span data-ttu-id="16dd5-171">Materializace objektu: Střední (rychlejší než EF5)</span><span class="sxs-lookup"><span data-stu-id="16dd5-171">Object materialization: Medium (Faster than EF5)</span></span> <br/> <span data-ttu-id="16dd5-172">-Vyhledávání identity: Střední</span><span class="sxs-lookup"><span data-stu-id="16dd5-172">- Identity lookup: Medium</span></span> |
| `}`                                                                                                  | <span data-ttu-id="16dd5-173">Připojení. Zavřít</span><span class="sxs-lookup"><span data-stu-id="16dd5-173">Connection.Close</span></span>          | <span data-ttu-id="16dd5-174">Nízká</span><span class="sxs-lookup"><span data-stu-id="16dd5-174">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                           | <span data-ttu-id="16dd5-175">Nízká</span><span class="sxs-lookup"><span data-stu-id="16dd5-175">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="16dd5-176">Nízká</span><span class="sxs-lookup"><span data-stu-id="16dd5-176">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |


<span data-ttu-id="16dd5-177">**Druhé provádění dotazů – Rychlý dotaz**</span><span class="sxs-lookup"><span data-stu-id="16dd5-177">**Second Query Execution – warm query**</span></span>

| <span data-ttu-id="16dd5-178">Kódování uživatelských zápisů</span><span class="sxs-lookup"><span data-stu-id="16dd5-178">Code User Writes</span></span>                                                                                     | <span data-ttu-id="16dd5-179">Action</span><span class="sxs-lookup"><span data-stu-id="16dd5-179">Action</span></span>                    | <span data-ttu-id="16dd5-180">Dopad na výkon EF4</span><span class="sxs-lookup"><span data-stu-id="16dd5-180">EF4 Performance Impact</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | <span data-ttu-id="16dd5-181">Dopad na výkon EF5</span><span class="sxs-lookup"><span data-stu-id="16dd5-181">EF5 Performance Impact</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | <span data-ttu-id="16dd5-182">Dopad na výkon EF6</span><span class="sxs-lookup"><span data-stu-id="16dd5-182">EF6 Performance Impact</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|:-----------------------------------------------------------------------------------------------------|:--------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `using(var db = new MyContext())` <br/> `{`                                                          | <span data-ttu-id="16dd5-183">Vytvoření kontextu</span><span class="sxs-lookup"><span data-stu-id="16dd5-183">Context creation</span></span>          | <span data-ttu-id="16dd5-184">Střední</span><span class="sxs-lookup"><span data-stu-id="16dd5-184">Medium</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | <span data-ttu-id="16dd5-185">Střední</span><span class="sxs-lookup"><span data-stu-id="16dd5-185">Medium</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | <span data-ttu-id="16dd5-186">Nízká</span><span class="sxs-lookup"><span data-stu-id="16dd5-186">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `  var q1 = ` <br/> `    from c in db.Customers` <br/> `    where c.Id == id1` <br/> `    select c;` | <span data-ttu-id="16dd5-187">Vytvoření výrazu dotazu</span><span class="sxs-lookup"><span data-stu-id="16dd5-187">Query expression creation</span></span> | <span data-ttu-id="16dd5-188">Nízká</span><span class="sxs-lookup"><span data-stu-id="16dd5-188">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | <span data-ttu-id="16dd5-189">Nízká</span><span class="sxs-lookup"><span data-stu-id="16dd5-189">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | <span data-ttu-id="16dd5-190">Nízká</span><span class="sxs-lookup"><span data-stu-id="16dd5-190">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `  var c1 = q1.First();`                                                                             | <span data-ttu-id="16dd5-191">Provádění dotazů LINQ</span><span class="sxs-lookup"><span data-stu-id="16dd5-191">LINQ query execution</span></span>      | <span data-ttu-id="16dd5-192">-Vyhledávání ~~načítání~~ metadat: ~~Vysoká, ale v mezipaměti~~ Slab</span><span class="sxs-lookup"><span data-stu-id="16dd5-192">- Metadata ~~loading~~ lookup: ~~High but cached~~ Low</span></span> <br/> <span data-ttu-id="16dd5-193">-Vyhledávání ~~generace~~ zobrazení: ~~Potenciálně velmi vysoké, ale v mezipaměti~~ Slab</span><span class="sxs-lookup"><span data-stu-id="16dd5-193">- View ~~generation~~ lookup: ~~Potentially very high but cached~~ Low</span></span> <br/> <span data-ttu-id="16dd5-194">-Vyhodnocení parametrů: Střední</span><span class="sxs-lookup"><span data-stu-id="16dd5-194">- Parameter evaluation: Medium</span></span> <br/> <span data-ttu-id="16dd5-195">-Vyhledávání ~~překladu~~ dotazů: Střední</span><span class="sxs-lookup"><span data-stu-id="16dd5-195">- Query ~~translation~~ lookup: Medium</span></span> <br/> <span data-ttu-id="16dd5-196">-Vyhledávání ~~generace~~ materializer: ~~Střední, ale v mezipaměti~~ Slab</span><span class="sxs-lookup"><span data-stu-id="16dd5-196">- Materializer ~~generation~~ lookup: ~~Medium but cached~~ Low</span></span> <br/> <span data-ttu-id="16dd5-197">-Spuštění databázového dotazu: Potenciálně vysoký</span><span class="sxs-lookup"><span data-stu-id="16dd5-197">- Database query execution: Potentially high</span></span> <br/> <span data-ttu-id="16dd5-198">+ Připojení. otevřít</span><span class="sxs-lookup"><span data-stu-id="16dd5-198">+ Connection.Open</span></span> <br/> <span data-ttu-id="16dd5-199">+ Command. ExecuteReader</span><span class="sxs-lookup"><span data-stu-id="16dd5-199">+ Command.ExecuteReader</span></span> <br/> <span data-ttu-id="16dd5-200">+ DataReader. Read</span><span class="sxs-lookup"><span data-stu-id="16dd5-200">+ DataReader.Read</span></span> <br/> <span data-ttu-id="16dd5-201">Materializace objektu: Střední</span><span class="sxs-lookup"><span data-stu-id="16dd5-201">Object materialization: Medium</span></span> <br/> <span data-ttu-id="16dd5-202">-Vyhledávání identity: Střední</span><span class="sxs-lookup"><span data-stu-id="16dd5-202">- Identity lookup: Medium</span></span> | <span data-ttu-id="16dd5-203">-Vyhledávání ~~načítání~~ metadat: ~~Vysoká, ale v mezipaměti~~ Slab</span><span class="sxs-lookup"><span data-stu-id="16dd5-203">- Metadata ~~loading~~ lookup: ~~High but cached~~ Low</span></span> <br/> <span data-ttu-id="16dd5-204">-Vyhledávání ~~generace~~ zobrazení: ~~Potenciálně velmi vysoké, ale v mezipaměti~~ Slab</span><span class="sxs-lookup"><span data-stu-id="16dd5-204">- View ~~generation~~ lookup: ~~Potentially very high but cached~~ Low</span></span> <br/> <span data-ttu-id="16dd5-205">-Vyhodnocení parametrů: Nízká</span><span class="sxs-lookup"><span data-stu-id="16dd5-205">- Parameter evaluation: Low</span></span> <br/> <span data-ttu-id="16dd5-206">-Vyhledávání ~~překladu~~ dotazů: ~~Střední, ale v mezipaměti~~ Slab</span><span class="sxs-lookup"><span data-stu-id="16dd5-206">- Query ~~translation~~ lookup: ~~Medium but cached~~ Low</span></span> <br/> <span data-ttu-id="16dd5-207">-Vyhledávání ~~generace~~ materializer: ~~Střední, ale v mezipaměti~~ Slab</span><span class="sxs-lookup"><span data-stu-id="16dd5-207">- Materializer ~~generation~~ lookup: ~~Medium but cached~~ Low</span></span> <br/> <span data-ttu-id="16dd5-208">-Spuštění databázového dotazu: Potenciálně vysoké (lepší dotazy v některých situacích)</span><span class="sxs-lookup"><span data-stu-id="16dd5-208">- Database query execution: Potentially high (Better queries in some situations)</span></span> <br/> <span data-ttu-id="16dd5-209">+ Připojení. otevřít</span><span class="sxs-lookup"><span data-stu-id="16dd5-209">+ Connection.Open</span></span> <br/> <span data-ttu-id="16dd5-210">+ Command. ExecuteReader</span><span class="sxs-lookup"><span data-stu-id="16dd5-210">+ Command.ExecuteReader</span></span> <br/> <span data-ttu-id="16dd5-211">+ DataReader. Read</span><span class="sxs-lookup"><span data-stu-id="16dd5-211">+ DataReader.Read</span></span> <br/> <span data-ttu-id="16dd5-212">Materializace objektu: Střední</span><span class="sxs-lookup"><span data-stu-id="16dd5-212">Object materialization: Medium</span></span> <br/> <span data-ttu-id="16dd5-213">-Vyhledávání identity: Střední</span><span class="sxs-lookup"><span data-stu-id="16dd5-213">- Identity lookup: Medium</span></span> | <span data-ttu-id="16dd5-214">-Vyhledávání ~~načítání~~ metadat: ~~Vysoká, ale v mezipaměti~~ Slab</span><span class="sxs-lookup"><span data-stu-id="16dd5-214">- Metadata ~~loading~~ lookup: ~~High but cached~~ Low</span></span> <br/> <span data-ttu-id="16dd5-215">-Vyhledávání ~~generace~~ zobrazení: ~~Střední, ale v mezipaměti~~ Slab</span><span class="sxs-lookup"><span data-stu-id="16dd5-215">- View ~~generation~~ lookup: ~~Medium but cached~~ Low</span></span> <br/> <span data-ttu-id="16dd5-216">-Vyhodnocení parametrů: Nízká</span><span class="sxs-lookup"><span data-stu-id="16dd5-216">- Parameter evaluation: Low</span></span> <br/> <span data-ttu-id="16dd5-217">-Vyhledávání ~~překladu~~ dotazů: ~~Střední, ale v mezipaměti~~ Slab</span><span class="sxs-lookup"><span data-stu-id="16dd5-217">- Query ~~translation~~ lookup: ~~Medium but cached~~ Low</span></span> <br/> <span data-ttu-id="16dd5-218">-Vyhledávání ~~generace~~ materializer: ~~Střední, ale v mezipaměti~~ Slab</span><span class="sxs-lookup"><span data-stu-id="16dd5-218">- Materializer ~~generation~~ lookup: ~~Medium but cached~~ Low</span></span> <br/> <span data-ttu-id="16dd5-219">-Spuštění databázového dotazu: Potenciálně vysoké (lepší dotazy v některých situacích)</span><span class="sxs-lookup"><span data-stu-id="16dd5-219">- Database query execution: Potentially high (Better queries in some situations)</span></span> <br/> <span data-ttu-id="16dd5-220">+ Připojení. otevřít</span><span class="sxs-lookup"><span data-stu-id="16dd5-220">+ Connection.Open</span></span> <br/> <span data-ttu-id="16dd5-221">+ Command. ExecuteReader</span><span class="sxs-lookup"><span data-stu-id="16dd5-221">+ Command.ExecuteReader</span></span> <br/> <span data-ttu-id="16dd5-222">+ DataReader. Read</span><span class="sxs-lookup"><span data-stu-id="16dd5-222">+ DataReader.Read</span></span> <br/> <span data-ttu-id="16dd5-223">Materializace objektu: Střední (rychlejší než EF5)</span><span class="sxs-lookup"><span data-stu-id="16dd5-223">Object materialization: Medium (Faster than EF5)</span></span> <br/> <span data-ttu-id="16dd5-224">-Vyhledávání identity: Střední</span><span class="sxs-lookup"><span data-stu-id="16dd5-224">- Identity lookup: Medium</span></span> |
| `}`                                                                                                  | <span data-ttu-id="16dd5-225">Připojení. Zavřít</span><span class="sxs-lookup"><span data-stu-id="16dd5-225">Connection.Close</span></span>          | <span data-ttu-id="16dd5-226">Nízká</span><span class="sxs-lookup"><span data-stu-id="16dd5-226">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | <span data-ttu-id="16dd5-227">Nízká</span><span class="sxs-lookup"><span data-stu-id="16dd5-227">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | <span data-ttu-id="16dd5-228">Nízká</span><span class="sxs-lookup"><span data-stu-id="16dd5-228">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |


<span data-ttu-id="16dd5-229">Existuje několik způsobů, jak snížit náklady na výkon u studených i tepléch dotazů, a my se podíváme na tyto možnosti v následující části.</span><span class="sxs-lookup"><span data-stu-id="16dd5-229">There are several ways to reduce the performance cost of both cold and warm queries, and we'll take a look at these in the following section.</span></span> <span data-ttu-id="16dd5-230">Konkrétně se podíváme na to, jak snížit náklady na načítání modelů v studených dotazech pomocí předem vygenerovaných zobrazení, která by měla přispět ke zmírnění bolesti při vytváření zobrazení.</span><span class="sxs-lookup"><span data-stu-id="16dd5-230">Specifically, we'll look at reducing the cost of model loading in cold queries by using pre-generated views, which should help alleviate performance pains experienced during view generation.</span></span> <span data-ttu-id="16dd5-231">V případě rychlých dotazů pokryjeme ukládání plánů dotazů do mezipaměti, žádné sledovací dotazy a různé možnosti spuštění dotazů.</span><span class="sxs-lookup"><span data-stu-id="16dd5-231">For warm queries, we'll cover query plan caching, no tracking queries, and different query execution options.</span></span>

### <a name="21-what-is-view-generation"></a><span data-ttu-id="16dd5-232">2,1 co je generování zobrazení?</span><span class="sxs-lookup"><span data-stu-id="16dd5-232">2.1 What is View Generation?</span></span>

<span data-ttu-id="16dd5-233">Aby bylo možné pochopit, co je generování zobrazení, je nutné nejprve pochopit, co jsou zobrazení mapování.</span><span class="sxs-lookup"><span data-stu-id="16dd5-233">In order to understand what view generation is, we must first understand what “Mapping Views” are.</span></span> <span data-ttu-id="16dd5-234">Zobrazení mapování jsou spustitelné reprezentace transformací určených v mapování pro každou sadu entit a přidružení.</span><span class="sxs-lookup"><span data-stu-id="16dd5-234">Mapping Views are executable representations of the transformations specified in the mapping for each entity set and association.</span></span> <span data-ttu-id="16dd5-235">Interní tato zobrazení mapování přebírají tvar CQTs (kanonické stromy dotazů).</span><span class="sxs-lookup"><span data-stu-id="16dd5-235">Internally, these mapping views take the shape of CQTs (canonical query trees).</span></span> <span data-ttu-id="16dd5-236">Existují dva typy zobrazení mapování:</span><span class="sxs-lookup"><span data-stu-id="16dd5-236">There are two types of mapping views:</span></span>

-   <span data-ttu-id="16dd5-237">Zobrazení dotazů: představuje transformaci nutnou k přechodu ze schématu databáze do koncepčního modelu.</span><span class="sxs-lookup"><span data-stu-id="16dd5-237">Query views: these represent the transformation necessary to go from the database schema to the conceptual model.</span></span>
-   <span data-ttu-id="16dd5-238">Zobrazení aktualizací: představuje transformaci nutnou k přechodu z koncepčního modelu do schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="16dd5-238">Update views: these represent the transformation necessary to go from the conceptual model to the database schema.</span></span>

<span data-ttu-id="16dd5-239">Mějte na paměti, že koncepční model se může od schématu databáze lišit různými způsoby.</span><span class="sxs-lookup"><span data-stu-id="16dd5-239">Keep in mind that the conceptual model might differ from the database schema in various ways.</span></span> <span data-ttu-id="16dd5-240">Například jedna jedna tabulka může být použita k uložení dat pro dva různé typy entit.</span><span class="sxs-lookup"><span data-stu-id="16dd5-240">For example, one single table might be used to store the data for two different entity types.</span></span> <span data-ttu-id="16dd5-241">Dědičnost a jiné než triviální mapování hrají roli ve složitosti zobrazení mapování.</span><span class="sxs-lookup"><span data-stu-id="16dd5-241">Inheritance and non-trivial mappings play a role in the complexity of the mapping views.</span></span>

<span data-ttu-id="16dd5-242">Proces výpočtu těchto zobrazení na základě specifikace mapování je to, co voláme na generování zobrazení.</span><span class="sxs-lookup"><span data-stu-id="16dd5-242">The process of computing these views based on the specification of the mapping is what we call view generation.</span></span> <span data-ttu-id="16dd5-243">Generování zobrazení může být provedeno dynamicky při načtení modelu nebo v čase sestavení pomocí "předem vygenerované zobrazení"; Druhá je serializovaná ve formě Entity SQL příkazů do souboru jazyka C @ no__t-0 nebo VB.</span><span class="sxs-lookup"><span data-stu-id="16dd5-243">View generation can either take place dynamically when a model is loaded, or at build time, by using "pre-generated views"; the latter are serialized in the form of Entity SQL statements to a C\# or VB file.</span></span>

<span data-ttu-id="16dd5-244">Při generování zobrazení jsou také ověřeny.</span><span class="sxs-lookup"><span data-stu-id="16dd5-244">When views are generated, they are also validated.</span></span> <span data-ttu-id="16dd5-245">Z hlediska výkonu je velká většina generování zobrazení skutečně ověřením zobrazení, která zajistí, aby připojení mezi entitami dávala smysl a aby měla správnou mohutnost pro všechny podporované operace.</span><span class="sxs-lookup"><span data-stu-id="16dd5-245">From a performance standpoint, the vast majority of the cost of view generation is actually the validation of the views which ensures that the connections between the entities make sense and have the correct cardinality for all the supported operations.</span></span>

<span data-ttu-id="16dd5-246">Když se spustí dotaz na sadu entit, dotaz se sloučí s odpovídajícím zobrazením dotazu a výsledek tohoto složení se spustí prostřednictvím kompilátoru plánu, aby se vytvořila reprezentace dotazu, kterou může záložní úložiště pochopit.</span><span class="sxs-lookup"><span data-stu-id="16dd5-246">When a query over an entity set is executed, the query is combined with the corresponding query view, and the result of this composition is run through the plan compiler to create the representation of the query that the backing store can understand.</span></span> <span data-ttu-id="16dd5-247">Pro SQL Server konečný výsledek této kompilace bude příkaz SELECT jazyka T-SQL.</span><span class="sxs-lookup"><span data-stu-id="16dd5-247">For SQL Server, the final result of this compilation will be a T-SQL SELECT statement.</span></span> <span data-ttu-id="16dd5-248">Při prvním provedení aktualizace sady entit se zobrazení aktualizace spustí pomocí podobného procesu a převede je na příkazy DML pro cílovou databázi.</span><span class="sxs-lookup"><span data-stu-id="16dd5-248">The first time an update over an entity set is performed, the update view is run through a similar process to transform it into DML statements for the target database.</span></span>

### <a name="22-factors-that-affect-view-generation-performance"></a><span data-ttu-id="16dd5-249">2,2 faktory ovlivňující výkon generování zobrazení</span><span class="sxs-lookup"><span data-stu-id="16dd5-249">2.2 Factors that affect View Generation performance</span></span>

<span data-ttu-id="16dd5-250">Výkon kroku generace (zobrazení) nezávisí jenom na velikosti modelu, ale také na způsobu propojení modelu.</span><span class="sxs-lookup"><span data-stu-id="16dd5-250">The performance of view generation step not only depends on the size of your model but also on how interconnected the model is.</span></span> <span data-ttu-id="16dd5-251">Pokud jsou dvě entity propojeny prostřednictvím řetězce dědičnosti nebo přidružení, označují se jako připojené.</span><span class="sxs-lookup"><span data-stu-id="16dd5-251">If two Entities are connected via an inheritance chain or an Association, they are said to be connected.</span></span> <span data-ttu-id="16dd5-252">Podobně pokud jsou dvě tabulky propojeny pomocí cizího klíče, jsou propojeny.</span><span class="sxs-lookup"><span data-stu-id="16dd5-252">Similarly if two tables are connected via a foreign key, they are connected.</span></span> <span data-ttu-id="16dd5-253">Když se zvýší počet propojených entit a tabulek ve vašich schématech, zvýší se náklady na zobrazení na generaci.</span><span class="sxs-lookup"><span data-stu-id="16dd5-253">As the number of connected Entities and tables in your schemas increase, the view generation cost increases.</span></span>

<span data-ttu-id="16dd5-254">Algoritmus, který používáme k vygenerování a ověření zobrazení, je exponenciální v nejhorším případě, ale k vylepšení této hodnoty používáme několik optimalizací.</span><span class="sxs-lookup"><span data-stu-id="16dd5-254">The algorithm that we use to generate and validate views is exponential in the worst case, though we do use some optimizations to improve this.</span></span> <span data-ttu-id="16dd5-255">Největší faktory, které zdají negativně ovlivnit výkon, jsou:</span><span class="sxs-lookup"><span data-stu-id="16dd5-255">The biggest factors that seem to negatively affect performance are:</span></span>

-   <span data-ttu-id="16dd5-256">Velikost modelu odkazující na počet entit a množství přidružení mezi těmito entitami.</span><span class="sxs-lookup"><span data-stu-id="16dd5-256">Model size, referring to the number of entities and the amount of associations between these entities.</span></span>
-   <span data-ttu-id="16dd5-257">Složitosti modelu, konkrétně dědičnost zahrnující velký počet typů.</span><span class="sxs-lookup"><span data-stu-id="16dd5-257">Model complexity, specifically inheritance involving a large number of types.</span></span>
-   <span data-ttu-id="16dd5-258">Použití nezávislých přidružení namísto přidružení cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="16dd5-258">Using Independent Associations, instead of Foreign Key Associations.</span></span>

<span data-ttu-id="16dd5-259">U malých jednoduchých modelů může být cena dostatečně malá, aby se bother pomocí předem vygenerovaných zobrazení.</span><span class="sxs-lookup"><span data-stu-id="16dd5-259">For small, simple models the cost may be small enough to not bother using pre-generated views.</span></span> <span data-ttu-id="16dd5-260">Při zvýšení velikosti modelu a složitosti je k dispozici několik možností, jak snížit náklady na generování a ověřování v zobrazení.</span><span class="sxs-lookup"><span data-stu-id="16dd5-260">As model size and complexity increase, there are several options available to reduce the cost of view generation and validation.</span></span>

### <a name="23-using-pre-generated-views-to-decrease-model-load-time"></a><span data-ttu-id="16dd5-261">2,3 použití předem vygenerovaných zobrazení ke snížení doby načítání modelu</span><span class="sxs-lookup"><span data-stu-id="16dd5-261">2.3 Using Pre-Generated Views to decrease model load time</span></span>

<span data-ttu-id="16dd5-262">Podrobné informace o tom, jak používat předem vygenerovaná zobrazení na Entity Framework 6, najdete v [Předgenerovaných zobrazeních mapování](~/ef6/fundamentals/performance/pre-generated-views.md) .</span><span class="sxs-lookup"><span data-stu-id="16dd5-262">For detailed information on how to use pre-generated views on Entity Framework 6 visit [Pre-Generated Mapping Views](~/ef6/fundamentals/performance/pre-generated-views.md)</span></span>

#### <a name="231-pre-generated-views-using-the-entity-framework-power-tools-community-edition"></a><span data-ttu-id="16dd5-263">2.3.1 předem vygenerovaná zobrazení pomocí edice Entity Framework Power Tools Community</span><span class="sxs-lookup"><span data-stu-id="16dd5-263">2.3.1 Pre-Generated views using the Entity Framework Power Tools Community Edition</span></span>

<span data-ttu-id="16dd5-264">Pomocí [nástroje Entity Framework 6 Power Tools Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) můžete vygenerovat zobrazení modelu EDMX a Code First tak, že kliknete pravým tlačítkem myši na soubor třídy modelu a pomocí nabídky Entity Framework vyberete "generovat zobrazení".</span><span class="sxs-lookup"><span data-stu-id="16dd5-264">You can use the [Entity Framework 6 Power Tools Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) to generate views of EDMX and Code First models by right-clicking the model class file and using the Entity Framework menu to select “Generate Views”.</span></span> <span data-ttu-id="16dd5-265">Edice Entity Framework Power Tools Community pracují pouze na kontextech odvozených od DbContext.</span><span class="sxs-lookup"><span data-stu-id="16dd5-265">The Entity Framework Power Tools Community Edition work only on DbContext-derived contexts.</span></span>

#### <a name="232-how-to-use-pre-generated-views-with-a-model-created-by-edmgen"></a><span data-ttu-id="16dd5-266">2.3.2 použití předem vygenerovaných zobrazení s modelem vytvořeným pomocí EDMGen</span><span class="sxs-lookup"><span data-stu-id="16dd5-266">2.3.2 How to use Pre-generated views with a model created by EDMGen</span></span>

<span data-ttu-id="16dd5-267">EDMGen je nástroj, který se dodává s .NET a pracuje s Entity Framework 4 a 5, ale ne s Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="16dd5-267">EDMGen is a utility that ships with .NET and works with Entity Framework 4 and 5, but not with Entity Framework 6.</span></span> <span data-ttu-id="16dd5-268">EDMGen umožňuje vygenerovat soubor modelu, vrstvu objektů a zobrazení z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="16dd5-268">EDMGen allows you to generate a model file, the object layer and the views from the command line.</span></span> <span data-ttu-id="16dd5-269">Jeden z výstupů bude soubor zobrazení ve vašem jazyce podle volby, VB nebo C @ no__t-0.</span><span class="sxs-lookup"><span data-stu-id="16dd5-269">One of the outputs will be a Views file in your language of choice, VB or C\#.</span></span> <span data-ttu-id="16dd5-270">Toto je soubor kódu obsahující Entity SQL fragmentů pro každou sadu entit.</span><span class="sxs-lookup"><span data-stu-id="16dd5-270">This is a code file containing Entity SQL snippets for each entity set.</span></span> <span data-ttu-id="16dd5-271">Chcete-li povolit předem vygenerovaná zobrazení, stačí do projektu přidat soubor.</span><span class="sxs-lookup"><span data-stu-id="16dd5-271">To enable pre-generated views, you simply include the file in your project.</span></span>

<span data-ttu-id="16dd5-272">Pokud ručně provedete úpravy souborů schématu pro model, budete muset znovu vygenerovat soubor zobrazení.</span><span class="sxs-lookup"><span data-stu-id="16dd5-272">If you manually make edits to the schema files for the model, you will need to re-generate the views file.</span></span> <span data-ttu-id="16dd5-273">To můžete provést tak, že spustíte EDMGen pomocí příznaku **/Mode: ViewGeneration** .</span><span class="sxs-lookup"><span data-stu-id="16dd5-273">You can do this by running EDMGen with the **/mode:ViewGeneration** flag.</span></span>

#### <a name="233-how-to-use-pre-generated-views-with-an-edmx-file"></a><span data-ttu-id="16dd5-274">2.3.3 Jak používat předem vygenerovaná zobrazení se souborem EDMX</span><span class="sxs-lookup"><span data-stu-id="16dd5-274">2.3.3 How to use Pre-Generated Views with an EDMX file</span></span>

<span data-ttu-id="16dd5-275">Pomocí EDMGen můžete také vygenerovat zobrazení pro soubor EDMX – dříve odkazované téma MSDN popisuje, jak přidat událost před sestavením, aby to bylo možné, ale to je složité a v některých případech je to možné.</span><span class="sxs-lookup"><span data-stu-id="16dd5-275">You can also use EDMGen to generate views for an EDMX file - the previously referenced MSDN topic describes how to add a pre-build event to do this - but this is complicated and there are some cases where it isn't possible.</span></span> <span data-ttu-id="16dd5-276">Obecně je snazší použít šablonu T4 k vygenerování zobrazení, když je model v souboru EDMX.</span><span class="sxs-lookup"><span data-stu-id="16dd5-276">It's generally easier to use a T4 template to generate the views when your model is in an edmx file.</span></span>

<span data-ttu-id="16dd5-277">Blog týmu ADO.NET má příspěvek, který popisuje, jak použít šablonu T4 pro generování zobrazení ( \<http://blogs.msdn.com/b/adonet/archive/2008/06/20/how-to-use-a-t4-template-for-view-generation.aspx>).</span><span class="sxs-lookup"><span data-stu-id="16dd5-277">The ADO.NET team blog has a post that describes how to use a T4 template for view generation ( \<http://blogs.msdn.com/b/adonet/archive/2008/06/20/how-to-use-a-t4-template-for-view-generation.aspx>).</span></span> <span data-ttu-id="16dd5-278">Tento příspěvek zahrnuje šablonu, kterou lze stáhnout a přidat do projektu.</span><span class="sxs-lookup"><span data-stu-id="16dd5-278">This post includes a template that can be downloaded and added to your project.</span></span> <span data-ttu-id="16dd5-279">Šablona byla napsána pro první verzi Entity Framework, takže není zaručena práce s nejnovějšími verzemi Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="16dd5-279">The template was written for the first version of Entity Framework, so they aren’t guaranteed to work with the latest versions of Entity Framework.</span></span> <span data-ttu-id="16dd5-280">Můžete si však stáhnout aktuálnější sadu šablon pro generování zobrazení pro Entity Framework 4 a 5from galerii sady Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="16dd5-280">However, you can download a more up-to-date set of view generation templates for Entity Framework 4 and 5from the Visual Studio Gallery:</span></span>

-   <span data-ttu-id="16dd5-281">VB.NET: \<http://visualstudiogallery.msdn.microsoft.com/118b44f2-1b91-4de2-a584-7a680418941d></span><span class="sxs-lookup"><span data-stu-id="16dd5-281">VB.NET: \<http://visualstudiogallery.msdn.microsoft.com/118b44f2-1b91-4de2-a584-7a680418941d></span></span>
-   <span data-ttu-id="16dd5-282">C\#: \<http://visualstudiogallery.msdn.microsoft.com/ae7730ce-ddab-470f-8456-1b313cd2c44d></span><span class="sxs-lookup"><span data-stu-id="16dd5-282">C\#: \<http://visualstudiogallery.msdn.microsoft.com/ae7730ce-ddab-470f-8456-1b313cd2c44d></span></span>

<span data-ttu-id="16dd5-283">Pokud používáte Entity Framework 6 můžete získat zobrazení T4 generování šablon z Galerie sady Visual Studio na \<http://visualstudiogallery.msdn.microsoft.com/18a7db90-6705-4d19-9dd1-0a6c23d0751f>.</span><span class="sxs-lookup"><span data-stu-id="16dd5-283">If you’re using Entity Framework 6 you can get the view generation T4 templates from the Visual Studio Gallery at \<http://visualstudiogallery.msdn.microsoft.com/18a7db90-6705-4d19-9dd1-0a6c23d0751f>.</span></span>

### <a name="24-reducing-the-cost-of-view-generation"></a><span data-ttu-id="16dd5-284">2,4 snížení nákladů na generaci zobrazení</span><span class="sxs-lookup"><span data-stu-id="16dd5-284">2.4 Reducing the cost of view generation</span></span>

<span data-ttu-id="16dd5-285">Použití předem vygenerovaných zobrazení přesouvá náklady na generaci zobrazení z načítání modelů (doba běhu) do doby návrhu.</span><span class="sxs-lookup"><span data-stu-id="16dd5-285">Using pre-generated views moves the cost of view generation from model loading (run time) to design time.</span></span> <span data-ttu-id="16dd5-286">I když to zlepšuje výkon při spuštění za běhu, pořád při vývoji dojde k bolesti generace zobrazení.</span><span class="sxs-lookup"><span data-stu-id="16dd5-286">While this improves startup performance at runtime, you will still experience the pain of view generation while you are developing.</span></span> <span data-ttu-id="16dd5-287">Existuje několik dalších zdvihů, které mohou snížit náklady na generaci zobrazení, a to jak v době kompilace, tak i v době běhu.</span><span class="sxs-lookup"><span data-stu-id="16dd5-287">There are several additional tricks that can help reduce the cost of view generation, both at compile time and run time.</span></span>

#### <a name="241-using-foreign-key-associations-to-reduce-view-generation-cost"></a><span data-ttu-id="16dd5-288">2.4.1 použití přidružení cizího klíče k omezení nákladů na generaci zobrazení</span><span class="sxs-lookup"><span data-stu-id="16dd5-288">2.4.1 Using Foreign Key Associations to reduce view generation cost</span></span>

<span data-ttu-id="16dd5-289">Zjistili jsme několik případů, kdy při přepínání přidružení v modelu z nezávislých přidružení k přidružení cizího klíče došlo k výraznému zlepšení času stráveného při generování zobrazení.</span><span class="sxs-lookup"><span data-stu-id="16dd5-289">We have seen a number of cases where switching the associations in the model from Independent Associations to Foreign Key Associations dramatically improved the time spent in view generation.</span></span>

<span data-ttu-id="16dd5-290">K předvedení tohoto vylepšení jsme vygenerovali dvě verze modelu Navision pomocí EDMGen.</span><span class="sxs-lookup"><span data-stu-id="16dd5-290">To demonstrate this improvement, we generated two versions of the Navision model by using EDMGen.</span></span> <span data-ttu-id="16dd5-291">*Poznámka: Popis modelu Navision naleznete v příloze C.*</span><span class="sxs-lookup"><span data-stu-id="16dd5-291">*Note: see appendix C for a description of the Navision model.*</span></span> <span data-ttu-id="16dd5-292">Model Navision je pro toto cvičení zajímavě z důvodu jeho velmi velkého množství entit a vztahů mezi nimi.</span><span class="sxs-lookup"><span data-stu-id="16dd5-292">The Navision model is interesting for this exercise due to its very large amount of entities and relationships between them.</span></span>

<span data-ttu-id="16dd5-293">Jedna verze tohoto velmi velkého modelu byla generována s přidruženími cizích klíčů a druhá byla vygenerována s nezávislými přidruženími.</span><span class="sxs-lookup"><span data-stu-id="16dd5-293">One version of this very large model was generated with Foreign Keys Associations and the other was generated with Independent Associations.</span></span> <span data-ttu-id="16dd5-294">Doba, po kterou trvalo generování zobrazení pro každý model, pak vyprší.</span><span class="sxs-lookup"><span data-stu-id="16dd5-294">We then timed how long it took to generate the views for each model.</span></span> <span data-ttu-id="16dd5-295">Entity Framework 5 test použil metodu GenerateViews () z třídy EntityViewGenerator pro generování zobrazení, zatímco test Entity Framework 6 používal metodu GenerateViews () z třídy StorageMappingItemCollection.</span><span class="sxs-lookup"><span data-stu-id="16dd5-295">Entity Framework 5 test used the GenerateViews() method from class EntityViewGenerator to generate the views, while the Entity Framework 6 test used the GenerateViews() method from class StorageMappingItemCollection.</span></span> <span data-ttu-id="16dd5-296">Z důvodu restrukturalizace kódu, ke které došlo v základu kódu Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="16dd5-296">This due to code restructuring that occurred in the Entity Framework 6 codebase.</span></span>

<span data-ttu-id="16dd5-297">Při použití Entity Framework 5 se generace zobrazení pro model s cizími klíči na testovacím počítači trvala 65 minut.</span><span class="sxs-lookup"><span data-stu-id="16dd5-297">Using Entity Framework 5, view generation for the model with Foreign Keys took 65 minutes in a lab machine.</span></span> <span data-ttu-id="16dd5-298">Není známo, jak dlouho trvalo generování zobrazení pro model, který používal nezávislá přidružení.</span><span class="sxs-lookup"><span data-stu-id="16dd5-298">It's unknown how long it would have taken to generate the views for the model that used independent associations.</span></span> <span data-ttu-id="16dd5-299">V našem testovacím prostředí jsme ponechali běžet za měsíc, než se počítač restartoval v našem testovacím prostředí, aby se nainstalovaly měsíční aktualizace.</span><span class="sxs-lookup"><span data-stu-id="16dd5-299">We left the test running for over a month before the machine was rebooted in our lab to install monthly updates.</span></span>

<span data-ttu-id="16dd5-300">Při použití Entity Framework 6 se ve stejném testovacím počítači trvalo 28 sekund ve generaci modelu s cizími klíči.</span><span class="sxs-lookup"><span data-stu-id="16dd5-300">Using Entity Framework 6, view generation for the model with Foreign Keys took 28 seconds in the same lab machine.</span></span> <span data-ttu-id="16dd5-301">Generování zobrazení modelu, který používá nezávislé přidružení, trvalo 58 sekund.</span><span class="sxs-lookup"><span data-stu-id="16dd5-301">View generation for the model that uses Independent Associations took 58 seconds.</span></span> <span data-ttu-id="16dd5-302">Vylepšení Entity Framework 6 ve svém kódu generování zobrazení znamenají, že mnoho projektů nebude potřebovat předem vygenerovaná zobrazení k získání rychlejšího spuštění.</span><span class="sxs-lookup"><span data-stu-id="16dd5-302">The improvements done to Entity Framework 6 on its view generation code mean that many projects won’t need pre-generated views to obtain faster startup times.</span></span>

<span data-ttu-id="16dd5-303">Je důležité přeznačit, že předběžně vygenerování zobrazení v Entity Framework 4 a 5 se dá provést pomocí EDMGen nebo Entity Framework nástrojů Power Tools.</span><span class="sxs-lookup"><span data-stu-id="16dd5-303">It’s important to remark that pre-generating views in Entity Framework 4 and 5 can be done with EDMGen or the Entity Framework Power Tools.</span></span> <span data-ttu-id="16dd5-304">Pro Entity Framework 6 se generování zobrazení dá provést prostřednictvím Entity Framework nástrojů Power Tools nebo programově, jak je popsané v [Předgenerovaných zobrazeních mapování](~/ef6/fundamentals/performance/pre-generated-views.md).</span><span class="sxs-lookup"><span data-stu-id="16dd5-304">For Entity Framework 6 view generation can be done via the Entity Framework Power Tools or programmatically as described in [Pre-Generated Mapping Views](~/ef6/fundamentals/performance/pre-generated-views.md).</span></span>

##### <a name="2411-how-to-use-foreign-keys-instead-of-independent-associations"></a><span data-ttu-id="16dd5-305">2.4.1.1, jak používat cizí klíče místo nezávislých přidružení</span><span class="sxs-lookup"><span data-stu-id="16dd5-305">2.4.1.1 How to use Foreign Keys instead of Independent Associations</span></span>

<span data-ttu-id="16dd5-306">Při použití EDMGen nebo Entity Designer v aplikaci Visual Studio získáte FKs ve výchozím nastavení a pro přepínání mezi FKs a službou IAs stačí pouze jeden příznak CheckBox nebo příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="16dd5-306">When using EDMGen or the Entity Designer in Visual Studio, you get FKs by default, and it only takes a single checkbox or command line flag to switch between FKs and IAs.</span></span>

<span data-ttu-id="16dd5-307">Pokud máte velký Code First model, bude použití nezávislých přidružení mít stejný účinek na generování zobrazení.</span><span class="sxs-lookup"><span data-stu-id="16dd5-307">If you have a large Code First model, using Independent Associations will have the same effect on view generation.</span></span> <span data-ttu-id="16dd5-308">Tomuto dopadu se můžete vyhnout zahrnutím vlastností cizích klíčů na třídy pro závislé objekty, i když někteří vývojáři to považují za znečišťující jejich objektový model.</span><span class="sxs-lookup"><span data-stu-id="16dd5-308">You can avoid this impact by including Foreign Key properties on the classes for your dependent objects, though some developers will consider this to be polluting their object model.</span></span> <span data-ttu-id="16dd5-309">Můžete najít další informace o této skutečnosti do \<http://blog.oneunicorn.com/2011/12/11/whats-the-deal-with-mapping-foreign-keys-using-the-entity-framework/>.</span><span class="sxs-lookup"><span data-stu-id="16dd5-309">You can find more information on this subject in \<http://blog.oneunicorn.com/2011/12/11/whats-the-deal-with-mapping-foreign-keys-using-the-entity-framework/>.</span></span>

| <span data-ttu-id="16dd5-310">Při použití</span><span class="sxs-lookup"><span data-stu-id="16dd5-310">When using</span></span>      | <span data-ttu-id="16dd5-311">Postup</span><span class="sxs-lookup"><span data-stu-id="16dd5-311">Do this</span></span>                                                                                                                                                                                                                                                                                                                              |
|:----------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="16dd5-312">Entity Designer</span><span class="sxs-lookup"><span data-stu-id="16dd5-312">Entity Designer</span></span> | <span data-ttu-id="16dd5-313">Po přidání přidružení mezi dvěma entitami se ujistěte, že máte referenční omezení.</span><span class="sxs-lookup"><span data-stu-id="16dd5-313">After adding an association between two entities, make sure you have a referential constraint.</span></span> <span data-ttu-id="16dd5-314">Referenční omezení určují Entity Framework použití cizích klíčů namísto nezávislých přidružení.</span><span class="sxs-lookup"><span data-stu-id="16dd5-314">Referential constraints tell Entity Framework to use Foreign Keys instead of Independent Associations.</span></span> <span data-ttu-id="16dd5-315">Další podrobnosti najdete \<http://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx>.</span><span class="sxs-lookup"><span data-stu-id="16dd5-315">For additional details visit \<http://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx>.</span></span> |
| <span data-ttu-id="16dd5-316">EDMGen</span><span class="sxs-lookup"><span data-stu-id="16dd5-316">EDMGen</span></span>          | <span data-ttu-id="16dd5-317">Při použití EDMGen k vygenerování souborů z databáze se vaše cizí klíče budou dodržovat a přidají se do modelu jako takové.</span><span class="sxs-lookup"><span data-stu-id="16dd5-317">When using EDMGen to generate your files from the database, your Foreign Keys will be respected and added to the model as such.</span></span> <span data-ttu-id="16dd5-318">Další informace o různých možnostech, které jsou vystavené EDMGen [http://msdn.microsoft.com/library/bb387165.aspx](https://msdn.microsoft.com/library/bb387165.aspx).</span><span class="sxs-lookup"><span data-stu-id="16dd5-318">For more information on the different options exposed by EDMGen visit [http://msdn.microsoft.com/library/bb387165.aspx](https://msdn.microsoft.com/library/bb387165.aspx).</span></span>                           |
| <span data-ttu-id="16dd5-319">Code First</span><span class="sxs-lookup"><span data-stu-id="16dd5-319">Code First</span></span>      | <span data-ttu-id="16dd5-320">Informace o tom, jak zahrnout vlastnosti cizích klíčů do závislých objektů při použití Code First, najdete v části "konvence vztahu" v tématu [konvence Code First](~/ef6/modeling/code-first/conventions/built-in.md) .</span><span class="sxs-lookup"><span data-stu-id="16dd5-320">See the "Relationship Convention" section of the [Code First Conventions](~/ef6/modeling/code-first/conventions/built-in.md) topic for information on how to include foreign key properties on dependent objects when using Code First.</span></span>                                                                                              |

#### <a name="242-moving-your-model-to-a-separate-assembly"></a><span data-ttu-id="16dd5-321">2.4.2 přesunutí modelu do samostatného sestavení</span><span class="sxs-lookup"><span data-stu-id="16dd5-321">2.4.2 Moving your model to a separate assembly</span></span>

<span data-ttu-id="16dd5-322">Když je model zahrnut přímo do projektu aplikace a Vy vygenerujete zobrazení prostřednictvím události před sestavením nebo šablony T4, generování a ověřování se provedou, když se projekt znovu vytvoří, i když se model nezměnil.</span><span class="sxs-lookup"><span data-stu-id="16dd5-322">When your model is included directly in your application's project and you generate views through a pre-build event or a T4 template, view generation and validation will take place whenever the project is rebuilt, even if the model wasn't changed.</span></span> <span data-ttu-id="16dd5-323">Pokud model přesunete do samostatného sestavení a odkazujete ho z projektu vaší aplikace, můžete provést další změny v aplikaci, aniž byste museli znovu sestavit projekt obsahující model.</span><span class="sxs-lookup"><span data-stu-id="16dd5-323">If you move the model to a separate assembly and reference it from your application's project, you can make other changes to your application without needing to rebuild the project containing the model.</span></span>

<span data-ttu-id="16dd5-324">*Poznámka:*  when přesunutí modelu do samostatných sestavení Nezapomeňte zkopírovat připojovací řetězce pro model do konfiguračního souboru aplikace klientského projektu.</span><span class="sxs-lookup"><span data-stu-id="16dd5-324">*Note:*  when moving your model to separate assemblies remember to copy the connection strings for the model into the application configuration file of the client project.</span></span>

#### <a name="243-disable-validation-of-an-edmx-based-model"></a><span data-ttu-id="16dd5-325">2.4.3 zakázání ověřování modelu založeného na EDMX</span><span class="sxs-lookup"><span data-stu-id="16dd5-325">2.4.3 Disable validation of an edmx-based model</span></span>

<span data-ttu-id="16dd5-326">Modely EDMX jsou ověřovány v době kompilace, a to i v případě, že model není změněn.</span><span class="sxs-lookup"><span data-stu-id="16dd5-326">EDMX models are validated at compile time, even if the model is unchanged.</span></span> <span data-ttu-id="16dd5-327">Pokud váš model již byl ověřen, můžete potlačit ověřování v době kompilace nastavením vlastnosti "ověřit při sestavení" na hodnotu false v okně Vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="16dd5-327">If your model has already been validated, you can suppress validation at compile time by setting the "Validate on Build" property to false in the properties window.</span></span> <span data-ttu-id="16dd5-328">Když změníte mapování nebo model, můžete dočasně znovu povolit ověřování a ověřit provedené změny.</span><span class="sxs-lookup"><span data-stu-id="16dd5-328">When you change your mapping or model, you can temporarily re-enable validation to verify your changes.</span></span>

<span data-ttu-id="16dd5-329">Zvýšení výkonu bylo provedeno v Entity Framework Designer pro Entity Framework 6 a náklady na ověření při sestavení jsou mnohem nižší než v předchozích verzích návrháře.</span><span class="sxs-lookup"><span data-stu-id="16dd5-329">Note that performance improvements were made to the Entity Framework Designer for Entity Framework 6, and the cost of the “Validate on Build” is much lower than in previous versions of the designer.</span></span>

## <a name="3-caching-in-the-entity-framework"></a><span data-ttu-id="16dd5-330">3 ukládání do mezipaměti v Entity Framework</span><span class="sxs-lookup"><span data-stu-id="16dd5-330">3 Caching in the Entity Framework</span></span>

<span data-ttu-id="16dd5-331">Entity Framework má následující formy integrovaného ukládání do mezipaměti:</span><span class="sxs-lookup"><span data-stu-id="16dd5-331">Entity Framework has the following forms of caching built-in:</span></span>

1.  <span data-ttu-id="16dd5-332">Ukládání objektů do mezipaměti – objekt ObjectStateManager integrovaný do instance ObjectContext udržuje v paměti objekty, které byly načteny pomocí této instance.</span><span class="sxs-lookup"><span data-stu-id="16dd5-332">Object caching – the ObjectStateManager built into an ObjectContext instance keeps track in memory of the objects that have been retrieved using that instance.</span></span> <span data-ttu-id="16dd5-333">Tato možnost je také známá jako mezipaměť první úrovně.</span><span class="sxs-lookup"><span data-stu-id="16dd5-333">This is also known as first-level cache.</span></span>
2.  <span data-ttu-id="16dd5-334">Ukládání plánu dotazů do mezipaměti – opětovné použití příkazu pro vygenerované úložiště při spuštění dotazu více než jednou.</span><span class="sxs-lookup"><span data-stu-id="16dd5-334">Query Plan Caching - reusing the generated store command when a query is executed more than once.</span></span>
3.  <span data-ttu-id="16dd5-335">Ukládání metadat do mezipaměti – sdílení metadat pro model napříč různými připojeními ke stejnému modelu.</span><span class="sxs-lookup"><span data-stu-id="16dd5-335">Metadata caching - sharing the metadata for a model across different connections to the same model.</span></span>

<span data-ttu-id="16dd5-336">Kromě mezipamětí, které jsou uvedené v EF, se k rozšiřování Entity Framework s mezipamětí pro výsledky načtené z databáze dá také použít zvláštní druh zprostředkovatele dat ADO.NET, který se označuje jako poskytovatel pro zabalení, a také označovaný jako ukládání do mezipaměti druhé úrovně.</span><span class="sxs-lookup"><span data-stu-id="16dd5-336">Besides the caches that EF provides out of the box, a special kind of ADO.NET data provider known as a wrapping provider can also be used to extend Entity Framework with a cache for the results retrieved from the database, also known as second-level caching.</span></span>

### <a name="31-object-caching"></a><span data-ttu-id="16dd5-337">Mezipaměť objektu 3,1</span><span class="sxs-lookup"><span data-stu-id="16dd5-337">3.1 Object Caching</span></span>

<span data-ttu-id="16dd5-338">Ve výchozím nastavení, když se entita vrátí do výsledků dotazu, těsně předtím, než EF materializuje, ObjectContext zkontroluje, jestli už entita se stejným klíčem načetla do svého objektu ObjectStateManager.</span><span class="sxs-lookup"><span data-stu-id="16dd5-338">By default when an entity is returned in the results of a query, just before EF materializes it, the ObjectContext will check if an entity with the same key has already been loaded into its ObjectStateManager.</span></span> <span data-ttu-id="16dd5-339">Pokud již existuje entita se stejnými klíči, bude zahrnuta do výsledků dotazu.</span><span class="sxs-lookup"><span data-stu-id="16dd5-339">If an entity with the same keys is already present EF will include it in the results of the query.</span></span> <span data-ttu-id="16dd5-340">I když EF bude i nadále vystavovat dotaz na databázi, může toto chování obejít většinu nákladů na vyhodnocování entitu víckrát.</span><span class="sxs-lookup"><span data-stu-id="16dd5-340">Although EF will still issue the query against the database, this behavior can bypass much of the cost of materializing the entity multiple times.</span></span>

#### <a name="311-getting-entities-from-the-object-cache-using-dbcontext-find"></a><span data-ttu-id="16dd5-341">3.1.1 získání entit z mezipaměti objektů pomocí hledání DbContext</span><span class="sxs-lookup"><span data-stu-id="16dd5-341">3.1.1 Getting entities from the object cache using DbContext Find</span></span>

<span data-ttu-id="16dd5-342">Na rozdíl od regulárního dotazu, metoda Find v Negenerickými (rozhraní API zahrnutá poprvé v EF 4,1) provede hledání v paměti před tím, než se dotaz vystaví na databázi.</span><span class="sxs-lookup"><span data-stu-id="16dd5-342">Unlike a regular query, the Find method in DbSet (APIs included for the first time in EF 4.1) will perform a search in memory before even issuing the query against the database.</span></span> <span data-ttu-id="16dd5-343">Je důležité si uvědomit, že dvě různé instance ObjectContext budou mít dvě různé instance ObjectStateManager, což znamená, že mají oddělené mezipaměti objektů.</span><span class="sxs-lookup"><span data-stu-id="16dd5-343">It’s important to note that two different ObjectContext instances will have two different ObjectStateManager instances, meaning that they have separate object caches.</span></span>

<span data-ttu-id="16dd5-344">Při hledání se pomocí hodnoty primárního klíče pokusí najít entitu sledovanou kontextem.</span><span class="sxs-lookup"><span data-stu-id="16dd5-344">Find uses the primary key value to attempt to find an entity tracked by the context.</span></span> <span data-ttu-id="16dd5-345">Pokud entita není v kontextu, bude dotaz proveden a vyhodnocován proti databázi a vrátí hodnotu null, pokud entita nebyla nalezena v kontextu nebo v databázi.</span><span class="sxs-lookup"><span data-stu-id="16dd5-345">If the entity is not in the context then a query will be executed and evaluated against the database, and null is returned if the entity is not found in the context or in the database.</span></span> <span data-ttu-id="16dd5-346">Všimněte si, že funkce Find vrátí také entity, které byly přidány do kontextu, ale ještě nebyly uloženy do databáze.</span><span class="sxs-lookup"><span data-stu-id="16dd5-346">Note that Find also returns entities that have been added to the context but have not yet been saved to the database.</span></span>

<span data-ttu-id="16dd5-347">Při použití Find se dá vzít v úvahu výkon.</span><span class="sxs-lookup"><span data-stu-id="16dd5-347">There is a performance consideration to be taken when using Find.</span></span> <span data-ttu-id="16dd5-348">Volání této metody ve výchozím nastavení spustí ověřování mezipaměti objektů za účelem zjištění změn, které jsou stále nečekají na potvrzení databáze.</span><span class="sxs-lookup"><span data-stu-id="16dd5-348">Invocations to this method by default will trigger a validation of the object cache in order to detect changes that are still pending commit to the database.</span></span> <span data-ttu-id="16dd5-349">Tento proces může být velmi nákladný, pokud je v mezipaměti objektů velký počet objektů nebo pokud je v grafu rozsáhlých objektů přidaných do mezipaměti objektů, ale může být také zakázán.</span><span class="sxs-lookup"><span data-stu-id="16dd5-349">This process can be very expensive if there are a very large number of objects in the object cache or in a large object graph being added to the object cache, but it can also be disabled.</span></span> <span data-ttu-id="16dd5-350">V některých případech můžete vnímat v pořadí podle velikosti rozdílu v volání metody Find, pokud zakážete automatické rozpoznávání změn.</span><span class="sxs-lookup"><span data-stu-id="16dd5-350">In certain cases, you may perceive over an order of magnitude of difference in calling the Find method when you disable auto detect changes.</span></span> <span data-ttu-id="16dd5-351">Druhé pořadí velikostí je však vnímano, když je objekt ve skutečnosti v mezipaměti, a když je nutné načíst objekt z databáze.</span><span class="sxs-lookup"><span data-stu-id="16dd5-351">Yet a second order of magnitude is perceived when the object actually is in the cache versus when the object has to be retrieved from the database.</span></span> <span data-ttu-id="16dd5-352">Tady je příklad grafu s měřeními provedenými pomocí některých mikrosrovnávacích testů vyjádřených v milisekundách s zatížením 5000 entit:</span><span class="sxs-lookup"><span data-stu-id="16dd5-352">Here is an example graph with measurements taken using some of our microbenchmarks, expressed in milliseconds, with a load of 5000 entities:</span></span>

<span data-ttu-id="16dd5-353">![.Net 4,5, logaritmické měřítko](~/ef6/media/net45logscale.png ".NET 4,5 – logaritmické měřítko")</span><span class="sxs-lookup"><span data-stu-id="16dd5-353">![.NET 4.5 logarithmic scale](~/ef6/media/net45logscale.png ".NET 4.5 - logarithmic scale")</span></span>

<span data-ttu-id="16dd5-354">Příklad hledání s automatickým rozpoznáním změn je zakázaný:</span><span class="sxs-lookup"><span data-stu-id="16dd5-354">Example of Find with auto-detect changes disabled:</span></span>

``` csharp
    context.Configuration.AutoDetectChangesEnabled = false;
    var product = context.Products.Find(productId);
    context.Configuration.AutoDetectChangesEnabled = true;
    ...
```

<span data-ttu-id="16dd5-355">Co je potřeba vzít v úvahu při použití metody Find:</span><span class="sxs-lookup"><span data-stu-id="16dd5-355">What you have to consider when using the Find method is:</span></span>

1.  <span data-ttu-id="16dd5-356">Pokud objekt není v mezipaměti, výhody hledání jsou negaci, ale syntaxe je stále jednodušší než dotaz podle klíče.</span><span class="sxs-lookup"><span data-stu-id="16dd5-356">If the object is not in the cache the benefits of Find are negated, but the syntax is still simpler than a query by key.</span></span>
2.  <span data-ttu-id="16dd5-357">Pokud je povoleno automatické rozpoznávání změn, náklady metody Find mohou být v závislosti na složitosti modelu a množství entit v mezipaměti objektů zvětšeny o jedno pořadí nebo ještě více.</span><span class="sxs-lookup"><span data-stu-id="16dd5-357">If auto detect changes is enabled the cost of the Find method may increase by one order of magnitude, or even more depending on the complexity of your model and the amount of entities in your object cache.</span></span>

<span data-ttu-id="16dd5-358">Mějte také na paměti, že funkce Find vrátí pouze hledanou entitu a nenačte automaticky přidružené entity, pokud ještě nejsou v mezipaměti objektů.</span><span class="sxs-lookup"><span data-stu-id="16dd5-358">Also, keep in mind that Find only returns the entity you are looking for and it does not automatically loads its associated entities if they are not already in the object cache.</span></span> <span data-ttu-id="16dd5-359">Pokud potřebujete načíst přidružené entity, můžete použít dotaz podle klíče s Eager načítání.</span><span class="sxs-lookup"><span data-stu-id="16dd5-359">If you need to retrieve associated entities, you can use a query by key with eager loading.</span></span> <span data-ttu-id="16dd5-360">Další informace naleznete v tématu \*\*8,1 opožděné načítání vs. Eager načítání @ no__t-0.</span><span class="sxs-lookup"><span data-stu-id="16dd5-360">For more information see **8.1 Lazy Loading vs. Eager Loading**.</span></span>

#### <a name="312-performance-issues-when-the-object-cache-has-many-entities"></a><span data-ttu-id="16dd5-361">problémy s výkonem 3.1.2, pokud má mezipaměť objektů mnoho entit</span><span class="sxs-lookup"><span data-stu-id="16dd5-361">3.1.2 Performance issues when the object cache has many entities</span></span>

<span data-ttu-id="16dd5-362">Mezipaměť objektů pomáhá zvýšit celkovou rychlost odezvy Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="16dd5-362">The object cache helps to increase the overall responsiveness of Entity Framework.</span></span> <span data-ttu-id="16dd5-363">Pokud však mezipaměť objektů má nahrané velké množství entit, může to mít vliv na určité operace, jako je přidání, odebrání, vyhledání, zadání, SaveChanges a další.</span><span class="sxs-lookup"><span data-stu-id="16dd5-363">However, when the object cache has a very large amount of entities loaded it may affect certain operations such as Add, Remove, Find, Entry, SaveChanges and more.</span></span> <span data-ttu-id="16dd5-364">Konkrétně operace, které aktivují volání DetectChanges, budou mít negativně vliv na velmi velké mezipaměti objektů.</span><span class="sxs-lookup"><span data-stu-id="16dd5-364">In particular, operations that trigger a call to DetectChanges will be negatively affected by very large object caches.</span></span> <span data-ttu-id="16dd5-365">DetectChanges synchronizuje graf objektů se správcem stavu objektu a jeho výkon se určí přímo podle velikosti grafu objektů.</span><span class="sxs-lookup"><span data-stu-id="16dd5-365">DetectChanges synchronizes the object graph with the object state manager and its performance will determined directly by the size of the object graph.</span></span> <span data-ttu-id="16dd5-366">Další informace o DetectChanges najdete v tématu [sledování změn v entitách POCO](https://msdn.microsoft.com/library/dd456848.aspx).</span><span class="sxs-lookup"><span data-stu-id="16dd5-366">For more information about DetectChanges, see [Tracking Changes in POCO Entities](https://msdn.microsoft.com/library/dd456848.aspx).</span></span>

<span data-ttu-id="16dd5-367">Při použití Entity Framework 6 můžou vývojáři volat AddRange a RemoveRange přímo na Negenerickými místo na iteraci v kolekci a volat metodu Add jednou na instanci.</span><span class="sxs-lookup"><span data-stu-id="16dd5-367">When using Entity Framework 6, developers are able to call AddRange and RemoveRange directly on a DbSet, instead of iterating on a collection and calling Add once per instance.</span></span> <span data-ttu-id="16dd5-368">Výhodou použití metod rozsahu je, že náklady na DetectChanges jsou placeny pouze jednou pro celou sadu entit, a to na rozdíl od jednou pro každou přidanou entitu.</span><span class="sxs-lookup"><span data-stu-id="16dd5-368">The advantage of using the range methods is that the cost of DetectChanges is only paid once for the entire set of entities as opposed to once per each added entity.</span></span>

### <a name="32-query-plan-caching"></a><span data-ttu-id="16dd5-369">Mezipaměť plánu dotazů 3,2</span><span class="sxs-lookup"><span data-stu-id="16dd5-369">3.2 Query Plan Caching</span></span>

<span data-ttu-id="16dd5-370">Při prvním spuštění dotazu projde interním kompilátorem plánu, který převede koncepční dotaz do příkazu Store (například T-SQL, který se spustí při spuštění proti SQL Server).</span><span class="sxs-lookup"><span data-stu-id="16dd5-370">The first time a query is executed, it goes through the internal plan compiler to translate the conceptual query into the store command (for example, the T-SQL which is executed when run against SQL Server).</span></span><span data-ttu-id="16dd5-371">  Pokud je povoleno ukládání plánů dotazů do mezipaměti, při příštím spuštění dotazu se příkaz Store načte přímo z mezipaměti plánu dotazu ke spuštění, přičemž se vynechá kompilátor plánu.</span><span class="sxs-lookup"><span data-stu-id="16dd5-371">  If query plan caching is enabled, the next time the query is executed the store command is retrieved directly from the query plan cache for execution, bypassing the plan compiler.</span></span>

<span data-ttu-id="16dd5-372">Mezipaměť plánu dotazu je sdílena mezi instancemi ObjectContext v rámci stejné domény AppDomain.</span><span class="sxs-lookup"><span data-stu-id="16dd5-372">The query plan cache is shared across ObjectContext instances within the same AppDomain.</span></span> <span data-ttu-id="16dd5-373">Nemusíte podržet instanci ObjectContext, abyste mohli těžit z ukládání do mezipaměti plánu dotazů.</span><span class="sxs-lookup"><span data-stu-id="16dd5-373">You don't need to hold onto an ObjectContext instance to benefit from query plan caching.</span></span>

#### <a name="321-some-notes-about-query-plan-caching"></a><span data-ttu-id="16dd5-374">3.2.1 poznámky k ukládání plánu dotazů do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="16dd5-374">3.2.1 Some notes about Query Plan Caching</span></span>

-   <span data-ttu-id="16dd5-375">Mezipaměť plánu dotazu je sdílena pro všechny typy dotazů: Objekty Entity SQL, LINQ to Entities a CompiledQuery.</span><span class="sxs-lookup"><span data-stu-id="16dd5-375">The query plan cache is shared for all query types: Entity SQL, LINQ to Entities, and CompiledQuery objects.</span></span>
-   <span data-ttu-id="16dd5-376">Ve výchozím nastavení je ukládání do mezipaměti plánu dotazů povoleno pro Entity SQL dotazy, ať už prováděné prostřednictvím EntityCommand nebo prostřednictvím ObjectQuery.</span><span class="sxs-lookup"><span data-stu-id="16dd5-376">By default, query plan caching is enabled for Entity SQL queries, whether executed through an EntityCommand or through an ObjectQuery.</span></span> <span data-ttu-id="16dd5-377">Ve výchozím nastavení je povolená taky pro LINQ to Entities dotazy v Entity Framework na platformě .NET 4,5 a v Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="16dd5-377">It is also enabled by default for LINQ to Entities queries in Entity Framework on .NET 4.5, and in Entity Framework 6</span></span>
    -   <span data-ttu-id="16dd5-378">Ukládání plánu dotazu do mezipaměti lze zakázat nastavením vlastnosti EnablePlanCaching (na EntityCommand nebo ObjectQuery) na false.</span><span class="sxs-lookup"><span data-stu-id="16dd5-378">Query plan caching can be disabled by setting the EnablePlanCaching property (on EntityCommand or ObjectQuery) to false.</span></span> <span data-ttu-id="16dd5-379">Příklad:</span><span class="sxs-lookup"><span data-stu-id="16dd5-379">For example:</span></span>
``` csharp
                    var query = from customer in context.Customer
                                where customer.CustomerId == id
                                select new
                                {
                                    customer.CustomerId,
                                    customer.Name
                                };
                    ObjectQuery oQuery = query as ObjectQuery;
                    oQuery.EnablePlanCaching = false;
```
-   <span data-ttu-id="16dd5-380">U parametrizovaných dotazů se při změně hodnoty parametru stále objeví dotaz uložený v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="16dd5-380">For parameterized queries, changing the parameter's value will still hit the cached query.</span></span> <span data-ttu-id="16dd5-381">Ale změna omezujících vlastností parametru (například Size, Precision nebo Scale), bude mít za následek jinou položku v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="16dd5-381">But changing a parameter's facets (for example, size, precision, or scale) will hit a different entry in the cache.</span></span>
-   <span data-ttu-id="16dd5-382">Při použití Entity SQL je řetězec dotazu součástí klíče.</span><span class="sxs-lookup"><span data-stu-id="16dd5-382">When using Entity SQL, the query string is part of the key.</span></span> <span data-ttu-id="16dd5-383">Pokud se dotaz změní na všechny, bude mít za následek jinou položku mezipaměti, i když dotazy budou funkčně ekvivalentní.</span><span class="sxs-lookup"><span data-stu-id="16dd5-383">Changing the query at all will result in different cache entries, even if the queries are functionally equivalent.</span></span> <span data-ttu-id="16dd5-384">To zahrnuje změny velikosti písmen nebo prázdných znaků.</span><span class="sxs-lookup"><span data-stu-id="16dd5-384">This includes changes to casing or whitespace.</span></span>
-   <span data-ttu-id="16dd5-385">Při použití LINQ je zpracován dotaz pro vygenerování části klíče.</span><span class="sxs-lookup"><span data-stu-id="16dd5-385">When using LINQ, the query is processed to generate a part of the key.</span></span> <span data-ttu-id="16dd5-386">Změna výrazu LINQ proto vygeneruje jiný klíč.</span><span class="sxs-lookup"><span data-stu-id="16dd5-386">Changing the LINQ expression will therefore generate a different key.</span></span>
-   <span data-ttu-id="16dd5-387">Mohou platit další technická omezení; Další podrobnosti najdete v tématu o autokompilovaných dotazech.</span><span class="sxs-lookup"><span data-stu-id="16dd5-387">Other technical limitations may apply; see Autocompiled Queries for more details.</span></span>

#### <a name="322-cache-eviction-algorithm"></a><span data-ttu-id="16dd5-388">algoritmus vyřazení mezipaměti 3.2.2</span><span class="sxs-lookup"><span data-stu-id="16dd5-388">3.2.2      Cache eviction algorithm</span></span>

<span data-ttu-id="16dd5-389">Princip fungování interního algoritmu vám pomůže zjistit, kdy se má povolit nebo zakázat ukládání do mezipaměti plánu dotazů.</span><span class="sxs-lookup"><span data-stu-id="16dd5-389">Understanding how the internal algorithm works will help you figure out when to enable or disable query plan caching.</span></span> <span data-ttu-id="16dd5-390">Čistící algoritmus je následující:</span><span class="sxs-lookup"><span data-stu-id="16dd5-390">The cleanup algorithm is as follows:</span></span>

1.  <span data-ttu-id="16dd5-391">Jakmile mezipaměť obsahuje stanovený počet položek (800), spustíme časovač, který pravidelně (jednou za minutu) vypíše mezipaměť.</span><span class="sxs-lookup"><span data-stu-id="16dd5-391">Once the cache contains a set number of entries (800), we start a timer that periodically (once-per-minute) sweeps the cache.</span></span>
2.  <span data-ttu-id="16dd5-392">Během úklidu mezipaměti se položky z mezipaměti odeberou na LFRU (nejméně často – nedávno používané).</span><span class="sxs-lookup"><span data-stu-id="16dd5-392">During cache sweeps, entries are removed from the cache on a LFRU (Least frequently – recently used) basis.</span></span> <span data-ttu-id="16dd5-393">Při rozhodování o tom, které položky se mají vysunout, tento algoritmus vezme v úvahu jak počet přístupů, tak i stáří.</span><span class="sxs-lookup"><span data-stu-id="16dd5-393">This algorithm takes both hit count and age into account when deciding which entries are ejected.</span></span>
3.  <span data-ttu-id="16dd5-394">Na konci každého úklidu mezipaměti mezipaměť opět obsahuje 800 záznamů.</span><span class="sxs-lookup"><span data-stu-id="16dd5-394">At the end of each cache sweep, the cache again contains 800 entries.</span></span>

<span data-ttu-id="16dd5-395">Při určování, které položky se mají vyřadit, se všechny položky mezipaměti zpracovávají stejně.</span><span class="sxs-lookup"><span data-stu-id="16dd5-395">All cache entries are treated equally when determining which entries to evict.</span></span> <span data-ttu-id="16dd5-396">To znamená, že příkaz Store pro CompiledQuery má stejnou možnost vyřazení jako příkaz Store pro dotaz Entity SQL.</span><span class="sxs-lookup"><span data-stu-id="16dd5-396">This means the store command for a CompiledQuery has the same chance of eviction as the store command for an Entity SQL query.</span></span>

<span data-ttu-id="16dd5-397">Všimněte si, že časovač vyřazení mezipaměti je v době, kdy jsou v mezipaměti dostupné entity 800, ale mezipaměť je Swept jenom 60 sekund po spuštění tohoto časovače.</span><span class="sxs-lookup"><span data-stu-id="16dd5-397">Note that the cache eviction timer is kicked in when there are 800 entities in the cache, but the cache is only swept 60 seconds after this timer is started.</span></span> <span data-ttu-id="16dd5-398">To znamená, že až 60 sekund může mezipaměť trvat poměrně velkou velikost.</span><span class="sxs-lookup"><span data-stu-id="16dd5-398">That means that for up to 60 seconds your cache may grow to be quite large.</span></span>

#### <a name="323-test-metrics-demonstrating-query-plan-caching-performance"></a><span data-ttu-id="16dd5-399">3.2.3 metriky testů, které demonstrují výkon mezipaměti plánu dotazů</span><span class="sxs-lookup"><span data-stu-id="16dd5-399">3.2.3       Test Metrics demonstrating query plan caching performance</span></span>

<span data-ttu-id="16dd5-400">Pro ukázku účinku ukládání do mezipaměti plánu dotazů na výkon vaší aplikace jsme provedli test, ve kterém jsme provedli několik dotazů Entity SQL pro model Navision.</span><span class="sxs-lookup"><span data-stu-id="16dd5-400">To demonstrate the effect of query plan caching on your application's performance, we performed a test where we executed a number of Entity SQL queries against the Navision model.</span></span> <span data-ttu-id="16dd5-401">Popis modelu Navision a typy dotazů, které se provedly, najdete v příloze.</span><span class="sxs-lookup"><span data-stu-id="16dd5-401">See the appendix for a description of the Navision model and the types of queries which were executed.</span></span> <span data-ttu-id="16dd5-402">V tomto testu nejdřív projdeme seznam dotazů a jednou se jednou provede jejich přidání do mezipaměti (Pokud je povolené ukládání do mezipaměti).</span><span class="sxs-lookup"><span data-stu-id="16dd5-402">In this test, we first iterate through the list of queries and execute each one once to add them to the cache (if caching is enabled).</span></span> <span data-ttu-id="16dd5-403">Tento krok je nečasový.</span><span class="sxs-lookup"><span data-stu-id="16dd5-403">This step is untimed.</span></span> <span data-ttu-id="16dd5-404">V dalším kroku se v hlavním vlákně po dobu více než 60 sekund uloží, aby bylo možné provést čištění mezipaměti. Nakonec projdeme seznam a za druhý čas pro spuštění dotazů uložených v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="16dd5-404">Next, we sleep the main thread for over 60 seconds to allow cache sweeping to take place; finally, we iterate through the list a 2nd time to execute the cached queries.</span></span> <span data-ttu-id="16dd5-405">Kromě toho je mezipaměť plánu SQL Server vyprázdněna před provedením každé sady dotazů, aby časy, které získají přesně, odrážely výhody poskytnuté mezipamětí plánu dotazu.</span><span class="sxs-lookup"><span data-stu-id="16dd5-405">Additionally, the SQL Server plan cache is flushed before each set of queries is executed so that the times we obtain accurately reflect the benefit given by the query plan cache.</span></span>

##### <a name="3231-test-results"></a><span data-ttu-id="16dd5-406">3.2.3.1 Výsledky testů</span><span class="sxs-lookup"><span data-stu-id="16dd5-406">3.2.3.1       Test Results</span></span>

| <span data-ttu-id="16dd5-407">Test</span><span class="sxs-lookup"><span data-stu-id="16dd5-407">Test</span></span>                                                                   | <span data-ttu-id="16dd5-408">EF5 bez mezipaměti</span><span class="sxs-lookup"><span data-stu-id="16dd5-408">EF5 no cache</span></span> | <span data-ttu-id="16dd5-409">EF5 v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="16dd5-409">EF5 cached</span></span> | <span data-ttu-id="16dd5-410">EF6 bez mezipaměti</span><span class="sxs-lookup"><span data-stu-id="16dd5-410">EF6 no cache</span></span> | <span data-ttu-id="16dd5-411">EF6 v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="16dd5-411">EF6 cached</span></span> |
|:-----------------------------------------------------------------------|:-------------|:-----------|:-------------|:-----------|
| <span data-ttu-id="16dd5-412">Vytváření výčtu všech dotazů 18723</span><span class="sxs-lookup"><span data-stu-id="16dd5-412">Enumerating all 18723 queries</span></span>                                          | <span data-ttu-id="16dd5-413">124</span><span class="sxs-lookup"><span data-stu-id="16dd5-413">124</span></span>          | <span data-ttu-id="16dd5-414">125,4</span><span class="sxs-lookup"><span data-stu-id="16dd5-414">125.4</span></span>      | <span data-ttu-id="16dd5-415">124,3</span><span class="sxs-lookup"><span data-stu-id="16dd5-415">124.3</span></span>        | <span data-ttu-id="16dd5-416">125,3</span><span class="sxs-lookup"><span data-stu-id="16dd5-416">125.3</span></span>      |
| <span data-ttu-id="16dd5-417">Zamezení čištění (jenom prvních 800 dotazů, bez ohledu na složitost)</span><span class="sxs-lookup"><span data-stu-id="16dd5-417">Avoiding sweep (just the first 800 queries, regardless of complexity)</span></span>  | <span data-ttu-id="16dd5-418">41,7</span><span class="sxs-lookup"><span data-stu-id="16dd5-418">41.7</span></span>         | <span data-ttu-id="16dd5-419">5.5</span><span class="sxs-lookup"><span data-stu-id="16dd5-419">5.5</span></span>        | <span data-ttu-id="16dd5-420">40.5</span><span class="sxs-lookup"><span data-stu-id="16dd5-420">40.5</span></span>         | <span data-ttu-id="16dd5-421">5.4</span><span class="sxs-lookup"><span data-stu-id="16dd5-421">5.4</span></span>        |
| <span data-ttu-id="16dd5-422">Jenom dotazy AggregatingSubtotals (celkem 178 – zabrání se tak sweep)</span><span class="sxs-lookup"><span data-stu-id="16dd5-422">Just the AggregatingSubtotals queries (178 total - which avoids sweep)</span></span> | <span data-ttu-id="16dd5-423">39,5</span><span class="sxs-lookup"><span data-stu-id="16dd5-423">39.5</span></span>         | <span data-ttu-id="16dd5-424">4.5</span><span class="sxs-lookup"><span data-stu-id="16dd5-424">4.5</span></span>        | <span data-ttu-id="16dd5-425">38,1</span><span class="sxs-lookup"><span data-stu-id="16dd5-425">38.1</span></span>         | <span data-ttu-id="16dd5-426">4.6</span><span class="sxs-lookup"><span data-stu-id="16dd5-426">4.6</span></span>        |

<span data-ttu-id="16dd5-427">*Všechny časy v sekundách.*</span><span class="sxs-lookup"><span data-stu-id="16dd5-427">*All times in seconds.*</span></span>

<span data-ttu-id="16dd5-428">Morální – při provádění spousty různých dotazů (například dynamicky se vytvořily dotazy), ukládání do mezipaměti neusnadní a výsledné vyprázdnění mezipaměti může uchovávat dotazy, které by mohly využít maximum z plánu do ukládání do mezipaměti v důsledku jejich použití.</span><span class="sxs-lookup"><span data-stu-id="16dd5-428">Moral - when executing lots of distinct queries (for example,  dynamically created queries), caching doesn't help and the resulting flushing of the cache can keep the queries that would benefit the most from plan caching from actually using it.</span></span>

<span data-ttu-id="16dd5-429">Dotazy AggregatingSubtotals jsou nejsložitější z dotazů, které jsme testovali pomocí.</span><span class="sxs-lookup"><span data-stu-id="16dd5-429">The AggregatingSubtotals queries are the most complex of the queries we tested with.</span></span> <span data-ttu-id="16dd5-430">Jak je očekáváno, složitější dotaz je, tím větší je přínos, který se zobrazí při ukládání plánu dotazů do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="16dd5-430">As expected, the more complex the query is, the more benefit you will see from query plan caching.</span></span>

<span data-ttu-id="16dd5-431">Vzhledem k tomu, že CompiledQuery je ve skutečnosti dotaz LINQ s plánem uloženým do mezipaměti, porovnání CompiledQuery oproti ekvivalentnímu dotazu Entity SQL by mělo mít podobné výsledky.</span><span class="sxs-lookup"><span data-stu-id="16dd5-431">Because a CompiledQuery is really a LINQ query with its plan cached, the comparison of a CompiledQuery versus the equivalent Entity SQL query should have similar results.</span></span> <span data-ttu-id="16dd5-432">Ve skutečnosti platí, že pokud má aplikace spoustu dynamických Entity SQL dotazů, vyplňování mezipaměti v dotazech také efektivně způsobí, že CompiledQueries "dekompilovat", když jsou vyprázdněny z mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="16dd5-432">In fact, if an app has lots of dynamic Entity SQL queries, filling the cache with queries will also effectively cause CompiledQueries to “decompile” when they are flushed from the cache.</span></span> <span data-ttu-id="16dd5-433">V tomto scénáři můžete zlepšit výkon tím, že zakážete ukládání do mezipaměti v dynamických dotazech, abyste naCompiledQueriesi prioritu.</span><span class="sxs-lookup"><span data-stu-id="16dd5-433">In this scenario, performance may be improved by disabling caching on the dynamic queries to prioritize the CompiledQueries.</span></span> <span data-ttu-id="16dd5-434">Ještě lepší je, že by bylo nutné aplikaci přepsat tak, aby místo dynamických dotazů používala parametrizované dotazy.</span><span class="sxs-lookup"><span data-stu-id="16dd5-434">Better yet, of course, would be to rewrite the app to use parameterized queries instead of dynamic queries.</span></span>

### <a name="33-using-compiledquery-to-improve-performance-with-linq-queries"></a><span data-ttu-id="16dd5-435">3,3 použití CompiledQuery pro zlepšení výkonu pomocí dotazů LINQ</span><span class="sxs-lookup"><span data-stu-id="16dd5-435">3.3 Using CompiledQuery to improve performance with LINQ queries</span></span>

<span data-ttu-id="16dd5-436">Naše testy označují, že použití CompiledQuery může přinést více než 7% u autokompilovaných dotazů LINQ; To znamená, že strávíte o 7% méně času spouštěním kódu z Entity Framework stacku; neznamená to, že vaše aplikace bude o 7% rychlejší.</span><span class="sxs-lookup"><span data-stu-id="16dd5-436">Our tests indicate that using CompiledQuery can bring a benefit of 7% over autocompiled LINQ queries; this means that you’ll spend 7% less time executing code from the Entity Framework stack; it does not mean your application will be 7% faster.</span></span> <span data-ttu-id="16dd5-437">Obecně řečeno, náklady na psaní a udržování CompiledQuerych objektů v EF 5,0 nemusí být v porovnání s výhodami v hodnotě problému.</span><span class="sxs-lookup"><span data-stu-id="16dd5-437">Generally speaking, the cost of writing and maintaining CompiledQuery objects in EF 5.0 may not be worth the trouble when compared to the benefits.</span></span> <span data-ttu-id="16dd5-438">Vaše kilometry se mohou lišit. tuto možnost můžete využít, pokud váš projekt vyžaduje nadbytečné vložení.</span><span class="sxs-lookup"><span data-stu-id="16dd5-438">Your mileage may vary, so exercise this option if your project requires the extra push.</span></span> <span data-ttu-id="16dd5-439">Všimněte si, že CompiledQueries jsou kompatibilní pouze s modely odvozenými pomocí ObjectContext a nejsou kompatibilní s modely odvozenými DbContext.</span><span class="sxs-lookup"><span data-stu-id="16dd5-439">Note that CompiledQueries are only compatible with ObjectContext-derived models, and not compatible with DbContext-derived models.</span></span>

<span data-ttu-id="16dd5-440">Další informace o vytvoření a vyvolání CompiledQuery naleznete v tématu [kompilované dotazy (LINQ to Entities)](https://msdn.microsoft.com/library/bb896297.aspx).</span><span class="sxs-lookup"><span data-stu-id="16dd5-440">For more information on creating and invoking a CompiledQuery, see [Compiled Queries (LINQ to Entities)](https://msdn.microsoft.com/library/bb896297.aspx).</span></span>

<span data-ttu-id="16dd5-441">Existují dva předpoklady, které je třeba provést při použití CompiledQuery, konkrétně požadavek na použití statických instancí a problémů, které mají s možností vytváření.</span><span class="sxs-lookup"><span data-stu-id="16dd5-441">There are two considerations you have to take when using a CompiledQuery, namely the requirement to use static instances and the problems they have with composability.</span></span> <span data-ttu-id="16dd5-442">Zde jsou podrobnější vysvětlení těchto dvou důležitých informací.</span><span class="sxs-lookup"><span data-stu-id="16dd5-442">Here follows an in-depth explanation of these two considerations.</span></span>

#### <a name="331-use-static-compiledquery-instances"></a><span data-ttu-id="16dd5-443">3.3.1 použití statických instancí CompiledQuery</span><span class="sxs-lookup"><span data-stu-id="16dd5-443">3.3.1       Use static CompiledQuery instances</span></span>

<span data-ttu-id="16dd5-444">Vzhledem k tomu, že kompilování dotazu LINQ je časově náročný proces, nechci to udělat pokaždé, když potřebujeme načíst data z databáze.</span><span class="sxs-lookup"><span data-stu-id="16dd5-444">Since compiling a LINQ query is a time-consuming process, we don’t want to do it every time we need to fetch data from the database.</span></span> <span data-ttu-id="16dd5-445">Instance CompiledQuery vám umožňují kompilovat jednou a běžet několikrát, ale musíte být opatrní a při každém pokusu znovu znovu použít stejnou instanci CompiledQuery, aniž byste ji museli kompilovat znovu a znovu používat.</span><span class="sxs-lookup"><span data-stu-id="16dd5-445">CompiledQuery instances allow you to compile once and run multiple times, but you have to be careful and procure to re-use the same CompiledQuery instance every time instead of compiling it over and over again.</span></span> <span data-ttu-id="16dd5-446">Použití statických členů k uložení instancí CompiledQuery bude nezbytné. jinak se vám neprojeví žádné výhody.</span><span class="sxs-lookup"><span data-stu-id="16dd5-446">The use of static members to store the CompiledQuery instances becomes necessary; otherwise you won’t see any benefit.</span></span>

<span data-ttu-id="16dd5-447">Předpokládejme například, že vaše stránka obsahuje následující tělo metody pro zpracování zobrazení produktů pro vybranou kategorii:</span><span class="sxs-lookup"><span data-stu-id="16dd5-447">For example, suppose your page has the following method body to handle displaying the products for the selected category:</span></span>

``` csharp
    // Warning: this is the wrong way of using CompiledQuery
    using (NorthwindEntities context = new NorthwindEntities())
    {
        string selectedCategory = this.categoriesList.SelectedValue;

        var productsForCategory = CompiledQuery.Compile<NorthwindEntities, string, IQueryable<Product>>(
            (NorthwindEntities nwnd, string category) =>
                nwnd.Products.Where(p => p.Category.CategoryName == category)
        );

        this.productsGrid.DataSource = productsForCategory.Invoke(context, selectedCategory).ToList();
        this.productsGrid.DataBind();
    }

    this.productsGrid.Visible = true;
```

<span data-ttu-id="16dd5-448">V tomto případě vytvoříte novou instanci CompiledQuery průběžně při každém volání metody.</span><span class="sxs-lookup"><span data-stu-id="16dd5-448">In this case, you will create a new CompiledQuery instance on the fly every time the method is called.</span></span> <span data-ttu-id="16dd5-449">Místo toho, aby se při načtení příkazu Store z mezipaměti plánu dotazů zobrazily výhody výkonu, CompiledQuery projde kompilátorem plánu pokaždé, když se vytvoří nová instance.</span><span class="sxs-lookup"><span data-stu-id="16dd5-449">Instead of seeing performance benefits by retrieving the store command from the query plan cache, the CompiledQuery will go through the plan compiler every time a new instance is created.</span></span> <span data-ttu-id="16dd5-450">Ve skutečnosti budete při každém volání metody znečištěni mezipaměť plánu dotazů novou položkou CompiledQuery.</span><span class="sxs-lookup"><span data-stu-id="16dd5-450">In fact, you will be polluting your query plan cache with a new CompiledQuery entry every time the method is called.</span></span>

<span data-ttu-id="16dd5-451">Místo toho chcete vytvořit statickou instanci zkompilovaného dotazu, takže vyvoláte stejný kompilovaný dotaz pokaždé, když je metoda volána.</span><span class="sxs-lookup"><span data-stu-id="16dd5-451">Instead, you want to create a static instance of the compiled query, so you are invoking the same compiled query every time the method is called.</span></span> <span data-ttu-id="16dd5-452">Jedním z těchto způsobů je přidat instanci CompiledQuery jako člena kontextu objektu.</span><span class="sxs-lookup"><span data-stu-id="16dd5-452">One way to so this is by adding the CompiledQuery instance as a member of your object context.</span></span><span data-ttu-id="16dd5-453">  Pak můžete udělat něco trochu čistšího, když k CompiledQuery přistupujete prostřednictvím pomocné metody:</span><span class="sxs-lookup"><span data-stu-id="16dd5-453">  You can then make things a little cleaner by accessing the CompiledQuery through a helper method:</span></span>

``` csharp
    public partial class NorthwindEntities : ObjectContext
    {
        private static readonly Func<NorthwindEntities, string, IEnumerable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
            (NorthwindEntities context, string categoryName) =>
                context.Products.Where(p => p.Category.CategoryName == categoryName)
            );

        public IEnumerable<Product> GetProductsForCategory(string categoryName)
        {
            return productsForCategoryCQ.Invoke(this, categoryName).ToList();
        }
```

<span data-ttu-id="16dd5-454">Tato pomocná metoda by se vyvolala takto:</span><span class="sxs-lookup"><span data-stu-id="16dd5-454">This helper method would be invoked as follows:</span></span>

``` csharp
    this.productsGrid.DataSource = context.GetProductsForCategory(selectedCategory);
```

#### <a name="332-composing-over-a-compiledquery"></a><span data-ttu-id="16dd5-455">3.3.2 vytváření přes CompiledQuery</span><span class="sxs-lookup"><span data-stu-id="16dd5-455">3.3.2       Composing over a CompiledQuery</span></span>

<span data-ttu-id="16dd5-456">Možnost vytvářet v jakémkoli dotazu LINQ je mimořádně užitečná; k tomu stačí vyvolat metodu za rozhraním IQueryable, jako je *Skip ()* nebo *Count ()* .</span><span class="sxs-lookup"><span data-stu-id="16dd5-456">The ability to compose over any LINQ query is extremely useful; to do this, you simply invoke a method after the IQueryable such as *Skip()* or *Count()*.</span></span> <span data-ttu-id="16dd5-457">Nicméně v podstatě vrátí nový objekt IQueryable.</span><span class="sxs-lookup"><span data-stu-id="16dd5-457">However, doing so essentially returns a new IQueryable object.</span></span> <span data-ttu-id="16dd5-458">I když není k dispozici nic, co by bylo možné zamezit vytváření přes CompiledQuery, způsobí to, že generování nového objektu IQueryable, který vyžaduje předání prostřednictvím kompilátoru plánu, bude zase.</span><span class="sxs-lookup"><span data-stu-id="16dd5-458">While there’s nothing to stop you technically from composing over a CompiledQuery, doing so will cause the generation of a new IQueryable object that requires passing through the plan compiler again.</span></span>

<span data-ttu-id="16dd5-459">Některé součásti využívají složené objekty IQueryable k povolení pokročilých funkcí.</span><span class="sxs-lookup"><span data-stu-id="16dd5-459">Some components will make use of composed IQueryable objects to enable advanced functionality.</span></span> <span data-ttu-id="16dd5-460">Například ASP. Objekt GridView v síti může být svázán s daty objektu IQueryable prostřednictvím vlastnosti SelectMethod.</span><span class="sxs-lookup"><span data-stu-id="16dd5-460">For example, ASP.NET’s GridView can be data-bound to an IQueryable object via the SelectMethod property.</span></span> <span data-ttu-id="16dd5-461">Prvek GridView potom vytvoří tento objekt IQueryable a umožní řazení a stránkování nad datovým modelem.</span><span class="sxs-lookup"><span data-stu-id="16dd5-461">The GridView will then compose over this IQueryable object to allow sorting and paging over the data model.</span></span> <span data-ttu-id="16dd5-462">Jak vidíte, použití CompiledQuery pro prvek GridView by nemuselo mít kompilovaný dotaz, ale vygenerovalo nový automaticky kompilovaný dotaz.</span><span class="sxs-lookup"><span data-stu-id="16dd5-462">As you can see, using a CompiledQuery for the GridView would not hit the compiled query but would generate a new autocompiled query.</span></span>

<span data-ttu-id="16dd5-463">Jedním z míst, kde můžete běžet, je přidání progresivních filtrů do dotazu.</span><span class="sxs-lookup"><span data-stu-id="16dd5-463">One place where you may run into this is when adding progressive filters to a query.</span></span> <span data-ttu-id="16dd5-464">Předpokládejme například, že byste měli stránku Customers (zákazníci) s několika rozevíracími seznamy pro volitelné filtry (například země a OrdersCount).</span><span class="sxs-lookup"><span data-stu-id="16dd5-464">For example, suppose you had a Customers page with several drop-down lists for optional filters (for example, Country and OrdersCount).</span></span> <span data-ttu-id="16dd5-465">Tyto filtry můžete vytvořit přes výsledky CompiledQuery IQueryable, ale pokud to uděláte, vytvoří se nový dotaz, který projde kompilátorem plánu pokaždé, když ho spustíte.</span><span class="sxs-lookup"><span data-stu-id="16dd5-465">You can compose these filters over the IQueryable results of a CompiledQuery, but doing so will result in the new query going through the plan compiler every time you execute it.</span></span>

``` csharp
    using (NorthwindEntities context = new NorthwindEntities())
    {
        IQueryable<Customer> myCustomers = context.InvokeCustomersForEmployee();

        if (this.orderCountFilterList.SelectedItem.Value != defaultFilterText)
        {
            int orderCount = int.Parse(orderCountFilterList.SelectedValue);
            myCustomers = myCustomers.Where(c => c.Orders.Count > orderCount);
        }

        if (this.countryFilterList.SelectedItem.Value != defaultFilterText)
        {
            myCustomers = myCustomers.Where(c => c.Address.Country == countryFilterList.SelectedValue);
        }

        this.customersGrid.DataSource = myCustomers;
        this.customersGrid.DataBind();
    }
```

 <span data-ttu-id="16dd5-466">Chcete-li se této opakované kompilaci vyhnout, můžete přepsat CompiledQuery a vzít v úvahu možné filtry:</span><span class="sxs-lookup"><span data-stu-id="16dd5-466">To avoid this re-compilation, you can rewrite the CompiledQuery to take the possible filters into account:</span></span>

``` csharp
    private static readonly Func<NorthwindEntities, int, int?, string, IQueryable<Customer>> customersForEmployeeWithFiltersCQ = CompiledQuery.Compile(
        (NorthwindEntities context, int empId, int? countFilter, string countryFilter) =>
            context.Customers.Where(c => c.Orders.Any(o => o.EmployeeID == empId))
            .Where(c => countFilter.HasValue == false || c.Orders.Count > countFilter)
            .Where(c => countryFilter == null || c.Address.Country == countryFilter)
        );
```

<span data-ttu-id="16dd5-467">Který by se vyvolal v uživatelském rozhraní jako:</span><span class="sxs-lookup"><span data-stu-id="16dd5-467">Which would be invoked in the UI like:</span></span>

``` csharp
    using (NorthwindEntities context = new NorthwindEntities())
    {
        int? countFilter = (this.orderCountFilterList.SelectedIndex == 0) ?
            (int?)null :
            int.Parse(this.orderCountFilterList.SelectedValue);

        string countryFilter = (this.countryFilterList.SelectedIndex == 0) ?
            null :
            this.countryFilterList.SelectedValue;

        IQueryable<Customer> myCustomers = context.InvokeCustomersForEmployeeWithFilters(
                countFilter, countryFilter);

        this.customersGrid.DataSource = myCustomers;
        this.customersGrid.DataBind();
    }
```

 <span data-ttu-id="16dd5-468">Kompromisy tady tvoří vygenerované úložiště, které bude mít vždycky filtry s kontrolními hodnotami null, ale u databázového serveru by se měly poměrně snadno optimalizovat:</span><span class="sxs-lookup"><span data-stu-id="16dd5-468">A tradeoff here is the generated store command will always have the filters with the null checks, but these should be fairly simple for the database server to optimize:</span></span>

``` SQL
...
WHERE ((0 = (CASE WHEN (@p__linq__1 IS NOT NULL) THEN cast(1 as bit) WHEN (@p__linq__1 IS NULL) THEN cast(0 as bit) END)) OR ([Project3].[C2] > @p__linq__2)) AND (@p__linq__3 IS NULL OR [Project3].[Country] = @p__linq__4)
```

### <a name="34-metadata-caching"></a><span data-ttu-id="16dd5-469">mezipaměť 3,4 metadat</span><span class="sxs-lookup"><span data-stu-id="16dd5-469">3.4 Metadata caching</span></span>

<span data-ttu-id="16dd5-470">Entity Framework také podporuje ukládání metadat do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="16dd5-470">The Entity Framework also supports Metadata caching.</span></span> <span data-ttu-id="16dd5-471">To je v podstatě ukládání informací o typech a mapování typu na databázi mezi různými připojeními ke stejnému modelu.</span><span class="sxs-lookup"><span data-stu-id="16dd5-471">This is essentially caching of type information and type-to-database mapping information across different connections to the same model.</span></span> <span data-ttu-id="16dd5-472">Mezipaměť metadat je jedinečná pro každou doménu AppDomain.</span><span class="sxs-lookup"><span data-stu-id="16dd5-472">The Metadata cache is unique per AppDomain.</span></span>

#### <a name="341-metadata-caching-algorithm"></a><span data-ttu-id="16dd5-473">algoritmus mezipaměti 3.4.1 metadata</span><span class="sxs-lookup"><span data-stu-id="16dd5-473">3.4.1 Metadata Caching algorithm</span></span>

1.  <span data-ttu-id="16dd5-474">Informace metadat pro model jsou uloženy v ItemCollection pro každou EntityConnection.</span><span class="sxs-lookup"><span data-stu-id="16dd5-474">Metadata information for a model is stored in an ItemCollection for each EntityConnection.</span></span>
    -   <span data-ttu-id="16dd5-475">Jako Poznámka na straně, existují různé objekty ItemCollection pro různé části modelu.</span><span class="sxs-lookup"><span data-stu-id="16dd5-475">As a side note, there are different ItemCollection objects for different parts of the model.</span></span> <span data-ttu-id="16dd5-476">Například StoreItemCollections obsahuje informace o databázovém modelu. ObjectItemCollection obsahuje informace o datovém modelu; EdmItemCollection obsahuje informace o koncepčním modelu.</span><span class="sxs-lookup"><span data-stu-id="16dd5-476">For example, StoreItemCollections contains the information about the database model; ObjectItemCollection contains information about the data model; EdmItemCollection contains information about the conceptual model.</span></span>

2.  <span data-ttu-id="16dd5-477">Pokud dvě připojení používají stejný připojovací řetězec, budou sdílet stejnou instanci ItemCollection.</span><span class="sxs-lookup"><span data-stu-id="16dd5-477">If two connections use the same connection string, they will share the same ItemCollection instance.</span></span>
3.  <span data-ttu-id="16dd5-478">Funkčně ekvivalentní, ale v textových i různých připojovacích řetězcích může dojít k různým mezipamětem metadat.</span><span class="sxs-lookup"><span data-stu-id="16dd5-478">Functionally equivalent but textually different connection strings may result in different metadata caches.</span></span> <span data-ttu-id="16dd5-479">Tokenizovat připojovací řetězce, takže jednoduše změnou pořadí tokenů by měla být sdílená metadata.</span><span class="sxs-lookup"><span data-stu-id="16dd5-479">We do tokenize connection strings, so simply changing the order of the tokens should result in shared metadata.</span></span> <span data-ttu-id="16dd5-480">Dva připojovací řetězce, které se zdají být funkčně stejné, ale nemusí být vyhodnoceny jako identické po tokenizace.</span><span class="sxs-lookup"><span data-stu-id="16dd5-480">But two connection strings that seem functionally the same may not be evaluated as identical after tokenization.</span></span>
4.  <span data-ttu-id="16dd5-481">ItemCollection se pravidelně kontroluje pro použití.</span><span class="sxs-lookup"><span data-stu-id="16dd5-481">The ItemCollection is periodically checked for use.</span></span> <span data-ttu-id="16dd5-482">Pokud se zjistí, že se pracovní prostor v poslední době nepoužil, bude pro vyčištění vyhodnocený jako při příštím čištění mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="16dd5-482">If it is determined that a workspace has not been accessed recently, it will be marked for cleanup on the next cache sweep.</span></span>
5.  <span data-ttu-id="16dd5-483">Pouze vytvořením EntityConnection dojde k vytvoření mezipaměti metadat (i když kolekce položek v ní nebudou inicializovány, dokud nebude připojení otevřeno).</span><span class="sxs-lookup"><span data-stu-id="16dd5-483">Merely creating an EntityConnection will cause a metadata cache to be created (though the item collections in it will not be initialized until the connection is opened).</span></span> <span data-ttu-id="16dd5-484">Tento pracovní prostor zůstane v paměti, dokud algoritmus ukládání do mezipaměti nezjistí, že se nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="16dd5-484">This workspace will remain in-memory until the caching algorithm determines it is not “in use”.</span></span>

<span data-ttu-id="16dd5-485">Zákaznický poradní tým zapsala blogový příspěvek popisuje uchovávající odkaz na objektu ItemCollection předejdete "vyřazení" při použití velkých modelů: \<http://blogs.msdn.com/b/appfabriccat/archive/2010/10/22/metadataworkspace-reference-in-wcf-services.aspx>.</span><span class="sxs-lookup"><span data-stu-id="16dd5-485">The Customer Advisory Team has written a blog post that describes holding a reference to an ItemCollection in order to avoid "deprecation" when using large models: \<http://blogs.msdn.com/b/appfabriccat/archive/2010/10/22/metadataworkspace-reference-in-wcf-services.aspx>.</span></span>

#### <a name="342-the-relationship-between-metadata-caching-and-query-plan-caching"></a><span data-ttu-id="16dd5-486">3.4.2 vztah mezi mezipamětí metadat a mezipamětí plánu dotazů</span><span class="sxs-lookup"><span data-stu-id="16dd5-486">3.4.2 The relationship between Metadata Caching and Query Plan Caching</span></span>

<span data-ttu-id="16dd5-487">Instance mezipaměti plánu dotazu žije v ItemCollection typů úložiště MetadataWorkspace.</span><span class="sxs-lookup"><span data-stu-id="16dd5-487">The query plan cache instance lives in the MetadataWorkspace's ItemCollection of store types.</span></span> <span data-ttu-id="16dd5-488">To znamená, že příkazy úložiště uložené v mezipaměti budou použity pro dotazy proti libovolnému kontextu vytvořenému pomocí daného objektu MetadataWorkspace.</span><span class="sxs-lookup"><span data-stu-id="16dd5-488">This means that cached store commands will be used for queries against any context instantiated using a given MetadataWorkspace.</span></span> <span data-ttu-id="16dd5-489">Také to znamená, že pokud máte dva řetězce připojení, které se mírně liší a neodpovídají po tokenizací, budete mít různé instance mezipaměti plánu dotazů.</span><span class="sxs-lookup"><span data-stu-id="16dd5-489">It also means that if you have two connections strings that are slightly different and don't match after tokenizing, you will have different query plan cache instances.</span></span>

### <a name="35-results-caching"></a><span data-ttu-id="16dd5-490">3,5 výsledků do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="16dd5-490">3.5 Results caching</span></span>

<span data-ttu-id="16dd5-491">Při ukládání výsledků do mezipaměti (označované také jako "ukládání do mezipaměti" druhé úrovně ") můžete uchovávat výsledky dotazů v místní mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="16dd5-491">With results caching (also known as "second-level caching"), you keep the results of queries in a local cache.</span></span> <span data-ttu-id="16dd5-492">Při vystavování dotazu nejprve vidíte, zda jsou výsledky k dispozici v místním prostředí před dotazem na úložiště.</span><span class="sxs-lookup"><span data-stu-id="16dd5-492">When issuing a query, you first see if the results are available locally before you query against the store.</span></span> <span data-ttu-id="16dd5-493">I když mezipaměť výsledků není přímo podporovaná Entity Framework, je možné přidat mezipaměť druhé úrovně pomocí poskytovatele pro vybalení.</span><span class="sxs-lookup"><span data-stu-id="16dd5-493">While results caching isn't directly supported by Entity Framework, it's possible to add a second level cache by using a wrapping provider.</span></span> <span data-ttu-id="16dd5-494">Příkladem zalamování poskytovatele s mezipamětí druhé úrovně je [Entity Framework Alachisoft mezipaměť druhé úrovně na základě NCache](https://www.alachisoft.com/ncache/entity-framework.html).</span><span class="sxs-lookup"><span data-stu-id="16dd5-494">An example wrapping provider with a second-level cache is Alachisoft's [Entity Framework Second Level Cache based on NCache](https://www.alachisoft.com/ncache/entity-framework.html).</span></span>

<span data-ttu-id="16dd5-495">Tato implementace ukládání do mezipaměti druhé úrovně je vložená funkce, která probíhá po vyhodnocení výrazu LINQ (a funcletized), a plán spuštění dotazu je vypočítán nebo načten z mezipaměti první úrovně.</span><span class="sxs-lookup"><span data-stu-id="16dd5-495">This implementation of second-level caching is an injected functionality that takes place after the LINQ expression has been evaluated (and funcletized) and the query execution plan is computed or retrieved from the first-level cache.</span></span> <span data-ttu-id="16dd5-496">Mezipaměť druhé úrovně pak uloží pouze nezpracované výsledky databáze, takže kanál materializace se následně provede i poté.</span><span class="sxs-lookup"><span data-stu-id="16dd5-496">The second-level cache will then store only the raw database results, so the materialization pipeline still executes afterwards.</span></span>

#### <a name="351-additional-references-for-results-caching-with-the-wrapping-provider"></a><span data-ttu-id="16dd5-497">3.5.1 další odkazy na výsledky ukládání do mezipaměti u poskytovatele zabalení</span><span class="sxs-lookup"><span data-stu-id="16dd5-497">3.5.1 Additional references for results caching with the wrapping provider</span></span>

-   <span data-ttu-id="16dd5-498">Julie Lerman napsala "ukládání do mezipaměti druhé úrovně v Entity Framework a v článku věnovaném službě Windows Azure" na webu MSDN, který obsahuje informace o tom, jak aktualizovat poskytovatele zabalení pro použití ukládání do mezipaměti Windows serveru AppFabric: [https://msdn.microsoft.com/magazine/hh394143.aspx](https://msdn.microsoft.com/magazine/hh394143.aspx)</span><span class="sxs-lookup"><span data-stu-id="16dd5-498">Julie Lerman has written a "Second-Level Caching in Entity Framework and Windows Azure" MSDN article that includes how to update the sample wrapping provider to use Windows Server AppFabric caching: [https://msdn.microsoft.com/magazine/hh394143.aspx](https://msdn.microsoft.com/magazine/hh394143.aspx)</span></span>
-   <span data-ttu-id="16dd5-499">Pokud pracujete s Entity Framework 5, na blogu týmu má příspěvek, který popisuje, jak pracovat s poskytovateli ukládání do mezipaměti pro Entity Framework 5: \<http://blogs.msdn.com/b/adonet/archive/2010/09/13/ef-caching-with-jarek-kowalski-s-provider.aspx>.</span><span class="sxs-lookup"><span data-stu-id="16dd5-499">If you are working with Entity Framework 5, the team blog has a post that describes how to get things running with the caching provider for Entity Framework 5: \<http://blogs.msdn.com/b/adonet/archive/2010/09/13/ef-caching-with-jarek-kowalski-s-provider.aspx>.</span></span> <span data-ttu-id="16dd5-500">Obsahuje taky šablonu T4, která vám usnadní automatizaci přidání ukládání do mezipaměti na druhé úrovni do vašeho projektu.</span><span class="sxs-lookup"><span data-stu-id="16dd5-500">It also includes a T4 template to help automate adding the 2nd-level caching to your project.</span></span>

## <a name="4-autocompiled-queries"></a><span data-ttu-id="16dd5-501">4 autokompilované dotazy</span><span class="sxs-lookup"><span data-stu-id="16dd5-501">4 Autocompiled Queries</span></span>

<span data-ttu-id="16dd5-502">Když je dotaz vydán pro databázi pomocí Entity Framework, musí projít řadou kroků před samotným vyhodnocování výsledků. jedním z těchto kroků je kompilace dotazů.</span><span class="sxs-lookup"><span data-stu-id="16dd5-502">When a query is issued against a database using Entity Framework, it must go through a series of steps before actually materializing the results; one such step is Query Compilation.</span></span> <span data-ttu-id="16dd5-503">U Entity SQL dotazů bylo známo, že mají dobrý výkon, když jsou automaticky ukládány do mezipaměti, takže druhý nebo třetí postup spuštění stejného dotazu může přeskočit plán kompilátoru a místo toho použít plán uložený v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="16dd5-503">Entity SQL queries were known to have good performance as they are automatically cached, so the second or third time you execute the same query it can skip the plan compiler and use the cached plan instead.</span></span>

<span data-ttu-id="16dd5-504">Entity Framework 5 zavádí také automatické ukládání do mezipaměti pro dotazy LINQ to Entities.</span><span class="sxs-lookup"><span data-stu-id="16dd5-504">Entity Framework 5 introduced automatic caching for LINQ to Entities queries as well.</span></span> <span data-ttu-id="16dd5-505">V minulých edicích Entity Framework vytváření CompiledQuery pro urychlení výkonu byl běžný postup, protože by to vedlo k tomu, že vaše LINQ to Entities dotazování bude možné ukládat do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="16dd5-505">In past editions of Entity Framework creating a CompiledQuery to speed your performance was a common practice, as this would make your LINQ to Entities query cacheable.</span></span> <span data-ttu-id="16dd5-506">Vzhledem k tomu, že ukládání do mezipaměti je nyní provedeno automaticky bez použití CompiledQuery, zavoláme tuto funkci u autokompilovaných dotazů.</span><span class="sxs-lookup"><span data-stu-id="16dd5-506">Since caching is now done automatically without the use of a CompiledQuery, we call this feature “autocompiled queries”.</span></span> <span data-ttu-id="16dd5-507">Další informace o mezipaměti plánu dotazů a jejím mechanismu najdete v tématu ukládání plánu dotazů do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="16dd5-507">For more information about the query plan cache and its mechanics, see Query Plan Caching.</span></span>

<span data-ttu-id="16dd5-508">Entity Framework detekuje, kdy je nutné znovu zkompilovat dotaz, a to tak, že je dotaz vyvolán, i když byl zkompilován před.</span><span class="sxs-lookup"><span data-stu-id="16dd5-508">Entity Framework detects when a query requires to be recompiled, and does so when the query is invoked even if it had been compiled before.</span></span> <span data-ttu-id="16dd5-509">Běžné podmínky, které způsobují překompilování dotazu, jsou:</span><span class="sxs-lookup"><span data-stu-id="16dd5-509">Common conditions that cause the query to be recompiled are:</span></span>

-   <span data-ttu-id="16dd5-510">Změna MergeOption přidruženého k vašemu dotazu.</span><span class="sxs-lookup"><span data-stu-id="16dd5-510">Changing the MergeOption associated to your query.</span></span> <span data-ttu-id="16dd5-511">Dotaz uložený v mezipaměti se nepoužije, místo toho se spustí kompilátor plánování a nově vytvořený plán se uloží do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="16dd5-511">The cached query will not be used, instead the plan compiler will run again and the newly created plan gets cached.</span></span>
-   <span data-ttu-id="16dd5-512">Změna hodnoty předané možnosti ContextOptions. UseCSharpNullComparisonBehavior.</span><span class="sxs-lookup"><span data-stu-id="16dd5-512">Changing the value of ContextOptions.UseCSharpNullComparisonBehavior.</span></span> <span data-ttu-id="16dd5-513">Získáte stejný efekt jako změny MergeOption.</span><span class="sxs-lookup"><span data-stu-id="16dd5-513">You get the same effect as changing the MergeOption.</span></span>

<span data-ttu-id="16dd5-514">Další podmínky můžou zabránit vašemu dotazu v používání mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="16dd5-514">Other conditions can prevent your query from using the cache.</span></span> <span data-ttu-id="16dd5-515">Mezi běžné příklady patří:</span><span class="sxs-lookup"><span data-stu-id="16dd5-515">Common examples are:</span></span>

-   <span data-ttu-id="16dd5-516">Pomocí IEnumerable @ no__t-0T @ no__t-1. Obsahuje @ no__t-2 @ no__t-3 (hodnota T).</span><span class="sxs-lookup"><span data-stu-id="16dd5-516">Using IEnumerable&lt;T&gt;.Contains&lt;&gt;(T value).</span></span>
-   <span data-ttu-id="16dd5-517">Použití funkcí, které vytváří dotazy s konstantami.</span><span class="sxs-lookup"><span data-stu-id="16dd5-517">Using functions that produce queries with constants.</span></span>
-   <span data-ttu-id="16dd5-518">Použití vlastností nemapovaného objektu.</span><span class="sxs-lookup"><span data-stu-id="16dd5-518">Using the properties of a non-mapped object.</span></span>
-   <span data-ttu-id="16dd5-519">Odkaz na dotaz na jiný dotaz, který vyžaduje překompilování.</span><span class="sxs-lookup"><span data-stu-id="16dd5-519">Linking your query to another query that requires to be recompiled.</span></span>

### <a name="41-using-ienumerablelttgtcontainslttgtt-value"></a><span data-ttu-id="16dd5-520">4,1 použití IEnumerable @ no__t-0T @ no__t-1. Obsahuje @ no__t-2T @ no__t-3 (hodnota T)</span><span class="sxs-lookup"><span data-stu-id="16dd5-520">4.1 Using IEnumerable&lt;T&gt;.Contains&lt;T&gt;(T value)</span></span>

<span data-ttu-id="16dd5-521">Entity Framework neukládá do mezipaměti dotazy, které vyvolávají IEnumerable @ no__t-0T @ no__t-1. Obsahuje hodnotu @ no__t-2T @ no__t-3 (T) proti kolekci v paměti, protože hodnoty kolekce jsou považovány za nestálé.</span><span class="sxs-lookup"><span data-stu-id="16dd5-521">Entity Framework does not cache queries that invoke IEnumerable&lt;T&gt;.Contains&lt;T&gt;(T value) against an in-memory collection, since the values of the collection are considered volatile.</span></span> <span data-ttu-id="16dd5-522">Následující příklad dotazu nebude uložen do mezipaměti, takže bude vždy zpracován kompilátorem plánu:</span><span class="sxs-lookup"><span data-stu-id="16dd5-522">The following example query will not be cached, so it will always be processed by the plan compiler:</span></span>

``` csharp
int[] ids = new int[10000];
...
using (var context = new MyContext())
{
    var query = context.MyEntities
                    .Where(entity => ids.Contains(entity.Id));

    var results = query.ToList();
    ...
}
```

<span data-ttu-id="16dd5-523">Všimněte si, že velikost rozhraní IEnumerable, proti kterému obsahuje, určuje, jak rychle nebo jak pomalu je dotaz zkompilován.</span><span class="sxs-lookup"><span data-stu-id="16dd5-523">Note that the size of the IEnumerable against which Contains is executed determines how fast or how slow your query is compiled.</span></span> <span data-ttu-id="16dd5-524">Při použití rozsáhlých kolekcí, jako je třeba ta, která je uvedená v předchozím příkladu, může dojít k výraznému snížení výkonu.</span><span class="sxs-lookup"><span data-stu-id="16dd5-524">Performance can suffer significantly when using large collections such as the one shown in the example above.</span></span>

<span data-ttu-id="16dd5-525">Entity Framework 6 obsahuje optimalizace způsobu IEnumerable @ no__t-0T @ no__t-1. Obsahuje @ no__t-2T @ no__t-3 (hodnota T) funguje při spuštění dotazů.</span><span class="sxs-lookup"><span data-stu-id="16dd5-525">Entity Framework 6 contains optimizations to the way IEnumerable&lt;T&gt;.Contains&lt;T&gt;(T value) works when queries are executed.</span></span> <span data-ttu-id="16dd5-526">Generovaný kód SQL je mnohem rychlejší, aby se vytvořil a čitelnější, ve většině případů se také rychleji spouští na serveru.</span><span class="sxs-lookup"><span data-stu-id="16dd5-526">The SQL code that is generated is much faster to produce and more readable, and in most cases it also executes faster in the server.</span></span>

### <a name="42-using-functions-that-produce-queries-with-constants"></a><span data-ttu-id="16dd5-527">4,2 použití funkcí, které vytváří dotazy s konstantami</span><span class="sxs-lookup"><span data-stu-id="16dd5-527">4.2 Using functions that produce queries with constants</span></span>

<span data-ttu-id="16dd5-528">Operátory Skip (), přijmout (), Contains () a DefautIfEmpty () LINQ nevytváří dotazy SQL s parametry, ale místo toho předávají hodnoty jako konstanty.</span><span class="sxs-lookup"><span data-stu-id="16dd5-528">The Skip(), Take(), Contains() and DefautIfEmpty() LINQ operators do not produce SQL queries with parameters but instead put the values passed to them as constants.</span></span> <span data-ttu-id="16dd5-529">Z tohoto důvodu dotazy, které by jinak mohly být identické, ukončí mezipaměť plánu dotazů v zásobníku EF i databázovém serveru a nevrátí se, pokud se stejné konstanty nepoužijí při následném spuštění dotazu.</span><span class="sxs-lookup"><span data-stu-id="16dd5-529">Because of this, queries that might otherwise be identical end up polluting the query plan cache, both on the EF stack and on the database server, and do not get reutilized unless the same constants are used in a subsequent query execution.</span></span> <span data-ttu-id="16dd5-530">Příklad:</span><span class="sxs-lookup"><span data-stu-id="16dd5-530">For example:</span></span>

``` csharp
var id = 10;
...
using (var context = new MyContext())
{
    var query = context.MyEntities.Select(entity => entity.Id).Contains(id);

    var results = query.ToList();
    ...
}
```

<span data-ttu-id="16dd5-531">V tomto příkladu se pokaždé, když se tento dotaz spustí s jinou hodnotou pro ID, bude dotaz zkompilován do nového plánu.</span><span class="sxs-lookup"><span data-stu-id="16dd5-531">In this example, each time this query is executed with a different value for id the query will be compiled into a new plan.</span></span>

<span data-ttu-id="16dd5-532">Konkrétně věnujte pozornost použití akce přeskočit a provést při stránkování.</span><span class="sxs-lookup"><span data-stu-id="16dd5-532">In particular pay attention to the use of Skip and Take when doing paging.</span></span> <span data-ttu-id="16dd5-533">V EF6 tyto metody mají přetížení lambda, které efektivně vede k opakovanému použití plánu dotazů v mezipaměti, protože EF může zachytit proměnné předané těmto metodám a jejich překlad na SQLparameters.</span><span class="sxs-lookup"><span data-stu-id="16dd5-533">In EF6 these methods have a lambda overload that effectively makes the cached query plan reusable because EF can capture variables passed to these methods and translate them to SQLparameters.</span></span> <span data-ttu-id="16dd5-534">To také pomáhá udržet čistič mezipaměti, protože jinak každý dotaz s jinou konstantou pro přeskočit a převzít by získal vlastní položku mezipaměti plánu dotazů.</span><span class="sxs-lookup"><span data-stu-id="16dd5-534">This also helps keep the cache cleaner since otherwise each query with a different constant for Skip and Take would get its own query plan cache entry.</span></span>

<span data-ttu-id="16dd5-535">Vezměte v úvahu následující kód, který je neoptimální, ale je určen pouze k tomu, aby exemplify tuto třídu dotazů:</span><span class="sxs-lookup"><span data-stu-id="16dd5-535">Consider the following code, which is suboptimal but is only meant to exemplify this class of queries:</span></span>

``` csharp
var customers = context.Customers.OrderBy(c => c.LastName);
for (var i = 0; i < count; ++i)
{
    var currentCustomer = customers.Skip(i).FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

<span data-ttu-id="16dd5-536">Rychlejší verze stejného kódu by zahrnovala volání Skip s výrazem lambda:</span><span class="sxs-lookup"><span data-stu-id="16dd5-536">A faster version of this same code would involve calling Skip with a lambda:</span></span>

``` csharp
var customers = context.Customers.OrderBy(c => c.LastName);
for (var i = 0; i < count; ++i)
{
    var currentCustomer = customers.Skip(() => i).FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

<span data-ttu-id="16dd5-537">Druhý fragment kódu může být spuštěn až o 11% rychleji, protože stejný plán dotazu se používá při každém spuštění dotazu, který šetří čas procesoru a zabraňuje znečišťující mezipaměti dotazů.</span><span class="sxs-lookup"><span data-stu-id="16dd5-537">The second snippet may run up to 11% faster because the same query plan is used every time the query is run, which saves CPU time and avoids polluting the query cache.</span></span> <span data-ttu-id="16dd5-538">Vzhledem k tomu, že parametr, který se má přeskočit, se nachází v uzávěrce, takže kód může vypadat stejně jako teď:</span><span class="sxs-lookup"><span data-stu-id="16dd5-538">Furthermore, because the parameter to Skip is in a closure the code might as well look like this now:</span></span>

``` csharp
var i = 0;
var skippyCustomers = context.Customers.OrderBy(c => c.LastName).Skip(() => i);
for (; i < count; ++i)
{
    var currentCustomer = skippyCustomers.FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

### <a name="43-using-the-properties-of-a-non-mapped-object"></a><span data-ttu-id="16dd5-539">4,3 použití vlastností nemapovaného objektu</span><span class="sxs-lookup"><span data-stu-id="16dd5-539">4.3 Using the properties of a non-mapped object</span></span>

<span data-ttu-id="16dd5-540">Když dotaz použije vlastnosti nemapovaného typu objektu jako parametr, dotaz nebude uložen do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="16dd5-540">When a query uses the properties of a non-mapped object type as a parameter then the query will not get cached.</span></span> <span data-ttu-id="16dd5-541">Příklad:</span><span class="sxs-lookup"><span data-stu-id="16dd5-541">For example:</span></span>

``` csharp
using (var context = new MyContext())
{
    var myObject = new NonMappedType();

    var query = from entity in context.MyEntities
                where entity.Name.StartsWith(myObject.MyProperty)
                select entity;

   var results = query.ToList();
    ...
}
```

<span data-ttu-id="16dd5-542">V tomto příkladu Předpokládejme, že třída NonMappedType není součástí modelu entity.</span><span class="sxs-lookup"><span data-stu-id="16dd5-542">In this example, assume that class NonMappedType is not part of the Entity model.</span></span> <span data-ttu-id="16dd5-543">Tento dotaz lze snadno změnit na nepoužitý Nemapovaný typ a místo toho použít místní proměnnou jako parametr pro dotaz:</span><span class="sxs-lookup"><span data-stu-id="16dd5-543">This query can easily be changed to not use a non-mapped type and instead use a local variable as the parameter to the query:</span></span>

``` csharp
using (var context = new MyContext())
{
    var myObject = new NonMappedType();
    var myValue = myObject.MyProperty;
    var query = from entity in context.MyEntities
                where entity.Name.StartsWith(myValue)
                select entity;

    var results = query.ToList();
    ...
}
```

<span data-ttu-id="16dd5-544">V takovém případě se dotaz bude moci načíst do mezipaměti a bude využívat mezipaměť plánu dotazů.</span><span class="sxs-lookup"><span data-stu-id="16dd5-544">In this case, the query will be able to get cached and will benefit from the query plan cache.</span></span>

### <a name="44-linking-to-queries-that-require-recompiling"></a><span data-ttu-id="16dd5-545">4,4 propojení s dotazy, které vyžadují opětovné kompilování</span><span class="sxs-lookup"><span data-stu-id="16dd5-545">4.4 Linking to queries that require recompiling</span></span>

<span data-ttu-id="16dd5-546">Pokud máte druhý dotaz, který se spoléhá na dotaz, který se má znovu zkompilovat, bude v tomto příkladu znovu zkompilován celý druhý dotaz, který se shoduje s výše uvedeným příkladem.</span><span class="sxs-lookup"><span data-stu-id="16dd5-546">Following the same example as above, if you have a second query that relies on a query that needs to be recompiled, your entire second query will also be recompiled.</span></span> <span data-ttu-id="16dd5-547">Tady je příklad, který ilustruje tento scénář:</span><span class="sxs-lookup"><span data-stu-id="16dd5-547">Here’s an example to illustrate this scenario:</span></span>

``` csharp
int[] ids = new int[10000];
...
using (var context = new MyContext())
{
    var firstQuery = from entity in context.MyEntities
                        where ids.Contains(entity.Id)
                        select entity;

    var secondQuery = from entity in context.MyEntities
                        where firstQuery.Any(otherEntity => otherEntity.Id == entity.Id)
                        select entity;

    var results = secondQuery.ToList();
    ...
}
```

<span data-ttu-id="16dd5-548">Příklad je obecný, ale ukazuje, jak propojení s firstQuery způsobuje, že secondQuery nebude možné získat do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="16dd5-548">The example is generic, but it illustrates how linking to firstQuery is causing secondQuery to be unable to get cached.</span></span> <span data-ttu-id="16dd5-549">Pokud firstQuery nebyl dotaz, který vyžaduje opětovné kompilování, pak secondQuery by byl uložen do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="16dd5-549">If firstQuery had not been a query that requires recompiling, then secondQuery would have been cached.</span></span>

## <a name="5-notracking-queries"></a><span data-ttu-id="16dd5-550">5 nesledovaných dotazů</span><span class="sxs-lookup"><span data-stu-id="16dd5-550">5 NoTracking Queries</span></span>

### <a name="51-disabling-change-tracking-to-reduce-state-management-overhead"></a><span data-ttu-id="16dd5-551">5,1 zakázání sledování změn pro snížení režie správy stavu</span><span class="sxs-lookup"><span data-stu-id="16dd5-551">5.1 Disabling change tracking to reduce state management overhead</span></span>

<span data-ttu-id="16dd5-552">Pokud jste ve scénáři jen pro čtení a chcete se vyhnout režii načítání objektů do objektu ObjectStateManager, můžete vydávat dotazy "bez sledování".</span><span class="sxs-lookup"><span data-stu-id="16dd5-552">If you are in a read-only scenario and want to avoid the overhead of loading the objects into the ObjectStateManager, you can issue "No Tracking" queries.</span></span><span data-ttu-id="16dd5-553">  Sledování změn je možné zakázat na úrovni dotazu.</span><span class="sxs-lookup"><span data-stu-id="16dd5-553">  Change tracking can be disabled at the query level.</span></span>

<span data-ttu-id="16dd5-554">Všimněte si, že když zakážete sledování změn, vypínáte mezipaměť objektů.</span><span class="sxs-lookup"><span data-stu-id="16dd5-554">Note though that by disabling change tracking you are effectively turning off the object cache.</span></span> <span data-ttu-id="16dd5-555">Při dotazování na entitu nemůžeme vynechávat z objektu ObjectStateManager načtením dříve vyhodnocených výsledků dotazu.</span><span class="sxs-lookup"><span data-stu-id="16dd5-555">When you query for an entity, we can't skip materialization by pulling the previously-materialized query results from the ObjectStateManager.</span></span> <span data-ttu-id="16dd5-556">Pokud se opakovaně dotazuje na stejné entity v rámci stejného kontextu, může se stát, že se vám v důsledku povolení sledování změn zobrazuje výhoda výkonu.</span><span class="sxs-lookup"><span data-stu-id="16dd5-556">If you are repeatedly querying for the same entities on the same context, you might actually see a performance benefit from enabling change tracking.</span></span>

<span data-ttu-id="16dd5-557">Při dotazování pomocí rozhraní ObjectContext, ObjectQuery a ObjectSet instance si po nastavení zapamatuje MergeOption a dotazy, které jsou vytvořeny, zdědí efektivní MergeOption nadřazeného dotazu.</span><span class="sxs-lookup"><span data-stu-id="16dd5-557">When querying using ObjectContext, ObjectQuery and ObjectSet instances will remember a MergeOption once it is set, and queries that are composed on them will inherit the effective MergeOption of the parent query.</span></span> <span data-ttu-id="16dd5-558">Při použití DbContext je sledování možné zakázat voláním modifikátoru AsNoTracking () na Negenerickými.</span><span class="sxs-lookup"><span data-stu-id="16dd5-558">When using DbContext, tracking can be disabled by calling the AsNoTracking() modifier on the DbSet.</span></span>

#### <a name="511-disabling-change-tracking-for-a-query-when-using-dbcontext"></a><span data-ttu-id="16dd5-559">5.1.1 zakazování sledování změn pro dotaz při použití DbContext</span><span class="sxs-lookup"><span data-stu-id="16dd5-559">5.1.1 Disabling change tracking for a query when using DbContext</span></span>

<span data-ttu-id="16dd5-560">Můžete přepnout režim dotazu na sledování pomocí zřetězení volání metody AsNoTracking () v dotazu.</span><span class="sxs-lookup"><span data-stu-id="16dd5-560">You can switch the mode of a query to NoTracking by chaining a call to the AsNoTracking() method in the query.</span></span> <span data-ttu-id="16dd5-561">Na rozdíl od ObjectQuery třídy Negenerickými a DbQuery v rozhraní API DbContext nemají pro MergeOption proměnlivou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="16dd5-561">Unlike ObjectQuery, the DbSet and DbQuery classes in the DbContext API don’t have a mutable property for the MergeOption.</span></span>

``` csharp
    var productsForCategory = from p in context.Products.AsNoTracking()
                                where p.Category.CategoryName == selectedCategory
                                select p;


```

#### <a name="512-disabling-change-tracking-at-the-query-level-using-objectcontext"></a><span data-ttu-id="16dd5-562">5.1.2 zakázání sledování změn na úrovni dotazu pomocí objektu ObjectContext</span><span class="sxs-lookup"><span data-stu-id="16dd5-562">5.1.2 Disabling change tracking at the query level using ObjectContext</span></span>

``` csharp
    var productsForCategory = from p in context.Products
                                where p.Category.CategoryName == selectedCategory
                                select p;

    ((ObjectQuery)productsForCategory).MergeOption = MergeOption.NoTracking;
```

#### <a name="513-disabling-change-tracking-for-an-entire-entity-set-using-objectcontext"></a><span data-ttu-id="16dd5-563">5.1.3 zakázání sledování změn pro celou sadu entit pomocí objektu ObjectContext</span><span class="sxs-lookup"><span data-stu-id="16dd5-563">5.1.3 Disabling change tracking for an entire entity set using ObjectContext</span></span>

``` csharp
    context.Products.MergeOption = MergeOption.NoTracking;

    var productsForCategory = from p in context.Products
                                where p.Category.CategoryName == selectedCategory
                                select p;
```

### <a name="52test-metrics-demonstrating-the-performance-benefit-of-notracking-queries"></a><span data-ttu-id="16dd5-564">5,2 testovacích metrik, které demonstrují výhody výkonu sledování dotazů</span><span class="sxs-lookup"><span data-stu-id="16dd5-564">5.2 Test Metrics demonstrating the performance benefit of NoTracking queries</span></span>

<span data-ttu-id="16dd5-565">V tomto testu se podíváme na náklady na naplnění objektu ObjectStateManager porovnáním sledování se sesledováním dotazů pro model Navision.</span><span class="sxs-lookup"><span data-stu-id="16dd5-565">In this test we look at the cost of filling the ObjectStateManager by comparing Tracking to NoTracking queries for the Navision model.</span></span> <span data-ttu-id="16dd5-566">Popis modelu Navision a typy dotazů, které se provedly, najdete v příloze.</span><span class="sxs-lookup"><span data-stu-id="16dd5-566">See the appendix for a description of the Navision model and the types of queries which were executed.</span></span> <span data-ttu-id="16dd5-567">V tomto testu projdeme seznam dotazů a jednou ho spustíme.</span><span class="sxs-lookup"><span data-stu-id="16dd5-567">In this test, we iterate through the list of queries and execute each one once.</span></span> <span data-ttu-id="16dd5-568">V případě, že se nesleduje dotazy a jednou s výchozí možností sloučení "AppendOnly", jsme spustili dvě variace testu.</span><span class="sxs-lookup"><span data-stu-id="16dd5-568">We ran two variations of the test, once with NoTracking queries and once with the default merge option of "AppendOnly".</span></span> <span data-ttu-id="16dd5-569">Každou variaci jsme spustili třikrát a vybereme střední hodnotu spuštění.</span><span class="sxs-lookup"><span data-stu-id="16dd5-569">We ran each variation 3 times and take the mean value of the runs.</span></span> <span data-ttu-id="16dd5-570">Mezi testy vymažeme mezipaměť dotazů na SQL Server a zmenšete databázi tempdb spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="16dd5-570">Between the tests we clear the query cache on the SQL Server and shrink the tempdb by running the following commands:</span></span>

1.  <span data-ttu-id="16dd5-571">DBCC DROPCLEANBUFFERS</span><span class="sxs-lookup"><span data-stu-id="16dd5-571">DBCC DROPCLEANBUFFERS</span></span>
2.  <span data-ttu-id="16dd5-572">DBCC FREEPROCCACHE</span><span class="sxs-lookup"><span data-stu-id="16dd5-572">DBCC FREEPROCCACHE</span></span>
3.  <span data-ttu-id="16dd5-573">DBCC SHRINKDATABASE (tempdb, 0)</span><span class="sxs-lookup"><span data-stu-id="16dd5-573">DBCC SHRINKDATABASE (tempdb, 0)</span></span>

<span data-ttu-id="16dd5-574">Výsledky testů, medián přes 3 běhy:</span><span class="sxs-lookup"><span data-stu-id="16dd5-574">Test Results, median over 3 runs:</span></span>

|                        | <span data-ttu-id="16dd5-575">BEZ SLEDOVÁNÍ – PRACOVNÍ SADA</span><span class="sxs-lookup"><span data-stu-id="16dd5-575">NO TRACKING – WORKING SET</span></span> | <span data-ttu-id="16dd5-576">BEZ SLEDOVÁNÍ – ČAS</span><span class="sxs-lookup"><span data-stu-id="16dd5-576">NO TRACKING – TIME</span></span> | <span data-ttu-id="16dd5-577">JENOM PŘIPOJIT – PRACOVNÍ SADA</span><span class="sxs-lookup"><span data-stu-id="16dd5-577">APPEND ONLY – WORKING SET</span></span> | <span data-ttu-id="16dd5-578">POUZE PŘIPOJENÍ – ČAS</span><span class="sxs-lookup"><span data-stu-id="16dd5-578">APPEND ONLY – TIME</span></span> |
|:-----------------------|:--------------------------|:-------------------|:--------------------------|:-------------------|
| <span data-ttu-id="16dd5-579">**Entity Framework 5**</span><span class="sxs-lookup"><span data-stu-id="16dd5-579">**Entity Framework 5**</span></span> | <span data-ttu-id="16dd5-580">460361728</span><span class="sxs-lookup"><span data-stu-id="16dd5-580">460361728</span></span>                 | <span data-ttu-id="16dd5-581">1163536 ms</span><span class="sxs-lookup"><span data-stu-id="16dd5-581">1163536 ms</span></span>         | <span data-ttu-id="16dd5-582">596545536</span><span class="sxs-lookup"><span data-stu-id="16dd5-582">596545536</span></span>                 | <span data-ttu-id="16dd5-583">1273042 ms</span><span class="sxs-lookup"><span data-stu-id="16dd5-583">1273042 ms</span></span>         |
| <span data-ttu-id="16dd5-584">**Entity Framework 6**</span><span class="sxs-lookup"><span data-stu-id="16dd5-584">**Entity Framework 6**</span></span> | <span data-ttu-id="16dd5-585">647127040</span><span class="sxs-lookup"><span data-stu-id="16dd5-585">647127040</span></span>                 | <span data-ttu-id="16dd5-586">190228 ms</span><span class="sxs-lookup"><span data-stu-id="16dd5-586">190228 ms</span></span>          | <span data-ttu-id="16dd5-587">832798720</span><span class="sxs-lookup"><span data-stu-id="16dd5-587">832798720</span></span>                 | <span data-ttu-id="16dd5-588">195521 ms</span><span class="sxs-lookup"><span data-stu-id="16dd5-588">195521 ms</span></span>          |

<span data-ttu-id="16dd5-589">Entity Framework 5 bude mít menší nároky na paměť na konci běhu než Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="16dd5-589">Entity Framework 5 will have a smaller memory footprint at the end of the run than Entity Framework 6 does.</span></span> <span data-ttu-id="16dd5-590">Další paměť spotřebovaná pomocí Entity Framework 6 je výsledkem dalších struktur paměti a kódu, který umožňuje nové funkce a lepší výkon.</span><span class="sxs-lookup"><span data-stu-id="16dd5-590">The additional memory consumed by Entity Framework 6 is the result of additional memory structures and code that enable new features and better performance.</span></span>

<span data-ttu-id="16dd5-591">K dispozici je také jasný rozdíl v paměti při použití objektu ObjectStateManager.</span><span class="sxs-lookup"><span data-stu-id="16dd5-591">There’s also a clear difference in memory footprint when using the ObjectStateManager.</span></span> <span data-ttu-id="16dd5-592">Entity Framework 5 zvýšila své nároky o 30% při udržení přehledu o všech entitách, které jsme z databáze vyhodnoceni.</span><span class="sxs-lookup"><span data-stu-id="16dd5-592">Entity Framework 5 increased its footprint by 30% when keeping track of all the entities we materialized from the database.</span></span> <span data-ttu-id="16dd5-593">Entity Framework 6 zvýšila své nároky o 28% při jejich provádění.</span><span class="sxs-lookup"><span data-stu-id="16dd5-593">Entity Framework 6 increased its footprint by 28% when doing so.</span></span>

<span data-ttu-id="16dd5-594">V průběhu času Entity Framework 6 Entity Framework 5 v tomto testu velkým okrajem.</span><span class="sxs-lookup"><span data-stu-id="16dd5-594">In terms of time, Entity Framework 6 outperforms Entity Framework 5 in this test by a large margin.</span></span> <span data-ttu-id="16dd5-595">Entity Framework 6 dokončil test přibližně o 16% času spotřebovaného Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="16dd5-595">Entity Framework 6 completed the test in roughly 16% of the time consumed by Entity Framework 5.</span></span> <span data-ttu-id="16dd5-596">Kromě toho Entity Framework 5 při použití objektu ObjectStateManager trvat déle než 9% času.</span><span class="sxs-lookup"><span data-stu-id="16dd5-596">Additionally, Entity Framework 5 takes 9% more time to complete when the ObjectStateManager is being used.</span></span> <span data-ttu-id="16dd5-597">V porovnání Entity Framework 6 při použití objektu ObjectStateManager používá 3% více času.</span><span class="sxs-lookup"><span data-stu-id="16dd5-597">In comparison, Entity Framework 6 is using 3% more time when using the ObjectStateManager.</span></span>

## <a name="6-query-execution-options"></a><span data-ttu-id="16dd5-598">6 možností provádění dotazů</span><span class="sxs-lookup"><span data-stu-id="16dd5-598">6 Query Execution Options</span></span>

<span data-ttu-id="16dd5-599">Entity Framework nabízí několik různých způsobů dotazování.</span><span class="sxs-lookup"><span data-stu-id="16dd5-599">Entity Framework offers several different ways to query.</span></span> <span data-ttu-id="16dd5-600">Podíváme se na následující možnosti, porovnejte jednotlivé odborníky a nevýhody a prověřte jejich charakteristiky výkonu:</span><span class="sxs-lookup"><span data-stu-id="16dd5-600">We'll take a look at the following options, compare the pros and cons of each, and examine their performance characteristics:</span></span>

-   <span data-ttu-id="16dd5-601">LINQ to Entities.</span><span class="sxs-lookup"><span data-stu-id="16dd5-601">LINQ to Entities.</span></span>
-   <span data-ttu-id="16dd5-602">Žádná LINQ to Entities sledování.</span><span class="sxs-lookup"><span data-stu-id="16dd5-602">No Tracking LINQ to Entities.</span></span>
-   <span data-ttu-id="16dd5-603">Entity SQL přes ObjectQuery.</span><span class="sxs-lookup"><span data-stu-id="16dd5-603">Entity SQL over an ObjectQuery.</span></span>
-   <span data-ttu-id="16dd5-604">Entity SQL přes EntityCommand.</span><span class="sxs-lookup"><span data-stu-id="16dd5-604">Entity SQL over an EntityCommand.</span></span>
-   <span data-ttu-id="16dd5-605">ExecuteStoreQuery.</span><span class="sxs-lookup"><span data-stu-id="16dd5-605">ExecuteStoreQuery.</span></span>
-   <span data-ttu-id="16dd5-606">SqlQuery.</span><span class="sxs-lookup"><span data-stu-id="16dd5-606">SqlQuery.</span></span>
-   <span data-ttu-id="16dd5-607">CompiledQuery.</span><span class="sxs-lookup"><span data-stu-id="16dd5-607">CompiledQuery.</span></span>

### <a name="61-linq-to-entities-queries"></a><span data-ttu-id="16dd5-608">6,1 LINQ to Entities dotazů</span><span class="sxs-lookup"><span data-stu-id="16dd5-608">6.1       LINQ to Entities queries</span></span>

``` csharp
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
```

<span data-ttu-id="16dd5-609">**IT**</span><span class="sxs-lookup"><span data-stu-id="16dd5-609">**Pros**</span></span>

-   <span data-ttu-id="16dd5-610">Vhodné pro CUD operace.</span><span class="sxs-lookup"><span data-stu-id="16dd5-610">Suitable for CUD operations.</span></span>
-   <span data-ttu-id="16dd5-611">Plně vyhodnocené objekty.</span><span class="sxs-lookup"><span data-stu-id="16dd5-611">Fully materialized objects.</span></span>
-   <span data-ttu-id="16dd5-612">Nejjednodušší pro zápis pomocí syntaxe integrované do programovacího jazyka.</span><span class="sxs-lookup"><span data-stu-id="16dd5-612">Simplest to write with syntax built into the programming language.</span></span>
-   <span data-ttu-id="16dd5-613">Dobrý výkon.</span><span class="sxs-lookup"><span data-stu-id="16dd5-613">Good performance.</span></span>

<span data-ttu-id="16dd5-614">**Cons**</span><span class="sxs-lookup"><span data-stu-id="16dd5-614">**Cons**</span></span>

-   <span data-ttu-id="16dd5-615">Některá technická omezení, jako například:</span><span class="sxs-lookup"><span data-stu-id="16dd5-615">Certain technical restrictions, such as:</span></span>
    -   <span data-ttu-id="16dd5-616">Vzorce používající DefaultIfEmpty pro dotazy VNĚJŠÍho spojení mají za následek složitější dotazy než jednoduché příkazy VNĚJŠÍho spojení v Entity SQL.</span><span class="sxs-lookup"><span data-stu-id="16dd5-616">Patterns using DefaultIfEmpty for OUTER JOIN queries result in more complex queries than simple OUTER JOIN statements in Entity SQL.</span></span>
    -   <span data-ttu-id="16dd5-617">Stále nemůžete používat jako u obecného porovnávání vzorů.</span><span class="sxs-lookup"><span data-stu-id="16dd5-617">You still can’t use LIKE with general pattern matching.</span></span>

### <a name="62-no-tracking-linq-to-entities-queries"></a><span data-ttu-id="16dd5-618">6,2 žádné LINQ to Entities dotazy sledování</span><span class="sxs-lookup"><span data-stu-id="16dd5-618">6.2       No Tracking LINQ to Entities queries</span></span>

<span data-ttu-id="16dd5-619">Když je kontext odvozený od objektu ObjectContext:</span><span class="sxs-lookup"><span data-stu-id="16dd5-619">When the context derives ObjectContext:</span></span>

``` csharp
context.Products.MergeOption = MergeOption.NoTracking;
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
```

<span data-ttu-id="16dd5-620">Když je kontext odvozený z DbContext:</span><span class="sxs-lookup"><span data-stu-id="16dd5-620">When the context derives DbContext:</span></span>

``` csharp
var q = context.Products.AsNoTracking()
                        .Where(p => p.Category.CategoryName == "Beverages");
```

<span data-ttu-id="16dd5-621">**IT**</span><span class="sxs-lookup"><span data-stu-id="16dd5-621">**Pros**</span></span>

-   <span data-ttu-id="16dd5-622">Vylepšený výkon v pravidelných dotazech LINQ.</span><span class="sxs-lookup"><span data-stu-id="16dd5-622">Improved performance over regular LINQ queries.</span></span>
-   <span data-ttu-id="16dd5-623">Plně vyhodnocené objekty.</span><span class="sxs-lookup"><span data-stu-id="16dd5-623">Fully materialized objects.</span></span>
-   <span data-ttu-id="16dd5-624">Nejjednodušší pro zápis pomocí syntaxe integrované do programovacího jazyka.</span><span class="sxs-lookup"><span data-stu-id="16dd5-624">Simplest to write with syntax built into the programming language.</span></span>

<span data-ttu-id="16dd5-625">**Cons**</span><span class="sxs-lookup"><span data-stu-id="16dd5-625">**Cons**</span></span>

-   <span data-ttu-id="16dd5-626">Není vhodné pro operace CUD.</span><span class="sxs-lookup"><span data-stu-id="16dd5-626">Not suitable for CUD operations.</span></span>
-   <span data-ttu-id="16dd5-627">Některá technická omezení, jako například:</span><span class="sxs-lookup"><span data-stu-id="16dd5-627">Certain technical restrictions, such as:</span></span>
    -   <span data-ttu-id="16dd5-628">Vzorce používající DefaultIfEmpty pro dotazy VNĚJŠÍho spojení mají za následek složitější dotazy než jednoduché příkazy VNĚJŠÍho spojení v Entity SQL.</span><span class="sxs-lookup"><span data-stu-id="16dd5-628">Patterns using DefaultIfEmpty for OUTER JOIN queries result in more complex queries than simple OUTER JOIN statements in Entity SQL.</span></span>
    -   <span data-ttu-id="16dd5-629">Stále nemůžete používat jako u obecného porovnávání vzorů.</span><span class="sxs-lookup"><span data-stu-id="16dd5-629">You still can’t use LIKE with general pattern matching.</span></span>

<span data-ttu-id="16dd5-630">Všimněte si, že dotazy, které jsou skalární vlastnosti projektu, nejsou sledovány ani v případě, že není zadáno žádné sledování.</span><span class="sxs-lookup"><span data-stu-id="16dd5-630">Note that queries that project scalar properties are not tracked even if the NoTracking is not specified.</span></span> <span data-ttu-id="16dd5-631">Příklad:</span><span class="sxs-lookup"><span data-stu-id="16dd5-631">For example:</span></span>

``` csharp
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages").Select(p => new { p.ProductName });
```

<span data-ttu-id="16dd5-632">Tento konkrétní dotaz explicitně neurčuje, že není sledován, ale vzhledem k tomu, že není vyhodnocování typ známý pro správce stavu objektu, materializovaná výsledek není sledován.</span><span class="sxs-lookup"><span data-stu-id="16dd5-632">This particular query doesn’t explicitly specify being NoTracking, but since it’s not materializing a type that’s known to the object state manager then the materialized result is not tracked.</span></span>

### <a name="63-entity-sql-over-an-objectquery"></a><span data-ttu-id="16dd5-633">6,3 Entity SQL ObjectQuery</span><span class="sxs-lookup"><span data-stu-id="16dd5-633">6.3       Entity SQL over an ObjectQuery</span></span>

``` csharp
ObjectQuery<Product> products = context.Products.Where("it.Category.CategoryName = 'Beverages'");
```

<span data-ttu-id="16dd5-634">**IT**</span><span class="sxs-lookup"><span data-stu-id="16dd5-634">**Pros**</span></span>

-   <span data-ttu-id="16dd5-635">Vhodné pro CUD operace.</span><span class="sxs-lookup"><span data-stu-id="16dd5-635">Suitable for CUD operations.</span></span>
-   <span data-ttu-id="16dd5-636">Plně vyhodnocené objekty.</span><span class="sxs-lookup"><span data-stu-id="16dd5-636">Fully materialized objects.</span></span>
-   <span data-ttu-id="16dd5-637">Podporuje ukládání plánu dotazů do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="16dd5-637">Supports query plan caching.</span></span>

<span data-ttu-id="16dd5-638">**Cons**</span><span class="sxs-lookup"><span data-stu-id="16dd5-638">**Cons**</span></span>

-   <span data-ttu-id="16dd5-639">Zahrnuje textové řetězce dotazů, které jsou lépe náchylné k chybě uživatele, než je konstrukce dotazu integrována do jazyka.</span><span class="sxs-lookup"><span data-stu-id="16dd5-639">Involves textual query strings which are more prone to user error than query constructs built into the language.</span></span>

### <a name="64-entity-sql-over-an-entity-command"></a><span data-ttu-id="16dd5-640">6,4 Entity SQL nad příkazem entity</span><span class="sxs-lookup"><span data-stu-id="16dd5-640">6.4       Entity SQL over an Entity Command</span></span>

``` csharp
EntityCommand cmd = eConn.CreateCommand();
cmd.CommandText = "Select p From NorthwindEntities.Products As p Where p.Category.CategoryName = 'Beverages'";

using (EntityDataReader reader = cmd.ExecuteReader(CommandBehavior.SequentialAccess))
{
    while (reader.Read())
    {
        // manually 'materialize' the product
    }
}
```

<span data-ttu-id="16dd5-641">**IT**</span><span class="sxs-lookup"><span data-stu-id="16dd5-641">**Pros**</span></span>

-   <span data-ttu-id="16dd5-642">Podporuje ukládání plánů dotazů do mezipaměti v rozhraní .NET 4,0 (všechny ostatní typy dotazů v rozhraní .NET 4,5 podporují ukládání plánů do mezipaměti).</span><span class="sxs-lookup"><span data-stu-id="16dd5-642">Supports query plan caching in .NET 4.0 (plan caching is supported by all other query types in .NET 4.5).</span></span>

<span data-ttu-id="16dd5-643">**Cons**</span><span class="sxs-lookup"><span data-stu-id="16dd5-643">**Cons**</span></span>

-   <span data-ttu-id="16dd5-644">Zahrnuje textové řetězce dotazů, které jsou lépe náchylné k chybě uživatele, než je konstrukce dotazu integrována do jazyka.</span><span class="sxs-lookup"><span data-stu-id="16dd5-644">Involves textual query strings which are more prone to user error than query constructs built into the language.</span></span>
-   <span data-ttu-id="16dd5-645">Není vhodné pro operace CUD.</span><span class="sxs-lookup"><span data-stu-id="16dd5-645">Not suitable for CUD operations.</span></span>
-   <span data-ttu-id="16dd5-646">Výsledky nejsou automaticky vyhodnoceny a musí být načteny z čtecího modulu dat.</span><span class="sxs-lookup"><span data-stu-id="16dd5-646">Results are not automatically materialized, and must be read from the data reader.</span></span>

### <a name="65-sqlquery-and-executestorequery"></a><span data-ttu-id="16dd5-647">6,5 SqlQuery a ExecuteStoreQuery</span><span class="sxs-lookup"><span data-stu-id="16dd5-647">6.5       SqlQuery and ExecuteStoreQuery</span></span>

<span data-ttu-id="16dd5-648">SqlQuery v databázi:</span><span class="sxs-lookup"><span data-stu-id="16dd5-648">SqlQuery on Database:</span></span>

``` csharp
// use this to obtain entities and not track them
var q1 = context.Database.SqlQuery<Product>("select * from products");
```

<span data-ttu-id="16dd5-649">SqlQuery na Negenerickými:</span><span class="sxs-lookup"><span data-stu-id="16dd5-649">SqlQuery on DbSet:</span></span>

``` csharp
// use this to obtain entities and have them tracked
var q2 = context.Products.SqlQuery("select * from products");
```

<span data-ttu-id="16dd5-650">ExecyteStoreQuery:</span><span class="sxs-lookup"><span data-stu-id="16dd5-650">ExecyteStoreQuery:</span></span>

``` csharp
var beverages = context.ExecuteStoreQuery<Product>(
@"     SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued, P.DiscontinuedDate
       FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
       WHERE        (C.CategoryName = 'Beverages')"
);
```

<span data-ttu-id="16dd5-651">**IT**</span><span class="sxs-lookup"><span data-stu-id="16dd5-651">**Pros**</span></span>

-   <span data-ttu-id="16dd5-652">Všeobecně nejrychlejší výkon, protože kompilátor plánu je obejít.</span><span class="sxs-lookup"><span data-stu-id="16dd5-652">Generally fastest performance since plan compiler is bypassed.</span></span>
-   <span data-ttu-id="16dd5-653">Plně vyhodnocené objekty.</span><span class="sxs-lookup"><span data-stu-id="16dd5-653">Fully materialized objects.</span></span>
-   <span data-ttu-id="16dd5-654">Vhodný pro CUD operace při použití z Negenerickými.</span><span class="sxs-lookup"><span data-stu-id="16dd5-654">Suitable for CUD operations when used from the DbSet.</span></span>

<span data-ttu-id="16dd5-655">**Cons**</span><span class="sxs-lookup"><span data-stu-id="16dd5-655">**Cons**</span></span>

-   <span data-ttu-id="16dd5-656">Dotaz je text a náchylný k chybám.</span><span class="sxs-lookup"><span data-stu-id="16dd5-656">Query is textual and error prone.</span></span>
-   <span data-ttu-id="16dd5-657">Dotaz je vázaný na konkrétní back-end pomocí sémantiky úložiště namísto koncepční sémantiky.</span><span class="sxs-lookup"><span data-stu-id="16dd5-657">Query is tied to a specific backend by using store semantics instead of conceptual semantics.</span></span>
-   <span data-ttu-id="16dd5-658">Pokud je k dispozici dědičnost, dotaz handcrafted musí mít pro podmínky mapování pro požadovaný typ účet.</span><span class="sxs-lookup"><span data-stu-id="16dd5-658">When inheritance is present, handcrafted query needs to account for mapping conditions for the type requested.</span></span>

### <a name="66-compiledquery"></a><span data-ttu-id="16dd5-659">6,6 CompiledQuery</span><span class="sxs-lookup"><span data-stu-id="16dd5-659">6.6       CompiledQuery</span></span>

``` csharp
private static readonly Func<NorthwindEntities, string, IQueryable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
    (NorthwindEntities context, string categoryName) =>
        context.Products.Where(p => p.Category.CategoryName == categoryName)
        );
…
var q = context.InvokeProductsForCategoryCQ("Beverages");
```

<span data-ttu-id="16dd5-660">**IT**</span><span class="sxs-lookup"><span data-stu-id="16dd5-660">**Pros**</span></span>

-   <span data-ttu-id="16dd5-661">Poskytuje až 7% zlepšení výkonu oproti pravidelným dotazům LINQ.</span><span class="sxs-lookup"><span data-stu-id="16dd5-661">Provides up to a 7% performance improvement over regular LINQ queries.</span></span>
-   <span data-ttu-id="16dd5-662">Plně vyhodnocené objekty.</span><span class="sxs-lookup"><span data-stu-id="16dd5-662">Fully materialized objects.</span></span>
-   <span data-ttu-id="16dd5-663">Vhodné pro CUD operace.</span><span class="sxs-lookup"><span data-stu-id="16dd5-663">Suitable for CUD operations.</span></span>

<span data-ttu-id="16dd5-664">**Cons**</span><span class="sxs-lookup"><span data-stu-id="16dd5-664">**Cons**</span></span>

-   <span data-ttu-id="16dd5-665">Zvýšení složitosti a režie programování.</span><span class="sxs-lookup"><span data-stu-id="16dd5-665">Increased complexity and programming overhead.</span></span>
-   <span data-ttu-id="16dd5-666">Zvýšení výkonu se při vytváření nad kompilovaným dotazem ztratí.</span><span class="sxs-lookup"><span data-stu-id="16dd5-666">The performance improvement is lost when composing on top of a compiled query.</span></span>
-   <span data-ttu-id="16dd5-667">Některé dotazy LINQ nelze zapsat jako CompiledQuery – například projekce anonymních typů.</span><span class="sxs-lookup"><span data-stu-id="16dd5-667">Some LINQ queries can't be written as a CompiledQuery - for example, projections of anonymous types.</span></span>

### <a name="67-performance-comparison-of-different-query-options"></a><span data-ttu-id="16dd5-668">6,7 porovnání výkonu různých možností dotazu</span><span class="sxs-lookup"><span data-stu-id="16dd5-668">6.7       Performance Comparison of different query options</span></span>

<span data-ttu-id="16dd5-669">Jednoduché mikrosrovnávací testy, u kterých nebyl při vytváření kontextu kladen test.</span><span class="sxs-lookup"><span data-stu-id="16dd5-669">Simple microbenchmarks where the context creation was not timed were put to the test.</span></span> <span data-ttu-id="16dd5-670">V kontrolovaném prostředí jsme naměřeni dotazování na 5000 časů pro sadu entit, které nejsou v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="16dd5-670">We measured querying 5000 times for a set of non-cached entities in a controlled environment.</span></span> <span data-ttu-id="16dd5-671">Tato čísla se budou považovat za upozornění: nereflektují se na skutečná čísla vytvořená aplikací, ale místo toho jsou velmi přesné měření toho, jak velká část rozdílu při dotazování je porovnávána. jablka na jablka s výjimkou nákladů na vytvoření nového kontextu.</span><span class="sxs-lookup"><span data-stu-id="16dd5-671">These numbers are to be taken with a warning: they do not reflect actual numbers produced by an application, but instead they are a very accurate measurement of how much of a performance difference there is when different querying options are compared apples-to-apples, excluding the cost of creating a new context.</span></span>

| <span data-ttu-id="16dd5-672">EF</span><span class="sxs-lookup"><span data-stu-id="16dd5-672">EF</span></span>  | <span data-ttu-id="16dd5-673">Test</span><span class="sxs-lookup"><span data-stu-id="16dd5-673">Test</span></span>                                 | <span data-ttu-id="16dd5-674">Čas (MS)</span><span class="sxs-lookup"><span data-stu-id="16dd5-674">Time (ms)</span></span> | <span data-ttu-id="16dd5-675">Memory (Paměť)</span><span class="sxs-lookup"><span data-stu-id="16dd5-675">Memory</span></span>   |
|:----|:-------------------------------------|:----------|:---------|
| <span data-ttu-id="16dd5-676">EF5</span><span class="sxs-lookup"><span data-stu-id="16dd5-676">EF5</span></span> | <span data-ttu-id="16dd5-677">ObjectContext ESQL</span><span class="sxs-lookup"><span data-stu-id="16dd5-677">ObjectContext ESQL</span></span>                   | <span data-ttu-id="16dd5-678">2414</span><span class="sxs-lookup"><span data-stu-id="16dd5-678">2414</span></span>      | <span data-ttu-id="16dd5-679">38801408</span><span class="sxs-lookup"><span data-stu-id="16dd5-679">38801408</span></span> |
| <span data-ttu-id="16dd5-680">EF5</span><span class="sxs-lookup"><span data-stu-id="16dd5-680">EF5</span></span> | <span data-ttu-id="16dd5-681">Dotaz LINQ pro ObjectContext</span><span class="sxs-lookup"><span data-stu-id="16dd5-681">ObjectContext Linq Query</span></span>             | <span data-ttu-id="16dd5-682">2692</span><span class="sxs-lookup"><span data-stu-id="16dd5-682">2692</span></span>      | <span data-ttu-id="16dd5-683">38277120</span><span class="sxs-lookup"><span data-stu-id="16dd5-683">38277120</span></span> |
| <span data-ttu-id="16dd5-684">EF5</span><span class="sxs-lookup"><span data-stu-id="16dd5-684">EF5</span></span> | <span data-ttu-id="16dd5-685">DbContext dotaz LINQ bez sledování</span><span class="sxs-lookup"><span data-stu-id="16dd5-685">DbContext Linq Query No Tracking</span></span>     | <span data-ttu-id="16dd5-686">2818</span><span class="sxs-lookup"><span data-stu-id="16dd5-686">2818</span></span>      | <span data-ttu-id="16dd5-687">41840640</span><span class="sxs-lookup"><span data-stu-id="16dd5-687">41840640</span></span> |
| <span data-ttu-id="16dd5-688">EF5</span><span class="sxs-lookup"><span data-stu-id="16dd5-688">EF5</span></span> | <span data-ttu-id="16dd5-689">Dotaz LINQ DbContext</span><span class="sxs-lookup"><span data-stu-id="16dd5-689">DbContext Linq Query</span></span>                 | <span data-ttu-id="16dd5-690">2930</span><span class="sxs-lookup"><span data-stu-id="16dd5-690">2930</span></span>      | <span data-ttu-id="16dd5-691">41771008</span><span class="sxs-lookup"><span data-stu-id="16dd5-691">41771008</span></span> |
| <span data-ttu-id="16dd5-692">EF5</span><span class="sxs-lookup"><span data-stu-id="16dd5-692">EF5</span></span> | <span data-ttu-id="16dd5-693">Nesledovaný dotaz LINQ pro ObjectContext</span><span class="sxs-lookup"><span data-stu-id="16dd5-693">ObjectContext Linq Query No Tracking</span></span> | <span data-ttu-id="16dd5-694">3013</span><span class="sxs-lookup"><span data-stu-id="16dd5-694">3013</span></span>      | <span data-ttu-id="16dd5-695">38412288</span><span class="sxs-lookup"><span data-stu-id="16dd5-695">38412288</span></span> |
|     |                                      |           |          |
| <span data-ttu-id="16dd5-696">EF6</span><span class="sxs-lookup"><span data-stu-id="16dd5-696">EF6</span></span> | <span data-ttu-id="16dd5-697">ObjectContext ESQL</span><span class="sxs-lookup"><span data-stu-id="16dd5-697">ObjectContext ESQL</span></span>                   | <span data-ttu-id="16dd5-698">2059</span><span class="sxs-lookup"><span data-stu-id="16dd5-698">2059</span></span>      | <span data-ttu-id="16dd5-699">46039040</span><span class="sxs-lookup"><span data-stu-id="16dd5-699">46039040</span></span> |
| <span data-ttu-id="16dd5-700">EF6</span><span class="sxs-lookup"><span data-stu-id="16dd5-700">EF6</span></span> | <span data-ttu-id="16dd5-701">Dotaz LINQ pro ObjectContext</span><span class="sxs-lookup"><span data-stu-id="16dd5-701">ObjectContext Linq Query</span></span>             | <span data-ttu-id="16dd5-702">3074</span><span class="sxs-lookup"><span data-stu-id="16dd5-702">3074</span></span>      | <span data-ttu-id="16dd5-703">45248512</span><span class="sxs-lookup"><span data-stu-id="16dd5-703">45248512</span></span> |
| <span data-ttu-id="16dd5-704">EF6</span><span class="sxs-lookup"><span data-stu-id="16dd5-704">EF6</span></span> | <span data-ttu-id="16dd5-705">DbContext dotaz LINQ bez sledování</span><span class="sxs-lookup"><span data-stu-id="16dd5-705">DbContext Linq Query No Tracking</span></span>     | <span data-ttu-id="16dd5-706">3125</span><span class="sxs-lookup"><span data-stu-id="16dd5-706">3125</span></span>      | <span data-ttu-id="16dd5-707">47575040</span><span class="sxs-lookup"><span data-stu-id="16dd5-707">47575040</span></span> |
| <span data-ttu-id="16dd5-708">EF6</span><span class="sxs-lookup"><span data-stu-id="16dd5-708">EF6</span></span> | <span data-ttu-id="16dd5-709">Dotaz LINQ DbContext</span><span class="sxs-lookup"><span data-stu-id="16dd5-709">DbContext Linq Query</span></span>                 | <span data-ttu-id="16dd5-710">3420</span><span class="sxs-lookup"><span data-stu-id="16dd5-710">3420</span></span>      | <span data-ttu-id="16dd5-711">47652864</span><span class="sxs-lookup"><span data-stu-id="16dd5-711">47652864</span></span> |
| <span data-ttu-id="16dd5-712">EF6</span><span class="sxs-lookup"><span data-stu-id="16dd5-712">EF6</span></span> | <span data-ttu-id="16dd5-713">Nesledovaný dotaz LINQ pro ObjectContext</span><span class="sxs-lookup"><span data-stu-id="16dd5-713">ObjectContext Linq Query No Tracking</span></span> | <span data-ttu-id="16dd5-714">3593</span><span class="sxs-lookup"><span data-stu-id="16dd5-714">3593</span></span>      | <span data-ttu-id="16dd5-715">45260800</span><span class="sxs-lookup"><span data-stu-id="16dd5-715">45260800</span></span> |

![EF5 Micro srovnávací testy, 5000 teplé iterace](~/ef6/media/ef5micro5000warm.png)

![EF6 Micro srovnávací testy, 5000 teplé iterace](~/ef6/media/ef6micro5000warm.png)

<span data-ttu-id="16dd5-718">Mikrosrovnávací testy jsou velmi citlivé na malé změny v kódu.</span><span class="sxs-lookup"><span data-stu-id="16dd5-718">Microbenchmarks are very sensitive to small changes in the code.</span></span> <span data-ttu-id="16dd5-719">V tomto případě je rozdíl mezi náklady na Entity Framework 5 a Entity Framework 6 způsoben přidáním vylepšení [zachycení](~/ef6/fundamentals/logging-and-interception.md) a [transakcí](~/ef6/saving/transactions.md).</span><span class="sxs-lookup"><span data-stu-id="16dd5-719">In this case, the difference between the costs of Entity Framework 5 and Entity Framework 6 are due to the addition of [interception](~/ef6/fundamentals/logging-and-interception.md) and [transactional improvements](~/ef6/saving/transactions.md).</span></span> <span data-ttu-id="16dd5-720">Tato mikrosrovnávacích číslech však představují doplněnou vizi do velmi malé fragmenty toho, co Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="16dd5-720">These microbenchmarks numbers, however, are an amplified vision into a very small fragment of what Entity Framework does.</span></span> <span data-ttu-id="16dd5-721">V reálných scénářích teplé dotazů by se při upgradu z Entity Framework 5 na Entity Framework 6 neměla zobrazovat regrese výkonu.</span><span class="sxs-lookup"><span data-stu-id="16dd5-721">Real-world scenarios of warm queries should not see a performance regression when upgrading from Entity Framework 5 to Entity Framework 6.</span></span>

<span data-ttu-id="16dd5-722">Pro porovnání reálného výkonu různých možností dotazu jsme vytvořili 5 samostatných variant testů, kde používáme jinou možnost dotazování pro výběr všech produktů, jejichž název kategorie je "nápoje".</span><span class="sxs-lookup"><span data-stu-id="16dd5-722">To compare the real-world performance of the different query options, we created 5 separate test variations where we use a different query option to select all products whose category name is "Beverages".</span></span> <span data-ttu-id="16dd5-723">Každá iterace zahrnuje náklady na vytvoření kontextu a náklady na vyhodnocováníy všech vrácených entit.</span><span class="sxs-lookup"><span data-stu-id="16dd5-723">Each iteration includes the cost of creating the context, and the cost of materializing all returned entities.</span></span> <span data-ttu-id="16dd5-724">než se vybere součet 1000 časovanéch iterací, neuplynulý čas spuštění 10 iterací.</span><span class="sxs-lookup"><span data-stu-id="16dd5-724">10 iterations are run untimed before taking the sum of 1000 timed iterations.</span></span> <span data-ttu-id="16dd5-725">Zobrazené výsledky jsou medián pořízený z 5 spuštění každého testu.</span><span class="sxs-lookup"><span data-stu-id="16dd5-725">The results shown are the median run taken from 5 runs of each test.</span></span> <span data-ttu-id="16dd5-726">Další informace naleznete v příloze B, která obsahuje kód pro test.</span><span class="sxs-lookup"><span data-stu-id="16dd5-726">For more information, see Appendix B which includes the code for the test.</span></span>

| <span data-ttu-id="16dd5-727">EF</span><span class="sxs-lookup"><span data-stu-id="16dd5-727">EF</span></span>  | <span data-ttu-id="16dd5-728">Test</span><span class="sxs-lookup"><span data-stu-id="16dd5-728">Test</span></span>                                        | <span data-ttu-id="16dd5-729">Čas (MS)</span><span class="sxs-lookup"><span data-stu-id="16dd5-729">Time (ms)</span></span> | <span data-ttu-id="16dd5-730">Memory (Paměť)</span><span class="sxs-lookup"><span data-stu-id="16dd5-730">Memory</span></span>   |
|:----|:--------------------------------------------|:----------|:---------|
| <span data-ttu-id="16dd5-731">EF5</span><span class="sxs-lookup"><span data-stu-id="16dd5-731">EF5</span></span> | <span data-ttu-id="16dd5-732">ObjectContext – příkaz entity</span><span class="sxs-lookup"><span data-stu-id="16dd5-732">ObjectContext Entity Command</span></span>                | <span data-ttu-id="16dd5-733">621</span><span class="sxs-lookup"><span data-stu-id="16dd5-733">621</span></span>       | <span data-ttu-id="16dd5-734">39350272</span><span class="sxs-lookup"><span data-stu-id="16dd5-734">39350272</span></span> |
| <span data-ttu-id="16dd5-735">EF5</span><span class="sxs-lookup"><span data-stu-id="16dd5-735">EF5</span></span> | <span data-ttu-id="16dd5-736">DbContext dotaz SQL v databázi</span><span class="sxs-lookup"><span data-stu-id="16dd5-736">DbContext Sql Query on Database</span></span>             | <span data-ttu-id="16dd5-737">825</span><span class="sxs-lookup"><span data-stu-id="16dd5-737">825</span></span>       | <span data-ttu-id="16dd5-738">37519360</span><span class="sxs-lookup"><span data-stu-id="16dd5-738">37519360</span></span> |
| <span data-ttu-id="16dd5-739">EF5</span><span class="sxs-lookup"><span data-stu-id="16dd5-739">EF5</span></span> | <span data-ttu-id="16dd5-740">Dotaz na úložiště ObjectContext</span><span class="sxs-lookup"><span data-stu-id="16dd5-740">ObjectContext Store Query</span></span>                   | <span data-ttu-id="16dd5-741">878</span><span class="sxs-lookup"><span data-stu-id="16dd5-741">878</span></span>       | <span data-ttu-id="16dd5-742">39460864</span><span class="sxs-lookup"><span data-stu-id="16dd5-742">39460864</span></span> |
| <span data-ttu-id="16dd5-743">EF5</span><span class="sxs-lookup"><span data-stu-id="16dd5-743">EF5</span></span> | <span data-ttu-id="16dd5-744">Nesledovaný dotaz LINQ pro ObjectContext</span><span class="sxs-lookup"><span data-stu-id="16dd5-744">ObjectContext Linq Query No Tracking</span></span>        | <span data-ttu-id="16dd5-745">969</span><span class="sxs-lookup"><span data-stu-id="16dd5-745">969</span></span>       | <span data-ttu-id="16dd5-746">38293504</span><span class="sxs-lookup"><span data-stu-id="16dd5-746">38293504</span></span> |
| <span data-ttu-id="16dd5-747">EF5</span><span class="sxs-lookup"><span data-stu-id="16dd5-747">EF5</span></span> | <span data-ttu-id="16dd5-748">Entita objektu ObjectContext SQL s použitím dotazu na objekt</span><span class="sxs-lookup"><span data-stu-id="16dd5-748">ObjectContext Entity Sql using Object Query</span></span> | <span data-ttu-id="16dd5-749">1089</span><span class="sxs-lookup"><span data-stu-id="16dd5-749">1089</span></span>      | <span data-ttu-id="16dd5-750">38981632</span><span class="sxs-lookup"><span data-stu-id="16dd5-750">38981632</span></span> |
| <span data-ttu-id="16dd5-751">EF5</span><span class="sxs-lookup"><span data-stu-id="16dd5-751">EF5</span></span> | <span data-ttu-id="16dd5-752">Kompilovaný dotaz ObjectContext</span><span class="sxs-lookup"><span data-stu-id="16dd5-752">ObjectContext Compiled Query</span></span>                | <span data-ttu-id="16dd5-753">1099</span><span class="sxs-lookup"><span data-stu-id="16dd5-753">1099</span></span>      | <span data-ttu-id="16dd5-754">38682624</span><span class="sxs-lookup"><span data-stu-id="16dd5-754">38682624</span></span> |
| <span data-ttu-id="16dd5-755">EF5</span><span class="sxs-lookup"><span data-stu-id="16dd5-755">EF5</span></span> | <span data-ttu-id="16dd5-756">Dotaz LINQ pro ObjectContext</span><span class="sxs-lookup"><span data-stu-id="16dd5-756">ObjectContext Linq Query</span></span>                    | <span data-ttu-id="16dd5-757">1152</span><span class="sxs-lookup"><span data-stu-id="16dd5-757">1152</span></span>      | <span data-ttu-id="16dd5-758">38178816</span><span class="sxs-lookup"><span data-stu-id="16dd5-758">38178816</span></span> |
| <span data-ttu-id="16dd5-759">EF5</span><span class="sxs-lookup"><span data-stu-id="16dd5-759">EF5</span></span> | <span data-ttu-id="16dd5-760">DbContext dotaz LINQ bez sledování</span><span class="sxs-lookup"><span data-stu-id="16dd5-760">DbContext Linq Query No Tracking</span></span>            | <span data-ttu-id="16dd5-761">1208</span><span class="sxs-lookup"><span data-stu-id="16dd5-761">1208</span></span>      | <span data-ttu-id="16dd5-762">41803776</span><span class="sxs-lookup"><span data-stu-id="16dd5-762">41803776</span></span> |
| <span data-ttu-id="16dd5-763">EF5</span><span class="sxs-lookup"><span data-stu-id="16dd5-763">EF5</span></span> | <span data-ttu-id="16dd5-764">DbContext dotaz SQL na Negenerickými</span><span class="sxs-lookup"><span data-stu-id="16dd5-764">DbContext Sql Query on DbSet</span></span>                | <span data-ttu-id="16dd5-765">1414</span><span class="sxs-lookup"><span data-stu-id="16dd5-765">1414</span></span>      | <span data-ttu-id="16dd5-766">37982208</span><span class="sxs-lookup"><span data-stu-id="16dd5-766">37982208</span></span> |
| <span data-ttu-id="16dd5-767">EF5</span><span class="sxs-lookup"><span data-stu-id="16dd5-767">EF5</span></span> | <span data-ttu-id="16dd5-768">Dotaz LINQ DbContext</span><span class="sxs-lookup"><span data-stu-id="16dd5-768">DbContext Linq Query</span></span>                        | <span data-ttu-id="16dd5-769">1574</span><span class="sxs-lookup"><span data-stu-id="16dd5-769">1574</span></span>      | <span data-ttu-id="16dd5-770">41738240</span><span class="sxs-lookup"><span data-stu-id="16dd5-770">41738240</span></span> |
|     |                                             |           |          |
| <span data-ttu-id="16dd5-771">EF6</span><span class="sxs-lookup"><span data-stu-id="16dd5-771">EF6</span></span> | <span data-ttu-id="16dd5-772">ObjectContext – příkaz entity</span><span class="sxs-lookup"><span data-stu-id="16dd5-772">ObjectContext Entity Command</span></span>                | <span data-ttu-id="16dd5-773">480</span><span class="sxs-lookup"><span data-stu-id="16dd5-773">480</span></span>       | <span data-ttu-id="16dd5-774">47247360</span><span class="sxs-lookup"><span data-stu-id="16dd5-774">47247360</span></span> |
| <span data-ttu-id="16dd5-775">EF6</span><span class="sxs-lookup"><span data-stu-id="16dd5-775">EF6</span></span> | <span data-ttu-id="16dd5-776">Dotaz na úložiště ObjectContext</span><span class="sxs-lookup"><span data-stu-id="16dd5-776">ObjectContext Store Query</span></span>                   | <span data-ttu-id="16dd5-777">493</span><span class="sxs-lookup"><span data-stu-id="16dd5-777">493</span></span>       | <span data-ttu-id="16dd5-778">46739456</span><span class="sxs-lookup"><span data-stu-id="16dd5-778">46739456</span></span> |
| <span data-ttu-id="16dd5-779">EF6</span><span class="sxs-lookup"><span data-stu-id="16dd5-779">EF6</span></span> | <span data-ttu-id="16dd5-780">DbContext dotaz SQL v databázi</span><span class="sxs-lookup"><span data-stu-id="16dd5-780">DbContext Sql Query on Database</span></span>             | <span data-ttu-id="16dd5-781">614</span><span class="sxs-lookup"><span data-stu-id="16dd5-781">614</span></span>       | <span data-ttu-id="16dd5-782">41607168</span><span class="sxs-lookup"><span data-stu-id="16dd5-782">41607168</span></span> |
| <span data-ttu-id="16dd5-783">EF6</span><span class="sxs-lookup"><span data-stu-id="16dd5-783">EF6</span></span> | <span data-ttu-id="16dd5-784">Nesledovaný dotaz LINQ pro ObjectContext</span><span class="sxs-lookup"><span data-stu-id="16dd5-784">ObjectContext Linq Query No Tracking</span></span>        | <span data-ttu-id="16dd5-785">684</span><span class="sxs-lookup"><span data-stu-id="16dd5-785">684</span></span>       | <span data-ttu-id="16dd5-786">46333952</span><span class="sxs-lookup"><span data-stu-id="16dd5-786">46333952</span></span> |
| <span data-ttu-id="16dd5-787">EF6</span><span class="sxs-lookup"><span data-stu-id="16dd5-787">EF6</span></span> | <span data-ttu-id="16dd5-788">Entita objektu ObjectContext SQL s použitím dotazu na objekt</span><span class="sxs-lookup"><span data-stu-id="16dd5-788">ObjectContext Entity Sql using Object Query</span></span> | <span data-ttu-id="16dd5-789">767</span><span class="sxs-lookup"><span data-stu-id="16dd5-789">767</span></span>       | <span data-ttu-id="16dd5-790">48865280</span><span class="sxs-lookup"><span data-stu-id="16dd5-790">48865280</span></span> |
| <span data-ttu-id="16dd5-791">EF6</span><span class="sxs-lookup"><span data-stu-id="16dd5-791">EF6</span></span> | <span data-ttu-id="16dd5-792">Kompilovaný dotaz ObjectContext</span><span class="sxs-lookup"><span data-stu-id="16dd5-792">ObjectContext Compiled Query</span></span>                | <span data-ttu-id="16dd5-793">788</span><span class="sxs-lookup"><span data-stu-id="16dd5-793">788</span></span>       | <span data-ttu-id="16dd5-794">48467968</span><span class="sxs-lookup"><span data-stu-id="16dd5-794">48467968</span></span> |
| <span data-ttu-id="16dd5-795">EF6</span><span class="sxs-lookup"><span data-stu-id="16dd5-795">EF6</span></span> | <span data-ttu-id="16dd5-796">DbContext dotaz LINQ bez sledování</span><span class="sxs-lookup"><span data-stu-id="16dd5-796">DbContext Linq Query No Tracking</span></span>            | <span data-ttu-id="16dd5-797">878</span><span class="sxs-lookup"><span data-stu-id="16dd5-797">878</span></span>       | <span data-ttu-id="16dd5-798">47554560</span><span class="sxs-lookup"><span data-stu-id="16dd5-798">47554560</span></span> |
| <span data-ttu-id="16dd5-799">EF6</span><span class="sxs-lookup"><span data-stu-id="16dd5-799">EF6</span></span> | <span data-ttu-id="16dd5-800">Dotaz LINQ pro ObjectContext</span><span class="sxs-lookup"><span data-stu-id="16dd5-800">ObjectContext Linq Query</span></span>                    | <span data-ttu-id="16dd5-801">953</span><span class="sxs-lookup"><span data-stu-id="16dd5-801">953</span></span>       | <span data-ttu-id="16dd5-802">47632384</span><span class="sxs-lookup"><span data-stu-id="16dd5-802">47632384</span></span> |
| <span data-ttu-id="16dd5-803">EF6</span><span class="sxs-lookup"><span data-stu-id="16dd5-803">EF6</span></span> | <span data-ttu-id="16dd5-804">DbContext dotaz SQL na Negenerickými</span><span class="sxs-lookup"><span data-stu-id="16dd5-804">DbContext Sql Query on DbSet</span></span>                | <span data-ttu-id="16dd5-805">1023</span><span class="sxs-lookup"><span data-stu-id="16dd5-805">1023</span></span>      | <span data-ttu-id="16dd5-806">41992192</span><span class="sxs-lookup"><span data-stu-id="16dd5-806">41992192</span></span> |
| <span data-ttu-id="16dd5-807">EF6</span><span class="sxs-lookup"><span data-stu-id="16dd5-807">EF6</span></span> | <span data-ttu-id="16dd5-808">Dotaz LINQ DbContext</span><span class="sxs-lookup"><span data-stu-id="16dd5-808">DbContext Linq Query</span></span>                        | <span data-ttu-id="16dd5-809">1290</span><span class="sxs-lookup"><span data-stu-id="16dd5-809">1290</span></span>      | <span data-ttu-id="16dd5-810">47529984</span><span class="sxs-lookup"><span data-stu-id="16dd5-810">47529984</span></span> |


![EF5 zahřívání dotaz 1000 – iterace](~/ef6/media/ef5warmquery1000.png)

![EF6 zahřívání dotaz 1000 – iterace](~/ef6/media/ef6warmquery1000.png)

> [!NOTE]
> <span data-ttu-id="16dd5-813">V případě úplnosti jsme zahrnuli variaci, kde spustíme Entity SQL dotaz na EntityCommand.</span><span class="sxs-lookup"><span data-stu-id="16dd5-813">For completeness, we included a variation where we execute an Entity SQL query on an EntityCommand.</span></span> <span data-ttu-id="16dd5-814">Nicméně vzhledem k tomu, že výsledky nejsou pro takové dotazy materializované, porovnání není nutně v případě jablek.</span><span class="sxs-lookup"><span data-stu-id="16dd5-814">However, because results are not materialized for such queries, the comparison isn't necessarily apples-to-apples.</span></span> <span data-ttu-id="16dd5-815">Test zahrnuje přibližnou aproximaci pro vyhodnocování, aby se pokus o porovnání povedl.</span><span class="sxs-lookup"><span data-stu-id="16dd5-815">The test includes a close approximation to materializing to try making the comparison fairer.</span></span>

<span data-ttu-id="16dd5-816">V tomto koncovém případě Entity Framework 6 Entity Framework 5 z důvodu zvýšení výkonu provedených na několika částech zásobníku, včetně mnohem světlejší DbContext inicializace a rychlejších hledání metadat @ no__t-0T @ no__t-1.</span><span class="sxs-lookup"><span data-stu-id="16dd5-816">In this end-to-end case, Entity Framework 6 outperforms Entity Framework 5 due to performance improvements made on several parts of the stack, including a much lighter DbContext initialization and faster MetadataCollection&lt;T&gt; lookups.</span></span>

## <a name="7-design-time-performance-considerations"></a><span data-ttu-id="16dd5-817">7 – požadavky na výkon při návrhu</span><span class="sxs-lookup"><span data-stu-id="16dd5-817">7 Design time performance considerations</span></span>

### <a name="71-inheritance-strategies"></a><span data-ttu-id="16dd5-818">7,1 strategie dědičnosti</span><span class="sxs-lookup"><span data-stu-id="16dd5-818">7.1       Inheritance Strategies</span></span>

<span data-ttu-id="16dd5-819">Dalším aspektem výkonu při použití Entity Framework je strategie dědičnosti, kterou používáte.</span><span class="sxs-lookup"><span data-stu-id="16dd5-819">Another performance consideration when using Entity Framework is the inheritance strategy you use.</span></span> <span data-ttu-id="16dd5-820">Entity Framework podporuje 3 základní typy dědičnosti a jejich kombinace:</span><span class="sxs-lookup"><span data-stu-id="16dd5-820">Entity Framework supports 3 basic types of inheritance and their combinations:</span></span>

-   <span data-ttu-id="16dd5-821">Tabulka na hierarchii (TPH) – kde každá sada dědičnosti mapuje na tabulku se sloupcem diskriminátoru, který označuje, který konkrétní typ v hierarchii je reprezentován na řádku.</span><span class="sxs-lookup"><span data-stu-id="16dd5-821">Table per Hierarchy (TPH) – where each inheritance set maps to a table with a discriminator column to indicate which particular type in the hierarchy is being represented in the row.</span></span>
-   <span data-ttu-id="16dd5-822">Tabulka na typ (TPT) – kde každý typ má svou vlastní tabulku v databázi; podřízené tabulky definují pouze sloupce, které nadřazená tabulka neobsahuje.</span><span class="sxs-lookup"><span data-stu-id="16dd5-822">Table per Type (TPT) – where each type has its own table in the database; the child tables only define the columns that the parent table doesn’t contain.</span></span>
-   <span data-ttu-id="16dd5-823">Tabulka na třídu (TPC) – kde každý typ má svou vlastní úplnou tabulku v databázi; podřízené tabulky definují všechna jejich pole, včetně těch definovaných v nadřazených typech.</span><span class="sxs-lookup"><span data-stu-id="16dd5-823">Table per Class (TPC) – where each type has its own full table in the database; the child tables define all their fields, including those defined in parent types.</span></span>

<span data-ttu-id="16dd5-824">Pokud váš model používá dědění TPT, generované dotazy budou složitější než ty, které jsou vygenerovány jinými strategiemi dědičnosti, což může vést k delší době spuštění v úložišti.</span><span class="sxs-lookup"><span data-stu-id="16dd5-824">If your model uses TPT inheritance, the queries which are generated will be more complex than those that are generated with the other inheritance strategies, which may result on longer execution times on the store.</span></span><span data-ttu-id="16dd5-825">  Generování dotazů přes TPT model a vyhodnotit výsledných objektů bude obecně trvat déle.</span><span class="sxs-lookup"><span data-stu-id="16dd5-825">  It will generally take longer to generate queries over a TPT model, and to materialize the resulting objects.</span></span>

<span data-ttu-id="16dd5-826">"Důležité informace o výkonu při použití dědičnosti TPT (tabulka na jeden typ) v Entity Framework" najdete v příspěvku blogu MSDN: \<http://blogs.msdn.com/b/adonet/archive/2010/08/17/performance-considerations-when-using-tpt-table-per-type-inheritance-in-the-entity-framework.aspx>.</span><span class="sxs-lookup"><span data-stu-id="16dd5-826">See the "Performance Considerations when using TPT (Table per Type) Inheritance in the Entity Framework" MSDN blog post: \<http://blogs.msdn.com/b/adonet/archive/2010/08/17/performance-considerations-when-using-tpt-table-per-type-inheritance-in-the-entity-framework.aspx>.</span></span>

#### <a name="711-avoiding-tpt-in-model-first-or-code-first-applications"></a><span data-ttu-id="16dd5-827">7.1.1 zamezení TPT v aplikacích Model First nebo Code First</span><span class="sxs-lookup"><span data-stu-id="16dd5-827">7.1.1       Avoiding TPT in Model First or Code First applications</span></span>

<span data-ttu-id="16dd5-828">Když vytvoříte model přes existující databázi, která má schéma TPT, nemáte mnoho možností.</span><span class="sxs-lookup"><span data-stu-id="16dd5-828">When you create a model over an existing database that has a TPT schema, you don't have many options.</span></span> <span data-ttu-id="16dd5-829">Ale při vytváření aplikace pomocí Model First nebo Code First byste se měli vyhnout dědičnosti TPT pro problémy s výkonem.</span><span class="sxs-lookup"><span data-stu-id="16dd5-829">But when creating an application using Model First or Code First, you should avoid TPT inheritance for performance concerns.</span></span>

<span data-ttu-id="16dd5-830">Při použití Model First v průvodci Entity Designer získáte TPT pro jakoukoliv dědičnost v modelu.</span><span class="sxs-lookup"><span data-stu-id="16dd5-830">When you use Model First in the Entity Designer Wizard, you will get TPT for any inheritance in your model.</span></span> <span data-ttu-id="16dd5-831">Pokud chcete přepnout na strategii TPH dědičnosti s první Model, můžete použít "Entity návrháře databáze generování Power Pack" k dispozici z Galerie sady Visual Studio ( \<http://visualstudiogallery.msdn.microsoft.com/df3541c3-d833-4b65-b942-989e7ec74c87/>).</span><span class="sxs-lookup"><span data-stu-id="16dd5-831">If you want to switch to a TPH inheritance strategy with Model First, you can use the "Entity Designer Database Generation Power Pack" available from the Visual Studio Gallery ( \<http://visualstudiogallery.msdn.microsoft.com/df3541c3-d833-4b65-b942-989e7ec74c87/>).</span></span>

<span data-ttu-id="16dd5-832">Při použití Code First ke konfiguraci mapování modelu s děděním bude EF standardně používat TPH, takže všechny entity v hierarchii dědičnosti budou namapovány na stejnou tabulku.</span><span class="sxs-lookup"><span data-stu-id="16dd5-832">When using Code First to configure the mapping of a model with inheritance, EF will use TPH by default, therefore all entities in the inheritance hierarchy will be mapped to the same table.</span></span> <span data-ttu-id="16dd5-833">V části "Mapování s rozhraní Fluent API" z "Kód první v entitě Framework4.1" článek v časopise MSDN Magazine ( [http://msdn.microsoft.com/magazine/hh126815.aspx](https://msdn.microsoft.com/magazine/hh126815.aspx)) pro další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="16dd5-833">See the "Mapping with the Fluent API" section of the "Code First in Entity Framework4.1" article in MSDN Magazine ( [http://msdn.microsoft.com/magazine/hh126815.aspx](https://msdn.microsoft.com/magazine/hh126815.aspx)) for more details.</span></span>

### <a name="72-upgrading-from-ef4-to-improve-model-generation-time"></a><span data-ttu-id="16dd5-834">7,2 upgrade z EF4 na zlepšení času generování modelu</span><span class="sxs-lookup"><span data-stu-id="16dd5-834">7.2       Upgrading from EF4 to improve model generation time</span></span>

<span data-ttu-id="16dd5-835">Vylepšení algoritmu, který generuje vrstvu úložiště (SSDL) modelu, je k dispozici v Entity Framework 5 a 6 a jako aktualizace Entity Framework 4 při instalaci sady Visual Studio 2010 SP1. SQL Server</span><span class="sxs-lookup"><span data-stu-id="16dd5-835">A SQL Server-specific improvement to the algorithm that generates the store-layer (SSDL) of the model is available in Entity Framework 5 and 6, and as an update to Entity Framework 4 when Visual Studio 2010 SP1 is installed.</span></span> <span data-ttu-id="16dd5-836">Následující výsledky testu ukazují vylepšení při generování velmi velkého modelu, v tomto případě v modelu Navision.</span><span class="sxs-lookup"><span data-stu-id="16dd5-836">The following test results demonstrate the improvement when generating a very big model, in this case the Navision model.</span></span> <span data-ttu-id="16dd5-837">Další podrobnosti najdete v příloze C.</span><span class="sxs-lookup"><span data-stu-id="16dd5-837">See Appendix C for more details about it.</span></span>

<span data-ttu-id="16dd5-838">Model obsahuje sady entit 1005 a sady přidružení 4227.</span><span class="sxs-lookup"><span data-stu-id="16dd5-838">The model contains 1005 entity sets and 4227 association sets.</span></span>

| <span data-ttu-id="16dd5-839">Konfiguraci</span><span class="sxs-lookup"><span data-stu-id="16dd5-839">Configuration</span></span>                              | <span data-ttu-id="16dd5-840">Rozpis spotřebovaného času</span><span class="sxs-lookup"><span data-stu-id="16dd5-840">Breakdown of time consumed</span></span>                                                                                                                                               |
|:-------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="16dd5-841">Visual Studio 2010, Entity Framework 4</span><span class="sxs-lookup"><span data-stu-id="16dd5-841">Visual Studio 2010, Entity Framework 4</span></span>     | <span data-ttu-id="16dd5-842">Generace SSDL: 2 hr 27 min.</span><span class="sxs-lookup"><span data-stu-id="16dd5-842">SSDL Generation: 2 hr 27 min</span></span> <br/> <span data-ttu-id="16dd5-843">Generování mapování: 1 sekunda</span><span class="sxs-lookup"><span data-stu-id="16dd5-843">Mapping Generation: 1 second</span></span> <br/> <span data-ttu-id="16dd5-844">Generování CSDL: 1 sekunda</span><span class="sxs-lookup"><span data-stu-id="16dd5-844">CSDL Generation: 1 second</span></span> <br/> <span data-ttu-id="16dd5-845">Generování ObjectLayer: 1 sekunda</span><span class="sxs-lookup"><span data-stu-id="16dd5-845">ObjectLayer Generation: 1 second</span></span> <br/> <span data-ttu-id="16dd5-846">Generování zobrazení: 2 h 14 min.</span><span class="sxs-lookup"><span data-stu-id="16dd5-846">View Generation: 2 h 14 min</span></span> |
| <span data-ttu-id="16dd5-847">Visual Studio 2010 SP1, Entity Framework 4</span><span class="sxs-lookup"><span data-stu-id="16dd5-847">Visual Studio 2010 SP1, Entity Framework 4</span></span> | <span data-ttu-id="16dd5-848">Generace SSDL: 1 sekunda</span><span class="sxs-lookup"><span data-stu-id="16dd5-848">SSDL Generation: 1 second</span></span> <br/> <span data-ttu-id="16dd5-849">Generování mapování: 1 sekunda</span><span class="sxs-lookup"><span data-stu-id="16dd5-849">Mapping Generation: 1 second</span></span> <br/> <span data-ttu-id="16dd5-850">Generování CSDL: 1 sekunda</span><span class="sxs-lookup"><span data-stu-id="16dd5-850">CSDL Generation: 1 second</span></span> <br/> <span data-ttu-id="16dd5-851">Generování ObjectLayer: 1 sekunda</span><span class="sxs-lookup"><span data-stu-id="16dd5-851">ObjectLayer Generation: 1 second</span></span> <br/> <span data-ttu-id="16dd5-852">Generování zobrazení: 1 hr 53 min.</span><span class="sxs-lookup"><span data-stu-id="16dd5-852">View Generation: 1 hr 53 min</span></span>   |
| <span data-ttu-id="16dd5-853">Visual Studio 2013 Entity Framework 5</span><span class="sxs-lookup"><span data-stu-id="16dd5-853">Visual Studio 2013, Entity Framework 5</span></span>     | <span data-ttu-id="16dd5-854">Generace SSDL: 1 sekunda</span><span class="sxs-lookup"><span data-stu-id="16dd5-854">SSDL Generation: 1 second</span></span> <br/> <span data-ttu-id="16dd5-855">Generování mapování: 1 sekunda</span><span class="sxs-lookup"><span data-stu-id="16dd5-855">Mapping Generation: 1 second</span></span> <br/> <span data-ttu-id="16dd5-856">Generování CSDL: 1 sekunda</span><span class="sxs-lookup"><span data-stu-id="16dd5-856">CSDL Generation: 1 second</span></span> <br/> <span data-ttu-id="16dd5-857">Generování ObjectLayer: 1 sekunda</span><span class="sxs-lookup"><span data-stu-id="16dd5-857">ObjectLayer Generation: 1 second</span></span> <br/> <span data-ttu-id="16dd5-858">Generování zobrazení: 65 minut</span><span class="sxs-lookup"><span data-stu-id="16dd5-858">View Generation: 65 minutes</span></span>    |
| <span data-ttu-id="16dd5-859">Visual Studio 2013, Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="16dd5-859">Visual Studio 2013, Entity Framework 6</span></span>     | <span data-ttu-id="16dd5-860">Generace SSDL: 1 sekunda</span><span class="sxs-lookup"><span data-stu-id="16dd5-860">SSDL Generation: 1 second</span></span> <br/> <span data-ttu-id="16dd5-861">Generování mapování: 1 sekunda</span><span class="sxs-lookup"><span data-stu-id="16dd5-861">Mapping Generation: 1 second</span></span> <br/> <span data-ttu-id="16dd5-862">Generování CSDL: 1 sekunda</span><span class="sxs-lookup"><span data-stu-id="16dd5-862">CSDL Generation: 1 second</span></span> <br/> <span data-ttu-id="16dd5-863">Generování ObjectLayer: 1 sekunda</span><span class="sxs-lookup"><span data-stu-id="16dd5-863">ObjectLayer Generation: 1 second</span></span> <br/> <span data-ttu-id="16dd5-864">Generování zobrazení: 28 sekund.</span><span class="sxs-lookup"><span data-stu-id="16dd5-864">View Generation: 28 seconds.</span></span>   |


<span data-ttu-id="16dd5-865">Je potřeba poznamenat, že při generování SSDL se zatížení téměř zcela stráví na SQL Server, zatímco vývojový počítač klienta čeká na nečinnost, než se výsledky vrátí ze serveru.</span><span class="sxs-lookup"><span data-stu-id="16dd5-865">It's worth noting that when generating the SSDL, the load is almost entirely spent on the SQL Server, while the client development machine is waiting idle for results to come back from the server.</span></span> <span data-ttu-id="16dd5-866">Specializující by mělo toto vylepšení obzvlášť.</span><span class="sxs-lookup"><span data-stu-id="16dd5-866">DBAs should particularly appreciate this improvement.</span></span> <span data-ttu-id="16dd5-867">Také je potřeba poznamenat, že v zásadě zobrazení generace probíhá celá cena za generování modelu.</span><span class="sxs-lookup"><span data-stu-id="16dd5-867">It's also worth noting that essentially the entire cost of model generation takes place in View Generation now.</span></span>

### <a name="73-splitting-large-models-with-database-first-and-model-first"></a><span data-ttu-id="16dd5-868">7,3 rozdělení velkých modelů pomocí Database First a Model First</span><span class="sxs-lookup"><span data-stu-id="16dd5-868">7.3       Splitting Large Models with Database First and Model First</span></span>

<span data-ttu-id="16dd5-869">Když se zvětší velikost modelu, plocha návrháře bude nenáročná a bude obtížné ji používat.</span><span class="sxs-lookup"><span data-stu-id="16dd5-869">As model size increases, the designer surface becomes cluttered and difficult to use.</span></span> <span data-ttu-id="16dd5-870">Model s více než 300 entitami obvykle považujeme za příliš velký, aby bylo možné efektivně používat návrháře.</span><span class="sxs-lookup"><span data-stu-id="16dd5-870">We typically consider a model with more than 300 entities to be too large to effectively use the designer.</span></span> <span data-ttu-id="16dd5-871">V následujícím příspěvku blogu popisuje několik možností pro rozdělení velkých modelů: \<http://blogs.msdn.com/b/adonet/archive/2008/11/25/working-with-large-models-in-entity-framework-part-2.aspx>.</span><span class="sxs-lookup"><span data-stu-id="16dd5-871">The following blog post describes several options for splitting large models: \<http://blogs.msdn.com/b/adonet/archive/2008/11/25/working-with-large-models-in-entity-framework-part-2.aspx>.</span></span>

<span data-ttu-id="16dd5-872">Příspěvek byl napsaný pro první verzi Entity Framework, ale postup se pořád týká.</span><span class="sxs-lookup"><span data-stu-id="16dd5-872">The post was written for the first version of Entity Framework, but the steps still apply.</span></span>

### <a name="74-performance-considerations-with-the-entity-data-source-control"></a><span data-ttu-id="16dd5-873">7,4 požadavky na výkon pomocí ovládacího prvku zdroje dat entity</span><span class="sxs-lookup"><span data-stu-id="16dd5-873">7.4       Performance considerations with the Entity Data Source Control</span></span>

<span data-ttu-id="16dd5-874">Zjistili jsme případy s vícevláknovými testy výkonu a zátěže, kde výkon webové aplikace s použitím ovládacího prvku EntityDataSource výrazně zhoršuje.</span><span class="sxs-lookup"><span data-stu-id="16dd5-874">We've seen cases in multi-threaded performance and stress tests where the performance of a web application using the EntityDataSource Control deteriorates significantly.</span></span> <span data-ttu-id="16dd5-875">Základní příčinou je, že objekt EntityDataSource opakovaně volá MetadataWorkspace. LoadFromAssembly na sestavení, na která webová aplikace odkazuje, aby zjistila typy, které se mají použít jako entity.</span><span class="sxs-lookup"><span data-stu-id="16dd5-875">The underlying cause is that the EntityDataSource repeatedly calls MetadataWorkspace.LoadFromAssembly on the assemblies referenced by the Web application to discover the types to be used as entities.</span></span>

<span data-ttu-id="16dd5-876">Řešením je nastavit ContextTypeName objektu EntityDataSource na název typu odvozené třídy ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="16dd5-876">The solution is to set the ContextTypeName of the EntityDataSource to the type name of your derived ObjectContext class.</span></span> <span data-ttu-id="16dd5-877">Tím se vypne mechanismus, který kontroluje všechna odkazovaná sestavení pro typy entit.</span><span class="sxs-lookup"><span data-stu-id="16dd5-877">This turns off the mechanism that scans all referenced assemblies for entity types.</span></span>

<span data-ttu-id="16dd5-878">Nastavení pole ContextTypeName také zabraňuje funkčnímu problému, kde EntityDataSource v rozhraní .NET 4,0 vyvolá výjimku ReflectionTypeLoadException, pokud nemůže načíst typ ze sestavení prostřednictvím reflexe.</span><span class="sxs-lookup"><span data-stu-id="16dd5-878">Setting the ContextTypeName field also prevents a functional problem where the EntityDataSource in .NET 4.0 throws a ReflectionTypeLoadException when it can't load a type from an assembly via reflection.</span></span> <span data-ttu-id="16dd5-879">Tento problém byl vyřešen v rozhraní .NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="16dd5-879">This issue has been fixed in .NET 4.5.</span></span>

### <a name="75-poco-entities-and-change-tracking-proxies"></a><span data-ttu-id="16dd5-880">7,5 entit POCO a proxy serverů pro sledování změn</span><span class="sxs-lookup"><span data-stu-id="16dd5-880">7.5       POCO entities and change tracking proxies</span></span>

<span data-ttu-id="16dd5-881">Entity Framework umožňuje používat vlastní datové třídy spolu s datovým modelem, aniž by bylo nutné provádět jakékoli úpravy samotných datových tříd.</span><span class="sxs-lookup"><span data-stu-id="16dd5-881">Entity Framework enables you to use custom data classes together with your data model without making any modifications to the data classes themselves.</span></span> <span data-ttu-id="16dd5-882">To znamená, že s datovým modelem můžete použít "objekty CLR" ve starém Old "(POCO), jako například existující objekty domény.</span><span class="sxs-lookup"><span data-stu-id="16dd5-882">This means that you can use "plain-old" CLR objects (POCO), such as existing domain objects, with your data model.</span></span> <span data-ttu-id="16dd5-883">Tyto POCO datové třídy (označované také jako trvalá ignorování objektů), které jsou mapovány na entity, které jsou definovány v datovém modelu, podporují většinu stejného chování dotazu, vložení, aktualizace a odstranění jako typy entit, které jsou generovány nástroji model EDM (Entity Data Model).</span><span class="sxs-lookup"><span data-stu-id="16dd5-883">These POCO data classes (also known as persistence-ignorant objects), which are mapped to entities that are defined in a data model, support most of the same query, insert, update, and delete behaviors as entity types that are generated by the Entity Data Model tools.</span></span>

<span data-ttu-id="16dd5-884">Entity Framework může také vytvořit proxy třídy odvozené z vašich typů POCO, které se používají, pokud chcete povolit funkce, jako je opožděné načítání a automatické sledování změn v entitách POCO.</span><span class="sxs-lookup"><span data-stu-id="16dd5-884">Entity Framework can also create proxy classes derived from your POCO types, which are used when you want to enable features such as lazy loading and automatic change tracking on POCO entities.</span></span> <span data-ttu-id="16dd5-885">Třídy POCO musí splňovat určité požadavky umožňující Entity Framework pro použití proxy, jak je popsáno zde: [http://msdn.microsoft.com/library/dd468057.aspx](https://msdn.microsoft.com/library/dd468057.aspx).</span><span class="sxs-lookup"><span data-stu-id="16dd5-885">Your POCO classes must meet certain requirements to allow Entity Framework to use proxies, as described here: [http://msdn.microsoft.com/library/dd468057.aspx](https://msdn.microsoft.com/library/dd468057.aspx).</span></span>

<span data-ttu-id="16dd5-886">Nepravděpodobné sledování proxy serverů upozorní správce stavu objektu pokaždé, když kterákoli z vlastností vašich entit má změnu hodnoty, takže Entity Framework ví skutečný stav entit po celou dobu.</span><span class="sxs-lookup"><span data-stu-id="16dd5-886">Chance tracking proxies will notify the object state manager each time any of the properties of your entities has its value changed, so Entity Framework knows the actual state of your entities all the time.</span></span> <span data-ttu-id="16dd5-887">K tomu je potřeba přidat události oznámení do těla metod setter vašich vlastností a nechat správce stavu objektů takové události zpracovat.</span><span class="sxs-lookup"><span data-stu-id="16dd5-887">This is done by adding notification events to the body of the setter methods of your properties, and having the object state manager processing such events.</span></span> <span data-ttu-id="16dd5-888">Všimněte si, že vytvoření entity proxy serveru bude obvykle dražší než vytváření neproxy entity POCO z důvodu přidané sady událostí vytvořených pomocí Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="16dd5-888">Note that creating a proxy entity will typically be more expensive than creating a non-proxy POCO entity due to the added set of events created by Entity Framework.</span></span>

<span data-ttu-id="16dd5-889">Pokud entita POCO nemá proxy server pro sledování změn, budou nalezeny změny porovnáním obsahu entit s kopií předchozího uloženého stavu.</span><span class="sxs-lookup"><span data-stu-id="16dd5-889">When a POCO entity does not have a change tracking proxy, changes are found by comparing the contents of your entities against a copy of a previous saved state.</span></span> <span data-ttu-id="16dd5-890">Toto hloubkové porovnání se změní na zdlouhavý proces, pokud máte ve svém kontextu mnoho entit nebo když vaše entity mají velmi velký objem vlastností, a to i v případě, že se žádná z nich od posledního porovnání nezměnila.</span><span class="sxs-lookup"><span data-stu-id="16dd5-890">This deep comparison will become a lengthy process when you have many entities in your context, or when your entities have a very large amount of properties, even if none of them changed since the last comparison took place.</span></span>

<span data-ttu-id="16dd5-891">Shrnutí: při vytváření proxy serveru pro sledování změn platíte výkon, ale sledování změn vám pomůže zrychlit proces zjišťování změn, pokud mají vaše entity mnoho vlastností nebo pokud máte v modelu mnoho entit.</span><span class="sxs-lookup"><span data-stu-id="16dd5-891">In summary: you’ll pay a performance hit when creating the change tracking proxy, but change tracking will help you speed up the change detection process when your entities have many properties or when you have many entities in your model.</span></span> <span data-ttu-id="16dd5-892">U entit s malým počtem vlastností, kde množství entit neroste příliš mnoho, nemusí mít proxy servery pro sledování změn žádný přínos.</span><span class="sxs-lookup"><span data-stu-id="16dd5-892">For entities with a small number of properties where the amount of entities doesn’t grow too much, having change tracking proxies may not be of much benefit.</span></span>

## <a name="8-loading-related-entities"></a><span data-ttu-id="16dd5-893">8\. načítání souvisejících entit</span><span class="sxs-lookup"><span data-stu-id="16dd5-893">8 Loading Related Entities</span></span>

### <a name="81-lazy-loading-vs-eager-loading"></a><span data-ttu-id="16dd5-894">8,1 opožděné načítání vs. Eager načítání</span><span class="sxs-lookup"><span data-stu-id="16dd5-894">8.1 Lazy Loading vs. Eager Loading</span></span>

<span data-ttu-id="16dd5-895">Entity Framework nabízí několik různých způsobů, jak načíst entity, které souvisejí s cílovou entitou.</span><span class="sxs-lookup"><span data-stu-id="16dd5-895">Entity Framework offers several different ways to load the entities that are related to your target entity.</span></span> <span data-ttu-id="16dd5-896">Například při dotazování na produkty existují různé způsoby, jak budou související objednávky načteny do Správce stavu objektu.</span><span class="sxs-lookup"><span data-stu-id="16dd5-896">For example, when you query for Products, there are different ways that the related Orders will be loaded into the Object State Manager.</span></span> <span data-ttu-id="16dd5-897">Z hlediska výkonu je největší otázkou, kterou je třeba vzít v úvahu při načítání souvisejících entit, použití opožděného načítání nebo Eager načítání.</span><span class="sxs-lookup"><span data-stu-id="16dd5-897">From a performance standpoint, the biggest question to consider when loading related entities will be whether to use Lazy Loading or Eager Loading.</span></span>

<span data-ttu-id="16dd5-898">Při použití Eager načítání jsou související entity načteny spolu s vaší cílovou sadou entit.</span><span class="sxs-lookup"><span data-stu-id="16dd5-898">When using Eager Loading, the related entities are loaded along with your target entity set.</span></span> <span data-ttu-id="16dd5-899">Pomocí příkazu include v dotazu můžete určit, které související entity chcete přenést.</span><span class="sxs-lookup"><span data-stu-id="16dd5-899">You use an Include statement in your query to indicate which related entities you want to bring in.</span></span>

<span data-ttu-id="16dd5-900">Při použití opožděného načítání bude počáteční dotaz do cílové sady entit přinese pouze.</span><span class="sxs-lookup"><span data-stu-id="16dd5-900">When using Lazy Loading, your initial query only brings in the target entity set.</span></span> <span data-ttu-id="16dd5-901">Kdykoli ale přistupujete k navigační vlastnosti, k načtení související entity se v úložišti vydá jiný dotaz.</span><span class="sxs-lookup"><span data-stu-id="16dd5-901">But whenever you access a navigation property, another query is issued against the store to load the related entity.</span></span>

<span data-ttu-id="16dd5-902">Po načtení entity se všechny další dotazy pro entitu načtou přímo z správce stavu objektu bez ohledu na to, jestli používáte opožděné načítání nebo Eager načítání.</span><span class="sxs-lookup"><span data-stu-id="16dd5-902">Once an entity has been loaded, any further queries for the entity will load it directly from the Object State Manager, whether you are using lazy loading or eager loading.</span></span>

### <a name="82-how-to-choose-between-lazy-loading-and-eager-loading"></a><span data-ttu-id="16dd5-903">8,2 Jak zvolit mezi opožděným načítáním a Eager načítáním</span><span class="sxs-lookup"><span data-stu-id="16dd5-903">8.2 How to choose between Lazy Loading and Eager Loading</span></span>

<span data-ttu-id="16dd5-904">Důležité je, abyste porozuměli rozdílu mezi opožděným načítáním a Eager načítáním, abyste mohli vytvořit správnou volbu pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="16dd5-904">The important thing is that you understand the difference between Lazy Loading and Eager Loading so that you can make the correct choice for your application.</span></span> <span data-ttu-id="16dd5-905">To vám pomůže vyhodnotit kompromisy mezi několika požadavky na databázi oproti jedné žádosti, která může obsahovat velkou datovou část.</span><span class="sxs-lookup"><span data-stu-id="16dd5-905">This will help you evaluate the tradeoff between multiple requests against the database versus a single request that may contain a large payload.</span></span> <span data-ttu-id="16dd5-906">Může být vhodné použít Eager načítání v některých částech aplikace a opožděné načítání v jiných částech.</span><span class="sxs-lookup"><span data-stu-id="16dd5-906">It may be appropriate to use eager loading in some parts of your application and lazy loading in other parts.</span></span>

<span data-ttu-id="16dd5-907">Jako příklad toho, co se děje v digestoři, Předpokládejme, že chcete zadat dotaz na zákazníky, kteří žijí v rámci Spojené království a jejich počet objednávek.</span><span class="sxs-lookup"><span data-stu-id="16dd5-907">As an example of what's happening under the hood, suppose you want to query for the customers who live in the UK and their order count.</span></span>

<span data-ttu-id="16dd5-908">**Použití Eager načítání**</span><span class="sxs-lookup"><span data-stu-id="16dd5-908">**Using Eager Loading**</span></span>

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var ukCustomers = context.Customers.Include(c => c.Orders).Where(c => c.Address.Country == "UK");
    var chosenCustomer = AskUserToPickCustomer(ukCustomers);
    Console.WriteLine("Customer Id: {0} has {1} orders", customer.CustomerID, customer.Orders.Count);
}
```

<span data-ttu-id="16dd5-909">**Použití opožděného načítání**</span><span class="sxs-lookup"><span data-stu-id="16dd5-909">**Using Lazy Loading**</span></span>

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    context.ContextOptions.LazyLoadingEnabled = true;

    //Notice that the Include method call is missing in the query
    var ukCustomers = context.Customers.Where(c => c.Address.Country == "UK");

    var chosenCustomer = AskUserToPickCustomer(ukCustomers);
    Console.WriteLine("Customer Id: {0} has {1} orders", customer.CustomerID, customer.Orders.Count);
}
```

<span data-ttu-id="16dd5-910">Při použití Eager načítání vydáte jediný dotaz, který vrátí všechny zákazníky a všechny objednávky.</span><span class="sxs-lookup"><span data-stu-id="16dd5-910">When using eager loading, you'll issue a single query that returns all customers and all orders.</span></span> <span data-ttu-id="16dd5-911">Příkaz Store vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="16dd5-911">The store command looks like:</span></span>

``` SQL
SELECT
[Project1].[C1] AS [C1],
[Project1].[CustomerID] AS [CustomerID],
[Project1].[CompanyName] AS [CompanyName],
[Project1].[ContactName] AS [ContactName],
[Project1].[ContactTitle] AS [ContactTitle],
[Project1].[Address] AS [Address],
[Project1].[City] AS [City],
[Project1].[Region] AS [Region],
[Project1].[PostalCode] AS [PostalCode],
[Project1].[Country] AS [Country],
[Project1].[Phone] AS [Phone],
[Project1].[Fax] AS [Fax],
[Project1].[C2] AS [C2],
[Project1].[OrderID] AS [OrderID],
[Project1].[CustomerID1] AS [CustomerID1],
[Project1].[EmployeeID] AS [EmployeeID],
[Project1].[OrderDate] AS [OrderDate],
[Project1].[RequiredDate] AS [RequiredDate],
[Project1].[ShippedDate] AS [ShippedDate],
[Project1].[ShipVia] AS [ShipVia],
[Project1].[Freight] AS [Freight],
[Project1].[ShipName] AS [ShipName],
[Project1].[ShipAddress] AS [ShipAddress],
[Project1].[ShipCity] AS [ShipCity],
[Project1].[ShipRegion] AS [ShipRegion],
[Project1].[ShipPostalCode] AS [ShipPostalCode],
[Project1].[ShipCountry] AS [ShipCountry]
FROM ( SELECT
      [Extent1].[CustomerID] AS [CustomerID],
       [Extent1].[CompanyName] AS [CompanyName],
       [Extent1].[ContactName] AS [ContactName],
       [Extent1].[ContactTitle] AS [ContactTitle],
       [Extent1].[Address] AS [Address],
       [Extent1].[City] AS [City],
       [Extent1].[Region] AS [Region],
       [Extent1].[PostalCode] AS [PostalCode],
       [Extent1].[Country] AS [Country],
       [Extent1].[Phone] AS [Phone],
       [Extent1].[Fax] AS [Fax],
      1 AS [C1],
       [Extent2].[OrderID] AS [OrderID],
       [Extent2].[CustomerID] AS [CustomerID1],
       [Extent2].[EmployeeID] AS [EmployeeID],
       [Extent2].[OrderDate] AS [OrderDate],
       [Extent2].[RequiredDate] AS [RequiredDate],
       [Extent2].[ShippedDate] AS [ShippedDate],
       [Extent2].[ShipVia] AS [ShipVia],
       [Extent2].[Freight] AS [Freight],
       [Extent2].[ShipName] AS [ShipName],
       [Extent2].[ShipAddress] AS [ShipAddress],
       [Extent2].[ShipCity] AS [ShipCity],
       [Extent2].[ShipRegion] AS [ShipRegion],
       [Extent2].[ShipPostalCode] AS [ShipPostalCode],
       [Extent2].[ShipCountry] AS [ShipCountry],
      CASE WHEN ([Extent2].[OrderID] IS NULL) THEN CAST(NULL AS int) ELSE 1 END AS [C2]
      FROM  [dbo].[Customers] AS [Extent1]
      LEFT OUTER JOIN [dbo].[Orders] AS [Extent2] ON [Extent1].[CustomerID] = [Extent2].[CustomerID]
      WHERE N'UK' = [Extent1].[Country]
)  AS [Project1]
ORDER BY [Project1].[CustomerID] ASC, [Project1].[C2] ASC
```

<span data-ttu-id="16dd5-912">Při použití opožděného načítání vydáte na začátku následující dotaz:</span><span class="sxs-lookup"><span data-stu-id="16dd5-912">When using lazy loading, you'll issue the following query initially:</span></span>

``` SQL
SELECT
[Extent1].[CustomerID] AS [CustomerID],
[Extent1].[CompanyName] AS [CompanyName],
[Extent1].[ContactName] AS [ContactName],
[Extent1].[ContactTitle] AS [ContactTitle],
[Extent1].[Address] AS [Address],
[Extent1].[City] AS [City],
[Extent1].[Region] AS [Region],
[Extent1].[PostalCode] AS [PostalCode],
[Extent1].[Country] AS [Country],
[Extent1].[Phone] AS [Phone],
[Extent1].[Fax] AS [Fax]
FROM [dbo].[Customers] AS [Extent1]
WHERE N'UK' = [Extent1].[Country]
```

<span data-ttu-id="16dd5-913">A pokaždé, když přistupujete k vlastnosti navigace objednávky zákazníka, je na obchod vystavený jiný dotaz, jako je následující:</span><span class="sxs-lookup"><span data-stu-id="16dd5-913">And each time you access the Orders navigation property of a customer another query like the following is issued against the store:</span></span>

``` SQL
exec sp_executesql N'SELECT
[Extent1].[OrderID] AS [OrderID],
[Extent1].[CustomerID] AS [CustomerID],
[Extent1].[EmployeeID] AS [EmployeeID],
[Extent1].[OrderDate] AS [OrderDate],
[Extent1].[RequiredDate] AS [RequiredDate],
[Extent1].[ShippedDate] AS [ShippedDate],
[Extent1].[ShipVia] AS [ShipVia],
[Extent1].[Freight] AS [Freight],
[Extent1].[ShipName] AS [ShipName],
[Extent1].[ShipAddress] AS [ShipAddress],
[Extent1].[ShipCity] AS [ShipCity],
[Extent1].[ShipRegion] AS [ShipRegion],
[Extent1].[ShipPostalCode] AS [ShipPostalCode],
[Extent1].[ShipCountry] AS [ShipCountry]
FROM [dbo].[Orders] AS [Extent1]
WHERE [Extent1].[CustomerID] = @EntityKeyValue1',N'@EntityKeyValue1 nchar(5)',@EntityKeyValue1=N'AROUT'
```

<span data-ttu-id="16dd5-914">Další informace najdete v tématu [načítání souvisejících objektů](https://msdn.microsoft.com/library/bb896272.aspx).</span><span class="sxs-lookup"><span data-stu-id="16dd5-914">For more information, see the [Loading Related Objects](https://msdn.microsoft.com/library/bb896272.aspx).</span></span>

#### <a name="821-lazy-loading-versus-eager-loading-cheat-sheet"></a><span data-ttu-id="16dd5-915">8.2.1 opožděné načítání vs. Eager načítání tahák listů</span><span class="sxs-lookup"><span data-stu-id="16dd5-915">8.2.1 Lazy Loading versus Eager Loading cheat sheet</span></span>

<span data-ttu-id="16dd5-916">K dispozici není žádná taková věc jako jedna velikost, aby bylo možné vybrat Eager načítání versus opožděné načítání.</span><span class="sxs-lookup"><span data-stu-id="16dd5-916">There’s no such thing as a one-size-fits-all to choosing eager loading versus lazy loading.</span></span> <span data-ttu-id="16dd5-917">Zkuste si nejdřív porozumět rozdílům mezi oběma strategiemi, abyste mohli dělat dobře kvalifikované rozhodnutí. Zvažte také, jestli váš kód odpovídá jakémukoli z následujících scénářů:</span><span class="sxs-lookup"><span data-stu-id="16dd5-917">Try first to understand the differences between both strategies so you can do a well informed decision; also, consider if your code fits to any of the following scenarios:</span></span>

| <span data-ttu-id="16dd5-918">Scénář</span><span class="sxs-lookup"><span data-stu-id="16dd5-918">Scenario</span></span>                                                                    | <span data-ttu-id="16dd5-919">Náš návrh</span><span class="sxs-lookup"><span data-stu-id="16dd5-919">Our Suggestion</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|:----------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="16dd5-920">Potřebujete získat přístup k mnoha vlastnostem navigace z načtených entit?</span><span class="sxs-lookup"><span data-stu-id="16dd5-920">Do you need to access many navigation properties from the fetched entities?</span></span> | <span data-ttu-id="16dd5-921">**Ne** – obě možnosti budou pravděpodobně provádět.</span><span class="sxs-lookup"><span data-stu-id="16dd5-921">**No** - Both options will probably do.</span></span> <span data-ttu-id="16dd5-922">Pokud ale datová část, kterou váš dotaz přináší, není příliš velká, může při použití Eager načítání docházet k výhodám výkonu, protože k vyhodnotití vašich objektů budete potřebovat méně síťových cyklů.</span><span class="sxs-lookup"><span data-stu-id="16dd5-922">However, if the payload your query is bringing is not too big, you may experience performance benefits by using Eager loading as it’ll require less network round trips to materialize your objects.</span></span> <br/> <br/> <span data-ttu-id="16dd5-923">**Ano** – Pokud potřebujete přístup k mnoha vlastnostem navigace z entit, provedete to tak, že v dotazu pomocí Eager načítání zadáte několik příkazů include.</span><span class="sxs-lookup"><span data-stu-id="16dd5-923">**Yes** -  If you need to access many navigation properties from the entities, you’d do that by using multiple include statements in your query with Eager loading.</span></span> <span data-ttu-id="16dd5-924">Mezi další entity, které zahrnete, se zobrazí větší část datové části, kterou váš dotaz vrátí.</span><span class="sxs-lookup"><span data-stu-id="16dd5-924">The more entities you include, the bigger the payload your query will return.</span></span> <span data-ttu-id="16dd5-925">Po zahrnutí tří nebo více entit do dotazu zvažte přepnutí na opožděné načítání.</span><span class="sxs-lookup"><span data-stu-id="16dd5-925">Once you include three or more entities into your query, consider switching to Lazy loading.</span></span> |
| <span data-ttu-id="16dd5-926">Víte přesně, jaká data budou potřebná v době běhu?</span><span class="sxs-lookup"><span data-stu-id="16dd5-926">Do you know exactly what data will be needed at run time?</span></span>                   | <span data-ttu-id="16dd5-927">**Žádné** – opožděné načítání bude pro vás vhodnější.</span><span class="sxs-lookup"><span data-stu-id="16dd5-927">**No** - Lazy loading will be better for you.</span></span> <span data-ttu-id="16dd5-928">V opačném případě můžete ukončit dotazování na data, která nebudete potřebovat.</span><span class="sxs-lookup"><span data-stu-id="16dd5-928">Otherwise, you may end up querying for data that you will not need.</span></span> <br/> <br/> <span data-ttu-id="16dd5-929">**Ano** – Eager načítání je pravděpodobně nejlepším tipem; pomůže vám to rychleji načítat celé sady.</span><span class="sxs-lookup"><span data-stu-id="16dd5-929">**Yes** - Eager loading is probably your best bet; it will help loading entire sets faster.</span></span> <span data-ttu-id="16dd5-930">Pokud Váš dotaz vyžaduje načtení velmi velkého množství dat a to je příliš pomalé, zkuste místo toho použít opožděné načtení.</span><span class="sxs-lookup"><span data-stu-id="16dd5-930">If your query requires fetching a very large amount of data, and this becomes too slow, then try Lazy loading instead.</span></span>                                                                                                                                                                                                                                                       |
| <span data-ttu-id="16dd5-931">Je váš kód spuštěný z vaší databáze?</span><span class="sxs-lookup"><span data-stu-id="16dd5-931">Is your code executing far from your database?</span></span> <span data-ttu-id="16dd5-932">(zvýšené latence sítě)</span><span class="sxs-lookup"><span data-stu-id="16dd5-932">(increased network latency)</span></span>  | <span data-ttu-id="16dd5-933">**Ne** – Pokud není latence sítě problémem, může použití opožděného načítání zjednodušit váš kód.</span><span class="sxs-lookup"><span data-stu-id="16dd5-933">**No** - When the network latency is not an issue, using Lazy loading may simplify your code.</span></span> <span data-ttu-id="16dd5-934">Mějte na paměti, že se topologie vaší aplikace může změnit, takže nemusíte přebírat blízkost databáze pro udělení.</span><span class="sxs-lookup"><span data-stu-id="16dd5-934">Remember that the topology of your application may change, so don’t take database proximity for granted.</span></span> <br/> <br/> <span data-ttu-id="16dd5-935">**Ano** – Pokud se jedná o problém sítě, můžete se rozhodnout, co je pro váš scénář lepší.</span><span class="sxs-lookup"><span data-stu-id="16dd5-935">**Yes** - When the network is a problem, only you can decide what fits better for your scenario.</span></span> <span data-ttu-id="16dd5-936">Obvykle se Eager načítání bude lepší, protože vyžaduje menší počet zpátečních cest.</span><span class="sxs-lookup"><span data-stu-id="16dd5-936">Typically Eager loading will be better because it requires fewer round trips.</span></span>                                                                                                                                                                                                      |


#### <a name="822-performance-concerns-with-multiple-includes"></a><span data-ttu-id="16dd5-937">8.2.2 problémy s výkonem s několika Zahrnutími</span><span class="sxs-lookup"><span data-stu-id="16dd5-937">8.2.2       Performance concerns with multiple Includes</span></span>

<span data-ttu-id="16dd5-938">Když uslyšíme otázky ohledně výkonu, které zahrnují problémy s dobou odezvy serveru, je zdrojem problému často dotazy s více příkazy include.</span><span class="sxs-lookup"><span data-stu-id="16dd5-938">When we hear performance questions that involve server response time problems, the source of the issue is frequently queries with multiple Include statements.</span></span> <span data-ttu-id="16dd5-939">I když jsou v dotazu i související entity výkonné, je důležité pochopit, co se děje v rámci pokrývání.</span><span class="sxs-lookup"><span data-stu-id="16dd5-939">While including related entities in a query is powerful, it's important to understand what's happening under the covers.</span></span>

<span data-ttu-id="16dd5-940">Trvá poměrně dlouhou dobu pro dotaz s více příkazy include, aby mohl projít interním kompilátorem plánu a vytvořit tak příkaz Store.</span><span class="sxs-lookup"><span data-stu-id="16dd5-940">It takes a relatively long time for a query with multiple Include statements in it to go through our internal plan compiler to produce the store command.</span></span> <span data-ttu-id="16dd5-941">Většina tohoto času stráví pokusem o optimalizaci výsledného dotazu.</span><span class="sxs-lookup"><span data-stu-id="16dd5-941">The majority of this time is spent trying to optimize the resulting query.</span></span> <span data-ttu-id="16dd5-942">Příkaz vygenerované úložiště bude obsahovat vnější spojení nebo sjednocení pro každé zahrnutí v závislosti na mapování.</span><span class="sxs-lookup"><span data-stu-id="16dd5-942">The generated store command will contain an Outer Join or Union for each Include, depending on your mapping.</span></span> <span data-ttu-id="16dd5-943">Tyto dotazy budou zahrnovat do rozsáhlých propojených grafů z databáze v jediné datové části, což bude acerbate problémy s šířkou pásma, zejména v případě, že je v datové části hodně redundance (například když se k průchodu používá více úrovní zahrnutí). asociace v směru 1: n.</span><span class="sxs-lookup"><span data-stu-id="16dd5-943">Queries like this will bring in large connected graphs from your database in a single payload, which will acerbate any bandwidth issues, especially when there is a lot of redundancy in the payload (for example, when multiple levels of Include are used to traverse associations in the one-to-many direction).</span></span>

<span data-ttu-id="16dd5-944">Můžete vyhledat případy, kdy dotazy vrací příliš velkou datovou část tím, že přistupují k podkladovým TSQLům pro dotaz pomocí ToTraceString a spustí se příkaz Store v SQL Server Management Studio pro zobrazení velikosti datové části.</span><span class="sxs-lookup"><span data-stu-id="16dd5-944">You can check for cases where your queries are returning excessively large payloads by accessing the underlying TSQL for the query by using ToTraceString and executing the store command in SQL Server Management Studio to see the payload size.</span></span> <span data-ttu-id="16dd5-945">V takových případech se můžete pokusit snížit počet příkazů include v dotazu, abyste mohli jednoduše přenést data, která potřebujete.</span><span class="sxs-lookup"><span data-stu-id="16dd5-945">In such cases you can try to reduce the number of Include statements in your query to just bring in the data you need.</span></span> <span data-ttu-id="16dd5-946">Nebo může být možné přerušit dotaz do menší posloupnosti poddotazů, například:</span><span class="sxs-lookup"><span data-stu-id="16dd5-946">Or you may be able to break your query into a smaller sequence of subqueries, for example:</span></span>

<span data-ttu-id="16dd5-947">**Před přerušením dotazu:**</span><span class="sxs-lookup"><span data-stu-id="16dd5-947">**Before breaking the query:**</span></span>

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var customers = from c in context.Customers.Include(c => c.Orders)
                    where c.LastName.StartsWith(lastNameParameter)
                    select c;

    foreach (Customer customer in customers)
    {
        ...
    }
}
```

<span data-ttu-id="16dd5-948">**Po přerušení dotazu:**</span><span class="sxs-lookup"><span data-stu-id="16dd5-948">**After breaking the query:**</span></span>

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var orders = from o in context.Orders
                 where o.Customer.LastName.StartsWith(lastNameParameter)
                 select o;

    orders.Load();

    var customers = from c in context.Customers
                    where c.LastName.StartsWith(lastNameParameter)
                    select c;

    foreach (Customer customer in customers)
    {
        ...
    }
}
```

<span data-ttu-id="16dd5-949">To bude fungovat jenom u sledovaných dotazů, protože využíváme možnosti, které musí kontext provádět rozlišení identity a opravy přidružení automaticky.</span><span class="sxs-lookup"><span data-stu-id="16dd5-949">This will work only on tracked queries, as we are making use of the ability the context has to perform identity resolution and association fixup automatically.</span></span>

<span data-ttu-id="16dd5-950">Stejně jako u opožděného načítání budou kompromisy více dotazy pro menší datové části.</span><span class="sxs-lookup"><span data-stu-id="16dd5-950">As with lazy loading, the tradeoff will be more queries for smaller payloads.</span></span> <span data-ttu-id="16dd5-951">Můžete také použít projekce jednotlivých vlastností a explicitně vybrat pouze data, která potřebujete z každé entity, ale v tomto případě nebudete v tomto případě načítat entity a aktualizace se nepodporují.</span><span class="sxs-lookup"><span data-stu-id="16dd5-951">You can also use projections of individual properties to explicitly select only the data you need from each entity, but you will not be loading entities in this case, and updates will not be supported.</span></span>

#### <a name="823-workaround-to-get-lazy-loading-of-properties"></a><span data-ttu-id="16dd5-952">8.2.3 alternativní řešení pro získání opožděného načítání vlastností</span><span class="sxs-lookup"><span data-stu-id="16dd5-952">8.2.3 Workaround to get lazy loading of properties</span></span>

<span data-ttu-id="16dd5-953">Entity Framework aktuálně nepodporuje opožděné načítání skalárních nebo složitých vlastností.</span><span class="sxs-lookup"><span data-stu-id="16dd5-953">Entity Framework currently doesn’t support lazy loading of scalar or complex properties.</span></span> <span data-ttu-id="16dd5-954">V případech, kdy máte tabulku, která obsahuje velký objekt, jako je například objekt BLOB, můžete použít rozdělení tabulky k oddělení velkých vlastností do samostatné entity.</span><span class="sxs-lookup"><span data-stu-id="16dd5-954">However, in cases where you have a table that includes a large object such as a BLOB, you can use table splitting to separate the large properties into a separate entity.</span></span> <span data-ttu-id="16dd5-955">Předpokládejme například, že máte tabulku produktů, která obsahuje sloupec fotografie varbinary.</span><span class="sxs-lookup"><span data-stu-id="16dd5-955">For example, suppose you have a Product table that includes a varbinary photo column.</span></span> <span data-ttu-id="16dd5-956">Pokud nepotřebujete k této vlastnosti v dotazech často přístup, můžete použít rozdělování tabulky a přenést do nich pouze části entity, které běžně potřebujete.</span><span class="sxs-lookup"><span data-stu-id="16dd5-956">If you don't frequently need to access this property in your queries, you can use table splitting to bring in only the parts of the entity that you normally need.</span></span> <span data-ttu-id="16dd5-957">Entita představující fotografii produktu bude načtena pouze v případě, že ji výslovně budete potřebovat.</span><span class="sxs-lookup"><span data-stu-id="16dd5-957">The entity representing the product photo will only be loaded when you explicitly need it.</span></span>

<span data-ttu-id="16dd5-958">Dobrý prostředek, který ukazuje, jak povolit rozdělení tabulky je Gil Fink "Tabulky rozdělení v Entity Framework" blogový příspěvek: \<http://blogs.microsoft.co.il/blogs/gilf/archive/2009/10/13/table-splitting-in-entity-framework.aspx>.</span><span class="sxs-lookup"><span data-stu-id="16dd5-958">A good resource that shows how to enable table splitting is Gil Fink's "Table Splitting in Entity Framework" blog post: \<http://blogs.microsoft.co.il/blogs/gilf/archive/2009/10/13/table-splitting-in-entity-framework.aspx>.</span></span>

## <a name="9-other-considerations"></a><span data-ttu-id="16dd5-959">9 dalších otázek</span><span class="sxs-lookup"><span data-stu-id="16dd5-959">9 Other considerations</span></span>

### <a name="91-server-garbage-collection"></a><span data-ttu-id="16dd5-960">Uvolňování paměti serveru 9,1</span><span class="sxs-lookup"><span data-stu-id="16dd5-960">9.1      Server Garbage Collection</span></span>

<span data-ttu-id="16dd5-961">Někteří uživatelé můžou zaznamenat spory o prostředky, které omezují paralelismuy, které očekává, pokud není systém uvolňování paměti správně nakonfigurovaný.</span><span class="sxs-lookup"><span data-stu-id="16dd5-961">Some users might experience resource contention that limits the parallelism they are expecting when the Garbage Collector is not properly configured.</span></span> <span data-ttu-id="16dd5-962">Kdykoli EF použijete ve scénáři s více vlákny nebo v jakékoli aplikaci, která se podobá systému na straně serveru, ujistěte se, že je povoleno shromažďování paměti serveru.</span><span class="sxs-lookup"><span data-stu-id="16dd5-962">Whenever EF is used in a multithreaded scenario, or in any application that resembles a server-side system, make sure to enable Server Garbage Collection.</span></span> <span data-ttu-id="16dd5-963">To se provádí prostřednictvím jednoduchého nastavení v konfiguračním souboru aplikace:</span><span class="sxs-lookup"><span data-stu-id="16dd5-963">This is done via a simple setting in your application config file:</span></span>

``` xml
<?xmlversion="1.0" encoding="utf-8" ?>
<configuration>
        <runtime>
               <gcServer enabled="true" />
        </runtime>
</configuration>
```

<span data-ttu-id="16dd5-964">To by mělo snížit spor vlákna a zvýšit vaši propustnost o až 30% v případě nasycených scénářů procesoru.</span><span class="sxs-lookup"><span data-stu-id="16dd5-964">This should decrease your thread contention and increase your throughput by up to 30% in CPU saturated scenarios.</span></span> <span data-ttu-id="16dd5-965">Obecně platí, že byste měli vždy testovat, jak se vaše aplikace chová, pomocí klasického uvolňování paměti (což je lépe vyladěno pro scénáře uživatelského rozhraní a na straně klienta) a také pro uvolňování paměti serveru.</span><span class="sxs-lookup"><span data-stu-id="16dd5-965">In general terms, you should always test how your application behaves using the classic Garbage Collection (which is better tuned for UI and client side scenarios) as well as the Server Garbage Collection.</span></span>

### <a name="92-autodetectchanges"></a><span data-ttu-id="16dd5-966">9,2 AutoDetectChanges</span><span class="sxs-lookup"><span data-stu-id="16dd5-966">9.2      AutoDetectChanges</span></span>

<span data-ttu-id="16dd5-967">Jak bylo zmíněno dříve, Entity Framework může zobrazit problémy s výkonem, pokud má mezipaměť objektů mnoho entit.</span><span class="sxs-lookup"><span data-stu-id="16dd5-967">As mentioned earlier, Entity Framework might show performance issues when the object cache has many entities.</span></span> <span data-ttu-id="16dd5-968">Některé operace, například přidat, odebrat, najít, vstup a SaveChanges, spouštějí volání DetectChanges, které mohou spotřebovat velké množství CPU na základě toho, jak velká je mezipaměť objektů.</span><span class="sxs-lookup"><span data-stu-id="16dd5-968">Certain operations, such as Add, Remove, Find, Entry and SaveChanges, trigger calls to DetectChanges which might consume a large amount of CPU based on how large the object cache has become.</span></span> <span data-ttu-id="16dd5-969">Důvodem je, že mezipaměť objektů a správce stavu objektu se pokusí zůstat v rámci každé operace provedené v kontextu jako synchronizované, aby byla vytvořená data zaručena správná v rámci nejrůznějších scénářů.</span><span class="sxs-lookup"><span data-stu-id="16dd5-969">The reason for this is that the object cache and the object state manager try to stay as synchronized as possible on each operation performed to a context so that the produced data is guaranteed to be correct under a wide array of scenarios.</span></span>

<span data-ttu-id="16dd5-970">Obecně platí, že pro celou dobu života vaší aplikace je povoleno automatické zjišťování změn Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="16dd5-970">It is generally a good practice to leave Entity Framework’s automatic change detection enabled for the entire life of your application.</span></span> <span data-ttu-id="16dd5-971">Pokud je váš scénář negativně ovlivněn vysokým využitím procesoru a vaše profily označují, že příčinou je volání DetectChanges, zvažte dočasné vypnutí AutoDetectChanges v citlivé části kódu:</span><span class="sxs-lookup"><span data-stu-id="16dd5-971">If your scenario is being negatively affected by high CPU usage and your profiles indicate that the culprit is the call to DetectChanges, consider temporarily turning off AutoDetectChanges in the sensitive portion of your code:</span></span>

``` csharp
try
{
    context.Configuration.AutoDetectChangesEnabled = false;
    var product = context.Products.Find(productId);
    ...
}
finally
{
    context.Configuration.AutoDetectChangesEnabled = true;
}
```

<span data-ttu-id="16dd5-972">Před vypnutím AutoDetectChanges je dobré pochopit, že to může způsobit, že Entity Framework ztratí schopnost sledovat určité informace o změnách, které se na těchto entitách konají.</span><span class="sxs-lookup"><span data-stu-id="16dd5-972">Before turning off AutoDetectChanges, it’s good to understand that this might cause Entity Framework to lose its ability to track certain information about the changes that are taking place on the entities.</span></span> <span data-ttu-id="16dd5-973">Pokud je zpracování chybné, může to způsobit nekonzistenci dat ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="16dd5-973">If handled incorrectly, this might cause data inconsistency on your application.</span></span> <span data-ttu-id="16dd5-974">Další informace o vypnutí AutoDetectChanges najdete v článku \<http://blog.oneunicorn.com/2012/03/12/secrets-of-detectchanges-part-3-switching-off-automatic-detectchanges/>.</span><span class="sxs-lookup"><span data-stu-id="16dd5-974">For more information on turning off AutoDetectChanges, read \<http://blog.oneunicorn.com/2012/03/12/secrets-of-detectchanges-part-3-switching-off-automatic-detectchanges/>.</span></span>

### <a name="93-context-per-request"></a><span data-ttu-id="16dd5-975">kontext 9,3 na požadavek</span><span class="sxs-lookup"><span data-stu-id="16dd5-975">9.3      Context per request</span></span>

<span data-ttu-id="16dd5-976">Kontexty Entity Framework jsou určeny pro použití jako krátkodobé instance, aby bylo možné zajistit optimální prostředí výkonu.</span><span class="sxs-lookup"><span data-stu-id="16dd5-976">Entity Framework’s contexts are meant to be used as short-lived instances in order to provide the most optimal performance experience.</span></span> <span data-ttu-id="16dd5-977">Očekává se, že kontexty by měly být krátké a zahozené a jako takové implementace jsou velmi odlehčené a znovu využívat metadata, kdykoli to bude možné.</span><span class="sxs-lookup"><span data-stu-id="16dd5-977">Contexts are expected to be short lived and discarded, and as such have been implemented to be very lightweight and reutilize metadata whenever possible.</span></span> <span data-ttu-id="16dd5-978">Ve webových scénářích je důležité mít na paměti, že je to v úmyslu, a nemají kontext po dobu delší než jeden požadavek.</span><span class="sxs-lookup"><span data-stu-id="16dd5-978">In web scenarios it’s important to keep this in mind and not have a context for more than the duration of a single request.</span></span> <span data-ttu-id="16dd5-979">Podobně ve scénářích, které nejsou webové, by měl být kontext zahozen na základě vašeho porozumění různých úrovní ukládání do mezipaměti v Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="16dd5-979">Similarly, in non-web scenarios, context should be discarded based on your understanding of the different levels of caching in the Entity Framework.</span></span> <span data-ttu-id="16dd5-980">Obecně řečeno, jedna by neměla mít instanci kontextu v průběhu životního cyklu aplikace a také kontexty na vlákno a statické kontexty.</span><span class="sxs-lookup"><span data-stu-id="16dd5-980">Generally speaking, one should avoid having a context instance throughout the life of the application, as well as contexts per thread and static contexts.</span></span>

### <a name="94-database-null-semantics"></a><span data-ttu-id="16dd5-981">9,4 sémantika hodnoty null databáze</span><span class="sxs-lookup"><span data-stu-id="16dd5-981">9.4      Database null semantics</span></span>

<span data-ttu-id="16dd5-982">Entity Framework ve výchozím nastavení vygeneruje kód SQL, který má sémantiku porovnávání s hodnotou C @ no__t-0.</span><span class="sxs-lookup"><span data-stu-id="16dd5-982">Entity Framework by default will generate SQL code that has C\# null comparison semantics.</span></span> <span data-ttu-id="16dd5-983">Vezměte v úvahu následující příklad dotazu:</span><span class="sxs-lookup"><span data-stu-id="16dd5-983">Consider the following example query:</span></span>

``` csharp
            int? categoryId = 7;
            int? supplierId = 8;
            decimal? unitPrice = 0;
            short? unitsInStock = 100;
            short? unitsOnOrder = 20;
            short? reorderLevel = null;

            var q = from p incontext.Products
                    wherep.Category.CategoryName == "Beverages"
                          || (p.CategoryID == categoryId
                                || p.SupplierID == supplierId
                                || p.UnitPrice == unitPrice
                                || p.UnitsInStock == unitsInStock
                                || p.UnitsOnOrder == unitsOnOrder
                                || p.ReorderLevel == reorderLevel)
                    select p;

            var r = q.ToList();
```

<span data-ttu-id="16dd5-984">V tomto příkladu porovnáváme počet proměnných s možnou hodnotou null v entitě, jako je ČísloDodavatele a JednotkováCena.</span><span class="sxs-lookup"><span data-stu-id="16dd5-984">In this example, we’re comparing a number of nullable variables against nullable properties on the entity, such as SupplierID and UnitPrice.</span></span> <span data-ttu-id="16dd5-985">Vygenerovaný SQL pro tento dotaz zobrazí dotaz, zda je hodnota parametru shodná s hodnotou sloupce, nebo pokud jsou parametry i hodnoty sloupce NULL.</span><span class="sxs-lookup"><span data-stu-id="16dd5-985">The generated SQL for this query will ask if the parameter value is the same as the column value, or if both the parameter and the column values are null.</span></span> <span data-ttu-id="16dd5-986">Tím se skryje způsob, jakým databázový server zpracovává hodnoty null, a poskytne konzistentní prostředí s hodnotou null v jazyce C @ no__t v různých dodavatelích databáze.</span><span class="sxs-lookup"><span data-stu-id="16dd5-986">This will hide the way the database server handles nulls and will provide a consistent C\# null experience across different database vendors.</span></span> <span data-ttu-id="16dd5-987">Na druhé straně generovaný kód je trochu konvoluce a nemusí být vhodný, pokud je míra porovnávání v příkazu WHERE dotazu vyšší než na velké číslo.</span><span class="sxs-lookup"><span data-stu-id="16dd5-987">On the other hand, the generated code is a bit convoluted and may not perform well when the amount of comparisons in the where statement of the query grows to a large number.</span></span>

<span data-ttu-id="16dd5-988">Jedním ze způsobů, jak řešit tuto situaci, je použití sémantiky s hodnotou null databáze.</span><span class="sxs-lookup"><span data-stu-id="16dd5-988">One way to deal with this situation is by using database null semantics.</span></span> <span data-ttu-id="16dd5-989">Všimněte si, že se to může potenciálně chovat jinak než sémantika null v jazyce C @ no__t-0, protože nyní Entity Framework vygeneruje jednodušší SQL, který zveřejňuje způsob, jakým databázový stroj zpracovává hodnoty null.</span><span class="sxs-lookup"><span data-stu-id="16dd5-989">Note that this might potentially behave differently to the C\# null semantics since now Entity Framework will generate simpler SQL that exposes the way the database engine handles null values.</span></span> <span data-ttu-id="16dd5-990">Sémantika s hodnotou null databáze může být aktivována pro každý kontext s jedním řádkem konfigurace s konfigurací kontextu:</span><span class="sxs-lookup"><span data-stu-id="16dd5-990">Database null semantics can be activated per-context with one single configuration line against the context configuration:</span></span>

``` csharp
                context.Configuration.UseDatabaseNullSemantics = true;
```

<span data-ttu-id="16dd5-991">Dotazy s malým až středním velikostí nebudou při použití sémantiky s hodnotou null databáze zobrazovat pozorně, ale rozdíl se projeví v dotazech s velkým počtem potenciálních porovnání s hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="16dd5-991">Small to medium sized queries will not display a perceptible performance improvement when using database null semantics, but the difference will become more noticeable on queries with a large number of potential null comparisons.</span></span>

<span data-ttu-id="16dd5-992">V příkladu dotazu výše byl rozdíl výkonu menší než 2% v mikrotestu běžícím v kontrolovaném prostředí.</span><span class="sxs-lookup"><span data-stu-id="16dd5-992">In the example query above, the performance difference was less than 2% in a microbenchmark running in a controlled environment.</span></span>

### <a name="95-async"></a><span data-ttu-id="16dd5-993">9,5 Async</span><span class="sxs-lookup"><span data-stu-id="16dd5-993">9.5      Async</span></span>

<span data-ttu-id="16dd5-994">Entity Framework 6 zavádí podporu asynchronních operací při spuštění v rozhraní .NET 4,5 nebo novějším.</span><span class="sxs-lookup"><span data-stu-id="16dd5-994">Entity Framework 6 introduced support of async operations when running on .NET 4.5 or later.</span></span> <span data-ttu-id="16dd5-995">U aplikací, u kterých se v/v v/v nepoužívá spor, bude výhodná použití asynchronních operací dotazů a ukládání.</span><span class="sxs-lookup"><span data-stu-id="16dd5-995">For the most part, applications that have IO related contention will benefit the most from using asynchronous query and save operations.</span></span> <span data-ttu-id="16dd5-996">Pokud vaše aplikace neutrpěla kolize vstupu/výstupu, použití Async bude v optimálních případech spouštěno synchronně a vracet výsledek za stejné množství jako synchronní volání, nebo v nejhorším případě jednoduše odložit provádění na asynchronní úlohu a přidat další Tim e pro dokončení vašeho scénáře.</span><span class="sxs-lookup"><span data-stu-id="16dd5-996">If your application does not suffer from IO contention, the use of async will, in the best cases, run synchronously and return the result in the same amount of time as a synchronous call, or in the worst case, simply defer execution to an asynchronous task and add extra time to the completion of your scenario.</span></span>

<span data-ttu-id="16dd5-997">Informace o tom, jak asynchronní programovací práce, který vám pomůže rozhodování o tom, pokud asynchronní zlepší výkon vaší aplikace navštívíte [http://msdn.microsoft.com/library/hh191443.aspx](https://msdn.microsoft.com/library/hh191443.aspx).</span><span class="sxs-lookup"><span data-stu-id="16dd5-997">For information on how asynchronous programming work that will help you deciding if async will improve the performance of your application visit [http://msdn.microsoft.com/library/hh191443.aspx](https://msdn.microsoft.com/library/hh191443.aspx).</span></span> <span data-ttu-id="16dd5-998">Další informace o použití asynchronních operací na Entity Framework naleznete v tématu [Async Query a Save @ no__t-1.</span><span class="sxs-lookup"><span data-stu-id="16dd5-998">For more information on the use of async operations on Entity Framework, see [Async Query and Save](~/ef6/fundamentals/async.md
).</span></span>

### <a name="96-ngen"></a><span data-ttu-id="16dd5-999">9,6 NGEN</span><span class="sxs-lookup"><span data-stu-id="16dd5-999">9.6      NGEN</span></span>

<span data-ttu-id="16dd5-1000">Entity Framework 6 není součástí výchozí instalace rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1000">Entity Framework 6 does not come in the default installation of .NET framework.</span></span> <span data-ttu-id="16dd5-1001">V takovém případě Entity Framework sestavení nejsou ve výchozím nastavení NGEN, což znamená, že veškerý Entity Framework kód podléhá stejným JIT'ingým nákladům jako jakékoli jiné sestavení MSIL.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1001">As such, the Entity Framework assemblies are not NGEN’d by default which means that all the Entity Framework code is subject to the same JIT’ing costs as any other MSIL assembly.</span></span> <span data-ttu-id="16dd5-1002">To může v produkčním prostředí snížit prostředí F5 při vývoji a také studeném spuštění vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1002">This might degrade the F5 experience while developing and also the cold startup of your application in the production environments.</span></span> <span data-ttu-id="16dd5-1003">Aby se snížily náklady na procesor a paměť JIT'ing, doporučuje se, abyste v případě potřeby naEntity Framework image NGEN.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1003">In order to reduce the CPU and memory costs of JIT’ing it is advisable to NGEN the Entity Framework images as appropriate.</span></span> <span data-ttu-id="16dd5-1004">Další informace o tom, jak zlepšit výkon při spuštění Entity Framework 6 s NGEN, najdete v tématu [zlepšení výkonu při spouštění pomocí Ngen](~/ef6/fundamentals/performance/ngen.md).</span><span class="sxs-lookup"><span data-stu-id="16dd5-1004">For more information on how to improve the startup performance of Entity Framework 6 with NGEN, see [Improving Startup Performance with NGen](~/ef6/fundamentals/performance/ngen.md).</span></span>

### <a name="97-code-first-versus-edmx"></a><span data-ttu-id="16dd5-1005">9,7 Code First oproti EDMX</span><span class="sxs-lookup"><span data-stu-id="16dd5-1005">9.7      Code First versus EDMX</span></span>

<span data-ttu-id="16dd5-1006">Entity Framework důvody týkající se problému neshody mezi objektově orientovaným programováním a relačními databázemi pomocí reprezentace v paměti koncepčního modelu (objektů), schématu úložiště (databáze) a mapování mezi Druhá.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1006">Entity Framework reasons about the impedance mismatch problem between object oriented programming and relational databases by having an in-memory representation of the conceptual model (the objects), the storage schema (the database) and a mapping between the two.</span></span> <span data-ttu-id="16dd5-1007">Tato metadata se nazývají model EDM (Entity Data Model) nebo EDM pro krátké.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1007">This metadata is called an Entity Data Model, or EDM for short.</span></span> <span data-ttu-id="16dd5-1008">Z tohoto modelu EDM Entity Framework odvozují zobrazení k převodu dat z objektů v paměti do databáze a zpět.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1008">From this EDM, Entity Framework will derive the views to roundtrip data from the objects in memory to the database and back.</span></span>

<span data-ttu-id="16dd5-1009">Když se používá Entity Framework se souborem EDMX, který formálně určuje koncepční model, schéma úložiště a mapování, pak fáze načítání modelu musí ověřit, jestli je EDM správný (například se ujistěte, že žádné mapování chybí), pak Vygenerujte zobrazení a pak ověřte zobrazení a nechte tato metadata připravená k použití.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1009">When Entity Framework is used with an EDMX file that formally specifies the conceptual model, the storage schema, and the mapping, then the model loading stage only has to validate that the EDM is correct (for example, make sure that no mappings are missing), then generate the views, then validate the views and have this metadata ready for use.</span></span> <span data-ttu-id="16dd5-1010">Pouze potom lze spustit dotaz nebo nová data uložit do úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1010">Only then can a query be executed or new data be saved to the data store.</span></span>

<span data-ttu-id="16dd5-1011">Přístup k Code First je, ve svém srdce, propracovaného generátoru model EDM (Entity Data Model).</span><span class="sxs-lookup"><span data-stu-id="16dd5-1011">The Code First approach is, at its heart, a sophisticated Entity Data Model generator.</span></span> <span data-ttu-id="16dd5-1012">Entity Framework musí z poskytnutého kódu vydávat EDM; provede tak analýzu tříd zapojených do modelu, použití konvencí a konfigurace modelu prostřednictvím rozhraní Fluent API.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1012">The Entity Framework has to produce an EDM from the provided code; it does so by analyzing the classes involved in the model, applying conventions and configuring the model via the Fluent API.</span></span> <span data-ttu-id="16dd5-1013">Po sestavení modelu EDM se Entity Framework v podstatě chová stejným způsobem, jako by byl v projektu přítomen soubor EDMX.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1013">After the EDM is built, the Entity Framework essentially behaves the same way as it would had an EDMX file been present in the project.</span></span> <span data-ttu-id="16dd5-1014">Proto sestavíte model z Code First zvyšuje složitost, která překládá do pomalejšího času spuštění pro Entity Framework ve srovnání s podmnožinou EDMX.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1014">Thus, building the model from Code First adds extra complexity that translates into a slower startup time for the Entity Framework when compared to having an EDMX.</span></span> <span data-ttu-id="16dd5-1015">Náklady jsou zcela závislé na velikosti a složitosti modelu, který je sestaven.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1015">The cost is completely dependent on the size and complexity of the model that’s being built.</span></span>

<span data-ttu-id="16dd5-1016">Když zvolíte použití EDMX a Code First, je důležité znát, že flexibilita zavedená Code First zvyšuje náklady na sestavování modelu poprvé.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1016">When choosing to use EDMX versus Code First, it’s important to know that the flexibility introduced by Code First increases the cost of building the model for the first time.</span></span> <span data-ttu-id="16dd5-1017">Pokud vaše aplikace může vydržet náklady na toto první zatížení, obvykle Code First bude upřednostňovaným způsobem, jak jít.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1017">If your application can withstand the cost of this first-time load then typically Code First will be the preferred way to go.</span></span>

## <a name="10-investigating-performance"></a><span data-ttu-id="16dd5-1018">10 prošetření výkonu</span><span class="sxs-lookup"><span data-stu-id="16dd5-1018">10 Investigating Performance</span></span>

### <a name="101-using-the-visual-studio-profiler"></a><span data-ttu-id="16dd5-1019">10,1 použití profileru sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="16dd5-1019">10.1 Using the Visual Studio Profiler</span></span>

<span data-ttu-id="16dd5-1020">Pokud máte problémy s výkonem Entity Framework, můžete použít Profiler, jako je ten, který je součástí sady Visual Studio, a zjistit, kde aplikace tráví svůj čas.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1020">If you are having performance issues with the Entity Framework, you can use a profiler like the one built into Visual Studio to see where your application is spending its time.</span></span> <span data-ttu-id="16dd5-1021">Toto je nástroj, který jsme použili k vygenerování výsečové grafy v blogovém příspěvku "Zkoumání výkonu technologie ADO.NET Entity Framework – část 1" ( \<http://blogs.msdn.com/b/adonet/archive/2008/02/04/exploring-the-performance-of-the-ado-net-entity-framework-part-1.aspx>) , které uvádí, kde Entity Framework stráví času během studené a horké dotazy.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1021">This is the tool we used to generate the pie charts in the “Exploring the Performance of the ADO.NET Entity Framework - Part 1” blog post ( \<http://blogs.msdn.com/b/adonet/archive/2008/02/04/exploring-the-performance-of-the-ado-net-entity-framework-part-1.aspx>) that show where Entity Framework spends its time during cold and warm queries.</span></span>

<span data-ttu-id="16dd5-1022">"Profilování Entity Framework pomocí programu Visual Studio 2010 profiler" na blogu, který je popsán v článku poradní tým pro data a modelování, zobrazuje reálný příklad použití profileru k prozkoumání problému s výkonem.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1022">The "Profiling Entity Framework using the Visual Studio 2010 Profiler" blog post written by the Data and Modeling Customer Advisory Team shows a real-world example of how they used the profiler to investigate a performance problem.</span></span><span data-ttu-id="16dd5-1023">  \<http://blogs.msdn.com/b/dmcat/archive/2010/04/30/profiling-entity-framework-using-the-visual-studio-2010-profiler.aspx>.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1023">  \<http://blogs.msdn.com/b/dmcat/archive/2010/04/30/profiling-entity-framework-using-the-visual-studio-2010-profiler.aspx>.</span></span> <span data-ttu-id="16dd5-1024">Tento příspěvek byl napsaný pro aplikaci pro Windows.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1024">This post was written for a windows application.</span></span> <span data-ttu-id="16dd5-1025">Pokud potřebujete profilovat webovou aplikaci, nástroje Windows Performance Record (WPR) a Windows Performance Analyzer (WPA) můžou pracovat lépe než při práci ze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1025">If you need to profile a web application the Windows Performance Recorder (WPR) and Windows Performance Analyzer (WPA) tools may work better than working from Visual Studio.</span></span> <span data-ttu-id="16dd5-1026">Požadavku webové části a WPA jsou součástí Windows Performance Toolkit, který je součástí sady Windows Assessment and Deployment Kit ( [http://www.microsoft.com/download/details.aspx?id=39982](https://www.microsoft.com/download/details.aspx?id=39982)).</span><span class="sxs-lookup"><span data-stu-id="16dd5-1026">WPR and WPA are part of the Windows Performance Toolkit which is included with the Windows Assessment and Deployment Kit ( [http://www.microsoft.com/download/details.aspx?id=39982](https://www.microsoft.com/download/details.aspx?id=39982)).</span></span>

### <a name="102-applicationdatabase-profiling"></a><span data-ttu-id="16dd5-1027">Profilace aplikace/databáze 10,2</span><span class="sxs-lookup"><span data-stu-id="16dd5-1027">10.2 Application/Database profiling</span></span>

<span data-ttu-id="16dd5-1028">Nástroje, jako je profiler integrovaný do sady Visual Studio, vás informují o tom, kde vaše aplikace stráví čas.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1028">Tools like the profiler built into Visual Studio tell you where your application is spending time.</span></span><span data-ttu-id="16dd5-1029">  K dispozici je jiný typ profileru, který provádí dynamickou analýzu běžící aplikace v produkčním prostředí nebo v předprodukčním prostředí v závislosti na potřebách a hledá běžné nástrah a anti-vzory přístupu k databázi.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1029">  Another type of profiler is available that performs dynamic analysis of your running application, either in production or pre-production depending on needs, and looks for common pitfalls and anti-patterns of database access.</span></span>

<span data-ttu-id="16dd5-1030">Jsou dva komerčně dostupný profilery Profiler Entity Framework ( \<http://efprof.com>) a ORMProfiler ( \<http://ormprofiler.com>).</span><span class="sxs-lookup"><span data-stu-id="16dd5-1030">Two commercially available profilers are the Entity Framework Profiler ( \<http://efprof.com>) and ORMProfiler ( \<http://ormprofiler.com>).</span></span>

<span data-ttu-id="16dd5-1031">Pokud je vaše aplikace MVC aplikací pomocí Code First, můžete použít MiniProfileru StackExchange.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1031">If your application is an MVC application using Code First, you can use StackExchange's MiniProfiler.</span></span> <span data-ttu-id="16dd5-1032">Scott Hanselman popisuje tento nástroj na svém blogu na: \<http://www.hanselman.com/blog/NuGetPackageOfTheWeek9ASPNETMiniProfilerFromStackExchangeRocksYourWorld.aspx>.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1032">Scott Hanselman describes this tool in his blog at: \<http://www.hanselman.com/blog/NuGetPackageOfTheWeek9ASPNETMiniProfilerFromStackExchangeRocksYourWorld.aspx>.</span></span>

<span data-ttu-id="16dd5-1033">Další informace o profilování aktivity databáze vaší aplikace najdete v článku o katalogu MSDN Julie Lerman [s názvem aktivita databáze profilace v Entity Framework](https://msdn.microsoft.com/magazine/gg490349.aspx).</span><span class="sxs-lookup"><span data-stu-id="16dd5-1033">For more information on profiling your application's database activity, see Julie Lerman's MSDN Magazine article titled [Profiling Database Activity in the Entity Framework](https://msdn.microsoft.com/magazine/gg490349.aspx).</span></span>

### <a name="103-database-logger"></a><span data-ttu-id="16dd5-1034">protokolovací nástroj databáze 10,3</span><span class="sxs-lookup"><span data-stu-id="16dd5-1034">10.3 Database logger</span></span>

<span data-ttu-id="16dd5-1035">Pokud používáte Entity Framework 6, zvažte také použití integrované funkce protokolování.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1035">If you are using Entity Framework 6 also consider using the built-in logging functionality.</span></span> <span data-ttu-id="16dd5-1036">Vlastnost databáze kontextu může být pokyn k protokolování své aktivity prostřednictvím jednoduché konfigurace na jednom řádku:</span><span class="sxs-lookup"><span data-stu-id="16dd5-1036">The Database property of the context can be instructed to log its activity via a simple one-line configuration:</span></span>

``` csharp
    using (var context = newQueryComparison.DbC.NorthwindEntities())
    {
        context.Database.Log = Console.WriteLine;
        var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
        q.ToList();
    }
```

<span data-ttu-id="16dd5-1037">V tomto příkladu se databázová aktivita bude protokolovat do konzoly, ale vlastnost log se dá nakonfigurovat tak, aby volala jakýkoli delegát akce @ no__t-0string @ no__t-1.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1037">In this example the database activity will be logged to the console, but the Log property can be configured to call any Action&lt;string&gt; delegate.</span></span>

<span data-ttu-id="16dd5-1038">Pokud chcete povolit protokolování databáze bez opětovné kompilace a používáte Entity Framework 6,1 nebo novější, můžete tak učinit přidáním zachytávací do souboru Web. config nebo App. config aplikace.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1038">If you want to enable database logging without recompiling, and you are using Entity Framework 6.1 or later, you can do so by adding an interceptor in the web.config or app.config file of your application.</span></span>

``` xml
  <interceptors>
    <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
      <parameters>
        <parameter value="C:\Path\To\My\LogOutput.txt"/>
      </parameters>
    </interceptor>
  </interceptors>
```

<span data-ttu-id="16dd5-1039">Další informace o tom, jak přidat protokolování bez opětovné kompilace přejít na \<http://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/>.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1039">For more information on how to add logging without recompiling go to \<http://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/>.</span></span>

## <a name="11-appendix"></a><span data-ttu-id="16dd5-1040">11. Dodatek</span><span class="sxs-lookup"><span data-stu-id="16dd5-1040">11 Appendix</span></span>

### <a name="111-a-test-environment"></a><span data-ttu-id="16dd5-1041">11,1. testovací prostředí</span><span class="sxs-lookup"><span data-stu-id="16dd5-1041">11.1 A. Test Environment</span></span>

<span data-ttu-id="16dd5-1042">Toto prostředí používá instalaci dvou počítačů s databází na samostatném počítači z klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1042">This environment uses a 2-machine setup with the database on a separate machine from the client application.</span></span> <span data-ttu-id="16dd5-1043">Počítače jsou ve stejném stojanu, takže latence sítě je poměrně nízká, ale je realističtější než prostředí s jedním počítačem.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1043">Machines are in the same rack, so network latency is relatively low, but more realistic than a single-machine environment.</span></span>

#### <a name="1111-app-server"></a><span data-ttu-id="16dd5-1044">Server aplikace 11.1.1</span><span class="sxs-lookup"><span data-stu-id="16dd5-1044">11.1.1       App Server</span></span>

##### <a name="11111-software-environment"></a><span data-ttu-id="16dd5-1045">11.1.1.1 softwarové prostředí</span><span class="sxs-lookup"><span data-stu-id="16dd5-1045">11.1.1.1      Software Environment</span></span>

-   <span data-ttu-id="16dd5-1046">Entity Framework 4 softwarové prostředí</span><span class="sxs-lookup"><span data-stu-id="16dd5-1046">Entity Framework 4 Software Environment</span></span>
    -   <span data-ttu-id="16dd5-1047">Název operačního systému: Windows Server 2008 R2 Enterprise SP1.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1047">OS Name: Windows Server 2008 R2 Enterprise SP1.</span></span>
    -   <span data-ttu-id="16dd5-1048">Visual Studio 2010 – Ultimate</span><span class="sxs-lookup"><span data-stu-id="16dd5-1048">Visual Studio 2010 – Ultimate.</span></span>
    -   <span data-ttu-id="16dd5-1049">Visual Studio 2010 SP1 (pouze pro některá porovnání).</span><span class="sxs-lookup"><span data-stu-id="16dd5-1049">Visual Studio 2010 SP1 (only for some comparisons).</span></span>
-   <span data-ttu-id="16dd5-1050">Entity Framework 5 a 6 softwarového prostředí</span><span class="sxs-lookup"><span data-stu-id="16dd5-1050">Entity Framework 5 and 6 Software Environment</span></span>
    -   <span data-ttu-id="16dd5-1051">Název operačního systému: Windows 8.1 Enterprise</span><span class="sxs-lookup"><span data-stu-id="16dd5-1051">OS Name: Windows 8.1 Enterprise</span></span>
    -   <span data-ttu-id="16dd5-1052">Visual Studio 2013 – Ultimate</span><span class="sxs-lookup"><span data-stu-id="16dd5-1052">Visual Studio 2013 – Ultimate.</span></span>

##### <a name="11112-hardware-environment"></a><span data-ttu-id="16dd5-1053">11.1.1.2 hardwarové prostředí</span><span class="sxs-lookup"><span data-stu-id="16dd5-1053">11.1.1.2      Hardware Environment</span></span>

-   <span data-ttu-id="16dd5-1054">Duální procesor:     Intel (R) Xeon (R) CPU L5520 W3530 @ 2,27 GHz, 2261 Mhz8 GHz, 4 jádra, 84 logických procesorů:</span><span class="sxs-lookup"><span data-stu-id="16dd5-1054">Dual Processor:     Intel(R) Xeon(R) CPU L5520 W3530 @ 2.27GHz, 2261 Mhz8 GHz, 4 Core(s), 84 Logical Processor(s).</span></span>
-   <span data-ttu-id="16dd5-1055">2412 GB RamRAM.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1055">2412 GB RamRAM.</span></span>
-   <span data-ttu-id="16dd5-1056">jednotka 136 GB SCSI250GB SATA 7200 ot./min. povolenou/s se rozdělí do 4 oddílů.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1056">136 GB SCSI250GB SATA 7200 rpm 3GB/s drive split into 4 partitions.</span></span>

#### <a name="1112-db-server"></a><span data-ttu-id="16dd5-1057">Server 11.1.2 DB</span><span class="sxs-lookup"><span data-stu-id="16dd5-1057">11.1.2       DB server</span></span>

##### <a name="11121-software-environment"></a><span data-ttu-id="16dd5-1058">11.1.2.1 softwarové prostředí</span><span class="sxs-lookup"><span data-stu-id="16dd5-1058">11.1.2.1      Software Environment</span></span>

-   <span data-ttu-id="16dd5-1059">Název operačního systému: Windows Server 2008 R 28.1 Enterprise SP1.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1059">OS Name: Windows Server 2008 R28.1 Enterprise SP1.</span></span>
-   <span data-ttu-id="16dd5-1060">SQL Server 2008 R22012.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1060">SQL Server 2008 R22012.</span></span>

##### <a name="11122-hardware-environment"></a><span data-ttu-id="16dd5-1061">11.1.2.2 hardwarové prostředí</span><span class="sxs-lookup"><span data-stu-id="16dd5-1061">11.1.2.2      Hardware Environment</span></span>

-   <span data-ttu-id="16dd5-1062">Jeden procesor: Intel (R) Xeon (R) CPU L5520 @ 2,27 GHz, 2261 MhzES-1620 0 @ 3.60 GHz, 4 jádra, 8 logických procesorů:.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1062">Single Processor: Intel(R) Xeon(R) CPU L5520  @ 2.27GHz, 2261 MhzES-1620 0 @ 3.60GHz, 4 Core(s), 8 Logical Processor(s).</span></span>
-   <span data-ttu-id="16dd5-1063">824 GB RamRAM.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1063">824 GB RamRAM.</span></span>
-   <span data-ttu-id="16dd5-1064">jednotka 465 GB ATA500GB SATA 7200 ot./min. 6 GB/s se rozdělí do 4 oddílů.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1064">465 GB ATA500GB SATA 7200 rpm 6GB/s drive split into 4 partitions.</span></span>

### <a name="112-b-query-performance-comparison-tests"></a><span data-ttu-id="16dd5-1065">11,2 B. testy porovnání výkonu dotazů</span><span class="sxs-lookup"><span data-stu-id="16dd5-1065">11.2      B. Query performance comparison tests</span></span>

<span data-ttu-id="16dd5-1066">Model Northwind byl použit k provedení těchto testů.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1066">The Northwind model was used to execute these tests.</span></span> <span data-ttu-id="16dd5-1067">Byla vygenerována z databáze pomocí návrháře Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1067">It was generated from the database using the Entity Framework designer.</span></span> <span data-ttu-id="16dd5-1068">Pak následující kód byl použit pro porovnání výkonu možností spuštění dotazu:</span><span class="sxs-lookup"><span data-stu-id="16dd5-1068">Then, the following code was used to compare the performance of the query execution options:</span></span>

``` csharp
using System;
using System.Collections.Generic;
using System.Data;
using System.Data.Common;
using System.Data.Entity.Infrastructure;
using System.Data.EntityClient;
using System.Data.Objects;
using System.Linq;

namespace QueryComparison
{
    public partial class NorthwindEntities : ObjectContext
    {
        private static readonly Func<NorthwindEntities, string, IQueryable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
            (NorthwindEntities context, string categoryName) =>
                context.Products.Where(p => p.Category.CategoryName == categoryName)
                );

        public IQueryable<Product> InvokeProductsForCategoryCQ(string categoryName)
        {
            return productsForCategoryCQ(this, categoryName);
        }
    }

    public class QueryTypePerfComparison
    {
        private static string entityConnectionStr = @"metadata=res://*/Northwind.csdl|res://*/Northwind.ssdl|res://*/Northwind.msl;provider=System.Data.SqlClient;provider connection string='data source=.;initial catalog=Northwind;integrated security=True;multipleactiveresultsets=True;App=EntityFramework'";

        public void LINQIncludingContextCreation()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {                 
                var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }

        public void LINQNoTracking()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                context.Products.MergeOption = MergeOption.NoTracking;

                var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }

        public void CompiledQuery()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                var q = context.InvokeProductsForCategoryCQ("Beverages");
                q.ToList();
            }
        }

        public void ObjectQuery()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                ObjectQuery<Product> products = context.Products.Where("it.Category.CategoryName = 'Beverages'");
                products.ToList();
            }
        }

        public void EntityCommand()
        {
            using (EntityConnection eConn = new EntityConnection(entityConnectionStr))
            {
                eConn.Open();
                EntityCommand cmd = eConn.CreateCommand();
                cmd.CommandText = "Select p From NorthwindEntities.Products As p Where p.Category.CategoryName = 'Beverages'";

                using (EntityDataReader reader = cmd.ExecuteReader(CommandBehavior.SequentialAccess))
                {
                    List<Product> productsList = new List<Product>();
                    while (reader.Read())
                    {
                        DbDataRecord record = (DbDataRecord)reader.GetValue(0);

                        // 'materialize' the product by accessing each field and value. Because we are materializing products, we won't have any nested data readers or records.
                        int fieldCount = record.FieldCount;

                        // Treat all products as Product, even if they are the subtype DiscontinuedProduct.
                        Product product = new Product();  

                        product.ProductID = record.GetInt32(0);
                        product.ProductName = record.GetString(1);
                        product.SupplierID = record.GetInt32(2);
                        product.CategoryID = record.GetInt32(3);
                        product.QuantityPerUnit = record.GetString(4);
                        product.UnitPrice = record.GetDecimal(5);
                        product.UnitsInStock = record.GetInt16(6);
                        product.UnitsOnOrder = record.GetInt16(7);
                        product.ReorderLevel = record.GetInt16(8);
                        product.Discontinued = record.GetBoolean(9);

                        productsList.Add(product);
                    }
                }
            }
        }

        public void ExecuteStoreQuery()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                ObjectResult<Product> beverages = context.ExecuteStoreQuery<Product>(
@"    SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued
    FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
    WHERE        (C.CategoryName = 'Beverages')"
);
                beverages.ToList();
            }
        }

        public void ExecuteStoreQueryDbContext()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {
                var beverages = context.Database.SqlQuery\<QueryComparison.DbC.Product>(
@"    SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued
    FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
    WHERE        (C.CategoryName = 'Beverages')"
);
                beverages.ToList();
            }
        }

        public void ExecuteStoreQueryDbSet()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {
                var beverages = context.Products.SqlQuery(
@"    SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued
    FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
    WHERE        (C.CategoryName = 'Beverages')"
);
                beverages.ToList();
            }
        }

        public void LINQIncludingContextCreationDbContext()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {                 
                var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }

        public void LINQNoTrackingDbContext()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {
                var q = context.Products.AsNoTracking().Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }
    }
}
```

### <a name="113-c-navision-model"></a><span data-ttu-id="16dd5-1069">11,3 C. model Navision</span><span class="sxs-lookup"><span data-stu-id="16dd5-1069">11.3 C. Navision Model</span></span>

<span data-ttu-id="16dd5-1070">Databáze Navision je velká databáze, která se používá k ukázce Microsoft Dynamics – NAV.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1070">The Navision database is a large database used to demo Microsoft Dynamics – NAV.</span></span> <span data-ttu-id="16dd5-1071">Vygenerovaný koncepční model obsahuje sady entit 1005 a sady přidružení 4227.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1071">The generated conceptual model contains 1005 entity sets and 4227 association sets.</span></span> <span data-ttu-id="16dd5-1072">Model použitý v testu je "plochá" – do něj nebyla přidána žádná dědičnost.</span><span class="sxs-lookup"><span data-stu-id="16dd5-1072">The model used in the test is “flat” – no inheritance has been added to it.</span></span>

#### <a name="1131-queries-used-for-navision-tests"></a><span data-ttu-id="16dd5-1073">dotazy 11.3.1 použité pro testy v Navision</span><span class="sxs-lookup"><span data-stu-id="16dd5-1073">11.3.1 Queries used for Navision tests</span></span>

<span data-ttu-id="16dd5-1074">Seznam dotazy, který se používá s modelem Navision, obsahuje 3 kategorie dotazů Entity SQL:</span><span class="sxs-lookup"><span data-stu-id="16dd5-1074">The queries list used with the Navision model contains 3 categories of Entity SQL queries:</span></span>

##### <a name="11311-lookup"></a><span data-ttu-id="16dd5-1075">11.3.1.1 vyhledávání</span><span class="sxs-lookup"><span data-stu-id="16dd5-1075">11.3.1.1 Lookup</span></span>

<span data-ttu-id="16dd5-1076">Jednoduchý vyhledávací dotaz bez agregací</span><span class="sxs-lookup"><span data-stu-id="16dd5-1076">A simple lookup query with no aggregations</span></span>

-   <span data-ttu-id="16dd5-1077">Výpočtu 16232</span><span class="sxs-lookup"><span data-stu-id="16dd5-1077">Count: 16232</span></span>
-   <span data-ttu-id="16dd5-1078">Příklad:</span><span class="sxs-lookup"><span data-stu-id="16dd5-1078">Example:</span></span>

``` xml
  <Query complexity="Lookup">
    <CommandText>Select value distinct top(4) e.Idle_Time From NavisionFKContext.Session as e</CommandText>
  </Query>
```

##### <a name="11312singleaggregating"></a><span data-ttu-id="16dd5-1079">11.3.1.2 SingleAggregating</span><span class="sxs-lookup"><span data-stu-id="16dd5-1079">11.3.1.2 SingleAggregating</span></span>

<span data-ttu-id="16dd5-1080">Normální dotaz BI s více agregacemi, ale žádné mezisoučty (jeden dotaz)</span><span class="sxs-lookup"><span data-stu-id="16dd5-1080">A normal BI query with multiple aggregations, but no subtotals (single query)</span></span>

-   <span data-ttu-id="16dd5-1081">Výpočtu 2313</span><span class="sxs-lookup"><span data-stu-id="16dd5-1081">Count: 2313</span></span>
-   <span data-ttu-id="16dd5-1082">Příklad:</span><span class="sxs-lookup"><span data-stu-id="16dd5-1082">Example:</span></span>

``` xml
  <Query complexity="SingleAggregating">
    <CommandText>NavisionFK.MDF_SessionLogin_Time_Max()</CommandText>
  </Query>
```

<span data-ttu-id="16dd5-1083">Kde MDF @ no__t-0SessionLogin @ no__t-1Time @ no__t-2Max () je definován v modelu jako:</span><span class="sxs-lookup"><span data-stu-id="16dd5-1083">Where MDF\_SessionLogin\_Time\_Max() is defined in the model as:</span></span>

``` xml
  <Function Name="MDF_SessionLogin_Time_Max" ReturnType="Collection(DateTime)">
    <DefiningExpression>SELECT VALUE Edm.Min(E.Login_Time) FROM NavisionFKContext.Session as E</DefiningExpression>
  </Function>
```

##### <a name="11313aggregatingsubtotals"></a><span data-ttu-id="16dd5-1084">11.3.1.3 AggregatingSubtotals</span><span class="sxs-lookup"><span data-stu-id="16dd5-1084">11.3.1.3 AggregatingSubtotals</span></span>

<span data-ttu-id="16dd5-1085">Dotaz BI s agregacemi a mezisoučty (přes sjednocení)</span><span class="sxs-lookup"><span data-stu-id="16dd5-1085">A BI query with aggregations and subtotals (via union all)</span></span>

-   <span data-ttu-id="16dd5-1086">Výpočtu 178</span><span class="sxs-lookup"><span data-stu-id="16dd5-1086">Count: 178</span></span>
-   <span data-ttu-id="16dd5-1087">Příklad:</span><span class="sxs-lookup"><span data-stu-id="16dd5-1087">Example:</span></span>

``` xml
  <Query complexity="AggregatingSubtotals">
    <CommandText>
using NavisionFK;
function AmountConsumed(entities Collection([CRONUS_International_Ltd__Zone])) as
(
    Edm.Sum(select value N.Block_Movement FROM entities as E, E.CRONUS_International_Ltd__Bin as N)
)
function AmountConsumed(P1 Edm.Int32) as
(
    AmountConsumed(select value e from NavisionFKContext.CRONUS_International_Ltd__Zone as e where e.Zone_Ranking = P1)
)
----------------------------------------------------------------------------------------------------------------------
(
    select top(10) Zone_Ranking, Cross_Dock_Bin_Zone, AmountConsumed(GroupPartition(E))
    from NavisionFKContext.CRONUS_International_Ltd__Zone as E
    where AmountConsumed(E.Zone_Ranking) > @MinAmountConsumed
    group by E.Zone_Ranking, E.Cross_Dock_Bin_Zone
)
union all
(
    select top(10) Zone_Ranking, Cast(null as Edm.Byte) as P2, AmountConsumed(GroupPartition(E))
    from NavisionFKContext.CRONUS_International_Ltd__Zone as E
    where AmountConsumed(E.Zone_Ranking) > @MinAmountConsumed
    group by E.Zone_Ranking
)
union all
{
    Row(Cast(null as Edm.Int32) as P1, Cast(null as Edm.Byte) as P2, AmountConsumed(select value E
                                                                         from NavisionFKContext.CRONUS_International_Ltd__Zone as E
                                                                         where AmountConsumed(E.Zone_Ranking) > @MinAmountConsumed))
}</CommandText>
    <Parameters>
      <Parameter Name="MinAmountConsumed" DbType="Int32" Value="10000" />
    </Parameters>
  </Query>
```
