---
title: Migrace – jádro EF
author: bricelam
ms.author: bricelam
ms.date: 10/05/2018
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: 190057daed61c58c1f89ee8d775913458e413a50
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "80136201"
---
# <a name="migrations"></a><span data-ttu-id="a7eeb-102">Migrace</span><span class="sxs-lookup"><span data-stu-id="a7eeb-102">Migrations</span></span>

<span data-ttu-id="a7eeb-103">Datový model se během vývoje změní a synchronizuje se s databází.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-103">A data model changes during development and gets out of sync with the database.</span></span> <span data-ttu-id="a7eeb-104">Můžete přetažení databáze a nechat EF vytvořit nový, který odpovídá modelu, ale tento postup má za následek ztrátu dat.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-104">You can drop the database and let EF create a new one that matches the model, but this procedure results in the loss of data.</span></span> <span data-ttu-id="a7eeb-105">Funkce migrace v EF Core poskytuje způsob, jak postupně aktualizovat schéma databáze, aby bylo synchronizováno s datovým modelem aplikace při zachování existujících dat v databázi.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-105">The migrations feature in EF Core provides a way to incrementally update the database schema to keep it in sync with the application's data model while preserving existing data in the database.</span></span>

<span data-ttu-id="a7eeb-106">Migrace zahrnují nástroje příkazového řádku a api, které pomáhají s následujícími úkoly:</span><span class="sxs-lookup"><span data-stu-id="a7eeb-106">Migrations includes command-line tools and APIs that help with the following tasks:</span></span>

