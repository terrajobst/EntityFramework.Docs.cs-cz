---
title: Co je nového v EF základní 2.1 - EF jádra
author: divega
ms.author: divega
ms.date: 2/20/2018
ms.assetid: 585F90A3-4D5A-4DD1-92D8-5243B14E0FEC
ms.technology: entity-framework-core
uid: core/what-is-new/ef-core-2.1
ms.openlocfilehash: 2372a6b2e3f3b7b1d9214a6ea321fe28cea45fff
ms.sourcegitcommit: 72e59e6af86b568653e1b29727529dfd7f65d312
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34754422"
---
# <a name="new-features-in-ef-core-21"></a><span data-ttu-id="1b256-102">Nové funkce v EF základní 2.1</span><span class="sxs-lookup"><span data-stu-id="1b256-102">New features in EF Core 2.1</span></span>

<span data-ttu-id="1b256-103">Kromě množství oprav chyb a malé vylepšení funkčnosti a výkonu EF základní 2.1 obsahuje některé zajímavé nové funkce:</span><span class="sxs-lookup"><span data-stu-id="1b256-103">Besides numerous bug fixes and small functional and performance enhancements, EF Core 2.1 includes some compelling new features:</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="1b256-104">opožděného načítání</span><span class="sxs-lookup"><span data-stu-id="1b256-104">Lazy loading</span></span>
<span data-ttu-id="1b256-105">Základní EF nyní obsahuje stavební bloky potřebné pro každý, kdo k vytváření tříd entit, které můžete načíst jejich navigační vlastnosti na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="1b256-105">EF Core now contains the necessary building blocks for anyone to author entity classes that can load their navigation properties on demand.</span></span> <span data-ttu-id="1b256-106">Také jsme vytvořili nový balíček, Microsoft.EntityFrameworkCore.Proxies, která využívá tyto stavební bloky k vytvoření opožděného načítání proxy třídy na základě minimálně upravit tříd entit (například třídy s virtuální navigační vlastnosti).</span><span class="sxs-lookup"><span data-stu-id="1b256-106">We have also created a new package, Microsoft.EntityFrameworkCore.Proxies, that leverages those building blocks to produce lazy loading proxy classes based on minimally modified entity classes (for example, classes with virtual navigation properties).</span></span>

