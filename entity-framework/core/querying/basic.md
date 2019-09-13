---
title: Základní dotazy – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ab6e35f1-397f-41c0-9ef4-85aec5466377
uid: core/querying/basic
ms.openlocfilehash: 49daa0d37175244617993cc6e911cbd59d27079f
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921744"
---
# <a name="basic-queries"></a><span data-ttu-id="34b37-102">Základní dotazy</span><span class="sxs-lookup"><span data-stu-id="34b37-102">Basic Queries</span></span>

<span data-ttu-id="34b37-103">Přečtěte si, jak načíst entity z databáze pomocí jazyka LINQ (Language Integrated Query).</span><span class="sxs-lookup"><span data-stu-id="34b37-103">Learn how to load entities from the database using Language Integrated Query (LINQ).</span></span>

> [!TIP]  
> <span data-ttu-id="34b37-104">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="34b37-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="101-linq-samples"></a><span data-ttu-id="34b37-105">Ukázky 101 LINQ</span><span class="sxs-lookup"><span data-stu-id="34b37-105">101 LINQ samples</span></span>

<span data-ttu-id="34b37-106">Tato stránka obsahuje několik příkladů, které vám pomůžou dosáhnout běžných úloh Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="34b37-106">This page shows a few examples to achieve common tasks with Entity Framework Core.</span></span> <span data-ttu-id="34b37-107">Rozsáhlou sadu ukázek, které ukazují, co je možné v LINQ, najdete v článku [101 LINQ Samples](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b).</span><span class="sxs-lookup"><span data-stu-id="34b37-107">For an extensive set of samples showing what is possible with LINQ, see [101 LINQ Samples](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b).</span></span>

## <a name="loading-all-data"></a><span data-ttu-id="34b37-108">Načítání všech dat</span><span class="sxs-lookup"><span data-stu-id="34b37-108">Loading all data</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.ToList();
}
```

## <a name="loading-a-single-entity"></a><span data-ttu-id="34b37-109">Načítá se jedna entita.</span><span class="sxs-lookup"><span data-stu-id="34b37-109">Loading a single entity</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Single(b => b.BlogId == 1);
}
```

## <a name="filtering"></a><span data-ttu-id="34b37-110">Filtrování</span><span class="sxs-lookup"><span data-stu-id="34b37-110">Filtering</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .Where(b => b.Url.Contains("dotnet"))
        .ToList();
}
```
