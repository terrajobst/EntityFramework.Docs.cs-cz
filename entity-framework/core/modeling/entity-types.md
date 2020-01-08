---
title: Typy entit – EF Core
description: Postup konfigurace a mapování typů entit pomocí Entity Framework Core
author: roji
ms.date: 12/03/2019
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/entity-types
ms.openlocfilehash: b3d9ad753637d021d9aa52965da38091ae690f77
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502451"
---
# <a name="entity-types"></a><span data-ttu-id="2d78d-103">Typy entit</span><span class="sxs-lookup"><span data-stu-id="2d78d-103">Entity Types</span></span>

<span data-ttu-id="2d78d-104">Zahrnutí Negenerickými typu v kontextu znamená, že je zahrnuto v modelu EF Core; obvykle na takový typ odkazujeme jako na *entitu*.</span><span class="sxs-lookup"><span data-stu-id="2d78d-104">Including a DbSet of a type on your context means that it is included in EF Core's model; we usually refer to such a type as an *entity*.</span></span> <span data-ttu-id="2d78d-105">EF Core může číst a zapisovat instance entit z/do databáze a pokud používáte relační databázi, EF Core může vytvořit tabulky pro entity pomocí migrací.</span><span class="sxs-lookup"><span data-stu-id="2d78d-105">EF Core can read and write entity instances from/to the database, and if you're using a relational database, EF Core can create tables for your entities via migrations.</span></span>

## <a name="including-types-in-the-model"></a><span data-ttu-id="2d78d-106">Zahrnutí typů do modelu</span><span class="sxs-lookup"><span data-stu-id="2d78d-106">Including types in the model</span></span>

<span data-ttu-id="2d78d-107">Podle konvence jsou typy, které jsou zpřístupněny ve vlastnostech Negenerickými ve vašem kontextu, zahrnuty v modelu jako entity.</span><span class="sxs-lookup"><span data-stu-id="2d78d-107">By convention, types that are exposed in DbSet properties on your context are included in the model as entities.</span></span> <span data-ttu-id="2d78d-108">Typy entit, které jsou zadány v metodě `OnModelCreating`, jsou také zahrnuty, stejně jako všechny typy, které jsou nalezeny rekurzivním zkoumáním vlastností navigace jiných zjištěných typů entit.</span><span class="sxs-lookup"><span data-stu-id="2d78d-108">Entity types that are specified in the `OnModelCreating` method are also included, as are any types that are found by recursively exploring the navigation properties of other discovered entity types.</span></span>

<span data-ttu-id="2d78d-109">V níže uvedeném příkladu kódu jsou zahrnuty všechny typy:</span><span class="sxs-lookup"><span data-stu-id="2d78d-109">In the code sample below, all types are included:</span></span>

* <span data-ttu-id="2d78d-110">`Blog` je obsažena, protože je zpřístupněna v vlastnosti Negenerickými v kontextu.</span><span class="sxs-lookup"><span data-stu-id="2d78d-110">`Blog` is included because it's exposed in a DbSet property on the context.</span></span>
* <span data-ttu-id="2d78d-111">`Post` je obsažena, protože je zjištěna prostřednictvím vlastnosti navigace `Blog.Posts`.</span><span class="sxs-lookup"><span data-stu-id="2d78d-111">`Post` is included because it's discovered via the `Blog.Posts` navigation property.</span></span>
* <span data-ttu-id="2d78d-112">`AuditEntry`, protože je zadaný v `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="2d78d-112">`AuditEntry` because it is specified in `OnModelCreating`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/EntityTypes.cs?name=EntityTypes&highlight=3,7,16)]

## <a name="excluding-types-from-the-model"></a><span data-ttu-id="2d78d-113">Vyloučení typů z modelu</span><span class="sxs-lookup"><span data-stu-id="2d78d-113">Excluding types from the model</span></span>

<span data-ttu-id="2d78d-114">Pokud nechcete, aby se typ zahrnul do modelu, můžete ho vyloučit:</span><span class="sxs-lookup"><span data-stu-id="2d78d-114">If you don't want a type to be included in the model, you can exclude it:</span></span>

### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="2d78d-115">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="2d78d-115">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?name=IgnoreType&highlight=1)]

### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="2d78d-116">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="2d78d-116">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?name=IgnoreType&highlight=3)]

***

## <a name="table-name"></a><span data-ttu-id="2d78d-117">Název tabulky</span><span class="sxs-lookup"><span data-stu-id="2d78d-117">Table name</span></span>

<span data-ttu-id="2d78d-118">Podle konvence se každý typ entity nastaví tak, aby se namapoval na databázovou tabulku se stejným názvem, jako má vlastnost Negenerickými, která zpřístupňuje entitu.</span><span class="sxs-lookup"><span data-stu-id="2d78d-118">By convention, each entity type will be set up to map to a database table with the same name as the DbSet property that exposes the entity.</span></span> <span data-ttu-id="2d78d-119">Pokud pro danou entitu neexistuje žádný Negenerickými, použije se název třídy.</span><span class="sxs-lookup"><span data-stu-id="2d78d-119">If no DbSet exists for the given entity, the class name is used.</span></span>

<span data-ttu-id="2d78d-120">Název tabulky můžete nakonfigurovat ručně:</span><span class="sxs-lookup"><span data-stu-id="2d78d-120">You can manually configure the table name:</span></span>

### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="2d78d-121">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="2d78d-121">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/TableName.cs?Name=TableName&highlight=1)]

### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="2d78d-122">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="2d78d-122">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/TableName.cs?Name=TableName&highlight=3-4)]

***

## <a name="table-schema"></a><span data-ttu-id="2d78d-123">Schéma tabulky</span><span class="sxs-lookup"><span data-stu-id="2d78d-123">Table schema</span></span>

<span data-ttu-id="2d78d-124">Při použití relační databáze jsou tabulky podle konvence vytvořené ve výchozím schématu vaší databáze.</span><span class="sxs-lookup"><span data-stu-id="2d78d-124">When using a relational database, tables are by convention created in your database's default schema.</span></span> <span data-ttu-id="2d78d-125">Například Microsoft SQL Server použije schéma `dbo` (SQLite nepodporuje schémata).</span><span class="sxs-lookup"><span data-stu-id="2d78d-125">For example, Microsoft SQL Server will use the `dbo` schema (SQLite does not support schemas).</span></span>

<span data-ttu-id="2d78d-126">Tabulky můžete nakonfigurovat tak, aby se vytvořily v určitém schématu následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="2d78d-126">You can configure tables to be created in a specific schema as follows:</span></span>

### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="2d78d-127">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="2d78d-127">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/TableNameAndSchema.cs?name=TableNameAndSchema&highlight=1)]

### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="2d78d-128">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="2d78d-128">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/TableNameAndSchema.cs?name=TableNameAndSchema&highlight=3-4)]

***

<span data-ttu-id="2d78d-129">Místo zadání schématu pro každou tabulku můžete také definovat výchozí schéma na úrovni modelu pomocí rozhraní Fluent API:</span><span class="sxs-lookup"><span data-stu-id="2d78d-129">Rather than specifying the schema for each table, you can also define the default schema at the model level with the fluent API:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DefaultSchema.cs?name=DefaultSchema&highlight=3)]

<span data-ttu-id="2d78d-130">Všimněte si, že nastavení výchozího schématu ovlivní také další databázové objekty, jako jsou například sekvence.</span><span class="sxs-lookup"><span data-stu-id="2d78d-130">Note that setting the default schema will also affect other database objects, such as sequences.</span></span>
