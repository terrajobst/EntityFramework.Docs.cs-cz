---
title: Začínáme na .NET Core – nová databáze – EF Core
author: rick-anderson
ms.author: riande
description: Začínáme s .NET Core pomocí Entity Framework Core
ms.date: 08/03/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: a0df80a8fe96be4f8cc3177919e2b087e14cb49c
ms.sourcegitcommit: 735715f10cc8a231c213e4f055d79f0effd86570
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/16/2019
ms.locfileid: "56325324"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a><span data-ttu-id="69561-103">Začínáme s EF Core na aplikace konzoly .NET Core s novou databázi</span><span class="sxs-lookup"><span data-stu-id="69561-103">Getting Started with EF Core on .NET Core Console App with a New database</span></span>

<span data-ttu-id="69561-104">V tomto kurzu vytvoříte konzolovou aplikaci .NET Core, který provádí přístup k datům s použitím Entity Framework Core databáze SQLite.</span><span class="sxs-lookup"><span data-stu-id="69561-104">In this tutorial, you create a .NET Core console app that performs data access against a SQLite database using Entity Framework Core.</span></span> <span data-ttu-id="69561-105">Migrace použijete k vytvoření databáze z modelu.</span><span class="sxs-lookup"><span data-stu-id="69561-105">You use migrations to create the database from the model.</span></span> <span data-ttu-id="69561-106">Zobrazit [ASP.NET Core – nová databáze](xref:core/get-started/aspnetcore/new-db) pro verzi sady Visual Studio pomocí ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="69561-106">See [ASP.NET Core - New database](xref:core/get-started/aspnetcore/new-db) for a Visual Studio version using ASP.NET Core MVC.</span></span>

<span data-ttu-id="69561-107">[Zobrazit ukázky v tomto článku na Githubu](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).</span><span class="sxs-lookup"><span data-stu-id="69561-107">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="69561-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="69561-108">Prerequisites</span></span>

