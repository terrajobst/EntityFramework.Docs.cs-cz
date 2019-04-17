---
title: Zásadní změny v EF Core 3.0 – EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 4b251638de43af6525f3e6faa0bd4113ab1714b9
ms.sourcegitcommit: 5280dcac4423acad8b440143433459b18886115b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/16/2019
ms.locfileid: "59619256"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="1c897-102">Rozbíjející změny zahrnuté v EF Core 3.0 (aktuálně ve verzi preview)</span><span class="sxs-lookup"><span data-stu-id="1c897-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1c897-103">Mějte prosím na paměti, že sady funkcí a plány budoucích verzí se vždy mohou změnit a přestože se snažíme se zachovat aktuální, nemusí odrážet naše nejnovější plány vůbec na této stránce vyprší.</span><span class="sxs-lookup"><span data-stu-id="1c897-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="1c897-104">Následující změny chování a rozhraní API by mohly přerušit aplikace vyvinuté pro jádro EF Core 2.2.x při upgradu na 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="1c897-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="1c897-105">Změny, které chceme ovlivní jen poskytovatelé databází jsou popsány v části [poskytovatel změny](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="1c897-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="1c897-106">Tady nejsou uvedené konce v nové vlastnosti představené z jednoho 3.0 ve verzi preview další 3.0 ve verzi Preview.</span><span class="sxs-lookup"><span data-stu-id="1c897-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="1c897-107">Dotazy LINQ se už nevyhodnocuje na straně klienta</span><span class="sxs-lookup"><span data-stu-id="1c897-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="1c897-108">[Sledování problému #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[také zjistit problém #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="1c897-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="1c897-109">Tato změna bude zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="1c897-109">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1c897-110">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-110">**Old behavior**</span></span>

<span data-ttu-id="1c897-111">Než 3.0 když EF Core nelze převést výraz, který byl součástí sady dotazů SQL nebo parametr, automaticky vyhodnotí výraz na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="1c897-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="1c897-112">Ve výchozím nastavení hodnocení klientů potencionálně nákladným výrazů aktivuje pouze upozornění.</span><span class="sxs-lookup"><span data-stu-id="1c897-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="1c897-113">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-113">**New behavior**</span></span>

<span data-ttu-id="1c897-114">Od verze 3.0, EF Core umožňuje pouze výrazy v projekci nejvyšší úrovně (poslední `Select()` volání v dotazu) k vyhodnocení na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="1c897-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="1c897-115">Když výrazy v další části dotazu nelze převést na SQL nebo parametr, je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="1c897-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="1c897-116">**Proč**</span><span class="sxs-lookup"><span data-stu-id="1c897-116">**Why**</span></span>

<span data-ttu-id="1c897-117">Vyhodnocení automatickou klientskou dotazů umožňuje mnoho dotazů, který se spustí i v případě, že je důležité části nelze přeložit.</span><span class="sxs-lookup"><span data-stu-id="1c897-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="1c897-118">Toto chování může způsobit neočekávané a potenciálně škodlivé chování, které může přestat pouze zřejmé v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="1c897-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="1c897-119">Například podmínku v `Where()` volání, které nelze přeložit může způsobit, že všechny řádky z tabulky přesunou z databázového serveru a filtr, který bude použit na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="1c897-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="1c897-120">Tato situace může pokračovat nezjištěné po snadno Pokud tabulka obsahuje jenom pár řádků v vývoje, ale přístupů pevné přesun aplikace do produkčního prostředí, kde tabulka může obsahovat miliony řádků.</span><span class="sxs-lookup"><span data-stu-id="1c897-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="1c897-121">Upozornění vyhodnocení klienta dokázaly také příliš jednoduché ignorovat během vývoje.</span><span class="sxs-lookup"><span data-stu-id="1c897-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="1c897-122">Kromě toho může automatickou klientskou vyhodnocení způsobit problémy, ve kterých vylepšení překladu dotazu pro konkrétní výrazy způsobila neúmyslnému zásadní změny mezi verzí.</span><span class="sxs-lookup"><span data-stu-id="1c897-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="1c897-123">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="1c897-123">**Mitigations**</span></span>

<span data-ttu-id="1c897-124">Pokud dotaz nelze přeložit plně, pak buď přepište dotaz ve formuláři, který lze přeložit, nebo použijte `AsEnumerable()`, `ToList()`, nebo podobný explicitně přenést data zpět do klienta ve kterém pak může být dále zpracovány pomocí LINQ na objekty.</span><span class="sxs-lookup"><span data-stu-id="1c897-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="1c897-125">Entity Framework Core už nejsou součástí sdíleného rozhraní ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1c897-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="1c897-126">Oznámení týkající se sledování problému #325</span><span class="sxs-lookup"><span data-stu-id="1c897-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="1c897-127">Tato změna byla zavedena v ASP.NET Core 3.0 ve verzi preview 1.</span><span class="sxs-lookup"><span data-stu-id="1c897-127">This change was introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="1c897-128">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-128">**Old behavior**</span></span>

<span data-ttu-id="1c897-129">Před ASP.NET Core 3.0, když se přidá odkaz na balíček pro `Microsoft.AspNetCore.App` nebo `Microsoft.AspNetCore.All`, měl by obsahovat EF Core a některé EF Core zprostředkovatele dat, jako jsou zprostředkovatele SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1c897-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="1c897-130">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-130">**New behavior**</span></span>

<span data-ttu-id="1c897-131">Od verze 3.0, rozhraní ASP.NET Core sdílené neobsahuje EF Core nebo žádným zprostředkovatelům dat. EF Core.</span><span class="sxs-lookup"><span data-stu-id="1c897-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="1c897-132">**Proč**</span><span class="sxs-lookup"><span data-stu-id="1c897-132">**Why**</span></span>

<span data-ttu-id="1c897-133">Před touto změnou získávání EF Core vyžaduje jiný postup v závislosti na tom, jestli aplikace cílené ASP.NET Core a SQL Server, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="1c897-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="1c897-134">Upgrade ASP.NET Core také vynutit upgrade EF Core a zprostředkovatele SQL Server, který nemusí být vždy žádoucí.</span><span class="sxs-lookup"><span data-stu-id="1c897-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="1c897-135">Díky této změně se možnosti načítání EF Core je stejná ve všech zprostředkovatelů, podporovaná implementace .NET a typy aplikací.</span><span class="sxs-lookup"><span data-stu-id="1c897-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="1c897-136">Vývojáři také nyní mohou ovládat přesně při upgradu EF Core a EF Core zprostředkovatelé dat.</span><span class="sxs-lookup"><span data-stu-id="1c897-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="1c897-137">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="1c897-137">**Mitigations**</span></span>

<span data-ttu-id="1c897-138">V aplikaci ASP.NET Core 3.0 nebo jakékoli jiné podporované aplikace použít EF Core, explicitně přidáte odkaz na balíček k poskytovateli databáze EF Core, který bude aplikace používat.</span><span class="sxs-lookup"><span data-stu-id="1c897-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="1c897-139">Byla přejmenovaná FromSql ExecuteSql a ExecuteSqlAsync</span><span class="sxs-lookup"><span data-stu-id="1c897-139">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="1c897-140">Sledování problému #10996</span><span class="sxs-lookup"><span data-stu-id="1c897-140">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="1c897-141">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="1c897-141">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1c897-142">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-142">**Old behavior**</span></span>

<span data-ttu-id="1c897-143">Před EF Core 3.0 byly tyto názvy metod přetížení pro práci s normální řetězec nebo řetězec, který by měl být interpolovaných do SQL a parametry.</span><span class="sxs-lookup"><span data-stu-id="1c897-143">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="1c897-144">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-144">**New behavior**</span></span>

<span data-ttu-id="1c897-145">Od verze EF Core 3.0, použijte `FromSqlRaw`, `ExecuteSqlRaw`, a `ExecuteSqlRawAsync` vytvořit parametrický dotaz, kde parametry jsou předány samostatně z řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="1c897-145">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="1c897-146">Příklad:</span><span class="sxs-lookup"><span data-stu-id="1c897-146">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="1c897-147">Použití `FromSqlInterpolated`, `ExecuteSqlInterpolated`, a `ExecuteSqlInterpolatedAsync` vytvořit parametrický dotaz, kde parametry jsou předány jako součást dotazu interpolované řetězce.</span><span class="sxs-lookup"><span data-stu-id="1c897-147">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="1c897-148">Příklad:</span><span class="sxs-lookup"><span data-stu-id="1c897-148">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="1c897-149">Všimněte si, že oba dotazy výše uvedené vytvoří stejný parametrizovaného dotazu SQL se stejnými parametry SQL.</span><span class="sxs-lookup"><span data-stu-id="1c897-149">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="1c897-150">**Proč**</span><span class="sxs-lookup"><span data-stu-id="1c897-150">**Why**</span></span>

<span data-ttu-id="1c897-151">Přetížení metody, jako je to velmi usnadňují omylem nezpracovaná srting metodu volat, pokud bylo záměrem pro volání metody interpolovaný řetězec a naopak.</span><span class="sxs-lookup"><span data-stu-id="1c897-151">Method overloads like this make it very easy to accidentally call the raw srting method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="1c897-152">To může vést k dotazům naprostou když měla být parametrizovány.</span><span class="sxs-lookup"><span data-stu-id="1c897-152">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="1c897-153">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="1c897-153">**Mitigations**</span></span>

<span data-ttu-id="1c897-154">Přepněte na nové názvy metod.</span><span class="sxs-lookup"><span data-stu-id="1c897-154">Switch to use the new method names.</span></span>

## <a name="query-execution-is-logged-at-debug-level"></a><span data-ttu-id="1c897-155">Provádění dotazu se protokoluje při ladění na úrovni</span><span class="sxs-lookup"><span data-stu-id="1c897-155">Query execution is logged at Debug level</span></span>

[<span data-ttu-id="1c897-156">Sledování problému #14523</span><span class="sxs-lookup"><span data-stu-id="1c897-156">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="1c897-157">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="1c897-157">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1c897-158">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-158">**Old behavior**</span></span>

<span data-ttu-id="1c897-159">Před EF Core 3.0, provádění dotazů a jiných příkazů protokolu byla zaznamenána v `Info` úroveň.</span><span class="sxs-lookup"><span data-stu-id="1c897-159">Before EF Core 3.0, execution of queries and other commands was logged at the `Info` level.</span></span>

<span data-ttu-id="1c897-160">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-160">**New behavior**</span></span>

<span data-ttu-id="1c897-161">Od verze EF Core 3.0, protokolování spuštění příkazu/SQL je na `Debug` úroveň.</span><span class="sxs-lookup"><span data-stu-id="1c897-161">Starting with EF Core 3.0, logging of command/SQL execution is at the `Debug` level.</span></span>

<span data-ttu-id="1c897-162">**Proč**</span><span class="sxs-lookup"><span data-stu-id="1c897-162">**Why**</span></span>

<span data-ttu-id="1c897-163">Tato změna byla provedena jak snížit šum na `Info` úrovně protokolování.</span><span class="sxs-lookup"><span data-stu-id="1c897-163">This change was made to reduce the noise at the `Info` log level.</span></span>

<span data-ttu-id="1c897-164">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="1c897-164">**Mitigations**</span></span>

<span data-ttu-id="1c897-165">Tato událost protokolování je definována `RelationalEventId.CommandExecuting` s ID události 20100.</span><span class="sxs-lookup"><span data-stu-id="1c897-165">This logging event is defined by `RelationalEventId.CommandExecuting` with event ID 20100.</span></span>
<span data-ttu-id="1c897-166">Do protokolu SQL na `Info` úroveň znovu, explicitně nakonfigurovat na úrovni `OnConfiguring` nebo `AddDbContext`.</span><span class="sxs-lookup"><span data-stu-id="1c897-166">To log SQL at the `Info` level again, explicitly configure the level in `OnConfiguring` or `AddDbContext`.</span></span>
<span data-ttu-id="1c897-167">Příklad:</span><span class="sxs-lookup"><span data-stu-id="1c897-167">For example:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Info)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="1c897-168">Dočasné hodnoty klíče už nejsou nastavené na instancí entit</span><span class="sxs-lookup"><span data-stu-id="1c897-168">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="1c897-169">Sledování problému #12378</span><span class="sxs-lookup"><span data-stu-id="1c897-169">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="1c897-170">Tato změna byla zavedená v EF Core 3.0 – náhled 2.</span><span class="sxs-lookup"><span data-stu-id="1c897-170">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="1c897-171">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-171">**Old behavior**</span></span>

<span data-ttu-id="1c897-172">Před EF Core 3.0 dočasné hodnoty přiřadily se všechny vlastnosti klíče, které by později skutečné hodnoty generován databází.</span><span class="sxs-lookup"><span data-stu-id="1c897-172">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="1c897-173">Obvykle tyto dočasné hodnoty byly velké záporná čísla.</span><span class="sxs-lookup"><span data-stu-id="1c897-173">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="1c897-174">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-174">**New behavior**</span></span>

<span data-ttu-id="1c897-175">Od verze 3.0, EF Core ukládá dočasné hodnotu klíče jako součást informací o sledování entity a ponechá samotné beze změny vlastnosti klíče.</span><span class="sxs-lookup"><span data-stu-id="1c897-175">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="1c897-176">**Proč**</span><span class="sxs-lookup"><span data-stu-id="1c897-176">**Why**</span></span>

<span data-ttu-id="1c897-177">Tato změna byla provedena zabránit chybně stávají trvalé při entita, která byla dříve sledován pomocí funkce některé dočasné hodnoty klíče `DbContext` instance se přesune na jiný `DbContext` instance.</span><span class="sxs-lookup"><span data-stu-id="1c897-177">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="1c897-178">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="1c897-178">**Mitigations**</span></span>

<span data-ttu-id="1c897-179">Na staré chování může záviset aplikace, které přiřazují hodnoty primárního klíče na cizí klíče formuláře přidružení mezi entitami, pokud primární klíče generované úložištěm a patří do entity v `Added` stavu.</span><span class="sxs-lookup"><span data-stu-id="1c897-179">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="1c897-180">Toho se lze vyvarovat podle:</span><span class="sxs-lookup"><span data-stu-id="1c897-180">This can be avoided by:</span></span>
* <span data-ttu-id="1c897-181">Bez použití klíče generované úložištěm.</span><span class="sxs-lookup"><span data-stu-id="1c897-181">Not using store-generated keys.</span></span>
* <span data-ttu-id="1c897-182">Nastavení vlastnosti navigace na vztahů namísto nastavování hodnot cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="1c897-182">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="1c897-183">Získejte skutečné hodnoty dočasné klíče z informací o sledování entity.</span><span class="sxs-lookup"><span data-stu-id="1c897-183">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="1c897-184">Například `context.Entry(blog).Property(e => e.Id).CurrentValue` i v případě vrátí hodnotu dočasné `blog.Id` samotný nebyla nastavena.</span><span class="sxs-lookup"><span data-stu-id="1c897-184">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="1c897-185">Metoda DetectChanges respektuje klíčových hodnot generovaných úložištěm</span><span class="sxs-lookup"><span data-stu-id="1c897-185">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="1c897-186">Sledování problému #14616</span><span class="sxs-lookup"><span data-stu-id="1c897-186">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="1c897-187">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="1c897-187">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1c897-188">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-188">**Old behavior**</span></span>

<span data-ttu-id="1c897-189">Před EF Core 3.0 Nesledované entity objevila `DetectChanges` mají sledovat v `Added` stavu a vložené jako nový řádek, kdy `SaveChanges` je volána.</span><span class="sxs-lookup"><span data-stu-id="1c897-189">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="1c897-190">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-190">**New behavior**</span></span>

<span data-ttu-id="1c897-191">Od verze EF Core 3.0, pokud používá entity generované hodnoty klíče a některá z hodnot klíče je nastavena, pak entity se bude sledovat v `Modified` stavu.</span><span class="sxs-lookup"><span data-stu-id="1c897-191">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="1c897-192">To znamená, že řádek entity se předpokládá, že existují a budou aktualizace `SaveChanges` je volána.</span><span class="sxs-lookup"><span data-stu-id="1c897-192">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="1c897-193">Pokud není nastavena hodnota klíče, nebo pokud není typ entity pomocí vygenerované klíče, pak nová entita se bude dál sledovat jako `Added` stejně jako v předchozích verzích.</span><span class="sxs-lookup"><span data-stu-id="1c897-193">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="1c897-194">**Proč**</span><span class="sxs-lookup"><span data-stu-id="1c897-194">**Why**</span></span>

<span data-ttu-id="1c897-195">Tato změna byla provedena na umožňují snadněji a konzistentnější pro práci s grafy odpojené entity při použití klíče generované úložištěm.</span><span class="sxs-lookup"><span data-stu-id="1c897-195">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="1c897-196">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="1c897-196">**Mitigations**</span></span>

<span data-ttu-id="1c897-197">Tato změna může přerušit aplikace, pokud typ entity je nakonfigurovaný na použití vygenerovat klíče, ale hodnoty klíče jsou explicitně nastavené pro nové instance.</span><span class="sxs-lookup"><span data-stu-id="1c897-197">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="1c897-198">Oprava je explicitně konfigurovat klíčové vlastnosti nejsou generovány pomocí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="1c897-198">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="1c897-199">Například pomocí rozhraní fluent API:</span><span class="sxs-lookup"><span data-stu-id="1c897-199">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="1c897-200">Nebo s anotacemi dat:</span><span class="sxs-lookup"><span data-stu-id="1c897-200">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="1c897-201">Kaskádové odstranění teď dojde okamžitě ve výchozím nastavení</span><span class="sxs-lookup"><span data-stu-id="1c897-201">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="1c897-202">Sledování problému #10114</span><span class="sxs-lookup"><span data-stu-id="1c897-202">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="1c897-203">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="1c897-203">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1c897-204">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-204">**Old behavior**</span></span>

<span data-ttu-id="1c897-205">Než 3.0 EF Core použít kaskádové akce (odstranění závislých položek při odstranění požadovaný instanční objekt nebo když porušeno vztah k požadované instanční objekt) nebyly provedeny dokud byla volána metoda SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="1c897-205">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="1c897-206">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-206">**New behavior**</span></span>

<span data-ttu-id="1c897-207">Od verze 3.0, EF Core provede kaskádové akce ihned po zjištění spouštěcí podmínky.</span><span class="sxs-lookup"><span data-stu-id="1c897-207">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="1c897-208">Například volání `context.Remove()` odstranit instančního objektu entity se výsledek ve všech sledovat související požadované položky závislé na také nastavena na `Deleted` okamžitě.</span><span class="sxs-lookup"><span data-stu-id="1c897-208">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="1c897-209">**Proč**</span><span class="sxs-lookup"><span data-stu-id="1c897-209">**Why**</span></span>

<span data-ttu-id="1c897-210">Tato změna byla provedena na zlepšení uživatelského rozhraní pro datových vazbách a auditování scénáře, kdy je potřeba vysvětlit entit, které se odstraní _před_ `SaveChanges` je volána.</span><span class="sxs-lookup"><span data-stu-id="1c897-210">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="1c897-211">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="1c897-211">**Mitigations**</span></span>

<span data-ttu-id="1c897-212">Předchozí chování lze obnovit nastavením na `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="1c897-212">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="1c897-213">Příklad:</span><span class="sxs-lookup"><span data-stu-id="1c897-213">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="1c897-214">DeleteBehavior.Restrict má sémantiku čisticího modulu</span><span class="sxs-lookup"><span data-stu-id="1c897-214">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="1c897-215">Sledování problému #12661</span><span class="sxs-lookup"><span data-stu-id="1c897-215">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="1c897-216">Tato změna bude zavedená v EF Core 3.0 – ve verzi preview 5.</span><span class="sxs-lookup"><span data-stu-id="1c897-216">This change will be introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="1c897-217">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-217">**Old behavior**</span></span>

<span data-ttu-id="1c897-218">Než 3.0 `DeleteBehavior.Restrict` vytvořit cizí klíče v databázi s `Restrict` sémantiku, ale také změněné interní opravy není zřejmé způsobem.</span><span class="sxs-lookup"><span data-stu-id="1c897-218">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="1c897-219">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-219">**New behavior**</span></span>

<span data-ttu-id="1c897-220">Od verze 3.0, `DeleteBehavior.Restrict` zajistí, že cizí klíče jsou vytvořeny pomocí `Restrict` sémantiku – tedy žádná cascades; throw na porušení omezení – bez vliv i na EF interní opravy.</span><span class="sxs-lookup"><span data-stu-id="1c897-220">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="1c897-221">**Proč**</span><span class="sxs-lookup"><span data-stu-id="1c897-221">**Why**</span></span>

<span data-ttu-id="1c897-222">Tato změna byla provedena na zlepšení uživatelského rozhraní pro použití `DeleteBehavior` intuitivní způsobem, bez neočekávané vedlejší účinky.</span><span class="sxs-lookup"><span data-stu-id="1c897-222">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="1c897-223">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="1c897-223">**Mitigations**</span></span>

<span data-ttu-id="1c897-224">Předchozí chování můžete obnovit pomocí `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="1c897-224">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="1c897-225">Typy dotazů jsou spojeny s typy entit</span><span class="sxs-lookup"><span data-stu-id="1c897-225">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="1c897-226">Sledování problému #14194</span><span class="sxs-lookup"><span data-stu-id="1c897-226">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="1c897-227">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="1c897-227">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1c897-228">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-228">**Old behavior**</span></span>

<span data-ttu-id="1c897-229">Před EF Core 3.0 [typy dotazů](xref:core/modeling/query-types) byly prostředky provádět dotazy na data, která nedefinuje primární klíč strukturovaným způsobem.</span><span class="sxs-lookup"><span data-stu-id="1c897-229">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="1c897-230">To znamená typ dotazu byla použita pro mapování typů entit bez klíčů (pravděpodobně ze zobrazení, ale pravděpodobně z tabulky) při pravidelných entity typ byl použit při klíč byl k dispozici (více pravděpodobně z tabulky, ale pravděpodobně ze zobrazení).</span><span class="sxs-lookup"><span data-stu-id="1c897-230">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="1c897-231">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-231">**New behavior**</span></span>

<span data-ttu-id="1c897-232">Typ dotazu teď bude pouze typ entity bez primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="1c897-232">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="1c897-233">Typy entit bez kódu mají stejné funkce jako typy dotazů v předchozích verzích.</span><span class="sxs-lookup"><span data-stu-id="1c897-233">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="1c897-234">**Proč**</span><span class="sxs-lookup"><span data-stu-id="1c897-234">**Why**</span></span>

<span data-ttu-id="1c897-235">Tato změna byla provedena, abyste snížili nejasnosti kolem účel typy dotazů.</span><span class="sxs-lookup"><span data-stu-id="1c897-235">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="1c897-236">Konkrétně jsou typy bez klíčů entit a z tohoto důvodu jsou ze své podstaty jen pro čtení, ale by neměl být použit pouze z důvodu typu entity musí být jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="1c897-236">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="1c897-237">Podobně jsou často namapované na zobrazení, ale je to jenom, protože zobrazení není často definují klíče.</span><span class="sxs-lookup"><span data-stu-id="1c897-237">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="1c897-238">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="1c897-238">**Mitigations**</span></span>

<span data-ttu-id="1c897-239">Následující části rozhraní API jsou teď zastaralé:</span><span class="sxs-lookup"><span data-stu-id="1c897-239">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="1c897-240">**`ModelBuilder.Query<>()`** – Místo toho `ModelBuilder.Entity<>().HasNoKey()` musí být volána k označení typu entity tak, že má žádné klíče.</span><span class="sxs-lookup"><span data-stu-id="1c897-240">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="1c897-241">To by stále probíhá konfigurace podle konvence se vyhnete chybné konfigurace, když primární klíč se očekává, ale neodpovídá konvenci.</span><span class="sxs-lookup"><span data-stu-id="1c897-241">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="1c897-242">**`DbQuery<>`** – Místo toho `DbSet<>` by měla sloužit.</span><span class="sxs-lookup"><span data-stu-id="1c897-242">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="1c897-243">**`DbContext.Query<>()`** – Místo toho `DbContext.Set<>()` by měla sloužit.</span><span class="sxs-lookup"><span data-stu-id="1c897-243">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="1c897-244">Došlo ke změně konfigurace rozhraní API pro vlastní typ relace</span><span class="sxs-lookup"><span data-stu-id="1c897-244">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="1c897-245">[Sledování problému #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[sledování problému #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[sledování problému #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="1c897-245">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="1c897-246">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="1c897-246">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1c897-247">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-247">**Old behavior**</span></span>

<span data-ttu-id="1c897-248">Před EF Core 3.0 byla provedena konfigurace vlastněné vztah přímo po `OwnsOne` nebo `OwnsMany` volání.</span><span class="sxs-lookup"><span data-stu-id="1c897-248">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="1c897-249">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-249">**New behavior**</span></span>

<span data-ttu-id="1c897-250">Od verze EF Core 3.0, že už fluent API pro konfiguraci vlastnosti navigace vlastníkovi použitím `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="1c897-250">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="1c897-251">Příklad:</span><span class="sxs-lookup"><span data-stu-id="1c897-251">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="1c897-252">Konfigurace související se o vztah mezi vlastníka a vlastní by měl být zřetězené teď po `WithOwner()` podobně jako na konfiguraci jiné vztahy.</span><span class="sxs-lookup"><span data-stu-id="1c897-252">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="1c897-253">Při konfiguraci pro vlastní typ, samotný by stále možné zřetězit po `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="1c897-253">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="1c897-254">Příklad:</span><span class="sxs-lookup"><span data-stu-id="1c897-254">For example:</span></span>

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

<span data-ttu-id="1c897-255">Kromě volá `Entity()`, `HasOne()`, nebo `Set()` s typem vlastnictví cíl se vyvolají výjimku.</span><span class="sxs-lookup"><span data-stu-id="1c897-255">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="1c897-256">**Proč**</span><span class="sxs-lookup"><span data-stu-id="1c897-256">**Why**</span></span>

<span data-ttu-id="1c897-257">Tato změna byla provedena vytvořit jasnější oddělení mezi konfigurací samotného typu vlastnictví a _vztah k_ typ vlastnictví.</span><span class="sxs-lookup"><span data-stu-id="1c897-257">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="1c897-258">Tím zase nejednoznačnosti a záměny kolem metody, jako je `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="1c897-258">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="1c897-259">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="1c897-259">**Mitigations**</span></span>

<span data-ttu-id="1c897-260">Změna konfigurace vlastněné typ vztahů používat nové plochy rozhraní API, jak je znázorněno v příkladu výše.</span><span class="sxs-lookup"><span data-stu-id="1c897-260">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="1c897-261">Závislých položek sdílení v tabulce k objektu zabezpečení jsou teď nepovinné.</span><span class="sxs-lookup"><span data-stu-id="1c897-261">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="1c897-262">Sledování problému #9005</span><span class="sxs-lookup"><span data-stu-id="1c897-262">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="1c897-263">Tato změna bude zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="1c897-263">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1c897-264">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-264">**Old behavior**</span></span>

<span data-ttu-id="1c897-265">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="1c897-265">Consider the following model:</span></span>
```C#
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```
<span data-ttu-id="1c897-266">Před EF Core 3.0, pokud `OrderDetails` vlastní `Order` nebo explicitně namapované na stejnou tabulku a `OrderDetails` instance byla požadována vždy při přidání nového `Order`.</span><span class="sxs-lookup"><span data-stu-id="1c897-266">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="1c897-267">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-267">**New behavior**</span></span>

<span data-ttu-id="1c897-268">Od verze 3.0, EF Core umožňuje přidat `Order` bez `OrderDetails` a mapuje všechny `OrderDetails` vlastnosti s výjimkou primární klíč pro sloupce s možnou hodnotou Null.</span><span class="sxs-lookup"><span data-stu-id="1c897-268">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="1c897-269">Při dotazování na EF Core sady `OrderDetails` k `null` Pokud některou z jejích požadovaných vlastností nemá žádnou hodnotu nebo nemá žádné požadované vlastnosti kromě primárního klíče a všechny vlastnosti jsou `null`.</span><span class="sxs-lookup"><span data-stu-id="1c897-269">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="1c897-270">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="1c897-270">**Mitigations**</span></span>

<span data-ttu-id="1c897-271">Pokud váš model obsahuje tabulku sdílení závislé se všemi sloupci volitelné, ale navigace ukazatel není má být `null` pak aplikace by měla upravit tak, aby zpracovat případy při navigaci `null`.</span><span class="sxs-lookup"><span data-stu-id="1c897-271">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="1c897-272">Pokud to není možné požadovanou vlastnost měla být přidána do typu entity nebo musí mít alespoň jednu vlastnost non -`null` přiřazena hodnota.</span><span class="sxs-lookup"><span data-stu-id="1c897-272">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

## <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="1c897-273">Všechny entity sdílení tabulku se sloupcem tokenů souběžnosti mít mapování na vlastnost</span><span class="sxs-lookup"><span data-stu-id="1c897-273">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="1c897-274">Sledování problému #14154</span><span class="sxs-lookup"><span data-stu-id="1c897-274">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="1c897-275">Tato změna bude zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="1c897-275">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1c897-276">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-276">**Old behavior**</span></span>

<span data-ttu-id="1c897-277">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="1c897-277">Consider the following model:</span></span>
```C#
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public byte[] Version { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Order>()
        .Property(o => o.Version).IsRowVersion().HasColumnName("Version");
}
```
<span data-ttu-id="1c897-278">Před EF Core 3.0, pokud `OrderDetails` vlastní `Order` nebo explicitně namapované na stejnou tabulku následně aktualizovat jenom `OrderDetails` neaktualizuje `Version` hodnoty na klientovi a příští aktualizace se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="1c897-278">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="1c897-279">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-279">**New behavior**</span></span>

<span data-ttu-id="1c897-280">Od verze 3.0, rozšíří EF Core nové `Version` hodnota, která se `Order` Pokud vlastní `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="1c897-280">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="1c897-281">V opačném případě dojde k výjimce během ověření modelu.</span><span class="sxs-lookup"><span data-stu-id="1c897-281">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="1c897-282">**Proč**</span><span class="sxs-lookup"><span data-stu-id="1c897-282">**Why**</span></span>

<span data-ttu-id="1c897-283">Tato změna byla provedena, když se aktualizuje jenom jeden z entity, které jsou namapované na stejnou tabulku, aby hodnota tokenu zastaralé souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="1c897-283">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="1c897-284">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="1c897-284">**Mitigations**</span></span>

<span data-ttu-id="1c897-285">Všechny entity v tabulce pro sdílení obsahu se mají zahrnout vlastnost, která je namapovaná na sloupci tokenů souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="1c897-285">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="1c897-286">Je možné vytvořit, jeden v stínové stavu:</span><span class="sxs-lookup"><span data-stu-id="1c897-286">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

## <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="1c897-287">Vlastnosti zděděné nenamapované typy jsou nyní mapovány do jednoho sloupce pro všechny odvozené typy</span><span class="sxs-lookup"><span data-stu-id="1c897-287">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="1c897-288">Sledování problému #13998</span><span class="sxs-lookup"><span data-stu-id="1c897-288">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="1c897-289">Tato změna bude zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="1c897-289">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1c897-290">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-290">**Old behavior**</span></span>

<span data-ttu-id="1c897-291">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="1c897-291">Consider the following model:</span></span>
```C#
public abstract class EntityBase
{
    public int Id { get; set; }
}

public abstract class OrderBase : EntityBase
{
    public int ShippingAddress { get; set; }
}

public class BulkOrder : OrderBase
{
}

public class Order : OrderBase
{
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Ignore<OrderBase>();
    modelBuilder.Entity<EntityBase>();
    modelBuilder.Entity<BulkOrder>();
    modelBuilder.Entity<Order>();
}
```

<span data-ttu-id="1c897-292">Před EF Core 3.0 `ShippingAddress` vlastnost by být mapována k oddělení sloupců pro `BulkOrder` a `Order` ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="1c897-292">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="1c897-293">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-293">**New behavior**</span></span>

<span data-ttu-id="1c897-294">Od verze 3.0, EF Core vytvoří pouze jeden sloupec pro `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="1c897-294">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="1c897-295">**Proč**</span><span class="sxs-lookup"><span data-stu-id="1c897-295">**Why**</span></span>

<span data-ttu-id="1c897-296">Staré chování nebyl očekáván.</span><span class="sxs-lookup"><span data-stu-id="1c897-296">The old behavoir was unexpected.</span></span>

<span data-ttu-id="1c897-297">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="1c897-297">**Mitigations**</span></span>

<span data-ttu-id="1c897-298">Vlastnost je stále explicitně mapovat pro oddělení sloupců v odvozených typů:</span><span class="sxs-lookup"><span data-stu-id="1c897-298">The property can still be explicitly mapped to separate column on the derived types:</span></span>

```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Ignore<OrderBase>();
    modelBuilder.Entity<EntityBase>();
    modelBuilder.Entity<BulkOrder>()
        .Property(o => o.ShippingAddress).HasColumnName("BulkShippingAddress");
    modelBuilder.Entity<Order>()
        .Property(o => o.ShippingAddress).HasColumnName("ShippingAddress");
}
```

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="1c897-299">Konvence vlastnost cizího klíče už neodpovídá stejný název jako vlastnost instančního objektu</span><span class="sxs-lookup"><span data-stu-id="1c897-299">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="1c897-300">Sledování problému #13274</span><span class="sxs-lookup"><span data-stu-id="1c897-300">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="1c897-301">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="1c897-301">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1c897-302">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-302">**Old behavior**</span></span>

<span data-ttu-id="1c897-303">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="1c897-303">Consider the following model:</span></span>
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
<span data-ttu-id="1c897-304">Před EF Core 3.0 `CustomerId` vlastnost se použije pro cizí klíč konvencí.</span><span class="sxs-lookup"><span data-stu-id="1c897-304">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="1c897-305">Nicméně pokud `Order` je typ vlastnictví, a potom to by také provést `CustomerId` primární klíč a to se většinou očekává.</span><span class="sxs-lookup"><span data-stu-id="1c897-305">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="1c897-306">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-306">**New behavior**</span></span>

<span data-ttu-id="1c897-307">Od verze 3.0, EF Core nesnaží při využívání vlastností cizího klíče podle konvence, pokud mají stejný název jako vlastnost instančního objektu.</span><span class="sxs-lookup"><span data-stu-id="1c897-307">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="1c897-308">Název instančního objektu typu zřetězený s názvem hlavní vlastnosti a název navigační zřetězená s vzory názvů vlastnosti principal budou stále odpovídat.</span><span class="sxs-lookup"><span data-stu-id="1c897-308">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="1c897-309">Příklad:</span><span class="sxs-lookup"><span data-stu-id="1c897-309">For example:</span></span>

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

<span data-ttu-id="1c897-310">**Proč**</span><span class="sxs-lookup"><span data-stu-id="1c897-310">**Why**</span></span>

<span data-ttu-id="1c897-311">Tato změna byla provedena na chybně nedefinujte vlastnost primárního klíče typ vlastnictví.</span><span class="sxs-lookup"><span data-stu-id="1c897-311">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="1c897-312">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="1c897-312">**Mitigations**</span></span>

<span data-ttu-id="1c897-313">Pokud byla vlastnost má být cizí klíč a proto část primárního klíče a pak explicitně jako takový ho nakonfigurujte.</span><span class="sxs-lookup"><span data-stu-id="1c897-313">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="1c897-314">Připojení k databázi je nyní uzavřeno, pokud není využito už před objektu TransactionScope byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="1c897-314">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="1c897-315">Sledování problému #14218</span><span class="sxs-lookup"><span data-stu-id="1c897-315">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="1c897-316">Tato změna bude zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="1c897-316">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1c897-317">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-317">**Old behavior**</span></span>

<span data-ttu-id="1c897-318">Před EF Core 3.0, pokud kontext otevře připojení k uvnitř `TransactionScope`, připojení zůstane otevřená, při aktuální `TransactionScope` je aktivní.</span><span class="sxs-lookup"><span data-stu-id="1c897-318">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

```C#
using (new TransactionScope())
{
    using (AdventureWorks context = new AdventureWorks())
    {
        context.ProductCategories.Add(new ProductCategory());
        context.SaveChanges();

        // Old behavior: Connection is still open at this point
        
        var categories = context.ProductCategories().ToList();
    }
}
```

<span data-ttu-id="1c897-319">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-319">**New behavior**</span></span>

<span data-ttu-id="1c897-320">Od verze 3.0, EF Core uzavře připojení co nejdříve po dokončení jeho použití.</span><span class="sxs-lookup"><span data-stu-id="1c897-320">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="1c897-321">**Proč**</span><span class="sxs-lookup"><span data-stu-id="1c897-321">**Why**</span></span>

<span data-ttu-id="1c897-322">Tato změna umožňuje použití několika kontextech v rámci stejného `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="1c897-322">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="1c897-323">Nové chování alose odpovídá EF6.</span><span class="sxs-lookup"><span data-stu-id="1c897-323">The new behavior alose matches EF6.</span></span>

<span data-ttu-id="1c897-324">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="1c897-324">**Mitigations**</span></span>

<span data-ttu-id="1c897-325">Pokud je nutné připojení zůstat otevřené explicitní volání konstruktoru `OpenConnection()` zajistí, že EF Core nezavírá je předčasně ukončena:</span><span class="sxs-lookup"><span data-stu-id="1c897-325">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

```C#
using (new TransactionScope())
{
    using (AdventureWorks context = new AdventureWorks())
    {
        context.Database.OpenConnection();
        context.ProductCategories.Add(new ProductCategory());
        context.SaveChanges();
        
        var categories = context.ProductCategories().ToList();
        context.Database.CloseConnection();
    }
}
```

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="1c897-326">Jednotlivé vlastnosti pomocí generování klíčů nezávislé celé číslo v paměti</span><span class="sxs-lookup"><span data-stu-id="1c897-326">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="1c897-327">Sledování problému #6872</span><span class="sxs-lookup"><span data-stu-id="1c897-327">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="1c897-328">Tato změna bude zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="1c897-328">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1c897-329">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-329">**Old behavior**</span></span>

<span data-ttu-id="1c897-330">Před EF Core 3.0 se použil jeden generátor sdíleného hodnot pro všechny vlastnosti klíče celé číslo v paměti.</span><span class="sxs-lookup"><span data-stu-id="1c897-330">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="1c897-331">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-331">**New behavior**</span></span>

<span data-ttu-id="1c897-332">Od verze EF Core 3.0, každé celé číslo klíčová vlastnost získá vlastní generátor hodnot při použití databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="1c897-332">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="1c897-333">Také pokud se odstraní databáze, pak generování klíčů je obnovit pro všechny tabulky.</span><span class="sxs-lookup"><span data-stu-id="1c897-333">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="1c897-334">**Proč**</span><span class="sxs-lookup"><span data-stu-id="1c897-334">**Why**</span></span>

<span data-ttu-id="1c897-335">Tato změna byla provedena zarovnat generování klíče v paměti blíže k generování klíčů skutečná databáze a zlepšit schopnost izolace testů od sebe navzájem při použití databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="1c897-335">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="1c897-336">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="1c897-336">**Mitigations**</span></span>

<span data-ttu-id="1c897-337">Toto může rozbít aplikaci, která se spoléhá na konkrétní hodnoty klíče v paměti nastavit.</span><span class="sxs-lookup"><span data-stu-id="1c897-337">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="1c897-338">Zvažte místo toho spoléhat na konkrétní hodnoty klíče, ani aktualizaci tak, aby odpovídala nové chování.</span><span class="sxs-lookup"><span data-stu-id="1c897-338">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="1c897-339">Základní pole se používají ve výchozím nastavení</span><span class="sxs-lookup"><span data-stu-id="1c897-339">Backing fields are used by default</span></span>

[<span data-ttu-id="1c897-340">Sledování problému #12430</span><span class="sxs-lookup"><span data-stu-id="1c897-340">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="1c897-341">Tato změna byla zavedená v EF Core 3.0 – náhled 2.</span><span class="sxs-lookup"><span data-stu-id="1c897-341">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="1c897-342">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-342">**Old behavior**</span></span>

<span data-ttu-id="1c897-343">Než 3.0 i v případě, že pomocné pole vlastnosti zná, EF Core by stále ve výchozím nastavení čtení a zápisu hodnoty vlastnosti pomocí metody getter a setter vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="1c897-343">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="1c897-344">K tomuto výjimka provádění dotazů, kde pomocné pole se nastavuje přímo Pokud jsou známé.</span><span class="sxs-lookup"><span data-stu-id="1c897-344">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="1c897-345">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-345">**New behavior**</span></span>

<span data-ttu-id="1c897-346">Počínaje EF Core 3.0, pokud je znám pomocným polem vlastnosti potom budou vždy číst a Zapisovat vlastnosti pomocí pole zálohování.</span><span class="sxs-lookup"><span data-stu-id="1c897-346">Starting with EF Core 3.0, if the backing field for a property is known, then will always read and write that property using the backing field.</span></span>
<span data-ttu-id="1c897-347">Pokud aplikace se spoléhá na další chování zakódovaný do metody getter nebo setter to může způsobit přerušení aplikace.</span><span class="sxs-lookup"><span data-stu-id="1c897-347">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="1c897-348">**Proč**</span><span class="sxs-lookup"><span data-stu-id="1c897-348">**Why**</span></span>

<span data-ttu-id="1c897-349">Tato změna byla provedena zabránit EF Core chybně aktivací obchodní logiky ve výchozím nastavení při provádění databázových operací zahrnující entity.</span><span class="sxs-lookup"><span data-stu-id="1c897-349">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="1c897-350">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="1c897-350">**Mitigations**</span></span>

<span data-ttu-id="1c897-351">Prostřednictvím konfigurace režim přístupu k vlastnosti v rozhraní API fluent modelBuilder lze obnovit chování pre-3.0.</span><span class="sxs-lookup"><span data-stu-id="1c897-351">The pre-3.0 behavior can be restored through configuration of the property access mode in the modelBuilder fluent API.</span></span>
<span data-ttu-id="1c897-352">Příklad:</span><span class="sxs-lookup"><span data-stu-id="1c897-352">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="1c897-353">Vyvolání výjimky, pokud je nalezeno více polí kompatibilní zálohování</span><span class="sxs-lookup"><span data-stu-id="1c897-353">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="1c897-354">Sledování problému #12523</span><span class="sxs-lookup"><span data-stu-id="1c897-354">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="1c897-355">Tato změna bude zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="1c897-355">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1c897-356">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-356">**Old behavior**</span></span>

<span data-ttu-id="1c897-357">Před EF Core 3.0 Pokud odpovídá více polí pravidel pro vyhledání pomocným polem vlastnosti, pak vybere jedno pole na základě některých pořadí priority.</span><span class="sxs-lookup"><span data-stu-id="1c897-357">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="1c897-358">To může způsobit nesprávné pole pro použití v případech, nejednoznačný.</span><span class="sxs-lookup"><span data-stu-id="1c897-358">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="1c897-359">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-359">**New behavior**</span></span>

<span data-ttu-id="1c897-360">Od verze EF Core 3.0, pokud více polí budou odpovídat na stejnou vlastnost, pak je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="1c897-360">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="1c897-361">**Proč**</span><span class="sxs-lookup"><span data-stu-id="1c897-361">**Why**</span></span>

<span data-ttu-id="1c897-362">Tato změna byla provedena, abyste se vyhnuli použití tiše jedno pole před jiným při může obsahovat jen jedno správné.</span><span class="sxs-lookup"><span data-stu-id="1c897-362">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="1c897-363">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="1c897-363">**Mitigations**</span></span>

<span data-ttu-id="1c897-364">Vlastnosti s nejednoznačný základní pole musí mít pole pro použití explicitně zadán.</span><span class="sxs-lookup"><span data-stu-id="1c897-364">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="1c897-365">Například pomocí rozhraní fluent API:</span><span class="sxs-lookup"><span data-stu-id="1c897-365">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="1c897-366">Vlastnost jen pro pole názvů by měl odpovídat názvu pole</span><span class="sxs-lookup"><span data-stu-id="1c897-366">Field-only property names should match the field name</span></span>

<span data-ttu-id="1c897-367">Tato změna bude zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="1c897-367">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1c897-368">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-368">**Old behavior**</span></span>

<span data-ttu-id="1c897-369">Před EF Core 3.0 vlastnost může být určeno hodnotu řetězce a pokud nebyla nalezena žádná vlastnost s tímto názvem na typu CLR EF Core by opakujte tak, aby odpovídaly na pole, které používá convetion pravidla.</span><span class="sxs-lookup"><span data-stu-id="1c897-369">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convetion rules.</span></span>
```C#
private class Blog
{
    private int _id;
    public string Name { get; set; }
}
```
```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id");
```

<span data-ttu-id="1c897-370">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-370">**New behavior**</span></span>

<span data-ttu-id="1c897-371">Od verze EF Core 3.0, vlastnost jen pro pole musí odpovídat názvu pole přesně.</span><span class="sxs-lookup"><span data-stu-id="1c897-371">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="1c897-372">**Proč**</span><span class="sxs-lookup"><span data-stu-id="1c897-372">**Why**</span></span>

<span data-ttu-id="1c897-373">Tato změna byla provedena, abyste se vyhnuli použití stejné pole pro dvě vlastnosti s názvem podobně, také udržuje pravidla pro porovnávání vlastností pouze pole je stejný jako u vlastnosti, které jsou namapovány na vlastnosti CLR.</span><span class="sxs-lookup"><span data-stu-id="1c897-373">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="1c897-374">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="1c897-374">**Mitigations**</span></span>

<span data-ttu-id="1c897-375">Vlastnosti pouze pro pole musí mít název jako pole, které jsou mapovány na stejný.</span><span class="sxs-lookup"><span data-stu-id="1c897-375">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="1c897-376">V novější verzi preview EF Core 3.0 Plánujeme znovu povolit explicitně konfigurace název pole, které se liší od názvu vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="1c897-376">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="1c897-377">AddDbContext/AddDbContextPool zavolejte už AddLogging a AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="1c897-377">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="1c897-378">Sledování problému #14756</span><span class="sxs-lookup"><span data-stu-id="1c897-378">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="1c897-379">Tato změna bude zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="1c897-379">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1c897-380">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-380">**Old behavior**</span></span>

<span data-ttu-id="1c897-381">Před EF Core 3.0, volání `AddDbContext` nebo `AddDbContextPool` by také zaregistrovat protokolování a ukládání služeb s D.I prostřednictvím volání do mezipaměti [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) a [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="1c897-381">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="1c897-382">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-382">**New behavior**</span></span>

<span data-ttu-id="1c897-383">Od verze EF Core 3.0, `AddDbContext` a `AddDbContextPool` nebude registru tyto služby s DI (Dependency Injection).</span><span class="sxs-lookup"><span data-stu-id="1c897-383">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="1c897-384">**Proč**</span><span class="sxs-lookup"><span data-stu-id="1c897-384">**Why**</span></span>

<span data-ttu-id="1c897-385">EF Core 3.0 nevyžaduje, že tyto služby jsou v cotainer DI vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="1c897-385">EF Core 3.0 does not require that these services are in the application's DI cotainer.</span></span> <span data-ttu-id="1c897-386">Nicméně pokud `ILoggerFactory` je zaregistrovaný v aplikačním kontejneru DI, pak bude i nadále využívat EF Core.</span><span class="sxs-lookup"><span data-stu-id="1c897-386">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="1c897-387">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="1c897-387">**Mitigations**</span></span>

<span data-ttu-id="1c897-388">Pokud vaše aplikace potřebuje tyto služby, registrujte je explicitně s využitím kontejnerů DI [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) nebo [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="1c897-388">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="1c897-389">DbContext.Entry teď provádí místní metoda DetectChanges</span><span class="sxs-lookup"><span data-stu-id="1c897-389">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="1c897-390">Sledování problému #13552</span><span class="sxs-lookup"><span data-stu-id="1c897-390">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="1c897-391">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="1c897-391">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1c897-392">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-392">**Old behavior**</span></span>

<span data-ttu-id="1c897-393">Před EF Core 3.0, volání `DbContext.Entry` způsobí změny, aby se rozpoznal pro všechny sledované entity.</span><span class="sxs-lookup"><span data-stu-id="1c897-393">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="1c897-394">Tím zajistíte, že stav zpřístupněn v `EntityEntry` byla aktuální.</span><span class="sxs-lookup"><span data-stu-id="1c897-394">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="1c897-395">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-395">**New behavior**</span></span>

<span data-ttu-id="1c897-396">Spouští se s EF Core 3.0, volání `DbContext.Entry` se teď jenom pokus o zjištění změn v dané entitě a všechny sledované instančního objektu entity, které s ním souvisejí.</span><span class="sxs-lookup"><span data-stu-id="1c897-396">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="1c897-397">To znamená, že se změní jinde nemusí byl zjištěn zavoláním této metody, které by mohly mít vliv na stav aplikace.</span><span class="sxs-lookup"><span data-stu-id="1c897-397">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="1c897-398">Všimněte si, že pokud `ChangeTracker.AutoDetectChangesEnabled` je nastavena na `false` pak i tato detekce místní změny se deaktivuje.</span><span class="sxs-lookup"><span data-stu-id="1c897-398">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="1c897-399">Jiné metody, které způsobují detekce změn – například `ChangeTracker.Entries` a `SaveChanges`– způsobí úplné `DetectChanges` všech sledovat entity.</span><span class="sxs-lookup"><span data-stu-id="1c897-399">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="1c897-400">**Proč**</span><span class="sxs-lookup"><span data-stu-id="1c897-400">**Why**</span></span>

<span data-ttu-id="1c897-401">Tato změna byla provedena pro zlepšení výkonu výchozí použití `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="1c897-401">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="1c897-402">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="1c897-402">**Mitigations**</span></span>

<span data-ttu-id="1c897-403">Volání `ChgangeTracker.DetectChanges()` explicitně před voláním `Entry` k zajištění chování pre-3.0.</span><span class="sxs-lookup"><span data-stu-id="1c897-403">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="1c897-404">Řetězec a bajtové pole klíčů se nerozlišují klientem generovaná ve výchozím nastavení</span><span class="sxs-lookup"><span data-stu-id="1c897-404">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="1c897-405">Sledování problému #14617</span><span class="sxs-lookup"><span data-stu-id="1c897-405">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="1c897-406">Tato změna bude zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="1c897-406">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1c897-407">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-407">**Old behavior**</span></span>

<span data-ttu-id="1c897-408">Před EF Core 3.0 `string` a `byte[]` klíčové vlastnosti použití explicitním nastavením nenulová hodnota.</span><span class="sxs-lookup"><span data-stu-id="1c897-408">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="1c897-409">V takovém případě by se vygenerovala hodnota klíče na klientovi jako identifikátor GUID serializovat do bajtů pro `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="1c897-409">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="1c897-410">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-410">**New behavior**</span></span>

<span data-ttu-id="1c897-411">Od verze EF Core 3.0 výjimka bude vyvolána označující, že nebyla nastavena žádná hodnota klíče.</span><span class="sxs-lookup"><span data-stu-id="1c897-411">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="1c897-412">**Proč**</span><span class="sxs-lookup"><span data-stu-id="1c897-412">**Why**</span></span>

<span data-ttu-id="1c897-413">Tato změna byla provedena, protože klient generován `string` / `byte[]` hodnoty nejsou obecně užitečné a výchozí chování dostal pevné argumentovat o generované hodnoty klíče v běžným způsobem.</span><span class="sxs-lookup"><span data-stu-id="1c897-413">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="1c897-414">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="1c897-414">**Mitigations**</span></span>

<span data-ttu-id="1c897-415">Chování pre-3.0 je možné získat tak, že explicitně zadáte, by měl klíčové vlastnosti použít generované hodnoty, pokud je nastavena žádná hodnota jiná než null.</span><span class="sxs-lookup"><span data-stu-id="1c897-415">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="1c897-416">Například pomocí rozhraní fluent API:</span><span class="sxs-lookup"><span data-stu-id="1c897-416">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="1c897-417">Nebo s anotacemi dat:</span><span class="sxs-lookup"><span data-stu-id="1c897-417">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="1c897-418">Implementaci třídy ILoggerFactory je nyní vymezené služby</span><span class="sxs-lookup"><span data-stu-id="1c897-418">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="1c897-419">Sledování problému #14698</span><span class="sxs-lookup"><span data-stu-id="1c897-419">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="1c897-420">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="1c897-420">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1c897-421">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-421">**Old behavior**</span></span>

<span data-ttu-id="1c897-422">Před EF Core 3.0 `ILoggerFactory` bylo zaregistrováno jako služba typu singleton.</span><span class="sxs-lookup"><span data-stu-id="1c897-422">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="1c897-423">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-423">**New behavior**</span></span>

<span data-ttu-id="1c897-424">Od verze EF Core 3.0, `ILoggerFactory` je teď zaregistrovaný jako obor.</span><span class="sxs-lookup"><span data-stu-id="1c897-424">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="1c897-425">**Proč**</span><span class="sxs-lookup"><span data-stu-id="1c897-425">**Why**</span></span>

<span data-ttu-id="1c897-426">Tato změna byla provedena umožňující přidružení protokolovací nástroj se `DbContext` instanci, která povoluje další funkce a odebírá někdy patologických chování, jako je například obrovské množství poskytovatelů vnitřní chybě služby.</span><span class="sxs-lookup"><span data-stu-id="1c897-426">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="1c897-427">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="1c897-427">**Mitigations**</span></span>

<span data-ttu-id="1c897-428">Tato změna by neměla mít vliv kód aplikace, pokud není registrace a použití vlastních služeb v poskytovateli EF Core vnitřní chybě služby.</span><span class="sxs-lookup"><span data-stu-id="1c897-428">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="1c897-429">Tato akce není běžné.</span><span class="sxs-lookup"><span data-stu-id="1c897-429">This isn't common.</span></span>
<span data-ttu-id="1c897-430">V těchto případech většinu toho, co bude dál fungovat, ale žádné služby typu singleton, která byla v závislosti na `ILoggerFactory` bude nutné změnit tak, aby získat `ILoggerFactory` jiným způsobem.</span><span class="sxs-lookup"><span data-stu-id="1c897-430">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="1c897-431">Pokud narazíte na situace tímto způsobem, založte prosím problém na na [sledování problémů Githubu EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) a dejte nám vědět, jak používáte `ILoggerFactory` tak, aby nám můžete lépe porozumět nechcete v budoucnu znovu rozdělit.</span><span class="sxs-lookup"><span data-stu-id="1c897-431">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="idbcontextoptionsextensionwithdebuginfo-merged-into-idbcontextoptionsextension"></a><span data-ttu-id="1c897-432">Sloučí IDbContextOptionsExtension IDbContextOptionsExtensionWithDebugInfo</span><span class="sxs-lookup"><span data-stu-id="1c897-432">IDbContextOptionsExtensionWithDebugInfo merged into IDbContextOptionsExtension</span></span>

[<span data-ttu-id="1c897-433">Sledování problému #13552</span><span class="sxs-lookup"><span data-stu-id="1c897-433">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="1c897-434">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="1c897-434">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1c897-435">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-435">**Old behavior**</span></span>

<span data-ttu-id="1c897-436">`IDbContextOptionsExtensionWithDebugInfo` Další volitelné rozhraní byla prodloužena z `IDbContextOptionsExtension` pro vyvarování rozbíjející změně rozhraní během cyklu vydání verze 2.x.</span><span class="sxs-lookup"><span data-stu-id="1c897-436">`IDbContextOptionsExtensionWithDebugInfo` was an additional optional interface extended from `IDbContextOptionsExtension` to avoid making a breaking change to the interface during the 2.x release cycle.</span></span>

<span data-ttu-id="1c897-437">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-437">**New behavior**</span></span>

<span data-ttu-id="1c897-438">Rozhraní jsou nyní sloučeny do `IDbContextOptionsExtension`.</span><span class="sxs-lookup"><span data-stu-id="1c897-438">The interfaces are now merged together into `IDbContextOptionsExtension`.</span></span>

<span data-ttu-id="1c897-439">**Proč**</span><span class="sxs-lookup"><span data-stu-id="1c897-439">**Why**</span></span>

<span data-ttu-id="1c897-440">Tato změna byla provedena, protože rozhraní jsou koncepčně jednou.</span><span class="sxs-lookup"><span data-stu-id="1c897-440">This change was made because the interfaces are conceptually one.</span></span>

<span data-ttu-id="1c897-441">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="1c897-441">**Mitigations**</span></span>

<span data-ttu-id="1c897-442">Žádné implementace `IDbContextOptionsExtension` bude muset být aktualizované kvůli podpoře nového člena.</span><span class="sxs-lookup"><span data-stu-id="1c897-442">Any implementations of `IDbContextOptionsExtension` will need to be updated to support the new member.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="1c897-443">Opožděné načtení proxy už předpokládají, že jsou plně načteny navigační vlastnosti</span><span class="sxs-lookup"><span data-stu-id="1c897-443">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="1c897-444">Sledování problému #12780</span><span class="sxs-lookup"><span data-stu-id="1c897-444">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="1c897-445">Tato změna bude zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="1c897-445">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1c897-446">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-446">**Old behavior**</span></span>

<span data-ttu-id="1c897-447">Před EF Core 3.0, jednou `DbContext` byl odstraněn neexistoval způsob, jak zjistit, zda danou navigační vlastnost s entitou získané z daného kontextu byl plně načten či nikoli.</span><span class="sxs-lookup"><span data-stu-id="1c897-447">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="1c897-448">Proxy by místo toho se předpokládá, že je načtena navigační odkaz, pokud má nenulovou hodnotu a, že je načtena navigace kolekce, pokud není prázdná.</span><span class="sxs-lookup"><span data-stu-id="1c897-448">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="1c897-449">V těchto případech by pokus o opožděné načtení no-op.</span><span class="sxs-lookup"><span data-stu-id="1c897-449">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="1c897-450">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-450">**New behavior**</span></span>

<span data-ttu-id="1c897-451">Od verze EF Core 3.0, proxy servery sledovat, jestli je načtena navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="1c897-451">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="1c897-452">To znamená, že při pokusu o přístup k navigační vlastnost, která je načtena po kontext se vyřadil. bude vždycky no-op, i v případě, že je načteno navigace je prázdný nebo mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="1c897-452">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="1c897-453">Naopak pokusu o přístup k navigační vlastnosti, které není načteno vyvolají výjimku pokud uvolnění kontextu, i když je navigační vlastnost kolekce není prázdná.</span><span class="sxs-lookup"><span data-stu-id="1c897-453">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="1c897-454">Pokud k této situaci dochází, znamená to kód aplikace se pokouší použít opožděné načtení na neplatný čas a aplikace by měla být změněna na tuto akci.</span><span class="sxs-lookup"><span data-stu-id="1c897-454">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="1c897-455">**Proč**</span><span class="sxs-lookup"><span data-stu-id="1c897-455">**Why**</span></span>

<span data-ttu-id="1c897-456">Tato změna byla provedena, aby chování konzistentní a správné při pokusu o opožděné načtení na vyřazený `DbContext` instance.</span><span class="sxs-lookup"><span data-stu-id="1c897-456">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="1c897-457">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="1c897-457">**Mitigations**</span></span>

<span data-ttu-id="1c897-458">Kód aplikace, který nebude pokoušet opožděné načtení vyřazený kontextu, nebo nakonfigurovat to být no-op. jak je popsáno v zpráva o výjimce.</span><span class="sxs-lookup"><span data-stu-id="1c897-458">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="1c897-459">Nadměrné vytváření zprostředkovatelů vnitřní chybě služby je teď k chybě ve výchozím nastavení</span><span class="sxs-lookup"><span data-stu-id="1c897-459">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="1c897-460">Sledování problému #10236</span><span class="sxs-lookup"><span data-stu-id="1c897-460">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="1c897-461">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="1c897-461">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1c897-462">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-462">**Old behavior**</span></span>

<span data-ttu-id="1c897-463">Upozornění by před EF Core 3.0 zaznamenávané aplikace vytváří patologických několik poskytovatelů vnitřní chybě služby.</span><span class="sxs-lookup"><span data-stu-id="1c897-463">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="1c897-464">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-464">**New behavior**</span></span>

<span data-ttu-id="1c897-465">Od verze EF Core 3.0, toto upozornění se teď považuje za a chyby a výjimky je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="1c897-465">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="1c897-466">**Proč**</span><span class="sxs-lookup"><span data-stu-id="1c897-466">**Why**</span></span>

<span data-ttu-id="1c897-467">Tato změna byla provedena na připravovat lepší kód aplikace prostřednictvím vystavení takovém patologických více explicitně.</span><span class="sxs-lookup"><span data-stu-id="1c897-467">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="1c897-468">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="1c897-468">**Mitigations**</span></span>

<span data-ttu-id="1c897-469">Nejvhodnější příčinu akce při výskytu této chyby je zjistěte původní příčinu a nevytvářet mnoho poskytovatelů vnitřní chybě služby.</span><span class="sxs-lookup"><span data-stu-id="1c897-469">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="1c897-470">Však chyba může být převést zpět na upozornění (nebo ignorovat) prostřednictvím konfigurace na `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1c897-470">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="1c897-471">Příklad:</span><span class="sxs-lookup"><span data-stu-id="1c897-471">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="1c897-472">Nové chování pro HasOne/HasMany volat jeden řetězec</span><span class="sxs-lookup"><span data-stu-id="1c897-472">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="1c897-473">Sledování problému #9171</span><span class="sxs-lookup"><span data-stu-id="1c897-473">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="1c897-474">Tato změna bude zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="1c897-474">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1c897-475">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-475">**Old behavior**</span></span>

<span data-ttu-id="1c897-476">Před EF Core 3.0 kód volání `HasOne` nebo `HasMany` s jeden řetězec byl interpretovat matoucí způsobem.</span><span class="sxs-lookup"><span data-stu-id="1c897-476">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpretted in a confusing way.</span></span>
<span data-ttu-id="1c897-477">Příklad:</span><span class="sxs-lookup"><span data-stu-id="1c897-477">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="1c897-478">Kód vypadá je týkající se `Samurai` některé jiné entity typu použití `Entrance` navigační vlastnost, která může být privátní.</span><span class="sxs-lookup"><span data-stu-id="1c897-478">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="1c897-479">Ve skutečnosti, tento kód se pokouší vytvořit relaci některé typ entity s názvem `Entrance` se žádné navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="1c897-479">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="1c897-480">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-480">**New behavior**</span></span>

<span data-ttu-id="1c897-481">Od verze EF Core 3.0, výše uvedený kód nyní dělá co podívali jako její by měl mít byla činnosti před.</span><span class="sxs-lookup"><span data-stu-id="1c897-481">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="1c897-482">**Proč**</span><span class="sxs-lookup"><span data-stu-id="1c897-482">**Why**</span></span>

<span data-ttu-id="1c897-483">Staré chování bylo velmi matoucí, zejména při čtení konfigurace kód a hledání chyb.</span><span class="sxs-lookup"><span data-stu-id="1c897-483">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="1c897-484">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="1c897-484">**Mitigations**</span></span>

<span data-ttu-id="1c897-485">Tímto přerušíte pouze aplikace, které jsou explicitně konfigurace relace používání řetězců pro názvy typů a bez explicitním zadáním navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="1c897-485">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="1c897-486">Není běžné.</span><span class="sxs-lookup"><span data-stu-id="1c897-486">This is not common.</span></span>
<span data-ttu-id="1c897-487">Předchozí chování můžete získat prostřednictvím explicitně předávání `null` pro název navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="1c897-487">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="1c897-488">Příklad:</span><span class="sxs-lookup"><span data-stu-id="1c897-488">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="1c897-489">Návratový typ pro několik asynchronních metod se změnil z úlohy na ValueTask</span><span class="sxs-lookup"><span data-stu-id="1c897-489">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="1c897-490">Sledování problému #15184</span><span class="sxs-lookup"><span data-stu-id="1c897-490">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="1c897-491">Tato změna bude zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="1c897-491">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1c897-492">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-492">**Old behavior**</span></span>

<span data-ttu-id="1c897-493">Dříve vrátila následující asynchronní metody `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="1c897-493">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="1c897-494">`ValueGenerator.NextValueAsync()` (a odvozených tříd)</span><span class="sxs-lookup"><span data-stu-id="1c897-494">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="1c897-495">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-495">**New behavior**</span></span>

<span data-ttu-id="1c897-496">Výše uvedené metody nyní vrací `ValueTask<T>` přes stejné `T` stejně jako předtím.</span><span class="sxs-lookup"><span data-stu-id="1c897-496">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="1c897-497">**Proč**</span><span class="sxs-lookup"><span data-stu-id="1c897-497">**Why**</span></span>

<span data-ttu-id="1c897-498">Tato změna snižuje počet přidělení haldy vzniklé při volání těchto metod, zlepšení obecné informace o výkonu.</span><span class="sxs-lookup"><span data-stu-id="1c897-498">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="1c897-499">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="1c897-499">**Mitigations**</span></span>

<span data-ttu-id="1c897-500">Aplikace jednoduše čeká na výše uvedené rozhraní API pouze muset být překompilovány - žádné změny zdroje jsou nezbytné.</span><span class="sxs-lookup"><span data-stu-id="1c897-500">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="1c897-501">Složitější využití (například předání vráceného `Task` k `Task.WhenAny()`) obvykle vyžadují, aby vráceného `ValueTask<T>` převedou na hodnoty `Task<T>` voláním `AsTask()` na něm.</span><span class="sxs-lookup"><span data-stu-id="1c897-501">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="1c897-502">Všimněte si, že to Neguje snížení přidělení, které tato změna přináší.</span><span class="sxs-lookup"><span data-stu-id="1c897-502">Note that this negates the allocation reduction that this change brings.</span></span>

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="1c897-503">Poznámka relační: TypeMapping je teď stejně TypeMapping</span><span class="sxs-lookup"><span data-stu-id="1c897-503">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="1c897-504">Sledování problému #9913</span><span class="sxs-lookup"><span data-stu-id="1c897-504">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="1c897-505">Tato změna byla zavedená v EF Core 3.0 – náhled 2.</span><span class="sxs-lookup"><span data-stu-id="1c897-505">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="1c897-506">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-506">**Old behavior**</span></span>

<span data-ttu-id="1c897-507">Název poznámky pro mapování anotace typu byl "Relační: TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="1c897-507">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="1c897-508">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-508">**New behavior**</span></span>

<span data-ttu-id="1c897-509">Název poznámky pro mapování anotace typu je nyní "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="1c897-509">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="1c897-510">**Proč**</span><span class="sxs-lookup"><span data-stu-id="1c897-510">**Why**</span></span>

<span data-ttu-id="1c897-511">Mapování typu jsou teď používá pro více než jen relační databáze poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="1c897-511">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="1c897-512">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="1c897-512">**Mitigations**</span></span>

<span data-ttu-id="1c897-513">Tímto přerušíte jenom aplikace s přístupem k mapování typů přímo jako poznámka, která není běžné.</span><span class="sxs-lookup"><span data-stu-id="1c897-513">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="1c897-514">Nejvhodnější opatření na opravu se má používat rovinu rozhraní API pro mapování typů přístupu spíše než přímo pomocí anotace.</span><span class="sxs-lookup"><span data-stu-id="1c897-514">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="1c897-515">Vyvolá výjimku, ToTable na odvozený typ.</span><span class="sxs-lookup"><span data-stu-id="1c897-515">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="1c897-516">Sledování problému #11811</span><span class="sxs-lookup"><span data-stu-id="1c897-516">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="1c897-517">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="1c897-517">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1c897-518">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-518">**Old behavior**</span></span>

<span data-ttu-id="1c897-519">Před EF Core 3.0 `ToTable()` volalo odvozený typ bude ignorovat, protože pouze dědičnosti mapování strategie byl TPH, kdy to není platný.</span><span class="sxs-lookup"><span data-stu-id="1c897-519">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="1c897-520">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-520">**New behavior**</span></span>

<span data-ttu-id="1c897-521">Spouští se s EF Core 3.0 a při přípravě na přidání TPT a TPC podporují v pozdější verzi `ToTable()` volalo odvozený typ bude nyní vyvolání výjimky v budoucnu vyhnuli o změnu neočekávané mapování.</span><span class="sxs-lookup"><span data-stu-id="1c897-521">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="1c897-522">**Proč**</span><span class="sxs-lookup"><span data-stu-id="1c897-522">**Why**</span></span>

<span data-ttu-id="1c897-523">Aktuálně není platná pro mapování odvozeného typu na jiné tabulky.</span><span class="sxs-lookup"><span data-stu-id="1c897-523">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="1c897-524">Tato změna se vyhnete přerušení v budoucnu, kdy bude platná věc udělat.</span><span class="sxs-lookup"><span data-stu-id="1c897-524">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="1c897-525">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="1c897-525">**Mitigations**</span></span>

<span data-ttu-id="1c897-526">Odeberte všechny pokusy o mapování odvozené typy s jinými tabulkami.</span><span class="sxs-lookup"><span data-stu-id="1c897-526">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="1c897-527">Nahradí HasIndex ForSqlServerHasIndex</span><span class="sxs-lookup"><span data-stu-id="1c897-527">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="1c897-528">Sledování problému #12366</span><span class="sxs-lookup"><span data-stu-id="1c897-528">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="1c897-529">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="1c897-529">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1c897-530">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-530">**Old behavior**</span></span>

<span data-ttu-id="1c897-531">Před EF Core 3.0 `ForSqlServerHasIndex().ForSqlServerInclude()` poskytuje způsob, jak konfigurovat sloupců použitých s `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="1c897-531">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="1c897-532">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-532">**New behavior**</span></span>

<span data-ttu-id="1c897-533">Od verze EF Core 3.0, pomocí `Include` na indexu se teď podporuje relační úrovni.</span><span class="sxs-lookup"><span data-stu-id="1c897-533">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="1c897-534">Použití `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="1c897-534">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="1c897-535">**Proč**</span><span class="sxs-lookup"><span data-stu-id="1c897-535">**Why**</span></span>

<span data-ttu-id="1c897-536">Tato změna byla provedena konsolidovat rozhraní API pro indexy s `Include` do jednoho umístění pro všechny databáze poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="1c897-536">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="1c897-537">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="1c897-537">**Mitigations**</span></span>

<span data-ttu-id="1c897-538">Pomocí nového rozhraní API, jak je znázorněno výše.</span><span class="sxs-lookup"><span data-stu-id="1c897-538">Use the new API, as shown above.</span></span>

## <a name="metadata-api-changes"></a><span data-ttu-id="1c897-539">Změn metadat rozhraní API</span><span class="sxs-lookup"><span data-stu-id="1c897-539">Metadata API changes</span></span>

[<span data-ttu-id="1c897-540">Sledování problému #214</span><span class="sxs-lookup"><span data-stu-id="1c897-540">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="1c897-541">Tato změna bude zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="1c897-541">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1c897-542">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-542">**New behavior**</span></span>

<span data-ttu-id="1c897-543">Následující vlastnosti byly převedeny na rozšiřující metody:</span><span class="sxs-lookup"><span data-stu-id="1c897-543">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="1c897-544">**Proč**</span><span class="sxs-lookup"><span data-stu-id="1c897-544">**Why**</span></span>

<span data-ttu-id="1c897-545">Tato změna usnadňuje provádění výše uvedených rozhraní.</span><span class="sxs-lookup"><span data-stu-id="1c897-545">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="1c897-546">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="1c897-546">**Mitigations**</span></span>

<span data-ttu-id="1c897-547">Pomocí nové metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="1c897-547">Use the new extension methods.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="1c897-548">EF Core už odešle – Direktiva pragma pro vynucení SQLite FK</span><span class="sxs-lookup"><span data-stu-id="1c897-548">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="1c897-549">Sledování problému #12151</span><span class="sxs-lookup"><span data-stu-id="1c897-549">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="1c897-550">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="1c897-550">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1c897-551">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-551">**Old behavior**</span></span>

<span data-ttu-id="1c897-552">Před EF Core 3.0, bude posílat EF Core `PRAGMA foreign_keys = 1` při otevření připojení k SQLite.</span><span class="sxs-lookup"><span data-stu-id="1c897-552">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="1c897-553">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-553">**New behavior**</span></span>

<span data-ttu-id="1c897-554">Od verze EF Core 3.0, už EF Core odešle `PRAGMA foreign_keys = 1` při otevření připojení k SQLite.</span><span class="sxs-lookup"><span data-stu-id="1c897-554">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="1c897-555">**Proč**</span><span class="sxs-lookup"><span data-stu-id="1c897-555">**Why**</span></span>

<span data-ttu-id="1c897-556">Tato změna byla provedena, protože používá EF Core `SQLitePCLRaw.bundle_e_sqlite3` ve výchozím nastavení, která zase znamená, že vynucení cizího klíče je ve výchozím nastavení zapnuté a není potřeba explicitně povolit pokaždé, když je otevřeno připojení.</span><span class="sxs-lookup"><span data-stu-id="1c897-556">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="1c897-557">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="1c897-557">**Mitigations**</span></span>

<span data-ttu-id="1c897-558">Cizí klíče jsou povolena ve výchozím nastavení SQLitePCLRaw.bundle_e_sqlite3, který se používá ve výchozím nastavení pro jádro EF Core.</span><span class="sxs-lookup"><span data-stu-id="1c897-558">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="1c897-559">Pro jiných případech může být povoleno cizích klíčů tak, že zadáte `Foreign Keys=True` v připojovacím řetězci.</span><span class="sxs-lookup"><span data-stu-id="1c897-559">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="1c897-560">Microsoft.EntityFrameworkCore.Sqlite nyní závisí na SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="1c897-560">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="1c897-561">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-561">**Old behavior**</span></span>

<span data-ttu-id="1c897-562">Před EF Core 3.0 používá EF Core `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="1c897-562">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="1c897-563">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-563">**New behavior**</span></span>

<span data-ttu-id="1c897-564">Od verze EF Core 3.0, pomocí EF Core `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="1c897-564">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="1c897-565">**Proč**</span><span class="sxs-lookup"><span data-stu-id="1c897-565">**Why**</span></span>

<span data-ttu-id="1c897-566">Tato změna byla provedena tak, aby používalo verzi SQLite v Iosu konzistentní s jinými platformami.</span><span class="sxs-lookup"><span data-stu-id="1c897-566">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="1c897-567">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="1c897-567">**Mitigations**</span></span>

<span data-ttu-id="1c897-568">Pokud chcete použít nativní verzi SQLite v Iosu, nakonfigurovat `Microsoft.Data.Sqlite` použít jinou `SQLitePCLRaw` sady.</span><span class="sxs-lookup"><span data-stu-id="1c897-568">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

## <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="1c897-569">Identifikátor GUID hodnoty jsou nyní uloženy jako TEXT na SQLite</span><span class="sxs-lookup"><span data-stu-id="1c897-569">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="1c897-570">Sledování problému #15078</span><span class="sxs-lookup"><span data-stu-id="1c897-570">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="1c897-571">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="1c897-571">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1c897-572">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-572">**Old behavior**</span></span>

<span data-ttu-id="1c897-573">Identifikátor GUID hodnoty byly dříve sored jako hodnoty objektu BLOB na SQLite.</span><span class="sxs-lookup"><span data-stu-id="1c897-573">Guid values were previously sored as BLOB values on SQLite.</span></span>

<span data-ttu-id="1c897-574">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-574">**New behavior**</span></span>

<span data-ttu-id="1c897-575">Identifikátor GUID hodnoty jsou nyní sotred jako TEXT.</span><span class="sxs-lookup"><span data-stu-id="1c897-575">Guid values are now sotred as TEXT.</span></span>

<span data-ttu-id="1c897-576">**Proč**</span><span class="sxs-lookup"><span data-stu-id="1c897-576">**Why**</span></span>

<span data-ttu-id="1c897-577">Binární formát GUID není standardizované.</span><span class="sxs-lookup"><span data-stu-id="1c897-577">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="1c897-578">Uložení hodnot jako TEXT díky databáze více kompatibilní s jinými technologiemi.</span><span class="sxs-lookup"><span data-stu-id="1c897-578">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="1c897-579">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="1c897-579">**Mitigations**</span></span>

<span data-ttu-id="1c897-580">Spuštěním SQL takto můžete migrovat existující databáze na nový formát.</span><span class="sxs-lookup"><span data-stu-id="1c897-580">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="1c897-581">V EF Core můžete také pokračovat pomocí předchozí chování configuirng převaděč hodnoty těchto vlastností.</span><span class="sxs-lookup"><span data-stu-id="1c897-581">In EF Core, you could also continue using the previous behavior by configuirng a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="1c897-582">Microsoft.Data.Sqlite zůstává schopný načíst hodnoty identifikátoru Guid z objektu BLOB a TEXTOVÉHO sloupce; ale vzhledem k tomu, že došlo ke změně výchozího formátu pro parametry a konstant bude pravděpodobně potřeba provést akci pro většinu scénářů zahrnující identifikátory GUID.</span><span class="sxs-lookup"><span data-stu-id="1c897-582">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="1c897-583">Hodnoty char jsou nyní uloženy jako TEXT na SQLite</span><span class="sxs-lookup"><span data-stu-id="1c897-583">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="1c897-584">Sledování problému #15020</span><span class="sxs-lookup"><span data-stu-id="1c897-584">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="1c897-585">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="1c897-585">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1c897-586">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-586">**Old behavior**</span></span>

<span data-ttu-id="1c897-587">Hodnoty char byly dříve sored jako CELOČÍSELNÉ hodnoty na SQLite.</span><span class="sxs-lookup"><span data-stu-id="1c897-587">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="1c897-588">Například znak hodnotu *A* byl uložen jako celočíselnou hodnotu 65.</span><span class="sxs-lookup"><span data-stu-id="1c897-588">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="1c897-589">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-589">**New behavior**</span></span>

<span data-ttu-id="1c897-590">Hodnoty char jsou nyní sotred jako TEXT.</span><span class="sxs-lookup"><span data-stu-id="1c897-590">Char values are now sotred as TEXT.</span></span>

<span data-ttu-id="1c897-591">**Proč**</span><span class="sxs-lookup"><span data-stu-id="1c897-591">**Why**</span></span>

<span data-ttu-id="1c897-592">Uložení hodnot jako TEXT je přirozenější a vytvoří databáze více kompatibilní s jinými technologiemi.</span><span class="sxs-lookup"><span data-stu-id="1c897-592">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="1c897-593">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="1c897-593">**Mitigations**</span></span>

<span data-ttu-id="1c897-594">Spuštěním SQL takto můžete migrovat existující databáze na nový formát.</span><span class="sxs-lookup"><span data-stu-id="1c897-594">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="1c897-595">V EF Core můžete také pokračovat pomocí předchozí chování configuirng převaděč hodnoty těchto vlastností.</span><span class="sxs-lookup"><span data-stu-id="1c897-595">In EF Core, you could also continue using the previous behavior by configuirng a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="1c897-596">Microsoft.Data.Sqlite také zbývá schopný načíst znakových hodnot z celé číslo a TEXT sloupců, takže určitých scénářích nevyžadují žádnou akci.</span><span class="sxs-lookup"><span data-stu-id="1c897-596">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="1c897-597">ID migrace jsou generovány pomocí neutrální jazykové verze kalendáře</span><span class="sxs-lookup"><span data-stu-id="1c897-597">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="1c897-598">Sledování problému #12978</span><span class="sxs-lookup"><span data-stu-id="1c897-598">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="1c897-599">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="1c897-599">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1c897-600">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-600">**Old behavior**</span></span>

<span data-ttu-id="1c897-601">ID migrace byly generovány pomocí kalendář jazykové verze currret neúmyslně.</span><span class="sxs-lookup"><span data-stu-id="1c897-601">Migration IDs were inadvertantly generated using the currret culture's calendar.</span></span>

<span data-ttu-id="1c897-602">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-602">**New behavior**</span></span>

<span data-ttu-id="1c897-603">ID migrace jsou teď vždy generovány pomocí neutrální jazykové verze kalendáře (gregoriánského).</span><span class="sxs-lookup"><span data-stu-id="1c897-603">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="1c897-604">**Proč**</span><span class="sxs-lookup"><span data-stu-id="1c897-604">**Why**</span></span>

<span data-ttu-id="1c897-605">Pořadí migrace je důležité při aktualizaci databáze nebo řešení konfliktů při sloučení.</span><span class="sxs-lookup"><span data-stu-id="1c897-605">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="1c897-606">Pomocí neutrální kalendáře se vyhnete řazení problémy, které můžou být výsledkem členy týmu s jiným kalendáře.</span><span class="sxs-lookup"><span data-stu-id="1c897-606">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="1c897-607">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="1c897-607">**Mitigations**</span></span>

<span data-ttu-id="1c897-608">Tato změna ovlivní těm, kdo používají jiné než gregoriánské kalendářní, kde rok je větší než gregoriánském kalendáři (např. thajský buddhistický kalendář).</span><span class="sxs-lookup"><span data-stu-id="1c897-608">This change affects anyone using a non-Gregorian calender where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="1c897-609">Migrace stávající ID bude potřeba aktualizovat tak, aby nové migrace jsou řazeny za stávající migrace.</span><span class="sxs-lookup"><span data-stu-id="1c897-609">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="1c897-610">ID migrace najdete v atributu migrace soubory návrháře migrace.</span><span class="sxs-lookup"><span data-stu-id="1c897-610">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="1c897-611">Tabulky historie migrace je také potřeba aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="1c897-611">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

## <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="1c897-612">LogQueryPossibleExceptionWithAggregateOperator byl přejmenován.</span><span class="sxs-lookup"><span data-stu-id="1c897-612">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="1c897-613">Sledování problému #10985</span><span class="sxs-lookup"><span data-stu-id="1c897-613">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="1c897-614">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="1c897-614">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1c897-615">**Změna**</span><span class="sxs-lookup"><span data-stu-id="1c897-615">**Change**</span></span>

<span data-ttu-id="1c897-616">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` byl přejmenován na `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="1c897-616">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="1c897-617">**Proč**</span><span class="sxs-lookup"><span data-stu-id="1c897-617">**Why**</span></span>

<span data-ttu-id="1c897-618">Zarovná pojmenování Tato událost upozornění s jinými událostmi upozornění.</span><span class="sxs-lookup"><span data-stu-id="1c897-618">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="1c897-619">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="1c897-619">**Mitigations**</span></span>

<span data-ttu-id="1c897-620">Použití nového názvu.</span><span class="sxs-lookup"><span data-stu-id="1c897-620">Use the new name.</span></span> <span data-ttu-id="1c897-621">(Všimněte si, že nedošlo ke změně číslo ID události.)</span><span class="sxs-lookup"><span data-stu-id="1c897-621">(Note that the event ID number has not changed.)</span></span>

## <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="1c897-622">Vysvětlení rozhraní API pro názvy omezení pro cizí klíč</span><span class="sxs-lookup"><span data-stu-id="1c897-622">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="1c897-623">Sledování problému #10730</span><span class="sxs-lookup"><span data-stu-id="1c897-623">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="1c897-624">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="1c897-624">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1c897-625">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-625">**Old behavior**</span></span>

<span data-ttu-id="1c897-626">Před EF Core 3.0 omezení pro cizí klíč názvy označovaly jako jednoduše "name".</span><span class="sxs-lookup"><span data-stu-id="1c897-626">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="1c897-627">Příklad:</span><span class="sxs-lookup"><span data-stu-id="1c897-627">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="1c897-628">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="1c897-628">**New behavior**</span></span>

<span data-ttu-id="1c897-629">Od verze EF Core 3.0, omezení pro cizí klíč názvy jsou dnes označovány jako "kruhového name".</span><span class="sxs-lookup"><span data-stu-id="1c897-629">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constaint name".</span></span> <span data-ttu-id="1c897-630">Příklad:</span><span class="sxs-lookup"><span data-stu-id="1c897-630">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="1c897-631">**Proč**</span><span class="sxs-lookup"><span data-stu-id="1c897-631">**Why**</span></span>

<span data-ttu-id="1c897-632">Tato změna přináší konzistenci pro názvy v této oblasti a také vysvětluje, že se jedná o název název cizího klíče kruhového a nikoli na sloupec nebo vlastnost, která je definována cizího klíče na.</span><span class="sxs-lookup"><span data-stu-id="1c897-632">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constaint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="1c897-633">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="1c897-633">**Mitigations**</span></span>

<span data-ttu-id="1c897-634">Použití nového názvu.</span><span class="sxs-lookup"><span data-stu-id="1c897-634">Use the new name.</span></span>
