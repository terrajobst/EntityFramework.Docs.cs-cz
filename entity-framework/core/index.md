---
title: Rychlý přehled – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
ms.technology: entity-framework-core
uid: core/index
ms.openlocfilehash: 103e5e069687950a8411f2d92c7b5a191844e0ae
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37948987"
---
# <a name="entity-framework-core-quick-overview"></a>Rychlý přehled Entity Framework Core

Entity Framework (EF) Core je odlehčený, rozšiřitelné, a multiplatformní verze oblíbených dat Entity Framework přístup k technologii.

EF Core může sloužit jako objektově relační Mapovač (O/RM), umožňuje vývojářům .NET pracovat s databází s použitím objektů .NET a eliminují pro většinu kódu přístupu k datům je obvykle potřeba psát.

EF Core podporuje mnoho databázových strojů, naleznete v tématu [poskytovatelé databází](providers/index.md) podrobnosti.

Pokud chcete další informace o psaní kódu, doporučujeme jednu z našich [Začínáme](get-started/index.md) vodítka, které vám pomůžou začít s EF Core.

## <a name="what-is-new-in-ef-core"></a>Novinky v EF Core

Pokud jste obeznámeni s EF Core a chcete přejít přímo do podrobností o nejnovější vydané verzi:

- **[Novinky v EF Core 2.1](xref:core/what-is-new/ef-core-2.1)**
- **[Upgrade stávající aplikace na EF Core 2.x](xref:core/miscellaneous/1x-2x-upgrade)**


## <a name="get-entity-framework-core"></a>Získat Entity Framework Core

[Nainstalujte balíček NuGet](https://docs.nuget.org/ndocs/quickstart/use-a-package) k poskytovateli databáze, kterou chcete použít. Například chcete nainstalovat zprostředkovatele SQL Server ve vývoji multiplatformních aplikací pomocí `dotnet` nástroj na příkazovém řádku:

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

Nebo v sadě Visual Studio pomocí konzole Správce balíčků:

``` PowerShell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```
Zobrazit [poskytovatelé databází](providers/index.md) informace o dostupných zprostředkovatelů a [instalace EF Core](get-started/install/index.md) podrobný postup instalace.

## <a name="the-model"></a>Model

Přístup k datům s EF Core se provádí pomocí modelu. Model se skládá z tříd entit a odvozené kontext, který reprezentuje relaci s databází, umožňuje dotazování a uložit data. Zobrazit [vytváření modelu](modeling/index.md) Další informace.

Můžete generovat model z existující databáze, předat kód modelu tak, aby odpovídaly vaší databáze nebo použít migraci EF k vytvoření databáze z vašeho modelu (a vyvíjejí ho jako model mění v průběhu času).

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
