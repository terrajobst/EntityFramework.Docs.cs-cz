---
title: Počítané sloupce – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: 4e92ed6d785f3b7bdf54015101bdcddb9e4e0e1c
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655917"
---
# <a name="computed-columns"></a>Vypočítané sloupce

> [!NOTE]  
> Konfigurace v této části platí pro relační databáze obecně. Metody rozšíření zobrazené tady budou k dispozici při instalaci zprostředkovatele relační databáze (z důvodu sdíleného balíčku *Microsoft. EntityFrameworkCore. relační* ).

Vypočítaný sloupec je sloupec, jehož hodnota je vypočítána v databázi. Vypočítaný sloupec může použít jiné sloupce v tabulce k výpočtu jeho hodnoty.

## <a name="conventions"></a>Konvence

Podle konvence se v modelu nevytvoří počítané sloupce.

## <a name="data-annotations"></a>Datové poznámky

U počítaných sloupců nelze konfigurovat datové poznámky.

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít k určení, že vlastnost by měla být namapována na vypočítaný sloupec.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/ComputedColumn.cs?name=ComputedColumn&highlight=9)]
