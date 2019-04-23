---
title: Požadované a volitelné vlastnosti – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: 564d9e62e2ed4f1a52b569630ed4994529e31dc1
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929807"
---
# <a name="required-and-optional-properties"></a>Požadované a volitelné vlastnosti

Vlastnost se považuje za nepovinné, pokud je platný pro něj tak, aby obsahovala `null`. Pokud `null` není platná hodnota pro přiřazení vlastnosti a má se za to se vyžaduje vlastnost.

## <a name="conventions"></a>Konvence

Podle konvence, vlastnost, jejíž typ CLR může obsahovat hodnotu null se nakonfigurují jako volitelné (`string`, `int?`, `byte[]`atd.). Vlastnosti, jejíž typ CLR nesmí obsahovat hodnotu null se nakonfigurují podle potřeby (`int`, `decimal`, `bool`atd.).

> [!NOTE]  
> Vlastnost, jejíž typ CLR nemůže obsahovat hodnotu null nejde nakonfigurovat jako volitelné. Vlastnost budou vždy považovat za vyžadovány rozhraním Entity Framework.

## <a name="data-annotations"></a>Datové poznámky

Anotací dat můžete použít k označení, že je vyžadována určitá vlastnost.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=14)]

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít k označení, že je vyžadována určitá vlastnost.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=11-13)]

