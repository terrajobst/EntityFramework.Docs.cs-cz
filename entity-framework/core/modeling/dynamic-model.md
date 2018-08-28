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
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>Přepínání mezi více modelů se stejným typem DbContext

Model sestavený `OnModelCreating` použít vlastnost na kontextu, chcete-li změnit, jak je sestaven modelu. Například to může být vyloučena určitá vlastnost:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a>IModelCacheKeyFactory
Ale pokud jste se pokusili, díky předchozímu bez jakýchkoli dalších změn byste získali stejného modelu pokaždé, když se vytvoří nový kontext pro libovolnou hodnotu z `IgnoreIntProperty`. Důvodem je ukládání do mezipaměti mechanismus EF používá ke zlepšení výkonu pouze vyvoláním modelu `OnModelCreating` jednou a ukládání do mezipaměti modelu.

Ve výchozím nastavení EF předpokládá, že pro jakýkoli kontext daného typu modelu budou stejné. K provedení této výchozí implementace `IModelCacheKeyFactory` vrátí klíč, který obsahuje pouze typ kontextu. Chcete-li změnit toto je třeba nahradit `IModelCacheKeyFactory` služby. Novou implementaci musí vracet objekt, který lze porovnávat na jiné modelu klíče pomocí `Equals` metodu, která bere v úvahu všechny proměnné, které mají vliv modelu:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
