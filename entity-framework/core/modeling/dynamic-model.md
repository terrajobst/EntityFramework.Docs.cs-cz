---
title: "Střídání mezi více modely stejného typu DbContext - EF jádra"
author: AndriySvyryd
uid: core/modeling/dynamic-model
ms.openlocfilehash: e6828c62c55ae6f48d46a20528268264c3f22b26
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>Střídání mezi více modely stejného typu DbContext

Model sestavený `OnModelCreating` může používat vlastnost pro daný kontext změnit, jak je založená modelu. Například je může být vyloučena určitá vlastnost:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a>IModelCacheKeyFactory
Ale pokud jste se pokusili provádění výše bez další změny, které byste získali stejný model pokaždé, když se vytvoří nový kontext pro žádnou hodnotu z `IgnoreIntProperty`. Je to způsobeno modelu ukládání do mezipaměti mechanismus EF používá ke zlepšení výkonu pouze vyvoláním `OnModelCreating` jednou a ukládání do mezipaměti v modelu.

Ve výchozím nastavení EF předpokládá, že pro jakýkoli kontext daného typu modelu budou stejné. K tomu výchozí implementaci `IModelCacheKeyFactory` vrátí klíč, který právě obsahuje typ kontextu. Chcete-li změnit toto je třeba nahradit `IModelCacheKeyFactory` služby. Novou implementací musí vrátit objekt, který je možné porovnávat do jiných modelu klíče pomocí `Equals` metoda, která bere v úvahu všechny proměnné, které ovlivňují modelu:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
