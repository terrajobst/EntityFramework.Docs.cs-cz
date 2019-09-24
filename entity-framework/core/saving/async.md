---
title: Asynchronní ukládání – EF Core
author: rowanmiller
ms.date: 01/24/2017
ms.assetid: b64a606e-ecd9-4807-829a-b6ec05ade33f
uid: core/saving/async
ms.openlocfilehash: 0823b86f0579dd3e42f6bd2aebfb433d3cbe00ab
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197816"
---
# <a name="asynchronous-saving"></a><span data-ttu-id="8acef-102">Asynchronní ukládání</span><span class="sxs-lookup"><span data-stu-id="8acef-102">Asynchronous Saving</span></span>

<span data-ttu-id="8acef-103">Asynchronní ukládání zabraňuje blokování vlákna během zápisu změn do databáze.</span><span class="sxs-lookup"><span data-stu-id="8acef-103">Asynchronous saving avoids blocking a thread while the changes are written to the database.</span></span> <span data-ttu-id="8acef-104">To může být užitečné, aby nedocházelo k zamrznutí uživatelského rozhraní aplikace silného klienta.</span><span class="sxs-lookup"><span data-stu-id="8acef-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="8acef-105">Asynchronní operace mohou také zvýšit propustnost ve webové aplikaci, kde může být vlákno uvolněno pro obsluhu dalších požadavků v době, kdy se operace databáze dokončí.</span><span class="sxs-lookup"><span data-stu-id="8acef-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="8acef-106">Další informace naleznete v tématu [asynchronní programování v C# ](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="8acef-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="8acef-107">EF Core nepodporuje více paralelních operací spuštěných ve stejné instanci kontextu.</span><span class="sxs-lookup"><span data-stu-id="8acef-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="8acef-108">Před zahájením další operace byste vždy měli počkat na dokončení operace.</span><span class="sxs-lookup"><span data-stu-id="8acef-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="8acef-109">To se obvykle provádí pomocí `await` klíčového slova pro každou asynchronní operaci.</span><span class="sxs-lookup"><span data-stu-id="8acef-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="8acef-110">Entity Framework Core poskytuje `DbContext.SaveChangesAsync()` jako asynchronní alternativu k `DbContext.SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="8acef-110">Entity Framework Core provides `DbContext.SaveChangesAsync()` as an asynchronous alternative to `DbContext.SaveChanges()`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Async/Sample.cs#Sample)]
