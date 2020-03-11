---
title: Komplexní typy – Návrhář EF – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9a8228ef-acfd-4575-860d-769d2c0e18a1
ms.openlocfilehash: a3c5578acee23688c04772d2dd0a2221779af562
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418649"
---
# <a name="complex-types---ef-designer"></a><span data-ttu-id="caa74-102">Komplexní typy – Návrhář EF</span><span class="sxs-lookup"><span data-stu-id="caa74-102">Complex Types - EF Designer</span></span>
<span data-ttu-id="caa74-103">Toto téma ukazuje, jak namapovat komplexní typy pomocí Entity Framework Designer (EF Designer) a jak se dotazovat na entity, které obsahují vlastnosti komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="caa74-103">This topic shows how to map complex types with the Entity Framework Designer (EF Designer) and how to query for entities that contain properties of complex type.</span></span>

<span data-ttu-id="caa74-104">Následující obrázek ukazuje hlavní okna, která se používají při práci s návrhářem EF.</span><span class="sxs-lookup"><span data-stu-id="caa74-104">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![Návrhář EF](~/ef6/media/efdesigner.png)

> [!NOTE]
> <span data-ttu-id="caa74-106">Při sestavování koncepčního modelu se může zobrazit upozornění týkající se nemapovaných entit a přidružení v Seznam chyb.</span><span class="sxs-lookup"><span data-stu-id="caa74-106">When you build the conceptual model, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="caa74-107">Tato upozornění můžete ignorovat, protože po výběru databáze z modelu budou tyto chyby pryč.</span><span class="sxs-lookup"><span data-stu-id="caa74-107">You can ignore these warnings because after you choose to generate the database from the model, the errors will go away.</span></span>

## <a name="what-is-a-complex-type"></a><span data-ttu-id="caa74-108">Co je komplexní typ</span><span class="sxs-lookup"><span data-stu-id="caa74-108">What is a Complex Type</span></span>

<span data-ttu-id="caa74-109">Komplexní typy jsou jiné než skalární vlastnosti typů entit, které umožňují organizovat skalární vlastnosti v rámci entit.</span><span class="sxs-lookup"><span data-stu-id="caa74-109">Complex types are non-scalar properties of entity types that enable scalar properties to be organized within entities.</span></span> <span data-ttu-id="caa74-110">Podobně jako entity, komplexní typy sestávají z skalárních vlastností nebo jiných vlastností komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="caa74-110">Like entities, complex types consist of scalar properties or other complex type properties.</span></span>

<span data-ttu-id="caa74-111">Při práci s objekty, které reprezentují komplexní typy, mějte na paměti následující:</span><span class="sxs-lookup"><span data-stu-id="caa74-111">When you work with objects that represent complex types, be aware of the following:</span></span>

