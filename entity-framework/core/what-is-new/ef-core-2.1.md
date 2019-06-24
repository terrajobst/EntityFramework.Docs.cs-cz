---
title: Novinky v EF Core 2.1 – EF Core
author: divega
ms.date: 02/20/2018
ms.assetid: 585F90A3-4D5A-4DD1-92D8-5243B14E0FEC
uid: core/what-is-new/ef-core-2.1
ms.openlocfilehash: 16600ccbb1194d584fae15671118d9c046f1f637
ms.sourcegitcommit: 06073f8efde97dd5f540dbfb69f574d8380566fe
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/24/2019
ms.locfileid: "67333860"
---
# <a name="new-features-in-ef-core-21"></a><span data-ttu-id="db1d9-102">Novinky v EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="db1d9-102">New features in EF Core 2.1</span></span>

<span data-ttu-id="db1d9-103">Kromě řadu oprav chyb a malé vylepšení funkčnosti a výkonu EF Core 2.1 obsahuje některé zajímavé nové funkce:</span><span class="sxs-lookup"><span data-stu-id="db1d9-103">Besides numerous bug fixes and small functional and performance enhancements, EF Core 2.1 includes some compelling new features:</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="db1d9-104">Opožděné načtení</span><span class="sxs-lookup"><span data-stu-id="db1d9-104">Lazy loading</span></span>
<span data-ttu-id="db1d9-105">EF Core teď obsahuje stavební bloky potřebné pro každého, kdo k vytváření tříd entit, které můžete načíst jejich vlastnosti navigace na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="db1d9-105">EF Core now contains the necessary building blocks for anyone to author entity classes that can load their navigation properties on demand.</span></span> <span data-ttu-id="db1d9-106">Vytvořili jsme také nový balíček, Microsoft.EntityFrameworkCore.Proxies, která využívá tyto stavební bloky k vytvoření proxy opožděné načtení třídy na základě minimálně upravit tříd entit (například třídy s virtuální navigační vlastnosti).</span><span class="sxs-lookup"><span data-stu-id="db1d9-106">We have also created a new package, Microsoft.EntityFrameworkCore.Proxies, that leverages those building blocks to produce lazy loading proxy classes based on minimally modified entity classes (for example, classes with virtual navigation properties).</span></span>

