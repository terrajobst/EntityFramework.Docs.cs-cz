---
title: Návrháře TPT dědičnosti - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: efc78c31-b4ea-4ea3-a0cd-c69eb507020e
ms.openlocfilehash: 68980fa89446940b8b7f5f73c519d38e727a9039
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996345"
---
# <a name="designer-tpt-inheritance"></a><span data-ttu-id="60a14-102">Návrháře TPT dědičnosti</span><span class="sxs-lookup"><span data-stu-id="60a14-102">Designer TPT Inheritance</span></span>
<span data-ttu-id="60a14-103">Tento podrobný návod ukazuje, jak implementovat za typ tabulky (TPT) dědičnosti ve vašem modelu pomocí návrháře Entity Framework (EF designeru).</span><span class="sxs-lookup"><span data-stu-id="60a14-103">This step-by-step walkthrough shows how to implement table-per-type (TPT) inheritance in your model using the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="60a14-104">Dědičnost na typ tabulka používá do samostatné tabulky v databázi zachovat data pro-zděděné vlastnosti a vlastnosti klíče pro každý typ v hierarchii dědičnosti.</span><span class="sxs-lookup"><span data-stu-id="60a14-104">Table-per-type inheritance uses a separate table in the database to maintain data for non-inherited properties and key properties for each type in the inheritance hierarchy.</span></span>

<span data-ttu-id="60a14-105">V tomto názorném postupu jsme se namapuje **kurzu** (základní typ), **OnlineCourse** (je odvozena z kurzu), a **OnsiteCourse** (je odvozena z **kurzu**) entity do tabulky se stejnými názvy.</span><span class="sxs-lookup"><span data-stu-id="60a14-105">In this walkthrough we will map the **Course** (base type), **OnlineCourse** (derives from Course), and **OnsiteCourse** (derives from **Course**) entities to tables with the same names.</span></span> <span data-ttu-id="60a14-106">Vytvoříme model z databáze a poté změňte model implementovat TPT dědičnosti.</span><span class="sxs-lookup"><span data-stu-id="60a14-106">We'll create a model from the database and then alter the model to implement the TPT inheritance.</span></span>

<span data-ttu-id="60a14-107">Můžete také začít s první Model a pak vygenerovat databáze z modelu.</span><span class="sxs-lookup"><span data-stu-id="60a14-107">You can also start with the Model First and then generate the database from the model.</span></span> <span data-ttu-id="60a14-108">EF designeru ve výchozím nastavení používá TPT strategie a proto všechny dědění v modelu budou zmapována do samostatných tabulek.</span><span class="sxs-lookup"><span data-stu-id="60a14-108">The EF Designer uses the TPT strategy by default and so any inheritance in the model will be mapped to separate tables.</span></span>

## <a name="other-inheritance-options"></a><span data-ttu-id="60a14-109">Další možnosti dědičnosti</span><span class="sxs-lookup"><span data-stu-id="60a14-109">Other Inheritance Options</span></span>

<span data-ttu-id="60a14-110">Za hierarchii tabulky (TPH) je jiný druh dědičnosti v databázi, kterou jeden tabulky je používán pro zachování dat pro všechny typy entit v hierarchii dědičnosti.</span><span class="sxs-lookup"><span data-stu-id="60a14-110">Table-per-Hierarchy (TPH) is another type of inheritance in which one database table is used to maintain data for all of the entity types in an inheritance hierarchy.</span></span>  <span data-ttu-id="60a14-111">Informace o tom, jak namapovat na hierarchii tabulky dědičnosti pomocí návrháře entit najdete v tématu [EF návrháře TPH dědičnosti](~/ef6/modeling/designer/inheritance/tph.md).</span><span class="sxs-lookup"><span data-stu-id="60a14-111">For information about how to map Table-per-Hierarchy inheritance with the Entity Designer, see [EF Designer TPH Inheritance](~/ef6/modeling/designer/inheritance/tph.md).</span></span> 

<span data-ttu-id="60a14-112">Mějte na paměti, že tabulka na konkrétní zadejte dědičnosti (TPC) a smíšené dědičnosti modely jsou podporované modulem runtime Entity Frameworku, ale nejsou podporovány EF designeru.</span><span class="sxs-lookup"><span data-stu-id="60a14-112">Note that, the Table-per-Concrete Type Inheritance (TPC) and mixed inheritance models are supported by the Entity Framework runtime but are not supported by the EF Designer.</span></span> <span data-ttu-id="60a14-113">Pokud chcete použít TPC nebo smíšené dědičnosti, máte dvě možnosti: použijte Code First, nebo ručně upravit soubor EDMX.</span><span class="sxs-lookup"><span data-stu-id="60a14-113">If you want to use TPC or mixed inheritance, you have two options: use Code First, or manually edit the EDMX file.</span></span> <span data-ttu-id="60a14-114">Pokud budete chtít pracovat s souboru EDMX, v okně podrobností mapování zařadí do "nouzový režim" a nebude možné změnit mapování pomocí návrháře.</span><span class="sxs-lookup"><span data-stu-id="60a14-114">If you choose to work with the EDMX file, the Mapping Details Window will be put into “safe mode” and you will not be able to use the designer to change the mappings.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="60a14-115">Požadavky</span><span class="sxs-lookup"><span data-stu-id="60a14-115">Prerequisites</span></span>

