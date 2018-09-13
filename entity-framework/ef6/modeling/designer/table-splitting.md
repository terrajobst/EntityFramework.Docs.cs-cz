---
title: Tabulka návrháře rozdělení - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 452f17c3-9f26-4de4-9894-8bc036e23b0f
ms.openlocfilehash: 8b0ca6778a06ed43b1365d2e5969ff15948f8004
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490691"
---
# <a name="designer-table-splitting"></a><span data-ttu-id="d97bc-102">Tabulka návrháře rozdělení</span><span class="sxs-lookup"><span data-stu-id="d97bc-102">Designer Table Splitting</span></span>
<span data-ttu-id="d97bc-103">Tento návod ukazuje, jak mapovat více typů entit na jedné tabulky tak, že upravíte model se Návrhář Entity Framework (EF designeru).</span><span class="sxs-lookup"><span data-stu-id="d97bc-103">This walkthrough shows how to map multiple entity types to a single table by modifying a model with the Entity Framework Designer (EF Designer).</span></span>

<span data-ttu-id="d97bc-104">Jeden z důvodů, proč můžete chtít použít tabulku rozdělení je zpoždění načítání některé vlastnosti, při použití opožděné načtení pro načtení objektů.</span><span class="sxs-lookup"><span data-stu-id="d97bc-104">One reason you may want to use table splitting is delaying the loading of some properties when using lazy loading to load your objects.</span></span> <span data-ttu-id="d97bc-105">Vlastnosti, které mohou obsahovat velmi velké množství dat do samostatné entity a načíst jenom ji v případě potřeby můžete oddělit.</span><span class="sxs-lookup"><span data-stu-id="d97bc-105">You can separate the properties that might contain very large amount of data into a seperate entity and only load it when required.</span></span>

