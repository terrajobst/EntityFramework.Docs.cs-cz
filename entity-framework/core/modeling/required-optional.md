---
title: Požadované a volitelné vlastnosti – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: b6716a5b03e1afc2933e317d606ef50f986c22c7
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995494"
---
# <a name="required-and-optional-properties"></a><span data-ttu-id="a0865-102">Požadované a volitelné vlastnosti</span><span class="sxs-lookup"><span data-stu-id="a0865-102">Required and Optional Properties</span></span>

<span data-ttu-id="a0865-103">Vlastnost se považuje za nepovinné, pokud je platný pro něj tak, aby obsahovala `null`.</span><span class="sxs-lookup"><span data-stu-id="a0865-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="a0865-104">Pokud `null` není platná hodnota pro přiřazení vlastnosti a má se za to se vyžaduje vlastnost.</span><span class="sxs-lookup"><span data-stu-id="a0865-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

## <a name="conventions"></a><span data-ttu-id="a0865-105">Konvence</span><span class="sxs-lookup"><span data-stu-id="a0865-105">Conventions</span></span>

<span data-ttu-id="a0865-106">Podle konvence, vlastnost, jejíž typ CLR může obsahovat hodnotu null se nakonfigurují jako volitelné (`string`, `int?`, `byte[]`atd.).</span><span class="sxs-lookup"><span data-stu-id="a0865-106">By convention, a property whose CLR type can contain null will be configured as optional (`string`, `int?`, `byte[]`, etc.).</span></span> <span data-ttu-id="a0865-107">Vlastnosti, jejíž typ CLR nesmí obsahovat hodnotu null se nakonfigurují podle potřeby (`int`, `decimal`, `bool`atd.).</span><span class="sxs-lookup"><span data-stu-id="a0865-107">Properties whose CLR type cannot contain null will be configured as required (`int`, `decimal`, `bool`, etc.).</span></span>

> [!NOTE]  
> <span data-ttu-id="a0865-108">Vlastnost, jejíž typ CLR nemůže obsahovat hodnotu null nejde nakonfigurovat jako volitelné.</span><span class="sxs-lookup"><span data-stu-id="a0865-108">A property whose CLR type cannot contain null cannot be configured as optional.</span></span> <span data-ttu-id="a0865-109">Vlastnost budou vždy považovat za vyžadovány rozhraním Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a0865-109">The property will always be considered required by Entity Framework.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="a0865-110">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="a0865-110">Data Annotations</span></span>

<span data-ttu-id="a0865-111">Anotací dat můžete použít k označení, že je vyžadována určitá vlastnost.</span><span class="sxs-lookup"><span data-stu-id="a0865-111">You can use Data Annotations to indicate that a property is required.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [Required]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="a0865-112">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="a0865-112">Fluent API</span></span>

<span data-ttu-id="a0865-113">Rozhraní Fluent API můžete použít k označení, že je vyžadována určitá vlastnost.</span><span class="sxs-lookup"><span data-stu-id="a0865-113">You can use the Fluent API to indicate that a property is required.</span></span>

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