<span data-ttu-id="1b256-107">Pro čtení [část opožděného načítání](xref:core/querying/related-data#lazy-loading) Další informace o tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="1b256-107">Read the [section on lazy loading](xref:core/querying/related-data#lazy-loading) for more information about this topic.</span></span>

## <a name="parameters-in-entity-constructors"></a><span data-ttu-id="1b256-108">Parametry v konstruktorech entity</span><span class="sxs-lookup"><span data-stu-id="1b256-108">Parameters in entity constructors</span></span>
<span data-ttu-id="1b256-109">Jako jeden z požadovaných stavební bloky pro opožděného načítání jsme povoleno vytváření entit, které vzít parametry v jejich konstruktory.</span><span class="sxs-lookup"><span data-stu-id="1b256-109">As one of the required building blocks for lazy loading, we enabled the creation of entities that take parameters in their constructors.</span></span> <span data-ttu-id="1b256-110">Parametry můžete vložit hodnoty vlastností, opožděného načítání Delegáti a služby.</span><span class="sxs-lookup"><span data-stu-id="1b256-110">You can use parameters to inject property values, lazy loading delegates, and services.</span></span>

<span data-ttu-id="1b256-111">Pro čtení [části na entity konstruktor s parametry](xref:core/modeling/constructors) Další informace o tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="1b256-111">Read the [section on entity constructor with parameters](xref:core/modeling/constructors) for more information about this topic.</span></span>

## <a name="value-conversions"></a><span data-ttu-id="1b256-112">Převody hodnot</span><span class="sxs-lookup"><span data-stu-id="1b256-112">Value conversions</span></span>
<span data-ttu-id="1b256-113">Až do této chvíle může EF základní mapují jenom vlastnosti typů nativně podporuje základní zprostředkovatel databáze.</span><span class="sxs-lookup"><span data-stu-id="1b256-113">Until now, EF Core could only map properties of types natively supported by the underlying database provider.</span></span> <span data-ttu-id="1b256-114">Hodnoty byly zkopírovány přepínat mezi sloupci a vlastnosti bez jakékoli transformace.</span><span class="sxs-lookup"><span data-stu-id="1b256-114">Values were copied back and forth between columns and properties without any transformation.</span></span> <span data-ttu-id="1b256-115">Počínaje EF základní 2.1, převody hodnot můžete použít k transformaci hodnot zjištěných z sloupců, než je použijete k vlastnosti a naopak.</span><span class="sxs-lookup"><span data-stu-id="1b256-115">Starting with EF Core 2.1, value conversions can be applied to transform the values obtained from columns before they are applied to properties, and vice versa.</span></span> <span data-ttu-id="1b256-116">Máme několik převody, které mohou být použity podle konvence podle potřeby, jakož i rozhraní API explicitní konfigurace, který umožňuje registraci vlastní převody mezi sloupci a vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="1b256-116">We have a number of conversions that can be applied by convention as necessary, as well as an explicit configuration API that allows registering custom conversions between columns and properties.</span></span> <span data-ttu-id="1b256-117">Některé aplikace této funkce jsou:</span><span class="sxs-lookup"><span data-stu-id="1b256-117">Some of the application of this feature are:</span></span>

- <span data-ttu-id="1b256-118">Ukládání výčty jako řetězce</span><span class="sxs-lookup"><span data-stu-id="1b256-118">Storing enums as strings</span></span>
- <span data-ttu-id="1b256-119">Mapování nepodepsané celých čísel s SQL serverem</span><span class="sxs-lookup"><span data-stu-id="1b256-119">Mapping unsigned integers with SQL Server</span></span>
- <span data-ttu-id="1b256-120">Automatické šifrování a dešifrování hodnot vlastností</span><span class="sxs-lookup"><span data-stu-id="1b256-120">Automatic encryption and decryption of property values</span></span>

<span data-ttu-id="1b256-121">Pro čtení [část převody hodnot](xref:core/modeling/value-conversions) Další informace o tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="1b256-121">Read the [section on value conversions](xref:core/modeling/value-conversions) for more information about this topic.</span></span>  

## <a name="linq-groupby-translation"></a><span data-ttu-id="1b256-122">Překlad LINQ GroupBy</span><span class="sxs-lookup"><span data-stu-id="1b256-122">LINQ GroupBy translation</span></span>
<span data-ttu-id="1b256-123">Před verzí 2.1, v základní EF operátor GroupBy LINQ by vždy vyhodnoceny v paměti.</span><span class="sxs-lookup"><span data-stu-id="1b256-123">Before version 2.1, in EF Core the GroupBy LINQ operator would always be evaluated in memory.</span></span> <span data-ttu-id="1b256-124">Teď podporují překladu klauzule SQL GROUP BY ve nejběžnější případy.</span><span class="sxs-lookup"><span data-stu-id="1b256-124">We now support translating it to the SQL GROUP BY clause in most common cases.</span></span>

<span data-ttu-id="1b256-125">Tento příklad ukazuje dotaz s GroupBy slouží k výpočtu různé agregační funkce:</span><span class="sxs-lookup"><span data-stu-id="1b256-125">This example shows a query with GroupBy used to compute various aggregate functions:</span></span>

``` csharp
var query = context.Orders
    .GroupBy(o => new { o.CustomerId, o.EmployeeId })
    .Select(g => new
        {
          g.Key.CustomerId,
          g.Key.EmployeeId,
          Sum = g.Sum(o => o.Amount),
          Min = g.Min(o => o.Amount),
          Max = g.Max(o => o.Amount),
          Avg = g.Average(o => Amount)
        });
```

<span data-ttu-id="1b256-126">Odpovídající překlad SQL vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="1b256-126">The corresponding SQL translation looks like this:</span></span>

``` SQL
SELECT [o].[CustomerId], [o].[EmployeeId],
    SUM([o].[Amount]), MIN([o].[Amount]), MAX([o].[Amount]), AVG([o].[Amount])
FROM [Orders] AS [o]
GROUP BY [o].[CustomerId], [o].[EmployeeId];
```

## <a name="data-seeding"></a><span data-ttu-id="1b256-127">Data synchronizace replik indexů</span><span class="sxs-lookup"><span data-stu-id="1b256-127">Data Seeding</span></span>
<span data-ttu-id="1b256-128">S novou verzí je možné zajistit počáteční data k naplnění databáze.</span><span class="sxs-lookup"><span data-stu-id="1b256-128">With the new release it will be possible to provide initial data to populate a database.</span></span> <span data-ttu-id="1b256-129">Na rozdíl od v EF6, synchronizace replik indexů dat je přidružena k typu entity jako součást konfigurace modelu.</span><span class="sxs-lookup"><span data-stu-id="1b256-129">Unlike in EF6, seeding data is associated to an entity type as part of the model configuration.</span></span> <span data-ttu-id="1b256-130">Potom základní EF migrace můžete vypočítat automaticky, co vložit, aktualizovat nebo odstranit potřeba operations použijí při upgradu databáze na novou verzi modelu.</span><span class="sxs-lookup"><span data-stu-id="1b256-130">Then EF Core migrations can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="1b256-131">Jako příklad, můžete to konfigurovat počáteční data pro metodu Post v `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="1b256-131">As an example, you can use this to configure seed data for a Post in `OnModelCreating`:</span></span>

``` csharp
modelBuilder.Entity<Post>().HasData(new Post{ Id = 1, Text = "Hello World!" });
```

<span data-ttu-id="1b256-132">Pro čtení [část dat synchronizace replik indexů](xref:core/modeling/data-seeding) Další informace o tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="1b256-132">Read the [section on data seeding](xref:core/modeling/data-seeding) for more information about this topic.</span></span>  

## <a name="query-types"></a><span data-ttu-id="1b256-133">Typy dotazů</span><span class="sxs-lookup"><span data-stu-id="1b256-133">Query types</span></span>
<span data-ttu-id="1b256-134">Model EF základní můžete nyní zahrnují typy dotazů.</span><span class="sxs-lookup"><span data-stu-id="1b256-134">An EF Core model can now include query types.</span></span> <span data-ttu-id="1b256-135">Na rozdíl od typy entit, typy dotazů není mít definované na nich klíče a nelze vložit, odstranit nebo aktualizovat (tj. jsou jen pro čtení), ale mohou být vráceny přímo pomocí dotazů.</span><span class="sxs-lookup"><span data-stu-id="1b256-135">Unlike entity types, query types do not have keys defined on them and cannot be inserted, deleted or updated (i.e. they are read-only), but they can be returned directly by queries.</span></span> <span data-ttu-id="1b256-136">Některé scénáře použití pro typy dotazů jsou:</span><span class="sxs-lookup"><span data-stu-id="1b256-136">Some of the usage scenarios for query types are:</span></span>

- <span data-ttu-id="1b256-137">Mapování na zobrazení bez primárních klíčů</span><span class="sxs-lookup"><span data-stu-id="1b256-137">Mapping to views without primary keys</span></span>
- <span data-ttu-id="1b256-138">Mapování tabulek bez primárních klíčů</span><span class="sxs-lookup"><span data-stu-id="1b256-138">Mapping to tables without primary keys</span></span>
- <span data-ttu-id="1b256-139">Mapování na dotazy, které jsou definované v modelu</span><span class="sxs-lookup"><span data-stu-id="1b256-139">Mapping to queries defined in the model</span></span>
- <span data-ttu-id="1b256-140">Slouží jako návratový typ pro `FromSql()` dotazy</span><span class="sxs-lookup"><span data-stu-id="1b256-140">Serving as the return type for `FromSql()` queries</span></span>

<span data-ttu-id="1b256-141">Pro čtení [části na typy dotazů](xref:core/modeling/query-types) Další informace o tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="1b256-141">Read the [section on query types](xref:core/modeling/query-types) for more information about this topic.</span></span>

## <a name="include-for-derived-types"></a><span data-ttu-id="1b256-142">Zahrnout pro odvozené typy</span><span class="sxs-lookup"><span data-stu-id="1b256-142">Include for derived types</span></span>
<span data-ttu-id="1b256-143">Je teď možné zadat definována pouze na navigační vlastnosti odvozených typů při zápisu výrazy pro `Include` metoda.</span><span class="sxs-lookup"><span data-stu-id="1b256-143">It will be now possible to specify navigation properties only defined on derived types when writing expressions for the `Include` method.</span></span> <span data-ttu-id="1b256-144">Pro verzi silného typu `Include`, podporujeme pomocí explicitní přetypování nebo `as` operátor.</span><span class="sxs-lookup"><span data-stu-id="1b256-144">For the strongly typed version of `Include`, we support using either an explicit cast or the `as` operator.</span></span> <span data-ttu-id="1b256-145">Nyní také podporují odkazování na názvy navigační vlastnost definovanou u odvozených typů v řetězec verze `Include`:</span><span class="sxs-lookup"><span data-stu-id="1b256-145">We also now support referencing the names of navigation property defined on derived types in the string version of `Include`:</span></span>

``` csharp
var option1 = context.People.Include(p => ((Student)p).School);
var option2 = context.People.Include(p => (p as Student).School);
var option3 = context.People.Include("School");
```

<span data-ttu-id="1b256-146">Pro čtení [části na zahrnout u odvozených typů](xref:core/querying/related-data#include-on-derived-types) Další informace o tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="1b256-146">Read the [section on Include with derived types](xref:core/querying/related-data#include-on-derived-types) for more information about this topic.</span></span>

## <a name="systemtransactions-support"></a><span data-ttu-id="1b256-147">System.Transactions – podpora</span><span class="sxs-lookup"><span data-stu-id="1b256-147">System.Transactions support</span></span>
<span data-ttu-id="1b256-148">Jsme přidali schopnost pracovat s funkcemi System.Transactions – například TransactionScope.</span><span class="sxs-lookup"><span data-stu-id="1b256-148">We have added the ability to work with System.Transactions features such as TransactionScope.</span></span> <span data-ttu-id="1b256-149">To bude fungovat na rozhraní .NET Framework a .NET Core, při použití zprostředkovatele databáze, které ji podporují.</span><span class="sxs-lookup"><span data-stu-id="1b256-149">This will work on both .NET Framework and .NET Core when using database providers that support it.</span></span>

<span data-ttu-id="1b256-150">Pro čtení [část System.Transactions –](xref:core/saving/transactions#using-systemtransactions) Další informace o tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="1b256-150">Read the [section on System.Transactions](xref:core/saving/transactions#using-systemtransactions) for more information about this topic.</span></span>

## <a name="better-column-ordering-in-initial-migration"></a><span data-ttu-id="1b256-151">Lepší pořadí sloupců v počáteční migrace</span><span class="sxs-lookup"><span data-stu-id="1b256-151">Better column ordering in initial migration</span></span>
<span data-ttu-id="1b256-152">Na základě názorů zákazníků, jsme jste aktualizovali migrace na začátku generovat sloupce pro tabulky ve stejném pořadí jako vlastnosti jsou deklarované v třídách.</span><span class="sxs-lookup"><span data-stu-id="1b256-152">Based on customer feedback, we have updated migrations to initially generate columns for tables in the same order as properties are declared in classes.</span></span> <span data-ttu-id="1b256-153">Všimněte si, že EF základní nelze změnit pořadí při přidání nové členy po vytvoření počáteční tabulky.</span><span class="sxs-lookup"><span data-stu-id="1b256-153">Note that EF Core cannot change order when new members are added after the initial table creation.</span></span>

## <a name="optimization-of-correlated-subqueries"></a><span data-ttu-id="1b256-154">Optimalizace korelační poddotazy</span><span class="sxs-lookup"><span data-stu-id="1b256-154">Optimization of correlated subqueries</span></span>
<span data-ttu-id="1b256-155">Jsme vylepšili naše překlad dotazu, aby se zabránilo spouštění "N + 1" dotazy SQL v mnoha běžným scénářům, ve kterých využití navigační vlastnosti v projekci vede k spojování dat z dotazu kořenové s daty z korelační poddotaz.</span><span class="sxs-lookup"><span data-stu-id="1b256-155">We have improved our query translation to avoid executing "N + 1" SQL queries in many common scenarios in which the usage of a navigation property in the projection leads to joining data from the root query with data from a correlated subquery.</span></span> <span data-ttu-id="1b256-156">Optimalizace vyžaduje ukládání do vyrovnávací paměti výsledky tvoří poddotaz a je nutné upravit dotaz tak, aby výslovný souhlas nové chování.</span><span class="sxs-lookup"><span data-stu-id="1b256-156">The optimization requires buffering the results form the subquery, and we require that you modify the query to opt-in the new behavior.</span></span>

<span data-ttu-id="1b256-157">Jako příklad následující dotaz normálně získá přeložit na jeden dotaz pro zákazníky, plus N (kde "N" je počet zákazníků vrátil) oddělit dotazy pro objednávky:</span><span class="sxs-lookup"><span data-stu-id="1b256-157">As an example, the following query normally gets translated into one query for Customers, plus N (where "N" is the number of customers returned) separate queries for Orders:</span></span>

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount));
```

<span data-ttu-id="1b256-158">Zahrnutím `ToList()` na správném místě znamenat, že ukládání do vyrovnávací paměti je vhodné pro objednávky, které umožňují optimalizace:</span><span class="sxs-lookup"><span data-stu-id="1b256-158">By including `ToList()` in the right place, you indicate that buffering is appropriate for the Orders, which enable the optimization:</span></span>

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount).ToList());
```

<span data-ttu-id="1b256-159">Všimněte si, že tento dotaz bude možné přeložit pouze dva dotazy SQL: jeden pro zákazníky a dalším pro objednávky.</span><span class="sxs-lookup"><span data-stu-id="1b256-159">Note that this query will be translated to only two SQL queries: One for Customers and the next one for Orders.</span></span>

## <a name="owned-attribute"></a><span data-ttu-id="1b256-160">Atribut [vlastní]</span><span class="sxs-lookup"><span data-stu-id="1b256-160">[Owned] attribute</span></span>

<span data-ttu-id="1b256-161">Nyní je možné nakonfigurovat [vlastní typy entit](xref:core/modeling/owned-entities) podle jednoduše zadávání poznámek k na typ s `[Owned]` a pak zajistit, owner entity přidají do modelu:</span><span class="sxs-lookup"><span data-stu-id="1b256-161">It is now possible to configure [owned entity types](xref:core/modeling/owned-entities) by simply annotating the type with `[Owned]` and then making sure the owner entity is added to the model:</span></span>

``` csharp
[Owned]
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}
```

## <a name="command-line-tool-dotnet-ef-included-in-net-core-sdk"></a><span data-ttu-id="1b256-162">Nástroj příkazového řádku dotnet-ef zahrnuté v rozhraní .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="1b256-162">Command-line tool dotnet-ef included in .NET Core SDK</span></span>

<span data-ttu-id="1b256-163">_Dotnet ef_ příkazy jsou teď součástí rozhraní .NET Core SDK, proto ji už nebude nutné k použití DotNetCliToolReference v projektu, abyste mohli používat migrace nebo vygenerovat DbContext z existující databáze.</span><span class="sxs-lookup"><span data-stu-id="1b256-163">The _dotnet-ef_ commands are now part of the .NET Core SDK, therefore it will no longer be necessary to use DotNetCliToolReference in the project to be able to use migrations or to scaffold a DbContext from an existing database.</span></span>

<span data-ttu-id="1b256-164">Projděte část o [instalaci nástrojů](xref:core/miscellaneous/cli/dotnet#installing-the-tools) další podrobnosti o tom, jak povolit nástroje příkazového řádku pro různé verze rozhraní .NET Core SDK a EF jádra.</span><span class="sxs-lookup"><span data-stu-id="1b256-164">See the section on [installing the tools](xref:core/miscellaneous/cli/dotnet#installing-the-tools) for more details on how to enable command line tools for different versions of the .NET Core SDK and EF Core.</span></span>

## <a name="microsoftentityframeworkcoreabstractions-package"></a><span data-ttu-id="1b256-165">Balíček Microsoft.EntityFrameworkCore.Abstractions</span><span class="sxs-lookup"><span data-stu-id="1b256-165">Microsoft.EntityFrameworkCore.Abstractions package</span></span>
<span data-ttu-id="1b256-166">Nový balíček obsahuje atributy a rozhraní, které můžete ve svých projektech light až EF hlavní funkce bez nutnosti převádět závislost na základní EF jako celek.</span><span class="sxs-lookup"><span data-stu-id="1b256-166">The new package contains attributes and interfaces that you can use in your projects to light up EF Core features without taking a dependency on EF Core as a whole.</span></span> <span data-ttu-id="1b256-167">Například ILazyLoader rozhraní a atribut [vlastněno] jsou umístěné v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="1b256-167">For example, the [Owned] attribute and the ILazyLoader interface are located here.</span></span>

## <a name="state-change-events"></a><span data-ttu-id="1b256-168">Události změny stavu</span><span class="sxs-lookup"><span data-stu-id="1b256-168">State change events</span></span>

<span data-ttu-id="1b256-169">Nové `Tracked` a `StateChanged` událostí na `ChangeTracker` slouží k zápisu logiky, která reaguje na entity zadáním DbContext nebo změně jejich stavu.</span><span class="sxs-lookup"><span data-stu-id="1b256-169">New `Tracked` And `StateChanged` events on `ChangeTracker` can be used to write logic that reacts to entities entering the DbContext or changing their state.</span></span>

## <a name="raw-sql-parameter-analyzer"></a><span data-ttu-id="1b256-170">Nezpracovaná analyzátoru parametr SQL</span><span class="sxs-lookup"><span data-stu-id="1b256-170">Raw SQL parameter analyzer</span></span>

<span data-ttu-id="1b256-171">Nový analyzátor kódu je součástí EF jádra, který zjistí potenciálně nebezpečného použití rozhraní API nezpracovaná SQL, jako je třeba `FromSql` nebo `ExecuteSqlCommand`.</span><span class="sxs-lookup"><span data-stu-id="1b256-171">A new code analyzer is included with EF Core that detects potentially unsafe usages of our raw-SQL APIs, like `FromSql` or `ExecuteSqlCommand`.</span></span> <span data-ttu-id="1b256-172">Například</span><span class="sxs-lookup"><span data-stu-id="1b256-172">E.g.</span></span> <span data-ttu-id="1b256-173">pro tento dotaz, se zobrazí upozornění, protože _minAge_ není parametry:</span><span class="sxs-lookup"><span data-stu-id="1b256-173">for the following query, you will see a warning because _minAge_ is not parameterized:</span></span>

``` csharp
var sql = $"SELECT * FROM People WHERE Age > {minAge}";
var query = context.People.FromSql(sql);
```

## <a name="database-provider-compatibility"></a><span data-ttu-id="1b256-174">Zprostředkovatel kompatibility databáze</span><span class="sxs-lookup"><span data-stu-id="1b256-174">Database provider compatibility</span></span>

<span data-ttu-id="1b256-175">Doporučuje se použít EF základní 2.1 zprostředkovatelům, které byly aktualizovány nebo alespoň testována pro práci s EF základní 2.1.</span><span class="sxs-lookup"><span data-stu-id="1b256-175">It is recommended that you use EF Core 2.1 with providers that have been updated or at least tested to work with EF Core 2.1.</span></span>

> [!TIP]
> <span data-ttu-id="1b256-176">Pokud zjistíte, že jsou všechny neočekávané nekompatibilita žádný problém nové funkce nebo pokud máte zpětnou vazbu na, nahlaste jej pomocí [náš sledovací modul problém](https://github.com/aspnet/EntityFrameworkCore/issues/new).</span><span class="sxs-lookup"><span data-stu-id="1b256-176">If you find any unexpected incompatibility or any issue in the new features, or if you have feedback on them, please report it using [our issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues/new).</span></span>
