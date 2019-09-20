---
title: Vlastní typy entit – EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 02/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
uid: core/modeling/owned-entities
ms.openlocfilehash: f69bae2de28156876e0aa57376b5dfac053adb9c
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149135"
---
# <a name="owned-entity-types"></a><span data-ttu-id="a350e-102">Vlastněné typy entit</span><span class="sxs-lookup"><span data-stu-id="a350e-102">Owned Entity Types</span></span>

>[!NOTE]
> <span data-ttu-id="a350e-103">Tato funkce je v EF Core 2,0 novinkou.</span><span class="sxs-lookup"><span data-stu-id="a350e-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="a350e-104">EF Core umožňuje modelovat typy entit, které se mohou u vlastností navigace u jiných typů entit vyskytovat pouze někdy.</span><span class="sxs-lookup"><span data-stu-id="a350e-104">EF Core allows you to model entity types that can only ever appear on navigation properties of other entity types.</span></span> <span data-ttu-id="a350e-105">Ty se nazývají _vlastněné typy entit_.</span><span class="sxs-lookup"><span data-stu-id="a350e-105">These are called _owned entity types_.</span></span> <span data-ttu-id="a350e-106">Entita obsahující typ entity, která je vlastníkem, je jeho _vlastník_.</span><span class="sxs-lookup"><span data-stu-id="a350e-106">The entity containing an owned entity type is its _owner_.</span></span>

<span data-ttu-id="a350e-107">Vlastněné entity jsou v podstatě součástí vlastníka a nemůže existovat bez nich, jsou koncepčně podobné [agregacím](https://martinfowler.com/bliki/DDD_Aggregate.html).</span><span class="sxs-lookup"><span data-stu-id="a350e-107">Owned entities are essentially a part of the owner and cannot exist without it, they are conceptually similar to [aggregates](https://martinfowler.com/bliki/DDD_Aggregate.html).</span></span>

## <a name="explicit-configuration"></a><span data-ttu-id="a350e-108">Explicitní konfigurace</span><span class="sxs-lookup"><span data-stu-id="a350e-108">Explicit configuration</span></span>

<span data-ttu-id="a350e-109">Vlastní typy entit nejsou nikdy zahrnuty EF Core v modelu podle konvence.</span><span class="sxs-lookup"><span data-stu-id="a350e-109">Owned entity types are never included by EF Core in the model by convention.</span></span> <span data-ttu-id="a350e-110">Můžete použít `OwnsOne` metodu v `OnModelCreating` nebo `OwnedAttribute` opatřit poznámkami k typu (novinkou v EF Core 2,1) ke konfiguraci typu jako vlastněný typ.</span><span class="sxs-lookup"><span data-stu-id="a350e-110">You can use the `OwnsOne` method in `OnModelCreating` or annotate the type with `OwnedAttribute` (new in EF Core 2.1) to configure the type as an owned type.</span></span>

<span data-ttu-id="a350e-111">V tomto příkladu `StreetAddress` je typ bez vlastnosti identity.</span><span class="sxs-lookup"><span data-stu-id="a350e-111">In this example, `StreetAddress` is a type with no identity property.</span></span> <span data-ttu-id="a350e-112">Slouží jako vlastnost typu objednávky k určení dodací adresy pro konkrétní objednávku.</span><span class="sxs-lookup"><span data-stu-id="a350e-112">It is used as a property of the Order type to specify the shipping address for a particular order.</span></span>

<span data-ttu-id="a350e-113">V případě, že `OwnedAttribute` je odkazováno z jiného typu entity, můžeme použít k tomu, aby ho považoval jako vlastněné entity:</span><span class="sxs-lookup"><span data-stu-id="a350e-113">We can use the `OwnedAttribute` to treat it as an owned entity when referenced from another entity type:</span></span>

[!code-csharp[StreetAddress](../../../samples/core/Modeling/OwnedEntities/StreetAddress.cs?name=StreetAddress)]

[!code-csharp[Order](../../../samples/core/Modeling/OwnedEntities/Order.cs?name=Order)]

<span data-ttu-id="a350e-114">Je `OwnsOne` také možné použít metodu v `OnModelCreating` k určení toho, že vlastnost je `ShippingAddress` vlastněná entita `Order` typu entity a v případě potřeby konfiguruje další omezující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="a350e-114">It is also possible to use the `OwnsOne` method in `OnModelCreating` to specify that the `ShippingAddress` property is an Owned Entity of the `Order` entity type and to configure additional facets if needed.</span></span>

[!code-csharp[OwnsOne](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOne)]

<span data-ttu-id="a350e-115">Pokud je `Order`vlastnostsoukromá v typu, můžete použít řetězcovou verzi `OwnsOne` metody: `ShippingAddress`</span><span class="sxs-lookup"><span data-stu-id="a350e-115">If the `ShippingAddress` property is private in the `Order` type, you can use the string version of the `OwnsOne` method:</span></span>

[!code-csharp[OwnsOneString](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneString)]

<span data-ttu-id="a350e-116">Podívejte se na [úplný ukázkový projekt](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) pro další kontext.</span><span class="sxs-lookup"><span data-stu-id="a350e-116">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) for more context.</span></span> 

