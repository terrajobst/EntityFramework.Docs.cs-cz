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
<a name="migrations-with-multiple-providers"></a><span data-ttu-id="061a1-102">Migrace s více poskytovatelů</span><span class="sxs-lookup"><span data-stu-id="061a1-102">Migrations with Multiple Providers</span></span>
==================================
<span data-ttu-id="061a1-103">[EF Core Tools] [ 1] pouze generování uživatelského rozhraní migrace pro aktivní zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="061a1-103">The [EF Core Tools][1] only scaffold migrations for the active provider.</span></span> <span data-ttu-id="061a1-104">V některých případech však můžete chtít použít více než jednoho poskytovatele (třeba Microsoft SQL Server a SQLite) s vaší DbContext.</span><span class="sxs-lookup"><span data-stu-id="061a1-104">Sometimes, however, you may want to use more than one provider (for example Microsoft SQL Server and SQLite) with your DbContext.</span></span> <span data-ttu-id="061a1-105">Existují dva způsoby, jak o to postarají s migrací.</span><span class="sxs-lookup"><span data-stu-id="061a1-105">There are two ways to handle this with Migrations.</span></span> <span data-ttu-id="061a1-106">Abyste mohli pro dvě sady migrace – jednu pro každého poskytovatele--nebo sloučení je jedno nastavení, která může pracovat na obojí.</span><span class="sxs-lookup"><span data-stu-id="061a1-106">You can maintain two sets of migrations--one for each provider--or merge them into a single set that can work on both.</span></span>

<a name="two-migration-sets"></a><span data-ttu-id="061a1-107">Dvě sady migrace</span><span class="sxs-lookup"><span data-stu-id="061a1-107">Two migration sets</span></span>
------------------
<span data-ttu-id="061a1-108">V první metodě generovat dvě migrace u každé změny modelu.</span><span class="sxs-lookup"><span data-stu-id="061a1-108">In the first approach, you generate two migrations for each model change.</span></span>

<span data-ttu-id="061a1-109">Jeden ze způsobů, jak provést to je umístění jednotlivých sad migrace [v samostatném sestavení] [ 2] a ručně přepínat aktivní zprostředkovatel (a migrace sestavení) přidání dvě migrace.</span><span class="sxs-lookup"><span data-stu-id="061a1-109">One way to do this is to put each migration set [in a separate assembly][2] and manually switch the active provider (and migrations assembly) between adding the two migrations.</span></span>

<span data-ttu-id="061a1-110">Další možností, která usnadňuje práci s nástroji je vytvoření nového typu, který je odvozen od vaší DbContext a přepíše aktivní zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="061a1-110">Another approach that makes working with the tools easier is to create a new type that derives from your DbContext and overrides the active provider.</span></span> <span data-ttu-id="061a1-111">Tento typ se používá při návrhu při přidání nebo použití migrace.</span><span class="sxs-lookup"><span data-stu-id="061a1-111">This type is used at design time when adding or applying migrations.</span></span>

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> <span data-ttu-id="061a1-112">Protože každá sada migrace používá vlastní typy DbContext, nevyžaduje tento přístup pomocí migrace samostatného sestavení.</span><span class="sxs-lookup"><span data-stu-id="061a1-112">Since each migration set uses its own DbContext types, this approach doesn't require using a separate migrations assembly.</span></span>

<span data-ttu-id="061a1-113">Při přidávání nové migraci, určete typy kontextu.</span><span class="sxs-lookup"><span data-stu-id="061a1-113">When adding new migration, specify the context types.</span></span>

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```
``` Console
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

> [!TIP]
> <span data-ttu-id="061a1-114">Není nutné určit výstupní adresář pro další migrace, protože byly vytvořeny jako na stejné úrovni jako poslední.</span><span class="sxs-lookup"><span data-stu-id="061a1-114">You don't need to specify the output directory for subsequent migrations since they are created as siblings to the last one.</span></span>

<a name="one-migration-set"></a><span data-ttu-id="061a1-115">Migrace jedné sady</span><span class="sxs-lookup"><span data-stu-id="061a1-115">One migration set</span></span>
-----------------
<span data-ttu-id="061a1-116">Pokud se vám s dvě sady migrace, můžete je ručně kombinovat do jediné sady, který lze použít na obou zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="061a1-116">If you don't like having two sets of migrations, you can manually combine them into a single set that can be applied to both providers.</span></span>

<span data-ttu-id="061a1-117">Poznámky můžou existovat společně, protože zprostředkovatele ignoruje jakékoli poznámky, které ho nerozumí.</span><span class="sxs-lookup"><span data-stu-id="061a1-117">Annotations can coexist since a provider ignores any annotations that it doesn't understand.</span></span> <span data-ttu-id="061a1-118">Například to sloupec primárního klíče, který spolupracuje s Microsoft SQL Server a SQLite může vypadat takto.</span><span class="sxs-lookup"><span data-stu-id="061a1-118">For example, a primary key column that works with both Microsoft SQL Server and SQLite might look like this.</span></span>

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

<span data-ttu-id="061a1-119">Pokud operace lze použít pouze na jeden zprostředkovatel (nebo jinak jsou mezi poskytovatele), použijte `ActiveProvider` vlastnost zjistit, které poskytovatel aktivní.</span><span class="sxs-lookup"><span data-stu-id="061a1-119">If operations can only be applied on one provider (or they're differently between providers), use the `ActiveProvider` property to tell which provider is active.</span></span>

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```


  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
