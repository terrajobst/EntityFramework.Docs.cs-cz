---
title: Migrace - EF jádra
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: dd164125c053497af94773011127853ad10d27a6
ms.sourcegitcommit: 72e59e6af86b568653e1b29727529dfd7f65d312
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34754506"
---
<a name="migrations"></a><span data-ttu-id="c4bdb-102">Migrace</span><span class="sxs-lookup"><span data-stu-id="c4bdb-102">Migrations</span></span>
==========
<span data-ttu-id="c4bdb-103">Migrace poskytují způsob, jak přírůstkově použít změny schématu k databázi a udržovat synchronizované se svým modelem EF základní současně v něm zachovat stávající data v databázi.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-103">Migrations provide a way to incrementally apply schema changes to the database to keep it in sync with your EF Core model while preserving existing data in the database.</span></span>

<a name="creating-the-database"></a><span data-ttu-id="c4bdb-104">Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="c4bdb-104">Creating the database</span></span>
---------------------
<span data-ttu-id="c4bdb-105">Poté, co jste [definované počáteční modelu][1], je čas vytvořit databázi.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-105">After you've [defined your initial model][1], it's time to create the database.</span></span> <span data-ttu-id="c4bdb-106">Chcete-li to provést, přidejte počáteční migrace.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-106">To do this, add an initial migration.</span></span>
<span data-ttu-id="c4bdb-107">Nainstalujte [EF základní nástroje] [ 2] a spusťte příslušný příkaz.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-107">Install the [EF Core Tools][2] and run the appropriate command.</span></span>

``` powershell
Add-Migration InitialCreate
```
``` Console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="c4bdb-108">Tři soubory budou přidány do projektu v části **migrace** directory:</span><span class="sxs-lookup"><span data-stu-id="c4bdb-108">Three files are added to your project under the **Migrations** directory:</span></span>

* <span data-ttu-id="c4bdb-109">**00000000000000_InitialCreate.cs**– soubor hlavní migrace.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-109">**00000000000000_InitialCreate.cs**--The main migrations file.</span></span> <span data-ttu-id="c4bdb-110">Obsahuje nezbytné uplatňovat migrace operace (v `Up()`) a vraťte se zpátky (v `Down()`).</span><span class="sxs-lookup"><span data-stu-id="c4bdb-110">Contains the operations necessary to apply the migration (in `Up()`) and to revert it (in `Down()`).</span></span>
* <span data-ttu-id="c4bdb-111">**00000000000000_InitialCreate.Designer.cs**– soubor metadat migrace.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-111">**00000000000000_InitialCreate.Designer.cs**--The migrations metadata file.</span></span> <span data-ttu-id="c4bdb-112">Obsahuje informace o používané EF.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-112">Contains information used by EF.</span></span>
* <span data-ttu-id="c4bdb-113">**MyContextModelSnapshot.cs**– snímek aktuální model.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-113">**MyContextModelSnapshot.cs**--A snapshot of your current model.</span></span> <span data-ttu-id="c4bdb-114">Použít k určení, co se změnilo při přidávání pro další migraci.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-114">Used to determine what changed when adding the next migration.</span></span>

<span data-ttu-id="c4bdb-115">Časové razítko v názvu souboru pomáhá udržovat seřadit časovém pořadí, abyste viděli průběh změny.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-115">The timestamp in the filename helps keep them ordered chronologically so you can see the progression of changes.</span></span>

> [!TIP]
> <span data-ttu-id="c4bdb-116">Jste volné k přesunutí souborů migrace a změnit jejich obor názvů.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-116">You are free to move Migrations files and change their namespace.</span></span> <span data-ttu-id="c4bdb-117">Nové migrace jsou vytvořené jako poslední migrace na stejné úrovni.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-117">New migrations are created as siblings of the last migration.</span></span>

<span data-ttu-id="c4bdb-118">Dále platí migrace pro databázi vytvořit schéma.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-118">Next, apply the migration to the database to create the schema.</span></span>

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="adding-another-migration"></a><span data-ttu-id="c4bdb-119">Přidání další migraci</span><span class="sxs-lookup"><span data-stu-id="c4bdb-119">Adding another migration</span></span>
------------------------
<span data-ttu-id="c4bdb-120">Po provedení změn pro svůj model EF jádra, bude synchronizován schématu databáze. A převeďte ho do aktuální, přidejte další migraci.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-120">After making changes to your EF Core model, the database schema will be out of sync. To bring it up to date, add another migration.</span></span> <span data-ttu-id="c4bdb-121">Název migrace slouží jako potvrzení zprávy v systému správy verzí.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-121">The migration name can be used like a commit message in a version control system.</span></span> <span data-ttu-id="c4bdb-122">Například pokud byly provedeny změny uložte zákazníka recenzemi produktů, I může zvolit něco podobného jako *AddProductReviews*.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-122">For example, if I made changes to save customer reviews of products, I might choose something like *AddProductReviews*.</span></span>

``` powershell
Add-Migration AddProductReviews
```
``` Console
dotnet ef migrations add AddProductReviews
```

<span data-ttu-id="c4bdb-123">Po migraci je generované uživatelské rozhraní, zkontrolujte přesnost a přidejte jakékoli další operace vyžaduje správně používat.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-123">Once the migration is scaffolded, you should review it for accuracy and add any additional operations required to apply it correctly.</span></span> <span data-ttu-id="c4bdb-124">Migrace může například obsahovat následující operace:</span><span class="sxs-lookup"><span data-stu-id="c4bdb-124">For example, your migration might contain the following operations:</span></span>

``` csharp
migrationBuilder.DropColumn(
    name: "FirstName",
    table: "Customer");

