---
title: Začínáme v ASP.NET Core – nová databáze – EF Core
author: rick-anderson
ms.author: riande
ms.date: 08/03/2018
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 5f69be179eb19c2b7e5743698876de441f1ccb91
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250930"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a><span data-ttu-id="363b1-102">Začínáme s EF Core v ASP.NET Core s novou databázi</span><span class="sxs-lookup"><span data-stu-id="363b1-102">Getting Started with EF Core on ASP.NET Core with a New database</span></span>

<span data-ttu-id="363b1-103">V tomto kurzu sestavíte aplikaci ASP.NET Core MVC, která provádí základní přístup k datům pomocí Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="363b1-103">In this tutorial, you build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="363b1-104">Tento kurz používá k vytvoření databáze z modelu migrace.</span><span class="sxs-lookup"><span data-stu-id="363b1-104">The tutorial uses migrations to create the database from the data model.</span></span>

<span data-ttu-id="363b1-105">Tento kurz provedením pomocí sady Visual Studio 2017 na Windows nebo pomocí rozhraní příkazového řádku .NET Core ve Windows, macOS nebo Linuxu.</span><span class="sxs-lookup"><span data-stu-id="363b1-105">You can follow the tutorial by using Visual Studio 2017 on Windows, or by using the .NET Core CLI on Windows, macOS, or Linux.</span></span>

