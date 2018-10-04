---
title: Vazby dat s WPF – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e90d48e6-bea5-47ef-b756-7b89cce4daf0
ms.openlocfilehash: 1933988277d3be8fecc02fced3293f2b7f80c901
ms.sourcegitcommit: ae399f9f3d1bae2c446b552247bd3af3ca5a2cf9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/04/2018
ms.locfileid: "48575662"
---
# <a name="databinding-with-wpf"></a><span data-ttu-id="2e7e9-102">Vazby dat s WPF</span><span class="sxs-lookup"><span data-stu-id="2e7e9-102">Databinding with WPF</span></span>
<span data-ttu-id="2e7e9-103">Tento podrobný návod ukazuje, jak svázat ovládací prvky WPF ve formě "hlavní podrobnosti" typy POCO.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-103">This step-by-step walkthrough shows how to bind POCO types to WPF controls in a “master-detail" form.</span></span> <span data-ttu-id="2e7e9-104">Aplikace používá rozhraní API Entity Framework pro naplnění objekty s daty z databáze, sledování změn a uložení dat do databáze.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-104">The application uses the Entity Framework APIs to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="2e7e9-105">Model definuje dva typy, které se účastní vztahu jednoho k několika: **kategorie** (hlavní\\hlavní) a **produktu** (závislé\\podrobnosti).</span><span class="sxs-lookup"><span data-stu-id="2e7e9-105">The model defines two types that participate in one-to-many relationship: **Category** (principal\\master) and **Product** (dependent\\detail).</span></span> <span data-ttu-id="2e7e9-106">Nástroje sady Visual Studio se použije k vytvoření vazby typy definované v modelu, který má ovládacích prvků WPF.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-106">Then, the Visual Studio tools are used to bind the types defined in the model to the WPF controls.</span></span> <span data-ttu-id="2e7e9-107">Navigace mezi související objekty umožňuje rozhraní datové vazby WPF: výběr řádků v zobrazení předlohy způsobí, že podrobné zobrazení aktualizace pomocí odpovídající podřízená data.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-107">The WPF data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="2e7e9-108">Snímky obrazovky a výpis kódu v tomto názorném postupu pocházejí ze sady Visual Studio 2013, ale můžete dokončit tento návod s Visual Studio 2012 nebo Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-108">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a><span data-ttu-id="2e7e9-109">Pomocí možnosti 'Object' pro vytvoření zdroje dat pro WPF</span><span class="sxs-lookup"><span data-stu-id="2e7e9-109">Use the 'Object' Option for Creating WPF Data Sources</span></span>

<span data-ttu-id="2e7e9-110">V předchozí verzi rozhraní Entity Framework jsme použili doporučujeme používat **databáze** při vytváření nového zdroje dat založené na modelu vytvářené pomocí návrháře EF možnost.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-110">With previous version of Entity Framework we used to recommend using the **Database** option when creating a new Data Source based on a model created with the EF Designer.</span></span> <span data-ttu-id="2e7e9-111">To bylo, protože kontext, který je odvozen od objektu ObjectContext a tříd entit, které jsou odvozeny z EntityObject vygeneruje návrháře.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-111">This was because the designer would generate a context that derived from ObjectContext and entity classes that derived from EntityObject.</span></span> <span data-ttu-id="2e7e9-112">Pomocí možnosti databáze by usnadňuje psaní nejlepší kód pro interakci s této plochy rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-112">Using the Database option would help you write the best code for interacting with this API surface.</span></span>

<span data-ttu-id="2e7e9-113">Ať už EF pro Visual Studio 2012 a Visual Studio 2013 vygenerovat kontext, který je odvozen od položky DbContext spolu s jednoduchou tříd entit POCO.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-113">The EF Designers for Visual Studio 2012 and Visual Studio 2013 generate a context that derives from DbContext together with simple POCO entity classes.</span></span> <span data-ttu-id="2e7e9-114">Pomocí sady Visual Studio 2010 doporučujeme přechodem na šablony generování kódu, který používá kontext databáze, jak je popsáno dále v tomto návodu.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-114">With Visual Studio 2010 we recommend swapping to a code generation template that uses DbContext as described later in this walkthrough.</span></span>

<span data-ttu-id="2e7e9-115">Při použití plochy rozhraní DbContext API byste měli použít **objekt** možnosti při vytváření nového zdroje dat, jak je znázorněno v tomto názorném postupu.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-115">When using the DbContext API surface you should use the **Object** option when creating a new Data Source, as shown in this walkthrough.</span></span>

<span data-ttu-id="2e7e9-116">V případě potřeby můžete [vrátit k generování kódu na základě ObjectContext](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) pro modely vytvořené pomocí EF designeru.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-116">If needed, you can [revert to ObjectContext based code generation](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) for models created with the EF Designer.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="2e7e9-117">Předpoklady</span><span class="sxs-lookup"><span data-stu-id="2e7e9-117">Pre-Requisites</span></span>

