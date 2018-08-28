---
title: Vlastněné typy entit – EF Core
author: julielerman
ms.date: 2/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
uid: core/modeling/owned-entities
ms.openlocfilehash: afbc853feab56d31a8ceba0da05fe6dc508b65bb
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994066"
---
# <a name="owned-entity-types"></a><span data-ttu-id="0c570-102">Vlastněné typy entit</span><span class="sxs-lookup"><span data-stu-id="0c570-102">Owned Entity Types</span></span>

>[!NOTE]
> <span data-ttu-id="0c570-103">Tato funkce je v EF Core 2.0 nová.</span><span class="sxs-lookup"><span data-stu-id="0c570-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="0c570-104">EF Core umožňuje modelu typy entit, které může vyskytovat vždy jen u vlastnosti navigace z ostatních typů entit.</span><span class="sxs-lookup"><span data-stu-id="0c570-104">EF Core allows you to model entity types that can only ever appear on navigation properties of other entity types.</span></span> <span data-ttu-id="0c570-105">Toto nastavení se nazývá _vlastněné typy entit_.</span><span class="sxs-lookup"><span data-stu-id="0c570-105">These are called _owned entity types_.</span></span> <span data-ttu-id="0c570-106">Entita obsahující typ vlastnictví entity je jeho _vlastníka_.</span><span class="sxs-lookup"><span data-stu-id="0c570-106">The entity containing an owned entity type is its _owner_.</span></span>

## <a name="explicit-configuration"></a><span data-ttu-id="0c570-107">Explicitní konfigurace</span><span class="sxs-lookup"><span data-stu-id="0c570-107">Explicit configuration</span></span>

<span data-ttu-id="0c570-108">Vlastní entity, které typy jsou nikdy součástí pomocí EF Core modelu konvencí.</span><span class="sxs-lookup"><span data-stu-id="0c570-108">Owned entity types are never included by EF Core in the model by convention.</span></span> <span data-ttu-id="0c570-109">Můžete použít `OwnsOne` metoda ve `OnModelCreating` nebo typ s poznámkami `OwnedAttribute` (novinka v EF Core 2.1) ke konfiguraci typu jako typ vlastnictví.</span><span class="sxs-lookup"><span data-stu-id="0c570-109">You can use the `OwnsOne` method in `OnModelCreating` or annotate the type with `OwnedAttribute` (new in EF Core 2.1) to configure the type as an owned type.</span></span>

<span data-ttu-id="0c570-110">V tomto příkladu je StreetAddress typ se žádná vlastnost identity.</span><span class="sxs-lookup"><span data-stu-id="0c570-110">In this example, StreetAddress is a type with no identity property.</span></span> <span data-ttu-id="0c570-111">Jako vlastnost typu pořadí slouží k určení dodací adresu pro konkrétní objednávku.</span><span class="sxs-lookup"><span data-stu-id="0c570-111">It is used as a property of the Order type to specify the shipping address for a particular order.</span></span> <span data-ttu-id="0c570-112">V `OnModelCreating`, můžeme použít `OwnsOne` metodu pro určení, že je vlastnost ShippingAddress vlastní Entity typu pořadí.</span><span class="sxs-lookup"><span data-stu-id="0c570-112">In `OnModelCreating`, we use the `OwnsOne` method to specify that the ShippingAddress property is an Owned Entity of the Order type.</span></span>

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}

// OnModelCreating
modelBuilder.Entity<Order>().OwnsOne(p => p.ShippingAddress);
```

<span data-ttu-id="0c570-113">Pokud je vlastnost ShippingAddress soukromá v typu pořadí, můžete použít verze řetězce `OwnsOne` metody:</span><span class="sxs-lookup"><span data-stu-id="0c570-113">If the ShippingAddress property is private in the Order type, you can use the string version of the `OwnsOne` method:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(typeof(StreetAddress), "ShippingAddress");
```

<span data-ttu-id="0c570-114">V tomto příkladu používáme `OwnedAttribute` k dosažení stejného cíle:</span><span class="sxs-lookup"><span data-stu-id="0c570-114">In this example, we use the `OwnedAttribute` to achieve the same goal:</span></span>

