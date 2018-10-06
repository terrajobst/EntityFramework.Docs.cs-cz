---
title: Začínáme v ASP.NET Core – nová databáze – EF Core
author: rick-anderson
ms.author: riande
ms.date: 08/03/2018
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 878478099878e4a0bc65c44fef0609d28f39f2b8
ms.sourcegitcommit: 7a7da65404c9338e1e3df42576a13be536a6f95f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2018
ms.locfileid: "48834770"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a><span data-ttu-id="e6d9e-102">Začínáme s EF Core v ASP.NET Core s novou databázi</span><span class="sxs-lookup"><span data-stu-id="e6d9e-102">Getting Started with EF Core on ASP.NET Core with a New database</span></span>

<span data-ttu-id="e6d9e-103">V tomto kurzu sestavíte aplikaci ASP.NET Core MVC, která provádí základní přístup k datům pomocí Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="e6d9e-103">In this tutorial, you build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="e6d9e-104">Tento kurz používá k vytvoření databáze z modelu migrace.</span><span class="sxs-lookup"><span data-stu-id="e6d9e-104">The tutorial uses migrations to create the database from the data model.</span></span>

<span data-ttu-id="e6d9e-105">Tento kurz provedením pomocí sady Visual Studio 2017 na Windows nebo pomocí rozhraní příkazového řádku .NET Core ve Windows, macOS nebo Linuxu.</span><span class="sxs-lookup"><span data-stu-id="e6d9e-105">You can follow the tutorial by using Visual Studio 2017 on Windows, or by using the .NET Core CLI on Windows, macOS, or Linux.</span></span>

