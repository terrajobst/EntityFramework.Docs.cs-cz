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
# <a name="inheritance-relational-database"></a><span data-ttu-id="b09f2-102">Dědičnost (relační databáze)</span><span class="sxs-lookup"><span data-stu-id="b09f2-102">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="b09f2-103">Obecně se vztahuje k relačním databázím konfigurace v této části.</span><span class="sxs-lookup"><span data-stu-id="b09f2-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="b09f2-104">Metody rozšíření je vidět tady bude k dispozici při instalaci poskytovatele relační databáze (z důvodu sdílený *Microsoft.EntityFrameworkCore.Relational* balíček).</span><span class="sxs-lookup"><span data-stu-id="b09f2-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="b09f2-105">Dědičnost v modelu EF slouží k řízení, jak je reprezentovaná dědičnosti tříd entit v databázi.</span><span class="sxs-lookup"><span data-stu-id="b09f2-105">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

> [!NOTE]  
> <span data-ttu-id="b09f2-106">V současné době pouze vzor na hierarchii tabulky (TPH) je implementované v EF Core.</span><span class="sxs-lookup"><span data-stu-id="b09f2-106">Currently, only the table-per-hierarchy (TPH) pattern is implemented in EF Core.</span></span> <span data-ttu-id="b09f2-107">Za typ tabulky (TPT), jako jsou jiné běžné vzory a -za konkrétní – typ tabulky (TPC) ještě nejsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="b09f2-107">Other common patterns like table-per-type (TPT) and table-per-concrete-type (TPC) are not yet available.</span></span>

## <a name="conventions"></a><span data-ttu-id="b09f2-108">Konvence</span><span class="sxs-lookup"><span data-stu-id="b09f2-108">Conventions</span></span>

<span data-ttu-id="b09f2-109">Podle konvence se namapují dědičnosti pomocí vzoru za hierarchii tabulky (TPH).</span><span class="sxs-lookup"><span data-stu-id="b09f2-109">By convention, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="b09f2-110">TPH používá jednu tabulku pro ukládání dat pro všechny typy v hierarchii.</span><span class="sxs-lookup"><span data-stu-id="b09f2-110">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="b09f2-111">Sloupec diskriminátoru se používá k identifikaci typu každý řádek představuje.</span><span class="sxs-lookup"><span data-stu-id="b09f2-111">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="b09f2-112">EF Core bude pouze nastavení dědičnosti, dvou nebo více zděděných typů je explicitně zahrnuta v modelu (viz [dědičnosti](../inheritance.md) další podrobnosti).</span><span class="sxs-lookup"><span data-stu-id="b09f2-112">EF Core will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="b09f2-113">Tady je příklad ukazující scénář jednoduché dědičnosti a data uložená v tabulce relační databáze pomocí vzoru TPH.</span><span class="sxs-lookup"><span data-stu-id="b09f2-113">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="b09f2-114">*Diskriminátoru* typu, který identifikuje sloupce *blogu* je uložen v jednotlivých řádcích.</span><span class="sxs-lookup"><span data-stu-id="b09f2-114">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="b09f2-116">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="b09f2-116">Data Annotations</span></span>

<span data-ttu-id="b09f2-117">Datové poznámky nelze použít ke konfiguraci dědičnosti.</span><span class="sxs-lookup"><span data-stu-id="b09f2-117">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="b09f2-118">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="b09f2-118">Fluent API</span></span>

<span data-ttu-id="b09f2-119">Rozhraní Fluent API můžete použít ke konfiguraci názvu a typu sloupec diskriminátoru a hodnoty, které se používají k identifikaci jednotlivých typů v hierarchii.</span><span class="sxs-lookup"><span data-stu-id="b09f2-119">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

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

## <a name="configuring-the-discriminator-property"></a><span data-ttu-id="b09f2-120">Konfigurace vlastnost diskriminátoru</span><span class="sxs-lookup"><span data-stu-id="b09f2-120">Configuring the discriminator property</span></span>

<span data-ttu-id="b09f2-121">Ve výše uvedených příkladech diskriminátor vytvoří jako [stínové vlastnosti](xref:core/modeling/shadow-properties) na základní entitu v hierarchii.</span><span class="sxs-lookup"><span data-stu-id="b09f2-121">In the examples above, the discriminator is created as a [shadow property](xref:core/modeling/shadow-properties) on the base entity of the hierarchy.</span></span> <span data-ttu-id="b09f2-122">Protože je vlastnost v modelu, můžete ji nakonfigurovat, stejně jako ostatní vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="b09f2-122">Since it is a property in the model, it can be configured just like other properties.</span></span> <span data-ttu-id="b09f2-123">Chcete-li například nastavit maximální délky, pokud výchozí podle úmluvy diskriminátoru se používá:</span><span class="sxs-lookup"><span data-stu-id="b09f2-123">For example, to set the max length when the default, by-convention discriminator is being used:</span></span>

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

<span data-ttu-id="b09f2-124">Diskriminátor lze také mapovat na skutečné vlastnosti CLR ve vaší entitě.</span><span class="sxs-lookup"><span data-stu-id="b09f2-124">The discriminator can also be mapped to an actual CLR property in your entity.</span></span> <span data-ttu-id="b09f2-125">Příklad:</span><span class="sxs-lookup"><span data-stu-id="b09f2-125">For example:</span></span>
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

<span data-ttu-id="b09f2-126">Kombinace těchto dvou věcí společně je možné namapovat na reálném vlastnost diskriminátoru i ho nakonfigurovat:</span><span class="sxs-lookup"><span data-stu-id="b09f2-126">Combining these two things together it is possible to both map the discriminator to a real property and configure it:</span></span>
```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
