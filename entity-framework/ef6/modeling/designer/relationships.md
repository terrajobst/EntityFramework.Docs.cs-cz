---
title: Relace – EF designeru - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 402fe960-754b-470f-976b-e5de3e9986b5
ms.openlocfilehash: d429c39dafbf183caabdc85748c188deb8dd6f66
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490648"
---
# <a name="relationships---ef-designer"></a><span data-ttu-id="dc929-102">Relace – EF designeru</span><span class="sxs-lookup"><span data-stu-id="dc929-102">Relationships - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="dc929-103">Tato stránka obsahuje informace o nastavení relace v modelu pomocí EF designeru.</span><span class="sxs-lookup"><span data-stu-id="dc929-103">This page provides information about setting up relationships in your model using the EF Designer.</span></span> <span data-ttu-id="dc929-104">Obecné informace o relacích v EF a jak přistupovat k a manipulaci s daty pomocí relací najdete v tématu [vztahy a navigačních vlastností](~/ef6/fundamentals/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="dc929-104">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).</span></span>

<span data-ttu-id="dc929-105">Přidružení definovat vztahy mezi typy entit v modelu.</span><span class="sxs-lookup"><span data-stu-id="dc929-105">Associations define relationships between entity types in a model.</span></span> <span data-ttu-id="dc929-106">Toto téma ukazuje, jak mapovat přidružení se Návrhář Entity Framework (EF designeru).</span><span class="sxs-lookup"><span data-stu-id="dc929-106">This topic shows how to map associations with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="dc929-107">Následující obrázek znázorňuje hlavní windows, které se používají při práci s EF designeru.</span><span class="sxs-lookup"><span data-stu-id="dc929-107">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EF designeru](~/ef6/media/efdesigner.png)

> [!NOTE]
> <span data-ttu-id="dc929-109">Při vytváření konceptuální model, upozornění na nenamapované entit a přidružení může zobrazit v seznamu chyb.</span><span class="sxs-lookup"><span data-stu-id="dc929-109">When you build the conceptual model, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="dc929-110">Tato upozornění můžete ignorovat, protože až zvolíte, aby se vygenerovala databáze z modelu, chyby zmizí.</span><span class="sxs-lookup"><span data-stu-id="dc929-110">You can ignore these warnings because after you choose to generate the database from the model, the errors will go away.</span></span>

## <a name="associations-overview"></a><span data-ttu-id="dc929-111">Přehled přidružení</span><span class="sxs-lookup"><span data-stu-id="dc929-111">Associations Overview</span></span>

<span data-ttu-id="dc929-112">Při návrhu modelu pomocí EF designeru soubor .edmx představuje model.</span><span class="sxs-lookup"><span data-stu-id="dc929-112">When you design your model using the EF Designer, an .edmx file represents your model.</span></span> <span data-ttu-id="dc929-113">V souboru .edmx **přidružení** element definuje vztah mezi dvěma typy entit.</span><span class="sxs-lookup"><span data-stu-id="dc929-113">In the .edmx file, an **Association** element defines a relationship between two entity types.</span></span> <span data-ttu-id="dc929-114">Asociace musíte zadat typy entit, které jsou zahrnuty v relaci a možný počet typů entit na každém konci vztahu, který je označován jako násobnost.</span><span class="sxs-lookup"><span data-stu-id="dc929-114">An association must specify the entity types that are involved in the relationship and the possible number of entity types at each end of the relationship, which is known as the multiplicity.</span></span> <span data-ttu-id="dc929-115">Násobnost zakončení přidružení může mít hodnotu jedna (1), žádný nebo jeden (0..1) nebo mnoho (\*).</span><span class="sxs-lookup"><span data-stu-id="dc929-115">The multiplicity of an association end can have a value of one (1), zero or one (0..1), or many (\*).</span></span> <span data-ttu-id="dc929-116">Tato informace zadána v dva podřízené **End** elementy.</span><span class="sxs-lookup"><span data-stu-id="dc929-116">This information is specified in two child **End** elements.</span></span>

