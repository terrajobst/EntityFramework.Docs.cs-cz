---
title: Novinky v EF Core 2.2 – EF Core
author: divega
ms.date: 11/14/2018
ms.assetid: 998C04F3-676A-4FCF-8450-CFB0457B4198
uid: core/what-is-new/ef-core-2.2
ms.openlocfilehash: 79b4efc3aee23e19a9ea1deb6373b9984b77f886
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688800"
---
# <a name="new-features-in-ef-core-22"></a><span data-ttu-id="828ab-102">Novinky v EF Core 2.2</span><span class="sxs-lookup"><span data-stu-id="828ab-102">New features in EF Core 2.2</span></span>

## <a name="spatial-data-support"></a><span data-ttu-id="828ab-103">Podpora prostorových dat</span><span class="sxs-lookup"><span data-stu-id="828ab-103">Spatial data support</span></span>

<span data-ttu-id="828ab-104">Prostorová data slouží k reprezentaci fyzické umístění a tvar objektů.</span><span class="sxs-lookup"><span data-stu-id="828ab-104">Spatial data can be used to represent the physical location and shape of objects.</span></span>
<span data-ttu-id="828ab-105">Velký počet databází můžete nativně ukládání, indexování a dotazovat prostorová data.</span><span class="sxs-lookup"><span data-stu-id="828ab-105">Many databases can natively store, index, and query spatial data.</span></span> <span data-ttu-id="828ab-106">Běžné scénáře zahrnují zadávání dotazů na objekty v daném vzdálenosti a testování, pokud je na dané místo obsahuje mnohoúhelníku.</span><span class="sxs-lookup"><span data-stu-id="828ab-106">Common scenarios include querying for objects within a given distance, and testing if a polygon contains a given location.</span></span>
<span data-ttu-id="828ab-107">EF Core 2.2 teď podporuje práci s prostorovými daty z různých databází pomocí typů z [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) knihovny (NTS).</span><span class="sxs-lookup"><span data-stu-id="828ab-107">EF Core 2.2 now supports working with spatial data from various databases using types from the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) (NTS) library.</span></span>

