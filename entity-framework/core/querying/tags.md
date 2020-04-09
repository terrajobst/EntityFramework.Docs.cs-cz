---
title: Značky dotazů – ef jádro
author: divega
ms.date: 11/14/2018
ms.assetid: 73C7A627-C8E9-452D-9CD5-AFCC8FEFE395
uid: core/querying/tags
ms.openlocfilehash: e8415b237df45ce652dcd152013f4f12a992aed7
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417888"
---
# <a name="query-tags"></a><span data-ttu-id="e53ec-102">Značky dotazu</span><span class="sxs-lookup"><span data-stu-id="e53ec-102">Query tags</span></span>

> [!NOTE]
> <span data-ttu-id="e53ec-103">Tato funkce je v EF Core 2.2 nová.</span><span class="sxs-lookup"><span data-stu-id="e53ec-103">This feature is new in EF Core 2.2.</span></span>

<span data-ttu-id="e53ec-104">Tato funkce pomáhá korelovat linq dotazy v kódu s generovanými dotazy SQL zachycenými v protokolech.</span><span class="sxs-lookup"><span data-stu-id="e53ec-104">This feature helps correlate LINQ queries in code with generated SQL queries captured in logs.</span></span>
<span data-ttu-id="e53ec-105">Dotaz LINQ oslníte pomocí `TagWith()` nové metody:</span><span class="sxs-lookup"><span data-stu-id="e53ec-105">You annotate a LINQ query using the new `TagWith()` method:</span></span>

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith("This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

<span data-ttu-id="e53ec-106">Tento dotaz LINQ je přeložen na následující příkaz SQL:</span><span class="sxs-lookup"><span data-stu-id="e53ec-106">This LINQ query is translated to the following SQL statement:</span></span>

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="e53ec-107">Je možné volat `TagWith()` mnohokrát na stejný dotaz.</span><span class="sxs-lookup"><span data-stu-id="e53ec-107">It's possible to call `TagWith()` many times on the same query.</span></span>
<span data-ttu-id="e53ec-108">Značky dotazu jsou kumulativní.</span><span class="sxs-lookup"><span data-stu-id="e53ec-108">Query tags are cumulative.</span></span>
<span data-ttu-id="e53ec-109">Například s ohledem na následující metody:</span><span class="sxs-lookup"><span data-stu-id="e53ec-109">For example, given the following methods:</span></span>

``` csharp
IQueryable<Friend> GetNearestFriends(Point myLocation) =>
    from f in context.Friends.TagWith("GetNearestFriends")
    orderby f.Location.Distance(myLocation) descending
    select f;

IQueryable<T> Limit<T>(IQueryable<T> source, int limit) =>
    source.TagWith("Limit").Take(limit);
```

<span data-ttu-id="e53ec-110">Následující dotaz:</span><span class="sxs-lookup"><span data-stu-id="e53ec-110">The following query:</span></span>

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).ToList();
```

<span data-ttu-id="e53ec-111">Překládá na:</span><span class="sxs-lookup"><span data-stu-id="e53ec-111">Translates to:</span></span>

``` sql
-- GetNearestFriends

-- Limit

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="e53ec-112">Je také možné použít víceřádkové řetězce jako značky dotazu.</span><span class="sxs-lookup"><span data-stu-id="e53ec-112">It's also possible to use multi-line strings as query tags.</span></span>
<span data-ttu-id="e53ec-113">Příklad:</span><span class="sxs-lookup"><span data-stu-id="e53ec-113">For example:</span></span>

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).TagWith(
@"This is a multi-line
string").ToList();
```

<span data-ttu-id="e53ec-114">Vytváří následující SQL:</span><span class="sxs-lookup"><span data-stu-id="e53ec-114">Produces the following SQL:</span></span>

``` sql
-- GetNearestFriends

-- Limit

-- This is a multi-line
-- string

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

## <a name="known-limitations"></a><span data-ttu-id="e53ec-115">Známá omezení</span><span class="sxs-lookup"><span data-stu-id="e53ec-115">Known limitations</span></span>

<span data-ttu-id="e53ec-116">**Značky dotazu nelze parametrizovat:** EF Core vždy považuje značky dotazu v linq dotazu jako řetězcové literály, které jsou zahrnuty v generované SQL.</span><span class="sxs-lookup"><span data-stu-id="e53ec-116">**Query tags aren't parameterizable:** EF Core always treats query tags in the LINQ query as string literals that are included in the generated SQL.</span></span>
<span data-ttu-id="e53ec-117">Zkompilované dotazy, které berou značky dotazu jako parametry nejsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="e53ec-117">Compiled queries that take query tags as parameters aren't allowed.</span></span>
