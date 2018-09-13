---
title: Definování dotazu – EF designeru - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e52a297e-85aa-42f6-a922-ba960f8a4b22
ms.openlocfilehash: b1589dc12ccb50754c2e950932a2d82bc4869f6b
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489469"
---
# <a name="defining-query---ef-designer"></a><span data-ttu-id="bd993-102">Definování dotazu – EF designeru</span><span class="sxs-lookup"><span data-stu-id="bd993-102">Defining Query - EF Designer</span></span>
<span data-ttu-id="bd993-103">Tento návod ukazuje, jak přidat, definování dotazů a odpovídající entita typ modelu pomocí EF designeru.</span><span class="sxs-lookup"><span data-stu-id="bd993-103">This walkthrough demonstrates how to add a defining query and a corresponding entity type to a model using the EF Designer.</span></span> <span data-ttu-id="bd993-104">Definování dotazu se běžně používá k zajištění funkce podobné, která poskytuje zobrazení databáze, ale zobrazení je definováno v modelu, ne databáze.</span><span class="sxs-lookup"><span data-stu-id="bd993-104">A defining query is commonly used to provide functionality similar to that provided by a database view, but the view is defined in the model, not the database.</span></span> <span data-ttu-id="bd993-105">Definování dotazu umožňuje provedení příkazu SQL, který je zadán v **DefiningQuery** prvek souboru .edmx.</span><span class="sxs-lookup"><span data-stu-id="bd993-105">A defining query allows you to execute a SQL statement that is specified in the **DefiningQuery** element of an .edmx file.</span></span> <span data-ttu-id="bd993-106">Další informace najdete v tématu **DefiningQuery** v [specifikace SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="bd993-106">For more information, see **DefiningQuery** in the [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span></span>

<span data-ttu-id="bd993-107">Při použití definice dotazů, máte také definovat typ entity v modelu.</span><span class="sxs-lookup"><span data-stu-id="bd993-107">When using defining queries, you also have to define an entity type in your model.</span></span> <span data-ttu-id="bd993-108">Typ entity se používá k poskytování data vystavená prostřednictvím definice dotazu.</span><span class="sxs-lookup"><span data-stu-id="bd993-108">The entity type is used to surface data exposed by the defining query.</span></span> <span data-ttu-id="bd993-109">Všimněte si, že data prezentované prostřednictvím tohoto typu entity je jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="bd993-109">Note that data surfaced through this entity type is read-only.</span></span>

<span data-ttu-id="bd993-110">Parametrizované dotazy nejde provést, protože definice dotazů.</span><span class="sxs-lookup"><span data-stu-id="bd993-110">Parameterized queries cannot be executed as defining queries.</span></span> <span data-ttu-id="bd993-111">Data však můžete aktualizovat pomocí mapování insert, update a delete funkce typ entity tohoto rovin dat pro uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="bd993-111">However, the data can be updated by mapping the insert, update, and delete functions of the entity type that surfaces the data to stored procedures.</span></span> <span data-ttu-id="bd993-112">Další informace najdete v tématu [Insert, Update a Delete pomocí uložené procedury](~/ef6/modeling/designer/stored-procedures/cud.md).</span><span class="sxs-lookup"><span data-stu-id="bd993-112">For more information, see [Insert, Update, and Delete with Stored Procedures](~/ef6/modeling/designer/stored-procedures/cud.md).</span></span>

<span data-ttu-id="bd993-113">Toto téma ukazuje, jak provádět následující úlohy.</span><span class="sxs-lookup"><span data-stu-id="bd993-113">This topic shows how to perform the following tasks.</span></span>

-   <span data-ttu-id="bd993-114">Přidat dotaz definující</span><span class="sxs-lookup"><span data-stu-id="bd993-114">Add a Defining Query</span></span>
-   <span data-ttu-id="bd993-115">Přidat typ Entity do modelu</span><span class="sxs-lookup"><span data-stu-id="bd993-115">Add an Entity Type to the Model</span></span>
-   <span data-ttu-id="bd993-116">Definování dotazu mapovat na typ Entity</span><span class="sxs-lookup"><span data-stu-id="bd993-116">Map the Defining Query to the Entity Type</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bd993-117">Požadavky</span><span class="sxs-lookup"><span data-stu-id="bd993-117">Prerequisites</span></span>

<span data-ttu-id="bd993-118">K dokončení toho návodu budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="bd993-118">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="bd993-119">Nejnovější verzi sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bd993-119">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="bd993-120">[Ukázkové databáze školy](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="bd993-120">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="bd993-121">Nastavení projektu</span><span class="sxs-lookup"><span data-stu-id="bd993-121">Set up the Project</span></span>

<span data-ttu-id="bd993-122">Tento názorný postup je pomocí sady Visual Studio 2012 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="bd993-122">This walkthrough is using Visual Studio 2012 or newer.</span></span>

-   <span data-ttu-id="bd993-123">Otevřít Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bd993-123">Open Visual Studio.</span></span>
-   <span data-ttu-id="bd993-124">Na **souboru** nabídky, přejděte k **nový**a potom klikněte na tlačítko **projektu**.</span><span class="sxs-lookup"><span data-stu-id="bd993-124">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="bd993-125">V levém podokně klikněte na tlačítko **Visual C\#** a pak vyberte **konzolovou aplikaci** šablony.</span><span class="sxs-lookup"><span data-stu-id="bd993-125">In the left pane, click **Visual C\#**, and then select the **Console Application** template.</span></span>
-   <span data-ttu-id="bd993-126">Zadejte **DefiningQuerySample** jako název projektu a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="bd993-126">Enter **DefiningQuerySample** as the name of the project and click **OK**.</span></span>

 

## <a name="create-a-model-based-on-the-school-database"></a><span data-ttu-id="bd993-127">Vytvořit Model založený na databáze školy</span><span class="sxs-lookup"><span data-stu-id="bd993-127">Create a Model based on the School Database</span></span>

-   <span data-ttu-id="bd993-128">Klikněte pravým tlačítkem na název projektu v Průzkumníku řešení, přejděte na **přidat**a potom klikněte na tlačítko **nová položka**.</span><span class="sxs-lookup"><span data-stu-id="bd993-128">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="bd993-129">Vyberte **Data** v levé nabídce a pak vyberte **datový Model Entity ADO.NET** v podokně šablon.</span><span class="sxs-lookup"><span data-stu-id="bd993-129">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="bd993-130">Zadejte **DefiningQueryModel.edmx** pro název souboru a pak klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="bd993-130">Enter **DefiningQueryModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="bd993-131">V dialogovém okně Výběr obsahu modelu vyberte **Generovat z databáze**a potom klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="bd993-131">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="bd993-132">Klikněte na tlačítko nové připojení.</span><span class="sxs-lookup"><span data-stu-id="bd993-132">Click New Connection.</span></span> <span data-ttu-id="bd993-133">V dialogovém okně Vlastnosti připojení zadat název serveru (například **(localdb)\\mssqllocaldb**), vyberte metodu ověřování, zadejte **School** pro název databáze a pak Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="bd993-133">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="bd993-134">Dialogové okno Vybrat datové připojení se aktualizuje se nastavení připojení databáze.</span><span class="sxs-lookup"><span data-stu-id="bd993-134">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="bd993-135">V dialogovém okně Zvolte vaše databázové objekty, zkontrolujte, **tabulky** uzlu.</span><span class="sxs-lookup"><span data-stu-id="bd993-135">In the Choose Your Database Objects dialog box, check the **Tables** node.</span></span> <span data-ttu-id="bd993-136">Tato možnost přidá všech tabulek, které **School** modelu.</span><span class="sxs-lookup"><span data-stu-id="bd993-136">This will add all the tables to the **School** model.</span></span>
-   <span data-ttu-id="bd993-137">Klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="bd993-137">Click **Finish**.</span></span>
-   <span data-ttu-id="bd993-138">V Průzkumníku řešení klikněte pravým tlačítkem myši **DefiningQueryModel.edmx** a vyberte možnost **otevřít v programu...** .</span><span class="sxs-lookup"><span data-stu-id="bd993-138">In Solution Explorer, right-click the **DefiningQueryModel.edmx** file and select **Open With…**.</span></span>
-   <span data-ttu-id="bd993-139">Vyberte **Editor (textový) XML**.</span><span class="sxs-lookup"><span data-stu-id="bd993-139">Select **XML (Text) Editor**.</span></span>

    ![Editor XML](~/ef6/media/xmleditor.png)

-   <span data-ttu-id="bd993-141">Klikněte na tlačítko **Ano** Pokud se zobrazí výzva s následující zprávou:</span><span class="sxs-lookup"><span data-stu-id="bd993-141">Click **Yes** if prompted with the following message:</span></span>

    ![Upozornění 2](~/ef6/media/warning2.png)

 

## <a name="add-a-defining-query"></a><span data-ttu-id="bd993-143">Přidat dotaz definující</span><span class="sxs-lookup"><span data-stu-id="bd993-143">Add a Defining Query</span></span>

<span data-ttu-id="bd993-144">V tomto kroku, který budeme používat editoru XML, který chcete přidat, definování dotazů a zadejte entitu do části SSDL soubor .edmx.</span><span class="sxs-lookup"><span data-stu-id="bd993-144">In this step we will use the XML Editor to add a defining query and an entity type to the SSDL section of the .edmx file.</span></span> 

-   <span data-ttu-id="bd993-145">Přidat **objektu EntitySet** prvek do části SSDL soubor .edmx (řádku 5 až 13).</span><span class="sxs-lookup"><span data-stu-id="bd993-145">Add an **EntitySet** element to the SSDL section of the .edmx file (line 5 thru 13).</span></span> <span data-ttu-id="bd993-146">Zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="bd993-146">Specify the following:</span></span>
    -   <span data-ttu-id="bd993-147">Pouze **název** a **EntityType** atributy **objektu EntitySet** zadávají se element.</span><span class="sxs-lookup"><span data-stu-id="bd993-147">Only the **Name** and **EntityType** attributes of the **EntitySet** element are specified.</span></span>
    -   <span data-ttu-id="bd993-148">Plně kvalifikovaný název typu entity se používá v **EntityType** atribut.</span><span class="sxs-lookup"><span data-stu-id="bd993-148">The fully-qualified name of the entity type is used in the **EntityType** attribute.</span></span>
    -   <span data-ttu-id="bd993-149">Je zadaný příkaz jazyka SQL, který se spustí v **DefiningQuery** elementu.</span><span class="sxs-lookup"><span data-stu-id="bd993-149">The SQL statement to be executed is specified in the **DefiningQuery** element.</span></span>

``` xml
    <!-- SSDL content -->
    <edmx:StorageModels>
      <Schema Namespace="SchoolModel.Store" Alias="Self" Provider="System.Data.SqlClient" ProviderManifestToken="2008" xmlns:store="http://schemas.microsoft.com/ado/2007/12/edm/EntityStoreSchemaGenerator" xmlns="http://schemas.microsoft.com/ado/2009/11/edm/ssdl">
        <EntityContainer Name="SchoolModelStoreContainer">
           <EntitySet Name="GradeReport" EntityType="SchoolModel.Store.GradeReport">
              <DefiningQuery>
                SELECT CourseID, Grade, FirstName, LastName
                FROM StudentGrade
                JOIN
                (SELECT * FROM Person WHERE EnrollmentDate IS NOT NULL) AS p
                ON StudentID = p.PersonID
              </DefiningQuery>
          </EntitySet>
          <EntitySet Name="Course" EntityType="SchoolModel.Store.Course" store:Type="Tables" Schema="dbo" />
```

-   <span data-ttu-id="bd993-150">Přidat **EntityType** prvek do části SSDL edmx.</span><span class="sxs-lookup"><span data-stu-id="bd993-150">Add the **EntityType** element to the SSDL section of the .edmx.</span></span> <span data-ttu-id="bd993-151">soubor jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="bd993-151">file as shown below.</span></span> <span data-ttu-id="bd993-152">Vezměte na vědomí následující:</span><span class="sxs-lookup"><span data-stu-id="bd993-152">Note the following:</span></span>
    -   <span data-ttu-id="bd993-153">Hodnota **název** atribut odpovídá hodnotě **EntityType** atribut **objektu EntitySet** element výše, i když plně kvalifikovaný název Typ entity se používá v **EntityType** atribut.</span><span class="sxs-lookup"><span data-stu-id="bd993-153">The value of the **Name** attribute corresponds to the value of the **EntityType** attribute in the **EntitySet** element above, although the fully-qualified name of the entity type is used in the **EntityType** attribute.</span></span>
    -   <span data-ttu-id="bd993-154">Názvy vlastností, které odpovídají názvům sloupce vrácený příkazem SQL v **DefiningQuery** – element (viz výše).</span><span class="sxs-lookup"><span data-stu-id="bd993-154">The property names correspond to the column names returned by the SQL statement in the **DefiningQuery** element (above).</span></span>
    -   <span data-ttu-id="bd993-155">V tomto příkladu klíč entity tvoří tři vlastnosti zajistit jedinečnou hodnotu klíče.</span><span class="sxs-lookup"><span data-stu-id="bd993-155">In this example, the entity key is composed of three properties to ensure a unique key value.</span></span>

``` xml
    <EntityType Name="GradeReport">
      <Key>
        <PropertyRef Name="CourseID" />
        <PropertyRef Name="FirstName" />
        <PropertyRef Name="LastName" />
      </Key>
      <Property Name="CourseID"
                Type="int"
                Nullable="false" />
      <Property Name="Grade"
                Type="decimal"
                Precision="3"
                Scale="2" />
      <Property Name="FirstName"
                Type="nvarchar"
                Nullable="false"
                MaxLength="50" />
      <Property Name="LastName"
                Type="nvarchar"
                Nullable="false"
                MaxLength="50" />
    </EntityType>
```

>[!NOTE]
> <span data-ttu-id="bd993-156">Pokud později spustíte **aktualizace modelu průvodce** dialogového okna, všechny změny provedené v modelu úložiště, včetně definice dotazů, bude přepsán.</span><span class="sxs-lookup"><span data-stu-id="bd993-156">If later you run the **Update Model Wizard** dialog, any changes made to the storage model, including defining queries, will be overwritten.</span></span>

 

## <a name="add-an-entity-type-to-the-model"></a><span data-ttu-id="bd993-157">Přidat typ Entity do modelu</span><span class="sxs-lookup"><span data-stu-id="bd993-157">Add an Entity Type to the Model</span></span>

<span data-ttu-id="bd993-158">V tomto kroku přidáme typ entity pro koncepční model pomocí EF designeru.</span><span class="sxs-lookup"><span data-stu-id="bd993-158">In this step we will add the entity type to the conceptual model using the EF Designer.</span></span>  <span data-ttu-id="bd993-159">Vezměte na vědomí následující:</span><span class="sxs-lookup"><span data-stu-id="bd993-159">Note the following:</span></span>

-   <span data-ttu-id="bd993-160">**Název** entity odpovídá hodnotě **EntityType** atribut **objektu EntitySet** element výše.</span><span class="sxs-lookup"><span data-stu-id="bd993-160">The **Name** of the entity corresponds to the value of the **EntityType** attribute in the **EntitySet** element above.</span></span>
-   <span data-ttu-id="bd993-161">Názvy vlastností, které odpovídají názvům sloupce vrácený příkazem SQL v **DefiningQuery** element výše.</span><span class="sxs-lookup"><span data-stu-id="bd993-161">The property names correspond to the column names returned by the SQL statement in the **DefiningQuery** element above.</span></span>
-   <span data-ttu-id="bd993-162">V tomto příkladu klíč entity tvoří tři vlastnosti zajistit jedinečnou hodnotu klíče.</span><span class="sxs-lookup"><span data-stu-id="bd993-162">In this example, the entity key is composed of three properties to ensure a unique key value.</span></span>

<span data-ttu-id="bd993-163">Otevřete model v EF designeru.</span><span class="sxs-lookup"><span data-stu-id="bd993-163">Open the model in the EF Designer.</span></span>

-   <span data-ttu-id="bd993-164">Dvakrát klikněte DefiningQueryModel.edmx.</span><span class="sxs-lookup"><span data-stu-id="bd993-164">Double-click the DefiningQueryModel.edmx.</span></span>
-   <span data-ttu-id="bd993-165">Řekněme, že **Ano** následující zprávu:</span><span class="sxs-lookup"><span data-stu-id="bd993-165">Say **Yes** to the following message:</span></span>

    ![Upozornění 2](~/ef6/media/warning2.png)

 

<span data-ttu-id="bd993-167">V návrháři entit, které poskytuje návrhové ploše pro úpravy váš model, se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="bd993-167">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

-   <span data-ttu-id="bd993-168">Klikněte pravým tlačítkem na Návrhář ploše a vyberte možnost **přidat nový**-&gt;**Entity...** .</span><span class="sxs-lookup"><span data-stu-id="bd993-168">Right-click the designer surface and select **Add New**-&gt;**Entity…**.</span></span>
-   <span data-ttu-id="bd993-169">Zadejte **GradeReport** u názvu entity a **CourseID** pro **vlastnost Key**.</span><span class="sxs-lookup"><span data-stu-id="bd993-169">Specify **GradeReport** for the entity name and **CourseID** for the **Key Property**.</span></span>
-   <span data-ttu-id="bd993-170">Klikněte pravým tlačítkem myši **GradeReport** entity a vyberte **přidat nový** - &gt; **skalární vlastnost**.</span><span class="sxs-lookup"><span data-stu-id="bd993-170">Right-click the **GradeReport** entity and select **Add New**-&gt; **Scalar Property**.</span></span>
-   <span data-ttu-id="bd993-171">Změnit výchozí název vlastnosti, která má **FirstName**.</span><span class="sxs-lookup"><span data-stu-id="bd993-171">Change the default name of the property to **FirstName**.</span></span>
-   <span data-ttu-id="bd993-172">Přidat jiné Skalární vlastnosti a určit **LastName** pro název.</span><span class="sxs-lookup"><span data-stu-id="bd993-172">Add another scalar property and specify **LastName** for the name.</span></span>
-   <span data-ttu-id="bd993-173">Přidat jiné Skalární vlastnosti a určit **na podnikové úrovni** pro název.</span><span class="sxs-lookup"><span data-stu-id="bd993-173">Add another scalar property and specify **Grade** for the name.</span></span>
-   <span data-ttu-id="bd993-174">V **vlastnosti** okno Změnit **na podnikové úrovni**společnosti **typ** vlastnost **desítkové**.</span><span class="sxs-lookup"><span data-stu-id="bd993-174">In the **Properties** window, change the **Grade**’s **Type** property to **Decimal**.</span></span>
-   <span data-ttu-id="bd993-175">Vyberte **FirstName** a **LastName** vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="bd993-175">Select the **FirstName** and **LastName** properties.</span></span>
-   <span data-ttu-id="bd993-176">V **vlastnosti** okno Změnit **EntityKey** hodnoty vlastnosti **True**.</span><span class="sxs-lookup"><span data-stu-id="bd993-176">In the **Properties** window, change the **EntityKey** property value to **True**.</span></span>

<span data-ttu-id="bd993-177">V důsledku toho byly přidány následující prvky do **CSDL** část souboru .edmx.</span><span class="sxs-lookup"><span data-stu-id="bd993-177">As a result, the following elements were added to the **CSDL** section of the .edmx file.</span></span>

``` xml
    <EntitySet Name="GradeReport" EntityType="SchoolModel.GradeReport" />

    <EntityType Name="GradeReport">
    . . .
    </EntityType>
```

 

## <a name="map-the-defining-query-to-the-entity-type"></a><span data-ttu-id="bd993-178">Definování dotazu mapovat na typ Entity</span><span class="sxs-lookup"><span data-stu-id="bd993-178">Map the Defining Query to the Entity Type</span></span>

<span data-ttu-id="bd993-179">V tomto kroku použijeme okno Podrobnosti o mapování pro mapování koncepční a typy entit úložiště.</span><span class="sxs-lookup"><span data-stu-id="bd993-179">In this step, we will use the Mapping Details window to map the conceptual and storage entity types.</span></span>

-   <span data-ttu-id="bd993-180">Klikněte pravým tlačítkem myši **GradeReport** entity na návrhové ploše a vyberte **mapování tabulky,**.</span><span class="sxs-lookup"><span data-stu-id="bd993-180">Right-click the **GradeReport** entity on the design surface and select **Table Mapping**.</span></span>  
    <span data-ttu-id="bd993-181">**Podrobnosti mapování** se zobrazí okno.</span><span class="sxs-lookup"><span data-stu-id="bd993-181">The **Mapping Details** window is displayed.</span></span>
-   <span data-ttu-id="bd993-182">Vyberte **GradeReport** z **&lt;přidat tabulku nebo zobrazení&gt;** rozevíracího seznamu (umístěný ve skupinovém rámečku **tabulky**s).</span><span class="sxs-lookup"><span data-stu-id="bd993-182">Select **GradeReport** from the **&lt;Add a Table or View&gt;** dropdown list (located under **Table**s).</span></span>  
    <span data-ttu-id="bd993-183">Výchozí mapování mezi koncepční a úložiště **GradeReport** zobrazí typ entity.</span><span class="sxs-lookup"><span data-stu-id="bd993-183">Default mappings between the conceptual and storage **GradeReport** entity type appear.</span></span>  
    <span data-ttu-id="bd993-184">![Mapování Details3](~/ef6/media/mappingdetails.png)</span><span class="sxs-lookup"><span data-stu-id="bd993-184">![Mapping Details3](~/ef6/media/mappingdetails.png)</span></span>

<span data-ttu-id="bd993-185">V důsledku toho **elementu EntitySetMapping** prvek se přidá do části mapování souboru .edmx.</span><span class="sxs-lookup"><span data-stu-id="bd993-185">As a result, the **EntitySetMapping** element is added to the mapping section of the .edmx file.</span></span> 

``` xml
    <EntitySetMapping Name="GradeReports">
      <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.GradeReport)">
        <MappingFragment StoreEntitySet="GradeReport">
          <ScalarProperty Name="LastName" ColumnName="LastName" />
          <ScalarProperty Name="FirstName" ColumnName="FirstName" />
          <ScalarProperty Name="Grade" ColumnName="Grade" />
          <ScalarProperty Name="CourseID" ColumnName="CourseID" />
        </MappingFragment>
      </EntityTypeMapping>
    </EntitySetMapping>
```

-   <span data-ttu-id="bd993-186">Zkompilujte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bd993-186">Compile the application.</span></span>

 

## <a name="call-the-defining-query-in-your-code"></a><span data-ttu-id="bd993-187">Definování dotazu volání v kódu</span><span class="sxs-lookup"><span data-stu-id="bd993-187">Call the Defining Query in your Code</span></span>

<span data-ttu-id="bd993-188">Definování dotazu teď můžete spustit pomocí **GradeReport** typu entity.</span><span class="sxs-lookup"><span data-stu-id="bd993-188">You can now execute the defining query by using the **GradeReport** entity type.</span></span> 

``` csharp
    using (var context = new SchoolEntities())
    {
        var report = context.GradeReports.FirstOrDefault();
        Console.WriteLine("{0} {1} got {2}",
            report.FirstName, report.LastName, report.Grade);
    }
```