-   <span data-ttu-id="caa74-112">Komplexní typy nemají klíče, a proto nemohou existovat nezávisle.</span><span class="sxs-lookup"><span data-stu-id="caa74-112">Complex types do not have keys and therefore cannot exist independently.</span></span> <span data-ttu-id="caa74-113">Komplexní typy mohou existovat pouze jako vlastnosti typů entit nebo jiných komplexních typů.</span><span class="sxs-lookup"><span data-stu-id="caa74-113">Complex types can only exist as properties of entity types or other complex types.</span></span>
-   <span data-ttu-id="caa74-114">Komplexní typy se nemůžou účastnit přidružení a nemůžou obsahovat navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="caa74-114">Complex types cannot participate in associations and cannot contain navigation properties.</span></span>
-   <span data-ttu-id="caa74-115">Vlastnosti komplexního typu nemohou mít **hodnotu null**.</span><span class="sxs-lookup"><span data-stu-id="caa74-115">Complex type properties cannot be **null**.</span></span> <span data-ttu-id="caa74-116">K hodnotě **InvalidOperationException **dojde, pokud je volána metoda **DbContext. SaveChanges** a byl zjištěn složitý objekt s hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="caa74-116">An **InvalidOperationException **occurs when **DbContext.SaveChanges** is called and a null complex object is encountered.</span></span> <span data-ttu-id="caa74-117">Skalární vlastnosti komplexních objektů mohou mít **hodnotu null**.</span><span class="sxs-lookup"><span data-stu-id="caa74-117">Scalar properties of complex objects can be **null**.</span></span>
-   <span data-ttu-id="caa74-118">Komplexní typy nemůžou dědit z jiných komplexních typů.</span><span class="sxs-lookup"><span data-stu-id="caa74-118">Complex types cannot inherit from other complex types.</span></span>
-   <span data-ttu-id="caa74-119">Komplexní typ musíte definovat jako **třídu**.</span><span class="sxs-lookup"><span data-stu-id="caa74-119">You must define the complex type as a **class**.</span></span> 
-   <span data-ttu-id="caa74-120">EF detekuje změny členů v objektu komplexního typu, když je volána metoda **DbContext. DetectChanges** .</span><span class="sxs-lookup"><span data-stu-id="caa74-120">EF detects changes to members on a complex type object when **DbContext.DetectChanges** is called.</span></span> <span data-ttu-id="caa74-121">Entity Framework volá automaticky **DetectChanges** při volání následujících členů: **negenerickými. Find**, **negenerickými. Local**, **negenerickými. Remove**, **negenerickými. Add**, **negenerickými. Attach**, **DbContext. SaveChanges**, **DbContext. GetValidationErrors**, **DbContext. entry**, **DbChangeTracker. Entries**.</span><span class="sxs-lookup"><span data-stu-id="caa74-121">Entity Framework calls **DetectChanges** automatically when the following members are called: **DbSet.Find**, **DbSet.Local**, **DbSet.Remove**, **DbSet.Add**, **DbSet.Attach**, **DbContext.SaveChanges**, **DbContext.GetValidationErrors**, **DbContext.Entry**, **DbChangeTracker.Entries**.</span></span>

## <a name="refactor-an-entitys-properties-into-new-complex-type"></a><span data-ttu-id="caa74-122">Refaktorování vlastností entity do nového komplexního typu</span><span class="sxs-lookup"><span data-stu-id="caa74-122">Refactor an Entity’s Properties into New Complex Type</span></span>

<span data-ttu-id="caa74-123">Pokud již máte entitu ve koncepčním modelu, můžete chtít Refaktorovat některé vlastnosti do vlastnosti komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="caa74-123">If you already have an entity in your conceptual model you may want to refactor some of the properties into a complex type property.</span></span>

<span data-ttu-id="caa74-124">Na návrhové ploše vyberte jednu nebo více vlastností (kromě vlastností navigace) entity, klikněte pravým tlačítkem myši a vyberte **refaktoring –&gt; přesunout na nový komplexní typ**.</span><span class="sxs-lookup"><span data-stu-id="caa74-124">On the designer surface, select one or more properties (excluding navigation properties) of an entity, then right-click and select **Refactor -&gt; Move to New Complex Type**.</span></span>

![Refaktorovat](~/ef6/media/refactor.png)

<span data-ttu-id="caa74-126">Do **prohlížeče modelu**se přidá nový komplexní typ s vybranými vlastnostmi.</span><span class="sxs-lookup"><span data-stu-id="caa74-126">A new complex type with the selected properties is added to the **Model Browser**.</span></span> <span data-ttu-id="caa74-127">Komplexnímu typu je udělen výchozí název.</span><span class="sxs-lookup"><span data-stu-id="caa74-127">The complex type is given a default name.</span></span>

<span data-ttu-id="caa74-128">Komplexní vlastnost nově vytvořeného typu nahradí vybrané vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="caa74-128">A complex property of the newly created type replaces the selected properties.</span></span> <span data-ttu-id="caa74-129">Všechna mapování vlastností jsou zachována.</span><span class="sxs-lookup"><span data-stu-id="caa74-129">All property mappings are preserved.</span></span>

![Refaktorovat 2](~/ef6/media/refactor2.png)

## <a name="create-a-new-complex-type"></a><span data-ttu-id="caa74-131">Vytvořit nový komplexní typ</span><span class="sxs-lookup"><span data-stu-id="caa74-131">Create a New Complex Type</span></span>

