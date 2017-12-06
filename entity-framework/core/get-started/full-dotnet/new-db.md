---
title: "Začínáme v rozhraní .NET Framework – nové databáze - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 52b69727-ded9-4a7b-b8d5-73f3acfbbad3
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/new-db
ms.openlocfilehash: bd7054c6834ae11bfdc352d63654e4304771e432
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a><span data-ttu-id="4b18c-102">Začínáme s EF základní na rozhraní .NET Framework s novou databázi</span><span class="sxs-lookup"><span data-stu-id="4b18c-102">Getting started with EF Core on .NET Framework with a New Database</span></span>

<span data-ttu-id="4b18c-103">V tomto návodu vytvoříte konzolovou aplikaci, která provádí základní data přístup ověřit v databázi Microsoft SQL Server pomocí rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="4b18c-103">In this walkthrough, you will build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="4b18c-104">Migrace použije k vytvoření databáze z modelu.</span><span class="sxs-lookup"><span data-stu-id="4b18c-104">You will use migrations to create the database from your model.</span></span>

> [!TIP]  
> <span data-ttu-id="4b18c-105">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="4b18c-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4b18c-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4b18c-106">Prerequisites</span></span>

<span data-ttu-id="4b18c-107">Následující požadované součásti jsou nutné k dokončení tohoto názorného postupu:</span><span class="sxs-lookup"><span data-stu-id="4b18c-107">The following prerequisites are needed to complete this walkthrough:</span></span>

