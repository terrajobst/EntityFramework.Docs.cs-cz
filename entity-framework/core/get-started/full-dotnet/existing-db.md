---
title: Začínáme v rozhraní .NET Framework – existující databáze – EF Core
author: rowanmiller
ms.author: divega
ms.date: 08/06/2018
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: d5c548927b736199c7d6fddc9c74139ca5f6614e
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614412"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a><span data-ttu-id="79298-102">Začínáme s EF Core na rozhraní .NET Framework s existující databáze</span><span class="sxs-lookup"><span data-stu-id="79298-102">Getting started with EF Core on .NET Framework with an Existing Database</span></span>

<span data-ttu-id="79298-103">V tomto kurzu vytvoříte konzolovou aplikaci, která provádí základní přístup k datům pro databázi serveru Microsoft SQL Server používá nástroj Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="79298-103">In this tutorial, you build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="79298-104">Vytvoření modelu Entity Framework ve zpětné analýze existující databázi.</span><span class="sxs-lookup"><span data-stu-id="79298-104">You create an Entity Framework model by reverse engineering an existing database.</span></span>

<span data-ttu-id="79298-105">[Zobrazit ukázky v tomto článku na Githubu](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb).</span><span class="sxs-lookup"><span data-stu-id="79298-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="79298-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="79298-106">Prerequisites</span></span>

