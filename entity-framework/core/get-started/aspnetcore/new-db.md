---
title: Začínáme v ASP.NET Core – nová databáze – EF Core
author: rick-anderson
ms.author: riande
ms.date: 08/03/2018
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: c6a86dd943dc7fe6f600455fe6743ea01a062aab
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996061"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a><span data-ttu-id="9d1dc-102">Začínáme s EF Core v ASP.NET Core s novou databázi</span><span class="sxs-lookup"><span data-stu-id="9d1dc-102">Getting Started with EF Core on ASP.NET Core with a New database</span></span>

<span data-ttu-id="9d1dc-103">V tomto kurzu sestavíte aplikaci ASP.NET Core MVC, která provádí základní přístup k datům pomocí Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="9d1dc-103">In this tutorial, you build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="9d1dc-104">Vytvoření databáze z vašeho modelu EF Core pomocí migrace.</span><span class="sxs-lookup"><span data-stu-id="9d1dc-104">You use migrations to create the database from your EF Core model.</span></span>

<span data-ttu-id="9d1dc-105">[Zobrazit ukázky v tomto článku na Githubu](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb).</span><span class="sxs-lookup"><span data-stu-id="9d1dc-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9d1dc-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9d1dc-106">Prerequisites</span></span>

<span data-ttu-id="9d1dc-107">Nainstalujte následující software:</span><span class="sxs-lookup"><span data-stu-id="9d1dc-107">Install the following software:</span></span>