<span data-ttu-id="363b1-106">Zobrazte ukázky v tomto článku na Githubu:</span><span class="sxs-lookup"><span data-stu-id="363b1-106">View this article's sample on GitHub:</span></span>
* [<span data-ttu-id="363b1-107">Visual Studio 2017 s SQL serverem</span><span class="sxs-lookup"><span data-stu-id="363b1-107">Visual Studio 2017 with SQL Server</span></span>](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb)
* <span data-ttu-id="363b1-108">[.NET core CLI s SQLite](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCoreNewDb.Sqlite/EFGetStarted.AspNetCore.NewDb).</span><span class="sxs-lookup"><span data-stu-id="363b1-108">[.NET Core CLI with SQLite](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCoreNewDb.Sqlite/EFGetStarted.AspNetCore.NewDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="363b1-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="363b1-109">Prerequisites</span></span>

<span data-ttu-id="363b1-110">Nainstalujte následující software:</span><span class="sxs-lookup"><span data-stu-id="363b1-110">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="363b1-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="363b1-111">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="363b1-112">[Visual Studio 2017 verze 15.7 nebo novější](https://www.visualstudio.com/downloads/) se tyto úlohy:</span><span class="sxs-lookup"><span data-stu-id="363b1-112">[Visual Studio 2017 version 15.7 or later](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="363b1-113">**Vývoj pro ASP.NET a web** (v části **Web a Cloud**)</span><span class="sxs-lookup"><span data-stu-id="363b1-113">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="363b1-114">**Vývoj pro různé platformy .NET core** (v části **další sady nástrojů**)</span><span class="sxs-lookup"><span data-stu-id="363b1-114">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="363b1-115">[.NET core 2.1 SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="363b1-115">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="363b1-116">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="363b1-116">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="363b1-117">[.NET core 2.1 SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="363b1-117">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

---

## <a name="create-a-new-project"></a><span data-ttu-id="363b1-118">Vytvoření nového projektu</span><span class="sxs-lookup"><span data-stu-id="363b1-118">Create a new project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="363b1-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="363b1-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="363b1-120">Otevřít Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="363b1-120">Open Visual Studio 2017</span></span>
* <span data-ttu-id="363b1-121">**Soubor > Nový > Projekt**</span><span class="sxs-lookup"><span data-stu-id="363b1-121">**File > New > Project**</span></span>
* <span data-ttu-id="363b1-122">V nabídce vlevo vyberte **nainstalováno > Visual C# > .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="363b1-122">From the left menu, select **Installed > Visual C# > .NET Core**.</span></span>
* <span data-ttu-id="363b1-123">Vyberte **webová aplikace ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="363b1-123">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="363b1-124">Zadejte **EFGetStarted.AspNetCore.NewDb** název a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="363b1-124">Enter **EFGetStarted.AspNetCore.NewDb** for the name and click **OK**.</span></span>
* <span data-ttu-id="363b1-125">V **nová webová aplikace ASP.NET Core** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="363b1-125">In the **New ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="363b1-126">Ujistěte se, že **.NET Core** a **ASP.NET Core 2.1** jsou vybrány v rozevíracích seznamech</span><span class="sxs-lookup"><span data-stu-id="363b1-126">Make sure that **.NET Core** and **ASP.NET Core 2.1** are selected in the drop-down lists</span></span>
  * <span data-ttu-id="363b1-127">Vyberte **webové aplikace (Model-View-Controller)** šablony projektu</span><span class="sxs-lookup"><span data-stu-id="363b1-127">Select the **Web Application (Model-View-Controller)** project template</span></span>
  * <span data-ttu-id="363b1-128">Ujistěte se, že **ověřování** je nastavena na **bez ověřování**</span><span class="sxs-lookup"><span data-stu-id="363b1-128">Make sure that **Authentication** is set to **No Authentication**</span></span>
  * <span data-ttu-id="363b1-129">Klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="363b1-129">Click **OK**</span></span>

<span data-ttu-id="363b1-130">Upozornění: Pokud používáte **jednotlivé uživatelské účty** místo **žádný** pro **ověřování** pak model Entity Framework Core se přidá do projektu v `Models\IdentityModel.cs`.</span><span class="sxs-lookup"><span data-stu-id="363b1-130">Warning: If you use **Individual User Accounts** instead of **None** for **Authentication** then an Entity Framework Core model will be added to the project in `Models\IdentityModel.cs`.</span></span> <span data-ttu-id="363b1-131">Pomocí technik, které se naučíte v tomto kurzu, můžete přidat druhý modelu nebo rozšiřte tento existující model tak, aby obsahovala tříd entit.</span><span class="sxs-lookup"><span data-stu-id="363b1-131">Using the techniques you learn in this tutorial, you can choose to add a second model, or extend this existing model to contain your entity classes.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="363b1-132">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="363b1-132">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="363b1-133">Spusťte následující příkaz pro vytvoření projektu MVC:</span><span class="sxs-lookup"><span data-stu-id="363b1-133">Run the following command to create an MVC project:</span></span>

   ```cli
   dotnet new mvc -n EFGetStarted.AspNetCore.NewDb
   ```
* <span data-ttu-id="363b1-134">Přejděte do adresáře projektu.</span><span class="sxs-lookup"><span data-stu-id="363b1-134">Change to the project directory.</span></span> <span data-ttu-id="363b1-135">Další příkazy, které zadáte nutné ke spuštění nového projektu.</span><span class="sxs-lookup"><span data-stu-id="363b1-135">The next commands you enter need to run against the new project.</span></span>

   ```cli
   cd EFGetStarted.AspNetCore.NewDb
   ```
---

## <a name="install-entity-framework-core"></a><span data-ttu-id="363b1-136">Nainstalujte Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="363b1-136">Install Entity Framework Core</span></span>

<span data-ttu-id="363b1-137">Instalace EF Core, nainstalujte balíček vytvořeno EF Core databáze, kterou chcete cílit na pro.</span><span class="sxs-lookup"><span data-stu-id="363b1-137">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="363b1-138">Seznam dostupných zprostředkovatelů najdete v tématu [poskytovatelé databází](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="363b1-138">For a list of available providers, see [Database Providers](../../providers/index.md).</span></span> 

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="363b1-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="363b1-139">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="363b1-140">Pro účely tohoto kurzu není nutné instalovat balíček poskytovatele, protože v tomto kurzu použijete SQL Server.</span><span class="sxs-lookup"><span data-stu-id="363b1-140">For this tutorial, you don't have to install a provider package because the tutorial uses SQL Server.</span></span> <span data-ttu-id="363b1-141">Je součástí balíčku zprostředkovatele SQL Server [Microsoft.AspnetCore.App Microsoft.aspnetcore.all](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span><span class="sxs-lookup"><span data-stu-id="363b1-141">The SQL Server provider package is included in the [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="363b1-142">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="363b1-142">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="363b1-143">Tento kurz používá SQLite, protože běží na všech platformách, které podporuje .NET Core.</span><span class="sxs-lookup"><span data-stu-id="363b1-143">This tutorial uses SQLite because it runs on all platforms that .NET Core supports.</span></span>

* <span data-ttu-id="363b1-144">Spusťte následující příkaz k instalaci zprostředkovatele SQLite:</span><span class="sxs-lookup"><span data-stu-id="363b1-144">Run the following command to install the SQLite provider:</span></span>

   ```cli
   dotnet add package Microsoft.EntityFrameworkCore.Sqlite
   ```

---

## <a name="create-the-model"></a><span data-ttu-id="363b1-145">Vytvoření modelu</span><span class="sxs-lookup"><span data-stu-id="363b1-145">Create the model</span></span>

<span data-ttu-id="363b1-146">Definování třídy kontextu a tříd entit, které tvoří modelu.</span><span class="sxs-lookup"><span data-stu-id="363b1-146">Define a context class and entity classes that make up the model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="363b1-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="363b1-147">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="363b1-148">Klikněte pravým tlačítkem na **modely** a pak zvolte položku **Přidat > třída**.</span><span class="sxs-lookup"><span data-stu-id="363b1-148">Right-click on the **Models** folder and select **Add > Class**.</span></span>
* <span data-ttu-id="363b1-149">Zadejte **Model.cs** jako název a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="363b1-149">Enter **Model.cs** as the name and click **OK**.</span></span>
* <span data-ttu-id="363b1-150">Obsah souboru nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="363b1-150">Replace the contents of the file with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="363b1-151">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="363b1-151">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="363b1-152">V **modely** vytvořit složku **Model.cs** následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="363b1-152">In the **Models** folder create **Model.cs** with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Models/Model.cs)]

