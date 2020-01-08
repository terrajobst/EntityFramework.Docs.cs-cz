---
title: Migrace s více zprostředkovateli – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/08/2017
uid: core/managing-schemas/migrations/providers
ms.openlocfilehash: efe95893f7dbfc8e5c4775e86d58abb32eee3c83
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502107"
---
# <a name="migrations-with-multiple-providers"></a>Migrace s více zprostředkovateli

[Nástroje pro EF Core][1] jsou jenom migrace uživatelského rozhraní pro aktivního poskytovatele. V některých případech však můžete chtít použít více než jednoho poskytovatele (například Microsoft SQL Server a SQLite) s vaším DbContext. Existují dva způsoby, jak to zvládnout s migracemi. Můžete udržovat dvě sady migrace – jeden pro každého poskytovatele – nebo je sloučit do jedné sady, která může fungovat na obou.

## <a name="two-migration-sets"></a>Dvě sady migrace

V prvním přístupu vygenerujete pro každou změnu modelu dvě migrace.

Jedním ze způsobů, jak to provést, je umístit každou sadu migrace [do samostatného sestavení][2] a ručně přepínat aktivní zprostředkovatele (a sestavení migrace) mezi přidáním těchto dvou migrací.

Dalším způsobem, který usnadňuje práci s nástroji, je vytvořit nový typ, který je odvozený od vašeho DbContext a přepíše aktivního poskytovatele. Tento typ se používá v době návrhu při přidávání nebo aplikování migrace.

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> Vzhledem k tomu, že každá sada migrace používá vlastní typy DbContext, tento přístup nevyžaduje použití samostatného sestavení migrace.

Při přidávání nové migrace zadejte typy kontextu.

### <a name="net-core-clitabdotnet-core-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

### <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```

***

> [!TIP]
> Nemusíte určovat výstupní adresář pro následné migrace, protože jsou vytvořené na stejné úrovni jako poslední.

## <a name="one-migration-set"></a>Jedna sada migrace

Pokud nechcete, aby se dvě sady migrace používaly, můžete je ručně zkombinovat do jedné sady, kterou je možné použít u obou zprostředkovatelů.

Poznámky můžou existovat společně, protože poskytovatel ignoruje všechny anotace, které nerozumí. Například sloupec primárního klíče, který pracuje s Microsoft SQL Server i SQLite, může vypadat takto.

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

Pokud se operace dají použít jenom u jednoho poskytovatele (nebo se mezi poskytovateli liší), pomocí vlastnosti `ActiveProvider` sdělte, který poskytovatel je aktivní.

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```

  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
