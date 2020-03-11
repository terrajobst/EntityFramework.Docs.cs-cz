---
title: Definování dotazu-EF Designer – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e52a297e-85aa-42f6-a922-ba960f8a4b22
ms.openlocfilehash: b1589dc12ccb50754c2e950932a2d82bc4869f6b
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418796"
---
# <a name="defining-query---ef-designer"></a><span data-ttu-id="ed7e0-102">Definice dotazu – Návrhář EF</span><span class="sxs-lookup"><span data-stu-id="ed7e0-102">Defining Query - EF Designer</span></span>
<span data-ttu-id="ed7e0-103">Tento návod ukazuje, jak přidat definiční dotaz a odpovídající typ entity do modelu pomocí návrháře EF.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-103">This walkthrough demonstrates how to add a defining query and a corresponding entity type to a model using the EF Designer.</span></span> <span data-ttu-id="ed7e0-104">Definiční dotaz se běžně používá k poskytování funkcí podobných těm, které poskytuje zobrazení databáze, ale zobrazení je definováno v modelu, nikoli v databázi.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-104">A defining query is commonly used to provide functionality similar to that provided by a database view, but the view is defined in the model, not the database.</span></span> <span data-ttu-id="ed7e0-105">Definiční dotaz umožňuje spustit příkaz SQL, který je určen v **DefiningQuery** elementu souboru. edmx.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-105">A defining query allows you to execute a SQL statement that is specified in the **DefiningQuery** element of an .edmx file.</span></span> <span data-ttu-id="ed7e0-106">Další informace najdete v tématu **DefiningQuery** ve [specifikaci SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="ed7e0-106">For more information, see **DefiningQuery** in the [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span></span>

<span data-ttu-id="ed7e0-107">Při použití definování dotazů musíte také definovat typ entity v modelu.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-107">When using defining queries, you also have to define an entity type in your model.</span></span> <span data-ttu-id="ed7e0-108">Typ entity se používá k vytvoření Surface dat vystavených definičním dotazem.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-108">The entity type is used to surface data exposed by the defining query.</span></span> <span data-ttu-id="ed7e0-109">Všimněte si, že data provedená prostřednictvím tohoto typu entity jsou jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-109">Note that data surfaced through this entity type is read-only.</span></span>

<span data-ttu-id="ed7e0-110">Parametrizované dotazy nelze provést jako definování dotazů.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-110">Parameterized queries cannot be executed as defining queries.</span></span> <span data-ttu-id="ed7e0-111">Data se ale dají aktualizovat mapováním funkcí INSERT, Update a DELETE typu entity, která doplňují data do uložených procedur.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-111">However, the data can be updated by mapping the insert, update, and delete functions of the entity type that surfaces the data to stored procedures.</span></span> <span data-ttu-id="ed7e0-112">Další informace najdete v tématu [vkládání, aktualizace a odstraňování s uloženými procedurami](~/ef6/modeling/designer/stored-procedures/cud.md).</span><span class="sxs-lookup"><span data-stu-id="ed7e0-112">For more information, see [Insert, Update, and Delete with Stored Procedures](~/ef6/modeling/designer/stored-procedures/cud.md).</span></span>

<span data-ttu-id="ed7e0-113">V tomto tématu se dozvíte, jak provádět následující úlohy.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-113">This topic shows how to perform the following tasks.</span></span>

-   <span data-ttu-id="ed7e0-114">Přidat definiční dotaz</span><span class="sxs-lookup"><span data-stu-id="ed7e0-114">Add a Defining Query</span></span>
-   <span data-ttu-id="ed7e0-115">Přidání typu entity do modelu</span><span class="sxs-lookup"><span data-stu-id="ed7e0-115">Add an Entity Type to the Model</span></span>
-   <span data-ttu-id="ed7e0-116">Namapujte definiční dotaz na typ entity.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-116">Map the Defining Query to the Entity Type</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ed7e0-117">Předpoklady</span><span class="sxs-lookup"><span data-stu-id="ed7e0-117">Prerequisites</span></span>

