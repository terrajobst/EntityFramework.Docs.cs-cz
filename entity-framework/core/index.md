---
title: Přehled – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
uid: core/index
ms.openlocfilehash: fa0695be29668789a179f9a0d6330f3361dbac29
ms.sourcegitcommit: 6c4e06bc62d98442530e93a44725e38e59483d42
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/18/2019
ms.locfileid: "58131420"
---
# <a name="entity-framework-core"></a><span data-ttu-id="e7480-102">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="e7480-102">Entity Framework Core</span></span>

<span data-ttu-id="e7480-103">Entity Framework (EF) Core je odlehčený, rozšiřitelné, [opensourcových](https://github.com/aspnet/EntityFrameworkCore) a multiplatformní verze oblíbených dat Entity Framework přístup k technologii.</span><span class="sxs-lookup"><span data-stu-id="e7480-103">Entity Framework (EF) Core is a lightweight, extensible, [open source](https://github.com/aspnet/EntityFrameworkCore) and cross-platform version of the popular Entity Framework data access technology.</span></span>

<span data-ttu-id="e7480-104">EF Core může sloužit jako objektově relační Mapovač (O/RM), umožňuje vývojářům .NET pracovat s databází s použitím objektů .NET a eliminují pro většinu kódu přístupu k datům je obvykle potřeba psát.</span><span class="sxs-lookup"><span data-stu-id="e7480-104">EF Core can serve as an object-relational mapper (O/RM), enabling .NET developers to work with a database using .NET objects, and eliminating the need for most of the data-access code they usually need to write.</span></span>

<span data-ttu-id="e7480-105">EF Core podporuje mnoho databázových strojů, naleznete v tématu [poskytovatelé databází](providers/index.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="e7480-105">EF Core supports many database engines, see [Database Providers](providers/index.md) for details.</span></span>

## <a name="the-model"></a><span data-ttu-id="e7480-106">Model</span><span class="sxs-lookup"><span data-stu-id="e7480-106">The Model</span></span>

<span data-ttu-id="e7480-107">Přístup k datům s EF Core se provádí pomocí modelu.</span><span class="sxs-lookup"><span data-stu-id="e7480-107">With EF Core, data access is performed using a model.</span></span> <span data-ttu-id="e7480-108">Model se skládá z tříd entit a objekt kontextu, který reprezentuje relaci s databází, umožňuje dotazování a uložit data.</span><span class="sxs-lookup"><span data-stu-id="e7480-108">A model is made up of entity classes and a context object that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="e7480-109">Zobrazit [vytváření modelu](modeling/index.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="e7480-109">See [Creating a Model](modeling/index.md) to learn more.</span></span>

<span data-ttu-id="e7480-110">Generování modelu z existující databázi, ručně kód modelu odpovídat databázi, nebo použít k vytvoření databáze z vašeho modelu migrace EF a potom vyvíjí jako váš model mění v průběhu času.</span><span class="sxs-lookup"><span data-stu-id="e7480-110">You can generate a model from an existing database, hand code a model to match your database, or use EF Migrations to create a database from your model, and then evolve it as your model changes over time.</span></span>

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
            optionsBuilder.UseSqlServer(
                @"Server=(localdb)\mssqllocaldb;Database=Blogging;Integrated Security=True");
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

## <a name="querying"></a><span data-ttu-id="e7480-111">Dotazování</span><span class="sxs-lookup"><span data-stu-id="e7480-111">Querying</span></span>

<span data-ttu-id="e7480-112">Instance třídy entity načtou z databáze pomocí Language Integrated Query (LINQ).</span><span class="sxs-lookup"><span data-stu-id="e7480-112">Instances of your entity classes are retrieved from the database using Language Integrated Query (LINQ).</span></span> <span data-ttu-id="e7480-113">Zobrazit [dotazování na Data](querying/index.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="e7480-113">See [Querying Data](querying/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blogs = db.Blogs
        .Where(b => b.Rating > 3)
        .OrderBy(b => b.Url)
        .ToList();
}
```

## <a name="saving-data"></a><span data-ttu-id="e7480-114">Ukládání dat</span><span class="sxs-lookup"><span data-stu-id="e7480-114">Saving Data</span></span>

<span data-ttu-id="e7480-115">Data je vytvořit, odstranit a upravit v databázi pomocí instance třídy entity.</span><span class="sxs-lookup"><span data-stu-id="e7480-115">Data is created, deleted, and modified in the database using instances of your entity classes.</span></span> <span data-ttu-id="e7480-116">Zobrazit [ukládání dat](saving/index.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="e7480-116">See [Saving Data](saving/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```

## <a name="next-steps"></a><span data-ttu-id="e7480-117">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e7480-117">Next steps</span></span>

<span data-ttu-id="e7480-118">Úvodní kurzy najdete na stránce [Začínáme s Entity Framework Core](get-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="e7480-118">For introductory tutorials, see [Getting Started with Entity Framework Core](get-started/index.md).</span></span>

