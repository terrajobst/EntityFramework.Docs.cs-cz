---
title: Vlastní sledování entity návod – EF6
author: divega
ms.date: 2016-10-23
ms.assetid: b21207c9-1d95-4aa3-ae05-bc5fe300dab0
ms.openlocfilehash: 64ca9ae42df1a1c740131e254b8f80f67b2f9f97
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995418"
---
# <a name="self-tracking-entities-walkthrough"></a><span data-ttu-id="e2c9f-102">Vlastní sledování návod entity</span><span class="sxs-lookup"><span data-stu-id="e2c9f-102">Self-Tracking Entities Walkthrough</span></span>
> [!IMPORTANT]
> <span data-ttu-id="e2c9f-103">Už doporučujeme použít šablonu samoobslužného tracking entity.</span><span class="sxs-lookup"><span data-stu-id="e2c9f-103">We no longer recommend using the self-tracking-entities template.</span></span> <span data-ttu-id="e2c9f-104">Pouze bude nadále být k dispozici pro podporu stávající aplikace.</span><span class="sxs-lookup"><span data-stu-id="e2c9f-104">It will only continue to be available to support existing applications.</span></span> <span data-ttu-id="e2c9f-105">Pokud aplikace potřebuje pracovat s grafy odpojených entit, zvažte další možnosti, jako [organizovaným entity](http://trackableentities.github.io/), což je technologie, podobně jako u samoobslužného-Tracking-entit, které je více aktivně vyvíjen Komunita, nebo psaní vlastního kódu pomocí nízké úrovně rozhraní API pro sledování změn.</span><span class="sxs-lookup"><span data-stu-id="e2c9f-105">If your application requires working with disconnected graphs of entities, consider other alternatives such as [Trackable Entities](http://trackableentities.github.io/), which is a technology similar to Self-Tracking-Entities that is more actively developed by the community, or writing custom code using the low-level change tracking APIs.</span></span>

<span data-ttu-id="e2c9f-106">Tento návod ukazuje scénář, ve kterém služby Windows Communication Foundation (WCF) poskytuje operace, která se vrátí entit grafu.</span><span class="sxs-lookup"><span data-stu-id="e2c9f-106">This walkthrough demonstrates the scenario in which a Windows Communication Foundation (WCF) service exposes an operation that returns an entity graph.</span></span> <span data-ttu-id="e2c9f-107">V dalším kroku klientská aplikace provádí úpravy tohoto grafu a odešle změny operace služby, která ověřuje a uloží aktualizace k databázi pomocí Entity Frameworku.</span><span class="sxs-lookup"><span data-stu-id="e2c9f-107">Next, a client application manipulates that graph and submits the modifications to a service operation that validates and saves the updates to a database using Entity Framework.</span></span>

<span data-ttu-id="e2c9f-108">Před dokončením tohoto návodu Ujistěte se, že čtete [Self-Tracking entity](index.md) stránky.</span><span class="sxs-lookup"><span data-stu-id="e2c9f-108">Before completing this walkthrough make sure you read the [Self-Tracking Entities](index.md) page.</span></span>

<span data-ttu-id="e2c9f-109">Tento návod provede následující akce:</span><span class="sxs-lookup"><span data-stu-id="e2c9f-109">This walkthrough completes the following actions:</span></span>

-   <span data-ttu-id="e2c9f-110">Vytvoří databázi přístup.</span><span class="sxs-lookup"><span data-stu-id="e2c9f-110">Creates a database to access.</span></span>
-   <span data-ttu-id="e2c9f-111">Vytvoří knihovnu tříd, která obsahuje model.</span><span class="sxs-lookup"><span data-stu-id="e2c9f-111">Creates a class library that contains the model.</span></span>
-   <span data-ttu-id="e2c9f-112">Záměna Self-Tracking Entity generátor šablony.</span><span class="sxs-lookup"><span data-stu-id="e2c9f-112">Swaps to the Self-Tracking Entity Generator template.</span></span>
-   <span data-ttu-id="e2c9f-113">Přesune tříd entit do samostatného projektu.</span><span class="sxs-lookup"><span data-stu-id="e2c9f-113">Moves the entity classes to a separate project.</span></span>
-   <span data-ttu-id="e2c9f-114">Vytvoří službu WCF, která poskytuje operace pro dotazování a uložení entity.</span><span class="sxs-lookup"><span data-stu-id="e2c9f-114">Creates a WCF service that exposes operations to query and save entities.</span></span>
-   <span data-ttu-id="e2c9f-115">Vytvoří klienta aplikace (konzoly a WPF), které využívají službu.</span><span class="sxs-lookup"><span data-stu-id="e2c9f-115">Creates client applications (Console and WPF) that consume the service.</span></span>

<span data-ttu-id="e2c9f-116">Použijeme první databáze v tomto podrobném návodu, ale stejné techniky stejnou měrou vztahují na první Model.</span><span class="sxs-lookup"><span data-stu-id="e2c9f-116">We'll use Database First in this walkthrough but the same techniques apply equally to Model First.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="e2c9f-117">Předpoklady</span><span class="sxs-lookup"><span data-stu-id="e2c9f-117">Pre-Requisites</span></span>

<span data-ttu-id="e2c9f-118">K dokončení tohoto návodu potřebujete novější verzi sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e2c9f-118">To complete this walkthrough you will need a recent version of Visual Studio.</span></span>

## <a name="create-a-database"></a><span data-ttu-id="e2c9f-119">Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="e2c9f-119">Create a Database</span></span>

<span data-ttu-id="e2c9f-120">Databázový server, který se instaluje se sadou Visual Studio se liší v závislosti na verzi sady Visual Studio, které jste nainstalovali:</span><span class="sxs-lookup"><span data-stu-id="e2c9f-120">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="e2c9f-121">Pokud používáte sadu Visual Studio 2012 pak vytvoříte databázi LocalDB.</span><span class="sxs-lookup"><span data-stu-id="e2c9f-121">If you are using Visual Studio 2012 then you'll be creating a LocalDB database.</span></span>
-   <span data-ttu-id="e2c9f-122">Pokud používáte Visual Studio 2010 vytvoříte databázi SQL Express.</span><span class="sxs-lookup"><span data-stu-id="e2c9f-122">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>

<span data-ttu-id="e2c9f-123">Pojďme tedy vygenerovala databáze.</span><span class="sxs-lookup"><span data-stu-id="e2c9f-123">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="e2c9f-124">Otevřít Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e2c9f-124">Open Visual Studio</span></span>
-   <span data-ttu-id="e2c9f-125">**Zobrazení –&gt; Průzkumníka serveru**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-125">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="e2c9f-126">Klikněte pravým tlačítkem na **datová připojení -&gt; přidat připojení...**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-126">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="e2c9f-127">Pokud jste ještě nepřipojili k databázi z Průzkumníka serveru předtím, než bude nutné vybrat **Microsoft SQL Server** jako zdroj dat</span><span class="sxs-lookup"><span data-stu-id="e2c9f-127">If you haven’t connected to a database from Server Explorer before you’ll need to select **Microsoft SQL Server** as the data source</span></span>
-   <span data-ttu-id="e2c9f-128">Připojte se k LocalDB nebo SQL Express, v závislosti na tom, co jste nainstalovali</span><span class="sxs-lookup"><span data-stu-id="e2c9f-128">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>
-   <span data-ttu-id="e2c9f-129">Zadejte **STESample** jako název databáze</span><span class="sxs-lookup"><span data-stu-id="e2c9f-129">Enter **STESample** as the database name</span></span>
-   <span data-ttu-id="e2c9f-130">Vyberte **OK** a zobrazí se výzva, pokud chcete vytvořit novou databázi, vyberte **Ano**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-130">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="e2c9f-131">Nová databáze se teď budou zobrazovat v Průzkumníku serveru</span><span class="sxs-lookup"><span data-stu-id="e2c9f-131">The new database will now appear in Server Explorer</span></span>
-   <span data-ttu-id="e2c9f-132">Pokud používáte sadu Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="e2c9f-132">If you are using Visual Studio 2012</span></span>
    -   <span data-ttu-id="e2c9f-133">Klikněte pravým tlačítkem na databázi v Průzkumníku serveru a vyberte **nový dotaz**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-133">Right-click on the database in Server Explorer and select **New Query**</span></span>
    -   <span data-ttu-id="e2c9f-134">Zkopírujte následující příkaz SQL na nový dotaz a pak klikněte pravým tlačítkem myši na dotazu a vyberte **spouštění**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-134">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>
-   <span data-ttu-id="e2c9f-135">Pokud používáte Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="e2c9f-135">If you are using Visual Studio 2010</span></span>
    -   <span data-ttu-id="e2c9f-136">Vyberte **dat –&gt; jazyka Transact SQL Editor –&gt; nové připojení dotazu...**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-136">Select **Data -&gt; Transact SQL Editor -&gt; New Query Connection...**</span></span>
    -   <span data-ttu-id="e2c9f-137">Zadejte **.\\ SQLEXPRESS** jako název serveru a klikněte na **OK**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-137">Enter **.\\SQLEXPRESS** as the server name and click **OK**</span></span>
    -   <span data-ttu-id="e2c9f-138">Vyberte **STESample** databázi z rozevíracího seznamu v horní části editoru dotazů</span><span class="sxs-lookup"><span data-stu-id="e2c9f-138">Select the **STESample** database from the drop down at the top of the query editor</span></span>
    -   <span data-ttu-id="e2c9f-139">Zkopírujte následující příkaz SQL na nový dotaz a pak klikněte pravým tlačítkem myši na dotazu a vyberte **provést SQL**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-139">Copy the following SQL into the new query, then right-click on the query and select **Execute SQL**</span></span>

``` SQL
    CREATE TABLE [dbo].[Blogs] (
        [BlogId] INT IDENTITY (1, 1) NOT NULL,
        [Name] NVARCHAR (200) NULL,
        [Url]  NVARCHAR (200) NULL,
        CONSTRAINT [PK_dbo.Blogs] PRIMARY KEY CLUSTERED ([BlogId] ASC)
    );

    CREATE TABLE [dbo].[Posts] (
        [PostId] INT IDENTITY (1, 1) NOT NULL,
        [Title] NVARCHAR (200) NULL,
        [Content] NTEXT NULL,
        [BlogId] INT NOT NULL,
        CONSTRAINT [PK_dbo.Posts] PRIMARY KEY CLUSTERED ([PostId] ASC),
        CONSTRAINT [FK_dbo.Posts_dbo.Blogs_BlogId] FOREIGN KEY ([BlogId]) REFERENCES [dbo].[Blogs] ([BlogId]) ON DELETE CASCADE
    );

    SET IDENTITY_INSERT [dbo].[Blogs] ON
    INSERT INTO [dbo].[Blogs] ([BlogId], [Name], [Url]) VALUES (1, N'ADO.NET Blog', N'blogs.msdn.com/adonet')
    SET IDENTITY_INSERT [dbo].[Blogs] OFF
    INSERT INTO [dbo].[Posts] ([Title], [Content], [BlogId]) VALUES (N'Intro to EF', N'Interesting stuff...', 1)
    INSERT INTO [dbo].[Posts] ([Title], [Content], [BlogId]) VALUES (N'What is New', N'More interesting stuff...', 1)
```

## <a name="create-the-model"></a><span data-ttu-id="e2c9f-140">Vytvoření modelu</span><span class="sxs-lookup"><span data-stu-id="e2c9f-140">Create the Model</span></span>

<span data-ttu-id="e2c9f-141">Až, nejprve je třeba projekt se umístí modelu.</span><span class="sxs-lookup"><span data-stu-id="e2c9f-141">First up, we need a project to put the model in.</span></span>

-   <span data-ttu-id="e2c9f-142">**Soubor –&gt; nové –&gt; projektu...**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-142">**File -&gt; New -&gt; Project...**</span></span>
-   <span data-ttu-id="e2c9f-143">Vyberte **Visual C\#**  v levém podokně a pak **knihovny tříd**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-143">Select **Visual C\#** from the left pane and then **Class Library**</span></span>
-   <span data-ttu-id="e2c9f-144">Zadejte **STESample** jako název a klikněte na **OK**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-144">Enter **STESample** as the name and click **OK**</span></span>

<span data-ttu-id="e2c9f-145">Teď vytvoříme jednoduchý model v EF designeru pro přístup k naší databázi:</span><span class="sxs-lookup"><span data-stu-id="e2c9f-145">Now we'll create a simple model in the EF Designer to access our database:</span></span>

-   <span data-ttu-id="e2c9f-146">**Projekt –&gt; přidat novou položku...**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-146">**Project -&gt; Add New Item...**</span></span>
-   <span data-ttu-id="e2c9f-147">Vyberte **Data** v levém podokně a pak **datový Model Entity ADO.NET**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-147">Select **Data** from the left pane and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="e2c9f-148">Zadejte **BloggingModel** jako název a klikněte na **OK**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-148">Enter **BloggingModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="e2c9f-149">Vyberte **Generovat z databáze** a klikněte na tlačítko **další**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-149">Select **Generate from database** and click **Next**</span></span>
-   <span data-ttu-id="e2c9f-150">Zadejte informace o připojení pro databázi, kterou jste vytvořili v předchozí části</span><span class="sxs-lookup"><span data-stu-id="e2c9f-150">Enter the connection information for the database that you created in the previous section</span></span>
-   <span data-ttu-id="e2c9f-151">Zadejte **BloggingContext** jako název připojovacího řetězce a klikněte na **další**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-151">Enter **BloggingContext** as the name for the connection string and click **Next**</span></span>
-   <span data-ttu-id="e2c9f-152">Zaškrtněte políčko vedle položky **tabulky** a klikněte na tlačítko **dokončit**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-152">Check the box next to **Tables** and click **Finish**</span></span>

## <a name="swap-to-ste-code-generation"></a><span data-ttu-id="e2c9f-153">Zaměnit STE generování kódu</span><span class="sxs-lookup"><span data-stu-id="e2c9f-153">Swap to STE Code Generation</span></span>

<span data-ttu-id="e2c9f-154">Nyní potřebujeme zakázat výchozí generování kódu a přepnutí na Self-Tracking entity.</span><span class="sxs-lookup"><span data-stu-id="e2c9f-154">Now we need to disable the default code generation and swap to Self-Tracking Entities.</span></span>

### <a name="if-you-are-using-visual-studio-2012"></a><span data-ttu-id="e2c9f-155">Pokud používáte sadu Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="e2c9f-155">If you are using Visual Studio 2012</span></span>

-   <span data-ttu-id="e2c9f-156">Rozbalte **BloggingModel.edmx** v **Průzkumníka řešení** a odstranit **BloggingModel.tt** a **BloggingModel.Context.tt** 
     *Tato akce zakáže výchozí generování kódu*</span><span class="sxs-lookup"><span data-stu-id="e2c9f-156">Expand **BloggingModel.edmx** in **Solution Explorer** and delete the **BloggingModel.tt** and **BloggingModel.Context.tt**
*This will disable the default code generation*</span></span>
-   <span data-ttu-id="e2c9f-157">Klikněte pravým tlačítkem na prázdnou oblast na EF designeru ploše a vyberte možnost **přidat položku generování kódu...**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-157">Right-click an empty area on the EF Designer surface and select **Add Code Generation Item...**</span></span>
-   <span data-ttu-id="e2c9f-158">Vyberte **Online** v levém podokně a vyhledejte **generátor STE**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-158">Select **Online** from the left pane and search for **STE Generator**</span></span>
-   <span data-ttu-id="e2c9f-159">Vyberte **STE generátor pro jazyk C\#**  šablony, zadejte **STETemplate** jako název a klikněte na **přidat**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-159">Select the **STE Generator for C\#** template, enter **STETemplate** as the name and click **Add**</span></span>
-   <span data-ttu-id="e2c9f-160">**STETemplate.tt** a **STETemplate.Context.tt** soubory budou přidány pod BloggingModel.edmx souboru</span><span class="sxs-lookup"><span data-stu-id="e2c9f-160">The **STETemplate.tt** and **STETemplate.Context.tt** files are added nested under the BloggingModel.edmx file</span></span>

### <a name="if-you-are-using-visual-studio-2010"></a><span data-ttu-id="e2c9f-161">Pokud používáte Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="e2c9f-161">If you are using Visual Studio 2010</span></span>

-   <span data-ttu-id="e2c9f-162">Klikněte pravým tlačítkem na prázdnou oblast na EF designeru ploše a vyberte možnost **přidat položku generování kódu...**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-162">Right-click an empty area on the EF Designer surface and select **Add Code Generation Item...**</span></span>
-   <span data-ttu-id="e2c9f-163">Vyberte **kód** v levém podokně a pak **generátor Entity Self-Tracking ADO.NET**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-163">Select **Code** from the left pane and then **ADO.NET Self-Tracking Entity Generator**</span></span>
-   <span data-ttu-id="e2c9f-164">Zadejte **STETemplate** jako název a klikněte na **přidat**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-164">Enter **STETemplate** as the name and click **Add**</span></span>
-   <span data-ttu-id="e2c9f-165">**STETemplate.tt** a **STETemplate.Context.tt** se přidají soubory přímo do vašeho projektu</span><span class="sxs-lookup"><span data-stu-id="e2c9f-165">The **STETemplate.tt** and **STETemplate.Context.tt** files are added directly to your project</span></span>

## <a name="move-entity-types-into-separate-project"></a><span data-ttu-id="e2c9f-166">Přesuňte typy entit do samostatného projektu</span><span class="sxs-lookup"><span data-stu-id="e2c9f-166">Move Entity Types into Separate Project</span></span>

<span data-ttu-id="e2c9f-167">Použití Self-Tracking entity naše klientská aplikace potřebuje přístup k entity prostor tříd vygenerovaných z našeho modelu.</span><span class="sxs-lookup"><span data-stu-id="e2c9f-167">To use Self-Tracking Entities our client application needs access to the entity classes generated from our model.</span></span> <span data-ttu-id="e2c9f-168">Protože nechceme zpřístupnit celý model do klientské aplikace teď ještě chvíli Zůstaneme k přesunutí tříd entit do samostatného projektu.</span><span class="sxs-lookup"><span data-stu-id="e2c9f-168">Because we don't want to expose the whole model to the client application we're going to move the entity classes into a separate project.</span></span>

<span data-ttu-id="e2c9f-169">Prvním krokem je se přestávají generovat tříd entit v existující projekt:</span><span class="sxs-lookup"><span data-stu-id="e2c9f-169">The first step is to stop generating entity classes in the existing project:</span></span>

-   <span data-ttu-id="e2c9f-170">Klikněte pravým tlačítkem na **STETemplate.tt** v **Průzkumníka řešení** a vyberte **vlastnosti**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-170">Right-click on **STETemplate.tt** in **Solution Explorer** and select **Properties**</span></span>
-   <span data-ttu-id="e2c9f-171">V **vlastnosti** okno vymazat **TextTemplatingFileGenerator** z **CustomTool** vlastnost</span><span class="sxs-lookup"><span data-stu-id="e2c9f-171">In the **Properties** window clear **TextTemplatingFileGenerator** from the **CustomTool** property</span></span>
-   <span data-ttu-id="e2c9f-172">Rozbalte **STETemplate.tt** v **Průzkumníka řešení** a odstraňte všechny soubory vnořená dole</span><span class="sxs-lookup"><span data-stu-id="e2c9f-172">Expand **STETemplate.tt** in **Solution Explorer** and delete all files nested under it</span></span>

<span data-ttu-id="e2c9f-173">V dalším kroku budeme přidat nový projekt a v něm generování tříd entit</span><span class="sxs-lookup"><span data-stu-id="e2c9f-173">Next, we are going to add a new project and generate the entity classes in it</span></span>

-   <span data-ttu-id="e2c9f-174">**Soubor –&gt; Add -&gt; projektu...**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-174">**File -&gt; Add -&gt; Project...**</span></span>
-   <span data-ttu-id="e2c9f-175">Vyberte **Visual C\#**  v levém podokně a pak **knihovny tříd**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-175">Select **Visual C\#** from the left pane and then **Class Library**</span></span>
-   <span data-ttu-id="e2c9f-176">Zadejte **STESample.Entities** jako název a klikněte na **OK**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-176">Enter **STESample.Entities** as the name and click **OK**</span></span>
-   <span data-ttu-id="e2c9f-177">**Projekt –&gt; přidat existující položku...**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-177">**Project -&gt; Add Existing Item...**</span></span>
-   <span data-ttu-id="e2c9f-178">Přejděte **STESample** složky projektu</span><span class="sxs-lookup"><span data-stu-id="e2c9f-178">Navigate to the **STESample** project folder</span></span>
-   <span data-ttu-id="e2c9f-179">Vyberte, chcete-li zobrazit **všechny soubory (\*.\*)**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-179">Select to view **All Files (\*.\*)**</span></span>
-   <span data-ttu-id="e2c9f-180">Vyberte **STETemplate.tt** souboru</span><span class="sxs-lookup"><span data-stu-id="e2c9f-180">Select the **STETemplate.tt** file</span></span>
-   <span data-ttu-id="e2c9f-181">Klikněte na šipku rozevíracího seznamu vedle **přidat** tlačítko a vyberte **přidat jako odkaz**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-181">Click on the drop down arrow next to the **Add** button and select **Add As Link**</span></span>

    ![AddLinkedTemplate](~/ef6/media/addlinkedtemplate.png)

<span data-ttu-id="e2c9f-183">My budeme také zajistit, aby že vygeneruje tříd entit ve stejném oboru názvů jako kontext.</span><span class="sxs-lookup"><span data-stu-id="e2c9f-183">We're also going to make sure the entity classes get generated in the same namespace as the context.</span></span> <span data-ttu-id="e2c9f-184">Právě to snižuje počet pomocí příkazů, které je potřeba přidat v celé aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e2c9f-184">This just reduces the number of using statements we need to add throughout our application.</span></span>

-   <span data-ttu-id="e2c9f-185">Klikněte pravým tlačítkem na propojený **STETemplate.tt** v **Průzkumníka řešení** a vyberte **vlastnosti**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-185">Right-click on the linked **STETemplate.tt** in **Solution Explorer** and select **Properties**</span></span>
-   <span data-ttu-id="e2c9f-186">V **vlastnosti** okno sady **Custom Tool Namespace** k **STESample**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-186">In the **Properties** window set **Custom Tool Namespace** to **STESample**</span></span>

<span data-ttu-id="e2c9f-187">Kód vygenerovaný šablony STE potřebovat odkaz na **System.Runtime.Serialization** kompilaci.</span><span class="sxs-lookup"><span data-stu-id="e2c9f-187">The code generated by the STE template will need a reference to **System.Runtime.Serialization** in order to compile.</span></span> <span data-ttu-id="e2c9f-188">Tato knihovna je potřeba pro WCF **kontraktu dat DataContract** a **DataMember** atributy, které se používají na typy serializovatelné entit.</span><span class="sxs-lookup"><span data-stu-id="e2c9f-188">This library is needed for the WCF **DataContract** and **DataMember** attributes that are used on the serializable entity types.</span></span>

-   <span data-ttu-id="e2c9f-189">Klikněte pravým tlačítkem na **STESample.Entities** projekt **Průzkumníka řešení** a vyberte **přidat odkaz...**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-189">Right click on the **STESample.Entities** project in **Solution Explorer** and select **Add Reference...**</span></span>
    -   <span data-ttu-id="e2c9f-190">V sadě Visual Studio 2012 – zaškrtněte políčko vedle položky **System.Runtime.Serialization** a klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-190">In Visual Studio 2012 - check the box next to **System.Runtime.Serialization** and click **OK**</span></span>
    -   <span data-ttu-id="e2c9f-191">V sadě Visual Studio 2010 – vyberte **System.Runtime.Serialization** a klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-191">In Visual Studio 2010 - select **System.Runtime.Serialization** and click **OK**</span></span>

<span data-ttu-id="e2c9f-192">A konečně projekt se náš kontext v něm potřebovat odkaz na typy entit.</span><span class="sxs-lookup"><span data-stu-id="e2c9f-192">Finally, the project with our context in it will need a reference to the entity types.</span></span>

-   <span data-ttu-id="e2c9f-193">Klikněte pravým tlačítkem na **STESample** projekt **Průzkumníka řešení** a vyberte **přidat odkaz...**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-193">Right click on the **STESample** project in **Solution Explorer** and select **Add Reference...**</span></span>
    -   <span data-ttu-id="e2c9f-194">V sadě Visual Studio 2012 – vyberte **řešení** v levém podokně zaškrtněte políčko vedle položky **STESample.Entities** a klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-194">In Visual Studio 2012 - select **Solution** from the left pane, check the box next to **STESample.Entities** and click **OK**</span></span>
    -   <span data-ttu-id="e2c9f-195">V sadě Visual Studio 2010 – vyberte **projekty** kartu, vyberte možnost **STESample.Entities** a klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-195">In Visual Studio 2010 - select the **Projects** tab, select **STESample.Entities** and click **OK**</span></span>

>[!NOTE]
> <span data-ttu-id="e2c9f-196">Další možností pro typy entit Přesun do samostatného projektu je přesunout soubor šablony, nikoli propojení z výchozího umístění.</span><span class="sxs-lookup"><span data-stu-id="e2c9f-196">Another option for moving the entity types to a separate project is to move the template file, rather than linking it from its default location.</span></span> <span data-ttu-id="e2c9f-197">Pokud to uděláte, budete muset aktualizovat **inputFile** proměnné v šabloně zadat relativní cestu k souboru edmx (v tomto příkladu, která by byla **... \\BloggingModel.edmx**).</span><span class="sxs-lookup"><span data-stu-id="e2c9f-197">If you do this, you will need to update the **inputFile** variable in the template to provide the relative path to the edmx file (in this example that would be **..\\BloggingModel.edmx**).</span></span>

## <a name="create-a-wcf-service"></a><span data-ttu-id="e2c9f-198">Vytvoření služby WCF</span><span class="sxs-lookup"><span data-stu-id="e2c9f-198">Create a WCF Service</span></span>

<span data-ttu-id="e2c9f-199">Teď je čas na přidání zpřístupnit naše data ve službě WCF, začneme vytvořením projektu.</span><span class="sxs-lookup"><span data-stu-id="e2c9f-199">Now it's time to add a WCF Service to expose our data, we'll start by creating the project.</span></span>

-   <span data-ttu-id="e2c9f-200">**Soubor –&gt; Add -&gt; projektu...**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-200">**File -&gt; Add -&gt; Project...**</span></span>
-   <span data-ttu-id="e2c9f-201">Vyberte **Visual C\#**  v levém podokně a pak **aplikace služby WCF**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-201">Select **Visual C\#** from the left pane and then **WCF Service Application**</span></span>
-   <span data-ttu-id="e2c9f-202">Zadejte **STESample.Service** jako název a klikněte na **OK**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-202">Enter **STESample.Service** as the name and click **OK**</span></span>
-   <span data-ttu-id="e2c9f-203">Přidejte odkaz na **System.Data.Entity** sestavení</span><span class="sxs-lookup"><span data-stu-id="e2c9f-203">Add a reference to the **System.Data.Entity** assembly</span></span>
-   <span data-ttu-id="e2c9f-204">Přidejte odkaz na **STESample** a **STESample.Entities** projekty</span><span class="sxs-lookup"><span data-stu-id="e2c9f-204">Add a reference to the **STESample** and **STESample.Entities** projects</span></span>

<span data-ttu-id="e2c9f-205">Potřebujeme zkopírujte připojovací řetězec EF do tohoto projektu tak, aby se nachází za běhu.</span><span class="sxs-lookup"><span data-stu-id="e2c9f-205">We need to copy the EF connection string to this project so that it is found at runtime.</span></span>

-   <span data-ttu-id="e2c9f-206">Otevřít **App.Config** souboru ** STESample ** projektu a zkopírujte **connectionStrings** – element</span><span class="sxs-lookup"><span data-stu-id="e2c9f-206">Open the **App.Config** file for the **STESample **project and copy the **connectionStrings** element</span></span>
-   <span data-ttu-id="e2c9f-207">Vložit **connectionStrings** prvek jako podřízený prvek **konfigurace** elementu **Web.Config** soubor **STESample.Service** projektu</span><span class="sxs-lookup"><span data-stu-id="e2c9f-207">Paste the **connectionStrings** element as a child element of the **configuration** element of the **Web.Config** file in the **STESample.Service** project</span></span>

<span data-ttu-id="e2c9f-208">Nyní je čas k implementaci aktuální služby.</span><span class="sxs-lookup"><span data-stu-id="e2c9f-208">Now it's time to implement the actual service.</span></span>

-   <span data-ttu-id="e2c9f-209">Otevřít **IService1.cs** a nahraďte jeho obsah následujícím kódem</span><span class="sxs-lookup"><span data-stu-id="e2c9f-209">Open **IService1.cs** and replace the contents with the following code</span></span>

``` csharp
    using System.Collections.Generic;
    using System.ServiceModel;

    namespace STESample.Service
    {
        [ServiceContract]
        public interface IService1
        {
            [OperationContract]
            List<Blog> GetBlogs();

            [OperationContract]
            void UpdateBlog(Blog blog);
        }
    }
```

-   <span data-ttu-id="e2c9f-210">Otevřít **Service1.svc** a nahraďte jeho obsah následujícím kódem</span><span class="sxs-lookup"><span data-stu-id="e2c9f-210">Open **Service1.svc** and replace the contents with the following code</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data;
    using System.Linq;

    namespace STESample.Service
    {
        public class Service1 : IService1
        {
            /// <summary>
            /// Gets all the Blogs and related Posts.
            /// </summary>
            public List<Blog> GetBlogs()
            {
                using (BloggingContext context = new BloggingContext())
                {
                    return context.Blogs.Include("Posts").ToList();
                }
            }

            /// <summary>
            /// Updates Blog and its related Posts.
            /// </summary>
            public void UpdateBlog(Blog blog)
            {
                using (BloggingContext context = new BloggingContext())
                {
                    try
                    {
                        // TODO: Perform validation on the updated order before applying the changes.

                        // The ApplyChanges method examines the change tracking information
                        // contained in the graph of self-tracking entities to infer the set of operations
                        // that need to be performed to reflect the changes in the database.
                        context.Blogs.ApplyChanges(blog);
                        context.SaveChanges();

                    }
                    catch (UpdateException)
                    {
                        // To avoid propagating exception messages that contain sensitive data to the client tier
                        // calls to ApplyChanges and SaveChanges should be wrapped in exception handling code.
                        throw new InvalidOperationException("Failed to update. Try your request again.");
                    }
                }
            }        
        }
    }
```

## <a name="consume-the-service-from-a-console-application"></a><span data-ttu-id="e2c9f-211">Používání této služby z konzolové aplikace</span><span class="sxs-lookup"><span data-stu-id="e2c9f-211">Consume the Service from a Console Application</span></span>

<span data-ttu-id="e2c9f-212">Pojďme vytvořit konzolovou aplikaci, která využívá naši službu.</span><span class="sxs-lookup"><span data-stu-id="e2c9f-212">Let's create a console application that uses our service.</span></span>

-   <span data-ttu-id="e2c9f-213">**Soubor –&gt; nové –&gt; projektu...**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-213">**File -&gt; New -&gt; Project...**</span></span>
-   <span data-ttu-id="e2c9f-214">Vyberte **Visual C\#**  v levém podokně a pak **konzolové aplikace**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-214">Select **Visual C\#** from the left pane and then **Console Application**</span></span>
-   <span data-ttu-id="e2c9f-215">Zadejte **STESample.ConsoleTest** jako název a klikněte na **OK**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-215">Enter **STESample.ConsoleTest** as the name and click **OK**</span></span>
-   <span data-ttu-id="e2c9f-216">Přidejte odkaz na **STESample.Entities** projektu</span><span class="sxs-lookup"><span data-stu-id="e2c9f-216">Add a reference to the **STESample.Entities** project</span></span>

<span data-ttu-id="e2c9f-217">Potřebujeme odkazu na službu do naší službě WCF</span><span class="sxs-lookup"><span data-stu-id="e2c9f-217">We need a service reference to our WCF service</span></span>

-   <span data-ttu-id="e2c9f-218">Klikněte pravým tlačítkem myši **STESample.ConsoleTest** projekt **Průzkumníka řešení** a vyberte **přidat odkaz na službu...**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-218">Right-click the **STESample.ConsoleTest** project in **Solution Explorer** and select **Add Service Reference...**</span></span>
-   <span data-ttu-id="e2c9f-219">Klikněte na tlačítko **zjišťování**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-219">Click **Discover**</span></span>
-   <span data-ttu-id="e2c9f-220">Zadejte **BloggingService** jako obor názvů a klikněte na **OK**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-220">Enter **BloggingService** as the namespace and click **OK**</span></span>

<span data-ttu-id="e2c9f-221">Nyní jsme můžete napsat kód k používání této služby.</span><span class="sxs-lookup"><span data-stu-id="e2c9f-221">Now we can write some code to consume the service.</span></span>

-   <span data-ttu-id="e2c9f-222">Otevřít **Program.cs** a nahraďte jeho obsah následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="e2c9f-222">Open **Program.cs** and replace the contents with the following code.</span></span>

``` csharp
    using STESample.ConsoleTest.BloggingService;
    using System;
    using System.Linq;

    namespace STESample.ConsoleTest
    {
        class Program
        {
            static void Main(string[] args)
            {
                // Print out the data before we change anything
                Console.WriteLine("Initial Data:");
                DisplayBlogsAndPosts();

                // Add a new Blog and some Posts
                AddBlogAndPost();
                Console.WriteLine("After Adding:");
                DisplayBlogsAndPosts();

                // Modify the Blog and one of its Posts
                UpdateBlogAndPost();
                Console.WriteLine("After Update:");
                DisplayBlogsAndPosts();

                // Delete the Blog and its Posts
                DeleteBlogAndPost();
                Console.WriteLine("After Delete:");
                DisplayBlogsAndPosts();

                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

            static void DisplayBlogsAndPosts()
            {
                using (var service = new Service1Client())
                {
                    // Get all Blogs (and Posts) from the service
                    // and print them to the console
                    var blogs = service.GetBlogs();
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(blog.Name);
                        foreach (var post in blog.Posts)
                        {
                            Console.WriteLine(" - {0}", post.Title);
                        }
                    }
                }

                Console.WriteLine();
                Console.WriteLine();
            }

            static void AddBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Create a new Blog with a couple of Posts
                    var newBlog = new Blog
                    {
                        Name = "The New Blog",
                        Posts =
                        {
                            new Post { Title = "Welcome to the new blog"},
                            new Post { Title = "What's new on the new blog"}
                        }
                    };

                    // Save the changes using the service
                    service.UpdateBlog(newBlog);
                }
            }

            static void UpdateBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Get all the Blogs
                    var blogs = service.GetBlogs();

                    // Use LINQ to Objects to find The New Blog
                    var blog = blogs.First(b => b.Name == "The New Blog");

                    // Update the Blogs name
                    blog.Name = "The Not-So-New Blog";

                    // Update one of the related posts
                    blog.Posts.First().Content = "Some interesting content...";

                    // Save the changes using the service
                    service.UpdateBlog(blog);
                }
            }

            static void DeleteBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Get all the Blogs
                    var blogs = service.GetBlogs();

                    // Use LINQ to Objects to find The Not-So-New Blog
                    var blog = blogs.First(b => b.Name == "The Not-So-New Blog");

                    // Mark all related Posts for deletion
                    // We need to call ToList because each Post will be removed from the
                    // Posts collection when we call MarkAsDeleted
                    foreach (var post in blog.Posts.ToList())
                    {
                        post.MarkAsDeleted();
                    }

                    // Mark the Blog for deletion
                    blog.MarkAsDeleted();

                    // Save the changes using the service
                    service.UpdateBlog(blog);
                }
            }
        }
    }