<span data-ttu-id="60a14-116">K dokončení toho návodu budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="60a14-116">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="60a14-117">Nejnovější verzi sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="60a14-117">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="60a14-118">[Ukázkové databáze školy](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="60a14-118">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="60a14-119">Nastavení projektu</span><span class="sxs-lookup"><span data-stu-id="60a14-119">Set up the Project</span></span>

-   <span data-ttu-id="60a14-120">Otevřít Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="60a14-120">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="60a14-121">Vyberte **souboru -&gt; nové –&gt; projektu**</span><span class="sxs-lookup"><span data-stu-id="60a14-121">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="60a14-122">V levém podokně klikněte na tlačítko **Visual C\#** a pak vyberte **konzoly** šablony.</span><span class="sxs-lookup"><span data-stu-id="60a14-122">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="60a14-123">Zadejte **TPTDBFirstSample** jako název.</span><span class="sxs-lookup"><span data-stu-id="60a14-123">Enter **TPTDBFirstSample** as the name.</span></span>
-   <span data-ttu-id="60a14-124">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="60a14-124">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="60a14-125">Vytvoření modelu</span><span class="sxs-lookup"><span data-stu-id="60a14-125">Create a Model</span></span>

-   <span data-ttu-id="60a14-126">Klikněte pravým tlačítkem na projekt v Průzkumníku řešení a vyberte **Add -&gt; nová položka**.</span><span class="sxs-lookup"><span data-stu-id="60a14-126">Right-click the project in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="60a14-127">Vyberte **Data** v levé nabídce a pak vyberte **datový Model Entity ADO.NET** v podokně šablon.</span><span class="sxs-lookup"><span data-stu-id="60a14-127">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="60a14-128">Zadejte **TPTModel.edmx** pro název souboru a pak klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="60a14-128">Enter **TPTModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="60a14-129">V dialogovém okně Výběr obsahu modelu, vyberte ** Generovat z databáze ** a klepněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="60a14-129">In the Choose Model Contents dialog box, select** Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="60a14-130">Klikněte na tlačítko **nové připojení**.</span><span class="sxs-lookup"><span data-stu-id="60a14-130">Click **New Connection**.</span></span>
    <span data-ttu-id="60a14-131">V dialogovém okně Vlastnosti připojení zadat název serveru (například **(localdb)\\mssqllocaldb**), vyberte metodu ověřování, zadejte **School** pro název databáze a pak Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="60a14-131">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="60a14-132">Dialogové okno Vybrat datové připojení se aktualizuje se nastavení připojení databáze.</span><span class="sxs-lookup"><span data-stu-id="60a14-132">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="60a14-133">V dialogovém okně Zvolte vaše databázové objekty tabulky uzlu, vyberte **oddělení**, **kurzu OnlineCourse a OnsiteCourse** tabulky.</span><span class="sxs-lookup"><span data-stu-id="60a14-133">In the Choose Your Database Objects dialog box, under the Tables node, select the **Department**, **Course, OnlineCourse, and OnsiteCourse** tables.</span></span>
-   <span data-ttu-id="60a14-134">Klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="60a14-134">Click **Finish**.</span></span>

<span data-ttu-id="60a14-135">V návrháři entit, které poskytuje návrhové ploše pro úpravy váš model, se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="60a14-135">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="60a14-136">Všechny objekty, které jste vybrali v dialogovém okně Zvolte vaše databázové objekty jsou přidány do modelu.</span><span class="sxs-lookup"><span data-stu-id="60a14-136">All the objects that you selected in the Choose Your Database Objects dialog box are added to the model.</span></span>

## <a name="implement-table-per-type-inheritance"></a><span data-ttu-id="60a14-137">Implementace dědičnosti na typ tabulky</span><span class="sxs-lookup"><span data-stu-id="60a14-137">Implement Table-per-Type Inheritance</span></span>

-   <span data-ttu-id="60a14-138">Na návrhové ploše, klikněte pravým tlačítkem myši **OnlineCourse** typu entity a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="60a14-138">On the design surface, right-click the **OnlineCourse** entity type and select **Properties**.</span></span>
-   <span data-ttu-id="60a14-139">V **vlastnosti** okno, nastavte vlastnost typu Base na **kurzu**.</span><span class="sxs-lookup"><span data-stu-id="60a14-139">In the **Properties** window, set the Base Type property to **Course**.</span></span>
-   <span data-ttu-id="60a14-140">Klikněte pravým tlačítkem myši **OnsiteCourse** typu entity a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="60a14-140">Right-click the **OnsiteCourse** entity type and select **Properties**.</span></span>
-   <span data-ttu-id="60a14-141">V **vlastnosti** okno, nastavte vlastnost typu Base na **kurzu**.</span><span class="sxs-lookup"><span data-stu-id="60a14-141">In the **Properties** window, set the Base Type property to **Course**.</span></span>
-   <span data-ttu-id="60a14-142">Klikněte pravým tlačítkem na přidružení (řádek) mezi **OnlineCourse** a **kurzu** typy entit.</span><span class="sxs-lookup"><span data-stu-id="60a14-142">Right-click the association (the line) between the **OnlineCourse** and **Course** entity types.</span></span>
    <span data-ttu-id="60a14-143">Vyberte **odstranit z modelu**.</span><span class="sxs-lookup"><span data-stu-id="60a14-143">Select **Delete from Model**.</span></span>
-   <span data-ttu-id="60a14-144">Klikněte pravým tlačítkem na přidružení mezi **OnsiteCourse** a **kurzu** typy entit.</span><span class="sxs-lookup"><span data-stu-id="60a14-144">Right-click the association between the **OnsiteCourse** and **Course** entity types.</span></span>
    <span data-ttu-id="60a14-145">Vyberte **odstranit z modelu**.</span><span class="sxs-lookup"><span data-stu-id="60a14-145">Select **Delete from Model**.</span></span>

<span data-ttu-id="60a14-146">Teď odstraníme **CourseID** vlastnost z **OnlineCourse** a **OnsiteCourse** vzhledem k tomu, že tyto třídy dědí **CourseID** z **kurzu** základní typ.</span><span class="sxs-lookup"><span data-stu-id="60a14-146">We will now delete the **CourseID** property from **OnlineCourse** and **OnsiteCourse** because these classes inherit **CourseID** from the **Course** base type.</span></span>

-   <span data-ttu-id="60a14-147">Klikněte pravým tlačítkem myši **CourseID** vlastnost **OnlineCourse** typu entity a pak vyberte **odstranit z modelu**.</span><span class="sxs-lookup"><span data-stu-id="60a14-147">Right-click the **CourseID** property of the **OnlineCourse** entity type, and then select **Delete from Model**.</span></span>
-   <span data-ttu-id="60a14-148">Klikněte pravým tlačítkem myši **CourseID** vlastnost **OnsiteCourse** typu entity a pak vyberte **odstranit z modelu**</span><span class="sxs-lookup"><span data-stu-id="60a14-148">Right-click the **CourseID** property of the **OnsiteCourse** entity type, and then select **Delete from Model**</span></span>
-   <span data-ttu-id="60a14-149">Každý typ tabulky dědičnosti je nyní implementována.</span><span class="sxs-lookup"><span data-stu-id="60a14-149">Table-per-type inheritance is now implemented.</span></span>

![TPT](~/ef6/media/tpt.png)

## <a name="use-the-model"></a><span data-ttu-id="60a14-151">Použití modelu</span><span class="sxs-lookup"><span data-stu-id="60a14-151">Use the Model</span></span>

<span data-ttu-id="60a14-152">Otevřít **Program.cs** souboru, kde **hlavní** je definována metoda.</span><span class="sxs-lookup"><span data-stu-id="60a14-152">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="60a14-153">Vložte následující kód do **hlavní** funkce.</span><span class="sxs-lookup"><span data-stu-id="60a14-153">Paste the following code into the **Main** function.</span></span> <span data-ttu-id="60a14-154">Kód spustí tři dotazy.</span><span class="sxs-lookup"><span data-stu-id="60a14-154">The code executes three queries.</span></span> <span data-ttu-id="60a14-155">První dotaz vrací všechny **kurzy** související se zadaným oddělení.</span><span class="sxs-lookup"><span data-stu-id="60a14-155">The first query brings back all **Courses** related to the specified department.</span></span> <span data-ttu-id="60a14-156">Druhý dotaz používá **OfType** metoda vrátí **OnlineCourses** související se zadaným oddělení.</span><span class="sxs-lookup"><span data-stu-id="60a14-156">The second query uses the **OfType** method to return **OnlineCourses** related to the specified department.</span></span> <span data-ttu-id="60a14-157">Vrátí třetí dotaz **OnsiteCourses**.</span><span class="sxs-lookup"><span data-stu-id="60a14-157">The third query returns **OnsiteCourses**.</span></span>

``` csharp
    using (var context = new SchoolEntities())
    {
        foreach (var department in context.Departments)
        {
            Console.WriteLine("The {0} department has the following courses:",
                               department.Name);

            Console.WriteLine("   All courses");
            foreach (var course in department.Courses )
            {
                Console.WriteLine("     {0}", course.Title);
            }

            foreach (var course in department.Courses.
                OfType<OnlineCourse>())
            {
                Console.WriteLine("   Online - {0}", course.Title);
            }

            foreach (var course in department.Courses.
                OfType<OnsiteCourse>())
            {
                Console.WriteLine("   Onsite - {0}", course.Title);
            }
        }
    }
```
