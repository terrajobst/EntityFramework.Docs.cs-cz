---
title: Dědičnost návrháře TPH - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 72d26a8e-20ab-4500-bd13-394a08e73394
ms.openlocfilehash: 43ba34a98c3960a7a3052a00e2ed2751c2f2b121
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490109"
---
# <a name="designer-tph-inheritance"></a><span data-ttu-id="70a68-102">Návrháře TPH dědičnosti</span><span class="sxs-lookup"><span data-stu-id="70a68-102">Designer TPH Inheritance</span></span>
<span data-ttu-id="70a68-103">Tento podrobný návod ukazuje, jak implementovat dědičnosti na hierarchii tabulky (TPH) v konceptuálním modelu se Návrhář Entity Framework (EF designeru).</span><span class="sxs-lookup"><span data-stu-id="70a68-103">This step-by-step walkthrough shows how to implement table-per-hierarchy (TPH) inheritance in your conceptual model with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="70a68-104">Dědičnost TPH pomocí jedné tabulky databáze zachovat data pro všechny typy entit v hierarchii dědičnosti.</span><span class="sxs-lookup"><span data-stu-id="70a68-104">TPH inheritance uses one database table to maintain data for all of the entity types in an inheritance hierarchy.</span></span>

<span data-ttu-id="70a68-105">V tomto názorném postupu jsme mapování tabulky osoba na tři typy entit: osoba (základní typ), studenty (je odvozena od osoby) a kurzů vedených (je odvozena od osoby).</span><span class="sxs-lookup"><span data-stu-id="70a68-105">In this walkthrough we will map the Person table to three entity types: Person (the base type), Student (derives from Person), and Instructor (derives from Person).</span></span> <span data-ttu-id="70a68-106">Vytvoříme Koncepční model z databáze (Database First) a poté změňte model implementovat dědičnost TPH pomocí EF designeru.</span><span class="sxs-lookup"><span data-stu-id="70a68-106">We'll create a conceptual model from the database (Database First) and then alter the model to implement the TPH inheritance using the EF Designer.</span></span>

<span data-ttu-id="70a68-107">Je možné namapovat na TPH dědičnosti pomocí modelu první, ale vy byste chtěli zapsat generování pracovního postupu vlastní databázi, což je komplexní.</span><span class="sxs-lookup"><span data-stu-id="70a68-107">It is possible to map to a TPH inheritance using Model First but you would have to write your own database generation workflow which is complex.</span></span> <span data-ttu-id="70a68-108">By pak přiřaďte tento pracovní postup **pracovní postup generování databáze** vlastnost v EF designeru.</span><span class="sxs-lookup"><span data-stu-id="70a68-108">You would then assign this workflow to the **Database Generation Workflow** property in the EF Designer.</span></span> <span data-ttu-id="70a68-109">Jednodušší alternativu je použití Code First.</span><span class="sxs-lookup"><span data-stu-id="70a68-109">An easier alternative is to use Code First.</span></span>

## <a name="other-inheritance-options"></a><span data-ttu-id="70a68-110">Další možnosti dědičnosti</span><span class="sxs-lookup"><span data-stu-id="70a68-110">Other Inheritance Options</span></span>

<span data-ttu-id="70a68-111">Za typ tabulky (TPT) je jiný druh dědičnosti, ve kterém samostatných tabulek v databázi se mapují na entity, které se účastní dědičnosti.</span><span class="sxs-lookup"><span data-stu-id="70a68-111">Table-per-Type (TPT) is another type of inheritance in which separate tables in the database are mapped to entities that participate in the inheritance.</span></span>  <span data-ttu-id="70a68-112">Informace o tom, jak mapovat na typ tabulky dědičnosti s EF designeru najdete v tématu [EF návrháře TPT dědičnosti](~/ef6/modeling/designer/inheritance/tpt.md).</span><span class="sxs-lookup"><span data-stu-id="70a68-112">For information about how to map Table-per-Type inheritance with the EF Designer, see [EF Designer TPT Inheritance](~/ef6/modeling/designer/inheritance/tpt.md).</span></span>

