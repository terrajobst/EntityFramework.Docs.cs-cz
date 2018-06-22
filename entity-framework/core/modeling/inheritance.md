---
title: Dědičnost – EF jádra
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
ms.technology: entity-framework-core
uid: core/modeling/inheritance
ms.openlocfilehash: f0394bc55dfbfea8277b1ddf898361165dd1fe51
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054196"
---
# <a name="inheritance"></a><span data-ttu-id="611f9-102">Dědičnost</span><span class="sxs-lookup"><span data-stu-id="611f9-102">Inheritance</span></span>

<span data-ttu-id="611f9-103">Dědičnost ve EF model slouží k řízení, jak je reprezentována dědičnosti ve třídách entity v databázi.</span><span class="sxs-lookup"><span data-stu-id="611f9-103">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="611f9-104">Konvence</span><span class="sxs-lookup"><span data-stu-id="611f9-104">Conventions</span></span>

<span data-ttu-id="611f9-105">Podle konvence je zprostředkovatel databáze k určení, jak se dědičnost nelze v databázi.</span><span class="sxs-lookup"><span data-stu-id="611f9-105">By convention, it is up to the database provider to determine how inheritance will be represented in the database.</span></span> <span data-ttu-id="611f9-106">V tématu [dědičnosti (relační databáze)](relational/inheritance.md) pro toto zpracování s poskytovatelem relační databáze.</span><span class="sxs-lookup"><span data-stu-id="611f9-106">See [Inheritance (Relational Database)](relational/inheritance.md) for how this is handled with a relational database provider.</span></span>

<span data-ttu-id="611f9-107">EF bude pouze nastavení dědičnosti, pokud dva nebo více typů zděděné jsou explicitně součástí modelu.</span><span class="sxs-lookup"><span data-stu-id="611f9-107">EF will only setup inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="611f9-108">EF nebudou kontrolovat pro základní nebo odvozené typy, které nebyly jinak součástí modelu.</span><span class="sxs-lookup"><span data-stu-id="611f9-108">EF will not scan for base or derived types that were not otherwise included in the model.</span></span> <span data-ttu-id="611f9-109">Můžete zahrnout typy v modelu vystavení *DbSet<TEntity>*  pro každý typ v hierarchii dědičnosti.</span><span class="sxs-lookup"><span data-stu-id="611f9-109">You can include types in the model by exposing a *DbSet<TEntity>* for each type in the inheritance hierarchy.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceDbSets.cs?highlight=3-4&name=Model)]

<span data-ttu-id="611f9-110">Pokud nechcete, aby ke zveřejnění *DbSet<TEntity>*  pro jeden nebo více entit v hierarchii, můžete použít rozhraní Fluent API zajistit jsou zahrnuty do modelu.</span><span class="sxs-lookup"><span data-stu-id="611f9-110">If you don't want to expose a *DbSet<TEntity>* for one or more entities in the hierarchy, you can use the Fluent API to ensure they are included in the model.</span></span>
<span data-ttu-id="611f9-111">A pokud nemáte spoléhají na konvence můžete určit základní typ explicitně pomocí `HasBaseType`.</span><span class="sxs-lookup"><span data-stu-id="611f9-111">And if you don't rely on conventions you can specify the base type explicitly using `HasBaseType`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> <span data-ttu-id="611f9-112">Můžete použít `.HasBaseType((Type)null)` na typ entity z hierarchie odebrat.</span><span class="sxs-lookup"><span data-stu-id="611f9-112">You can use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="611f9-113">Datových poznámek</span><span class="sxs-lookup"><span data-stu-id="611f9-113">Data Annotations</span></span>

<span data-ttu-id="611f9-114">Poznámky dat nelze použít ke konfiguraci dědičnosti.</span><span class="sxs-lookup"><span data-stu-id="611f9-114">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="611f9-115">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="611f9-115">Fluent API</span></span>

<span data-ttu-id="611f9-116">Rozhraní Fluent API pro dědičnosti závisí na poskytovateli databáze, kterou používáte.</span><span class="sxs-lookup"><span data-stu-id="611f9-116">The Fluent API for inheritance depends on the database provider you are using.</span></span> <span data-ttu-id="611f9-117">V tématu [dědičnosti (relační databáze)](relational/inheritance.md) konfigurace můžete provést pro poskytovatele relační databáze.</span><span class="sxs-lookup"><span data-stu-id="611f9-117">See [Inheritance (Relational Database)](relational/inheritance.md) for the configuration you can perform for a relational database provider.</span></span>
