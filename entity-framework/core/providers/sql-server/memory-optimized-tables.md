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
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a>Paměťově optimalizované tabulky podporují v zprostředkovatel databáze SQL Server EF jádra

> [!NOTE]  
>
> Tato funkce byla zavedená v EF základní 1.1.

[K paměťově optimalizovaným tabulkám](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) jsou funkce systému SQL Server, kde je umístěn celou tabulku v paměti. Druhé kopie dat v tabulce se udržuje na disku, ale jenom pro účely odolnost. Data v paměťově optimalizovaných tabulek je pouze pro čtení z disku během obnovení databáze. Například po server restartujte.

## <a name="configuring-a-memory-optimized-table"></a>Konfigurace paměťově optimalizované tabulky

Můžete určit, zda je v tabulce, entita je namapovaná na paměťově optimalizované. Při použití EF jádra pro vytváření a údržbu databáze založené na modelu (buď pomocí migrace nebo `Database.EnsureCreated()`), vytvoří se paměťově optimalizované tabulky pro tyto entity.

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
