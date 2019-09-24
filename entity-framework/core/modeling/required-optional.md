---
title: Povinné a volitelné vlastnosti – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: fd9e96e6f79965e63b07c21217edd004fd5c4d54
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197851"
---
# <a name="required-and-optional-properties"></a><span data-ttu-id="498d7-102">Povinné a volitelné vlastnosti</span><span class="sxs-lookup"><span data-stu-id="498d7-102">Required and Optional Properties</span></span>

<span data-ttu-id="498d7-103">Vlastnost je považována za volitelnou, pokud je platná pro její `null`zahrnutí.</span><span class="sxs-lookup"><span data-stu-id="498d7-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="498d7-104">Pokud `null` není platná hodnota, která má být přiřazena vlastnosti, je považována za požadovanou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="498d7-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

<span data-ttu-id="498d7-105">Při mapování na schéma relační databáze jsou požadované vlastnosti vytvořeny jako sloupce, které neumožňují hodnotu null, a volitelné vlastnosti jsou vytvořeny jako sloupce s možnou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="498d7-105">When mapping to a relational database schema, required properties are created as non-nullable columns, and optional properties are created as nullable columns.</span></span>

## <a name="conventions"></a><span data-ttu-id="498d7-106">Konvence</span><span class="sxs-lookup"><span data-stu-id="498d7-106">Conventions</span></span>

<span data-ttu-id="498d7-107">Podle konvence vlastnost, jejíž typ .NET může obsahovat hodnotu null, bude nakonfigurována jako volitelná, zatímco vlastnosti, jejichž typ .NET nesmí obsahovat hodnotu null, budou nakonfigurovány jako povinné.</span><span class="sxs-lookup"><span data-stu-id="498d7-107">By convention, a property whose .NET type can contain null will be configured as optional, whereas properties whose .NET type cannot contain null will be configured as required.</span></span> <span data-ttu-id="498d7-108">Například všechny vlastnosti s typy hodnot .NET (`int`, `decimal`, `bool`atd.) jsou konfigurovány podle potřeby a všechny vlastnosti s Nullable typy hodnot typu .NET (`int?`, `decimal?`, `bool?`atd.). nakonfigurováno jako volitelné.</span><span class="sxs-lookup"><span data-stu-id="498d7-108">For example, all properties with .NET value types (`int`, `decimal`, `bool`, etc.) are configured as required, and all properties with nullable .NET value types (`int?`, `decimal?`, `bool?`, etc.) are configured as optional.</span></span>

<span data-ttu-id="498d7-109">C#8 zavádí novou funkci nazvanou [typ odkazu s možnou hodnotou null](/dotnet/csharp/tutorials/nullable-reference-types), která umožňuje odkazování na typy odkazů, které označují, zda jsou pro ně platné hodnoty null nebo ne.</span><span class="sxs-lookup"><span data-stu-id="498d7-109">C# 8 introduced a new feature called [nullable reference types](/dotnet/csharp/tutorials/nullable-reference-types), which allows reference types to be annotated, indicating whether it is valid for them to contain null or not.</span></span> <span data-ttu-id="498d7-110">Tato funkce je ve výchozím nastavení zakázaná a v případě jejího povolení upraví chování EF Core následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="498d7-110">This feature is disabled by default, and if enabled, it modifies EF Core's behavior in the following way:</span></span>

* <span data-ttu-id="498d7-111">Pokud jsou typy odkazů s možnou hodnotou null (výchozí), všechny vlastnosti s odkazy .NET se nakonfigurují jako volitelné podle konvence `string`(např.).</span><span class="sxs-lookup"><span data-stu-id="498d7-111">If nullable reference types are disabled (the default), all properties with .NET reference types are configured as optional by convention (e.g. `string`).</span></span>
* <span data-ttu-id="498d7-112">Pokud jsou povoleny typy odkazů s možnou hodnotou null, budou vlastnosti nakonfigurovány na základě C# hodnoty null jejich typu `string?` .NET: budou nakonfigurovány jako volitelné `string` , zatímco budou nakonfigurovány jako povinné.</span><span class="sxs-lookup"><span data-stu-id="498d7-112">If nullable reference types are enabled, properties will be configured based on the C# nullability of their .NET type: `string?` will be configured as optional, whereas `string` will be configured as required.</span></span>

