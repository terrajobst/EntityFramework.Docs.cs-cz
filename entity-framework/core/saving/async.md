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
# <a name="asynchronous-saving"></a><span data-ttu-id="60919-102">Asynchronní ukládání</span><span class="sxs-lookup"><span data-stu-id="60919-102">Asynchronous Saving</span></span>

<span data-ttu-id="60919-103">Asynchronní ukládání zabraňuje blokování vlákna během změny jsou zapsána do databáze.</span><span class="sxs-lookup"><span data-stu-id="60919-103">Asynchronous saving avoids blocking a thread while the changes are written to the database.</span></span> <span data-ttu-id="60919-104">To může být užitečné, aby se zabránilo zmrazení rozhraní silná klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="60919-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="60919-105">Asynchronní operace může také zvýšit propustnost ve webové aplikaci, kde vlákno mohou být uvolněna pro ostatní žádosti o služby při dokončení operace databáze.</span><span class="sxs-lookup"><span data-stu-id="60919-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="60919-106">Další informace najdete v tématu [asynchronní programování v jazyce C#](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="60919-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="60919-107">Základní EF nepodporuje více paralelních operací se spouští ke stejné instanci kontextu.</span><span class="sxs-lookup"><span data-stu-id="60919-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="60919-108">Vždy by měl počkejte na dokončení před zahájením další operace operace.</span><span class="sxs-lookup"><span data-stu-id="60919-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="60919-109">To se obvykle provádí pomocí `await` – klíčové slovo na každou asynchronní operaci.</span><span class="sxs-lookup"><span data-stu-id="60919-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="60919-110">Poskytuje Entity Framework Core `DbContext.SaveChangesAsync()` asynchronní namísto `DbContext.SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="60919-110">Entity Framework Core provides `DbContext.SaveChangesAsync()` as an asynchronous alternative to `DbContext.SaveChanges()`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Async/Sample.cs#Sample)]
