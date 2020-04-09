---
title: Začínáme – EF Core
author: rick-anderson
ms.date: 09/17/2019
ms.assetid: 3c88427c-20c6-42ec-a736-22d3eccd5071
uid: core/get-started/index
ms.openlocfilehash: 0e7a1ee159cdf5b72448fe6d73c972975b1ab95b
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416883"
---
# <a name="getting-started-with-ef-core"></a><span data-ttu-id="a63e9-102">Začínáme s EF Core</span><span class="sxs-lookup"><span data-stu-id="a63e9-102">Getting Started with EF Core</span></span>

<span data-ttu-id="a63e9-103">V tomto kurzu vytvoříte konzolovou aplikaci .NET Core, která provádí přístup k datům proti databázi SQLite pomocí jádra entity frameworku.</span><span class="sxs-lookup"><span data-stu-id="a63e9-103">In this tutorial, you create a .NET Core console app that performs data access against a SQLite database using Entity Framework Core.</span></span>

<span data-ttu-id="a63e9-104">Kurz můžete sledovat pomocí Visual Studia ve Windows nebo pomocí rozhraní .NET Core CLI ve Windows, macOS nebo Linuxu.</span><span class="sxs-lookup"><span data-stu-id="a63e9-104">You can follow the tutorial by using Visual Studio on Windows, or by using the .NET Core CLI on Windows, macOS, or Linux.</span></span>

