---
title: Tabulky historie migrací vlastní - EF základní
author: bricelam
ms.author: bricelam
ms.date: 11/7/2017
ms.technology: entity-framework-core
ms.openlocfilehash: cb9892241f3d7f1fae6293bd60a8a5c3e7120969
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
ms.locfileid: "26054727"
---
<a name="custom-migrations-history-table"></a>Tabulky historie migrací vlastní
===============================
Ve výchozím nastavení, uchovává informace o základní EF, které migrace se aplikovaly k databázi je zaznamenáním v tabulce s názvem `__EFMigrationsHistory`. Z různých důvodů můžete přizpůsobit tak, aby lépe vyhovoval vašim potřebám.

> [!IMPORTANT]
> Pokud upravíte tabulky historie migrací *po* použití migrace, je zodpovědná za aktualizaci existující tabulky v databázi.

<a name="schema-and-table-name"></a>Schéma a tabulku název
----------------------
Můžete změnit pomocí názvu tabulky a schématu `MigrationsHistoryTable()` metoda v `OnConfiguring()` (nebo `ConfigureServices()` na ASP.NET Core). Tady je příklad pomocí zprostředkovatele jádra EF SQL serveru.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

<a name="other-changes"></a>Další změny
-------------
Pokud chcete nakonfigurovat další aspekty tabulky, přepsání a nahraďte specifický pro zprostředkovatele `IHistoryRepository` služby. Tady je příklad změny názvu sloupce MigrationId k *Id* na serveru SQL Server.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> `SqlServerHistoryRepository`je uvnitř vnitřní obor názvů a může v budoucích verzích změnit.

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
