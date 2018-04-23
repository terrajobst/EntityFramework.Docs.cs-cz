---
title: Vlastní typy entit - EF jádra
author: julielerman
ms.author: divega
ms.date: 2/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
ms.technology: entity-framework-core
uid: core/modeling/owned-entities
ms.openlocfilehash: f2f05499a3e3494f420d916df2db19667a6f1e29
ms.sourcegitcommit: 26f33758c47399ae933f22fec8e1d19fa7d2c0b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/19/2018
---
# <a name="owned-entity-types"></a>Typy entit ve vlastnictví

>[!NOTE]
> Tato funkce je v EF základní verze 2.0 nový.

Základní EF umožňuje modelu typy entit, které se mohou objevit pouze někdy na navigační vlastnosti jiných typů entit. Toto nastavení se nazývá _vlastní typy entit_. Entita obsahující typ entity ve vlastnictví je jeho _vlastníka_.

## <a name="explicit-configuration"></a>Explicitní konfigurace

Vlastní entity, které typy jsou nikdy součástí podle EF základní model pomocí konvence. Můžete použít `OwnsOne` metoda v `OnModelCreating` nebo opatřit poznámkami na typ s `OwnedAttribute` (v EF základní 2.1 nového) ke konfiguraci typu jako typ vlastní.

V tomto příkladu je StreetAddress typu s žádná vlastnost identity. Jako vlastnost typu pořadí slouží k určení adresy příjemce pro konkrétní pořadí. V `OnModelCreating`, použijeme `OwnsOne` metoda k určení, zda je vlastnost ShippingAddress vlastněná entita typu pořadí.

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

Pokud je vlastnost ShippingAddress privátní v typu pořadí, můžete použít řetězec verze `OwnsOne` metoda:

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

V EF základní 2.0 a 2.1 můžete pouze navigační vlastnosti odkazu bod pro vlastní typy. Kolekce vlastní typy nejsou podporovány. Tyto reference vlastní typy jsou vždy v relaci se na vlastníka, proto není nutné vlastní hodnoty klíče. V předchozím příkladu typ StreetAddress není nutné definovat klíčovou vlastnost.  

Aby k pochopení, jak EF základní sleduje tyto objekty, je užitečné myslíte, že primární klíč je vytvořen jako [stínové vlastnost](xref:core/modeling/shadow-properties) pro vlastní typ. Hodnota klíče instance vlastní typ bude stejná jako hodnota klíče instance vlastníka.      

## <a name="mapping-owned-types-with-table-splitting"></a>Mapování vlastní typy s rozdělení tabulky

Při použití relačních databází, podle konvence vlastní typy jsou namapované na stejné tabulce jako vlastník. To vyžaduje rozdělení tabulky do dvou: některé sloupce se použije k uložení dat vlastníka a některé sloupce se použije k ukládání dat vlastní entity. Toto je běžné funkce označované jako rozdělení tabulky.

> [!TIP]
> Vlastní typy s rozdělení tabulky lze použít na tom, jak komplexní typy, které se používají v EF6 velmi podobně.

Podle konvence EF základní bude název sloupce databáze pro vlastnosti typu entity ve vlastnictví následující vzor _EntityProperty_OwnedEntityProperty_. Proto se zobrazí vlastnosti StreetAddress v tabulce objednávky s názvy ShippingAddress_Street a ShippingAddress_City.

Můžete připojit `HasColumnName` metoda přejmenovat tyto sloupce. V případě, kdy StreetAddress veřejná vlastnost bude mapování

``` csharp
modelBuilder.Entity<Order>().OwnsOne(
    o => o.ShippingAddress,
    sa =>
        {
            sa.Property(p=>p.Street).HasColumnName("ShipsToStreet");
            sa.Property(p=>p.City).HasColumnName("ShipsToCity");
        });
```

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a>Sdílení stejný typ formátu .NET mezi více vlastní typy

Typ entity ve vlastnictví může být stejného typu .NET jako jiný typ vlastní entity, proto, že typ formátu .NET nemusí stačit k identifikaci vlastní typ.

V takových případech bude vlastnost od vlastníka odkazující na vlastní entity _definování navigační_ typu vlastní entity. Z pohledu EF jádra definující navigační je součástí identity typu souběžně s typ formátu .NET.   

Například ve třídě následující ShippingAddress a BillingAddress jsou obě typu .NET, StreetAddress:

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

Chcete-li pochopit, jak bude EF základní rozlišit sledovaných instancí těchto objektů, může být užitečné myslíte, že definující navigační stala součástí klíče instance u hodnoty klíče vlastníka a typ formátu .NET ve vlastnictví typu.

## <a name="nested-owned-types"></a>Vnořené typy ve vlastnictví

V tomto příkladu vlastní Rozpis objednávek BillingAddress a ShippingAddress, které jsou oba typy StreetAddress. Potom Rozpis objednávek vlastní podle typu pořadí.

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

Je možné řetězec `OwnsOne` metoda fluent mapování ke konfiguraci tohoto modelu:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

Je možné dosáhnout stejnou věc pomocí `OwnedAttribute` na Rozpis objednávek a StreetAdress.

Kromě vnořené vlastní typy můžete odkazovat vlastní typ regulární entity. V následujícím příkladu je země regulární entita (tj. bez vlastněných):

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

Tato funkce nastaví EF6 typy entit ve vlastnictví kromě komplexní typy.

## <a name="storing-owned-types-in-separate-tables"></a>Ukládání vlastní typy v samostatných tabulkách

Na rozdíl od EF6 komplexní typy, také může být vlastní typy uložená do samostatné tabulky od vlastníka. Chcete-li přepsat názvů, který se mapuje na stejné tabulce jako vlastník vlastní typ, můžete jednoduše volat `ToTable` a zadejte jiný název tabulky. Následující příklad mapování Rozpis objednávek a jeho dvě adresy do samostatné tabulky z pořadí:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    }).ToTable("OrderDetails");
```

## <a name="querying-owned-types"></a>Dotazování vlastní typy

Při dotazování vlastník, budou ve výchozím nastavení zahrnuty vlastní typy. Není nutné k použití `Include` metoda, i když vlastní typy jsou uloženy do samostatné tabulky. Podle modelu před popsané, následující dotaz načte pořadí, Rozpis objednávek a dva vlastní StreeAddresses všechny čekající objednávky z databáze:

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a>Omezení

Tady jsou některá omezení typů entit ve vlastnictví. Některé z těchto omezení jsou nezbytné, aby pracovní jak vlastní typy, ale jiná jsou v daném okamžiku omezení, které jsme chtěli odebrat v budoucích verzích:

### <a name="current-shortcomings"></a>Aktuální nedostatků
- Dědičnost vlastní typy není podporován.
- Vlastní typy nemůže být na který odkazuje navigační vlastnost kolekce
- Vzhledem k tomu, že používají rozdělení tabulky pomocí vlastněných výchozí typy také mají následující omezení, pokud explicitně namapované na jiné tabulky:
   - Nemůže vlastnit odvozený typ
   - Definující navigační vlastnost nelze nastavit na hodnotu null (tj. vlastněných vždy jsou požadovány typy ve stejné tabulce)

### <a name="by-design-restrictions"></a>Omezení podle návrhu
- Nelze vytvořit `DbSet<T>`
- Nelze volat `Entity<T>()` s vlastní typ na `ModelBuilder`