## <a name="implicit-keys"></a><span data-ttu-id="a350e-117">Implicitní klíče</span><span class="sxs-lookup"><span data-stu-id="a350e-117">Implicit keys</span></span>

<span data-ttu-id="a350e-118">Vlastněné typy nakonfigurované `OwnsOne` nebo zjištěné prostřednictvím navigační navigace mají vždy relaci 1:1 s vlastníkem, proto nepotřebují vlastní hodnoty klíče, protože hodnoty cizích klíčů jsou jedinečné.</span><span class="sxs-lookup"><span data-stu-id="a350e-118">Owned types configured with `OwnsOne` or discovered through a reference navigation always have a one-to-one relationship with the owner, therefore they don't need their own key values as the foreign key values are unique.</span></span> <span data-ttu-id="a350e-119">V předchozím příkladu `StreetAddress` typ nepotřebuje definovat klíčovou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="a350e-119">In the previous example, the `StreetAddress` type does not need to define a key property.</span></span>  

<span data-ttu-id="a350e-120">Aby bylo možné pochopit, jak EF Core tyto objekty sledovat, je užitečné vědět, že je primární klíč vytvořen jako [vlastnost Shadow](xref:core/modeling/shadow-properties) pro vlastněný typ.</span><span class="sxs-lookup"><span data-stu-id="a350e-120">In order to understand how EF Core tracks these objects, it is useful to know that a primary key is created as a [shadow property](xref:core/modeling/shadow-properties) for the owned type.</span></span> <span data-ttu-id="a350e-121">Hodnota klíče instance typu, který je vlastníkem, bude stejná jako hodnota klíče instance vlastníka.</span><span class="sxs-lookup"><span data-stu-id="a350e-121">The value of the key of an instance of the owned type will be the same as the value of the key of the owner instance.</span></span>

## <a name="collections-of-owned-types"></a><span data-ttu-id="a350e-122">Kolekce vlastněných typů</span><span class="sxs-lookup"><span data-stu-id="a350e-122">Collections of owned types</span></span>

> [!NOTE]
> <span data-ttu-id="a350e-123">Tato funkce je v EF Core 2,2 novinkou.</span><span class="sxs-lookup"><span data-stu-id="a350e-123">This feature is new in EF Core 2.2.</span></span>

<span data-ttu-id="a350e-124">Chcete-li konfigurovat kolekci vlastněných typů `OwnsMany` , `OnModelCreating`použijte v.</span><span class="sxs-lookup"><span data-stu-id="a350e-124">To configure a collection of owned types use `OwnsMany` in `OnModelCreating`.</span></span>

