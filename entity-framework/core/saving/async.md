---
title: Asynchronní ukládání - EF jádra
author: rowanmiller
ms.author: divega
ms.date: 01/24/2017
ms.assetid: b64a606e-ecd9-4807-829a-b6ec05ade33f
ms.technology: entity-framework-core
uid: core/saving/async
ms.openlocfilehash: 640e5f131b0e9ef4e5028d1dcaf80f3e5bbd9d9b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054133"
---
# <a name="asynchronous-saving"></a>Asynchronní ukládání

Asynchronní ukládání zabraňuje blokování vlákna během změny jsou zapsána do databáze. To může být užitečné, aby se zabránilo zmrazení rozhraní silná klientské aplikace. Asynchronní operace může také zvýšit propustnost ve webové aplikaci, kde vlákno mohou být uvolněna pro ostatní žádosti o služby při dokončení operace databáze. Další informace najdete v tématu [asynchronní programování v jazyce C#](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> Základní EF nepodporuje více paralelních operací se spouští ke stejné instanci kontextu. Vždy by měl počkejte na dokončení před zahájením další operace operace. To se obvykle provádí pomocí `await` – klíčové slovo na každou asynchronní operaci.

Poskytuje Entity Framework Core `DbContext.SaveChangesAsync()` asynchronní namísto `DbContext.SaveChanges()`.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Async/Sample.cs#Sample)]
