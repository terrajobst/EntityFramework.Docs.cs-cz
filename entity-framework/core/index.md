---
title: "Rychlý přehled - EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
ms.technology: entity-framework-core
uid: core/index
ms.openlocfilehash: 13de9cf98111b8e253e073c591fcec04206b4c4f
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="entity-framework-core-quick-overview"></a>Rychlý přehled základní Entity Framework

Základní Entity Framework (EF) je lightweight rozšiřitelný, a přístup technologie a platformy verzi oblíbených datům Entity Framework.

Základní EF je objekt relační mapper (O/RM), která umožňuje vývojářům rozhraní .NET pro práci s objekty .NET pomocí databáze. Eliminuje nutnost většinu kódu přístup k datům, které vývojáři potřebují obvykle k zápisu. Jádro EF podporuje mnoho databázové stroje najdete v tématu [zprostředkovatelů databáze](providers/index.md) podrobnosti.

Pokud chcete další informace o psaní kódu, bychom doporučili jeden z našich [Začínáme](get-started/index.md) příručky, které vám pomůžou začít s EF jádra.

## <a name="latest-version-ef-core-20"></a>Nejnovější verzi: základní EF 2.0

Pokud jste se seznámili s EF jádra a chcete přejít rovnou do podrobnosti nové verze:

- **[Nové funkce v EF základní 2.0](what-is-new/index.md)**
- **[Upgrade existující aplikace, aby EF základní 2.0](miscellaneous/1x-2x-upgrade.md)**

## <a name="get-entity-framework-core"></a>Získat Entity Framework Core

[Nainstalujte balíček NuGet](https://docs.nuget.org/ndocs/quickstart/use-a-package) k poskytovateli databáze, kterou chcete použít. Například Abyste mohli nainstalovat poskytovatele serveru SQL Server v vývoj pro různé platformy pomocí `dotnet` nástroj na příkazovém řádku:

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

Nebo v sadě Visual Studio pomocí konzoly Správce balíčků:

``` PowerShell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```
V tématu [zprostředkovatelů databáze](providers/index.md) informace o dostupných zprostředkovatelů a [instalace jádra EF](get-started/install/index.md) podrobné kroky instalace.

## <a name="the-model"></a>Model

Základní EF se provádí přístup k datům pomocí modelu. Model se skládá z tříd entit a odvozené kontext, který představuje relaci s databází, což umožňuje dotazování a uložit data. V tématu [vytvoření modelu](modeling/index.md) Další informace.

Můžete generovat model z existující databáze, ruční kód modelu tak, aby odpovídaly vaší databázi nebo použijte EF migrace vytvořit databázi z modelu (a momentální ho jako váš model změny v čase).

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

Instance třídy entita se načítají z databáze pomocí jazyka integrovaného dotazu (LINQ). V tématu [dotazování dat](querying/index.md) Další informace.

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

Data je vytvořit, odstranit a upravit v databázi pomocí instance třídy entity. V tématu [ukládání dat](saving/index.md) Další informace.

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```
