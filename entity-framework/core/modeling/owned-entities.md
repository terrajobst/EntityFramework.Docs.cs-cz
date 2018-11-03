---
title: Vlastněné typy entit – EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 02/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
uid: core/modeling/owned-entities
ms.openlocfilehash: 58da3b6b951b3fa4aa04ec75f5759555c1f0cde5
ms.sourcegitcommit: 39080d38e1adea90db741257e60dc0e7ed08aa82
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/03/2018
ms.locfileid: "50980025"
---
# <a name="owned-entity-types"></a><span data-ttu-id="53869-102">Vlastněné typy entit</span><span class="sxs-lookup"><span data-stu-id="53869-102">Owned Entity Types</span></span>

>[!NOTE]
> <span data-ttu-id="53869-103">Tato funkce je v EF Core 2.0 nová.</span><span class="sxs-lookup"><span data-stu-id="53869-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="53869-104">EF Core umožňuje modelu typy entit, které může vyskytovat vždy jen u vlastnosti navigace z ostatních typů entit.</span><span class="sxs-lookup"><span data-stu-id="53869-104">EF Core allows you to model entity types that can only ever appear on navigation properties of other entity types.</span></span> <span data-ttu-id="53869-105">Toto nastavení se nazývá _vlastněné typy entit_.</span><span class="sxs-lookup"><span data-stu-id="53869-105">These are called _owned entity types_.</span></span> <span data-ttu-id="53869-106">Entita obsahující typ vlastnictví entity je jeho _vlastníka_.</span><span class="sxs-lookup"><span data-stu-id="53869-106">The entity containing an owned entity type is its _owner_.</span></span>

## <a name="explicit-configuration"></a><span data-ttu-id="53869-107">Explicitní konfigurace</span><span class="sxs-lookup"><span data-stu-id="53869-107">Explicit configuration</span></span>

<span data-ttu-id="53869-108">Vlastní entity, které typy jsou nikdy součástí pomocí EF Core modelu konvencí.</span><span class="sxs-lookup"><span data-stu-id="53869-108">Owned entity types are never included by EF Core in the model by convention.</span></span> <span data-ttu-id="53869-109">Můžete použít `OwnsOne` metoda ve `OnModelCreating` nebo typ s poznámkami `OwnedAttribute` (novinka v EF Core 2.1) ke konfiguraci typu jako typ vlastnictví.</span><span class="sxs-lookup"><span data-stu-id="53869-109">You can use the `OwnsOne` method in `OnModelCreating` or annotate the type with `OwnedAttribute` (new in EF Core 2.1) to configure the type as an owned type.</span></span>

<span data-ttu-id="53869-110">V tomto příkladu `StreetAddress` je typ s žádnou vlastnost identity.</span><span class="sxs-lookup"><span data-stu-id="53869-110">In this example, `StreetAddress` is a type with no identity property.</span></span> <span data-ttu-id="53869-111">Jako vlastnost typu pořadí slouží k určení dodací adresu pro konkrétní objednávku.</span><span class="sxs-lookup"><span data-stu-id="53869-111">It is used as a property of the Order type to specify the shipping address for a particular order.</span></span>

<span data-ttu-id="53869-112">Můžeme použít `OwnedAttribute` považovat za vlastnictví entity při odkazování z jiného typu entity:</span><span class="sxs-lookup"><span data-stu-id="53869-112">We can use the `OwnedAttribute` to treat it as an owned entity when referenced from another entity type:</span></span>

[!code-csharp[StreetAddress](../../../samples/core/Modeling/OwnedEntities/StreetAddress.cs?name=StreetAddress)]

[!code-csharp[Order](../../../samples/core/Modeling/OwnedEntities/Order.cs?name=Order)]

<span data-ttu-id="53869-113">Je také možné použít `OwnsOne` metoda ve `OnModelCreating` určit, že `ShippingAddress` vlastností je vlastní Entity `Order` typu entity a v případě potřeby nakonfigurujte další charakteristiky.</span><span class="sxs-lookup"><span data-stu-id="53869-113">It is also possible to use the `OwnsOne` method in `OnModelCreating` to specify that the `ShippingAddress` property is an Owned Entity of the `Order` entity type and to configure additional facets if needed.</span></span>

[!code-csharp[OwnsOne](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOne)]

