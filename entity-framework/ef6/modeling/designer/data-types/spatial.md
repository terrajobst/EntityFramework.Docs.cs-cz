---
title: Prostorový – EF designeru - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 06baa6e1-d680-4a95-845b-81305c87a962
caps.latest.revision: 3
ms.openlocfilehash: 2332818275763fd90274f426b7fced4c3c14ac17
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/09/2018
ms.locfileid: "37914130"
---
# <a name="spatial---ef-designer"></a><span data-ttu-id="a67d0-102">Prostorový – EF designeru</span><span class="sxs-lookup"><span data-stu-id="a67d0-102">Spatial - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="a67d0-103">**EF5 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="a67d0-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="a67d0-104">Pokud používáte starší verzi, některé nebo všechny informace neplatí.</span><span class="sxs-lookup"><span data-stu-id="a67d0-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="a67d0-105">Videa a podrobný návod ukazuje, jak mapovat prostorové typy s Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="a67d0-105">The video and step-by-step walkthrough shows how to map spatial types with the Entity Framework Designer.</span></span> <span data-ttu-id="a67d0-106">Také ukazuje, jak dotaz LINQ slouží k vyhledání vzdálenost mezi dvěma umístěními.</span><span class="sxs-lookup"><span data-stu-id="a67d0-106">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="a67d0-107">Tento názorný postup použije první Model k vytvoření nové databáze, ale EF designeru je také možné se [Database First](~/ef6/modeling/designer/workflows/database-first.md) pracovního postupu pro mapování na existující databázi.</span><span class="sxs-lookup"><span data-stu-id="a67d0-107">This walkthrough will use Model First to create a new database, but the EF Designer can also be used with the [Database First](~/ef6/modeling/designer/workflows/database-first.md) workflow to map to an existing database.</span></span>

<span data-ttu-id="a67d0-108">Podpora prostorových typů byla zavedena v Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="a67d0-108">Spatial type support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="a67d0-109">Všimněte si, že pokud chcete používat nové funkce jako prostorových typů výčtů a funkce vracející tabulku, je potřeba cílit rozhraní .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="a67d0-109">Note that to use the new features like spatial type, enums, and Table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="a67d0-110">Visual Studio 2012 cílí na rozhraní .NET 4.5 ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="a67d0-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="a67d0-111">Používat typy prostorových dat je nutné také použít poskytovatele Entity Framework, který zahrnuje prostorová podporu.</span><span class="sxs-lookup"><span data-stu-id="a67d0-111">To use spatial data types you must also use an Entity Framework provider that has spatial support.</span></span> <span data-ttu-id="a67d0-112">Zobrazit [Podpora zprostředkovatele pro typy prostorových](~/ef6/fundamentals/providers/spatial-support.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="a67d0-112">See [provider support for spatial types](~/ef6/fundamentals/providers/spatial-support.md) for more information.</span></span>

