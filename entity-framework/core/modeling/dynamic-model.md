---
title: Přepínání mezi více modelů pomocí stejného DbContext typu – EF Core
author: AndriySvyryd
ms.date: 12/10/2017
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/dynamic-model
ms.openlocfilehash: 1d87efb668c12a2862583fba16a6c444b3cda9de
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994983"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a><span data-ttu-id="9ac15-102">Přepínání mezi více modelů se stejným typem DbContext</span><span class="sxs-lookup"><span data-stu-id="9ac15-102">Alternating between multiple models with the same DbContext type</span></span>

<span data-ttu-id="9ac15-103">Model sestavený `OnModelCreating` použít vlastnost na kontextu, chcete-li změnit, jak je sestaven modelu.</span><span class="sxs-lookup"><span data-stu-id="9ac15-103">The model built in `OnModelCreating` could use a property on the context to change how the model is built.</span></span> <span data-ttu-id="9ac15-104">Například to může být vyloučena určitá vlastnost:</span><span class="sxs-lookup"><span data-stu-id="9ac15-104">For example it could be used to exclude a certain property:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a><span data-ttu-id="9ac15-105">IModelCacheKeyFactory</span><span class="sxs-lookup"><span data-stu-id="9ac15-105">IModelCacheKeyFactory</span></span>
<span data-ttu-id="9ac15-106">Ale pokud jste se pokusili, díky předchozímu bez jakýchkoli dalších změn byste získali stejného modelu pokaždé, když se vytvoří nový kontext pro libovolnou hodnotu z `IgnoreIntProperty`.</span><span class="sxs-lookup"><span data-stu-id="9ac15-106">However if you tried doing the above without additional changes you would get the same model every time a new context is created for any value of `IgnoreIntProperty`.</span></span> <span data-ttu-id="9ac15-107">Důvodem je ukládání do mezipaměti mechanismus EF používá ke zlepšení výkonu pouze vyvoláním modelu `OnModelCreating` jednou a ukládání do mezipaměti modelu.</span><span class="sxs-lookup"><span data-stu-id="9ac15-107">This is caused by the model caching mechanism EF uses to improve the performance by only invoking `OnModelCreating` once and caching the model.</span></span>

<span data-ttu-id="9ac15-108">Ve výchozím nastavení EF předpokládá, že pro jakýkoli kontext daného typu modelu budou stejné.</span><span class="sxs-lookup"><span data-stu-id="9ac15-108">By default EF assumes that for any given context type the model will be the same.</span></span> <span data-ttu-id="9ac15-109">K provedení této výchozí implementace `IModelCacheKeyFactory` vrátí klíč, který obsahuje pouze typ kontextu.</span><span class="sxs-lookup"><span data-stu-id="9ac15-109">To accomplish this the default implementation of `IModelCacheKeyFactory` returns a key that just contains the context type.</span></span> <span data-ttu-id="9ac15-110">Chcete-li změnit toto je třeba nahradit `IModelCacheKeyFactory` služby.</span><span class="sxs-lookup"><span data-stu-id="9ac15-110">To change this you need to replace the `IModelCacheKeyFactory` service.</span></span> <span data-ttu-id="9ac15-111">Novou implementaci musí vracet objekt, který lze porovnávat na jiné modelu klíče pomocí `Equals` metodu, která bere v úvahu všechny proměnné, které mají vliv modelu:</span><span class="sxs-lookup"><span data-stu-id="9ac15-111">The new implementation needs to return an object that can be compared to other model keys using the `Equals` method that takes into account all the variables that affect the model:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
