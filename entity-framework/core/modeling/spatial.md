---
title: Prostorová data – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: 335d4f3a601624f7c994b7dcacefe4ef6798beb3
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655603"
---
# <a name="spatial-data"></a><span data-ttu-id="e24a4-102">Prostorová data</span><span class="sxs-lookup"><span data-stu-id="e24a4-102">Spatial Data</span></span>

> [!NOTE]
> <span data-ttu-id="e24a4-103">Tato funkce se přidala do EF Core 2,2.</span><span class="sxs-lookup"><span data-stu-id="e24a4-103">This feature was added in EF Core 2.2.</span></span>

<span data-ttu-id="e24a4-104">Prostorová data představují fyzické umístění a tvar objektů.</span><span class="sxs-lookup"><span data-stu-id="e24a4-104">Spatial data represents the physical location and the shape of objects.</span></span> <span data-ttu-id="e24a4-105">Mnohé databáze poskytují podporu pro tento typ dat, aby je bylo možné indexovat a dotazovat společně s ostatními daty.</span><span class="sxs-lookup"><span data-stu-id="e24a4-105">Many databases provide support for this type of data so it can be indexed and queried alongside other data.</span></span> <span data-ttu-id="e24a4-106">Mezi běžné scénáře patří dotazování pro objekty v dané vzdálenosti od místa nebo výběr objektu, jehož ohraničení obsahuje dané umístění.</span><span class="sxs-lookup"><span data-stu-id="e24a4-106">Common scenarios include querying for objects within a given distance from a location, or selecting the object whose border contains a given location.</span></span> <span data-ttu-id="e24a4-107">EF Core podporuje mapování na prostorové datové typy pomocí knihovny prostorů [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) .</span><span class="sxs-lookup"><span data-stu-id="e24a4-107">EF Core supports mapping to spatial data types using the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) spatial library.</span></span>

## <a name="installing"></a><span data-ttu-id="e24a4-108">Instalují</span><span class="sxs-lookup"><span data-stu-id="e24a4-108">Installing</span></span>

<span data-ttu-id="e24a4-109">Aby bylo možné použít prostorová data s EF Core, je nutné nainstalovat příslušný podpůrný balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="e24a4-109">In order to use spatial data with EF Core, you need to install the appropriate supporting NuGet package.</span></span> <span data-ttu-id="e24a4-110">Který balíček, který potřebujete nainstalovat, závisí na používaném poskytovateli.</span><span class="sxs-lookup"><span data-stu-id="e24a4-110">Which package you need to install depends on the provider you're using.</span></span>

