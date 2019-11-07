---
title: Střídání mezi několika modely se stejným typem DbContext-EF Core
author: AndriySvyryd
ms.date: 12/10/2017
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/dynamic-model
ms.openlocfilehash: 034076b1595894e80b98467354f6c9f139bd7426
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655728"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a><span data-ttu-id="0562f-102">Střídání mezi několika modely se stejným typem DbContext</span><span class="sxs-lookup"><span data-stu-id="0562f-102">Alternating between multiple models with the same DbContext type</span></span>

<span data-ttu-id="0562f-103">Model sestavený `OnModelCreating` mohl použít vlastnost v kontextu ke změně způsobu sestavení modelu.</span><span class="sxs-lookup"><span data-stu-id="0562f-103">The model built in `OnModelCreating` could use a property on the context to change how the model is built.</span></span> <span data-ttu-id="0562f-104">Můžete například použít k vyloučení určité vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="0562f-104">For example it could be used to exclude a certain property:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a><span data-ttu-id="0562f-105">IModelCacheKeyFactory</span><span class="sxs-lookup"><span data-stu-id="0562f-105">IModelCacheKeyFactory</span></span>

<span data-ttu-id="0562f-106">Nicméně pokud jste se pokusili provést výše uvedené bez dalších změn, měli byste stejný model pokaždé, když se vytvoří nový kontext pro libovolnou hodnotu `IgnoreIntProperty`.</span><span class="sxs-lookup"><span data-stu-id="0562f-106">However if you tried doing the above without additional changes you would get the same model every time a new context is created for any value of `IgnoreIntProperty`.</span></span> <span data-ttu-id="0562f-107">To je způsobeno tím, že nástroj pro ukládání modelů do mezipaměti EF používá ke zlepšení výkonu pouze vyvoláním `OnModelCreating` jednou a ukládáním do mezipaměti modelu.</span><span class="sxs-lookup"><span data-stu-id="0562f-107">This is caused by the model caching mechanism EF uses to improve the performance by only invoking `OnModelCreating` once and caching the model.</span></span>

<span data-ttu-id="0562f-108">Ve výchozím nastavení předpokládá, že pro jakýkoli daný typ kontextu bude model stejný.</span><span class="sxs-lookup"><span data-stu-id="0562f-108">By default EF assumes that for any given context type the model will be the same.</span></span> <span data-ttu-id="0562f-109">Chcete-li dosáhnout této výchozí implementace `IModelCacheKeyFactory` vrátí klíč, který pouze obsahuje typ kontextu.</span><span class="sxs-lookup"><span data-stu-id="0562f-109">To accomplish this the default implementation of `IModelCacheKeyFactory` returns a key that just contains the context type.</span></span> <span data-ttu-id="0562f-110">Chcete-li toto změnit, je třeba nahradit službu `IModelCacheKeyFactory`.</span><span class="sxs-lookup"><span data-stu-id="0562f-110">To change this you need to replace the `IModelCacheKeyFactory` service.</span></span> <span data-ttu-id="0562f-111">Nová implementace vyžaduje vrácení objektu, který lze porovnat s jinými klíči modelů pomocí metody `Equals`, která přihlíží ke všem proměnným, které mají vliv na model:</span><span class="sxs-lookup"><span data-stu-id="0562f-111">The new implementation needs to return an object that can be compared to other model keys using the `Equals` method that takes into account all the variables that affect the model:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
