---
title: Začínáme – EF Core
author: rick-anderson
ms.date: 09/17/2019
ms.assetid: 3c88427c-20c6-42ec-a736-22d3eccd5071
uid: core/get-started/index
ms.openlocfilehash: 0e7a1ee159cdf5b72448fe6d73c972975b1ab95b
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416883"
---
# <a name="getting-started-with-ef-core"></a><span data-ttu-id="e4f98-102">Začínáme s EF Core</span><span class="sxs-lookup"><span data-stu-id="e4f98-102">Getting Started with EF Core</span></span>

<span data-ttu-id="e4f98-103">V tomto kurzu vytvoříte konzolovou aplikaci .NET Core, která provádí přístup k datům v databázi SQLite pomocí Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="e4f98-103">In this tutorial, you create a .NET Core console app that performs data access against a SQLite database using Entity Framework Core.</span></span>

<span data-ttu-id="e4f98-104">Můžete postupovat podle kurzu pomocí sady Visual Studio ve Windows nebo pomocí .NET Core CLI ve Windows, macOS nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="e4f98-104">You can follow the tutorial by using Visual Studio on Windows, or by using the .NET Core CLI on Windows, macOS, or Linux.</span></span>

<span data-ttu-id="e4f98-105">[Podívejte se na ukázku tohoto článku na GitHubu](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/GetStarted).</span><span class="sxs-lookup"><span data-stu-id="e4f98-105">[View this article's sample on GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/GetStarted).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e4f98-106">Předpoklady</span><span class="sxs-lookup"><span data-stu-id="e4f98-106">Prerequisites</span></span>

<span data-ttu-id="e4f98-107">Nainstalujte následující software:</span><span class="sxs-lookup"><span data-stu-id="e4f98-107">Install the following software:</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="e4f98-108">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="e4f98-108">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="e4f98-109">[.NET Core SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="e4f98-109">[.NET Core SDK](https://www.microsoft.com/net/download/core).</span></span>

### <a name="visual-studio"></a>[<span data-ttu-id="e4f98-110">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4f98-110">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e4f98-111">[Visual Studio 2019 verze 16,3 nebo novější](https://www.visualstudio.com/downloads/) s touto úlohou:</span><span class="sxs-lookup"><span data-stu-id="e4f98-111">[Visual Studio 2019 version 16.3 or later](https://www.visualstudio.com/downloads/) with this  workload:</span></span>
  * <span data-ttu-id="e4f98-112">**Vývoj pro různé platformy .NET Core** (v **jiných sad nástrojů**)</span><span class="sxs-lookup"><span data-stu-id="e4f98-112">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

---

## <a name="create-a-new-project"></a><span data-ttu-id="e4f98-113">Vytvoření nového projektu</span><span class="sxs-lookup"><span data-stu-id="e4f98-113">Create a new project</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="e4f98-114">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="e4f98-114">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new console -o EFGetStarted
cd EFGetStarted
```

### <a name="visual-studio"></a>[<span data-ttu-id="e4f98-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4f98-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e4f98-116">Otevřete sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e4f98-116">Open Visual Studio</span></span>
* <span data-ttu-id="e4f98-117">Klikněte na **vytvořit nový projekt** .</span><span class="sxs-lookup"><span data-stu-id="e4f98-117">Click **Create a new project**</span></span>
* <span data-ttu-id="e4f98-118">Vyberte možnost **aplikace konzoly (.NET Core)** se **C#** značkou a klikněte na **Další** .</span><span class="sxs-lookup"><span data-stu-id="e4f98-118">Select **Console App (.NET Core)** with the **C#** tag and click **Next**</span></span>
* <span data-ttu-id="e4f98-119">Jako název zadejte **EFGetStarted** a klikněte na **vytvořit** .</span><span class="sxs-lookup"><span data-stu-id="e4f98-119">Enter **EFGetStarted** for the name and click **Create**</span></span>

---

## <a name="install-entity-framework-core"></a><span data-ttu-id="e4f98-120">Nainstalovat Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="e4f98-120">Install Entity Framework Core</span></span>

<span data-ttu-id="e4f98-121">Chcete-li nainstalovat EF Core, nainstalujte balíček pro zprostředkovatele EF Corech databází, které chcete cílit.</span><span class="sxs-lookup"><span data-stu-id="e4f98-121">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="e4f98-122">V tomto kurzu se používá SQLite, protože běží na všech platformách, které .NET Core podporuje.</span><span class="sxs-lookup"><span data-stu-id="e4f98-122">This tutorial uses SQLite because it runs on all platforms that .NET Core supports.</span></span> <span data-ttu-id="e4f98-123">Seznam dostupných zprostředkovatelů najdete v tématu [poskytovatelé databází](../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="e4f98-123">For a list of available providers, see [Database Providers](../providers/index.md).</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="e4f98-124">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="e4f98-124">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

### <a name="visual-studio"></a>[<span data-ttu-id="e4f98-125">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4f98-125">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e4f98-126">**Nástroje > Správce balíčků NuGet > konzole správce balíčků**</span><span class="sxs-lookup"><span data-stu-id="e4f98-126">**Tools > NuGet Package Manager > Package Manager Console**</span></span>
* <span data-ttu-id="e4f98-127">Spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="e4f98-127">Run the following commands:</span></span>

  ``` PowerShell
  Install-Package Microsoft.EntityFrameworkCore.Sqlite
  ```

<span data-ttu-id="e4f98-128">Tip: balíčky můžete nainstalovat také tak, že kliknete pravým tlačítkem na projekt a vyberete **Spravovat balíčky NuGet** .</span><span class="sxs-lookup"><span data-stu-id="e4f98-128">Tip: You can also install packages by right-clicking on the project and selecting **Manage NuGet Packages**</span></span>

---

## <a name="create-the-model"></a><span data-ttu-id="e4f98-129">Vytvoření modelu</span><span class="sxs-lookup"><span data-stu-id="e4f98-129">Create the model</span></span>

<span data-ttu-id="e4f98-130">Definujte třídu kontextu a třídy entit, které tvoří model.</span><span class="sxs-lookup"><span data-stu-id="e4f98-130">Define a context class and entity classes that make up the model.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="e4f98-131">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="e4f98-131">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="e4f98-132">V adresáři projektu vytvořte **model.cs** s následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="e4f98-132">In the project directory, create **Model.cs** with the following code</span></span>

### <a name="visual-studio"></a>[<span data-ttu-id="e4f98-133">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4f98-133">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e4f98-134">Klikněte pravým tlačítkem na projekt a vyberte **přidat > třídy** .</span><span class="sxs-lookup"><span data-stu-id="e4f98-134">Right-click on the project and select **Add > Class**</span></span>
* <span data-ttu-id="e4f98-135">Jako název zadejte **model.cs** a klikněte na **Přidat** .</span><span class="sxs-lookup"><span data-stu-id="e4f98-135">Enter **Model.cs** as the name and click **Add**</span></span>
* <span data-ttu-id="e4f98-136">Obsah souboru nahraďte následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="e4f98-136">Replace the contents of the file with the following code</span></span>

---

[!code-csharp[Main](../../../samples/core/GetStarted/Model.cs)]

<span data-ttu-id="e4f98-137">EF Core může také provádět [zpětnou analýzu](../managing-schemas/scaffolding.md) modelu z existující databáze.</span><span class="sxs-lookup"><span data-stu-id="e4f98-137">EF Core can also [reverse engineer](../managing-schemas/scaffolding.md) a model from an existing database.</span></span>

<span data-ttu-id="e4f98-138">Tip: v reálné aplikaci vložíte každou třídu do samostatného souboru a [připojovací řetězec](../miscellaneous/connection-strings.md) umístíte do konfiguračního souboru nebo proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="e4f98-138">Tip: In a real app, you put each class in a separate file and put the [connection string](../miscellaneous/connection-strings.md) in a configuration file or environment variable.</span></span> <span data-ttu-id="e4f98-139">V případě jednoduchého kurzu je vše obsaženo v jednom souboru.</span><span class="sxs-lookup"><span data-stu-id="e4f98-139">To keep the tutorial simple, everything is contained in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="e4f98-140">Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="e4f98-140">Create the database</span></span>

<span data-ttu-id="e4f98-141">V následujících krocích se k vytvoření databáze používají [migrace](xref:core/managing-schemas/migrations/index) .</span><span class="sxs-lookup"><span data-stu-id="e4f98-141">The following steps use [migrations](xref:core/managing-schemas/migrations/index) to create a database.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="e4f98-142">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="e4f98-142">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="e4f98-143">Spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="e4f98-143">Run the following commands:</span></span>

  ```dotnetcli
  dotnet tool install --global dotnet-ef
  dotnet add package Microsoft.EntityFrameworkCore.Design
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  <span data-ttu-id="e4f98-144">Tím se nainstaluje [dotnet EF](../miscellaneous/cli/dotnet.md) a návrhový balíček, který je nutný ke spuštění příkazu na projektu.</span><span class="sxs-lookup"><span data-stu-id="e4f98-144">This installs [dotnet ef](../miscellaneous/cli/dotnet.md) and the design package which is required to run the command on a project.</span></span> <span data-ttu-id="e4f98-145">Příkaz `migrations` vygeneruje migraci k vytvoření počáteční sady tabulek pro model.</span><span class="sxs-lookup"><span data-stu-id="e4f98-145">The `migrations` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="e4f98-146">Příkaz `database update` vytvoří databázi a použije novou migraci na ni.</span><span class="sxs-lookup"><span data-stu-id="e4f98-146">The `database update` command creates the database and applies the new migration to it.</span></span>

### <a name="visual-studio"></a>[<span data-ttu-id="e4f98-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4f98-147">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e4f98-148">V **konzole správce balíčků** spusťte následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="e4f98-148">Run the following commands in **Package Manager Console**</span></span>

  ``` PowerShell
  Install-Package Microsoft.EntityFrameworkCore.Tools
  Add-Migration InitialCreate
  Update-Database
  ```

  <span data-ttu-id="e4f98-149">Tím se nainstaluje [nástroje PMC pro EF Core](../miscellaneous/cli/powershell.md).</span><span class="sxs-lookup"><span data-stu-id="e4f98-149">This installs the [PMC tools for EF Core](../miscellaneous/cli/powershell.md).</span></span> <span data-ttu-id="e4f98-150">Příkaz `Add-Migration` vygeneruje migraci k vytvoření počáteční sady tabulek pro model.</span><span class="sxs-lookup"><span data-stu-id="e4f98-150">The `Add-Migration` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="e4f98-151">Příkaz `Update-Database` vytvoří databázi a použije novou migraci na ni.</span><span class="sxs-lookup"><span data-stu-id="e4f98-151">The `Update-Database` command creates the database and applies the new migration to it.</span></span>

---

## <a name="create-read-update--delete"></a><span data-ttu-id="e4f98-152">Vytvořit, číst, aktualizovat & Odstranit</span><span class="sxs-lookup"><span data-stu-id="e4f98-152">Create, read, update & delete</span></span>

* <span data-ttu-id="e4f98-153">Otevřete *program.cs* a nahraďte obsah následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="e4f98-153">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../samples/core/GetStarted/Program.cs)]

## <a name="run-the-app"></a><span data-ttu-id="e4f98-154">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="e4f98-154">Run the app</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="e4f98-155">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="e4f98-155">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet run
```

### <a name="visual-studio"></a>[<span data-ttu-id="e4f98-156">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4f98-156">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e4f98-157">Visual Studio používá nekonzistentní pracovní adresář při spuštění konzolových aplikací .NET Core.</span><span class="sxs-lookup"><span data-stu-id="e4f98-157">Visual Studio uses an inconsistent working directory when running .NET Core console apps.</span></span> <span data-ttu-id="e4f98-158">(viz [dotnet/Project-System # 3619](https://github.com/dotnet/project-system/issues/3619)) Výsledkem je výjimka, která je vyvolána: *žádná taková tabulka: Blogy*.</span><span class="sxs-lookup"><span data-stu-id="e4f98-158">(see [dotnet/project-system#3619](https://github.com/dotnet/project-system/issues/3619)) This results in an exception being thrown: *no such table: Blogs*.</span></span> <span data-ttu-id="e4f98-159">Aktualizace pracovního adresáře:</span><span class="sxs-lookup"><span data-stu-id="e4f98-159">To update the working directory:</span></span>

* <span data-ttu-id="e4f98-160">Klikněte pravým tlačítkem na projekt a vyberte **Upravit soubor projektu** .</span><span class="sxs-lookup"><span data-stu-id="e4f98-160">Right-click on the project and select **Edit Project File**</span></span>
* <span data-ttu-id="e4f98-161">Hned pod vlastností *targetFramework* přidejte následující:</span><span class="sxs-lookup"><span data-stu-id="e4f98-161">Just below the *TargetFramework* property, add the following:</span></span>

  ``` XML
  <StartWorkingDirectory>$(MSBuildProjectDirectory)</StartWorkingDirectory>
  ```

* <span data-ttu-id="e4f98-162">Soubor uložte</span><span class="sxs-lookup"><span data-stu-id="e4f98-162">Save the file</span></span>

<span data-ttu-id="e4f98-163">Teď můžete aplikaci spustit:</span><span class="sxs-lookup"><span data-stu-id="e4f98-163">Now you can run the app:</span></span>

* <span data-ttu-id="e4f98-164">**Ladit > Spustit bez ladění**</span><span class="sxs-lookup"><span data-stu-id="e4f98-164">**Debug > Start Without Debugging**</span></span>

---

## <a name="next-steps"></a><span data-ttu-id="e4f98-165">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e4f98-165">Next steps</span></span>

* <span data-ttu-id="e4f98-166">Pokud chcete používat EF Core ve webové aplikaci, postupujte podle [kurzu ASP.NET Core](/aspnet/core/data/ef-rp/intro) .</span><span class="sxs-lookup"><span data-stu-id="e4f98-166">Follow the [ASP.NET Core Tutorial](/aspnet/core/data/ef-rp/intro) to use EF Core in a web app</span></span>
* <span data-ttu-id="e4f98-167">Další informace o [výrazech dotazů LINQ](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)</span><span class="sxs-lookup"><span data-stu-id="e4f98-167">Learn more about [LINQ query expressions](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)</span></span>
* <span data-ttu-id="e4f98-168">[Nakonfigurujte svůj model](xref:core/modeling/index) tak, aby určoval například [požadované](xref:core/modeling/entity-properties#required-and-optional-properties) a [maximální délku](xref:core/modeling/entity-properties#maximum-length) .</span><span class="sxs-lookup"><span data-stu-id="e4f98-168">[Configure your model](xref:core/modeling/index) to specify things like [required](xref:core/modeling/entity-properties#required-and-optional-properties) and [maximum length](xref:core/modeling/entity-properties#maximum-length)</span></span>
* <span data-ttu-id="e4f98-169">Aktualizace schématu databáze po změně modelu pomocí [migrace](xref:core/managing-schemas/migrations/index)</span><span class="sxs-lookup"><span data-stu-id="e4f98-169">Use [Migrations](xref:core/managing-schemas/migrations/index) to update the database schema after changing your model</span></span>
