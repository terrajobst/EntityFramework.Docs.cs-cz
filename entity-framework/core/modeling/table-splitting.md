---
title: Rozdělení tabulky – EF Core
description: Postup konfigurace rozdělení tabulky pomocí Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 04/10/2019
uid: core/modeling/table-splitting
ms.openlocfilehash: 0e48c516de43cdc2b54c56f1a96f5e01f9fbbbc4
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824566"
---
# <a name="table-splitting"></a><span data-ttu-id="b3df6-103">Dělení tabulky</span><span class="sxs-lookup"><span data-stu-id="b3df6-103">Table Splitting</span></span>

>[!NOTE]
> <span data-ttu-id="b3df6-104">Tato funkce je v EF Core 2,0 novinkou.</span><span class="sxs-lookup"><span data-stu-id="b3df6-104">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="b3df6-105">EF Core umožňuje mapovat dvě nebo více entit na jeden řádek.</span><span class="sxs-lookup"><span data-stu-id="b3df6-105">EF Core allows to map two or more entities to a single row.</span></span> <span data-ttu-id="b3df6-106">Tato metoda se nazývá _rozdělení tabulky_ nebo _sdílení tabulky_.</span><span class="sxs-lookup"><span data-stu-id="b3df6-106">This is called _table splitting_ or _table sharing_.</span></span>

## <a name="configuration"></a><span data-ttu-id="b3df6-107">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="b3df6-107">Configuration</span></span>

<span data-ttu-id="b3df6-108">Chcete-li použít rozdělení tabulky typů entit na stejnou tabulku, je třeba namapovat primární klíče na stejné sloupce a alespoň jednu relaci nakonfigurovanou mezi primárním klíčem jednoho typu entity a druhým ve stejné tabulce.</span><span class="sxs-lookup"><span data-stu-id="b3df6-108">To use table splitting the entity types need to be mapped to the same table, have the primary keys mapped to the same columns and at least one relationship configured between the primary key of one entity type and another in the same table.</span></span>

<span data-ttu-id="b3df6-109">Běžný scénář rozdělování tabulky používá pouze podmnožinu sloupců v tabulce pro zvýšení výkonu nebo zapouzdření.</span><span class="sxs-lookup"><span data-stu-id="b3df6-109">A common scenario for table splitting is using only a subset of the columns in the table for greater performance or encapsulation.</span></span>

<span data-ttu-id="b3df6-110">V tomto příkladu `Order` představuje podmnožinu `DetailedOrder`.</span><span class="sxs-lookup"><span data-stu-id="b3df6-110">In this example `Order` represents a subset of `DetailedOrder`.</span></span>

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

<span data-ttu-id="b3df6-111">Kromě požadované konfigurace voláme `Property(o => o.Status).HasColumnName("Status")` k mapování `DetailedOrder.Status` na stejný sloupec jako `Order.Status`.</span><span class="sxs-lookup"><span data-stu-id="b3df6-111">In addition to the required configuration we call `Property(o => o.Status).HasColumnName("Status")` to map `DetailedOrder.Status` to the same column as `Order.Status`.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

> [!TIP]
> <span data-ttu-id="b3df6-112">Podívejte se na [úplný ukázkový projekt](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) pro další kontext.</span><span class="sxs-lookup"><span data-stu-id="b3df6-112">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) for more context.</span></span>

## <a name="usage"></a><span data-ttu-id="b3df6-113">Použití</span><span class="sxs-lookup"><span data-stu-id="b3df6-113">Usage</span></span>

<span data-ttu-id="b3df6-114">Ukládání a dotazování entit pomocí rozdělení tabulky je prováděno stejným způsobem jako jiné entity.</span><span class="sxs-lookup"><span data-stu-id="b3df6-114">Saving and querying entities using table splitting is done in the same way as other entities.</span></span> <span data-ttu-id="b3df6-115">A počínaje EF Core 3,0 odkaz na závislou entitu lze `null`.</span><span class="sxs-lookup"><span data-stu-id="b3df6-115">And starting with EF Core 3.0 the dependent entity reference can be `null`.</span></span> <span data-ttu-id="b3df6-116">Pokud jsou všechny sloupce používané závislou entitou `NULL` databáze, nebude při dotazování vytvořena její instance.</span><span class="sxs-lookup"><span data-stu-id="b3df6-116">If all of the columns used by the dependent entity are `NULL` is the database then no instance for it will be created when queried.</span></span> <span data-ttu-id="b3df6-117">To by také mohlo nastat u všech vlastností, které jsou volitelné a nastavené na `null`, což se nemusí očekávat.</span><span class="sxs-lookup"><span data-stu-id="b3df6-117">This would also happen all of the properties are optional and set to `null`, which might not be expected.</span></span>

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a><span data-ttu-id="b3df6-118">Tokeny souběžnosti</span><span class="sxs-lookup"><span data-stu-id="b3df6-118">Concurrency tokens</span></span>

<span data-ttu-id="b3df6-119">Pokud některý z typů entit, které sdílí tabulku, má Token souběžnosti, musí být součástí všech ostatních typů entit, aby se předešlo zastaralosti tokenu souběžnosti, pokud je aktualizována pouze jedna z entit namapovaných na stejnou tabulku.</span><span class="sxs-lookup"><span data-stu-id="b3df6-119">If any of the entity types sharing a table has a concurrency token then it must be included in all other entity types to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="b3df6-120">Aby nedocházelo k vystavení kódu, je možné ho vytvořit ve stínovém stavu.</span><span class="sxs-lookup"><span data-stu-id="b3df6-120">To avoid exposing it to the consuming code it's possible the create one in shadow-state.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]
