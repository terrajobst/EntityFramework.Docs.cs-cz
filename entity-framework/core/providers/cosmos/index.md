---
title: Poskytovatel Azure Cosmos DB – EF Core
description: Dokumentace pro poskytovatele databáze, která umožňuje použití Entity Framework Core s rozhraním API Azure Cosmos DB SQL
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/cosmos/index
ms.openlocfilehash: 6903aab4911f7478afe3d8987a791ae1c5ccebce
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502211"
---
# <a name="ef-core-azure-cosmos-db-provider"></a>Poskytovatel EF Core Azure Cosmos DB

> [!NOTE]
> Tento zprostředkovatel je v EF Core 3,0 novinkou.

Tento poskytovatel databáze umožňuje použití Entity Framework Core s Azure Cosmos DB. Poskytovatel se udržuje jako součást [projektu Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

Před čtením této části se důrazně doporučuje seznámení s [Azure Cosmos DB dokumentaci](/azure/cosmos-db/introduction) .

> [!NOTE]
> Tento zprostředkovatel funguje jenom s rozhraním SQL API Azure Cosmos DB.

## <a name="install"></a>Instalace produktu

Nainstalujte [balíček NuGet Microsoft. EntityFrameworkCore. Cosmos](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).

### <a name="net-core-clitabdotnet-core-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Cosmos
```

### <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Cosmos
```

***

## <a name="get-started"></a>Začínáme

> [!TIP]  
> Ukázku tohoto článku můžete zobrazit [na GitHubu](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos).

Podobně jako u jiných poskytovatelů je prvním krokem volání [UseCosmos](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosDbContextOptionsExtensions.UseCosmos):

[!code-csharp[Configuration](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Configuration)]

> [!WARNING]
> Koncový bod a klíč se tady pevně zakódované pro jednoduchost, ale v produkční aplikaci by se měly [ukládat securily](/aspnet/core/security/app-secrets#secret-manager) .

V tomto příkladu `Order` je jednoduchá entita s odkazem na [vlastní typ](../../modeling/owned-entities.md) `StreetAddress`.

[!code-csharp[Order](../../../../samples/core/Cosmos/ModelBuilding/Order.cs?name=Order)]

[!code-csharp[StreetAddress](../../../../samples/core/Cosmos/ModelBuilding/StreetAddress.cs?name=StreetAddress)]

Ukládání a quering dat se řídí normálním vzorem EF:

[!code-csharp[HelloCosmos](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=HelloCosmos)]

> [!IMPORTANT]
> Volání [EnsureCreatedAsync](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreatedAsync) je nezbytné pro vytvoření požadovaných kontejnerů a vložení [počátečních dat](../../modeling/data-seeding.md) , pokud jsou uvedena v modelu. `EnsureCreatedAsync` by však měly být volány pouze během nasazení, nikoli v normálním provozu, protože může dojít k problémům s výkonem.

## <a name="cosmos-specific-model-customization"></a>Přizpůsobení modelu specifického pro Cosmos

Ve výchozím nastavení jsou všechny typy entit namapovány na stejný kontejner pojmenovaný za odvozeným kontextem (`"OrderContext"` v tomto případě). Chcete-li změnit výchozí název kontejneru, použijte [HasDefaultContainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosModelBuilderExtensions.HasDefaultContainer):

[!code-csharp[DefaultContainer](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=DefaultContainer)]

Pokud chcete mapovat typ entity na jiný kontejner, použijte [ToContainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToContainer):

[!code-csharp[Container](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Container)]

Chcete-li identifikovat typ entity, který daná položka představuje EF Core přidá hodnotu diskriminátoru, i když nejsou k dispozici žádné odvozené typy entit. Název a hodnotu diskriminátoru [lze změnit](../../modeling/inheritance.md).

Pokud se v jednom kontejneru nebude nikdy ukládat žádný jiný typ entity, může se diskriminátor odebrat voláním [HasNoDiscriminator](/dotnet/api/Microsoft.EntityFrameworkCore.Metadata.Builders.EntityTypeBuilder.HasNoDiscriminator):

[!code-csharp[NoDiscriminator](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=NoDiscriminator)]

### <a name="partition-keys"></a>Klíče oddílu

Ve výchozím nastavení EF Core vytvoří kontejnery s klíčem oddílu nastaveným na `"__partitionKey"` bez zadání jakékoli hodnoty při vkládání položek. Pokud ale chcete plně využívat možnosti výkonu Azure Cosmos, měli byste použít [pečlivě vybraný klíč oddílu](/azure/cosmos-db/partition-data) . Dá se nakonfigurovat voláním [HasPartitionKey](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.HasPartitionKey):

[!code-csharp[PartitionKey](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PartitionKey)]

> [!NOTE]
>Vlastnost klíče oddílu může být libovolného typu, pokud je [převedena na řetězec](xref:core/modeling/value-conversions).

Po nakonfigurování by vlastnost klíče oddílu měla vždycky mít hodnotu, která není null. Při vystavení dotazu je možné přidat podmínku, která bude obsahovat jeden oddíl.

[!code-csharp[PartitionKey](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=PartitionKey)]

## <a name="embedded-entities"></a>Vložené entity

Pro entity vlastněné v Cosmos jsou vložené do stejné položky jako vlastník. Chcete-li změnit název vlastnosti, použijte [ToJsonProperty](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToJsonProperty):

[!code-csharp[PropertyNames](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PropertyNames)]

V této konfiguraci je pořadí z výše uvedeného příkladu uloženo takto:

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
    "_rid": "6QEKAM+BOOABAAAAAAAAAA==",
    "_self": "dbs/6QEKAA==/colls/6QEKAM+BOOA=/docs/6QEKAM+BOOABAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683c-692e763901d5\"",
    "_attachments": "attachments/",
    "_ts": 1568163674
}
```

Jsou vložené i kolekce vlastněných entit. V dalším příkladu použijeme třídu `Distributor` s kolekcí `StreetAddress`:

[!code-csharp[Distributor](../../../../samples/core/Cosmos/ModelBuilding/Distributor.cs?name=Distributor)]

Vlastněné entity nemusejí poskytovat explicitní hodnoty klíčů, které se mají uložit:

[!code-csharp[OwnedCollection](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=OwnedCollection)]

Budou trvale zachované tímto způsobem:

``` json
{
    "Id": 1,
    "Discriminator": "Distributor",
    "id": "Distributor|1",
    "ShippingCenters": [
        {
            "City": "Phoenix",
            "Street": "500 S 48th Street"
        },
        {
            "City": "Anaheim",
            "Street": "5650 Dolly Ave"
        }
    ],
    "_rid": "6QEKANzISj0BAAAAAAAAAA==",
    "_self": "dbs/6QEKAA==/colls/6QEKANzISj0=/docs/6QEKANzISj0BAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683c-7b2b439701d5\"",
    "_attachments": "attachments/",
    "_ts": 1568163705
}
```

Interně EF Core vždy musí mít jedinečné hodnoty klíčů pro všechny sledované entity. Primární klíč vytvořený ve výchozím nastavení pro kolekce vlastněných typů se skládá z vlastností cizích klíčů odkazujících na vlastníka a vlastnost `int` odpovídající indexu v poli JSON. Pro načtení těchto hodnot můžete použít rozhraní API pro zápis:

[!code-csharp[ImpliedProperties](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=ImpliedProperties)]

> [!TIP]
> V případě potřeby je možné změnit výchozí primární klíč pro vlastní typy entit, ale hodnoty klíčů by měly být poskytnuty explicitně.

## <a name="working-with-disconnected-entities"></a>Práce s odpojenými entitami

Každá položka musí mít `id` hodnotu, která je pro daný klíč oddílu jedinečná. Ve výchozím nastavení EF Core generuje hodnotu zřetězením diskriminátoru a hodnot primárního klíče pomocí | jako oddělovače. Hodnoty klíče jsou generovány pouze tehdy, když entita vstoupí do stavu `Added`. To může představovat problém při [připojování entit](../../saving/disconnected-entities.md) , pokud nemají vlastnost `id` v typu .NET k uložení hodnoty.

Pokud chcete toto omezení obejít, můžete vytvořit a nastavit hodnotu `id` ručně nebo označit entitu jako přidanou a pak ji změnit na požadovaný stav:

[!code-csharp[Attach](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?highlight=4&name=Attach)]

Toto je výsledný formát JSON:

``` json
{
    "Id": 1,
    "Discriminator": "Distributor",
    "id": "Distributor|1",
    "ShippingCenters": [
        {
            "City": "Phoenix",
            "Street": "500 S 48th Street"
        }
    ],
    "_rid": "JBwtAN8oNYEBAAAAAAAAAA==",
    "_self": "dbs/JBwtAA==/colls/JBwtAN8oNYE=/docs/JBwtAN8oNYEBAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-9377-d7a1ae7c01d5\"",
    "_attachments": "attachments/",
    "_ts": 1572917100
}
```
