---
title: "Začínáme na základní EF ASP.NET Core - novou databázi –"
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
ms.date: 04/07/2017
ms.topic: get-started-article
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 7e7ecaff29e9830bf3bcf742e6a5d54e1ced24de
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/05/2017
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a><span data-ttu-id="af9f2-102">Začínáme s EF základní na ASP.NET Core s novou databázi</span><span class="sxs-lookup"><span data-stu-id="af9f2-102">Getting Started with EF Core on ASP.NET Core with a New database</span></span>

<span data-ttu-id="af9f2-103">V tomto návodu vytvoříte aplikaci ASP.NET MVC jádra, která provádí základní přístup k datům používá Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="af9f2-103">In this walkthrough, you will build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="af9f2-104">Migrace použije k vytvoření databáze z modelu EF jádra.</span><span class="sxs-lookup"><span data-stu-id="af9f2-104">You will use migrations to create the database from your EF Core model.</span></span> <span data-ttu-id="af9f2-105">V tématu [další prostředky](#additional-resources) pro další kurzy Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="af9f2-105">See [Additional Resources](#additional-resources) for more Entity Framework Core tutorials.</span></span>

<span data-ttu-id="af9f2-106">Tento kurz vyžaduje:</span><span class="sxs-lookup"><span data-stu-id="af9f2-106">This tutorial requires:</span></span>
* <span data-ttu-id="af9f2-107">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) s tyto úlohy:</span><span class="sxs-lookup"><span data-stu-id="af9f2-107">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="af9f2-108">**Vývoj pro ASP.NET a webové** (v části **Web a Cloud**)</span><span class="sxs-lookup"><span data-stu-id="af9f2-108">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="af9f2-109">**Vývoj pro různé platformy .NET core** (v části **ostatní modulové**)</span><span class="sxs-lookup"><span data-stu-id="af9f2-109">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="af9f2-110">[Základní rozhraní .NET 2.0 SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="af9f2-110">[.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).</span></span>

> [!TIP]  
> <span data-ttu-id="af9f2-111">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="af9f2-111">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb) on GitHub.</span></span>

## <a name="create-a-new-project-in-visual-studio-2017"></a><span data-ttu-id="af9f2-112">Vytvořte nový projekt v Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="af9f2-112">Create a new project in Visual Studio 2017</span></span>

* <span data-ttu-id="af9f2-113">**Soubor > Nový > Projekt**</span><span class="sxs-lookup"><span data-stu-id="af9f2-113">**File > New > Project**</span></span>
* <span data-ttu-id="af9f2-114">V levé nabídce vyberte **nainstalovaná > šablony > Visual C# > .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="af9f2-114">From the left menu select **Installed > Templates > Visual C# > .NET Core**.</span></span>
* <span data-ttu-id="af9f2-115">Vyberte **webové aplikace ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="af9f2-115">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="af9f2-116">Zadejte **EFGetStarted.AspNetCore.NewDb** pro název a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="af9f2-116">Enter **EFGetStarted.AspNetCore.NewDb** for the name and click **OK**.</span></span>
* <span data-ttu-id="af9f2-117">V **nové webové aplikace ASP.NET Core** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="af9f2-117">In the **New ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="af9f2-118">Zkontrolujte možnosti **.NET Core** a **technologii ASP.NET 2.0 základní** jsou vybrány v rozevíracích seznamů</span><span class="sxs-lookup"><span data-stu-id="af9f2-118">Ensure the options **.NET Core** and **ASP.NET Core 2.0** are selected in the drop down lists</span></span>
  * <span data-ttu-id="af9f2-119">Vyberte **webové aplikace (Model-View-Controller)** šablona projektu</span><span class="sxs-lookup"><span data-stu-id="af9f2-119">Select the **Web Application (Model-View-Controller)** project template</span></span>
  * <span data-ttu-id="af9f2-120">Ujistěte se, že **ověřování** je nastaven na **bez ověřování**</span><span class="sxs-lookup"><span data-stu-id="af9f2-120">Ensure that **Authentication** is set to **No Authentication**</span></span>
  * <span data-ttu-id="af9f2-121">Klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="af9f2-121">Click **OK**</span></span>