<span data-ttu-id="a350e-125">Vlastněné typy potřebují primární klíč.</span><span class="sxs-lookup"><span data-stu-id="a350e-125">Owned types need a primary key.</span></span> <span data-ttu-id="a350e-126">Pokud v typu .NET nejsou k dispozici žádné dobré vlastnosti kandidátů, EF Core se může pokusit vytvořit.</span><span class="sxs-lookup"><span data-stu-id="a350e-126">If there are no good candidates properties on the .NET type, EF Core can try to create one.</span></span> <span data-ttu-id="a350e-127">Pokud jsou však vlastní typy definovány prostřednictvím kolekce, není třeba pouze vytvořit vlastnost Shadow, která bude fungovat jako cizí klíč, a primární klíč vlastněné instance, jak to máme: pro `OwnsOne`je možné použít více vlastněných instancí typu pro Každý vlastník, a proto klíč vlastníka nestačí poskytnout jedinečnou identitu pro každou vlastní instanci.</span><span class="sxs-lookup"><span data-stu-id="a350e-127">However, when owned types are defined through a collection, it isn't enough to just create a shadow property to act as both the foreign key into the owner and the primary key of the owned instance, as we do for `OwnsOne`: there can be multiple owned type instances for each owner, and hence the key of the owner isn't enough to provide a unique identity for each owned instance.</span></span>

<span data-ttu-id="a350e-128">Mezi ně patří tyto dvě nejjednodušší řešení:</span><span class="sxs-lookup"><span data-stu-id="a350e-128">The two most straightforward solutions to this are:</span></span>
- <span data-ttu-id="a350e-129">Definování náhradního primárního klíče pro novou vlastnost nezávisle na cizím klíči, který odkazuje na vlastníka.</span><span class="sxs-lookup"><span data-stu-id="a350e-129">Defining a surrogate primary key on a new property independent of the foreign key that points to the owner.</span></span> <span data-ttu-id="a350e-130">Obsažené hodnoty by musely být jedinečné ve všech vlastníkech (např. Pokud nadřazený {1} objekt má {1}podřízenou {2} {1}položku), takže tato hodnota nemá žádný z těchto významů.</span><span class="sxs-lookup"><span data-stu-id="a350e-130">The contained values would need to be unique across all owners (e.g. if Parent {1} has Child {1}, then Parent {2} cannot have Child {1}), so the value doesn't have any inherent meaning.</span></span> <span data-ttu-id="a350e-131">Vzhledem k tomu, že cizí klíč není součástí primárního klíče, lze jeho hodnoty změnit, takže byste mohli přesunout podřízenou položku z jedné nadřazené položky do druhé, ale obvykle to vzroste s sémantikou agregace.</span><span class="sxs-lookup"><span data-stu-id="a350e-131">Since the foreign key is not part of the primary key its values can be changed, so you could move a child from one parent to another one, however this usually goes against aggregate semantics.</span></span>
- <span data-ttu-id="a350e-132">Použití cizího klíče a další vlastnosti jako složeného klíče.</span><span class="sxs-lookup"><span data-stu-id="a350e-132">Using the foreign key and an additional property as a composite key.</span></span> <span data-ttu-id="a350e-133">Hodnota další vlastnosti teď musí být pro {1} danou nadřazenou položku jedinečná (takže pokud nadřazený objekt {1,1} má {2} podřízenou položku, může mít podřízený objekt {2,1}i podřízenou položku).</span><span class="sxs-lookup"><span data-stu-id="a350e-133">The additional property value now only needs to be unique for a given parent (so if Parent {1} has Child {1,1} then Parent {2} can still have Child {2,1}).</span></span> <span data-ttu-id="a350e-134">Vytvořením části cizího klíče v primárním klíči se vztah mezi vlastníkem a vlastněnou entitou stane neproměnlivým a bude lépe odrážet agregovanou sémantiku.</span><span class="sxs-lookup"><span data-stu-id="a350e-134">By making the foreign key part of the primary key the relationship between the owner and the owned entity becomes immutable and reflects aggregate semantics better.</span></span> <span data-ttu-id="a350e-135">To je to, co EF Core ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="a350e-135">This is what EF Core does by default.</span></span>

