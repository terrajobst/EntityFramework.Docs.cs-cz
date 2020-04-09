---
title: Co je nového v EF Core 2.2 - EF Core
author: divega
ms.date: 11/14/2018
ms.assetid: 998C04F3-676A-4FCF-8450-CFB0457B4198
uid: core/what-is-new/ef-core-2.2
ms.openlocfilehash: fb9de799753bebd7b4092cd8f4af74703dee3e45
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417445"
---
# <a name="new-features-in-ef-core-22"></a>Nové funkce v EF Core 2.2

## <a name="spatial-data-support"></a>Podpora prostorových dat

Prostorová data lze použít k reprezentaci fyzického umístění a tvaru objektů.
Mnoho databází může nativně ukládat, indexovat a dotazovat prostorová data.
Běžné scénáře zahrnují dotazování na objekty v dané vzdálenosti a testování, pokud polygon obsahuje dané umístění.
EF Core 2.2 nyní podporuje práci s prostorovými daty z různých databází pomocí typů z knihovny [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) (NTS).

Podpora prostorových dat je implementována jako řada rozšiřujících balíčků specifických pro zprostředkovatele.
Každý z těchto balíčků přispívá mapování pro typy a metody NTS a odpovídající prostorové typy a funkce v databázi.
Taková rozšíření zprostředkovatele jsou nyní k dispozici pro [SQL Server](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite/), [SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite/)a [PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite/) (z [projektu Npgsql).](https://www.npgsql.org/)
Prostorové typy lze použít přímo s [poskytovatelem EF Core v paměti](xref:core/providers/in-memory/index) bez dalších rozšíření.

Po instalaci rozšíření zprostředkovatele můžete do entik přidat vlastnosti podporovaných typů. Příklad:

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

Potom můžete zachovat entity s prostorovými daty:

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

A můžete provádět databázové dotazy založené na prostorových datech a operacích:

``` csharp
  var nearestFriends =
      (from f in context.Friends
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

Další informace o této funkci naleznete v [dokumentaci prostorových typů](xref:core/modeling/spatial).

## <a name="collections-of-owned-entities"></a>Sbírky vlastněných subjektů

EF Core 2.0 přidal možnost modelovat vlastnictví v one-to-one přidružení.
EF Core 2.2 rozšiřuje možnost vyjádřit vlastnictví na 1:N sdružení.
Vlastnictví pomáhá omezit způsob použití entit.

Například vlastněné entity:

- Může se někdy zobrazit pouze v navigačních vlastnostech jiných typů entit.
- Jsou automaticky načteny a mohou být sledovány pouze DbContext vedle jejich vlastníka.

V relačních databázích jsou vlastněné kolekce mapovány na samostatné tabulky od vlastníka, stejně jako běžná přidružení 1:N.
Ale v databázích orientovaných na dokumenty plánujeme vnořit vlastněné entity (ve vlastněných sbírkách nebo odkazech) do stejného dokumentu jako vlastník.

Tuto funkci můžete použít voláním nového rozhraní API OwnsMany() :

``` csharp
modelBuilder.Entity<Customer>().OwnsMany(c => c.Addresses);
```

Další informace naleznete v [aktualizované dokumentaci k vlastněným entitám](xref:core/modeling/owned-entities#collections-of-owned-types).

## <a name="query-tags"></a>Značky dotazu

Tato funkce zjednodušuje korelaci linq dotazů v kódu s generovanými dotazy SQL zachycenými v protokolech.

Chcete-li využít výhod značek dotazu, označte dotaz LINQ pomocí metody new TagWith().
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

Další informace naleznete v dokumentaci k [značkám dotazu](xref:core/querying/tags).
