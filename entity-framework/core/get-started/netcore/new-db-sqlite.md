---
title: Začínáme na .NET Core – nová databáze – EF Core
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
description: Začínáme s .NET Core pomocí Entity Framework Core
keywords: .NET core, Entity Framework Core, VS Code, Visual Studio Code, Mac, Linux
ms.date: 06/05/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
ms.technology: entity-framework-core
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: e4eafed037325237345efbc3d7d42b32270a54e3
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/08/2018
ms.locfileid: "37911499"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a><span data-ttu-id="5a7ff-104">Začínáme s EF Core na aplikace konzoly .NET Core s novou databázi</span><span class="sxs-lookup"><span data-stu-id="5a7ff-104">Getting Started with EF Core on .NET Core Console App with a New database</span></span>

<span data-ttu-id="5a7ff-105">V tomto návodu vytvoříte konzolovou aplikaci .NET Core, který provádí přístup k datům s použitím Entity Framework Core databáze SQLite.</span><span class="sxs-lookup"><span data-stu-id="5a7ff-105">In this walkthrough, you create a .NET Core console app that performs data access against a SQLite database using Entity Framework Core.</span></span> <span data-ttu-id="5a7ff-106">Migrace použijete k vytvoření databáze z modelu.</span><span class="sxs-lookup"><span data-stu-id="5a7ff-106">You use migrations to create the database from the model.</span></span> <span data-ttu-id="5a7ff-107">Zobrazit [ASP.NET Core – nová databáze](xref:core/get-started/aspnetcore/new-db) pro verzi sady Visual Studio pomocí ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="5a7ff-107">See [ASP.NET Core - New database](xref:core/get-started/aspnetcore/new-db) for a Visual Studio version using ASP.NET Core MVC.</span></span>

> [!TIP]  
> <span data-ttu-id="5a7ff-108">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="5a7ff-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5a7ff-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5a7ff-109">Prerequisites</span></span>