* [<span data-ttu-id="4b18c-108">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="4b18c-108">Visual Studio 2017</span></span>](https://www.visualstudio.com/downloads/)

* [<span data-ttu-id="4b18c-109">Nejnovější verze Správce balíčků NuGet</span><span class="sxs-lookup"><span data-stu-id="4b18c-109">Latest version of NuGet Package Manager</span></span>](https://dist.nuget.org/index.html)

* [<span data-ttu-id="4b18c-110">Nejnovější verzi prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="4b18c-110">Latest version of Windows PowerShell</span></span>](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

## <a name="create-a-new-project"></a><span data-ttu-id="4b18c-111">Vytvoření nového projektu</span><span class="sxs-lookup"><span data-stu-id="4b18c-111">Create a new project</span></span>

* <span data-ttu-id="4b18c-112">Otevřete Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4b18c-112">Open Visual Studio</span></span>

* <span data-ttu-id="4b18c-113">Soubor > Nový > projekt...</span><span class="sxs-lookup"><span data-stu-id="4b18c-113">File > New > Project...</span></span>

* <span data-ttu-id="4b18c-114">V levé nabídce vyberte šablony > Visual C# > klasické ploše systému Windows</span><span class="sxs-lookup"><span data-stu-id="4b18c-114">From the left menu select Templates > Visual C# > Windows Classic Desktop</span></span>

* <span data-ttu-id="4b18c-115">Vyberte **konzolovou aplikaci (rozhraní .NET Framework)** šablona projektu</span><span class="sxs-lookup"><span data-stu-id="4b18c-115">Select the **Console App (.NET Framework)** project template</span></span>

* <span data-ttu-id="4b18c-116">Ujistěte se, kterou cílíte **rozhraní .NET Framework 4.5.1** nebo novější</span><span class="sxs-lookup"><span data-stu-id="4b18c-116">Ensure you are targeting **.NET Framework 4.5.1** or later</span></span>

* <span data-ttu-id="4b18c-117">Pojmenujte projekt a klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="4b18c-117">Give the project a name and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="4b18c-118">Nainstalujte rozhraní Entity Framework</span><span class="sxs-lookup"><span data-stu-id="4b18c-118">Install Entity Framework</span></span>

<span data-ttu-id="4b18c-119">Abyste mohli používat EF jádra, nainstalujte balíček pro následující zprostředkovatele databáze, kterou chcete zacílit.</span><span class="sxs-lookup"><span data-stu-id="4b18c-119">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="4b18c-120">Tento návod používá SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4b18c-120">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="4b18c-121">Seznam dostupných zprostředkovatelů naleznete v části [zprostředkovatelů databáze](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="4b18c-121">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="4b18c-122">Nástroje > Správce balíčků NuGet > Konzola správce balíčků</span><span class="sxs-lookup"><span data-stu-id="4b18c-122">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="4b18c-123">Spustit`Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="4b18c-123">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="4b18c-124">Dále v tomto návodu také použijeme některé nástroje Entity Framework pro správnou údržbu databáze.</span><span class="sxs-lookup"><span data-stu-id="4b18c-124">Later in this walkthrough we will also be using some Entity Framework Tools to maintain the database.</span></span> <span data-ttu-id="4b18c-125">Proto se nainstaluje balíček nástroje.</span><span class="sxs-lookup"><span data-stu-id="4b18c-125">So we will install the tools package as well.</span></span>

* <span data-ttu-id="4b18c-126">Spustit`Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="4b18c-126">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="create-your-model"></a><span data-ttu-id="4b18c-127">Vytvoření modelu</span><span class="sxs-lookup"><span data-stu-id="4b18c-127">Create your model</span></span>

<span data-ttu-id="4b18c-128">Nyní je čas k definování kontextu a entity tříd, které tvoří modelu.</span><span class="sxs-lookup"><span data-stu-id="4b18c-128">Now it's time to define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="4b18c-129">Projekt > přidejte třídu...</span><span class="sxs-lookup"><span data-stu-id="4b18c-129">Project > Add Class...</span></span>

* <span data-ttu-id="4b18c-130">Zadejte *Model.cs* jako název a klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="4b18c-130">Enter *Model.cs* as the name and click **OK**</span></span>

* <span data-ttu-id="4b18c-131">Obsah souboru nahraďte následujícím kódem</span><span class="sxs-lookup"><span data-stu-id="4b18c-131">Replace the contents of the file with the following code</span></span>

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Model.cs)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using System.Collections.Generic;

namespace EFGetStarted.ConsoleApp
{
    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;");
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Url { get; set; }

        public List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public Blog Blog { get; set; }
    }
}
```

> [!TIP]  
> <span data-ttu-id="4b18c-132">V reálné aplikaci byste put každá třída v samostatném souboru a chápat připojovací řetězec `App.Config` souboru a přečtěte si ho použití `ConfigurationManager`.</span><span class="sxs-lookup"><span data-stu-id="4b18c-132">In a real application you would put each class in a separate file and put the connection string in the `App.Config` file and read it out using `ConfigurationManager`.</span></span> <span data-ttu-id="4b18c-133">Z důvodu zjednodušení jsme jsou uvedení všechno, co v souboru jednoho kódu pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="4b18c-133">For the sake of simplicity, we are putting everything in a single code file for this tutorial.</span></span>

## <a name="create-your-database"></a><span data-ttu-id="4b18c-134">Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="4b18c-134">Create your database</span></span>

<span data-ttu-id="4b18c-135">Teď, když máte model, můžete k vytvoření databáze pro vás migrace.</span><span class="sxs-lookup"><span data-stu-id="4b18c-135">Now that you have a model, you can use migrations to create a database for you.</span></span>

* <span data-ttu-id="4b18c-136">Nástroje pro –> balíček NuGet Manager –> Konzola správce balíčků</span><span class="sxs-lookup"><span data-stu-id="4b18c-136">Tools –> NuGet Package Manager –> Package Manager Console</span></span>

* <span data-ttu-id="4b18c-137">Spustit `Add-Migration MyFirstMigration` chcete vygenerovat migraci k vytvoření počáteční sadu tabulek pro váš model.</span><span class="sxs-lookup"><span data-stu-id="4b18c-137">Run `Add-Migration MyFirstMigration` to scaffold a migration to create the initial set of tables for your model.</span></span>

* <span data-ttu-id="4b18c-138">Spustit `Update-Database` použít nové migrace do databáze.</span><span class="sxs-lookup"><span data-stu-id="4b18c-138">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="4b18c-139">Protože vaše databáze ještě neexistuje, vytvoří se vám před použitím migrace.</span><span class="sxs-lookup"><span data-stu-id="4b18c-139">Because your database doesn't exist yet, it will be created for you before the migration is applied.</span></span>

> [!TIP]  
> <span data-ttu-id="4b18c-140">Pokud provedete budoucí změny modelu, můžete použít `Add-Migration` příkaz chcete vygenerovat nový migraci, aby schéma odpovídající změn v databázi.</span><span class="sxs-lookup"><span data-stu-id="4b18c-140">If you make future changes to your model, you can use the `Add-Migration` command to scaffold a new migration to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="4b18c-141">Po zaškrtnutí automaticky generovaný kód (a všechny požadované změny provedené), můžete použít `Update-Database` příkaz k použití změn do databáze.</span><span class="sxs-lookup"><span data-stu-id="4b18c-141">Once you have checked the scaffolded code (and made any required changes), you can use the `Update-Database` command to apply the changes to the database.</span></span>
>
><span data-ttu-id="4b18c-142">Používá EF `__EFMigrationsHistory` tabulky v databázi ke sledování migrace, která již byla do databáze použít.</span><span class="sxs-lookup"><span data-stu-id="4b18c-142">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="4b18c-143">Použití modelu</span><span class="sxs-lookup"><span data-stu-id="4b18c-143">Use your model</span></span>

<span data-ttu-id="4b18c-144">Nyní můžete modelu provádí přístup k datům.</span><span class="sxs-lookup"><span data-stu-id="4b18c-144">You can now use your model to perform data access.</span></span>

* <span data-ttu-id="4b18c-145">Otevřete *Program.cs*</span><span class="sxs-lookup"><span data-stu-id="4b18c-145">Open *Program.cs*</span></span>

* <span data-ttu-id="4b18c-146">Obsah souboru nahraďte následujícím kódem</span><span class="sxs-lookup"><span data-stu-id="4b18c-146">Replace the contents of the file with the following code</span></span>

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Program.cs)] -->
``` csharp
using System;

namespace EFGetStarted.ConsoleApp
{
    class Program
    {
        static void Main(string[] args)
        {
            using (var db = new BloggingContext())
            {
                db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                var count = db.SaveChanges();
                Console.WriteLine("{0} records saved to database", count);

                Console.WriteLine();
                Console.WriteLine("All blogs in database:");
                foreach (var blog in db.Blogs)
                {
                    Console.WriteLine(" - {0}", blog.Url);
                }
            }
        }
    }
}
```

* <span data-ttu-id="4b18c-147">Ladění > Spustit bez ladění</span><span class="sxs-lookup"><span data-stu-id="4b18c-147">Debug > Start Without Debugging</span></span>

<span data-ttu-id="4b18c-148">Zobrazí se, že blogů se ukládá do databáze a pak budou vytištěny podrobnosti o všech blogy ke konzole.</span><span class="sxs-lookup"><span data-stu-id="4b18c-148">You will see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

![obrázek](_static/output-new-db.png)
