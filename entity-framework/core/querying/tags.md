---
title: Značky dotazů – EF Core
author: divega
ms.date: 11/14/2018
ms.assetid: 73C7A627-C8E9-452D-9CD5-AFCC8FEFE395
uid: core/querying/tags
ms.openlocfilehash: e8415b237df45ce652dcd152013f4f12a992aed7
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417888"
---
# <a name="query-tags"></a><span data-ttu-id="24c37-102">Značky dotazů</span><span class="sxs-lookup"><span data-stu-id="24c37-102">Query tags</span></span>

> [!NOTE]
> <span data-ttu-id="24c37-103">Tato funkce je v EF Core 2,2 novinkou.</span><span class="sxs-lookup"><span data-stu-id="24c37-103">This feature is new in EF Core 2.2.</span></span>

<span data-ttu-id="24c37-104">Tato funkce pomáhá korelovat dotazy LINQ v kódu s generovanými dotazy SQL zaznamenanými v protokolech.</span><span class="sxs-lookup"><span data-stu-id="24c37-104">This feature helps correlate LINQ queries in code with generated SQL queries captured in logs.</span></span>
<span data-ttu-id="24c37-105">Dotaz LINQ můžete opatřit pomocí nové metody `TagWith()`:</span><span class="sxs-lookup"><span data-stu-id="24c37-105">You annotate a LINQ query using the new `TagWith()` method:</span></span>

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith("This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

<span data-ttu-id="24c37-106">Tento dotaz LINQ je přeložen na následující příkaz SQL:</span><span class="sxs-lookup"><span data-stu-id="24c37-106">This LINQ query is translated to the following SQL statement:</span></span>

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="24c37-107">Je možné volat `TagWith()` mnohokrát na stejný dotaz.</span><span class="sxs-lookup"><span data-stu-id="24c37-107">It's possible to call `TagWith()` many times on the same query.</span></span>
<span data-ttu-id="24c37-108">Značky dotazu jsou kumulativní.</span><span class="sxs-lookup"><span data-stu-id="24c37-108">Query tags are cumulative.</span></span>
<span data-ttu-id="24c37-109">Například s ohledem na následující metody:</span><span class="sxs-lookup"><span data-stu-id="24c37-109">For example, given the following methods:</span></span>

``` csharp
IQueryable<Friend> GetNearestFriends(Point myLocation) =>
    from f in context.Friends.TagWith("GetNearestFriends")
    orderby f.Location.Distance(myLocation) descending
    select f;

IQueryable<T> Limit<T>(IQueryable<T> source, int limit) =>
    source.TagWith("Limit").Take(limit);
```

<span data-ttu-id="24c37-110">Následující dotaz:</span><span class="sxs-lookup"><span data-stu-id="24c37-110">The following query:</span></span>

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).ToList();
```

<span data-ttu-id="24c37-111">Překládá se na:</span><span class="sxs-lookup"><span data-stu-id="24c37-111">Translates to:</span></span>

``` sql
-- GetNearestFriends

-- Limit

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="24c37-112">Je také možné použít víceřádkové řetězce jako značky dotazu.</span><span class="sxs-lookup"><span data-stu-id="24c37-112">It's also possible to use multi-line strings as query tags.</span></span>
<span data-ttu-id="24c37-113">Příklad:</span><span class="sxs-lookup"><span data-stu-id="24c37-113">For example:</span></span>

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).TagWith(
@"This is a multi-line
string").ToList();
```

<span data-ttu-id="24c37-114">Vytvoří následující SQL:</span><span class="sxs-lookup"><span data-stu-id="24c37-114">Produces the following SQL:</span></span>

``` sql
-- GetNearestFriends

-- Limit

-- This is a multi-line
-- string

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

## <a name="known-limitations"></a><span data-ttu-id="24c37-115">Známá omezení</span><span class="sxs-lookup"><span data-stu-id="24c37-115">Known limitations</span></span>

<span data-ttu-id="24c37-116">**Značky dotazu nejsou parametrizovat:** EF Core vždy zpracovává značky dotazů v dotazu LINQ jako řetězcové literály, které jsou zahrnuty ve vygenerovaném jazyce SQL.</span><span class="sxs-lookup"><span data-stu-id="24c37-116">**Query tags aren't parameterizable:** EF Core always treats query tags in the LINQ query as string literals that are included in the generated SQL.</span></span>
<span data-ttu-id="24c37-117">Zkompilované dotazy, které přijímají značky dotazu jako parametry nejsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="24c37-117">Compiled queries that take query tags as parameters aren't allowed.</span></span>