```

<span data-ttu-id="e2c9f-223">Nyní můžete spustit aplikaci sledujte v akci.</span><span class="sxs-lookup"><span data-stu-id="e2c9f-223">You can now run the application to see it in action.</span></span>

-   <span data-ttu-id="e2c9f-224">Klikněte pravým tlačítkem **STESample.ConsoleTest** projekt **Průzkumníku řešení** a vyberte **ladění -&gt; zahájit novou instanci**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-224">Right-click the **STESample.ConsoleTest** project in **Solution Explorer** and select **Debug -&gt; Start new instance**</span></span>

<span data-ttu-id="e2c9f-225">Pokud aplikace provádí, zobrazí se následující výstup.</span><span class="sxs-lookup"><span data-stu-id="e2c9f-225">You'll see the following output when the application executes.</span></span>

```
Initial Data:
ADO.NET Blog
- Intro to EF
- What is New

After Adding:
ADO.NET Blog
- Intro to EF
- What is New
The New Blog
- Welcome to the new blog
- What's new on the new blog

After Update:
ADO.NET Blog
- Intro to EF
- What is New
The Not-So-New Blog
- Welcome to the new blog
- What's new on the new blog

After Delete:
ADO.NET Blog
- Intro to EF
- What is New

Press any key to exit...
```

## <a name="consume-the-service-from-a-wpf-application"></a><span data-ttu-id="e2c9f-226">Využívat služby z aplikace WPF</span><span class="sxs-lookup"><span data-stu-id="e2c9f-226">Consume the Service from a WPF Application</span></span>

<span data-ttu-id="e2c9f-227">Vytvoříme aplikaci WPF, která využívá naši službu.</span><span class="sxs-lookup"><span data-stu-id="e2c9f-227">Let's create a WPF application that uses our service.</span></span>

-   <span data-ttu-id="e2c9f-228">**Soubor –&gt; nové –&gt; projektu...**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-228">**File -&gt; New -&gt; Project...**</span></span>
-   <span data-ttu-id="e2c9f-229">Vyberte **Visual C\#**  v levém podokně a pak **aplikace WPF**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-229">Select **Visual C\#** from the left pane and then **WPF Application**</span></span>
-   <span data-ttu-id="e2c9f-230">Zadejte **STESample.WPFTest** jako název a klikněte na **OK**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-230">Enter **STESample.WPFTest** as the name and click **OK**</span></span>
-   <span data-ttu-id="e2c9f-231">Přidejte odkaz na **STESample.Entities** projektu</span><span class="sxs-lookup"><span data-stu-id="e2c9f-231">Add a reference to the **STESample.Entities** project</span></span>

<span data-ttu-id="e2c9f-232">Potřebujeme odkazu na službu do naší službě WCF</span><span class="sxs-lookup"><span data-stu-id="e2c9f-232">We need a service reference to our WCF service</span></span>

-   <span data-ttu-id="e2c9f-233">Klikněte pravým tlačítkem myši **STESample.WPFTest** projekt **Průzkumníka řešení** a vyberte **přidat odkaz na službu...**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-233">Right-click the **STESample.WPFTest** project in **Solution Explorer** and select **Add Service Reference...**</span></span>
-   <span data-ttu-id="e2c9f-234">Klikněte na tlačítko **zjišťování**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-234">Click **Discover**</span></span>
-   <span data-ttu-id="e2c9f-235">Zadejte **BloggingService** jako obor názvů a klikněte na **OK**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-235">Enter **BloggingService** as the namespace and click **OK**</span></span>

<span data-ttu-id="e2c9f-236">Nyní jsme můžete napsat kód k používání této služby.</span><span class="sxs-lookup"><span data-stu-id="e2c9f-236">Now we can write some code to consume the service.</span></span>

-   <span data-ttu-id="e2c9f-237">Otevřít **souboru MainWindow.xaml** a nahraďte jeho obsah následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="e2c9f-237">Open **MainWindow.xaml** and replace the contents with the following code.</span></span>

``` xaml
    <Window
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:STESample="clr-namespace:STESample;assembly=STESample.Entities"
        mc:Ignorable="d" x:Class="STESample.WPFTest.MainWindow"
        Title="MainWindow" Height="350" Width="525" Loaded="Window_Loaded">

        <Window.Resources>
            <CollectionViewSource
                x:Key="blogViewSource"
                d:DesignSource="{d:DesignInstance {x:Type STESample:Blog}, CreateList=True}"/>
            <CollectionViewSource
                x:Key="blogPostsViewSource"
                Source="{Binding Posts, Source={StaticResource blogViewSource}}"/>
        </Window.Resources>

        <Grid DataContext="{StaticResource blogViewSource}">
            <DataGrid AutoGenerateColumns="False" EnableRowVirtualization="True"
                      ItemsSource="{Binding}" Margin="10,10,10,179">
                <DataGrid.Columns>
                    <DataGridTextColumn Binding="{Binding BlogId}" Header="Id" Width="Auto" IsReadOnly="True" />
                    <DataGridTextColumn Binding="{Binding Name}" Header="Name" Width="Auto"/>
                    <DataGridTextColumn Binding="{Binding Url}" Header="Url" Width="Auto"/>
                </DataGrid.Columns>
            </DataGrid>
            <DataGrid AutoGenerateColumns="False" EnableRowVirtualization="True"
                      ItemsSource="{Binding Source={StaticResource blogPostsViewSource}}" Margin="10,145,10,38">
                <DataGrid.Columns>
                    <DataGridTextColumn Binding="{Binding PostId}" Header="Id" Width="Auto"  IsReadOnly="True"/>
                    <DataGridTextColumn Binding="{Binding Title}" Header="Title" Width="Auto"/>
                    <DataGridTextColumn Binding="{Binding Content}" Header="Content" Width="Auto"/>
                </DataGrid.Columns>
            </DataGrid>
            <Button Width="68" Height="23" HorizontalAlignment="Right" VerticalAlignment="Bottom"
                    Margin="0,0,10,10" Click="buttonSave_Click">Save</Button>
        </Grid>
    </Window>
