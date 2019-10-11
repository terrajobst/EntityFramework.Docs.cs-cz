---
title: Co je nového v EF Core 2,2-EF Core
author: divega
ms.date: 11/14/2018
ms.assetid: 998C04F3-676A-4FCF-8450-CFB0457B4198
uid: core/what-is-new/ef-core-2.2
ms.openlocfilehash: 5fcf7c6dfb4d8cb7928ef974af6deb52df7c63eb
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181378"
---
# <a name="new-features-in-ef-core-22"></a><span data-ttu-id="d2fe0-102">Nové funkce v EF Core 2,2</span><span class="sxs-lookup"><span data-stu-id="d2fe0-102">New features in EF Core 2.2</span></span>

## <a name="spatial-data-support"></a><span data-ttu-id="d2fe0-103">Podpora prostorových dat</span><span class="sxs-lookup"><span data-stu-id="d2fe0-103">Spatial data support</span></span>

<span data-ttu-id="d2fe0-104">Prostorová data lze použít k reprezentaci fyzického umístění a tvaru objektů.</span><span class="sxs-lookup"><span data-stu-id="d2fe0-104">Spatial data can be used to represent the physical location and shape of objects.</span></span>
<span data-ttu-id="d2fe0-105">Mnoho databází může nativně ukládat, indexovat a dotazovat prostorová data.</span><span class="sxs-lookup"><span data-stu-id="d2fe0-105">Many databases can natively store, index, and query spatial data.</span></span> <span data-ttu-id="d2fe0-106">Mezi běžné scénáře patří dotazování pro objekty v dané vzdálenosti a testování, pokud mnohoúhelník obsahuje dané umístění.</span><span class="sxs-lookup"><span data-stu-id="d2fe0-106">Common scenarios include querying for objects within a given distance, and testing if a polygon contains a given location.</span></span>
<span data-ttu-id="d2fe0-107">EF Core 2,2 teď podporuje práci s prostorovými daty z různých databází pomocí typů z knihovny [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) (NTS).</span><span class="sxs-lookup"><span data-stu-id="d2fe0-107">EF Core 2.2 now supports working with spatial data from various databases using types from the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) (NTS) library.</span></span>

