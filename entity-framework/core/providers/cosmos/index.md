---
title: Zprostředkovatel Azure Cosmos DB – ef core
description: Dokumentace pro poskytovatele databáze, která umožňuje core entity frameworku, který se má používat s rozhraním SQL API Azure Cosmos DB
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/cosmos/index
ms.openlocfilehash: 74284bf78f404e376436a1ef5d5933186c85ae49
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417319"
---
# <a name="ef-core-azure-cosmos-db-provider"></a>EF Core Azure Cosmos DB Provider

> [!NOTE]
> Tento poskytovatel je nový v EF Core 3.0.

Tento poskytovatel databáze umožňuje entity Framework Core pro použití s Azure Cosmos DB. Zprostředkovatel je udržován jako součást [základního projektu entity frameworku](https://github.com/aspnet/EntityFrameworkCore).

Důrazně doporučujeme seznámit se s [dokumentací Azure Cosmos DB](/azure/cosmos-db/introduction) před přečtením této části.

> [!NOTE]
> Tento zprostředkovatel funguje jenom s rozhraním SQL API Azure Cosmos DB.

## <a name="install"></a>Instalace

Nainstalujte [balíček Microsoft.EntityFrameworkCore.Cosmos NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).

### <a name="net-core-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Cosmos
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Cosmos
```

***

## <a name="get-started"></a>Začínáme

> [!TIP]  
> Ukázku tohoto článku můžete zobrazit [na GitHubu](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Cosmos).

Stejně jako u jiných poskytovatelů prvním krokem je volání [UseCosmos](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosDbContextOptionsExtensions.UseCosmos):

[!code-csharp[Configuration](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Configuration)]

> [!WARNING]
> Koncový bod a klíč jsou pevně zakódovány zde pro jednoduchost, ale v produkční aplikaci by měly být [uloženy bezpečně](/aspnet/core/security/app-secrets#secret-manager).

V tomto `Order` příkladu je jednoduchá entita s odkazem na [vlastněný typ](../../modeling/owned-entities.md) `StreetAddress`.

[!code-csharp[Order](../../../../samples/core/Cosmos/ModelBuilding/Order.cs?name=Order)]

[!code-csharp[StreetAddress](../../../../samples/core/Cosmos/ModelBuilding/StreetAddress.cs?name=StreetAddress)]

Ukládání a quering data se řídí normální ef vzor:

[!code-csharp[HelloCosmos](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=HelloCosmos)]

> [!IMPORTANT]
> Volání [EnsureCreatedAsync](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreatedAsync) je nezbytné k vytvoření požadovaných kontejnerů a vložení [dat osiva,](../../modeling/data-seeding.md) pokud je k dispozici v modelu. Však `EnsureCreatedAsync` by měla být volána pouze během nasazení, není normální provoz, protože může způsobit problémy s výkonem.

## <a name="cosmos-specific-model-customization"></a>Přizpůsobení modelu specifické pro Cosmos

Ve výchozím nastavení jsou všechny typy entit mapovány na`"OrderContext"` stejný kontejner pojmenovaný podle odvozeného kontextu ( v tomto případě). Chcete-li změnit výchozí název kontejneru, použijte [hasdefaultcontainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosModelBuilderExtensions.HasDefaultContainer):

[!code-csharp[DefaultContainer](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=DefaultContainer)]

Chcete-li namapovat typ entity na jiný kontejner, použijte [ToContainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToContainer):

[!code-csharp[Container](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Container)]

Chcete-li identifikovat typ entity, který daná položka představuje EF Core přidá hodnotu diskriminátoru i v případě, že neexistují žádné odvozené typy entit. Název a hodnotu diskriminátoru [lze změnit](../../modeling/inheritance.md).

Pokud žádný jiný typ entity bude někdy uloženve stejném kontejneru diskriminátor lze odebrat voláním [HasNoDiscriminator](/dotnet/api/Microsoft.EntityFrameworkCore.Metadata.Builders.EntityTypeBuilder.HasNoDiscriminator):

[!code-csharp[NoDiscriminator](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=NoDiscriminator)]

### <a name="partition-keys"></a>Klíče oddílů

Ve výchozím nastavení EF Core vytvoří kontejnery s klíčem oddílu nastavena bez `"__partitionKey"` zadání jakékoli hodnoty pro něj při vkládání položek. Ale plně využít možnosti výkonu Azure Cosmos [pečlivě vybraný klíč oddílu](/azure/cosmos-db/partition-data) by měl být použit. To může být nakonfigurován voláním [HasPartitionKey](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.HasPartitionKey):

[!code-csharp[PartitionKey](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PartitionKey)]

> [!NOTE]
>Vlastnost klíče oddílu může být libovolného typu, pokud je [převedena na řetězec](xref:core/modeling/value-conversions).

Po konfiguraci by vlastnost klíče oddílu měla mít vždy nenulovou hodnotu. Při vydávání dotazu lze přidat podmínku, aby byla jednooddílová.

[!code-csharp[PartitionKey](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=PartitionKey)]

## <a name="embedded-entities"></a>Vložené entity

Pro Cosmos vlastněné entity jsou vloženy do stejné položky jako vlastník. Chcete-li změnit název vlastnosti, použijte [ToJsonProperty](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToJsonProperty):

[!code-csharp[PropertyNames](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PropertyNames)]

S touto konfigurací je pořadí z výše uvedeného příkladu uloženo takto:

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

Kolekce vlastněných entit jsou také vloženy. Pro další příklad použijeme `Distributor` třídu s `StreetAddress`kolekcí :

[!code-csharp[Distributor](../../../../samples/core/Cosmos/ModelBuilding/Distributor.cs?name=Distributor)]

Vlastněné entity nemusí poskytovat explicitní hodnoty klíče, které mají být uloženy:

[!code-csharp[OwnedCollection](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=OwnedCollection)]

Budou přetrvávat tímto způsobem:

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

Interně EF Core musí mít vždy jedinečné hodnoty klíče pro všechny sledované entity. Primární klíč vytvořený ve výchozím nastavení pro kolekce vlastněných typů se skládá `int` z vlastností cizího klíče směřujících na vlastníka a vlastnosti odpovídající indexu v poli JSON. Chcete-li načíst tyto hodnoty vstupní rozhraní API lze použít:

[!code-csharp[ImpliedProperties](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=ImpliedProperties)]

> [!TIP]
> V případě potřeby lze změnit výchozí primární klíč pro typy vlastněných entit, ale pak by měly být explicitně poskytnuty hodnoty klíče.

## <a name="working-with-disconnected-entities"></a>Práce s odpojenými entitami

Každá položka musí `id` mít hodnotu, která je jedinečná pro daný klíč oddílu. Ve výchozím nastavení EF Core generuje hodnotu zřetězením diskriminátoru a hodnot primárního klíče pomocí '|' jako oddělovače. Hodnoty klíče jsou generovány pouze v `Added` případě, že entita vstoupí do stavu. To může představovat problém [při připojování entit,](../../saving/disconnected-entities.md) pokud nemají `id` vlastnost na typu .NET pro uložení hodnoty.

Chcete-li toto omezení obejít, můžete vytvořit a nastavit hodnotu `id` ručně nebo označit entitu jako přidanou jako první a poté ji změnit na požadovaný stav:

[!code-csharp[Attach](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?highlight=4&name=Attach)]

Toto je výsledný JSON:

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
