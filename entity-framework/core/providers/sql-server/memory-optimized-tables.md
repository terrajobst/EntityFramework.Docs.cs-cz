---
title: Microsoft SQL Server poskytovatel databáze – paměťově optimalizované tabulky – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: 7383e74b3f83172f9b8e0eaf9bd09d4e187e87f8
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813486"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a><span data-ttu-id="cb439-102">Podpora paměťově optimalizovaných tabulek v SQL Server EF Core poskytovatel databáze</span><span class="sxs-lookup"><span data-stu-id="cb439-102">Memory-Optimized Tables support in SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="cb439-103">[Paměťově optimalizované tabulky](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) jsou funkce SQL Server, kde se nachází celá tabulka v paměti.</span><span class="sxs-lookup"><span data-stu-id="cb439-103">[Memory-Optimized Tables](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) are a feature of SQL Server where the entire table resides in memory.</span></span> <span data-ttu-id="cb439-104">Druhá kopie dat tabulky je udržována na disku, ale pouze pro účely trvanlivosti.</span><span class="sxs-lookup"><span data-stu-id="cb439-104">A second copy of the table data is maintained on disk, but only for durability purposes.</span></span> <span data-ttu-id="cb439-105">Data v paměťově optimalizovaných tabulkách se při obnovení databáze čtou jenom z disku.</span><span class="sxs-lookup"><span data-stu-id="cb439-105">Data in memory-optimized tables is only read from disk during database recovery.</span></span> <span data-ttu-id="cb439-106">Například po restartování serveru.</span><span class="sxs-lookup"><span data-stu-id="cb439-106">For example, after a server restart.</span></span>

## <a name="configuring-a-memory-optimized-table"></a><span data-ttu-id="cb439-107">Konfigurace paměťově optimalizované tabulky</span><span class="sxs-lookup"><span data-stu-id="cb439-107">Configuring a memory-optimized table</span></span>

<span data-ttu-id="cb439-108">Můžete určit, že tabulka, na kterou je entita mapována, je optimalizovaná pro paměť.</span><span class="sxs-lookup"><span data-stu-id="cb439-108">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="cb439-109">Při použití EF Core k vytvoření a údržbě databáze založené na modelu (s migracemi nebo `Database.EnsureCreated()`) bude pro tyto entity vytvořená paměťově optimalizovaná tabulka.</span><span class="sxs-lookup"><span data-stu-id="cb439-109">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
