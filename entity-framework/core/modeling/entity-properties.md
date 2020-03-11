---
title: Vlastnosti entity – EF Core
description: Postup konfigurace a mapování vlastností entity pomocí Entity Framework Core
author: roji
ms.date: 12/10/2019
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/entity-properties
ms.openlocfilehash: b67603fbffd1f1c8506bc21f8972c851eb8eef29
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417214"
---
# <a name="entity-properties"></a><span data-ttu-id="74a65-103">Vlastnosti entity</span><span class="sxs-lookup"><span data-stu-id="74a65-103">Entity Properties</span></span>

<span data-ttu-id="74a65-104">Každý typ entity v modelu má sadu vlastností, které EF Core budou číst a zapisovat z databáze.</span><span class="sxs-lookup"><span data-stu-id="74a65-104">Each entity type in your model has a set of properties, which EF Core will read and write from the database.</span></span> <span data-ttu-id="74a65-105">Pokud používáte relační databázi, vlastnosti entity se mapují na sloupce tabulky.</span><span class="sxs-lookup"><span data-stu-id="74a65-105">If you're using a relational database, entity properties map to table columns.</span></span>

## <a name="included-and-excluded-properties"></a><span data-ttu-id="74a65-106">Zahrnuté a vyloučené vlastnosti</span><span class="sxs-lookup"><span data-stu-id="74a65-106">Included and excluded properties</span></span>

<span data-ttu-id="74a65-107">Podle konvence budou v modelu zahrnuty všechny veřejné vlastnosti s metodami getter a setter.</span><span class="sxs-lookup"><span data-stu-id="74a65-107">By convention, all public properties with a getter and a setter will be included in the model.</span></span>

<span data-ttu-id="74a65-108">Konkrétní vlastnosti lze vyloučit následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="74a65-108">Specific properties can be excluded as follows:</span></span>

### <a name="data-annotations"></a>[<span data-ttu-id="74a65-109">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="74a65-109">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreProperty.cs?name=IgnoreProperty&highlight=6)]

### <a name="fluent-api"></a>[<span data-ttu-id="74a65-110">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="74a65-110">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreProperty.cs?name=IgnoreProperty&highlight=3,4)]

***

## <a name="column-names"></a><span data-ttu-id="74a65-111">Názvy sloupců</span><span class="sxs-lookup"><span data-stu-id="74a65-111">Column names</span></span>

<span data-ttu-id="74a65-112">Podle konvence při použití relační databáze jsou vlastnosti entit namapovány na sloupce tabulky se stejným názvem jako vlastnost.</span><span class="sxs-lookup"><span data-stu-id="74a65-112">By convention, when using a relational database, entity properties are mapped to table columns having the same name as the property.</span></span>

<span data-ttu-id="74a65-113">Pokud chcete nakonfigurovat sloupce s různými názvy, můžete tak učinit následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="74a65-113">If you prefer to configure your columns with different names, you can do so as following:</span></span>

### <a name="data-annotations"></a>[<span data-ttu-id="74a65-114">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="74a65-114">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ColumnName.cs?Name=ColumnName&highlight=3)]

### <a name="fluent-api"></a>[<span data-ttu-id="74a65-115">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="74a65-115">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ColumnName.cs?Name=ColumnName&highlight=3-5)]

***

## <a name="column-data-types"></a><span data-ttu-id="74a65-116">Datové typy sloupců</span><span class="sxs-lookup"><span data-stu-id="74a65-116">Column data types</span></span>

