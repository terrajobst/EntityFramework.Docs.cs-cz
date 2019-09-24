---
title: Dědičnost – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: 1f20c455176b5922364584f8c7688c15a4c3f0f9
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197283"
---
# <a name="inheritance"></a><span data-ttu-id="88878-102">Dědičnost</span><span class="sxs-lookup"><span data-stu-id="88878-102">Inheritance</span></span>

<span data-ttu-id="88878-103">Dědičnost v modelu EF se používá k řízení způsobu reprezentace dědičnosti tříd entit v databázi.</span><span class="sxs-lookup"><span data-stu-id="88878-103">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="88878-104">Konvence</span><span class="sxs-lookup"><span data-stu-id="88878-104">Conventions</span></span>

<span data-ttu-id="88878-105">Podle konvence je až od poskytovatele databáze k určení, jak bude dědění v databázi reprezentované.</span><span class="sxs-lookup"><span data-stu-id="88878-105">By convention, it is up to the database provider to determine how inheritance will be represented in the database.</span></span> <span data-ttu-id="88878-106">Podívejte se na téma [Dědičnost (relační databáze)](relational/inheritance.md) , ve kterém se tato funkce zpracovává u poskytovatele relační databáze.</span><span class="sxs-lookup"><span data-stu-id="88878-106">See [Inheritance (Relational Database)](relational/inheritance.md) for how this is handled with a relational database provider.</span></span>

<span data-ttu-id="88878-107">EF nastaví dědění pouze v případě, že jsou do modelu explicitně zahrnuty dva nebo více děděných typů.</span><span class="sxs-lookup"><span data-stu-id="88878-107">EF will only setup inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="88878-108">EF nebude hledat základní nebo odvozené typy, které nebyly jinak zahrnuty v modelu.</span><span class="sxs-lookup"><span data-stu-id="88878-108">EF will not scan for base or derived types that were not otherwise included in the model.</span></span> <span data-ttu-id="88878-109">Do modelu můžete zahrnout typy tím, že vystavíte *negenerickými<TEntity>*  pro každý typ v hierarchii dědičnosti.</span><span class="sxs-lookup"><span data-stu-id="88878-109">You can include types in the model by exposing a *DbSet<TEntity>* for each type in the inheritance hierarchy.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?highlight=3-4&name=Model)]

<span data-ttu-id="88878-110">Pokud nechcete zveřejnit *negenerickými<TEntity>*  pro jednu nebo více entit v hierarchii, můžete použít rozhraní Fluent API, abyste zajistili, že jsou zahrnuté v modelu.</span><span class="sxs-lookup"><span data-stu-id="88878-110">If you don't want to expose a *DbSet<TEntity>* for one or more entities in the hierarchy, you can use the Fluent API to ensure they are included in the model.</span></span>
<span data-ttu-id="88878-111">A pokud nespoléháte na konvence, můžete základní typ zadat explicitně pomocí `HasBaseType`.</span><span class="sxs-lookup"><span data-stu-id="88878-111">And if you don't rely on conventions, you can specify the base type explicitly using `HasBaseType`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> <span data-ttu-id="88878-112">Můžete použít `.HasBaseType((Type)null)` k odebrání typu entity z hierarchie.</span><span class="sxs-lookup"><span data-stu-id="88878-112">You can use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="88878-113">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="88878-113">Data Annotations</span></span>

<span data-ttu-id="88878-114">Ke konfiguraci dědičnosti nelze použít datové poznámky.</span><span class="sxs-lookup"><span data-stu-id="88878-114">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="88878-115">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="88878-115">Fluent API</span></span>

<span data-ttu-id="88878-116">Rozhraní API Fluent pro dědění závisí na použitém poskytovateli databáze.</span><span class="sxs-lookup"><span data-stu-id="88878-116">The Fluent API for inheritance depends on the database provider you are using.</span></span> <span data-ttu-id="88878-117">Viz téma [Dědičnost (relační databáze)](relational/inheritance.md) pro konfiguraci, kterou můžete provést pro poskytovatele relační databáze.</span><span class="sxs-lookup"><span data-stu-id="88878-117">See [Inheritance (Relational Database)](relational/inheritance.md) for the configuration you can perform for a relational database provider.</span></span>
