---
title: Začínáme v ASP.NET Core – existující databáze – EF Core
author: rowanmiller
ms.date: 08/02/2018
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: c8acb95395968f710e6b896de6c3598cb7b23676
ms.sourcegitcommit: e66745c9f91258b2cacf5ff263141be3cba4b09e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/06/2019
ms.locfileid: "54058783"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a><span data-ttu-id="6f4fe-102">Začínáme s EF Core v ASP.NET Core s existující databáze</span><span class="sxs-lookup"><span data-stu-id="6f4fe-102">Getting Started with EF Core on ASP.NET Core with an Existing Database</span></span>

<span data-ttu-id="6f4fe-103">V tomto kurzu sestavíte aplikaci ASP.NET Core MVC, která provádí základní přístup k datům pomocí Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="6f4fe-103">In this tutorial, you build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="6f4fe-104">Můžete provést zpětnou analýzu stávající databázi k vytvoření modelu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="6f4fe-104">You reverse engineer an existing database to create an Entity Framework model.</span></span>

<span data-ttu-id="6f4fe-105">[Zobrazit ukázky v tomto článku na Githubu](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).</span><span class="sxs-lookup"><span data-stu-id="6f4fe-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6f4fe-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6f4fe-106">Prerequisites</span></span>

<span data-ttu-id="6f4fe-107">Nainstalujte následující software:</span><span class="sxs-lookup"><span data-stu-id="6f4fe-107">Install the following software:</span></span>

* <span data-ttu-id="6f4fe-108">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) se tyto úlohy:</span><span class="sxs-lookup"><span data-stu-id="6f4fe-108">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="6f4fe-109">**Vývoj pro ASP.NET a web** (v části **Web a Cloud**)</span><span class="sxs-lookup"><span data-stu-id="6f4fe-109">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="6f4fe-110">**Vývoj pro různé platformy .NET core** (v části **další sady nástrojů**)</span><span class="sxs-lookup"><span data-stu-id="6f4fe-110">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="6f4fe-111">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="6f4fe-111">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

## <a name="create-blogging-database"></a><span data-ttu-id="6f4fe-112">Vytvoření databáze blogovací</span><span class="sxs-lookup"><span data-stu-id="6f4fe-112">Create Blogging database</span></span>

<span data-ttu-id="6f4fe-113">Tento kurz používá **blogovací** databáze na instanci LocalDb jako existující databázi.</span><span class="sxs-lookup"><span data-stu-id="6f4fe-113">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span> <span data-ttu-id="6f4fe-114">Pokud jste již vytvořili **blogovací** databáze jako součást další kurz, přeskočit tyto kroky.</span><span class="sxs-lookup"><span data-stu-id="6f4fe-114">If you have already created the **Blogging** database as part of another tutorial, skip these steps.</span></span>

* <span data-ttu-id="6f4fe-115">Otevřít Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6f4fe-115">Open Visual Studio</span></span>
* <span data-ttu-id="6f4fe-116">**Nástroje -> připojení k databázi...**</span><span class="sxs-lookup"><span data-stu-id="6f4fe-116">**Tools -> Connect to Database...**</span></span>
* <span data-ttu-id="6f4fe-117">Vyberte **Microsoft SQL Server** a klikněte na tlačítko **pokračovat**</span><span class="sxs-lookup"><span data-stu-id="6f4fe-117">Select **Microsoft SQL Server** and click **Continue**</span></span>
* <span data-ttu-id="6f4fe-118">Zadejte **(localdb) \mssqllocaldb** jako **název serveru**</span><span class="sxs-lookup"><span data-stu-id="6f4fe-118">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>
* <span data-ttu-id="6f4fe-119">Zadejte **hlavní** jako **název_databáze** a klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="6f4fe-119">Enter **master** as the **Database Name** and click **OK**</span></span>
* <span data-ttu-id="6f4fe-120">Hlavní databázi se nyní zobrazí v části **datová připojení** v **Průzkumníka serveru**</span><span class="sxs-lookup"><span data-stu-id="6f4fe-120">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>
* <span data-ttu-id="6f4fe-121">Klikněte pravým tlačítkem na databázi v **Průzkumníka serveru** a vyberte **nový dotaz**</span><span class="sxs-lookup"><span data-stu-id="6f4fe-121">Right-click on the database in **Server Explorer** and select **New Query**</span></span>
* <span data-ttu-id="6f4fe-122">Zkopírujte skript níže do editoru dotazů</span><span class="sxs-lookup"><span data-stu-id="6f4fe-122">Copy the script listed below into the query editor</span></span>
* <span data-ttu-id="6f4fe-123">Klikněte pravým tlačítkem myši na editor dotazů a vyberte **spouštění**</span><span class="sxs-lookup"><span data-stu-id="6f4fe-123">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="6f4fe-124">Vytvoření nového projektu</span><span class="sxs-lookup"><span data-stu-id="6f4fe-124">Create a new project</span></span>

