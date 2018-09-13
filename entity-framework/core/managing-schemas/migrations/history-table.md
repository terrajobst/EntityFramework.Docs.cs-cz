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
<a name="custom-migrations-history-table"></a><span data-ttu-id="c9c06-102">Tabulky historie vlastní migrace</span><span class="sxs-lookup"><span data-stu-id="c9c06-102">Custom Migrations History Table</span></span>
===============================
<span data-ttu-id="c9c06-103">Ve výchozím nastavení, EF Core uchovává informace o migrace, které se použily k databázi pomocí záznamu v tabulce s názvem `__EFMigrationsHistory`.</span><span class="sxs-lookup"><span data-stu-id="c9c06-103">By default, EF Core keeps track of which migrations have been applied to the database by recording them in a table named `__EFMigrationsHistory`.</span></span> <span data-ttu-id="c9c06-104">Z různých důvodů můžete chtít přizpůsobit v této tabulce, aby lépe vyhovovala vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="c9c06-104">For various reasons, you may want to customize this table to better suit your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c9c06-105">Pokud upravíte v tabulce historie migrace *po* použití migrace, budete muset aktualizaci existující tabulky v databázi.</span><span class="sxs-lookup"><span data-stu-id="c9c06-105">If you customize the Migrations history table *after* applying migrations, you are responsible for updating the existing table in the database.</span></span>

<a name="schema-and-table-name"></a><span data-ttu-id="c9c06-106">Schéma a platný název tabulky</span><span class="sxs-lookup"><span data-stu-id="c9c06-106">Schema and table name</span></span>
----------------------
<span data-ttu-id="c9c06-107">Schéma a tabulku s využitím název můžete změnit `MigrationsHistoryTable()` metoda `OnConfiguring()` (nebo `ConfigureServices()` v ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="c9c06-107">You can change the schema and table name using the `MigrationsHistoryTable()` method in `OnConfiguring()` (or `ConfigureServices()` on ASP.NET Core).</span></span> <span data-ttu-id="c9c06-108">Tady je příklad použití zprostředkovatele SQL Server EF Core.</span><span class="sxs-lookup"><span data-stu-id="c9c06-108">Here is an example using the SQL Server EF Core provider.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

<a name="other-changes"></a><span data-ttu-id="c9c06-109">Další změny</span><span class="sxs-lookup"><span data-stu-id="c9c06-109">Other changes</span></span>
-------------
<span data-ttu-id="c9c06-110">Ke konfiguraci dalších aspektů v tabulce, přepište a nahraďte specifickým pro zprostředkovatele `IHistoryRepository` služby.</span><span class="sxs-lookup"><span data-stu-id="c9c06-110">To configure additional aspects of the table, override and replace the provider-specific `IHistoryRepository` service.</span></span> <span data-ttu-id="c9c06-111">Tady je příklad změny názvu sloupce MigrationId *Id* na SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="c9c06-111">Here is an example of changing the MigrationId column name to *Id* on SQL Server.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> <span data-ttu-id="c9c06-112">`SqlServerHistoryRepository` je uvnitř vnitřní obor názvů a mohou v budoucích verzích změnit.</span><span class="sxs-lookup"><span data-stu-id="c9c06-112">`SqlServerHistoryRepository` is inside an internal namespace and may change in future releases.</span></span>

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
