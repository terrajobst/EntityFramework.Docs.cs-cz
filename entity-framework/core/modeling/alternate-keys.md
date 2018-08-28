---
title: Alternativní klíče – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
uid: core/modeling/alternate-keys
ms.openlocfilehash: b26d8bc1630af9e811d9c4e7da850a618bc8042e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996968"
---
# <a name="alternate-keys"></a>Alternativní klíče

Alternativní klíče slouží jako alternativní jedinečný identifikátor pro každou instanci entity kromě primární klíč. Alternativní klíče můžete použít jako cíl relace. Při používání relační databáze mapuje koncept jedinečný index nebo omezení na sloupce alternativního klíče a jeden nebo více omezení cizího klíče, které odkazují na sloupce.

> [!TIP]  
> Pokud chcete vynutit jedinečnosti sloupce, pak je třeba jedinečný index místo alternativního klíče, přečtěte si téma [indexy](indexes.md). V EF alternativní klíče poskytnout více funkcí než jedinečné indexy, protože slouží jako cíl cizího klíče.

Alternativní klíče jsou obvykle vložena za vás v případě potřeby a není potřeba nakonfigurovat ručně. Zobrazit [konvence](#conventions) další podrobnosti.

## <a name="conventions"></a>Konvence

Podle úmluvy je zavedená alternativního klíče pro vás při určování vlastnost, která není primární klíč jako cíl vztahu.

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

## <a name="data-annotations"></a>Datové poznámky

Alternativní klíče nelze konfigurovat pomocí datových poznámek.

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít ke konfiguraci jedné vlastnosti alternativního klíče.

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

Můžete také použít rozhraní Fluent API nakonfigurovat několik vlastností tak, aby alternativního klíče (označované jako složený klíč alternativní).

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
