---
title: Návrhář dědičnosti TPH – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 72d26a8e-20ab-4500-bd13-394a08e73394
ms.openlocfilehash: 43ba34a98c3960a7a3052a00e2ed2751c2f2b121
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418425"
---
# <a name="designer-tph-inheritance"></a><span data-ttu-id="1f04d-102">Dědičnost typu TPH návrháře</span><span class="sxs-lookup"><span data-stu-id="1f04d-102">Designer TPH Inheritance</span></span>
<span data-ttu-id="1f04d-103">V tomto podrobném návodu se dozvíte, jak implementovat dědičnost typu Table-per-Hierarchy (TPH) ve koncepčním modelu pomocí Entity Framework Designer (EF Designer).</span><span class="sxs-lookup"><span data-stu-id="1f04d-103">This step-by-step walkthrough shows how to implement table-per-hierarchy (TPH) inheritance in your conceptual model with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="1f04d-104">Dědičnost TPH používá jednu databázovou tabulku k uchování dat pro všechny typy entit v hierarchii dědičnosti.</span><span class="sxs-lookup"><span data-stu-id="1f04d-104">TPH inheritance uses one database table to maintain data for all of the entity types in an inheritance hierarchy.</span></span>

<span data-ttu-id="1f04d-105">V tomto návodu namapujeme tabulku Person na tři typy entit: osoba (základní typ), student (odvozený od osoby) a instruktor (odvozený od osoby).</span><span class="sxs-lookup"><span data-stu-id="1f04d-105">In this walkthrough we will map the Person table to three entity types: Person (the base type), Student (derives from Person), and Instructor (derives from Person).</span></span> <span data-ttu-id="1f04d-106">Vytvoříme koncepční model z databáze (Database First) a pak upravíte model pro implementaci dědičnosti TPH pomocí návrháře EF.</span><span class="sxs-lookup"><span data-stu-id="1f04d-106">We'll create a conceptual model from the database (Database First) and then alter the model to implement the TPH inheritance using the EF Designer.</span></span>

<span data-ttu-id="1f04d-107">Je možné namapovat na dědičnost TPH pomocí Model First, ale museli byste napsat vlastní pracovní postup generování databáze, který je složitý.</span><span class="sxs-lookup"><span data-stu-id="1f04d-107">It is possible to map to a TPH inheritance using Model First but you would have to write your own database generation workflow which is complex.</span></span> <span data-ttu-id="1f04d-108">Tento pracovní postup byste pak přiřadili vlastnosti **pracovního postupu generování databáze** v Návrháři EF.</span><span class="sxs-lookup"><span data-stu-id="1f04d-108">You would then assign this workflow to the **Database Generation Workflow** property in the EF Designer.</span></span> <span data-ttu-id="1f04d-109">Jednodušší alternativou je použití Code First.</span><span class="sxs-lookup"><span data-stu-id="1f04d-109">An easier alternative is to use Code First.</span></span>

## <a name="other-inheritance-options"></a><span data-ttu-id="1f04d-110">Další možnosti dědičnosti</span><span class="sxs-lookup"><span data-stu-id="1f04d-110">Other Inheritance Options</span></span>

<span data-ttu-id="1f04d-111">Tabulka na typ (TPT) je jiný typ dědičnosti, ve kterém jsou oddělené tabulky v databázi mapovány na entity, které se účastní dědičnosti.</span><span class="sxs-lookup"><span data-stu-id="1f04d-111">Table-per-Type (TPT) is another type of inheritance in which separate tables in the database are mapped to entities that participate in the inheritance.</span></span> <span data-ttu-id="1f04d-112"> Informace o tom, jak namapovat dědičnost tabulek na typ pomocí návrháře EF, najdete v tématu [DĚDIČNOST TPT návrháře EF](~/ef6/modeling/designer/inheritance/tpt.md).</span><span class="sxs-lookup"><span data-stu-id="1f04d-112"> For information about how to map Table-per-Type inheritance with the EF Designer, see [EF Designer TPT Inheritance](~/ef6/modeling/designer/inheritance/tpt.md).</span></span>

