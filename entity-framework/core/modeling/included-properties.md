---
title: Zahrnutí a vyloučení vlastností – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/included-properties
ms.openlocfilehash: 022534091bb48e491c8808791a401216a339d7b0
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929820"
---
# <a name="including--excluding-properties"></a>Zahrnutí a vyloučení vlastností

Včetně vlastnost v modelu znamená, že EF má metadata o tuto vlastnost a pokusí se čtení a zápis dat z/do databáze.

## <a name="conventions"></a>Konvence

Podle konvence se zahrnou veřejné vlastnosti metody getter a setter v modelu.

## <a name="data-annotations"></a>Datové poznámky

Datové poznámky můžete být z modelu vyloučena určitá vlastnost.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/IgnoreProperty.cs?highlight=17)]

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete být z modelu vyloučena určitá vlastnost.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/IgnoreProperty.cs?highlight=12,13)]