``` csharp
[Owned]
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}
```

## <a name="implicit-keys"></a><span data-ttu-id="0c570-115">Implicitní klíče</span><span class="sxs-lookup"><span data-stu-id="0c570-115">Implicit keys</span></span>

<span data-ttu-id="0c570-116">V EF Core 2.0 a 2.1 může odkazovat pouze vlastnosti navigace odkaz na vlastní typy.</span><span class="sxs-lookup"><span data-stu-id="0c570-116">In EF Core 2.0 and 2.1, only reference navigation properties can point to owned types.</span></span> <span data-ttu-id="0c570-117">Kolekce vlastněné typy nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="0c570-117">Collections of owned types are not supported.</span></span> <span data-ttu-id="0c570-118">Tyto referenční vlastní typy musí mít vždy po jejím obnovení relace s vlastníkem, proto prohlížející nemusí mít své vlastní hodnoty klíče.</span><span class="sxs-lookup"><span data-stu-id="0c570-118">These reference owned types always have a one-to-one relationship with the owner, therefore they don't need their own key values.</span></span> <span data-ttu-id="0c570-119">V předchozím příkladu StreetAddress typ není nutné definovat vlastnost klíče.</span><span class="sxs-lookup"><span data-stu-id="0c570-119">In the previous example, the StreetAddress type does not need to define a key property.</span></span>  

<span data-ttu-id="0c570-120">Chcete-li pochopit, jak EF Core sleduje tyto objekty, je vhodné popřemýšlet, primární klíč je vytvořen jako [stínové vlastnosti](xref:core/modeling/shadow-properties) pro typ vlastnictví.</span><span class="sxs-lookup"><span data-stu-id="0c570-120">In order to understand how EF Core tracks these objects, it is useful to think that a primary key is created as a [shadow property](xref:core/modeling/shadow-properties) for the owned type.</span></span> <span data-ttu-id="0c570-121">Hodnota klíče instance typu vlastnictví budou stejné jako hodnotu klíče vlastníka instance.</span><span class="sxs-lookup"><span data-stu-id="0c570-121">The value of the key of an instance of the owned type will be the same as the value of the key of the owner instance.</span></span>      

## <a name="mapping-owned-types-with-table-splitting"></a><span data-ttu-id="0c570-122">Mapování vlastní typy s rozdělení tabulky</span><span class="sxs-lookup"><span data-stu-id="0c570-122">Mapping owned types with table splitting</span></span>

<span data-ttu-id="0c570-123">Při použití relačních databází, podle konvence vlastní typy jsou namapovány na stejnou tabulku jako vlastník.</span><span class="sxs-lookup"><span data-stu-id="0c570-123">When using relational databases, by convention owned types are mapped to the same table as the owner.</span></span> <span data-ttu-id="0c570-124">To vyžaduje, aby rozdělení tabulky ve dvou: některé sloupce se použije k uložení dat vlastníka a některé sloupce se použije k ukládání dat vlastní entity.</span><span class="sxs-lookup"><span data-stu-id="0c570-124">This requires splitting the table in two: some columns will be used to store the data of the owner, and some columns will be used to store data of the owned entity.</span></span> <span data-ttu-id="0c570-125">Toto je běžné funkce označuje jako rozdělení tabulky.</span><span class="sxs-lookup"><span data-stu-id="0c570-125">This is a common feature known as table splitting.</span></span>

> [!TIP]
> <span data-ttu-id="0c570-126">Vlastněné typy uloženého s rozdělení tabulky mohou být použity velmi podobně jako na tom, jak komplexní typy, které se používají v EF6.</span><span class="sxs-lookup"><span data-stu-id="0c570-126">Owned types stored with table splitting can be used very similarly to how complex types are used in EF6.</span></span>