<span data-ttu-id="1f04d-113">Dědičnost typů (TPC) podle konkrétního typu () a smíšené modely dědičnosti jsou podporovány modulem runtime Entity Framework, ale Návrhář EF je nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="1f04d-113">Table-per-Concrete Type Inheritance (TPC) and mixed inheritance models are supported by the Entity Framework runtime but are not supported by the EF Designer.</span></span> <span data-ttu-id="1f04d-114">Pokud chcete použít TPC nebo smíšenou dědičnost, máte dvě možnosti: použijte Code First nebo ručně upravte soubor EDMX.</span><span class="sxs-lookup"><span data-stu-id="1f04d-114">If you want to use TPC or mixed inheritance, you have two options: use Code First, or manually edit the EDMX file.</span></span> <span data-ttu-id="1f04d-115">Pokud se rozhodnete pracovat se souborem EDMX, bude okno Podrobnosti mapování přepnuto do bezpečného režimu a nebudete moci použít návrháře ke změně mapování.</span><span class="sxs-lookup"><span data-stu-id="1f04d-115">If you choose to work with the EDMX file, the Mapping Details Window will be put into “safe mode” and you will not be able to use the designer to change the mappings.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1f04d-116">Předpoklady</span><span class="sxs-lookup"><span data-stu-id="1f04d-116">Prerequisites</span></span>

<span data-ttu-id="1f04d-117">K dokončení toho návodu budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="1f04d-117">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="1f04d-118">Poslední verze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1f04d-118">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="1f04d-119">[Ukázková databáze školy](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="1f04d-119">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="1f04d-120">Nastavení projektu</span><span class="sxs-lookup"><span data-stu-id="1f04d-120">Set up the Project</span></span>

-   <span data-ttu-id="1f04d-121">Otevřete Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="1f04d-121">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="1f04d-122">Vybrat **soubor-&gt; projekt New-&gt;**</span><span class="sxs-lookup"><span data-stu-id="1f04d-122">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="1f04d-123">V levém podokně klikněte na položku **Visual C\#** a pak vyberte šablonu **konzoly** .</span><span class="sxs-lookup"><span data-stu-id="1f04d-123">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="1f04d-124">Jako název zadejte **TPHDBFirstSample** .</span><span class="sxs-lookup"><span data-stu-id="1f04d-124">Enter **TPHDBFirstSample** as the name.</span></span>
-   <span data-ttu-id="1f04d-125">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="1f04d-125">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="1f04d-126">Vytvoření modelu</span><span class="sxs-lookup"><span data-stu-id="1f04d-126">Create a Model</span></span>

-   <span data-ttu-id="1f04d-127">Klikněte pravým tlačítkem myši na název projektu v Průzkumník řešení a vyberte možnost **Přidat-&gt; novou položku**.</span><span class="sxs-lookup"><span data-stu-id="1f04d-127">Right-click the project name in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="1f04d-128">V nabídce vlevo vyberte **data** a v podokně šablony vyberte **ADO.NET model EDM (Entity Data Model)** .</span><span class="sxs-lookup"><span data-stu-id="1f04d-128">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="1f04d-129">Jako název souboru zadejte **TPHModel. edmx** a pak klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="1f04d-129">Enter **TPHModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="1f04d-130">V dialogovém okně Vybrat obsah modelu vyberte možnost **Generovat z databáze**a poté klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="1f04d-130">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="1f04d-131">Klikněte na **nové připojení**.</span><span class="sxs-lookup"><span data-stu-id="1f04d-131">Click **New Connection**.</span></span>
    <span data-ttu-id="1f04d-132">V dialogovém okně Vlastnosti připojení zadejte název serveru (například **(LocalDB)\\mssqllocaldb**), vyberte metodu ověřování, jako název databáze zadejte **School** a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="1f04d-132">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="1f04d-133">Dialogové okno zvolit datové připojení je aktualizováno nastavením připojení k databázi.</span><span class="sxs-lookup"><span data-stu-id="1f04d-133">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="1f04d-134">V dialogovém okně zvolte objekty databáze v uzlu tabulky vyberte tabulku **osoba** .</span><span class="sxs-lookup"><span data-stu-id="1f04d-134">In the Choose Your Database Objects dialog box, under the Tables node, select the **Person** table.</span></span>
