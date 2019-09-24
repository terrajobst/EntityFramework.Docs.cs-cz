---
title: Mapování sloupce – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
uid: core/modeling/relational/columns
ms.openlocfilehash: eaffc0cc1642f64edabeeef974f5f6de7a23b656
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197214"
---
# <a name="column-mapping"></a>Mapování sloupců

> [!NOTE]  
> Konfigurace v této části platí pro relační databáze obecně. Metody rozšíření zobrazené tady budou k dispozici při instalaci zprostředkovatele relační databáze (z důvodu sdíleného balíčku *Microsoft. EntityFrameworkCore. relační* ).

Mapování sloupce určuje, která data sloupce by se měla dotazovat a Uložit do databáze.

## <a name="conventions"></a>Konvence

Podle konvence se každá vlastnost nastaví tak, aby se namapovala na sloupec se stejným názvem, jako má vlastnost.

## <a name="data-annotations"></a>Datové poznámky

Můžete použít datové poznámky ke konfiguraci sloupce, ke kterému je mapována vlastnost.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Relational/Column.cs?highlight=13)]

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít ke konfiguraci sloupce, ke kterému je namapovaná vlastnost.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/Column.cs?highlight=11-13)]
