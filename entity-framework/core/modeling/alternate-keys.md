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
# <a name="alternate-keys"></a><span data-ttu-id="67ee0-102">Alternativní klíče</span><span class="sxs-lookup"><span data-stu-id="67ee0-102">Alternate Keys</span></span>

<span data-ttu-id="67ee0-103">Alternativní klíč slouží jako alternativní jedinečný identifikátor pro každou instanci entity kromě primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="67ee0-103">An alternate key serves as an alternate unique identifier for each entity instance in addition to the primary key.</span></span> <span data-ttu-id="67ee0-104">Alternativní klíče lze použít jako cíl relace.</span><span class="sxs-lookup"><span data-stu-id="67ee0-104">Alternate keys can be used as the target of a relationship.</span></span> <span data-ttu-id="67ee0-105">Při použití relační databáze se tato mapa mapuje na koncept jedinečného indexu nebo omezení pro sloupce nebo sloupce alternativních klíčů a jedno nebo více omezení cizího klíče, které odkazují na sloupce.</span><span class="sxs-lookup"><span data-stu-id="67ee0-105">When using a relational database this maps to the concept of a unique index/constraint on the alternate key column(s) and one or more foreign key constraints that reference the column(s).</span></span>

> [!TIP]  
> <span data-ttu-id="67ee0-106">Pokud chcete vymáhat jedinečnost sloupce a chcete místo alternativního klíče vyhovět jedinečným indexům, přečtěte si téma [indexy](indexes.md).</span><span class="sxs-lookup"><span data-stu-id="67ee0-106">If you just want to enforce uniqueness of a column then you want a unique index rather than an alternate key, see [Indexes](indexes.md).</span></span> <span data-ttu-id="67ee0-107">V EF poskytují alternativní klíče větší funkce než jedinečné indexy, protože je lze použít jako cíl cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="67ee0-107">In EF, alternate keys provide greater functionality than unique indexes because they can be used as the target of a foreign key.</span></span>

<span data-ttu-id="67ee0-108">V případě potřeby jsou obvykle zavedeny alternativní klíče a není nutné je ručně konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="67ee0-108">Alternate keys are typically introduced for you when needed and you do not need to manually configure them.</span></span> <span data-ttu-id="67ee0-109">Další podrobnosti najdete v tématu věnovaném [konvencím](#conventions) .</span><span class="sxs-lookup"><span data-stu-id="67ee0-109">See [Conventions](#conventions) for more details.</span></span>

## <a name="conventions"></a><span data-ttu-id="67ee0-110">Konvence</span><span class="sxs-lookup"><span data-stu-id="67ee0-110">Conventions</span></span>

<span data-ttu-id="67ee0-111">Podle konvence se pro vás zavádí alternativní klíč, pokud identifikujete vlastnost, která není primárním klíčem, jako cíl relace.</span><span class="sxs-lookup"><span data-stu-id="67ee0-111">By convention, an alternate key is introduced for you when you identify a property, that is not the primary key, as the target of a relationship.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="67ee0-112">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="67ee0-112">Data Annotations</span></span>

<span data-ttu-id="67ee0-113">Alternativní klíče nelze konfigurovat pomocí datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="67ee0-113">Alternate keys can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="67ee0-114">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="67ee0-114">Fluent API</span></span>

<span data-ttu-id="67ee0-115">Pomocí rozhraní Fluent API můžete nakonfigurovat jednu vlastnost jako alternativní klíč.</span><span class="sxs-lookup"><span data-stu-id="67ee0-115">You can use the Fluent API to configure a single property to be an alternate key.</span></span>

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

<span data-ttu-id="67ee0-116">Rozhraní Fluent API můžete použít také ke konfiguraci více vlastností, aby byl alternativním klíčem (označovaným jako složený alternativní klíč).</span><span class="sxs-lookup"><span data-stu-id="67ee0-116">You can also use the Fluent API to configure multiple properties to be an alternate key (known as a composite alternate key).</span></span>

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