<span data-ttu-id="53869-114">Pokud `ShippingAddress` vlastnost je v privátní `Order` typ, můžete použít verze řetězce `OwnsOne` metody:</span><span class="sxs-lookup"><span data-stu-id="53869-114">If the `ShippingAddress` property is private in the `Order` type, you can use the string version of the `OwnsOne` method:</span></span>

[!code-csharp[OwnsOneString](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneString)]

<span data-ttu-id="53869-115">Zobrazit [úplný ukázkový projekt](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) pro širší kontext.</span><span class="sxs-lookup"><span data-stu-id="53869-115">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) for more context.</span></span> 

## <a name="implicit-keys"></a><span data-ttu-id="53869-116">Implicitní klíče</span><span class="sxs-lookup"><span data-stu-id="53869-116">Implicit keys</span></span>

<span data-ttu-id="53869-117">Vlastněné typy nakonfigurovanou `OwnsOne` nebo zjištěný prostřednictvím navigační odkaz vždy mít relaci s vlastníkem, proto nepotřebují vlastní hodnoty klíče, jako jsou jedinečné hodnoty cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="53869-117">Owned types configured with `OwnsOne` or discovered through a reference navigation always have a one-to-one relationship with the owner, therefore they don't need their own key values as the foreign key values are unique.</span></span> <span data-ttu-id="53869-118">V předchozím příkladu `StreetAddress` typ není nutné definovat vlastnost klíče.</span><span class="sxs-lookup"><span data-stu-id="53869-118">In the previous example, the `StreetAddress` type does not need to define a key property.</span></span>  

<span data-ttu-id="53869-119">Chcete-li pochopit, jak EF Core sleduje tyto objekty, je vhodné popřemýšlet, primární klíč je vytvořen jako [stínové vlastnosti](xref:core/modeling/shadow-properties) pro typ vlastnictví.</span><span class="sxs-lookup"><span data-stu-id="53869-119">In order to understand how EF Core tracks these objects, it is useful to think that a primary key is created as a [shadow property](xref:core/modeling/shadow-properties) for the owned type.</span></span> <span data-ttu-id="53869-120">Hodnota klíče instance typu vlastnictví budou stejné jako hodnotu klíče vlastníka instance.</span><span class="sxs-lookup"><span data-stu-id="53869-120">The value of the key of an instance of the owned type will be the same as the value of the key of the owner instance.</span></span>

## <a name="collections-of-owned-types"></a><span data-ttu-id="53869-121">Vlastněné typy kolekcí</span><span class="sxs-lookup"><span data-stu-id="53869-121">Collections of owned types</span></span>

>[!NOTE]
> <span data-ttu-id="53869-122">Tato funkce je nového v EF Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="53869-122">This feature is new in EF Core 2.2.</span></span>

