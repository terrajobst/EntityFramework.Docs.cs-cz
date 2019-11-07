---
title: Omezení poskytovatele Azure Cosmos DB – EF Core
description: Omezení poskytovatele Entity Framework Core Azure Cosmos DB
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/cosmos/limitations
ms.openlocfilehash: 2631526b152d6ddcacf25173c8d51e4e3cb24500
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655990"
---
# <a name="ef-core-azure-cosmos-db-provider-limitations"></a><span data-ttu-id="50e68-103">Omezení poskytovatele EF Core Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="50e68-103">EF Core Azure Cosmos DB Provider Limitations</span></span>

<span data-ttu-id="50e68-104">Poskytovatel Cosmos má několik omezení.</span><span class="sxs-lookup"><span data-stu-id="50e68-104">The Cosmos provider has a number of limitations.</span></span> <span data-ttu-id="50e68-105">Mnohé z těchto omezení jsou výsledkem omezení v podkladovém databázovém stroji Cosmos a nejsou specifické pro EF.</span><span class="sxs-lookup"><span data-stu-id="50e68-105">Many of these limitations are a result of limitations in the underlying Cosmos database engine and are not specific to EF.</span></span> <span data-ttu-id="50e68-106">Ale většina se [ještě neimplementovala](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).</span><span class="sxs-lookup"><span data-stu-id="50e68-106">But most simply [haven't been implemented yet](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).</span></span>

## <a name="temporary-limitations"></a><span data-ttu-id="50e68-107">Dočasná omezení</span><span class="sxs-lookup"><span data-stu-id="50e68-107">Temporary limitations</span></span>

- <span data-ttu-id="50e68-108">I v případě, že existuje pouze jeden typ entity bez dědění dědičnosti na kontejner, má stále vlastnost diskriminátor.</span><span class="sxs-lookup"><span data-stu-id="50e68-108">Even if there is only one entity type without inheritance mapped to a container it still has a discriminator property.</span></span>
- <span data-ttu-id="50e68-109">Typy entit s klíči oddílů v některých scénářích nefungují správně.</span><span class="sxs-lookup"><span data-stu-id="50e68-109">Entity types with partition keys don't work correctly in some scenarios</span></span>
- <span data-ttu-id="50e68-110">`Include` volání nejsou podporovaná.</span><span class="sxs-lookup"><span data-stu-id="50e68-110">`Include` calls are not supported</span></span>
- <span data-ttu-id="50e68-111">`Join` volání nejsou podporovaná.</span><span class="sxs-lookup"><span data-stu-id="50e68-111">`Join` calls are not supported</span></span>

## <a name="azure-cosmos-db-sdk-limitations"></a><span data-ttu-id="50e68-112">Omezení Azure Cosmos DB SDK</span><span class="sxs-lookup"><span data-stu-id="50e68-112">Azure Cosmos DB SDK limitations</span></span>

- <span data-ttu-id="50e68-113">Jsou k dispozici pouze asynchronní metody.</span><span class="sxs-lookup"><span data-stu-id="50e68-113">Only async methods are provided</span></span>

> [!WARNING]
> <span data-ttu-id="50e68-114">Vzhledem k tomu, že nejsou k dispozici žádné verze synchronizace metod nízké úrovně EF Core spoléhá na, jsou aktuálně implementovány odpovídající funkce voláním `.Wait()` na vrácené `Task`.</span><span class="sxs-lookup"><span data-stu-id="50e68-114">Since there are no sync versions of the low level methods EF Core relies on, the corresponding functionality is currently implemented by calling `.Wait()` on the returned `Task`.</span></span> <span data-ttu-id="50e68-115">To znamená, že použití metod jako `SaveChanges`nebo `ToList` namísto jejich asynchronních protějšků by mohlo vést k zablokování vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="50e68-115">This means that using methods like `SaveChanges`, or `ToList` instead of their async counterparts could lead to a deadlock in your application</span></span>

## <a name="azure-cosmos-db-limitations"></a><span data-ttu-id="50e68-116">Omezení Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="50e68-116">Azure Cosmos DB limitations</span></span>

<span data-ttu-id="50e68-117">Můžete si prohlédnout úplný přehled [Azure Cosmos DB podporovaných funkcí](/azure/cosmos-db/modeling-data), jedná se o nejdůležitější rozdíly oproti relační databázi:</span><span class="sxs-lookup"><span data-stu-id="50e68-117">You can see the full overview of [Azure Cosmos DB supported features](/azure/cosmos-db/modeling-data), these are the most notable differences compared to a relational database:</span></span>

- <span data-ttu-id="50e68-118">Transakce iniciované klientem nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="50e68-118">Client-initiated transactions are not supported</span></span>
- <span data-ttu-id="50e68-119">Některé dotazy mezi oddíly nejsou v závislosti na závislých operátorech podporovány nebo mnohem pomalejší.</span><span class="sxs-lookup"><span data-stu-id="50e68-119">Some cross-partition queries are either not supported or much slower depending on the operators involved</span></span>
