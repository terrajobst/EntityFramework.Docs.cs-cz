---
title: "Co je nového v EF základní 2.1 - EF jádra"
author: divega
ms.author: divega
ms.date: 2/20/2018
ms.assetid: 585F90A3-4D5A-4DD1-92D8-5243B14E0FEC
ms.technology: entity-framework-core
uid: core/what-is-new/ef-core-2.1
ms.openlocfilehash: bb1e691e0f22bd36467d58c02bde22c63067207e
ms.sourcegitcommit: fcaeaf085171dfb5c080ec42df1d1df8dfe204fb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
# <a name="new-features-in-ef-core-21"></a><span data-ttu-id="898c7-102">Nové funkce v EF základní 2.1</span><span class="sxs-lookup"><span data-stu-id="898c7-102">New features in EF Core 2.1</span></span>
> [!NOTE]  
> <span data-ttu-id="898c7-103">Tato verze je stále ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="898c7-103">This release is still in preview.</span></span>

<span data-ttu-id="898c7-104">Kromě malé množství vylepšení a opravy chyb pro více než 100 produktu EF základní 2.1 obsahuje několik nových funkcí:</span><span class="sxs-lookup"><span data-stu-id="898c7-104">Besides numerous small improvements and more than a hundred product bug fixes, EF Core 2.1 includes several new features:</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="898c7-105">opožděného načítání</span><span class="sxs-lookup"><span data-stu-id="898c7-105">Lazy loading</span></span>
<span data-ttu-id="898c7-106">Základní EF nyní obsahuje stavební bloky potřebné pro každý, kdo k vytváření tříd entit, které můžete načíst jejich navigační vlastnosti na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="898c7-106">EF Core now contains the necessary building blocks for anyone to author entity classes that can load their navigation properties on demand.</span></span> <span data-ttu-id="898c7-107">Také jsme vytvořili nový balíček, Microsoft.EntityFrameworkCore.Proxies, která využívá tyto stavební bloky k vytvoření opožděného načítání proxy třídy na základě minimálně upravit tříd entit (např. tříd pomocí virtuální navigační vlastnosti).</span><span class="sxs-lookup"><span data-stu-id="898c7-107">We have also created a new package, Microsoft.EntityFrameworkCore.Proxies, that leverages those building blocks to produce lazy loading proxy classes based on minimally modified entity classes (e.g. classes with virtual navigation properties).</span></span>

