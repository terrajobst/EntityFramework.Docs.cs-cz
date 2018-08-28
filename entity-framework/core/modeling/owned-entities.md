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
# <a name="owned-entity-types"></a>Vlastněné typy entit

>[!NOTE]
> Tato funkce je v EF Core 2.0 nová.

EF Core umožňuje modelu typy entit, které může vyskytovat vždy jen u vlastnosti navigace z ostatních typů entit. Toto nastavení se nazývá _vlastněné typy entit_. Entita obsahující typ vlastnictví entity je jeho _vlastníka_.

## <a name="explicit-configuration"></a>Explicitní konfigurace

Vlastní entity, které typy jsou nikdy součástí pomocí EF Core modelu konvencí. Můžete použít `OwnsOne` metoda ve `OnModelCreating` nebo typ s poznámkami `OwnedAttribute` (novinka v EF Core 2.1) ke konfiguraci typu jako typ vlastnictví.

V tomto příkladu je StreetAddress typ se žádná vlastnost identity. Jako vlastnost typu pořadí slouží k určení dodací adresu pro konkrétní objednávku. V `OnModelCreating`, můžeme použít `OwnsOne` metodu pro určení, že je vlastnost ShippingAddress vlastní Entity typu pořadí.

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

Pokud je vlastnost ShippingAddress soukromá v typu pořadí, můžete použít verze řetězce `OwnsOne` metody:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(typeof(StreetAddress), "ShippingAddress");
```

V tomto příkladu používáme `OwnedAttribute` k dosažení stejného cíle:

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

## <a name="implicit-keys"></a>Implicitní klíče

V EF Core 2.0 a 2.1 může odkazovat pouze vlastnosti navigace odkaz na vlastní typy. Kolekce vlastněné typy nejsou podporovány. Tyto referenční vlastní typy musí mít vždy po jejím obnovení relace s vlastníkem, proto prohlížející nemusí mít své vlastní hodnoty klíče. V předchozím příkladu StreetAddress typ není nutné definovat vlastnost klíče.  

Chcete-li pochopit, jak EF Core sleduje tyto objekty, je vhodné popřemýšlet, primární klíč je vytvořen jako [stínové vlastnosti](xref:core/modeling/shadow-properties) pro typ vlastnictví. Hodnota klíče instance typu vlastnictví budou stejné jako hodnotu klíče vlastníka instance.      

## <a name="mapping-owned-types-with-table-splitting"></a>Mapování vlastní typy s rozdělení tabulky

Při použití relačních databází, podle konvence vlastní typy jsou namapovány na stejnou tabulku jako vlastník. To vyžaduje, aby rozdělení tabulky ve dvou: některé sloupce se použije k uložení dat vlastníka a některé sloupce se použije k ukládání dat vlastní entity. Toto je běžné funkce označuje jako rozdělení tabulky.

> [!TIP]
> Vlastněné typy uloženého s rozdělení tabulky mohou být použity velmi podobně jako na tom, jak komplexní typy, které se používají v EF6.

Podle konvence EF Core bude název sloupce databáze pro vlastnosti typu vlastnictví entity podle vzoru _EntityProperty_OwnedEntityProperty_. Proto StreetAddress vlastnosti se zobrazí v tabulce objednávky s názvy ShippingAddress_Street a ShippingAddress_City.

Můžete připojit `HasColumnName` metoda přejmenování sloupců. V případě, kdy StreetAddress veřejné vlastnosti by mapování

``` csharp
modelBuilder.Entity<Order>().OwnsOne(
    o => o.ShippingAddress,
    sa =>
        {
            sa.Property(p=>p.Street).HasColumnName("ShipsToStreet");
            sa.Property(p=>p.City).HasColumnName("ShipsToCity");
        });
