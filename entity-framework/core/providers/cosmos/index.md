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
# <a name="ef-core-azure-cosmos-db-provider"></a><span data-ttu-id="b0c5c-103">EF Core Azure Cosmos DB Provider</span><span class="sxs-lookup"><span data-stu-id="b0c5c-103">EF Core Azure Cosmos DB Provider</span></span>

> [!NOTE]
> <span data-ttu-id="b0c5c-104">Tento poskytovatel je nový v EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="b0c5c-104">This provider is new in EF Core 3.0.</span></span>

<span data-ttu-id="b0c5c-105">Tento poskytovatel databáze umožňuje entity Framework Core pro použití s Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="b0c5c-105">This database provider allows Entity Framework Core to be used with Azure Cosmos DB.</span></span> <span data-ttu-id="b0c5c-106">Zprostředkovatel je udržován jako součást [základního projektu entity frameworku](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="b0c5c-106">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

<span data-ttu-id="b0c5c-107">Důrazně doporučujeme seznámit se s [dokumentací Azure Cosmos DB](/azure/cosmos-db/introduction) před přečtením této části.</span><span class="sxs-lookup"><span data-stu-id="b0c5c-107">It is strongly recommended to familiarize yourself with the [Azure Cosmos DB documentation](/azure/cosmos-db/introduction) before reading this section.</span></span>

> [!NOTE]
> <span data-ttu-id="b0c5c-108">Tento zprostředkovatel funguje jenom s rozhraním SQL API Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="b0c5c-108">This provider only works with the SQL API of Azure Cosmos DB.</span></span>

## <a name="install"></a><span data-ttu-id="b0c5c-109">Instalace</span><span class="sxs-lookup"><span data-stu-id="b0c5c-109">Install</span></span>

<span data-ttu-id="b0c5c-110">Nainstalujte [balíček Microsoft.EntityFrameworkCore.Cosmos NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).</span><span class="sxs-lookup"><span data-stu-id="b0c5c-110">Install the [Microsoft.EntityFrameworkCore.Cosmos NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="b0c5c-111">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="b0c5c-111">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Cosmos
```

### <a name="visual-studio"></a>[<span data-ttu-id="b0c5c-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b0c5c-112">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Cosmos
```

***

## <a name="get-started"></a><span data-ttu-id="b0c5c-113">Začínáme</span><span class="sxs-lookup"><span data-stu-id="b0c5c-113">Get started</span></span>

> [!TIP]  
> <span data-ttu-id="b0c5c-114">Ukázku tohoto článku můžete zobrazit [na GitHubu](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Cosmos).</span><span class="sxs-lookup"><span data-stu-id="b0c5c-114">You can view this article's [sample on GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Cosmos).</span></span>

<span data-ttu-id="b0c5c-115">Stejně jako u jiných poskytovatelů prvním krokem je volání [UseCosmos](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosDbContextOptionsExtensions.UseCosmos):</span><span class="sxs-lookup"><span data-stu-id="b0c5c-115">Like for other providers the first step is to call [UseCosmos](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosDbContextOptionsExtensions.UseCosmos):</span></span>

[!code-csharp[Configuration](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Configuration)]

> [!WARNING]
> <span data-ttu-id="b0c5c-116">Koncový bod a klíč jsou pevně zakódovány zde pro jednoduchost, ale v produkční aplikaci by měly být [uloženy bezpečně](/aspnet/core/security/app-secrets#secret-manager).</span><span class="sxs-lookup"><span data-stu-id="b0c5c-116">The endpoint and key are hardcoded here for simplicity, but in a production app these should be [stored securely](/aspnet/core/security/app-secrets#secret-manager).</span></span>

<span data-ttu-id="b0c5c-117">V tomto `Order` příkladu je jednoduchá entita s odkazem na [vlastněný typ](../../modeling/owned-entities.md) `StreetAddress`.</span><span class="sxs-lookup"><span data-stu-id="b0c5c-117">In this example `Order` is a simple entity with a reference to the [owned type](../../modeling/owned-entities.md) `StreetAddress`.</span></span>

[!code-csharp[Order](../../../../samples/core/Cosmos/ModelBuilding/Order.cs?name=Order)]

[!code-csharp[StreetAddress](../../../../samples/core/Cosmos/ModelBuilding/StreetAddress.cs?name=StreetAddress)]

<span data-ttu-id="b0c5c-118">Ukládání a quering data se řídí normální ef vzor:</span><span class="sxs-lookup"><span data-stu-id="b0c5c-118">Saving and quering data follows the normal EF pattern:</span></span>

[!code-csharp[HelloCosmos](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=HelloCosmos)]

> [!IMPORTANT]
> <span data-ttu-id="b0c5c-119">Volání [EnsureCreatedAsync](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreatedAsync) je nezbytné k vytvoření požadovaných kontejnerů a vložení [dat osiva,](../../modeling/data-seeding.md) pokud je k dispozici v modelu.</span><span class="sxs-lookup"><span data-stu-id="b0c5c-119">Calling [EnsureCreatedAsync](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreatedAsync) is necessary to create the required containers and insert the [seed data](../../modeling/data-seeding.md) if present in the model.</span></span> <span data-ttu-id="b0c5c-120">Však `EnsureCreatedAsync` by měla být volána pouze během nasazení, není normální provoz, protože může způsobit problémy s výkonem.</span><span class="sxs-lookup"><span data-stu-id="b0c5c-120">However `EnsureCreatedAsync` should only be called during deployment, not normal operation, as it may cause performance issues.</span></span>

## <a name="cosmos-specific-model-customization"></a><span data-ttu-id="b0c5c-121">Přizpůsobení modelu specifické pro Cosmos</span><span class="sxs-lookup"><span data-stu-id="b0c5c-121">Cosmos-specific model customization</span></span>

<span data-ttu-id="b0c5c-122">Ve výchozím nastavení jsou všechny typy entit mapovány na`"OrderContext"` stejný kontejner pojmenovaný podle odvozeného kontextu ( v tomto případě).</span><span class="sxs-lookup"><span data-stu-id="b0c5c-122">By default all entity types are mapped to the same container, named after the derived context (`"OrderContext"` in this case).</span></span> <span data-ttu-id="b0c5c-123">Chcete-li změnit výchozí název kontejneru, použijte [hasdefaultcontainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosModelBuilderExtensions.HasDefaultContainer):</span><span class="sxs-lookup"><span data-stu-id="b0c5c-123">To change the default container name use [HasDefaultContainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosModelBuilderExtensions.HasDefaultContainer):</span></span>

[!code-csharp[DefaultContainer](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=DefaultContainer)]

<span data-ttu-id="b0c5c-124">Chcete-li namapovat typ entity na jiný kontejner, použijte [ToContainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToContainer):</span><span class="sxs-lookup"><span data-stu-id="b0c5c-124">To map an entity type to a different container use [ToContainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToContainer):</span></span>

[!code-csharp[Container](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Container)]

<span data-ttu-id="b0c5c-125">Chcete-li identifikovat typ entity, který daná položka představuje EF Core přidá hodnotu diskriminátoru i v případě, že neexistují žádné odvozené typy entit.</span><span class="sxs-lookup"><span data-stu-id="b0c5c-125">To identify the entity type that a given item represent EF Core adds a discriminator value even if there are no derived entity types.</span></span> <span data-ttu-id="b0c5c-126">Název a hodnotu diskriminátoru [lze změnit](../../modeling/inheritance.md).</span><span class="sxs-lookup"><span data-stu-id="b0c5c-126">The name and value of the discriminator [can be changed](../../modeling/inheritance.md).</span></span>

<span data-ttu-id="b0c5c-127">Pokud žádný jiný typ entity bude někdy uloženve stejném kontejneru diskriminátor lze odebrat voláním [HasNoDiscriminator](/dotnet/api/Microsoft.EntityFrameworkCore.Metadata.Builders.EntityTypeBuilder.HasNoDiscriminator):</span><span class="sxs-lookup"><span data-stu-id="b0c5c-127">If no other entity type will ever be stored in the same container the discriminator can be removed by calling [HasNoDiscriminator](/dotnet/api/Microsoft.EntityFrameworkCore.Metadata.Builders.EntityTypeBuilder.HasNoDiscriminator):</span></span>

[!code-csharp[NoDiscriminator](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=NoDiscriminator)]

### <a name="partition-keys"></a><span data-ttu-id="b0c5c-128">Klíče oddílů</span><span class="sxs-lookup"><span data-stu-id="b0c5c-128">Partition keys</span></span>

<span data-ttu-id="b0c5c-129">Ve výchozím nastavení EF Core vytvoří kontejnery s klíčem oddílu nastavena bez `"__partitionKey"` zadání jakékoli hodnoty pro něj při vkládání položek.</span><span class="sxs-lookup"><span data-stu-id="b0c5c-129">By default EF Core will create containers with the partition key set to `"__partitionKey"` without supplying any value for it when inserting items.</span></span> <span data-ttu-id="b0c5c-130">Ale plně využít možnosti výkonu Azure Cosmos [pečlivě vybraný klíč oddílu](/azure/cosmos-db/partition-data) by měl být použit.</span><span class="sxs-lookup"><span data-stu-id="b0c5c-130">But to fully leverage the performance capabilities of Azure Cosmos a [carefully selected partition key](/azure/cosmos-db/partition-data) should be used.</span></span> <span data-ttu-id="b0c5c-131">To může být nakonfigurován voláním [HasPartitionKey](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.HasPartitionKey):</span><span class="sxs-lookup"><span data-stu-id="b0c5c-131">It can be configured by calling [HasPartitionKey](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.HasPartitionKey):</span></span>

[!code-csharp[PartitionKey](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PartitionKey)]

> [!NOTE]
><span data-ttu-id="b0c5c-132">Vlastnost klíče oddílu může být libovolného typu, pokud je [převedena na řetězec](xref:core/modeling/value-conversions).</span><span class="sxs-lookup"><span data-stu-id="b0c5c-132">The partition key property can be of any type as long as it is [converted to string](xref:core/modeling/value-conversions).</span></span>

<span data-ttu-id="b0c5c-133">Po konfiguraci by vlastnost klíče oddílu měla mít vždy nenulovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="b0c5c-133">Once configured the partition key property should always have a non-null value.</span></span> <span data-ttu-id="b0c5c-134">Při vydávání dotazu lze přidat podmínku, aby byla jednooddílová.</span><span class="sxs-lookup"><span data-stu-id="b0c5c-134">When issuing a query a condition can be added to make it single-partition.</span></span>

[!code-csharp[PartitionKey](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=PartitionKey)]

## <a name="embedded-entities"></a><span data-ttu-id="b0c5c-135">Vložené entity</span><span class="sxs-lookup"><span data-stu-id="b0c5c-135">Embedded entities</span></span>

<span data-ttu-id="b0c5c-136">Pro Cosmos vlastněné entity jsou vloženy do stejné položky jako vlastník.</span><span class="sxs-lookup"><span data-stu-id="b0c5c-136">For Cosmos owned entities are embedded in the same item as the owner.</span></span> <span data-ttu-id="b0c5c-137">Chcete-li změnit název vlastnosti, použijte [ToJsonProperty](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToJsonProperty):</span><span class="sxs-lookup"><span data-stu-id="b0c5c-137">To change a property name use [ToJsonProperty](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToJsonProperty):</span></span>

[!code-csharp[PropertyNames](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PropertyNames)]

<span data-ttu-id="b0c5c-138">S touto konfigurací je pořadí z výše uvedeného příkladu uloženo takto:</span><span class="sxs-lookup"><span data-stu-id="b0c5c-138">With this configuration the order from the example above is stored like this:</span></span>

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

<span data-ttu-id="b0c5c-139">Kolekce vlastněných entit jsou také vloženy.</span><span class="sxs-lookup"><span data-stu-id="b0c5c-139">Collections of owned entities are embedded as well.</span></span> <span data-ttu-id="b0c5c-140">Pro další příklad použijeme `Distributor` třídu s `StreetAddress`kolekcí :</span><span class="sxs-lookup"><span data-stu-id="b0c5c-140">For the next example we'll use the `Distributor` class with a collection of `StreetAddress`:</span></span>

[!code-csharp[Distributor](../../../../samples/core/Cosmos/ModelBuilding/Distributor.cs?name=Distributor)]

<span data-ttu-id="b0c5c-141">Vlastněné entity nemusí poskytovat explicitní hodnoty klíče, které mají být uloženy:</span><span class="sxs-lookup"><span data-stu-id="b0c5c-141">The owned entities don't need to provide explicit key values to be stored:</span></span>

[!code-csharp[OwnedCollection](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=OwnedCollection)]

<span data-ttu-id="b0c5c-142">Budou přetrvávat tímto způsobem:</span><span class="sxs-lookup"><span data-stu-id="b0c5c-142">They will be persisted in this way:</span></span>

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

<span data-ttu-id="b0c5c-143">Interně EF Core musí mít vždy jedinečné hodnoty klíče pro všechny sledované entity.</span><span class="sxs-lookup"><span data-stu-id="b0c5c-143">Internally EF Core always needs to have unique key values for all tracked entities.</span></span> <span data-ttu-id="b0c5c-144">Primární klíč vytvořený ve výchozím nastavení pro kolekce vlastněných typů se skládá `int` z vlastností cizího klíče směřujících na vlastníka a vlastnosti odpovídající indexu v poli JSON.</span><span class="sxs-lookup"><span data-stu-id="b0c5c-144">The primary key created by default for collections of owned types consists of the foreign key properties pointing to the owner and an `int` property corresponding to the index in the JSON array.</span></span> <span data-ttu-id="b0c5c-145">Chcete-li načíst tyto hodnoty vstupní rozhraní API lze použít:</span><span class="sxs-lookup"><span data-stu-id="b0c5c-145">To retrieve these values entry API could be used:</span></span>

[!code-csharp[ImpliedProperties](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=ImpliedProperties)]

> [!TIP]
> <span data-ttu-id="b0c5c-146">V případě potřeby lze změnit výchozí primární klíč pro typy vlastněných entit, ale pak by měly být explicitně poskytnuty hodnoty klíče.</span><span class="sxs-lookup"><span data-stu-id="b0c5c-146">When necessary the default primary key for the owned entity types can be changed, but then key values should be provided explicitly.</span></span>

## <a name="working-with-disconnected-entities"></a><span data-ttu-id="b0c5c-147">Práce s odpojenými entitami</span><span class="sxs-lookup"><span data-stu-id="b0c5c-147">Working with disconnected entities</span></span>

<span data-ttu-id="b0c5c-148">Každá položka musí `id` mít hodnotu, která je jedinečná pro daný klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="b0c5c-148">Every item needs to have an `id` value that is unique for the given partition key.</span></span> <span data-ttu-id="b0c5c-149">Ve výchozím nastavení EF Core generuje hodnotu zřetězením diskriminátoru a hodnot primárního klíče pomocí '|' jako oddělovače.</span><span class="sxs-lookup"><span data-stu-id="b0c5c-149">By default EF Core generates the value by concatenating the discriminator and the primary key values, using '|' as a delimiter.</span></span> <span data-ttu-id="b0c5c-150">Hodnoty klíče jsou generovány pouze v `Added` případě, že entita vstoupí do stavu.</span><span class="sxs-lookup"><span data-stu-id="b0c5c-150">The key values are only generated when an entity enters the `Added` state.</span></span> <span data-ttu-id="b0c5c-151">To může představovat problém [při připojování entit,](../../saving/disconnected-entities.md) pokud nemají `id` vlastnost na typu .NET pro uložení hodnoty.</span><span class="sxs-lookup"><span data-stu-id="b0c5c-151">This might pose a problem when [attaching entities](../../saving/disconnected-entities.md) if they don't have an `id` property on the .NET type to store the value.</span></span>

<span data-ttu-id="b0c5c-152">Chcete-li toto omezení obejít, můžete vytvořit a nastavit hodnotu `id` ručně nebo označit entitu jako přidanou jako první a poté ji změnit na požadovaný stav:</span><span class="sxs-lookup"><span data-stu-id="b0c5c-152">To work around this limitation one could create and set the `id` value manually or mark the entity as added first, then changing it to the desired state:</span></span>

[!code-csharp[Attach](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?highlight=4&name=Attach)]

<span data-ttu-id="b0c5c-153">Toto je výsledný JSON:</span><span class="sxs-lookup"><span data-stu-id="b0c5c-153">This is the resulting JSON:</span></span>

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
