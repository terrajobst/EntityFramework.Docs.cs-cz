---
title: Dědičnost – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: c5fa9d13dec8cfc3e1cac69e471f509cbbb9e4c5
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995893"
---
# <a name="inheritance"></a><span data-ttu-id="dbfae-102">Dědičnost</span><span class="sxs-lookup"><span data-stu-id="dbfae-102">Inheritance</span></span>

<span data-ttu-id="dbfae-103">Dědičnost v modelu EF slouží k řízení, jak je reprezentovaná dědičnosti tříd entit v databázi.</span><span class="sxs-lookup"><span data-stu-id="dbfae-103">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="dbfae-104">Konvence</span><span class="sxs-lookup"><span data-stu-id="dbfae-104">Conventions</span></span>

<span data-ttu-id="dbfae-105">Podle konvence je až poskytovatel databáze k určení, jak se dědičnost reprezentovat v databázi.</span><span class="sxs-lookup"><span data-stu-id="dbfae-105">By convention, it is up to the database provider to determine how inheritance will be represented in the database.</span></span> <span data-ttu-id="dbfae-106">Zobrazit [dědičnost (relační databáze)](relational/inheritance.md) pro jak tento problém řeší poskytovatele relační databáze.</span><span class="sxs-lookup"><span data-stu-id="dbfae-106">See [Inheritance (Relational Database)](relational/inheritance.md) for how this is handled with a relational database provider.</span></span>

<span data-ttu-id="dbfae-107">EF bude pouze nastavení dědičnosti, dvou nebo více zděděných typů je explicitně zahrnuta v modelu.</span><span class="sxs-lookup"><span data-stu-id="dbfae-107">EF will only setup inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="dbfae-108">EF nebudou zjišťovat základní nebo odvozené typy, které jinak nejsou zahrnuty v modelu.</span><span class="sxs-lookup"><span data-stu-id="dbfae-108">EF will not scan for base or derived types that were not otherwise included in the model.</span></span> <span data-ttu-id="dbfae-109">Může obsahovat typy v modelu zveřejněním *DbSet<TEntity>*  pro každý typ v hierarchii dědičnosti.</span><span class="sxs-lookup"><span data-stu-id="dbfae-109">You can include types in the model by exposing a *DbSet<TEntity>* for each type in the inheritance hierarchy.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceDbSets.cs?highlight=3-4&name=Model)]

<span data-ttu-id="dbfae-110">Pokud už nechcete vystavit *DbSet<TEntity>*  pro jeden nebo více entit v hierarchii, můžete použít rozhraní Fluent API zajistit jsou zahrnuty v modelu.</span><span class="sxs-lookup"><span data-stu-id="dbfae-110">If you don't want to expose a *DbSet<TEntity>* for one or more entities in the hierarchy, you can use the Fluent API to ensure they are included in the model.</span></span>
<span data-ttu-id="dbfae-111">A pokud nemusíte spoléhat na vytváření názvů můžete určit základní typ explicitně pomocí `HasBaseType`.</span><span class="sxs-lookup"><span data-stu-id="dbfae-111">And if you don't rely on conventions you can specify the base type explicitly using `HasBaseType`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> <span data-ttu-id="dbfae-112">Můžete použít `.HasBaseType((Type)null)` odebrat z hierarchie typu entity.</span><span class="sxs-lookup"><span data-stu-id="dbfae-112">You can use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="dbfae-113">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="dbfae-113">Data Annotations</span></span>

<span data-ttu-id="dbfae-114">Datové poznámky nelze použít ke konfiguraci dědičnosti.</span><span class="sxs-lookup"><span data-stu-id="dbfae-114">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="dbfae-115">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="dbfae-115">Fluent API</span></span>

<span data-ttu-id="dbfae-116">Rozhraní Fluent API dědičnosti závisí na poskytovateli databáze, které používáte.</span><span class="sxs-lookup"><span data-stu-id="dbfae-116">The Fluent API for inheritance depends on the database provider you are using.</span></span> <span data-ttu-id="dbfae-117">Zobrazit [dědičnost (relační databáze)](relational/inheritance.md) pro konfiguraci můžete provést pro poskytovatele relační databáze.</span><span class="sxs-lookup"><span data-stu-id="dbfae-117">See [Inheritance (Relational Database)](relational/inheritance.md) for the configuration you can perform for a relational database provider.</span></span>