<span data-ttu-id="db1d9-107">Přečtěte si [věnované opožděné načtení](xref:core/querying/related-data#lazy-loading) Další informace o tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="db1d9-107">Read the [section on lazy loading](xref:core/querying/related-data#lazy-loading) for more information about this topic.</span></span>

## <a name="parameters-in-entity-constructors"></a><span data-ttu-id="db1d9-108">Parametry v konstruktorech entity</span><span class="sxs-lookup"><span data-stu-id="db1d9-108">Parameters in entity constructors</span></span>
<span data-ttu-id="db1d9-109">Jako jeden z požadovaných stavební bloky pro opožděné načtení jsme povolili vytvoření entity, které přijímají parametry v jejich konstruktory.</span><span class="sxs-lookup"><span data-stu-id="db1d9-109">As one of the required building blocks for lazy loading, we enabled the creation of entities that take parameters in their constructors.</span></span> <span data-ttu-id="db1d9-110">Parametry můžete použít k vložení hodnoty vlastností, opožděné načtení delegátů a služeb.</span><span class="sxs-lookup"><span data-stu-id="db1d9-110">You can use parameters to inject property values, lazy loading delegates, and services.</span></span>

<span data-ttu-id="db1d9-111">Čtení [věnované entity konstruktor s parametry](xref:core/modeling/constructors) Další informace o tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="db1d9-111">Read the [section on entity constructor with parameters](xref:core/modeling/constructors) for more information about this topic.</span></span>

## <a name="value-conversions"></a><span data-ttu-id="db1d9-112">Převody hodnot</span><span class="sxs-lookup"><span data-stu-id="db1d9-112">Value conversions</span></span>
<span data-ttu-id="db1d9-113">Až doteď EF Core namapovat pouze vlastnosti typů nativně podporuje základní zprostředkovatel databáze.</span><span class="sxs-lookup"><span data-stu-id="db1d9-113">Until now, EF Core could only map properties of types natively supported by the underlying database provider.</span></span> <span data-ttu-id="db1d9-114">Hodnoty byly zkopírovány vpřed a zpět mezi sloupci a vlastnosti bez jakékoli transformace.</span><span class="sxs-lookup"><span data-stu-id="db1d9-114">Values were copied back and forth between columns and properties without any transformation.</span></span> <span data-ttu-id="db1d9-115">Od verze EF Core 2.1, převody hodnot lze použít k transformaci hodnoty získané ze sloupců, než tato nastavení použijí do vlastností a naopak.</span><span class="sxs-lookup"><span data-stu-id="db1d9-115">Starting with EF Core 2.1, value conversions can be applied to transform the values obtained from columns before they are applied to properties, and vice versa.</span></span> <span data-ttu-id="db1d9-116">Máme několik převodů, které se dají nastavit podle konvence, podle potřeby, jakož i rozhraní API explicitní konfigurace, které umožňuje zaregistrovat vlastní převody mezi sloupci a vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="db1d9-116">We have a number of conversions that can be applied by convention as necessary, as well as an explicit configuration API that allows registering custom conversions between columns and properties.</span></span> <span data-ttu-id="db1d9-117">Zde jsou některé aplikace tuto funkci:</span><span class="sxs-lookup"><span data-stu-id="db1d9-117">Some of the application of this feature are:</span></span>

- <span data-ttu-id="db1d9-118">Ukládání výčty jako řetězce</span><span class="sxs-lookup"><span data-stu-id="db1d9-118">Storing enums as strings</span></span>
- <span data-ttu-id="db1d9-119">Mapování typu unsigned integer s SQL serverem</span><span class="sxs-lookup"><span data-stu-id="db1d9-119">Mapping unsigned integers with SQL Server</span></span>
- <span data-ttu-id="db1d9-120">Automatické šifrování a dešifrování hodnot vlastností</span><span class="sxs-lookup"><span data-stu-id="db1d9-120">Automatic encryption and decryption of property values</span></span>

<span data-ttu-id="db1d9-121">Přečtěte si [věnované převody hodnot](xref:core/modeling/value-conversions) Další informace o tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="db1d9-121">Read the [section on value conversions](xref:core/modeling/value-conversions) for more information about this topic.</span></span>  

## <a name="linq-groupby-translation"></a><span data-ttu-id="db1d9-122">LINQ GroupBy translation</span><span class="sxs-lookup"><span data-stu-id="db1d9-122">LINQ GroupBy translation</span></span>
<span data-ttu-id="db1d9-123">Dříve než ve verzi 2.1 v EF Core – operátor GroupBy LINQ vždy vyhodnocována v paměti.</span><span class="sxs-lookup"><span data-stu-id="db1d9-123">Before version 2.1, in EF Core the GroupBy LINQ operator would always be evaluated in memory.</span></span> <span data-ttu-id="db1d9-124">Nyní podporujeme překladu klauzule GROUP BY jazyka SQL ve nejběžnější případy.</span><span class="sxs-lookup"><span data-stu-id="db1d9-124">We now support translating it to the SQL GROUP BY clause in most common cases.</span></span>

<span data-ttu-id="db1d9-125">Tento příklad ukazuje dotaz s GroupBy slouží k výpočtu různých agregační funkce:</span><span class="sxs-lookup"><span data-stu-id="db1d9-125">This example shows a query with GroupBy used to compute various aggregate functions:</span></span>

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

<span data-ttu-id="db1d9-126">Odpovídající překlad SQL vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="db1d9-126">The corresponding SQL translation looks like this:</span></span>

``` SQL
SELECT [o].[CustomerId], [o].[EmployeeId],
    SUM([o].[Amount]), MIN([o].[Amount]), MAX([o].[Amount]), AVG([o].[Amount])
FROM [Orders] AS [o]
GROUP BY [o].[CustomerId], [o].[EmployeeId];
```

## <a name="data-seeding"></a><span data-ttu-id="db1d9-127">Předvyplnění dat</span><span class="sxs-lookup"><span data-stu-id="db1d9-127">Data Seeding</span></span>
<span data-ttu-id="db1d9-128">V nové verzi je možné poskytnout počátečních dat k naplnění databáze.</span><span class="sxs-lookup"><span data-stu-id="db1d9-128">With the new release it will be possible to provide initial data to populate a database.</span></span> <span data-ttu-id="db1d9-129">Na rozdíl od v EF6, předvyplnění dat je přidružena k typu entity jako součást konfigurace modelu.</span><span class="sxs-lookup"><span data-stu-id="db1d9-129">Unlike in EF6, seeding data is associated to an entity type as part of the model configuration.</span></span> <span data-ttu-id="db1d9-130">Pak můžete automaticky výpočetní migrace EF Core co vložit, aktualizovat nebo odstranit operací nutné použít při upgradu databáze na novou verzi modelu.</span><span class="sxs-lookup"><span data-stu-id="db1d9-130">Then EF Core migrations can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="db1d9-131">Jako příklad, můžete nakonfigurovat počáteční data pro příspěvek ve `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="db1d9-131">As an example, you can use this to configure seed data for a Post in `OnModelCreating`:</span></span>

``` csharp
modelBuilder.Entity<Post>().HasData(new Post{ Id = 1, Text = "Hello World!" });
```

<span data-ttu-id="db1d9-132">Čtení [věnované předvyplnění dat](xref:core/modeling/data-seeding) Další informace o tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="db1d9-132">Read the [section on data seeding](xref:core/modeling/data-seeding) for more information about this topic.</span></span>  

## <a name="query-types"></a><span data-ttu-id="db1d9-133">Typy dotazů</span><span class="sxs-lookup"><span data-stu-id="db1d9-133">Query types</span></span>
<span data-ttu-id="db1d9-134">Model, pomocí EF Core teď může obsahovat typy dotazů.</span><span class="sxs-lookup"><span data-stu-id="db1d9-134">An EF Core model can now include query types.</span></span> <span data-ttu-id="db1d9-135">Na rozdíl od typy entit, typy dotazů není definována zasílají klíče mají a nelze vložit, odstranit nebo aktualizovat (to znamená, že jsou jen pro čtení), ale mohou být vráceny přímo pomocí dotazů.</span><span class="sxs-lookup"><span data-stu-id="db1d9-135">Unlike entity types, query types do not have keys defined on them and cannot be inserted, deleted or updated (that is, they are read-only), but they can be returned directly by queries.</span></span> <span data-ttu-id="db1d9-136">Mezi scénáře použití pro typy dotazů, patří:</span><span class="sxs-lookup"><span data-stu-id="db1d9-136">Some of the usage scenarios for query types are:</span></span>

- <span data-ttu-id="db1d9-137">Mapování na zobrazení bez primárních klíčů</span><span class="sxs-lookup"><span data-stu-id="db1d9-137">Mapping to views without primary keys</span></span>
- <span data-ttu-id="db1d9-138">Mapování tabulek bez primárních klíčů</span><span class="sxs-lookup"><span data-stu-id="db1d9-138">Mapping to tables without primary keys</span></span>
- <span data-ttu-id="db1d9-139">Mapování na dotazy definované v modelu</span><span class="sxs-lookup"><span data-stu-id="db1d9-139">Mapping to queries defined in the model</span></span>
- <span data-ttu-id="db1d9-140">Slouží jako návratový typ pro `FromSql()` dotazy</span><span class="sxs-lookup"><span data-stu-id="db1d9-140">Serving as the return type for `FromSql()` queries</span></span>

<span data-ttu-id="db1d9-141">Čtení [části na typy dotazů](xref:core/modeling/query-types) Další informace o tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="db1d9-141">Read the [section on query types](xref:core/modeling/query-types) for more information about this topic.</span></span>

## <a name="include-for-derived-types"></a><span data-ttu-id="db1d9-142">Zahrnout pro odvozené typy</span><span class="sxs-lookup"><span data-stu-id="db1d9-142">Include for derived types</span></span>
<span data-ttu-id="db1d9-143">Bude nyní specifikovat navigační vlastnosti definována pouze na odvozené typy při psaní výrazů pro `Include` metody.</span><span class="sxs-lookup"><span data-stu-id="db1d9-143">It will be now possible to specify navigation properties only defined on derived types when writing expressions for the `Include` method.</span></span> <span data-ttu-id="db1d9-144">Pro verzi silného typu `Include`, podporujeme používání explicitní přetypování nebo `as` operátor.</span><span class="sxs-lookup"><span data-stu-id="db1d9-144">For the strongly typed version of `Include`, we support using either an explicit cast or the `as` operator.</span></span> <span data-ttu-id="db1d9-145">Také teď podporují odkazování na název navigační vlastnosti definované v odvozených typů ve verzi řetězec `Include`:</span><span class="sxs-lookup"><span data-stu-id="db1d9-145">We also now support referencing the names of navigation property defined on derived types in the string version of `Include`:</span></span>

``` csharp
var option1 = context.People.Include(p => ((Student)p).School);
var option2 = context.People.Include(p => (p as Student).School);
var option3 = context.People.Include("School");
```

<span data-ttu-id="db1d9-146">Čtení [věnované zahrnout u odvozených typů](xref:core/querying/related-data#include-on-derived-types) Další informace o tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="db1d9-146">Read the [section on Include with derived types](xref:core/querying/related-data#include-on-derived-types) for more information about this topic.</span></span>

## <a name="systemtransactions-support"></a><span data-ttu-id="db1d9-147">Podpora System.Transactions</span><span class="sxs-lookup"><span data-stu-id="db1d9-147">System.Transactions support</span></span>
<span data-ttu-id="db1d9-148">Přidali jsme možnost pro práci s System.Transactions funkce, jako je objekt TransactionScope.</span><span class="sxs-lookup"><span data-stu-id="db1d9-148">We have added the ability to work with System.Transactions features such as TransactionScope.</span></span> <span data-ttu-id="db1d9-149">To bude fungovat na rozhraní .NET Framework a .NET Core, při použití poskytovatelé databází, které ho podporují.</span><span class="sxs-lookup"><span data-stu-id="db1d9-149">This will work on both .NET Framework and .NET Core when using database providers that support it.</span></span>

<span data-ttu-id="db1d9-150">Čtení [věnované System.Transactions](xref:core/saving/transactions#using-systemtransactions) Další informace o tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="db1d9-150">Read the [section on System.Transactions](xref:core/saving/transactions#using-systemtransactions) for more information about this topic.</span></span>

## <a name="better-column-ordering-in-initial-migration"></a><span data-ttu-id="db1d9-151">Lepší pořadí sloupců v počáteční migraci</span><span class="sxs-lookup"><span data-stu-id="db1d9-151">Better column ordering in initial migration</span></span>
<span data-ttu-id="db1d9-152">Na základě názorů zákazníků, aktualizovali jsme migrace se zpočátku vygenerovat sloupce tabulky ve stejném pořadí, protože vlastnosti jsou deklarovány ve třídách.</span><span class="sxs-lookup"><span data-stu-id="db1d9-152">Based on customer feedback, we have updated migrations to initially generate columns for tables in the same order as properties are declared in classes.</span></span> <span data-ttu-id="db1d9-153">Všimněte si, že EF Core nelze změnit pořadí při přidání nových členů po vytvoření počátečního tabulky.</span><span class="sxs-lookup"><span data-stu-id="db1d9-153">Note that EF Core cannot change order when new members are added after the initial table creation.</span></span>

## <a name="optimization-of-correlated-subqueries"></a><span data-ttu-id="db1d9-154">Optimalizace korelační poddotazů</span><span class="sxs-lookup"><span data-stu-id="db1d9-154">Optimization of correlated subqueries</span></span>
<span data-ttu-id="db1d9-155">Vylepšili jsme naše překladu dotazu, aby se zabránilo spouštění "N + 1" dotazy SQL v mnoha běžných scénářů, ve kterých využití vlastnost navigace v projekci vede k spojování dat z dotazu kořenové s daty z korelační poddotazu.</span><span class="sxs-lookup"><span data-stu-id="db1d9-155">We have improved our query translation to avoid executing "N + 1" SQL queries in many common scenarios in which the usage of a navigation property in the projection leads to joining data from the root query with data from a correlated subquery.</span></span> <span data-ttu-id="db1d9-156">Optimalizace vyžaduje ukládání do vyrovnávací paměti výsledky poddotazu a je potřeba se, že změníte dotaz k vyjádření výslovného souhlasu nové chování.</span><span class="sxs-lookup"><span data-stu-id="db1d9-156">The optimization requires buffering the results from the subquery, and we require that you modify the query to opt-in the new behavior.</span></span>

<span data-ttu-id="db1d9-157">Například následující dotaz obvykle převedeny do jednoho dotazu pro zákazníky, a navíc N (kde "N" je počet zákazníků vrátil) samostatné dotazy pro objednávky:</span><span class="sxs-lookup"><span data-stu-id="db1d9-157">As an example, the following query normally gets translated into one query for Customers, plus N (where "N" is the number of customers returned) separate queries for Orders:</span></span>

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount));
```

<span data-ttu-id="db1d9-158">Zahrnutím `ToList()` na správném místě, určujete, že ukládání do vyrovnávací paměti je vhodný pro objednávky, které umožňují optimalizace:</span><span class="sxs-lookup"><span data-stu-id="db1d9-158">By including `ToList()` in the right place, you indicate that buffering is appropriate for the Orders, which enable the optimization:</span></span>

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount).ToList());
```

<span data-ttu-id="db1d9-159">Všimněte si, že tento dotaz se přeložit na pouze dva dotazy SQL: Jeden pro zákazníky a další příkaz za objednávky.</span><span class="sxs-lookup"><span data-stu-id="db1d9-159">Note that this query will be translated to only two SQL queries: One for Customers and the next one for Orders.</span></span>

## <a name="owned-attribute"></a><span data-ttu-id="db1d9-160">Atribut [vlastní]</span><span class="sxs-lookup"><span data-stu-id="db1d9-160">[Owned] attribute</span></span>

<span data-ttu-id="db1d9-161">Nyní je možné nakonfigurovat [vlastněné typy entit](xref:core/modeling/owned-entities) jednoduše anotací typu s `[Owned]` a pak zajistit, owner entity se přidá do modelu:</span><span class="sxs-lookup"><span data-stu-id="db1d9-161">It is now possible to configure [owned entity types](xref:core/modeling/owned-entities) by simply annotating the type with `[Owned]` and then making sure the owner entity is added to the model:</span></span>

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

## <a name="command-line-tool-dotnet-ef-included-in-net-core-sdk"></a><span data-ttu-id="db1d9-162">Nástroj příkazového řádku dotnet-ef zahrnutý v sadě SDK .NET Core</span><span class="sxs-lookup"><span data-stu-id="db1d9-162">Command-line tool dotnet-ef included in .NET Core SDK</span></span>

<span data-ttu-id="db1d9-163">_Dotnet ef_ příkazy jsou teď součástí sady .NET Core SDK, proto už bude nutné použít DotNetCliToolReference v projektu mohli používat migrace nebo scaffold DbContext z existující databáze.</span><span class="sxs-lookup"><span data-stu-id="db1d9-163">The _dotnet-ef_ commands are now part of the .NET Core SDK, therefore it will no longer be necessary to use DotNetCliToolReference in the project to be able to use migrations or to scaffold a DbContext from an existing database.</span></span>

<span data-ttu-id="db1d9-164">Naleznete v části [instalace nástrojů](xref:core/miscellaneous/cli/dotnet#installing-the-tools) podrobné informace o tom, jak povolit nástroje příkazového řádku pro různé verze sady SDK .NET Core a EF Core.</span><span class="sxs-lookup"><span data-stu-id="db1d9-164">See the section on [installing the tools](xref:core/miscellaneous/cli/dotnet#installing-the-tools) for more details on how to enable command line tools for different versions of the .NET Core SDK and EF Core.</span></span>

## <a name="microsoftentityframeworkcoreabstractions-package"></a><span data-ttu-id="db1d9-165">Microsoft.EntityFrameworkCore.Abstractions balíčku</span><span class="sxs-lookup"><span data-stu-id="db1d9-165">Microsoft.EntityFrameworkCore.Abstractions package</span></span>
<span data-ttu-id="db1d9-166">Nový balíček obsahuje atributy a rozhraní, které můžete použít ve vašich projektech, aby bylo možné EF Core funkce bez nutnosti přepínat závislost na EF Core jako celek.</span><span class="sxs-lookup"><span data-stu-id="db1d9-166">The new package contains attributes and interfaces that you can use in your projects to light up EF Core features without taking a dependency on EF Core as a whole.</span></span> <span data-ttu-id="db1d9-167">Například atribut [vlastněno] a ILazyLoader interface jsou umístěny zde.</span><span class="sxs-lookup"><span data-stu-id="db1d9-167">For example, the [Owned] attribute and the ILazyLoader interface are located here.</span></span>

## <a name="state-change-events"></a><span data-ttu-id="db1d9-168">Události změny stavu</span><span class="sxs-lookup"><span data-stu-id="db1d9-168">State change events</span></span>

<span data-ttu-id="db1d9-169">Nové `Tracked` a `StateChanged` události na `ChangeTracker` můžete použít pro zapsání logiku, která reaguje na entity zadávání uvolněn objekt DbContext nebo změnit jejich stav.</span><span class="sxs-lookup"><span data-stu-id="db1d9-169">New `Tracked` And `StateChanged` events on `ChangeTracker` can be used to write logic that reacts to entities entering the DbContext or changing their state.</span></span>

## <a name="raw-sql-parameter-analyzer"></a><span data-ttu-id="db1d9-170">Nezpracovaná analyzátoru parametr SQL</span><span class="sxs-lookup"><span data-stu-id="db1d9-170">Raw SQL parameter analyzer</span></span>

<span data-ttu-id="db1d9-171">Nový analyzátor kódu je součástí EF Core, který zjistí potenciálně nebezpečná použití rozhraní API nezpracovaná SQL, jako je třeba `FromSql` nebo `ExecuteSqlCommand`.</span><span class="sxs-lookup"><span data-stu-id="db1d9-171">A new code analyzer is included with EF Core that detects potentially unsafe usages of our raw-SQL APIs, like `FromSql` or `ExecuteSqlCommand`.</span></span> <span data-ttu-id="db1d9-172">Například následující dotaz, zobrazí se upozornění protože _minAge_ není s parametry:</span><span class="sxs-lookup"><span data-stu-id="db1d9-172">For example, for the following query, you will see a warning because _minAge_ is not parameterized:</span></span>

``` csharp
var sql = $"SELECT * FROM People WHERE Age > {minAge}";
var query = context.People.FromSql(sql);
```

## <a name="database-provider-compatibility"></a><span data-ttu-id="db1d9-173">Zprostředkovatel kompatibility databáze</span><span class="sxs-lookup"><span data-stu-id="db1d9-173">Database provider compatibility</span></span>

<span data-ttu-id="db1d9-174">Doporučuje se, že používáte EF Core 2.1 s poskytovateli, které byly aktualizovány nebo alespoň testovat pro práci s EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="db1d9-174">It is recommended that you use EF Core 2.1 with providers that have been updated or at least tested to work with EF Core 2.1.</span></span>

> [!TIP]
> <span data-ttu-id="db1d9-175">Pokud zjistíte všechny neočekávané nekompatibility ani žádnou vydat v nových funkcí nebo pokud máte nějakou zpětnou vazbu, oznamte ji pomocí [naše sledování problémů](https://github.com/aspnet/EntityFrameworkCore/issues/new).</span><span class="sxs-lookup"><span data-stu-id="db1d9-175">If you find any unexpected incompatibility or any issue in the new features, or if you have feedback on them, please report it using [our issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues/new).</span></span>
