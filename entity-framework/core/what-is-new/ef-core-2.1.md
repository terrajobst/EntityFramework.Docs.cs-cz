---
title: Co je nového v EF Core 2.1 - EF Core
author: divega
ms.date: 02/20/2018
ms.assetid: 585F90A3-4D5A-4DD1-92D8-5243B14E0FEC
uid: core/what-is-new/ef-core-2.1
ms.openlocfilehash: ba3a26bcd76cd0b9615b13f32456e7280afe533a
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417480"
---
# <a name="new-features-in-ef-core-21"></a><span data-ttu-id="a7adb-102">Nové funkce v EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="a7adb-102">New features in EF Core 2.1</span></span>

<span data-ttu-id="a7adb-103">Kromě mnoha oprav chyb a malých funkčních a výkonnostních vylepšení obsahuje EF Core 2.1 některé přesvědčivé nové funkce:</span><span class="sxs-lookup"><span data-stu-id="a7adb-103">Besides numerous bug fixes and small functional and performance enhancements, EF Core 2.1 includes some compelling new features:</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="a7adb-104">Opožděné načtení</span><span class="sxs-lookup"><span data-stu-id="a7adb-104">Lazy loading</span></span>

<span data-ttu-id="a7adb-105">EF Core nyní obsahuje nezbytné stavební bloky pro každého, kdo autor entity třídy, které můžete načíst jejich navigační vlastnosti na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="a7adb-105">EF Core now contains the necessary building blocks for anyone to author entity classes that can load their navigation properties on demand.</span></span> <span data-ttu-id="a7adb-106">Také jsme vytvořili nový balíček, Microsoft.EntityFrameworkCore.Proxies, který využívá tyto stavební bloky k výrobě opožděné načítání proxy třídy založené na minimálně upravené entity třídy (například třídy s vlastnostmi virtuální navigace).</span><span class="sxs-lookup"><span data-stu-id="a7adb-106">We have also created a new package, Microsoft.EntityFrameworkCore.Proxies, that leverages those building blocks to produce lazy loading proxy classes based on minimally modified entity classes (for example, classes with virtual navigation properties).</span></span>

