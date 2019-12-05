---
title: Microsoft SQL Server poskytovatel databáze – paměťově optimalizované tabulky – EF Core
description: Jak používat paměťově optimalizované tabulky s poskytovatelem databáze SQL Server Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: a504fb3487aea6dd36abf204a7427095e3d29118
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824649"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a><span data-ttu-id="35538-103">Podpora paměťově optimalizovaných tabulek v SQL Server EF Core poskytovatel databáze</span><span class="sxs-lookup"><span data-stu-id="35538-103">Memory-Optimized Tables support in SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="35538-104">[Paměťově optimalizované tabulky](/sql/relational-databases/in-memory-oltp/memory-optimized-tables) jsou funkce SQL Server, kde se nachází celá tabulka v paměti.</span><span class="sxs-lookup"><span data-stu-id="35538-104">[Memory-Optimized Tables](/sql/relational-databases/in-memory-oltp/memory-optimized-tables) are a feature of SQL Server where the entire table resides in memory.</span></span> <span data-ttu-id="35538-105">Druhá kopie dat tabulky je udržována na disku, ale pouze pro účely trvanlivosti.</span><span class="sxs-lookup"><span data-stu-id="35538-105">A second copy of the table data is maintained on disk, but only for durability purposes.</span></span> <span data-ttu-id="35538-106">Data v paměťově optimalizovaných tabulkách se při obnovení databáze čtou jenom z disku.</span><span class="sxs-lookup"><span data-stu-id="35538-106">Data in memory-optimized tables is only read from disk during database recovery.</span></span> <span data-ttu-id="35538-107">Například po restartování serveru.</span><span class="sxs-lookup"><span data-stu-id="35538-107">For example, after a server restart.</span></span>

## <a name="configuring-a-memory-optimized-table"></a><span data-ttu-id="35538-108">Konfigurace paměťově optimalizované tabulky</span><span class="sxs-lookup"><span data-stu-id="35538-108">Configuring a memory-optimized table</span></span>

<span data-ttu-id="35538-109">Můžete určit, že tabulka, na kterou je entita mapována, je optimalizovaná pro paměť.</span><span class="sxs-lookup"><span data-stu-id="35538-109">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="35538-110">Při použití EF Core k vytvoření a údržbě databáze založené na modelu (buď s [migrací](xref:core/managing-schemas/migrations/index) nebo [EnsureCreated](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreated)) se pro tyto entity vytvoří paměťově optimalizovaná tabulka.</span><span class="sxs-lookup"><span data-stu-id="35538-110">When using EF Core to create and maintain a database based on your model (either with [migrations](xref:core/managing-schemas/migrations/index) or [EnsureCreated](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreated)), a memory-optimized table will be created for these entities.</span></span>

[!code-csharp[IsMemoryOptimized](../../../../samples/core/SqlServer/InMemory/InMemoryContext.cs?name=IsMemoryOptimized)]