---

<span data-ttu-id="363b1-153">Produkční aplikace obvykle vložili každá třída v samostatném souboru.</span><span class="sxs-lookup"><span data-stu-id="363b1-153">A production app would typically put each class in a separate file.</span></span> <span data-ttu-id="363b1-154">V tomto kurzu z důvodu zjednodušení, vloží tyto třídy v jednom souboru.</span><span class="sxs-lookup"><span data-stu-id="363b1-154">For the sake of simplicity, this tutorial puts these classes in one file.</span></span>

## <a name="register-the-context-with-dependency-injection"></a><span data-ttu-id="363b1-155">Zaregistrovat kontext pomocí vkládání závislostí</span><span class="sxs-lookup"><span data-stu-id="363b1-155">Register the context with dependency injection</span></span>

<span data-ttu-id="363b1-156">Služby (například `BloggingContext`) jsou registrovány [injektáž závislostí](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) během spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="363b1-156">Services (such as `BloggingContext`) are registered with [dependency injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) during application startup.</span></span> <span data-ttu-id="363b1-157">Komponenty, které vyžadují tyto služby (jako jsou řadiče MVC) jsou k dispozici tyto služby prostřednictvím vlastnosti nebo parametry konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="363b1-157">Components that require these services (such as MVC controllers) are provided these services via constructor parameters or properties.</span></span>

<span data-ttu-id="363b1-158">Chcete-li `BloggingContext` k dispozici pro kontrolery MVC, zaregistrujte ho jako služba.</span><span class="sxs-lookup"><span data-stu-id="363b1-158">To make `BloggingContext` available to MVC controllers, register it as a service.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="363b1-159">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="363b1-159">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="363b1-160">V **Startup.cs** přidejte následující `using` příkazy:</span><span class="sxs-lookup"><span data-stu-id="363b1-160">In **Startup.cs** add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

* <span data-ttu-id="363b1-161">Přidejte následující zvýrazněný kód do `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="363b1-161">Add the following highlighted code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=12-14)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="363b1-162">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="363b1-162">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="363b1-163">V **Startup.cs** přidejte následující `using` příkazy:</span><span class="sxs-lookup"><span data-stu-id="363b1-163">In **Startup.cs** add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs#AddedUsings)]

* <span data-ttu-id="363b1-164">Přidejte následující zvýrazněný kód do `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="363b1-164">Add the following highlighted code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs?name=ConfigureServices&highlight=12-14)]