<span data-ttu-id="dc929-117">Za běhu instance typu entity na jednom konci asociace je přístupný prostřednictvím vlastnosti navigace nebo cizí klíče (Pokud se rozhodnete vystavit cizí klíče v entitách).</span><span class="sxs-lookup"><span data-stu-id="dc929-117">At run time, entity type instances at one end of an association can be accessed through navigation properties or foreign keys (if you choose to expose foreign keys in your entities).</span></span> <span data-ttu-id="dc929-118">S vystavený cizí klíče, je vztah mezi entitami spravované pomocí **elementu ReferentialConstraint** – element (podřízený prvek **přidružení** element).</span><span class="sxs-lookup"><span data-stu-id="dc929-118">With foreign keys exposed, the relationship between the entities is managed with a **ReferentialConstraint** element (a child element of the **Association** element).</span></span> <span data-ttu-id="dc929-119">Doporučujeme vždy vystavit cizí klíče u relací v entitách.</span><span class="sxs-lookup"><span data-stu-id="dc929-119">It is recommended that you always expose foreign keys for relationships in your entities.</span></span>

> [!NOTE]
> <span data-ttu-id="dc929-120">V m (\*:\*) cizí klíče nelze přidat do entity.</span><span class="sxs-lookup"><span data-stu-id="dc929-120">In many-to-many (\*:\*) you cannot add foreign keys to the entities.</span></span> <span data-ttu-id="dc929-121">V \*:\* vztah, informací o přidružení se spravuje pomocí nezávislý objekt.</span><span class="sxs-lookup"><span data-stu-id="dc929-121">In a \*:\* relationship, the association information is managed with an independent object.</span></span>

