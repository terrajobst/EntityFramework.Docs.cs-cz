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
<a name="custom-migrations-history-table"></a><span data-ttu-id="d7153-102">Tabulky historie migrací vlastní</span><span class="sxs-lookup"><span data-stu-id="d7153-102">Custom Migrations History Table</span></span>
===============================
<span data-ttu-id="d7153-103">Ve výchozím nastavení, uchovává informace o základní EF, které migrace se aplikovaly k databázi je zaznamenáním v tabulce s názvem `__EFMigrationsHistory`.</span><span class="sxs-lookup"><span data-stu-id="d7153-103">By default, EF Core keeps track of which migrations have been applied to the database by recording them in a table named `__EFMigrationsHistory`.</span></span> <span data-ttu-id="d7153-104">Z různých důvodů můžete přizpůsobit tak, aby lépe vyhovoval vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="d7153-104">For various reasons, you may want to customize this table to better suit your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d7153-105">Pokud upravíte tabulky historie migrací *po* použití migrace, je zodpovědná za aktualizaci existující tabulky v databázi.</span><span class="sxs-lookup"><span data-stu-id="d7153-105">If you customize the Migrations history table *after* applying migrations, you are responsible for updating the existing table in the database.</span></span>

<a name="schema-and-table-name"></a><span data-ttu-id="d7153-106">Schéma a tabulku název</span><span class="sxs-lookup"><span data-stu-id="d7153-106">Schema and table name</span></span>
----------------------
<span data-ttu-id="d7153-107">Můžete změnit pomocí názvu tabulky a schématu `MigrationsHistoryTable()` metoda v `OnConfiguring()` (nebo `ConfigureServices()` na ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="d7153-107">You can change the schema and table name using the `MigrationsHistoryTable()` method in `OnConfiguring()` (or `ConfigureServices()` on ASP.NET Core).</span></span> <span data-ttu-id="d7153-108">Tady je příklad pomocí zprostředkovatele jádra EF SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="d7153-108">Here is an example using the SQL Server EF Core provider.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

<a name="other-changes"></a><span data-ttu-id="d7153-109">Další změny</span><span class="sxs-lookup"><span data-stu-id="d7153-109">Other changes</span></span>
-------------
<span data-ttu-id="d7153-110">Pokud chcete nakonfigurovat další aspekty tabulky, přepsání a nahraďte specifický pro zprostředkovatele `IHistoryRepository` služby.</span><span class="sxs-lookup"><span data-stu-id="d7153-110">To configure additional aspects of the table, override and replace the provider-specific `IHistoryRepository` service.</span></span> <span data-ttu-id="d7153-111">Tady je příklad změny názvu sloupce MigrationId k *Id* na serveru SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d7153-111">Here is an example of changing the MigrationId column name to *Id* on SQL Server.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> <span data-ttu-id="d7153-112">`SqlServerHistoryRepository`je uvnitř vnitřní obor názvů a může v budoucích verzích změnit.</span><span class="sxs-lookup"><span data-stu-id="d7153-112">`SqlServerHistoryRepository` is inside an internal namespace and may change in future releases.</span></span>

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
