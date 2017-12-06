---
title: "Požadované a volitelné vlastnosti - EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
ms.technology: entity-framework-core
uid: core/modeling/required-optional
ms.openlocfilehash: 2af1d49e12ef980f81cb9c00556dee471673ccae
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="required-and-optional-properties"></a>Požadované a volitelné vlastnosti

Vlastnost považovat za volitelné, pokud je platná mohla obsahovat `null`. Pokud `null` není platná hodnota pro přiřazení vlastnosti a považuje se vyžaduje vlastnost.

## <a name="conventions"></a>Konvence

Podle konvence vlastnost s typem CLR může obsahovat hodnotu null bude nakonfigurován jako volitelná (`string`, `int?`, `byte[]`atd.). Nakonfiguruje s typem CLR nesmí obsahovat hodnotu null vlastnosti podle potřeby (`int`, `decimal`, `bool`atd.).

> [!NOTE]  
> Vlastnost s typem CLR nesmí obsahovat hodnotu null nelze konfigurovat jako volitelná. Vlastnost bude vždy považovat za vyžaduje Entity Framework.

## <a name="data-annotations"></a>Datových poznámek

Poznámky dat můžete použít k označení, že je vyžadována určitá vlastnost.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [Required]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít k označení, že je vyžadována určitá vlastnost.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Url)
            .IsRequired();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