<span data-ttu-id="e24a4-111">Poskytovatel EF Core</span><span class="sxs-lookup"><span data-stu-id="e24a4-111">EF Core Provider</span></span>                        | <span data-ttu-id="e24a4-112">Prostorový balíček NuGet</span><span class="sxs-lookup"><span data-stu-id="e24a4-112">Spatial NuGet Package</span></span>
--------------------------------------- | ---------------------
<span data-ttu-id="e24a4-113">Microsoft. EntityFrameworkCore. SqlServer</span><span class="sxs-lookup"><span data-stu-id="e24a4-113">Microsoft.EntityFrameworkCore.SqlServer</span></span> | [<span data-ttu-id="e24a4-114">Microsoft. EntityFrameworkCore. SqlServer. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="e24a4-114">Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
<span data-ttu-id="e24a4-115">Microsoft. EntityFrameworkCore. sqlite</span><span class="sxs-lookup"><span data-stu-id="e24a4-115">Microsoft.EntityFrameworkCore.Sqlite</span></span>    | [<span data-ttu-id="e24a4-116">Microsoft. EntityFrameworkCore. sqlite. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="e24a4-116">Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
<span data-ttu-id="e24a4-117">Microsoft. EntityFrameworkCore. inMemory</span><span class="sxs-lookup"><span data-stu-id="e24a4-117">Microsoft.EntityFrameworkCore.InMemory</span></span>  | [<span data-ttu-id="e24a4-118">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="e24a4-118">NetTopologySuite</span></span>](https://www.nuget.org/packages/NetTopologySuite)
<span data-ttu-id="e24a4-119">Npgsql. EntityFrameworkCore. PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="e24a4-119">Npgsql.EntityFrameworkCore.PostgreSQL</span></span>   | [<span data-ttu-id="e24a4-120">Npgsql. EntityFrameworkCore. PostgreSQL. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="e24a4-120">Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite</span></span>](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a><span data-ttu-id="e24a4-121">Zpětná analýza</span><span class="sxs-lookup"><span data-stu-id="e24a4-121">Reverse engineering</span></span>

<span data-ttu-id="e24a4-122">Prostorové balíčky NuGet také umožňují modely [zpětné analýzy](../managing-schemas/scaffolding.md) s prostorovými vlastnostmi, ale ***před*** spuštěním `Scaffold-DbContext` nebo `dotnet ef dbcontext scaffold`je potřeba balíček nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="e24a4-122">The spatial NuGet packages also enable [reverse engineering](../managing-schemas/scaffolding.md) models with spatial properties, but you need to install the package ***before*** running `Scaffold-DbContext` or `dotnet ef dbcontext scaffold`.</span></span> <span data-ttu-id="e24a4-123">Pokud to neuděláte, zobrazí se upozornění týkající se nehledání mapování typů pro sloupce a sloupce se přeskočí.</span><span class="sxs-lookup"><span data-stu-id="e24a4-123">If you don't, you'll receive warnings about not finding type mappings for the columns and the columns will be skipped.</span></span>

## <a name="nettopologysuite-nts"></a><span data-ttu-id="e24a4-124">NetTopologySuite (NTS)</span><span class="sxs-lookup"><span data-stu-id="e24a4-124">NetTopologySuite (NTS)</span></span>

<span data-ttu-id="e24a4-125">NetTopologySuite je prostorová knihovna pro .NET.</span><span class="sxs-lookup"><span data-stu-id="e24a4-125">NetTopologySuite is a spatial library for .NET.</span></span> <span data-ttu-id="e24a4-126">EF Core umožňuje mapování na prostorové datové typy v databázi pomocí typů NTS v modelu.</span><span class="sxs-lookup"><span data-stu-id="e24a4-126">EF Core enables mapping to spatial data types in the database by using NTS types in your model.</span></span>

<span data-ttu-id="e24a4-127">Chcete-li povolit mapování na prostorové typy prostřednictvím NTS, volejte metodu UseNetTopologySuite na tvůrci možností DbContext poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="e24a4-127">To enable mapping to spatial types via NTS, call the UseNetTopologySuite method on the provider's DbContext options builder.</span></span> <span data-ttu-id="e24a4-128">Například pomocí SQL Server byste to rádi volali.</span><span class="sxs-lookup"><span data-stu-id="e24a4-128">For example, with SQL Server you'd call it like this.</span></span>

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

<span data-ttu-id="e24a4-129">Existuje několik prostorových datových typů.</span><span class="sxs-lookup"><span data-stu-id="e24a4-129">There are several spatial data types.</span></span> <span data-ttu-id="e24a4-130">Typ, který použijete, závisí na typech tvarů, které chcete zapnout.</span><span class="sxs-lookup"><span data-stu-id="e24a4-130">Which type you use depends on the types of shapes you want to allow.</span></span> <span data-ttu-id="e24a4-131">Tady je hierarchie typů NTS, které můžete použít pro vlastnosti v modelu.</span><span class="sxs-lookup"><span data-stu-id="e24a4-131">Here is the hierarchy of NTS types that you can use for properties in your model.</span></span> <span data-ttu-id="e24a4-132">Nacházejí se v oboru názvů `NetTopologySuite.Geometries`.</span><span class="sxs-lookup"><span data-stu-id="e24a4-132">They're located within the `NetTopologySuite.Geometries` namespace.</span></span>

* <span data-ttu-id="e24a4-133">Geometrie</span><span class="sxs-lookup"><span data-stu-id="e24a4-133">Geometry</span></span>
  * <span data-ttu-id="e24a4-134">Vyberte</span><span class="sxs-lookup"><span data-stu-id="e24a4-134">Point</span></span>
  * <span data-ttu-id="e24a4-135">LineString</span><span class="sxs-lookup"><span data-stu-id="e24a4-135">LineString</span></span>
  * <span data-ttu-id="e24a4-136">Postupně</span><span class="sxs-lookup"><span data-stu-id="e24a4-136">Polygon</span></span>
  * <span data-ttu-id="e24a4-137">GeometryCollection</span><span class="sxs-lookup"><span data-stu-id="e24a4-137">GeometryCollection</span></span>
    * <span data-ttu-id="e24a4-138">MultiPoint</span><span class="sxs-lookup"><span data-stu-id="e24a4-138">MultiPoint</span></span>
    * <span data-ttu-id="e24a4-139">MultiLineString</span><span class="sxs-lookup"><span data-stu-id="e24a4-139">MultiLineString</span></span>
    * <span data-ttu-id="e24a4-140">MultiPolygon</span><span class="sxs-lookup"><span data-stu-id="e24a4-140">MultiPolygon</span></span>

> [!WARNING]
> <span data-ttu-id="e24a4-141">CircularString, CompoundCurve a CurePolygon nejsou podporovány NTS.</span><span class="sxs-lookup"><span data-stu-id="e24a4-141">CircularString, CompoundCurve, and CurePolygon aren't supported by NTS.</span></span>

<span data-ttu-id="e24a4-142">Použití typu základní geometrie umožňuje, aby byl jakýkoli typ obrazce určen vlastností.</span><span class="sxs-lookup"><span data-stu-id="e24a4-142">Using the base Geometry type allows any type of shape to be specified by the property.</span></span>

<span data-ttu-id="e24a4-143">Následující třídy entit se dají použít k mapování tabulek v [ukázkové databázi World World Imports](https://go.microsoft.com/fwlink/?LinkID=800630).</span><span class="sxs-lookup"><span data-stu-id="e24a4-143">The following entity classes could be used to map to tables in the [Wide World Importers sample database](https://go.microsoft.com/fwlink/?LinkID=800630).</span></span>

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

### <a name="creating-values"></a><span data-ttu-id="e24a4-144">Vytváření hodnot</span><span class="sxs-lookup"><span data-stu-id="e24a4-144">Creating values</span></span>

<span data-ttu-id="e24a4-145">Konstruktory lze použít k vytvoření objektů geometrie; NTS však doporučuje použít místo toho objekt pro vytváření geometrie.</span><span class="sxs-lookup"><span data-stu-id="e24a4-145">You can use constructors to create geometry objects; however, NTS recommends using a geometry factory instead.</span></span> <span data-ttu-id="e24a4-146">To vám umožní určit výchozí SRID (prostorový referenční systém používaný souřadnicemi) a vám umožní ovládat pokročilejší věci, jako je model přesnosti (použitý během výpočtů) a sekvence souřadnic (určuje, které souřadnice--Dimensions a míry – jsou k dispozici).</span><span class="sxs-lookup"><span data-stu-id="e24a4-146">This lets you specify a default SRID (the spatial reference system used by the coordinates) and gives you control over more advanced things like the precision model (used during calculations) and the coordinate sequence (determines which ordinates--dimensions and measures--are available).</span></span>

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> <span data-ttu-id="e24a4-147">4326 odkazuje na WGS 84, Standard používaný v GPS a dalších geografických systémech.</span><span class="sxs-lookup"><span data-stu-id="e24a4-147">4326 refers to WGS 84, a standard used in GPS and other geographic systems.</span></span>

### <a name="longitude-and-latitude"></a><span data-ttu-id="e24a4-148">Zeměpisná délka a zeměpisná šířka</span><span class="sxs-lookup"><span data-stu-id="e24a4-148">Longitude and Latitude</span></span>

<span data-ttu-id="e24a4-149">Souřadnice v NTS jsou vyhledané v hodnotách X a Y.</span><span class="sxs-lookup"><span data-stu-id="e24a4-149">Coordinates in NTS are in terms of X and Y values.</span></span> <span data-ttu-id="e24a4-150">Aby představoval zeměpisnou délku a zeměpisnou šířku, použijte X pro zeměpisnou délku a Y pro zeměpisnou šířku.</span><span class="sxs-lookup"><span data-stu-id="e24a4-150">To represent longitude and latitude, use X for longitude and Y for latitude.</span></span> <span data-ttu-id="e24a4-151">Všimněte si, že se jedná o **zpětnou** hodnotu z formátu `latitude, longitude`, ve kterém tyto hodnoty obvykle vidíte.</span><span class="sxs-lookup"><span data-stu-id="e24a4-151">Note that this is **backwards** from the `latitude, longitude` format in which you typically see these values.</span></span>

### <a name="srid-ignored-during-client-operations"></a><span data-ttu-id="e24a4-152">SRID se během operací klienta ignorovat.</span><span class="sxs-lookup"><span data-stu-id="e24a4-152">SRID Ignored during client operations</span></span>

<span data-ttu-id="e24a4-153">NTS ignoruje hodnoty SRID během operací.</span><span class="sxs-lookup"><span data-stu-id="e24a4-153">NTS ignores SRID values during operations.</span></span> <span data-ttu-id="e24a4-154">Předpokládá planární souřadnicový systém.</span><span class="sxs-lookup"><span data-stu-id="e24a4-154">It assumes a planar coordinate system.</span></span> <span data-ttu-id="e24a4-155">To znamená, že pokud zadáte souřadnice z oblasti Zeměpisná délka a zeměpisná šířka, některé hodnoty vyhodnocené klientem, jako je vzdálenost, délka a oblast, budou ve stupních, nikoli měřičích.</span><span class="sxs-lookup"><span data-stu-id="e24a4-155">This means that if you specify coordinates in terms of longitude and latitude, some client-evaluated values like distance, length, and area will be in degrees, not meters.</span></span> <span data-ttu-id="e24a4-156">Pro smysluplnější hodnoty musíte nejprve před výpočtem těchto hodnot vyhodnotit souřadnice pro jiný systém souřadnic pomocí knihovny, jako je [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) .</span><span class="sxs-lookup"><span data-stu-id="e24a4-156">For more meaningful values, you first need to project the coordinates to another coordinate system using a library like [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) before calculating these values.</span></span>

<span data-ttu-id="e24a4-157">Pokud je operace vyhodnocena serverem pomocí SQL EF Core prostřednictvím SQL, určí se jednotka výsledku databáze.</span><span class="sxs-lookup"><span data-stu-id="e24a4-157">If an operation is server-evaluated by EF Core via SQL, the result's unit will be determined by the database.</span></span>

<span data-ttu-id="e24a4-158">Tady je příklad použití ProjNet4GeoAPI k výpočtu vzdálenosti mezi dvěma městy.</span><span class="sxs-lookup"><span data-stu-id="e24a4-158">Here is an example of using ProjNet4GeoAPI to calculate the distance between two cities.</span></span>

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

## <a name="querying-data"></a><span data-ttu-id="e24a4-159">Dotazování na data</span><span class="sxs-lookup"><span data-stu-id="e24a4-159">Querying Data</span></span>

<span data-ttu-id="e24a4-160">V jazyce LINQ budou metody a vlastnosti NTS dostupné jako databázové funkce přeloženy do jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="e24a4-160">In LINQ, the NTS methods and properties available as database functions will be translated to SQL.</span></span> <span data-ttu-id="e24a4-161">Například vzdálenost a obsahuje metody jsou přeloženy v následujících dotazech.</span><span class="sxs-lookup"><span data-stu-id="e24a4-161">For example, the Distance and Contains methods are translated in the following queries.</span></span> <span data-ttu-id="e24a4-162">Tabulka na konci tohoto článku ukazuje, kteří členové jsou podporováni různými poskytovateli EF Core.</span><span class="sxs-lookup"><span data-stu-id="e24a4-162">The table at the end of this article shows which members are supported by various EF Core providers.</span></span>

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a><span data-ttu-id="e24a4-163">SQL Server</span><span class="sxs-lookup"><span data-stu-id="e24a4-163">SQL Server</span></span>

<span data-ttu-id="e24a4-164">Pokud používáte SQL Server, máte k dispozici několik dalších věcí, o kterých byste měli vědět.</span><span class="sxs-lookup"><span data-stu-id="e24a4-164">If you're using SQL Server, there are some additional things you should be aware of.</span></span>

### <a name="geography-or-geometry"></a><span data-ttu-id="e24a4-165">Zeměpisná nebo geometrie</span><span class="sxs-lookup"><span data-stu-id="e24a4-165">Geography or geometry</span></span>

<span data-ttu-id="e24a4-166">Ve výchozím nastavení jsou prostorové vlastnosti namapovány na `geography` sloupce v SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e24a4-166">By default, spatial properties are mapped to `geography` columns in SQL Server.</span></span> <span data-ttu-id="e24a4-167">Pokud chcete použít `geometry`, nakonfigurujte v modelu [typ sloupce](xref:core/modeling/relational/data-types) .</span><span class="sxs-lookup"><span data-stu-id="e24a4-167">To use `geometry`, [configure the column type](xref:core/modeling/relational/data-types) in your model.</span></span>

### <a name="geography-polygon-rings"></a><span data-ttu-id="e24a4-168">Geografické kroužky mnohoúhelníků</span><span class="sxs-lookup"><span data-stu-id="e24a4-168">Geography polygon rings</span></span>

<span data-ttu-id="e24a4-169">Při použití `geography`ho typu sloupce SQL Server ukládá další požadavky na vnější prstenec (nebo prostředí) a vnitřní prstence (nebo díry).</span><span class="sxs-lookup"><span data-stu-id="e24a4-169">When using the `geography` column type, SQL Server imposes additional requirements on the exterior ring (or shell) and interior rings (or holes).</span></span> <span data-ttu-id="e24a4-170">Vnější prstenec musí být orientovaný proti směru hodinových ručiček a vnitřní prstence po směru hodinových ručiček.</span><span class="sxs-lookup"><span data-stu-id="e24a4-170">The exterior ring must be oriented counterclockwise and the interior rings clockwise.</span></span> <span data-ttu-id="e24a4-171">NTS ho před odesláním hodnot do databáze ověří.</span><span class="sxs-lookup"><span data-stu-id="e24a4-171">NTS validates this before sending values to the database.</span></span>

### <a name="fullglobe"></a><span data-ttu-id="e24a4-172">FullGlobe</span><span class="sxs-lookup"><span data-stu-id="e24a4-172">FullGlobe</span></span>

<span data-ttu-id="e24a4-173">SQL Server má nestandardní typ geometrie, který představuje úplný glóbus při použití `geography`ho typu sloupce.</span><span class="sxs-lookup"><span data-stu-id="e24a4-173">SQL Server has a non-standard geometry type to represent the full globe when using the `geography` column type.</span></span> <span data-ttu-id="e24a4-174">Má také způsob, jak znázornit mnohoúhelníky na základě plného světa (bez vnějšího okruhu).</span><span class="sxs-lookup"><span data-stu-id="e24a4-174">It also has a way to represent polygons based on the full globe (without an exterior ring).</span></span> <span data-ttu-id="e24a4-175">Ani jedna z těchto možností není podporována nástrojem NTS.</span><span class="sxs-lookup"><span data-stu-id="e24a4-175">Neither of these are supported by NTS.</span></span>

> [!WARNING]
> <span data-ttu-id="e24a4-176">FullGlobe a mnohoúhelníky, které jsou na nich založené, nejsou podporovány NTS.</span><span class="sxs-lookup"><span data-stu-id="e24a4-176">FullGlobe and polygons based on it aren't supported by NTS.</span></span>

## <a name="sqlite"></a><span data-ttu-id="e24a4-177">SQLite</span><span class="sxs-lookup"><span data-stu-id="e24a4-177">SQLite</span></span>

<span data-ttu-id="e24a4-178">Zde jsou některé další informace pro ty, které používají SQLite.</span><span class="sxs-lookup"><span data-stu-id="e24a4-178">Here is some additional information for those using SQLite.</span></span>

### <a name="installing-spatialite"></a><span data-ttu-id="e24a4-179">Instalace SpatiaLite</span><span class="sxs-lookup"><span data-stu-id="e24a4-179">Installing SpatiaLite</span></span>

<span data-ttu-id="e24a4-180">V systému Windows je nativní knihovna mod_spatialite distribuována jako závislost balíčku NuGet.</span><span class="sxs-lookup"><span data-stu-id="e24a4-180">On Windows, the native mod_spatialite library is distributed as a NuGet package dependency.</span></span> <span data-ttu-id="e24a4-181">Jiné platformy je potřeba nainstalovat samostatně.</span><span class="sxs-lookup"><span data-stu-id="e24a4-181">Other platforms need to install it separately.</span></span> <span data-ttu-id="e24a4-182">To se obvykle provádí pomocí Správce balíčků softwaru.</span><span class="sxs-lookup"><span data-stu-id="e24a4-182">This is typically done using a software package manager.</span></span> <span data-ttu-id="e24a4-183">Například můžete použít APT v Ubuntu a homebrew na MacOS.</span><span class="sxs-lookup"><span data-stu-id="e24a4-183">For example, you can use APT on Ubuntu and Homebrew on MacOS.</span></span>

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

### <a name="configuring-srid"></a><span data-ttu-id="e24a4-184">Konfigurace SRID</span><span class="sxs-lookup"><span data-stu-id="e24a4-184">Configuring SRID</span></span>

<span data-ttu-id="e24a4-185">V SpatiaLite musí sloupce určovat SRID na sloupec.</span><span class="sxs-lookup"><span data-stu-id="e24a4-185">In SpatiaLite, columns need to specify an SRID per column.</span></span> <span data-ttu-id="e24a4-186">Výchozí SRID je `0`.</span><span class="sxs-lookup"><span data-stu-id="e24a4-186">The default SRID is `0`.</span></span> <span data-ttu-id="e24a4-187">Určete jiný SRID pomocí metody ForSqliteHasSrid.</span><span class="sxs-lookup"><span data-stu-id="e24a4-187">Specify a different SRID using the ForSqliteHasSrid method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a><span data-ttu-id="e24a4-188">Rozměr</span><span class="sxs-lookup"><span data-stu-id="e24a4-188">Dimension</span></span>

<span data-ttu-id="e24a4-189">Podobně jako u SRID je jako součást sloupce zadána také dimenze sloupce (nebo souřadnice).</span><span class="sxs-lookup"><span data-stu-id="e24a4-189">Similar to SRID, a column's dimension (or ordinates) is also specified as part of the column.</span></span> <span data-ttu-id="e24a4-190">Výchozí souřadnice jsou X a Y. Povolte další souřadnice (Z a M) pomocí metody ForSqliteHasDimension.</span><span class="sxs-lookup"><span data-stu-id="e24a4-190">The default ordinates are X and Y. Enable additional ordinates (Z and M) using the ForSqliteHasDimension method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a><span data-ttu-id="e24a4-191">Přeložené operace</span><span class="sxs-lookup"><span data-stu-id="e24a4-191">Translated Operations</span></span>

<span data-ttu-id="e24a4-192">Tato tabulka uvádí, které NTS členové jsou přeloženi do jazyka SQL každým poskytovatelem EF Core.</span><span class="sxs-lookup"><span data-stu-id="e24a4-192">This table shows which NTS members are translated into SQL by each EF Core provider.</span></span>

<span data-ttu-id="e24a4-193">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="e24a4-193">NetTopologySuite</span></span> | <span data-ttu-id="e24a4-194">SQL Server (geometrie)</span><span class="sxs-lookup"><span data-stu-id="e24a4-194">SQL Server (geometry)</span></span> | <span data-ttu-id="e24a4-195">SQL Server (geografie)</span><span class="sxs-lookup"><span data-stu-id="e24a4-195">SQL Server (geography)</span></span> | <span data-ttu-id="e24a4-196">SQLite</span><span class="sxs-lookup"><span data-stu-id="e24a4-196">SQLite</span></span> | <span data-ttu-id="e24a4-197">Npgsql</span><span class="sxs-lookup"><span data-stu-id="e24a4-197">Npgsql</span></span>
--- |:---:|:---:|:---:|:---:
<span data-ttu-id="e24a4-198">Geometrie. Area</span><span class="sxs-lookup"><span data-stu-id="e24a4-198">Geometry.Area</span></span> | <span data-ttu-id="e24a4-199">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-199">✔</span></span> | <span data-ttu-id="e24a4-200">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-200">✔</span></span> | <span data-ttu-id="e24a4-201">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-201">✔</span></span> | <span data-ttu-id="e24a4-202">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-202">✔</span></span>
<span data-ttu-id="e24a4-203">Geometry. AsBinary ()</span><span class="sxs-lookup"><span data-stu-id="e24a4-203">Geometry.AsBinary()</span></span> | <span data-ttu-id="e24a4-204">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-204">✔</span></span> | <span data-ttu-id="e24a4-205">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-205">✔</span></span> | <span data-ttu-id="e24a4-206">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-206">✔</span></span> | <span data-ttu-id="e24a4-207">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-207">✔</span></span>
<span data-ttu-id="e24a4-208">Geometry. AsText ()</span><span class="sxs-lookup"><span data-stu-id="e24a4-208">Geometry.AsText()</span></span> | <span data-ttu-id="e24a4-209">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-209">✔</span></span> | <span data-ttu-id="e24a4-210">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-210">✔</span></span> | <span data-ttu-id="e24a4-211">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-211">✔</span></span> | <span data-ttu-id="e24a4-212">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-212">✔</span></span>
<span data-ttu-id="e24a4-213">Geometrie. hranice</span><span class="sxs-lookup"><span data-stu-id="e24a4-213">Geometry.Boundary</span></span> | <span data-ttu-id="e24a4-214">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-214">✔</span></span> | | <span data-ttu-id="e24a4-215">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-215">✔</span></span> | <span data-ttu-id="e24a4-216">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-216">✔</span></span>
<span data-ttu-id="e24a4-217">Geometry. Buffer (Double)</span><span class="sxs-lookup"><span data-stu-id="e24a4-217">Geometry.Buffer(double)</span></span> | <span data-ttu-id="e24a4-218">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-218">✔</span></span> | <span data-ttu-id="e24a4-219">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-219">✔</span></span> | <span data-ttu-id="e24a4-220">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-220">✔</span></span> | <span data-ttu-id="e24a4-221">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-221">✔</span></span>
<span data-ttu-id="e24a4-222">Geometry. Buffer (Double, int)</span><span class="sxs-lookup"><span data-stu-id="e24a4-222">Geometry.Buffer(double, int)</span></span> | | | <span data-ttu-id="e24a4-223">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-223">✔</span></span> | <span data-ttu-id="e24a4-224">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-224">✔</span></span>
<span data-ttu-id="e24a4-225">Geometrie. těžiště</span><span class="sxs-lookup"><span data-stu-id="e24a4-225">Geometry.Centroid</span></span> | <span data-ttu-id="e24a4-226">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-226">✔</span></span> | | <span data-ttu-id="e24a4-227">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-227">✔</span></span> | <span data-ttu-id="e24a4-228">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-228">✔</span></span>
<span data-ttu-id="e24a4-229">Geometrie. Contains (geometrie)</span><span class="sxs-lookup"><span data-stu-id="e24a4-229">Geometry.Contains(Geometry)</span></span> | <span data-ttu-id="e24a4-230">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-230">✔</span></span> | <span data-ttu-id="e24a4-231">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-231">✔</span></span> | <span data-ttu-id="e24a4-232">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-232">✔</span></span> | <span data-ttu-id="e24a4-233">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-233">✔</span></span>
<span data-ttu-id="e24a4-234">Geometry. ConvexHull ()</span><span class="sxs-lookup"><span data-stu-id="e24a4-234">Geometry.ConvexHull()</span></span> | <span data-ttu-id="e24a4-235">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-235">✔</span></span> | <span data-ttu-id="e24a4-236">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-236">✔</span></span> | <span data-ttu-id="e24a4-237">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-237">✔</span></span> | <span data-ttu-id="e24a4-238">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-238">✔</span></span>
<span data-ttu-id="e24a4-239">Geometry. CoveredBy (geometrie)</span><span class="sxs-lookup"><span data-stu-id="e24a4-239">Geometry.CoveredBy(Geometry)</span></span> | | | <span data-ttu-id="e24a4-240">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-240">✔</span></span> | <span data-ttu-id="e24a4-241">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-241">✔</span></span>
<span data-ttu-id="e24a4-242">Geometry. pokrývání (geometrie)</span><span class="sxs-lookup"><span data-stu-id="e24a4-242">Geometry.Covers(Geometry)</span></span> | | | <span data-ttu-id="e24a4-243">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-243">✔</span></span> | <span data-ttu-id="e24a4-244">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-244">✔</span></span>
<span data-ttu-id="e24a4-245">Geometrie. křížení (geometrie)</span><span class="sxs-lookup"><span data-stu-id="e24a4-245">Geometry.Crosses(Geometry)</span></span> | <span data-ttu-id="e24a4-246">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-246">✔</span></span> | | <span data-ttu-id="e24a4-247">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-247">✔</span></span> | <span data-ttu-id="e24a4-248">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-248">✔</span></span>
<span data-ttu-id="e24a4-249">Geometry. rozdíl (geometrie)</span><span class="sxs-lookup"><span data-stu-id="e24a4-249">Geometry.Difference(Geometry)</span></span> | <span data-ttu-id="e24a4-250">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-250">✔</span></span> | <span data-ttu-id="e24a4-251">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-251">✔</span></span> | <span data-ttu-id="e24a4-252">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-252">✔</span></span> | <span data-ttu-id="e24a4-253">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-253">✔</span></span>
<span data-ttu-id="e24a4-254">Geometrie. Dimension</span><span class="sxs-lookup"><span data-stu-id="e24a4-254">Geometry.Dimension</span></span> | <span data-ttu-id="e24a4-255">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-255">✔</span></span> | <span data-ttu-id="e24a4-256">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-256">✔</span></span> | <span data-ttu-id="e24a4-257">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-257">✔</span></span> | <span data-ttu-id="e24a4-258">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-258">✔</span></span>
<span data-ttu-id="e24a4-259">Geometrie. unkloub (geometrie)</span><span class="sxs-lookup"><span data-stu-id="e24a4-259">Geometry.Disjoint(Geometry)</span></span> | <span data-ttu-id="e24a4-260">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-260">✔</span></span> | <span data-ttu-id="e24a4-261">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-261">✔</span></span> | <span data-ttu-id="e24a4-262">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-262">✔</span></span> | <span data-ttu-id="e24a4-263">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-263">✔</span></span>
<span data-ttu-id="e24a4-264">Geometrie. Distance (geometrie)</span><span class="sxs-lookup"><span data-stu-id="e24a4-264">Geometry.Distance(Geometry)</span></span> | <span data-ttu-id="e24a4-265">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-265">✔</span></span> | <span data-ttu-id="e24a4-266">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-266">✔</span></span> | <span data-ttu-id="e24a4-267">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-267">✔</span></span> | <span data-ttu-id="e24a4-268">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-268">✔</span></span>
<span data-ttu-id="e24a4-269">Geometrie. obálky</span><span class="sxs-lookup"><span data-stu-id="e24a4-269">Geometry.Envelope</span></span> | <span data-ttu-id="e24a4-270">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-270">✔</span></span> | | <span data-ttu-id="e24a4-271">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-271">✔</span></span> | <span data-ttu-id="e24a4-272">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-272">✔</span></span>
<span data-ttu-id="e24a4-273">Geometry. EqualsExact (geometrie)</span><span class="sxs-lookup"><span data-stu-id="e24a4-273">Geometry.EqualsExact(Geometry)</span></span> | | | | <span data-ttu-id="e24a4-274">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-274">✔</span></span>
<span data-ttu-id="e24a4-275">Geometry. EqualsTopologically (geometrie)</span><span class="sxs-lookup"><span data-stu-id="e24a4-275">Geometry.EqualsTopologically(Geometry)</span></span> | <span data-ttu-id="e24a4-276">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-276">✔</span></span> | <span data-ttu-id="e24a4-277">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-277">✔</span></span> | <span data-ttu-id="e24a4-278">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-278">✔</span></span> | <span data-ttu-id="e24a4-279">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-279">✔</span></span>
<span data-ttu-id="e24a4-280">Geometrie. GeometryType</span><span class="sxs-lookup"><span data-stu-id="e24a4-280">Geometry.GeometryType</span></span> | <span data-ttu-id="e24a4-281">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-281">✔</span></span> | <span data-ttu-id="e24a4-282">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-282">✔</span></span> | <span data-ttu-id="e24a4-283">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-283">✔</span></span> | <span data-ttu-id="e24a4-284">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-284">✔</span></span>
<span data-ttu-id="e24a4-285">Geometry. GetGeometryN (int)</span><span class="sxs-lookup"><span data-stu-id="e24a4-285">Geometry.GetGeometryN(int)</span></span> | <span data-ttu-id="e24a4-286">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-286">✔</span></span> | | <span data-ttu-id="e24a4-287">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-287">✔</span></span> | <span data-ttu-id="e24a4-288">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-288">✔</span></span>
<span data-ttu-id="e24a4-289">Geometrie. InteriorPoint</span><span class="sxs-lookup"><span data-stu-id="e24a4-289">Geometry.InteriorPoint</span></span> | <span data-ttu-id="e24a4-290">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-290">✔</span></span> | | <span data-ttu-id="e24a4-291">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-291">✔</span></span> | <span data-ttu-id="e24a4-292">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-292">✔</span></span>
<span data-ttu-id="e24a4-293">Geometry. proprůsečík (geometrie)</span><span class="sxs-lookup"><span data-stu-id="e24a4-293">Geometry.Intersection(Geometry)</span></span> | <span data-ttu-id="e24a4-294">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-294">✔</span></span> | <span data-ttu-id="e24a4-295">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-295">✔</span></span> | <span data-ttu-id="e24a4-296">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-296">✔</span></span> | <span data-ttu-id="e24a4-297">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-297">✔</span></span>
<span data-ttu-id="e24a4-298">Geometrie. INTERSECTY (geometrie)</span><span class="sxs-lookup"><span data-stu-id="e24a4-298">Geometry.Intersects(Geometry)</span></span> | <span data-ttu-id="e24a4-299">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-299">✔</span></span> | <span data-ttu-id="e24a4-300">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-300">✔</span></span> | <span data-ttu-id="e24a4-301">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-301">✔</span></span> | <span data-ttu-id="e24a4-302">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-302">✔</span></span>
<span data-ttu-id="e24a4-303">Geometrie. Empty</span><span class="sxs-lookup"><span data-stu-id="e24a4-303">Geometry.IsEmpty</span></span> | <span data-ttu-id="e24a4-304">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-304">✔</span></span> | <span data-ttu-id="e24a4-305">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-305">✔</span></span> | <span data-ttu-id="e24a4-306">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-306">✔</span></span> | <span data-ttu-id="e24a4-307">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-307">✔</span></span>
<span data-ttu-id="e24a4-308">Geometrie. zjednodušená</span><span class="sxs-lookup"><span data-stu-id="e24a4-308">Geometry.IsSimple</span></span> | <span data-ttu-id="e24a4-309">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-309">✔</span></span> | | <span data-ttu-id="e24a4-310">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-310">✔</span></span> | <span data-ttu-id="e24a4-311">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-311">✔</span></span>
<span data-ttu-id="e24a4-312">Geometrie. IsValid</span><span class="sxs-lookup"><span data-stu-id="e24a4-312">Geometry.IsValid</span></span> | <span data-ttu-id="e24a4-313">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-313">✔</span></span> | <span data-ttu-id="e24a4-314">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-314">✔</span></span> | <span data-ttu-id="e24a4-315">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-315">✔</span></span> | <span data-ttu-id="e24a4-316">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-316">✔</span></span>
<span data-ttu-id="e24a4-317">Geometry. IsWithinDistance (geometrie, Double)</span><span class="sxs-lookup"><span data-stu-id="e24a4-317">Geometry.IsWithinDistance(Geometry, double)</span></span> | <span data-ttu-id="e24a4-318">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-318">✔</span></span> | | <span data-ttu-id="e24a4-319">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-319">✔</span></span> | <span data-ttu-id="e24a4-320">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-320">✔</span></span>
<span data-ttu-id="e24a4-321">Geometrie. Length</span><span class="sxs-lookup"><span data-stu-id="e24a4-321">Geometry.Length</span></span> | <span data-ttu-id="e24a4-322">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-322">✔</span></span> | <span data-ttu-id="e24a4-323">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-323">✔</span></span> | <span data-ttu-id="e24a4-324">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-324">✔</span></span> | <span data-ttu-id="e24a4-325">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-325">✔</span></span>
<span data-ttu-id="e24a4-326">Geometrie. NumGeometries</span><span class="sxs-lookup"><span data-stu-id="e24a4-326">Geometry.NumGeometries</span></span> | <span data-ttu-id="e24a4-327">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-327">✔</span></span> | <span data-ttu-id="e24a4-328">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-328">✔</span></span> | <span data-ttu-id="e24a4-329">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-329">✔</span></span> | <span data-ttu-id="e24a4-330">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-330">✔</span></span>
<span data-ttu-id="e24a4-331">Geometrie. NumPoints</span><span class="sxs-lookup"><span data-stu-id="e24a4-331">Geometry.NumPoints</span></span> | <span data-ttu-id="e24a4-332">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-332">✔</span></span> | <span data-ttu-id="e24a4-333">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-333">✔</span></span> | <span data-ttu-id="e24a4-334">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-334">✔</span></span> | <span data-ttu-id="e24a4-335">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-335">✔</span></span>
<span data-ttu-id="e24a4-336">Geometrie. OgcGeometryType</span><span class="sxs-lookup"><span data-stu-id="e24a4-336">Geometry.OgcGeometryType</span></span> | <span data-ttu-id="e24a4-337">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-337">✔</span></span> | <span data-ttu-id="e24a4-338">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-338">✔</span></span> | <span data-ttu-id="e24a4-339">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-339">✔</span></span> | <span data-ttu-id="e24a4-340">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-340">✔</span></span>
<span data-ttu-id="e24a4-341">Geometrie. překryvy (geometrie)</span><span class="sxs-lookup"><span data-stu-id="e24a4-341">Geometry.Overlaps(Geometry)</span></span> | <span data-ttu-id="e24a4-342">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-342">✔</span></span> | <span data-ttu-id="e24a4-343">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-343">✔</span></span> | <span data-ttu-id="e24a4-344">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-344">✔</span></span> | <span data-ttu-id="e24a4-345">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-345">✔</span></span>
<span data-ttu-id="e24a4-346">Geometrie. PointOnSurface</span><span class="sxs-lookup"><span data-stu-id="e24a4-346">Geometry.PointOnSurface</span></span> | <span data-ttu-id="e24a4-347">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-347">✔</span></span> | | <span data-ttu-id="e24a4-348">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-348">✔</span></span> | <span data-ttu-id="e24a4-349">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-349">✔</span></span>
<span data-ttu-id="e24a4-350">Geometry. propojovat (geometrie, String)</span><span class="sxs-lookup"><span data-stu-id="e24a4-350">Geometry.Relate(Geometry, string)</span></span> | <span data-ttu-id="e24a4-351">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-351">✔</span></span> | | <span data-ttu-id="e24a4-352">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-352">✔</span></span> | <span data-ttu-id="e24a4-353">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-353">✔</span></span>
<span data-ttu-id="e24a4-354">Geometry. Reverse ()</span><span class="sxs-lookup"><span data-stu-id="e24a4-354">Geometry.Reverse()</span></span> | | | <span data-ttu-id="e24a4-355">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-355">✔</span></span> | <span data-ttu-id="e24a4-356">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-356">✔</span></span>
<span data-ttu-id="e24a4-357">Geometrie. SRID</span><span class="sxs-lookup"><span data-stu-id="e24a4-357">Geometry.SRID</span></span> | <span data-ttu-id="e24a4-358">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-358">✔</span></span> | <span data-ttu-id="e24a4-359">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-359">✔</span></span> | <span data-ttu-id="e24a4-360">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-360">✔</span></span> | <span data-ttu-id="e24a4-361">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-361">✔</span></span>
<span data-ttu-id="e24a4-362">Geometry. SymmetricDifference (geometrie)</span><span class="sxs-lookup"><span data-stu-id="e24a4-362">Geometry.SymmetricDifference(Geometry)</span></span> | <span data-ttu-id="e24a4-363">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-363">✔</span></span> | <span data-ttu-id="e24a4-364">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-364">✔</span></span> | <span data-ttu-id="e24a4-365">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-365">✔</span></span> | <span data-ttu-id="e24a4-366">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-366">✔</span></span>
<span data-ttu-id="e24a4-367">Geometry. ToBinary ()</span><span class="sxs-lookup"><span data-stu-id="e24a4-367">Geometry.ToBinary()</span></span> | <span data-ttu-id="e24a4-368">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-368">✔</span></span> | <span data-ttu-id="e24a4-369">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-369">✔</span></span> | <span data-ttu-id="e24a4-370">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-370">✔</span></span> | <span data-ttu-id="e24a4-371">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-371">✔</span></span>
<span data-ttu-id="e24a4-372">Geometry. ToText ()</span><span class="sxs-lookup"><span data-stu-id="e24a4-372">Geometry.ToText()</span></span> | <span data-ttu-id="e24a4-373">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-373">✔</span></span> | <span data-ttu-id="e24a4-374">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-374">✔</span></span> | <span data-ttu-id="e24a4-375">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-375">✔</span></span> | <span data-ttu-id="e24a4-376">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-376">✔</span></span>
<span data-ttu-id="e24a4-377">Geometrie. touchs (geometrie)</span><span class="sxs-lookup"><span data-stu-id="e24a4-377">Geometry.Touches(Geometry)</span></span> | <span data-ttu-id="e24a4-378">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-378">✔</span></span> | | <span data-ttu-id="e24a4-379">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-379">✔</span></span> | <span data-ttu-id="e24a4-380">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-380">✔</span></span>
<span data-ttu-id="e24a4-381">Geometry. Union ()</span><span class="sxs-lookup"><span data-stu-id="e24a4-381">Geometry.Union()</span></span> | | | <span data-ttu-id="e24a4-382">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-382">✔</span></span> | <span data-ttu-id="e24a4-383">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-383">✔</span></span>
<span data-ttu-id="e24a4-384">Geometry. Union (geometrie)</span><span class="sxs-lookup"><span data-stu-id="e24a4-384">Geometry.Union(Geometry)</span></span> | <span data-ttu-id="e24a4-385">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-385">✔</span></span> | <span data-ttu-id="e24a4-386">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-386">✔</span></span> | <span data-ttu-id="e24a4-387">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-387">✔</span></span> | <span data-ttu-id="e24a4-388">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-388">✔</span></span>
<span data-ttu-id="e24a4-389">Geometrie. uvnitř (geometrie)</span><span class="sxs-lookup"><span data-stu-id="e24a4-389">Geometry.Within(Geometry)</span></span> | <span data-ttu-id="e24a4-390">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-390">✔</span></span> | <span data-ttu-id="e24a4-391">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-391">✔</span></span> | <span data-ttu-id="e24a4-392">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-392">✔</span></span> | <span data-ttu-id="e24a4-393">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-393">✔</span></span>
<span data-ttu-id="e24a4-394">Geometriecollection. Count</span><span class="sxs-lookup"><span data-stu-id="e24a4-394">GeometryCollection.Count</span></span> | <span data-ttu-id="e24a4-395">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-395">✔</span></span> | <span data-ttu-id="e24a4-396">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-396">✔</span></span> | <span data-ttu-id="e24a4-397">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-397">✔</span></span> | <span data-ttu-id="e24a4-398">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-398">✔</span></span>
<span data-ttu-id="e24a4-399">GeometryCollection [int]</span><span class="sxs-lookup"><span data-stu-id="e24a4-399">GeometryCollection[int]</span></span> | <span data-ttu-id="e24a4-400">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-400">✔</span></span> | <span data-ttu-id="e24a4-401">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-401">✔</span></span> | <span data-ttu-id="e24a4-402">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-402">✔</span></span> | <span data-ttu-id="e24a4-403">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-403">✔</span></span>
<span data-ttu-id="e24a4-404">LineString. Count</span><span class="sxs-lookup"><span data-stu-id="e24a4-404">LineString.Count</span></span> | <span data-ttu-id="e24a4-405">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-405">✔</span></span> | <span data-ttu-id="e24a4-406">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-406">✔</span></span> | <span data-ttu-id="e24a4-407">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-407">✔</span></span> | <span data-ttu-id="e24a4-408">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-408">✔</span></span>
<span data-ttu-id="e24a4-409">LineString. EndPoint</span><span class="sxs-lookup"><span data-stu-id="e24a4-409">LineString.EndPoint</span></span> | <span data-ttu-id="e24a4-410">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-410">✔</span></span> | <span data-ttu-id="e24a4-411">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-411">✔</span></span> | <span data-ttu-id="e24a4-412">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-412">✔</span></span> | <span data-ttu-id="e24a4-413">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-413">✔</span></span>
<span data-ttu-id="e24a4-414">LineString. GetPointN (int)</span><span class="sxs-lookup"><span data-stu-id="e24a4-414">LineString.GetPointN(int)</span></span> | <span data-ttu-id="e24a4-415">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-415">✔</span></span> | <span data-ttu-id="e24a4-416">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-416">✔</span></span> | <span data-ttu-id="e24a4-417">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-417">✔</span></span> | <span data-ttu-id="e24a4-418">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-418">✔</span></span>
<span data-ttu-id="e24a4-419">LineString. uzavřeno</span><span class="sxs-lookup"><span data-stu-id="e24a4-419">LineString.IsClosed</span></span> | <span data-ttu-id="e24a4-420">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-420">✔</span></span> | <span data-ttu-id="e24a4-421">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-421">✔</span></span> | <span data-ttu-id="e24a4-422">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-422">✔</span></span> | <span data-ttu-id="e24a4-423">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-423">✔</span></span>
<span data-ttu-id="e24a4-424">LineString. pozvonění</span><span class="sxs-lookup"><span data-stu-id="e24a4-424">LineString.IsRing</span></span> | <span data-ttu-id="e24a4-425">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-425">✔</span></span> | | <span data-ttu-id="e24a4-426">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-426">✔</span></span> | <span data-ttu-id="e24a4-427">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-427">✔</span></span>
<span data-ttu-id="e24a4-428">LineString. StartPoint</span><span class="sxs-lookup"><span data-stu-id="e24a4-428">LineString.StartPoint</span></span> | <span data-ttu-id="e24a4-429">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-429">✔</span></span> | <span data-ttu-id="e24a4-430">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-430">✔</span></span> | <span data-ttu-id="e24a4-431">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-431">✔</span></span> | <span data-ttu-id="e24a4-432">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-432">✔</span></span>
<span data-ttu-id="e24a4-433">MultiLineString. uzavřeno</span><span class="sxs-lookup"><span data-stu-id="e24a4-433">MultiLineString.IsClosed</span></span> | <span data-ttu-id="e24a4-434">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-434">✔</span></span> | <span data-ttu-id="e24a4-435">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-435">✔</span></span> | <span data-ttu-id="e24a4-436">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-436">✔</span></span> | <span data-ttu-id="e24a4-437">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-437">✔</span></span>
<span data-ttu-id="e24a4-438">Point. M</span><span class="sxs-lookup"><span data-stu-id="e24a4-438">Point.M</span></span> | <span data-ttu-id="e24a4-439">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-439">✔</span></span> | <span data-ttu-id="e24a4-440">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-440">✔</span></span> | <span data-ttu-id="e24a4-441">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-441">✔</span></span> | <span data-ttu-id="e24a4-442">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-442">✔</span></span>
<span data-ttu-id="e24a4-443">Point. X</span><span class="sxs-lookup"><span data-stu-id="e24a4-443">Point.X</span></span> | <span data-ttu-id="e24a4-444">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-444">✔</span></span> | <span data-ttu-id="e24a4-445">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-445">✔</span></span> | <span data-ttu-id="e24a4-446">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-446">✔</span></span> | <span data-ttu-id="e24a4-447">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-447">✔</span></span>
<span data-ttu-id="e24a4-448">Point. Y</span><span class="sxs-lookup"><span data-stu-id="e24a4-448">Point.Y</span></span> | <span data-ttu-id="e24a4-449">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-449">✔</span></span> | <span data-ttu-id="e24a4-450">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-450">✔</span></span> | <span data-ttu-id="e24a4-451">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-451">✔</span></span> | <span data-ttu-id="e24a4-452">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-452">✔</span></span>
<span data-ttu-id="e24a4-453">Point. Z</span><span class="sxs-lookup"><span data-stu-id="e24a4-453">Point.Z</span></span> | <span data-ttu-id="e24a4-454">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-454">✔</span></span> | <span data-ttu-id="e24a4-455">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-455">✔</span></span> | <span data-ttu-id="e24a4-456">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-456">✔</span></span> | <span data-ttu-id="e24a4-457">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-457">✔</span></span>
<span data-ttu-id="e24a4-458">Mnohoúhelník. ExteriorRing</span><span class="sxs-lookup"><span data-stu-id="e24a4-458">Polygon.ExteriorRing</span></span> | <span data-ttu-id="e24a4-459">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-459">✔</span></span> | <span data-ttu-id="e24a4-460">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-460">✔</span></span> | <span data-ttu-id="e24a4-461">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-461">✔</span></span> | <span data-ttu-id="e24a4-462">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-462">✔</span></span>
<span data-ttu-id="e24a4-463">Mnohoúhelník. GetInteriorRingN (int)</span><span class="sxs-lookup"><span data-stu-id="e24a4-463">Polygon.GetInteriorRingN(int)</span></span> | <span data-ttu-id="e24a4-464">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-464">✔</span></span> | <span data-ttu-id="e24a4-465">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-465">✔</span></span> | <span data-ttu-id="e24a4-466">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-466">✔</span></span> | <span data-ttu-id="e24a4-467">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-467">✔</span></span>
<span data-ttu-id="e24a4-468">Mnohoúhelník. NumInteriorRings</span><span class="sxs-lookup"><span data-stu-id="e24a4-468">Polygon.NumInteriorRings</span></span> | <span data-ttu-id="e24a4-469">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-469">✔</span></span> | <span data-ttu-id="e24a4-470">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-470">✔</span></span> | <span data-ttu-id="e24a4-471">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-471">✔</span></span> | <span data-ttu-id="e24a4-472">✔</span><span class="sxs-lookup"><span data-stu-id="e24a4-472">✔</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e24a4-473">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="e24a4-473">Additional resources</span></span>

* [<span data-ttu-id="e24a4-474">Prostorová data v SQL Server</span><span class="sxs-lookup"><span data-stu-id="e24a4-474">Spatial Data in SQL Server</span></span>](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [<span data-ttu-id="e24a4-475">Domovská stránka SpatiaLite</span><span class="sxs-lookup"><span data-stu-id="e24a4-475">SpatiaLite Homepage</span></span>](https://www.gaia-gis.it/fossil/libspatialite)
* [<span data-ttu-id="e24a4-476">Npgsql prostorová dokumentace</span><span class="sxs-lookup"><span data-stu-id="e24a4-476">Npgsql Spatial Documentation</span></span>](https://www.npgsql.org/efcore/mapping/nts.html)
* [<span data-ttu-id="e24a4-477">Dokumentace k PostGIS</span><span class="sxs-lookup"><span data-stu-id="e24a4-477">PostGIS Documentation</span></span>](https://postgis.net/documentation/)
