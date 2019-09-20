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
# <a name="owned-entity-types"></a>Vlastněné typy entit

>[!NOTE]
> Tato funkce je v EF Core 2,0 novinkou.

EF Core umožňuje modelovat typy entit, které se mohou u vlastností navigace u jiných typů entit vyskytovat pouze někdy. Ty se nazývají _vlastněné typy entit_. Entita obsahující typ entity, která je vlastníkem, je jeho _vlastník_.

Vlastněné entity jsou v podstatě součástí vlastníka a nemůže existovat bez nich, jsou koncepčně podobné [agregacím](https://martinfowler.com/bliki/DDD_Aggregate.html).

## <a name="explicit-configuration"></a>Explicitní konfigurace

Vlastní typy entit nejsou nikdy zahrnuty EF Core v modelu podle konvence. Můžete použít `OwnsOne` metodu v `OnModelCreating` nebo `OwnedAttribute` opatřit poznámkami k typu (novinkou v EF Core 2,1) ke konfiguraci typu jako vlastněný typ.

V tomto příkladu `StreetAddress` je typ bez vlastnosti identity. Slouží jako vlastnost typu objednávky k určení dodací adresy pro konkrétní objednávku.

V případě, že `OwnedAttribute` je odkazováno z jiného typu entity, můžeme použít k tomu, aby ho považoval jako vlastněné entity:

[!code-csharp[StreetAddress](../../../samples/core/Modeling/OwnedEntities/StreetAddress.cs?name=StreetAddress)]

[!code-csharp[Order](../../../samples/core/Modeling/OwnedEntities/Order.cs?name=Order)]

Je `OwnsOne` také možné použít metodu v `OnModelCreating` k určení toho, že vlastnost je `ShippingAddress` vlastněná entita `Order` typu entity a v případě potřeby konfiguruje další omezující vlastnosti.

[!code-csharp[OwnsOne](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOne)]

Pokud je `Order`vlastnostsoukromá v typu, můžete použít řetězcovou verzi `OwnsOne` metody: `ShippingAddress`

[!code-csharp[OwnsOneString](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneString)]

Podívejte se na [úplný ukázkový projekt](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) pro další kontext. 

## <a name="implicit-keys"></a>Implicitní klíče

Vlastněné typy nakonfigurované `OwnsOne` nebo zjištěné prostřednictvím navigační navigace mají vždy relaci 1:1 s vlastníkem, proto nepotřebují vlastní hodnoty klíče, protože hodnoty cizích klíčů jsou jedinečné. V předchozím příkladu `StreetAddress` typ nepotřebuje definovat klíčovou vlastnost.  

Aby bylo možné pochopit, jak EF Core tyto objekty sledovat, je užitečné vědět, že je primární klíč vytvořen jako [vlastnost Shadow](xref:core/modeling/shadow-properties) pro vlastněný typ. Hodnota klíče instance typu, který je vlastníkem, bude stejná jako hodnota klíče instance vlastníka.

## <a name="collections-of-owned-types"></a>Kolekce vlastněných typů

> [!NOTE]
> Tato funkce je v EF Core 2,2 novinkou.

Chcete-li konfigurovat kolekci vlastněných typů `OwnsMany` , `OnModelCreating`použijte v.

Vlastněné typy potřebují primární klíč. Pokud v typu .NET nejsou k dispozici žádné dobré vlastnosti kandidátů, EF Core se může pokusit vytvořit. Pokud jsou však vlastní typy definovány prostřednictvím kolekce, není třeba pouze vytvořit vlastnost Shadow, která bude fungovat jako cizí klíč, a primární klíč vlastněné instance, jak to máme: pro `OwnsOne`je možné použít více vlastněných instancí typu pro Každý vlastník, a proto klíč vlastníka nestačí poskytnout jedinečnou identitu pro každou vlastní instanci.

Mezi ně patří tyto dvě nejjednodušší řešení:
- Definování náhradního primárního klíče pro novou vlastnost nezávisle na cizím klíči, který odkazuje na vlastníka. Obsažené hodnoty by musely být jedinečné ve všech vlastníkech (např. Pokud nadřazený {1} objekt má {1}podřízenou {2} {1}položku), takže tato hodnota nemá žádný z těchto významů. Vzhledem k tomu, že cizí klíč není součástí primárního klíče, lze jeho hodnoty změnit, takže byste mohli přesunout podřízenou položku z jedné nadřazené položky do druhé, ale obvykle to vzroste s sémantikou agregace.
- Použití cizího klíče a další vlastnosti jako složeného klíče. Hodnota další vlastnosti teď musí být pro {1} danou nadřazenou položku jedinečná (takže pokud nadřazený objekt {1,1} má {2} podřízenou položku, může mít podřízený objekt {2,1}i podřízenou položku). Vytvořením části cizího klíče v primárním klíči se vztah mezi vlastníkem a vlastněnou entitou stane neproměnlivým a bude lépe odrážet agregovanou sémantiku. To je to, co EF Core ve výchozím nastavení.

V tomto příkladu použijeme `Distributor` třídu:

[!code-csharp[Distributor](../../../samples/core/Modeling/OwnedEntities/Distributor.cs?name=Distributor)]

Ve výchozím nastavení `("DistributorId", "Id")` bude primární klíč použitý pro daný typ, na který `ShippingCenters` odkazuje vlastnost navigace, v `"DistributorId"` případě, kde je `"Id"` hodnota FK a `int` je jedinečná.

Konfigurace jiného volání `HasKey`PK:

[!code-csharp[OwnsMany](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsMany)]

> [!NOTE]
> Předtím, než `WithOwner()` metoda EF Core 3,0 neexistuje, by mělo být toto volání odebráno.

## <a name="mapping-owned-types-with-table-splitting"></a>Mapování vlastněných typů s dělením tabulky

Při použití relačních databází jsou ve výchozím nastavení vlastní typy odkazů namapovány na stejnou tabulku jako vlastník. To vyžaduje rozdělení tabulky ve dvou případech: některé sloupce budou použity k uložení dat vlastníka a některé sloupce budou použity k uložení dat vlastněné entity. Jedná se o běžnou funkci známou jako [rozdělení tabulky](table-splitting.md).

Ve výchozím nastavení EF Core pojmenuje sloupce databáze pro vlastnosti typu entity, které patří do vzoru _Navigation_OwnedEntityProperty_. Proto se `StreetAddress` vlastnosti zobrazí v tabulce Orders s názvy ' ShippingAddress_Street ' a ' ShippingAddress_City '.

K přejmenování těchto sloupců `HasColumnName` lze použít metodu:

[!code-csharp[ColumnNames](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=ColumnNames)]

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a>Sdílení stejného typu .NET mezi více vlastněných typů

Vlastněný typ entity může být stejného typu .NET jako jiný vlastněný typ entity, proto typ .NET nemusí být dostatečně k identifikaci vlastněné typu.

V těchto případech se vlastnost ukazující z vlastníka na vlastní entitu staly _definováním navigace_ typu vlastněné entity. Z perspektivy EF Core je definování navigace součástí identity typu spolu s typem .NET.   

Například v následující třídě `ShippingAddress` a `BillingAddress` jsou oba stejného typu `StreetAddress`.NET:

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

Aby bylo možné pochopit, jak EF Core odlišuje sledované instance těchto objektů, může být užitečné vzít v úvahu, že definující navigace se stane součástí klíče instance spolu s hodnotou klíče vlastníka a typu .NET vlastněných typů.

## <a name="nested-owned-types"></a>Vnořené vlastní typy

V tomto příkladu `OrderDetails` vlastní `BillingAddress` a `ShippingAddress`, které jsou oba `StreetAddress` typy. Pak `OrderDetails` vlastní`DetailedOrder` typ.

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/OwnedEntities/DetailedOrder.cs?name=DetailedOrder)]

