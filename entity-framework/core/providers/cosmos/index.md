---
title: Poskytovatel Azure Cosmos DB – EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 09/12/2019
ms.assetid: 28264681-4486-4891-888c-be5e4ade24f1
uid: core/providers/cosmos/index
ms.openlocfilehash: 96686256bb93f5828bb21fed167eb57812806390
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813548"
---
# <a name="ef-core-azure-cosmos-db-provider"></a>Poskytovatel EF Core Azure Cosmos DB

>[!NOTE]
> Tento zprostředkovatel je v EF Core 3,0 novinkou.

Tento poskytovatel databáze umožňuje použití Entity Framework Core s Azure Cosmos DB. Poskytovatel se udržuje jako součást [projektu Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

Před čtením této části se důrazně doporučuje seznámení s [Azure Cosmos DB dokumentaci](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction) .

## <a name="install"></a>Instalace

Nainstalujte [balíček NuGet Microsoft. EntityFrameworkCore. Cosmos](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).

# <a name="net-core-clitabdotnet-core-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.Cosmos
```

# <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Cosmos
```

***

## <a name="get-started"></a>Začínáme

> [!TIP]  
> Ukázku tohoto článku můžete zobrazit [na GitHubu](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos).

Podobně jako u jiných poskytovatelů je prvním krokem volání `UseCosmos`:

[!code-csharp[Configuration](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Configuration)]

> [!WARNING]
> Koncový bod a klíč se tady pevně zakódované pro jednoduchost, ale v produkční aplikaci by se měly [ukládat securily](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) .

V tomto příkladu `Order` je jednoduchá entita s odkazem na [vlastní typ](../../modeling/owned-entities.md) `StreetAddress`.

[!code-csharp[Order](../../../../samples/core/Cosmos/ModelBuilding/Order.cs?name=Order)]

[!code-csharp[StreetAddress](../../../../samples/core/Cosmos/ModelBuilding/StreetAddress.cs?name=StreetAddress)]

Ukládání a quering dat se řídí normálním vzorem EF:

[!code-csharp[HelloCosmos](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=HelloCosmos)]

> [!IMPORTANT]
> Volání `EnsureCreated` je nezbytné pro vytvoření požadovaných kolekcí a vložení [počátečních dat](../../modeling/data-seeding.md) , pokud jsou uvedena v modelu. Měla `EnsureCreated` by být však volána pouze během nasazení, nikoli v normálním provozu, protože může způsobit problémy s výkonem.

## <a name="cosmos-specific-model-customization"></a>Přizpůsobení modelu specifického pro Cosmos

Ve výchozím nastavení jsou všechny typy entit namapovány na stejný kontejner pojmenovaný za odvozeným kontextem (`"OrderContext"` v tomto případě). Chcete-li změnit výchozí název kontejneru `HasDefaultContainer`, použijte:

[!code-csharp[DefaultContainer](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=DefaultContainer)]

Chcete-li namapovat typ entity na jiný kontejner `ToContainer`, použijte:

[!code-csharp[Container](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Container)]

Chcete-li identifikovat typ entity, který daná položka představuje EF Core přidá hodnotu diskriminátoru, i když nejsou k dispozici žádné odvozené typy entit. Název a hodnotu diskriminátoru [lze změnit](../../modeling/inheritance.md).

## <a name="embedded-entities"></a>Vložené entity

Pro entity vlastněné v Cosmos jsou vložené do stejné položky jako vlastník. Chcete-li změnit název vlastnosti `ToJsonProperty`, použijte:

[!code-csharp[PropertyNames](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PropertyNames)]

V této konfiguraci je pořadí z výše uvedeného příkladu uloženo takto:

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
    "_rid": "6QEKAM+BOOABAAAAAAAAAA==",
    "_self": "dbs/6QEKAA==/colls/6QEKAM+BOOA=/docs/6QEKAM+BOOABAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683c-692e763901d5\"",
    "_attachments": "attachments/",
    "_ts": 1568163674
}
```

Jsou vložené i kolekce vlastněných entit. V dalším příkladu budeme používat `Distributor` třídu s `StreetAddress`kolekcí:

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
            "Discriminator": "StreetAddress",
            "Street": "500 S 48th Street"
        },
        {
            "City": "Anaheim",
            "Discriminator": "StreetAddress",
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

Interně EF Core vždy musí mít jedinečné hodnoty klíčů pro všechny sledované entity. Primární klíč vytvořený ve výchozím nastavení pro kolekce vlastněných typů se skládá z vlastností cizích klíčů odkazujících na vlastníka a `int` vlastnost odpovídající indexu v poli JSON. Pro načtení těchto hodnot můžete použít rozhraní API pro zápis:

[!code-csharp[ImpliedProperties](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=ImpliedProperties)]

> [!TIP]
> V případě potřeby je možné změnit výchozí primární klíč pro vlastní typy entit, ale hodnoty klíčů by měly být poskytnuty explicitně.

## <a name="working-with-disconnected-entities"></a>Práce s odpojenými entitami

Každá položka musí mít `id` hodnotu, která je pro daný klíč oddílu jedinečná. Ve výchozím nastavení EF Core generuje hodnotu zřetězením diskriminátoru a hodnot primárního klíče pomocí | jako oddělovače. Hodnoty klíče jsou generovány pouze tehdy, když entita vstoupí `Added` do stavu. To může představovat problém při [připojování entit](../../saving/disconnected-entities.md) , pokud `id` nemají vlastnost v typu .NET k uložení hodnoty.

Pokud chcete toto omezení obejít, můžete ho vytvořit a `id` nastavit ručně nebo nejdřív označit entitu jako přidanou, a pak ji změnit na požadovaný stav:

[!code-csharp[Attach](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?highlight=4&name=Attach)]

Toto je výsledný formát JSON:

``` json
{
    "Id": 1,
    "Discriminator": "Order",
    "TrackingNumber": null,
    "id": "Order|1",
    "Address": {
        "ShipsToCity": "London",
        "Discriminator": "StreetAddress",
        "ShipsToStreet": "3 Abbey Road"
    },
    "_rid": "6QEKAM+BOOABAAAAAAAAAA==",
    "_self": "dbs/6QEKAA==/colls/6QEKAM+BOOA=/docs/6QEKAM+BOOABAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683c-8f7ac48f01d5\"",
    "_attachments": "attachments/",
    "_ts": 1568163739
}
```
