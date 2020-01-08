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
# <a name="migrations-with-multiple-providers"></a><span data-ttu-id="a511a-102">Migrace s více zprostředkovateli</span><span class="sxs-lookup"><span data-stu-id="a511a-102">Migrations with Multiple Providers</span></span>

<span data-ttu-id="a511a-103">[Nástroje pro EF Core][1] jsou jenom migrace uživatelského rozhraní pro aktivního poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="a511a-103">The [EF Core Tools][1] only scaffold migrations for the active provider.</span></span> <span data-ttu-id="a511a-104">V některých případech však můžete chtít použít více než jednoho poskytovatele (například Microsoft SQL Server a SQLite) s vaším DbContext.</span><span class="sxs-lookup"><span data-stu-id="a511a-104">Sometimes, however, you may want to use more than one provider (for example Microsoft SQL Server and SQLite) with your DbContext.</span></span> <span data-ttu-id="a511a-105">Existují dva způsoby, jak to zvládnout s migracemi.</span><span class="sxs-lookup"><span data-stu-id="a511a-105">There are two ways to handle this with Migrations.</span></span> <span data-ttu-id="a511a-106">Můžete udržovat dvě sady migrace – jeden pro každého poskytovatele – nebo je sloučit do jedné sady, která může fungovat na obou.</span><span class="sxs-lookup"><span data-stu-id="a511a-106">You can maintain two sets of migrations--one for each provider--or merge them into a single set that can work on both.</span></span>

## <a name="two-migration-sets"></a><span data-ttu-id="a511a-107">Dvě sady migrace</span><span class="sxs-lookup"><span data-stu-id="a511a-107">Two migration sets</span></span>

<span data-ttu-id="a511a-108">V prvním přístupu vygenerujete pro každou změnu modelu dvě migrace.</span><span class="sxs-lookup"><span data-stu-id="a511a-108">In the first approach, you generate two migrations for each model change.</span></span>

<span data-ttu-id="a511a-109">Jedním ze způsobů, jak to provést, je umístit každou sadu migrace [do samostatného sestavení][2] a ručně přepínat aktivní zprostředkovatele (a sestavení migrace) mezi přidáním těchto dvou migrací.</span><span class="sxs-lookup"><span data-stu-id="a511a-109">One way to do this is to put each migration set [in a separate assembly][2] and manually switch the active provider (and migrations assembly) between adding the two migrations.</span></span>

<span data-ttu-id="a511a-110">Dalším způsobem, který usnadňuje práci s nástroji, je vytvořit nový typ, který je odvozený od vašeho DbContext a přepíše aktivního poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="a511a-110">Another approach that makes working with the tools easier is to create a new type that derives from your DbContext and overrides the active provider.</span></span> <span data-ttu-id="a511a-111">Tento typ se používá v době návrhu při přidávání nebo aplikování migrace.</span><span class="sxs-lookup"><span data-stu-id="a511a-111">This type is used at design time when adding or applying migrations.</span></span>

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> <span data-ttu-id="a511a-112">Vzhledem k tomu, že každá sada migrace používá vlastní typy DbContext, tento přístup nevyžaduje použití samostatného sestavení migrace.</span><span class="sxs-lookup"><span data-stu-id="a511a-112">Since each migration set uses its own DbContext types, this approach doesn't require using a separate migrations assembly.</span></span>

<span data-ttu-id="a511a-113">Při přidávání nové migrace zadejte typy kontextu.</span><span class="sxs-lookup"><span data-stu-id="a511a-113">When adding new migration, specify the context types.</span></span>

### <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="a511a-114">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="a511a-114">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

### <a name="visual-studiotabvs"></a>[<span data-ttu-id="a511a-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a511a-115">Visual Studio</span></span>](#tab/vs)

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```

***

> [!TIP]
> <span data-ttu-id="a511a-116">Nemusíte určovat výstupní adresář pro následné migrace, protože jsou vytvořené na stejné úrovni jako poslední.</span><span class="sxs-lookup"><span data-stu-id="a511a-116">You don't need to specify the output directory for subsequent migrations since they are created as siblings to the last one.</span></span>

## <a name="one-migration-set"></a><span data-ttu-id="a511a-117">Jedna sada migrace</span><span class="sxs-lookup"><span data-stu-id="a511a-117">One migration set</span></span>

<span data-ttu-id="a511a-118">Pokud nechcete, aby se dvě sady migrace používaly, můžete je ručně zkombinovat do jedné sady, kterou je možné použít u obou zprostředkovatelů.</span><span class="sxs-lookup"><span data-stu-id="a511a-118">If you don't like having two sets of migrations, you can manually combine them into a single set that can be applied to both providers.</span></span>

<span data-ttu-id="a511a-119">Poznámky můžou existovat společně, protože poskytovatel ignoruje všechny anotace, které nerozumí.</span><span class="sxs-lookup"><span data-stu-id="a511a-119">Annotations can coexist since a provider ignores any annotations that it doesn't understand.</span></span> <span data-ttu-id="a511a-120">Například sloupec primárního klíče, který pracuje s Microsoft SQL Server i SQLite, může vypadat takto.</span><span class="sxs-lookup"><span data-stu-id="a511a-120">For example, a primary key column that works with both Microsoft SQL Server and SQLite might look like this.</span></span>

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

<span data-ttu-id="a511a-121">Pokud se operace dají použít jenom u jednoho poskytovatele (nebo se mezi poskytovateli liší), pomocí vlastnosti `ActiveProvider` sdělte, který poskytovatel je aktivní.</span><span class="sxs-lookup"><span data-stu-id="a511a-121">If operations can only be applied on one provider (or they're differently between providers), use the `ActiveProvider` property to tell which provider is active.</span></span>

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```

  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
