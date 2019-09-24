---
title: Pole pro zálohování – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: c3ca8bb97992c192672e8c2f2040b0de029df68d
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197478"
---
# <a name="backing-fields"></a>Pomocná pole

> [!NOTE]  
> Tato funkce je v EF Core 1,1 novinkou.

Zálohovací pole umožňují EF čtení a zápis do pole, nikoli jako vlastnost. To může být užitečné, pokud je zapouzdření ve třídě používáno k omezení použití nebo rozšíření sémantiky pro přístup k datům pomocí kódu aplikace, ale tato hodnota by měla být načtena z databáze nebo zapsána do databáze bez použití těchto omezení/ prvky.

## <a name="conventions"></a>Konvence

Podle konvence se pro danou vlastnost (uvedená v pořadí podle priority) zobrazí následující pole jako zálohovaná pole. Pole jsou zjištěna pouze pro vlastnosti, které jsou zahrnuty v modelu. Další informace o tom, které vlastnosti jsou zahrnuty v modelu, najdete v tématu [zahrnutí & s výjimkou vlastností](included-properties.md).

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/BackingField.cs#Sample)]

Pokud je nakonfigurováno zálohovací pole, EF přepíše přímo do tohoto pole při vyhodnocování instancí entit z databáze (místo použití vlastnosti setter). Pokud EF potřebuje číst nebo zapsat hodnotu v jinou dobu, bude tuto vlastnost používat, pokud je to možné. Například pokud EF potřebuje aktualizovat hodnotu pro vlastnost, použije vlastnost setter, pokud je definována. Je-li vlastnost určena pouze pro čtení, bude zapsána do pole.

## <a name="data-annotations"></a>Datové poznámky

U zálohovaných polí nelze konfigurovat datové poznámky.

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít ke konfiguraci pole pro zálohování pro vlastnost.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a>Řízení při použití pole

Můžete nakonfigurovat, kdy EF používá pole nebo vlastnost. Podporované možnosti najdete ve [výčtu PropertyAccessMode](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) .

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a>Pole bez vlastnosti

V modelu můžete také vytvořit koncepční vlastnost, která nemá odpovídající vlastnost CLR v třídě entity, ale místo toho používá pole k uložení dat v entitě. To se liší od [vlastností stín](shadow-properties.md), kde jsou data uložená v sledování změn. To se obvykle používá, pokud třída entity používá metody k získání nebo nastavení hodnot.

EF můžete zadat název pole v `Property(...)` rozhraní API. Pokud neexistuje žádná vlastnost se zadaným názvem, pak bude v EF Hledat pole.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldNoProperty.cs#Sample)]

Můžete také zvolit, že chcete vlastnosti zadat jiný název než název pole. Tento název se pak použije při vytváření modelu, hlavně se použije pro název sloupce, který je namapovaný na v databázi.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldConceptualProperty.cs#Sample)]

Pokud třída entity neobsahuje žádnou vlastnost, lze použít `EF.Property(...)` metodu v dotazu LINQ pro odkaz na vlastnost, která je koncepčně součástí modelu.

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
