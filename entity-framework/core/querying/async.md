---
title: Asynchronní dotazy – EF Core
author: rowanmiller
ms.date: 01/24/2017
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: 415c57df599f1cb1a255f01d1072890fedfd6d2b
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921695"
---
# <a name="asynchronous-queries"></a><span data-ttu-id="b7d89-102">Asynchronní dotazy</span><span class="sxs-lookup"><span data-stu-id="b7d89-102">Asynchronous Queries</span></span>

<span data-ttu-id="b7d89-103">Asynchronní dotazy zabraňují zablokování vlákna při spuštění dotazu v databázi.</span><span class="sxs-lookup"><span data-stu-id="b7d89-103">Asynchronous queries avoid blocking a thread while the query is executed in the database.</span></span> <span data-ttu-id="b7d89-104">To může být užitečné, aby nedocházelo k zamrznutí uživatelského rozhraní aplikace silného klienta.</span><span class="sxs-lookup"><span data-stu-id="b7d89-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="b7d89-105">Asynchronní operace mohou také zvýšit propustnost ve webové aplikaci, kde může být vlákno uvolněno pro obsluhu dalších požadavků v době, kdy se operace databáze dokončí.</span><span class="sxs-lookup"><span data-stu-id="b7d89-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="b7d89-106">Další informace naleznete v tématu [asynchronní programování v C# ](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="b7d89-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="b7d89-107">EF Core nepodporuje více paralelních operací spuštěných ve stejné instanci kontextu.</span><span class="sxs-lookup"><span data-stu-id="b7d89-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="b7d89-108">Před zahájením další operace byste vždy měli počkat na dokončení operace.</span><span class="sxs-lookup"><span data-stu-id="b7d89-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="b7d89-109">To se obvykle provádí pomocí `await` klíčového slova pro každou asynchronní operaci.</span><span class="sxs-lookup"><span data-stu-id="b7d89-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="b7d89-110">Entity Framework Core poskytuje sadu asynchronních metod rozšíření, které lze použít jako alternativu k metodám LINQ, které způsobují provedení dotazu a vrácených výsledků.</span><span class="sxs-lookup"><span data-stu-id="b7d89-110">Entity Framework Core provides a set of asynchronous extension methods that can be used as an alternative to the LINQ methods that cause a query to be executed and results returned.</span></span> <span data-ttu-id="b7d89-111">Příklady zahrnují `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`, atd. Neexistují asynchronní verze operátorů `Where(...)`LINQ, jako např., `OrderBy(...)`atd., protože tyto metody sestavují pouze strom výrazů LINQ a nezpůsobí, že dotaz bude proveden v databázi.</span><span class="sxs-lookup"><span data-stu-id="b7d89-111">Examples include `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`, etc. There are not async versions of LINQ operators such as `Where(...)`, `OrderBy(...)`, etc. because these methods only build up the LINQ expression tree and do not cause the query to be executed in the database.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="b7d89-112">Metody rozšíření EF Core Async jsou definovány v `Microsoft.EntityFrameworkCore` oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="b7d89-112">The EF Core async extension methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="b7d89-113">Tento obor názvů musí být importován, aby byly metody k dispozici.</span><span class="sxs-lookup"><span data-stu-id="b7d89-113">This namespace must be imported for the methods to be available.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Async/Sample.cs#Sample)]