<span data-ttu-id="d97bc-106">Následující obrázek znázorňuje hlavní windows, které se používají při práci s EF designeru.</span><span class="sxs-lookup"><span data-stu-id="d97bc-106">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EF designeru](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a><span data-ttu-id="d97bc-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d97bc-108">Prerequisites</span></span>

<span data-ttu-id="d97bc-109">K dokončení toho návodu budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="d97bc-109">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="d97bc-110">Nejnovější verzi sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d97bc-110">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="d97bc-111">[Ukázkové databáze školy](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="d97bc-111">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="d97bc-112">Nastavení projektu</span><span class="sxs-lookup"><span data-stu-id="d97bc-112">Set up the Project</span></span>

<span data-ttu-id="d97bc-113">Tento návod používá Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="d97bc-113">This walkthrough is using Visual Studio 2012.</span></span>

-   <span data-ttu-id="d97bc-114">Otevřít Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="d97bc-114">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="d97bc-115">Na **souboru** nabídky, přejděte k **nový**a potom klikněte na tlačítko **projektu**.</span><span class="sxs-lookup"><span data-stu-id="d97bc-115">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="d97bc-116">V levém podokně klikněte na tlačítko Visual C\#a pak vyberte šablonu Konzolová aplikace.</span><span class="sxs-lookup"><span data-stu-id="d97bc-116">In the left pane, click Visual C\#, and then select the Console Application template.</span></span>
-   <span data-ttu-id="d97bc-117">Zadejte **TableSplittingSample** jako název projektu a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="d97bc-117">Enter **TableSplittingSample** as the name of the project and click **OK**.</span></span>

## <a name="create-a-model-based-on-the-school-database"></a><span data-ttu-id="d97bc-118">Vytvořit Model založený na databáze školy</span><span class="sxs-lookup"><span data-stu-id="d97bc-118">Create a Model based on the School Database</span></span>

-   <span data-ttu-id="d97bc-119">Klikněte pravým tlačítkem na název projektu v Průzkumníku řešení, přejděte na **přidat**a potom klikněte na tlačítko **nová položka**.</span><span class="sxs-lookup"><span data-stu-id="d97bc-119">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="d97bc-120">Vyberte **Data** v levé nabídce a pak vyberte **datový Model Entity ADO.NET** v podokně šablon.</span><span class="sxs-lookup"><span data-stu-id="d97bc-120">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="d97bc-121">Zadejte **TableSplittingModel.edmx** pro název souboru a pak klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="d97bc-121">Enter **TableSplittingModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="d97bc-122">V dialogovém okně Výběr obsahu modelu vyberte **Generovat z databáze**a potom klikněte na tlačítko **Další.**</span><span class="sxs-lookup"><span data-stu-id="d97bc-122">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next.**</span></span>
-   <span data-ttu-id="d97bc-123">Klikněte na tlačítko nové připojení.</span><span class="sxs-lookup"><span data-stu-id="d97bc-123">Click New Connection.</span></span> <span data-ttu-id="d97bc-124">V dialogovém okně Vlastnosti připojení zadat název serveru (například **(localdb)\\mssqllocaldb**), vyberte metodu ověřování, zadejte **School** pro název databáze a pak Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="d97bc-124">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="d97bc-125">Dialogové okno Vybrat datové připojení se aktualizuje se nastavení připojení databáze.</span><span class="sxs-lookup"><span data-stu-id="d97bc-125">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="d97bc-126">V dialogovém okně Zvolte vaše databázové objekty Rozbalit **tabulky** uzlu a kontrolu **osoba** tabulky.</span><span class="sxs-lookup"><span data-stu-id="d97bc-126">In the Choose Your Database Objects dialog box, unfold the **Tables** node and check the **Person** table.</span></span> <span data-ttu-id="d97bc-127">Tím se přidá do zadané tabulky **School** modelu.</span><span class="sxs-lookup"><span data-stu-id="d97bc-127">This will add the specified table to the **School** model.</span></span>
-   <span data-ttu-id="d97bc-128">Klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="d97bc-128">Click **Finish**.</span></span>

<span data-ttu-id="d97bc-129">V návrháři entit, které poskytuje návrhové ploše pro úpravy váš model, se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="d97bc-129">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="d97bc-130">Všechny objekty, které jste vybrali v **zvolte vaše databázové objekty** dialogové okno se přidají do modelu.</span><span class="sxs-lookup"><span data-stu-id="d97bc-130">All the objects that you selected in the **Choose Your Database Objects** dialog box are added to the model.</span></span>

## <a name="map-two-entities-to-a-single-table"></a><span data-ttu-id="d97bc-131">Dvě entity, které mapují na jedné tabulky</span><span class="sxs-lookup"><span data-stu-id="d97bc-131">Map Two Entities to a Single Table</span></span>

<span data-ttu-id="d97bc-132">V této části se rozdělí **osoba** entity do dvou entit a jejich namapování na jednu tabulku.</span><span class="sxs-lookup"><span data-stu-id="d97bc-132">In this section you will split the **Person** entity into two entities and then map them to a single table.</span></span>

> [!NOTE]
> <span data-ttu-id="d97bc-133">**Osoba** entita neobsahuje žádné vlastnosti, které mohou obsahovat velké množství dat; používá se jenom jako příklad.</span><span class="sxs-lookup"><span data-stu-id="d97bc-133">The **Person** entity does not contain any properties that may contain large amount of data; it is just used as an example.</span></span>

-   <span data-ttu-id="d97bc-134">Klikněte pravým tlačítkem na prázdnou oblast návrhové plochy, přejděte na **přidat nový**a klikněte na tlačítko **Entity**.</span><span class="sxs-lookup"><span data-stu-id="d97bc-134">Right-click an empty area of the design surface, point to **Add New**, and click **Entity**.</span></span>
    <span data-ttu-id="d97bc-135">**Novou entitu** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d97bc-135">The **New Entity** dialog box appears.</span></span>
-   <span data-ttu-id="d97bc-136">Typ **HireInfo** pro **název Entity** a **PersonID** pro **vlastnost Key** název.</span><span class="sxs-lookup"><span data-stu-id="d97bc-136">Type **HireInfo** for the **Entity name** and **PersonID** for the **Key Property** name.</span></span>
-   <span data-ttu-id="d97bc-137">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="d97bc-137">Click **OK**.</span></span>
-   <span data-ttu-id="d97bc-138">Nový typ entity se vytvoří a zobrazí na návrhové ploše.</span><span class="sxs-lookup"><span data-stu-id="d97bc-138">A new entity type is created and displayed on the design surface.</span></span>
-   <span data-ttu-id="d97bc-139">Vyberte **HireDate** vlastnost **osoba** typu entity a stiskněte klávesu **Ctrl + X** klíče.</span><span class="sxs-lookup"><span data-stu-id="d97bc-139">Select the **HireDate** property of the **Person** entity type and press **Ctrl+X** keys.</span></span>
-   <span data-ttu-id="d97bc-140">Vyberte **HireInfo** entity a stiskněte klávesu **Ctrl + V** klíče.</span><span class="sxs-lookup"><span data-stu-id="d97bc-140">Select the **HireInfo** entity and press **Ctrl+V** keys.</span></span>
-   <span data-ttu-id="d97bc-141">Vytvoření přidružení mezi **osoba** a **HireInfo**.</span><span class="sxs-lookup"><span data-stu-id="d97bc-141">Create an association between **Person** and **HireInfo**.</span></span> <span data-ttu-id="d97bc-142">Chcete-li to provést, klikněte pravým tlačítkem na prázdnou oblast návrhové plochy, přejděte na **přidat nový**a klikněte na tlačítko **přidružení**.</span><span class="sxs-lookup"><span data-stu-id="d97bc-142">To do this, right-click an empty area of the design surface, point to **Add New**, and click **Association**.</span></span>
-   <span data-ttu-id="d97bc-143">**Přidat přidružení** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d97bc-143">The **Add Association** dialog box appears.</span></span> <span data-ttu-id="d97bc-144">**PersonHireInfo** ve výchozím nastavení je zadaný název.</span><span class="sxs-lookup"><span data-stu-id="d97bc-144">The **PersonHireInfo** name is given by default.</span></span>
-   <span data-ttu-id="d97bc-145">Zadejte násobnost **1(One)** na obou stranách relace.</span><span class="sxs-lookup"><span data-stu-id="d97bc-145">Specify multiplicity **1(One)** on both ends of the relationship.</span></span>
-   <span data-ttu-id="d97bc-146">Stisknutím klávesy **OK**.</span><span class="sxs-lookup"><span data-stu-id="d97bc-146">Press **OK**.</span></span>

<span data-ttu-id="d97bc-147">Vyžaduje další krok **podrobnosti mapování** okna.</span><span class="sxs-lookup"><span data-stu-id="d97bc-147">The next step requires the **Mapping Details** window.</span></span> <span data-ttu-id="d97bc-148">Pokud toto okno nelze zobrazit, klikněte pravým tlačítkem na návrhové ploše a vyberte **podrobnosti mapování**.</span><span class="sxs-lookup"><span data-stu-id="d97bc-148">If you cannot see this window, right-click the design surface and select **Mapping Details**.</span></span>

-   <span data-ttu-id="d97bc-149">Vyberte **HireInfo** typu entity a klikněte na tlačítko **&lt;přidat tabulku nebo zobrazení&gt;** v **podrobnosti mapování** okna.</span><span class="sxs-lookup"><span data-stu-id="d97bc-149">Select the **HireInfo** entity type and click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="d97bc-150">Vyberte **osoba** z **&lt;přidat tabulku nebo zobrazení&gt;** pole rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="d97bc-150">Select **Person** from the **&lt;Add a Table or View&gt;** field drop-down list.</span></span> <span data-ttu-id="d97bc-151">Seznam obsahuje tabulky nebo zobrazení pro které je možné mapovat vybrané entity.</span><span class="sxs-lookup"><span data-stu-id="d97bc-151">The list contains tables or views to which the selected entity can be mapped.</span></span>
    <span data-ttu-id="d97bc-152">Příslušné vlastnosti by měly být namapované ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="d97bc-152">The appropriate properties should be mapped by default.</span></span>

    ![Mapování](~/ef6/media/mapping.png)

-   <span data-ttu-id="d97bc-154">Vyberte **PersonHireInfo** přidružení na návrhové ploše.</span><span class="sxs-lookup"><span data-stu-id="d97bc-154">Select the **PersonHireInfo** association on the design surface.</span></span>
-   <span data-ttu-id="d97bc-155">Klikněte pravým tlačítkem na přidružení na návrhové ploše a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="d97bc-155">Right-click the association on the design surface and select **Properties**.</span></span>
-   <span data-ttu-id="d97bc-156">V **vlastnosti** okna, vyberte **referenční omezení** vlastnosti a klikněte na tlačítko se třemi tečkami.</span><span class="sxs-lookup"><span data-stu-id="d97bc-156">In the **Properties** window, select the **Referential Constraints** property and click the ellipses button.</span></span>
-   <span data-ttu-id="d97bc-157">Vyberte **osoba** z **hlavní** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="d97bc-157">Select **Person** from the **Principal** drop-down list.</span></span>
-   <span data-ttu-id="d97bc-158">Stisknutím klávesy **OK**.</span><span class="sxs-lookup"><span data-stu-id="d97bc-158">Press **OK**.</span></span>

 

## <a name="use-the-model"></a><span data-ttu-id="d97bc-159">Použití modelu</span><span class="sxs-lookup"><span data-stu-id="d97bc-159">Use the Model</span></span>

-   <span data-ttu-id="d97bc-160">Vložte následující kód do metody Main.</span><span class="sxs-lookup"><span data-stu-id="d97bc-160">Paste the following code in the Main method.</span></span>

``` csharp
    using (var context = new SchoolEntities())
    {
        Person person = new Person()
        {
            FirstName = "Kimberly",
            LastName = "Morgan",
            Discriminator = "Instructor",
        };

        person.HireInfo = new HireInfo()
        {
            HireDate = DateTime.Now
        };

        // Add the new person to the context.
        context.People.Add(person);

        // Insert a row into the Person table.  
        context.SaveChanges();

        // Execute a query against the Person table.
        // The query returns columns that map to the Person entity.
        var existingPerson = context.People.FirstOrDefault();

        // Execute a query against the Person table.
        // The query returns columns that map to the Instructor entity.
        var hireInfo = existingPerson.HireInfo;

        Console.WriteLine("{0} was hired on {1}",
            existingPerson.LastName, hireInfo.HireDate);
    }
```
-   <span data-ttu-id="d97bc-161">Kompilace a spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="d97bc-161">Compile and run the application.</span></span>

<span data-ttu-id="d97bc-162">Následující příkazy T-SQL, které byly spuštěny před **School** databáze jako výsledek spuštění této aplikace.</span><span class="sxs-lookup"><span data-stu-id="d97bc-162">The following T-SQL statements were executed against the **School** database as a result of running this application.</span></span> 

-   <span data-ttu-id="d97bc-163">Následující **vložit** se spustil v důsledku spuštění kontextu. SaveChanges() a kombinuje data z **osoba** a **HireInfo** entity</span><span class="sxs-lookup"><span data-stu-id="d97bc-163">The following **INSERT** was executed as a result of executing context.SaveChanges() and combines data from the **Person** and **HireInfo** entities</span></span>

    ![Insert](~/ef6/media/insert.png)

-   <span data-ttu-id="d97bc-165">Následující **vyberte** se spustil v důsledku spuštění kontextu. People.FirstOrDefault() a vybere pouze sloupce mapovat na **osoby**</span><span class="sxs-lookup"><span data-stu-id="d97bc-165">The following **SELECT** was executed as a result of executing context.People.FirstOrDefault() and selects just the columns mapped to **Person**</span></span>

    ![Vyberte možnost 1](~/ef6/media/select1.png)

-   <span data-ttu-id="d97bc-167">Následující **vyberte** byl proveden v důsledku přístup k existingPerson.Instructor vlastnost navigace a vybere pouze sloupce, které jsou namapované na **HireInfo**</span><span class="sxs-lookup"><span data-stu-id="d97bc-167">The following **SELECT** was executed as a result of accessing the navigation property existingPerson.Instructor and selects just the columns mapped to **HireInfo**</span></span>

    ![Vyberte 2](~/ef6/media/select2.png)
