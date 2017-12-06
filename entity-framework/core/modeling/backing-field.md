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
# <a name="backing-fields"></a><span data-ttu-id="b16ef-102">Základní pole</span><span class="sxs-lookup"><span data-stu-id="b16ef-102">Backing Fields</span></span>

> [!NOTE]  
> <span data-ttu-id="b16ef-103">Tato funkce je nového v EF základní 1.1.</span><span class="sxs-lookup"><span data-stu-id="b16ef-103">This feature is new in EF Core 1.1.</span></span>

<span data-ttu-id="b16ef-104">Základní pole Povolit EF číst nebo zapisovat do pole, nikoli vlastnost.</span><span class="sxs-lookup"><span data-stu-id="b16ef-104">Backing fields allow EF to read and/or write to a field rather than a property.</span></span> <span data-ttu-id="b16ef-105">To může být užitečné, když zapouzdření ve třídě, je používána pro omezit použití nebo vylepšit sémantiku kolem přístup k datům kód aplikace, ale hodnota by měla číst z a zapsána do databáze bez použití těchto omezení nebo vylepšení.</span><span class="sxs-lookup"><span data-stu-id="b16ef-105">This can be useful when encapsulation in the class is being used to restrict the use of and/or enhance the semantics around access to the data by application code, but the value should be read from and/or written to the database without using those restrictions/enhancements.</span></span>

## <a name="conventions"></a><span data-ttu-id="b16ef-106">Konvence</span><span class="sxs-lookup"><span data-stu-id="b16ef-106">Conventions</span></span>

<span data-ttu-id="b16ef-107">Podle konvence budou zjištěny následující pole jako pole pro danou vlastnost (uvedené v pořadí priorit) zálohování.</span><span class="sxs-lookup"><span data-stu-id="b16ef-107">By convention, the following fields will be discovered as backing fields for a given property (listed in precedence order).</span></span> <span data-ttu-id="b16ef-108">Pole budou zjišťována pouze pro vlastnosti, které jsou součástí modelu.</span><span class="sxs-lookup"><span data-stu-id="b16ef-108">Fields are only discovered for properties that are included in the model.</span></span> <span data-ttu-id="b16ef-109">Další informace, na kterém jsou vlastnosti součástí modelu najdete v tématu [včetně & vyloučeny vlastnosti](included-properties.md).</span><span class="sxs-lookup"><span data-stu-id="b16ef-109">For more information on which properties are included in the model, see [Including & Excluding Properties](included-properties.md).</span></span>

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/BackingField.cs#Sample)]

<span data-ttu-id="b16ef-110">Pokud pole zálohování je nakonfigurován, EF bude zapisovat přímo do tohoto pole při vyhodnocování instancí entit z databáze (místo použití Metoda setter vlastnosti).</span><span class="sxs-lookup"><span data-stu-id="b16ef-110">When a backing field is configured, EF will write directly to that field when materializing entity instances from the database (rather than using the property setter).</span></span> <span data-ttu-id="b16ef-111">Pokud EF potřebuje ke čtení nebo zápis hodnoty v jinou dobu, je-li to možné používat vlastnost.</span><span class="sxs-lookup"><span data-stu-id="b16ef-111">If EF needs to read or write the value at other times, it will use the property if possible.</span></span> <span data-ttu-id="b16ef-112">Například pokud EF potřebuje aktualizujte hodnotu pro vlastnost, použije metoda setter vlastnosti je-li definován.</span><span class="sxs-lookup"><span data-stu-id="b16ef-112">For example, if EF needs to update the value for a property, it will use the property setter if one is defined.</span></span> <span data-ttu-id="b16ef-113">Pokud vlastnost je jen pro čtení, budou zapisovat do pole.</span><span class="sxs-lookup"><span data-stu-id="b16ef-113">If the property is read-only, then it will write to the field.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="b16ef-114">Datových poznámek</span><span class="sxs-lookup"><span data-stu-id="b16ef-114">Data Annotations</span></span>

