---
title: Přehled – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
uid: core/index
ms.openlocfilehash: d9fcafb35248b1af54e1ac707e2ff7d4e80e4aa2
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995648"
---
# <a name="entity-framework-core"></a><span data-ttu-id="4158c-102">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="4158c-102">Entity Framework Core</span></span>

<span data-ttu-id="4158c-103">Entity Framework (EF) Core je odlehčený, rozšiřitelné, a multiplatformní verze oblíbených dat Entity Framework přístup k technologii.</span><span class="sxs-lookup"><span data-stu-id="4158c-103">Entity Framework (EF) Core is a lightweight, extensible, and cross-platform version of the popular Entity Framework data access technology.</span></span>

<span data-ttu-id="4158c-104">EF Core může sloužit jako objektově relační Mapovač (O/RM), umožňuje vývojářům .NET pracovat s databází s použitím objektů .NET a eliminují pro většinu kódu přístupu k datům je obvykle potřeba psát.</span><span class="sxs-lookup"><span data-stu-id="4158c-104">EF Core can serve as an object-relational mapper (O/RM), enabling .NET developers to work with a database using .NET objects, and eliminating the need for most of the data-access code they usually need to write.</span></span>

<span data-ttu-id="4158c-105">EF Core podporuje mnoho databázových strojů, naleznete v tématu [poskytovatelé databází](providers/index.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="4158c-105">EF Core supports many database engines, see [Database Providers](providers/index.md) for details.</span></span>

<span data-ttu-id="4158c-106">Pokud chcete další informace o psaní kódu, doporučujeme jednu z našich [Začínáme](get-started/index.md) vodítka, které vám pomůžou začít s EF Core.</span><span class="sxs-lookup"><span data-stu-id="4158c-106">If you like to learn by writing code, we'd recommend one of our [Getting Started](get-started/index.md) guides to get you started with EF Core.</span></span>

## <a name="what-is-new-in-ef-core"></a><span data-ttu-id="4158c-107">Novinky v EF Core</span><span class="sxs-lookup"><span data-stu-id="4158c-107">What is new in EF Core</span></span>

<span data-ttu-id="4158c-108">Pokud jste obeznámeni s EF Core a chcete přejít přímo do podrobností o nejnovější vydané verzi:</span><span class="sxs-lookup"><span data-stu-id="4158c-108">If you are familiar with EF Core and want to jump straight into the details of the latest releases:</span></span>

- <span data-ttu-id="4158c-109">**[Novinky v EF Core 2.1](xref:core/what-is-new/ef-core-2.1)**</span><span class="sxs-lookup"><span data-stu-id="4158c-109">**[What is new in EF Core 2.1](xref:core/what-is-new/ef-core-2.1)**</span></span>
- <span data-ttu-id="4158c-110">**[Upgrade stávající aplikace na EF Core 2.x](xref:core/miscellaneous/1x-2x-upgrade)**</span><span class="sxs-lookup"><span data-stu-id="4158c-110">**[Upgrading existing applications to EF Core 2.x](xref:core/miscellaneous/1x-2x-upgrade)**</span></span>


## <a name="get-entity-framework-core"></a><span data-ttu-id="4158c-111">Získat Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="4158c-111">Get Entity Framework Core</span></span>

<span data-ttu-id="4158c-112">[Nainstalujte balíček NuGet](https://docs.nuget.org/ndocs/quickstart/use-a-package) k poskytovateli databáze, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="4158c-112">[Install the NuGet package](https://docs.nuget.org/ndocs/quickstart/use-a-package) for the database provider you want to use.</span></span> <span data-ttu-id="4158c-113">Například chcete nainstalovat zprostředkovatele SQL Server ve vývoji multiplatformních aplikací pomocí `dotnet` nástroj na příkazovém řádku:</span><span class="sxs-lookup"><span data-stu-id="4158c-113">For example, to install the SQL Server provider in cross-platform development using `dotnet` tool in the command line:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="4158c-114">Nebo v sadě Visual Studio pomocí konzole Správce balíčků:</span><span class="sxs-lookup"><span data-stu-id="4158c-114">Or in Visual Studio, using the Package Manager Console:</span></span>

``` PowerShell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```
<span data-ttu-id="4158c-115">Zobrazit [poskytovatelé databází](providers/index.md) informace o dostupných zprostředkovatelů a [instalace EF Core](get-started/install/index.md) podrobný postup instalace.</span><span class="sxs-lookup"><span data-stu-id="4158c-115">See [Database Providers](providers/index.md) for information on available providers and [Installing EF Core](get-started/install/index.md) for more detailed installation steps.</span></span>

## <a name="the-model"></a><span data-ttu-id="4158c-116">Model</span><span class="sxs-lookup"><span data-stu-id="4158c-116">The Model</span></span>

<span data-ttu-id="4158c-117">Přístup k datům s EF Core se provádí pomocí modelu.</span><span class="sxs-lookup"><span data-stu-id="4158c-117">With EF Core, data access is performed using a model.</span></span> <span data-ttu-id="4158c-118">Model se skládá z tříd entit a odvozené kontext, který reprezentuje relaci s databází, umožňuje dotazování a uložit data.</span><span class="sxs-lookup"><span data-stu-id="4158c-118">A model is made up of entity classes and a derived context that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="4158c-119">Zobrazit [vytváření modelu](modeling/index.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="4158c-119">See [Creating a Model](modeling/index.md) to learn more.</span></span>

<span data-ttu-id="4158c-120">Můžete generovat model z existující databáze, předat kód modelu tak, aby odpovídaly vaší databáze nebo použít migraci EF k vytvoření databáze z vašeho modelu (a vyvíjejí ho jako model mění v průběhu času).</span><span class="sxs-lookup"><span data-stu-id="4158c-120">You can generate a model from an existing database, hand code a model to match your database, or use EF Migrations to create a database from your model (and evolve it as your model changes over time).</span></span>

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

## <a name="querying"></a><span data-ttu-id="4158c-121">Dotazování</span><span class="sxs-lookup"><span data-stu-id="4158c-121">Querying</span></span>

<span data-ttu-id="4158c-122">Instance třídy entity načtou z databáze pomocí Language Integrated Query (LINQ).</span><span class="sxs-lookup"><span data-stu-id="4158c-122">Instances of your entity classes are retrieved from the database using Language Integrated Query (LINQ).</span></span> <span data-ttu-id="4158c-123">Zobrazit [dotazování na Data](querying/index.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="4158c-123">See [Querying Data](querying/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blogs = db.Blogs
        .Where(b => b.Rating > 3)
        .OrderBy(b => b.Url)
        .ToList();
}
```

## <a name="saving-data"></a><span data-ttu-id="4158c-124">Ukládání dat</span><span class="sxs-lookup"><span data-stu-id="4158c-124">Saving Data</span></span>

<span data-ttu-id="4158c-125">Data je vytvořit, odstranit a upravit v databázi pomocí instance třídy entity.</span><span class="sxs-lookup"><span data-stu-id="4158c-125">Data is created, deleted, and modified in the database using instances of your entity classes.</span></span> <span data-ttu-id="4158c-126">Zobrazit [ukládání dat](saving/index.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="4158c-126">See [Saving Data](saving/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```
