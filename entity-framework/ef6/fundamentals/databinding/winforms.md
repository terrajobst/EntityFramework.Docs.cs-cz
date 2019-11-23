---
title: Vazba s WinForms – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 80fc5062-2f1c-4dbd-ab6e-b99496784b36
ms.openlocfilehash: 4b3eee20ff238864b94ef4edfb97c1bae0713300
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181791"
---
# <a name="databinding-with-winforms"></a><span data-ttu-id="32c94-102">Datová vazba s WinForms</span><span class="sxs-lookup"><span data-stu-id="32c94-102">Databinding with WinForms</span></span>
<span data-ttu-id="32c94-103">V tomto podrobném návodu se dozvíte, jak navazovat POCO typy na ovládací prvky WinForms (Window Forms) ve formuláři "Master-Detail".</span><span class="sxs-lookup"><span data-stu-id="32c94-103">This step-by-step walkthrough shows how to bind POCO types to Window Forms (WinForms) controls in a “master-detail" form.</span></span> <span data-ttu-id="32c94-104">Aplikace používá Entity Framework k naplnění objektů daty z databáze, sledování změn a zachování dat v databázi.</span><span class="sxs-lookup"><span data-stu-id="32c94-104">The application uses Entity Framework to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="32c94-105">Model definuje dva typy, které se účastní vztahu 1:1: kategorie (hlavní\\hlavní server) a produkt (podrobnosti o závislých\\ch).</span><span class="sxs-lookup"><span data-stu-id="32c94-105">The model defines two types that participate in one-to-many relationship: Category (principal\\master) and Product (dependent\\detail).</span></span> <span data-ttu-id="32c94-106">Nástroje sady Visual Studio pak slouží k navázání typů definovaných v modelu na ovládací prvky WinForms.</span><span class="sxs-lookup"><span data-stu-id="32c94-106">Then, the Visual Studio tools are used to bind the types defined in the model to the WinForms controls.</span></span> <span data-ttu-id="32c94-107">Rozhraní WinForms pro vázání dat umožňuje navigaci mezi souvisejícími objekty: výběrem řádků v zobrazení Předloha způsobí, že se podrobné zobrazení aktualizuje s odpovídajícími podřízenými daty.</span><span class="sxs-lookup"><span data-stu-id="32c94-107">The WinForms data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="32c94-108">Snímky obrazovky a výpisy kódu v tomto návodu jsou pořízeny z Visual Studio 2013, ale můžete tento návod dokončit pomocí sady Visual Studio 2012 nebo Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="32c94-108">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="32c94-109">Předpoklady</span><span class="sxs-lookup"><span data-stu-id="32c94-109">Pre-Requisites</span></span>

