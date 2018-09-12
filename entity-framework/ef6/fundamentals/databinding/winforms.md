---
title: Vazby dat s WinForms - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 80fc5062-2f1c-4dbd-ab6e-b99496784b36
ms.openlocfilehash: 48e6d997875a25a5954484f854953df69a267d05
ms.sourcegitcommit: 8d04a2ad98036f32ca70c77ce3040c5edb1cdf82
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/11/2018
ms.locfileid: "44384849"
---
# <a name="databinding-with-winforms"></a><span data-ttu-id="09e6c-102">Vazby dat s WinForms</span><span class="sxs-lookup"><span data-stu-id="09e6c-102">Databinding with WinForms</span></span>
<span data-ttu-id="09e6c-103">Tento podrobný návod ukazuje, jak svázat ovládací prvky formuláře Windows (WinForms) ve formě "hlavní podrobnosti" typy POCO.</span><span class="sxs-lookup"><span data-stu-id="09e6c-103">This step-by-step walkthrough shows how to bind POCO types to Window Forms (WinForms) controls in a “master-detail" form.</span></span> <span data-ttu-id="09e6c-104">Aplikace používá Entity Framework pro naplnění objekty s daty z databáze, sledování změn a uložení dat do databáze.</span><span class="sxs-lookup"><span data-stu-id="09e6c-104">The application uses Entity Framework to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="09e6c-105">Model definuje dva typy, které se účastní vztahu jednoho k několika: kategorie (hlavní\\hlavní) a produktu (závislé\\podrobnosti).</span><span class="sxs-lookup"><span data-stu-id="09e6c-105">The model defines two types that participate in one-to-many relationship: Category (principal\\master) and Product (dependent\\detail).</span></span> <span data-ttu-id="09e6c-106">Nástroje sady Visual Studio se použije k vytvoření vazby typy definované v modelu, který má ovládacích prvků WinForms.</span><span class="sxs-lookup"><span data-stu-id="09e6c-106">Then, the Visual Studio tools are used to bind the types defined in the model to the WinForms controls.</span></span> <span data-ttu-id="09e6c-107">Datová vazba rozhraní WinForms umožňuje navigaci mezi související objekty: výběr řádků v zobrazení předlohy způsobí, že podrobné zobrazení aktualizace pomocí odpovídající podřízená data.</span><span class="sxs-lookup"><span data-stu-id="09e6c-107">The WinForms data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="09e6c-108">Snímky obrazovky a výpis kódu v tomto názorném postupu pocházejí ze sady Visual Studio 2013, ale můžete dokončit tento návod s Visual Studio 2012 nebo Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="09e6c-108">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="09e6c-109">Předpoklady</span><span class="sxs-lookup"><span data-stu-id="09e6c-109">Pre-Requisites</span></span>

