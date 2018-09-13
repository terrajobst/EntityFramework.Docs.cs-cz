---
title: Faktory ovlivňující výkon u EF4 EF5 a EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d6d5a465-6434-45fa-855d-5eb48c61a2ea
ms.openlocfilehash: a58461a6d18d9d53c002b5d45cecbff7b0cdf81e
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490256"
---
# <a name="performance-considerations-for-ef-4-5-and-6"></a><span data-ttu-id="1c6b4-102">Faktory ovlivňující výkon u EF 6, 4 a 5</span><span class="sxs-lookup"><span data-stu-id="1c6b4-102">Performance considerations for EF 4, 5, and 6</span></span>
<span data-ttu-id="1c6b4-103">David Obando, Eric Dettinger a další</span><span class="sxs-lookup"><span data-stu-id="1c6b4-103">By David Obando, Eric Dettinger and others</span></span>

<span data-ttu-id="1c6b4-104">Publikováno: Duben 2012</span><span class="sxs-lookup"><span data-stu-id="1c6b4-104">Published: April 2012</span></span>

<span data-ttu-id="1c6b4-105">Poslední aktualizace: květen 2014</span><span class="sxs-lookup"><span data-stu-id="1c6b4-105">Last updated: May 2014</span></span>

------------------------------------------------------------------------

## <a name="1-introduction"></a><span data-ttu-id="1c6b4-106">1. Úvod</span><span class="sxs-lookup"><span data-stu-id="1c6b4-106">1. Introduction</span></span>

<span data-ttu-id="1c6b4-107">Pohodlný způsob, jak poskytuje abstrakci pro přístup k datům v objektově orientované aplikace jsou objektově-relační mapování rozhraní.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-107">Object-Relational Mapping frameworks are a convenient way to provide an abstraction for data access in an object-oriented application.</span></span> <span data-ttu-id="1c6b4-108">Pro aplikace .NET doporučujeme od Microsoftu O/RM je Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-108">For .NET applications, Microsoft's recommended O/RM is Entity Framework.</span></span> <span data-ttu-id="1c6b4-109">Pomocí libovolné abstrakce, výkon se může stát žádný problém.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-109">With any abstraction though, performance can become a concern.</span></span>

<span data-ttu-id="1c6b4-110">Tento dokument White Paper byla zapsána do zobrazit důležité informace o výkonu, při vývoji aplikací pomocí Entity Frameworku, vývojáři získat představu o Entity Framework vnitřní algoritmy, které mohou ovlivnit výkon a poskytuje tipy pro šetření a zlepšení výkonu ve svých aplikacích, které používají rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-110">This whitepaper was written to show the performance considerations when developing applications using Entity Framework, to give developers an idea of the Entity Framework internal algorithms that can affect performance, and to provide tips for investigation and improving performance in their applications that use Entity Framework.</span></span> <span data-ttu-id="1c6b4-111">Existuje mnoho témat dobrý výkon již k dispozici na webu a také Snažili jsme směřující k těmto prostředkům, kde je to možné.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-111">There are a number of good topics on performance already available on the web, and we've also tried pointing to these resources where possible.</span></span>

<span data-ttu-id="1c6b4-112">Výkon je složité téma.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-112">Performance is a tricky topic.</span></span> <span data-ttu-id="1c6b4-113">Tento dokument White Paper má jako zdroj pomoci provedete výkonu související s rozhodnutí, která pro vaše aplikace, které používají rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-113">This whitepaper is intended as a resource to help you make performance related decisions for your applications that use Entity Framework.</span></span> <span data-ttu-id="1c6b4-114">Uvádíme některé metriky testů k předvedení výkon, ale tyto metriky nejsou určeny jako absolutní indikátory výkonu, které se zobrazí ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-114">We have included some test metrics to demonstrate performance, but these metrics aren't intended as absolute indicators of the performance you will see in your application.</span></span>

<span data-ttu-id="1c6b4-115">Praktické důvodů tento dokument předpokládá Entity Framework 4 je spuštěn v rámci rozhraní .NET 4.0 a Entity Framework 5 a 6 jsou spuštěny v rozhraní .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-115">For practical purposes, this document assumes Entity Framework 4 is run under .NET 4.0 and Entity Framework 5 and 6 are run under .NET 4.5.</span></span> <span data-ttu-id="1c6b4-116">Řada vylepšení výkonu pro Entity Framework 5 jsou uložené v základních komponent, které se dodávají s rozhraním .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-116">Many of the performance improvements made for Entity Framework 5 reside within the core components that ship with .NET 4.5.</span></span>

<span data-ttu-id="1c6b4-117">Entity Framework 6 je out of band verze a nezávisí na komponenty Entity Framework, které se dodávají s .NET.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-117">Entity Framework 6 is an out of band release and does not depend on the Entity Framework components that ship with .NET.</span></span> <span data-ttu-id="1c6b4-118">Entity Framework 6 pracovat na rozhraní .NET 4.0 a 4.5 rozhraní .NET a můžou nabízet velká výhoda všem uživatelům, kteří nejsou z rozhraní .NET 4.0, ale má nejnovější součásti Entity Framework ve svých aplikacích.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-118">Entity Framework 6 work on both .NET 4.0 and .NET 4.5, and can offer a big performance benefit to those who haven’t upgraded from .NET 4.0 but want the latest Entity Framework bits in their application.</span></span> <span data-ttu-id="1c6b4-119">Když tento dokument uvádí Entity Framework 6, odkazuje na nejnovější verzi k dispozici v době psaní tohoto textu: verze 6.1.0.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-119">When this document mentions Entity Framework 6, it refers to the latest version available at the time of this writing: version 6.1.0.</span></span>

## <a name="2-cold-vs-warm-query-execution"></a><span data-ttu-id="1c6b4-120">2. Studená vs. Provádění dotazu teplé</span><span class="sxs-lookup"><span data-stu-id="1c6b4-120">2. Cold vs. Warm Query Execution</span></span>

<span data-ttu-id="1c6b4-121">Při prvním provedené jakéhokoli dotazu pro daný model Entity Framework nemá spoustu práce na pozadí k načtení a ověření modelu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-121">The very first time any query is made against a given model, the Entity Framework does a lot of work behind the scenes to load and validate the model.</span></span> <span data-ttu-id="1c6b4-122">Často označujeme tento první dotaz jako dotaz "studenými".</span><span class="sxs-lookup"><span data-stu-id="1c6b4-122">We frequently refer to this first query as a "cold" query.</span></span>  <span data-ttu-id="1c6b4-123">Další dotazy vůči už načtený modelu jsou označovány jako "teplé" dotazy a je mnohem rychlejší.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-123">Further queries against an already loaded model are known as "warm" queries, and are much faster.</span></span>

<span data-ttu-id="1c6b4-124">Pojďme trvat souhrnný přehled trvají delší dobu při provádění dotazu používá nástroj Entity Framework a v tématu, kde se věci zlepšení v Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-124">Let’s take a high-level view of where time is spent when executing a query using Entity Framework, and see where things are improving in Entity Framework 6.</span></span>

<span data-ttu-id="1c6b4-125">**První spuštění dotazu – studenou dotazu**</span><span class="sxs-lookup"><span data-stu-id="1c6b4-125">**First Query Execution – cold query**</span></span>

| <span data-ttu-id="1c6b4-126">Uživatel zapíše kód</span><span class="sxs-lookup"><span data-stu-id="1c6b4-126">Code User Writes</span></span>                                                                                     | <span data-ttu-id="1c6b4-127">Akce</span><span class="sxs-lookup"><span data-stu-id="1c6b4-127">Action</span></span>                    | <span data-ttu-id="1c6b4-128">EF4 Dopad na výkon</span><span class="sxs-lookup"><span data-stu-id="1c6b4-128">EF4 Performance Impact</span></span>                                                                                                                                                                                                                                                                                                                                                                                                        | <span data-ttu-id="1c6b4-129">EF5 Dopad na výkon</span><span class="sxs-lookup"><span data-stu-id="1c6b4-129">EF5 Performance Impact</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                    | <span data-ttu-id="1c6b4-130">EF6 Dopad na výkon</span><span class="sxs-lookup"><span data-stu-id="1c6b4-130">EF6 Performance Impact</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|:-----------------------------------------------------------------------------------------------------|:--------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `using(var db = new MyContext())` <br/> `{`                                                          | <span data-ttu-id="1c6b4-131">Vytvoření kontextu</span><span class="sxs-lookup"><span data-stu-id="1c6b4-131">Context creation</span></span>          | <span data-ttu-id="1c6b4-132">Střední</span><span class="sxs-lookup"><span data-stu-id="1c6b4-132">Medium</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                        | <span data-ttu-id="1c6b4-133">Střední</span><span class="sxs-lookup"><span data-stu-id="1c6b4-133">Medium</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | <span data-ttu-id="1c6b4-134">Nízká</span><span class="sxs-lookup"><span data-stu-id="1c6b4-134">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `  var q1 = ` <br/> `    from c in db.Customers` <br/> `    where c.Id == id1` <br/> `    select c;` | <span data-ttu-id="1c6b4-135">Vytvoření výrazu dotazu</span><span class="sxs-lookup"><span data-stu-id="1c6b4-135">Query expression creation</span></span> | <span data-ttu-id="1c6b4-136">Nízká</span><span class="sxs-lookup"><span data-stu-id="1c6b4-136">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                           | <span data-ttu-id="1c6b4-137">Nízká</span><span class="sxs-lookup"><span data-stu-id="1c6b4-137">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="1c6b4-138">Nízká</span><span class="sxs-lookup"><span data-stu-id="1c6b4-138">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `  var c1 = q1.First();`                                                                             | <span data-ttu-id="1c6b4-139">Provádění dotazů LINQ</span><span class="sxs-lookup"><span data-stu-id="1c6b4-139">LINQ query execution</span></span>      | <span data-ttu-id="1c6b4-140">-Načítání metadat: Vysoká, ale v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="1c6b4-140">- Metadata loading: High but cached</span></span> <br/> <span data-ttu-id="1c6b4-141">– Zobrazení generování: potenciálně velmi vysoké, ale v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="1c6b4-141">- View generation: Potentially very high but cached</span></span> <br/> <span data-ttu-id="1c6b4-142">-Parametr hodnocení: střední</span><span class="sxs-lookup"><span data-stu-id="1c6b4-142">- Parameter evaluation: Medium</span></span> <br/> <span data-ttu-id="1c6b4-143">-Dotazování překlad: střední</span><span class="sxs-lookup"><span data-stu-id="1c6b4-143">- Query translation: Medium</span></span> <br/> <span data-ttu-id="1c6b4-144">-Generování materializer: střední ale v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="1c6b4-144">- Materializer generation: Medium but cached</span></span> <br/> <span data-ttu-id="1c6b4-145">-Databáze provádění dotazů: potenciálně velkému</span><span class="sxs-lookup"><span data-stu-id="1c6b4-145">- Database query execution: Potentially high</span></span> <br/> <span data-ttu-id="1c6b4-146">+ Connection.Open</span><span class="sxs-lookup"><span data-stu-id="1c6b4-146">+ Connection.Open</span></span> <br/> <span data-ttu-id="1c6b4-147">+ Command.ExecuteReader</span><span class="sxs-lookup"><span data-stu-id="1c6b4-147">+ Command.ExecuteReader</span></span> <br/> <span data-ttu-id="1c6b4-148">+ DataReader.Read</span><span class="sxs-lookup"><span data-stu-id="1c6b4-148">+ DataReader.Read</span></span> <br/> <span data-ttu-id="1c6b4-149">Materializace objektů: střední</span><span class="sxs-lookup"><span data-stu-id="1c6b4-149">Object materialization: Medium</span></span> <br/> <span data-ttu-id="1c6b4-150">-Vyhledávání identity: střední</span><span class="sxs-lookup"><span data-stu-id="1c6b4-150">- Identity lookup: Medium</span></span> | <span data-ttu-id="1c6b4-151">-Načítání metadat: Vysoká, ale v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="1c6b4-151">- Metadata loading: High but cached</span></span> <br/> <span data-ttu-id="1c6b4-152">– Zobrazení generování: potenciálně velmi vysoké, ale v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="1c6b4-152">- View generation: Potentially very high but cached</span></span> <br/> <span data-ttu-id="1c6b4-153">-Parametr hodnocení: Nízká</span><span class="sxs-lookup"><span data-stu-id="1c6b4-153">- Parameter evaluation: Low</span></span> <br/> <span data-ttu-id="1c6b4-154">-Dotazování překlad: střední ale v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="1c6b4-154">- Query translation: Medium but cached</span></span> <br/> <span data-ttu-id="1c6b4-155">-Generování materializer: střední ale v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="1c6b4-155">- Materializer generation: Medium but cached</span></span> <br/> <span data-ttu-id="1c6b4-156">-Databáze provádění dotazů: potenciálně velkému (lepší dotazy v některých situacích)</span><span class="sxs-lookup"><span data-stu-id="1c6b4-156">- Database query execution: Potentially high (Better queries in some situations)</span></span> <br/> <span data-ttu-id="1c6b4-157">+ Connection.Open</span><span class="sxs-lookup"><span data-stu-id="1c6b4-157">+ Connection.Open</span></span> <br/> <span data-ttu-id="1c6b4-158">+ Command.ExecuteReader</span><span class="sxs-lookup"><span data-stu-id="1c6b4-158">+ Command.ExecuteReader</span></span> <br/> <span data-ttu-id="1c6b4-159">+ DataReader.Read</span><span class="sxs-lookup"><span data-stu-id="1c6b4-159">+ DataReader.Read</span></span> <br/> <span data-ttu-id="1c6b4-160">Materializace objektů: střední</span><span class="sxs-lookup"><span data-stu-id="1c6b4-160">Object materialization: Medium</span></span> <br/> <span data-ttu-id="1c6b4-161">-Vyhledávání identity: střední</span><span class="sxs-lookup"><span data-stu-id="1c6b4-161">- Identity lookup: Medium</span></span> | <span data-ttu-id="1c6b4-162">-Načítání metadat: Vysoká, ale v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="1c6b4-162">- Metadata loading: High but cached</span></span> <br/> <span data-ttu-id="1c6b4-163">– Zobrazení generování: střední ale v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="1c6b4-163">- View generation: Medium but cached</span></span> <br/> <span data-ttu-id="1c6b4-164">-Parametr hodnocení: Nízká</span><span class="sxs-lookup"><span data-stu-id="1c6b4-164">- Parameter evaluation: Low</span></span> <br/> <span data-ttu-id="1c6b4-165">-Dotazování překlad: střední ale v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="1c6b4-165">- Query translation: Medium but cached</span></span> <br/> <span data-ttu-id="1c6b4-166">-Generování materializer: střední ale v mezipaměti</span><span class="sxs-lookup"><span data-stu-id="1c6b4-166">- Materializer generation: Medium but cached</span></span> <br/> <span data-ttu-id="1c6b4-167">-Databáze provádění dotazů: potenciálně velkému (lepší dotazy v některých situacích)</span><span class="sxs-lookup"><span data-stu-id="1c6b4-167">- Database query execution: Potentially high (Better queries in some situations)</span></span> <br/> <span data-ttu-id="1c6b4-168">+ Connection.Open</span><span class="sxs-lookup"><span data-stu-id="1c6b4-168">+ Connection.Open</span></span> <br/> <span data-ttu-id="1c6b4-169">+ Command.ExecuteReader</span><span class="sxs-lookup"><span data-stu-id="1c6b4-169">+ Command.ExecuteReader</span></span> <br/> <span data-ttu-id="1c6b4-170">+ DataReader.Read</span><span class="sxs-lookup"><span data-stu-id="1c6b4-170">+ DataReader.Read</span></span> <br/> <span data-ttu-id="1c6b4-171">Materializace objektů: střední (rychlejší než EF5)</span><span class="sxs-lookup"><span data-stu-id="1c6b4-171">Object materialization: Medium (Faster than EF5)</span></span> <br/> <span data-ttu-id="1c6b4-172">-Vyhledávání identity: střední</span><span class="sxs-lookup"><span data-stu-id="1c6b4-172">- Identity lookup: Medium</span></span> |
| `}`                                                                                                  | <span data-ttu-id="1c6b4-173">Connection.Close</span><span class="sxs-lookup"><span data-stu-id="1c6b4-173">Connection.Close</span></span>          | <span data-ttu-id="1c6b4-174">Nízká</span><span class="sxs-lookup"><span data-stu-id="1c6b4-174">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                           | <span data-ttu-id="1c6b4-175">Nízká</span><span class="sxs-lookup"><span data-stu-id="1c6b4-175">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="1c6b4-176">Nízká</span><span class="sxs-lookup"><span data-stu-id="1c6b4-176">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |


<span data-ttu-id="1c6b4-177">**Spuštění druhého dotazu – teplé dotazu**</span><span class="sxs-lookup"><span data-stu-id="1c6b4-177">**Second Query Execution – warm query**</span></span>

| <span data-ttu-id="1c6b4-178">Uživatel zapíše kód</span><span class="sxs-lookup"><span data-stu-id="1c6b4-178">Code User Writes</span></span>                                                                                     | <span data-ttu-id="1c6b4-179">Akce</span><span class="sxs-lookup"><span data-stu-id="1c6b4-179">Action</span></span>                    | <span data-ttu-id="1c6b4-180">EF4 Dopad na výkon</span><span class="sxs-lookup"><span data-stu-id="1c6b4-180">EF4 Performance Impact</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | <span data-ttu-id="1c6b4-181">EF5 Dopad na výkon</span><span class="sxs-lookup"><span data-stu-id="1c6b4-181">EF5 Performance Impact</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | <span data-ttu-id="1c6b4-182">EF6 Dopad na výkon</span><span class="sxs-lookup"><span data-stu-id="1c6b4-182">EF6 Performance Impact</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|:-----------------------------------------------------------------------------------------------------|:--------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `using(var db = new MyContext())` <br/> `{`                                                          | <span data-ttu-id="1c6b4-183">Vytvoření kontextu</span><span class="sxs-lookup"><span data-stu-id="1c6b4-183">Context creation</span></span>          | <span data-ttu-id="1c6b4-184">Střední</span><span class="sxs-lookup"><span data-stu-id="1c6b4-184">Medium</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | <span data-ttu-id="1c6b4-185">Střední</span><span class="sxs-lookup"><span data-stu-id="1c6b4-185">Medium</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | <span data-ttu-id="1c6b4-186">Nízká</span><span class="sxs-lookup"><span data-stu-id="1c6b4-186">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `  var q1 = ` <br/> `    from c in db.Customers` <br/> `    where c.Id == id1` <br/> `    select c;` | <span data-ttu-id="1c6b4-187">Vytvoření výrazu dotazu</span><span class="sxs-lookup"><span data-stu-id="1c6b4-187">Query expression creation</span></span> | <span data-ttu-id="1c6b4-188">Nízká</span><span class="sxs-lookup"><span data-stu-id="1c6b4-188">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | <span data-ttu-id="1c6b4-189">Nízká</span><span class="sxs-lookup"><span data-stu-id="1c6b4-189">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | <span data-ttu-id="1c6b4-190">Nízká</span><span class="sxs-lookup"><span data-stu-id="1c6b4-190">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `  var c1 = q1.First();`                                                                             | <span data-ttu-id="1c6b4-191">Provádění dotazů LINQ</span><span class="sxs-lookup"><span data-stu-id="1c6b4-191">LINQ query execution</span></span>      | <span data-ttu-id="1c6b4-192">-Metadat ~~načítání~~ vyhledávání: ~~vysoká, ale v mezipaměti~~ nízká</span><span class="sxs-lookup"><span data-stu-id="1c6b4-192">- Metadata ~~loading~~ lookup: ~~High but cached~~ Low</span></span> <br/> <span data-ttu-id="1c6b4-193">– Zobrazení ~~generování~~ vyhledávání: ~~potenciálně velmi vysoké, ale v mezipaměti~~ nízká</span><span class="sxs-lookup"><span data-stu-id="1c6b4-193">- View ~~generation~~ lookup: ~~Potentially very high but cached~~ Low</span></span> <br/> <span data-ttu-id="1c6b4-194">-Parametr hodnocení: střední</span><span class="sxs-lookup"><span data-stu-id="1c6b4-194">- Parameter evaluation: Medium</span></span> <br/> <span data-ttu-id="1c6b4-195">-Dotazování ~~překlad~~ vyhledávání: střední</span><span class="sxs-lookup"><span data-stu-id="1c6b4-195">- Query ~~translation~~ lookup: Medium</span></span> <br/> <span data-ttu-id="1c6b4-196">-Materializer ~~generování~~ vyhledávání: ~~střední ale v mezipaměti~~ nízká</span><span class="sxs-lookup"><span data-stu-id="1c6b4-196">- Materializer ~~generation~~ lookup: ~~Medium but cached~~ Low</span></span> <br/> <span data-ttu-id="1c6b4-197">-Databáze provádění dotazů: potenciálně velkému</span><span class="sxs-lookup"><span data-stu-id="1c6b4-197">- Database query execution: Potentially high</span></span> <br/> <span data-ttu-id="1c6b4-198">+ Connection.Open</span><span class="sxs-lookup"><span data-stu-id="1c6b4-198">+ Connection.Open</span></span> <br/> <span data-ttu-id="1c6b4-199">+ Command.ExecuteReader</span><span class="sxs-lookup"><span data-stu-id="1c6b4-199">+ Command.ExecuteReader</span></span> <br/> <span data-ttu-id="1c6b4-200">+ DataReader.Read</span><span class="sxs-lookup"><span data-stu-id="1c6b4-200">+ DataReader.Read</span></span> <br/> <span data-ttu-id="1c6b4-201">Materializace objektů: střední</span><span class="sxs-lookup"><span data-stu-id="1c6b4-201">Object materialization: Medium</span></span> <br/> <span data-ttu-id="1c6b4-202">-Vyhledávání identity: střední</span><span class="sxs-lookup"><span data-stu-id="1c6b4-202">- Identity lookup: Medium</span></span> | <span data-ttu-id="1c6b4-203">-Metadat ~~načítání~~ vyhledávání: ~~vysoká, ale v mezipaměti~~ nízká</span><span class="sxs-lookup"><span data-stu-id="1c6b4-203">- Metadata ~~loading~~ lookup: ~~High but cached~~ Low</span></span> <br/> <span data-ttu-id="1c6b4-204">– Zobrazení ~~generování~~ vyhledávání: ~~potenciálně velmi vysoké, ale v mezipaměti~~ nízká</span><span class="sxs-lookup"><span data-stu-id="1c6b4-204">- View ~~generation~~ lookup: ~~Potentially very high but cached~~ Low</span></span> <br/> <span data-ttu-id="1c6b4-205">-Parametr hodnocení: Nízká</span><span class="sxs-lookup"><span data-stu-id="1c6b4-205">- Parameter evaluation: Low</span></span> <br/> <span data-ttu-id="1c6b4-206">-Dotazování ~~překlad~~ vyhledávání: ~~střední ale v mezipaměti~~ nízká</span><span class="sxs-lookup"><span data-stu-id="1c6b4-206">- Query ~~translation~~ lookup: ~~Medium but cached~~ Low</span></span> <br/> <span data-ttu-id="1c6b4-207">-Materializer ~~generování~~ vyhledávání: ~~střední ale v mezipaměti~~ nízká</span><span class="sxs-lookup"><span data-stu-id="1c6b4-207">- Materializer ~~generation~~ lookup: ~~Medium but cached~~ Low</span></span> <br/> <span data-ttu-id="1c6b4-208">-Databáze provádění dotazů: potenciálně velkému (lepší dotazy v některých situacích)</span><span class="sxs-lookup"><span data-stu-id="1c6b4-208">- Database query execution: Potentially high (Better queries in some situations)</span></span> <br/> <span data-ttu-id="1c6b4-209">+ Connection.Open</span><span class="sxs-lookup"><span data-stu-id="1c6b4-209">+ Connection.Open</span></span> <br/> <span data-ttu-id="1c6b4-210">+ Command.ExecuteReader</span><span class="sxs-lookup"><span data-stu-id="1c6b4-210">+ Command.ExecuteReader</span></span> <br/> <span data-ttu-id="1c6b4-211">+ DataReader.Read</span><span class="sxs-lookup"><span data-stu-id="1c6b4-211">+ DataReader.Read</span></span> <br/> <span data-ttu-id="1c6b4-212">Materializace objektů: střední</span><span class="sxs-lookup"><span data-stu-id="1c6b4-212">Object materialization: Medium</span></span> <br/> <span data-ttu-id="1c6b4-213">-Vyhledávání identity: střední</span><span class="sxs-lookup"><span data-stu-id="1c6b4-213">- Identity lookup: Medium</span></span> | <span data-ttu-id="1c6b4-214">-Metadat ~~načítání~~ vyhledávání: ~~vysoká, ale v mezipaměti~~ nízká</span><span class="sxs-lookup"><span data-stu-id="1c6b4-214">- Metadata ~~loading~~ lookup: ~~High but cached~~ Low</span></span> <br/> <span data-ttu-id="1c6b4-215">– Zobrazení ~~generování~~ vyhledávání: ~~střední ale v mezipaměti~~ nízká</span><span class="sxs-lookup"><span data-stu-id="1c6b4-215">- View ~~generation~~ lookup: ~~Medium but cached~~ Low</span></span> <br/> <span data-ttu-id="1c6b4-216">-Parametr hodnocení: Nízká</span><span class="sxs-lookup"><span data-stu-id="1c6b4-216">- Parameter evaluation: Low</span></span> <br/> <span data-ttu-id="1c6b4-217">-Dotazování ~~překlad~~ vyhledávání: ~~střední ale v mezipaměti~~ nízká</span><span class="sxs-lookup"><span data-stu-id="1c6b4-217">- Query ~~translation~~ lookup: ~~Medium but cached~~ Low</span></span> <br/> <span data-ttu-id="1c6b4-218">-Materializer ~~generování~~ vyhledávání: ~~střední ale v mezipaměti~~ nízká</span><span class="sxs-lookup"><span data-stu-id="1c6b4-218">- Materializer ~~generation~~ lookup: ~~Medium but cached~~ Low</span></span> <br/> <span data-ttu-id="1c6b4-219">-Databáze provádění dotazů: potenciálně velkému (lepší dotazy v některých situacích)</span><span class="sxs-lookup"><span data-stu-id="1c6b4-219">- Database query execution: Potentially high (Better queries in some situations)</span></span> <br/> <span data-ttu-id="1c6b4-220">+ Connection.Open</span><span class="sxs-lookup"><span data-stu-id="1c6b4-220">+ Connection.Open</span></span> <br/> <span data-ttu-id="1c6b4-221">+ Command.ExecuteReader</span><span class="sxs-lookup"><span data-stu-id="1c6b4-221">+ Command.ExecuteReader</span></span> <br/> <span data-ttu-id="1c6b4-222">+ DataReader.Read</span><span class="sxs-lookup"><span data-stu-id="1c6b4-222">+ DataReader.Read</span></span> <br/> <span data-ttu-id="1c6b4-223">Materializace objektů: střední (rychlejší než EF5)</span><span class="sxs-lookup"><span data-stu-id="1c6b4-223">Object materialization: Medium (Faster than EF5)</span></span> <br/> <span data-ttu-id="1c6b4-224">-Vyhledávání identity: střední</span><span class="sxs-lookup"><span data-stu-id="1c6b4-224">- Identity lookup: Medium</span></span> |
| `}`                                                                                                  | <span data-ttu-id="1c6b4-225">Connection.Close</span><span class="sxs-lookup"><span data-stu-id="1c6b4-225">Connection.Close</span></span>          | <span data-ttu-id="1c6b4-226">Nízká</span><span class="sxs-lookup"><span data-stu-id="1c6b4-226">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | <span data-ttu-id="1c6b4-227">Nízká</span><span class="sxs-lookup"><span data-stu-id="1c6b4-227">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | <span data-ttu-id="1c6b4-228">Nízká</span><span class="sxs-lookup"><span data-stu-id="1c6b4-228">Low</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |


<span data-ttu-id="1c6b4-229">Existuje několik způsobů, jak snížit náklady na výkon dotazů, studené a horké a provedeme najdete v těchto v následující části.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-229">There are several ways to reduce the performance cost of both cold and warm queries, and we'll take a look at these in the following section.</span></span> <span data-ttu-id="1c6b4-230">Konkrétně zaměříme na snížíte náklady na načítání v studenou dotazy pomocí předem vygenerovaných zobrazení, které by měly pomoci zmírnit výkonu důsledně došlo během generování zobrazení modelu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-230">Specifically, we'll look at reducing the cost of model loading in cold queries by using pre-generated views, which should help alleviate performance pains experienced during view generation.</span></span> <span data-ttu-id="1c6b4-231">Za dotazy na horké budeme zabývat ukládání do mezipaměti plánu dotazu, žádné dotazy sledování a možnosti spuštění různých dotazů.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-231">For warm queries, we'll cover query plan caching, no tracking queries, and different query execution options.</span></span>

### <a name="21-what-is-view-generation"></a><span data-ttu-id="1c6b4-232">2.1 co je generování zobrazení?</span><span class="sxs-lookup"><span data-stu-id="1c6b4-232">2.1 What is View Generation?</span></span>

<span data-ttu-id="1c6b4-233">Chcete-li pochopit, jaké zobrazení generování není, musíte nejprve rozumí tomu, co jsou "Zobrazení mapování".</span><span class="sxs-lookup"><span data-stu-id="1c6b4-233">In order to understand what view generation is, we must first understand what “Mapping Views” are.</span></span> <span data-ttu-id="1c6b4-234">Zobrazení mapování jsou spustitelné vyjádření transformací zadán v mapování pro každou sadu entit a přidružení.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-234">Mapping Views are executable representations of the transformations specified in the mapping for each entity set and association.</span></span> <span data-ttu-id="1c6b4-235">Interně tato zobrazení mapování mít tvar CQTs (dotaz kanonické stromy).</span><span class="sxs-lookup"><span data-stu-id="1c6b4-235">Internally, these mapping views take the shape of CQTs (canonical query trees).</span></span> <span data-ttu-id="1c6b4-236">Existují dva typy zobrazení mapování:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-236">There are two types of mapping views:</span></span>

-   <span data-ttu-id="1c6b4-237">Zobrazení dotazu: představují transformace potřeba přejít ze schématu databáze do koncepčního modelu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-237">Query views: these represent the transformation necessary to go from the database schema to the conceptual model.</span></span>
-   <span data-ttu-id="1c6b4-238">Aktualizovat zobrazení: představují transformace potřeba přejít na schéma databáze v konceptuálním modelu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-238">Update views: these represent the transformation necessary to go from the conceptual model to the database schema.</span></span>

<span data-ttu-id="1c6b4-239">Mějte na paměti, že konceptuálního modelu se mohou lišit od schématu databáze různými způsoby.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-239">Keep in mind that the conceptual model might differ from the database schema in various ways.</span></span> <span data-ttu-id="1c6b4-240">Například jeden jedné tabulky dají zneužít k ukládání dat pro dva typy různých entit.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-240">For example, one single table might be used to store the data for two different entity types.</span></span> <span data-ttu-id="1c6b4-241">Dědičnost a nejsou v netriviálních mapování hrají roli složitosti zobrazení mapování.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-241">Inheritance and non-trivial mappings play a role in the complexity of the mapping views.</span></span>

<span data-ttu-id="1c6b4-242">Proces výpočetních tato zobrazení podle specifikace mapování je, čemu říkáme generování zobrazení.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-242">The process of computing these views based on the specification of the mapping is what we call view generation.</span></span> <span data-ttu-id="1c6b4-243">Generování zobrazení můžete buď probíhat dynamicky při načtení modelu, nebo v okamžiku sestavení s použitím "předem vygenerovaných zobrazení"; se serializují ve formuláři Entity SQL příkazy a C\# nebo soubor jazyka Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-243">View generation can either take place dynamically when a model is loaded, or at build time, by using "pre-generated views"; the latter are serialized in the form of Entity SQL statements to a C\# or VB file.</span></span>

