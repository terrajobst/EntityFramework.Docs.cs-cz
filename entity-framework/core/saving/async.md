---
title: Asynchronní ukládání – EF Core
author: rowanmiller
ms.date: 01/24/2017
ms.assetid: b64a606e-ecd9-4807-829a-b6ec05ade33f
uid: core/saving/async
ms.openlocfilehash: 6f482a77300ff2930953686751a579b022bf6f77
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997279"
---
# <a name="asynchronous-saving"></a>Asynchronní ukládání

Asynchronní ukládání se vyhnete blokování vlákna, zatímco se změny jsou zapsány do databáze. To může být užitečné, aby se zabránilo zmrazení uživatelského rozhraní tlustá klientské aplikace. Asynchronní operace může také zvýšit propustnost ve webové aplikaci, ve kterém můžete vlákno uvolnit pro ostatní žádosti o služby, zatímco se dokončují operace databáze. Další informace najdete v tématu [asynchronní programování v jazyce C#](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> EF Core nepodporuje více paralelních operací běží na stejné instance kontextu. Vždy můžete čekat pro operaci dokončit před zahájením další operaci. To se obvykle provádí pomocí `await` – klíčové slovo na každou asynchronní operaci.

Entity Framework Core poskytuje `DbContext.SaveChangesAsync()` asynchronní alternativa k `DbContext.SaveChanges()`.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Async/Sample.cs#Sample)]
