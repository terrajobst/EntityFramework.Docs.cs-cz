---
title: Začínáme v rozhraní .NET Framework – nová databáze – EF Core
author: rowanmiller
ms.author: divega
ms.date: 08/06/2018
ms.assetid: 52b69727-ded9-4a7b-b8d5-73f3acfbbad3
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/new-db
ms.openlocfilehash: 088ac915041489242eb8090e7bf3a2bdc8036534
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614425"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a><span data-ttu-id="0e4d0-102">Začínáme s EF Core na rozhraní .NET Framework s novou databázi</span><span class="sxs-lookup"><span data-stu-id="0e4d0-102">Getting started with EF Core on .NET Framework with a New Database</span></span>

<span data-ttu-id="0e4d0-103">V tomto kurzu vytvoříte konzolovou aplikaci, která provádí základní přístup k datům pro databázi serveru Microsoft SQL Server používá nástroj Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0e4d0-103">In this tutorial, you build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="0e4d0-104">Migrace použijete k vytvoření databáze z modelu.</span><span class="sxs-lookup"><span data-stu-id="0e4d0-104">You use migrations to create the database from a model.</span></span>

<span data-ttu-id="0e4d0-105">[Zobrazit ukázky v tomto článku na Githubu](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb).</span><span class="sxs-lookup"><span data-stu-id="0e4d0-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0e4d0-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0e4d0-106">Prerequisites</span></span>

* [<span data-ttu-id="0e4d0-107">Visual Studio 2017 verze 15.7 nebo novější</span><span class="sxs-lookup"><span data-stu-id="0e4d0-107">Visual Studio 2017 version 15.7 or later</span></span>](https://www.visualstudio.com/downloads/)

## <a name="create-a-new-project"></a><span data-ttu-id="0e4d0-108">Vytvoření nového projektu</span><span class="sxs-lookup"><span data-stu-id="0e4d0-108">Create a new project</span></span>

* <span data-ttu-id="0e4d0-109">Otevřít Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="0e4d0-109">Open Visual Studio 2017</span></span>

* <span data-ttu-id="0e4d0-110">**Soubor > Nový > projekt...**</span><span class="sxs-lookup"><span data-stu-id="0e4d0-110">**File > New > Project...**</span></span>

* <span data-ttu-id="0e4d0-111">V levé nabídce vyberte **nainstalováno > Visual C# > Windows Desktop**</span><span class="sxs-lookup"><span data-stu-id="0e4d0-111">From the left menu select **Installed > Visual C# > Windows Desktop**</span></span>

* <span data-ttu-id="0e4d0-112">Vyberte **Konzolová aplikace (.NET Framework)** šablony projektu</span><span class="sxs-lookup"><span data-stu-id="0e4d0-112">Select the **Console App (.NET Framework)** project template</span></span>

* <span data-ttu-id="0e4d0-113">Ujistěte se, že projekt cílí **rozhraní .NET Framework 4.6.1** nebo novější</span><span class="sxs-lookup"><span data-stu-id="0e4d0-113">Make sure that the project targets **.NET Framework 4.6.1** or later</span></span>

* <span data-ttu-id="0e4d0-114">Pojmenujte projekt *ConsoleApp.NewDb* a klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="0e4d0-114">Name the project *ConsoleApp.NewDb* and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="0e4d0-115">Nainstalujte rozhraní Entity Framework</span><span class="sxs-lookup"><span data-stu-id="0e4d0-115">Install Entity Framework</span></span>

<span data-ttu-id="0e4d0-116">Použití EF Core, nainstalujte balíček pro poskytovatelů databáze, kterou chcete cílit.</span><span class="sxs-lookup"><span data-stu-id="0e4d0-116">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="0e4d0-117">Tento kurz používá systém SQL Server.</span><span class="sxs-lookup"><span data-stu-id="0e4d0-117">This tutorial uses SQL Server.</span></span> <span data-ttu-id="0e4d0-118">Seznam dostupných zprostředkovatelů najdete v části [poskytovatelé databází](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="0e4d0-118">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="0e4d0-119">Nástroje > Správce balíčků NuGet > Konzola správce balíčků</span><span class="sxs-lookup"><span data-stu-id="0e4d0-119">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="0e4d0-120">Spustit `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="0e4d0-120">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="0e4d0-121">Později v tomto kurzu použijete některé Entity Framework Tools pro správnou údržbu databáze.</span><span class="sxs-lookup"><span data-stu-id="0e4d0-121">Later in this tutorial you use some Entity Framework Tools to maintain the database.</span></span> <span data-ttu-id="0e4d0-122">Balíček nástroje tak instalaci.</span><span class="sxs-lookup"><span data-stu-id="0e4d0-122">So install the tools package as well.</span></span>

* <span data-ttu-id="0e4d0-123">Spustit `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="0e4d0-123">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="create-the-model"></a><span data-ttu-id="0e4d0-124">Vytvoření modelu</span><span class="sxs-lookup"><span data-stu-id="0e4d0-124">Create the model</span></span>

<span data-ttu-id="0e4d0-125">Nyní je možné definovat kontext a entity třídy, které tvoří modelu.</span><span class="sxs-lookup"><span data-stu-id="0e4d0-125">Now it's time to define a context and entity classes that make up the model.</span></span>

* <span data-ttu-id="0e4d0-126">**Projekt > Přidat třídu...**</span><span class="sxs-lookup"><span data-stu-id="0e4d0-126">**Project > Add Class...**</span></span>

* <span data-ttu-id="0e4d0-127">Zadejte *Model.cs* jako název a klikněte na **OK**</span><span class="sxs-lookup"><span data-stu-id="0e4d0-127">Enter *Model.cs* as the name and click **OK**</span></span>

* <span data-ttu-id="0e4d0-128">Obsah souboru nahraďte následujícím kódem</span><span class="sxs-lookup"><span data-stu-id="0e4d0-128">Replace the contents of the file with the following code</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Model.cs)] 