<span data-ttu-id="caa74-132">Můžete také vytvořit nový komplexní typ, který neobsahuje vlastnosti existující entity.</span><span class="sxs-lookup"><span data-stu-id="caa74-132">You can also create a new complex type that does not contain properties of an existing entity.</span></span>

<span data-ttu-id="caa74-133">Klikněte pravým tlačítkem na složku **komplexních typů** v prohlížeči modelů, přejděte na **komplexní typ AddNew...** .</span><span class="sxs-lookup"><span data-stu-id="caa74-133">Right-click the **Complex Types** folder in the Model Browser, point to **AddNew Complex Type…**.</span></span> <span data-ttu-id="caa74-134">Případně můžete vybrat složku **komplexních typů** a stisknout klávesu **INSERT** na klávesnici.</span><span class="sxs-lookup"><span data-stu-id="caa74-134">Alternatively, you can select the **Complex Types** folder and press the **Insert** key on your keyboard.</span></span>

![Přidat nový komplexní typ](~/ef6/media/addnewcomplextype.png)

<span data-ttu-id="caa74-136">Do složky s výchozím názvem se přidá nový komplexní typ.</span><span class="sxs-lookup"><span data-stu-id="caa74-136">A new complex type is added to the folder with a default name.</span></span> <span data-ttu-id="caa74-137">Nyní můžete do typu přidat vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="caa74-137">You can now add properties to the type.</span></span>

## <a name="add-properties-to-a-complex-type"></a><span data-ttu-id="caa74-138">Přidání vlastností do komplexního typu</span><span class="sxs-lookup"><span data-stu-id="caa74-138">Add Properties to a Complex Type</span></span>

<span data-ttu-id="caa74-139">Vlastnosti komplexního typu mohou být skalární typy nebo existující komplexní typy.</span><span class="sxs-lookup"><span data-stu-id="caa74-139">Properties of a complex type can be scalar types or existing complex types.</span></span> <span data-ttu-id="caa74-140">Vlastnosti komplexního typu ale nemůžou mít cyklické odkazy.</span><span class="sxs-lookup"><span data-stu-id="caa74-140">However, complex type properties cannot have circular references.</span></span> <span data-ttu-id="caa74-141">Například komplexní typ **OnsiteCourseDetails** nemůže mít vlastnost komplexního typu **OnsiteCourseDetails**.</span><span class="sxs-lookup"><span data-stu-id="caa74-141">For example, a complex type **OnsiteCourseDetails** cannot have a property of complex type **OnsiteCourseDetails**.</span></span>

<span data-ttu-id="caa74-142">Vlastnost můžete do komplexního typu přidat libovolným způsobem uvedeným níže.</span><span class="sxs-lookup"><span data-stu-id="caa74-142">You can add a property to a complex type in any of the ways listed below.</span></span>

-   <span data-ttu-id="caa74-143">V prohlížeči modelů klikněte pravým tlačítkem myši na komplexní typ, přejděte na **Přidat**a pak na **skalární vlastnost** nebo **komplexní vlastnost**a pak vyberte požadovaný typ vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="caa74-143">Right-click a complex type in the Model Browser, point to **Add**, then point to **Scalar Property** or **Complex Property**, then select the desired property type.</span></span> <span data-ttu-id="caa74-144">Případně můžete vybrat složitý typ a pak na klávesnici stisknout klávesu **Insert** .</span><span class="sxs-lookup"><span data-stu-id="caa74-144">Alternatively, you can select a complex type and then press the **Insert** key on your keyboard.</span></span>  

    ![Přidat vlastnosti do komplexního typu](~/ef6/media/addpropertiestocomplextype.png)

    <span data-ttu-id="caa74-146">Do komplexního typu se přidá nová vlastnost s výchozím názvem.</span><span class="sxs-lookup"><span data-stu-id="caa74-146">A new property is added to the complex type with a default name.</span></span>

- <span data-ttu-id="caa74-147">Ani</span><span class="sxs-lookup"><span data-stu-id="caa74-147">OR -</span></span>

