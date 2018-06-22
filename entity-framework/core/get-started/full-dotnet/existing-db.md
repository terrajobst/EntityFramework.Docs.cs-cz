---
title: Začínáme v rozhraní .NET Framework – stávající databázi - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: 3cd69109e3cf8dbc103f9eea6e2553df17f29a98
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812622"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a><span data-ttu-id="53e41-102">Začínáme s EF základní na rozhraní .NET Framework s existující databázi</span><span class="sxs-lookup"><span data-stu-id="53e41-102">Getting started with EF Core on .NET Framework with an Existing Database</span></span>

<span data-ttu-id="53e41-103">V tomto návodu vytvoříte konzolovou aplikaci, která provádí základní data přístup ověřit v databázi Microsoft SQL Server pomocí rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="53e41-103">In this walkthrough, you will build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="53e41-104">Zpětná analýza použije pro vytvoření modelu Entity Framework založené na existující databázi.</span><span class="sxs-lookup"><span data-stu-id="53e41-104">You will use reverse engineering to create an Entity Framework model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="53e41-105">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="53e41-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="53e41-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="53e41-106">Prerequisites</span></span>

<span data-ttu-id="53e41-107">Následující požadované součásti jsou nutné k dokončení tohoto názorného postupu:</span><span class="sxs-lookup"><span data-stu-id="53e41-107">The following prerequisites are needed to complete this walkthrough:</span></span>

