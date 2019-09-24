---
title: Maximální délka – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
uid: core/modeling/max-length
ms.openlocfilehash: b6f0594fed0c491b4f79dcda5273cdebe9ecf35f
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197222"
---
# <a name="maximum-length"></a>Maximální délka

Konfigurace maximální délky poskytuje nápovědu pro úložiště dat o vhodném datovém typu, který se má pro danou vlastnost použít. Maximální délka se vztahuje pouze na datové typy pole, například `string` a `byte[]`.

> [!NOTE]  
> Entity Framework neprovádí žádné ověření maximální délky před předáním dat poskytovateli. Pokud je to vhodné, je až do poskytovatele nebo úložiště dat. Například při cílení na SQL Server, což překračuje maximální délku, dojde k výjimce, protože datový typ podkladového sloupce nebude moci ukládat nadbytečné údaje.

## <a name="conventions"></a>Konvence

Podle konvence je ponecháno až od poskytovatele databáze, aby bylo možné zvolit vhodný datový typ pro vlastnosti. U vlastností, které mají délku, bude poskytovatel databáze obecně vybírat datový typ, který umožňuje nejdelší délku dat. Například Microsoft SQL Server budou použity `nvarchar(max)` pro `string` vlastnosti (nebo `nvarchar(450)` Pokud je sloupec použit jako klíč).

## <a name="data-annotations"></a>Datové poznámky

Pomocí datových poznámek můžete nakonfigurovat maximální délku vlastnosti. V tomto příkladu, který cílí na SQL Server, by to `nvarchar(500)` vedlo k použití datového typu.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/MaxLength.cs?highlight=14)]

## <a name="fluent-api"></a>Rozhraní Fluent API

K nakonfigurování maximální délky vlastnosti můžete použít rozhraní Fluent API. V tomto příkladu, který cílí na SQL Server, by to `nvarchar(500)` vedlo k použití datového typu.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/MaxLength.cs?highlight=11-13)]
