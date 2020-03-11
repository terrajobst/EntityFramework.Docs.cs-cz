---
title: Microsoft SQL Server poskytovatel databáze – paměťově optimalizované tabulky – EF Core
description: Jak používat paměťově optimalizované tabulky s poskytovatelem databáze SQL Server Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: a504fb3487aea6dd36abf204a7427095e3d29118
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417789"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a>Podpora paměťově optimalizovaných tabulek v SQL Server EF Core poskytovatel databáze

[Paměťově optimalizované tabulky](/sql/relational-databases/in-memory-oltp/memory-optimized-tables) jsou funkce SQL Server, kde se nachází celá tabulka v paměti. Druhá kopie dat tabulky je udržována na disku, ale pouze pro účely trvanlivosti. Data v paměťově optimalizovaných tabulkách se při obnovení databáze čtou jenom z disku. Například po restartování serveru.

## <a name="configuring-a-memory-optimized-table"></a>Konfigurace paměťově optimalizované tabulky

Můžete určit, že tabulka, na kterou je entita mapována, je optimalizovaná pro paměť. Při použití EF Core k vytvoření a údržbě databáze založené na modelu (buď s [migrací](xref:core/managing-schemas/migrations/index) nebo [EnsureCreated](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreated)) se pro tyto entity vytvoří paměťově optimalizovaná tabulka.

[!code-csharp[IsMemoryOptimized](../../../../samples/core/SqlServer/InMemory/InMemoryContext.cs?name=IsMemoryOptimized)]
