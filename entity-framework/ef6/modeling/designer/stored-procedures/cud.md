---
title: CUD uložené procedury návrháře – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1e773972-2da5-45e0-85a2-3cf3fbcfa5cf
ms.openlocfilehash: bdb0df969c33d5ad3f103bfa9af6002c9c2bb9b3
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813551"
---
# <a name="designer-cud-stored-procedures"></a><span data-ttu-id="7077e-102">CUD uložené procedury návrháře</span><span class="sxs-lookup"><span data-stu-id="7077e-102">Designer CUD Stored Procedures</span></span>

<span data-ttu-id="7077e-103">V tomto podrobném návodu se dozvíte, jak namapovat operace CREATE @ no__t-0insert, Update a Delete (CUD) typu entity na uložené procedury pomocí Entity Framework Designer (EF Designer).</span><span class="sxs-lookup"><span data-stu-id="7077e-103">This step-by-step walkthrough show how to map the create\\insert, update, and delete (CUD) operations of an entity type to stored procedures using the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="7077e-104"> Ve výchozím nastavení Entity Framework automaticky generuje příkazy SQL pro operace CUD, ale můžete také namapovat uložené procedury na tyto operace.</span><span class="sxs-lookup"><span data-stu-id="7077e-104"> By default, the Entity Framework automatically generates the SQL statements for the CUD operations, but you can also map stored procedures to these operations.</span></span>  

<span data-ttu-id="7077e-105">Všimněte si, že Code First nepodporuje mapování na uložené procedury nebo funkce.</span><span class="sxs-lookup"><span data-stu-id="7077e-105">Note, that Code First does not support mapping to stored procedures or functions.</span></span> <span data-ttu-id="7077e-106">Uložené procedury nebo funkce však můžete volat pomocí metody System. data. entity. Negenerickými. SqlQuery.</span><span class="sxs-lookup"><span data-stu-id="7077e-106">However, you can call stored procedures or functions by using the System.Data.Entity.DbSet.SqlQuery method.</span></span> <span data-ttu-id="7077e-107">Příklad:</span><span class="sxs-lookup"><span data-stu-id="7077e-107">For example:</span></span>