<span data-ttu-id="a350e-136">V tomto příkladu použijeme `Distributor` třídu:</span><span class="sxs-lookup"><span data-stu-id="a350e-136">In this example we'll use the `Distributor` class:</span></span>

[!code-csharp[Distributor](../../../samples/core/Modeling/OwnedEntities/Distributor.cs?name=Distributor)]

<span data-ttu-id="a350e-137">Ve výchozím nastavení `("DistributorId", "Id")` bude primární klíč použitý pro daný typ, na který `ShippingCenters` odkazuje vlastnost navigace, v `"DistributorId"` případě, kde je `"Id"` hodnota FK a `int` je jedinečná.</span><span class="sxs-lookup"><span data-stu-id="a350e-137">By default the primary key used for the owned type referenced through the `ShippingCenters` navigation property will be `("DistributorId", "Id")` where `"DistributorId"` is the FK and `"Id"` is a unique `int` value.</span></span>

<span data-ttu-id="a350e-138">Konfigurace jiného volání `HasKey`PK:</span><span class="sxs-lookup"><span data-stu-id="a350e-138">To configure a different PK call `HasKey`:</span></span>

[!code-csharp[OwnsMany](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsMany)]

> [!NOTE]
> <span data-ttu-id="a350e-139">Předtím, než `WithOwner()` metoda EF Core 3,0 neexistuje, by mělo být toto volání odebráno.</span><span class="sxs-lookup"><span data-stu-id="a350e-139">Before EF Core 3.0 `WithOwner()` method didn't exist so this call should be removed.</span></span>

## <a name="mapping-owned-types-with-table-splitting"></a><span data-ttu-id="a350e-140">Mapování vlastněných typů s dělením tabulky</span><span class="sxs-lookup"><span data-stu-id="a350e-140">Mapping owned types with table splitting</span></span>

<span data-ttu-id="a350e-141">Při použití relačních databází jsou ve výchozím nastavení vlastní typy odkazů namapovány na stejnou tabulku jako vlastník.</span><span class="sxs-lookup"><span data-stu-id="a350e-141">When using relational databases, by default reference owned types are mapped to the same table as the owner.</span></span> <span data-ttu-id="a350e-142">To vyžaduje rozdělení tabulky ve dvou případech: některé sloupce budou použity k uložení dat vlastníka a některé sloupce budou použity k uložení dat vlastněné entity.</span><span class="sxs-lookup"><span data-stu-id="a350e-142">This requires splitting the table in two: some columns will be used to store the data of the owner, and some columns will be used to store data of the owned entity.</span></span> <span data-ttu-id="a350e-143">Jedná se o běžnou funkci známou jako [rozdělení tabulky](table-splitting.md).</span><span class="sxs-lookup"><span data-stu-id="a350e-143">This is a common feature known as [table splitting](table-splitting.md).</span></span>

<span data-ttu-id="a350e-144">Ve výchozím nastavení EF Core pojmenuje sloupce databáze pro vlastnosti typu entity, které patří do vzoru _Navigation_OwnedEntityProperty_.</span><span class="sxs-lookup"><span data-stu-id="a350e-144">By default, EF Core will name the database columns for the properties of the owned entity type following the pattern _Navigation_OwnedEntityProperty_.</span></span> <span data-ttu-id="a350e-145">Proto se `StreetAddress` vlastnosti zobrazí v tabulce Orders s názvy ' ShippingAddress_Street ' a ' ShippingAddress_City '.</span><span class="sxs-lookup"><span data-stu-id="a350e-145">Therefore the `StreetAddress` properties will appear in the 'Orders' table with the names 'ShippingAddress_Street' and 'ShippingAddress_City'.</span></span>

<span data-ttu-id="a350e-146">K přejmenování těchto sloupců `HasColumnName` lze použít metodu:</span><span class="sxs-lookup"><span data-stu-id="a350e-146">You can use the `HasColumnName` method to rename those columns:</span></span>

