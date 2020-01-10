---
title: Rozdělení tabulky – EF Core
description: Postup konfigurace rozdělení tabulky pomocí Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 01/03/2020
uid: core/modeling/table-splitting
ms.openlocfilehash: c38d3ee0efa82f84a1051017ae40c9f3fdd57f1f
ms.sourcegitcommit: 4e86f01740e407ff25e704a11b1f7d7e66bfb2a6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/09/2020
ms.locfileid: "75781167"
---
# <a name="table-splitting"></a>Dělení tabulky

EF Core umožňuje mapovat dvě nebo více entit na jeden řádek. Tato metoda se nazývá _rozdělení tabulky_ nebo _sdílení tabulky_.

## <a name="configuration"></a>Konfigurace

Chcete-li použít rozdělení tabulky typů entit na stejnou tabulku, je třeba namapovat primární klíče na stejné sloupce a alespoň jednu relaci nakonfigurovanou mezi primárním klíčem jednoho typu entity a druhým ve stejné tabulce.

Běžný scénář rozdělování tabulky používá pouze podmnožinu sloupců v tabulce pro zvýšení výkonu nebo zapouzdření.

V tomto příkladu `Order` představuje podmnožinu `DetailedOrder`.

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

Kromě požadované konfigurace voláme `Property(o => o.Status).HasColumnName("Status")` k mapování `DetailedOrder.Status` na stejný sloupec jako `Order.Status`.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting)]

> [!TIP]
> Podívejte se na [úplný ukázkový projekt](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) pro další kontext.

## <a name="usage"></a>Použití

Ukládání a dotazování entit pomocí rozdělení tabulky je prováděno stejným způsobem jako jiné entity:

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="optional-dependent-entity"></a>Volitelná závislá entita

> [!NOTE]
> Tato funkce byla představena v EF Core 3,0.

Pokud jsou všechny sloupce používané závislou entitou v databázi `NULL`, pak při dotazování nebude vytvořena žádná instance. To umožňuje modelování volitelné závislé entity, kde vlastnost Relationship objektu zabezpečení má hodnotu null. Všimněte si, že by se taky měly všechny vlastnosti závislé na tom, že jsou volitelné a nastavené na `null`, což se nemusí očekávat.

## <a name="concurrency-tokens"></a>Tokeny souběžnosti

Pokud některý z typů entit sdílících tabulku má Token souběžnosti, musí být zahrnut ve všech ostatních typech entit současně. To je nezbytné, aby se předešlo zastaralosti tokenu souběžnosti, pokud je aktualizována pouze jedna z entit namapovaných na stejnou tabulku.

Aby nedocházelo k vystavení tokenu souběžnosti k náročnému kódu, je možné vytvořit jednu jako [vlastnost Shadow](xref:core/modeling/shadow-properties):

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]
