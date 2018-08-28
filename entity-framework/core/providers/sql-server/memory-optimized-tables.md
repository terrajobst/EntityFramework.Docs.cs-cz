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
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a>Paměťově optimalizované tabulky se podporují v poskytovatele databáze SQL serveru EF Core

> [!NOTE]  
>
> Tato funkce byla zavedená v EF Core 1.1.

[K paměťově optimalizovaným tabulkám](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) jsou funkce systému SQL Server, kde je umístěn celé tabulky v paměti. Druhé kopie tabulkových dat se udržuje na disku, ale pouze pro účely odolnosti. Data v paměťově optimalizovaných tabulek je číst pouze z disku během obnovení databáze. Například po server restartovat.

## <a name="configuring-a-memory-optimized-table"></a>Konfigurace paměťově optimalizované tabulky

Můžete určit, že je tabulka entita je namapována na paměťově optimalizovaná. Když pomocí EF Core pro vytváření a údržbu databáze na základě modelu (pomocí migrace nebo `Database.EnsureCreated()`), paměťově optimalizované tabulky se vytvoří pro tyto entity.

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
