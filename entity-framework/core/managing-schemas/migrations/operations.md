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
# <a name="custom-migrations-operations"></a><span data-ttu-id="6721e-102">Operace s vlastní migrací</span><span class="sxs-lookup"><span data-stu-id="6721e-102">Custom Migrations Operations</span></span>

<span data-ttu-id="6721e-103">Rozhraní MigrationBuilder API umožňuje provádět v průběhu migrace mnoho různých druhů operací, ale je to daleko od sebe vyčerpávající.</span><span class="sxs-lookup"><span data-stu-id="6721e-103">The MigrationBuilder API allows you to perform many different kinds of operations during a migration, but it's far from exhaustive.</span></span> <span data-ttu-id="6721e-104">Rozhraní API je ale také rozšiřitelné, což vám umožní definovat vaše vlastní operace.</span><span class="sxs-lookup"><span data-stu-id="6721e-104">However, the API is also extensible allowing you to define your own operations.</span></span> <span data-ttu-id="6721e-105">Existují dva způsoby, jak rozhraní API rozšiřuje: pomocí metody `Sql()`, nebo definováním vlastních objektů `MigrationOperation`.</span><span class="sxs-lookup"><span data-stu-id="6721e-105">There are two ways to extend the API: Using the `Sql()` method, or by defining custom `MigrationOperation` objects.</span></span>

<span data-ttu-id="6721e-106">Pro ilustraci se podívejme na implementaci operace, která vytvoří uživatele databáze pomocí každého přístupu.</span><span class="sxs-lookup"><span data-stu-id="6721e-106">To illustrate, let's look at implementing an operation that creates a database user using each approach.</span></span> <span data-ttu-id="6721e-107">V našich migracích chceme povolit zápis následující kód:</span><span class="sxs-lookup"><span data-stu-id="6721e-107">In our migrations, we want to enable writing the following code:</span></span>

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

## <a name="using-migrationbuildersql"></a><span data-ttu-id="6721e-108">Použití MigrationBuilder. SQL ()</span><span class="sxs-lookup"><span data-stu-id="6721e-108">Using MigrationBuilder.Sql()</span></span>

<span data-ttu-id="6721e-109">Nejjednodušší způsob implementace vlastní operace je definovat metodu rozšíření, která volá `MigrationBuilder.Sql()`.</span><span class="sxs-lookup"><span data-stu-id="6721e-109">The easiest way to implement a custom operation is to define an extension method that calls `MigrationBuilder.Sql()`.</span></span> <span data-ttu-id="6721e-110">Tady je příklad, který generuje odpovídající jazyk Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="6721e-110">Here is an example that generates the appropriate Transact-SQL.</span></span>

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

<span data-ttu-id="6721e-111">Pokud vaše migrace potřebují podporovat více poskytovatelů databáze, můžete použít vlastnost `MigrationBuilder.ActiveProvider`.</span><span class="sxs-lookup"><span data-stu-id="6721e-111">If your migrations need to support multiple database providers, you can use the `MigrationBuilder.ActiveProvider` property.</span></span> <span data-ttu-id="6721e-112">Tady je příklad podporující Microsoft SQL Server a PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="6721e-112">Here's an example supporting both Microsoft SQL Server and PostgreSQL.</span></span>

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

<span data-ttu-id="6721e-113">Tento přístup funguje jenom v případě, že znáte každého poskytovatele, kde se vaše vlastní operace použije.</span><span class="sxs-lookup"><span data-stu-id="6721e-113">This approach only works if you know every provider where your custom operation will be applied.</span></span>

## <a name="using-a-migrationoperation"></a><span data-ttu-id="6721e-114">Použití MigrationOperation</span><span class="sxs-lookup"><span data-stu-id="6721e-114">Using a MigrationOperation</span></span>

<span data-ttu-id="6721e-115">Chcete-li oddělit vlastní operaci z databáze SQL, můžete definovat vlastní `MigrationOperation` pro reprezentaci.</span><span class="sxs-lookup"><span data-stu-id="6721e-115">To decouple the custom operation from the SQL, you can define your own `MigrationOperation` to represent it.</span></span> <span data-ttu-id="6721e-116">Tato operace je pak předána poskytovateli, aby mohla určit vhodný SQL pro vygenerování.</span><span class="sxs-lookup"><span data-stu-id="6721e-116">The operation is then passed to the provider so it can determine the appropriate SQL to generate.</span></span>

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

<span data-ttu-id="6721e-117">Pomocí tohoto přístupu metoda rozšíření stačí k `MigrationBuilder.Operations`přidat jednu z těchto operací.</span><span class="sxs-lookup"><span data-stu-id="6721e-117">With this approach, the extension method just needs to add one of these operations to `MigrationBuilder.Operations`.</span></span>

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

<span data-ttu-id="6721e-118">Tento přístup vyžaduje, aby každý zprostředkovatel znal, jak vygenerovat SQL pro tuto operaci ve své `IMigrationsSqlGenerator` službě.</span><span class="sxs-lookup"><span data-stu-id="6721e-118">This approach requires each provider to know how to generate SQL for this operation in their `IMigrationsSqlGenerator` service.</span></span> <span data-ttu-id="6721e-119">Tady je příklad přepsání generátoru SQL Server pro zpracování nové operace.</span><span class="sxs-lookup"><span data-stu-id="6721e-119">Here is an example overriding the SQL Server's generator to handle the new operation.</span></span>

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

<span data-ttu-id="6721e-120">Nahraďte výchozí migraci služby generátoru SQL pomocí aktualizované verze.</span><span class="sxs-lookup"><span data-stu-id="6721e-120">Replace the default migrations sql generator service with the updated one.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
