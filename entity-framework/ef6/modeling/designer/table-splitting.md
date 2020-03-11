---
title: Rozdělení tabulky návrháře – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 452f17c3-9f26-4de4-9894-8bc036e23b0f
ms.openlocfilehash: f5e7532e6c0b473d8ce77cbd11e3e673b0af6cbe
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418166"
---
# <a name="designer-table-splitting"></a><span data-ttu-id="be81e-102">Rozdělení tabulky návrháře</span><span class="sxs-lookup"><span data-stu-id="be81e-102">Designer Table Splitting</span></span>
<span data-ttu-id="be81e-103">Tento návod ukazuje, jak namapovat více typů entit na jednu tabulku úpravou modelu pomocí Entity Framework Designer (EF Designer).</span><span class="sxs-lookup"><span data-stu-id="be81e-103">This walkthrough shows how to map multiple entity types to a single table by modifying a model with the Entity Framework Designer (EF Designer).</span></span>

<span data-ttu-id="be81e-104">Jedním z důvodů, proč můžete chtít použít rozdělování tabulky, je zpoždění načítání některých vlastností při použití opožděného načítání pro načtení vašich objektů.</span><span class="sxs-lookup"><span data-stu-id="be81e-104">One reason you may want to use table splitting is delaying the loading of some properties when using lazy loading to load your objects.</span></span><span data-ttu-id="be81e-105"> Vlastnosti, které mohou obsahovat velmi velké množství dat, můžete oddělit do samostatné entity a v případě potřeby je načíst.</span><span class="sxs-lookup"><span data-stu-id="be81e-105"> You can separate the properties that might contain very large amount of data into a separate entity and only load it when required.</span></span>

