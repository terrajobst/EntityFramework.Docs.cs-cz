---
title: Vázání dat pomocí WPF-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e90d48e6-bea5-47ef-b756-7b89cce4daf0
ms.openlocfilehash: 1933988277d3be8fecc02fced3293f2b7f80c901
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416594"
---
# <a name="databinding-with-wpf"></a><span data-ttu-id="d459c-102">Vázání dat pomocí WPF</span><span class="sxs-lookup"><span data-stu-id="d459c-102">Databinding with WPF</span></span>
<span data-ttu-id="d459c-103">V tomto podrobném návodu se dozvíte, jak navazovat POCO typy na ovládací prvky WPF ve formuláři "Master-Detail".</span><span class="sxs-lookup"><span data-stu-id="d459c-103">This step-by-step walkthrough shows how to bind POCO types to WPF controls in a “master-detail" form.</span></span> <span data-ttu-id="d459c-104">Aplikace používá rozhraní Entity Framework API k naplnění objektů daty z databáze, sledování změn a zachování dat v databázi.</span><span class="sxs-lookup"><span data-stu-id="d459c-104">The application uses the Entity Framework APIs to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="d459c-105">Model definuje dva typy, které se účastní vztahu 1:1: **kategorie** (hlavní\\hlavní server) a **produkt** (podrobnosti o závislých\\ch).</span><span class="sxs-lookup"><span data-stu-id="d459c-105">The model defines two types that participate in one-to-many relationship: **Category** (principal\\master) and **Product** (dependent\\detail).</span></span> <span data-ttu-id="d459c-106">Nástroje sady Visual Studio pak slouží k navázání typů definovaných v modelu na ovládací prvky WPF.</span><span class="sxs-lookup"><span data-stu-id="d459c-106">Then, the Visual Studio tools are used to bind the types defined in the model to the WPF controls.</span></span> <span data-ttu-id="d459c-107">Rozhraní WPF pro datovou vazbu umožňuje navigaci mezi souvisejícími objekty: výběrem řádků v zobrazení Předloha způsobí, že se podrobné zobrazení aktualizuje s odpovídajícími podřízenými daty.</span><span class="sxs-lookup"><span data-stu-id="d459c-107">The WPF data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="d459c-108">Snímky obrazovky a výpisy kódu v tomto návodu jsou pořízeny z Visual Studio 2013, ale můžete tento návod dokončit pomocí sady Visual Studio 2012 nebo Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="d459c-108">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a><span data-ttu-id="d459c-109">Použití možnosti Object pro vytváření zdrojů dat WPF</span><span class="sxs-lookup"><span data-stu-id="d459c-109">Use the 'Object' Option for Creating WPF Data Sources</span></span>

<span data-ttu-id="d459c-110">V předchozí verzi Entity Framework jsme použili k doporučení použití možnosti **databáze** při vytváření nového zdroje dat založeného na modelu vytvořeném pomocí návrháře EF.</span><span class="sxs-lookup"><span data-stu-id="d459c-110">With previous version of Entity Framework we used to recommend using the **Database** option when creating a new Data Source based on a model created with the EF Designer.</span></span> <span data-ttu-id="d459c-111">Důvodem je, že návrhář vygeneroval kontext, který je odvozen z třídy ObjectContext a entity, které jsou odvozeny z objektů EntityObject.</span><span class="sxs-lookup"><span data-stu-id="d459c-111">This was because the designer would generate a context that derived from ObjectContext and entity classes that derived from EntityObject.</span></span> <span data-ttu-id="d459c-112">Použití možnosti databáze vám pomůže při psaní nejlepšího kódu pro interakci s tímto povrchem rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="d459c-112">Using the Database option would help you write the best code for interacting with this API surface.</span></span>

<span data-ttu-id="d459c-113">Návrháři EF pro Visual Studio 2012 a Visual Studio 2013 generují kontext, který je odvozený z DbContext společně s jednoduchými třídami POCO entit.</span><span class="sxs-lookup"><span data-stu-id="d459c-113">The EF Designers for Visual Studio 2012 and Visual Studio 2013 generate a context that derives from DbContext together with simple POCO entity classes.</span></span> <span data-ttu-id="d459c-114">V rámci sady Visual Studio 2010 doporučujeme odkládací šablonu pro generování kódu, která používá DbContext, jak je popsáno dále v tomto návodu.</span><span class="sxs-lookup"><span data-stu-id="d459c-114">With Visual Studio 2010 we recommend swapping to a code generation template that uses DbContext as described later in this walkthrough.</span></span>

<span data-ttu-id="d459c-115">Při použití plochy rozhraní API DbContext byste měli použít možnost **objektu** při vytváření nového zdroje dat, jak je znázorněno v tomto návodu.</span><span class="sxs-lookup"><span data-stu-id="d459c-115">When using the DbContext API surface you should use the **Object** option when creating a new Data Source, as shown in this walkthrough.</span></span>