* [<span data-ttu-id="79298-107">Visual Studio 2017 verze 15.7 nebo novější</span><span class="sxs-lookup"><span data-stu-id="79298-107">Visual Studio 2017 version 15.7 or later</span></span>](https://www.visualstudio.com/downloads/)

## <a name="create-blogging-database"></a><span data-ttu-id="79298-108">Vytvoření databáze blogovací</span><span class="sxs-lookup"><span data-stu-id="79298-108">Create Blogging database</span></span>

<span data-ttu-id="79298-109">Tento kurz používá **blogovací** databáze na instanci LocalDb jako existující databázi.</span><span class="sxs-lookup"><span data-stu-id="79298-109">This tutorial uses a **Blogging** database on the LocalDb instance as the existing database.</span></span> <span data-ttu-id="79298-110">Pokud jste již vytvořili **blogovací** databáze jako součást další kurz, přeskočit tyto kroky.</span><span class="sxs-lookup"><span data-stu-id="79298-110">If you have already created the **Blogging** database as part of another tutorial, skip these steps.</span></span>

* <span data-ttu-id="79298-111">Otevřít Visual Studio</span><span class="sxs-lookup"><span data-stu-id="79298-111">Open Visual Studio</span></span>

* <span data-ttu-id="79298-112">**Nástroje > připojení k databázi...**</span><span class="sxs-lookup"><span data-stu-id="79298-112">**Tools > Connect to Database...**</span></span>

* <span data-ttu-id="79298-113">Vyberte **Microsoft SQL Server** a klikněte na tlačítko **pokračovat**</span><span class="sxs-lookup"><span data-stu-id="79298-113">Select **Microsoft SQL Server** and click **Continue**</span></span>

* <span data-ttu-id="79298-114">Zadejte **(localdb) \mssqllocaldb** jako **název serveru**</span><span class="sxs-lookup"><span data-stu-id="79298-114">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>

* <span data-ttu-id="79298-115">Zadejte **hlavní** jako **název_databáze** a klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="79298-115">Enter **master** as the **Database Name** and click **OK**</span></span>

* <span data-ttu-id="79298-116">Hlavní databázi se nyní zobrazí v části **datová připojení** v **Průzkumníka serveru**</span><span class="sxs-lookup"><span data-stu-id="79298-116">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>

* <span data-ttu-id="79298-117">Klikněte pravým tlačítkem na databázi v **Průzkumníka serveru** a vyberte **nový dotaz**</span><span class="sxs-lookup"><span data-stu-id="79298-117">Right-click on the database in **Server Explorer** and select **New Query**</span></span>

* <span data-ttu-id="79298-118">Zkopírujte skript níže do editoru dotazů</span><span class="sxs-lookup"><span data-stu-id="79298-118">Copy the script listed below into the query editor</span></span>

* <span data-ttu-id="79298-119">Klikněte pravým tlačítkem myši na editor dotazů a vyberte **spouštění**</span><span class="sxs-lookup"><span data-stu-id="79298-119">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="79298-120">Vytvoření nového projektu</span><span class="sxs-lookup"><span data-stu-id="79298-120">Create a new project</span></span>

* <span data-ttu-id="79298-121">Otevřít Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="79298-121">Open Visual Studio 2017</span></span>

* <span data-ttu-id="79298-122">**Soubor > Nový > projekt...**</span><span class="sxs-lookup"><span data-stu-id="79298-122">**File > New > Project...**</span></span>

* <span data-ttu-id="79298-123">V levé nabídce vyberte **nainstalováno > Visual C# > Windows Desktop**</span><span class="sxs-lookup"><span data-stu-id="79298-123">From the left menu select **Installed > Visual C# > Windows Desktop**</span></span>

* <span data-ttu-id="79298-124">Vyberte **Konzolová aplikace (.NET Framework)** šablony projektu</span><span class="sxs-lookup"><span data-stu-id="79298-124">Select the **Console App (.NET Framework)** project template</span></span>

* <span data-ttu-id="79298-125">Ujistěte se, že projekt cílí **rozhraní .NET Framework 4.6.1** nebo novější</span><span class="sxs-lookup"><span data-stu-id="79298-125">Make sure that the project targets **.NET Framework 4.6.1** or later</span></span>

* <span data-ttu-id="79298-126">Pojmenujte projekt *ConsoleApp.ExistingDb* a klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="79298-126">Name the project *ConsoleApp.ExistingDb* and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="79298-127">Nainstalujte rozhraní Entity Framework</span><span class="sxs-lookup"><span data-stu-id="79298-127">Install Entity Framework</span></span>

<span data-ttu-id="79298-128">Použití EF Core, nainstalujte balíček pro poskytovatelů databáze, kterou chcete cílit.</span><span class="sxs-lookup"><span data-stu-id="79298-128">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="79298-129">Tento kurz používá systém SQL Server.</span><span class="sxs-lookup"><span data-stu-id="79298-129">This tutorial uses SQL Server.</span></span> <span data-ttu-id="79298-130">Seznam dostupných zprostředkovatelů najdete v části [poskytovatelé databází](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="79298-130">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="79298-131">**Nástroje > Správce balíčků NuGet > Konzola správce balíčků**</span><span class="sxs-lookup"><span data-stu-id="79298-131">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="79298-132">Spustit `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="79298-132">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="79298-133">V dalším kroku použijete některé Entity Framework Tools provést zpětnou analýzu databáze.</span><span class="sxs-lookup"><span data-stu-id="79298-133">In the next step, you use some Entity Framework Tools to reverse engineer the database.</span></span> <span data-ttu-id="79298-134">Balíček nástroje tak instalaci.</span><span class="sxs-lookup"><span data-stu-id="79298-134">So install the tools package as well.</span></span>

* <span data-ttu-id="79298-135">Spustit `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="79298-135">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="reverse-engineer-the-model"></a><span data-ttu-id="79298-136">Zpětná analýza modelu</span><span class="sxs-lookup"><span data-stu-id="79298-136">Reverse engineer the model</span></span>

<span data-ttu-id="79298-137">Nyní je čas vytvořit EF model založený na existující databázi.</span><span class="sxs-lookup"><span data-stu-id="79298-137">Now it's time to create the EF model based on an existing database.</span></span>

* <span data-ttu-id="79298-138">**Balíček NuGet –> nástroje Správce –> Konzola správce balíčků**</span><span class="sxs-lookup"><span data-stu-id="79298-138">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>

* <span data-ttu-id="79298-139">Spusťte následující příkaz pro vytvoření modelu z existující databáze</span><span class="sxs-lookup"><span data-stu-id="79298-139">Run the following command to create a model from the existing database</span></span>

  ``` powershell
  Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
  ```

> [!TIP]  
> <span data-ttu-id="79298-140">Můžete zadat tabulkami k vygenerování entity pro tak, že přidáte `-Tables` argument výše uvedeného příkazu.</span><span class="sxs-lookup"><span data-stu-id="79298-140">You can specify the tables to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="79298-141">Například `-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="79298-141">For example, `-Tables Blog,Post`.</span></span>

<span data-ttu-id="79298-142">Zpětná analýza procesu vytvoření tříd entit (`Blog` a `Post`) a odvozené kontextu (`BloggingContext`) na základě schématu existující databázi.</span><span class="sxs-lookup"><span data-stu-id="79298-142">The reverse engineer process created entity classes (`Blog` and `Post`) and a derived context (`BloggingContext`) based on the schema of the existing database.</span></span>

<span data-ttu-id="79298-143">Entity třídy jsou jednoduché C# objekty, jež reprezentují data budete dotazování a ukládání.</span><span class="sxs-lookup"><span data-stu-id="79298-143">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span> <span data-ttu-id="79298-144">Tady jsou `Blog` a `Post` tříd entit:</span><span class="sxs-lookup"><span data-stu-id="79298-144">Here are the `Blog` and `Post` entity classes:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Post.cs)]

> [!TIP]  
> <span data-ttu-id="79298-145">Povolit opožděné načtení, můžete nastavit vlastnosti navigace `virtual` (Blog.Post a Post.Blog).</span><span class="sxs-lookup"><span data-stu-id="79298-145">To enable lazy loading, you can make navigation properties `virtual` (Blog.Post and Post.Blog).</span></span>

<span data-ttu-id="79298-146">Kontext představuje relaci s databází.</span><span class="sxs-lookup"><span data-stu-id="79298-146">The context represents a session with the database.</span></span> <span data-ttu-id="79298-147">Obsahuje metody, které můžete použít k dotazování a uložit instancí tříd entit.</span><span class="sxs-lookup"><span data-stu-id="79298-147">It has methods that you can use to query and save instances of the entity classes.</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/BloggingContext.cs)]

## <a name="use-the-model"></a><span data-ttu-id="79298-148">Použití modelu</span><span class="sxs-lookup"><span data-stu-id="79298-148">Use the model</span></span>

<span data-ttu-id="79298-149">Nyní můžete model přístup k datům.</span><span class="sxs-lookup"><span data-stu-id="79298-149">You can now use the model to perform data access.</span></span>

* <span data-ttu-id="79298-150">Otevřít *Program.cs*</span><span class="sxs-lookup"><span data-stu-id="79298-150">Open *Program.cs*</span></span>

* <span data-ttu-id="79298-151">Obsah souboru nahraďte následujícím kódem</span><span class="sxs-lookup"><span data-stu-id="79298-151">Replace the contents of the file with the following code</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Program.cs)] 

* <span data-ttu-id="79298-152">Ladit > Spustit bez ladění</span><span class="sxs-lookup"><span data-stu-id="79298-152">Debug > Start Without Debugging</span></span>

  <span data-ttu-id="79298-153">Uvidíte, že jeden blogu se uloží do databáze a pak se podrobnosti o všech blogy vytisknou na konzole.</span><span class="sxs-lookup"><span data-stu-id="79298-153">You see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

  ![obrázek](_static/output-existing-db.png)

## <a name="additional-resources"></a><span data-ttu-id="79298-155">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="79298-155">Additional Resources</span></span>

* [<span data-ttu-id="79298-156">EF Core na rozhraní .NET Framework s novou databázi</span><span class="sxs-lookup"><span data-stu-id="79298-156">EF Core on .NET Framework with a new database</span></span>](xref:core/get-started/full-dotnet/new-db)
* <span data-ttu-id="79298-157">[EF Core na .NET Core s novou databázi - SQLite](xref:core/get-started/netcore/new-db-sqlite) – kurz EF konzole pro různé platformy.</span><span class="sxs-lookup"><span data-stu-id="79298-157">[EF Core on .NET Core with a new database - SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
