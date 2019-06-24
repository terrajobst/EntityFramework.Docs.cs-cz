---
title: Přehled – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
uid: core/index
ms.openlocfilehash: 0e35a2b3f89c92b717b8e05c8fa3ae5af5ce8fd3
ms.sourcegitcommit: 06073f8efde97dd5f540dbfb69f574d8380566fe
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/24/2019
ms.locfileid: "67333782"
---
# <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework (EF) Core je odlehčený, rozšiřitelné, [opensourcových](https://github.com/aspnet/EntityFrameworkCore) a multiplatformní verze oblíbených dat Entity Framework přístup k technologii.

EF Core může sloužit jako objektově relační Mapovač (O/RM), umožňuje vývojářům .NET pracovat s databází s použitím objektů .NET a eliminují pro většinu kódu přístupu k datům je obvykle potřeba psát.

EF Core podporuje mnoho databázových strojů, naleznete v tématu [poskytovatelé databází](providers/index.md) podrobnosti.

## <a name="the-model"></a>Model

Přístup k datům s EF Core se provádí pomocí modelu. Model se skládá z tříd entit a objekt kontextu, který reprezentuje relaci s databází, umožňuje dotazování a uložit data. Zobrazit [vytváření modelu](modeling/index.md) Další informace.

Můžete generovat model z existující databáze, předat kód modelu pro vyhledání databáze, nebo použijte [migrace EF](managing-schemas/migrations/index.md) vytvoření databáze z modelu a pak ho můžete vyvíjet s modelu mění v průběhu času.

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

Instance třídy entity načtou z databáze pomocí Language Integrated Query (LINQ). Zobrazit [dotazování na Data](querying/index.md) Další informace.

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

Data je vytvořit, odstranit a upravit v databázi pomocí instance třídy entity. Zobrazit [ukládání dat](saving/index.md) Další informace.

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```

## <a name="next-steps"></a>Další kroky

Úvodní kurzy najdete na stránce [Začínáme s Entity Framework Core](get-started/index.md).