<span data-ttu-id="d459c-116">V případě potřeby se můžete [vrátit na generování kódu založeného na ObjectContext](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) pro modely vytvořené pomocí návrháře EF.</span><span class="sxs-lookup"><span data-stu-id="d459c-116">If needed, you can [revert to ObjectContext based code generation](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) for models created with the EF Designer.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="d459c-117">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d459c-117">Pre-Requisites</span></span>

<span data-ttu-id="d459c-118">Pro dokončení tohoto Názorného postupu musíte mít nainstalovanou Visual Studio 2013, Visual Studio 2012 nebo Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="d459c-118">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="d459c-119">Pokud používáte Visual Studio 2010, je také nutné nainstalovat NuGet.</span><span class="sxs-lookup"><span data-stu-id="d459c-119">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="d459c-120">Další informace najdete v tématu [instalace NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).</span><span class="sxs-lookup"><span data-stu-id="d459c-120">For more information, see [Installing NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).</span></span>  

## <a name="create-the-application"></a><span data-ttu-id="d459c-121">Vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="d459c-121">Create the Application</span></span>

-   <span data-ttu-id="d459c-122">Otevřete sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d459c-122">Open Visual Studio</span></span>
-   <span data-ttu-id="d459c-123">**Soubor-&gt; projekt New-&gt;...**</span><span class="sxs-lookup"><span data-stu-id="d459c-123">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="d459c-124">V levém podokně vyberte **Windows** a **WPFApplication** v pravém podokně.</span><span class="sxs-lookup"><span data-stu-id="d459c-124">Select **Windows** in the left pane and **WPFApplication** in the right pane</span></span>
-   <span data-ttu-id="d459c-125">Jako název zadejte **WPFwithEFSample** </span><span class="sxs-lookup"><span data-stu-id="d459c-125">Enter **WPFwithEFSample** as the name</span></span>
-   <span data-ttu-id="d459c-126">Vybrat **OK**</span><span class="sxs-lookup"><span data-stu-id="d459c-126">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="d459c-127">Instalace balíčku Entity Framework NuGet</span><span class="sxs-lookup"><span data-stu-id="d459c-127">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="d459c-128">V Průzkumník řešení klikněte pravým tlačítkem na projekt **WinFormswithEFSample** .</span><span class="sxs-lookup"><span data-stu-id="d459c-128">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="d459c-129">Vyberte **Spravovat balíčky NuGet...**</span><span class="sxs-lookup"><span data-stu-id="d459c-129">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="d459c-130">V dialogovém okně Spravovat balíčky NuGet vyberte kartu **online** a zvolte balíček **EntityFramework** .</span><span class="sxs-lookup"><span data-stu-id="d459c-130">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="d459c-131">Klikněte na **nainstalovat** .</span><span class="sxs-lookup"><span data-stu-id="d459c-131">Click **Install**</span></span>  
    >[!NOTE]
    > <span data-ttu-id="d459c-132">Kromě sestavení EntityFramework je také přidáno odkaz na System. ComponentModel. DataAnnotations.</span><span class="sxs-lookup"><span data-stu-id="d459c-132">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="d459c-133">Pokud má projekt odkaz na System. data. entity, bude po instalaci balíčku EntityFramework odebrán.</span><span class="sxs-lookup"><span data-stu-id="d459c-133">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="d459c-134">Sestavení System. data. entity již není používáno pro Entity Framework 6 aplikací.</span><span class="sxs-lookup"><span data-stu-id="d459c-134">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="define-a-model"></a><span data-ttu-id="d459c-135">Definování modelu</span><span class="sxs-lookup"><span data-stu-id="d459c-135">Define a Model</span></span>

<span data-ttu-id="d459c-136">V tomto návodu můžete zvolit implementaci modelu pomocí Code First nebo návrháře EF.</span><span class="sxs-lookup"><span data-stu-id="d459c-136">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="d459c-137">Proveďte jednu ze dvou následujících částí.</span><span class="sxs-lookup"><span data-stu-id="d459c-137">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="d459c-138">Možnost 1: definování modelu pomocí Code First</span><span class="sxs-lookup"><span data-stu-id="d459c-138">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="d459c-139">V této části se dozvíte, jak vytvořit model a jeho přidruženou databázi pomocí Code First.</span><span class="sxs-lookup"><span data-stu-id="d459c-139">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="d459c-140">Přejděte k další části (**možnost 2: definice modelu pomocí Database First)** , pokud byste chtěli použít Database First k zpětné analýze modelu z databáze pomocí návrháře EF.</span><span class="sxs-lookup"><span data-stu-id="d459c-140">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="d459c-141">Při použití vývoje Code First obvykle začněte psaním .NET Framework tříd, které definují koncepční (doménový) model.</span><span class="sxs-lookup"><span data-stu-id="d459c-141">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="d459c-142">Přidat novou třídu do **WPFwithEFSample:**</span><span class="sxs-lookup"><span data-stu-id="d459c-142">Add a new class to the **WPFwithEFSample:**</span></span>
    -   <span data-ttu-id="d459c-143">Klikněte pravým tlačítkem myši na název projektu.</span><span class="sxs-lookup"><span data-stu-id="d459c-143">Right-click on the project name</span></span>
    -   <span data-ttu-id="d459c-144">Výběr položky **Přidat**a **Nová položka**</span><span class="sxs-lookup"><span data-stu-id="d459c-144">Select **Add**, then **New Item**</span></span>
    -   <span data-ttu-id="d459c-145">Vyberte **třídu** a jako název třídy zadejte  **produktu** .</span><span class="sxs-lookup"><span data-stu-id="d459c-145">Select **Class** and enter **Product** for the class name</span></span>
