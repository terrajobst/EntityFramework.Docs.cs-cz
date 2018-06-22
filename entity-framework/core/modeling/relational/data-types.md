---
title: Datové typy - EF jádra
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
ms.technology: entity-framework-core
uid: core/modeling/relational/data-types
ms.openlocfilehash: fd4668a3f9554eb9d3b1161d5dddce2fcdcac712
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/05/2017
ms.locfileid: "26054544"
---
# <a name="data-types"></a>Datové typy

> [!NOTE]  
> Obecně se vztahuje na relační databáze konfigurace v této části. Rozšiřující metody zobrazeny zde bude k dispozici při instalaci poskytovatele relační databáze (z důvodu sdílený *Microsoft.EntityFrameworkCore.Relational* balíčku).

Datový typ odkazuje na konkrétní typ databáze sloupce, na kterou vlastnost namapovaná.

## <a name="conventions"></a>Konvence

Dle konvencí se vybere zprostředkovatel databáze typu dat na základě typu CLR vlastnosti. Také bere v úvahu další metadata, například nakonfigurované [maximální délku](../max-length.md), zda vlastnost je částí primární klíč atd.

Například používá systém SQL Server `datetime2(7)` pro `DateTime` vlastnosti, a `nvarchar(max)` pro `string` vlastnosti (nebo `nvarchar(450)` pro `string` vlastnosti, které se používají jako klíč).

## <a name="data-annotations"></a>Datových poznámek

Můžete zadat přesný datový typ pro sloupec datových poznámek.

Například následující kód konfiguruje `Url` jako řetězec kódování unicode s maximální délkou `200` a `Rating` jako desetinné přesnosti `5` a škálovat z `2`.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít také k určení stejné typy dat sloupců.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
