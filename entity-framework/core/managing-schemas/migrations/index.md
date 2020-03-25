---
title: Migrace – EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/05/2018
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: 190057daed61c58c1f89ee8d775913458e413a50
ms.sourcegitcommit: c3b8386071d64953ee68788ef9d951144881a6ab
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/24/2020
ms.locfileid: "80136201"
---
# <a name="migrations"></a><span data-ttu-id="9c9eb-102">Migrace</span><span class="sxs-lookup"><span data-stu-id="9c9eb-102">Migrations</span></span>

<span data-ttu-id="9c9eb-103">Datový model se během vývoje mění a nesynchronizuje se s databází.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-103">A data model changes during development and gets out of sync with the database.</span></span> <span data-ttu-id="9c9eb-104">Databázi můžete vyřadit a nechat EF vytvořit nové, které odpovídají modelu, ale tento postup vede ke ztrátě dat.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-104">You can drop the database and let EF create a new one that matches the model, but this procedure results in the loss of data.</span></span> <span data-ttu-id="9c9eb-105">Funkce migrace v EF Core poskytuje způsob, jak přírůstkově aktualizovat schéma databáze, aby se zachovala synchronizace s datovým modelem aplikace a současně zachovává stávající data v databázi.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-105">The migrations feature in EF Core provides a way to incrementally update the database schema to keep it in sync with the application's data model while preserving existing data in the database.</span></span>

<span data-ttu-id="9c9eb-106">Migrace zahrnuje nástroje příkazového řádku a rozhraní API, které vám pomůžou s následujícími úlohami:</span><span class="sxs-lookup"><span data-stu-id="9c9eb-106">Migrations includes command-line tools and APIs that help with the following tasks:</span></span>

