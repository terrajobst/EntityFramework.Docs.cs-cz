---
title: Prostor – Návrhář EF – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 06baa6e1-d680-4a95-845b-81305c87a962
ms.openlocfilehash: a9c54fbc14dd02ce5d4d91449a0d5f9e72f7f0f7
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418537"
---
# <a name="spatial---ef-designer"></a><span data-ttu-id="1bf56-102">Prostor – Návrhář EF</span><span class="sxs-lookup"><span data-stu-id="1bf56-102">Spatial - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="1bf56-103">**EF5 pouze** funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="1bf56-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="1bf56-104">Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.</span><span class="sxs-lookup"><span data-stu-id="1bf56-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="1bf56-105">Video a podrobný návod vám ukáže, jak namapovat prostorové typy pomocí Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="1bf56-105">The video and step-by-step walkthrough shows how to map spatial types with the Entity Framework Designer.</span></span> <span data-ttu-id="1bf56-106">Také ukazuje, jak použít dotaz LINQ k nalezení vzdálenosti mezi dvěma umístěními.</span><span class="sxs-lookup"><span data-stu-id="1bf56-106">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="1bf56-107">Tento návod použije Model First k vytvoření nové databáze, ale Návrhář EF lze použít také s pracovním postupem [Database First](~/ef6/modeling/designer/workflows/database-first.md) k mapování existující databáze.</span><span class="sxs-lookup"><span data-stu-id="1bf56-107">This walkthrough will use Model First to create a new database, but the EF Designer can also be used with the [Database First](~/ef6/modeling/designer/workflows/database-first.md) workflow to map to an existing database.</span></span>

<span data-ttu-id="1bf56-108">Podpora prostorového typu byla představena v Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="1bf56-108">Spatial type support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="1bf56-109">Všimněte si, že pro použití nových funkcí, jako je prostorový typ, výčty a funkce vracející tabulku, je nutné cílit .NET Framework 4,5.</span><span class="sxs-lookup"><span data-stu-id="1bf56-109">Note that to use the new features like spatial type, enums, and Table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="1bf56-110">Visual Studio 2012 cílí na .NET 4,5 ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="1bf56-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="1bf56-111">Chcete-li použít typy prostorových dat, musíte také použít poskytovatele Entity Framework, který má prostorovou podporu.</span><span class="sxs-lookup"><span data-stu-id="1bf56-111">To use spatial data types you must also use an Entity Framework provider that has spatial support.</span></span> <span data-ttu-id="1bf56-112">Další informace najdete v tématu [Podpora poskytovatele pro prostorové typy](~/ef6/fundamentals/providers/spatial-support.md) .</span><span class="sxs-lookup"><span data-stu-id="1bf56-112">See [provider support for spatial types](~/ef6/fundamentals/providers/spatial-support.md) for more information.</span></span>