-   <span data-ttu-id="caa74-148">Klikněte pravým tlačítkem na plochu **Návrháře EF** a vyberte **Kopírovat**, klikněte pravým tlačítkem na komplexní typ v **prohlížeči modelů** a vyberte **Vložit**.</span><span class="sxs-lookup"><span data-stu-id="caa74-148">Right-click an entity property on the **EF  Designer** surface and select **Copy**, then right-click the complex type in the **Model Browser** and select **Paste**.</span></span>

## <a name="rename-a-complex-type"></a><span data-ttu-id="caa74-149">Přejmenování komplexního typu</span><span class="sxs-lookup"><span data-stu-id="caa74-149">Rename a Complex Type</span></span>

<span data-ttu-id="caa74-150">Při přejmenování komplexního typu jsou všechny odkazy na typ aktualizovány v celém projektu.</span><span class="sxs-lookup"><span data-stu-id="caa74-150">When you rename a complex type, all references to the type are updated throughout the project.</span></span>

-   <span data-ttu-id="caa74-151">Pomalu dvakrát klikněte na komplexní typ v **prohlížeči modelů**.</span><span class="sxs-lookup"><span data-stu-id="caa74-151">Slowly double-click a complex type in the **Model Browser**.</span></span>
    <span data-ttu-id="caa74-152">Název bude vybrán a v režimu úprav.</span><span class="sxs-lookup"><span data-stu-id="caa74-152">The name will be selected and in edit mode.</span></span>

- <span data-ttu-id="caa74-153">Ani</span><span class="sxs-lookup"><span data-stu-id="caa74-153">OR -</span></span>

-   <span data-ttu-id="caa74-154">V **prohlížeči modelů** klikněte pravým tlačítkem na komplexní typ a vyberte **Přejmenovat**.</span><span class="sxs-lookup"><span data-stu-id="caa74-154">Right-click a complex type in the **Model Browser** and select **Rename**.</span></span>

- <span data-ttu-id="caa74-155">Ani</span><span class="sxs-lookup"><span data-stu-id="caa74-155">OR -</span></span>

-   <span data-ttu-id="caa74-156">V prohlížeči modelů vyberte složitý typ a stiskněte klávesu F2.</span><span class="sxs-lookup"><span data-stu-id="caa74-156">Select a complex type in the Model Browser and press the F2 key.</span></span>

- <span data-ttu-id="caa74-157">Ani</span><span class="sxs-lookup"><span data-stu-id="caa74-157">OR -</span></span>

-   <span data-ttu-id="caa74-158">V **prohlížeči modelů** klikněte pravým tlačítkem na komplexní typ a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="caa74-158">Right-click a complex type in the **Model Browser** and select **Properties**.</span></span> <span data-ttu-id="caa74-159">Upravte název v okně **vlastnosti** .</span><span class="sxs-lookup"><span data-stu-id="caa74-159">Edit the name in the **Properties** window.</span></span>

## <a name="add-an-existing-complex-type-to-an-entity-and-map-its-properties-to-table-columns"></a><span data-ttu-id="caa74-160">Přidání existujícího komplexního typu k entitě a mapování vlastností na sloupce tabulky</span><span class="sxs-lookup"><span data-stu-id="caa74-160">Add an Existing Complex Type to an Entity and Map its Properties to Table Columns</span></span>

1.  <span data-ttu-id="caa74-161">Klikněte pravým tlačítkem na entitu, přejděte na **Přidat nový**a vyberte **složitá vlastnost**.</span><span class="sxs-lookup"><span data-stu-id="caa74-161">Right-click an entity, point to **Add New**, and select **Complex Property**.</span></span>
    <span data-ttu-id="caa74-162">Do entity se přidá vlastnost komplexního typu s výchozím názvem.</span><span class="sxs-lookup"><span data-stu-id="caa74-162">A complex type property with a default name is added to the entity.</span></span> <span data-ttu-id="caa74-163">K vlastnosti je přiřazen výchozí typ (zvoleno z existujících komplexních typů).</span><span class="sxs-lookup"><span data-stu-id="caa74-163">A default type (chosen from the existing complex types) is assigned to the property.</span></span>
