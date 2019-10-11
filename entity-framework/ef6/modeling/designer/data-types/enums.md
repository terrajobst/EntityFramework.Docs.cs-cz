---
title: Podpora výčtu – Návrhář EF – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c6ae6d8f-1ace-47db-ad47-b1718f1ba082
ms.openlocfilehash: 92a763b84a04d3ce7ec0853ef2a4852356cf7997
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182523"
---
# <a name="enum-support---ef-designer"></a><span data-ttu-id="7c682-102">Podpora výčtu – Návrhář EF</span><span class="sxs-lookup"><span data-stu-id="7c682-102">Enum Support - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="7c682-103">**EF5 pouze** funkce, rozhraní API atd. popsané na této stránce byly představeny v Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="7c682-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="7c682-104">Pokud používáte starší verzi, některé nebo všechny tyto informace neplatí.</span><span class="sxs-lookup"><span data-stu-id="7c682-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="7c682-105">Toto video a podrobný návod vám ukáže, jak používat typy výčtu s Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="7c682-105">This video and step-by-step walkthrough shows how to use enum types with the Entity Framework Designer.</span></span> <span data-ttu-id="7c682-106">Také ukazuje, jak používat výčty v dotazu LINQ.</span><span class="sxs-lookup"><span data-stu-id="7c682-106">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="7c682-107">Tento návod použije Model First k vytvoření nové databáze, ale Návrhář EF lze použít také s pracovním postupem [Database First](~/ef6/modeling/designer/workflows/database-first.md) k mapování existující databáze.</span><span class="sxs-lookup"><span data-stu-id="7c682-107">This walkthrough will use Model First to create a new database, but the EF Designer can also be used with the [Database First](~/ef6/modeling/designer/workflows/database-first.md) workflow to map to an existing database.</span></span>