<span data-ttu-id="ed7e0-118">K dokončení toho návodu budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="ed7e0-118">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="ed7e0-119">Poslední verze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-119">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="ed7e0-120">[Ukázková databáze školy](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="ed7e0-120">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="ed7e0-121">Nastavení projektu</span><span class="sxs-lookup"><span data-stu-id="ed7e0-121">Set up the Project</span></span>

<span data-ttu-id="ed7e0-122">Tento návod používá aplikaci Visual Studio 2012 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-122">This walkthrough is using Visual Studio 2012 or newer.</span></span>

-   <span data-ttu-id="ed7e0-123">Otevřete sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-123">Open Visual Studio.</span></span>
-   <span data-ttu-id="ed7e0-124">V nabídce **soubor** přejděte na příkaz **Nový**a klikněte na **projekt**.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-124">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="ed7e0-125">V levém podokně klikněte na položku **Visual C\#** a pak vyberte šablonu **Konzolová aplikace** .</span><span class="sxs-lookup"><span data-stu-id="ed7e0-125">In the left pane, click **Visual C\#**, and then select the **Console Application** template.</span></span>
-   <span data-ttu-id="ed7e0-126">Jako název projektu zadejte **DefiningQuerySample** a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-126">Enter **DefiningQuerySample** as the name of the project and click **OK**.</span></span>

 

## <a name="create-a-model-based-on-the-school-database"></a><span data-ttu-id="ed7e0-127">Vytvoření modelu založeného na školní databázi</span><span class="sxs-lookup"><span data-stu-id="ed7e0-127">Create a Model based on the School Database</span></span>

-   <span data-ttu-id="ed7e0-128">Klikněte pravým tlačítkem myši na název projektu v Průzkumník řešení, přejděte na **Přidat**a klikněte na **Nová položka**.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-128">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="ed7e0-129">V nabídce vlevo vyberte **data** a v podokně šablony vyberte **ADO.NET model EDM (Entity Data Model)** .</span><span class="sxs-lookup"><span data-stu-id="ed7e0-129">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="ed7e0-130">Jako název souboru zadejte **DefiningQueryModel. edmx** a pak klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-130">Enter **DefiningQueryModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="ed7e0-131">V dialogovém okně Vybrat obsah modelu vyberte možnost **Generovat z databáze**a poté klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-131">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="ed7e0-132">Klikněte na nové připojení.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-132">Click New Connection.</span></span> <span data-ttu-id="ed7e0-133">V dialogovém okně Vlastnosti připojení zadejte název serveru (například **(LocalDB)\\mssqllocaldb**), vyberte metodu ověřování, jako název databáze zadejte **School** a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-133">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="ed7e0-134">Dialogové okno zvolit datové připojení je aktualizováno nastavením připojení k databázi.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-134">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="ed7e0-135">V dialogovém okně zvolit objekty databáze zaškrtněte uzel **tabulky** .</span><span class="sxs-lookup"><span data-stu-id="ed7e0-135">In the Choose Your Database Objects dialog box, check the **Tables** node.</span></span> <span data-ttu-id="ed7e0-136">Tím se do **školního** modelu přidá všechny tabulky.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-136">This will add all the tables to the **School** model.</span></span>
-   <span data-ttu-id="ed7e0-137">Klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-137">Click **Finish**.</span></span>
-   <span data-ttu-id="ed7e0-138">V Průzkumník řešení klikněte pravým tlačítkem na soubor **DefiningQueryModel. edmx** a vyberte **otevřít s...** .</span><span class="sxs-lookup"><span data-stu-id="ed7e0-138">In Solution Explorer, right-click the **DefiningQueryModel.edmx** file and select **Open With…**.</span></span>
-   <span data-ttu-id="ed7e0-139">Vyberte **Editor XML (text)** .</span><span class="sxs-lookup"><span data-stu-id="ed7e0-139">Select **XML (Text) Editor**.</span></span>

    ![Editor XML](~/ef6/media/xmleditor.png)