```

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a>Sdílení stejného typu .NET mezi několika typy vlastnictví

Typ vlastnictví entity může být stejného typu .NET jako jiný typ vlastnictví entity, proto, že typ formátu .NET nemusí být dost informací k identifikaci typu vlastnictví.

V těchto případech se vlastnost k vlastní entitě odkazující od vlastníka se stane _definování navigace_ typu vlastnictví entity. Z pohledu EF Core definující navigace je součástí identity typu vedle typ formátu .NET.   

Například ve třídě následující ShippingAddress i BillingAddress jsou stejného typu .NET, StreetAddress:

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

Chcete-li pochopit, jak bude EF Core rozlišovat sledované instancí těchto objektů, může být vhodné popřemýšlet, že se definující navigace stala součástí klíče instance u hodnoty klíče vlastníka a typ .NET vlastnictví.

## <a name="nested-owned-types"></a>Vnořené typy vlastnictví

V tomto příkladu je vlastníkem OrderDetails BillingAddress a ShippingAddress, které jsou oba typy StreetAddress. Potom OrderDetails vlastní typ objednávky.

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

Je možné řetězec `OwnsOne` metoda v fluent mapování konfigurace tohoto modelu:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

Je možné dosáhnout stejnou věc, kterou pomocí `OwnedAttribute` OrderDetails a StreetAdress.

Kromě vnořené typy vlastnictví může odkazovat na typ vlastnictví regulární entity. V následujícím příkladu je země regulární – vlastní entity:

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

Tato možnost nastaví v EF6 vlastněné typy entit kromě komplexní typy.

## <a name="storing-owned-types-in-separate-tables"></a>Ukládání vlastní typy v samostatných tabulkách

Na rozdíl od komplexní typy EF6, také mohou být vlastněné typy uloženy do samostatné tabulky od vlastníka. Aby bylo možné přepsat vytváření názvů, který mapuje typ vlastnictví na stejnou tabulku jako vlastník, můžete jednoduše zavoláte `ToTable` a zadejte jiný název tabulky. V následujícím příkladu se namapuje OrderDetails a jeho dvou adres do samostatné tabulky z objednávky:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
        od.ToTable("OrderDetails");
    });
```

## <a name="querying-owned-types"></a>Zjišťují se vlastněné typy

Při dotazování na vlastníka vlastněné typy budou zahrnuty ve výchozím nastavení. Není nutné používat `Include` metoda, i když vlastněné typy jsou uložené v samostatné tabulce. Na základě modelu před popsané, následující dotaz bude o přijetí změn pořadí, OrderDetails a dva StreetAddresses vlastnictví pro všechny čekající na vyřízení objednávky z databáze:

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a>Omezení

Některé z těchto omezení jsou základem pro jak vlastněné pracovní typy entit, ale jiná omezení, že můžeme být schopen odebrat v budoucích verzích jsou:

### <a name="shortcomings-in-previous-versions"></a>Nedostatky v předchozích verzích
- V EF Core 2.0 navigaci na, který vlastní typy entit se nedá deklarovat v typy odvozené entit, pokud vlastnictví entity jsou explicitně namapovány na samostatnou tabulku z hierarchie vlastníka. Toto omezení byl odebrán v EF Core 2.1

### <a name="current-shortcomings"></a>Aktuální nedostatky
- Hierarchie dědičnosti, které zahrnují vlastněné typy entit nejsou podporovány.
- Vlastněné typy entit nemůže být na kterou odkazoval navigační vlastnost kolekce (pouze odkaz, který se aktuálně podporují navigaci)
- Navigaci vlastněné typy entit nemůže mít hodnotu null, pokud jsou explicitně namapované na samostatnou tabulku od vlastníka
- Instance vlastněné typy entit nemůže je sdílet více vlastníky (to je dobře známé scénář pro hodnotu objekty, které nelze implementovat s využitím vlastněné typy entit)

### <a name="by-design-restrictions"></a>Omezení podle návrhu
- Nelze vytvořit `DbSet<T>`
- Nejde volat `Entity<T>()` s typem vlastnictví na `ModelBuilder`