<span data-ttu-id="e6d9e-106">Zobrazte ukázky v tomto článku na Githubu:</span><span class="sxs-lookup"><span data-stu-id="e6d9e-106">View this article's sample on GitHub:</span></span>
* [<span data-ttu-id="e6d9e-107">Visual Studio 2017 s SQL serverem</span><span class="sxs-lookup"><span data-stu-id="e6d9e-107">Visual Studio 2017 with SQL Server</span></span>](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb)
* <span data-ttu-id="e6d9e-108">[.NET core CLI s SQLite](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite).</span><span class="sxs-lookup"><span data-stu-id="e6d9e-108">[.NET Core CLI with SQLite](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e6d9e-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e6d9e-109">Prerequisites</span></span>

<span data-ttu-id="e6d9e-110">Nainstalujte následující software:</span><span class="sxs-lookup"><span data-stu-id="e6d9e-110">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e6d9e-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e6d9e-111">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e6d9e-112">[Visual Studio 2017 verze 15.7 nebo novější](https://www.visualstudio.com/downloads/) se tyto úlohy:</span><span class="sxs-lookup"><span data-stu-id="e6d9e-112">[Visual Studio 2017 version 15.7 or later](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="e6d9e-113">**Vývoj pro ASP.NET a web** (v části **Web a Cloud**)</span><span class="sxs-lookup"><span data-stu-id="e6d9e-113">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="e6d9e-114">**Vývoj pro různé platformy .NET core** (v části **další sady nástrojů**)</span><span class="sxs-lookup"><span data-stu-id="e6d9e-114">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="e6d9e-115">[.NET core 2.1 SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="e6d9e-115">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e6d9e-116">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="e6d9e-116">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="e6d9e-117">[.NET core 2.1 SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="e6d9e-117">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

---

## <a name="create-a-new-project"></a><span data-ttu-id="e6d9e-118">Vytvoření nového projektu</span><span class="sxs-lookup"><span data-stu-id="e6d9e-118">Create a new project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e6d9e-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e6d9e-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e6d9e-120">Otevřít Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="e6d9e-120">Open Visual Studio 2017</span></span>
* <span data-ttu-id="e6d9e-121">**Soubor > Nový > Projekt**</span><span class="sxs-lookup"><span data-stu-id="e6d9e-121">**File > New > Project**</span></span>
* <span data-ttu-id="e6d9e-122">V nabídce vlevo vyberte **nainstalováno > Visual C# > .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="e6d9e-122">From the left menu, select **Installed > Visual C# > .NET Core**.</span></span>
* <span data-ttu-id="e6d9e-123">Vyberte **webová aplikace ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="e6d9e-123">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="e6d9e-124">Zadejte **EFGetStarted.AspNetCore.NewDb** název a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="e6d9e-124">Enter **EFGetStarted.AspNetCore.NewDb** for the name and click **OK**.</span></span>
* <span data-ttu-id="e6d9e-125">V **nová webová aplikace ASP.NET Core** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="e6d9e-125">In the **New ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="e6d9e-126">Ujistěte se, že **.NET Core** a **ASP.NET Core 2.1** jsou vybrány v rozevíracích seznamech</span><span class="sxs-lookup"><span data-stu-id="e6d9e-126">Make sure that **.NET Core** and **ASP.NET Core 2.1** are selected in the drop-down lists</span></span>
  * <span data-ttu-id="e6d9e-127">Vyberte **webové aplikace (Model-View-Controller)** šablony projektu</span><span class="sxs-lookup"><span data-stu-id="e6d9e-127">Select the **Web Application (Model-View-Controller)** project template</span></span>
  * <span data-ttu-id="e6d9e-128">Ujistěte se, že **ověřování** je nastavena na **bez ověřování**</span><span class="sxs-lookup"><span data-stu-id="e6d9e-128">Make sure that **Authentication** is set to **No Authentication**</span></span>
  * <span data-ttu-id="e6d9e-129">Klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="e6d9e-129">Click **OK**</span></span>

<span data-ttu-id="e6d9e-130">Upozornění: Pokud používáte **jednotlivé uživatelské účty** místo **žádný** pro **ověřování** pak model Entity Framework Core se přidá do projektu v `Models\IdentityModel.cs`.</span><span class="sxs-lookup"><span data-stu-id="e6d9e-130">Warning: If you use **Individual User Accounts** instead of **None** for **Authentication** then an Entity Framework Core model will be added to the project in `Models\IdentityModel.cs`.</span></span> <span data-ttu-id="e6d9e-131">Pomocí technik, které se naučíte v tomto kurzu, můžete přidat druhý modelu nebo rozšiřte tento existující model tak, aby obsahovala tříd entit.</span><span class="sxs-lookup"><span data-stu-id="e6d9e-131">Using the techniques you learn in this tutorial, you can choose to add a second model, or extend this existing model to contain your entity classes.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e6d9e-132">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="e6d9e-132">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="e6d9e-133">Spusťte následující příkaz pro vytvoření projektu MVC:</span><span class="sxs-lookup"><span data-stu-id="e6d9e-133">Run the following command to create an MVC project:</span></span>

   ```cli
   dotnet new mvc -n EFGetStarted.AspNetCore.NewDb
   ```
* <span data-ttu-id="e6d9e-134">Přejděte do adresáře projektu.</span><span class="sxs-lookup"><span data-stu-id="e6d9e-134">Change to the project directory.</span></span> <span data-ttu-id="e6d9e-135">Další příkazy, které zadáte nutné ke spuštění nového projektu.</span><span class="sxs-lookup"><span data-stu-id="e6d9e-135">The next commands you enter need to run against the new project.</span></span>

   ```cli
   cd EFGetStarted.AspNetCore.NewDb
   ```
---

## <a name="install-entity-framework-core"></a><span data-ttu-id="e6d9e-136">Nainstalujte Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="e6d9e-136">Install Entity Framework Core</span></span>

<span data-ttu-id="e6d9e-137">Instalace EF Core, nainstalujte balíček vytvořeno EF Core databáze, kterou chcete cílit na pro.</span><span class="sxs-lookup"><span data-stu-id="e6d9e-137">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="e6d9e-138">Seznam dostupných zprostředkovatelů najdete v tématu [poskytovatelé databází](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="e6d9e-138">For a list of available providers, see [Database Providers](../../providers/index.md).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e6d9e-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e6d9e-139">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e6d9e-140">Pro účely tohoto kurzu není nutné instalovat balíček poskytovatele, protože v tomto kurzu použijete SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e6d9e-140">For this tutorial, you don't have to install a provider package because the tutorial uses SQL Server.</span></span> <span data-ttu-id="e6d9e-141">Je součástí balíčku zprostředkovatele SQL Server [Microsoft.AspnetCore.App Microsoft.aspnetcore.all](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span><span class="sxs-lookup"><span data-stu-id="e6d9e-141">The SQL Server provider package is included in the [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e6d9e-142">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="e6d9e-142">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="e6d9e-143">Tento kurz používá SQLite, protože běží na všech platformách, které podporuje .NET Core.</span><span class="sxs-lookup"><span data-stu-id="e6d9e-143">This tutorial uses SQLite because it runs on all platforms that .NET Core supports.</span></span>

* <span data-ttu-id="e6d9e-144">Spusťte následující příkaz k instalaci zprostředkovatele SQLite:</span><span class="sxs-lookup"><span data-stu-id="e6d9e-144">Run the following command to install the SQLite provider:</span></span>

   ```cli
   dotnet add package Microsoft.EntityFrameworkCore.Sqlite
   ```

---

## <a name="create-the-model"></a><span data-ttu-id="e6d9e-145">Vytvoření modelu</span><span class="sxs-lookup"><span data-stu-id="e6d9e-145">Create the model</span></span>

<span data-ttu-id="e6d9e-146">Definování třídy kontextu a tříd entit, které tvoří modelu.</span><span class="sxs-lookup"><span data-stu-id="e6d9e-146">Define a context class and entity classes that make up the model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e6d9e-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e6d9e-147">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e6d9e-148">Klikněte pravým tlačítkem na **modely** a pak zvolte položku **Přidat > třída**.</span><span class="sxs-lookup"><span data-stu-id="e6d9e-148">Right-click on the **Models** folder and select **Add > Class**.</span></span>
* <span data-ttu-id="e6d9e-149">Zadejte **Model.cs** jako název a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="e6d9e-149">Enter **Model.cs** as the name and click **OK**.</span></span>
* <span data-ttu-id="e6d9e-150">Obsah souboru nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="e6d9e-150">Replace the contents of the file with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e6d9e-151">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="e6d9e-151">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="e6d9e-152">V **modely** vytvořit složku **Model.cs** následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="e6d9e-152">In the **Models** folder create **Model.cs** with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Models/Model.cs)]

---

<span data-ttu-id="e6d9e-153">Produkční aplikace obvykle vložili každá třída v samostatném souboru.</span><span class="sxs-lookup"><span data-stu-id="e6d9e-153">A production app would typically put each class in a separate file.</span></span> <span data-ttu-id="e6d9e-154">V tomto kurzu z důvodu zjednodušení, vloží tyto třídy v jednom souboru.</span><span class="sxs-lookup"><span data-stu-id="e6d9e-154">For the sake of simplicity, this tutorial puts these classes in one file.</span></span>

## <a name="register-the-context-with-dependency-injection"></a><span data-ttu-id="e6d9e-155">Zaregistrovat kontext pomocí vkládání závislostí</span><span class="sxs-lookup"><span data-stu-id="e6d9e-155">Register the context with dependency injection</span></span>

<span data-ttu-id="e6d9e-156">Služby (například `BloggingContext`) jsou registrovány [injektáž závislostí](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) během spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="e6d9e-156">Services (such as `BloggingContext`) are registered with [dependency injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) during application startup.</span></span> <span data-ttu-id="e6d9e-157">Komponenty, které vyžadují tyto služby (jako jsou řadiče MVC) jsou k dispozici tyto služby prostřednictvím vlastnosti nebo parametry konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="e6d9e-157">Components that require these services (such as MVC controllers) are provided these services via constructor parameters or properties.</span></span>

<span data-ttu-id="e6d9e-158">Chcete-li `BloggingContext` k dispozici pro kontrolery MVC, zaregistrujte ho jako služba.</span><span class="sxs-lookup"><span data-stu-id="e6d9e-158">To make `BloggingContext` available to MVC controllers, register it as a service.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e6d9e-159">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e6d9e-159">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e6d9e-160">V **Startup.cs** přidejte následující `using` příkazy:</span><span class="sxs-lookup"><span data-stu-id="e6d9e-160">In **Startup.cs** add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

* <span data-ttu-id="e6d9e-161">Přidejte následující zvýrazněný kód do `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="e6d9e-161">Add the following highlighted code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=12-14)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e6d9e-162">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="e6d9e-162">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="e6d9e-163">V **Startup.cs** přidejte následující `using` příkazy:</span><span class="sxs-lookup"><span data-stu-id="e6d9e-163">In **Startup.cs** add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs#AddedUsings)]

* <span data-ttu-id="e6d9e-164">Přidejte následující zvýrazněný kód do `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="e6d9e-164">Add the following highlighted code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs?name=ConfigureServices&highlight=12-14)]

---

<span data-ttu-id="e6d9e-165">Produkční aplikace obvykle vložili připojovací řetězec v konfigurační soubor nebo prostředí proměnnou.</span><span class="sxs-lookup"><span data-stu-id="e6d9e-165">A production app would typically put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="e6d9e-166">Z důvodu zjednodušení v tomto kurzu ji definuje v kódu.</span><span class="sxs-lookup"><span data-stu-id="e6d9e-166">For the sake of simplicity, this tutorial defines it in code.</span></span> <span data-ttu-id="e6d9e-167">Zobrazit [připojovací řetězce](../../miscellaneous/connection-strings.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="e6d9e-167">See [Connection Strings](../../miscellaneous/connection-strings.md) for more information.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="e6d9e-168">Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="e6d9e-168">Create the database</span></span>

<span data-ttu-id="e6d9e-169">Následující kroky použijte [migrace](xref:core/managing-schemas/migrations/index) k vytvoření databáze.</span><span class="sxs-lookup"><span data-stu-id="e6d9e-169">The following steps use [migrations](xref:core/managing-schemas/migrations/index) to create a database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e6d9e-170">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e6d9e-170">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e6d9e-171">**Nástroje > Správce balíčků NuGet > Konzola správce balíčků**</span><span class="sxs-lookup"><span data-stu-id="e6d9e-171">**Tools > NuGet Package Manager > Package Manager Console**</span></span>
* <span data-ttu-id="e6d9e-172">Spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="e6d9e-172">Run the following commands:</span></span>

  ```powershell
  Add-Migration InitialCreate
  Update-Database
  ```

  <span data-ttu-id="e6d9e-173">Pokud se zobrazí chyba `The term 'add-migration' is not recognized as the name of a cmdlet`, zavřete a znovu otevřete Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e6d9e-173">If you get an error stating `The term 'add-migration' is not recognized as the name of a cmdlet`, close and reopen Visual Studio.</span></span>

  <span data-ttu-id="e6d9e-174">`Add-Migration` Příkaz vygeneruje uživatelské rozhraní migrace vytvořte počáteční sadu tabulek pro model.</span><span class="sxs-lookup"><span data-stu-id="e6d9e-174">The `Add-Migration` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="e6d9e-175">`Update-Database` Příkaz vytvoří databázi a použije novou migraci na něj.</span><span class="sxs-lookup"><span data-stu-id="e6d9e-175">The `Update-Database` command creates the database and applies the new migration to it.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e6d9e-176">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="e6d9e-176">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="e6d9e-177">Spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="e6d9e-177">Run the following commands:</span></span>

  ```cli
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  <span data-ttu-id="e6d9e-178">`migrations` Příkaz vygeneruje uživatelské rozhraní migrace vytvořte počáteční sadu tabulek pro model.</span><span class="sxs-lookup"><span data-stu-id="e6d9e-178">The `migrations` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="e6d9e-179">`database update` Příkaz vytvoří databázi a použije novou migraci na něj.</span><span class="sxs-lookup"><span data-stu-id="e6d9e-179">The `database update` command creates the database and applies the new migration to it.</span></span>

---

## <a name="create-a-controller"></a><span data-ttu-id="e6d9e-180">Vytvoření kontroleru</span><span class="sxs-lookup"><span data-stu-id="e6d9e-180">Create a controller</span></span>

<span data-ttu-id="e6d9e-181">Kontroler a zobrazení pro generování uživatelského rozhraní `Blog` entity.</span><span class="sxs-lookup"><span data-stu-id="e6d9e-181">Scaffold a controller and views for the `Blog` entity.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e6d9e-182">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e6d9e-182">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e6d9e-183">Klikněte pravým tlačítkem na **řadiče** složky **Průzkumníku řešení** a vyberte **Přidat > kontroler**.</span><span class="sxs-lookup"><span data-stu-id="e6d9e-183">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="e6d9e-184">Vyberte **kontroler MVC se zobrazeními, s použitím Entity Framework** a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="e6d9e-184">Select **MVC Controller with views, using Entity Framework** and click **Add**.</span></span>
* <span data-ttu-id="e6d9e-185">Nastavte **třída modelu** k **blogu** a **třída kontextu dat** k **BloggingContext**.</span><span class="sxs-lookup"><span data-stu-id="e6d9e-185">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**.</span></span>
* <span data-ttu-id="e6d9e-186">Klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="e6d9e-186">Click **Add**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e6d9e-187">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="e6d9e-187">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="e6d9e-188">Spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="e6d9e-188">Run the following commands:</span></span>

  ```cli
  dotnet tool install -g dotnet-aspnet-codegenerator
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
  dotnet restore
  dotnet aspnet-codegenerator controller -name BlogsController -m Blog -dc BloggingContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  <span data-ttu-id="e6d9e-189">`tool install` a `add package` příkazy instalace nástrojů, můžete vygenerovat kontrolerů a zobrazení.</span><span class="sxs-lookup"><span data-stu-id="e6d9e-189">The `tool install` and `add package` commands install the tooling that can scaffold controllers and views.</span></span> <span data-ttu-id="e6d9e-190">`restore` Příkaz zajistí, že se stáhnou všechny balíčky v projektu a `aspnet-codegenerator` příkaz dělá základní kostry aplikace.</span><span class="sxs-lookup"><span data-stu-id="e6d9e-190">The `restore` command ensures that all of the project's packages are downloaded, and the `aspnet-codegenerator` command does the scaffolding.</span></span>
---

<span data-ttu-id="e6d9e-191">Modul generování uživatelského rozhraní vytvoří následující soubory:</span><span class="sxs-lookup"><span data-stu-id="e6d9e-191">The scaffolding engine creates the following files:</span></span>

* <span data-ttu-id="e6d9e-192">Kontroler (*Controllers/BlogsController.cs*)</span><span class="sxs-lookup"><span data-stu-id="e6d9e-192">A controller (*Controllers/BlogsController.cs*)</span></span>
* <span data-ttu-id="e6d9e-193">Zobrazení syntaxe Razor pro vytvořit, odstranit, podrobností, úpravy a Index stránky (_Views/Movies/\*.cshtml_)</span><span class="sxs-lookup"><span data-stu-id="e6d9e-193">Razor views for Create, Delete, Details, Edit, and Index pages (_Views/Movies/\*.cshtml_)</span></span>

## <a name="run-the-application"></a><span data-ttu-id="e6d9e-194">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="e6d9e-194">Run the application</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e6d9e-195">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e6d9e-195">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e6d9e-196">**Ladění** > **spustit bez ladění**</span><span class="sxs-lookup"><span data-stu-id="e6d9e-196">**Debug** > **Start Without Debugging**</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e6d9e-197">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="e6d9e-197">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet run
```
---

* <span data-ttu-id="e6d9e-198">Přejděte na `/Blogs`</span><span class="sxs-lookup"><span data-stu-id="e6d9e-198">Navigate to `/Blogs`</span></span>

* <span data-ttu-id="e6d9e-199">Použití **vytvořit nový** odkaz můžete vytvořit některé položky blogu.</span><span class="sxs-lookup"><span data-stu-id="e6d9e-199">Use the **Create New** link to create some blog entries.</span></span>

  ![Vytvoření stránky](_static/create.png)

* <span data-ttu-id="e6d9e-201">Test **podrobnosti**, **upravit**, a **odstranit** odkazy.</span><span class="sxs-lookup"><span data-stu-id="e6d9e-201">Test the **Details**, **Edit**, and **Delete** links.</span></span>

  ![Indexová stránka](_static/index-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="e6d9e-203">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="e6d9e-203">Additional Resources</span></span>

* [<span data-ttu-id="e6d9e-204">Kurz: Začínáme s EF Core na .NET Core s novou databázi pomocí SQLite</span><span class="sxs-lookup"><span data-stu-id="e6d9e-204">Tutorial: Get started with EF Core on .NET Core with a new database using SQLite</span></span>](xref:core/get-started/netcore/new-db-sqlite)
* [<span data-ttu-id="e6d9e-205">Kurz: Začínáme se stránkami Razor v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e6d9e-205">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="e6d9e-206">Kurz: Stránky Razor pomocí Entity Framework Core v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e6d9e-206">Tutorial: Razor Pages with Entity Framework Core in ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro)