-   <span data-ttu-id="ed7e0-141">Po zobrazení výzvy s následující zprávou klikněte na **Ano** :</span><span class="sxs-lookup"><span data-stu-id="ed7e0-141">Click **Yes** if prompted with the following message:</span></span>

    ![Upozornění 2](~/ef6/media/warning2.png)

 

## <a name="add-a-defining-query"></a><span data-ttu-id="ed7e0-143">Přidat definiční dotaz</span><span class="sxs-lookup"><span data-stu-id="ed7e0-143">Add a Defining Query</span></span>

<span data-ttu-id="ed7e0-144">V tomto kroku použijeme editor XML k přidání definičního dotazu a typu entity do oddílu SSDL souboru. edmx.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-144">In this step we will use the XML Editor to add a defining query and an entity type to the SSDL section of the .edmx file.</span></span> 

-   <span data-ttu-id="ed7e0-145">Přidejte element **EntitySet** do oddílu SSDL souboru. edmx (řádek 5 až 13).</span><span class="sxs-lookup"><span data-stu-id="ed7e0-145">Add an **EntitySet** element to the SSDL section of the .edmx file (line 5 thru 13).</span></span> <span data-ttu-id="ed7e0-146">Určete následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="ed7e0-146">Specify the following:</span></span>
    -   <span data-ttu-id="ed7e0-147">Jsou zadány pouze atributy **Name** a **EntityType** elementu **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="ed7e0-147">Only the **Name** and **EntityType** attributes of the **EntitySet** element are specified.</span></span>
    -   <span data-ttu-id="ed7e0-148">Plně kvalifikovaný název typu entity se používá v atributu **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="ed7e0-148">The fully-qualified name of the entity type is used in the **EntityType** attribute.</span></span>
    -   <span data-ttu-id="ed7e0-149">Příkaz jazyka SQL, který má být spuštěn, je určen v prvku  **DefiningQuery** .</span><span class="sxs-lookup"><span data-stu-id="ed7e0-149">The SQL statement to be executed is specified in the **DefiningQuery** element.</span></span>

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

-   <span data-ttu-id="ed7e0-150">Přidejte element **EntityType** do oddílu SSDL v. edmx.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-150">Add the **EntityType** element to the SSDL section of the .edmx.</span></span> <span data-ttu-id="ed7e0-151">souboru, jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-151">file as shown below.</span></span> <span data-ttu-id="ed7e0-152">Je třeba počítat s následujícím:</span><span class="sxs-lookup"><span data-stu-id="ed7e0-152">Note the following:</span></span>
    -   <span data-ttu-id="ed7e0-153">Hodnota atributu **Name** odpovídá hodnotě atributu **EntityType** v elementu **EntitySet** výše, i když je plně kvalifikovaný název typu entity použit v atributu **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="ed7e0-153">The value of the **Name** attribute corresponds to the value of the **EntityType** attribute in the **EntitySet** element above, although the fully-qualified name of the entity type is used in the **EntityType** attribute.</span></span>
    -   <span data-ttu-id="ed7e0-154">Názvy vlastností odpovídají názvům sloupců vráceným příkazem SQL v elementu **DefiningQuery** (výše).</span><span class="sxs-lookup"><span data-stu-id="ed7e0-154">The property names correspond to the column names returned by the SQL statement in the **DefiningQuery** element (above).</span></span>
    -   <span data-ttu-id="ed7e0-155">V tomto příkladu se klíč entity skládá ze tří vlastností, aby se zajistila hodnota jedinečného klíče.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-155">In this example, the entity key is composed of three properties to ensure a unique key value.</span></span>

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
> <span data-ttu-id="ed7e0-156">Pokud budete později spouštět dialog **Průvodce aktualizací modelu** , budou přepsány všechny změny modelu úložiště, včetně definování dotazů.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-156">If later you run the **Update Model Wizard** dialog, any changes made to the storage model, including defining queries, will be overwritten.</span></span>

 

