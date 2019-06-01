---
title: Začínáme v ASP.NET Core – nová databáze – EF Core
author: rick-anderson
ms.author: riande
ms.date: 08/03/2018
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 2eb1668b8c077fabc9cb21088452fd1bead7ff22
ms.sourcegitcommit: ea1cdec0b982b922a59b9d9301d3ed2b94baca0f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452246"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a><span data-ttu-id="2f1d6-102">Začínáme s EF Core v ASP.NET Core s novou databázi</span><span class="sxs-lookup"><span data-stu-id="2f1d6-102">Getting Started with EF Core on ASP.NET Core with a New database</span></span>

<span data-ttu-id="2f1d6-103">V tomto kurzu sestavíte aplikaci ASP.NET Core MVC, která provádí základní přístup k datům pomocí Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="2f1d6-103">In this tutorial, you build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="2f1d6-104">Tento kurz používá k vytvoření databáze z modelu migrace.</span><span class="sxs-lookup"><span data-stu-id="2f1d6-104">The tutorial uses migrations to create the database from the data model.</span></span>

<span data-ttu-id="2f1d6-105">Tento kurz provedením pomocí sady Visual Studio 2017 na Windows nebo pomocí rozhraní příkazového řádku .NET Core ve Windows, macOS nebo Linuxu.</span><span class="sxs-lookup"><span data-stu-id="2f1d6-105">You can follow the tutorial by using Visual Studio 2017 on Windows, or by using the .NET Core CLI on Windows, macOS, or Linux.</span></span>

