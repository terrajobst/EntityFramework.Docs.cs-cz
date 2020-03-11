---
title: Asynchronní ukládání – EF Core
author: rowanmiller
ms.date: 01/24/2017
ms.assetid: b64a606e-ecd9-4807-829a-b6ec05ade33f
uid: core/saving/async
ms.openlocfilehash: 0823b86f0579dd3e42f6bd2aebfb433d3cbe00ab
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417640"
---
# <a name="asynchronous-saving"></a>Asynchronní ukládání

Asynchronní ukládání zabraňuje blokování vlákna během zápisu změn do databáze. To může být užitečné, aby nedocházelo k zamrznutí uživatelského rozhraní aplikace silného klienta. Asynchronní operace mohou také zvýšit propustnost ve webové aplikaci, kde může být vlákno uvolněno pro obsluhu dalších požadavků v době, kdy se operace databáze dokončí. Další informace naleznete v tématu [asynchronní programování v C# ](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> EF Core nepodporuje více paralelních operací spuštěných ve stejné instanci kontextu. Před zahájením další operace byste vždy měli počkat na dokončení operace. To se obvykle provádí pomocí klíčového slova `await` pro každou asynchronní operaci.

Entity Framework Core poskytuje `DbContext.SaveChangesAsync()` jako asynchronní alternativu k `DbContext.SaveChanges()`.

[!code-csharp[Main](../../../samples/core/Saving/Async/Sample.cs#Sample)]