2.  <span data-ttu-id="caa74-164">Přiřaďte požadovaný typ k vlastnosti v okně **vlastnosti** .</span><span class="sxs-lookup"><span data-stu-id="caa74-164">Assign the desired type to the property in the **Properties** window.</span></span>
    <span data-ttu-id="caa74-165">Po přidání vlastnosti komplexního typu do entity je nutné namapovat její vlastnosti na sloupce tabulky.</span><span class="sxs-lookup"><span data-stu-id="caa74-165">After adding a complex type property to an entity, you must map its properties to table columns.</span></span>
3.  <span data-ttu-id="caa74-166">Pravým tlačítkem myši klikněte na typ entity na návrhové ploše nebo v **prohlížeči modelů** a vyberte **mapování tabulek**.</span><span class="sxs-lookup"><span data-stu-id="caa74-166">Right-click an entity type on the design surface or in the **Model Browser** and select **Table Mappings**.</span></span>
    <span data-ttu-id="caa74-167">Mapování tabulek se zobrazí v okně **Podrobnosti mapování** .</span><span class="sxs-lookup"><span data-stu-id="caa74-167">The table mappings are displayed in the **Mapping Details** window.</span></span>
4.  <span data-ttu-id="caa74-168">Rozbalte uzel **mapy pro &lt;název tabulky&gt;**  Node.</span><span class="sxs-lookup"><span data-stu-id="caa74-168">Expand the **Maps to &lt;Table Name&gt;** node.</span></span>
    <span data-ttu-id="caa74-169">Zobrazí se uzel  **mapování sloupců** .</span><span class="sxs-lookup"><span data-stu-id="caa74-169">A **Column Mappings** node appears.</span></span>
5.  <span data-ttu-id="caa74-170">Rozbalte uzel **mapování sloupců** .</span><span class="sxs-lookup"><span data-stu-id="caa74-170">Expand the **Column Mappings** node.</span></span>
    <span data-ttu-id="caa74-171">Zobrazí se seznam všech sloupců v tabulce.</span><span class="sxs-lookup"><span data-stu-id="caa74-171">A list of all the columns in the table appears.</span></span> <span data-ttu-id="caa74-172">Výchozí vlastnosti (pokud existují), na které jsou sloupce namapovány pod položkou **hodnota nebo vlastnost** záhlaví.</span><span class="sxs-lookup"><span data-stu-id="caa74-172">The default properties (if any) to which the columns map are listed under the **Value/Property** heading.</span></span>
6.  <span data-ttu-id="caa74-173">Vyberte sloupec, který chcete namapovat, a potom klikněte pravým tlačítkem na odpovídající **hodnotu nebo vlastnost** pole.</span><span class="sxs-lookup"><span data-stu-id="caa74-173">Select the column you want to map, and then right-click the corresponding **Value/Property** field.</span></span>
    <span data-ttu-id="caa74-174">Zobrazí se rozevírací seznam všech skalárních vlastností.</span><span class="sxs-lookup"><span data-stu-id="caa74-174">A drop-down list of all the scalar properties is displayed.</span></span>
7.  <span data-ttu-id="caa74-175">Vyberte odpovídající vlastnost.</span><span class="sxs-lookup"><span data-stu-id="caa74-175">Select the appropriate property.</span></span>

    ![Mapování komplexního typu](~/ef6/media/mapcomplextype.png)

8.  <span data-ttu-id="caa74-177">Opakujte kroky 6 a 7 pro každý sloupec tabulky.</span><span class="sxs-lookup"><span data-stu-id="caa74-177">Repeat steps 6 and 7 for each table column.</span></span>

>[!NOTE]
> <span data-ttu-id="caa74-178">Chcete-li odstranit mapování sloupce, vyberte sloupec, který chcete namapovat, a poté klikněte na pole **hodnota nebo vlastnost** .</span><span class="sxs-lookup"><span data-stu-id="caa74-178">To delete a column mapping, select the column that you want to map, and then click the **Value/Property** field.</span></span> <span data-ttu-id="caa74-179">Pak v rozevíracím seznamu vyberte **Odstranit** .</span><span class="sxs-lookup"><span data-stu-id="caa74-179">Then, select **Delete** from the drop-down list.</span></span>

