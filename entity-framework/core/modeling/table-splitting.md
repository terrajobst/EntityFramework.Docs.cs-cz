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
# <a name="table-splitting"></a><span data-ttu-id="4de15-102">Rozdělení tabulky</span><span class="sxs-lookup"><span data-stu-id="4de15-102">Table Splitting</span></span>

>[!NOTE]
> <span data-ttu-id="4de15-103">Tato funkce je v EF Core 2.0 nová.</span><span class="sxs-lookup"><span data-stu-id="4de15-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="4de15-104">EF Core umožňuje mapovat dvěma nebo více entit na jeden řádek.</span><span class="sxs-lookup"><span data-stu-id="4de15-104">EF Core allows to map two or more entities to a single row.</span></span> <span data-ttu-id="4de15-105">Tento postup se nazývá _tabulky rozdělení_ nebo _tabulky sdílení_.</span><span class="sxs-lookup"><span data-stu-id="4de15-105">This is called _table splitting_ or _table sharing_.</span></span>

## <a name="configuration"></a><span data-ttu-id="4de15-106">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="4de15-106">Configuration</span></span>

<span data-ttu-id="4de15-107">Použití rozdělení tabulky, které je potřeba namapovat na stejnou tabulku typy entit, mají primární klíče, které jsou mapovány na stejné sloupce a alespoň jeden vztah mezi primární klíč typu jednu entitu a druhý ve stejné tabulce.</span><span class="sxs-lookup"><span data-stu-id="4de15-107">To use table splitting the entity types need to be mapped to the same table, have the primary keys mapped to the same columns and at least one relationship configured between the primary key of one entity type and another in the same table.</span></span>

<span data-ttu-id="4de15-108">Běžný scénář pro rozdělení tabulky používá pouze podmnožina sloupců v tabulce pro vyšší výkon nebo zapouzdření.</span><span class="sxs-lookup"><span data-stu-id="4de15-108">A common scenario for table splitting is using only a subset of the columns in the table for greater performance or encapsulation.</span></span>

<span data-ttu-id="4de15-109">V tomto příkladu `Order` představuje podmnožinu `DetailedOrder`.</span><span class="sxs-lookup"><span data-stu-id="4de15-109">In this example `Order` represents a subset of `DetailedOrder`.</span></span>

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

<span data-ttu-id="4de15-110">Kromě požadovanou konfiguraci říkáme `HasBaseType((string)null)` , aby mapování `DetailedOrder` ve stejné hierarchii jako `Order`.</span><span class="sxs-lookup"><span data-stu-id="4de15-110">In addition to the required configuration we call `HasBaseType((string)null)` to avoid mapping `DetailedOrder` in the same hierarchy as `Order`.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

<span data-ttu-id="4de15-111">Zobrazit [úplný ukázkový projekt](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) pro širší kontext.</span><span class="sxs-lookup"><span data-stu-id="4de15-111">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) for more context.</span></span>

## <a name="usage"></a><span data-ttu-id="4de15-112">Použití</span><span class="sxs-lookup"><span data-stu-id="4de15-112">Usage</span></span>

<span data-ttu-id="4de15-113">Ukládání a dotazování entit s využitím rozdělení tabulky se provádí stejným způsobem jako ostatní entity, jediným rozdílem je, že všechny entity sdílení řádek musí být sledována pro vložení.</span><span class="sxs-lookup"><span data-stu-id="4de15-113">Saving and querying entities using table splitting is done in the same way as other entities, the only difference is that all entities sharing a row must be tracked for the insert.</span></span>

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a><span data-ttu-id="4de15-114">Tokeny souběžnosti</span><span class="sxs-lookup"><span data-stu-id="4de15-114">Concurrency tokens</span></span>

<span data-ttu-id="4de15-115">Pokud má některý z typů entit sdílení tabulku tokenem souběžnosti pak ho musí obsahovat všechny ostatní typy entit se vyhnete hodnota tokenu zastaralé souběžnosti, když se aktualizuje jenom jeden z entity, které jsou namapované na stejnou tabulku.</span><span class="sxs-lookup"><span data-stu-id="4de15-115">If any of the entity types sharing a table has a concurrency token then it must be included in all other entity types to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="4de15-116">Aby se zabránilo bude vystavená časově náročný kód je možné vytvořit, jeden v stínové stavu.</span><span class="sxs-lookup"><span data-stu-id="4de15-116">To avoid exposing it to the consuming code it's possible the create one in shadow-state.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]