---
title: Co je nového v EF Core 2,2-EF Core
author: divega
ms.date: 11/14/2018
ms.assetid: 998C04F3-676A-4FCF-8450-CFB0457B4198
uid: core/what-is-new/ef-core-2.2
ms.openlocfilehash: fb9de799753bebd7b4092cd8f4af74703dee3e45
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417445"
---
# <a name="new-features-in-ef-core-22"></a>Nové funkce v EF Core 2,2

## <a name="spatial-data-support"></a>Podpora prostorových dat

Prostorová data lze použít k reprezentaci fyzického umístění a tvaru objektů.
Mnoho databází může nativně ukládat, indexovat a dotazovat prostorová data.
Mezi běžné scénáře patří dotazování pro objekty v dané vzdálenosti a testování, pokud mnohoúhelník obsahuje dané umístění.
EF Core 2,2 teď podporuje práci s prostorovými daty z různých databází pomocí typů z knihovny [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) (NTS).

Podpora prostorových dat je implementována jako řada balíčků rozšíření specifických pro daného poskytovatele.
Každý z těchto balíčků přispívá k mapováním pro typy a metody NTS a odpovídající prostorové typy a funkce v databázi.
Tato rozšíření poskytovatele jsou nyní k dispozici pro [SQL Server](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite/), [SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite/)a [PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite/) (z [projektu Npgsql](https://www.npgsql.org/)).
Prostorové typy lze použít přímo s [EF Core poskytovatelem v paměti](xref:core/providers/in-memory/index) bez dalších rozšíření.

Po instalaci rozšíření poskytovatele můžete do entit přidat vlastnosti podporovaných typů. Příklad:

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

Pak můžete zachovat entity s prostorovými daty:

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

A můžete spouštět databázové dotazy na základě prostorových dat a operací:

``` csharp
  var nearestFriends =
      (from f in context.Friends
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

Další informace o této funkci naleznete v [dokumentaci prostorových typů](xref:core/modeling/spatial).

## <a name="collections-of-owned-entities"></a>Kolekce vlastněných entit

EF Core 2,0 přidali možnost modelování vlastnictví v přidruženích 1:1.
EF Core 2,2 rozšiřuje vlastnictví na asociace 1: n.
Vlastnictví pomáhá omezit způsob použití entit.

Vlastními entitami například:

- Dá se použít jenom u navigačních vlastností jiných typů entit.
- Jsou automaticky načteny a lze je sledovat pouze pomocí DbContext společně se svým vlastníkem.

V relačních databázích jsou vlastněné kolekce namapovány na samostatné tabulky od vlastníka, stejně jako u běžných přidružení typu 1: n.
V databázích orientovaných na dokumentu ale plánujeme vnořovat vlastněné entity (ve vlastních kolekcích nebo odkazech) v rámci stejného dokumentu jako vlastník.

Funkci můžete použít voláním nového rozhraní API OwnsMany ():

``` csharp
modelBuilder.Entity<Customer>().OwnsMany(c => c.Addresses);
```

Další informace najdete v dokumentaci k [aktualizovaným vlastněným entitám](xref:core/modeling/owned-entities#collections-of-owned-types).

## <a name="query-tags"></a>Značky dotazů

Tato funkce zjednodušuje korelaci dotazů LINQ v kódu s generovanými dotazy SQL zaznamenanými v protokolech.

Chcete-li využít výhod značek dotazů, můžete pomocí nové metody TagWith () opatřit poznámkami dotaz LINQ.
Použití prostorového dotazu z předchozího příkladu:

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith(@"This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

Tento dotaz LINQ vytvoří následující výstup SQL:

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

Další informace najdete v [dokumentaci značek dotazů](xref:core/querying/tags).
