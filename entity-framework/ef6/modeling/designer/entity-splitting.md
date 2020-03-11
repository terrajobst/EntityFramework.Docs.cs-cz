---
title: Rozdělení entity návrháře – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: aa2dd48a-1f0e-49dd-863d-d6b4f5834832
ms.openlocfilehash: ba1895ae491cec909ff88a8784eea82f1876f595
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418460"
---
# <a name="designer-entity-splitting"></a><span data-ttu-id="0672f-102">Rozdělení entity návrháře</span><span class="sxs-lookup"><span data-stu-id="0672f-102">Designer Entity Splitting</span></span>
<span data-ttu-id="0672f-103">Tento návod ukazuje, jak namapovat typ entity na dvě tabulky úpravou modelu pomocí Entity Framework Designer (EF Designer).</span><span class="sxs-lookup"><span data-stu-id="0672f-103">This walkthrough shows how to map an entity type to two tables by modifying a model with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="0672f-104">Pokud tabulky sdílejí společný klíč, můžete ji namapovat na více tabulek.</span><span class="sxs-lookup"><span data-stu-id="0672f-104">You can map an entity to multiple tables when the tables share a common key.</span></span> <span data-ttu-id="0672f-105">Koncepty, které platí pro mapování typu entity na dvě tabulky, lze snadno rozšířit a mapovat typ entity na více než dvě tabulky.</span><span class="sxs-lookup"><span data-stu-id="0672f-105">The concepts that apply to mapping an entity type to two tables are easily extended to mapping an entity type to more than two tables.</span></span>