[!code-csharp[ColumnNames](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=ColumnNames)]

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a><span data-ttu-id="a350e-147">Sdílení stejného typu .NET mezi více vlastněných typů</span><span class="sxs-lookup"><span data-stu-id="a350e-147">Sharing the same .NET type among multiple owned types</span></span>

<span data-ttu-id="a350e-148">Vlastněný typ entity může být stejného typu .NET jako jiný vlastněný typ entity, proto typ .NET nemusí být dostatečně k identifikaci vlastněné typu.</span><span class="sxs-lookup"><span data-stu-id="a350e-148">An owned entity type can be of the same .NET type as another owned entity type, therefore the .NET type may not be enough to identify an owned type.</span></span>

<span data-ttu-id="a350e-149">V těchto případech se vlastnost ukazující z vlastníka na vlastní entitu staly _definováním navigace_ typu vlastněné entity.</span><span class="sxs-lookup"><span data-stu-id="a350e-149">In those cases, the property pointing from the owner to the owned entity becomes the _defining navigation_ of the owned entity type.</span></span> <span data-ttu-id="a350e-150">Z perspektivy EF Core je definování navigace součástí identity typu spolu s typem .NET.</span><span class="sxs-lookup"><span data-stu-id="a350e-150">From the perspective of EF Core, the defining navigation is part of the type's identity alongside the .NET type.</span></span>   

<span data-ttu-id="a350e-151">Například v následující třídě `ShippingAddress` a `BillingAddress` jsou oba stejného typu `StreetAddress`.NET:</span><span class="sxs-lookup"><span data-stu-id="a350e-151">For example, in the following class `ShippingAddress` and `BillingAddress` are both of the same .NET type, `StreetAddress`:</span></span>

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

<span data-ttu-id="a350e-152">Aby bylo možné pochopit, jak EF Core odlišuje sledované instance těchto objektů, může být užitečné vzít v úvahu, že definující navigace se stane součástí klíče instance spolu s hodnotou klíče vlastníka a typu .NET vlastněných typů.</span><span class="sxs-lookup"><span data-stu-id="a350e-152">In order to understand how EF Core will distinguish tracked instances of these objects, it may be useful to think that the defining navigation has become part of the key of the instance alongside the value of the key of the owner and the .NET type of the owned type.</span></span>

## <a name="nested-owned-types"></a><span data-ttu-id="a350e-153">Vnořené vlastní typy</span><span class="sxs-lookup"><span data-stu-id="a350e-153">Nested owned types</span></span>

<span data-ttu-id="a350e-154">V tomto příkladu `OrderDetails` vlastní `BillingAddress` a `ShippingAddress`, které jsou oba `StreetAddress` typy.</span><span class="sxs-lookup"><span data-stu-id="a350e-154">In this example `OrderDetails` owns `BillingAddress` and `ShippingAddress`, which are both `StreetAddress` types.</span></span> <span data-ttu-id="a350e-155">Pak `OrderDetails` vlastní`DetailedOrder` typ.</span><span class="sxs-lookup"><span data-stu-id="a350e-155">Then `OrderDetails` is owned by the `DetailedOrder` type.</span></span>

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/OwnedEntities/DetailedOrder.cs?name=DetailedOrder)]

[!code-csharp[OrderStatus](../../../samples/core/Modeling/OwnedEntities/OrderStatus.cs?name=OrderStatus)]

<span data-ttu-id="a350e-156">Kromě vnořených vlastněných typů může vlastněný typ odkazovat na běžnou entitu, může to být buď vlastník, nebo jiná entita, pokud je vlastněná entita na závislé straně.</span><span class="sxs-lookup"><span data-stu-id="a350e-156">In addition to nested owned types, an owned type can reference a regular entity, it can be either the owner or a different entity as long as the owned entity is on the dependent side.</span></span> <span data-ttu-id="a350e-157">Tato schopnost nastavuje vlastní typy entit od složitých typů v EF6.</span><span class="sxs-lookup"><span data-stu-id="a350e-157">This capability sets owned entity types apart from complex types in EF6.</span></span>

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

