---
title: Co je nového v EF Core 5,0
author: ajcvickers
ms.date: 01/29/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: 65d7bd43e8a00c77fd6091a74c677635710d03e3
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417964"
---
# <a name="whats-new-in-ef-core-50"></a><span data-ttu-id="2cfc2-102">Co je nového v EF Core 5,0</span><span class="sxs-lookup"><span data-stu-id="2cfc2-102">What's New in EF Core 5.0</span></span>

<span data-ttu-id="2cfc2-103">EF Core 5,0 je aktuálně ve vývoji.</span><span class="sxs-lookup"><span data-stu-id="2cfc2-103">EF Core 5.0 is currently in development.</span></span>
<span data-ttu-id="2cfc2-104">Tato stránka bude obsahovat přehled zajímavých změn představených v každé verzi Preview.</span><span class="sxs-lookup"><span data-stu-id="2cfc2-104">This page will contain an overview of interesting changes introduced in each preview.</span></span>
<span data-ttu-id="2cfc2-105">První verze Preview EF Core 5,0 se nezávazně očekává v prvním čtvrtletí 2020.</span><span class="sxs-lookup"><span data-stu-id="2cfc2-105">The first preview of EF Core 5.0 is tentatively expected in in the first quarter of 2020.</span></span>

<span data-ttu-id="2cfc2-106">Tato stránka neduplikuje [plán pro EF Core 5,0](plan.md).</span><span class="sxs-lookup"><span data-stu-id="2cfc2-106">This page does not duplicate the [plan for EF Core 5.0](plan.md).</span></span>
<span data-ttu-id="2cfc2-107">Plán popisuje celkový motiv EF Core 5,0, včetně všeho, co plánujeme zahrnout, před odesláním finální verze.</span><span class="sxs-lookup"><span data-stu-id="2cfc2-107">The plan describes the overall themes for EF Core 5.0, including everything we are planning to include before shipping the final release.</span></span>

<span data-ttu-id="2cfc2-108">Odkazy odsud budeme přidávat do oficiální dokumentace, která je publikovaná.</span><span class="sxs-lookup"><span data-stu-id="2cfc2-108">We will add links from here to the official documentation as it is published.</span></span>

## <a name="preview-1-not-yet-shipped"></a><span data-ttu-id="2cfc2-109">Preview 1 (ještě Neexpedováno)</span><span class="sxs-lookup"><span data-stu-id="2cfc2-109">Preview 1 (Not yet shipped)</span></span>

### <a name="simple-logging"></a><span data-ttu-id="2cfc2-110">Jednoduché protokolování</span><span class="sxs-lookup"><span data-stu-id="2cfc2-110">Simple logging</span></span>

<span data-ttu-id="2cfc2-111">Tato funkce přidá funkce podobné `Database.Log` v EF6.</span><span class="sxs-lookup"><span data-stu-id="2cfc2-111">This feature adds functionality similar to `Database.Log` in EF6.</span></span>
<span data-ttu-id="2cfc2-112">To znamená, že poskytuje jednoduchý způsob, jak získat protokoly z EF Core, aniž by bylo nutné konfigurovat žádný druh externího protokolovacího rozhraní.</span><span class="sxs-lookup"><span data-stu-id="2cfc2-112">That is, it provides a simple way to get logs from EF Core without the need to configure any kind of external logging framework.</span></span>

