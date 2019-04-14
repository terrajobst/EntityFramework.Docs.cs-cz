---
title: Tabulka rozdělení – EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 04/10/2019
ms.assetid: 0EC2CCE1-BD55-45D8-9EA9-20634987F094
uid: core/modeling/table-splitting
ms.openlocfilehash: 4a0bfaf017106a0bfdff084b1c472bdc17459a89
ms.sourcegitcommit: 8f801993c9b8cd8a8fbfa7134818a8edca79e31a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/14/2019
ms.locfileid: "59562599"
---
# <a name="table-splitting"></a>Rozdělení tabulky

>[!NOTE]
> Tato funkce je v EF Core 2.0 nová.

EF Core umožňuje mapovat dvěma nebo více entit na jeden řádek. Tento postup se nazývá _tabulky rozdělení_ nebo _tabulky sdílení_.

## <a name="configuration"></a>Konfigurace

Použití rozdělení tabulky, které je potřeba namapovat na stejnou tabulku typy entit, mají primární klíče, které jsou mapovány na stejné sloupce a alespoň jeden vztah mezi primární klíč typu jednu entitu a druhý ve stejné tabulce.

Běžný scénář pro rozdělení tabulky používá pouze podmnožina sloupců v tabulce pro vyšší výkon nebo zapouzdření.

V tomto příkladu `Order` představuje podmnožinu `DetailedOrder`.

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

Kromě požadovanou konfiguraci říkáme `HasBaseType((string)null)` , aby mapování `DetailedOrder` ve stejné hierarchii jako `Order`.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

Zobrazit [úplný ukázkový projekt](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) pro širší kontext.

## <a name="usage"></a>Použití

Ukládání a dotazování entit s využitím rozdělení tabulky se provádí stejným způsobem jako ostatní entity, jediným rozdílem je, že všechny entity sdílení řádek musí být sledována pro vložení.

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a>Tokeny souběžnosti

Pokud má některý z typů entit sdílení tabulku tokenem souběžnosti pak ho musí obsahovat všechny ostatní typy entit se vyhnete hodnota tokenu zastaralé souběžnosti, když se aktualizuje jenom jeden z entity, které jsou namapované na stejnou tabulku.

Aby se zabránilo bude vystavená časově náročný kód je možné vytvořit, jeden v stínové stavu.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]