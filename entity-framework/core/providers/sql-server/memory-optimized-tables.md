---
title: "Microsoft SQL Server databáze zprostředkovatele - paměťově optimalizované tabulky - EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
ms.technology: entity-framework-core
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: 83a0decffc5946d036903b8b8add59f0ea31b21f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a><span data-ttu-id="31881-102">Paměťově optimalizované tabulky podporují v zprostředkovatel databáze SQL Server EF jádra</span><span class="sxs-lookup"><span data-stu-id="31881-102">Memory-Optimized Tables support in SQL Server EF Core Database Provider</span></span>

> [!NOTE]  
>
> <span data-ttu-id="31881-103">Tato funkce byla zavedená v EF základní 1.1.</span><span class="sxs-lookup"><span data-stu-id="31881-103">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="31881-104">[K paměťově optimalizovaným tabulkám](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) jsou funkce systému SQL Server, kde je umístěn celou tabulku v paměti.</span><span class="sxs-lookup"><span data-stu-id="31881-104">[Memory-Optimized Tables](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) are a feature of SQL Server where the entire table resides in memory.</span></span> <span data-ttu-id="31881-105">Druhé kopie dat v tabulce se udržuje na disku, ale jenom pro účely odolnost.</span><span class="sxs-lookup"><span data-stu-id="31881-105">A second copy of the table data is maintained on disk, but only for durability purposes.</span></span> <span data-ttu-id="31881-106">Data v paměťově optimalizovaných tabulek je pouze pro čtení z disku během obnovení databáze.</span><span class="sxs-lookup"><span data-stu-id="31881-106">Data in memory-optimized tables is only read from disk during database recovery.</span></span> <span data-ttu-id="31881-107">Například po server restartujte.</span><span class="sxs-lookup"><span data-stu-id="31881-107">For example, after a server restart.</span></span>

## <a name="configuring-a-memory-optimized-table"></a><span data-ttu-id="31881-108">Konfigurace paměťově optimalizované tabulky</span><span class="sxs-lookup"><span data-stu-id="31881-108">Configuring a memory-optimized table</span></span>

<span data-ttu-id="31881-109">Můžete určit, zda je v tabulce, entita je namapovaná na paměťově optimalizované.</span><span class="sxs-lookup"><span data-stu-id="31881-109">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="31881-110">Při použití EF jádra pro vytváření a údržbu databáze založené na modelu (buď pomocí migrace nebo `Database.EnsureCreated()`), vytvoří se paměťově optimalizované tabulky pro tyto entity.</span><span class="sxs-lookup"><span data-stu-id="31881-110">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
