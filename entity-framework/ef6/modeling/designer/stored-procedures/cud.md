---
title: Návrháře CUD uložených procedur komponentami TableAdapter-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1e773972-2da5-45e0-85a2-3cf3fbcfa5cf
ms.openlocfilehash: 35a00aa817c8643352c517c233977efd49e3baac
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489554"
---
# <a name="designer-cud-stored-procedures"></a><span data-ttu-id="26c8a-102">Návrháře CUD uložené procedury</span><span class="sxs-lookup"><span data-stu-id="26c8a-102">Designer CUD Stored Procedures</span></span>
<span data-ttu-id="26c8a-103">Tento podrobný návod ukazují, jak vytvořit mapování\\vložení, aktualizace a odstranění (vytvoření) operace typu entity na uložené procedury pomocí návrháře Entity Framework (EF designeru).</span><span class="sxs-lookup"><span data-stu-id="26c8a-103">This step-by-step walkthrough show how to map the create\\insert, update, and delete (CUD) operations of an entity type to stored procedures using the Entity Framework Designer (EF Designer).</span></span>  <span data-ttu-id="26c8a-104">Ve výchozím nastavení Entity Framework automaticky generuje příkazů SQL pro operace vytvoření, ale můžete také namapovat uložených procedur na těchto operací.</span><span class="sxs-lookup"><span data-stu-id="26c8a-104">By default, the Entity Framework automatically generates the SQL statements for the CUD operations, but you can also map stored procedures to these operations.</span></span>  

