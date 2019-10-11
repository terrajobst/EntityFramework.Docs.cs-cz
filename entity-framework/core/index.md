---
title: Přehled Entity Framework Core-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
uid: core/index
ms.openlocfilehash: e736251753134b716e64f24f6c517ed9f66a7db4
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181316"
---
# <a name="entity-framework-core"></a><span data-ttu-id="af3bf-102">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="af3bf-102">Entity Framework Core</span></span>

<span data-ttu-id="af3bf-103">Jádro Entity Framework (EF) je odlehčená, rozšiřitelná, [Open zdrojová](https://github.com/aspnet/EntityFrameworkCore) a verze pro různé platformy, která je oblíbená entity Frameworká technologie pro přístup k datům.</span><span class="sxs-lookup"><span data-stu-id="af3bf-103">Entity Framework (EF) Core is a lightweight, extensible, [open source](https://github.com/aspnet/EntityFrameworkCore) and cross-platform version of the popular Entity Framework data access technology.</span></span>

<span data-ttu-id="af3bf-104">EF Core může sloužit jako relační Mapovač (O/RM), což umožňuje vývojářům rozhraní .NET pracovat s databází pomocí objektů .NET a eliminovat nutnost většiny kódu přístupu k datům, které obvykle potřebují napsat.</span><span class="sxs-lookup"><span data-stu-id="af3bf-104">EF Core can serve as an object-relational mapper (O/RM), enabling .NET developers to work with a database using .NET objects, and eliminating the need for most of the data-access code they usually need to write.</span></span>

<span data-ttu-id="af3bf-105">EF Core podporuje mnoho databázových strojů, další informace najdete v tématu [poskytovatelé databází](providers/index.md) .</span><span class="sxs-lookup"><span data-stu-id="af3bf-105">EF Core supports many database engines, see [Database Providers](providers/index.md) for details.</span></span>

## <a name="the-model"></a><span data-ttu-id="af3bf-106">Model</span><span class="sxs-lookup"><span data-stu-id="af3bf-106">The Model</span></span>

<span data-ttu-id="af3bf-107">Při použití EF Core se přístup k datům provádí pomocí modelu.</span><span class="sxs-lookup"><span data-stu-id="af3bf-107">With EF Core, data access is performed using a model.</span></span> <span data-ttu-id="af3bf-108">Model je tvořen třídami entit a kontextový objekt, který představuje relaci s databází, a umožňuje vám dotazování a ukládání dat.</span><span class="sxs-lookup"><span data-stu-id="af3bf-108">A model is made up of entity classes and a context object that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="af3bf-109">Další informace najdete v tématu [Vytvoření modelu](modeling/index.md) .</span><span class="sxs-lookup"><span data-stu-id="af3bf-109">See [Creating a Model](modeling/index.md) to learn more.</span></span>

<span data-ttu-id="af3bf-110">Můžete vygenerovat model z existující databáze, kód pro psaní rukou modelu, který odpovídá vaší databázi, nebo použít [migrace EF](managing-schemas/migrations/index.md) k vytvoření databáze z modelu a pak ji vyvíjet jako změny modelu v průběhu času.</span><span class="sxs-lookup"><span data-stu-id="af3bf-110">You can generate a model from an existing database, hand code a model to match your database, or use [EF Migrations](managing-schemas/migrations/index.md) to create a database from your model, and then evolve it as your model changes over time.</span></span>

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

## <a name="querying"></a><span data-ttu-id="af3bf-111">Dotazování</span><span class="sxs-lookup"><span data-stu-id="af3bf-111">Querying</span></span>

<span data-ttu-id="af3bf-112">Instance tříd vaší entity se načítají z databáze pomocí jazyka LINQ (Language Integrated Query).</span><span class="sxs-lookup"><span data-stu-id="af3bf-112">Instances of your entity classes are retrieved from the database using Language Integrated Query (LINQ).</span></span> <span data-ttu-id="af3bf-113">Další informace najdete v tématu [dotazování na data](querying/index.md) .</span><span class="sxs-lookup"><span data-stu-id="af3bf-113">See [Querying Data](querying/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blogs = db.Blogs
        .Where(b => b.Rating > 3)
        .OrderBy(b => b.Url)
        .ToList();
}
```

## <a name="saving-data"></a><span data-ttu-id="af3bf-114">Ukládání dat</span><span class="sxs-lookup"><span data-stu-id="af3bf-114">Saving Data</span></span>

<span data-ttu-id="af3bf-115">Data se vytvářejí, odstraňují a upravují v databázi s použitím instancí tříd vaší entity.</span><span class="sxs-lookup"><span data-stu-id="af3bf-115">Data is created, deleted, and modified in the database using instances of your entity classes.</span></span> <span data-ttu-id="af3bf-116">Další informace najdete v tématu [ukládání dat](saving/index.md) .</span><span class="sxs-lookup"><span data-stu-id="af3bf-116">See [Saving Data](saving/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```

## <a name="next-steps"></a><span data-ttu-id="af3bf-117">Další kroky</span><span class="sxs-lookup"><span data-stu-id="af3bf-117">Next steps</span></span>

<span data-ttu-id="af3bf-118">Úvodní kurzy najdete v tématu [Začínáme s Entity Framework Core](get-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="af3bf-118">For introductory tutorials, see [Getting Started with Entity Framework Core](get-started/index.md).</span></span>

