---
title: Dědičnost TPT v Návrháři – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: efc78c31-b4ea-4ea3-a0cd-c69eb507020e
ms.openlocfilehash: 84330fba4807620aa242a70cd8ac76a60284416d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418208"
---
# <a name="designer-tpt-inheritance"></a><span data-ttu-id="ffd26-102">Dědičnost TPT v Návrháři</span><span class="sxs-lookup"><span data-stu-id="ffd26-102">Designer TPT Inheritance</span></span>
<span data-ttu-id="ffd26-103">V tomto podrobném návodu se dozvíte, jak implementovat dědičnost tabulek na typ (TPT) v modelu pomocí Entity Framework Designer (EF Designer).</span><span class="sxs-lookup"><span data-stu-id="ffd26-103">This step-by-step walkthrough shows how to implement table-per-type (TPT) inheritance in your model using the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="ffd26-104">Dědičnost typu tabulka na typ používá samostatnou tabulku v databázi pro zachování dat pro nezděděné vlastnosti a klíčové vlastnosti pro každý typ v hierarchii dědičnosti.</span><span class="sxs-lookup"><span data-stu-id="ffd26-104">Table-per-type inheritance uses a separate table in the database to maintain data for non-inherited properties and key properties for each type in the inheritance hierarchy.</span></span>

<span data-ttu-id="ffd26-105">V tomto návodu namapujeme **kurz** (základní typ), **OnlineCourse** (odvozený od kurzu) a **OnsiteCourse** (z **kurzu**) na tabulky se stejnými názvy.</span><span class="sxs-lookup"><span data-stu-id="ffd26-105">In this walkthrough we will map the **Course** (base type), **OnlineCourse** (derives from Course), and **OnsiteCourse** (derives from **Course**) entities to tables with the same names.</span></span> <span data-ttu-id="ffd26-106">Vytvoříme model z databáze a pak upravíte model pro implementaci TPT dědičnosti.</span><span class="sxs-lookup"><span data-stu-id="ffd26-106">We'll create a model from the database and then alter the model to implement the TPT inheritance.</span></span>

<span data-ttu-id="ffd26-107">Můžete také začít s Model First a potom databázi vygenerovat z modelu.</span><span class="sxs-lookup"><span data-stu-id="ffd26-107">You can also start with the Model First and then generate the database from the model.</span></span> <span data-ttu-id="ffd26-108">Návrhář EF používá ve výchozím nastavení strategii TPT, takže jakákoli dědičnost v modelu bude namapována na samostatné tabulky.</span><span class="sxs-lookup"><span data-stu-id="ffd26-108">The EF Designer uses the TPT strategy by default and so any inheritance in the model will be mapped to separate tables.</span></span>

## <a name="other-inheritance-options"></a><span data-ttu-id="ffd26-109">Další možnosti dědičnosti</span><span class="sxs-lookup"><span data-stu-id="ffd26-109">Other Inheritance Options</span></span>

<span data-ttu-id="ffd26-110">Tabulka na hierarchii (TPH) je jiný typ dědičnosti, ve kterém se používá jedna databázová tabulka k údržbě dat pro všechny typy entit v hierarchii dědičnosti.</span><span class="sxs-lookup"><span data-stu-id="ffd26-110">Table-per-Hierarchy (TPH) is another type of inheritance in which one database table is used to maintain data for all of the entity types in an inheritance hierarchy.</span></span><span data-ttu-id="ffd26-111">  Informace o tom, jak namapovat dědění tabulky na hierarchii s Entity Designer, naleznete v tématu [dědičnosti TPH v nástroji EF Designer](~/ef6/modeling/designer/inheritance/tph.md).</span><span class="sxs-lookup"><span data-stu-id="ffd26-111">  For information about how to map Table-per-Hierarchy inheritance with the Entity Designer, see [EF Designer TPH Inheritance](~/ef6/modeling/designer/inheritance/tph.md).</span></span> 

<span data-ttu-id="ffd26-112">Všimněte si, že modul runtime Entity Framework podporuje dědičnost typu tabulky podle konkrétního typu (TPC) a smíšené modely dědičnosti, ale Návrhář EF je nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="ffd26-112">Note that, the Table-per-Concrete Type Inheritance (TPC) and mixed inheritance models are supported by the Entity Framework runtime but are not supported by the EF Designer.</span></span> <span data-ttu-id="ffd26-113">Pokud chcete použít TPC nebo smíšenou dědičnost, máte dvě možnosti: použijte Code First nebo ručně upravte soubor EDMX.</span><span class="sxs-lookup"><span data-stu-id="ffd26-113">If you want to use TPC or mixed inheritance, you have two options: use Code First, or manually edit the EDMX file.</span></span> <span data-ttu-id="ffd26-114">Pokud se rozhodnete pracovat se souborem EDMX, bude okno Podrobnosti mapování přepnuto do bezpečného režimu a nebudete moci použít návrháře ke změně mapování.</span><span class="sxs-lookup"><span data-stu-id="ffd26-114">If you choose to work with the EDMX file, the Mapping Details Window will be put into “safe mode” and you will not be able to use the designer to change the mappings.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ffd26-115">Předpoklady</span><span class="sxs-lookup"><span data-stu-id="ffd26-115">Prerequisites</span></span>

