---
title: Poskytovatel Azure Cosmos DB – práce s nestrukturovanými daty – EF Core
description: Jak pracovat s Azure Cosmos DB nestrukturovanými daty pomocí Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/cosmos/unstructured-data
ms.openlocfilehash: 69f979d46174ff56310b334f28438ac271f45155
ms.sourcegitcommit: b3cf5d2e3cb170b9916795d1d8c88678269639b1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2020
ms.locfileid: "76888093"
---
# <a name="working-with-unstructured-data-in-ef-core-azure-cosmos-db-provider"></a><span data-ttu-id="322d1-103">Práce s nestrukturovanými daty v EF Core poskytovateli Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="322d1-103">Working with Unstructured Data in EF Core Azure Cosmos DB Provider</span></span>

<span data-ttu-id="322d1-104">EF Core byla navržena tak, aby usnadnila práci s daty, která následují po schématu definovaném v modelu.</span><span class="sxs-lookup"><span data-stu-id="322d1-104">EF Core was designed to make it easy to work with data that follows a schema defined in the model.</span></span> <span data-ttu-id="322d1-105">Jedna z možností Azure Cosmos DB je ale flexibilita ve tvaru uložených dat.</span><span class="sxs-lookup"><span data-stu-id="322d1-105">However one of the strengths of Azure Cosmos DB is the flexibility in the shape of the data stored.</span></span>

## <a name="accessing-the-raw-json"></a><span data-ttu-id="322d1-106">Přístup k nezpracovanému formátu JSON</span><span class="sxs-lookup"><span data-stu-id="322d1-106">Accessing the raw JSON</span></span>

<span data-ttu-id="322d1-107">Je možné získat přístup k vlastnostem, které nejsou sledovány EF Core prostřednictvím speciální vlastnosti ve [stínovém stavu](../../modeling/shadow-properties.md) s názvem `"__jObject"` obsahující `JObject` reprezentující data přijatá ze Storu a data, která budou uložena:</span><span class="sxs-lookup"><span data-stu-id="322d1-107">It is possible to access the properties that are not tracked by EF Core through a special property in [shadow-state](../../modeling/shadow-properties.md) named `"__jObject"` that contains a `JObject` representing the data recieved from the store and data that will be stored:</span></span>

[!code-csharp[Unmapped](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=23,24&name=Unmapped)]

``` json
{
    "Id": 1,
    "PartitionKey": "1",
    "TrackingNumber": null,
    "id": "1",
    "Address": {
        "ShipsToCity": "London",
        "ShipsToStreet": "221 B Baker St"
    },
    "_rid": "eLMaAK8TzkIBAAAAAAAAAA==",
    "_self": "dbs/eLMaAA==/colls/eLMaAK8TzkI=/docs/eLMaAK8TzkIBAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683e-0a12bf8d01d5\"",
    "_attachments": "attachments/",
    "BillingAddress": "Clarence House",
    "_ts": 1568164374
}
```

> [!WARNING]
> <span data-ttu-id="322d1-108">Vlastnost `"__jObject"` je součástí infrastruktury EF Core a měla by se používat jenom jako poslední možnost, protože v budoucích verzích může mít jiné chování.</span><span class="sxs-lookup"><span data-stu-id="322d1-108">The `"__jObject"` property is part of the EF Core infrastructure and should only be used as a last resort as it is likely to have different behavior in future releases.</span></span>

> [!NOTE]
> <span data-ttu-id="322d1-109">Změny v entitě přepíšou hodnoty uložené v `"__jObject"` během `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="322d1-109">Changes to the entity will override the values stored in `"__jObject"` during `SaveChanges`.</span></span>

## <a name="using-cosmosclient"></a><span data-ttu-id="322d1-110">Použití CosmosClient</span><span class="sxs-lookup"><span data-stu-id="322d1-110">Using CosmosClient</span></span>

<span data-ttu-id="322d1-111">Pro úplné oddělit od EF Core získat objekt [CosmosClient](/dotnet/api/Microsoft.Azure.Cosmos.CosmosClient) , který je [součástí sady Azure Cosmos DB SDK](/azure/cosmos-db/sql-api-get-started) od `DbContext`:</span><span class="sxs-lookup"><span data-stu-id="322d1-111">To decouple completely from EF Core get the [CosmosClient](/dotnet/api/Microsoft.Azure.Cosmos.CosmosClient) object that is [part of the Azure Cosmos DB SDK](/azure/cosmos-db/sql-api-get-started) from `DbContext`:</span></span>

[!code-csharp[CosmosClient](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=3&name=CosmosClient)]

## <a name="missing-property-values"></a><span data-ttu-id="322d1-112">Chybějící hodnoty vlastností</span><span class="sxs-lookup"><span data-stu-id="322d1-112">Missing property values</span></span>

<span data-ttu-id="322d1-113">V předchozím příkladu jsme z objednávky odebrali vlastnost `"TrackingNumber"`.</span><span class="sxs-lookup"><span data-stu-id="322d1-113">In the previous example we removed the `"TrackingNumber"` property from the order.</span></span> <span data-ttu-id="322d1-114">Vzhledem k tomu, jak indexování funguje v Cosmos DB, dotazy, které odkazují na chybějící vlastnost někam jinde než v projekci, by mohly vracet neočekávané výsledky.</span><span class="sxs-lookup"><span data-stu-id="322d1-114">Because of how indexing works in Cosmos DB, queries that reference the missing property somewhere else than in the projection could return unexpected results.</span></span> <span data-ttu-id="322d1-115">Příklad:</span><span class="sxs-lookup"><span data-stu-id="322d1-115">For example:</span></span>

[!code-csharp[MissingProperties](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?name=MissingProperties)]

<span data-ttu-id="322d1-116">Seřazený dotaz ve skutečnosti nevrátí žádné výsledky.</span><span class="sxs-lookup"><span data-stu-id="322d1-116">The sorted query actually returns no results.</span></span> <span data-ttu-id="322d1-117">To znamená, že by měl být při přímém práci s obchodem nutné vždy naplnit vlastnosti namapované EF Core.</span><span class="sxs-lookup"><span data-stu-id="322d1-117">This means that one should take care to always populate properties mapped by EF Core when working with the store directly.</span></span>

> [!NOTE]
> <span data-ttu-id="322d1-118">Toto chování se může v budoucích verzích Cosmos změnit.</span><span class="sxs-lookup"><span data-stu-id="322d1-118">This behavior might change in future versions of Cosmos.</span></span> <span data-ttu-id="322d1-119">Například v současné době, pokud zásady indexování definují složený index {ID/?</span><span class="sxs-lookup"><span data-stu-id="322d1-119">For instance, currently if the indexing policy defines the composite index {Id/?</span></span> <span data-ttu-id="322d1-120">ASC, TrackingNumber/?</span><span class="sxs-lookup"><span data-stu-id="322d1-120">ASC, TrackingNumber/?</span></span> <span data-ttu-id="322d1-121">ASC)}, pak dotaz, který má "ORDER BY c.Id ASC, c. diskriminátor __ASC",__ vrátí položky, u kterých chybí vlastnost `"TrackingNumber"`.</span><span class="sxs-lookup"><span data-stu-id="322d1-121">ASC)}, then a query that has 'ORDER BY c.Id ASC, c.Discriminator ASC' __would__ return items that are missing the `"TrackingNumber"` property.</span></span>
