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
# <a name="spatial-data"></a><span data-ttu-id="1bab0-102">Prostorová data</span><span class="sxs-lookup"><span data-stu-id="1bab0-102">Spatial Data</span></span>

> [!NOTE]
> <span data-ttu-id="1bab0-103">Tato funkce se přidala do EF Core 2,2.</span><span class="sxs-lookup"><span data-stu-id="1bab0-103">This feature was added in EF Core 2.2.</span></span>

<span data-ttu-id="1bab0-104">Prostorová data představují fyzické umístění a tvar objektů.</span><span class="sxs-lookup"><span data-stu-id="1bab0-104">Spatial data represents the physical location and the shape of objects.</span></span> <span data-ttu-id="1bab0-105">Mnohé databáze poskytují podporu pro tento typ dat, aby je bylo možné indexovat a dotazovat společně s ostatními daty.</span><span class="sxs-lookup"><span data-stu-id="1bab0-105">Many databases provide support for this type of data so it can be indexed and queried alongside other data.</span></span> <span data-ttu-id="1bab0-106">Mezi běžné scénáře patří dotazování pro objekty v dané vzdálenosti od místa nebo výběr objektu, jehož ohraničení obsahuje dané umístění.</span><span class="sxs-lookup"><span data-stu-id="1bab0-106">Common scenarios include querying for objects within a given distance from a location, or selecting the object whose border contains a given location.</span></span> <span data-ttu-id="1bab0-107">EF Core podporuje mapování na prostorové datové typy pomocí knihovny prostorů [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) .</span><span class="sxs-lookup"><span data-stu-id="1bab0-107">EF Core supports mapping to spatial data types using the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) spatial library.</span></span>

## <a name="installing"></a><span data-ttu-id="1bab0-108">Instalace nástroje</span><span class="sxs-lookup"><span data-stu-id="1bab0-108">Installing</span></span>

<span data-ttu-id="1bab0-109">Aby bylo možné použít prostorová data s EF Core, je nutné nainstalovat příslušný podpůrný balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="1bab0-109">In order to use spatial data with EF Core, you need to install the appropriate supporting NuGet package.</span></span> <span data-ttu-id="1bab0-110">Který balíček, který potřebujete nainstalovat, závisí na používaném poskytovateli.</span><span class="sxs-lookup"><span data-stu-id="1bab0-110">Which package you need to install depends on the provider you're using.</span></span>

