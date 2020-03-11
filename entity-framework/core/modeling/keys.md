---
title: Klíče – EF Core
description: Postup při konfiguraci klíčů pro typy entit při použití Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/keys
ms.openlocfilehash: abd65a5ea079a49fd7a3bbc84a9337f6ee19fab1
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416465"
---
# <a name="keys"></a><span data-ttu-id="7e73d-103">Klíče</span><span class="sxs-lookup"><span data-stu-id="7e73d-103">Keys</span></span>

<span data-ttu-id="7e73d-104">Klíč slouží jako jedinečný identifikátor pro každou instanci entity.</span><span class="sxs-lookup"><span data-stu-id="7e73d-104">A key serves as a unique identifier for each entity instance.</span></span> <span data-ttu-id="7e73d-105">Většina entit v EF má jediný klíč, který se mapuje na koncept *primárního klíče* v relačních databázích (pro entity bez klíčů, viz [entity bez klíčů](xref:core/modeling/keyless-entity-types)).</span><span class="sxs-lookup"><span data-stu-id="7e73d-105">Most entities in EF have a single key, which maps to the concept of a *primary key* in relational databases (for entities without keys, see [Keyless entities](xref:core/modeling/keyless-entity-types)).</span></span> <span data-ttu-id="7e73d-106">Entity můžou mít další klíče nad rámec primárního klíče (Další informace najdete v tématu [alternativní klíče](#alternate-keys) ).</span><span class="sxs-lookup"><span data-stu-id="7e73d-106">Entities can have additional keys beyond the primary key (see [Alternate Keys](#alternate-keys) for more information).</span></span>

<span data-ttu-id="7e73d-107">Podle konvence bude vlastnost s názvem `Id` nebo `<type name>Id` nakonfigurovaná jako primární klíč entity.</span><span class="sxs-lookup"><span data-stu-id="7e73d-107">By convention, a property named `Id` or `<type name>Id` will be configured as the primary key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3,11)]

> [!NOTE]
> <span data-ttu-id="7e73d-108">[Vlastní typy entit](xref:core/modeling/owned-entities) používají pro definování klíčů různá pravidla.</span><span class="sxs-lookup"><span data-stu-id="7e73d-108">[Owned entity types](xref:core/modeling/owned-entities) use different rules to define keys.</span></span>

<span data-ttu-id="7e73d-109">Jednu vlastnost můžete nastavit jako primární klíč entity následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="7e73d-109">You can configure a single property to be the primary key of an entity as follows:</span></span>

## <a name="data-annotations"></a>[<span data-ttu-id="7e73d-110">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="7e73d-110">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?name=KeySingle&highlight=3)]

## <a name="fluent-api"></a>[<span data-ttu-id="7e73d-111">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="7e73d-111">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?name=KeySingle&highlight=4)]

***

<span data-ttu-id="7e73d-112">Můžete také nakonfigurovat více vlastností, které mají být klíčem entity – to se označuje jako složený klíč.</span><span class="sxs-lookup"><span data-stu-id="7e73d-112">You can also configure multiple properties to be the key of an entity - this is known as a composite key.</span></span> <span data-ttu-id="7e73d-113">Složené klíče se dají konfigurovat jenom pomocí rozhraní API Fluent. konvence nikdy nenastaví složený klíč a nelze použít datové poznámky ke konfiguraci jednoho.</span><span class="sxs-lookup"><span data-stu-id="7e73d-113">Composite keys can only be configured using the Fluent API; conventions will never setup a composite key, and you can not use Data Annotations to configure one.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?name=KeyComposite&highlight=4)]

## <a name="primary-key-name"></a><span data-ttu-id="7e73d-114">Název primárního klíče</span><span class="sxs-lookup"><span data-stu-id="7e73d-114">Primary key name</span></span>

<span data-ttu-id="7e73d-115">Podle konvence se v relačních databázích vytvoří primární klíče s názvem `PK_<type name>`.</span><span class="sxs-lookup"><span data-stu-id="7e73d-115">By convention, on relational databases primary keys are created with the name `PK_<type name>`.</span></span> <span data-ttu-id="7e73d-116">Název omezení primárního klíče můžete nakonfigurovat takto:</span><span class="sxs-lookup"><span data-stu-id="7e73d-116">You can configure the name of the primary key constraint as follows:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyName.cs?name=KeyName&highlight=5)]

## <a name="key-types-and-values"></a><span data-ttu-id="7e73d-117">Typy a hodnoty klíčů</span><span class="sxs-lookup"><span data-stu-id="7e73d-117">Key types and values</span></span>

<span data-ttu-id="7e73d-118">I když EF Core podporuje použití vlastností libovolného primitivního typu jako primárního klíče, včetně `string`, `Guid`, `byte[]` a dalších, ne všechny databáze podporují všechny typy jako klíče.</span><span class="sxs-lookup"><span data-stu-id="7e73d-118">While EF Core supports using properties of any primitive type as the primary key, including `string`, `Guid`, `byte[]` and others, not all databases support all types as keys.</span></span> <span data-ttu-id="7e73d-119">V některých případech je možné hodnoty klíče převést na podporovaný typ automaticky, jinak by se měl převod [zadat ručně](xref:core/modeling/value-conversions).</span><span class="sxs-lookup"><span data-stu-id="7e73d-119">In some cases the key values can be converted to a supported type automatically, otherwise the conversion should be [specified manually](xref:core/modeling/value-conversions).</span></span>

