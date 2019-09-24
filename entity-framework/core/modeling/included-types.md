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
# <a name="including--excluding-types"></a>Zahrnutí a vyloučení typů

Zahrnutí typu v modelu znamená, že EF má metadata o tomto typu a pokusí se číst a zapisovat instance z/do databáze.

## <a name="conventions"></a>Konvence

Podle konvence jsou typy, které jsou `DbSet` vystaveny ve vlastnostech v kontextu, zahrnuty v modelu. Kromě toho jsou zahrnuty i typy, které jsou `OnModelCreating` uvedeny v metodě. Nakonec všechny typy, které jsou nalezeny rekurzivním zkoumáním navigačních vlastností zjištěných typů, jsou také zahrnuty v modelu.

**Například v následujícím kódu se objevují všechny tři typy:**

* `Blog`protože je zveřejněn ve `DbSet` vlastnosti v kontextu

* `Post`protože je zjištěna prostřednictvím `Blog.Posts` navigační vlastnosti

* `AuditEntry`protože je uveden v`OnModelCreating`

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

## <a name="data-annotations"></a>Datové poznámky

K vyloučení typu z modelu můžete použít datové poznámky.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít k vyloučení typu z modelu.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?highlight=12)]
