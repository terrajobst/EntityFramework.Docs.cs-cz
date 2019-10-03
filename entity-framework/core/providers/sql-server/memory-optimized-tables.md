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
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a>Podpora paměťově optimalizovaných tabulek v SQL Server EF Core poskytovatel databáze

[Paměťově optimalizované tabulky](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) jsou funkce SQL Server, kde se nachází celá tabulka v paměti. Druhá kopie dat tabulky je udržována na disku, ale pouze pro účely trvanlivosti. Data v paměťově optimalizovaných tabulkách se při obnovení databáze čtou jenom z disku. Například po restartování serveru.

## <a name="configuring-a-memory-optimized-table"></a>Konfigurace paměťově optimalizované tabulky

Můžete určit, že tabulka, na kterou je entita mapována, je optimalizovaná pro paměť. Při použití EF Core k vytvoření a údržbě databáze založené na modelu (s migracemi nebo `Database.EnsureCreated()`) bude pro tyto entity vytvořená paměťově optimalizovaná tabulka.

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
