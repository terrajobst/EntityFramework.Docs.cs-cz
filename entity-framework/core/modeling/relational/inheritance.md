---
title: Dědičnost (relační databáze) – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 381d1878007bb78b359eb49649f4356f1e5eb04a
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655634"
---
# <a name="inheritance-relational-database"></a><span data-ttu-id="91b88-102">Dědičnost (relační databáze)</span><span class="sxs-lookup"><span data-stu-id="91b88-102">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="91b88-103">Konfigurace v této části platí pro relační databáze obecně.</span><span class="sxs-lookup"><span data-stu-id="91b88-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="91b88-104">Metody rozšíření zobrazené tady budou k dispozici při instalaci zprostředkovatele relační databáze (z důvodu sdíleného balíčku *Microsoft. EntityFrameworkCore. relační* ).</span><span class="sxs-lookup"><span data-stu-id="91b88-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="91b88-105">Dědičnost v modelu EF se používá k řízení způsobu reprezentace dědičnosti tříd entit v databázi.</span><span class="sxs-lookup"><span data-stu-id="91b88-105">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

> [!NOTE]  
> <span data-ttu-id="91b88-106">V současné době je v EF Core implementován pouze vzor Table-per-Hierarchy (TPH).</span><span class="sxs-lookup"><span data-stu-id="91b88-106">Currently, only the table-per-hierarchy (TPH) pattern is implemented in EF Core.</span></span> <span data-ttu-id="91b88-107">Další běžné vzorce, jako je například tabulka na typ (TPT) a tabulka-podle konkrétního typu (TPC), ještě nejsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="91b88-107">Other common patterns like table-per-type (TPT) and table-per-concrete-type (TPC) are not yet available.</span></span>

## <a name="conventions"></a><span data-ttu-id="91b88-108">Konvence</span><span class="sxs-lookup"><span data-stu-id="91b88-108">Conventions</span></span>

<span data-ttu-id="91b88-109">Podle konvence se dědičnost mapuje pomocí vzoru Table-per-Hierarchy (TPH).</span><span class="sxs-lookup"><span data-stu-id="91b88-109">By convention, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="91b88-110">K ukládání dat pro všechny typy v hierarchii používá typ TPH jednu tabulku.</span><span class="sxs-lookup"><span data-stu-id="91b88-110">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="91b88-111">Sloupec diskriminátoru slouží k identifikaci typu, který každý řádek představuje.</span><span class="sxs-lookup"><span data-stu-id="91b88-111">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="91b88-112">EF Core bude dědění nastavení pouze v případě, že jsou do modelu explicitně zahrnuty dva nebo více děděných typů (další podrobnosti naleznete v tématu [Dědičnost](../inheritance.md) ).</span><span class="sxs-lookup"><span data-stu-id="91b88-112">EF Core will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="91b88-113">Níže je příklad znázorňující jednoduchý scénář dědičnosti a data uložená v relační tabulce databáze pomocí vzoru TPH.</span><span class="sxs-lookup"><span data-stu-id="91b88-113">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="91b88-114">Sloupec *diskriminátor* určuje, který typ *blogu* je uložený v každém řádku.</span><span class="sxs-lookup"><span data-stu-id="91b88-114">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs#Model)]

![obrázek](_static/inheritance-tph-data.png)

>[!NOTE]
> <span data-ttu-id="91b88-116">Sloupce databáze se při použití mapování typu TPH automaticky v případě potřeby nepovolují s možnou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="91b88-116">Database columns are automatically made nullable as necessary when using TPH mapping.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="91b88-117">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="91b88-117">Data Annotations</span></span>

<span data-ttu-id="91b88-118">Ke konfiguraci dědičnosti nelze použít datové poznámky.</span><span class="sxs-lookup"><span data-stu-id="91b88-118">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="91b88-119">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="91b88-119">Fluent API</span></span>

<span data-ttu-id="91b88-120">Rozhraní Fluent API můžete použít ke konfiguraci názvu a typu sloupce diskriminátoru a hodnot, které se používají k identifikaci každého typu v hierarchii.</span><span class="sxs-lookup"><span data-stu-id="91b88-120">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/InheritanceTPHDiscriminator.cs#Inheritance)]

## <a name="configuring-the-discriminator-property"></a><span data-ttu-id="91b88-121">Konfigurace vlastnosti diskriminátoru</span><span class="sxs-lookup"><span data-stu-id="91b88-121">Configuring the discriminator property</span></span>

<span data-ttu-id="91b88-122">V předchozích příkladech je diskriminátor vytvořen jako [vlastnost Shadow](xref:core/modeling/shadow-properties) v základní entitě hierarchie.</span><span class="sxs-lookup"><span data-stu-id="91b88-122">In the examples above, the discriminator is created as a [shadow property](xref:core/modeling/shadow-properties) on the base entity of the hierarchy.</span></span> <span data-ttu-id="91b88-123">Vzhledem k tomu, že se jedná o vlastnost v modelu, lze ji nakonfigurovat stejně jako jiné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="91b88-123">Since it is a property in the model, it can be configured just like other properties.</span></span> <span data-ttu-id="91b88-124">Například pro nastavení maximální délky při použití výchozího diskriminátoru podle konvence:</span><span class="sxs-lookup"><span data-stu-id="91b88-124">For example, to set the max length when the default, by-convention discriminator is being used:</span></span>

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

<span data-ttu-id="91b88-125">Diskriminátor lze také namapovat na skutečnou vlastnost CLR v entitě.</span><span class="sxs-lookup"><span data-stu-id="91b88-125">The discriminator can also be mapped to an actual CLR property in your entity.</span></span> <span data-ttu-id="91b88-126">Příklad:</span><span class="sxs-lookup"><span data-stu-id="91b88-126">For example:</span></span>

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

<span data-ttu-id="91b88-127">Kombinování těchto dvou věcí je možné namapovat mezi diskriminátory na skutečnou vlastnost a nakonfigurovat ji:</span><span class="sxs-lookup"><span data-stu-id="91b88-127">Combining these two things together it is possible to both map the discriminator to a real property and configure it:</span></span>

```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
