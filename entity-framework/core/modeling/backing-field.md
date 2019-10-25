---
title: Pole pro zálohování – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: 288440a4494117fe59d27187e24424c4d2fd44ab
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/23/2019
ms.locfileid: "72811873"
---
# <a name="backing-fields"></a><span data-ttu-id="25aa0-102">Pomocná pole</span><span class="sxs-lookup"><span data-stu-id="25aa0-102">Backing Fields</span></span>

> [!NOTE]  
> <span data-ttu-id="25aa0-103">Tato funkce je v EF Core 1,1 novinkou.</span><span class="sxs-lookup"><span data-stu-id="25aa0-103">This feature is new in EF Core 1.1.</span></span>

<span data-ttu-id="25aa0-104">Zálohovací pole umožňují EF čtení a zápis do pole, nikoli jako vlastnost.</span><span class="sxs-lookup"><span data-stu-id="25aa0-104">Backing fields allow EF to read and/or write to a field rather than a property.</span></span> <span data-ttu-id="25aa0-105">To může být užitečné, pokud je zapouzdření ve třídě používáno k omezení použití nebo rozšíření sémantiky pro přístup k datům pomocí kódu aplikace, ale tato hodnota by měla být načtena z databáze nebo zapsána do databáze bez použití těchto omezení/ prvky.</span><span class="sxs-lookup"><span data-stu-id="25aa0-105">This can be useful when encapsulation in the class is being used to restrict the use of and/or enhance the semantics around access to the data by application code, but the value should be read from and/or written to the database without using those restrictions/enhancements.</span></span>

## <a name="conventions"></a><span data-ttu-id="25aa0-106">Konvence</span><span class="sxs-lookup"><span data-stu-id="25aa0-106">Conventions</span></span>

<span data-ttu-id="25aa0-107">Podle konvence se pro danou vlastnost (uvedená v pořadí podle priority) zobrazí následující pole jako zálohovaná pole.</span><span class="sxs-lookup"><span data-stu-id="25aa0-107">By convention, the following fields will be discovered as backing fields for a given property (listed in precedence order).</span></span> <span data-ttu-id="25aa0-108">Pole jsou zjištěna pouze pro vlastnosti, které jsou zahrnuty v modelu.</span><span class="sxs-lookup"><span data-stu-id="25aa0-108">Fields are only discovered for properties that are included in the model.</span></span> <span data-ttu-id="25aa0-109">Další informace o tom, které vlastnosti jsou zahrnuty v modelu, najdete v tématu [zahrnutí & s výjimkou vlastností](included-properties.md).</span><span class="sxs-lookup"><span data-stu-id="25aa0-109">For more information on which properties are included in the model, see [Including & Excluding Properties](included-properties.md).</span></span>

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/BackingField.cs#Sample)]

<span data-ttu-id="25aa0-110">Pokud je nakonfigurováno zálohovací pole, EF přepíše přímo do tohoto pole při vyhodnocování instancí entit z databáze (místo použití vlastnosti setter).</span><span class="sxs-lookup"><span data-stu-id="25aa0-110">When a backing field is configured, EF will write directly to that field when materializing entity instances from the database (rather than using the property setter).</span></span> <span data-ttu-id="25aa0-111">Pokud EF potřebuje číst nebo zapsat hodnotu v jinou dobu, bude tuto vlastnost používat, pokud je to možné.</span><span class="sxs-lookup"><span data-stu-id="25aa0-111">If EF needs to read or write the value at other times, it will use the property if possible.</span></span> <span data-ttu-id="25aa0-112">Například pokud EF potřebuje aktualizovat hodnotu pro vlastnost, použije vlastnost setter, pokud je definována.</span><span class="sxs-lookup"><span data-stu-id="25aa0-112">For example, if EF needs to update the value for a property, it will use the property setter if one is defined.</span></span> <span data-ttu-id="25aa0-113">Je-li vlastnost určena pouze pro čtení, bude zapsána do pole.</span><span class="sxs-lookup"><span data-stu-id="25aa0-113">If the property is read-only, then it will write to the field.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="25aa0-114">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="25aa0-114">Data Annotations</span></span>

<span data-ttu-id="25aa0-115">U zálohovaných polí nelze konfigurovat datové poznámky.</span><span class="sxs-lookup"><span data-stu-id="25aa0-115">Backing fields cannot be configured with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="25aa0-116">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="25aa0-116">Fluent API</span></span>

<span data-ttu-id="25aa0-117">Rozhraní Fluent API můžete použít ke konfiguraci pole pro zálohování pro vlastnost.</span><span class="sxs-lookup"><span data-stu-id="25aa0-117">You can use the Fluent API to configure a backing field for a property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a><span data-ttu-id="25aa0-118">Řízení při použití pole</span><span class="sxs-lookup"><span data-stu-id="25aa0-118">Controlling when the field is used</span></span>

<span data-ttu-id="25aa0-119">Můžete nakonfigurovat, kdy EF používá pole nebo vlastnost.</span><span class="sxs-lookup"><span data-stu-id="25aa0-119">You can configure when EF uses the field or property.</span></span> <span data-ttu-id="25aa0-120">Podporované možnosti najdete ve [výčtu PropertyAccessMode](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) .</span><span class="sxs-lookup"><span data-stu-id="25aa0-120">See the [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) for the supported options.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a><span data-ttu-id="25aa0-121">Pole bez vlastnosti</span><span class="sxs-lookup"><span data-stu-id="25aa0-121">Fields without a property</span></span>

<span data-ttu-id="25aa0-122">V modelu můžete také vytvořit koncepční vlastnost, která nemá odpovídající vlastnost CLR v třídě entity, ale místo toho používá pole k uložení dat v entitě.</span><span class="sxs-lookup"><span data-stu-id="25aa0-122">You can also create a conceptual property in your model that does not have a corresponding CLR property in the entity class, but instead uses a field to store the data in the entity.</span></span> <span data-ttu-id="25aa0-123">To se liší od [vlastností stín](shadow-properties.md), kde jsou data uložená v sledování změn.</span><span class="sxs-lookup"><span data-stu-id="25aa0-123">This is different from [Shadow Properties](shadow-properties.md), where the data is stored in the change tracker.</span></span> <span data-ttu-id="25aa0-124">To se obvykle používá, pokud třída entity používá metody k získání nebo nastavení hodnot.</span><span class="sxs-lookup"><span data-stu-id="25aa0-124">This would typically be used if the entity class uses methods to get/set values.</span></span>

<span data-ttu-id="25aa0-125">EF můžete zadat název pole v rozhraní `Property(...)` API.</span><span class="sxs-lookup"><span data-stu-id="25aa0-125">You can give EF the name of the field in the `Property(...)` API.</span></span> <span data-ttu-id="25aa0-126">Pokud neexistuje žádná vlastnost se zadaným názvem, pak bude v EF Hledat pole.</span><span class="sxs-lookup"><span data-stu-id="25aa0-126">If there is no property with the given name, then EF will look for a field.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldNoProperty.cs#Sample)]

<span data-ttu-id="25aa0-127">Pokud třída entity neobsahuje žádnou vlastnost, můžete použít metodu `EF.Property(...)` v dotazu LINQ pro odkazování na vlastnost, která je koncepčně součástí modelu.</span><span class="sxs-lookup"><span data-stu-id="25aa0-127">When there is no property in the entity class, you can use the `EF.Property(...)` method in a LINQ query to refer to the property that is conceptually part of the model.</span></span>

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "_validatedUrl"));
```