``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]");
```

## <a name="considerations-when-mapping-the-cud-operations-to-stored-procedures"></a><span data-ttu-id="7077e-108">Předpoklady při mapování operací CUD na uložené procedury</span><span class="sxs-lookup"><span data-stu-id="7077e-108">Considerations when Mapping the CUD Operations to Stored Procedures</span></span>

<span data-ttu-id="7077e-109">Při mapování operací CUD na uložené procedury platí následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="7077e-109">When mapping the CUD operations to stored procedures, the following considerations apply:</span></span>

- <span data-ttu-id="7077e-110">Pokud mapujete jednu z operací CUD na uloženou proceduru, namapujte na ni všechny.</span><span class="sxs-lookup"><span data-stu-id="7077e-110">If you are mapping one of the CUD operations to a stored procedure, map all of them.</span></span> <span data-ttu-id="7077e-111">Pokud nemapujete všechny tři, nemapované operace selžou, pokud se spustí, a vyvolá **UpdateException** will.</span><span class="sxs-lookup"><span data-stu-id="7077e-111">If you do not map all three, the unmapped operations will fail if executed and an **UpdateException** will be thrown.</span></span>
- <span data-ttu-id="7077e-112">Všechny parametry uložené procedury je nutné namapovat na vlastnosti entity.</span><span class="sxs-lookup"><span data-stu-id="7077e-112">You must map every parameter of the stored procedure to entity properties.</span></span>
- <span data-ttu-id="7077e-113">Pokud server vygeneruje hodnotu primárního klíče pro vložený řádek, musíte tuto hodnotu namapovat zpátky na klíčovou vlastnost entity.</span><span class="sxs-lookup"><span data-stu-id="7077e-113">If the server generates the primary key value for the inserted row, you must map this value back to the entity's key property.</span></span> <span data-ttu-id="7077e-114">V následujícím příkladu vrátí procedura **InsertPerson** stored nově vytvořený primární klíč jako součást sady výsledků uložených procedur.</span><span class="sxs-lookup"><span data-stu-id="7077e-114">In the example that follows, the **InsertPerson** stored procedure returns the newly created primary key as part of the stored procedure's result set.</span></span> <span data-ttu-id="7077e-115">Primární klíč je namapován na klíč entity (**PersonID**) pomocí **vazeb výsledku &lt;Add @ no__t-3** feature v Návrháři EF.</span><span class="sxs-lookup"><span data-stu-id="7077e-115">The primary key is mapped to the entity key (**PersonID**) using the **&lt;Add Result Bindings&gt;** feature of the EF Designer.</span></span>
- <span data-ttu-id="7077e-116">Volání uložených procedur jsou namapována 1:1 s entitami v koncepčním modelu.</span><span class="sxs-lookup"><span data-stu-id="7077e-116">The stored procedure calls are mapped 1:1 with the entities in the conceptual model.</span></span> <span data-ttu-id="7077e-117">Například Pokud implementujete hierarchii dědičnosti v koncepčním modelu a poté namapujete uložené procedury CUD pro **nadřazený objekt** (základní) a **podřízené** (odvozené) entity, uložení **podřízených** změn bude volat pouze **podřízený**element. s uložené procedury nebudou aktivovat volání uložených procedur **nadřazené**procedury.</span><span class="sxs-lookup"><span data-stu-id="7077e-117">For example, if you implement an inheritance hierarchy in your conceptual model and then map the CUD stored procedures for the **Parent** (base) and the **Child** (derived) entities, saving the **Child** changes will only call the **Child**’s stored procedures, it will not trigger the **Parent**’s stored procedures calls.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7077e-118">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7077e-118">Prerequisites</span></span>

<span data-ttu-id="7077e-119">K dokončení toho návodu budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="7077e-119">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="7077e-120">Poslední verze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7077e-120">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="7077e-121">[Ukázková databáze školy](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="7077e-121">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="7077e-122">Nastavení projektu</span><span class="sxs-lookup"><span data-stu-id="7077e-122">Set up the Project</span></span>

- <span data-ttu-id="7077e-123">Otevřete Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="7077e-123">Open Visual Studio 2012.</span></span>
- <span data-ttu-id="7077e-124">Vybrat **soubor-&gt; nový-&gt; projekt**</span><span class="sxs-lookup"><span data-stu-id="7077e-124">Select **File-&gt; New -&gt; Project**</span></span>
- <span data-ttu-id="7077e-125">V levém podokně klikněte na položku **Visual C @ no__t-1**a potom vyberte šablonu **konzoly** .</span><span class="sxs-lookup"><span data-stu-id="7077e-125">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
- <span data-ttu-id="7077e-126">Do pole název zadejte **CUDSProcsSample** AS.</span><span class="sxs-lookup"><span data-stu-id="7077e-126">Enter **CUDSProcsSample** as the name.</span></span>
- <span data-ttu-id="7077e-127">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="7077e-127">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="7077e-128">Vytvoření modelu</span><span class="sxs-lookup"><span data-stu-id="7077e-128">Create a Model</span></span>

- <span data-ttu-id="7077e-129">Klikněte pravým tlačítkem myši na název projektu v Průzkumník řešení a vyberte možnost **Přidat-&gt; nová položka**.</span><span class="sxs-lookup"><span data-stu-id="7077e-129">Right-click the project name in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
- <span data-ttu-id="7077e-130">V nabídce vlevo vyberte **data** a v podokně šablony vyberte **ADO.NET model EDM (Entity Data Model)** .</span><span class="sxs-lookup"><span data-stu-id="7077e-130">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
- <span data-ttu-id="7077e-131">Jako název souboru zadejte **CUDSProcs. edmx** a pak klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="7077e-131">Enter **CUDSProcs.edmx** for the file name, and then click **Add**.</span></span>
- <span data-ttu-id="7077e-132">V dialogovém okně Vybrat obsah modelu vyberte možnost **Generovat z databáze**a poté klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="7077e-132">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
- <span data-ttu-id="7077e-133">Klikněte na **nové připojení**.</span><span class="sxs-lookup"><span data-stu-id="7077e-133">Click **New Connection**.</span></span> <span data-ttu-id="7077e-134">V dialogovém okně Vlastnosti připojení zadejte název serveru (například **(LocalDB)\\mssqllocaldb**), vyberte metodu ověřování, jako název databáze zadejte **School** a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="7077e-134">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="7077e-135">Dialogové okno zvolit datové připojení je aktualizováno nastavením připojení k databázi.</span><span class="sxs-lookup"><span data-stu-id="7077e-135">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
- <span data-ttu-id="7077e-136">V dialogovém okně zvolte objekty databáze pod **tabulkami** node vyberte tabulku **osoba** .</span><span class="sxs-lookup"><span data-stu-id="7077e-136">In the Choose Your Database Objects dialog box, under the **Tables** node, select the **Person** table.</span></span>
- <span data-ttu-id="7077e-137">V uzlu **uložené procedury a funkce** vyberte také následující uložené procedury: **DeletePerson**, **InsertPerson**a **UpdatePerson**.</span><span class="sxs-lookup"><span data-stu-id="7077e-137">Also, select the following stored procedures under the **Stored Procedures and Functions** node: **DeletePerson**, **InsertPerson**, and **UpdatePerson**.</span></span>
- <span data-ttu-id="7077e-138">Počínaje sadou Visual Studio 2012 Návrhář EF podporuje hromadný import uložených procedur.</span><span class="sxs-lookup"><span data-stu-id="7077e-138">Starting with Visual Studio 2012 the EF Designer supports bulk import of stored procedures.</span></span> <span data-ttu-id="7077e-139">Ve výchozím nastavení je zaškrtnuto políčko **Importovat vybrané uložené procedury a funkce do modelu entity** .</span><span class="sxs-lookup"><span data-stu-id="7077e-139">The **Import selected stored procedures and functions into the entity model** is checked by default.</span></span> <span data-ttu-id="7077e-140">Vzhledem k tomu, že v tomto příkladu máme uložené procedury, které vkládají, aktualizují a odstraňují typy entit, nechcete je naimportovat a zruší zaškrtnutí tohoto políčka.</span><span class="sxs-lookup"><span data-stu-id="7077e-140">Since in this example we have stored procedures that insert, update, and delete entity types, we do not want to import them and will uncheck this checkbox.</span></span>

    ![Importovat S – procs](~/ef6/media/importsprocs.jpg)

- <span data-ttu-id="7077e-142">Klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="7077e-142">Click **Finish**.</span></span>
    <span data-ttu-id="7077e-143">Zobrazí se Návrhář EF, který poskytuje návrhovou plochu pro úpravu vašeho modelu.</span><span class="sxs-lookup"><span data-stu-id="7077e-143">The EF Designer, which provides a design surface for editing your model, is displayed.</span></span>

## <a name="map-the-person-entity-to-stored-procedures"></a><span data-ttu-id="7077e-144">Mapování entity Person na uložené procedury</span><span class="sxs-lookup"><span data-stu-id="7077e-144">Map the Person Entity to Stored Procedures</span></span>

- <span data-ttu-id="7077e-145">Klikněte pravým tlačítkem na typ **osoba**@no__t – 1entity a vyberte **mapování uložených procedur**.</span><span class="sxs-lookup"><span data-stu-id="7077e-145">Right-click the **Person** entity type and select **Stored Procedure Mapping**.</span></span>
- <span data-ttu-id="7077e-146">Mapování uložených procedur se zobrazí v **podrobnostech mapování** window.</span><span class="sxs-lookup"><span data-stu-id="7077e-146">The stored procedure mappings appear in the **Mapping Details** window.</span></span>
- <span data-ttu-id="7077e-147">Klikněte na **@no__t – 1Select Vložit funkci @ no__t-2**.</span><span class="sxs-lookup"><span data-stu-id="7077e-147">Click **&lt;Select Insert Function&gt;**.</span></span>
    <span data-ttu-id="7077e-148">Pole se zobrazí v rozevíracím seznamu uložených procedur v modelu úložiště, které lze namapovat na typy entit v koncepčním modelu.</span><span class="sxs-lookup"><span data-stu-id="7077e-148">The field becomes a drop-down list of the stored procedures in the storage model that can be mapped to entity types in the conceptual model.</span></span>
    <span data-ttu-id="7077e-149">V rozevíracím seznamu vyberte **InsertPerson** from.</span><span class="sxs-lookup"><span data-stu-id="7077e-149">Select **InsertPerson** from the drop-down list.</span></span>
- <span data-ttu-id="7077e-150">Zobrazí se výchozí mapování mezi parametry uložené procedury a vlastnostmi entity.</span><span class="sxs-lookup"><span data-stu-id="7077e-150">Default mappings between stored procedure parameters and entity properties appear.</span></span> <span data-ttu-id="7077e-151">Všimněte si, že šipky označují směr mapování: Hodnoty vlastností jsou dodány parametrům uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="7077e-151">Note that arrows indicate the mapping direction: Property values are supplied to stored procedure parameters.</span></span>
- <span data-ttu-id="7077e-152">Klikněte na **@no__t – vazba výsledku 1Add @ no__t-2**.</span><span class="sxs-lookup"><span data-stu-id="7077e-152">Click **&lt;Add Result Binding&gt;**.</span></span>
- <span data-ttu-id="7077e-153">Zadejte **NewPersonID**, název parametru vráceného procedurou **InsertPerson** stored.</span><span class="sxs-lookup"><span data-stu-id="7077e-153">Type **NewPersonID**, the name of the parameter returned by the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="7077e-154">Nezapomeňte zadat počáteční ani koncové mezery.</span><span class="sxs-lookup"><span data-stu-id="7077e-154">Make sure not to type leading or trailing spaces.</span></span>
- <span data-ttu-id="7077e-155">Stiskněte klávesu **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="7077e-155">Press **Enter**.</span></span>
- <span data-ttu-id="7077e-156">Ve výchozím nastavení je **NewPersonID** Is mapována na klíč entity **PersonID**.</span><span class="sxs-lookup"><span data-stu-id="7077e-156">By default, **NewPersonID** is mapped to the entity key **PersonID**.</span></span> <span data-ttu-id="7077e-157">Všimněte si, že šipka indikuje směr mapování: Do vlastnosti je zadána hodnota sloupce výsledek.</span><span class="sxs-lookup"><span data-stu-id="7077e-157">Note that an arrow indicates the direction of the mapping: The value of the result column is supplied to the property.</span></span>

    ![Mapování podrobností](~/ef6/media/mappingdetails.png)

- <span data-ttu-id="7077e-159">Klikněte na **@no__t – 1Select Update – funkce @ no__t-2** And vyberte **UpdatePerson** from výsledného rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="7077e-159">Click **&lt;Select Update Function&gt;** and select **UpdatePerson** from the resulting drop-down list.</span></span>
- <span data-ttu-id="7077e-160">Zobrazí se výchozí mapování mezi parametry uložené procedury a vlastnostmi entity.</span><span class="sxs-lookup"><span data-stu-id="7077e-160">Default mappings between stored procedure parameters and entity properties appear.</span></span>
- <span data-ttu-id="7077e-161">Klikněte na **&lt;Select odstranit funkci @ no__t-2** And vyberte **DeletePerson** from výsledného rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="7077e-161">Click **&lt;Select Delete Function&gt;** and select **DeletePerson** from the resulting drop-down list.</span></span>
- <span data-ttu-id="7077e-162">Zobrazí se výchozí mapování mezi parametry uložené procedury a vlastnostmi entity.</span><span class="sxs-lookup"><span data-stu-id="7077e-162">Default mappings between stored procedure parameters and entity properties appear.</span></span>

<span data-ttu-id="7077e-163">Operace vložení, aktualizace a odstranění **osoby**typu  entity jsou nyní namapovány na uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="7077e-163">The insert, update, and delete operations of the **Person** entity type are now mapped to stored procedures.</span></span>

<span data-ttu-id="7077e-164">Pokud chcete povolit kontrolu souběžnosti při aktualizaci nebo odstranění entity s uloženými procedurami, použijte jednu z následujících možností:</span><span class="sxs-lookup"><span data-stu-id="7077e-164">If you want to enable concurrency checking when updating or deleting an entity with stored procedures, use one of the following options:</span></span>

- <span data-ttu-id="7077e-165">Použijte **výstupní** parametr pro vrácení počtu ovlivněných řádků z uložené procedury a zaškrtnutí **parametru ovlivněné řádky** checkbox vedle názvu parametru.</span><span class="sxs-lookup"><span data-stu-id="7077e-165">Use an **OUTPUT** parameter to return the number of affected rows from the stored procedure and check the **Rows Affected Parameter** checkbox next to the parameter name.</span></span> <span data-ttu-id="7077e-166">Pokud je vrácená hodnota nula, když je operace volána, vyvolá se  [**OptimisticConcurrencyException**](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) will.</span><span class="sxs-lookup"><span data-stu-id="7077e-166">If the value returned is zero when the operation is called, an  [**OptimisticConcurrencyException**](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) will be thrown.</span></span>
- <span data-ttu-id="7077e-167">Zaškrtněte políčko **použít původní hodnotu** vedle vlastnosti, kterou chcete použít pro kontrolu souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="7077e-167">Check the **Use Original Value** checkbox next to a property that you want to use for concurrency checking.</span></span> <span data-ttu-id="7077e-168">Při pokusu o aktualizaci se při zápisu dat zpět do databáze použije hodnota vlastnosti, která byla původně načtena z databáze.</span><span class="sxs-lookup"><span data-stu-id="7077e-168">When an update is attempted, the value of the property that was originally read from the database will be used when writing data back to the database.</span></span> <span data-ttu-id="7077e-169">Pokud hodnota neodpovídá hodnotě v databázi, vyvolá se **OptimisticConcurrencyException** will.</span><span class="sxs-lookup"><span data-stu-id="7077e-169">If the value does not match the value in the database, an **OptimisticConcurrencyException** will be thrown.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="7077e-170">Použití modelu</span><span class="sxs-lookup"><span data-stu-id="7077e-170">Use the Model</span></span>

<span data-ttu-id="7077e-171">Otevřete soubor **program.cs** , kde je definována metoda **Main** .</span><span class="sxs-lookup"><span data-stu-id="7077e-171">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="7077e-172">Do funkce Main přidejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="7077e-172">Add the following code into the Main function.</span></span>

<span data-ttu-id="7077e-173">Kód vytvoří nový objekt **Person** a pak objekt aktualizuje a nakonec objekt odstraní.</span><span class="sxs-lookup"><span data-stu-id="7077e-173">The code creates a new **Person** object, then updates the object, and finally deletes the object.</span></span>

``` csharp
    using (var context = new SchoolEntities())
    {
        var newInstructor = new Person
        {
            FirstName = "Robyn",
            LastName = "Martin",
            HireDate = DateTime.Now,
            Discriminator = "Instructor"
        }

        // Add the new object to the context.
        context.People.Add(newInstructor);

        Console.WriteLine("Added {0} {1} to the context.",
            newInstructor.FirstName, newInstructor.LastName);

        Console.WriteLine("Before SaveChanges, the PersonID is: {0}",
            newInstructor.PersonID);

        // SaveChanges will call the InsertPerson sproc.  
        // The PersonID property will be assigned the value
        // returned by the sproc.
        context.SaveChanges();

        Console.WriteLine("After SaveChanges, the PersonID is: {0}",
            newInstructor.PersonID);

        // Modify the object and call SaveChanges.
        // This time, the UpdatePerson will be called.
        newInstructor.FirstName = "Rachel";
        context.SaveChanges();

        // Remove the object from the context and call SaveChanges.
        // The DeletePerson sproc will be called.
        context.People.Remove(newInstructor);
        context.SaveChanges();

        Person deletedInstructor = context.People.
            Where(p => p.PersonID == newInstructor.PersonID).
            FirstOrDefault();

        if (deletedInstructor == null)
            Console.WriteLine("A person with PersonID {0} was deleted.",
                newInstructor.PersonID);
    }
```

- <span data-ttu-id="7077e-174">Zkompilujte a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7077e-174">Compile and run the application.</span></span> <span data-ttu-id="7077e-175">Program vytvoří následující výstup \*</span><span class="sxs-lookup"><span data-stu-id="7077e-175">The program produces the following output \*</span></span>

> [!NOTE]
> <span data-ttu-id="7077e-176">PersonID se automaticky generuje serverem, takže se pravděpodobně zobrazí jiné číslo \*</span><span class="sxs-lookup"><span data-stu-id="7077e-176">PersonID is auto-generated by the server, so you will most likely see a different number\*</span></span>

``` Output
Added Robyn Martin to the context.
Before SaveChanges, the PersonID is: 0
After SaveChanges, the PersonID is: 51
A person with PersonID 51 was deleted.
```

<span data-ttu-id="7077e-177">Pokud pracujete s konečnou verzí sady Visual Studio, můžete použít IntelliTrace s ladicím programem k zobrazení příkazů SQL, které se spustí.</span><span class="sxs-lookup"><span data-stu-id="7077e-177">If you are working with the Ultimate version of Visual Studio, you can use Intellitrace with the debugger to see the SQL statements that get executed.</span></span>

![IntelliTrace](~/ef6/media/intellitrace.png)
