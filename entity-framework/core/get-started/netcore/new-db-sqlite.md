---
title: Začínáme na základní EF .NET Core - novou databázi –
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
description: Začínáme s .NET Core pomocí Entity Framework Core
keywords: .NET core, Entity Framework Core, VS kód, Visual Studio Code, Mac, Linux
ms.date: 04/05/2017
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
ms.technology: entity-framework-core
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: fcace3c0f259b1a456d9ca1086e6a1549c070d57
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812531"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a><span data-ttu-id="a374e-104">Začínáme s EF základní na konzolové aplikace .NET Core s novou databázi</span><span class="sxs-lookup"><span data-stu-id="a374e-104">Getting Started with EF Core on .NET Core Console App with a New database</span></span>

<span data-ttu-id="a374e-105">V tomto návodu vytvoříte konzolovou aplikaci .NET Core, která provádí základní přístup k datům s použitím Entity Framework Core databáze SQLite.</span><span class="sxs-lookup"><span data-stu-id="a374e-105">In this walkthrough, you will create a .NET Core console app that performs basic data access against a SQLite database using Entity Framework Core.</span></span> <span data-ttu-id="a374e-106">Migrace použije k vytvoření databáze z modelu.</span><span class="sxs-lookup"><span data-stu-id="a374e-106">You will use migrations to create the database from your model.</span></span> <span data-ttu-id="a374e-107">V tématu [ASP.NET Core - novou databázi](xref:core/get-started/aspnetcore/new-db) pro verzi Visual Studio pomocí technologie ASP.NET MVC jádra.</span><span class="sxs-lookup"><span data-stu-id="a374e-107">See [ASP.NET Core - New database](xref:core/get-started/aspnetcore/new-db) for a Visual Studio version using ASP.NET Core MVC.</span></span>

> [!TIP]  
> <span data-ttu-id="a374e-108">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="a374e-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a374e-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a374e-109">Prerequisites</span></span>

