---
title: Dědičnost (relační databáze) – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 2d0a2abc554f5f115479f886ca3f9f4f01b80b5b
ms.sourcegitcommit: ea1cdec0b982b922a59b9d9301d3ed2b94baca0f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452277"
---
# <a name="inheritance-relational-database"></a><span data-ttu-id="7bdb8-102">Dědičnost (relační databáze)</span><span class="sxs-lookup"><span data-stu-id="7bdb8-102">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="7bdb8-103">Obecně se vztahuje k relačním databázím konfigurace v této části.</span><span class="sxs-lookup"><span data-stu-id="7bdb8-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="7bdb8-104">Metody rozšíření je vidět tady bude k dispozici při instalaci poskytovatele relační databáze (z důvodu sdílený *Microsoft.EntityFrameworkCore.Relational* balíček).</span><span class="sxs-lookup"><span data-stu-id="7bdb8-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="7bdb8-105">Dědičnost v modelu EF slouží k řízení, jak je reprezentovaná dědičnosti tříd entit v databázi.</span><span class="sxs-lookup"><span data-stu-id="7bdb8-105">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

> [!NOTE]  
> <span data-ttu-id="7bdb8-106">V současné době pouze vzor na hierarchii tabulky (TPH) je implementované v EF Core.</span><span class="sxs-lookup"><span data-stu-id="7bdb8-106">Currently, only the table-per-hierarchy (TPH) pattern is implemented in EF Core.</span></span> <span data-ttu-id="7bdb8-107">Za typ tabulky (TPT), jako jsou jiné běžné vzory a -za konkrétní – typ tabulky (TPC) ještě nejsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="7bdb8-107">Other common patterns like table-per-type (TPT) and table-per-concrete-type (TPC) are not yet available.</span></span>

## <a name="conventions"></a><span data-ttu-id="7bdb8-108">Konvence</span><span class="sxs-lookup"><span data-stu-id="7bdb8-108">Conventions</span></span>

<span data-ttu-id="7bdb8-109">Podle konvence se namapují dědičnosti pomocí vzoru za hierarchii tabulky (TPH).</span><span class="sxs-lookup"><span data-stu-id="7bdb8-109">By convention, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="7bdb8-110">TPH používá jednu tabulku pro ukládání dat pro všechny typy v hierarchii.</span><span class="sxs-lookup"><span data-stu-id="7bdb8-110">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="7bdb8-111">Sloupec diskriminátoru se používá k identifikaci typu každý řádek představuje.</span><span class="sxs-lookup"><span data-stu-id="7bdb8-111">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="7bdb8-112">EF Core bude pouze nastavení dědičnosti, dvou nebo více zděděných typů je explicitně zahrnuta v modelu (viz [dědičnosti](../inheritance.md) další podrobnosti).</span><span class="sxs-lookup"><span data-stu-id="7bdb8-112">EF Core will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="7bdb8-113">Tady je příklad ukazující scénář jednoduché dědičnosti a data uložená v tabulce relační databáze pomocí vzoru TPH.</span><span class="sxs-lookup"><span data-stu-id="7bdb8-113">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="7bdb8-114">*Diskriminátoru* typu, který identifikuje sloupce *blogu* je uložen v jednotlivých řádcích.</span><span class="sxs-lookup"><span data-stu-id="7bdb8-114">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

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

>[!NOTE]
> <span data-ttu-id="7bdb8-116">Sloupce databáze se automaticky stane s možnou hodnotou Null podle potřeby při použití TPH mapování.</span><span class="sxs-lookup"><span data-stu-id="7bdb8-116">Database columns are automatically made nullable as necessary when using TPH mapping.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="7bdb8-117">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="7bdb8-117">Data Annotations</span></span>

<span data-ttu-id="7bdb8-118">Datové poznámky nelze použít ke konfiguraci dědičnosti.</span><span class="sxs-lookup"><span data-stu-id="7bdb8-118">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="7bdb8-119">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="7bdb8-119">Fluent API</span></span>

<span data-ttu-id="7bdb8-120">Rozhraní Fluent API můžete použít ke konfiguraci názvu a typu sloupec diskriminátoru a hodnoty, které se používají k identifikaci jednotlivých typů v hierarchii.</span><span class="sxs-lookup"><span data-stu-id="7bdb8-120">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

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

## <a name="configuring-the-discriminator-property"></a><span data-ttu-id="7bdb8-121">Konfigurace vlastnost diskriminátoru</span><span class="sxs-lookup"><span data-stu-id="7bdb8-121">Configuring the discriminator property</span></span>

<span data-ttu-id="7bdb8-122">Ve výše uvedených příkladech diskriminátor vytvoří jako [stínové vlastnosti](xref:core/modeling/shadow-properties) na základní entitu v hierarchii.</span><span class="sxs-lookup"><span data-stu-id="7bdb8-122">In the examples above, the discriminator is created as a [shadow property](xref:core/modeling/shadow-properties) on the base entity of the hierarchy.</span></span> <span data-ttu-id="7bdb8-123">Protože je vlastnost v modelu, můžete ji nakonfigurovat, stejně jako ostatní vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="7bdb8-123">Since it is a property in the model, it can be configured just like other properties.</span></span> <span data-ttu-id="7bdb8-124">Chcete-li například nastavit maximální délky, pokud výchozí podle úmluvy diskriminátoru se používá:</span><span class="sxs-lookup"><span data-stu-id="7bdb8-124">For example, to set the max length when the default, by-convention discriminator is being used:</span></span>

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

<span data-ttu-id="7bdb8-125">Diskriminátor lze také mapovat na skutečné vlastnosti CLR ve vaší entitě.</span><span class="sxs-lookup"><span data-stu-id="7bdb8-125">The discriminator can also be mapped to an actual CLR property in your entity.</span></span> <span data-ttu-id="7bdb8-126">Příklad:</span><span class="sxs-lookup"><span data-stu-id="7bdb8-126">For example:</span></span>
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

<span data-ttu-id="7bdb8-127">Kombinace těchto dvou věcí společně je možné namapovat na reálném vlastnost diskriminátoru i ho nakonfigurovat:</span><span class="sxs-lookup"><span data-stu-id="7bdb8-127">Combining these two things together it is possible to both map the discriminator to a real property and configure it:</span></span>
```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