<span data-ttu-id="d2fe0-108">Podpora prostorových dat je implementována jako řada balíčků rozšíření specifických pro daného poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="d2fe0-108">Spatial data support is implemented as a series of provider-specific extension packages.</span></span>
<span data-ttu-id="d2fe0-109">Každý z těchto balíčků přispívá k mapováním pro typy a metody NTS a odpovídající prostorové typy a funkce v databázi.</span><span class="sxs-lookup"><span data-stu-id="d2fe0-109">Each of these packages contributes mappings for NTS types and methods, and the corresponding spatial types and functions in the database.</span></span>
<span data-ttu-id="d2fe0-110">Tato rozšíření poskytovatele jsou nyní k dispozici pro [SQL Server](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite/), [SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite/)a [PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite/) (z [projektu Npgsql](https://www.npgsql.org/)).</span><span class="sxs-lookup"><span data-stu-id="d2fe0-110">Such provider extensions are now available for [SQL Server](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite/), [SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite/), and [PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite/) (from the [Npgsql project](https://www.npgsql.org/)).</span></span>
<span data-ttu-id="d2fe0-111">Prostorové typy lze použít přímo s [EF Core poskytovatelem v paměti](https://docs.microsoft.com/en-us/ef/core/providers/in-memory/) bez dalších rozšíření.</span><span class="sxs-lookup"><span data-stu-id="d2fe0-111">Spatial types can be used directly with the [EF Core in-memory provider](https://docs.microsoft.com/en-us/ef/core/providers/in-memory/) without additional extensions.</span></span>

<span data-ttu-id="d2fe0-112">Po instalaci rozšíření poskytovatele můžete do entit přidat vlastnosti podporovaných typů.</span><span class="sxs-lookup"><span data-stu-id="d2fe0-112">Once the provider extension is installed, you can add properties of supported types to your entities.</span></span> <span data-ttu-id="d2fe0-113">Příklad:</span><span class="sxs-lookup"><span data-stu-id="d2fe0-113">For example:</span></span>

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

<span data-ttu-id="d2fe0-114">Pak můžete zachovat entity s prostorovými daty:</span><span class="sxs-lookup"><span data-stu-id="d2fe0-114">You can then persist entities with spatial data:</span></span>

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
<span data-ttu-id="d2fe0-115">A můžete spouštět databázové dotazy na základě prostorových dat a operací:</span><span class="sxs-lookup"><span data-stu-id="d2fe0-115">And you can execute database queries based on spatial data and operations:</span></span>

``` csharp
  var nearestFriends =
      (from f in context.Friends
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

<span data-ttu-id="d2fe0-116">Další informace o této funkci naleznete v [dokumentaci prostorových typů](xref:core/modeling/spatial).</span><span class="sxs-lookup"><span data-stu-id="d2fe0-116">For more information on this feature, see the [spatial types documentation](xref:core/modeling/spatial).</span></span> 

## <a name="collections-of-owned-entities"></a><span data-ttu-id="d2fe0-117">Kolekce vlastněných entit</span><span class="sxs-lookup"><span data-stu-id="d2fe0-117">Collections of owned entities</span></span>

<span data-ttu-id="d2fe0-118">EF Core 2,0 přidali možnost modelování vlastnictví v přidruženích 1:1.</span><span class="sxs-lookup"><span data-stu-id="d2fe0-118">EF Core 2.0 added the ability to model ownership in one-to-one associations.</span></span>
<span data-ttu-id="d2fe0-119">EF Core 2,2 rozšiřuje vlastnictví na asociace 1: n.</span><span class="sxs-lookup"><span data-stu-id="d2fe0-119">EF Core 2.2 extends the ability to express ownership to one-to-many associations.</span></span>
<span data-ttu-id="d2fe0-120">Vlastnictví pomáhá omezit způsob použití entit.</span><span class="sxs-lookup"><span data-stu-id="d2fe0-120">Ownership helps constrain how entities are used.</span></span>

<span data-ttu-id="d2fe0-121">Vlastními entitami například:</span><span class="sxs-lookup"><span data-stu-id="d2fe0-121">For example, owned entities:</span></span>
- <span data-ttu-id="d2fe0-122">Dá se použít jenom u navigačních vlastností jiných typů entit.</span><span class="sxs-lookup"><span data-stu-id="d2fe0-122">Can only ever appear on navigation properties of other entity types.</span></span> 
- <span data-ttu-id="d2fe0-123">Jsou automaticky načteny a lze je sledovat pouze pomocí DbContext společně se svým vlastníkem.</span><span class="sxs-lookup"><span data-stu-id="d2fe0-123">Are automatically loaded, and can only be tracked by a DbContext alongside their owner.</span></span>

<span data-ttu-id="d2fe0-124">V relačních databázích jsou vlastněné kolekce namapovány na samostatné tabulky od vlastníka, stejně jako u běžných přidružení typu 1: n.</span><span class="sxs-lookup"><span data-stu-id="d2fe0-124">In relational databases, owned collections are mapped to separate tables from the owner, just like regular one-to-many associations.</span></span>
<span data-ttu-id="d2fe0-125">V databázích orientovaných na dokumentu ale plánujeme vnořovat vlastněné entity (ve vlastních kolekcích nebo odkazech) v rámci stejného dokumentu jako vlastník.</span><span class="sxs-lookup"><span data-stu-id="d2fe0-125">But in document-oriented databases, we plan to nest owned entities (in owned collections or references) within the same document as the owner.</span></span>

<span data-ttu-id="d2fe0-126">Funkci můžete použít voláním nového rozhraní API OwnsMany ():</span><span class="sxs-lookup"><span data-stu-id="d2fe0-126">You can use the feature by calling the new OwnsMany() API:</span></span>

``` csharp
modelBuilder.Entity<Customer>().OwnsMany(c => c.Addresses);
```

<span data-ttu-id="d2fe0-127">Další informace najdete v dokumentaci k [aktualizovaným vlastněným entitám](xref:core/modeling/owned-entities#collections-of-owned-types).</span><span class="sxs-lookup"><span data-stu-id="d2fe0-127">For more information, see the [updated owned entities documentation](xref:core/modeling/owned-entities#collections-of-owned-types).</span></span>

## <a name="query-tags"></a><span data-ttu-id="d2fe0-128">Značky dotazů</span><span class="sxs-lookup"><span data-stu-id="d2fe0-128">Query tags</span></span>

<span data-ttu-id="d2fe0-129">Tato funkce zjednodušuje korelaci dotazů LINQ v kódu s generovanými dotazy SQL zaznamenanými v protokolech.</span><span class="sxs-lookup"><span data-stu-id="d2fe0-129">This feature simplifies the correlation of LINQ queries in code with generated SQL queries captured in logs.</span></span>

<span data-ttu-id="d2fe0-130">Chcete-li využít výhod značek dotazů, můžete pomocí nové metody TagWith () opatřit poznámkami dotaz LINQ.</span><span class="sxs-lookup"><span data-stu-id="d2fe0-130">To take advantage of query tags, you annotate a LINQ query using the new TagWith() method.</span></span>
<span data-ttu-id="d2fe0-131">Použití prostorového dotazu z předchozího příkladu:</span><span class="sxs-lookup"><span data-stu-id="d2fe0-131">Using the spatial query from a previous example:</span></span>

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith(@"This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

<span data-ttu-id="d2fe0-132">Tento dotaz LINQ vytvoří následující výstup SQL:</span><span class="sxs-lookup"><span data-stu-id="d2fe0-132">This LINQ query will produce the following SQL output:</span></span>

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="d2fe0-133">Další informace najdete v [dokumentaci značek dotazů](xref:core/querying/tags).</span><span class="sxs-lookup"><span data-stu-id="d2fe0-133">For more information, see the [query tags documentation](xref:core/querying/tags).</span></span> 