<span data-ttu-id="70a68-113">Smíšené dědičnosti modely a tabulky na konkrétní typ dědičnosti (TPC) podporuje modul runtime rozhraní Entity Framework, ale nepodporuje EF designeru.</span><span class="sxs-lookup"><span data-stu-id="70a68-113">Table-per-Concrete Type Inheritance (TPC) and mixed inheritance models are supported by the Entity Framework runtime but are not supported by the EF Designer.</span></span> <span data-ttu-id="70a68-114">Pokud chcete použít TPC nebo smíšené dědičnosti, máte dvě možnosti: použijte Code First, nebo ručně upravit soubor EDMX.</span><span class="sxs-lookup"><span data-stu-id="70a68-114">If you want to use TPC or mixed inheritance, you have two options: use Code First, or manually edit the EDMX file.</span></span> <span data-ttu-id="70a68-115">Pokud budete chtít pracovat s souboru EDMX, v okně podrobností mapování zařadí do "nouzový režim" a nebude možné změnit mapování pomocí návrháře.</span><span class="sxs-lookup"><span data-stu-id="70a68-115">If you choose to work with the EDMX file, the Mapping Details Window will be put into “safe mode” and you will not be able to use the designer to change the mappings.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="70a68-116">Požadavky</span><span class="sxs-lookup"><span data-stu-id="70a68-116">Prerequisites</span></span>