<span data-ttu-id="a350e-158">Je možné zřetězit `OwnsOne` metodu v rámci volání Fluent pro konfiguraci tohoto modelu:</span><span class="sxs-lookup"><span data-stu-id="a350e-158">It is possible to chain the `OwnsOne` method in a fluent call to configure this model:</span></span>

[!code-csharp[OwnsOneNested](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneNested)]

<span data-ttu-id="a350e-159">Všimněte si `WithOwner` volání, které se používá ke konfiguraci navigační vlastnosti odkazující zpátky na vlastníka.</span><span class="sxs-lookup"><span data-stu-id="a350e-159">Notice the `WithOwner` call used to configure the navigation property pointing back at the owner.</span></span>

<span data-ttu-id="a350e-160">Výsledek lze dosáhnout pomocí `OwnedAttribute` obou `OrderDetails` i `StreetAdress`.</span><span class="sxs-lookup"><span data-stu-id="a350e-160">It is possible to achieve the result using `OwnedAttribute` on both `OrderDetails` and `StreetAdress`.</span></span>

## <a name="storing-owned-types-in-separate-tables"></a><span data-ttu-id="a350e-161">Ukládání vlastněných typů do samostatných tabulek</span><span class="sxs-lookup"><span data-stu-id="a350e-161">Storing owned types in separate tables</span></span>

<span data-ttu-id="a350e-162">I na rozdíl od EF6 složitých typů mohou být vlastněné typy uloženy v samostatné tabulce od vlastníka.</span><span class="sxs-lookup"><span data-stu-id="a350e-162">Also unlike EF6 complex types, owned types can be stored in a separate table from the owner.</span></span> <span data-ttu-id="a350e-163">Aby bylo možné přepsat konvenci, která mapuje vlastní typ na stejnou tabulku jako vlastník, můžete jednoduše zavolat `ToTable` a zadat jiný název tabulky.</span><span class="sxs-lookup"><span data-stu-id="a350e-163">In order to override the convention that maps an owned type to the same table as the owner, you can simply call `ToTable` and provide a different table name.</span></span> <span data-ttu-id="a350e-164">Následující příklad bude mapovat `OrderDetails` a jeho dvě adresy na samostatnou tabulku z: `DetailedOrder`</span><span class="sxs-lookup"><span data-stu-id="a350e-164">The following example will map `OrderDetails` and its two addresses to a separate table from `DetailedOrder`:</span></span>

[!code-csharp[OwnsOneTable](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneTable)]

## <a name="querying-owned-types"></a><span data-ttu-id="a350e-165">Dotazování na vlastní typy</span><span class="sxs-lookup"><span data-stu-id="a350e-165">Querying owned types</span></span>

<span data-ttu-id="a350e-166">Při dotazování vlastníka budou ve výchozím nastavení zahrnuty vlastněné typy.</span><span class="sxs-lookup"><span data-stu-id="a350e-166">When querying the owner the owned types will be included by default.</span></span> <span data-ttu-id="a350e-167">Není nutné používat `Include` metodu, a to ani v případě, že vlastní typy jsou uloženy v samostatné tabulce.</span><span class="sxs-lookup"><span data-stu-id="a350e-167">It is not necessary to use the `Include` method, even if the owned types are stored in a separate table.</span></span> <span data-ttu-id="a350e-168">V závislosti na modelu popsaném dříve bude následující dotaz získávat `Order` `OrderDetails` a dvě vlastněná `StreetAddresses` z databáze:</span><span class="sxs-lookup"><span data-stu-id="a350e-168">Based on the model described before, the following query will get `Order`, `OrderDetails` and the two owned `StreetAddresses` from the database:</span></span>

[!code-csharp[DetailedOrderQuery](../../../samples/core/Modeling/OwnedEntities/Program.cs?name=DetailedOrderQuery)]

