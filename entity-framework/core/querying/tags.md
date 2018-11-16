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
# <a name="query-tags"></a>Značky dotazu
> [!NOTE]
> Tato funkce je nového v EF Core 2.2.

Tato funkce pomáhá korelace dotazů LINQ v kódu pomocí generovaného dotazů SQL, které jsou zachyceny v protokolech.
Opatřit poznámkami pomocí nového dotazu LINQ `TagWith()` metody: 

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith("This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

Tento dotaz LINQ, je přeložen do následujícího příkazu SQL:

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

Je možné volat `TagWith()` v mnoha případech ve stejném dotazu.
Klíčová slova dotazu jsou kumulativní.
Mějme například následující metody:

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

Přeloží na:

``` sql
-- GetNearestFriends

-- Limit

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

Je také možné použít víceřádkových řetězců jako značky dotazu.
Příklad:

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).TagWith(
@"This is a multi-line
string").ToList();
```

Vytvoří následující příkaz SQL:

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
**Značky dotazu nejsou parametrizovat:** EF Core vždy zpracovává značky dotazu v dotazu LINQ jako řetězcové literály, které jsou součástí generovaného SQL.
Kompilované dotazy, které vyžadují značky dotazu, jako parametry se nepovolují.