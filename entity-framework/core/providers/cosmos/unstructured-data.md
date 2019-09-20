---
title: Poskytovatel Azure Cosmos DB – práce s nestrukturovanými daty – EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 09/12/2019
ms.assetid: b47d41b6-984f-419a-ab10-2ed3b95e3919
uid: core/providers/cosmos/unstructured-data
ms.openlocfilehash: 86bb0f7915c8a2561e7d5cd5dffc27474218a112
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/20/2019
ms.locfileid: "71150814"
---
# <a name="working-with-unstructured-data-in-ef-core-azure-cosmos-db-provider"></a><span data-ttu-id="dc96b-102">Práce s nestrukturovanými daty v EF Core poskytovateli Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="dc96b-102">Working with Unstructured Data in EF Core Azure Cosmos DB Provider</span></span>

<span data-ttu-id="dc96b-103">EF Core byla navržena tak, aby usnadnila práci s daty, která následují po schématu definovaném v modelu.</span><span class="sxs-lookup"><span data-stu-id="dc96b-103">EF Core was designed to make it easy to work with data that follows a schema defined in the model.</span></span> <span data-ttu-id="dc96b-104">Jedna z možností Azure Cosmos DB je ale flexibilita ve tvaru uložených dat.</span><span class="sxs-lookup"><span data-stu-id="dc96b-104">However one of the strengths of Azure Cosmos DB is the flexibility in the shape of the data stored.</span></span>

## <a name="accessing-the-raw-json"></a><span data-ttu-id="dc96b-105">Přístup k nezpracovanému formátu JSON</span><span class="sxs-lookup"><span data-stu-id="dc96b-105">Accessing the raw JSON</span></span>

<span data-ttu-id="dc96b-106">Je možné získat přístup k vlastnostem, které nejsou sledovány EF Core prostřednictvím speciální vlastnosti ve [stínovém stavu](../../modeling/shadow-properties.md) s názvem `"__jObject"` , který obsahuje `JObject` reprezentující data přijatá ze Storu a data, která budou uložena:</span><span class="sxs-lookup"><span data-stu-id="dc96b-106">It is possible to access the properties that are not tracked by EF Core through a special property in [shadow-state](../../modeling/shadow-properties.md) named `"__jObject"` that contains a `JObject` representing the data recieved from the store and data that will be stored:</span></span>

[!code-csharp[Unmapped](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=21-23&name=Unmapped)]

``` json
{
    "Id": 1,
    "Discriminator": "Order",
    "TrackingNumber": null,
    "id": "Order|1",
    "Address": {
        "ShipsToCity": "London",
        "Discriminator": "StreetAddress",
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
> <span data-ttu-id="dc96b-107">Tato `"__jObject"` vlastnost je součástí infrastruktury EF Core a měla by se používat jenom jako poslední způsob, protože v budoucích verzích může mít jiné chování.</span><span class="sxs-lookup"><span data-stu-id="dc96b-107">The `"__jObject"` property is part of the EF Core infrastructure and should only be used as a last resort as it is likely to have different behavior in future releases.</span></span>

> [!NOTE]
> <span data-ttu-id="dc96b-108">Změny v entitě přepíšou hodnoty uložené v `"__jObject"` průběhu. `SaveChanges`</span><span class="sxs-lookup"><span data-stu-id="dc96b-108">Changes to the entity will override the values stored in `"__jObject"` during `SaveChanges`.</span></span>

## <a name="using-cosmosclient"></a><span data-ttu-id="dc96b-109">Použití CosmosClient</span><span class="sxs-lookup"><span data-stu-id="dc96b-109">Using CosmosClient</span></span>

<span data-ttu-id="dc96b-110">Chcete-li úplně oddělit od EF Core `CosmosClient` získat objekt, který je [součástí sady Azure Cosmos DB SDK](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-get-started) , z `DbContext`:</span><span class="sxs-lookup"><span data-stu-id="dc96b-110">To decouple completely from EF Core get the `CosmosClient` object that is [part of the Azure Cosmos DB SDK](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-get-started) from `DbContext`:</span></span>

[!code-csharp[CosmosClient](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=3&name=CosmosClient)]

## <a name="missing-property-values"></a><span data-ttu-id="dc96b-111">Chybějící hodnoty vlastností</span><span class="sxs-lookup"><span data-stu-id="dc96b-111">Missing property values</span></span>

<span data-ttu-id="dc96b-112">V předchozím příkladu jsme odebrali `"TrackingNumber"` vlastnost z objednávky.</span><span class="sxs-lookup"><span data-stu-id="dc96b-112">In the previous example we removed the `"TrackingNumber"` property from the order.</span></span> <span data-ttu-id="dc96b-113">Vzhledem k tomu, jak indexování funguje v Cosmos DB, dotazy, které odkazují na chybějící vlastnost někam jinde než v projekci, by mohly vracet neočekávané výsledky.</span><span class="sxs-lookup"><span data-stu-id="dc96b-113">Because of how indexing works in Cosmos DB, queries that reference the missing property somewhere else than in the projection could return unexpected results.</span></span> <span data-ttu-id="dc96b-114">Příklad:</span><span class="sxs-lookup"><span data-stu-id="dc96b-114">For example:</span></span>

[!code-csharp[MissingProperties](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?name=MissingProperties)]

<span data-ttu-id="dc96b-115">Seřazený dotaz ve skutečnosti nevrátí žádné výsledky.</span><span class="sxs-lookup"><span data-stu-id="dc96b-115">The sorted query actually returns no results.</span></span> <span data-ttu-id="dc96b-116">To znamená, že by měl být při přímém práci s obchodem nutné vždy naplnit vlastnosti namapované EF Core.</span><span class="sxs-lookup"><span data-stu-id="dc96b-116">This means that one should take care to always populate properties mapped by EF Core when working with the store directly.</span></span>

> [!NOTE]
> <span data-ttu-id="dc96b-117">Toto chování se může v budoucích verzích Cosmos změnit.</span><span class="sxs-lookup"><span data-stu-id="dc96b-117">This behavior might change in future versions of Cosmos.</span></span> <span data-ttu-id="dc96b-118">Například v současné době, pokud zásady indexování definují složený index {ID/?</span><span class="sxs-lookup"><span data-stu-id="dc96b-118">For instance, currently if the indexing policy defines the composite index {Id/?</span></span> <span data-ttu-id="dc96b-119">ASC, TrackingNumber/?</span><span class="sxs-lookup"><span data-stu-id="dc96b-119">ASC, TrackingNumber/?</span></span> <span data-ttu-id="dc96b-120">ASC)}, pak dotaz, který má "ORDER by c.ID ASC, c. diskriminátor __ASC",__ vrátí položky, u kterých chybí `"TrackingNumber"` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="dc96b-120">ASC)}, then a query the has 'ORDER BY c.Id ASC, c.Discriminator ASC' __would__ return items that are missing the `"TrackingNumber"` property.</span></span>