---
title: Alternativní klíče – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
uid: core/modeling/alternate-keys
ms.openlocfilehash: 87df5d174a1db12fb3ab763ac76c3b863a83087e
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197471"
---
# <a name="alternate-keys"></a>Alternativní klíče

Alternativní klíč slouží jako alternativní jedinečný identifikátor pro každou instanci entity kromě primárního klíče. Alternativní klíče lze použít jako cíl relace. Při použití relační databáze se tato mapa mapuje na koncept jedinečného indexu nebo omezení pro sloupce nebo sloupce alternativních klíčů a jedno nebo více omezení cizího klíče, které odkazují na sloupce.

> [!TIP]  
> Pokud chcete vymáhat jedinečnost sloupce a chcete místo alternativního klíče vyhovět jedinečným indexům, přečtěte si téma [indexy](indexes.md). V EF poskytují alternativní klíče větší funkce než jedinečné indexy, protože je lze použít jako cíl cizího klíče.

V případě potřeby jsou obvykle zavedeny alternativní klíče a není nutné je ručně konfigurovat. Další podrobnosti najdete v tématu věnovaném [konvencím](#conventions) .

## <a name="conventions"></a>Konvence

Podle konvence se pro vás zavádí alternativní klíč, pokud identifikujete vlastnost, která není primárním klíčem, jako cíl relace.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/AlternateKey.cs?highlight=12)] -->
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

Pomocí rozhraní Fluent API můžete nakonfigurovat jednu vlastnost jako alternativní klíč.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/AlternateKeySingle.cs?highlight=7,8)] -->
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

Rozhraní Fluent API můžete použít také ke konfiguraci více vlastností, aby byl alternativním klíčem (označovaným jako složený alternativní klíč).

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/AlternateKeyComposite.cs?highlight=7,8)] -->
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
