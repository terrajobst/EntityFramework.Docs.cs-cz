---
title: Asynchronní dotazy – jádro EF
author: smitpatel
ms.date: 10/03/2019
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: ce26db32a616dac5edac2a8451014ae63cbfc12d
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417751"
---
# <a name="asynchronous-queries"></a><span data-ttu-id="316fe-102">Asynchronní dotazy</span><span class="sxs-lookup"><span data-stu-id="316fe-102">Asynchronous Queries</span></span>

<span data-ttu-id="316fe-103">Asynchronní dotazy vyhnout blokování podprocesu při spuštění dotazu v databázi.</span><span class="sxs-lookup"><span data-stu-id="316fe-103">Asynchronous queries avoid blocking a thread while the query is executed in the database.</span></span> <span data-ttu-id="316fe-104">Asynchronní dotazy jsou důležité pro udržování responzivní uživatelské rozhraní v aplikacích tlusté klienta.</span><span class="sxs-lookup"><span data-stu-id="316fe-104">Async queries are important for keeping a responsive UI in thick-client applications.</span></span> <span data-ttu-id="316fe-105">Mohou také zvýšit propustnost ve webových aplikacích, kde uvolní vlákno pro obsluhu jiných požadavků ve webových aplikacích.</span><span class="sxs-lookup"><span data-stu-id="316fe-105">They can also increase throughput in web applications where they free up the thread to service other requests in web applications.</span></span> <span data-ttu-id="316fe-106">Další informace naleznete [v tématu Asynchronní programování v jazyce C#](/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="316fe-106">For more information, see [Asynchronous Programming in C#](/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="316fe-107">EF Core nepodporuje více paralelních operací spuštěna na stejné instanci kontextu.</span><span class="sxs-lookup"><span data-stu-id="316fe-107">EF Core doesn't support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="316fe-108">Před zahájením další operace byste měli vždy počkat na dokončení operace.</span><span class="sxs-lookup"><span data-stu-id="316fe-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="316fe-109">To se obvykle provádí `await` pomocí klíčového slova pro každou asynchronní operaci.</span><span class="sxs-lookup"><span data-stu-id="316fe-109">This is typically done by using the `await` keyword on each async operation.</span></span>

<span data-ttu-id="316fe-110">Entity Framework Core poskytuje sadu asynchronních rozšiřujících metod podobných metodám LINQ, které spouštějí dotaz a vracejí výsledky.</span><span class="sxs-lookup"><span data-stu-id="316fe-110">Entity Framework Core provides a set of async extension methods similar to the LINQ methods, which execute a query and return results.</span></span> <span data-ttu-id="316fe-111">Příklady `ToListAsync()`zahrnují `ToArrayAsync()` `SingleAsync()`, , .</span><span class="sxs-lookup"><span data-stu-id="316fe-111">Examples include `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`.</span></span> <span data-ttu-id="316fe-112">Neexistují žádné asynchronní verze některých operátorů LINQ, například `Where(...)` nebo `OrderBy(...)` protože tyto metody pouze vytvářejí strom výrazů LINQ a nezpůsobují spuštění dotazu v databázi.</span><span class="sxs-lookup"><span data-stu-id="316fe-112">There are no async versions of some LINQ operators such as `Where(...)` or `OrderBy(...)` because these methods only build up the LINQ expression tree and don't cause the query to be executed in the database.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="316fe-113">Metody rozšíření async EF Core `Microsoft.EntityFrameworkCore` jsou definovány v oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="316fe-113">The EF Core async extension methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="316fe-114">Tento obor názvů musí být importován, aby byly metody k dispozici.</span><span class="sxs-lookup"><span data-stu-id="316fe-114">This namespace must be imported for the methods to be available.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Async/Sample.cs#ToListAsync)]