<span data-ttu-id="af9f2-122">Upozornění: Pokud používáte **jednotlivé uživatelské účty** místo **žádné** pro **ověřování** pak model Entity Framework Core přidá do projektu v `Models\IdentityModel.cs`.</span><span class="sxs-lookup"><span data-stu-id="af9f2-122">Warning: If you use **Individual User Accounts** instead of **None** for **Authentication** then an Entity Framework Core model will be added to your project in `Models\IdentityModel.cs`.</span></span> <span data-ttu-id="af9f2-123">Pomocí technik, které se dozvíte v tomto návodu, můžete přidat druhý modelu nebo rozšířit tento existující model tak, aby obsahovala vaše třídy entity.</span><span class="sxs-lookup"><span data-stu-id="af9f2-123">Using the techniques you will learn in this walkthrough, you can choose to add a second model, or extend this existing model to contain your entity classes.</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="af9f2-124">Instalace jádra Entity Framework</span><span class="sxs-lookup"><span data-stu-id="af9f2-124">Install Entity Framework Core</span></span>

<span data-ttu-id="af9f2-125">Nainstalujte balíček pro následující zprostředkovatele databáze EF jádra, které chcete cílit.</span><span class="sxs-lookup"><span data-stu-id="af9f2-125">Install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="af9f2-126">Tento návod používá SQL Server.</span><span class="sxs-lookup"><span data-stu-id="af9f2-126">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="af9f2-127">Seznam dostupných zprostředkovatelů naleznete v části [zprostředkovatelů databáze](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="af9f2-127">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="af9f2-128">**Nástroje > Správce balíčků NuGet > Konzola správce balíčků**</span><span class="sxs-lookup"><span data-stu-id="af9f2-128">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="af9f2-129">Spustit`Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="af9f2-129">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="af9f2-130">Použijeme některé Entity Framework jádra nástroje k vytvoření databáze z modelu EF jádra.</span><span class="sxs-lookup"><span data-stu-id="af9f2-130">We will be using some Entity Framework Core Tools to create a database from your EF Core model.</span></span> <span data-ttu-id="af9f2-131">Proto se nainstaluje balíček nástroje:</span><span class="sxs-lookup"><span data-stu-id="af9f2-131">So we will install the tools package as well:</span></span>

* <span data-ttu-id="af9f2-132">Spustit`Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="af9f2-132">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

<span data-ttu-id="af9f2-133">Nemůžeme vytvořit kontrolery a zobrazení později na pomocí některé nástroje ASP.NET Core generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="af9f2-133">We will be using some ASP.NET Core Scaffolding tools to create controllers and views later on.</span></span> <span data-ttu-id="af9f2-134">Proto se nainstaluje tohoto návrhu balíčku:</span><span class="sxs-lookup"><span data-stu-id="af9f2-134">So we will install this design package as well:</span></span>

* <span data-ttu-id="af9f2-135">Spustit`Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span><span class="sxs-lookup"><span data-stu-id="af9f2-135">Run `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span></span>

## <a name="create-the-model"></a><span data-ttu-id="af9f2-136">Vytvoření modelu</span><span class="sxs-lookup"><span data-stu-id="af9f2-136">Create the model</span></span>

<span data-ttu-id="af9f2-137">Definujte kontext a entity třídy, které tvoří modelu:</span><span class="sxs-lookup"><span data-stu-id="af9f2-137">Define a context and entity classes that make up the model:</span></span>

* <span data-ttu-id="af9f2-138">Klikněte pravým tlačítkem na **modely** složky a vyberte **Přidat > třída**.</span><span class="sxs-lookup"><span data-stu-id="af9f2-138">Right-click on the **Models** folder and select **Add > Class**.</span></span>
* <span data-ttu-id="af9f2-139">Zadejte **Model.cs** jako název a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="af9f2-139">Enter **Model.cs** as the name and click **OK**.</span></span>
* <span data-ttu-id="af9f2-140">Obsah souboru nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="af9f2-140">Replace the contents of the file with the following code:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

<span data-ttu-id="af9f2-141">Poznámka: V reálné aplikaci byste obvykle umístili každá třída z modelu v samostatném souboru.</span><span class="sxs-lookup"><span data-stu-id="af9f2-141">Note: In a real app you would typically put each class from your model in a separate file.</span></span> <span data-ttu-id="af9f2-142">Z důvodu zjednodušení jsme jsou uvedení všechny třídy v jednom souboru pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="af9f2-142">For the sake of simplicity, we are putting all the classes in one file for this tutorial.</span></span>

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="af9f2-143">Zaregistrovat váš kontext vkládání závislostí</span><span class="sxs-lookup"><span data-stu-id="af9f2-143">Register your context with dependency injection</span></span>

<span data-ttu-id="af9f2-144">Služby (například `BloggingContext`) jsou registrovány [vkládání závislostí](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) během spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="af9f2-144">Services (such as `BloggingContext`) are registered with [dependency injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) during application startup.</span></span> <span data-ttu-id="af9f2-145">Součásti, které vyžadují tyto služby (například vaše řadiče MVC) jsou pak k dispozici tyto služby prostřednictvím konstruktor parametry nebo vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="af9f2-145">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span>

<span data-ttu-id="af9f2-146">V pořadí pro naše řadiče MVC, chcete-li použít `BloggingContext` jsme se registrovat jako službu.</span><span class="sxs-lookup"><span data-stu-id="af9f2-146">In order for our MVC controllers to make use of `BloggingContext` we will register it as a service.</span></span>

* <span data-ttu-id="af9f2-147">Otevřete **Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="af9f2-147">Open **Startup.cs**</span></span>
* <span data-ttu-id="af9f2-148">Přidejte následující `using` příkazy:</span><span class="sxs-lookup"><span data-stu-id="af9f2-148">Add the following `using` statements:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

<span data-ttu-id="af9f2-149">Přidat `AddDbContext` metody pro registraci jako služba:</span><span class="sxs-lookup"><span data-stu-id="af9f2-149">Add the `AddDbContext` method to register it as a service:</span></span>

* <span data-ttu-id="af9f2-150">Přidejte následující kód, který `ConfigureServices` metoda:</span><span class="sxs-lookup"><span data-stu-id="af9f2-150">Add the following code to the `ConfigureServices` method:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

<span data-ttu-id="af9f2-151">Poznámka: Skutečné aplikace gennerally přepnutím připojovací řetězec v konfiguračním souboru.</span><span class="sxs-lookup"><span data-stu-id="af9f2-151">Note: A real app would gennerally put the connection string in a configuration file.</span></span> <span data-ttu-id="af9f2-152">Z důvodu zjednodušení jsme se jeho definováním v kódu.</span><span class="sxs-lookup"><span data-stu-id="af9f2-152">For the sake of simplicity, we are defining it in code.</span></span> <span data-ttu-id="af9f2-153">V tématu [připojovací řetězce](../../miscellaneous/connection-strings.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="af9f2-153">See [Connection Strings](../../miscellaneous/connection-strings.md) for more information.</span></span>

## <a name="create-your-database"></a><span data-ttu-id="af9f2-154">Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="af9f2-154">Create your database</span></span>

<span data-ttu-id="af9f2-155">Jakmile máte modelu, můžete použít [migrace](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) k vytvoření databáze.</span><span class="sxs-lookup"><span data-stu-id="af9f2-155">Once you have a model, you can use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="af9f2-156">Otevřete pomocí PMC:</span><span class="sxs-lookup"><span data-stu-id="af9f2-156">Open the PMC:</span></span>

  <span data-ttu-id="af9f2-157">**Nástroje pro –> balíček NuGet Manager –> Konzola správce balíčků**</span><span class="sxs-lookup"><span data-stu-id="af9f2-157">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="af9f2-158">Spustit `Add-Migration InitialCreate` chcete vygenerovat migraci k vytvoření počáteční sadu tabulek pro váš model.</span><span class="sxs-lookup"><span data-stu-id="af9f2-158">Run `Add-Migration InitialCreate` to scaffold a migration to create the initial set of tables for your model.</span></span> <span data-ttu-id="af9f2-159">Pokud se zobrazí chybová zpráva, `The term 'add-migration' is not recognized as the name of a cmdlet`, zavřete a znovu otevřete Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="af9f2-159">If you receive an error stating `The term 'add-migration' is not recognized as the name of a cmdlet`, close and reopen Visual Studio.</span></span>
* <span data-ttu-id="af9f2-160">Spustit `Update-Database` použít nové migrace do databáze.</span><span class="sxs-lookup"><span data-stu-id="af9f2-160">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="af9f2-161">Tento příkaz vytvoří databázi před použitím migrace.</span><span class="sxs-lookup"><span data-stu-id="af9f2-161">This command creates the database before applying migrations.</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="af9f2-162">Vytvořit řadič</span><span class="sxs-lookup"><span data-stu-id="af9f2-162">Create a controller</span></span>

<span data-ttu-id="af9f2-163">Povolte generování uživatelského rozhraní v projektu:</span><span class="sxs-lookup"><span data-stu-id="af9f2-163">Enable scaffolding in the project:</span></span>

* <span data-ttu-id="af9f2-164">Klikněte pravým tlačítkem na **řadiče** složky v **Průzkumníku řešení** a vyberte **Přidat > řadiče**.</span><span class="sxs-lookup"><span data-stu-id="af9f2-164">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="af9f2-165">Vyberte **minimální závislosti** a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="af9f2-165">Select **Minimal Dependencies** and click **Add**.</span></span>
* <span data-ttu-id="af9f2-166">Můžete ignorovat nebo odstranit *ScaffoldingReadMe.txt* souboru.</span><span class="sxs-lookup"><span data-stu-id="af9f2-166">You can ignore or delete the *ScaffoldingReadMe.txt* file.</span></span>

<span data-ttu-id="af9f2-167">Teď, když je povoleno generování uživatelského rozhraní, jsme vygenerovat řadič pro `Blog` entity.</span><span class="sxs-lookup"><span data-stu-id="af9f2-167">Now that scaffolding is enabled, we can scaffold a controller for the `Blog` entity.</span></span>

* <span data-ttu-id="af9f2-168">Klikněte pravým tlačítkem na **řadiče** složky v **Průzkumníku řešení** a vyberte **Přidat > řadiče**.</span><span class="sxs-lookup"><span data-stu-id="af9f2-168">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="af9f2-169">Vyberte **kontroler MVC se zobrazeními s využitím nástroje Entity Framework** a klikněte na tlačítko **Ok**.</span><span class="sxs-lookup"><span data-stu-id="af9f2-169">Select **MVC Controller with views, using Entity Framework** and click **Ok**.</span></span>
* <span data-ttu-id="af9f2-170">Nastavit **třída modelu** k **Blog** a **třída kontextu dat** k **BloggingContext**.</span><span class="sxs-lookup"><span data-stu-id="af9f2-170">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**.</span></span>
* <span data-ttu-id="af9f2-171">Klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="af9f2-171">Click **Add**.</span></span>


## <a name="run-the-application"></a><span data-ttu-id="af9f2-172">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="af9f2-172">Run the application</span></span>

<span data-ttu-id="af9f2-173">Stisknutím klávesy F5 spusťte a testování aplikací.</span><span class="sxs-lookup"><span data-stu-id="af9f2-173">Press F5 to run and test the app.</span></span>

* <span data-ttu-id="af9f2-174">Přejděte na`/Blogs`</span><span class="sxs-lookup"><span data-stu-id="af9f2-174">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="af9f2-175">Pomocí odkazu vytvořte vytvořit některé položky blogu.</span><span class="sxs-lookup"><span data-stu-id="af9f2-175">Use the create link to create some blog entries.</span></span> <span data-ttu-id="af9f2-176">Testování podrobnosti a odstraňte odkazy.</span><span class="sxs-lookup"><span data-stu-id="af9f2-176">Test the details and delete links.</span></span>

![obrázek](_static/create.png)

![obrázek](_static/index-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="af9f2-179">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="af9f2-179">Additional Resources</span></span>

* <span data-ttu-id="af9f2-180">[EF - novou databázi pomocí SQLite](xref:core/get-started/netcore/new-db-sqlite) – kurz EF konzoly napříč platformami.</span><span class="sxs-lookup"><span data-stu-id="af9f2-180">[EF - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="af9f2-181">Úvod do základní ASP.NET MVC v Mac nebo Linux</span><span class="sxs-lookup"><span data-stu-id="af9f2-181">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="af9f2-182">Úvod do základní ASP.NET MVC pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="af9f2-182">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="af9f2-183">Začínáme s ASP.NET Core a Entity Framework Core pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="af9f2-183">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