<span data-ttu-id="32c94-110">Pro dokončení tohoto Názorného postupu musíte mít nainstalovanou Visual Studio 2013, Visual Studio 2012 nebo Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="32c94-110">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="32c94-111">Pokud používáte Visual Studio 2010, je také nutné nainstalovat NuGet.</span><span class="sxs-lookup"><span data-stu-id="32c94-111">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="32c94-112">Další informace najdete v tématu [instalace NuGet](https://docs.nuget.org/docs/start-here/installing-nuget).</span><span class="sxs-lookup"><span data-stu-id="32c94-112">For more information, see [Installing NuGet](https://docs.nuget.org/docs/start-here/installing-nuget).</span></span>

## <a name="create-the-application"></a><span data-ttu-id="32c94-113">Vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="32c94-113">Create the Application</span></span>

-   <span data-ttu-id="32c94-114">Otevřít Visual Studio</span><span class="sxs-lookup"><span data-stu-id="32c94-114">Open Visual Studio</span></span>
-   <span data-ttu-id="32c94-115">**Soubor-&gt; projekt New-&gt;...**</span><span class="sxs-lookup"><span data-stu-id="32c94-115">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="32c94-116">V levém podokně vyberte **Windows** a v pravém podokně klikněte na **Windows FormsApplication** .</span><span class="sxs-lookup"><span data-stu-id="32c94-116">Select **Windows** in the left pane and **Windows FormsApplication** in the right pane</span></span>
-   <span data-ttu-id="32c94-117">Jako název zadejte **WinFormswithEFSample** .</span><span class="sxs-lookup"><span data-stu-id="32c94-117">Enter **WinFormswithEFSample** as the name</span></span>
-   <span data-ttu-id="32c94-118">Vybrat **OK**</span><span class="sxs-lookup"><span data-stu-id="32c94-118">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="32c94-119">Instalace balíčku Entity Framework NuGet</span><span class="sxs-lookup"><span data-stu-id="32c94-119">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="32c94-120">V Průzkumník řešení klikněte pravým tlačítkem na projekt **WinFormswithEFSample** .</span><span class="sxs-lookup"><span data-stu-id="32c94-120">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="32c94-121">Vyberte **Spravovat balíčky NuGet...**</span><span class="sxs-lookup"><span data-stu-id="32c94-121">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="32c94-122">V dialogovém okně Spravovat balíčky NuGet vyberte kartu **online** a zvolte balíček **EntityFramework** .</span><span class="sxs-lookup"><span data-stu-id="32c94-122">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="32c94-123">Klikněte na **nainstalovat** .</span><span class="sxs-lookup"><span data-stu-id="32c94-123">Click **Install**</span></span>  
    > [!NOTE]
    > <span data-ttu-id="32c94-124">Kromě sestavení EntityFramework je také přidáno odkaz na System. ComponentModel. DataAnnotations.</span><span class="sxs-lookup"><span data-stu-id="32c94-124">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="32c94-125">Pokud má projekt odkaz na System. data. entity, bude po instalaci balíčku EntityFramework odebrán.</span><span class="sxs-lookup"><span data-stu-id="32c94-125">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="32c94-126">Sestavení System. data. entity již není používáno pro Entity Framework 6 aplikací.</span><span class="sxs-lookup"><span data-stu-id="32c94-126">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="implementing-ilistsource-for-collections"></a><span data-ttu-id="32c94-127">Implementace IListSource pro kolekce</span><span class="sxs-lookup"><span data-stu-id="32c94-127">Implementing IListSource for Collections</span></span>

<span data-ttu-id="32c94-128">Vlastnosti kolekce musí implementovat rozhraní IListSource, aby bylo možné při použití model Windows Forms Povolit obousměrnou datovou vazbu s řazením.</span><span class="sxs-lookup"><span data-stu-id="32c94-128">Collection properties must implement the IListSource interface to enable two-way data binding with sorting when using Windows Forms.</span></span> <span data-ttu-id="32c94-129">Za tímto účelem rozšíříme kolekci ObservableCollection, aby se přidaly funkce IListSource.</span><span class="sxs-lookup"><span data-stu-id="32c94-129">To do this we are going to extend ObservableCollection to add IListSource functionality.</span></span>

-   <span data-ttu-id="32c94-130">Přidejte do projektu třídu **ObservableListSource** :</span><span class="sxs-lookup"><span data-stu-id="32c94-130">Add an **ObservableListSource** class to the project:</span></span>
    -   <span data-ttu-id="32c94-131">Klikněte pravým tlačítkem myši na název projektu.</span><span class="sxs-lookup"><span data-stu-id="32c94-131">Right-click on the project name</span></span>
    -   <span data-ttu-id="32c94-132">Vybrat **Add-&gt; novou položku**</span><span class="sxs-lookup"><span data-stu-id="32c94-132">Select **Add -&gt; New Item**</span></span>
    -   <span data-ttu-id="32c94-133">Vyberte **třídu** a jako název třídy zadejte **ObservableListSource** .</span><span class="sxs-lookup"><span data-stu-id="32c94-133">Select **Class** and enter **ObservableListSource** for the class name</span></span>
-   <span data-ttu-id="32c94-134">Nahraďte kód vygenerovaný ve výchozím nastavení následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="32c94-134">Replace the code generated by default with the following code:</span></span>

<span data-ttu-id="32c94-135">*Tato třída umožňuje obousměrnou datovou vazbu a řazení. Třída je odvozena z kolekci ObservableCollection&lt;T&gt; a přidává explicitní implementaci IListSource. Metoda GetList () IListSource je implementována pro vrácení implementace IBindingList, která zůstává synchronizována s kolekci ObservableCollection. Implementace IBindingList vygenerovaná ToBindingList podporuje řazení. Metoda rozšíření ToBindingList je definována v sestavení EntityFramework.*</span><span class="sxs-lookup"><span data-stu-id="32c94-135">*This class enables two-way data binding as well as sorting. The class derives from ObservableCollection&lt;T&gt; and adds an explicit implementation of IListSource. The GetList() method of IListSource is implemented to return an IBindingList implementation that stays in sync with the ObservableCollection. The IBindingList implementation generated by ToBindingList supports sorting. The ToBindingList extension method is defined in the EntityFramework assembly.*</span></span>

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

## <a name="define-a-model"></a><span data-ttu-id="32c94-136">Definování modelu</span><span class="sxs-lookup"><span data-stu-id="32c94-136">Define a Model</span></span>

<span data-ttu-id="32c94-137">V tomto návodu můžete zvolit implementaci modelu pomocí Code First nebo návrháře EF.</span><span class="sxs-lookup"><span data-stu-id="32c94-137">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="32c94-138">Proveďte jednu ze dvou následujících částí.</span><span class="sxs-lookup"><span data-stu-id="32c94-138">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="32c94-139">Možnost 1: definování modelu pomocí Code First</span><span class="sxs-lookup"><span data-stu-id="32c94-139">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="32c94-140">V této části se dozvíte, jak vytvořit model a jeho přidruženou databázi pomocí Code First.</span><span class="sxs-lookup"><span data-stu-id="32c94-140">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="32c94-141">Přejděte k další části (**možnost 2: definice modelu pomocí Database First)** , pokud byste chtěli použít Database First k zpětné analýze modelu z databáze pomocí návrháře EF.</span><span class="sxs-lookup"><span data-stu-id="32c94-141">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="32c94-142">Při použití vývoje Code First obvykle začněte psaním .NET Framework tříd, které definují koncepční (doménový) model.</span><span class="sxs-lookup"><span data-stu-id="32c94-142">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="32c94-143">Přidat novou třídu **produktu** do projektu</span><span class="sxs-lookup"><span data-stu-id="32c94-143">Add a new **Product** class to project</span></span>
-   <span data-ttu-id="32c94-144">Nahraďte kód vygenerovaný ve výchozím nastavení následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="32c94-144">Replace the code generated by default with the following code:</span></span>

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

-   <span data-ttu-id="32c94-145">Přidejte do projektu třídu **kategorie** .</span><span class="sxs-lookup"><span data-stu-id="32c94-145">Add a **Category** class to the project.</span></span>
-   <span data-ttu-id="32c94-146">Nahraďte kód vygenerovaný ve výchozím nastavení následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="32c94-146">Replace the code generated by default with the following code:</span></span>

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

<span data-ttu-id="32c94-147">Kromě definování entit musíte definovat třídu, která je odvozena z **DbContext** a zpřístupňuje **Negenerickými&lt;vlastnosti TEntity&gt;** .</span><span class="sxs-lookup"><span data-stu-id="32c94-147">In addition to defining entities, you need to define a class that derives from **DbContext** and exposes **DbSet&lt;TEntity&gt;** properties.</span></span> <span data-ttu-id="32c94-148">Vlastnosti **negenerickými** umožňují, aby kontext věděl, které typy chcete do modelu zahrnout.</span><span class="sxs-lookup"><span data-stu-id="32c94-148">The **DbSet** properties let the context know which types you want to include in the model.</span></span> <span data-ttu-id="32c94-149">Typy **DbContext** a **negenerickými** jsou definovány v sestavení EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="32c94-149">The **DbContext** and **DbSet** types are defined in the EntityFramework assembly.</span></span>

<span data-ttu-id="32c94-150">Instance DbContext odvozeného typu spravuje objekty entit za běhu, což zahrnuje vyplnění objektů daty z databáze, sledování změn a uchování dat do databáze.</span><span class="sxs-lookup"><span data-stu-id="32c94-150">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="32c94-151">Přidejte do projektu novou třídu **ProductContext** .</span><span class="sxs-lookup"><span data-stu-id="32c94-151">Add a new **ProductContext** class to the project.</span></span>
-   <span data-ttu-id="32c94-152">Nahraďte kód vygenerovaný ve výchozím nastavení následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="32c94-152">Replace the code generated by default with the following code:</span></span>

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

<span data-ttu-id="32c94-153">Zkompilujte projekt.</span><span class="sxs-lookup"><span data-stu-id="32c94-153">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="32c94-154">Možnost 2: definování modelu pomocí Database First</span><span class="sxs-lookup"><span data-stu-id="32c94-154">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="32c94-155">V této části se dozvíte, jak použít Database First k zpětné analýze modelu z databáze pomocí návrháře EF.</span><span class="sxs-lookup"><span data-stu-id="32c94-155">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="32c94-156">Pokud jste dokončili předchozí oddíl (**možnost 1: Definujte model pomocí Code First)** , pak tuto část přeskočte a přejděte rovnou k oddílu **opožděné načítání** .</span><span class="sxs-lookup"><span data-stu-id="32c94-156">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="32c94-157">Vytvoření existující databáze</span><span class="sxs-lookup"><span data-stu-id="32c94-157">Create an Existing Database</span></span>

<span data-ttu-id="32c94-158">Když cílíte na existující databázi, bude už vytvořená, ale pro tento návod musíme pro přístup vytvořit databázi.</span><span class="sxs-lookup"><span data-stu-id="32c94-158">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="32c94-159">Databázový server, který je nainstalovaný se sadou Visual Studio, se liší v závislosti na verzi sady Visual Studio, kterou jste nainstalovali:</span><span class="sxs-lookup"><span data-stu-id="32c94-159">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="32c94-160">Pokud používáte Visual Studio 2010, budete vytvářet databázi SQL Express.</span><span class="sxs-lookup"><span data-stu-id="32c94-160">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="32c94-161">Pokud používáte Visual Studio 2012, budete vytvářet databázi [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) .</span><span class="sxs-lookup"><span data-stu-id="32c94-161">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="32c94-162">Pojďme dopředu a vygenerovat databázi.</span><span class="sxs-lookup"><span data-stu-id="32c94-162">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="32c94-163">**Zobrazení-&gt; Průzkumník serveru**</span><span class="sxs-lookup"><span data-stu-id="32c94-163">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="32c94-164">Klikněte pravým tlačítkem na **datová připojení –&gt; přidat připojení...**</span><span class="sxs-lookup"><span data-stu-id="32c94-164">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="32c94-165">Pokud jste se k databázi nepřipojili z Průzkumník serveru před tím, než bude nutné vybrat Microsoft SQL Server jako zdroj dat</span><span class="sxs-lookup"><span data-stu-id="32c94-165">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Změnit zdroj dat](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="32c94-167">Připojte se buď k LocalDB nebo SQL Express v závislosti na tom, který z nich jste nainstalovali, a jako název databáze zadejte **produkty** .</span><span class="sxs-lookup"><span data-stu-id="32c94-167">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![Přidat LocalDB připojení](~/ef6/media/addconnectionlocaldb.png)

    ![Přidat Express připojení](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="32c94-170">Vyberte **OK** a zobrazí se dotaz, jestli chcete vytvořit novou databázi, a pak vyberte **Ano** .</span><span class="sxs-lookup"><span data-stu-id="32c94-170">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Vytvoření databáze](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="32c94-172">Nová databáze se nyní zobrazí v Průzkumník serveru, klikněte na ni pravým tlačítkem myši a vyberte **Nový dotaz** .</span><span class="sxs-lookup"><span data-stu-id="32c94-172">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="32c94-173">Zkopírujte následující příkaz SQL do nového dotazu, klikněte na něj pravým tlačítkem myši a vyberte **Spustit** .</span><span class="sxs-lookup"><span data-stu-id="32c94-173">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

#### <a name="reverse-engineer-model"></a><span data-ttu-id="32c94-174">Model zpětného analýz</span><span class="sxs-lookup"><span data-stu-id="32c94-174">Reverse Engineer Model</span></span>

<span data-ttu-id="32c94-175">Budeme používat Entity Framework Designer, který je součástí sady Visual Studio, a vytvořit náš model.</span><span class="sxs-lookup"><span data-stu-id="32c94-175">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="32c94-176">**Projekt –&gt; přidat novou položku...**</span><span class="sxs-lookup"><span data-stu-id="32c94-176">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="32c94-177">V nabídce vlevo vyberte **data** a pak **ADO.NET model EDM (Entity Data Model)**</span><span class="sxs-lookup"><span data-stu-id="32c94-177">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="32c94-178">Jako název zadejte **ProductModel** a klikněte na **OK** .</span><span class="sxs-lookup"><span data-stu-id="32c94-178">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="32c94-179">Spustí se **průvodce model EDM (Entity Data Model)** .</span><span class="sxs-lookup"><span data-stu-id="32c94-179">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="32c94-180">Vyberte **Generovat z databáze** a klikněte na **Další** .</span><span class="sxs-lookup"><span data-stu-id="32c94-180">Select **Generate from Database** and click **Next**</span></span>

    ![ChooseModelContents](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="32c94-182">Vyberte připojení k databázi, kterou jste vytvořili v první části, jako název připojovacího řetězce zadejte **ProductContext** a klikněte na **Další** .</span><span class="sxs-lookup"><span data-stu-id="32c94-182">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![Zvolit připojení](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="32c94-184">Kliknutím na zaškrtávací políčko vedle tabulky naimportujete všechny tabulky a kliknete na Dokončit.</span><span class="sxs-lookup"><span data-stu-id="32c94-184">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Zvolit vaše objekty](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="32c94-186">Po dokončení procesu zpětného zpracování se do projektu přidá nový model, který jste otevřeli, abyste mohli zobrazit Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="32c94-186">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="32c94-187">Do projektu byl také přidán soubor App. config s podrobnostmi o připojení pro databázi.</span><span class="sxs-lookup"><span data-stu-id="32c94-187">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="32c94-188">Další kroky v aplikaci Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="32c94-188">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="32c94-189">Pokud pracujete v aplikaci Visual Studio 2010, bude nutné aktualizovat návrháře EF, aby používal generování kódu EF6.</span><span class="sxs-lookup"><span data-stu-id="32c94-189">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="32c94-190">V Návrháři EF klikněte pravým tlačítkem na prázdný bod modelu a vyberte **Přidat položku pro generování kódu...**</span><span class="sxs-lookup"><span data-stu-id="32c94-190">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="32c94-191">V nabídce vlevo vyberte **online šablony** a vyhledejte **DbContext** .</span><span class="sxs-lookup"><span data-stu-id="32c94-191">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="32c94-192">Vyberte **generátor EF 6. x DbContext pro jazyk C\#,** jako název zadejte **ProductsModel** a klikněte na Přidat.</span><span class="sxs-lookup"><span data-stu-id="32c94-192">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="32c94-193">Aktualizuje se generování kódu pro datovou vazbu.</span><span class="sxs-lookup"><span data-stu-id="32c94-193">Updating code generation for data binding</span></span>

<span data-ttu-id="32c94-194">EF generuje kód z modelu pomocí šablon T4.</span><span class="sxs-lookup"><span data-stu-id="32c94-194">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="32c94-195">Šablony dodávané se sadou Visual Studio nebo stažené z galerie sady Visual Studio jsou určené pro účely obecného použití.</span><span class="sxs-lookup"><span data-stu-id="32c94-195">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="32c94-196">To znamená, že entity vygenerované z těchto šablon mají jednoduché vlastnosti rozhraní ICollection&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="32c94-196">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="32c94-197">Při provádění datových vazeb je ale žádoucí, aby měly vlastnosti kolekce, které implementují IListSource.</span><span class="sxs-lookup"><span data-stu-id="32c94-197">However, when doing data binding it is desirable to have collection properties that implement IListSource.</span></span> <span data-ttu-id="32c94-198">To je důvod, proč jsme vytvořili třídu ObservableListSource výše a teď se chystáme upravit šablony, aby bylo možné tuto třídu používat.</span><span class="sxs-lookup"><span data-stu-id="32c94-198">This is why we created the ObservableListSource class above and we are now going to modify the templates to make use of this class.</span></span>

-   <span data-ttu-id="32c94-199">Otevřete **Průzkumník řešení** a vyhledejte soubor **ProductModel. edmx.**</span><span class="sxs-lookup"><span data-stu-id="32c94-199">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="32c94-200">Vyhledejte soubor **ProductModel.TT** , který bude vnořen do souboru ProductModel. edmx.</span><span class="sxs-lookup"><span data-stu-id="32c94-200">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![Šablona modelu produktu](~/ef6/media/productmodeltemplate.png)

-   <span data-ttu-id="32c94-202">Poklikejte na soubor ProductModel.tt a otevře se v editoru Visual studia.</span><span class="sxs-lookup"><span data-stu-id="32c94-202">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="32c94-203">Vyhledejte a nahraďte dva výskyty "**ICollection**" pomocí "**ObservableListSource**".</span><span class="sxs-lookup"><span data-stu-id="32c94-203">Find and replace the two occurrences of “**ICollection**” with “**ObservableListSource**”.</span></span> <span data-ttu-id="32c94-204">Tyto jsou umístěné přibližně na řádcích 296 a 484.</span><span class="sxs-lookup"><span data-stu-id="32c94-204">These are located at approximately lines 296 and 484.</span></span>
-   <span data-ttu-id="32c94-205">Najde první výskyt "**HashSet –** " a nahradí ho "**ObservableListSource**".</span><span class="sxs-lookup"><span data-stu-id="32c94-205">Find and replace the first occurrence of “**HashSet**” with “**ObservableListSource**”.</span></span> <span data-ttu-id="32c94-206">Tento výskyt je umístěný přibližně na řádku 50.</span><span class="sxs-lookup"><span data-stu-id="32c94-206">This occurrence is located at approximately line 50.</span></span> <span data-ttu-id="32c94-207">**Neměňte druhý** výskyt HashSet –, který byl nalezen později v kódu.</span><span class="sxs-lookup"><span data-stu-id="32c94-207">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="32c94-208">Uložte soubor ProductModel.tt.</span><span class="sxs-lookup"><span data-stu-id="32c94-208">Save the ProductModel.tt file.</span></span> <span data-ttu-id="32c94-209">To by mělo způsobit opětovné vygenerování kódu pro entity.</span><span class="sxs-lookup"><span data-stu-id="32c94-209">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="32c94-210">Pokud se kód znovu negeneruje automaticky, klikněte pravým tlačítkem na ProductModel.tt a zvolte spustit vlastní nástroj.</span><span class="sxs-lookup"><span data-stu-id="32c94-210">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="32c94-211">Pokud teď otevřete soubor Category.cs (který je vnořený pod ProductModel.tt), měli byste vidět, že kolekce Products má typ **ObservableListSource&lt;&gt;produktu** .</span><span class="sxs-lookup"><span data-stu-id="32c94-211">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableListSource&lt;Product&gt;**.</span></span>

<span data-ttu-id="32c94-212">Zkompilujte projekt.</span><span class="sxs-lookup"><span data-stu-id="32c94-212">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="32c94-213">Opožděné načítání</span><span class="sxs-lookup"><span data-stu-id="32c94-213">Lazy Loading</span></span>

<span data-ttu-id="32c94-214">Vlastnost **Products** **u vlastnosti Category třídy a** **kategorie** u třídy **produkt** je vlastností navigace.</span><span class="sxs-lookup"><span data-stu-id="32c94-214">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="32c94-215">V Entity Framework navigační vlastnosti poskytují způsob, jak procházet relaci mezi dvěma typy entit.</span><span class="sxs-lookup"><span data-stu-id="32c94-215">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="32c94-216">EF vám nabízí možnost načítat související entity z databáze automaticky při prvním přístupu k vlastnosti navigace.</span><span class="sxs-lookup"><span data-stu-id="32c94-216">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="32c94-217">U tohoto typu načítání (tzv. opožděné načítání) mějte na paměti, že při prvním přístupu k jednotlivým vlastnostem navigace se v databázi spustí samostatný dotaz, pokud obsah ještě není v kontextu.</span><span class="sxs-lookup"><span data-stu-id="32c94-217">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="32c94-218">Při použití typů entit POCO nahrazuje EF opožděné načítání vytvořením instancí odvozených typů proxy během běhu a následným přepsáním virtuálních vlastností ve třídách, aby bylo možné přidat zavěšení zatížení.</span><span class="sxs-lookup"><span data-stu-id="32c94-218">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="32c94-219">Chcete-li získat opožděné načítání souvisejících objektů, je nutné deklarovat metody Get vlastnosti navigace jako **veřejné** a **virtuální** (**přepsatelné** v Visual Basic) a třídu nesmí být **zapečetěna** (**NotOverridable** v Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="32c94-219">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="32c94-220">Při použití Database First navigační vlastnosti se automaticky vypnuly virtuálním, aby bylo možné povolit opožděné načítání.</span><span class="sxs-lookup"><span data-stu-id="32c94-220">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="32c94-221">V části Code First jsme se rozhodli, aby navigační vlastnosti byly ve stejném důsledku virtuální.</span><span class="sxs-lookup"><span data-stu-id="32c94-221">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="32c94-222">Svázání objektu s ovládacími prvky</span><span class="sxs-lookup"><span data-stu-id="32c94-222">Bind Object to Controls</span></span>

<span data-ttu-id="32c94-223">Přidejte třídy, které jsou definovány v modelu jako zdroje dat pro tuto aplikaci WinForms.</span><span class="sxs-lookup"><span data-stu-id="32c94-223">Add the classes that are defined in the model as data sources for this WinForms application.</span></span>

-   <span data-ttu-id="32c94-224">V hlavní nabídce vyberte **projekt –&gt; přidat nový zdroj dat...**</span><span class="sxs-lookup"><span data-stu-id="32c94-224">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="32c94-225">(v aplikaci Visual Studio 2010 je nutné vybrat **data –&gt; přidat nový zdroj dat...** )</span><span class="sxs-lookup"><span data-stu-id="32c94-225">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="32c94-226">V okně zvolte typ zdroje dat vyberte **objekt** a klikněte na **Další** .</span><span class="sxs-lookup"><span data-stu-id="32c94-226">In the Choose a Data Source Type window, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="32c94-227">V dialogovém okně Vybrat datové objekty rozložte **WinFormswithEFSample** dvakrát a vyberte **kategorie** . není nutné vybírat zdroj dat produktu, protože se k němu dostanete prostřednictvím vlastnosti produktu ve zdroji dat kategorie.</span><span class="sxs-lookup"><span data-stu-id="32c94-227">In the Select the Data Objects dialog, unfold the **WinFormswithEFSample** two times and select **Category** There is no need to select the Product data source, because we will get to it through the Product’s property on the Category data source.</span></span>

    ![Zdroj dat](~/ef6/media/datasource.png)

-   <span data-ttu-id="32c94-229">Klikněte na tlačítko **Dokončit.**</span><span class="sxs-lookup"><span data-stu-id="32c94-229">Click **Finish.**</span></span>
    <span data-ttu-id="32c94-230">Pokud se nezobrazí okno zdroje dat, vyberte **zobrazení-&gt; jiné zdroje dat&gt; Windows.**</span><span class="sxs-lookup"><span data-stu-id="32c94-230">If the Data Sources window is not showing up, select **View -&gt; Other Windows-&gt; Data Sources**</span></span>
-   <span data-ttu-id="32c94-231">Stiskněte ikonu připnutí, aby se okno zdroje dat neautomaticky skrylo.</span><span class="sxs-lookup"><span data-stu-id="32c94-231">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="32c94-232">Pokud je okno již viditelné, může být nutné spustit tlačítko Aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="32c94-232">You may need to hit the refresh button if the window was already visible.</span></span>

    ![Zdroj dat 2](~/ef6/media/datasource2.png)

-   <span data-ttu-id="32c94-234">V Průzkumník řešení dvakrát klikněte na soubor **Form1.cs** a otevřete tak hlavní formulář v návrháři.</span><span class="sxs-lookup"><span data-stu-id="32c94-234">In Solution Explorer, double-click the **Form1.cs** file to open the main form in designer.</span></span>
-   <span data-ttu-id="32c94-235">Vyberte zdroj dat **kategorie** a přetáhněte jej na formuláři.</span><span class="sxs-lookup"><span data-stu-id="32c94-235">Select the **Category** data source and drag it on the form.</span></span> <span data-ttu-id="32c94-236">Ve výchozím nastavení jsou nové ovládací prvky DataGridView (**categoryDataGridView**) a navigační panely nástrojů přidány do návrháře.</span><span class="sxs-lookup"><span data-stu-id="32c94-236">By default, a new DataGridView (**categoryDataGridView**) and Navigation toolbar controls are added to the designer.</span></span> <span data-ttu-id="32c94-237">Tyto ovládací prvky jsou vázány na komponenty BindingSource (**categoryBindingSource**) a vazby navigátor (**categoryBindingNavigator**), které jsou vytvořeny také.</span><span class="sxs-lookup"><span data-stu-id="32c94-237">These controls are bound to the BindingSource (**categoryBindingSource**) and Binding Navigator (**categoryBindingNavigator**) components that are created as well.</span></span>
-   <span data-ttu-id="32c94-238">Upravte sloupce v **categoryDataGridView**.</span><span class="sxs-lookup"><span data-stu-id="32c94-238">Edit the columns on the **categoryDataGridView**.</span></span> <span data-ttu-id="32c94-239">Chceme nastavit sloupec **KódKategorie** na hodnotu jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="32c94-239">We want to set the **CategoryId** column to read-only.</span></span> <span data-ttu-id="32c94-240">Hodnota vlastnosti **CategoryID** je vygenerována databází poté, co data uložíme.</span><span class="sxs-lookup"><span data-stu-id="32c94-240">The value for the **CategoryId** property is generated by the database after we save the data.</span></span>
    -   <span data-ttu-id="32c94-241">Klikněte pravým tlačítkem myši na ovládací prvek DataGridView a vyberte možnost Upravit sloupce...</span><span class="sxs-lookup"><span data-stu-id="32c94-241">Right-click the DataGridView control and select Edit Columns…</span></span>
    -   <span data-ttu-id="32c94-242">Vyberte sloupec KódKategorie a nastavte vlastnost ReadOnly na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="32c94-242">Select the CategoryId column and set ReadOnly to True</span></span>
    -   <span data-ttu-id="32c94-243">Stiskněte OK</span><span class="sxs-lookup"><span data-stu-id="32c94-243">Press OK</span></span>
-   <span data-ttu-id="32c94-244">Vyberte možnost produkty ze zdroje dat kategorie a přetáhněte ji do formuláře.</span><span class="sxs-lookup"><span data-stu-id="32c94-244">Select Products from under the Category data source and drag it on the form.</span></span> <span data-ttu-id="32c94-245">Do formuláře jsou přidány productDataGridView a productBindingSource.</span><span class="sxs-lookup"><span data-stu-id="32c94-245">The productDataGridView and productBindingSource are added to the form.</span></span>
-   <span data-ttu-id="32c94-246">Upravte sloupce v productDataGridView.</span><span class="sxs-lookup"><span data-stu-id="32c94-246">Edit the columns on the productDataGridView.</span></span> <span data-ttu-id="32c94-247">Chceme skrýt sloupce KódKategorie a Category a nastavit ProductId jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="32c94-247">We want to hide the CategoryId and Category columns and set ProductId to read-only.</span></span> <span data-ttu-id="32c94-248">Po uložení dat se hodnota vlastnosti ProductId vygeneruje v databázi.</span><span class="sxs-lookup"><span data-stu-id="32c94-248">The value for the ProductId property is generated by the database after we save the data.</span></span>
    -   <span data-ttu-id="32c94-249">Klikněte pravým tlačítkem myši na ovládací prvek DataGridView a vyberte možnost **Upravit sloupce...** .</span><span class="sxs-lookup"><span data-stu-id="32c94-249">Right-click the DataGridView control and select **Edit Columns...**.</span></span>
    -   <span data-ttu-id="32c94-250">Vyberte sloupec **ProductID** a nastavte vlastnost **ReadOnly** na **hodnotu true**.</span><span class="sxs-lookup"><span data-stu-id="32c94-250">Select the **ProductId** column and set **ReadOnly** to **True**.</span></span>
    -   <span data-ttu-id="32c94-251">Vyberte sloupec **CategoryID** a stiskněte tlačítko **Odebrat** .</span><span class="sxs-lookup"><span data-stu-id="32c94-251">Select the **CategoryId** column and press the **Remove** button.</span></span> <span data-ttu-id="32c94-252">Totéž proveďte se sloupcem **Category (kategorie** ).</span><span class="sxs-lookup"><span data-stu-id="32c94-252">Do the same with the **Category** column.</span></span>
    -   <span data-ttu-id="32c94-253">Stisknutím klávesy **OK**.</span><span class="sxs-lookup"><span data-stu-id="32c94-253">Press **OK**.</span></span>

    <span data-ttu-id="32c94-254">Zatím jsme přidružili ovládací prvky DataGridView k komponentám BindingSource v návrháři.</span><span class="sxs-lookup"><span data-stu-id="32c94-254">So far, we associated our DataGridView controls with BindingSource components in the designer.</span></span> <span data-ttu-id="32c94-255">V další části přidáme kód do kódu na pozadí pro nastavení categoryBindingSource. DataSource na kolekci entit, které jsou aktuálně sledovány pomocí DbContext.</span><span class="sxs-lookup"><span data-stu-id="32c94-255">In the next section we will add code to the code behind to set categoryBindingSource.DataSource to the collection of entities that are currently tracked by DbContext.</span></span> <span data-ttu-id="32c94-256">Když jsme přetáhli produkty z kategorie do kategorie, WinForms se postará o nastavení vlastnosti productsBindingSource. DataSource na categoryBindingSource a vlastnost productsBindingSource. DataMember na Products.</span><span class="sxs-lookup"><span data-stu-id="32c94-256">When we dragged-and-dropped Products from under the Category, the WinForms took care of setting up the productsBindingSource.DataSource property to categoryBindingSource and productsBindingSource.DataMember property to Products.</span></span> <span data-ttu-id="32c94-257">Z důvodu této vazby budou v productDataGridView zobrazeny pouze produkty patřící do aktuálně vybrané kategorie.</span><span class="sxs-lookup"><span data-stu-id="32c94-257">Because of this binding, only the products that belong to the currently selected Category will be displayed in the productDataGridView.</span></span>
-   <span data-ttu-id="32c94-258">Na panelu nástrojů navigace povolte tlačítko **Uložit** kliknutím pravým tlačítkem myši a výběrem možnosti **povoleno**.</span><span class="sxs-lookup"><span data-stu-id="32c94-258">Enable the **Save** button on the Navigation toolbar by clicking the right mouse button and selecting **Enabled**.</span></span>

    ![Návrhář – formulář 1](~/ef6/media/form1-designer.png)

-   <span data-ttu-id="32c94-260">Přidejte obslužnou rutinu události pro tlačítko Uložit dvojitým kliknutím na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="32c94-260">Add the event handler for the save button by double-clicking on the button.</span></span> <span data-ttu-id="32c94-261">Tím se přidá obslužná rutina události a budete přinášet k kódu za jeho pozadí.</span><span class="sxs-lookup"><span data-stu-id="32c94-261">This will add the event handler and bring you to the code behind for the form.</span></span> <span data-ttu-id="32c94-262">Do další části se přidá kód pro obslužnou rutinu události **categoryBindingNavigatorSaveItem\_Click** .</span><span class="sxs-lookup"><span data-stu-id="32c94-262">The code for the **categoryBindingNavigatorSaveItem\_Click** event handler will be added in the next section.</span></span>

## <a name="add-the-code-that-handles-data-interaction"></a><span data-ttu-id="32c94-263">Přidat kód, který zpracovává interakci s daty</span><span class="sxs-lookup"><span data-stu-id="32c94-263">Add the Code that Handles Data Interaction</span></span>

<span data-ttu-id="32c94-264">Nyní přidáme kód pro použití ProductContext k provedení přístupu k datům.</span><span class="sxs-lookup"><span data-stu-id="32c94-264">We'll now add the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="32c94-265">Aktualizujte kód pro hlavní okno formuláře, jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="32c94-265">Update the code for the main form window as shown below.</span></span>

<span data-ttu-id="32c94-266">Kód deklaruje dlouhodobě běžící instanci ProductContext.</span><span class="sxs-lookup"><span data-stu-id="32c94-266">The code declares a long-running instance of ProductContext.</span></span> <span data-ttu-id="32c94-267">Objekt ProductContext se používá k dotazování a ukládání dat do databáze.</span><span class="sxs-lookup"><span data-stu-id="32c94-267">The ProductContext object is used to query and save data to the database.</span></span> <span data-ttu-id="32c94-268">Metoda Dispose () v instanci ProductContext je pak volána z přepsané metody při zavírání.</span><span class="sxs-lookup"><span data-stu-id="32c94-268">The Dispose() method on the ProductContext instance is then called from the overridden OnClosing method.</span></span> <span data-ttu-id="32c94-269">Komentáře kódu poskytují podrobné informace o tom, co kód dělá.</span><span class="sxs-lookup"><span data-stu-id="32c94-269">The code comments provide details about what the code does.</span></span>

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

## <a name="test-the-windows-forms-application"></a><span data-ttu-id="32c94-270">Testování aplikace model Windows Forms</span><span class="sxs-lookup"><span data-stu-id="32c94-270">Test the Windows Forms Application</span></span>

-   <span data-ttu-id="32c94-271">Zkompilujte a spusťte aplikaci a můžete otestovat její funkčnost.</span><span class="sxs-lookup"><span data-stu-id="32c94-271">Compile and run the application and you can test out the functionality.</span></span>

    ![Formulář 1 před uložením](~/ef6/media/form1beforesave.png)

-   <span data-ttu-id="32c94-273">Po uložení klíčů vygenerovaných úložiště se zobrazí na obrazovce.</span><span class="sxs-lookup"><span data-stu-id="32c94-273">After saving the store generated keys are shown on the screen.</span></span>

    ![Formulář 1 po uložení](~/ef6/media/form1aftersave.png)

-   <span data-ttu-id="32c94-275">Pokud jste použili Code First, uvidíte také, že se pro vás vytvoří databáze **WinFormswithEFSample. ProductContext** .</span><span class="sxs-lookup"><span data-stu-id="32c94-275">If you used Code First, then you will also see that a **WinFormswithEFSample.ProductContext** database is created for you.</span></span>

    ![Průzkumník objektů serveru](~/ef6/media/serverobjexplorer.png)
