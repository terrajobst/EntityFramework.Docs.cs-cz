---
title: Asynchronní dotazy – EF Core
author: rowanmiller
ms.date: 01/24/2017
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: de00e25279e29355a4eb3e55597a8578ceccecb6
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993562"
---
# <a name="asynchronous-queries"></a>Asynchronní dotazy

Asynchronní dotazy Vyhněte se blokování vlákna, zatímco je dotaz proveden v databázi. To může být užitečné, aby se zabránilo zmrazení uživatelského rozhraní tlustá klientské aplikace. Asynchronní operace může také zvýšit propustnost ve webové aplikaci, ve kterém můžete vlákno uvolnit pro ostatní žádosti o služby, zatímco se dokončují operace databáze. Další informace najdete v tématu [asynchronní programování v jazyce C#](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> EF Core nepodporuje více paralelních operací běží na stejné instance kontextu. Vždy můžete čekat pro operaci dokončit před zahájením další operaci. To se obvykle provádí pomocí `await` – klíčové slovo na každou asynchronní operaci.

Entity Framework Core obsahuje sadu asynchronní rozšiřující metody, které lze použít jako alternativu k LINQ metody, které způsobí provedení dotazu a výsledky. Mezi příklady patří `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`atd. Nejsou žádná asynchronní verze operátory LINQ, jako `Where(...)`, `OrderBy(...)`a tak dále, protože tyto metody pouze sestavení stromu výrazů LINQ a nevyvolávají dotazu má být proveden v databázi.

> [!IMPORTANT]  
> Rozšiřující metody pro asynchronní EF Core jsou definovány v `Microsoft.EntityFrameworkCore` oboru názvů. Tento obor názvů musí být naimportovány pro metody musí být k dispozici.

[!code-csharp[Main](../../../samples/core/Querying/Querying/Async/Sample.cs#Sample)]