<span data-ttu-id="26c8a-105">Všimněte si, že Code First nepodporuje mapování uložené procedury nebo funkce.</span><span class="sxs-lookup"><span data-stu-id="26c8a-105">Note, that Code First does not support mapping to stored procedures or functions.</span></span> <span data-ttu-id="26c8a-106">Pomocí metody System.Data.Entity.DbSet.SqlQuery však můžete volat uložené procedury nebo funkce.</span><span class="sxs-lookup"><span data-stu-id="26c8a-106">However, you can call stored procedures or functions by using the System.Data.Entity.DbSet.SqlQuery method.</span></span> <span data-ttu-id="26c8a-107">Příklad:</span><span class="sxs-lookup"><span data-stu-id="26c8a-107">For example:</span></span>
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]");
```

## <a name="considerations-when-mapping-the-cud-operations-to-stored-procedures"></a><span data-ttu-id="26c8a-108">Důležité informace o mapování CUD operace na uložené procedury</span><span class="sxs-lookup"><span data-stu-id="26c8a-108">Considerations when Mapping the CUD Operations to Stored Procedures</span></span>

<span data-ttu-id="26c8a-109">Při mapování CUD operace na uložené procedury, platí následující aspekty:</span><span class="sxs-lookup"><span data-stu-id="26c8a-109">When mapping the CUD operations to stored procedures, the following considerations apply:</span></span> 

-   <span data-ttu-id="26c8a-110">Při jedné z operací CUD mapování uložené procedury, namapujte všechny z nich.</span><span class="sxs-lookup"><span data-stu-id="26c8a-110">If you are mapping one of the CUD operations to a stored procedure, map all of them.</span></span> <span data-ttu-id="26c8a-111">Pokud není mapování všech třech umístěních, nenamapované operace se nezdaří, pokud jsou provedeny a **UpdateException** bude vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="26c8a-111">If you do not map all three, the unmapped operations will fail if executed and an **UpdateException** will be thrown.</span></span>
-   <span data-ttu-id="26c8a-112">Každý parametr uložené procedury musí být namapovaný na vlastnosti entity.</span><span class="sxs-lookup"><span data-stu-id="26c8a-112">You must map every parameter of the stored procedure to entity properties.</span></span>
-   <span data-ttu-id="26c8a-113">Pokud server vygeneruje hodnotu primárního klíče pro vložený řádek, je nutné mapovat tato hodnota zpět na vlastnost klíče entity.</span><span class="sxs-lookup"><span data-stu-id="26c8a-113">If the server generates the primary key value for the inserted row, you must map this value back to the entity's key property.</span></span> <span data-ttu-id="26c8a-114">V následujícím příkladu **InsertPerson** uloženou proceduru vrátí nově vytvořený primární klíč v rámci uložené procedury sada výsledků dotazu.</span><span class="sxs-lookup"><span data-stu-id="26c8a-114">In the example that follows, the **InsertPerson** stored procedure returns the newly created primary key as part of the stored procedure's result set.</span></span> <span data-ttu-id="26c8a-115">Primární klíč je namapována na klíč entity (**PersonID**) pomocí **&lt;přidat výsledných vazeb&gt;** funkce EF designeru.</span><span class="sxs-lookup"><span data-stu-id="26c8a-115">The primary key is mapped to the entity key (**PersonID**) using the **&lt;Add Result Bindings&gt;** feature of the EF Designer.</span></span>
-   <span data-ttu-id="26c8a-116">Volání uložené procedury jsou namapované 1:1 s entitami v konceptuálním modelu.</span><span class="sxs-lookup"><span data-stu-id="26c8a-116">The stored procedure calls are mapped 1:1 with the entities in the conceptual model.</span></span> <span data-ttu-id="26c8a-117">Například pokud se rozhodnete implementovat hierarchie dědičnosti v konceptuálním modelu a pak mapování vytvoření uložené procedury pro **nadřazené** (základní) a **podřízené** (odvozené) entity, uložení **Podřízené** změny bude volat pouze **podřízené**uživatele uložených procedur, nebude spustí **nadřazené**uživatele uložené procedury volání.</span><span class="sxs-lookup"><span data-stu-id="26c8a-117">For example, if you implement an inheritance hierarchy in your conceptual model and then map the CUD stored procedures for the **Parent** (base) and the **Child** (derived) entities, saving the **Child** changes will only call the **Child**’s stored procedures, it will not trigger the **Parent**’s stored procedures calls.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="26c8a-118">Požadavky</span><span class="sxs-lookup"><span data-stu-id="26c8a-118">Prerequisites</span></span>

<span data-ttu-id="26c8a-119">K dokončení toho návodu budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="26c8a-119">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="26c8a-120">Nejnovější verzi sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="26c8a-120">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="26c8a-121">[Ukázkové databáze školy](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="26c8a-121">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="26c8a-122">Nastavení projektu</span><span class="sxs-lookup"><span data-stu-id="26c8a-122">Set up the Project</span></span>

-   <span data-ttu-id="26c8a-123">Otevřít Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="26c8a-123">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="26c8a-124">Vyberte **souboru -&gt; nové –&gt; projektu**</span><span class="sxs-lookup"><span data-stu-id="26c8a-124">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="26c8a-125">V levém podokně klikněte na tlačítko **Visual C\#** a pak vyberte **konzoly** šablony.</span><span class="sxs-lookup"><span data-stu-id="26c8a-125">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="26c8a-126">Zadejte **CUDSProcsSample** jako název.</span><span class="sxs-lookup"><span data-stu-id="26c8a-126">Enter **CUDSProcsSample** as the name.</span></span>
-   <span data-ttu-id="26c8a-127">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="26c8a-127">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="26c8a-128">Vytvoření modelu</span><span class="sxs-lookup"><span data-stu-id="26c8a-128">Create a Model</span></span>

-   <span data-ttu-id="26c8a-129">Klikněte pravým tlačítkem na název projektu v Průzkumníku řešení a vyberte **Add -&gt; nová položka**.</span><span class="sxs-lookup"><span data-stu-id="26c8a-129">Right-click the project name in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="26c8a-130">Vyberte **Data** v levé nabídce a pak vyberte **datový Model Entity ADO.NET** v podokně šablon.</span><span class="sxs-lookup"><span data-stu-id="26c8a-130">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="26c8a-131">Zadejte **CUDSProcs.edmx** pro název souboru a pak klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="26c8a-131">Enter **CUDSProcs.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="26c8a-132">V dialogovém okně Výběr obsahu modelu vyberte **Generovat z databáze**a potom klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="26c8a-132">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="26c8a-133">Klikněte na tlačítko **nové připojení**.</span><span class="sxs-lookup"><span data-stu-id="26c8a-133">Click **New Connection**.</span></span> <span data-ttu-id="26c8a-134">V dialogovém okně Vlastnosti připojení zadat název serveru (například **(localdb)\\mssqllocaldb**), vyberte metodu ověřování, zadejte **School** pro název databáze a pak Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="26c8a-134">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="26c8a-135">Dialogové okno Vybrat datové připojení se aktualizuje se nastavení připojení databáze.</span><span class="sxs-lookup"><span data-stu-id="26c8a-135">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="26c8a-136">V okně Zvolte vaše databázové objekty dialogovém okně **tabulky** uzlu, vyberte **osoba** tabulky.</span><span class="sxs-lookup"><span data-stu-id="26c8a-136">In the Choose Your Database Objects dialog box, under the **Tables** node, select the **Person** table.</span></span>
-   <span data-ttu-id="26c8a-137">Také vybrat následující uložené procedury v části **uložené procedury a funkce** uzlu: **DeletePerson**, **InsertPerson**, a **UpdatePerson** .</span><span class="sxs-lookup"><span data-stu-id="26c8a-137">Also, select the following stored procedures under the **Stored Procedures and Functions** node: **DeletePerson**, **InsertPerson**, and **UpdatePerson**.</span></span> 
-   <span data-ttu-id="26c8a-138">Od verze Visual Studio 2012 EF designeru podporuje hromadný import uložených procedur.</span><span class="sxs-lookup"><span data-stu-id="26c8a-138">Starting with Visual Studio 2012 the EF Designer supports bulk import of stored procedures.</span></span> <span data-ttu-id="26c8a-139">**Importovat vybrané uložených procedur a funkcí do entity model** je ve výchozím nastavení zaškrtnuto.</span><span class="sxs-lookup"><span data-stu-id="26c8a-139">The **Import selected stored procedures and functions into the entity model** is checked by default.</span></span> <span data-ttu-id="26c8a-140">Protože v tomto příkladu budeme mít uložené procedury, které vložit, aktualizovat a odstranit typy entit, nechcete, aby importovat jsme se zrušit zaškrtnutí tohoto políčka.</span><span class="sxs-lookup"><span data-stu-id="26c8a-140">Since in this example we have stored procedures that insert, update, and delete entity types, we do not want to import them and will uncheck this checkbox.</span></span> 

    ![Importovat S Procs](~/ef6/media/importsprocs.jpg)

-   <span data-ttu-id="26c8a-142">Klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="26c8a-142">Click **Finish**.</span></span>
    <span data-ttu-id="26c8a-143">EF designeru, která poskytuje návrhové ploše pro úpravy váš model, se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="26c8a-143">The EF Designer, which provides a design surface for editing your model, is displayed.</span></span>

## <a name="map-the-person-entity-to-stored-procedures"></a><span data-ttu-id="26c8a-144">Mapa entita osoba, která má uložené procedury</span><span class="sxs-lookup"><span data-stu-id="26c8a-144">Map the Person Entity to Stored Procedures</span></span>

-   <span data-ttu-id="26c8a-145">Klikněte pravým tlačítkem myši **osoba** typu entity a vyberte **mapování uložené procedury**.</span><span class="sxs-lookup"><span data-stu-id="26c8a-145">Right-click the **Person** entity type and select **Stored Procedure Mapping**.</span></span>
-   <span data-ttu-id="26c8a-146">Mapování uložené procedury joinkind **podrobnosti mapování** okna.</span><span class="sxs-lookup"><span data-stu-id="26c8a-146">The stored procedure mappings appear in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="26c8a-147">Klikněte na tlačítko  **&lt;vyberte Vložit funkci&gt;**.</span><span class="sxs-lookup"><span data-stu-id="26c8a-147">Click **&lt;Select Insert Function&gt;**.</span></span>
    <span data-ttu-id="26c8a-148">Pole stane rozevíracího seznamu uložených procedur v modelu úložiště, který je možné mapovat na typy entit v konceptuálním modelu.</span><span class="sxs-lookup"><span data-stu-id="26c8a-148">The field becomes a drop-down list of the stored procedures in the storage model that can be mapped to entity types in the conceptual model.</span></span>
    <span data-ttu-id="26c8a-149">Vyberte **InsertPerson** z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="26c8a-149">Select **InsertPerson** from the drop-down list.</span></span>
-   <span data-ttu-id="26c8a-150">Zobrazí se výchozí mapování mezi parametry uložené procedury a vlastností entity.</span><span class="sxs-lookup"><span data-stu-id="26c8a-150">Default mappings between stored procedure parameters and entity properties appear.</span></span> <span data-ttu-id="26c8a-151">Všimněte si, že šipky označují směr mapování: vlastnost hodnot pro parametry uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="26c8a-151">Note that arrows indicate the mapping direction: Property values are supplied to stored procedure parameters.</span></span>
-   <span data-ttu-id="26c8a-152">Klikněte na tlačítko  **&lt;přidat vazbu výsledek&gt;**.</span><span class="sxs-lookup"><span data-stu-id="26c8a-152">Click **&lt;Add Result Binding&gt;**.</span></span>
-   <span data-ttu-id="26c8a-153">Typ **NewPersonID**, název parametru vrácené **InsertPerson** uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="26c8a-153">Type **NewPersonID**, the name of the parameter returned by the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="26c8a-154">Zajistěte, aby zadejte počáteční ani koncové mezery.</span><span class="sxs-lookup"><span data-stu-id="26c8a-154">Make sure not to type leading or trailing spaces.</span></span>
-   <span data-ttu-id="26c8a-155">Stisknutím klávesy **zadejte**.</span><span class="sxs-lookup"><span data-stu-id="26c8a-155">Press **Enter**.</span></span>
-   <span data-ttu-id="26c8a-156">Ve výchozím nastavení **NewPersonID** je namapována na klíč entity **PersonID**.</span><span class="sxs-lookup"><span data-stu-id="26c8a-156">By default, **NewPersonID** is mapped to the entity key **PersonID**.</span></span> <span data-ttu-id="26c8a-157">Všimněte si, že šipka označuje směr mapování: pro vlastnost není zadána hodnota ve sloupci výsledků.</span><span class="sxs-lookup"><span data-stu-id="26c8a-157">Note that an arrow indicates the direction of the mapping: The value of the result column is supplied to the property.</span></span>

    ![Podrobnosti mapování](~/ef6/media/mappingdetails.png)

-   <span data-ttu-id="26c8a-159">Klikněte na tlačítko **&lt;vyberte funkce aktualizace&gt;** a vyberte **UpdatePerson** v rozevíracím seznamu rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="26c8a-159">Click **&lt;Select Update Function&gt;** and select **UpdatePerson** from the resulting drop-down list.</span></span>
-   <span data-ttu-id="26c8a-160">Zobrazí se výchozí mapování mezi parametry uložené procedury a vlastností entity.</span><span class="sxs-lookup"><span data-stu-id="26c8a-160">Default mappings between stored procedure parameters and entity properties appear.</span></span>
-   <span data-ttu-id="26c8a-161">Klikněte na tlačítko **&lt;vyberte funkce Odstranit&gt;** a vyberte **DeletePerson** v rozevíracím seznamu rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="26c8a-161">Click **&lt;Select Delete Function&gt;** and select **DeletePerson** from the resulting drop-down list.</span></span>
-   <span data-ttu-id="26c8a-162">Zobrazí se výchozí mapování mezi parametry uložené procedury a vlastností entity.</span><span class="sxs-lookup"><span data-stu-id="26c8a-162">Default mappings between stored procedure parameters and entity properties appear.</span></span>

<span data-ttu-id="26c8a-163">Vložení, aktualizace a odstranění operace **osoba** typ entity jsou teď namapovaných na uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="26c8a-163">The insert, update, and delete operations of the **Person** entity type are now mapped to stored procedures.</span></span>

<span data-ttu-id="26c8a-164">Pokud chcete povolit souběžnost kontroly při aktualizaci nebo odstranění entity s uloženými procedurami, použijte jednu z následujících možností:</span><span class="sxs-lookup"><span data-stu-id="26c8a-164">If you want to enable concurrency checking when updating or deleting an entity with stored procedures, use one of the following options:</span></span>

-   <span data-ttu-id="26c8a-165">Použití **výstup** parametr a vrátit tak počet ovlivněné řádky z uložené procedury a kontrolu **parametr ovlivněných řádků** zaškrtávací políčko vedle názvu parametru.</span><span class="sxs-lookup"><span data-stu-id="26c8a-165">Use an **OUTPUT** parameter to return the number of affected rows from the stored procedure and check the **Rows Affected Parameter** checkbox next to the parameter name.</span></span> <span data-ttu-id="26c8a-166">Pokud vrácená hodnota je nula, při volání operace [ **OptimisticConcurrencyException** ](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) bude vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="26c8a-166">If the value returned is zero when the operation is called, an  [**OptimisticConcurrencyException**](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) will be thrown.</span></span>
-   <span data-ttu-id="26c8a-167">Zkontrolujte **použít původní hodnota** zaškrtávací políčko vedle vlastnosti, která chcete použít pro kontrolu souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="26c8a-167">Check the **Use Original Value** checkbox next to a property that you want to use for concurrency checking.</span></span> <span data-ttu-id="26c8a-168">Při pokusu o aktualizaci, použije se hodnota vlastnosti, která byla původně načtena z databáze při zápis dat zpět do databáze.</span><span class="sxs-lookup"><span data-stu-id="26c8a-168">When an update is attempted, the value of the property that was originally read from the database will be used when writing data back to the database.</span></span> <span data-ttu-id="26c8a-169">Pokud hodnota neodpovídá hodnotě v databázi, **OptimisticConcurrencyException** bude vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="26c8a-169">If the value does not match the value in the database, an **OptimisticConcurrencyException** will be thrown.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="26c8a-170">Použití modelu</span><span class="sxs-lookup"><span data-stu-id="26c8a-170">Use the Model</span></span>

<span data-ttu-id="26c8a-171">Otevřít **Program.cs** souboru, kde **hlavní** je definována metoda.</span><span class="sxs-lookup"><span data-stu-id="26c8a-171">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="26c8a-172">Přidejte následující kód do funkce Main.</span><span class="sxs-lookup"><span data-stu-id="26c8a-172">Add the following code into the Main function.</span></span>

<span data-ttu-id="26c8a-173">Kód vytvoří novou **osoba** pak aktualizuje objekt objektu a nakonec odstraní objekt.</span><span class="sxs-lookup"><span data-stu-id="26c8a-173">The code creates a new **Person** object, then updates the object, and finally deletes the object.</span></span>         

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

-   <span data-ttu-id="26c8a-174">Kompilace a spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="26c8a-174">Compile and run the application.</span></span> <span data-ttu-id="26c8a-175">Program vygeneruje následující výstup \*</span><span class="sxs-lookup"><span data-stu-id="26c8a-175">The program produces the following output \*</span></span>
    >[!NOTE]
> <span data-ttu-id="26c8a-176">PersonID je automaticky vytvořen serveru, pravděpodobně se zobrazí jiné číslo \*</span><span class="sxs-lookup"><span data-stu-id="26c8a-176">PersonID is auto-generated by the server, so you will most likely see a different number\*</span></span>

```
Added Robyn Martin to the context.
Before SaveChanges, the PersonID is: 0
After SaveChanges, the PersonID is: 51
A person with PersonID 51 was deleted.
```

<span data-ttu-id="26c8a-177">Při práci s verzí sady Visual Studio Ultimate, vám pomůže nástroj Intellitrace v ladicím programu naleznete v tématu příkazy SQL, které se provádějí.</span><span class="sxs-lookup"><span data-stu-id="26c8a-177">If you are working with the Ultimate version of Visual Studio, you can use Intellitrace with the debugger to see the SQL statements that get executed.</span></span>

![Intellitrace](~/ef6/media/intellitrace.png)
