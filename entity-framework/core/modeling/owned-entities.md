---
title: "Vlastní typy entit - EF jádra"
author: julielerman
ms.author: divega
ms.date: 2/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
ms.technology: entity-framework-core
uid: core/modeling/owned-entities
ms.openlocfilehash: a6823377eb626ca92263c31351e1aef61db5a787
ms.sourcegitcommit: 4b7d3d3e258b0d9cb778bb45a9f4a33c0792e38e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="owned-entity-types"></a><span data-ttu-id="321aa-102">Typy entit ve vlastnictví</span><span class="sxs-lookup"><span data-stu-id="321aa-102">Owned Entity Types</span></span>

>[!NOTE]
> <span data-ttu-id="321aa-103">Tato funkce je v EF základní verze 2.0 nový.</span><span class="sxs-lookup"><span data-stu-id="321aa-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="321aa-104">Základní EF umožňuje modelu typy entit, které se mohou objevit pouze někdy na navigační vlastnosti jiných typů entit.</span><span class="sxs-lookup"><span data-stu-id="321aa-104">EF Core allows you to model entity types that can only ever appear on navigation properties of other entity types.</span></span> <span data-ttu-id="321aa-105">Toto nastavení se nazývá _vlastní typy entit_.</span><span class="sxs-lookup"><span data-stu-id="321aa-105">These are called _owned entity types_.</span></span> <span data-ttu-id="321aa-106">Entita obsahující typ entity ve vlastnictví je jeho _vlastníka_.</span><span class="sxs-lookup"><span data-stu-id="321aa-106">The entity containing an owned entity type is its _owner_.</span></span>

## <a name="explicit-configuration"></a><span data-ttu-id="321aa-107">Explicitní konfigurace</span><span class="sxs-lookup"><span data-stu-id="321aa-107">Explicit configuration</span></span>

<span data-ttu-id="321aa-108">Vlastní entity, které typy jsou nikdy součástí podle EF základní model pomocí konvence.</span><span class="sxs-lookup"><span data-stu-id="321aa-108">Owned entity types are never included by EF Core in the model by convention.</span></span> <span data-ttu-id="321aa-109">Můžete použít `OwnsOne` metoda v `OnModelCreating` nebo opatřit poznámkami na typ s `OwnedAttrbibute` (v EF základní 2.1 nového) ke konfiguraci typu jako typ vlastní.</span><span class="sxs-lookup"><span data-stu-id="321aa-109">You can use the `OwnsOne` method in `OnModelCreating` or annotate the type with `OwnedAttrbibute` (new in EF Core 2.1) to configure the type as an owned type.</span></span>

<span data-ttu-id="321aa-110">V tomto příkladu je StreetAddress typu s žádná vlastnost identity.</span><span class="sxs-lookup"><span data-stu-id="321aa-110">In this example, StreetAddress is a type with no identity property.</span></span> <span data-ttu-id="321aa-111">Jako vlastnost typu pořadí slouží k určení adresy příjemce pro konkrétní pořadí.</span><span class="sxs-lookup"><span data-stu-id="321aa-111">It is used as a property of the Order type to specify the shipping address for a particular order.</span></span> <span data-ttu-id="321aa-112">V `OnModelCreating`, použijeme `OwnsOne` metoda k určení, zda je vlastnost ShippingAddress vlastněná entita typu pořadí.</span><span class="sxs-lookup"><span data-stu-id="321aa-112">In `OnModelCreating`, we use the `OwnsOne` method to specify that the ShippingAddress property is an Owned Entity of the Order type.</span></span>

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