* <span data-ttu-id="6f4fe-125">Otevřít Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="6f4fe-125">Open Visual Studio 2017</span></span>
* <span data-ttu-id="6f4fe-126">**Soubor > Nový > projekt...**</span><span class="sxs-lookup"><span data-stu-id="6f4fe-126">**File > New > Project...**</span></span>
* <span data-ttu-id="6f4fe-127">V levé nabídce vyberte **nainstalováno > Visual C# > Web**</span><span class="sxs-lookup"><span data-stu-id="6f4fe-127">From the left menu select **Installed > Visual C# > Web**</span></span>
* <span data-ttu-id="6f4fe-128">Vyberte **webové aplikace ASP.NET Core** šablony projektu</span><span class="sxs-lookup"><span data-stu-id="6f4fe-128">Select the **ASP.NET Core Web Application** project template</span></span>
* <span data-ttu-id="6f4fe-129">Zadejte **EFGetStarted.AspNetCore.ExistingDb** název (musí odpovídat přesně oboru názvů, které se později používá v kódu) a klikněte na **OK**</span><span class="sxs-lookup"><span data-stu-id="6f4fe-129">Enter **EFGetStarted.AspNetCore.ExistingDb** as the name (it has to match exactly the namespace later used in the code) and click **OK**</span></span> 
* <span data-ttu-id="6f4fe-130">Počkejte **nová webová aplikace ASP.NET Core** zobrazit dialogové okno</span><span class="sxs-lookup"><span data-stu-id="6f4fe-130">Wait for the **New ASP.NET Core Web Application** dialog to appear</span></span>
* <span data-ttu-id="6f4fe-131">Ujistěte se, že rozevírací seznam cílové rozhraní framework je nastaven na **.NET Core**, a rozevírací seznam verze je nastavena na **ASP.NET Core 2.1**</span><span class="sxs-lookup"><span data-stu-id="6f4fe-131">Make sure that the target framework dropdown is set to **.NET Core**, and the version dropdown is set to **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="6f4fe-132">Vyberte **webové aplikace (Model-View-Controller)** šablony</span><span class="sxs-lookup"><span data-stu-id="6f4fe-132">Select the **Web Application (Model-View-Controller)** template</span></span>
* <span data-ttu-id="6f4fe-133">Ujistěte se, že **ověřování** je nastavena na **bez ověřování**</span><span class="sxs-lookup"><span data-stu-id="6f4fe-133">Ensure that **Authentication** is set to **No Authentication**</span></span>
* <span data-ttu-id="6f4fe-134">Klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="6f4fe-134">Click **OK**</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="6f4fe-135">Nainstalujte Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="6f4fe-135">Install Entity Framework Core</span></span>