<span data-ttu-id="53869-123">Ke konfiguraci kolekce vlastněné typy `OwnsMany` byste měli použít ve `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="53869-123">To configure a collection of owned types `OwnsMany` should be used in `OnModelCreating`.</span></span> <span data-ttu-id="53869-124">Ale primární klíč nenakonfigurují podle konvence, takže je potřeba explicitně zadat.</span><span class="sxs-lookup"><span data-stu-id="53869-124">However the primary key will not be configured by convention, so it need to be specified explicitly.</span></span> <span data-ttu-id="53869-125">Je běžné použití komplexní klíče u těchto typů entit začlenění cizí klíč pro vlastníka a dalších jedinečnou vlastnost, která může také být ve stavu stínové:</span><span class="sxs-lookup"><span data-stu-id="53869-125">It is common to use a complex key for these type of entities incorporating the foreign key to the owner and an additional unique property that can also be in shadow state:</span></span>

[!code-csharp[OwnsMany](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsMany)]

## <a name="mapping-owned-types-with-table-splitting"></a><span data-ttu-id="53869-126">Mapování vlastní typy s rozdělení tabulky</span><span class="sxs-lookup"><span data-stu-id="53869-126">Mapping owned types with table splitting</span></span>

<span data-ttu-id="53869-127">Při používání relační databáze, podle úmluvy odkazu, který vlastní typy jsou namapovány na stejnou tabulku jako vlastník.</span><span class="sxs-lookup"><span data-stu-id="53869-127">When using relational databases, by convention reference owned types are mapped to the same table as the owner.</span></span> <span data-ttu-id="53869-128">To vyžaduje, aby rozdělení tabulky ve dvou: některé sloupce se použije k uložení dat vlastníka a některé sloupce se použije k ukládání dat vlastní entity.</span><span class="sxs-lookup"><span data-stu-id="53869-128">This requires splitting the table in two: some columns will be used to store the data of the owner, and some columns will be used to store data of the owned entity.</span></span> <span data-ttu-id="53869-129">Toto je běžné funkce označuje jako rozdělení tabulky.</span><span class="sxs-lookup"><span data-stu-id="53869-129">This is a common feature known as table splitting.</span></span>

> [!TIP]
> <span data-ttu-id="53869-130">Vlastněné typy uloženého s rozdělení tabulky je možné použít jak komplexní typy, které se používají v EF6 podobně.</span><span class="sxs-lookup"><span data-stu-id="53869-130">Owned types stored with table splitting can be used similarly to how complex types are used in EF6.</span></span>

<span data-ttu-id="53869-131">Podle konvence EF Core bude název sloupce databáze pro vlastnosti typu vlastnictví entity podle vzoru _Navigation_OwnedEntityProperty_.</span><span class="sxs-lookup"><span data-stu-id="53869-131">By convention, EF Core will name the database columns for the properties of the owned entity type following the pattern _Navigation_OwnedEntityProperty_.</span></span> <span data-ttu-id="53869-132">Proto `StreetAddress` vlastnosti se zobrazí v tabulce "Orders" s názvy "ShippingAddress_Street" a "ShippingAddress_City".</span><span class="sxs-lookup"><span data-stu-id="53869-132">Therefore the `StreetAddress` properties will appear in the 'Orders' table with the names 'ShippingAddress_Street' and 'ShippingAddress_City'.</span></span>

<span data-ttu-id="53869-133">Můžete připojit `HasColumnName` metoda přejmenovat tyto sloupce:</span><span class="sxs-lookup"><span data-stu-id="53869-133">You can append the `HasColumnName` method to rename those columns:</span></span>

[!code-csharp[ColumnNames](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=ColumnNames)]

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a><span data-ttu-id="53869-134">Sdílení stejného typu .NET mezi několika typy vlastnictví</span><span class="sxs-lookup"><span data-stu-id="53869-134">Sharing the same .NET type among multiple owned types</span></span>

<span data-ttu-id="53869-135">Typ vlastnictví entity může být stejného typu .NET jako jiný typ vlastnictví entity, proto, že typ formátu .NET nemusí být dost informací k identifikaci typu vlastnictví.</span><span class="sxs-lookup"><span data-stu-id="53869-135">An owned entity type can be of the same .NET type as another owned entity type, therefore the .NET type may not be enough to identify an owned type.</span></span>

<span data-ttu-id="53869-136">V těchto případech se vlastnost k vlastní entitě odkazující od vlastníka se stane _definování navigace_ typu vlastnictví entity.</span><span class="sxs-lookup"><span data-stu-id="53869-136">In those cases, the property pointing from the owner to the owned entity becomes the _defining navigation_ of the owned entity type.</span></span> <span data-ttu-id="53869-137">Z pohledu EF Core definující navigace je součástí identity typu vedle typ formátu .NET.</span><span class="sxs-lookup"><span data-stu-id="53869-137">From the perspective of EF Core, the defining navigation is part of the type's identity alongside the .NET type.</span></span>   

<span data-ttu-id="53869-138">Například ve třídě následující `ShippingAddress` a `BillingAddress` jsou stejného typu .NET `StreetAddress`:</span><span class="sxs-lookup"><span data-stu-id="53869-138">For example, in the following class `ShippingAddress` and `BillingAddress` are both of the same .NET type, `StreetAddress`:</span></span>

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

<span data-ttu-id="53869-139">Chcete-li pochopit, jak bude EF Core rozlišovat sledované instancí těchto objektů, může být vhodné popřemýšlet, že se definující navigace stala součástí klíče instance u hodnoty klíče vlastníka a typ .NET vlastnictví.</span><span class="sxs-lookup"><span data-stu-id="53869-139">In order to understand how EF Core will distinguish tracked instances of these objects, it may be useful to think that the defining navigation has become part of the key of the instance alongside the value of the key of the owner and the .NET type of the owned type.</span></span>

## <a name="nested-owned-types"></a><span data-ttu-id="53869-140">Vnořené typy vlastnictví</span><span class="sxs-lookup"><span data-stu-id="53869-140">Nested owned types</span></span>

<span data-ttu-id="53869-141">V tomto příkladu `OrderDetails` vlastní `BillingAddress` a `ShippingAddress`, které jsou obě `StreetAddress` typy.</span><span class="sxs-lookup"><span data-stu-id="53869-141">In this example `OrderDetails` owns `BillingAddress` and `ShippingAddress`, which are both `StreetAddress` types.</span></span> <span data-ttu-id="53869-142">Potom `OrderDetails` není ve vlastnictví `DetailedOrder` typu.</span><span class="sxs-lookup"><span data-stu-id="53869-142">Then `OrderDetails` is owned by the `DetailedOrder` type.</span></span>

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/OwnedEntities/DetailedOrder.cs?name=DetailedOrder)]

[!code-csharp[OrderStatus](../../../samples/core/Modeling/OwnedEntities/OrderStatus.cs?name=OrderStatus)]

<span data-ttu-id="53869-143">Kromě vnořené typy vlastnictví vlastní typ, který může odkazovat regulární entity, vlastní entity je na závislé straně může být vlastníkem nebo jiné entity.</span><span class="sxs-lookup"><span data-stu-id="53869-143">In addition to nested owned types, an owned type can reference a regular entity, it can be either the owner or a different entity as long as the owned entity is on the dependent side.</span></span> <span data-ttu-id="53869-144">Tato možnost nastaví v EF6 vlastněné typy entit kromě komplexní typy.</span><span class="sxs-lookup"><span data-stu-id="53869-144">This capability sets owned entity types apart from complex types in EF6.</span></span>

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

<span data-ttu-id="53869-145">Je možné řetězec `OwnsOne` metoda fluent volání ke konfiguraci tohoto modelu:</span><span class="sxs-lookup"><span data-stu-id="53869-145">It is possible to chain the `OwnsOne` method in a fluent call to configure this model:</span></span>

[!code-csharp[OwnsOneNested](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneNested)]

<span data-ttu-id="53869-146">Je také možné dosáhnout stejnou věc, kterou pomocí `OwnedAttribute` u obou `OrderDetails` a `StreetAdress`.</span><span class="sxs-lookup"><span data-stu-id="53869-146">It is also possible to achieve the same thing using `OwnedAttribute` on both `OrderDetails` and `StreetAdress`.</span></span>

## <a name="storing-owned-types-in-separate-tables"></a><span data-ttu-id="53869-147">Ukládání vlastní typy v samostatných tabulkách</span><span class="sxs-lookup"><span data-stu-id="53869-147">Storing owned types in separate tables</span></span>

<span data-ttu-id="53869-148">Na rozdíl od komplexní typy EF6, také mohou být vlastněné typy uloženy do samostatné tabulky od vlastníka.</span><span class="sxs-lookup"><span data-stu-id="53869-148">Also unlike EF6 complex types, owned types can be stored in a separate table from the owner.</span></span> <span data-ttu-id="53869-149">Aby bylo možné přepsat vytváření názvů, který mapuje typ vlastnictví na stejnou tabulku jako vlastník, můžete jednoduše zavoláte `ToTable` a zadejte jiný název tabulky.</span><span class="sxs-lookup"><span data-stu-id="53869-149">In order to override the convention that maps an owned type to the same table as the owner, you can simply call `ToTable` and provide a different table name.</span></span> <span data-ttu-id="53869-150">Následující příklad provede mapování `OrderDetails` a jeho dvou adres do samostatné tabulky z `DetailedOrder`:</span><span class="sxs-lookup"><span data-stu-id="53869-150">The following example will map `OrderDetails` and its two addresses to a separate table from `DetailedOrder`:</span></span>

[!code-csharp[OwnsOneTable](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneTable)]

## <a name="querying-owned-types"></a><span data-ttu-id="53869-151">Zjišťují se vlastněné typy</span><span class="sxs-lookup"><span data-stu-id="53869-151">Querying owned types</span></span>

<span data-ttu-id="53869-152">Při dotazování na vlastníka vlastněné typy budou zahrnuty ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="53869-152">When querying the owner the owned types will be included by default.</span></span> <span data-ttu-id="53869-153">Není nutné používat `Include` metoda, i když vlastněné typy jsou uložené v samostatné tabulce.</span><span class="sxs-lookup"><span data-stu-id="53869-153">It is not necessary to use the `Include` method, even if the owned types are stored in a separate table.</span></span> <span data-ttu-id="53869-154">Na základě modelu před popsané, následující dotaz zobrazí `Order`, `OrderDetails` a dva vlastní `StreetAddresses` z databáze:</span><span class="sxs-lookup"><span data-stu-id="53869-154">Based on the model described before, the following query will get `Order`, `OrderDetails` and the two owned `StreetAddresses` from the database:</span></span>

[!code-csharp[DetailedOrderQuery](../../../samples/core/Modeling/OwnedEntities/Program.cs?name=DetailedOrderQuery)]

## <a name="limitations"></a><span data-ttu-id="53869-155">Omezení</span><span class="sxs-lookup"><span data-stu-id="53869-155">Limitations</span></span>

<span data-ttu-id="53869-156">Některé z těchto omezení jsou základem pro jak vlastněné pracovní typy entit, ale jiná omezení, že můžeme být schopen odebrat v budoucích verzích jsou:</span><span class="sxs-lookup"><span data-stu-id="53869-156">Some of these limitations are fundamental to how owned entity types work, but some others are restrictions that we may be able to remove in future releases:</span></span>

### <a name="by-design-restrictions"></a><span data-ttu-id="53869-157">Omezení podle návrhu</span><span class="sxs-lookup"><span data-stu-id="53869-157">By-design restrictions</span></span>
- <span data-ttu-id="53869-158">Nelze vytvořit `DbSet<T>` pro typ vlastnictví</span><span class="sxs-lookup"><span data-stu-id="53869-158">You cannot create a `DbSet<T>` for an owned type</span></span>
- <span data-ttu-id="53869-159">Nejde volat `Entity<T>()` s typem vlastnictví na `ModelBuilder`</span><span class="sxs-lookup"><span data-stu-id="53869-159">You cannot call `Entity<T>()` with an owned type on `ModelBuilder`</span></span>

### <a name="current-shortcomings"></a><span data-ttu-id="53869-160">Aktuální nedostatky</span><span class="sxs-lookup"><span data-stu-id="53869-160">Current shortcomings</span></span>
- <span data-ttu-id="53869-161">Hierarchie dědičnosti, které zahrnují vlastněné typy entit nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="53869-161">Inheritance hierarchies that include owned entity types are not supported</span></span>
- <span data-ttu-id="53869-162">Navigační odkaz pro vlastní typy entit nemůže mít hodnotu null, pokud jsou explicitně namapované na samostatnou tabulku od vlastníka</span><span class="sxs-lookup"><span data-stu-id="53869-162">Reference navigations to owned entity types cannot be null unless they are explicitly mapped to a separate table from the owner</span></span>
- <span data-ttu-id="53869-163">Instance vlastněné typy entit nemůže je sdílet více vlastníky (to je dobře známé scénář pro hodnotu objekty, které nelze implementovat s využitím vlastněné typy entit)</span><span class="sxs-lookup"><span data-stu-id="53869-163">Instances of owned entity types cannot be shared by multiple owners (this is a well-known scenario for value objects that cannot be implemented using owned entity types)</span></span>

### <a name="shortcomings-in-previous-versions"></a><span data-ttu-id="53869-164">Nedostatky v předchozích verzích</span><span class="sxs-lookup"><span data-stu-id="53869-164">Shortcomings in previous versions</span></span>
- <span data-ttu-id="53869-165">V EF Core 2.0 navigaci na, který vlastní typy entit se nedá deklarovat v typy odvozené entit, pokud vlastnictví entity jsou explicitně namapovány na samostatnou tabulku z hierarchie vlastníka.</span><span class="sxs-lookup"><span data-stu-id="53869-165">In EF Core 2.0, navigations to owned entity types cannot be declared in derived entity types unless the owned entities are explicitly mapped to a separate table from the owner hierarchy.</span></span> <span data-ttu-id="53869-166">Toto omezení byl odebrán v EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="53869-166">This limitation has been removed in EF Core 2.1</span></span>
- <span data-ttu-id="53869-167">En EF Core 2.0 a 2.1 navigaci na jediný odkaz na vlastní typy byly podporovány.</span><span class="sxs-lookup"><span data-stu-id="53869-167">En EF Core 2.0 and 2.1 only reference navigations to owned types were supported.</span></span> <span data-ttu-id="53869-168">Toto omezení byl odebrán v EF Core 2.2</span><span class="sxs-lookup"><span data-stu-id="53869-168">This limitation has been removed in EF Core 2.2</span></span>