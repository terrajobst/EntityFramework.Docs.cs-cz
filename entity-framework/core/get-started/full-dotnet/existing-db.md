---
title: Začínáme v rozhraní .NET Framework – existující databáze – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: 39e77ab8c124df67458cc5fa6db2882b65943ebe
ms.sourcegitcommit: 4467032fd6ca223e5965b59912d74cf88a1dd77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/01/2018
ms.locfileid: "39388465"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a><span data-ttu-id="00aa3-102">Začínáme s EF Core na rozhraní .NET Framework s existující databáze</span><span class="sxs-lookup"><span data-stu-id="00aa3-102">Getting started with EF Core on .NET Framework with an Existing Database</span></span>

<span data-ttu-id="00aa3-103">V tomto návodu vytvoříte konzolovou aplikaci, která provádí základní přístup k datům pro databázi serveru Microsoft SQL Server používá nástroj Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="00aa3-103">In this walkthrough, you will build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="00aa3-104">Zpětná analýza použije k vytvoření Entity Framework model založený na existující databázi.</span><span class="sxs-lookup"><span data-stu-id="00aa3-104">You will use reverse engineering to create an Entity Framework model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="00aa3-105">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="00aa3-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="00aa3-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="00aa3-106">Prerequisites</span></span>

<span data-ttu-id="00aa3-107">Následující požadované součásti jsou potřeba k dokončení tohoto návodu:</span><span class="sxs-lookup"><span data-stu-id="00aa3-107">The following prerequisites are needed to complete this walkthrough:</span></span>

