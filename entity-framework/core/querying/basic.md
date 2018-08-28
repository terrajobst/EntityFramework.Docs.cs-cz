---
title: Základní dotazy – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ab6e35f1-397f-41c0-9ef4-85aec5466377
uid: core/querying/basic
ms.openlocfilehash: 6a381f419cb0958ea0835070e22fe7a3212457d7
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993702"
---
# <a name="basic-queries"></a><span data-ttu-id="51518-102">Základní dotazy</span><span class="sxs-lookup"><span data-stu-id="51518-102">Basic Queries</span></span>

<span data-ttu-id="51518-103">Další informace o načtení entit z databáze pomocí Language Integrated Query (LINQ).</span><span class="sxs-lookup"><span data-stu-id="51518-103">Learn how to load entities from the database using Language Integrated Query (LINQ).</span></span>

> [!TIP]  
> <span data-ttu-id="51518-104">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="51518-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="101-linq-samples"></a><span data-ttu-id="51518-105">101 ukázek pro LINQ</span><span class="sxs-lookup"><span data-stu-id="51518-105">101 LINQ samples</span></span>

<span data-ttu-id="51518-106">Tato stránka zobrazuje několik příkladů běžných úloh s Entity Framework Core dosáhnout.</span><span class="sxs-lookup"><span data-stu-id="51518-106">This page shows a few examples to achieve common tasks with Entity Framework Core.</span></span> <span data-ttu-id="51518-107">Rozsáhlou sadu ukázky zobrazuje, jaké jsou možnosti s dotazy LINQ, naleznete v tématu [101 ukázek pro LINQ](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b).</span><span class="sxs-lookup"><span data-stu-id="51518-107">For an extensive set of samples showing what is possible with LINQ, see [101 LINQ Samples](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b).</span></span>

## <a name="loading-all-data"></a><span data-ttu-id="51518-108">Načítají se všechna data</span><span class="sxs-lookup"><span data-stu-id="51518-108">Loading all data</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.ToList();
}
```

## <a name="loading-a-single-entity"></a><span data-ttu-id="51518-109">Načtení jedné entity</span><span class="sxs-lookup"><span data-stu-id="51518-109">Loading a single entity</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Single(b => b.BlogId == 1);
}
```

## <a name="filtering"></a><span data-ttu-id="51518-110">Filtrování</span><span class="sxs-lookup"><span data-stu-id="51518-110">Filtering</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .Where(b => b.Url.Contains("dotnet"))
        .ToList();
}
```