<span data-ttu-id="7c682-108">Podpora výčtu byla představena v Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="7c682-108">Enum support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="7c682-109">Chcete-li používat nové funkce, jako jsou například výčty, prostorové datové typy a funkce vracející tabulku, je nutné cílit .NET Framework 4,5.</span><span class="sxs-lookup"><span data-stu-id="7c682-109">To use the new features like enums, spatial data types, and table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="7c682-110">Visual Studio 2012 cílí na .NET 4,5 ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="7c682-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="7c682-111">V Entity Framework výčet může mít následující základní typy: **Byte**, **Int16**, **Int32**, **Int64** nebo **SByte**.</span><span class="sxs-lookup"><span data-stu-id="7c682-111">In Entity Framework, an enumeration can have the following underlying types: **Byte**, **Int16**, **Int32**, **Int64** , or **SByte**.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="7c682-112">Přehrát video</span><span class="sxs-lookup"><span data-stu-id="7c682-112">Watch the Video</span></span>
<span data-ttu-id="7c682-113">Toto video ukazuje, jak používat typy výčtu s Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="7c682-113">This video shows how to use enum types with the Entity Framework Designer.</span></span> <span data-ttu-id="7c682-114">Také ukazuje, jak používat výčty v dotazu LINQ.</span><span class="sxs-lookup"><span data-stu-id="7c682-114">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="7c682-115">**Prezentující**: Helena Kornich</span><span class="sxs-lookup"><span data-stu-id="7c682-115">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="7c682-116">**Video**: [WMV](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)</span><span class="sxs-lookup"><span data-stu-id="7c682-116">**Video**: [WMV](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="7c682-117">Předpoklady</span><span class="sxs-lookup"><span data-stu-id="7c682-117">Pre-Requisites</span></span>

<span data-ttu-id="7c682-118">K dokončení tohoto Názorného postupu budete muset mít nainstalovanou verzi sady Visual Studio 2012, Ultimate, Premium, Professional nebo web Express.</span><span class="sxs-lookup"><span data-stu-id="7c682-118">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="7c682-119">Nastavení projektu</span><span class="sxs-lookup"><span data-stu-id="7c682-119">Set up the Project</span></span>

1.  <span data-ttu-id="7c682-120">Otevřete Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="7c682-120">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="7c682-121">V nabídce **soubor** přejděte na příkaz **Nový**a potom klikněte na **projekt** .</span><span class="sxs-lookup"><span data-stu-id="7c682-121">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="7c682-122">V levém podokně klikněte na položku **Visual C @ no__t-1**a potom vyberte šablonu **konzoly** .</span><span class="sxs-lookup"><span data-stu-id="7c682-122">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="7c682-123">Jako název projektu zadejte **EnumEFDesigner** a klikněte na **OK** .</span><span class="sxs-lookup"><span data-stu-id="7c682-123">Enter **EnumEFDesigner** as the name of the project and click **OK**</span></span>

## <a name="create-a-new-model-using-the-ef-designer"></a><span data-ttu-id="7c682-124">Vytvoření nového modelu pomocí návrháře EF</span><span class="sxs-lookup"><span data-stu-id="7c682-124">Create a New Model using the EF Designer</span></span>

1.  <span data-ttu-id="7c682-125">Klikněte pravým tlačítkem myši na název projektu v Průzkumník řešení, přejděte na **Přidat**a klikněte na **Nová položka** .</span><span class="sxs-lookup"><span data-stu-id="7c682-125">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="7c682-126">V nabídce vlevo vyberte **data** a v podokně šablony vyberte **ADO.NET model EDM (Entity Data Model)** .</span><span class="sxs-lookup"><span data-stu-id="7c682-126">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane</span></span>
3.  <span data-ttu-id="7c682-127">Jako název souboru zadejte **EnumTestModel. edmx** a pak klikněte na **Přidat** .</span><span class="sxs-lookup"><span data-stu-id="7c682-127">Enter **EnumTestModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="7c682-128">Na stránce průvodce model EDM (Entity Data Model) vyberte v dialogovém okně Vybrat obsah modelu možnost **prázdný model** .</span><span class="sxs-lookup"><span data-stu-id="7c682-128">On the Entity Data Model Wizard page, select **Empty Model** in the Choose Model Contents dialog box</span></span>
5.  <span data-ttu-id="7c682-129">Klikněte na **Dokončit** .</span><span class="sxs-lookup"><span data-stu-id="7c682-129">Click **Finish**</span></span>

<span data-ttu-id="7c682-130">Zobrazí se Entity Designer, která poskytuje návrhovou plochu pro úpravu vašeho modelu.</span><span class="sxs-lookup"><span data-stu-id="7c682-130">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

<span data-ttu-id="7c682-131">Průvodce provede následující akce:</span><span class="sxs-lookup"><span data-stu-id="7c682-131">The wizard performs the following actions:</span></span>

-   <span data-ttu-id="7c682-132">Vygeneruje soubor EnumTestModel. edmx definující koncepční model, model úložiště a mapování mezi nimi.</span><span class="sxs-lookup"><span data-stu-id="7c682-132">Generates the EnumTestModel.edmx file that defines the conceptual model, the storage model, and the mapping between them.</span></span> <span data-ttu-id="7c682-133">Nastaví vlastnost zpracování artefaktu metadat souboru EDMX pro vložení do výstupního sestavení tak, aby generované soubory metadat byly vloženy do sestavení.</span><span class="sxs-lookup"><span data-stu-id="7c682-133">Sets the Metadata Artifact Processing property of the .edmx file to Embed in Output Assembly so the generated metadata files get embedded into the assembly.</span></span>
-   <span data-ttu-id="7c682-134">Přidá odkaz na následující sestavení: EntityFramework, System. ComponentModel. DataAnnotations a System. data. entity.</span><span class="sxs-lookup"><span data-stu-id="7c682-134">Adds a reference to the following assemblies: EntityFramework, System.ComponentModel.DataAnnotations, and System.Data.Entity.</span></span>
-   <span data-ttu-id="7c682-135">Vytvoří soubory EnumTestModel.tt a EnumTestModel.Context.tt a přidá je do souboru. edmx.</span><span class="sxs-lookup"><span data-stu-id="7c682-135">Creates EnumTestModel.tt and EnumTestModel.Context.tt files and adds them under the .edmx file.</span></span> <span data-ttu-id="7c682-136">Tyto soubory šablon T4 generují kód, který definuje DbContext odvozený typ a POCO typy, které se mapují na entity v modelu. edmx.</span><span class="sxs-lookup"><span data-stu-id="7c682-136">These T4 template files generate the code that defines the DbContext derived type and POCO types that map to the entities in the .edmx model.</span></span>

## <a name="add-a-new-entity-type"></a><span data-ttu-id="7c682-137">Přidat nový typ entity</span><span class="sxs-lookup"><span data-stu-id="7c682-137">Add a New Entity Type</span></span>

1.  <span data-ttu-id="7c682-138">Klikněte pravým tlačítkem myši na prázdnou oblast návrhové plochy, vyberte **Add-&gt; entita –** zobrazí se dialogové okno nová entita.</span><span class="sxs-lookup"><span data-stu-id="7c682-138">Right-click an empty area of the design surface, select **Add -&gt; Entity**, the New Entity dialog box appears</span></span>
2.  <span data-ttu-id="7c682-139">Zadejte pro název typu **oddělení** a pro název vlastnosti Key zadejte **DepartmentID** a ponechte typ jako typ **Int32** .</span><span class="sxs-lookup"><span data-stu-id="7c682-139">Specify **Department** for the type name and specify **DepartmentID** for the key property name, leave the type as **Int32**</span></span>
3.  <span data-ttu-id="7c682-140">Klikněte na **OK** .</span><span class="sxs-lookup"><span data-stu-id="7c682-140">Click **OK**</span></span>
4.  <span data-ttu-id="7c682-141">Klikněte pravým tlačítkem na entitu a vyberte možnost **Přidat skalární vlastnost New-&gt;** .</span><span class="sxs-lookup"><span data-stu-id="7c682-141">Right-click the entity and select **Add New -&gt; Scalar Property**</span></span>
5.  <span data-ttu-id="7c682-142">Přejmenujte novou vlastnost na **název** .</span><span class="sxs-lookup"><span data-stu-id="7c682-142">Rename the new property to **Name**</span></span>
6.  <span data-ttu-id="7c682-143">Změnit typ nové vlastnosti na **Int32** (ve výchozím nastavení je nová vlastnost typu String), aby se typ změnil, otevřete okno Vlastnosti a změňte vlastnost Type na **Int32** .</span><span class="sxs-lookup"><span data-stu-id="7c682-143">Change the type of the new property to **Int32** (by default, the new property is of String type) To change the type, open the Properties window and change the Type property to **Int32**</span></span>
7.  <span data-ttu-id="7c682-144">Přidejte další skalární vlastnost a přejmenujte ji na **rozpočet**, změňte typ na **Decimal** .</span><span class="sxs-lookup"><span data-stu-id="7c682-144">Add another scalar property and rename it to **Budget**, change the type to **Decimal**</span></span>

## <a name="add-an-enum-type"></a><span data-ttu-id="7c682-145">Přidat typ výčtu</span><span class="sxs-lookup"><span data-stu-id="7c682-145">Add an Enum Type</span></span>

1.  <span data-ttu-id="7c682-146">V Entity Framework Designer klikněte pravým tlačítkem na vlastnost Name, vyberte **převést na enum** .</span><span class="sxs-lookup"><span data-stu-id="7c682-146">In the Entity Framework Designer, right-click the Name property, select **Convert to enum**</span></span>

    ![Převést na výčet](~/ef6/media/converttoenum.png)

2.  <span data-ttu-id="7c682-148">V dialogovém okně **Přidat výčet** zadejte **DepartmentNames** pro název typu výčtu, změňte podkladový typ na **Int32**a pak přidejte následující členy do typu: Angličtina, matematický a ekonomie</span><span class="sxs-lookup"><span data-stu-id="7c682-148">In the **Add Enum** dialog box type **DepartmentNames** for the Enum Type Name, change the Underlying Type to **Int32**, and then add the following members to the type: English, Math, and Economics</span></span>

    ![Přidat typ výčtu](~/ef6/media/addenumtype.png)

3.  <span data-ttu-id="7c682-150">Stiskněte **OK**</span><span class="sxs-lookup"><span data-stu-id="7c682-150">Press **OK**</span></span>
4.  <span data-ttu-id="7c682-151">Uložte model a sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="7c682-151">Save the model and build the project</span></span>
    > [!NOTE]
    > <span data-ttu-id="7c682-152">Při sestavování se mohou v Seznam chyb zobrazit upozornění na nemapované entity a přidružení.</span><span class="sxs-lookup"><span data-stu-id="7c682-152">When you build, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="7c682-153">Tato upozornění můžete ignorovat, protože po tom, co se rozhodneme vygenerovat databázi z modelu, budou tyto chyby pryč.</span><span class="sxs-lookup"><span data-stu-id="7c682-153">You can ignore these warnings because after we choose to generate the database from the model, the errors will go away.</span></span>

<span data-ttu-id="7c682-154">Pokud se podíváte na okno Vlastnosti, všimnete si, že typ vlastnosti Name byl změněn na **DepartmentNames** a nově přidaný typ výčtu byl přidán do seznamu typů.</span><span class="sxs-lookup"><span data-stu-id="7c682-154">If you look at the Properties window, you will notice that the type of the Name property was changed to **DepartmentNames** and the newly added enum type was added to the list of types.</span></span>

<span data-ttu-id="7c682-155">Pokud přepnete do okna prohlížeče modelů, uvidíte, že typ byl také přidán do uzlu typy výčtu.</span><span class="sxs-lookup"><span data-stu-id="7c682-155">If you switch to the Model Browser window, you will see that the type was also added to the Enum Types node.</span></span>

![Prohlížeč modelů](~/ef6/media/modelbrowser.png)

>[!NOTE]
> <span data-ttu-id="7c682-157">Můžete také přidat nové typy výčtu z tohoto okna kliknutím pravým tlačítkem myši a výběrem možnosti **Přidat typ výčtu**.</span><span class="sxs-lookup"><span data-stu-id="7c682-157">You can also add new enum types from this window by clicking the right mouse button and selecting **Add Enum Type**.</span></span> <span data-ttu-id="7c682-158">Jakmile se typ vytvoří, zobrazí se v seznamu typů a bude možné ho přidružit k vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="7c682-158">Once the type is created it will appear in the list of types and you would be able to associate with a property</span></span>

## <a name="generate-database-from-model"></a><span data-ttu-id="7c682-159">Generování databáze z modelu</span><span class="sxs-lookup"><span data-stu-id="7c682-159">Generate Database from Model</span></span>

<span data-ttu-id="7c682-160">Teď můžeme vygenerovat databázi založenou na modelu.</span><span class="sxs-lookup"><span data-stu-id="7c682-160">Now we can generate a database that is based on the model.</span></span>

1.  <span data-ttu-id="7c682-161">Klikněte pravým tlačítkem myši na prázdné místo na Entity Designer povrchu a vyberte možnost **Generovat databázi z modelu** .</span><span class="sxs-lookup"><span data-stu-id="7c682-161">Right-click an empty space on the Entity Designer surface and select **Generate Database from Model**</span></span>
2.  <span data-ttu-id="7c682-162">Zobrazí se dialogové okno vybrat datové připojení v průvodci generováním databáze, klikněte na tlačítko **nové připojení** zadat **(LocalDB) \\mssqllocaldb** pro název serveru a **EnumTest** pro databázi a klikněte na **OK** .</span><span class="sxs-lookup"><span data-stu-id="7c682-162">The Choose Your Data Connection Dialog Box of the Generate Database Wizard is displayed Click the **New Connection** button Specify **(localdb)\\mssqllocaldb** for the server name and **EnumTest** for the database and click **OK**</span></span>
3.  <span data-ttu-id="7c682-163">Dialogové okno s dotazem, zda chcete vytvořit novou databázi, se zobrazí automaticky, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="7c682-163">A dialog asking if you want to create a new database will pop up, click **Yes**.</span></span>
4.  <span data-ttu-id="7c682-164">Klikněte na tlačítko **Další** a Průvodce vytvořením databáze GENERUJE příkaz DDL (Data Definition Language) pro vytvoření databáze. VYGENEROVANÝ skript DDL se zobrazí v dialogovém okně Souhrn a nastavení – Poznámka, že skript DDL neobsahuje definici tabulky, která je mapována na typ výčtu</span><span class="sxs-lookup"><span data-stu-id="7c682-164">Click **Next** and the Create Database Wizard generates data definition language (DDL) for creating a database The generated DDL is displayed in the Summary and Settings Dialog Box Note, that the DDL does not contain a definition for a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="7c682-165">Kliknutím na **Dokončit** kliknutím na Dokončit nespustíte skript DDL.</span><span class="sxs-lookup"><span data-stu-id="7c682-165">Click **Finish** Clicking Finish does not execute the DDL script.</span></span>
6.  <span data-ttu-id="7c682-166">Průvodce vytvořením databáze provede následující akce: Otevře soubor **EnumTest. edmx. SQL** v editoru T-SQL, vygeneruje oddíly schématu úložiště a mapování souboru EDMX přidá informace o připojovacím řetězci do souboru App. config.</span><span class="sxs-lookup"><span data-stu-id="7c682-166">The Create Database Wizard does the following: Opens the **EnumTest.edmx.sql** in T-SQL Editor Generates the store schema and mapping sections of the EDMX file Adds connection string information to the App.config file</span></span>
7.  <span data-ttu-id="7c682-167">Klikněte pravým tlačítkem myši v editoru T-SQL a vyberte **Spustit** dialog připojit k serveru, zadejte informace o připojení z kroku 2 a klikněte na **připojit** .</span><span class="sxs-lookup"><span data-stu-id="7c682-167">Click the right mouse button in T-SQL Editor and select **Execute** The Connect to Server dialog appears, enter the connection information from step 2 and click **Connect**</span></span>
8.  <span data-ttu-id="7c682-168">Chcete-li zobrazit vygenerované schéma, klikněte pravým tlačítkem myši na název databáze v Průzkumník objektů systému SQL Server a vyberte možnost **aktualizovat** .</span><span class="sxs-lookup"><span data-stu-id="7c682-168">To view the generated schema, right-click on the database name in SQL Server Object Explorer and select **Refresh**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="7c682-169">Zachovat a načíst data</span><span class="sxs-lookup"><span data-stu-id="7c682-169">Persist and Retrieve Data</span></span>

<span data-ttu-id="7c682-170">Otevřete soubor Program.cs, kde je definována metoda Main.</span><span class="sxs-lookup"><span data-stu-id="7c682-170">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="7c682-171">Do funkce Main přidejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="7c682-171">Add the following code into the Main function.</span></span> <span data-ttu-id="7c682-172">Kód přidá do kontextu nový objekt oddělení.</span><span class="sxs-lookup"><span data-stu-id="7c682-172">The code adds a new Department object to the context.</span></span> <span data-ttu-id="7c682-173">Pak data uloží.</span><span class="sxs-lookup"><span data-stu-id="7c682-173">It then saves the data.</span></span> <span data-ttu-id="7c682-174">Kód také spustí dotaz LINQ, který vrací oddělení, kde název je DepartmentNames. English.</span><span class="sxs-lookup"><span data-stu-id="7c682-174">The code also executes a LINQ query that returns a Department where the name is DepartmentNames.English.</span></span>

``` csharp
using (var context = new EnumTestModelContainer())
{
    context.Departments.Add(new Department{ Name = DepartmentNames.English });

    context.SaveChanges();

    var department = (from d in context.Departments
                        where d.Name == DepartmentNames.English
                        select d).FirstOrDefault();

    Console.WriteLine(
        "DepartmentID: {0} and Name: {1}",
        department.DepartmentID,  
        department.Name);
}
```

<span data-ttu-id="7c682-175">Zkompilujte a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7c682-175">Compile and run the application.</span></span> <span data-ttu-id="7c682-176">Program vytvoří následující výstup:</span><span class="sxs-lookup"><span data-stu-id="7c682-176">The program produces the following output:</span></span>

```console
DepartmentID: 1 Name: English
```

<span data-ttu-id="7c682-177">Chcete-li zobrazit data v databázi, klikněte pravým tlačítkem myši na název databáze v Průzkumník objektů systému SQL Server a vyberte možnost **aktualizovat**.</span><span class="sxs-lookup"><span data-stu-id="7c682-177">To view data in the database, right-click on the database name in SQL Server Object Explorer and select **Refresh**.</span></span> <span data-ttu-id="7c682-178">Pak klikněte pravým tlačítkem myši na tabulku a vyberte **Zobrazit data**.</span><span class="sxs-lookup"><span data-stu-id="7c682-178">Then, click the right mouse button on the table and select **View Data**.</span></span>

## <a name="summary"></a><span data-ttu-id="7c682-179">Souhrn</span><span class="sxs-lookup"><span data-stu-id="7c682-179">Summary</span></span>

<span data-ttu-id="7c682-180">V tomto návodu jsme se podívali na to, jak namapovat typy výčtu pomocí Entity Framework Designer a jak používat výčty v kódu.</span><span class="sxs-lookup"><span data-stu-id="7c682-180">In this walkthrough we looked at how to map enum types using the Entity Framework Designer and how to use enums in code.</span></span> 