* <span data-ttu-id="9d1dc-108">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) se tyto úlohy:</span><span class="sxs-lookup"><span data-stu-id="9d1dc-108">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="9d1dc-109">**Vývoj pro ASP.NET a web** (v části **Web a Cloud**)</span><span class="sxs-lookup"><span data-stu-id="9d1dc-109">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="9d1dc-110">**Vývoj pro různé platformy .NET core** (v části **další sady nástrojů**)</span><span class="sxs-lookup"><span data-stu-id="9d1dc-110">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="9d1dc-111">[.NET core 2.1 SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="9d1dc-111">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

## <a name="create-a-new-project-in-visual-studio-2017"></a><span data-ttu-id="9d1dc-112">Vytvoření nového projektu v sadě Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="9d1dc-112">Create a new project in Visual Studio 2017</span></span>

* <span data-ttu-id="9d1dc-113">Otevřít Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="9d1dc-113">Open Visual Studio 2017</span></span>
* <span data-ttu-id="9d1dc-114">**Soubor > Nový > Projekt**</span><span class="sxs-lookup"><span data-stu-id="9d1dc-114">**File > New > Project**</span></span>
* <span data-ttu-id="9d1dc-115">V levé nabídce vyberte **nainstalováno > Visual C# > .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="9d1dc-115">From the left menu select **Installed > Visual C# > .NET Core**.</span></span>
* <span data-ttu-id="9d1dc-116">Vyberte **webová aplikace ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="9d1dc-116">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="9d1dc-117">Zadejte **EFGetStarted.AspNetCore.NewDb** název a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="9d1dc-117">Enter **EFGetStarted.AspNetCore.NewDb** for the name and click **OK**.</span></span>
* <span data-ttu-id="9d1dc-118">V **nová webová aplikace ASP.NET Core** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="9d1dc-118">In the **New ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="9d1dc-119">Zkontrolujte možnosti **.NET Core** a **ASP.NET Core 2.1** jsou vybrány v rozevíracích seznamech</span><span class="sxs-lookup"><span data-stu-id="9d1dc-119">Ensure the options **.NET Core** and **ASP.NET Core 2.1** are selected in the drop down lists</span></span>
  * <span data-ttu-id="9d1dc-120">Vyberte **webové aplikace (Model-View-Controller)** šablony projektu</span><span class="sxs-lookup"><span data-stu-id="9d1dc-120">Select the **Web Application (Model-View-Controller)** project template</span></span>
  * <span data-ttu-id="9d1dc-121">Ujistěte se, že **ověřování** je nastavena na **bez ověřování**</span><span class="sxs-lookup"><span data-stu-id="9d1dc-121">Ensure that **Authentication** is set to **No Authentication**</span></span>
  * <span data-ttu-id="9d1dc-122">Klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="9d1dc-122">Click **OK**</span></span>

<span data-ttu-id="9d1dc-123">Upozornění: Pokud používáte **jednotlivé uživatelské účty** místo **žádný** pro **ověřování** pak model Entity Framework Core se přidají do vašeho projektu v `Models\IdentityModel.cs`.</span><span class="sxs-lookup"><span data-stu-id="9d1dc-123">Warning: If you use **Individual User Accounts** instead of **None** for **Authentication** then an Entity Framework Core model will be added to your project in `Models\IdentityModel.cs`.</span></span> <span data-ttu-id="9d1dc-124">Pomocí technik, které se naučíte v tomto kurzu, můžete přidat druhý modelu nebo rozšiřte tento existující model tak, aby obsahovala tříd entit.</span><span class="sxs-lookup"><span data-stu-id="9d1dc-124">Using the techniques you learn in this tutorial, you can choose to add a second model, or extend this existing model to contain your entity classes.</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="9d1dc-125">Nainstalujte Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="9d1dc-125">Install Entity Framework Core</span></span>

<span data-ttu-id="9d1dc-126">Instalace EF Core, nainstalujte balíček vytvořeno EF Core databáze, kterou chcete cílit na pro.</span><span class="sxs-lookup"><span data-stu-id="9d1dc-126">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="9d1dc-127">Seznam dostupných zprostředkovatelů najdete v části [poskytovatelé databází](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="9d1dc-127">For a list of available providers see [Database Providers](../../providers/index.md).</span></span> 

<span data-ttu-id="9d1dc-128">Pro účely tohoto kurzu není nutné instalovat balíček poskytovatele, protože v tomto kurzu použijete SQL Server.</span><span class="sxs-lookup"><span data-stu-id="9d1dc-128">For this tutorial, you don't have to install a provider package because the tutorial uses SQL Server.</span></span> <span data-ttu-id="9d1dc-129">Je součástí balíčku zprostředkovatele SQL Server [Microsoft.AspnetCore.App Microsoft.aspnetcore.all](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span><span class="sxs-lookup"><span data-stu-id="9d1dc-129">The SQL Server provider package is included in the [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span></span>

## <a name="create-the-model"></a><span data-ttu-id="9d1dc-130">Vytvoření modelu</span><span class="sxs-lookup"><span data-stu-id="9d1dc-130">Create the model</span></span>

<span data-ttu-id="9d1dc-131">Definování třídy kontextu a tříd entit, které tvoří modelu:</span><span class="sxs-lookup"><span data-stu-id="9d1dc-131">Define a context class and entity classes that make up the model:</span></span>

* <span data-ttu-id="9d1dc-132">Klikněte pravým tlačítkem na **modely** a pak zvolte položku **Přidat > třída**.</span><span class="sxs-lookup"><span data-stu-id="9d1dc-132">Right-click on the **Models** folder and select **Add > Class**.</span></span>
* <span data-ttu-id="9d1dc-133">Zadejte **Model.cs** jako název a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="9d1dc-133">Enter **Model.cs** as the name and click **OK**.</span></span>
* <span data-ttu-id="9d1dc-134">Obsah souboru nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="9d1dc-134">Replace the contents of the file with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

<span data-ttu-id="9d1dc-135">V reálné aplikaci obvykle vložíte každá třída z vašeho modelu v samostatném souboru.</span><span class="sxs-lookup"><span data-stu-id="9d1dc-135">In a real app you would typically put each class from your model in a separate file.</span></span> <span data-ttu-id="9d1dc-136">V tomto kurzu z důvodu zjednodušení, umístí všechny třídy v jednom souboru.</span><span class="sxs-lookup"><span data-stu-id="9d1dc-136">For the sake of simplicity, this tutorial puts all the classes in one file.</span></span>

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="9d1dc-137">Kontext zaregistrovat injektáž závislostí</span><span class="sxs-lookup"><span data-stu-id="9d1dc-137">Register your context with dependency injection</span></span>

<span data-ttu-id="9d1dc-138">Služby (například `BloggingContext`) jsou registrovány [injektáž závislostí](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) během spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="9d1dc-138">Services (such as `BloggingContext`) are registered with [dependency injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) during application startup.</span></span> <span data-ttu-id="9d1dc-139">Komponenty, které vyžadují tyto služby (například vaše řadiče MVC) jsou pak k dispozici tyto služby prostřednictvím vlastnosti nebo parametry konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="9d1dc-139">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span>

<span data-ttu-id="9d1dc-140">Chcete-li `BloggingContext` k dispozici pro kontrolery MVC, zaregistrujte ho jako služba.</span><span class="sxs-lookup"><span data-stu-id="9d1dc-140">To make `BloggingContext` available to MVC controllers, register it as a service.</span></span>

* <span data-ttu-id="9d1dc-141">Otevřít **Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="9d1dc-141">Open **Startup.cs**</span></span>
* <span data-ttu-id="9d1dc-142">Přidejte následující `using` příkazy:</span><span class="sxs-lookup"><span data-stu-id="9d1dc-142">Add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

<span data-ttu-id="9d1dc-143">Volání `AddDbContext` metody pro registraci kontextu jako služba.</span><span class="sxs-lookup"><span data-stu-id="9d1dc-143">Call the `AddDbContext` method to register the context as a service.</span></span>

* <span data-ttu-id="9d1dc-144">Přidejte následující zvýrazněný kód do `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="9d1dc-144">Add the following highlighted code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=13-14)]

<span data-ttu-id="9d1dc-145">Poznámka: Skutečné aplikace obecně vložili připojovací řetězec v konfigurační soubor nebo prostředí proměnnou.</span><span class="sxs-lookup"><span data-stu-id="9d1dc-145">Note: A real app would generally put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="9d1dc-146">Z důvodu zjednodušení v tomto kurzu ji definuje v kódu.</span><span class="sxs-lookup"><span data-stu-id="9d1dc-146">For the sake of simplicity, this tutorial defines it in code.</span></span> <span data-ttu-id="9d1dc-147">Zobrazit [připojovací řetězce](../../miscellaneous/connection-strings.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="9d1dc-147">See [Connection Strings](../../miscellaneous/connection-strings.md) for more information.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="9d1dc-148">Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="9d1dc-148">Create the database</span></span>

<span data-ttu-id="9d1dc-149">Jakmile budete mít modelu, můžete použít [migrace](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) k vytvoření databáze.</span><span class="sxs-lookup"><span data-stu-id="9d1dc-149">Once you have a model, you can use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="9d1dc-150">**Nástroje > Správce balíčků NuGet > Konzola správce balíčků**</span><span class="sxs-lookup"><span data-stu-id="9d1dc-150">**Tools > NuGet Package Manager > Package Manager Console**</span></span>
* <span data-ttu-id="9d1dc-151">Spustit `Add-Migration InitialCreate` k migraci na vytvoření počáteční sadu tabulek pro váš model.</span><span class="sxs-lookup"><span data-stu-id="9d1dc-151">Run `Add-Migration InitialCreate` to scaffold a migration to create the initial set of tables for your model.</span></span> <span data-ttu-id="9d1dc-152">Pokud se zobrazí chyba `The term 'add-migration' is not recognized as the name of a cmdlet`, zavřete a znovu otevřete Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9d1dc-152">If you receive an error stating `The term 'add-migration' is not recognized as the name of a cmdlet`, close and reopen Visual Studio.</span></span>
* <span data-ttu-id="9d1dc-153">Spustit `Update-Database` použít novou migraci databáze.</span><span class="sxs-lookup"><span data-stu-id="9d1dc-153">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="9d1dc-154">Tento příkaz vytvoří databázi před použitím migrace.</span><span class="sxs-lookup"><span data-stu-id="9d1dc-154">This command creates the database before applying migrations.</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="9d1dc-155">Vytvoření kontroleru</span><span class="sxs-lookup"><span data-stu-id="9d1dc-155">Create a controller</span></span>

<span data-ttu-id="9d1dc-156">Kontroler a zobrazení pro generování uživatelského rozhraní `Blog` entity.</span><span class="sxs-lookup"><span data-stu-id="9d1dc-156">Scaffold a controller and views for the `Blog` entity.</span></span>

* <span data-ttu-id="9d1dc-157">Klikněte pravým tlačítkem na **řadiče** složky **Průzkumníku řešení** a vyberte **Přidat > kontroler**.</span><span class="sxs-lookup"><span data-stu-id="9d1dc-157">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="9d1dc-158">Vyberte **kontroler MVC se zobrazeními, s použitím Entity Framework** a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="9d1dc-158">Select **MVC Controller with views, using Entity Framework** and click **Add**.</span></span>
* <span data-ttu-id="9d1dc-159">Nastavte **třída modelu** k **blogu** a **třída kontextu dat** k **BloggingContext**.</span><span class="sxs-lookup"><span data-stu-id="9d1dc-159">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**.</span></span>
* <span data-ttu-id="9d1dc-160">Klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="9d1dc-160">Click **Add**.</span></span>


## <a name="run-the-application"></a><span data-ttu-id="9d1dc-161">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="9d1dc-161">Run the application</span></span>

<span data-ttu-id="9d1dc-162">Stiskněte klávesu F5 ke spuštění a testování aplikace.</span><span class="sxs-lookup"><span data-stu-id="9d1dc-162">Press F5 to run and test the app.</span></span>

* <span data-ttu-id="9d1dc-163">Přejděte na `/Blogs`</span><span class="sxs-lookup"><span data-stu-id="9d1dc-163">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="9d1dc-164">Pomocí odkazu vytvořit některé položky blogu.</span><span class="sxs-lookup"><span data-stu-id="9d1dc-164">Use the create link to create some blog entries.</span></span> <span data-ttu-id="9d1dc-165">Otestujte podrobnosti a odkazy odstranit.</span><span class="sxs-lookup"><span data-stu-id="9d1dc-165">Test the details and delete links.</span></span>

![obrázek](_static/create.png)

![obrázek](_static/index-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="9d1dc-168">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="9d1dc-168">Additional Resources</span></span>

* <span data-ttu-id="9d1dc-169">[EF - novou databázi pomocí SQLite](xref:core/get-started/netcore/new-db-sqlite) – kurz EF konzole pro různé platformy.</span><span class="sxs-lookup"><span data-stu-id="9d1dc-169">[EF - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="9d1dc-170">Úvod do ASP.NET Core MVC v systému Mac nebo Linux</span><span class="sxs-lookup"><span data-stu-id="9d1dc-170">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="9d1dc-171">Úvod do ASP.NET Core MVC se sadou Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9d1dc-171">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="9d1dc-172">Začínáme s technologiemi ASP.NET Core a Entity Framework Core pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9d1dc-172">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