* <span data-ttu-id="a7eeb-107">[Vytvořte migraci](#create-a-migration).</span><span class="sxs-lookup"><span data-stu-id="a7eeb-107">[Create a migration](#create-a-migration).</span></span> <span data-ttu-id="a7eeb-108">Vygenerujte kód, který může aktualizovat databázi a synchronizovat ji se sadou změn modelu.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-108">Generate code that can update the database to sync it with a set of model changes.</span></span>
* <span data-ttu-id="a7eeb-109">[Aktualizujte databázi](#update-the-database).</span><span class="sxs-lookup"><span data-stu-id="a7eeb-109">[Update the database](#update-the-database).</span></span> <span data-ttu-id="a7eeb-110">Chcete-li aktualizovat schéma databáze, použijte čekající migrace.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-110">Apply pending migrations to update the database schema.</span></span>
* <span data-ttu-id="a7eeb-111">[Přizpůsobení kódu migrace](#customize-migration-code).</span><span class="sxs-lookup"><span data-stu-id="a7eeb-111">[Customize migration code](#customize-migration-code).</span></span> <span data-ttu-id="a7eeb-112">Někdy je třeba upravit nebo doplnit generovaný kód.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-112">Sometimes the generated code needs to be modified or supplemented.</span></span>
* <span data-ttu-id="a7eeb-113">[Odebrání migrace](#remove-a-migration).</span><span class="sxs-lookup"><span data-stu-id="a7eeb-113">[Remove a migration](#remove-a-migration).</span></span> <span data-ttu-id="a7eeb-114">Odstraňte generovaný kód.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-114">Delete the generated code.</span></span>
* <span data-ttu-id="a7eeb-115">[Vrátit migraci](#revert-a-migration).</span><span class="sxs-lookup"><span data-stu-id="a7eeb-115">[Revert a migration](#revert-a-migration).</span></span> <span data-ttu-id="a7eeb-116">Vrátit zpět změny databáze.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-116">Undo the database changes.</span></span>
* <span data-ttu-id="a7eeb-117">[Generovat skripty SQL](#generate-sql-scripts).</span><span class="sxs-lookup"><span data-stu-id="a7eeb-117">[Generate SQL scripts](#generate-sql-scripts).</span></span> <span data-ttu-id="a7eeb-118">Možná budete potřebovat skript k aktualizaci produkční databáze nebo k řešení potíží s kódem migrace.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-118">You might need a script to update a production database or to troubleshoot migration code.</span></span>
* <span data-ttu-id="a7eeb-119">[Použití migrace za běhu](#apply-migrations-at-runtime).</span><span class="sxs-lookup"><span data-stu-id="a7eeb-119">[Apply migrations at runtime](#apply-migrations-at-runtime).</span></span> <span data-ttu-id="a7eeb-120">Pokud aktualizace návrhu a spuštěné skripty nejsou nejlepší `Migrate()` mise, zavolejte metodu.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-120">When design-time updates and running scripts aren't the best options, call the `Migrate()` method.</span></span>

> [!TIP]
> <span data-ttu-id="a7eeb-121">Pokud `DbContext` je v jiném sestavení než projekt spuštění, můžete explicitně zadat cílové a spouštěcí projekty v [nástrojích konzoly Správce balíčků](xref:core/miscellaneous/cli/powershell#target-and-startup-project) nebo [v nástrojích rozhraní .NET Core CLI](xref:core/miscellaneous/cli/dotnet#target-project-and-startup-project).</span><span class="sxs-lookup"><span data-stu-id="a7eeb-121">If the `DbContext` is in a different assembly than the startup project, you can explicitly specify the target and startup projects in either the [Package Manager Console tools](xref:core/miscellaneous/cli/powershell#target-and-startup-project) or the [.NET Core CLI tools](xref:core/miscellaneous/cli/dotnet#target-project-and-startup-project).</span></span>

## <a name="install-the-tools"></a><span data-ttu-id="a7eeb-122">Instalace nástrojů</span><span class="sxs-lookup"><span data-stu-id="a7eeb-122">Install the tools</span></span>

<span data-ttu-id="a7eeb-123">Nainstalujte [nástroje příkazového řádku](xref:core/miscellaneous/cli/index):</span><span class="sxs-lookup"><span data-stu-id="a7eeb-123">Install the [command-line tools](xref:core/miscellaneous/cli/index):</span></span>

* <span data-ttu-id="a7eeb-124">Pro Visual Studio doporučujeme [nástroje konzoly Správce balíčků](xref:core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="a7eeb-124">For Visual Studio, we recommend the [Package Manager Console tools](xref:core/miscellaneous/cli/powershell).</span></span>
* <span data-ttu-id="a7eeb-125">Pro ostatní vývojová prostředí zvolte [nástroje rozhraní .NET Core CLI](xref:core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="a7eeb-125">For other development environments, choose the [.NET Core CLI tools](xref:core/miscellaneous/cli/dotnet).</span></span>

## <a name="create-a-migration"></a><span data-ttu-id="a7eeb-126">Vytvoření migrace</span><span class="sxs-lookup"><span data-stu-id="a7eeb-126">Create a migration</span></span>

<span data-ttu-id="a7eeb-127">Po definování [počátečního modelu](xref:core/modeling/index)je čas vytvořit databázi.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-127">After you've [defined your initial model](xref:core/modeling/index), it's time to create the database.</span></span> <span data-ttu-id="a7eeb-128">Chcete-li přidat počáteční migraci, spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-128">To add an initial migration, run the following command.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="a7eeb-129">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="a7eeb-129">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add InitialCreate
```

### <a name="visual-studio"></a>[<span data-ttu-id="a7eeb-130">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a7eeb-130">Visual Studio</span></span>](#tab/vs)

``` powershell
Add-Migration InitialCreate
```

***

<span data-ttu-id="a7eeb-131">V adresáři **Migrace** jsou do projektu přidány tři soubory:</span><span class="sxs-lookup"><span data-stu-id="a7eeb-131">Three files are added to your project under the **Migrations** directory:</span></span>

* <span data-ttu-id="a7eeb-132">**XXXXXXXXXXXXXX_InitialCreate.cs**--Hlavní soubor migrace.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-132">**XXXXXXXXXXXXXX_InitialCreate.cs**--The main migrations file.</span></span> <span data-ttu-id="a7eeb-133">Obsahuje operace nezbytné k použití `Up()`migrace (v ) a `Down()`vrátit ji (v ).</span><span class="sxs-lookup"><span data-stu-id="a7eeb-133">Contains the operations necessary to apply the migration (in `Up()`) and to revert it (in `Down()`).</span></span>
* <span data-ttu-id="a7eeb-134">**XXXXXXXXXXXXXX_InitialCreate.Designer.cs**--Soubor metadat migrace.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-134">**XXXXXXXXXXXXXX_InitialCreate.Designer.cs**--The migrations metadata file.</span></span> <span data-ttu-id="a7eeb-135">Obsahuje informace používané EF.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-135">Contains information used by EF.</span></span>
* <span data-ttu-id="a7eeb-136">**MyContextModelSnapshot.cs**--Snímek aktuálního modelu.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-136">**MyContextModelSnapshot.cs**--A snapshot of your current model.</span></span> <span data-ttu-id="a7eeb-137">Slouží k určení, co se změnilo při přidávání další migrace.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-137">Used to determine what changed when adding the next migration.</span></span>

<span data-ttu-id="a7eeb-138">Časové razítko v názvu souboru pomáhá udržet je uspořádány chronologicky, takže můžete vidět průběh změn.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-138">The timestamp in the filename helps keep them ordered chronologically so you can see the progression of changes.</span></span>

> [!TIP]
> <span data-ttu-id="a7eeb-139">Můžete přesunout soubory migrace a změnit jejich obor názvů.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-139">You are free to move Migrations files and change their namespace.</span></span> <span data-ttu-id="a7eeb-140">Nové migrace jsou vytvořeny jako na stejné úrovni poslední migrace.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-140">New migrations are created as siblings of the last migration.</span></span>

## <a name="update-the-database"></a><span data-ttu-id="a7eeb-141">Aktualizace databáze</span><span class="sxs-lookup"><span data-stu-id="a7eeb-141">Update the database</span></span>

<span data-ttu-id="a7eeb-142">Dále použijte migraci do databáze k vytvoření schématu.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-142">Next, apply the migration to the database to create the schema.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="a7eeb-143">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="a7eeb-143">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef database update
```

### <a name="visual-studio"></a>[<span data-ttu-id="a7eeb-144">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a7eeb-144">Visual Studio</span></span>](#tab/vs)

``` powershell
Update-Database
```

***

## <a name="customize-migration-code"></a><span data-ttu-id="a7eeb-145">Přizpůsobení kódu migrace</span><span class="sxs-lookup"><span data-stu-id="a7eeb-145">Customize migration code</span></span>

<span data-ttu-id="a7eeb-146">Po provedení změn modelu EF Core je schéma databáze pravděpodobně nesynchronizované. Chcete-li ji aktualizovat, přidejte další migraci.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-146">After making changes to your EF Core model, the database schema might be out of sync. To bring it up to date, add another migration.</span></span> <span data-ttu-id="a7eeb-147">Název migrace lze použít jako zprávu o potvrzení v systému správy verzí.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-147">The migration name can be used like a commit message in a version control system.</span></span> <span data-ttu-id="a7eeb-148">Můžete například zvolit název jako *AddProductReviews,* pokud je změna novou třídou entity pro recenze.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-148">For example, you might choose a name like *AddProductReviews* if the change is a new entity class for reviews.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="a7eeb-149">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="a7eeb-149">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add AddProductReviews
```

### <a name="visual-studio"></a>[<span data-ttu-id="a7eeb-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a7eeb-150">Visual Studio</span></span>](#tab/vs)

``` powershell
Add-Migration AddProductReviews
```

***

<span data-ttu-id="a7eeb-151">Jakmile je migrace šetrné k ráže (kód pro ni vygenerovaný), zkontrolujte kód pro přesnost a přidat, odebrat nebo upravit všechny operace potřebné k použití správně.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-151">Once the migration is scaffolded (code generated for it), review the code for accuracy and add, remove or modify any operations required to apply it correctly.</span></span>

<span data-ttu-id="a7eeb-152">Migrace může například obsahovat následující operace:</span><span class="sxs-lookup"><span data-stu-id="a7eeb-152">For example, a migration might contain the following operations:</span></span>

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

<span data-ttu-id="a7eeb-153">Zatímco tyto operace, aby schéma databáze kompatibilní, nezachovávají existující názvy zákazníků.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-153">While these operations make the database schema compatible, they don't preserve the existing customer names.</span></span> <span data-ttu-id="a7eeb-154">Chcete-li, aby to bylo lepší, přepište to takto.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-154">To make it better, rewrite it as follows.</span></span>

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
> <span data-ttu-id="a7eeb-155">Proces migrace lešení varuje, když operace může mít za následek ztrátu dat (jako je uvolnění sloupce).</span><span class="sxs-lookup"><span data-stu-id="a7eeb-155">The migration scaffolding process warns when an operation might result in data loss (like dropping a column).</span></span> <span data-ttu-id="a7eeb-156">Pokud se zobrazí toto upozornění, nezapomeňte zkontrolovat kód migrace pro přesnost.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-156">If you see that warning, be especially sure to review the migrations code for accuracy.</span></span>

<span data-ttu-id="a7eeb-157">Použijte migraci do databáze pomocí příslušného příkazu.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-157">Apply the migration to the database using the appropriate command.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="a7eeb-158">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="a7eeb-158">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef database update
```

### <a name="visual-studio"></a>[<span data-ttu-id="a7eeb-159">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a7eeb-159">Visual Studio</span></span>](#tab/vs)

``` powershell
Update-Database
```

***

### <a name="empty-migrations"></a><span data-ttu-id="a7eeb-160">Prázdné migrace</span><span class="sxs-lookup"><span data-stu-id="a7eeb-160">Empty migrations</span></span>

<span data-ttu-id="a7eeb-161">Někdy je užitečné přidat migraci bez provedení změn modelu.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-161">Sometimes it's useful to add a migration without making any model changes.</span></span> <span data-ttu-id="a7eeb-162">V tomto případě přidání nové migrace vytvoří soubory kódu s prázdnými třídami.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-162">In this case, adding a new migration creates code files with empty classes.</span></span> <span data-ttu-id="a7eeb-163">Tuto migraci můžete přizpůsobit tak, aby prováděla operace, které přímo nesouvisejí s modelem EF Core.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-163">You can customize this migration to perform operations that don't directly relate to the EF Core model.</span></span> <span data-ttu-id="a7eeb-164">Některé věci, které byste mohli chtít spravovat tímto způsobem, jsou:</span><span class="sxs-lookup"><span data-stu-id="a7eeb-164">Some things you might want to manage this way are:</span></span>

* <span data-ttu-id="a7eeb-165">Fulltextové vyhledávání</span><span class="sxs-lookup"><span data-stu-id="a7eeb-165">Full-Text Search</span></span>
* <span data-ttu-id="a7eeb-166">Functions</span><span class="sxs-lookup"><span data-stu-id="a7eeb-166">Functions</span></span>
* <span data-ttu-id="a7eeb-167">Uložené procedury</span><span class="sxs-lookup"><span data-stu-id="a7eeb-167">Stored procedures</span></span>
* <span data-ttu-id="a7eeb-168">Aktivační události</span><span class="sxs-lookup"><span data-stu-id="a7eeb-168">Triggers</span></span>
* <span data-ttu-id="a7eeb-169">Zobrazení</span><span class="sxs-lookup"><span data-stu-id="a7eeb-169">Views</span></span>

## <a name="remove-a-migration"></a><span data-ttu-id="a7eeb-170">Odebrání migrace</span><span class="sxs-lookup"><span data-stu-id="a7eeb-170">Remove a migration</span></span>

<span data-ttu-id="a7eeb-171">Někdy přidáte migraci a uvědomíte si, že před použitím je třeba provést další změny modelu EF Core.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-171">Sometimes you add a migration and realize you need to make additional changes to your EF Core model before applying it.</span></span> <span data-ttu-id="a7eeb-172">Chcete-li odebrat poslední migraci, použijte tento příkaz.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-172">To remove the last migration, use this command.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="a7eeb-173">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="a7eeb-173">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations remove
```

### <a name="visual-studio"></a>[<span data-ttu-id="a7eeb-174">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a7eeb-174">Visual Studio</span></span>](#tab/vs)

``` powershell
Remove-Migration
```

***

<span data-ttu-id="a7eeb-175">Po odebrání migrace můžete provést další změny modelu a znovu jej přidat.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-175">After removing the migration, you can make the additional model changes and add it again.</span></span>

## <a name="revert-a-migration"></a><span data-ttu-id="a7eeb-176">Vrácení migrace</span><span class="sxs-lookup"><span data-stu-id="a7eeb-176">Revert a migration</span></span>

<span data-ttu-id="a7eeb-177">Pokud jste již migraci (nebo několik migrací) do databáze použili, ale potřebujete ji vrátit, můžete použít stejný příkaz k aplikování migrace, ale zadejte název migrace, ke které chcete přejít zpět.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-177">If you already applied a migration (or several migrations) to the database but need to revert it, you can use the same command to apply migrations, but specify the name of the migration you want to roll back to.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="a7eeb-178">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="a7eeb-178">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef database update LastGoodMigration
```

### <a name="visual-studio"></a>[<span data-ttu-id="a7eeb-179">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a7eeb-179">Visual Studio</span></span>](#tab/vs)

``` powershell
Update-Database LastGoodMigration
```

***

## <a name="generate-sql-scripts"></a><span data-ttu-id="a7eeb-180">Generovat skripty SQL</span><span class="sxs-lookup"><span data-stu-id="a7eeb-180">Generate SQL scripts</span></span>

<span data-ttu-id="a7eeb-181">Při ladění migrace nebo jejich nasazení do produkční databáze, je užitečné generovat skript SQL.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-181">When debugging your migrations or deploying them to a production database, it's useful to generate a SQL script.</span></span> <span data-ttu-id="a7eeb-182">Skript pak může být dále přezkoumána pro přesnost a vyladěny tak, aby vyhovovaly potřebám produkční databáze.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-182">The script can then be further reviewed for accuracy and tuned to fit the needs of a production database.</span></span> <span data-ttu-id="a7eeb-183">Skript lze také použít ve spojení s technologií nasazení.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-183">The script can also be used in conjunction with a deployment technology.</span></span> <span data-ttu-id="a7eeb-184">Základní příkaz je následující.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-184">The basic command is as follows.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="a7eeb-185">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="a7eeb-185">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

#### <a name="basic-usage"></a><span data-ttu-id="a7eeb-186">Základní použití</span><span class="sxs-lookup"><span data-stu-id="a7eeb-186">Basic Usage</span></span>
```dotnetcli
dotnet ef migrations script
```

#### <a name="with-from-to-implied"></a><span data-ttu-id="a7eeb-187">S From (na implicitní)</span><span class="sxs-lookup"><span data-stu-id="a7eeb-187">With From (to implied)</span></span>
<span data-ttu-id="a7eeb-188">Tím se vygeneruje skript SQL z této migrace na nejnovější migraci.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-188">This will generate a SQL script from this migration to the latest migration.</span></span>
```dotnetcli
dotnet ef migrations script 20190725054716_Add_new_tables
```

#### <a name="with-from-and-to"></a><span data-ttu-id="a7eeb-189">S od a do</span><span class="sxs-lookup"><span data-stu-id="a7eeb-189">With From and To</span></span>
<span data-ttu-id="a7eeb-190">Tím se vygeneruje `from` skript SQL `to` z migrace na zadanou migraci.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-190">This will generate a SQL script from the `from` migration to the specified `to` migration.</span></span>
```dotnetcli
dotnet ef migrations script 20190725054716_Add_new_tables 20190829031257_Add_audit_table
```
<span data-ttu-id="a7eeb-191">Můžete `from` použít, který je novější `to` než pro generování skriptu vrácení zpět.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-191">You can use a `from` that is newer than the `to` in order to generate a rollback script.</span></span> <span data-ttu-id="a7eeb-192">*Vezměte prosím na vědomí potenciální scénáře ztráty dat.*</span><span class="sxs-lookup"><span data-stu-id="a7eeb-192">*Please take note of potential data loss scenarios.*</span></span>

### <a name="visual-studio"></a>[<span data-ttu-id="a7eeb-193">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a7eeb-193">Visual Studio</span></span>](#tab/vs)

#### <a name="basic-usage"></a><span data-ttu-id="a7eeb-194">Základní použití</span><span class="sxs-lookup"><span data-stu-id="a7eeb-194">Basic Usage</span></span>
``` powershell
Script-Migration
```

#### <a name="with-from-to-implied"></a><span data-ttu-id="a7eeb-195">S From (na implicitní)</span><span class="sxs-lookup"><span data-stu-id="a7eeb-195">With From (to implied)</span></span>
<span data-ttu-id="a7eeb-196">Tím se vygeneruje skript SQL z této migrace na nejnovější migraci.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-196">This will generate a SQL script from this migration to the latest migration.</span></span>
```powershell
Script-Migration 20190725054716_Add_new_tables
```

#### <a name="with-from-and-to"></a><span data-ttu-id="a7eeb-197">S od a do</span><span class="sxs-lookup"><span data-stu-id="a7eeb-197">With From and To</span></span>
<span data-ttu-id="a7eeb-198">Tím se vygeneruje `from` skript SQL `to` z migrace na zadanou migraci.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-198">This will generate a SQL script from the `from` migration to the specified `to` migration.</span></span>
```powershell
Script-Migration 20190725054716_Add_new_tables 20190829031257_Add_audit_table
```
<span data-ttu-id="a7eeb-199">Můžete `from` použít, který je novější `to` než pro generování skriptu vrácení zpět.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-199">You can use a `from` that is newer than the `to` in order to generate a rollback script.</span></span> <span data-ttu-id="a7eeb-200">*Vezměte prosím na vědomí potenciální scénáře ztráty dat.*</span><span class="sxs-lookup"><span data-stu-id="a7eeb-200">*Please take note of potential data loss scenarios.*</span></span>

***

<span data-ttu-id="a7eeb-201">Tento příkaz má několik možností.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-201">There are several options to this command.</span></span>

<span data-ttu-id="a7eeb-202">**Z** migrace by měla být poslední migrace použitá v databázi před spuštěním skriptu.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-202">The **from** migration should be the last migration applied to the database before running the script.</span></span> <span data-ttu-id="a7eeb-203">Pokud nebyly použity žádné `0` migrace, zadejte (toto je výchozí).</span><span class="sxs-lookup"><span data-stu-id="a7eeb-203">If no migrations have been applied, specify `0` (this is the default).</span></span>

<span data-ttu-id="a7eeb-204">Migrace **na** je poslední migrace, která bude použita v databázi po spuštění skriptu.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-204">The **to** migration is the last migration that will be applied to the database after running the script.</span></span> <span data-ttu-id="a7eeb-205">To to to to toto nastavení má být poslední migrace v projektu.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-205">This defaults to the last migration in your project.</span></span>

<span data-ttu-id="a7eeb-206">**Idempotentní** skript může být volitelně generován.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-206">An **idempotent** script can optionally be generated.</span></span> <span data-ttu-id="a7eeb-207">Tento skript použije migraci pouze v případě, že ještě nebyly použity v databázi.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-207">This script only applies migrations if they haven't already been applied to the database.</span></span> <span data-ttu-id="a7eeb-208">To je užitečné, pokud přesně nevíte, jaká byla poslední migrace použitá v databázi, nebo pokud nasazujete do více databází, které mohou být při jiné migraci.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-208">This is useful if you don't exactly know what the last migration applied to the database was or if you are deploying to multiple databases that may each be at a different migration.</span></span>

## <a name="apply-migrations-at-runtime"></a><span data-ttu-id="a7eeb-209">Použití migrace za běhu</span><span class="sxs-lookup"><span data-stu-id="a7eeb-209">Apply migrations at runtime</span></span>

<span data-ttu-id="a7eeb-210">Některé aplikace mohou chtít použít migrace za běhu při spuštění nebo prvním spuštění.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-210">Some apps may want to apply migrations at runtime during startup or first run.</span></span> <span data-ttu-id="a7eeb-211">Proveďte to `Migrate()` metodou.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-211">Do this using the `Migrate()` method.</span></span>

<span data-ttu-id="a7eeb-212">Tato metoda vychází z `IMigrator` nad služby, které lze použít pro pokročilejší scénáře.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-212">This method builds on top of the `IMigrator` service, which can be used for more advanced scenarios.</span></span> <span data-ttu-id="a7eeb-213">Používá `myDbContext.GetInfrastructure().GetService<IMigrator>()` se k přístupu.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-213">Use `myDbContext.GetInfrastructure().GetService<IMigrator>()` to access it.</span></span>

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
>
> * <span data-ttu-id="a7eeb-214">Tento přístup není pro každého.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-214">This approach isn't for everyone.</span></span> <span data-ttu-id="a7eeb-215">I když je to skvělé pro aplikace s místní databází, většina aplikací bude vyžadovat robustnější strategii nasazení, jako je generování skriptů SQL.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-215">While it's great for apps with a local database, most applications will require more robust deployment strategy like generating SQL scripts.</span></span>
> * <span data-ttu-id="a7eeb-216">Nevolejte `EnsureCreated()` dříve `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-216">Don't call `EnsureCreated()` before `Migrate()`.</span></span> <span data-ttu-id="a7eeb-217">`EnsureCreated()`obchází migrace k vytvoření schématu, `Migrate()` který způsobí selhání.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-217">`EnsureCreated()` bypasses Migrations to create the schema, which causes `Migrate()` to fail.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a7eeb-218">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a7eeb-218">Next steps</span></span>

<span data-ttu-id="a7eeb-219">Další informace naleznete v tématu <xref:core/miscellaneous/cli/index>.</span><span class="sxs-lookup"><span data-stu-id="a7eeb-219">For more information, see <xref:core/miscellaneous/cli/index>.</span></span>