* [<span data-ttu-id="53e41-108">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="53e41-108">Visual Studio 2017</span></span>](https://www.visualstudio.com/downloads/)

* [<span data-ttu-id="53e41-109">Nejnovější verze Správce balíčků NuGet</span><span class="sxs-lookup"><span data-stu-id="53e41-109">Latest version of NuGet Package Manager</span></span>](https://dist.nuget.org/index.html)

* [<span data-ttu-id="53e41-110">Nejnovější verzi prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="53e41-110">Latest version of Windows PowerShell</span></span>](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

* [<span data-ttu-id="53e41-111">Databáze blogu</span><span class="sxs-lookup"><span data-stu-id="53e41-111">Blogging database</span></span>](#blogging-database)

### <a name="blogging-database"></a><span data-ttu-id="53e41-112">Databáze blogu</span><span class="sxs-lookup"><span data-stu-id="53e41-112">Blogging database</span></span>

<span data-ttu-id="53e41-113">Tento kurz používá **blogu** databáze ve vaší instanci LocalDb jako existující databáze.</span><span class="sxs-lookup"><span data-stu-id="53e41-113">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="53e41-114">Pokud jste již vytvořili **blogu** databázi jako součást jiného kurzu, můžete přeskočit tyto kroky.</span><span class="sxs-lookup"><span data-stu-id="53e41-114">If you have already created the **Blogging** database as part of another tutorial, you can skip these steps.</span></span>

* <span data-ttu-id="53e41-115">Otevřete Visual Studio</span><span class="sxs-lookup"><span data-stu-id="53e41-115">Open Visual Studio</span></span>

* <span data-ttu-id="53e41-116">Nástroje > připojit k databázi...</span><span class="sxs-lookup"><span data-stu-id="53e41-116">Tools > Connect to Database...</span></span>

* <span data-ttu-id="53e41-117">Vyberte **Microsoft SQL Server** a klikněte na tlačítko **pokračovat**</span><span class="sxs-lookup"><span data-stu-id="53e41-117">Select **Microsoft SQL Server** and click **Continue**</span></span>

* <span data-ttu-id="53e41-118">Zadejte **\mssqllocaldb (localdb)** jako **název serveru**</span><span class="sxs-lookup"><span data-stu-id="53e41-118">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>

* <span data-ttu-id="53e41-119">Zadejte **hlavní** jako **název databáze** a klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="53e41-119">Enter **master** as the **Database Name** and click **OK**</span></span>

* <span data-ttu-id="53e41-120">Hlavní databázi se nyní zobrazí v části **připojení dat** v **Průzkumníka serveru**</span><span class="sxs-lookup"><span data-stu-id="53e41-120">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>

* <span data-ttu-id="53e41-121">Klikněte pravým tlačítkem na databázi v **Průzkumníka serveru** a vyberte **nový dotaz**</span><span class="sxs-lookup"><span data-stu-id="53e41-121">Right-click on the database in **Server Explorer** and select **New Query**</span></span>

* <span data-ttu-id="53e41-122">Zkopírujte skript, uvedené níže, do editoru dotazů</span><span class="sxs-lookup"><span data-stu-id="53e41-122">Copy the script, listed below, into the query editor</span></span>

* <span data-ttu-id="53e41-123">Klikněte pravým tlačítkem na editoru dotazů a vyberte **spouštění**</span><span class="sxs-lookup"><span data-stu-id="53e41-123">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="53e41-124">Vytvoření nového projektu</span><span class="sxs-lookup"><span data-stu-id="53e41-124">Create a new project</span></span>

* <span data-ttu-id="53e41-125">Otevřete Visual Studio</span><span class="sxs-lookup"><span data-stu-id="53e41-125">Open Visual Studio</span></span>

* <span data-ttu-id="53e41-126">Soubor > Nový > projekt...</span><span class="sxs-lookup"><span data-stu-id="53e41-126">File > New > Project...</span></span>

* <span data-ttu-id="53e41-127">V levé nabídce vyberte šablony > Visual C# > Windows</span><span class="sxs-lookup"><span data-stu-id="53e41-127">From the left menu select Templates > Visual C# > Windows</span></span>

* <span data-ttu-id="53e41-128">Vyberte **konzolové aplikace** šablona projektu</span><span class="sxs-lookup"><span data-stu-id="53e41-128">Select the **Console Application** project template</span></span>

* <span data-ttu-id="53e41-129">Ujistěte se, kterou cílíte **rozhraní .NET Framework 4.5.1** nebo novější</span><span class="sxs-lookup"><span data-stu-id="53e41-129">Ensure you are targeting **.NET Framework 4.5.1** or later</span></span>

* <span data-ttu-id="53e41-130">Pojmenujte projekt a klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="53e41-130">Give the project a name and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="53e41-131">Nainstalujte rozhraní Entity Framework</span><span class="sxs-lookup"><span data-stu-id="53e41-131">Install Entity Framework</span></span>

<span data-ttu-id="53e41-132">Abyste mohli používat EF jádra, nainstalujte balíček pro následující zprostředkovatele databáze, kterou chcete zacílit.</span><span class="sxs-lookup"><span data-stu-id="53e41-132">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="53e41-133">Tento návod používá SQL Server.</span><span class="sxs-lookup"><span data-stu-id="53e41-133">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="53e41-134">Seznam dostupných zprostředkovatelů naleznete v části [zprostředkovatelů databáze](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="53e41-134">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="53e41-135">Nástroje > Správce balíčků NuGet > Konzola správce balíčků</span><span class="sxs-lookup"><span data-stu-id="53e41-135">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="53e41-136">Spustit `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="53e41-136">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="53e41-137">Povolit zpětnou analýzu z existující databáze je potřeba nainstalovat několik dalších balíčků příliš.</span><span class="sxs-lookup"><span data-stu-id="53e41-137">To enable reverse engineering from an existing database we need to install a couple of other packages too.</span></span>

* <span data-ttu-id="53e41-138">Spustit `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="53e41-138">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="53e41-139">Provést zpětnou analýzu modelu</span><span class="sxs-lookup"><span data-stu-id="53e41-139">Reverse engineer your model</span></span>

<span data-ttu-id="53e41-140">Nyní je čas vytvořit model EF založený na existující databázi.</span><span class="sxs-lookup"><span data-stu-id="53e41-140">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="53e41-141">Nástroje pro –> balíček NuGet Manager –> Konzola správce balíčků</span><span class="sxs-lookup"><span data-stu-id="53e41-141">Tools –> NuGet Package Manager –> Package Manager Console</span></span>

* <span data-ttu-id="53e41-142">Spusťte následující příkaz pro vytvoření modelu z existující databáze</span><span class="sxs-lookup"><span data-stu-id="53e41-142">Run the following command to create a model from the existing database</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="53e41-143">Proces zpětné analýzy vytvořili tříd entit a odvozené kontext na základě schématu existující databáze.</span><span class="sxs-lookup"><span data-stu-id="53e41-143">The reverse engineer process created entity classes and a derived context based on the schema of the existing database.</span></span> <span data-ttu-id="53e41-144">Třídy entity jsou jednoduché C# objekty, které představují data, budete mít dotazování a ukládání.</span><span class="sxs-lookup"><span data-stu-id="53e41-144">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span>

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Blog.cs)] -->
``` csharp
using System;
using System.Collections.Generic;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    public partial class Blog
    {
        public Blog()
        {
            Post = new HashSet<Post>();
        }

        public int BlogId { get; set; }
        public string Url { get; set; }

        public virtual ICollection<Post> Post { get; set; }
    }
}
```

<span data-ttu-id="53e41-145">Kontext představuje relaci s databází a umožňuje dotazování a uložit instance tříd entity.</span><span class="sxs-lookup"><span data-stu-id="53e41-145">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/BloggingContext.cs)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Metadata;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    public partial class BloggingContext : DbContext
    {
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
        }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<Blog>(entity =>
            {
                entity.Property(e => e.Url).IsRequired();
            });

            modelBuilder.Entity<Post>(entity =>
            {
                entity.HasOne(d => d.Blog)
                    .WithMany(p => p.Post)
                    .HasForeignKey(d => d.BlogId);
            });
        }

        public virtual DbSet<Blog> Blog { get; set; }
        public virtual DbSet<Post> Post { get; set; }
    }
}
```

## <a name="use-your-model"></a><span data-ttu-id="53e41-146">Použití modelu</span><span class="sxs-lookup"><span data-stu-id="53e41-146">Use your model</span></span>

<span data-ttu-id="53e41-147">Nyní můžete modelu provádí přístup k datům.</span><span class="sxs-lookup"><span data-stu-id="53e41-147">You can now use your model to perform data access.</span></span>

* <span data-ttu-id="53e41-148">Otevřete *Program.cs*</span><span class="sxs-lookup"><span data-stu-id="53e41-148">Open *Program.cs*</span></span>

* <span data-ttu-id="53e41-149">Obsah souboru nahraďte následujícím kódem</span><span class="sxs-lookup"><span data-stu-id="53e41-149">Replace the contents of the file with the following code</span></span>

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Program.cs)] -->
``` csharp
using System;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    class Program
    {
        static void Main(string[] args)
        {
            using (var db = new BloggingContext())
            {
                db.Blog.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                var count = db.SaveChanges();
                Console.WriteLine("{0} records saved to database", count);

                Console.WriteLine();
                Console.WriteLine("All blogs in database:");
                foreach (var blog in db.Blog)
                {
                    Console.WriteLine(" - {0}", blog.Url);
                }
            }
        }
    }
}
```

* <span data-ttu-id="53e41-150">Ladění > Spustit bez ladění</span><span class="sxs-lookup"><span data-stu-id="53e41-150">Debug > Start Without Debugging</span></span>

<span data-ttu-id="53e41-151">Zobrazí se, že blogů se ukládá do databáze a pak budou vytištěny podrobnosti o všech blogy ke konzole.</span><span class="sxs-lookup"><span data-stu-id="53e41-151">You will see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

![obrázek](_static/output-existing-db.png)
