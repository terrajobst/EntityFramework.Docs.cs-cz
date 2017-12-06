---
title: "Zálohování polí - EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
ms.technology: entity-framework-core
uid: core/modeling/backing-field
ms.openlocfilehash: e95417b3368d09a64f9ec02ae40c38dc770c2e6b
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/05/2017
---
# <a name="backing-fields"></a>Základní pole

> [!NOTE]  
> Tato funkce je nového v EF základní 1.1.

Základní pole Povolit EF číst nebo zapisovat do pole, nikoli vlastnost. To může být užitečné, když zapouzdření ve třídě, je používána pro omezit použití nebo vylepšit sémantiku kolem přístup k datům kód aplikace, ale hodnota by měla číst z a zapsána do databáze bez použití těchto omezení nebo vylepšení.

## <a name="conventions"></a>Konvence

Podle konvence budou zjištěny následující pole jako pole pro danou vlastnost (uvedené v pořadí priorit) zálohování. Pole budou zjišťována pouze pro vlastnosti, které jsou součástí modelu. Další informace, na kterém jsou vlastnosti součástí modelu najdete v tématu [včetně & vyloučeny vlastnosti](included-properties.md).

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/BackingField.cs#Sample)]

Pokud pole zálohování je nakonfigurován, EF bude zapisovat přímo do tohoto pole při vyhodnocování instancí entit z databáze (místo použití Metoda setter vlastnosti). Pokud EF potřebuje ke čtení nebo zápis hodnoty v jinou dobu, je-li to možné používat vlastnost. Například pokud EF potřebuje aktualizujte hodnotu pro vlastnost, použije metoda setter vlastnosti je-li definován. Pokud vlastnost je jen pro čtení, budou zapisovat do pole.

## <a name="data-annotations"></a>Datových poznámek

Zálohování pole nelze konfigurovat pomocí datových poznámek.

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít ke konfiguraci základní pole pro vlastnost.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a>Řízení, pokud se používá pole

Můžete nakonfigurovat při EF používá pole nebo vlastnost. Najdete v článku [PropertyAccessMode výčtu](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) podporované možnosti.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a>Pole bez vlastnost

Můžete také vytvořit konceptuální vlastnosti v daném modelu, který nemá odpovídající vlastnosti CLR ve třídě entity, ale místo toho používá pole k uložení dat v entitě. To se liší od [vlastnosti stínové](shadow-properties.md), kde jsou data uložena v nástroji Sledování změn. To by obvykle použijí, pokud třídy entita pomocí metody get nebo nastavení hodnoty.

Můžete udělit EF název v poli `Property(...)` rozhraní API. Pokud není žádná vlastnost s daným názvem, bude vypadat EF pro pole.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldNoProperty.cs#Sample)]

Můžete také poskytnout vlastnost název, jiný než název pole. Tento název se pak používá při vytváření modelu, zejména se použije pro název sloupce, který je namapovaný na v databázi.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldConceptualProperty.cs#Sample)]

Pokud není žádná vlastnost v třídě entity, můžete použít `EF.Property(...)` metoda v dotazu LINQ odkazovat na vlastnost, která je koncepčně součástí modelu.

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