<span data-ttu-id="a7adb-107">Další informace o tomto tématu naleznete v [části opožděné načítání.](xref:core/querying/related-data#lazy-loading)</span><span class="sxs-lookup"><span data-stu-id="a7adb-107">Read the [section on lazy loading](xref:core/querying/related-data#lazy-loading) for more information about this topic.</span></span>

## <a name="parameters-in-entity-constructors"></a><span data-ttu-id="a7adb-108">Parametry v konstruktorech entit</span><span class="sxs-lookup"><span data-stu-id="a7adb-108">Parameters in entity constructors</span></span>

<span data-ttu-id="a7adb-109">Jako jeden z požadovaných stavebních bloků pro opožděné načítání jsme povolili vytváření entit, které berou parametry v jejich konstruktorech.</span><span class="sxs-lookup"><span data-stu-id="a7adb-109">As one of the required building blocks for lazy loading, we enabled the creation of entities that take parameters in their constructors.</span></span> <span data-ttu-id="a7adb-110">Parametry můžete použít k vložení hodnoty vlastností, opožděné načítání delegátů a služeb.</span><span class="sxs-lookup"><span data-stu-id="a7adb-110">You can use parameters to inject property values, lazy loading delegates, and services.</span></span>

<span data-ttu-id="a7adb-111">Další informace o tomto tématu naleznete v [části o konstruktoru entit s parametry.](xref:core/modeling/constructors)</span><span class="sxs-lookup"><span data-stu-id="a7adb-111">Read the [section on entity constructor with parameters](xref:core/modeling/constructors) for more information about this topic.</span></span>

## <a name="value-conversions"></a><span data-ttu-id="a7adb-112">Převody hodnot</span><span class="sxs-lookup"><span data-stu-id="a7adb-112">Value conversions</span></span>

<span data-ttu-id="a7adb-113">Až do teď EF Core mohl mapovat pouze vlastnosti typů nativně podporovaných zprostředkovatelem základní databáze.</span><span class="sxs-lookup"><span data-stu-id="a7adb-113">Until now, EF Core could only map properties of types natively supported by the underlying database provider.</span></span> <span data-ttu-id="a7adb-114">Hodnoty byly zkopírovány tam a zpět mezi sloupci a vlastnostmi bez jakékoli transformace.</span><span class="sxs-lookup"><span data-stu-id="a7adb-114">Values were copied back and forth between columns and properties without any transformation.</span></span> <span data-ttu-id="a7adb-115">Počínaje EF Core 2.1, převody hodnot lze použít k transformaci hodnoty získané ze sloupců před jejich použitím na vlastnosti a naopak.</span><span class="sxs-lookup"><span data-stu-id="a7adb-115">Starting with EF Core 2.1, value conversions can be applied to transform the values obtained from columns before they are applied to properties, and vice versa.</span></span> <span data-ttu-id="a7adb-116">Máme řadu převodů, které lze použít podle konvence podle potřeby, stejně jako explicitní konfigurace rozhraní API, které umožňuje registraci vlastní převody mezi sloupci a vlastnostmi.</span><span class="sxs-lookup"><span data-stu-id="a7adb-116">We have a number of conversions that can be applied by convention as necessary, as well as an explicit configuration API that allows registering custom conversions between columns and properties.</span></span> <span data-ttu-id="a7adb-117">Některé z použití této funkce jsou:</span><span class="sxs-lookup"><span data-stu-id="a7adb-117">Some of the application of this feature are:</span></span>

- <span data-ttu-id="a7adb-118">Ukládání výčtů jako řetězců</span><span class="sxs-lookup"><span data-stu-id="a7adb-118">Storing enums as strings</span></span>
- <span data-ttu-id="a7adb-119">Mapování nepodepsaných intel s SQL Serverem</span><span class="sxs-lookup"><span data-stu-id="a7adb-119">Mapping unsigned integers with SQL Server</span></span>
- <span data-ttu-id="a7adb-120">Automatické šifrování a dešifrování hodnot vlastností</span><span class="sxs-lookup"><span data-stu-id="a7adb-120">Automatic encryption and decryption of property values</span></span>

<span data-ttu-id="a7adb-121">Další informace o tomto tématu naleznete v [části o převodech hodnot.](xref:core/modeling/value-conversions)</span><span class="sxs-lookup"><span data-stu-id="a7adb-121">Read the [section on value conversions](xref:core/modeling/value-conversions) for more information about this topic.</span></span>  

## <a name="linq-groupby-translation"></a><span data-ttu-id="a7adb-122">LINQ GroupPodle překladu</span><span class="sxs-lookup"><span data-stu-id="a7adb-122">LINQ GroupBy translation</span></span>

<span data-ttu-id="a7adb-123">Před verzí 2.1 v EF Core groupby linq operátor by vždy vyhodnoceny v paměti.</span><span class="sxs-lookup"><span data-stu-id="a7adb-123">Before version 2.1, in EF Core the GroupBy LINQ operator would always be evaluated in memory.</span></span> <span data-ttu-id="a7adb-124">Nyní podporujeme překlad do klauzule SQL GROUP BY ve většině běžných případů.</span><span class="sxs-lookup"><span data-stu-id="a7adb-124">We now support translating it to the SQL GROUP BY clause in most common cases.</span></span>

<span data-ttu-id="a7adb-125">Tento příklad ukazuje dotaz s GroupBy slouží k výpočtu různých agregačních funkcí:</span><span class="sxs-lookup"><span data-stu-id="a7adb-125">This example shows a query with GroupBy used to compute various aggregate functions:</span></span>

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
          Avg = g.Average(o => o.Amount)
        });
```

<span data-ttu-id="a7adb-126">Odpovídající překlad SQL vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="a7adb-126">The corresponding SQL translation looks like this:</span></span>

``` SQL
SELECT [o].[CustomerId], [o].[EmployeeId],
    SUM([o].[Amount]), MIN([o].[Amount]), MAX([o].[Amount]), AVG([o].[Amount])