<span data-ttu-id="a374e-110">Následující požadované součásti jsou nutné k dokončení tohoto názorného postupu:</span><span class="sxs-lookup"><span data-stu-id="a374e-110">The following prerequisites are needed to complete this walkthrough:</span></span>
* <span data-ttu-id="a374e-111">Operační systém, který podporuje .NET Core.</span><span class="sxs-lookup"><span data-stu-id="a374e-111">An operating system that supports .NET Core.</span></span>
* <span data-ttu-id="a374e-112">[.NET Core SDK](https://www.microsoft.com/net/core) 2.0 (i když pokyny slouží k vytvoření aplikace s předchozí verzí s velmi málo změny).</span><span class="sxs-lookup"><span data-stu-id="a374e-112">[The .NET Core SDK](https://www.microsoft.com/net/core) 2.0 (although the instructions can be used to create an application with a previous version with very few modifications).</span></span>

## <a name="create-a-new-project"></a><span data-ttu-id="a374e-113">Vytvoření nového projektu</span><span class="sxs-lookup"><span data-stu-id="a374e-113">Create a new project</span></span>

* <span data-ttu-id="a374e-114">Vytvořte novou `ConsoleApp.SQLite` složky pro váš projekt a použití `dotnet` příkaz k naplnění s aplikací .NET Core.</span><span class="sxs-lookup"><span data-stu-id="a374e-114">Create a new `ConsoleApp.SQLite` folder for your project and use the `dotnet` command to populate it with a .NET Core app.</span></span>

``` Console
mkdir ConsoleApp.SQLite
cd ConsoleApp.SQLite/
dotnet new console
```

## <a name="install-entity-framework-core"></a><span data-ttu-id="a374e-115">Instalace jádra Entity Framework</span><span class="sxs-lookup"><span data-stu-id="a374e-115">Install Entity Framework Core</span></span>

<span data-ttu-id="a374e-116">Abyste mohli používat EF jádra, nainstalujte balíček pro následující zprostředkovatele databáze, kterou chcete zacílit.</span><span class="sxs-lookup"><span data-stu-id="a374e-116">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="a374e-117">Tento návod používá SQLite.</span><span class="sxs-lookup"><span data-stu-id="a374e-117">This walkthrough uses SQLite.</span></span> <span data-ttu-id="a374e-118">Seznam dostupných zprostředkovatelů naleznete v části [zprostředkovatelů databáze](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="a374e-118">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="a374e-119">Instalace Microsoft.EntityFrameworkCore.Sqlite a Microsoft.EntityFrameworkCore.Design</span><span class="sxs-lookup"><span data-stu-id="a374e-119">Install Microsoft.EntityFrameworkCore.Sqlite and Microsoft.EntityFrameworkCore.Design</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
dotnet add package Microsoft.EntityFrameworkCore.Design
```

* <span data-ttu-id="a374e-120">Ručně upravte `ConsoleApp.SQLite.csproj` přidat DotNetCliToolReference do Microsoft.EntityFrameworkCore.Tools.DotNet:</span><span class="sxs-lookup"><span data-stu-id="a374e-120">Manually edit `ConsoleApp.SQLite.csproj` to add a DotNetCliToolReference to Microsoft.EntityFrameworkCore.Tools.DotNet:</span></span>

  ``` xml
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  </ItemGroup>
  ```

<span data-ttu-id="a374e-121">`ConsoleApp.SQLite.csproj` má teď obsahují následující:</span><span class="sxs-lookup"><span data-stu-id="a374e-121">`ConsoleApp.SQLite.csproj` should now contain the following:</span></span>

[!code[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/ConsoleApp.SQLite.csproj)]

 <span data-ttu-id="a374e-122">Poznámka: Byly čísla verzí používá výše v době publikování správně.</span><span class="sxs-lookup"><span data-stu-id="a374e-122">Note: The version numbers used above were correct at the time of publishing.</span></span>

*  <span data-ttu-id="a374e-123">Spustit `dotnet restore` k instalaci nové balíčky.</span><span class="sxs-lookup"><span data-stu-id="a374e-123">Run `dotnet restore` to install the new packages.</span></span>

## <a name="create-the-model"></a><span data-ttu-id="a374e-124">Vytvoření modelu</span><span class="sxs-lookup"><span data-stu-id="a374e-124">Create the model</span></span>

<span data-ttu-id="a374e-125">Definujte kontextu a entity třídy, které tvoří modelu.</span><span class="sxs-lookup"><span data-stu-id="a374e-125">Define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="a374e-126">Vytvořte novou *Model.cs* soubor s tímto obsahem.</span><span class="sxs-lookup"><span data-stu-id="a374e-126">Create a new *Model.cs* file with the following contents.</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

<span data-ttu-id="a374e-127">Tip: V reálné aplikaci jste by uložte každá třída v samostatném souboru a připojovací řetězec v konfiguračním souboru.</span><span class="sxs-lookup"><span data-stu-id="a374e-127">Tip: In a real application you would put each class in a separate file and put the connection string in a configuration file.</span></span> <span data-ttu-id="a374e-128">Zjednodušení tento kurz, jsme jsou uvedení vše v jednom souboru.</span><span class="sxs-lookup"><span data-stu-id="a374e-128">To keep the tutorial simple, we are putting everything in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="a374e-129">Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="a374e-129">Create the database</span></span>

<span data-ttu-id="a374e-130">Jakmile máte modelu, můžete použít [migrace](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) k vytvoření databáze.</span><span class="sxs-lookup"><span data-stu-id="a374e-130">Once you have a model, you can use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="a374e-131">Spustit `dotnet ef migrations add InitialCreate` vygenerovat migrace a vytvořit počáteční sadu tabulek pro model.</span><span class="sxs-lookup"><span data-stu-id="a374e-131">Run `dotnet ef migrations add InitialCreate` to scaffold a migration and create the initial set of tables for the model.</span></span>
* <span data-ttu-id="a374e-132">Spustit `dotnet ef database update` použít nové migrace do databáze.</span><span class="sxs-lookup"><span data-stu-id="a374e-132">Run `dotnet ef database update` to apply the new migration to the database.</span></span> <span data-ttu-id="a374e-133">Tento příkaz vytvoří databázi před použitím migrace.</span><span class="sxs-lookup"><span data-stu-id="a374e-133">This command creates the database before applying migrations.</span></span>

> [!NOTE]  
> <span data-ttu-id="a374e-134">Při použití relativní cesty s SQLite, cesta bude relativně k hlavní sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="a374e-134">When using relative paths with SQLite, the path will be relative to the application's main assembly.</span></span> <span data-ttu-id="a374e-135">V této ukázce je hlavní binární `bin/Debug/netcoreapp2.0/ConsoleApp.SQLite.dll`, aby se v databázi SQLite `bin/Debug/netcoreapp2.0/blogging.db`.</span><span class="sxs-lookup"><span data-stu-id="a374e-135">In this sample, the main binary is `bin/Debug/netcoreapp2.0/ConsoleApp.SQLite.dll`, so the SQLite database will be in `bin/Debug/netcoreapp2.0/blogging.db`.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="a374e-136">Použití modelu</span><span class="sxs-lookup"><span data-stu-id="a374e-136">Use your model</span></span>

* <span data-ttu-id="a374e-137">Otevřete *Program.cs* a nahraďte jeho obsah následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="a374e-137">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* <span data-ttu-id="a374e-138">Testovací aplikace:</span><span class="sxs-lookup"><span data-stu-id="a374e-138">Test the app:</span></span>

  `dotnet run`

  <span data-ttu-id="a374e-139">Blogů je uložený na databázi a podrobnosti o všech blogy se zobrazují v konzole.</span><span class="sxs-lookup"><span data-stu-id="a374e-139">One blog is saved to the database and the details of all blogs are displayed in the console.</span></span>

  ``` Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a><span data-ttu-id="a374e-140">Změna modelu:</span><span class="sxs-lookup"><span data-stu-id="a374e-140">Changing the model:</span></span>

- <span data-ttu-id="a374e-141">Pokud provedete změny modelu, můžete použít `dotnet ef migrations add` příkaz chcete vygenerovat nový [migrace](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) aby odpovídající schéma změn v databázi.</span><span class="sxs-lookup"><span data-stu-id="a374e-141">If you make changes to your model, you can use the `dotnet ef migrations add` command to scaffold a new [migration](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations)  to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="a374e-142">Po zaškrtnutí automaticky generovaný kód (a všechny požadované změny provedené), můžete použít `dotnet ef database update` příkaz k použití změn do databáze.</span><span class="sxs-lookup"><span data-stu-id="a374e-142">Once you have checked the scaffolded code (and made any required changes), you can use the `dotnet ef database update` command to apply the changes to the database.</span></span>
- <span data-ttu-id="a374e-143">Používá EF `__EFMigrationsHistory` tabulky v databázi ke sledování migrace, která již byla do databáze použít.</span><span class="sxs-lookup"><span data-stu-id="a374e-143">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>
- <span data-ttu-id="a374e-144">SQLite nepodporuje všechny migrace (změny schématu) z důvodu omezení v SQLite.</span><span class="sxs-lookup"><span data-stu-id="a374e-144">SQLite does not support all migrations (schema changes) due to limitations in SQLite.</span></span> <span data-ttu-id="a374e-145">V tématu [SQLite omezení](../../providers/sqlite/limitations.md).</span><span class="sxs-lookup"><span data-stu-id="a374e-145">See [SQLite Limitations](../../providers/sqlite/limitations.md).</span></span> <span data-ttu-id="a374e-146">Pro nový vývoj zvažte vyřazení databáze a vytvoření nového než pomocí migrace, když se změní modelu.</span><span class="sxs-lookup"><span data-stu-id="a374e-146">For new development, consider dropping the database and creating a new one rather than using migrations when your model changes.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a374e-147">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="a374e-147">Additional Resources</span></span>

* <span data-ttu-id="a374e-148">[.NET core - novou databázi pomocí SQLite](xref:core/get-started/netcore/new-db-sqlite) – kurz EF konzoly napříč platformami.</span><span class="sxs-lookup"><span data-stu-id="a374e-148">[.NET Core - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="a374e-149">Úvod do základní ASP.NET MVC v Mac nebo Linux</span><span class="sxs-lookup"><span data-stu-id="a374e-149">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="a374e-150">Úvod do základní ASP.NET MVC pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a374e-150">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="a374e-151">Začínáme s technologiemi ASP.NET Core a Entity Framework Core pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a374e-151">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