-   <span data-ttu-id="d459c-146">Nahraďte definici třídy  **produktu** následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="d459c-146">Replace the **Product** class definition with the following code:</span></span>

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

<span data-ttu-id="d459c-147">Vlastnost **Products** **u vlastnosti Category třídy a** **kategorie** u třídy **produkt** je vlastností navigace.</span><span class="sxs-lookup"><span data-stu-id="d459c-147">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="d459c-148">V Entity Framework navigační vlastnosti poskytují způsob, jak procházet relaci mezi dvěma typy entit.</span><span class="sxs-lookup"><span data-stu-id="d459c-148">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="d459c-149">Kromě definování entit musíte definovat třídu, která je odvozena z DbContext a zpřístupňuje Negenerickými&lt;vlastnosti TEntity&gt;.</span><span class="sxs-lookup"><span data-stu-id="d459c-149">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="d459c-150">Vlastnosti Negenerickými&lt;TEntity&gt; umožňují kontextu zjistit, které typy chcete do modelu zahrnout.</span><span class="sxs-lookup"><span data-stu-id="d459c-150">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="d459c-151">Instance DbContext odvozeného typu spravuje objekty entit za běhu, což zahrnuje vyplnění objektů daty z databáze, sledování změn a uchování dat do databáze.</span><span class="sxs-lookup"><span data-stu-id="d459c-151">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="d459c-152">Přidejte do projektu novou třídu **ProductContext** s následující definicí:</span><span class="sxs-lookup"><span data-stu-id="d459c-152">Add a new **ProductContext** class to the project with the following definition:</span></span>

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

<span data-ttu-id="d459c-153">Zkompilujte projekt.</span><span class="sxs-lookup"><span data-stu-id="d459c-153">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="d459c-154">Možnost 2: definování modelu pomocí Database First</span><span class="sxs-lookup"><span data-stu-id="d459c-154">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="d459c-155">V této části se dozvíte, jak použít Database First k zpětné analýze modelu z databáze pomocí návrháře EF.</span><span class="sxs-lookup"><span data-stu-id="d459c-155">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="d459c-156">Pokud jste dokončili předchozí oddíl (**možnost 1: Definujte model pomocí Code First)** , pak tuto část přeskočte a přejděte rovnou k oddílu **opožděné načítání** .</span><span class="sxs-lookup"><span data-stu-id="d459c-156">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="d459c-157">Vytvoření existující databáze</span><span class="sxs-lookup"><span data-stu-id="d459c-157">Create an Existing Database</span></span>

