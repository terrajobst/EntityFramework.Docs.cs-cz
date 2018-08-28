---
title: Dědičnost (relační databáze) – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 019893ec8268ef9e59d581799a13d63610c80616
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996319"
---
# <a name="inheritance-relational-database"></a>Dědičnost (relační databáze)

> [!NOTE]  
> Obecně se vztahuje k relačním databázím konfigurace v této části. Metody rozšíření je vidět tady bude k dispozici při instalaci poskytovatele relační databáze (z důvodu sdílený *Microsoft.EntityFrameworkCore.Relational* balíček).

Dědičnost v modelu EF slouží k řízení, jak je reprezentovaná dědičnosti tříd entit v databázi.

> [!NOTE]  
> V současné době pouze vzor na hierarchii tabulky (TPH) je implementované v EF Core. Za typ tabulky (TPT), jako jsou jiné běžné vzory a -za konkrétní – typ tabulky (TPC) ještě nejsou k dispozici.

## <a name="conventions"></a>Konvence

Podle konvence se namapují dědičnosti pomocí vzoru za hierarchii tabulky (TPH). TPH používá jednu tabulku pro ukládání dat pro všechny typy v hierarchii. Sloupec diskriminátoru se používá k identifikaci typu každý řádek představuje.

EF Core bude pouze nastavení dědičnosti, dvou nebo více zděděných typů je explicitně zahrnuta v modelu (viz [dědičnosti](../inheritance.md) další podrobnosti).

Tady je příklad ukazující scénář jednoduché dědičnosti a data uložená v tabulce relační databáze pomocí vzoru TPH. *Diskriminátoru* typu, který identifikuje sloupce *blogu* je uložen v jednotlivých řádcích.

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

## <a name="data-annotations"></a>Datové poznámky

Datové poznámky nelze použít ke konfiguraci dědičnosti.

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít ke konfiguraci názvu a typu sloupec diskriminátoru a hodnoty, které se používají k identifikaci jednotlivých typů v hierarchii.

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

## <a name="configuring-the-discriminator-property"></a>Konfigurace vlastnost diskriminátoru

Ve výše uvedených příkladech diskriminátor vytvoří jako [stínové vlastnosti](xref:core/modeling/shadow-properties) na základní entitu v hierarchii. Protože je vlastnost v modelu, můžete ji nakonfigurovat, stejně jako ostatní vlastnosti. Chcete-li například nastavit maximální délky, pokud výchozí podle úmluvy diskriminátoru se používá:

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

Diskriminátor lze také mapovat na skutečné vlastnosti CLR ve vaší entitě. Příklad:
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

Kombinace těchto dvou věcí společně je možné namapovat na reálném vlastnost diskriminátoru i ho nakonfigurovat:
```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