<span data-ttu-id="2e7e9-118">Musíte mít Visual Studio 2013, Visual Studio 2012 nebo Visual Studio 2010 nainstalovaný k dokončení tohoto návodu.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-118">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="2e7e9-119">Pokud používáte Visual Studio 2010, musíte také nainstalovat NuGet.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-119">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="2e7e9-120">Další informace najdete v tématu [instalace balíčků NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).</span><span class="sxs-lookup"><span data-stu-id="2e7e9-120">For more information, see [Installing NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).</span></span>  

## <a name="create-the-application"></a><span data-ttu-id="2e7e9-121">Vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="2e7e9-121">Create the Application</span></span>

-   <span data-ttu-id="2e7e9-122">Otevřít Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2e7e9-122">Open Visual Studio</span></span>
-   <span data-ttu-id="2e7e9-123">**Soubor –&gt; nové –&gt; projektu...**</span><span class="sxs-lookup"><span data-stu-id="2e7e9-123">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="2e7e9-124">Vyberte **Windows** v levém podokně a **WPFApplication** v pravém podokně</span><span class="sxs-lookup"><span data-stu-id="2e7e9-124">Select **Windows** in the left pane and **WPFApplication** in the right pane</span></span>
-   <span data-ttu-id="2e7e9-125">Zadejte **WPFwithEFSample** jako název</span><span class="sxs-lookup"><span data-stu-id="2e7e9-125">Enter **WPFwithEFSample** as the name</span></span>
-   <span data-ttu-id="2e7e9-126">Vyberte **OK**</span><span class="sxs-lookup"><span data-stu-id="2e7e9-126">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="2e7e9-127">Nainstalovat balíček NuGet pro rozhraní Entity Framework</span><span class="sxs-lookup"><span data-stu-id="2e7e9-127">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="2e7e9-128">V Průzkumníku řešení klikněte pravým tlačítkem myši na **WinFormswithEFSample** projektu</span><span class="sxs-lookup"><span data-stu-id="2e7e9-128">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="2e7e9-129">Vyberte **spravovat balíčky NuGet...**</span><span class="sxs-lookup"><span data-stu-id="2e7e9-129">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="2e7e9-130">V dialogovém okně Spravovat balíčky NuGet vyberte **Online** kartě a zvolte **EntityFramework** balíčku</span><span class="sxs-lookup"><span data-stu-id="2e7e9-130">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="2e7e9-131">Klikněte na tlačítko **instalace**</span><span class="sxs-lookup"><span data-stu-id="2e7e9-131">Click **Install**</span></span>  
    >[!NOTE]
    > <span data-ttu-id="2e7e9-132">Kromě EntityFramework sestavení je také přidán odkaz na System.ComponentModel.DataAnnotations.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-132">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="2e7e9-133">Pokud projekt obsahuje odkaz na System.Data.Entity, pak se odebere při instalaci balíčku objektu EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-133">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="2e7e9-134">Sestavení System.Data.Entity se už používá pro aplikace Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-134">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="define-a-model"></a><span data-ttu-id="2e7e9-135">Definovat Model</span><span class="sxs-lookup"><span data-stu-id="2e7e9-135">Define a Model</span></span>

<span data-ttu-id="2e7e9-136">V tomto návodu jste se rozhodli implementují model Code First nebo EF designeru.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-136">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="2e7e9-137">Proveďte jeden z následujících dvou částech.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-137">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="2e7e9-138">Možnost 1: Definujte Model pomocí Code First</span><span class="sxs-lookup"><span data-stu-id="2e7e9-138">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="2e7e9-139">Tato část ukazuje, jak vytvořit model a jeho přidružená databáze pomocí Code First.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-139">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="2e7e9-140">Přejít k další části (**možnost 2: definovat model pomocí Database First)** Pokud byste chtěli raději použít první databázi vrátit pracovníkovi modelu z databáze pomocí EF designeru</span><span class="sxs-lookup"><span data-stu-id="2e7e9-140">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="2e7e9-141">Při použití vývoje Code First obvykle začnete vytvořením tříd rozhraní .NET Framework, které definují model koncepční (domény).</span><span class="sxs-lookup"><span data-stu-id="2e7e9-141">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="2e7e9-142">Přidejte novou třídu do **WPFwithEFSample:**</span><span class="sxs-lookup"><span data-stu-id="2e7e9-142">Add a new class to the **WPFwithEFSample:**</span></span>
    -   <span data-ttu-id="2e7e9-143">Klikněte pravým tlačítkem na název projektu</span><span class="sxs-lookup"><span data-stu-id="2e7e9-143">Right-click on the project name</span></span>
    -   <span data-ttu-id="2e7e9-144">Vyberte **přidat**, pak **nová položka**</span><span class="sxs-lookup"><span data-stu-id="2e7e9-144">Select **Add**, then **New Item**</span></span>
    -   <span data-ttu-id="2e7e9-145">Vyberte **třídy** a zadejte **produktu** pro název třídy</span><span class="sxs-lookup"><span data-stu-id="2e7e9-145">Select **Class** and enter **Product** for the class name</span></span>
-   <span data-ttu-id="2e7e9-146">Nahradit **produktu** třídy definice s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="2e7e9-146">Replace the **Product** class definition with the following code:</span></span>

