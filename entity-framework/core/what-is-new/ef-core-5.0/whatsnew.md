---
title: Co je nového v EF Core 5.0
description: Přehled nových funkcí v EF Core 5.0
author: ajcvickers
ms.date: 03/30/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: c047a308cadf44eea577dcab29b68b36942a50df
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "80634271"
---
# <a name="whats-new-in-ef-core-50"></a><span data-ttu-id="11948-103">Co je nového v EF Core 5.0</span><span class="sxs-lookup"><span data-stu-id="11948-103">What's New in EF Core 5.0</span></span>

<span data-ttu-id="11948-104">EF Core 5.0 je v současné době ve vývoji.</span><span class="sxs-lookup"><span data-stu-id="11948-104">EF Core 5.0 is currently in development.</span></span>
<span data-ttu-id="11948-105">Tato stránka bude obsahovat přehled zajímavých změn zavedených v každém náhledu.</span><span class="sxs-lookup"><span data-stu-id="11948-105">This page will contain an overview of interesting changes introduced in each preview.</span></span>

<span data-ttu-id="11948-106">Tato stránka neduplikuje [plán ef core 5.0](plan.md).</span><span class="sxs-lookup"><span data-stu-id="11948-106">This page does not duplicate the [plan for EF Core 5.0](plan.md).</span></span>
<span data-ttu-id="11948-107">Plán popisuje celková témata pro EF Core 5.0, včetně všeho, co plánujeme zahrnout před odesláním konečné verze.</span><span class="sxs-lookup"><span data-stu-id="11948-107">The plan describes the overall themes for EF Core 5.0, including everything we are planning to include before shipping the final release.</span></span>

<span data-ttu-id="11948-108">Odkazy zde přidáme k oficiální dokumentaci tak, jak je zveřejněna.</span><span class="sxs-lookup"><span data-stu-id="11948-108">We will add links from here to the official documentation as it is published.</span></span>

## <a name="preview-2"></a><span data-ttu-id="11948-109">Preview 2</span><span class="sxs-lookup"><span data-stu-id="11948-109">Preview 2</span></span>

### <a name="use-a-c-attribute-to-specify-a-property-backing-field"></a><span data-ttu-id="11948-110">Použití atributu C# k určení pole pro podporu vlastností</span><span class="sxs-lookup"><span data-stu-id="11948-110">Use a C# attribute to specify a property backing field</span></span>

<span data-ttu-id="11948-111">Atribut C# lze nyní použít k určení záložního pole pro vlastnost.</span><span class="sxs-lookup"><span data-stu-id="11948-111">A C# attribute can now be used to specify the backing field for a property.</span></span>
<span data-ttu-id="11948-112">Tento atribut umožňuje EF Core stále zapisovat a číst z doprovodného pole, jak by normálně dojít, i v případě, že záložní pole nelze najít automaticky.</span><span class="sxs-lookup"><span data-stu-id="11948-112">This attribute allows EF Core to still write to and read from the backing field as would normally happen, even when the backing field cannot be found automatically.</span></span>
<span data-ttu-id="11948-113">Příklad:</span><span class="sxs-lookup"><span data-stu-id="11948-113">For example:</span></span>

```CSharp
public class Blog
{
    private string _mainTitle;

    public int Id { get; set; }

    [BackingField(nameof(_mainTitle))]
    public string Title
    {
        get => _mainTitle;
        set => _mainTitle = value;
    }
}
```