<span data-ttu-id="321aa-113">Pokud je vlastnost ShippingAddress privátní v typu pořadí, můžete použít řetězec verze `OwnsOne` metoda:</span><span class="sxs-lookup"><span data-stu-id="321aa-113">If the ShippingAddress property is private in the Order type, you can use the string version of the `OwnsOne` method:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(typeof(StreetAddress), "ShippingAddress");
```

<span data-ttu-id="321aa-114">V tomto příkladu používáme `OwnedAttribute` k dosažení stejného cíle:</span><span class="sxs-lookup"><span data-stu-id="321aa-114">In this example, we use the `OwnedAttribute` to achieve the same goal:</span></span>

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

## <a name="implicit-keys"></a><span data-ttu-id="321aa-115">Implicitní klíče</span><span class="sxs-lookup"><span data-stu-id="321aa-115">Implicit keys</span></span>

<span data-ttu-id="321aa-116">V EF základní 2.0 a 2.1 můžete pouze navigační vlastnosti odkazu bod pro vlastní typy.</span><span class="sxs-lookup"><span data-stu-id="321aa-116">In EF Core 2.0 and 2.1, only reference navigation properties can point to owned types.</span></span> <span data-ttu-id="321aa-117">Kolekce vlastní typy nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="321aa-117">Collections of owned types are not supported.</span></span> <span data-ttu-id="321aa-118">Tyto reference vlastní typy jsou vždy v relaci se na vlastníka, proto není nutné vlastní hodnoty klíče.</span><span class="sxs-lookup"><span data-stu-id="321aa-118">These reference owned types always have a one-to-one relationship with the owner, therefore they don't need their own key values.</span></span> <span data-ttu-id="321aa-119">V předchozím příkladu typ StreetAddress není nutné definovat klíčovou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="321aa-119">In the previous example, the StreetAddress type does not need to define a key property.</span></span>  

<span data-ttu-id="321aa-120">Aby k pochopení, jak EF základní sleduje tyto objekty, je užitečné myslíte, že primární klíč je vytvořen jako [stínové vlastnost](xref:core/modeling/shadow-properties) pro vlastní typ.</span><span class="sxs-lookup"><span data-stu-id="321aa-120">In order to understanding how EF Core tracks these objects, it is useful to think that a primary key is created as a [shadow property](xref:core/modeling/shadow-properties) for the owned type.</span></span> <span data-ttu-id="321aa-121">Hodnota klíče instance vlastní typ bude stejná jako hodnota klíče instance vlastníka.</span><span class="sxs-lookup"><span data-stu-id="321aa-121">The value of the key of an instance of the owned type will be the same as the value of the key of the owner instance.</span></span>      

## <a name="mapping-owned-types-with-table-splitting"></a><span data-ttu-id="321aa-122">Mapování vlastní typy s rozdělení tabulky</span><span class="sxs-lookup"><span data-stu-id="321aa-122">Mapping owned types with table splitting</span></span>

<span data-ttu-id="321aa-123">Při použití relačních databází, podle konvence vlastní typy jsou namapované na stejné tabulce jako vlastník.</span><span class="sxs-lookup"><span data-stu-id="321aa-123">When using relational databases, by convention owned types are mapped to the same table as the owner.</span></span> <span data-ttu-id="321aa-124">To vyžaduje rozdělení tabulky do dvou: některé sloupce se použije k uložení dat vlastníka a některé sloupce se použije k ukládání dat vlastní entity.</span><span class="sxs-lookup"><span data-stu-id="321aa-124">This requires splitting the table in two: some columns will be used to store the data of the owner, and some columns will be used to store data of the owned entity.</span></span> <span data-ttu-id="321aa-125">Toto je běžné funkce označované jako rozdělení tabulky.</span><span class="sxs-lookup"><span data-stu-id="321aa-125">This is a common feature known as table splitting.</span></span>

> [!TIP]
> <span data-ttu-id="321aa-126">Vlastní typy s rozdělení tabulky lze použít na tom, jak komplexní typy, které se používají v EF6 velmi podobně.</span><span class="sxs-lookup"><span data-stu-id="321aa-126">Owned types stored with table splitting can be used very similarly to how complex types are used in EF6.</span></span>

<span data-ttu-id="321aa-127">Podle konvence EF základní bude název sloupce databáze pro vlastnosti typu entity ve vlastnictví následující vzor _EntityProperty_OwnedEntityProperty_.</span><span class="sxs-lookup"><span data-stu-id="321aa-127">By convention, EF Core will name the database columns for the properties of the owned entity type following the pattern _EntityProperty_OwnedEntityProperty_.</span></span> <span data-ttu-id="321aa-128">Proto se zobrazí vlastnosti StreetAddress v tabulce objednávky s názvy ShippingAddress_Street a ShippingAddress_City.</span><span class="sxs-lookup"><span data-stu-id="321aa-128">Therefore the StreetAddress properties will appear in the Orders table with the names ShippingAddress_Street and ShippingAddress_City.</span></span>

<span data-ttu-id="321aa-129">Můžete připojit `HasColumnName` metoda přejmenovat tyto sloupce.</span><span class="sxs-lookup"><span data-stu-id="321aa-129">You can append the `HasColumnName` method to rename those columns.</span></span> <span data-ttu-id="321aa-130">V případě, kdy StreetAddress veřejná vlastnost bude mapování</span><span class="sxs-lookup"><span data-stu-id="321aa-130">In the case where StreetAddress is a public property, the mappings would be</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(
    o => o.ShippingAddress,
    sa =>
        {
            sa.Property(p=>p.Street).HasColumnName("ShipsToStreet");
            sa.Property(p=>p.City).HasColumnName("ShipsToCity");
        });
```

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a><span data-ttu-id="321aa-131">Sdílení stejný typ formátu .NET mezi více vlastní typy</span><span class="sxs-lookup"><span data-stu-id="321aa-131">Sharing the same .NET type among multiple owned types</span></span>

