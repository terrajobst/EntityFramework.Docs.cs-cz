---
title: Zahrnutí & s výjimkou typů – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: ca83b1c432bdf4853dba81e12ec4a739bc8218dc
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197374"
---
# <a name="including--excluding-types"></a><span data-ttu-id="9aca9-102">Zahrnutí a vyloučení typů</span><span class="sxs-lookup"><span data-stu-id="9aca9-102">Including & Excluding Types</span></span>

<span data-ttu-id="9aca9-103">Zahrnutí typu v modelu znamená, že EF má metadata o tomto typu a pokusí se číst a zapisovat instance z/do databáze.</span><span class="sxs-lookup"><span data-stu-id="9aca9-103">Including a type in the model means that EF has metadata about that type and will attempt to read and write instances from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="9aca9-104">Konvence</span><span class="sxs-lookup"><span data-stu-id="9aca9-104">Conventions</span></span>

<span data-ttu-id="9aca9-105">Podle konvence jsou typy, které jsou `DbSet` vystaveny ve vlastnostech v kontextu, zahrnuty v modelu.</span><span class="sxs-lookup"><span data-stu-id="9aca9-105">By convention, types that are exposed in `DbSet` properties on your context are included in your model.</span></span> <span data-ttu-id="9aca9-106">Kromě toho jsou zahrnuty i typy, které jsou `OnModelCreating` uvedeny v metodě.</span><span class="sxs-lookup"><span data-stu-id="9aca9-106">In addition, types that are mentioned in the `OnModelCreating` method are also included.</span></span> <span data-ttu-id="9aca9-107">Nakonec všechny typy, které jsou nalezeny rekurzivním zkoumáním navigačních vlastností zjištěných typů, jsou také zahrnuty v modelu.</span><span class="sxs-lookup"><span data-stu-id="9aca9-107">Finally, any types that are found by recursively exploring the navigation properties of discovered types are also included in the model.</span></span>

<span data-ttu-id="9aca9-108">**Například v následujícím kódu se objevují všechny tři typy:**</span><span class="sxs-lookup"><span data-stu-id="9aca9-108">**For example, in the following code listing all three types are discovered:**</span></span>

* <span data-ttu-id="9aca9-109">`Blog`protože je zveřejněn ve `DbSet` vlastnosti v kontextu</span><span class="sxs-lookup"><span data-stu-id="9aca9-109">`Blog` because it is exposed in a `DbSet` property on the context</span></span>

* <span data-ttu-id="9aca9-110">`Post`protože je zjištěna prostřednictvím `Blog.Posts` navigační vlastnosti</span><span class="sxs-lookup"><span data-stu-id="9aca9-110">`Post` because it is discovered via the `Blog.Posts` navigation property</span></span>

* <span data-ttu-id="9aca9-111">`AuditEntry`protože je uveden v`OnModelCreating`</span><span class="sxs-lookup"><span data-stu-id="9aca9-111">`AuditEntry` because it is mentioned in `OnModelCreating`</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/IncludedTypes.cs?highlight=3,7,16)] -->
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

## <a name="data-annotations"></a><span data-ttu-id="9aca9-112">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="9aca9-112">Data Annotations</span></span>

<span data-ttu-id="9aca9-113">K vyloučení typu z modelu můžete použít datové poznámky.</span><span class="sxs-lookup"><span data-stu-id="9aca9-113">You can use Data Annotations to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a><span data-ttu-id="9aca9-114">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="9aca9-114">Fluent API</span></span>

<span data-ttu-id="9aca9-115">Rozhraní Fluent API můžete použít k vyloučení typu z modelu.</span><span class="sxs-lookup"><span data-stu-id="9aca9-115">You can use the Fluent API to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?highlight=12)]
