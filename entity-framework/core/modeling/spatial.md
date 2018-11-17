---
title: Prostorových dat – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: cf488c6b7d94ca19018efe1c23ff410fe7eb594b
ms.sourcegitcommit: 81c53ac43d8f15b900f117294ec71dc49fe028fa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/16/2018
ms.locfileid: "51817907"
---
# <a name="spatial-data"></a><span data-ttu-id="4fc3a-102">Prostorová Data</span><span class="sxs-lookup"><span data-stu-id="4fc3a-102">Spatial Data</span></span>

> [!NOTE]
> <span data-ttu-id="4fc3a-103">Tato funkce je nového v EF Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-103">This feature is new in EF Core 2.2.</span></span>

<span data-ttu-id="4fc3a-104">Prostorová data představuje fyzické umístění a tvar objektů.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-104">Spatial data represents the physical location and the shape of objects.</span></span> <span data-ttu-id="4fc3a-105">Mnoho databází poskytují podporu pro tento typ dat je možné indexovat a dotazovat společně s dalšími daty.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-105">Many databases provide support for this type of data so it can be indexed and queried alongside other data.</span></span> <span data-ttu-id="4fc3a-106">Běžné scénáře zahrnují zadávání dotazů na objekty v rámci dané vzdálenost od umístění, nebo jeho výběru objektu, jehož ohraničení obsahuje na dané místo.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-106">Common scenarios include querying for objects within a given distance from a location, or selecting the object whose border contains a given location.</span></span> <span data-ttu-id="4fc3a-107">EF Core podporuje mapování pro typy prostorových dat pomocí [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) prostorových knihovny.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-107">EF Core supports mapping to spatial data types using the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) spatial library.</span></span>

## <a name="installing"></a><span data-ttu-id="4fc3a-108">Instalace</span><span class="sxs-lookup"><span data-stu-id="4fc3a-108">Installing</span></span>

<span data-ttu-id="4fc3a-109">Chcete-li použít prostorová data s EF Core, musíte nainstalovat odpovídající podpůrný balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-109">In order to use spatial data with EF Core, you need to install the appropriate supporting NuGet package.</span></span> <span data-ttu-id="4fc3a-110">Balíčky, které je potřeba nainstalovat závisí na poskytovateli, které používáte.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-110">Which package you need to install depends on the provider you're using.</span></span>

