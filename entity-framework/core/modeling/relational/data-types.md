---
title: Datové typy – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
uid: core/modeling/relational/data-types
ms.openlocfilehash: 9060f66c752be01090ce40be6bf3a32f348ce571
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993518"
---
# <a name="data-types"></a>Datové typy

> [!NOTE]  
> Obecně se vztahuje k relačním databázím konfigurace v této části. Metody rozšíření je vidět tady bude k dispozici při instalaci poskytovatele relační databáze (z důvodu sdílený *Microsoft.EntityFrameworkCore.Relational* balíček).

Datový typ odkazuje na konkrétní typ databáze sloupce, ke kterému je namapována vlastnost.

## <a name="conventions"></a>Konvence

Podle konvence vybere poskytovatele databáze datový typ na základě typu CLR vlastnosti. Také bere v úvahu další metadata, například nakonfigurovaným [maximální délku](../max-length.md), zda vlastnost je částí primárního klíče, atd.

Například SQL Server používá `datetime2(7)` pro `DateTime` vlastnosti, a `nvarchar(max)` pro `string` vlastnosti (nebo `nvarchar(450)` pro `string` vlastnosti, které slouží jako klíč).

## <a name="data-annotations"></a>Datové poznámky

Datové poznámky můžete zadat přesný datový typ pro sloupec.

Například následující kód konfiguruje `Url` jako kódování unicode řetězec s maximální délkou `200` a `Rating` jako desetinné přesnosti `5` a škálovat z `2`.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít také k určení stejné datové typy sloupců.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
