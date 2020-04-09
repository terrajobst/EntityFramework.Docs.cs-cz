---
title: Asynchronní ukládání – EF Core
author: rowanmiller
ms.date: 01/24/2017
ms.assetid: b64a606e-ecd9-4807-829a-b6ec05ade33f
uid: core/saving/async
ms.openlocfilehash: 0823b86f0579dd3e42f6bd2aebfb433d3cbe00ab
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417640"
---
# <a name="asynchronous-saving"></a>Asynchronní ukládání

Asynchronní ukládání zabraňuje blokování podprocesu při změny jsou zapsány do databáze. To může být užitečné, aby se zabránilo zmrazení ui aplikace tlusté klienta. Asynchronní operace můžete také zvýšit propustnost ve webové aplikaci, kde vlákno lze uvolnit až do služby jiné požadavky, zatímco operace databáze dokončí. Další informace naleznete [v tématu Asynchronní programování v jazyce C#](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> EF Core nepodporuje více paralelních operací spuštěna na stejné instanci kontextu. Před zahájením další operace byste měli vždy počkat na dokončení operace. To se obvykle provádí `await` pomocí klíčového slova pro každou asynchronní operaci.

Entity Framework `DbContext.SaveChangesAsync()` Core poskytuje jako asynchronní alternativu k `DbContext.SaveChanges()`.

[!code-csharp[Main](../../../samples/core/Saving/Async/Sample.cs#Sample)]