<span data-ttu-id="be81e-106">Následující obrázek ukazuje hlavní okna, která se používají při práci s návrhářem EF.</span><span class="sxs-lookup"><span data-stu-id="be81e-106">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![Návrhář EF](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a><span data-ttu-id="be81e-108">Předpoklady</span><span class="sxs-lookup"><span data-stu-id="be81e-108">Prerequisites</span></span>

<span data-ttu-id="be81e-109">K dokončení toho návodu budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="be81e-109">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="be81e-110">Poslední verze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="be81e-110">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="be81e-111">[Ukázková databáze školy](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="be81e-111">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="be81e-112">Nastavení projektu</span><span class="sxs-lookup"><span data-stu-id="be81e-112">Set up the Project</span></span>

<span data-ttu-id="be81e-113">Tento návod používá Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="be81e-113">This walkthrough is using Visual Studio 2012.</span></span>

-   <span data-ttu-id="be81e-114">Otevřete Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="be81e-114">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="be81e-115">V nabídce **soubor** přejděte na příkaz **Nový**a klikněte na **projekt**.</span><span class="sxs-lookup"><span data-stu-id="be81e-115">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="be81e-116">V levém podokně klikněte na položku Visual C\#a pak vyberte šablonu Konzolová aplikace.</span><span class="sxs-lookup"><span data-stu-id="be81e-116">In the left pane, click Visual C\#, and then select the Console Application template.</span></span>
-   <span data-ttu-id="be81e-117">Jako název projektu zadejte **TableSplittingSample** a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="be81e-117">Enter **TableSplittingSample** as the name of the project and click **OK**.</span></span>

## <a name="create-a-model-based-on-the-school-database"></a><span data-ttu-id="be81e-118">Vytvoření modelu založeného na školní databázi</span><span class="sxs-lookup"><span data-stu-id="be81e-118">Create a Model based on the School Database</span></span>

-   <span data-ttu-id="be81e-119">Klikněte pravým tlačítkem myši na název projektu v Průzkumník řešení, přejděte na **Přidat**a klikněte na **Nová položka**.</span><span class="sxs-lookup"><span data-stu-id="be81e-119">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="be81e-120">V nabídce vlevo vyberte **data** a v podokně šablony vyberte **ADO.NET model EDM (Entity Data Model)** .</span><span class="sxs-lookup"><span data-stu-id="be81e-120">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="be81e-121">Jako název souboru zadejte **TableSplittingModel. edmx** a pak klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="be81e-121">Enter **TableSplittingModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="be81e-122">V dialogovém okně Vybrat obsah modelu vyberte možnost **Generovat z databáze**a poté klikněte na tlačítko **Další.**</span><span class="sxs-lookup"><span data-stu-id="be81e-122">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next.**</span></span>
-   <span data-ttu-id="be81e-123">Klikněte na nové připojení.</span><span class="sxs-lookup"><span data-stu-id="be81e-123">Click New Connection.</span></span> <span data-ttu-id="be81e-124">V dialogovém okně Vlastnosti připojení zadejte název serveru (například **(LocalDB)\\mssqllocaldb**), vyberte metodu ověřování, jako název databáze zadejte **School** a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="be81e-124">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="be81e-125">Dialogové okno zvolit datové připojení je aktualizováno nastavením připojení k databázi.</span><span class="sxs-lookup"><span data-stu-id="be81e-125">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="be81e-126">V dialogovém okně zvolit objekty databáze rozložte **tabulky** uzel a ověřte tabulku **Person** .</span><span class="sxs-lookup"><span data-stu-id="be81e-126">In the Choose Your Database Objects dialog box, unfold the **Tables** node and check the **Person** table.</span></span> <span data-ttu-id="be81e-127">Tím se přidá zadaná tabulka do **školního** modelu.</span><span class="sxs-lookup"><span data-stu-id="be81e-127">This will add the specified table to the **School** model.</span></span>
-   <span data-ttu-id="be81e-128">Klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="be81e-128">Click **Finish**.</span></span>

<span data-ttu-id="be81e-129">Zobrazí se Entity Designer, která poskytuje návrhovou plochu pro úpravu vašeho modelu.</span><span class="sxs-lookup"><span data-stu-id="be81e-129">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="be81e-130">Do modelu jsou přidány všechny objekty, které jste vybrali v dialogovém okně **zvolit objekty databáze** .</span><span class="sxs-lookup"><span data-stu-id="be81e-130">All the objects that you selected in the **Choose Your Database Objects** dialog box are added to the model.</span></span>

## <a name="map-two-entities-to-a-single-table"></a><span data-ttu-id="be81e-131">Mapování dvou entit na jednu tabulku</span><span class="sxs-lookup"><span data-stu-id="be81e-131">Map Two Entities to a Single Table</span></span>

<span data-ttu-id="be81e-132">V této části budete entitu **osoba** rozdělit do dvou entit a pak je namapovat na jednu tabulku.</span><span class="sxs-lookup"><span data-stu-id="be81e-132">In this section you will split the **Person** entity into two entities and then map them to a single table.</span></span>

> [!NOTE]
> <span data-ttu-id="be81e-133">Entita **osoby** neobsahuje žádné vlastnosti, které by mohly obsahovat velké množství dat. slouží pouze jako příklad.</span><span class="sxs-lookup"><span data-stu-id="be81e-133">The **Person** entity does not contain any properties that may contain large amount of data; it is just used as an example.</span></span>

-   <span data-ttu-id="be81e-134">Klikněte pravým tlačítkem myši na prázdnou oblast návrhové plochy, přejděte na **Přidat nový**a klikněte na **entita**.</span><span class="sxs-lookup"><span data-stu-id="be81e-134">Right-click an empty area of the design surface, point to **Add New**, and click **Entity**.</span></span>
    <span data-ttu-id="be81e-135">Zobrazí se dialogové okno **nová entity** .</span><span class="sxs-lookup"><span data-stu-id="be81e-135">The **New Entity** dialog box appears.</span></span>
-   <span data-ttu-id="be81e-136">Jako **název entity** zadejte **HireInfo** a pro název **Klíčové vlastnosti** **PersonID** .</span><span class="sxs-lookup"><span data-stu-id="be81e-136">Type **HireInfo** for the **Entity name** and **PersonID** for the **Key Property** name.</span></span>
-   <span data-ttu-id="be81e-137">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="be81e-137">Click **OK**.</span></span>
-   <span data-ttu-id="be81e-138">Vytvoří se nový typ entity, který se zobrazí na návrhové ploše.</span><span class="sxs-lookup"><span data-stu-id="be81e-138">A new entity type is created and displayed on the design surface.</span></span>
-   <span data-ttu-id="be81e-139">Vyberte vlastnost **zaměstnánod**  **osoby** typu entity a stiskněte klávesy **CTRL + X** .</span><span class="sxs-lookup"><span data-stu-id="be81e-139">Select the **HireDate** property of the **Person** entity type and press **Ctrl+X** keys.</span></span>
-   <span data-ttu-id="be81e-140">Vyberte entitu **HireInfo** a stiskněte klávesy **CTRL + V** .</span><span class="sxs-lookup"><span data-stu-id="be81e-140">Select the **HireInfo** entity and press **Ctrl+V** keys.</span></span>
-   <span data-ttu-id="be81e-141">Vytvořte přidružení mezi **osobami** a **HireInfo**.</span><span class="sxs-lookup"><span data-stu-id="be81e-141">Create an association between **Person** and **HireInfo**.</span></span> <span data-ttu-id="be81e-142">Provedete to tak, že kliknete pravým tlačítkem myši na prázdnou oblast návrhové plochy, najeďte na **Přidat nový**a kliknete na **asociace**.</span><span class="sxs-lookup"><span data-stu-id="be81e-142">To do this, right-click an empty area of the design surface, point to **Add New**, and click **Association**.</span></span>
-   <span data-ttu-id="be81e-143">Zobrazí se dialogové okno **přidat přidružení** .</span><span class="sxs-lookup"><span data-stu-id="be81e-143">The **Add Association** dialog box appears.</span></span> <span data-ttu-id="be81e-144">Ve výchozím nastavení je zadán název **PersonHireInfo** .</span><span class="sxs-lookup"><span data-stu-id="be81e-144">The **PersonHireInfo** name is given by default.</span></span>
-   <span data-ttu-id="be81e-145">Zadejte násobnost **1 (jedna)** na obou koncích relace.</span><span class="sxs-lookup"><span data-stu-id="be81e-145">Specify multiplicity **1(One)** on both ends of the relationship.</span></span>
-   <span data-ttu-id="be81e-146">Stiskněte **OK**.</span><span class="sxs-lookup"><span data-stu-id="be81e-146">Press **OK**.</span></span>

<span data-ttu-id="be81e-147">Další krok vyžaduje okno s **podrobnostmi mapování** .</span><span class="sxs-lookup"><span data-stu-id="be81e-147">The next step requires the **Mapping Details** window.</span></span> <span data-ttu-id="be81e-148">Pokud toto okno nevidíte, klikněte pravým tlačítkem myši na návrhovou plochu a vyberte **mapování podrobností**.</span><span class="sxs-lookup"><span data-stu-id="be81e-148">If you cannot see this window, right-click the design surface and select **Mapping Details**.</span></span>

-   <span data-ttu-id="be81e-149">Vyberte typ entity **HireInfo** a v okně **Podrobnosti mapování** klikněte **&lt;přidat tabulku nebo zobrazení&gt;**  .</span><span class="sxs-lookup"><span data-stu-id="be81e-149">Select the **HireInfo** entity type and click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="be81e-150">V rozevíracím seznamu **&lt;přidat tabulku nebo zobrazení&gt;**  pole vyberte **osoba** .</span><span class="sxs-lookup"><span data-stu-id="be81e-150">Select **Person** from the **&lt;Add a Table or View&gt;** field drop-down list.</span></span> <span data-ttu-id="be81e-151">Seznam obsahuje tabulky nebo zobrazení, do kterých lze mapovat vybranou entitu.</span><span class="sxs-lookup"><span data-stu-id="be81e-151">The list contains tables or views to which the selected entity can be mapped.</span></span>
    <span data-ttu-id="be81e-152">Ve výchozím nastavení by měly být namapované odpovídající vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="be81e-152">The appropriate properties should be mapped by default.</span></span>

    ![Mapování](~/ef6/media/mapping.png)

-   <span data-ttu-id="be81e-154">Vyberte přidružení **PersonHireInfo** na návrhové ploše.</span><span class="sxs-lookup"><span data-stu-id="be81e-154">Select the **PersonHireInfo** association on the design surface.</span></span>
-   <span data-ttu-id="be81e-155">Na návrhové ploše klikněte pravým tlačítkem na přidružení a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="be81e-155">Right-click the association on the design surface and select **Properties**.</span></span>
-   <span data-ttu-id="be81e-156">V okně **vlastnosti** vyberte vlastnost **referenčních omezení** a klikněte na tlačítko se třemi tečkami.</span><span class="sxs-lookup"><span data-stu-id="be81e-156">In the **Properties** window, select the **Referential Constraints** property and click the ellipses button.</span></span>
-   <span data-ttu-id="be81e-157">V rozevíracím seznamu **hlavní objekty** vyberte **osoba** .</span><span class="sxs-lookup"><span data-stu-id="be81e-157">Select **Person** from the **Principal** drop-down list.</span></span>
-   <span data-ttu-id="be81e-158">Stiskněte **OK**.</span><span class="sxs-lookup"><span data-stu-id="be81e-158">Press **OK**.</span></span>

 

## <a name="use-the-model"></a><span data-ttu-id="be81e-159">Použití modelu</span><span class="sxs-lookup"><span data-stu-id="be81e-159">Use the Model</span></span>

-   <span data-ttu-id="be81e-160">Do metody Main vložte následující kód.</span><span class="sxs-lookup"><span data-stu-id="be81e-160">Paste the following code in the Main method.</span></span>

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
-   <span data-ttu-id="be81e-161">Zkompilujte a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="be81e-161">Compile and run the application.</span></span>

<span data-ttu-id="be81e-162">V důsledku spuštění této aplikace byly v databázi **School** provedeny následující příkazy jazyka T-SQL.</span><span class="sxs-lookup"><span data-stu-id="be81e-162">The following T-SQL statements were executed against the **School** database as a result of running this application.</span></span> 

-   <span data-ttu-id="be81e-163">Následující **vložení** bylo provedeno jako výsledek spuštěného kontextu. SaveChanges () a kombinuje data z entit **Person** a **HireInfo**</span><span class="sxs-lookup"><span data-stu-id="be81e-163">The following **INSERT** was executed as a result of executing context.SaveChanges() and combines data from the **Person** and **HireInfo** entities</span></span>

    ![Vložit](~/ef6/media/insert.png)

-   <span data-ttu-id="be81e-165">Následující **příkaz SELECT** byl proveden jako výsledek spuštěného kontextu. Lidi. FirstOrDefault () a vybere pouze sloupce namapované na **osobu**</span><span class="sxs-lookup"><span data-stu-id="be81e-165">The following **SELECT** was executed as a result of executing context.People.FirstOrDefault() and selects just the columns mapped to **Person**</span></span>

    ![Vyberte 1](~/ef6/media/select1.png)

-   <span data-ttu-id="be81e-167">Následující **příkaz SELECT** byl proveden jako výsledek přístupu k navigační vlastnosti ExistingPerson. instruktor a vybere pouze sloupce namapované na **HireInfo** .</span><span class="sxs-lookup"><span data-stu-id="be81e-167">The following **SELECT** was executed as a result of accessing the navigation property existingPerson.Instructor and selects just the columns mapped to **HireInfo**</span></span>

    ![Vybrat 2](~/ef6/media/select2.png)
