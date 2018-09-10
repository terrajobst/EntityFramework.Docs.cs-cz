---
title: Komplexní typy – EF designeru - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 9a8228ef-acfd-4575-860d-769d2c0e18a1
ms.openlocfilehash: 2a516bd14131fd035a4d005e0fdf140f7ff4d65f
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250826"
---
# <a name="complex-types---ef-designer"></a><span data-ttu-id="18f84-102">Komplexní typy – EF designeru</span><span class="sxs-lookup"><span data-stu-id="18f84-102">Complex Types - EF Designer</span></span>
<span data-ttu-id="18f84-103">Toto téma ukazuje, jak namapovat komplexní typy se Návrhář Entity Framework (EF designeru) a zadávat dotazy na entity, které obsahují vlastností komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="18f84-103">This topic shows how to map complex types with the Entity Framework Designer (EF Designer) and how to query for entities that contain properties of complex type.</span></span>

<span data-ttu-id="18f84-104">Následující obrázek znázorňuje hlavní windows, které se používají při práci s EF designeru.</span><span class="sxs-lookup"><span data-stu-id="18f84-104">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EF designeru](~/ef6/media/efdesigner.png)

> [!NOTE]
> <span data-ttu-id="18f84-106">Při vytváření konceptuální model, upozornění na nenamapované entit a přidružení může zobrazit v seznamu chyb.</span><span class="sxs-lookup"><span data-stu-id="18f84-106">When you build the conceptual model, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="18f84-107">Tato upozornění můžete ignorovat, protože až zvolíte, aby se vygenerovala databáze z modelu, chyby zmizí.</span><span class="sxs-lookup"><span data-stu-id="18f84-107">You can ignore these warnings because after you choose to generate the database from the model, the errors will go away.</span></span>

## <a name="what-is-a-complex-type"></a><span data-ttu-id="18f84-108">Co je komplexní typ</span><span class="sxs-lookup"><span data-stu-id="18f84-108">What is a Complex Type</span></span>

<span data-ttu-id="18f84-109">Komplexní typy jsou vlastnosti neskalární typy entit, které umožňují Skalární vlastnosti, které chcete být uspořádány v rámci entit.</span><span class="sxs-lookup"><span data-stu-id="18f84-109">Complex types are non-scalar properties of entity types that enable scalar properties to be organized within entities.</span></span> <span data-ttu-id="18f84-110">Stejně jako entity komplexní typy skládat z Skalární vlastnosti nebo dalších vlastností komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="18f84-110">Like entities, complex types consist of scalar properties or other complex type properties.</span></span>

<span data-ttu-id="18f84-111">Při práci s objekty, které představují komplexní typy, mějte na paměti toto:</span><span class="sxs-lookup"><span data-stu-id="18f84-111">When you work with objects that represent complex types, be aware of the following:</span></span>

