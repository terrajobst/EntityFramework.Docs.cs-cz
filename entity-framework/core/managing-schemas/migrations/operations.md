---
title: Vlastní migrace Operations - EF jádra
author: bricelam
ms.author: bricelam
ms.date: 11/7/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 84d80175e719c950844b13688e1a4992614f25d8
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/14/2018
---
<a name="custom-migrations-operations"></a>Operace vlastní migrace
============================
Rozhraní API MigrationBuilder umožňuje provádět mnoho různých druhů operations během migrace, ale je daleko od vyčerpávající. Rozhraní API je však také rozšiřitelný, což umožňuje definovat vlastní operace. Existují dva způsoby, jak rozšířit rozhraní API: pomocí `Sql()` metoda, nebo tak, že definujete vlastní `MigrationOperation` objekty.

Pro ilustraci, podíváme se na implementace operace, která vytvoří uživatele databáze pomocí jednotlivých přístupů. V našem migrace chceme povolit zápis následující kód:

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

<a name="using-migrationbuildersql"></a>Pomocí MigrationBuilder.Sql()
----------------------------
Nejjednodušší způsob, jak implementovat vlastní operace je definují metody rozšíření, která volá `MigrationBuilder.Sql()`.
Tady je příklad, který generuje odpovídající Transact-SQL.

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

Pokud vaše migrace potřebujete podporu více poskytovatelů databáze, můžete použít `MigrationBuilder.ActiveProvider` vlastnost. Tady je příklad podpora Microsoft SQL Server a PostgreSQL.

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
{
    switch (migrationBuilder.ActiveProvider)
    {
        case "Npgsql.EntityFrameworkCore.PostgreSQL":
            return migrationBuilder
                .Sql($"CREATE USER {name} WITH PASSWORD '{password}';");

        case "Microsoft.EntityFrameworkCore.SqlServer":
            return migrationBuilder
                .Sql($"CREATE USER {name} WITH PASSWORD = '{password}';");
    }
}
```

Tento postup funguje jenom v případě vědět každý poskytovatel kde bude použito vlastní operaci.

<a name="using-a-migrationoperation"></a>Použití MigrationOperation
---------------------------
Oddělit vlastní operaci provést z SQL, můžete definovat vlastní `MigrationOperation` představují ho. Operaci se potom předána zprostředkovateli tak může zjistit, příslušné SQL pro generování.

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

S tímto přístupem metodě rozšíření právě musí přidat jednu z těchto operací na `MigrationBuilder.Operations`.

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
{
    migrationBuilder.Operations.Add(
        new CreateUserOperation
        {
            Name = name,
            Password = password
        });

    return migrationBuilder;
}
```

Tento přístup vyžaduje každý poskytovatel vědět, jak generovat SQL pro tuto operaci v jejich `IMigrationsSqlGenerator` služby. Tady je příklad přepsání generátor SQL Server pro operaci nového zpracování.

``` csharp
class MyMigrationsSqlGenerator : SqlServerMigrationsSqlGenerator
{
    public MyMigrationsSqlGenerator(
        MigrationsSqlGeneratorDependencies dependencies,
        IMigrationsAnnotationProvider migrationsAnnotations)
        : base(dependencies, migrationsAnnotations)
    {
    }

    protected override void Generate(
        MigrationOperation operation,
        IModel model,
        MigrationCommandListBuilder builder)
    {
        if (operation is CreateUserOperation createUserOperation)
        {
            Generate(createUserOperation, builder);
        }
        else
        {
            base.Generate(operation, model, builder);
        }
    }

    private void Generate(
        CreateUserOperation operation,
        MigrationCommandListBuilder builder)
    {
        var sqlHelper = Dependencies.SqlGenerationHelper;
        var stringMapping = Dependencies.TypeMapper.GetMapping(typeof(string));

        builder
            .Append("CREATE USER ")
            .Append(sqlHelper.DelimitIdentifier(name))
            .Append(" WITH PASSWORD = ")
            .Append(stringMapping.GenerateSqlLiteral(password))
            .AppendLine(sqlHelper.StatementTerminator)
            .EndCommand();
    }
}
```

Výchozí služba generátor sql migrace nahradíte aktualizovanou položkou.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