<span data-ttu-id="498d7-113">Následující příklad znázorňuje typ entity s požadovanými a volitelnými vlastnostmi, přičemž odkazovaná funkce s možnou hodnotou null je zakázaná (výchozí) a povolená:</span><span class="sxs-lookup"><span data-stu-id="498d7-113">The following example shows an entity type with required and optional properties, with the nullable reference feature disabled (the default) and enabled:</span></span>

# <a name="without-nullable-reference-types-defaulttabwithout-nrt"></a>[<span data-ttu-id="498d7-114">Bez odkazových typů s možnou hodnotou null (výchozí)</span><span class="sxs-lookup"><span data-stu-id="498d7-114">Without nullable reference types (default)</span></span>](#tab/without-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/CustomerWithoutNullableReferenceTypes.cs?name=Customer&highlight=4-8)]

# <a name="with-nullable-reference-typestabwith-nrt"></a>[<span data-ttu-id="498d7-115">S typy odkazů s možnou hodnotou null</span><span class="sxs-lookup"><span data-stu-id="498d7-115">With nullable reference types</span></span>](#tab/with-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Customer.cs?name=Customer&highlight=4-6)]

***

<span data-ttu-id="498d7-116">Doporučuje se použít typy odkazů s možnou hodnotou null, protože flowuje C# hodnotu null vyjádřenou v kódu, aby EF Core model a databáze, a obviates použití rozhraní Fluent API nebo datových poznámek pro vyjádření stejného konceptu dvakrát.</span><span class="sxs-lookup"><span data-stu-id="498d7-116">Using nullable reference types is recommended since it flows the nullability expressed in C# code to EF Core's model and to the database, and obviates the use of the Fluent API or Data Annotations to express the same concept twice.</span></span>

> [!NOTE]
> <span data-ttu-id="498d7-117">Cvičení opatrnosti při povolování typů odkazů s možnou hodnotou null u existujícího projektu: vlastnosti referenčního typu, které byly dříve nakonfigurovány jako volitelné, budou nyní konfigurovány jako povinné, pokud nejsou explicitně opatřeny poznámkami k Nullable.</span><span class="sxs-lookup"><span data-stu-id="498d7-117">Exercise caution when enabling nullable reference types on an existing project: reference type properties which were previously configured as optional will now be configured as required, unless they are explicitly annotated to be nullable.</span></span> <span data-ttu-id="498d7-118">Při správě schématu relační databáze to může způsobit, že se budou vygenerovat migrace, které změní hodnotu vlastnosti Database na sloupec.</span><span class="sxs-lookup"><span data-stu-id="498d7-118">When managing a relational database schema, this may cause migrations to be generated which alter the database column's nullability.</span></span>

<span data-ttu-id="498d7-119">Další informace o typech odkazů s možnou hodnotou null a o tom, jak je používat s EF Core, [najdete na stránce vyhrazená dokumentace pro tuto funkci](xref:core/miscellaneous/nullable-reference-types).</span><span class="sxs-lookup"><span data-stu-id="498d7-119">For more information on nullable reference types and how to use them with EF Core, [see the dedicated documentation page for this feature](xref:core/miscellaneous/nullable-reference-types).</span></span>

## <a name="configuration"></a><span data-ttu-id="498d7-120">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="498d7-120">Configuration</span></span>

<span data-ttu-id="498d7-121">Vlastnost, která by byla volitelná konvencí, se dá nakonfigurovat tak, aby se vyžadovala takto:</span><span class="sxs-lookup"><span data-stu-id="498d7-121">A property that would be optional by convention can be configured to be required as follows:</span></span>

# <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="498d7-122">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="498d7-122">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=14)]

# <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="498d7-123">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="498d7-123">Fluent API</span></span>](#tab/fluent-api) 

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=11-13)]

***