<span data-ttu-id="0c570-127">Podle konvence EF Core bude název sloupce databáze pro vlastnosti typu vlastnictví entity podle vzoru _EntityProperty_OwnedEntityProperty_.</span><span class="sxs-lookup"><span data-stu-id="0c570-127">By convention, EF Core will name the database columns for the properties of the owned entity type following the pattern _EntityProperty_OwnedEntityProperty_.</span></span> <span data-ttu-id="0c570-128">Proto StreetAddress vlastnosti se zobrazí v tabulce objednávky s názvy ShippingAddress_Street a ShippingAddress_City.</span><span class="sxs-lookup"><span data-stu-id="0c570-128">Therefore the StreetAddress properties will appear in the Orders table with the names ShippingAddress_Street and ShippingAddress_City.</span></span>

<span data-ttu-id="0c570-129">Můžete připojit `HasColumnName` metoda přejmenování sloupců.</span><span class="sxs-lookup"><span data-stu-id="0c570-129">You can append the `HasColumnName` method to rename those columns.</span></span> <span data-ttu-id="0c570-130">V případě, kdy StreetAddress veřejné vlastnosti by mapování</span><span class="sxs-lookup"><span data-stu-id="0c570-130">In the case where StreetAddress is a public property, the mappings would be</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(
    o => o.ShippingAddress,
    sa =>
        {
            sa.Property(p=>p.Street).HasColumnName("ShipsToStreet");
            sa.Property(p=>p.City).HasColumnName("ShipsToCity");
        });
```

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a><span data-ttu-id="0c570-131">Sdílení stejného typu .NET mezi několika typy vlastnictví</span><span class="sxs-lookup"><span data-stu-id="0c570-131">Sharing the same .NET type among multiple owned types</span></span>

<span data-ttu-id="0c570-132">Typ vlastnictví entity může být stejného typu .NET jako jiný typ vlastnictví entity, proto, že typ formátu .NET nemusí být dost informací k identifikaci typu vlastnictví.</span><span class="sxs-lookup"><span data-stu-id="0c570-132">An owned entity type can be of the same .NET type as another owned entity type, therefore the .NET type may not be enough to identify an owned type.</span></span>

<span data-ttu-id="0c570-133">V těchto případech se vlastnost k vlastní entitě odkazující od vlastníka se stane _definování navigace_ typu vlastnictví entity.</span><span class="sxs-lookup"><span data-stu-id="0c570-133">In those cases, the property pointing from the owner to the owned entity becomes the _defining navigation_ of the owned entity type.</span></span> <span data-ttu-id="0c570-134">Z pohledu EF Core definující navigace je součástí identity typu vedle typ formátu .NET.</span><span class="sxs-lookup"><span data-stu-id="0c570-134">From the perspective of EF Core, the defining navigation is part of the type's identity alongside the .NET type.</span></span>   

<span data-ttu-id="0c570-135">Například ve třídě následující ShippingAddress i BillingAddress jsou stejného typu .NET, StreetAddress:</span><span class="sxs-lookup"><span data-stu-id="0c570-135">For example, in the following class, ShippingAddress and BillingAddress are both of the same .NET type, StreetAddress:</span></span>

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

<span data-ttu-id="0c570-136">Chcete-li pochopit, jak bude EF Core rozlišovat sledované instancí těchto objektů, může být vhodné popřemýšlet, že se definující navigace stala součástí klíče instance u hodnoty klíče vlastníka a typ .NET vlastnictví.</span><span class="sxs-lookup"><span data-stu-id="0c570-136">In order to understand how EF Core will distinguish tracked instances of these objects, it may be useful to think that the defining navigation has become part of the key of the instance alongside the value of the key of the owner and the .NET type of the owned type.</span></span>

## <a name="nested-owned-types"></a><span data-ttu-id="0c570-137">Vnořené typy vlastnictví</span><span class="sxs-lookup"><span data-stu-id="0c570-137">Nested owned types</span></span>

<span data-ttu-id="0c570-138">V tomto příkladu je vlastníkem OrderDetails BillingAddress a ShippingAddress, které jsou oba typy StreetAddress.</span><span class="sxs-lookup"><span data-stu-id="0c570-138">In this example OrderDetails owns BillingAddress and ShippingAddress, which are both StreetAddress types.</span></span> <span data-ttu-id="0c570-139">Potom OrderDetails vlastní typ objednávky.</span><span class="sxs-lookup"><span data-stu-id="0c570-139">Then OrderDetails is owned by the Order type.</span></span>

``` csharp
public class Order
{
    public int Id { get; set; }
    public OrderDetails OrderDetails { get; set; }
    public OrderStatus Status { get; set; }
}

