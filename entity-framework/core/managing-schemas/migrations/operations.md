---
title: Operace s vlastní migrací – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/operations
ms.openlocfilehash: 93de6ee1b2eda1875188ace6eda299260fbcc1fe
ms.sourcegitcommit: 082946dcaa1ee5174e692dbfe53adeed40609c6a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2018
ms.locfileid: "51028080"
---
<a name="custom-migrations-operations"></a>Operace vlastní migrace
============================
Rozhraní API MigrationBuilder lze provádět mnoho různých typů operací během migrace, ale je daleko od vyčerpávající. Rozhraní API je však také rozšiřitelný, umožňuje definovat vlastní operace. Existují dva způsoby, jak rozšířit rozhraní API: použití `Sql()` metodu, nebo tak, že definujete vlastní `MigrationOperation` objekty.

Pro ilustraci, Podívejme se na implementaci operace, která vytvoří uživatele databáze s využitím každý přístup. V našem migrace chceme umožnit zápis následující kód:

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

<a name="using-migrationbuildersql"></a>Pomocí MigrationBuilder.Sql()
----------------------------
Nejjednodušší způsob, jak implementovat vlastní operace se má definovat rozšiřující metodu, která volá `MigrationBuilder.Sql()`.
Tady je příklad, který generuje příslušné příkazů jazyka Transact-SQL.

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

Pokud vaše migrace potřebovat pro podporu více poskytovatelů pro databáze, můžete použít `MigrationBuilder.ActiveProvider` vlastnost. Tady je příklad podporuje Microsoft SQL Server využívající databázi PostgreSQL.

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

    return migrationBuilder;
}
```

Tento postup funguje jenom v případě znát každý zprostředkovatele kde se budou aplikovat vaše vlastní operace.

<a name="using-a-migrationoperation"></a>Použití MigrationOperation
---------------------------
Pokud chcete oddělit vlastní operace v SQL, můžete definovat vlastní `MigrationOperation` který ho zastupuje. Operace je pak předán zprostředkovatel tak může zjistit, správný příkaz SQL pro generování.

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

S tímto přístupem jenom potřebuje rozšiřující metoda pro přidání jednoho z těchto operací `MigrationBuilder.Operations`.

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

Tento přístup vyžaduje každý poskytovatel vědět, jak ke generování SQL pro tuto operaci v jejich `IMigrationsSqlGenerator` služby. Tady je příklad přepsání generátor SQL Server pro novou operaci zpracování.

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
        var stringMapping = Dependencies.TypeMappingSource.FindMapping(typeof(string));

        builder
            .Append("CREATE USER ")
            .Append(sqlHelper.DelimitIdentifier(operation.Name))
            .Append(" WITH PASSWORD = ")
            .Append(stringMapping.GenerateSqlLiteral(operation.Password))
            .AppendLine(sqlHelper.StatementTerminator)
            .EndCommand();
    }
}
```

Aktualizované nahradíte výchozí migrace sql generátoru service.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