FROM [Orders] AS [o]
GROUP BY [o].[CustomerId], [o].[EmployeeId];
```

## <a name="data-seeding"></a><span data-ttu-id="a7adb-127">Předvyplnění dat</span><span class="sxs-lookup"><span data-stu-id="a7adb-127">Data Seeding</span></span>

<span data-ttu-id="a7adb-128">S novou verzí bude možné poskytnout počáteční data k naplnění databáze.</span><span class="sxs-lookup"><span data-stu-id="a7adb-128">With the new release it will be possible to provide initial data to populate a database.</span></span> <span data-ttu-id="a7adb-129">Na rozdíl od EF6 jsou data osiva přidružena k typu entity jako součást konfigurace modelu.</span><span class="sxs-lookup"><span data-stu-id="a7adb-129">Unlike in EF6, seeding data is associated to an entity type as part of the model configuration.</span></span> <span data-ttu-id="a7adb-130">Migrace EF Core pak mohou automaticky vypočítat, jaké operace vložení, aktualizace nebo odstranění je třeba použít při upgradu databáze na novou verzi modelu.</span><span class="sxs-lookup"><span data-stu-id="a7adb-130">Then EF Core migrations can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="a7adb-131">Jako příklad můžete použít ke konfiguraci osiva `OnModelCreating`data pro post v :</span><span class="sxs-lookup"><span data-stu-id="a7adb-131">As an example, you can use this to configure seed data for a Post in `OnModelCreating`:</span></span>

``` csharp
modelBuilder.Entity<Post>().HasData(new Post{ Id = 1, Text = "Hello World!" });
```

<span data-ttu-id="a7adb-132">Další informace o tomto tématu naleznete v části o [osivu dat.](xref:core/modeling/data-seeding)</span><span class="sxs-lookup"><span data-stu-id="a7adb-132">Read the [section on data seeding](xref:core/modeling/data-seeding) for more information about this topic.</span></span>  

## <a name="query-types"></a><span data-ttu-id="a7adb-133">Typy dotazů</span><span class="sxs-lookup"><span data-stu-id="a7adb-133">Query types</span></span>

<span data-ttu-id="a7adb-134">Model EF Core nyní může obsahovat typy dotazů.</span><span class="sxs-lookup"><span data-stu-id="a7adb-134">An EF Core model can now include query types.</span></span> <span data-ttu-id="a7adb-135">Na rozdíl od typů entit typy typů nemají klíče definované na nich a nelze vložit, odstranit nebo aktualizovat (to znamená, že jsou jen pro čtení), ale mohou být vráceny přímo dotazy.</span><span class="sxs-lookup"><span data-stu-id="a7adb-135">Unlike entity types, query types do not have keys defined on them and cannot be inserted, deleted or updated (that is, they are read-only), but they can be returned directly by queries.</span></span> <span data-ttu-id="a7adb-136">Některé scénáře použití pro typy dotazů jsou:</span><span class="sxs-lookup"><span data-stu-id="a7adb-136">Some of the usage scenarios for query types are:</span></span>

- <span data-ttu-id="a7adb-137">Mapování na zobrazení bez primárních klíčů</span><span class="sxs-lookup"><span data-stu-id="a7adb-137">Mapping to views without primary keys</span></span>
- <span data-ttu-id="a7adb-138">Mapování na tabulky bez primárních klíčů</span><span class="sxs-lookup"><span data-stu-id="a7adb-138">Mapping to tables without primary keys</span></span>
- <span data-ttu-id="a7adb-139">Mapování na dotazy definované v modelu</span><span class="sxs-lookup"><span data-stu-id="a7adb-139">Mapping to queries defined in the model</span></span>
- <span data-ttu-id="a7adb-140">Slouží jako návratový `FromSql()` typ pro dotazy</span><span class="sxs-lookup"><span data-stu-id="a7adb-140">Serving as the return type for `FromSql()` queries</span></span>

<span data-ttu-id="a7adb-141">Další informace o tomto tématu naleznete v [části o typech dotazů.](xref:core/modeling/keyless-entity-types)</span><span class="sxs-lookup"><span data-stu-id="a7adb-141">Read the [section on query types](xref:core/modeling/keyless-entity-types) for more information about this topic.</span></span>

## <a name="include-for-derived-types"></a><span data-ttu-id="a7adb-142">Zahrnout pro odvozené typy</span><span class="sxs-lookup"><span data-stu-id="a7adb-142">Include for derived types</span></span>

<span data-ttu-id="a7adb-143">Nyní bude možné zadat navigační vlastnosti definované pouze na odvozených `Include` typech při psaní výrazů pro metodu.</span><span class="sxs-lookup"><span data-stu-id="a7adb-143">It will be now possible to specify navigation properties only defined on derived types when writing expressions for the `Include` method.</span></span> <span data-ttu-id="a7adb-144">Pro verzi silného `Include`typu , podporujeme pomocí explicitní `as` přetypovačky nebo operátor.</span><span class="sxs-lookup"><span data-stu-id="a7adb-144">For the strongly typed version of `Include`, we support using either an explicit cast or the `as` operator.</span></span> <span data-ttu-id="a7adb-145">Nyní také podporujeme odkazování na názvy vlastností navigace definovaných na odvozených typech `Include`ve verzi řetězce :</span><span class="sxs-lookup"><span data-stu-id="a7adb-145">We also now support referencing the names of navigation property defined on derived types in the string version of `Include`:</span></span>

``` csharp
var option1 = context.People.Include(p => ((Student)p).School);
var option2 = context.People.Include(p => (p as Student).School);
var option3 = context.People.Include("School");
```

<span data-ttu-id="a7adb-146">Další informace o tomto tématu naleznete v [části Zahrnout s odvozenými typy.](xref:core/querying/related-data#include-on-derived-types)</span><span class="sxs-lookup"><span data-stu-id="a7adb-146">Read the [section on Include with derived types](xref:core/querying/related-data#include-on-derived-types) for more information about this topic.</span></span>

## <a name="systemtransactions-support"></a><span data-ttu-id="a7adb-147">Podpora system.transactions</span><span class="sxs-lookup"><span data-stu-id="a7adb-147">System.Transactions support</span></span>

<span data-ttu-id="a7adb-148">Přidali jsme možnost pracovat s Funkcemi System.Transactions, jako je TransactionScope.</span><span class="sxs-lookup"><span data-stu-id="a7adb-148">We have added the ability to work with System.Transactions features such as TransactionScope.</span></span> <span data-ttu-id="a7adb-149">To bude fungovat na rozhraní .NET Framework a .NET Core při použití poskytovatelů databáze, které ji podporují.</span><span class="sxs-lookup"><span data-stu-id="a7adb-149">This will work on both .NET Framework and .NET Core when using database providers that support it.</span></span>

<span data-ttu-id="a7adb-150">Další informace o tomto tématu naleznete v [části System.Transactions.](xref:core/saving/transactions#using-systemtransactions)</span><span class="sxs-lookup"><span data-stu-id="a7adb-150">Read the [section on System.Transactions](xref:core/saving/transactions#using-systemtransactions) for more information about this topic.</span></span>

## <a name="better-column-ordering-in-initial-migration"></a><span data-ttu-id="a7adb-151">Lepší řazení sloupců při počáteční migraci</span><span class="sxs-lookup"><span data-stu-id="a7adb-151">Better column ordering in initial migration</span></span>

<span data-ttu-id="a7adb-152">Na základě zpětné vazby od zákazníků jsme aktualizovali migrace, abychom zpočátku generovali sloupce pro tabulky ve stejném pořadí jako vlastnosti deklarované ve třídách.</span><span class="sxs-lookup"><span data-stu-id="a7adb-152">Based on customer feedback, we have updated migrations to initially generate columns for tables in the same order as properties are declared in classes.</span></span> <span data-ttu-id="a7adb-153">Všimněte si, že EF Core nelze změnit pořadí při přidání nových členů po vytvoření počáteční tabulky.</span><span class="sxs-lookup"><span data-stu-id="a7adb-153">Note that EF Core cannot change order when new members are added after the initial table creation.</span></span>

## <a name="optimization-of-correlated-subqueries"></a><span data-ttu-id="a7adb-154">Optimalizace korelovaných poddotazů</span><span class="sxs-lookup"><span data-stu-id="a7adb-154">Optimization of correlated subqueries</span></span>

<span data-ttu-id="a7adb-155">Vylepšili jsme překlad dotazů, abychom se vyhnuli provádění dotazů SQL "N + 1" v mnoha běžných scénářích, ve kterých použití navigační vlastnosti v projekci vede k připojení dat z kořenového dotazu s daty z korelovaného poddotazu.</span><span class="sxs-lookup"><span data-stu-id="a7adb-155">We have improved our query translation to avoid executing "N + 1" SQL queries in many common scenarios in which the usage of a navigation property in the projection leads to joining data from the root query with data from a correlated subquery.</span></span> <span data-ttu-id="a7adb-156">Optimalizace vyžaduje ukládání výsledků do vyrovnávací paměti z poddotazu a požadujeme, abyste upravit dotaz opt-in nové chování.</span><span class="sxs-lookup"><span data-stu-id="a7adb-156">The optimization requires buffering the results from the subquery, and we require that you modify the query to opt-in the new behavior.</span></span>

<span data-ttu-id="a7adb-157">Jako příklad následující dotaz obvykle získá přeloženy do jednoho dotazu pro zákazníky plus N (kde "N" je počet zákazníků vrátil) samostatné dotazy pro objednávky:</span><span class="sxs-lookup"><span data-stu-id="a7adb-157">As an example, the following query normally gets translated into one query for Customers, plus N (where "N" is the number of customers returned) separate queries for Orders:</span></span>

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount));
```

