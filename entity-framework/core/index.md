---
title: Přehled Entity Framework Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
uid: core/index
ms.openlocfilehash: 0107a520e5a698eaf76426b63c6f784392559167
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71196974"
---
# <a name="entity-framework-core"></a>Entity Framework Core

Jádro Entity Framework (EF) je odlehčená, rozšiřitelná, [Open zdrojová](https://github.com/aspnet/EntityFrameworkCore) a verze pro různé platformy, která je oblíbená entity Frameworká technologie pro přístup k datům.

EF Core může sloužit jako relační Mapovač (O/RM), což umožňuje vývojářům rozhraní .NET pracovat s databází pomocí objektů .NET a eliminovat nutnost většiny kódu přístupu k datům, které obvykle potřebují napsat.

EF Core podporuje mnoho databázových strojů, další informace najdete v tématu [poskytovatelé databází](providers/index.md) .

## <a name="the-model"></a>Model

Při použití EF Core se přístup k datům provádí pomocí modelu. Model je tvořen třídami entit a kontextový objekt, který představuje relaci s databází, a umožňuje vám dotazování a ukládání dat. Další informace najdete v tématu [Vytvoření modelu](modeling/index.md) .

Můžete vygenerovat model z existující databáze, kód pro psaní rukou modelu, který odpovídá vaší databázi, nebo použít [migrace EF](managing-schemas/migrations/index.md) k vytvoření databáze z modelu a pak ji vyvíjet jako změny modelu v průběhu času.

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

## <a name="querying"></a>Dotazování

Instance tříd vaší entity se načítají z databáze pomocí jazyka LINQ (Language Integrated Query). Další informace najdete v tématu [dotazování na data](querying/index.md) .

``` csharp
using (var db = new BloggingContext())
{
    var blogs = db.Blogs
        .Where(b => b.Rating > 3)
        .OrderBy(b => b.Url)
        .ToList();
}
```

## <a name="saving-data"></a>Ukládání dat

Data se vytvářejí, odstraňují a upravují v databázi s použitím instancí tříd vaší entity. Další informace najdete v tématu [ukládání dat](saving/index.md) .

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```

## <a name="next-steps"></a>Další kroky

Úvodní kurzy najdete v tématu [Začínáme s Entity Framework Core](get-started/index.md).

