---
title: Začínáme v ASP.NET Core – existující databáze – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: e28149346ccd7531449ea696505588317471e6dd
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949150"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a><span data-ttu-id="3977c-102">Začínáme s EF Core v ASP.NET Core s existující databáze</span><span class="sxs-lookup"><span data-stu-id="3977c-102">Getting Started with EF Core on ASP.NET Core with an Existing Database</span></span>

<span data-ttu-id="3977c-103">V tomto návodu vytvoříte aplikaci ASP.NET Core MVC, která provádí základní přístup k datům pomocí Entity Frameworku.</span><span class="sxs-lookup"><span data-stu-id="3977c-103">In this walkthrough, you will build an ASP.NET Core MVC application that performs basic data access using Entity Framework.</span></span> <span data-ttu-id="3977c-104">Zpětná analýza použije k vytvoření Entity Framework model založený na existující databázi.</span><span class="sxs-lookup"><span data-stu-id="3977c-104">You will use reverse engineering to create an Entity Framework model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="3977c-105">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="3977c-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3977c-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3977c-106">Prerequisites</span></span>

<span data-ttu-id="3977c-107">Následující požadované součásti jsou potřeba k dokončení tohoto návodu:</span><span class="sxs-lookup"><span data-stu-id="3977c-107">The following prerequisites are needed to complete this walkthrough:</span></span>

