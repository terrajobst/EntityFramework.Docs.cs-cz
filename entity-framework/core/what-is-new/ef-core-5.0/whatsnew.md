---
title: Co je nového v EF Core 5,0
author: ajcvickers
ms.date: 03/15/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: 08a93555fd76d8a9f6d3011f59d9a34f76d0b22f
ms.sourcegitcommit: c3b8386071d64953ee68788ef9d951144881a6ab
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/24/2020
ms.locfileid: "80136247"
---
# <a name="whats-new-in-ef-core-50"></a><span data-ttu-id="b5525-102">Co je nového v EF Core 5,0</span><span class="sxs-lookup"><span data-stu-id="b5525-102">What's New in EF Core 5.0</span></span>

<span data-ttu-id="b5525-103">EF Core 5,0 je aktuálně ve vývoji.</span><span class="sxs-lookup"><span data-stu-id="b5525-103">EF Core 5.0 is currently in development.</span></span>
<span data-ttu-id="b5525-104">Tato stránka bude obsahovat přehled zajímavých změn představených v každé verzi Preview.</span><span class="sxs-lookup"><span data-stu-id="b5525-104">This page will contain an overview of interesting changes introduced in each preview.</span></span>

<span data-ttu-id="b5525-105">Tato stránka neduplikuje [plán pro EF Core 5,0](plan.md).</span><span class="sxs-lookup"><span data-stu-id="b5525-105">This page does not duplicate the [plan for EF Core 5.0](plan.md).</span></span>
<span data-ttu-id="b5525-106">Plán popisuje celkový motiv EF Core 5,0, včetně všeho, co plánujeme zahrnout, před odesláním finální verze.</span><span class="sxs-lookup"><span data-stu-id="b5525-106">The plan describes the overall themes for EF Core 5.0, including everything we are planning to include before shipping the final release.</span></span>

<span data-ttu-id="b5525-107">Odkazy odsud budeme přidávat do oficiální dokumentace, která je publikovaná.</span><span class="sxs-lookup"><span data-stu-id="b5525-107">We will add links from here to the official documentation as it is published.</span></span>

## <a name="preview-1"></a><span data-ttu-id="b5525-108">Ve verzi Preview 1</span><span class="sxs-lookup"><span data-stu-id="b5525-108">Preview 1</span></span>

### <a name="simple-logging"></a><span data-ttu-id="b5525-109">Jednoduché protokolování</span><span class="sxs-lookup"><span data-stu-id="b5525-109">Simple logging</span></span>

<span data-ttu-id="b5525-110">Tato funkce přidá funkce podobné `Database.Log` v EF6.</span><span class="sxs-lookup"><span data-stu-id="b5525-110">This feature adds functionality similar to `Database.Log` in EF6.</span></span>
<span data-ttu-id="b5525-111">To znamená, že poskytuje jednoduchý způsob, jak získat protokoly z EF Core, aniž by bylo nutné konfigurovat žádný druh externího protokolovacího rozhraní.</span><span class="sxs-lookup"><span data-stu-id="b5525-111">That is, it provides a simple way to get logs from EF Core without the need to configure any kind of external logging framework.</span></span>

