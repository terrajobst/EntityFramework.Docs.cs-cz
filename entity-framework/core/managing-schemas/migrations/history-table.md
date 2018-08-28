---
title: Tabulky historie vlastní migrace – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/7/2017
ms.openlocfilehash: 7ee76cadd6fac4ec403918e88460e43067ae5815
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995693"
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
