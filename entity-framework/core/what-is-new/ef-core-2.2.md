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
# <a name="new-features-in-ef-core-22"></a>Novinky v EF Core 2.2

## <a name="spatial-data-support"></a>Podpora prostorových dat

Prostorová data slouží k reprezentaci fyzické umístění a tvar objektů.
Velký počet databází můžete nativně ukládání, indexování a dotazovat prostorová data. Běžné scénáře zahrnují zadávání dotazů na objekty v daném vzdálenosti a testování, pokud je na dané místo obsahuje mnohoúhelníku.
EF Core 2.2 teď podporuje práci s prostorovými daty z různých databází pomocí typů z [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) knihovny (NTS).

Podpora prostorových dat je implementovaný jako řadu balíčky rozšíření specifické pro zprostředkovatele.
Každá z těchto balíčků přispívá mapování pro typy chny Zarážky a metody a odpovídající prostorové typy a funkce v databázi.
Taková rozšíření zprostředkovatele jsou teď dostupné pro [systému SQL Server](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite/), [SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite/), a [PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite/) (z [Npgsql projektu](http://www.npgsql.org/)).
Prostorové typy lze použít přímo s [zprostředkovatele v paměti EF Core](https://docs.microsoft.com/en-us/ef/core/providers/in-memory/) bez další rozšíření.

Po instalaci poskytovatele rozšíření můžete přidat vlastnosti podporovaných typů pro entity. Příklad:

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

Potom můžete zachovat trvale entity s prostorovými daty formátu:

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
A můžete spustit na základě prostorová data a operace databázové dotazy:

``` csharp
  var nearestFriends =
      (from f in context.Friends
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

Další informace o této funkci najdete v tématu [prostorové typy dokumentaci](xref:core/modeling/spatial). 

## <a name="collections-of-owned-entities"></a>Kolekce entit vlastněná podnikem

EF Core 2.0 přidali možnost modelu vlastnictví v 1: 1 přidružení.
EF Core 2.2 rozšiřuje možnosti express vlastnictví na jednoho k několika přidružení.
Vlastnictví pomáhá omezit, jak se používají entity.

Například vlastní entity:
- Může objevit pouze v navigační vlastnosti z ostatních typů entit. 
- Jsou automaticky načteny a lze sledovat pouze DbContext spolu s jejich vlastníka.

V relačních databázích vlastní kolekce se mapují k oddělení od vlastníka, stejně jako regulární přidružení jednoho k několika tabulek.
Ale v dokumentu orientovaného databázích, plánujeme vnořit vlastnictví entity (ve vlastnictví kolekce nebo odkazů) ve stejném dokumentu jako vlastník.

Po zavolání nové rozhraní API OwnsMany() můžete použít funkci:

``` csharp
modelBuilder.Entity<Customer>().OwnsMany(c => c.Addresses);
```

Další informace najdete v tématu [aktualizovat vlastnictví entity dokumentaci](xref:core/modeling/owned-entities#collections-of-owned-types).

## <a name="query-tags"></a>Značky dotazu

Tato funkce usnadňuje korelace dotazů LINQ v kódu pomocí generovaného dotazů SQL, které jsou zachyceny v protokolech.

Využijte výhod značky dotazu, přidávat poznámky k dotazu LINQ pomocí nové metody TagWith().
Pomocí prostorových dotazů z předchozího příkladu:

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

Další informace najdete v tématu [dotazu značky dokumentace](xref:core/querying/tags). 