---
title: Asynchronní dotazy – EF jádra
author: rowanmiller
ms.author: divega
ms.date: 01/24/2017
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
ms.technology: entity-framework-core
uid: core/querying/async
ms.openlocfilehash: 6554f04d0edfe0ca2ee72ebed8b878a1997a9500
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="asynchronous-queries"></a>Asynchronní dotazy

Asynchronní dotazy Vyhněte se blokování vlákno, zatímco je dotaz proveden v databázi. To může být užitečné, aby se zabránilo zmrazení rozhraní silná klientské aplikace. Asynchronní operace může také zvýšit propustnost ve webové aplikaci, kde vlákno mohou být uvolněna pro ostatní žádosti o služby při dokončení operace databáze. Další informace najdete v tématu [asynchronní programování v jazyce C#](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> Základní EF nepodporuje více paralelních operací se spouští ke stejné instanci kontextu. Vždy by měl počkejte na dokončení před zahájením další operace operace. To se obvykle provádí pomocí `await` – klíčové slovo na každou asynchronní operaci.

Entity Framework Core poskytuje sadu metod asynchronní rozšíření, které lze použít jako alternativu k LINQ metody, které způsobí provedení dotazu a výsledky vrácené. Mezi příklady patří `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`atd. Nejsou k dispozici asynchronních verzích operátory LINQ jako `Where(...)`, `OrderBy(...)`, atd., protože tyto metody pouze sestavovat strom výrazu LINQ nebo nezpůsobí dotazu, který má být proveden v databázi.

> [!IMPORTANT]  
> Rozšiřující metody pro asynchronní EF základní jsou definovány v `Microsoft.EntityFrameworkCore` oboru názvů. Tento obor názvů musí být importovány pro metody být k dispozici.

[!code-csharp[Main](../../../samples/core/Querying/Querying/Async/Sample.cs#Sample)]
