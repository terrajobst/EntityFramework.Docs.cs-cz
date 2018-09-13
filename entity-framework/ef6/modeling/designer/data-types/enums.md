---
title: EF6 podpory – EF designeru – výčet
author: divega
ms.date: 10/23/2016
ms.assetid: c6ae6d8f-1ace-47db-ad47-b1718f1ba082
ms.openlocfilehash: 4c2562ade28dc30df20eb2a35c179582bf6e0777
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490789"
---
# <a name="enum-support---ef-designer"></a><span data-ttu-id="bc893-102">Podpora výčtu – EF designeru</span><span class="sxs-lookup"><span data-stu-id="bc893-102">Enum Support - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="bc893-103">**EF5 a vyšší pouze** – funkce rozhraní API, atd. popsané na této stránce se zavedly v Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="bc893-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="bc893-104">Pokud používáte starší verzi, některé nebo všechny informace neplatí.</span><span class="sxs-lookup"><span data-stu-id="bc893-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="bc893-105">Tato videa a podrobný návod ukazuje, jak používat typy výčtu s Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="bc893-105">This video and step-by-step walkthrough shows how to use enum types with the Entity Framework Designer.</span></span> <span data-ttu-id="bc893-106">Také ukazuje, jak používat výčty v dotazu LINQ.</span><span class="sxs-lookup"><span data-stu-id="bc893-106">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="bc893-107">Tento názorný postup použije první Model k vytvoření nové databáze, ale EF designeru je také možné se [Database First](~/ef6/modeling/designer/workflows/database-first.md) pracovního postupu pro mapování na existující databázi.</span><span class="sxs-lookup"><span data-stu-id="bc893-107">This walkthrough will use Model First to create a new database, but the EF Designer can also be used with the [Database First](~/ef6/modeling/designer/workflows/database-first.md) workflow to map to an existing database.</span></span>

