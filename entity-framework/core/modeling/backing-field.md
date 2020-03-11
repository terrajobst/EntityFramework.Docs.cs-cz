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
# <a name="backing-fields"></a><span data-ttu-id="78b6c-102">Pomocná pole</span><span class="sxs-lookup"><span data-stu-id="78b6c-102">Backing Fields</span></span>

<span data-ttu-id="78b6c-103">Zálohovací pole umožňují EF čtení a zápis do pole, nikoli jako vlastnost.</span><span class="sxs-lookup"><span data-stu-id="78b6c-103">Backing fields allow EF to read and/or write to a field rather than a property.</span></span> <span data-ttu-id="78b6c-104">To může být užitečné, pokud je zapouzdření ve třídě používáno k omezení použití nebo rozšíření sémantiky pro přístup k datům pomocí kódu aplikace, ale tato hodnota by měla být načtena z databáze nebo zapsána do databáze bez použití těchto omezení nebo vylepšení.</span><span class="sxs-lookup"><span data-stu-id="78b6c-104">This can be useful when encapsulation in the class is being used to restrict the use of and/or enhance the semantics around access to the data by application code, but the value should be read from and/or written to the database without using those restrictions/enhancements.</span></span>

## <a name="basic-configuration"></a><span data-ttu-id="78b6c-105">Základní konfigurace</span><span class="sxs-lookup"><span data-stu-id="78b6c-105">Basic configuration</span></span>

<span data-ttu-id="78b6c-106">Podle konvence se pro danou vlastnost (uvedená v pořadí podle priority) zobrazí následující pole jako zálohovaná pole.</span><span class="sxs-lookup"><span data-stu-id="78b6c-106">By convention, the following fields will be discovered as backing fields for a given property (listed in precedence order).</span></span> 

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

<span data-ttu-id="78b6c-107">V následující ukázce je vlastnost `Url` nakonfigurovaná tak, aby měla `_url` jako své pole pro zálohování:</span><span class="sxs-lookup"><span data-stu-id="78b6c-107">In the following sample, the `Url` property is configured to have `_url` as its backing field:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/BackingField.cs#Sample)]

<span data-ttu-id="78b6c-108">Všimněte si, že pole pro zálohování jsou zjištěna pouze pro vlastnosti, které jsou součástí modelu.</span><span class="sxs-lookup"><span data-stu-id="78b6c-108">Note that backing fields are only discovered for properties that are included in the model.</span></span> <span data-ttu-id="78b6c-109">Další informace o tom, které vlastnosti jsou zahrnuty v modelu, najdete v tématu [zahrnutí & s výjimkou vlastností](included-properties.md).</span><span class="sxs-lookup"><span data-stu-id="78b6c-109">For more information on which properties are included in the model, see [Including & Excluding Properties](included-properties.md).</span></span>

<span data-ttu-id="78b6c-110">Můžete také nakonfigurovat pole pro zálohování explicitně, např. Pokud název pole neodpovídá výše uvedeným konvencím:</span><span class="sxs-lookup"><span data-stu-id="78b6c-110">You can also configure backing fields explicitly, e.g. if the field name doesn't correspond to the above conventions:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingField.cs?name=BackingField&highlight=5)]

## <a name="field-and-property-access"></a><span data-ttu-id="78b6c-111">Přístup k polím a vlastnostem</span><span class="sxs-lookup"><span data-stu-id="78b6c-111">Field and property access</span></span>

<span data-ttu-id="78b6c-112">Ve výchozím nastavení bude EF vždy číst a zapisovat do pole zálohování – za předpokladu, že je správně nakonfigurovaný – a nikdy vlastnost nepoužije.</span><span class="sxs-lookup"><span data-stu-id="78b6c-112">By default, EF will always read and write to the backing field - assuming one has been properly configured - and will never use the property.</span></span> <span data-ttu-id="78b6c-113">EF ale taky podporuje i další vzory přístupu.</span><span class="sxs-lookup"><span data-stu-id="78b6c-113">However, EF also supports other access patterns.</span></span> <span data-ttu-id="78b6c-114">Například následující příklad dá pokyn EF k zápisu do pole zálohování pouze v době, kdy je vyhodnocování, a pro použití vlastnosti ve všech ostatních případech:</span><span class="sxs-lookup"><span data-stu-id="78b6c-114">For example, the following sample instructs EF to write to the backing field only while materializing, and to use the property in all other cases:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldAccessMode.cs?name=BackingFieldAccessMode&highlight=6)]