<span data-ttu-id="2cfc2-113">Předběžná dokumentace je součástí [týdenního stavu EF pro 5. prosince 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="2cfc2-113">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="2cfc2-114">Další dokumentaci sleduje problém [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).</span><span class="sxs-lookup"><span data-stu-id="2cfc2-114">Additional documentation is tracked by issue [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).</span></span>

### <a name="simple-way-to-get-generated-sql"></a><span data-ttu-id="2cfc2-115">Jednoduchý způsob, jak získat vygenerovaný SQL</span><span class="sxs-lookup"><span data-stu-id="2cfc2-115">Simple way to get generated SQL</span></span>

<span data-ttu-id="2cfc2-116">EF Core 5,0 zavádí metodu rozšíření `ToQueryString`, která vrátí SQL, který EF Core generuje při provádění dotazu LINQ.</span><span class="sxs-lookup"><span data-stu-id="2cfc2-116">EF Core 5.0 introduces the `ToQueryString` extension method which will return the SQL that EF Core will generate when executing a LINQ query.</span></span>

<span data-ttu-id="2cfc2-117">Do [9. ledna 2020 se do týdenního stavu EF](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246)zahrnuje předběžná dokumentace.</span><span class="sxs-lookup"><span data-stu-id="2cfc2-117">Preliminary documentation is included in the [EF weekly status for January 9, 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span></span>

<span data-ttu-id="2cfc2-118">Další dokumentaci sleduje problém [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).</span><span class="sxs-lookup"><span data-stu-id="2cfc2-118">Additional documentation is tracked by issue [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).</span></span>

### <a name="enhanced-debug-views"></a><span data-ttu-id="2cfc2-119">Rozšířená zobrazení ladění</span><span class="sxs-lookup"><span data-stu-id="2cfc2-119">Enhanced debug views</span></span>

<span data-ttu-id="2cfc2-120">Zobrazení ladění představují snadný způsob, jak se při ladění problémů podívat na vnitřní EF Core.</span><span class="sxs-lookup"><span data-stu-id="2cfc2-120">Debug views are an easy way to look at the internals of EF Core when debugging issues.</span></span>
<span data-ttu-id="2cfc2-121">Zobrazení ladění pro model bylo implementováno před nějakým časem.</span><span class="sxs-lookup"><span data-stu-id="2cfc2-121">A debug view for the Model was implemented some time ago.</span></span>
<span data-ttu-id="2cfc2-122">Pro EF Core 5,0 jsme provedli zobrazení modelu pro čtení a přidání nového zobrazení ladění pro sledované entity ve Správci stavu.</span><span class="sxs-lookup"><span data-stu-id="2cfc2-122">For EF Core 5.0, we have made the model view easier to read and added a new debug view for tracked entities in the state manager.</span></span>

<span data-ttu-id="2cfc2-123">Předběžná dokumentace je zahrnutá ve [stavu týden EF pro 12. prosince 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span><span class="sxs-lookup"><span data-stu-id="2cfc2-123">Preliminary documentation is included in the [EF weekly status for December 12, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span></span>

<span data-ttu-id="2cfc2-124">Další dokumentaci sleduje problém [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).</span><span class="sxs-lookup"><span data-stu-id="2cfc2-124">Additional documentation is tracked by issue [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).</span></span>

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a><span data-ttu-id="2cfc2-125">V inicializovaných DbContext se dá změnit připojení nebo připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="2cfc2-125">Connection or connection string can be changed on initialized DbContext</span></span>

<span data-ttu-id="2cfc2-126">Nyní je snazší vytvořit instanci DbContext bez připojení nebo připojovacího řetězce.</span><span class="sxs-lookup"><span data-stu-id="2cfc2-126">It is now easier to create a DbContext instance without any connection or connection string.</span></span>
<span data-ttu-id="2cfc2-127">Připojení nebo připojovací řetězec se teď dá v instanci kontextu taky připojit.</span><span class="sxs-lookup"><span data-stu-id="2cfc2-127">Also, the connection or connection string can now be mutated on the context instance.</span></span>
<span data-ttu-id="2cfc2-128">To umožňuje stejné instanci kontextu dynamicky se připojovat k různým databázím.</span><span class="sxs-lookup"><span data-stu-id="2cfc2-128">This allows the same context instance to dynamically connect to different databases.</span></span>

<span data-ttu-id="2cfc2-129">Dokumentace je sledována pomocí [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075)problému.</span><span class="sxs-lookup"><span data-stu-id="2cfc2-129">Documentation is tracked by issue [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).</span></span>

### <a name="change-tracking-proxies"></a><span data-ttu-id="2cfc2-130">Proxy se sledováním změn</span><span class="sxs-lookup"><span data-stu-id="2cfc2-130">Change-tracking proxies</span></span>

<span data-ttu-id="2cfc2-131">EF Core teď můžou generovat proxy a moduly runtime, které automaticky implementují [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) a [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span><span class="sxs-lookup"><span data-stu-id="2cfc2-131">EF Core can now generate runtime proxies that automatically implement [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) and [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span></span>
<span data-ttu-id="2cfc2-132">Ty pak nahlásí změny hodnot u vlastností entity přímo na EF Core a nemusíte přitom Hledat změny.</span><span class="sxs-lookup"><span data-stu-id="2cfc2-132">These then report value changes on entity properties directly to EF Core, avoiding the need to scan for changes.</span></span>
<span data-ttu-id="2cfc2-133">Nicméně proxy se dodávají s vlastní sadou omezení, takže nejsou pro všechny.</span><span class="sxs-lookup"><span data-stu-id="2cfc2-133">However, proxies come with their own set of limitations, so they are not for everyone.</span></span>

<span data-ttu-id="2cfc2-134">Dokumentace je sledována pomocí [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076)problému.</span><span class="sxs-lookup"><span data-stu-id="2cfc2-134">Documentation is tracked by issue [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).</span></span>

### <a name="improved-handling-of-database-null-semantics"></a><span data-ttu-id="2cfc2-135">Vylepšené zpracování sémantiky s hodnotou null databáze</span><span class="sxs-lookup"><span data-stu-id="2cfc2-135">Improved handling of database null semantics</span></span>

<span data-ttu-id="2cfc2-136">Relační databáze obvykle považují hodnotu NULL za neznámou hodnotu, takže se nerovná žádnému jiné hodnotě NULL.</span><span class="sxs-lookup"><span data-stu-id="2cfc2-136">Relational databases typically treat NULL as an unknown value and therefore not equal to any other NULL.</span></span>
<span data-ttu-id="2cfc2-137">C#na druhé straně zachází s hodnotou null jako s definovanou hodnotou, která je porovnána s jinou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="2cfc2-137">C#, on the other hand, treats null as a defined value which compares equal to any other null.</span></span>
<span data-ttu-id="2cfc2-138">EF Core ve výchozím nastavení překládá dotazy tak, aby používaly C# sémantiku null.</span><span class="sxs-lookup"><span data-stu-id="2cfc2-138">EF Core by default translates queries so that they use C# null semantics.</span></span>
<span data-ttu-id="2cfc2-139">EF Core 5,0 významně vylepšuje efektivitu těchto překladů.</span><span class="sxs-lookup"><span data-stu-id="2cfc2-139">EF Core 5.0 greatly improves the efficiency of these translations.</span></span>

<span data-ttu-id="2cfc2-140">Dokumentace je sledována pomocí [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612)problému.</span><span class="sxs-lookup"><span data-stu-id="2cfc2-140">Documentation is tracked by issue [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).</span></span>

### <a name="indexer-properties"></a><span data-ttu-id="2cfc2-141">Vlastnosti indexeru</span><span class="sxs-lookup"><span data-stu-id="2cfc2-141">Indexer properties</span></span>

<span data-ttu-id="2cfc2-142">EF Core 5,0 podporuje mapování vlastností C# indexeru.</span><span class="sxs-lookup"><span data-stu-id="2cfc2-142">EF Core 5.0 supports mapping of C# indexer properties.</span></span>
<span data-ttu-id="2cfc2-143">To umožňuje entitám fungovat jako vaky objektů a dat, kde jsou sloupce namapovány na pojmenované vlastnosti v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="2cfc2-143">This allows entities to act as property bags where columns are mapped to named properties in the bag.</span></span>

<span data-ttu-id="2cfc2-144">Dokumentace je sledována pomocí [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018)problému.</span><span class="sxs-lookup"><span data-stu-id="2cfc2-144">Documentation is tracked by issue [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).</span></span>

### <a name="generation-of-check-constraints-for-enum-mappings"></a><span data-ttu-id="2cfc2-145">Generování omezení CHECK pro mapování výčtu</span><span class="sxs-lookup"><span data-stu-id="2cfc2-145">Generation of check constraints for enum mappings</span></span>

<span data-ttu-id="2cfc2-146">Migrace EF Core 5,0 teď můžou generovat kontrolní omezení pro mapování vlastností výčtu.</span><span class="sxs-lookup"><span data-stu-id="2cfc2-146">EF Core 5.0 Migrations can now generate CHECK constraints for enum property mappings.</span></span>
<span data-ttu-id="2cfc2-147">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2cfc2-147">For example:</span></span>

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN('Useful', 'Useless', 'Unknown'))
```

<span data-ttu-id="2cfc2-148">Dokumentace je sledována pomocí [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082)problému.</span><span class="sxs-lookup"><span data-stu-id="2cfc2-148">Documentation is tracked by issue [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).</span></span>

### <a name="query-translations-for-more-datetime-constructs"></a><span data-ttu-id="2cfc2-149">Překlady dotazů pro další konstrukce DateTime</span><span class="sxs-lookup"><span data-stu-id="2cfc2-149">Query translations for more DateTime constructs</span></span>

<span data-ttu-id="2cfc2-150">Dotazy obsahující nové konstrukce DataTime jsou nyní přeloženy.</span><span class="sxs-lookup"><span data-stu-id="2cfc2-150">Queries containing new DataTime construction are now translated.</span></span>
<span data-ttu-id="2cfc2-151">Také SQL Server funkce DateDiffWeek je nyní namapována.</span><span class="sxs-lookup"><span data-stu-id="2cfc2-151">Also, the SQL Server function DateDiffWeek is now mapped.</span></span>

<span data-ttu-id="2cfc2-152">Dokumentace je sledována pomocí [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)problému.</span><span class="sxs-lookup"><span data-stu-id="2cfc2-152">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translations-for-more-byte-array-constructs"></a><span data-ttu-id="2cfc2-153">Překlady dotazů pro další konstrukce bajtových polí</span><span class="sxs-lookup"><span data-stu-id="2cfc2-153">Query translations for more byte array constructs</span></span>

<span data-ttu-id="2cfc2-154">Dotazy používající vlastnosti Contains, Length, SequenceEqual atd. on Byte [] jsou nyní přeloženy na SQL.</span><span class="sxs-lookup"><span data-stu-id="2cfc2-154">Queries using Contains, Length, SequenceEqual, etc. on byte[] properties are now translated to SQL.</span></span>

<span data-ttu-id="2cfc2-155">Předběžná dokumentace je součástí [týdenního stavu EF pro 5. prosince 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="2cfc2-155">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="2cfc2-156">Další dokumentaci sleduje problém [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="2cfc2-156">Additional documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-reverse"></a><span data-ttu-id="2cfc2-157">Přesměrovat dotaz na zpětný překlad</span><span class="sxs-lookup"><span data-stu-id="2cfc2-157">Query translation for Reverse</span></span>

<span data-ttu-id="2cfc2-158">Dotazy používající `Reverse` jsou nyní přeloženy.</span><span class="sxs-lookup"><span data-stu-id="2cfc2-158">Queries using `Reverse` are now translated.</span></span>
<span data-ttu-id="2cfc2-159">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2cfc2-159">For example:</span></span>

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

<span data-ttu-id="2cfc2-160">Dokumentace je sledována pomocí [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)problému.</span><span class="sxs-lookup"><span data-stu-id="2cfc2-160">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-bitwise-operators"></a><span data-ttu-id="2cfc2-161">Převod dotazů pro bitové operátory</span><span class="sxs-lookup"><span data-stu-id="2cfc2-161">Query translation for bitwise operators</span></span>

<span data-ttu-id="2cfc2-162">Dotazy používající bitové operátory jsou nyní přeloženy ve více případech například:</span><span class="sxs-lookup"><span data-stu-id="2cfc2-162">Queries using bitwise operators are now translated in more cases For example:</span></span>

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

<span data-ttu-id="2cfc2-163">Dokumentace je sledována pomocí [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)problému.</span><span class="sxs-lookup"><span data-stu-id="2cfc2-163">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-strings-on-cosmos"></a><span data-ttu-id="2cfc2-164">Překlad dotazů pro řetězce v Cosmos</span><span class="sxs-lookup"><span data-stu-id="2cfc2-164">Query translation for strings on Cosmos</span></span>

<span data-ttu-id="2cfc2-165">Dotazy, které používají metody řetězce Contains, StartsWith a EndsWith, jsou nyní přeloženy při použití poskytovatele Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2cfc2-165">Queries that use the string methods Contains, StartsWith, and EndsWith are now translated when using the Azure Cosmos DB provider.</span></span>

<span data-ttu-id="2cfc2-166">Dokumentace je sledována pomocí [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)problému.</span><span class="sxs-lookup"><span data-stu-id="2cfc2-166">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>