<span data-ttu-id="a67d0-113">Existují dva typy hlavní prostorových dat: zeměpisné oblasti a geometry.</span><span class="sxs-lookup"><span data-stu-id="a67d0-113">There are two main spatial data types: geography and geometry.</span></span> <span data-ttu-id="a67d0-114">Datový typ geography ukládá elipsoidním data (například GPS zeměpisné šířky a délky koordinuje).</span><span class="sxs-lookup"><span data-stu-id="a67d0-114">The geography data type stores ellipsoidal data (for example, GPS latitude and longitude coordinates).</span></span> <span data-ttu-id="a67d0-115">Datový typ geometry představuje Euclidean souřadnicovém systému (bez stromové struktury).</span><span class="sxs-lookup"><span data-stu-id="a67d0-115">The geometry data type represents Euclidean (flat) coordinate system.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="a67d0-116">Podívejte se na video</span><span class="sxs-lookup"><span data-stu-id="a67d0-116">Watch the video</span></span>
<span data-ttu-id="a67d0-117">Toto video ukazuje, jak mapovat prostorové typy s Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="a67d0-117">This video shows how to map spatial types with the Entity Framework Designer.</span></span> <span data-ttu-id="a67d0-118">Také ukazuje, jak dotaz LINQ slouží k vyhledání vzdálenost mezi dvěma umístěními.</span><span class="sxs-lookup"><span data-stu-id="a67d0-118">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="a67d0-119">**Přednášející:**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="a67d0-119">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="a67d0-120">**Video**: [WMV](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)</span><span class="sxs-lookup"><span data-stu-id="a67d0-120">**Video**: [WMV](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="a67d0-121">Předpoklady</span><span class="sxs-lookup"><span data-stu-id="a67d0-121">Pre-Requisites</span></span>

<span data-ttu-id="a67d0-122">Musíte mít Visual Studio 2012, Ultimate, Premium, Professional nebo Web Express edition nainstalované k dokončení tohoto návodu.</span><span class="sxs-lookup"><span data-stu-id="a67d0-122">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="a67d0-123">Nastavení projektu</span><span class="sxs-lookup"><span data-stu-id="a67d0-123">Set up the Project</span></span>

1.  <span data-ttu-id="a67d0-124">Otevřít Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="a67d0-124">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="a67d0-125">Na **souboru** nabídky, přejděte k **nový**a potom klikněte na tlačítko **projektu**</span><span class="sxs-lookup"><span data-stu-id="a67d0-125">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="a67d0-126">V levém podokně klikněte na tlačítko **Visual C\#** a pak vyberte **konzoly** šablony</span><span class="sxs-lookup"><span data-stu-id="a67d0-126">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="a67d0-127">Zadejte **SpatialEFDesigner** jako název projektu a klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="a67d0-127">Enter **SpatialEFDesigner** as the name of the project and click **OK**</span></span>

## <a name="create-a-new-model-using-the-ef-designer"></a><span data-ttu-id="a67d0-128">Vytvořit nový Model využívající EF designeru</span><span class="sxs-lookup"><span data-stu-id="a67d0-128">Create a New Model using the EF Designer</span></span>

1.  <span data-ttu-id="a67d0-129">Klikněte pravým tlačítkem na název projektu v Průzkumníku řešení, přejděte na **přidat**a potom klikněte na tlačítko **nová položka**</span><span class="sxs-lookup"><span data-stu-id="a67d0-129">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="a67d0-130">Vyberte **Data** v levé nabídce a pak vyberte **datový Model Entity ADO.NET** v podokně šablon</span><span class="sxs-lookup"><span data-stu-id="a67d0-130">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane</span></span>
3.  <span data-ttu-id="a67d0-131">Zadejte **UniversityModel.edmx** pro název souboru a pak klikněte na tlačítko **přidat**</span><span class="sxs-lookup"><span data-stu-id="a67d0-131">Enter **UniversityModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="a67d0-132">Na stránce Průvodce datovým modelem Entity vyberte **prázdný Model** v dialogovém okně Výběr obsahu modelu</span><span class="sxs-lookup"><span data-stu-id="a67d0-132">On the Entity Data Model Wizard page, select **Empty Model** in the Choose Model Contents dialog box</span></span>
5.  <span data-ttu-id="a67d0-133">Klikněte na tlačítko **dokončit**</span><span class="sxs-lookup"><span data-stu-id="a67d0-133">Click **Finish**</span></span>

<span data-ttu-id="a67d0-134">V návrháři entit, které poskytuje návrhové ploše pro úpravy váš model, se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="a67d0-134">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

<span data-ttu-id="a67d0-135">Průvodce provede následující akce:</span><span class="sxs-lookup"><span data-stu-id="a67d0-135">The wizard performs the following actions:</span></span>

-   <span data-ttu-id="a67d0-136">Generuje EnumTestModel.edmx soubor, který definuje mapování mezi nimi, konceptuálního modelu a model úložiště.</span><span class="sxs-lookup"><span data-stu-id="a67d0-136">Generates the EnumTestModel.edmx file that defines the conceptual model, the storage model, and the mapping between them.</span></span> <span data-ttu-id="a67d0-137">Nastaví vlastnost Metadata artefaktů zpracování souboru vložit do výstupního sestavení tak získat soubory generované metadat vložen do sestavení.</span><span class="sxs-lookup"><span data-stu-id="a67d0-137">Sets the Metadata Artifact Processing property of the .edmx file to Embed in Output Assembly so the generated metadata files get embedded into the assembly.</span></span>
-   <span data-ttu-id="a67d0-138">Přidá odkaz na následující sestavení: objektu EntityFramework, System.ComponentModel.DataAnnotations a System.Data.Entity.</span><span class="sxs-lookup"><span data-stu-id="a67d0-138">Adds a reference to the following assemblies: EntityFramework, System.ComponentModel.DataAnnotations, and System.Data.Entity.</span></span>
-   <span data-ttu-id="a67d0-139">Vytvoří soubory UniversityModel.tt a UniversityModel.Context.tt a přidá je v souboru .edmx.</span><span class="sxs-lookup"><span data-stu-id="a67d0-139">Creates UniversityModel.tt and UniversityModel.Context.tt files and adds them under the .edmx file.</span></span> <span data-ttu-id="a67d0-140">Tyto soubory šablon T4 generovat kód, který definuje DbContext odvozený typ a POCO typy, které mapují na entity v modelu edmx</span><span class="sxs-lookup"><span data-stu-id="a67d0-140">These T4 template files generate the code that defines the DbContext derived type and POCO types that map to the entities in the .edmx model</span></span>

## <a name="add-a-new-entity-type"></a><span data-ttu-id="a67d0-141">Přidat nový typ Entity</span><span class="sxs-lookup"><span data-stu-id="a67d0-141">Add a New Entity Type</span></span>

1.  <span data-ttu-id="a67d0-142">Klikněte pravým tlačítkem na prázdnou oblast návrhové plochy, vyberte **Add -&gt; Entity**, zobrazí se dialogové okno Nová entita</span><span class="sxs-lookup"><span data-stu-id="a67d0-142">Right-click an empty area of the design surface, select **Add -&gt; Entity**, the New Entity dialog box appears</span></span>
2.  <span data-ttu-id="a67d0-143">Zadejte **University** pro typ název a určete **UniversityID** název klíče vlastnosti ponechat typ jako **Int32**</span><span class="sxs-lookup"><span data-stu-id="a67d0-143">Specify **University** for the type name and specify **UniversityID** for the key property name, leave the type as **Int32**</span></span>
3.  <span data-ttu-id="a67d0-144">Klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="a67d0-144">Click **OK**</span></span>
4.  <span data-ttu-id="a67d0-145">Klikněte pravým tlačítkem na entitu a vyberte **přidat nový -&gt; skalární vlastnost**</span><span class="sxs-lookup"><span data-stu-id="a67d0-145">Right-click the entity and select **Add New -&gt; Scalar Property**</span></span>
5.  <span data-ttu-id="a67d0-146">Přejmenujte novou vlastnost **název**</span><span class="sxs-lookup"><span data-stu-id="a67d0-146">Rename the new property to **Name**</span></span>
6.  <span data-ttu-id="a67d0-147">Přidat jiný skalární vlastnost a přejmenujte ho na **umístění** otevřete okno Vlastnosti a změna typu novou vlastnost **zeměpisné oblasti**</span><span class="sxs-lookup"><span data-stu-id="a67d0-147">Add another scalar property and rename it to **Location** Open the Properties window and change the type of the new property to **Geography**</span></span>
7.  <span data-ttu-id="a67d0-148">Uložit model a sestavte projekt</span><span class="sxs-lookup"><span data-stu-id="a67d0-148">Save the model and build the project</span></span>
    > [!NOTE]
    > <span data-ttu-id="a67d0-149">Při sestavování se může zobrazit upozornění na nenamapované entit a přidružení v seznamu chyb.</span><span class="sxs-lookup"><span data-stu-id="a67d0-149">When you build, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="a67d0-150">Tato upozornění můžete ignorovat, protože po zvolíme, aby se vygenerovala databáze z modelu, chyby zmizí.</span><span class="sxs-lookup"><span data-stu-id="a67d0-150">You can ignore these warnings because after we choose to generate the database from the model, the errors will go away.</span></span>

## <a name="generate-database-from-model"></a><span data-ttu-id="a67d0-151">Generování databáze z modelu</span><span class="sxs-lookup"><span data-stu-id="a67d0-151">Generate Database from Model</span></span>

<span data-ttu-id="a67d0-152">Nyní jsme můžete vygenerovat databázi, která je založená na modelu.</span><span class="sxs-lookup"><span data-stu-id="a67d0-152">Now we can generate a database that is based on the model.</span></span>

1.  <span data-ttu-id="a67d0-153">Klikněte pravým tlačítkem na prázdné místo na ploše a vyberte v návrháři entit **generovat databáze z modelu**</span><span class="sxs-lookup"><span data-stu-id="a67d0-153">Right-click an empty space on the Entity Designer surface and select **Generate Database from Model**</span></span>
2.  <span data-ttu-id="a67d0-154">Vyberte vaše Data Connection Dialog Box průvodce generovat databáze je zobrazit kliknutím **nové připojení** tlačítko zadejte **(localdb)\\mssqllocaldb** pro název serveru a  **University** databáze a klikněte na **OK**</span><span class="sxs-lookup"><span data-stu-id="a67d0-154">The Choose Your Data Connection Dialog Box of the Generate Database Wizard is displayed Click the **New Connection** button Specify **(localdb)\\mssqllocaldb** for the server name and **University** for the database and click **OK**</span></span>
3.  <span data-ttu-id="a67d0-155">Dialogové okno s dotazem, zda chcete vytvořit novou databázi se automaticky otevře, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="a67d0-155">A dialog asking if you want to create a new database will pop up, click **Yes**.</span></span>
4.  <span data-ttu-id="a67d0-156">Klikněte na tlačítko **Další** a Průvodce vytvořením databáze generuje jazyk pro definování dat (DDL) pro vytvoření databáze vygenerovaný DDL se zobrazí v souhrnu a nastavení dialogové okno pole Poznámka, která jazyka DDL neobsahuje definici pro Tabulka, která se mapuje na typ výčtu</span><span class="sxs-lookup"><span data-stu-id="a67d0-156">Click **Next** and the Create Database Wizard generates data definition language (DDL) for creating a database The generated DDL is displayed in the Summary and Settings Dialog Box Note, that the DDL does not contain a definition for a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="a67d0-157">Klikněte na tlačítko **Dokončit** kliknutím na Dokončit nespustí skriptu jazyka DDL.</span><span class="sxs-lookup"><span data-stu-id="a67d0-157">Click **Finish** Clicking Finish does not execute the DDL script.</span></span>
6.  <span data-ttu-id="a67d0-158">Průvodce vytvořením databáze provede následující: otevře **UniversityModel.edmx.sql** v editoru jazyka T-SQL generuje úložiště schématu a mapování oddílů EDMX soubor přidá připojovacího řetězce do souboru App.config</span><span class="sxs-lookup"><span data-stu-id="a67d0-158">The Create Database Wizard does the following: Opens the **UniversityModel.edmx.sql** in T-SQL Editor Generates the store schema and mapping sections of the EDMX file Adds connection string information to the App.config file</span></span>
7.  <span data-ttu-id="a67d0-159">Klikněte pravým tlačítkem myši v editoru jazyka T-SQL a vyberte **Execute** připojení k serveru dialogové okno zobrazí, zadejte informace o připojení z kroku 2 a klikněte na tlačítko **Connect**</span><span class="sxs-lookup"><span data-stu-id="a67d0-159">Click the right mouse button in T-SQL Editor and select **Execute** The Connect to Server dialog appears, enter the connection information from step 2 and click **Connect**</span></span>
8.  <span data-ttu-id="a67d0-160">Chcete-li zobrazit generované schéma, klikněte pravým tlačítkem na název databáze v Průzkumníku objektů systému SQL Server a vyberte **aktualizovat**</span><span class="sxs-lookup"><span data-stu-id="a67d0-160">To view the generated schema, right-click on the database name in SQL Server Object Explorer and select **Refresh**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="a67d0-161">Zachovat a načtení dat</span><span class="sxs-lookup"><span data-stu-id="a67d0-161">Persist and Retrieve Data</span></span>

<span data-ttu-id="a67d0-162">Otevřete soubor Program.cs, ve kterém je definována metoda Main.</span><span class="sxs-lookup"><span data-stu-id="a67d0-162">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="a67d0-163">Přidejte následující kód do funkce Main.</span><span class="sxs-lookup"><span data-stu-id="a67d0-163">Add the following code into the Main function.</span></span>

<span data-ttu-id="a67d0-164">Kód přidá dva nové objekty University do kontextu.</span><span class="sxs-lookup"><span data-stu-id="a67d0-164">The code adds two new University objects to the context.</span></span> <span data-ttu-id="a67d0-165">Prostorové vlastnosti jsou inicializovány pomocí metody DbGeography.FromText.</span><span class="sxs-lookup"><span data-stu-id="a67d0-165">Spatial properties are initialized by using the DbGeography.FromText method.</span></span> <span data-ttu-id="a67d0-166">Zeměpisné oblasti bod zastoupeny EDT WellKnownText je předán metodě.</span><span class="sxs-lookup"><span data-stu-id="a67d0-166">The geography point represented as WellKnownText is passed to the method.</span></span> <span data-ttu-id="a67d0-167">Kód poté uloží data.</span><span class="sxs-lookup"><span data-stu-id="a67d0-167">The code then saves the data.</span></span> <span data-ttu-id="a67d0-168">Potom dotaz LINQ, který, který vrací objekt University, kde je nejbližší do zadaného umístění, jeho umístění je vytvořen a spustit.</span><span class="sxs-lookup"><span data-stu-id="a67d0-168">Then, the LINQ query that that returns a University object where its location is closest to the specified location, is constructed and executed.</span></span>

``` csharp
using (var context = new UniversityModelContainer())
{
    context.Universities.Add(new University()
    {
        Name = "Graphic Design Institute",
        Location = DbGeography.FromText("POINT(-122.336106 47.605049)"),
    });

    context.Universities.Add(new University()
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

<span data-ttu-id="a67d0-169">Kompilace a spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="a67d0-169">Compile and run the application.</span></span> <span data-ttu-id="a67d0-170">Program vygeneruje následující výstup:</span><span class="sxs-lookup"><span data-stu-id="a67d0-170">The program produces the following output:</span></span>

```
The closest University to you is: School of Fine Art.
```

<span data-ttu-id="a67d0-171">Chcete-li zobrazit data v databázi, klikněte pravým tlačítkem na název databáze v Průzkumníku objektů systému SQL Server a vyberte **aktualizovat**.</span><span class="sxs-lookup"><span data-stu-id="a67d0-171">To view data in the database, right-click on the database name in SQL Server Object Explorer and select **Refresh**.</span></span> <span data-ttu-id="a67d0-172">Klikněte pravým tlačítkem myši na tabulku a výběrem **Data zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="a67d0-172">Then, click the right mouse button on the table and select **View Data**.</span></span>

## <a name="summary"></a><span data-ttu-id="a67d0-173">Souhrn</span><span class="sxs-lookup"><span data-stu-id="a67d0-173">Summary</span></span>

<span data-ttu-id="a67d0-174">V tomto názorném postupu jsme se podívali na tom, jak mapovat pomocí Entity Framework Designer prostorové typy a jak používat prostorové typy v kódu.</span><span class="sxs-lookup"><span data-stu-id="a67d0-174">In this walkthrough we looked at how to map spatial types using the Entity Framework Designer and how to use spatial types in code.</span></span> 
