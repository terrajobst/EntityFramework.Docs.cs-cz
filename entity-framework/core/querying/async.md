---
title: Asynchronní dotazy – EF Core
author: rowanmiller
ms.date: 01/24/2017
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: 415c57df599f1cb1a255f01d1072890fedfd6d2b
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921695"
---
# <a name="asynchronous-queries"></a>Asynchronní dotazy

Asynchronní dotazy zabraňují zablokování vlákna při spuštění dotazu v databázi. To může být užitečné, aby nedocházelo k zamrznutí uživatelského rozhraní aplikace silného klienta. Asynchronní operace mohou také zvýšit propustnost ve webové aplikaci, kde může být vlákno uvolněno pro obsluhu dalších požadavků v době, kdy se operace databáze dokončí. Další informace naleznete v tématu [asynchronní programování v C# ](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> EF Core nepodporuje více paralelních operací spuštěných ve stejné instanci kontextu. Před zahájením další operace byste vždy měli počkat na dokončení operace. To se obvykle provádí pomocí `await` klíčového slova pro každou asynchronní operaci.

Entity Framework Core poskytuje sadu asynchronních metod rozšíření, které lze použít jako alternativu k metodám LINQ, které způsobují provedení dotazu a vrácených výsledků. Příklady zahrnují `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`, atd. Neexistují asynchronní verze operátorů `Where(...)`LINQ, jako např., `OrderBy(...)`atd., protože tyto metody sestavují pouze strom výrazů LINQ a nezpůsobí, že dotaz bude proveden v databázi.

> [!IMPORTANT]  
> Metody rozšíření EF Core Async jsou definovány v `Microsoft.EntityFrameworkCore` oboru názvů. Tento obor názvů musí být importován, aby byly metody k dispozici.

[!code-csharp[Main](../../../samples/core/Querying/Async/Sample.cs#Sample)]
