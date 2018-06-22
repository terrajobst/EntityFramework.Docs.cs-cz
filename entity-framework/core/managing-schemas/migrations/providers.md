---
title: Migrace s několika poskytovateli - EF jádra
author: bricelam
ms.author: bricelam
ms.date: 11/8/2017
ms.technology: entity-framework-core
ms.openlocfilehash: d950e74ed4cef7d4274aabcf3eda7b0b735574c6
ms.sourcegitcommit: 2ef0a4a90b01edd22b9206f8729b8de459ef8cab
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/20/2018
ms.locfileid: "30002802"
---
<a name="migrations-with-multiple-providers"></a>Migrace s několika poskytovateli
==================================
[EF základní nástroje] [ 1] pouze vygenerovat migrace pro aktivní zprostředkovatele. V některých případech ale můžete použít více než jednoho poskytovatele (například Microsoft SQL Server a SQLite) s vaší DbContext. Existují dva způsoby, jak to postarají s migrací. Je možné uchovávat dvě sady migrací – jednu pro každý poskytovatel--nebo sloučení do jedné sady, která může pracovat na obojí.

<a name="two-migration-sets"></a>Dvě sady migrace
------------------
V prvním přístupem je generovat dvě migrace u každé změny modelu.

Jeden ze způsobů, jak provést toto je umístění každé sadě migrace [v samostatném sestavení] [ 2] a ručně přepnout active zprostředkovatele (a migrace sestavení) mezi přidáním dvě migrace.

Jiná možnost, která usnadňuje práci s nástroji pro je vytvoření nového typu, která pochází z vaší DbContext a přepíše active zprostředkovatele. Tento typ se používá v návrhu čas při přidávání nebo použití migrace.

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> Protože každá sada migrace používá vlastní typy DbContext, nevyžaduje tento přístup pomocí samostatné migrace sestavení.

Při přidávání nové migrace, určete typy kontextu.

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```
``` Console
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

> [!TIP]
> Nemusíte určit výstupního adresáře pro následné migrace, protože se vytvářejí na stejné úrovni na poslední.

<a name="one-migration-set"></a>Sada jeden migrace
-----------------
Pokud chcete mít dvě sady migrace, můžete je ručně zkombinovat do jedné sady, který lze použít na obou zprostředkovatele.

Vzhledem k tomu, že zprostředkovatel ignoruje žádné poznámky, které není pochopit, může existovat současně poznámky. Sloupec primárního klíče, který funguje s Microsoft SQL Server a SQLite může například vypadat například takto.

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

Pokud operace lze použít pouze na jednoho poskytovatele (nebo jinak jsou mezi zprostředkovatelé), použijte `ActiveProvider` vlastnost říct poskytovatele, kterého je aktivní.

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```


  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