<span data-ttu-id="1bab0-111">Poskytovatel EF Core</span><span class="sxs-lookup"><span data-stu-id="1bab0-111">EF Core Provider</span></span>                        | <span data-ttu-id="1bab0-112">Prostorový balíček NuGet</span><span class="sxs-lookup"><span data-stu-id="1bab0-112">Spatial NuGet Package</span></span>
--------------------------------------- | ---------------------
<span data-ttu-id="1bab0-113">Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="1bab0-113">Microsoft.EntityFrameworkCore.SqlServer</span></span> | [<span data-ttu-id="1bab0-114">Microsoft. EntityFrameworkCore. SqlServer. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="1bab0-114">Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
<span data-ttu-id="1bab0-115">Microsoft.EntityFrameworkCore.Sqlite</span><span class="sxs-lookup"><span data-stu-id="1bab0-115">Microsoft.EntityFrameworkCore.Sqlite</span></span>    | [<span data-ttu-id="1bab0-116">Microsoft. EntityFrameworkCore. sqlite. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="1bab0-116">Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
<span data-ttu-id="1bab0-117">Microsoft.EntityFrameworkCore.InMemory</span><span class="sxs-lookup"><span data-stu-id="1bab0-117">Microsoft.EntityFrameworkCore.InMemory</span></span>  | [<span data-ttu-id="1bab0-118">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="1bab0-118">NetTopologySuite</span></span>](https://www.nuget.org/packages/NetTopologySuite)
<span data-ttu-id="1bab0-119">Npgsql.EntityFrameworkCore.PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="1bab0-119">Npgsql.EntityFrameworkCore.PostgreSQL</span></span>   | [<span data-ttu-id="1bab0-120">Npgsql. EntityFrameworkCore. PostgreSQL. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="1bab0-120">Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite</span></span>](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a><span data-ttu-id="1bab0-121">Zpětná analýza</span><span class="sxs-lookup"><span data-stu-id="1bab0-121">Reverse engineering</span></span>

<span data-ttu-id="1bab0-122">Prostorové balíčky NuGet také umožňují modely [zpětné analýzy](../managing-schemas/scaffolding.md) s prostorovými vlastnostmi, ale ***před*** spuštěním `Scaffold-DbContext` nebo `dotnet ef dbcontext scaffold`je potřeba balíček nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="1bab0-122">The spatial NuGet packages also enable [reverse engineering](../managing-schemas/scaffolding.md) models with spatial properties, but you need to install the package ***before*** running `Scaffold-DbContext` or `dotnet ef dbcontext scaffold`.</span></span> <span data-ttu-id="1bab0-123">Pokud to neuděláte, zobrazí se upozornění týkající se nehledání mapování typů pro sloupce a sloupce se přeskočí.</span><span class="sxs-lookup"><span data-stu-id="1bab0-123">If you don't, you'll receive warnings about not finding type mappings for the columns and the columns will be skipped.</span></span>

## <a name="nettopologysuite-nts"></a><span data-ttu-id="1bab0-124">NetTopologySuite (NTS)</span><span class="sxs-lookup"><span data-stu-id="1bab0-124">NetTopologySuite (NTS)</span></span>

<span data-ttu-id="1bab0-125">NetTopologySuite je prostorová knihovna pro .NET.</span><span class="sxs-lookup"><span data-stu-id="1bab0-125">NetTopologySuite is a spatial library for .NET.</span></span> <span data-ttu-id="1bab0-126">EF Core umožňuje mapování na prostorové datové typy v databázi pomocí typů NTS v modelu.</span><span class="sxs-lookup"><span data-stu-id="1bab0-126">EF Core enables mapping to spatial data types in the database by using NTS types in your model.</span></span>

<span data-ttu-id="1bab0-127">Chcete-li povolit mapování na prostorové typy prostřednictvím NTS, volejte metodu UseNetTopologySuite na tvůrci možností DbContext poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="1bab0-127">To enable mapping to spatial types via NTS, call the UseNetTopologySuite method on the provider's DbContext options builder.</span></span> <span data-ttu-id="1bab0-128">Například pomocí SQL Server byste to rádi volali.</span><span class="sxs-lookup"><span data-stu-id="1bab0-128">For example, with SQL Server you'd call it like this.</span></span>

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

<span data-ttu-id="1bab0-129">Existuje několik prostorových datových typů.</span><span class="sxs-lookup"><span data-stu-id="1bab0-129">There are several spatial data types.</span></span> <span data-ttu-id="1bab0-130">Typ, který použijete, závisí na typech tvarů, které chcete zapnout.</span><span class="sxs-lookup"><span data-stu-id="1bab0-130">Which type you use depends on the types of shapes you want to allow.</span></span> <span data-ttu-id="1bab0-131">Tady je hierarchie typů NTS, které můžete použít pro vlastnosti v modelu.</span><span class="sxs-lookup"><span data-stu-id="1bab0-131">Here is the hierarchy of NTS types that you can use for properties in your model.</span></span> <span data-ttu-id="1bab0-132">Nacházejí se v oboru názvů `NetTopologySuite.Geometries`.</span><span class="sxs-lookup"><span data-stu-id="1bab0-132">They're located within the `NetTopologySuite.Geometries` namespace.</span></span>

* <span data-ttu-id="1bab0-133">Geometrie</span><span class="sxs-lookup"><span data-stu-id="1bab0-133">Geometry</span></span>
  * <span data-ttu-id="1bab0-134">Vyberte</span><span class="sxs-lookup"><span data-stu-id="1bab0-134">Point</span></span>
  * <span data-ttu-id="1bab0-135">LineString</span><span class="sxs-lookup"><span data-stu-id="1bab0-135">LineString</span></span>
  * <span data-ttu-id="1bab0-136">Mnohoúhelník</span><span class="sxs-lookup"><span data-stu-id="1bab0-136">Polygon</span></span>
  * <span data-ttu-id="1bab0-137">GeometryCollection</span><span class="sxs-lookup"><span data-stu-id="1bab0-137">GeometryCollection</span></span>
    * <span data-ttu-id="1bab0-138">MultiPoint</span><span class="sxs-lookup"><span data-stu-id="1bab0-138">MultiPoint</span></span>
    * <span data-ttu-id="1bab0-139">MultiLineString</span><span class="sxs-lookup"><span data-stu-id="1bab0-139">MultiLineString</span></span>
    * <span data-ttu-id="1bab0-140">MultiPolygon</span><span class="sxs-lookup"><span data-stu-id="1bab0-140">MultiPolygon</span></span>

> [!WARNING]
> <span data-ttu-id="1bab0-141">CircularString, CompoundCurve a CurePolygon nejsou podporovány NTS.</span><span class="sxs-lookup"><span data-stu-id="1bab0-141">CircularString, CompoundCurve, and CurePolygon aren't supported by NTS.</span></span>

<span data-ttu-id="1bab0-142">Použití typu základní geometrie umožňuje, aby byl jakýkoli typ obrazce určen vlastností.</span><span class="sxs-lookup"><span data-stu-id="1bab0-142">Using the base Geometry type allows any type of shape to be specified by the property.</span></span>

<span data-ttu-id="1bab0-143">Následující třídy entit se dají použít k mapování tabulek v [ukázkové databázi World World Imports](https://go.microsoft.com/fwlink/?LinkID=800630).</span><span class="sxs-lookup"><span data-stu-id="1bab0-143">The following entity classes could be used to map to tables in the [Wide World Importers sample database](https://go.microsoft.com/fwlink/?LinkID=800630).</span></span>

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

### <a name="creating-values"></a><span data-ttu-id="1bab0-144">Vytváření hodnot</span><span class="sxs-lookup"><span data-stu-id="1bab0-144">Creating values</span></span>

<span data-ttu-id="1bab0-145">Konstruktory lze použít k vytvoření objektů geometrie; NTS však doporučuje použít místo toho objekt pro vytváření geometrie.</span><span class="sxs-lookup"><span data-stu-id="1bab0-145">You can use constructors to create geometry objects; however, NTS recommends using a geometry factory instead.</span></span> <span data-ttu-id="1bab0-146">To vám umožní určit výchozí SRID (prostorový referenční systém používaný souřadnicemi) a vám umožní ovládat pokročilejší věci, jako je model přesnosti (použitý během výpočtů) a sekvence souřadnic (určuje, které souřadnice--Dimensions a míry – jsou k dispozici).</span><span class="sxs-lookup"><span data-stu-id="1bab0-146">This lets you specify a default SRID (the spatial reference system used by the coordinates) and gives you control over more advanced things like the precision model (used during calculations) and the coordinate sequence (determines which ordinates--dimensions and measures--are available).</span></span>

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> <span data-ttu-id="1bab0-147">4326 odkazuje na WGS 84, Standard používaný v GPS a dalších geografických systémech.</span><span class="sxs-lookup"><span data-stu-id="1bab0-147">4326 refers to WGS 84, a standard used in GPS and other geographic systems.</span></span>

### <a name="longitude-and-latitude"></a><span data-ttu-id="1bab0-148">Zeměpisná délka a zeměpisná šířka</span><span class="sxs-lookup"><span data-stu-id="1bab0-148">Longitude and Latitude</span></span>

<span data-ttu-id="1bab0-149">Souřadnice v NTS jsou vyhledané v hodnotách X a Y.</span><span class="sxs-lookup"><span data-stu-id="1bab0-149">Coordinates in NTS are in terms of X and Y values.</span></span> <span data-ttu-id="1bab0-150">Aby představoval zeměpisnou délku a zeměpisnou šířku, použijte X pro zeměpisnou délku a Y pro zeměpisnou šířku.</span><span class="sxs-lookup"><span data-stu-id="1bab0-150">To represent longitude and latitude, use X for longitude and Y for latitude.</span></span> <span data-ttu-id="1bab0-151">Všimněte si, že se jedná o **zpětnou** hodnotu z formátu `latitude, longitude`, ve kterém tyto hodnoty obvykle vidíte.</span><span class="sxs-lookup"><span data-stu-id="1bab0-151">Note that this is **backwards** from the `latitude, longitude` format in which you typically see these values.</span></span>

### <a name="srid-ignored-during-client-operations"></a><span data-ttu-id="1bab0-152">SRID se během operací klienta ignorovat.</span><span class="sxs-lookup"><span data-stu-id="1bab0-152">SRID Ignored during client operations</span></span>

<span data-ttu-id="1bab0-153">NTS ignoruje hodnoty SRID během operací.</span><span class="sxs-lookup"><span data-stu-id="1bab0-153">NTS ignores SRID values during operations.</span></span> <span data-ttu-id="1bab0-154">Předpokládá planární souřadnicový systém.</span><span class="sxs-lookup"><span data-stu-id="1bab0-154">It assumes a planar coordinate system.</span></span> <span data-ttu-id="1bab0-155">To znamená, že pokud zadáte souřadnice z oblasti Zeměpisná délka a zeměpisná šířka, některé hodnoty vyhodnocené klientem, jako je vzdálenost, délka a oblast, budou ve stupních, nikoli měřičích.</span><span class="sxs-lookup"><span data-stu-id="1bab0-155">This means that if you specify coordinates in terms of longitude and latitude, some client-evaluated values like distance, length, and area will be in degrees, not meters.</span></span> <span data-ttu-id="1bab0-156">Pro smysluplnější hodnoty musíte nejprve před výpočtem těchto hodnot vyhodnotit souřadnice pro jiný systém souřadnic pomocí knihovny, jako je [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) .</span><span class="sxs-lookup"><span data-stu-id="1bab0-156">For more meaningful values, you first need to project the coordinates to another coordinate system using a library like [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) before calculating these values.</span></span>

<span data-ttu-id="1bab0-157">Pokud je operace vyhodnocena serverem pomocí SQL EF Core prostřednictvím SQL, určí se jednotka výsledku databáze.</span><span class="sxs-lookup"><span data-stu-id="1bab0-157">If an operation is server-evaluated by EF Core via SQL, the result's unit will be determined by the database.</span></span>

<span data-ttu-id="1bab0-158">Tady je příklad použití ProjNet4GeoAPI k výpočtu vzdálenosti mezi dvěma městy.</span><span class="sxs-lookup"><span data-stu-id="1bab0-158">Here is an example of using ProjNet4GeoAPI to calculate the distance between two cities.</span></span>

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

## <a name="querying-data"></a><span data-ttu-id="1bab0-159">Dotazování na data</span><span class="sxs-lookup"><span data-stu-id="1bab0-159">Querying Data</span></span>

<span data-ttu-id="1bab0-160">V jazyce LINQ budou metody a vlastnosti NTS dostupné jako databázové funkce přeloženy do jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="1bab0-160">In LINQ, the NTS methods and properties available as database functions will be translated to SQL.</span></span> <span data-ttu-id="1bab0-161">Například vzdálenost a obsahuje metody jsou přeloženy v následujících dotazech.</span><span class="sxs-lookup"><span data-stu-id="1bab0-161">For example, the Distance and Contains methods are translated in the following queries.</span></span> <span data-ttu-id="1bab0-162">Tabulka na konci tohoto článku ukazuje, kteří členové jsou podporováni různými poskytovateli EF Core.</span><span class="sxs-lookup"><span data-stu-id="1bab0-162">The table at the end of this article shows which members are supported by various EF Core providers.</span></span>

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a><span data-ttu-id="1bab0-163">Server SQL</span><span class="sxs-lookup"><span data-stu-id="1bab0-163">SQL Server</span></span>

<span data-ttu-id="1bab0-164">Pokud používáte SQL Server, máte k dispozici několik dalších věcí, o kterých byste měli vědět.</span><span class="sxs-lookup"><span data-stu-id="1bab0-164">If you're using SQL Server, there are some additional things you should be aware of.</span></span>

### <a name="geography-or-geometry"></a><span data-ttu-id="1bab0-165">Zeměpisná nebo geometrie</span><span class="sxs-lookup"><span data-stu-id="1bab0-165">Geography or geometry</span></span>

<span data-ttu-id="1bab0-166">Ve výchozím nastavení jsou prostorové vlastnosti namapovány na `geography` sloupce v SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1bab0-166">By default, spatial properties are mapped to `geography` columns in SQL Server.</span></span> <span data-ttu-id="1bab0-167">Pokud chcete použít `geometry`, nakonfigurujte v modelu [typ sloupce](xref:core/modeling/entity-properties#column-data-types) .</span><span class="sxs-lookup"><span data-stu-id="1bab0-167">To use `geometry`, [configure the column type](xref:core/modeling/entity-properties#column-data-types) in your model.</span></span>

### <a name="geography-polygon-rings"></a><span data-ttu-id="1bab0-168">Geografické kroužky mnohoúhelníků</span><span class="sxs-lookup"><span data-stu-id="1bab0-168">Geography polygon rings</span></span>

<span data-ttu-id="1bab0-169">Při použití `geography`ho typu sloupce SQL Server ukládá další požadavky na vnější prstenec (nebo prostředí) a vnitřní prstence (nebo díry).</span><span class="sxs-lookup"><span data-stu-id="1bab0-169">When using the `geography` column type, SQL Server imposes additional requirements on the exterior ring (or shell) and interior rings (or holes).</span></span> <span data-ttu-id="1bab0-170">Vnější prstenec musí být orientovaný proti směru hodinových ručiček a vnitřní prstence po směru hodinových ručiček.</span><span class="sxs-lookup"><span data-stu-id="1bab0-170">The exterior ring must be oriented counterclockwise and the interior rings clockwise.</span></span> <span data-ttu-id="1bab0-171">NTS ho před odesláním hodnot do databáze ověří.</span><span class="sxs-lookup"><span data-stu-id="1bab0-171">NTS validates this before sending values to the database.</span></span>

### <a name="fullglobe"></a><span data-ttu-id="1bab0-172">FullGlobe</span><span class="sxs-lookup"><span data-stu-id="1bab0-172">FullGlobe</span></span>

<span data-ttu-id="1bab0-173">SQL Server má nestandardní typ geometrie, který představuje úplný glóbus při použití `geography`ho typu sloupce.</span><span class="sxs-lookup"><span data-stu-id="1bab0-173">SQL Server has a non-standard geometry type to represent the full globe when using the `geography` column type.</span></span> <span data-ttu-id="1bab0-174">Má také způsob, jak znázornit mnohoúhelníky na základě plného světa (bez vnějšího okruhu).</span><span class="sxs-lookup"><span data-stu-id="1bab0-174">It also has a way to represent polygons based on the full globe (without an exterior ring).</span></span> <span data-ttu-id="1bab0-175">Ani jedna z těchto možností není podporována nástrojem NTS.</span><span class="sxs-lookup"><span data-stu-id="1bab0-175">Neither of these are supported by NTS.</span></span>

> [!WARNING]
> <span data-ttu-id="1bab0-176">FullGlobe a mnohoúhelníky, které jsou na nich založené, nejsou podporovány NTS.</span><span class="sxs-lookup"><span data-stu-id="1bab0-176">FullGlobe and polygons based on it aren't supported by NTS.</span></span>

## <a name="sqlite"></a><span data-ttu-id="1bab0-177">SQLite</span><span class="sxs-lookup"><span data-stu-id="1bab0-177">SQLite</span></span>

<span data-ttu-id="1bab0-178">Zde jsou některé další informace pro ty, které používají SQLite.</span><span class="sxs-lookup"><span data-stu-id="1bab0-178">Here is some additional information for those using SQLite.</span></span>

### <a name="installing-spatialite"></a><span data-ttu-id="1bab0-179">Instalace SpatiaLite</span><span class="sxs-lookup"><span data-stu-id="1bab0-179">Installing SpatiaLite</span></span>

<span data-ttu-id="1bab0-180">V systému Windows je nativní knihovna mod_spatialite distribuována jako závislost balíčku NuGet.</span><span class="sxs-lookup"><span data-stu-id="1bab0-180">On Windows, the native mod_spatialite library is distributed as a NuGet package dependency.</span></span> <span data-ttu-id="1bab0-181">Jiné platformy je potřeba nainstalovat samostatně.</span><span class="sxs-lookup"><span data-stu-id="1bab0-181">Other platforms need to install it separately.</span></span> <span data-ttu-id="1bab0-182">To se obvykle provádí pomocí Správce balíčků softwaru.</span><span class="sxs-lookup"><span data-stu-id="1bab0-182">This is typically done using a software package manager.</span></span> <span data-ttu-id="1bab0-183">Například můžete použít APT v Ubuntu a homebrew na MacOS.</span><span class="sxs-lookup"><span data-stu-id="1bab0-183">For example, you can use APT on Ubuntu and Homebrew on MacOS.</span></span>

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

<span data-ttu-id="1bab0-184">Bohužel novější verze PROJ (závislost SpatiaLite) jsou nekompatibilní s výchozí sadou [SQLitePCLRaw sady](/dotnet/standard/data/sqlite/custom-versions#bundles)EF.</span><span class="sxs-lookup"><span data-stu-id="1bab0-184">Unfortunately, newer versions of PROJ (a dependency of SpatiaLite) are incompatible with EF's default [SQLitePCLRaw bundle](/dotnet/standard/data/sqlite/custom-versions#bundles).</span></span> <span data-ttu-id="1bab0-185">Můžete to obejít buď vytvořením vlastního [poskytovatele SQLitePCLRaw](/dotnet/standard/data/sqlite/custom-versions#sqlitepclraw-providers) , který používá knihovnu systémových souborů, nebo můžete nainstalovat vlastní sestavení SpatiaLite, které zakazuje podporu proj.</span><span class="sxs-lookup"><span data-stu-id="1bab0-185">You can work around this by either creating a custom [SQLitePCLRaw provider](/dotnet/standard/data/sqlite/custom-versions#sqlitepclraw-providers) that uses the system SQLite library, or you can install a custom build of SpatiaLite disabling PROJ support.</span></span>

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

### <a name="configuring-srid"></a><span data-ttu-id="1bab0-186">Konfigurace SRID</span><span class="sxs-lookup"><span data-stu-id="1bab0-186">Configuring SRID</span></span>

<span data-ttu-id="1bab0-187">V SpatiaLite musí sloupce určovat SRID na sloupec.</span><span class="sxs-lookup"><span data-stu-id="1bab0-187">In SpatiaLite, columns need to specify an SRID per column.</span></span> <span data-ttu-id="1bab0-188">Výchozí SRID je `0`.</span><span class="sxs-lookup"><span data-stu-id="1bab0-188">The default SRID is `0`.</span></span> <span data-ttu-id="1bab0-189">Určete jiný SRID pomocí metody ForSqliteHasSrid.</span><span class="sxs-lookup"><span data-stu-id="1bab0-189">Specify a different SRID using the ForSqliteHasSrid method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a><span data-ttu-id="1bab0-190">Rozměr</span><span class="sxs-lookup"><span data-stu-id="1bab0-190">Dimension</span></span>

<span data-ttu-id="1bab0-191">Podobně jako u SRID je jako součást sloupce zadána také dimenze sloupce (nebo souřadnice).</span><span class="sxs-lookup"><span data-stu-id="1bab0-191">Similar to SRID, a column's dimension (or ordinates) is also specified as part of the column.</span></span> <span data-ttu-id="1bab0-192">Výchozí souřadnice jsou X a Y. Povolte další souřadnice (Z a M) pomocí metody ForSqliteHasDimension.</span><span class="sxs-lookup"><span data-stu-id="1bab0-192">The default ordinates are X and Y. Enable additional ordinates (Z and M) using the ForSqliteHasDimension method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a><span data-ttu-id="1bab0-193">Přeložené operace</span><span class="sxs-lookup"><span data-stu-id="1bab0-193">Translated Operations</span></span>

<span data-ttu-id="1bab0-194">Tato tabulka uvádí, které NTS členové jsou přeloženi do jazyka SQL každým poskytovatelem EF Core.</span><span class="sxs-lookup"><span data-stu-id="1bab0-194">This table shows which NTS members are translated into SQL by each EF Core provider.</span></span>

<span data-ttu-id="1bab0-195">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="1bab0-195">NetTopologySuite</span></span> | <span data-ttu-id="1bab0-196">SQL Server (geometrie)</span><span class="sxs-lookup"><span data-stu-id="1bab0-196">SQL Server (geometry)</span></span> | <span data-ttu-id="1bab0-197">SQL Server (geografie)</span><span class="sxs-lookup"><span data-stu-id="1bab0-197">SQL Server (geography)</span></span> | <span data-ttu-id="1bab0-198">SQLite</span><span class="sxs-lookup"><span data-stu-id="1bab0-198">SQLite</span></span> | <span data-ttu-id="1bab0-199">Npgsql</span><span class="sxs-lookup"><span data-stu-id="1bab0-199">Npgsql</span></span>
--- |:---:|:---:|:---:|:---:
<span data-ttu-id="1bab0-200">Geometrie. Area</span><span class="sxs-lookup"><span data-stu-id="1bab0-200">Geometry.Area</span></span> | <span data-ttu-id="1bab0-201">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-201">✔</span></span> | <span data-ttu-id="1bab0-202">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-202">✔</span></span> | <span data-ttu-id="1bab0-203">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-203">✔</span></span> | <span data-ttu-id="1bab0-204">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-204">✔</span></span>
<span data-ttu-id="1bab0-205">Geometry. AsBinary ()</span><span class="sxs-lookup"><span data-stu-id="1bab0-205">Geometry.AsBinary()</span></span> | <span data-ttu-id="1bab0-206">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-206">✔</span></span> | <span data-ttu-id="1bab0-207">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-207">✔</span></span> | <span data-ttu-id="1bab0-208">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-208">✔</span></span> | <span data-ttu-id="1bab0-209">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-209">✔</span></span>
<span data-ttu-id="1bab0-210">Geometry. AsText ()</span><span class="sxs-lookup"><span data-stu-id="1bab0-210">Geometry.AsText()</span></span> | <span data-ttu-id="1bab0-211">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-211">✔</span></span> | <span data-ttu-id="1bab0-212">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-212">✔</span></span> | <span data-ttu-id="1bab0-213">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-213">✔</span></span> | <span data-ttu-id="1bab0-214">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-214">✔</span></span>
<span data-ttu-id="1bab0-215">Geometrie. hranice</span><span class="sxs-lookup"><span data-stu-id="1bab0-215">Geometry.Boundary</span></span> | <span data-ttu-id="1bab0-216">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-216">✔</span></span> | | <span data-ttu-id="1bab0-217">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-217">✔</span></span> | <span data-ttu-id="1bab0-218">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-218">✔</span></span>
<span data-ttu-id="1bab0-219">Geometry. Buffer (Double)</span><span class="sxs-lookup"><span data-stu-id="1bab0-219">Geometry.Buffer(double)</span></span> | <span data-ttu-id="1bab0-220">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-220">✔</span></span> | <span data-ttu-id="1bab0-221">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-221">✔</span></span> | <span data-ttu-id="1bab0-222">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-222">✔</span></span> | <span data-ttu-id="1bab0-223">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-223">✔</span></span>
<span data-ttu-id="1bab0-224">Geometry. Buffer (Double, int)</span><span class="sxs-lookup"><span data-stu-id="1bab0-224">Geometry.Buffer(double, int)</span></span> | | | <span data-ttu-id="1bab0-225">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-225">✔</span></span> | <span data-ttu-id="1bab0-226">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-226">✔</span></span>
<span data-ttu-id="1bab0-227">Geometrie. těžiště</span><span class="sxs-lookup"><span data-stu-id="1bab0-227">Geometry.Centroid</span></span> | <span data-ttu-id="1bab0-228">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-228">✔</span></span> | | <span data-ttu-id="1bab0-229">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-229">✔</span></span> | <span data-ttu-id="1bab0-230">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-230">✔</span></span>
<span data-ttu-id="1bab0-231">Geometrie. Contains (geometrie)</span><span class="sxs-lookup"><span data-stu-id="1bab0-231">Geometry.Contains(Geometry)</span></span> | <span data-ttu-id="1bab0-232">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-232">✔</span></span> | <span data-ttu-id="1bab0-233">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-233">✔</span></span> | <span data-ttu-id="1bab0-234">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-234">✔</span></span> | <span data-ttu-id="1bab0-235">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-235">✔</span></span>
<span data-ttu-id="1bab0-236">Geometry. ConvexHull ()</span><span class="sxs-lookup"><span data-stu-id="1bab0-236">Geometry.ConvexHull()</span></span> | <span data-ttu-id="1bab0-237">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-237">✔</span></span> | <span data-ttu-id="1bab0-238">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-238">✔</span></span> | <span data-ttu-id="1bab0-239">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-239">✔</span></span> | <span data-ttu-id="1bab0-240">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-240">✔</span></span>
<span data-ttu-id="1bab0-241">Geometry. CoveredBy (geometrie)</span><span class="sxs-lookup"><span data-stu-id="1bab0-241">Geometry.CoveredBy(Geometry)</span></span> | | | <span data-ttu-id="1bab0-242">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-242">✔</span></span> | <span data-ttu-id="1bab0-243">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-243">✔</span></span>
<span data-ttu-id="1bab0-244">Geometry. pokrývání (geometrie)</span><span class="sxs-lookup"><span data-stu-id="1bab0-244">Geometry.Covers(Geometry)</span></span> | | | <span data-ttu-id="1bab0-245">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-245">✔</span></span> | <span data-ttu-id="1bab0-246">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-246">✔</span></span>
<span data-ttu-id="1bab0-247">Geometrie. křížení (geometrie)</span><span class="sxs-lookup"><span data-stu-id="1bab0-247">Geometry.Crosses(Geometry)</span></span> | <span data-ttu-id="1bab0-248">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-248">✔</span></span> | | <span data-ttu-id="1bab0-249">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-249">✔</span></span> | <span data-ttu-id="1bab0-250">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-250">✔</span></span>
<span data-ttu-id="1bab0-251">Geometry. rozdíl (geometrie)</span><span class="sxs-lookup"><span data-stu-id="1bab0-251">Geometry.Difference(Geometry)</span></span> | <span data-ttu-id="1bab0-252">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-252">✔</span></span> | <span data-ttu-id="1bab0-253">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-253">✔</span></span> | <span data-ttu-id="1bab0-254">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-254">✔</span></span> | <span data-ttu-id="1bab0-255">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-255">✔</span></span>
<span data-ttu-id="1bab0-256">Geometrie. Dimension</span><span class="sxs-lookup"><span data-stu-id="1bab0-256">Geometry.Dimension</span></span> | <span data-ttu-id="1bab0-257">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-257">✔</span></span> | <span data-ttu-id="1bab0-258">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-258">✔</span></span> | <span data-ttu-id="1bab0-259">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-259">✔</span></span> | <span data-ttu-id="1bab0-260">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-260">✔</span></span>
<span data-ttu-id="1bab0-261">Geometrie. unkloub (geometrie)</span><span class="sxs-lookup"><span data-stu-id="1bab0-261">Geometry.Disjoint(Geometry)</span></span> | <span data-ttu-id="1bab0-262">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-262">✔</span></span> | <span data-ttu-id="1bab0-263">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-263">✔</span></span> | <span data-ttu-id="1bab0-264">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-264">✔</span></span> | <span data-ttu-id="1bab0-265">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-265">✔</span></span>
<span data-ttu-id="1bab0-266">Geometrie. Distance (geometrie)</span><span class="sxs-lookup"><span data-stu-id="1bab0-266">Geometry.Distance(Geometry)</span></span> | <span data-ttu-id="1bab0-267">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-267">✔</span></span> | <span data-ttu-id="1bab0-268">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-268">✔</span></span> | <span data-ttu-id="1bab0-269">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-269">✔</span></span> | <span data-ttu-id="1bab0-270">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-270">✔</span></span>
<span data-ttu-id="1bab0-271">Geometrie. obálky</span><span class="sxs-lookup"><span data-stu-id="1bab0-271">Geometry.Envelope</span></span> | <span data-ttu-id="1bab0-272">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-272">✔</span></span> | | <span data-ttu-id="1bab0-273">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-273">✔</span></span> | <span data-ttu-id="1bab0-274">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-274">✔</span></span>
<span data-ttu-id="1bab0-275">Geometry. EqualsExact (geometrie)</span><span class="sxs-lookup"><span data-stu-id="1bab0-275">Geometry.EqualsExact(Geometry)</span></span> | | | | <span data-ttu-id="1bab0-276">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-276">✔</span></span>
<span data-ttu-id="1bab0-277">Geometry. EqualsTopologically (geometrie)</span><span class="sxs-lookup"><span data-stu-id="1bab0-277">Geometry.EqualsTopologically(Geometry)</span></span> | <span data-ttu-id="1bab0-278">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-278">✔</span></span> | <span data-ttu-id="1bab0-279">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-279">✔</span></span> | <span data-ttu-id="1bab0-280">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-280">✔</span></span> | <span data-ttu-id="1bab0-281">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-281">✔</span></span>
<span data-ttu-id="1bab0-282">Geometrie. GeometryType</span><span class="sxs-lookup"><span data-stu-id="1bab0-282">Geometry.GeometryType</span></span> | <span data-ttu-id="1bab0-283">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-283">✔</span></span> | <span data-ttu-id="1bab0-284">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-284">✔</span></span> | <span data-ttu-id="1bab0-285">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-285">✔</span></span> | <span data-ttu-id="1bab0-286">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-286">✔</span></span>
<span data-ttu-id="1bab0-287">Geometry. GetGeometryN (int)</span><span class="sxs-lookup"><span data-stu-id="1bab0-287">Geometry.GetGeometryN(int)</span></span> | <span data-ttu-id="1bab0-288">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-288">✔</span></span> | | <span data-ttu-id="1bab0-289">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-289">✔</span></span> | <span data-ttu-id="1bab0-290">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-290">✔</span></span>
<span data-ttu-id="1bab0-291">Geometrie. InteriorPoint</span><span class="sxs-lookup"><span data-stu-id="1bab0-291">Geometry.InteriorPoint</span></span> | <span data-ttu-id="1bab0-292">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-292">✔</span></span> | | <span data-ttu-id="1bab0-293">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-293">✔</span></span> | <span data-ttu-id="1bab0-294">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-294">✔</span></span>
<span data-ttu-id="1bab0-295">Geometry. proprůsečík (geometrie)</span><span class="sxs-lookup"><span data-stu-id="1bab0-295">Geometry.Intersection(Geometry)</span></span> | <span data-ttu-id="1bab0-296">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-296">✔</span></span> | <span data-ttu-id="1bab0-297">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-297">✔</span></span> | <span data-ttu-id="1bab0-298">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-298">✔</span></span> | <span data-ttu-id="1bab0-299">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-299">✔</span></span>
<span data-ttu-id="1bab0-300">Geometrie. INTERSECTY (geometrie)</span><span class="sxs-lookup"><span data-stu-id="1bab0-300">Geometry.Intersects(Geometry)</span></span> | <span data-ttu-id="1bab0-301">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-301">✔</span></span> | <span data-ttu-id="1bab0-302">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-302">✔</span></span> | <span data-ttu-id="1bab0-303">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-303">✔</span></span> | <span data-ttu-id="1bab0-304">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-304">✔</span></span>
<span data-ttu-id="1bab0-305">Geometrie. Empty</span><span class="sxs-lookup"><span data-stu-id="1bab0-305">Geometry.IsEmpty</span></span> | <span data-ttu-id="1bab0-306">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-306">✔</span></span> | <span data-ttu-id="1bab0-307">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-307">✔</span></span> | <span data-ttu-id="1bab0-308">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-308">✔</span></span> | <span data-ttu-id="1bab0-309">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-309">✔</span></span>
<span data-ttu-id="1bab0-310">Geometrie. zjednodušená</span><span class="sxs-lookup"><span data-stu-id="1bab0-310">Geometry.IsSimple</span></span> | <span data-ttu-id="1bab0-311">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-311">✔</span></span> | | <span data-ttu-id="1bab0-312">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-312">✔</span></span> | <span data-ttu-id="1bab0-313">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-313">✔</span></span>
<span data-ttu-id="1bab0-314">Geometrie. IsValid</span><span class="sxs-lookup"><span data-stu-id="1bab0-314">Geometry.IsValid</span></span> | <span data-ttu-id="1bab0-315">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-315">✔</span></span> | <span data-ttu-id="1bab0-316">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-316">✔</span></span> | <span data-ttu-id="1bab0-317">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-317">✔</span></span> | <span data-ttu-id="1bab0-318">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-318">✔</span></span>
<span data-ttu-id="1bab0-319">Geometry. IsWithinDistance (geometrie, Double)</span><span class="sxs-lookup"><span data-stu-id="1bab0-319">Geometry.IsWithinDistance(Geometry, double)</span></span> | <span data-ttu-id="1bab0-320">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-320">✔</span></span> | | <span data-ttu-id="1bab0-321">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-321">✔</span></span> | <span data-ttu-id="1bab0-322">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-322">✔</span></span>
<span data-ttu-id="1bab0-323">Geometrie. Length</span><span class="sxs-lookup"><span data-stu-id="1bab0-323">Geometry.Length</span></span> | <span data-ttu-id="1bab0-324">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-324">✔</span></span> | <span data-ttu-id="1bab0-325">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-325">✔</span></span> | <span data-ttu-id="1bab0-326">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-326">✔</span></span> | <span data-ttu-id="1bab0-327">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-327">✔</span></span>
<span data-ttu-id="1bab0-328">Geometrie. NumGeometries</span><span class="sxs-lookup"><span data-stu-id="1bab0-328">Geometry.NumGeometries</span></span> | <span data-ttu-id="1bab0-329">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-329">✔</span></span> | <span data-ttu-id="1bab0-330">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-330">✔</span></span> | <span data-ttu-id="1bab0-331">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-331">✔</span></span> | <span data-ttu-id="1bab0-332">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-332">✔</span></span>
<span data-ttu-id="1bab0-333">Geometrie. NumPoints</span><span class="sxs-lookup"><span data-stu-id="1bab0-333">Geometry.NumPoints</span></span> | <span data-ttu-id="1bab0-334">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-334">✔</span></span> | <span data-ttu-id="1bab0-335">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-335">✔</span></span> | <span data-ttu-id="1bab0-336">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-336">✔</span></span> | <span data-ttu-id="1bab0-337">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-337">✔</span></span>
<span data-ttu-id="1bab0-338">Geometrie. OgcGeometryType</span><span class="sxs-lookup"><span data-stu-id="1bab0-338">Geometry.OgcGeometryType</span></span> | <span data-ttu-id="1bab0-339">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-339">✔</span></span> | <span data-ttu-id="1bab0-340">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-340">✔</span></span> | <span data-ttu-id="1bab0-341">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-341">✔</span></span> | <span data-ttu-id="1bab0-342">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-342">✔</span></span>
<span data-ttu-id="1bab0-343">Geometrie. překryvy (geometrie)</span><span class="sxs-lookup"><span data-stu-id="1bab0-343">Geometry.Overlaps(Geometry)</span></span> | <span data-ttu-id="1bab0-344">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-344">✔</span></span> | <span data-ttu-id="1bab0-345">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-345">✔</span></span> | <span data-ttu-id="1bab0-346">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-346">✔</span></span> | <span data-ttu-id="1bab0-347">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-347">✔</span></span>
<span data-ttu-id="1bab0-348">Geometrie. PointOnSurface</span><span class="sxs-lookup"><span data-stu-id="1bab0-348">Geometry.PointOnSurface</span></span> | <span data-ttu-id="1bab0-349">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-349">✔</span></span> | | <span data-ttu-id="1bab0-350">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-350">✔</span></span> | <span data-ttu-id="1bab0-351">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-351">✔</span></span>
<span data-ttu-id="1bab0-352">Geometry. propojovat (geometrie, String)</span><span class="sxs-lookup"><span data-stu-id="1bab0-352">Geometry.Relate(Geometry, string)</span></span> | <span data-ttu-id="1bab0-353">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-353">✔</span></span> | | <span data-ttu-id="1bab0-354">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-354">✔</span></span> | <span data-ttu-id="1bab0-355">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-355">✔</span></span>
<span data-ttu-id="1bab0-356">Geometry. Reverse ()</span><span class="sxs-lookup"><span data-stu-id="1bab0-356">Geometry.Reverse()</span></span> | | | <span data-ttu-id="1bab0-357">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-357">✔</span></span> | <span data-ttu-id="1bab0-358">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-358">✔</span></span>
<span data-ttu-id="1bab0-359">Geometrie. SRID</span><span class="sxs-lookup"><span data-stu-id="1bab0-359">Geometry.SRID</span></span> | <span data-ttu-id="1bab0-360">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-360">✔</span></span> | <span data-ttu-id="1bab0-361">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-361">✔</span></span> | <span data-ttu-id="1bab0-362">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-362">✔</span></span> | <span data-ttu-id="1bab0-363">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-363">✔</span></span>
<span data-ttu-id="1bab0-364">Geometry. SymmetricDifference (geometrie)</span><span class="sxs-lookup"><span data-stu-id="1bab0-364">Geometry.SymmetricDifference(Geometry)</span></span> | <span data-ttu-id="1bab0-365">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-365">✔</span></span> | <span data-ttu-id="1bab0-366">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-366">✔</span></span> | <span data-ttu-id="1bab0-367">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-367">✔</span></span> | <span data-ttu-id="1bab0-368">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-368">✔</span></span>
<span data-ttu-id="1bab0-369">Geometry. ToBinary ()</span><span class="sxs-lookup"><span data-stu-id="1bab0-369">Geometry.ToBinary()</span></span> | <span data-ttu-id="1bab0-370">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-370">✔</span></span> | <span data-ttu-id="1bab0-371">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-371">✔</span></span> | <span data-ttu-id="1bab0-372">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-372">✔</span></span> | <span data-ttu-id="1bab0-373">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-373">✔</span></span>
<span data-ttu-id="1bab0-374">Geometry. ToText ()</span><span class="sxs-lookup"><span data-stu-id="1bab0-374">Geometry.ToText()</span></span> | <span data-ttu-id="1bab0-375">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-375">✔</span></span> | <span data-ttu-id="1bab0-376">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-376">✔</span></span> | <span data-ttu-id="1bab0-377">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-377">✔</span></span> | <span data-ttu-id="1bab0-378">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-378">✔</span></span>
<span data-ttu-id="1bab0-379">Geometrie. touchs (geometrie)</span><span class="sxs-lookup"><span data-stu-id="1bab0-379">Geometry.Touches(Geometry)</span></span> | <span data-ttu-id="1bab0-380">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-380">✔</span></span> | | <span data-ttu-id="1bab0-381">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-381">✔</span></span> | <span data-ttu-id="1bab0-382">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-382">✔</span></span>
<span data-ttu-id="1bab0-383">Geometry. Union ()</span><span class="sxs-lookup"><span data-stu-id="1bab0-383">Geometry.Union()</span></span> | | | <span data-ttu-id="1bab0-384">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-384">✔</span></span> | <span data-ttu-id="1bab0-385">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-385">✔</span></span>
<span data-ttu-id="1bab0-386">Geometry. Union (geometrie)</span><span class="sxs-lookup"><span data-stu-id="1bab0-386">Geometry.Union(Geometry)</span></span> | <span data-ttu-id="1bab0-387">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-387">✔</span></span> | <span data-ttu-id="1bab0-388">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-388">✔</span></span> | <span data-ttu-id="1bab0-389">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-389">✔</span></span> | <span data-ttu-id="1bab0-390">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-390">✔</span></span>
<span data-ttu-id="1bab0-391">Geometrie. uvnitř (geometrie)</span><span class="sxs-lookup"><span data-stu-id="1bab0-391">Geometry.Within(Geometry)</span></span> | <span data-ttu-id="1bab0-392">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-392">✔</span></span> | <span data-ttu-id="1bab0-393">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-393">✔</span></span> | <span data-ttu-id="1bab0-394">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-394">✔</span></span> | <span data-ttu-id="1bab0-395">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-395">✔</span></span>
<span data-ttu-id="1bab0-396">Geometriecollection. Count</span><span class="sxs-lookup"><span data-stu-id="1bab0-396">GeometryCollection.Count</span></span> | <span data-ttu-id="1bab0-397">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-397">✔</span></span> | <span data-ttu-id="1bab0-398">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-398">✔</span></span> | <span data-ttu-id="1bab0-399">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-399">✔</span></span> | <span data-ttu-id="1bab0-400">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-400">✔</span></span>
<span data-ttu-id="1bab0-401">GeometryCollection [int]</span><span class="sxs-lookup"><span data-stu-id="1bab0-401">GeometryCollection[int]</span></span> | <span data-ttu-id="1bab0-402">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-402">✔</span></span> | <span data-ttu-id="1bab0-403">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-403">✔</span></span> | <span data-ttu-id="1bab0-404">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-404">✔</span></span> | <span data-ttu-id="1bab0-405">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-405">✔</span></span>
<span data-ttu-id="1bab0-406">LineString. Count</span><span class="sxs-lookup"><span data-stu-id="1bab0-406">LineString.Count</span></span> | <span data-ttu-id="1bab0-407">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-407">✔</span></span> | <span data-ttu-id="1bab0-408">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-408">✔</span></span> | <span data-ttu-id="1bab0-409">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-409">✔</span></span> | <span data-ttu-id="1bab0-410">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-410">✔</span></span>
<span data-ttu-id="1bab0-411">LineString. EndPoint</span><span class="sxs-lookup"><span data-stu-id="1bab0-411">LineString.EndPoint</span></span> | <span data-ttu-id="1bab0-412">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-412">✔</span></span> | <span data-ttu-id="1bab0-413">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-413">✔</span></span> | <span data-ttu-id="1bab0-414">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-414">✔</span></span> | <span data-ttu-id="1bab0-415">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-415">✔</span></span>
<span data-ttu-id="1bab0-416">LineString. GetPointN (int)</span><span class="sxs-lookup"><span data-stu-id="1bab0-416">LineString.GetPointN(int)</span></span> | <span data-ttu-id="1bab0-417">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-417">✔</span></span> | <span data-ttu-id="1bab0-418">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-418">✔</span></span> | <span data-ttu-id="1bab0-419">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-419">✔</span></span> | <span data-ttu-id="1bab0-420">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-420">✔</span></span>
<span data-ttu-id="1bab0-421">LineString. uzavřeno</span><span class="sxs-lookup"><span data-stu-id="1bab0-421">LineString.IsClosed</span></span> | <span data-ttu-id="1bab0-422">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-422">✔</span></span> | <span data-ttu-id="1bab0-423">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-423">✔</span></span> | <span data-ttu-id="1bab0-424">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-424">✔</span></span> | <span data-ttu-id="1bab0-425">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-425">✔</span></span>
<span data-ttu-id="1bab0-426">LineString. pozvonění</span><span class="sxs-lookup"><span data-stu-id="1bab0-426">LineString.IsRing</span></span> | <span data-ttu-id="1bab0-427">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-427">✔</span></span> | | <span data-ttu-id="1bab0-428">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-428">✔</span></span> | <span data-ttu-id="1bab0-429">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-429">✔</span></span>
<span data-ttu-id="1bab0-430">LineString. StartPoint</span><span class="sxs-lookup"><span data-stu-id="1bab0-430">LineString.StartPoint</span></span> | <span data-ttu-id="1bab0-431">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-431">✔</span></span> | <span data-ttu-id="1bab0-432">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-432">✔</span></span> | <span data-ttu-id="1bab0-433">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-433">✔</span></span> | <span data-ttu-id="1bab0-434">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-434">✔</span></span>
<span data-ttu-id="1bab0-435">MultiLineString. uzavřeno</span><span class="sxs-lookup"><span data-stu-id="1bab0-435">MultiLineString.IsClosed</span></span> | <span data-ttu-id="1bab0-436">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-436">✔</span></span> | <span data-ttu-id="1bab0-437">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-437">✔</span></span> | <span data-ttu-id="1bab0-438">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-438">✔</span></span> | <span data-ttu-id="1bab0-439">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-439">✔</span></span>
<span data-ttu-id="1bab0-440">Point. M</span><span class="sxs-lookup"><span data-stu-id="1bab0-440">Point.M</span></span> | <span data-ttu-id="1bab0-441">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-441">✔</span></span> | <span data-ttu-id="1bab0-442">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-442">✔</span></span> | <span data-ttu-id="1bab0-443">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-443">✔</span></span> | <span data-ttu-id="1bab0-444">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-444">✔</span></span>
<span data-ttu-id="1bab0-445">Point. X</span><span class="sxs-lookup"><span data-stu-id="1bab0-445">Point.X</span></span> | <span data-ttu-id="1bab0-446">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-446">✔</span></span> | <span data-ttu-id="1bab0-447">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-447">✔</span></span> | <span data-ttu-id="1bab0-448">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-448">✔</span></span> | <span data-ttu-id="1bab0-449">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-449">✔</span></span>
<span data-ttu-id="1bab0-450">Point. Y</span><span class="sxs-lookup"><span data-stu-id="1bab0-450">Point.Y</span></span> | <span data-ttu-id="1bab0-451">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-451">✔</span></span> | <span data-ttu-id="1bab0-452">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-452">✔</span></span> | <span data-ttu-id="1bab0-453">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-453">✔</span></span> | <span data-ttu-id="1bab0-454">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-454">✔</span></span>
<span data-ttu-id="1bab0-455">Point. Z</span><span class="sxs-lookup"><span data-stu-id="1bab0-455">Point.Z</span></span> | <span data-ttu-id="1bab0-456">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-456">✔</span></span> | <span data-ttu-id="1bab0-457">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-457">✔</span></span> | <span data-ttu-id="1bab0-458">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-458">✔</span></span> | <span data-ttu-id="1bab0-459">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-459">✔</span></span>
<span data-ttu-id="1bab0-460">Mnohoúhelník. ExteriorRing</span><span class="sxs-lookup"><span data-stu-id="1bab0-460">Polygon.ExteriorRing</span></span> | <span data-ttu-id="1bab0-461">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-461">✔</span></span> | <span data-ttu-id="1bab0-462">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-462">✔</span></span> | <span data-ttu-id="1bab0-463">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-463">✔</span></span> | <span data-ttu-id="1bab0-464">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-464">✔</span></span>
<span data-ttu-id="1bab0-465">Mnohoúhelník. GetInteriorRingN (int)</span><span class="sxs-lookup"><span data-stu-id="1bab0-465">Polygon.GetInteriorRingN(int)</span></span> | <span data-ttu-id="1bab0-466">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-466">✔</span></span> | <span data-ttu-id="1bab0-467">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-467">✔</span></span> | <span data-ttu-id="1bab0-468">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-468">✔</span></span> | <span data-ttu-id="1bab0-469">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-469">✔</span></span>
<span data-ttu-id="1bab0-470">Mnohoúhelník. NumInteriorRings</span><span class="sxs-lookup"><span data-stu-id="1bab0-470">Polygon.NumInteriorRings</span></span> | <span data-ttu-id="1bab0-471">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-471">✔</span></span> | <span data-ttu-id="1bab0-472">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-472">✔</span></span> | <span data-ttu-id="1bab0-473">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-473">✔</span></span> | <span data-ttu-id="1bab0-474">✔</span><span class="sxs-lookup"><span data-stu-id="1bab0-474">✔</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1bab0-475">Další materiály a zdroje informací</span><span class="sxs-lookup"><span data-stu-id="1bab0-475">Additional resources</span></span>

* [<span data-ttu-id="1bab0-476">Prostorová data v SQL Server</span><span class="sxs-lookup"><span data-stu-id="1bab0-476">Spatial Data in SQL Server</span></span>](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [<span data-ttu-id="1bab0-477">Domovská stránka SpatiaLite</span><span class="sxs-lookup"><span data-stu-id="1bab0-477">SpatiaLite Homepage</span></span>](https://www.gaia-gis.it/fossil/libspatialite)
* [<span data-ttu-id="1bab0-478">Npgsql prostorová dokumentace</span><span class="sxs-lookup"><span data-stu-id="1bab0-478">Npgsql Spatial Documentation</span></span>](https://www.npgsql.org/efcore/mapping/nts.html)
* [<span data-ttu-id="1bab0-479">Dokumentace k PostGIS</span><span class="sxs-lookup"><span data-stu-id="1bab0-479">PostGIS Documentation</span></span>](https://postgis.net/documentation/)
