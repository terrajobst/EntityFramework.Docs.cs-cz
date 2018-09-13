---
title: Rozdělení návrháře entit - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: aa2dd48a-1f0e-49dd-863d-d6b4f5834832
ms.openlocfilehash: ba1895ae491cec909ff88a8784eea82f1876f595
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490843"
---
# <a name="designer-entity-splitting"></a><span data-ttu-id="92b13-102">Rozdělení návrháře entit</span><span class="sxs-lookup"><span data-stu-id="92b13-102">Designer Entity Splitting</span></span>
<span data-ttu-id="92b13-103">Tento návod ukazuje, jak pro mapování typu entity na dvě tabulky tak, že upravíte model se Návrhář Entity Framework (EF designeru).</span><span class="sxs-lookup"><span data-stu-id="92b13-103">This walkthrough shows how to map an entity type to two tables by modifying a model with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="92b13-104">Entitu můžete namapovat na několik tabulek při tabulky sdílet společný klíč.</span><span class="sxs-lookup"><span data-stu-id="92b13-104">You can map an entity to multiple tables when the tables share a common key.</span></span> <span data-ttu-id="92b13-105">Koncepty, které platí pro mapování typu entity na dvě tabulky jsou snadno rozšířit mapování typu entity k více než dvě tabulky.</span><span class="sxs-lookup"><span data-stu-id="92b13-105">The concepts that apply to mapping an entity type to two tables are easily extended to mapping an entity type to more than two tables.</span></span>