<span data-ttu-id="ffd26-116">K dokončení toho návodu budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="ffd26-116">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="ffd26-117">Poslední verze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ffd26-117">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="ffd26-118">[Ukázková databáze školy](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="ffd26-118">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="ffd26-119">Nastavení projektu</span><span class="sxs-lookup"><span data-stu-id="ffd26-119">Set up the Project</span></span>

-   <span data-ttu-id="ffd26-120">Otevřete Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="ffd26-120">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="ffd26-121">Vybrat **soubor-&gt; projekt New-&gt;**</span><span class="sxs-lookup"><span data-stu-id="ffd26-121">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="ffd26-122">V levém podokně klikněte na položku **Visual C\#** a pak vyberte šablonu **konzoly** .</span><span class="sxs-lookup"><span data-stu-id="ffd26-122">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="ffd26-123">Jako název zadejte **TPTDBFirstSample** .</span><span class="sxs-lookup"><span data-stu-id="ffd26-123">Enter **TPTDBFirstSample** as the name.</span></span>
-   <span data-ttu-id="ffd26-124">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="ffd26-124">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="ffd26-125">Vytvoření modelu</span><span class="sxs-lookup"><span data-stu-id="ffd26-125">Create a Model</span></span>

-   <span data-ttu-id="ffd26-126">V Průzkumník řešení klikněte pravým tlačítkem na projekt a vyberte **Přidat-&gt; nová položka**.</span><span class="sxs-lookup"><span data-stu-id="ffd26-126">Right-click the project in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="ffd26-127">V nabídce vlevo vyberte **data** a v podokně šablony vyberte **ADO.NET model EDM (Entity Data Model)** .</span><span class="sxs-lookup"><span data-stu-id="ffd26-127">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="ffd26-128">Jako název souboru zadejte **TPTModel. edmx** a pak klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="ffd26-128">Enter **TPTModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="ffd26-129">V dialogovém okně Vybrat obsah modelu vyberte možnost ** generovat z databáze**a poté klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="ffd26-129">In the Choose Model Contents dialog box, select** Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="ffd26-130">Klikněte na **nové připojení**.</span><span class="sxs-lookup"><span data-stu-id="ffd26-130">Click **New Connection**.</span></span>
    <span data-ttu-id="ffd26-131">V dialogovém okně Vlastnosti připojení zadejte název serveru (například **(LocalDB)\\mssqllocaldb**), vyberte metodu ověřování, jako název databáze zadejte **School** a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="ffd26-131">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="ffd26-132">Dialogové okno zvolit datové připojení je aktualizováno nastavením připojení k databázi.</span><span class="sxs-lookup"><span data-stu-id="ffd26-132">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="ffd26-133">V dialogovém okně zvolte objekty databáze v uzlu tabulky vyberte tabulky **oddělení**, **kurz, OnlineCourse a OnsiteCourse** .</span><span class="sxs-lookup"><span data-stu-id="ffd26-133">In the Choose Your Database Objects dialog box, under the Tables node, select the **Department**, **Course, OnlineCourse, and OnsiteCourse** tables.</span></span>
-   <span data-ttu-id="ffd26-134">Klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="ffd26-134">Click **Finish**.</span></span>

<span data-ttu-id="ffd26-135">Zobrazí se Entity Designer, která poskytuje návrhovou plochu pro úpravu vašeho modelu.</span><span class="sxs-lookup"><span data-stu-id="ffd26-135">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="ffd26-136">Do modelu jsou přidány všechny objekty, které jste vybrali v dialogovém okně zvolit objekty databáze.</span><span class="sxs-lookup"><span data-stu-id="ffd26-136">All the objects that you selected in the Choose Your Database Objects dialog box are added to the model.</span></span>

## <a name="implement-table-per-type-inheritance"></a><span data-ttu-id="ffd26-137">Implementace dědičnosti tabulek na typ</span><span class="sxs-lookup"><span data-stu-id="ffd26-137">Implement Table-per-Type Inheritance</span></span>

