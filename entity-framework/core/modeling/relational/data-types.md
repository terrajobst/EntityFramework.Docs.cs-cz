---
title: Datové typy – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
uid: core/modeling/relational/data-types
ms.openlocfilehash: d667cbcb821e321faed36d097b531c7c55b81248
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149163"
---
# <a name="data-types"></a>Datové typy

> [!NOTE]  
> Konfigurace v této části platí pro relační databáze obecně. Metody rozšíření zobrazené tady budou k dispozici při instalaci zprostředkovatele relační databáze (z důvodu sdíleného balíčku *Microsoft. EntityFrameworkCore. relační* ).

Datový typ odkazuje na konkrétní typ sloupce, na který je vlastnost mapována.

## <a name="conventions"></a>Konvence

Podle konvence poskytovatel databáze vybere datový typ založený na typu rozhraní .NET vlastnosti. Také vezme v úvahu jiná metadata, jako je například nakonfigurovaná [Maximální délka](../max-length.md), zda je vlastnost částí primárního klíče atd.

`datetime2(7)` Například SQL Server používá pro `DateTime` vlastnosti a `nvarchar(max)` pro `string` vlastnosti (nebo `nvarchar(450)` pro `string` vlastnosti, které se používají jako klíč).

## <a name="data-annotations"></a>Datové poznámky

K určení přesného datového typu pro sloupec můžete použít datové poznámky.

Například následující kód se nakonfiguruje `Url` jako řetězec jiný než Unicode s maximální `200` délkou a `Rating` `2`jako desítkový s přesností `5` a měřítkem.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít také k určení stejných datových typů pro sloupce.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
