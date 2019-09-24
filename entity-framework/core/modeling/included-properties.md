---
title: Zahrnutí & s výjimkou vlastností – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/included-properties
ms.openlocfilehash: cd111af891ef0bbaccf515eed0c1991f105bd362
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197423"
---
# <a name="including--excluding-properties"></a>Zahrnutí a vyloučení vlastností

Zahrnutím vlastnosti v modelu znamená, že EF má metadata o této vlastnosti a pokusí se o čtení a zápis hodnot z databáze.

## <a name="conventions"></a>Konvence

Podle konvence se do modelu zahrne veřejné vlastnosti s mechanismem getter a setter.

## <a name="data-annotations"></a>Datové poznámky

K vyloučení vlastnosti z modelu můžete použít datové poznámky.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreProperty.cs?highlight=17)]

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít k vyloučení vlastnosti z modelu.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreProperty.cs?highlight=12,13)]