<span data-ttu-id="a7adb-158">Zahrnutím `ToList()` na správném místě označíte, že ukládání do vyrovnávací paměti je vhodné pro objednávky, které umožňují optimalizaci:</span><span class="sxs-lookup"><span data-stu-id="a7adb-158">By including `ToList()` in the right place, you indicate that buffering is appropriate for the Orders, which enable the optimization:</span></span>

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount).ToList());
```

<span data-ttu-id="a7adb-159">Všimněte si, že tento dotaz bude přeložen pouze na dva dotazy SQL: Jeden pro zákazníky a další pro objednávky.</span><span class="sxs-lookup"><span data-stu-id="a7adb-159">Note that this query will be translated to only two SQL queries: One for Customers and the next one for Orders.</span></span>

## <a name="owned-attribute"></a><span data-ttu-id="a7adb-160">[Vlastněný] atribut</span><span class="sxs-lookup"><span data-stu-id="a7adb-160">[Owned] attribute</span></span>

<span data-ttu-id="a7adb-161">Nyní je možné nakonfigurovat [typy vlastněných entit](xref:core/modeling/owned-entities) jednoduše `[Owned]` anotací typu a ujistěte se, že entita vlastníka je přidána do modelu:</span><span class="sxs-lookup"><span data-stu-id="a7adb-161">It is now possible to configure [owned entity types](xref:core/modeling/owned-entities) by simply annotating the type with `[Owned]` and then making sure the owner entity is added to the model:</span></span>

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

## <a name="command-line-tool-dotnet-ef-included-in-net-core-sdk"></a><span data-ttu-id="a7adb-162">Nástroj příkazového řádku dotnet-ef obsažený v sadě .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="a7adb-162">Command-line tool dotnet-ef included in .NET Core SDK</span></span>

<span data-ttu-id="a7adb-163">Příkazy _dotnet-ef_ jsou nyní součástí sady .NET Core SDK, proto již nebude nutné použít DotNetCliToolReference v projektu, aby bylo možné použít migrace nebo vytvořit uživatelské rozhraní DbContext z existující databáze.</span><span class="sxs-lookup"><span data-stu-id="a7adb-163">The _dotnet-ef_ commands are now part of the .NET Core SDK, therefore it will no longer be necessary to use DotNetCliToolReference in the project to be able to use migrations or to scaffold a DbContext from an existing database.</span></span>

<span data-ttu-id="a7adb-164">Další podrobnosti o povolení nástrojů příkazového řádku pro různé verze sady .NET Core SDK a EF Core najdete v části [o instalaci nástrojů.](xref:core/miscellaneous/cli/dotnet#installing-the-tools)</span><span class="sxs-lookup"><span data-stu-id="a7adb-164">See the section on [installing the tools](xref:core/miscellaneous/cli/dotnet#installing-the-tools) for more details on how to enable command line tools for different versions of the .NET Core SDK and EF Core.</span></span>

## <a name="microsoftentityframeworkcoreabstractions-package"></a><span data-ttu-id="a7adb-165">Balíček Microsoft.EntityFrameworkCore.Abstractions</span><span class="sxs-lookup"><span data-stu-id="a7adb-165">Microsoft.EntityFrameworkCore.Abstractions package</span></span>

<span data-ttu-id="a7adb-166">Nový balíček obsahuje atributy a rozhraní, které můžete použít ve svých projektech k osvětlení funkcí EF Core bez závislosti na EF Core jako celku.</span><span class="sxs-lookup"><span data-stu-id="a7adb-166">The new package contains attributes and interfaces that you can use in your projects to light up EF Core features without taking a dependency on EF Core as a whole.</span></span> <span data-ttu-id="a7adb-167">Zde jsou například umístěny atribut [Owned] a rozhraní ILazyLoader.</span><span class="sxs-lookup"><span data-stu-id="a7adb-167">For example, the [Owned] attribute and the ILazyLoader interface are located here.</span></span>

## <a name="state-change-events"></a><span data-ttu-id="a7adb-168">Události změny stavu</span><span class="sxs-lookup"><span data-stu-id="a7adb-168">State change events</span></span>

<span data-ttu-id="a7adb-169">New `Tracked` `StateChanged` And `ChangeTracker` události na lze napsat logiku, která reaguje na entity zadání DbContext nebo změny jejich stavu.</span><span class="sxs-lookup"><span data-stu-id="a7adb-169">New `Tracked` And `StateChanged` events on `ChangeTracker` can be used to write logic that reacts to entities entering the DbContext or changing their state.</span></span>

## <a name="raw-sql-parameter-analyzer"></a><span data-ttu-id="a7adb-170">Analyzátor nezpracovaných parametrů SQL</span><span class="sxs-lookup"><span data-stu-id="a7adb-170">Raw SQL parameter analyzer</span></span>

<span data-ttu-id="a7adb-171">Nový analyzátor kódu je součástí EF Core, který detekuje potenciálně nebezpečné použití `FromSql` našich nezpracovaných SQL API, jako je nebo `ExecuteSqlCommand`.</span><span class="sxs-lookup"><span data-stu-id="a7adb-171">A new code analyzer is included with EF Core that detects potentially unsafe usages of our raw-SQL APIs, like `FromSql` or `ExecuteSqlCommand`.</span></span> <span data-ttu-id="a7adb-172">Například pro následující dotaz se zobrazí upozornění, protože _minAge_ není parametrizován:</span><span class="sxs-lookup"><span data-stu-id="a7adb-172">For example, for the following query, you will see a warning because _minAge_ is not parameterized:</span></span>

``` csharp
var sql = $"SELECT * FROM People WHERE Age > {minAge}";
var query = context.People.FromSql(sql);
```

## <a name="database-provider-compatibility"></a><span data-ttu-id="a7adb-173">Kompatibilita zprostředkovatele databáze</span><span class="sxs-lookup"><span data-stu-id="a7adb-173">Database provider compatibility</span></span>

<span data-ttu-id="a7adb-174">Doporučujeme používat EF Core 2.1 s poskytovateli, kteří byli aktualizováni nebo alespoň testováni pro práci s EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="a7adb-174">It is recommended that you use EF Core 2.1 with providers that have been updated or at least tested to work with EF Core 2.1.</span></span>

> [!TIP]
> <span data-ttu-id="a7adb-175">Pokud v nových funkcích zjistíte neočekávanou nekompatibilitu nebo jakýkoli problém nebo máte-li k nim zpětnou vazbu, nahlaste ji pomocí [našeho nástroje pro sledování problémů](https://github.com/aspnet/EntityFrameworkCore/issues/new).</span><span class="sxs-lookup"><span data-stu-id="a7adb-175">If you find any unexpected incompatibility or any issue in the new features, or if you have feedback on them, please report it using [our issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues/new).</span></span>