<span data-ttu-id="70a68-117">K dokončení toho návodu budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="70a68-117">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="70a68-118">Nejnovější verzi sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="70a68-118">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="70a68-119">[Ukázkové databáze školy](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="70a68-119">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="70a68-120">Nastavení projektu</span><span class="sxs-lookup"><span data-stu-id="70a68-120">Set up the Project</span></span>

-   <span data-ttu-id="70a68-121">Otevřít Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="70a68-121">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="70a68-122">Vyberte **souboru -&gt; nové –&gt; projektu**</span><span class="sxs-lookup"><span data-stu-id="70a68-122">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="70a68-123">V levém podokně klikněte na tlačítko **Visual C\#** a pak vyberte **konzoly** šablony.</span><span class="sxs-lookup"><span data-stu-id="70a68-123">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="70a68-124">Zadejte **TPHDBFirstSample** jako název.</span><span class="sxs-lookup"><span data-stu-id="70a68-124">Enter **TPHDBFirstSample** as the name.</span></span>
-   <span data-ttu-id="70a68-125">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="70a68-125">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="70a68-126">Vytvoření modelu</span><span class="sxs-lookup"><span data-stu-id="70a68-126">Create a Model</span></span>

-   <span data-ttu-id="70a68-127">Klikněte pravým tlačítkem na název projektu v Průzkumníku řešení a vyberte **Add -&gt; nová položka**.</span><span class="sxs-lookup"><span data-stu-id="70a68-127">Right-click the project name in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="70a68-128">Vyberte **Data** v levé nabídce a pak vyberte **datový Model Entity ADO.NET** v podokně šablon.</span><span class="sxs-lookup"><span data-stu-id="70a68-128">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="70a68-129">Zadejte **TPHModel.edmx** pro název souboru a pak klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="70a68-129">Enter **TPHModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="70a68-130">V dialogovém okně Výběr obsahu modelu vyberte **Generovat z databáze**a potom klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="70a68-130">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="70a68-131">Klikněte na tlačítko **nové připojení**.</span><span class="sxs-lookup"><span data-stu-id="70a68-131">Click **New Connection**.</span></span>
    <span data-ttu-id="70a68-132">V dialogovém okně Vlastnosti připojení zadat název serveru (například **(localdb)\\mssqllocaldb**), vyberte metodu ověřování, zadejte **School** pro název databáze a pak Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="70a68-132">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="70a68-133">Dialogové okno Vybrat datové připojení se aktualizuje se nastavení připojení databáze.</span><span class="sxs-lookup"><span data-stu-id="70a68-133">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="70a68-134">V dialogovém okně Zvolte vaše databázové objekty tabulky uzlu, vyberte **osoba** tabulky.</span><span class="sxs-lookup"><span data-stu-id="70a68-134">In the Choose Your Database Objects dialog box, under the Tables node, select the **Person** table.</span></span>
-   <span data-ttu-id="70a68-135">Klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="70a68-135">Click **Finish**.</span></span>

<span data-ttu-id="70a68-136">V návrháři entit, které poskytuje návrhové ploše pro úpravy váš model, se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="70a68-136">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="70a68-137">Všechny objekty, které jste vybrali v dialogovém okně Zvolte vaše databázové objekty jsou přidány do modelu.</span><span class="sxs-lookup"><span data-stu-id="70a68-137">All the objects that you selected in the Choose Your Database Objects dialog box are added to the model.</span></span>

<span data-ttu-id="70a68-138">To znamená jak **osoba** vypadá tabulka v databázi.</span><span class="sxs-lookup"><span data-stu-id="70a68-138">That is how the **Person** table looks in the database.</span></span>

![Tabulka osoby](~/ef6/media/persontable.png) 

## <a name="implement-table-per-hierarchy-inheritance"></a><span data-ttu-id="70a68-140">Implementace tabulky za hierarchie dědičnosti</span><span class="sxs-lookup"><span data-stu-id="70a68-140">Implement Table-per-Hierarchy Inheritance</span></span>

<span data-ttu-id="70a68-141">**Osoba** tabulka má **diskriminátoru** sloupec, který může mít jednu ze dvou hodnot: "Studenta" a "Kurzů vedených".</span><span class="sxs-lookup"><span data-stu-id="70a68-141">The **Person** table has the **Discriminator** column, which can have one of two values: “Student” and “Instructor”.</span></span> <span data-ttu-id="70a68-142">V závislosti na hodnotě **osoba** tabulky se namapují na **Student** entity nebo **kurzů vedených** entity.</span><span class="sxs-lookup"><span data-stu-id="70a68-142">Depending on the value the **Person** table will be mapped to the **Student** entity or the **Instructor** entity.</span></span> <span data-ttu-id="70a68-143">**Osoba** tabulka má také dva sloupce **HireDate** a **EnrollmentDate**, která musí být **s možnou hodnotou Null** vzhledem k tomu, že uživatel nemůže být pro studenty a instruktorem ve stejnou dobu (alespoň není v tomto názorném postupu).</span><span class="sxs-lookup"><span data-stu-id="70a68-143">The **Person** table also has two columns, **HireDate** and **EnrollmentDate**, which must be **nullable** because a person cannot be a student and an instructor at the same time (at least not in this walkthrough).</span></span>

### <a name="add-new-entities"></a><span data-ttu-id="70a68-144">Přidat nové entity</span><span class="sxs-lookup"><span data-stu-id="70a68-144">Add new Entities</span></span>

-   <span data-ttu-id="70a68-145">Přidáte novou entitu.</span><span class="sxs-lookup"><span data-stu-id="70a68-145">Add a new entity.</span></span>
    <span data-ttu-id="70a68-146">Chcete-li to provést, klikněte pravým tlačítkem na prázdné místo návrhové plochy Entity Framework Designer a vyberte **Add -&gt;Entity**.</span><span class="sxs-lookup"><span data-stu-id="70a68-146">To do this, right-click on an empty space of the design surface of the Entity Framework Designer, and select **Add-&gt;Entity**.</span></span>
-   <span data-ttu-id="70a68-147">Typ **kurzů vedených** pro **název Entity** a vyberte **osoba** z rozevíracího seznamu pro **základní typ**.</span><span class="sxs-lookup"><span data-stu-id="70a68-147">Type **Instructor** for the **Entity name** and select **Person** from the drop-down list for the **Base type**.</span></span>
-   <span data-ttu-id="70a68-148">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="70a68-148">Click **OK**.</span></span>
-   <span data-ttu-id="70a68-149">Přidejte další novou entitu.</span><span class="sxs-lookup"><span data-stu-id="70a68-149">Add another new entity.</span></span> <span data-ttu-id="70a68-150">Typ **Student** pro **název Entity** a vyberte **osoba** z rozevíracího seznamu pro **základní typ**.</span><span class="sxs-lookup"><span data-stu-id="70a68-150">Type **Student** for the **Entity name** and select **Person** from the drop-down list for the **Base type**.</span></span>

<span data-ttu-id="70a68-151">Na návrhovou plochu byly přidány dva nové typy entit.</span><span class="sxs-lookup"><span data-stu-id="70a68-151">Two new entity types were added to the design surface.</span></span> <span data-ttu-id="70a68-152">Šipka ukazuje z nové typy entit, které se **osoba** typu entity; to znamená, že **osoba** je základním typem pro nové typy entit.</span><span class="sxs-lookup"><span data-stu-id="70a68-152">An arrow points from the new entity types to the **Person** entity type; this indicates that **Person** is the base type for the new entity types.</span></span>

-   <span data-ttu-id="70a68-153">Klikněte pravým tlačítkem myši **HireDate** vlastnost **osoba** entity.</span><span class="sxs-lookup"><span data-stu-id="70a68-153">Right-click the **HireDate** property of the **Person** entity.</span></span> <span data-ttu-id="70a68-154">Vyberte **Vyjmout** (nebo pomocí klávesy Ctrl-X).</span><span class="sxs-lookup"><span data-stu-id="70a68-154">Select **Cut** (or use the Ctrl-X key).</span></span>
-   <span data-ttu-id="70a68-155">Klikněte pravým tlačítkem myši **kurzů vedených** entity a vyberte **vložit** (nebo pomocí klávesy Ctrl-V).</span><span class="sxs-lookup"><span data-stu-id="70a68-155">Right-click the **Instructor** entity and select **Paste** (or use the Ctrl-V key).</span></span>
-   <span data-ttu-id="70a68-156">Klikněte pravým tlačítkem myši **HireDate** vlastnosti a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="70a68-156">Right-click the **HireDate** property and select **Properties**.</span></span>
-   <span data-ttu-id="70a68-157">V **vlastnosti** okno, nastaveno **Nullable** vlastnost **false**.</span><span class="sxs-lookup"><span data-stu-id="70a68-157">In the **Properties** window, set the **Nullable** property to **false**.</span></span>
-   <span data-ttu-id="70a68-158">Klikněte pravým tlačítkem myši **EnrollmentDate** vlastnost **osoba** entity.</span><span class="sxs-lookup"><span data-stu-id="70a68-158">Right-click the **EnrollmentDate** property of the **Person** entity.</span></span> <span data-ttu-id="70a68-159">Vyberte **Vyjmout** (nebo pomocí klávesy Ctrl-X).</span><span class="sxs-lookup"><span data-stu-id="70a68-159">Select **Cut** (or use the Ctrl-X key).</span></span>
-   <span data-ttu-id="70a68-160">Klikněte pravým tlačítkem myši **Student** entity a vyberte **vložit (nebo klíče pomocí Ctrl-V).**</span><span class="sxs-lookup"><span data-stu-id="70a68-160">Right-click the **Student** entity and select **Paste(or use the Ctrl-V key).**</span></span>
-   <span data-ttu-id="70a68-161">Vyberte **EnrollmentDate** vlastnost a nastavte **Nullable** vlastnost **false**.</span><span class="sxs-lookup"><span data-stu-id="70a68-161">Select the **EnrollmentDate** property and set the **Nullable** property to **false**.</span></span>
-   <span data-ttu-id="70a68-162">Vyberte **osoba** typu entity.</span><span class="sxs-lookup"><span data-stu-id="70a68-162">Select the **Person** entity type.</span></span> <span data-ttu-id="70a68-163">V **vlastnosti** okno, nastavte jeho **abstraktní** vlastnost **true**.</span><span class="sxs-lookup"><span data-stu-id="70a68-163">In the **Properties** window, set its **Abstract** property to **true**.</span></span>
-   <span data-ttu-id="70a68-164">Odstranit **diskriminátoru** vlastnost z **osoba**.</span><span class="sxs-lookup"><span data-stu-id="70a68-164">Delete the **Discriminator** property from **Person**.</span></span> <span data-ttu-id="70a68-165">V následující části je vysvětlen z důvodů, proč je nutné ji odstranit.</span><span class="sxs-lookup"><span data-stu-id="70a68-165">The reason it should be deleted is explained in the following section.</span></span>

### <a name="map-the-entities"></a><span data-ttu-id="70a68-166">Mapovat entity</span><span class="sxs-lookup"><span data-stu-id="70a68-166">Map the entities</span></span>

-   <span data-ttu-id="70a68-167">Klikněte pravým tlačítkem myši **kurzů vedených** a vyberte **mapování tabulky.**</span><span class="sxs-lookup"><span data-stu-id="70a68-167">Right-click the **Instructor** and select **Table Mapping.**</span></span>
    <span data-ttu-id="70a68-168">V okně Podrobnosti mapování je vybraná entita instruktorem.</span><span class="sxs-lookup"><span data-stu-id="70a68-168">The Instructor entity is selected in the Mapping Details window.</span></span>
-   <span data-ttu-id="70a68-169">Klikněte na tlačítko **&lt;přidat tabulku nebo zobrazení&gt;** v **podrobnosti mapování** okna.</span><span class="sxs-lookup"><span data-stu-id="70a68-169">Click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
    <span data-ttu-id="70a68-170">**&lt;Přidat tabulku nebo zobrazení&gt;** pole stane rozevírací seznam tabulek nebo zobrazení pro které je možné mapovat vybrané entity.</span><span class="sxs-lookup"><span data-stu-id="70a68-170">The **&lt;Add a Table or View&gt;** field becomes a drop-down list of tables or views to which the selected entity can be mapped.</span></span>
-   <span data-ttu-id="70a68-171">Vyberte **osoba** z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="70a68-171">Select **Person** from the drop-down list.</span></span>
-   <span data-ttu-id="70a68-172">**Podrobnosti mapování** okno se aktualizuje s výchozí mapování sloupců a možnost pro přidání podmínky.</span><span class="sxs-lookup"><span data-stu-id="70a68-172">The **Mapping Details** window is updated with default column mappings and an option for adding a condition.</span></span>
-   <span data-ttu-id="70a68-173">Klikněte na  **&lt;přidat podmínku&gt;**.</span><span class="sxs-lookup"><span data-stu-id="70a68-173">Click on **&lt;Add a Condition&gt;**.</span></span>
    <span data-ttu-id="70a68-174">**&lt;Přidat podmínku&gt;** pole bude rozevírací seznam sloupců, pro které můžete nastavit podmínky.</span><span class="sxs-lookup"><span data-stu-id="70a68-174">The **&lt;Add a Condition&gt;** field becomes a drop-down list of columns for which conditions can be set.</span></span>
-   <span data-ttu-id="70a68-175">Vyberte **diskriminátoru** z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="70a68-175">Select **Discriminator** from the drop-down list.</span></span>
-   <span data-ttu-id="70a68-176">V **operátor** sloupec **podrobnosti mapování** okně = z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="70a68-176">In the **Operator** column of the **Mapping Details** window, select = from the drop-down list.</span></span>
-   <span data-ttu-id="70a68-177">V **/vlastnost Value** sloupců, typ **kurzů vedených**.</span><span class="sxs-lookup"><span data-stu-id="70a68-177">In the **Value/Property** column, type **Instructor**.</span></span> <span data-ttu-id="70a68-178">Konečný výsledek by měl vypadat nějak takto:</span><span class="sxs-lookup"><span data-stu-id="70a68-178">The end result should look like this:</span></span>

    ![Podrobnosti mapování](~/ef6/media/mappingdetails2.png)

-   <span data-ttu-id="70a68-180">Opakujte tyto kroky pro **Student** typ entity, ale vytvořit podmínku, která je rovna **Student** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="70a68-180">Repeat these steps for the **Student** entity type, but make the condition equal to **Student** value.</span></span>  
    <span data-ttu-id="70a68-181">*Z důvodu jsme chtěli odebrat **diskriminátoru** je vlastnost, protože sloupec tabulky nelze mapovat více než jednou. V tomto sloupci se použije pro podmíněné mapování, proto jej nelze použít pro vlastnost mapování také. Jediným způsobem, který může sloužit pro obě, pokud používá podmínku **Is Null** nebo **Is Not Null** porovnání.*</span><span class="sxs-lookup"><span data-stu-id="70a68-181">*The reason we wanted to remove the **Discriminator** property, is because you cannot map a table column more than once. This column will be used for conditional mapping, so it cannot be used for property mapping as well. The only way it can be used for both, if a condition uses an **Is Null** or **Is Not Null** comparison.*</span></span>

<span data-ttu-id="70a68-182">Tabulky na hierarchii dědičnosti je nyní implementována.</span><span class="sxs-lookup"><span data-stu-id="70a68-182">Table-per-hierarchy inheritance is now implemented.</span></span>

![Poslední TPH](~/ef6/media/finaltph.png)

## <a name="use-the-model"></a><span data-ttu-id="70a68-184">Použití modelu</span><span class="sxs-lookup"><span data-stu-id="70a68-184">Use the Model</span></span>

<span data-ttu-id="70a68-185">Otevřít **Program.cs** souboru, kde **hlavní** je definována metoda.</span><span class="sxs-lookup"><span data-stu-id="70a68-185">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="70a68-186">Vložte následující kód do **hlavní** funkce.</span><span class="sxs-lookup"><span data-stu-id="70a68-186">Paste the following code into the **Main** function.</span></span> <span data-ttu-id="70a68-187">Kód spustí tři dotazy.</span><span class="sxs-lookup"><span data-stu-id="70a68-187">The code executes three queries.</span></span> <span data-ttu-id="70a68-188">První dotaz vrací všechny **osoba** objekty.</span><span class="sxs-lookup"><span data-stu-id="70a68-188">The first query brings back all **Person** objects.</span></span> <span data-ttu-id="70a68-189">Druhý dotaz používá **OfType** metoda vrátí **kurzů vedených** objekty.</span><span class="sxs-lookup"><span data-stu-id="70a68-189">The second query uses the **OfType** method to return **Instructor** objects.</span></span> <span data-ttu-id="70a68-190">Používá se třetí dotaz **OfType** metoda vrátí **Student** objekty.</span><span class="sxs-lookup"><span data-stu-id="70a68-190">The third query uses the **OfType** method to return **Student** objects.</span></span>

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
