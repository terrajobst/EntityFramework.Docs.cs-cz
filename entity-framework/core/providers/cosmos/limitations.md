---
title: Omezení poskytovatele Azure Cosmos DB – EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 09/12/2019
ms.assetid: 9d02a2cd-484e-4687-b8a8-3748ba46dbc9
uid: core/providers/cosmos/limitations
ms.openlocfilehash: 8dcc82a68c89e21ad1902a0bbbce8ebbc3535801
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/20/2019
ms.locfileid: "71150807"
---
# <a name="ef-core-azure-cosmos-db-provider-limitations"></a><span data-ttu-id="a2cad-102">Omezení poskytovatele EF Core Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="a2cad-102">EF Core Azure Cosmos DB Provider Limitations</span></span>

<span data-ttu-id="a2cad-103">Poskytovatel Cosmos má několik omezení.</span><span class="sxs-lookup"><span data-stu-id="a2cad-103">The Cosmos provider has a number of limitations.</span></span> <span data-ttu-id="a2cad-104">Mnohé z těchto omezení jsou výsledkem omezení v podkladovém databázovém stroji Cosmos a nejsou specifické pro EF.</span><span class="sxs-lookup"><span data-stu-id="a2cad-104">Many of these limitations are a result of limitations in the underlying Cosmos database engine and are not specific to EF.</span></span> <span data-ttu-id="a2cad-105">Ale většina se [ještě neimplementovala](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).</span><span class="sxs-lookup"><span data-stu-id="a2cad-105">But most simply [haven't been implemented yet](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).</span></span>

## <a name="temporary-limitations"></a><span data-ttu-id="a2cad-106">Dočasná omezení</span><span class="sxs-lookup"><span data-stu-id="a2cad-106">Temporary limitations</span></span>

- <span data-ttu-id="a2cad-107">I v případě, že existuje pouze jeden typ entity bez dědění dědičnosti na kontejner, má stále vlastnost diskriminátor.</span><span class="sxs-lookup"><span data-stu-id="a2cad-107">Even if there is only one entity type without inheritance mapped to a container it still has a discriminator property.</span></span>
- <span data-ttu-id="a2cad-108">Typy entit s klíči oddílů v některých scénářích nefungují správně.</span><span class="sxs-lookup"><span data-stu-id="a2cad-108">Entity types with partition keys don't work correctly in some scenarios</span></span>
- <span data-ttu-id="a2cad-109">`Include`volání nejsou podporovaná.</span><span class="sxs-lookup"><span data-stu-id="a2cad-109">`Include` calls are not supported</span></span>
- <span data-ttu-id="a2cad-110">`Join`volání nejsou podporovaná.</span><span class="sxs-lookup"><span data-stu-id="a2cad-110">`Join` calls are not supported</span></span>

## <a name="azure-cosmos-db-sdk-limitations"></a><span data-ttu-id="a2cad-111">Omezení Azure Cosmos DB SDK</span><span class="sxs-lookup"><span data-stu-id="a2cad-111">Azure Cosmos DB SDK limitations</span></span>

- <span data-ttu-id="a2cad-112">Jsou k dispozici pouze asynchronní metody.</span><span class="sxs-lookup"><span data-stu-id="a2cad-112">Only async methods are provided</span></span>

> [!WARNING]
> <span data-ttu-id="a2cad-113">Vzhledem k tomu, že nejsou k dispozici žádné verze synchronizace metod nízké úrovně EF Core spoléhá na, jsou aktuálně implementovány `.Wait()` odpovídající funkce voláním vráceného. `Task`</span><span class="sxs-lookup"><span data-stu-id="a2cad-113">Since there are no sync versions of the low level methods EF Core relies on, the corresponding functionality is currently implemented by calling `.Wait()` on the returned `Task`.</span></span> <span data-ttu-id="a2cad-114">To znamená, že použití metod `SaveChanges`jako nebo `ToList` namísto jejich asynchronních protějšků by mohlo vést k zablokování aplikace</span><span class="sxs-lookup"><span data-stu-id="a2cad-114">This means that using methods like `SaveChanges`, or `ToList` instead of their async counterparts could lead to a deadlock in your application</span></span>

## <a name="azure-cosmos-db-limitations"></a><span data-ttu-id="a2cad-115">Omezení Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="a2cad-115">Azure Cosmos DB limitations</span></span>

<span data-ttu-id="a2cad-116">Můžete si prohlédnout úplný přehled [Azure Cosmos DB podporovaných funkcí](https://docs.microsoft.com/en-us/azure/cosmos-db/modeling-data), jedná se o nejdůležitější rozdíly oproti relační databázi:</span><span class="sxs-lookup"><span data-stu-id="a2cad-116">You can see the full overview of [Azure Cosmos DB supported features](https://docs.microsoft.com/en-us/azure/cosmos-db/modeling-data), these are the most notable differences compared to a relational database:</span></span>

- <span data-ttu-id="a2cad-117">Transakce iniciované klientem nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="a2cad-117">Client-initiated transactions are not supported</span></span>
- <span data-ttu-id="a2cad-118">Některé dotazy mezi oddíly nejsou v závislosti na závislých operátorech podporovány nebo mnohem pomalejší.</span><span class="sxs-lookup"><span data-stu-id="a2cad-118">Some cross-partition queries are either not supported or much slower depending on the operators involved</span></span>