-   <span data-ttu-id="18f84-112">Komplexní typy nemají klíče a proto nemůže existovat nezávisle na sobě.</span><span class="sxs-lookup"><span data-stu-id="18f84-112">Complex types do not have keys and therefore cannot exist independently.</span></span> <span data-ttu-id="18f84-113">Komplexní typy může existovat pouze jako typy entit nebo jiné komplexní typy vlastností.</span><span class="sxs-lookup"><span data-stu-id="18f84-113">Complex types can only exist as properties of entity types or other complex types.</span></span>
-   <span data-ttu-id="18f84-114">Komplexní typy nemůže být součástí přidružení a nesmí obsahovat navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="18f84-114">Complex types cannot participate in associations and cannot contain navigation properties.</span></span>
-   <span data-ttu-id="18f84-115">Komplexní typ vlastnosti nemohou být **null**.</span><span class="sxs-lookup"><span data-stu-id="18f84-115">Complex type properties cannot be **null**.</span></span> <span data-ttu-id="18f84-116">\*\* InvalidOperationException \*\* dochází při **DbContext.SaveChanges** se nazývá a komplexní objekt s hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="18f84-116">An \*\*InvalidOperationException \*\*occurs when **DbContext.SaveChanges** is called and a null complex object is encountered.</span></span> <span data-ttu-id="18f84-117">Může být Skalární vlastnosti komplexních objektů **null**.</span><span class="sxs-lookup"><span data-stu-id="18f84-117">Scalar properties of complex objects can be **null**.</span></span>
-   <span data-ttu-id="18f84-118">Komplexní typy nemůžou dědit z jiné komplexní typy.</span><span class="sxs-lookup"><span data-stu-id="18f84-118">Complex types cannot inherit from other complex types.</span></span>
-   <span data-ttu-id="18f84-119">Je nutné definovat jako komplexní typ **třídy**.</span><span class="sxs-lookup"><span data-stu-id="18f84-119">You must define the complex type as a **class**.</span></span> 
-   <span data-ttu-id="18f84-120">EF zjistí změny členů na komplexní typ objektu při **DbContext.DetectChanges** je volána.</span><span class="sxs-lookup"><span data-stu-id="18f84-120">EF detects changes to members on a complex type object when **DbContext.DetectChanges** is called.</span></span> <span data-ttu-id="18f84-121">Zavolá rozhraní Entity Framework **metoda DetectChanges** automaticky kdy jsou volány následující členy: **DbSet.Find**, **DbSet.Local**, **DbSet.Remove**, **DbSet.Add**, **DbSet.Attach**, **DbContext.SaveChanges**, **DbContext.GetValidationErrors**, **DbContext.Entry**, **DbChangeTracker.Entries**.</span><span class="sxs-lookup"><span data-stu-id="18f84-121">Entity Framework calls **DetectChanges** automatically when the following members are called: **DbSet.Find**, **DbSet.Local**, **DbSet.Remove**, **DbSet.Add**, **DbSet.Attach**, **DbContext.SaveChanges**, **DbContext.GetValidationErrors**, **DbContext.Entry**, **DbChangeTracker.Entries**.</span></span>

## <a name="refactor-an-entitys-properties-into-new-complex-type"></a><span data-ttu-id="18f84-122">Refaktorovat vlastnosti Entity novou komplexní typ</span><span class="sxs-lookup"><span data-stu-id="18f84-122">Refactor an Entity’s Properties into New Complex Type</span></span>

<span data-ttu-id="18f84-123">Pokud už máte v konceptuálním modelu entity můžete chtít Refaktorovat některých vlastností do vlastností komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="18f84-123">If you already have an entity in your conceptual model you may want to refactor some of the properties into a complex type property.</span></span>

<span data-ttu-id="18f84-124">Na návrhové ploše, vyberte jednu nebo více vlastností (s výjimkou navigačních vlastností) entity, klikněte pravým tlačítkem a vyberte **Refaktorovat -&gt; přesunout na nový komplexní typ**.</span><span class="sxs-lookup"><span data-stu-id="18f84-124">On the designer surface, select one or more properties (excluding navigation properties) of an entity, then right-click and select **Refactor -&gt; Move to New Complex Type**.</span></span>

![Refaktorovat](~/ef6/media/refactor.png)

<span data-ttu-id="18f84-126">Nový komplexní typ s vybranou vlastností se přidá do **prohlížeč modelu**.</span><span class="sxs-lookup"><span data-stu-id="18f84-126">A new complex type with the selected properties is added to the **Model Browser**.</span></span> <span data-ttu-id="18f84-127">Komplexní typ je přiřazen výchozí název.</span><span class="sxs-lookup"><span data-stu-id="18f84-127">The complex type is given a default name.</span></span>

<span data-ttu-id="18f84-128">Komplexní vlastnosti nově vytvořeného typu nahradí vybraných vlastností.</span><span class="sxs-lookup"><span data-stu-id="18f84-128">A complex property of the newly created type replaces the selected properties.</span></span> <span data-ttu-id="18f84-129">Jsou zachovány všechny mapování vlastností.</span><span class="sxs-lookup"><span data-stu-id="18f84-129">All property mappings are preserved.</span></span>

![Refaktorovat 2](~/ef6/media/refactor2.png)

