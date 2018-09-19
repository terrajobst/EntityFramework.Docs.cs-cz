---
title: Operace s vlastní migrací – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
ms.openlocfilehash: d715fe0408f25eb75c3160af79bb98fc87e41b17
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/18/2018
ms.locfileid: "46284067"
---
<a name="custom-migrations-operations"></a><span data-ttu-id="bc593-102">Operace vlastní migrace</span><span class="sxs-lookup"><span data-stu-id="bc593-102">Custom Migrations Operations</span></span>
============================
<span data-ttu-id="bc593-103">Rozhraní API MigrationBuilder lze provádět mnoho různých typů operací během migrace, ale je daleko od vyčerpávající.</span><span class="sxs-lookup"><span data-stu-id="bc593-103">The MigrationBuilder API allows you to perform many different kinds of operations during a migration, but it's far from exhaustive.</span></span> <span data-ttu-id="bc593-104">Rozhraní API je však také rozšiřitelný, umožňuje definovat vlastní operace.</span><span class="sxs-lookup"><span data-stu-id="bc593-104">However, the API is also extensible allowing you to define your own operations.</span></span> <span data-ttu-id="bc593-105">Existují dva způsoby, jak rozšířit rozhraní API: použití `Sql()` metodu, nebo tak, že definujete vlastní `MigrationOperation` objekty.</span><span class="sxs-lookup"><span data-stu-id="bc593-105">There are two ways to extend the API: Using the `Sql()` method, or by defining custom `MigrationOperation` objects.</span></span>

<span data-ttu-id="bc593-106">Pro ilustraci, Podívejme se na implementaci operace, která vytvoří uživatele databáze s využitím každý přístup.</span><span class="sxs-lookup"><span data-stu-id="bc593-106">To illustrate, let's look at implementing an operation that creates a database user using each approach.</span></span> <span data-ttu-id="bc593-107">V našem migrace chceme umožnit zápis následující kód:</span><span class="sxs-lookup"><span data-stu-id="bc593-107">In our migrations, we want to enable writing the following code:</span></span>

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

<a name="using-migrationbuildersql"></a><span data-ttu-id="bc593-108">Pomocí MigrationBuilder.Sql()</span><span class="sxs-lookup"><span data-stu-id="bc593-108">Using MigrationBuilder.Sql()</span></span>
----------------------------
<span data-ttu-id="bc593-109">Nejjednodušší způsob, jak implementovat vlastní operace se má definovat rozšiřující metodu, která volá `MigrationBuilder.Sql()`.</span><span class="sxs-lookup"><span data-stu-id="bc593-109">The easiest way to implement a custom operation is to define an extension method that calls `MigrationBuilder.Sql()`.</span></span>
<span data-ttu-id="bc593-110">Tady je příklad, který generuje příslušné příkazů jazyka Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="bc593-110">Here is an example that generates the appropriate Transact-SQL.</span></span>

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

<span data-ttu-id="bc593-111">Pokud vaše migrace potřebovat pro podporu více poskytovatelů pro databáze, můžete použít `MigrationBuilder.ActiveProvider` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="bc593-111">If your migrations need to support multiple database providers, you can use the `MigrationBuilder.ActiveProvider` property.</span></span> <span data-ttu-id="bc593-112">Tady je příklad podporuje Microsoft SQL Server využívající databázi PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="bc593-112">Here's an example supporting both Microsoft SQL Server and PostgreSQL.</span></span>

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

<span data-ttu-id="bc593-113">Tento postup funguje jenom v případě znát každý zprostředkovatele kde se budou aplikovat vaše vlastní operace.</span><span class="sxs-lookup"><span data-stu-id="bc593-113">This approach only works if you know every provider where your custom operation will be applied.</span></span>

<a name="using-a-migrationoperation"></a><span data-ttu-id="bc593-114">Použití MigrationOperation</span><span class="sxs-lookup"><span data-stu-id="bc593-114">Using a MigrationOperation</span></span>
---------------------------
<span data-ttu-id="bc593-115">Pokud chcete oddělit vlastní operace v SQL, můžete definovat vlastní `MigrationOperation` který ho zastupuje.</span><span class="sxs-lookup"><span data-stu-id="bc593-115">To decouple the custom operation from the SQL, you can define your own `MigrationOperation` to represent it.</span></span> <span data-ttu-id="bc593-116">Operace je pak předán zprostředkovatel tak může zjistit, správný příkaz SQL pro generování.</span><span class="sxs-lookup"><span data-stu-id="bc593-116">The operation is then passed to the provider so it can determine the appropriate SQL to generate.</span></span>

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

<span data-ttu-id="bc593-117">S tímto přístupem jenom potřebuje rozšiřující metoda pro přidání jednoho z těchto operací `MigrationBuilder.Operations`.</span><span class="sxs-lookup"><span data-stu-id="bc593-117">With this approach, the extension method just needs to add one of these operations to `MigrationBuilder.Operations`.</span></span>

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

<span data-ttu-id="bc593-118">Tento přístup vyžaduje každý poskytovatel vědět, jak ke generování SQL pro tuto operaci v jejich `IMigrationsSqlGenerator` služby.</span><span class="sxs-lookup"><span data-stu-id="bc593-118">This approach requires each provider to know how to generate SQL for this operation in their `IMigrationsSqlGenerator` service.</span></span> <span data-ttu-id="bc593-119">Tady je příklad přepsání generátor SQL Server pro novou operaci zpracování.</span><span class="sxs-lookup"><span data-stu-id="bc593-119">Here is an example overriding the SQL Server's generator to handle the new operation.</span></span>

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

<span data-ttu-id="bc593-120">Aktualizované nahradíte výchozí migrace sql generátoru service.</span><span class="sxs-lookup"><span data-stu-id="bc593-120">Replace the default migrations sql generator service with the updated one.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
