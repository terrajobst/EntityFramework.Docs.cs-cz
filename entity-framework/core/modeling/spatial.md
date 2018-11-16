---
title: Prostorových dat – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: 49c18758af2f2383ea082ead2f6df4c022152b36
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688802"
---
# <a name="spatial-data"></a>Prostorová Data

> [!NOTE]
> Tato funkce je nového v EF Core 2.2.

Prostorová data představuje fyzické umístění a tvar objektů. Mnoho databází poskytují podporu pro tento typ dat je možné indexovat a dotazovat společně s dalšími daty. Běžné scénáře zahrnují zadávání dotazů na objekty v rámci dané vzdálenost od umístění, nebo jeho výběru objektu, jehož ohraničení obsahuje na dané místo. EF Core podporuje mapování pro typy prostorových dat pomocí [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) prostorových knihovny.

## <a name="installing"></a>Instalace

Chcete-li použít prostorová data s EF Core, musíte nainstalovat odpovídající podpůrný balíček NuGet. Balíčky, které je potřeba nainstalovat závisí na poskytovateli, které používáte.

EF Core poskytovatele                        | Prostorový balíček NuGet
--------------------------------------- | ---------------------
Microsoft.EntityFrameworkCore.SqlServer | [Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
Microsoft.EntityFrameworkCore.Sqlite    | [Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
Microsoft.EntityFrameworkCore.InMemory  | [NetTopologySuite](https://www.nuget.org/packages/NetTopologySuite)
Npgsql.EntityFrameworkCore.PostgreSQL   | [Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a>Zpětná analýza

Prostorový NuGet balíčky také umožňují [zpětná analýza](../managing-schemas/scaffolding.md) modely s prostorových vlastnosti, ale je potřeba nainstalovat balíček ***před*** systémem `Scaffold-DbContext` nebo `dotnet ef dbcontext scaffold`. Pokud to neuděláte, zobrazí se upozornění o nenašli mapování typů pro sloupce a sloupce, které se přeskočí.

## <a name="nettopologysuite-nts"></a>NetTopologySuite (NTS)

NetTopologySuite je prostorových knihovna pro .NET. EF Core umožňuje mapování prostorová data typů v databázi s použitím typů chny Zarážky ve vašem modelu.

Pokud chcete povolit mapování pro typy prostorových prostřednictvím chny Zarážky, volání metody UseNetTopologySuite na poskytovatele DbContext možnosti Tvůrce. Například s SQL serverem by zavoláte ji následujícím způsobem.

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

Existuje několik typů prostorová data. Jaký typ použijete závisí na typy tvary, které chcete povolit. Tady je hierarchie typů chny Zarážky, které můžete použít pro vlastnosti v modelu. Jsou umístěny v rámci `NetTopologySuite.Geometries` oboru názvů. Odpovídající rozhraní v balíčku GeoAPI (`GeoAPI.Geometries` oboru názvů) je také možné.

* Geometrie
  * Bod
  * LineString
  * Mnohoúhelník
  * GeometryCollection
    * Systému multiPoint
    * MultiLineString
    * MultiPolygon

> [!WARNING]
> CircularString, CompoundCurve a CurePolygon nepodporuje chny Zarážky.

Použití základního typu Geometry umožňuje libovolný typ tvar, který má být určené vlastností.

Následující třídy entit může použít k mapování na tabulky v [ukázkové databáze Wide World Importers](http://go.microsoft.com/fwlink/?LinkID=800630).

``` csharp
[Table("Cities", Schema = "Application"))]
class City
{
    public int CityID { get; set; }

    public string CityName { get; set; }

    public IPoint Location { get; set; }
}

[Table("Countries", Schema = "Application"))]
class Country
{
    public int CountryID { get; set; }

    public string CountryName { get; set; }

    // Database includes both Polygon and MultiPolygon values
    public IGeometry Border { get; set; }
}
```

### <a name="creating-values"></a>Vytváření hodnoty

Konstruktory můžete použít k vytvoření geometrické objekty; Směřuje doporučuje místo toho použít objekt pro vytváření geometry. To umožňuje určit výchozí SRID (spatial reference systém souřadnice) a umožňuje kontrolu nad pokročilejší věci jako přesnost modelu (využitých výpočtů) a souřadnice pořadí (Určuje, která souřadnice--dimenze a míry – jsou k dispozici).

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> 4326 odkazuje na WGS 84 standard používaný v GPS a jiných zeměpisných systémů.

### <a name="longitude-and-latitude"></a>Zeměpisné šířky a délky

Souřadnice v chny Zarážky jsou z hlediska hodnoty X a Y. K reprezentaci zeměpisné šířky a délky, použijte pro zeměpisnou délku a Y pro zeměpisnou šířku X. Všimněte si, že toto je **zpětně** z `latitude, longitude` formát, ve kterém se obvykle zobrazí tyto hodnoty.

### <a name="srid-ignored-during-client-operations"></a>SRID ignorovat během operace klienta

Směřuje ignoruje SRID hodnoty během operací. Předpokládá planární systém souřadnic. To znamená, že pokud zadáte souřadnice z hlediska zeměpisnou délku a šířku, některé hodnoty klienta vyhodnocen jako vzdálenost, délku a oblast bude ve stupních, ne měřiče. Pro více smysluplné hodnoty, je nutné nejprve do projektu souřadnice, kde jiný souřadnicový systém pomocí knihovny jako [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) před výpočtem tyto hodnoty.

Pokud operace serveru vyhodnocovaný EF Core pomocí SQL, určí databáze jednotky výsledek.

Tady je příklad použití ProjNet4GeoAPI k výpočtu vzdálenost mezi dvěma měst.

``` csharp
static class GeometryExtensions
{
    static readonly IGeometryServices _geometryServices = NtsGeometryServices.Instance;
    static readonly ICoordinateSystemServices _coordinateSystemServices
        = new CoordinateSystemServices(
            new CoordinateSystemFactory(),
            new CoordinateTransformationFactory(),
            new Dictionary<int, string>
            {
                // Coordinate systems:

                // (3857 and 4326 included automatically)

                // This coordinate system covers the area of our data.
                // Different data requires a different coordinate system.
                [2855] =
                @"
                    PROJCS[""NAD83(HARN) / Washington North"",
                        GEOGCS[""NAD83(HARN)"",
                            DATUM[""NAD83_High_Accuracy_Regional_Network"",
                                SPHEROID[""GRS 1980"",6378137,298.257222101,
                                    AUTHORITY[""EPSG"",""7019""]],
                                AUTHORITY[""EPSG"",""6152""]],
                            PRIMEM[""Greenwich"",0,
                                AUTHORITY[""EPSG"",""8901""]],
                            UNIT[""degree"",0.01745329251994328,
                                AUTHORITY[""EPSG"",""9122""]],
                            AUTHORITY[""EPSG"",""4152""]],
                        PROJECTION[""Lambert_Conformal_Conic_2SP""],
                        PARAMETER[""standard_parallel_1"",48.73333333333333],
                        PARAMETER[""standard_parallel_2"",47.5],
                        PARAMETER[""latitude_of_origin"",47],
                        PARAMETER[""central_meridian"",-120.8333333333333],
                        PARAMETER[""false_easting"",500000],
                        PARAMETER[""false_northing"",0],
                        UNIT[""metre"",1,
                            AUTHORITY[""EPSG"",""9001""]],
                        AUTHORITY[""EPSG"",""2855""]]
                "
            });

    public static IGeometry ProjectTo(this IGeometry geometry, int srid)
    {
        var geometryFactory = _geometryServices.CreateGeometryFactory(srid);
        var transformation = _coordinateSystemServices.CreateTransformation(geometry.SRID, srid);

        return GeometryTransform.TransformGeometry(
            geometryFactory,
            geometry,
            transformation.MathTransform);
    }
}
```

``` csharp
var seattle = new Point(-122.333056, 47.609722) { SRID = 4326 };
var redmond = new Point(-122.123889, 47.669444) { SRID = 4326 };

var distance = seattle.ProjectTo(2855).Distance(redmond.ProjectTo(2855));
```

## <a name="querying-data"></a>Dotazování na data

V technologii LINQ ny Klienty metody a vlastnosti, které jsou k dispozici jako funkce databáze bude do kódu SQL. Například jsou přeloženy metody vzdálenosti a obsahuje následující dotazy. V tabulce na konci tohoto článku jsou uvedeny podporované členy podle různých zprostředkovatelů EF Core.

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a>SQL Server

Pokud používáte systém SQL Server, existují některé další věci, které byste měli vědět.

### <a name="geography-or-geometry"></a>Zeměpisné oblasti nebo geometrie

Ve výchozím nastavení, prostorová vlastností se mapují na `geography` sloupce v systému SQL Server. Chcete-li použít `geometry`, [nakonfigurovat typ sloupce](xref:core/modeling/relational/data-types) ve vašem modelu.

### <a name="geography-polygon-rings"></a>Zeměpisné oblasti prstence mnohoúhelníku

Při použití `geography` typ sloupce, SQL Server vyžaduje další požadavky na vnější prstenec (nebo prostředí) a vnitřní okruhů (nebo děr). Vnější prstenec musí být orientovaný proti směru hodinových ručiček a vnitřní prstenci po směru hodinových ručiček. Směřuje ověří to před odesláním hodnoty do databáze.

### <a name="fullglobe"></a>Objekt FullGlobe

Systém SQL Server má nestandardní geometrie typ pro reprezentaci úplné světě při použití `geography` typ sloupce. Má také způsob, jak reprezentaci mnohoúhelníky založené na celé zeměkouli (bez vnější prstenec). Ani jeden z těchto podporovaných chny Zarážky.

> [!WARNING]
> Objekt FullGlobe a na jejím základě mnohoúhelníky chny Zarážky nejsou podporovány.

## <a name="sqlite"></a>SQLite

Zde jsou některé další informace k těm, kteří používají SQLite.

### <a name="installing-spatialite"></a>Instalace SpatiaLite

Na Windows nativní mod_spatialite knihovny je distribuován jako závislost balíčku NuGet. Jiné platformy je nutné nainstalovat samostatně. To se obvykle provádí pomocí Správce balíčků softwaru. Můžete například použít APT na Ubuntu a Homebrew v systému MacOS.

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

### <a name="configuring-srid"></a>Konfigurace SRID

Sloupce v SpatiaLite, třeba zadat SRID na sloupec. Výchozí hodnota je SRID `0`. Zadejte jiný SRID ForSqliteHasSrid metodou.

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a>Rozměr

Podobně jako u SRID, dimenze sloupec (nebo souřadnice) je také zadaný jako součást sloupci. Jsou výchozí souřadnice X a Y. povolit další souřadnice (Z a M) ForSqliteHasDimension metodou.

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a>Přeložené operace

Tato tabulka uvádí, které členy chny Zarážky jsou přeloženy do SQL od každého poskytovatele EF Core.

NetTopologySuite | SQL Server (Geometrie) | SQL Server (geography) | SQLite | Npgsql
--- |:---:|:---:|:---:|:---:
Geometry.Area | ✔ | ✔ | ✔ | ✔
Geometry.AsBinary() | ✔ | ✔ | ✔ | ✔
Geometry.AsText() | ✔ | ✔ | ✔ | ✔
Geometry.Boundary | ✔ | | ✔ | ✔
Geometry.Buffer(double) | ✔ | ✔ | ✔ | ✔
Geometry.Buffer (double, int). | | | ✔
Geometry.Centroid | ✔ | | ✔ | ✔
Geometry.Contains(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.ConvexHull() | ✔ | ✔ | ✔ | ✔
Geometry.CoveredBy(Geometry) | | | ✔ | ✔
Geometry.Covers(Geometry) | | | ✔ | ✔
Geometry.Crosses(Geometry) | ✔ | | ✔ | ✔
Geometry.Difference(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Dimension | ✔ | ✔ | ✔ | ✔
Geometry.Disjoint(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Distance(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Envelope | ✔ | | ✔ | ✔
Geometry.EqualsExact(Geometry) | | | | ✔
Geometry.EqualsTopologically(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.GeometryType | ✔ | ✔ | ✔ | ✔
Geometry.GetGeometryN(int) | ✔ | | ✔ | ✔
Geometry.InteriorPoint | ✔ | | ✔
Geometry.Intersection(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Intersects(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.IsEmpty | ✔ | ✔ | ✔ | ✔
Geometry.IsSimple | ✔ | | ✔ | ✔
Geometry.IsValid | ✔ | ✔ | ✔ | ✔
Geometry.IsWithinDistance (geometrie, double) | ✔ | | ✔
Geometry.Length | ✔ | ✔ | ✔ | ✔
Geometry.NumGeometries | ✔ | ✔ | ✔ | ✔
Geometry.NumPoints | ✔ | ✔ | ✔ | ✔
Geometry.OgcGeometryType | ✔ | ✔ | ✔
Geometry.Overlaps(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.PointOnSurface | ✔ | | ✔ | ✔
Geometry.Relate (geometrie, string) | ✔ | | ✔ | ✔
Geometry.Reverse() | | | ✔ | ✔
Geometry.SRID | ✔ | ✔ | ✔ | ✔
Geometry.SymmetricDifference(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.ToBinary() | ✔ | ✔ | ✔ | ✔
Geometry.ToText() | ✔ | ✔ | ✔ | ✔
Geometry.Touches(Geometry) | ✔ | | ✔ | ✔
Geometry.Union() | | | ✔
Geometry.Union(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Within(Geometry) | ✔ | ✔ | ✔ | ✔
GeometryCollection.Count | ✔ | ✔ | ✔ | ✔
GeometryCollection [int] | ✔ | ✔ | ✔ | ✔
LineString.Count | ✔ | ✔ | ✔ | ✔
LineString.EndPoint | ✔ | ✔ | ✔ | ✔
LineString.GetPointN(int) | ✔ | ✔ | ✔ | ✔
LineString.IsClosed | ✔ | ✔ | ✔ | ✔
LineString.IsRing | ✔ | | ✔ | ✔
LineString.StartPoint | ✔ | ✔ | ✔ | ✔
MultiLineString.IsClosed | ✔ | ✔ | ✔ | ✔
Point.M | ✔ | ✔ | ✔ | ✔
Point.X | ✔ | ✔ | ✔ | ✔
Point.Y | ✔ | ✔ | ✔ | ✔
Point.Z | ✔ | ✔ | ✔ | ✔
Polygon.ExteriorRing | ✔ | ✔ | ✔ | ✔
Polygon.GetInteriorRingN(int) | ✔ | ✔ | ✔ | ✔
Polygon.NumInteriorRings | ✔ | ✔ | ✔ | ✔

## <a name="additional-resources"></a>Další zdroje

* [Prostorová Data v systému SQL Server](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [Domovská stránka SpatiaLite](https://www.gaia-gis.it/fossil/libspatialite)
* [Dokumentace ke službě PostGIS](http://postgis.net/documentation/)
