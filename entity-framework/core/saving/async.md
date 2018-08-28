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
# <a name="asynchronous-saving"></a><span data-ttu-id="1f668-102">Asynchronní ukládání</span><span class="sxs-lookup"><span data-stu-id="1f668-102">Asynchronous Saving</span></span>

<span data-ttu-id="1f668-103">Asynchronní ukládání se vyhnete blokování vlákna, zatímco se změny jsou zapsány do databáze.</span><span class="sxs-lookup"><span data-stu-id="1f668-103">Asynchronous saving avoids blocking a thread while the changes are written to the database.</span></span> <span data-ttu-id="1f668-104">To může být užitečné, aby se zabránilo zmrazení uživatelského rozhraní tlustá klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="1f668-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="1f668-105">Asynchronní operace může také zvýšit propustnost ve webové aplikaci, ve kterém můžete vlákno uvolnit pro ostatní žádosti o služby, zatímco se dokončují operace databáze.</span><span class="sxs-lookup"><span data-stu-id="1f668-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="1f668-106">Další informace najdete v tématu [asynchronní programování v jazyce C#](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="1f668-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="1f668-107">EF Core nepodporuje více paralelních operací běží na stejné instance kontextu.</span><span class="sxs-lookup"><span data-stu-id="1f668-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="1f668-108">Vždy můžete čekat pro operaci dokončit před zahájením další operaci.</span><span class="sxs-lookup"><span data-stu-id="1f668-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="1f668-109">To se obvykle provádí pomocí `await` – klíčové slovo na každou asynchronní operaci.</span><span class="sxs-lookup"><span data-stu-id="1f668-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="1f668-110">Entity Framework Core poskytuje `DbContext.SaveChangesAsync()` asynchronní alternativa k `DbContext.SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="1f668-110">Entity Framework Core provides `DbContext.SaveChangesAsync()` as an asynchronous alternative to `DbContext.SaveChanges()`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Async/Sample.cs#Sample)]
