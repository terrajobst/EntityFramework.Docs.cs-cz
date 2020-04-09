---
title: Datová vazba s WPF - EF6
author: divega
ms.date: 04/02/2020
ms.assetid: e90d48e6-bea7785-47ef-b756-7b89cce4daf0
ms.openlocfilehash: 6908e2a7597d0c199620c6015ed3ea06226c5ea9
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/03/2020
ms.locfileid: "80639147"
---
> [!IMPORTANT]
> <span data-ttu-id="52d5a-102">**Tento dokument je platný pouze pro wpf v rozhraní .NET Framework.**</span><span class="sxs-lookup"><span data-stu-id="52d5a-102">**This document is valid for WPF on the .NET Framework only**</span></span>
>
> <span data-ttu-id="52d5a-103">Tento dokument popisuje datové vazby pro WPF v rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="52d5a-103">This document describes databinding for WPF on the .NET Framework.</span></span> <span data-ttu-id="52d5a-104">Pro nové projekty .NET Core doporučujeme použít [EF Core](/ef/core) namísto entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="52d5a-104">For new .NET Core projects, we recommend you use [EF Core](/ef/core) instead of Entity Framework 6.</span></span> <span data-ttu-id="52d5a-105">Dokumentace pro datové vazby v EF Core je sledována v [problém #778](https://github.com/dotnet/EntityFramework.Docs/issues/778).</span><span class="sxs-lookup"><span data-stu-id="52d5a-105">The documentation for databinding in EF Core is tracked in [Issue #778](https://github.com/dotnet/EntityFramework.Docs/issues/778).</span></span>

# <a name="databinding-with-wpf"></a><span data-ttu-id="52d5a-106">Datová vazba s WPF</span><span class="sxs-lookup"><span data-stu-id="52d5a-106">Databinding with WPF</span></span>
<span data-ttu-id="52d5a-107">Tento podrobný návod ukazuje, jak svázat typy POCO s ovládacími prvky WPF ve formuláři "hlavní podrobnosti".</span><span class="sxs-lookup"><span data-stu-id="52d5a-107">This step-by-step walkthrough shows how to bind POCO types to WPF controls in a “master-detail" form.</span></span> <span data-ttu-id="52d5a-108">Aplikace používá rozhraní API entity framework k naplnění objektů daty z databáze, sledování změn a zachování dat do databáze.</span><span class="sxs-lookup"><span data-stu-id="52d5a-108">The application uses the Entity Framework APIs to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="52d5a-109">Model definuje dva typy, které se účastní vztahu 1:N: **Kategorie** \\(hlavní\\hlavní) a **Product** (závislý detail).</span><span class="sxs-lookup"><span data-stu-id="52d5a-109">The model defines two types that participate in one-to-many relationship: **Category** (principal\\master) and **Product** (dependent\\detail).</span></span> <span data-ttu-id="52d5a-110">Potom Visual Studio nástroje se používají k vazbě typy definované v modelu wpf ovládací prvky.</span><span class="sxs-lookup"><span data-stu-id="52d5a-110">Then, the Visual Studio tools are used to bind the types defined in the model to the WPF controls.</span></span> <span data-ttu-id="52d5a-111">WPF rozhraní pro vázání dat umožňuje navigaci mezi souvisejícíobjekty: výběr řádků v hlavním zobrazení způsobí, že zobrazení podrobností aktualizovat s odpovídající podřízené údaje.</span><span class="sxs-lookup"><span data-stu-id="52d5a-111">The WPF data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="52d5a-112">Snímky obrazovky a výpisy kódu v tomto návodu jsou převzaty z Visual Studia 2013, ale tento návod můžete dokončit pomocí sady Visual Studio 2012 nebo Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="52d5a-112">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a><span data-ttu-id="52d5a-113">Použití možnosti "Objekt" pro vytváření zdrojů dat WPF</span><span class="sxs-lookup"><span data-stu-id="52d5a-113">Use the 'Object' Option for Creating WPF Data Sources</span></span>

<span data-ttu-id="52d5a-114">S předchozí verzí entity framework jsme použili k doporučení používat **možnost Databáze** při vytváření nového zdroje dat na základě modelu vytvořeného pomocí EF Designer.</span><span class="sxs-lookup"><span data-stu-id="52d5a-114">With previous version of Entity Framework we used to recommend using the **Database** option when creating a new Data Source based on a model created with the EF Designer.</span></span> <span data-ttu-id="52d5a-115">Důvodem bylo, že návrhář by generovat kontext, který je odvozen z ObjectContext a entity třídy, které jsou odvozeny z EntityObject.</span><span class="sxs-lookup"><span data-stu-id="52d5a-115">This was because the designer would generate a context that derived from ObjectContext and entity classes that derived from EntityObject.</span></span> <span data-ttu-id="52d5a-116">Použití možnost databáze by vám pomůže napsat nejlepší kód pro interakci s tímto povrchem rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="52d5a-116">Using the Database option would help you write the best code for interacting with this API surface.</span></span>

<span data-ttu-id="52d5a-117">Návrháři EF pro Visual Studio 2012 a Visual Studio 2013 generovat kontext, který je odvozen z DbContext spolu s jednoduchými třídami entit POCO.</span><span class="sxs-lookup"><span data-stu-id="52d5a-117">The EF Designers for Visual Studio 2012 and Visual Studio 2013 generate a context that derives from DbContext together with simple POCO entity classes.</span></span> <span data-ttu-id="52d5a-118">S Visual Studio 2010 doporučujeme prohození na šablonu generování kódu, která používá DbContext, jak je popsáno dále v tomto návodu.</span><span class="sxs-lookup"><span data-stu-id="52d5a-118">With Visual Studio 2010 we recommend swapping to a code generation template that uses DbContext as described later in this walkthrough.</span></span>

<span data-ttu-id="52d5a-119">Při použití povrchu rozhraní API DbContext byste měli použít **možnost Object** při vytváření nového zdroje dat, jak je znázorněno v tomto návodu.</span><span class="sxs-lookup"><span data-stu-id="52d5a-119">When using the DbContext API surface you should use the **Object** option when creating a new Data Source, as shown in this walkthrough.</span></span>

<span data-ttu-id="52d5a-120">V případě potřeby se můžete [vrátit k generování kódu založeného na ObjektContext](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) pro modely vytvořené pomocí EF Designer.</span><span class="sxs-lookup"><span data-stu-id="52d5a-120">If needed, you can [revert to ObjectContext based code generation](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) for models created with the EF Designer.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="52d5a-121">Požadavky</span><span class="sxs-lookup"><span data-stu-id="52d5a-121">Pre-Requisites</span></span>

<span data-ttu-id="52d5a-122">K dokončení tohoto návodu je potřeba mít nainstalovanou Visual Studio 2013, Visual Studio 2012 nebo Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="52d5a-122">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="52d5a-123">Pokud používáte Visual Studio 2010, budete muset také nainstalovat NuGet.</span><span class="sxs-lookup"><span data-stu-id="52d5a-123">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="52d5a-124">Další informace naleznete [v tématu Instalace NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).</span><span class="sxs-lookup"><span data-stu-id="52d5a-124">For more information, see [Installing NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).</span></span>  

## <a name="create-the-application"></a><span data-ttu-id="52d5a-125">Vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="52d5a-125">Create the Application</span></span>

-   <span data-ttu-id="52d5a-126">Otevřete sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="52d5a-126">Open Visual Studio</span></span>
-   <span data-ttu-id="52d5a-127">**Soubor&gt; -&gt; Nový - Projekt....**</span><span class="sxs-lookup"><span data-stu-id="52d5a-127">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="52d5a-128">V levém podokně a WPFApplication v pravém podokně vyberte **Windows** a **WPFApplication.**</span><span class="sxs-lookup"><span data-stu-id="52d5a-128">Select **Windows** in the left pane and **WPFApplication** in the right pane</span></span>
-   <span data-ttu-id="52d5a-129">Jako název zadejte **WPFwithEFSample.** </span><span class="sxs-lookup"><span data-stu-id="52d5a-129">Enter **WPFwithEFSample** as the name</span></span>
-   <span data-ttu-id="52d5a-130">Vybrat **OK**</span><span class="sxs-lookup"><span data-stu-id="52d5a-130">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="52d5a-131">Instalace balíčku Entity Framework NuGet</span><span class="sxs-lookup"><span data-stu-id="52d5a-131">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="52d5a-132">V Průzkumníku řešení klikněte pravým tlačítkem myši na projekt **WinFormswithEFSample**</span><span class="sxs-lookup"><span data-stu-id="52d5a-132">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="52d5a-133">Vyberte **Spravovat balíčky NuGet...**</span><span class="sxs-lookup"><span data-stu-id="52d5a-133">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="52d5a-134">V dialogovém okně Spravovat balíčky NuGet vyberte kartu **Online** a zvolte **balíček EntityFramework.**</span><span class="sxs-lookup"><span data-stu-id="52d5a-134">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="52d5a-135">Klikněte na **Instalovat.**</span><span class="sxs-lookup"><span data-stu-id="52d5a-135">Click **Install**</span></span>  
    >[!NOTE]
    > <span data-ttu-id="52d5a-136">Kromě entityFramework sestavení je také přidán odkaz na System.ComponentModel.DataAnnotations.</span><span class="sxs-lookup"><span data-stu-id="52d5a-136">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="52d5a-137">Pokud projekt obsahuje odkaz na System.Data.Entity, pak bude odebrán při instalaci balíčku EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="52d5a-137">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="52d5a-138">Sestavení System.Data.Entity se již nepoužívá pro aplikace entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="52d5a-138">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="define-a-model"></a><span data-ttu-id="52d5a-139">Definování modelu</span><span class="sxs-lookup"><span data-stu-id="52d5a-139">Define a Model</span></span>

<span data-ttu-id="52d5a-140">V tomto návodu můžete zvolit implementaci modelu pomocí Code First nebo EF Designer.</span><span class="sxs-lookup"><span data-stu-id="52d5a-140">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="52d5a-141">Dokončete jednu ze dvou následujících částí.</span><span class="sxs-lookup"><span data-stu-id="52d5a-141">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="52d5a-142">Možnost 1: Definujte model pomocí kódu jako první</span><span class="sxs-lookup"><span data-stu-id="52d5a-142">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="52d5a-143">Tato část ukazuje, jak vytvořit model a jeho přidružené databáze pomocí Code First.</span><span class="sxs-lookup"><span data-stu-id="52d5a-143">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="52d5a-144">Přejít na další část **(Možnost 2: Definujte model pomocí databáze první),** pokud byste raději použít Database First pro zpětnou analýzu modelu z databáze pomocí návrháře EF</span><span class="sxs-lookup"><span data-stu-id="52d5a-144">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="52d5a-145">Při použití vývoje Code First obvykle začínáte zápisem tříd rozhraní .NET Framework, které definují váš koncepční (doménový) model.</span><span class="sxs-lookup"><span data-stu-id="52d5a-145">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="52d5a-146">Přidejte novou třídu do **wpfwithEFSample:**</span><span class="sxs-lookup"><span data-stu-id="52d5a-146">Add a new class to the **WPFwithEFSample:**</span></span>
    -   <span data-ttu-id="52d5a-147">Klikněte pravým tlačítkem myši na název projektu</span><span class="sxs-lookup"><span data-stu-id="52d5a-147">Right-click on the project name</span></span>
    -   <span data-ttu-id="52d5a-148">Vyberte **Přidat**a pak **Novou položku.**</span><span class="sxs-lookup"><span data-stu-id="52d5a-148">Select **Add**, then **New Item**</span></span>
    -   <span data-ttu-id="52d5a-149">Vyberte **Třídu** a zadejte **produkt** pro název třídy.</span><span class="sxs-lookup"><span data-stu-id="52d5a-149">Select **Class** and enter **Product** for the class name</span></span>
-   <span data-ttu-id="52d5a-150">Nahraďte definici **třídy produktu** následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="52d5a-150">Replace the **Product** class definition with the following code:</span></span>

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

<span data-ttu-id="52d5a-151">**Vlastnost Products** ve vlastnosti **Category** a **Category** ve třídě **Product** jsou navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="52d5a-151">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="52d5a-152">V rozhraní Entity Framework poskytují navigační vlastnosti způsob navigace pro navigaci ve vztahu mezi dvěma typy entit.</span><span class="sxs-lookup"><span data-stu-id="52d5a-152">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="52d5a-153">Kromě definování entit je třeba definovat třídu, která je odvozena od&lt;DbContext a zpřístupňuje vlastnosti DbSet TEntity.&gt;</span><span class="sxs-lookup"><span data-stu-id="52d5a-153">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="52d5a-154">Vlastnosti&lt;DbSet&gt; TEntity umožňují kontextu vědět, které typy chcete zahrnout do modelu.</span><span class="sxs-lookup"><span data-stu-id="52d5a-154">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="52d5a-155">Instance odvozeného typu DbContext spravuje objekty entity za běhu, který zahrnuje naplnění objektů s daty z databáze, sledování změn a uchování dat do databáze.</span><span class="sxs-lookup"><span data-stu-id="52d5a-155">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="52d5a-156">Přidejte do projektu novou třídu **ProductContext** s následující definicí:</span><span class="sxs-lookup"><span data-stu-id="52d5a-156">Add a new **ProductContext** class to the project with the following definition:</span></span>

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

<span data-ttu-id="52d5a-157">Zkompilujte projekt.</span><span class="sxs-lookup"><span data-stu-id="52d5a-157">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="52d5a-158">Možnost 2: Definování modelu pomocí databáze jako první</span><span class="sxs-lookup"><span data-stu-id="52d5a-158">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="52d5a-159">Tato část ukazuje, jak pomocí database first zpětnou analýzu modelu z databáze pomocí ef návrháře.</span><span class="sxs-lookup"><span data-stu-id="52d5a-159">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="52d5a-160">Pokud jste dokončili předchozí část **(Možnost 1: Definujte model pomocí kódu jako první)**, přeskočte tuto část a přejděte přímo do části **Opožděné načítání.**</span><span class="sxs-lookup"><span data-stu-id="52d5a-160">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="52d5a-161">Vytvoření existující databáze</span><span class="sxs-lookup"><span data-stu-id="52d5a-161">Create an Existing Database</span></span>

<span data-ttu-id="52d5a-162">Obvykle, když cílíte na existující databázi, bude již vytvořena, ale pro tento návod musíme vytvořit databázi pro přístup.</span><span class="sxs-lookup"><span data-stu-id="52d5a-162">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="52d5a-163">Databázový server nainstalovaný v sadě Visual Studio se liší v závislosti na nainstalované verzi sady Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="52d5a-163">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="52d5a-164">Pokud používáte Visual Studio 2010, budete vytvářet databázi SQL Express.</span><span class="sxs-lookup"><span data-stu-id="52d5a-164">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="52d5a-165">Pokud používáte Visual Studio 2012 pak budete vytvářet databázi [LocalDB.](https://msdn.microsoft.com/library/hh510202.aspx)</span><span class="sxs-lookup"><span data-stu-id="52d5a-165">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="52d5a-166">Pojďme do toho a vygenerujme databázi.</span><span class="sxs-lookup"><span data-stu-id="52d5a-166">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="52d5a-167">**Zobrazení&gt; – Průzkumník serveru**</span><span class="sxs-lookup"><span data-stu-id="52d5a-167">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="52d5a-168">Klikněte pravým tlačítkem na **Datová připojení -&gt; Přidat připojení...**</span><span class="sxs-lookup"><span data-stu-id="52d5a-168">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="52d5a-169">Pokud jste se nepřipojili k databázi z Průzkumníka serveru, budete muset jako zdroj dat vybrat Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="52d5a-169">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Změnit zdroj dat](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="52d5a-171">Připojte se k LocalDB nebo SQL Express, v závislosti na tom, který z nich jste nainstalovali, a zadejte **produkty** jako název databáze</span><span class="sxs-lookup"><span data-stu-id="52d5a-171">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![Přidat místní databázi připojení](~/ef6/media/addconnectionlocaldb.png)

    ![Přidat expres připojení](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="52d5a-174">Vyberte **OK** a budete dotázáni, zda chcete vytvořit novou databázi, vyberte **Ano**</span><span class="sxs-lookup"><span data-stu-id="52d5a-174">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Create Database](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="52d5a-176">Nová databáze se nyní zobrazí v Průzkumníkovi serveru, klikněte na ni pravým tlačítkem myši a vyberte **Nový dotaz**</span><span class="sxs-lookup"><span data-stu-id="52d5a-176">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="52d5a-177">Zkopírujte následující SQL do nového dotazu, klikněte pravým tlačítkem myši na dotaz a vyberte **Spustit**</span><span class="sxs-lookup"><span data-stu-id="52d5a-177">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

#### <a name="reverse-engineer-model"></a><span data-ttu-id="52d5a-178">Model zpětné analýzy</span><span class="sxs-lookup"><span data-stu-id="52d5a-178">Reverse Engineer Model</span></span>

<span data-ttu-id="52d5a-179">Budeme využívat Návrhář rozhraní entity, který je součástí sady Visual Studio, k vytvoření našeho modelu.</span><span class="sxs-lookup"><span data-stu-id="52d5a-179">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="52d5a-180">**Projekt&gt; – přidat novou položku...**</span><span class="sxs-lookup"><span data-stu-id="52d5a-180">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="52d5a-181">V yberte **Data** z levé nabídky a potom **ADO.NET datový model entity.**</span><span class="sxs-lookup"><span data-stu-id="52d5a-181">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="52d5a-182">Jako název zadejte **ProductModel** a klepněte na **ok.**</span><span class="sxs-lookup"><span data-stu-id="52d5a-182">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="52d5a-183">Tím se spustí **Průvodce datovým modelem entity.**</span><span class="sxs-lookup"><span data-stu-id="52d5a-183">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="52d5a-184">Vyberte **Generovat z databáze** a klepněte na **Další.**</span><span class="sxs-lookup"><span data-stu-id="52d5a-184">Select **Generate from Database** and click **Next**</span></span>

    ![Zvolte obsah modelu](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="52d5a-186">Vyberte připojení k databázi, kterou jste vytvořili v první části, zadejte **ProductContext** jako název připojovacího řetězce a klepněte na tlačítko **Další.**</span><span class="sxs-lookup"><span data-stu-id="52d5a-186">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![Vyberte si připojení](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="52d5a-188">Kliknutím na zaškrtávací políčko vedle položky Tabulky importujete všechny tabulky a kliknete na tlačítko Dokončit.</span><span class="sxs-lookup"><span data-stu-id="52d5a-188">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Vyberte si své objekty](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="52d5a-190">Po dokončení procesu zpětné analýzy je nový model přidán do projektu a otevřen pro zobrazení v návrháři architektury entit.</span><span class="sxs-lookup"><span data-stu-id="52d5a-190">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="52d5a-191">Do projektu byl také přidán soubor App.config s podrobnostmi o připojení pro databázi.</span><span class="sxs-lookup"><span data-stu-id="52d5a-191">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="52d5a-192">Další kroky v sadě Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="52d5a-192">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="52d5a-193">Pokud pracujete v sadě Visual Studio 2010, budete muset aktualizovat návrháře EF, aby používal generování kódu EF6.</span><span class="sxs-lookup"><span data-stu-id="52d5a-193">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="52d5a-194">Klikněte pravým tlačítkem myši na prázdné místo modelu v návrháři EF a vyberte **Přidat položku generování kódu...**</span><span class="sxs-lookup"><span data-stu-id="52d5a-194">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="52d5a-195">V levé nabídce **vyberte online šablony** a vyhledejte **dbcontext.**</span><span class="sxs-lookup"><span data-stu-id="52d5a-195">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="52d5a-196">Vyberte **generátor EF 6.x\#DbContext pro C ,** zadejte jako název Název **ProductsModel** a klepněte na tlačítko Přidat</span><span class="sxs-lookup"><span data-stu-id="52d5a-196">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="52d5a-197">Aktualizace generování kódu pro datová vazba</span><span class="sxs-lookup"><span data-stu-id="52d5a-197">Updating code generation for data binding</span></span>

<span data-ttu-id="52d5a-198">EF generuje kód z vašeho modelu pomocí šablon T4.</span><span class="sxs-lookup"><span data-stu-id="52d5a-198">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="52d5a-199">Šablony dodávané v sadě Visual Studio nebo stažené z galerie sady Visual Studio jsou určeny pro běžné použití.</span><span class="sxs-lookup"><span data-stu-id="52d5a-199">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="52d5a-200">To znamená, že entity generované z těchto šablon&lt;&gt; mají jednoduché vlastnosti ICollection T.</span><span class="sxs-lookup"><span data-stu-id="52d5a-200">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="52d5a-201">Však při provádění datové vazby pomocí WPF je žádoucí použít **ObservableCollection** pro vlastnosti kolekce tak, aby WPF můžete sledovat změny provedené v kolekcích.</span><span class="sxs-lookup"><span data-stu-id="52d5a-201">However, when doing data binding using WPF it is desirable to use **ObservableCollection** for collection properties so that WPF can keep track of changes made to the collections.</span></span> <span data-ttu-id="52d5a-202">Za tímto účelem budeme upravovat šablony používat ObservableCollection.</span><span class="sxs-lookup"><span data-stu-id="52d5a-202">To this end we will to modify the templates to use ObservableCollection.</span></span>

-   <span data-ttu-id="52d5a-203">Otevřete **Průzkumníka řešení** a najděte soubor **ProductModel.edmx**</span><span class="sxs-lookup"><span data-stu-id="52d5a-203">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="52d5a-204">Vyhledání **souboru ProductModel.tt,** který bude vnořen do souboru ProductModel.edmx</span><span class="sxs-lookup"><span data-stu-id="52d5a-204">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![Šablona modelu produktu WPF](~/ef6/media/wpfproductmodeltemplate.png)

-   <span data-ttu-id="52d5a-206">Poklepáním na soubor ProductModel.tt soubor otevřete v editoru Sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="52d5a-206">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="52d5a-207">Najděte a nahraďte dva výskyty "**ICollection**" s "**ObservableCollection**".</span><span class="sxs-lookup"><span data-stu-id="52d5a-207">Find and replace the two occurrences of “**ICollection**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="52d5a-208">Ty se nacházejí přibližně na linkách 296 a 484.</span><span class="sxs-lookup"><span data-stu-id="52d5a-208">These are located approximately at lines 296 and 484.</span></span>
-   <span data-ttu-id="52d5a-209">Najděte a nahraďte první výskyt "**HashSet**" "**observablecollection**".</span><span class="sxs-lookup"><span data-stu-id="52d5a-209">Find and replace the first occurrence of “**HashSet**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="52d5a-210">Tento výskyt se nachází přibližně na řádku 50.</span><span class="sxs-lookup"><span data-stu-id="52d5a-210">This occurrence is located approximately at line 50.</span></span> <span data-ttu-id="52d5a-211">**Nenahrazujte** druhý výskyt HashSet nalezen později v kódu.</span><span class="sxs-lookup"><span data-stu-id="52d5a-211">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="52d5a-212">Najděte a nahraďte jediný výskyt "**System.Collections.Generic**" s "**System.Collections.ObjectModel**".</span><span class="sxs-lookup"><span data-stu-id="52d5a-212">Find and replace the only occurrence of “**System.Collections.Generic**” with “**System.Collections.ObjectModel**”.</span></span> <span data-ttu-id="52d5a-213">Nachází se přibližně na lince 424.</span><span class="sxs-lookup"><span data-stu-id="52d5a-213">This is located approximately at line 424.</span></span>
-   <span data-ttu-id="52d5a-214">Uložte soubor ProductModel.tt.</span><span class="sxs-lookup"><span data-stu-id="52d5a-214">Save the ProductModel.tt file.</span></span> <span data-ttu-id="52d5a-215">To by mělo způsobit kód pro entity, které mají být obnoveny.</span><span class="sxs-lookup"><span data-stu-id="52d5a-215">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="52d5a-216">Pokud se kód neregeneruje automaticky, klikněte pravým tlačítkem myši na ProductModel.tt a zvolte "Spustit vlastní nástroj".</span><span class="sxs-lookup"><span data-stu-id="52d5a-216">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="52d5a-217">Pokud nyní otevřete soubor Category.cs (který je vnořený pod ProductModel.tt), měli byste vidět, že kolekce Products má typ **ObservableCollection&lt;Product&gt;**.</span><span class="sxs-lookup"><span data-stu-id="52d5a-217">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableCollection&lt;Product&gt;**.</span></span>

<span data-ttu-id="52d5a-218">Zkompilujte projekt.</span><span class="sxs-lookup"><span data-stu-id="52d5a-218">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="52d5a-219">Opožděné načítání</span><span class="sxs-lookup"><span data-stu-id="52d5a-219">Lazy Loading</span></span>

<span data-ttu-id="52d5a-220">**Vlastnost Products** ve vlastnosti **Category** a **Category** ve třídě **Product** jsou navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="52d5a-220">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="52d5a-221">V rozhraní Entity Framework poskytují navigační vlastnosti způsob navigace pro navigaci ve vztahu mezi dvěma typy entit.</span><span class="sxs-lookup"><span data-stu-id="52d5a-221">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="52d5a-222">EF umožňuje automatické načítání souvisejících entit z databáze při prvním přístupu k vlastnosti navigace.</span><span class="sxs-lookup"><span data-stu-id="52d5a-222">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="52d5a-223">S tímto typem načítání (tzv. opožděné načítání), uvědomte si, že při prvním přístupu ke každé navigační vlastnosti bude proveden samostatný dotaz proti databázi, pokud obsah již není v kontextu.</span><span class="sxs-lookup"><span data-stu-id="52d5a-223">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="52d5a-224">Při použití typů entit POCO ef dosahuje opožděné načítání vytvořením instance odvozených typů proxy během běhu a pak přepsání virtuální vlastnosti ve vašich třídách přidat zatížení háku.</span><span class="sxs-lookup"><span data-stu-id="52d5a-224">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="52d5a-225">Chcete-li získat opožděné načítání souvisejících objektů, musíte deklarovat vlastnosti navigace getters jako **veřejné** a **virtuální** **(Overridable** v jazyce Visual Basic) a třídy nesmí být **zapečetěné** **(NotOverridable** v jazyce Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="52d5a-225">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="52d5a-226">Při použití databáze první navigační vlastnosti jsou automaticky virtuální povolit opožděné načítání.</span><span class="sxs-lookup"><span data-stu-id="52d5a-226">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="52d5a-227">V části Kód první jsme se rozhodli, aby navigační vlastnosti virtuální ze stejného důvodu</span><span class="sxs-lookup"><span data-stu-id="52d5a-227">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="52d5a-228">Vazba objektu na ovládací prvky</span><span class="sxs-lookup"><span data-stu-id="52d5a-228">Bind Object to Controls</span></span>

<span data-ttu-id="52d5a-229">Přidejte třídy, které jsou definovány v modelu jako zdroje dat pro tuto aplikaci WPF.</span><span class="sxs-lookup"><span data-stu-id="52d5a-229">Add the classes that are defined in the model as data sources for this WPF application.</span></span>

-   <span data-ttu-id="52d5a-230">Poklepáním na **soubor MainWindow.xaml** v Průzkumníku řešení otevřete hlavní formulář.</span><span class="sxs-lookup"><span data-stu-id="52d5a-230">Double-click **MainWindow.xaml** in Solution Explorer to open the main form</span></span>
-   <span data-ttu-id="52d5a-231">V hlavní nabídce vyberte **Project -&gt; Add New Data Source ...**</span><span class="sxs-lookup"><span data-stu-id="52d5a-231">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="52d5a-232">(V sadě Visual Studio 2010 je potřeba vybrat **data –&gt; přidat nový zdroj dat...**)</span><span class="sxs-lookup"><span data-stu-id="52d5a-232">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="52d5a-233">V okně Zvolit typ zdroje dat vyberte **Objekt** a klepněte na **Další.**</span><span class="sxs-lookup"><span data-stu-id="52d5a-233">In the Choose a Data Source Typewindow, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="52d5a-234">V dialogovém okně Vybrat datové objekty rozložte **wpfsEFsample** dvakrát a vyberte **kategorii**</span><span class="sxs-lookup"><span data-stu-id="52d5a-234">In the Select the Data Objects dialog, unfold the **WPFwithEFSample** two times and select **Category**</span></span>  
    <span data-ttu-id="52d5a-235">*Není nutné vybírat zdroj dat **produktu,** protože se k němu dostaneme prostřednictvím **vlastnosti produktu**ve zdroji dat **kategorie.***</span><span class="sxs-lookup"><span data-stu-id="52d5a-235">*There is no need to select the **Product** data source, because we will get to it through the **Product**’s property on the **Category** data source*</span></span>  

    ![Vybrat datové objekty](~/ef6/media/selectdataobjects.png)

-   <span data-ttu-id="52d5a-237">Klikněte na **Dokončit.**</span><span class="sxs-lookup"><span data-stu-id="52d5a-237">Click **Finish.**</span></span>
-   <span data-ttu-id="52d5a-238">Okno Zdroje dat se otevře vedle okna MainWindow.xaml \*Pokud se okno Zdroje dat nezobrazuje, vyberte Zobrazit – \*\*&gt; jiné zdroje dat windows.&gt; \*\* \*</span><span class="sxs-lookup"><span data-stu-id="52d5a-238">The Data Sources window is opened next to the MainWindow.xaml window *If the Data Sources window is not showing up, select **View -&gt; Other Windows-&gt; Data Sources***</span></span>
-   <span data-ttu-id="52d5a-239">Stiskněte ikonu špendlíku, aby se okno Zdroje dat automaticky neskrývalo.</span><span class="sxs-lookup"><span data-stu-id="52d5a-239">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="52d5a-240">Možná budete muset stisknout tlačítko aktualizace, pokud okno již bylo viditelné.</span><span class="sxs-lookup"><span data-stu-id="52d5a-240">You may need to hit the refresh button if the window was already visible.</span></span>

    ![Zdroje dat](~/ef6/media/datasources.png)

-   <span data-ttu-id="52d5a-242">Vyberte zdroj dat **kategorie** a přetáhněte jej do formuláře.</span><span class="sxs-lookup"><span data-stu-id="52d5a-242">Select the **Category** data source and drag it on the form.</span></span>

<span data-ttu-id="52d5a-243">Při přetažení tohoto zdroje došlo k následujícímu:</span><span class="sxs-lookup"><span data-stu-id="52d5a-243">The following happened when we dragged this source:</span></span>

-   <span data-ttu-id="52d5a-244">Prostředek **categoryViewSource** a ovládací prvek **categoryDataGrid** byly přidány do xaml.</span><span class="sxs-lookup"><span data-stu-id="52d5a-244">The **categoryViewSource** resource and the **categoryDataGrid** control were added to XAML</span></span> 
-   <span data-ttu-id="52d5a-245">Vlastnost DataContext v nadřazeném elementu Grid byla nastavena na "{StaticResource **categoryViewSource** }".</span><span class="sxs-lookup"><span data-stu-id="52d5a-245">The DataContext property on the parent Grid element was set to "{StaticResource **categoryViewSource** }".</span></span><span data-ttu-id="52d5a-246">Prostředek **categoryViewSource** slouží jako zdroj vazby pro vnější\\nadřazený element Grid.</span><span class="sxs-lookup"><span data-stu-id="52d5a-246"> The **categoryViewSource** resource serves as a binding source for the outer\\parent Grid element.</span></span> <span data-ttu-id="52d5a-247">Vnitřní Elementy Grid pak dědí hodnotu DataContext z nadřazené mřížky (vlastnost CategoryDataGrid ItemsSource je nastavena na {Binding})The inner Grid elements then inherit the DataContext value from the parent Grid (the categoryDataGrid's ItemsSource vlastnost is set to "{Binding}")</span><span class="sxs-lookup"><span data-stu-id="52d5a-247">The inner Grid elements then inherit the DataContext value from the parent Grid (the categoryDataGrid’s ItemsSource property is set to "{Binding}")</span></span>

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

## <a name="adding-a-details-grid"></a><span data-ttu-id="52d5a-248">Přidání mřížky podrobností</span><span class="sxs-lookup"><span data-stu-id="52d5a-248">Adding a Details Grid</span></span>

<span data-ttu-id="52d5a-249">Nyní, když máme mřížku pro zobrazení kategorie, přidáme mřížku podrobností, která zobrazí přidružené produkty.</span><span class="sxs-lookup"><span data-stu-id="52d5a-249">Now that we have a grid to display Categories let's add a details grid to display the associated Products.</span></span>

-   <span data-ttu-id="52d5a-250">Vyberte vlastnost **Products** z části zdroje dat **Kategorie** a přetáhněte ji do formuláře.</span><span class="sxs-lookup"><span data-stu-id="52d5a-250">Select the **Products** property from under the **Category** data source and drag it on the form.</span></span>
    -   <span data-ttu-id="52d5a-251">Do xaml jsou přidány mřížky prostředku a produktu ProductDataGrid **kategorieProductsViewSource** a **productDataGrid.**</span><span class="sxs-lookup"><span data-stu-id="52d5a-251">The **categoryProductsViewSource** resource and **productDataGrid** grid are added to XAML</span></span>
    -   <span data-ttu-id="52d5a-252">Cesta vazby pro tento prostředek je nastavena na produkty</span><span class="sxs-lookup"><span data-stu-id="52d5a-252">The binding path for this resource is set to Products</span></span>
    -   <span data-ttu-id="52d5a-253">WPF rozhraní pro vázání dat zajišťuje, že pouze produkty související s vybranou kategorií se zobrazí v **produktu DataGrid**</span><span class="sxs-lookup"><span data-stu-id="52d5a-253">WPF data-binding framework ensures that only Products related to the selected Category show up in **productDataGrid**</span></span>
-   <span data-ttu-id="52d5a-254">Z panelu nástrojů přetáhněte **tlačítko** do formuláře.</span><span class="sxs-lookup"><span data-stu-id="52d5a-254">From the Toolbox, drag **Button** on to the form.</span></span> <span data-ttu-id="52d5a-255">Nastavte vlastnost **Name** na **tlačítkoUložit** a vlastnost **Obsah** **uložit**.</span><span class="sxs-lookup"><span data-stu-id="52d5a-255">Set the **Name** property to **buttonSave** and the **Content** property to **Save**.</span></span>

<span data-ttu-id="52d5a-256">Formulář by měl vypadat podobně jako tento:</span><span class="sxs-lookup"><span data-stu-id="52d5a-256">The form should look similar to this:</span></span>

![Designer](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a><span data-ttu-id="52d5a-258">Přidat kód, který zpracovává interakci dat</span><span class="sxs-lookup"><span data-stu-id="52d5a-258">Add Code that Handles Data Interaction</span></span>

<span data-ttu-id="52d5a-259">Je čas přidat některé obslužné rutiny událostí do hlavního okna.</span><span class="sxs-lookup"><span data-stu-id="52d5a-259">It's time to add some event handlers to the main window.</span></span>

-   <span data-ttu-id="52d5a-260">V okně XAML klikněte \*\* &lt;\*\* na element Window, který vybere hlavní okno</span><span class="sxs-lookup"><span data-stu-id="52d5a-260">In the XAML window, click on the **&lt;Window** element, this selects the main window</span></span>
-   <span data-ttu-id="52d5a-261">V okně **Vlastnosti** zvolte **Události** v pravém horním rohu a potom poklepejte na textové pole vpravo od popisku **Načteno.**</span><span class="sxs-lookup"><span data-stu-id="52d5a-261">In the **Properties** window choose **Events** at the top right, then double-click the text box to right of the **Loaded** label</span></span>

    ![Vlastnosti hlavního okna](~/ef6/media/mainwindowproperties.png)

-   <span data-ttu-id="52d5a-263">Přidejte také událost **Click** pro tlačítko **Uložit** poklepáním na tlačítko Uložit v návrháři.</span><span class="sxs-lookup"><span data-stu-id="52d5a-263">Also add the **Click** event for the **Save** button by double-clicking the Save button in the designer.</span></span> 

<span data-ttu-id="52d5a-264">Tím se dostanete do kódu za formuláře, budeme nyní upravit kód použít ProductContext k provedení přístupu k datům.</span><span class="sxs-lookup"><span data-stu-id="52d5a-264">This brings you to the code behind for the form, we'll now edit the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="52d5a-265">Aktualizujte kód pro MainWindow, jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="52d5a-265">Update the code for the MainWindow as shown below.</span></span>

<span data-ttu-id="52d5a-266">Kód deklaruje dlouhotrvající instanci **ProductContext**.</span><span class="sxs-lookup"><span data-stu-id="52d5a-266">The code declares a long-running instance of **ProductContext**.</span></span> <span data-ttu-id="52d5a-267">**ProductContext** Objekt se používá k dotazování a ukládání dat do databáze.</span><span class="sxs-lookup"><span data-stu-id="52d5a-267">The **ProductContext** object is used to query and save data to the database.</span></span> <span data-ttu-id="52d5a-268">**Dispose()** na **ProductContext** instance je pak volána z přepsané **OnClosing** metoda.</span><span class="sxs-lookup"><span data-stu-id="52d5a-268">The **Dispose()** on the **ProductContext** instance is then called from the overridden **OnClosing** method.</span></span><span data-ttu-id="52d5a-269">Komentáře kódu poskytují podrobnosti o tom, co kód dělá.</span><span class="sxs-lookup"><span data-stu-id="52d5a-269"> The code comments provide details about what the code does.</span></span>

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

## <a name="test-the-wpf-application"></a><span data-ttu-id="52d5a-270">Testování aplikace WPF</span><span class="sxs-lookup"><span data-stu-id="52d5a-270">Test the WPF Application</span></span>

-   <span data-ttu-id="52d5a-271">Zkompilujte a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="52d5a-271">Compile and run the application.</span></span> <span data-ttu-id="52d5a-272">Pokud jste použili Kód První, pak uvidíte, že **wpfwithEFSample.ProductContext** databáze je vytvořen pro vás.</span><span class="sxs-lookup"><span data-stu-id="52d5a-272">If you used Code First, then you will see that a **WPFwithEFSample.ProductContext** database is created for you.</span></span>
-   <span data-ttu-id="52d5a-273">Zadejte název kategorie do horní mřížky a názvy produktů v dolní mřížce *Nezadávejte nic ve sloupcích ID, protože primární klíč je generován databází.*</span><span class="sxs-lookup"><span data-stu-id="52d5a-273">Enter a category name in the top grid and product names in the bottom grid *Do not enter anything in ID columns, because the primary key is generated by the database*</span></span>

    ![Hlavní okno s novými kategoriemi a produkty](~/ef6/media/screen1.png)

-   <span data-ttu-id="52d5a-275">Stisknutím tlačítka **Uložit** data uložíte do databáze.</span><span class="sxs-lookup"><span data-stu-id="52d5a-275">Press the **Save** button to save the data to the database</span></span>

<span data-ttu-id="52d5a-276">Po volání **DbContext SaveChanges()**, ID jsou naplněny databáze generované hodnoty.</span><span class="sxs-lookup"><span data-stu-id="52d5a-276">After the call to DbContext’s **SaveChanges()**, the IDs are populated with the database generated values.</span></span> <span data-ttu-id="52d5a-277">Protože jsme **volali Refresh()** po **SaveChanges()** Ovládací prvky **DataGrid** jsou aktualizovány s novými hodnotami také.</span><span class="sxs-lookup"><span data-stu-id="52d5a-277">Because we called **Refresh()** after **SaveChanges()** the **DataGrid** controls are updated with the new values as well.</span></span>

![Hlavní okno s vyplněnými ID](~/ef6/media/screen2.png)

## <a name="additional-resources"></a><span data-ttu-id="52d5a-279">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="52d5a-279">Additional Resources</span></span>

<span data-ttu-id="52d5a-280">Další informace o datové vazbě na kolekce pomocí WPF naleznete v [tomto tématu](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) v dokumentaci WPF.</span><span class="sxs-lookup"><span data-stu-id="52d5a-280">To learn more about data binding to collections using WPF, see [this topic](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) in the WPF documentation.</span></span>  
