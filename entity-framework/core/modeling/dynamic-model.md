---
title: "Střídání mezi více modely stejného typu DbContext - EF jádra"
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
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>Střídání mezi více modely stejného typu DbContext

Model sestavený `OnModelCreating` může používat vlastnost pro daný kontext změnit, jak je založená modelu. Například je může být vyloučena určitá vlastnost:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a>IModelCacheKeyFactory
Ale pokud jste se pokusili provádění výše bez další změny, které byste získali stejný model pokaždé, když se vytvoří nový kontext pro žádnou hodnotu z `IgnoreIntProperty`. Je to způsobeno modelu ukládání do mezipaměti mechanismus EF používá ke zlepšení výkonu pouze vyvoláním `OnModelCreating` jednou a ukládání do mezipaměti v modelu.

Ve výchozím nastavení EF předpokládá, že pro jakýkoli kontext daného typu modelu budou stejné. K tomu výchozí implementaci `IModelCacheKeyFactory` vrátí klíč, který právě obsahuje typ kontextu. Chcete-li změnit toto je třeba nahradit `IModelCacheKeyFactory` služby. Novou implementací musí vrátit objekt, který je možné porovnávat do jiných modelu klíče pomocí `Equals` metoda, která bere v úvahu všechny proměnné, které ovlivňují modelu:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
