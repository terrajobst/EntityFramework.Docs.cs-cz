---
title: "Vlastní migrace Operations - EF jádra"
author: bricelam
ms.author: bricelam
ms.date: 11/7/2017
ms.technology: entity-framework-core
ms.openlocfilehash: d41409dee034e84d22092a5f9111dd79c87dcec3
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
<a name="custom-migrations-operations"></a><span data-ttu-id="f75c6-102">Operace vlastní migrace</span><span class="sxs-lookup"><span data-stu-id="f75c6-102">Custom Migrations Operations</span></span>
============================
<span data-ttu-id="f75c6-103">Rozhraní API MigrationBuilder umožňuje provádět mnoho různých druhů operations během migrace, ale je daleko od vyčerpávající.</span><span class="sxs-lookup"><span data-stu-id="f75c6-103">The MigrationBuilder API allows you to perform many different kinds of operations during a migration, but it's far from exhaustive.</span></span> <span data-ttu-id="f75c6-104">Rozhraní API je však také rozšiřitelný, což umožňuje definovat vlastní operace.</span><span class="sxs-lookup"><span data-stu-id="f75c6-104">However, the API is also extensible allowing you to define your own operations.</span></span> <span data-ttu-id="f75c6-105">Existují dva způsoby, jak rozšířit rozhraní API: pomocí `Sql()` metoda, nebo tak, že definujete vlastní `MigrationOperation` objekty.</span><span class="sxs-lookup"><span data-stu-id="f75c6-105">There are two ways to extend the API: Using the `Sql()` method, or by defining custom `MigrationOperation` objects.</span></span>

<span data-ttu-id="f75c6-106">Pro ilustraci, podíváme se na implementace operace, která vytvoří uživatele databáze pomocí jednotlivých přístupů.</span><span class="sxs-lookup"><span data-stu-id="f75c6-106">To illustrate, let's look at implementing an operation that creates a database user using each approach.</span></span> <span data-ttu-id="f75c6-107">V našem migrace chceme povolit zápis následující kód:</span><span class="sxs-lookup"><span data-stu-id="f75c6-107">In our migrations, we want to enable writing the following code:</span></span>

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

<a name="using-migrationbuildersql"></a><span data-ttu-id="f75c6-108">Pomocí MigrationBuilder.Sql()</span><span class="sxs-lookup"><span data-stu-id="f75c6-108">Using MigrationBuilder.Sql()</span></span>
----------------------------
<span data-ttu-id="f75c6-109">Nejjednodušší způsob, jak implementovat vlastní operace je definují metody rozšíření, která volá `MigrationBuilder.Sql()`.</span><span class="sxs-lookup"><span data-stu-id="f75c6-109">The easiest way to implement a custom operation is to define an extension method that calls `MigrationBuilder.Sql()`.</span></span>
<span data-ttu-id="f75c6-110">Tady je příklad, který generuje odpovídající Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="f75c6-110">Here is an example that generates the appropriate Transact-SQL.</span></span>

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

<span data-ttu-id="f75c6-111">Pokud vaše migrace potřebujete podporu více poskytovatelů databáze, můžete použít `MigrationBuilder.ActiveProvider` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="f75c6-111">If your migrations need to support multiple database providers, you can use the `MigrationBuilder.ActiveProvider` property.</span></span> <span data-ttu-id="f75c6-112">Tady je příklad podpora Microsoft SQL Server a PostreSQL.</span><span class="sxs-lookup"><span data-stu-id="f75c6-112">Here's an example supporting both Microsoft SQL Server and PostreSQL.</span></span>

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

<span data-ttu-id="f75c6-113">Tento postup funguje jenom v případě vědět každý poskytovatel kde bude použito vlastní operaci.</span><span class="sxs-lookup"><span data-stu-id="f75c6-113">This approach only works if you know every provider where your custom operation will be applied.</span></span>

<a name="using-a-migrationoperation"></a><span data-ttu-id="f75c6-114">Použití MigrationOperation</span><span class="sxs-lookup"><span data-stu-id="f75c6-114">Using a MigrationOperation</span></span>
---------------------------
<span data-ttu-id="f75c6-115">Oddělit vlastní operaci provést z SQL, můžete definovat vlastní `MigrationOperation` představují ho.</span><span class="sxs-lookup"><span data-stu-id="f75c6-115">To decouple the custom operation from the SQL, you can define your own `MigrationOperation` to represent it.</span></span> <span data-ttu-id="f75c6-116">Operaci se potom předána zprostředkovateli tak může zjistit, příslušné SQL pro generování.</span><span class="sxs-lookup"><span data-stu-id="f75c6-116">The operation is then passed to the provider so it can determine the appropriate SQL to generate.</span></span>

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

<span data-ttu-id="f75c6-117">S tímto přístupem metodě rozšíření právě musí přidat jednu z těchto operací na `MigrationBuilder.Operations`.</span><span class="sxs-lookup"><span data-stu-id="f75c6-117">With this approach, the extension method just needs to add one of these operations to `MigrationBuilder.Operations`.</span></span>

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

<span data-ttu-id="f75c6-118">Tento přístup vyžaduje každý poskytovatel vědět, jak generovat SQL pro tuto operaci v jejich `IMigrationsSqlGenerator` služby.</span><span class="sxs-lookup"><span data-stu-id="f75c6-118">This approach requires each provider to know how to generate SQL for this operation in their `IMigrationsSqlGenerator` service.</span></span> <span data-ttu-id="f75c6-119">Tady je příklad přepsání generátor SQL Server pro operaci nového zpracování.</span><span class="sxs-lookup"><span data-stu-id="f75c6-119">Here is an example overriding the SQL Server's generator to handle the new operation.</span></span>

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

<span data-ttu-id="f75c6-120">Výchozí služba generátor sql migrace nahradíte aktualizovanou položkou.</span><span class="sxs-lookup"><span data-stu-id="f75c6-120">Replace the default migrations sql generator service with the updated one.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
