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
# <a name="including--excluding-properties"></a><span data-ttu-id="de6e7-102">Zahrnutí a vyloučení vlastností</span><span class="sxs-lookup"><span data-stu-id="de6e7-102">Including & Excluding Properties</span></span>

<span data-ttu-id="de6e7-103">Včetně vlastnost v modelu znamená, že EF má metadata o tuto vlastnost a pokusí se čtení a zápis dat z/do databáze.</span><span class="sxs-lookup"><span data-stu-id="de6e7-103">Including a property in the model means that EF has metadata about that property and will attempt to read and write values from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="de6e7-104">Konvence</span><span class="sxs-lookup"><span data-stu-id="de6e7-104">Conventions</span></span>

<span data-ttu-id="de6e7-105">Podle konvence se zahrnou veřejné vlastnosti metody getter a setter v modelu.</span><span class="sxs-lookup"><span data-stu-id="de6e7-105">By convention, public properties with a getter and a setter will be included in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="de6e7-106">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="de6e7-106">Data Annotations</span></span>

<span data-ttu-id="de6e7-107">Datové poznámky můžete být z modelu vyloučena určitá vlastnost.</span><span class="sxs-lookup"><span data-stu-id="de6e7-107">You can use Data Annotations to exclude a property from the model.</span></span>

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

## <a name="fluent-api"></a><span data-ttu-id="de6e7-108">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="de6e7-108">Fluent API</span></span>

<span data-ttu-id="de6e7-109">Rozhraní Fluent API můžete být z modelu vyloučena určitá vlastnost.</span><span class="sxs-lookup"><span data-stu-id="de6e7-109">You can use the Fluent API to exclude a property from the model.</span></span>

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
