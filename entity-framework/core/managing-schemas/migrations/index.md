---
title: Migrace – EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/05/2018
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: 5ae06a4342a556936dc44c5bf6622814eaad4733
ms.sourcegitcommit: 7a7da65404c9338e1e3df42576a13be536a6f95f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2018
ms.locfileid: "48834744"
---
<a name="migrations"></a><span data-ttu-id="d8092-102">Migrace</span><span class="sxs-lookup"><span data-stu-id="d8092-102">Migrations</span></span>
==========

<span data-ttu-id="d8092-103">Datový model se změní při vývoji a získá synchronizován s databází.</span><span class="sxs-lookup"><span data-stu-id="d8092-103">A data model changes during development and gets out of sync with the database.</span></span> <span data-ttu-id="d8092-104">Můžete vyřaďte databázi a umožní vytvořit nový, který odpovídá modelu EF, ale tento postup má za následek ztrátu dat.</span><span class="sxs-lookup"><span data-stu-id="d8092-104">You can drop the database and let EF create a new one that matches the model, but this procedure results in the loss of data.</span></span> <span data-ttu-id="d8092-105">Funkce migrace v EF Core poskytuje způsob, jak přírůstkové aktualizace schématu databáze, aby udržovat synchronizované s datovým modelem aplikace při zachování stávajících dat v databázi.</span><span class="sxs-lookup"><span data-stu-id="d8092-105">The migrations feature in EF Core provides a way to incrementally update the database schema to keep it in sync with the application's data model while preserving existing data in the database.</span></span>

<span data-ttu-id="d8092-106">Migrace zahrnuje nástroje pro příkazový řádek a rozhraní API, která vám pomůžou s následující úkoly:</span><span class="sxs-lookup"><span data-stu-id="d8092-106">Migrations includes command-line tools and APIs that help with the following tasks:</span></span>

