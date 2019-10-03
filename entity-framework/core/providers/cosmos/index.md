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
# <a name="ef-core-azure-cosmos-db-provider"></a><span data-ttu-id="97b5f-102">Poskytovatel EF Core Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="97b5f-102">EF Core Azure Cosmos DB Provider</span></span>

>[!NOTE]
> <span data-ttu-id="97b5f-103">Tento zprostředkovatel je v EF Core 3,0 novinkou.</span><span class="sxs-lookup"><span data-stu-id="97b5f-103">This provider is new in EF Core 3.0.</span></span>

<span data-ttu-id="97b5f-104">Tento poskytovatel databáze umožňuje použití Entity Framework Core s Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="97b5f-104">This database provider allows Entity Framework Core to be used with Azure Cosmos DB.</span></span> <span data-ttu-id="97b5f-105">Poskytovatel se udržuje jako součást [projektu Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="97b5f-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

<span data-ttu-id="97b5f-106">Před čtením této části se důrazně doporučuje seznámení s [Azure Cosmos DB dokumentaci](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction) .</span><span class="sxs-lookup"><span data-stu-id="97b5f-106">It is strongly recommended to familiarize yourself with the [Azure Cosmos DB documentation](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction) before reading this section.</span></span>

## <a name="install"></a><span data-ttu-id="97b5f-107">Instalace</span><span class="sxs-lookup"><span data-stu-id="97b5f-107">Install</span></span>

<span data-ttu-id="97b5f-108">Nainstalujte [balíček NuGet Microsoft. EntityFrameworkCore. Cosmos](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).</span><span class="sxs-lookup"><span data-stu-id="97b5f-108">Install the [Microsoft.EntityFrameworkCore.Cosmos NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).</span></span>

# <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="97b5f-109">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="97b5f-109">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.Cosmos
```

# <a name="visual-studiotabvs"></a>[<span data-ttu-id="97b5f-110">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="97b5f-110">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Cosmos
```

***

## <a name="get-started"></a><span data-ttu-id="97b5f-111">Začínáme</span><span class="sxs-lookup"><span data-stu-id="97b5f-111">Get Started</span></span>

> [!TIP]  
> <span data-ttu-id="97b5f-112">Ukázku tohoto článku můžete zobrazit [na GitHubu](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos).</span><span class="sxs-lookup"><span data-stu-id="97b5f-112">You can view this article's [sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos).</span></span>

<span data-ttu-id="97b5f-113">Podobně jako u jiných poskytovatelů je prvním krokem volání `UseCosmos`:</span><span class="sxs-lookup"><span data-stu-id="97b5f-113">Like for other providers the first step is to call `UseCosmos`:</span></span>

[!code-csharp[Configuration](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Configuration)]

> [!WARNING]
> <span data-ttu-id="97b5f-114">Koncový bod a klíč se tady pevně zakódované pro jednoduchost, ale v produkční aplikaci by se měly [ukládat securily](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) .</span><span class="sxs-lookup"><span data-stu-id="97b5f-114">The endpoint and key are hardcoded here for simplicity, but in a production app these should be [stored securily](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager)</span></span>

<span data-ttu-id="97b5f-115">V tomto příkladu `Order` je jednoduchá entita s odkazem na [vlastní typ](../../modeling/owned-entities.md) `StreetAddress`.</span><span class="sxs-lookup"><span data-stu-id="97b5f-115">In this example `Order` is a simple entity with a reference to the [owned type](../../modeling/owned-entities.md) `StreetAddress`.</span></span>

[!code-csharp[Order](../../../../samples/core/Cosmos/ModelBuilding/Order.cs?name=Order)]

[!code-csharp[StreetAddress](../../../../samples/core/Cosmos/ModelBuilding/StreetAddress.cs?name=StreetAddress)]

<span data-ttu-id="97b5f-116">Ukládání a quering dat se řídí normálním vzorem EF:</span><span class="sxs-lookup"><span data-stu-id="97b5f-116">Saving and quering data follows the normal EF pattern:</span></span>

[!code-csharp[HelloCosmos](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=HelloCosmos)]

> [!IMPORTANT]
> <span data-ttu-id="97b5f-117">Volání `EnsureCreated` je nezbytné pro vytvoření požadovaných kolekcí a vložení [počátečních dat](../../modeling/data-seeding.md) , pokud jsou uvedena v modelu.</span><span class="sxs-lookup"><span data-stu-id="97b5f-117">Calling `EnsureCreated` is necessary to create the required collections and insert the [seed data](../../modeling/data-seeding.md) if present in the model.</span></span> <span data-ttu-id="97b5f-118">Měla `EnsureCreated` by být však volána pouze během nasazení, nikoli v normálním provozu, protože může způsobit problémy s výkonem.</span><span class="sxs-lookup"><span data-stu-id="97b5f-118">However `EnsureCreated` should only be called during deployment, not normal operation, as it may cause performance issues.</span></span>

## <a name="cosmos-specific-model-customization"></a><span data-ttu-id="97b5f-119">Přizpůsobení modelu specifického pro Cosmos</span><span class="sxs-lookup"><span data-stu-id="97b5f-119">Cosmos-specific Model Customization</span></span>

<span data-ttu-id="97b5f-120">Ve výchozím nastavení jsou všechny typy entit namapovány na stejný kontejner pojmenovaný za odvozeným kontextem (`"OrderContext"` v tomto případě).</span><span class="sxs-lookup"><span data-stu-id="97b5f-120">By default all entity types are mapped to the same container, named after the derived context (`"OrderContext"` in this case).</span></span> <span data-ttu-id="97b5f-121">Chcete-li změnit výchozí název kontejneru `HasDefaultContainer`, použijte:</span><span class="sxs-lookup"><span data-stu-id="97b5f-121">To change the default container name use `HasDefaultContainer`:</span></span>

[!code-csharp[DefaultContainer](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=DefaultContainer)]

<span data-ttu-id="97b5f-122">Chcete-li namapovat typ entity na jiný kontejner `ToContainer`, použijte:</span><span class="sxs-lookup"><span data-stu-id="97b5f-122">To map an entity type to a different container use `ToContainer`:</span></span>

[!code-csharp[Container](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Container)]

<span data-ttu-id="97b5f-123">Chcete-li identifikovat typ entity, který daná položka představuje EF Core přidá hodnotu diskriminátoru, i když nejsou k dispozici žádné odvozené typy entit.</span><span class="sxs-lookup"><span data-stu-id="97b5f-123">To identify the entity type that a given item represent EF Core adds a discriminator value even if there are no derived entity types.</span></span> <span data-ttu-id="97b5f-124">Název a hodnotu diskriminátoru [lze změnit](../../modeling/inheritance.md).</span><span class="sxs-lookup"><span data-stu-id="97b5f-124">The name and value of the discriminator [can be changed](../../modeling/inheritance.md).</span></span>

## <a name="embedded-entities"></a><span data-ttu-id="97b5f-125">Vložené entity</span><span class="sxs-lookup"><span data-stu-id="97b5f-125">Embedded Entities</span></span>

<span data-ttu-id="97b5f-126">Pro entity vlastněné v Cosmos jsou vložené do stejné položky jako vlastník.</span><span class="sxs-lookup"><span data-stu-id="97b5f-126">For Cosmos owned entities are embedded in the same item as the owner.</span></span> <span data-ttu-id="97b5f-127">Chcete-li změnit název vlastnosti `ToJsonProperty`, použijte:</span><span class="sxs-lookup"><span data-stu-id="97b5f-127">To change a property name use `ToJsonProperty`:</span></span>

[!code-csharp[PropertyNames](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PropertyNames)]

<span data-ttu-id="97b5f-128">V této konfiguraci je pořadí z výše uvedeného příkladu uloženo takto:</span><span class="sxs-lookup"><span data-stu-id="97b5f-128">With this configuration the order from the example above is stored like this:</span></span>

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

<span data-ttu-id="97b5f-129">Jsou vložené i kolekce vlastněných entit.</span><span class="sxs-lookup"><span data-stu-id="97b5f-129">Collections of owned entities are embedded as well.</span></span> <span data-ttu-id="97b5f-130">V dalším příkladu budeme používat `Distributor` třídu s `StreetAddress`kolekcí:</span><span class="sxs-lookup"><span data-stu-id="97b5f-130">For the next example we'll use the `Distributor` class with a collection of `StreetAddress`:</span></span>

[!code-csharp[Distributor](../../../../samples/core/Cosmos/ModelBuilding/Distributor.cs?name=Distributor)]

<span data-ttu-id="97b5f-131">Vlastněné entity nemusejí poskytovat explicitní hodnoty klíčů, které se mají uložit:</span><span class="sxs-lookup"><span data-stu-id="97b5f-131">The owned entities don't need to provide explicit key values to be stored:</span></span>

[!code-csharp[OwnedCollection](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=OwnedCollection)]

<span data-ttu-id="97b5f-132">Budou trvale zachované tímto způsobem:</span><span class="sxs-lookup"><span data-stu-id="97b5f-132">They will be persisted in this way:</span></span>

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

<span data-ttu-id="97b5f-133">Interně EF Core vždy musí mít jedinečné hodnoty klíčů pro všechny sledované entity.</span><span class="sxs-lookup"><span data-stu-id="97b5f-133">Internally EF Core always needs to have unique key values for all tracked entities.</span></span> <span data-ttu-id="97b5f-134">Primární klíč vytvořený ve výchozím nastavení pro kolekce vlastněných typů se skládá z vlastností cizích klíčů odkazujících na vlastníka a `int` vlastnost odpovídající indexu v poli JSON.</span><span class="sxs-lookup"><span data-stu-id="97b5f-134">The primary key created by default for collections of owned types consists of the foreign key properties pointing to the owner and an `int` property corresponding to the index in the JSON array.</span></span> <span data-ttu-id="97b5f-135">Pro načtení těchto hodnot můžete použít rozhraní API pro zápis:</span><span class="sxs-lookup"><span data-stu-id="97b5f-135">To retrieve these values entry API could be used:</span></span>

[!code-csharp[ImpliedProperties](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=ImpliedProperties)]

> [!TIP]
> <span data-ttu-id="97b5f-136">V případě potřeby je možné změnit výchozí primární klíč pro vlastní typy entit, ale hodnoty klíčů by měly být poskytnuty explicitně.</span><span class="sxs-lookup"><span data-stu-id="97b5f-136">When necessary the default primary key for the owned entity types can be changed, but then key values should be provided explicitly.</span></span>

## <a name="working-with-disconnected-entities"></a><span data-ttu-id="97b5f-137">Práce s odpojenými entitami</span><span class="sxs-lookup"><span data-stu-id="97b5f-137">Working with Disconnected Entities</span></span>

<span data-ttu-id="97b5f-138">Každá položka musí mít `id` hodnotu, která je pro daný klíč oddílu jedinečná.</span><span class="sxs-lookup"><span data-stu-id="97b5f-138">Every item needs to have an `id` value that is unique for the given partition key.</span></span> <span data-ttu-id="97b5f-139">Ve výchozím nastavení EF Core generuje hodnotu zřetězením diskriminátoru a hodnot primárního klíče pomocí | jako oddělovače.</span><span class="sxs-lookup"><span data-stu-id="97b5f-139">By default EF Core generates the value by concatenating the discriminator and the primary key values, using '|' as a delimiter.</span></span> <span data-ttu-id="97b5f-140">Hodnoty klíče jsou generovány pouze tehdy, když entita vstoupí `Added` do stavu.</span><span class="sxs-lookup"><span data-stu-id="97b5f-140">The key values are only generated when an entity enters the `Added` state.</span></span> <span data-ttu-id="97b5f-141">To může představovat problém při [připojování entit](../../saving/disconnected-entities.md) , pokud `id` nemají vlastnost v typu .NET k uložení hodnoty.</span><span class="sxs-lookup"><span data-stu-id="97b5f-141">This might pose a problem when [attaching entities](../../saving/disconnected-entities.md) if they don't have an `id` property on the .NET type to store the value.</span></span>

<span data-ttu-id="97b5f-142">Pokud chcete toto omezení obejít, můžete ho vytvořit a `id` nastavit ručně nebo nejdřív označit entitu jako přidanou, a pak ji změnit na požadovaný stav:</span><span class="sxs-lookup"><span data-stu-id="97b5f-142">To work around this limitation one could create and set the `id` value manually or mark the entity as added first, then changing it to the desired state:</span></span>

[!code-csharp[Attach](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?highlight=4&name=Attach)]

<span data-ttu-id="97b5f-143">Toto je výsledný formát JSON:</span><span class="sxs-lookup"><span data-stu-id="97b5f-143">This is the resulting JSON:</span></span>

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
