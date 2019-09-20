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
# <a name="working-with-unstructured-data-in-ef-core-azure-cosmos-db-provider"></a>Práce s nestrukturovanými daty v EF Core poskytovateli Azure Cosmos DB

EF Core byla navržena tak, aby usnadnila práci s daty, která následují po schématu definovaném v modelu. Jedna z možností Azure Cosmos DB je ale flexibilita ve tvaru uložených dat.

## <a name="accessing-the-raw-json"></a>Přístup k nezpracovanému formátu JSON

Je možné získat přístup k vlastnostem, které nejsou sledovány EF Core prostřednictvím speciální vlastnosti ve [stínovém stavu](../../modeling/shadow-properties.md) s názvem `"__jObject"` , který obsahuje `JObject` reprezentující data přijatá ze Storu a data, která budou uložena:

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
> Tato `"__jObject"` vlastnost je součástí infrastruktury EF Core a měla by se používat jenom jako poslední způsob, protože v budoucích verzích může mít jiné chování.

> [!NOTE]
> Změny v entitě přepíšou hodnoty uložené v `"__jObject"` průběhu. `SaveChanges`

## <a name="using-cosmosclient"></a>Použití CosmosClient

Chcete-li úplně oddělit od EF Core `CosmosClient` získat objekt, který je [součástí sady Azure Cosmos DB SDK](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-get-started) , z `DbContext`:

[!code-csharp[CosmosClient](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=3&name=CosmosClient)]

## <a name="missing-property-values"></a>Chybějící hodnoty vlastností

V předchozím příkladu jsme odebrali `"TrackingNumber"` vlastnost z objednávky. Vzhledem k tomu, jak indexování funguje v Cosmos DB, dotazy, které odkazují na chybějící vlastnost někam jinde než v projekci, by mohly vracet neočekávané výsledky. Příklad:

[!code-csharp[MissingProperties](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?name=MissingProperties)]

Seřazený dotaz ve skutečnosti nevrátí žádné výsledky. To znamená, že by měl být při přímém práci s obchodem nutné vždy naplnit vlastnosti namapované EF Core.

> [!NOTE]
> Toto chování se může v budoucích verzích Cosmos změnit. Například v současné době, pokud zásady indexování definují složený index {ID/? ASC, TrackingNumber/? ASC)}, pak dotaz, který má "ORDER by c.ID ASC, c. diskriminátor __ASC",__ vrátí položky, u kterých chybí `"TrackingNumber"` vlastnost.