* <span data-ttu-id="00aa3-108">[Visual Studio 2017](https://www.visualstudio.com/downloads/) – minimální verze 15.3</span><span class="sxs-lookup"><span data-stu-id="00aa3-108">[Visual Studio 2017](https://www.visualstudio.com/downloads/) - at least version 15.3</span></span>

* [<span data-ttu-id="00aa3-109">Nejnovější verzi správce balíčků NuGet</span><span class="sxs-lookup"><span data-stu-id="00aa3-109">Latest version of NuGet Package Manager</span></span>](https://dist.nuget.org/index.html)

* [<span data-ttu-id="00aa3-110">Nejnovější verzi prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="00aa3-110">Latest version of Windows PowerShell</span></span>](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

* [<span data-ttu-id="00aa3-111">Blogovací databáze</span><span class="sxs-lookup"><span data-stu-id="00aa3-111">Blogging database</span></span>](#blogging-database)

### <a name="blogging-database"></a><span data-ttu-id="00aa3-112">Blogovací databáze</span><span class="sxs-lookup"><span data-stu-id="00aa3-112">Blogging database</span></span>

<span data-ttu-id="00aa3-113">Tento kurz používá **blogovací** databáze na instanci LocalDb jako existující databázi.</span><span class="sxs-lookup"><span data-stu-id="00aa3-113">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="00aa3-114">Pokud jste již vytvořili **blogovací** databázi jako součást další kurz, můžete přeskočit tyto kroky.</span><span class="sxs-lookup"><span data-stu-id="00aa3-114">If you have already created the **Blogging** database as part of another tutorial, you can skip these steps.</span></span>

* <span data-ttu-id="00aa3-115">Otevřít Visual Studio</span><span class="sxs-lookup"><span data-stu-id="00aa3-115">Open Visual Studio</span></span>

* <span data-ttu-id="00aa3-116">Nástroje > připojení k databázi...</span><span class="sxs-lookup"><span data-stu-id="00aa3-116">Tools > Connect to Database...</span></span>

* <span data-ttu-id="00aa3-117">Vyberte **Microsoft SQL Server** a klikněte na tlačítko **pokračovat**</span><span class="sxs-lookup"><span data-stu-id="00aa3-117">Select **Microsoft SQL Server** and click **Continue**</span></span>

* <span data-ttu-id="00aa3-118">Zadejte **(localdb) \mssqllocaldb** jako **název serveru**</span><span class="sxs-lookup"><span data-stu-id="00aa3-118">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>

* <span data-ttu-id="00aa3-119">Zadejte **hlavní** jako **název_databáze** a klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="00aa3-119">Enter **master** as the **Database Name** and click **OK**</span></span>

* <span data-ttu-id="00aa3-120">Hlavní databázi se nyní zobrazí v části **datová připojení** v **Průzkumníka serveru**</span><span class="sxs-lookup"><span data-stu-id="00aa3-120">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>

* <span data-ttu-id="00aa3-121">Klikněte pravým tlačítkem na databázi v **Průzkumníka serveru** a vyberte **nový dotaz**</span><span class="sxs-lookup"><span data-stu-id="00aa3-121">Right-click on the database in **Server Explorer** and select **New Query**</span></span>

* <span data-ttu-id="00aa3-122">Zkopírujte skript uvedený níže, do editoru dotazů</span><span class="sxs-lookup"><span data-stu-id="00aa3-122">Copy the script, listed below, into the query editor</span></span>

* <span data-ttu-id="00aa3-123">Klikněte pravým tlačítkem myši na editor dotazů a vyberte **spouštění**</span><span class="sxs-lookup"><span data-stu-id="00aa3-123">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="00aa3-124">Vytvoření nového projektu</span><span class="sxs-lookup"><span data-stu-id="00aa3-124">Create a new project</span></span>

* <span data-ttu-id="00aa3-125">Otevřít Visual Studio</span><span class="sxs-lookup"><span data-stu-id="00aa3-125">Open Visual Studio</span></span>

* <span data-ttu-id="00aa3-126">Soubor > Nový > projekt...</span><span class="sxs-lookup"><span data-stu-id="00aa3-126">File > New > Project...</span></span>

* <span data-ttu-id="00aa3-127">V levé nabídce vyberte šablony > Visual C# > Windows</span><span class="sxs-lookup"><span data-stu-id="00aa3-127">From the left menu select Templates > Visual C# > Windows</span></span>

* <span data-ttu-id="00aa3-128">Vyberte **konzolovou aplikaci** šablony projektu</span><span class="sxs-lookup"><span data-stu-id="00aa3-128">Select the **Console Application** project template</span></span>

* <span data-ttu-id="00aa3-129">Zkontrolujte cílovou **rozhraní .NET Framework 4.6.1** nebo novější</span><span class="sxs-lookup"><span data-stu-id="00aa3-129">Ensure you are targeting **.NET Framework 4.6.1** or later</span></span>

* <span data-ttu-id="00aa3-130">Pojmenujte projekt a klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="00aa3-130">Give the project a name and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="00aa3-131">Nainstalujte rozhraní Entity Framework</span><span class="sxs-lookup"><span data-stu-id="00aa3-131">Install Entity Framework</span></span>

<span data-ttu-id="00aa3-132">Použití EF Core, nainstalujte balíček pro poskytovatelů databáze, kterou chcete cílit.</span><span class="sxs-lookup"><span data-stu-id="00aa3-132">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="00aa3-133">Tento návod používá systém SQL Server.</span><span class="sxs-lookup"><span data-stu-id="00aa3-133">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="00aa3-134">Seznam dostupných zprostředkovatelů najdete v části [poskytovatelé databází](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="00aa3-134">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="00aa3-135">Nástroje > Správce balíčků NuGet > Konzola správce balíčků</span><span class="sxs-lookup"><span data-stu-id="00aa3-135">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="00aa3-136">Spustit `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="00aa3-136">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="00aa3-137">Povolit zpětnou analýzu z existující databáze potřebujeme příliš nainstalovat několik dalších balíčků.</span><span class="sxs-lookup"><span data-stu-id="00aa3-137">To enable reverse engineering from an existing database we need to install a couple of other packages too.</span></span>

* <span data-ttu-id="00aa3-138">Spustit `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="00aa3-138">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="00aa3-139">Provést zpětnou analýzu modelu</span><span class="sxs-lookup"><span data-stu-id="00aa3-139">Reverse engineer your model</span></span>

<span data-ttu-id="00aa3-140">Nyní je čas vytvořit EF model založený na existující databázi.</span><span class="sxs-lookup"><span data-stu-id="00aa3-140">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="00aa3-141">Balíček NuGet –> nástroje Správce –> Konzola správce balíčků</span><span class="sxs-lookup"><span data-stu-id="00aa3-141">Tools –> NuGet Package Manager –> Package Manager Console</span></span>

* <span data-ttu-id="00aa3-142">Spusťte následující příkaz pro vytvoření modelu z existující databáze</span><span class="sxs-lookup"><span data-stu-id="00aa3-142">Run the following command to create a model from the existing database</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="00aa3-143">Zpětná analýza procesu vytvoření tříd entit a odvozené kontext na základě schématu existující databázi.</span><span class="sxs-lookup"><span data-stu-id="00aa3-143">The reverse engineer process created entity classes and a derived context based on the schema of the existing database.</span></span> <span data-ttu-id="00aa3-144">Entity třídy jsou jednoduché C# objekty, jež reprezentují data budete dotazování a ukládání.</span><span class="sxs-lookup"><span data-stu-id="00aa3-144">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span>

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

<span data-ttu-id="00aa3-145">Kontext představuje relaci s databází a umožňuje dotazování a uložit instancí tříd entit.</span><span class="sxs-lookup"><span data-stu-id="00aa3-145">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

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

## <a name="use-your-model"></a><span data-ttu-id="00aa3-146">Použití modelu</span><span class="sxs-lookup"><span data-stu-id="00aa3-146">Use your model</span></span>

<span data-ttu-id="00aa3-147">Teď můžete svůj model přístup k datům.</span><span class="sxs-lookup"><span data-stu-id="00aa3-147">You can now use your model to perform data access.</span></span>

* <span data-ttu-id="00aa3-148">Otevřít *Program.cs*</span><span class="sxs-lookup"><span data-stu-id="00aa3-148">Open *Program.cs*</span></span>

* <span data-ttu-id="00aa3-149">Obsah souboru nahraďte následujícím kódem</span><span class="sxs-lookup"><span data-stu-id="00aa3-149">Replace the contents of the file with the following code</span></span>

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

* <span data-ttu-id="00aa3-150">Ladit > Spustit bez ladění</span><span class="sxs-lookup"><span data-stu-id="00aa3-150">Debug > Start Without Debugging</span></span>

<span data-ttu-id="00aa3-151">Uvidíte, že blogů se uloží do databáze a pak se podrobnosti o všech blogy vytisknou na konzole.</span><span class="sxs-lookup"><span data-stu-id="00aa3-151">You will see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

![obrázek](_static/output-existing-db.png)
