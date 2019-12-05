---
title: Dědičnost – EF Core
description: Jak nakonfigurovat dědičnost typu entity pomocí Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 10/27/2016
uid: core/modeling/inheritance
ms.openlocfilehash: 4d43a432174c92ab7f3f9d78a234aefb0a4a17e8
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824673"
---
# <a name="inheritance"></a><span data-ttu-id="e66f2-103">Dědičnost</span><span class="sxs-lookup"><span data-stu-id="e66f2-103">Inheritance</span></span>

<span data-ttu-id="e66f2-104">Dědičnost v modelu EF se používá k řízení způsobu reprezentace dědičnosti tříd entit v databázi.</span><span class="sxs-lookup"><span data-stu-id="e66f2-104">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="e66f2-105">Konvence</span><span class="sxs-lookup"><span data-stu-id="e66f2-105">Conventions</span></span>

<span data-ttu-id="e66f2-106">Ve výchozím nastavení je to poskytovatel databáze, aby bylo možné určit, jak bude dědění v databázi reprezentované.</span><span class="sxs-lookup"><span data-stu-id="e66f2-106">By default, it is up to the database provider to determine how inheritance will be represented in the database.</span></span> <span data-ttu-id="e66f2-107">Podívejte se na téma [Dědičnost (relační databáze)](relational/inheritance.md) , ve kterém se tato funkce zpracovává u poskytovatele relační databáze.</span><span class="sxs-lookup"><span data-stu-id="e66f2-107">See [Inheritance (Relational Database)](relational/inheritance.md) for how this is handled with a relational database provider.</span></span>

<span data-ttu-id="e66f2-108">EF nastaví dědění pouze v případě, že jsou do modelu explicitně zahrnuty dva nebo více děděných typů.</span><span class="sxs-lookup"><span data-stu-id="e66f2-108">EF will only setup inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="e66f2-109">EF nebude hledat základní nebo odvozené typy, které nebyly jinak zahrnuty v modelu.</span><span class="sxs-lookup"><span data-stu-id="e66f2-109">EF will not scan for base or derived types that were not otherwise included in the model.</span></span> <span data-ttu-id="e66f2-110">Do modelu můžete zahrnout typy tím, že vystavíte *negenerickými\<TEntity >* pro každý typ v hierarchii dědičnosti.</span><span class="sxs-lookup"><span data-stu-id="e66f2-110">You can include types in the model by exposing a *DbSet\<TEntity>* for each type in the inheritance hierarchy.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?highlight=3-4&name=Model)]

<span data-ttu-id="e66f2-111">Pokud nechcete zveřejnit *negenerickými\<TEntity >* pro jednu nebo více entit v hierarchii, můžete použít rozhraní Fluent API, abyste zajistili, že jsou zahrnuté v modelu.</span><span class="sxs-lookup"><span data-stu-id="e66f2-111">If you don't want to expose a *DbSet\<TEntity>* for one or more entities in the hierarchy, you can use the Fluent API to ensure they are included in the model.</span></span>
<span data-ttu-id="e66f2-112">A pokud nespoléháte na konvence, můžete základní typ explicitně zadat pomocí `HasBaseType`.</span><span class="sxs-lookup"><span data-stu-id="e66f2-112">And if you don't rely on conventions, you can specify the base type explicitly using `HasBaseType`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> <span data-ttu-id="e66f2-113">Pomocí `.HasBaseType((Type)null)` můžete z hierarchie odebrat typ entity.</span><span class="sxs-lookup"><span data-stu-id="e66f2-113">You can use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="e66f2-114">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="e66f2-114">Data Annotations</span></span>

<span data-ttu-id="e66f2-115">Ke konfiguraci dědičnosti nelze použít datové poznámky.</span><span class="sxs-lookup"><span data-stu-id="e66f2-115">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="e66f2-116">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="e66f2-116">Fluent API</span></span>

<span data-ttu-id="e66f2-117">Rozhraní API Fluent pro dědění závisí na použitém poskytovateli databáze.</span><span class="sxs-lookup"><span data-stu-id="e66f2-117">The Fluent API for inheritance depends on the database provider you are using.</span></span> <span data-ttu-id="e66f2-118">Viz téma [Dědičnost (relační databáze)](relational/inheritance.md) pro konfiguraci, kterou můžete provést pro poskytovatele relační databáze.</span><span class="sxs-lookup"><span data-stu-id="e66f2-118">See [Inheritance (Relational Database)](relational/inheritance.md) for the configuration you can perform for a relational database provider.</span></span>