* <span data-ttu-id="d8092-107">[Vytvoření migrace](#create-a-migration).</span><span class="sxs-lookup"><span data-stu-id="d8092-107">[Create a migration](#create-a-migration).</span></span> <span data-ttu-id="d8092-108">Generovat kód, který může aktualizovat databázi pro synchronizaci se sadou změn modelu.</span><span class="sxs-lookup"><span data-stu-id="d8092-108">Generate code that can update the database to sync it with a set of model changes.</span></span>
* <span data-ttu-id="d8092-109">[Aktualizovat databázi](#update-the-database).</span><span class="sxs-lookup"><span data-stu-id="d8092-109">[Update the database](#update-the-database).</span></span> <span data-ttu-id="d8092-110">Použijte čekající migrace aktualizovat schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="d8092-110">Apply pending migrations to update the database schema.</span></span>
* <span data-ttu-id="d8092-111">[Migrace code přizpůsobit](#customize-migration-code).</span><span class="sxs-lookup"><span data-stu-id="d8092-111">[Customize migration code](#customize-migration-code).</span></span> <span data-ttu-id="d8092-112">Někdy musí upravit, nebo doplnit generovaného kódu.</span><span class="sxs-lookup"><span data-stu-id="d8092-112">Sometimes the generated code needs to be modified or supplemented.</span></span>
* <span data-ttu-id="d8092-113">[Odebrat migrace](#remove-a-migration).</span><span class="sxs-lookup"><span data-stu-id="d8092-113">[Remove a migration](#remove-a-migration).</span></span> <span data-ttu-id="d8092-114">Odstranění vygenerovaného kódu.</span><span class="sxs-lookup"><span data-stu-id="d8092-114">Delete the generated code.</span></span>
* <span data-ttu-id="d8092-115">[Vrácení migrace](#revert-a-migration).</span><span class="sxs-lookup"><span data-stu-id="d8092-115">[Revert a migration](#revert-a-migration).</span></span> <span data-ttu-id="d8092-116">Vrátit zpět změny databáze.</span><span class="sxs-lookup"><span data-stu-id="d8092-116">Undo the database changes.</span></span>
* <span data-ttu-id="d8092-117">[Generovat SQL skripty](#generate-sql-scripts).</span><span class="sxs-lookup"><span data-stu-id="d8092-117">[Generate SQL scripts](#generate-sql-scripts).</span></span> <span data-ttu-id="d8092-118">Může být nutné skript aktualizovat produkční databáze nebo řešení potíží s migrací kódu.</span><span class="sxs-lookup"><span data-stu-id="d8092-118">You might need a script to update a production database or to troubleshoot migration code.</span></span>
* <span data-ttu-id="d8092-119">[Použití migrace za běhu](#apply-migrations-at-runtime).</span><span class="sxs-lookup"><span data-stu-id="d8092-119">[Apply migrations at runtime](#apply-migrations-at-runtime).</span></span> <span data-ttu-id="d8092-120">Když aktualizace návrhu a spouštění skriptů nejsou nejlepší možnosti, zavolejte `Migrate()` metody.</span><span class="sxs-lookup"><span data-stu-id="d8092-120">When design-time updates and running scripts aren't the best options, call the `Migrate()` method.</span></span>

<a name="install-the-tools"></a><span data-ttu-id="d8092-121">Instalace nástrojů</span><span class="sxs-lookup"><span data-stu-id="d8092-121">Install the tools</span></span>
-----------------

<span data-ttu-id="d8092-122">Nainstalujte [nástroje příkazového řádku](xref:core/miscellaneous/cli/index):</span><span class="sxs-lookup"><span data-stu-id="d8092-122">Install the [command-line tools](xref:core/miscellaneous/cli/index):</span></span>
* <span data-ttu-id="d8092-123">Pro Visual Studio, doporučujeme, abyste [Konzola správce balíčků nástroje](xref:core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="d8092-123">For Visual Studio, we recommend the [Package Manager Console tools](xref:core/miscellaneous/cli/powershell).</span></span>
* <span data-ttu-id="d8092-124">U jiných vývojových prostředích, zvolte [nástroje rozhraní příkazového řádku .NET Core](xref:core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="d8092-124">For other development environments, choose the [.NET Core CLI tools](xref:core/miscellaneous/cli/dotnet).</span></span>

<a name="create-a-migration"></a><span data-ttu-id="d8092-125">Vytvoření migrace</span><span class="sxs-lookup"><span data-stu-id="d8092-125">Create a migration</span></span>
------------------

<span data-ttu-id="d8092-126">Po [definované počáteční modelu](xref:core/modeling/index), je čas vytvořit databázi.</span><span class="sxs-lookup"><span data-stu-id="d8092-126">After you've [defined your initial model](xref:core/modeling/index), it's time to create the database.</span></span> <span data-ttu-id="d8092-127">Chcete-li přidat počáteční migraci spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="d8092-127">To add an initial migration, run the following command.</span></span>

``` powershell
Add-Migration InitialCreate
```
``` Console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="d8092-128">Tři soubory jsou přidány do projektu v rámci **migrace** adresáře:</span><span class="sxs-lookup"><span data-stu-id="d8092-128">Three files are added to your project under the **Migrations** directory:</span></span>

* <span data-ttu-id="d8092-129">**00000000000000_InitialCreate.cs**– migrace hlavní soubor.</span><span class="sxs-lookup"><span data-stu-id="d8092-129">**00000000000000_InitialCreate.cs**--The main migrations file.</span></span> <span data-ttu-id="d8092-130">Obsahuje operace, která je nutné použít migraci (v `Up()`) a vrátit ji (v `Down()`).</span><span class="sxs-lookup"><span data-stu-id="d8092-130">Contains the operations necessary to apply the migration (in `Up()`) and to revert it (in `Down()`).</span></span>
* <span data-ttu-id="d8092-131">**00000000000000_InitialCreate.Designer.cs**– soubor metadat migrace.</span><span class="sxs-lookup"><span data-stu-id="d8092-131">**00000000000000_InitialCreate.Designer.cs**--The migrations metadata file.</span></span> <span data-ttu-id="d8092-132">Obsahuje informace, které EF používá.</span><span class="sxs-lookup"><span data-stu-id="d8092-132">Contains information used by EF.</span></span>
* <span data-ttu-id="d8092-133">**MyContextModelSnapshot.cs**– snímek aktuální model.</span><span class="sxs-lookup"><span data-stu-id="d8092-133">**MyContextModelSnapshot.cs**--A snapshot of your current model.</span></span> <span data-ttu-id="d8092-134">Umožňuje určit, co se změnilo, při přidávání další migraci.</span><span class="sxs-lookup"><span data-stu-id="d8092-134">Used to determine what changed when adding the next migration.</span></span>

<span data-ttu-id="d8092-135">Časové razítko v názvu souboru udržet chronologicky seřadit, abyste viděli průběh změny.</span><span class="sxs-lookup"><span data-stu-id="d8092-135">The timestamp in the filename helps keep them ordered chronologically so you can see the progression of changes.</span></span>

> [!TIP]
> <span data-ttu-id="d8092-136">Můžete libovolně přesouvat soubory migrace a měnit jejich oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="d8092-136">You are free to move Migrations files and change their namespace.</span></span> <span data-ttu-id="d8092-137">Nová migrace jsou vytvořeny jako poslední migrace objektů stejné úrovně.</span><span class="sxs-lookup"><span data-stu-id="d8092-137">New migrations are created as siblings of the last migration.</span></span>

<a name="update-the-database"></a><span data-ttu-id="d8092-138">Aktualizace databáze</span><span class="sxs-lookup"><span data-stu-id="d8092-138">Update the database</span></span>
-------------------

<span data-ttu-id="d8092-139">V dalším kroku použití migrace k databázi a vytvoření schématu.</span><span class="sxs-lookup"><span data-stu-id="d8092-139">Next, apply the migration to the database to create the schema.</span></span>

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="customize-migration-code"></a><span data-ttu-id="d8092-140">Přizpůsobení migrace kódu</span><span class="sxs-lookup"><span data-stu-id="d8092-140">Customize migration code</span></span>
------------------------

<span data-ttu-id="d8092-141">Po provedení změn do modelu EF Core, může být schématu databáze synchronizované. Abyste je připojili k aktuálním stavu, přidejte další migraci.</span><span class="sxs-lookup"><span data-stu-id="d8092-141">After making changes to your EF Core model, the database schema might be out of sync. To bring it up to date, add another migration.</span></span> <span data-ttu-id="d8092-142">Název migrace můžete použít jako zprávu potvrzení v systému správy verzí.</span><span class="sxs-lookup"><span data-stu-id="d8092-142">The migration name can be used like a commit message in a version control system.</span></span> <span data-ttu-id="d8092-143">Například můžete zvolit název, například *AddProductReviews* Pokud změna je novou třídu entity u kontroly.</span><span class="sxs-lookup"><span data-stu-id="d8092-143">For example, you might choose a name like *AddProductReviews* if the change is a new entity class for reviews.</span></span>

``` powershell
Add-Migration AddProductReviews
```
``` Console
dotnet ef migrations add AddProductReviews
```

<span data-ttu-id="d8092-144">Po migraci je vygenerovaná (kód generovaný pro něj), projděte si kód pro přesnost a přidat, odebrat nebo změnit všechny operace nutné k použití správně.</span><span class="sxs-lookup"><span data-stu-id="d8092-144">Once the migration is scaffolded (code generated for it), review the code for accuracy and add, remove or modify any operations required to apply it correctly.</span></span>

<span data-ttu-id="d8092-145">Migrace může například obsahovat následující operace:</span><span class="sxs-lookup"><span data-stu-id="d8092-145">For example, a migration might contain the following operations:</span></span>

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

<span data-ttu-id="d8092-146">Přestože tyto operace provést schéma databáze kompatibilní, neuchovají se stávajícími názvy zákazníka.</span><span class="sxs-lookup"><span data-stu-id="d8092-146">While these operations make the database schema compatible, they don't preserve the existing customer names.</span></span> <span data-ttu-id="d8092-147">Chcete-li lépe, přepište ho následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="d8092-147">To make it better, rewrite it as follows.</span></span>

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
> <span data-ttu-id="d8092-148">Proces migrace generování uživatelského rozhraní upozorní, když operace může způsobit ztrátu dat (např. vyřazení sloupce).</span><span class="sxs-lookup"><span data-stu-id="d8092-148">The migration scaffolding process warns when an operation might result in data loss (like dropping a column).</span></span> <span data-ttu-id="d8092-149">Pokud zjistíte, že upozornění, zejména si přečtěte téma migrace kódu přesnost.</span><span class="sxs-lookup"><span data-stu-id="d8092-149">If you see that warning, be especially sure to review the migrations code for accuracy.</span></span>

<span data-ttu-id="d8092-150">Použití migrace k databázi pomocí příslušný příkaz.</span><span class="sxs-lookup"><span data-stu-id="d8092-150">Apply the migration to the database using the appropriate command.</span></span>

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

### <a name="empty-migrations"></a><span data-ttu-id="d8092-151">Prázdný migrace</span><span class="sxs-lookup"><span data-stu-id="d8092-151">Empty migrations</span></span>

<span data-ttu-id="d8092-152">Někdy je užitečné, chcete-li přidat migrace bez jakýchkoli změn modelu.</span><span class="sxs-lookup"><span data-stu-id="d8092-152">Sometimes it's useful to add a migration without making any model changes.</span></span> <span data-ttu-id="d8092-153">V tomto případě přidáte novou migraci vytvoří soubory s kódem s prázdné třídy.</span><span class="sxs-lookup"><span data-stu-id="d8092-153">In this case, adding a new migration creates code files with empty classes.</span></span> <span data-ttu-id="d8092-154">Můžete přizpůsobit, tato migrace provádět operace, které přímo nesouvisejí modelu EF Core.</span><span class="sxs-lookup"><span data-stu-id="d8092-154">You can customize this migration to perform operations that don't directly relate to the EF Core model.</span></span> <span data-ttu-id="d8092-155">Některé věci, které můžete chtít spravovat tímto způsobem jsou:</span><span class="sxs-lookup"><span data-stu-id="d8092-155">Some things you might want to manage this way are:</span></span>

* <span data-ttu-id="d8092-156">Fulltextové vyhledávání</span><span class="sxs-lookup"><span data-stu-id="d8092-156">Full-Text Search</span></span>
* <span data-ttu-id="d8092-157">Funkce</span><span class="sxs-lookup"><span data-stu-id="d8092-157">Functions</span></span>
* <span data-ttu-id="d8092-158">Uložené procedury</span><span class="sxs-lookup"><span data-stu-id="d8092-158">Stored procedures</span></span>
* <span data-ttu-id="d8092-159">Aktivační procedury</span><span class="sxs-lookup"><span data-stu-id="d8092-159">Triggers</span></span>
* <span data-ttu-id="d8092-160">Zobrazení</span><span class="sxs-lookup"><span data-stu-id="d8092-160">Views</span></span>

<a name="remove-a-migration"></a><span data-ttu-id="d8092-161">Odstranit migraci</span><span class="sxs-lookup"><span data-stu-id="d8092-161">Remove a migration</span></span>
------------------
<span data-ttu-id="d8092-162">Někdy přidejte migraci a Uvědomte si, že budete muset udělat dodatečné změny do modelu EF Core před každým jejím použitím.</span><span class="sxs-lookup"><span data-stu-id="d8092-162">Sometimes you add a migration and realize you need to make additional changes to your EF Core model before applying it.</span></span> <span data-ttu-id="d8092-163">Pokud chcete odebrat posledního migrace, použijte tento příkaz.</span><span class="sxs-lookup"><span data-stu-id="d8092-163">To remove the last migration, use this command.</span></span>

``` powershell
Remove-Migration
```
``` Console
dotnet ef migrations remove
```

<span data-ttu-id="d8092-164">Po odebrání migrace, můžete provést další změny a potom ho znovu přidejte.</span><span class="sxs-lookup"><span data-stu-id="d8092-164">After removing the migration, you can make the additional model changes and add it again.</span></span>

<a name="revert-a-migration"></a><span data-ttu-id="d8092-165">Vrácení migrace</span><span class="sxs-lookup"><span data-stu-id="d8092-165">Revert a migration</span></span>
------------------
<span data-ttu-id="d8092-166">Pokud jste již nainstalovali migrace (nebo několik migrací) do databáze, ale potřeba ji vzít zpět, slouží k použití migrace, ale zadejte název, který chcete vrátit zpět k migraci stejný příkaz.</span><span class="sxs-lookup"><span data-stu-id="d8092-166">If you already applied a migration (or several migrations) to the database but need to revert it, you can use the same command to apply migrations, but specify the name of the migration you want to roll back to.</span></span>

``` powershell
Update-Database LastGoodMigration
```
``` Console
dotnet ef database update LastGoodMigration
```

<a name="generate-sql-scripts"></a><span data-ttu-id="d8092-167">Generování skriptů SQL</span><span class="sxs-lookup"><span data-stu-id="d8092-167">Generate SQL scripts</span></span>
--------------------
<span data-ttu-id="d8092-168">Při ladění vaše migrace nebo jejich nasazení do produkční databáze, je užitečné se vygenerovat skript SQL.</span><span class="sxs-lookup"><span data-stu-id="d8092-168">When debugging your migrations or deploying them to a production database, it's useful to generate a SQL script.</span></span> <span data-ttu-id="d8092-169">Skript můžete být dále kontroluje přesnost a vyladění podle potřeb provozní databáze.</span><span class="sxs-lookup"><span data-stu-id="d8092-169">The script can then be further reviewed for accuracy and tuned to fit the needs of a production database.</span></span> <span data-ttu-id="d8092-170">Skript je také možné ve spojení s technologií nasazení.</span><span class="sxs-lookup"><span data-stu-id="d8092-170">The script can also be used in conjunction with a deployment technology.</span></span> <span data-ttu-id="d8092-171">Základní příkaz vypadá takto.</span><span class="sxs-lookup"><span data-stu-id="d8092-171">The basic command is as follows.</span></span>

``` powershell
Script-Migration
```
``` Console
dotnet ef migrations script
```

<span data-ttu-id="d8092-172">Existuje několik možností pro tento příkaz.</span><span class="sxs-lookup"><span data-stu-id="d8092-172">There are several options to this command.</span></span>

<span data-ttu-id="d8092-173">**z** migrace by měla být použita pro databázi před spuštěním skriptu poslední migrace.</span><span class="sxs-lookup"><span data-stu-id="d8092-173">The **from** migration should be the last migration applied to the database before running the script.</span></span> <span data-ttu-id="d8092-174">Pokud byly použity žádné migrace, zadejte `0` (Toto je výchozí hodnota).</span><span class="sxs-lookup"><span data-stu-id="d8092-174">If no migrations have been applied, specify `0` (this is the default).</span></span>

<span data-ttu-id="d8092-175">**k** migrace je poslední migrace, která bude použita pro databázi po spuštění skriptu.</span><span class="sxs-lookup"><span data-stu-id="d8092-175">The **to** migration is the last migration that will be applied to the database after running the script.</span></span> <span data-ttu-id="d8092-176">Výchozí hodnota poslední migrace ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="d8092-176">This defaults to the last migration in your project.</span></span>

<span data-ttu-id="d8092-177">**Idempotentní** skriptu můžete volitelně vygenerovat.</span><span class="sxs-lookup"><span data-stu-id="d8092-177">An **idempotent** script can optionally be generated.</span></span> <span data-ttu-id="d8092-178">Tento skript migrace platí pouze, pokud ještě nebyla použita k databázi.</span><span class="sxs-lookup"><span data-stu-id="d8092-178">This script only applies migrations if they haven't already been applied to the database.</span></span> <span data-ttu-id="d8092-179">To je užitečné, pokud zatím nevíte přesně poslední migrace použita pro databázi byla nebo pokud provádíte nasazení do více databází, které lze jednotlivě za různé migrace.</span><span class="sxs-lookup"><span data-stu-id="d8092-179">This is useful if you don't exactly know what the last migration applied to the database was or if you are deploying to multiple databases that may each be at a different migration.</span></span>

<a name="apply-migrations-at-runtime"></a><span data-ttu-id="d8092-180">Použití migrace za běhu</span><span class="sxs-lookup"><span data-stu-id="d8092-180">Apply migrations at runtime</span></span>
---------------------------
<span data-ttu-id="d8092-181">Některé aplikace může být vhodné pro použití migrace za běhu při spuštění nebo při prvním spuštění.</span><span class="sxs-lookup"><span data-stu-id="d8092-181">Some apps may want to apply migrations at runtime during startup or first run.</span></span> <span data-ttu-id="d8092-182">K tomu `Migrate()` metody.</span><span class="sxs-lookup"><span data-stu-id="d8092-182">Do this using the `Migrate()` method.</span></span>

<span data-ttu-id="d8092-183">Tato metoda vytvoří nahoře `IMigrator` služby, které lze použít pro pokročilejší scénáře.</span><span class="sxs-lookup"><span data-stu-id="d8092-183">This method builds on top of the `IMigrator` service, which can be used for more advanced scenarios.</span></span> <span data-ttu-id="d8092-184">Použití `DbContext.GetService<IMigrator>()` k němu přistupovat.</span><span class="sxs-lookup"><span data-stu-id="d8092-184">Use `DbContext.GetService<IMigrator>()` to access it.</span></span>

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
> * <span data-ttu-id="d8092-185">Tento přístup se nehodí pro každého.</span><span class="sxs-lookup"><span data-stu-id="d8092-185">This approach isn't for everyone.</span></span> <span data-ttu-id="d8092-186">I když je skvělé pro aplikace s místní databází, většina aplikací bude vyžadovat více robustní strategii nasazení, jako je generování skriptů SQL.</span><span class="sxs-lookup"><span data-stu-id="d8092-186">While it's great for apps with a local database, most applications will require more robust deployment strategy like generating SQL scripts.</span></span>
> * <span data-ttu-id="d8092-187">Nevolejte `EnsureCreated()` před `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="d8092-187">Don't call `EnsureCreated()` before `Migrate()`.</span></span> <span data-ttu-id="d8092-188">`EnsureCreated()` obchází migrace k vytvoření schématu, což způsobí, že `Migrate()` selhání.</span><span class="sxs-lookup"><span data-stu-id="d8092-188">`EnsureCreated()` bypasses Migrations to create the schema, which causes `Migrate()` to fail.</span></span>

<a name="next-steps"></a><span data-ttu-id="d8092-189">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d8092-189">Next steps</span></span>
----------

<span data-ttu-id="d8092-190">Další informace naleznete v tématu <xref:core/miscellaneous/cli/index>.</span><span class="sxs-lookup"><span data-stu-id="d8092-190">For more information, see <xref:core/miscellaneous/cli/index>.</span></span>