## <a name="add-an-entity-type-to-the-model"></a><span data-ttu-id="ed7e0-157">Přidání typu entity do modelu</span><span class="sxs-lookup"><span data-stu-id="ed7e0-157">Add an Entity Type to the Model</span></span>

<span data-ttu-id="ed7e0-158">V tomto kroku přidáme typ entity do koncepčního modelu pomocí návrháře EF.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-158">In this step we will add the entity type to the conceptual model using the EF Designer.</span></span> <span data-ttu-id="ed7e0-159"> Vezměte na vědomí následující:</span><span class="sxs-lookup"><span data-stu-id="ed7e0-159"> Note the following:</span></span>

-   <span data-ttu-id="ed7e0-160">**Název** entity odpovídá hodnotě atributu **EntityType** v elementu **EntitySet** výše.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-160">The **Name** of the entity corresponds to the value of the **EntityType** attribute in the **EntitySet** element above.</span></span>
-   <span data-ttu-id="ed7e0-161">Názvy vlastností odpovídají názvům sloupců vráceným příkazem SQL ve výše uvedeném elementu **DefiningQuery** .</span><span class="sxs-lookup"><span data-stu-id="ed7e0-161">The property names correspond to the column names returned by the SQL statement in the **DefiningQuery** element above.</span></span>
-   <span data-ttu-id="ed7e0-162">V tomto příkladu se klíč entity skládá ze tří vlastností, aby se zajistila hodnota jedinečného klíče.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-162">In this example, the entity key is composed of three properties to ensure a unique key value.</span></span>

<span data-ttu-id="ed7e0-163">Otevřete model v Návrháři EF.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-163">Open the model in the EF Designer.</span></span>

-   <span data-ttu-id="ed7e0-164">Dvakrát klikněte na DefiningQueryModel. edmx.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-164">Double-click the DefiningQueryModel.edmx.</span></span>
-   <span data-ttu-id="ed7e0-165">Řekněte **Ano** pro následující zprávu:</span><span class="sxs-lookup"><span data-stu-id="ed7e0-165">Say **Yes** to the following message:</span></span>

    ![Upozornění 2](~/ef6/media/warning2.png)

 

<span data-ttu-id="ed7e0-167">Zobrazí se Entity Designer, která poskytuje návrhovou plochu pro úpravu vašeho modelu.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-167">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

-   <span data-ttu-id="ed7e0-168">Klikněte pravým tlačítkem myši na plochu návrháře a vyberte **Přidat novou**-&gt;**entitu...** .</span><span class="sxs-lookup"><span data-stu-id="ed7e0-168">Right-click the designer surface and select **Add New**-&gt;**Entity…**.</span></span>
-   <span data-ttu-id="ed7e0-169">Zadejte **GradeReport** pro název entity a **CourseID** pro **klíčovou vlastnost**.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-169">Specify **GradeReport** for the entity name and **CourseID** for the **Key Property**.</span></span>
-   <span data-ttu-id="ed7e0-170">Klikněte pravým tlačítkem na entitu **GradeReport** a vyberte **přidat novou**-&gt; **skalární vlastnost**.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-170">Right-click the **GradeReport** entity and select **Add New**-&gt; **Scalar Property**.</span></span>
-   <span data-ttu-id="ed7e0-171">Změňte výchozí název vlastnosti na **FirstName**.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-171">Change the default name of the property to **FirstName**.</span></span>
-   <span data-ttu-id="ed7e0-172">Přidejte další skalární vlastnost a jako název zadejte **LastName** .</span><span class="sxs-lookup"><span data-stu-id="ed7e0-172">Add another scalar property and specify **LastName** for the name.</span></span>
-   <span data-ttu-id="ed7e0-173">Přidejte další skalární vlastnost a jako název zadejte **třídu** .</span><span class="sxs-lookup"><span data-stu-id="ed7e0-173">Add another scalar property and specify **Grade** for the name.</span></span>
-   <span data-ttu-id="ed7e0-174">V okně **vlastnosti** změňte vlastnost **typ** **třídy**na hodnotu **Decimal**.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-174">In the **Properties** window, change the **Grade**’s **Type** property to **Decimal**.</span></span>
-   <span data-ttu-id="ed7e0-175">Vyberte vlastnosti **FirstName** a **LastName** .</span><span class="sxs-lookup"><span data-stu-id="ed7e0-175">Select the **FirstName** and **LastName** properties.</span></span>
-   <span data-ttu-id="ed7e0-176">V okně **vlastnosti** změňte hodnotu vlastnosti **EntityKey** na hodnotu **true**.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-176">In the **Properties** window, change the **EntityKey** property value to **True**.</span></span>

