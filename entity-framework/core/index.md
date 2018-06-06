---
title: Rychlý přehled - EF jádra
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
ms.technology: entity-framework-core
uid: core/index
ms.openlocfilehash: 3befcbd3ff3da5dd159e6e6cb5fe7140c81317c2
ms.sourcegitcommit: a2b38dedc88ca3ccbfe7b1db9602ca02da8294cd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34686659"
---
# <a name="entity-framework-core-quick-overview"></a>Rychlý přehled základní Entity Framework

Základní Entity Framework (EF) je lightweight rozšiřitelný, a přístup technologie a platformy verzi oblíbených datům Entity Framework.

Základní EF může sloužit jako objekt relační mapper (O RM), umožňuje vývojářům pracovat s databází pomocí objekty .NET, rozhraní .NET a povinnost většinu kódu, přístup k datům většinou potřebují k zápisu. 

Jádro EF podporuje mnoho databázové stroje najdete v tématu [zprostředkovatelů databáze](providers/index.md) podrobnosti.

Pokud chcete další informace o psaní kódu, bychom doporučili jeden z našich [Začínáme](get-started/index.md) příručky, které vám pomůžou začít s EF jádra.

## <a name="what-is-new-in-ef-core"></a>Co je nového v EF jádra

Pokud jste se seznámili s EF jádra a chcete přejít rovnou do podrobnosti o nejnovějších verzích:

- **[Co je nového v EF základní 2.1](xref:core/what-is-new/ef-core-2.1)**
- **[Upgrade existující aplikace, aby EF základní 2.x](xref:core/miscellaneous/1x-2x-upgrade)**


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
