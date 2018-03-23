---
title: Začínáme na základní EF ASP.NET Core - existující databázi –
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: db2469d0badd428734425c1f568667f00bef2f4f
ms.sourcegitcommit: 90139dbd6f485473afda0788a5a314c9aa601ea0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/23/2018
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a><span data-ttu-id="0fc31-102">Začínáme s EF základní na ASP.NET Core s existující databázi</span><span class="sxs-lookup"><span data-stu-id="0fc31-102">Getting Started with EF Core on ASP.NET Core with an Existing Database</span></span>

<span data-ttu-id="0fc31-103">V tomto návodu vytvoříte aplikaci ASP.NET MVC jádra, která provádí základní data přístup pomocí rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0fc31-103">In this walkthrough, you will build an ASP.NET Core MVC application that performs basic data access using Entity Framework.</span></span> <span data-ttu-id="0fc31-104">Zpětná analýza použije pro vytvoření modelu Entity Framework založené na existující databázi.</span><span class="sxs-lookup"><span data-stu-id="0fc31-104">You will use reverse engineering to create an Entity Framework model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="0fc31-105">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="0fc31-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0fc31-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0fc31-106">Prerequisites</span></span>

<span data-ttu-id="0fc31-107">Následující požadované součásti jsou nutné k dokončení tohoto názorného postupu:</span><span class="sxs-lookup"><span data-stu-id="0fc31-107">The following prerequisites are needed to complete this walkthrough:</span></span>

