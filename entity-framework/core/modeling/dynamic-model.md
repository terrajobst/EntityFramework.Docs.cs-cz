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
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>Střídání mezi několika modely se stejným typem DbContext

Model sestavený `OnModelCreating` mohl použít vlastnost v kontextu ke změně způsobu sestavení modelu. Můžete například použít k vyloučení určité vlastnosti:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a>IModelCacheKeyFactory

Nicméně pokud jste se pokusili provést výše uvedené bez dalších změn, měli byste stejný model pokaždé, když se vytvoří nový kontext pro libovolnou hodnotu `IgnoreIntProperty`. To je způsobeno tím, že nástroj pro ukládání modelů do mezipaměti EF používá ke zlepšení výkonu pouze vyvoláním `OnModelCreating` jednou a ukládáním do mezipaměti modelu.

Ve výchozím nastavení předpokládá, že pro jakýkoli daný typ kontextu bude model stejný. Chcete-li dosáhnout této výchozí implementace `IModelCacheKeyFactory` vrátí klíč, který pouze obsahuje typ kontextu. Chcete-li toto změnit, je třeba nahradit službu `IModelCacheKeyFactory`. Nová implementace vyžaduje vrácení objektu, který lze porovnat s jinými klíči modelů pomocí metody `Equals`, která přihlíží ke všem proměnným, které mají vliv na model:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
