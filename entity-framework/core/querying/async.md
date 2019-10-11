---
title: Asynchronní dotazy – EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: ce26db32a616dac5edac2a8451014ae63cbfc12d
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181826"
---
# <a name="asynchronous-queries"></a><span data-ttu-id="efd4d-102">Asynchronní dotazy</span><span class="sxs-lookup"><span data-stu-id="efd4d-102">Asynchronous Queries</span></span>

<span data-ttu-id="efd4d-103">Asynchronní dotazy zabraňují zablokování vlákna při spuštění dotazu v databázi.</span><span class="sxs-lookup"><span data-stu-id="efd4d-103">Asynchronous queries avoid blocking a thread while the query is executed in the database.</span></span> <span data-ttu-id="efd4d-104">Asynchronní dotazy jsou důležité pro udržení reagujícího uživatelského rozhraní ve silných klientských aplikacích.</span><span class="sxs-lookup"><span data-stu-id="efd4d-104">Async queries are important for keeping a responsive UI in thick-client applications.</span></span> <span data-ttu-id="efd4d-105">Můžou také zvýšit propustnost webových aplikací, kde uvolní vlákno pro obsluhu dalších požadavků ve webových aplikacích.</span><span class="sxs-lookup"><span data-stu-id="efd4d-105">They can also increase throughput in web applications where they free up the thread to service other requests in web applications.</span></span> <span data-ttu-id="efd4d-106">Další informace naleznete v tématu [asynchronní programování v C# ](/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="efd4d-106">For more information, see [Asynchronous Programming in C#](/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="efd4d-107">EF Core nepodporuje spouštění více paralelních operací ve stejné instanci kontextu.</span><span class="sxs-lookup"><span data-stu-id="efd4d-107">EF Core doesn't support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="efd4d-108">Před zahájením další operace byste vždy měli počkat na dokončení operace.</span><span class="sxs-lookup"><span data-stu-id="efd4d-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="efd4d-109">To se obvykle provádí pomocí klíčového slova `await` u každé asynchronní operace.</span><span class="sxs-lookup"><span data-stu-id="efd4d-109">This is typically done by using the `await` keyword on each async operation.</span></span>

<span data-ttu-id="efd4d-110">Entity Framework Core poskytuje sadu asynchronních metod rozšíření podobných metodám LINQ, které spustí dotaz a vrátí výsledky.</span><span class="sxs-lookup"><span data-stu-id="efd4d-110">Entity Framework Core provides a set of async extension methods similar to the LINQ methods, which execute a query and return results.</span></span> <span data-ttu-id="efd4d-111">Mezi příklady patří `ToListAsync()`, `ToArrayAsync()` `SingleAsync()`.</span><span class="sxs-lookup"><span data-stu-id="efd4d-111">Examples include `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`.</span></span> <span data-ttu-id="efd4d-112">Neexistují žádné asynchronní verze některých operátorů LINQ, například `Where(...)` nebo `OrderBy(...)`, protože tyto metody sestavují pouze strom výrazů LINQ a nezpůsobí, že by se dotaz spustil v databázi.</span><span class="sxs-lookup"><span data-stu-id="efd4d-112">There are no async versions of some LINQ operators such as `Where(...)` or `OrderBy(...)` because these methods only build up the LINQ expression tree and don't cause the query to be executed in the database.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="efd4d-113">Metody rozšíření EF Core Async jsou definovány v oboru názvů `Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="efd4d-113">The EF Core async extension methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="efd4d-114">Tento obor názvů musí být importován, aby byly metody k dispozici.</span><span class="sxs-lookup"><span data-stu-id="efd4d-114">This namespace must be imported for the methods to be available.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Async/Sample.cs#ToListAsync)]