<span data-ttu-id="828ab-108">Podpora prostorových dat je implementovaný jako řadu balíčky rozšíření specifické pro zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="828ab-108">Spatial data support is implemented as a series of provider-specific extension packages.</span></span>
<span data-ttu-id="828ab-109">Každá z těchto balíčků přispívá mapování pro typy chny Zarážky a metody a odpovídající prostorové typy a funkce v databázi.</span><span class="sxs-lookup"><span data-stu-id="828ab-109">Each of these packages contributes mappings for NTS types and methods, and the corresponding spatial types and functions in the database.</span></span>
<span data-ttu-id="828ab-110">Taková rozšíření zprostředkovatele jsou teď dostupné pro [systému SQL Server](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite/), [SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite/), a [PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite/) (z [Npgsql projektu](http://www.npgsql.org/)).</span><span class="sxs-lookup"><span data-stu-id="828ab-110">Such provider extensions are now available for [SQL Server](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite/), [SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite/), and [PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite/) (from the [Npgsql project](http://www.npgsql.org/)).</span></span>
<span data-ttu-id="828ab-111">Prostorové typy lze použít přímo s [zprostředkovatele v paměti EF Core](https://docs.microsoft.com/en-us/ef/core/providers/in-memory/) bez další rozšíření.</span><span class="sxs-lookup"><span data-stu-id="828ab-111">Spatial types can be used directly with the [EF Core in-memory provider](https://docs.microsoft.com/en-us/ef/core/providers/in-memory/) without additional extensions.</span></span>

<span data-ttu-id="828ab-112">Po instalaci poskytovatele rozšíření můžete přidat vlastnosti podporovaných typů pro entity.</span><span class="sxs-lookup"><span data-stu-id="828ab-112">Once the provider extension is installed, you can add properties of supported types to your entities.</span></span> <span data-ttu-id="828ab-113">Příklad:</span><span class="sxs-lookup"><span data-stu-id="828ab-113">For example:</span></span>

``` csharp
using NetTopologySuite.Geometries;

namespace MyApp
{
  public class Friend
  {
    [Key]
    public string Name { get; set; }
  
    [Required]
    public Point Location { get; set; }
  }
}
``` 

<span data-ttu-id="828ab-114">Potom můžete zachovat trvale entity s prostorovými daty formátu:</span><span class="sxs-lookup"><span data-stu-id="828ab-114">You can then persist entities with spatial data:</span></span>

``` csharp
using (var context = new MyDbContext())
{
    context.Add(
        new Friend
        {
            Name = "Bill",
            Location = new Point(-122.34877, 47.6233355) {SRID = 4326 }
        });
    context.SaveChanges();
}
```
<span data-ttu-id="828ab-115">A můžete spustit na základě prostorová data a operace databázové dotazy:</span><span class="sxs-lookup"><span data-stu-id="828ab-115">And you can execute database queries based on spatial data and operations:</span></span>

``` csharp
  var nearestFriends =
      (from f in context.Friends
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

<span data-ttu-id="828ab-116">Další informace o této funkci najdete v tématu [prostorové typy dokumentaci](xref:core/modeling/spatial).</span><span class="sxs-lookup"><span data-stu-id="828ab-116">For more information on this feature, see the [spatial types documentation](xref:core/modeling/spatial).</span></span> 

## <a name="collections-of-owned-entities"></a><span data-ttu-id="828ab-117">Kolekce entit vlastněná podnikem</span><span class="sxs-lookup"><span data-stu-id="828ab-117">Collections of owned entities</span></span>

<span data-ttu-id="828ab-118">EF Core 2.0 přidali možnost modelu vlastnictví v 1: 1 přidružení.</span><span class="sxs-lookup"><span data-stu-id="828ab-118">EF Core 2.0 added the ability to model ownership in one-to-one associations.</span></span>
<span data-ttu-id="828ab-119">EF Core 2.2 rozšiřuje možnosti express vlastnictví na jednoho k několika přidružení.</span><span class="sxs-lookup"><span data-stu-id="828ab-119">EF Core 2.2 extends the ability to express ownership to one-to-many associations.</span></span>
<span data-ttu-id="828ab-120">Vlastnictví pomáhá omezit, jak se používají entity.</span><span class="sxs-lookup"><span data-stu-id="828ab-120">Ownership helps constrain how entities are used.</span></span>

<span data-ttu-id="828ab-121">Například vlastní entity:</span><span class="sxs-lookup"><span data-stu-id="828ab-121">For example, owned entities:</span></span>
- <span data-ttu-id="828ab-122">Může objevit pouze v navigační vlastnosti z ostatních typů entit.</span><span class="sxs-lookup"><span data-stu-id="828ab-122">Can only ever appear on navigation properties of other entity types.</span></span> 
- <span data-ttu-id="828ab-123">Jsou automaticky načteny a lze sledovat pouze DbContext spolu s jejich vlastníka.</span><span class="sxs-lookup"><span data-stu-id="828ab-123">Are automatically loaded, and can only be tracked by a DbContext alongside their owner.</span></span>

<span data-ttu-id="828ab-124">V relačních databázích vlastní kolekce se mapují k oddělení od vlastníka, stejně jako regulární přidružení jednoho k několika tabulek.</span><span class="sxs-lookup"><span data-stu-id="828ab-124">In relational databases, owned collections are mapped to separate tables from the owner, just like regular one-to-many associations.</span></span>
<span data-ttu-id="828ab-125">Ale v dokumentu orientovaného databázích, plánujeme vnořit vlastnictví entity (ve vlastnictví kolekce nebo odkazů) ve stejném dokumentu jako vlastník.</span><span class="sxs-lookup"><span data-stu-id="828ab-125">But in document-oriented databases, we plan to nest owned entities (in owned collections or references) within the same document as the owner.</span></span>

<span data-ttu-id="828ab-126">Po zavolání nové rozhraní API OwnsMany() můžete použít funkci:</span><span class="sxs-lookup"><span data-stu-id="828ab-126">You can use the feature by calling the new OwnsMany() API:</span></span>

``` csharp
modelBuilder.Entity<Customer>().OwnsMany(c => c.Addresses);
```

<span data-ttu-id="828ab-127">Další informace najdete v tématu [aktualizovat vlastnictví entity dokumentaci](xref:core/modeling/owned-entities#collections-of-owned-types).</span><span class="sxs-lookup"><span data-stu-id="828ab-127">For more information, see the [updated owned entities documentation](xref:core/modeling/owned-entities#collections-of-owned-types).</span></span>

## <a name="query-tags"></a><span data-ttu-id="828ab-128">Značky dotazu</span><span class="sxs-lookup"><span data-stu-id="828ab-128">Query tags</span></span>

<span data-ttu-id="828ab-129">Tato funkce usnadňuje korelace dotazů LINQ v kódu pomocí generovaného dotazů SQL, které jsou zachyceny v protokolech.</span><span class="sxs-lookup"><span data-stu-id="828ab-129">This feature simplifies the correlation of LINQ queries in code with generated SQL queries captured in logs.</span></span>

<span data-ttu-id="828ab-130">Využijte výhod značky dotazu, přidávat poznámky k dotazu LINQ pomocí nové metody TagWith().</span><span class="sxs-lookup"><span data-stu-id="828ab-130">To take advantage of query tags, you annotate a LINQ query using the new TagWith() method.</span></span>
<span data-ttu-id="828ab-131">Pomocí prostorových dotazů z předchozího příkladu:</span><span class="sxs-lookup"><span data-stu-id="828ab-131">Using the spatial query from a previous example:</span></span>

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith(@"This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

<span data-ttu-id="828ab-132">Tento dotaz LINQ vytvoří následující výstup SQL:</span><span class="sxs-lookup"><span data-stu-id="828ab-132">This LINQ query will produce the following SQL output:</span></span>

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="828ab-133">Další informace najdete v tématu [dotazu značky dokumentace](xref:core/querying/tags).</span><span class="sxs-lookup"><span data-stu-id="828ab-133">For more information, see the [query tags documentation](xref:core/querying/tags).</span></span> 