<span data-ttu-id="d459c-158">Když cílíte na existující databázi, bude už vytvořená, ale pro tento návod musíme pro přístup vytvořit databázi.</span><span class="sxs-lookup"><span data-stu-id="d459c-158">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="d459c-159">Databázový server, který je nainstalovaný se sadou Visual Studio, se liší v závislosti na verzi sady Visual Studio, kterou jste nainstalovali:</span><span class="sxs-lookup"><span data-stu-id="d459c-159">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="d459c-160">Pokud používáte Visual Studio 2010, budete vytvářet databázi SQL Express.</span><span class="sxs-lookup"><span data-stu-id="d459c-160">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="d459c-161">Pokud používáte Visual Studio 2012, budete vytvářet databázi [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) .</span><span class="sxs-lookup"><span data-stu-id="d459c-161">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="d459c-162">Pojďme dopředu a vygenerovat databázi.</span><span class="sxs-lookup"><span data-stu-id="d459c-162">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="d459c-163">**Zobrazení-&gt; Průzkumník serveru**</span><span class="sxs-lookup"><span data-stu-id="d459c-163">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="d459c-164">Klikněte pravým tlačítkem na **datová připojení –&gt; přidat připojení...**</span><span class="sxs-lookup"><span data-stu-id="d459c-164">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="d459c-165">Pokud jste se k databázi nepřipojili z Průzkumník serveru před tím, než bude nutné vybrat Microsoft SQL Server jako zdroj dat</span><span class="sxs-lookup"><span data-stu-id="d459c-165">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Změnit zdroj dat](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="d459c-167">Připojte se buď k LocalDB nebo SQL Express v závislosti na tom, který z nich jste nainstalovali, a jako název databáze zadejte **produkty** .</span><span class="sxs-lookup"><span data-stu-id="d459c-167">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![Přidat LocalDB připojení](~/ef6/media/addconnectionlocaldb.png)

    ![Přidat Express připojení](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="d459c-170">Vyberte **OK** a zobrazí se dotaz, jestli chcete vytvořit novou databázi, a pak vyberte **Ano** .</span><span class="sxs-lookup"><span data-stu-id="d459c-170">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Create Database](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="d459c-172">Nová databáze se nyní zobrazí v Průzkumník serveru, klikněte na ni pravým tlačítkem myši a vyberte **Nový dotaz** .</span><span class="sxs-lookup"><span data-stu-id="d459c-172">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="d459c-173">Zkopírujte následující příkaz SQL do nového dotazu, klikněte na něj pravým tlačítkem myši a vyberte **Spustit** .</span><span class="sxs-lookup"><span data-stu-id="d459c-173">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

#### <a name="reverse-engineer-model"></a><span data-ttu-id="d459c-174">Model zpětného analýz</span><span class="sxs-lookup"><span data-stu-id="d459c-174">Reverse Engineer Model</span></span>

<span data-ttu-id="d459c-175">Budeme používat Entity Framework Designer, který je součástí sady Visual Studio, a vytvořit náš model.</span><span class="sxs-lookup"><span data-stu-id="d459c-175">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="d459c-176">**Projekt –&gt; přidat novou položku...**</span><span class="sxs-lookup"><span data-stu-id="d459c-176">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="d459c-177">V nabídce vlevo vyberte **data** a pak **ADO.NET model EDM (Entity Data Model)**</span><span class="sxs-lookup"><span data-stu-id="d459c-177">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="d459c-178">Jako název zadejte **ProductModel** a klikněte na **OK** .</span><span class="sxs-lookup"><span data-stu-id="d459c-178">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="d459c-179">Spustí se **průvodce model EDM (Entity Data Model)** .</span><span class="sxs-lookup"><span data-stu-id="d459c-179">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="d459c-180">Vyberte **Generovat z databáze** a klikněte na **Další** .</span><span class="sxs-lookup"><span data-stu-id="d459c-180">Select **Generate from Database** and click **Next**</span></span>

    ![Zvolit obsah modelu](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="d459c-182">Vyberte připojení k databázi, kterou jste vytvořili v první části, jako název připojovacího řetězce zadejte **ProductContext** a klikněte na **Další** .</span><span class="sxs-lookup"><span data-stu-id="d459c-182">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![Zvolit připojení](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="d459c-184">Kliknutím na zaškrtávací políčko vedle tabulky naimportujete všechny tabulky a kliknete na Dokončit.</span><span class="sxs-lookup"><span data-stu-id="d459c-184">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Zvolit vaše objekty](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="d459c-186">Po dokončení procesu zpětného zpracování se do projektu přidá nový model, který jste otevřeli, abyste mohli zobrazit Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="d459c-186">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="d459c-187">Do projektu byl také přidán soubor App. config s podrobnostmi o připojení pro databázi.</span><span class="sxs-lookup"><span data-stu-id="d459c-187">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="d459c-188">Další kroky v aplikaci Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="d459c-188">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="d459c-189">Pokud pracujete v aplikaci Visual Studio 2010, bude nutné aktualizovat návrháře EF, aby používal generování kódu EF6.</span><span class="sxs-lookup"><span data-stu-id="d459c-189">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="d459c-190">V Návrháři EF klikněte pravým tlačítkem na prázdný bod modelu a vyberte **Přidat položku pro generování kódu...**</span><span class="sxs-lookup"><span data-stu-id="d459c-190">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="d459c-191">V nabídce vlevo vyberte **online šablony** a vyhledejte **DbContext** .</span><span class="sxs-lookup"><span data-stu-id="d459c-191">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="d459c-192">Vyberte **generátor EF 6. x DbContext pro jazyk C\#,** jako název zadejte **ProductsModel** a klikněte na Přidat.</span><span class="sxs-lookup"><span data-stu-id="d459c-192">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="d459c-193">Aktualizuje se generování kódu pro datovou vazbu.</span><span class="sxs-lookup"><span data-stu-id="d459c-193">Updating code generation for data binding</span></span>

<span data-ttu-id="d459c-194">EF generuje kód z modelu pomocí šablon T4.</span><span class="sxs-lookup"><span data-stu-id="d459c-194">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="d459c-195">Šablony dodávané se sadou Visual Studio nebo stažené z galerie sady Visual Studio jsou určené pro účely obecného použití.</span><span class="sxs-lookup"><span data-stu-id="d459c-195">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="d459c-196">To znamená, že entity vygenerované z těchto šablon mají jednoduché vlastnosti rozhraní ICollection&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="d459c-196">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="d459c-197">Při vytváření datových vazeb pomocí WPF je však žádoucí použít **kolekci ObservableCollection** pro vlastnosti kolekce, aby WPF mohl sledovat změny provedené v kolekcích.</span><span class="sxs-lookup"><span data-stu-id="d459c-197">However, when doing data binding using WPF it is desirable to use **ObservableCollection** for collection properties so that WPF can keep track of changes made to the collections.</span></span> <span data-ttu-id="d459c-198">K tomuto účelu budeme upravovat šablony, aby používaly kolekci ObservableCollection.</span><span class="sxs-lookup"><span data-stu-id="d459c-198">To this end we will to modify the templates to use ObservableCollection.</span></span>

-   <span data-ttu-id="d459c-199">Otevřete **Průzkumník řešení** a vyhledejte soubor **ProductModel. edmx.**</span><span class="sxs-lookup"><span data-stu-id="d459c-199">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="d459c-200">Vyhledejte soubor **ProductModel.TT** , který bude vnořen do souboru ProductModel. edmx.</span><span class="sxs-lookup"><span data-stu-id="d459c-200">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![Šablona modelu produktu WPF](~/ef6/media/wpfproductmodeltemplate.png)

-   <span data-ttu-id="d459c-202">Poklikejte na soubor ProductModel.tt a otevře se v editoru Visual studia.</span><span class="sxs-lookup"><span data-stu-id="d459c-202">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="d459c-203">Vyhledejte a nahraďte dva výskyty "**ICollection**" pomocí "**kolekci ObservableCollection**".</span><span class="sxs-lookup"><span data-stu-id="d459c-203">Find and replace the two occurrences of “**ICollection**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="d459c-204">Ty se nacházejí přibližně na řádcích 296 a 484.</span><span class="sxs-lookup"><span data-stu-id="d459c-204">These are located approximately at lines 296 and 484.</span></span>
-   <span data-ttu-id="d459c-205">Najde první výskyt "**HashSet –** " a nahradí ho "**kolekci ObservableCollection**".</span><span class="sxs-lookup"><span data-stu-id="d459c-205">Find and replace the first occurrence of “**HashSet**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="d459c-206">Tento výskyt je umístěný přibližně na řádku 50.</span><span class="sxs-lookup"><span data-stu-id="d459c-206">This occurrence is located approximately at line 50.</span></span> <span data-ttu-id="d459c-207">**Neměňte druhý** výskyt HashSet –, který byl nalezen později v kódu.</span><span class="sxs-lookup"><span data-stu-id="d459c-207">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="d459c-208">Vyhledejte a nahraďte pouze výskyt "**System. Collections. Generic**" pomocí "**System. Collections. ObjectModel**".</span><span class="sxs-lookup"><span data-stu-id="d459c-208">Find and replace the only occurrence of “**System.Collections.Generic**” with “**System.Collections.ObjectModel**”.</span></span> <span data-ttu-id="d459c-209">To se nachází přibližně na řádku 424.</span><span class="sxs-lookup"><span data-stu-id="d459c-209">This is located approximately at line 424.</span></span>
-   <span data-ttu-id="d459c-210">Uložte soubor ProductModel.tt.</span><span class="sxs-lookup"><span data-stu-id="d459c-210">Save the ProductModel.tt file.</span></span> <span data-ttu-id="d459c-211">To by mělo způsobit opětovné vygenerování kódu pro entity.</span><span class="sxs-lookup"><span data-stu-id="d459c-211">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="d459c-212">Pokud se kód znovu negeneruje automaticky, klikněte pravým tlačítkem na ProductModel.tt a zvolte spustit vlastní nástroj.</span><span class="sxs-lookup"><span data-stu-id="d459c-212">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="d459c-213">Pokud teď otevřete soubor Category.cs (který je vnořený pod ProductModel.tt), měli byste vidět, že kolekce Products má typ **kolekci observablecollection&lt;&gt;produktu** .</span><span class="sxs-lookup"><span data-stu-id="d459c-213">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableCollection&lt;Product&gt;**.</span></span>

<span data-ttu-id="d459c-214">Zkompilujte projekt.</span><span class="sxs-lookup"><span data-stu-id="d459c-214">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="d459c-215">Opožděné načítání</span><span class="sxs-lookup"><span data-stu-id="d459c-215">Lazy Loading</span></span>

<span data-ttu-id="d459c-216">Vlastnost **Products** **u vlastnosti Category třídy a** **kategorie** u třídy **produkt** je vlastností navigace.</span><span class="sxs-lookup"><span data-stu-id="d459c-216">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="d459c-217">V Entity Framework navigační vlastnosti poskytují způsob, jak procházet relaci mezi dvěma typy entit.</span><span class="sxs-lookup"><span data-stu-id="d459c-217">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="d459c-218">EF vám nabízí možnost načítat související entity z databáze automaticky při prvním přístupu k vlastnosti navigace.</span><span class="sxs-lookup"><span data-stu-id="d459c-218">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="d459c-219">U tohoto typu načítání (tzv. opožděné načítání) mějte na paměti, že při prvním přístupu k jednotlivým vlastnostem navigace se v databázi spustí samostatný dotaz, pokud obsah ještě není v kontextu.</span><span class="sxs-lookup"><span data-stu-id="d459c-219">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="d459c-220">Při použití typů entit POCO nahrazuje EF opožděné načítání vytvořením instancí odvozených typů proxy během běhu a následným přepsáním virtuálních vlastností ve třídách, aby bylo možné přidat zavěšení zatížení.</span><span class="sxs-lookup"><span data-stu-id="d459c-220">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="d459c-221">Chcete-li získat opožděné načítání souvisejících objektů, je nutné deklarovat metody Get vlastnosti navigace jako **veřejné** a **virtuální** (**přepsatelné** v Visual Basic) a třídu nesmí být **zapečetěna** (**NotOverridable** v Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="d459c-221">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="d459c-222">Při použití Database First navigační vlastnosti se automaticky vypnuly virtuálním, aby bylo možné povolit opožděné načítání.</span><span class="sxs-lookup"><span data-stu-id="d459c-222">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="d459c-223">V části Code First jsme se rozhodli, aby navigační vlastnosti byly ve stejném důsledku virtuální.</span><span class="sxs-lookup"><span data-stu-id="d459c-223">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="d459c-224">Svázání objektu s ovládacími prvky</span><span class="sxs-lookup"><span data-stu-id="d459c-224">Bind Object to Controls</span></span>

<span data-ttu-id="d459c-225">Přidejte třídy, které jsou definovány v modelu jako zdroje dat pro tuto aplikaci WPF.</span><span class="sxs-lookup"><span data-stu-id="d459c-225">Add the classes that are defined in the model as data sources for this WPF application.</span></span>

-   <span data-ttu-id="d459c-226">Poklikejte na **MainWindow. XAML** v Průzkumník řešení k otevření hlavního formuláře.</span><span class="sxs-lookup"><span data-stu-id="d459c-226">Double-click **MainWindow.xaml** in Solution Explorer to open the main form</span></span>
-   <span data-ttu-id="d459c-227">V hlavní nabídce vyberte **projekt –&gt; přidat nový zdroj dat...**</span><span class="sxs-lookup"><span data-stu-id="d459c-227">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="d459c-228">(v aplikaci Visual Studio 2010 je nutné vybrat **data –&gt; přidat nový zdroj dat...** )</span><span class="sxs-lookup"><span data-stu-id="d459c-228">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="d459c-229">Ve formuláři zvolte zdroj dat Typewindow vyberte **objekt** a klikněte na **Další** .</span><span class="sxs-lookup"><span data-stu-id="d459c-229">In the Choose a Data Source Typewindow, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="d459c-230">V dialogovém okně Vybrat datové objekty odložte **WPFwithEFSample** dvakrát a vyberte **kategorii** .</span><span class="sxs-lookup"><span data-stu-id="d459c-230">In the Select the Data Objects dialog, unfold the **WPFwithEFSample** two times and select **Category**</span></span>  
    <span data-ttu-id="d459c-231">*Nemusíte vybírat zdroj dat **produktu** , protože se k němu dostanete prostřednictvím vlastnosti **produktu**ve zdroji dat **kategorie** .*</span><span class="sxs-lookup"><span data-stu-id="d459c-231">*There is no need to select the **Product** data source, because we will get to it through the **Product**’s property on the **Category** data source*</span></span>  

    ![Vybrat datové objekty](~/ef6/media/selectdataobjects.png)

-   <span data-ttu-id="d459c-233">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="d459c-233">Click **Finish.**</span></span>
-   <span data-ttu-id="d459c-234">Okno zdroje dat je otevřeno vedle okna MainWindow. XAML *, pokud se nezobrazí okno zdroje dat, vyberte možnost **zobrazit –&gt; jiné zdroje dat&gt; Windows***  .</span><span class="sxs-lookup"><span data-stu-id="d459c-234">The Data Sources window is opened next to the MainWindow.xaml window *If the Data Sources window is not showing up, select **View -&gt; Other Windows-&gt; Data Sources***</span></span>
-   <span data-ttu-id="d459c-235">Stiskněte ikonu připnutí, aby se okno zdroje dat neautomaticky skrylo.</span><span class="sxs-lookup"><span data-stu-id="d459c-235">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="d459c-236">Pokud je okno již viditelné, může být nutné spustit tlačítko Aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="d459c-236">You may need to hit the refresh button if the window was already visible.</span></span>

    ![Zdroje dat](~/ef6/media/datasources.png)

-   <span data-ttu-id="d459c-238">Vyberte zdroj dat **kategorie** a přetáhněte jej na formuláři.</span><span class="sxs-lookup"><span data-stu-id="d459c-238">Select the **Category** data source and drag it on the form.</span></span>

<span data-ttu-id="d459c-239">Při přetažení tohoto zdroje došlo k následujícímu:</span><span class="sxs-lookup"><span data-stu-id="d459c-239">The following happened when we dragged this source:</span></span>

-   <span data-ttu-id="d459c-240">Prostředek **categoryViewSource** a ovládací prvek **categoryDataGrid** byly přidány do jazyka XAML.</span><span class="sxs-lookup"><span data-stu-id="d459c-240">The **categoryViewSource** resource and the **categoryDataGrid** control were added to XAML</span></span> 
-   <span data-ttu-id="d459c-241">Vlastnost DataContext v nadřazeném elementu gridu se nastavila na {StaticResource **categoryViewSource** }.</span><span class="sxs-lookup"><span data-stu-id="d459c-241">The DataContext property on the parent Grid element was set to "{StaticResource **categoryViewSource** }".</span></span><span data-ttu-id="d459c-242"> Prostředek **categoryViewSource** slouží jako zdroj vazby pro vnější\\nadřazený prvek mřížky.</span><span class="sxs-lookup"><span data-stu-id="d459c-242"> The **categoryViewSource** resource serves as a binding source for the outer\\parent Grid element.</span></span> <span data-ttu-id="d459c-243">Vnitřní elementy mřížky pak zdědí hodnotu DataContext z nadřazené mřížky (vlastnost ItemsSource categoryDataGrid je nastavená na {Binding}).</span><span class="sxs-lookup"><span data-stu-id="d459c-243">The inner Grid elements then inherit the DataContext value from the parent Grid (the categoryDataGrid’s ItemsSource property is set to "{Binding}")</span></span>

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

## <a name="adding-a-details-grid"></a><span data-ttu-id="d459c-244">Přidání mřížky podrobností</span><span class="sxs-lookup"><span data-stu-id="d459c-244">Adding a Details Grid</span></span>

<span data-ttu-id="d459c-245">Teď, když máme mřížku pro zobrazení kategorií, přidáme k zobrazení přidružených produktů tabulku podrobností.</span><span class="sxs-lookup"><span data-stu-id="d459c-245">Now that we have a grid to display Categories let's add a details grid to display the associated Products.</span></span>

-   <span data-ttu-id="d459c-246">Vyberte vlastnost **Products** ze zdroje dat **kategorie** a přetáhněte ji do formuláře.</span><span class="sxs-lookup"><span data-stu-id="d459c-246">Select the **Products** property from under the **Category** data source and drag it on the form.</span></span>
    -   <span data-ttu-id="d459c-247">Prostředek **categoryProductsViewSource** a **productDataGrid** mřížka se přidají do XAML.</span><span class="sxs-lookup"><span data-stu-id="d459c-247">The **categoryProductsViewSource** resource and **productDataGrid** grid are added to XAML</span></span>
    -   <span data-ttu-id="d459c-248">Cesta vazby pro tento prostředek je nastavená na Products.</span><span class="sxs-lookup"><span data-stu-id="d459c-248">The binding path for this resource is set to Products</span></span>
    -   <span data-ttu-id="d459c-249">Architektura pro datovou vazbu WPF zajišťuje, aby se v **productDataGrid** zobrazovaly jenom produkty související s vybranou kategorií.</span><span class="sxs-lookup"><span data-stu-id="d459c-249">WPF data-binding framework ensures that only Products related to the selected Category show up in **productDataGrid**</span></span>
-   <span data-ttu-id="d459c-250">Přetáhněte **tlačítko** myši na panelu nástrojů do formuláře.</span><span class="sxs-lookup"><span data-stu-id="d459c-250">From the Toolbox, drag **Button** on to the form.</span></span> <span data-ttu-id="d459c-251">Nastavte vlastnost **Name** na **ButtonSave** a vlastnost **Content** , která se má **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="d459c-251">Set the **Name** property to **buttonSave** and the **Content** property to **Save**.</span></span>

<span data-ttu-id="d459c-252">Formulář by měl vypadat nějak takto:</span><span class="sxs-lookup"><span data-stu-id="d459c-252">The form should look similar to this:</span></span>

![Designer](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a><span data-ttu-id="d459c-254">Přidat kód, který zpracovává interakci s daty</span><span class="sxs-lookup"><span data-stu-id="d459c-254">Add Code that Handles Data Interaction</span></span>

<span data-ttu-id="d459c-255">Je čas přidat do hlavního okna nějaké obslužné rutiny událostí.</span><span class="sxs-lookup"><span data-stu-id="d459c-255">It's time to add some event handlers to the main window.</span></span>

-   <span data-ttu-id="d459c-256">V okně XAML klikněte na prvek **&lt;okno** . tím se vybere hlavní okno.</span><span class="sxs-lookup"><span data-stu-id="d459c-256">In the XAML window, click on the **&lt;Window** element, this selects the main window</span></span>
-   <span data-ttu-id="d459c-257">V okně **vlastnosti** zvolte **události** v pravém horním rohu a potom poklikejte na textové pole napravo od **načteného** popisku.</span><span class="sxs-lookup"><span data-stu-id="d459c-257">In the **Properties** window choose **Events** at the top right, then double-click the text box to right of the **Loaded** label</span></span>

    ![Vlastnosti hlavního okna](~/ef6/media/mainwindowproperties.png)

-   <span data-ttu-id="d459c-259">Přidejte také událost **Click** pro tlačítko **Uložit** dvojitým kliknutím na tlačítko Uložit v návrháři.</span><span class="sxs-lookup"><span data-stu-id="d459c-259">Also add the **Click** event for the **Save** button by double-clicking the Save button in the designer.</span></span> 

<span data-ttu-id="d459c-260">Tím se vám zobrazí kód na pozadí pro formulář. teď kód upravíte, aby se k přístupu k datům používal ProductContext.</span><span class="sxs-lookup"><span data-stu-id="d459c-260">This brings you to the code behind for the form, we'll now edit the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="d459c-261">Aktualizujte kód pro MainWindow, jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="d459c-261">Update the code for the MainWindow as shown below.</span></span>

<span data-ttu-id="d459c-262">Kód deklaruje dlouhodobě běžící instanci **ProductContext**.</span><span class="sxs-lookup"><span data-stu-id="d459c-262">The code declares a long-running instance of **ProductContext**.</span></span> <span data-ttu-id="d459c-263">Objekt **ProductContext** se používá k dotazování a ukládání dat do databáze.</span><span class="sxs-lookup"><span data-stu-id="d459c-263">The **ProductContext** object is used to query and save data to the database.</span></span> <span data-ttu-id="d459c-264">Metoda **Dispose ()** v instanci **ProductContext** je pak volána z přepsané metody při **zavírání** .</span><span class="sxs-lookup"><span data-stu-id="d459c-264">The **Dispose()** on the **ProductContext** instance is then called from the overridden **OnClosing** method.</span></span><span data-ttu-id="d459c-265"> Komentáře kódu poskytují podrobné informace o tom, co kód dělá.</span><span class="sxs-lookup"><span data-stu-id="d459c-265"> The code comments provide details about what the code does.</span></span>

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

## <a name="test-the-wpf-application"></a><span data-ttu-id="d459c-266">Testování aplikace WPF</span><span class="sxs-lookup"><span data-stu-id="d459c-266">Test the WPF Application</span></span>

-   <span data-ttu-id="d459c-267">Zkompilujte a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d459c-267">Compile and run the application.</span></span> <span data-ttu-id="d459c-268">Pokud jste použili Code First, zobrazí se vám pro vás vytvořená databáze **WPFwithEFSample. ProductContext** .</span><span class="sxs-lookup"><span data-stu-id="d459c-268">If you used Code First, then you will see that a **WPFwithEFSample.ProductContext** database is created for you.</span></span>
-   <span data-ttu-id="d459c-269">Zadejte název kategorie v horní mřížce a názvy produktů v dolním mřížce *nezadávají žádné údaje ve SLOUPCÍCH ID, protože primární klíč je vygenerovaný databází* .</span><span class="sxs-lookup"><span data-stu-id="d459c-269">Enter a category name in the top grid and product names in the bottom grid *Do not enter anything in ID columns, because the primary key is generated by the database*</span></span>

    ![Hlavní okno s novými kategoriemi a produkty](~/ef6/media/screen1.png)

-   <span data-ttu-id="d459c-271">Kliknutím na tlačítko **Uložit** uložte data do databáze.</span><span class="sxs-lookup"><span data-stu-id="d459c-271">Press the **Save** button to save the data to the database</span></span>

<span data-ttu-id="d459c-272">Po volání **metody SaveChanges ()** pro DbContext se identifikátory naplní hodnotami generovanými databází.</span><span class="sxs-lookup"><span data-stu-id="d459c-272">After the call to DbContext’s **SaveChanges()**, the IDs are populated with the database generated values.</span></span> <span data-ttu-id="d459c-273">Protože jsme volali **Refresh ()** po **metody SaveChanges ()** , ovládací prvky **DataGrid** jsou aktualizovány také novými hodnotami.</span><span class="sxs-lookup"><span data-stu-id="d459c-273">Because we called **Refresh()** after **SaveChanges()** the **DataGrid** controls are updated with the new values as well.</span></span>

![Hlavní okno s vyplněnými ID](~/ef6/media/screen2.png)

## <a name="additional-resources"></a><span data-ttu-id="d459c-275">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="d459c-275">Additional Resources</span></span>

<span data-ttu-id="d459c-276">Další informace o datové vazbě na kolekce pomocí WPF naleznete v [tomto tématu](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) v dokumentaci k platformě WPF.</span><span class="sxs-lookup"><span data-stu-id="d459c-276">To learn more about data binding to collections using WPF, see [this topic](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) in the WPF documentation.</span></span>  
