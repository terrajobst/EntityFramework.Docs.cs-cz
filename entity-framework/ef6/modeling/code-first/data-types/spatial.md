---
title: Prostorový - Code First – EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: d617aed1-15f2-48a9-b187-186991c666e3
caps.latest.revision: 3
ms.openlocfilehash: 03557820bc689d8e7389a1f84121b7eeaa904df1
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/09/2018
ms.locfileid: "37914260"
---
# <a name="spatial---code-first"></a><span data-ttu-id="31473-102">Prostorový - Code First</span><span class="sxs-lookup"><span data-stu-id="31473-102">Spatial - Code First</span></span>
> [!NOTE]
> <span data-ttu-id="31473-103">**EF5 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="31473-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="31473-104">Pokud používáte starší verzi, některé nebo všechny informace neplatí.</span><span class="sxs-lookup"><span data-stu-id="31473-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="31473-105">Videa a podrobný návod ukazuje, jak mapovat prostorové typy s Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="31473-105">The video and step-by-step walkthrough shows how to map spatial types with Entity Framework Code First.</span></span> <span data-ttu-id="31473-106">Také ukazuje, jak dotaz LINQ slouží k vyhledání vzdálenost mezi dvěma umístěními.</span><span class="sxs-lookup"><span data-stu-id="31473-106">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="31473-107">Tento názorný postup použije Code First k vytvoření nové databáze, ale můžete také použít [Code First pro existující databázi](~/ef6/modeling/code-first/workflows/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="31473-107">This walkthrough will use Code First to create a new database, but you can also use [Code First to an existing database](~/ef6/modeling/code-first/workflows/existing-database.md).</span></span>

<span data-ttu-id="31473-108">Podpora prostorových typů byla zavedena v Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="31473-108">Spatial type support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="31473-109">Všimněte si, že pokud chcete používat nové funkce jako prostorových typů výčtů a funkce vracející tabulku, je potřeba cílit rozhraní .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="31473-109">Note that to use the new features like spatial type, enums, and Table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="31473-110">Visual Studio 2012 cílí na rozhraní .NET 4.5 ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="31473-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="31473-111">Používat typy prostorových dat je nutné také použít poskytovatele Entity Framework, který zahrnuje prostorová podporu.</span><span class="sxs-lookup"><span data-stu-id="31473-111">To use spatial data types you must also use an Entity Framework provider that has spatial support.</span></span> <span data-ttu-id="31473-112">Zobrazit [Podpora zprostředkovatele pro typy prostorových](~/ef6/fundamentals/providers/spatial-support.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="31473-112">See [provider support for spatial types](~/ef6/fundamentals/providers/spatial-support.md) for more information.</span></span>

<span data-ttu-id="31473-113">Existují dva typy hlavní prostorových dat: zeměpisné oblasti a geometry.</span><span class="sxs-lookup"><span data-stu-id="31473-113">There are two main spatial data types: geography and geometry.</span></span> <span data-ttu-id="31473-114">Datový typ geography ukládá elipsoidním data (například GPS zeměpisné šířky a délky koordinuje).</span><span class="sxs-lookup"><span data-stu-id="31473-114">The geography data type stores ellipsoidal data (for example, GPS latitude and longitude coordinates).</span></span> <span data-ttu-id="31473-115">Datový typ geometry představuje Euclidean souřadnicovém systému (bez stromové struktury).</span><span class="sxs-lookup"><span data-stu-id="31473-115">The geometry data type represents Euclidean (flat) coordinate system.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="31473-116">Podívejte se na video</span><span class="sxs-lookup"><span data-stu-id="31473-116">Watch the video</span></span>
<span data-ttu-id="31473-117">Toto video ukazuje, jak mapovat prostorové typy s Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="31473-117">This video shows how to map spatial types with Entity Framework Code First.</span></span> <span data-ttu-id="31473-118">Také ukazuje, jak dotaz LINQ slouží k vyhledání vzdálenost mezi dvěma umístěními.</span><span class="sxs-lookup"><span data-stu-id="31473-118">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="31473-119">**Přednášející:**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="31473-119">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="31473-120">**Video**: [WMV](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="31473-120">**Video**: [WMV](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="31473-121">Předpoklady</span><span class="sxs-lookup"><span data-stu-id="31473-121">Pre-Requisites</span></span>

<span data-ttu-id="31473-122">Musíte mít Visual Studio 2012, Ultimate, Premium, Professional nebo Web Express edition nainstalované k dokončení tohoto návodu.</span><span class="sxs-lookup"><span data-stu-id="31473-122">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="31473-123">Nastavení projektu</span><span class="sxs-lookup"><span data-stu-id="31473-123">Set up the Project</span></span>

1.  <span data-ttu-id="31473-124">Otevřít Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="31473-124">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="31473-125">Na **souboru** nabídky, přejděte k **nový**a potom klikněte na tlačítko **projektu**</span><span class="sxs-lookup"><span data-stu-id="31473-125">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="31473-126">V levém podokně klikněte na tlačítko **Visual C\#** a pak vyberte **konzoly** šablony</span><span class="sxs-lookup"><span data-stu-id="31473-126">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="31473-127">Zadejte **SpatialCodeFirst** jako název projektu a klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="31473-127">Enter **SpatialCodeFirst** as the name of the project and click **OK**</span></span>

## <a name="define-a-new-model-using-code-first"></a><span data-ttu-id="31473-128">Definovat nový Model využívající Code First</span><span class="sxs-lookup"><span data-stu-id="31473-128">Define a New Model using Code First</span></span>

<span data-ttu-id="31473-129">Při použití vývoje Code First obvykle začnete vytvořením tříd rozhraní .NET Framework, které definují model koncepční (domény).</span><span class="sxs-lookup"><span data-stu-id="31473-129">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="31473-130">Následující kód definuje třídu University.</span><span class="sxs-lookup"><span data-stu-id="31473-130">The code below defines the University class.</span></span>

<span data-ttu-id="31473-131">Univerzity má vlastnost umístění DbGeography typu.</span><span class="sxs-lookup"><span data-stu-id="31473-131">The University has the Location property of the DbGeography type.</span></span> <span data-ttu-id="31473-132">Pokud chcete použít typ DbGeography, musíte přidat odkaz na sestavení System.Data.Entity a také přidat System.Data.Spatial příkaz using.</span><span class="sxs-lookup"><span data-stu-id="31473-132">To use the DbGeography type, you must add a reference to the System.Data.Entity assembly and also add the System.Data.Spatial using statement.</span></span>

<span data-ttu-id="31473-133">Otevřete soubor Program.cs a vložte následující příkazy using v horní části souboru:</span><span class="sxs-lookup"><span data-stu-id="31473-133">Open the Program.cs file and paste the following using statements at the top of the file:</span></span>

``` csharp
using System.Data.Spatial;
```

<span data-ttu-id="31473-134">Do souboru Program.cs přidejte následující definice třídy University.</span><span class="sxs-lookup"><span data-stu-id="31473-134">Add the following University class definition to the Program.cs file.</span></span>

``` csharp
public class University  
{
    public int UniversityID { get; set; }
    public string Name { get; set; }
    public DbGeography Location { get; set; }
}
```

## <a name="define-the-dbcontext-derived-type"></a><span data-ttu-id="31473-135">Definování uvolněn objekt DbContext odvozený typ</span><span class="sxs-lookup"><span data-stu-id="31473-135">Define the DbContext Derived Type</span></span>

<span data-ttu-id="31473-136">Kromě definování entit, budete muset definovat třídu, která je odvozena od položky DbContext a zveřejňuje DbSet&lt;TEntity&gt; vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="31473-136">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="31473-137">DbSet&lt;TEntity&gt; vlastnosti umožňují kontextu vědět, jaké typy, které chcete zahrnout do modelu.</span><span class="sxs-lookup"><span data-stu-id="31473-137">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="31473-138">Instance typu DbContext odvozené spravuje objekty entity za běhu, který obsahuje naplnění objekty s daty z databáze, změňte sledování a zachovává data do databáze.</span><span class="sxs-lookup"><span data-stu-id="31473-138">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

<span data-ttu-id="31473-139">Kontext databáze a DbSet typy jsou definovány v sestavení objektu EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="31473-139">The DbContext and DbSet types are defined in the EntityFramework assembly.</span></span> <span data-ttu-id="31473-140">Přidáme odkaz na tuto knihovnu DLL pomocí balíčku NuGet objektu EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="31473-140">We will add a reference to this DLL by using the EntityFramework NuGet package.</span></span>

1.  <span data-ttu-id="31473-141">V Průzkumníku řešení klikněte pravým tlačítkem na název projektu.</span><span class="sxs-lookup"><span data-stu-id="31473-141">In Solution Explorer, right-click on the project name.</span></span>
2.  <span data-ttu-id="31473-142">Vyberte **spravovat balíčky NuGet...**</span><span class="sxs-lookup"><span data-stu-id="31473-142">Select **Manage NuGet Packages…**</span></span>
3.  <span data-ttu-id="31473-143">V dialogovém okně Spravovat balíčky NuGet vyberte **Online** kartě a zvolte **EntityFramework** balíčku.</span><span class="sxs-lookup"><span data-stu-id="31473-143">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package.</span></span>
4.  <span data-ttu-id="31473-144">Klikněte na tlačítko **instalace**</span><span class="sxs-lookup"><span data-stu-id="31473-144">Click **Install**</span></span>

<span data-ttu-id="31473-145">Všimněte si, že kromě sestavení objektu EntityFramework, také se přidá odkaz na sestavení System.ComponentModel.DataAnnotations.</span><span class="sxs-lookup"><span data-stu-id="31473-145">Note, that in addition to the EntityFramework  assembly, a reference to the System.ComponentModel.DataAnnotations assembly is also added.</span></span>

<span data-ttu-id="31473-146">V horní části souboru Program.cs přidejte následující příkaz using:</span><span class="sxs-lookup"><span data-stu-id="31473-146">At the top of the Program.cs file, add the following using statement:</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="31473-147">V souboru Program.cs přidejte definici kontextu.</span><span class="sxs-lookup"><span data-stu-id="31473-147">In the Program.cs add the context definition.</span></span> 

``` csharp
public partial class UniversityContext : DbContext
{
    public DbSet<University> Universities { get; set; }
}
```

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="31473-148">Zachovat a načtení dat</span><span class="sxs-lookup"><span data-stu-id="31473-148">Persist and Retrieve Data</span></span>

<span data-ttu-id="31473-149">Otevřete soubor Program.cs, ve kterém je definována metoda Main.</span><span class="sxs-lookup"><span data-stu-id="31473-149">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="31473-150">Přidejte následující kód do funkce Main.</span><span class="sxs-lookup"><span data-stu-id="31473-150">Add the following code into the Main function.</span></span>

<span data-ttu-id="31473-151">Kód přidá dva nové objekty University do kontextu.</span><span class="sxs-lookup"><span data-stu-id="31473-151">The code adds two new University objects to the context.</span></span> <span data-ttu-id="31473-152">Prostorové vlastnosti jsou inicializovány pomocí metody DbGeography.FromText.</span><span class="sxs-lookup"><span data-stu-id="31473-152">Spatial properties are initialized by using the DbGeography.FromText method.</span></span> <span data-ttu-id="31473-153">Zeměpisné oblasti bod zastoupeny EDT WellKnownText je předán metodě.</span><span class="sxs-lookup"><span data-stu-id="31473-153">The geography point represented as WellKnownText is passed to the method.</span></span> <span data-ttu-id="31473-154">Kód poté uloží data.</span><span class="sxs-lookup"><span data-stu-id="31473-154">The code then saves the data.</span></span> <span data-ttu-id="31473-155">Potom dotaz LINQ, který, který vrací objekt University, kde je nejbližší do zadaného umístění, jeho umístění je vytvořen a spustit.</span><span class="sxs-lookup"><span data-stu-id="31473-155">Then, the LINQ query that that returns a University object where its location is closest to the specified location, is constructed and executed.</span></span>

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

<span data-ttu-id="31473-156">Kompilace a spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="31473-156">Compile and run the application.</span></span> <span data-ttu-id="31473-157">Program vygeneruje následující výstup:</span><span class="sxs-lookup"><span data-stu-id="31473-157">The program produces the following output:</span></span>

```
The closest University to you is: School of Fine Art.
```

## <a name="view-the-generated-database"></a><span data-ttu-id="31473-158">Zobrazí se vygenerovaný databáze</span><span class="sxs-lookup"><span data-stu-id="31473-158">View the Generated Database</span></span>

<span data-ttu-id="31473-159">Když spustíte aplikaci poprvé, Entity Framework vytvoří databázi za vás.</span><span class="sxs-lookup"><span data-stu-id="31473-159">When you run the application the first time, the Entity Framework creates a database for you.</span></span> <span data-ttu-id="31473-160">Když máme nainstalované Visual Studio 2012, vytvoří se databáze na instanci LocalDB.</span><span class="sxs-lookup"><span data-stu-id="31473-160">Because we have Visual Studio 2012 installed, the database will be created on the LocalDB instance.</span></span> <span data-ttu-id="31473-161">Ve výchozím nastavení, Entity Framework pojmenuje databázi za plně kvalifikovaný název odvozené kontextu (v tomto příkladu, který je **SpatialCodeFirst.UniversityContext**).</span><span class="sxs-lookup"><span data-stu-id="31473-161">By default, the Entity Framework names the database after the fully qualified name of the derived context (in this example that is **SpatialCodeFirst.UniversityContext**).</span></span> <span data-ttu-id="31473-162">Následující časy, které se použije existující databázi.</span><span class="sxs-lookup"><span data-stu-id="31473-162">The subsequent times the existing database will be used.</span></span>  

<span data-ttu-id="31473-163">Všimněte si, že pokud provedete změny modelu po vytvoření databáze, by měl použít migrace Code First aktualizovat schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="31473-163">Note, that if you make any changes to your model after the database has been created, you should use Code First Migrations to update the database schema.</span></span> <span data-ttu-id="31473-164">Zobrazit [Code First pro novou databázi](~/ef6/modeling/code-first/workflows/new-database.md) pro příklad použití migrace.</span><span class="sxs-lookup"><span data-stu-id="31473-164">See [Code First to a New Database](~/ef6/modeling/code-first/workflows/new-database.md) for an example of using Migrations.</span></span>

<span data-ttu-id="31473-165">Chcete-li zobrazit databáze a dat, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="31473-165">To view the database and data, do the following:</span></span>

1.  <span data-ttu-id="31473-166">V hlavní nabídce sady Visual Studio 2012 vyberte **zobrazení**  - &gt; **Průzkumník objektů systému SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="31473-166">In the Visual Studio 2012 main menu, select **View** -&gt; **SQL Server Object Explorer**.</span></span>
2.  <span data-ttu-id="31473-167">Pokud LocalDB se nenachází v seznamu serverů, klikněte pravým tlačítkem myši na **systému SQL Server** a vyberte **přidat SQL Server** použijte výchozí **ověřování Windows** pro připojení k instanci LocalDB</span><span class="sxs-lookup"><span data-stu-id="31473-167">If LocalDB is not in the list of servers, click the right mouse button on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the the LocalDB instance</span></span>
3.  <span data-ttu-id="31473-168">Rozbalte uzel LocalDB</span><span class="sxs-lookup"><span data-stu-id="31473-168">Expand the LocalDB node</span></span>
4.  <span data-ttu-id="31473-169">Rozbalit **databází** složky a zobrazí novou databázi procházením **univerzity** tabulky</span><span class="sxs-lookup"><span data-stu-id="31473-169">Unfold the **Databases** folder to see the new database and browse to the **Universities** table</span></span>
5.  <span data-ttu-id="31473-170">K zobrazení dat, klikněte pravým tlačítkem na tabulku a vyberte **zobrazení dat**</span><span class="sxs-lookup"><span data-stu-id="31473-170">To view data, right-click on the table and select **View Data**</span></span>

## <a name="summary"></a><span data-ttu-id="31473-171">Souhrn</span><span class="sxs-lookup"><span data-stu-id="31473-171">Summary</span></span>

<span data-ttu-id="31473-172">V tomto názorném postupu jsme se podívali na tom, jak používat prostorové typy s Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="31473-172">In this walkthrough we looked at how to use spatial types with Entity Framework Code First.</span></span> 