-   <span data-ttu-id="ffd26-138">Na návrhové ploše klikněte pravým tlačítkem myši na typ entity **OnlineCourse** a vyberte možnost **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="ffd26-138">On the design surface, right-click the **OnlineCourse** entity type and select **Properties**.</span></span>
-   <span data-ttu-id="ffd26-139">V okně **vlastnosti** nastavte vlastnost základní typ na **kurz**.</span><span class="sxs-lookup"><span data-stu-id="ffd26-139">In the **Properties** window, set the Base Type property to **Course**.</span></span>
-   <span data-ttu-id="ffd26-140">Klikněte pravým tlačítkem myši na typ entity **OnsiteCourse** a vyberte možnost **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="ffd26-140">Right-click the **OnsiteCourse** entity type and select **Properties**.</span></span>
-   <span data-ttu-id="ffd26-141">V okně **vlastnosti** nastavte vlastnost základní typ na **kurz**.</span><span class="sxs-lookup"><span data-stu-id="ffd26-141">In the **Properties** window, set the Base Type property to **Course**.</span></span>
-   <span data-ttu-id="ffd26-142">Klikněte pravým tlačítkem na přidružení (spojnici) mezi typy entit **OnlineCourse** a **Course** .</span><span class="sxs-lookup"><span data-stu-id="ffd26-142">Right-click the association (the line) between the **OnlineCourse** and **Course** entity types.</span></span>
    <span data-ttu-id="ffd26-143">Vyberte možnost **Odstranit z modelu**.</span><span class="sxs-lookup"><span data-stu-id="ffd26-143">Select **Delete from Model**.</span></span>
-   <span data-ttu-id="ffd26-144">Klikněte pravým tlačítkem na přidružení mezi typy entit **OnsiteCourse** a **Course** .</span><span class="sxs-lookup"><span data-stu-id="ffd26-144">Right-click the association between the **OnsiteCourse** and **Course** entity types.</span></span>
    <span data-ttu-id="ffd26-145">Vyberte možnost **Odstranit z modelu**.</span><span class="sxs-lookup"><span data-stu-id="ffd26-145">Select **Delete from Model**.</span></span>

<span data-ttu-id="ffd26-146">Nyní odstraníme vlastnost **CourseID** z **OnlineCourse** a **OnsiteCourse** , protože tyto třídy dědí **CourseID** od typu základ **kurzu** .</span><span class="sxs-lookup"><span data-stu-id="ffd26-146">We will now delete the **CourseID** property from **OnlineCourse** and **OnsiteCourse** because these classes inherit **CourseID** from the **Course** base type.</span></span>

-   <span data-ttu-id="ffd26-147">Klikněte pravým tlačítkem na vlastnost **CourseID** typu entity **OnlineCourse** a pak vyberte **Odstranit z modelu**.</span><span class="sxs-lookup"><span data-stu-id="ffd26-147">Right-click the **CourseID** property of the **OnlineCourse** entity type, and then select **Delete from Model**.</span></span>
-   <span data-ttu-id="ffd26-148">Klikněte pravým tlačítkem na vlastnost **CourseID** typu entity **OnsiteCourse** a pak vyberte **Odstranit z modelu** .</span><span class="sxs-lookup"><span data-stu-id="ffd26-148">Right-click the **CourseID** property of the **OnsiteCourse** entity type, and then select **Delete from Model**</span></span>
-   <span data-ttu-id="ffd26-149">Dědičnost typu tabulka-na typ je nyní implementována.</span><span class="sxs-lookup"><span data-stu-id="ffd26-149">Table-per-type inheritance is now implemented.</span></span>

![TPT](~/ef6/media/tpt.png)

## <a name="use-the-model"></a><span data-ttu-id="ffd26-151">Použití modelu</span><span class="sxs-lookup"><span data-stu-id="ffd26-151">Use the Model</span></span>

<span data-ttu-id="ffd26-152">Otevřete soubor **program.cs** , kde je definována metoda **Main** .</span><span class="sxs-lookup"><span data-stu-id="ffd26-152">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="ffd26-153">Do funkce **Main** vložte následující kód.</span><span class="sxs-lookup"><span data-stu-id="ffd26-153">Paste the following code into the **Main** function.</span></span> <span data-ttu-id="ffd26-154">Kód spouští tři dotazy.</span><span class="sxs-lookup"><span data-stu-id="ffd26-154">The code executes three queries.</span></span> <span data-ttu-id="ffd26-155">První dotaz vrátí zpět všechny **kurzy** týkající se určeného oddělení.</span><span class="sxs-lookup"><span data-stu-id="ffd26-155">The first query brings back all **Courses** related to the specified department.</span></span> <span data-ttu-id="ffd26-156">Druhý dotaz používá metodu **OfType** k vrácení **OnlineCourses** souvisejících se zadaným oddělením.</span><span class="sxs-lookup"><span data-stu-id="ffd26-156">The second query uses the **OfType** method to return **OnlineCourses** related to the specified department.</span></span> <span data-ttu-id="ffd26-157">Třetí dotaz vrátí **OnsiteCourses**.</span><span class="sxs-lookup"><span data-stu-id="ffd26-157">The third query returns **OnsiteCourses**.</span></span>

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