[!code-csharp[OrderStatus](../../../samples/core/Modeling/OwnedEntities/OrderStatus.cs?name=OrderStatus)]

Kromě vnořených vlastněných typů může vlastněný typ odkazovat na běžnou entitu, může to být buď vlastník, nebo jiná entita, pokud je vlastněná entita na závislé straně. Tato schopnost nastavuje vlastní typy entit od složitých typů v EF6.

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

Je možné zřetězit `OwnsOne` metodu v rámci volání Fluent pro konfiguraci tohoto modelu:

[!code-csharp[OwnsOneNested](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneNested)]

Všimněte si `WithOwner` volání, které se používá ke konfiguraci navigační vlastnosti odkazující zpátky na vlastníka.

Výsledek lze dosáhnout pomocí `OwnedAttribute` obou `OrderDetails` i `StreetAdress`.

## <a name="storing-owned-types-in-separate-tables"></a>Ukládání vlastněných typů do samostatných tabulek

I na rozdíl od EF6 složitých typů mohou být vlastněné typy uloženy v samostatné tabulce od vlastníka. Aby bylo možné přepsat konvenci, která mapuje vlastní typ na stejnou tabulku jako vlastník, můžete jednoduše zavolat `ToTable` a zadat jiný název tabulky. Následující příklad bude mapovat `OrderDetails` a jeho dvě adresy na samostatnou tabulku z: `DetailedOrder`

[!code-csharp[OwnsOneTable](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneTable)]

## <a name="querying-owned-types"></a>Dotazování na vlastní typy

Při dotazování vlastníka budou ve výchozím nastavení zahrnuty vlastněné typy. Není nutné používat `Include` metodu, a to ani v případě, že vlastní typy jsou uloženy v samostatné tabulce. V závislosti na modelu popsaném dříve bude následující dotaz získávat `Order` `OrderDetails` a dvě vlastněná `StreetAddresses` z databáze:

[!code-csharp[DetailedOrderQuery](../../../samples/core/Modeling/OwnedEntities/Program.cs?name=DetailedOrderQuery)]

## <a name="limitations"></a>Omezení

Některá z těchto omezení jsou zásadní pro to, jak vlastní typy entit fungují, ale některé jiné jsou omezení, která v budoucích verzích může být možné odebrat:

### <a name="by-design-restrictions"></a>Omezení podle návrhu
- Nemůžete vytvořit `DbSet<T>` pro vlastněný typ.
- Nelze volat `Entity<T>()` s vlastníkem typu na`ModelBuilder`

### <a name="current-shortcomings"></a>Aktuální nedostatky
- Hierarchie dědičnosti, které zahrnují vlastní typy entit, se nepodporují.
- Referenční navigace na vlastní typy entit nemůžou mít hodnotu null, pokud nejsou explicitně namapované na samostatnou tabulku od vlastníka.
- Instance typů vlastněných entit nemůže sdílet více vlastníků (Toto je známý scénář pro objekty hodnot, které není možné implementovat pomocí vlastněných typů entit).

### <a name="shortcomings-in-previous-versions"></a>Nedostatky v předchozích verzích
- V EF Core 2,0 nemůže být navigace na vlastní typy entit deklarována v odvozených typech entit, pokud vlastněné entity nejsou explicitně mapovány na samostatnou tabulku z hierarchie Owner. Toto omezení se odebralo v EF Core 2,1.
- V EF Core 2,0 a 2,1 jsou podporovány pouze navigační odkazy na vlastní typy. Toto omezení se odebralo v EF Core 2,2.
