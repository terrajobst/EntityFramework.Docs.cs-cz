---
title: Zahrnutí a vyloučení typů – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: f533b24312af37634ce4957e43c39ce776bf0bf0
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929794"
---
# <a name="including--excluding-types"></a><span data-ttu-id="ec49a-102">Zahrnutí a vyloučení typů</span><span class="sxs-lookup"><span data-stu-id="ec49a-102">Including & Excluding Types</span></span>

<span data-ttu-id="ec49a-103">Včetně typu modelu znamená, že EF má metadata o typ, který se pokusí o čtení a zápis instance z/do databáze.</span><span class="sxs-lookup"><span data-stu-id="ec49a-103">Including a type in the model means that EF has metadata about that type and will attempt to read and write instances from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="ec49a-104">Konvence</span><span class="sxs-lookup"><span data-stu-id="ec49a-104">Conventions</span></span>

<span data-ttu-id="ec49a-105">Podle konvence, typy, které jsou vystaveny `DbSet` vlastnosti na kontext jsou součástí modelu.</span><span class="sxs-lookup"><span data-stu-id="ec49a-105">By convention, types that are exposed in `DbSet` properties on your context are included in your model.</span></span> <span data-ttu-id="ec49a-106">Kromě toho, typy, které jsou uvedeny v `OnModelCreating` metody jsou zahrnuté také.</span><span class="sxs-lookup"><span data-stu-id="ec49a-106">In addition, types that are mentioned in the `OnModelCreating` method are also included.</span></span> <span data-ttu-id="ec49a-107">A konečně všechny typy, které se nacházejí ve rekurzivně zkoumání navigační vlastnosti zjištěných typů jsou taky součástí modelu.</span><span class="sxs-lookup"><span data-stu-id="ec49a-107">Finally, any types that are found by recursively exploring the navigation properties of discovered types are also included in the model.</span></span>

<span data-ttu-id="ec49a-108">**Například v následující ukázce kódu zjištěny všechny tři typy:**</span><span class="sxs-lookup"><span data-stu-id="ec49a-108">**For example, in the following code listing all three types are discovered:**</span></span>

* <span data-ttu-id="ec49a-109">`Blog` protože je zpřístupněná `DbSet` vlastnost pro daný kontext</span><span class="sxs-lookup"><span data-stu-id="ec49a-109">`Blog` because it is exposed in a `DbSet` property on the context</span></span>

* <span data-ttu-id="ec49a-110">`Post` protože je zjištěna prostřednictvím `Blog.Posts` navigační vlastnost</span><span class="sxs-lookup"><span data-stu-id="ec49a-110">`Post` because it is discovered via the `Blog.Posts` navigation property</span></span>

* <span data-ttu-id="ec49a-111">`AuditEntry` vzhledem k tomu, že je uvedený v `OnModelCreating`</span><span class="sxs-lookup"><span data-stu-id="ec49a-111">`AuditEntry` because it is mentioned in `OnModelCreating`</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/IncludedTypes.cs?highlight=3,7,16)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<AuditEntry>();
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

    public Blog Blog { get; set; }
}

public class AuditEntry
{
    public int AuditEntryId { get; set; }
    public string Username { get; set; }
    public string Action { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="ec49a-112">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="ec49a-112">Data Annotations</span></span>

<span data-ttu-id="ec49a-113">Anotací dat můžete použít k vyloučení typu z modelu.</span><span class="sxs-lookup"><span data-stu-id="ec49a-113">You can use Data Annotations to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a><span data-ttu-id="ec49a-114">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="ec49a-114">Fluent API</span></span>

<span data-ttu-id="ec49a-115">Rozhraní Fluent API můžete použít k vyloučení typu z modelu.</span><span class="sxs-lookup"><span data-stu-id="ec49a-115">You can use the Fluent API to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/IgnoreType.cs?highlight=12)]