<span data-ttu-id="dc929-122">Informace o prvcích CSDL (**elementu ReferentialConstraint**, **přidružení**atd) najdete v článku [specifikace CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="dc929-122">For information about CSDL elements (**ReferentialConstraint**, **Association**, etc.) see the [CSDL specification](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span></span>

## <a name="create-and-delete-associations"></a><span data-ttu-id="dc929-123">Vytvářet a odstraňovat přidružení</span><span class="sxs-lookup"><span data-stu-id="dc929-123">Create and Delete Associations</span></span>

<span data-ttu-id="dc929-124">Vytváření přidružení k EF designeru aktualizace modelu obsahu souboru .edmx.</span><span class="sxs-lookup"><span data-stu-id="dc929-124">Creating an association with the EF Designer updates the model content of the .edmx file.</span></span> <span data-ttu-id="dc929-125">Po vytvoření asociace, musíte vytvořit mapování pro přidružení (popsáno dále v tomto tématu).</span><span class="sxs-lookup"><span data-stu-id="dc929-125">After creating an association, you must create the mappings for the association (discussed later in this topic).</span></span>

> [!NOTE]
> <span data-ttu-id="dc929-126">V této části se předpokládá, že jste už přidali entity, které chcete vytvořit přidružení mezi do modelu.</span><span class="sxs-lookup"><span data-stu-id="dc929-126">This section assumes that you already added the entities you wish to create an association between to your model.</span></span>

### <a name="to-create-an-association"></a><span data-ttu-id="dc929-127">Vytvoření přidružení</span><span class="sxs-lookup"><span data-stu-id="dc929-127">To create an association</span></span>

1.  <span data-ttu-id="dc929-128">Klikněte pravým tlačítkem na prázdnou oblast návrhové plochy, přejděte na **přidat nový**a vyberte **přidružení...** .</span><span class="sxs-lookup"><span data-stu-id="dc929-128">Right-click an empty area of the design surface, point to **Add New**, and select **Association…**.</span></span>
2.  <span data-ttu-id="dc929-129">Zadejte nastavení pro přidružení v **Přidat přidružení** dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="dc929-129">Fill in the settings for the association in the **Add Association** dialog.</span></span>

    ![Přidat přidružení](~/ef6/media/addassociation.png)

    > [!NOTE]
    > <span data-ttu-id="dc929-131">Můžete také bez přidání navigační vlastnosti nebo vlastnosti cizího klíče k entitám stranách asociace zrušením \*\* navigační vlastnost \*\* a \*\* přidat vlastnosti cizího klíče &lt;názvu typu entity&gt; Entity \*\* Zaškrtávací políčka.</span><span class="sxs-lookup"><span data-stu-id="dc929-131">You can choose to not add navigation properties or foreign key properties to the entities at the ends of the association by clearing the \*\*Navigation Property \*\*and \*\*Add foreign key properties to the &lt;entity type name&gt; Entity \*\*checkboxes.</span></span> <span data-ttu-id="dc929-132">Pokud chcete přidat pouze jednu vlastnost navigace, přidružení bude pouze v jednom směru umožňujících přechody.</span><span class="sxs-lookup"><span data-stu-id="dc929-132">If you add only one navigation property, the association will be traversable in only one direction.</span></span> <span data-ttu-id="dc929-133">Pokud chcete přidat žádné navigační vlastnosti, musíte zvolit přidání vlastnosti cizího klíče pro přístup k entitám stranách asociace.</span><span class="sxs-lookup"><span data-stu-id="dc929-133">If you add no navigation properties, you must choose to add foreign key properties in order to access entities at the ends of the association.</span></span>
    
3.  <span data-ttu-id="dc929-134">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="dc929-134">Click **OK**.</span></span>

### <a name="to-delete-an-association"></a><span data-ttu-id="dc929-135">Odstranit spojení</span><span class="sxs-lookup"><span data-stu-id="dc929-135">To delete an association</span></span>

<span data-ttu-id="dc929-136">Chcete-li odstranit přidružení proveďte jednu z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="dc929-136">To delete an association do one of the following:</span></span>

-   <span data-ttu-id="dc929-137">Klikněte pravým tlačítkem na asociace v EF designeru ploše a vyberte možnost **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="dc929-137">Right-click the association on the EF Designer surface and select **Delete**.</span></span>

- <span data-ttu-id="dc929-138">OR –</span><span class="sxs-lookup"><span data-stu-id="dc929-138">OR -</span></span>

-   <span data-ttu-id="dc929-139">Vyberte jeden nebo více přidružení a stiskněte klávesu DELETE.</span><span class="sxs-lookup"><span data-stu-id="dc929-139">Select one or more associations and press the DELETE key.</span></span>

## <a name="include-foreign-key-properties-in-your-entities-referential-constraints"></a><span data-ttu-id="dc929-140">Zahrnují vlastnosti cizího klíče v entitách (referenční omezení)</span><span class="sxs-lookup"><span data-stu-id="dc929-140">Include Foreign Key Properties in Your Entities (Referential Constraints)</span></span>

<span data-ttu-id="dc929-141">Doporučujeme vždy vystavit cizí klíče u relací v entitách.</span><span class="sxs-lookup"><span data-stu-id="dc929-141">It is recommended that you always expose foreign keys for relationships in your entities.</span></span> <span data-ttu-id="dc929-142">Entity Framework používá referenční omezení k určení, že vlastnost funguje jako cizí klíč pro relaci.</span><span class="sxs-lookup"><span data-stu-id="dc929-142">Entity Framework uses a referential constraint to identify that a property acts as the foreign key for a relationship.</span></span>

<span data-ttu-id="dc929-143">Pokud jste zaškrtli možnost ***přidat vlastnosti cizího klíče &lt;názvu typu entity&gt; Entity*** zaškrtávací políčko při vytvoření relace, pro vás byl přidán tento referenční omezení.</span><span class="sxs-lookup"><span data-stu-id="dc929-143">If you checked the ***Add foreign key properties to the &lt;entity type name&gt; Entity*** checkbox when creating a relationship, this referential constraint was added for you.</span></span>

<span data-ttu-id="dc929-144">Použijete-li přidat nebo upravit referenční omezení EF designeru, EF designeru přidá nebo upraví **elementu ReferentialConstraint** prvek CSDL obsah souboru .edmx.</span><span class="sxs-lookup"><span data-stu-id="dc929-144">When you use the EF Designer to add or edit a referential constraint, the EF Designer adds or modifies a **ReferentialConstraint** element in the CSDL content of the .edmx file.</span></span>

-   <span data-ttu-id="dc929-145">Dvakrát klikněte na panel přidružení, na kterou chcete upravit.</span><span class="sxs-lookup"><span data-stu-id="dc929-145">Double-click the association that you want to edit.</span></span>
    <span data-ttu-id="dc929-146">**Referenční omezení** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="dc929-146">The **Referential Constraint** dialog box appears.</span></span>
-   <span data-ttu-id="dc929-147">Z **hlavní** rozevíracího seznamu vyberte hlavní entity v referenčním omezení.</span><span class="sxs-lookup"><span data-stu-id="dc929-147">From the **Principal** drop-down list, select the principal entity in the referential constraint.</span></span>
    <span data-ttu-id="dc929-148">Klíčové vlastnosti entity jsou přidány do **klíč objektu** seznam v dialogovém okně.</span><span class="sxs-lookup"><span data-stu-id="dc929-148">The entity's key properties are added to the **Principal Key** list in the dialog box.</span></span>
-   <span data-ttu-id="dc929-149">Z **závislé** rozevíracího seznamu vyberte závislé entity v referenčním omezení.</span><span class="sxs-lookup"><span data-stu-id="dc929-149">From the **Dependent** drop-down list, select the dependent entity in the referential constraint.</span></span>
-   <span data-ttu-id="dc929-150">Pro každý klíč instančního objektu, který má závislé klíč, vyberte z rozevíracích seznamů v odpovídajícím klíčem závislé **závislé klíč** sloupce.</span><span class="sxs-lookup"><span data-stu-id="dc929-150">For each principal key that has a dependent key, select a corresponding dependent key from the drop-down lists in the **Dependent Key** column.</span></span>

    ![Omezení REF](~/ef6/media/refconstraint.png)

-   <span data-ttu-id="dc929-152">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="dc929-152">Click **OK**.</span></span>

## <a name="create-and-edit-association-mappings"></a><span data-ttu-id="dc929-153">Vytvořte a upravte mapování přidružení</span><span class="sxs-lookup"><span data-stu-id="dc929-153">Create and Edit Association Mappings</span></span>

<span data-ttu-id="dc929-154">Můžete určit, jak mapuje přidružení k databázi **podrobnosti mapování** okno EF designeru.</span><span class="sxs-lookup"><span data-stu-id="dc929-154">You can specify how an association maps to the database in the **Mapping Details** window of the EF Designer.</span></span>

> [!NOTE]
> <span data-ttu-id="dc929-155">Můžete namapovat pouze podrobnosti pro přidružení, které nemají zadaný referenční omezení.</span><span class="sxs-lookup"><span data-stu-id="dc929-155">You can only map details for the associations that do not have a referential constraint specified.</span></span> <span data-ttu-id="dc929-156">Pokud je zadáno referenční omezení vlastností cizího klíče je zahrnuta v entitě a podrobnosti o mapování můžete použít entity na sloupec cizího klíče mapuje na ovládací prvek.</span><span class="sxs-lookup"><span data-stu-id="dc929-156">If a referential constraint is specified then a foreign key property is included in the entity and you can use the Mapping Details for the entity to control which column the foreign key maps to.</span></span>

### <a name="create-an-association-mapping"></a><span data-ttu-id="dc929-157">Vytvoření mapování přidružení</span><span class="sxs-lookup"><span data-stu-id="dc929-157">Create an association mapping</span></span>

-   <span data-ttu-id="dc929-158">Klikněte pravým tlačítkem na přidružení v návrhové ploše a vyberte **mapování tabulky,**.</span><span class="sxs-lookup"><span data-stu-id="dc929-158">Right-click an association in the design surface and select **Table Mapping**.</span></span>
    <span data-ttu-id="dc929-159">Zobrazí se přidružení mapování v **podrobnosti mapování** okna.</span><span class="sxs-lookup"><span data-stu-id="dc929-159">This displays the association mapping in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="dc929-160">Klikněte na tlačítko **přidat tabulku nebo zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="dc929-160">Click **Add a Table or View**.</span></span>
    <span data-ttu-id="dc929-161">Se zobrazí rozevírací seznam obsahující všechny tabulky v modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="dc929-161">A drop-down list appears that includes all the tables in the storage model.</span></span>
-   <span data-ttu-id="dc929-162">Vyberte tabulku, do kterého se namapuje přidružení.</span><span class="sxs-lookup"><span data-stu-id="dc929-162">Select the table to which the association will map.</span></span>
    <span data-ttu-id="dc929-163">**Podrobnosti mapování** okně se zobrazí oba konce přidružení a klíčové vlastnosti pro typ entity v každé **End**.</span><span class="sxs-lookup"><span data-stu-id="dc929-163">The **Mapping Details** window displays both ends of the association and the key properties for the entity type at each **End**.</span></span>
-   <span data-ttu-id="dc929-164">Pro každou vlastnost klíče, klikněte na tlačítko **sloupec** pole a vyberte sloupec, do kterého se namapuje vlastnost.</span><span class="sxs-lookup"><span data-stu-id="dc929-164">For each key property, click the **Column** field, and select the column to which the property will map.</span></span>

    ![Podrobnosti mapování 4](~/ef6/media/mappingdetails4.png)

### <a name="edit-an-association-mapping"></a><span data-ttu-id="dc929-166">Upravit mapování přidružení</span><span class="sxs-lookup"><span data-stu-id="dc929-166">Edit an association mapping</span></span>

-   <span data-ttu-id="dc929-167">Klikněte pravým tlačítkem na přidružení v návrhové ploše a vyberte **mapování tabulky,**.</span><span class="sxs-lookup"><span data-stu-id="dc929-167">Right-click an association in the design surface and select **Table Mapping**.</span></span>
    <span data-ttu-id="dc929-168">Zobrazí se přidružení mapování v **podrobnosti mapování** okna.</span><span class="sxs-lookup"><span data-stu-id="dc929-168">This displays the association mapping in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="dc929-169">Klikněte na tlačítko **mapuje &lt;název tabulky&gt;**.</span><span class="sxs-lookup"><span data-stu-id="dc929-169">Click **Maps to &lt;Table Name&gt;**.</span></span>
    <span data-ttu-id="dc929-170">Se zobrazí rozevírací seznam obsahující všechny tabulky v modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="dc929-170">A drop-down list appears that includes all the tables in the storage model.</span></span>
-   <span data-ttu-id="dc929-171">Vyberte tabulku, do kterého se namapuje přidružení.</span><span class="sxs-lookup"><span data-stu-id="dc929-171">Select the table to which the association will map.</span></span>
    <span data-ttu-id="dc929-172">**Podrobnosti mapování** okně se zobrazí oba konce přidružení a klíčové vlastnosti typu entity na každém konci.</span><span class="sxs-lookup"><span data-stu-id="dc929-172">The **Mapping Details** window displays both ends of the association and the key properties for the entity type at each End.</span></span>
-   <span data-ttu-id="dc929-173">Pro každou vlastnost klíče, klikněte na tlačítko **sloupec** pole a vyberte sloupec, do kterého se namapuje vlastnost.</span><span class="sxs-lookup"><span data-stu-id="dc929-173">For each key property, click the **Column** field, and select the column to which the property will map.</span></span>

## <a name="edit-and-delete-navigation-properties"></a><span data-ttu-id="dc929-174">Upravit a odstranit navigační vlastnosti</span><span class="sxs-lookup"><span data-stu-id="dc929-174">Edit and Delete Navigation Properties</span></span>

<span data-ttu-id="dc929-175">Navigační vlastnosti jsou místní vlastnosti, které se používají pro vyhledání entity na konci asociace v modelu.</span><span class="sxs-lookup"><span data-stu-id="dc929-175">Navigation properties are shortcut properties that are used to locate the entities at the ends of an association in a model.</span></span> <span data-ttu-id="dc929-176">Navigační vlastnosti lze vytvořit při vytvoření přidružení mezi dvěma typy entit.</span><span class="sxs-lookup"><span data-stu-id="dc929-176">Navigation properties can be created when you create an association between two entity types.</span></span>

#### <a name="to-edit-navigation-properties"></a><span data-ttu-id="dc929-177">Chcete-li upravit vlastnosti navigace</span><span class="sxs-lookup"><span data-stu-id="dc929-177">To edit navigation properties</span></span>

-   <span data-ttu-id="dc929-178">Vyberte vlastnost navigace na povrchu EF designeru.</span><span class="sxs-lookup"><span data-stu-id="dc929-178">Select a navigation property on the EF Designer surface.</span></span>
    <span data-ttu-id="dc929-179">Informace o vlastnosti navigace se zobrazí v sadě Visual Studio **vlastnosti** okna.</span><span class="sxs-lookup"><span data-stu-id="dc929-179">Information about the navigation property is displayed in the Visual Studio **Properties** window.</span></span>
-   <span data-ttu-id="dc929-180">Změňte nastavení vlastnosti **vlastnosti** okna.</span><span class="sxs-lookup"><span data-stu-id="dc929-180">Change the property settings in the **Properties** window.</span></span>

#### <a name="to-delete-navigation-properties"></a><span data-ttu-id="dc929-181">Chcete-li odstranit navigační vlastnosti</span><span class="sxs-lookup"><span data-stu-id="dc929-181">To delete navigation properties</span></span>

-   <span data-ttu-id="dc929-182">Pokud cizí klíče nejsou přístupná na typy entit v konceptuálním modelu, odstranění navigační vlastnost, aby odpovídající přidružení pouze v jednom směru umožňujících přechody nebo není umožňujících přechody vůbec.</span><span class="sxs-lookup"><span data-stu-id="dc929-182">If foreign keys are not exposed on entity types in the conceptual model, deleting a navigation property may make the corresponding association traversable in only one direction or not traversable at all.</span></span>
-   <span data-ttu-id="dc929-183">Klikněte pravým tlačítkem na vlastnost navigace v EF designeru ploše a vyberte možnost **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="dc929-183">Right-click a navigation property on the EF Designer surface and select **Delete**.</span></span>