<span data-ttu-id="b16ef-115">Zálohování pole nelze konfigurovat pomocí datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="b16ef-115">Backing fields cannot be configured with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="b16ef-116">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="b16ef-116">Fluent API</span></span>

<span data-ttu-id="b16ef-117">Rozhraní Fluent API můžete použít ke konfiguraci základní pole pro vlastnost.</span><span class="sxs-lookup"><span data-stu-id="b16ef-117">You can use the Fluent API to configure a backing field for a property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a><span data-ttu-id="b16ef-118">Řízení, pokud se používá pole</span><span class="sxs-lookup"><span data-stu-id="b16ef-118">Controlling when the field is used</span></span>

<span data-ttu-id="b16ef-119">Můžete nakonfigurovat při EF používá pole nebo vlastnost.</span><span class="sxs-lookup"><span data-stu-id="b16ef-119">You can configure when EF uses the field or property.</span></span> <span data-ttu-id="b16ef-120">Najdete v článku [PropertyAccessMode výčtu](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) podporované možnosti.</span><span class="sxs-lookup"><span data-stu-id="b16ef-120">See the [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) for the supported options.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a><span data-ttu-id="b16ef-121">Pole bez vlastnost</span><span class="sxs-lookup"><span data-stu-id="b16ef-121">Fields without a property</span></span>

<span data-ttu-id="b16ef-122">Můžete také vytvořit konceptuální vlastnosti v daném modelu, který nemá odpovídající vlastnosti CLR ve třídě entity, ale místo toho používá pole k uložení dat v entitě.</span><span class="sxs-lookup"><span data-stu-id="b16ef-122">You can also create a conceptual property in your model that does not have a corresponding CLR property in the entity class, but instead uses a field to store the data in the entity.</span></span> <span data-ttu-id="b16ef-123">To se liší od [vlastnosti stínové](shadow-properties.md), kde jsou data uložena v nástroji Sledování změn.</span><span class="sxs-lookup"><span data-stu-id="b16ef-123">This is different from [Shadow Properties](shadow-properties.md), where the data is stored in the change tracker.</span></span> <span data-ttu-id="b16ef-124">To by obvykle použijí, pokud třídy entita pomocí metody get nebo nastavení hodnoty.</span><span class="sxs-lookup"><span data-stu-id="b16ef-124">This would typically be used if the entity class uses methods to get/set values.</span></span>

<span data-ttu-id="b16ef-125">Můžete udělit EF název v poli `Property(...)` rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b16ef-125">You can give EF the name of the field in the `Property(...)` API.</span></span> <span data-ttu-id="b16ef-126">Pokud není žádná vlastnost s daným názvem, bude vypadat EF pro pole.</span><span class="sxs-lookup"><span data-stu-id="b16ef-126">If there is no property with the given name, then EF will look for a field.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldNoProperty.cs#Sample)]

<span data-ttu-id="b16ef-127">Můžete také poskytnout vlastnost název, jiný než název pole.</span><span class="sxs-lookup"><span data-stu-id="b16ef-127">You can also choose to give the property a name, other than the field name.</span></span> <span data-ttu-id="b16ef-128">Tento název se pak používá při vytváření modelu, zejména se použije pro název sloupce, který je namapovaný na v databázi.</span><span class="sxs-lookup"><span data-stu-id="b16ef-128">This name is then used when creating the model, most notably it will be used for the column name that is mapped to in the database.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldConceptualProperty.cs#Sample)]

<span data-ttu-id="b16ef-129">Pokud není žádná vlastnost v třídě entity, můžete použít `EF.Property(...)` metoda v dotazu LINQ odkazovat na vlastnost, která je koncepčně součástí modelu.</span><span class="sxs-lookup"><span data-stu-id="b16ef-129">When there is no property in the entity class, you can use the `EF.Property(...)` method in a LINQ query to refer to the property that is conceptually part of the model.</span></span>

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