<span data-ttu-id="5a7ff-110">[.NET Core SDK](https://www.microsoft.com/net/core) 2.1</span><span class="sxs-lookup"><span data-stu-id="5a7ff-110">[The .NET Core SDK](https://www.microsoft.com/net/core) 2.1</span></span>

## <a name="create-a-new-project"></a><span data-ttu-id="5a7ff-111">Vytvoření nového projektu</span><span class="sxs-lookup"><span data-stu-id="5a7ff-111">Create a new project</span></span>

* <span data-ttu-id="5a7ff-112">Vytvořte nový projekt konzoly:</span><span class="sxs-lookup"><span data-stu-id="5a7ff-112">Create a new console project:</span></span>

``` Console
dotnet new console -o ConsoleApp.SQLite
cd ConsoleApp.SQLite/
```

## <a name="install-entity-framework-core"></a><span data-ttu-id="5a7ff-113">Nainstalujte Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="5a7ff-113">Install Entity Framework Core</span></span>

<span data-ttu-id="5a7ff-114">Použití EF Core, nainstalujte balíček pro poskytovatelů databáze, kterou chcete cílit.</span><span class="sxs-lookup"><span data-stu-id="5a7ff-114">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="5a7ff-115">Tento návod používá SQLite.</span><span class="sxs-lookup"><span data-stu-id="5a7ff-115">This walkthrough uses SQLite.</span></span> <span data-ttu-id="5a7ff-116">Seznam dostupných zprostředkovatelů najdete v části [poskytovatelé databází](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="5a7ff-116">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="5a7ff-117">Instalace Microsoft.EntityFrameworkCore.Sqlite a Microsoft.EntityFrameworkCore.Design</span><span class="sxs-lookup"><span data-stu-id="5a7ff-117">Install Microsoft.EntityFrameworkCore.Sqlite and Microsoft.EntityFrameworkCore.Design</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
dotnet add package Microsoft.EntityFrameworkCore.Design
```

* <span data-ttu-id="5a7ff-118">Spustit `dotnet restore` nainstalovat nové balíčky.</span><span class="sxs-lookup"><span data-stu-id="5a7ff-118">Run `dotnet restore` to install the new packages.</span></span>

## <a name="create-the-model"></a><span data-ttu-id="5a7ff-119">Vytvoření modelu</span><span class="sxs-lookup"><span data-stu-id="5a7ff-119">Create the model</span></span>

<span data-ttu-id="5a7ff-120">Definování kontextu a entity třídy, které tvoří modelu.</span><span class="sxs-lookup"><span data-stu-id="5a7ff-120">Define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="5a7ff-121">Vytvořte nový *Model.cs* soubor s tímto obsahem.</span><span class="sxs-lookup"><span data-stu-id="5a7ff-121">Create a new *Model.cs* file with the following contents.</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

<span data-ttu-id="5a7ff-122">Tip: V reálné aplikaci můžete vložit každá třída v samostatném souboru a vložit připojovací řetězec do konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="5a7ff-122">Tip: In a real application, you put each class in a separate file and put the connection string in a configuration file.</span></span> <span data-ttu-id="5a7ff-123">Pro zjednodušení tento kurz, všechno, co je obsažen v jednom souboru.</span><span class="sxs-lookup"><span data-stu-id="5a7ff-123">To keep the tutorial simple, everything is contained in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="5a7ff-124">Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="5a7ff-124">Create the database</span></span>

<span data-ttu-id="5a7ff-125">Jakmile budete mít modelu, použijete [migrace](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) k vytvoření databáze.</span><span class="sxs-lookup"><span data-stu-id="5a7ff-125">Once you have a model, you use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="5a7ff-126">Spustit `dotnet ef migrations add InitialCreate` generování uživatelského rozhraní migrace a vytvářet počáteční sadu tabulek pro model.</span><span class="sxs-lookup"><span data-stu-id="5a7ff-126">Run `dotnet ef migrations add InitialCreate` to scaffold a migration and create the initial set of tables for the model.</span></span>
* <span data-ttu-id="5a7ff-127">Spustit `dotnet ef database update` použít novou migraci databáze.</span><span class="sxs-lookup"><span data-stu-id="5a7ff-127">Run `dotnet ef database update` to apply the new migration to the database.</span></span> <span data-ttu-id="5a7ff-128">Tento příkaz vytvoří databázi před použitím migrace.</span><span class="sxs-lookup"><span data-stu-id="5a7ff-128">This command creates the database before applying migrations.</span></span>

<span data-ttu-id="5a7ff-129">*Blogging.db*\* je databáze SQLite v adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="5a7ff-129">The *blogging.db*\* SQLite DB is in the project directory.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="5a7ff-130">Použití modelu</span><span class="sxs-lookup"><span data-stu-id="5a7ff-130">Use your model</span></span>

* <span data-ttu-id="5a7ff-131">Otevřít *Program.cs* a nahraďte jeho obsah následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="5a7ff-131">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* <span data-ttu-id="5a7ff-132">Testování aplikace:</span><span class="sxs-lookup"><span data-stu-id="5a7ff-132">Test the app:</span></span>

  `dotnet run`

  <span data-ttu-id="5a7ff-133">Blogů se uloží do databáze a podrobnosti o všech blogy se zobrazují v konzole.</span><span class="sxs-lookup"><span data-stu-id="5a7ff-133">One blog is saved to the database and the details of all blogs are displayed in the console.</span></span>

  ``` Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a><span data-ttu-id="5a7ff-134">Změna modelu:</span><span class="sxs-lookup"><span data-stu-id="5a7ff-134">Changing the model:</span></span>

- <span data-ttu-id="5a7ff-135">Pokud provedete změny modelu, můžete použít `dotnet ef migrations add` příkaz scaffold nový [migrace](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) provést odpovídající schématu změn v databázi.</span><span class="sxs-lookup"><span data-stu-id="5a7ff-135">If you make changes to your model, you can use the `dotnet ef migrations add` command to scaffold a new [migration](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations)  to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="5a7ff-136">Po zaškrtnutí automaticky generovaný kód (a všechny požadované změny), můžete použít `dotnet ef database update` příkaz změny se projeví do databáze.</span><span class="sxs-lookup"><span data-stu-id="5a7ff-136">Once you have checked the scaffolded code (and made any required changes), you can use the `dotnet ef database update` command to apply the changes to the database.</span></span>
- <span data-ttu-id="5a7ff-137">Používá EF `__EFMigrationsHistory` tabulky v databázi ke sledování migrace, které již byly implementovány do databáze.</span><span class="sxs-lookup"><span data-stu-id="5a7ff-137">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>
- <span data-ttu-id="5a7ff-138">SQLite nepodporuje všechny migrace (změny schématu) z důvodu omezení v SQLite.</span><span class="sxs-lookup"><span data-stu-id="5a7ff-138">SQLite does not support all migrations (schema changes) due to limitations in SQLite.</span></span> <span data-ttu-id="5a7ff-139">Zobrazit [omezení SQLite](../../providers/sqlite/limitations.md).</span><span class="sxs-lookup"><span data-stu-id="5a7ff-139">See [SQLite Limitations](../../providers/sqlite/limitations.md).</span></span> <span data-ttu-id="5a7ff-140">Pro nový vývoj zvažte vyřazení databáze a vytvořením nového spíš než migrace při změně vašeho modelu.</span><span class="sxs-lookup"><span data-stu-id="5a7ff-140">For new development, consider dropping the database and creating a new one rather than using migrations when your model changes.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5a7ff-141">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="5a7ff-141">Additional Resources</span></span>

* <span data-ttu-id="5a7ff-142">[.NET core – nová databáze s SQLite](xref:core/get-started/netcore/new-db-sqlite) – kurz EF konzole pro různé platformy.</span><span class="sxs-lookup"><span data-stu-id="5a7ff-142">[.NET Core - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="5a7ff-143">Úvod do ASP.NET Core MVC v systému Mac nebo Linux</span><span class="sxs-lookup"><span data-stu-id="5a7ff-143">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="5a7ff-144">Úvod do ASP.NET Core MVC se sadou Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5a7ff-144">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="5a7ff-145">Začínáme s technologiemi ASP.NET Core a Entity Framework Core pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5a7ff-145">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
