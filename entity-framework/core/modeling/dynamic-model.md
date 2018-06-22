---
title: Střídání mezi více modely stejného typu DbContext - EF jádra
author: AndriySvyryd
ms.author: divega
ms.date: 12/10/2017
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/dynamic-model
ms.openlocfilehash: 57136802001124ebf9ae7682e33f8dc7826fc2b0
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678719"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a><span data-ttu-id="ca00e-102">Střídání mezi více modely stejného typu DbContext</span><span class="sxs-lookup"><span data-stu-id="ca00e-102">Alternating between multiple models with the same DbContext type</span></span>

<span data-ttu-id="ca00e-103">Model sestavený `OnModelCreating` může používat vlastnost pro daný kontext změnit, jak je založená modelu.</span><span class="sxs-lookup"><span data-stu-id="ca00e-103">The model built in `OnModelCreating` could use a property on the context to change how the model is built.</span></span> <span data-ttu-id="ca00e-104">Například je může být vyloučena určitá vlastnost:</span><span class="sxs-lookup"><span data-stu-id="ca00e-104">For example it could be used to exclude a certain property:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a><span data-ttu-id="ca00e-105">IModelCacheKeyFactory</span><span class="sxs-lookup"><span data-stu-id="ca00e-105">IModelCacheKeyFactory</span></span>
<span data-ttu-id="ca00e-106">Ale pokud jste se pokusili provádění výše bez další změny, které byste získali stejný model pokaždé, když se vytvoří nový kontext pro žádnou hodnotu z `IgnoreIntProperty`.</span><span class="sxs-lookup"><span data-stu-id="ca00e-106">However if you tried doing the above without additional changes you would get the same model every time a new context is created for any value of `IgnoreIntProperty`.</span></span> <span data-ttu-id="ca00e-107">Je to způsobeno modelu ukládání do mezipaměti mechanismus EF používá ke zlepšení výkonu pouze vyvoláním `OnModelCreating` jednou a ukládání do mezipaměti v modelu.</span><span class="sxs-lookup"><span data-stu-id="ca00e-107">This is caused by the model caching mechanism EF uses to improve the performance by only invoking `OnModelCreating` once and caching the model.</span></span>

<span data-ttu-id="ca00e-108">Ve výchozím nastavení EF předpokládá, že pro jakýkoli kontext daného typu modelu budou stejné.</span><span class="sxs-lookup"><span data-stu-id="ca00e-108">By default EF assumes that for any given context type the model will be the same.</span></span> <span data-ttu-id="ca00e-109">K tomu výchozí implementaci `IModelCacheKeyFactory` vrátí klíč, který právě obsahuje typ kontextu.</span><span class="sxs-lookup"><span data-stu-id="ca00e-109">To accomplish this the default implementation of `IModelCacheKeyFactory` returns a key that just contains the context type.</span></span> <span data-ttu-id="ca00e-110">Chcete-li změnit toto je třeba nahradit `IModelCacheKeyFactory` služby.</span><span class="sxs-lookup"><span data-stu-id="ca00e-110">To change this you need to replace the `IModelCacheKeyFactory` service.</span></span> <span data-ttu-id="ca00e-111">Novou implementací musí vrátit objekt, který je možné porovnávat do jiných modelu klíče pomocí `Equals` metoda, která bere v úvahu všechny proměnné, které ovlivňují modelu:</span><span class="sxs-lookup"><span data-stu-id="ca00e-111">The new implementation needs to return an object that can be compared to other model keys using the `Equals` method that takes into account all the variables that affect the model:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
