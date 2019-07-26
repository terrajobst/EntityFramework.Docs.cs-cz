---
title: Začínáme s ASP.NET Core – existující databáze – EF Core
author: rowanmiller
description: Začínáme s EF Core ASP.NET Core se stávající databází
ms.date: 08/02/2018
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: 6b0ed0a9222644bee31d23234aa27b2084137f4a
ms.sourcegitcommit: 755a15a789631cc4ea581e2262a2dcc49c219eef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2019
ms.locfileid: "68497514"
---
# <a name="get-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a><span data-ttu-id="c0eb3-103">Začínáme s EF Core ASP.NET Core se stávající databází</span><span class="sxs-lookup"><span data-stu-id="c0eb3-103">Get started with EF Core on ASP.NET Core with an existing database</span></span>

<span data-ttu-id="c0eb3-104">V tomto kurzu vytvoříte aplikaci ASP.NET Core MVC, která provádí základní přístup k datům pomocí Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="c0eb3-104">In this tutorial, you build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="c0eb3-105">Můžete provést zpětnou analýzu existující databáze a vytvořit model Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c0eb3-105">You reverse engineer an existing database to create an Entity Framework model.</span></span>

<span data-ttu-id="c0eb3-106">[Podívejte se na ukázku tohoto článku na GitHubu](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).</span><span class="sxs-lookup"><span data-stu-id="c0eb3-106">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c0eb3-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c0eb3-107">Prerequisites</span></span>

<span data-ttu-id="c0eb3-108">Nainstalujte následující software:</span><span class="sxs-lookup"><span data-stu-id="c0eb3-108">Install the following software:</span></span>