-   <span data-ttu-id="1f04d-135">Klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="1f04d-135">Click **Finish**.</span></span>

<span data-ttu-id="1f04d-136">Zobrazí se Entity Designer, která poskytuje návrhovou plochu pro úpravu vašeho modelu.</span><span class="sxs-lookup"><span data-stu-id="1f04d-136">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="1f04d-137">Do modelu jsou přidány všechny objekty, které jste vybrali v dialogovém okně zvolit objekty databáze.</span><span class="sxs-lookup"><span data-stu-id="1f04d-137">All the objects that you selected in the Choose Your Database Objects dialog box are added to the model.</span></span>

<span data-ttu-id="1f04d-138">To znamená, jak tabulka **osoby** vypadá v databázi.</span><span class="sxs-lookup"><span data-stu-id="1f04d-138">That is how the **Person** table looks in the database.</span></span>

![Tabulka Person](~/ef6/media/persontable.png) 

## <a name="implement-table-per-hierarchy-inheritance"></a><span data-ttu-id="1f04d-140">Implementace dědičnosti tabulek na hierarchii</span><span class="sxs-lookup"><span data-stu-id="1f04d-140">Implement Table-per-Hierarchy Inheritance</span></span>

<span data-ttu-id="1f04d-141">Tabulka **Person** má sloupec **diskriminátor** , který může mít jednu ze dvou hodnot: "student" a "instruktor".</span><span class="sxs-lookup"><span data-stu-id="1f04d-141">The **Person** table has the **Discriminator** column, which can have one of two values: “Student” and “Instructor”.</span></span> <span data-ttu-id="1f04d-142">V závislosti na hodnotě, kterou bude tabulka **Person** namapována na entitu **studenta** nebo na entitu **instruktora** .</span><span class="sxs-lookup"><span data-stu-id="1f04d-142">Depending on the value the **Person** table will be mapped to the **Student** entity or the **Instructor** entity.</span></span> <span data-ttu-id="1f04d-143">Tabulka **Person** má také dva sloupce, **ZaměstnánOd** a **EnrollmentDate**, které musí mít **hodnotu null** , protože osoba nemůže být studentem a instruktorem ve stejnou dobu (alespoň v tomto návodu).</span><span class="sxs-lookup"><span data-stu-id="1f04d-143">The **Person** table also has two columns, **HireDate** and **EnrollmentDate**, which must be **nullable** because a person cannot be a student and an instructor at the same time (at least not in this walkthrough).</span></span>

### <a name="add-new-entities"></a><span data-ttu-id="1f04d-144">Přidat nové entity</span><span class="sxs-lookup"><span data-stu-id="1f04d-144">Add new Entities</span></span>

-   <span data-ttu-id="1f04d-145">Přidejte novou entitu.</span><span class="sxs-lookup"><span data-stu-id="1f04d-145">Add a new entity.</span></span>
    <span data-ttu-id="1f04d-146">Provedete to tak, že kliknete pravým tlačítkem myši na prázdné místo na návrhové ploše Entity Framework Designer a vyberete **Přidat-&gt;entitu**.</span><span class="sxs-lookup"><span data-stu-id="1f04d-146">To do this, right-click on an empty space of the design surface of the Entity Framework Designer, and select **Add-&gt;Entity**.</span></span>