<span data-ttu-id="1c6b4-244">Při generování zobrazení, se také ověří.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-244">When views are generated, they are also validated.</span></span> <span data-ttu-id="1c6b4-245">Většinu náklady na generování zobrazení z hlediska výkonu je ve skutečnosti ověření zobrazení, která zajistí, že připojení mezi entitami dávat smysl a mít správné kardinalitu pro všechny podporované operace.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-245">From a performance standpoint, the vast majority of the cost of view generation is actually the validation of the views which ensures that the connections between the entities make sense and have the correct cardinality for all the supported operations.</span></span>

<span data-ttu-id="1c6b4-246">Při spuštění dotazu za sadu entit dotaz je v kombinaci s odpovídající zobrazení dotazu a výsledek tohoto sestavení je projít plán kompilátor vytvoří reprezentace dotaz, který umožní pochopit záložního úložiště.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-246">When a query over an entity set is executed, the query is combined with the corresponding query view, and the result of this composition is run through the plan compiler to create the representation of the query that the backing store can understand.</span></span> <span data-ttu-id="1c6b4-247">Konečný výsledek tohoto kompilace pro SQL Server, bude příkaz T-SQL SELECT.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-247">For SQL Server, the final result of this compilation will be a T-SQL SELECT statement.</span></span> <span data-ttu-id="1c6b4-248">Při prvním provádí aktualizace přes sadu entit, zobrazení aktualizace je spuštěn prostřednictvím podobným jeho transformaci na příkazy DML pro cílovou databázi.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-248">The first time an update over an entity set is performed, the update view is run through a similar process to transform it into DML statements for the target database.</span></span>

### <a name="22-factors-that-affect-view-generation-performance"></a><span data-ttu-id="1c6b4-249">2.2 faktory, které ovlivňují výkon generování zobrazení</span><span class="sxs-lookup"><span data-stu-id="1c6b4-249">2.2 Factors that affect View Generation performance</span></span>

<span data-ttu-id="1c6b4-250">Výkon zobrazení generování krok nejen závisí na velikosti vašeho modelu, ale také na způsob propojených je model.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-250">The performance of view generation step not only depends on the size of your model but also on how interconnected the model is.</span></span> <span data-ttu-id="1c6b4-251">Pokud dvě entity, které jsou připojené přes řetěz dědičnosti nebo přidružení, se označují za připojené.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-251">If two Entities are connected via an inheritance chain or an Association, they are said to be connected.</span></span> <span data-ttu-id="1c6b4-252">Podobně pokud dvě tabulky jsou připojené přes cizího klíče, jsou připojeni.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-252">Similarly if two tables are connected via a foreign key, they are connected.</span></span> <span data-ttu-id="1c6b4-253">Jak zvýšit počet připojených entit a tabulek v vaše schémat, generování zobrazení náklady narůstají.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-253">As the number of connected Entities and tables in your schemas increase, the view generation cost increases.</span></span>

<span data-ttu-id="1c6b4-254">Algoritmus, který jsme použili pro generování a ověření zobrazení je exponenciální v nejhorším případě ale některé optimalizace používáme ke zlepšování to.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-254">The algorithm that we use to generate and validate views is exponential in the worst case, though we do use some optimizations to improve this.</span></span> <span data-ttu-id="1c6b4-255">Zdá se, že negativně ovlivnit výkon největší faktory jsou:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-255">The biggest factors that seem to negatively affect performance are:</span></span>

-   <span data-ttu-id="1c6b4-256">Model velikost odkazující na počet entit a množství přidružení mezi těmito entitami.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-256">Model size, referring to the number of entities and the amount of associations between these entities.</span></span>
-   <span data-ttu-id="1c6b4-257">Model složitost, konkrétně dědičnosti zahrnujících velký počet typů.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-257">Model complexity, specifically inheritance involving a large number of types.</span></span>
-   <span data-ttu-id="1c6b4-258">Místo nezávislé přidružení cizího klíče asociace.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-258">Using Independent Associations, instead of Foreign Key Associations.</span></span>

<span data-ttu-id="1c6b4-259">Pro malé, jednoduchých modelů může být dostatečně malá, aby nebyl nepokoušejte pomocí předem vygenerovaných zobrazení náklady.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-259">For small, simple models the cost may be small enough to not bother using pre-generated views.</span></span> <span data-ttu-id="1c6b4-260">Model velikosti a složitosti zvýšit, máte několik možností, které jsou k dispozici, abyste snížili náklady na generování zobrazení a ověření.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-260">As model size and complexity increase, there are several options available to reduce the cost of view generation and validation.</span></span>

### <a name="23-using-pre-generated-views-to-decrease-model-load-time"></a><span data-ttu-id="1c6b4-261">2.3 použití Pre-Generated zobrazení modelu snížit dobu načítání</span><span class="sxs-lookup"><span data-stu-id="1c6b4-261">2.3 Using Pre-Generated Views to decrease model load time</span></span>

#### <a name="231-pre-generated-views-using-the-entity-framework-power-tools"></a><span data-ttu-id="1c6b4-262">2.3.1 předem vygenerovaných zobrazení pomocí Entity Framework Power Tools</span><span class="sxs-lookup"><span data-stu-id="1c6b4-262">2.3.1 Pre-Generated views using the Entity Framework Power Tools</span></span>

<span data-ttu-id="1c6b4-263">Můžete také zvážit použití Entity Framework Power Tools ke generování zobrazení modelů EDMX a Code First pravým tlačítkem myši na soubor třídy modelu a pomocí Entity Framework nabídce vyberte "Generování zobrazení".</span><span class="sxs-lookup"><span data-stu-id="1c6b4-263">You can also consider using the Entity Framework Power Tools to generate views of EDMX and Code First models by right-clicking the model class file and using the Entity Framework menu to select “Generate Views”.</span></span> <span data-ttu-id="1c6b4-264">Entity Framework Power Tools fungují jenom s kontexty odvozené DbContext a lze nalézt v \< http://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d>.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-264">The Entity Framework Power Tools work only on DbContext-derived contexts and can be found at \<http://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d>.</span></span>

<span data-ttu-id="1c6b4-265">Další informace o tom, jak použít předem vygenerovaných zobrazení v Entity Framework 6 [zobrazení mapování Pre-Generated](~/ef6/fundamentals/performance/pre-generated-views.md).</span><span class="sxs-lookup"><span data-stu-id="1c6b4-265">For more information on how to use pre-generated views on Entity Framework 6 visit [Pre-Generated Mapping Views](~/ef6/fundamentals/performance/pre-generated-views.md).</span></span>

#### <a name="232-how-to-use-pre-generated-views-with-a-model-created-by-edmgen"></a><span data-ttu-id="1c6b4-266">2.3.2 jak pomocí předem vygenerovaných zobrazení s modelem vytvořené EDMGen</span><span class="sxs-lookup"><span data-stu-id="1c6b4-266">2.3.2 How to use Pre-generated views with a model created by EDMGen</span></span>

<span data-ttu-id="1c6b4-267">EDMGen je nástroj, který se dodává s využitím .NET a funguje s Entity Framework 4 a 5, ale ne s Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-267">EDMGen is a utility that ships with .NET and works with Entity Framework 4 and 5, but not with Entity Framework 6.</span></span> <span data-ttu-id="1c6b4-268">EDMGen umožňuje generovat soubor modelu, objektové vrstvě a zobrazení z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-268">EDMGen allows you to generate a model file, the object layer and the views from the command line.</span></span> <span data-ttu-id="1c6b4-269">Jedna z výstupů se bude zobrazení souboru v jazyce podle vlastní volby, VB nebo C\#.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-269">One of the outputs will be a Views file in your language of choice, VB or C\#.</span></span> <span data-ttu-id="1c6b4-270">Toto je soubor kódu obsahující fragmenty Entity SQL pro každou sadu entit.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-270">This is a code file containing Entity SQL snippets for each entity set.</span></span> <span data-ttu-id="1c6b4-271">Pokud chcete povolit předem vygenerovaných zobrazení, jednoduše zahrnout soubor ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-271">To enable pre-generated views, you simply include the file in your project.</span></span>

<span data-ttu-id="1c6b4-272">Pokud provádíte ruční úpravy soubory schémat pro model, musíte znovu vygenerovat zobrazení souboru.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-272">If you manually make edits to the schema files for the model, you will need to re-generate the views file.</span></span> <span data-ttu-id="1c6b4-273">Můžete to provést spuštěním EDMGen s **/mode:ViewGeneration** příznak.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-273">You can do this by running EDMGen with the **/mode:ViewGeneration** flag.</span></span>