<span data-ttu-id="11948-114">Dokumentace je sledována podle [#2230](https://github.com/dotnet/EntityFramework.Docs/issues/2230).</span><span class="sxs-lookup"><span data-stu-id="11948-114">Documentation is tracked by issue [#2230](https://github.com/dotnet/EntityFramework.Docs/issues/2230).</span></span>

### <a name="complete-discriminator-mapping"></a><span data-ttu-id="11948-115">Kompletní mapování diskriminátoru</span><span class="sxs-lookup"><span data-stu-id="11948-115">Complete discriminator mapping</span></span>

<span data-ttu-id="11948-116">EF Core používá diskriminátor sloupec pro [mapování TPH hierarchie dědičnosti](/ef/core/modeling/inheritance).</span><span class="sxs-lookup"><span data-stu-id="11948-116">EF Core uses a discriminator column for [TPH mapping of an inheritance hierarchy](/ef/core/modeling/inheritance).</span></span>
<span data-ttu-id="11948-117">Některá vylepšení výkonu jsou možná, pokud EF Core zná všechny možné hodnoty diskriminátoru.</span><span class="sxs-lookup"><span data-stu-id="11948-117">Some performance enhancements are possible so long as EF Core knows all possible values for the discriminator.</span></span>
<span data-ttu-id="11948-118">EF Core 5.0 nyní implementuje tato vylepšení.</span><span class="sxs-lookup"><span data-stu-id="11948-118">EF Core 5.0 now implements these enhancements.</span></span>

<span data-ttu-id="11948-119">Například předchozí verze EF Core by vždy generovat tento SQL pro dotaz vrácení všech typů v hierarchii:</span><span class="sxs-lookup"><span data-stu-id="11948-119">For example, previous versions of EF Core would always generate this SQL for a query returning all types in a hierarchy:</span></span>

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
WHERE [a].[Discriminator] IN (N'Animal', N'Cat', N'Dog', N'Human')
```

<span data-ttu-id="11948-120">Ef Core 5.0 nyní vygeneruje následující, když je nakonfigurováno úplné mapování diskriminátoru:</span><span class="sxs-lookup"><span data-stu-id="11948-120">EF Core 5.0 will now generate the following when a complete discriminator mapping is configured:</span></span>

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
```

<span data-ttu-id="11948-121">Bude to výchozí chování začínající náhledem 3.</span><span class="sxs-lookup"><span data-stu-id="11948-121">It will be the default behavior starting with preview 3.</span></span>

### <a name="performance-improvements-in-microsoftdatasqlite"></a><span data-ttu-id="11948-122">Vylepšení výkonu v Microsoft.Data.Sqlite</span><span class="sxs-lookup"><span data-stu-id="11948-122">Performance improvements in Microsoft.Data.Sqlite</span></span>

<span data-ttu-id="11948-123">Provedli jsme dvě vylepšení výkonu pro SQLIte:</span><span class="sxs-lookup"><span data-stu-id="11948-123">We have made two performance improvements for SQLIte:</span></span>

* <span data-ttu-id="11948-124">Načítání binárních a řetězcových dat pomocí GetBytes, GetChars a GetTextReader je nyní efektivnější využitím sqliteblob a datových proudů.</span><span class="sxs-lookup"><span data-stu-id="11948-124">Retrieving binary and string data with GetBytes, GetChars, and GetTextReader is now more efficient by making use of SqliteBlob and streams.</span></span>
* <span data-ttu-id="11948-125">Inicializace SqliteConnection je nyní opožděná.</span><span class="sxs-lookup"><span data-stu-id="11948-125">Initialization of SqliteConnection is now lazy.</span></span>

<span data-ttu-id="11948-126">Tato vylepšení jsou v ADO.NET microsoft.Data.Sqlite zprostředkovatele a tím také zlepšit výkon mimo EF Core.</span><span class="sxs-lookup"><span data-stu-id="11948-126">These improvements are in the ADO.NET Microsoft.Data.Sqlite provider and hence also improve performance outside of EF Core.</span></span>

## <a name="preview-1"></a><span data-ttu-id="11948-127">Náhled 1</span><span class="sxs-lookup"><span data-stu-id="11948-127">Preview 1</span></span>

### <a name="simple-logging"></a><span data-ttu-id="11948-128">Jednoduché protokolování</span><span class="sxs-lookup"><span data-stu-id="11948-128">Simple logging</span></span>

<span data-ttu-id="11948-129">Tato funkce přidává funkce `Database.Log` podobné v EF6.</span><span class="sxs-lookup"><span data-stu-id="11948-129">This feature adds functionality similar to `Database.Log` in EF6.</span></span>
<span data-ttu-id="11948-130">To znamená, že poskytuje jednoduchý způsob, jak získat protokoly z EF Core bez nutnosti konfigurovat jakýkoli druh architektury externíprotokolování.</span><span class="sxs-lookup"><span data-stu-id="11948-130">That is, it provides a simple way to get logs from EF Core without the need to configure any kind of external logging framework.</span></span>

<span data-ttu-id="11948-131">Předběžná dokumentace je zahrnuta do [stavu EF týdně pro prosinec 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="11948-131">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="11948-132">Další dokumentace je sledována podle [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).</span><span class="sxs-lookup"><span data-stu-id="11948-132">Additional documentation is tracked by issue [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).</span></span>

### <a name="simple-way-to-get-generated-sql"></a><span data-ttu-id="11948-133">Jednoduchý způsob, jak získat generované SQL</span><span class="sxs-lookup"><span data-stu-id="11948-133">Simple way to get generated SQL</span></span>

<span data-ttu-id="11948-134">EF Core 5.0 `ToQueryString` zavádí metodu rozšíření, která vrátí SQL, které EF Core vygeneruje při provádění dotazu LINQ.</span><span class="sxs-lookup"><span data-stu-id="11948-134">EF Core 5.0 introduces the `ToQueryString` extension method, which will return the SQL that EF Core will generate when executing a LINQ query.</span></span>

<span data-ttu-id="11948-135">Předběžná dokumentace je zahrnuta do [stavu EF týdně pro leden 9, 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span><span class="sxs-lookup"><span data-stu-id="11948-135">Preliminary documentation is included in the [EF weekly status for January 9, 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span></span>

<span data-ttu-id="11948-136">Další dokumentace je sledována podle [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).</span><span class="sxs-lookup"><span data-stu-id="11948-136">Additional documentation is tracked by issue [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).</span></span>

### <a name="use-a-c-attribute-to-indicate-that-an-entity-has-no-key"></a><span data-ttu-id="11948-137">Použití atributu C# k označení, že entita nemá žádný klíč</span><span class="sxs-lookup"><span data-stu-id="11948-137">Use a C# attribute to indicate that an entity has no key</span></span>

<span data-ttu-id="11948-138">Typ entity lze nyní nakonfigurovat tak, `KeylessAttribute`aby pomocí nového .</span><span class="sxs-lookup"><span data-stu-id="11948-138">An entity type can now be configured as having no key using the new `KeylessAttribute`.</span></span>
<span data-ttu-id="11948-139">Příklad:</span><span class="sxs-lookup"><span data-stu-id="11948-139">For example:</span></span>

```CSharp
[Keyless]
public class Address
{
    public string Street { get; set; }
    public string City { get; set; }
    public int Zip { get; set; }
}
```

<span data-ttu-id="11948-140">Dokumentace je sledována podle [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186).</span><span class="sxs-lookup"><span data-stu-id="11948-140">Documentation is tracked by issue [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186).</span></span>

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a><span data-ttu-id="11948-141">Připojení nebo připojovací řetězec lze změnit na inicializovaném dbcontext</span><span class="sxs-lookup"><span data-stu-id="11948-141">Connection or connection string can be changed on initialized DbContext</span></span>

<span data-ttu-id="11948-142">Nyní je jednodušší vytvořit instanci DbContext bez připojení nebo připojovacířetězec.</span><span class="sxs-lookup"><span data-stu-id="11948-142">It is now easier to create a DbContext instance without any connection or connection string.</span></span>
<span data-ttu-id="11948-143">Také připojení nebo připojovací řetězec lze nyní zmutovat na instanci kontextu.</span><span class="sxs-lookup"><span data-stu-id="11948-143">Also, the connection or connection string can now be mutated on the context instance.</span></span>
<span data-ttu-id="11948-144">Tato funkce umožňuje stejnou instanci kontextu dynamicky se připojit k různým databázím.</span><span class="sxs-lookup"><span data-stu-id="11948-144">This feature allows the same context instance to dynamically connect to different databases.</span></span>

<span data-ttu-id="11948-145">Dokumentace je sledována podle [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).</span><span class="sxs-lookup"><span data-stu-id="11948-145">Documentation is tracked by issue [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).</span></span>

### <a name="change-tracking-proxies"></a><span data-ttu-id="11948-146">Proxy servery pro sledování změn</span><span class="sxs-lookup"><span data-stu-id="11948-146">Change-tracking proxies</span></span>

<span data-ttu-id="11948-147">EF Core nyní můžete generovat runtime proxy servery, které automaticky implementují [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) a [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span><span class="sxs-lookup"><span data-stu-id="11948-147">EF Core can now generate runtime proxies that automatically implement [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) and [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span></span>
<span data-ttu-id="11948-148">Ty pak sestavy změny hodnoty vlastností entity přímo EF Core, vyhnout se nutnosti hledat změny.</span><span class="sxs-lookup"><span data-stu-id="11948-148">These then report value changes on entity properties directly to EF Core, avoiding the need to scan for changes.</span></span>
<span data-ttu-id="11948-149">Nicméně, proxy přijít s vlastní sadou omezení, takže nejsou pro každého.</span><span class="sxs-lookup"><span data-stu-id="11948-149">However, proxies come with their own set of limitations, so they are not for everyone.</span></span>

<span data-ttu-id="11948-150">Dokumentace je sledována podle [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).</span><span class="sxs-lookup"><span data-stu-id="11948-150">Documentation is tracked by issue [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).</span></span>

### <a name="enhanced-debug-views"></a><span data-ttu-id="11948-151">Vylepšená zobrazení ladění</span><span class="sxs-lookup"><span data-stu-id="11948-151">Enhanced debug views</span></span>

<span data-ttu-id="11948-152">Ladicí zobrazení jsou snadný způsob, jak se podívat na vnitřní ef jádra při ladění problémy.</span><span class="sxs-lookup"><span data-stu-id="11948-152">Debug views are an easy way to look at the internals of EF Core when debugging issues.</span></span>
<span data-ttu-id="11948-153">Před časem bylo implementováno zobrazení ladění modelu.</span><span class="sxs-lookup"><span data-stu-id="11948-153">A debug view for the Model was implemented some time ago.</span></span>
<span data-ttu-id="11948-154">Pro EF Core 5.0 jsme usnadnili čtení zobrazení modelu a přidali jsme nové zobrazení ladění pro sledované entity ve správci stavu.</span><span class="sxs-lookup"><span data-stu-id="11948-154">For EF Core 5.0, we have made the model view easier to read and added a new debug view for tracked entities in the state manager.</span></span>

<span data-ttu-id="11948-155">Předběžná dokumentace je zahrnuta do [stavu EF týdně pro prosinec 12, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span><span class="sxs-lookup"><span data-stu-id="11948-155">Preliminary documentation is included in the [EF weekly status for December 12, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span></span>

<span data-ttu-id="11948-156">Další dokumentace je sledována podle [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).</span><span class="sxs-lookup"><span data-stu-id="11948-156">Additional documentation is tracked by issue [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).</span></span>

### <a name="improved-handling-of-database-null-semantics"></a><span data-ttu-id="11948-157">Vylepšené zpracování sémantiky null databáze</span><span class="sxs-lookup"><span data-stu-id="11948-157">Improved handling of database null semantics</span></span>

<span data-ttu-id="11948-158">Relační databáze obvykle považují hodnotu NULL za neznámou hodnotu, a proto se nerovnají žádné jiné hodnotě NULL.</span><span class="sxs-lookup"><span data-stu-id="11948-158">Relational databases typically treat NULL as an unknown value and therefore not equal to any other NULL.</span></span>
<span data-ttu-id="11948-159">Zatímco C# zachází null jako definovanou hodnotu, která porovnává rovná se jakékoli jiné null.</span><span class="sxs-lookup"><span data-stu-id="11948-159">While C# treats null as a defined value, which compares equal to any other null.</span></span>
<span data-ttu-id="11948-160">EF Core ve výchozím nastavení překládá dotazy tak, aby používaly sémantiku C# null.</span><span class="sxs-lookup"><span data-stu-id="11948-160">EF Core by default translates queries so that they use C# null semantics.</span></span>
<span data-ttu-id="11948-161">EF Core 5.0 výrazně zvyšuje efektivitu těchto překladů.</span><span class="sxs-lookup"><span data-stu-id="11948-161">EF Core 5.0 greatly improves the efficiency of these translations.</span></span>

<span data-ttu-id="11948-162">Dokumentace je sledována podle [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).</span><span class="sxs-lookup"><span data-stu-id="11948-162">Documentation is tracked by issue [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).</span></span>

### <a name="indexer-properties"></a><span data-ttu-id="11948-163">Vlastnosti indexeru</span><span class="sxs-lookup"><span data-stu-id="11948-163">Indexer properties</span></span>

<span data-ttu-id="11948-164">EF Core 5.0 podporuje mapování vlastností indexeru Jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="11948-164">EF Core 5.0 supports mapping of C# indexer properties.</span></span>
<span data-ttu-id="11948-165">Tyto vlastnosti umožňují entity působit jako vlastnost tašky, kde jsou sloupce mapovány na pojmenované vlastnosti v vaku.</span><span class="sxs-lookup"><span data-stu-id="11948-165">These properties allow entities to act as property bags where columns are mapped to named properties in the bag.</span></span>

<span data-ttu-id="11948-166">Dokumentace je sledována podle [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).</span><span class="sxs-lookup"><span data-stu-id="11948-166">Documentation is tracked by issue [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).</span></span>

### <a name="generation-of-check-constraints-for-enum-mappings"></a><span data-ttu-id="11948-167">Generování kontrolních omezení pro mapování výčtu</span><span class="sxs-lookup"><span data-stu-id="11948-167">Generation of check constraints for enum mappings</span></span>

<span data-ttu-id="11948-168">Ef Core 5.0 Migrace nyní můžete generovat CHECK omezení pro mapování vlastností výčtu.</span><span class="sxs-lookup"><span data-stu-id="11948-168">EF Core 5.0 Migrations can now generate CHECK constraints for enum property mappings.</span></span>
<span data-ttu-id="11948-169">Příklad:</span><span class="sxs-lookup"><span data-stu-id="11948-169">For example:</span></span>

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN ('Useful', 'Useless', 'Unknown'))
```

<span data-ttu-id="11948-170">Dokumentace je sledována podle [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).</span><span class="sxs-lookup"><span data-stu-id="11948-170">Documentation is tracked by issue [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).</span></span>

### <a name="isrelational"></a><span data-ttu-id="11948-171">JeRelational</span><span class="sxs-lookup"><span data-stu-id="11948-171">IsRelational</span></span>

<span data-ttu-id="11948-172">Kromě `IsRelational` stávající `IsSqlServer`metody , `IsSqlite`a `IsInMemory`.</span><span class="sxs-lookup"><span data-stu-id="11948-172">A new `IsRelational` method has been added in addition to the existing `IsSqlServer`, `IsSqlite`, and `IsInMemory`.</span></span>
<span data-ttu-id="11948-173">Tuto metodu lze použít k testování, pokud DbContext používá libovolný zprostředkovatel relační databáze.</span><span class="sxs-lookup"><span data-stu-id="11948-173">This method can be used to test if the DbContext is using any relational database provider.</span></span>
<span data-ttu-id="11948-174">Příklad:</span><span class="sxs-lookup"><span data-stu-id="11948-174">For example:</span></span>

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    if (Database.IsRelational())
    {
        // Do relational-specific model configuration.
    }
}
```

<span data-ttu-id="11948-175">Dokumentace je sledována podle [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185).</span><span class="sxs-lookup"><span data-stu-id="11948-175">Documentation is tracked by issue [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185).</span></span>

### <a name="cosmos-optimistic-concurrency-with-etags"></a><span data-ttu-id="11948-176">Cosmos optimistická souběžnost s ETags</span><span class="sxs-lookup"><span data-stu-id="11948-176">Cosmos optimistic concurrency with ETags</span></span>

<span data-ttu-id="11948-177">Poskytovatel databáze Azure Cosmos DB teď podporuje optimistickou souběžnost pomocí etags.</span><span class="sxs-lookup"><span data-stu-id="11948-177">The Azure Cosmos DB database provider now supports optimistic concurrency using ETags.</span></span>
<span data-ttu-id="11948-178">Ke konfiguraci eTagu použijte tvůrce modelů v aplikaci OnModelCreating:</span><span class="sxs-lookup"><span data-stu-id="11948-178">Use the model builder in OnModelCreating to configure an ETag:</span></span>

```CSharp
builder.Entity<Customer>().Property(c => c.ETag).IsEtagConcurrency();
```

<span data-ttu-id="11948-179">SaveChanges pak vyvolá `DbUpdateConcurrencyException` konflikt souběžnosti, který [může být zpracován](https://docs.microsoft.com/ef/core/saving/concurrency) k implementaci opakování atd.</span><span class="sxs-lookup"><span data-stu-id="11948-179">SaveChanges will then throw an `DbUpdateConcurrencyException` on a concurrency conflict, which [can be handled](https://docs.microsoft.com/ef/core/saving/concurrency) to implement retries, etc.</span></span>

<span data-ttu-id="11948-180">Dokumentace je sledována podle [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).</span><span class="sxs-lookup"><span data-stu-id="11948-180">Documentation is tracked by issue [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).</span></span>

### <a name="query-translations-for-more-datetime-constructs"></a><span data-ttu-id="11948-181">Překlady dotazů pro další konstrukce DateTime</span><span class="sxs-lookup"><span data-stu-id="11948-181">Query translations for more DateTime constructs</span></span>

<span data-ttu-id="11948-182">Dotazy obsahující novou konstrukci DateTime jsou nyní přeloženy.</span><span class="sxs-lookup"><span data-stu-id="11948-182">Queries containing new DateTime construction are now translated.</span></span>

<span data-ttu-id="11948-183">Kromě toho jsou nyní mapovány následující funkce serveru SQL Server:</span><span class="sxs-lookup"><span data-stu-id="11948-183">In addition, the following SQL Server functions are now mapped:</span></span>

* <span data-ttu-id="11948-184">DatumDiffWeek</span><span class="sxs-lookup"><span data-stu-id="11948-184">DateDiffWeek</span></span>
* <span data-ttu-id="11948-185">DateFromParts</span><span class="sxs-lookup"><span data-stu-id="11948-185">DateFromParts</span></span>

<span data-ttu-id="11948-186">Příklad:</span><span class="sxs-lookup"><span data-stu-id="11948-186">For example:</span></span>

```CSharp
var count = context.Orders.Count(c => date > EF.Functions.DateFromParts(DateTime.Now.Year, 12, 25));

```

<span data-ttu-id="11948-187">Dokumentace je sledována podle [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="11948-187">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translations-for-more-byte-array-constructs"></a><span data-ttu-id="11948-188">Překlady dotazů pro více konstrukcí bajtového pole</span><span class="sxs-lookup"><span data-stu-id="11948-188">Query translations for more byte array constructs</span></span>

<span data-ttu-id="11948-189">Dotazy pomocí contains, length, sequenceequal atd.</span><span class="sxs-lookup"><span data-stu-id="11948-189">Queries using Contains, Length, SequenceEqual, etc. on byte[] properties are now translated to SQL.</span></span>

<span data-ttu-id="11948-190">Předběžná dokumentace je zahrnuta do [stavu EF týdně pro prosinec 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="11948-190">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="11948-191">Další dokumentace je sledována podle [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="11948-191">Additional documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-reverse"></a><span data-ttu-id="11948-192">Překlad dotazu pro reverzní</span><span class="sxs-lookup"><span data-stu-id="11948-192">Query translation for Reverse</span></span>

<span data-ttu-id="11948-193">Dotazy pomocí `Reverse` jsou nyní přeloženy.</span><span class="sxs-lookup"><span data-stu-id="11948-193">Queries using `Reverse` are now translated.</span></span>
<span data-ttu-id="11948-194">Příklad:</span><span class="sxs-lookup"><span data-stu-id="11948-194">For example:</span></span>

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

<span data-ttu-id="11948-195">Dokumentace je sledována podle [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="11948-195">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-bitwise-operators"></a><span data-ttu-id="11948-196">Překlad dotazu pro bitové operátory</span><span class="sxs-lookup"><span data-stu-id="11948-196">Query translation for bitwise operators</span></span>

<span data-ttu-id="11948-197">Dotazy pomocí bitových operátorů jsou nyní přeloženy ve více případech Například:</span><span class="sxs-lookup"><span data-stu-id="11948-197">Queries using bitwise operators are now translated in more cases For example:</span></span>

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

<span data-ttu-id="11948-198">Dokumentace je sledována podle [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="11948-198">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-strings-on-cosmos"></a><span data-ttu-id="11948-199">Překlad dotazu pro řetězce v Cosmos</span><span class="sxs-lookup"><span data-stu-id="11948-199">Query translation for strings on Cosmos</span></span>

<span data-ttu-id="11948-200">Dotazy, které používají metody řetězce Contains, StartsWith a EndsWith, jsou nyní přeloženy při použití zprostředkovatele Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="11948-200">Queries that use the string methods Contains, StartsWith, and EndsWith are now translated when using the Azure Cosmos DB provider.</span></span>

<span data-ttu-id="11948-201">Dokumentace je sledována podle [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="11948-201">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>