-   <span data-ttu-id="1f04d-147">Do pole **název entity** zadejte  **instruktora** a v rozevíracím seznamu pro **základní typ**vyberte **osoba** .</span><span class="sxs-lookup"><span data-stu-id="1f04d-147">Type **Instructor** for the **Entity name** and select **Person** from the drop-down list for the **Base type**.</span></span>
-   <span data-ttu-id="1f04d-148">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="1f04d-148">Click **OK**.</span></span>
-   <span data-ttu-id="1f04d-149">Přidejte další novou entitu.</span><span class="sxs-lookup"><span data-stu-id="1f04d-149">Add another new entity.</span></span> <span data-ttu-id="1f04d-150">Jako **název entity** zadejte  **student** a v rozevíracím seznamu pro **základní typ**vyberte **Person** .</span><span class="sxs-lookup"><span data-stu-id="1f04d-150">Type **Student** for the **Entity name** and select **Person** from the drop-down list for the **Base type**.</span></span>

<span data-ttu-id="1f04d-151">Na návrhovou plochu se přidaly dva nové typy entit.</span><span class="sxs-lookup"><span data-stu-id="1f04d-151">Two new entity types were added to the design surface.</span></span> <span data-ttu-id="1f04d-152">Šipka směřuje od nových typů entit od typu **osoba** typ entity; To znamená, že  **osoba** je základním typem pro nové typy entit.</span><span class="sxs-lookup"><span data-stu-id="1f04d-152">An arrow points from the new entity types to the **Person** entity type; this indicates that **Person** is the base type for the new entity types.</span></span>

-   <span data-ttu-id="1f04d-153">Klikněte pravým tlačítkem na vlastnost **zaměstnánod**  **osoby** entity.</span><span class="sxs-lookup"><span data-stu-id="1f04d-153">Right-click the **HireDate** property of the **Person** entity.</span></span> <span data-ttu-id="1f04d-154">Vyberte **Vyjmout** (nebo použijte klávesu CTRL-X).</span><span class="sxs-lookup"><span data-stu-id="1f04d-154">Select **Cut** (or use the Ctrl-X key).</span></span>
-   <span data-ttu-id="1f04d-155">Klikněte pravým tlačítkem myši na **instruktora** entitu a vyberte **Vložit** (nebo použijte klávesu CTRL-V).</span><span class="sxs-lookup"><span data-stu-id="1f04d-155">Right-click the **Instructor** entity and select **Paste** (or use the Ctrl-V key).</span></span>
-   <span data-ttu-id="1f04d-156">Klikněte pravým tlačítkem na vlastnost **HireDate** a vyberte možnost **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="1f04d-156">Right-click the **HireDate** property and select **Properties**.</span></span>
-   <span data-ttu-id="1f04d-157">V okně **vlastnosti** nastavte vlastnost **Nullable** na **false**.</span><span class="sxs-lookup"><span data-stu-id="1f04d-157">In the **Properties** window, set the **Nullable** property to **false**.</span></span>
-   <span data-ttu-id="1f04d-158">Klikněte pravým tlačítkem na vlastnost **EnrollmentDate**  **osoby** entity.</span><span class="sxs-lookup"><span data-stu-id="1f04d-158">Right-click the **EnrollmentDate** property of the **Person** entity.</span></span> <span data-ttu-id="1f04d-159">Vyberte **Vyjmout** (nebo použijte klávesu CTRL-X).</span><span class="sxs-lookup"><span data-stu-id="1f04d-159">Select **Cut** (or use the Ctrl-X key).</span></span>
-   <span data-ttu-id="1f04d-160">Klikněte pravým tlačítkem na entitu **studenta** a vyberte **Vložit (nebo použijte klávesu CTRL-V).**</span><span class="sxs-lookup"><span data-stu-id="1f04d-160">Right-click the **Student** entity and select **Paste(or use the Ctrl-V key).**</span></span>
-   <span data-ttu-id="1f04d-161">Vyberte vlastnost **EnrollmentDate** a nastavte vlastnost **Nullable** na **false**.</span><span class="sxs-lookup"><span data-stu-id="1f04d-161">Select the **EnrollmentDate** property and set the **Nullable** property to **false**.</span></span>
-   <span data-ttu-id="1f04d-162">Vyberte typ entity  **osoba** .</span><span class="sxs-lookup"><span data-stu-id="1f04d-162">Select the **Person** entity type.</span></span> <span data-ttu-id="1f04d-163">V okně **vlastnosti** nastavte jeho vlastnost **abstract** na **hodnotu true**.</span><span class="sxs-lookup"><span data-stu-id="1f04d-163">In the **Properties** window, set its **Abstract** property to **true**.</span></span>
-   <span data-ttu-id="1f04d-164">Odstraňte vlastnost **diskriminátor** z **Person**.</span><span class="sxs-lookup"><span data-stu-id="1f04d-164">Delete the **Discriminator** property from **Person**.</span></span> <span data-ttu-id="1f04d-165">Důvod, proč by měl být odstraněn, je vysvětlen v následující části.</span><span class="sxs-lookup"><span data-stu-id="1f04d-165">The reason it should be deleted is explained in the following section.</span></span>