## <a name="create-a-new-complex-type"></a><span data-ttu-id="18f84-131">Vytvořte nový komplexní typ</span><span class="sxs-lookup"><span data-stu-id="18f84-131">Create a New Complex Type</span></span>

<span data-ttu-id="18f84-132">Můžete také vytvořit nový komplexní typ, který obsahuje vlastnosti existující entity.</span><span class="sxs-lookup"><span data-stu-id="18f84-132">You can also create a new complex type that does not contain properties of an existing entity.</span></span>

<span data-ttu-id="18f84-133">Klikněte pravým tlačítkem na **komplexní typy** složky přejděte v prohlížeči modelů **komplexní typ funkce operací AddNew...** .</span><span class="sxs-lookup"><span data-stu-id="18f84-133">Right-click the **Complex Types** folder in the Model Browser, point to **AddNew Complex Type…**.</span></span> <span data-ttu-id="18f84-134">Alternativně můžete vybrat **komplexní typy** složky a stiskněte klávesu **vložit** kláves na klávesnici.</span><span class="sxs-lookup"><span data-stu-id="18f84-134">Alternatively, you can select the **Complex Types** folder and press the **Insert** key on your keyboard.</span></span>

![Přidat nový komplexní typ](~/ef6/media/addnewcomplextype.png)

<span data-ttu-id="18f84-136">Nový komplexní typ se přidá do složky s výchozím názvem.</span><span class="sxs-lookup"><span data-stu-id="18f84-136">A new complex type is added to the folder with a default name.</span></span> <span data-ttu-id="18f84-137">Nyní můžete přidat vlastnosti typu.</span><span class="sxs-lookup"><span data-stu-id="18f84-137">You can now add properties to the type.</span></span>

## <a name="add-properties-to-a-complex-type"></a><span data-ttu-id="18f84-138">Přidání vlastností komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="18f84-138">Add Properties to a Complex Type</span></span>

<span data-ttu-id="18f84-139">Vlastnosti komplexní typ může být Skalární typy nebo existující komplexní typy.</span><span class="sxs-lookup"><span data-stu-id="18f84-139">Properties of a complex type can be scalar types or existing complex types.</span></span> <span data-ttu-id="18f84-140">Komplexní typ vlastnosti však nesmí obsahovat Kruhové odkazy.</span><span class="sxs-lookup"><span data-stu-id="18f84-140">However, complex type properties cannot have circular references.</span></span> <span data-ttu-id="18f84-141">Například komplexní typ **OnsiteCourseDetails** nemůže mít vlastnost komplexního typu **OnsiteCourseDetails**.</span><span class="sxs-lookup"><span data-stu-id="18f84-141">For example, a complex type **OnsiteCourseDetails** cannot have a property of complex type **OnsiteCourseDetails**.</span></span>

<span data-ttu-id="18f84-142">Přidat vlastnost na komplexní typ v některém z níže uvedených způsobů.</span><span class="sxs-lookup"><span data-stu-id="18f84-142">You can add a property to a complex type in any of the ways listed below.</span></span>

-   <span data-ttu-id="18f84-143">Klikněte pravým tlačítkem na komplexní typ v modelu prohlížeč, přejděte na **přidat**, přejděte na **skalární vlastnost** nebo **komplexní vlastnost**, pak vyberte typ požadované vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="18f84-143">Right-click a complex type in the Model Browser, point to **Add**, then point to **Scalar Property** or **Complex Property**, then select the desired property type.</span></span> <span data-ttu-id="18f84-144">Alternativně můžete vybrat komplexní typ a potom stiskněte klávesu **vložit** kláves na klávesnici.</span><span class="sxs-lookup"><span data-stu-id="18f84-144">Alternatively, you can select a complex type and then press the **Insert** key on your keyboard.</span></span>  

    ![Přidání vlastností komplexního typu](~/ef6/media/addpropertiestocomplextype.png)

    <span data-ttu-id="18f84-146">Komplexní typ s výchozím názvem je přidána nová vlastnost.</span><span class="sxs-lookup"><span data-stu-id="18f84-146">A new property is added to the complex type with a default name.</span></span>