``` csharp
    namespace WPFwithEFSample
    {
        public class Product
        {
            public int ProductId { get; set; }
            public string Name { get; set; }

            public int CategoryId { get; set; }
            public virtual Category Category { get; set; }
        }
    }

-   Add a **Category** class with the following definition:

    using System.Collections.ObjectModel;

    namespace WPFwithEFSample
    {
        public class Category
        {
            public Category()
            {
                this.Products = new ObservableCollection<Product>();
            }

            public int CategoryId { get; set; }
            public string Name { get; set; }

            public virtual ObservableCollection<Product> Products { get; private set; }
        }
    }
```

<span data-ttu-id="2e7e9-147">**Produkty** vlastnost **kategorie** třídy a **kategorie** vlastnost **produktu** třídy jsou navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-147">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="2e7e9-148">Navigační vlastnosti v Entity Framework, poskytují způsob, jak procházet vztah mezi dvěma typy entit.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-148">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="2e7e9-149">Kromě definování entit, budete muset definovat třídu, která je odvozena od položky DbContext a zveřejňuje DbSet&lt;TEntity&gt; vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-149">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="2e7e9-150">DbSet&lt;TEntity&gt; vlastnosti umožňují kontextu vědět, jaké typy, které chcete zahrnout do modelu.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-150">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="2e7e9-151">Instance typu DbContext odvozené spravuje objekty entity za běhu, který obsahuje naplnění objekty s daty z databáze, změňte sledování a zachovává data do databáze.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-151">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="2e7e9-152">Přidat nový **ProductContext** třídy do projektu s následující definice:</span><span class="sxs-lookup"><span data-stu-id="2e7e9-152">Add a new **ProductContext** class to the project with the following definition:</span></span>

``` csharp
    using System.Data.Entity;

    namespace WPFwithEFSample
    {
        public class ProductContext : DbContext
        {
            public DbSet<Category> Categories { get; set; }
            public DbSet<Product> Products { get; set; }
        }
    }
```

<span data-ttu-id="2e7e9-153">Zkompilujte projekt.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-153">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="2e7e9-154">Možnost 2: Definujte model pomocí Database First</span><span class="sxs-lookup"><span data-stu-id="2e7e9-154">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="2e7e9-155">Tato část ukazuje způsob použití Database First provést zpětnou analýzu modelu z databáze pomocí EF designeru.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-155">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="2e7e9-156">Pokud jste dokončili předchozí části (**možnost 1: definovat model pomocí Code First)**, tuto část přeskočit a přejít rovnou do **opožděné načtení** oddílu.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-156">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="2e7e9-157">Vytvořit existující databáze</span><span class="sxs-lookup"><span data-stu-id="2e7e9-157">Create an Existing Database</span></span>