<span data-ttu-id="7e73d-120">Při přidávání nové entity do kontextu musí mít klíčové vlastnosti vždy jinou než výchozí hodnotu, ale [v databázi budou vygenerovány](xref:core/modeling/generated-properties)některé typy.</span><span class="sxs-lookup"><span data-stu-id="7e73d-120">Key properties must always have a non-default value when adding a new entity to the context, but some types will be [generated by the database](xref:core/modeling/generated-properties).</span></span> <span data-ttu-id="7e73d-121">V takovém případě EF se při přidání entity pro účely sledování pokusí vygenerovat dočasnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="7e73d-121">In that case EF will try to generate a temporary value when the entity is added for tracking purposes.</span></span> <span data-ttu-id="7e73d-122">Po volání [metody SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) se tato dočasná hodnota nahradí hodnotou generovanou databází.</span><span class="sxs-lookup"><span data-stu-id="7e73d-122">After [SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) is called the temporary value will be replaced by the value generated by the database.</span></span>

> [!Important]
> <span data-ttu-id="7e73d-123">Pokud má klíčová vlastnost hodnotu vygenerovanou databází a při přidání entity je zadaná jiná než výchozí hodnota, pak EF předpokládá, že entita v databázi již existuje a pokusí se ji aktualizovat místo vložení nového.</span><span class="sxs-lookup"><span data-stu-id="7e73d-123">If a key property has its value generated by the database and a non-default value is specified when an entity is added, then EF will assume that the entity already exists in the database and will try to update it instead of inserting a new one.</span></span> <span data-ttu-id="7e73d-124">Chcete-li se vyhnout této generaci hodnoty, nebo se podívejte, [jak zadat explicitní hodnoty pro vygenerované vlastnosti](../saving/explicit-values-generated-properties.md).</span><span class="sxs-lookup"><span data-stu-id="7e73d-124">To avoid this turn off value generation or see [how to specify explicit values for generated properties](../saving/explicit-values-generated-properties.md).</span></span>

## <a name="alternate-keys"></a><span data-ttu-id="7e73d-125">Alternativní klíče</span><span class="sxs-lookup"><span data-stu-id="7e73d-125">Alternate Keys</span></span>

<span data-ttu-id="7e73d-126">Alternativní klíč slouží jako alternativní jedinečný identifikátor pro každou instanci entity kromě primárního klíče; dá se použít jako cíl relace.</span><span class="sxs-lookup"><span data-stu-id="7e73d-126">An alternate key serves as an alternate unique identifier for each entity instance in addition to the primary key; it can be used as the target of a relationship.</span></span> <span data-ttu-id="7e73d-127">Při použití relační databáze se tato mapa mapuje na koncept jedinečného indexu nebo omezení pro sloupce nebo sloupce alternativních klíčů a jedno nebo více omezení cizího klíče, které odkazují na sloupce.</span><span class="sxs-lookup"><span data-stu-id="7e73d-127">When using a relational database this maps to the concept of a unique index/constraint on the alternate key column(s) and one or more foreign key constraints that reference the column(s).</span></span>

> [!TIP]
> <span data-ttu-id="7e73d-128">Pokud chcete u sloupce jenom vyhovět jedinečnost, definujte jedinečný index místo alternativního klíče (viz [indexy](indexes.md)).</span><span class="sxs-lookup"><span data-stu-id="7e73d-128">If you just want to enforce uniqueness on a column, define a unique index rather than an alternate key (see [Indexes](indexes.md)).</span></span> <span data-ttu-id="7e73d-129">V EF jsou alternativní klíče jen pro čtení a poskytují další sémantiku v rámci jedinečných indexů, protože je lze použít jako cíl cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="7e73d-129">In EF, alternate keys are read-only and provide additional semantics over unique indexes because they can be used as the target of a foreign key.</span></span>

<span data-ttu-id="7e73d-130">V případě potřeby jsou obvykle zavedeny alternativní klíče a není nutné je ručně konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="7e73d-130">Alternate keys are typically introduced for you when needed and you do not need to manually configure them.</span></span> <span data-ttu-id="7e73d-131">Podle konvence se pro vás zavede alternativní klíč, když identifikujete vlastnost, která není primárním klíčem jako cíl relace.</span><span class="sxs-lookup"><span data-stu-id="7e73d-131">By convention, an alternate key is introduced for you when you identify a property which isn't the primary key as the target of a relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/AlternateKey.cs?name=AlternateKey&highlight=12)]

<span data-ttu-id="7e73d-132">Jednu vlastnost můžete také nakonfigurovat tak, aby byla alternativním klíčem:</span><span class="sxs-lookup"><span data-stu-id="7e73d-132">You can also configure a single property to be an alternate key:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeySingle.cs?name=AlternateKeySingle&highlight=4)]

<span data-ttu-id="7e73d-133">Můžete také nakonfigurovat více vlastností jako alternativní klíč (označované jako složený alternativní klíč):</span><span class="sxs-lookup"><span data-stu-id="7e73d-133">You can also configure multiple properties to be an alternate key (known as a composite alternate key):</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeyComposite.cs?name=AlternateKeyComposite&highlight=4)]

<span data-ttu-id="7e73d-134">Nakonec se podle konvence index a omezení, které jsou představené pro alternativní klíč, budou jmenovat `AK_<type name>_<property name>` (u složených alternativních klíčů `<property name>` se změní na seznam názvů vlastností oddělených znakem.)</span><span class="sxs-lookup"><span data-stu-id="7e73d-134">Finally, by convention, the index and constraint that are introduced for an alternate key will be named `AK_<type name>_<property name>` (for composite alternate keys `<property name>` becomes an underscore separated list of property names).</span></span> <span data-ttu-id="7e73d-135">Můžete nakonfigurovat název indexu alternativního klíče a jedinečné omezení:</span><span class="sxs-lookup"><span data-stu-id="7e73d-135">You can configure the name of the alternate key's index and unique constraint:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeyName.cs?name=AlternateKeyName&highlight=5)]
