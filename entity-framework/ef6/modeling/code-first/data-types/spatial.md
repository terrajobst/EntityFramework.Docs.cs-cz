---
title: Prostorové Code First – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d617aed1-15f2-48a9-b187-186991c666e3
ms.openlocfilehash: 018f480c1f0f1e74fc9f7a8950a6880e96f1facc
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182653"
---
# <a name="spatial---code-first"></a><span data-ttu-id="3b06b-102">Prostorové Code First</span><span class="sxs-lookup"><span data-stu-id="3b06b-102">Spatial - Code First</span></span>
> [!NOTE]
> <span data-ttu-id="3b06b-103">**EF5 pouze** funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="3b06b-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="3b06b-104">Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.</span><span class="sxs-lookup"><span data-stu-id="3b06b-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="3b06b-105">Video a podrobný návod vám ukáže, jak namapovat prostorové typy pomocí Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="3b06b-105">The video and step-by-step walkthrough shows how to map spatial types with Entity Framework Code First.</span></span> <span data-ttu-id="3b06b-106">Také ukazuje, jak použít dotaz LINQ k nalezení vzdálenosti mezi dvěma umístěními.</span><span class="sxs-lookup"><span data-stu-id="3b06b-106">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="3b06b-107">Tento návod použije Code First k vytvoření nové databáze, ale můžete také použít [Code First do existující databáze](~/ef6/modeling/code-first/workflows/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="3b06b-107">This walkthrough will use Code First to create a new database, but you can also use [Code First to an existing database](~/ef6/modeling/code-first/workflows/existing-database.md).</span></span>

<span data-ttu-id="3b06b-108">Podpora prostorového typu byla představena v Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="3b06b-108">Spatial type support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="3b06b-109">Všimněte si, že pro použití nových funkcí, jako je prostorový typ, výčty a funkce vracející tabulku, je nutné cílit .NET Framework 4,5.</span><span class="sxs-lookup"><span data-stu-id="3b06b-109">Note that to use the new features like spatial type, enums, and Table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="3b06b-110">Visual Studio 2012 cílí na .NET 4,5 ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="3b06b-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="3b06b-111">Chcete-li použít typy prostorových dat, musíte také použít poskytovatele Entity Framework, který má prostorovou podporu.</span><span class="sxs-lookup"><span data-stu-id="3b06b-111">To use spatial data types you must also use an Entity Framework provider that has spatial support.</span></span> <span data-ttu-id="3b06b-112">Další informace najdete v tématu [Podpora poskytovatele pro prostorové typy](~/ef6/fundamentals/providers/spatial-support.md) .</span><span class="sxs-lookup"><span data-stu-id="3b06b-112">See [provider support for spatial types](~/ef6/fundamentals/providers/spatial-support.md) for more information.</span></span>

<span data-ttu-id="3b06b-113">Existují dva hlavní typy prostorových dat: Geografie a geometrie.</span><span class="sxs-lookup"><span data-stu-id="3b06b-113">There are two main spatial data types: geography and geometry.</span></span> <span data-ttu-id="3b06b-114">Zeměpisný datový typ ukládá data ellipsoidal (například souřadnice GPS zeměpisné šířky a délky).</span><span class="sxs-lookup"><span data-stu-id="3b06b-114">The geography data type stores ellipsoidal data (for example, GPS latitude and longitude coordinates).</span></span> <span data-ttu-id="3b06b-115">Typ dat geometrie představuje systém souřadnic Euclidean (plochý).</span><span class="sxs-lookup"><span data-stu-id="3b06b-115">The geometry data type represents Euclidean (flat) coordinate system.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="3b06b-116">Přehrát video</span><span class="sxs-lookup"><span data-stu-id="3b06b-116">Watch the video</span></span>
<span data-ttu-id="3b06b-117">Toto video ukazuje, jak namapovat prostorové typy pomocí Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="3b06b-117">This video shows how to map spatial types with Entity Framework Code First.</span></span> <span data-ttu-id="3b06b-118">Také ukazuje, jak použít dotaz LINQ k nalezení vzdálenosti mezi dvěma umístěními.</span><span class="sxs-lookup"><span data-stu-id="3b06b-118">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="3b06b-119">**Prezentující**: Helena Kornich</span><span class="sxs-lookup"><span data-stu-id="3b06b-119">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="3b06b-120">**Video**: [WMV](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="3b06b-120">**Video**: [WMV](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="3b06b-121">Předpoklady</span><span class="sxs-lookup"><span data-stu-id="3b06b-121">Pre-Requisites</span></span>

<span data-ttu-id="3b06b-122">K dokončení tohoto Názorného postupu budete muset mít nainstalovanou verzi sady Visual Studio 2012, Ultimate, Premium, Professional nebo web Express.</span><span class="sxs-lookup"><span data-stu-id="3b06b-122">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="3b06b-123">Nastavení projektu</span><span class="sxs-lookup"><span data-stu-id="3b06b-123">Set up the Project</span></span>

1.  <span data-ttu-id="3b06b-124">Otevřete Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="3b06b-124">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="3b06b-125">V nabídce **soubor** přejděte na příkaz **Nový**a potom klikněte na **projekt** .</span><span class="sxs-lookup"><span data-stu-id="3b06b-125">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="3b06b-126">V levém podokně klikněte na položku **Visual C @ no__t-1**a potom vyberte šablonu **konzoly** .</span><span class="sxs-lookup"><span data-stu-id="3b06b-126">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="3b06b-127">Jako název projektu zadejte **SpatialCodeFirst** a klikněte na **OK** .</span><span class="sxs-lookup"><span data-stu-id="3b06b-127">Enter **SpatialCodeFirst** as the name of the project and click **OK**</span></span>

## <a name="define-a-new-model-using-code-first"></a><span data-ttu-id="3b06b-128">Definování nového modelu pomocí Code First</span><span class="sxs-lookup"><span data-stu-id="3b06b-128">Define a New Model using Code First</span></span>

<span data-ttu-id="3b06b-129">Při použití vývoje Code First obvykle začněte psaním .NET Framework tříd, které definují koncepční (doménový) model.</span><span class="sxs-lookup"><span data-stu-id="3b06b-129">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="3b06b-130">Následující kód definuje třídu univerzity.</span><span class="sxs-lookup"><span data-stu-id="3b06b-130">The code below defines the University class.</span></span>

<span data-ttu-id="3b06b-131">Univerzita má vlastnost Location typu DbGeography.</span><span class="sxs-lookup"><span data-stu-id="3b06b-131">The University has the Location property of the DbGeography type.</span></span> <span data-ttu-id="3b06b-132">Chcete-li použít typ DbGeography, musíte přidat odkaz na sestavení System. data. entity a také přidat příkaz System. data. prostor using.</span><span class="sxs-lookup"><span data-stu-id="3b06b-132">To use the DbGeography type, you must add a reference to the System.Data.Entity assembly and also add the System.Data.Spatial using statement.</span></span>

<span data-ttu-id="3b06b-133">Otevřete soubor Program.cs a vložte následující příkazy using do horní části souboru:</span><span class="sxs-lookup"><span data-stu-id="3b06b-133">Open the Program.cs file and paste the following using statements at the top of the file:</span></span>

``` csharp
using System.Data.Spatial;
```

<span data-ttu-id="3b06b-134">Přidejte do souboru Program.cs následující definici třídy University.</span><span class="sxs-lookup"><span data-stu-id="3b06b-134">Add the following University class definition to the Program.cs file.</span></span>

``` csharp
public class University  
{
    public int UniversityID { get; set; }
    public string Name { get; set; }
    public DbGeography Location { get; set; }
}
```

## <a name="define-the-dbcontext-derived-type"></a><span data-ttu-id="3b06b-135">Definice DbContext odvozeného typu</span><span class="sxs-lookup"><span data-stu-id="3b06b-135">Define the DbContext Derived Type</span></span>

<span data-ttu-id="3b06b-136">Kromě definování entit musíte definovat třídu, která je odvozena z DbContext a zpřístupňuje Negenerickými @ no__t-0TEntity @ no__t-1 vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="3b06b-136">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="3b06b-137">Vlastnosti Negenerickými @ no__t-0TEntity @ no__t-1 umožňují, aby kontext věděl, které typy chcete do modelu zahrnout.</span><span class="sxs-lookup"><span data-stu-id="3b06b-137">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="3b06b-138">Instance DbContext odvozeného typu spravuje objekty entit za běhu, což zahrnuje vyplnění objektů daty z databáze, sledování změn a uchování dat do databáze.</span><span class="sxs-lookup"><span data-stu-id="3b06b-138">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

<span data-ttu-id="3b06b-139">Typy DbContext a Negenerickými jsou definovány v sestavení EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="3b06b-139">The DbContext and DbSet types are defined in the EntityFramework assembly.</span></span> <span data-ttu-id="3b06b-140">Do této knihovny DLL přidáme odkaz pomocí balíčku NuGet EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="3b06b-140">We will add a reference to this DLL by using the EntityFramework NuGet package.</span></span>

1.  <span data-ttu-id="3b06b-141">V Průzkumník řešení klikněte pravým tlačítkem myši na název projektu.</span><span class="sxs-lookup"><span data-stu-id="3b06b-141">In Solution Explorer, right-click on the project name.</span></span>
2.  <span data-ttu-id="3b06b-142">Vyberte **Spravovat balíčky NuGet...**</span><span class="sxs-lookup"><span data-stu-id="3b06b-142">Select **Manage NuGet Packages…**</span></span>
3.  <span data-ttu-id="3b06b-143">V dialogovém okně Spravovat balíčky NuGet vyberte kartu **online** a zvolte balíček **EntityFramework** .</span><span class="sxs-lookup"><span data-stu-id="3b06b-143">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package.</span></span>
4.  <span data-ttu-id="3b06b-144">Klikněte na **nainstalovat** .</span><span class="sxs-lookup"><span data-stu-id="3b06b-144">Click **Install**</span></span>

<span data-ttu-id="3b06b-145">Všimněte si, že kromě sestavení EntityFramework je také přidáno odkaz na sestavení System. ComponentModel. DataAnnotations.</span><span class="sxs-lookup"><span data-stu-id="3b06b-145">Note, that in addition to the EntityFramework  assembly, a reference to the System.ComponentModel.DataAnnotations assembly is also added.</span></span>

<span data-ttu-id="3b06b-146">V horní části souboru Program.cs přidejte následující příkaz using:</span><span class="sxs-lookup"><span data-stu-id="3b06b-146">At the top of the Program.cs file, add the following using statement:</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="3b06b-147">Do Program.cs přidejte definici kontextu.</span><span class="sxs-lookup"><span data-stu-id="3b06b-147">In the Program.cs add the context definition.</span></span> 

``` csharp
public partial class UniversityContext : DbContext
{
    public DbSet<University> Universities { get; set; }
}
```

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="3b06b-148">Zachovat a načíst data</span><span class="sxs-lookup"><span data-stu-id="3b06b-148">Persist and Retrieve Data</span></span>

<span data-ttu-id="3b06b-149">Otevřete soubor Program.cs, kde je definována metoda Main.</span><span class="sxs-lookup"><span data-stu-id="3b06b-149">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="3b06b-150">Do funkce Main přidejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="3b06b-150">Add the following code into the Main function.</span></span>

<span data-ttu-id="3b06b-151">Kód přidá do kontextu dva nové objekty vysoké školy.</span><span class="sxs-lookup"><span data-stu-id="3b06b-151">The code adds two new University objects to the context.</span></span> <span data-ttu-id="3b06b-152">Prostorové vlastnosti jsou inicializovány pomocí metody DbGeography. FromText.</span><span class="sxs-lookup"><span data-stu-id="3b06b-152">Spatial properties are initialized by using the DbGeography.FromText method.</span></span> <span data-ttu-id="3b06b-153">Zeměpisný bod reprezentovaný jako WellKnownText je předán metodě.</span><span class="sxs-lookup"><span data-stu-id="3b06b-153">The geography point represented as WellKnownText is passed to the method.</span></span> <span data-ttu-id="3b06b-154">Kód pak data uloží.</span><span class="sxs-lookup"><span data-stu-id="3b06b-154">The code then saves the data.</span></span> <span data-ttu-id="3b06b-155">Pak dotaz LINQ, který vrací objekt univerzity, kde je jeho umístění nejblíže zadanému umístění, je vytvořen a proveden.</span><span class="sxs-lookup"><span data-stu-id="3b06b-155">Then, the LINQ query that that returns a University object where its location is closest to the specified location, is constructed and executed.</span></span>

``` csharp
using (var context = new UniversityContext ())
{
    context.Universities.Add(new University()
        {
            Name = "Graphic Design Institute",
            Location = DbGeography.FromText("POINT(-122.336106 47.605049)"),
        });

    context. Universities.Add(new University()
        {
            Name = "School of Fine Art",
            Location = DbGeography.FromText("POINT(-122.335197 47.646711)"),
        });

    context.SaveChanges();

    var myLocation = DbGeography.FromText("POINT(-122.296623 47.640405)");

    var university = (from u in context.Universities
                        orderby u.Location.Distance(myLocation)
                        select u).FirstOrDefault();

    Console.WriteLine(
        "The closest University to you is: {0}.",
        university.Name);
}
```

<span data-ttu-id="3b06b-156">Zkompilujte a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3b06b-156">Compile and run the application.</span></span> <span data-ttu-id="3b06b-157">Program vytvoří následující výstup:</span><span class="sxs-lookup"><span data-stu-id="3b06b-157">The program produces the following output:</span></span>

```console
The closest University to you is: School of Fine Art.
```

## <a name="view-the-generated-database"></a><span data-ttu-id="3b06b-158">Zobrazení vygenerované databáze</span><span class="sxs-lookup"><span data-stu-id="3b06b-158">View the Generated Database</span></span>

<span data-ttu-id="3b06b-159">Při prvním spuštění aplikace Entity Framework vytvoří pro vás databázi.</span><span class="sxs-lookup"><span data-stu-id="3b06b-159">When you run the application the first time, the Entity Framework creates a database for you.</span></span> <span data-ttu-id="3b06b-160">Vzhledem k tomu, že jsme nainstalovali Visual Studio 2012, databáze se vytvoří v instanci LocalDB.</span><span class="sxs-lookup"><span data-stu-id="3b06b-160">Because we have Visual Studio 2012 installed, the database will be created on the LocalDB instance.</span></span> <span data-ttu-id="3b06b-161">Ve výchozím nastavení Entity Framework pojmenuje databázi za plně kvalifikovaným názvem odvozeného kontextu (v tomto příkladu je to **SpatialCodeFirst. UniversityContext**).</span><span class="sxs-lookup"><span data-stu-id="3b06b-161">By default, the Entity Framework names the database after the fully qualified name of the derived context (in this example that is **SpatialCodeFirst.UniversityContext**).</span></span> <span data-ttu-id="3b06b-162">Následující časy budou použity při dalším použití existující databáze.</span><span class="sxs-lookup"><span data-stu-id="3b06b-162">The subsequent times the existing database will be used.</span></span>  

<span data-ttu-id="3b06b-163">Všimněte si, že pokud provedete změny modelu po vytvoření databáze, měli byste použít Migrace Code First k aktualizaci schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="3b06b-163">Note, that if you make any changes to your model after the database has been created, you should use Code First Migrations to update the database schema.</span></span> <span data-ttu-id="3b06b-164">Příklad použití migrace naleznete v tématu [Code First do nové databáze](~/ef6/modeling/code-first/workflows/new-database.md) .</span><span class="sxs-lookup"><span data-stu-id="3b06b-164">See [Code First to a New Database](~/ef6/modeling/code-first/workflows/new-database.md) for an example of using Migrations.</span></span>

<span data-ttu-id="3b06b-165">Chcete-li zobrazit databázi a data, postupujte následovně:</span><span class="sxs-lookup"><span data-stu-id="3b06b-165">To view the database and data, do the following:</span></span>

1.  <span data-ttu-id="3b06b-166">V hlavní nabídce sady Visual Studio 2012 vyberte **zobrazení** - @ no__t-2 **Průzkumník objektů systému SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="3b06b-166">In the Visual Studio 2012 main menu, select **View** -&gt; **SQL Server Object Explorer**.</span></span>
2.  <span data-ttu-id="3b06b-167">Pokud LocalDB není v seznamu serverů, klikněte pravým tlačítkem myši na **SQL Server** a vyberte **Přidat SQL Server** pro připojení k instanci LocalDB použijte výchozí **ověřování systému Windows** .</span><span class="sxs-lookup"><span data-stu-id="3b06b-167">If LocalDB is not in the list of servers, click the right mouse button on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the the LocalDB instance</span></span>
3.  <span data-ttu-id="3b06b-168">Rozbalte uzel LocalDB</span><span class="sxs-lookup"><span data-stu-id="3b06b-168">Expand the LocalDB node</span></span>
4.  <span data-ttu-id="3b06b-169">Rozložte složku **databáze** , aby se zobrazila nová databáze, a přejděte do tabulky **univerzity** .</span><span class="sxs-lookup"><span data-stu-id="3b06b-169">Unfold the **Databases** folder to see the new database and browse to the **Universities** table</span></span>
5.  <span data-ttu-id="3b06b-170">Chcete-li zobrazit data, klikněte pravým tlačítkem myši na tabulku a vyberte možnost **Zobrazit data.**</span><span class="sxs-lookup"><span data-stu-id="3b06b-170">To view data, right-click on the table and select **View Data**</span></span>

## <a name="summary"></a><span data-ttu-id="3b06b-171">Souhrn</span><span class="sxs-lookup"><span data-stu-id="3b06b-171">Summary</span></span>

<span data-ttu-id="3b06b-172">V tomto návodu jsme se podívali na to, jak používat prostorové typy s Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="3b06b-172">In this walkthrough we looked at how to use spatial types with Entity Framework Code First.</span></span> 
