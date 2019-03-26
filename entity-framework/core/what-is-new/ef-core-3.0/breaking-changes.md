---
title: Zásadní změny v EF Core 3.0 – EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 7ed55d4cae36f6b25059a5b218db4b0d5e2fb266
ms.sourcegitcommit: 645785187ae23ddf7d7b0642c7a4da5ffb0c7f30
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/25/2019
ms.locfileid: "58419741"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="c7b02-102">Rozbíjející změny zahrnuté v EF Core 3.0 (aktuálně ve verzi preview)</span><span class="sxs-lookup"><span data-stu-id="c7b02-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c7b02-103">Mějte prosím na paměti, že sady funkcí a plány budoucích verzí se vždy mohou změnit a přestože se snažíme se zachovat aktuální, nemusí odrážet naše nejnovější plány vůbec na této stránce vyprší.</span><span class="sxs-lookup"><span data-stu-id="c7b02-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="c7b02-104">Následující změny chování a rozhraní API by mohly přerušit aplikace vyvinuté pro jádro EF Core 2.2.x při upgradu na 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="c7b02-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="c7b02-105">Změny, které chceme ovlivní jen poskytovatelé databází jsou popsány v části [poskytovatel změny](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="c7b02-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="c7b02-106">Tady nejsou uvedené konce v nové vlastnosti představené z jednoho 3.0 ve verzi preview další 3.0 ve verzi Preview.</span><span class="sxs-lookup"><span data-stu-id="c7b02-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="c7b02-107">Dotazy LINQ se už nevyhodnocuje na straně klienta</span><span class="sxs-lookup"><span data-stu-id="c7b02-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="c7b02-108">[Sledování problému #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[také zjistit problém #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="c7b02-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="c7b02-109">Tato změna bude zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="c7b02-109">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="c7b02-110">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-110">**Old behavior**</span></span>

<span data-ttu-id="c7b02-111">Než 3.0 když EF Core nelze převést výraz, který byl součástí sady dotazů SQL nebo parametr, automaticky vyhodnotí výraz na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="c7b02-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="c7b02-112">Ve výchozím nastavení hodnocení klientů potencionálně nákladným výrazů aktivuje pouze upozornění.</span><span class="sxs-lookup"><span data-stu-id="c7b02-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="c7b02-113">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-113">**New behavior**</span></span>

<span data-ttu-id="c7b02-114">Od verze 3.0, EF Core umožňuje pouze výrazy v projekci nejvyšší úrovně (poslední `Select()` volání v dotazu) k vyhodnocení na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="c7b02-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="c7b02-115">Když výrazy v další části dotazu nelze převést na SQL nebo parametr, je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="c7b02-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="c7b02-116">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c7b02-116">**Why**</span></span>

<span data-ttu-id="c7b02-117">Vyhodnocení automatickou klientskou dotazů umožňuje mnoho dotazů, který se spustí i v případě, že je důležité části nelze přeložit.</span><span class="sxs-lookup"><span data-stu-id="c7b02-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="c7b02-118">Toto chování může způsobit neočekávané a potenciálně škodlivé chování, které může přestat pouze zřejmé v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="c7b02-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="c7b02-119">Například podmínku v `Where()` volání, které nelze přeložit může způsobit, že všechny řádky z tabulky přesunou z databázového serveru a filtr, který bude použit na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="c7b02-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="c7b02-120">Tato situace může pokračovat nezjištěné po snadno Pokud tabulka obsahuje jenom pár řádků v vývoje, ale přístupů pevné přesun aplikace do produkčního prostředí, kde tabulka může obsahovat miliony řádků.</span><span class="sxs-lookup"><span data-stu-id="c7b02-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="c7b02-121">Upozornění vyhodnocení klienta dokázaly také příliš jednoduché ignorovat během vývoje.</span><span class="sxs-lookup"><span data-stu-id="c7b02-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="c7b02-122">Kromě toho může automatickou klientskou vyhodnocení způsobit problémy, ve kterých vylepšení překladu dotazu pro konkrétní výrazy způsobila neúmyslnému zásadní změny mezi verzí.</span><span class="sxs-lookup"><span data-stu-id="c7b02-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="c7b02-123">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="c7b02-123">**Mitigations**</span></span>

<span data-ttu-id="c7b02-124">Pokud dotaz nelze přeložit plně, pak buď přepište dotaz ve formuláři, který lze přeložit, nebo použijte `AsEnumerable()`, `ToList()`, nebo podobný explicitně přenést data zpět do klienta ve kterém pak může být dále zpracovány pomocí LINQ na objekty.</span><span class="sxs-lookup"><span data-stu-id="c7b02-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="c7b02-125">Entity Framework Core už nejsou součástí sdíleného rozhraní ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c7b02-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="c7b02-126">Oznámení týkající se sledování problému #325</span><span class="sxs-lookup"><span data-stu-id="c7b02-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="c7b02-127">Tato změna byla zavedena v ASP.NET Core 3.0 ve verzi preview 1.</span><span class="sxs-lookup"><span data-stu-id="c7b02-127">This change was introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="c7b02-128">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-128">**Old behavior**</span></span>

<span data-ttu-id="c7b02-129">Před ASP.NET Core 3.0, když se přidá odkaz na balíček pro `Microsoft.AspNetCore.App` nebo `Microsoft.AspNetCore.All`, měl by obsahovat EF Core a některé EF Core zprostředkovatele dat, jako jsou zprostředkovatele SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c7b02-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="c7b02-130">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-130">**New behavior**</span></span>

<span data-ttu-id="c7b02-131">Od verze 3.0, rozhraní ASP.NET Core sdílené neobsahuje EF Core nebo žádným zprostředkovatelům dat. EF Core.</span><span class="sxs-lookup"><span data-stu-id="c7b02-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="c7b02-132">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c7b02-132">**Why**</span></span>

<span data-ttu-id="c7b02-133">Před touto změnou získávání EF Core vyžaduje jiný postup v závislosti na tom, jestli aplikace cílené ASP.NET Core a SQL Server, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="c7b02-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="c7b02-134">Upgrade ASP.NET Core také vynutit upgrade EF Core a zprostředkovatele SQL Server, který nemusí být vždy žádoucí.</span><span class="sxs-lookup"><span data-stu-id="c7b02-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="c7b02-135">Díky této změně se možnosti načítání EF Core je stejná ve všech zprostředkovatelů, podporovaná implementace .NET a typy aplikací.</span><span class="sxs-lookup"><span data-stu-id="c7b02-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="c7b02-136">Vývojáři také nyní mohou ovládat přesně při upgradu EF Core a EF Core zprostředkovatelé dat.</span><span class="sxs-lookup"><span data-stu-id="c7b02-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="c7b02-137">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="c7b02-137">**Mitigations**</span></span>

<span data-ttu-id="c7b02-138">V aplikaci ASP.NET Core 3.0 nebo jakékoli jiné podporované aplikace použít EF Core, explicitně přidáte odkaz na balíček k poskytovateli databáze EF Core, který bude aplikace používat.</span><span class="sxs-lookup"><span data-stu-id="c7b02-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="query-execution-is-logged-at-debug-level"></a><span data-ttu-id="c7b02-139">Provádění dotazu se protokoluje při ladění na úrovni</span><span class="sxs-lookup"><span data-stu-id="c7b02-139">Query execution is logged at Debug level</span></span>

[<span data-ttu-id="c7b02-140">Sledování problému #14523</span><span class="sxs-lookup"><span data-stu-id="c7b02-140">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="c7b02-141">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="c7b02-141">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="c7b02-142">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-142">**Old behavior**</span></span>

<span data-ttu-id="c7b02-143">Před EF Core 3.0, provádění dotazů a jiných příkazů protokolu byla zaznamenána v `Info` úroveň.</span><span class="sxs-lookup"><span data-stu-id="c7b02-143">Before EF Core 3.0, execution of queries and other commands was logged at the `Info` level.</span></span>

<span data-ttu-id="c7b02-144">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-144">**New behavior**</span></span>

<span data-ttu-id="c7b02-145">Od verze EF Core 3.0, protokolování spuštění příkazu/SQL je na `Debug` úroveň.</span><span class="sxs-lookup"><span data-stu-id="c7b02-145">Starting with EF Core 3.0, logging of command/SQL execution is at the `Debug` level.</span></span>

<span data-ttu-id="c7b02-146">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c7b02-146">**Why**</span></span>

<span data-ttu-id="c7b02-147">Tato změna byla provedena jak snížit šum na `Info` úrovně protokolování.</span><span class="sxs-lookup"><span data-stu-id="c7b02-147">This change was made to reduce the noise at the `Info` log level.</span></span>

<span data-ttu-id="c7b02-148">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="c7b02-148">**Mitigations**</span></span>

<span data-ttu-id="c7b02-149">Tato událost protokolování je definována `RelationalEventId.CommandExecuting` s ID události 20100.</span><span class="sxs-lookup"><span data-stu-id="c7b02-149">This logging event is defined by `RelationalEventId.CommandExecuting` with event ID 20100.</span></span>
<span data-ttu-id="c7b02-150">Do protokolu SQL na `Info` úroveň znovu, explicitně nakonfigurovat na úrovni `OnConfiguring` nebo `AddDbContext`.</span><span class="sxs-lookup"><span data-stu-id="c7b02-150">To log SQL at the `Info` level again, explicitly configure the level in `OnConfiguring` or `AddDbContext`.</span></span>
<span data-ttu-id="c7b02-151">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c7b02-151">For example:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Info)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="c7b02-152">Dočasné hodnoty klíče už nejsou nastavené na instancí entit</span><span class="sxs-lookup"><span data-stu-id="c7b02-152">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="c7b02-153">Sledování problému #12378</span><span class="sxs-lookup"><span data-stu-id="c7b02-153">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="c7b02-154">Tato změna byla zavedená v EF Core 3.0 – náhled 2.</span><span class="sxs-lookup"><span data-stu-id="c7b02-154">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="c7b02-155">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-155">**Old behavior**</span></span>

<span data-ttu-id="c7b02-156">Před EF Core 3.0 dočasné hodnoty přiřadily se všechny vlastnosti klíče, které by později skutečné hodnoty generován databází.</span><span class="sxs-lookup"><span data-stu-id="c7b02-156">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="c7b02-157">Obvykle tyto dočasné hodnoty byly velké záporná čísla.</span><span class="sxs-lookup"><span data-stu-id="c7b02-157">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="c7b02-158">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-158">**New behavior**</span></span>

<span data-ttu-id="c7b02-159">Od verze 3.0, EF Core ukládá dočasné hodnotu klíče jako součást informací o sledování entity a ponechá samotné beze změny vlastnosti klíče.</span><span class="sxs-lookup"><span data-stu-id="c7b02-159">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="c7b02-160">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c7b02-160">**Why**</span></span>

<span data-ttu-id="c7b02-161">Tato změna byla provedena zabránit chybně stávají trvalé při entita, která byla dříve sledován pomocí funkce některé dočasné hodnoty klíče `DbContext` instance se přesune na jiný `DbContext` instance.</span><span class="sxs-lookup"><span data-stu-id="c7b02-161">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="c7b02-162">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="c7b02-162">**Mitigations**</span></span>

<span data-ttu-id="c7b02-163">Na staré chování může záviset aplikace, které přiřazují hodnoty primárního klíče na cizí klíče formuláře přidružení mezi entitami, pokud primární klíče generované úložištěm a patří do entity v `Added` stavu.</span><span class="sxs-lookup"><span data-stu-id="c7b02-163">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="c7b02-164">Toho se lze vyvarovat podle:</span><span class="sxs-lookup"><span data-stu-id="c7b02-164">This can be avoided by:</span></span>
* <span data-ttu-id="c7b02-165">Bez použití klíče generované úložištěm.</span><span class="sxs-lookup"><span data-stu-id="c7b02-165">Not using store-generated keys.</span></span>
* <span data-ttu-id="c7b02-166">Nastavení vlastnosti navigace na vztahů namísto nastavování hodnot cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="c7b02-166">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="c7b02-167">Získejte skutečné hodnoty dočasné klíče z informací o sledování entity.</span><span class="sxs-lookup"><span data-stu-id="c7b02-167">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="c7b02-168">Například `context.Entry(blog).Property(e => e.Id).CurrentValue` i v případě vrátí hodnotu dočasné `blog.Id` samotný nebyla nastavena.</span><span class="sxs-lookup"><span data-stu-id="c7b02-168">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="c7b02-169">Metoda DetectChanges respektuje klíčových hodnot generovaných úložištěm</span><span class="sxs-lookup"><span data-stu-id="c7b02-169">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="c7b02-170">Sledování problému #14616</span><span class="sxs-lookup"><span data-stu-id="c7b02-170">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="c7b02-171">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="c7b02-171">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="c7b02-172">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-172">**Old behavior**</span></span>

<span data-ttu-id="c7b02-173">Před EF Core 3.0 Nesledované entity objevila `DetectChanges` mají sledovat v `Added` stavu a vložené jako nový řádek, kdy `SaveChanges` je volána.</span><span class="sxs-lookup"><span data-stu-id="c7b02-173">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="c7b02-174">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-174">**New behavior**</span></span>

<span data-ttu-id="c7b02-175">Od verze EF Core 3.0, pokud používá entity generované hodnoty klíče a některá z hodnot klíče je nastavena, pak entity se bude sledovat v `Modified` stavu.</span><span class="sxs-lookup"><span data-stu-id="c7b02-175">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="c7b02-176">To znamená, že řádek entity se předpokládá, že existují a budou aktualizace `SaveChanges` je volána.</span><span class="sxs-lookup"><span data-stu-id="c7b02-176">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="c7b02-177">Pokud není nastavena hodnota klíče, nebo pokud není typ entity pomocí vygenerované klíče, pak nová entita se bude dál sledovat jako `Added` stejně jako v předchozích verzích.</span><span class="sxs-lookup"><span data-stu-id="c7b02-177">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="c7b02-178">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c7b02-178">**Why**</span></span>

<span data-ttu-id="c7b02-179">Tato změna byla provedena na umožňují snadněji a konzistentnější pro práci s grafy odpojené entity při použití klíče generované úložištěm.</span><span class="sxs-lookup"><span data-stu-id="c7b02-179">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="c7b02-180">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="c7b02-180">**Mitigations**</span></span>

<span data-ttu-id="c7b02-181">Tato změna může přerušit aplikace, pokud typ entity je nakonfigurovaný na použití vygenerovat klíče, ale hodnoty klíče jsou explicitně nastavené pro nové instance.</span><span class="sxs-lookup"><span data-stu-id="c7b02-181">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="c7b02-182">Oprava je explicitně konfigurovat klíčové vlastnosti nejsou generovány pomocí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="c7b02-182">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="c7b02-183">Například pomocí rozhraní fluent API:</span><span class="sxs-lookup"><span data-stu-id="c7b02-183">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="c7b02-184">Nebo s anotacemi dat:</span><span class="sxs-lookup"><span data-stu-id="c7b02-184">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="c7b02-185">Kaskádové odstranění teď dojde okamžitě ve výchozím nastavení</span><span class="sxs-lookup"><span data-stu-id="c7b02-185">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="c7b02-186">Sledování problému #10114</span><span class="sxs-lookup"><span data-stu-id="c7b02-186">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="c7b02-187">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="c7b02-187">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="c7b02-188">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-188">**Old behavior**</span></span>

<span data-ttu-id="c7b02-189">Než 3.0 EF Core použít kaskádové akce (odstranění závislých položek při odstranění požadovaný instanční objekt nebo když porušeno vztah k požadované instanční objekt) nebyly provedeny dokud byla volána metoda SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="c7b02-189">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="c7b02-190">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-190">**New behavior**</span></span>

<span data-ttu-id="c7b02-191">Od verze 3.0, EF Core provede kaskádové akce ihned po zjištění spouštěcí podmínky.</span><span class="sxs-lookup"><span data-stu-id="c7b02-191">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="c7b02-192">Například volání `context.Remove()` odstranit instančního objektu entity se výsledek ve všech sledovat související požadované položky závislé na také nastavena na `Deleted` okamžitě.</span><span class="sxs-lookup"><span data-stu-id="c7b02-192">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="c7b02-193">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c7b02-193">**Why**</span></span>

<span data-ttu-id="c7b02-194">Tato změna byla provedena na zlepšení uživatelského rozhraní pro datových vazbách a auditování scénáře, kdy je potřeba vysvětlit entit, které se odstraní _před_ `SaveChanges` je volána.</span><span class="sxs-lookup"><span data-stu-id="c7b02-194">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="c7b02-195">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="c7b02-195">**Mitigations**</span></span>

<span data-ttu-id="c7b02-196">Předchozí chování lze obnovit nastavením na `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="c7b02-196">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="c7b02-197">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c7b02-197">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="c7b02-198">Typy dotazů jsou spojeny s typy entit</span><span class="sxs-lookup"><span data-stu-id="c7b02-198">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="c7b02-199">Sledování problému #14194</span><span class="sxs-lookup"><span data-stu-id="c7b02-199">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="c7b02-200">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="c7b02-200">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="c7b02-201">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-201">**Old behavior**</span></span>

<span data-ttu-id="c7b02-202">Před EF Core 3.0 [typy dotazů](xref:core/modeling/query-types) byly prostředky provádět dotazy na data, která nedefinuje primární klíč strukturovaným způsobem.</span><span class="sxs-lookup"><span data-stu-id="c7b02-202">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="c7b02-203">To znamená typ dotazu byla použita pro mapování typů entit bez klíčů (pravděpodobně ze zobrazení, ale pravděpodobně z tabulky) při pravidelných entity typ byl použit při klíč byl k dispozici (více pravděpodobně z tabulky, ale pravděpodobně ze zobrazení).</span><span class="sxs-lookup"><span data-stu-id="c7b02-203">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="c7b02-204">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-204">**New behavior**</span></span>

<span data-ttu-id="c7b02-205">Typ dotazu teď bude pouze typ entity bez primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="c7b02-205">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="c7b02-206">Typy entit bez kódu mají stejné funkce jako typy dotazů v předchozích verzích.</span><span class="sxs-lookup"><span data-stu-id="c7b02-206">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="c7b02-207">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c7b02-207">**Why**</span></span>

<span data-ttu-id="c7b02-208">Tato změna byla provedena, abyste snížili nejasnosti kolem účel typy dotazů.</span><span class="sxs-lookup"><span data-stu-id="c7b02-208">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="c7b02-209">Konkrétně jsou typy bez klíčů entit a z tohoto důvodu jsou ze své podstaty jen pro čtení, ale by neměl být použit pouze z důvodu typu entity musí být jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="c7b02-209">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="c7b02-210">Podobně jsou často namapované na zobrazení, ale je to jenom, protože zobrazení není často definují klíče.</span><span class="sxs-lookup"><span data-stu-id="c7b02-210">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="c7b02-211">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="c7b02-211">**Mitigations**</span></span>

<span data-ttu-id="c7b02-212">Následující části rozhraní API jsou teď zastaralé:</span><span class="sxs-lookup"><span data-stu-id="c7b02-212">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="c7b02-213">**`ModelBuilder.Query<>()`** – Místo toho `ModelBuilder.Entity<>().HasNoKey()` musí být volána k označení typu entity tak, že má žádné klíče.</span><span class="sxs-lookup"><span data-stu-id="c7b02-213">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="c7b02-214">To by stále probíhá konfigurace podle konvence se vyhnete chybné konfigurace, když primární klíč se očekává, ale neodpovídá konvenci.</span><span class="sxs-lookup"><span data-stu-id="c7b02-214">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="c7b02-215">**`DbQuery<>`** – Místo toho `DbSet<>` by měla sloužit.</span><span class="sxs-lookup"><span data-stu-id="c7b02-215">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="c7b02-216">**`DbContext.Query<>()`** – Místo toho `DbContext.Set<>()` by měla sloužit.</span><span class="sxs-lookup"><span data-stu-id="c7b02-216">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="c7b02-217">Došlo ke změně konfigurace rozhraní API pro vlastní typ relace</span><span class="sxs-lookup"><span data-stu-id="c7b02-217">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="c7b02-218">[Sledování problému #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[sledování problému #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[sledování problému #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="c7b02-218">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="c7b02-219">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="c7b02-219">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="c7b02-220">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-220">**Old behavior**</span></span>

<span data-ttu-id="c7b02-221">Před EF Core 3.0 byla provedena konfigurace vlastněné vztah přímo po `OwnsOne` nebo `OwnsMany` volání.</span><span class="sxs-lookup"><span data-stu-id="c7b02-221">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="c7b02-222">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-222">**New behavior**</span></span>

<span data-ttu-id="c7b02-223">Od verze EF Core 3.0, že už fluent API pro konfiguraci vlastnosti navigace vlastníkovi použitím `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="c7b02-223">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="c7b02-224">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c7b02-224">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="c7b02-225">Konfigurace související se o vztah mezi vlastníka a vlastní by měl být zřetězené teď po `WithOwner()` podobně jako na konfiguraci jiné vztahy.</span><span class="sxs-lookup"><span data-stu-id="c7b02-225">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="c7b02-226">Při konfiguraci pro vlastní typ, samotný by stále možné zřetězit po `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="c7b02-226">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="c7b02-227">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c7b02-227">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details, eb =>
    {
        eb.WithOwner()
            .HasForeignKey(e => e.AlternateId)
            .HasConstraintName("FK_OrderDetails");
            
        eb.ToTable("OrderDetails");
        eb.HasKey(e => e.AlternateId);
        eb.HasIndex(e => e.Id);

        eb.HasOne(e => e.Customer).WithOne();

        eb.HasData(
            new OrderDetails
            {
                AlternateId = 1,
                Id = -1
            });
    });
```

<span data-ttu-id="c7b02-228">Kromě volá `Entity()`, `HasOne()`, nebo `Set()` s typem vlastnictví cíl se vyvolají výjimku.</span><span class="sxs-lookup"><span data-stu-id="c7b02-228">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="c7b02-229">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c7b02-229">**Why**</span></span>

<span data-ttu-id="c7b02-230">Tato změna byla provedena vytvořit jasnější oddělení mezi konfigurací samotného typu vlastnictví a _vztah k_ typ vlastnictví.</span><span class="sxs-lookup"><span data-stu-id="c7b02-230">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="c7b02-231">Tím zase nejednoznačnosti a záměny kolem metody, jako je `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="c7b02-231">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="c7b02-232">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="c7b02-232">**Mitigations**</span></span>

<span data-ttu-id="c7b02-233">Změna konfigurace vlastněné typ vztahů používat nové plochy rozhraní API, jak je znázorněno v příkladu výše.</span><span class="sxs-lookup"><span data-stu-id="c7b02-233">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="c7b02-234">Konvence vlastnost cizího klíče už neodpovídá stejný název jako vlastnost instančního objektu</span><span class="sxs-lookup"><span data-stu-id="c7b02-234">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="c7b02-235">Sledování problému #13274</span><span class="sxs-lookup"><span data-stu-id="c7b02-235">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="c7b02-236">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="c7b02-236">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="c7b02-237">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-237">**Old behavior**</span></span>

<span data-ttu-id="c7b02-238">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="c7b02-238">Consider the following model:</span></span>
```C#
public class Customer
{
    public int CustomerId { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}

```
<span data-ttu-id="c7b02-239">Před EF Core 3.0 `CustomerId` vlastnost se použije pro cizí klíč konvencí.</span><span class="sxs-lookup"><span data-stu-id="c7b02-239">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="c7b02-240">Nicméně pokud `Order` je typ vlastnictví, a potom to by také provést `CustomerId` primární klíč a to se většinou očekává.</span><span class="sxs-lookup"><span data-stu-id="c7b02-240">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="c7b02-241">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-241">**New behavior**</span></span>

<span data-ttu-id="c7b02-242">Od verze 3.0, nebude zkoušet EF Core při využívání vlastností cizího klíče podle konvence, pokud mají stejný název jako vlastnost instančního objektu.</span><span class="sxs-lookup"><span data-stu-id="c7b02-242">Starting with 3.0, EF Core won't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="c7b02-243">Název instančního objektu typu zřetězený s názvem hlavní vlastnosti a název navigační zřetězená s vzory názvů vlastnosti principal budou stále odpovídat.</span><span class="sxs-lookup"><span data-stu-id="c7b02-243">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="c7b02-244">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c7b02-244">For example:</span></span>

```C#
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}
```

```C#
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int BuyerId { get; set; }
    public Customer Buyer { get; set; }
}
```

<span data-ttu-id="c7b02-245">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c7b02-245">**Why**</span></span>

<span data-ttu-id="c7b02-246">Tato změna byla provedena na chybně nedefinujte vlastnost primárního klíče typ vlastnictví.</span><span class="sxs-lookup"><span data-stu-id="c7b02-246">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="c7b02-247">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="c7b02-247">**Mitigations**</span></span>

<span data-ttu-id="c7b02-248">Pokud byla vlastnost má být cizí klíč a proto část primárního klíče a pak explicitně jako takový ho nakonfigurujte.</span><span class="sxs-lookup"><span data-stu-id="c7b02-248">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="c7b02-249">Jednotlivé vlastnosti pomocí generování klíčů nezávislé celé číslo v paměti</span><span class="sxs-lookup"><span data-stu-id="c7b02-249">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="c7b02-250">Sledování problému #6872</span><span class="sxs-lookup"><span data-stu-id="c7b02-250">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="c7b02-251">Tato změna bude zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="c7b02-251">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="c7b02-252">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-252">**Old behavior**</span></span>

<span data-ttu-id="c7b02-253">Před EF Core 3.0 se použil jeden generátor sdíleného hodnot pro všechny vlastnosti klíče celé číslo v paměti.</span><span class="sxs-lookup"><span data-stu-id="c7b02-253">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="c7b02-254">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-254">**New behavior**</span></span>

<span data-ttu-id="c7b02-255">Od verze EF Core 3.0, každé celé číslo klíčová vlastnost získá vlastní generátor hodnot při použití databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="c7b02-255">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="c7b02-256">Také pokud se odstraní databáze, pak generování klíčů je obnovit pro všechny tabulky.</span><span class="sxs-lookup"><span data-stu-id="c7b02-256">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="c7b02-257">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c7b02-257">**Why**</span></span>

<span data-ttu-id="c7b02-258">Tato změna byla provedena zarovnat generování klíče v paměti blíže k generování klíčů skutečná databáze a zlepšit schopnost izolace testů od sebe navzájem při použití databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="c7b02-258">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="c7b02-259">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="c7b02-259">**Mitigations**</span></span>

<span data-ttu-id="c7b02-260">Toto může rozbít aplikaci, která se spoléhá na konkrétní hodnoty klíče v paměti nastavit.</span><span class="sxs-lookup"><span data-stu-id="c7b02-260">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="c7b02-261">Zvažte místo toho spoléhat na konkrétní hodnoty klíče, ani aktualizaci tak, aby odpovídala nové chování.</span><span class="sxs-lookup"><span data-stu-id="c7b02-261">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="c7b02-262">Základní pole se používají ve výchozím nastavení</span><span class="sxs-lookup"><span data-stu-id="c7b02-262">Backing fields are used by default</span></span>

[<span data-ttu-id="c7b02-263">Sledování problému #12430</span><span class="sxs-lookup"><span data-stu-id="c7b02-263">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="c7b02-264">Tato změna byla zavedená v EF Core 3.0 – náhled 2.</span><span class="sxs-lookup"><span data-stu-id="c7b02-264">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="c7b02-265">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-265">**Old behavior**</span></span>

<span data-ttu-id="c7b02-266">Než 3.0 i v případě, že pomocné pole vlastnosti zná, EF Core by stále ve výchozím nastavení čtení a zápisu hodnoty vlastnosti pomocí metody getter a setter vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="c7b02-266">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="c7b02-267">K tomuto výjimka provádění dotazů, kde pomocné pole se nastavuje přímo Pokud jsou známé.</span><span class="sxs-lookup"><span data-stu-id="c7b02-267">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="c7b02-268">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-268">**New behavior**</span></span>

<span data-ttu-id="c7b02-269">Počínaje EF Core 3.0, pokud je znám pomocným polem vlastnosti potom budou vždy číst a Zapisovat vlastnosti pomocí pole zálohování.</span><span class="sxs-lookup"><span data-stu-id="c7b02-269">Starting with EF Core 3.0, if the backing field for a property is known, then will always read and write that property using the backing field.</span></span>
<span data-ttu-id="c7b02-270">Pokud aplikace se spoléhá na další chování zakódovaný do metody getter nebo setter to může způsobit přerušení aplikace.</span><span class="sxs-lookup"><span data-stu-id="c7b02-270">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="c7b02-271">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c7b02-271">**Why**</span></span>

<span data-ttu-id="c7b02-272">Tato změna byla provedena zabránit EF Core chybně aktivací obchodní logiky ve výchozím nastavení při provádění databázových operací zahrnující entity.</span><span class="sxs-lookup"><span data-stu-id="c7b02-272">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="c7b02-273">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="c7b02-273">**Mitigations**</span></span>

<span data-ttu-id="c7b02-274">Prostřednictvím konfigurace režim přístupu k vlastnosti v rozhraní API fluent modelBuilder lze obnovit chování pre-3.0.</span><span class="sxs-lookup"><span data-stu-id="c7b02-274">The pre-3.0 behavior can be restored through configuration of the property access mode in the modelBuilder fluent API.</span></span>
<span data-ttu-id="c7b02-275">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c7b02-275">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="c7b02-276">Vyvolání výjimky, pokud je nalezeno více polí kompatibilní zálohování</span><span class="sxs-lookup"><span data-stu-id="c7b02-276">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="c7b02-277">Sledování problému #12523</span><span class="sxs-lookup"><span data-stu-id="c7b02-277">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="c7b02-278">Tato změna bude zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="c7b02-278">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="c7b02-279">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-279">**Old behavior**</span></span>

<span data-ttu-id="c7b02-280">Před EF Core 3.0 Pokud odpovídá více polí pravidel pro vyhledání pomocným polem vlastnosti, pak vybere jedno pole na základě některých pořadí priority.</span><span class="sxs-lookup"><span data-stu-id="c7b02-280">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="c7b02-281">To může způsobit nesprávné pole pro použití v případech, nejednoznačný.</span><span class="sxs-lookup"><span data-stu-id="c7b02-281">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="c7b02-282">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-282">**New behavior**</span></span>

<span data-ttu-id="c7b02-283">Od verze EF Core 3.0, pokud více polí budou odpovídat na stejnou vlastnost, pak je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="c7b02-283">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="c7b02-284">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c7b02-284">**Why**</span></span>

<span data-ttu-id="c7b02-285">Tato změna byla provedena, abyste se vyhnuli použití tiše jedno pole před jiným při může obsahovat jen jedno správné.</span><span class="sxs-lookup"><span data-stu-id="c7b02-285">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="c7b02-286">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="c7b02-286">**Mitigations**</span></span>

<span data-ttu-id="c7b02-287">Vlastnosti s nejednoznačný základní pole musí mít pole pro použití explicitně zadán.</span><span class="sxs-lookup"><span data-stu-id="c7b02-287">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="c7b02-288">Například pomocí rozhraní fluent API:</span><span class="sxs-lookup"><span data-stu-id="c7b02-288">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="c7b02-289">AddDbContext/AddDbContextPool zavolejte už AddLogging a AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="c7b02-289">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="c7b02-290">Sledování problému #14756</span><span class="sxs-lookup"><span data-stu-id="c7b02-290">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="c7b02-291">Tato změna bude zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="c7b02-291">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="c7b02-292">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-292">**Old behavior**</span></span>

<span data-ttu-id="c7b02-293">Před EF Core 3.0, volání `AddDbContext` nebo `AddDbContextPool` by také zaregistrovat protokolování a ukládání služeb s D.I prostřednictvím volání do mezipaměti [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) a [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="c7b02-293">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="c7b02-294">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-294">**New behavior**</span></span>

<span data-ttu-id="c7b02-295">Od verze EF Core 3.0, `AddDbContext` a `AddDbContextPool` nebude registru tyto služby s DI (Dependency Injection).</span><span class="sxs-lookup"><span data-stu-id="c7b02-295">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="c7b02-296">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c7b02-296">**Why**</span></span>

<span data-ttu-id="c7b02-297">EF Core 3.0 nevyžaduje, že tyto služby jsou v cotainer DI vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="c7b02-297">EF Core 3.0 does not require that these services are in the application's DI cotainer.</span></span> <span data-ttu-id="c7b02-298">Nicméně pokud `ILoggerFactory` je zaregistrovaný v aplikačním kontejneru DI, pak bude i nadále využívat EF Core.</span><span class="sxs-lookup"><span data-stu-id="c7b02-298">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="c7b02-299">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="c7b02-299">**Mitigations**</span></span>

<span data-ttu-id="c7b02-300">Pokud vaše aplikace potřebuje tyto služby, registrujte je explicitně s využitím kontejnerů DI [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) nebo [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="c7b02-300">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="c7b02-301">DbContext.Entry teď provádí místní metoda DetectChanges</span><span class="sxs-lookup"><span data-stu-id="c7b02-301">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="c7b02-302">Sledování problému #13552</span><span class="sxs-lookup"><span data-stu-id="c7b02-302">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="c7b02-303">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="c7b02-303">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="c7b02-304">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-304">**Old behavior**</span></span>

<span data-ttu-id="c7b02-305">Před EF Core 3.0, volání `DbContext.Entry` způsobí změny, aby se rozpoznal pro všechny sledované entity.</span><span class="sxs-lookup"><span data-stu-id="c7b02-305">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="c7b02-306">Tím zajistíte, že stav zpřístupněn v `EntityEntry` byla aktuální.</span><span class="sxs-lookup"><span data-stu-id="c7b02-306">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="c7b02-307">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-307">**New behavior**</span></span>

<span data-ttu-id="c7b02-308">Spouští se s EF Core 3.0, volání `DbContext.Entry` se teď jenom pokus o zjištění změn v dané entitě a všechny sledované instančního objektu entity, které s ním souvisejí.</span><span class="sxs-lookup"><span data-stu-id="c7b02-308">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="c7b02-309">To znamená, že se změní jinde nemusí byl zjištěn zavoláním této metody, které by mohly mít vliv na stav aplikace.</span><span class="sxs-lookup"><span data-stu-id="c7b02-309">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="c7b02-310">Všimněte si, že pokud `ChangeTracker.AutoDetectChangesEnabled` je nastavena na `false` pak i tato detekce místní změny se deaktivuje.</span><span class="sxs-lookup"><span data-stu-id="c7b02-310">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="c7b02-311">Jiné metody, které způsobují detekce změn – například `ChangeTracker.Entries` a `SaveChanges`– způsobí úplné `DetectChanges` všech sledovat entity.</span><span class="sxs-lookup"><span data-stu-id="c7b02-311">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="c7b02-312">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c7b02-312">**Why**</span></span>

<span data-ttu-id="c7b02-313">Tato změna byla provedena pro zlepšení výkonu výchozí použití `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="c7b02-313">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="c7b02-314">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="c7b02-314">**Mitigations**</span></span>

<span data-ttu-id="c7b02-315">Volání `ChgangeTracker.DetectChanges()` explicitně před voláním `Entry` k zajištění chování pre-3.0.</span><span class="sxs-lookup"><span data-stu-id="c7b02-315">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="c7b02-316">Řetězec a bajtové pole klíčů se nerozlišují klientem generovaná ve výchozím nastavení</span><span class="sxs-lookup"><span data-stu-id="c7b02-316">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="c7b02-317">Sledování problému #14617</span><span class="sxs-lookup"><span data-stu-id="c7b02-317">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="c7b02-318">Tato změna bude zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="c7b02-318">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="c7b02-319">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-319">**Old behavior**</span></span>

<span data-ttu-id="c7b02-320">Před EF Core 3.0 `string` a `byte[]` klíčové vlastnosti použití explicitním nastavením nenulová hodnota.</span><span class="sxs-lookup"><span data-stu-id="c7b02-320">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="c7b02-321">V takovém případě by se vygenerovala hodnota klíče na klientovi jako identifikátor GUID serializovat do bajtů pro `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="c7b02-321">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="c7b02-322">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-322">**New behavior**</span></span>

<span data-ttu-id="c7b02-323">Od verze EF Core 3.0 výjimka bude vyvolána označující, že nebyla nastavena žádná hodnota klíče.</span><span class="sxs-lookup"><span data-stu-id="c7b02-323">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="c7b02-324">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c7b02-324">**Why**</span></span>

<span data-ttu-id="c7b02-325">Tato změna byla provedena, protože klient generován `string` / `byte[]` hodnoty nejsou obecně užitečné a výchozí chování dostal pevné argumentovat o generované hodnoty klíče v běžným způsobem.</span><span class="sxs-lookup"><span data-stu-id="c7b02-325">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="c7b02-326">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="c7b02-326">**Mitigations**</span></span>

<span data-ttu-id="c7b02-327">Chování pre-3.0 je možné získat tak, že explicitně zadáte, by měl klíčové vlastnosti použít generované hodnoty, pokud je nastavena žádná hodnota jiná než null.</span><span class="sxs-lookup"><span data-stu-id="c7b02-327">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="c7b02-328">Například pomocí rozhraní fluent API:</span><span class="sxs-lookup"><span data-stu-id="c7b02-328">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="c7b02-329">Nebo s anotacemi dat:</span><span class="sxs-lookup"><span data-stu-id="c7b02-329">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="c7b02-330">Implementaci třídy ILoggerFactory je nyní vymezené služby</span><span class="sxs-lookup"><span data-stu-id="c7b02-330">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="c7b02-331">Sledování problému #14698</span><span class="sxs-lookup"><span data-stu-id="c7b02-331">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="c7b02-332">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="c7b02-332">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="c7b02-333">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-333">**Old behavior**</span></span>

<span data-ttu-id="c7b02-334">Před EF Core 3.0 `ILoggerFactory` bylo zaregistrováno jako služba typu singleton.</span><span class="sxs-lookup"><span data-stu-id="c7b02-334">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="c7b02-335">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-335">**New behavior**</span></span>

<span data-ttu-id="c7b02-336">Od verze EF Core 3.0, `ILoggerFactory` je teď zaregistrovaný jako obor.</span><span class="sxs-lookup"><span data-stu-id="c7b02-336">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="c7b02-337">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c7b02-337">**Why**</span></span>

<span data-ttu-id="c7b02-338">Tato změna byla provedena umožňující přidružení protokolovací nástroj se `DbContext` instanci, která povoluje další funkce a odebírá někdy patologických chování, jako je například obrovské množství poskytovatelů vnitřní chybě služby.</span><span class="sxs-lookup"><span data-stu-id="c7b02-338">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="c7b02-339">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="c7b02-339">**Mitigations**</span></span>

<span data-ttu-id="c7b02-340">Tato změna by neměla mít vliv kód aplikace, pokud není registrace a použití vlastních služeb v poskytovateli EF Core vnitřní chybě služby.</span><span class="sxs-lookup"><span data-stu-id="c7b02-340">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="c7b02-341">Tato akce není běžné.</span><span class="sxs-lookup"><span data-stu-id="c7b02-341">This isn't common.</span></span>
<span data-ttu-id="c7b02-342">V těchto případech většinu toho, co bude dál fungovat, ale žádné služby typu singleton, která byla v závislosti na `ILoggerFactory` bude nutné změnit tak, aby získat `ILoggerFactory` jiným způsobem.</span><span class="sxs-lookup"><span data-stu-id="c7b02-342">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="c7b02-343">Pokud narazíte na situace tímto způsobem, založte prosím problém na na [sledování problémů Githubu EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) a dejte nám vědět, jak používáte `ILoggerFactory` tak, aby nám můžete lépe porozumět nechcete v budoucnu znovu rozdělit.</span><span class="sxs-lookup"><span data-stu-id="c7b02-343">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="idbcontextoptionsextensionwithdebuginfo-merged-into-idbcontextoptionsextension"></a><span data-ttu-id="c7b02-344">Sloučí IDbContextOptionsExtension IDbContextOptionsExtensionWithDebugInfo</span><span class="sxs-lookup"><span data-stu-id="c7b02-344">IDbContextOptionsExtensionWithDebugInfo merged into IDbContextOptionsExtension</span></span>

[<span data-ttu-id="c7b02-345">Sledování problému #13552</span><span class="sxs-lookup"><span data-stu-id="c7b02-345">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="c7b02-346">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="c7b02-346">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="c7b02-347">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-347">**Old behavior**</span></span>

<span data-ttu-id="c7b02-348">`IDbContextOptionsExtensionWithDebugInfo` Další volitelné rozhraní byla prodloužena z `IDbContextOptionsExtension` pro vyvarování rozbíjející změně rozhraní během cyklu vydání verze 2.x.</span><span class="sxs-lookup"><span data-stu-id="c7b02-348">`IDbContextOptionsExtensionWithDebugInfo` was an additional optional interface extended from `IDbContextOptionsExtension` to avoid making a breaking change to the interface during the 2.x release cycle.</span></span>

<span data-ttu-id="c7b02-349">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-349">**New behavior**</span></span>

<span data-ttu-id="c7b02-350">Rozhraní jsou nyní sloučeny do `IDbContextOptionsExtension`.</span><span class="sxs-lookup"><span data-stu-id="c7b02-350">The interfaces are now merged together into `IDbContextOptionsExtension`.</span></span>

<span data-ttu-id="c7b02-351">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c7b02-351">**Why**</span></span>

<span data-ttu-id="c7b02-352">Tato změna byla provedena, protože rozhraní jsou koncepčně jednou.</span><span class="sxs-lookup"><span data-stu-id="c7b02-352">This change was made because the interfaces are conceptually one.</span></span>

<span data-ttu-id="c7b02-353">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="c7b02-353">**Mitigations**</span></span>

<span data-ttu-id="c7b02-354">Žádné implementace `IDbContextOptionsExtension` bude muset být aktualizované kvůli podpoře nového člena.</span><span class="sxs-lookup"><span data-stu-id="c7b02-354">Any implementations of `IDbContextOptionsExtension` will need to be updated to support the new member.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="c7b02-355">Opožděné načtení proxy už předpokládají, že jsou plně načteny navigační vlastnosti</span><span class="sxs-lookup"><span data-stu-id="c7b02-355">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="c7b02-356">Sledování problému #12780</span><span class="sxs-lookup"><span data-stu-id="c7b02-356">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="c7b02-357">Tato změna bude zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="c7b02-357">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="c7b02-358">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-358">**Old behavior**</span></span>

<span data-ttu-id="c7b02-359">Před EF Core 3.0, jednou `DbContext` byl odstraněn neexistoval způsob, jak zjistit, zda danou navigační vlastnost s entitou získané z daného kontextu byl plně načten či nikoli.</span><span class="sxs-lookup"><span data-stu-id="c7b02-359">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="c7b02-360">Proxy by místo toho se předpokládá, že je načtena navigační odkaz, pokud má nenulovou hodnotu a, že je načtena navigace kolekce, pokud není prázdná.</span><span class="sxs-lookup"><span data-stu-id="c7b02-360">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="c7b02-361">V těchto případech by pokus o opožděné načtení no-op.</span><span class="sxs-lookup"><span data-stu-id="c7b02-361">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="c7b02-362">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-362">**New behavior**</span></span>

<span data-ttu-id="c7b02-363">Od verze EF Core 3.0, proxy servery sledovat, jestli je načtena navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="c7b02-363">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="c7b02-364">To znamená, že při pokusu o přístup k navigační vlastnost, která je načtena po kontext se vyřadil. bude vždycky no-op, i v případě, že je načteno navigace je prázdný nebo mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="c7b02-364">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="c7b02-365">Naopak pokusu o přístup k navigační vlastnosti, které není načteno vyvolají výjimku pokud uvolnění kontextu, i když je navigační vlastnost kolekce není prázdná.</span><span class="sxs-lookup"><span data-stu-id="c7b02-365">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="c7b02-366">Pokud k této situaci dochází, znamená to kód aplikace se pokouší použít opožděné načtení na neplatný čas a aplikace by měla být změněna na tuto akci.</span><span class="sxs-lookup"><span data-stu-id="c7b02-366">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="c7b02-367">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c7b02-367">**Why**</span></span>

<span data-ttu-id="c7b02-368">Tato změna byla provedena, aby chování konzistentní a správné při pokusu o opožděné načtení na vyřazený `DbContext` instance.</span><span class="sxs-lookup"><span data-stu-id="c7b02-368">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="c7b02-369">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="c7b02-369">**Mitigations**</span></span>

<span data-ttu-id="c7b02-370">Kód aplikace, který nebude pokoušet opožděné načtení vyřazený kontextu, nebo nakonfigurovat to být no-op. jak je popsáno v zpráva o výjimce.</span><span class="sxs-lookup"><span data-stu-id="c7b02-370">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="c7b02-371">Nadměrné vytváření zprostředkovatelů vnitřní chybě služby je teď k chybě ve výchozím nastavení</span><span class="sxs-lookup"><span data-stu-id="c7b02-371">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="c7b02-372">Sledování problému #10236</span><span class="sxs-lookup"><span data-stu-id="c7b02-372">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="c7b02-373">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="c7b02-373">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="c7b02-374">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-374">**Old behavior**</span></span>

<span data-ttu-id="c7b02-375">Upozornění by před EF Core 3.0 zaznamenávané aplikace vytváří patologických několik poskytovatelů vnitřní chybě služby.</span><span class="sxs-lookup"><span data-stu-id="c7b02-375">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="c7b02-376">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-376">**New behavior**</span></span>

<span data-ttu-id="c7b02-377">Od verze EF Core 3.0, toto upozornění se teď považuje za a chyby a výjimky je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="c7b02-377">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="c7b02-378">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c7b02-378">**Why**</span></span>

<span data-ttu-id="c7b02-379">Tato změna byla provedena na připravovat lepší kód aplikace prostřednictvím vystavení takovém patologických více explicitně.</span><span class="sxs-lookup"><span data-stu-id="c7b02-379">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="c7b02-380">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="c7b02-380">**Mitigations**</span></span>

<span data-ttu-id="c7b02-381">Nejvhodnější příčinu akce při výskytu této chyby je zjistěte původní příčinu a nevytvářet mnoho poskytovatelů vnitřní chybě služby.</span><span class="sxs-lookup"><span data-stu-id="c7b02-381">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="c7b02-382">Však chyba může být převést zpět na upozornění (nebo ignorovat) prostřednictvím konfigurace na `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="c7b02-382">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="c7b02-383">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c7b02-383">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="c7b02-384">Nové chování pro HasOne/HasMany volat jeden řetězec</span><span class="sxs-lookup"><span data-stu-id="c7b02-384">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="c7b02-385">Sledování problému #9171</span><span class="sxs-lookup"><span data-stu-id="c7b02-385">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="c7b02-386">Tato změna bude zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="c7b02-386">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="c7b02-387">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-387">**Old behavior**</span></span>

<span data-ttu-id="c7b02-388">Před EF Core 3.0 kód volání `HasOne` nebo `HasMany` s jeden řetězec byl interpretovat matoucí způsobem.</span><span class="sxs-lookup"><span data-stu-id="c7b02-388">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpretted in a confusing way.</span></span>
<span data-ttu-id="c7b02-389">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c7b02-389">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="c7b02-390">Kód vypadá je týkající se `Samurai` některé jiné entity typu použití `Entrance` navigační vlastnost, která může být privátní.</span><span class="sxs-lookup"><span data-stu-id="c7b02-390">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="c7b02-391">Ve skutečnosti, tento kód se pokouší vytvořit relaci některé typ entity s názvem `Entrance` se žádné navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="c7b02-391">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="c7b02-392">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-392">**New behavior**</span></span>

<span data-ttu-id="c7b02-393">Od verze EF Core 3.0, výše uvedený kód nyní dělá co podívali jako její by měl mít byla činnosti před.</span><span class="sxs-lookup"><span data-stu-id="c7b02-393">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="c7b02-394">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c7b02-394">**Why**</span></span>

<span data-ttu-id="c7b02-395">Staré chování bylo velmi matoucí, zejména při čtení konfigurace kód a hledání chyb.</span><span class="sxs-lookup"><span data-stu-id="c7b02-395">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="c7b02-396">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="c7b02-396">**Mitigations**</span></span>

<span data-ttu-id="c7b02-397">Tímto přerušíte pouze aplikace, které jsou explicitně konfigurace relace používání řetězců pro názvy typů a bez explicitním zadáním navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="c7b02-397">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="c7b02-398">Není běžné.</span><span class="sxs-lookup"><span data-stu-id="c7b02-398">This is not common.</span></span>
<span data-ttu-id="c7b02-399">Předchozí chování můžete získat prostřednictvím explicitně předávání `null` pro název navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="c7b02-399">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="c7b02-400">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c7b02-400">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="c7b02-401">Poznámka relační: TypeMapping je teď stejně TypeMapping</span><span class="sxs-lookup"><span data-stu-id="c7b02-401">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="c7b02-402">Sledování problému #9913</span><span class="sxs-lookup"><span data-stu-id="c7b02-402">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="c7b02-403">Tato změna byla zavedená v EF Core 3.0 – náhled 2.</span><span class="sxs-lookup"><span data-stu-id="c7b02-403">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="c7b02-404">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-404">**Old behavior**</span></span>

<span data-ttu-id="c7b02-405">Název poznámky pro mapování anotace typu byl "Relační: TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="c7b02-405">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="c7b02-406">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-406">**New behavior**</span></span>

<span data-ttu-id="c7b02-407">Název poznámky pro mapování anotace typu je nyní "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="c7b02-407">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="c7b02-408">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c7b02-408">**Why**</span></span>

<span data-ttu-id="c7b02-409">Mapování typu jsou teď používá pro více než jen relační databáze poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="c7b02-409">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="c7b02-410">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="c7b02-410">**Mitigations**</span></span>

<span data-ttu-id="c7b02-411">Tímto přerušíte jenom aplikace s přístupem k mapování typů přímo jako poznámka, která není běžné.</span><span class="sxs-lookup"><span data-stu-id="c7b02-411">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="c7b02-412">Nejvhodnější opatření na opravu se má používat rovinu rozhraní API pro mapování typů přístupu spíše než přímo pomocí anotace.</span><span class="sxs-lookup"><span data-stu-id="c7b02-412">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="c7b02-413">Vyvolá výjimku, ToTable na odvozený typ.</span><span class="sxs-lookup"><span data-stu-id="c7b02-413">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="c7b02-414">Sledování problému #11811</span><span class="sxs-lookup"><span data-stu-id="c7b02-414">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="c7b02-415">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="c7b02-415">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="c7b02-416">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-416">**Old behavior**</span></span>

<span data-ttu-id="c7b02-417">Před EF Core 3.0 `ToTable()` volalo odvozený typ bude ignorovat, protože pouze dědičnosti mapování strategie byl TPH, kdy to není platný.</span><span class="sxs-lookup"><span data-stu-id="c7b02-417">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="c7b02-418">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-418">**New behavior**</span></span>

<span data-ttu-id="c7b02-419">Spouští se s EF Core 3.0 a při přípravě na přidání TPT a TPC podporují v pozdější verzi `ToTable()` volalo odvozený typ bude nyní vyvolání výjimky v budoucnu vyhnuli o změnu neočekávané mapování.</span><span class="sxs-lookup"><span data-stu-id="c7b02-419">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="c7b02-420">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c7b02-420">**Why**</span></span>

<span data-ttu-id="c7b02-421">Aktuálně není platná pro mapování odvozeného typu na jiné tabulky.</span><span class="sxs-lookup"><span data-stu-id="c7b02-421">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="c7b02-422">Tato změna se vyhnete přerušení v budoucnu, kdy bude platná věc udělat.</span><span class="sxs-lookup"><span data-stu-id="c7b02-422">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="c7b02-423">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="c7b02-423">**Mitigations**</span></span>

<span data-ttu-id="c7b02-424">Odeberte všechny pokusy o mapování odvozené typy s jinými tabulkami.</span><span class="sxs-lookup"><span data-stu-id="c7b02-424">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="c7b02-425">Nahradí HasIndex ForSqlServerHasIndex</span><span class="sxs-lookup"><span data-stu-id="c7b02-425">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="c7b02-426">Sledování problému #12366</span><span class="sxs-lookup"><span data-stu-id="c7b02-426">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="c7b02-427">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="c7b02-427">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="c7b02-428">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-428">**Old behavior**</span></span>

<span data-ttu-id="c7b02-429">Před EF Core 3.0 `ForSqlServerHasIndex().ForSqlServerInclude()` poskytuje způsob, jak konfigurovat sloupců použitých s `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="c7b02-429">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="c7b02-430">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-430">**New behavior**</span></span>

<span data-ttu-id="c7b02-431">Od verze EF Core 3.0, pomocí `Include` na indexu se teď podporuje relační úrovni.</span><span class="sxs-lookup"><span data-stu-id="c7b02-431">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="c7b02-432">Použití `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="c7b02-432">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="c7b02-433">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c7b02-433">**Why**</span></span>

<span data-ttu-id="c7b02-434">Tato změna byla provedena konsolidovat rozhraní API pro indexy s `Includes` do jednoho umístění pro všechny databáze poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="c7b02-434">This change was made to consolidate the API for indexes with `Includes` into one place for all database providers.</span></span>

<span data-ttu-id="c7b02-435">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="c7b02-435">**Mitigations**</span></span>

<span data-ttu-id="c7b02-436">Pomocí nového rozhraní API, jak je znázorněno výše.</span><span class="sxs-lookup"><span data-stu-id="c7b02-436">Use the new API, as shown above.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="c7b02-437">EF Core už odešle – Direktiva pragma pro vynucení SQLite FK</span><span class="sxs-lookup"><span data-stu-id="c7b02-437">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="c7b02-438">Sledování problému #12151</span><span class="sxs-lookup"><span data-stu-id="c7b02-438">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="c7b02-439">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="c7b02-439">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="c7b02-440">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-440">**Old behavior**</span></span>

<span data-ttu-id="c7b02-441">Před EF Core 3.0, bude posílat EF Core `PRAGMA foreign_keys = 1` při otevření připojení k SQLite.</span><span class="sxs-lookup"><span data-stu-id="c7b02-441">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="c7b02-442">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-442">**New behavior**</span></span>

<span data-ttu-id="c7b02-443">Od verze EF Core 3.0, už EF Core odešle `PRAGMA foreign_keys = 1` při otevření připojení k SQLite.</span><span class="sxs-lookup"><span data-stu-id="c7b02-443">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="c7b02-444">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c7b02-444">**Why**</span></span>

<span data-ttu-id="c7b02-445">Tato změna byla provedena, protože používá EF Core `SQLitePCLRaw.bundle_e_sqlite3` ve výchozím nastavení, která zase znamená, že vynucení cizího klíče je ve výchozím nastavení zapnuté a není potřeba explicitně povolit pokaždé, když je otevřeno připojení.</span><span class="sxs-lookup"><span data-stu-id="c7b02-445">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="c7b02-446">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="c7b02-446">**Mitigations**</span></span>

<span data-ttu-id="c7b02-447">Cizí klíče jsou povolena ve výchozím nastavení SQLitePCLRaw.bundle_e_sqlite3, který se používá ve výchozím nastavení pro jádro EF Core.</span><span class="sxs-lookup"><span data-stu-id="c7b02-447">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="c7b02-448">Pro jiných případech může být povoleno cizích klíčů tak, že zadáte `Foreign Keys=True` v připojovacím řetězci.</span><span class="sxs-lookup"><span data-stu-id="c7b02-448">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="c7b02-449">Microsoft.EntityFrameworkCore.Sqlite nyní závisí na SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="c7b02-449">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="c7b02-450">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-450">**Old behavior**</span></span>

<span data-ttu-id="c7b02-451">Před EF Core 3.0 používá EF Core `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="c7b02-451">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="c7b02-452">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-452">**New behavior**</span></span>

<span data-ttu-id="c7b02-453">Od verze EF Core 3.0, pomocí EF Core `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="c7b02-453">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="c7b02-454">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c7b02-454">**Why**</span></span>

<span data-ttu-id="c7b02-455">Tato změna byla provedena tak, aby používalo verzi SQLite v Iosu konzistentní s jinými platformami.</span><span class="sxs-lookup"><span data-stu-id="c7b02-455">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="c7b02-456">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="c7b02-456">**Mitigations**</span></span>

<span data-ttu-id="c7b02-457">Pokud chcete použít nativní verzi SQLite v Iosu, nakonfigurovat `Microsoft.Data.Sqlite` použít jinou `SQLitePCLRaw` sady.</span><span class="sxs-lookup"><span data-stu-id="c7b02-457">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

## <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="c7b02-458">Identifikátor GUID hodnoty jsou nyní uloženy jako TEXT na SQLite</span><span class="sxs-lookup"><span data-stu-id="c7b02-458">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="c7b02-459">Sledování problému #15078</span><span class="sxs-lookup"><span data-stu-id="c7b02-459">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="c7b02-460">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="c7b02-460">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="c7b02-461">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-461">**Old behavior**</span></span>

<span data-ttu-id="c7b02-462">Identifikátor GUID hodnoty byly dříve sored jako hodnoty objektu BLOB na SQLite.</span><span class="sxs-lookup"><span data-stu-id="c7b02-462">Guid values were previously sored as BLOB values on SQLite.</span></span>

<span data-ttu-id="c7b02-463">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-463">**New behavior**</span></span>

<span data-ttu-id="c7b02-464">Identifikátor GUID hodnoty jsou nyní sotred jako TEXT.</span><span class="sxs-lookup"><span data-stu-id="c7b02-464">Guid values are now sotred as TEXT.</span></span>

<span data-ttu-id="c7b02-465">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c7b02-465">**Why**</span></span>

<span data-ttu-id="c7b02-466">Binární formát GUID není standardizované.</span><span class="sxs-lookup"><span data-stu-id="c7b02-466">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="c7b02-467">Uložení hodnot jako TEXT díky databáze více kompatibilní s jinými technologiemi.</span><span class="sxs-lookup"><span data-stu-id="c7b02-467">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="c7b02-468">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="c7b02-468">**Mitigations**</span></span>

<span data-ttu-id="c7b02-469">Spuštěním SQL takto můžete migrovat existující databáze na nový formát.</span><span class="sxs-lookup"><span data-stu-id="c7b02-469">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET GuidColumn = hex(substr(GuidColumn, 4, 1)) ||
                 hex(substr(GuidColumn, 3, 1)) ||
                 hex(substr(GuidColumn, 2, 1)) ||
                 hex(substr(GuidColumn, 1, 1)) || '-' ||
                 hex(substr(GuidColumn, 6, 1)) ||
                 hex(substr(GuidColumn, 5, 1)) || '-' ||
                 hex(substr(GuidColumn, 8, 1)) ||
                 hex(substr(GuidColumn, 7, 1)) || '-' ||
                 hex(substr(GuidColumn, 9, 2)) || '-' ||
                 hex(substr(GuidColumn, 11, 6))
WHERE typeof(GuidColumn) == 'blob';
```

<span data-ttu-id="c7b02-470">V EF Core můžete také pokračovat pomocí předchozí chování configuirng převaděč hodnoty těchto vlastností.</span><span class="sxs-lookup"><span data-stu-id="c7b02-470">In EF Core, you could also continue using the previous behavior by configuirng a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="c7b02-471">Microsoft.Data.Sqlite zůstává schopný načíst hodnoty identifikátoru Guid z objektu BLOB a TEXTOVÉHO sloupce; ale vzhledem k tomu, že došlo ke změně výchozího formátu pro parametry a konstant bude pravděpodobně potřeba provést akci pro většinu scénářů zahrnující identifikátory GUID.</span><span class="sxs-lookup"><span data-stu-id="c7b02-471">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="c7b02-472">Hodnoty char jsou nyní uloženy jako TEXT na SQLite</span><span class="sxs-lookup"><span data-stu-id="c7b02-472">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="c7b02-473">Sledování problému #15020</span><span class="sxs-lookup"><span data-stu-id="c7b02-473">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="c7b02-474">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="c7b02-474">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="c7b02-475">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-475">**Old behavior**</span></span>

<span data-ttu-id="c7b02-476">Hodnoty char byly dříve sored jako CELOČÍSELNÉ hodnoty na SQLite.</span><span class="sxs-lookup"><span data-stu-id="c7b02-476">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="c7b02-477">Například znak hodnotu *A* byl uložen jako celočíselnou hodnotu 65.</span><span class="sxs-lookup"><span data-stu-id="c7b02-477">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="c7b02-478">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-478">**New behavior**</span></span>

<span data-ttu-id="c7b02-479">Hodnoty char jsou nyní sotred jako TEXT.</span><span class="sxs-lookup"><span data-stu-id="c7b02-479">Char values are now sotred as TEXT.</span></span>

<span data-ttu-id="c7b02-480">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c7b02-480">**Why**</span></span>

<span data-ttu-id="c7b02-481">Uložení hodnot jako TEXT je přirozenější a vytvoří databáze více kompatibilní s jinými technologiemi.</span><span class="sxs-lookup"><span data-stu-id="c7b02-481">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="c7b02-482">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="c7b02-482">**Mitigations**</span></span>

<span data-ttu-id="c7b02-483">Spuštěním SQL takto můžete migrovat existující databáze na nový formát.</span><span class="sxs-lookup"><span data-stu-id="c7b02-483">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="c7b02-484">V EF Core můžete také pokračovat pomocí předchozí chování configuirng převaděč hodnoty těchto vlastností.</span><span class="sxs-lookup"><span data-stu-id="c7b02-484">In EF Core, you could also continue using the previous behavior by configuirng a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="c7b02-485">Microsoft.Data.Sqlite také zbývá schopný načíst znakových hodnot z celé číslo a TEXT sloupců, takže určitých scénářích nevyžadují žádnou akci.</span><span class="sxs-lookup"><span data-stu-id="c7b02-485">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="c7b02-486">ID migrace jsou generovány pomocí neutrální jazykové verze kalendáře</span><span class="sxs-lookup"><span data-stu-id="c7b02-486">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="c7b02-487">Sledování problému #12978</span><span class="sxs-lookup"><span data-stu-id="c7b02-487">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="c7b02-488">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="c7b02-488">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="c7b02-489">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-489">**Old behavior**</span></span>

<span data-ttu-id="c7b02-490">ID migrace byly generovány pomocí kalendář jazykové verze currret neúmyslně.</span><span class="sxs-lookup"><span data-stu-id="c7b02-490">Migration IDs were inadvertantly generated using the currret culture's calendar.</span></span>

<span data-ttu-id="c7b02-491">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-491">**New behavior**</span></span>

<span data-ttu-id="c7b02-492">ID migrace jsou teď vždy generovány pomocí neutrální jazykové verze kalendáře (gregoriánského).</span><span class="sxs-lookup"><span data-stu-id="c7b02-492">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="c7b02-493">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c7b02-493">**Why**</span></span>

<span data-ttu-id="c7b02-494">Pořadí migrace je důležité při aktualizaci databáze nebo řešení konfliktů při sloučení.</span><span class="sxs-lookup"><span data-stu-id="c7b02-494">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="c7b02-495">Pomocí neutrální kalendáře se vyhnete řazení problémy, které můžou být výsledkem členy týmu s jiným kalendáře.</span><span class="sxs-lookup"><span data-stu-id="c7b02-495">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="c7b02-496">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="c7b02-496">**Mitigations**</span></span>

<span data-ttu-id="c7b02-497">Tato změna ovlivní těm, kdo používají jiné než gregoriánské kalendářní, kde rok je větší než gregoriánském kalendáři (např. thajský buddhistický kalendář).</span><span class="sxs-lookup"><span data-stu-id="c7b02-497">This change affects anyone using a non-Gregorian calender where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="c7b02-498">Migrace stávající ID bude potřeba aktualizovat tak, aby nové migrace jsou řazeny za stávající migrace.</span><span class="sxs-lookup"><span data-stu-id="c7b02-498">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="c7b02-499">ID migrace najdete v atributu migrace soubory návrháře migrace.</span><span class="sxs-lookup"><span data-stu-id="c7b02-499">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="c7b02-500">Tabulky historie migrace je také potřeba aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="c7b02-500">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

## <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="c7b02-501">LogQueryPossibleExceptionWithAggregateOperator byl přejmenován.</span><span class="sxs-lookup"><span data-stu-id="c7b02-501">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="c7b02-502">Sledování problému #10985</span><span class="sxs-lookup"><span data-stu-id="c7b02-502">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="c7b02-503">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="c7b02-503">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="c7b02-504">**Změna**</span><span class="sxs-lookup"><span data-stu-id="c7b02-504">**Change**</span></span>

<span data-ttu-id="c7b02-505">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` byl přejmenován na `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="c7b02-505">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="c7b02-506">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c7b02-506">**Why**</span></span>

<span data-ttu-id="c7b02-507">Zarovná pojmenování Tato událost upozornění s jinými událostmi upozornění.</span><span class="sxs-lookup"><span data-stu-id="c7b02-507">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="c7b02-508">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="c7b02-508">**Mitigations**</span></span>

<span data-ttu-id="c7b02-509">Použití nového názvu.</span><span class="sxs-lookup"><span data-stu-id="c7b02-509">Use the new name.</span></span> <span data-ttu-id="c7b02-510">(Všimněte si, že nedošlo ke změně číslo ID události.)</span><span class="sxs-lookup"><span data-stu-id="c7b02-510">(Note that the event ID number has not changed.)</span></span>

## <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="c7b02-511">Vysvětlení rozhraní API pro názvy omezení pro cizí klíč</span><span class="sxs-lookup"><span data-stu-id="c7b02-511">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="c7b02-512">Sledování problému #10730</span><span class="sxs-lookup"><span data-stu-id="c7b02-512">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="c7b02-513">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="c7b02-513">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="c7b02-514">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-514">**Old behavior**</span></span>

<span data-ttu-id="c7b02-515">Před EF Core 3.0 omezení pro cizí klíč názvy označovaly jako jednoduše "name".</span><span class="sxs-lookup"><span data-stu-id="c7b02-515">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="c7b02-516">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c7b02-516">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="c7b02-517">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c7b02-517">**New behavior**</span></span>

<span data-ttu-id="c7b02-518">Od verze EF Core 3.0, omezení pro cizí klíč názvy jsou dnes označovány jako "kruhového name".</span><span class="sxs-lookup"><span data-stu-id="c7b02-518">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constaint name".</span></span> <span data-ttu-id="c7b02-519">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c7b02-519">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="c7b02-520">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c7b02-520">**Why**</span></span>

<span data-ttu-id="c7b02-521">Tato změna přináší konzistenci pro názvy v této oblasti a také vysvětluje, že se jedná o název název cizího klíče kruhového a nikoli na sloupec nebo vlastnost, která je definována cizího klíče na.</span><span class="sxs-lookup"><span data-stu-id="c7b02-521">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constaint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="c7b02-522">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="c7b02-522">**Mitigations**</span></span>

<span data-ttu-id="c7b02-523">Použití nového názvu.</span><span class="sxs-lookup"><span data-stu-id="c7b02-523">Use the new name.</span></span>
