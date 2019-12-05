---
title: Klíče (primární) – EF Core
description: Postup při konfiguraci klíčů pro typy entit při použití Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/keys
ms.openlocfilehash: fdaccb42259ba9dad97a05c626edd0291ca96cb0
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824618"
---
# <a name="keys-primary"></a><span data-ttu-id="ec59e-103">Klíče (primární)</span><span class="sxs-lookup"><span data-stu-id="ec59e-103">Keys (primary)</span></span>

<span data-ttu-id="ec59e-104">Klíč slouží jako primární jedinečný identifikátor pro každou instanci entity.</span><span class="sxs-lookup"><span data-stu-id="ec59e-104">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="ec59e-105">Při použití relační databáze se tato mapa mapuje na koncept *primárního klíče*.</span><span class="sxs-lookup"><span data-stu-id="ec59e-105">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="ec59e-106">Můžete také nakonfigurovat jedinečný identifikátor, který není primárním klíčem (Další informace najdete v tématu [alternativní klíče](alternate-keys.md) ).</span><span class="sxs-lookup"><span data-stu-id="ec59e-106">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

<span data-ttu-id="ec59e-107">Jednu z následujících metod lze použít k nastavení nebo vytvoření primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="ec59e-107">One of the following methods can be used to setup/create a primary key.</span></span>

## <a name="conventions"></a><span data-ttu-id="ec59e-108">Konvence</span><span class="sxs-lookup"><span data-stu-id="ec59e-108">Conventions</span></span>

<span data-ttu-id="ec59e-109">Ve výchozím nastavení bude vlastnost s názvem `Id` nebo `<type name>Id` nakonfigurována jako klíč entity.</span><span class="sxs-lookup"><span data-stu-id="ec59e-109">By default, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3)]

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyTypeNameId.cs?name=KeyId&highlight=3)]

> [!NOTE]
> <span data-ttu-id="ec59e-110">[Vlastní typy entit](xref:core/modeling/owned-entities) používají pro definování klíčů různá pravidla.</span><span class="sxs-lookup"><span data-stu-id="ec59e-110">[Owned entity types](xref:core/modeling/owned-entities) use different rules to define keys.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="ec59e-111">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="ec59e-111">Data Annotations</span></span>

<span data-ttu-id="ec59e-112">Pomocí datových poznámek můžete nakonfigurovat jednu vlastnost, která bude klíč entity.</span><span class="sxs-lookup"><span data-stu-id="ec59e-112">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="ec59e-113">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="ec59e-113">Fluent API</span></span>

<span data-ttu-id="ec59e-114">Pomocí rozhraní Fluent API můžete nakonfigurovat jedinou vlastnost, která bude klíč entity.</span><span class="sxs-lookup"><span data-stu-id="ec59e-114">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

<span data-ttu-id="ec59e-115">Rozhraní Fluent API můžete použít také ke konfiguraci více vlastností, které budou klíč entity (označované jako složený klíč).</span><span class="sxs-lookup"><span data-stu-id="ec59e-115">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="ec59e-116">Složené klíče se dají nakonfigurovat jenom pomocí konvencí rozhraní API Fluent nikdy nenastaví složený klíč a nemůžete použít datové poznámky ke konfiguraci jednoho.</span><span class="sxs-lookup"><span data-stu-id="ec59e-116">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]

## <a name="key-types-and-values"></a><span data-ttu-id="ec59e-117">Typy a hodnoty klíčů</span><span class="sxs-lookup"><span data-stu-id="ec59e-117">Key types and values</span></span>

<span data-ttu-id="ec59e-118">EF Core podporuje použití vlastností libovolného primitivního typu jako primárního klíče, včetně `string`, `Guid`, `byte[]` a dalších.</span><span class="sxs-lookup"><span data-stu-id="ec59e-118">EF Core supports using properties of any primitive type as the primary key, including `string`, `Guid`, `byte[]` and others.</span></span> <span data-ttu-id="ec59e-119">Ale ne všechny databáze je podporují.</span><span class="sxs-lookup"><span data-stu-id="ec59e-119">But not all databases support them.</span></span> <span data-ttu-id="ec59e-120">V některých případech je možné hodnoty klíče převést na podporovaný typ automaticky, jinak by se měl převod [zadat ručně](xref:core/modeling/value-conversions).</span><span class="sxs-lookup"><span data-stu-id="ec59e-120">In some cases the key values can be converted to a supported type automatically, otherwise the conversion should be [specified manually](xref:core/modeling/value-conversions).</span></span>

<span data-ttu-id="ec59e-121">Při přidávání nové entity do kontextu musí mít klíčové vlastnosti vždy jinou než výchozí hodnotu, ale [v databázi budou vygenerovány](xref:core/modeling/generated-properties)některé typy.</span><span class="sxs-lookup"><span data-stu-id="ec59e-121">Key properties must always have a non-default value when adding a new entity to the context, but some types will be [generated by the database](xref:core/modeling/generated-properties).</span></span> <span data-ttu-id="ec59e-122">V takovém případě EF se při přidání entity pro účely sledování pokusí vygenerovat dočasnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ec59e-122">In that case EF will try to generate a temporary value when the entity is added for tracking purposes.</span></span> <span data-ttu-id="ec59e-123">Po volání [metody SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) se tato dočasná hodnota nahradí hodnotou generovanou databází.</span><span class="sxs-lookup"><span data-stu-id="ec59e-123">After [SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) is called the temporary value will be replaced by the value generated by the database.</span></span>

> [!Important]
> <span data-ttu-id="ec59e-124">Pokud je vlastnost klíče vygenerovaná databází a při přidání entity je zadaná jiná než výchozí hodnota, pak EF předpokládá, že entita v databázi již existuje a pokusí se ji aktualizovat místo vložení nového.</span><span class="sxs-lookup"><span data-stu-id="ec59e-124">If a key property has value generated by the database and a non-default value is specified when an entity is added then EF will assume that the entity already exists in the database and will try to update it instead of inserting a new one.</span></span> <span data-ttu-id="ec59e-125">Chcete-li se vyhnout této generaci hodnoty, nebo se podívejte, [jak zadat explicitní hodnoty pro vygenerované vlastnosti](../saving/explicit-values-generated-properties.md).</span><span class="sxs-lookup"><span data-stu-id="ec59e-125">To avoid this turn off value generation or see [how to specify explicit values for generated properties](../saving/explicit-values-generated-properties.md).</span></span>