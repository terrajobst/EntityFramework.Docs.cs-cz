---
title: Prostorová data – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: 5b45f83ca7f02665f52ccfe16b5af506a6046a62
ms.sourcegitcommit: f2a38c086291699422d8b28a72d9611d1b24ad0d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/16/2020
ms.locfileid: "76124428"
---
# <a name="spatial-data"></a>Prostorová data

> [!NOTE]
> Tato funkce se přidala do EF Core 2,2.

Prostorová data představují fyzické umístění a tvar objektů. Mnohé databáze poskytují podporu pro tento typ dat, aby je bylo možné indexovat a dotazovat společně s ostatními daty. Mezi běžné scénáře patří dotazování pro objekty v dané vzdálenosti od místa nebo výběr objektu, jehož ohraničení obsahuje dané umístění. EF Core podporuje mapování na prostorové datové typy pomocí knihovny prostorů [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) .

## <a name="installing"></a>Instalace nástroje

Aby bylo možné použít prostorová data s EF Core, je nutné nainstalovat příslušný podpůrný balíček NuGet. Který balíček, který potřebujete nainstalovat, závisí na používaném poskytovateli.

Poskytovatel EF Core                        | Prostorový balíček NuGet
--------------------------------------- | ---------------------
Microsoft.EntityFrameworkCore.SqlServer | [Microsoft. EntityFrameworkCore. SqlServer. NetTopologySuite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
Microsoft.EntityFrameworkCore.Sqlite    | [Microsoft. EntityFrameworkCore. sqlite. NetTopologySuite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
Microsoft.EntityFrameworkCore.InMemory  | [NetTopologySuite](https://www.nuget.org/packages/NetTopologySuite)
Npgsql.EntityFrameworkCore.PostgreSQL   | [Npgsql. EntityFrameworkCore. PostgreSQL. NetTopologySuite](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a>Zpětná analýza

Prostorové balíčky NuGet také umožňují modely [zpětné analýzy](../managing-schemas/scaffolding.md) s prostorovými vlastnostmi, ale ***před*** spuštěním `Scaffold-DbContext` nebo `dotnet ef dbcontext scaffold`je potřeba balíček nainstalovat. Pokud to neuděláte, zobrazí se upozornění týkající se nehledání mapování typů pro sloupce a sloupce se přeskočí.

## <a name="nettopologysuite-nts"></a>NetTopologySuite (NTS)

NetTopologySuite je prostorová knihovna pro .NET. EF Core umožňuje mapování na prostorové datové typy v databázi pomocí typů NTS v modelu.

Chcete-li povolit mapování na prostorové typy prostřednictvím NTS, volejte metodu UseNetTopologySuite na tvůrci možností DbContext poskytovatele. Například pomocí SQL Server byste to rádi volali.

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

Existuje několik prostorových datových typů. Typ, který použijete, závisí na typech tvarů, které chcete zapnout. Tady je hierarchie typů NTS, které můžete použít pro vlastnosti v modelu. Nacházejí se v oboru názvů `NetTopologySuite.Geometries`.

* Geometrie
  * Vyberte
  * LineString
  * Mnohoúhelník
  * GeometryCollection
    * MultiPoint
    * MultiLineString
    * MultiPolygon

> [!WARNING]
> CircularString, CompoundCurve a CurePolygon nejsou podporovány NTS.

Použití typu základní geometrie umožňuje, aby byl jakýkoli typ obrazce určen vlastností.

Následující třídy entit se dají použít k mapování tabulek v [ukázkové databázi World World Imports](https://go.microsoft.com/fwlink/?LinkID=800630).

``` csharp
[Table("Cities", Schema = "Application"))]
class City
{
    public int CityID { get; set; }

    public string CityName { get; set; }

    public Point Location { get; set; }
}

[Table("Countries", Schema = "Application"))]
class Country
{
    public int CountryID { get; set; }

    public string CountryName { get; set; }

    // Database includes both Polygon and MultiPolygon values
    public Geometry Border { get; set; }
}
```

### <a name="creating-values"></a>Vytváření hodnot

Konstruktory lze použít k vytvoření objektů geometrie; NTS však doporučuje použít místo toho objekt pro vytváření geometrie. To vám umožní určit výchozí SRID (prostorový referenční systém používaný souřadnicemi) a vám umožní ovládat pokročilejší věci, jako je model přesnosti (použitý během výpočtů) a sekvence souřadnic (určuje, které souřadnice--Dimensions a míry – jsou k dispozici).

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> 4326 odkazuje na WGS 84, Standard používaný v GPS a dalších geografických systémech.

### <a name="longitude-and-latitude"></a>Zeměpisná délka a zeměpisná šířka

Souřadnice v NTS jsou vyhledané v hodnotách X a Y. Aby představoval zeměpisnou délku a zeměpisnou šířku, použijte X pro zeměpisnou délku a Y pro zeměpisnou šířku. Všimněte si, že se jedná o **zpětnou** hodnotu z formátu `latitude, longitude`, ve kterém tyto hodnoty obvykle vidíte.

### <a name="srid-ignored-during-client-operations"></a>SRID se během operací klienta ignorovat.

NTS ignoruje hodnoty SRID během operací. Předpokládá planární souřadnicový systém. To znamená, že pokud zadáte souřadnice z oblasti Zeměpisná délka a zeměpisná šířka, některé hodnoty vyhodnocené klientem, jako je vzdálenost, délka a oblast, budou ve stupních, nikoli měřičích. Pro smysluplnější hodnoty musíte nejprve před výpočtem těchto hodnot vyhodnotit souřadnice pro jiný systém souřadnic pomocí knihovny, jako je [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) .

Pokud je operace vyhodnocena serverem pomocí SQL EF Core prostřednictvím SQL, určí se jednotka výsledku databáze.

Tady je příklad použití ProjNet4GeoAPI k výpočtu vzdálenosti mezi dvěma městy.

``` csharp
static class GeometryExtensions
{
    static readonly CoordinateSystemServices _coordinateSystemServices
        = new CoordinateSystemServices(
            new CoordinateSystemFactory(),
            new CoordinateTransformationFactory(),
            new Dictionary<int, string>
            {
                // Coordinate systems:

                [4326] = GeographicCoordinateSystem.WGS84.WKT,

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

    public static Geometry ProjectTo(this Geometry geometry, int srid)
    {
        var transformation = _coordinateSystemServices.CreateTransformation(geometry.SRID, srid);

        var result = geometry.Copy();
        result.Apply(new MathTransformFilter(transformation.MathTransform));

        return result;
    }

    class MathTransformFilter : ICoordinateSequenceFilter
    {
        readonly MathTransform _transform;

        public MathTransformFilter(MathTransform transform)
            => _transform = transform;

        public bool Done => false;
        public bool GeometryChanged => true;

        public void Filter(CoordinateSequence seq, int i)
        {
            var result = _transform.Transform(
                new[]
                {
                    seq.GetOrdinate(i, Ordinate.X),
                    seq.GetOrdinate(i, Ordinate.Y)
                });
            seq.SetOrdinate(i, Ordinate.X, result[0]);
            seq.SetOrdinate(i, Ordinate.Y, result[1]);
        }
    }
}
```

``` csharp
var seattle = new Point(-122.333056, 47.609722) { SRID = 4326 };
var redmond = new Point(-122.123889, 47.669444) { SRID = 4326 };

var distance = seattle.ProjectTo(2855).Distance(redmond.ProjectTo(2855));
```

## <a name="querying-data"></a>Dotazování na data

V jazyce LINQ budou metody a vlastnosti NTS dostupné jako databázové funkce přeloženy do jazyka SQL. Například vzdálenost a obsahuje metody jsou přeloženy v následujících dotazech. Tabulka na konci tohoto článku ukazuje, kteří členové jsou podporováni různými poskytovateli EF Core.

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a>Server SQL

Pokud používáte SQL Server, máte k dispozici několik dalších věcí, o kterých byste měli vědět.

### <a name="geography-or-geometry"></a>Zeměpisná nebo geometrie

Ve výchozím nastavení jsou prostorové vlastnosti namapovány na `geography` sloupce v SQL Server. Pokud chcete použít `geometry`, nakonfigurujte v modelu [typ sloupce](xref:core/modeling/entity-properties#column-data-types) .

### <a name="geography-polygon-rings"></a>Geografické kroužky mnohoúhelníků

Při použití `geography`ho typu sloupce SQL Server ukládá další požadavky na vnější prstenec (nebo prostředí) a vnitřní prstence (nebo díry). Vnější prstenec musí být orientovaný proti směru hodinových ručiček a vnitřní prstence po směru hodinových ručiček. NTS ho před odesláním hodnot do databáze ověří.

### <a name="fullglobe"></a>FullGlobe

SQL Server má nestandardní typ geometrie, který představuje úplný glóbus při použití `geography`ho typu sloupce. Má také způsob, jak znázornit mnohoúhelníky na základě plného světa (bez vnějšího okruhu). Ani jedna z těchto možností není podporována nástrojem NTS.

> [!WARNING]
> FullGlobe a mnohoúhelníky, které jsou na nich založené, nejsou podporovány NTS.

## <a name="sqlite"></a>SQLite

Zde jsou některé další informace pro ty, které používají SQLite.

### <a name="installing-spatialite"></a>Instalace SpatiaLite

V systému Windows je nativní knihovna mod_spatialite distribuována jako závislost balíčku NuGet. Jiné platformy je potřeba nainstalovat samostatně. To se obvykle provádí pomocí Správce balíčků softwaru. Například můžete použít APT v Ubuntu a homebrew na MacOS.

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

Bohužel novější verze PROJ (závislost SpatiaLite) jsou nekompatibilní s výchozí sadou [SQLitePCLRaw sady](/dotnet/standard/data/sqlite/custom-versions#bundles)EF. Můžete to obejít buď vytvořením vlastního [poskytovatele SQLitePCLRaw](/dotnet/standard/data/sqlite/custom-versions#sqlitepclraw-providers) , který používá knihovnu systémových souborů, nebo můžete nainstalovat vlastní sestavení SpatiaLite, které zakazuje podporu proj.

``` sh
curl https://www.gaia-gis.it/gaia-sins/libspatialite-4.3.0a.tar.gz | tar -xz
cd libspatialite-4.3.0a

if [[ `uname -s` == Darwin* ]]; then
    # Mac OS requires some minor patching
    sed -i "" "s/shrext_cmds='\`test \\.\$module = .yes && echo .so \\|\\| echo \\.dylib\`'/shrext_cmds='.dylib'/g" configure
fi

./configure --disable-proj
make
make install
```

### <a name="configuring-srid"></a>Konfigurace SRID

V SpatiaLite musí sloupce určovat SRID na sloupec. Výchozí SRID je `0`. Určete jiný SRID pomocí metody ForSqliteHasSrid.

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a>Rozměr

Podobně jako u SRID je jako součást sloupce zadána také dimenze sloupce (nebo souřadnice). Výchozí souřadnice jsou X a Y. Povolte další souřadnice (Z a M) pomocí metody ForSqliteHasDimension.

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a>Přeložené operace

Tato tabulka uvádí, které NTS členové jsou přeloženi do jazyka SQL každým poskytovatelem EF Core.

NetTopologySuite | SQL Server (geometrie) | SQL Server (geografie) | SQLite | Npgsql
--- |:---:|:---:|:---:|:---:
Geometrie. Area | ✔ | ✔ | ✔ | ✔
Geometry. AsBinary () | ✔ | ✔ | ✔ | ✔
Geometry. AsText () | ✔ | ✔ | ✔ | ✔
Geometrie. hranice | ✔ | | ✔ | ✔
Geometry. Buffer (Double) | ✔ | ✔ | ✔ | ✔
Geometry. Buffer (Double, int) | | | ✔ | ✔
Geometrie. těžiště | ✔ | | ✔ | ✔
Geometrie. Contains (geometrie) | ✔ | ✔ | ✔ | ✔
Geometry. ConvexHull () | ✔ | ✔ | ✔ | ✔
Geometry. CoveredBy (geometrie) | | | ✔ | ✔
Geometry. pokrývání (geometrie) | | | ✔ | ✔
Geometrie. křížení (geometrie) | ✔ | | ✔ | ✔
Geometry. rozdíl (geometrie) | ✔ | ✔ | ✔ | ✔
Geometrie. Dimension | ✔ | ✔ | ✔ | ✔
Geometrie. unkloub (geometrie) | ✔ | ✔ | ✔ | ✔
Geometrie. Distance (geometrie) | ✔ | ✔ | ✔ | ✔
Geometrie. obálky | ✔ | | ✔ | ✔
Geometry. EqualsExact (geometrie) | | | | ✔
Geometry. EqualsTopologically (geometrie) | ✔ | ✔ | ✔ | ✔
Geometrie. GeometryType | ✔ | ✔ | ✔ | ✔
Geometry. GetGeometryN (int) | ✔ | | ✔ | ✔
Geometrie. InteriorPoint | ✔ | | ✔ | ✔
Geometry. proprůsečík (geometrie) | ✔ | ✔ | ✔ | ✔
Geometrie. INTERSECTY (geometrie) | ✔ | ✔ | ✔ | ✔
Geometrie. Empty | ✔ | ✔ | ✔ | ✔
Geometrie. zjednodušená | ✔ | | ✔ | ✔
Geometrie. IsValid | ✔ | ✔ | ✔ | ✔
Geometry. IsWithinDistance (geometrie, Double) | ✔ | | ✔ | ✔
Geometrie. Length | ✔ | ✔ | ✔ | ✔
Geometrie. NumGeometries | ✔ | ✔ | ✔ | ✔
Geometrie. NumPoints | ✔ | ✔ | ✔ | ✔
Geometrie. OgcGeometryType | ✔ | ✔ | ✔ | ✔
Geometrie. překryvy (geometrie) | ✔ | ✔ | ✔ | ✔
Geometrie. PointOnSurface | ✔ | | ✔ | ✔
Geometry. propojovat (geometrie, String) | ✔ | | ✔ | ✔
Geometry. Reverse () | | | ✔ | ✔
Geometrie. SRID | ✔ | ✔ | ✔ | ✔
Geometry. SymmetricDifference (geometrie) | ✔ | ✔ | ✔ | ✔
Geometry. ToBinary () | ✔ | ✔ | ✔ | ✔
Geometry. ToText () | ✔ | ✔ | ✔ | ✔
Geometrie. touchs (geometrie) | ✔ | | ✔ | ✔
Geometry. Union () | | | ✔ | ✔
Geometry. Union (geometrie) | ✔ | ✔ | ✔ | ✔
Geometrie. uvnitř (geometrie) | ✔ | ✔ | ✔ | ✔
Geometriecollection. Count | ✔ | ✔ | ✔ | ✔
GeometryCollection [int] | ✔ | ✔ | ✔ | ✔
LineString. Count | ✔ | ✔ | ✔ | ✔
LineString. EndPoint | ✔ | ✔ | ✔ | ✔
LineString. GetPointN (int) | ✔ | ✔ | ✔ | ✔
LineString. uzavřeno | ✔ | ✔ | ✔ | ✔
LineString. pozvonění | ✔ | | ✔ | ✔
LineString. StartPoint | ✔ | ✔ | ✔ | ✔
MultiLineString. uzavřeno | ✔ | ✔ | ✔ | ✔
Point. M | ✔ | ✔ | ✔ | ✔
Point. X | ✔ | ✔ | ✔ | ✔
Point. Y | ✔ | ✔ | ✔ | ✔
Point. Z | ✔ | ✔ | ✔ | ✔
Mnohoúhelník. ExteriorRing | ✔ | ✔ | ✔ | ✔
Mnohoúhelník. GetInteriorRingN (int) | ✔ | ✔ | ✔ | ✔
Mnohoúhelník. NumInteriorRings | ✔ | ✔ | ✔ | ✔

## <a name="additional-resources"></a>Další materiály a zdroje informací

* [Prostorová data v SQL Server](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [Domovská stránka SpatiaLite](https://www.gaia-gis.it/fossil/libspatialite)
* [Npgsql prostorová dokumentace](https://www.npgsql.org/efcore/mapping/nts.html)
* [Dokumentace k PostGIS](https://postgis.net/documentation/)
