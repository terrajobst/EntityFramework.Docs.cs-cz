---
title: Operace s vlastními migracemi – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/operations
ms.openlocfilehash: bd2bfdc24977a47eaf7a6756a88b758b563d818a
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416875"
---
# <a name="custom-migrations-operations"></a>Operace s vlastní migrací

Rozhraní MigrationBuilder API umožňuje provádět v průběhu migrace mnoho různých druhů operací, ale je to daleko od sebe vyčerpávající. Rozhraní API je ale také rozšiřitelné, což vám umožní definovat vaše vlastní operace. Existují dva způsoby, jak rozhraní API rozšiřuje: pomocí metody `Sql()`, nebo definováním vlastních objektů `MigrationOperation`.

Pro ilustraci se podívejme na implementaci operace, která vytvoří uživatele databáze pomocí každého přístupu. V našich migracích chceme povolit zápis následující kód:

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

## <a name="using-migrationbuildersql"></a>Použití MigrationBuilder. SQL ()

Nejjednodušší způsob implementace vlastní operace je definovat metodu rozšíření, která volá `MigrationBuilder.Sql()`. Tady je příklad, který generuje odpovídající jazyk Transact-SQL.

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

Pokud vaše migrace potřebují podporovat více poskytovatelů databáze, můžete použít vlastnost `MigrationBuilder.ActiveProvider`. Tady je příklad podporující Microsoft SQL Server a PostgreSQL.

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

Tento přístup funguje jenom v případě, že znáte každého poskytovatele, kde se vaše vlastní operace použije.

## <a name="using-a-migrationoperation"></a>Použití MigrationOperation

Chcete-li oddělit vlastní operaci z databáze SQL, můžete definovat vlastní `MigrationOperation` pro reprezentaci. Tato operace je pak předána poskytovateli, aby mohla určit vhodný SQL pro vygenerování.

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

Pomocí tohoto přístupu metoda rozšíření stačí k `MigrationBuilder.Operations`přidat jednu z těchto operací.

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

Tento přístup vyžaduje, aby každý zprostředkovatel znal, jak vygenerovat SQL pro tuto operaci ve své `IMigrationsSqlGenerator` službě. Tady je příklad přepsání generátoru SQL Server pro zpracování nové operace.

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

Nahraďte výchozí migraci služby generátoru SQL pomocí aktualizované verze.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
