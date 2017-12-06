---
title: "Alternativní klíče - EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
ms.technology: entity-framework-core
uid: core/modeling/alternate-keys
ms.openlocfilehash: 09f86a8932b71ec8f30ee90a088091a00233c20f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="alternate-keys"></a>Alternativní klíče

Alternativní klíč slouží jako alternativní jedinečný identifikátor pro každou instanci entity kromě primární klíč. Alternativní klíče můžete použít jako cíl vztahu. Při použití relační databáze mapuje koncept jedinečný index nebo omezení na alternativní sloupce klíče a jeden nebo více omezení cizích klíčů odkazující sloupce.

> [!TIP]  
> Pokud chcete vynutit jedinečnost sloupce pak chcete jedinečný index, nikoli alternativní klíč, přečtěte si téma [indexy](indexes.md). V EF alternativní klíče poskytují více funkcí než jedinečné indexy, protože je možné použít jako cíl pro cizí klíč.

Alternativní klíče jsou obvykle zavedená pro vás v případě potřeby a není potřeba je ručně nakonfigurovat. V tématu [konvence](#conventions) další podrobnosti.

## <a name="conventions"></a>Konvence

Podle konvence uvádíme alternativní klíč pro vás při určování vlastnost, která není primární klíč jako cíl vztahu.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/AlternateKey.cs?highlight=12)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .HasForeignKey(p => p.BlogUrl)
            .HasPrincipalKey(b => b.Url);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public string BlogUrl { get; set; }
    public Blog Blog { get; set; }
}
```

## <a name="data-annotations"></a>Datových poznámek

Alternativní klíče nelze konfigurovat pomocí datových poznámek.

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete nakonfigurovat jednu vlastnost, která má být alternativní klíč.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/AlternateKeySingle.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasAlternateKey(c => c.LicensePlate);
    }
}

class Car
{
    public int CarId { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }
}
```

Rozhraní Fluent API můžete také použít ke konfiguraci víc vlastností na alternativním klíčem (označované jako složený klíč alternativní).

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/AlternateKeyComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasAlternateKey(c => new { c.State, c.LicensePlate });
    }
}

class Car
{
    public int CarId { get; set; }
    public string State { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }
}
```
