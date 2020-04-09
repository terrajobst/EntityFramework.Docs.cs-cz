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
# <a name="asynchronous-saving"></a><span data-ttu-id="3c7d8-102">Asynchronní ukládání</span><span class="sxs-lookup"><span data-stu-id="3c7d8-102">Asynchronous Saving</span></span>

<span data-ttu-id="3c7d8-103">Asynchronní ukládání zabraňuje blokování podprocesu při změny jsou zapsány do databáze.</span><span class="sxs-lookup"><span data-stu-id="3c7d8-103">Asynchronous saving avoids blocking a thread while the changes are written to the database.</span></span> <span data-ttu-id="3c7d8-104">To může být užitečné, aby se zabránilo zmrazení ui aplikace tlusté klienta.</span><span class="sxs-lookup"><span data-stu-id="3c7d8-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="3c7d8-105">Asynchronní operace můžete také zvýšit propustnost ve webové aplikaci, kde vlákno lze uvolnit až do služby jiné požadavky, zatímco operace databáze dokončí.</span><span class="sxs-lookup"><span data-stu-id="3c7d8-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="3c7d8-106">Další informace naleznete [v tématu Asynchronní programování v jazyce C#](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="3c7d8-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="3c7d8-107">EF Core nepodporuje více paralelních operací spuštěna na stejné instanci kontextu.</span><span class="sxs-lookup"><span data-stu-id="3c7d8-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="3c7d8-108">Před zahájením další operace byste měli vždy počkat na dokončení operace.</span><span class="sxs-lookup"><span data-stu-id="3c7d8-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="3c7d8-109">To se obvykle provádí `await` pomocí klíčového slova pro každou asynchronní operaci.</span><span class="sxs-lookup"><span data-stu-id="3c7d8-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="3c7d8-110">Entity Framework `DbContext.SaveChangesAsync()` Core poskytuje jako asynchronní alternativu k `DbContext.SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="3c7d8-110">Entity Framework Core provides `DbContext.SaveChangesAsync()` as an asynchronous alternative to `DbContext.SaveChanges()`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Async/Sample.cs#Sample)]
