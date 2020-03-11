---
title: Pole pro zálohování – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: 20cf9dc9b0d556f29680bce588bcbdc4ea48fa74
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417307"
---
# <a name="backing-fields"></a>Pomocná pole

Zálohovací pole umožňují EF čtení a zápis do pole, nikoli jako vlastnost. To může být užitečné, pokud je zapouzdření ve třídě používáno k omezení použití nebo rozšíření sémantiky pro přístup k datům pomocí kódu aplikace, ale tato hodnota by měla být načtena z databáze nebo zapsána do databáze bez použití těchto omezení nebo vylepšení.

## <a name="basic-configuration"></a>Základní konfigurace

Podle konvence se pro danou vlastnost (uvedená v pořadí podle priority) zobrazí následující pole jako zálohovaná pole. 

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

V následující ukázce je vlastnost `Url` nakonfigurovaná tak, aby měla `_url` jako své pole pro zálohování:

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/BackingField.cs#Sample)]

Všimněte si, že pole pro zálohování jsou zjištěna pouze pro vlastnosti, které jsou součástí modelu. Další informace o tom, které vlastnosti jsou zahrnuty v modelu, najdete v tématu [zahrnutí & s výjimkou vlastností](included-properties.md).

Můžete také nakonfigurovat pole pro zálohování explicitně, např. Pokud název pole neodpovídá výše uvedeným konvencím:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingField.cs?name=BackingField&highlight=5)]

## <a name="field-and-property-access"></a>Přístup k polím a vlastnostem

Ve výchozím nastavení bude EF vždy číst a zapisovat do pole zálohování – za předpokladu, že je správně nakonfigurovaný – a nikdy vlastnost nepoužije. EF ale taky podporuje i další vzory přístupu. Například následující příklad dá pokyn EF k zápisu do pole zálohování pouze v době, kdy je vyhodnocování, a pro použití vlastnosti ve všech ostatních případech:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldAccessMode.cs?name=BackingFieldAccessMode&highlight=6)]

Kompletní sadu podporovaných možností najdete v tématu [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) .

> [!NOTE]
> U EF Core 3,0 se výchozí režim přístupu k vlastnostem změnil z `PreferFieldDuringConstruction` na `PreferField`.

## <a name="field-only-properties"></a>Vlastnosti pouze polí

V modelu můžete také vytvořit koncepční vlastnost, která nemá odpovídající vlastnost CLR v třídě entity, ale místo toho používá pole k uložení dat v entitě. To se liší od [vlastností stín](shadow-properties.md), kde jsou data uložená v sledování změn, nikoli v typu CLR entity. Vlastnosti pouze pole se obvykle používají, pokud třída entity používá metody, nikoli vlastnosti pro získání nebo nastavení hodnot, nebo v případech, kdy by pole neměly být vystavena vůbec v doménovém modelu (např. primární klíče).

Vlastnost pouze pro pole můžete nakonfigurovat zadáním názvu v rozhraní `Property(...)` API:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldNoProperty.cs#Sample)]

EF se pokusí najít vlastnost CLR se zadaným názvem nebo pole, pokud vlastnost nebyla nalezena. Pokud není nalezena vlastnost ani pole, bude místo toho nastavena vlastnost Shadow.

Možná budete muset odkazovat na vlastnost jenom pro pole z dotazů LINQ, ale tato pole jsou obvykle soukromá. Můžete použít metodu `EF.Property(...)` v dotazu LINQ pro odkazování na pole:

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "_validatedUrl"));
```
