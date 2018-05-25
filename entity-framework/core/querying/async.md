---
title: Asynchronní dotazy – EF jádra
author: rowanmiller
ms.author: divega
ms.date: 01/24/2017
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
ms.technology: entity-framework-core
uid: core/querying/async
ms.openlocfilehash: 6554f04d0edfe0ca2ee72ebed8b878a1997a9500
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="asynchronous-queries"></a><span data-ttu-id="fe0a0-102">Asynchronní dotazy</span><span class="sxs-lookup"><span data-stu-id="fe0a0-102">Asynchronous Queries</span></span>

<span data-ttu-id="fe0a0-103">Asynchronní dotazy Vyhněte se blokování vlákno, zatímco je dotaz proveden v databázi.</span><span class="sxs-lookup"><span data-stu-id="fe0a0-103">Asynchronous queries avoid blocking a thread while the query is executed in the database.</span></span> <span data-ttu-id="fe0a0-104">To může být užitečné, aby se zabránilo zmrazení rozhraní silná klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="fe0a0-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="fe0a0-105">Asynchronní operace může také zvýšit propustnost ve webové aplikaci, kde vlákno mohou být uvolněna pro ostatní žádosti o služby při dokončení operace databáze.</span><span class="sxs-lookup"><span data-stu-id="fe0a0-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="fe0a0-106">Další informace najdete v tématu [asynchronní programování v jazyce C#](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="fe0a0-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="fe0a0-107">Základní EF nepodporuje více paralelních operací se spouští ke stejné instanci kontextu.</span><span class="sxs-lookup"><span data-stu-id="fe0a0-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="fe0a0-108">Vždy by měl počkejte na dokončení před zahájením další operace operace.</span><span class="sxs-lookup"><span data-stu-id="fe0a0-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="fe0a0-109">To se obvykle provádí pomocí `await` – klíčové slovo na každou asynchronní operaci.</span><span class="sxs-lookup"><span data-stu-id="fe0a0-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="fe0a0-110">Entity Framework Core poskytuje sadu metod asynchronní rozšíření, které lze použít jako alternativu k LINQ metody, které způsobí provedení dotazu a výsledky vrácené.</span><span class="sxs-lookup"><span data-stu-id="fe0a0-110">Entity Framework Core provides a set of asynchronous extension methods that can be used as an alternative to the LINQ methods that cause a query to be executed and results returned.</span></span> <span data-ttu-id="fe0a0-111">Mezi příklady patří `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`atd. Nejsou k dispozici asynchronních verzích operátory LINQ jako `Where(...)`, `OrderBy(...)`, atd., protože tyto metody pouze sestavovat strom výrazu LINQ nebo nezpůsobí dotazu, který má být proveden v databázi.</span><span class="sxs-lookup"><span data-stu-id="fe0a0-111">Examples include `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`, etc. There are not async versions of LINQ operators such as `Where(...)`, `OrderBy(...)`, etc. because these methods only build up the LINQ expression tree and do not cause the query to be executed in the database.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="fe0a0-112">Rozšiřující metody pro asynchronní EF základní jsou definovány v `Microsoft.EntityFrameworkCore` oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="fe0a0-112">The EF Core async extension methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="fe0a0-113">Tento obor názvů musí být importovány pro metody být k dispozici.</span><span class="sxs-lookup"><span data-stu-id="fe0a0-113">This namespace must be imported for the methods to be available.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/Async/Sample.cs#Sample)]