> [!TIP]  
> <span data-ttu-id="0e4d0-129">V reálné aplikaci byste umístit každá třída v samostatném souboru a vložte připojovací řetězec konfigurační soubor nebo prostředí proměnnou.</span><span class="sxs-lookup"><span data-stu-id="0e4d0-129">In a real application you would put each class in a separate file and put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="0e4d0-130">Z důvodu zjednodušení všechno, co je v souboru jednoho kódu pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="0e4d0-130">For the sake of simplicity, everything is in a single code file for this tutorial.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="0e4d0-131">Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="0e4d0-131">Create the database</span></span>

<span data-ttu-id="0e4d0-132">Teď, když máte model, můžete k vytvoření databáze migrace.</span><span class="sxs-lookup"><span data-stu-id="0e4d0-132">Now that you have a model, you can use migrations to create a database.</span></span>

* <span data-ttu-id="0e4d0-133">**Nástroje > Správce balíčků NuGet > Konzola správce balíčků**</span><span class="sxs-lookup"><span data-stu-id="0e4d0-133">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="0e4d0-134">Spustit `Add-Migration InitialCreate` k migraci na vytvoření počáteční sadu tabulek pro model.</span><span class="sxs-lookup"><span data-stu-id="0e4d0-134">Run `Add-Migration InitialCreate` to scaffold a migration to create the initial set of tables for the model.</span></span>

* <span data-ttu-id="0e4d0-135">Spustit `Update-Database` použít novou migraci databáze.</span><span class="sxs-lookup"><span data-stu-id="0e4d0-135">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="0e4d0-136">Vzhledem k tomu, že databáze ještě neexistuje, vytvoří se před použitím migrace.</span><span class="sxs-lookup"><span data-stu-id="0e4d0-136">Because the database doesn't exist yet, it will be created before the migration is applied.</span></span>

> [!TIP]  
> <span data-ttu-id="0e4d0-137">Pokud provedete změny modelu, můžete použít `Add-Migration` příkaz scaffold novou migraci provést odpovídající schématu změn v databázi.</span><span class="sxs-lookup"><span data-stu-id="0e4d0-137">If you make changes to the model, you can use the `Add-Migration` command to scaffold a new migration to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="0e4d0-138">Po zaškrtnutí automaticky generovaný kód (a všechny požadované změny), můžete použít `Update-Database` příkaz změny se projeví do databáze.</span><span class="sxs-lookup"><span data-stu-id="0e4d0-138">Once you have checked the scaffolded code (and made any required changes), you can use the `Update-Database` command to apply the changes to the database.</span></span>
>
> <span data-ttu-id="0e4d0-139">Používá EF `__EFMigrationsHistory` tabulky v databázi ke sledování migrace, které již byly implementovány do databáze.</span><span class="sxs-lookup"><span data-stu-id="0e4d0-139">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="0e4d0-140">Použití modelu</span><span class="sxs-lookup"><span data-stu-id="0e4d0-140">Use the model</span></span>

<span data-ttu-id="0e4d0-141">Nyní můžete model přístup k datům.</span><span class="sxs-lookup"><span data-stu-id="0e4d0-141">You can now use the model to perform data access.</span></span>

* <span data-ttu-id="0e4d0-142">Otevřít *Program.cs*</span><span class="sxs-lookup"><span data-stu-id="0e4d0-142">Open *Program.cs*</span></span>

* <span data-ttu-id="0e4d0-143">Obsah souboru nahraďte následujícím kódem</span><span class="sxs-lookup"><span data-stu-id="0e4d0-143">Replace the contents of the file with the following code</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Program.cs)]

* <span data-ttu-id="0e4d0-144">**Ladit > Spustit bez ladění**</span><span class="sxs-lookup"><span data-stu-id="0e4d0-144">**Debug > Start Without Debugging**</span></span>

  <span data-ttu-id="0e4d0-145">Uvidíte, že jeden blogu se uloží do databáze a pak se podrobnosti o všech blogy vytisknou na konzole.</span><span class="sxs-lookup"><span data-stu-id="0e4d0-145">You see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

  ![obrázek](_static/output-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="0e4d0-147">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="0e4d0-147">Additional Resources</span></span>

* [<span data-ttu-id="0e4d0-148">EF Core na rozhraní .NET Framework s existující databáze</span><span class="sxs-lookup"><span data-stu-id="0e4d0-148">EF Core on .NET Framework with an existing database</span></span>](xref:core/get-started/full-dotnet/existing-db)
* <span data-ttu-id="0e4d0-149">[EF Core na .NET Core s novou databázi - SQLite](xref:core/get-started/netcore/new-db-sqlite) – kurz EF konzole pro různé platformy.</span><span class="sxs-lookup"><span data-stu-id="0e4d0-149">[EF Core on .NET Core with a new database - SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
