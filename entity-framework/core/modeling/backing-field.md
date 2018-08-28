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
# <a name="backing-fields"></a><span data-ttu-id="36508-102">Pomocná pole</span><span class="sxs-lookup"><span data-stu-id="36508-102">Backing Fields</span></span>

> [!NOTE]  
> <span data-ttu-id="36508-103">Tato funkce je nového v EF Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="36508-103">This feature is new in EF Core 1.1.</span></span>

<span data-ttu-id="36508-104">Základní pole umožňují EF číst nebo zapisovat do pole, nikoli vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="36508-104">Backing fields allow EF to read and/or write to a field rather than a property.</span></span> <span data-ttu-id="36508-105">To může být užitečné, když zapouzdření ve třídě se používá k omezení použití a/nebo vylepšit sémantiku týkající se přístupu k datům kódem aplikace, ale hodnota by měla být číst z a/nebo zapsána do databáze bez použití těchto omezení / vylepšení.</span><span class="sxs-lookup"><span data-stu-id="36508-105">This can be useful when encapsulation in the class is being used to restrict the use of and/or enhance the semantics around access to the data by application code, but the value should be read from and/or written to the database without using those restrictions/enhancements.</span></span>

## <a name="conventions"></a><span data-ttu-id="36508-106">Konvence</span><span class="sxs-lookup"><span data-stu-id="36508-106">Conventions</span></span>

<span data-ttu-id="36508-107">Podle konvence bude zjištěno následující pole jako pomocná pole pro dané vlastnosti (uvedené v pořadí priority).</span><span class="sxs-lookup"><span data-stu-id="36508-107">By convention, the following fields will be discovered as backing fields for a given property (listed in precedence order).</span></span> <span data-ttu-id="36508-108">Pole budou zjišťována pouze pro vlastnosti, které jsou součástí modelu.</span><span class="sxs-lookup"><span data-stu-id="36508-108">Fields are only discovered for properties that are included in the model.</span></span> <span data-ttu-id="36508-109">Další informace, na které vlastnosti jsou součástí modelu najdete v tématu [zahrnutí a vyloučení vlastností](included-properties.md).</span><span class="sxs-lookup"><span data-stu-id="36508-109">For more information on which properties are included in the model, see [Including & Excluding Properties](included-properties.md).</span></span>

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/BackingField.cs#Sample)]

<span data-ttu-id="36508-110">Pokud je nakonfigurovaná pomocným polem, EF bude zapisovat přímo do tohoto pole při materializaci instancí entit z databáze (spíš než setter vlastnosti).</span><span class="sxs-lookup"><span data-stu-id="36508-110">When a backing field is configured, EF will write directly to that field when materializing entity instances from the database (rather than using the property setter).</span></span> <span data-ttu-id="36508-111">Pokud EF potřebuje číst nebo zapisovat hodnoty v jinou dobu, použije vlastnost Pokud je to možné.</span><span class="sxs-lookup"><span data-stu-id="36508-111">If EF needs to read or write the value at other times, it will use the property if possible.</span></span> <span data-ttu-id="36508-112">Například pokud EF je potřeba aktualizovat hodnotu pro vlastnost, použije tento zdroj vlastnosti je-li definován.</span><span class="sxs-lookup"><span data-stu-id="36508-112">For example, if EF needs to update the value for a property, it will use the property setter if one is defined.</span></span> <span data-ttu-id="36508-113">Pokud vlastnost je jen pro čtení, bude zapisovat do příslušného pole.</span><span class="sxs-lookup"><span data-stu-id="36508-113">If the property is read-only, then it will write to the field.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="36508-114">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="36508-114">Data Annotations</span></span>

<span data-ttu-id="36508-115">Pomocná pole nelze konfigurovat pomocí datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="36508-115">Backing fields cannot be configured with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="36508-116">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="36508-116">Fluent API</span></span>

<span data-ttu-id="36508-117">Rozhraní Fluent API můžete použít ke konfiguraci pomocné pole pro vlastnost.</span><span class="sxs-lookup"><span data-stu-id="36508-117">You can use the Fluent API to configure a backing field for a property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a><span data-ttu-id="36508-118">Řízení při použití pole</span><span class="sxs-lookup"><span data-stu-id="36508-118">Controlling when the field is used</span></span>

<span data-ttu-id="36508-119">Můžete nakonfigurovat při EF používá pole nebo vlastnost.</span><span class="sxs-lookup"><span data-stu-id="36508-119">You can configure when EF uses the field or property.</span></span> <span data-ttu-id="36508-120">Zobrazit [PropertyAccessMode výčtu](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) podporované možnosti.</span><span class="sxs-lookup"><span data-stu-id="36508-120">See the [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) for the supported options.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a><span data-ttu-id="36508-121">Pole bez vlastnosti</span><span class="sxs-lookup"><span data-stu-id="36508-121">Fields without a property</span></span>

<span data-ttu-id="36508-122">Můžete také vytvořit konceptuální vlastnost v modelu, který nemá odpovídající vlastnost CLR ve třídě entity, ale místo toho používá pole k uložení dat v dané entitě.</span><span class="sxs-lookup"><span data-stu-id="36508-122">You can also create a conceptual property in your model that does not have a corresponding CLR property in the entity class, but instead uses a field to store the data in the entity.</span></span> <span data-ttu-id="36508-123">Tím se liší od [stínové vlastnosti](shadow-properties.md), kde jsou data uložená v nástroji Sledování změn.</span><span class="sxs-lookup"><span data-stu-id="36508-123">This is different from [Shadow Properties](shadow-properties.md), where the data is stored in the change tracker.</span></span> <span data-ttu-id="36508-124">To by obvykle použijí, pokud třída entity pomocí metody get/set hodnoty.</span><span class="sxs-lookup"><span data-stu-id="36508-124">This would typically be used if the entity class uses methods to get/set values.</span></span>

<span data-ttu-id="36508-125">EF můžete dát název pole v `Property(...)` rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="36508-125">You can give EF the name of the field in the `Property(...)` API.</span></span> <span data-ttu-id="36508-126">Pokud není žádná vlastnost s daným názvem, bude vypadat EF pro pole.</span><span class="sxs-lookup"><span data-stu-id="36508-126">If there is no property with the given name, then EF will look for a field.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldNoProperty.cs#Sample)]

<span data-ttu-id="36508-127">Můžete také poskytnout vlastnost názvu než názvu pole.</span><span class="sxs-lookup"><span data-stu-id="36508-127">You can also choose to give the property a name, other than the field name.</span></span> <span data-ttu-id="36508-128">Tento název se použije při vytváření modelu, zejména se použije pro název sloupce, který je namapovaný na v databázi.</span><span class="sxs-lookup"><span data-stu-id="36508-128">This name is then used when creating the model, most notably it will be used for the column name that is mapped to in the database.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldConceptualProperty.cs#Sample)]

<span data-ttu-id="36508-129">Pokud není žádná vlastnost ve třídě entity, můžete použít `EF.Property(...)` metoda v dotazu LINQ k odkazování na vlastnost, která je koncepčně součástí modelu.</span><span class="sxs-lookup"><span data-stu-id="36508-129">When there is no property in the entity class, you can use the `EF.Property(...)` method in a LINQ query to refer to the property that is conceptually part of the model.</span></span>

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
