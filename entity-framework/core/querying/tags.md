---
title: Značky dotazu – EF Core
author: divega
ms.date: 11/14/2018
ms.assetid: 73C7A627-C8E9-452D-9CD5-AFCC8FEFE395
uid: core/querying/tags
ms.openlocfilehash: 3a4d516cab5836c659e42d825c4f1bf89355d671
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688799"
---
# <a name="query-tags"></a><span data-ttu-id="37704-102">Značky dotazu</span><span class="sxs-lookup"><span data-stu-id="37704-102">Query tags</span></span>
> [!NOTE]
> <span data-ttu-id="37704-103">Tato funkce je nového v EF Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="37704-103">This feature is new in EF Core 2.2.</span></span>

<span data-ttu-id="37704-104">Tato funkce pomáhá korelace dotazů LINQ v kódu pomocí generovaného dotazů SQL, které jsou zachyceny v protokolech.</span><span class="sxs-lookup"><span data-stu-id="37704-104">This feature helps correlate LINQ queries in code with generated SQL queries captured in logs.</span></span>
<span data-ttu-id="37704-105">Opatřit poznámkami pomocí nového dotazu LINQ `TagWith()` metody:</span><span class="sxs-lookup"><span data-stu-id="37704-105">You annotate a LINQ query using the new `TagWith()` method:</span></span> 

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith("This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

<span data-ttu-id="37704-106">Tento dotaz LINQ, je přeložen do následujícího příkazu SQL:</span><span class="sxs-lookup"><span data-stu-id="37704-106">This LINQ query is translated to the following SQL statement:</span></span>

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="37704-107">Je možné volat `TagWith()` v mnoha případech ve stejném dotazu.</span><span class="sxs-lookup"><span data-stu-id="37704-107">It's possible to call `TagWith()` many times on the same query.</span></span>
<span data-ttu-id="37704-108">Klíčová slova dotazu jsou kumulativní.</span><span class="sxs-lookup"><span data-stu-id="37704-108">Query tags are cumulative.</span></span>
<span data-ttu-id="37704-109">Mějme například následující metody:</span><span class="sxs-lookup"><span data-stu-id="37704-109">For example, given the following methods:</span></span>

``` csharp
IQueryable<Friend> GetNearestFriends(Point myLocation) =>
    from f in context.Friends.TagWith("GetNearestFriends")
    orderby f.Location.Distance(myLocation) descending
    select f;

IQueryable<T> Limit<T>(IQueryable<T> source, int limit) =>
    source.TagWith("Limit").Take(limit);
```

<span data-ttu-id="37704-110">Následující dotaz:</span><span class="sxs-lookup"><span data-stu-id="37704-110">The following query:</span></span>   

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).ToList();
```

<span data-ttu-id="37704-111">Přeloží na:</span><span class="sxs-lookup"><span data-stu-id="37704-111">Translates to:</span></span>

``` sql
-- GetNearestFriends

-- Limit

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="37704-112">Je také možné použít víceřádkových řetězců jako značky dotazu.</span><span class="sxs-lookup"><span data-stu-id="37704-112">It's also possible to use multi-line strings as query tags.</span></span>
<span data-ttu-id="37704-113">Příklad:</span><span class="sxs-lookup"><span data-stu-id="37704-113">For example:</span></span>

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).TagWith(
@"This is a multi-line
string").ToList();
```

<span data-ttu-id="37704-114">Vytvoří následující příkaz SQL:</span><span class="sxs-lookup"><span data-stu-id="37704-114">Produces the following SQL:</span></span>

``` sql
-- GetNearestFriends

-- Limit

-- This is a multi-line
-- string

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

## <a name="known-limitations"></a><span data-ttu-id="37704-115">Známá omezení</span><span class="sxs-lookup"><span data-stu-id="37704-115">Known limitations</span></span>
<span data-ttu-id="37704-116">**Značky dotazu nejsou parametrizovat:** EF Core vždy zpracovává značky dotazu v dotazu LINQ jako řetězcové literály, které jsou součástí generovaného SQL.</span><span class="sxs-lookup"><span data-stu-id="37704-116">**Query tags aren't parameterizable:** EF Core always treats query tags in the LINQ query as string literals that are included in the generated SQL.</span></span>
<span data-ttu-id="37704-117">Kompilované dotazy, které vyžadují značky dotazu, jako parametry se nepovolují.</span><span class="sxs-lookup"><span data-stu-id="37704-117">Compiled queries that take query tags as parameters aren't allowed.</span></span>