<span data-ttu-id="1bf56-113">Existují dva hlavní typy prostorových dat: Geografie a geometrie.</span><span class="sxs-lookup"><span data-stu-id="1bf56-113">There are two main spatial data types: geography and geometry.</span></span> <span data-ttu-id="1bf56-114">Zeměpisný datový typ ukládá data ellipsoidal (například souřadnice GPS zeměpisné šířky a délky).</span><span class="sxs-lookup"><span data-stu-id="1bf56-114">The geography data type stores ellipsoidal data (for example, GPS latitude and longitude coordinates).</span></span> <span data-ttu-id="1bf56-115">Typ dat geometrie představuje systém souřadnic Euclidean (plochý).</span><span class="sxs-lookup"><span data-stu-id="1bf56-115">The geometry data type represents Euclidean (flat) coordinate system.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="1bf56-116">Přehrát video</span><span class="sxs-lookup"><span data-stu-id="1bf56-116">Watch the video</span></span>
<span data-ttu-id="1bf56-117">Toto video ukazuje, jak namapovat prostorové typy pomocí Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="1bf56-117">This video shows how to map spatial types with the Entity Framework Designer.</span></span> <span data-ttu-id="1bf56-118">Také ukazuje, jak použít dotaz LINQ k nalezení vzdálenosti mezi dvěma umístěními.</span><span class="sxs-lookup"><span data-stu-id="1bf56-118">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="1bf56-119">**Prezentující**: Helena Kornich</span><span class="sxs-lookup"><span data-stu-id="1bf56-119">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="1bf56-120">**Video**: [wmv](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)</span><span class="sxs-lookup"><span data-stu-id="1bf56-120">**Video**: [WMV](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="1bf56-121">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1bf56-121">Pre-Requisites</span></span>

<span data-ttu-id="1bf56-122">K dokončení tohoto Názorného postupu budete muset mít nainstalovanou verzi sady Visual Studio 2012, Ultimate, Premium, Professional nebo web Express.</span><span class="sxs-lookup"><span data-stu-id="1bf56-122">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="1bf56-123">Nastavení projektu</span><span class="sxs-lookup"><span data-stu-id="1bf56-123">Set up the Project</span></span>

1.  <span data-ttu-id="1bf56-124">Otevřete Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="1bf56-124">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="1bf56-125">V nabídce **soubor** přejděte na příkaz **Nový**a potom klikněte na **projekt** .</span><span class="sxs-lookup"><span data-stu-id="1bf56-125">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="1bf56-126">V levém podokně klikněte na položku **Visual C\#** a pak vyberte šablonu **konzoly** .</span><span class="sxs-lookup"><span data-stu-id="1bf56-126">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="1bf56-127">Jako název projektu zadejte **SpatialEFDesigner** a klikněte na **OK** .</span><span class="sxs-lookup"><span data-stu-id="1bf56-127">Enter **SpatialEFDesigner** as the name of the project and click **OK**</span></span>

## <a name="create-a-new-model-using-the-ef-designer"></a><span data-ttu-id="1bf56-128">Vytvoření nového modelu pomocí návrháře EF</span><span class="sxs-lookup"><span data-stu-id="1bf56-128">Create a New Model using the EF Designer</span></span>

1.  <span data-ttu-id="1bf56-129">Klikněte pravým tlačítkem myši na název projektu v Průzkumník řešení, přejděte na **Přidat**a klikněte na **Nová položka** .</span><span class="sxs-lookup"><span data-stu-id="1bf56-129">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="1bf56-130">V nabídce vlevo vyberte **data** a v podokně šablony vyberte **ADO.NET model EDM (Entity Data Model)** .</span><span class="sxs-lookup"><span data-stu-id="1bf56-130">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane</span></span>
3.  <span data-ttu-id="1bf56-131">Jako název souboru zadejte **UniversityModel. edmx** a pak klikněte na **Přidat** .</span><span class="sxs-lookup"><span data-stu-id="1bf56-131">Enter **UniversityModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="1bf56-132">Na stránce průvodce model EDM (Entity Data Model) vyberte v dialogovém okně Vybrat obsah modelu možnost **prázdný model** .</span><span class="sxs-lookup"><span data-stu-id="1bf56-132">On the Entity Data Model Wizard page, select **Empty Model** in the Choose Model Contents dialog box</span></span>
5.  <span data-ttu-id="1bf56-133">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="1bf56-133">Click **Finish**</span></span>

<span data-ttu-id="1bf56-134">Zobrazí se Entity Designer, která poskytuje návrhovou plochu pro úpravu vašeho modelu.</span><span class="sxs-lookup"><span data-stu-id="1bf56-134">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

<span data-ttu-id="1bf56-135">Průvodce provede následující akce:</span><span class="sxs-lookup"><span data-stu-id="1bf56-135">The wizard performs the following actions:</span></span>

-   <span data-ttu-id="1bf56-136">Vygeneruje soubor EnumTestModel. edmx definující koncepční model, model úložiště a mapování mezi nimi.</span><span class="sxs-lookup"><span data-stu-id="1bf56-136">Generates the EnumTestModel.edmx file that defines the conceptual model, the storage model, and the mapping between them.</span></span> <span data-ttu-id="1bf56-137">Nastaví vlastnost zpracování artefaktu metadat souboru EDMX pro vložení do výstupního sestavení tak, aby generované soubory metadat byly vloženy do sestavení.</span><span class="sxs-lookup"><span data-stu-id="1bf56-137">Sets the Metadata Artifact Processing property of the .edmx file to Embed in Output Assembly so the generated metadata files get embedded into the assembly.</span></span>
-   <span data-ttu-id="1bf56-138">Přidá odkaz na následující sestavení: EntityFramework, System. ComponentModel. DataAnnotations a System. data. entity.</span><span class="sxs-lookup"><span data-stu-id="1bf56-138">Adds a reference to the following assemblies: EntityFramework, System.ComponentModel.DataAnnotations, and System.Data.Entity.</span></span>
-   <span data-ttu-id="1bf56-139">Vytvoří soubory UniversityModel.tt a UniversityModel.Context.tt a přidá je do souboru. edmx.</span><span class="sxs-lookup"><span data-stu-id="1bf56-139">Creates UniversityModel.tt and UniversityModel.Context.tt files and adds them under the .edmx file.</span></span> <span data-ttu-id="1bf56-140">Tyto soubory šablon T4 generují kód, který definuje DbContext odvozený typ a POCO typy, které se mapují na entity v modelu. edmx.</span><span class="sxs-lookup"><span data-stu-id="1bf56-140">These T4 template files generate the code that defines the DbContext derived type and POCO types that map to the entities in the .edmx model</span></span>

## <a name="add-a-new-entity-type"></a><span data-ttu-id="1bf56-141">Přidat nový typ entity</span><span class="sxs-lookup"><span data-stu-id="1bf56-141">Add a New Entity Type</span></span>

1.  <span data-ttu-id="1bf56-142">Klikněte pravým tlačítkem myši na prázdnou oblast návrhové plochy, vyberte možnost **přidat&gt; entitu**, otevře se dialogové okno nová entita.</span><span class="sxs-lookup"><span data-stu-id="1bf56-142">Right-click an empty area of the design surface, select **Add -&gt; Entity**, the New Entity dialog box appears</span></span>
2.  <span data-ttu-id="1bf56-143">Zadejte **univerzita** pro název typu a jako název vlastnosti zadejte **UniversityID** . ponechte typ jako **Int32** .</span><span class="sxs-lookup"><span data-stu-id="1bf56-143">Specify **University** for the type name and specify **UniversityID** for the key property name, leave the type as **Int32**</span></span>
3.  <span data-ttu-id="1bf56-144">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="1bf56-144">Click **OK**</span></span>
4.  <span data-ttu-id="1bf56-145">Klikněte pravým tlačítkem na entitu a vyberte **Přidat novou&gt; skalární vlastnost** .</span><span class="sxs-lookup"><span data-stu-id="1bf56-145">Right-click the entity and select **Add New -&gt; Scalar Property**</span></span>
5.  <span data-ttu-id="1bf56-146">Přejmenujte novou vlastnost na **název** .</span><span class="sxs-lookup"><span data-stu-id="1bf56-146">Rename the new property to **Name**</span></span>
6.  <span data-ttu-id="1bf56-147">Přidejte další skalární vlastnost a přejmenujte ji na **umístění** otevřít okno Vlastnosti a změňte typ nové vlastnosti na **geografie** .</span><span class="sxs-lookup"><span data-stu-id="1bf56-147">Add another scalar property and rename it to **Location** Open the Properties window and change the type of the new property to **Geography**</span></span>
7.  <span data-ttu-id="1bf56-148">Uložte model a sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="1bf56-148">Save the model and build the project</span></span>
    > [!NOTE]
    > <span data-ttu-id="1bf56-149">Při sestavování se mohou v Seznam chyb zobrazit upozornění na nemapované entity a přidružení.</span><span class="sxs-lookup"><span data-stu-id="1bf56-149">When you build, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="1bf56-150">Tato upozornění můžete ignorovat, protože po tom, co se rozhodneme vygenerovat databázi z modelu, budou tyto chyby pryč.</span><span class="sxs-lookup"><span data-stu-id="1bf56-150">You can ignore these warnings because after we choose to generate the database from the model, the errors will go away.</span></span>

## <a name="generate-database-from-model"></a><span data-ttu-id="1bf56-151">Generování databáze z modelu</span><span class="sxs-lookup"><span data-stu-id="1bf56-151">Generate Database from Model</span></span>

<span data-ttu-id="1bf56-152">Teď můžeme vygenerovat databázi založenou na modelu.</span><span class="sxs-lookup"><span data-stu-id="1bf56-152">Now we can generate a database that is based on the model.</span></span>

1.  <span data-ttu-id="1bf56-153">Klikněte pravým tlačítkem myši na prázdné místo na Entity Designer povrchu a vyberte možnost **Generovat databázi z modelu** .</span><span class="sxs-lookup"><span data-stu-id="1bf56-153">Right-click an empty space on the Entity Designer surface and select **Generate Database from Model**</span></span>
2.  <span data-ttu-id="1bf56-154">Zobrazí se dialogové okno vybrat datové připojení v průvodci generováním databáze, klikněte na tlačítko **nové připojení** zadat **(LocalDB)\\mssqllocaldb** pro název serveru a **University** pro databázi a klikněte na tlačítko **OK** .</span><span class="sxs-lookup"><span data-stu-id="1bf56-154">The Choose Your Data Connection Dialog Box of the Generate Database Wizard is displayed Click the **New Connection** button Specify **(localdb)\\mssqllocaldb** for the server name and **University** for the database and click **OK**</span></span>
3.  <span data-ttu-id="1bf56-155">Dialogové okno s dotazem, zda chcete vytvořit novou databázi, se zobrazí automaticky, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="1bf56-155">A dialog asking if you want to create a new database will pop up, click **Yes**.</span></span>
4.  <span data-ttu-id="1bf56-156">Klikněte na tlačítko **Další** a Průvodce vytvořením databáze generuje pro vytvoření databáze jazyk DDL (Data Definition Language), který VYGENEROVANÝ skript DDL je zobrazen v dialogovém okně Souhrn a nastavení. Všimněte si, že skript DDL neobsahuje definici tabulky, která je mapována na typ výčtu.</span><span class="sxs-lookup"><span data-stu-id="1bf56-156">Click **Next** and the Create Database Wizard generates data definition language (DDL) for creating a database The generated DDL is displayed in the Summary and Settings Dialog Box Note, that the DDL does not contain a definition for a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="1bf56-157">Kliknutím na **Dokončit** kliknutím na Dokončit nespustíte skript DDL.</span><span class="sxs-lookup"><span data-stu-id="1bf56-157">Click **Finish** Clicking Finish does not execute the DDL script.</span></span>
6.  <span data-ttu-id="1bf56-158">Průvodce vytvořením databáze provede následující: otevře **UniversityModel. edmx. SQL** v editoru T-SQL vygeneruje oddíly schématu úložiště a mapování souboru EDMX přidá informace o připojovacím řetězci do souboru App. config.</span><span class="sxs-lookup"><span data-stu-id="1bf56-158">The Create Database Wizard does the following: Opens the **UniversityModel.edmx.sql** in T-SQL Editor Generates the store schema and mapping sections of the EDMX file Adds connection string information to the App.config file</span></span>
7.  <span data-ttu-id="1bf56-159">Klikněte pravým tlačítkem myši v editoru T-SQL a vyberte **Spustit** dialog připojit k serveru, zadejte informace o připojení z kroku 2 a klikněte na **připojit** .</span><span class="sxs-lookup"><span data-stu-id="1bf56-159">Click the right mouse button in T-SQL Editor and select **Execute** The Connect to Server dialog appears, enter the connection information from step 2 and click **Connect**</span></span>
8.  <span data-ttu-id="1bf56-160">Chcete-li zobrazit vygenerované schéma, klikněte pravým tlačítkem myši na název databáze v Průzkumník objektů systému SQL Server a vyberte možnost **aktualizovat** .</span><span class="sxs-lookup"><span data-stu-id="1bf56-160">To view the generated schema, right-click on the database name in SQL Server Object Explorer and select **Refresh**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="1bf56-161">Zachovat a načíst data</span><span class="sxs-lookup"><span data-stu-id="1bf56-161">Persist and Retrieve Data</span></span>

<span data-ttu-id="1bf56-162">Otevřete soubor Program.cs, kde je definována metoda Main.</span><span class="sxs-lookup"><span data-stu-id="1bf56-162">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="1bf56-163">Do funkce Main přidejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="1bf56-163">Add the following code into the Main function.</span></span>

<span data-ttu-id="1bf56-164">Kód přidá do kontextu dva nové objekty vysoké školy.</span><span class="sxs-lookup"><span data-stu-id="1bf56-164">The code adds two new University objects to the context.</span></span> <span data-ttu-id="1bf56-165">Prostorové vlastnosti jsou inicializovány pomocí metody DbGeography. FromText.</span><span class="sxs-lookup"><span data-stu-id="1bf56-165">Spatial properties are initialized by using the DbGeography.FromText method.</span></span> <span data-ttu-id="1bf56-166">Zeměpisný bod reprezentovaný jako WellKnownText je předán metodě.</span><span class="sxs-lookup"><span data-stu-id="1bf56-166">The geography point represented as WellKnownText is passed to the method.</span></span> <span data-ttu-id="1bf56-167">Kód pak data uloží.</span><span class="sxs-lookup"><span data-stu-id="1bf56-167">The code then saves the data.</span></span> <span data-ttu-id="1bf56-168">Pak dotaz LINQ, který vrací objekt univerzity, kde je jeho umístění nejblíže zadanému umístění, je vytvořen a proveden.</span><span class="sxs-lookup"><span data-stu-id="1bf56-168">Then, the LINQ query that that returns a University object where its location is closest to the specified location, is constructed and executed.</span></span>

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

<span data-ttu-id="1bf56-169">Zkompilujte a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1bf56-169">Compile and run the application.</span></span> <span data-ttu-id="1bf56-170">Program vytvoří následující výstup:</span><span class="sxs-lookup"><span data-stu-id="1bf56-170">The program produces the following output:</span></span>

```console
The closest University to you is: School of Fine Art.
```

<span data-ttu-id="1bf56-171">Chcete-li zobrazit data v databázi, klikněte pravým tlačítkem myši na název databáze v Průzkumník objektů systému SQL Server a vyberte možnost **aktualizovat**.</span><span class="sxs-lookup"><span data-stu-id="1bf56-171">To view data in the database, right-click on the database name in SQL Server Object Explorer and select **Refresh**.</span></span> <span data-ttu-id="1bf56-172">Pak klikněte pravým tlačítkem myši na tabulku a vyberte **Zobrazit data**.</span><span class="sxs-lookup"><span data-stu-id="1bf56-172">Then, click the right mouse button on the table and select **View Data**.</span></span>

## <a name="summary"></a><span data-ttu-id="1bf56-173">Souhrn</span><span class="sxs-lookup"><span data-stu-id="1bf56-173">Summary</span></span>

<span data-ttu-id="1bf56-174">V tomto návodu jsme se podívali na to, jak namapovat typy prostorů pomocí Entity Framework Designer a jak používat prostorové typy v kódu.</span><span class="sxs-lookup"><span data-stu-id="1bf56-174">In this walkthrough we looked at how to map spatial types using the Entity Framework Designer and how to use spatial types in code.</span></span> 