### <a name="map-the-entities"></a><span data-ttu-id="1f04d-166">Mapování entit</span><span class="sxs-lookup"><span data-stu-id="1f04d-166">Map the entities</span></span>

-   <span data-ttu-id="1f04d-167">Klikněte pravým tlačítkem na **instruktor** a vyberte **mapování tabulky.**</span><span class="sxs-lookup"><span data-stu-id="1f04d-167">Right-click the **Instructor** and select **Table Mapping.**</span></span>
    <span data-ttu-id="1f04d-168">V okně podrobností mapování je vybrána entita Instructor.</span><span class="sxs-lookup"><span data-stu-id="1f04d-168">The Instructor entity is selected in the Mapping Details window.</span></span>
-   <span data-ttu-id="1f04d-169">V okna **podrobností mapování** klikněte na **&lt;přidat tabulku nebo zobrazení&gt;**  .</span><span class="sxs-lookup"><span data-stu-id="1f04d-169">Click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
    <span data-ttu-id="1f04d-170"> **&lt;přidání tabulky nebo zobrazení&gt;** pole se zobrazí rozevírací seznam tabulek nebo zobrazení, do kterých lze vybranou entitu namapovat.</span><span class="sxs-lookup"><span data-stu-id="1f04d-170">The **&lt;Add a Table or View&gt;** field becomes a drop-down list of tables or views to which the selected entity can be mapped.</span></span>
-   <span data-ttu-id="1f04d-171">Z rozevíracího seznamu vyberte **osoba** .</span><span class="sxs-lookup"><span data-stu-id="1f04d-171">Select **Person** from the drop-down list.</span></span>
-   <span data-ttu-id="1f04d-172">Okno  **podrobností mapování** se aktualizuje s výchozími mapováními sloupců a možností pro přidání podmínky.</span><span class="sxs-lookup"><span data-stu-id="1f04d-172">The **Mapping Details** window is updated with default column mappings and an option for adding a condition.</span></span>
-   <span data-ttu-id="1f04d-173">Klikněte na \*\*&lt;přidat&gt;podmínky \*\*.</span><span class="sxs-lookup"><span data-stu-id="1f04d-173">Click on **&lt;Add a Condition&gt;**.</span></span>
    <span data-ttu-id="1f04d-174"> **&lt;přidat podmínku&gt;**  pole se zobrazí rozevírací seznam sloupců, pro které lze nastavit podmínky.</span><span class="sxs-lookup"><span data-stu-id="1f04d-174">The **&lt;Add a Condition&gt;** field becomes a drop-down list of columns for which conditions can be set.</span></span>