- <span data-ttu-id="18f84-147">OR –</span><span class="sxs-lookup"><span data-stu-id="18f84-147">OR -</span></span>

-   <span data-ttu-id="18f84-148">Klikněte pravým tlačítkem na vlastnost entity na **EF designeru** ploše a vyberte možnost **kopírování**, klikněte pravým tlačítkem na komplexní typ, **prohlížeč modelu** a vyberte  **Vložit**.</span><span class="sxs-lookup"><span data-stu-id="18f84-148">Right-click an entity property on the **EF  Designer** surface and select **Copy**, then right-click the complex type in the **Model Browser** and select **Paste**.</span></span>

## <a name="rename-a-complex-type"></a><span data-ttu-id="18f84-149">Přejmenovat komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="18f84-149">Rename a Complex Type</span></span>

<span data-ttu-id="18f84-150">Při přejmenování komplexní typ, se aktualizují všechny odkazy na typ v celém projektu.</span><span class="sxs-lookup"><span data-stu-id="18f84-150">When you rename a complex type, all references to the type are updated throughout the project.</span></span>

-   <span data-ttu-id="18f84-151">Pomalu klikněte dvakrát na komplexní typ **prohlížeč modelu**.</span><span class="sxs-lookup"><span data-stu-id="18f84-151">Slowly double-click a complex type in the **Model Browser**.</span></span>
    <span data-ttu-id="18f84-152">Název bude vybrán a v režimu úprav.</span><span class="sxs-lookup"><span data-stu-id="18f84-152">The name will be selected and in edit mode.</span></span>

- <span data-ttu-id="18f84-153">OR –</span><span class="sxs-lookup"><span data-stu-id="18f84-153">OR -</span></span>

-   <span data-ttu-id="18f84-154">Klikněte pravým tlačítkem na komplexní typ **prohlížeč modelu** a vyberte **přejmenovat**.</span><span class="sxs-lookup"><span data-stu-id="18f84-154">Right-click a complex type in the **Model Browser** and select **Rename**.</span></span>

- <span data-ttu-id="18f84-155">OR –</span><span class="sxs-lookup"><span data-stu-id="18f84-155">OR -</span></span>

-   <span data-ttu-id="18f84-156">Vyberte komplexní typ v prohlížeči modelů a stisknutím klávesy F2.</span><span class="sxs-lookup"><span data-stu-id="18f84-156">Select a complex type in the Model Browser and press the F2 key.</span></span>

- <span data-ttu-id="18f84-157">OR –</span><span class="sxs-lookup"><span data-stu-id="18f84-157">OR -</span></span>

-   <span data-ttu-id="18f84-158">Klikněte pravým tlačítkem na komplexní typ **prohlížeč modelu** a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="18f84-158">Right-click a complex type in the **Model Browser** and select **Properties**.</span></span> <span data-ttu-id="18f84-159">Upravit název v **vlastnosti** okna.</span><span class="sxs-lookup"><span data-stu-id="18f84-159">Edit the name in the **Properties** window.</span></span>

## <a name="add-an-existing-complex-type-to-an-entity-and-map-its-properties-to-table-columns"></a><span data-ttu-id="18f84-160">Přidat existující komplexního typu Entity a namapujte jeho vlastnosti na sloupce tabulky</span><span class="sxs-lookup"><span data-stu-id="18f84-160">Add an Existing Complex Type to an Entity and Map its Properties to Table Columns</span></span>

1.  <span data-ttu-id="18f84-161">Klikněte pravým tlačítkem na entitu, přejdete na **přidat nový**a vyberte **komplexní vlastnost**.</span><span class="sxs-lookup"><span data-stu-id="18f84-161">Right-click an entity, point to **Add New**, and select **Complex Property**.</span></span>
    <span data-ttu-id="18f84-162">Komplexní vlastnost s výchozím názvem se přidá k entitě.</span><span class="sxs-lookup"><span data-stu-id="18f84-162">A complex type property with a default name is added to the entity.</span></span> <span data-ttu-id="18f84-163">Výchozí typ (zvolit existující komplexní typy) je přiřazen k vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="18f84-163">A default type (chosen from the existing complex types) is assigned to the property.</span></span>