<span data-ttu-id="92b13-106">Následující obrázek znázorňuje hlavní windows, které se používají při práci s EF designeru.</span><span class="sxs-lookup"><span data-stu-id="92b13-106">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EF designeru](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a><span data-ttu-id="92b13-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="92b13-108">Prerequisites</span></span>

<span data-ttu-id="92b13-109">Visual Studio 2012 nebo Visual Studio 2010, Ultimate, Premium, Professional nebo Web Express edition.</span><span class="sxs-lookup"><span data-stu-id="92b13-109">Visual Studio 2012 or Visual Studio 2010, Ultimate, Premium, Professional, or Web Express edition.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="92b13-110">Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="92b13-110">Create the Database</span></span>

<span data-ttu-id="92b13-111">Databázový server, který se instaluje se sadou Visual Studio se liší v závislosti na verzi sady Visual Studio, které jste nainstalovali:</span><span class="sxs-lookup"><span data-stu-id="92b13-111">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="92b13-112">Pokud používáte sadu Visual Studio 2012 pak vytvoříte databázi LocalDB.</span><span class="sxs-lookup"><span data-stu-id="92b13-112">If you are using Visual Studio 2012 then you'll be creating a LocalDB database.</span></span>
-   <span data-ttu-id="92b13-113">Pokud používáte Visual Studio 2010 vytvoříte databázi SQL Express.</span><span class="sxs-lookup"><span data-stu-id="92b13-113">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>

<span data-ttu-id="92b13-114">Nejprve vytvoříme databázi pomocí dvou tabulek, které budeme zkombinovat do jedné entity.</span><span class="sxs-lookup"><span data-stu-id="92b13-114">First we'll create a database with two tables that we are going to combine into a single entity.</span></span>

-   <span data-ttu-id="92b13-115">Otevřít Visual Studio</span><span class="sxs-lookup"><span data-stu-id="92b13-115">Open Visual Studio</span></span>
-   <span data-ttu-id="92b13-116">**Zobrazení –&gt; Průzkumníka serveru**</span><span class="sxs-lookup"><span data-stu-id="92b13-116">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="92b13-117">Klikněte pravým tlačítkem na **datová připojení -&gt; přidat připojení...**</span><span class="sxs-lookup"><span data-stu-id="92b13-117">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="92b13-118">Pokud jste ještě nepřipojili k databázi z Průzkumníka serveru předtím, než bude nutné vybrat **Microsoft SQL Server** jako zdroj dat</span><span class="sxs-lookup"><span data-stu-id="92b13-118">If you haven’t connected to a database from Server Explorer before you’ll need to select **Microsoft SQL Server** as the data source</span></span>
-   <span data-ttu-id="92b13-119">Připojte se k LocalDB nebo SQL Express, v závislosti na tom, co jste nainstalovali</span><span class="sxs-lookup"><span data-stu-id="92b13-119">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>
-   <span data-ttu-id="92b13-120">Zadejte **EntitySplitting** jako název databáze</span><span class="sxs-lookup"><span data-stu-id="92b13-120">Enter **EntitySplitting** as the database name</span></span>
-   <span data-ttu-id="92b13-121">Vyberte **OK** a zobrazí se výzva, pokud chcete vytvořit novou databázi, vyberte **Ano**</span><span class="sxs-lookup"><span data-stu-id="92b13-121">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="92b13-122">Nová databáze se teď budou zobrazovat v Průzkumníku serveru</span><span class="sxs-lookup"><span data-stu-id="92b13-122">The new database will now appear in Server Explorer</span></span>
-   <span data-ttu-id="92b13-123">Pokud používáte sadu Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="92b13-123">If you are using Visual Studio 2012</span></span>
    -   <span data-ttu-id="92b13-124">Klikněte pravým tlačítkem na databázi v Průzkumníku serveru a vyberte **nový dotaz**</span><span class="sxs-lookup"><span data-stu-id="92b13-124">Right-click on the database in Server Explorer and select **New Query**</span></span>
    -   <span data-ttu-id="92b13-125">Zkopírujte následující příkaz SQL na nový dotaz a pak klikněte pravým tlačítkem myši na dotazu a vyberte **spouštění**</span><span class="sxs-lookup"><span data-stu-id="92b13-125">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>
-   <span data-ttu-id="92b13-126">Pokud používáte Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="92b13-126">If you are using Visual Studio 2010</span></span>
    -   <span data-ttu-id="92b13-127">Vyberte **dat –&gt; jazyka Transact SQL Editor –&gt; nové připojení dotazu...**</span><span class="sxs-lookup"><span data-stu-id="92b13-127">Select **Data -&gt; Transact SQL Editor -&gt; New Query Connection...**</span></span>
    -   <span data-ttu-id="92b13-128">Zadejte **.\\ SQLEXPRESS** jako název serveru a klikněte na **OK**</span><span class="sxs-lookup"><span data-stu-id="92b13-128">Enter **.\\SQLEXPRESS** as the server name and click **OK**</span></span>
    -   <span data-ttu-id="92b13-129">Vyberte **EntitySplitting** databázi z rozevíracího seznamu v horní části editoru dotazů</span><span class="sxs-lookup"><span data-stu-id="92b13-129">Select the **EntitySplitting** database from the drop down at the top of the query editor</span></span>
    -   <span data-ttu-id="92b13-130">Zkopírujte následující příkaz SQL na nový dotaz a pak klikněte pravým tlačítkem myši na dotazu a vyberte **provést SQL**</span><span class="sxs-lookup"><span data-stu-id="92b13-130">Copy the following SQL into the new query, then right-click on the query and select **Execute SQL**</span></span>

``` SQL
CREATE TABLE [dbo].[Person] (
[PersonId] INT IDENTITY (1, 1) NOT NULL,
[FirstName] NVARCHAR (200) NULL,
[LastName] NVARCHAR (200) NULL,
CONSTRAINT [PK_Person] PRIMARY KEY CLUSTERED ([PersonId] ASC)
);

CREATE TABLE [dbo].[PersonInfo] (
[PersonId] INT NOT NULL,
[Email] NVARCHAR (200) NULL,
[Phone] NVARCHAR (50) NULL,
CONSTRAINT [PK_PersonInfo] PRIMARY KEY CLUSTERED ([PersonId] ASC),
CONSTRAINT [FK_Person_PersonInfo] FOREIGN KEY ([PersonId]) REFERENCES [dbo].[Person] ([PersonId]) ON DELETE CASCADE
);
```

## <a name="create-the-project"></a><span data-ttu-id="92b13-131">Vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="92b13-131">Create the Project</span></span>

-   <span data-ttu-id="92b13-132">Na **souboru** nabídky, přejděte k **nový**a potom klikněte na tlačítko **projektu**.</span><span class="sxs-lookup"><span data-stu-id="92b13-132">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="92b13-133">V levém podokně klikněte na tlačítko **Visual C\#** a pak vyberte **konzolovou aplikaci** šablony.</span><span class="sxs-lookup"><span data-stu-id="92b13-133">In the left pane, click **Visual C\#**, and then select the **Console Application** template.</span></span>
-   <span data-ttu-id="92b13-134">Zadejte **MapEntityToTablesSample** jako název projektu a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="92b13-134">Enter **MapEntityToTablesSample** as the name of the project and click **OK**.</span></span>
-   <span data-ttu-id="92b13-135">Klikněte na tlačítko **ne** Pokud se zobrazí výzva k uložení dotazu SQL vytvořené v první části.</span><span class="sxs-lookup"><span data-stu-id="92b13-135">Click **No** if prompted to save the SQL query created in the first section.</span></span>

## <a name="create-a-model-based-on-the-database"></a><span data-ttu-id="92b13-136">Vytvořit Model založený na databázi</span><span class="sxs-lookup"><span data-stu-id="92b13-136">Create a Model based on the Database</span></span>

-   <span data-ttu-id="92b13-137">Klikněte pravým tlačítkem na název projektu v Průzkumníku řešení, přejděte na **přidat**a potom klikněte na tlačítko **nová položka**.</span><span class="sxs-lookup"><span data-stu-id="92b13-137">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="92b13-138">Vyberte **Data** v levé nabídce a pak vyberte **datový Model Entity ADO.NET** v podokně šablon.</span><span class="sxs-lookup"><span data-stu-id="92b13-138">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="92b13-139">Zadejte **MapEntityToTablesModel.edmx** pro název souboru a pak klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="92b13-139">Enter **MapEntityToTablesModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="92b13-140">V dialogovém okně Výběr obsahu modelu vyberte **Generovat z databáze**a potom klikněte na tlačítko **Další.**</span><span class="sxs-lookup"><span data-stu-id="92b13-140">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next.**</span></span>
-   <span data-ttu-id="92b13-141">Vyberte **EntitySplitting** připojení z rozevíracího seznamu a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="92b13-141">Select the **EntitySplitting** connection from the drop down and click **Next**.</span></span>
-   <span data-ttu-id="92b13-142">V dialogovém okně Zvolte vaše databázové objekty, zaškrtněte políčko vedle položky **tabulky** uzlu.</span><span class="sxs-lookup"><span data-stu-id="92b13-142">In the Choose Your Database Objects dialog box, check the box next to the **Tables** node.</span></span>
    <span data-ttu-id="92b13-143">Tato možnost přidá všechny tabulky z **EntitySplitting** databáze do modelu.</span><span class="sxs-lookup"><span data-stu-id="92b13-143">This will add all the tables from the **EntitySplitting** database to the model.</span></span>
-   <span data-ttu-id="92b13-144">Klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="92b13-144">Click **Finish**.</span></span>

<span data-ttu-id="92b13-145">V návrháři entit, které poskytuje návrhové ploše pro úpravy váš model, se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="92b13-145">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

## <a name="map-an-entity-to-two-tables"></a><span data-ttu-id="92b13-146">Mapovat Entity na dvě tabulky</span><span class="sxs-lookup"><span data-stu-id="92b13-146">Map an Entity to Two Tables</span></span>

<span data-ttu-id="92b13-147">V tomto kroku budeme aktualizovat **osoba** typ entity kombinovat data z **osoba** a **PersonInfo** tabulky.</span><span class="sxs-lookup"><span data-stu-id="92b13-147">In this step we will update the **Person** entity type to combine data from the **Person** and **PersonInfo** tables.</span></span>

-   <span data-ttu-id="92b13-148">Vyberte **e-mailu** a **Phone** vlastnosti \*\* PersonInfo \*\* entity a stiskněte klávesu **Ctrl + X** klíče.</span><span class="sxs-lookup"><span data-stu-id="92b13-148">Select the **Email** and **Phone** properties of the \*\*PersonInfo \*\*entity and press **Ctrl+X** keys.</span></span>
-   <span data-ttu-id="92b13-149">Vyberte \*\* osoba \*\* entity a stiskněte klávesu **Ctrl + V** klíče.</span><span class="sxs-lookup"><span data-stu-id="92b13-149">Select the \*\*Person \*\*entity and press **Ctrl+V** keys.</span></span>
-   <span data-ttu-id="92b13-150">Na návrhové ploše, vyberte **PersonInfo** entity a stiskněte klávesu **odstranit** tlačítko na klávesnici.</span><span class="sxs-lookup"><span data-stu-id="92b13-150">On the design surface, select the **PersonInfo** entity and press **Delete** button on the keyboard.</span></span>
-   <span data-ttu-id="92b13-151">Klikněte na tlačítko **ne** když se zobrazí výzva, pokud chcete odebrat **PersonInfo** tabulky z modelu, jsme ji namapovat na **osoba** entity.</span><span class="sxs-lookup"><span data-stu-id="92b13-151">Click **No** when asked if you want to remove the **PersonInfo** table from the model, we are about to map it to the **Person** entity.</span></span>

    ![Odstranit tabulky](~/ef6/media/deletetables.png)

<span data-ttu-id="92b13-153">Vyžadovat další kroky **podrobnosti mapování** okna.</span><span class="sxs-lookup"><span data-stu-id="92b13-153">The next steps require the **Mapping Details** window.</span></span> <span data-ttu-id="92b13-154">Pokud toto okno nelze zobrazit, klikněte pravým tlačítkem na návrhové ploše a vyberte **podrobnosti mapování**.</span><span class="sxs-lookup"><span data-stu-id="92b13-154">If you cannot see this window, right-click the design surface and select **Mapping Details**.</span></span>

-   <span data-ttu-id="92b13-155">Vyberte **osoba** typu entity a klikněte na tlačítko **&lt;přidat tabulku nebo zobrazení&gt;** v **podrobnosti mapování** okna.</span><span class="sxs-lookup"><span data-stu-id="92b13-155">Select the **Person** entity type and click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="92b13-156">Vyberte \*\* PersonInfo \*\* z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="92b13-156">Select \*\*PersonInfo \*\* from the drop-down list.</span></span>
    <span data-ttu-id="92b13-157">**Podrobnosti mapování** okno se aktualizuje s výchozí mapování sloupců, jedná se o jemné pro náš scénář.</span><span class="sxs-lookup"><span data-stu-id="92b13-157">The **Mapping Details** window is updated with default column mappings, these are fine for our scenario.</span></span>

<span data-ttu-id="92b13-158">**Osoba** typ entity je nyní namapována na **osoba** a **PersonInfo** tabulky.</span><span class="sxs-lookup"><span data-stu-id="92b13-158">The **Person** entity type is now mapped to the **Person** and **PersonInfo** tables.</span></span>

![Mapování 2](~/ef6/media/mapping2.png)

## <a name="use-the-model"></a><span data-ttu-id="92b13-160">Použití modelu</span><span class="sxs-lookup"><span data-stu-id="92b13-160">Use the Model</span></span>

-   <span data-ttu-id="92b13-161">Vložte následující kód do metody Main.</span><span class="sxs-lookup"><span data-stu-id="92b13-161">Paste the following code in the Main method.</span></span>

``` csharp
    using (var context = new EntitySplittingEntities())
    {
        var person = new Person
        {
            FirstName = "John",
            LastName = "Doe",
            Email = "john@example.com",
            Phone = "555-555-5555"
        };

        context.People.Add(person);
        context.SaveChanges();

        foreach (var item in context.People)
        {
            Console.WriteLine(item.FirstName);
        }
    }
```

-   <span data-ttu-id="92b13-162">Kompilace a spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="92b13-162">Compile and run the application.</span></span>

<span data-ttu-id="92b13-163">Následující příkazy T-SQL byly provedeny na databázi v důsledku spuštění této aplikace.</span><span class="sxs-lookup"><span data-stu-id="92b13-163">The following T-SQL statements were executed against the database as a result of running this application.</span></span> 

-   <span data-ttu-id="92b13-164">Následující dva **vložit** příkazy byly spuštěny v důsledku spuštění kontextu. SaveChanges().</span><span class="sxs-lookup"><span data-stu-id="92b13-164">The following two **INSERT** statements were executed as a result of executing context.SaveChanges().</span></span> <span data-ttu-id="92b13-165">Přijímají data z **osoba** entity a rozdělit ho mezi **osoba** a **PersonInfo** tabulky.</span><span class="sxs-lookup"><span data-stu-id="92b13-165">They take the data from the **Person** entity and split it between the **Person** and **PersonInfo** tables.</span></span>

    ![Vložit 1](~/ef6/media/insert1.png)

    ![Vložit 2](~/ef6/media/insert2.png)
-   <span data-ttu-id="92b13-168">Následující **vyberte** se spustil v důsledku výčet uživatelů v databázi.</span><span class="sxs-lookup"><span data-stu-id="92b13-168">The following **SELECT** was executed as a result of enumerating the people in the database.</span></span> <span data-ttu-id="92b13-169">Kombinuje data z **osoba** a **PersonInfo** tabulky.</span><span class="sxs-lookup"><span data-stu-id="92b13-169">It combines the data from the **Person** and **PersonInfo** table.</span></span>

    ![Vyberte](~/ef6/media/select.png)