<span data-ttu-id="ed7e0-177">V důsledku toho byly do oddílu **CSDL** souboru. edmx přidány následující prvky.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-177">As a result, the following elements were added to the **CSDL** section of the .edmx file.</span></span>

``` xml
    <EntitySet Name="GradeReport" EntityType="SchoolModel.GradeReport" />

    <EntityType Name="GradeReport">
    . . .
    </EntityType>
```

 

## <a name="map-the-defining-query-to-the-entity-type"></a><span data-ttu-id="ed7e0-178">Namapujte definiční dotaz na typ entity.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-178">Map the Defining Query to the Entity Type</span></span>

<span data-ttu-id="ed7e0-179">V tomto kroku použijeme okno Podrobnosti mapování k mapování typů koncepčních a entit úložišť.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-179">In this step, we will use the Mapping Details window to map the conceptual and storage entity types.</span></span>

-   <span data-ttu-id="ed7e0-180">Na návrhové ploše klikněte pravým tlačítkem na entitu **GradeReport** a vyberte **mapování tabulky**.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-180">Right-click the **GradeReport** entity on the design surface and select **Table Mapping**.</span></span>  
    <span data-ttu-id="ed7e0-181">Zobrazí se okno **Podrobnosti mapování** .</span><span class="sxs-lookup"><span data-stu-id="ed7e0-181">The **Mapping Details** window is displayed.</span></span>
-   <span data-ttu-id="ed7e0-182">Vyberte **GradeReport** z **&lt;přidat tabulku nebo zobrazení&gt;** rozevírací seznam (umístěný v **tabulce**s).</span><span class="sxs-lookup"><span data-stu-id="ed7e0-182">Select **GradeReport** from the **&lt;Add a Table or View&gt;** dropdown list (located under **Table**s).</span></span>  
    <span data-ttu-id="ed7e0-183">Zobrazí se výchozí mapování mezi typem entity koncepčního a **GradeReportho** úložiště.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-183">Default mappings between the conceptual and storage **GradeReport** entity type appear.</span></span>  
    <span data-ttu-id="ed7e0-184">Mapování ![Details3](~/ef6/media/mappingdetails.png)</span><span class="sxs-lookup"><span data-stu-id="ed7e0-184">![Mapping Details3](~/ef6/media/mappingdetails.png)</span></span>

<span data-ttu-id="ed7e0-185">V důsledku toho se prvek **EntitySetMapping** přidá do oddílu mapování v souboru. edmx.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-185">As a result, the **EntitySetMapping** element is added to the mapping section of the .edmx file.</span></span> 

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

-   <span data-ttu-id="ed7e0-186">Zkompilujte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ed7e0-186">Compile the application.</span></span>

 

## <a name="call-the-defining-query-in-your-code"></a><span data-ttu-id="ed7e0-187">Volání definičního dotazu ve vašem kódu</span><span class="sxs-lookup"><span data-stu-id="ed7e0-187">Call the Defining Query in your Code</span></span>

<span data-ttu-id="ed7e0-188">Nyní můžete spustit definiční dotaz pomocí typu entity **GradeReport** .</span><span class="sxs-lookup"><span data-stu-id="ed7e0-188">You can now execute the defining query by using the **GradeReport** entity type.</span></span> 

``` csharp
    using (var context = new SchoolEntities())
    {
        var report = context.GradeReports.FirstOrDefault();
        Console.WriteLine("{0} {1} got {2}",
            report.FirstName, report.LastName, report.Grade);
    }
```
