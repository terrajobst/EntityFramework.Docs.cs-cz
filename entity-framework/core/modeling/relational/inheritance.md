---
title: Dědičnost (relační databáze) - EF jádra
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
ms.technology: entity-framework-core
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 22eed0002b5903d3cfd18a7e4af0fcd2d46a5c4c
ms.sourcegitcommit: d2434edbfa6fbcee7287e33b4915033b796e417e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/12/2018
ms.locfileid: "29152353"
---
# <a name="inheritance-relational-database"></a>Dědičnost (relační databáze)

> [!NOTE]  
> Obecně se vztahuje na relační databáze konfigurace v této části. Rozšiřující metody zobrazeny zde bude k dispozici při instalaci poskytovatele relační databáze (z důvodu sdílený *Microsoft.EntityFrameworkCore.Relational* balíčku).

Dědičnost ve EF model slouží k řízení, jak je reprezentována dědičnosti ve třídách entity v databázi.

> [!NOTE]  
> V současné době pouze vzoru tabulce pro hierarchii (TPH) je implementovaná v EF jádra. Další obecné vzory jako tabulka za type (TPT) a -za konkrétní – typ tabulky (TPC) ještě nejsou k dispozici.

## <a name="conventions"></a>Konvence

Podle konvence budou mapována dědičnosti pomocí vzoru tabulce pro hierarchii (TPH). TPH používá jednu tabulku pro ukládání dat pro všechny typy v hierarchii. Sloupce diskriminátoru slouží k identifikaci typů, které se každý řádek představuje.

EF základní bude pouze nastavení dědičnosti, pokud dva nebo více typů zděděné jsou explicitně součástí modelu (viz [dědičnosti](../inheritance.md) podrobnosti).

Níže je příkladem zobrazujícím scénář jednoduchá dědičnost a data uložená v tabulce relační databáze pomocí vzoru TPH. *Diskriminátoru* identifikuje sloupce typu *Blog* je uložený v jednotlivých řádcích.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/Conventions/Samples/InheritanceDbSets.cs)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<RssBlog> RssBlogs { get; set; }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

![obrázek](_static/inheritance-tph-data.png)

## <a name="data-annotations"></a>Datových poznámek

Poznámky dat nelze použít ke konfiguraci dědičnosti.

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít ke konfiguraci názvu a typu sloupce diskriminátoru a hodnoty, které se používají k identifikaci každý typ v hierarchii.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/InheritanceTPHDiscriminator.cs?highlight=7,8,9,10)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasDiscriminator<string>("blog_type")
            .HasValue<Blog>("blog_base")
            .HasValue<RssBlog>("blog_rss");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

## <a name="configuring-the-discriminator-property"></a>Konfigurace vlastností diskriminátoru

Ve výše uvedených příkladech, diskriminátoru je vytvořen jako [stínové vlastnost](xref:core/modeling/shadow-properties) na základní entitu v hierarchii. Vzhledem k tomu, že je vlastnost v modelu, můžete nakonfigurovat stejně jako ostatní vlastnosti. Chcete-li například nastavit maximální délka, pokud se používá výchozí, diskriminátoru podle konvence:

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

Diskriminátoru můžete taky namapovaný na vlastnost skutečné CLR v entitě. Příklad:
```C#
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasDiscriminator<string>("BlogType");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public string BlogType { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

Kombinování tyto dvě věci společně je možné namapovat diskriminátoru na vlastnost reálné i jeho konfiguraci:
```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
