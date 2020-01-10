---
title: Vlastní typy entit – EF Core
description: Jak nakonfigurovat vlastní typy entit nebo agregace při použití Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/owned-entities
ms.openlocfilehash: 30b91b6e66b6c0f516d1ba12485304b52770cbef
ms.sourcegitcommit: 4e86f01740e407ff25e704a11b1f7d7e66bfb2a6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/09/2020
ms.locfileid: "75781232"
---
# <a name="owned-entity-types"></a>Vlastněné typy entit

EF Core umožňuje modelovat typy entit, které se mohou u vlastností navigace u jiných typů entit vyskytovat pouze někdy. Ty se nazývají _vlastněné typy entit_. Entita obsahující typ entity, která je vlastníkem, je jeho _vlastník_.

Vlastněné entity jsou v podstatě součástí vlastníka a nemůže existovat bez nich, jsou koncepčně podobné [agregacím](https://martinfowler.com/bliki/DDD_Aggregate.html). To znamená, že vlastněný typ je podle definice na závislé straně vztahu s vlastníkem.

## <a name="explicit-configuration"></a>Explicitní konfigurace

Vlastní typy entit nejsou nikdy zahrnuty EF Core v modelu podle konvence. Můžete použít metodu `OwnsOne` v `OnModelCreating` nebo opatřit poznámkami typ pomocí `OwnedAttribute` (novinka v EF Core 2,1) ke konfiguraci typu jako vlastněný typ.

V tomto příkladu je `StreetAddress` typ bez vlastnosti identity. Slouží jako vlastnost typu objednávky k určení dodací adresy pro konkrétní objednávku.

V případě, že je odkazováno z jiného typu entity, můžeme použít `OwnedAttribute` k jeho zakládání jako k entitě vlastněné:

[!code-csharp[StreetAddress](../../../samples/core/Modeling/OwnedEntities/StreetAddress.cs?name=StreetAddress)]

[!code-csharp[Order](../../../samples/core/Modeling/OwnedEntities/Order.cs?name=Order)]

Je také možné použít metodu `OwnsOne` v `OnModelCreating` k určení toho, že vlastnost `ShippingAddress` je vlastněnou entitou typu entity `Order` a v případě potřeby konfiguraci dalších omezujících vlastností.

[!code-csharp[OwnsOne](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOne)]

Pokud je vlastnost `ShippingAddress` soukromá v typu `Order`, můžete použít řetězcovou verzi `OwnsOne` metody:

[!code-csharp[OwnsOneString](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneString)]

Podívejte se na [úplný ukázkový projekt](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) pro další kontext.

## <a name="implicit-keys"></a>Implicitní klíče

Vlastněné typy nakonfigurované pomocí `OwnsOne` nebo zjištěné prostřednictvím navigační navigace mají vždy relaci 1:1 s vlastníkem, proto nepotřebují vlastní hodnoty klíče, protože hodnoty cizích klíčů jsou jedinečné. V předchozím příkladu `StreetAddress` typ nepotřebuje definovat klíčovou vlastnost.  

Aby bylo možné pochopit, jak EF Core tyto objekty sledovat, je užitečné vědět, že je primární klíč vytvořen jako [vlastnost Shadow](xref:core/modeling/shadow-properties) pro vlastněný typ. Hodnota klíče instance typu, který je vlastníkem, bude stejná jako hodnota klíče instance vlastníka.

## <a name="collections-of-owned-types"></a>Kolekce vlastněných typů

> [!NOTE]
> Tato funkce je v EF Core 2,2 novinkou.

Pro konfiguraci kolekce vlastněných typů použijte `OwnsMany` v `OnModelCreating`.

Vlastněné typy potřebují primární klíč. Pokud v typu .NET nejsou k dispozici žádné dobré vlastnosti kandidátů, EF Core se může pokusit vytvořit. Pokud jsou však vlastní typy definovány prostřednictvím kolekce, není třeba pouze vytvořit vlastnost Shadow, která bude fungovat jako cizí klíč, a primární klíč vlastněné instance, jak je určeno pro `OwnsOne`: pro každého vlastníka může existovat více vlastněných instancí typu, a proto klíč vlastníka není dostatečný, aby poskytoval jedinečnou identitu pro každou vlastní instanci.

Mezi ně patří tyto dvě nejjednodušší řešení:

- Definování náhradního primárního klíče pro novou vlastnost nezávisle na cizím klíči, který odkazuje na vlastníka. Obsažené hodnoty by musely být jedinečné ve všech vlastníkech (např. Pokud nadřazený {1} má podřízený {1}, pak nadřazený {2} nemůže mít podřízený {1}), takže hodnota nemá žádný z těchto významů. Vzhledem k tomu, že cizí klíč není součástí primárního klíče, lze jeho hodnoty změnit, takže byste mohli přesunout podřízenou položku z jedné nadřazené položky do druhé, ale obvykle to vzroste s sémantikou agregace.
- Použití cizího klíče a další vlastnosti jako složeného klíče. Hodnota další vlastnosti teď musí být pro danou nadřazenou položku jedinečná (takže pokud nadřazený {1} má podřízenou položku {1,1} pak nadřazený {2} může mít podřízený {2,1}). Vytvořením části cizího klíče v primárním klíči se vztah mezi vlastníkem a vlastněnou entitou stane neproměnlivým a bude lépe odrážet agregovanou sémantiku. To je to, co EF Core ve výchozím nastavení.

V tomto příkladu použijeme třídu `Distributor`:

[!code-csharp[Distributor](../../../samples/core/Modeling/OwnedEntities/Distributor.cs?name=Distributor)]

Ve výchozím nastavení se primární klíč použitý pro typ, na který se odkazuje pomocí navigační vlastnosti `ShippingCenters`, `("DistributorId", "Id")`, kde `"DistributorId"` je FK a `"Id"` je jedinečná `int` hodnota.

Konfigurace jiného volání `HasKey`v rámci PK:

[!code-csharp[OwnsMany](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsMany)]

> [!NOTE]
> Před EF Core 3,0 `WithOwner()` metoda neexistovala, aby bylo toto volání odebráno. Také primární klíč nebyl zjištěn automaticky, takže byl vždy zadán.

## <a name="mapping-owned-types-with-table-splitting"></a>Mapování vlastněných typů s dělením tabulky

Při použití relačních databází jsou ve výchozím nastavení vlastní typy odkazů namapovány na stejnou tabulku jako vlastník. To vyžaduje rozdělení tabulky ve dvou případech: některé sloupce budou použity k uložení dat vlastníka a některé sloupce budou použity k uložení dat vlastněné entity. Jedná se o běžnou funkci známou jako [rozdělení tabulky](table-splitting.md).

Ve výchozím nastavení EF Core pojmenuje sloupce databáze pro vlastnosti typu entity, které jsou v rámci vzoru _Navigation_OwnedEntityProperty_. Proto se vlastnosti `StreetAddress` zobrazí v tabulce Orders s názvy "ShippingAddress_Street" a "ShippingAddress_City".

K přejmenování těchto sloupců můžete použít metodu `HasColumnName`:

[!code-csharp[ColumnNames](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=ColumnNames)]

> [!NOTE]
> Většinu běžných metod konfigurace typu entity, jako je například [Ignore](/dotnet/api/microsoft.entityframeworkcore.metadata.builders.ownednavigationbuilder.ignore) , lze volat stejným způsobem.

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a>Sdílení stejného typu .NET mezi více vlastněných typů

Vlastněný typ entity může být stejného typu .NET jako jiný vlastněný typ entity, proto typ .NET nemusí být dostatečně k identifikaci vlastněné typu.

V těchto případech se vlastnost ukazující z vlastníka na vlastní entitu staly _definováním navigace_ typu vlastněné entity. Z perspektivy EF Core je definování navigace součástí identity typu spolu s typem .NET.

Například v následující třídě `ShippingAddress` a `BillingAddress` jsou oba stejného typu .NET `StreetAddress`:

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

Aby bylo možné pochopit, jak EF Core odlišuje sledované instance těchto objektů, může být užitečné vzít v úvahu, že definující navigace se stane součástí klíče instance spolu s hodnotou klíče vlastníka a typu .NET vlastněných typů.

## <a name="nested-owned-types"></a>Vnořené vlastní typy

V tomto příkladu `OrderDetails` vlastní `BillingAddress` a `ShippingAddress`, které jsou oba `StreetAddress` typy. Pak `OrderDetails` vlastní `DetailedOrder` typ.

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/OwnedEntities/DetailedOrder.cs?name=DetailedOrder)]