* [<span data-ttu-id="69561-109">.NET Core 2.1 SDK</span><span class="sxs-lookup"><span data-stu-id="69561-109">The .NET Core 2.1 SDK</span></span>](https://www.microsoft.com/net/core)

## <a name="create-a-new-project"></a><span data-ttu-id="69561-110">Vytvoření nového projektu</span><span class="sxs-lookup"><span data-stu-id="69561-110">Create a new project</span></span>

* <span data-ttu-id="69561-111">Vytvořte nový projekt konzoly:</span><span class="sxs-lookup"><span data-stu-id="69561-111">Create a new console project:</span></span>

  ``` Console
  dotnet new console -o ConsoleApp.SQLite
  ```
## <a name="change-the-current-directory"></a><span data-ttu-id="69561-112">Změňte aktuální adresář</span><span class="sxs-lookup"><span data-stu-id="69561-112">Change the current directory</span></span>

<span data-ttu-id="69561-113">V dalších krocích, potřebujeme k vydávání `dotnet` příkazy vzhledem k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="69561-113">In subsequent steps, we need to issue `dotnet` commands against the application.</span></span>

* <span data-ttu-id="69561-114">Můžeme změnit aktuální adresář do adresáře aplikace následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="69561-114">We change the current directory to the application's directory like this:</span></span>

  ``` Console
  cd ConsoleApp.SQLite/
  ```
## <a name="install-entity-framework-core"></a><span data-ttu-id="69561-115">Nainstalujte Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="69561-115">Install Entity Framework Core</span></span>

<span data-ttu-id="69561-116">Použití EF Core, nainstalujte balíček pro poskytovatelů databáze, kterou chcete cílit.</span><span class="sxs-lookup"><span data-stu-id="69561-116">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="69561-117">Tento návod používá SQLite.</span><span class="sxs-lookup"><span data-stu-id="69561-117">This walkthrough uses SQLite.</span></span> <span data-ttu-id="69561-118">Seznam dostupných zprostředkovatelů najdete v části [poskytovatelé databází](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="69561-118">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="69561-119">Instalace Microsoft.EntityFrameworkCore.Sqlite a Microsoft.EntityFrameworkCore.Design</span><span class="sxs-lookup"><span data-stu-id="69561-119">Install Microsoft.EntityFrameworkCore.Sqlite and Microsoft.EntityFrameworkCore.Design</span></span>

  ```Console
  dotnet add package Microsoft.EntityFrameworkCore.Sqlite
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

* <span data-ttu-id="69561-120">Spustit `dotnet restore` nainstalovat nové balíčky.</span><span class="sxs-lookup"><span data-stu-id="69561-120">Run `dotnet restore` to install the new packages.</span></span>

## <a name="create-the-model"></a><span data-ttu-id="69561-121">Vytvoření modelu</span><span class="sxs-lookup"><span data-stu-id="69561-121">Create the model</span></span>

<span data-ttu-id="69561-122">Definování kontextu a entity třídy, které tvoří modelu.</span><span class="sxs-lookup"><span data-stu-id="69561-122">Define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="69561-123">Vytvořte nový *Model.cs* soubor s tímto obsahem.</span><span class="sxs-lookup"><span data-stu-id="69561-123">Create a new *Model.cs* file with the following contents.</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

<span data-ttu-id="69561-124">Tip: V reálné aplikaci vložit každá třída v samostatném souboru a vložit připojovací řetězec konfigurační soubor nebo prostředí proměnnou.</span><span class="sxs-lookup"><span data-stu-id="69561-124">Tip: In a real application, you put each class in a separate file and put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="69561-125">Pro zjednodušení tento kurz, všechno, co je obsažen v jednom souboru.</span><span class="sxs-lookup"><span data-stu-id="69561-125">To keep the tutorial simple, everything is contained in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="69561-126">Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="69561-126">Create the database</span></span>

<span data-ttu-id="69561-127">Jakmile budete mít modelu, použijete [migrace](xref:core/managing-schemas/migrations/index) k vytvoření databáze.</span><span class="sxs-lookup"><span data-stu-id="69561-127">Once you have a model, you use [migrations](xref:core/managing-schemas/migrations/index) to create a database.</span></span>

* <span data-ttu-id="69561-128">Spustit `dotnet ef migrations add InitialCreate` generování uživatelského rozhraní migrace a vytvářet počáteční sadu tabulek pro model.</span><span class="sxs-lookup"><span data-stu-id="69561-128">Run `dotnet ef migrations add InitialCreate` to scaffold a migration and create the initial set of tables for the model.</span></span>
* <span data-ttu-id="69561-129">Spustit `dotnet ef database update` použít novou migraci databáze.</span><span class="sxs-lookup"><span data-stu-id="69561-129">Run `dotnet ef database update` to apply the new migration to the database.</span></span> <span data-ttu-id="69561-130">Tento příkaz vytvoří databázi před použitím migrace.</span><span class="sxs-lookup"><span data-stu-id="69561-130">This command creates the database before applying migrations.</span></span>

<span data-ttu-id="69561-131">*Blogging.db* SQLite databáze je v adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="69561-131">The *blogging.db* SQLite DB is in the project directory.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="69561-132">Použití modelu</span><span class="sxs-lookup"><span data-stu-id="69561-132">Use the model</span></span>

* <span data-ttu-id="69561-133">Otevřít *Program.cs* a nahraďte jeho obsah následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="69561-133">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* <span data-ttu-id="69561-134">Testování aplikace z konzoly.</span><span class="sxs-lookup"><span data-stu-id="69561-134">Test the app from the console.</span></span> <span data-ttu-id="69561-135">Zobrazit [Poznámka: Visual Studio](#vs) ke spuštění aplikace ze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="69561-135">See the [Visual Studio note](#vs) to run the app from Visual Studio.</span></span>

  `dotnet run`

  <span data-ttu-id="69561-136">Blogů se uloží do databáze a podrobnosti o všech blogy se zobrazují v konzole.</span><span class="sxs-lookup"><span data-stu-id="69561-136">One blog is saved to the database and the details of all blogs are displayed in the console.</span></span>

  ```Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a><span data-ttu-id="69561-137">Změna modelu:</span><span class="sxs-lookup"><span data-stu-id="69561-137">Changing the model:</span></span>

- <span data-ttu-id="69561-138">Pokud provedete změny modelu, můžete použít `dotnet ef migrations add` příkaz scaffold nový [migrace](xref:core/managing-schemas/migrations/index).</span><span class="sxs-lookup"><span data-stu-id="69561-138">If you make changes to the model, you can use the `dotnet ef migrations add` command to scaffold a new [migration](xref:core/managing-schemas/migrations/index).</span></span> <span data-ttu-id="69561-139">Po zaškrtnutí automaticky generovaný kód (a všechny požadované změny), můžete použít `dotnet ef database update` příkaz pro použití schématu změn v databázi.</span><span class="sxs-lookup"><span data-stu-id="69561-139">Once you have checked the scaffolded code (and made any required changes), you can use the `dotnet ef database update` command to apply the schema changes to the database.</span></span>
- <span data-ttu-id="69561-140">Používá EF Core `__EFMigrationsHistory` tabulky v databázi ke sledování migrace, které již byly implementovány do databáze.</span><span class="sxs-lookup"><span data-stu-id="69561-140">EF Core uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>
- <span data-ttu-id="69561-141">Databázový stroj SQLite nepodporuje některé změny schématu, které podporuje většina jiných relačních databází.</span><span class="sxs-lookup"><span data-stu-id="69561-141">The SQLite database engine doesn't support certain schema changes that are supported by most other relational databases.</span></span> <span data-ttu-id="69561-142">Například `DropColumn` operace není podporována.</span><span class="sxs-lookup"><span data-stu-id="69561-142">For example, the `DropColumn` operation is not supported.</span></span> <span data-ttu-id="69561-143">Migrace EF Core dojde k vygenerování kódu pro tyto operace.</span><span class="sxs-lookup"><span data-stu-id="69561-143">EF Core Migrations will generate code for these operations.</span></span> <span data-ttu-id="69561-144">Ale pokud se pokusíte použít na databázi nebo vygenerovat skript, EF Core vyvolá výjimky.</span><span class="sxs-lookup"><span data-stu-id="69561-144">But if you try to apply them to a database or generate a script, EF Core throws exceptions.</span></span> <span data-ttu-id="69561-145">Zobrazit [omezení SQLite](../../providers/sqlite/limitations.md).</span><span class="sxs-lookup"><span data-stu-id="69561-145">See [SQLite Limitations](../../providers/sqlite/limitations.md).</span></span> <span data-ttu-id="69561-146">Pro nový vývoj zvažte vyřazení databáze a vytvořením nového spíš než migrace při změně modelu.</span><span class="sxs-lookup"><span data-stu-id="69561-146">For new development, consider dropping the database and creating a new one rather than using migrations when the model changes.</span></span>

<a name="vs"></a>
### <a name="run-from-visual-studio"></a><span data-ttu-id="69561-147">Spustit ze sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="69561-147">Run from Visual Studio</span></span>

<span data-ttu-id="69561-148">Tuto ukázku spustit ze sady Visual Studio, musíte nastavit pracovní adresář ručně, aby se použije kořen projektu.</span><span class="sxs-lookup"><span data-stu-id="69561-148">To run this sample from Visual Studio, you must set the working directory manually to be the root of the project.</span></span> <span data-ttu-id="69561-149">Pokud nenastavíte pracovního adresáře a následující `Microsoft.Data.Sqlite.SqliteException` je vyvolána výjimka: `SQLite Error 1: 'no such table: Blogs'`.</span><span class="sxs-lookup"><span data-stu-id="69561-149">If  you don't set the working directory, the following `Microsoft.Data.Sqlite.SqliteException` is thrown: `SQLite Error 1: 'no such table: Blogs'`.</span></span>

<span data-ttu-id="69561-150">Chcete-li nastavit pracovní adresář:</span><span class="sxs-lookup"><span data-stu-id="69561-150">To set the working directory:</span></span>

* <span data-ttu-id="69561-151">V **Průzkumníka řešení**, klikněte pravým tlačítkem na projekt a pak vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="69561-151">In **Solution Explorer**, right click the project and then select **Properties**.</span></span>
* <span data-ttu-id="69561-152">Vyberte **ladění** kartu v levém podokně.</span><span class="sxs-lookup"><span data-stu-id="69561-152">Select the **Debug** tab in the left pane.</span></span>
* <span data-ttu-id="69561-153">Nastavte **pracovní adresář** k adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="69561-153">Set **Working directory** to the project directory.</span></span>
* <span data-ttu-id="69561-154">Uložte změny.</span><span class="sxs-lookup"><span data-stu-id="69561-154">Save the changes.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="69561-155">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="69561-155">Additional Resources</span></span>

* [<span data-ttu-id="69561-156">Kurz: Začínáme s EF Core v ASP.NET Core s novou databázi pomocí SQLite</span><span class="sxs-lookup"><span data-stu-id="69561-156">Tutorial: Get started with EF Core on ASP.NET Core with a new database using SQLite</span></span>](xref:core/get-started/aspnetcore/new-db)
* [<span data-ttu-id="69561-157">Kurz: Začínáme se stránkami Razor v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="69561-157">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="69561-158">Kurz: Stránky Razor pomocí Entity Framework Core v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="69561-158">Tutorial: Razor Pages with Entity Framework Core in ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro)
