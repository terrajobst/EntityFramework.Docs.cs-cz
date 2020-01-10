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
# <a name="table-splitting"></a><span data-ttu-id="757c9-103">Dělení tabulky</span><span class="sxs-lookup"><span data-stu-id="757c9-103">Table Splitting</span></span>

<span data-ttu-id="757c9-104">EF Core umožňuje mapovat dvě nebo více entit na jeden řádek.</span><span class="sxs-lookup"><span data-stu-id="757c9-104">EF Core allows to map two or more entities to a single row.</span></span> <span data-ttu-id="757c9-105">Tato metoda se nazývá _rozdělení tabulky_ nebo _sdílení tabulky_.</span><span class="sxs-lookup"><span data-stu-id="757c9-105">This is called _table splitting_ or _table sharing_.</span></span>

## <a name="configuration"></a><span data-ttu-id="757c9-106">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="757c9-106">Configuration</span></span>

<span data-ttu-id="757c9-107">Chcete-li použít rozdělení tabulky typů entit na stejnou tabulku, je třeba namapovat primární klíče na stejné sloupce a alespoň jednu relaci nakonfigurovanou mezi primárním klíčem jednoho typu entity a druhým ve stejné tabulce.</span><span class="sxs-lookup"><span data-stu-id="757c9-107">To use table splitting the entity types need to be mapped to the same table, have the primary keys mapped to the same columns and at least one relationship configured between the primary key of one entity type and another in the same table.</span></span>

<span data-ttu-id="757c9-108">Běžný scénář rozdělování tabulky používá pouze podmnožinu sloupců v tabulce pro zvýšení výkonu nebo zapouzdření.</span><span class="sxs-lookup"><span data-stu-id="757c9-108">A common scenario for table splitting is using only a subset of the columns in the table for greater performance or encapsulation.</span></span>

<span data-ttu-id="757c9-109">V tomto příkladu `Order` představuje podmnožinu `DetailedOrder`.</span><span class="sxs-lookup"><span data-stu-id="757c9-109">In this example `Order` represents a subset of `DetailedOrder`.</span></span>

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

<span data-ttu-id="757c9-110">Kromě požadované konfigurace voláme `Property(o => o.Status).HasColumnName("Status")` k mapování `DetailedOrder.Status` na stejný sloupec jako `Order.Status`.</span><span class="sxs-lookup"><span data-stu-id="757c9-110">In addition to the required configuration we call `Property(o => o.Status).HasColumnName("Status")` to map `DetailedOrder.Status` to the same column as `Order.Status`.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting)]

> [!TIP]
> <span data-ttu-id="757c9-111">Podívejte se na [úplný ukázkový projekt](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) pro další kontext.</span><span class="sxs-lookup"><span data-stu-id="757c9-111">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) for more context.</span></span>

## <a name="usage"></a><span data-ttu-id="757c9-112">Použití</span><span class="sxs-lookup"><span data-stu-id="757c9-112">Usage</span></span>

<span data-ttu-id="757c9-113">Ukládání a dotazování entit pomocí rozdělení tabulky je prováděno stejným způsobem jako jiné entity:</span><span class="sxs-lookup"><span data-stu-id="757c9-113">Saving and querying entities using table splitting is done in the same way as other entities:</span></span>

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="optional-dependent-entity"></a><span data-ttu-id="757c9-114">Volitelná závislá entita</span><span class="sxs-lookup"><span data-stu-id="757c9-114">Optional dependent entity</span></span>

> [!NOTE]
> <span data-ttu-id="757c9-115">Tato funkce byla představena v EF Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="757c9-115">This feature was introduced in EF Core 3.0.</span></span>

<span data-ttu-id="757c9-116">Pokud jsou všechny sloupce používané závislou entitou v databázi `NULL`, pak při dotazování nebude vytvořena žádná instance.</span><span class="sxs-lookup"><span data-stu-id="757c9-116">If all of the columns used by a dependent entity are `NULL` in the database, then no instance for it will be created when queried.</span></span> <span data-ttu-id="757c9-117">To umožňuje modelování volitelné závislé entity, kde vlastnost Relationship objektu zabezpečení má hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="757c9-117">This allows modeling an optional dependent entity, where the relationship property on the principal would be null.</span></span> <span data-ttu-id="757c9-118">Všimněte si, že by se taky měly všechny vlastnosti závislé na tom, že jsou volitelné a nastavené na `null`, což se nemusí očekávat.</span><span class="sxs-lookup"><span data-stu-id="757c9-118">Note that This would also happen all of the dependent's properties are optional and set to `null`, which might not be expected.</span></span>

## <a name="concurrency-tokens"></a><span data-ttu-id="757c9-119">Tokeny souběžnosti</span><span class="sxs-lookup"><span data-stu-id="757c9-119">Concurrency tokens</span></span>

<span data-ttu-id="757c9-120">Pokud některý z typů entit sdílících tabulku má Token souběžnosti, musí být zahrnut ve všech ostatních typech entit současně.</span><span class="sxs-lookup"><span data-stu-id="757c9-120">If any of the entity types sharing a table has a concurrency token then it must be included in all other entity types as well.</span></span> <span data-ttu-id="757c9-121">To je nezbytné, aby se předešlo zastaralosti tokenu souběžnosti, pokud je aktualizována pouze jedna z entit namapovaných na stejnou tabulku.</span><span class="sxs-lookup"><span data-stu-id="757c9-121">This is necessary in order to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="757c9-122">Aby nedocházelo k vystavení tokenu souběžnosti k náročnému kódu, je možné vytvořit jednu jako [vlastnost Shadow](xref:core/modeling/shadow-properties):</span><span class="sxs-lookup"><span data-stu-id="757c9-122">To avoid exposing the concurrency token to the consuming code, it's possible the create one as a [shadow property](xref:core/modeling/shadow-properties):</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]