<span data-ttu-id="2f1d6-106">Zobrazte ukázky v tomto článku na Githubu:</span><span class="sxs-lookup"><span data-stu-id="2f1d6-106">View this article's sample on GitHub:</span></span>
* [<span data-ttu-id="2f1d6-107">Visual Studio 2017 with SQL Server</span><span class="sxs-lookup"><span data-stu-id="2f1d6-107">Visual Studio 2017 with SQL Server</span></span>](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb)
* <span data-ttu-id="2f1d6-108">[.NET core CLI s SQLite](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite).</span><span class="sxs-lookup"><span data-stu-id="2f1d6-108">[.NET Core CLI with SQLite](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2f1d6-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2f1d6-109">Prerequisites</span></span>

<span data-ttu-id="2f1d6-110">Nainstalujte následující software:</span><span class="sxs-lookup"><span data-stu-id="2f1d6-110">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2f1d6-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2f1d6-111">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2f1d6-112">[Visual Studio 2017 verze 15.7 nebo novější](https://www.visualstudio.com/downloads/) se tyto úlohy:</span><span class="sxs-lookup"><span data-stu-id="2f1d6-112">[Visual Studio 2017 version 15.7 or later](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="2f1d6-113">**Vývoj pro ASP.NET a web** (v části **Web a Cloud**)</span><span class="sxs-lookup"><span data-stu-id="2f1d6-113">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="2f1d6-114">**Vývoj pro různé platformy .NET core** (v části **další sady nástrojů**)</span><span class="sxs-lookup"><span data-stu-id="2f1d6-114">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="2f1d6-115">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="2f1d6-115">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2f1d6-116">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="2f1d6-116">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="2f1d6-117">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="2f1d6-117">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

---

## <a name="create-a-new-project"></a><span data-ttu-id="2f1d6-118">Vytvoření nového projektu</span><span class="sxs-lookup"><span data-stu-id="2f1d6-118">Create a new project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2f1d6-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2f1d6-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2f1d6-120">Open Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="2f1d6-120">Open Visual Studio 2017</span></span>
* <span data-ttu-id="2f1d6-121">**Soubor > Nový > Projekt**</span><span class="sxs-lookup"><span data-stu-id="2f1d6-121">**File > New > Project**</span></span>
* <span data-ttu-id="2f1d6-122">V nabídce vlevo vyberte **nainstalováno > Visual C# > .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="2f1d6-122">From the left menu, select **Installed > Visual C# > .NET Core**.</span></span>
* <span data-ttu-id="2f1d6-123">Vyberte **webová aplikace ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="2f1d6-123">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="2f1d6-124">Zadejte **EFGetStarted.AspNetCore.NewDb** název a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="2f1d6-124">Enter **EFGetStarted.AspNetCore.NewDb** for the name and click **OK**.</span></span>
* <span data-ttu-id="2f1d6-125">V **nová webová aplikace ASP.NET Core** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="2f1d6-125">In the **New ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="2f1d6-126">Ujistěte se, že **.NET Core** a **ASP.NET Core 2.1** jsou vybrány v rozevíracích seznamech</span><span class="sxs-lookup"><span data-stu-id="2f1d6-126">Make sure that **.NET Core** and **ASP.NET Core 2.1** are selected in the drop-down lists</span></span>
  * <span data-ttu-id="2f1d6-127">Vyberte **webové aplikace (Model-View-Controller)** šablony projektu</span><span class="sxs-lookup"><span data-stu-id="2f1d6-127">Select the **Web Application (Model-View-Controller)** project template</span></span>
  * <span data-ttu-id="2f1d6-128">Ujistěte se, že **ověřování** je nastavena na **bez ověřování**</span><span class="sxs-lookup"><span data-stu-id="2f1d6-128">Make sure that **Authentication** is set to **No Authentication**</span></span>
  * <span data-ttu-id="2f1d6-129">Klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="2f1d6-129">Click **OK**</span></span>

<span data-ttu-id="2f1d6-130">Upozornění: Pokud používáte **jednotlivé uživatelské účty** místo **žádný** pro **ověřování** pak model Entity Framework Core se přidá do projektu `Models\IdentityModel.cs`.</span><span class="sxs-lookup"><span data-stu-id="2f1d6-130">Warning: If you use **Individual User Accounts** instead of **None** for **Authentication** then an Entity Framework Core model will be added to the project in `Models\IdentityModel.cs`.</span></span> <span data-ttu-id="2f1d6-131">Pomocí technik, které se naučíte v tomto kurzu, můžete přidat druhý modelu nebo rozšiřte tento existující model tak, aby obsahovala tříd entit.</span><span class="sxs-lookup"><span data-stu-id="2f1d6-131">Using the techniques you learn in this tutorial, you can choose to add a second model, or extend this existing model to contain your entity classes.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2f1d6-132">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="2f1d6-132">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="2f1d6-133">Spusťte následující příkaz pro vytvoření projektu MVC:</span><span class="sxs-lookup"><span data-stu-id="2f1d6-133">Run the following command to create an MVC project:</span></span>

   ```cli
   dotnet new mvc -n EFGetStarted.AspNetCore.NewDb
   ```
* <span data-ttu-id="2f1d6-134">Přejděte do adresáře projektu.</span><span class="sxs-lookup"><span data-stu-id="2f1d6-134">Change to the project directory.</span></span> <span data-ttu-id="2f1d6-135">Další příkazy, které zadáte nutné ke spuštění nového projektu.</span><span class="sxs-lookup"><span data-stu-id="2f1d6-135">The next commands you enter need to run against the new project.</span></span>

   ```cli
   cd EFGetStarted.AspNetCore.NewDb
   ```
---

## <a name="install-entity-framework-core"></a><span data-ttu-id="2f1d6-136">Nainstalujte Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="2f1d6-136">Install Entity Framework Core</span></span>

<span data-ttu-id="2f1d6-137">Instalace EF Core, nainstalujte balíček vytvořeno EF Core databáze, kterou chcete cílit na pro.</span><span class="sxs-lookup"><span data-stu-id="2f1d6-137">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="2f1d6-138">Seznam dostupných zprostředkovatelů najdete v tématu [poskytovatelé databází](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="2f1d6-138">For a list of available providers, see [Database Providers](../../providers/index.md).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2f1d6-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2f1d6-139">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="2f1d6-140">Pro účely tohoto kurzu není nutné instalovat balíček poskytovatele, protože v tomto kurzu použijete SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2f1d6-140">For this tutorial, you don't have to install a provider package because the tutorial uses SQL Server.</span></span> <span data-ttu-id="2f1d6-141">Je součástí balíčku zprostředkovatele SQL Server [Microsoft.AspnetCore.App Microsoft.aspnetcore.all](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span><span class="sxs-lookup"><span data-stu-id="2f1d6-141">The SQL Server provider package is included in the [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2f1d6-142">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="2f1d6-142">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="2f1d6-143">Tento kurz používá SQLite, protože běží na všech platformách, které podporuje .NET Core.</span><span class="sxs-lookup"><span data-stu-id="2f1d6-143">This tutorial uses SQLite because it runs on all platforms that .NET Core supports.</span></span>

* <span data-ttu-id="2f1d6-144">Spusťte následující příkaz k instalaci zprostředkovatele SQLite:</span><span class="sxs-lookup"><span data-stu-id="2f1d6-144">Run the following command to install the SQLite provider:</span></span>

   ```cli
   dotnet add package Microsoft.EntityFrameworkCore.Sqlite
   ```

---

## <a name="create-the-model"></a><span data-ttu-id="2f1d6-145">Vytvoření modelu</span><span class="sxs-lookup"><span data-stu-id="2f1d6-145">Create the model</span></span>

<span data-ttu-id="2f1d6-146">Definování třídy kontextu a tříd entit, které tvoří modelu.</span><span class="sxs-lookup"><span data-stu-id="2f1d6-146">Define a context class and entity classes that make up the model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2f1d6-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2f1d6-147">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2f1d6-148">Klikněte pravým tlačítkem na **modely** a pak zvolte položku **Přidat > třída**.</span><span class="sxs-lookup"><span data-stu-id="2f1d6-148">Right-click on the **Models** folder and select **Add > Class**.</span></span>
* <span data-ttu-id="2f1d6-149">Zadejte **Model.cs** jako název a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="2f1d6-149">Enter **Model.cs** as the name and click **OK**.</span></span>
* <span data-ttu-id="2f1d6-150">Obsah souboru nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="2f1d6-150">Replace the contents of the file with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2f1d6-151">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="2f1d6-151">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="2f1d6-152">V **modely** vytvořit složku **Model.cs** následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="2f1d6-152">In the **Models** folder create **Model.cs** with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Models/Model.cs)]

---

<span data-ttu-id="2f1d6-153">Produkční aplikace obvykle vložili každá třída v samostatném souboru.</span><span class="sxs-lookup"><span data-stu-id="2f1d6-153">A production app would typically put each class in a separate file.</span></span> <span data-ttu-id="2f1d6-154">V tomto kurzu z důvodu zjednodušení, vloží tyto třídy v jednom souboru.</span><span class="sxs-lookup"><span data-stu-id="2f1d6-154">For the sake of simplicity, this tutorial puts these classes in one file.</span></span>

## <a name="register-the-context-with-dependency-injection"></a><span data-ttu-id="2f1d6-155">Zaregistrovat kontext pomocí vkládání závislostí</span><span class="sxs-lookup"><span data-stu-id="2f1d6-155">Register the context with dependency injection</span></span>

<span data-ttu-id="2f1d6-156">Chcete-li `BloggingContext` k dispozici pro kontrolery MVC registrovat jako službu v `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="2f1d6-156">To make `BloggingContext` available to MVC controllers, register it as a service in `Startup.cs`.</span></span>

<span data-ttu-id="2f1d6-157">Služby (například `BloggingContext`) jsou registrovány [injektáž závislostí](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) během spuštění aplikace tak, aby se je možné poskytnout automaticky součásti, které využívají služeb (jako jsou řadiče MVC) prostřednictvím konstruktoru parametry a vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2f1d6-157">Services (such as `BloggingContext`) are registered with [dependency injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) during application startup so that they can be provided automatically to components that consume services (such as MVC controllers) via constructor parameters and properties.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2f1d6-158">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2f1d6-158">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2f1d6-159">V **Startup.cs** přidejte následující `using` příkazy:</span><span class="sxs-lookup"><span data-stu-id="2f1d6-159">In **Startup.cs** add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

* <span data-ttu-id="2f1d6-160">Přidejte následující zvýrazněný kód do `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="2f1d6-160">Add the following highlighted code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=12-14)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2f1d6-161">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="2f1d6-161">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="2f1d6-162">V **Startup.cs** přidejte následující `using` příkazy:</span><span class="sxs-lookup"><span data-stu-id="2f1d6-162">In **Startup.cs** add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs#AddedUsings)]

* <span data-ttu-id="2f1d6-163">Přidejte následující zvýrazněný kód do `ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="2f1d6-163">Add the following highlighted code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs?name=ConfigureServices&highlight=12-14)]

---

<span data-ttu-id="2f1d6-164">Produkční aplikace obvykle vložili připojovací řetězec v konfigurační soubor nebo prostředí proměnnou.</span><span class="sxs-lookup"><span data-stu-id="2f1d6-164">A production app would typically put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="2f1d6-165">Z důvodu zjednodušení v tomto kurzu ji definuje v kódu.</span><span class="sxs-lookup"><span data-stu-id="2f1d6-165">For the sake of simplicity, this tutorial defines it in code.</span></span> <span data-ttu-id="2f1d6-166">Zobrazit [připojovací řetězce](../../miscellaneous/connection-strings.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="2f1d6-166">See [Connection Strings](../../miscellaneous/connection-strings.md) for more information.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="2f1d6-167">Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="2f1d6-167">Create the database</span></span>

<span data-ttu-id="2f1d6-168">Následující kroky použijte [migrace](xref:core/managing-schemas/migrations/index) k vytvoření databáze.</span><span class="sxs-lookup"><span data-stu-id="2f1d6-168">The following steps use [migrations](xref:core/managing-schemas/migrations/index) to create a database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2f1d6-169">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2f1d6-169">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2f1d6-170">**Nástroje > Správce balíčků NuGet > Konzola správce balíčků**</span><span class="sxs-lookup"><span data-stu-id="2f1d6-170">**Tools > NuGet Package Manager > Package Manager Console**</span></span>
* <span data-ttu-id="2f1d6-171">Spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="2f1d6-171">Run the following commands:</span></span>

  ```powershell
  Add-Migration InitialCreate
  Update-Database
  ```

  <span data-ttu-id="2f1d6-172">Pokud se zobrazí chyba `The term 'add-migration' is not recognized as the name of a cmdlet`, zavřete a znovu otevřete Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2f1d6-172">If you get an error stating `The term 'add-migration' is not recognized as the name of a cmdlet`, close and reopen Visual Studio.</span></span>

  <span data-ttu-id="2f1d6-173">`Add-Migration` Příkaz vygeneruje uživatelské rozhraní migrace vytvořte počáteční sadu tabulek pro model.</span><span class="sxs-lookup"><span data-stu-id="2f1d6-173">The `Add-Migration` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="2f1d6-174">`Update-Database` Příkaz vytvoří databázi a použije novou migraci na něj.</span><span class="sxs-lookup"><span data-stu-id="2f1d6-174">The `Update-Database` command creates the database and applies the new migration to it.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2f1d6-175">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="2f1d6-175">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="2f1d6-176">Spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="2f1d6-176">Run the following commands:</span></span>

  ```cli
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  <span data-ttu-id="2f1d6-177">`migrations` Příkaz vygeneruje uživatelské rozhraní migrace vytvořte počáteční sadu tabulek pro model.</span><span class="sxs-lookup"><span data-stu-id="2f1d6-177">The `migrations` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="2f1d6-178">`database update` Příkaz vytvoří databázi a použije novou migraci na něj.</span><span class="sxs-lookup"><span data-stu-id="2f1d6-178">The `database update` command creates the database and applies the new migration to it.</span></span>

---

## <a name="create-a-controller"></a><span data-ttu-id="2f1d6-179">Vytvoření kontroleru</span><span class="sxs-lookup"><span data-stu-id="2f1d6-179">Create a controller</span></span>

<span data-ttu-id="2f1d6-180">Kontroler a zobrazení pro generování uživatelského rozhraní `Blog` entity.</span><span class="sxs-lookup"><span data-stu-id="2f1d6-180">Scaffold a controller and views for the `Blog` entity.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2f1d6-181">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2f1d6-181">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2f1d6-182">Klikněte pravým tlačítkem na **řadiče** složky **Průzkumníku řešení** a vyberte **Přidat > kontroler**.</span><span class="sxs-lookup"><span data-stu-id="2f1d6-182">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="2f1d6-183">Vyberte **kontroler MVC se zobrazeními, s použitím Entity Framework** a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="2f1d6-183">Select **MVC Controller with views, using Entity Framework** and click **Add**.</span></span>
* <span data-ttu-id="2f1d6-184">Nastavte **třída modelu** k **blogu** a **třída kontextu dat** k **BloggingContext**.</span><span class="sxs-lookup"><span data-stu-id="2f1d6-184">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**.</span></span>
* <span data-ttu-id="2f1d6-185">Klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="2f1d6-185">Click **Add**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2f1d6-186">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="2f1d6-186">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="2f1d6-187">Spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="2f1d6-187">Run the following commands:</span></span>

  ```cli
  dotnet tool install -g dotnet-aspnet-codegenerator
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
  dotnet restore
  dotnet aspnet-codegenerator controller -name BlogsController -m Blog -dc BloggingContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  <span data-ttu-id="2f1d6-188">`tool install` a `add package` příkazy instalace nástrojů, můžete vygenerovat kontrolerů a zobrazení.</span><span class="sxs-lookup"><span data-stu-id="2f1d6-188">The `tool install` and `add package` commands install the tooling that can scaffold controllers and views.</span></span> <span data-ttu-id="2f1d6-189">`restore` Příkaz zajistí, že se stáhnou všechny balíčky v projektu a `aspnet-codegenerator` příkaz dělá základní kostry aplikace.</span><span class="sxs-lookup"><span data-stu-id="2f1d6-189">The `restore` command ensures that all of the project's packages are downloaded, and the `aspnet-codegenerator` command does the scaffolding.</span></span>
---

<span data-ttu-id="2f1d6-190">Modul generování uživatelského rozhraní vytvoří následující soubory:</span><span class="sxs-lookup"><span data-stu-id="2f1d6-190">The scaffolding engine creates the following files:</span></span>

* <span data-ttu-id="2f1d6-191">Kontroler (*Controllers/BlogsController.cs*)</span><span class="sxs-lookup"><span data-stu-id="2f1d6-191">A controller (*Controllers/BlogsController.cs*)</span></span>
* <span data-ttu-id="2f1d6-192">Zobrazení syntaxe Razor pro vytvořit, odstranit, podrobností, úpravy a Index stránky (_Views/Blogs/\*.cshtml_)</span><span class="sxs-lookup"><span data-stu-id="2f1d6-192">Razor views for Create, Delete, Details, Edit, and Index pages (_Views/Blogs/\*.cshtml_)</span></span>

## <a name="run-the-application"></a><span data-ttu-id="2f1d6-193">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="2f1d6-193">Run the application</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2f1d6-194">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2f1d6-194">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2f1d6-195">**Ladění** > **spustit bez ladění**</span><span class="sxs-lookup"><span data-stu-id="2f1d6-195">**Debug** > **Start Without Debugging**</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2f1d6-196">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="2f1d6-196">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet run
```
---

* <span data-ttu-id="2f1d6-197">Přejděte na `/Blogs`</span><span class="sxs-lookup"><span data-stu-id="2f1d6-197">Navigate to `/Blogs`</span></span>

* <span data-ttu-id="2f1d6-198">Použití **vytvořit nový** odkaz můžete vytvořit některé položky blogu.</span><span class="sxs-lookup"><span data-stu-id="2f1d6-198">Use the **Create New** link to create some blog entries.</span></span>

  ![Vytvoření stránky](_static/create.png)

* <span data-ttu-id="2f1d6-200">Test **podrobnosti**, **upravit**, a **odstranit** odkazy.</span><span class="sxs-lookup"><span data-stu-id="2f1d6-200">Test the **Details**, **Edit**, and **Delete** links.</span></span>

  ![Indexová stránka](_static/index-new-db.png)

## <a name="additional-tutorials"></a><span data-ttu-id="2f1d6-202">Další kurzy</span><span class="sxs-lookup"><span data-stu-id="2f1d6-202">Additional tutorials</span></span>

* [<span data-ttu-id="2f1d6-203">Začínáme s EF Core na .NET Core s novou databázi pomocí SQLite</span><span class="sxs-lookup"><span data-stu-id="2f1d6-203">Get started with EF Core on .NET Core with a new database using SQLite</span></span>](xref:core/get-started/netcore/new-db-sqlite)
* <span data-ttu-id="2f1d6-204">ASP.NET Core MVC:</span><span class="sxs-lookup"><span data-stu-id="2f1d6-204">ASP.NET Core MVC:</span></span>
  * [<span data-ttu-id="2f1d6-205">Začínáme s ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="2f1d6-205">Get started with ASP.NET Core MVC</span></span>](/aspnet/core/tutorials/first-mvc-app/start-mvc)
  * [<span data-ttu-id="2f1d6-206">Začínáme s EF Core ve webové aplikaci ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="2f1d6-206">Get started with EF Core in an ASP.NET MVC web app</span></span>](/aspnet/core/data/ef-mvc/intro)
* <span data-ttu-id="2f1d6-207">[Stránky Razor](/aspnet/core/razor-pages/index):</span><span class="sxs-lookup"><span data-stu-id="2f1d6-207">[Razor Pages](/aspnet/core/razor-pages/index):</span></span>
  * [<span data-ttu-id="2f1d6-208">Začínáme se stránkami Razor v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2f1d6-208">Get started with Razor Pages in ASP.NET Core</span></span>](/aspnet/core/tutorials/razor-pages/razor-pages-start)
  * [<span data-ttu-id="2f1d6-209">Stránky Razor pomocí Entity Framework Core v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2f1d6-209">Razor Pages with Entity Framework Core in ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro)
