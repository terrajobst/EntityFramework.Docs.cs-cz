---
title: Relace – Návrhář EF – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 402fe960-754b-470f-976b-e5de3e9986b5
ms.openlocfilehash: d429c39dafbf183caabdc85748c188deb8dd6f66
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418243"
---
# <a name="relationships---ef-designer"></a><span data-ttu-id="14f0b-102">Vztahy – Návrhář EF</span><span class="sxs-lookup"><span data-stu-id="14f0b-102">Relationships - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="14f0b-103">Tato stránka poskytuje informace o nastavení vztahů v modelu pomocí návrháře EF.</span><span class="sxs-lookup"><span data-stu-id="14f0b-103">This page provides information about setting up relationships in your model using the EF Designer.</span></span> <span data-ttu-id="14f0b-104">Obecné informace o relacích v EF a o tom, jak přistupovat k datům a pracovat s nimi pomocí vztahů, najdete v tématu [relace & vlastnosti navigace](~/ef6/fundamentals/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="14f0b-104">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).</span></span>

<span data-ttu-id="14f0b-105">Přidružení definují vztahy mezi typy entit v modelu.</span><span class="sxs-lookup"><span data-stu-id="14f0b-105">Associations define relationships between entity types in a model.</span></span> <span data-ttu-id="14f0b-106">Toto téma ukazuje, jak mapovat asociace pomocí Entity Framework Designer (EF Designer).</span><span class="sxs-lookup"><span data-stu-id="14f0b-106">This topic shows how to map associations with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="14f0b-107">Následující obrázek ukazuje hlavní okna, která se používají při práci s návrhářem EF.</span><span class="sxs-lookup"><span data-stu-id="14f0b-107">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![Návrhář EF](~/ef6/media/efdesigner.png)

> [!NOTE]
> <span data-ttu-id="14f0b-109">Při sestavování koncepčního modelu se může zobrazit upozornění týkající se nemapovaných entit a přidružení v Seznam chyb.</span><span class="sxs-lookup"><span data-stu-id="14f0b-109">When you build the conceptual model, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="14f0b-110">Tato upozornění můžete ignorovat, protože po výběru databáze z modelu budou tyto chyby pryč.</span><span class="sxs-lookup"><span data-stu-id="14f0b-110">You can ignore these warnings because after you choose to generate the database from the model, the errors will go away.</span></span>

## <a name="associations-overview"></a><span data-ttu-id="14f0b-111">Přehled přidružení</span><span class="sxs-lookup"><span data-stu-id="14f0b-111">Associations Overview</span></span>

<span data-ttu-id="14f0b-112">Při návrhu modelu pomocí návrháře EF představuje soubor. edmx váš model.</span><span class="sxs-lookup"><span data-stu-id="14f0b-112">When you design your model using the EF Designer, an .edmx file represents your model.</span></span> <span data-ttu-id="14f0b-113">V souboru EDMX definuje element **Association** vztah mezi dvěma typy entit.</span><span class="sxs-lookup"><span data-stu-id="14f0b-113">In the .edmx file, an **Association** element defines a relationship between two entity types.</span></span> <span data-ttu-id="14f0b-114">Přidružení musí určovat typy entit, které jsou součástí relace, a možný počet typů entit na každém konci relace, který se označuje jako násobnost.</span><span class="sxs-lookup"><span data-stu-id="14f0b-114">An association must specify the entity types that are involved in the relationship and the possible number of entity types at each end of the relationship, which is known as the multiplicity.</span></span> <span data-ttu-id="14f0b-115">Násobnost elementu end přidružení může mít hodnotu jedna (1), 0 nebo 1 (0.. 1) nebo mnoho (\*).</span><span class="sxs-lookup"><span data-stu-id="14f0b-115">The multiplicity of an association end can have a value of one (1), zero or one (0..1), or many (\*).</span></span> <span data-ttu-id="14f0b-116">Tyto informace jsou zadány ve dvou podřízených elementech **End** .</span><span class="sxs-lookup"><span data-stu-id="14f0b-116">This information is specified in two child **End** elements.</span></span>

