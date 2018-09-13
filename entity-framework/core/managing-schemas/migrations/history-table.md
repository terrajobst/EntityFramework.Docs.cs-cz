---
title: Tabulky historie vlastní migrace – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
ms.openlocfilehash: 1a253972a8f4e410421ec8a77c079e588d368819
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45488813"
---
<a name="custom-migrations-history-table"></a>Tabulky historie vlastní migrace
===============================
Ve výchozím nastavení, EF Core uchovává informace o migrace, které se použily k databázi pomocí záznamu v tabulce s názvem `__EFMigrationsHistory`. Z různých důvodů můžete chtít přizpůsobit v této tabulce, aby lépe vyhovovala vašim potřebám.

> [!IMPORTANT]
> Pokud upravíte v tabulce historie migrace *po* použití migrace, budete muset aktualizaci existující tabulky v databázi.

<a name="schema-and-table-name"></a>Schéma a platný název tabulky
----------------------
Schéma a tabulku s využitím název můžete změnit `MigrationsHistoryTable()` metoda `OnConfiguring()` (nebo `ConfigureServices()` v ASP.NET Core). Tady je příklad použití zprostředkovatele SQL Server EF Core.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

<a name="other-changes"></a>Další změny
-------------
Ke konfiguraci dalších aspektů v tabulce, přepište a nahraďte specifickým pro zprostředkovatele `IHistoryRepository` služby. Tady je příklad změny názvu sloupce MigrationId *Id* na SQL serveru.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> `SqlServerHistoryRepository` je uvnitř vnitřní obor názvů a mohou v budoucích verzích změnit.

``` csharp
class MyHistoryRepository : SqlServerHistoryRepository
{
    public MyHistoryRepository(HistoryRepositoryDependencies dependencies)
        : base(dependencies)
    {
    }

    protected override void ConfigureTable(EntityTypeBuilder<HistoryRow> history)
    {
        base.ConfigureTable(history);

        history.Property(h => h.MigrationId).HasColumnName("Id");
    }
}
```