public class OrderDetails
{
    public StreetAddress BillingAddress { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}

public enum OrderStatus
{
    Pending,
    Shipped
}

public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}
```

<span data-ttu-id="0c570-140">Je možné řetězec `OwnsOne` metoda v fluent mapování konfigurace tohoto modelu:</span><span class="sxs-lookup"><span data-stu-id="0c570-140">It is possible to chain the `OwnsOne` method in a fluent mapping to configure this model:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

<span data-ttu-id="0c570-141">Je možné dosáhnout stejnou věc, kterou pomocí `OwnedAttribute` OrderDetails a StreetAdress.</span><span class="sxs-lookup"><span data-stu-id="0c570-141">It is possible to achieve the same thing using `OwnedAttribute` on both OrderDetails and StreetAdress.</span></span>

<span data-ttu-id="0c570-142">Kromě vnořené typy vlastnictví může odkazovat na typ vlastnictví regulární entity.</span><span class="sxs-lookup"><span data-stu-id="0c570-142">In addition to nested owned types, an owned type can reference a regular entity.</span></span> <span data-ttu-id="0c570-143">V následujícím příkladu je země regulární – vlastní entity:</span><span class="sxs-lookup"><span data-stu-id="0c570-143">In the following example, Country is a regular non-owned entity:</span></span>

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

<span data-ttu-id="0c570-144">Tato možnost nastaví v EF6 vlastněné typy entit kromě komplexní typy.</span><span class="sxs-lookup"><span data-stu-id="0c570-144">This capability sets owned entity types apart from complex types in EF6.</span></span>

## <a name="storing-owned-types-in-separate-tables"></a><span data-ttu-id="0c570-145">Ukládání vlastní typy v samostatných tabulkách</span><span class="sxs-lookup"><span data-stu-id="0c570-145">Storing owned types in separate tables</span></span>

<span data-ttu-id="0c570-146">Na rozdíl od komplexní typy EF6, také mohou být vlastněné typy uloženy do samostatné tabulky od vlastníka.</span><span class="sxs-lookup"><span data-stu-id="0c570-146">Also unlike EF6 complex types, owned types can be stored in a separate table from the owner.</span></span> <span data-ttu-id="0c570-147">Aby bylo možné přepsat vytváření názvů, který mapuje typ vlastnictví na stejnou tabulku jako vlastník, můžete jednoduše zavoláte `ToTable` a zadejte jiný název tabulky.</span><span class="sxs-lookup"><span data-stu-id="0c570-147">In order to override the convention that maps an owned type to the same table as the owner, you can simply call `ToTable` and provide a different table name.</span></span> <span data-ttu-id="0c570-148">V následujícím příkladu se namapuje OrderDetails a jeho dvou adres do samostatné tabulky z objednávky:</span><span class="sxs-lookup"><span data-stu-id="0c570-148">The following example will map OrderDetails and its two addresses to a separate table from Order:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
        od.ToTable("OrderDetails");
    });
```

## <a name="querying-owned-types"></a><span data-ttu-id="0c570-149">Zjišťují se vlastněné typy</span><span class="sxs-lookup"><span data-stu-id="0c570-149">Querying owned types</span></span>