## <a name="limitations"></a><span data-ttu-id="a350e-169">Omezení</span><span class="sxs-lookup"><span data-stu-id="a350e-169">Limitations</span></span>

<span data-ttu-id="a350e-170">Některá z těchto omezení jsou zásadní pro to, jak vlastní typy entit fungují, ale některé jiné jsou omezení, která v budoucích verzích může být možné odebrat:</span><span class="sxs-lookup"><span data-stu-id="a350e-170">Some of these limitations are fundamental to how owned entity types work, but some others are restrictions that we may be able to remove in future releases:</span></span>

### <a name="by-design-restrictions"></a><span data-ttu-id="a350e-171">Omezení podle návrhu</span><span class="sxs-lookup"><span data-stu-id="a350e-171">By-design restrictions</span></span>
- <span data-ttu-id="a350e-172">Nemůžete vytvořit `DbSet<T>` pro vlastněný typ.</span><span class="sxs-lookup"><span data-stu-id="a350e-172">You cannot create a `DbSet<T>` for an owned type</span></span>
- <span data-ttu-id="a350e-173">Nelze volat `Entity<T>()` s vlastníkem typu na`ModelBuilder`</span><span class="sxs-lookup"><span data-stu-id="a350e-173">You cannot call `Entity<T>()` with an owned type on `ModelBuilder`</span></span>

### <a name="current-shortcomings"></a><span data-ttu-id="a350e-174">Aktuální nedostatky</span><span class="sxs-lookup"><span data-stu-id="a350e-174">Current shortcomings</span></span>
- <span data-ttu-id="a350e-175">Hierarchie dědičnosti, které zahrnují vlastní typy entit, se nepodporují.</span><span class="sxs-lookup"><span data-stu-id="a350e-175">Inheritance hierarchies that include owned entity types are not supported</span></span>
- <span data-ttu-id="a350e-176">Referenční navigace na vlastní typy entit nemůžou mít hodnotu null, pokud nejsou explicitně namapované na samostatnou tabulku od vlastníka.</span><span class="sxs-lookup"><span data-stu-id="a350e-176">Reference navigations to owned entity types cannot be null unless they are explicitly mapped to a separate table from the owner</span></span>
- <span data-ttu-id="a350e-177">Instance typů vlastněných entit nemůže sdílet více vlastníků (Toto je známý scénář pro objekty hodnot, které není možné implementovat pomocí vlastněných typů entit).</span><span class="sxs-lookup"><span data-stu-id="a350e-177">Instances of owned entity types cannot be shared by multiple owners (this is a well-known scenario for value objects that cannot be implemented using owned entity types)</span></span>

### <a name="shortcomings-in-previous-versions"></a><span data-ttu-id="a350e-178">Nedostatky v předchozích verzích</span><span class="sxs-lookup"><span data-stu-id="a350e-178">Shortcomings in previous versions</span></span>
- <span data-ttu-id="a350e-179">V EF Core 2,0 nemůže být navigace na vlastní typy entit deklarována v odvozených typech entit, pokud vlastněné entity nejsou explicitně mapovány na samostatnou tabulku z hierarchie Owner.</span><span class="sxs-lookup"><span data-stu-id="a350e-179">In EF Core 2.0, navigations to owned entity types cannot be declared in derived entity types unless the owned entities are explicitly mapped to a separate table from the owner hierarchy.</span></span> <span data-ttu-id="a350e-180">Toto omezení se odebralo v EF Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="a350e-180">This limitation has been removed in EF Core 2.1</span></span>
- <span data-ttu-id="a350e-181">V EF Core 2,0 a 2,1 jsou podporovány pouze navigační odkazy na vlastní typy.</span><span class="sxs-lookup"><span data-stu-id="a350e-181">In EF Core 2.0 and 2.1 only reference navigations to owned types were supported.</span></span> <span data-ttu-id="a350e-182">Toto omezení se odebralo v EF Core 2,2.</span><span class="sxs-lookup"><span data-stu-id="a350e-182">This limitation has been removed in EF Core 2.2</span></span>