<span data-ttu-id="14f0b-117">V době běhu lze k instancím typu entity na jednom konci přidružit prostřednictvím vlastností navigace nebo cizích klíčů (Pokud se rozhodnete vystavit cizí klíče ve svých entitách).</span><span class="sxs-lookup"><span data-stu-id="14f0b-117">At run time, entity type instances at one end of an association can be accessed through navigation properties or foreign keys (if you choose to expose foreign keys in your entities).</span></span> <span data-ttu-id="14f0b-118">Při zpřístupnění cizích klíčů je vztah mezi entitami spravován pomocí elementu **elementu ReferentialConstraint** (podřízený element elementu **Association** ).</span><span class="sxs-lookup"><span data-stu-id="14f0b-118">With foreign keys exposed, the relationship between the entities is managed with a **ReferentialConstraint** element (a child element of the **Association** element).</span></span> <span data-ttu-id="14f0b-119">Doporučuje se, abyste vždycky vystavili cizí klíče pro vztahy ve svých entitách.</span><span class="sxs-lookup"><span data-stu-id="14f0b-119">It is recommended that you always expose foreign keys for relationships in your entities.</span></span>

> [!NOTE]
> <span data-ttu-id="14f0b-120">V m:n (\*:\*) nemůžete přidávat cizí klíče k entitám.</span><span class="sxs-lookup"><span data-stu-id="14f0b-120">In many-to-many (\*:\*) you cannot add foreign keys to the entities.</span></span> <span data-ttu-id="14f0b-121">Ve vztahu \*:\* jsou informace o přidružení spravovány pomocí nezávislého objektu.</span><span class="sxs-lookup"><span data-stu-id="14f0b-121">In a \*:\* relationship, the association information is managed with an independent object.</span></span>

