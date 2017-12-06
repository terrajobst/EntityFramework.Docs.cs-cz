---
title: "Migrace s několika poskytovateli - EF jádra"
author: bricelam
ms.author: bricelam
ms.date: 11/8/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 6b278a5ae270b6a84269dffd72eeca609b168cdd
ms.sourcegitcommit: 3b6159db8a6c0653f13c7b528367b4e69ac3d51e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/28/2017
---
<a name="migrations-with-multiple-providers"></a>Migrace s několika poskytovateli
==================================
[EF základní nástroje] [ 1] pouze vygenerovat migrace pro aktivní zprostředkovatele. V některých případech ale můžete použít více než jednoho poskytovatele (například Microsoft SQL Server a SQLite) s vaší DbContext. Existují dva způsoby, jak to postarají s migrací. Je možné uchovávat dvě sady migrací – jednu pro každý poskytovatel--nebo sloučení do jedné sady, která může pracovat na obojí.

<a name="two-migration-sets"></a>Dvě sady migrace
------------------
V prvním přístupem je generovat dvě migrace u každé změny modelu.

Jeden ze způsobů, jak provést toto je umístění každé sadě migrace [v samostatném sestavení] [ 2] a ručně přepnout active zprostředkovatele (a migrace sestavení) mezi přidáním dvě migrace.

Jiná možnost, že je díky práci s nástroji pro vytvoření nového typu odvozená od vašeho DbContext a přepíše active zprostředkovatele. Tento typ se používá v návrhu čas při přidávání nebo použití migrace.

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
