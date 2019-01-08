---
title: Vlastněné typy entit – EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 02/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
uid: core/modeling/owned-entities
ms.openlocfilehash: b2d72b08de79939904bf4e726c695440c906a8aa
ms.sourcegitcommit: 7bde8e6ad3c4565a4638646ce04bcf5e66f7b5fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/07/2019
ms.locfileid: "54069201"
---
# <a name="owned-entity-types"></a>Vlastněné typy entit

>[!NOTE]
> Tato funkce je v EF Core 2.0 nová.

EF Core umožňuje modelu typy entit, které může vyskytovat vždy jen u vlastnosti navigace z ostatních typů entit. Toto nastavení se nazývá _vlastněné typy entit_. Entita obsahující typ vlastnictví entity je jeho _vlastníka_.

## <a name="explicit-configuration"></a>Explicitní konfigurace

Vlastní entity, které typy jsou nikdy součástí pomocí EF Core modelu konvencí. Můžete použít `OwnsOne` metoda ve `OnModelCreating` nebo typ s poznámkami `OwnedAttribute` (novinka v EF Core 2.1) ke konfiguraci typu jako typ vlastnictví.

V tomto příkladu `StreetAddress` je typ s žádnou vlastnost identity. Jako vlastnost typu pořadí slouží k určení dodací adresu pro konkrétní objednávku.

Můžeme použít `OwnedAttribute` považovat za vlastnictví entity při odkazování z jiného typu entity:

[!code-csharp[StreetAddress](../../../samples/core/Modeling/OwnedEntities/StreetAddress.cs?name=StreetAddress)]

[!code-csharp[Order](../../../samples/core/Modeling/OwnedEntities/Order.cs?name=Order)]

Je také možné použít `OwnsOne` metoda ve `OnModelCreating` určit, že `ShippingAddress` vlastností je vlastní Entity `Order` typu entity a v případě potřeby nakonfigurujte další charakteristiky.

[!code-csharp[OwnsOne](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOne)]

Pokud `ShippingAddress` vlastnost je v privátní `Order` typ, můžete použít verze řetězce `OwnsOne` metody:

[!code-csharp[OwnsOneString](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneString)]

Zobrazit [úplný ukázkový projekt](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) pro širší kontext. 

## <a name="implicit-keys"></a>Implicitní klíče

Vlastněné typy nakonfigurovanou `OwnsOne` nebo zjištěný prostřednictvím navigační odkaz vždy mít relaci s vlastníkem, proto nepotřebují vlastní hodnoty klíče, jako jsou jedinečné hodnoty cizího klíče. V předchozím příkladu `StreetAddress` typ není nutné definovat vlastnost klíče.  

Chcete-li pochopit, jak EF Core sleduje tyto objekty, je vhodné popřemýšlet, primární klíč je vytvořen jako [stínové vlastnosti](xref:core/modeling/shadow-properties) pro typ vlastnictví. Hodnota klíče instance typu vlastnictví budou stejné jako hodnotu klíče vlastníka instance.

## <a name="collections-of-owned-types"></a>Vlastněné typy kolekcí

>[!NOTE]
> Tato funkce je nového v EF Core 2.2.

Ke konfiguraci kolekce vlastněné typy `OwnsMany` byste měli použít ve `OnModelCreating`. Ale primární klíč se nenakonfigurují podle konvence, proto musí být zadané explicitně přímo. Je běžné použití komplexní klíče u těchto typů entit začlenění cizí klíč pro vlastníka a dalších jedinečnou vlastnost, která může také být ve stavu stínové:

[!code-csharp[OwnsMany](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsMany)]

## <a name="mapping-owned-types-with-table-splitting"></a>Mapování vlastní typy s rozdělení tabulky

Při používání relační databáze, podle úmluvy odkazu, který vlastní typy jsou namapovány na stejnou tabulku jako vlastník. To vyžaduje, aby rozdělení tabulky ve dvou: některé sloupce se použije k uložení dat vlastníka a některé sloupce se použije k ukládání dat vlastní entity. Toto je běžné funkce označuje jako rozdělení tabulky.

> [!TIP]
> Vlastněné typy uloženého s rozdělení tabulky je možné použít jak komplexní typy, které se používají v EF6 podobně.

Podle konvence EF Core bude název sloupce databáze pro vlastnosti typu vlastnictví entity podle vzoru _Navigation_OwnedEntityProperty_. Proto `StreetAddress` vlastnosti se zobrazí v tabulce "Orders" s názvy "ShippingAddress_Street" a "ShippingAddress_City".

Můžete připojit `HasColumnName` metoda přejmenovat tyto sloupce:

[!code-csharp[ColumnNames](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=ColumnNames)]

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a>Sdílení stejného typu .NET mezi několika typy vlastnictví

Typ vlastnictví entity může být stejného typu .NET jako jiný typ vlastnictví entity, proto, že typ formátu .NET nemusí být dost informací k identifikaci typu vlastnictví.

V těchto případech se vlastnost k vlastní entitě odkazující od vlastníka se stane _definování navigace_ typu vlastnictví entity. Z pohledu EF Core definující navigace je součástí identity typu vedle typ formátu .NET.   

Například ve třídě následující `ShippingAddress` a `BillingAddress` jsou stejného typu .NET `StreetAddress`:

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

Chcete-li pochopit, jak bude EF Core rozlišovat sledované instancí těchto objektů, může být vhodné popřemýšlet, že se definující navigace stala součástí klíče instance u hodnoty klíče vlastníka a typ .NET vlastnictví.

## <a name="nested-owned-types"></a>Vnořené typy vlastnictví

V tomto příkladu `OrderDetails` vlastní `BillingAddress` a `ShippingAddress`, které jsou obě `StreetAddress` typy. Potom `OrderDetails` není ve vlastnictví `DetailedOrder` typu.

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/OwnedEntities/DetailedOrder.cs?name=DetailedOrder)]

[!code-csharp[OrderStatus](../../../samples/core/Modeling/OwnedEntities/OrderStatus.cs?name=OrderStatus)]

Kromě vnořené typy vlastnictví vlastní typ, který může odkazovat regulární entity, vlastní entity je na závislé straně může být vlastníkem nebo jiné entity. Tato možnost nastaví v EF6 vlastněné typy entit kromě komplexní typy.

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

Je možné řetězec `OwnsOne` metoda fluent volání ke konfiguraci tohoto modelu:

[!code-csharp[OwnsOneNested](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneNested)]

Je také možné dosáhnout stejnou věc, kterou pomocí `OwnedAttribute` u obou `OrderDetails` a `StreetAdress`.

## <a name="storing-owned-types-in-separate-tables"></a>Ukládání vlastní typy v samostatných tabulkách

Na rozdíl od komplexní typy EF6, také mohou být vlastněné typy uloženy do samostatné tabulky od vlastníka. Aby bylo možné přepsat vytváření názvů, který mapuje typ vlastnictví na stejnou tabulku jako vlastník, můžete jednoduše zavoláte `ToTable` a zadejte jiný název tabulky. Následující příklad provede mapování `OrderDetails` a jeho dvou adres do samostatné tabulky z `DetailedOrder`:

[!code-csharp[OwnsOneTable](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneTable)]

## <a name="querying-owned-types"></a>Zjišťují se vlastněné typy

Při dotazování na vlastníka vlastněné typy budou zahrnuty ve výchozím nastavení. Není nutné používat `Include` metoda, i když vlastněné typy jsou uložené v samostatné tabulce. Na základě modelu před popsané, následující dotaz zobrazí `Order`, `OrderDetails` a dva vlastní `StreetAddresses` z databáze:

[!code-csharp[DetailedOrderQuery](../../../samples/core/Modeling/OwnedEntities/Program.cs?name=DetailedOrderQuery)]

## <a name="limitations"></a>Omezení

Některé z těchto omezení jsou základem pro jak vlastněné pracovní typy entit, ale jiná omezení, že můžeme být schopen odebrat v budoucích verzích jsou:

### <a name="by-design-restrictions"></a>Omezení podle návrhu
- Nelze vytvořit `DbSet<T>` pro typ vlastnictví
- Nejde volat `Entity<T>()` s typem vlastnictví na `ModelBuilder`

### <a name="current-shortcomings"></a>Aktuální nedostatky
- Hierarchie dědičnosti, které zahrnují vlastněné typy entit nejsou podporovány.
- Navigační odkaz pro vlastní typy entit nemůže mít hodnotu null, pokud jsou explicitně namapované na samostatnou tabulku od vlastníka
- Instance vlastněné typy entit nemůže je sdílet více vlastníky (to je dobře známé scénář pro hodnotu objekty, které nelze implementovat s využitím vlastněné typy entit)

### <a name="shortcomings-in-previous-versions"></a>Nedostatky v předchozích verzích
- V EF Core 2.0 navigaci na, který vlastní typy entit se nedá deklarovat v typy odvozené entit, pokud vlastnictví entity jsou explicitně namapovány na samostatnou tabulku z hierarchie vlastníka. Toto omezení byl odebrán v EF Core 2.1
- V EF Core 2.0 a 2.1 pouze referenční podporovaly navigaci na vlastní typy. Toto omezení byl odebrán v EF Core 2.2
