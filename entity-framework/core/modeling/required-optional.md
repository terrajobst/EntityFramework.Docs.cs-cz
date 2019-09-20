---
title: Požadované/volitelné vlastnosti – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: 7200cd2eeeba2f22365ef09b1f50edd077240130
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149153"
---
# <a name="required-and-optional-properties"></a>Povinné a volitelné vlastnosti

Vlastnost je považována za volitelnou, pokud je platná pro její `null`zahrnutí. Pokud `null` není platná hodnota, která má být přiřazena vlastnosti, je považována za požadovanou vlastnost.

## <a name="conventions"></a>Konvence

Podle konvence vlastnost, jejíž typ .NET může obsahovat hodnotu null, bude nakonfigurována jako volitelná `int?`( `byte[]``string`,, atd.). Vlastnosti, jejichž typ CLR nemůže obsahovat hodnotu null, budou nakonfigurovány`int`jako `decimal`povinné `bool`(,, atd.).

> [!NOTE]  
> Vlastnost, jejíž typ .NET nemůže obsahovat hodnotu null, nelze nakonfigurovat jako volitelnou. Vlastnost bude vždy posouzena jako požadovaná Entity Framework.

## <a name="data-annotations"></a>Datové poznámky

Můžete použít datové poznámky k indikaci, že vlastnost je povinná.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=14)]

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít k indikaci, že vlastnost je povinná.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=11-13)]

