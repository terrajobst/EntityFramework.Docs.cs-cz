---
title: Rozdělení tabulky – EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 04/10/2019
ms.assetid: 0EC2CCE1-BD55-45D8-9EA9-20634987F094
uid: core/modeling/table-splitting
ms.openlocfilehash: a3a2e5842a6c6b4b490084d205a0d44bb46c17ee
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656030"
---
# <a name="table-splitting"></a>Dělení tabulky

>[!NOTE]
> Tato funkce je v EF Core 2,0 novinkou.

EF Core umožňuje mapovat dvě nebo více entit na jeden řádek. Tato metoda se nazývá _rozdělení tabulky_ nebo _sdílení tabulky_.

## <a name="configuration"></a>Konfigurace

Chcete-li použít rozdělení tabulky typů entit na stejnou tabulku, je třeba namapovat primární klíče na stejné sloupce a alespoň jednu relaci nakonfigurovanou mezi primárním klíčem jednoho typu entity a druhým ve stejné tabulce.

Běžný scénář rozdělování tabulky používá pouze podmnožinu sloupců v tabulce pro zvýšení výkonu nebo zapouzdření.

V tomto příkladu `Order` představuje podmnožinu `DetailedOrder`.

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

Kromě požadované konfigurace voláme `Property(o => o.Status).HasColumnName("Status")` k mapování `DetailedOrder.Status` na stejný sloupec jako `Order.Status`.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

> [!TIP]
> Podívejte se na [úplný ukázkový projekt](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) pro další kontext.

## <a name="usage"></a>Použití

Ukládání a dotazování entit pomocí rozdělení tabulky je prováděno stejným způsobem jako jiné entity. A počínaje EF Core 3,0 odkaz na závislou entitu lze `null`. Pokud jsou všechny sloupce používané závislou entitou `NULL` databáze, nebude při dotazování vytvořena její instance. To by také mohlo nastat u všech vlastností, které jsou volitelné a nastavené na `null`, což se nemusí očekávat.

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a>Tokeny souběžnosti

Pokud některý z typů entit, které sdílí tabulku, má Token souběžnosti, musí být součástí všech ostatních typů entit, aby se předešlo zastaralosti tokenu souběžnosti, pokud je aktualizována pouze jedna z entit namapovaných na stejnou tabulku.

Aby nedocházelo k vystavení kódu, je možné ho vytvořit ve stínovém stavu.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]