-   <span data-ttu-id="1f04d-175">V rozevíracím seznamu vyberte **diskriminátor** .</span><span class="sxs-lookup"><span data-stu-id="1f04d-175">Select **Discriminator** from the drop-down list.</span></span>
-   <span data-ttu-id="1f04d-176">Ve sloupci **operátor** v okně **Podrobnosti mapování** v rozevíracím seznamu vyberte =.</span><span class="sxs-lookup"><span data-stu-id="1f04d-176">In the **Operator** column of the **Mapping Details** window, select = from the drop-down list.</span></span>
-   <span data-ttu-id="1f04d-177">Do sloupce **hodnota nebo vlastnost** zadejte **instruktor**.</span><span class="sxs-lookup"><span data-stu-id="1f04d-177">In the **Value/Property** column, type **Instructor**.</span></span> <span data-ttu-id="1f04d-178">Konečný výsledek by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="1f04d-178">The end result should look like this:</span></span>

    ![Mapování podrobností](~/ef6/media/mappingdetails2.png)

-   <span data-ttu-id="1f04d-180">Opakujte tento postup pro typ entity  **studenta** , ale nastavte podmínku rovnou hodnotě **student** .</span><span class="sxs-lookup"><span data-stu-id="1f04d-180">Repeat these steps for the **Student** entity type, but make the condition equal to **Student** value.</span></span>  
    <span data-ttu-id="1f04d-181">*Důvodem, proč jsme chtěli odebrat vlastnost **diskriminátoru** , je to, že sloupec tabulky nejde namapovat více než jednou. Tento sloupec bude použit pro podmíněné mapování, takže jej nelze použít také pro mapování vlastností. Jediným způsobem, jak lze použít pro obě, pokud podmínka používá **hodnotu null** nebo není **null** porovnání.*</span><span class="sxs-lookup"><span data-stu-id="1f04d-181">*The reason we wanted to remove the **Discriminator** property, is because you cannot map a table column more than once. This column will be used for conditional mapping, so it cannot be used for property mapping as well. The only way it can be used for both, if a condition uses an **Is Null** or **Is Not Null** comparison.*</span></span>

<span data-ttu-id="1f04d-182">Dědičnost tabulky na hierarchii je nyní implementována.</span><span class="sxs-lookup"><span data-stu-id="1f04d-182">Table-per-hierarchy inheritance is now implemented.</span></span>

![Konečný TPH](~/ef6/media/finaltph.png)

## <a name="use-the-model"></a><span data-ttu-id="1f04d-184">Použití modelu</span><span class="sxs-lookup"><span data-stu-id="1f04d-184">Use the Model</span></span>

<span data-ttu-id="1f04d-185">Otevřete soubor **program.cs** , kde je definována metoda **Main** .</span><span class="sxs-lookup"><span data-stu-id="1f04d-185">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="1f04d-186">Do funkce **Main** vložte následující kód.</span><span class="sxs-lookup"><span data-stu-id="1f04d-186">Paste the following code into the **Main** function.</span></span> <span data-ttu-id="1f04d-187">Kód spouští tři dotazy.</span><span class="sxs-lookup"><span data-stu-id="1f04d-187">The code executes three queries.</span></span> <span data-ttu-id="1f04d-188">První dotaz vrátí všechny objekty **Person** .</span><span class="sxs-lookup"><span data-stu-id="1f04d-188">The first query brings back all **Person** objects.</span></span> <span data-ttu-id="1f04d-189">Druhý dotaz používá metodu **OfType** k vrácení objektů **instruktora** .</span><span class="sxs-lookup"><span data-stu-id="1f04d-189">The second query uses the **OfType** method to return **Instructor** objects.</span></span> <span data-ttu-id="1f04d-190">Třetí dotaz používá metodu **OfType** k vrácení objektů **studenta** .</span><span class="sxs-lookup"><span data-stu-id="1f04d-190">The third query uses the **OfType** method to return **Student** objects.</span></span>

``` csharp
    using (var context = new SchoolEntities())
    {
        Console.WriteLine("All people:");
        foreach (var person in context.People)
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }

        Console.WriteLine("Instructors only: ");
        foreach (var person in context.People.OfType<Instructor>())
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }

        Console.WriteLine("Students only: ");
        foreach (var person in context.People.OfType<Student>())
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }
    }
```
