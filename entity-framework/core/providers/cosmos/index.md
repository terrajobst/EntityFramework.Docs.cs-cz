---
title: Poskytovatel Azure Cosmos DB – EF Core
description: Dokumentace pro poskytovatele databáze, která umožňuje použití Entity Framework Core s rozhraním API Azure Cosmos DB SQL
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/cosmos/index
ms.openlocfilehash: 6cac695288d9ba84968b7fab6361f55e9b51be67
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656093"
---
# <a name="ef-core-azure-cosmos-db-provider"></a><span data-ttu-id="be061-103">Poskytovatel EF Core Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="be061-103">EF Core Azure Cosmos DB Provider</span></span>

>[!NOTE]
> <span data-ttu-id="be061-104">Tento zprostředkovatel je v EF Core 3,0 novinkou.</span><span class="sxs-lookup"><span data-stu-id="be061-104">This provider is new in EF Core 3.0.</span></span>

<span data-ttu-id="be061-105">Tento poskytovatel databáze umožňuje použití Entity Framework Core s Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="be061-105">This database provider allows Entity Framework Core to be used with Azure Cosmos DB.</span></span> <span data-ttu-id="be061-106">Poskytovatel se udržuje jako součást [projektu Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="be061-106">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

<span data-ttu-id="be061-107">Před čtením této části se důrazně doporučuje seznámení s [Azure Cosmos DB dokumentaci](/azure/cosmos-db/introduction) .</span><span class="sxs-lookup"><span data-stu-id="be061-107">It is strongly recommended to familiarize yourself with the [Azure Cosmos DB documentation](/azure/cosmos-db/introduction) before reading this section.</span></span>

>[!NOTE]
> <span data-ttu-id="be061-108">Tento zprostředkovatel funguje jenom s rozhraním SQL API Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="be061-108">This provider only works with the SQL API of Azure Cosmos DB.</span></span>

## <a name="install"></a><span data-ttu-id="be061-109">Instalace</span><span class="sxs-lookup"><span data-stu-id="be061-109">Install</span></span>

<span data-ttu-id="be061-110">Nainstalujte [balíček NuGet Microsoft. EntityFrameworkCore. Cosmos](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).</span><span class="sxs-lookup"><span data-stu-id="be061-110">Install the [Microsoft.EntityFrameworkCore.Cosmos NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).</span></span>

## <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="be061-111">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="be061-111">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.Cosmos
```

## <a name="visual-studiotabvs"></a>[<span data-ttu-id="be061-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="be061-112">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Cosmos
```

***

## <a name="get-started"></a><span data-ttu-id="be061-113">Začínáme</span><span class="sxs-lookup"><span data-stu-id="be061-113">Get started</span></span>

> [!TIP]  
> <span data-ttu-id="be061-114">Ukázku tohoto článku můžete zobrazit [na GitHubu](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos).</span><span class="sxs-lookup"><span data-stu-id="be061-114">You can view this article's [sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos).</span></span>

<span data-ttu-id="be061-115">Podobně jako u jiných poskytovatelů je prvním krokem volání [UseCosmos](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosDbContextOptionsExtensions.UseCosmos):</span><span class="sxs-lookup"><span data-stu-id="be061-115">Like for other providers the first step is to call [UseCosmos](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosDbContextOptionsExtensions.UseCosmos):</span></span>

[!code-csharp[Configuration](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Configuration)]

> [!WARNING]
> <span data-ttu-id="be061-116">Koncový bod a klíč se tady pevně zakódované pro jednoduchost, ale v produkční aplikaci by se měly [ukládat securily](/aspnet/core/security/app-secrets#secret-manager) .</span><span class="sxs-lookup"><span data-stu-id="be061-116">The endpoint and key are hardcoded here for simplicity, but in a production app these should be [stored securily](/aspnet/core/security/app-secrets#secret-manager)</span></span>

<span data-ttu-id="be061-117">V tomto příkladu `Order` je jednoduchá entita s odkazem na [vlastní typ](../../modeling/owned-entities.md) `StreetAddress`.</span><span class="sxs-lookup"><span data-stu-id="be061-117">In this example `Order` is a simple entity with a reference to the [owned type](../../modeling/owned-entities.md) `StreetAddress`.</span></span>

[!code-csharp[Order](../../../../samples/core/Cosmos/ModelBuilding/Order.cs?name=Order)]

[!code-csharp[StreetAddress](../../../../samples/core/Cosmos/ModelBuilding/StreetAddress.cs?name=StreetAddress)]

<span data-ttu-id="be061-118">Ukládání a quering dat se řídí normálním vzorem EF:</span><span class="sxs-lookup"><span data-stu-id="be061-118">Saving and quering data follows the normal EF pattern:</span></span>

[!code-csharp[HelloCosmos](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=HelloCosmos)]

> [!IMPORTANT]
> <span data-ttu-id="be061-119">Volání [EnsureCreatedAsync](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreatedAsync) je nezbytné pro vytvoření požadovaných kontejnerů a vložení [počátečních dat](../../modeling/data-seeding.md) , pokud jsou uvedena v modelu.</span><span class="sxs-lookup"><span data-stu-id="be061-119">Calling [EnsureCreatedAsync](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreatedAsync) is necessary to create the required containers and insert the [seed data](../../modeling/data-seeding.md) if present in the model.</span></span> <span data-ttu-id="be061-120">`EnsureCreatedAsync` by však měly být volány pouze během nasazení, nikoli v normálním provozu, protože může dojít k problémům s výkonem.</span><span class="sxs-lookup"><span data-stu-id="be061-120">However `EnsureCreatedAsync` should only be called during deployment, not normal operation, as it may cause performance issues.</span></span>

## <a name="cosmos-specific-model-customization"></a><span data-ttu-id="be061-121">Přizpůsobení modelu specifického pro Cosmos</span><span class="sxs-lookup"><span data-stu-id="be061-121">Cosmos-specific model customization</span></span>

<span data-ttu-id="be061-122">Ve výchozím nastavení jsou všechny typy entit namapovány na stejný kontejner pojmenovaný za odvozeným kontextem (`"OrderContext"` v tomto případě).</span><span class="sxs-lookup"><span data-stu-id="be061-122">By default all entity types are mapped to the same container, named after the derived context (`"OrderContext"` in this case).</span></span> <span data-ttu-id="be061-123">Chcete-li změnit výchozí název kontejneru, použijte [HasDefaultContainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosModelBuilderExtensions.HasDefaultContainer):</span><span class="sxs-lookup"><span data-stu-id="be061-123">To change the default container name use [HasDefaultContainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosModelBuilderExtensions.HasDefaultContainer):</span></span>

[!code-csharp[DefaultContainer](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=DefaultContainer)]

<span data-ttu-id="be061-124">Pokud chcete mapovat typ entity na jiný kontejner, použijte [ToContainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToContainer):</span><span class="sxs-lookup"><span data-stu-id="be061-124">To map an entity type to a different container use [ToContainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToContainer):</span></span>

[!code-csharp[Container](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Container)]

<span data-ttu-id="be061-125">Chcete-li identifikovat typ entity, který daná položka představuje EF Core přidá hodnotu diskriminátoru, i když nejsou k dispozici žádné odvozené typy entit.</span><span class="sxs-lookup"><span data-stu-id="be061-125">To identify the entity type that a given item represent EF Core adds a discriminator value even if there are no derived entity types.</span></span> <span data-ttu-id="be061-126">Název a hodnotu diskriminátoru [lze změnit](../../modeling/inheritance.md).</span><span class="sxs-lookup"><span data-stu-id="be061-126">The name and value of the discriminator [can be changed](../../modeling/inheritance.md).</span></span>

<span data-ttu-id="be061-127">Pokud se v jednom kontejneru nebude nikdy ukládat žádný jiný typ entity, může se diskriminátor odebrat voláním [HasNoDiscriminator](/dotnet/api/Microsoft.EntityFrameworkCore.Metadata.Builders.EntityTypeBuilder.HasNoDiscriminator):</span><span class="sxs-lookup"><span data-stu-id="be061-127">If no other entity type will ever be stored in the same container the discriminator can be removed by calling [HasNoDiscriminator](/dotnet/api/Microsoft.EntityFrameworkCore.Metadata.Builders.EntityTypeBuilder.HasNoDiscriminator):</span></span>

[!code-csharp[NoDiscriminator](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=NoDiscriminator)]

### <a name="partition-keys"></a><span data-ttu-id="be061-128">Klíče oddílu</span><span class="sxs-lookup"><span data-stu-id="be061-128">Partition keys</span></span>

<span data-ttu-id="be061-129">Ve výchozím nastavení EF Core vytvoří kontejnery s klíčem oddílu nastaveným na `"__partitionKey"` bez zadání jakékoli hodnoty při vkládání položek.</span><span class="sxs-lookup"><span data-stu-id="be061-129">By default EF Core will create containers with the partition key set to `"__partitionKey"` without supplying any value for it when inserting items.</span></span> <span data-ttu-id="be061-130">Pokud ale chcete plně využívat možnosti výkonu Azure Cosmos, měli byste použít [pečlivě vybraný klíč oddílu](/azure/cosmos-db/partition-data) .</span><span class="sxs-lookup"><span data-stu-id="be061-130">But to fully leverage the performance capabilities of Azure Cosmos a [carefully selected partition key](/azure/cosmos-db/partition-data) should be used.</span></span> <span data-ttu-id="be061-131">Dá se nakonfigurovat voláním [HasPartitionKey](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.HasPartitionKey):</span><span class="sxs-lookup"><span data-stu-id="be061-131">It can be configured by calling [HasPartitionKey](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.HasPartitionKey):</span></span>

[!code-csharp[PartitionKey](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PartitionKey)]

>[!NOTE]
><span data-ttu-id="be061-132">Vlastnost klíče oddílu může být libovolného typu, pokud je [převedena na řetězec](xref:core/modeling/value-conversions).</span><span class="sxs-lookup"><span data-stu-id="be061-132">The partition key property can be of any type as long as it is [converted to string](xref:core/modeling/value-conversions).</span></span>

<span data-ttu-id="be061-133">Po nakonfigurování by vlastnost klíče oddílu měla vždycky mít hodnotu, která není null.</span><span class="sxs-lookup"><span data-stu-id="be061-133">Once configured the partition key property should always have a non-null value.</span></span> <span data-ttu-id="be061-134">Při vystavení dotazu je možné přidat podmínku, která bude obsahovat jeden oddíl.</span><span class="sxs-lookup"><span data-stu-id="be061-134">When issuing a query a condition can be added to make it single-partition.</span></span>

[!code-csharp[PartitionKey](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=PartitionKey)]

## <a name="embedded-entities"></a><span data-ttu-id="be061-135">Vložené entity</span><span class="sxs-lookup"><span data-stu-id="be061-135">Embedded entities</span></span>

<span data-ttu-id="be061-136">Pro entity vlastněné v Cosmos jsou vložené do stejné položky jako vlastník.</span><span class="sxs-lookup"><span data-stu-id="be061-136">For Cosmos owned entities are embedded in the same item as the owner.</span></span> <span data-ttu-id="be061-137">Chcete-li změnit název vlastnosti, použijte [ToJsonProperty](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToJsonProperty):</span><span class="sxs-lookup"><span data-stu-id="be061-137">To change a property name use [ToJsonProperty](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToJsonProperty):</span></span>

[!code-csharp[PropertyNames](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PropertyNames)]

<span data-ttu-id="be061-138">V této konfiguraci je pořadí z výše uvedeného příkladu uloženo takto:</span><span class="sxs-lookup"><span data-stu-id="be061-138">With this configuration the order from the example above is stored like this:</span></span>

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

<span data-ttu-id="be061-139">Jsou vložené i kolekce vlastněných entit.</span><span class="sxs-lookup"><span data-stu-id="be061-139">Collections of owned entities are embedded as well.</span></span> <span data-ttu-id="be061-140">V dalším příkladu použijeme třídu `Distributor` s kolekcí `StreetAddress`:</span><span class="sxs-lookup"><span data-stu-id="be061-140">For the next example we'll use the `Distributor` class with a collection of `StreetAddress`:</span></span>

[!code-csharp[Distributor](../../../../samples/core/Cosmos/ModelBuilding/Distributor.cs?name=Distributor)]

<span data-ttu-id="be061-141">Vlastněné entity nemusejí poskytovat explicitní hodnoty klíčů, které se mají uložit:</span><span class="sxs-lookup"><span data-stu-id="be061-141">The owned entities don't need to provide explicit key values to be stored:</span></span>

[!code-csharp[OwnedCollection](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=OwnedCollection)]

<span data-ttu-id="be061-142">Budou trvale zachované tímto způsobem:</span><span class="sxs-lookup"><span data-stu-id="be061-142">They will be persisted in this way:</span></span>

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

<span data-ttu-id="be061-143">Interně EF Core vždy musí mít jedinečné hodnoty klíčů pro všechny sledované entity.</span><span class="sxs-lookup"><span data-stu-id="be061-143">Internally EF Core always needs to have unique key values for all tracked entities.</span></span> <span data-ttu-id="be061-144">Primární klíč vytvořený ve výchozím nastavení pro kolekce vlastněných typů se skládá z vlastností cizích klíčů odkazujících na vlastníka a vlastnost `int` odpovídající indexu v poli JSON.</span><span class="sxs-lookup"><span data-stu-id="be061-144">The primary key created by default for collections of owned types consists of the foreign key properties pointing to the owner and an `int` property corresponding to the index in the JSON array.</span></span> <span data-ttu-id="be061-145">Pro načtení těchto hodnot můžete použít rozhraní API pro zápis:</span><span class="sxs-lookup"><span data-stu-id="be061-145">To retrieve these values entry API could be used:</span></span>

[!code-csharp[ImpliedProperties](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=ImpliedProperties)]

> [!TIP]
> <span data-ttu-id="be061-146">V případě potřeby je možné změnit výchozí primární klíč pro vlastní typy entit, ale hodnoty klíčů by měly být poskytnuty explicitně.</span><span class="sxs-lookup"><span data-stu-id="be061-146">When necessary the default primary key for the owned entity types can be changed, but then key values should be provided explicitly.</span></span>

## <a name="working-with-disconnected-entities"></a><span data-ttu-id="be061-147">Práce s odpojenými entitami</span><span class="sxs-lookup"><span data-stu-id="be061-147">Working with disconnected entities</span></span>

<span data-ttu-id="be061-148">Každá položka musí mít `id` hodnotu, která je pro daný klíč oddílu jedinečná.</span><span class="sxs-lookup"><span data-stu-id="be061-148">Every item needs to have an `id` value that is unique for the given partition key.</span></span> <span data-ttu-id="be061-149">Ve výchozím nastavení EF Core generuje hodnotu zřetězením diskriminátoru a hodnot primárního klíče pomocí | jako oddělovače.</span><span class="sxs-lookup"><span data-stu-id="be061-149">By default EF Core generates the value by concatenating the discriminator and the primary key values, using '|' as a delimiter.</span></span> <span data-ttu-id="be061-150">Hodnoty klíče jsou generovány pouze tehdy, když entita vstoupí do stavu `Added`.</span><span class="sxs-lookup"><span data-stu-id="be061-150">The key values are only generated when an entity enters the `Added` state.</span></span> <span data-ttu-id="be061-151">To může představovat problém při [připojování entit](../../saving/disconnected-entities.md) , pokud nemají vlastnost `id` v typu .NET k uložení hodnoty.</span><span class="sxs-lookup"><span data-stu-id="be061-151">This might pose a problem when [attaching entities](../../saving/disconnected-entities.md) if they don't have an `id` property on the .NET type to store the value.</span></span>

<span data-ttu-id="be061-152">Pokud chcete toto omezení obejít, můžete vytvořit a nastavit hodnotu `id` ručně nebo označit entitu jako přidanou a pak ji změnit na požadovaný stav:</span><span class="sxs-lookup"><span data-stu-id="be061-152">To work around this limitation one could create and set the `id` value manually or mark the entity as added first, then changing it to the desired state:</span></span>

[!code-csharp[Attach](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?highlight=4&name=Attach)]

<span data-ttu-id="be061-153">Toto je výsledný formát JSON:</span><span class="sxs-lookup"><span data-stu-id="be061-153">This is the resulting JSON:</span></span>

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
