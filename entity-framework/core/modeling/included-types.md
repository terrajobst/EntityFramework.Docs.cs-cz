---
title: "Včetně & vyloučení typů - EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
ms.technology: entity-framework-core
uid: core/modeling/included-types
ms.openlocfilehash: a8d7293a144968d2506bdcc76e55a1a0b1e3fd4b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="including--excluding-types"></a><span data-ttu-id="8a8eb-102">Včetně & vyloučení typů</span><span class="sxs-lookup"><span data-stu-id="8a8eb-102">Including & Excluding Types</span></span>

<span data-ttu-id="8a8eb-103">Včetně typu ve model znamená, že EF má metadata o, zadejte a se pokusí číst a zapisovat instancí z/do databáze.</span><span class="sxs-lookup"><span data-stu-id="8a8eb-103">Including a type in the model means that EF has metadata about that type and will attempt to read and write instances from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="8a8eb-104">Konvence</span><span class="sxs-lookup"><span data-stu-id="8a8eb-104">Conventions</span></span>

<span data-ttu-id="8a8eb-105">Podle konvence, typy, které jsou zveřejněné v `DbSet` vlastnosti na váš kontext jsou součástí modelu.</span><span class="sxs-lookup"><span data-stu-id="8a8eb-105">By convention, types that are exposed in `DbSet` properties on your context are included in your model.</span></span> <span data-ttu-id="8a8eb-106">Kromě toho typy, které jsou uvedeny v `OnModelCreating` metoda jsou také zahrnuty.</span><span class="sxs-lookup"><span data-stu-id="8a8eb-106">In addition, types that are mentioned in the `OnModelCreating` method are also included.</span></span> <span data-ttu-id="8a8eb-107">Nakonec všechny typy, které se nacházejí ve rekurzivně zkoumat navigační vlastnosti zjištěné typy jsou také uvedené v modelu.</span><span class="sxs-lookup"><span data-stu-id="8a8eb-107">Finally, any types that are found by recursively exploring the navigation properties of discovered types are also included in the model.</span></span>

<span data-ttu-id="8a8eb-108">**Například v následující ukázce kódu jsou zjištěny všechny tři typy:**</span><span class="sxs-lookup"><span data-stu-id="8a8eb-108">**For example, in the following code listing all three types are discovered:**</span></span>

* <span data-ttu-id="8a8eb-109">`Blog`protože je zpřístupněná `DbSet` vlastnost pro daný kontext</span><span class="sxs-lookup"><span data-stu-id="8a8eb-109">`Blog` because it is exposed in a `DbSet` property on the context</span></span>

* <span data-ttu-id="8a8eb-110">`Post`protože je zjištěna prostřednictvím `Blog.Posts` navigační vlastnost</span><span class="sxs-lookup"><span data-stu-id="8a8eb-110">`Post` because it is discovered via the `Blog.Posts` navigation property</span></span>

* <span data-ttu-id="8a8eb-111">`AuditEntry`vzhledem k tomu, že je uvedený v`OnModelCreating`</span><span class="sxs-lookup"><span data-stu-id="8a8eb-111">`AuditEntry` because it is mentioned in `OnModelCreating`</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="8a8eb-112">Datových poznámek</span><span class="sxs-lookup"><span data-stu-id="8a8eb-112">Data Annotations</span></span>

<span data-ttu-id="8a8eb-113">Můžete vyloučit typ z modelu datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="8a8eb-113">You can use Data Annotations to exclude a type from the model.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/IgnoreType.cs?highlight=9)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogMetadata Metadata { get; set; }
}

[NotMapped]
public class BlogMetadata
{
    public DateTime LoadedFromDatabase { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="8a8eb-114">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="8a8eb-114">Fluent API</span></span>

<span data-ttu-id="8a8eb-115">Rozhraní Fluent API můžete použít k vyloučení typu z modelu.</span><span class="sxs-lookup"><span data-stu-id="8a8eb-115">You can use the Fluent API to exclude a type from the model.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IgnoreType.cs?highlight=7)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Ignore<BlogMetadata>();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogMetadata Metadata { get; set; }
}

public class BlogMetadata
{
    public DateTime LoadedFromDatabase { get; set; }
}
```
