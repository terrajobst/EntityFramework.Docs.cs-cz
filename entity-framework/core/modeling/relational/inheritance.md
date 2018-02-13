---
title: "Dědičnost (relační databáze) - EF jádra"
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
---
# <a name="inheritance-relational-database"></a><span data-ttu-id="3ea93-102">Dědičnost (relační databáze)</span><span class="sxs-lookup"><span data-stu-id="3ea93-102">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="3ea93-103">Obecně se vztahuje na relační databáze konfigurace v této části.</span><span class="sxs-lookup"><span data-stu-id="3ea93-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="3ea93-104">Rozšiřující metody zobrazeny zde bude k dispozici při instalaci poskytovatele relační databáze (z důvodu sdílený *Microsoft.EntityFrameworkCore.Relational* balíčku).</span><span class="sxs-lookup"><span data-stu-id="3ea93-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="3ea93-105">Dědičnost ve EF model slouží k řízení, jak je reprezentována dědičnosti ve třídách entity v databázi.</span><span class="sxs-lookup"><span data-stu-id="3ea93-105">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

> [!NOTE]  
> <span data-ttu-id="3ea93-106">V současné době pouze vzoru tabulce pro hierarchii (TPH) je implementovaná v EF jádra.</span><span class="sxs-lookup"><span data-stu-id="3ea93-106">Currently, only the table-per-hierarchy (TPH) pattern is implemented in EF Core.</span></span> <span data-ttu-id="3ea93-107">Další obecné vzory jako tabulka za type (TPT) a -za konkrétní – typ tabulky (TPC) ještě nejsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="3ea93-107">Other common patterns like table-per-type (TPT) and table-per-concrete-type (TPC) are not yet available.</span></span>

## <a name="conventions"></a><span data-ttu-id="3ea93-108">Konvence</span><span class="sxs-lookup"><span data-stu-id="3ea93-108">Conventions</span></span>

<span data-ttu-id="3ea93-109">Podle konvence budou mapována dědičnosti pomocí vzoru tabulce pro hierarchii (TPH).</span><span class="sxs-lookup"><span data-stu-id="3ea93-109">By convention, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="3ea93-110">TPH používá jednu tabulku pro ukládání dat pro všechny typy v hierarchii.</span><span class="sxs-lookup"><span data-stu-id="3ea93-110">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="3ea93-111">Sloupce diskriminátoru slouží k identifikaci typů, které se každý řádek představuje.</span><span class="sxs-lookup"><span data-stu-id="3ea93-111">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="3ea93-112">EF základní bude pouze nastavení dědičnosti, pokud dva nebo více typů zděděné jsou explicitně součástí modelu (viz [dědičnosti](../inheritance.md) podrobnosti).</span><span class="sxs-lookup"><span data-stu-id="3ea93-112">EF Core will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="3ea93-113">Níže je příkladem zobrazujícím scénář jednoduchá dědičnost a data uložená v tabulce relační databáze pomocí vzoru TPH.</span><span class="sxs-lookup"><span data-stu-id="3ea93-113">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="3ea93-114">*Diskriminátoru* identifikuje sloupce typu *Blog* je uložený v jednotlivých řádcích.</span><span class="sxs-lookup"><span data-stu-id="3ea93-114">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="3ea93-116">Datových poznámek</span><span class="sxs-lookup"><span data-stu-id="3ea93-116">Data Annotations</span></span>

<span data-ttu-id="3ea93-117">Poznámky dat nelze použít ke konfiguraci dědičnosti.</span><span class="sxs-lookup"><span data-stu-id="3ea93-117">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="3ea93-118">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="3ea93-118">Fluent API</span></span>

<span data-ttu-id="3ea93-119">Rozhraní Fluent API můžete použít ke konfiguraci názvu a typu sloupce diskriminátoru a hodnoty, které se používají k identifikaci každý typ v hierarchii.</span><span class="sxs-lookup"><span data-stu-id="3ea93-119">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

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

## <a name="configuring-the-discriminator-property"></a><span data-ttu-id="3ea93-120">Konfigurace vlastností diskriminátoru</span><span class="sxs-lookup"><span data-stu-id="3ea93-120">Configuring the discriminator property</span></span>

<span data-ttu-id="3ea93-121">Ve výše uvedených příkladech, diskriminátoru je vytvořen jako [stínové vlastnost](xref:core/modeling/shadow-properties) na základní entitu v hierarchii.</span><span class="sxs-lookup"><span data-stu-id="3ea93-121">In the examples above, the discriminator is created as a [shadow property](xref:core/modeling/shadow-properties) on the base entity of the hierarchy.</span></span> <span data-ttu-id="3ea93-122">Vzhledem k tomu, že je vlastnost v modelu, můžete nakonfigurovat stejně jako ostatní vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="3ea93-122">Since it is a property in the model, it can be configured just like other properties.</span></span> <span data-ttu-id="3ea93-123">Chcete-li například nastavit maximální délka, pokud se používá výchozí, diskriminátoru podle konvence:</span><span class="sxs-lookup"><span data-stu-id="3ea93-123">For example, to set the max length when the default, by-convention discriminator is being used:</span></span>

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

<span data-ttu-id="3ea93-124">Diskriminátoru můžete taky namapovaný na vlastnost skutečné CLR v entitě.</span><span class="sxs-lookup"><span data-stu-id="3ea93-124">The discriminator can also be mapped to an actual CLR property in your entity.</span></span> <span data-ttu-id="3ea93-125">Příklad:</span><span class="sxs-lookup"><span data-stu-id="3ea93-125">For example:</span></span>
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

<span data-ttu-id="3ea93-126">Kombinování tyto dvě věci společně je možné namapovat diskriminátoru na vlastnost reálné i jeho konfiguraci:</span><span class="sxs-lookup"><span data-stu-id="3ea93-126">Combining these two things together it is possible to both map the discriminator to a real property and configure it:</span></span>
```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