<span data-ttu-id="6f4fe-136">Instalace EF Core, nainstalujte balíček vytvořeno EF Core databáze, kterou chcete cílit na pro.</span><span class="sxs-lookup"><span data-stu-id="6f4fe-136">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="6f4fe-137">Seznam dostupných zprostředkovatelů najdete v části [poskytovatelé databází](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="6f4fe-137">For a list of available providers see [Database Providers](../../providers/index.md).</span></span> 

<span data-ttu-id="6f4fe-138">Pro účely tohoto kurzu není nutné instalovat balíček poskytovatele, protože v tomto kurzu použijete SQL Server.</span><span class="sxs-lookup"><span data-stu-id="6f4fe-138">For this tutorial, you don't have to install a provider package because the tutorial uses SQL Server.</span></span> <span data-ttu-id="6f4fe-139">Je součástí balíčku zprostředkovatele SQL Server [Microsoft.AspnetCore.App Microsoft.aspnetcore.all](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span><span class="sxs-lookup"><span data-stu-id="6f4fe-139">The SQL Server provider package is included in the [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="6f4fe-140">Provést zpětnou analýzu modelu</span><span class="sxs-lookup"><span data-stu-id="6f4fe-140">Reverse engineer your model</span></span>

<span data-ttu-id="6f4fe-141">Nyní je čas vytvořit EF model založený na existující databázi.</span><span class="sxs-lookup"><span data-stu-id="6f4fe-141">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="6f4fe-142">**Balíček NuGet –> nástroje Správce –> Konzola správce balíčků**</span><span class="sxs-lookup"><span data-stu-id="6f4fe-142">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="6f4fe-143">Spusťte následující příkaz pro vytvoření modelu z existující databáze:</span><span class="sxs-lookup"><span data-stu-id="6f4fe-143">Run the following command to create a model from the existing database:</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="6f4fe-144">Pokud se zobrazí chyba `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, zavřete a znovu otevřete Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6f4fe-144">If you receive an error stating `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, then close and reopen Visual Studio.</span></span>

> [!TIP]  
> <span data-ttu-id="6f4fe-145">Můžete určit, které tabulky, kterou chcete vygenerovat tak, že přidáte entity pro `-Tables` argument výše uvedeného příkazu.</span><span class="sxs-lookup"><span data-stu-id="6f4fe-145">You can specify which tables you want to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="6f4fe-146">Například, `-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="6f4fe-146">For example, `-Tables Blog,Post`.</span></span>

<span data-ttu-id="6f4fe-147">Zpětná analýza procesu vytvoření tříd entit (`Blog.cs` & `Post.cs`) a odvozené kontextu (`BloggingContext.cs`) na základě schématu existující databázi.</span><span class="sxs-lookup"><span data-stu-id="6f4fe-147">The reverse engineer process created entity classes (`Blog.cs` & `Post.cs`) and a derived context (`BloggingContext.cs`) based on the schema of the existing database.</span></span>

 <span data-ttu-id="6f4fe-148">Entity třídy jsou jednoduché C# objekty, jež reprezentují data budete dotazování a ukládání.</span><span class="sxs-lookup"><span data-stu-id="6f4fe-148">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span> <span data-ttu-id="6f4fe-149">Tady jsou `Blog` a `Post` tříd entit:</span><span class="sxs-lookup"><span data-stu-id="6f4fe-149">Here are the `Blog` and `Post` entity classes:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Post.cs)]

> [!TIP]  
> <span data-ttu-id="6f4fe-150">Povolit opožděné načtení, můžete nastavit vlastnosti navigace `virtual` (Blog.Post a Post.Blog).</span><span class="sxs-lookup"><span data-stu-id="6f4fe-150">To enable lazy loading, you can make navigation properties `virtual` (Blog.Post and Post.Blog).</span></span>

 <span data-ttu-id="6f4fe-151">Kontext představuje relaci s databází a umožňuje dotazování a uložit instancí tříd entit.</span><span class="sxs-lookup"><span data-stu-id="6f4fe-151">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

<!-- Static code listing, rather than a linked file, because the tutorial modifies the context file heavily -->
 ``` csharp
public partial class BloggingContext : DbContext
{
    public BloggingContext()
    {
    }

    public BloggingContext(DbContextOptions<BloggingContext> options)
        : base(options)
    {
    }

    public virtual DbSet<Blog> Blog { get; set; }
    public virtual DbSet<Post> Post { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        if (!optionsBuilder.IsConfigured)
        {
            #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
        }
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>(entity =>
        {
            entity.Property(e => e.Url).IsRequired();
        });

        modelBuilder.Entity<Post>(entity =>
        {
            entity.HasOne(d => d.Blog)
                .WithMany(p => p.Post)
                .HasForeignKey(d => d.BlogId);
        });
    }
}
```

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="6f4fe-152">Kontext zaregistrovat injektáž závislostí</span><span class="sxs-lookup"><span data-stu-id="6f4fe-152">Register your context with dependency injection</span></span>

<span data-ttu-id="6f4fe-153">Koncept vkládání závislostí je zásadním ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6f4fe-153">The concept of dependency injection is central to ASP.NET Core.</span></span> <span data-ttu-id="6f4fe-154">Služby (například `BloggingContext`) jsou registrované pomocí vkládání závislostí při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="6f4fe-154">Services (such as `BloggingContext`) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="6f4fe-155">Komponenty, které vyžadují tyto služby (například vaše řadiče MVC) jsou pak k dispozici tyto služby prostřednictvím vlastnosti nebo parametry konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="6f4fe-155">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span> <span data-ttu-id="6f4fe-156">Další informace o vkládání závislostí v tématu [injektáž závislostí](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) článku na webu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6f4fe-156">For more information on dependency injection see the [Dependency Injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) article on the ASP.NET site.</span></span>

### <a name="register-and-configure-your-context-in-startupcs"></a><span data-ttu-id="6f4fe-157">Registrace a konfigurace v souboru Startup.cs kontext</span><span class="sxs-lookup"><span data-stu-id="6f4fe-157">Register and configure your context in Startup.cs</span></span>

<span data-ttu-id="6f4fe-158">Chcete-li `BloggingContext` k dispozici pro kontrolery MVC, zaregistrujte ho jako služba.</span><span class="sxs-lookup"><span data-stu-id="6f4fe-158">To make `BloggingContext` available to MVC controllers, register it as a service.</span></span>

* <span data-ttu-id="6f4fe-159">Otevřít **Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="6f4fe-159">Open **Startup.cs**</span></span>
* <span data-ttu-id="6f4fe-160">Přidejte následující `using` příkazy na začátku souboru</span><span class="sxs-lookup"><span data-stu-id="6f4fe-160">Add the following `using` statements at the start of the file</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

<span data-ttu-id="6f4fe-161">Teď můžete použít `AddDbContext(...)` metody pro registraci jako služba.</span><span class="sxs-lookup"><span data-stu-id="6f4fe-161">Now you can use the `AddDbContext(...)` method to register it as a service.</span></span>
* <span data-ttu-id="6f4fe-162">Vyhledejte `ConfigureServices(...)` – metoda</span><span class="sxs-lookup"><span data-stu-id="6f4fe-162">Locate the `ConfigureServices(...)` method</span></span>
* <span data-ttu-id="6f4fe-163">Přidejte následující zvýrazněný kód k registraci kontextu jako služba</span><span class="sxs-lookup"><span data-stu-id="6f4fe-163">Add the following highlighted code to register the context as a service</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=14-15)]

> [!TIP]  
> <span data-ttu-id="6f4fe-164">V reálné aplikaci obvykle vložíte připojovací řetězec do proměnné typu konfigurační soubor nebo prostředí.</span><span class="sxs-lookup"><span data-stu-id="6f4fe-164">In a real application you would typically put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="6f4fe-165">Z důvodu zjednodušení se v tomto kurzu můžete definovat v kódu.</span><span class="sxs-lookup"><span data-stu-id="6f4fe-165">For the sake of simplicity, this tutorial has you define it in code.</span></span> <span data-ttu-id="6f4fe-166">Další informace najdete v tématu [připojovací řetězce](../../miscellaneous/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="6f4fe-166">For more information, see [Connection Strings](../../miscellaneous/connection-strings.md).</span></span>

## <a name="create-a-controller-and-views"></a><span data-ttu-id="6f4fe-167">Vytvoření kontroleru a zobrazení</span><span class="sxs-lookup"><span data-stu-id="6f4fe-167">Create a controller and views</span></span>

* <span data-ttu-id="6f4fe-168">Klikněte pravým tlačítkem na **řadiče** složky **Průzkumníka řešení** a vyberte **Přidat -> řadiče...**</span><span class="sxs-lookup"><span data-stu-id="6f4fe-168">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="6f4fe-169">Vyberte **kontroler MVC se zobrazeními, s použitím Entity Framework** a klikněte na tlačítko **Ok**</span><span class="sxs-lookup"><span data-stu-id="6f4fe-169">Select **MVC Controller with views, using Entity Framework** and click **Ok**</span></span>
* <span data-ttu-id="6f4fe-170">Nastavte **třída modelu** k **blogu** a **třída kontextu dat** k **BloggingContext**</span><span class="sxs-lookup"><span data-stu-id="6f4fe-170">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**</span></span>
* <span data-ttu-id="6f4fe-171">Klikněte na tlačítko **přidat**</span><span class="sxs-lookup"><span data-stu-id="6f4fe-171">Click **Add**</span></span>

## <a name="run-the-application"></a><span data-ttu-id="6f4fe-172">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="6f4fe-172">Run the application</span></span>

<span data-ttu-id="6f4fe-173">Nyní můžete spustit aplikaci sledujte v akci.</span><span class="sxs-lookup"><span data-stu-id="6f4fe-173">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="6f4fe-174">**Ladění -> spuštění bez ladění**</span><span class="sxs-lookup"><span data-stu-id="6f4fe-174">**Debug -> Start Without Debugging**</span></span>
* <span data-ttu-id="6f4fe-175">Aplikace vytvoří a otevře ve webovém prohlížeči</span><span class="sxs-lookup"><span data-stu-id="6f4fe-175">The application builds and opens in a web browser</span></span>
* <span data-ttu-id="6f4fe-176">Přejděte na `/Blogs`</span><span class="sxs-lookup"><span data-stu-id="6f4fe-176">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="6f4fe-177">Klikněte na tlačítko **vytvořit nový**</span><span class="sxs-lookup"><span data-stu-id="6f4fe-177">Click **Create New**</span></span>
* <span data-ttu-id="6f4fe-178">Zadejte **Url** nového blogu a klikněte na **Create**</span><span class="sxs-lookup"><span data-stu-id="6f4fe-178">Enter a **Url** for the new blog and click **Create**</span></span>

  ![Vytvoření stránky](_static/create.png)

  ![Indexová stránka](_static/index-existing-db.png)

## <a name="next-steps"></a><span data-ttu-id="6f4fe-181">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6f4fe-181">Next steps</span></span>

<span data-ttu-id="6f4fe-182">Další informace o tom, jak generování uživatelského rozhraní třídy kontextu a entity najdete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="6f4fe-182">For more information about how to scaffold a context and entity classes, see the following articles:</span></span>
* [<span data-ttu-id="6f4fe-183">Zpětná analýza</span><span class="sxs-lookup"><span data-stu-id="6f4fe-183">Reverse Engineering</span></span>](xref:core/managing-schemas/scaffolding)
* [<span data-ttu-id="6f4fe-184">Reference – rozhraní příkazového řádku .NET nástroje Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="6f4fe-184">Entity Framework Core tools reference - .NET CLI</span></span>](xref:core/miscellaneous/cli/dotnet#dotnet-ef-dbcontext-scaffold)
* [<span data-ttu-id="6f4fe-185">Referenční dokumentace nástrojů v Entity Framework Core - Konzola správce balíčků</span><span class="sxs-lookup"><span data-stu-id="6f4fe-185">Entity Framework Core tools reference - Package Manager Console</span></span>](xref:core/miscellaneous/cli/powershell#scaffold-dbcontext)