<span data-ttu-id="14f0b-122">Informace o prvcích CSDL (**elementu ReferentialConstraint**, **Association**atd.) najdete ve [specifikaci CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="14f0b-122">For information about CSDL elements (**ReferentialConstraint**, **Association**, etc.) see the [CSDL specification](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span></span>

## <a name="create-and-delete-associations"></a><span data-ttu-id="14f0b-123">Vytváření a odstraňování přidružení</span><span class="sxs-lookup"><span data-stu-id="14f0b-123">Create and Delete Associations</span></span>

<span data-ttu-id="14f0b-124">Vytvořením přidružení s návrhářem EF se aktualizuje obsah modelu souboru. edmx.</span><span class="sxs-lookup"><span data-stu-id="14f0b-124">Creating an association with the EF Designer updates the model content of the .edmx file.</span></span> <span data-ttu-id="14f0b-125">Po vytvoření přidružení je nutné vytvořit mapování pro přidružení (popsáno dále v tomto tématu).</span><span class="sxs-lookup"><span data-stu-id="14f0b-125">After creating an association, you must create the mappings for the association (discussed later in this topic).</span></span>

> [!NOTE]
> <span data-ttu-id="14f0b-126">V této části se předpokládá, že už jste přidali entity, pro které chcete vytvořit přidružení mezi modelem.</span><span class="sxs-lookup"><span data-stu-id="14f0b-126">This section assumes that you already added the entities you wish to create an association between to your model.</span></span>

### <a name="to-create-an-association"></a><span data-ttu-id="14f0b-127">Vytvoření přidružení</span><span class="sxs-lookup"><span data-stu-id="14f0b-127">To create an association</span></span>

1.  <span data-ttu-id="14f0b-128">Klikněte pravým tlačítkem myši na prázdnou oblast návrhové plochy, přejděte na **Přidat nový**a vyberte **asociace...** .</span><span class="sxs-lookup"><span data-stu-id="14f0b-128">Right-click an empty area of the design surface, point to **Add New**, and select **Association…**.</span></span>
2.  <span data-ttu-id="14f0b-129">Do dialogového okna **Přidat přidružení** zadejte nastavení pro přidružení.</span><span class="sxs-lookup"><span data-stu-id="14f0b-129">Fill in the settings for the association in the **Add Association** dialog.</span></span>

    ![Přidat přidružení](~/ef6/media/addassociation.png)

    > [!NOTE]
    > <span data-ttu-id="14f0b-131">Můžete se rozhodnout, že do entit na koncích přidružení nepřidáte vlastnosti navigace nebo cizí klíč, a to tak, že zrušíte **navigační vlastnost **a **přidáte vlastnosti cizího klíče do &lt;název typu entity&gt; **zaškrtávací políčka entit.</span><span class="sxs-lookup"><span data-stu-id="14f0b-131">You can choose to not add navigation properties or foreign key properties to the entities at the ends of the association by clearing the **Navigation Property **and **Add foreign key properties to the &lt;entity type name&gt; Entity **checkboxes.</span></span> <span data-ttu-id="14f0b-132">Pokud přidáte jenom jednu navigační vlastnost, bude možné toto přidružení procházet jenom v jednom směru.</span><span class="sxs-lookup"><span data-stu-id="14f0b-132">If you add only one navigation property, the association will be traversable in only one direction.</span></span> <span data-ttu-id="14f0b-133">Pokud nepřidáte žádné navigační vlastnosti, musíte se rozhodnout přidat vlastnosti cizího klíče, aby bylo možné přistupovat k entitám na koncích přidružení.</span><span class="sxs-lookup"><span data-stu-id="14f0b-133">If you add no navigation properties, you must choose to add foreign key properties in order to access entities at the ends of the association.</span></span>
    
3.  <span data-ttu-id="14f0b-134">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="14f0b-134">Click **OK**.</span></span>

### <a name="to-delete-an-association"></a><span data-ttu-id="14f0b-135">Odstranění přidružení</span><span class="sxs-lookup"><span data-stu-id="14f0b-135">To delete an association</span></span>

<span data-ttu-id="14f0b-136">Chcete-li odstranit přidružení, proveďte jednu z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="14f0b-136">To delete an association do one of the following:</span></span>

-   <span data-ttu-id="14f0b-137">Klikněte pravým tlačítkem na přidružení na povrchu návrháře EF a vyberte **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="14f0b-137">Right-click the association on the EF Designer surface and select **Delete**.</span></span>

- <span data-ttu-id="14f0b-138">Ani</span><span class="sxs-lookup"><span data-stu-id="14f0b-138">OR -</span></span>

-   <span data-ttu-id="14f0b-139">Vyberte jedno nebo více přidružení a stiskněte klávesu DELETE.</span><span class="sxs-lookup"><span data-stu-id="14f0b-139">Select one or more associations and press the DELETE key.</span></span>

## <a name="include-foreign-key-properties-in-your-entities-referential-constraints"></a><span data-ttu-id="14f0b-140">Zahrnutí vlastností cizího klíče do entit (referenční omezení)</span><span class="sxs-lookup"><span data-stu-id="14f0b-140">Include Foreign Key Properties in Your Entities (Referential Constraints)</span></span>

<span data-ttu-id="14f0b-141">Doporučuje se, abyste vždycky vystavili cizí klíče pro vztahy ve svých entitách.</span><span class="sxs-lookup"><span data-stu-id="14f0b-141">It is recommended that you always expose foreign keys for relationships in your entities.</span></span> <span data-ttu-id="14f0b-142">Entity Framework používá referenční omezení k identifikaci, že vlastnost slouží jako cizí klíč pro relaci.</span><span class="sxs-lookup"><span data-stu-id="14f0b-142">Entity Framework uses a referential constraint to identify that a property acts as the foreign key for a relationship.</span></span>

<span data-ttu-id="14f0b-143">Pokud jste zaškrtli políčko ***Přidat vlastnosti cizího klíče do pole &lt;název typu entity&gt; entitu*** při vytváření relace, toto referenční omezení bylo přidáno za vás.</span><span class="sxs-lookup"><span data-stu-id="14f0b-143">If you checked the ***Add foreign key properties to the &lt;entity type name&gt; Entity*** checkbox when creating a relationship, this referential constraint was added for you.</span></span>

<span data-ttu-id="14f0b-144">Když použijete návrháře EF k přidání nebo úpravě referenčního omezení, Návrhář EF přidá nebo upraví element **elementu referentialconstraint** v obsahu CSDL souboru. edmx.</span><span class="sxs-lookup"><span data-stu-id="14f0b-144">When you use the EF Designer to add or edit a referential constraint, the EF Designer adds or modifies a **ReferentialConstraint** element in the CSDL content of the .edmx file.</span></span>

-   <span data-ttu-id="14f0b-145">Dvakrát klikněte na přidružení, které chcete upravit.</span><span class="sxs-lookup"><span data-stu-id="14f0b-145">Double-click the association that you want to edit.</span></span>
    <span data-ttu-id="14f0b-146">Zobrazí se dialogové okno **referenční omezení** .</span><span class="sxs-lookup"><span data-stu-id="14f0b-146">The **Referential Constraint** dialog box appears.</span></span>
-   <span data-ttu-id="14f0b-147">V rozevíracím seznamu  **zabezpečení** vyberte entitu zabezpečení v referenčním omezení.</span><span class="sxs-lookup"><span data-stu-id="14f0b-147">From the **Principal** drop-down list, select the principal entity in the referential constraint.</span></span>
    <span data-ttu-id="14f0b-148">Klíčové vlastnosti entity se přidají do seznamu **hlavních klíčů** v dialogovém okně.</span><span class="sxs-lookup"><span data-stu-id="14f0b-148">The entity's key properties are added to the **Principal Key** list in the dialog box.</span></span>
-   <span data-ttu-id="14f0b-149">V rozevíracím seznamu **závislé** vyberte entitu Dependent v referenčním omezení.</span><span class="sxs-lookup"><span data-stu-id="14f0b-149">From the **Dependent** drop-down list, select the dependent entity in the referential constraint.</span></span>
-   <span data-ttu-id="14f0b-150">V případě každého hlavního klíče, který má závislý klíč, vyberte odpovídající klíč závislý v rozevíracích seznamech ve sloupci **závislého klíče** .</span><span class="sxs-lookup"><span data-stu-id="14f0b-150">For each principal key that has a dependent key, select a corresponding dependent key from the drop-down lists in the **Dependent Key** column.</span></span>

    ![Omezení ref](~/ef6/media/refconstraint.png)

-   <span data-ttu-id="14f0b-152">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="14f0b-152">Click **OK**.</span></span>

## <a name="create-and-edit-association-mappings"></a><span data-ttu-id="14f0b-153">Vytvoření a úprava mapování přidružení</span><span class="sxs-lookup"><span data-stu-id="14f0b-153">Create and Edit Association Mappings</span></span>

<span data-ttu-id="14f0b-154">Můžete určit způsob, jakým se přidružení mapuje k databázi v okně **Podrobnosti mapování** v Návrháři EF.</span><span class="sxs-lookup"><span data-stu-id="14f0b-154">You can specify how an association maps to the database in the **Mapping Details** window of the EF Designer.</span></span>

> [!NOTE]
> <span data-ttu-id="14f0b-155">Můžete mapovat pouze podrobnosti o přidruženích, u kterých není zadáno referenční omezení.</span><span class="sxs-lookup"><span data-stu-id="14f0b-155">You can only map details for the associations that do not have a referential constraint specified.</span></span> <span data-ttu-id="14f0b-156">Pokud je zadáno referenční omezení, je v entitě obsažena vlastnost cizího klíče a můžete použít Podrobnosti mapování pro entitu k určení, ke kterému sloupci se cizí klíč mapuje.</span><span class="sxs-lookup"><span data-stu-id="14f0b-156">If a referential constraint is specified then a foreign key property is included in the entity and you can use the Mapping Details for the entity to control which column the foreign key maps to.</span></span>

### <a name="create-an-association-mapping"></a><span data-ttu-id="14f0b-157">Vytvoření mapování přidružení</span><span class="sxs-lookup"><span data-stu-id="14f0b-157">Create an association mapping</span></span>

-   <span data-ttu-id="14f0b-158">Klikněte pravým tlačítkem na přidružení na návrhové ploše a vyberte **mapování tabulky**.</span><span class="sxs-lookup"><span data-stu-id="14f0b-158">Right-click an association in the design surface and select **Table Mapping**.</span></span>
    <span data-ttu-id="14f0b-159">V okně **Podrobnosti mapování** se zobrazí mapování přidružení.</span><span class="sxs-lookup"><span data-stu-id="14f0b-159">This displays the association mapping in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="14f0b-160">Klikněte na **Přidat tabulku nebo zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="14f0b-160">Click **Add a Table or View**.</span></span>
    <span data-ttu-id="14f0b-161">Zobrazí se rozevírací seznam, který obsahuje všechny tabulky v modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="14f0b-161">A drop-down list appears that includes all the tables in the storage model.</span></span>
-   <span data-ttu-id="14f0b-162">Vyberte tabulku, do které bude mapování asociace namapováno.</span><span class="sxs-lookup"><span data-stu-id="14f0b-162">Select the table to which the association will map.</span></span>
    <span data-ttu-id="14f0b-163">Okno **Podrobnosti mapování** zobrazuje obě konce přidružení a klíčové vlastnosti pro typ entity na každém **konci**.</span><span class="sxs-lookup"><span data-stu-id="14f0b-163">The **Mapping Details** window displays both ends of the association and the key properties for the entity type at each **End**.</span></span>
-   <span data-ttu-id="14f0b-164">Pro každou klíčovou vlastnost klikněte na pole **sloupec** a vyberte sloupec, ke kterému bude namapována vlastnost.</span><span class="sxs-lookup"><span data-stu-id="14f0b-164">For each key property, click the **Column** field, and select the column to which the property will map.</span></span>

    ![Podrobnosti mapování 4](~/ef6/media/mappingdetails4.png)

### <a name="edit-an-association-mapping"></a><span data-ttu-id="14f0b-166">Úprava mapování přidružení</span><span class="sxs-lookup"><span data-stu-id="14f0b-166">Edit an association mapping</span></span>

-   <span data-ttu-id="14f0b-167">Klikněte pravým tlačítkem na přidružení na návrhové ploše a vyberte **mapování tabulky**.</span><span class="sxs-lookup"><span data-stu-id="14f0b-167">Right-click an association in the design surface and select **Table Mapping**.</span></span>
    <span data-ttu-id="14f0b-168">V okně **Podrobnosti mapování** se zobrazí mapování přidružení.</span><span class="sxs-lookup"><span data-stu-id="14f0b-168">This displays the association mapping in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="14f0b-169">Klikněte na tlačítko **mapy &lt;název tabulky&gt;** .</span><span class="sxs-lookup"><span data-stu-id="14f0b-169">Click **Maps to &lt;Table Name&gt;**.</span></span>
    <span data-ttu-id="14f0b-170">Zobrazí se rozevírací seznam, který obsahuje všechny tabulky v modelu úložiště.</span><span class="sxs-lookup"><span data-stu-id="14f0b-170">A drop-down list appears that includes all the tables in the storage model.</span></span>
-   <span data-ttu-id="14f0b-171">Vyberte tabulku, do které bude mapování asociace namapováno.</span><span class="sxs-lookup"><span data-stu-id="14f0b-171">Select the table to which the association will map.</span></span>
    <span data-ttu-id="14f0b-172">Okno **Podrobnosti mapování** zobrazuje obě konce přidružení a klíčové vlastnosti pro typ entity na každém konci.</span><span class="sxs-lookup"><span data-stu-id="14f0b-172">The **Mapping Details** window displays both ends of the association and the key properties for the entity type at each End.</span></span>
-   <span data-ttu-id="14f0b-173">Pro každou klíčovou vlastnost klikněte na pole **sloupec** a vyberte sloupec, ke kterému bude namapována vlastnost.</span><span class="sxs-lookup"><span data-stu-id="14f0b-173">For each key property, click the **Column** field, and select the column to which the property will map.</span></span>

## <a name="edit-and-delete-navigation-properties"></a><span data-ttu-id="14f0b-174">Upravit a odstranit navigační vlastnosti</span><span class="sxs-lookup"><span data-stu-id="14f0b-174">Edit and Delete Navigation Properties</span></span>

<span data-ttu-id="14f0b-175">Navigační vlastnosti jsou vlastnosti zástupce, které slouží k vyhledání entit na koncích přidružení v modelu.</span><span class="sxs-lookup"><span data-stu-id="14f0b-175">Navigation properties are shortcut properties that are used to locate the entities at the ends of an association in a model.</span></span> <span data-ttu-id="14f0b-176">Navigační vlastnosti lze vytvořit při vytváření přidružení mezi dvěma typy entit.</span><span class="sxs-lookup"><span data-stu-id="14f0b-176">Navigation properties can be created when you create an association between two entity types.</span></span>

#### <a name="to-edit-navigation-properties"></a><span data-ttu-id="14f0b-177">Úprava vlastností navigace</span><span class="sxs-lookup"><span data-stu-id="14f0b-177">To edit navigation properties</span></span>

-   <span data-ttu-id="14f0b-178">Vyberte navigační vlastnost na povrchu EF návrháře.</span><span class="sxs-lookup"><span data-stu-id="14f0b-178">Select a navigation property on the EF Designer surface.</span></span>
    <span data-ttu-id="14f0b-179">Informace o vlastnosti navigace se zobrazí v okně **vlastnosti** sady Visual Studio .</span><span class="sxs-lookup"><span data-stu-id="14f0b-179">Information about the navigation property is displayed in the Visual Studio **Properties** window.</span></span>
-   <span data-ttu-id="14f0b-180">Změňte nastavení vlastnosti v okně **vlastnosti** .</span><span class="sxs-lookup"><span data-stu-id="14f0b-180">Change the property settings in the **Properties** window.</span></span>

#### <a name="to-delete-navigation-properties"></a><span data-ttu-id="14f0b-181">Odstranění vlastností navigace</span><span class="sxs-lookup"><span data-stu-id="14f0b-181">To delete navigation properties</span></span>

-   <span data-ttu-id="14f0b-182">Pokud nejsou cizí klíče vystaveny na typech entit v koncepčním modelu, odstraněním navigační vlastnosti může být v jednom směru provázáno odpovídající přidružení.</span><span class="sxs-lookup"><span data-stu-id="14f0b-182">If foreign keys are not exposed on entity types in the conceptual model, deleting a navigation property may make the corresponding association traversable in only one direction or not traversable at all.</span></span>
-   <span data-ttu-id="14f0b-183">Klikněte pravým tlačítkem na navigační vlastnost na povrchu návrháře EF a vyberte **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="14f0b-183">Right-click a navigation property on the EF Designer surface and select **Delete**.</span></span>