<span data-ttu-id="09e6c-110">Musíte mít Visual Studio 2013, Visual Studio 2012 nebo Visual Studio 2010 nainstalovaný k dokončení tohoto návodu.</span><span class="sxs-lookup"><span data-stu-id="09e6c-110">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="09e6c-111">Pokud používáte Visual Studio 2010, musíte také nainstalovat NuGet.</span><span class="sxs-lookup"><span data-stu-id="09e6c-111">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="09e6c-112">Další informace najdete v tématu [instalace balíčků NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span><span class="sxs-lookup"><span data-stu-id="09e6c-112">For more information, see [Installing NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span></span>

## <a name="create-the-application"></a><span data-ttu-id="09e6c-113">Vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="09e6c-113">Create the Application</span></span>

-   <span data-ttu-id="09e6c-114">Otevřít Visual Studio</span><span class="sxs-lookup"><span data-stu-id="09e6c-114">Open Visual Studio</span></span>
-   <span data-ttu-id="09e6c-115">**Soubor –&gt; nové –&gt; projektu...**</span><span class="sxs-lookup"><span data-stu-id="09e6c-115">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="09e6c-116">Vyberte **Windows** v levém podokně a **Windows FormsApplication** v pravém podokně</span><span class="sxs-lookup"><span data-stu-id="09e6c-116">Select **Windows** in the left pane and **Windows FormsApplication** in the right pane</span></span>
-   <span data-ttu-id="09e6c-117">Zadejte **WinFormswithEFSample** jako název</span><span class="sxs-lookup"><span data-stu-id="09e6c-117">Enter **WinFormswithEFSample** as the name</span></span>
-   <span data-ttu-id="09e6c-118">Vyberte **OK**</span><span class="sxs-lookup"><span data-stu-id="09e6c-118">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="09e6c-119">Nainstalovat balíček NuGet pro rozhraní Entity Framework</span><span class="sxs-lookup"><span data-stu-id="09e6c-119">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="09e6c-120">V Průzkumníku řešení klikněte pravým tlačítkem myši na **WinFormswithEFSample** projektu</span><span class="sxs-lookup"><span data-stu-id="09e6c-120">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="09e6c-121">Vyberte **spravovat balíčky NuGet...**</span><span class="sxs-lookup"><span data-stu-id="09e6c-121">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="09e6c-122">V dialogovém okně Spravovat balíčky NuGet vyberte **Online** kartě a zvolte **EntityFramework** balíčku</span><span class="sxs-lookup"><span data-stu-id="09e6c-122">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="09e6c-123">Klikněte na tlačítko **instalace**</span><span class="sxs-lookup"><span data-stu-id="09e6c-123">Click **Install**</span></span>  
    > [!NOTE]
    > <span data-ttu-id="09e6c-124">Kromě EntityFramework sestavení je také přidán odkaz na System.ComponentModel.DataAnnotations.</span><span class="sxs-lookup"><span data-stu-id="09e6c-124">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="09e6c-125">Pokud projekt obsahuje odkaz na System.Data.Entity, pak se odebere při instalaci balíčku objektu EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="09e6c-125">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="09e6c-126">Sestavení System.Data.Entity se už používá pro aplikace Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="09e6c-126">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="implementing-ilistsource-for-collections"></a><span data-ttu-id="09e6c-127">Implementace rozhraní IListSource pro kolekce</span><span class="sxs-lookup"><span data-stu-id="09e6c-127">Implementing IListSource for Collections</span></span>

<span data-ttu-id="09e6c-128">Vlastnosti kolekce musí implementovat rozhraní IListSource obousměrný datové vazby s řazení při používání formulářů Windows.</span><span class="sxs-lookup"><span data-stu-id="09e6c-128">Collection properties must implement the IListSource interface to enable two-way data binding with sorting when using Windows Forms.</span></span> <span data-ttu-id="09e6c-129">K tomu budeme rozšiřovat observablecollection – přidání funkce rozhraní IListSource.</span><span class="sxs-lookup"><span data-stu-id="09e6c-129">To do this we are going to extend ObservableCollection to add IListSource functionality.</span></span>

-   <span data-ttu-id="09e6c-130">Přidat **ObservableListSource** třídy do projektu:</span><span class="sxs-lookup"><span data-stu-id="09e6c-130">Add an **ObservableListSource** class to the project:</span></span>
    -   <span data-ttu-id="09e6c-131">Klikněte pravým tlačítkem na název projektu</span><span class="sxs-lookup"><span data-stu-id="09e6c-131">Right-click on the project name</span></span>
    -   <span data-ttu-id="09e6c-132">Vyberte **Add -&gt; nová položka**</span><span class="sxs-lookup"><span data-stu-id="09e6c-132">Select **Add -&gt; New Item**</span></span>
    -   <span data-ttu-id="09e6c-133">Vyberte **třídy** a zadejte **ObservableListSource** pro název třídy</span><span class="sxs-lookup"><span data-stu-id="09e6c-133">Select **Class** and enter **ObservableListSource** for the class name</span></span>
-   <span data-ttu-id="09e6c-134">Nahraďte kód vygenerovaný ve výchozím nastavení s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="09e6c-134">Replace the code generated by default with the following code:</span></span>

<span data-ttu-id="09e6c-135">*Tato třída umožňuje obousměrný datové vazby, stejně jako řazení. Třída je odvozena z kolekci ObservableCollection&lt;T&gt; a přidá explicitní implementace rozhraní IListSource. Metoda GetList() IListSource implementované tak, aby vrátila implementace rozhraní IBindingList, které zůstávají synchronizované s kolekci ObservableCollection. Implementace IBindingList generovaných ToBindingList podporuje řazení. Metoda rozšíření ToBindingList je definována v sestavení objektu EntityFramework.*</span><span class="sxs-lookup"><span data-stu-id="09e6c-135">*This class enables two-way data binding as well as sorting. The class derives from ObservableCollection&lt;T&gt; and adds an explicit implementation of IListSource. The GetList() method of IListSource is implemented to return an IBindingList implementation that stays in sync with the ObservableCollection. The IBindingList implementation generated by ToBindingList supports sorting. The ToBindingList extention method is defined in the EntityFramework assembly.*</span></span>

``` csharp
    using System.Collections;
    using System.Collections.Generic;
    using System.Collections.ObjectModel;
    using System.ComponentModel;
    using System.Diagnostics.CodeAnalysis;
    using System.Data.Entity;

    namespace WinFormswithEFSample
    {
        public class ObservableListSource<T> : ObservableCollection<T>, IListSource
            where T : class
        {
            private IBindingList _bindingList;

            bool IListSource.ContainsListCollection { get { return false; } }

            IList IListSource.GetList()
            {
                return _bindingList ?? (_bindingList = this.ToBindingList());
            }
        }
    }
```

## <a name="define-a-model"></a><span data-ttu-id="09e6c-136">Definovat Model</span><span class="sxs-lookup"><span data-stu-id="09e6c-136">Define a Model</span></span>

<span data-ttu-id="09e6c-137">V tomto návodu jste se rozhodli implementují model Code First nebo EF designeru.</span><span class="sxs-lookup"><span data-stu-id="09e6c-137">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="09e6c-138">Proveďte jeden z následujících dvou částech.</span><span class="sxs-lookup"><span data-stu-id="09e6c-138">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="09e6c-139">Možnost 1: Definujte Model pomocí Code First</span><span class="sxs-lookup"><span data-stu-id="09e6c-139">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="09e6c-140">Tato část ukazuje, jak vytvořit model a jeho přidružená databáze pomocí Code First.</span><span class="sxs-lookup"><span data-stu-id="09e6c-140">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="09e6c-141">Přejít k další části (**možnost 2: definovat model pomocí Database First)** Pokud byste chtěli raději použít první databázi vrátit pracovníkovi modelu z databáze pomocí EF designeru</span><span class="sxs-lookup"><span data-stu-id="09e6c-141">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="09e6c-142">Při použití vývoje Code First obvykle začnete vytvořením tříd rozhraní .NET Framework, které definují model koncepční (domény).</span><span class="sxs-lookup"><span data-stu-id="09e6c-142">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="09e6c-143">Přidat nový **produktu** třídy do projektu</span><span class="sxs-lookup"><span data-stu-id="09e6c-143">Add a new **Product** class to project</span></span>
-   <span data-ttu-id="09e6c-144">Nahraďte kód vygenerovaný ve výchozím nastavení s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="09e6c-144">Replace the code generated by default with the following code:</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

    namespace WinFormswithEFSample
    {
        public class Product
        {
            public int ProductId { get; set; }
            public string Name { get; set; }

            public int CategoryId { get; set; }
            public virtual Category Category { get; set; }
        }
    }
```

-   <span data-ttu-id="09e6c-145">Přidat **kategorie** třídy do projektu.</span><span class="sxs-lookup"><span data-stu-id="09e6c-145">Add a **Category** class to the project.</span></span>
-   <span data-ttu-id="09e6c-146">Nahraďte kód vygenerovaný ve výchozím nastavení s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="09e6c-146">Replace the code generated by default with the following code:</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

    namespace WinFormswithEFSample
    {
        public class Category
        {
            private readonly ObservableListSource<Product> _products =
                    new ObservableListSource<Product>();

            public int CategoryId { get; set; }
            public string Name { get; set; }
            public virtual ObservableListSource<Product> Products { get { return _products; } }
        }
    }
```

<span data-ttu-id="09e6c-147">Kromě definování entit, budete muset definovat třídu, která je odvozena z **DbContext** a zveřejňuje **DbSet&lt;TEntity&gt;**  vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="09e6c-147">In addition to defining entities, you need to define a class that derives from **DbContext** and exposes **DbSet&lt;TEntity&gt;** properties.</span></span> <span data-ttu-id="09e6c-148">**DbSet** vlastnosti umožňují kontextu vědět, jaké typy, které chcete zahrnout do modelu.</span><span class="sxs-lookup"><span data-stu-id="09e6c-148">The **DbSet** properties let the context know which types you want to include in the model.</span></span> <span data-ttu-id="09e6c-149">**DbContext** a **DbSet** typy jsou definovány v sestavení objektu EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="09e6c-149">The **DbContext** and **DbSet** types are defined in the EntityFramework assembly.</span></span>

<span data-ttu-id="09e6c-150">Instance typu DbContext odvozené spravuje objekty entity za běhu, který obsahuje naplnění objekty s daty z databáze, změňte sledování a zachovává data do databáze.</span><span class="sxs-lookup"><span data-stu-id="09e6c-150">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="09e6c-151">Přidat nový **ProductContext** třídy do projektu.</span><span class="sxs-lookup"><span data-stu-id="09e6c-151">Add a new **ProductContext** class to the project.</span></span>
-   <span data-ttu-id="09e6c-152">Nahraďte kód vygenerovaný ve výchozím nastavení s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="09e6c-152">Replace the code generated by default with the following code:</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Linq;
    using System.Text;

    namespace WinFormswithEFSample
    {
        public class ProductContext : DbContext
        {
            public DbSet<Category> Categories { get; set; }
            public DbSet<Product> Products { get; set; }
        }
    }
```

<span data-ttu-id="09e6c-153">Zkompilujte projekt.</span><span class="sxs-lookup"><span data-stu-id="09e6c-153">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="09e6c-154">Možnost 2: Definujte model pomocí Database First</span><span class="sxs-lookup"><span data-stu-id="09e6c-154">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="09e6c-155">Tato část ukazuje způsob použití Database First provést zpětnou analýzu modelu z databáze pomocí EF designeru.</span><span class="sxs-lookup"><span data-stu-id="09e6c-155">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="09e6c-156">Pokud jste dokončili předchozí části (**možnost 1: definovat model pomocí Code First)**, tuto část přeskočit a přejít rovnou do **opožděné načtení** oddílu.</span><span class="sxs-lookup"><span data-stu-id="09e6c-156">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="09e6c-157">Vytvořit existující databáze</span><span class="sxs-lookup"><span data-stu-id="09e6c-157">Create an Existing Database</span></span>

<span data-ttu-id="09e6c-158">Obvykle Pokud cílíte na existující databázi, které se už vytvořili, ale v tomto návodu budeme potřebovat vytvořit databázi pro přístup k.</span><span class="sxs-lookup"><span data-stu-id="09e6c-158">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="09e6c-159">Databázový server, který se instaluje se sadou Visual Studio se liší v závislosti na verzi sady Visual Studio, které jste nainstalovali:</span><span class="sxs-lookup"><span data-stu-id="09e6c-159">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="09e6c-160">Pokud používáte Visual Studio 2010 vytvoříte databázi SQL Express.</span><span class="sxs-lookup"><span data-stu-id="09e6c-160">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="09e6c-161">Pokud používáte sadu Visual Studio 2012 pak vytvoříte [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) databáze.</span><span class="sxs-lookup"><span data-stu-id="09e6c-161">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="09e6c-162">Pojďme tedy vygenerovala databáze.</span><span class="sxs-lookup"><span data-stu-id="09e6c-162">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="09e6c-163">**Zobrazení –&gt; Průzkumníka serveru**</span><span class="sxs-lookup"><span data-stu-id="09e6c-163">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="09e6c-164">Klikněte pravým tlačítkem na **datová připojení -&gt; přidat připojení...**</span><span class="sxs-lookup"><span data-stu-id="09e6c-164">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="09e6c-165">Pokud jste ještě nepřipojili k databázi z Průzkumníka serveru předtím, než bude nutné vybrat jako zdroj dat Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="09e6c-165">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Změnit zdroj dat](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="09e6c-167">Připojení k LocalDB nebo SQL Express, v závislosti na tom, co jste nainstalovali a zadejte **produkty** jako název databáze</span><span class="sxs-lookup"><span data-stu-id="09e6c-167">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![Přidat připojení LocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![Přidat připojení Express](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="09e6c-170">Vyberte **OK** a zobrazí se výzva, pokud chcete vytvořit novou databázi, vyberte **Ano**</span><span class="sxs-lookup"><span data-stu-id="09e6c-170">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Vytvoření databáze](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="09e6c-172">Nové databáze se teď budou zobrazovat v Průzkumníku serveru, klikněte pravým tlačítkem myši na něj a vyberte **nový dotaz**</span><span class="sxs-lookup"><span data-stu-id="09e6c-172">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="09e6c-173">Zkopírujte následující příkaz SQL na nový dotaz a pak klikněte pravým tlačítkem myši na dotazu a vyberte **spouštění**</span><span class="sxs-lookup"><span data-stu-id="09e6c-173">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

#### <a name="reverse-engineer-model"></a><span data-ttu-id="09e6c-174">Zpětná analýza modelu</span><span class="sxs-lookup"><span data-stu-id="09e6c-174">Reverse Engineer Model</span></span>

<span data-ttu-id="09e6c-175">My budeme používat Entity Framework Designer, která je součástí sady Visual Studio, k vytvoření našeho modelu.</span><span class="sxs-lookup"><span data-stu-id="09e6c-175">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="09e6c-176">**Projekt –&gt; přidat novou položku...**</span><span class="sxs-lookup"><span data-stu-id="09e6c-176">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="09e6c-177">Vyberte **Data** v levé nabídce a potom **datový Model Entity ADO.NET**</span><span class="sxs-lookup"><span data-stu-id="09e6c-177">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="09e6c-178">Zadejte **ProductModel** jako název a klikněte na **OK**</span><span class="sxs-lookup"><span data-stu-id="09e6c-178">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="09e6c-179">Tím se spustí **Průvodce datovým modelem Entity**</span><span class="sxs-lookup"><span data-stu-id="09e6c-179">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="09e6c-180">Vyberte **Generovat z databáze** a klikněte na tlačítko **další**</span><span class="sxs-lookup"><span data-stu-id="09e6c-180">Select **Generate from Database** and click **Next**</span></span>

    ![ChooseModelContents](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="09e6c-182">Vyberte připojení k databázi vytvořené v první části, zadejte **ProductContext** jako název připojovacího řetězce a klikněte na tlačítko **další**</span><span class="sxs-lookup"><span data-stu-id="09e6c-182">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![Zvolte připojení](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="09e6c-184">Klikněte na zaškrtávací políčko vedle "Tables" k importu všech tabulek a klikněte na tlačítko 'Dokončit'</span><span class="sxs-lookup"><span data-stu-id="09e6c-184">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Vyberte objekty](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="09e6c-186">Po dokončení procesu zpětné analýzy nový model je přidána do projektu a zpřístupnili k zobrazení v Návrháři Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="09e6c-186">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="09e6c-187">Soubor App.config má také byla přidána do projektu s podrobnostmi o připojení pro databázi.</span><span class="sxs-lookup"><span data-stu-id="09e6c-187">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="09e6c-188">Další kroky v sadě Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="09e6c-188">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="09e6c-189">Pokud pracujete v sadě Visual Studio 2010 je potřeba aktualizovat EF designeru používat EF6 generování kódu.</span><span class="sxs-lookup"><span data-stu-id="09e6c-189">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="09e6c-190">Klikněte pravým tlačítkem na prázdné místo modelu v EF designeru a vyberte **přidat položku generování kódu...**</span><span class="sxs-lookup"><span data-stu-id="09e6c-190">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="09e6c-191">Vyberte **Online šablon** v levé nabídce a vyhledejte **DbContext**</span><span class="sxs-lookup"><span data-stu-id="09e6c-191">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="09e6c-192">Vyberte **EF 6.x DbContext generátor pro jazyk C\#,** zadejte **ProductsModel** jako název a klikněte na tlačítko Přidat</span><span class="sxs-lookup"><span data-stu-id="09e6c-192">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="09e6c-193">Aktualizuje se generování kódu pro vytváření datových vazeb</span><span class="sxs-lookup"><span data-stu-id="09e6c-193">Updating code generation for data binding</span></span>

<span data-ttu-id="09e6c-194">EF generuje kód z modelu pomocí šablon T4.</span><span class="sxs-lookup"><span data-stu-id="09e6c-194">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="09e6c-195">Šablony součástí sady Visual Studio nebo stáhnout z Galerie sady Visual Studio jsou určené pro obecné účely použití.</span><span class="sxs-lookup"><span data-stu-id="09e6c-195">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="09e6c-196">To znamená, že entity vygenerované z těchto šablon mít jednoduché rozhraní ICollection&lt;T&gt; vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="09e6c-196">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="09e6c-197">Při vytváření datových vazeb je však třeba mít nastavenou vlastnost kolekce, které implementují rozhraní IListSource.</span><span class="sxs-lookup"><span data-stu-id="09e6c-197">However, when doing data binding it is desirable to have collection properties that implement IListSource.</span></span> <span data-ttu-id="09e6c-198">To je důvod, proč jsme vytvořili výše ObservableListSource třídy a teď budeme upravovat šablony, aby pomocí této třídy.</span><span class="sxs-lookup"><span data-stu-id="09e6c-198">This is why we created the ObservableListSource class above and we are now going to modify the templates to make use of this class.</span></span>

-   <span data-ttu-id="09e6c-199">Otevřít **Průzkumníka řešení** a najít **ProductModel.edmx** souboru</span><span class="sxs-lookup"><span data-stu-id="09e6c-199">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="09e6c-200">Najít **ProductModel.tt** souboru, který se vnoří pod ProductModel.edmx souboru</span><span class="sxs-lookup"><span data-stu-id="09e6c-200">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![Šablona modelu produktu](~/ef6/media/productmodeltemplate.png)

-   <span data-ttu-id="09e6c-202">Poklikejte na soubor ProductModel.tt ho otevřete v editoru sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="09e6c-202">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="09e6c-203">Najít a nahradit dva výskyty "**rozhraní ICollection**"s"**ObservableListSource**".</span><span class="sxs-lookup"><span data-stu-id="09e6c-203">Find and replace the two occurrences of “**ICollection**” with “**ObservableListSource**”.</span></span> <span data-ttu-id="09e6c-204">Tyto jsou umístěny na přibližně řádky 296 a 484.</span><span class="sxs-lookup"><span data-stu-id="09e6c-204">These are located at approximately lines 296 and 484.</span></span>
-   <span data-ttu-id="09e6c-205">Najít a nahradit první výskyt "**HashSet**"s"**ObservableListSource**".</span><span class="sxs-lookup"><span data-stu-id="09e6c-205">Find and replace the first occurrence of “**HashSet**” with “**ObservableListSource**”.</span></span> <span data-ttu-id="09e6c-206">Výskyt této události se nachází na řádku přibližně 50.</span><span class="sxs-lookup"><span data-stu-id="09e6c-206">This occurrence is located at approximately line 50.</span></span> <span data-ttu-id="09e6c-207">**Ne** nahraďte druhým výskytem HashSet najdete dále v kódu.</span><span class="sxs-lookup"><span data-stu-id="09e6c-207">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="09e6c-208">Uložte soubor ProductModel.tt.</span><span class="sxs-lookup"><span data-stu-id="09e6c-208">Save the ProductModel.tt file.</span></span> <span data-ttu-id="09e6c-209">To by měl způsobí, že kód pro entity být znovu vygenerován.</span><span class="sxs-lookup"><span data-stu-id="09e6c-209">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="09e6c-210">Pokud kód automaticky neobnoví, klikněte pravým tlačítkem na ProductModel.tt a zvolte možnost "Spustit vlastní nástroj".</span><span class="sxs-lookup"><span data-stu-id="09e6c-210">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="09e6c-211">Pokud nyní otevřete soubor Category.cs (což je vnořený ProductModel.tt), pak byste měli vidět, že má kolekce produktů typ **ObservableListSource&lt;produktu&gt;**.</span><span class="sxs-lookup"><span data-stu-id="09e6c-211">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableListSource&lt;Product&gt;**.</span></span>

<span data-ttu-id="09e6c-212">Zkompilujte projekt.</span><span class="sxs-lookup"><span data-stu-id="09e6c-212">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="09e6c-213">Opožděné načtení</span><span class="sxs-lookup"><span data-stu-id="09e6c-213">Lazy Loading</span></span>

<span data-ttu-id="09e6c-214">**Produkty** vlastnost **kategorie** třídy a **kategorie** vlastnost **produktu** třídy jsou navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="09e6c-214">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="09e6c-215">Navigační vlastnosti v Entity Framework, poskytují způsob, jak procházet vztah mezi dvěma typy entit.</span><span class="sxs-lookup"><span data-stu-id="09e6c-215">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="09e6c-216">EF poskytuje možnost načtení souvisejících entit z databáze automaticky při prvním přístupu navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="09e6c-216">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="09e6c-217">S tímto typem načítání (označované jako opožděné načtení) mějte na paměti, že při prvním přístupu ke každé vlastnosti navigace samostatný dotaz se spustí na databázi není-li obsah již v kontextu.</span><span class="sxs-lookup"><span data-stu-id="09e6c-217">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="09e6c-218">Při použití typů entit POCO, EF dosahuje opožděné načtení vytváření instancí typů odvozených proxy za běhu a potom přepsáním virtuálních vlastností ve třídách přidat hook načítání.</span><span class="sxs-lookup"><span data-stu-id="09e6c-218">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="09e6c-219">Pokud chcete získat opožděné načtení souvisejících objektů, je třeba deklarovat navigace gettery vlastností jako **veřejné** a **virtuální** (**Overridable** v jazyce Visual Basic), a je třída nesmí být **zapečetěné** (**NotOverridable** v jazyce Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="09e6c-219">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="09e6c-220">Při použití databáze první navigačních vlastností se automaticky provede virtuální povolit opožděné načtení.</span><span class="sxs-lookup"><span data-stu-id="09e6c-220">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="09e6c-221">V části Code First jsme zvolili, navigačních vlastností virtuální ze stejného důvodu</span><span class="sxs-lookup"><span data-stu-id="09e6c-221">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="09e6c-222">Vytvoření vazby objektů k ovládacím prvkům</span><span class="sxs-lookup"><span data-stu-id="09e6c-222">Bind Object to Controls</span></span>

<span data-ttu-id="09e6c-223">Přidání třídy, které jsou definovány v modelu jako zdroj dat pro tuto aplikaci WinForms.</span><span class="sxs-lookup"><span data-stu-id="09e6c-223">Add the classes that are defined in the model as data sources for this WinForms application.</span></span>

-   <span data-ttu-id="09e6c-224">V hlavní nabídce vyberte **projekt –&gt; přidat nový zdroj dat...**</span><span class="sxs-lookup"><span data-stu-id="09e6c-224">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="09e6c-225">(v sadě Visual Studio 2010, je nutné vybrat **dat –&gt; přidat nový zdroj dat...** )</span><span class="sxs-lookup"><span data-stu-id="09e6c-225">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="09e6c-226">V okně zvolte typ zdroje dat, vyberte **objekt** a klikněte na tlačítko **další**</span><span class="sxs-lookup"><span data-stu-id="09e6c-226">In the Choose a Data Source Type window, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="09e6c-227">Vyberte datové objekty dialogu nejextrémnějších **WinFormswithEFSample** dvěma časy a vyberte **kategorie** není nutné vybrat zdroj dat produktu, protože se dostaneme k němu prostřednictvím produktu Vlastnost ve zdroji dat kategorie.</span><span class="sxs-lookup"><span data-stu-id="09e6c-227">In the Select the Data Objects dialog, unfold the **WinFormswithEFSample** two times and select **Category** There is no need to select the Product data source, because we will get to it through the Product’s property on the Category data source.</span></span>

    ![Zdroj dat](~/ef6/media/datasource.png)

-   <span data-ttu-id="09e6c-229">Klikněte na tlačítko **dokončit.** 
     *Pokud okna zdroje dat se nezobrazuje, vyberte \*\*\* zobrazení –&gt; ostatní Windows -&gt; zdroje dat*\*</span><span class="sxs-lookup"><span data-stu-id="09e6c-229">Click **Finish.**
*If the Data Sources window is not showing up, select\*\*\*View -&gt; Other Windows-&gt; Data Sources*\*</span></span>
-   <span data-ttu-id="09e6c-230">Stiskněte ikonu připínáčku tak okna zdroje dat není automaticky skrýt.</span><span class="sxs-lookup"><span data-stu-id="09e6c-230">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="09e6c-231">Budete muset stiskněte tlačítko Aktualizovat, pokud už je okno viditelné.</span><span class="sxs-lookup"><span data-stu-id="09e6c-231">You may need to hit the refresh button if the window was already visible.</span></span>

    ![Zdroj dat 2](~/ef6/media/datasource2.png)

-   <span data-ttu-id="09e6c-233">V Průzkumníku řešení poklikejte **Form1.cs** soubor otevřete hlavní formulář v návrháři.</span><span class="sxs-lookup"><span data-stu-id="09e6c-233">In Solution Explorer, double-click the **Form1.cs** file to open the main form in designer.</span></span>
-   <span data-ttu-id="09e6c-234">Vyberte **kategorie** zdroje dat a přetáhněte ji na formuláři.</span><span class="sxs-lookup"><span data-stu-id="09e6c-234">Select the **Category** data source and drag it on the form.</span></span> <span data-ttu-id="09e6c-235">Ve výchozím nastavení nového ovládacího prvku DataGridView (**categoryDataGridView**) a nástrojů navigační ovládací prvky jsou přidány do návrháře.</span><span class="sxs-lookup"><span data-stu-id="09e6c-235">By default, a new DataGridView (**categoryDataGridView**) and Navigation toolbar controls are added to the designer.</span></span> <span data-ttu-id="09e6c-236">Tyto ovládací prvky, které jsou vázány na objekt BindingSource (**categoryBindingSource**) a Navigátor vazby (**categoryBindingNavigator**) komponent, které jsou vytvořeny také.</span><span class="sxs-lookup"><span data-stu-id="09e6c-236">These controls are bound to the BindingSource (**categoryBindingSource**) and Binding Navigator (**categoryBindingNavigator**) components that are created as well.</span></span>
-   <span data-ttu-id="09e6c-237">Upravit sloupce na **categoryDataGridView**.</span><span class="sxs-lookup"><span data-stu-id="09e6c-237">Edit the columns on the **categoryDataGridView**.</span></span> <span data-ttu-id="09e6c-238">Chceme nastavit **CategoryId** sloupec jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="09e6c-238">We want to set the **CategoryId** column to read-only.</span></span> <span data-ttu-id="09e6c-239">Hodnota **CategoryId** vlastnost je generován databází, poté, co jsme uložit data.</span><span class="sxs-lookup"><span data-stu-id="09e6c-239">The value for the **CategoryId** property is generated by the database after we save the data.</span></span>
    -   <span data-ttu-id="09e6c-240">Klikněte pravým tlačítkem na ovládací prvek DataGridView a vyberte Upravit sloupce...</span><span class="sxs-lookup"><span data-stu-id="09e6c-240">Right-click the DataGridView control and select Edit Columns…</span></span>
    -   <span data-ttu-id="09e6c-241">Vyberte sloupec ID kategorie a nastaven na hodnotu True jen pro čtení</span><span class="sxs-lookup"><span data-stu-id="09e6c-241">Select the CategoryId column and set ReadOnly to True</span></span>
    -   <span data-ttu-id="09e6c-242">Kliknutím na tlačítko OK</span><span class="sxs-lookup"><span data-stu-id="09e6c-242">Press OK</span></span>
-   <span data-ttu-id="09e6c-243">Vyberte produkty ze zdroje dat kategorii a přetáhněte ho ve formuláři.</span><span class="sxs-lookup"><span data-stu-id="09e6c-243">Select Products from under the Category data source and drag it on the form.</span></span> <span data-ttu-id="09e6c-244">ProductDataGridView a productBindingSource se přidají do formuláře.</span><span class="sxs-lookup"><span data-stu-id="09e6c-244">The productDataGridView and productBindingSource are added to the form.</span></span>
-   <span data-ttu-id="09e6c-245">Upravte sloupce na productDataGridView.</span><span class="sxs-lookup"><span data-stu-id="09e6c-245">Edit the columns on the productDataGridView.</span></span> <span data-ttu-id="09e6c-246">Chcete skrýt sloupce CategoryId a kategorie a nastavte ProductId jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="09e6c-246">We want to hide the CategoryId and Category columns and set ProductId to read-only.</span></span> <span data-ttu-id="09e6c-247">Hodnota vlastnosti ProductId je generován databází, poté, co jsme uložit data.</span><span class="sxs-lookup"><span data-stu-id="09e6c-247">The value for the ProductId property is generated by the database after we save the data.</span></span>
    -   <span data-ttu-id="09e6c-248">Klikněte pravým tlačítkem na ovládací prvek DataGridView a vyberte **upravit sloupce...** .</span><span class="sxs-lookup"><span data-stu-id="09e6c-248">Right-click the DataGridView control and select **Edit Columns...**.</span></span>
    -   <span data-ttu-id="09e6c-249">Vyberte **ProductId** sloupec a sadu **jen pro čtení** k **True**.</span><span class="sxs-lookup"><span data-stu-id="09e6c-249">Select the **ProductId** column and set **ReadOnly** to **True**.</span></span>
    -   <span data-ttu-id="09e6c-250">Vyberte **CategoryId** sloupce a stiskněte klávesu **odebrat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="09e6c-250">Select the **CategoryId** column and press the **Remove** button.</span></span> <span data-ttu-id="09e6c-251">Postupujte stejným způsobem pracovat s **kategorie** sloupce.</span><span class="sxs-lookup"><span data-stu-id="09e6c-251">Do the same with the **Category** column.</span></span>
    -   <span data-ttu-id="09e6c-252">Stisknutím klávesy **OK**.</span><span class="sxs-lookup"><span data-stu-id="09e6c-252">Press **OK**.</span></span>

    <span data-ttu-id="09e6c-253">Zatím jsme naše ovládacích prvků DataGridView spojené s BindingSource součásti v návrháři.</span><span class="sxs-lookup"><span data-stu-id="09e6c-253">So far, we associated our DataGridView controls with BindingSource components in the designer.</span></span> <span data-ttu-id="09e6c-254">V další části přidáme kód do kódu nastavit categoryBindingSource.DataSource ke kolekci entit, které jsou aktuálně sledovány objektem DbContext.</span><span class="sxs-lookup"><span data-stu-id="09e6c-254">In the next section we will add code to the code behind to set categoryBindingSource.DataSource to the collection of entities that are currently tracked by DbContext.</span></span> <span data-ttu-id="09e6c-255">Když jsme kvůli usnadnění použití vypsány a vyřadit produkty z kategorie WinForms trvalo stará o nastavení vlastnost productsBindingSource.DataSource categoryBindingSource a productsBindingSource.DataMember vlastnosti k produktům.</span><span class="sxs-lookup"><span data-stu-id="09e6c-255">When we dragged-and-dropped Products from under the Category, the WinForms took care of setting up the productsBindingSource.DataSource property to categoryBindingSource and productsBindingSource.DataMember property to Products.</span></span> <span data-ttu-id="09e6c-256">Z důvodu této vazby v productDataGridView zobrazí pouze produkty, které patří do vybrané kategorie.</span><span class="sxs-lookup"><span data-stu-id="09e6c-256">Because of this binding, only the products that belong to the currently selected Category will be displayed in the productDataGridView.</span></span>
-   <span data-ttu-id="09e6c-257">Povolit **Uložit** tlačítko na navigačním panelu kliknete pravým tlačítkem myši a výběrem **povoleno**.</span><span class="sxs-lookup"><span data-stu-id="09e6c-257">Enable the **Save** button on the Navigation toolbar by clicking the right mouse button and selecting **Enabled**.</span></span>

    ![Návrhář formulářů 1](~/ef6/media/form1-designer.png)

-   <span data-ttu-id="09e6c-259">Přidat obslužnou rutinu události pro uložení tlačítko dvojitým kliknutím na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="09e6c-259">Add the event handler for the save button by double-clicking on the button.</span></span> <span data-ttu-id="09e6c-260">Tím přidáte obslužné rutiny události a můžete přenést do kódu pro formulář.</span><span class="sxs-lookup"><span data-stu-id="09e6c-260">This will add the event handler and bring you to the code behind for the form.</span></span> <span data-ttu-id="09e6c-261">Kód **categoryBindingNavigatorSaveItem\_klikněte na tlačítko** obslužné rutiny události bude přidána v další části.</span><span class="sxs-lookup"><span data-stu-id="09e6c-261">The code for the **categoryBindingNavigatorSaveItem\_Click** event handler will be added in the next section.</span></span>

## <a name="add-the-code-that-handles-data-interaction"></a><span data-ttu-id="09e6c-262">Přidejte kód, který zpracovává Data interakce</span><span class="sxs-lookup"><span data-stu-id="09e6c-262">Add the Code that Handles Data Interaction</span></span>

<span data-ttu-id="09e6c-263">Teď přidáme kód pro použití ProductContext přístup k datům.</span><span class="sxs-lookup"><span data-stu-id="09e6c-263">We'll now add the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="09e6c-264">Aktualizace kódu pro okno hlavního formuláře, jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="09e6c-264">Update the code for the main form window as shown below.</span></span>

<span data-ttu-id="09e6c-265">Kód deklaruje instanci dlouhotrvající ProductContext.</span><span class="sxs-lookup"><span data-stu-id="09e6c-265">The code declares a long-running instance of ProductContext.</span></span> <span data-ttu-id="09e6c-266">Objekt ProductContext se používá k dotazování a ukládání dat do databáze.</span><span class="sxs-lookup"><span data-stu-id="09e6c-266">The ProductContext object is used to query and save data to the database.</span></span> <span data-ttu-id="09e6c-267">Metoda Dispose() na instanci ProductContext se pak volá z přepsané metody OnClosing.</span><span class="sxs-lookup"><span data-stu-id="09e6c-267">The Dispose() method on the ProductContext instance is then called from the overridden OnClosing method.</span></span> <span data-ttu-id="09e6c-268">V komentářích ke kódu obsahují podrobné informace o co kód dělá.</span><span class="sxs-lookup"><span data-stu-id="09e6c-268">The code comments provide details about what the code does.</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.ComponentModel;
    using System.Data;
    using System.Drawing;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Windows.Forms;
    using System.Data.Entity;

    namespace WinFormswithEFSample
    {
        public partial class Form1 : Form
        {
            ProductContext _context;
            public Form1()
            {
                InitializeComponent();
            }

            protected override void OnLoad(EventArgs e)
            {
                base.OnLoad(e);
                _context = new ProductContext();

                // Call the Load method to get the data for the given DbSet
                // from the database.
                // The data is materialized as entities. The entities are managed by
                // the DbContext instance.
                _context.Categories.Load();

                // Bind the categoryBindingSource.DataSource to
                // all the Unchanged, Modified and Added Category objects that
                // are currently tracked by the DbContext.
                // Note that we need to call ToBindingList() on the
                // ObservableCollection<TEntity> returned by
                // the DbSet.Local property to get the BindingList<T>
                // in order to facilitate two-way binding in WinForms.
                this.categoryBindingSource.DataSource =
                    _context.Categories.Local.ToBindingList();
            }

            private void categoryBindingNavigatorSaveItem_Click(object sender, EventArgs e)
            {
                this.Validate();

                // Currently, the Entity Framework doesn’t mark the entities
                // that are removed from a navigation property (in our example the Products)
                // as deleted in the context.
                // The following code uses LINQ to Objects against the Local collection
                // to find all products and marks any that do not have
                // a Category reference as deleted.
                // The ToList call is required because otherwise
                // the collection will be modified
                // by the Remove call while it is being enumerated.
                // In most other situations you can do LINQ to Objects directly
                // against the Local property without using ToList first.
                foreach (var product in _context.Products.Local.ToList())
                {
                    if (product.Category == null)
                    {
                        _context.Products.Remove(product);
                    }
                }

                // Save the changes to the database.
                this._context.SaveChanges();

                // Refresh the controls to show the values         
                // that were generated by the database.
                this.categoryDataGridView.Refresh();
                this.productsDataGridView.Refresh();
            }

            protected override void OnClosing(CancelEventArgs e)
            {
                base.OnClosing(e);
                this._context.Dispose();
            }
        }
    }
```

## <a name="test-the-windows-forms-application"></a><span data-ttu-id="09e6c-269">Testování aplikace Windows Forms</span><span class="sxs-lookup"><span data-stu-id="09e6c-269">Test the Windows Forms Application</span></span>

-   <span data-ttu-id="09e6c-270">Kompilace a spuštění, které můžete otestovat aplikaci a budete si moct funkce.</span><span class="sxs-lookup"><span data-stu-id="09e6c-270">Compile and run the application and you can test out the functionality.</span></span>

    ![Formulář 1 před uložit](~/ef6/media/form1beforesave.png)

-   <span data-ttu-id="09e6c-272">Po uložení klíče úložiště, vygeneruje se zobrazí na obrazovce.</span><span class="sxs-lookup"><span data-stu-id="09e6c-272">After saving the store generated keys are shown on the screen.</span></span>

    ![Formuláře 1 po uložení](~/ef6/media/form1aftersave.png)

-   <span data-ttu-id="09e6c-274">Pokud jste použili Code First, pak bude také uvidíte, že **WinFormswithEFSample.ProductContext** databáze se vytvoří za vás.</span><span class="sxs-lookup"><span data-stu-id="09e6c-274">If you used Code First, then you will also see that a **WinFormswithEFSample.ProductContext** database is created for you.</span></span>

    ![Průzkumník objektů serveru](~/ef6/media/serverobjexplorer.png)
