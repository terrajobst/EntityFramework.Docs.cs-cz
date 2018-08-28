---
title: Migrace – EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: 4a5d6f3798c7af7597f95cebea1aeb9e5e58d277
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996519"
---
<a name="migrations"></a><span data-ttu-id="e092a-102">Migrace</span><span class="sxs-lookup"><span data-stu-id="e092a-102">Migrations</span></span>
==========
<span data-ttu-id="e092a-103">Migrace poskytují způsob, jak postupně provést změny schématu databáze, kterou chcete udržovat synchronizované s EF Core model při zachování stávajících dat v databázi.</span><span class="sxs-lookup"><span data-stu-id="e092a-103">Migrations provide a way to incrementally apply schema changes to the database to keep it in sync with your EF Core model while preserving existing data in the database.</span></span>

<a name="creating-the-database"></a><span data-ttu-id="e092a-104">Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="e092a-104">Creating the database</span></span>
---------------------
<span data-ttu-id="e092a-105">Po [definované počáteční modelu][1], je čas vytvořit databázi.</span><span class="sxs-lookup"><span data-stu-id="e092a-105">After you've [defined your initial model][1], it's time to create the database.</span></span> <span data-ttu-id="e092a-106">Chcete-li to provést, přidejte počáteční migraci.</span><span class="sxs-lookup"><span data-stu-id="e092a-106">To do this, add an initial migration.</span></span>
<span data-ttu-id="e092a-107">Nainstalujte [EF Core Tools] [ 2] a spusťte příslušný příkaz.</span><span class="sxs-lookup"><span data-stu-id="e092a-107">Install the [EF Core Tools][2] and run the appropriate command.</span></span>

