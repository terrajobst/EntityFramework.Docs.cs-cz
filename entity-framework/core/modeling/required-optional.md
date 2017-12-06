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
# <a name="required-and-optional-properties"></a><span data-ttu-id="c17fe-102">Požadované a volitelné vlastnosti</span><span class="sxs-lookup"><span data-stu-id="c17fe-102">Required and Optional Properties</span></span>

<span data-ttu-id="c17fe-103">Vlastnost považovat za volitelné, pokud je platná mohla obsahovat `null`.</span><span class="sxs-lookup"><span data-stu-id="c17fe-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="c17fe-104">Pokud `null` není platná hodnota pro přiřazení vlastnosti a považuje se vyžaduje vlastnost.</span><span class="sxs-lookup"><span data-stu-id="c17fe-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

## <a name="conventions"></a><span data-ttu-id="c17fe-105">Konvence</span><span class="sxs-lookup"><span data-stu-id="c17fe-105">Conventions</span></span>

<span data-ttu-id="c17fe-106">Podle konvence vlastnost s typem CLR může obsahovat hodnotu null bude nakonfigurován jako volitelná (`string`, `int?`, `byte[]`atd.).</span><span class="sxs-lookup"><span data-stu-id="c17fe-106">By convention, a property whose CLR type can contain null will be configured as optional (`string`, `int?`, `byte[]`, etc.).</span></span> <span data-ttu-id="c17fe-107">Nakonfiguruje s typem CLR nesmí obsahovat hodnotu null vlastnosti podle potřeby (`int`, `decimal`, `bool`atd.).</span><span class="sxs-lookup"><span data-stu-id="c17fe-107">Properties whose CLR type cannot contain null will be configured as required (`int`, `decimal`, `bool`, etc.).</span></span>

> [!NOTE]  
> <span data-ttu-id="c17fe-108">Vlastnost s typem CLR nesmí obsahovat hodnotu null nelze konfigurovat jako volitelná.</span><span class="sxs-lookup"><span data-stu-id="c17fe-108">A property whose CLR type cannot contain null cannot be configured as optional.</span></span> <span data-ttu-id="c17fe-109">Vlastnost bude vždy považovat za vyžaduje Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c17fe-109">The property will always be considered required by Entity Framework.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="c17fe-110">Datových poznámek</span><span class="sxs-lookup"><span data-stu-id="c17fe-110">Data Annotations</span></span>

<span data-ttu-id="c17fe-111">Poznámky dat můžete použít k označení, že je vyžadována určitá vlastnost.</span><span class="sxs-lookup"><span data-stu-id="c17fe-111">You can use Data Annotations to indicate that a property is required.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [Required]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="c17fe-112">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="c17fe-112">Fluent API</span></span>

<span data-ttu-id="c17fe-113">Rozhraní Fluent API můžete použít k označení, že je vyžadována určitá vlastnost.</span><span class="sxs-lookup"><span data-stu-id="c17fe-113">You can use the Fluent API to indicate that a property is required.</span></span>

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