<span data-ttu-id="321aa-132">Typ entity ve vlastnictví může být stejného typu .NET jako jiný typ vlastní entity, proto, že typ formátu .NET nemusí stačit k identifikaci vlastní typ.</span><span class="sxs-lookup"><span data-stu-id="321aa-132">An owned entity type can be of the same .NET type as another owned entity type, therefore the .NET type may not be enough to identify an owned type.</span></span>

<span data-ttu-id="321aa-133">V takových případech bude vlastnost od vlastníka odkazující na vlastní entity _definování navigační_ typu vlastní entity.</span><span class="sxs-lookup"><span data-stu-id="321aa-133">In those cases, the property pointing from the owner to the owned entity becomes the _defining navigation_ of the owned entity type.</span></span> <span data-ttu-id="321aa-134">Z pohledu EF jádra definující navigační je součástí identity typu souběžně s typ formátu .NET.</span><span class="sxs-lookup"><span data-stu-id="321aa-134">From the perspective of EF Core, the defining navigation is part of the type's identity alongside the .NET type.</span></span>   

<span data-ttu-id="321aa-135">Například ve třídě následující ShippingAddress a BillingAddress jsou obě typu .NET, StreetAddress:</span><span class="sxs-lookup"><span data-stu-id="321aa-135">For example, in the following class, ShippingAddress and BillingAddress are both of the same .NET type, StreetAddress:</span></span>

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

<span data-ttu-id="321aa-136">Chcete-li pochopit, jak bude EF základní rozlišit sledovaných instancí těchto objektů, může být užitečné myslíte, že definující navigační stala součástí klíče instance u hodnoty klíče vlastníka a typ formátu .NET ve vlastnictví typu.</span><span class="sxs-lookup"><span data-stu-id="321aa-136">In order to understand how EF Core will distinguish tracked instances of these objects, it may be useful to think that the defining navigation has become part of the key of the instance alongside the value of the key of the owner and the .NET type of the owned type.</span></span>

## <a name="nested-owned-types"></a><span data-ttu-id="321aa-137">Vnořené typy ve vlastnictví</span><span class="sxs-lookup"><span data-stu-id="321aa-137">Nested owned types</span></span>

<span data-ttu-id="321aa-138">V tomto příkladu vlastní Rozpis objednávek BillingAddress a ShippingAddress, které jsou oba typy StreetAddress.</span><span class="sxs-lookup"><span data-stu-id="321aa-138">In this example OrderDetails owns BillingAddress and ShippingAddress, which are both StreetAddress types.</span></span> <span data-ttu-id="321aa-139">Potom Rozpis objednávek vlastní podle typu pořadí.</span><span class="sxs-lookup"><span data-stu-id="321aa-139">Then OrderDetails is owned by the Order type.</span></span>

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

public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}
```

<span data-ttu-id="321aa-140">Je možné řetězec `OwnsOne` metoda fluent mapování ke konfiguraci tohoto modelu:</span><span class="sxs-lookup"><span data-stu-id="321aa-140">It is possible to chain the `OwnsOne` method in a fluent mapping to configure this model:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

<span data-ttu-id="321aa-141">Je možné dosáhnout stejnou věc pomocí `OwnedAttribute` na Rozpis objednávek a StreetAdress.</span><span class="sxs-lookup"><span data-stu-id="321aa-141">It is possible to achieve the same thing using `OwnedAttribute` on both OrderDetails and StreetAdress.</span></span>

<span data-ttu-id="321aa-142">Kromě vnořené vlastní typy můžete odkazovat vlastní typ regulární entity.</span><span class="sxs-lookup"><span data-stu-id="321aa-142">In addition to nested owned types, an owned type can reference a regular entity.</span></span> <span data-ttu-id="321aa-143">V následujícím příkladu je země regulární entita (tj. bez vlastněných):</span><span class="sxs-lookup"><span data-stu-id="321aa-143">In the following example, Country is a regular (i.e. non-owned) entity:</span></span>

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

<span data-ttu-id="321aa-144">Tato funkce nastaví EF6 typy entit ve vlastnictví kromě komplexní typy.</span><span class="sxs-lookup"><span data-stu-id="321aa-144">This capability sets owned entity types apart from complex types in EF6.</span></span>

## <a name="storing-owned-types-in-separate-tables"></a><span data-ttu-id="321aa-145">Ukládání vlastní typy v samostatných tabulkách</span><span class="sxs-lookup"><span data-stu-id="321aa-145">Storing owned types in separate tables</span></span>

<span data-ttu-id="321aa-146">Na rozdíl od EF6 komplexní typy, také může být vlastní typy uložená do samostatné tabulky od vlastníka.</span><span class="sxs-lookup"><span data-stu-id="321aa-146">Also unlike EF6 complex types, owned types can be stored in a separate table from the owner.</span></span> <span data-ttu-id="321aa-147">Chcete-li přepsat názvů, který se mapuje na stejné tabulce jako vlastník vlastní typ, můžete jednoduše volat `ToTable` a zadejte jiný název tabulky.</span><span class="sxs-lookup"><span data-stu-id="321aa-147">In order to override the convention that maps an owned type to the same table as the owner, you can simply call `ToTable` and provide a different table name.</span></span> <span data-ttu-id="321aa-148">Následující příklad mapování Rozpis objednávek a jeho dvě adresy do samostatné tabulky z pořadí:</span><span class="sxs-lookup"><span data-stu-id="321aa-148">The following example will map OrderDetails and its two addresses to a separate table from Order:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    }).ToTable("OrderDetails");
```