<span data-ttu-id="74a65-117">Při použití relační databáze poskytovatel databáze vybere datový typ založený na typu rozhraní .NET vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="74a65-117">When using a relational database, the database provider selects a data type based on the .NET type of the property.</span></span> <span data-ttu-id="74a65-118">Také vezme v úvahu jiná metadata, jako je například nakonfigurovaná [Maximální délka](#maximum-length), zda je vlastnost částí primárního klíče atd.</span><span class="sxs-lookup"><span data-stu-id="74a65-118">It also takes into account other metadata, such as the configured [maximum length](#maximum-length), whether the property is part of a primary key, etc.</span></span>

<span data-ttu-id="74a65-119">Například SQL Server map `DateTime` vlastnosti do `datetime2(7)` sloupců a `string` vlastnosti `nvarchar(max)` sloupců (nebo `nvarchar(450)` pro vlastnosti, které se používají jako klíč).</span><span class="sxs-lookup"><span data-stu-id="74a65-119">For example, SQL Server maps `DateTime` properties to `datetime2(7)` columns, and `string` properties to `nvarchar(max)` columns (or to `nvarchar(450)` for properties that are used as a key).</span></span>

<span data-ttu-id="74a65-120">Sloupce můžete také nakonfigurovat tak, aby pro sloupec určily přesný datový typ.</span><span class="sxs-lookup"><span data-stu-id="74a65-120">You can also configure your columns to specify an exact data type for a column.</span></span> <span data-ttu-id="74a65-121">Například následující kód nakonfiguruje `Url` jako řetězec jiný než Unicode s maximální délkou `200` a `Rating` jako desetinné číslo s přesností `5` a škálováním `2`:</span><span class="sxs-lookup"><span data-stu-id="74a65-121">For example the following code configures `Url` as a non-unicode string with maximum length of `200` and `Rating` as decimal with precision of `5` and scale of `2`:</span></span>

### <a name="data-annotations"></a>[<span data-ttu-id="74a65-122">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="74a65-122">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ColumnDataType.cs?name=ColumnDataType&highlight=4,6)]

### <a name="fluent-api"></a>[<span data-ttu-id="74a65-123">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="74a65-123">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ColumnDataType.cs?name=ColumnDataType&highlight=5-6)]

***

### <a name="maximum-length"></a><span data-ttu-id="74a65-124">Maximální délka</span><span class="sxs-lookup"><span data-stu-id="74a65-124">Maximum length</span></span>

<span data-ttu-id="74a65-125">Konfigurace maximální délky poskytuje nápovědu pro poskytovatele databáze o příslušném datovém typu sloupce, který se má zvolit pro danou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="74a65-125">Configuring a maximum length provides a hint to the database provider about the appropriate column data type to choose for a given property.</span></span> <span data-ttu-id="74a65-126">Maximální délka se vztahuje pouze na datové typy pole, například `string` a `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="74a65-126">Maximum length only applies to array data types, such as `string` and `byte[]`.</span></span>

> [!NOTE]
> <span data-ttu-id="74a65-127">Entity Framework neprovádí žádné ověření maximální délky před předáním dat poskytovateli.</span><span class="sxs-lookup"><span data-stu-id="74a65-127">Entity Framework does not do any validation of maximum length before passing data to the provider.</span></span> <span data-ttu-id="74a65-128">Pokud je to vhodné, je až do poskytovatele nebo úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="74a65-128">It is up to the provider or data store to validate if appropriate.</span></span> <span data-ttu-id="74a65-129">Například při cílení na SQL Server, což překračuje maximální délku, dojde k výjimce, protože datový typ podkladového sloupce nebude moci ukládat nadbytečné údaje.</span><span class="sxs-lookup"><span data-stu-id="74a65-129">For example, when targeting SQL Server, exceeding the maximum length will result in an exception as the data type of the underlying column will not allow excess data to be stored.</span></span>

<span data-ttu-id="74a65-130">V následujícím příkladu způsobí konfigurace maximální délky 500 sloupec typu `nvarchar(500)`, který se má vytvořit v SQL Server:</span><span class="sxs-lookup"><span data-stu-id="74a65-130">In the following example, configuring a maximum length of 500 will cause a column of type `nvarchar(500)` to be created on SQL Server:</span></span>

#### <a name="data-annotations"></a>[<span data-ttu-id="74a65-131">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="74a65-131">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/MaxLength.cs?name=MaxLength&highlight=4)]

#### <a name="fluent-api"></a>[<span data-ttu-id="74a65-132">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="74a65-132">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/MaxLength.cs?name=MaxLength&highlight=3-5)]

***

## <a name="required-and-optional-properties"></a><span data-ttu-id="74a65-133">Povinné a volitelné vlastnosti</span><span class="sxs-lookup"><span data-stu-id="74a65-133">Required and optional properties</span></span>

<span data-ttu-id="74a65-134">Vlastnost je považována za volitelnou, pokud je platná pro, aby obsahovala `null`.</span><span class="sxs-lookup"><span data-stu-id="74a65-134">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="74a65-135">Pokud `null` není platná hodnota, která má být přiřazena vlastnosti, považuje se za povinnou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="74a65-135">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span> <span data-ttu-id="74a65-136">Při mapování na schéma relační databáze jsou požadované vlastnosti vytvořeny jako sloupce, které neumožňují hodnotu null, a volitelné vlastnosti jsou vytvořeny jako sloupce s možnou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="74a65-136">When mapping to a relational database schema, required properties are created as non-nullable columns, and optional properties are created as nullable columns.</span></span>

### <a name="conventions"></a><span data-ttu-id="74a65-137">Zásady</span><span class="sxs-lookup"><span data-stu-id="74a65-137">Conventions</span></span>

<span data-ttu-id="74a65-138">Podle konvence vlastnost, jejíž typ .NET může obsahovat hodnotu null, bude nakonfigurována jako volitelná, zatímco vlastnosti, jejichž typ .NET nesmí obsahovat hodnotu null, budou nakonfigurovány jako povinné.</span><span class="sxs-lookup"><span data-stu-id="74a65-138">By convention, a property whose .NET type can contain null will be configured as optional, whereas properties whose .NET type cannot contain null will be configured as required.</span></span> <span data-ttu-id="74a65-139">Například všechny vlastnosti s typy hodnot .NET (`int`, `decimal`, `bool`atd.) jsou nakonfigurovány jako povinné a všechny vlastnosti s Nullable typy hodnot (`int?`, `decimal?`, `bool?`atd.) jsou nakonfigurovány jako volitelné.</span><span class="sxs-lookup"><span data-stu-id="74a65-139">For example, all properties with .NET value types (`int`, `decimal`, `bool`, etc.) are configured as required, and all properties with nullable .NET value types (`int?`, `decimal?`, `bool?`, etc.) are configured as optional.</span></span>

<span data-ttu-id="74a65-140">C#8 zavádí novou funkci nazvanou [typ odkazu s možnou hodnotou null](/dotnet/csharp/tutorials/nullable-reference-types), která umožňuje odkazování na typy odkazů, které označují, zda jsou pro ně platné hodnoty null nebo ne.</span><span class="sxs-lookup"><span data-stu-id="74a65-140">C# 8 introduced a new feature called [nullable reference types](/dotnet/csharp/tutorials/nullable-reference-types), which allows reference types to be annotated, indicating whether it is valid for them to contain null or not.</span></span> <span data-ttu-id="74a65-141">Tato funkce je ve výchozím nastavení zakázaná a v případě jejího povolení upraví chování EF Core následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="74a65-141">This feature is disabled by default, and if enabled, it modifies EF Core's behavior in the following way:</span></span>

* <span data-ttu-id="74a65-142">Pokud jsou typy odkazů s možnou hodnotou null (výchozí), všechny vlastnosti s odkazy .NET se nakonfigurují jako volitelné podle konvence (např. `string`).</span><span class="sxs-lookup"><span data-stu-id="74a65-142">If nullable reference types are disabled (the default), all properties with .NET reference types are configured as optional by convention (e.g. `string`).</span></span>
* <span data-ttu-id="74a65-143">Pokud jsou povoleny typy odkazů s možnou hodnotou null, budou vlastnosti nakonfigurovány na základě C# hodnoty null jejich typu .net: `string?` budou konfigurovány jako volitelné, zatímco `string` budou konfigurovány jako povinné.</span><span class="sxs-lookup"><span data-stu-id="74a65-143">If nullable reference types are enabled, properties will be configured based on the C# nullability of their .NET type: `string?` will be configured as optional, whereas `string` will be configured as required.</span></span>

<span data-ttu-id="74a65-144">Následující příklad znázorňuje typ entity s požadovanými a volitelnými vlastnostmi, přičemž odkazovaná funkce s možnou hodnotou null je zakázaná (výchozí) a povolená:</span><span class="sxs-lookup"><span data-stu-id="74a65-144">The following example shows an entity type with required and optional properties, with the nullable reference feature disabled (the default) and enabled:</span></span>

#### <a name="without-nullable-reference-types-default"></a>[<span data-ttu-id="74a65-145">Bez odkazových typů s možnou hodnotou null (výchozí)</span><span class="sxs-lookup"><span data-stu-id="74a65-145">Without nullable reference types (default)</span></span>](#tab/without-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/CustomerWithoutNullableReferenceTypes.cs?name=Customer&highlight=4-8)]

#### <a name="with-nullable-reference-types"></a>[<span data-ttu-id="74a65-146">S typy odkazů s možnou hodnotou null</span><span class="sxs-lookup"><span data-stu-id="74a65-146">With nullable reference types</span></span>](#tab/with-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Customer.cs?name=Customer&highlight=4-6)]

***

<span data-ttu-id="74a65-147">Doporučuje se použít typy odkazů s možnou hodnotou null, protože flowuje C# hodnotu null vyjádřenou v kódu, aby EF Core model a databáze, a obviates použití rozhraní Fluent API nebo datových poznámek pro vyjádření stejného konceptu dvakrát.</span><span class="sxs-lookup"><span data-stu-id="74a65-147">Using nullable reference types is recommended since it flows the nullability expressed in C# code to EF Core's model and to the database, and obviates the use of the Fluent API or Data Annotations to express the same concept twice.</span></span>

> [!NOTE]
> <span data-ttu-id="74a65-148">Cvičení opatrnosti při povolování typů odkazů s možnou hodnotou null u existujícího projektu: vlastnosti referenčního typu, které byly dříve nakonfigurovány jako volitelné, budou nyní konfigurovány jako povinné, pokud nejsou explicitně opatřeny poznámkami k Nullable.</span><span class="sxs-lookup"><span data-stu-id="74a65-148">Exercise caution when enabling nullable reference types on an existing project: reference type properties which were previously configured as optional will now be configured as required, unless they are explicitly annotated to be nullable.</span></span> <span data-ttu-id="74a65-149">Při správě schématu relační databáze to může způsobit, že se budou vygenerovat migrace, které změní hodnotu vlastnosti Database na sloupec.</span><span class="sxs-lookup"><span data-stu-id="74a65-149">When managing a relational database schema, this may cause migrations to be generated which alter the database column's nullability.</span></span>

<span data-ttu-id="74a65-150">Další informace o typech odkazů s možnou hodnotou null a o tom, jak je používat s EF Core, [najdete na stránce vyhrazená dokumentace pro tuto funkci](xref:core/miscellaneous/nullable-reference-types).</span><span class="sxs-lookup"><span data-stu-id="74a65-150">For more information on nullable reference types and how to use them with EF Core, [see the dedicated documentation page for this feature](xref:core/miscellaneous/nullable-reference-types).</span></span>

### <a name="explicit-configuration"></a><span data-ttu-id="74a65-151">Explicitní konfigurace</span><span class="sxs-lookup"><span data-stu-id="74a65-151">Explicit configuration</span></span>

<span data-ttu-id="74a65-152">Vlastnost, která by byla volitelná konvencí, se dá nakonfigurovat tak, aby se vyžadovala takto:</span><span class="sxs-lookup"><span data-stu-id="74a65-152">A property that would be optional by convention can be configured to be required as follows:</span></span>

#### <a name="data-annotations"></a>[<span data-ttu-id="74a65-153">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="74a65-153">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?name=Required&highlight=4)]

#### <a name="fluent-api"></a>[<span data-ttu-id="74a65-154">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="74a65-154">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?name=Required&highlight=3-5)]

***
