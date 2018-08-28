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
# <a name="alternate-keys"></a><span data-ttu-id="9d11e-102">Alternativní klíče</span><span class="sxs-lookup"><span data-stu-id="9d11e-102">Alternate Keys</span></span>

<span data-ttu-id="9d11e-103">Alternativní klíče slouží jako alternativní jedinečný identifikátor pro každou instanci entity kromě primární klíč.</span><span class="sxs-lookup"><span data-stu-id="9d11e-103">An alternate key serves as an alternate unique identifier for each entity instance in addition to the primary key.</span></span> <span data-ttu-id="9d11e-104">Alternativní klíče můžete použít jako cíl relace.</span><span class="sxs-lookup"><span data-stu-id="9d11e-104">Alternate keys can be used as the target of a relationship.</span></span> <span data-ttu-id="9d11e-105">Při používání relační databáze mapuje koncept jedinečný index nebo omezení na sloupce alternativního klíče a jeden nebo více omezení cizího klíče, které odkazují na sloupce.</span><span class="sxs-lookup"><span data-stu-id="9d11e-105">When using a relational database this maps to the concept of a unique index/constraint on the alternate key column(s) and one or more foreign key constraints that reference the column(s).</span></span>

> [!TIP]  
> <span data-ttu-id="9d11e-106">Pokud chcete vynutit jedinečnosti sloupce, pak je třeba jedinečný index místo alternativního klíče, přečtěte si téma [indexy](indexes.md).</span><span class="sxs-lookup"><span data-stu-id="9d11e-106">If you just want to enforce uniqueness of a column then you want a unique index rather than an alternate key, see [Indexes](indexes.md).</span></span> <span data-ttu-id="9d11e-107">V EF alternativní klíče poskytnout více funkcí než jedinečné indexy, protože slouží jako cíl cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="9d11e-107">In EF, alternate keys provide greater functionality than unique indexes because they can be used as the target of a foreign key.</span></span>

<span data-ttu-id="9d11e-108">Alternativní klíče jsou obvykle vložena za vás v případě potřeby a není potřeba nakonfigurovat ručně.</span><span class="sxs-lookup"><span data-stu-id="9d11e-108">Alternate keys are typically introduced for you when needed and you do not need to manually configure them.</span></span> <span data-ttu-id="9d11e-109">Zobrazit [konvence](#conventions) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="9d11e-109">See [Conventions](#conventions) for more details.</span></span>

## <a name="conventions"></a><span data-ttu-id="9d11e-110">Konvence</span><span class="sxs-lookup"><span data-stu-id="9d11e-110">Conventions</span></span>

<span data-ttu-id="9d11e-111">Podle úmluvy je zavedená alternativního klíče pro vás při určování vlastnost, která není primární klíč jako cíl vztahu.</span><span class="sxs-lookup"><span data-stu-id="9d11e-111">By convention, an alternate key is introduced for you when you identify a property, that is not the primary key, as the target of a relationship.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="9d11e-112">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="9d11e-112">Data Annotations</span></span>

<span data-ttu-id="9d11e-113">Alternativní klíče nelze konfigurovat pomocí datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="9d11e-113">Alternate keys can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="9d11e-114">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="9d11e-114">Fluent API</span></span>

<span data-ttu-id="9d11e-115">Rozhraní Fluent API můžete použít ke konfiguraci jedné vlastnosti alternativního klíče.</span><span class="sxs-lookup"><span data-stu-id="9d11e-115">You can use the Fluent API to configure a single property to be an alternate key.</span></span>

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

<span data-ttu-id="9d11e-116">Můžete také použít rozhraní Fluent API nakonfigurovat několik vlastností tak, aby alternativního klíče (označované jako složený klíč alternativní).</span><span class="sxs-lookup"><span data-stu-id="9d11e-116">You can also use the Fluent API to configure multiple properties to be an alternate key (known as a composite alternate key).</span></span>

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