* <span data-ttu-id="9c9eb-107">[Vytvořte migraci](#create-a-migration).</span><span class="sxs-lookup"><span data-stu-id="9c9eb-107">[Create a migration](#create-a-migration).</span></span> <span data-ttu-id="9c9eb-108">Vygeneruje kód, který může aktualizovat databázi pro synchronizaci se sadou změn modelu.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-108">Generate code that can update the database to sync it with a set of model changes.</span></span>
* <span data-ttu-id="9c9eb-109">[Aktualizujte databázi](#update-the-database).</span><span class="sxs-lookup"><span data-stu-id="9c9eb-109">[Update the database](#update-the-database).</span></span> <span data-ttu-id="9c9eb-110">Použijte nedokončené migrace k aktualizaci schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-110">Apply pending migrations to update the database schema.</span></span>
* <span data-ttu-id="9c9eb-111">[Přizpůsobení kódu migrace](#customize-migration-code).</span><span class="sxs-lookup"><span data-stu-id="9c9eb-111">[Customize migration code](#customize-migration-code).</span></span> <span data-ttu-id="9c9eb-112">Někdy je nutné vygenerovaný kód upravit nebo doplnit.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-112">Sometimes the generated code needs to be modified or supplemented.</span></span>
* <span data-ttu-id="9c9eb-113">[Odeberte migraci](#remove-a-migration).</span><span class="sxs-lookup"><span data-stu-id="9c9eb-113">[Remove a migration](#remove-a-migration).</span></span> <span data-ttu-id="9c9eb-114">Odstraňte generovaný kód.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-114">Delete the generated code.</span></span>
* <span data-ttu-id="9c9eb-115">[Vrácení migrace zpět](#revert-a-migration)</span><span class="sxs-lookup"><span data-stu-id="9c9eb-115">[Revert a migration](#revert-a-migration).</span></span> <span data-ttu-id="9c9eb-116">Vrátí zpět změny databáze.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-116">Undo the database changes.</span></span>
* <span data-ttu-id="9c9eb-117">[Vygenerujte skripty SQL](#generate-sql-scripts).</span><span class="sxs-lookup"><span data-stu-id="9c9eb-117">[Generate SQL scripts](#generate-sql-scripts).</span></span> <span data-ttu-id="9c9eb-118">Je možné, že budete potřebovat skript pro aktualizaci provozní databáze nebo pro řešení potíží s kódem migrace.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-118">You might need a script to update a production database or to troubleshoot migration code.</span></span>
* <span data-ttu-id="9c9eb-119">[Použijte migrace za běhu](#apply-migrations-at-runtime).</span><span class="sxs-lookup"><span data-stu-id="9c9eb-119">[Apply migrations at runtime](#apply-migrations-at-runtime).</span></span> <span data-ttu-id="9c9eb-120">Pokud nejsou dostupné aktualizace a spouštění skriptů v době návrhu, zavolejte metodu `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-120">When design-time updates and running scripts aren't the best options, call the `Migrate()` method.</span></span>

> [!TIP]
> <span data-ttu-id="9c9eb-121">Pokud je `DbContext` v jiném sestavení než projekt po spuštění, můžete explicitně zadat cílové a spouštěné projekty buď v [nástrojích konzoly Správce balíčků](xref:core/miscellaneous/cli/powershell#target-and-startup-project) , nebo v [.NET Core CLIch nástrojích](xref:core/miscellaneous/cli/dotnet#target-project-and-startup-project).</span><span class="sxs-lookup"><span data-stu-id="9c9eb-121">If the `DbContext` is in a different assembly than the startup project, you can explicitly specify the target and startup projects in either the [Package Manager Console tools](xref:core/miscellaneous/cli/powershell#target-and-startup-project) or the [.NET Core CLI tools](xref:core/miscellaneous/cli/dotnet#target-project-and-startup-project).</span></span>

## <a name="install-the-tools"></a><span data-ttu-id="9c9eb-122">Instalace nástrojů</span><span class="sxs-lookup"><span data-stu-id="9c9eb-122">Install the tools</span></span>

<span data-ttu-id="9c9eb-123">Instalace [nástrojů příkazového řádku](xref:core/miscellaneous/cli/index):</span><span class="sxs-lookup"><span data-stu-id="9c9eb-123">Install the [command-line tools](xref:core/miscellaneous/cli/index):</span></span>

* <span data-ttu-id="9c9eb-124">Pro Visual Studio doporučujeme [Nástroje konzoly Správce balíčků](xref:core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="9c9eb-124">For Visual Studio, we recommend the [Package Manager Console tools](xref:core/miscellaneous/cli/powershell).</span></span>
* <span data-ttu-id="9c9eb-125">Pro jiná vývojová prostředí vyberte [nástroje .NET Core CLI](xref:core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="9c9eb-125">For other development environments, choose the [.NET Core CLI tools](xref:core/miscellaneous/cli/dotnet).</span></span>

## <a name="create-a-migration"></a><span data-ttu-id="9c9eb-126">Vytvoření migrace</span><span class="sxs-lookup"><span data-stu-id="9c9eb-126">Create a migration</span></span>

<span data-ttu-id="9c9eb-127">Po [Definování počátečního modelu](xref:core/modeling/index)je čas vytvořit databázi.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-127">After you've [defined your initial model](xref:core/modeling/index), it's time to create the database.</span></span> <span data-ttu-id="9c9eb-128">Chcete-li přidat počáteční migraci, spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-128">To add an initial migration, run the following command.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="9c9eb-129">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="9c9eb-129">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add InitialCreate
```

### <a name="visual-studio"></a>[<span data-ttu-id="9c9eb-130">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9c9eb-130">Visual Studio</span></span>](#tab/vs)

``` powershell
Add-Migration InitialCreate
```

***

<span data-ttu-id="9c9eb-131">Do projektu se v adresáři **migrace** přidají tři soubory:</span><span class="sxs-lookup"><span data-stu-id="9c9eb-131">Three files are added to your project under the **Migrations** directory:</span></span>

* <span data-ttu-id="9c9eb-132">**XXXXXXXXXXXXXX_InitialCreate. cs**– hlavní soubor migrace.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-132">**XXXXXXXXXXXXXX_InitialCreate.cs**--The main migrations file.</span></span> <span data-ttu-id="9c9eb-133">Obsahuje operace nutné k použití migrace (v `Up()`) a k jejímu vrácení (v `Down()`).</span><span class="sxs-lookup"><span data-stu-id="9c9eb-133">Contains the operations necessary to apply the migration (in `Up()`) and to revert it (in `Down()`).</span></span>
* <span data-ttu-id="9c9eb-134">**XXXXXXXXXXXXXX_InitialCreate. Designer. cs**– soubor metadat migrace.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-134">**XXXXXXXXXXXXXX_InitialCreate.Designer.cs**--The migrations metadata file.</span></span> <span data-ttu-id="9c9eb-135">Obsahuje informace, které používá EF.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-135">Contains information used by EF.</span></span>
* <span data-ttu-id="9c9eb-136">**MyContextModelSnapshot.cs**– snímek aktuálního modelu.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-136">**MyContextModelSnapshot.cs**--A snapshot of your current model.</span></span> <span data-ttu-id="9c9eb-137">Slouží k určení toho, co se změnilo při přidávání další migrace.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-137">Used to determine what changed when adding the next migration.</span></span>

<span data-ttu-id="9c9eb-138">Časové razítko v názvu souboru pomáhá udržet seřazené chronologicky, takže se můžete podívat na průběh změn.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-138">The timestamp in the filename helps keep them ordered chronologically so you can see the progression of changes.</span></span>

> [!TIP]
> <span data-ttu-id="9c9eb-139">Můžete přesunout soubory migrace a změnit jejich obor názvů.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-139">You are free to move Migrations files and change their namespace.</span></span> <span data-ttu-id="9c9eb-140">Nové migrace se vytvoří jako na stejné úrovni jako poslední migrace.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-140">New migrations are created as siblings of the last migration.</span></span>

## <a name="update-the-database"></a><span data-ttu-id="9c9eb-141">Aktualizace databáze</span><span class="sxs-lookup"><span data-stu-id="9c9eb-141">Update the database</span></span>

<span data-ttu-id="9c9eb-142">V dalším kroku použijte migraci na databázi a vytvořte schéma.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-142">Next, apply the migration to the database to create the schema.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="9c9eb-143">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="9c9eb-143">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef database update
```

### <a name="visual-studio"></a>[<span data-ttu-id="9c9eb-144">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9c9eb-144">Visual Studio</span></span>](#tab/vs)

``` powershell
Update-Database
```

***

## <a name="customize-migration-code"></a><span data-ttu-id="9c9eb-145">Přizpůsobení kódu migrace</span><span class="sxs-lookup"><span data-stu-id="9c9eb-145">Customize migration code</span></span>

<span data-ttu-id="9c9eb-146">Po provedení změn v modelu EF Core nemusí být schéma databáze synchronizované. Pokud ho chcete uvést v aktuálním stavu, přidejte další migraci.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-146">After making changes to your EF Core model, the database schema might be out of sync. To bring it up to date, add another migration.</span></span> <span data-ttu-id="9c9eb-147">Název migrace lze použít jako zprávu potvrzení v systému správy verzí.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-147">The migration name can be used like a commit message in a version control system.</span></span> <span data-ttu-id="9c9eb-148">Například můžete zvolit název jako *AddProductReviews* , pokud je změna novou třídou entity pro recenze.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-148">For example, you might choose a name like *AddProductReviews* if the change is a new entity class for reviews.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="9c9eb-149">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="9c9eb-149">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add AddProductReviews
```

### <a name="visual-studio"></a>[<span data-ttu-id="9c9eb-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9c9eb-150">Visual Studio</span></span>](#tab/vs)

``` powershell
Add-Migration AddProductReviews
```

***

<span data-ttu-id="9c9eb-151">Jakmile se migrace vygeneruje z vygenerovaného kódu (kód vygenerovaný pro něj), zkontrolujte přesnost kódu a přidejte, odeberte nebo upravte všechny operace, které jsou nutné k jejich použití.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-151">Once the migration is scaffolded (code generated for it), review the code for accuracy and add, remove or modify any operations required to apply it correctly.</span></span>

<span data-ttu-id="9c9eb-152">Například migrace může obsahovat následující operace:</span><span class="sxs-lookup"><span data-stu-id="9c9eb-152">For example, a migration might contain the following operations:</span></span>

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

<span data-ttu-id="9c9eb-153">I když tyto operace zajistí kompatibilitu schématu databáze, nezachovají se stávající názvy zákazníků.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-153">While these operations make the database schema compatible, they don't preserve the existing customer names.</span></span> <span data-ttu-id="9c9eb-154">Chcete-li to lepší, přepište ho následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-154">To make it better, rewrite it as follows.</span></span>

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
> <span data-ttu-id="9c9eb-155">Proces generování uživatelského rozhraní se upozorní na to, že výsledkem operace může být ztráta dat (například vyřazení sloupce).</span><span class="sxs-lookup"><span data-stu-id="9c9eb-155">The migration scaffolding process warns when an operation might result in data loss (like dropping a column).</span></span> <span data-ttu-id="9c9eb-156">Pokud se zobrazí toto upozornění, nezapomeňte si projít kód migrace, aby se zajistila přesnost.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-156">If you see that warning, be especially sure to review the migrations code for accuracy.</span></span>

<span data-ttu-id="9c9eb-157">Pomocí příslušného příkazu použijte migraci na databázi.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-157">Apply the migration to the database using the appropriate command.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="9c9eb-158">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="9c9eb-158">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef database update
```

### <a name="visual-studio"></a>[<span data-ttu-id="9c9eb-159">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9c9eb-159">Visual Studio</span></span>](#tab/vs)

``` powershell
Update-Database
```

***

### <a name="empty-migrations"></a><span data-ttu-id="9c9eb-160">Prázdné migrace</span><span class="sxs-lookup"><span data-stu-id="9c9eb-160">Empty migrations</span></span>

<span data-ttu-id="9c9eb-161">Někdy je vhodné přidat migraci bez provedení jakýchkoli změn modelu.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-161">Sometimes it's useful to add a migration without making any model changes.</span></span> <span data-ttu-id="9c9eb-162">V takovém případě přidání nové migrace vytvoří soubory kódu s prázdnými třídami.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-162">In this case, adding a new migration creates code files with empty classes.</span></span> <span data-ttu-id="9c9eb-163">Tuto migraci můžete přizpůsobit tak, aby prováděla operace, které přímo nesouvisejí s modelem EF Core.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-163">You can customize this migration to perform operations that don't directly relate to the EF Core model.</span></span> <span data-ttu-id="9c9eb-164">Tímto způsobem můžete chtít spravovat tyto věci:</span><span class="sxs-lookup"><span data-stu-id="9c9eb-164">Some things you might want to manage this way are:</span></span>

* <span data-ttu-id="9c9eb-165">Fulltextové vyhledávání</span><span class="sxs-lookup"><span data-stu-id="9c9eb-165">Full-Text Search</span></span>
* <span data-ttu-id="9c9eb-166">Functions</span><span class="sxs-lookup"><span data-stu-id="9c9eb-166">Functions</span></span>
* <span data-ttu-id="9c9eb-167">Uložené procedury</span><span class="sxs-lookup"><span data-stu-id="9c9eb-167">Stored procedures</span></span>
* <span data-ttu-id="9c9eb-168">Aktivační události</span><span class="sxs-lookup"><span data-stu-id="9c9eb-168">Triggers</span></span>
* <span data-ttu-id="9c9eb-169">Zobrazení</span><span class="sxs-lookup"><span data-stu-id="9c9eb-169">Views</span></span>

## <a name="remove-a-migration"></a><span data-ttu-id="9c9eb-170">Odebrání migrace</span><span class="sxs-lookup"><span data-stu-id="9c9eb-170">Remove a migration</span></span>

<span data-ttu-id="9c9eb-171">Někdy můžete přidat migraci a zajistěte, abyste před použitím v modelu EF Core provedli další změny.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-171">Sometimes you add a migration and realize you need to make additional changes to your EF Core model before applying it.</span></span> <span data-ttu-id="9c9eb-172">K odebrání poslední migrace použijte tento příkaz.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-172">To remove the last migration, use this command.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="9c9eb-173">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="9c9eb-173">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations remove
```

### <a name="visual-studio"></a>[<span data-ttu-id="9c9eb-174">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9c9eb-174">Visual Studio</span></span>](#tab/vs)

``` powershell
Remove-Migration
```

***

<span data-ttu-id="9c9eb-175">Po odebrání migrace můžete provést další změny modelu a znovu ho přidat.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-175">After removing the migration, you can make the additional model changes and add it again.</span></span>

## <a name="revert-a-migration"></a><span data-ttu-id="9c9eb-176">Vrácení migrace zpět</span><span class="sxs-lookup"><span data-stu-id="9c9eb-176">Revert a migration</span></span>

<span data-ttu-id="9c9eb-177">Pokud jste již v databázi použili migraci (nebo několik migrací), ale potřebujete je vrátit zpět, můžete použít stejný příkaz pro použití migrace, ale zadejte název migrace, na kterou se chcete vrátit.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-177">If you already applied a migration (or several migrations) to the database but need to revert it, you can use the same command to apply migrations, but specify the name of the migration you want to roll back to.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="9c9eb-178">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="9c9eb-178">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef database update LastGoodMigration
```

### <a name="visual-studio"></a>[<span data-ttu-id="9c9eb-179">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9c9eb-179">Visual Studio</span></span>](#tab/vs)

``` powershell
Update-Database LastGoodMigration
```

***

## <a name="generate-sql-scripts"></a><span data-ttu-id="9c9eb-180">Generování skriptů SQL</span><span class="sxs-lookup"><span data-stu-id="9c9eb-180">Generate SQL scripts</span></span>

<span data-ttu-id="9c9eb-181">Při ladění migrací nebo jejich nasazení do provozní databáze je užitečné vygenerovat skript SQL.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-181">When debugging your migrations or deploying them to a production database, it's useful to generate a SQL script.</span></span> <span data-ttu-id="9c9eb-182">Skript pak může být dále revidován pro přesnost a vyladěný tak, aby vyhovoval potřebám provozní databáze.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-182">The script can then be further reviewed for accuracy and tuned to fit the needs of a production database.</span></span> <span data-ttu-id="9c9eb-183">Skript se dá použít i ve spojení s technologií nasazení.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-183">The script can also be used in conjunction with a deployment technology.</span></span> <span data-ttu-id="9c9eb-184">Základní příkaz je následující.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-184">The basic command is as follows.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="9c9eb-185">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="9c9eb-185">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

#### <a name="basic-usage"></a><span data-ttu-id="9c9eb-186">Základní využití</span><span class="sxs-lookup"><span data-stu-id="9c9eb-186">Basic Usage</span></span>
```dotnetcli
dotnet ef migrations script
```

#### <a name="with-from-to-implied"></a><span data-ttu-id="9c9eb-187">S od (do implicitního)</span><span class="sxs-lookup"><span data-stu-id="9c9eb-187">With From (to implied)</span></span>
<span data-ttu-id="9c9eb-188">Tím se z této migrace vygeneruje skript SQL na nejnovější migraci.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-188">This will generate a SQL script from this migration to the latest migration.</span></span>
```dotnetcli
dotnet ef migrations script 20190725054716_Add_new_tables
```

#### <a name="with-from-and-to"></a><span data-ttu-id="9c9eb-189">S z a na</span><span class="sxs-lookup"><span data-stu-id="9c9eb-189">With From and To</span></span>
<span data-ttu-id="9c9eb-190">Tím se vygeneruje skript SQL z `from` migrace do zadané `to` migrace.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-190">This will generate a SQL script from the `from` migration to the specified `to` migration.</span></span>
```dotnetcli
dotnet ef migrations script 20190725054716_Add_new_tables 20190829031257_Add_audit_table
```
<span data-ttu-id="9c9eb-191">Můžete použít `from`, který je novější než `to`, aby bylo možné vygenerovat skript pro vrácení zpět.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-191">You can use a `from` that is newer than the `to` in order to generate a rollback script.</span></span> <span data-ttu-id="9c9eb-192">*Vezměte prosím na vědomí potenciální scénáře ztráty dat.*</span><span class="sxs-lookup"><span data-stu-id="9c9eb-192">*Please take note of potential data loss scenarios.*</span></span>

### <a name="visual-studio"></a>[<span data-ttu-id="9c9eb-193">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9c9eb-193">Visual Studio</span></span>](#tab/vs)

#### <a name="basic-usage"></a><span data-ttu-id="9c9eb-194">Základní využití</span><span class="sxs-lookup"><span data-stu-id="9c9eb-194">Basic Usage</span></span>
``` powershell
Script-Migration
```

#### <a name="with-from-to-implied"></a><span data-ttu-id="9c9eb-195">S od (do implicitního)</span><span class="sxs-lookup"><span data-stu-id="9c9eb-195">With From (to implied)</span></span>
<span data-ttu-id="9c9eb-196">Tím se z této migrace vygeneruje skript SQL na nejnovější migraci.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-196">This will generate a SQL script from this migration to the latest migration.</span></span>
```powershell
Script-Migration 20190725054716_Add_new_tables
```

#### <a name="with-from-and-to"></a><span data-ttu-id="9c9eb-197">S z a na</span><span class="sxs-lookup"><span data-stu-id="9c9eb-197">With From and To</span></span>
<span data-ttu-id="9c9eb-198">Tím se vygeneruje skript SQL z `from` migrace do zadané `to` migrace.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-198">This will generate a SQL script from the `from` migration to the specified `to` migration.</span></span>
```powershell
Script-Migration 20190725054716_Add_new_tables 20190829031257_Add_audit_table
```
<span data-ttu-id="9c9eb-199">Můžete použít `from`, který je novější než `to`, aby bylo možné vygenerovat skript pro vrácení zpět.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-199">You can use a `from` that is newer than the `to` in order to generate a rollback script.</span></span> <span data-ttu-id="9c9eb-200">*Vezměte prosím na vědomí potenciální scénáře ztráty dat.*</span><span class="sxs-lookup"><span data-stu-id="9c9eb-200">*Please take note of potential data loss scenarios.*</span></span>

***

<span data-ttu-id="9c9eb-201">Tento příkaz obsahuje několik možností.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-201">There are several options to this command.</span></span>

<span data-ttu-id="9c9eb-202">Migrace **z** migrace by měla být poslední migrací použitou pro databázi před spuštěním skriptu.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-202">The **from** migration should be the last migration applied to the database before running the script.</span></span> <span data-ttu-id="9c9eb-203">Pokud žádné migrace nepoužíváte, zadejte `0` (Toto je výchozí nastavení).</span><span class="sxs-lookup"><span data-stu-id="9c9eb-203">If no migrations have been applied, specify `0` (this is the default).</span></span>

<span data-ttu-id="9c9eb-204">Migrace **do** je poslední migrace, která se použije pro databázi po spuštění skriptu.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-204">The **to** migration is the last migration that will be applied to the database after running the script.</span></span> <span data-ttu-id="9c9eb-205">Tato hodnota se nastaví jako poslední migrace v projektu.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-205">This defaults to the last migration in your project.</span></span>

<span data-ttu-id="9c9eb-206">Skript **idempotentní** může být volitelně vygenerován.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-206">An **idempotent** script can optionally be generated.</span></span> <span data-ttu-id="9c9eb-207">Tento skript aplikuje migrace jenom v případě, že se v databázi ještě nepoužily.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-207">This script only applies migrations if they haven't already been applied to the database.</span></span> <span data-ttu-id="9c9eb-208">To je užitečné, Pokud nevíte přesně, co poslední migrace použila v databázi, nebo pokud nasazujete do více databází, které mohou být při jiné migraci.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-208">This is useful if you don't exactly know what the last migration applied to the database was or if you are deploying to multiple databases that may each be at a different migration.</span></span>

## <a name="apply-migrations-at-runtime"></a><span data-ttu-id="9c9eb-209">Použít migrace za běhu</span><span class="sxs-lookup"><span data-stu-id="9c9eb-209">Apply migrations at runtime</span></span>

<span data-ttu-id="9c9eb-210">Některé aplikace můžou chtít při spuštění nebo prvním spuštění použít migrace za běhu.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-210">Some apps may want to apply migrations at runtime during startup or first run.</span></span> <span data-ttu-id="9c9eb-211">Použijte metodu `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-211">Do this using the `Migrate()` method.</span></span>

<span data-ttu-id="9c9eb-212">Tato metoda sestaví na `IMigrator` službu, která se dá použít pro pokročilejší scénáře.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-212">This method builds on top of the `IMigrator` service, which can be used for more advanced scenarios.</span></span> <span data-ttu-id="9c9eb-213">Pro přístup k němu použijte `myDbContext.GetInfrastructure().GetService<IMigrator>()`.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-213">Use `myDbContext.GetInfrastructure().GetService<IMigrator>()` to access it.</span></span>

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
>
> * <span data-ttu-id="9c9eb-214">Tento přístup není pro všechny.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-214">This approach isn't for everyone.</span></span> <span data-ttu-id="9c9eb-215">I když je skvělé pro aplikace s místní databází, většina aplikací bude vyžadovat robustnější strategii nasazení, jako je generování skriptů SQL.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-215">While it's great for apps with a local database, most applications will require more robust deployment strategy like generating SQL scripts.</span></span>
> * <span data-ttu-id="9c9eb-216">Nevolejte `EnsureCreated()` před `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-216">Don't call `EnsureCreated()` before `Migrate()`.</span></span> <span data-ttu-id="9c9eb-217">`EnsureCreated()` obchází migrace za účelem vytvoření schématu, což způsobí, že `Migrate()` selže.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-217">`EnsureCreated()` bypasses Migrations to create the schema, which causes `Migrate()` to fail.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9c9eb-218">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9c9eb-218">Next steps</span></span>

<span data-ttu-id="9c9eb-219">Další informace naleznete v tématu <xref:core/miscellaneous/cli/index>.</span><span class="sxs-lookup"><span data-stu-id="9c9eb-219">For more information, see <xref:core/miscellaneous/cli/index>.</span></span>
