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
# <a name="query-tags"></a>Značky dotazů

> [!NOTE]
> Tato funkce je v EF Core 2,2 novinkou.

Tato funkce pomáhá korelovat dotazy LINQ v kódu s generovanými dotazy SQL zaznamenanými v protokolech.
Dotaz LINQ můžete opatřit pomocí nové metody `TagWith()`:

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith("This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

Tento dotaz LINQ je přeložen na následující příkaz SQL:

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

Je možné volat `TagWith()` mnohokrát na stejný dotaz.
Značky dotazu jsou kumulativní.
Například s ohledem na následující metody:

``` csharp
IQueryable<Friend> GetNearestFriends(Point myLocation) =>
    from f in context.Friends.TagWith("GetNearestFriends")
    orderby f.Location.Distance(myLocation) descending
    select f;

IQueryable<T> Limit<T>(IQueryable<T> source, int limit) =>
    source.TagWith("Limit").Take(limit);
```

Následující dotaz:

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).ToList();
```

Překládá se na:

``` sql
-- GetNearestFriends

-- Limit

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

Je také možné použít víceřádkové řetězce jako značky dotazu.
Příklad:

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).TagWith(
@"This is a multi-line
string").ToList();
```

Vytvoří následující SQL:

``` sql
-- GetNearestFriends

-- Limit

-- This is a multi-line
-- string

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

## <a name="known-limitations"></a>Známá omezení

**Značky dotazu nejsou parametrizovat:** EF Core vždy zpracovává značky dotazů v dotazu LINQ jako řetězcové literály, které jsou zahrnuty ve vygenerovaném jazyce SQL.
Zkompilované dotazy, které přijímají značky dotazu jako parametry nejsou povoleny.
