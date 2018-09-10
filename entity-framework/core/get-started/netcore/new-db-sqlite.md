---
title: Začínáme na .NET Core – nová databáze – EF Core
author: rick-anderson
ms.author: riande
description: Začínáme s .NET Core pomocí Entity Framework Core
ms.date: 08/03/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: 69f7160cd82f2ec58e436a85369b52fe7c31040c
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250306"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a><span data-ttu-id="36854-103">Začínáme s EF Core na aplikace konzoly .NET Core s novou databázi</span><span class="sxs-lookup"><span data-stu-id="36854-103">Getting Started with EF Core on .NET Core Console App with a New database</span></span>

<span data-ttu-id="36854-104">V tomto kurzu vytvoříte konzolovou aplikaci .NET Core, který provádí přístup k datům s použitím Entity Framework Core databáze SQLite.</span><span class="sxs-lookup"><span data-stu-id="36854-104">In this tutorial, you create a .NET Core console app that performs data access against a SQLite database using Entity Framework Core.</span></span> <span data-ttu-id="36854-105">Migrace použijete k vytvoření databáze z modelu.</span><span class="sxs-lookup"><span data-stu-id="36854-105">You use migrations to create the database from the model.</span></span> <span data-ttu-id="36854-106">Zobrazit [ASP.NET Core – nová databáze](xref:core/get-started/aspnetcore/new-db) pro verzi sady Visual Studio pomocí ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="36854-106">See [ASP.NET Core - New database](xref:core/get-started/aspnetcore/new-db) for a Visual Studio version using ASP.NET Core MVC.</span></span>

<span data-ttu-id="36854-107">Zobrazit ukázky v tomto článku na Githubu] (https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).</span><span class="sxs-lookup"><span data-stu-id="36854-107">View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="36854-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="36854-108">Prerequisites</span></span>

