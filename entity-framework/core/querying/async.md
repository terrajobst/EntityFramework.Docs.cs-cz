---
title: Asynchronní dotazy – jádro EF
author: smitpatel
ms.date: 10/03/2019
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: ce26db32a616dac5edac2a8451014ae63cbfc12d
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417751"
---
# <a name="asynchronous-queries"></a>Asynchronní dotazy

Asynchronní dotazy vyhnout blokování podprocesu při spuštění dotazu v databázi. Asynchronní dotazy jsou důležité pro udržování responzivní uživatelské rozhraní v aplikacích tlusté klienta. Mohou také zvýšit propustnost ve webových aplikacích, kde uvolní vlákno pro obsluhu jiných požadavků ve webových aplikacích. Další informace naleznete [v tématu Asynchronní programování v jazyce C#](/dotnet/csharp/async).

> [!WARNING]  
> EF Core nepodporuje více paralelních operací spuštěna na stejné instanci kontextu. Před zahájením další operace byste měli vždy počkat na dokončení operace. To se obvykle provádí `await` pomocí klíčového slova pro každou asynchronní operaci.

Entity Framework Core poskytuje sadu asynchronních rozšiřujících metod podobných metodám LINQ, které spouštějí dotaz a vracejí výsledky. Příklady `ToListAsync()`zahrnují `ToArrayAsync()` `SingleAsync()`, , . Neexistují žádné asynchronní verze některých operátorů LINQ, například `Where(...)` nebo `OrderBy(...)` protože tyto metody pouze vytvářejí strom výrazů LINQ a nezpůsobují spuštění dotazu v databázi.

> [!IMPORTANT]  
> Metody rozšíření async EF Core `Microsoft.EntityFrameworkCore` jsou definovány v oboru názvů. Tento obor názvů musí být importován, aby byly metody k dispozici.

[!code-csharp[Main](../../../samples/core/Querying/Async/Sample.cs#ToListAsync)]
