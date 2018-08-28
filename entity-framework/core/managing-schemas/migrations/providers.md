---
title: Migrace s několika poskytovateli – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/8/2017
ms.openlocfilehash: 7ae695037992323337a780cda29d8c8ed8a13458
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997970"
---
<a name="migrations-with-multiple-providers"></a>Migrace s více poskytovatelů
==================================
[EF Core Tools] [ 1] pouze generování uživatelského rozhraní migrace pro aktivní zprostředkovatele. V některých případech však můžete chtít použít více než jednoho poskytovatele (třeba Microsoft SQL Server a SQLite) s vaší DbContext. Existují dva způsoby, jak o to postarají s migrací. Abyste mohli pro dvě sady migrace – jednu pro každého poskytovatele--nebo sloučení je jedno nastavení, která může pracovat na obojí.

<a name="two-migration-sets"></a>Dvě sady migrace
------------------
V první metodě generovat dvě migrace u každé změny modelu.

Jeden ze způsobů, jak provést to je umístění jednotlivých sad migrace [v samostatném sestavení] [ 2] a ručně přepínat aktivní zprostředkovatel (a migrace sestavení) přidání dvě migrace.

Další možností, která usnadňuje práci s nástroji je vytvoření nového typu, který je odvozen od vaší DbContext a přepíše aktivní zprostředkovatele. Tento typ se používá při návrhu při přidání nebo použití migrace.

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> Protože každá sada migrace používá vlastní typy DbContext, nevyžaduje tento přístup pomocí migrace samostatného sestavení.

Při přidávání nové migraci, určete typy kontextu.

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```
``` Console
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

> [!TIP]
> Není nutné určit výstupní adresář pro další migrace, protože byly vytvořeny jako na stejné úrovni jako poslední.

<a name="one-migration-set"></a>Migrace jedné sady
-----------------
Pokud se vám s dvě sady migrace, můžete je ručně kombinovat do jediné sady, který lze použít na obou zprostředkovatele.

Poznámky můžou existovat společně, protože zprostředkovatele ignoruje jakékoli poznámky, které ho nerozumí. Například to sloupec primárního klíče, který spolupracuje s Microsoft SQL Server a SQLite může vypadat takto.

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

Pokud operace lze použít pouze na jeden zprostředkovatel (nebo jinak jsou mezi poskytovatele), použijte `ActiveProvider` vlastnost zjistit, které poskytovatel aktivní.

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```


  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