<span data-ttu-id="898c7-108">Pro čtení [část opožděného načítání](xref:core/querying/related-data#lazy-loading) Další informace o tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="898c7-108">Read the [section on lazy loading](xref:core/querying/related-data#lazy-loading) for more information about this topic.</span></span>

## <a name="parameters-in-entity-constructors"></a><span data-ttu-id="898c7-109">Parametry v konstruktorech entity</span><span class="sxs-lookup"><span data-stu-id="898c7-109">Parameters in entity constructors</span></span>
<span data-ttu-id="898c7-110">Jako jeden z požadovaných stavební bloky pro opožděného načítání jsme povoleno vytváření entit, které vzít parametry v jejich konstruktory.</span><span class="sxs-lookup"><span data-stu-id="898c7-110">As one of the required building blocks for lazy loading, we enabled the creation of entities that take parameters in their constructors.</span></span> <span data-ttu-id="898c7-111">Parametry můžete vložit hodnoty vlastností, opožděného načítání Delegáti a služby.</span><span class="sxs-lookup"><span data-stu-id="898c7-111">You can use parameters to inject property values, lazy loading delegates, and services.</span></span>

<span data-ttu-id="898c7-112">Pro čtení [části na entity konstruktor s parametry](xref:core/modeling/constructors) Další informace o tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="898c7-112">Read the [section on entity constructor with parameters](xref:core/modeling/constructors) for more information about this topic.</span></span>

## <a name="value-conversions"></a><span data-ttu-id="898c7-113">Převody hodnot</span><span class="sxs-lookup"><span data-stu-id="898c7-113">Value conversions</span></span>
<span data-ttu-id="898c7-114">Až do této chvíle může EF základní mapují jenom vlastnosti typů nativně podporuje základní zprostředkovatel databáze.</span><span class="sxs-lookup"><span data-stu-id="898c7-114">Until now, EF Core could only map properties of types natively supported by the underlying database provider.</span></span> <span data-ttu-id="898c7-115">Hodnoty byly zkopírovány přepínat mezi sloupci a vlastnosti bez jakékoli transformace.</span><span class="sxs-lookup"><span data-stu-id="898c7-115">Values were copied back and forth between columns and properties without any transformation.</span></span> <span data-ttu-id="898c7-116">Počínaje EF základní 2.1, převody hodnot můžete použít k transformaci hodnot zjištěných z sloupců, než je použijete k vlastnosti a naopak.</span><span class="sxs-lookup"><span data-stu-id="898c7-116">Starting with EF Core 2.1, value conversions can be applied to transform the values obtained from columns before they are applied to properties, and vice versa.</span></span> <span data-ttu-id="898c7-117">Máme několik převody, které mohou být použity podle konvence podle potřeby, jakož i rozhraní API explicitní konfigurace, který umožňuje registraci vlastní převody mezi sloupci a vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="898c7-117">We have a number of conversions that can be applied by convention as necessary, as well as an explicit configuration API that allows registering custom conversions between columns and properties.</span></span> <span data-ttu-id="898c7-118">Některé aplikace této funkce jsou:</span><span class="sxs-lookup"><span data-stu-id="898c7-118">Some of the application of this feature are:</span></span>

- <span data-ttu-id="898c7-119">Ukládání výčty jako řetězce</span><span class="sxs-lookup"><span data-stu-id="898c7-119">Storing enums as strings</span></span>
- <span data-ttu-id="898c7-120">Mapování nepodepsané celých čísel s SQL serverem</span><span class="sxs-lookup"><span data-stu-id="898c7-120">Mapping unsigned integers with SQL Server</span></span>
- <span data-ttu-id="898c7-121">Automatické šifrování a dešifrování hodnot vlastností</span><span class="sxs-lookup"><span data-stu-id="898c7-121">Automatic encryption and decryption of property values</span></span>

<span data-ttu-id="898c7-122">Pro čtení [část převody hodnot](xref:core/modeling/value-conversions) Další informace o tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="898c7-122">Read the [section on value conversions](xref:core/modeling/value-conversions) for more information about this topic.</span></span>  

## <a name="linq-groupby-translation"></a><span data-ttu-id="898c7-123">Překlad LINQ GroupBy</span><span class="sxs-lookup"><span data-stu-id="898c7-123">LINQ GroupBy translation</span></span>
<span data-ttu-id="898c7-124">Před vyhodnocením verze 2.1, které jsou v základní EF operátor GroupBy LINQ byl vždy v paměti.</span><span class="sxs-lookup"><span data-stu-id="898c7-124">Before version 2.1, in EF Core the GroupBy LINQ operator was always be evaluated in memory.</span></span> <span data-ttu-id="898c7-125">Teď podporují překladu klauzule SQL GROUP BY ve nejběžnější případy.</span><span class="sxs-lookup"><span data-stu-id="898c7-125">We now support translating it to the SQL GROUP BY clause in most common cases.</span></span>

<span data-ttu-id="898c7-126">Tento příklad ukazuje dotaz s GroupBy slouží k výpočtu různé agregační funkce:</span><span class="sxs-lookup"><span data-stu-id="898c7-126">This example shows a query with GroupBy used to compute various aggregate functions:</span></span>

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

<span data-ttu-id="898c7-127">Odpovídající překlad SQL vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="898c7-127">The corresponding SQL translation looks like this:</span></span>

``` SQL
SELECT [o].[CustomerId], [o].[EmployeeId],
    SUM([o].[Amount]), MIN([o].[Amount]), MAX([o].[Amount]), AVG([o].[Amount])
FROM [Orders] AS [o]
GROUP BY [o].[CustomerId], [o].[EmployeeId];
```

## <a name="data-seeding"></a><span data-ttu-id="898c7-128">Data synchronizace replik indexů</span><span class="sxs-lookup"><span data-stu-id="898c7-128">Data Seeding</span></span>
<span data-ttu-id="898c7-129">S novou verzí je možné zajistit počáteční data k naplnění databáze.</span><span class="sxs-lookup"><span data-stu-id="898c7-129">With the new release it will be possible to provide initial data to populate a database.</span></span> <span data-ttu-id="898c7-130">Na rozdíl od v EF6, synchronizace replik indexů dat je přidružena k typu entity jako součást konfigurace modelu.</span><span class="sxs-lookup"><span data-stu-id="898c7-130">Unlike in EF6, seeding data is associated to an entity type as part of the model configuration.</span></span> <span data-ttu-id="898c7-131">Potom základní EF migrace můžete vypočítat automaticky, co vložit, aktualizovat nebo odstranit potřeba operations použijí při upgradu databáze na novou verzi modelu.</span><span class="sxs-lookup"><span data-stu-id="898c7-131">Then EF Core migrations can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="898c7-132">Jako příklad, můžete to konfigurovat počáteční data pro metodu Post v `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="898c7-132">As an example, you can use this to configure seed data for a Post in `OnModelCreating`:</span></span>

``` csharp
modelBuilder.Entity<Post>().SeedData(new Post{ Id = 1, Text = "Hello World!" });
```

<span data-ttu-id="898c7-133">Pro čtení [část dat synchronizace replik indexů](xref:core/modeling/data-seeding) Další informace o tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="898c7-133">Read the [section on data seeding](xref:core/modeling/data-seeding) for more information about this topic.</span></span>  

## <a name="query-types"></a><span data-ttu-id="898c7-134">Typy dotazů</span><span class="sxs-lookup"><span data-stu-id="898c7-134">Query types</span></span>
<span data-ttu-id="898c7-135">Model EF základní můžete nyní zahrnují typy dotazů.</span><span class="sxs-lookup"><span data-stu-id="898c7-135">An EF Core model can now include query types.</span></span> <span data-ttu-id="898c7-136">Na rozdíl od typy entit, typy dotazů není mít definované na nich klíče a nelze vložit, odstranit nebo aktualizovat (tj. jsou jen pro čtení), ale mohou být vráceny přímo pomocí dotazů.</span><span class="sxs-lookup"><span data-stu-id="898c7-136">Unlike entity types, query types do not have keys defined on them and cannot be inserted, deleted or updated (i.e. they are read-only), but they can be returned directly by queries.</span></span> <span data-ttu-id="898c7-137">Některé scénáře použití pro typy dotazů jsou:</span><span class="sxs-lookup"><span data-stu-id="898c7-137">Some of the usage scenarios for query types are:</span></span>

- <span data-ttu-id="898c7-138">Mapování na zobrazení bez primárních klíčů</span><span class="sxs-lookup"><span data-stu-id="898c7-138">Mapping to views without primary keys</span></span>
- <span data-ttu-id="898c7-139">Mapování tabulek bez primárních klíčů</span><span class="sxs-lookup"><span data-stu-id="898c7-139">Mapping to tables without primary keys</span></span>
- <span data-ttu-id="898c7-140">Mapování na dotazy, které jsou definované v modelu</span><span class="sxs-lookup"><span data-stu-id="898c7-140">Mapping to queries defined in the model</span></span>
- <span data-ttu-id="898c7-141">Slouží jako návratový typ pro `FromSql()` dotazy</span><span class="sxs-lookup"><span data-stu-id="898c7-141">Serving as the return type for `FromSql()` queries</span></span>

<span data-ttu-id="898c7-142">Pro čtení [části na typy dotazů](xref:core/modeling/query-types) Další informace o tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="898c7-142">Read the [section on query types](xref:core/modeling/query-types) for more information about this topic.</span></span>

## <a name="include-for-derived-types"></a><span data-ttu-id="898c7-143">Zahrnout pro odvozené typy</span><span class="sxs-lookup"><span data-stu-id="898c7-143">Include for derived types</span></span>
<span data-ttu-id="898c7-144">Je teď možné zadat definována pouze na navigační vlastnosti odvozených typů při zápisu výrazy pro `Include` metoda.</span><span class="sxs-lookup"><span data-stu-id="898c7-144">It will be now possible to specify navigation properties only defined on derived types when writing expressions for the `Include` method.</span></span> <span data-ttu-id="898c7-145">Pro verzi silného typu `Include`, podporujeme pomocí explicitní přetypování nebo `as` operátor.</span><span class="sxs-lookup"><span data-stu-id="898c7-145">For the strongly typed version of `Include`, we support using either an explicit cast or the `as` operator.</span></span> <span data-ttu-id="898c7-146">Nyní také podporují odkazování na názvy navigační vlastnost definovanou u odvozených typů v řetězec verze `Include`:</span><span class="sxs-lookup"><span data-stu-id="898c7-146">We also now support referencing the names of navigation property defined on derived types in the string version of `Include`:</span></span>

``` csharp
var option1 = context.People.Include(p => ((Student)p).School);
var option2 = context.People.Include(p => (p as Student).School);
var option3 = context.People.Include("School");
```

<span data-ttu-id="898c7-147">Pro čtení [části na zahrnout u odvozených typů](xref:core/querying/related-data#include-on-derived-types) Další informace o tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="898c7-147">Read the [section on Include with derived types](xref:core/querying/related-data#include-on-derived-types) for more information about this topic.</span></span>

## <a name="systemtransactions-support"></a><span data-ttu-id="898c7-148">System.Transactions – podpora</span><span class="sxs-lookup"><span data-stu-id="898c7-148">System.Transactions support</span></span>
<span data-ttu-id="898c7-149">Jsme přidali schopnost pracovat s funkcemi System.Transactions – například TransactionScope.</span><span class="sxs-lookup"><span data-stu-id="898c7-149">We have added the ability to work with System.Transactions features such as TransactionScope.</span></span> <span data-ttu-id="898c7-150">To bude fungovat na rozhraní .NET Framework a .NET Core, při použití zprostředkovatele databáze, které ji podporují.</span><span class="sxs-lookup"><span data-stu-id="898c7-150">This will work on both .NET Framework and .NET Core when using database providers that support it.</span></span>

<span data-ttu-id="898c7-151">Pro čtení [část System.Transactions –](xref:core/saving/transactions#using-systemtransactions) Další informace o tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="898c7-151">Read the [section on System.Transactions](xref:core/saving/transactions#using-systemtransactions) for more information about this topic.</span></span>

## <a name="better-column-ordering-in-initial-migration"></a><span data-ttu-id="898c7-152">Lepší pořadí sloupců v počáteční migrace</span><span class="sxs-lookup"><span data-stu-id="898c7-152">Better column ordering in initial migration</span></span>
<span data-ttu-id="898c7-153">Na základě názorů zákazníků, jsme jste aktualizovali migrace na začátku generovat sloupce pro tabulky ve stejném pořadí jako vlastnosti jsou deklarované v třídách.</span><span class="sxs-lookup"><span data-stu-id="898c7-153">Based on customer feedback, we have updated migrations to initially generate columns for tables in the same order as properties are declared in classes.</span></span> <span data-ttu-id="898c7-154">Všimněte si, že EF základní nelze změnit pořadí při přidání nové členy po vytvoření počáteční tabulky.</span><span class="sxs-lookup"><span data-stu-id="898c7-154">Note that EF Core cannot change order when new members are added after the initial table creation.</span></span>

## <a name="optimization-of-correlated-subqueries"></a><span data-ttu-id="898c7-155">Optimalizace korelační poddotazy</span><span class="sxs-lookup"><span data-stu-id="898c7-155">Optimization of correlated subqueries</span></span>
<span data-ttu-id="898c7-156">Jsme vylepšili naše překlad dotazu, aby se zabránilo spouštění "N + 1" dotazy SQL v mnoha běžným scénářům, ve kterých využití navigační vlastnosti v projekci vede k spojování dat z dotazu kořenové s daty z korelační poddotaz.</span><span class="sxs-lookup"><span data-stu-id="898c7-156">We have improved our query translation to avoid executing "N + 1" SQL queries in many common scenarios in which the usage of a navigation property in the projection leads to joining data from the root query with data from a correlated subquery.</span></span> <span data-ttu-id="898c7-157">Optimalizace vyžaduje ukládání do vyrovnávací paměti výsledky tvoří poddotaz a je nutné upravit dotaz tak, aby výslovný souhlas nové chování.</span><span class="sxs-lookup"><span data-stu-id="898c7-157">The optimization requires buffering the results form the subquery, and we require that you modify the query to opt-in the new behavior.</span></span>

<span data-ttu-id="898c7-158">Jako příklad následující dotaz normálně získá přeložit na jeden dotaz pro zákazníky, plus N (kde "N" je počet zákazníků vrátil) oddělit dotazy pro objednávky:</span><span class="sxs-lookup"><span data-stu-id="898c7-158">As an example, the following query normally gets translated into one query for Customers, plus N (where "N" is the number of customers returned) separate queries for Orders:</span></span>

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount));
```

<span data-ttu-id="898c7-159">Zahrnutím `ToList()` na správném místě znamenat, že ukládání do vyrovnávací paměti je vhodné pro objednávky, které umožňují optimalizace:</span><span class="sxs-lookup"><span data-stu-id="898c7-159">By including `ToList()` in the right place, you indicate that buffering is appropriate for the Orders, which enable the optimization:</span></span>

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount).ToList());
```

<span data-ttu-id="898c7-160">Všimněte si, že tento dotaz bude možné přeložit pouze dva dotazy SQL: jeden pro zákazníky a dalším pro objednávky.</span><span class="sxs-lookup"><span data-stu-id="898c7-160">Note that this query will be translated to only two SQL queries: One for Customers and the next one for Orders.</span></span>

## <a name="ownedattribute"></a><span data-ttu-id="898c7-161">OwnedAttribute</span><span class="sxs-lookup"><span data-stu-id="898c7-161">OwnedAttribute</span></span>

<span data-ttu-id="898c7-162">Nyní je možné nakonfigurovat [vlastní typy entit](xref:core/modeling/owned-entities) podle jednoduše zadávání poznámek k na typ s `[Owned]` a pak zajistit, owner entity přidají do modelu:</span><span class="sxs-lookup"><span data-stu-id="898c7-162">It is now possible to configure [owned entity types](xref:core/modeling/owned-entities) by simply annotating the type with `[Owned]` and then making sure the owner entity is added to the model:</span></span>

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

## <a name="database-provider-compatibility"></a><span data-ttu-id="898c7-163">Zprostředkovatel kompatibility databáze</span><span class="sxs-lookup"><span data-stu-id="898c7-163">Database provider compatibility</span></span>

<span data-ttu-id="898c7-164">Základní EF 2.1 byla navržená tak, aby byl kompatibilní s poskytovateli databáze vytvořené pro EF základní 2.0.</span><span class="sxs-lookup"><span data-stu-id="898c7-164">EF Core 2.1 was designed to be compatible with database providers created for EF Core 2.0.</span></span> <span data-ttu-id="898c7-165">Zatímco některé funkce popsané výše (např. převody hodnot) vyžadují poskytovatele aktualizované, ostatní (např. opožděného načítání) bude light zprostředkovatelům existující.</span><span class="sxs-lookup"><span data-stu-id="898c7-165">While some of the features described above (e.g. value conversions) require an updated provider, others (e.g. lazy loading) will light up with existing providers.</span></span>

> [!TIP]
> <span data-ttu-id="898c7-166">Pokud zjistíte, že jsou všechny neočekávané nekompatibilita žádný problém nové funkce nebo pokud máte zpětnou vazbu na, nahlaste jej pomocí [náš sledovací modul problém](https://github.com/aspnet/EntityFrameworkCore/issues/new).</span><span class="sxs-lookup"><span data-stu-id="898c7-166">If you find any unexpected incompatibility or any issue in the new features, or if you have feedback on them, please report it using [our issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues/new).</span></span>