``` powershell
Add-Migration InitialCreate
```
``` Console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="e092a-108">Tři soubory jsou přidány do projektu v rámci **migrace** adresáře:</span><span class="sxs-lookup"><span data-stu-id="e092a-108">Three files are added to your project under the **Migrations** directory:</span></span>

* <span data-ttu-id="e092a-109">**00000000000000_InitialCreate.cs**– migrace hlavní soubor.</span><span class="sxs-lookup"><span data-stu-id="e092a-109">**00000000000000_InitialCreate.cs**--The main migrations file.</span></span> <span data-ttu-id="e092a-110">Obsahuje operace, která je nutné použít migraci (v `Up()`) a vrátit ji (v `Down()`).</span><span class="sxs-lookup"><span data-stu-id="e092a-110">Contains the operations necessary to apply the migration (in `Up()`) and to revert it (in `Down()`).</span></span>
* <span data-ttu-id="e092a-111">**00000000000000_InitialCreate.Designer.cs**– soubor metadat migrace.</span><span class="sxs-lookup"><span data-stu-id="e092a-111">**00000000000000_InitialCreate.Designer.cs**--The migrations metadata file.</span></span> <span data-ttu-id="e092a-112">Obsahuje informace, které EF používá.</span><span class="sxs-lookup"><span data-stu-id="e092a-112">Contains information used by EF.</span></span>
* <span data-ttu-id="e092a-113">**MyContextModelSnapshot.cs**– snímek aktuální model.</span><span class="sxs-lookup"><span data-stu-id="e092a-113">**MyContextModelSnapshot.cs**--A snapshot of your current model.</span></span> <span data-ttu-id="e092a-114">Umožňuje určit, co se změnilo, při přidávání další migraci.</span><span class="sxs-lookup"><span data-stu-id="e092a-114">Used to determine what changed when adding the next migration.</span></span>

<span data-ttu-id="e092a-115">Časové razítko v názvu souboru udržet chronologicky seřadit, abyste viděli průběh změny.</span><span class="sxs-lookup"><span data-stu-id="e092a-115">The timestamp in the filename helps keep them ordered chronologically so you can see the progression of changes.</span></span>

> [!TIP]
> <span data-ttu-id="e092a-116">Můžete libovolně přesouvat soubory migrace a měnit jejich oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="e092a-116">You are free to move Migrations files and change their namespace.</span></span> <span data-ttu-id="e092a-117">Nová migrace jsou vytvořeny jako poslední migrace objektů stejné úrovně.</span><span class="sxs-lookup"><span data-stu-id="e092a-117">New migrations are created as siblings of the last migration.</span></span>

<span data-ttu-id="e092a-118">V dalším kroku použití migrace k databázi a vytvoření schématu.</span><span class="sxs-lookup"><span data-stu-id="e092a-118">Next, apply the migration to the database to create the schema.</span></span>

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="adding-another-migration"></a><span data-ttu-id="e092a-119">Přidání další migraci</span><span class="sxs-lookup"><span data-stu-id="e092a-119">Adding another migration</span></span>
------------------------
<span data-ttu-id="e092a-120">Po provedení změn do modelu EF Core, bude synchronizován schéma databáze. Abyste je připojili k aktuálním stavu, přidejte další migraci.</span><span class="sxs-lookup"><span data-stu-id="e092a-120">After making changes to your EF Core model, the database schema will be out of sync. To bring it up to date, add another migration.</span></span> <span data-ttu-id="e092a-121">Název migrace můžete použít jako zprávu potvrzení v systému správy verzí.</span><span class="sxs-lookup"><span data-stu-id="e092a-121">The migration name can be used like a commit message in a version control system.</span></span> <span data-ttu-id="e092a-122">Například, pokud byly provedeny změny uložte zapracováním připomínek zákazníků produktů, můžu rozhodnout něco jako *AddProductReviews*.</span><span class="sxs-lookup"><span data-stu-id="e092a-122">For example, if I made changes to save customer reviews of products, I might choose something like *AddProductReviews*.</span></span>

``` powershell
Add-Migration AddProductReviews
```
``` Console
dotnet ef migrations add AddProductReviews
```

<span data-ttu-id="e092a-123">Po migraci je automaticky, zkontrolujte přesnost a přidejte jakékoli další operace nutné k použití správně.</span><span class="sxs-lookup"><span data-stu-id="e092a-123">Once the migration is scaffolded, you should review it for accuracy and add any additional operations required to apply it correctly.</span></span> <span data-ttu-id="e092a-124">Migrace může například obsahovat následující operace:</span><span class="sxs-lookup"><span data-stu-id="e092a-124">For example, your migration might contain the following operations:</span></span>

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

<span data-ttu-id="e092a-125">Přestože tyto operace provést schéma databáze kompatibilní, neuchovají se stávajícími názvy zákazníka.</span><span class="sxs-lookup"><span data-stu-id="e092a-125">While these operations make the database schema compatible, they don't preserve the existing customer names.</span></span> <span data-ttu-id="e092a-126">Chcete-li lépe, přepište ho následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="e092a-126">To make it better, rewrite it as follows.</span></span>

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
> <span data-ttu-id="e092a-127">Přidání nové migrace upozorní, když automaticky operace, která může způsobit ztrátu dat (např. vyřazení sloupce).</span><span class="sxs-lookup"><span data-stu-id="e092a-127">Adding a new migration warns when an operation is scaffolded that may result in data loss (like dropping a column).</span></span> <span data-ttu-id="e092a-128">Nezapomeňte si zejména Zkontrolujte tyto migrace přesnost.</span><span class="sxs-lookup"><span data-stu-id="e092a-128">Be sure to especially review these migrations for accuracy.</span></span>

<span data-ttu-id="e092a-129">Použití migrace k databázi pomocí příslušný příkaz.</span><span class="sxs-lookup"><span data-stu-id="e092a-129">Apply the migration to the database using the appropriate command.</span></span>

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="removing-a-migration"></a><span data-ttu-id="e092a-130">Odebírá se migrace</span><span class="sxs-lookup"><span data-stu-id="e092a-130">Removing a migration</span></span>
--------------------
<span data-ttu-id="e092a-131">Někdy přidejte migraci a Uvědomte si, že budete muset udělat dodatečné změny do modelu EF Core před každým jejím použitím.</span><span class="sxs-lookup"><span data-stu-id="e092a-131">Sometimes you add a migration and realize you need to make additional changes to your EF Core model before applying it.</span></span>
<span data-ttu-id="e092a-132">Pokud chcete odebrat posledního migrace, použijte tento příkaz.</span><span class="sxs-lookup"><span data-stu-id="e092a-132">To remove the last migration, use this command.</span></span>

``` powershell
Remove-Migration
```
``` Console
dotnet ef migrations remove
```

<span data-ttu-id="e092a-133">Po odebrání, můžete provést další změny a potom ho znovu přidejte.</span><span class="sxs-lookup"><span data-stu-id="e092a-133">After removing it, you can make the additional model changes and add it again.</span></span>

<a name="reverting-a-migration"></a><span data-ttu-id="e092a-134">Vrácení migrace zpět</span><span class="sxs-lookup"><span data-stu-id="e092a-134">Reverting a migration</span></span>
---------------------
<span data-ttu-id="e092a-135">Pokud jste již nainstalovali migrace (nebo několik migrací) do databáze, ale potřeba ji vzít zpět, slouží k použití migrace, ale zadejte název, který chcete vrátit zpět k migraci stejný příkaz.</span><span class="sxs-lookup"><span data-stu-id="e092a-135">If you already applied a migration (or several migrations) to the database but need to revert it, you can use the same command to apply migrations, but specify the name of the migration you want to roll back to.</span></span>

``` powershell
Update-Database LastGoodMigration
```
``` Console
dotnet ef database update LastGoodMigration
```

<a name="empty-migrations"></a><span data-ttu-id="e092a-136">Prázdný migrace</span><span class="sxs-lookup"><span data-stu-id="e092a-136">Empty migrations</span></span>
----------------
<span data-ttu-id="e092a-137">Někdy je užitečné, chcete-li přidat migrace bez jakýchkoli změn modelu.</span><span class="sxs-lookup"><span data-stu-id="e092a-137">Sometimes it's useful to add a migration without making any model changes.</span></span> <span data-ttu-id="e092a-138">Přidání nové migrace vytvoří v tomto případě některý z prázdné.</span><span class="sxs-lookup"><span data-stu-id="e092a-138">In this case, adding a new migration creates an empty one.</span></span> <span data-ttu-id="e092a-139">Můžete přizpůsobit, tato migrace provádět operace, které přímo nesouvisejí modelu EF Core.</span><span class="sxs-lookup"><span data-stu-id="e092a-139">You can customize this migration to perform operations that don't directly relate to the EF Core model.</span></span>
<span data-ttu-id="e092a-140">Některé věci, které můžete chtít spravovat tímto způsobem jsou:</span><span class="sxs-lookup"><span data-stu-id="e092a-140">Some things you might want to manage this way are:</span></span>

* <span data-ttu-id="e092a-141">Fulltextové vyhledávání</span><span class="sxs-lookup"><span data-stu-id="e092a-141">Full-Text Search</span></span>
* <span data-ttu-id="e092a-142">Funkce</span><span class="sxs-lookup"><span data-stu-id="e092a-142">Functions</span></span>
* <span data-ttu-id="e092a-143">Uložené procedury</span><span class="sxs-lookup"><span data-stu-id="e092a-143">Stored procedures</span></span>
* <span data-ttu-id="e092a-144">Aktivační procedury</span><span class="sxs-lookup"><span data-stu-id="e092a-144">Triggers</span></span>
* <span data-ttu-id="e092a-145">Zobrazení</span><span class="sxs-lookup"><span data-stu-id="e092a-145">Views</span></span>
* <span data-ttu-id="e092a-146">Atd.</span><span class="sxs-lookup"><span data-stu-id="e092a-146">etc.</span></span>

<a name="generating-a-sql-script"></a><span data-ttu-id="e092a-147">Generování skriptu SQL</span><span class="sxs-lookup"><span data-stu-id="e092a-147">Generating a SQL script</span></span>
-----------------------
<span data-ttu-id="e092a-148">Při ladění vaše migrace nebo jejich nasazení do produkční databáze, je užitečné se vygenerovat skript SQL.</span><span class="sxs-lookup"><span data-stu-id="e092a-148">When debugging your migrations or deploying them to a production database, it's useful to generate a SQL script.</span></span> <span data-ttu-id="e092a-149">Skript můžete být dále kontroluje přesnost a vyladění podle potřeb provozní databáze.</span><span class="sxs-lookup"><span data-stu-id="e092a-149">The script can then be further reviewed for accuracy and tuned to fit the needs of a production database.</span></span> <span data-ttu-id="e092a-150">Skript je také možné ve spojení s technologií nasazení.</span><span class="sxs-lookup"><span data-stu-id="e092a-150">The script can also be used in conjunction with a deployment technology.</span></span> <span data-ttu-id="e092a-151">Základní příkaz vypadá takto.</span><span class="sxs-lookup"><span data-stu-id="e092a-151">The basic command is as follows.</span></span>

``` powershell
Script-Migration
```
``` Console
dotnet ef migrations script
```

<span data-ttu-id="e092a-152">Existuje několik možností pro tento příkaz.</span><span class="sxs-lookup"><span data-stu-id="e092a-152">There are several options to this command.</span></span>

<span data-ttu-id="e092a-153">**z** migrace by měla být použita pro databázi před spuštěním skriptu poslední migrace.</span><span class="sxs-lookup"><span data-stu-id="e092a-153">The **from** migration should be the last migration applied to the database before running the script.</span></span> <span data-ttu-id="e092a-154">Pokud byly použity žádné migrace, zadejte `0` (Toto je výchozí hodnota).</span><span class="sxs-lookup"><span data-stu-id="e092a-154">If no migrations have been applied, specify `0` (this is the default).</span></span>

<span data-ttu-id="e092a-155">**k** migrace je poslední migrace, která bude použita pro databázi po spuštění skriptu.</span><span class="sxs-lookup"><span data-stu-id="e092a-155">The **to** migration is the last migration that will be applied to the database after running the script.</span></span> <span data-ttu-id="e092a-156">Výchozí hodnota poslední migrace ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="e092a-156">This defaults to the last migration in your project.</span></span>

<span data-ttu-id="e092a-157">**Idempotentní** skriptu můžete volitelně vygenerovat.</span><span class="sxs-lookup"><span data-stu-id="e092a-157">An **idempotent** script can optionally be generated.</span></span> <span data-ttu-id="e092a-158">Tento skript migrace platí pouze, pokud ještě nebyla použita k databázi.</span><span class="sxs-lookup"><span data-stu-id="e092a-158">This script only applies migrations if they haven't already been applied to the database.</span></span> <span data-ttu-id="e092a-159">To je užitečné, pokud zatím nevíte přesně poslední migrace použita pro databázi byla nebo pokud provádíte nasazení do více databází, které lze jednotlivě za různé migrace.</span><span class="sxs-lookup"><span data-stu-id="e092a-159">This is useful if you don't exactly know what the last migration applied to the database was or if you are deploying to multiple databases that may each be at a different migration.</span></span>

<a name="applying-migrations-at-runtime"></a><span data-ttu-id="e092a-160">Použití migrace za běhu</span><span class="sxs-lookup"><span data-stu-id="e092a-160">Applying migrations at runtime</span></span>
------------------------------
<span data-ttu-id="e092a-161">Některé aplikace může být vhodné pro použití migrace za běhu při spuštění nebo při prvním spuštění.</span><span class="sxs-lookup"><span data-stu-id="e092a-161">Some apps may want to apply migrations at runtime during startup or first run.</span></span> <span data-ttu-id="e092a-162">K tomu `Migrate()` metody.</span><span class="sxs-lookup"><span data-stu-id="e092a-162">Do this using the `Migrate()` method.</span></span>

<span data-ttu-id="e092a-163">Upozornění: Tento přístup se nehodí pro každého.</span><span class="sxs-lookup"><span data-stu-id="e092a-163">Caution, this approach isn't for everyone.</span></span> <span data-ttu-id="e092a-164">I když je skvělé pro aplikace s místní databází, většina aplikací bude vyžadovat více robustní strategii nasazení, jako je generování skriptů SQL.</span><span class="sxs-lookup"><span data-stu-id="e092a-164">While it's great for apps with a local database, most applications will require more robust deployment strategy like generating SQL scripts.</span></span>

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
> <span data-ttu-id="e092a-165">Nevolejte `EnsureCreated()` před `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="e092a-165">Don't call `EnsureCreated()` before `Migrate()`.</span></span> <span data-ttu-id="e092a-166">`EnsureCreated()` obchází migrace k vytvoření schématu, což způsobí, že `Migrate()` selhání.</span><span class="sxs-lookup"><span data-stu-id="e092a-166">`EnsureCreated()` bypasses Migrations to create the schema, which causes `Migrate()` to fail.</span></span>

> [!NOTE]
> <span data-ttu-id="e092a-167">Tato metoda vytvoří nahoře `IMigrator` služby, které lze použít pro pokročilejší scénáře.</span><span class="sxs-lookup"><span data-stu-id="e092a-167">This method builds on top of the `IMigrator` service, which can be used for more advanced scenarios.</span></span> <span data-ttu-id="e092a-168">Použití `DbContext.GetService<IMigrator>()` k němu přistupovat.</span><span class="sxs-lookup"><span data-stu-id="e092a-168">Use `DbContext.GetService<IMigrator>()` to access it.</span></span>


  [1]: ../../modeling/index.md
  [2]: ../../miscellaneous/cli/index.md