<span data-ttu-id="1c6b4-274">Pro další informaci naleznete v tématu [postupy: zobrazení Pre-Generate ke zlepšení výkonu dotazů](https://msdn.microsoft.com/library/bb896240.aspx).</span><span class="sxs-lookup"><span data-stu-id="1c6b4-274">For further reference, see [How to: Pre-Generate Views to Improve Query Performance](https://msdn.microsoft.com/library/bb896240.aspx).</span></span>

#### <a name="233-how-to-use-pre-generated-views-with-an-edmx-file"></a><span data-ttu-id="1c6b4-275">2.3.3 jak Pre-Generated zobrazení pomocí souboru EDMX</span><span class="sxs-lookup"><span data-stu-id="1c6b4-275">2.3.3 How to use Pre-Generated Views with an EDMX file</span></span>

<span data-ttu-id="1c6b4-276">EDMGen můžete také použít ke generování zobrazení souboru EDMX – dříve odkazovaných MSDN téma popisuje postup přidání události před sestavením k tomu –, ale to je složitá a existují případy, kdy není možné.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-276">You can also use EDMGen to generate views for an EDMX file - the previously referenced MSDN topic describes how to add a pre-build event to do this - but this is complicated and there are some cases where it isn't possible.</span></span> <span data-ttu-id="1c6b4-277">Je obvykle jednodušší použít šablonu T4 pro generování zobrazení, když je model v souboru edmx.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-277">It's generally easier to use a T4 template to generate the views when your model is in an edmx file.</span></span>

<span data-ttu-id="1c6b4-278">Blog týmu ADO.NET má příspěvek, který popisuje, jak použít šablonu T4 pro generování zobrazení ( \< http://blogs.msdn.com/b/adonet/archive/2008/06/20/how-to-use-a-t4-template-for-view-generation.aspx>).</span><span class="sxs-lookup"><span data-stu-id="1c6b4-278">The ADO.NET team blog has a post that describes how to use a T4 template for view generation ( \<http://blogs.msdn.com/b/adonet/archive/2008/06/20/how-to-use-a-t4-template-for-view-generation.aspx>).</span></span> <span data-ttu-id="1c6b4-279">Tento příspěvek obsahuje šablonu, která je možné stáhnout a přidat do projektu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-279">This post includes a template that can be downloaded and added to your project.</span></span> <span data-ttu-id="1c6b4-280">Šablony bylo napsáno pro první verzi Entity Frameworku, takže není zaručena pro práci s nejnovější verzí Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-280">The template was written for the first version of Entity Framework, so they aren’t guaranteed to work with the latest versions of Entity Framework.</span></span> <span data-ttu-id="1c6b4-281">Však si můžete stáhnout aktuální sadu zobrazení generování šablon pro Entity Framework 4 a 5from Galerie sady Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-281">However, you can download a more up-to-date set of view generation templates for Entity Framework 4 and 5from the Visual Studio Gallery:</span></span>

-   <span data-ttu-id="1c6b4-282">VB.NET: \<http://visualstudiogallery.msdn.microsoft.com/118b44f2-1b91-4de2-a584-7a680418941d></span><span class="sxs-lookup"><span data-stu-id="1c6b4-282">VB.NET: \<http://visualstudiogallery.msdn.microsoft.com/118b44f2-1b91-4de2-a584-7a680418941d></span></span>
-   <span data-ttu-id="1c6b4-283">C\#: \<http://visualstudiogallery.msdn.microsoft.com/ae7730ce-ddab-470f-8456-1b313cd2c44d></span><span class="sxs-lookup"><span data-stu-id="1c6b4-283">C\#: \<http://visualstudiogallery.msdn.microsoft.com/ae7730ce-ddab-470f-8456-1b313cd2c44d></span></span>

<span data-ttu-id="1c6b4-284">Pokud používáte Entity Framework 6 můžete získat zobrazení T4 generování šablon z Galerie sady Visual Studio na \< http://visualstudiogallery.msdn.microsoft.com/18a7db90-6705-4d19-9dd1-0a6c23d0751f>.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-284">If you’re using Entity Framework 6 you can get the view generation T4 templates from the Visual Studio Gallery at \<http://visualstudiogallery.msdn.microsoft.com/18a7db90-6705-4d19-9dd1-0a6c23d0751f>.</span></span>

#### <a name="234-how-to-use-pre-generated-views-with-a-code-first-model"></a><span data-ttu-id="1c6b4-285">2.3.4 jak Pre-Generated zobrazení pomocí modelu Code First</span><span class="sxs-lookup"><span data-stu-id="1c6b4-285">2.3.4 How to use Pre-Generated Views with a Code First model</span></span>

<span data-ttu-id="1c6b4-286">Je také možné použít předem vygenerovaných zobrazení s projektem Code First.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-286">It's also possible to use pre-generated views with a Code First project.</span></span> <span data-ttu-id="1c6b4-287">Entity Framework Power Tools má schopnost generovat soubor zobrazení projektu Code First.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-287">The Entity Framework Power Tools has the ability to generate a views file for your Code First project.</span></span> <span data-ttu-id="1c6b4-288">Entity Framework Power Tools můžete najít v Galerii Visual Studio na \< http://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d/>.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-288">You can find the Entity Framework Power Tools in the Visual Studio Gallery at \<http://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d/>.</span></span>

### <a name="24-reducing-the-cost-of-view-generation"></a><span data-ttu-id="1c6b4-289">2.4 snížíte náklady na generování zobrazení</span><span class="sxs-lookup"><span data-stu-id="1c6b4-289">2.4 Reducing the cost of view generation</span></span>

<span data-ttu-id="1c6b4-290">Čas kompilace pomocí předem vygenerovaných zobrazení přesune náklady na generování zobrazení Načítání modelu (čas spuštění).</span><span class="sxs-lookup"><span data-stu-id="1c6b4-290">Using pre-generated views moves the cost of view generation from model loading (run time) to compile time.</span></span> <span data-ttu-id="1c6b4-291">Když to zlepšuje výkon při spouštění v době běhu, bude stále přetrvávají bolest generování zobrazení při vývoji.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-291">While this improves startup performance at runtime, you will still experience the pain of view generation while you are developing.</span></span> <span data-ttu-id="1c6b4-292">Existuje několik dalších triky, které můžou pomoct snížit náklady na generování zobrazení, jak v době kompilace a spuštění čas.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-292">There are several additional tricks that can help reduce the cost of view generation, both at compile time and run time.</span></span>

#### <a name="241-using-foreign-key-associations-to-reduce-view-generation-cost"></a><span data-ttu-id="1c6b4-293">2.4.1 pomocí přidružení cizího klíče, abyste snížili náklady na generování zobrazení</span><span class="sxs-lookup"><span data-stu-id="1c6b4-293">2.4.1 Using Foreign Key Associations to reduce view generation cost</span></span>

<span data-ttu-id="1c6b4-294">Jsme viděli počet případů, kdy přepínání přidružení v modelu z nezávislé přidružení cizího klíče asociace výrazné vylepšení doby trvání generování zobrazení.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-294">We have seen a number of cases where switching the associations in the model from Independent Associations to Foreign Key Associations dramatically improved the time spent in view generation.</span></span>

<span data-ttu-id="1c6b4-295">Abychom si předvedli toto vylepšení, jsme dvě verze modelu Navision vygenerovaného s využitím EDMGen.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-295">To demonstrate this improvement, we generated two versions of the Navision model by using EDMGen.</span></span> <span data-ttu-id="1c6b4-296">*Poznámka: seeappendix Cfor popis Navision modelu.*</span><span class="sxs-lookup"><span data-stu-id="1c6b4-296">*Note: seeappendix Cfor a description of the Navision model.*</span></span> <span data-ttu-id="1c6b4-297">Navision model je zajímavé pro toto cvičení z důvodu jeho velmi velké množství entit a vztahů mezi nimi.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-297">The Navision model is interesting for this exercise due to its very large amount of entities and relationships between them.</span></span>

<span data-ttu-id="1c6b4-298">Jedna verze tohoto modelu velmi velké se vygeneroval s přidružení cizího klíče a druhá se vygeneroval s nezávislé přidružení.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-298">One version of this very large model was generated with Foreign Keys Associations and the other was generated with Independent Associations.</span></span> <span data-ttu-id="1c6b4-299">Potom skončila, jak dlouho trvalo generování zobrazení pro každý model.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-299">We then timed how long it took to generate the views for each model.</span></span> <span data-ttu-id="1c6b4-300">Entity Framework5 test použil metodu GenerateViews() ze třídy EntityViewGenerator ke generování zobrazení, i když Entity Framework 6 test použít metodu GenerateViews() ze třídy objekt StorageMappingItemCollection.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-300">Entity Framework5 test used the GenerateViews() method from class EntityViewGenerator to generate the views, while the Entity Framework 6 test used the GenerateViews() method from class StorageMappingItemCollection.</span></span> <span data-ttu-id="1c6b4-301">To z důvodu kód restrukturalizaci, ke které došlo v základu kódu Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-301">This due to code restructuring that occurred in the Entity Framework 6 codebase.</span></span>

<span data-ttu-id="1c6b4-302">Pomocí Entity Framework 5, generování zobrazení pro model s cizí klíče trvalo 65 minut na testovacím počítači.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-302">Using Entity Framework 5, view generation for the model with Foreign Keys took 65 minutes in a lab machine.</span></span> <span data-ttu-id="1c6b4-303">Není známo jak dlouho by trvalo generování zobrazení pro model, který používá nezávislé přidružení.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-303">It's unknown how long it would have taken to generate the views for the model that used independent associations.</span></span> <span data-ttu-id="1c6b4-304">Ponechali jsme test spuštěného víc než měsíc předtím, než počítač byl restartován v našem testovacím prostředí instalace měsíčních aktualizací.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-304">We left the test running for over a month before the machine was rebooted in our lab to install monthly updates.</span></span>

<span data-ttu-id="1c6b4-305">Pomocí Entity Framework 6, trvalo generování zobrazení pro model s cizí klíče 28 sekundách ve stejném počítači testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-305">Using Entity Framework 6, view generation for the model with Foreign Keys took 28 seconds in the same lab machine.</span></span> <span data-ttu-id="1c6b4-306">Generování zobrazení pro model, který používá nezávislé přidružení trvalo 58 sekund.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-306">View generation for the model that uses Independent Associations took 58 seconds.</span></span> <span data-ttu-id="1c6b4-307">Vylepšení Hotovo a Entity Framework 6 na jeho zobrazení generování kódu znamená, že mnoho projektů, nebude nutné předem vygenerovaných zobrazení zajistit kratší časy spouštění.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-307">The improvements done to Entity Framework 6 on its view generation code mean that many projects won’t need pre-generated views to obtain faster startup times.</span></span>

<span data-ttu-id="1c6b4-308">Je důležité na příspěvek, který předem generování zobrazení v Entity Framework 4 a 5, můžete provést s EDMGen nebo Entity Framework Power Tools.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-308">It’s important to remark that pre-generating views in Entity Framework 4 and 5 can be done with EDMGen or the Entity Framework Power Tools.</span></span> <span data-ttu-id="1c6b4-309">Pro zobrazení Entity Framework 6 generování to provést prostřednictvím Entity Framework Power Tools nebo programově, jak je popsáno v [zobrazení mapování Pre-Generated](~/ef6/fundamentals/performance/pre-generated-views.md).</span><span class="sxs-lookup"><span data-stu-id="1c6b4-309">For Entity Framework 6 view generation can be done via the Entity Framework Power Tools or programmatically as described in [Pre-Generated Mapping Views](~/ef6/fundamentals/performance/pre-generated-views.md).</span></span>

##### <a name="2411-how-to-use-foreign-keys-instead-of-independent-associations"></a><span data-ttu-id="1c6b4-310">2.4.1.1 jak nahrazujícím nezávislé přidružení cizího klíče</span><span class="sxs-lookup"><span data-stu-id="1c6b4-310">2.4.1.1 How to use Foreign Keys instead of Independent Associations</span></span>

<span data-ttu-id="1c6b4-311">Pokud používáte EDMGen nebo návrháři entit v sadě Visual Studio, dostanete FKs ve výchozím nastavení a trvá jen jednoho zaškrtávacího políčka nebo příkazového řádku příznak přepínat mezi FKs a služby ověřování v Internetu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-311">When using EDMGen or the Entity Designer in Visual Studio, you get FKs by default, and it only takes a single checkbox or command line flag to switch between FKs and IAs.</span></span>

<span data-ttu-id="1c6b4-312">Pokud máte velké model Code First pomocí nezávislé přidružení bude mít stejný účinek na generování zobrazení.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-312">If you have a large Code First model, using Independent Associations will have the same effect on view generation.</span></span> <span data-ttu-id="1c6b4-313">I když někteří vývojáři bude předpokládat, že to být zahlcení jejich objektového modelu, můžete tomuto důsledku vyhnout zahrnutím vlastnosti cizího klíče u tříd pro vaše závislé objekty.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-313">You can avoid this impact by including Foreign Key properties on the classes for your dependent objects, though some developers will consider this to be polluting their object model.</span></span> <span data-ttu-id="1c6b4-314">Můžete najít další informace o této skutečnosti do \< http://blog.oneunicorn.com/2011/12/11/whats-the-deal-with-mapping-foreign-keys-using-the-entity-framework/>.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-314">You can find more information on this subject in \<http://blog.oneunicorn.com/2011/12/11/whats-the-deal-with-mapping-foreign-keys-using-the-entity-framework/>.</span></span>

| <span data-ttu-id="1c6b4-315">Při použití</span><span class="sxs-lookup"><span data-stu-id="1c6b4-315">When using</span></span>      | <span data-ttu-id="1c6b4-316">Postup</span><span class="sxs-lookup"><span data-stu-id="1c6b4-316">Do this</span></span>                                                                                                                                                                                                                                                                                                                              |
|:----------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="1c6b4-317">V návrháři entit</span><span class="sxs-lookup"><span data-stu-id="1c6b4-317">Entity Designer</span></span> | <span data-ttu-id="1c6b4-318">Po přidání přidružení mezi dvěma entitami, ujistěte se, že máte referenční omezení.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-318">After adding an association between two entities, make sure you have a referential constraint.</span></span> <span data-ttu-id="1c6b4-319">Referenční omezení řekněte Entity Framework nahrazujícím nezávislé přidružení cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-319">Referential constraints tell Entity Framework to use Foreign Keys instead of Independent Associations.</span></span> <span data-ttu-id="1c6b4-320">Další podrobnosti najdete \< http://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx>.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-320">For additional details visit \<http://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx>.</span></span> |
| <span data-ttu-id="1c6b4-321">EDMGen</span><span class="sxs-lookup"><span data-stu-id="1c6b4-321">EDMGen</span></span>          | <span data-ttu-id="1c6b4-322">Při použití EDMGen ke generování souborů z databáze, cizí klíče budou dodržovat i a přidá do modelu jako takové.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-322">When using EDMGen to generate your files from the database, your Foreign Keys will be respected and added to the model as such.</span></span> <span data-ttu-id="1c6b4-323">Další informace o různých možnostech, které jsou vystavené EDMGen [ http://msdn.microsoft.com/library/bb387165.aspx ](https://msdn.microsoft.com/library/bb387165.aspx).</span><span class="sxs-lookup"><span data-stu-id="1c6b4-323">For more information on the different options exposed by EDMGen visit [http://msdn.microsoft.com/library/bb387165.aspx](https://msdn.microsoft.com/library/bb387165.aspx).</span></span>                           |
| <span data-ttu-id="1c6b4-324">Kód nejprve</span><span class="sxs-lookup"><span data-stu-id="1c6b4-324">Code First</span></span>      | <span data-ttu-id="1c6b4-325">Najdete v části "Konvence vztah" [první konvence kódu](~/ef6/modeling/code-first/conventions/built-in.md) informace o tom, jak při použití Code First obsahovat vlastnosti cizího klíče na závislé objekty.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-325">See the "Relationship Convention" section of the [Code First Conventions](~/ef6/modeling/code-first/conventions/built-in.md) topic for information on how to include foreign key properties on dependent objects when using Code First.</span></span>                                                                                              |

#### <a name="242-moving-your-model-to-a-separate-assembly"></a><span data-ttu-id="1c6b4-326">2.4.2 Přesun do samostatných sestavení modelu</span><span class="sxs-lookup"><span data-stu-id="1c6b4-326">2.4.2 Moving your model to a separate assembly</span></span>

<span data-ttu-id="1c6b4-327">Když váš model je přímo součástí vaší aplikace a generovat zobrazení prostřednictvím události před sestavením nebo šablony T4, generování zobrazení a ověření proběhne pokaždé, když se je znovu sestavit projekt, i když je model nebyl změněn.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-327">When your model is included directly in your application's project and you generate views through a pre-build event or a T4 template, view generation and validation will take place whenever the project is rebuilt, even if the model wasn't changed.</span></span> <span data-ttu-id="1c6b4-328">Pokud přesunout do samostatných sestavení modelu a na něj odkazovat z vaší aplikace, lze provádět jiné změny do vaší aplikace bez nutnosti znovu sestavit projekt, který obsahuje model.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-328">If you move the model to a separate assembly and reference it from your application's project, you can make other changes to your application without needing to rebuild the project containing the model.</span></span>

<span data-ttu-id="1c6b4-329">*Poznámka:* při přesunu modelu do samostatných sestavení, nezapomeňte si zkopírovat připojovací řetězce pro model do konfiguračního souboru aplikace projektu klienta.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-329">*Note:*  when moving your model to separate assemblies remember to copy the connection strings for the model into the application configuration file of the client project.</span></span>

#### <a name="243-disable-validation-of-an-edmx-based-model"></a><span data-ttu-id="1c6b4-330">2.4.3 zakážete ověřování modelu bázi edmx</span><span class="sxs-lookup"><span data-stu-id="1c6b4-330">2.4.3 Disable validation of an edmx-based model</span></span>

<span data-ttu-id="1c6b4-331">Modely EDMX se ověřují v době kompilace, i když je model beze změny.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-331">EDMX models are validated at compile time, even if the model is unchanged.</span></span> <span data-ttu-id="1c6b4-332">Pokud model je už potvrzená, můžete potlačit ověření v době kompilace tak, že nastavíte vlastnost "Ověřit při sestavení" v okně Vlastnosti na hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-332">If your model has already been validated, you can suppress validation at compile time by setting the "Validate on Build" property to false in the properties window.</span></span> <span data-ttu-id="1c6b4-333">Při změně mapování nebo modelu, můžete dočasně znovu povolit ověření zkontrolujte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-333">When you change your mapping or model, you can temporarily re-enable validation to verify your changes.</span></span>

<span data-ttu-id="1c6b4-334">Všimněte si, že byla provedena vylepšení výkonu Entity Framework Designer pro Entity Framework 6 a náklady "ověřit při sestavení" jsou mnohem nižší než v předchozích verzích návrháře.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-334">Note that performance improvements were made to the Entity Framework Designer for Entity Framework 6, and the cost of the “Validate on Build” is much lower than in previous versions of the designer.</span></span>

## <a name="3-caching-in-the-entity-framework"></a><span data-ttu-id="1c6b4-335">3 ukládání do mezipaměti v Entity Framework</span><span class="sxs-lookup"><span data-stu-id="1c6b4-335">3 Caching in the Entity Framework</span></span>

<span data-ttu-id="1c6b4-336">Entity Framework má následující formy integrované ukládání do mezipaměti:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-336">Entity Framework has the following forms of caching built-in:</span></span>

1.  <span data-ttu-id="1c6b4-337">Objekt, ukládání do mezipaměti – objektu ObjectStateManager integrované do ObjectContext instance sleduje v paměti objektů, které byly načteny pomocí této instance.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-337">Object caching – the ObjectStateManager built into an ObjectContext instance keeps track in memory of the objects that have been retrieved using that instance.</span></span> <span data-ttu-id="1c6b4-338">Toto je také označovaný jako první úrovně mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-338">This is also known as first-level cache.</span></span>
2.  <span data-ttu-id="1c6b4-339">Mezipaměti plánu dotazu – opětovné použití příkaz generované úložiště, pokud je dotaz proveden více než jednou.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-339">Query Plan Caching - reusing the generated store command when a query is executed more than once.</span></span>
3.  <span data-ttu-id="1c6b4-340">Metadata ukládání do mezipaměti – sdílení metadata pro model v různých připojení do stejného modelu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-340">Metadata caching - sharing the metadata for a model across different connections to the same model.</span></span>

<span data-ttu-id="1c6b4-341">Kromě mezipamětí, které EF poskytuje hned po spuštění zvláštní druh zprostředkovatele dat ADO.NET, označované jako zprostředkovatele zabalení lze také rozšířit Entity Framework s mezipamětí pro výsledky získané z databáze, označované také jako ukládání do mezipaměti druhé úrovně.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-341">Besides the caches that EF provides out of the box, a special kind of ADO.NET data provider known as a wrapping provider can also be used to extend Entity Framework with a cache for the results retrieved from the database, also known as second-level caching.</span></span>

### <a name="31-object-caching"></a><span data-ttu-id="1c6b4-342">3.1 objektu ukládání do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="1c6b4-342">3.1 Object Caching</span></span>

<span data-ttu-id="1c6b4-343">Ve výchozím nastavení když entity se vrátí ve výsledcích dotazu, těsně před plánovaným začátkem EF bude realizována, objekt ObjectContext zkontroluje Pokud entity se stejným klíčem již byla načtena do jeho objektu ObjectStateManager.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-343">By default when an entity is returned in the results of a query, just before EF materializes it, the ObjectContext will check if an entity with the same key has already been loaded into its ObjectStateManager.</span></span> <span data-ttu-id="1c6b4-344">Pokud už entity se stejnými klíči EF jej zahrnout do výsledků dotazu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-344">If an entity with the same keys is already present EF will include it in the results of the query.</span></span> <span data-ttu-id="1c6b4-345">I když EF stále vydá dotaz na databázi, můžete toto chování obcházet velkou část náklady materializaci entita více než jednou.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-345">Although EF will still issue the query against the database, this behavior can bypass much of the cost of materializing the entity multiple times.</span></span>

#### <a name="311-getting-entities-from-the-object-cache-using-dbcontext-find"></a><span data-ttu-id="1c6b4-346">3.1.1 získání entity z mezipaměti objekt DbContext hledání</span><span class="sxs-lookup"><span data-stu-id="1c6b4-346">3.1.1 Getting entities from the object cache using DbContext Find</span></span>

<span data-ttu-id="1c6b4-347">Na rozdíl od pravidelných dotaz metodu najít v DbSet (zahrnutá v EF 4.1 poprvé rozhraní API) provede vyhledávání v paměti před i zadání dotazu na databázi.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-347">Unlike a regular query, the Find method in DbSet (APIs included for the first time in EF 4.1) will perform a search in memory before even issuing the query against the database.</span></span> <span data-ttu-id="1c6b4-348">Je důležité si uvědomit, že dvě různé instance ObjectContext bude mít dva různé instance objektu ObjectStateManager, což znamená, že mají samostatný objekt mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-348">It’s important to note that two different ObjectContext instances will have two different ObjectStateManager instances, meaning that they have separate object caches.</span></span>

<span data-ttu-id="1c6b4-349">Hledání používá hodnotu primárního klíče pokusí se najít entitu sledován pomocí funkce kontextu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-349">Find uses the primary key value to attempt to find an entity tracked by the context.</span></span> <span data-ttu-id="1c6b4-350">Entitu se nenachází v kontextu pak dotaz provést, který se vyhodnotí na databázi a vrátí se hodnota null, pokud entita nebyl nalezen v kontextu nebo v databázi.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-350">If the entity is not in the context then a query will be executed and evaluated against the database, and null is returned if the entity is not found in the context or in the database.</span></span> <span data-ttu-id="1c6b4-351">Všimněte si, že hledání vrací entity, které byly přidány do kontextu, ale ještě nebyly uloženy do databáze.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-351">Note that Find also returns entities that have been added to the context but have not yet been saved to the database.</span></span>

<span data-ttu-id="1c6b4-352">Existuje posouzení výkonu, jež mají být provedeny při hledání.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-352">There is a performance consideration to be taken when using Find.</span></span> <span data-ttu-id="1c6b4-353">Volání této metody ve výchozím nastavení spustí ověření mezipaměti objektů zjišťovat změny, které jsou stále čeká na potvrzení změn do databáze.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-353">Invocations to this method by default will trigger a validation of the object cache in order to detect changes that are still pending commit to the database.</span></span> <span data-ttu-id="1c6b4-354">Tento proces může být velmi náročná, pokud existuje velký počet objektů v mezipaměti objektů nebo velkého objektu grafu se přidávají do mezipaměti objektů, ale můžete také zakázat.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-354">This process can be very expensive if there are a very large number of objects in the object cache or in a large object graph being added to the object cache, but it can also be disabled.</span></span> <span data-ttu-id="1c6b4-355">V některých případech může přes řád velké rozdíly v volání najít metodu, pokud zakážete automatické zjišťování vnímat změny.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-355">In certain cases, you may perceive over an order of magnitude of difference in calling the Find method when you disable auto detect changes.</span></span> <span data-ttu-id="1c6b4-356">Ještě druhý řádově bude považován, když ve skutečnosti je objekt v mezipaměti a když má objekt, který se má načíst z databáze.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-356">Yet a second order of magnitude is perceived when the object actually is in the cache versus when the object has to be retrieved from the database.</span></span> <span data-ttu-id="1c6b4-357">Tady je ukázka grafu s měření pomocí některé z našich microbenchmarks vyjádřené v milisekundách, s načtením 5000 entit:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-357">Here is an example graph with measurements taken using some of our microbenchmarks, expressed in milliseconds, with a load of 5000 entities:</span></span>

<span data-ttu-id="1c6b4-358">![Hodnota na logaritmické stupnici .NET 4.5](~/ef6/media/net45logscale.png ".NET 4.5 – logaritmické měřítko")</span><span class="sxs-lookup"><span data-stu-id="1c6b4-358">![.NET 4.5 logarithmic scale](~/ef6/media/net45logscale.png ".NET 4.5 - logarithmic scale")</span></span>

<span data-ttu-id="1c6b4-359">Příklad najít změnami auto-detect zakázané:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-359">Example of Find with auto-detect changes disabled:</span></span>

``` csharp
    context.Configuration.AutoDetectChangesEnabled = false;
    var product = context.Products.Find(productId);
    context.Configuration.AutoDetectChangesEnabled = true;
    ...
```

<span data-ttu-id="1c6b4-360">Co je nutné zvážit při použití metody najít je:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-360">What you have to consider when using the Find method is:</span></span>

1.  <span data-ttu-id="1c6b4-361">Pokud objekt není v mezipaměti jsou negovat výhody hledání, ale syntaxe je ještě jednodušší než dotaz podle klíče.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-361">If the object is not in the cache the benefits of Find are negated, but the syntax is still simpler than a query by key.</span></span>
2.  <span data-ttu-id="1c6b4-362">Pokud automatické zjištění změn je povoleno náklady na metodu Find může zvýšit jeden řádově, nebo ještě větší v závislosti na složitosti modelu a množství entit ve vaší mezipaměti objektů.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-362">If auto detect changes is enabled the cost of the Find method may increase by one order of magnitude, or even more depending on the complexity of your model and the amount of entities in your object cache.</span></span>

<span data-ttu-id="1c6b4-363">Také Uvědomte si, že najít pouze vrátí entitu, kterou jste hledali a neodstraní automaticky načte jeho přidružených entit Pokud nejsou již v mezipaměti objektů.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-363">Also, keep in mind that Find only returns the entity you are looking for and it does not automatically loads its associated entities if they are not already in the object cache.</span></span> <span data-ttu-id="1c6b4-364">Pokud potřebujete získat související entity, můžete pomocí klíče se nemůžou dočkat, až načítání dotazu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-364">If you need to retrieve associated entities, you can use a query by key with eager loading.</span></span> <span data-ttu-id="1c6b4-365">Další informace najdete v části **8.1 opožděné načtení vs. Předběžné načítání**.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-365">For more information see **8.1 Lazy Loading vs. Eager Loading**.</span></span>

#### <a name="312-performance-issues-when-the-object-cache-has-many-entities"></a><span data-ttu-id="1c6b4-366">3.1.2 Pokud objekt mezipaměti má mnoho entit problémů s výkonem</span><span class="sxs-lookup"><span data-stu-id="1c6b4-366">3.1.2 Performance issues when the object cache has many entities</span></span>

<span data-ttu-id="1c6b4-367">Objekt mezipaměti pomáhá zvýšit celkovou rychlost reakce Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-367">The object cache helps to increase the overall responsiveness of Entity Framework.</span></span> <span data-ttu-id="1c6b4-368">Pokud objekt mezipaměti má velmi velký objem entity načíst že ji může mít vliv na určité operace, třeba přidat, odebrat, najít položku, SaveChanges a další.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-368">However, when the object cache has a very large amount of entities loaded it may affect certain operations such as Add, Remove, Find, Entry, SaveChanges and more.</span></span> <span data-ttu-id="1c6b4-369">Konkrétně se operace, které aktivují volání metoda DetectChanges negativně ovlivní mezipaměti velmi velké objekty.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-369">In particular, operations that trigger a call to DetectChanges will be negatively affected by very large object caches.</span></span> <span data-ttu-id="1c6b4-370">Metoda DetectChanges synchronizuje grafu objektu s objekt state manager a jeho výkonu se určuje podle velikosti objektu grafu přímo.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-370">DetectChanges synchronizes the object graph with the object state manager and its performance will determined directly by the size of the object graph.</span></span> <span data-ttu-id="1c6b4-371">Další informace o metoda DetectChanges najdete v tématu [sledování změn v entity objektů POCO](https://msdn.microsoft.com/library/dd456848.aspx).</span><span class="sxs-lookup"><span data-stu-id="1c6b4-371">For more information about DetectChanges, see [Tracking Changes in POCO Entities](https://msdn.microsoft.com/library/dd456848.aspx).</span></span>

<span data-ttu-id="1c6b4-372">Pokud používáte Entity Framework 6, jsou vývojáři schopni volání AddRange a RemoveRange přímo na DbSet, namísto iterace v kolekci a přidat volání jednou za instanci.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-372">When using Entity Framework 6, developers are able to call AddRange and RemoveRange directly on a DbSet, instead of iterating on a collection and calling Add once per instance.</span></span> <span data-ttu-id="1c6b4-373">Výhodou použití rozsahu metody je, že náklady na metoda DetectChanges pouze platí jednou pro celou sadu entit, na rozdíl od jednou za každé přidání entity.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-373">The advantage of using the range methods is that the cost of DetectChanges is only paid once for the entire set of entities as opposed to once per each added entity.</span></span>

### <a name="32-query-plan-caching"></a><span data-ttu-id="1c6b4-374">3.2 dotazu do mezipaměti plánu</span><span class="sxs-lookup"><span data-stu-id="1c6b4-374">3.2 Query Plan Caching</span></span>

<span data-ttu-id="1c6b4-375">Prvním je dotaz proveden, prochází přes vnitřní plán kompilátor převede koncepční dotaz na příkaz úložiště (třeba T-SQL, který se spustí při spuštění SQL serveru).</span><span class="sxs-lookup"><span data-stu-id="1c6b4-375">The first time a query is executed, it goes through the internal plan compiler to translate the conceptual query into the store command (for example, the T-SQL which is executed when run against SQL Server).</span></span>  <span data-ttu-id="1c6b4-376">Pokud je povoleno ukládání do mezipaměti plánu dotazu, při příštím dotaz je proveden úložišti je příkaz načíst přímo z mezipaměti plánu dotazu pro provádění bez použití kompilátoru plánu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-376">If query plan caching is enabled, the next time the query is executed the store command is retrieved directly from the query plan cache for execution, bypassing the plan compiler.</span></span>

<span data-ttu-id="1c6b4-377">Mezipaměti plánu dotazu je sdílen mezi instance ObjectContext v rámci téže třídy AppDomain.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-377">The query plan cache is shared across ObjectContext instances within the same AppDomain.</span></span> <span data-ttu-id="1c6b4-378">Není nutné pro udržení instance ObjectContext, abyste využili výhod ukládání do mezipaměti plánu dotazu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-378">You don't need to hold onto an ObjectContext instance to benefit from query plan caching.</span></span>

#### <a name="321-some-notes-about-query-plan-caching"></a><span data-ttu-id="1c6b4-379">3.2.1 několik poznámek o dotazu plánování ukládání do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="1c6b4-379">3.2.1 Some notes about Query Plan Caching</span></span>

-   <span data-ttu-id="1c6b4-380">Mezipaměti plánu dotazu jsou sdílena pro všechny typy dotazů: Entity SQL, technologii LINQ to Entities a CompiledQuery objekty.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-380">The query plan cache is shared for all query types: Entity SQL, LINQ to Entities, and CompiledQuery objects.</span></span>
-   <span data-ttu-id="1c6b4-381">Ve výchozím nastavení ukládání do mezipaměti plánu dotazu je povoleno pro Entity SQL dotazy, zda provést prostřednictvím EntityCommand nebo ObjectQuery.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-381">By default, query plan caching is enabled for Entity SQL queries, whether executed through an EntityCommand or through an ObjectQuery.</span></span> <span data-ttu-id="1c6b4-382">Je také ve výchozím nastavení zapnutá pro LINQ dotazy entity v Entity Framework v rozhraní .NET 4.5 a Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="1c6b4-382">It is also enabled by default for LINQ to Entities queries in Entity Framework on .NET 4.5, and in Entity Framework 6</span></span>
    -   <span data-ttu-id="1c6b4-383">Ukládání do mezipaměti plánu dotazu je možné zakázat nastavením vlastnosti EnablePlanCaching (na EntityCommand nebo ObjectQuery) na hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-383">Query plan caching can be disabled by setting the EnablePlanCaching property (on EntityCommand or ObjectQuery) to false.</span></span> <span data-ttu-id="1c6b4-384">Příklad:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-384">For example:</span></span>
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
-   <span data-ttu-id="1c6b4-385">Pro parametrizované dotazy se změna hodnoty parametru stále dostanou dotazu z mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-385">For parameterized queries, changing the parameter's value will still hit the cached query.</span></span> <span data-ttu-id="1c6b4-386">Ale změna omezujících vlastností parametrů (například velikost, přesnost nebo škálování) se dostanou jinou položku v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-386">But changing a parameter's facets (for example, size, precision, or scale) will hit a different entry in the cache.</span></span>
-   <span data-ttu-id="1c6b4-387">Pokud používáte Entity SQL, řetězec dotazu je součástí klíče.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-387">When using Entity SQL, the query string is part of the key.</span></span> <span data-ttu-id="1c6b4-388">Změna dotazu vůbec způsobí jiný mezipaměť, i v případě, dotazy jsou funkčně ekvivalentní.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-388">Changing the query at all will result in different cache entries, even if the queries are functionally equivalent.</span></span> <span data-ttu-id="1c6b4-389">To zahrnuje změny v malých a velkých písmen nebo prázdný znak.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-389">This includes changes to casing or whitespace.</span></span>
-   <span data-ttu-id="1c6b4-390">Při použití LINQ dotaz zpracování k vygenerování součástí klíče.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-390">When using LINQ, the query is processed to generate a part of the key.</span></span> <span data-ttu-id="1c6b4-391">Nahradit výraz LINQ proto vygeneruje za jiný klíč.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-391">Changing the LINQ expression will therefore generate a different key.</span></span>
-   <span data-ttu-id="1c6b4-392">Další technická omezení můžou vztahovat; Další podrobnosti najdete v Autocompiled dotazy.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-392">Other technical limitations may apply; see Autocompiled Queries for more details.</span></span>

#### <a name="322------cache-eviction-algorithm"></a><span data-ttu-id="1c6b4-393">3.2.2 mezipaměti vyřazení algoritmus</span><span class="sxs-lookup"><span data-stu-id="1c6b4-393">3.2.2      Cache eviction algorithm</span></span>

<span data-ttu-id="1c6b4-394">Ke zjištění, jak funguje interním algoritmu a vám pomůžou přijít na to, když chcete povolit nebo zakázat dotazu ukládání do mezipaměti plánu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-394">Understanding how the internal algorithm works will help you figure out when to enable or disable query plan caching.</span></span> <span data-ttu-id="1c6b4-395">Algoritmus vyčištění vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-395">The cleanup algorithm is as follows:</span></span>

1.  <span data-ttu-id="1c6b4-396">Jakmile mezipaměti obsahuje stanovený počet položek (800), začneme časovač, pravidelně (jednou za minutu) přesune do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-396">Once the cache contains a set number of entries (800), we start a timer that periodically (once-per-minute) sweeps the cache.</span></span>
2.  <span data-ttu-id="1c6b4-397">Během změny mezipaměti položky jsou odebrány z mezipaměti LFRU (nejméně často – nedávno použité) základ.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-397">During cache sweeps, entries are removed from the cache on a LFRU (Least frequently – recently used) basis.</span></span> <span data-ttu-id="1c6b4-398">Tento algoritmus zohledňuje počet průchodů a stáří při rozhodování o tom, které položky jsou vysunout.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-398">This algorithm takes both hit count and age into account when deciding which entries are ejected.</span></span>
3.  <span data-ttu-id="1c6b4-399">Na konci každé vyčištění mezipaměti se mezipaměť znovu obsahuje 800 položky.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-399">At the end of each cache sweep, the cache again contains 800 entries.</span></span>

<span data-ttu-id="1c6b4-400">Všechny položky mezipaměti se zachází stejně při určení položek k vyřazení.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-400">All cache entries are treated equally when determining which entries to evict.</span></span> <span data-ttu-id="1c6b4-401">To znamená, že příkaz úložiště pro CompiledQuery má stejnou šanci vyřazení jako příkaz úložiště dotazu Entity SQL.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-401">This means the store command for a CompiledQuery has the same chance of eviction as the store command for an Entity SQL query.</span></span>

<span data-ttu-id="1c6b4-402">Všimněte si, že se spustí se časovač vyřazení mezipaměti v, když nejsou 800 entity v mezipaměti, ale mezipaměti je pouze, jež jsou 60 sekund, po spuštění tohoto časovače.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-402">Note that the cache eviction timer is kicked in when there are 800 entities in the cache, but the cache is only swept 60 seconds after this timer is started.</span></span> <span data-ttu-id="1c6b4-403">To znamená, že až na 60 sekund mezipaměti může zvětšit poměrně velká.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-403">That means that for up to 60 seconds your cache may grow to be quite large.</span></span>

#### <a name="323-------test-metrics-demonstrating-query-plan-caching-performance"></a><span data-ttu-id="1c6b4-404">3.2.3 testování metriky demonstrace plán dotazu, ukládání do mezipaměti výkonu</span><span class="sxs-lookup"><span data-stu-id="1c6b4-404">3.2.3       Test Metrics demonstrating query plan caching performance</span></span>

<span data-ttu-id="1c6b4-405">Abychom si předvedli efekt plán dotazu do mezipaměti na výkon vaší aplikace, jsme provedli test kde jsme spouštěli počet dotazů Entity SQL proti Navision modelu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-405">To demonstrate the effect of query plan caching on your application's performance, we performed a test where we executed a number of Entity SQL queries against the Navision model.</span></span> <span data-ttu-id="1c6b4-406">Viz dodatek popis modelu Navision a typy dotazů, které byly spuštěny.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-406">See the appendix for a description of the Navision model and the types of queries which were executed.</span></span> <span data-ttu-id="1c6b4-407">V tomto testu jsme první iteraci v rámci seznamu dotazů a po spuštění každé z nich přidat do mezipaměti (Pokud je povoleno ukládání do mezipaměti).</span><span class="sxs-lookup"><span data-stu-id="1c6b4-407">In this test, we first iterate through the list of queries and execute each one once to add them to the cache (if caching is enabled).</span></span> <span data-ttu-id="1c6b4-408">Tento krok je untimed.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-408">This step is untimed.</span></span> <span data-ttu-id="1c6b4-409">V dalším kroku můžeme přejít do režimu spánku hlavního vlákna pro více než 60 sekund mezipaměti cílit na konkrétní uskutečnit; Nakonec jsme iterovat přes seznam 2. čas ke spuštění dotazy uložené v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-409">Next, we sleep the main thread for over 60 seconds to allow cache sweeping to take place; finally, we iterate through the list a 2nd time to execute the cached queries.</span></span> <span data-ttu-id="1c6b4-410">Kromě toho má SQL Server mezipaměti plánu vyprázdní před provedením každého sadu dotazů tak, aby kolikrát získáme přesně odrážet výhodu Dal mezipaměti plánu dotazu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-410">Additionally, he SQL Server plan cache is flushed before each set of queries is executed so that the times we obtain accurately reflect the benefit given by the query plan cache.</span></span>

##### <a name="3231-------test-results"></a><span data-ttu-id="1c6b4-411">3.2.3.1 výsledky testů</span><span class="sxs-lookup"><span data-stu-id="1c6b4-411">3.2.3.1       Test Results</span></span>

| <span data-ttu-id="1c6b4-412">Test</span><span class="sxs-lookup"><span data-stu-id="1c6b4-412">Test</span></span>                                                                   | <span data-ttu-id="1c6b4-413">EF5 žádná mezipaměť</span><span class="sxs-lookup"><span data-stu-id="1c6b4-413">EF5 no cache</span></span> | <span data-ttu-id="1c6b4-414">EF5 do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="1c6b4-414">EF5 cached</span></span> | <span data-ttu-id="1c6b4-415">EF6 žádná mezipaměť</span><span class="sxs-lookup"><span data-stu-id="1c6b4-415">EF6 no cache</span></span> | <span data-ttu-id="1c6b4-416">EF6 do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="1c6b4-416">EF6 cached</span></span> |
|:-----------------------------------------------------------------------|:-------------|:-----------|:-------------|:-----------|
| <span data-ttu-id="1c6b4-417">Výčet všech 18723 dotazů</span><span class="sxs-lookup"><span data-stu-id="1c6b4-417">Enumerating all 18723 queries</span></span>                                          | <span data-ttu-id="1c6b4-418">124</span><span class="sxs-lookup"><span data-stu-id="1c6b4-418">124</span></span>          | <span data-ttu-id="1c6b4-419">125.4</span><span class="sxs-lookup"><span data-stu-id="1c6b4-419">125.4</span></span>      | <span data-ttu-id="1c6b4-420">124.3</span><span class="sxs-lookup"><span data-stu-id="1c6b4-420">124.3</span></span>        | <span data-ttu-id="1c6b4-421">125.3</span><span class="sxs-lookup"><span data-stu-id="1c6b4-421">125.3</span></span>      |
| <span data-ttu-id="1c6b4-422">Jak se vyhnout oblouku (pouze první 800 dotazy, bez ohledu na složitost)</span><span class="sxs-lookup"><span data-stu-id="1c6b4-422">Avoiding sweep (just the first 800 queries, regardless of complexity)</span></span>  | <span data-ttu-id="1c6b4-423">41.7</span><span class="sxs-lookup"><span data-stu-id="1c6b4-423">41.7</span></span>         | <span data-ttu-id="1c6b4-424">5.5</span><span class="sxs-lookup"><span data-stu-id="1c6b4-424">5.5</span></span>        | <span data-ttu-id="1c6b4-425">40.5</span><span class="sxs-lookup"><span data-stu-id="1c6b4-425">40.5</span></span>         | <span data-ttu-id="1c6b4-426">5.4</span><span class="sxs-lookup"><span data-stu-id="1c6b4-426">5.4</span></span>        |
| <span data-ttu-id="1c6b4-427">Právě AggregatingSubtotals dotazy (178 celkem – nemusí tedy přenášet úklidu)</span><span class="sxs-lookup"><span data-stu-id="1c6b4-427">Just the AggregatingSubtotals queries (178 total - which avoids sweep)</span></span> | <span data-ttu-id="1c6b4-428">39.5</span><span class="sxs-lookup"><span data-stu-id="1c6b4-428">39.5</span></span>         | <span data-ttu-id="1c6b4-429">4.5</span><span class="sxs-lookup"><span data-stu-id="1c6b4-429">4.5</span></span>        | <span data-ttu-id="1c6b4-430">38.1</span><span class="sxs-lookup"><span data-stu-id="1c6b4-430">38.1</span></span>         | <span data-ttu-id="1c6b4-431">4.6</span><span class="sxs-lookup"><span data-stu-id="1c6b4-431">4.6</span></span>        |

<span data-ttu-id="1c6b4-432">*Celou dobu v sekundách.*</span><span class="sxs-lookup"><span data-stu-id="1c6b4-432">*All times in seconds.*</span></span>

<span data-ttu-id="1c6b4-433">Osobnostní - při provádění mnoho různých dotazů (například dynamicky vytvořené dotazy), ukládání do mezipaměti nepomůže a výsledné vyprazdňování mezipaměti můžete ponechat dotazy, které je výhodná maximum z jejího použití ukládání do mezipaměti plánu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-433">Moral - when executing lots of distinct queries (for example,  dynamically created queries), caching doesn't help and the resulting flushing of the cache can keep the queries that would benefit the most from plan caching from actually using it.</span></span>

<span data-ttu-id="1c6b4-434">Dotazy AggregatingSubtotals jsou nejsložitější dotazů, které jsme testovali s.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-434">The AggregatingSubtotals queries are the most complex of the queries we tested with.</span></span> <span data-ttu-id="1c6b4-435">Podle očekávání, tím složitější je dotaz další výhody, zobrazí se do mezipaměti plánu dotazu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-435">As expected, the more complex the query is, the more benefit you will see from query plan caching.</span></span>

<span data-ttu-id="1c6b4-436">Protože CompiledQuery je ve skutečnosti dotaz LINQ s svůj plán v mezipaměti, porovnání CompiledQuery oproti ekvivalentní Entity SQL dotazu by měl mít podobné výsledky.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-436">Because a CompiledQuery is really a LINQ query with its plan cached, the comparison of a CompiledQuery versus the equivalent Entity SQL query should have similar results.</span></span> <span data-ttu-id="1c6b4-437">Ve skutečnosti Pokud aplikace obsahuje velké množství dynamických dotazů Entity SQL, naplnění mezipaměti s dotazy také fakticky způsobí CompiledQueries "dekompilovat", když budou zapsány z mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-437">In fact, if an app has lots of dynamic Entity SQL queries, filling the cache with queries will also effectively cause CompiledQueries to “decompile” when they are flushed from the cache.</span></span> <span data-ttu-id="1c6b4-438">V tomto scénáři se dá vylepšit výkon tím, že zakážete, ukládání do mezipaměti na dynamické dotazy k určení priority CompiledQueries.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-438">In this scenario, performance may be improved by disabling caching on the dynamic queries to prioritize the CompiledQueries.</span></span> <span data-ttu-id="1c6b4-439">Ještě lepší je samozřejmě, bude pro přepsání aplikace pro použití parametrizovaných dotazů místo dynamických dotazů.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-439">Better yet, of course, would be to rewrite the app to use parameterized queries instead of dynamic queries.</span></span>

### <a name="33-using-compiledquery-to-improve-performance-with-linq-queries"></a><span data-ttu-id="1c6b4-440">3.3 pomocí CompiledQuery ke zlepšení výkonu s dotazy LINQ</span><span class="sxs-lookup"><span data-stu-id="1c6b4-440">3.3 Using CompiledQuery to improve performance with LINQ queries</span></span>

<span data-ttu-id="1c6b4-441">Naše testy zjistí, že pomocí CompiledQuery přinést výhody 7 % přes autocompiled LINQ dotazy; To znamená, že strávíte 7 % méně času provádění kódu v sadě Entity Framework; neznamená to, že vaše aplikace bude rychlejší % 7.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-441">Our tests indicate that using CompiledQuery can bring a benefit of 7% over autocompiled LINQ queries; this means that you’ll spend 7% less time executing code from the Entity Framework stack; it does not mean your application will be 7% faster.</span></span> <span data-ttu-id="1c6b4-442">Obecně řečeno náklady na psaní a údržbě CompiledQuery objekty v EF 5.0 nemusí být vhodné potíže s ve srovnání s výhody.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-442">Generally speaking, the cost of writing and maintaining CompiledQuery objects in EF 5.0 may not be worth the trouble when compared to the benefits.</span></span> <span data-ttu-id="1c6b4-443">Vaše vzdálenost mohou lišit, tak výkon tuto možnost, pokud váš projekt vyžaduje další nasdílení změn.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-443">Your mileage may vary, so exercise this option if your project requires the extra push.</span></span> <span data-ttu-id="1c6b4-444">Všimněte si, že CompiledQueries jsou pouze kompatibilní s odvozenému objektu ObjectContext modely a není kompatibilní s modely odvozené DbContext.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-444">Note that CompiledQueries are only compatible with ObjectContext-derived models, and not compatible with DbContext-derived models.</span></span>

<span data-ttu-id="1c6b4-445">Další informace o vytváření a vyvolávání CompiledQuery najdete v tématu [zkompilován dotazy (LINQ to Entities)](https://msdn.microsoft.com/library/bb896297.aspx).</span><span class="sxs-lookup"><span data-stu-id="1c6b4-445">For more information on creating and invoking a CompiledQuery, see [Compiled Queries (LINQ to Entities)](https://msdn.microsoft.com/library/bb896297.aspx).</span></span>

<span data-ttu-id="1c6b4-446">Existují dvě okolnosti, které je třeba provést při použití CompiledQuery, a to nutnost používat statické instance a potíží, že budou mít s skládání.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-446">There are two considerations you have to take when using a CompiledQuery, namely the requirement to use static instances and the problems they have with composability.</span></span> <span data-ttu-id="1c6b4-447">Následuje podrobné vysvětlení těchto dvou důležitých informací.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-447">Here follows an in-depth explanation of these two considerations.</span></span>

#### <a name="331-------use-static-compiledquery-instances"></a><span data-ttu-id="1c6b4-448">3.3.1 použít statické instance CompiledQuery</span><span class="sxs-lookup"><span data-stu-id="1c6b4-448">3.3.1       Use static CompiledQuery instances</span></span>

<span data-ttu-id="1c6b4-449">Protože je kompilování dotazu LINQ časově náročný proces, nechceme to pokaždé, když budeme potřebovat načíst data z databáze.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-449">Since compiling a LINQ query is a time-consuming process, we don’t want to do it every time we need to fetch data from the database.</span></span> <span data-ttu-id="1c6b4-450">Instance CompiledQuery umožňují jednou kompilace a spuštění více než jednou, ale musí být opatrní a nákupem znovu použít stejnou instanci CompiledQuery pokaždé, když místo kompilaci znovu a znovu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-450">CompiledQuery instances allow you to compile once and run multiple times, but you have to be careful and procure to re-use the same CompiledQuery instance every time instead of compiling it over and over again.</span></span> <span data-ttu-id="1c6b4-451">Použití statických členů k ukládání instancí CompiledQuery nezbytná.; jinak se nezobrazí žádnou výhodu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-451">The use of static members to store the CompiledQuery instances becomes necessary; otherwise you won’t see any benefit.</span></span>

<span data-ttu-id="1c6b4-452">Předpokládejme například, že vaše stránka obsahuje následující tělo metody pro zpracování zobrazení produkty pro vybrané kategorie:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-452">For example, suppose your page has the following method body to handle displaying the products for the selected category:</span></span>

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

<span data-ttu-id="1c6b4-453">V tomto případě vytvoříte novou instanci CompiledQuery průběžně pokaždé, když je volána metoda.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-453">In this case, you will create a new CompiledQuery instance on the fly every time the method is called.</span></span> <span data-ttu-id="1c6b4-454">Místo toho přinese zlepšení výkonu načtením příkaz úložiště z mezipaměti plánu dotazu, abyste CompiledQuery projdou kompilátoru plán pokaždé, když je vytvořena nová instance.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-454">Instead of seeing performance benefits by retrieving the store command from the query plan cache, the CompiledQuery will go through the plan compiler every time a new instance is created.</span></span> <span data-ttu-id="1c6b4-455">Ve skutečnosti je bude možné zahlcení vaší mezipaměti plánu dotazu s novým záznamem CompiledQuery pokaždé, když je volána metoda.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-455">In fact, you will be polluting your query plan cache with a new CompiledQuery entry every time the method is called.</span></span>

<span data-ttu-id="1c6b4-456">Místo toho chcete vytvořit instanci statické v kompilovaném dotazu, takže pokaždé, když je volána metoda vyvoláváte stejný zkompilovaný dotaz.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-456">Instead, you want to create a static instance of the compiled query, so you are invoking the same compiled query every time the method is called.</span></span> <span data-ttu-id="1c6b4-457">Jeden ze způsobů, jak to tedy přidáním CompiledQuery instance jako člen objektu kontextu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-457">One way to so this is by adding the CompiledQuery instance as a member of your object context.</span></span>  <span data-ttu-id="1c6b4-458">Potom můžete provést akce trochu čistější díky přístupu CompiledQuery s využitím pomocnou metodu:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-458">You can then make things a little cleaner by accessing the CompiledQuery through a helper method:</span></span>

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

<span data-ttu-id="1c6b4-459">Tuto metodu helper by spustit následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-459">This helper method would be invoked as follows:</span></span>

``` csharp
    this.productsGrid.DataSource = context.GetProductsForCategory(selectedCategory);
```

#### <a name="332-------composing-over-a-compiledquery"></a><span data-ttu-id="1c6b4-460">3.3.2 sestavování přes CompiledQuery</span><span class="sxs-lookup"><span data-stu-id="1c6b4-460">3.3.2       Composing over a CompiledQuery</span></span>

<span data-ttu-id="1c6b4-461">Je velmi užitečné; možnost compose přes jakýkoli dotaz LINQ k tomuto účelu můžete jednoduše vyvolat metodu po položka IQueryable, jako *Skip()* nebo *Count()*.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-461">The ability to compose over any LINQ query is extremely useful; to do this, you simply invoke a method after the IQueryable such as *Skip()* or *Count()*.</span></span> <span data-ttu-id="1c6b4-462">Však to tedy v podstatě vrátí nový objekt IQueryable.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-462">However, doing so essentially returns a new IQueryable object.</span></span> <span data-ttu-id="1c6b4-463">Není nutné nic zastavit technicky z sestavování přes CompiledQuery, díky tomu budou generování nového IQueryable objektu, který vyžaduje procházející kompilátoru plán znovu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-463">While there’s nothing to stop you technically from composing over a CompiledQuery, doing so will cause the generation of a new IQueryable object that requires passing through the plan compiler again.</span></span>

<span data-ttu-id="1c6b4-464">Některé součásti se využívají složené IQueryable objekty umožňují pokročilé funkce.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-464">Some components will make use of composed IQueryable objects to enable advanced functionality.</span></span> <span data-ttu-id="1c6b4-465">Například ASP. NET prvku GridView může být vázán na data IQueryable objektu prostřednictvím vlastnosti metoda SelectMethod.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-465">For example, ASP.NET’s GridView can be data-bound to an IQueryable object via the SelectMethod property.</span></span> <span data-ttu-id="1c6b4-466">Přes tento objekt IQueryable umožňuje řazení a stránkování datovém modelu se pak compose prvku GridView.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-466">The GridView will then compose over this IQueryable object to allow sorting and paging over the data model.</span></span> <span data-ttu-id="1c6b4-467">Jak vidíte, pomocí prvku GridView CompiledQuery by přístupů kompilovaném dotazu ale vygeneruje nový dotaz autocompiled.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-467">As you can see, using a CompiledQuery for the GridView would not hit the compiled query but would generate a new autocompiled query.</span></span>

<span data-ttu-id="1c6b4-468">Zákaznický poradní tým to popisuje v jejich "Potenciální výkon problémy s zkompilován LINQ dotaz znovu zkompiluje" blogovém příspěvku: <http://blogs.msdn.com/b/appfabriccat/archive/2010/08/06/potential-performance-issues-with-compiled-linq-query-re-compiles.aspx>.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-468">The Customer Advisory Team discusses this in their "Potential Performance Issues with Compiled LINQ Query Re-Compiles" blog post: <http://blogs.msdn.com/b/appfabriccat/archive/2010/08/06/potential-performance-issues-with-compiled-linq-query-re-compiles.aspx>.</span></span>

<span data-ttu-id="1c6b4-469">Jedno místo, kde můžete narazit to je při přidávání progresivní filtry dotazu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-469">One place where you may run into this is when adding progressive filters to a query.</span></span> <span data-ttu-id="1c6b4-470">Předpokládejme například, že byste měli stránku zákazníky s několika rozevíracích seznamech pro volitelné filtry (například země a OrdersCount).</span><span class="sxs-lookup"><span data-stu-id="1c6b4-470">For example, suppose you had a Customers page with several drop-down lists for optional filters (for example, Country and OrdersCount).</span></span> <span data-ttu-id="1c6b4-471">Tyto filtry můžete vytvářet přes výsledky IQueryable CompiledQuery, ale to uděláte, dojde v nový dotaz projít kompilátoru plán pokaždé, když ji spustíte.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-471">You can compose these filters over the IQueryable results of a CompiledQuery, but doing so will result in the new query going through the plan compiler every time you execute it.</span></span>

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

 <span data-ttu-id="1c6b4-472">Abyste předešli této opakované kompilace, je možné přepsat CompiledQuery možné filtry vzít v úvahu:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-472">To avoid this re-compilation, you can rewrite the CompiledQuery to take the possible filters into account:</span></span>

``` csharp
    private static readonly Func<NorthwindEntities, int, int?, string, IQueryable<Customer>> customersForEmployeeWithFiltersCQ = CompiledQuery.Compile(
        (NorthwindEntities context, int empId, int? countFilter, string countryFilter) =>
            context.Customers.Where(c => c.Orders.Any(o => o.EmployeeID == empId))
            .Where(c => countFilter.HasValue == false || c.Orders.Count > countFilter)
            .Where(c => countryFilter == null || c.Address.Country == countryFilter)
        );
```

<span data-ttu-id="1c6b4-473">Který by vyvolání v uživatelském rozhraní, jako jsou:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-473">Which would be invoked in the UI like:</span></span>

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

 <span data-ttu-id="1c6b4-474">Kompromis tady je příkaz generované úložiště bude mít vždy filtry se kontroly hodnoty null, ale tady by měly být pro server databáze pro optimalizaci poměrně jednoduchý:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-474">A tradeoff here is the generated store command will always have the filters with the null checks, but these should be fairly simple for the database server to optimize:</span></span>

``` SQL
...
WHERE ((0 = (CASE WHEN (@p__linq__1 IS NOT NULL) THEN cast(1 as bit) WHEN (@p__linq__1 IS NULL) THEN cast(0 as bit) END)) OR ([Project3].[C2] > @p__linq__2)) AND (@p__linq__3 IS NULL OR [Project3].[Country] = @p__linq__4)
```

### <a name="34-metadata-caching"></a><span data-ttu-id="1c6b4-475">3.4 ukládání do mezipaměti metadat</span><span class="sxs-lookup"><span data-stu-id="1c6b4-475">3.4 Metadata caching</span></span>

<span data-ttu-id="1c6b4-476">Entity Framework také podporuje ukládání do mezipaměti metadat.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-476">The Entity Framework also supports Metadata caching.</span></span> <span data-ttu-id="1c6b4-477">To je v podstatě ukládání do mezipaměti informace o typu a informace o mapování typu databáze po různých připojení do stejného modelu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-477">This is essentially caching of type information and type-to-database mapping information across different connections to the same model.</span></span> <span data-ttu-id="1c6b4-478">Metadata mezipaměti je jedinečná na doménu aplikace.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-478">The Metadata cache is unique per AppDomain.</span></span>

#### <a name="341-metadata-caching-algorithm"></a><span data-ttu-id="1c6b4-479">3.4.1 ukládání do mezipaměti metadat algoritmus</span><span class="sxs-lookup"><span data-stu-id="1c6b4-479">3.4.1 Metadata Caching algorithm</span></span>

1.  <span data-ttu-id="1c6b4-480">Informace metadat modelu je uložen v objektu ItemCollection pro každý EntityConnection.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-480">Metadata information for a model is stored in an ItemCollection for each EntityConnection.</span></span>
    -   <span data-ttu-id="1c6b4-481">Jako poznámka na okraj existují různé objekty ItemCollection pro různé součásti modelu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-481">As a side note, there are different ItemCollection objects for different parts of the model.</span></span> <span data-ttu-id="1c6b4-482">Například StoreItemCollections obsahuje informace o modelu databáze; ObjectItemCollection obsahuje informace o modelu dat Kolekci EdmItemCollection obsahuje informace o konceptuálního modelu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-482">For example, StoreItemCollections contains the information about the database model; ObjectItemCollection contains information about the data model; EdmItemCollection contains information about the conceptual model.</span></span>

2.  <span data-ttu-id="1c6b4-483">Pokud dvě připojení používat stejný připojovací řetězec, budou sdílet stejnou instanci ItemCollection.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-483">If two connections use the same connection string, they will share the same ItemCollection instance.</span></span>
3.  <span data-ttu-id="1c6b4-484">Funkčně ekvivalentní, ale pomocí textu jiné připojovací řetězce může vést k rozdílná metadata mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-484">Functionally equivalent but textually different connection strings may result in different metadata caches.</span></span> <span data-ttu-id="1c6b4-485">Připojovací řetězce, není to jednoduše změnou pořadí tokeny by měl být sdílená metadata tokenizovat.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-485">We do tokenize connection strings, so simply changing the order of the tokens should result in shared metadata.</span></span> <span data-ttu-id="1c6b4-486">Ale dva připojovací řetězce, které vypadá to, že funkčně stejný nemusí být vyhodnoceny jako shodné Tokenizace.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-486">But two connection strings that seem functionally the same may not be evaluated as identical after tokenization.</span></span>
4.  <span data-ttu-id="1c6b4-487">Element ItemCollection je pravidelně kontroluje pro použití.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-487">The ItemCollection is periodically checked for use.</span></span> <span data-ttu-id="1c6b4-488">Pokud je zjištěno, že pracovní prostor nepřistupovalo nedávno, budou označeny pro vyčištění na další vyčištění mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-488">If it is determined that a workspace has not been accessed recently, it will be marked for cleanup on the next cache sweep.</span></span>
5.  <span data-ttu-id="1c6b4-489">Vytváření pouze EntityConnection způsobí, že mezipaměti metadat, který se má vytvořit (v případě, že kolekce položek v něm nelze inicializovat dokud je otevřeno připojení).</span><span class="sxs-lookup"><span data-stu-id="1c6b4-489">Merely creating an EntityConnection will cause a metadata cache to be created (though the item collections in it will not be initialized until the connection is opened).</span></span> <span data-ttu-id="1c6b4-490">Tento pracovní prostor zůstane v paměti, dokud nebude algoritmu ukládání do mezipaměti určuje, že ho nepoužívá "".</span><span class="sxs-lookup"><span data-stu-id="1c6b4-490">This workspace will remain in-memory until the caching algorithm determines it is not “in use”.</span></span>

<span data-ttu-id="1c6b4-491">Zákaznický poradní tým zapsala blogový příspěvek popisuje uchovávající odkaz na objektu ItemCollection předejdete "vyřazení" při použití velkých modelů: \< http://blogs.msdn.com/b/appfabriccat/archive/2010/10/22/metadataworkspace-reference-in-wcf-services.aspx>.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-491">The Customer Advisory Team has written a blog post that describes holding a reference to an ItemCollection in order to avoid "deprecation" when using large models: \<http://blogs.msdn.com/b/appfabriccat/archive/2010/10/22/metadataworkspace-reference-in-wcf-services.aspx>.</span></span>

#### <a name="342-the-relationship-between-metadata-caching-and-query-plan-caching"></a><span data-ttu-id="1c6b4-492">3.4.2 vztah mezi ukládání do mezipaměti metadat a plán dotazu ukládání do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="1c6b4-492">3.4.2 The relationship between Metadata Caching and Query Plan Caching</span></span>

<span data-ttu-id="1c6b4-493">Instance mezipaměti plánu dotazu nachází v objektu MetadataWorkspace ItemCollection typů úložiště.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-493">The query plan cache instance lives in the MetadataWorkspace's ItemCollection of store types.</span></span> <span data-ttu-id="1c6b4-494">To znamená, že příkazy v mezipaměti úložiště se použije pro dotazy na jakýkoli kontext vytvořen pomocí daného objektu MetadataWorkspace.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-494">This means that cached store commands will be used for queries against any context instantiated using a given MetadataWorkspace.</span></span> <span data-ttu-id="1c6b4-495">Také znamená, že pokud máte dva řetězce připojení, které se mírně liší a po tokenizaci se neshodují, budete mít jiný dotaz, naplánujte instancí mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-495">It also means that if you have two connections strings that are slightly different and don't match after tokenizing, you will have different query plan cache instances.</span></span>

### <a name="35-results-caching"></a><span data-ttu-id="1c6b4-496">3.5 výsledky ukládání do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="1c6b4-496">3.5 Results caching</span></span>

<span data-ttu-id="1c6b4-497">S výsledky ukládání do mezipaměti (označované také jako "druhé úrovně ukládání do mezipaměti") zachovat výsledky dotazů v místní mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-497">With results caching (also known as "second-level caching"), you keep the results of queries in a local cache.</span></span> <span data-ttu-id="1c6b4-498">Po zadání dotazu, nejprve zobrazí, pokud jsou k dispozici místně před dotaz proti úložišti výsledky.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-498">When issuing a query, you first see if the results are available locally before you query against the store.</span></span> <span data-ttu-id="1c6b4-499">Když výsledky ukládání do mezipaměti není přímo podporován rozhraním Entity Framework, je možné přidat druhou úroveň mezipaměti s použitím zprostředkovatele zabalení.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-499">While results caching isn't directly supported by Entity Framework, it's possible to add a second level cache by using a wrapping provider.</span></span> <span data-ttu-id="1c6b4-500">Příklad zabalení zprostředkovatele s mezipamětí druhé úrovně se od Alachisoft [Entity Framework druhou úroveň mezipaměti podle NCache](http://www.alachisoft.com/ncache/entity-framework.html).</span><span class="sxs-lookup"><span data-stu-id="1c6b4-500">An example wrapping provider with a second-level cache is Alachisoft's [Entity Framework Second Level Cache based on NCache](http://www.alachisoft.com/ncache/entity-framework.html).</span></span>

<span data-ttu-id="1c6b4-501">Tato implementace druhé úrovně, ukládání do mezipaměti je vložená funkce, která přebírá místo po vyhodnocení výrazu LINQ (a funcletized) a plán provádění dotazu je vypočítáván nebo načíst z mezipaměti první úrovně.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-501">This implementation of second-level caching is an injected functionality that takes place after the LINQ expression has been evaluated (and funcletized) and the query execution plan is computed or retrieved from the first-level cache.</span></span> <span data-ttu-id="1c6b4-502">Druhé úrovně mezipaměti pak uloží pouze výsledky nezpracovaná databáze, tak kanálu materializace stále provede později.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-502">The second-level cache will then store only the raw database results, so the materialization pipeline still executes afterwards.</span></span>

#### <a name="351-additional-references-for-results-caching-with-the-wrapping-provider"></a><span data-ttu-id="1c6b4-503">3.5.1 Další informace o ukládání do mezipaměti s tímto poskytovatelem zabalení výsledky</span><span class="sxs-lookup"><span data-stu-id="1c6b4-503">3.5.1 Additional references for results caching with the wrapping provider</span></span>

-   <span data-ttu-id="1c6b4-504">Julie Lerman zapsala článku MSDN "Druhé úrovně ukládání do mezipaměti v Entity Framework a Windows Azure", obsahující aktualizace poskytovatele zabalení Ukázka použití ukládání do mezipaměti systému Windows Server AppFabric: [https://msdn.microsoft.com/magazine/hh394143.aspx](https://msdn.microsoft.com/magazine/hh394143.aspx)</span><span class="sxs-lookup"><span data-stu-id="1c6b4-504">Julie Lerman has written a "Second-Level Caching in Entity Framework and Windows Azure" MSDN article that includes how to update the sample wrapping provider to use Windows Server AppFabric caching: [https://msdn.microsoft.com/magazine/hh394143.aspx](https://msdn.microsoft.com/magazine/hh394143.aspx)</span></span>
-   <span data-ttu-id="1c6b4-505">Pokud pracujete s Entity Framework 5, na blogu týmu má příspěvek, který popisuje, jak pracovat s poskytovateli ukládání do mezipaměti pro Entity Framework 5: \< http://blogs.msdn.com/b/adonet/archive/2010/09/13/ef-caching-with-jarek-kowalski-s-provider.aspx>.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-505">If you are working with Entity Framework 5, the team blog has a post that describes how to get things running with the caching provider for Entity Framework 5: \<http://blogs.msdn.com/b/adonet/archive/2010/09/13/ef-caching-with-jarek-kowalski-s-provider.aspx>.</span></span> <span data-ttu-id="1c6b4-506">Zahrnuje také šablona T4 pro automatizaci přidání úrovně 2. ukládání do mezipaměti do projektu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-506">It also includes a T4 template to help automate adding the 2nd-level caching to your project.</span></span>

## <a name="4-autocompiled-queries"></a><span data-ttu-id="1c6b4-507">4 Autocompiled dotazy</span><span class="sxs-lookup"><span data-stu-id="1c6b4-507">4 Autocompiled Queries</span></span>

<span data-ttu-id="1c6b4-508">Vystavení dotaz na databázi pomocí Entity Frameworku ho musí projít řadou kroků před skutečně materializaci výsledky; jeden takový krokem je kompilace dotazu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-508">When a query is issued against a database using Entity Framework, it must go through a series of steps before actually materializing the results; one such step is Query Compilation.</span></span> <span data-ttu-id="1c6b4-509">Dotazy SQL entity používalo mít dobrý výkon, se automaticky uloží do mezipaměti, aby druhý nebo třetí čas spuštění stejný dotaz může přeskočit kompilátoru plánu a místo toho použijte plánů v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-509">Entity SQL queries were known to have good performance as they are automatically cached, so the second or third time you execute the same query it can skip the plan compiler and use the cached plan instead.</span></span>

<span data-ttu-id="1c6b4-510">Entity Framework 5 zavedená, automatické ukládání do mezipaměti pro funkci LINQ na entity dotazy a také.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-510">Entity Framework 5 introduced automatic caching for LINQ to Entities queries as well.</span></span> <span data-ttu-id="1c6b4-511">V posledních verzích sady Entity Framework vytváření CompiledQuery urychlit byl výkon běžnou praxí, jak to s žádným LINQ dotazu entity možné ukládat do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-511">In past editions of Entity Framework creating a CompiledQuery to speed your performance was a common practice, as this would make your LINQ to Entities query cacheable.</span></span> <span data-ttu-id="1c6b4-512">Protože ukládání do mezipaměti se teď provádí automaticky bez použití CompiledQuery, budeme volat tuto funkci "autocompiled dotazy".</span><span class="sxs-lookup"><span data-stu-id="1c6b4-512">Since caching is now done automatically without the use of a CompiledQuery, we call this feature “autocompiled queries”.</span></span> <span data-ttu-id="1c6b4-513">Další informace o mezipaměti plánu dotazu a jeho mechanics najdete v článku ukládání do mezipaměti plánu dotazu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-513">For more information about the query plan cache and its mechanics, see Query Plan Caching.</span></span>

<span data-ttu-id="1c6b4-514">Entity Framework rozpozná, pokud dotaz vyžaduje překompilovat, a o to postará při vyvolání dotaz i v případě, kdyby byl zkompilován před.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-514">Entity Framework detects when a query requires to be recompiled, and does so when the query is invoked even if it had been compiled before.</span></span> <span data-ttu-id="1c6b4-515">Běžné podmínky, které způsobí dotaz překompilovat jsou:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-515">Common conditions that cause the query to be recompiled are:</span></span>

-   <span data-ttu-id="1c6b4-516">Změna MergeOption přidružené k vašemu dotazu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-516">Changing the MergeOption associated to your query.</span></span> <span data-ttu-id="1c6b4-517">V mezipaměti dotaz nebude použit, místo toho kompilátor plán znovu spustí a nově vytvořený plán získá uložili do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-517">The cached query will not be used, instead the plan compiler will run again and the newly created plan gets cached.</span></span>
-   <span data-ttu-id="1c6b4-518">Změna hodnoty ContextOptions.UseCSharpNullComparisonBehavior.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-518">Changing the value of ContextOptions.UseCSharpNullComparisonBehavior.</span></span> <span data-ttu-id="1c6b4-519">Budete mít stejný účinek jako v případě změny MergeOption.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-519">You get the same effect as changing the MergeOption.</span></span>

<span data-ttu-id="1c6b4-520">Další podmínky zabránit použití mezipaměti váš dotaz.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-520">Other conditions can prevent your query from using the cache.</span></span> <span data-ttu-id="1c6b4-521">Obvyklými příklady jsou:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-521">Common examples are:</span></span>

-   <span data-ttu-id="1c6b4-522">Použití rozhraní IEnumerable&lt;T&gt;. Obsahuje&lt;&gt;(hodnota T).</span><span class="sxs-lookup"><span data-stu-id="1c6b4-522">Using IEnumerable&lt;T&gt;.Contains&lt;&gt;(T value).</span></span>
-   <span data-ttu-id="1c6b4-523">Pomocí funkcí, které vytvářejí dotazy s konstanty.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-523">Using functions that produce queries with constants.</span></span>
-   <span data-ttu-id="1c6b4-524">Pomocí vlastností objektu není namapována.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-524">Using the properties of a non-mapped object.</span></span>
-   <span data-ttu-id="1c6b4-525">Propojení dotaz k jinému dotazu, který vyžaduje třeba znovu zkompilovat.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-525">Linking your query to another query that requires to be recompiled.</span></span>

### <a name="41-using-ienumerablelttgtcontainslttgtt-value"></a><span data-ttu-id="1c6b4-526">4.1 pomocí IEnumerable&lt;T&gt;. Obsahuje&lt;T&gt;(hodnota T)</span><span class="sxs-lookup"><span data-stu-id="1c6b4-526">4.1 Using IEnumerable&lt;T&gt;.Contains&lt;T&gt;(T value)</span></span>

<span data-ttu-id="1c6b4-527">Entity Framework neukládá do mezipaměti dotazů, které vyvolají IEnumerable&lt;T&gt;. Obsahuje&lt;T&gt;(hodnota T) proti kolekci v paměti, protože hodnoty kolekce se považují za volatile.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-527">Entity Framework does not cache queries that invoke IEnumerable&lt;T&gt;.Contains&lt;T&gt;(T value) against an in-memory collection, since the values of the collection are considered volatile.</span></span> <span data-ttu-id="1c6b4-528">Následující příklad dotazu nebudou zapisována do mezipaměti, abyste ho vždy se zpracuje kompilátorem plánu:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-528">The following example query will not be cached, so it will always be processed by the plan compiler:</span></span>

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

<span data-ttu-id="1c6b4-529">Mějte na paměti, která se spustí velikost IEnumerable, proti které obsahuje Určuje, jak rychle nebo pomalu jak je zkompilován dotaz.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-529">Note that the size of the IEnumerable against which Contains is executed determines how fast or how slow your query is compiled.</span></span> <span data-ttu-id="1c6b4-530">Výkon může být negativně výrazně při používání rozsáhlých kolekcí, jako je uvedeno v předchozím příkladu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-530">Performance can suffer significantly when using large collections such as the one shown in the example above.</span></span>

<span data-ttu-id="1c6b4-531">Entity Framework 6 obsahuje Optimalizace způsobu, jakým IEnumerable&lt;T&gt;. Obsahuje&lt;T&gt;(hodnota T) funguje při provádění dotazů.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-531">Entity Framework 6 contains optimizations to the way IEnumerable&lt;T&gt;.Contains&lt;T&gt;(T value) works when queries are executed.</span></span> <span data-ttu-id="1c6b4-532">Kód SQL, který je generován se vytvoří mnohem rychleji a lépe čitelný, a ve většině případů se také provede rychleji na serveru.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-532">The SQL code that is generated is much faster to produce and more readable, and in most cases it also executes faster in the server.</span></span>

### <a name="42-using-functions-that-produce-queries-with-constants"></a><span data-ttu-id="1c6b4-533">4.2 pomocí funkcí, které vytvářejí dotazy pomocí konstant</span><span class="sxs-lookup"><span data-stu-id="1c6b4-533">4.2 Using functions that produce queries with constants</span></span>

<span data-ttu-id="1c6b4-534">Operátory Skip(), Take(), metoda Contains() a DefautIfEmpty() LINQ záměrně neprodukují příkazy jazyka SQL s parametry, ale místo toho umístit hodnot předaných jako konstanty.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-534">The Skip(), Take(), Contains() and DefautIfEmpty() LINQ operators do not produce SQL queries with parameters but instead put the values passed to them as constants.</span></span> <span data-ttu-id="1c6b4-535">Z tohoto důvodu dotazy, které jinak může být identické skončí zahlcení dotaz plánování mezipaměti, v zásobníku EF a na serveru databáze a získat reutilized není Pokud stejnou konstanty se používají v provádění dalších dotazů.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-535">Because of this, queries that might otherwise be identical end up polluting the query plan cache, both on the EF stack and on the database server, and do not get reutilized unless the same constants are used in a subsequent query execution.</span></span> <span data-ttu-id="1c6b4-536">Příklad:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-536">For example:</span></span>

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

<span data-ttu-id="1c6b4-537">V tomto příkladu se pokaždé, když tento dotaz provádí s jinou hodnotou pro id dotazu se zkompiluje do nového plánu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-537">In this example, each time this query is executed with a different value for id the query will be compiled into a new plan.</span></span>

<span data-ttu-id="1c6b4-538">V konkrétní věnujte pozornost tomu použití Skip a Take, při stránkování.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-538">In particular pay attention to the use of Skip and Take when doing paging.</span></span> <span data-ttu-id="1c6b4-539">EF6 mít tyto metody přetížení lambda, umožňující efektivní plán dotazu z mezipaměti opakovaně použitelné protože EF můžete zachytit proměnné předána tyto metody a překládat je SQLparameters.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-539">In EF6 these methods have a lambda overload that effectively makes the cached query plan reusable because EF can capture variables passed to these methods and translate them to SQLparameters.</span></span> <span data-ttu-id="1c6b4-540">Navíc pomáhají zachovat mezipaměti přehlednější, protože každý dotaz s jinou konstantou Skip a Take, jinak by zobrazí jeho vlastní položka mezipaměti plánu dotazu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-540">This also helps keep the cache cleaner since otherwise each query with a different constant for Skip and Take would get its own query plan cache entry.</span></span>

<span data-ttu-id="1c6b4-541">Vezměte v úvahu následující kód, který je neoptimální ale slouží pouze k exemplify Tato třída dotazy:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-541">Consider the following code, which is suboptimal but is only meant to exemplify this class of queries:</span></span>

``` csharp
var customers = context.Customers.OrderBy(c => c.LastName);
for (var i = 0; i < count; ++i)
{
    var currentCustomer = customers.Skip(i).FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

<span data-ttu-id="1c6b4-542">Rychlejší verzi Tento stejný kód by vyžadovalo volání přeskočit pomocí výrazu lambda:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-542">A faster version of this same code would involve calling Skip with a lambda:</span></span>

``` csharp
var customers = context.Customers.OrderBy(c => c.LastName);
for (var i = 0; i \< count; ++i)
{
    var currentCustomer = customers.Skip(() => i).FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

<span data-ttu-id="1c6b4-543">Druhý fragment kódu může spustit až 11 % rychlejší, protože stejný plán dotazu, který se používá při každém spuštění dotazu, což šetří čas procesoru a zabraňuje zahlcení mezipaměti dotazů.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-543">The second snippet may run up to 11% faster because the same query plan is used every time the query is run, which saves CPU time and avoids polluting the query cache.</span></span> <span data-ttu-id="1c6b4-544">Navíc vzhledem k tomu, že parametr Skip je v uzávěru kód může také vypadat takto nyní:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-544">Furthermore, because the parameter to Skip is in a closure the code might as well look like this now:</span></span>

``` csharp
var i = 0;
var skippyCustomers = context.Customers.OrderBy(c => c.LastName).Skip(() => i);
for (; i < count; ++i)
{
    var currentCustomer = skippyCustomers.FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

### <a name="43-using-the-properties-of-a-non-mapped-object"></a><span data-ttu-id="1c6b4-545">4.3 pomocí vlastností objektu bez mapované</span><span class="sxs-lookup"><span data-stu-id="1c6b4-545">4.3 Using the properties of a non-mapped object</span></span>

<span data-ttu-id="1c6b4-546">Když dotaz používá vlastnosti typu objektu není mapován jako parametr a dotaz nebude získat uložené v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-546">When a query uses the properties of a non-mapped object type as a parameter then the query will not get cached.</span></span> <span data-ttu-id="1c6b4-547">Příklad:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-547">For example:</span></span>

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

<span data-ttu-id="1c6b4-548">V tomto příkladu se předpokládá, že třída NonMappedType není součástí modelu Entity.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-548">In this example, assume that class NonMappedType is not part of the Entity model.</span></span> <span data-ttu-id="1c6b4-549">Tento dotaz lze snadno změnit nelze použít typ není namapována a místo toho použít místní proměnné jako parametr dotazu:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-549">This query can easily be changed to not use a non-mapped type and instead use a local variable as the parameter to the query:</span></span>

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

<span data-ttu-id="1c6b4-550">Dotaz v tomto případě bude moct získat do mezipaměti a budou těžit z mezipaměti plánu dotazu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-550">In this case, the query will be able to get cached and will benefit from the query plan cache.</span></span>

### <a name="44-linking-to-queries-that-require-recompiling"></a><span data-ttu-id="1c6b4-551">4.4 odkazování na dotazy, které vyžadují opětovné kompilaci</span><span class="sxs-lookup"><span data-stu-id="1c6b4-551">4.4 Linking to queries that require recompiling</span></span>

<span data-ttu-id="1c6b4-552">V následujícím příkladu stejný jako výše Pokud máte druhý dotaz, který závisí na dotaz, který je potřeba překompilovat, celý druhého dotazu se také překompilují.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-552">Following the same example as above, if you have a second query that relies on a query that needs to be recompiled, your entire second query will also be recompiled.</span></span> <span data-ttu-id="1c6b4-553">Tady je příklad pro ilustraci tohoto scénáře:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-553">Here’s an example to illustrate this scenario:</span></span>

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

<span data-ttu-id="1c6b4-554">V příkladu je obecný, ale ukazuje, jak je propojení firstQuery příčinou secondQuery nebude možné získat uložené v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-554">The example is generic, but it illustrates how linking to firstQuery is causing secondQuery to be unable to get cached.</span></span> <span data-ttu-id="1c6b4-555">Pokud firstQuery nebyla dotaz, který vyžaduje opětovné kompilaci, pak secondQuery by mít se zapíší do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-555">If firstQuery had not been a query that requires recompiling, then secondQuery would have been cached.</span></span>

## <a name="5-notracking-queries"></a><span data-ttu-id="1c6b4-556">5 NoTracking dotazy</span><span class="sxs-lookup"><span data-stu-id="1c6b4-556">5 NoTracking Queries</span></span>

### <a name="51-disabling-change-tracking-to-reduce-state-management-overhead"></a><span data-ttu-id="1c6b4-557">5.1 zakázání ke snížení režie na správu stavu sledování změn</span><span class="sxs-lookup"><span data-stu-id="1c6b4-557">5.1 Disabling change tracking to reduce state management overhead</span></span>

<span data-ttu-id="1c6b4-558">Pokud se v případě jen pro čtení a režie načítání objektů do objektu ObjectStateManager určitě nepřejete, můžete posílat dotazy "No sledování".</span><span class="sxs-lookup"><span data-stu-id="1c6b4-558">If you are in a read-only scenario and want to avoid the overhead of loading the objects into the ObjectStateManager, you can issue "No Tracking" queries.</span></span>  <span data-ttu-id="1c6b4-559">Sledování změn je možné zakázat na úrovni dotazů.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-559">Change tracking can be disabled at the query level.</span></span>

<span data-ttu-id="1c6b4-560">Nezapomeňte, že tím, že zakážete sledování vám změn jsou účinně vypnutí mezipaměti objektů.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-560">Note though that by disabling change tracking you are effectively turning off the object cache.</span></span> <span data-ttu-id="1c6b4-561">Když odešlete dotaz pro entitu, jsme nelze přeskočit materializace díky přebírání výsledky dříve materializovaného dotazu z objektu ObjectStateManager.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-561">When you query for an entity, we can't skip materialization by pulling the previously-materialized query results from the ObjectStateManager.</span></span> <span data-ttu-id="1c6b4-562">Pokud jsou opakovaně dotazování na stejné entity ve stejném kontextu, může se ve skutečnosti zobrazit výkonu těžit z povolení řešení change tracking.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-562">If you are repeatedly querying for the same entities on the same context, you might actually see a performance benefit from enabling change tracking.</span></span>

<span data-ttu-id="1c6b4-563">Při dotazování pomocí objektu ObjectContext, instance ObjectQuery a ObjectSet si budou pamatovat MergeOption po nastavení a dotazy, které se skládají na nich zdědí efektivní MergeOption nadřazený dotaz.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-563">When querying using ObjectContext, ObjectQuery and ObjectSet instances will remember a MergeOption once it is set, and queries that are composed on them will inherit the effective MergeOption of the parent query.</span></span> <span data-ttu-id="1c6b4-564">Při použití DbContext, je možné zakázat sledování získaný pomocí funkce modifikátor AsNoTracking() DbSet.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-564">When using DbContext, tracking can be disabled by calling the AsNoTracking() modifier on the DbSet.</span></span>

#### <a name="511-disabling-change-tracking-for-a-query-when-using-dbcontext"></a><span data-ttu-id="1c6b4-565">5.1.1 zakázání sledování změn pro dotaz při použití DbContext</span><span class="sxs-lookup"><span data-stu-id="1c6b4-565">5.1.1 Disabling change tracking for a query when using DbContext</span></span>

<span data-ttu-id="1c6b4-566">Přepnout režim dotazu na NoTracking pomocí zřetězení volání metody AsNoTracking() v dotazu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-566">You can switch the mode of a query to NoTracking by chaining a call to the AsNoTracking() method in the query.</span></span> <span data-ttu-id="1c6b4-567">Na rozdíl od ObjectQuery nemají DbSet a DbQuery třídy v rozhraní API pro DbContext měnitelné vlastnosti pro MergeOption.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-567">Unlike ObjectQuery, the DbSet and DbQuery classes in the DbContext API don’t have a mutable property for the MergeOption.</span></span>

``` csharp
    var productsForCategory = from p in context.Products.AsNoTracking()
                                where p.Category.CategoryName == selectedCategory
                                select p;


```

#### <a name="512-disabling-change-tracking-at-the-query-level-using-objectcontext"></a><span data-ttu-id="1c6b4-568">5.1.2 zakázání řešení change tracking na úrovni dotazů pomocí ObjectContext</span><span class="sxs-lookup"><span data-stu-id="1c6b4-568">5.1.2 Disabling change tracking at the query level using ObjectContext</span></span>

``` csharp
    var productsForCategory = from p in context.Products
                                where p.Category.CategoryName == selectedCategory
                                select p;

    ((ObjectQuery)productsForCategory).MergeOption = MergeOption.NoTracking;
```

#### <a name="513-disabling-change-tracking-for-an-entire-entity-set-using-objectcontext"></a><span data-ttu-id="1c6b4-569">5.1.3 zakázání sledování změn pro entitu celá sada s použitím objektu ObjectContext</span><span class="sxs-lookup"><span data-stu-id="1c6b4-569">5.1.3 Disabling change tracking for an entire entity set using ObjectContext</span></span>

``` csharp
    context.Products.MergeOption = MergeOption.NoTracking;

    var productsForCategory = from p in context.Products
                                where p.Category.CategoryName == selectedCategory
                                select p;
```

### <a name="52-test-metrics-demonstrating-the-performance-benefit-of-notracking-queries"></a><span data-ttu-id="1c6b4-570">5.2 ukázka výhody výkonu dotazů NoTracking metriky testu</span><span class="sxs-lookup"><span data-stu-id="1c6b4-570">5.2 Test Metrics demonstrating the performance benefit of NoTracking queries</span></span>

<span data-ttu-id="1c6b4-571">V tomto testu podíváme za cenu porovnáním sledování NoTracking dotazy pro Navision model vyplnění objektu ObjectStateManager.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-571">In this test we look at the cost of filling the ObjectStateManager by comparing Tracking to NoTracking queries for the Navision model.</span></span> <span data-ttu-id="1c6b4-572">Viz dodatek popis modelu Navision a typy dotazů, které byly spuštěny.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-572">See the appendix for a description of the Navision model and the types of queries which were executed.</span></span> <span data-ttu-id="1c6b4-573">V tomto testu jsme iteraci v rámci seznamu dotazů a každý z nich provedena pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-573">In this test, we iterate through the list of queries and execute each one once.</span></span> <span data-ttu-id="1c6b4-574">Spustili jsme dvě varianty testu, jednou s dotazy NoTracking a jednou s možností sloučení výchozí pouze "Přidat".</span><span class="sxs-lookup"><span data-stu-id="1c6b4-574">We ran two variations of the test, once with NoTracking queries and once with the default merge option of "AppendOnly".</span></span> <span data-ttu-id="1c6b4-575">Jsme spustili každou změnu 3krát a trvat střední hodnoty spuštění.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-575">We ran each variation 3 times and take the mean value of the runs.</span></span> <span data-ttu-id="1c6b4-576">Mezi testy jsme vymazat mezipaměť dotazu na SQL serveru a zmenšit databázi tempdb spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-576">Between the tests we clear the query cache on the SQL Server and shrink the tempdb by running the following commands:</span></span>

1.  <span data-ttu-id="1c6b4-577">PŘÍKAZ DBCC DROPCLEANBUFFERS</span><span class="sxs-lookup"><span data-stu-id="1c6b4-577">DBCC DROPCLEANBUFFERS</span></span>
2.  <span data-ttu-id="1c6b4-578">PŘÍKAZ DBCC FREEPROCCACHE</span><span class="sxs-lookup"><span data-stu-id="1c6b4-578">DBCC FREEPROCCACHE</span></span>
3.  <span data-ttu-id="1c6b4-579">Příkaz DBCC SHRINKDATABASE (databáze tempdb, 0)</span><span class="sxs-lookup"><span data-stu-id="1c6b4-579">DBCC SHRINKDATABASE (tempdb, 0)</span></span>

<span data-ttu-id="1c6b4-580">Výsledky, střední více než 3 spuštění testů:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-580">Test Results, median over 3 runs:</span></span>

|                        | <span data-ttu-id="1c6b4-581">ŽÁDNÉ SLEDOVÁNÍ – PRACOVNÍ SADA</span><span class="sxs-lookup"><span data-stu-id="1c6b4-581">NO TRACKING – WORKING SET</span></span> | <span data-ttu-id="1c6b4-582">ŽÁDNÉ SLEDOVÁNÍ – ČAS</span><span class="sxs-lookup"><span data-stu-id="1c6b4-582">NO TRACKING – TIME</span></span> | <span data-ttu-id="1c6b4-583">PŘIPOJIT POUZE – PRACOVNÍ SADA</span><span class="sxs-lookup"><span data-stu-id="1c6b4-583">APPEND ONLY – WORKING SET</span></span> | <span data-ttu-id="1c6b4-584">PŘIPOJIT POUZE – ČAS</span><span class="sxs-lookup"><span data-stu-id="1c6b4-584">APPEND ONLY – TIME</span></span> |
|:-----------------------|:--------------------------|:-------------------|:--------------------------|:-------------------|
| <span data-ttu-id="1c6b4-585">**Entity Framework 5**</span><span class="sxs-lookup"><span data-stu-id="1c6b4-585">**Entity Framework 5**</span></span> | <span data-ttu-id="1c6b4-586">460361728</span><span class="sxs-lookup"><span data-stu-id="1c6b4-586">460361728</span></span>                 | <span data-ttu-id="1c6b4-587">1163536 ms</span><span class="sxs-lookup"><span data-stu-id="1c6b4-587">1163536 ms</span></span>         | <span data-ttu-id="1c6b4-588">596545536</span><span class="sxs-lookup"><span data-stu-id="1c6b4-588">596545536</span></span>                 | <span data-ttu-id="1c6b4-589">1273042 ms</span><span class="sxs-lookup"><span data-stu-id="1c6b4-589">1273042 ms</span></span>         |
| <span data-ttu-id="1c6b4-590">**Entity Framework 6**</span><span class="sxs-lookup"><span data-stu-id="1c6b4-590">**Entity Framework 6**</span></span> | <span data-ttu-id="1c6b4-591">647127040</span><span class="sxs-lookup"><span data-stu-id="1c6b4-591">647127040</span></span>                 | <span data-ttu-id="1c6b4-592">190228 ms</span><span class="sxs-lookup"><span data-stu-id="1c6b4-592">190228 ms</span></span>          | <span data-ttu-id="1c6b4-593">832798720</span><span class="sxs-lookup"><span data-stu-id="1c6b4-593">832798720</span></span>                 | <span data-ttu-id="1c6b4-594">195521 ms</span><span class="sxs-lookup"><span data-stu-id="1c6b4-594">195521 ms</span></span>          |

<span data-ttu-id="1c6b4-595">Entity Framework 5 bude mít menší nároky na paměť na konci spuštění než Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-595">Entity Framework 5 will have a smaller memory footprint at the end of the run than Entity Framework 6 does.</span></span> <span data-ttu-id="1c6b4-596">Další paměti používané Entity Framework 6 je výsledkem další paměť struktur a kód, který povolení nových funkcí a vyšší výkon.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-596">The additional memory consumed by Entity Framework 6 is the result of additional memory structures and code that enable new features and better performance.</span></span>

<span data-ttu-id="1c6b4-597">Je také vymazat rozdíl v nároky na paměť při použití objektu ObjectStateManager.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-597">There’s also a clear difference in memory footprint when using the ObjectStateManager.</span></span> <span data-ttu-id="1c6b4-598">Entity Framework 5 vyšší nároky na jeho místo o 30 % při udržování přehledu o všechny entity, které jsme materializovaného z databáze.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-598">Entity Framework 5 increased its footprint by 30% when keeping track of all the entities we materialized from the database.</span></span> <span data-ttu-id="1c6b4-599">Když to tak uděláte Entity Framework 6 zvýšit své nároky na místo o 28 %.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-599">Entity Framework 6 increased its footprint by 28% when doing so.</span></span>

<span data-ttu-id="1c6b4-600">Z hlediska času Entity Framework 6 lepší výkon než Entity Framework 5 v tomto testu pomocí velký okraj.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-600">In terms of time, Entity Framework 6 outperforms Entity Framework 5 in this test by a large margin.</span></span> <span data-ttu-id="1c6b4-601">Entity Framework 6 dokončit test v zhruba 16 % času používané Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-601">Entity Framework 6 completed the test in roughly 16% of the time consumed by Entity Framework 5.</span></span> <span data-ttu-id="1c6b4-602">Kromě toho Entity Framework 5 % 9, další nějakou dobu trvá dokončení při použití objektu ObjectStateManager.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-602">Additionally, Entity Framework 5 takes 9% more time to complete when the ObjectStateManager is being used.</span></span> <span data-ttu-id="1c6b4-603">Ve srovnání s Entity Framework 6 používá více času při použití objektu ObjectStateManager % 3.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-603">In comparison, Entity Framework 6 is using 3% more time when using the ObjectStateManager.</span></span>

## <a name="6-query-execution-options"></a><span data-ttu-id="1c6b4-604">6 možnosti spuštění dotazu</span><span class="sxs-lookup"><span data-stu-id="1c6b4-604">6 Query Execution Options</span></span>

<span data-ttu-id="1c6b4-605">Entity Framework nabízí několik různých způsobů, jak dotaz.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-605">Entity Framework offers several different ways to query.</span></span> <span data-ttu-id="1c6b4-606">Použijeme podívejte se na následující možnosti, porovnat výhody a nevýhody jednotlivých a zkontrolovat jejich výkonnostní charakteristice:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-606">We'll take a look at the following options, compare the pros and cons of each, and examine their performance characteristics:</span></span>

-   <span data-ttu-id="1c6b4-607">Technologie LINQ to Entities.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-607">LINQ to Entities.</span></span>
-   <span data-ttu-id="1c6b4-608">Žádné sledování LINQ to Entities.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-608">No Tracking LINQ to Entities.</span></span>
-   <span data-ttu-id="1c6b4-609">Entita SQL přes ObjectQuery.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-609">Entity SQL over an ObjectQuery.</span></span>
-   <span data-ttu-id="1c6b4-610">Entita SQL přes EntityCommand.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-610">Entity SQL over an EntityCommand.</span></span>
-   <span data-ttu-id="1c6b4-611">ExecuteStoreQuery.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-611">ExecuteStoreQuery.</span></span>
-   <span data-ttu-id="1c6b4-612">SqlQuery.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-612">SqlQuery.</span></span>
-   <span data-ttu-id="1c6b4-613">CompiledQuery.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-613">CompiledQuery.</span></span>

### <a name="61-------linq-to-entities-queries"></a><span data-ttu-id="1c6b4-614">6.1 dotazech LINQ to Entities</span><span class="sxs-lookup"><span data-stu-id="1c6b4-614">6.1       LINQ to Entities queries</span></span>

``` csharp
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
```

<span data-ttu-id="1c6b4-615">**V oblasti IT**</span><span class="sxs-lookup"><span data-stu-id="1c6b4-615">**Pros**</span></span>

-   <span data-ttu-id="1c6b4-616">Vhodný pro operace vytvoření.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-616">Suitable for CUD operations.</span></span>
-   <span data-ttu-id="1c6b4-617">Plně materializovaného objekty.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-617">Fully materialized objects.</span></span>
-   <span data-ttu-id="1c6b4-618">Nejjednodušší jak psát pomocí syntaxe integrovaná v programovacím jazyce.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-618">Simplest to write with syntax built into the programming language.</span></span>
-   <span data-ttu-id="1c6b4-619">Dobrý výkon.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-619">Good performance.</span></span>

<span data-ttu-id="1c6b4-620">**Nevýhody**</span><span class="sxs-lookup"><span data-stu-id="1c6b4-620">**Cons**</span></span>

-   <span data-ttu-id="1c6b4-621">Určitá technická omezení, jako například:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-621">Certain technical restrictions, such as:</span></span>
    -   <span data-ttu-id="1c6b4-622">Využitím DefaultIfEmpty OUTER JOIN dotazů za následek složitější dotazy než jednoduché příkazy OUTER JOIN v Entity SQL.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-622">Patterns using DefaultIfEmpty for OUTER JOIN queries result in more complex queries than simple OUTER JOIN statements in Entity SQL.</span></span>
    -   <span data-ttu-id="1c6b4-623">Stále nelze použít s obecné porovnávání vzorů.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-623">You still can’t use LIKE with general pattern matching.</span></span>

### <a name="62-------no-tracking-linq-to-entities-queries"></a><span data-ttu-id="1c6b4-624">6.2 žádné sledování LINQ dotazy na entity</span><span class="sxs-lookup"><span data-stu-id="1c6b4-624">6.2       No Tracking LINQ to Entities queries</span></span>

<span data-ttu-id="1c6b4-625">Když kontextu odvozuje ObjectContext:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-625">When the context derives ObjectContext:</span></span>

``` csharp
context.Products.MergeOption = MergeOption.NoTracking;
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
```

<span data-ttu-id="1c6b4-626">Když kontextu odvozuje DbContext:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-626">When the context derives DbContext:</span></span>

``` csharp
var q = context.Products.AsNoTracking()
                        .Where(p => p.Category.CategoryName == "Beverages");
```

<span data-ttu-id="1c6b4-627">**V oblasti IT**</span><span class="sxs-lookup"><span data-stu-id="1c6b4-627">**Pros**</span></span>

-   <span data-ttu-id="1c6b4-628">Vylepšený výkon než regulární dotazů LINQ.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-628">Improved performance over regular LINQ queries.</span></span>
-   <span data-ttu-id="1c6b4-629">Plně materializovaného objekty.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-629">Fully materialized objects.</span></span>
-   <span data-ttu-id="1c6b4-630">Nejjednodušší jak psát pomocí syntaxe integrovaná v programovacím jazyce.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-630">Simplest to write with syntax built into the programming language.</span></span>

<span data-ttu-id="1c6b4-631">**Nevýhody**</span><span class="sxs-lookup"><span data-stu-id="1c6b4-631">**Cons**</span></span>

-   <span data-ttu-id="1c6b4-632">Není vhodný pro operace vytvoření.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-632">Not suitable for CUD operations.</span></span>
-   <span data-ttu-id="1c6b4-633">Určitá technická omezení, jako například:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-633">Certain technical restrictions, such as:</span></span>
    -   <span data-ttu-id="1c6b4-634">Využitím DefaultIfEmpty OUTER JOIN dotazů za následek složitější dotazy než jednoduché příkazy OUTER JOIN v Entity SQL.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-634">Patterns using DefaultIfEmpty for OUTER JOIN queries result in more complex queries than simple OUTER JOIN statements in Entity SQL.</span></span>
    -   <span data-ttu-id="1c6b4-635">Stále nelze použít s obecné porovnávání vzorů.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-635">You still can’t use LIKE with general pattern matching.</span></span>

<span data-ttu-id="1c6b4-636">Všimněte si, že i když není zadaný NoTracking nebudou pro účely dotazů, které Skalární vlastnosti projektu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-636">Note that queries that project scalar properties are not tracked even if the NoTracking is not specified.</span></span> <span data-ttu-id="1c6b4-637">Příklad:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-637">For example:</span></span>

``` csharp
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages").Select(p => new { p.ProductName });
```

<span data-ttu-id="1c6b4-638">Tento konkrétní dotaz nemá určenou explicitně se NoTracking, ale vzhledem k tomu, že není materializaci typ, který je známo, že správce stavu objektu pak výsledku materializovaného není sledována.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-638">This particular query doesn’t explicitly specify being NoTracking, but since it’s not materializing a type that’s known to the object state manager then the materialized result is not tracked.</span></span>

### <a name="63-------entity-sql-over-an-objectquery"></a><span data-ttu-id="1c6b4-639">6.3 entity SQL přes ObjectQuery</span><span class="sxs-lookup"><span data-stu-id="1c6b4-639">6.3       Entity SQL over an ObjectQuery</span></span>

``` csharp
ObjectQuery<Product> products = context.Products.Where("it.Category.CategoryName = 'Beverages'");
```

<span data-ttu-id="1c6b4-640">**V oblasti IT**</span><span class="sxs-lookup"><span data-stu-id="1c6b4-640">**Pros**</span></span>

-   <span data-ttu-id="1c6b4-641">Vhodný pro operace vytvoření.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-641">Suitable for CUD operations.</span></span>
-   <span data-ttu-id="1c6b4-642">Plně materializovaného objekty.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-642">Fully materialized objects.</span></span>
-   <span data-ttu-id="1c6b4-643">Podporuje dotazování, ukládání do mezipaměti plánu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-643">Supports query plan caching.</span></span>

<span data-ttu-id="1c6b4-644">**Nevýhody**</span><span class="sxs-lookup"><span data-stu-id="1c6b4-644">**Cons**</span></span>

-   <span data-ttu-id="1c6b4-645">Zahrnuje dotazu textové řetězce, které jsou více náchylná k chybám uživatelů než konstrukce dotazů integrované do jazyka.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-645">Involves textual query strings which are more prone to user error than query constructs built into the language.</span></span>

### <a name="64-------entity-sql-over-an-entity-command"></a><span data-ttu-id="1c6b4-646">6.4 entity SQL přes příkaz Entity</span><span class="sxs-lookup"><span data-stu-id="1c6b4-646">6.4       Entity SQL over an Entity Command</span></span>

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

<span data-ttu-id="1c6b4-647">**V oblasti IT**</span><span class="sxs-lookup"><span data-stu-id="1c6b4-647">**Pros**</span></span>

-   <span data-ttu-id="1c6b4-648">Podporuje dotazování, ukládání do mezipaměti plánu v rozhraní .NET 4.0 (ukládání do mezipaměti plánu se podporuje další typy dotazů v rozhraní .NET 4.5).</span><span class="sxs-lookup"><span data-stu-id="1c6b4-648">Supports query plan caching in .NET 4.0 (plan caching is supported by all other query types in .NET 4.5).</span></span>

<span data-ttu-id="1c6b4-649">**Nevýhody**</span><span class="sxs-lookup"><span data-stu-id="1c6b4-649">**Cons**</span></span>

-   <span data-ttu-id="1c6b4-650">Zahrnuje dotazu textové řetězce, které jsou více náchylná k chybám uživatelů než konstrukce dotazů integrované do jazyka.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-650">Involves textual query strings which are more prone to user error than query constructs built into the language.</span></span>
-   <span data-ttu-id="1c6b4-651">Není vhodný pro operace vytvoření.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-651">Not suitable for CUD operations.</span></span>
-   <span data-ttu-id="1c6b4-652">Výsledky nejsou automaticky vyhodnocena a musí být četlo čtecí modul dat.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-652">Results are not automatically materialized, and must be read from the data reader.</span></span>

### <a name="65-------sqlquery-and-executestorequery"></a><span data-ttu-id="1c6b4-653">6.5 SqlQuery a ExecuteStoreQuery</span><span class="sxs-lookup"><span data-stu-id="1c6b4-653">6.5       SqlQuery and ExecuteStoreQuery</span></span>

<span data-ttu-id="1c6b4-654">SqlQuery v databázi:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-654">SqlQuery on Database:</span></span>

``` csharp
// use this to obtain entities and not track them
var q1 = context.Database.SqlQuery<Product>("select * from products");
```

<span data-ttu-id="1c6b4-655">SqlQuery na DbSet:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-655">SqlQuery on DbSet:</span></span>

``` csharp
// use this to obtain entities and have them tracked
var q2 = context.Products.SqlQuery("select * from products");
```

<span data-ttu-id="1c6b4-656">ExecyteStoreQuery:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-656">ExecyteStoreQuery:</span></span>

``` csharp
var beverages = context.ExecuteStoreQuery<Product>(
@"     SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued, P.DiscontinuedDate
       FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
       WHERE        (C.CategoryName = 'Beverages')"
);
```

<span data-ttu-id="1c6b4-657">**V oblasti IT**</span><span class="sxs-lookup"><span data-stu-id="1c6b4-657">**Pros**</span></span>

-   <span data-ttu-id="1c6b4-658">Obecně nejrychlejší výkon od plánu kompilátoru přeskočí.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-658">Generally fastest performance since plan compiler is bypassed.</span></span>
-   <span data-ttu-id="1c6b4-659">Plně materializovaného objekty.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-659">Fully materialized objects.</span></span>
-   <span data-ttu-id="1c6b4-660">Vhodný pro operace vytvoření při použití z DbSet.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-660">Suitable for CUD operations when used from the DbSet.</span></span>

<span data-ttu-id="1c6b4-661">**Nevýhody**</span><span class="sxs-lookup"><span data-stu-id="1c6b4-661">**Cons**</span></span>

-   <span data-ttu-id="1c6b4-662">Dotaz je textové a náchylné k chybám.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-662">Query is textual and error prone.</span></span>
-   <span data-ttu-id="1c6b4-663">Dotaz se váže na konkrétní back-endu s využitím úložiště sémantikou místo koncepční sémantiku.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-663">Query is tied to a specific backend by using store semantics instead of conceptual semantics.</span></span>
-   <span data-ttu-id="1c6b4-664">Při dědičnost je k dispozici, je potřeba účet pro mapování podmínky pro požadovaný typ rukodìlných dotazu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-664">When inheritance is present, handcrafted query needs to account for mapping conditions for the type requested.</span></span>

### <a name="66-------compiledquery"></a><span data-ttu-id="1c6b4-665">6.6 CompiledQuery</span><span class="sxs-lookup"><span data-stu-id="1c6b4-665">6.6       CompiledQuery</span></span>

``` csharp
private static readonly Func<NorthwindEntities, string, IQueryable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
    (NorthwindEntities context, string categoryName) =>
        context.Products.Where(p => p.Category.CategoryName == categoryName)
        );
…
var q = context.InvokeProductsForCategoryCQ("Beverages");
```

<span data-ttu-id="1c6b4-666">**V oblasti IT**</span><span class="sxs-lookup"><span data-stu-id="1c6b4-666">**Pros**</span></span>

-   <span data-ttu-id="1c6b4-667">Poskytuje až zlepšení výkonu 7 % prostřednictvím pravidelné dotazů LINQ.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-667">Provides up to a 7% performance improvement over regular LINQ queries.</span></span>
-   <span data-ttu-id="1c6b4-668">Plně materializovaného objekty.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-668">Fully materialized objects.</span></span>
-   <span data-ttu-id="1c6b4-669">Vhodný pro operace vytvoření.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-669">Suitable for CUD operations.</span></span>

<span data-ttu-id="1c6b4-670">**Nevýhody**</span><span class="sxs-lookup"><span data-stu-id="1c6b4-670">**Cons**</span></span>

-   <span data-ttu-id="1c6b4-671">Zvýšení složitosti a programování režijní náklady.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-671">Increased complexity and programming overhead.</span></span>
-   <span data-ttu-id="1c6b4-672">Zlepšení výkonu je ztracena při psaní nad kompilovaném dotazu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-672">The performance improvement is lost when composing on top of a compiled query.</span></span>
-   <span data-ttu-id="1c6b4-673">Některé dotazy LINQ nelze zapsat jako CompiledQuery – například projekcí anonymních typů.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-673">Some LINQ queries can't be written as a CompiledQuery - for example, projections of anonymous types.</span></span>

### <a name="67-------performance-comparison-of-different-query-options"></a><span data-ttu-id="1c6b4-674">6.7 porovnání výkonu možností jiný dotaz</span><span class="sxs-lookup"><span data-stu-id="1c6b4-674">6.7       Performance Comparison of different query options</span></span>

<span data-ttu-id="1c6b4-675">Byly jednoduché microbenchmarks, kde se vytvoření kontextu vypršel časový limit zařazení do testu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-675">Simple microbenchmarks where the context creation was not timed were put to the test.</span></span> <span data-ttu-id="1c6b4-676">Jsme měří dotazování 5000 časy pro sadu entit bez mezipaměti v řízeném prostředí.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-676">We measured querying 5000 times for a set of non-cached entities in a controlled environment.</span></span> <span data-ttu-id="1c6b4-677">Tato čísla jsou mají být provedeny s upozorněním: neodrážejí aktuální počet vytvářených aplikace, ale místo toho jsou jak velká část rozdíly ve výkonnosti se, pokud jsou porovnány různé možnosti dotazování velmi přesné měření jablka na apples, s výjimkou náklady na vytvoření nový kontext.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-677">These numbers are to be taken with a warning: they do not reflect actual numbers produced by an application, but instead they are a very accurate measurement of how much of a performance difference there is when different querying options are compared apples-to-apples, excluding the cost of creating a new context.</span></span>

| <span data-ttu-id="1c6b4-678">EF</span><span class="sxs-lookup"><span data-stu-id="1c6b4-678">EF</span></span>  | <span data-ttu-id="1c6b4-679">Test</span><span class="sxs-lookup"><span data-stu-id="1c6b4-679">Test</span></span>                                 | <span data-ttu-id="1c6b4-680">Doba (ms)</span><span class="sxs-lookup"><span data-stu-id="1c6b4-680">Time (ms)</span></span> | <span data-ttu-id="1c6b4-681">Paměť</span><span class="sxs-lookup"><span data-stu-id="1c6b4-681">Memory</span></span>   |
|:----|:-------------------------------------|:----------|:---------|
| <span data-ttu-id="1c6b4-682">EF5</span><span class="sxs-lookup"><span data-stu-id="1c6b4-682">EF5</span></span> | <span data-ttu-id="1c6b4-683">ObjectContext ESQL</span><span class="sxs-lookup"><span data-stu-id="1c6b4-683">ObjectContext ESQL</span></span>                   | <span data-ttu-id="1c6b4-684">2414</span><span class="sxs-lookup"><span data-stu-id="1c6b4-684">2414</span></span>      | <span data-ttu-id="1c6b4-685">38801408</span><span class="sxs-lookup"><span data-stu-id="1c6b4-685">38801408</span></span> |
| <span data-ttu-id="1c6b4-686">EF5</span><span class="sxs-lookup"><span data-stu-id="1c6b4-686">EF5</span></span> | <span data-ttu-id="1c6b4-687">Dotaz Linq ObjectContext</span><span class="sxs-lookup"><span data-stu-id="1c6b4-687">ObjectContext Linq Query</span></span>             | <span data-ttu-id="1c6b4-688">2692</span><span class="sxs-lookup"><span data-stu-id="1c6b4-688">2692</span></span>      | <span data-ttu-id="1c6b4-689">38277120</span><span class="sxs-lookup"><span data-stu-id="1c6b4-689">38277120</span></span> |
| <span data-ttu-id="1c6b4-690">EF5</span><span class="sxs-lookup"><span data-stu-id="1c6b4-690">EF5</span></span> | <span data-ttu-id="1c6b4-691">Žádné sledování dotazu DbContext Linq</span><span class="sxs-lookup"><span data-stu-id="1c6b4-691">DbContext Linq Query No Tracking</span></span>     | <span data-ttu-id="1c6b4-692">2818</span><span class="sxs-lookup"><span data-stu-id="1c6b4-692">2818</span></span>      | <span data-ttu-id="1c6b4-693">41840640</span><span class="sxs-lookup"><span data-stu-id="1c6b4-693">41840640</span></span> |
| <span data-ttu-id="1c6b4-694">EF5</span><span class="sxs-lookup"><span data-stu-id="1c6b4-694">EF5</span></span> | <span data-ttu-id="1c6b4-695">Dotaz Linq DbContext</span><span class="sxs-lookup"><span data-stu-id="1c6b4-695">DbContext Linq Query</span></span>                 | <span data-ttu-id="1c6b4-696">2930</span><span class="sxs-lookup"><span data-stu-id="1c6b4-696">2930</span></span>      | <span data-ttu-id="1c6b4-697">41771008</span><span class="sxs-lookup"><span data-stu-id="1c6b4-697">41771008</span></span> |
| <span data-ttu-id="1c6b4-698">EF5</span><span class="sxs-lookup"><span data-stu-id="1c6b4-698">EF5</span></span> | <span data-ttu-id="1c6b4-699">Objekt ObjectContext Linq dotaz bez sledování</span><span class="sxs-lookup"><span data-stu-id="1c6b4-699">ObjectContext Linq Query No Tracking</span></span> | <span data-ttu-id="1c6b4-700">3013</span><span class="sxs-lookup"><span data-stu-id="1c6b4-700">3013</span></span>      | <span data-ttu-id="1c6b4-701">38412288</span><span class="sxs-lookup"><span data-stu-id="1c6b4-701">38412288</span></span> |
|     |                                      |           |          |
| <span data-ttu-id="1c6b4-702">EF6</span><span class="sxs-lookup"><span data-stu-id="1c6b4-702">EF6</span></span> | <span data-ttu-id="1c6b4-703">ObjectContext ESQL</span><span class="sxs-lookup"><span data-stu-id="1c6b4-703">ObjectContext ESQL</span></span>                   | <span data-ttu-id="1c6b4-704">2059</span><span class="sxs-lookup"><span data-stu-id="1c6b4-704">2059</span></span>      | <span data-ttu-id="1c6b4-705">46039040</span><span class="sxs-lookup"><span data-stu-id="1c6b4-705">46039040</span></span> |
| <span data-ttu-id="1c6b4-706">EF6</span><span class="sxs-lookup"><span data-stu-id="1c6b4-706">EF6</span></span> | <span data-ttu-id="1c6b4-707">Dotaz Linq ObjectContext</span><span class="sxs-lookup"><span data-stu-id="1c6b4-707">ObjectContext Linq Query</span></span>             | <span data-ttu-id="1c6b4-708">3074</span><span class="sxs-lookup"><span data-stu-id="1c6b4-708">3074</span></span>      | <span data-ttu-id="1c6b4-709">45248512</span><span class="sxs-lookup"><span data-stu-id="1c6b4-709">45248512</span></span> |
| <span data-ttu-id="1c6b4-710">EF6</span><span class="sxs-lookup"><span data-stu-id="1c6b4-710">EF6</span></span> | <span data-ttu-id="1c6b4-711">Žádné sledování dotazu DbContext Linq</span><span class="sxs-lookup"><span data-stu-id="1c6b4-711">DbContext Linq Query No Tracking</span></span>     | <span data-ttu-id="1c6b4-712">3125</span><span class="sxs-lookup"><span data-stu-id="1c6b4-712">3125</span></span>      | <span data-ttu-id="1c6b4-713">47575040</span><span class="sxs-lookup"><span data-stu-id="1c6b4-713">47575040</span></span> |
| <span data-ttu-id="1c6b4-714">EF6</span><span class="sxs-lookup"><span data-stu-id="1c6b4-714">EF6</span></span> | <span data-ttu-id="1c6b4-715">Dotaz Linq DbContext</span><span class="sxs-lookup"><span data-stu-id="1c6b4-715">DbContext Linq Query</span></span>                 | <span data-ttu-id="1c6b4-716">3420</span><span class="sxs-lookup"><span data-stu-id="1c6b4-716">3420</span></span>      | <span data-ttu-id="1c6b4-717">47652864</span><span class="sxs-lookup"><span data-stu-id="1c6b4-717">47652864</span></span> |
| <span data-ttu-id="1c6b4-718">EF6</span><span class="sxs-lookup"><span data-stu-id="1c6b4-718">EF6</span></span> | <span data-ttu-id="1c6b4-719">Objekt ObjectContext Linq dotaz bez sledování</span><span class="sxs-lookup"><span data-stu-id="1c6b4-719">ObjectContext Linq Query No Tracking</span></span> | <span data-ttu-id="1c6b4-720">3593</span><span class="sxs-lookup"><span data-stu-id="1c6b4-720">3593</span></span>      | <span data-ttu-id="1c6b4-721">45260800</span><span class="sxs-lookup"><span data-stu-id="1c6b4-721">45260800</span></span> |

![EF5 micro srovnávací testy, 5000 teplé iterací](~/ef6/media/ef5micro5000warm.png)

![EF6 micro srovnávací testy, 5000 teplé iterací](~/ef6/media/ef6micro5000warm.png)

<span data-ttu-id="1c6b4-724">Microbenchmarks jsou velmi citlivé na malých změn v kódu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-724">Microbenchmarks are very sensitive to small changes in the code.</span></span> <span data-ttu-id="1c6b4-725">V tomto případě rozdíl mezi náklady na Entity Framework 5 a Entity Framework 6 jsou z důvodu přidání [zachycení](~/ef6/fundamentals/logging-and-interception.md) a [transakční vylepšení](~/ef6/saving/transactions.md).</span><span class="sxs-lookup"><span data-stu-id="1c6b4-725">In this case, the difference between the costs of Entity Framework 5 and Entity Framework 6 are due to the addition of [interception](~/ef6/fundamentals/logging-and-interception.md) and [transactional improvements](~/ef6/saving/transactions.md).</span></span> <span data-ttu-id="1c6b4-726">Tato čísla microbenchmarks jsou však zesilovací vizi do velmi malý fragment toho, co dělá rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-726">These microbenchmarks numbers, however, are an amplified vision into a very small fragment of what Entity Framework does.</span></span> <span data-ttu-id="1c6b4-727">Reálné scénáře teplé dotazů by se neměly zobrazovat regrese výkonu při upgradu z Entity Framework 5 na Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-727">Real-world scenarios of warm queries should not see a performance regression when upgrading from Entity Framework 5 to Entity Framework 6.</span></span>

<span data-ttu-id="1c6b4-728">Porovnat výkon skutečné možností jiný dotaz, vytvořili jsme 5 variace samostatný test, kde používáme možnost jiný dotaz, vyberte všechny produkty, jejichž název kategorie je "Nápoje".</span><span class="sxs-lookup"><span data-stu-id="1c6b4-728">To compare the real-world performance of the different query options, we created 5 separate test variations where we use a different query option to select all products whose category name is "Beverages".</span></span> <span data-ttu-id="1c6b4-729">Každá iterace zahrnuje náklady na vytvoření kontextu a náklady na materializaci všechny vrácené entity.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-729">Each iteration includes the cost of creating the context, and the cost of materializing all returned entities.</span></span> <span data-ttu-id="1c6b4-730">Před provedením součtu vypršel časový limit 1000 iterací jsou spuštěny untimed 10 iterací.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-730">10 iterations are run untimed before taking the sum of 1000 timed iterations.</span></span> <span data-ttu-id="1c6b4-731">Střední spustit z 5 spuštění každého testu jsou výsledky zobrazeny.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-731">The results shown are the median run taken from 5 runs of each test.</span></span> <span data-ttu-id="1c6b4-732">Další informace najdete v tématu dodatku B, který obsahuje kód pro test.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-732">For more information, see Appendix B which includes the code for the test.</span></span>

| <span data-ttu-id="1c6b4-733">EF</span><span class="sxs-lookup"><span data-stu-id="1c6b4-733">EF</span></span>  | <span data-ttu-id="1c6b4-734">Test</span><span class="sxs-lookup"><span data-stu-id="1c6b4-734">Test</span></span>                                        | <span data-ttu-id="1c6b4-735">Doba (ms)</span><span class="sxs-lookup"><span data-stu-id="1c6b4-735">Time (ms)</span></span> | <span data-ttu-id="1c6b4-736">Paměť</span><span class="sxs-lookup"><span data-stu-id="1c6b4-736">Memory</span></span>   |
|:----|:--------------------------------------------|:----------|:---------|
| <span data-ttu-id="1c6b4-737">EF5</span><span class="sxs-lookup"><span data-stu-id="1c6b4-737">EF5</span></span> | <span data-ttu-id="1c6b4-738">Příkaz ObjectContext Entity</span><span class="sxs-lookup"><span data-stu-id="1c6b4-738">ObjectContext Entity Command</span></span>                | <span data-ttu-id="1c6b4-739">621</span><span class="sxs-lookup"><span data-stu-id="1c6b4-739">621</span></span>       | <span data-ttu-id="1c6b4-740">39350272</span><span class="sxs-lookup"><span data-stu-id="1c6b4-740">39350272</span></span> |
| <span data-ttu-id="1c6b4-741">EF5</span><span class="sxs-lookup"><span data-stu-id="1c6b4-741">EF5</span></span> | <span data-ttu-id="1c6b4-742">Kontext databáze. dotaz Sql na databázi</span><span class="sxs-lookup"><span data-stu-id="1c6b4-742">DbContext Sql Query on Database</span></span>             | <span data-ttu-id="1c6b4-743">825</span><span class="sxs-lookup"><span data-stu-id="1c6b4-743">825</span></span>       | <span data-ttu-id="1c6b4-744">37519360</span><span class="sxs-lookup"><span data-stu-id="1c6b4-744">37519360</span></span> |
| <span data-ttu-id="1c6b4-745">EF5</span><span class="sxs-lookup"><span data-stu-id="1c6b4-745">EF5</span></span> | <span data-ttu-id="1c6b4-746">Query Store ObjectContext</span><span class="sxs-lookup"><span data-stu-id="1c6b4-746">ObjectContext Store Query</span></span>                   | <span data-ttu-id="1c6b4-747">878</span><span class="sxs-lookup"><span data-stu-id="1c6b4-747">878</span></span>       | <span data-ttu-id="1c6b4-748">39460864</span><span class="sxs-lookup"><span data-stu-id="1c6b4-748">39460864</span></span> |
| <span data-ttu-id="1c6b4-749">EF5</span><span class="sxs-lookup"><span data-stu-id="1c6b4-749">EF5</span></span> | <span data-ttu-id="1c6b4-750">Objekt ObjectContext Linq dotaz bez sledování</span><span class="sxs-lookup"><span data-stu-id="1c6b4-750">ObjectContext Linq Query No Tracking</span></span>        | <span data-ttu-id="1c6b4-751">969</span><span class="sxs-lookup"><span data-stu-id="1c6b4-751">969</span></span>       | <span data-ttu-id="1c6b4-752">38293504</span><span class="sxs-lookup"><span data-stu-id="1c6b4-752">38293504</span></span> |
| <span data-ttu-id="1c6b4-753">EF5</span><span class="sxs-lookup"><span data-stu-id="1c6b4-753">EF5</span></span> | <span data-ttu-id="1c6b4-754">Pomocí dotazu objektu ObjectContext Entity Sql</span><span class="sxs-lookup"><span data-stu-id="1c6b4-754">ObjectContext Entity Sql using Object Query</span></span> | <span data-ttu-id="1c6b4-755">1089</span><span class="sxs-lookup"><span data-stu-id="1c6b4-755">1089</span></span>      | <span data-ttu-id="1c6b4-756">38981632</span><span class="sxs-lookup"><span data-stu-id="1c6b4-756">38981632</span></span> |
| <span data-ttu-id="1c6b4-757">EF5</span><span class="sxs-lookup"><span data-stu-id="1c6b4-757">EF5</span></span> | <span data-ttu-id="1c6b4-758">Zkompilovaný dotaz ObjectContext</span><span class="sxs-lookup"><span data-stu-id="1c6b4-758">ObjectContext Compiled Query</span></span>                | <span data-ttu-id="1c6b4-759">1099</span><span class="sxs-lookup"><span data-stu-id="1c6b4-759">1099</span></span>      | <span data-ttu-id="1c6b4-760">38682624</span><span class="sxs-lookup"><span data-stu-id="1c6b4-760">38682624</span></span> |
| <span data-ttu-id="1c6b4-761">EF5</span><span class="sxs-lookup"><span data-stu-id="1c6b4-761">EF5</span></span> | <span data-ttu-id="1c6b4-762">Dotaz Linq ObjectContext</span><span class="sxs-lookup"><span data-stu-id="1c6b4-762">ObjectContext Linq Query</span></span>                    | <span data-ttu-id="1c6b4-763">1152</span><span class="sxs-lookup"><span data-stu-id="1c6b4-763">1152</span></span>      | <span data-ttu-id="1c6b4-764">38178816</span><span class="sxs-lookup"><span data-stu-id="1c6b4-764">38178816</span></span> |
| <span data-ttu-id="1c6b4-765">EF5</span><span class="sxs-lookup"><span data-stu-id="1c6b4-765">EF5</span></span> | <span data-ttu-id="1c6b4-766">Žádné sledování dotazu DbContext Linq</span><span class="sxs-lookup"><span data-stu-id="1c6b4-766">DbContext Linq Query No Tracking</span></span>            | <span data-ttu-id="1c6b4-767">1208</span><span class="sxs-lookup"><span data-stu-id="1c6b4-767">1208</span></span>      | <span data-ttu-id="1c6b4-768">41803776</span><span class="sxs-lookup"><span data-stu-id="1c6b4-768">41803776</span></span> |
| <span data-ttu-id="1c6b4-769">EF5</span><span class="sxs-lookup"><span data-stu-id="1c6b4-769">EF5</span></span> | <span data-ttu-id="1c6b4-770">Kontext databáze. dotaz Sql na DbSet</span><span class="sxs-lookup"><span data-stu-id="1c6b4-770">DbContext Sql Query on DbSet</span></span>                | <span data-ttu-id="1c6b4-771">1414</span><span class="sxs-lookup"><span data-stu-id="1c6b4-771">1414</span></span>      | <span data-ttu-id="1c6b4-772">37982208</span><span class="sxs-lookup"><span data-stu-id="1c6b4-772">37982208</span></span> |
| <span data-ttu-id="1c6b4-773">EF5</span><span class="sxs-lookup"><span data-stu-id="1c6b4-773">EF5</span></span> | <span data-ttu-id="1c6b4-774">Dotaz Linq DbContext</span><span class="sxs-lookup"><span data-stu-id="1c6b4-774">DbContext Linq Query</span></span>                        | <span data-ttu-id="1c6b4-775">1574</span><span class="sxs-lookup"><span data-stu-id="1c6b4-775">1574</span></span>      | <span data-ttu-id="1c6b4-776">41738240</span><span class="sxs-lookup"><span data-stu-id="1c6b4-776">41738240</span></span> |
|     |                                             |           |          |
| <span data-ttu-id="1c6b4-777">EF6</span><span class="sxs-lookup"><span data-stu-id="1c6b4-777">EF6</span></span> | <span data-ttu-id="1c6b4-778">Příkaz ObjectContext Entity</span><span class="sxs-lookup"><span data-stu-id="1c6b4-778">ObjectContext Entity Command</span></span>                | <span data-ttu-id="1c6b4-779">480</span><span class="sxs-lookup"><span data-stu-id="1c6b4-779">480</span></span>       | <span data-ttu-id="1c6b4-780">47247360</span><span class="sxs-lookup"><span data-stu-id="1c6b4-780">47247360</span></span> |
| <span data-ttu-id="1c6b4-781">EF6</span><span class="sxs-lookup"><span data-stu-id="1c6b4-781">EF6</span></span> | <span data-ttu-id="1c6b4-782">Query Store ObjectContext</span><span class="sxs-lookup"><span data-stu-id="1c6b4-782">ObjectContext Store Query</span></span>                   | <span data-ttu-id="1c6b4-783">493</span><span class="sxs-lookup"><span data-stu-id="1c6b4-783">493</span></span>       | <span data-ttu-id="1c6b4-784">46739456</span><span class="sxs-lookup"><span data-stu-id="1c6b4-784">46739456</span></span> |
| <span data-ttu-id="1c6b4-785">EF6</span><span class="sxs-lookup"><span data-stu-id="1c6b4-785">EF6</span></span> | <span data-ttu-id="1c6b4-786">Kontext databáze. dotaz Sql na databázi</span><span class="sxs-lookup"><span data-stu-id="1c6b4-786">DbContext Sql Query on Database</span></span>             | <span data-ttu-id="1c6b4-787">614</span><span class="sxs-lookup"><span data-stu-id="1c6b4-787">614</span></span>       | <span data-ttu-id="1c6b4-788">41607168</span><span class="sxs-lookup"><span data-stu-id="1c6b4-788">41607168</span></span> |
| <span data-ttu-id="1c6b4-789">EF6</span><span class="sxs-lookup"><span data-stu-id="1c6b4-789">EF6</span></span> | <span data-ttu-id="1c6b4-790">Objekt ObjectContext Linq dotaz bez sledování</span><span class="sxs-lookup"><span data-stu-id="1c6b4-790">ObjectContext Linq Query No Tracking</span></span>        | <span data-ttu-id="1c6b4-791">684</span><span class="sxs-lookup"><span data-stu-id="1c6b4-791">684</span></span>       | <span data-ttu-id="1c6b4-792">46333952</span><span class="sxs-lookup"><span data-stu-id="1c6b4-792">46333952</span></span> |
| <span data-ttu-id="1c6b4-793">EF6</span><span class="sxs-lookup"><span data-stu-id="1c6b4-793">EF6</span></span> | <span data-ttu-id="1c6b4-794">Pomocí dotazu objektu ObjectContext Entity Sql</span><span class="sxs-lookup"><span data-stu-id="1c6b4-794">ObjectContext Entity Sql using Object Query</span></span> | <span data-ttu-id="1c6b4-795">767</span><span class="sxs-lookup"><span data-stu-id="1c6b4-795">767</span></span>       | <span data-ttu-id="1c6b4-796">48865280</span><span class="sxs-lookup"><span data-stu-id="1c6b4-796">48865280</span></span> |
| <span data-ttu-id="1c6b4-797">EF6</span><span class="sxs-lookup"><span data-stu-id="1c6b4-797">EF6</span></span> | <span data-ttu-id="1c6b4-798">Zkompilovaný dotaz ObjectContext</span><span class="sxs-lookup"><span data-stu-id="1c6b4-798">ObjectContext Compiled Query</span></span>                | <span data-ttu-id="1c6b4-799">788</span><span class="sxs-lookup"><span data-stu-id="1c6b4-799">788</span></span>       | <span data-ttu-id="1c6b4-800">48467968</span><span class="sxs-lookup"><span data-stu-id="1c6b4-800">48467968</span></span> |
| <span data-ttu-id="1c6b4-801">EF6</span><span class="sxs-lookup"><span data-stu-id="1c6b4-801">EF6</span></span> | <span data-ttu-id="1c6b4-802">Žádné sledování dotazu DbContext Linq</span><span class="sxs-lookup"><span data-stu-id="1c6b4-802">DbContext Linq Query No Tracking</span></span>            | <span data-ttu-id="1c6b4-803">878</span><span class="sxs-lookup"><span data-stu-id="1c6b4-803">878</span></span>       | <span data-ttu-id="1c6b4-804">47554560</span><span class="sxs-lookup"><span data-stu-id="1c6b4-804">47554560</span></span> |
| <span data-ttu-id="1c6b4-805">EF6</span><span class="sxs-lookup"><span data-stu-id="1c6b4-805">EF6</span></span> | <span data-ttu-id="1c6b4-806">Dotaz Linq ObjectContext</span><span class="sxs-lookup"><span data-stu-id="1c6b4-806">ObjectContext Linq Query</span></span>                    | <span data-ttu-id="1c6b4-807">953</span><span class="sxs-lookup"><span data-stu-id="1c6b4-807">953</span></span>       | <span data-ttu-id="1c6b4-808">47632384</span><span class="sxs-lookup"><span data-stu-id="1c6b4-808">47632384</span></span> |
| <span data-ttu-id="1c6b4-809">EF6</span><span class="sxs-lookup"><span data-stu-id="1c6b4-809">EF6</span></span> | <span data-ttu-id="1c6b4-810">Kontext databáze. dotaz Sql na DbSet</span><span class="sxs-lookup"><span data-stu-id="1c6b4-810">DbContext Sql Query on DbSet</span></span>                | <span data-ttu-id="1c6b4-811">1023</span><span class="sxs-lookup"><span data-stu-id="1c6b4-811">1023</span></span>      | <span data-ttu-id="1c6b4-812">41992192</span><span class="sxs-lookup"><span data-stu-id="1c6b4-812">41992192</span></span> |
| <span data-ttu-id="1c6b4-813">EF6</span><span class="sxs-lookup"><span data-stu-id="1c6b4-813">EF6</span></span> | <span data-ttu-id="1c6b4-814">Dotaz Linq DbContext</span><span class="sxs-lookup"><span data-stu-id="1c6b4-814">DbContext Linq Query</span></span>                        | <span data-ttu-id="1c6b4-815">. 1290</span><span class="sxs-lookup"><span data-stu-id="1c6b4-815">1290</span></span>      | <span data-ttu-id="1c6b4-816">47529984</span><span class="sxs-lookup"><span data-stu-id="1c6b4-816">47529984</span></span> |


![EF5 teplé dotazu 1000 iterací](~/ef6/media/ef5warmquery1000.png)

![EF6 teplé dotazu 1000 iterací](~/ef6/media/ef6warmquery1000.png)

> [!NOTE]
> <span data-ttu-id="1c6b4-819">Pro úplnost jsme zahrnuli variace, kde jsme na EntityCommand spuštění dotazu Entity SQL.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-819">For completeness, we included a variation where we execute an Entity SQL query on an EntityCommand.</span></span> <span data-ttu-id="1c6b4-820">Ale protože výsledky nejsou vyhodnocena pro takové dotazy, porovnání, není nutně jablka jablka.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-820">However, because results are not materialized for such queries, the comparison isn't necessarily apples-to-apples.</span></span> <span data-ttu-id="1c6b4-821">Test zahrnuje aproximace pro materializaci vyzkoušet srovnávání spravedlivější.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-821">The test includes a close approximation to materializing to try making the comparison fairer.</span></span>

<span data-ttu-id="1c6b4-822">V tomto případě začátku do konce, Entity Framework 6 lepší výkon než Entity Framework 5 z důvodu vylepšení výkonu na několik částí zásobníku, včetně mnohem míň DbContext inicializace a rychlejší MetadataCollection&lt;T&gt; vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-822">In this end-to-end case, Entity Framework 6 outperforms Entity Framework 5 due to performance improvements made on several parts of the stack, including a much lighter DbContext initialization and faster MetadataCollection&lt;T&gt; lookups.</span></span>

## <a name="7-design-time-performance-considerations"></a><span data-ttu-id="1c6b4-823">Faktory ovlivňující výkon čas návrh 7</span><span class="sxs-lookup"><span data-stu-id="1c6b4-823">7 Design time performance considerations</span></span>

### <a name="71-------inheritance-strategies"></a><span data-ttu-id="1c6b4-824">7.1 strategie dědičnosti</span><span class="sxs-lookup"><span data-stu-id="1c6b4-824">7.1       Inheritance Strategies</span></span>

<span data-ttu-id="1c6b4-825">Dalším aspektem výkon při použití rozhraní Entity Framework je strategie dědičnosti, které používáte.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-825">Another performance consideration when using Entity Framework is the inheritance strategy you use.</span></span> <span data-ttu-id="1c6b4-826">Entity Framework podporuje 3 základní typy dědičnosti a jejich kombinace:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-826">Entity Framework supports 3 basic types of inheritance and their combinations:</span></span>

-   <span data-ttu-id="1c6b4-827">V řádku je reprezentované tabulky na hierarchii (TPH) – kde každý dědičnosti nastavit mapování na tabulku s sloupec diskriminátoru, který označuje, který konkrétní typ v hierarchii.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-827">Table per Hierarchy (TPH) – where each inheritance set maps to a table with a discriminator column to indicate which particular type in the hierarchy is being represented in the row.</span></span>
-   <span data-ttu-id="1c6b4-828">Tabulky podle typu (TPT) – kam každý typ má své vlastní tabulky v databázi. podřízené tabulky definovat pouze sloupce, které neobsahuje nadřazené tabulky.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-828">Table per Type (TPT) – where each type has its own table in the database; the child tables only define the columns that the parent table doesn’t contain.</span></span>
-   <span data-ttu-id="1c6b4-829">Tabulky podle třídy (TPC) – kam každý typ má své vlastní celou tabulku v databázi. podřízené tabulky definovat všechny jejich polí, včetně těch, které definovaný v nadřazené typy.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-829">Table per Class (TPC) – where each type has its own full table in the database; the child tables define all their fields, including those defined in parent types.</span></span>

<span data-ttu-id="1c6b4-830">Používá-li model dědičnosti TPT, dotazy, které se generují bude složitější než ty, které jsou generovány s dalšími strategiemi dědičnosti, což může způsobit na delší dobu provádění ve storu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-830">If your model uses TPT inheritance, the queries which are generated will be more complex than those that are generated with the other inheritance strategies, which may result on longer execution times on the store.</span></span>  <span data-ttu-id="1c6b4-831">Obecně bude trvat déle, vygenerujte dotazy TPT modelu a materializovat výsledných objektech.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-831">It will generally take longer to generate queries over a TPT model, and to materialize the resulting objects.</span></span>

<span data-ttu-id="1c6b4-832">"Důležité informace o výkonu při použití dědičnosti TPT (tabulka na jeden typ) v Entity Framework" najdete v příspěvku blogu MSDN: \< http://blogs.msdn.com/b/adonet/archive/2010/08/17/performance-considerations-when-using-tpt-table-per-type-inheritance-in-the-entity-framework.aspx>.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-832">See the "Performance Considerations when using TPT (Table per Type) Inheritance in the Entity Framework" MSDN blog post: \<http://blogs.msdn.com/b/adonet/archive/2010/08/17/performance-considerations-when-using-tpt-table-per-type-inheritance-in-the-entity-framework.aspx>.</span></span>

#### <a name="711-------avoiding-tpt-in-model-first-or-code-first-applications"></a><span data-ttu-id="1c6b4-833">7.1.1 vyhnout TPT v aplikacích první Model nebo Code First</span><span class="sxs-lookup"><span data-stu-id="1c6b4-833">7.1.1       Avoiding TPT in Model First or Code First applications</span></span>

<span data-ttu-id="1c6b4-834">Při vytváření modelu přes existující databázi, která má schéma TPT nemáte celou řadu možností.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-834">When you create a model over an existing database that has a TPT schema, you don't have many options.</span></span> <span data-ttu-id="1c6b4-835">Ale při vytváření aplikace pomocí modelu první nebo Code First, měli byste se vyhnout TPT dědičnost dopadům na výkon.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-835">But when creating an application using Model First or Code First, you should avoid TPT inheritance for performance concerns.</span></span>

<span data-ttu-id="1c6b4-836">Při použití modelu první v Průvodci návrháře entit, zobrazí se TPT jakékoli dědičnosti ve vašem modelu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-836">When you use Model First in the Entity Designer Wizard, you will get TPT for any inheritance in your model.</span></span> <span data-ttu-id="1c6b4-837">Pokud chcete přepnout na strategii TPH dědičnosti s první Model, můžete použít "Entity návrháře databáze generování Power Pack" k dispozici z Galerie sady Visual Studio ( \< http://visualstudiogallery.msdn.microsoft.com/df3541c3-d833-4b65-b942-989e7ec74c87/>).</span><span class="sxs-lookup"><span data-stu-id="1c6b4-837">If you want to switch to a TPH inheritance strategy with Model First, you can use the "Entity Designer Database Generation Power Pack" available from the Visual Studio Gallery ( \<http://visualstudiogallery.msdn.microsoft.com/df3541c3-d833-4b65-b942-989e7ec74c87/>).</span></span>

<span data-ttu-id="1c6b4-838">Při použití Code First pro konfiguraci mapování modelu s dědičnosti, EF použije TPH ve výchozím nastavení, proto všechny entity v hierarchii dědičnosti budou zmapována do stejné tabulky.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-838">When using Code First to configure the mapping of a model with inheritance, EF will use TPH by default, therefore all entities in the inheritance hierarchy will be mapped to the same table.</span></span> <span data-ttu-id="1c6b4-839">V části "Mapování s rozhraní Fluent API" z "Kód první v entitě Framework4.1" článek v časopise MSDN Magazine ( [ http://msdn.microsoft.com/magazine/hh126815.aspx ](https://msdn.microsoft.com/magazine/hh126815.aspx)) pro další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-839">See the "Mapping with the Fluent API" section of the "Code First in Entity Framework4.1" article in MSDN Magazine ( [http://msdn.microsoft.com/magazine/hh126815.aspx](https://msdn.microsoft.com/magazine/hh126815.aspx)) for more details.</span></span>

### <a name="72-------upgrading-from-ef4-to-improve-model-generation-time"></a><span data-ttu-id="1c6b4-840">7.2 upgrade z EF4 ke zlepšení generování modelu čas</span><span class="sxs-lookup"><span data-stu-id="1c6b4-840">7.2       Upgrading from EF4 to improve model generation time</span></span>

<span data-ttu-id="1c6b4-841">SQL Server – konkrétní zlepšení algoritmu, který generuje vrstvy úložiště (SSDL) modelu je k dispozici v Entity Framework 5 a 6 a jako aktualizace Entity Framework 4, při instalaci sady Visual Studio 2010 SP1.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-841">A SQL Server-specific improvement to the algorithm that generates the store-layer (SSDL) of the model is available in Entity Framework 5 and 6, and as an update to Entity Framework 4 when Visual Studio 2010 SP1 is installed.</span></span> <span data-ttu-id="1c6b4-842">Následující výsledky testů ukazují zlepšení při vytváření velmi velkých objemů modelu, v tomto případě Navision modelu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-842">The following test results demonstrate the improvement when generating a very big model, in this case the Navision model.</span></span> <span data-ttu-id="1c6b4-843">Další podrobnosti naleznete v tématu dodatku C.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-843">See Appendix C for more details about it.</span></span>

<span data-ttu-id="1c6b4-844">Model obsahuje sady 1005 entit a sad 4227 přidružení.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-844">The model contains 1005 entity sets and 4227 association sets.</span></span>

| <span data-ttu-id="1c6b4-845">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="1c6b4-845">Configuration</span></span>                              | <span data-ttu-id="1c6b4-846">Rozpis uplynulý čas</span><span class="sxs-lookup"><span data-stu-id="1c6b4-846">Breakdown of time consumed</span></span>                                                                                                                                               |
|:-------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="1c6b4-847">Visual Studio 2010, Entity Framework 4</span><span class="sxs-lookup"><span data-stu-id="1c6b4-847">Visual Studio 2010, Entity Framework 4</span></span>     | <span data-ttu-id="1c6b4-848">Generování souborů SSDL: 2 hr 27 min</span><span class="sxs-lookup"><span data-stu-id="1c6b4-848">SSDL Generation: 2 hr 27 min</span></span> <br/> <span data-ttu-id="1c6b4-849">Generování mapování: 1 sekunda</span><span class="sxs-lookup"><span data-stu-id="1c6b4-849">Mapping Generation: 1 second</span></span> <br/> <span data-ttu-id="1c6b4-850">CSDL generace: 1 sekunda</span><span class="sxs-lookup"><span data-stu-id="1c6b4-850">CSDL Generation: 1 second</span></span> <br/> <span data-ttu-id="1c6b4-851">ObjectLayer generace: 1 sekunda</span><span class="sxs-lookup"><span data-stu-id="1c6b4-851">ObjectLayer Generation: 1 second</span></span> <br/> <span data-ttu-id="1c6b4-852">Generování zobrazení: 2 h 14 min</span><span class="sxs-lookup"><span data-stu-id="1c6b4-852">View Generation: 2 h 14 min</span></span> |
| <span data-ttu-id="1c6b4-853">Visual Studio 2010 SP1, Entity Framework 4</span><span class="sxs-lookup"><span data-stu-id="1c6b4-853">Visual Studio 2010 SP1, Entity Framework 4</span></span> | <span data-ttu-id="1c6b4-854">Generování souborů SSDL: 1 sekunda</span><span class="sxs-lookup"><span data-stu-id="1c6b4-854">SSDL Generation: 1 second</span></span> <br/> <span data-ttu-id="1c6b4-855">Generování mapování: 1 sekunda</span><span class="sxs-lookup"><span data-stu-id="1c6b4-855">Mapping Generation: 1 second</span></span> <br/> <span data-ttu-id="1c6b4-856">CSDL generace: 1 sekunda</span><span class="sxs-lookup"><span data-stu-id="1c6b4-856">CSDL Generation: 1 second</span></span> <br/> <span data-ttu-id="1c6b4-857">ObjectLayer generace: 1 sekunda</span><span class="sxs-lookup"><span data-stu-id="1c6b4-857">ObjectLayer Generation: 1 second</span></span> <br/> <span data-ttu-id="1c6b4-858">Generování zobrazení: 1 hr 53 min.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-858">View Generation: 1 hr 53 min</span></span>   |
| <span data-ttu-id="1c6b4-859">Visual Studio 2013, rozhraní Entity Framework 5</span><span class="sxs-lookup"><span data-stu-id="1c6b4-859">Visual Studio 2013, Entity Framework 5</span></span>     | <span data-ttu-id="1c6b4-860">Generování souborů SSDL: 1 sekunda</span><span class="sxs-lookup"><span data-stu-id="1c6b4-860">SSDL Generation: 1 second</span></span> <br/> <span data-ttu-id="1c6b4-861">Generování mapování: 1 sekunda</span><span class="sxs-lookup"><span data-stu-id="1c6b4-861">Mapping Generation: 1 second</span></span> <br/> <span data-ttu-id="1c6b4-862">CSDL generace: 1 sekunda</span><span class="sxs-lookup"><span data-stu-id="1c6b4-862">CSDL Generation: 1 second</span></span> <br/> <span data-ttu-id="1c6b4-863">ObjectLayer generace: 1 sekunda</span><span class="sxs-lookup"><span data-stu-id="1c6b4-863">ObjectLayer Generation: 1 second</span></span> <br/> <span data-ttu-id="1c6b4-864">Generování zobrazení: 65 minut</span><span class="sxs-lookup"><span data-stu-id="1c6b4-864">View Generation: 65 minutes</span></span>    |
| <span data-ttu-id="1c6b4-865">Visual Studio 2013, rozhraní Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="1c6b4-865">Visual Studio 2013, Entity Framework 6</span></span>     | <span data-ttu-id="1c6b4-866">Generování souborů SSDL: 1 sekunda</span><span class="sxs-lookup"><span data-stu-id="1c6b4-866">SSDL Generation: 1 second</span></span> <br/> <span data-ttu-id="1c6b4-867">Generování mapování: 1 sekunda</span><span class="sxs-lookup"><span data-stu-id="1c6b4-867">Mapping Generation: 1 second</span></span> <br/> <span data-ttu-id="1c6b4-868">CSDL generace: 1 sekunda</span><span class="sxs-lookup"><span data-stu-id="1c6b4-868">CSDL Generation: 1 second</span></span> <br/> <span data-ttu-id="1c6b4-869">ObjectLayer generace: 1 sekunda</span><span class="sxs-lookup"><span data-stu-id="1c6b4-869">ObjectLayer Generation: 1 second</span></span> <br/> <span data-ttu-id="1c6b4-870">Generování zobrazení: 28 sekundách.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-870">View Generation: 28 seconds.</span></span>   |


<span data-ttu-id="1c6b4-871">Je vhodné poznamenat, že při generování souborů SSDL, zatížení je téměř zcela strávená na serveru SQL Server čeká na klientském počítači. vývojové nečinnosti výsledků k téhle akci vrátit ze serveru.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-871">It's worth noting that when generating the SSDL, the load is almost entirely spent on the SQL Server, while the client development machine is waiting idle for results to come back from the server.</span></span> <span data-ttu-id="1c6b4-872">Specializující by měl ocení zejména toto vylepšení.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-872">DBAs should particularly appreciate this improvement.</span></span> <span data-ttu-id="1c6b4-873">Je také vhodné poznamenat, že v podstatě náklady na generování modelu probíhá generování zobrazení nyní.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-873">It's also worth noting that essentially the entire cost of model generation takes place in View Generation now.</span></span>

### <a name="73-------splitting-large-models-with-database-first-and-model-first"></a><span data-ttu-id="1c6b4-874">7.3 nejprve rozdělení velkých modelů s databází a Model First</span><span class="sxs-lookup"><span data-stu-id="1c6b4-874">7.3       Splitting Large Models with Database First and Model First</span></span>

<span data-ttu-id="1c6b4-875">Jak se zvyšuje velikost modelu, na plochu návrháře stane nevypadala a obtížně použitelný.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-875">As model size increases, the designer surface becomes cluttered and difficult to use.</span></span> <span data-ttu-id="1c6b4-876">Obvykle považujeme modelu s více než 300 entit příliš velkou efektivní použití návrháře.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-876">We typically consider a model with more than 300 entities to be too large to effectively use the designer.</span></span> <span data-ttu-id="1c6b4-877">V následujícím příspěvku blogu popisuje několik možností pro rozdělení velkých modelů: \< http://blogs.msdn.com/b/adonet/archive/2008/11/25/working-with-large-models-in-entity-framework-part-2.aspx>.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-877">The following blog post describes several options for splitting large models: \<http://blogs.msdn.com/b/adonet/archive/2008/11/25/working-with-large-models-in-entity-framework-part-2.aspx>.</span></span>

<span data-ttu-id="1c6b4-878">Příspěvek byl zapsán pro první verzi Entity Frameworku, ale postup se vztahuje.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-878">The post was written for the first version of Entity Framework, but the steps still apply.</span></span>

### <a name="74-------performance-considerations-with-the-entity-data-source-control"></a><span data-ttu-id="1c6b4-879">7.4 důležité informace o výkonu s Entity Data Source Control</span><span class="sxs-lookup"><span data-stu-id="1c6b4-879">7.4       Performance considerations with the Entity Data Source Control</span></span>

<span data-ttu-id="1c6b4-880">Zaznamenali jsme případy vícevláknové výkonnostních a zátěžových testů ve kterém výkon webové aplikace pomocí ovládacího prvku EntityDataSource deteriorates výrazně.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-880">We've seen cases in multi-threaded performance and stress tests where the performance of a web application using the EntityDataSource Control deteriorates significantly.</span></span> <span data-ttu-id="1c6b4-881">Příčinou je, že třídu EntityDataSource platí MetadataWorkspace.LoadFromAssembly opakovaně volá na sestavení odkazuje webové aplikace ke zjištění typy použité jako entity.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-881">The underlying cause is that the EntityDataSource repeatedly calls MetadataWorkspace.LoadFromAssembly on the assemblies referenced by the Web application to discover the types to be used as entities.</span></span>

<span data-ttu-id="1c6b4-882">Řešením je nastavit ContextTypeName třídu EntityDataSource platí na název typu odvozené třídy objektu ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-882">The solution is to set the ContextTypeName of the EntityDataSource to the type name of your derived ObjectContext class.</span></span> <span data-ttu-id="1c6b4-883">Tím dojde k vypnutí mechanismus, který prohledá všechna odkazovaná sestavení pro typy entit.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-883">This turns off the mechanism that scans all referenced assemblies for entity types.</span></span>

<span data-ttu-id="1c6b4-884">Nastavení pole ContextTypeName zabrání také o funkční problém, ve kterém třídu EntityDataSource platí v rozhraní .NET 4.0 výjimce ReflectionTypeLoadException byla vyvolána při provádění jej nelze načíst typ z sestavení prostřednictvím reflexe.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-884">Setting the ContextTypeName field also prevents a functional problem where the EntityDataSource in .NET 4.0 throws a ReflectionTypeLoadException when it can't load a type from an assembly via reflection.</span></span> <span data-ttu-id="1c6b4-885">Tento problém byl vyřešen v rozhraní .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-885">This issue has been fixed in .NET 4.5.</span></span>

### <a name="75-------poco-entities-and-change-tracking-proxies"></a><span data-ttu-id="1c6b4-886">7.5 POCO entity a change tracking proxy servery</span><span class="sxs-lookup"><span data-stu-id="1c6b4-886">7.5       POCO entities and change tracking proxies</span></span>

<span data-ttu-id="1c6b4-887">Entity Framework umožňuje používat vlastní datové třídy společně s datový model bez provedení změn na datové třídy sami.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-887">Entity Framework enables you to use custom data classes together with your data model without making any modifications to the data classes themselves.</span></span> <span data-ttu-id="1c6b4-888">To znamená, že můžete použít "prostý staré" CLR objektů POCO, jako je například existujících objektů domény s datovým modelem.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-888">This means that you can use "plain-old" CLR objects (POCO), such as existing domain objects, with your data model.</span></span> <span data-ttu-id="1c6b4-889">Tyto POCO datových tříd (označované také jako ignorujících objekty), které jsou mapovány na subjekty, které jsou definovány v datovém modelu, podporují většinu stejného dotazu, vložit, aktualizovat a odstranit chování jako typy entit, které jsou generovány pomocí nástroje modelu Entity Data Model.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-889">These POCO data classes (also known as persistence-ignorant objects), which are mapped to entities that are defined in a data model, support most of the same query, insert, update, and delete behaviors as entity types that are generated by the Entity Data Model tools.</span></span>

<span data-ttu-id="1c6b4-890">Entity Framework můžete také vytvořit proxy třídy odvozené od POCO typy, které se používají, když chcete povolit funkce, jako je automatické sledování změn pro POCO entity a opožděné načítání.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-890">Entity Framework can also create proxy classes derived from your POCO types, which are used when you want to enable features such as lazy loading and automatic change tracking on POCO entities.</span></span> <span data-ttu-id="1c6b4-891">Třídy POCO musí splňovat určité požadavky umožňující Entity Framework pro použití proxy, jak je popsáno zde: [ http://msdn.microsoft.com/library/dd468057.aspx ](https://msdn.microsoft.com/library/dd468057.aspx).</span><span class="sxs-lookup"><span data-stu-id="1c6b4-891">Your POCO classes must meet certain requirements to allow Entity Framework to use proxies, as described here: [http://msdn.microsoft.com/library/dd468057.aspx](https://msdn.microsoft.com/library/dd468057.aspx).</span></span>

<span data-ttu-id="1c6b4-892">Možnost sledování proxy bude informovat správce stavu objektu pokaždé, když vlastnosti vaší entity má svou hodnotu mění, takže Entity Framework neustále ví skutečného stavu entity.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-892">Chance tracking proxies will notify the object state manager each time any of the properties of your entities has its value changed, so Entity Framework knows the actual state of your entities all the time.</span></span> <span data-ttu-id="1c6b4-893">To se provádí přidáním oznámení událostí do těla metody setter vlastnosti, a s object Manageru stav zpracování těchto událostí.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-893">This is done by adding notification events to the body of the setter methods of your properties, and having the object state manager processing such events.</span></span> <span data-ttu-id="1c6b4-894">Všimněte si, že vytváření proxy entity se obvykle být dražší než vytvoříte entitu POCO bez proxy serveru z důvodu přidání sadu událostí, které jsou vytvořené Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-894">Note that creating a proxy entity will typically be more expensive than creating a non-proxy POCO entity due to the added set of events created by Entity Framework.</span></span>

<span data-ttu-id="1c6b4-895">Entita POCO nemá proxy sledování změn, změny se nacházejí porovnáním obsah entity kopii předchozímu uloženému stavu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-895">When a POCO entity does not have a change tracking proxy, changes are found by comparing the contents of your entities against a copy of a previous saved state.</span></span> <span data-ttu-id="1c6b4-896">Podrobné porovnání se stanou časově náročný proces Pokud máte mnoho entit ve vaší místní, nebo když vaše entity obsahují velmi velké množství vlastností, i v případě, že žádná z nich změněn od posledního porovnání konal úplně.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-896">This deep comparison will become a lengthy process when you have many entities in your context, or when your entities have a very large amount of properties, even if none of them changed since the last comparison took place.</span></span>

<span data-ttu-id="1c6b4-897">Stručně řečeno: zaplatíte výkonu při vytváření proxy sledování změn, ale řešení change tracking vám pomůže urychlit proces zjišťování změn při entity mají mnoho vlastností nebo pokud máte mnoho entit v modelu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-897">In summary: you’ll pay a performance hit when creating the change tracking proxy, but change tracking will help you speed up the change detection process when your entities have many properties or when you have many entities in your model.</span></span> <span data-ttu-id="1c6b4-898">U entit s malý počet vlastností, kde velikost entity není růst příliš mnoho proxy sledování změn nemusí být mnoho výhod.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-898">For entities with a small number of properties where the amount of entities doesn’t grow too much, having change tracking proxies may not be of much benefit.</span></span>

## <a name="8-loading-related-entities"></a><span data-ttu-id="1c6b4-899">8 načtení souvisejících entit</span><span class="sxs-lookup"><span data-stu-id="1c6b4-899">8 Loading Related Entities</span></span>

### <a name="81-lazy-loading-vs-eager-loading"></a><span data-ttu-id="1c6b4-900">8.1 opožděné načtení vs. Předběžné načítání</span><span class="sxs-lookup"><span data-stu-id="1c6b4-900">8.1 Lazy Loading vs. Eager Loading</span></span>

<span data-ttu-id="1c6b4-901">Entity Framework nabízí několik různých způsobů načítání entit, které se vztahují na cílovou entitu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-901">Entity Framework offers several different ways to load the entities that are related to your target entity.</span></span> <span data-ttu-id="1c6b4-902">Například při dotazování u produktů, existují různé způsoby, že související objednávky se načtou do Správce stavu objektu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-902">For example, when you query for Products, there are different ways that the related Orders will be loaded into the Object State Manager.</span></span> <span data-ttu-id="1c6b4-903">Z hlediska výkonu bude největší dotaz a vezměte v úvahu při načítání související entity, jestli se má použít opožděné načtení nebo nemůžou dočkat, až načítání.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-903">From a performance standpoint, the biggest question to consider when loading related entities will be whether to use Lazy Loading or Eager Loading.</span></span>

<span data-ttu-id="1c6b4-904">Při použití nemůžou dočkat, až načítání, související entity načtou spolu s cílovou sadou entit.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-904">When using Eager Loading, the related entities are loaded along with your target entity set.</span></span> <span data-ttu-id="1c6b4-905">K označení, která související entity, které chcete připojit použijete příkaz Include v dotazu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-905">You use an Include statement in your query to indicate which related entities you want to bring in.</span></span>

<span data-ttu-id="1c6b4-906">Při použití opožděné načtení, připojí počátečního dotazu pouze v cílové sady entit.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-906">When using Lazy Loading, your initial query only brings in the target entity set.</span></span> <span data-ttu-id="1c6b4-907">Ale kdykoli budete přistupovat k vlastnosti navigace, jiný dotaz je vydaný pro úložiště se načíst související entity.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-907">But whenever you access a navigation property, another query is issued against the store to load the related entity.</span></span>

<span data-ttu-id="1c6b4-908">Po načtení entity, žádné další dotazy entity se načtení přímo ze Správce stavu objektu, jestli používáte opožděné načtení nebo předběžné načítání.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-908">Once an entity has been loaded, any further queries for the entity will load it directly from the Object State Manager, whether you are using lazy loading or eager loading.</span></span>

### <a name="82-how-to-choose-between-lazy-loading-and-eager-loading"></a><span data-ttu-id="1c6b4-909">8.2 návodu k výběru mezi opožděné načtení a nemůžou dočkat, až načítání</span><span class="sxs-lookup"><span data-stu-id="1c6b4-909">8.2 How to choose between Lazy Loading and Eager Loading</span></span>

<span data-ttu-id="1c6b4-910">Důležité je, když rozumíte rozdílu mezi opožděné načtení a nemůžou dočkat, až načítání, tak, aby měli správnou volbu pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-910">The important thing is that you understand the difference between Lazy Loading and Eager Loading so that you can make the correct choice for your application.</span></span> <span data-ttu-id="1c6b4-911">To vám pomůže vyhodnotit kompromis mezi více požadavků na databázi a jeden požadavek, který může obsahovat velké datové části.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-911">This will help you evaluate the tradeoff between multiple requests against the database versus a single request that may contain a large payload.</span></span> <span data-ttu-id="1c6b4-912">Může být vhodné je použít v ostatních částech nemůžou dočkat, až načítání v některé části vaší aplikace a opožděné načtení.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-912">It may be appropriate to use eager loading in some parts of your application and lazy loading in other parts.</span></span>

<span data-ttu-id="1c6b4-913">Jako příklad toho, co se děje pod pokličkou Předpokládejme, že chcete zadat dotaz pro zákazníky, kteří žijí v Spojeném království a počtu jejich pořadí.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-913">As an example of what's happening under the hood, suppose you want to query for the customers who live in the UK and their order count.</span></span>

<span data-ttu-id="1c6b4-914">**Pomocí předběžné načítání**</span><span class="sxs-lookup"><span data-stu-id="1c6b4-914">**Using Eager Loading**</span></span>

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var ukCustomers = context.Customers.Include(c => c.Orders).Where(c => c.Address.Country == "UK");
    var chosenCustomer = AskUserToPickCustomer(ukCustomers);
    Console.WriteLine("Customer Id: {0} has {1} orders", customer.CustomerID, customer.Orders.Count);
}
```

<span data-ttu-id="1c6b4-915">**Pomocí opožděné načtení**</span><span class="sxs-lookup"><span data-stu-id="1c6b4-915">**Using Lazy Loading**</span></span>

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

<span data-ttu-id="1c6b4-916">Pokud používáte předběžné načítání, budete vydávat jeden dotaz, který vrátí všechny zákazníky a všechny objednávky.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-916">When using eager loading, you'll issue a single query that returns all customers and all orders.</span></span> <span data-ttu-id="1c6b4-917">Příkaz úložiště vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-917">The store command looks like:</span></span>

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

<span data-ttu-id="1c6b4-918">Při použití opožděné načtení, budete nejdřív vydejte následující dotaz:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-918">When using lazy loading, you'll issue the following query initially:</span></span>

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

<span data-ttu-id="1c6b4-919">A pokaždé, když přistupujete navigační vlastnost objednávky zákazníka úložišti uživatelů, kteří je vydán jiný dotaz podobný tomuto:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-919">And each time you access the Orders navigation property of a customer another query like the following is issued against the store:</span></span>

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

<span data-ttu-id="1c6b4-920">Další informace najdete v tématu [načítání související objekty](https://msdn.microsoft.com/library/bb896272.aspx).</span><span class="sxs-lookup"><span data-stu-id="1c6b4-920">For more information, see the [Loading Related Objects](https://msdn.microsoft.com/library/bb896272.aspx).</span></span>

#### <a name="821-lazy-loading-versus-eager-loading-cheat-sheet"></a><span data-ttu-id="1c6b4-921">8.2.1 opožděné načtení a nemůžou dočkat, až načítání tahák</span><span class="sxs-lookup"><span data-stu-id="1c6b4-921">8.2.1 Lazy Loading versus Eager Loading cheat sheet</span></span>

<span data-ttu-id="1c6b4-922">Není žádná taková věc, kterou jako univerzální výběrem předběžné načítání a opožděné načtení.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-922">There’s no such thing as a one-size-fits-all to choosing eager loading versus lazy loading.</span></span> <span data-ttu-id="1c6b4-923">Zkuste nejdřív znát rozdíly mezi obě strategie tedy můžete také přijímat podložená rozhodnutí; Zvažte také, pokud váš kód odpovídá podmínkám ke kterékoli z následujících scénářů:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-923">Try first to understand the differences between both strategies so you can do a well informed decision; also, consider if your code fits to any of the following scenarios:</span></span>

| <span data-ttu-id="1c6b4-924">Scénář</span><span class="sxs-lookup"><span data-stu-id="1c6b4-924">Scenario</span></span>                                                                    | <span data-ttu-id="1c6b4-925">Náš návrh</span><span class="sxs-lookup"><span data-stu-id="1c6b4-925">Our Suggestion</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|:----------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="1c6b4-926">Je potřeba pro přístup k mnoha navigační vlastnosti z načtených entity?</span><span class="sxs-lookup"><span data-stu-id="1c6b4-926">Do you need to access many navigation properties from the fetched entities?</span></span> | <span data-ttu-id="1c6b4-927">**Ne** -pravděpodobné bude, že obě možnosti.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-927">**No** - Both options will probably do.</span></span> <span data-ttu-id="1c6b4-928">Nicméně pokud datová část, kterou přináší váš dotaz není příliš velký, že může docházet přinese zlepšení výkonu pomocí předběžné načítání, jak tomu budete potřebovat méně síti, zpomalí sloučit objekty.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-928">However, if the payload your query is bringing is not too big, you may experience performance benefits by using Eager loading as it’ll require less network round trips to materialize your objects.</span></span> <br/> <br/> <span data-ttu-id="1c6b4-929">**Ano** – Pokud je potřeba přístup k mnoha navigační vlastnosti z entity, provedli byste, že pomocí několika #include v dotazu se nemůžou dočkat, až načítání.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-929">**Yes** -  If you need to access many navigation properties from the entities, you’d do that by using multiple include statements in your query with Eager loading.</span></span> <span data-ttu-id="1c6b4-930">Zahrnout další entity, že čím větší datovou část dotaz vrátí.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-930">The more entities you include, the bigger the payload your query will return.</span></span> <span data-ttu-id="1c6b4-931">Jakmile zahrnete tři nebo více entit do vašeho dotazu, zvažte možnost opožděné načtení.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-931">Once you include three or more entities into your query, consider switching to Lazy loading.</span></span> |
| <span data-ttu-id="1c6b4-932">Víte, jaká data přesně bude potřeba v době běhu?</span><span class="sxs-lookup"><span data-stu-id="1c6b4-932">Do you know exactly what data will be needed at run time?</span></span>                   | <span data-ttu-id="1c6b4-933">**Ne** – opožděné načtení bude lepší za vás.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-933">**No** - Lazy loading will be better for you.</span></span> <span data-ttu-id="1c6b4-934">Jinak můžete skončit dotazování na data, že nebudete potřebovat.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-934">Otherwise, you may end up querying for data that you will not need.</span></span> <br/> <br/> <span data-ttu-id="1c6b4-935">**Ano** – nemůžou dočkat, až načítání je pravděpodobně nejlepším řešením; pomůže to zrychlení načítání celé sady.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-935">**Yes** - Eager loading is probably your best bet; it will help loading entire sets faster.</span></span> <span data-ttu-id="1c6b4-936">Pokud váš dotaz vyžaduje načítají velké množství dat a toto řešení je příliš pomalé, zkuste opožděné načtení místo.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-936">If your query requires fetching a very large amount of data, and this becomes too slow, then try Lazy loading instead.</span></span>                                                                                                                                                                                                                                                       |
| <span data-ttu-id="1c6b4-937">Váš kód spouští daleko od databáze?</span><span class="sxs-lookup"><span data-stu-id="1c6b4-937">Is your code executing far from your database?</span></span> <span data-ttu-id="1c6b4-938">(latence sítě)</span><span class="sxs-lookup"><span data-stu-id="1c6b4-938">(increased network latency)</span></span>  | <span data-ttu-id="1c6b4-939">**Ne** – Pokud latence sítě není problém, pomocí opožděné načtení může zjednodušit kód.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-939">**No** - When the network latency is not an issue, using Lazy loading may simplify your code.</span></span> <span data-ttu-id="1c6b4-940">Mějte na paměti, že topologii vaší aplikace se může změnit postupně nevyřídí databáze blízkosti samozřejmost tedy.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-940">Remember that the topology of your application may change, so don’t take database proximity for granted.</span></span> <br/> <br/> <span data-ttu-id="1c6b4-941">**Ano** – Pokud síť není problém, pouze se můžete rozhodnout, co lépe vyhovuje vašemu scénáři.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-941">**Yes** - When the network is a problem, only you can decide what fits better for your scenario.</span></span> <span data-ttu-id="1c6b4-942">Předběžné načítání obvykle bude lepší, protože vyžaduje menší počet zpátečních cest.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-942">Typically Eager loading will be better because it requires fewer round trips.</span></span>                                                                                                                                                                                                      |


#### <a name="822-------performance-concerns-with-multiple-includes"></a><span data-ttu-id="1c6b4-943">8.2.2 aspekty výkonu s více zahrnuje</span><span class="sxs-lookup"><span data-stu-id="1c6b4-943">8.2.2       Performance concerns with multiple Includes</span></span>

<span data-ttu-id="1c6b4-944">Slyšeli jsme, že otázky výkonu, které zahrnují problémech čas odpovědi serveru, příčinu problému při často dotazy s více příkazy Include.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-944">When we hear performance questions that involve server response time problems, the source of the issue is frequently queries with multiple Include statements.</span></span> <span data-ttu-id="1c6b4-945">Zatímco včetně souvisejících entit v dotazu je efektivní, je důležité pochopit, co se děje na pozadí.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-945">While including related entities in a query is powerful, it's important to understand what's happening under the covers.</span></span>

<span data-ttu-id="1c6b4-946">Trvá poměrně dlouho pro dotaz s více příkazy Include tak, projděte si naše interní plán kompilátor vytvoří příkaz úložiště.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-946">It takes a relatively long time for a query with multiple Include statements in it to go through our internal plan compiler to produce the store command.</span></span> <span data-ttu-id="1c6b4-947">Většina této doby se věnovalo snaze optimalizovat výsledný dotaz.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-947">The majority of this time is spent trying to optimize the resulting query.</span></span> <span data-ttu-id="1c6b4-948">Příkaz generované úložiště bude obsahovat Outer Join nebo sjednocení pro každé zahrnutí, v závislosti na vaší mapování.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-948">The generated store command will contain an Outer Join or Union for each Include, depending on your mapping.</span></span> <span data-ttu-id="1c6b4-949">Dotazy tímto způsobem bude přenést velkých grafů připojených z databáze v jedné datové části, která bude acerbate jakékoli potíže se šířkou pásma, zejména v případě, že dochází k mnoha redundance v datové části (například pokud několik úrovní zahrnout se používá k procházení přidružení ve směru 1 n).</span><span class="sxs-lookup"><span data-stu-id="1c6b4-949">Queries like this will bring in large connected graphs from your database in a single payload, which will acerbate any bandwidth issues, especially when there is a lot of redundancy in the payload (for example, when multiple levels of Include are used to traverse associations in the one-to-many direction).</span></span>

<span data-ttu-id="1c6b4-950">Můžete zkontrolovat pro případy, kde dotazů jako nadměrně velké datové části vrací tak přístup k podkladové TSQL pro dotaz s použitím ToTraceString a spouští se příkaz úložiště v SQL Server Management Studio zobrazíte velikost datové části.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-950">You can check for cases where your queries are returning excessively large payloads by accessing the underlying TSQL for the query by using ToTraceString and executing the store command in SQL Server Management Studio to see the payload size.</span></span> <span data-ttu-id="1c6b4-951">V takovém případě můžete zkusit snížit počet vložených příkazů v dotazu jenom umožňuje přinést si data, která potřebujete.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-951">In such cases you can try to reduce the number of Include statements in your query to just bring in the data you need.</span></span> <span data-ttu-id="1c6b4-952">Nebo je možné rozdělit svůj dotaz na menší posloupnost poddotazy, například:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-952">Or you may be able to break your query into a smaller sequence of subqueries, for example:</span></span>

<span data-ttu-id="1c6b4-953">**Před dopadem na dřívější kód dotazu:**</span><span class="sxs-lookup"><span data-stu-id="1c6b4-953">**Before breaking the query:**</span></span>

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

<span data-ttu-id="1c6b4-954">**Po rozdělení dotazu:**</span><span class="sxs-lookup"><span data-stu-id="1c6b4-954">**After breaking the query:**</span></span>

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

<span data-ttu-id="1c6b4-955">Bude to fungovat jenom u sledovaných dotazy, jak provádíme použijte možnost kontextu má provádět překlad IP adres a přidružení oprava identity.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-955">This will work only on tracked queries, as we are making use of the ability the context has to perform identity resolution and association fixup automatically.</span></span>

<span data-ttu-id="1c6b4-956">Stejně jako u opožděné načtení, bude kompromis více dotazů pro menší datové části.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-956">As with lazy loading, the tradeoff will be more queries for smaller payloads.</span></span> <span data-ttu-id="1c6b4-957">Projekce jednotlivé vlastnosti můžete použít také explicitně vybrat pouze potřebná data z jednotlivých entit, ale můžete nesmí být načítání entit v tomto případě a aktualizace nebudou podporovány.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-957">You can also use projections of individual properties to explicitly select only the data you need from each entity, but you will not be loading entities in this case, and updates will not be supported.</span></span>

#### <a name="823-workaround-to-get-lazy-loading-of-properties"></a><span data-ttu-id="1c6b4-958">8.2.3 alternativní řešení Chcete-li získat opožděné načtení vlastností</span><span class="sxs-lookup"><span data-stu-id="1c6b4-958">8.2.3 Workaround to get lazy loading of properties</span></span>

<span data-ttu-id="1c6b4-959">Entity Framework v současné době nepodporuje opožděné načtení skalární nebo komplexní vlastností.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-959">Entity Framework currently doesn’t support lazy loading of scalar or complex properties.</span></span> <span data-ttu-id="1c6b4-960">Ale v případech, kdy máte tabulku, která zahrnuje velkých objektů, jako je například objekt BLOB, můžete rozdělení tabulky rozčlenit velké vlastnosti do samostatné entity.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-960">However, in cases where you have a table that includes a large object such as a BLOB, you can use table splitting to separate the large properties into a separate entity.</span></span> <span data-ttu-id="1c6b4-961">Předpokládejme například, že máte tabulku produktů, která zahrnuje sloupce varbinary fotografii.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-961">For example, suppose you have a Product table that includes a varbinary photo column.</span></span> <span data-ttu-id="1c6b4-962">Pokud nepotřebujete často se k této vlastnosti v dotazech, můžete použít pro entity, která je obvykle třeba jen ty části rozdělení tabulky.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-962">If you don't frequently need to access this property in your queries, you can use table splitting to bring in only the parts of the entity that you normally need.</span></span> <span data-ttu-id="1c6b4-963">Entita, která představuje fotografie produktu se načtou jenom když ho potřebujete explicitně.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-963">The entity representing the product photo will only be loaded when you explicitly need it.</span></span>

<span data-ttu-id="1c6b4-964">Dobrý prostředek, který ukazuje, jak povolit rozdělení tabulky je Gil Fink "Tabulky rozdělení v Entity Framework" blogový příspěvek: \< http://blogs.microsoft.co.il/blogs/gilf/archive/2009/10/13/table-splitting-in-entity-framework.aspx>.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-964">A good resource that shows how to enable table splitting is Gil Fink's "Table Splitting in Entity Framework" blog post: \<http://blogs.microsoft.co.il/blogs/gilf/archive/2009/10/13/table-splitting-in-entity-framework.aspx>.</span></span>

## <a name="9-other-considerations"></a><span data-ttu-id="1c6b4-965">9 další důležité informace</span><span class="sxs-lookup"><span data-stu-id="1c6b4-965">9 Other considerations</span></span>

### <a name="91------server-garbage-collection"></a><span data-ttu-id="1c6b4-966">9.1 uvolnění paměti serveru</span><span class="sxs-lookup"><span data-stu-id="1c6b4-966">9.1      Server Garbage Collection</span></span>

<span data-ttu-id="1c6b4-967">Někteří uživatelé setkat sporu prostředků, která omezuje paralelismu, které jsou se očekává při uvolňování paměti není správně nakonfigurována.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-967">Some users might experience resource contention that limits the parallelism they are expecting when the Garbage Collector is not properly configured.</span></span> <span data-ttu-id="1c6b4-968">Pokaždé, když EF se používá ve scénáři s více vlákny, nebo v jakékoli aplikaci, která vypadá podobně jako na straně serveru systému, ujistěte se, že chcete povolit uvolnění paměti serveru.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-968">Whenever EF is used in a multithreaded scenario, or in any application that resembles a server-side system, make sure to enable Server Garbage Collection.</span></span> <span data-ttu-id="1c6b4-969">To se provádí prostřednictvím jednoduché nastavení v konfiguračním souboru aplikace:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-969">This is done via a simple setting in your application config file:</span></span>

``` xml
<?xmlversion="1.0" encoding="utf-8" ?>
<configuration>
        <runtime>
               <gcServer enabled="true" />
        </runtime>
</configuration>
```

<span data-ttu-id="1c6b4-970">To by měla snížit vaše spor vlákna a zvýšit propustnost až o 30 % ve scénářích procesoru přeplněný.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-970">This should decrease your thread contention and increase your throughput by up to 30% in CPU saturated scenarios.</span></span> <span data-ttu-id="1c6b4-971">Obecně řečeno byste měli vždy otestovat chování aplikací pomocí klasické kolekce uvolnění paměti (která je vyladěná lépe pro scénáře na straně uživatelského rozhraní a klient) a také kolekce volnění paměti serveru.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-971">In general terms, you should always test how your application behaves using the classic Garbage Collection (which is better tuned for UI and client side scenarios) as well as the Server Garbage Collection.</span></span>

### <a name="92------autodetectchanges"></a><span data-ttu-id="1c6b4-972">9.2 AutoDetectChanges</span><span class="sxs-lookup"><span data-stu-id="1c6b4-972">9.2      AutoDetectChanges</span></span>

<span data-ttu-id="1c6b4-973">Jak už bylo zmíněno dříve, Entity Framework může zobrazit problémy s výkonem při mezipaměti objektů má mnoho entit.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-973">As mentioned earlier, Entity Framework might show performance issues when the object cache has many entities.</span></span> <span data-ttu-id="1c6b4-974">Některé operace, jako je například přidat, odebrat, hledání, vstupu a SaveChanges, aktivovat volání metoda DetectChanges, které může využívat velké procento využití procesoru podle jak velký stal mezipaměti objektů.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-974">Certain operations, such as Add, Remove, Find, Entry and SaveChanges, trigger calls to DetectChanges which might consume a large amount of CPU based on how large the object cache has become.</span></span> <span data-ttu-id="1c6b4-975">Důvodem je, že mezipaměti objektů a zkuste správce stavu objekt zůstat jako synchronizují nejvíce na každou operace prováděné na kontext, tak, aby vyprodukované dat je záruku správnosti v rámci široké škály scénářů.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-975">The reason for this is that the object cache and the object state manager try to stay as synchronized as possible on each operation performed to a context so that the produced data is guaranteed to be correct under a wide array of scenarios.</span></span>

<span data-ttu-id="1c6b4-976">Obecně je vhodné ponechat Entity Framework automaticky změnit detekce povolené pro celou dobu životnosti vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-976">It is generally a good practice to leave Entity Framework’s automatic change detection enabled for the entire life of your application.</span></span> <span data-ttu-id="1c6b4-977">Pokud váš scénář bude negativně ovlivněna podle vysoké využití procesoru a profily znamenat, že nadměrné spotřeby volání metoda DetectChanges, zvažte dočasné vypnutí AutoDetectChanges v citlivých část kódu:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-977">If your scenario is being negatively affected by high CPU usage and your profiles indicate that the culprit is the call to DetectChanges, consider temporarily turning off AutoDetectChanges in the sensitive portion of your code:</span></span>

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

<span data-ttu-id="1c6b4-978">Před vypnutím AutoDetectChanges, je vhodné pochopit, že to může vést ke ztrátě schopnosti ke sledování určité informace o změnách, které budou probíhat na entity Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-978">Before turning off AutoDetectChanges, it’s good to understand that this might cause Entity Framework to lose its ability to track certain information about the changes that are taking place on the entities.</span></span> <span data-ttu-id="1c6b4-979">Pokud nesprávně zpracována, tato akce může způsobit nekonzistenci dat ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-979">If handled incorrectly, this might cause data inconsistency on your application.</span></span> <span data-ttu-id="1c6b4-980">Další informace o vypnutí AutoDetectChanges najdete v článku \< http://blog.oneunicorn.com/2012/03/12/secrets-of-detectchanges-part-3-switching-off-automatic-detectchanges/>.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-980">For more information on turning off AutoDetectChanges, read \<http://blog.oneunicorn.com/2012/03/12/secrets-of-detectchanges-part-3-switching-off-automatic-detectchanges/>.</span></span>

### <a name="93------context-per-request"></a><span data-ttu-id="1c6b4-981">9.3 kontext každý požadavek</span><span class="sxs-lookup"><span data-stu-id="1c6b4-981">9.3      Context per request</span></span>

<span data-ttu-id="1c6b4-982">Kontext Entity Framework jsou určené pro použití jako krátkodobé a jednorázové instance, aby bylo možné poskytovat optimální výkon prostředí.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-982">Entity Framework’s contexts are meant to be used as short-lived instances in order to provide the most optimal performance experience.</span></span> <span data-ttu-id="1c6b4-983">Kontexty očekává krátkodobé žít a zahodí a proto je implementovaná odlehčení a reutilize metadat, kdykoli je to možné.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-983">Contexts are expected to be short lived and discarded, and as such have been implemented to be very lightweight and reutilize metadata whenever possible.</span></span> <span data-ttu-id="1c6b4-984">Ve web scénářích je potřeba to mějte na paměti a není nutné kontext pro více než jedné žádosti.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-984">In web scenarios it’s important to keep this in mind and not have a context for more than the duration of a single request.</span></span> <span data-ttu-id="1c6b4-985">Podobně v mimo web scénářích kontextu měly být zahozeny podle pochopíte různé úrovně ukládání do mezipaměti v Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-985">Similarly, in non-web scenarios, context should be discarded based on your understanding of the different levels of caching in the Entity Framework.</span></span> <span data-ttu-id="1c6b4-986">Obecně řečeno jeden by měl nepoužívejte instance kontextu po celou dobu životnosti aplikace, stejně jako kontexty na vlákno a statické kontexty.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-986">Generally speaking, one should avoid having a context instance throughout the life of the application, as well as contexts per thread and static contexts.</span></span>

### <a name="94------database-null-semantics"></a><span data-ttu-id="1c6b4-987">9.4 sémantika s hodnotou null databáze</span><span class="sxs-lookup"><span data-stu-id="1c6b4-987">9.4      Database null semantics</span></span>

<span data-ttu-id="1c6b4-988">Entity Framework ve výchozím nastavení vygeneruje kód SQL, který má C\# sémantiku porovnání s hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-988">Entity Framework by default will generate SQL code that has C\# null comparison semantics.</span></span> <span data-ttu-id="1c6b4-989">Vezměte v úvahu následující příklad dotazu:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-989">Consider the following example query:</span></span>

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

<span data-ttu-id="1c6b4-990">V tomto příkladu jsme řadu s možnou hodnotou Null proměnné s možnou hodnotou NULL vlastnosti entity, jako je například KódDodavatele a UnitPrice srovnání.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-990">In this example, we’re comparing a number of nullable variables against nullable properties on the entity, such as SupplierID and UnitPrice.</span></span> <span data-ttu-id="1c6b4-991">Vygenerovaný SQL pro tento dotaz se zeptá, pokud hodnota parametru je stejná jako hodnota sloupce, nebo pokud parametr a hodnoty ve sloupcích mají hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-991">The generated SQL for this query will ask if the parameter value is the same as the column value, or if both the parameter and the column values are null.</span></span> <span data-ttu-id="1c6b4-992">Tím se skrýt tak, jak databázový server zpracovává hodnoty Null a bude poskytovat konzistentní C\# null prostředí mezi dodavateli jinou databázi.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-992">This will hide the way the database server handles nulls and will provide a consistent C\# null experience across different database vendors.</span></span> <span data-ttu-id="1c6b4-993">Na druhé straně generovaný kód je poněkud složitými a nemusí fungovat dobře v případě množství porovnávání v where příkazu dotazu se hodně zvětšuje.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-993">On the other hand, the generated code is a bit convoluted and may not perform well when the amount of comparisons in the where statement of the query grows to a large number.</span></span>

<span data-ttu-id="1c6b4-994">Jedním ze způsobů řešení této situace je pomocí databáze sémantika s hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-994">One way to deal with this situation is by using database null semantics.</span></span> <span data-ttu-id="1c6b4-995">Všimněte si, že to může potenciálně chovat jinak c\# null sémantiku od teď Entity Frameworku vygeneruje jednodušší SQL, který poskytuje způsob, jak databázový stroj zpracovává hodnoty null.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-995">Note that this might potentially behave differently to the C\# null semantics since now Entity Framework will generate simpler SQL that exposes the way the database engine handles null values.</span></span> <span data-ttu-id="1c6b4-996">Sémantika s hodnotou null databáze může být aktivovaná za kontextu s jedním řádkem jediné konfiguraci proti kontextu konfigurace:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-996">Database null semantics can be activated per-context with one single configuration line against the context configuration:</span></span>

``` csharp
                context.Configuration.UseDatabaseNullSemantics = true;
```

<span data-ttu-id="1c6b4-997">Malé a střední velikosti dotazy nezobrazí zlepšení postřehnutelné výkonu při použití databáze sémantika s hodnotou null, ale rozdíl bude snadněji postřehnutelné na dotazy s velkým množstvím možných porovnávání s hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-997">Small to medium sized queries will not display a perceptible performance improvement when using database null semantics, but the difference will become more noticeable on queries with a large number of potential null comparisons.</span></span>

<span data-ttu-id="1c6b4-998">Ve výše uvedené vzorový dotaz byl rozdíly ve výkonnosti méně než 2 % microbenchmark spuštěné v řízeném prostředí.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-998">In the example query above, the performance difference was less than 2% in a microbenchmark running in a controlled environment.</span></span>

### <a name="95------async"></a><span data-ttu-id="1c6b4-999">9.5 asynchronní</span><span class="sxs-lookup"><span data-stu-id="1c6b4-999">9.5      Async</span></span>

<span data-ttu-id="1c6b4-1000">Entity Framework 6 zavedena podpora asynchronní operace při spuštění v rozhraní .NET 4.5 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1000">Entity Framework 6 introduced support of async operations when running on .NET 4.5 or later.</span></span> <span data-ttu-id="1c6b4-1001">Ve většině případů aplikací, které obsahují vstupně-výstupní operace týkající se sporů využívat na maximum pomocí asynchronního dotazu, který se operace uložení.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1001">For the most part, applications that have IO related contention will benefit the most from using asynchronous query and save operations.</span></span> <span data-ttu-id="1c6b4-1002">Pokud vaše aplikace nezpůsobuje žádné kolize vstupně-výstupní operace, použijte asynchronní, v nejlepší případech běžely synchronně a vrátí výsledek ve stejnou dobu jako synchronní volání nebo v nejhorším případě, jednoduše odložit provádění asynchronní úloha a přidat další tim elektronické pro dokončení vašeho scénáře.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1002">If your application does not suffer from IO contention, the use of async will, in the best cases, run synchronously and return the result in the same amount of time as a synchronous call, or in the worst case, simply defer execution to an asynchronous task and add extra time to the completion of your scenario.</span></span>

<span data-ttu-id="1c6b4-1003">Informace o tom, jak asynchronní programovací práce, který vám pomůže rozhodování o tom, pokud asynchronní zlepší výkon vaší aplikace navštívíte [ http://msdn.microsoft.com/library/hh191443.aspx ](https://msdn.microsoft.com/library/hh191443.aspx).</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1003">For information on how asynchronous programming work that will help you deciding if async will improve the performance of your application visit [http://msdn.microsoft.com/library/hh191443.aspx](https://msdn.microsoft.com/library/hh191443.aspx).</span></span> <span data-ttu-id="1c6b4-1004">Další informace týkající se použití asynchronních operací v Entity Framework naleznete v tématu [asynchronního dotazu a uložit](~/ef6/fundamentals/async.md
).</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1004">For more information on the use of async operations on Entity Framework, see [Async Query and Save](~/ef6/fundamentals/async.md
).</span></span>

### <a name="96------ngen"></a><span data-ttu-id="1c6b4-1005">9.6 NGEN</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1005">9.6      NGEN</span></span>

<span data-ttu-id="1c6b4-1006">Entity Framework 6 nepochází ve výchozí instalaci rozhraní .NET framework.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1006">Entity Framework 6 does not come in the default installation of .NET framework.</span></span> <span data-ttu-id="1c6b4-1007">V důsledku toho sestavení rozhraní Entity Framework nejsou že Ngen by ve výchozím nastavení, což znamená, že veškerý kód Entity Framework se řídí stejnou náklady JIT'ing jako jakékoli jiné sestavení jazyka MSIL.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1007">As such, the Entity Framework assemblies are not NGEN’d by default which means that all the Entity Framework code is subject to the same JIT’ing costs as any other MSIL assembly.</span></span> <span data-ttu-id="1c6b4-1008">To může snížit F5 zkušenosti při vývoji a také úplné spuštění vaší aplikace v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1008">This might degrade the F5 experience while developing and also the cold startup of your application in the production environments.</span></span> <span data-ttu-id="1c6b4-1009">Aby bylo možné snížit náklady na využití procesoru a paměti JIT'ing se doporučuje NGEN Entity Framework Image podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1009">In order to reduce the CPU and memory costs of JIT’ing it is advisable to NGEN the Entity Framework images as appropriate.</span></span> <span data-ttu-id="1c6b4-1010">Další informace o tom, jak zlepšit výkon při spuštění nástroje Entity Framework 6 pomocí technologie NGEN najdete v tématu [zlepšuje výkon při spouštění pomocí technologie NGen](~/ef6/fundamentals/performance/ngen.md).</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1010">For more information on how to improve the startup performance of Entity Framework 6 with NGEN, see [Improving Startup Performance with NGen](~/ef6/fundamentals/performance/ngen.md).</span></span>

### <a name="97------code-first-versus-edmx"></a><span data-ttu-id="1c6b4-1011">9.7 kódu nejprve oproti EDMX</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1011">9.7      Code First versus EDMX</span></span>

<span data-ttu-id="1c6b4-1012">Entity Framework důvodů o problému vzniklé vzájemné napětí Neshoda mezi objektově orientované programování a relačními databázemi tím, že reprezentaci v paměti koncepčního modelu (objekty), schéma úložiště (databáze) a mapování mezi dvě.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1012">Entity Framework reasons about the impedance mismatch problem between object oriented programming and relational databases by having an in-memory representation of the conceptual model (the objects), the storage schema (the database) and a mapping between the two.</span></span> <span data-ttu-id="1c6b4-1013">Tato metadata je volána modelu Entity Data Model nebo EDM pro krátké.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1013">This metadata is called an Entity Data Model, or EDM for short.</span></span> <span data-ttu-id="1c6b4-1014">Z tohoto modelu EDM Entity Framework bude odvozovat zobrazení umožňujícím zpětnou transformaci dat z objektů v paměti do databáze a zpět.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1014">From this EDM, Entity Framework will derive the views to roundtrip data from the objects in memory to the database and back.</span></span>

<span data-ttu-id="1c6b4-1015">Pokud Entity Framework je použit společně s souboru EDMX, který formálně určuje konceptuální model, schéma úložiště a mapování, pak fázi načítání modelu se musí ověřit správnost modelu EDM (například, ujistěte se, že nebyly nalezeny žádné mapování), pak Generovat zobrazení, ověření zobrazení a mít tato metadata, která je připravená k použití.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1015">When Entity Framework is used with an EDMX file that formally specifies the conceptual model, the storage schema, and the mapping, then the model loading stage only has to validate that the EDM is correct (for example, make sure that no mappings are missing), then generate the views, then validate the views and have this metadata ready for use.</span></span> <span data-ttu-id="1c6b4-1016">Pouze spustit pak může dotaz nebo nová data uložit do úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1016">Only then can a query be executed or new data be saved to the data store.</span></span>

<span data-ttu-id="1c6b4-1017">Přístupu Code First je svou podstatou sofistikované generátor modelu Entity Data Model.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1017">The Code First approach is, at its heart, a sophisticated Entity Data Model generator.</span></span> <span data-ttu-id="1c6b4-1018">Entity Framework je pro vytvoření modelu EDM z poskytnutého kódu; dělá to tak analýza zahrnutých v modelu s použitím konvence a konfigurace modelu přes rozhraní Fluent API třídy.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1018">The Entity Framework has to produce an EDM from the provided code; it does so by analyzing the classes involved in the model, applying conventions and configuring the model via the Fluent API.</span></span> <span data-ttu-id="1c6b4-1019">Po sestavení modelu EDM Entity Framework v podstatě se chová stejně jako by měl soubor EDMX bylo k dispozici v projektu.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1019">After the EDM is built, the Entity Framework essentially behaves the same way as it would had an EDMX file been present in the project.</span></span> <span data-ttu-id="1c6b4-1020">Díky tomu se sestavení modelu z Code First přidá další složitosti, který překládá do pomalejší čas spuštění pro Entity Framework ve srovnání s tím, že EDMX.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1020">Thus, building the model from Code First adds extra complexity that translates into a slower startup time for the Entity Framework when compared to having an EDMX.</span></span> <span data-ttu-id="1c6b4-1021">Náklady jsou zcela závisí na velikosti a složitosti modelu, který má být sestaven.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1021">The cost is completely dependent on the size and complexity of the model that’s being built.</span></span>

<span data-ttu-id="1c6b4-1022">Pokud se rozhodnete použít EDMX a Code First, je důležité vědět, že pružnosti ji Code First zvyšuje náklady na sestavení modelu poprvé.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1022">When choosing to use EDMX versus Code First, it’s important to know that the flexibility introduced by Code First increases the cost of building the model for the first time.</span></span> <span data-ttu-id="1c6b4-1023">Pokud vaše aplikace dokázal náklady na toto první zatížení obvykle Code First budou preferovaný způsob, jak přejít.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1023">If your application can withstand the cost of this first-time load then typically Code First will be the preferred way to go.</span></span>

## <a name="10-investigating-performance"></a><span data-ttu-id="1c6b4-1024">10 zkoumání výkonu</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1024">10 Investigating Performance</span></span>

### <a name="101-using-the-visual-studio-profiler"></a><span data-ttu-id="1c6b4-1025">10.1 pomocí Profiler sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1025">10.1 Using the Visual Studio Profiler</span></span>

<span data-ttu-id="1c6b4-1026">Pokud máte problémy s výkonem s použitím rozhraní Entity Framework, můžete zobrazit, kde aplikace spotřebuje své doby profiler stejný, jako je integrované do sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1026">If you are having performance issues with the Entity Framework, you can use a profiler like the one built into Visual Studio to see where your application is spending its time.</span></span> <span data-ttu-id="1c6b4-1027">Toto je nástroj, který jsme použili k vygenerování výsečové grafy v blogovém příspěvku "Zkoumání výkonu technologie ADO.NET Entity Framework – část 1" ( \< http://blogs.msdn.com/b/adonet/archive/2008/02/04/exploring-the-performance-of-the-ado-net-entity-framework-part-1.aspx>) , které uvádí, kde Entity Framework stráví času během studené a horké dotazy.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1027">This is the tool we used to generate the pie charts in the “Exploring the Performance of the ADO.NET Entity Framework - Part 1” blog post ( \<http://blogs.msdn.com/b/adonet/archive/2008/02/04/exploring-the-performance-of-the-ado-net-entity-framework-part-1.aspx>) that show where Entity Framework spends its time during cold and warm queries.</span></span>

<span data-ttu-id="1c6b4-1028">Blogový příspěvek "Profilace Entity Framework pomocí Visual Studio 2010 Profiler" napsal Data a modelování zákaznického poradního týmu ukazuje příklad reálného světa jak používají profiler k prozkoumat problémy s výkonem.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1028">The "Profiling Entity Framework using the Visual Studio 2010 Profiler" blog post written by the Data and Modeling Customer Advisory Team shows a real-world example of how they used the profiler to investigate a performance problem.</span></span>  <span data-ttu-id="1c6b4-1029">\<http://blogs.msdn.com/b/dmcat/archive/2010/04/30/profiling-entity-framework-using-the-visual-studio-2010-profiler.aspx>.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1029">\<http://blogs.msdn.com/b/dmcat/archive/2010/04/30/profiling-entity-framework-using-the-visual-studio-2010-profiler.aspx>.</span></span> <span data-ttu-id="1c6b4-1030">Tento příspěvek napsaný pro aplikace systému windows.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1030">This post was written for a windows application.</span></span> <span data-ttu-id="1c6b4-1031">Pokud chcete profilovat webové aplikace mohou nástroje požadavku webové Windows Performance Recorder (části) a Windows Performance Analyzer (WPA) fungují lépe než pracovní ze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1031">If you need to profile a web application the Windows Performance Recorder (WPR) and Windows Performance Analyzer (WPA) tools may work better than working from Visual Studio.</span></span> <span data-ttu-id="1c6b4-1032">Požadavku webové části a WPA jsou součástí Windows Performance Toolkit, který je součástí sady Windows Assessment and Deployment Kit ( [ http://www.microsoft.com/en-US/download/details.aspx?id=39982 ](https://www.microsoft.com/en-US/download/details.aspx?id=39982)).</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1032">WPR and WPA are part of the Windows Performance Toolkit which is included with the Windows Assessment and Deployment Kit ( [http://www.microsoft.com/en-US/download/details.aspx?id=39982](https://www.microsoft.com/en-US/download/details.aspx?id=39982)).</span></span>

### <a name="102-applicationdatabase-profiling"></a><span data-ttu-id="1c6b4-1033">10.2 profilace aplikace a databáze</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1033">10.2 Application/Database profiling</span></span>

<span data-ttu-id="1c6b4-1034">Nástroje, jako je integrované do sady Visual Studio profiler zjistit, kde aplikace spotřebuje čas.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1034">Tools like the profiler built into Visual Studio tell you where your application is spending time.</span></span>  <span data-ttu-id="1c6b4-1035">Je k dispozici jiný typ profileru, který provádí dynamické analýze aplikace běžící v produkčním prostředí nebo předprodukčním prostředí, v závislosti na požadavcích a hledá běžné nástrahy a antimodely přístup k databázi.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1035">Another type of profiler is available that performs dynamic analysis of your running application, either in production or pre-production depending on needs, and looks for common pitfalls and anti-patterns of database access.</span></span>

<span data-ttu-id="1c6b4-1036">Jsou dva komerčně dostupný profilery Profiler Entity Framework ( \< http://efprof.com>) a ORMProfiler ( \< http://ormprofiler.com>).</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1036">Two commercially available profilers are the Entity Framework Profiler ( \<http://efprof.com>) and ORMProfiler ( \<http://ormprofiler.com>).</span></span>

<span data-ttu-id="1c6b4-1037">Pokud vaše aplikace je aplikace MVC pomocí Code First, můžete použít MiniProfiler od StackExchange.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1037">If your application is an MVC application using Code First, you can use StackExchange's MiniProfiler.</span></span> <span data-ttu-id="1c6b4-1038">Scott Hanselman popisuje tento nástroj na svém blogu na: \< http://www.hanselman.com/blog/NuGetPackageOfTheWeek9ASPNETMiniProfilerFromStackExchangeRocksYourWorld.aspx>.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1038">Scott Hanselman describes this tool in his blog at: \<http://www.hanselman.com/blog/NuGetPackageOfTheWeek9ASPNETMiniProfilerFromStackExchangeRocksYourWorld.aspx>.</span></span>

<span data-ttu-id="1c6b4-1039">Další informace o aktivitě databáze dané aplikace, najdete v článku MSDN Magazine Julie Lerman s názvem profilace [profilace aktivity databáze v Entity Framework](https://msdn.microsoft.com/magazine/gg490349.aspx).</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1039">For more information on profiling your application's database activity, see Julie Lerman's MSDN Magazine article titled [Profiling Database Activity in the Entity Framework](https://msdn.microsoft.com/magazine/gg490349.aspx).</span></span>

### <a name="103-database-logger"></a><span data-ttu-id="1c6b4-1040">10.3 protokolovací nástroj databáze</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1040">10.3 Database logger</span></span>

<span data-ttu-id="1c6b4-1041">Pokud používáte Entity Framework 6 také zvážit použití vestavěné protokolování.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1041">If you are using Entity Framework 6 also consider using the built-in logging functionality.</span></span> <span data-ttu-id="1c6b4-1042">Protokolování jeho činnosti prostřednictvím jednoduché konfigurace jednořádkové může být nastavena vlastnost databáze kontextu:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1042">The Database property of the context can be instructed to log its activity via a simple one-line configuration:</span></span>

``` csharp
    using (var context = newQueryComparison.DbC.NorthwindEntities())
    {
        context.Database.Log = Console.WriteLine;
        var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
        q.ToList();
    }
```

<span data-ttu-id="1c6b4-1043">V tomto příkladu se budou protokolovat činnosti databáze do konzoly, ale vlastnost Log lze nastavit volat žádnou akci&lt;řetězec&gt; delegovat.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1043">In this example the database activity will be logged to the console, but the Log property can be configured to call any Action&lt;string&gt; delegate.</span></span>

<span data-ttu-id="1c6b4-1044">Pokud chcete povolit protokolování do databáze bez opětovné kompilace a vy používáte Entity Framework 6.1 nebo novější, můžete tak učinit tak, že přidáte zachycování v souboru web.config nebo app.config aplikace.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1044">If you want to enable database logging without recompiling, and you are using Entity Framework 6.1 or later, you can do so by adding an interceptor in the web.config or app.config file of your application.</span></span>

``` xml
  <interceptors>
    <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
      <parameters>
        <parameter value="C:\Path\To\My\LogOutput.txt"/>
      </parameters>
    </interceptor>
  </interceptors>
```

<span data-ttu-id="1c6b4-1045">Další informace o tom, jak přidat protokolování bez opětovné kompilace přejít na \< http://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/>.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1045">For more information on how to add logging without recompiling go to \<http://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/>.</span></span>

## <a name="11-appendix"></a><span data-ttu-id="1c6b4-1046">Dodatek 11</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1046">11 Appendix</span></span>

### <a name="111-a-test-environment"></a><span data-ttu-id="1c6b4-1047">11.1 A. testovacího prostředí</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1047">11.1 A. Test Environment</span></span>

<span data-ttu-id="1c6b4-1048">Toto prostředí používá počítač 2. nastavení se databáze na samostatném počítači z klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1048">This environment uses a 2-machine setup with the database on a separate machine from the client application.</span></span> <span data-ttu-id="1c6b4-1049">Počítače jsou ve stejné stojanu, tak je latence sítě, relativně nízký, ale než jeden počítač prostředí víc odpovídají realitě.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1049">Machines are in the same rack, so network latency is relatively low, but more realistic than a single-machine environment.</span></span>

#### <a name="1111-------app-server"></a><span data-ttu-id="1c6b4-1050">11.1.1 aplikačního serveru</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1050">11.1.1       App Server</span></span>

##### <a name="11111------software-environment"></a><span data-ttu-id="1c6b4-1051">11.1.1.1 softwarovém prostředí</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1051">11.1.1.1      Software Environment</span></span>

-   <span data-ttu-id="1c6b4-1052">Entity Framework 4 softwarovém prostředí</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1052">Entity Framework 4 Software Environment</span></span>
    -   <span data-ttu-id="1c6b4-1053">Název operačního systému: Windows Server 2008 R2 Enterprise SP1.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1053">OS Name: Windows Server 2008 R2 Enterprise SP1.</span></span>
    -   <span data-ttu-id="1c6b4-1054">Visual Studio 2010 – Ultimate.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1054">Visual Studio 2010 – Ultimate.</span></span>
    -   <span data-ttu-id="1c6b4-1055">Visual Studio 2010 SP1 (pouze pro několik porovnání).</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1055">Visual Studio 2010 SP1 (only for some comparisons).</span></span>
-   <span data-ttu-id="1c6b4-1056">Entity Framework 5 a 6 softwarovém prostředí</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1056">Entity Framework 5 and 6 Software Environment</span></span>
    -   <span data-ttu-id="1c6b4-1057">Název operačního systému: Windows 8.1 Enterprise</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1057">OS Name: Windows 8.1 Enterprise</span></span>
    -   <span data-ttu-id="1c6b4-1058">Visual Studio 2013 – Ultimate.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1058">Visual Studio 2013 – Ultimate.</span></span>

##### <a name="11112------hardware-environment"></a><span data-ttu-id="1c6b4-1059">11.1.1.2 hardwarového prostředí</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1059">11.1.1.2      Hardware Environment</span></span>

-   <span data-ttu-id="1c6b4-1060">Dvoujádrový procesor: Intel(R) Xeon(R) CPU L5520 W3530 @ 2,27 GHz,. 2261 Mhz8 GHz, 4 jader, 84 logických procesorů.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1060">Dual Processor:     Intel(R) Xeon(R) CPU L5520 W3530 @ 2.27GHz, 2261 Mhz8 GHz, 4 Core(s), 84 Logical Processor(s).</span></span>
-   <span data-ttu-id="1c6b4-1061">RamRAM 2412 GB.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1061">2412 GB RamRAM.</span></span>
-   <span data-ttu-id="1c6b4-1062">136 GB SCSI250GB SATA 7200 ot. / min / 3GB/s disku rozdělit do 4 oddíly.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1062">136 GB SCSI250GB SATA 7200 rpm 3GB/s drive split into 4 partitions.</span></span>

#### <a name="1112-------db-server"></a><span data-ttu-id="1c6b4-1063">11.1.2 Databázového serveru</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1063">11.1.2       DB server</span></span>

##### <a name="11121------software-environment"></a><span data-ttu-id="1c6b4-1064">11.1.2.1 softwarovém prostředí</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1064">11.1.2.1      Software Environment</span></span>

-   <span data-ttu-id="1c6b4-1065">Název operačního systému: Windows Server 2008 R28.1 Enterprise s aktualizací SP1.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1065">OS Name: Windows Server 2008 R28.1 Enterprise SP1.</span></span>
-   <span data-ttu-id="1c6b4-1066">SQL Server 2008 R22012.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1066">SQL Server 2008 R22012.</span></span>

##### <a name="11122------hardware-environment"></a><span data-ttu-id="1c6b4-1067">11.1.2.2 hardwarového prostředí</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1067">11.1.2.2      Hardware Environment</span></span>

-   <span data-ttu-id="1c6b4-1068">Jeden procesor: Intel(R) Xeon(R) CPU L5520 @ 2,27 GHz,. 2261 MhzES-1620 0 @ 3.60 GHz, 4 jader, 8 logických procesorů.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1068">Single Processor: Intel(R) Xeon(R) CPU L5520  @ 2.27GHz, 2261 MhzES-1620 0 @ 3.60GHz, 4 Core(s), 8 Logical Processor(s).</span></span>
-   <span data-ttu-id="1c6b4-1069">RamRAM 824 GB.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1069">824 GB RamRAM.</span></span>
-   <span data-ttu-id="1c6b4-1070">465 GB ATA500GB SATA 7200 ot. / min 6GB/s disku rozdělit do 4 oddíly.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1070">465 GB ATA500GB SATA 7200 rpm 6GB/s drive split into 4 partitions.</span></span>

### <a name="112------b-query-performance-comparison-tests"></a><span data-ttu-id="1c6b4-1071">11.2 testy porovnání výkonu dotazů B.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1071">11.2      B. Query performance comparison tests</span></span>

<span data-ttu-id="1c6b4-1072">Northwind model byl použit ke spuštění těchto testů.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1072">The Northwind model was used to execute these tests.</span></span> <span data-ttu-id="1c6b4-1073">Byla vygenerována z databáze pomocí Entity Framework designer.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1073">It was generated from the database using the Entity Framework designer.</span></span> <span data-ttu-id="1c6b4-1074">Následující kód se použije k porovnání výkonu provádění dotazu:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1074">Then, the following code was used to compare the performance of the query execution options:</span></span>

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

### <a name="113-c-navision-model"></a><span data-ttu-id="1c6b4-1075">Model Navision 11,3 C.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1075">11.3 C. Navision Model</span></span>

<span data-ttu-id="1c6b4-1076">Navision databáze je velké databáze, použít pro ukázkové aplikace Microsoft Dynamics – NAV.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1076">The Navision database is a large database used to demo Microsoft Dynamics – NAV.</span></span> <span data-ttu-id="1c6b4-1077">Vygenerovaný konceptuální model obsahuje sady 1005 entit a sad 4227 přidružení.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1077">The generated conceptual model contains 1005 entity sets and 4227 association sets.</span></span> <span data-ttu-id="1c6b4-1078">Model použitý v testu je "ploché" – žádné dědičnosti se přidala do něj.</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1078">The model used in the test is “flat” – no inheritance has been added to it.</span></span>

#### <a name="1131-queries-used-for-navision-tests"></a><span data-ttu-id="1c6b4-1079">11.3.1 dotazy použité pro Navision testy</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1079">11.3.1 Queries used for Navision tests</span></span>

<span data-ttu-id="1c6b4-1080">Seznam dotazů použít s modelem Navision obsahuje 3 kategorie jazyka Entity SQL:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1080">The queries list used with the Navision model contains 3 categories of Entity SQL queries:</span></span>

##### <a name="11311-lookup"></a><span data-ttu-id="1c6b4-1081">11.3.1.1 vyhledávání</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1081">11.3.1.1 Lookup</span></span>

<span data-ttu-id="1c6b4-1082">Jednoduché vyhledávací dotaz s žádné agregace</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1082">A simple lookup query with no aggregations</span></span>

-   <span data-ttu-id="1c6b4-1083">Počet: 16232</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1083">Count: 16232</span></span>
-   <span data-ttu-id="1c6b4-1084">Příklad:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1084">Example:</span></span>

``` xml
  <Query complexity="Lookup">
    <CommandText>Select value distinct top(4) e.Idle_Time From NavisionFKContext.Session as e</CommandText>
  </Query>
```

##### <a name="11312-singleaggregating"></a><span data-ttu-id="1c6b4-1085">11.3.1.2 SingleAggregating</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1085">11.3.1.2 SingleAggregating</span></span>

<span data-ttu-id="1c6b4-1086">Normální BI dotaz s více agregací, ale žádné mezisoučty (jeden dotaz)</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1086">A normal BI query with multiple aggregations, but no subtotals (single query)</span></span>

-   <span data-ttu-id="1c6b4-1087">Počet: 2313</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1087">Count: 2313</span></span>
-   <span data-ttu-id="1c6b4-1088">Příklad:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1088">Example:</span></span>

``` xml
  <Query complexity="SingleAggregating">
    <CommandText>NavisionFK.MDF_SessionLogin_Time_Max()</CommandText>
  </Query>
```

<span data-ttu-id="1c6b4-1089">Kde MDF\_SessionLogin\_čas\_Max() je definován v modelu jako:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1089">Where MDF\_SessionLogin\_Time\_Max() is defined in the model as:</span></span>

``` xml
  <Function Name="MDF_SessionLogin_Time_Max" ReturnType="Collection(DateTime)">
    <DefiningExpression>SELECT VALUE Edm.Min(E.Login_Time) FROM NavisionFKContext.Session as E</DefiningExpression>
  </Function>
```

##### <a name="11313-aggregatingsubtotals"></a><span data-ttu-id="1c6b4-1090">11.3.1.3 AggregatingSubtotals</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1090">11.3.1.3 AggregatingSubtotals</span></span>

<span data-ttu-id="1c6b4-1091">Dotaz BI s agregace a souhrny (prostřednictvím sjednocení všechny)</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1091">A BI query with aggregations and subtotals (via union all)</span></span>

-   <span data-ttu-id="1c6b4-1092">Počet: 178</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1092">Count: 178</span></span>
-   <span data-ttu-id="1c6b4-1093">Příklad:</span><span class="sxs-lookup"><span data-stu-id="1c6b4-1093">Example:</span></span>

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