<span data-ttu-id="0c570-150">Při dotazování na vlastníka vlastněné typy budou zahrnuty ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="0c570-150">When querying the owner the owned types will be included by default.</span></span> <span data-ttu-id="0c570-151">Není nutné používat `Include` metoda, i když vlastněné typy jsou uložené v samostatné tabulce.</span><span class="sxs-lookup"><span data-stu-id="0c570-151">It is not necessary to use the `Include` method, even if the owned types are stored in a separate table.</span></span> <span data-ttu-id="0c570-152">Na základě modelu před popsané, následující dotaz bude o přijetí změn pořadí, OrderDetails a dva StreetAddresses vlastnictví pro všechny čekající na vyřízení objednávky z databáze:</span><span class="sxs-lookup"><span data-stu-id="0c570-152">Based on the model described before, the following query will pull Order, OrderDetails and the two owned StreetAddresses for all pending orders from the database:</span></span>

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a><span data-ttu-id="0c570-153">Omezení</span><span class="sxs-lookup"><span data-stu-id="0c570-153">Limitations</span></span>

<span data-ttu-id="0c570-154">Některé z těchto omezení jsou základem pro jak vlastněné pracovní typy entit, ale jiná omezení, že můžeme být schopen odebrat v budoucích verzích jsou:</span><span class="sxs-lookup"><span data-stu-id="0c570-154">Some of these limitations are fundamental to how owned entity types work, but some others are restrictions that we may be able to remove in future releases:</span></span>

### <a name="shortcomings-in-previous-versions"></a><span data-ttu-id="0c570-155">Nedostatky v předchozích verzích</span><span class="sxs-lookup"><span data-stu-id="0c570-155">Shortcomings in previous versions</span></span>
- <span data-ttu-id="0c570-156">V EF Core 2.0 navigaci na, který vlastní typy entit se nedá deklarovat v typy odvozené entit, pokud vlastnictví entity jsou explicitně namapovány na samostatnou tabulku z hierarchie vlastníka.</span><span class="sxs-lookup"><span data-stu-id="0c570-156">In EF Core 2.0, navigations to owned entity types cannot be declared in derived entity types unless the owned entities are explicitly mapped to a separate table from the owner hierarchy.</span></span> <span data-ttu-id="0c570-157">Toto omezení byl odebrán v EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="0c570-157">This limitation has been removed in EF Core 2.1</span></span>

### <a name="current-shortcomings"></a><span data-ttu-id="0c570-158">Aktuální nedostatky</span><span class="sxs-lookup"><span data-stu-id="0c570-158">Current shortcomings</span></span>
- <span data-ttu-id="0c570-159">Hierarchie dědičnosti, které zahrnují vlastněné typy entit nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="0c570-159">Inheritance hierarchies that include owned entity types are not supported</span></span>
- <span data-ttu-id="0c570-160">Vlastněné typy entit nemůže být na kterou odkazoval navigační vlastnost kolekce (pouze odkaz, který se aktuálně podporují navigaci)</span><span class="sxs-lookup"><span data-stu-id="0c570-160">Owned entity types cannot be pointed at by a collection navigation property (only reference navigations are currently supported)</span></span>
- <span data-ttu-id="0c570-161">Navigaci vlastněné typy entit nemůže mít hodnotu null, pokud jsou explicitně namapované na samostatnou tabulku od vlastníka</span><span class="sxs-lookup"><span data-stu-id="0c570-161">Navigations to owned entity types cannot be null unless they are explicitly mapped to a separate table from the owner</span></span>
- <span data-ttu-id="0c570-162">Instance vlastněné typy entit nemůže je sdílet více vlastníky (to je dobře známé scénář pro hodnotu objekty, které nelze implementovat s využitím vlastněné typy entit)</span><span class="sxs-lookup"><span data-stu-id="0c570-162">Instances of owned entity types cannot be shared by multiple owners (this is a well-known scenario for value objects that cannot be implemented using owned entity types)</span></span>

### <a name="by-design-restrictions"></a><span data-ttu-id="0c570-163">Omezení podle návrhu</span><span class="sxs-lookup"><span data-stu-id="0c570-163">By-design restrictions</span></span>
- <span data-ttu-id="0c570-164">Nelze vytvořit `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="0c570-164">You cannot create a `DbSet<T>`</span></span>
- <span data-ttu-id="0c570-165">Nejde volat `Entity<T>()` s typem vlastnictví na `ModelBuilder`</span><span class="sxs-lookup"><span data-stu-id="0c570-165">You cannot call `Entity<T>()` with an owned type on `ModelBuilder`</span></span>