## <a name="map-a-function-import-to-a-complex-type"></a><span data-ttu-id="caa74-180">Mapování importu funkce na komplexní typ</span><span class="sxs-lookup"><span data-stu-id="caa74-180">Map a Function Import to a Complex Type</span></span>

<span data-ttu-id="caa74-181">Importy funkcí jsou založeny na uložených procedurách.</span><span class="sxs-lookup"><span data-stu-id="caa74-181">Function imports are based on stored procedures.</span></span> <span data-ttu-id="caa74-182">Chcete-li namapovat import funkce na komplexní typ, sloupce vrácené odpovídajícím uloženým postupem musí odpovídat vlastností komplexního typu v Number a musí mít typy úložiště, které jsou kompatibilní s typy vlastností.</span><span class="sxs-lookup"><span data-stu-id="caa74-182">To map a function import to a complex type, the columns returned by the corresponding stored procedure must match the properties of the complex type in number and must have storage types that are compatible with the property types.</span></span>

-   <span data-ttu-id="caa74-183">Dvakrát klikněte na importovanou funkci, kterou chcete namapovat na komplexní typ.</span><span class="sxs-lookup"><span data-stu-id="caa74-183">Double-click on an imported function that you want to map to a complex type.</span></span>

    ![Importy funkcí](~/ef6/media/functionimports.png)

-   <span data-ttu-id="caa74-185">Zadejte nastavení nového importu funkce následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="caa74-185">Fill in the settings for the new function import, as follows:</span></span>
    -   <span data-ttu-id="caa74-186">Zadejte uloženou proceduru, pro kterou vytváříte import funkce v poli **název uložené procedury** .</span><span class="sxs-lookup"><span data-stu-id="caa74-186">Specify the stored procedure for which you are creating a function import in the **Stored Procedure Name** field.</span></span> <span data-ttu-id="caa74-187">Toto pole obsahuje rozevírací seznam, který zobrazuje všechny uložené procedury v modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="caa74-187">This field is a drop-down list that displays all the stored procedures in the storage model.</span></span>
    -   <span data-ttu-id="caa74-188">Zadejte název importované funkce v poli  **importovat název funkce** .</span><span class="sxs-lookup"><span data-stu-id="caa74-188">Specify the name of the function import in the **Function Import Name** field.</span></span>
    -   <span data-ttu-id="caa74-189">Jako návratový typ vyberte **složitý** a pak v rozevíracím seznamu zvolte vhodný typ, a pak zadejte konkrétní komplexní návratový typ.</span><span class="sxs-lookup"><span data-stu-id="caa74-189">Select **Complex** as the return type and then specify the specific complex return type by choosing the appropriate type from the drop-down list.</span></span>

        ![Upravit import funkce](~/ef6/media/editfunctionimport.png)

-   <span data-ttu-id="caa74-191">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="caa74-191">Click **OK**.</span></span>
    <span data-ttu-id="caa74-192">V koncepčním modelu se vytvoří položka importování funkce.</span><span class="sxs-lookup"><span data-stu-id="caa74-192">The function import entry is created in the conceptual model.</span></span>

### <a name="customize-column-mapping-for-function-import"></a><span data-ttu-id="caa74-193">Přizpůsobení mapování sloupce pro import funkce</span><span class="sxs-lookup"><span data-stu-id="caa74-193">Customize Column Mapping for Function Import</span></span>

-   <span data-ttu-id="caa74-194">V prohlížeči modelů klikněte pravým tlačítkem na import funkce a vyberte **mapování importu funkcí**.</span><span class="sxs-lookup"><span data-stu-id="caa74-194">Right-click the function import in the Model Browser and select **Function Import Mapping**.</span></span>
    <span data-ttu-id="caa74-195">Zobrazí se okno **Podrobnosti mapování** a zobrazí se výchozí mapování pro import funkce.</span><span class="sxs-lookup"><span data-stu-id="caa74-195">The **Mapping Details** window appears and shows the default mapping for the function import.</span></span> <span data-ttu-id="caa74-196">Šipky označují mapování mezi hodnotami sloupce a hodnotami vlastností.</span><span class="sxs-lookup"><span data-stu-id="caa74-196">Arrows indicate the mappings between column values and property values.</span></span> <span data-ttu-id="caa74-197">Ve výchozím nastavení se názvy sloupců považují za stejné jako názvy vlastností komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="caa74-197">By default, the column names are assumed to be the same as the complex type's property names.</span></span> <span data-ttu-id="caa74-198">Výchozí názvy sloupců se zobrazí v šedém textu.</span><span class="sxs-lookup"><span data-stu-id="caa74-198">The default column names appear in gray text.</span></span>