<span data-ttu-id="b5525-112">Předběžná dokumentace je součástí [týdenního stavu EF pro 5. prosince 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="b5525-112">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="b5525-113">Další dokumentaci sleduje problém [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).</span><span class="sxs-lookup"><span data-stu-id="b5525-113">Additional documentation is tracked by issue [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).</span></span>

### <a name="simple-way-to-get-generated-sql"></a><span data-ttu-id="b5525-114">Jednoduchý způsob, jak získat vygenerovaný SQL</span><span class="sxs-lookup"><span data-stu-id="b5525-114">Simple way to get generated SQL</span></span>

<span data-ttu-id="b5525-115">EF Core 5,0 zavádí metodu rozšíření `ToQueryString`, která vrátí SQL, který EF Core generuje při provádění dotazu LINQ.</span><span class="sxs-lookup"><span data-stu-id="b5525-115">EF Core 5.0 introduces the `ToQueryString` extension method which will return the SQL that EF Core will generate when executing a LINQ query.</span></span>

<span data-ttu-id="b5525-116">Do [9. ledna 2020 se do týdenního stavu EF](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246)zahrnuje předběžná dokumentace.</span><span class="sxs-lookup"><span data-stu-id="b5525-116">Preliminary documentation is included in the [EF weekly status for January 9, 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span></span>

<span data-ttu-id="b5525-117">Další dokumentaci sleduje problém [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).</span><span class="sxs-lookup"><span data-stu-id="b5525-117">Additional documentation is tracked by issue [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).</span></span>

### <a name="use-a-c-attribute-to-indicate-that-an-entity-has-no-key"></a><span data-ttu-id="b5525-118">Označení, C# že entita nemá žádný klíč, pomocí atributu</span><span class="sxs-lookup"><span data-stu-id="b5525-118">Use a C# attribute to indicate that an entity has no key</span></span>

<span data-ttu-id="b5525-119">Typ entity se teď dá nakonfigurovat tak, aby neobsahoval žádný klíč pomocí nového `KeylessAttribute`.</span><span class="sxs-lookup"><span data-stu-id="b5525-119">An entity type can now be configured as having no key using the new `KeylessAttribute`.</span></span>
<span data-ttu-id="b5525-120">Například:</span><span class="sxs-lookup"><span data-stu-id="b5525-120">For example:</span></span>

```CSharp
[Keyless]
public class Address
{
    public string Street { get; set; }
    public string City { get; set; }
    public int Zip { get; set; }
}
```

<span data-ttu-id="b5525-121">Dokumentace je sledována pomocí [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186)problému.</span><span class="sxs-lookup"><span data-stu-id="b5525-121">Documentation is tracked by issue [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186).</span></span>

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a><span data-ttu-id="b5525-122">V inicializovaných DbContext se dá změnit připojení nebo připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="b5525-122">Connection or connection string can be changed on initialized DbContext</span></span>

<span data-ttu-id="b5525-123">Nyní je snazší vytvořit instanci DbContext bez připojení nebo připojovacího řetězce.</span><span class="sxs-lookup"><span data-stu-id="b5525-123">It is now easier to create a DbContext instance without any connection or connection string.</span></span>
<span data-ttu-id="b5525-124">Připojení nebo připojovací řetězec se teď dá v instanci kontextu taky připojit.</span><span class="sxs-lookup"><span data-stu-id="b5525-124">Also, the connection or connection string can now be mutated on the context instance.</span></span>
<span data-ttu-id="b5525-125">To umožňuje stejné instanci kontextu dynamicky se připojovat k různým databázím.</span><span class="sxs-lookup"><span data-stu-id="b5525-125">This allows the same context instance to dynamically connect to different databases.</span></span>

<span data-ttu-id="b5525-126">Dokumentace je sledována pomocí [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075)problému.</span><span class="sxs-lookup"><span data-stu-id="b5525-126">Documentation is tracked by issue [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).</span></span>

### <a name="change-tracking-proxies"></a><span data-ttu-id="b5525-127">Proxy se sledováním změn</span><span class="sxs-lookup"><span data-stu-id="b5525-127">Change-tracking proxies</span></span>

<span data-ttu-id="b5525-128">EF Core teď můžou generovat proxy a moduly runtime, které automaticky implementují [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) a [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span><span class="sxs-lookup"><span data-stu-id="b5525-128">EF Core can now generate runtime proxies that automatically implement [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) and [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span></span>
<span data-ttu-id="b5525-129">Ty pak nahlásí změny hodnot u vlastností entity přímo na EF Core a nemusíte přitom Hledat změny.</span><span class="sxs-lookup"><span data-stu-id="b5525-129">These then report value changes on entity properties directly to EF Core, avoiding the need to scan for changes.</span></span>
<span data-ttu-id="b5525-130">Nicméně proxy se dodávají s vlastní sadou omezení, takže nejsou pro všechny.</span><span class="sxs-lookup"><span data-stu-id="b5525-130">However, proxies come with their own set of limitations, so they are not for everyone.</span></span>

<span data-ttu-id="b5525-131">Dokumentace je sledována pomocí [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076)problému.</span><span class="sxs-lookup"><span data-stu-id="b5525-131">Documentation is tracked by issue [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).</span></span>

### <a name="enhanced-debug-views"></a><span data-ttu-id="b5525-132">Rozšířená zobrazení ladění</span><span class="sxs-lookup"><span data-stu-id="b5525-132">Enhanced debug views</span></span>

<span data-ttu-id="b5525-133">Zobrazení ladění představují snadný způsob, jak se při ladění problémů podívat na vnitřní EF Core.</span><span class="sxs-lookup"><span data-stu-id="b5525-133">Debug views are an easy way to look at the internals of EF Core when debugging issues.</span></span>
<span data-ttu-id="b5525-134">Zobrazení ladění pro model bylo implementováno před nějakým časem.</span><span class="sxs-lookup"><span data-stu-id="b5525-134">A debug view for the Model was implemented some time ago.</span></span>
<span data-ttu-id="b5525-135">Pro EF Core 5,0 jsme provedli zobrazení modelu pro čtení a přidání nového zobrazení ladění pro sledované entity ve Správci stavu.</span><span class="sxs-lookup"><span data-stu-id="b5525-135">For EF Core 5.0, we have made the model view easier to read and added a new debug view for tracked entities in the state manager.</span></span>

<span data-ttu-id="b5525-136">Předběžná dokumentace je zahrnutá ve [stavu týden EF pro 12. prosince 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span><span class="sxs-lookup"><span data-stu-id="b5525-136">Preliminary documentation is included in the [EF weekly status for December 12, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span></span>

<span data-ttu-id="b5525-137">Další dokumentaci sleduje problém [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).</span><span class="sxs-lookup"><span data-stu-id="b5525-137">Additional documentation is tracked by issue [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).</span></span>

### <a name="improved-handling-of-database-null-semantics"></a><span data-ttu-id="b5525-138">Vylepšené zpracování sémantiky s hodnotou null databáze</span><span class="sxs-lookup"><span data-stu-id="b5525-138">Improved handling of database null semantics</span></span>

<span data-ttu-id="b5525-139">Relační databáze obvykle považují hodnotu NULL za neznámou hodnotu, takže se nerovná žádnému jiné hodnotě NULL.</span><span class="sxs-lookup"><span data-stu-id="b5525-139">Relational databases typically treat NULL as an unknown value and therefore not equal to any other NULL.</span></span>
<span data-ttu-id="b5525-140">C#na druhé straně zachází s hodnotou null jako s definovanou hodnotou, která je porovnána s jinou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="b5525-140">C#, on the other hand, treats null as a defined value which compares equal to any other null.</span></span>
<span data-ttu-id="b5525-141">EF Core ve výchozím nastavení překládá dotazy tak, aby používaly C# sémantiku null.</span><span class="sxs-lookup"><span data-stu-id="b5525-141">EF Core by default translates queries so that they use C# null semantics.</span></span>
<span data-ttu-id="b5525-142">EF Core 5,0 významně vylepšuje efektivitu těchto překladů.</span><span class="sxs-lookup"><span data-stu-id="b5525-142">EF Core 5.0 greatly improves the efficiency of these translations.</span></span>

<span data-ttu-id="b5525-143">Dokumentace je sledována pomocí [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612)problému.</span><span class="sxs-lookup"><span data-stu-id="b5525-143">Documentation is tracked by issue [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).</span></span>

### <a name="indexer-properties"></a><span data-ttu-id="b5525-144">Vlastnosti indexeru</span><span class="sxs-lookup"><span data-stu-id="b5525-144">Indexer properties</span></span>

<span data-ttu-id="b5525-145">EF Core 5,0 podporuje mapování vlastností C# indexeru.</span><span class="sxs-lookup"><span data-stu-id="b5525-145">EF Core 5.0 supports mapping of C# indexer properties.</span></span>
<span data-ttu-id="b5525-146">To umožňuje entitám fungovat jako vaky objektů a dat, kde jsou sloupce namapovány na pojmenované vlastnosti v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="b5525-146">This allows entities to act as property bags where columns are mapped to named properties in the bag.</span></span>

<span data-ttu-id="b5525-147">Dokumentace je sledována pomocí [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018)problému.</span><span class="sxs-lookup"><span data-stu-id="b5525-147">Documentation is tracked by issue [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).</span></span>

### <a name="generation-of-check-constraints-for-enum-mappings"></a><span data-ttu-id="b5525-148">Generování omezení CHECK pro mapování výčtu</span><span class="sxs-lookup"><span data-stu-id="b5525-148">Generation of check constraints for enum mappings</span></span>

<span data-ttu-id="b5525-149">Migrace EF Core 5,0 teď můžou generovat kontrolní omezení pro mapování vlastností výčtu.</span><span class="sxs-lookup"><span data-stu-id="b5525-149">EF Core 5.0 Migrations can now generate CHECK constraints for enum property mappings.</span></span>
<span data-ttu-id="b5525-150">Například:</span><span class="sxs-lookup"><span data-stu-id="b5525-150">For example:</span></span>

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN('Useful', 'Useless', 'Unknown'))
```

<span data-ttu-id="b5525-151">Dokumentace je sledována pomocí [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082)problému.</span><span class="sxs-lookup"><span data-stu-id="b5525-151">Documentation is tracked by issue [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).</span></span>

### <a name="isrelational"></a><span data-ttu-id="b5525-152">Relace</span><span class="sxs-lookup"><span data-stu-id="b5525-152">IsRelational</span></span>

<span data-ttu-id="b5525-153">Kromě stávajících `IsSqlServer`, `IsSqlite`a `IsInMemory`byla přidána nová `IsRelational` metoda.</span><span class="sxs-lookup"><span data-stu-id="b5525-153">A new `IsRelational` method has been added in addition to the existing `IsSqlServer`, `IsSqlite`, and `IsInMemory`.</span></span>
<span data-ttu-id="b5525-154">To se dá použít k otestování, jestli DbContext používá jakéhokoli poskytovatele relačních databází.</span><span class="sxs-lookup"><span data-stu-id="b5525-154">This can be used to test if the DbContext is using any relational database provider.</span></span>
<span data-ttu-id="b5525-155">Například:</span><span class="sxs-lookup"><span data-stu-id="b5525-155">For example:</span></span>

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    if (Database.IsRelational())
    {
        // Do relational-specific model configuration.
    }
}
```

<span data-ttu-id="b5525-156">Dokumentace je sledována pomocí [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185)problému.</span><span class="sxs-lookup"><span data-stu-id="b5525-156">Documentation is tracked by issue [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185).</span></span>

### <a name="cosmos-optimistic-concurrency-with-etags"></a><span data-ttu-id="b5525-157">Cosmos Optimistická souběžnost se značkami ETag</span><span class="sxs-lookup"><span data-stu-id="b5525-157">Cosmos optimistic concurrency with ETags</span></span>

<span data-ttu-id="b5525-158">Zprostředkovatel databáze Azure Cosmos DB nyní podporuje optimistickou souběžnost pomocí značek ETag.</span><span class="sxs-lookup"><span data-stu-id="b5525-158">The Azure Cosmos DB database provider now supports optimistic concurrency using ETags.</span></span>
<span data-ttu-id="b5525-159">Pomocí Tvůrce modelů v OnModelCreating nakonfigurujte ETag:</span><span class="sxs-lookup"><span data-stu-id="b5525-159">Use the model builder in OnModelCreating to configure an ETag:</span></span>

```CSharp
builder.Entity<Customer>().Property(c => c.ETag).IsEtagConcurrency();
```

<span data-ttu-id="b5525-160">SaveChanges pak vygeneruje `DbUpdateConcurrencyException` v konfliktu souběžnosti, který [může být zpracován](https://docs.microsoft.com/ef/core/saving/concurrency) pro implementaci opakování, atd.</span><span class="sxs-lookup"><span data-stu-id="b5525-160">SaveChanges will then throw an `DbUpdateConcurrencyException` on a concurrency conflict, which [can be handled](https://docs.microsoft.com/ef/core/saving/concurrency) to implement retries, etc.</span></span>


<span data-ttu-id="b5525-161">Dokumentace je sledována pomocí [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099)problému.</span><span class="sxs-lookup"><span data-stu-id="b5525-161">Documentation is tracked by issue [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).</span></span>

### <a name="query-translations-for-more-datetime-constructs"></a><span data-ttu-id="b5525-162">Překlady dotazů pro další konstrukce DateTime</span><span class="sxs-lookup"><span data-stu-id="b5525-162">Query translations for more DateTime constructs</span></span>

<span data-ttu-id="b5525-163">Dotazy obsahující nové konstrukce DateTime jsou nyní přeloženy.</span><span class="sxs-lookup"><span data-stu-id="b5525-163">Queries containing new DateTime construction are now translated.</span></span>

<span data-ttu-id="b5525-164">Kromě toho jsou nyní namapovány následující funkce SQL Server:</span><span class="sxs-lookup"><span data-stu-id="b5525-164">In addition, the following SQL Server functions are now mapped:</span></span>
* <span data-ttu-id="b5525-165">DateDiffWeek</span><span class="sxs-lookup"><span data-stu-id="b5525-165">DateDiffWeek</span></span>
* <span data-ttu-id="b5525-166">DateFromParts</span><span class="sxs-lookup"><span data-stu-id="b5525-166">DateFromParts</span></span>

<span data-ttu-id="b5525-167">Například:</span><span class="sxs-lookup"><span data-stu-id="b5525-167">For example:</span></span>

```CSharp
var count = context.Orders.Count(c => date > EF.Functions.DateFromParts(DateTime.Now.Year, 12, 25));

```

<span data-ttu-id="b5525-168">Dokumentace je sledována pomocí [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)problému.</span><span class="sxs-lookup"><span data-stu-id="b5525-168">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translations-for-more-byte-array-constructs"></a><span data-ttu-id="b5525-169">Překlady dotazů pro další konstrukce bajtových polí</span><span class="sxs-lookup"><span data-stu-id="b5525-169">Query translations for more byte array constructs</span></span>

<span data-ttu-id="b5525-170">Dotazy používající vlastnosti Contains, Length, SequenceEqual atd. on Byte [] jsou nyní přeloženy na SQL.</span><span class="sxs-lookup"><span data-stu-id="b5525-170">Queries using Contains, Length, SequenceEqual, etc. on byte[] properties are now translated to SQL.</span></span>

<span data-ttu-id="b5525-171">Předběžná dokumentace je součástí [týdenního stavu EF pro 5. prosince 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="b5525-171">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="b5525-172">Další dokumentaci sleduje problém [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="b5525-172">Additional documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-reverse"></a><span data-ttu-id="b5525-173">Přesměrovat dotaz na zpětný překlad</span><span class="sxs-lookup"><span data-stu-id="b5525-173">Query translation for Reverse</span></span>

<span data-ttu-id="b5525-174">Dotazy používající `Reverse` jsou nyní přeloženy.</span><span class="sxs-lookup"><span data-stu-id="b5525-174">Queries using `Reverse` are now translated.</span></span>
<span data-ttu-id="b5525-175">Například:</span><span class="sxs-lookup"><span data-stu-id="b5525-175">For example:</span></span>

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

<span data-ttu-id="b5525-176">Dokumentace je sledována pomocí [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)problému.</span><span class="sxs-lookup"><span data-stu-id="b5525-176">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-bitwise-operators"></a><span data-ttu-id="b5525-177">Převod dotazů pro bitové operátory</span><span class="sxs-lookup"><span data-stu-id="b5525-177">Query translation for bitwise operators</span></span>

<span data-ttu-id="b5525-178">Dotazy používající bitové operátory jsou nyní přeloženy ve více případech například:</span><span class="sxs-lookup"><span data-stu-id="b5525-178">Queries using bitwise operators are now translated in more cases For example:</span></span>

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

<span data-ttu-id="b5525-179">Dokumentace je sledována pomocí [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)problému.</span><span class="sxs-lookup"><span data-stu-id="b5525-179">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-strings-on-cosmos"></a><span data-ttu-id="b5525-180">Překlad dotazů pro řetězce v Cosmos</span><span class="sxs-lookup"><span data-stu-id="b5525-180">Query translation for strings on Cosmos</span></span>

<span data-ttu-id="b5525-181">Dotazy, které používají metody řetězce Contains, StartsWith a EndsWith, jsou nyní přeloženy při použití poskytovatele Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="b5525-181">Queries that use the string methods Contains, StartsWith, and EndsWith are now translated when using the Azure Cosmos DB provider.</span></span>

<span data-ttu-id="b5525-182">Dokumentace je sledována pomocí [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)problému.</span><span class="sxs-lookup"><span data-stu-id="b5525-182">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>