---

<span data-ttu-id="363b1-165">Produkční aplikace obvykle vložili připojovací řetězec v konfigurační soubor nebo prostředí proměnnou.</span><span class="sxs-lookup"><span data-stu-id="363b1-165">A production app would typically put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="363b1-166">Z důvodu zjednodušení v tomto kurzu ji definuje v kódu.</span><span class="sxs-lookup"><span data-stu-id="363b1-166">For the sake of simplicity, this tutorial defines it in code.</span></span> <span data-ttu-id="363b1-167">Zobrazit [připojovací řetězce](../../miscellaneous/connection-strings.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="363b1-167">See [Connection Strings](../../miscellaneous/connection-strings.md) for more information.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="363b1-168">Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="363b1-168">Create the database</span></span>

<span data-ttu-id="363b1-169">Následující kroky použijte [migrace](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) k vytvoření databáze.</span><span class="sxs-lookup"><span data-stu-id="363b1-169">The following steps use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="363b1-170">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="363b1-170">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="363b1-171">**Nástroje > Správce balíčků NuGet > Konzola správce balíčků**</span><span class="sxs-lookup"><span data-stu-id="363b1-171">**Tools > NuGet Package Manager > Package Manager Console**</span></span>
* <span data-ttu-id="363b1-172">Spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="363b1-172">Run the following commands:</span></span>

  ```powershell
  Add-Migration InitialCreate
  Update-Database
  ```

  <span data-ttu-id="363b1-173">Pokud se zobrazí chyba `The term 'add-migration' is not recognized as the name of a cmdlet`, zavřete a znovu otevřete Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="363b1-173">If you get an error stating `The term 'add-migration' is not recognized as the name of a cmdlet`, close and reopen Visual Studio.</span></span>

  <span data-ttu-id="363b1-174">`Add-Migration` Příkaz vygeneruje uživatelské rozhraní migrace vytvořte počáteční sadu tabulek pro model.</span><span class="sxs-lookup"><span data-stu-id="363b1-174">The `Add-Migration` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="363b1-175">`Update-Database` Příkaz vytvoří databázi a použije novou migraci na něj.</span><span class="sxs-lookup"><span data-stu-id="363b1-175">The `Update-Database` command creates the database and applies the new migration to it.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="363b1-176">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="363b1-176">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="363b1-177">Spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="363b1-177">Run the following commands:</span></span>

  ```cli
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  <span data-ttu-id="363b1-178">`migrations` Příkaz vygeneruje uživatelské rozhraní migrace vytvořte počáteční sadu tabulek pro model.</span><span class="sxs-lookup"><span data-stu-id="363b1-178">The `migrations` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="363b1-179">`database update` Příkaz vytvoří databázi a použije novou migraci na něj.</span><span class="sxs-lookup"><span data-stu-id="363b1-179">The `database update` command creates the database and applies the new migration to it.</span></span>

---

## <a name="create-a-controller"></a><span data-ttu-id="363b1-180">Vytvoření kontroleru</span><span class="sxs-lookup"><span data-stu-id="363b1-180">Create a controller</span></span>

<span data-ttu-id="363b1-181">Kontroler a zobrazení pro generování uživatelského rozhraní `Blog` entity.</span><span class="sxs-lookup"><span data-stu-id="363b1-181">Scaffold a controller and views for the `Blog` entity.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="363b1-182">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="363b1-182">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="363b1-183">Klikněte pravým tlačítkem na **řadiče** složky **Průzkumníku řešení** a vyberte **Přidat > kontroler**.</span><span class="sxs-lookup"><span data-stu-id="363b1-183">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="363b1-184">Vyberte **kontroler MVC se zobrazeními, s použitím Entity Framework** a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="363b1-184">Select **MVC Controller with views, using Entity Framework** and click **Add**.</span></span>
* <span data-ttu-id="363b1-185">Nastavte **třída modelu** k **blogu** a **třída kontextu dat** k **BloggingContext**.</span><span class="sxs-lookup"><span data-stu-id="363b1-185">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**.</span></span>
* <span data-ttu-id="363b1-186">Klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="363b1-186">Click **Add**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="363b1-187">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="363b1-187">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="363b1-188">Spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="363b1-188">Run the following commands:</span></span>

  ```cli
  dotnet tool install -g dotnet-aspnet-codegenerator
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
  dotnet restore
  dotnet aspnet-codegenerator controller -name BlogsController -m Blog -dc BloggingContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
  ```

  <span data-ttu-id="363b1-189">`tool install` a `add package` příkazy instalace nástrojů, můžete vygenerovat kontrolerů a zobrazení.</span><span class="sxs-lookup"><span data-stu-id="363b1-189">The `tool install` and `add package` commands install the tooling that can scaffold controllers and views.</span></span> <span data-ttu-id="363b1-190">`restore` Příkaz zajistí, že se stáhnou všechny balíčky v projektu a `aspnet-codegenerator` příkaz dělá základní kostry aplikace.</span><span class="sxs-lookup"><span data-stu-id="363b1-190">The `restore` command ensures that all of the project's packages are downloaded, and the `aspnet-codegenerator` command does the scaffolding.</span></span>
---

<span data-ttu-id="363b1-191">Modul generování uživatelského rozhraní vytvoří následující soubory:</span><span class="sxs-lookup"><span data-stu-id="363b1-191">The scaffolding engine creates the following files:</span></span>

* <span data-ttu-id="363b1-192">Kontroler (*Controllers/BlogsController.cs*)</span><span class="sxs-lookup"><span data-stu-id="363b1-192">A controller (*Controllers/BlogsController.cs*)</span></span>
* <span data-ttu-id="363b1-193">Zobrazení syntaxe Razor pro vytvořit, odstranit, podrobností, úpravy a Index stránky (_Views/Movies/\*.cshtml_)</span><span class="sxs-lookup"><span data-stu-id="363b1-193">Razor views for Create, Delete, Details, Edit, and Index pages (_Views/Movies/\*.cshtml_)</span></span>

## <a name="run-the-application"></a><span data-ttu-id="363b1-194">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="363b1-194">Run the application</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="363b1-195">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="363b1-195">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="363b1-196">**Ladění** > **spustit bez ladění**</span><span class="sxs-lookup"><span data-stu-id="363b1-196">**Debug** > **Start Without Debugging**</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="363b1-197">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="363b1-197">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet run
```
---

* <span data-ttu-id="363b1-198">Přejděte na `/Blogs`</span><span class="sxs-lookup"><span data-stu-id="363b1-198">Navigate to `/Blogs`</span></span>

* <span data-ttu-id="363b1-199">Použití **vytvořit nový** odkaz můžete vytvořit některé položky blogu.</span><span class="sxs-lookup"><span data-stu-id="363b1-199">Use the **Create New** link to create some blog entries.</span></span>

  ![Vytvoření stránky](_static/create.png)

* <span data-ttu-id="363b1-201">Test **podrobnosti**, **upravit**, a **odstranit** odkazy.</span><span class="sxs-lookup"><span data-stu-id="363b1-201">Test the **Details**, **Edit**, and **Delete** links.</span></span>

  ![Indexová stránka](_static/index-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="363b1-203">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="363b1-203">Additional Resources</span></span>

* <span data-ttu-id="363b1-204">[EF - novou databázi pomocí SQLite](xref:core/get-started/netcore/new-db-sqlite) – kurz EF konzole pro různé platformy.</span><span class="sxs-lookup"><span data-stu-id="363b1-204">[EF - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="363b1-205">Úvod do ASP.NET Core MVC v systému Mac nebo Linux</span><span class="sxs-lookup"><span data-stu-id="363b1-205">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="363b1-206">Úvod do ASP.NET Core MVC se sadou Visual Studio</span><span class="sxs-lookup"><span data-stu-id="363b1-206">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="363b1-207">Začínáme s technologiemi ASP.NET Core a Entity Framework Core pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="363b1-207">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