<span data-ttu-id="2e7e9-158">Obvykle Pokud cílíte na existující databázi, které se už vytvořili, ale v tomto návodu budeme potřebovat vytvořit databázi pro přístup k.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-158">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="2e7e9-159">Databázový server, který se instaluje se sadou Visual Studio se liší v závislosti na verzi sady Visual Studio, které jste nainstalovali:</span><span class="sxs-lookup"><span data-stu-id="2e7e9-159">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="2e7e9-160">Pokud používáte Visual Studio 2010 vytvoříte databázi SQL Express.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-160">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="2e7e9-161">Pokud používáte sadu Visual Studio 2012 pak vytvoříte [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) databáze.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-161">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="2e7e9-162">Pojďme tedy vygenerovala databáze.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-162">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="2e7e9-163">**Zobrazení –&gt; Průzkumníka serveru**</span><span class="sxs-lookup"><span data-stu-id="2e7e9-163">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="2e7e9-164">Klikněte pravým tlačítkem na **datová připojení -&gt; přidat připojení...**</span><span class="sxs-lookup"><span data-stu-id="2e7e9-164">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="2e7e9-165">Pokud jste ještě nepřipojili k databázi z Průzkumníka serveru předtím, než bude nutné vybrat jako zdroj dat Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="2e7e9-165">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Změnit zdroj dat](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="2e7e9-167">Připojení k LocalDB nebo SQL Express, v závislosti na tom, co jste nainstalovali a zadejte **produkty** jako název databáze</span><span class="sxs-lookup"><span data-stu-id="2e7e9-167">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![Přidat připojení LocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![Přidat připojení Express](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="2e7e9-170">Vyberte **OK** a zobrazí se výzva, pokud chcete vytvořit novou databázi, vyberte **Ano**</span><span class="sxs-lookup"><span data-stu-id="2e7e9-170">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Vytvoření databáze](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="2e7e9-172">Nové databáze se teď budou zobrazovat v Průzkumníku serveru, klikněte pravým tlačítkem myši na něj a vyberte **nový dotaz**</span><span class="sxs-lookup"><span data-stu-id="2e7e9-172">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="2e7e9-173">Zkopírujte následující příkaz SQL na nový dotaz a pak klikněte pravým tlačítkem myši na dotazu a vyberte **spouštění**</span><span class="sxs-lookup"><span data-stu-id="2e7e9-173">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

``` SQL
    CREATE TABLE [dbo].[Categories] (
        [CategoryId] [int] NOT NULL IDENTITY,
        [Name] [nvarchar](max),
        CONSTRAINT [PK_dbo.Categories] PRIMARY KEY ([CategoryId])
    )

    CREATE TABLE [dbo].[Products] (
        [ProductId] [int] NOT NULL IDENTITY,
        [Name] [nvarchar](max),
        [CategoryId] [int] NOT NULL,
        CONSTRAINT [PK_dbo.Products] PRIMARY KEY ([ProductId])
    )

    CREATE INDEX [IX_CategoryId] ON [dbo].[Products]([CategoryId])

    ALTER TABLE [dbo].[Products] ADD CONSTRAINT [FK_dbo.Products_dbo.Categories_CategoryId] FOREIGN KEY ([CategoryId]) REFERENCES [dbo].[Categories] ([CategoryId]) ON DELETE CASCADE
```

#### <a name="reverse-engineer-model"></a><span data-ttu-id="2e7e9-174">Zpětná analýza modelu</span><span class="sxs-lookup"><span data-stu-id="2e7e9-174">Reverse Engineer Model</span></span>

<span data-ttu-id="2e7e9-175">My budeme používat Entity Framework Designer, která je součástí sady Visual Studio, k vytvoření našeho modelu.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-175">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="2e7e9-176">**Projekt –&gt; přidat novou položku...**</span><span class="sxs-lookup"><span data-stu-id="2e7e9-176">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="2e7e9-177">Vyberte **Data** v levé nabídce a potom **datový Model Entity ADO.NET**</span><span class="sxs-lookup"><span data-stu-id="2e7e9-177">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="2e7e9-178">Zadejte **ProductModel** jako název a klikněte na **OK**</span><span class="sxs-lookup"><span data-stu-id="2e7e9-178">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="2e7e9-179">Tím se spustí **Průvodce datovým modelem Entity**</span><span class="sxs-lookup"><span data-stu-id="2e7e9-179">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="2e7e9-180">Vyberte **Generovat z databáze** a klikněte na tlačítko **další**</span><span class="sxs-lookup"><span data-stu-id="2e7e9-180">Select **Generate from Database** and click **Next**</span></span>

    ![Zvolte Model obsahu](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="2e7e9-182">Vyberte připojení k databázi vytvořené v první části, zadejte **ProductContext** jako název připojovacího řetězce a klikněte na tlačítko **další**</span><span class="sxs-lookup"><span data-stu-id="2e7e9-182">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![Zvolte připojení](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="2e7e9-184">Klikněte na zaškrtávací políčko vedle "Tables" k importu všech tabulek a klikněte na tlačítko 'Dokončit'</span><span class="sxs-lookup"><span data-stu-id="2e7e9-184">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Vyberte objekty](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="2e7e9-186">Po dokončení procesu zpětné analýzy nový model je přidána do projektu a zpřístupnili k zobrazení v Návrháři Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-186">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="2e7e9-187">Soubor App.config má také byla přidána do projektu s podrobnostmi o připojení pro databázi.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-187">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="2e7e9-188">Další kroky v sadě Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="2e7e9-188">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="2e7e9-189">Pokud pracujete v sadě Visual Studio 2010 je potřeba aktualizovat EF designeru používat EF6 generování kódu.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-189">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="2e7e9-190">Klikněte pravým tlačítkem na prázdné místo modelu v EF designeru a vyberte **přidat položku generování kódu...**</span><span class="sxs-lookup"><span data-stu-id="2e7e9-190">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="2e7e9-191">Vyberte **Online šablon** v levé nabídce a vyhledejte **DbContext**</span><span class="sxs-lookup"><span data-stu-id="2e7e9-191">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="2e7e9-192">Vyberte **EF 6.x DbContext generátor pro jazyk C\#,** zadejte **ProductsModel** jako název a klikněte na tlačítko Přidat</span><span class="sxs-lookup"><span data-stu-id="2e7e9-192">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="2e7e9-193">Aktualizuje se generování kódu pro vytváření datových vazeb</span><span class="sxs-lookup"><span data-stu-id="2e7e9-193">Updating code generation for data binding</span></span>

<span data-ttu-id="2e7e9-194">EF generuje kód z modelu pomocí šablon T4.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-194">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="2e7e9-195">Šablony součástí sady Visual Studio nebo stáhnout z Galerie sady Visual Studio jsou určené pro obecné účely použití.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-195">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="2e7e9-196">To znamená, že entity vygenerované z těchto šablon mít jednoduché rozhraní ICollection&lt;T&gt; vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-196">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="2e7e9-197">Při provádění data vazby pomocí grafického subsystému WPF je však žádoucí použít **kolekci ObservableCollection** pro vlastnosti kolekce tak, že WPF může udržovat přehled o provedené změny kolekce.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-197">However, when doing data binding using WPF it is desirable to use **ObservableCollection** for collection properties so that WPF can keep track of changes made to the collections.</span></span> <span data-ttu-id="2e7e9-198">Za tímto účelem jsme se k úpravě šablony pro použití kolekci ObservableCollection.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-198">To this end we will to modify the templates to use ObservableCollection.</span></span>

-   <span data-ttu-id="2e7e9-199">Otevřít **Průzkumníka řešení** a najít **ProductModel.edmx** souboru</span><span class="sxs-lookup"><span data-stu-id="2e7e9-199">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="2e7e9-200">Najít **ProductModel.tt** souboru, který se vnoří pod ProductModel.edmx souboru</span><span class="sxs-lookup"><span data-stu-id="2e7e9-200">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![Šablona Model WPF produktu](~/ef6/media/wpfproductmodeltemplate.png)

-   <span data-ttu-id="2e7e9-202">Poklikejte na soubor ProductModel.tt ho otevřete v editoru sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2e7e9-202">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="2e7e9-203">Najít a nahradit dva výskyty "**rozhraní ICollection**"s"**kolekci ObservableCollection**".</span><span class="sxs-lookup"><span data-stu-id="2e7e9-203">Find and replace the two occurrences of “**ICollection**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="2e7e9-204">Toto jsou přibližně v řádcích 296 a 484.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-204">These are located approximately at lines 296 and 484.</span></span>
-   <span data-ttu-id="2e7e9-205">Najít a nahradit první výskyt "**HashSet**"s"**kolekci ObservableCollection**".</span><span class="sxs-lookup"><span data-stu-id="2e7e9-205">Find and replace the first occurrence of “**HashSet**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="2e7e9-206">Výskyt této události je umístěn přibližně na řádku 50.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-206">This occurrence is located approximately at line 50.</span></span> <span data-ttu-id="2e7e9-207">**Ne** nahraďte druhým výskytem HashSet najdete dále v kódu.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-207">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="2e7e9-208">Najít a nahradit jenom výskyt "**System.Collections.Generic**"s"**System.Collections.ObjectModel**".</span><span class="sxs-lookup"><span data-stu-id="2e7e9-208">Find and replace the only occurrence of “**System.Collections.Generic**” with “**System.Collections.ObjectModel**”.</span></span> <span data-ttu-id="2e7e9-209">Tento soubor je umístěn přibližně na řádku 424.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-209">This is located approximately at line 424.</span></span>
-   <span data-ttu-id="2e7e9-210">Uložte soubor ProductModel.tt.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-210">Save the ProductModel.tt file.</span></span> <span data-ttu-id="2e7e9-211">To by měl způsobí, že kód pro entity být znovu vygenerován.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-211">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="2e7e9-212">Pokud kód automaticky neobnoví, klikněte pravým tlačítkem na ProductModel.tt a zvolte možnost "Spustit vlastní nástroj".</span><span class="sxs-lookup"><span data-stu-id="2e7e9-212">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="2e7e9-213">Pokud nyní otevřete soubor Category.cs (což je vnořený ProductModel.tt), pak byste měli vidět, že má kolekce produktů typ **kolekci ObservableCollection&lt;produktu&gt;**.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-213">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableCollection&lt;Product&gt;**.</span></span>

<span data-ttu-id="2e7e9-214">Zkompilujte projekt.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-214">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="2e7e9-215">Opožděné načtení</span><span class="sxs-lookup"><span data-stu-id="2e7e9-215">Lazy Loading</span></span>

<span data-ttu-id="2e7e9-216">**Produkty** vlastnost **kategorie** třídy a **kategorie** vlastnost **produktu** třídy jsou navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-216">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="2e7e9-217">Navigační vlastnosti v Entity Framework, poskytují způsob, jak procházet vztah mezi dvěma typy entit.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-217">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="2e7e9-218">EF poskytuje možnost načtení souvisejících entit z databáze automaticky při prvním přístupu navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-218">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="2e7e9-219">S tímto typem načítání (označované jako opožděné načtení) mějte na paměti, že při prvním přístupu ke každé vlastnosti navigace samostatný dotaz se spustí na databázi není-li obsah již v kontextu.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-219">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="2e7e9-220">Při použití typů entit POCO, EF dosahuje opožděné načtení vytváření instancí typů odvozených proxy za běhu a potom přepsáním virtuálních vlastností ve třídách přidat hook načítání.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-220">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="2e7e9-221">Pokud chcete získat opožděné načtení souvisejících objektů, je třeba deklarovat navigace gettery vlastností jako **veřejné** a **virtuální** (**Overridable** v jazyce Visual Basic), a je třída nesmí být **zapečetěné** (**NotOverridable** v jazyce Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="2e7e9-221">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="2e7e9-222">Při použití databáze první navigačních vlastností se automaticky provede virtuální povolit opožděné načtení.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-222">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="2e7e9-223">V části Code First jsme zvolili, navigačních vlastností virtuální ze stejného důvodu</span><span class="sxs-lookup"><span data-stu-id="2e7e9-223">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="2e7e9-224">Vytvoření vazby objektů k ovládacím prvkům</span><span class="sxs-lookup"><span data-stu-id="2e7e9-224">Bind Object to Controls</span></span>

<span data-ttu-id="2e7e9-225">Přidání třídy, které jsou definovány v modelu jako zdroj dat pro tuto aplikaci WPF.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-225">Add the classes that are defined in the model as data sources for this WPF application.</span></span>

-   <span data-ttu-id="2e7e9-226">Dvakrát klikněte na panel **souboru MainWindow.xaml** v Průzkumníku řešení otevřete hlavní formulář</span><span class="sxs-lookup"><span data-stu-id="2e7e9-226">Double-click **MainWindow.xaml** in Solution Explorer to open the main form</span></span>
-   <span data-ttu-id="2e7e9-227">V hlavní nabídce vyberte **projekt –&gt; přidat nový zdroj dat...**</span><span class="sxs-lookup"><span data-stu-id="2e7e9-227">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="2e7e9-228">(v sadě Visual Studio 2010, je nutné vybrat **dat –&gt; přidat nový zdroj dat...** )</span><span class="sxs-lookup"><span data-stu-id="2e7e9-228">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="2e7e9-229">V seznamu zvolte Typewindow zdroje dat, vyberte **objekt** a klikněte na tlačítko **další**</span><span class="sxs-lookup"><span data-stu-id="2e7e9-229">In the Choose a Data Source Typewindow, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="2e7e9-230">Vyberte datové objekty dialogu nejextrémnějších **WPFwithEFSample** dvěma časy a vyberte **kategorie**</span><span class="sxs-lookup"><span data-stu-id="2e7e9-230">In the Select the Data Objects dialog, unfold the **WPFwithEFSample** two times and select **Category**</span></span>  
    <span data-ttu-id="2e7e9-231">*Není nutné vybrat **produktu** zdroje dat, protože jsme se na portálu **produktu**vlastnost **kategorie** zdroj dat*</span><span class="sxs-lookup"><span data-stu-id="2e7e9-231">*There is no need to select the **Product** data source, because we will get to it through the **Product**’s property on the **Category** data source*</span></span>  

    ![Vyberte datové objekty](~/ef6/media/selectdataobjects.png)

-   <span data-ttu-id="2e7e9-233">Klikněte na tlačítko **dokončit.**</span><span class="sxs-lookup"><span data-stu-id="2e7e9-233">Click **Finish.**</span></span>
-   <span data-ttu-id="2e7e9-234">Otevření okna zdrojů dat vedle okna souboru MainWindow.xaml *Pokud okna zdroje dat se nezobrazuje, vyberte **zobrazení –&gt; ostatní Windows -&gt; zdroje dat***</span><span class="sxs-lookup"><span data-stu-id="2e7e9-234">The Data Sources window is opened next to the MainWindow.xaml window *If the Data Sources window is not showing up, select **View -&gt; Other Windows-&gt; Data Sources***</span></span>
-   <span data-ttu-id="2e7e9-235">Stiskněte ikonu připínáčku tak okna zdroje dat není automaticky skrýt.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-235">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="2e7e9-236">Budete muset stiskněte tlačítko Aktualizovat, pokud už je okno viditelné.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-236">You may need to hit the refresh button if the window was already visible.</span></span>

    ![Zdroje dat](~/ef6/media/datasources.png)

-   <span data-ttu-id="2e7e9-238">Vyberte **kategorie** zdroje dat a přetáhněte ji na formuláři.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-238">Select the **Category** data source and drag it on the form.</span></span>

<span data-ttu-id="2e7e9-239">Tímto se stalo při přetažení jsme tento zdroj:</span><span class="sxs-lookup"><span data-stu-id="2e7e9-239">The following happened when we dragged this source:</span></span>

-   <span data-ttu-id="2e7e9-240">**CategoryViewSource** prostředků a **categoryDataGrid** ovládací prvek byl přidán do XAML</span><span class="sxs-lookup"><span data-stu-id="2e7e9-240">The **categoryViewSource** resource and the **categoryDataGrid** control were added to XAML</span></span> 
-   <span data-ttu-id="2e7e9-241">Vlastnost DataContext v elementu nadřazené mřížky byla nastavená na "{StaticResource **categoryViewSource** }".</span><span class="sxs-lookup"><span data-stu-id="2e7e9-241">The DataContext property on the parent Grid element was set to "{StaticResource **categoryViewSource** }".</span></span> <span data-ttu-id="2e7e9-242">**CategoryViewSource** prostředků slouží jako zdroj vazby pro vnější\\nadřazeného elementu mřížky.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-242">The **categoryViewSource** resource serves as a binding source for the outer\\parent Grid element.</span></span> <span data-ttu-id="2e7e9-243">Hodnota kontextu DataContext vnitřní elementy mřížky pak dědit z nadřazené mřížky (vlastnost ItemsSource categoryDataGrid je nastavená na "{vazba}")</span><span class="sxs-lookup"><span data-stu-id="2e7e9-243">The inner Grid elements then inherit the DataContext value from the parent Grid (the categoryDataGrid’s ItemsSource property is set to "{Binding}")</span></span>

``` xml
    <Window.Resources>
        <CollectionViewSource x:Key="categoryViewSource"
                                d:DesignSource="{d:DesignInstance {x:Type local:Category}, CreateList=True}"/>
    </Window.Resources>
    <Grid DataContext="{StaticResource categoryViewSource}">
        <DataGrid x:Name="categoryDataGrid" AutoGenerateColumns="False" EnableRowVirtualization="True"
                    ItemsSource="{Binding}" Margin="13,13,43,191"
                    RowDetailsVisibilityMode="VisibleWhenSelected">
            <DataGrid.Columns>
                <DataGridTextColumn x:Name="categoryIdColumn" Binding="{Binding CategoryId}"
                                    Header="Category Id" Width="SizeToHeader"/>
                <DataGridTextColumn x:Name="nameColumn" Binding="{Binding Name}"
                                    Header="Name" Width="SizeToHeader"/>
            </DataGrid.Columns>
        </DataGrid>
    </Grid>
```

## <a name="adding-a-details-grid"></a><span data-ttu-id="2e7e9-244">Přidání podrobností mřížky</span><span class="sxs-lookup"><span data-stu-id="2e7e9-244">Adding a Details Grid</span></span>

<span data-ttu-id="2e7e9-245">Když teď máme mřížky zobrazíte kategorie Pojďme přidáte podrobnosti mřížce se zobrazí související produkty.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-245">Now that we have a grid to display Categories let's add a details grid to display the associated Products.</span></span>

-   <span data-ttu-id="2e7e9-246">Vyberte **produkty** vlastnosti v rámci **kategorie** zdroje dat a přetáhněte ji na formuláři.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-246">Select the **Products** property from under the **Category** data source and drag it on the form.</span></span>
    -   <span data-ttu-id="2e7e9-247">**CategoryProductsViewSource** prostředků a **productDataGrid** mřížky jsou přidány do XAML</span><span class="sxs-lookup"><span data-stu-id="2e7e9-247">The **categoryProductsViewSource** resource and **productDataGrid** grid are added to XAML</span></span>
    -   <span data-ttu-id="2e7e9-248">Cesta vazby pro tento prostředek nastavená na produkty</span><span class="sxs-lookup"><span data-stu-id="2e7e9-248">The binding path for this resource is set to Products</span></span>
    -   <span data-ttu-id="2e7e9-249">Rozhraní datové vazby WPF zajistí, že pouze produkty, které souvisejí se do vybrané kategorie se zobrazí v **productDataGrid**</span><span class="sxs-lookup"><span data-stu-id="2e7e9-249">WPF data-binding framework ensures that only Products related to the selected Category show up in **productDataGrid**</span></span>
-   <span data-ttu-id="2e7e9-250">Z panelu nástrojů přetáhněte **tlačítko** do formuláře.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-250">From the Toolbox, drag **Button** on to the form.</span></span> <span data-ttu-id="2e7e9-251">Nastavte **název** vlastnost **buttonSave** a **obsahu** vlastnost **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-251">Set the **Name** property to **buttonSave** and the **Content** property to **Save**.</span></span>

<span data-ttu-id="2e7e9-252">Formulář by měl vypadat nějak takto:</span><span class="sxs-lookup"><span data-stu-id="2e7e9-252">The form should look similar to this:</span></span>

![Návrhář](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a><span data-ttu-id="2e7e9-254">Přidejte kód, který zpracovává Data interakce</span><span class="sxs-lookup"><span data-stu-id="2e7e9-254">Add Code that Handles Data Interaction</span></span>

<span data-ttu-id="2e7e9-255">Je čas přidat několik obslužných rutin událostí do hlavního okna.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-255">It's time to add some event handlers to the main window.</span></span>

-   <span data-ttu-id="2e7e9-256">V okně, XAML, klikněte na  **&lt;okno** elementu, tato možnost vybere hlavní okno</span><span class="sxs-lookup"><span data-stu-id="2e7e9-256">In the XAML window, click on the **&lt;Window** element, this selects the main window</span></span>
-   <span data-ttu-id="2e7e9-257">V **vlastnosti** okna zvolte **události** v pravém horním rohu, pak poklikejte na textové pole na pravé straně **Loaded** popisek</span><span class="sxs-lookup"><span data-stu-id="2e7e9-257">In the **Properties** window choose **Events** at the top right, then double-click the text box to right of the **Loaded** label</span></span>

    ![Hlavní okno Vlastnosti](~/ef6/media/mainwindowproperties.png)

-   <span data-ttu-id="2e7e9-259">Také přidat **klikněte na tlačítko** události **Uložit** tlačítko dvojitým kliknutím na tlačítko Uložit v návrháři.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-259">Also add the **Click** event for the **Save** button by double-clicking the Save button in the designer.</span></span> 

<span data-ttu-id="2e7e9-260">To přináší do kódu pro formulář, budeme vám teď upravovat kód, který použije ProductContext přístup k datům.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-260">This brings you to the code behind for the form, we'll now edit the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="2e7e9-261">Aktualizujte kód pro hlavního okna MainWindow, jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-261">Update the code for the MainWindow as shown below.</span></span>

<span data-ttu-id="2e7e9-262">Kód deklaruje instanci dlouhotrvající **ProductContext**.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-262">The code declares a long-running instance of **ProductContext**.</span></span> <span data-ttu-id="2e7e9-263">**ProductContext** objekt se používá k dotazování a ukládání dat do databáze.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-263">The **ProductContext** object is used to query and save data to the database.</span></span> <span data-ttu-id="2e7e9-264">**Dispose()** na **ProductContext** instance se nazývá pak z přepsané **OnClosing** metody.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-264">The **Dispose()** on the **ProductContext** instance is then called from the overridden **OnClosing** method.</span></span> <span data-ttu-id="2e7e9-265">V komentářích ke kódu obsahují podrobné informace o co kód dělá.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-265">The code comments provide details about what the code does.</span></span>

``` csharp
    using System.Data.Entity;
    using System.Linq;
    using System.Windows;

    namespace WPFwithEFSample
    {
        public partial class MainWindow : Window
        {
            private ProductContext _context = new ProductContext();
            public MainWindow()
            {
                InitializeComponent();
            }

            private void Window_Loaded(object sender, RoutedEventArgs e)
            {
                System.Windows.Data.CollectionViewSource categoryViewSource =
                    ((System.Windows.Data.CollectionViewSource)(this.FindResource("categoryViewSource")));

                // Load is an extension method on IQueryable,
                // defined in the System.Data.Entity namespace.
                // This method enumerates the results of the query,
                // similar to ToList but without creating a list.
                // When used with Linq to Entities this method
                // creates entity objects and adds them to the context.
                _context.Categories.Load();

                // After the data is loaded call the DbSet<T>.Local property
                // to use the DbSet<T> as a binding source.
                categoryViewSource.Source = _context.Categories.Local;
            }

            private void buttonSave_Click(object sender, RoutedEventArgs e)
            {
                // When you delete an object from the related entities collection
                // (in this case Products), the Entity Framework doesn’t mark
                // these child entities as deleted.
                // Instead, it removes the relationship between the parent and the child
                // by setting the parent reference to null.
                // So we manually have to delete the products
                // that have a Category reference set to null.

                // The following code uses LINQ to Objects
                // against the Local collection of Products.
                // The ToList call is required because otherwise the collection will be modified
                // by the Remove call while it is being enumerated.
                // In most other situations you can use LINQ to Objects directly
                // against the Local property without using ToList first.
                foreach (var product in _context.Products.Local.ToList())
                {
                    if (product.Category == null)
                    {
                        _context.Products.Remove(product);
                    }
                }

                _context.SaveChanges();
                // Refresh the grids so the database generated values show up.
                this.categoryDataGrid.Items.Refresh();
                this.productsDataGrid.Items.Refresh();
            }

            protected override void OnClosing(System.ComponentModel.CancelEventArgs e)
            {
                base.OnClosing(e);
                this._context.Dispose();
            }
        }

    }
```

## <a name="test-the-wpf-application"></a><span data-ttu-id="2e7e9-266">Testování aplikace WPF</span><span class="sxs-lookup"><span data-stu-id="2e7e9-266">Test the WPF Application</span></span>

-   <span data-ttu-id="2e7e9-267">Kompilace a spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-267">Compile and run the application.</span></span> <span data-ttu-id="2e7e9-268">Pokud jste použili Code First, pak uvidíte, že **WPFwithEFSample.ProductContext** databáze se vytvoří za vás.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-268">If you used Code First, then you will see that a **WPFwithEFSample.ProductContext** database is created for you.</span></span>
-   <span data-ttu-id="2e7e9-269">Zadejte název kategorie v mřížce a produktu důležití v mřížce dolů *nezadávejte nic v ID sloupce, protože primární klíč je generován databází*</span><span class="sxs-lookup"><span data-stu-id="2e7e9-269">Enter a category name in the top grid and product names in the bottom grid *Do not enter anything in ID columns, because the primary key is generated by the database*</span></span>

    ![Hlavní okno se nové kategorie a produkty](~/ef6/media/screen1.png)

-   <span data-ttu-id="2e7e9-271">Stisknutím klávesy **Uložit** tlačítko pro uložení dat do databáze</span><span class="sxs-lookup"><span data-stu-id="2e7e9-271">Press the **Save** button to save the data to the database</span></span>

<span data-ttu-id="2e7e9-272">Po volání na DbContext **SaveChanges()**, ID jsou vyplněna podle hodnot v databázi vygeneruje.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-272">After the call to DbContext’s **SaveChanges()**, the IDs are populated with the database generated values.</span></span> <span data-ttu-id="2e7e9-273">Protože jsme volat **Refresh()** po **SaveChanges()** **DataGrid** ovládací prvky jsou aktualizovány s novými hodnotami.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-273">Because we called **Refresh()** after **SaveChanges()** the **DataGrid** controls are updated with the new values as well.</span></span>

![Hlavní okno s ID vyplní](~/ef6/media/screen2.png)

## <a name="additional-resources"></a><span data-ttu-id="2e7e9-275">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="2e7e9-275">Additional Resources</span></span>

<span data-ttu-id="2e7e9-276">Další informace o vytváření datových vazeb do kolekce pomocí grafického subsystému WPF, naleznete v tématu [v tomto tématu](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) v dokumentaci k WPF.</span><span class="sxs-lookup"><span data-stu-id="2e7e9-276">To learn more about data binding to collections using WPF, see [this topic](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) in the WPF documentation.</span></span>  
