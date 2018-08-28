---
title: Asynchronní dotazy – EF Core
author: rowanmiller
ms.date: 01/24/2017
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: de00e25279e29355a4eb3e55597a8578ceccecb6
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993562"
---
# <a name="asynchronous-queries"></a><span data-ttu-id="c8891-102">Asynchronní dotazy</span><span class="sxs-lookup"><span data-stu-id="c8891-102">Asynchronous Queries</span></span>

<span data-ttu-id="c8891-103">Asynchronní dotazy Vyhněte se blokování vlákna, zatímco je dotaz proveden v databázi.</span><span class="sxs-lookup"><span data-stu-id="c8891-103">Asynchronous queries avoid blocking a thread while the query is executed in the database.</span></span> <span data-ttu-id="c8891-104">To může být užitečné, aby se zabránilo zmrazení uživatelského rozhraní tlustá klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="c8891-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="c8891-105">Asynchronní operace může také zvýšit propustnost ve webové aplikaci, ve kterém můžete vlákno uvolnit pro ostatní žádosti o služby, zatímco se dokončují operace databáze.</span><span class="sxs-lookup"><span data-stu-id="c8891-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="c8891-106">Další informace najdete v tématu [asynchronní programování v jazyce C#](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="c8891-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="c8891-107">EF Core nepodporuje více paralelních operací běží na stejné instance kontextu.</span><span class="sxs-lookup"><span data-stu-id="c8891-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="c8891-108">Vždy můžete čekat pro operaci dokončit před zahájením další operaci.</span><span class="sxs-lookup"><span data-stu-id="c8891-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="c8891-109">To se obvykle provádí pomocí `await` – klíčové slovo na každou asynchronní operaci.</span><span class="sxs-lookup"><span data-stu-id="c8891-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="c8891-110">Entity Framework Core obsahuje sadu asynchronní rozšiřující metody, které lze použít jako alternativu k LINQ metody, které způsobí provedení dotazu a výsledky.</span><span class="sxs-lookup"><span data-stu-id="c8891-110">Entity Framework Core provides a set of asynchronous extension methods that can be used as an alternative to the LINQ methods that cause a query to be executed and results returned.</span></span> <span data-ttu-id="c8891-111">Mezi příklady patří `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`atd. Nejsou žádná asynchronní verze operátory LINQ, jako `Where(...)`, `OrderBy(...)`a tak dále, protože tyto metody pouze sestavení stromu výrazů LINQ a nevyvolávají dotazu má být proveden v databázi.</span><span class="sxs-lookup"><span data-stu-id="c8891-111">Examples include `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`, etc. There are not async versions of LINQ operators such as `Where(...)`, `OrderBy(...)`, etc. because these methods only build up the LINQ expression tree and do not cause the query to be executed in the database.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="c8891-112">Rozšiřující metody pro asynchronní EF Core jsou definovány v `Microsoft.EntityFrameworkCore` oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="c8891-112">The EF Core async extension methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="c8891-113">Tento obor názvů musí být naimportovány pro metody musí být k dispozici.</span><span class="sxs-lookup"><span data-stu-id="c8891-113">This namespace must be imported for the methods to be available.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/Async/Sample.cs#Sample)]