2.  <span data-ttu-id="18f84-164">Přiřaďte požadovaného typu vlastnosti v **vlastnosti** okna.</span><span class="sxs-lookup"><span data-stu-id="18f84-164">Assign the desired type to the property in the **Properties** window.</span></span>
    <span data-ttu-id="18f84-165">Po přidání vlastnosti komplexní typ entity, je nutné mapovat na sloupce tabulky její vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="18f84-165">After adding a complex type property to an entity, you must map its properties to table columns.</span></span>
3.  <span data-ttu-id="18f84-166">Klikněte pravým tlačítkem na typ entity na návrhové ploše nebo na **prohlížeč modelu** a vyberte **mapování tabulky**.</span><span class="sxs-lookup"><span data-stu-id="18f84-166">Right-click an entity type on the design surface or in the **Model Browser** and select **Table Mappings**.</span></span>
    <span data-ttu-id="18f84-167">Mapování tabulky se zobrazí v **podrobnosti mapování** okna.</span><span class="sxs-lookup"><span data-stu-id="18f84-167">The table mappings are displayed in the **Mapping Details** window.</span></span>
4.  <span data-ttu-id="18f84-168">Rozbalte **mapuje &lt;název tabulky&gt;**  uzlu.</span><span class="sxs-lookup"><span data-stu-id="18f84-168">Expand the **Maps to &lt;Table Name&gt;** node.</span></span>
    <span data-ttu-id="18f84-169">A **mapování sloupců** uzel se objeví.</span><span class="sxs-lookup"><span data-stu-id="18f84-169">A **Column Mappings** node appears.</span></span>
5.  <span data-ttu-id="18f84-170">Rozbalte **mapování sloupců** uzlu.</span><span class="sxs-lookup"><span data-stu-id="18f84-170">Expand the **Column Mappings** node.</span></span>
    <span data-ttu-id="18f84-171">Zobrazí se seznam všech sloupců v tabulce.</span><span class="sxs-lookup"><span data-stu-id="18f84-171">A list of all the columns in the table appears.</span></span> <span data-ttu-id="18f84-172">Výchozí vlastnosti (pokud existuje) který mapovat sloupce, které jsou uvedeny v části **/vlastnost Value** záhlaví.</span><span class="sxs-lookup"><span data-stu-id="18f84-172">The default properties (if any) to which the columns map are listed under the **Value/Property** heading.</span></span>
6.  <span data-ttu-id="18f84-173">Vyberte sloupec, který chcete propojit a klikněte pravým tlačítkem na příslušné **/vlastnost Value** pole.</span><span class="sxs-lookup"><span data-stu-id="18f84-173">Select the column you want to map, and then right-click the corresponding **Value/Property** field.</span></span>
    <span data-ttu-id="18f84-174">Zobrazí se rozevírací seznam Skalární vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="18f84-174">A drop-down list of all the scalar properties is displayed.</span></span>
7.  <span data-ttu-id="18f84-175">Vyberte příslušnou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="18f84-175">Select the appropriate property.</span></span>

    ![Komplexní typ mapy](~/ef6/media/mapcomplextype.png)

8.  <span data-ttu-id="18f84-177">Opakujte kroky 6 a 7 pro každý sloupec tabulky.</span><span class="sxs-lookup"><span data-stu-id="18f84-177">Repeat steps 6 and 7 for each table column.</span></span>

>[!NOTE]
> <span data-ttu-id="18f84-178">Pokud chcete odstranit mapování sloupce, vyberte sloupec, který chcete namapovat a potom klikněte na tlačítko **/vlastnost Value** pole.</span><span class="sxs-lookup"><span data-stu-id="18f84-178">To delete a column mapping, select the column that you want to map, and then click the **Value/Property** field.</span></span> <span data-ttu-id="18f84-179">Vyberte **odstranit** z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="18f84-179">Then, select **Delete** from the drop-down list.</span></span>