<span data-ttu-id="78b6c-115">Kompletní sadu podporovaných možností najdete v tématu [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) .</span><span class="sxs-lookup"><span data-stu-id="78b6c-115">See the [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) for the complete set of supported options.</span></span>

> [!NOTE]
> <span data-ttu-id="78b6c-116">U EF Core 3,0 se výchozí režim přístupu k vlastnostem změnil z `PreferFieldDuringConstruction` na `PreferField`.</span><span class="sxs-lookup"><span data-stu-id="78b6c-116">With EF Core 3.0, the default property access mode changed from `PreferFieldDuringConstruction` to `PreferField`.</span></span>

## <a name="field-only-properties"></a><span data-ttu-id="78b6c-117">Vlastnosti pouze polí</span><span class="sxs-lookup"><span data-stu-id="78b6c-117">Field-only properties</span></span>

<span data-ttu-id="78b6c-118">V modelu můžete také vytvořit koncepční vlastnost, která nemá odpovídající vlastnost CLR v třídě entity, ale místo toho používá pole k uložení dat v entitě.</span><span class="sxs-lookup"><span data-stu-id="78b6c-118">You can also create a conceptual property in your model that does not have a corresponding CLR property in the entity class, but instead uses a field to store the data in the entity.</span></span> <span data-ttu-id="78b6c-119">To se liší od [vlastností stín](shadow-properties.md), kde jsou data uložená v sledování změn, nikoli v typu CLR entity.</span><span class="sxs-lookup"><span data-stu-id="78b6c-119">This is different from [Shadow Properties](shadow-properties.md), where the data is stored in the change tracker, rather than in the entity's CLR type.</span></span> <span data-ttu-id="78b6c-120">Vlastnosti pouze pole se obvykle používají, pokud třída entity používá metody, nikoli vlastnosti pro získání nebo nastavení hodnot, nebo v případech, kdy by pole neměly být vystavena vůbec v doménovém modelu (např. primární klíče).</span><span class="sxs-lookup"><span data-stu-id="78b6c-120">Field-only properties are commonly used when the entity class uses methods instead of properties to get/set values, or in cases where fields shouldn't be exposed at all in the domain model (e.g. primary keys).</span></span>

<span data-ttu-id="78b6c-121">Vlastnost pouze pro pole můžete nakonfigurovat zadáním názvu v rozhraní `Property(...)` API:</span><span class="sxs-lookup"><span data-stu-id="78b6c-121">You can configure a field-only property by providing a name in the `Property(...)` API:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldNoProperty.cs#Sample)]

<span data-ttu-id="78b6c-122">EF se pokusí najít vlastnost CLR se zadaným názvem nebo pole, pokud vlastnost nebyla nalezena.</span><span class="sxs-lookup"><span data-stu-id="78b6c-122">EF will attempt to find a CLR property with the given name, or a field if a property isn't found.</span></span> <span data-ttu-id="78b6c-123">Pokud není nalezena vlastnost ani pole, bude místo toho nastavena vlastnost Shadow.</span><span class="sxs-lookup"><span data-stu-id="78b6c-123">If neither a property nor a field are found, a shadow property will be set up instead.</span></span>

<span data-ttu-id="78b6c-124">Možná budete muset odkazovat na vlastnost jenom pro pole z dotazů LINQ, ale tato pole jsou obvykle soukromá.</span><span class="sxs-lookup"><span data-stu-id="78b6c-124">You may need to refer to a field-only property from LINQ queries, but such fields are typically private.</span></span> <span data-ttu-id="78b6c-125">Můžete použít metodu `EF.Property(...)` v dotazu LINQ pro odkazování na pole:</span><span class="sxs-lookup"><span data-stu-id="78b6c-125">You can use the `EF.Property(...)` method in a LINQ query to refer to the field:</span></span>

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "_validatedUrl"));
```