[!code-csharp[OrderStatus](../../../samples/core/Modeling/OwnedEntities/OrderStatus.cs?name=OrderStatus)]

Každá navigace k vlastnímu typu definuje samostatný typ entity s zcela nezávislou konfigurací.

Kromě vnořených vlastněných typů může vlastněný typ odkazovat na běžnou entitu, může to být buď vlastník, nebo jiná entita, pokud je vlastněná entita na závislé straně. Tato schopnost nastavuje vlastní typy entit od složitých typů v EF6.

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

Je možné zřetězit metodu `OwnsOne` v volání Fluent pro konfiguraci tohoto modelu:

[!code-csharp[OwnsOneNested](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneNested)]

Všimněte si `WithOwner` volání, které slouží ke konfiguraci navigační vlastnosti odkazující zpět na vlastníka. Pro konfiguraci navigace na typ entity vlastníka, který není součástí vztahu vlastnictví `WithOwner()` by měl být volán bez argumentů.

Výsledek lze dosáhnout pomocí `OwnedAttribute` v `OrderDetails` i `StreetAddress`.

## <a name="storing-owned-types-in-separate-tables"></a>Ukládání vlastněných typů do samostatných tabulek

I na rozdíl od EF6 složitých typů mohou být vlastněné typy uloženy v samostatné tabulce od vlastníka. Aby bylo možné přepsat konvenci, která mapuje vlastněný typ na stejnou tabulku jako vlastník, můžete jednoduše volat `ToTable` a zadat jiný název tabulky. Následující příklad vytvoří mapování `OrderDetails` a jeho dvou adres na samostatnou tabulku z `DetailedOrder`:

[!code-csharp[OwnsOneTable](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneTable)]

Je také možné použít `TableAttribute` k tomuto účelu, ale Upozorňujeme, že to selže, pokud existuje více navigačních prvků stejného typu, protože v takovém případě je více typů entit namapováno na stejnou tabulku.

## <a name="querying-owned-types"></a>Dotazování na vlastní typy

Při dotazování vlastníka budou ve výchozím nastavení zahrnuty vlastněné typy. Není nutné používat metodu `Include`, ani když jsou vlastněné typy uloženy v samostatné tabulce. V závislosti na modelu popsaném dříve se následující dotaz zobrazí `Order``OrderDetails` a dva vlastněné `StreetAddresses` z databáze:

[!code-csharp[DetailedOrderQuery](../../../samples/core/Modeling/OwnedEntities/Program.cs?name=DetailedOrderQuery)]

## <a name="limitations"></a>Omezení

Některá z těchto omezení jsou zásadní pro to, jak vlastní typy entit fungují, ale některé jiné jsou omezení, která v budoucích verzích může být možné odebrat:

### <a name="by-design-restrictions"></a>Omezení podle návrhu

- Nemůžete vytvořit `DbSet<T>` pro vlastněný typ.
- `Entity<T>()` nelze volat s vlastněným typem v `ModelBuilder`

### <a name="current-shortcomings"></a>Aktuální nedostatky

- Vlastní typy entit nemůžou mít Hierarchie dědičnosti.
- Referenční navigace na vlastní typy entit nemůžou mít hodnotu null, pokud nejsou explicitně namapované na samostatnou tabulku od vlastníka.
- Instance typů vlastněných entit nemůže sdílet více vlastníků (Toto je známý scénář pro objekty hodnot, které není možné implementovat pomocí vlastněných typů entit).

### <a name="shortcomings-in-previous-versions"></a>Nedostatky v předchozích verzích

- V EF Core 2,0 nemůže být navigace na vlastní typy entit deklarována v odvozených typech entit, pokud vlastněné entity nejsou explicitně mapovány na samostatnou tabulku z hierarchie Owner. Toto omezení se odebralo v EF Core 2,1.
- V EF Core 2,0 a 2,1 jsou podporovány pouze navigační odkazy na vlastní typy. Toto omezení se odebralo v EF Core 2,2.
