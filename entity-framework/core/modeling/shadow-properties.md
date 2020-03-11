---
title: Vlastnosti stínu – EF Core
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: 229cfd83f75b01dff9ac9ad30ee55c7cc727c19e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417410"
---
# <a name="shadow-properties"></a><span data-ttu-id="44df0-102">Stínové vlastnosti</span><span class="sxs-lookup"><span data-stu-id="44df0-102">Shadow Properties</span></span>

<span data-ttu-id="44df0-103">Vlastnosti stínu jsou vlastnosti, které nejsou definovány ve vaší třídě entity .NET, ale jsou definovány pro daný typ entity v modelu EF Core.</span><span class="sxs-lookup"><span data-stu-id="44df0-103">Shadow properties are properties that are not defined in your .NET entity class but are defined for that entity type in the EF Core model.</span></span> <span data-ttu-id="44df0-104">Hodnota a stav těchto vlastností se v sledování změn uchovávají čistě.</span><span class="sxs-lookup"><span data-stu-id="44df0-104">The value and state of these properties is maintained purely in the Change Tracker.</span></span> <span data-ttu-id="44df0-105">Vlastnosti stínu jsou užitečné, pokud jsou v databázi data, která by neměla být vystavena na mapovaných typech entit.</span><span class="sxs-lookup"><span data-stu-id="44df0-105">Shadow properties are useful when there is data in the database that should not be exposed on the mapped entity types.</span></span>

## <a name="foreign-key-shadow-properties"></a><span data-ttu-id="44df0-106">Vlastnosti stínování cizího klíče</span><span class="sxs-lookup"><span data-stu-id="44df0-106">Foreign key shadow properties</span></span>

<span data-ttu-id="44df0-107">Vlastnosti stínu se nejčastěji používají pro vlastnosti cizího klíče, kde vztah mezi dvěma entitami představuje hodnota cizího klíče v databázi, ale vztah je spravován na typech entit pomocí navigačních vlastností mezi entitou. druhy.</span><span class="sxs-lookup"><span data-stu-id="44df0-107">Shadow properties are most often used for foreign key properties, where the relationship between two entities is represented by a foreign key value in the database, but the relationship is managed on the entity types using navigation properties between the entity types.</span></span> <span data-ttu-id="44df0-108">Podle konvence zavádí EF vlastnost Shadow při zjištění vztahu, ale v třídě závislé entity není nalezena žádná vlastnost cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="44df0-108">By convention, EF will introduce a shadow property when a relationship is discovered but no foreign key property is found in the dependent entity class.</span></span>

<span data-ttu-id="44df0-109">Vlastnost bude pojmenována `<navigation property name><principal key property name>` (navigace na závislé entitě, která odkazuje na hlavní entitu, se používá pro pojmenování).</span><span class="sxs-lookup"><span data-stu-id="44df0-109">The property will be named `<navigation property name><principal key property name>` (the navigation on the dependent entity, which points to the principal entity, is used for the naming).</span></span> <span data-ttu-id="44df0-110">Pokud název vlastnosti klíč objektu zabezpečení obsahuje název vlastnosti navigace, pak bude název pouze `<principal key property name>`.</span><span class="sxs-lookup"><span data-stu-id="44df0-110">If the principal key property name includes the name of the navigation property, then the name will just be `<principal key property name>`.</span></span> <span data-ttu-id="44df0-111">Pokud není k dispozici žádná navigační vlastnost závislá entita, bude na svém místě použit název typu objektu zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="44df0-111">If there is no navigation property on the dependent entity, then the principal type name is used in its place.</span></span>

<span data-ttu-id="44df0-112">Například následující výpis kódu bude mít za následek zavlečení `BlogId` vlastnosti Shadow do `Post` entity:</span><span class="sxs-lookup"><span data-stu-id="44df0-112">For example, the following code listing will result in a `BlogId` shadow property being introduced to the `Post` entity:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/ShadowForeignKey.cs?name=Conventions&highlight=21-23)]

## <a name="configuring-shadow-properties"></a><span data-ttu-id="44df0-113">Konfigurace vlastností stínu</span><span class="sxs-lookup"><span data-stu-id="44df0-113">Configuring shadow properties</span></span>

<span data-ttu-id="44df0-114">Ke konfiguraci vlastností stínu můžete použít rozhraní Fluent API.</span><span class="sxs-lookup"><span data-stu-id="44df0-114">You can use the Fluent API to configure shadow properties.</span></span> <span data-ttu-id="44df0-115">Po volání přetížení řetězce `Property`můžete zřetězit libovolné volání konfigurace, které byste měli pro jiné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="44df0-115">Once you have called the string overload of `Property`, you can chain any of the configuration calls you would for other properties.</span></span> <span data-ttu-id="44df0-116">V následující ukázce, protože `Blog` nemá žádnou vlastnost CLR s názvem `LastUpdated`, je vytvořena vlastnost Shadow:</span><span class="sxs-lookup"><span data-stu-id="44df0-116">In the following sample, since `Blog` has no CLR property named `LastUpdated`, a shadow property is created:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ShadowProperty.cs?name=ShadowProperty&highlight=8)]

<span data-ttu-id="44df0-117">Pokud se název zadaný metodě `Property` shoduje s názvem existující vlastnosti (vlastnost Shadow nebo jedna definovaná na třídě entity), pak kód nakonfiguruje tuto vlastnost na místo zavedení nové vlastnosti stínu.</span><span class="sxs-lookup"><span data-stu-id="44df0-117">If the name supplied to the `Property` method matches the name of an existing property (a shadow property or one defined on the entity class), then the code will configure that existing property rather than introducing a new shadow property.</span></span>

## <a name="accessing-shadow-properties"></a><span data-ttu-id="44df0-118">Přístup k vlastnostem Shadow</span><span class="sxs-lookup"><span data-stu-id="44df0-118">Accessing shadow properties</span></span>

<span data-ttu-id="44df0-119">Hodnoty vlastností stínů se dají získat a změnit prostřednictvím rozhraní `ChangeTracker` API:</span><span class="sxs-lookup"><span data-stu-id="44df0-119">Shadow property values can be obtained and changed through the `ChangeTracker` API:</span></span>

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

<span data-ttu-id="44df0-120">Na vlastnosti stínu se dá odkazovat v dotazech LINQ prostřednictvím statické metody `EF.Property`:</span><span class="sxs-lookup"><span data-stu-id="44df0-120">Shadow properties can be referenced in LINQ queries via the `EF.Property` static method:</span></span>

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```
