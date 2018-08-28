---
title: Zahrnutí a vyloučení vlastností – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/included-properties
ms.openlocfilehash: 07b70e4517b67490e04a9ec9fa22b9b5d5217681
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998252"
---
# <a name="including--excluding-properties"></a>Zahrnutí a vyloučení vlastností

Včetně vlastnost v modelu znamená, že EF má metadata o tuto vlastnost a pokusí se čtení a zápis dat z/do databáze.

## <a name="conventions"></a>Konvence

Podle konvence se zahrnou veřejné vlastnosti metody getter a setter v modelu.

## <a name="data-annotations"></a>Datové poznámky

Datové poznámky můžete být z modelu vyloučena určitá vlastnost.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/IgnoreProperty.cs?highlight=6)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    [NotMapped]
    public DateTime LoadedFromDatabase { get; set; }
}
```

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete být z modelu vyloučena určitá vlastnost.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IgnoreProperty.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Ignore(b => b.LoadedFromDatabase);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public DateTime LoadedFromDatabase { get; set; }
}
```