## <a name="map-a-function-import-to-a-complex-type"></a><span data-ttu-id="18f84-180">Mapování importované funkce komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="18f84-180">Map a Function Import to a Complex Type</span></span>

<span data-ttu-id="18f84-181">Funkce jsou založeny na uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="18f84-181">Function imports are based on stored procedures.</span></span> <span data-ttu-id="18f84-182">Mapování importované funkce na komplexní typ, sloupců vrácený odpovídající uložené procedury musí odpovídat vlastnosti komplexní typ, číslo a musí mít typy úložišť, které jsou kompatibilní s typy vlastností.</span><span class="sxs-lookup"><span data-stu-id="18f84-182">To map a function import to a complex type, the columns returned by the corresponding stored procedure must match the properties of the complex type in number and must have storage types that are compatible with the property types.</span></span>

-   <span data-ttu-id="18f84-183">Dvakrát klikněte na importované funkce, která chcete namapovat komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="18f84-183">Double-click on an imported function that you want to map to a complex type.</span></span>

    ![Imports – funkce](~/ef6/media/functionimports.png)

-   <span data-ttu-id="18f84-185">Zadejte nastavení pro nové importované funkce takto:</span><span class="sxs-lookup"><span data-stu-id="18f84-185">Fill in the settings for the new function import, as follows:</span></span>
    -   <span data-ttu-id="18f84-186">Zadejte uloženou proceduru, pro kterou vytváříte importované funkce v **název uložené procedury** pole.</span><span class="sxs-lookup"><span data-stu-id="18f84-186">Specify the stored procedure for which you are creating a function import in the **Stored Procedure Name** field.</span></span> <span data-ttu-id="18f84-187">Toto pole je rozevírací seznam, který zobrazuje všechny uložené procedury v modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="18f84-187">This field is a drop-down list that displays all the stored procedures in the storage model.</span></span>
    -   <span data-ttu-id="18f84-188">Zadejte název v importované funkce **importovat název funkce** pole.</span><span class="sxs-lookup"><span data-stu-id="18f84-188">Specify the name of the function import in the **Function Import Name** field.</span></span>
    -   <span data-ttu-id="18f84-189">Vyberte **komplexní** jako návratový typ a pak zadejte konkrétní komplexní návratový typ zvolením příslušného typu z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="18f84-189">Select **Complex** as the return type and then specify the specific complex return type by choosing the appropriate type from the drop-down list.</span></span>

        ![Upravit importované funkce](~/ef6/media/editfunctionimport.png)

-   <span data-ttu-id="18f84-191">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="18f84-191">Click **OK**.</span></span>
    <span data-ttu-id="18f84-192">Položka import funkce je vytvořena v konceptuálním modelu.</span><span class="sxs-lookup"><span data-stu-id="18f84-192">The function import entry is created in the conceptual model.</span></span>

### <a name="customize-column-mapping-for-function-import"></a><span data-ttu-id="18f84-193">Mapování importované funkce sloupec</span><span class="sxs-lookup"><span data-stu-id="18f84-193">Customize Column Mapping for Function Import</span></span>

-   <span data-ttu-id="18f84-194">Importovaná funkce v prohlížeči modelů klikněte pravým tlačítkem a vyberte **Import mapování funkcí**.</span><span class="sxs-lookup"><span data-stu-id="18f84-194">Right-click the function import in the Model Browser and select **Function Import Mapping**.</span></span>
    <span data-ttu-id="18f84-195">**Podrobnosti mapování** okna se zobrazí a zobrazí výchozí mapování importované funkce.</span><span class="sxs-lookup"><span data-stu-id="18f84-195">The **Mapping Details** window appears and shows the default mapping for the function import.</span></span> <span data-ttu-id="18f84-196">Šipky označují mapování mezi hodnot sloupců a hodnot vlastností.</span><span class="sxs-lookup"><span data-stu-id="18f84-196">Arrows indicate the mappings between column values and property values.</span></span> <span data-ttu-id="18f84-197">Ve výchozím nastavení názvy sloupců jsou předpokládá se, že stejné jako názvy vlastností komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="18f84-197">By default, the column names are assumed to be the same as the complex type's property names.</span></span> <span data-ttu-id="18f84-198">Výchozí názvy sloupců se zobrazí v šedé textu.</span><span class="sxs-lookup"><span data-stu-id="18f84-198">The default column names appear in gray text.</span></span>