-   <span data-ttu-id="caa74-199">V případě potřeby změňte názvy sloupců tak, aby odpovídaly názvům sloupců, které jsou vráceny uloženou procedurou, která odpovídá importu funkce.</span><span class="sxs-lookup"><span data-stu-id="caa74-199">If necessary, change the column names to match the column names that are returned by the stored procedure that corresponds to the function import.</span></span>

## <a name="delete-a-complex-type"></a><span data-ttu-id="caa74-200">Odstranění komplexního typu</span><span class="sxs-lookup"><span data-stu-id="caa74-200">Delete a Complex Type</span></span>

<span data-ttu-id="caa74-201">Při odstranění komplexního typu se odstraní typ z konceptuálního modelu a mapování pro všechny instance typu budou odstraněna.</span><span class="sxs-lookup"><span data-stu-id="caa74-201">When you delete a complex type, the type is deleted from the conceptual model and mappings for all instances of the type are deleted.</span></span> <span data-ttu-id="caa74-202">Odkazy na tento typ však nejsou aktualizovány.</span><span class="sxs-lookup"><span data-stu-id="caa74-202">However, references to the type are not updated.</span></span> <span data-ttu-id="caa74-203">Například Pokud entita má vlastnost komplexního typu typu ComplexType1 a ComplexType1 je v **prohlížeči modelů**odstraněna, není aktualizována odpovídající vlastnost entity.</span><span class="sxs-lookup"><span data-stu-id="caa74-203">For example, if an entity has a complex type property of type ComplexType1 and ComplexType1 is deleted in the **Model Browser**, the corresponding entity property is not updated.</span></span> <span data-ttu-id="caa74-204">Model nebude ověřen, protože obsahuje entitu, která odkazuje na odstraněný komplexní typ.</span><span class="sxs-lookup"><span data-stu-id="caa74-204">The model will not validate  because it contains an entity that references a deleted complex type.</span></span> <span data-ttu-id="caa74-205">Odkazy na odstraněné komplexní typy můžete aktualizovat nebo odstranit pomocí Entity Designer.</span><span class="sxs-lookup"><span data-stu-id="caa74-205">You can update or delete references to deleted complex types by using the Entity Designer.</span></span>

-   <span data-ttu-id="caa74-206">V prohlížeči modelů klikněte pravým tlačítkem na komplexní typ a vyberte **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="caa74-206">Right-click a complex type in the Model Browser and select **Delete**.</span></span>

- <span data-ttu-id="caa74-207">Ani</span><span class="sxs-lookup"><span data-stu-id="caa74-207">OR -</span></span>

-   <span data-ttu-id="caa74-208">V prohlížeči modelů vyberte složitý typ a stiskněte klávesu DELETE na klávesnici.</span><span class="sxs-lookup"><span data-stu-id="caa74-208">Select a complex type in the Model Browser and press the Delete key on your keyboard.</span></span>

## <a name="query-for-entities-containing-properties-of-complex-type"></a><span data-ttu-id="caa74-209">Dotaz na entity obsahující vlastnosti komplexního typu</span><span class="sxs-lookup"><span data-stu-id="caa74-209">Query for Entities Containing Properties of Complex Type</span></span>

<span data-ttu-id="caa74-210">Následující kód ukazuje, jak spustit dotaz, který vrací kolekci objektů typu entity, které obsahují vlastnost komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="caa74-210">The following code shows how to execute a query that returns a collection of entity type objects that contain a complex type property.</span></span>

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
