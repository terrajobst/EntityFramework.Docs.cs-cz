---
title: Dědičnost (relační databáze) – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
uid: core/modeling/relational/inheritance
ms.openlocfilehash: c660107619470a726fe13ad8eee2850749e6dcd9
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812092"
---
# <a name="inheritance-relational-database"></a>Dědičnost (relační databáze)

> [!NOTE]  
> Konfigurace v této části platí pro relační databáze obecně. Metody rozšíření zobrazené tady budou k dispozici při instalaci zprostředkovatele relační databáze (z důvodu sdíleného balíčku *Microsoft. EntityFrameworkCore. relační* ).

Dědičnost v modelu EF se používá k řízení způsobu reprezentace dědičnosti tříd entit v databázi.

> [!NOTE]  
> V současné době je v EF Core implementován pouze vzor Table-per-Hierarchy (TPH). Další běžné vzorce, jako je například tabulka na typ (TPT) a tabulka-podle konkrétního typu (TPC), ještě nejsou k dispozici.

## <a name="conventions"></a>Konvence

Podle konvence se dědičnost mapuje pomocí vzoru Table-per-Hierarchy (TPH). K ukládání dat pro všechny typy v hierarchii používá typ TPH jednu tabulku. Sloupec diskriminátoru slouží k identifikaci typu, který každý řádek představuje.

EF Core bude dědění nastavení pouze v případě, že jsou do modelu explicitně zahrnuty dva nebo více děděných typů (další podrobnosti naleznete v tématu [Dědičnost](../inheritance.md) ).

Níže je příklad znázorňující jednoduchý scénář dědičnosti a data uložená v relační tabulce databáze pomocí vzoru TPH. Sloupec *diskriminátor* určuje, který typ *blogu* je uložený v každém řádku.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/Conventions/InheritanceDbSets.cs)] -->
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

>[!NOTE]
> Sloupce databáze se při použití mapování typu TPH automaticky v případě potřeby nepovolují s možnou hodnotou null.

## <a name="data-annotations"></a>Datové poznámky

Ke konfiguraci dědičnosti nelze použít datové poznámky.

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít ke konfiguraci názvu a typu sloupce diskriminátoru a hodnot, které se používají k identifikaci každého typu v hierarchii.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/InheritanceTPHDiscriminator.cs?highlight=7,8,9,10)] -->
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

## <a name="configuring-the-discriminator-property"></a>Konfigurace vlastnosti diskriminátoru

V předchozích příkladech je diskriminátor vytvořen jako [vlastnost Shadow](xref:core/modeling/shadow-properties) v základní entitě hierarchie. Vzhledem k tomu, že se jedná o vlastnost v modelu, lze ji nakonfigurovat stejně jako jiné vlastnosti. Například pro nastavení maximální délky při použití výchozího diskriminátoru podle konvence:

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

Diskriminátor lze také namapovat na skutečnou vlastnost CLR v entitě. Příklad:

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

Kombinování těchto dvou věcí je možné namapovat mezi diskriminátory na skutečnou vlastnost a nakonfigurovat ji:

```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
