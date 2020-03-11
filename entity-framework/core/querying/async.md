---
title: Asynchronní dotazy – EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: ce26db32a616dac5edac2a8451014ae63cbfc12d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417751"
---
# <a name="asynchronous-queries"></a>Asynchronní dotazy

Asynchronní dotazy zabraňují zablokování vlákna při spuštění dotazu v databázi. Asynchronní dotazy jsou důležité pro udržení reagujícího uživatelského rozhraní ve silných klientských aplikacích. Můžou také zvýšit propustnost webových aplikací, kde uvolní vlákno pro obsluhu dalších požadavků ve webových aplikacích. Další informace naleznete v tématu [asynchronní programování v C# ](/dotnet/csharp/async).

> [!WARNING]  
> EF Core nepodporuje spouštění více paralelních operací ve stejné instanci kontextu. Před zahájením další operace byste vždy měli počkat na dokončení operace. To se obvykle provádí pomocí klíčového slova `await` v každé asynchronní operaci.

Entity Framework Core poskytuje sadu asynchronních metod rozšíření podobných metodám LINQ, které spustí dotaz a vrátí výsledky. Příklady zahrnují `ToListAsync()`, `ToArrayAsync()``SingleAsync()`. Neexistují žádné asynchronní verze některých operátorů LINQ, jako je například `Where(...)` nebo `OrderBy(...)`, protože tyto metody sestavují pouze strom výrazů LINQ a nezpůsobí, že dotaz bude proveden v databázi.

> [!IMPORTANT]  
> Metody rozšíření EF Core Async jsou definovány v oboru názvů `Microsoft.EntityFrameworkCore`. Tento obor názvů musí být importován, aby byly metody k dispozici.

[!code-csharp[Main](../../../samples/core/Querying/Async/Sample.cs#ToListAsync)]
