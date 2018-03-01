---
title: "Rychlý přehled - EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
ms.technology: entity-framework-core
uid: core/index
ms.openlocfilehash: c76b4cd318151b502c549fa0a82800f9987ed94c
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="entity-framework-core-quick-overview"></a><span data-ttu-id="14e16-102">Rychlý přehled základní Entity Framework</span><span class="sxs-lookup"><span data-stu-id="14e16-102">Entity Framework Core Quick Overview</span></span>

<span data-ttu-id="14e16-103">Základní Entity Framework (EF) je lightweight rozšiřitelný, a přístup technologie a platformy verzi oblíbených datům Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="14e16-103">Entity Framework (EF) Core is a lightweight, extensible, and cross-platform version of the popular Entity Framework data access technology.</span></span>

<span data-ttu-id="14e16-104">Základní EF je objekt relační mapper (O/RM), která umožňuje vývojářům rozhraní .NET pro práci s objekty .NET pomocí databáze.</span><span class="sxs-lookup"><span data-stu-id="14e16-104">EF Core is an object-relational mapper (O/RM) that enables .NET developers to work with a database using .NET objects.</span></span> <span data-ttu-id="14e16-105">Díky tomu není nutná většina kódu pro přístup k datům, který vývojáři obvykle musí vytvářet.</span><span class="sxs-lookup"><span data-stu-id="14e16-105">It eliminates the need for most of the data-access code that developers usually need to write.</span></span> <span data-ttu-id="14e16-106">Jádro EF podporuje mnoho databázové stroje najdete v tématu [zprostředkovatelů databáze](providers/index.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="14e16-106">EF Core supports many database engines, see [Database Providers](providers/index.md) for details.</span></span>

<span data-ttu-id="14e16-107">Pokud chcete další informace o psaní kódu, bychom doporučili jeden z našich [Začínáme](get-started/index.md) příručky, které vám pomůžou začít s EF jádra.</span><span class="sxs-lookup"><span data-stu-id="14e16-107">If you like to learn by writing code, we'd recommend one of our [Getting Started](get-started/index.md) guides to get you started with EF Core.</span></span>

## <a name="what-is-new-in-ef-core"></a><span data-ttu-id="14e16-108">Co je nového v EF jádra</span><span class="sxs-lookup"><span data-stu-id="14e16-108">What is new in EF Core</span></span>

<span data-ttu-id="14e16-109">Pokud jste se seznámili s EF jádra a chcete přejít rovnou do podrobnosti o nejnovějších verzích:</span><span class="sxs-lookup"><span data-stu-id="14e16-109">If you are familiar with EF Core and want to jump straight into the details of the latest releases:</span></span>

- <span data-ttu-id="14e16-110">**[Co je nového v EF základní 2.1 (momentálně ve verzi preview)](xref:core/what-is-new/ef-core-2.1)**</span><span class="sxs-lookup"><span data-stu-id="14e16-110">**[What is new in EF Core 2.1 (currently in preview)](xref:core/what-is-new/ef-core-2.1)**</span></span>
- <span data-ttu-id="14e16-111">**[Co je nového v EF základní 2.0 (na nejnovější prodejní verzi)](xref:core/what-is-new/ef-core-2.0)**</span><span class="sxs-lookup"><span data-stu-id="14e16-111">**[What is new in EF Core 2.0 (the latest released version)](xref:core/what-is-new/ef-core-2.0)**</span></span>
- <span data-ttu-id="14e16-112">**[Upgrade existující aplikace, aby EF základní 2.0](xref:core/miscellaneous/1x-2x-upgrade)**</span><span class="sxs-lookup"><span data-stu-id="14e16-112">**[Upgrading existing applications to EF Core 2.0](xref:core/miscellaneous/1x-2x-upgrade)**</span></span>


## <a name="get-entity-framework-core"></a><span data-ttu-id="14e16-113">Získat Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="14e16-113">Get Entity Framework Core</span></span>

<span data-ttu-id="14e16-114">[Nainstalujte balíček NuGet](https://docs.nuget.org/ndocs/quickstart/use-a-package) k poskytovateli databáze, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="14e16-114">[Install the NuGet package](https://docs.nuget.org/ndocs/quickstart/use-a-package) for the database provider you want to use.</span></span> <span data-ttu-id="14e16-115">Například</span><span class="sxs-lookup"><span data-stu-id="14e16-115">E.g.</span></span> <span data-ttu-id="14e16-116">Abyste mohli nainstalovat poskytovatele serveru SQL Server v vývoj pro různé platformy pomocí `dotnet` nástroj na příkazovém řádku:</span><span class="sxs-lookup"><span data-stu-id="14e16-116">to install the SQL Server provider in cross-platform development using `dotnet` tool in the command line:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="14e16-117">Nebo v sadě Visual Studio pomocí konzoly Správce balíčků:</span><span class="sxs-lookup"><span data-stu-id="14e16-117">Or in Visual Studio, using the Package Manager Console:</span></span>

``` PowerShell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```
<span data-ttu-id="14e16-118">V tématu [zprostředkovatelů databáze](providers/index.md) informace o dostupných zprostředkovatelů a [instalace jádra EF](get-started/install/index.md) podrobné kroky instalace.</span><span class="sxs-lookup"><span data-stu-id="14e16-118">See [Database Providers](providers/index.md) for information on available providers and [Installing EF Core](get-started/install/index.md) for more detailed installation steps.</span></span>

## <a name="the-model"></a><span data-ttu-id="14e16-119">Model</span><span class="sxs-lookup"><span data-stu-id="14e16-119">The Model</span></span>

<span data-ttu-id="14e16-120">Základní EF se provádí přístup k datům pomocí modelu.</span><span class="sxs-lookup"><span data-stu-id="14e16-120">With EF Core, data access is performed using a model.</span></span> <span data-ttu-id="14e16-121">Model se skládá z tříd entit a odvozené kontext, který představuje relaci s databází, což umožňuje dotazování a uložit data.</span><span class="sxs-lookup"><span data-stu-id="14e16-121">A model is made up of entity classes and a derived context that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="14e16-122">V tématu [vytvoření modelu](modeling/index.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="14e16-122">See [Creating a Model](modeling/index.md) to learn more.</span></span>

<span data-ttu-id="14e16-123">Můžete generovat model z existující databáze, ruční kód modelu tak, aby odpovídaly vaší databázi nebo použijte EF migrace vytvořit databázi z modelu (a momentální ho jako váš model změny v čase).</span><span class="sxs-lookup"><span data-stu-id="14e16-123">You can generate a model from an existing database, hand code a model to match your database, or use EF Migrations to create a database from your model (and evolve it as your model changes over time).</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
using System.Collections.Generic;

namespace Intro
{
    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=MyDatabase;Trusted_Connection=True;");
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Url { get; set; }
        public int Rating { get; set; }
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

## <a name="querying"></a><span data-ttu-id="14e16-124">Dotazování</span><span class="sxs-lookup"><span data-stu-id="14e16-124">Querying</span></span>

<span data-ttu-id="14e16-125">Instance třídy entita se načítají z databáze pomocí jazyka integrovaného dotazu (LINQ).</span><span class="sxs-lookup"><span data-stu-id="14e16-125">Instances of your entity classes are retrieved from the database using Language Integrated Query (LINQ).</span></span> <span data-ttu-id="14e16-126">V tématu [dotazování dat](querying/index.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="14e16-126">See [Querying Data](querying/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blogs = db.Blogs
        .Where(b => b.Rating > 3)
        .OrderBy(b => b.Url)
        .ToList();
}
```

## <a name="saving-data"></a><span data-ttu-id="14e16-127">Ukládání dat</span><span class="sxs-lookup"><span data-stu-id="14e16-127">Saving Data</span></span>

<span data-ttu-id="14e16-128">Data je vytvořit, odstranit a upravit v databázi pomocí instance třídy entity.</span><span class="sxs-lookup"><span data-stu-id="14e16-128">Data is created, deleted, and modified in the database using instances of your entity classes.</span></span> <span data-ttu-id="14e16-129">V tématu [ukládání dat](saving/index.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="14e16-129">See [Saving Data](saving/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```