* [<span data-ttu-id="36854-109">.NET Core 2.1 SDK</span><span class="sxs-lookup"><span data-stu-id="36854-109">The .NET Core 2.1 SDK</span></span>](https://www.microsoft.com/net/core)

## <a name="create-a-new-project"></a><span data-ttu-id="36854-110">Vytvoření nového projektu</span><span class="sxs-lookup"><span data-stu-id="36854-110">Create a new project</span></span>

* <span data-ttu-id="36854-111">Vytvořte nový projekt konzoly:</span><span class="sxs-lookup"><span data-stu-id="36854-111">Create a new console project:</span></span>

  ``` Console
  dotnet new console -o ConsoleApp.SQLite
  ```
## <a name="change-the-current-directory"></a><span data-ttu-id="36854-112">Změňte aktuální adresář</span><span class="sxs-lookup"><span data-stu-id="36854-112">Change the current directory</span></span> 

<span data-ttu-id="36854-113">V dalších krocích, potřebujeme k vydávání `dotnet` příkazy vzhledem k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="36854-113">In subsequent steps, we need to issue `dotnet` commands against the application.</span></span> 

* <span data-ttu-id="36854-114">Můžeme změnit aktuální adresář do adresáře aplikace následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="36854-114">We change the current directory to the application's directory like this:</span></span>

  ``` Console
  cd ConsoleApp.SQLite/
  ```
## <a name="install-entity-framework-core"></a><span data-ttu-id="36854-115">Nainstalujte Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="36854-115">Install Entity Framework Core</span></span>

<span data-ttu-id="36854-116">Použití EF Core, nainstalujte balíček pro poskytovatelů databáze, kterou chcete cílit.</span><span class="sxs-lookup"><span data-stu-id="36854-116">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="36854-117">Tento návod používá SQLite.</span><span class="sxs-lookup"><span data-stu-id="36854-117">This walkthrough uses SQLite.</span></span> <span data-ttu-id="36854-118">Seznam dostupných zprostředkovatelů najdete v části [poskytovatelé databází](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="36854-118">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="36854-119">Instalace Microsoft.EntityFrameworkCore.Sqlite a Microsoft.EntityFrameworkCore.Design</span><span class="sxs-lookup"><span data-stu-id="36854-119">Install Microsoft.EntityFrameworkCore.Sqlite and Microsoft.EntityFrameworkCore.Design</span></span>

  ```Console
  dotnet add package Microsoft.EntityFrameworkCore.Sqlite
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

* <span data-ttu-id="36854-120">Spustit `dotnet restore` nainstalovat nové balíčky.</span><span class="sxs-lookup"><span data-stu-id="36854-120">Run `dotnet restore` to install the new packages.</span></span>

## <a name="create-the-model"></a><span data-ttu-id="36854-121">Vytvoření modelu</span><span class="sxs-lookup"><span data-stu-id="36854-121">Create the model</span></span>

<span data-ttu-id="36854-122">Definování kontextu a entity třídy, které tvoří modelu.</span><span class="sxs-lookup"><span data-stu-id="36854-122">Define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="36854-123">Vytvořte nový *Model.cs* soubor s tímto obsahem.</span><span class="sxs-lookup"><span data-stu-id="36854-123">Create a new *Model.cs* file with the following contents.</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

<span data-ttu-id="36854-124">Tip: V reálné aplikaci, můžete vložit každá třída v samostatném souboru a vložit připojovací řetězec konfigurační soubor nebo prostředí proměnnou.</span><span class="sxs-lookup"><span data-stu-id="36854-124">Tip: In a real application, you put each class in a separate file and put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="36854-125">Pro zjednodušení tento kurz, všechno, co je obsažen v jednom souboru.</span><span class="sxs-lookup"><span data-stu-id="36854-125">To keep the tutorial simple, everything is contained in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="36854-126">Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="36854-126">Create the database</span></span>

<span data-ttu-id="36854-127">Jakmile budete mít modelu, použijete [migrace](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) k vytvoření databáze.</span><span class="sxs-lookup"><span data-stu-id="36854-127">Once you have a model, you use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="36854-128">Spustit `dotnet ef migrations add InitialCreate` generování uživatelského rozhraní migrace a vytvářet počáteční sadu tabulek pro model.</span><span class="sxs-lookup"><span data-stu-id="36854-128">Run `dotnet ef migrations add InitialCreate` to scaffold a migration and create the initial set of tables for the model.</span></span>
* <span data-ttu-id="36854-129">Spustit `dotnet ef database update` použít novou migraci databáze.</span><span class="sxs-lookup"><span data-stu-id="36854-129">Run `dotnet ef database update` to apply the new migration to the database.</span></span> <span data-ttu-id="36854-130">Tento příkaz vytvoří databázi před použitím migrace.</span><span class="sxs-lookup"><span data-stu-id="36854-130">This command creates the database before applying migrations.</span></span>

<span data-ttu-id="36854-131">*Blogging.db*\* je databáze SQLite v adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="36854-131">The *blogging.db*\* SQLite DB is in the project directory.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="36854-132">Použití modelu</span><span class="sxs-lookup"><span data-stu-id="36854-132">Use the model</span></span>

* <span data-ttu-id="36854-133">Otevřít *Program.cs* a nahraďte jeho obsah následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="36854-133">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* <span data-ttu-id="36854-134">Testování aplikace:</span><span class="sxs-lookup"><span data-stu-id="36854-134">Test the app:</span></span>

  `dotnet run`

  <span data-ttu-id="36854-135">Blogů se uloží do databáze a podrobnosti o všech blogy se zobrazují v konzole.</span><span class="sxs-lookup"><span data-stu-id="36854-135">One blog is saved to the database and the details of all blogs are displayed in the console.</span></span>

  ```Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a><span data-ttu-id="36854-136">Změna modelu:</span><span class="sxs-lookup"><span data-stu-id="36854-136">Changing the model:</span></span>

- <span data-ttu-id="36854-137">Pokud provedete změny modelu, můžete použít `dotnet ef migrations add` příkaz scaffold nový [migrace](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="36854-137">If you make changes to the model, you can use the `dotnet ef migrations add` command to scaffold a new [migration](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations).</span></span> <span data-ttu-id="36854-138">Po zaškrtnutí automaticky generovaný kód (a všechny požadované změny), můžete použít `dotnet ef database update` příkaz pro použití schématu změn v databázi.</span><span class="sxs-lookup"><span data-stu-id="36854-138">Once you have checked the scaffolded code (and made any required changes), you can use the `dotnet ef database update` command to apply the schema changes to the database.</span></span>
- <span data-ttu-id="36854-139">Používá EF Core `__EFMigrationsHistory` tabulky v databázi ke sledování migrace, které již byly implementovány do databáze.</span><span class="sxs-lookup"><span data-stu-id="36854-139">EF Core uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>
- <span data-ttu-id="36854-140">Databázový stroj SQLite nepodporuje některé změny schématu, které podporuje většina jiných relačních databází.</span><span class="sxs-lookup"><span data-stu-id="36854-140">The SQLite database engine doesn't support certain schema changes that are supported by most other relational databases.</span></span> <span data-ttu-id="36854-141">Například `DropColumn` operace není podporována.</span><span class="sxs-lookup"><span data-stu-id="36854-141">For example, the `DropColumn` operation is not supported.</span></span> <span data-ttu-id="36854-142">Migrace EF Core dojde k vygenerování kódu pro tyto operace.</span><span class="sxs-lookup"><span data-stu-id="36854-142">EF Core Migrations will generate code for these operations.</span></span> <span data-ttu-id="36854-143">Ale pokud se pokusíte použít na databázi nebo vygenerovat skript, EF Core vyvolá výjimky.</span><span class="sxs-lookup"><span data-stu-id="36854-143">But if you try to apply them to a database or generate a script, EF Core throws exceptions.</span></span> <span data-ttu-id="36854-144">Zobrazit [omezení SQLite](../../providers/sqlite/limitations.md).</span><span class="sxs-lookup"><span data-stu-id="36854-144">See [SQLite Limitations](../../providers/sqlite/limitations.md).</span></span> <span data-ttu-id="36854-145">Pro nový vývoj zvažte vyřazení databáze a vytvořením nového spíš než migrace při změně modelu.</span><span class="sxs-lookup"><span data-stu-id="36854-145">For new development, consider dropping the database and creating a new one rather than using migrations when the model changes.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="36854-146">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="36854-146">Additional Resources</span></span>

* [<span data-ttu-id="36854-147">Úvod do ASP.NET Core MVC v systému Mac nebo Linux</span><span class="sxs-lookup"><span data-stu-id="36854-147">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="36854-148">Úvod do ASP.NET Core MVC se sadou Visual Studio</span><span class="sxs-lookup"><span data-stu-id="36854-148">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="36854-149">Začínáme s technologiemi ASP.NET Core a Entity Framework Core pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="36854-149">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