migrationBuilder.DropColumn(
    name: "LastName",
    table: "Customer");

migrationBuilder.AddColumn<string>(
    name: "Name",
    table: "Customer",
    nullable: true);
```

<span data-ttu-id="c4bdb-125">Při těchto operací upravit schéma databáze kompatibilní, neuchovají se stávajícími názvy zákazníka.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-125">While these operations make the database schema compatible, they don't preserve the existing customer names.</span></span> <span data-ttu-id="c4bdb-126">Chcete-li lépe, přepište ho následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-126">To make it better, rewrite it as follows.</span></span>

``` csharp
migrationBuilder.AddColumn<string>(
    name: "Name",
    table: "Customer",
    nullable: true);

migrationBuilder.Sql(
@"
    UPDATE Customer
    SET Name = FirstName + ' ' + LastName;
");

migrationBuilder.DropColumn(
    name: "FirstName",
    table: "Customer");

migrationBuilder.DropColumn(
    name: "LastName",
    table: "Customer");
```

> [!TIP]
> <span data-ttu-id="c4bdb-127">Přidání nové migrace upozorní, když je vygeneroval operace, které může způsobit ztrátu dat (např. vyřazení sloupce).</span><span class="sxs-lookup"><span data-stu-id="c4bdb-127">Adding a new migration warns when an operation is scaffolded that may result in data loss (like dropping a column).</span></span> <span data-ttu-id="c4bdb-128">Ujistěte se, že zejména Zkontrolujte tyto migrace přesnost.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-128">Be sure to especially review these migrations for accuracy.</span></span>

<span data-ttu-id="c4bdb-129">Migrace se vztahují na databázi pomocí příslušný příkaz.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-129">Apply the migration to the database using the appropriate command.</span></span>

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="removing-a-migration"></a><span data-ttu-id="c4bdb-130">Odebrání migrace</span><span class="sxs-lookup"><span data-stu-id="c4bdb-130">Removing a migration</span></span>
--------------------
<span data-ttu-id="c4bdb-131">Někdy přidejte migrace a Uvědomte si, že budete muset udělat další změny modelu EF základní před každým jejím použitím.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-131">Sometimes you add a migration and realize you need to make additional changes to your EF Core model before applying it.</span></span>
<span data-ttu-id="c4bdb-132">Chcete-li odebrat poslední migrace, použijte tento příkaz.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-132">To remove the last migration, use this command.</span></span>

``` powershell
Remove-Migration
```
``` Console
dotnet ef migrations remove
```

<span data-ttu-id="c4bdb-133">Po odebrání, můžete změnit další modelu a ji znovu přidejte.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-133">After removing it, you can make the additional model changes and add it again.</span></span>

<a name="reverting-a-migration"></a><span data-ttu-id="c4bdb-134">Vrácení migrace</span><span class="sxs-lookup"><span data-stu-id="c4bdb-134">Reverting a migration</span></span>
---------------------
<span data-ttu-id="c4bdb-135">Pokud jste již nainstalovali migrace (nebo několik migrace) do databáze, ale potřeba vraťte se zpátky, můžete stejný příkaz použijte migrace, ale zadejte název migrace, které chcete vrátit zpět.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-135">If you already applied a migration (or several migrations) to the database but need to revert it, you can use the same command to apply migrations, but specify the name of the migration you want to roll back to.</span></span>

``` powershell
Update-Database LastGoodMigration
```
``` Console
dotnet ef database update LastGoodMigration
```

<a name="empty-migrations"></a><span data-ttu-id="c4bdb-136">Prázdný migrace</span><span class="sxs-lookup"><span data-stu-id="c4bdb-136">Empty migrations</span></span>
----------------
<span data-ttu-id="c4bdb-137">Někdy je užitečné pro přidání migrace bez jakýchkoli změn modelu.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-137">Sometimes it's useful to add a migration without making any model changes.</span></span> <span data-ttu-id="c4bdb-138">V takovém případě přidávání nové migrace vytvoří některého prázdný.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-138">In this case, adding a new migration creates an empty one.</span></span> <span data-ttu-id="c4bdb-139">Tato migrace k provádění operací, které přímo nesouvisejí model EF základní můžete přizpůsobit.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-139">You can customize this migration to perform operations that don't directly relate to the EF Core model.</span></span>
<span data-ttu-id="c4bdb-140">Některé věci, které můžete chtít spravovat tímto způsobem jsou:</span><span class="sxs-lookup"><span data-stu-id="c4bdb-140">Some things you might want to manage this way are:</span></span>

* <span data-ttu-id="c4bdb-141">Fulltextové vyhledávání</span><span class="sxs-lookup"><span data-stu-id="c4bdb-141">Full-Text Search</span></span>
* <span data-ttu-id="c4bdb-142">Funkce</span><span class="sxs-lookup"><span data-stu-id="c4bdb-142">Functions</span></span>
* <span data-ttu-id="c4bdb-143">Uložené procedury</span><span class="sxs-lookup"><span data-stu-id="c4bdb-143">Stored procedures</span></span>
* <span data-ttu-id="c4bdb-144">Aktivační procedury</span><span class="sxs-lookup"><span data-stu-id="c4bdb-144">Triggers</span></span>
* <span data-ttu-id="c4bdb-145">zobrazení</span><span class="sxs-lookup"><span data-stu-id="c4bdb-145">Views</span></span>
* <span data-ttu-id="c4bdb-146">Atd.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-146">etc.</span></span>

<a name="generating-a-sql-script"></a><span data-ttu-id="c4bdb-147">Generování skriptu SQL</span><span class="sxs-lookup"><span data-stu-id="c4bdb-147">Generating a SQL script</span></span>
-----------------------
<span data-ttu-id="c4bdb-148">Při ladění vaše migrace nebo jejich nasazení do provozní databáze, je užitečné pro generování skriptu SQL.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-148">When debugging your migrations or deploying them to a production database, it's useful to generate a SQL script.</span></span> <span data-ttu-id="c4bdb-149">Skript můžete mít další zkontrolovat přesnost a přizpůsobená podle potřeb z provozní databáze.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-149">The script can then be further reviewed for accuracy and tuned to fit the needs of a production database.</span></span> <span data-ttu-id="c4bdb-150">Skript lze také použít ve spojení s technologií nasazení.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-150">The script can also be used in conjunction with a deployment technology.</span></span> <span data-ttu-id="c4bdb-151">Příkaz základní je následující.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-151">The basic command is as follows.</span></span>

``` powershell
Script-Migration
```
``` Console
dotnet ef migrations script
```

<span data-ttu-id="c4bdb-152">Existuje několik možností pro tento příkaz.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-152">There are several options to this command.</span></span>

<span data-ttu-id="c4bdb-153">**z** migrace by měl být poslední migrace do databáze použít před spuštěním skriptu.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-153">The **from** migration should be the last migration applied to the database before running the script.</span></span> <span data-ttu-id="c4bdb-154">Pokud se neaplikovaly žádné migrace, zadejte `0` (Toto je výchozí hodnota).</span><span class="sxs-lookup"><span data-stu-id="c4bdb-154">If no migrations have been applied, specify `0` (this is the default).</span></span>

<span data-ttu-id="c4bdb-155">**k** migrace je poslední migrace, která bude použita k databázi, po spuštění skriptu.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-155">The **to** migration is the last migration that will be applied to the database after running the script.</span></span> <span data-ttu-id="c4bdb-156">Toto výchozí nastavení na poslední migrace ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-156">This defaults to the last migration in your project.</span></span>

<span data-ttu-id="c4bdb-157">**Idempotent** skript může volitelně generovat.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-157">An **idempotent** script can optionally be generated.</span></span> <span data-ttu-id="c4bdb-158">Tento skript migrací platí pouze, pokud již nebyla použita k databázi.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-158">This script only applies migrations if they haven't already been applied to the database.</span></span> <span data-ttu-id="c4bdb-159">To je užitečné, pokud neznáte přesně co do databáze použít poslední migrace byla nebo Pokud nasazujete více databází, které může být v různých migrace.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-159">This is useful if you don't exactly know what the last migration applied to the database was or if you are deploying to multiple databases that may each be at a different migration.</span></span>

<a name="applying-migrations-at-runtime"></a><span data-ttu-id="c4bdb-160">Použití migrace za běhu</span><span class="sxs-lookup"><span data-stu-id="c4bdb-160">Applying migrations at runtime</span></span>
------------------------------
<span data-ttu-id="c4bdb-161">Některé aplikace může chtít použít migrace za běhu při spuštění nebo prvním spuštění.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-161">Some apps may want to apply migrations at runtime during startup or first run.</span></span> <span data-ttu-id="c4bdb-162">To provést pomocí `Migrate()` metoda.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-162">Do this using the `Migrate()` method.</span></span>

<span data-ttu-id="c4bdb-163">Upozornění: Tento přístup není pro všechny uživatele.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-163">Caution, this approach isn't for everyone.</span></span> <span data-ttu-id="c4bdb-164">I když je skvělá pro aplikace s místní databází, bude vyžadovat většinu aplikací robustnější strategii nasazení, jako je generování skriptů SQL.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-164">While it's great for apps with a local database, most applications will require more robust deployment strategy like generating SQL scripts.</span></span>

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
> <span data-ttu-id="c4bdb-165">Nemůžete volat `EnsureCreated()` před `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-165">Don't call `EnsureCreated()` before `Migrate()`.</span></span> <span data-ttu-id="c4bdb-166">`EnsureCreated()` obchází migrace vytvořit schéma, což způsobí, že `Migrate()` selhání.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-166">`EnsureCreated()` bypasses Migrations to create the schema, which causes `Migrate()` to fail.</span></span>

> [!NOTE]
> <span data-ttu-id="c4bdb-167">Tato metoda vytvoří na `IMigrator` služby, která lze použít pro pokročilejší scénáře.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-167">This method builds on top of the `IMigrator` service, which can be used for more advanced scenarios.</span></span> <span data-ttu-id="c4bdb-168">Použití `DbContext.GetService<IMigrator>()` k přístupu.</span><span class="sxs-lookup"><span data-stu-id="c4bdb-168">Use `DbContext.GetService<IMigrator>()` to access it.</span></span>


  [1]: ../../modeling/index.md
  [2]: ../../miscellaneous/cli/index.md
