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
# <a name="working-with-unstructured-data-in-ef-core-azure-cosmos-db-provider"></a>Práce s nestrukturovanými daty v EF Core poskytovateli Azure Cosmos DB

EF Core byla navržena tak, aby usnadnila práci s daty, která následují po schématu definovaném v modelu. Jedna z možností Azure Cosmos DB je ale flexibilita ve tvaru uložených dat.

## <a name="accessing-the-raw-json"></a>Přístup k nezpracovanému formátu JSON

Je možné získat přístup k vlastnostem, které nejsou sledovány EF Core prostřednictvím speciální vlastnosti ve [stínovém stavu](../../modeling/shadow-properties.md) s názvem `"__jObject"` obsahující `JObject` reprezentující data přijatá ze Storu a data, která budou uložena:

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
> Vlastnost `"__jObject"` je součástí infrastruktury EF Core a měla by se používat jenom jako poslední možnost, protože v budoucích verzích může mít jiné chování.

> [!NOTE]
> Změny v entitě přepíšou hodnoty uložené v `"__jObject"` během `SaveChanges`.

## <a name="using-cosmosclient"></a>Použití CosmosClient

Pro úplné oddělit od EF Core získat objekt [CosmosClient](/dotnet/api/Microsoft.Azure.Cosmos.CosmosClient) , který je [součástí sady Azure Cosmos DB SDK](/azure/cosmos-db/sql-api-get-started) od `DbContext`:

[!code-csharp[CosmosClient](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=3&name=CosmosClient)]

## <a name="missing-property-values"></a>Chybějící hodnoty vlastností

V předchozím příkladu jsme z objednávky odebrali vlastnost `"TrackingNumber"`. Vzhledem k tomu, jak indexování funguje v Cosmos DB, dotazy, které odkazují na chybějící vlastnost někam jinde než v projekci, by mohly vracet neočekávané výsledky. Příklad:

[!code-csharp[MissingProperties](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?name=MissingProperties)]

Seřazený dotaz ve skutečnosti nevrátí žádné výsledky. To znamená, že by měl být při přímém práci s obchodem nutné vždy naplnit vlastnosti namapované EF Core.

> [!NOTE]
> Toto chování se může v budoucích verzích Cosmos změnit. Například v současné době, pokud zásady indexování definují složený index {ID/? ASC, TrackingNumber/? ASC)}, pak dotaz, který má "ORDER BY c.Id ASC, c. diskriminátor __ASC",__ vrátí položky, u kterých chybí vlastnost `"TrackingNumber"`.