* <span data-ttu-id="3977c-108">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) se tyto úlohy:</span><span class="sxs-lookup"><span data-stu-id="3977c-108">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="3977c-109">**Vývoj pro ASP.NET a web** (v části **Web a Cloud**)</span><span class="sxs-lookup"><span data-stu-id="3977c-109">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="3977c-110">**Vývoj pro různé platformy .NET core** (v části **další sady nástrojů**)</span><span class="sxs-lookup"><span data-stu-id="3977c-110">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="3977c-111">[.NET core 2.0 SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="3977c-111">[.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).</span></span>
* [<span data-ttu-id="3977c-112">Blogovací databáze</span><span class="sxs-lookup"><span data-stu-id="3977c-112">Blogging database</span></span>](#blogging-database)

### <a name="blogging-database"></a><span data-ttu-id="3977c-113">Blogovací databáze</span><span class="sxs-lookup"><span data-stu-id="3977c-113">Blogging database</span></span>

<span data-ttu-id="3977c-114">Tento kurz používá **blogovací** databáze na instanci LocalDb jako existující databázi.</span><span class="sxs-lookup"><span data-stu-id="3977c-114">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="3977c-115">Pokud jste již vytvořili **blogovací** databázi jako součást další kurz, můžete přeskočit tyto kroky.</span><span class="sxs-lookup"><span data-stu-id="3977c-115">If you have already created the **Blogging** database as part of another tutorial, you can skip these steps.</span></span>

* <span data-ttu-id="3977c-116">Otevřít Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3977c-116">Open Visual Studio</span></span>
* <span data-ttu-id="3977c-117">**Nástroje -> připojení k databázi...**</span><span class="sxs-lookup"><span data-stu-id="3977c-117">**Tools -> Connect to Database...**</span></span>
* <span data-ttu-id="3977c-118">Vyberte **Microsoft SQL Server** a klikněte na tlačítko **pokračovat**</span><span class="sxs-lookup"><span data-stu-id="3977c-118">Select **Microsoft SQL Server** and click **Continue**</span></span>
* <span data-ttu-id="3977c-119">Zadejte **(localdb) \mssqllocaldb** jako **název serveru**</span><span class="sxs-lookup"><span data-stu-id="3977c-119">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>
* <span data-ttu-id="3977c-120">Zadejte **hlavní** jako **název_databáze** a klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="3977c-120">Enter **master** as the **Database Name** and click **OK**</span></span>
* <span data-ttu-id="3977c-121">Hlavní databázi se nyní zobrazí v části **datová připojení** v **Průzkumníka serveru**</span><span class="sxs-lookup"><span data-stu-id="3977c-121">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>
* <span data-ttu-id="3977c-122">Klikněte pravým tlačítkem na databázi v **Průzkumníka serveru** a vyberte **nový dotaz**</span><span class="sxs-lookup"><span data-stu-id="3977c-122">Right-click on the database in **Server Explorer** and select **New Query**</span></span>
* <span data-ttu-id="3977c-123">Zkopírujte skript uvedený níže, do editoru dotazů</span><span class="sxs-lookup"><span data-stu-id="3977c-123">Copy the script, listed below, into the query editor</span></span>
* <span data-ttu-id="3977c-124">Klikněte pravým tlačítkem myši na editor dotazů a vyberte **spouštění**</span><span class="sxs-lookup"><span data-stu-id="3977c-124">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="3977c-125">Vytvoření nového projektu</span><span class="sxs-lookup"><span data-stu-id="3977c-125">Create a new project</span></span>

* <span data-ttu-id="3977c-126">Otevřít Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="3977c-126">Open Visual Studio 2017</span></span>
* <span data-ttu-id="3977c-127">**Soubor -> Nový -> projekt...**</span><span class="sxs-lookup"><span data-stu-id="3977c-127">**File -> New -> Project...**</span></span>
* <span data-ttu-id="3977c-128">V levé nabídce vyberte **nainstalováno -> šablony -> Visual C# -> Web**</span><span class="sxs-lookup"><span data-stu-id="3977c-128">From the left menu select **Installed -> Templates -> Visual C# -> Web**</span></span>
* <span data-ttu-id="3977c-129">Vyberte **webová aplikace ASP.NET Core (.NET Core)** šablony projektu</span><span class="sxs-lookup"><span data-stu-id="3977c-129">Select the **ASP.NET Core Web Application (.NET Core)** project template</span></span>
* <span data-ttu-id="3977c-130">Zadejte **EFGetStarted.AspNetCore.ExistingDb** jako název a klikněte na **OK**</span><span class="sxs-lookup"><span data-stu-id="3977c-130">Enter **EFGetStarted.AspNetCore.ExistingDb** as the name and click **OK**</span></span>
* <span data-ttu-id="3977c-131">Počkejte **nová webová aplikace ASP.NET Core** zobrazit dialogové okno</span><span class="sxs-lookup"><span data-stu-id="3977c-131">Wait for the **New ASP.NET Core Web Application** dialog to appear</span></span>
* <span data-ttu-id="3977c-132">V části **2.0 šablony ASP.NET Core** vyberte **webové aplikace (Model-View-Controller)**</span><span class="sxs-lookup"><span data-stu-id="3977c-132">Under **ASP.NET Core Templates 2.0** select the **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="3977c-133">Ujistěte se, že **ověřování** je nastavena na **bez ověřování**</span><span class="sxs-lookup"><span data-stu-id="3977c-133">Ensure that **Authentication** is set to **No Authentication**</span></span>
* <span data-ttu-id="3977c-134">Klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="3977c-134">Click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="3977c-135">Nainstalujte rozhraní Entity Framework</span><span class="sxs-lookup"><span data-stu-id="3977c-135">Install Entity Framework</span></span>

<span data-ttu-id="3977c-136">Použití EF Core, nainstalujte balíček pro poskytovatelů databáze, kterou chcete cílit.</span><span class="sxs-lookup"><span data-stu-id="3977c-136">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="3977c-137">Tento návod používá systém SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3977c-137">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="3977c-138">Seznam dostupných zprostředkovatelů najdete v části [poskytovatelé databází](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="3977c-138">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="3977c-139">**Nástroje > Správce balíčků NuGet > Konzola správce balíčků**</span><span class="sxs-lookup"><span data-stu-id="3977c-139">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="3977c-140">Spustit `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="3977c-140">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="3977c-141">Použijeme některé Entity Framework Tools pro vytvoření modelu z databáze.</span><span class="sxs-lookup"><span data-stu-id="3977c-141">We will be using some Entity Framework Tools to create a model from the database.</span></span> <span data-ttu-id="3977c-142">Proto se nainstaluje balíček nástroje:</span><span class="sxs-lookup"><span data-stu-id="3977c-142">So we will install the tools package as well:</span></span>

* <span data-ttu-id="3977c-143">Spustit `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="3977c-143">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

<span data-ttu-id="3977c-144">Jsme k vytvoření kontrolerů a zobrazení později pomocí některé nástroje pro generování uživatelského rozhraní ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3977c-144">We will be using some ASP.NET Core Scaffolding tools to create controllers and views later on.</span></span> <span data-ttu-id="3977c-145">Proto se nainstaluje tohoto návrhu balíčku:</span><span class="sxs-lookup"><span data-stu-id="3977c-145">So we will install this design package as well:</span></span>

* <span data-ttu-id="3977c-146">Spustit `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span><span class="sxs-lookup"><span data-stu-id="3977c-146">Run `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="3977c-147">Provést zpětnou analýzu modelu</span><span class="sxs-lookup"><span data-stu-id="3977c-147">Reverse engineer your model</span></span>

<span data-ttu-id="3977c-148">Nyní je čas vytvořit EF model založený na existující databázi.</span><span class="sxs-lookup"><span data-stu-id="3977c-148">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="3977c-149">**Balíček NuGet –> nástroje Správce –> Konzola správce balíčků**</span><span class="sxs-lookup"><span data-stu-id="3977c-149">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="3977c-150">Spusťte následující příkaz pro vytvoření modelu z existující databáze:</span><span class="sxs-lookup"><span data-stu-id="3977c-150">Run the following command to create a model from the existing database:</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="3977c-151">Pokud se zobrazí chyba `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, zavřete a znovu otevřete Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3977c-151">If you receive an error stating `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, then close and reopen Visual Studio.</span></span>

> [!TIP]  
> <span data-ttu-id="3977c-152">Můžete určit, které tabulky, kterou chcete vygenerovat tak, že přidáte entity pro `-Tables` argument výše uvedeného příkazu.</span><span class="sxs-lookup"><span data-stu-id="3977c-152">You can specify which tables you want to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="3977c-153">Například `-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="3977c-153">For example, `-Tables Blog,Post`.</span></span>

<span data-ttu-id="3977c-154">Zpětná analýza procesu vytvoření tříd entit (`Blog.cs` & `Post.cs`) a odvozené kontextu (`BloggingContext.cs`) na základě schématu existující databázi.</span><span class="sxs-lookup"><span data-stu-id="3977c-154">The reverse engineer process created entity classes (`Blog.cs` & `Post.cs`) and a derived context (`BloggingContext.cs`) based on the schema of the existing database.</span></span>

 <span data-ttu-id="3977c-155">Entity třídy jsou jednoduché C# objekty, jež reprezentují data budete dotazování a ukládání.</span><span class="sxs-lookup"><span data-stu-id="3977c-155">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

 <span data-ttu-id="3977c-156">Kontext představuje relaci s databází a umožňuje dotazování a uložit instancí tříd entit.</span><span class="sxs-lookup"><span data-stu-id="3977c-156">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

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

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="3977c-157">Kontext zaregistrovat injektáž závislostí</span><span class="sxs-lookup"><span data-stu-id="3977c-157">Register your context with dependency injection</span></span>

<span data-ttu-id="3977c-158">Koncept vkládání závislostí je zásadním ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3977c-158">The concept of dependency injection is central to ASP.NET Core.</span></span> <span data-ttu-id="3977c-159">Služby (například `BloggingContext`) jsou registrované pomocí vkládání závislostí při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="3977c-159">Services (such as `BloggingContext`) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="3977c-160">Komponenty, které vyžadují tyto služby (například vaše řadiče MVC) jsou pak k dispozici tyto služby prostřednictvím vlastnosti nebo parametry konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="3977c-160">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span> <span data-ttu-id="3977c-161">Další informace o vkládání závislostí v tématu [injektáž závislostí](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) článku na webu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3977c-161">For more information on dependency injection see the [Dependency Injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) article on the ASP.NET site.</span></span>

### <a name="remove-inline-context-configuration"></a><span data-ttu-id="3977c-162">Odebrat konfiguraci kontextu vložené</span><span class="sxs-lookup"><span data-stu-id="3977c-162">Remove inline context configuration</span></span>

<span data-ttu-id="3977c-163">V ASP.NET Core, konfigurace se obvykle provádí v **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="3977c-163">In ASP.NET Core, configuration is generally performed in **Startup.cs**.</span></span> <span data-ttu-id="3977c-164">Tak, aby odpovídal na tento model přesuneme konfigurace poskytovatele databáze za účelem **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="3977c-164">To conform to this pattern, we will move configuration of the database provider to **Startup.cs**.</span></span>

* <span data-ttu-id="3977c-165">Otevřít `Models\BloggingContext.cs`</span><span class="sxs-lookup"><span data-stu-id="3977c-165">Open `Models\BloggingContext.cs`</span></span>
* <span data-ttu-id="3977c-166">Odstranit `OnConfiguring(...)` – metoda</span><span class="sxs-lookup"><span data-stu-id="3977c-166">Delete the `OnConfiguring(...)` method</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
    optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
}
```

* <span data-ttu-id="3977c-167">Přidejte následující konstruktor, který vám umožní konfigurace mají být předány do kontextu pomocí vkládání závislostí</span><span class="sxs-lookup"><span data-stu-id="3977c-167">Add the following constructor, which will allow configuration to be passed into the context by dependency injection</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/BloggingContext.cs#Constructor)]

### <a name="register-and-configure-your-context-in-startupcs"></a><span data-ttu-id="3977c-168">Registrace a konfigurace v souboru Startup.cs kontext</span><span class="sxs-lookup"><span data-stu-id="3977c-168">Register and configure your context in Startup.cs</span></span>

<span data-ttu-id="3977c-169">Aby naše kontrolery MVC, aby využívání `BloggingContext` budeme registrovat jako službu.</span><span class="sxs-lookup"><span data-stu-id="3977c-169">In order for our MVC controllers to make use of `BloggingContext` we are going to register it as a service.</span></span>

* <span data-ttu-id="3977c-170">Otevřít **Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="3977c-170">Open **Startup.cs**</span></span>
* <span data-ttu-id="3977c-171">Přidejte následující `using` příkazy na začátku souboru</span><span class="sxs-lookup"><span data-stu-id="3977c-171">Add the following `using` statements at the start of the file</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

<span data-ttu-id="3977c-172">Teď můžeme použít `AddDbContext(...)` metody pro registraci jako služba.</span><span class="sxs-lookup"><span data-stu-id="3977c-172">Now we can use the `AddDbContext(...)` method to register it as a service.</span></span>
* <span data-ttu-id="3977c-173">Vyhledejte `ConfigureServices(...)` – metoda</span><span class="sxs-lookup"><span data-stu-id="3977c-173">Locate the `ConfigureServices(...)` method</span></span>
* <span data-ttu-id="3977c-174">Přidejte následující kód k registraci kontextu jako služba</span><span class="sxs-lookup"><span data-stu-id="3977c-174">Add the following code to register the context as a service</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

> [!TIP]  
> <span data-ttu-id="3977c-175">V reálné aplikaci byste obvykle vložte připojovací řetězec v konfiguračním souboru.</span><span class="sxs-lookup"><span data-stu-id="3977c-175">In a real application you would typically put the connection string in a configuration file.</span></span> <span data-ttu-id="3977c-176">Z důvodu zjednodušení můžeme ji definování v kódu.</span><span class="sxs-lookup"><span data-stu-id="3977c-176">For the sake of simplicity, we are defining it in code.</span></span> <span data-ttu-id="3977c-177">Další informace najdete v tématu [připojovací řetězce](../../miscellaneous/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="3977c-177">For more information, see [Connection Strings](../../miscellaneous/connection-strings.md).</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="3977c-178">Vytvoření kontroleru</span><span class="sxs-lookup"><span data-stu-id="3977c-178">Create a controller</span></span>

<span data-ttu-id="3977c-179">Dále jsme v našem projektu budete povolit generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="3977c-179">Next, we'll enable scaffolding in our project.</span></span>

* <span data-ttu-id="3977c-180">Klikněte pravým tlačítkem na **řadiče** složky **Průzkumníka řešení** a vyberte **Přidat -> řadiče...**</span><span class="sxs-lookup"><span data-stu-id="3977c-180">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="3977c-181">Vyberte **úplné závislosti** a klikněte na tlačítko **přidat**</span><span class="sxs-lookup"><span data-stu-id="3977c-181">Select **Full Dependencies** and click **Add**</span></span>
* <span data-ttu-id="3977c-182">Můžete ignorovat podle pokynů `ScaffoldingReadMe.txt` soubor, který otevírá</span><span class="sxs-lookup"><span data-stu-id="3977c-182">You can ignore the instructions in the `ScaffoldingReadMe.txt` file that opens</span></span>

<span data-ttu-id="3977c-183">Teď, když je povoleno generování uživatelského rozhraní, jsme generování uživatelského rozhraní pro řadič `Blog` entity.</span><span class="sxs-lookup"><span data-stu-id="3977c-183">Now that scaffolding is enabled, we can scaffold a controller for the `Blog` entity.</span></span>

* <span data-ttu-id="3977c-184">Klikněte pravým tlačítkem na **řadiče** složky **Průzkumníka řešení** a vyberte **Přidat -> řadiče...**</span><span class="sxs-lookup"><span data-stu-id="3977c-184">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="3977c-185">Vyberte **kontroler MVC se zobrazeními, s použitím Entity Framework** a klikněte na tlačítko **Ok**</span><span class="sxs-lookup"><span data-stu-id="3977c-185">Select **MVC Controller with views, using Entity Framework** and click **Ok**</span></span>
* <span data-ttu-id="3977c-186">Nastavte **třída modelu** k **blogu** a **třída kontextu dat** k **BloggingContext**</span><span class="sxs-lookup"><span data-stu-id="3977c-186">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**</span></span>
* <span data-ttu-id="3977c-187">Klikněte na tlačítko **přidat**</span><span class="sxs-lookup"><span data-stu-id="3977c-187">Click **Add**</span></span>

## <a name="run-the-application"></a><span data-ttu-id="3977c-188">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="3977c-188">Run the application</span></span>

<span data-ttu-id="3977c-189">Nyní můžete spustit aplikaci sledujte v akci.</span><span class="sxs-lookup"><span data-stu-id="3977c-189">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="3977c-190">**Ladění -> spuštění bez ladění**</span><span class="sxs-lookup"><span data-stu-id="3977c-190">**Debug -> Start Without Debugging**</span></span>
* <span data-ttu-id="3977c-191">Aplikace bude sestavení a otevře ve webovém prohlížeči</span><span class="sxs-lookup"><span data-stu-id="3977c-191">The application will build and open in a web browser</span></span>
* <span data-ttu-id="3977c-192">Přejděte na `/Blogs`</span><span class="sxs-lookup"><span data-stu-id="3977c-192">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="3977c-193">Klikněte na tlačítko **vytvořit nový**</span><span class="sxs-lookup"><span data-stu-id="3977c-193">Click **Create New**</span></span>
* <span data-ttu-id="3977c-194">Zadejte **Url** nového blogu a klikněte na **Create**</span><span class="sxs-lookup"><span data-stu-id="3977c-194">Enter a **Url** for the new blog and click **Create**</span></span>

![obrázek](_static/create.png)

![obrázek](_static/index-existing-db.png)