<span data-ttu-id="a63e9-105">[Zobrazit ukázku tohoto článku na GitHubu](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/GetStarted).</span><span class="sxs-lookup"><span data-stu-id="a63e9-105">[View this article's sample on GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/GetStarted).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a63e9-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a63e9-106">Prerequisites</span></span>

<span data-ttu-id="a63e9-107">Nainstalujte následující software:</span><span class="sxs-lookup"><span data-stu-id="a63e9-107">Install the following software:</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="a63e9-108">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="a63e9-108">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="a63e9-109">[Sada SDK jádra .NET](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="a63e9-109">[.NET Core SDK](https://www.microsoft.com/net/download/core).</span></span>

### <a name="visual-studio"></a>[<span data-ttu-id="a63e9-110">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a63e9-110">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a63e9-111">[Visual Studio 2019 verze 16.3 nebo novější](https://www.visualstudio.com/downloads/) s tímto zatížením:</span><span class="sxs-lookup"><span data-stu-id="a63e9-111">[Visual Studio 2019 version 16.3 or later](https://www.visualstudio.com/downloads/) with this  workload:</span></span>
  * <span data-ttu-id="a63e9-112">**Vývoj napříč platformami .NET Core** (v části **Jiné sady nástrojů)**</span><span class="sxs-lookup"><span data-stu-id="a63e9-112">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

---

## <a name="create-a-new-project"></a><span data-ttu-id="a63e9-113">Vytvoření nového projektu</span><span class="sxs-lookup"><span data-stu-id="a63e9-113">Create a new project</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="a63e9-114">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="a63e9-114">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new console -o EFGetStarted
cd EFGetStarted
```

### <a name="visual-studio"></a>[<span data-ttu-id="a63e9-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a63e9-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a63e9-116">Otevřete sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a63e9-116">Open Visual Studio</span></span>
* <span data-ttu-id="a63e9-117">Klikněte na **Vytvořit nový projekt.**</span><span class="sxs-lookup"><span data-stu-id="a63e9-117">Click **Create a new project**</span></span>
* <span data-ttu-id="a63e9-118">Vyberte **Konzolovou aplikaci (.NET Core)** pomocí značky **C#** a klikněte na **Další.**</span><span class="sxs-lookup"><span data-stu-id="a63e9-118">Select **Console App (.NET Core)** with the **C#** tag and click **Next**</span></span>
* <span data-ttu-id="a63e9-119">Zadejte **EFGetStarted** pro název a klepněte na **vytvořit**</span><span class="sxs-lookup"><span data-stu-id="a63e9-119">Enter **EFGetStarted** for the name and click **Create**</span></span>

---

## <a name="install-entity-framework-core"></a><span data-ttu-id="a63e9-120">Instalace jádra rozhraní entity</span><span class="sxs-lookup"><span data-stu-id="a63e9-120">Install Entity Framework Core</span></span>

<span data-ttu-id="a63e9-121">Chcete-li nainstalovat EF Core, nainstalujte balíček pro zprostředkovatele databáze EF Core, na které chcete cílit.</span><span class="sxs-lookup"><span data-stu-id="a63e9-121">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="a63e9-122">Tento kurz používá SQLite, protože běží na všech platformách, které podporuje .NET Core.</span><span class="sxs-lookup"><span data-stu-id="a63e9-122">This tutorial uses SQLite because it runs on all platforms that .NET Core supports.</span></span> <span data-ttu-id="a63e9-123">Seznam dostupných zprostředkovatelů naleznete v [tématu Database Providers](../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="a63e9-123">For a list of available providers, see [Database Providers](../providers/index.md).</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="a63e9-124">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="a63e9-124">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

### <a name="visual-studio"></a>[<span data-ttu-id="a63e9-125">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a63e9-125">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a63e9-126">**Nástroje >, že správce balíčků NuGet > konzoli správce balíčků**</span><span class="sxs-lookup"><span data-stu-id="a63e9-126">**Tools > NuGet Package Manager > Package Manager Console**</span></span>
* <span data-ttu-id="a63e9-127">Spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="a63e9-127">Run the following commands:</span></span>

  ``` PowerShell
  Install-Package Microsoft.EntityFrameworkCore.Sqlite
  ```

<span data-ttu-id="a63e9-128">Tip: Balíčky můžete nainstalovat také kliknutím pravým tlačítkem myši na projekt a výběrem **možnosti Spravovat balíčky NuGet**</span><span class="sxs-lookup"><span data-stu-id="a63e9-128">Tip: You can also install packages by right-clicking on the project and selecting **Manage NuGet Packages**</span></span>

---

## <a name="create-the-model"></a><span data-ttu-id="a63e9-129">Vytvoření modelu</span><span class="sxs-lookup"><span data-stu-id="a63e9-129">Create the model</span></span>

<span data-ttu-id="a63e9-130">Definujte třídu kontextu a třídy entit, které tvoří model.</span><span class="sxs-lookup"><span data-stu-id="a63e9-130">Define a context class and entity classes that make up the model.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="a63e9-131">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="a63e9-131">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="a63e9-132">V adresáři projektu vytvořte **Model.cs** s následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="a63e9-132">In the project directory, create **Model.cs** with the following code</span></span>

### <a name="visual-studio"></a>[<span data-ttu-id="a63e9-133">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a63e9-133">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a63e9-134">Klikněte pravým tlačítkem myši na projekt a vyberte **přidat > třídu.**</span><span class="sxs-lookup"><span data-stu-id="a63e9-134">Right-click on the project and select **Add > Class**</span></span>
* <span data-ttu-id="a63e9-135">Zadejte **Model.cs** jako název a klikněte na **Přidat.**</span><span class="sxs-lookup"><span data-stu-id="a63e9-135">Enter **Model.cs** as the name and click **Add**</span></span>
* <span data-ttu-id="a63e9-136">Nahrazení obsahu souboru následujícím kódem</span><span class="sxs-lookup"><span data-stu-id="a63e9-136">Replace the contents of the file with the following code</span></span>

---

[!code-csharp[Main](../../../samples/core/GetStarted/Model.cs)]

<span data-ttu-id="a63e9-137">EF Core můžete také [zpětnou analýzu](../managing-schemas/scaffolding.md) modelu z existující databáze.</span><span class="sxs-lookup"><span data-stu-id="a63e9-137">EF Core can also [reverse engineer](../managing-schemas/scaffolding.md) a model from an existing database.</span></span>

<span data-ttu-id="a63e9-138">Tip: V reálné aplikaci vložíte každou třídu do samostatného souboru a [vložíte připojovací řetězec](../miscellaneous/connection-strings.md) do konfiguračního souboru nebo proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="a63e9-138">Tip: In a real app, you put each class in a separate file and put the [connection string](../miscellaneous/connection-strings.md) in a configuration file or environment variable.</span></span> <span data-ttu-id="a63e9-139">Chcete-li, aby byl výukový program jednoduchý, je vše obsaženo v jednom souboru.</span><span class="sxs-lookup"><span data-stu-id="a63e9-139">To keep the tutorial simple, everything is contained in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="a63e9-140">Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="a63e9-140">Create the database</span></span>

<span data-ttu-id="a63e9-141">Následující kroky používají [migrace](xref:core/managing-schemas/migrations/index) k vytvoření databáze.</span><span class="sxs-lookup"><span data-stu-id="a63e9-141">The following steps use [migrations](xref:core/managing-schemas/migrations/index) to create a database.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="a63e9-142">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="a63e9-142">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="a63e9-143">Spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="a63e9-143">Run the following commands:</span></span>

  ```dotnetcli
  dotnet tool install --global dotnet-ef
  dotnet add package Microsoft.EntityFrameworkCore.Design
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  <span data-ttu-id="a63e9-144">Tím [nainstalujete dotnet ef](../miscellaneous/cli/dotnet.md) a návrhový balíček, který je nutný ke spuštění příkazu v projektu.</span><span class="sxs-lookup"><span data-stu-id="a63e9-144">This installs [dotnet ef](../miscellaneous/cli/dotnet.md) and the design package which is required to run the command on a project.</span></span> <span data-ttu-id="a63e9-145">Příkaz `migrations` přisycování kašovina migrace vytvořit počáteční sadu tabulek pro model.</span><span class="sxs-lookup"><span data-stu-id="a63e9-145">The `migrations` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="a63e9-146">Příkaz `database update` vytvoří databázi a použije na něj novou migraci.</span><span class="sxs-lookup"><span data-stu-id="a63e9-146">The `database update` command creates the database and applies the new migration to it.</span></span>

### <a name="visual-studio"></a>[<span data-ttu-id="a63e9-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a63e9-147">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a63e9-148">Spuštění následujících příkazů v **konzole Správce balíčků**</span><span class="sxs-lookup"><span data-stu-id="a63e9-148">Run the following commands in **Package Manager Console**</span></span>

  ``` PowerShell
  Install-Package Microsoft.EntityFrameworkCore.Tools
  Add-Migration InitialCreate
  Update-Database
  ```

  <span data-ttu-id="a63e9-149">Tím se nainstaluje [nástroje PMC pro EF Core](../miscellaneous/cli/powershell.md).</span><span class="sxs-lookup"><span data-stu-id="a63e9-149">This installs the [PMC tools for EF Core](../miscellaneous/cli/powershell.md).</span></span> <span data-ttu-id="a63e9-150">Příkaz `Add-Migration` přisycování kašovina migrace vytvořit počáteční sadu tabulek pro model.</span><span class="sxs-lookup"><span data-stu-id="a63e9-150">The `Add-Migration` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="a63e9-151">Příkaz `Update-Database` vytvoří databázi a použije na něj novou migraci.</span><span class="sxs-lookup"><span data-stu-id="a63e9-151">The `Update-Database` command creates the database and applies the new migration to it.</span></span>

---

## <a name="create-read-update--delete"></a><span data-ttu-id="a63e9-152">Vytvořit, číst, aktualizovat & odstranit</span><span class="sxs-lookup"><span data-stu-id="a63e9-152">Create, read, update & delete</span></span>

* <span data-ttu-id="a63e9-153">Otevřete *Program.cs* a nahraďte obsah následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="a63e9-153">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../samples/core/GetStarted/Program.cs)]

## <a name="run-the-app"></a><span data-ttu-id="a63e9-154">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="a63e9-154">Run the app</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="a63e9-155">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="a63e9-155">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet run
```

### <a name="visual-studio"></a>[<span data-ttu-id="a63e9-156">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a63e9-156">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a63e9-157">Visual Studio používá nekonzistentní pracovní adresář při spuštění konzolových aplikací .NET Core.</span><span class="sxs-lookup"><span data-stu-id="a63e9-157">Visual Studio uses an inconsistent working directory when running .NET Core console apps.</span></span> <span data-ttu-id="a63e9-158">(viz [dotnet/project-system#3619](https://github.com/dotnet/project-system/issues/3619)) Výsledkem je vyvolání výjimky: *žádná taková tabulka: Blogy*.</span><span class="sxs-lookup"><span data-stu-id="a63e9-158">(see [dotnet/project-system#3619](https://github.com/dotnet/project-system/issues/3619)) This results in an exception being thrown: *no such table: Blogs*.</span></span> <span data-ttu-id="a63e9-159">Aktualizace pracovního adresáře:</span><span class="sxs-lookup"><span data-stu-id="a63e9-159">To update the working directory:</span></span>

* <span data-ttu-id="a63e9-160">Klikněte pravým tlačítkem myši na projekt a vyberte **Upravit soubor projektu.**</span><span class="sxs-lookup"><span data-stu-id="a63e9-160">Right-click on the project and select **Edit Project File**</span></span>
* <span data-ttu-id="a63e9-161">Těsně pod *TargetFramework* vlastnost, přidejte následující:</span><span class="sxs-lookup"><span data-stu-id="a63e9-161">Just below the *TargetFramework* property, add the following:</span></span>

  ``` XML
  <StartWorkingDirectory>$(MSBuildProjectDirectory)</StartWorkingDirectory>
  ```

* <span data-ttu-id="a63e9-162">Soubor uložte</span><span class="sxs-lookup"><span data-stu-id="a63e9-162">Save the file</span></span>

<span data-ttu-id="a63e9-163">Nyní můžete spustit aplikaci:</span><span class="sxs-lookup"><span data-stu-id="a63e9-163">Now you can run the app:</span></span>

* <span data-ttu-id="a63e9-164">**Ladění > spustit bez ladění**</span><span class="sxs-lookup"><span data-stu-id="a63e9-164">**Debug > Start Without Debugging**</span></span>

---

## <a name="next-steps"></a><span data-ttu-id="a63e9-165">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a63e9-165">Next steps</span></span>

* <span data-ttu-id="a63e9-166">Používání ef core ve webové aplikaci pomocí [základního ASP.NET](/aspnet/core/data/ef-rp/intro)</span><span class="sxs-lookup"><span data-stu-id="a63e9-166">Follow the [ASP.NET Core Tutorial](/aspnet/core/data/ef-rp/intro) to use EF Core in a web app</span></span>
* <span data-ttu-id="a63e9-167">Další informace o [výrazech dotazů LINQ](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)</span><span class="sxs-lookup"><span data-stu-id="a63e9-167">Learn more about [LINQ query expressions](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)</span></span>
* <span data-ttu-id="a63e9-168">[Konfigurace modelu](xref:core/modeling/index) pro určení požadovaných [a](xref:core/modeling/entity-properties#required-and-optional-properties) maximální [délky](xref:core/modeling/entity-properties#maximum-length)</span><span class="sxs-lookup"><span data-stu-id="a63e9-168">[Configure your model](xref:core/modeling/index) to specify things like [required](xref:core/modeling/entity-properties#required-and-optional-properties) and [maximum length](xref:core/modeling/entity-properties#maximum-length)</span></span>
* <span data-ttu-id="a63e9-169">Aktualizace schématu databáze po změně modelu pomocí [migrace](xref:core/managing-schemas/migrations/index)</span><span class="sxs-lookup"><span data-stu-id="a63e9-169">Use [Migrations](xref:core/managing-schemas/migrations/index) to update the database schema after changing your model</span></span>