-   <span data-ttu-id="18f84-199">V případě potřeby změňte názvy sloupců tak, aby odpovídaly názvy sloupců, které jsou vráceny pomocí uložené procedury, která odpovídá na importované funkce.</span><span class="sxs-lookup"><span data-stu-id="18f84-199">If necessary, change the column names to match the column names that are returned by the stored procedure that corresponds to the function import.</span></span>

## <a name="delete-a-complex-type"></a><span data-ttu-id="18f84-200">Odstranit komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="18f84-200">Delete a Complex Type</span></span>

<span data-ttu-id="18f84-201">Když odstraníte komplexní typ, typ je odstraněn z Koncepční model a budou odstraněny mapování pro všechny instance daného typu.</span><span class="sxs-lookup"><span data-stu-id="18f84-201">When you delete a complex type, the type is deleted from the conceptual model and mappings for all instances of the type are deleted.</span></span> <span data-ttu-id="18f84-202">Odkazy na typ se ale neaktualizují.</span><span class="sxs-lookup"><span data-stu-id="18f84-202">However, references to the type are not updated.</span></span> <span data-ttu-id="18f84-203">Například, pokud má entita komplexní vlastnost typu ComplexType1 a ComplexType1 se odstraní v **prohlížeč modelu**, odpovídající vlastnost entity není aktualizován.</span><span class="sxs-lookup"><span data-stu-id="18f84-203">For example, if an entity has a complex type property of type ComplexType1 and ComplexType1 is deleted in the **Model Browser**, the corresponding entity property is not updated.</span></span> <span data-ttu-id="18f84-204">Model nebude ověřit, protože obsahuje entitu, která odkazuje na odstraněný komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="18f84-204">The model will not validate  because it contains an entity that references a deleted complex type.</span></span> <span data-ttu-id="18f84-205">Můžete aktualizovat nebo odstranit odkazy na odstraněný komplexních typů pomocí návrháře entit.</span><span class="sxs-lookup"><span data-stu-id="18f84-205">You can update or delete references to deleted complex types by using the Entity Designer.</span></span>

-   <span data-ttu-id="18f84-206">Komplexní typ v prohlížeči modelů klikněte pravým tlačítkem a vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="18f84-206">Right-click a complex type in the Model Browser and select **Delete**.</span></span>

- <span data-ttu-id="18f84-207">OR –</span><span class="sxs-lookup"><span data-stu-id="18f84-207">OR -</span></span>

-   <span data-ttu-id="18f84-208">Vyberte komplexní typ v prohlížeči modelů a na klávesnici stiskněte klávesu Delete.</span><span class="sxs-lookup"><span data-stu-id="18f84-208">Select a complex type in the Model Browser and press the Delete key on your keyboard.</span></span>

## <a name="query-for-entities-containing-properties-of-complex-type"></a><span data-ttu-id="18f84-209">Dotazy na entity obsahující vlastnosti tohoto komplexního typu</span><span class="sxs-lookup"><span data-stu-id="18f84-209">Query for Entities Containing Properties of Complex Type</span></span>

<span data-ttu-id="18f84-210">Následující kód ukazuje, jak spustit dotaz, který vrátí kolekci entit typu objektů, které obsahují komplexní typ vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="18f84-210">The following code shows how to execute a query that returns a collection of entity type objects that contain a complex type property.</span></span>

``` csharp
    using (SchoolEntities context = new SchoolEntities())
    {
        var courses =
            from c in context.OnsiteCourses
            order by c.Details.Time
            select c;

        foreach (var c in courses)
        {
            Console.WriteLine("Time: " + c.Details.Time);
            Console.WriteLine("Days: " + c.Details.Days);
            Console.WriteLine("Location: " + c.Details.Location);
        }
    }
```