<span data-ttu-id="0672f-106">Následující obrázek ukazuje hlavní okna, která se používají při práci s návrhářem EF.</span><span class="sxs-lookup"><span data-stu-id="0672f-106">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![Návrhář EF](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a><span data-ttu-id="0672f-108">Předpoklady</span><span class="sxs-lookup"><span data-stu-id="0672f-108">Prerequisites</span></span>

<span data-ttu-id="0672f-109">Visual Studio 2012 nebo Visual Studio 2010, Ultimate, Premium, Professional nebo web Express Edition.</span><span class="sxs-lookup"><span data-stu-id="0672f-109">Visual Studio 2012 or Visual Studio 2010, Ultimate, Premium, Professional, or Web Express edition.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="0672f-110">Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="0672f-110">Create the Database</span></span>

<span data-ttu-id="0672f-111">Databázový server, který je nainstalovaný se sadou Visual Studio, se liší v závislosti na verzi sady Visual Studio, kterou jste nainstalovali:</span><span class="sxs-lookup"><span data-stu-id="0672f-111">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="0672f-112">Pokud používáte Visual Studio 2012, budete vytvářet databázi LocalDB.</span><span class="sxs-lookup"><span data-stu-id="0672f-112">If you are using Visual Studio 2012 then you'll be creating a LocalDB database.</span></span>
-   <span data-ttu-id="0672f-113">Pokud používáte Visual Studio 2010, budete vytvářet databázi SQL Express.</span><span class="sxs-lookup"><span data-stu-id="0672f-113">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>

<span data-ttu-id="0672f-114">Nejprve vytvoříme databázi se dvěma tabulkami, které budeme zkombinovat do jedné entity.</span><span class="sxs-lookup"><span data-stu-id="0672f-114">First we'll create a database with two tables that we are going to combine into a single entity.</span></span>

-   <span data-ttu-id="0672f-115">Otevřete sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0672f-115">Open Visual Studio</span></span>
-   <span data-ttu-id="0672f-116">**Zobrazení-&gt; Průzkumník serveru**</span><span class="sxs-lookup"><span data-stu-id="0672f-116">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="0672f-117">Klikněte pravým tlačítkem na **datová připojení –&gt; přidat připojení...**</span><span class="sxs-lookup"><span data-stu-id="0672f-117">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="0672f-118">Pokud jste se k databázi nepřipojili z Průzkumník serveru před tím, než bude nutné vybrat **Microsoft SQL Server** jako zdroj dat</span><span class="sxs-lookup"><span data-stu-id="0672f-118">If you haven’t connected to a database from Server Explorer before you’ll need to select **Microsoft SQL Server** as the data source</span></span>
-   <span data-ttu-id="0672f-119">Připojte se k LocalDB nebo SQL Express v závislosti na tom, který z nich máte nainstalovanou.</span><span class="sxs-lookup"><span data-stu-id="0672f-119">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>
-   <span data-ttu-id="0672f-120">Jako název databáze zadejte **EntitySplitting** .</span><span class="sxs-lookup"><span data-stu-id="0672f-120">Enter **EntitySplitting** as the database name</span></span>
-   <span data-ttu-id="0672f-121">Vyberte **OK** a zobrazí se dotaz, jestli chcete vytvořit novou databázi, a pak vyberte **Ano** .</span><span class="sxs-lookup"><span data-stu-id="0672f-121">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="0672f-122">Nová databáze se nyní zobrazí v Průzkumník serveru</span><span class="sxs-lookup"><span data-stu-id="0672f-122">The new database will now appear in Server Explorer</span></span>
-   <span data-ttu-id="0672f-123">Pokud používáte Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="0672f-123">If you are using Visual Studio 2012</span></span>
    -   <span data-ttu-id="0672f-124">Klikněte pravým tlačítkem na databázi v Průzkumník serveru a vyberte **Nový dotaz** .</span><span class="sxs-lookup"><span data-stu-id="0672f-124">Right-click on the database in Server Explorer and select **New Query**</span></span>
    -   <span data-ttu-id="0672f-125">Zkopírujte následující příkaz SQL do nového dotazu, klikněte na něj pravým tlačítkem myši a vyberte **Spustit** .</span><span class="sxs-lookup"><span data-stu-id="0672f-125">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>
-   <span data-ttu-id="0672f-126">Pokud používáte Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="0672f-126">If you are using Visual Studio 2010</span></span>
    -   <span data-ttu-id="0672f-127">Vyberte **data-&gt; Editor jazyka Transact SQL –&gt; nové připojení dotazu...**</span><span class="sxs-lookup"><span data-stu-id="0672f-127">Select **Data -&gt; Transact SQL Editor -&gt; New Query Connection...**</span></span>
    -   <span data-ttu-id="0672f-128">Jako název serveru zadejte **.\\SQLEXPRESS** a klikněte na **OK** .</span><span class="sxs-lookup"><span data-stu-id="0672f-128">Enter **.\\SQLEXPRESS** as the server name and click **OK**</span></span>
    -   <span data-ttu-id="0672f-129">Vyberte databázi **EntitySplitting** v rozevíracím seznamu v horní části editoru dotazů.</span><span class="sxs-lookup"><span data-stu-id="0672f-129">Select the **EntitySplitting** database from the drop down at the top of the query editor</span></span>
    -   <span data-ttu-id="0672f-130">Zkopírujte následující příkaz SQL do nového dotazu, potom klikněte pravým tlačítkem na dotaz a vyberte **Spustit SQL** .</span><span class="sxs-lookup"><span data-stu-id="0672f-130">Copy the following SQL into the new query, then right-click on the query and select **Execute SQL**</span></span>

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

## <a name="create-the-project"></a><span data-ttu-id="0672f-131">Vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="0672f-131">Create the Project</span></span>

-   <span data-ttu-id="0672f-132">V nabídce **soubor** přejděte na příkaz **Nový**a klikněte na **projekt**.</span><span class="sxs-lookup"><span data-stu-id="0672f-132">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="0672f-133">V levém podokně klikněte na položku **Visual C\#** a pak vyberte šablonu **Konzolová aplikace** .</span><span class="sxs-lookup"><span data-stu-id="0672f-133">In the left pane, click **Visual C\#**, and then select the **Console Application** template.</span></span>
-   <span data-ttu-id="0672f-134">Jako název projektu zadejte **MapEntityToTablesSample** a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="0672f-134">Enter **MapEntityToTablesSample** as the name of the project and click **OK**.</span></span>
-   <span data-ttu-id="0672f-135">Pokud se zobrazí výzva k uložení dotazu SQL vytvořeného v první části, klikněte na **ne** .</span><span class="sxs-lookup"><span data-stu-id="0672f-135">Click **No** if prompted to save the SQL query created in the first section.</span></span>

## <a name="create-a-model-based-on-the-database"></a><span data-ttu-id="0672f-136">Vytvoření modelu založeného na databázi</span><span class="sxs-lookup"><span data-stu-id="0672f-136">Create a Model based on the Database</span></span>

-   <span data-ttu-id="0672f-137">Klikněte pravým tlačítkem myši na název projektu v Průzkumník řešení, přejděte na **Přidat**a klikněte na **Nová položka**.</span><span class="sxs-lookup"><span data-stu-id="0672f-137">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="0672f-138">V nabídce vlevo vyberte **data** a v podokně šablony vyberte **ADO.NET model EDM (Entity Data Model)** .</span><span class="sxs-lookup"><span data-stu-id="0672f-138">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="0672f-139">Jako název souboru zadejte **MapEntityToTablesModel. edmx** a pak klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="0672f-139">Enter **MapEntityToTablesModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="0672f-140">V dialogovém okně Vybrat obsah modelu vyberte možnost **Generovat z databáze**a poté klikněte na tlačítko **Další.**</span><span class="sxs-lookup"><span data-stu-id="0672f-140">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next.**</span></span>
-   <span data-ttu-id="0672f-141">V rozevíracím seznamu vyberte připojení **EntitySplitting** a klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="0672f-141">Select the **EntitySplitting** connection from the drop down and click **Next**.</span></span>
-   <span data-ttu-id="0672f-142">V dialogovém okně zvolit objekty databáze zaškrtněte políčko vedle **tabulky** uzel.</span><span class="sxs-lookup"><span data-stu-id="0672f-142">In the Choose Your Database Objects dialog box, check the box next to the **Tables** node.</span></span>
    <span data-ttu-id="0672f-143">Tato akce přidá všechny tabulky z databáze **EntitySplitting** do modelu.</span><span class="sxs-lookup"><span data-stu-id="0672f-143">This will add all the tables from the **EntitySplitting** database to the model.</span></span>
-   <span data-ttu-id="0672f-144">Klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="0672f-144">Click **Finish**.</span></span>

<span data-ttu-id="0672f-145">Zobrazí se Entity Designer, která poskytuje návrhovou plochu pro úpravu vašeho modelu.</span><span class="sxs-lookup"><span data-stu-id="0672f-145">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

## <a name="map-an-entity-to-two-tables"></a><span data-ttu-id="0672f-146">Mapování entity na dvě tabulky</span><span class="sxs-lookup"><span data-stu-id="0672f-146">Map an Entity to Two Tables</span></span>

<span data-ttu-id="0672f-147">V tomto kroku aktualizujeme typ entity **Person** na kombinování dat z tabulek **Person** a **PersonInfo** .</span><span class="sxs-lookup"><span data-stu-id="0672f-147">In this step we will update the **Person** entity type to combine data from the **Person** and **PersonInfo** tables.</span></span>

-   <span data-ttu-id="0672f-148">Vyberte vlastnosti **e-mailu** a **telefonů** entity **PersonInfo **a stiskněte klávesy **CTRL + X** .</span><span class="sxs-lookup"><span data-stu-id="0672f-148">Select the **Email** and **Phone** properties of the **PersonInfo **entity and press **Ctrl+X** keys.</span></span>
-   <span data-ttu-id="0672f-149">Vyberte entitu **osoba **a stiskněte klávesy **CTRL + V** .</span><span class="sxs-lookup"><span data-stu-id="0672f-149">Select the **Person **entity and press **Ctrl+V** keys.</span></span>
-   <span data-ttu-id="0672f-150">Na návrhové ploše vyberte entitu  **PersonInfo** a stiskněte tlačítko **Odstranit** na klávesnici.</span><span class="sxs-lookup"><span data-stu-id="0672f-150">On the design surface, select the **PersonInfo** entity and press **Delete** button on the keyboard.</span></span>
-   <span data-ttu-id="0672f-151">Po zobrazení výzvy klikněte na tlačítko **ne** , pokud chcete odebrat tabulku **PersonInfo** z modelu, chystáme se ji namapovat na entitu **Person** .</span><span class="sxs-lookup"><span data-stu-id="0672f-151">Click **No** when asked if you want to remove the **PersonInfo** table from the model, we are about to map it to the **Person** entity.</span></span>

    ![Odstranit tabulky](~/ef6/media/deletetables.png)

<span data-ttu-id="0672f-153">Další kroky vyžadují okno s **podrobnostmi mapování** .</span><span class="sxs-lookup"><span data-stu-id="0672f-153">The next steps require the **Mapping Details** window.</span></span> <span data-ttu-id="0672f-154">Pokud toto okno nevidíte, klikněte pravým tlačítkem myši na návrhovou plochu a vyberte **mapování podrobností**.</span><span class="sxs-lookup"><span data-stu-id="0672f-154">If you cannot see this window, right-click the design surface and select **Mapping Details**.</span></span>

-   <span data-ttu-id="0672f-155">Vyberte typ entity  **osoba** a v okně **Podrobnosti mapování** klikněte na **&lt;přidat tabulku nebo zobrazení&gt;**  .</span><span class="sxs-lookup"><span data-stu-id="0672f-155">Select the **Person** entity type and click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="0672f-156">V rozevíracím seznamu vyberte **PersonInfo ** .</span><span class="sxs-lookup"><span data-stu-id="0672f-156">Select **PersonInfo ** from the drop-down list.</span></span>
    <span data-ttu-id="0672f-157">Okno  **podrobností mapování** se aktualizuje s výchozími mapováními sloupců, což je pro náš scénář přesné.</span><span class="sxs-lookup"><span data-stu-id="0672f-157">The **Mapping Details** window is updated with default column mappings, these are fine for our scenario.</span></span>

<span data-ttu-id="0672f-158">Typ entity  **osoba** je teď namapovaný na **uživatele** a tabulky **PersonInfo** .</span><span class="sxs-lookup"><span data-stu-id="0672f-158">The **Person** entity type is now mapped to the **Person** and **PersonInfo** tables.</span></span>

![Mapování 2](~/ef6/media/mapping2.png)

## <a name="use-the-model"></a><span data-ttu-id="0672f-160">Použití modelu</span><span class="sxs-lookup"><span data-stu-id="0672f-160">Use the Model</span></span>

-   <span data-ttu-id="0672f-161">Do metody Main vložte následující kód.</span><span class="sxs-lookup"><span data-stu-id="0672f-161">Paste the following code in the Main method.</span></span>

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

-   <span data-ttu-id="0672f-162">Zkompilujte a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0672f-162">Compile and run the application.</span></span>

<span data-ttu-id="0672f-163">V důsledku spuštění této aplikace byly v databázi provedeny následující příkazy jazyka T-SQL.</span><span class="sxs-lookup"><span data-stu-id="0672f-163">The following T-SQL statements were executed against the database as a result of running this application.</span></span> 

-   <span data-ttu-id="0672f-164">Následující dva příkazy **INSERT** byly provedeny jako výsledek spuštěného kontextu. SaveChanges ().</span><span class="sxs-lookup"><span data-stu-id="0672f-164">The following two **INSERT** statements were executed as a result of executing context.SaveChanges().</span></span> <span data-ttu-id="0672f-165">Přebírají data z entity **Person** a rozdělí je mezi tabulka **Person** a **PersonInfo** .</span><span class="sxs-lookup"><span data-stu-id="0672f-165">They take the data from the **Person** entity and split it between the **Person** and **PersonInfo** tables.</span></span>

    ![Vložit 1](~/ef6/media/insert1.png)

    ![Vložit 2](~/ef6/media/insert2.png)
-   <span data-ttu-id="0672f-168">Následující **příkaz SELECT** byl proveden jako výsledek vytváření výčtu osob v databázi.</span><span class="sxs-lookup"><span data-stu-id="0672f-168">The following **SELECT** was executed as a result of enumerating the people in the database.</span></span> <span data-ttu-id="0672f-169">Kombinuje data z tabulky **Person** a **PersonInfo** .</span><span class="sxs-lookup"><span data-stu-id="0672f-169">It combines the data from the **Person** and **PersonInfo** table.</span></span>

    ![Vyberte](~/ef6/media/select.png)