## <a name="querying-owned-types"></a><span data-ttu-id="321aa-149">Dotazování vlastní typy</span><span class="sxs-lookup"><span data-stu-id="321aa-149">Querying owned types</span></span>

<span data-ttu-id="321aa-150">Při dotazování vlastník, budou ve výchozím nastavení zahrnuty vlastní typy.</span><span class="sxs-lookup"><span data-stu-id="321aa-150">When querying the owner the owned types will be included by default.</span></span> <span data-ttu-id="321aa-151">Není nutné k použití `Include` metoda, i když vlastní typy jsou uloženy do samostatné tabulky.</span><span class="sxs-lookup"><span data-stu-id="321aa-151">It is not necessary to use the `Include` method, even if the owned types are stored in a separate table.</span></span> <span data-ttu-id="321aa-152">Podle modelu před popsané, následující dotaz načte pořadí, Rozpis objednávek a dva vlastní StreeAddresses všechny čekající objednávky z databáze:</span><span class="sxs-lookup"><span data-stu-id="321aa-152">Based on the model described before, the following query will pull Order, OrderDetails and the two owned StreeAddresses for all pending orders from the database:</span></span>

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a><span data-ttu-id="321aa-153">Omezení</span><span class="sxs-lookup"><span data-stu-id="321aa-153">Limitations</span></span>

<span data-ttu-id="321aa-154">Tady jsou některá omezení typů entit ve vlastnictví.</span><span class="sxs-lookup"><span data-stu-id="321aa-154">Here are some limitations of owned entity types.</span></span> <span data-ttu-id="321aa-155">Některé z těchto omezení jsou nezbytné, aby pracovní jak vlastní typy, ale jiná jsou v daném okamžiku omezení, které jsme chtěli odebrat v budoucích verzích:</span><span class="sxs-lookup"><span data-stu-id="321aa-155">Some of these limitations are fundamental to how owned types work, but some others are point-in-time restrictions that we would like to remove in future releases:</span></span>

### <a name="current-shortcomings"></a><span data-ttu-id="321aa-156">Aktuální nedostatků</span><span class="sxs-lookup"><span data-stu-id="321aa-156">Current shortcomings</span></span>
- <span data-ttu-id="321aa-157">Dědičnost vlastní typy není podporován.</span><span class="sxs-lookup"><span data-stu-id="321aa-157">Inheritance of owned types is not supported</span></span>
- <span data-ttu-id="321aa-158">Vlastní typy nemůže být na který odkazuje navigační vlastnost kolekce</span><span class="sxs-lookup"><span data-stu-id="321aa-158">Owned types cannot be pointed at by a collection navigation property</span></span>
- <span data-ttu-id="321aa-159">Vzhledem k tomu, že používají rozdělení tabulky pomocí vlastněných výchozí typy také mají následující omezení, pokud explicitně namapované na jiné tabulky:</span><span class="sxs-lookup"><span data-stu-id="321aa-159">Since they use table splitting by default owned types also have the following restrictions unless explicitly mapped to a different table:</span></span>
   - <span data-ttu-id="321aa-160">Nemůže vlastnit odvozený typ</span><span class="sxs-lookup"><span data-stu-id="321aa-160">They cannot be owned by a derived type</span></span>
   - <span data-ttu-id="321aa-161">Definující navigační vlastnost nelze nastavit na hodnotu null (tj. vlastněných vždy jsou požadovány typy ve stejné tabulce)</span><span class="sxs-lookup"><span data-stu-id="321aa-161">The defining navigation property cannot be set to null (i.e. owned types on the same table are always required)</span></span>

### <a name="by-design-restrictions"></a><span data-ttu-id="321aa-162">Omezení podle návrhu</span><span class="sxs-lookup"><span data-stu-id="321aa-162">By-design restrictions</span></span>
- <span data-ttu-id="321aa-163">Nelze vytvořit `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="321aa-163">You cannot create a `DbSet<T>`</span></span>
- <span data-ttu-id="321aa-164">Nelze volat `Entity<T>()` s vlastní typ na `ModelBuilder`</span><span class="sxs-lookup"><span data-stu-id="321aa-164">You cannot call `Entity<T>()` with an owned type on `ModelBuilder`</span></span>
