---
title: Pomocná pole – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: 79221b6f7968675ff10f80d5df181b674b6a20c9
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994090"
---
# <a name="backing-fields"></a>Pomocná pole

> [!NOTE]  
> Tato funkce je nového v EF Core 1.1.

Základní pole umožňují EF číst nebo zapisovat do pole, nikoli vlastnosti. To může být užitečné, když zapouzdření ve třídě se používá k omezení použití a/nebo vylepšit sémantiku týkající se přístupu k datům kódem aplikace, ale hodnota by měla být číst z a/nebo zapsána do databáze bez použití těchto omezení / vylepšení.

## <a name="conventions"></a>Konvence

Podle konvence bude zjištěno následující pole jako pomocná pole pro dané vlastnosti (uvedené v pořadí priority). Pole budou zjišťována pouze pro vlastnosti, které jsou součástí modelu. Další informace, na které vlastnosti jsou součástí modelu najdete v tématu [zahrnutí a vyloučení vlastností](included-properties.md).

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/BackingField.cs#Sample)]

Pokud je nakonfigurovaná pomocným polem, EF bude zapisovat přímo do tohoto pole při materializaci instancí entit z databáze (spíš než setter vlastnosti). Pokud EF potřebuje číst nebo zapisovat hodnoty v jinou dobu, použije vlastnost Pokud je to možné. Například pokud EF je potřeba aktualizovat hodnotu pro vlastnost, použije tento zdroj vlastnosti je-li definován. Pokud vlastnost je jen pro čtení, bude zapisovat do příslušného pole.

## <a name="data-annotations"></a>Datové poznámky

Pomocná pole nelze konfigurovat pomocí datových poznámek.

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít ke konfiguraci pomocné pole pro vlastnost.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a>Řízení při použití pole

Můžete nakonfigurovat při EF používá pole nebo vlastnost. Zobrazit [PropertyAccessMode výčtu](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) podporované možnosti.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a>Pole bez vlastnosti

Můžete také vytvořit konceptuální vlastnost v modelu, který nemá odpovídající vlastnost CLR ve třídě entity, ale místo toho používá pole k uložení dat v dané entitě. Tím se liší od [stínové vlastnosti](shadow-properties.md), kde jsou data uložená v nástroji Sledování změn. To by obvykle použijí, pokud třída entity pomocí metody get/set hodnoty.

EF můžete dát název pole v `Property(...)` rozhraní API. Pokud není žádná vlastnost s daným názvem, bude vypadat EF pro pole.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldNoProperty.cs#Sample)]

Můžete také poskytnout vlastnost názvu než názvu pole. Tento název se použije při vytváření modelu, zejména se použije pro název sloupce, který je namapovaný na v databázi.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldConceptualProperty.cs#Sample)]

Pokud není žádná vlastnost ve třídě entity, můžete použít `EF.Property(...)` metoda v dotazu LINQ k odkazování na vlastnost, která je koncepčně součástí modelu.

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