* <span data-ttu-id="0fc31-108">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) s tyto úlohy:</span><span class="sxs-lookup"><span data-stu-id="0fc31-108">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="0fc31-109">**Vývoj pro ASP.NET a webové** (v části **Web a Cloud**)</span><span class="sxs-lookup"><span data-stu-id="0fc31-109">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="0fc31-110">**Vývoj pro různé platformy .NET core** (v části **ostatní modulové**)</span><span class="sxs-lookup"><span data-stu-id="0fc31-110">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="0fc31-111">[Základní rozhraní .NET 2.0 SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="0fc31-111">[.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).</span></span>
* [<span data-ttu-id="0fc31-112">Databáze blogu</span><span class="sxs-lookup"><span data-stu-id="0fc31-112">Blogging database</span></span>](#blogging-database)

### <a name="blogging-database"></a><span data-ttu-id="0fc31-113">Databáze blogu</span><span class="sxs-lookup"><span data-stu-id="0fc31-113">Blogging database</span></span>

<span data-ttu-id="0fc31-114">Tento kurz používá **blogu** databáze ve vaší instanci LocalDb jako existující databáze.</span><span class="sxs-lookup"><span data-stu-id="0fc31-114">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="0fc31-115">Pokud jste již vytvořili **blogu** databázi jako součást jiného kurzu, můžete přeskočit tyto kroky.</span><span class="sxs-lookup"><span data-stu-id="0fc31-115">If you have already created the **Blogging** database as part of another tutorial, you can skip these steps.</span></span>

* <span data-ttu-id="0fc31-116">Open Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0fc31-116">Open Visual Studio</span></span>
* <span data-ttu-id="0fc31-117">**Nástroje -> připojit k databázi...**</span><span class="sxs-lookup"><span data-stu-id="0fc31-117">**Tools -> Connect to Database...**</span></span>
* <span data-ttu-id="0fc31-118">Vyberte **Microsoft SQL Server** a klikněte na tlačítko **pokračovat**</span><span class="sxs-lookup"><span data-stu-id="0fc31-118">Select **Microsoft SQL Server** and click **Continue**</span></span>
* <span data-ttu-id="0fc31-119">Zadejte **\mssqllocaldb (localdb)** jako **název serveru**</span><span class="sxs-lookup"><span data-stu-id="0fc31-119">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>
* <span data-ttu-id="0fc31-120">Zadejte **hlavní** jako **název databáze** a klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="0fc31-120">Enter **master** as the **Database Name** and click **OK**</span></span>
* <span data-ttu-id="0fc31-121">Hlavní databázi se nyní zobrazí v části **připojení dat** v **Průzkumníka serveru**</span><span class="sxs-lookup"><span data-stu-id="0fc31-121">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>
* <span data-ttu-id="0fc31-122">Klikněte pravým tlačítkem na databázi v **Průzkumníka serveru** a vyberte **nový dotaz**</span><span class="sxs-lookup"><span data-stu-id="0fc31-122">Right-click on the database in **Server Explorer** and select **New Query**</span></span>
* <span data-ttu-id="0fc31-123">Zkopírujte skript, uvedené níže, do editoru dotazů</span><span class="sxs-lookup"><span data-stu-id="0fc31-123">Copy the script, listed below, into the query editor</span></span>
* <span data-ttu-id="0fc31-124">Klikněte pravým tlačítkem na editoru dotazů a vyberte **spouštění**</span><span class="sxs-lookup"><span data-stu-id="0fc31-124">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="0fc31-125">Vytvoření nového projektu</span><span class="sxs-lookup"><span data-stu-id="0fc31-125">Create a new project</span></span>

* <span data-ttu-id="0fc31-126">Otevřete Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="0fc31-126">Open Visual Studio 2017</span></span>
* <span data-ttu-id="0fc31-127">**Soubor -> New -> projekt...**</span><span class="sxs-lookup"><span data-stu-id="0fc31-127">**File -> New -> Project...**</span></span>
* <span data-ttu-id="0fc31-128">V levé nabídce vyberte **nainstalovaná -> šablony pro -> Visual C# -> Web**</span><span class="sxs-lookup"><span data-stu-id="0fc31-128">From the left menu select **Installed -> Templates -> Visual C# -> Web**</span></span>
* <span data-ttu-id="0fc31-129">Vyberte **webové aplikace ASP.NET Core (.NET Core)** šablona projektu</span><span class="sxs-lookup"><span data-stu-id="0fc31-129">Select the **ASP.NET Core Web Application (.NET Core)** project template</span></span>
* <span data-ttu-id="0fc31-130">Zadejte **EFGetStarted.AspNetCore.ExistingDb** jako název a klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="0fc31-130">Enter **EFGetStarted.AspNetCore.ExistingDb** as the name and click **OK**</span></span>
* <span data-ttu-id="0fc31-131">Počkejte **nové webové aplikace ASP.NET Core** zobrazit dialogové okno</span><span class="sxs-lookup"><span data-stu-id="0fc31-131">Wait for the **New ASP.NET Core Web Application** dialog to appear</span></span>
* <span data-ttu-id="0fc31-132">V části **technologii ASP.NET 2.0 šablony základní** vyberte **webové aplikace (Model-View-Controller)**</span><span class="sxs-lookup"><span data-stu-id="0fc31-132">Under **ASP.NET Core Templates 2.0** select the **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="0fc31-133">Ujistěte se, že **ověřování** je nastaven na **bez ověřování**</span><span class="sxs-lookup"><span data-stu-id="0fc31-133">Ensure that **Authentication** is set to **No Authentication**</span></span>
* <span data-ttu-id="0fc31-134">Klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="0fc31-134">Click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="0fc31-135">Nainstalujte rozhraní Entity Framework</span><span class="sxs-lookup"><span data-stu-id="0fc31-135">Install Entity Framework</span></span>

<span data-ttu-id="0fc31-136">Abyste mohli používat EF jádra, nainstalujte balíček pro následující zprostředkovatele databáze, kterou chcete zacílit.</span><span class="sxs-lookup"><span data-stu-id="0fc31-136">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="0fc31-137">Tento návod používá SQL Server.</span><span class="sxs-lookup"><span data-stu-id="0fc31-137">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="0fc31-138">Seznam dostupných zprostředkovatelů naleznete v části [zprostředkovatelů databáze](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="0fc31-138">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="0fc31-139">**Nástroje > Správce balíčků NuGet > Konzola správce balíčků**</span><span class="sxs-lookup"><span data-stu-id="0fc31-139">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="0fc31-140">Spustit `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="0fc31-140">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="0fc31-141">Použijeme některé nástroje Entity Framework pro vytvoření modelu z databáze.</span><span class="sxs-lookup"><span data-stu-id="0fc31-141">We will be using some Entity Framework Tools to create a model from the database.</span></span> <span data-ttu-id="0fc31-142">Proto se nainstaluje balíček nástroje:</span><span class="sxs-lookup"><span data-stu-id="0fc31-142">So we will install the tools package as well:</span></span>

* <span data-ttu-id="0fc31-143">Spustit `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="0fc31-143">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

<span data-ttu-id="0fc31-144">Nemůžeme vytvořit kontrolery a zobrazení později na pomocí některé nástroje ASP.NET Core generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="0fc31-144">We will be using some ASP.NET Core Scaffolding tools to create controllers and views later on.</span></span> <span data-ttu-id="0fc31-145">Proto se nainstaluje tohoto návrhu balíčku:</span><span class="sxs-lookup"><span data-stu-id="0fc31-145">So we will install this design package as well:</span></span>

* <span data-ttu-id="0fc31-146">Spustit `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span><span class="sxs-lookup"><span data-stu-id="0fc31-146">Run `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="0fc31-147">Provést zpětnou analýzu modelu</span><span class="sxs-lookup"><span data-stu-id="0fc31-147">Reverse engineer your model</span></span>

<span data-ttu-id="0fc31-148">Nyní je čas vytvořit model EF založený na existující databázi.</span><span class="sxs-lookup"><span data-stu-id="0fc31-148">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="0fc31-149">**Nástroje pro –> balíček NuGet Manager –> Konzola správce balíčků**</span><span class="sxs-lookup"><span data-stu-id="0fc31-149">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="0fc31-150">Spusťte následující příkaz pro vytvoření modelu z existující databáze:</span><span class="sxs-lookup"><span data-stu-id="0fc31-150">Run the following command to create a model from the existing database:</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="0fc31-151">Pokud se zobrazí chybová zpráva, `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, zavřete a znovu otevřete Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0fc31-151">If you receive an error stating `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, then close and reopen Visual Studio.</span></span>

> [!TIP]  
> <span data-ttu-id="0fc31-152">Můžete určit, které tabulky, kterou chcete vygenerovat entity pro přidáním `-Tables` argument výše uvedeného příkazu.</span><span class="sxs-lookup"><span data-stu-id="0fc31-152">You can specify which tables you want to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="0fc31-153">Například</span><span class="sxs-lookup"><span data-stu-id="0fc31-153">E.g.</span></span> <span data-ttu-id="0fc31-154">`-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="0fc31-154">`-Tables Blog,Post`.</span></span>

<span data-ttu-id="0fc31-155">Proces zpětné analýzy vytvořen tříd entit (`Blog.cs` & `Post.cs`) a odvozené kontextu (`BloggingContext.cs`) na základě schématu existující databáze.</span><span class="sxs-lookup"><span data-stu-id="0fc31-155">The reverse engineer process created entity classes (`Blog.cs` & `Post.cs`) and a derived context (`BloggingContext.cs`) based on the schema of the existing database.</span></span>

 <span data-ttu-id="0fc31-156">Třídy entity jsou jednoduché C# objekty, které představují data, budete mít dotazování a ukládání.</span><span class="sxs-lookup"><span data-stu-id="0fc31-156">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

 <span data-ttu-id="0fc31-157">Kontext představuje relaci s databází a umožňuje dotazování a uložit instance tříd entity.</span><span class="sxs-lookup"><span data-stu-id="0fc31-157">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

<!-- Static code listing, rather than a linked file, because the walkthrough modifies the context file heavily -->
 ``` csharp
public partial class BloggingContext : DbContext
{
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

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="0fc31-158">Zaregistrovat váš kontext vkládání závislostí</span><span class="sxs-lookup"><span data-stu-id="0fc31-158">Register your context with dependency injection</span></span>

<span data-ttu-id="0fc31-159">Koncept vkládání závislostí je důležitá pro ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0fc31-159">The concept of dependency injection is central to ASP.NET Core.</span></span> <span data-ttu-id="0fc31-160">Služby (například `BloggingContext`) jsou registrovány vkládání závislostí při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="0fc31-160">Services (such as `BloggingContext`) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="0fc31-161">Součásti, které vyžadují tyto služby (například vaše řadiče MVC) jsou pak k dispozici tyto služby prostřednictvím konstruktor parametry nebo vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="0fc31-161">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span> <span data-ttu-id="0fc31-162">Další informace o vkládání závislostí v tématu [vkládání závislostí](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) článku na webu technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0fc31-162">For more information on dependency injection see the [Dependency Injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) article on the ASP.NET site.</span></span>

### <a name="remove-inline-context-configuration"></a><span data-ttu-id="0fc31-163">Odebrat konfiguraci vložený kontext</span><span class="sxs-lookup"><span data-stu-id="0fc31-163">Remove inline context configuration</span></span>

<span data-ttu-id="0fc31-164">V ASP.NET Core, konfigurace se obvykle provádí v **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="0fc31-164">In ASP.NET Core, configuration is generally performed in **Startup.cs**.</span></span> <span data-ttu-id="0fc31-165">Tak, aby odpovídala tento vzor, jsme se přesune o konfiguraci zprostředkovatele databáze na **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="0fc31-165">To conform to this pattern, we will move configuration of the database provider to **Startup.cs**.</span></span>

* <span data-ttu-id="0fc31-166">Otevřete `Models\BloggingContext.cs`</span><span class="sxs-lookup"><span data-stu-id="0fc31-166">Open `Models\BloggingContext.cs`</span></span>
* <span data-ttu-id="0fc31-167">Odstranit `OnConfiguring(...)` – metoda</span><span class="sxs-lookup"><span data-stu-id="0fc31-167">Delete the `OnConfiguring(...)` method</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
    optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
}
```

* <span data-ttu-id="0fc31-168">Přidejte následující konstruktor, který vám umožní konfigurace, které mají být předány do kontextu pomocí vkládání závislostí</span><span class="sxs-lookup"><span data-stu-id="0fc31-168">Add the following constructor, which will allow configuration to be passed into the context by dependency injection</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/BloggingContext.cs#Constructor)]

### <a name="register-and-configure-your-context-in-startupcs"></a><span data-ttu-id="0fc31-169">Zaregistrovat a nakonfigurovat v souboru Startup.cs váš kontext</span><span class="sxs-lookup"><span data-stu-id="0fc31-169">Register and configure your context in Startup.cs</span></span>

<span data-ttu-id="0fc31-170">V pořadí pro naše řadiče MVC, chcete-li použít `BloggingContext` přidáme a zaregistrujte ho jako služba.</span><span class="sxs-lookup"><span data-stu-id="0fc31-170">In order for our MVC controllers to make use of `BloggingContext` we are going to register it as a service.</span></span>

* <span data-ttu-id="0fc31-171">Otevřete **Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="0fc31-171">Open **Startup.cs**</span></span>
* <span data-ttu-id="0fc31-172">Přidejte následující `using` příkazy na začátku souboru</span><span class="sxs-lookup"><span data-stu-id="0fc31-172">Add the following `using` statements at the start of the file</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

<span data-ttu-id="0fc31-173">Nyní můžeme použít `AddDbContext(...)` metody pro registraci jako služba.</span><span class="sxs-lookup"><span data-stu-id="0fc31-173">Now we can use the `AddDbContext(...)` method to register it as a service.</span></span>
* <span data-ttu-id="0fc31-174">Vyhledejte `ConfigureServices(...)` – metoda</span><span class="sxs-lookup"><span data-stu-id="0fc31-174">Locate the `ConfigureServices(...)` method</span></span>
* <span data-ttu-id="0fc31-175">Přidejte následující kód k registraci kontextu jako služby</span><span class="sxs-lookup"><span data-stu-id="0fc31-175">Add the following code to register the context as a service</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

> [!TIP]  
> <span data-ttu-id="0fc31-176">V reálné aplikaci byste obvykle umístili připojovací řetězec v konfiguračním souboru.</span><span class="sxs-lookup"><span data-stu-id="0fc31-176">In a real application you would typically put the connection string in a configuration file.</span></span> <span data-ttu-id="0fc31-177">Z důvodu zjednodušení jsme se jeho definováním v kódu.</span><span class="sxs-lookup"><span data-stu-id="0fc31-177">For the sake of simplicity, we are defining it in code.</span></span> <span data-ttu-id="0fc31-178">Další informace najdete v tématu [připojovací řetězce](../../miscellaneous/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="0fc31-178">For more information, see [Connection Strings](../../miscellaneous/connection-strings.md).</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="0fc31-179">Vytvořit řadič</span><span class="sxs-lookup"><span data-stu-id="0fc31-179">Create a controller</span></span>

<span data-ttu-id="0fc31-180">Dál jsme budete povolte generování uživatelského rozhraní v našem projektu.</span><span class="sxs-lookup"><span data-stu-id="0fc31-180">Next, we'll enable scaffolding in our project.</span></span>

* <span data-ttu-id="0fc31-181">Klikněte pravým tlačítkem na **řadiče** složky v **Průzkumníku řešení** a vyberte **Přidat -> řadiče...**</span><span class="sxs-lookup"><span data-stu-id="0fc31-181">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="0fc31-182">Vyberte **úplné závislosti** a klikněte na tlačítko **přidat**</span><span class="sxs-lookup"><span data-stu-id="0fc31-182">Select **Full Dependencies** and click **Add**</span></span>
* <span data-ttu-id="0fc31-183">Můžete ignorovat podle pokynů `ScaffoldingReadMe.txt` soubor, který otevírá</span><span class="sxs-lookup"><span data-stu-id="0fc31-183">You can ignore the instructions in the `ScaffoldingReadMe.txt` file that opens</span></span>

<span data-ttu-id="0fc31-184">Teď, když je povoleno generování uživatelského rozhraní, jsme vygenerovat řadič pro `Blog` entity.</span><span class="sxs-lookup"><span data-stu-id="0fc31-184">Now that scaffolding is enabled, we can scaffold a controller for the `Blog` entity.</span></span>

* <span data-ttu-id="0fc31-185">Klikněte pravým tlačítkem na **řadiče** složky v **Průzkumníku řešení** a vyberte **Přidat -> řadiče...**</span><span class="sxs-lookup"><span data-stu-id="0fc31-185">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="0fc31-186">Vyberte **kontroler MVC se zobrazeními s využitím nástroje Entity Framework** a klikněte na tlačítko **Ok**</span><span class="sxs-lookup"><span data-stu-id="0fc31-186">Select **MVC Controller with views, using Entity Framework** and click **Ok**</span></span>
* <span data-ttu-id="0fc31-187">Nastavit **třída modelu** k **Blog** a **třída kontextu dat** k **BloggingContext**</span><span class="sxs-lookup"><span data-stu-id="0fc31-187">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**</span></span>
* <span data-ttu-id="0fc31-188">Klikněte na tlačítko **přidat**</span><span class="sxs-lookup"><span data-stu-id="0fc31-188">Click **Add**</span></span>

## <a name="run-the-application"></a><span data-ttu-id="0fc31-189">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="0fc31-189">Run the application</span></span>

<span data-ttu-id="0fc31-190">Nyní můžete spustit aplikaci najdete v části v akci.</span><span class="sxs-lookup"><span data-stu-id="0fc31-190">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="0fc31-191">**-Ladění > Spustit bez ladění**</span><span class="sxs-lookup"><span data-stu-id="0fc31-191">**Debug -> Start Without Debugging**</span></span>
* <span data-ttu-id="0fc31-192">Aplikace bude sestavovat a otevřete ve webovém prohlížeči</span><span class="sxs-lookup"><span data-stu-id="0fc31-192">The application will build and open in a web browser</span></span>
* <span data-ttu-id="0fc31-193">Přejděte na `/Blogs`</span><span class="sxs-lookup"><span data-stu-id="0fc31-193">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="0fc31-194">Klikněte na tlačítko **vytvořit nový**</span><span class="sxs-lookup"><span data-stu-id="0fc31-194">Click **Create New**</span></span>
* <span data-ttu-id="0fc31-195">Zadejte **Url** pro nové blog a klikněte na tlačítko **vytvořit**</span><span class="sxs-lookup"><span data-stu-id="0fc31-195">Enter a **Url** for the new blog and click **Create**</span></span>

![obrázek](_static/create.png)

![obrázek](_static/index-existing-db.png)
