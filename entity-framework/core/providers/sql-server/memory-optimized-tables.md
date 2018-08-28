---
title: Microsoft SQL Server Database poskytovatele - paměťově optimalizované tabulky – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: 63d2cbf8b69e4f1945ad60914e284fb42c48e8db
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995799"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a><span data-ttu-id="2f2a2-102">Paměťově optimalizované tabulky se podporují v poskytovatele databáze SQL serveru EF Core</span><span class="sxs-lookup"><span data-stu-id="2f2a2-102">Memory-Optimized Tables support in SQL Server EF Core Database Provider</span></span>

> [!NOTE]  
>
> <span data-ttu-id="2f2a2-103">Tato funkce byla zavedená v EF Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="2f2a2-103">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="2f2a2-104">[K paměťově optimalizovaným tabulkám](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) jsou funkce systému SQL Server, kde je umístěn celé tabulky v paměti.</span><span class="sxs-lookup"><span data-stu-id="2f2a2-104">[Memory-Optimized Tables](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) are a feature of SQL Server where the entire table resides in memory.</span></span> <span data-ttu-id="2f2a2-105">Druhé kopie tabulkových dat se udržuje na disku, ale pouze pro účely odolnosti.</span><span class="sxs-lookup"><span data-stu-id="2f2a2-105">A second copy of the table data is maintained on disk, but only for durability purposes.</span></span> <span data-ttu-id="2f2a2-106">Data v paměťově optimalizovaných tabulek je číst pouze z disku během obnovení databáze.</span><span class="sxs-lookup"><span data-stu-id="2f2a2-106">Data in memory-optimized tables is only read from disk during database recovery.</span></span> <span data-ttu-id="2f2a2-107">Například po server restartovat.</span><span class="sxs-lookup"><span data-stu-id="2f2a2-107">For example, after a server restart.</span></span>

## <a name="configuring-a-memory-optimized-table"></a><span data-ttu-id="2f2a2-108">Konfigurace paměťově optimalizované tabulky</span><span class="sxs-lookup"><span data-stu-id="2f2a2-108">Configuring a memory-optimized table</span></span>

<span data-ttu-id="2f2a2-109">Můžete určit, že je tabulka entita je namapována na paměťově optimalizovaná.</span><span class="sxs-lookup"><span data-stu-id="2f2a2-109">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="2f2a2-110">Když pomocí EF Core pro vytváření a údržbu databáze na základě modelu (pomocí migrace nebo `Database.EnsureCreated()`), paměťově optimalizované tabulky se vytvoří pro tyto entity.</span><span class="sxs-lookup"><span data-stu-id="2f2a2-110">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