```

-   <span data-ttu-id="e2c9f-238">Otevřete kódu pro hlavní okno MainWindow (**MainWindow.xaml.cs**) a nahraďte jeho obsah následujícím kódem</span><span class="sxs-lookup"><span data-stu-id="e2c9f-238">Open the code behind for MainWindow (**MainWindow.xaml.cs**) and replace the contents with the following code</span></span>

``` csharp
    using STESample.WPFTest.BloggingService;
    using System.Collections.Generic;
    using System.Linq;
    using System.Windows;
    using System.Windows.Data;

    namespace STESample.WPFTest
    {
        public partial class MainWindow : Window
        {
            public MainWindow()
            {
                InitializeComponent();
            }

            private void Window_Loaded(object sender, RoutedEventArgs e)
            {
                using (var service = new Service1Client())
                {
                    // Find the view source for Blogs and populate it with all Blogs (and related Posts)
                    // from the Service. The default editing functionality of WPF will allow the objects
                    // to be manipulated on the screen.
                    var blogsViewSource = (CollectionViewSource)this.FindResource("blogViewSource");
                    blogsViewSource.Source = service.GetBlogs().ToList();
                }
            }

            private void buttonSave_Click(object sender, RoutedEventArgs e)
            {
                using (var service = new Service1Client())
                {
                    // Get the blogs that are bound to the screen
                    var blogsViewSource = (CollectionViewSource)this.FindResource("blogViewSource");
                    var blogs = (List<Blog>)blogsViewSource.Source;

                    // Save all Blogs and related Posts
                    foreach (var blog in blogs)
                    {
                        service.UpdateBlog(blog);
                    }

                    // Re-query for data to get database-generated keys etc.
                    blogsViewSource.Source = service.GetBlogs().ToList();
                }
            }
        }
    }
```

<span data-ttu-id="e2c9f-239">Nyní můžete spustit aplikaci sledujte v akci.</span><span class="sxs-lookup"><span data-stu-id="e2c9f-239">You can now run the application to see it in action.</span></span>

-   <span data-ttu-id="e2c9f-240">Klikněte pravým tlačítkem **STESample.WPFTest** projekt **Průzkumníku řešení** a vyberte **ladění -&gt; zahájit novou instanci**</span><span class="sxs-lookup"><span data-stu-id="e2c9f-240">Right-click the **STESample.WPFTest** project in **Solution Explorer** and select **Debug -&gt; Start new instance**</span></span>
-   <span data-ttu-id="e2c9f-241">Můžete pracovat s daty na obrazovce a uložte ho prostřednictvím používání služby **Uložit** tlačítko</span><span class="sxs-lookup"><span data-stu-id="e2c9f-241">You can manipulate the data using the screen and save it via the service using the **Save** button</span></span>

![WPF](~/ef6/media/wpf.png)