* <span data-ttu-id="c0eb3-109">[Visual Studio 2017 15,7](https://www.visualstudio.com/downloads/) s těmito úlohami:</span><span class="sxs-lookup"><span data-stu-id="c0eb3-109">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="c0eb3-110">**Vývoj pro ASP.NET a web** (v části **Web & Cloud**)</span><span class="sxs-lookup"><span data-stu-id="c0eb3-110">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="c0eb3-111">**Vývoj pro různé platformy .NET Core** (v **jiných sad nástrojů**)</span><span class="sxs-lookup"><span data-stu-id="c0eb3-111">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="c0eb3-112">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="c0eb3-112">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

## <a name="create-blogging-database"></a><span data-ttu-id="c0eb3-113">Vytvořit blogovací databázi</span><span class="sxs-lookup"><span data-stu-id="c0eb3-113">Create Blogging database</span></span>

<span data-ttu-id="c0eb3-114">V tomto kurzu se jako existující databáze používá **blogovací** databáze na instanci LocalDB.</span><span class="sxs-lookup"><span data-stu-id="c0eb3-114">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span> <span data-ttu-id="c0eb3-115">Pokud jste již vytvořili **blogovací** databázi jako součást jiného kurzu, přeskočte tyto kroky.</span><span class="sxs-lookup"><span data-stu-id="c0eb3-115">If you have already created the **Blogging** database as part of another tutorial, skip these steps.</span></span>

* <span data-ttu-id="c0eb3-116">Otevřít Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c0eb3-116">Open Visual Studio</span></span>
* <span data-ttu-id="c0eb3-117">**Nástroje – > se připojit k databázi...**</span><span class="sxs-lookup"><span data-stu-id="c0eb3-117">**Tools -> Connect to Database...**</span></span>
* <span data-ttu-id="c0eb3-118">Vyberte **Microsoft SQL Server** a klikněte na **pokračovat** .</span><span class="sxs-lookup"><span data-stu-id="c0eb3-118">Select **Microsoft SQL Server** and click **Continue**</span></span>
* <span data-ttu-id="c0eb3-119">Jako **název serveru** zadejte **(LocalDB) \mssqllocaldb** .</span><span class="sxs-lookup"><span data-stu-id="c0eb3-119">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>
* <span data-ttu-id="c0eb3-120">Jako **název databáze** zadejte **Master** a klikněte na **OK** .</span><span class="sxs-lookup"><span data-stu-id="c0eb3-120">Enter **master** as the **Database Name** and click **OK**</span></span>
* <span data-ttu-id="c0eb3-121">Hlavní databáze se teď zobrazuje v části **datová připojení** v **Průzkumník serveru**</span><span class="sxs-lookup"><span data-stu-id="c0eb3-121">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>
* <span data-ttu-id="c0eb3-122">Klikněte pravým tlačítkem na databázi v **Průzkumník serveru** a vyberte **Nový dotaz** .</span><span class="sxs-lookup"><span data-stu-id="c0eb3-122">Right-click on the database in **Server Explorer** and select **New Query**</span></span>
* <span data-ttu-id="c0eb3-123">Zkopírujte níže uvedený skript do editoru dotazů.</span><span class="sxs-lookup"><span data-stu-id="c0eb3-123">Copy the script listed below into the query editor</span></span>
* <span data-ttu-id="c0eb3-124">Klikněte pravým tlačítkem na Editor dotazů a vyberte **Spustit** .</span><span class="sxs-lookup"><span data-stu-id="c0eb3-124">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="c0eb3-125">Vytvoření nového projektu</span><span class="sxs-lookup"><span data-stu-id="c0eb3-125">Create a new project</span></span>

* <span data-ttu-id="c0eb3-126">Open Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="c0eb3-126">Open Visual Studio 2017</span></span>
* <span data-ttu-id="c0eb3-127">**Soubor > Nový > projektu...**</span><span class="sxs-lookup"><span data-stu-id="c0eb3-127">**File > New > Project...**</span></span>
* <span data-ttu-id="c0eb3-128">V nabídce vlevo vyberte **nainstalované > Visual C# > Web** .</span><span class="sxs-lookup"><span data-stu-id="c0eb3-128">From the left menu select **Installed > Visual C# > Web**</span></span>
* <span data-ttu-id="c0eb3-129">Vyberte šablonu projektu **ASP.NET Core webové aplikace**</span><span class="sxs-lookup"><span data-stu-id="c0eb3-129">Select the **ASP.NET Core Web Application** project template</span></span>
* <span data-ttu-id="c0eb3-130">Jako název zadejte **EFGetStarted. AspNetCore. ExistingDb** (musí přesně odpovídat oboru názvů, který se později použil v kódu) a klikněte na **OK** .</span><span class="sxs-lookup"><span data-stu-id="c0eb3-130">Enter **EFGetStarted.AspNetCore.ExistingDb** as the name (it has to match exactly the namespace later used in the code) and click **OK**</span></span> 
* <span data-ttu-id="c0eb3-131">Počkejte, než se zobrazí dialogové okno **nové webové aplikace ASP.NET Core** .</span><span class="sxs-lookup"><span data-stu-id="c0eb3-131">Wait for the **New ASP.NET Core Web Application** dialog to appear</span></span>
* <span data-ttu-id="c0eb3-132">Ujistěte se, že rozevírací seznam cílové rozhraní je nastavený na **.NET Core**, a rozevírací seznam verze je nastavený na **ASP.NET Core 2,1** .</span><span class="sxs-lookup"><span data-stu-id="c0eb3-132">Make sure that the target framework dropdown is set to **.NET Core**, and the version dropdown is set to **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="c0eb3-133">Výběr šablony **webové aplikace (model-zobrazení-kontroler)**</span><span class="sxs-lookup"><span data-stu-id="c0eb3-133">Select the **Web Application (Model-View-Controller)** template</span></span>
* <span data-ttu-id="c0eb3-134">Ujistěte se, že **ověřování** je nastaveno na **bez ověřování** .</span><span class="sxs-lookup"><span data-stu-id="c0eb3-134">Ensure that **Authentication** is set to **No Authentication**</span></span>
* <span data-ttu-id="c0eb3-135">Klikněte na **OK** .</span><span class="sxs-lookup"><span data-stu-id="c0eb3-135">Click **OK**</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="c0eb3-136">Nainstalovat Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="c0eb3-136">Install Entity Framework Core</span></span>

<span data-ttu-id="c0eb3-137">Chcete-li nainstalovat EF Core, nainstalujte balíček pro zprostředkovatele EF Corech databází, které chcete cílit.</span><span class="sxs-lookup"><span data-stu-id="c0eb3-137">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="c0eb3-138">Seznam dostupných zprostředkovatelů najdete v tématu [poskytovatelé databáze](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="c0eb3-138">For a list of available providers see [Database Providers](../../providers/index.md).</span></span> 

<span data-ttu-id="c0eb3-139">Pro účely tohoto kurzu nemusíte instalovat balíček poskytovatele, protože tento kurz používá SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c0eb3-139">For this tutorial, you don't have to install a provider package because the tutorial uses SQL Server.</span></span> <span data-ttu-id="c0eb3-140">Balíček poskytovatele SQL Server je součástí [Microsoft. AspnetCore. app Metapackage](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span><span class="sxs-lookup"><span data-stu-id="c0eb3-140">The SQL Server provider package is included in the [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="c0eb3-141">Zpětná analýza modelu</span><span class="sxs-lookup"><span data-stu-id="c0eb3-141">Reverse engineer your model</span></span>

<span data-ttu-id="c0eb3-142">Nyní je čas vytvořit model EF založený na vaší stávající databázi.</span><span class="sxs-lookup"><span data-stu-id="c0eb3-142">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="c0eb3-143">**Nástroje – > Správce balíčků NuGet – > konzolu Správce balíčků**</span><span class="sxs-lookup"><span data-stu-id="c0eb3-143">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="c0eb3-144">Spuštěním následujícího příkazu vytvořte model z existující databáze:</span><span class="sxs-lookup"><span data-stu-id="c0eb3-144">Run the following command to create a model from the existing database:</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="c0eb3-145">Pokud se zobrazí chyba s oznámením `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, zavřete a znovu otevřete Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c0eb3-145">If you receive an error stating `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, then close and reopen Visual Studio.</span></span>

> [!TIP]  
> <span data-ttu-id="c0eb3-146">Přidáním `-Tables` argumentu do výše uvedeného příkazu můžete určit, pro které tabulky chcete generovat entity.</span><span class="sxs-lookup"><span data-stu-id="c0eb3-146">You can specify which tables you want to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="c0eb3-147">Například, `-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="c0eb3-147">For example, `-Tables Blog,Post`.</span></span>

<span data-ttu-id="c0eb3-148">Proces zpětného analýz vytvořil třídy entit (`Blog.cs` & `Post.cs`) a odvozený kontext (`BloggingContext.cs`) založený na schématu existující databáze.</span><span class="sxs-lookup"><span data-stu-id="c0eb3-148">The reverse engineer process created entity classes (`Blog.cs` & `Post.cs`) and a derived context (`BloggingContext.cs`) based on the schema of the existing database.</span></span>

 <span data-ttu-id="c0eb3-149">Třídy entit jsou jednoduché C# objekty, které reprezentují data, která budete dotazovat a ukládat.</span><span class="sxs-lookup"><span data-stu-id="c0eb3-149">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span> <span data-ttu-id="c0eb3-150">Tady jsou `Blog` třídy entit `Post` a:</span><span class="sxs-lookup"><span data-stu-id="c0eb3-150">Here are the `Blog` and `Post` entity classes:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Post.cs)]

> [!TIP]  
> <span data-ttu-id="c0eb3-151">Chcete-li povolit opožděné načítání, můžete vytvořit `virtual` navigační vlastnosti (blog.post a post. blog).</span><span class="sxs-lookup"><span data-stu-id="c0eb3-151">To enable lazy loading, you can make navigation properties `virtual` (Blog.Post and Post.Blog).</span></span>

 <span data-ttu-id="c0eb3-152">Kontext představuje relaci s databází a umožňuje dotazování a ukládání instancí tříd entit.</span><span class="sxs-lookup"><span data-stu-id="c0eb3-152">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

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

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="c0eb3-153">Registrace kontextu pomocí injektáže závislosti</span><span class="sxs-lookup"><span data-stu-id="c0eb3-153">Register your context with dependency injection</span></span>

<span data-ttu-id="c0eb3-154">Koncept vkládání závislostí je střední až ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c0eb3-154">The concept of dependency injection is central to ASP.NET Core.</span></span> <span data-ttu-id="c0eb3-155">Služby (například `BloggingContext`) jsou registrovány se vkládáním závislostí během spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="c0eb3-155">Services (such as `BloggingContext`) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="c0eb3-156">Služby, které vyžadují tyto služby (například řadiče MVC), pak poskytují tyto služby prostřednictvím parametrů nebo vlastností konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="c0eb3-156">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span> <span data-ttu-id="c0eb3-157">Další informace o vkládání závislostí najdete v článku o [Injektáže závislosti](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) na webu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c0eb3-157">For more information on dependency injection see the [Dependency Injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) article on the ASP.NET site.</span></span>

### <a name="register-and-configure-your-context-in-startupcs"></a><span data-ttu-id="c0eb3-158">Registrace a konfigurace kontextu v Startup.cs</span><span class="sxs-lookup"><span data-stu-id="c0eb3-158">Register and configure your context in Startup.cs</span></span>

<span data-ttu-id="c0eb3-159">Pokud ho `BloggingContext` chcete zpřístupnit pro řadiče MVC, zaregistrujte ho jako službu.</span><span class="sxs-lookup"><span data-stu-id="c0eb3-159">To make `BloggingContext` available to MVC controllers, register it as a service.</span></span>

* <span data-ttu-id="c0eb3-160">Otevřít **Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="c0eb3-160">Open **Startup.cs**</span></span>
* <span data-ttu-id="c0eb3-161">Na začátek souboru `using` přidejte následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="c0eb3-161">Add the following `using` statements at the start of the file</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

<span data-ttu-id="c0eb3-162">Nyní můžete použít `AddDbContext(...)` metodu k její registraci jako službu.</span><span class="sxs-lookup"><span data-stu-id="c0eb3-162">Now you can use the `AddDbContext(...)` method to register it as a service.</span></span>
* <span data-ttu-id="c0eb3-163">`ConfigureServices(...)` Vyhledejte metodu</span><span class="sxs-lookup"><span data-stu-id="c0eb3-163">Locate the `ConfigureServices(...)` method</span></span>
* <span data-ttu-id="c0eb3-164">Přidejte následující zvýrazněný kód pro registraci kontextu jako služby.</span><span class="sxs-lookup"><span data-stu-id="c0eb3-164">Add the following highlighted code to register the context as a service</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=14-15)]

> [!TIP]  
> <span data-ttu-id="c0eb3-165">V reálné aplikaci byste obvykle vložili připojovací řetězec do konfiguračního souboru nebo proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="c0eb3-165">In a real application you would typically put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="c0eb3-166">V zájmu jednoduchosti vám tento kurz definovali v kódu.</span><span class="sxs-lookup"><span data-stu-id="c0eb3-166">For the sake of simplicity, this tutorial has you define it in code.</span></span> <span data-ttu-id="c0eb3-167">Další informace najdete v tématu [připojovací řetězce](../../miscellaneous/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="c0eb3-167">For more information, see [Connection Strings](../../miscellaneous/connection-strings.md).</span></span>

## <a name="create-a-controller-and-views"></a><span data-ttu-id="c0eb3-168">Vytvoření kontroleru a zobrazení</span><span class="sxs-lookup"><span data-stu-id="c0eb3-168">Create a controller and views</span></span>

* <span data-ttu-id="c0eb3-169">Klikněte pravým tlačítkem na  složku Controllers v **Průzkumník řešení** a vyberte **Add-> Controller...**</span><span class="sxs-lookup"><span data-stu-id="c0eb3-169">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="c0eb3-170">Vyberte **kontroler MVC se zobrazeními, pomocí Entity Framework** a klikněte na **OK** .</span><span class="sxs-lookup"><span data-stu-id="c0eb3-170">Select **MVC Controller with views, using Entity Framework** and click **Ok**</span></span>
* <span data-ttu-id="c0eb3-171">Nastavení **třídy modelu** na **blog** a **třídu kontextu dat** na **BloggingContext**</span><span class="sxs-lookup"><span data-stu-id="c0eb3-171">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**</span></span>
* <span data-ttu-id="c0eb3-172">Klikněte na **Přidat** .</span><span class="sxs-lookup"><span data-stu-id="c0eb3-172">Click **Add**</span></span>

## <a name="run-the-application"></a><span data-ttu-id="c0eb3-173">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="c0eb3-173">Run the application</span></span>

<span data-ttu-id="c0eb3-174">Teď můžete aplikaci spustit, aby se zobrazila v akci.</span><span class="sxs-lookup"><span data-stu-id="c0eb3-174">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="c0eb3-175">**Ladění – > Spustit bez ladění**</span><span class="sxs-lookup"><span data-stu-id="c0eb3-175">**Debug -> Start Without Debugging**</span></span>
* <span data-ttu-id="c0eb3-176">Aplikace se vytvoří a otevře ve webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="c0eb3-176">The application builds and opens in a web browser</span></span>
* <span data-ttu-id="c0eb3-177">Přejděte na `/Blogs`</span><span class="sxs-lookup"><span data-stu-id="c0eb3-177">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="c0eb3-178">Klikněte na **vytvořit nový** .</span><span class="sxs-lookup"><span data-stu-id="c0eb3-178">Click **Create New**</span></span>
* <span data-ttu-id="c0eb3-179">Zadejte **adresu URL** nového blogu a klikněte na **vytvořit** .</span><span class="sxs-lookup"><span data-stu-id="c0eb3-179">Enter a **Url** for the new blog and click **Create**</span></span>

  ![Vytvoření stránky](_static/create.png)

  ![Stránka indexu](_static/index-existing-db.png)

## <a name="next-steps"></a><span data-ttu-id="c0eb3-182">Další postup</span><span class="sxs-lookup"><span data-stu-id="c0eb3-182">Next steps</span></span>

<span data-ttu-id="c0eb3-183">Další informace o tom, jak vytvořit generátory kontextu a třídy entit, najdete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="c0eb3-183">For more information about how to scaffold a context and entity classes, see the following articles:</span></span>
* [<span data-ttu-id="c0eb3-184">Zpětná analýza</span><span class="sxs-lookup"><span data-stu-id="c0eb3-184">Reverse Engineering</span></span>](xref:core/managing-schemas/scaffolding)
* [<span data-ttu-id="c0eb3-185">Referenční informace k nástrojům pro Entity Framework Core – .NET CLI</span><span class="sxs-lookup"><span data-stu-id="c0eb3-185">Entity Framework Core tools reference - .NET CLI</span></span>](xref:core/miscellaneous/cli/dotnet#dotnet-ef-dbcontext-scaffold)
* [<span data-ttu-id="c0eb3-186">Referenční informace o nástrojích Entity Framework Core Tools – konzola správce balíčků</span><span class="sxs-lookup"><span data-stu-id="c0eb3-186">Entity Framework Core tools reference - Package Manager Console</span></span>](xref:core/miscellaneous/cli/powershell#scaffold-dbcontext)
