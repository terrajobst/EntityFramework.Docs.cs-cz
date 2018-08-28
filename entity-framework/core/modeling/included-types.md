---
title: Zahrnutí a vyloučení typů – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: a5a14f62524754fed179e9a41fac5e29faf185ca
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996147"
---
# <a name="including--excluding-types"></a>Zahrnutí a vyloučení typů

Včetně typu modelu znamená, že EF má metadata o typ, který se pokusí o čtení a zápis instance z/do databáze.

## <a name="conventions"></a>Konvence

Podle konvence, typy, které jsou vystaveny `DbSet` vlastnosti na kontext jsou součástí modelu. Kromě toho, typy, které jsou uvedeny v `OnModelCreating` metody jsou zahrnuté také. A konečně všechny typy, které se nacházejí ve rekurzivně zkoumání navigační vlastnosti zjištěných typů jsou taky součástí modelu.

**Například v následující ukázce kódu zjištěny všechny tři typy:**

* `Blog` protože je zpřístupněná `DbSet` vlastnost pro daný kontext

* `Post` protože je zjištěna prostřednictvím `Blog.Posts` navigační vlastnost

* `AuditEntry` vzhledem k tomu, že je uvedený v `OnModelCreating`

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

## <a name="data-annotations"></a>Datové poznámky

Anotací dat můžete použít k vyloučení typu z modelu.

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

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít k vyloučení typu z modelu.

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
