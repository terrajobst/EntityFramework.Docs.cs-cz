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
<a name="migrations-with-multiple-providers"></a><span data-ttu-id="b5259-102">Migrace s několika poskytovateli</span><span class="sxs-lookup"><span data-stu-id="b5259-102">Migrations with Multiple Providers</span></span>
==================================
<span data-ttu-id="b5259-103">[EF základní nástroje] [ 1] pouze vygenerovat migrace pro aktivní zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="b5259-103">The [EF Core Tools][1] only scaffold migrations for the active provider.</span></span> <span data-ttu-id="b5259-104">V některých případech ale můžete použít více než jednoho poskytovatele (například Microsoft SQL Server a SQLite) s vaší DbContext.</span><span class="sxs-lookup"><span data-stu-id="b5259-104">Sometimes, however, you may want to use more than one provider (for example Microsoft SQL Server and SQLite) with your DbContext.</span></span> <span data-ttu-id="b5259-105">Existují dva způsoby, jak to postarají s migrací.</span><span class="sxs-lookup"><span data-stu-id="b5259-105">There are two ways to handle this with Migrations.</span></span> <span data-ttu-id="b5259-106">Je možné uchovávat dvě sady migrací – jednu pro každý poskytovatel--nebo sloučení do jedné sady, která může pracovat na obojí.</span><span class="sxs-lookup"><span data-stu-id="b5259-106">You can maintain two sets of migrations--one for each provider--or merge them into a single set that can work on both.</span></span>

<a name="two-migration-sets"></a><span data-ttu-id="b5259-107">Dvě sady migrace</span><span class="sxs-lookup"><span data-stu-id="b5259-107">Two migration sets</span></span>
------------------
<span data-ttu-id="b5259-108">V prvním přístupem je generovat dvě migrace u každé změny modelu.</span><span class="sxs-lookup"><span data-stu-id="b5259-108">In the first approach, you generate two migrations for each model change.</span></span>

<span data-ttu-id="b5259-109">Jeden ze způsobů, jak provést toto je umístění každé sadě migrace [v samostatném sestavení] [ 2] a ručně přepnout active zprostředkovatele (a migrace sestavení) mezi přidáním dvě migrace.</span><span class="sxs-lookup"><span data-stu-id="b5259-109">One way to do this is to put each migration set [in a separate assembly][2] and manually switch the active provider (and migrations assembly) between adding the two migrations.</span></span>

<span data-ttu-id="b5259-110">Jiná možnost, že je díky práci s nástroji pro vytvoření nového typu odvozená od vašeho DbContext a přepíše active zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="b5259-110">Another approach that makes working with the tools easier is to create a new type derives from your DbContext and overrides the active provider.</span></span> <span data-ttu-id="b5259-111">Tento typ se používá v návrhu čas při přidávání nebo použití migrace.</span><span class="sxs-lookup"><span data-stu-id="b5259-111">This type is used at design time when adding or applying migrations.</span></span>

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> <span data-ttu-id="b5259-112">Protože každá sada migrace používá vlastní typy DbContext, nevyžaduje tento přístup pomocí samostatné migrace sestavení.</span><span class="sxs-lookup"><span data-stu-id="b5259-112">Since each migration set uses its own DbContext types, this approach doesn't require using a separate migrations assembly.</span></span>

<span data-ttu-id="b5259-113">Při přidávání nové migrace, určete typy kontextu.</span><span class="sxs-lookup"><span data-stu-id="b5259-113">When adding new migration, specify the context types.</span></span>

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```
``` Console
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

> [!TIP]
> <span data-ttu-id="b5259-114">Nemusíte určit výstupního adresáře pro následné migrace, protože se vytvářejí na stejné úrovni na poslední.</span><span class="sxs-lookup"><span data-stu-id="b5259-114">You don't need to specify the output directory for subsequent migrations since they are created as siblings to the last one.</span></span>

<a name="one-migration-set"></a><span data-ttu-id="b5259-115">Sada jeden migrace</span><span class="sxs-lookup"><span data-stu-id="b5259-115">One migration set</span></span>
-----------------
<span data-ttu-id="b5259-116">Pokud chcete mít dvě sady migrace, můžete je ručně zkombinovat do jedné sady, který lze použít na obou zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="b5259-116">If you don't like having two sets of migrations, you can manually combine them into a single set that can be applied to both providers.</span></span>

<span data-ttu-id="b5259-117">Vzhledem k tomu, že zprostředkovatel ignoruje žádné poznámky, které není pochopit, může existovat současně poznámky.</span><span class="sxs-lookup"><span data-stu-id="b5259-117">Annotations can coexist since a provider ignores any annotations that it doesn't understand.</span></span> <span data-ttu-id="b5259-118">Sloupec primárního klíče, který funguje s Microsoft SQL Server a SQLite může například vypadat například takto.</span><span class="sxs-lookup"><span data-stu-id="b5259-118">For example, a primary key column that works with both Microsoft SQL Server and SQLite might look like this.</span></span>

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

<span data-ttu-id="b5259-119">Pokud operace lze použít pouze na jednoho poskytovatele (nebo jinak jsou mezi zprostředkovatelé), použijte `ActiveProvider` vlastnost říct poskytovatele, kterého je aktivní.</span><span class="sxs-lookup"><span data-stu-id="b5259-119">If operations can only be applied on one provider (or they're differently between providers), use the `ActiveProvider` property to tell which provider is active.</span></span>

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```


  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