<span data-ttu-id="bc893-108">Podpora výčtu byla zavedena v Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="bc893-108">Enum support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="bc893-109">Pokud chcete použít nové funkce jako výčty prostorové datové typy a funkce vracející tabulku, je potřeba cílit rozhraní .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="bc893-109">To use the new features like enums, spatial data types, and table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="bc893-110">Visual Studio 2012 cílí na rozhraní .NET 4.5 ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="bc893-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="bc893-111">V rozhraní Entity Framework výčet může mít následující základní typy: **bajtů**, **Int16**, **Int32**, **Int64** , nebo **SByte**.</span><span class="sxs-lookup"><span data-stu-id="bc893-111">In Entity Framework, an enumeration can have the following underlying types: **Byte**, **Int16**, **Int32**, **Int64** , or **SByte**.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="bc893-112">Podívejte se na Video</span><span class="sxs-lookup"><span data-stu-id="bc893-112">Watch the Video</span></span>
<span data-ttu-id="bc893-113">Toto video ukazuje, jak používat typy výčtu s Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="bc893-113">This video shows how to use enum types with the Entity Framework Designer.</span></span> <span data-ttu-id="bc893-114">Také ukazuje, jak používat výčty v dotazu LINQ.</span><span class="sxs-lookup"><span data-stu-id="bc893-114">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="bc893-115">**Přednášející:**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="bc893-115">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="bc893-116">**Video**: [WMV](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)</span><span class="sxs-lookup"><span data-stu-id="bc893-116">**Video**: [WMV](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="bc893-117">Předpoklady</span><span class="sxs-lookup"><span data-stu-id="bc893-117">Pre-Requisites</span></span>

<span data-ttu-id="bc893-118">Musíte mít Visual Studio 2012, Ultimate, Premium, Professional nebo Web Express edition nainstalované k dokončení tohoto návodu.</span><span class="sxs-lookup"><span data-stu-id="bc893-118">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="bc893-119">Nastavení projektu</span><span class="sxs-lookup"><span data-stu-id="bc893-119">Set up the Project</span></span>

1.  <span data-ttu-id="bc893-120">Otevřít Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="bc893-120">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="bc893-121">Na **souboru** nabídky, přejděte k **nový**a potom klikněte na tlačítko **projektu**</span><span class="sxs-lookup"><span data-stu-id="bc893-121">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="bc893-122">V levém podokně klikněte na tlačítko **Visual C\#** a pak vyberte **konzoly** šablony</span><span class="sxs-lookup"><span data-stu-id="bc893-122">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="bc893-123">Zadejte **EnumEFDesigner** jako název projektu a klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="bc893-123">Enter **EnumEFDesigner** as the name of the project and click **OK**</span></span>

## <a name="create-a-new-model-using-the-ef-designer"></a><span data-ttu-id="bc893-124">Vytvořit nový Model využívající EF designeru</span><span class="sxs-lookup"><span data-stu-id="bc893-124">Create a New Model using the EF Designer</span></span>

1.  <span data-ttu-id="bc893-125">Klikněte pravým tlačítkem na název projektu v Průzkumníku řešení, přejděte na **přidat**a potom klikněte na tlačítko **nová položka**</span><span class="sxs-lookup"><span data-stu-id="bc893-125">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="bc893-126">Vyberte **Data** v levé nabídce a pak vyberte **datový Model Entity ADO.NET** v podokně šablon</span><span class="sxs-lookup"><span data-stu-id="bc893-126">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane</span></span>
3.  <span data-ttu-id="bc893-127">Zadejte **EnumTestModel.edmx** pro název souboru a pak klikněte na tlačítko **přidat**</span><span class="sxs-lookup"><span data-stu-id="bc893-127">Enter **EnumTestModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="bc893-128">Na stránce Průvodce datovým modelem Entity vyberte **prázdný Model** v dialogovém okně Výběr obsahu modelu</span><span class="sxs-lookup"><span data-stu-id="bc893-128">On the Entity Data Model Wizard page, select **Empty Model** in the Choose Model Contents dialog box</span></span>
5.  <span data-ttu-id="bc893-129">Klikněte na tlačítko **dokončit**</span><span class="sxs-lookup"><span data-stu-id="bc893-129">Click **Finish**</span></span>

<span data-ttu-id="bc893-130">V návrháři entit, které poskytuje návrhové ploše pro úpravy váš model, se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="bc893-130">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

<span data-ttu-id="bc893-131">Průvodce provede následující akce:</span><span class="sxs-lookup"><span data-stu-id="bc893-131">The wizard performs the following actions:</span></span>

-   <span data-ttu-id="bc893-132">Generuje EnumTestModel.edmx soubor, který definuje mapování mezi nimi, konceptuálního modelu a model úložiště.</span><span class="sxs-lookup"><span data-stu-id="bc893-132">Generates the EnumTestModel.edmx file that defines the conceptual model, the storage model, and the mapping between them.</span></span> <span data-ttu-id="bc893-133">Nastaví vlastnost Metadata artefaktů zpracování souboru vložit do výstupního sestavení tak získat soubory generované metadat vložen do sestavení.</span><span class="sxs-lookup"><span data-stu-id="bc893-133">Sets the Metadata Artifact Processing property of the .edmx file to Embed in Output Assembly so the generated metadata files get embedded into the assembly.</span></span>
-   <span data-ttu-id="bc893-134">Přidá odkaz na následující sestavení: objektu EntityFramework, System.ComponentModel.DataAnnotations a System.Data.Entity.</span><span class="sxs-lookup"><span data-stu-id="bc893-134">Adds a reference to the following assemblies: EntityFramework, System.ComponentModel.DataAnnotations, and System.Data.Entity.</span></span>
-   <span data-ttu-id="bc893-135">Vytvoří soubory EnumTestModel.tt a EnumTestModel.Context.tt a přidá je v souboru .edmx.</span><span class="sxs-lookup"><span data-stu-id="bc893-135">Creates EnumTestModel.tt and EnumTestModel.Context.tt files and adds them under the .edmx file.</span></span> <span data-ttu-id="bc893-136">Tyto soubory šablon T4 generovat kód, který definuje DbContext odvozený typ a POCO typy, které mapují na entity v modelu edmx.</span><span class="sxs-lookup"><span data-stu-id="bc893-136">These T4 template files generate the code that defines the DbContext derived type and POCO types that map to the entities in the .edmx model.</span></span>

## <a name="add-a-new-entity-type"></a><span data-ttu-id="bc893-137">Přidat nový typ Entity</span><span class="sxs-lookup"><span data-stu-id="bc893-137">Add a New Entity Type</span></span>

1.  <span data-ttu-id="bc893-138">Klikněte pravým tlačítkem na prázdnou oblast návrhové plochy, vyberte **Add -&gt; Entity**, zobrazí se dialogové okno Nová entita</span><span class="sxs-lookup"><span data-stu-id="bc893-138">Right-click an empty area of the design surface, select **Add -&gt; Entity**, the New Entity dialog box appears</span></span>
2.  <span data-ttu-id="bc893-139">Zadejte **oddělení** pro typ název a určete **DepartmentID** název klíče vlastnosti ponechat typ jako **Int32**</span><span class="sxs-lookup"><span data-stu-id="bc893-139">Specify **Department** for the type name and specify **DepartmentID** for the key property name, leave the type as **Int32**</span></span>
3.  <span data-ttu-id="bc893-140">Klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="bc893-140">Click **OK**</span></span>
4.  <span data-ttu-id="bc893-141">Klikněte pravým tlačítkem na entitu a vyberte **přidat nový -&gt; skalární vlastnost**</span><span class="sxs-lookup"><span data-stu-id="bc893-141">Right-click the entity and select **Add New -&gt; Scalar Property**</span></span>
5.  <span data-ttu-id="bc893-142">Přejmenujte novou vlastnost **název**</span><span class="sxs-lookup"><span data-stu-id="bc893-142">Rename the new property to **Name**</span></span>
6.  <span data-ttu-id="bc893-143">Změna typu novou vlastnost **Int32** (ve výchozím nastavení, nové vlastnosti je typu String) Chcete-li změnit typ, otevřete okno Vlastnosti a změňte vlastnost typ **Int32**</span><span class="sxs-lookup"><span data-stu-id="bc893-143">Change the type of the new property to **Int32** (by default, the new property is of String type) To change the type, open the Properties window and change the Type property to **Int32**</span></span>
7.  <span data-ttu-id="bc893-144">Přidat jiný skalární vlastnost a přejmenujte ho na **rozpočtu**, změňte typ na **Decimal**</span><span class="sxs-lookup"><span data-stu-id="bc893-144">Add another scalar property and rename it to **Budget**, change the type to **Decimal**</span></span>

## <a name="add-an-enum-type"></a><span data-ttu-id="bc893-145">Přidání typu výčtu</span><span class="sxs-lookup"><span data-stu-id="bc893-145">Add an Enum Type</span></span>

1.  <span data-ttu-id="bc893-146">V Návrháři Entity Framework Designer klikněte pravým tlačítkem na název vlastnosti, vyberte **převést na výčet**</span><span class="sxs-lookup"><span data-stu-id="bc893-146">In the Entity Framework Designer, right-click the Name property, select **Convert to enum**</span></span>

    ![Převést na výčet](~/ef6/media/converttoenum.png)

2.  <span data-ttu-id="bc893-148">V **přidat výčtu** dialogového okna zadejte **DepartmentNames** názvem typu výčtu, změňte základní typ na **Int32**, a potom přidat následující členy typu: angličtině, Matematické a hospodárnost</span><span class="sxs-lookup"><span data-stu-id="bc893-148">In the **Add Enum** dialog box type **DepartmentNames** for the Enum Type Name, change the Underlying Type to **Int32**, and then add the following members to the type: English, Math, and Economics</span></span>

    ![Přidat typ výčtu](~/ef6/media/addenumtype.png)

3.  <span data-ttu-id="bc893-150">Stisknutím klávesy **OK**</span><span class="sxs-lookup"><span data-stu-id="bc893-150">Press **OK**</span></span>
4.  <span data-ttu-id="bc893-151">Uložit model a sestavte projekt</span><span class="sxs-lookup"><span data-stu-id="bc893-151">Save the model and build the project</span></span>
    > [!NOTE]
    > <span data-ttu-id="bc893-152">Při sestavování se může zobrazit upozornění na nenamapované entit a přidružení v seznamu chyb.</span><span class="sxs-lookup"><span data-stu-id="bc893-152">When you build, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="bc893-153">Tato upozornění můžete ignorovat, protože po zvolíme, aby se vygenerovala databáze z modelu, chyby zmizí.</span><span class="sxs-lookup"><span data-stu-id="bc893-153">You can ignore these warnings because after we choose to generate the database from the model, the errors will go away.</span></span>

<span data-ttu-id="bc893-154">Když se podíváte na okno Vlastnosti, všimnete si, že typ vlastnosti názvu byl změněn na **DepartmentNames** a typ výčtu nově přidaných byl přidán do seznamu typů.</span><span class="sxs-lookup"><span data-stu-id="bc893-154">If you look at the Properties window, you will notice that the type of the Name property was changed to **DepartmentNames** and the newly added enum type was added to the list of types.</span></span>

<span data-ttu-id="bc893-155">Pokud přejdete do okna prohlížeče modelu, uvidíte, že typ se taky přidala do uzlu typy výčtu.</span><span class="sxs-lookup"><span data-stu-id="bc893-155">If you switch to the Model Browser window, you will see that the type was also added to the Enum Types node.</span></span>

![Prohlížeč modelu](~/ef6/media/modelbrowser.png)

>[!NOTE]
> <span data-ttu-id="bc893-157">Nové typy výčtu z tohoto okna můžete také přidat kliknutím pravým tlačítkem myši a vyberete **přidat typ výčtu**.</span><span class="sxs-lookup"><span data-stu-id="bc893-157">You can also add new enum types from this window by clicking the right mouse button and selecting **Add Enum Type**.</span></span> <span data-ttu-id="bc893-158">Po vytvoření typu se zobrazí v seznamu typů a bylo by možné přiřadit k vlastnosti</span><span class="sxs-lookup"><span data-stu-id="bc893-158">Once the type is created it will appear in the list of types and you would be able to associate with a property</span></span>

## <a name="generate-database-from-model"></a><span data-ttu-id="bc893-159">Generování databáze z modelu</span><span class="sxs-lookup"><span data-stu-id="bc893-159">Generate Database from Model</span></span>

<span data-ttu-id="bc893-160">Nyní jsme můžete vygenerovat databázi, která je založená na modelu.</span><span class="sxs-lookup"><span data-stu-id="bc893-160">Now we can generate a database that is based on the model.</span></span>

1.  <span data-ttu-id="bc893-161">Klikněte pravým tlačítkem na prázdné místo na ploše a vyberte v návrháři entit **generovat databáze z modelu**</span><span class="sxs-lookup"><span data-stu-id="bc893-161">Right-click an empty space on the Entity Designer surface and select **Generate Database from Model**</span></span>
2.  <span data-ttu-id="bc893-162">Vyberte vaše Data Connection Dialog Box průvodce generovat databáze je zobrazit kliknutím **nové připojení** tlačítko zadejte **(localdb)\\mssqllocaldb** pro název serveru a  **EnumTest** databáze a klikněte na **OK**</span><span class="sxs-lookup"><span data-stu-id="bc893-162">The Choose Your Data Connection Dialog Box of the Generate Database Wizard is displayed Click the **New Connection** button Specify **(localdb)\\mssqllocaldb** for the server name and **EnumTest** for the database and click **OK**</span></span>
3.  <span data-ttu-id="bc893-163">Dialogové okno s dotazem, zda chcete vytvořit novou databázi se automaticky otevře, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="bc893-163">A dialog asking if you want to create a new database will pop up, click **Yes**.</span></span>
4.  <span data-ttu-id="bc893-164">Klikněte na tlačítko **Další** a Průvodce vytvořením databáze generuje jazyk pro definování dat (DDL) pro vytvoření databáze vygenerovaný DDL se zobrazí v souhrnu a nastavení dialogové okno pole Poznámka, která jazyka DDL neobsahuje definici pro Tabulka, která se mapuje na typ výčtu</span><span class="sxs-lookup"><span data-stu-id="bc893-164">Click **Next** and the Create Database Wizard generates data definition language (DDL) for creating a database The generated DDL is displayed in the Summary and Settings Dialog Box Note, that the DDL does not contain a definition for a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="bc893-165">Klikněte na tlačítko **Dokončit** kliknutím na Dokončit nespustí skriptu jazyka DDL.</span><span class="sxs-lookup"><span data-stu-id="bc893-165">Click **Finish** Clicking Finish does not execute the DDL script.</span></span>
6.  <span data-ttu-id="bc893-166">Průvodce vytvořením databáze provede následující: otevře **EnumTest.edmx.sql** v editoru jazyka T-SQL generuje úložiště schématu a mapování oddílů EDMX soubor přidá připojovacího řetězce do souboru App.config</span><span class="sxs-lookup"><span data-stu-id="bc893-166">The Create Database Wizard does the following: Opens the **EnumTest.edmx.sql** in T-SQL Editor Generates the store schema and mapping sections of the EDMX file Adds connection string information to the App.config file</span></span>
7.  <span data-ttu-id="bc893-167">Klikněte pravým tlačítkem myši v editoru jazyka T-SQL a vyberte **Execute** připojení k serveru dialogové okno zobrazí, zadejte informace o připojení z kroku 2 a klikněte na tlačítko **Connect**</span><span class="sxs-lookup"><span data-stu-id="bc893-167">Click the right mouse button in T-SQL Editor and select **Execute** The Connect to Server dialog appears, enter the connection information from step 2 and click **Connect**</span></span>
8.  <span data-ttu-id="bc893-168">Chcete-li zobrazit generované schéma, klikněte pravým tlačítkem na název databáze v Průzkumníku objektů systému SQL Server a vyberte **aktualizovat**</span><span class="sxs-lookup"><span data-stu-id="bc893-168">To view the generated schema, right-click on the database name in SQL Server Object Explorer and select **Refresh**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="bc893-169">Zachovat a načtení dat</span><span class="sxs-lookup"><span data-stu-id="bc893-169">Persist and Retrieve Data</span></span>

<span data-ttu-id="bc893-170">Otevřete soubor Program.cs, ve kterém je definována metoda Main.</span><span class="sxs-lookup"><span data-stu-id="bc893-170">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="bc893-171">Přidejte následující kód do funkce Main.</span><span class="sxs-lookup"><span data-stu-id="bc893-171">Add the following code into the Main function.</span></span> <span data-ttu-id="bc893-172">Kód přidá nový objekt oddělení do kontextu.</span><span class="sxs-lookup"><span data-stu-id="bc893-172">The code adds a new Department object to the context.</span></span> <span data-ttu-id="bc893-173">Pak uloží data.</span><span class="sxs-lookup"><span data-stu-id="bc893-173">It then saves the data.</span></span> <span data-ttu-id="bc893-174">Kód také spustí dotaz LINQ, který vrátí oddělení, kde název je DepartmentNames.English.</span><span class="sxs-lookup"><span data-stu-id="bc893-174">The code also executes a LINQ query that returns a Department where the name is DepartmentNames.English.</span></span>

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

<span data-ttu-id="bc893-175">Kompilace a spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="bc893-175">Compile and run the application.</span></span> <span data-ttu-id="bc893-176">Program vygeneruje následující výstup:</span><span class="sxs-lookup"><span data-stu-id="bc893-176">The program produces the following output:</span></span>

```
DepartmentID: 1 Name: English
```

<span data-ttu-id="bc893-177">Chcete-li zobrazit data v databázi, klikněte pravým tlačítkem na název databáze v Průzkumníku objektů systému SQL Server a vyberte **aktualizovat**.</span><span class="sxs-lookup"><span data-stu-id="bc893-177">To view data in the database, right-click on the database name in SQL Server Object Explorer and select **Refresh**.</span></span> <span data-ttu-id="bc893-178">Klikněte pravým tlačítkem myši na tabulku a výběrem **Data zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="bc893-178">Then, click the right mouse button on the table and select **View Data**.</span></span>

## <a name="summary"></a><span data-ttu-id="bc893-179">Souhrn</span><span class="sxs-lookup"><span data-stu-id="bc893-179">Summary</span></span>

<span data-ttu-id="bc893-180">V tomto názorném postupu jsme se podívali na tom, jak mapovat typy výčtu pomocí Entity Framework Designer a jak používat výčty v kódu.</span><span class="sxs-lookup"><span data-stu-id="bc893-180">In this walkthrough we looked at how to map enum types using the Entity Framework Designer and how to use enums in code.</span></span> 