<span data-ttu-id="4fc3a-111">EF Core poskytovatele</span><span class="sxs-lookup"><span data-stu-id="4fc3a-111">EF Core Provider</span></span>                        | <span data-ttu-id="4fc3a-112">Prostorový balíček NuGet</span><span class="sxs-lookup"><span data-stu-id="4fc3a-112">Spatial NuGet Package</span></span>
--------------------------------------- | ---------------------
<span data-ttu-id="4fc3a-113">Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="4fc3a-113">Microsoft.EntityFrameworkCore.SqlServer</span></span> | [<span data-ttu-id="4fc3a-114">Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="4fc3a-114">Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
<span data-ttu-id="4fc3a-115">Microsoft.EntityFrameworkCore.Sqlite</span><span class="sxs-lookup"><span data-stu-id="4fc3a-115">Microsoft.EntityFrameworkCore.Sqlite</span></span>    | [<span data-ttu-id="4fc3a-116">Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="4fc3a-116">Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
<span data-ttu-id="4fc3a-117">Microsoft.EntityFrameworkCore.InMemory</span><span class="sxs-lookup"><span data-stu-id="4fc3a-117">Microsoft.EntityFrameworkCore.InMemory</span></span>  | [<span data-ttu-id="4fc3a-118">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="4fc3a-118">NetTopologySuite</span></span>](https://www.nuget.org/packages/NetTopologySuite)
<span data-ttu-id="4fc3a-119">Npgsql.EntityFrameworkCore.PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="4fc3a-119">Npgsql.EntityFrameworkCore.PostgreSQL</span></span>   | [<span data-ttu-id="4fc3a-120">Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="4fc3a-120">Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite</span></span>](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a><span data-ttu-id="4fc3a-121">Zpětná analýza</span><span class="sxs-lookup"><span data-stu-id="4fc3a-121">Reverse engineering</span></span>

<span data-ttu-id="4fc3a-122">Prostorový NuGet balíčky také umožňují [zpětná analýza](../managing-schemas/scaffolding.md) modely s prostorových vlastnosti, ale je potřeba nainstalovat balíček ***před*** systémem `Scaffold-DbContext` nebo `dotnet ef dbcontext scaffold`.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-122">The spatial NuGet packages also enable [reverse engineering](../managing-schemas/scaffolding.md) models with spatial properties, but you need to install the package ***before*** running `Scaffold-DbContext` or `dotnet ef dbcontext scaffold`.</span></span> <span data-ttu-id="4fc3a-123">Pokud to neuděláte, zobrazí se upozornění o nenašli mapování typů pro sloupce a sloupce, které se přeskočí.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-123">If you don't, you'll receive warnings about not finding type mappings for the columns and the columns will be skipped.</span></span>

## <a name="nettopologysuite-nts"></a><span data-ttu-id="4fc3a-124">NetTopologySuite (NTS)</span><span class="sxs-lookup"><span data-stu-id="4fc3a-124">NetTopologySuite (NTS)</span></span>

<span data-ttu-id="4fc3a-125">NetTopologySuite je prostorových knihovna pro .NET.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-125">NetTopologySuite is a spatial library for .NET.</span></span> <span data-ttu-id="4fc3a-126">EF Core umožňuje mapování prostorová data typů v databázi s použitím typů chny Zarážky ve vašem modelu.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-126">EF Core enables mapping to spatial data types in the database by using NTS types in your model.</span></span>

<span data-ttu-id="4fc3a-127">Pokud chcete povolit mapování pro typy prostorových prostřednictvím chny Zarážky, volání metody UseNetTopologySuite na poskytovatele DbContext možnosti Tvůrce.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-127">To enable mapping to spatial types via NTS, call the UseNetTopologySuite method on the provider's DbContext options builder.</span></span> <span data-ttu-id="4fc3a-128">Například s SQL serverem by zavoláte ji následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-128">For example, with SQL Server you'd call it like this.</span></span>

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

<span data-ttu-id="4fc3a-129">Existuje několik typů prostorová data.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-129">There are several spatial data types.</span></span> <span data-ttu-id="4fc3a-130">Jaký typ použijete závisí na typy tvary, které chcete povolit.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-130">Which type you use depends on the types of shapes you want to allow.</span></span> <span data-ttu-id="4fc3a-131">Tady je hierarchie typů chny Zarážky, které můžete použít pro vlastnosti v modelu.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-131">Here is the hierarchy of NTS types that you can use for properties in your model.</span></span> <span data-ttu-id="4fc3a-132">Jsou umístěny v rámci `NetTopologySuite.Geometries` oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-132">They're located within the `NetTopologySuite.Geometries` namespace.</span></span> <span data-ttu-id="4fc3a-133">Odpovídající rozhraní v balíčku GeoAPI (`GeoAPI.Geometries` oboru názvů) je také možné.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-133">Corresponding interfaces in the GeoAPI package (`GeoAPI.Geometries` namespace) can also be used.</span></span>

* <span data-ttu-id="4fc3a-134">Geometrie</span><span class="sxs-lookup"><span data-stu-id="4fc3a-134">Geometry</span></span>
  * <span data-ttu-id="4fc3a-135">Bod</span><span class="sxs-lookup"><span data-stu-id="4fc3a-135">Point</span></span>
  * <span data-ttu-id="4fc3a-136">LineString</span><span class="sxs-lookup"><span data-stu-id="4fc3a-136">LineString</span></span>
  * <span data-ttu-id="4fc3a-137">Mnohoúhelník</span><span class="sxs-lookup"><span data-stu-id="4fc3a-137">Polygon</span></span>
  * <span data-ttu-id="4fc3a-138">GeometryCollection</span><span class="sxs-lookup"><span data-stu-id="4fc3a-138">GeometryCollection</span></span>
    * <span data-ttu-id="4fc3a-139">Systému multiPoint</span><span class="sxs-lookup"><span data-stu-id="4fc3a-139">MultiPoint</span></span>
    * <span data-ttu-id="4fc3a-140">MultiLineString</span><span class="sxs-lookup"><span data-stu-id="4fc3a-140">MultiLineString</span></span>
    * <span data-ttu-id="4fc3a-141">MultiPolygon</span><span class="sxs-lookup"><span data-stu-id="4fc3a-141">MultiPolygon</span></span>

> [!WARNING]
> <span data-ttu-id="4fc3a-142">CircularString, CompoundCurve a CurePolygon nepodporuje chny Zarážky.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-142">CircularString, CompoundCurve, and CurePolygon aren't supported by NTS.</span></span>

<span data-ttu-id="4fc3a-143">Použití základního typu Geometry umožňuje libovolný typ tvar, který má být určené vlastností.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-143">Using the base Geometry type allows any type of shape to be specified by the property.</span></span>

<span data-ttu-id="4fc3a-144">Následující třídy entit může použít k mapování na tabulky v [ukázkové databáze Wide World Importers](http://go.microsoft.com/fwlink/?LinkID=800630).</span><span class="sxs-lookup"><span data-stu-id="4fc3a-144">The following entity classes could be used to map to tables in the [Wide World Importers sample database](http://go.microsoft.com/fwlink/?LinkID=800630).</span></span>

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

### <a name="creating-values"></a><span data-ttu-id="4fc3a-145">Vytváření hodnoty</span><span class="sxs-lookup"><span data-stu-id="4fc3a-145">Creating values</span></span>

<span data-ttu-id="4fc3a-146">Konstruktory můžete použít k vytvoření geometrické objekty; Směřuje doporučuje místo toho použít objekt pro vytváření geometry.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-146">You can use constructors to create geometry objects; however, NTS recommends using a geometry factory instead.</span></span> <span data-ttu-id="4fc3a-147">To umožňuje určit výchozí SRID (spatial reference systém souřadnice) a umožňuje kontrolu nad pokročilejší věci jako přesnost modelu (využitých výpočtů) a souřadnice pořadí (Určuje, která souřadnice--dimenze a míry – jsou k dispozici).</span><span class="sxs-lookup"><span data-stu-id="4fc3a-147">This lets you specify a default SRID (the spatial reference system used by the coordinates) and gives you control over more advanced things like the precision model (used during calculations) and the coordinate sequence (determines which ordinates--dimensions and measures--are available).</span></span>

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> <span data-ttu-id="4fc3a-148">4326 odkazuje na WGS 84 standard používaný v GPS a jiných zeměpisných systémů.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-148">4326 refers to WGS 84, a standard used in GPS and other geographic systems.</span></span>

### <a name="longitude-and-latitude"></a><span data-ttu-id="4fc3a-149">Zeměpisné šířky a délky</span><span class="sxs-lookup"><span data-stu-id="4fc3a-149">Longitude and Latitude</span></span>

<span data-ttu-id="4fc3a-150">Souřadnice v chny Zarážky jsou z hlediska hodnoty X a Y.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-150">Coordinates in NTS are in terms of X and Y values.</span></span> <span data-ttu-id="4fc3a-151">K reprezentaci zeměpisné šířky a délky, použijte pro zeměpisnou délku a Y pro zeměpisnou šířku X.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-151">To represent longitude and latitude, use X for longitude and Y for latitude.</span></span> <span data-ttu-id="4fc3a-152">Všimněte si, že toto je **zpětně** z `latitude, longitude` formát, ve kterém se obvykle zobrazí tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-152">Note that this is **backwards** from the `latitude, longitude` format in which you typically see these values.</span></span>

### <a name="srid-ignored-during-client-operations"></a><span data-ttu-id="4fc3a-153">SRID ignorovat během operace klienta</span><span class="sxs-lookup"><span data-stu-id="4fc3a-153">SRID Ignored during client operations</span></span>

<span data-ttu-id="4fc3a-154">Směřuje ignoruje SRID hodnoty během operací.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-154">NTS ignores SRID values during operations.</span></span> <span data-ttu-id="4fc3a-155">Předpokládá planární systém souřadnic.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-155">It assumes a planar coordinate system.</span></span> <span data-ttu-id="4fc3a-156">To znamená, že pokud zadáte souřadnice z hlediska zeměpisnou délku a šířku, některé hodnoty klienta vyhodnocen jako vzdálenost, délku a oblast bude ve stupních, ne měřiče.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-156">This means that if you specify coordinates in terms of longitude and latitude, some client-evaluated values like distance, length, and area will be in degrees, not meters.</span></span> <span data-ttu-id="4fc3a-157">Pro více smysluplné hodnoty, je nutné nejprve do projektu souřadnice, kde jiný souřadnicový systém pomocí knihovny jako [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) před výpočtem tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-157">For more meaningful values, you first need to project the coordinates to another coordinate system using a library like [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) before calculating these values.</span></span>

<span data-ttu-id="4fc3a-158">Pokud operace serveru vyhodnocovaný EF Core pomocí SQL, určí databáze jednotky výsledek.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-158">If an operation is server-evaluated by EF Core via SQL, the result's unit will be determined by the database.</span></span>

<span data-ttu-id="4fc3a-159">Tady je příklad použití ProjNet4GeoAPI k výpočtu vzdálenost mezi dvěma měst.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-159">Here is an example of using ProjNet4GeoAPI to calculate the distance between two cities.</span></span>

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

## <a name="querying-data"></a><span data-ttu-id="4fc3a-160">Dotazování na data</span><span class="sxs-lookup"><span data-stu-id="4fc3a-160">Querying Data</span></span>

<span data-ttu-id="4fc3a-161">V technologii LINQ ny Klienty metody a vlastnosti, které jsou k dispozici jako funkce databáze bude do kódu SQL.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-161">In LINQ, the NTS methods and properties available as database functions will be translated to SQL.</span></span> <span data-ttu-id="4fc3a-162">Například jsou přeloženy metody vzdálenosti a obsahuje následující dotazy.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-162">For example, the Distance and Contains methods are translated in the following queries.</span></span> <span data-ttu-id="4fc3a-163">V tabulce na konci tohoto článku jsou uvedeny podporované členy podle různých zprostředkovatelů EF Core.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-163">The table at the end of this article shows which members are supported by various EF Core providers.</span></span>

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a><span data-ttu-id="4fc3a-164">SQL Server</span><span class="sxs-lookup"><span data-stu-id="4fc3a-164">SQL Server</span></span>

<span data-ttu-id="4fc3a-165">Pokud používáte systém SQL Server, existují některé další věci, které byste měli vědět.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-165">If you're using SQL Server, there are some additional things you should be aware of.</span></span>

### <a name="geography-or-geometry"></a><span data-ttu-id="4fc3a-166">Zeměpisné oblasti nebo geometrie</span><span class="sxs-lookup"><span data-stu-id="4fc3a-166">Geography or geometry</span></span>

<span data-ttu-id="4fc3a-167">Ve výchozím nastavení, prostorová vlastností se mapují na `geography` sloupce v systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-167">By default, spatial properties are mapped to `geography` columns in SQL Server.</span></span> <span data-ttu-id="4fc3a-168">Chcete-li použít `geometry`, [nakonfigurovat typ sloupce](xref:core/modeling/relational/data-types) ve vašem modelu.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-168">To use `geometry`, [configure the column type](xref:core/modeling/relational/data-types) in your model.</span></span>

### <a name="geography-polygon-rings"></a><span data-ttu-id="4fc3a-169">Zeměpisné oblasti prstence mnohoúhelníku</span><span class="sxs-lookup"><span data-stu-id="4fc3a-169">Geography polygon rings</span></span>

<span data-ttu-id="4fc3a-170">Při použití `geography` typ sloupce, SQL Server vyžaduje další požadavky na vnější prstenec (nebo prostředí) a vnitřní okruhů (nebo děr).</span><span class="sxs-lookup"><span data-stu-id="4fc3a-170">When using the `geography` column type, SQL Server imposes additional requirements on the exterior ring (or shell) and interior rings (or holes).</span></span> <span data-ttu-id="4fc3a-171">Vnější prstenec musí být orientovaný proti směru hodinových ručiček a vnitřní prstenci po směru hodinových ručiček.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-171">The exterior ring must be oriented counterclockwise and the interior rings clockwise.</span></span> <span data-ttu-id="4fc3a-172">Směřuje ověří to před odesláním hodnoty do databáze.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-172">NTS validates this before sending values to the database.</span></span>

### <a name="fullglobe"></a><span data-ttu-id="4fc3a-173">Objekt FullGlobe</span><span class="sxs-lookup"><span data-stu-id="4fc3a-173">FullGlobe</span></span>

<span data-ttu-id="4fc3a-174">Systém SQL Server má nestandardní geometrie typ pro reprezentaci úplné světě při použití `geography` typ sloupce.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-174">SQL Server has a non-standard geometry type to represent the full globe when using the `geography` column type.</span></span> <span data-ttu-id="4fc3a-175">Má také způsob, jak reprezentaci mnohoúhelníky založené na celé zeměkouli (bez vnější prstenec).</span><span class="sxs-lookup"><span data-stu-id="4fc3a-175">It also has a way to represent polygons based on the full globe (without an exterior ring).</span></span> <span data-ttu-id="4fc3a-176">Ani jeden z těchto podporovaných chny Zarážky.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-176">Neither of these are supported by NTS.</span></span>

> [!WARNING]
> <span data-ttu-id="4fc3a-177">Objekt FullGlobe a na jejím základě mnohoúhelníky chny Zarážky nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-177">FullGlobe and polygons based on it aren't supported by NTS.</span></span>

## <a name="sqlite"></a><span data-ttu-id="4fc3a-178">SQLite</span><span class="sxs-lookup"><span data-stu-id="4fc3a-178">SQLite</span></span>

<span data-ttu-id="4fc3a-179">Zde jsou některé další informace k těm, kteří používají SQLite.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-179">Here is some additional information for those using SQLite.</span></span>

### <a name="installing-spatialite"></a><span data-ttu-id="4fc3a-180">Instalace SpatiaLite</span><span class="sxs-lookup"><span data-stu-id="4fc3a-180">Installing SpatiaLite</span></span>

<span data-ttu-id="4fc3a-181">Na Windows nativní mod_spatialite knihovny je distribuován jako závislost balíčku NuGet.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-181">On Windows, the native mod_spatialite library is distributed as a NuGet package dependency.</span></span> <span data-ttu-id="4fc3a-182">Jiné platformy je nutné nainstalovat samostatně.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-182">Other platforms need to install it separately.</span></span> <span data-ttu-id="4fc3a-183">To se obvykle provádí pomocí Správce balíčků softwaru.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-183">This is typically done using a software package manager.</span></span> <span data-ttu-id="4fc3a-184">Můžete například použít APT na Ubuntu a Homebrew v systému MacOS.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-184">For example, you can use APT on Ubuntu and Homebrew on MacOS.</span></span>

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

### <a name="configuring-srid"></a><span data-ttu-id="4fc3a-185">Konfigurace SRID</span><span class="sxs-lookup"><span data-stu-id="4fc3a-185">Configuring SRID</span></span>

<span data-ttu-id="4fc3a-186">Sloupce v SpatiaLite, třeba zadat SRID na sloupec.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-186">In SpatiaLite, columns need to specify an SRID per column.</span></span> <span data-ttu-id="4fc3a-187">Výchozí hodnota je SRID `0`.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-187">The default SRID is `0`.</span></span> <span data-ttu-id="4fc3a-188">Zadejte jiný SRID ForSqliteHasSrid metodou.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-188">Specify a different SRID using the ForSqliteHasSrid method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a><span data-ttu-id="4fc3a-189">Rozměr</span><span class="sxs-lookup"><span data-stu-id="4fc3a-189">Dimension</span></span>

<span data-ttu-id="4fc3a-190">Podobně jako u SRID, dimenze sloupec (nebo souřadnice) je také zadaný jako součást sloupci.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-190">Similar to SRID, a column's dimension (or ordinates) is also specified as part of the column.</span></span> <span data-ttu-id="4fc3a-191">Jsou výchozí souřadnice X a Y. povolit další souřadnice (Z a M) ForSqliteHasDimension metodou.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-191">The default ordinates are X and Y. Enable additional ordinates (Z and M) using the ForSqliteHasDimension method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a><span data-ttu-id="4fc3a-192">Přeložené operace</span><span class="sxs-lookup"><span data-stu-id="4fc3a-192">Translated Operations</span></span>

<span data-ttu-id="4fc3a-193">Tato tabulka uvádí, které členy chny Zarážky jsou přeloženy do SQL od každého poskytovatele EF Core.</span><span class="sxs-lookup"><span data-stu-id="4fc3a-193">This table shows which NTS members are translated into SQL by each EF Core provider.</span></span>

<span data-ttu-id="4fc3a-194">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="4fc3a-194">NetTopologySuite</span></span> | <span data-ttu-id="4fc3a-195">SQL Server (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="4fc3a-195">SQL Server (geometry)</span></span> | <span data-ttu-id="4fc3a-196">SQL Server (geography)</span><span class="sxs-lookup"><span data-stu-id="4fc3a-196">SQL Server (geography)</span></span> | <span data-ttu-id="4fc3a-197">SQLite</span><span class="sxs-lookup"><span data-stu-id="4fc3a-197">SQLite</span></span> | <span data-ttu-id="4fc3a-198">Npgsql</span><span class="sxs-lookup"><span data-stu-id="4fc3a-198">Npgsql</span></span>
--- |:---:|:---:|:---:|:---:
<span data-ttu-id="4fc3a-199">Geometry.Area</span><span class="sxs-lookup"><span data-stu-id="4fc3a-199">Geometry.Area</span></span> | <span data-ttu-id="4fc3a-200">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-200">✔</span></span> | <span data-ttu-id="4fc3a-201">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-201">✔</span></span> | <span data-ttu-id="4fc3a-202">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-202">✔</span></span> | <span data-ttu-id="4fc3a-203">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-203">✔</span></span>
<span data-ttu-id="4fc3a-204">Geometry.AsBinary()</span><span class="sxs-lookup"><span data-stu-id="4fc3a-204">Geometry.AsBinary()</span></span> | <span data-ttu-id="4fc3a-205">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-205">✔</span></span> | <span data-ttu-id="4fc3a-206">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-206">✔</span></span> | <span data-ttu-id="4fc3a-207">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-207">✔</span></span> | <span data-ttu-id="4fc3a-208">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-208">✔</span></span>
<span data-ttu-id="4fc3a-209">Geometry.AsText()</span><span class="sxs-lookup"><span data-stu-id="4fc3a-209">Geometry.AsText()</span></span> | <span data-ttu-id="4fc3a-210">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-210">✔</span></span> | <span data-ttu-id="4fc3a-211">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-211">✔</span></span> | <span data-ttu-id="4fc3a-212">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-212">✔</span></span> | <span data-ttu-id="4fc3a-213">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-213">✔</span></span>
<span data-ttu-id="4fc3a-214">Geometry.Boundary</span><span class="sxs-lookup"><span data-stu-id="4fc3a-214">Geometry.Boundary</span></span> | <span data-ttu-id="4fc3a-215">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-215">✔</span></span> | | <span data-ttu-id="4fc3a-216">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-216">✔</span></span> | <span data-ttu-id="4fc3a-217">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-217">✔</span></span>
<span data-ttu-id="4fc3a-218">Geometry.Buffer(double)</span><span class="sxs-lookup"><span data-stu-id="4fc3a-218">Geometry.Buffer(double)</span></span> | <span data-ttu-id="4fc3a-219">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-219">✔</span></span> | <span data-ttu-id="4fc3a-220">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-220">✔</span></span> | <span data-ttu-id="4fc3a-221">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-221">✔</span></span> | <span data-ttu-id="4fc3a-222">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-222">✔</span></span>
<span data-ttu-id="4fc3a-223">Geometry.Buffer (double, int).</span><span class="sxs-lookup"><span data-stu-id="4fc3a-223">Geometry.Buffer(double, int)</span></span> | | | <span data-ttu-id="4fc3a-224">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-224">✔</span></span>
<span data-ttu-id="4fc3a-225">Geometry.Centroid</span><span class="sxs-lookup"><span data-stu-id="4fc3a-225">Geometry.Centroid</span></span> | <span data-ttu-id="4fc3a-226">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-226">✔</span></span> | | <span data-ttu-id="4fc3a-227">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-227">✔</span></span> | <span data-ttu-id="4fc3a-228">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-228">✔</span></span>
<span data-ttu-id="4fc3a-229">Geometry.Contains(Geometry)</span><span class="sxs-lookup"><span data-stu-id="4fc3a-229">Geometry.Contains(Geometry)</span></span> | <span data-ttu-id="4fc3a-230">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-230">✔</span></span> | <span data-ttu-id="4fc3a-231">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-231">✔</span></span> | <span data-ttu-id="4fc3a-232">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-232">✔</span></span> | <span data-ttu-id="4fc3a-233">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-233">✔</span></span>
<span data-ttu-id="4fc3a-234">Geometry.ConvexHull()</span><span class="sxs-lookup"><span data-stu-id="4fc3a-234">Geometry.ConvexHull()</span></span> | <span data-ttu-id="4fc3a-235">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-235">✔</span></span> | <span data-ttu-id="4fc3a-236">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-236">✔</span></span> | <span data-ttu-id="4fc3a-237">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-237">✔</span></span> | <span data-ttu-id="4fc3a-238">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-238">✔</span></span>
<span data-ttu-id="4fc3a-239">Geometry.CoveredBy(Geometry)</span><span class="sxs-lookup"><span data-stu-id="4fc3a-239">Geometry.CoveredBy(Geometry)</span></span> | | | <span data-ttu-id="4fc3a-240">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-240">✔</span></span> | <span data-ttu-id="4fc3a-241">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-241">✔</span></span>
<span data-ttu-id="4fc3a-242">Geometry.Covers(Geometry)</span><span class="sxs-lookup"><span data-stu-id="4fc3a-242">Geometry.Covers(Geometry)</span></span> | | | <span data-ttu-id="4fc3a-243">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-243">✔</span></span> | <span data-ttu-id="4fc3a-244">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-244">✔</span></span>
<span data-ttu-id="4fc3a-245">Geometry.Crosses(Geometry)</span><span class="sxs-lookup"><span data-stu-id="4fc3a-245">Geometry.Crosses(Geometry)</span></span> | <span data-ttu-id="4fc3a-246">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-246">✔</span></span> | | <span data-ttu-id="4fc3a-247">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-247">✔</span></span> | <span data-ttu-id="4fc3a-248">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-248">✔</span></span>
<span data-ttu-id="4fc3a-249">Geometry.Difference(Geometry)</span><span class="sxs-lookup"><span data-stu-id="4fc3a-249">Geometry.Difference(Geometry)</span></span> | <span data-ttu-id="4fc3a-250">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-250">✔</span></span> | <span data-ttu-id="4fc3a-251">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-251">✔</span></span> | <span data-ttu-id="4fc3a-252">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-252">✔</span></span> | <span data-ttu-id="4fc3a-253">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-253">✔</span></span>
<span data-ttu-id="4fc3a-254">Geometry.Dimension</span><span class="sxs-lookup"><span data-stu-id="4fc3a-254">Geometry.Dimension</span></span> | <span data-ttu-id="4fc3a-255">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-255">✔</span></span> | <span data-ttu-id="4fc3a-256">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-256">✔</span></span> | <span data-ttu-id="4fc3a-257">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-257">✔</span></span> | <span data-ttu-id="4fc3a-258">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-258">✔</span></span>
<span data-ttu-id="4fc3a-259">Geometry.Disjoint(Geometry)</span><span class="sxs-lookup"><span data-stu-id="4fc3a-259">Geometry.Disjoint(Geometry)</span></span> | <span data-ttu-id="4fc3a-260">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-260">✔</span></span> | <span data-ttu-id="4fc3a-261">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-261">✔</span></span> | <span data-ttu-id="4fc3a-262">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-262">✔</span></span> | <span data-ttu-id="4fc3a-263">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-263">✔</span></span>
<span data-ttu-id="4fc3a-264">Geometry.Distance(Geometry)</span><span class="sxs-lookup"><span data-stu-id="4fc3a-264">Geometry.Distance(Geometry)</span></span> | <span data-ttu-id="4fc3a-265">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-265">✔</span></span> | <span data-ttu-id="4fc3a-266">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-266">✔</span></span> | <span data-ttu-id="4fc3a-267">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-267">✔</span></span> | <span data-ttu-id="4fc3a-268">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-268">✔</span></span>
<span data-ttu-id="4fc3a-269">Geometry.Envelope</span><span class="sxs-lookup"><span data-stu-id="4fc3a-269">Geometry.Envelope</span></span> | <span data-ttu-id="4fc3a-270">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-270">✔</span></span> | | <span data-ttu-id="4fc3a-271">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-271">✔</span></span> | <span data-ttu-id="4fc3a-272">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-272">✔</span></span>
<span data-ttu-id="4fc3a-273">Geometry.EqualsExact(Geometry)</span><span class="sxs-lookup"><span data-stu-id="4fc3a-273">Geometry.EqualsExact(Geometry)</span></span> | | | | <span data-ttu-id="4fc3a-274">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-274">✔</span></span>
<span data-ttu-id="4fc3a-275">Geometry.EqualsTopologically(Geometry)</span><span class="sxs-lookup"><span data-stu-id="4fc3a-275">Geometry.EqualsTopologically(Geometry)</span></span> | <span data-ttu-id="4fc3a-276">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-276">✔</span></span> | <span data-ttu-id="4fc3a-277">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-277">✔</span></span> | <span data-ttu-id="4fc3a-278">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-278">✔</span></span> | <span data-ttu-id="4fc3a-279">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-279">✔</span></span>
<span data-ttu-id="4fc3a-280">Geometry.GeometryType</span><span class="sxs-lookup"><span data-stu-id="4fc3a-280">Geometry.GeometryType</span></span> | <span data-ttu-id="4fc3a-281">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-281">✔</span></span> | <span data-ttu-id="4fc3a-282">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-282">✔</span></span> | <span data-ttu-id="4fc3a-283">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-283">✔</span></span> | <span data-ttu-id="4fc3a-284">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-284">✔</span></span>
<span data-ttu-id="4fc3a-285">Geometry.GetGeometryN(int)</span><span class="sxs-lookup"><span data-stu-id="4fc3a-285">Geometry.GetGeometryN(int)</span></span> | <span data-ttu-id="4fc3a-286">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-286">✔</span></span> | | <span data-ttu-id="4fc3a-287">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-287">✔</span></span> | <span data-ttu-id="4fc3a-288">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-288">✔</span></span>
<span data-ttu-id="4fc3a-289">Geometry.InteriorPoint</span><span class="sxs-lookup"><span data-stu-id="4fc3a-289">Geometry.InteriorPoint</span></span> | <span data-ttu-id="4fc3a-290">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-290">✔</span></span> | | <span data-ttu-id="4fc3a-291">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-291">✔</span></span>
<span data-ttu-id="4fc3a-292">Geometry.Intersection(Geometry)</span><span class="sxs-lookup"><span data-stu-id="4fc3a-292">Geometry.Intersection(Geometry)</span></span> | <span data-ttu-id="4fc3a-293">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-293">✔</span></span> | <span data-ttu-id="4fc3a-294">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-294">✔</span></span> | <span data-ttu-id="4fc3a-295">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-295">✔</span></span> | <span data-ttu-id="4fc3a-296">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-296">✔</span></span>
<span data-ttu-id="4fc3a-297">Geometry.Intersects(Geometry)</span><span class="sxs-lookup"><span data-stu-id="4fc3a-297">Geometry.Intersects(Geometry)</span></span> | <span data-ttu-id="4fc3a-298">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-298">✔</span></span> | <span data-ttu-id="4fc3a-299">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-299">✔</span></span> | <span data-ttu-id="4fc3a-300">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-300">✔</span></span> | <span data-ttu-id="4fc3a-301">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-301">✔</span></span>
<span data-ttu-id="4fc3a-302">Geometry.IsEmpty</span><span class="sxs-lookup"><span data-stu-id="4fc3a-302">Geometry.IsEmpty</span></span> | <span data-ttu-id="4fc3a-303">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-303">✔</span></span> | <span data-ttu-id="4fc3a-304">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-304">✔</span></span> | <span data-ttu-id="4fc3a-305">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-305">✔</span></span> | <span data-ttu-id="4fc3a-306">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-306">✔</span></span>
<span data-ttu-id="4fc3a-307">Geometry.IsSimple</span><span class="sxs-lookup"><span data-stu-id="4fc3a-307">Geometry.IsSimple</span></span> | <span data-ttu-id="4fc3a-308">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-308">✔</span></span> | | <span data-ttu-id="4fc3a-309">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-309">✔</span></span> | <span data-ttu-id="4fc3a-310">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-310">✔</span></span>
<span data-ttu-id="4fc3a-311">Geometry.IsValid</span><span class="sxs-lookup"><span data-stu-id="4fc3a-311">Geometry.IsValid</span></span> | <span data-ttu-id="4fc3a-312">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-312">✔</span></span> | <span data-ttu-id="4fc3a-313">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-313">✔</span></span> | <span data-ttu-id="4fc3a-314">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-314">✔</span></span> | <span data-ttu-id="4fc3a-315">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-315">✔</span></span>
<span data-ttu-id="4fc3a-316">Geometry.IsWithinDistance (geometrie, double)</span><span class="sxs-lookup"><span data-stu-id="4fc3a-316">Geometry.IsWithinDistance(Geometry, double)</span></span> | <span data-ttu-id="4fc3a-317">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-317">✔</span></span> | | <span data-ttu-id="4fc3a-318">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-318">✔</span></span>
<span data-ttu-id="4fc3a-319">Geometry.Length</span><span class="sxs-lookup"><span data-stu-id="4fc3a-319">Geometry.Length</span></span> | <span data-ttu-id="4fc3a-320">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-320">✔</span></span> | <span data-ttu-id="4fc3a-321">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-321">✔</span></span> | <span data-ttu-id="4fc3a-322">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-322">✔</span></span> | <span data-ttu-id="4fc3a-323">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-323">✔</span></span>
<span data-ttu-id="4fc3a-324">Geometry.NumGeometries</span><span class="sxs-lookup"><span data-stu-id="4fc3a-324">Geometry.NumGeometries</span></span> | <span data-ttu-id="4fc3a-325">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-325">✔</span></span> | <span data-ttu-id="4fc3a-326">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-326">✔</span></span> | <span data-ttu-id="4fc3a-327">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-327">✔</span></span> | <span data-ttu-id="4fc3a-328">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-328">✔</span></span>
<span data-ttu-id="4fc3a-329">Geometry.NumPoints</span><span class="sxs-lookup"><span data-stu-id="4fc3a-329">Geometry.NumPoints</span></span> | <span data-ttu-id="4fc3a-330">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-330">✔</span></span> | <span data-ttu-id="4fc3a-331">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-331">✔</span></span> | <span data-ttu-id="4fc3a-332">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-332">✔</span></span> | <span data-ttu-id="4fc3a-333">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-333">✔</span></span>
<span data-ttu-id="4fc3a-334">Geometry.OgcGeometryType</span><span class="sxs-lookup"><span data-stu-id="4fc3a-334">Geometry.OgcGeometryType</span></span> | <span data-ttu-id="4fc3a-335">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-335">✔</span></span> | <span data-ttu-id="4fc3a-336">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-336">✔</span></span> | <span data-ttu-id="4fc3a-337">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-337">✔</span></span>
<span data-ttu-id="4fc3a-338">Geometry.Overlaps(Geometry)</span><span class="sxs-lookup"><span data-stu-id="4fc3a-338">Geometry.Overlaps(Geometry)</span></span> | <span data-ttu-id="4fc3a-339">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-339">✔</span></span> | <span data-ttu-id="4fc3a-340">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-340">✔</span></span> | <span data-ttu-id="4fc3a-341">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-341">✔</span></span> | <span data-ttu-id="4fc3a-342">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-342">✔</span></span>
<span data-ttu-id="4fc3a-343">Geometry.PointOnSurface</span><span class="sxs-lookup"><span data-stu-id="4fc3a-343">Geometry.PointOnSurface</span></span> | <span data-ttu-id="4fc3a-344">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-344">✔</span></span> | | <span data-ttu-id="4fc3a-345">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-345">✔</span></span> | <span data-ttu-id="4fc3a-346">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-346">✔</span></span>
<span data-ttu-id="4fc3a-347">Geometry.Relate (geometrie, string)</span><span class="sxs-lookup"><span data-stu-id="4fc3a-347">Geometry.Relate(Geometry, string)</span></span> | <span data-ttu-id="4fc3a-348">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-348">✔</span></span> | | <span data-ttu-id="4fc3a-349">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-349">✔</span></span> | <span data-ttu-id="4fc3a-350">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-350">✔</span></span>
<span data-ttu-id="4fc3a-351">Geometry.Reverse()</span><span class="sxs-lookup"><span data-stu-id="4fc3a-351">Geometry.Reverse()</span></span> | | | <span data-ttu-id="4fc3a-352">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-352">✔</span></span> | <span data-ttu-id="4fc3a-353">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-353">✔</span></span>
<span data-ttu-id="4fc3a-354">Geometry.SRID</span><span class="sxs-lookup"><span data-stu-id="4fc3a-354">Geometry.SRID</span></span> | <span data-ttu-id="4fc3a-355">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-355">✔</span></span> | <span data-ttu-id="4fc3a-356">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-356">✔</span></span> | <span data-ttu-id="4fc3a-357">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-357">✔</span></span> | <span data-ttu-id="4fc3a-358">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-358">✔</span></span>
<span data-ttu-id="4fc3a-359">Geometry.SymmetricDifference(Geometry)</span><span class="sxs-lookup"><span data-stu-id="4fc3a-359">Geometry.SymmetricDifference(Geometry)</span></span> | <span data-ttu-id="4fc3a-360">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-360">✔</span></span> | <span data-ttu-id="4fc3a-361">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-361">✔</span></span> | <span data-ttu-id="4fc3a-362">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-362">✔</span></span> | <span data-ttu-id="4fc3a-363">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-363">✔</span></span>
<span data-ttu-id="4fc3a-364">Geometry.ToBinary()</span><span class="sxs-lookup"><span data-stu-id="4fc3a-364">Geometry.ToBinary()</span></span> | <span data-ttu-id="4fc3a-365">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-365">✔</span></span> | <span data-ttu-id="4fc3a-366">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-366">✔</span></span> | <span data-ttu-id="4fc3a-367">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-367">✔</span></span> | <span data-ttu-id="4fc3a-368">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-368">✔</span></span>
<span data-ttu-id="4fc3a-369">Geometry.ToText()</span><span class="sxs-lookup"><span data-stu-id="4fc3a-369">Geometry.ToText()</span></span> | <span data-ttu-id="4fc3a-370">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-370">✔</span></span> | <span data-ttu-id="4fc3a-371">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-371">✔</span></span> | <span data-ttu-id="4fc3a-372">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-372">✔</span></span> | <span data-ttu-id="4fc3a-373">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-373">✔</span></span>
<span data-ttu-id="4fc3a-374">Geometry.Touches(Geometry)</span><span class="sxs-lookup"><span data-stu-id="4fc3a-374">Geometry.Touches(Geometry)</span></span> | <span data-ttu-id="4fc3a-375">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-375">✔</span></span> | | <span data-ttu-id="4fc3a-376">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-376">✔</span></span> | <span data-ttu-id="4fc3a-377">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-377">✔</span></span>
<span data-ttu-id="4fc3a-378">Geometry.Union()</span><span class="sxs-lookup"><span data-stu-id="4fc3a-378">Geometry.Union()</span></span> | | | <span data-ttu-id="4fc3a-379">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-379">✔</span></span>
<span data-ttu-id="4fc3a-380">Geometry.Union(Geometry)</span><span class="sxs-lookup"><span data-stu-id="4fc3a-380">Geometry.Union(Geometry)</span></span> | <span data-ttu-id="4fc3a-381">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-381">✔</span></span> | <span data-ttu-id="4fc3a-382">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-382">✔</span></span> | <span data-ttu-id="4fc3a-383">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-383">✔</span></span> | <span data-ttu-id="4fc3a-384">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-384">✔</span></span>
<span data-ttu-id="4fc3a-385">Geometry.Within(Geometry)</span><span class="sxs-lookup"><span data-stu-id="4fc3a-385">Geometry.Within(Geometry)</span></span> | <span data-ttu-id="4fc3a-386">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-386">✔</span></span> | <span data-ttu-id="4fc3a-387">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-387">✔</span></span> | <span data-ttu-id="4fc3a-388">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-388">✔</span></span> | <span data-ttu-id="4fc3a-389">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-389">✔</span></span>
<span data-ttu-id="4fc3a-390">GeometryCollection.Count</span><span class="sxs-lookup"><span data-stu-id="4fc3a-390">GeometryCollection.Count</span></span> | <span data-ttu-id="4fc3a-391">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-391">✔</span></span> | <span data-ttu-id="4fc3a-392">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-392">✔</span></span> | <span data-ttu-id="4fc3a-393">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-393">✔</span></span> | <span data-ttu-id="4fc3a-394">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-394">✔</span></span>
<span data-ttu-id="4fc3a-395">GeometryCollection [int]</span><span class="sxs-lookup"><span data-stu-id="4fc3a-395">GeometryCollection[int]</span></span> | <span data-ttu-id="4fc3a-396">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-396">✔</span></span> | <span data-ttu-id="4fc3a-397">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-397">✔</span></span> | <span data-ttu-id="4fc3a-398">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-398">✔</span></span> | <span data-ttu-id="4fc3a-399">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-399">✔</span></span>
<span data-ttu-id="4fc3a-400">LineString.Count</span><span class="sxs-lookup"><span data-stu-id="4fc3a-400">LineString.Count</span></span> | <span data-ttu-id="4fc3a-401">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-401">✔</span></span> | <span data-ttu-id="4fc3a-402">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-402">✔</span></span> | <span data-ttu-id="4fc3a-403">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-403">✔</span></span> | <span data-ttu-id="4fc3a-404">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-404">✔</span></span>
<span data-ttu-id="4fc3a-405">LineString.EndPoint</span><span class="sxs-lookup"><span data-stu-id="4fc3a-405">LineString.EndPoint</span></span> | <span data-ttu-id="4fc3a-406">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-406">✔</span></span> | <span data-ttu-id="4fc3a-407">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-407">✔</span></span> | <span data-ttu-id="4fc3a-408">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-408">✔</span></span> | <span data-ttu-id="4fc3a-409">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-409">✔</span></span>
<span data-ttu-id="4fc3a-410">LineString.GetPointN(int)</span><span class="sxs-lookup"><span data-stu-id="4fc3a-410">LineString.GetPointN(int)</span></span> | <span data-ttu-id="4fc3a-411">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-411">✔</span></span> | <span data-ttu-id="4fc3a-412">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-412">✔</span></span> | <span data-ttu-id="4fc3a-413">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-413">✔</span></span> | <span data-ttu-id="4fc3a-414">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-414">✔</span></span>
<span data-ttu-id="4fc3a-415">LineString.IsClosed</span><span class="sxs-lookup"><span data-stu-id="4fc3a-415">LineString.IsClosed</span></span> | <span data-ttu-id="4fc3a-416">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-416">✔</span></span> | <span data-ttu-id="4fc3a-417">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-417">✔</span></span> | <span data-ttu-id="4fc3a-418">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-418">✔</span></span> | <span data-ttu-id="4fc3a-419">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-419">✔</span></span>
<span data-ttu-id="4fc3a-420">LineString.IsRing</span><span class="sxs-lookup"><span data-stu-id="4fc3a-420">LineString.IsRing</span></span> | <span data-ttu-id="4fc3a-421">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-421">✔</span></span> | | <span data-ttu-id="4fc3a-422">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-422">✔</span></span> | <span data-ttu-id="4fc3a-423">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-423">✔</span></span>
<span data-ttu-id="4fc3a-424">LineString.StartPoint</span><span class="sxs-lookup"><span data-stu-id="4fc3a-424">LineString.StartPoint</span></span> | <span data-ttu-id="4fc3a-425">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-425">✔</span></span> | <span data-ttu-id="4fc3a-426">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-426">✔</span></span> | <span data-ttu-id="4fc3a-427">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-427">✔</span></span> | <span data-ttu-id="4fc3a-428">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-428">✔</span></span>
<span data-ttu-id="4fc3a-429">MultiLineString.IsClosed</span><span class="sxs-lookup"><span data-stu-id="4fc3a-429">MultiLineString.IsClosed</span></span> | <span data-ttu-id="4fc3a-430">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-430">✔</span></span> | <span data-ttu-id="4fc3a-431">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-431">✔</span></span> | <span data-ttu-id="4fc3a-432">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-432">✔</span></span> | <span data-ttu-id="4fc3a-433">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-433">✔</span></span>
<span data-ttu-id="4fc3a-434">Point.M</span><span class="sxs-lookup"><span data-stu-id="4fc3a-434">Point.M</span></span> | <span data-ttu-id="4fc3a-435">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-435">✔</span></span> | <span data-ttu-id="4fc3a-436">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-436">✔</span></span> | <span data-ttu-id="4fc3a-437">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-437">✔</span></span> | <span data-ttu-id="4fc3a-438">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-438">✔</span></span>
<span data-ttu-id="4fc3a-439">Point.X</span><span class="sxs-lookup"><span data-stu-id="4fc3a-439">Point.X</span></span> | <span data-ttu-id="4fc3a-440">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-440">✔</span></span> | <span data-ttu-id="4fc3a-441">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-441">✔</span></span> | <span data-ttu-id="4fc3a-442">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-442">✔</span></span> | <span data-ttu-id="4fc3a-443">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-443">✔</span></span>
<span data-ttu-id="4fc3a-444">Point.Y</span><span class="sxs-lookup"><span data-stu-id="4fc3a-444">Point.Y</span></span> | <span data-ttu-id="4fc3a-445">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-445">✔</span></span> | <span data-ttu-id="4fc3a-446">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-446">✔</span></span> | <span data-ttu-id="4fc3a-447">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-447">✔</span></span> | <span data-ttu-id="4fc3a-448">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-448">✔</span></span>
<span data-ttu-id="4fc3a-449">Point.Z</span><span class="sxs-lookup"><span data-stu-id="4fc3a-449">Point.Z</span></span> | <span data-ttu-id="4fc3a-450">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-450">✔</span></span> | <span data-ttu-id="4fc3a-451">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-451">✔</span></span> | <span data-ttu-id="4fc3a-452">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-452">✔</span></span> | <span data-ttu-id="4fc3a-453">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-453">✔</span></span>
<span data-ttu-id="4fc3a-454">Polygon.ExteriorRing</span><span class="sxs-lookup"><span data-stu-id="4fc3a-454">Polygon.ExteriorRing</span></span> | <span data-ttu-id="4fc3a-455">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-455">✔</span></span> | <span data-ttu-id="4fc3a-456">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-456">✔</span></span> | <span data-ttu-id="4fc3a-457">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-457">✔</span></span> | <span data-ttu-id="4fc3a-458">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-458">✔</span></span>
<span data-ttu-id="4fc3a-459">Polygon.GetInteriorRingN(int)</span><span class="sxs-lookup"><span data-stu-id="4fc3a-459">Polygon.GetInteriorRingN(int)</span></span> | <span data-ttu-id="4fc3a-460">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-460">✔</span></span> | <span data-ttu-id="4fc3a-461">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-461">✔</span></span> | <span data-ttu-id="4fc3a-462">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-462">✔</span></span> | <span data-ttu-id="4fc3a-463">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-463">✔</span></span>
<span data-ttu-id="4fc3a-464">Polygon.NumInteriorRings</span><span class="sxs-lookup"><span data-stu-id="4fc3a-464">Polygon.NumInteriorRings</span></span> | <span data-ttu-id="4fc3a-465">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-465">✔</span></span> | <span data-ttu-id="4fc3a-466">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-466">✔</span></span> | <span data-ttu-id="4fc3a-467">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-467">✔</span></span> | <span data-ttu-id="4fc3a-468">✔</span><span class="sxs-lookup"><span data-stu-id="4fc3a-468">✔</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4fc3a-469">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="4fc3a-469">Additional resources</span></span>

* [<span data-ttu-id="4fc3a-470">Prostorová Data v systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="4fc3a-470">Spatial Data in SQL Server</span></span>](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [<span data-ttu-id="4fc3a-471">Domovská stránka SpatiaLite</span><span class="sxs-lookup"><span data-stu-id="4fc3a-471">SpatiaLite Homepage</span></span>](https://www.gaia-gis.it/fossil/libspatialite)
* [<span data-ttu-id="4fc3a-472">Prostorový dokumentaci Npgsql</span><span class="sxs-lookup"><span data-stu-id="4fc3a-472">Npgsql Spatial Documentation</span></span>](http://www.npgsql.org/efcore/mapping/nts.html)
* [<span data-ttu-id="4fc3a-473">Dokumentace ke službě PostGIS</span><span class="sxs-lookup"><span data-stu-id="4fc3a-473">PostGIS Documentation</span></span>](http://postgis.net/documentation/)
