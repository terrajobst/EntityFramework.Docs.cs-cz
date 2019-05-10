---
title: Zásadní změny v EF Core 3.0 – EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: b1b5e286e08a8b6b4efe225a176e76023f9fdd20
ms.sourcegitcommit: 960e42a01b3a2f76da82e074f64f52252a8afecc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/08/2019
ms.locfileid: "65405232"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="9a26f-102">Rozbíjející změny zahrnuté v EF Core 3.0 (aktuálně ve verzi preview)</span><span class="sxs-lookup"><span data-stu-id="9a26f-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9a26f-103">Mějte prosím na paměti, že sady funkcí a plány budoucích verzí se vždy mohou změnit a přestože se snažíme se zachovat aktuální, nemusí odrážet naše nejnovější plány vůbec na této stránce vyprší.</span><span class="sxs-lookup"><span data-stu-id="9a26f-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="9a26f-104">Následující změny chování a rozhraní API by mohly přerušit aplikace vyvinuté pro jádro EF Core 2.2.x při upgradu na 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="9a26f-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="9a26f-105">Změny, které chceme ovlivní jen poskytovatelé databází jsou popsány v části [poskytovatel změny](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="9a26f-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="9a26f-106">Tady nejsou uvedené konce v nové vlastnosti představené z jednoho 3.0 ve verzi preview další 3.0 ve verzi Preview.</span><span class="sxs-lookup"><span data-stu-id="9a26f-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="9a26f-107">Dotazy LINQ se už nevyhodnocuje na straně klienta</span><span class="sxs-lookup"><span data-stu-id="9a26f-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="9a26f-108">[Sledování problému #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[také zjistit problém #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="9a26f-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="9a26f-109">Tato změna bude zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="9a26f-109">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="9a26f-110">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-110">**Old behavior**</span></span>

<span data-ttu-id="9a26f-111">Než 3.0 když EF Core nelze převést výraz, který byl součástí sady dotazů SQL nebo parametr, automaticky vyhodnotí výraz na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="9a26f-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="9a26f-112">Ve výchozím nastavení hodnocení klientů potencionálně nákladným výrazů aktivuje pouze upozornění.</span><span class="sxs-lookup"><span data-stu-id="9a26f-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="9a26f-113">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-113">**New behavior**</span></span>

<span data-ttu-id="9a26f-114">Od verze 3.0, EF Core umožňuje pouze výrazy v projekci nejvyšší úrovně (poslední `Select()` volání v dotazu) k vyhodnocení na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="9a26f-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="9a26f-115">Když výrazy v další části dotazu nelze převést na SQL nebo parametr, je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="9a26f-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="9a26f-116">**Proč**</span><span class="sxs-lookup"><span data-stu-id="9a26f-116">**Why**</span></span>

<span data-ttu-id="9a26f-117">Vyhodnocení automatickou klientskou dotazů umožňuje mnoho dotazů, který se spustí i v případě, že je důležité části nelze přeložit.</span><span class="sxs-lookup"><span data-stu-id="9a26f-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="9a26f-118">Toto chování může způsobit neočekávané a potenciálně škodlivé chování, které může přestat pouze zřejmé v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="9a26f-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="9a26f-119">Například podmínku v `Where()` volání, které nelze přeložit může způsobit, že všechny řádky z tabulky přesunou z databázového serveru a filtr, který bude použit na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="9a26f-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="9a26f-120">Tato situace může pokračovat nezjištěné po snadno Pokud tabulka obsahuje jenom pár řádků v vývoje, ale přístupů pevné přesun aplikace do produkčního prostředí, kde tabulka může obsahovat miliony řádků.</span><span class="sxs-lookup"><span data-stu-id="9a26f-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="9a26f-121">Upozornění vyhodnocení klienta dokázaly také příliš jednoduché ignorovat během vývoje.</span><span class="sxs-lookup"><span data-stu-id="9a26f-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="9a26f-122">Kromě toho může automatickou klientskou vyhodnocení způsobit problémy, ve kterých vylepšení překladu dotazu pro konkrétní výrazy způsobila neúmyslnému zásadní změny mezi verzí.</span><span class="sxs-lookup"><span data-stu-id="9a26f-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="9a26f-123">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="9a26f-123">**Mitigations**</span></span>

<span data-ttu-id="9a26f-124">Pokud dotaz nelze přeložit plně, pak buď přepište dotaz ve formuláři, který lze přeložit, nebo použijte `AsEnumerable()`, `ToList()`, nebo podobný explicitně přenést data zpět do klienta ve kterém pak může být dále zpracovány pomocí LINQ na objekty.</span><span class="sxs-lookup"><span data-stu-id="9a26f-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="9a26f-125">Entity Framework Core už nejsou součástí sdíleného rozhraní ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9a26f-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="9a26f-126">Oznámení týkající se sledování problému #325</span><span class="sxs-lookup"><span data-stu-id="9a26f-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="9a26f-127">Tato změna byla zavedena v ASP.NET Core 3.0 ve verzi preview 1.</span><span class="sxs-lookup"><span data-stu-id="9a26f-127">This change was introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="9a26f-128">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-128">**Old behavior**</span></span>

<span data-ttu-id="9a26f-129">Před ASP.NET Core 3.0, když se přidá odkaz na balíček pro `Microsoft.AspNetCore.App` nebo `Microsoft.AspNetCore.All`, měl by obsahovat EF Core a některé EF Core zprostředkovatele dat, jako jsou zprostředkovatele SQL Server.</span><span class="sxs-lookup"><span data-stu-id="9a26f-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="9a26f-130">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-130">**New behavior**</span></span>

<span data-ttu-id="9a26f-131">Od verze 3.0, rozhraní ASP.NET Core sdílené neobsahuje EF Core nebo žádným zprostředkovatelům dat. EF Core.</span><span class="sxs-lookup"><span data-stu-id="9a26f-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="9a26f-132">**Proč**</span><span class="sxs-lookup"><span data-stu-id="9a26f-132">**Why**</span></span>

<span data-ttu-id="9a26f-133">Před touto změnou získávání EF Core vyžaduje jiný postup v závislosti na tom, jestli aplikace cílené ASP.NET Core a SQL Server, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="9a26f-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="9a26f-134">Upgrade ASP.NET Core také vynutit upgrade EF Core a zprostředkovatele SQL Server, který nemusí být vždy žádoucí.</span><span class="sxs-lookup"><span data-stu-id="9a26f-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="9a26f-135">Díky této změně se možnosti načítání EF Core je stejná ve všech zprostředkovatelů, podporovaná implementace .NET a typy aplikací.</span><span class="sxs-lookup"><span data-stu-id="9a26f-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="9a26f-136">Vývojáři také nyní mohou ovládat přesně při upgradu EF Core a EF Core zprostředkovatelé dat.</span><span class="sxs-lookup"><span data-stu-id="9a26f-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="9a26f-137">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="9a26f-137">**Mitigations**</span></span>

<span data-ttu-id="9a26f-138">V aplikaci ASP.NET Core 3.0 nebo jakékoli jiné podporované aplikace použít EF Core, explicitně přidáte odkaz na balíček k poskytovateli databáze EF Core, který bude aplikace používat.</span><span class="sxs-lookup"><span data-stu-id="9a26f-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="9a26f-139">EF Core nástroji příkazového řádku dotnet ef, už nejsou součástí sady .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="9a26f-139">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="9a26f-140">Sledování problému #14016</span><span class="sxs-lookup"><span data-stu-id="9a26f-140">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="9a26f-141">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 4 a odpovídající verze sady .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="9a26f-141">This change was introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="9a26f-142">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-142">**Old behavior**</span></span>

<span data-ttu-id="9a26f-143">Než 3.0 `dotnet ef` nástroj je zahrnutý v .NET Core SDK a se snadno k dispozici pro použití z příkazového řádku z libovolného projektu bez nutnosti pár kroků navíc.</span><span class="sxs-lookup"><span data-stu-id="9a26f-143">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="9a26f-144">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-144">**New behavior**</span></span>

<span data-ttu-id="9a26f-145">Od verze 3.0, sady .NET SDK nepodporuje incude `dotnet ef` nástroj, takže než budete moct použít, budete mít explicitně ji nainstalovat jako nástroj pro místní nebo globální.</span><span class="sxs-lookup"><span data-stu-id="9a26f-145">Starting in 3.0, the .NET SDK does not incude the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="9a26f-146">**Proč**</span><span class="sxs-lookup"><span data-stu-id="9a26f-146">**Why**</span></span>

<span data-ttu-id="9a26f-147">Tato změna umožňuje distribuci a aktualizaci `dotnet ef` jako regulární nástroj rozhraní příkazového řádku .NET na webu NuGet, konzistentní s skutečnost, že je EF Core 3.0 také vždy distribuován jako balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="9a26f-147">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="9a26f-148">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="9a26f-148">**Mitigations**</span></span>

<span data-ttu-id="9a26f-149">Abyste mohli spravovat migrace nebo vygenerované uživatelské rozhraní `DbContext`, nainstalovat `dotnet-ef` pomocí `dotnet tool install` příkazu.</span><span class="sxs-lookup"><span data-stu-id="9a26f-149">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` using the `dotnet tool install` command.</span></span>
<span data-ttu-id="9a26f-150">Například můžete ji nainstalovat jako globální nástroj, zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="9a26f-150">For example, to install it as a global tool, you can type this command:</span></span>

  ``` console
  $ dotnet tool install --global dotnet-ef --version <exact-version>
  ```

<span data-ttu-id="9a26f-151">Můžete také získat jeho místní nástroj při obnovování závislosti projektu, který deklaruje jako závislost pomocí nástroje [soubor manifestu nástroje](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="9a26f-151">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

## <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="9a26f-152">Byla přejmenovaná FromSql ExecuteSql a ExecuteSqlAsync</span><span class="sxs-lookup"><span data-stu-id="9a26f-152">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="9a26f-153">Sledování problému #10996</span><span class="sxs-lookup"><span data-stu-id="9a26f-153">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="9a26f-154">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="9a26f-154">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="9a26f-155">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-155">**Old behavior**</span></span>

<span data-ttu-id="9a26f-156">Před EF Core 3.0 byly tyto názvy metod přetížení pro práci s normální řetězec nebo řetězec, který by měl být interpolovaných do SQL a parametry.</span><span class="sxs-lookup"><span data-stu-id="9a26f-156">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="9a26f-157">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-157">**New behavior**</span></span>

<span data-ttu-id="9a26f-158">Od verze EF Core 3.0, použijte `FromSqlRaw`, `ExecuteSqlRaw`, a `ExecuteSqlRawAsync` vytvořit parametrický dotaz, kde parametry jsou předány samostatně z řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="9a26f-158">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="9a26f-159">Příklad:</span><span class="sxs-lookup"><span data-stu-id="9a26f-159">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="9a26f-160">Použití `FromSqlInterpolated`, `ExecuteSqlInterpolated`, a `ExecuteSqlInterpolatedAsync` vytvořit parametrický dotaz, kde parametry jsou předány jako součást dotazu interpolované řetězce.</span><span class="sxs-lookup"><span data-stu-id="9a26f-160">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="9a26f-161">Příklad:</span><span class="sxs-lookup"><span data-stu-id="9a26f-161">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="9a26f-162">Všimněte si, že oba dotazy výše uvedené vytvoří stejný parametrizovaného dotazu SQL se stejnými parametry SQL.</span><span class="sxs-lookup"><span data-stu-id="9a26f-162">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="9a26f-163">**Proč**</span><span class="sxs-lookup"><span data-stu-id="9a26f-163">**Why**</span></span>

<span data-ttu-id="9a26f-164">Přetížení metody, jako je to velmi usnadňují omylem volat metodu nezpracovaného řetězce, pokud bylo záměrem pro volání metody interpolovaný řetězec a naopak.</span><span class="sxs-lookup"><span data-stu-id="9a26f-164">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="9a26f-165">To může vést k dotazům naprostou když měla být parametrizovány.</span><span class="sxs-lookup"><span data-stu-id="9a26f-165">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="9a26f-166">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="9a26f-166">**Mitigations**</span></span>

<span data-ttu-id="9a26f-167">Přepněte na nové názvy metod.</span><span class="sxs-lookup"><span data-stu-id="9a26f-167">Switch to use the new method names.</span></span>

## <a name="query-execution-is-logged-at-debug-level"></a><span data-ttu-id="9a26f-168">Provádění dotazu se protokoluje při ladění na úrovni</span><span class="sxs-lookup"><span data-stu-id="9a26f-168">Query execution is logged at Debug level</span></span>

[<span data-ttu-id="9a26f-169">Sledování problému #14523</span><span class="sxs-lookup"><span data-stu-id="9a26f-169">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="9a26f-170">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="9a26f-170">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="9a26f-171">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-171">**Old behavior**</span></span>

<span data-ttu-id="9a26f-172">Před EF Core 3.0, provádění dotazů a jiných příkazů protokolu byla zaznamenána v `Info` úroveň.</span><span class="sxs-lookup"><span data-stu-id="9a26f-172">Before EF Core 3.0, execution of queries and other commands was logged at the `Info` level.</span></span>

<span data-ttu-id="9a26f-173">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-173">**New behavior**</span></span>

<span data-ttu-id="9a26f-174">Od verze EF Core 3.0, protokolování spuštění příkazu/SQL je na `Debug` úroveň.</span><span class="sxs-lookup"><span data-stu-id="9a26f-174">Starting with EF Core 3.0, logging of command/SQL execution is at the `Debug` level.</span></span>

<span data-ttu-id="9a26f-175">**Proč**</span><span class="sxs-lookup"><span data-stu-id="9a26f-175">**Why**</span></span>

<span data-ttu-id="9a26f-176">Tato změna byla provedena jak snížit šum na `Info` úrovně protokolování.</span><span class="sxs-lookup"><span data-stu-id="9a26f-176">This change was made to reduce the noise at the `Info` log level.</span></span>

<span data-ttu-id="9a26f-177">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="9a26f-177">**Mitigations**</span></span>

<span data-ttu-id="9a26f-178">Tato událost protokolování je definována `RelationalEventId.CommandExecuting` s ID události 20100.</span><span class="sxs-lookup"><span data-stu-id="9a26f-178">This logging event is defined by `RelationalEventId.CommandExecuting` with event ID 20100.</span></span>
<span data-ttu-id="9a26f-179">Do protokolu SQL na `Info` úroveň znovu, explicitně nakonfigurovat na úrovni `OnConfiguring` nebo `AddDbContext`.</span><span class="sxs-lookup"><span data-stu-id="9a26f-179">To log SQL at the `Info` level again, explicitly configure the level in `OnConfiguring` or `AddDbContext`.</span></span>
<span data-ttu-id="9a26f-180">Příklad:</span><span class="sxs-lookup"><span data-stu-id="9a26f-180">For example:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Info)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="9a26f-181">Dočasné hodnoty klíče už nejsou nastavené na instancí entit</span><span class="sxs-lookup"><span data-stu-id="9a26f-181">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="9a26f-182">Sledování problému #12378</span><span class="sxs-lookup"><span data-stu-id="9a26f-182">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="9a26f-183">Tato změna byla zavedená v EF Core 3.0 – náhled 2.</span><span class="sxs-lookup"><span data-stu-id="9a26f-183">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="9a26f-184">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-184">**Old behavior**</span></span>

<span data-ttu-id="9a26f-185">Před EF Core 3.0 dočasné hodnoty přiřadily se všechny vlastnosti klíče, které by později skutečné hodnoty generován databází.</span><span class="sxs-lookup"><span data-stu-id="9a26f-185">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="9a26f-186">Obvykle tyto dočasné hodnoty byly velké záporná čísla.</span><span class="sxs-lookup"><span data-stu-id="9a26f-186">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="9a26f-187">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-187">**New behavior**</span></span>

<span data-ttu-id="9a26f-188">Od verze 3.0, EF Core ukládá dočasné hodnotu klíče jako součást informací o sledování entity a ponechá samotné beze změny vlastnosti klíče.</span><span class="sxs-lookup"><span data-stu-id="9a26f-188">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="9a26f-189">**Proč**</span><span class="sxs-lookup"><span data-stu-id="9a26f-189">**Why**</span></span>

<span data-ttu-id="9a26f-190">Tato změna byla provedena zabránit chybně stávají trvalé při entita, která byla dříve sledován pomocí funkce některé dočasné hodnoty klíče `DbContext` instance se přesune na jiný `DbContext` instance.</span><span class="sxs-lookup"><span data-stu-id="9a26f-190">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="9a26f-191">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="9a26f-191">**Mitigations**</span></span>

<span data-ttu-id="9a26f-192">Na staré chování může záviset aplikace, které přiřazují hodnoty primárního klíče na cizí klíče formuláře přidružení mezi entitami, pokud primární klíče generované úložištěm a patří do entity v `Added` stavu.</span><span class="sxs-lookup"><span data-stu-id="9a26f-192">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="9a26f-193">Toho se lze vyvarovat podle:</span><span class="sxs-lookup"><span data-stu-id="9a26f-193">This can be avoided by:</span></span>
* <span data-ttu-id="9a26f-194">Bez použití klíče generované úložištěm.</span><span class="sxs-lookup"><span data-stu-id="9a26f-194">Not using store-generated keys.</span></span>
* <span data-ttu-id="9a26f-195">Nastavení vlastnosti navigace na vztahů namísto nastavování hodnot cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="9a26f-195">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="9a26f-196">Získejte skutečné hodnoty dočasné klíče z informací o sledování entity.</span><span class="sxs-lookup"><span data-stu-id="9a26f-196">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="9a26f-197">Například `context.Entry(blog).Property(e => e.Id).CurrentValue` i v případě vrátí hodnotu dočasné `blog.Id` samotný nebyla nastavena.</span><span class="sxs-lookup"><span data-stu-id="9a26f-197">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="9a26f-198">Metoda DetectChanges respektuje klíčových hodnot generovaných úložištěm</span><span class="sxs-lookup"><span data-stu-id="9a26f-198">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="9a26f-199">Sledování problému #14616</span><span class="sxs-lookup"><span data-stu-id="9a26f-199">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="9a26f-200">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="9a26f-200">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="9a26f-201">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-201">**Old behavior**</span></span>

<span data-ttu-id="9a26f-202">Před EF Core 3.0 Nesledované entity objevila `DetectChanges` mají sledovat v `Added` stavu a vložené jako nový řádek, kdy `SaveChanges` je volána.</span><span class="sxs-lookup"><span data-stu-id="9a26f-202">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="9a26f-203">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-203">**New behavior**</span></span>

<span data-ttu-id="9a26f-204">Od verze EF Core 3.0, pokud používá entity generované hodnoty klíče a některá z hodnot klíče je nastavena, pak entity se bude sledovat v `Modified` stavu.</span><span class="sxs-lookup"><span data-stu-id="9a26f-204">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="9a26f-205">To znamená, že řádek entity se předpokládá, že existují a budou aktualizace `SaveChanges` je volána.</span><span class="sxs-lookup"><span data-stu-id="9a26f-205">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="9a26f-206">Pokud není nastavena hodnota klíče, nebo pokud není typ entity pomocí vygenerované klíče, pak nová entita se bude dál sledovat jako `Added` stejně jako v předchozích verzích.</span><span class="sxs-lookup"><span data-stu-id="9a26f-206">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="9a26f-207">**Proč**</span><span class="sxs-lookup"><span data-stu-id="9a26f-207">**Why**</span></span>

<span data-ttu-id="9a26f-208">Tato změna byla provedena na umožňují snadněji a konzistentnější pro práci s grafy odpojené entity při použití klíče generované úložištěm.</span><span class="sxs-lookup"><span data-stu-id="9a26f-208">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="9a26f-209">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="9a26f-209">**Mitigations**</span></span>

<span data-ttu-id="9a26f-210">Tato změna může přerušit aplikace, pokud typ entity je nakonfigurovaný na použití vygenerovat klíče, ale hodnoty klíče jsou explicitně nastavené pro nové instance.</span><span class="sxs-lookup"><span data-stu-id="9a26f-210">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="9a26f-211">Oprava je explicitně konfigurovat klíčové vlastnosti nejsou generovány pomocí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="9a26f-211">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="9a26f-212">Například pomocí rozhraní fluent API:</span><span class="sxs-lookup"><span data-stu-id="9a26f-212">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="9a26f-213">Nebo s anotacemi dat:</span><span class="sxs-lookup"><span data-stu-id="9a26f-213">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="9a26f-214">Kaskádové odstranění teď dojde okamžitě ve výchozím nastavení</span><span class="sxs-lookup"><span data-stu-id="9a26f-214">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="9a26f-215">Sledování problému #10114</span><span class="sxs-lookup"><span data-stu-id="9a26f-215">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="9a26f-216">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="9a26f-216">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="9a26f-217">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-217">**Old behavior**</span></span>

<span data-ttu-id="9a26f-218">Než 3.0 EF Core použít kaskádové akce (odstranění závislých položek při odstranění požadovaný instanční objekt nebo když porušeno vztah k požadované instanční objekt) nebyly provedeny dokud byla volána metoda SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="9a26f-218">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="9a26f-219">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-219">**New behavior**</span></span>

<span data-ttu-id="9a26f-220">Od verze 3.0, EF Core provede kaskádové akce ihned po zjištění spouštěcí podmínky.</span><span class="sxs-lookup"><span data-stu-id="9a26f-220">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="9a26f-221">Například volání `context.Remove()` odstranit instančního objektu entity se výsledek ve všech sledovat související požadované položky závislé na také nastavena na `Deleted` okamžitě.</span><span class="sxs-lookup"><span data-stu-id="9a26f-221">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="9a26f-222">**Proč**</span><span class="sxs-lookup"><span data-stu-id="9a26f-222">**Why**</span></span>

<span data-ttu-id="9a26f-223">Tato změna byla provedena na zlepšení uživatelského rozhraní pro datových vazbách a auditování scénáře, kdy je potřeba vysvětlit entit, které se odstraní _před_ `SaveChanges` je volána.</span><span class="sxs-lookup"><span data-stu-id="9a26f-223">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="9a26f-224">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="9a26f-224">**Mitigations**</span></span>

<span data-ttu-id="9a26f-225">Předchozí chování lze obnovit nastavením na `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="9a26f-225">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="9a26f-226">Příklad:</span><span class="sxs-lookup"><span data-stu-id="9a26f-226">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="9a26f-227">DeleteBehavior.Restrict má sémantiku čisticího modulu</span><span class="sxs-lookup"><span data-stu-id="9a26f-227">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="9a26f-228">Sledování problému #12661</span><span class="sxs-lookup"><span data-stu-id="9a26f-228">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="9a26f-229">Tato změna bude zavedená v EF Core 3.0 – ve verzi preview 5.</span><span class="sxs-lookup"><span data-stu-id="9a26f-229">This change will be introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="9a26f-230">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-230">**Old behavior**</span></span>

<span data-ttu-id="9a26f-231">Než 3.0 `DeleteBehavior.Restrict` vytvořit cizí klíče v databázi s `Restrict` sémantiku, ale také změněné interní opravy není zřejmé způsobem.</span><span class="sxs-lookup"><span data-stu-id="9a26f-231">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="9a26f-232">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-232">**New behavior**</span></span>

<span data-ttu-id="9a26f-233">Od verze 3.0, `DeleteBehavior.Restrict` zajistí, že cizí klíče jsou vytvořeny pomocí `Restrict` sémantiku – tedy žádná cascades; throw na porušení omezení – bez vliv i na EF interní opravy.</span><span class="sxs-lookup"><span data-stu-id="9a26f-233">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="9a26f-234">**Proč**</span><span class="sxs-lookup"><span data-stu-id="9a26f-234">**Why**</span></span>

<span data-ttu-id="9a26f-235">Tato změna byla provedena na zlepšení uživatelského rozhraní pro použití `DeleteBehavior` intuitivní způsobem, bez neočekávané vedlejší účinky.</span><span class="sxs-lookup"><span data-stu-id="9a26f-235">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="9a26f-236">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="9a26f-236">**Mitigations**</span></span>

<span data-ttu-id="9a26f-237">Předchozí chování můžete obnovit pomocí `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="9a26f-237">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="9a26f-238">Typy dotazů jsou spojeny s typy entit</span><span class="sxs-lookup"><span data-stu-id="9a26f-238">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="9a26f-239">Sledování problému #14194</span><span class="sxs-lookup"><span data-stu-id="9a26f-239">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="9a26f-240">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="9a26f-240">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="9a26f-241">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-241">**Old behavior**</span></span>

<span data-ttu-id="9a26f-242">Před EF Core 3.0 [typy dotazů](xref:core/modeling/query-types) byly prostředky provádět dotazy na data, která nedefinuje primární klíč strukturovaným způsobem.</span><span class="sxs-lookup"><span data-stu-id="9a26f-242">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="9a26f-243">To znamená typ dotazu byla použita pro mapování typů entit bez klíčů (pravděpodobně ze zobrazení, ale pravděpodobně z tabulky) při pravidelných entity typ byl použit při klíč byl k dispozici (více pravděpodobně z tabulky, ale pravděpodobně ze zobrazení).</span><span class="sxs-lookup"><span data-stu-id="9a26f-243">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="9a26f-244">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-244">**New behavior**</span></span>

<span data-ttu-id="9a26f-245">Typ dotazu teď bude pouze typ entity bez primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="9a26f-245">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="9a26f-246">Typy entit bez kódu mají stejné funkce jako typy dotazů v předchozích verzích.</span><span class="sxs-lookup"><span data-stu-id="9a26f-246">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="9a26f-247">**Proč**</span><span class="sxs-lookup"><span data-stu-id="9a26f-247">**Why**</span></span>

<span data-ttu-id="9a26f-248">Tato změna byla provedena, abyste snížili nejasnosti kolem účel typy dotazů.</span><span class="sxs-lookup"><span data-stu-id="9a26f-248">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="9a26f-249">Konkrétně jsou typy bez klíčů entit a z tohoto důvodu jsou ze své podstaty jen pro čtení, ale by neměl být použit pouze z důvodu typu entity musí být jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="9a26f-249">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="9a26f-250">Podobně jsou často namapované na zobrazení, ale je to jenom, protože zobrazení není často definují klíče.</span><span class="sxs-lookup"><span data-stu-id="9a26f-250">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="9a26f-251">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="9a26f-251">**Mitigations**</span></span>

<span data-ttu-id="9a26f-252">Následující části rozhraní API jsou teď zastaralé:</span><span class="sxs-lookup"><span data-stu-id="9a26f-252">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="9a26f-253">**`ModelBuilder.Query<>()`** – Místo toho `ModelBuilder.Entity<>().HasNoKey()` musí být volána k označení typu entity tak, že má žádné klíče.</span><span class="sxs-lookup"><span data-stu-id="9a26f-253">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="9a26f-254">To by stále probíhá konfigurace podle konvence se vyhnete chybné konfigurace, když primární klíč se očekává, ale neodpovídá konvenci.</span><span class="sxs-lookup"><span data-stu-id="9a26f-254">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="9a26f-255">**`DbQuery<>`** – Místo toho `DbSet<>` by měla sloužit.</span><span class="sxs-lookup"><span data-stu-id="9a26f-255">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="9a26f-256">**`DbContext.Query<>()`** – Místo toho `DbContext.Set<>()` by měla sloužit.</span><span class="sxs-lookup"><span data-stu-id="9a26f-256">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="9a26f-257">Došlo ke změně konfigurace rozhraní API pro vlastní typ relace</span><span class="sxs-lookup"><span data-stu-id="9a26f-257">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="9a26f-258">[Sledování problému #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[sledování problému #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[sledování problému #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="9a26f-258">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="9a26f-259">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="9a26f-259">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="9a26f-260">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-260">**Old behavior**</span></span>

<span data-ttu-id="9a26f-261">Před EF Core 3.0 byla provedena konfigurace vlastněné vztah přímo po `OwnsOne` nebo `OwnsMany` volání.</span><span class="sxs-lookup"><span data-stu-id="9a26f-261">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="9a26f-262">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-262">**New behavior**</span></span>

<span data-ttu-id="9a26f-263">Od verze EF Core 3.0, že už fluent API pro konfiguraci vlastnosti navigace vlastníkovi použitím `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="9a26f-263">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="9a26f-264">Příklad:</span><span class="sxs-lookup"><span data-stu-id="9a26f-264">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="9a26f-265">Konfigurace související se o vztah mezi vlastníka a vlastní by měl být zřetězené teď po `WithOwner()` podobně jako na konfiguraci jiné vztahy.</span><span class="sxs-lookup"><span data-stu-id="9a26f-265">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="9a26f-266">Při konfiguraci pro vlastní typ, samotný by stále možné zřetězit po `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="9a26f-266">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="9a26f-267">Příklad:</span><span class="sxs-lookup"><span data-stu-id="9a26f-267">For example:</span></span>

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

<span data-ttu-id="9a26f-268">Kromě volá `Entity()`, `HasOne()`, nebo `Set()` s typem vlastnictví cíl se vyvolají výjimku.</span><span class="sxs-lookup"><span data-stu-id="9a26f-268">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="9a26f-269">**Proč**</span><span class="sxs-lookup"><span data-stu-id="9a26f-269">**Why**</span></span>

<span data-ttu-id="9a26f-270">Tato změna byla provedena vytvořit jasnější oddělení mezi konfigurací samotného typu vlastnictví a _vztah k_ typ vlastnictví.</span><span class="sxs-lookup"><span data-stu-id="9a26f-270">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="9a26f-271">Tím zase nejednoznačnosti a záměny kolem metody, jako je `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="9a26f-271">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="9a26f-272">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="9a26f-272">**Mitigations**</span></span>

<span data-ttu-id="9a26f-273">Změna konfigurace vlastněné typ vztahů používat nové plochy rozhraní API, jak je znázorněno v příkladu výše.</span><span class="sxs-lookup"><span data-stu-id="9a26f-273">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="9a26f-274">Závislých položek sdílení v tabulce k objektu zabezpečení jsou teď nepovinné.</span><span class="sxs-lookup"><span data-stu-id="9a26f-274">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="9a26f-275">Sledování problému #9005</span><span class="sxs-lookup"><span data-stu-id="9a26f-275">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="9a26f-276">Tato změna bude zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="9a26f-276">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="9a26f-277">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-277">**Old behavior**</span></span>

<span data-ttu-id="9a26f-278">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="9a26f-278">Consider the following model:</span></span>
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
<span data-ttu-id="9a26f-279">Před EF Core 3.0, pokud `OrderDetails` vlastní `Order` nebo explicitně namapované na stejnou tabulku a `OrderDetails` instance byla požadována vždy při přidání nového `Order`.</span><span class="sxs-lookup"><span data-stu-id="9a26f-279">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="9a26f-280">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-280">**New behavior**</span></span>

<span data-ttu-id="9a26f-281">Od verze 3.0, EF Core umožňuje přidat `Order` bez `OrderDetails` a mapuje všechny `OrderDetails` vlastnosti s výjimkou primární klíč pro sloupce s možnou hodnotou Null.</span><span class="sxs-lookup"><span data-stu-id="9a26f-281">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="9a26f-282">Při dotazování na EF Core sady `OrderDetails` k `null` Pokud některou z jejích požadovaných vlastností nemá žádnou hodnotu nebo nemá žádné požadované vlastnosti kromě primárního klíče a všechny vlastnosti jsou `null`.</span><span class="sxs-lookup"><span data-stu-id="9a26f-282">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="9a26f-283">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="9a26f-283">**Mitigations**</span></span>

<span data-ttu-id="9a26f-284">Pokud váš model obsahuje tabulku sdílení závislé se všemi sloupci volitelné, ale navigace ukazatel není má být `null` pak aplikace by měla upravit tak, aby zpracovat případy při navigaci `null`.</span><span class="sxs-lookup"><span data-stu-id="9a26f-284">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="9a26f-285">Pokud to není možné požadovanou vlastnost měla být přidána do typu entity nebo musí mít alespoň jednu vlastnost non -`null` přiřazena hodnota.</span><span class="sxs-lookup"><span data-stu-id="9a26f-285">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

## <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="9a26f-286">Všechny entity sdílení tabulku se sloupcem tokenů souběžnosti mít mapování na vlastnost</span><span class="sxs-lookup"><span data-stu-id="9a26f-286">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="9a26f-287">Sledování problému #14154</span><span class="sxs-lookup"><span data-stu-id="9a26f-287">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="9a26f-288">Tato změna bude zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="9a26f-288">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="9a26f-289">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-289">**Old behavior**</span></span>

<span data-ttu-id="9a26f-290">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="9a26f-290">Consider the following model:</span></span>
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
<span data-ttu-id="9a26f-291">Před EF Core 3.0, pokud `OrderDetails` vlastní `Order` nebo explicitně namapované na stejnou tabulku následně aktualizovat jenom `OrderDetails` neaktualizuje `Version` hodnoty na klientovi a příští aktualizace se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="9a26f-291">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="9a26f-292">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-292">**New behavior**</span></span>

<span data-ttu-id="9a26f-293">Od verze 3.0, rozšíří EF Core nové `Version` hodnota, která se `Order` Pokud vlastní `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="9a26f-293">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="9a26f-294">V opačném případě dojde k výjimce během ověření modelu.</span><span class="sxs-lookup"><span data-stu-id="9a26f-294">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="9a26f-295">**Proč**</span><span class="sxs-lookup"><span data-stu-id="9a26f-295">**Why**</span></span>

<span data-ttu-id="9a26f-296">Tato změna byla provedena, když se aktualizuje jenom jeden z entity, které jsou namapované na stejnou tabulku, aby hodnota tokenu zastaralé souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="9a26f-296">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="9a26f-297">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="9a26f-297">**Mitigations**</span></span>

<span data-ttu-id="9a26f-298">Všechny entity v tabulce pro sdílení obsahu se mají zahrnout vlastnost, která je namapovaná na sloupci tokenů souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="9a26f-298">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="9a26f-299">Je možné vytvořit, jeden v stínové stavu:</span><span class="sxs-lookup"><span data-stu-id="9a26f-299">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

## <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="9a26f-300">Vlastnosti zděděné nenamapované typy jsou nyní mapovány do jednoho sloupce pro všechny odvozené typy</span><span class="sxs-lookup"><span data-stu-id="9a26f-300">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="9a26f-301">Sledování problému #13998</span><span class="sxs-lookup"><span data-stu-id="9a26f-301">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="9a26f-302">Tato změna bude zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="9a26f-302">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="9a26f-303">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-303">**Old behavior**</span></span>

<span data-ttu-id="9a26f-304">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="9a26f-304">Consider the following model:</span></span>
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

<span data-ttu-id="9a26f-305">Před EF Core 3.0 `ShippingAddress` vlastnost by být mapována k oddělení sloupců pro `BulkOrder` a `Order` ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="9a26f-305">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="9a26f-306">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-306">**New behavior**</span></span>

<span data-ttu-id="9a26f-307">Od verze 3.0, EF Core vytvoří pouze jeden sloupec pro `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="9a26f-307">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="9a26f-308">**Proč**</span><span class="sxs-lookup"><span data-stu-id="9a26f-308">**Why**</span></span>

<span data-ttu-id="9a26f-309">Staré chování nebyl očekáván.</span><span class="sxs-lookup"><span data-stu-id="9a26f-309">The old behavoir was unexpected.</span></span>

<span data-ttu-id="9a26f-310">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="9a26f-310">**Mitigations**</span></span>

<span data-ttu-id="9a26f-311">Vlastnost je stále explicitně mapovat pro oddělení sloupců v odvozených typů:</span><span class="sxs-lookup"><span data-stu-id="9a26f-311">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="9a26f-312">Konvence vlastnost cizího klíče už neodpovídá stejný název jako vlastnost instančního objektu</span><span class="sxs-lookup"><span data-stu-id="9a26f-312">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="9a26f-313">Sledování problému #13274</span><span class="sxs-lookup"><span data-stu-id="9a26f-313">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="9a26f-314">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="9a26f-314">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="9a26f-315">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-315">**Old behavior**</span></span>

<span data-ttu-id="9a26f-316">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="9a26f-316">Consider the following model:</span></span>
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
<span data-ttu-id="9a26f-317">Před EF Core 3.0 `CustomerId` vlastnost se použije pro cizí klíč konvencí.</span><span class="sxs-lookup"><span data-stu-id="9a26f-317">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="9a26f-318">Nicméně pokud `Order` je typ vlastnictví, a potom to by také provést `CustomerId` primární klíč a to se většinou očekává.</span><span class="sxs-lookup"><span data-stu-id="9a26f-318">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="9a26f-319">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-319">**New behavior**</span></span>

<span data-ttu-id="9a26f-320">Od verze 3.0, EF Core nesnaží při využívání vlastností cizího klíče podle konvence, pokud mají stejný název jako vlastnost instančního objektu.</span><span class="sxs-lookup"><span data-stu-id="9a26f-320">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="9a26f-321">Název instančního objektu typu zřetězený s názvem hlavní vlastnosti a název navigační zřetězená s vzory názvů vlastnosti principal budou stále odpovídat.</span><span class="sxs-lookup"><span data-stu-id="9a26f-321">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="9a26f-322">Příklad:</span><span class="sxs-lookup"><span data-stu-id="9a26f-322">For example:</span></span>

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

<span data-ttu-id="9a26f-323">**Proč**</span><span class="sxs-lookup"><span data-stu-id="9a26f-323">**Why**</span></span>

<span data-ttu-id="9a26f-324">Tato změna byla provedena na chybně nedefinujte vlastnost primárního klíče typ vlastnictví.</span><span class="sxs-lookup"><span data-stu-id="9a26f-324">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="9a26f-325">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="9a26f-325">**Mitigations**</span></span>

<span data-ttu-id="9a26f-326">Pokud byla vlastnost má být cizí klíč a proto část primárního klíče a pak explicitně jako takový ho nakonfigurujte.</span><span class="sxs-lookup"><span data-stu-id="9a26f-326">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="9a26f-327">Připojení k databázi je nyní uzavřeno, pokud není využito už před objektu TransactionScope byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="9a26f-327">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="9a26f-328">Sledování problému #14218</span><span class="sxs-lookup"><span data-stu-id="9a26f-328">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="9a26f-329">Tato změna bude zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="9a26f-329">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="9a26f-330">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-330">**Old behavior**</span></span>

<span data-ttu-id="9a26f-331">Před EF Core 3.0, pokud kontext otevře připojení k uvnitř `TransactionScope`, připojení zůstane otevřená, při aktuální `TransactionScope` je aktivní.</span><span class="sxs-lookup"><span data-stu-id="9a26f-331">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="9a26f-332">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-332">**New behavior**</span></span>

<span data-ttu-id="9a26f-333">Od verze 3.0, EF Core uzavře připojení co nejdříve po dokončení jeho použití.</span><span class="sxs-lookup"><span data-stu-id="9a26f-333">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="9a26f-334">**Proč**</span><span class="sxs-lookup"><span data-stu-id="9a26f-334">**Why**</span></span>

<span data-ttu-id="9a26f-335">Tato změna umožňuje použití několika kontextech v rámci stejného `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="9a26f-335">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="9a26f-336">Nové chování také odpovídající EF6.</span><span class="sxs-lookup"><span data-stu-id="9a26f-336">The new behavior also matches EF6.</span></span>

<span data-ttu-id="9a26f-337">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="9a26f-337">**Mitigations**</span></span>

<span data-ttu-id="9a26f-338">Pokud je nutné připojení zůstat otevřené explicitní volání konstruktoru `OpenConnection()` zajistí, že EF Core nezavírá je předčasně ukončena:</span><span class="sxs-lookup"><span data-stu-id="9a26f-338">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="9a26f-339">Jednotlivé vlastnosti pomocí generování klíčů nezávislé celé číslo v paměti</span><span class="sxs-lookup"><span data-stu-id="9a26f-339">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="9a26f-340">Sledování problému #6872</span><span class="sxs-lookup"><span data-stu-id="9a26f-340">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="9a26f-341">Tato změna bude zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="9a26f-341">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="9a26f-342">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-342">**Old behavior**</span></span>

<span data-ttu-id="9a26f-343">Před EF Core 3.0 se použil jeden generátor sdíleného hodnot pro všechny vlastnosti klíče celé číslo v paměti.</span><span class="sxs-lookup"><span data-stu-id="9a26f-343">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="9a26f-344">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-344">**New behavior**</span></span>

<span data-ttu-id="9a26f-345">Od verze EF Core 3.0, každé celé číslo klíčová vlastnost získá vlastní generátor hodnot při použití databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="9a26f-345">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="9a26f-346">Také pokud se odstraní databáze, pak generování klíčů je obnovit pro všechny tabulky.</span><span class="sxs-lookup"><span data-stu-id="9a26f-346">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="9a26f-347">**Proč**</span><span class="sxs-lookup"><span data-stu-id="9a26f-347">**Why**</span></span>

<span data-ttu-id="9a26f-348">Tato změna byla provedena zarovnat generování klíče v paměti blíže k generování klíčů skutečná databáze a zlepšit schopnost izolace testů od sebe navzájem při použití databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="9a26f-348">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="9a26f-349">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="9a26f-349">**Mitigations**</span></span>

<span data-ttu-id="9a26f-350">Toto může rozbít aplikaci, která se spoléhá na konkrétní hodnoty klíče v paměti nastavit.</span><span class="sxs-lookup"><span data-stu-id="9a26f-350">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="9a26f-351">Zvažte místo toho spoléhat na konkrétní hodnoty klíče, ani aktualizaci tak, aby odpovídala nové chování.</span><span class="sxs-lookup"><span data-stu-id="9a26f-351">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="9a26f-352">Základní pole se používají ve výchozím nastavení</span><span class="sxs-lookup"><span data-stu-id="9a26f-352">Backing fields are used by default</span></span>

[<span data-ttu-id="9a26f-353">Sledování problému #12430</span><span class="sxs-lookup"><span data-stu-id="9a26f-353">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="9a26f-354">Tato změna byla zavedená v EF Core 3.0 – náhled 2.</span><span class="sxs-lookup"><span data-stu-id="9a26f-354">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="9a26f-355">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-355">**Old behavior**</span></span>

<span data-ttu-id="9a26f-356">Než 3.0 i v případě, že pomocné pole vlastnosti zná, EF Core by stále ve výchozím nastavení čtení a zápisu hodnoty vlastnosti pomocí metody getter a setter vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="9a26f-356">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="9a26f-357">K tomuto výjimka provádění dotazů, kde pomocné pole se nastavuje přímo Pokud jsou známé.</span><span class="sxs-lookup"><span data-stu-id="9a26f-357">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="9a26f-358">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-358">**New behavior**</span></span>

<span data-ttu-id="9a26f-359">Počínaje EF Core 3.0, pokud jsou známé vlastnosti pole zálohování, potom EF Core budou vždy číst a Zapisovat vlastnosti pomocí pole zálohování.</span><span class="sxs-lookup"><span data-stu-id="9a26f-359">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="9a26f-360">Pokud aplikace se spoléhá na další chování zakódovaný do metody getter nebo setter to může způsobit přerušení aplikace.</span><span class="sxs-lookup"><span data-stu-id="9a26f-360">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="9a26f-361">**Proč**</span><span class="sxs-lookup"><span data-stu-id="9a26f-361">**Why**</span></span>

<span data-ttu-id="9a26f-362">Tato změna byla provedena zabránit EF Core chybně aktivací obchodní logiky ve výchozím nastavení při provádění databázových operací zahrnující entity.</span><span class="sxs-lookup"><span data-stu-id="9a26f-362">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="9a26f-363">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="9a26f-363">**Mitigations**</span></span>

<span data-ttu-id="9a26f-364">Prostřednictvím konfigurace režim přístupu k vlastnosti v rozhraní API fluent modelBuilder lze obnovit chování pre-3.0.</span><span class="sxs-lookup"><span data-stu-id="9a26f-364">The pre-3.0 behavior can be restored through configuration of the property access mode in the modelBuilder fluent API.</span></span>
<span data-ttu-id="9a26f-365">Příklad:</span><span class="sxs-lookup"><span data-stu-id="9a26f-365">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="9a26f-366">Vyvolání výjimky, pokud je nalezeno více polí kompatibilní zálohování</span><span class="sxs-lookup"><span data-stu-id="9a26f-366">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="9a26f-367">Sledování problému #12523</span><span class="sxs-lookup"><span data-stu-id="9a26f-367">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="9a26f-368">Tato změna bude zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="9a26f-368">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="9a26f-369">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-369">**Old behavior**</span></span>

<span data-ttu-id="9a26f-370">Před EF Core 3.0 Pokud odpovídá více polí pravidel pro vyhledání pomocným polem vlastnosti, pak vybere jedno pole na základě některých pořadí priority.</span><span class="sxs-lookup"><span data-stu-id="9a26f-370">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="9a26f-371">To může způsobit nesprávné pole pro použití v případech, nejednoznačný.</span><span class="sxs-lookup"><span data-stu-id="9a26f-371">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="9a26f-372">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-372">**New behavior**</span></span>

<span data-ttu-id="9a26f-373">Od verze EF Core 3.0, pokud více polí budou odpovídat na stejnou vlastnost, pak je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="9a26f-373">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="9a26f-374">**Proč**</span><span class="sxs-lookup"><span data-stu-id="9a26f-374">**Why**</span></span>

<span data-ttu-id="9a26f-375">Tato změna byla provedena, abyste se vyhnuli použití tiše jedno pole před jiným při může obsahovat jen jedno správné.</span><span class="sxs-lookup"><span data-stu-id="9a26f-375">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="9a26f-376">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="9a26f-376">**Mitigations**</span></span>

<span data-ttu-id="9a26f-377">Vlastnosti s nejednoznačný základní pole musí mít pole pro použití explicitně zadán.</span><span class="sxs-lookup"><span data-stu-id="9a26f-377">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="9a26f-378">Například pomocí rozhraní fluent API:</span><span class="sxs-lookup"><span data-stu-id="9a26f-378">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="9a26f-379">Vlastnost jen pro pole názvů by měl odpovídat názvu pole</span><span class="sxs-lookup"><span data-stu-id="9a26f-379">Field-only property names should match the field name</span></span>

<span data-ttu-id="9a26f-380">Tato změna bude zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="9a26f-380">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="9a26f-381">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-381">**Old behavior**</span></span>

<span data-ttu-id="9a26f-382">Před EF Core 3.0 vlastnost může být určeno hodnotu řetězce a pokud nebyla nalezena žádná vlastnost s tímto názvem na typu CLR EF Core by opakujte tak, aby odpovídaly na pole, které používá convetion pravidla.</span><span class="sxs-lookup"><span data-stu-id="9a26f-382">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convetion rules.</span></span>
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

<span data-ttu-id="9a26f-383">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-383">**New behavior**</span></span>

<span data-ttu-id="9a26f-384">Od verze EF Core 3.0, vlastnost jen pro pole musí odpovídat názvu pole přesně.</span><span class="sxs-lookup"><span data-stu-id="9a26f-384">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="9a26f-385">**Proč**</span><span class="sxs-lookup"><span data-stu-id="9a26f-385">**Why**</span></span>

<span data-ttu-id="9a26f-386">Tato změna byla provedena, abyste se vyhnuli použití stejné pole pro dvě vlastnosti s názvem podobně, také udržuje pravidla pro porovnávání vlastností pouze pole je stejný jako u vlastnosti, které jsou namapovány na vlastnosti CLR.</span><span class="sxs-lookup"><span data-stu-id="9a26f-386">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="9a26f-387">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="9a26f-387">**Mitigations**</span></span>

<span data-ttu-id="9a26f-388">Vlastnosti pouze pro pole musí mít název jako pole, které jsou mapovány na stejný.</span><span class="sxs-lookup"><span data-stu-id="9a26f-388">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="9a26f-389">V novější verzi preview EF Core 3.0 Plánujeme znovu povolit explicitně konfigurace název pole, které se liší od názvu vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="9a26f-389">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="9a26f-390">AddDbContext/AddDbContextPool zavolejte už AddLogging a AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="9a26f-390">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="9a26f-391">Sledování problému #14756</span><span class="sxs-lookup"><span data-stu-id="9a26f-391">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="9a26f-392">Tato změna bude zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="9a26f-392">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="9a26f-393">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-393">**Old behavior**</span></span>

<span data-ttu-id="9a26f-394">Před EF Core 3.0, volání `AddDbContext` nebo `AddDbContextPool` by také zaregistrovat protokolování a ukládání služeb s D.I prostřednictvím volání do mezipaměti [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) a [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="9a26f-394">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="9a26f-395">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-395">**New behavior**</span></span>

<span data-ttu-id="9a26f-396">Od verze EF Core 3.0, `AddDbContext` a `AddDbContextPool` nebude registru tyto služby s DI (Dependency Injection).</span><span class="sxs-lookup"><span data-stu-id="9a26f-396">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="9a26f-397">**Proč**</span><span class="sxs-lookup"><span data-stu-id="9a26f-397">**Why**</span></span>

<span data-ttu-id="9a26f-398">EF Core 3.0 nevyžaduje, že tyto služby jsou v cotainer DI vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="9a26f-398">EF Core 3.0 does not require that these services are in the application's DI cotainer.</span></span> <span data-ttu-id="9a26f-399">Nicméně pokud `ILoggerFactory` je zaregistrovaný v aplikačním kontejneru DI, pak bude i nadále využívat EF Core.</span><span class="sxs-lookup"><span data-stu-id="9a26f-399">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="9a26f-400">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="9a26f-400">**Mitigations**</span></span>

<span data-ttu-id="9a26f-401">Pokud vaše aplikace potřebuje tyto služby, registrujte je explicitně s využitím kontejnerů DI [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) nebo [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="9a26f-401">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="9a26f-402">DbContext.Entry teď provádí místní metoda DetectChanges</span><span class="sxs-lookup"><span data-stu-id="9a26f-402">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="9a26f-403">Sledování problému #13552</span><span class="sxs-lookup"><span data-stu-id="9a26f-403">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="9a26f-404">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="9a26f-404">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="9a26f-405">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-405">**Old behavior**</span></span>

<span data-ttu-id="9a26f-406">Před EF Core 3.0, volání `DbContext.Entry` způsobí změny, aby se rozpoznal pro všechny sledované entity.</span><span class="sxs-lookup"><span data-stu-id="9a26f-406">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="9a26f-407">Tím zajistíte, že stav zpřístupněn v `EntityEntry` byla aktuální.</span><span class="sxs-lookup"><span data-stu-id="9a26f-407">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="9a26f-408">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-408">**New behavior**</span></span>

<span data-ttu-id="9a26f-409">Spouští se s EF Core 3.0, volání `DbContext.Entry` se teď jenom pokus o zjištění změn v dané entitě a všechny sledované instančního objektu entity, které s ním souvisejí.</span><span class="sxs-lookup"><span data-stu-id="9a26f-409">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="9a26f-410">To znamená, že se změní jinde nemusí byl zjištěn zavoláním této metody, které by mohly mít vliv na stav aplikace.</span><span class="sxs-lookup"><span data-stu-id="9a26f-410">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="9a26f-411">Všimněte si, že pokud `ChangeTracker.AutoDetectChangesEnabled` je nastavena na `false` pak i tato detekce místní změny se deaktivuje.</span><span class="sxs-lookup"><span data-stu-id="9a26f-411">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="9a26f-412">Jiné metody, které způsobují detekce změn – například `ChangeTracker.Entries` a `SaveChanges`– způsobí úplné `DetectChanges` všech sledovat entity.</span><span class="sxs-lookup"><span data-stu-id="9a26f-412">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="9a26f-413">**Proč**</span><span class="sxs-lookup"><span data-stu-id="9a26f-413">**Why**</span></span>

<span data-ttu-id="9a26f-414">Tato změna byla provedena pro zlepšení výkonu výchozí použití `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="9a26f-414">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="9a26f-415">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="9a26f-415">**Mitigations**</span></span>

<span data-ttu-id="9a26f-416">Volání `ChgangeTracker.DetectChanges()` explicitně před voláním `Entry` k zajištění chování pre-3.0.</span><span class="sxs-lookup"><span data-stu-id="9a26f-416">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="9a26f-417">Řetězec a bajtové pole klíčů se nerozlišují klientem generovaná ve výchozím nastavení</span><span class="sxs-lookup"><span data-stu-id="9a26f-417">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="9a26f-418">Sledování problému #14617</span><span class="sxs-lookup"><span data-stu-id="9a26f-418">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="9a26f-419">Tato změna bude zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="9a26f-419">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="9a26f-420">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-420">**Old behavior**</span></span>

<span data-ttu-id="9a26f-421">Před EF Core 3.0 `string` a `byte[]` klíčové vlastnosti použití explicitním nastavením nenulová hodnota.</span><span class="sxs-lookup"><span data-stu-id="9a26f-421">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="9a26f-422">V takovém případě by se vygenerovala hodnota klíče na klientovi jako identifikátor GUID serializovat do bajtů pro `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="9a26f-422">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="9a26f-423">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-423">**New behavior**</span></span>

<span data-ttu-id="9a26f-424">Od verze EF Core 3.0 výjimka bude vyvolána označující, že nebyla nastavena žádná hodnota klíče.</span><span class="sxs-lookup"><span data-stu-id="9a26f-424">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="9a26f-425">**Proč**</span><span class="sxs-lookup"><span data-stu-id="9a26f-425">**Why**</span></span>

<span data-ttu-id="9a26f-426">Tato změna byla provedena, protože klient generován `string` / `byte[]` hodnoty nejsou obecně užitečné a výchozí chování dostal pevné argumentovat o generované hodnoty klíče v běžným způsobem.</span><span class="sxs-lookup"><span data-stu-id="9a26f-426">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="9a26f-427">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="9a26f-427">**Mitigations**</span></span>

<span data-ttu-id="9a26f-428">Chování pre-3.0 je možné získat tak, že explicitně zadáte, by měl klíčové vlastnosti použít generované hodnoty, pokud je nastavena žádná hodnota jiná než null.</span><span class="sxs-lookup"><span data-stu-id="9a26f-428">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="9a26f-429">Například pomocí rozhraní fluent API:</span><span class="sxs-lookup"><span data-stu-id="9a26f-429">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="9a26f-430">Nebo s anotacemi dat:</span><span class="sxs-lookup"><span data-stu-id="9a26f-430">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="9a26f-431">Implementaci třídy ILoggerFactory je nyní vymezené služby</span><span class="sxs-lookup"><span data-stu-id="9a26f-431">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="9a26f-432">Sledování problému #14698</span><span class="sxs-lookup"><span data-stu-id="9a26f-432">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="9a26f-433">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="9a26f-433">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="9a26f-434">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-434">**Old behavior**</span></span>

<span data-ttu-id="9a26f-435">Před EF Core 3.0 `ILoggerFactory` bylo zaregistrováno jako služba typu singleton.</span><span class="sxs-lookup"><span data-stu-id="9a26f-435">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="9a26f-436">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-436">**New behavior**</span></span>

<span data-ttu-id="9a26f-437">Od verze EF Core 3.0, `ILoggerFactory` je teď zaregistrovaný jako obor.</span><span class="sxs-lookup"><span data-stu-id="9a26f-437">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="9a26f-438">**Proč**</span><span class="sxs-lookup"><span data-stu-id="9a26f-438">**Why**</span></span>

<span data-ttu-id="9a26f-439">Tato změna byla provedena umožňující přidružení protokolovací nástroj se `DbContext` instanci, která povoluje další funkce a odebírá někdy patologických chování, jako je například obrovské množství poskytovatelů vnitřní chybě služby.</span><span class="sxs-lookup"><span data-stu-id="9a26f-439">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="9a26f-440">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="9a26f-440">**Mitigations**</span></span>

<span data-ttu-id="9a26f-441">Tato změna by neměla mít vliv kód aplikace, pokud není registrace a použití vlastních služeb v poskytovateli EF Core vnitřní chybě služby.</span><span class="sxs-lookup"><span data-stu-id="9a26f-441">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="9a26f-442">Tato akce není běžné.</span><span class="sxs-lookup"><span data-stu-id="9a26f-442">This isn't common.</span></span>
<span data-ttu-id="9a26f-443">V těchto případech většinu toho, co bude dál fungovat, ale žádné služby typu singleton, která byla v závislosti na `ILoggerFactory` bude nutné změnit tak, aby získat `ILoggerFactory` jiným způsobem.</span><span class="sxs-lookup"><span data-stu-id="9a26f-443">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="9a26f-444">Pokud narazíte na situace tímto způsobem, založte prosím problém na na [sledování problémů Githubu EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) a dejte nám vědět, jak používáte `ILoggerFactory` tak, aby nám můžete lépe porozumět nechcete v budoucnu znovu rozdělit.</span><span class="sxs-lookup"><span data-stu-id="9a26f-444">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="idbcontextoptionsextensionwithdebuginfo-merged-into-idbcontextoptionsextension"></a><span data-ttu-id="9a26f-445">Sloučí IDbContextOptionsExtension IDbContextOptionsExtensionWithDebugInfo</span><span class="sxs-lookup"><span data-stu-id="9a26f-445">IDbContextOptionsExtensionWithDebugInfo merged into IDbContextOptionsExtension</span></span>

[<span data-ttu-id="9a26f-446">Sledování problému #13552</span><span class="sxs-lookup"><span data-stu-id="9a26f-446">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="9a26f-447">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="9a26f-447">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="9a26f-448">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-448">**Old behavior**</span></span>

<span data-ttu-id="9a26f-449">`IDbContextOptionsExtensionWithDebugInfo` Další volitelné rozhraní byla prodloužena z `IDbContextOptionsExtension` pro vyvarování rozbíjející změně rozhraní během cyklu vydání verze 2.x.</span><span class="sxs-lookup"><span data-stu-id="9a26f-449">`IDbContextOptionsExtensionWithDebugInfo` was an additional optional interface extended from `IDbContextOptionsExtension` to avoid making a breaking change to the interface during the 2.x release cycle.</span></span>

<span data-ttu-id="9a26f-450">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-450">**New behavior**</span></span>

<span data-ttu-id="9a26f-451">Rozhraní jsou nyní sloučeny do `IDbContextOptionsExtension`.</span><span class="sxs-lookup"><span data-stu-id="9a26f-451">The interfaces are now merged together into `IDbContextOptionsExtension`.</span></span>

<span data-ttu-id="9a26f-452">**Proč**</span><span class="sxs-lookup"><span data-stu-id="9a26f-452">**Why**</span></span>

<span data-ttu-id="9a26f-453">Tato změna byla provedena, protože rozhraní jsou koncepčně jednou.</span><span class="sxs-lookup"><span data-stu-id="9a26f-453">This change was made because the interfaces are conceptually one.</span></span>

<span data-ttu-id="9a26f-454">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="9a26f-454">**Mitigations**</span></span>

<span data-ttu-id="9a26f-455">Žádné implementace `IDbContextOptionsExtension` bude muset být aktualizované kvůli podpoře nového člena.</span><span class="sxs-lookup"><span data-stu-id="9a26f-455">Any implementations of `IDbContextOptionsExtension` will need to be updated to support the new member.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="9a26f-456">Opožděné načtení proxy už předpokládají, že jsou plně načteny navigační vlastnosti</span><span class="sxs-lookup"><span data-stu-id="9a26f-456">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="9a26f-457">Sledování problému #12780</span><span class="sxs-lookup"><span data-stu-id="9a26f-457">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="9a26f-458">Tato změna bude zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="9a26f-458">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="9a26f-459">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-459">**Old behavior**</span></span>

<span data-ttu-id="9a26f-460">Před EF Core 3.0, jednou `DbContext` byl odstraněn neexistoval způsob, jak zjistit, zda danou navigační vlastnost s entitou získané z daného kontextu byl plně načten či nikoli.</span><span class="sxs-lookup"><span data-stu-id="9a26f-460">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="9a26f-461">Proxy by místo toho se předpokládá, že je načtena navigační odkaz, pokud má nenulovou hodnotu a, že je načtena navigace kolekce, pokud není prázdná.</span><span class="sxs-lookup"><span data-stu-id="9a26f-461">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="9a26f-462">V těchto případech by pokus o opožděné načtení no-op.</span><span class="sxs-lookup"><span data-stu-id="9a26f-462">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="9a26f-463">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-463">**New behavior**</span></span>

<span data-ttu-id="9a26f-464">Od verze EF Core 3.0, proxy servery sledovat, jestli je načtena navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="9a26f-464">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="9a26f-465">To znamená, že při pokusu o přístup k navigační vlastnost, která je načtena po kontext se vyřadil. bude vždycky no-op, i v případě, že je načteno navigace je prázdný nebo mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="9a26f-465">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="9a26f-466">Naopak pokusu o přístup k navigační vlastnosti, které není načteno vyvolají výjimku pokud uvolnění kontextu, i když je navigační vlastnost kolekce není prázdná.</span><span class="sxs-lookup"><span data-stu-id="9a26f-466">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="9a26f-467">Pokud k této situaci dochází, znamená to kód aplikace se pokouší použít opožděné načtení na neplatný čas a aplikace by měla být změněna na tuto akci.</span><span class="sxs-lookup"><span data-stu-id="9a26f-467">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="9a26f-468">**Proč**</span><span class="sxs-lookup"><span data-stu-id="9a26f-468">**Why**</span></span>

<span data-ttu-id="9a26f-469">Tato změna byla provedena, aby chování konzistentní a správné při pokusu o opožděné načtení na vyřazený `DbContext` instance.</span><span class="sxs-lookup"><span data-stu-id="9a26f-469">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="9a26f-470">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="9a26f-470">**Mitigations**</span></span>

<span data-ttu-id="9a26f-471">Kód aplikace, který nebude pokoušet opožděné načtení vyřazený kontextu, nebo nakonfigurovat to být no-op. jak je popsáno v zpráva o výjimce.</span><span class="sxs-lookup"><span data-stu-id="9a26f-471">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="9a26f-472">Nadměrné vytváření zprostředkovatelů vnitřní chybě služby je teď k chybě ve výchozím nastavení</span><span class="sxs-lookup"><span data-stu-id="9a26f-472">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="9a26f-473">Sledování problému #10236</span><span class="sxs-lookup"><span data-stu-id="9a26f-473">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="9a26f-474">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="9a26f-474">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="9a26f-475">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-475">**Old behavior**</span></span>

<span data-ttu-id="9a26f-476">Upozornění by před EF Core 3.0 zaznamenávané aplikace vytváří patologických několik poskytovatelů vnitřní chybě služby.</span><span class="sxs-lookup"><span data-stu-id="9a26f-476">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="9a26f-477">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-477">**New behavior**</span></span>

<span data-ttu-id="9a26f-478">Od verze EF Core 3.0, toto upozornění se teď považuje za a chyby a výjimky je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="9a26f-478">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="9a26f-479">**Proč**</span><span class="sxs-lookup"><span data-stu-id="9a26f-479">**Why**</span></span>

<span data-ttu-id="9a26f-480">Tato změna byla provedena na připravovat lepší kód aplikace prostřednictvím vystavení takovém patologických více explicitně.</span><span class="sxs-lookup"><span data-stu-id="9a26f-480">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="9a26f-481">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="9a26f-481">**Mitigations**</span></span>

<span data-ttu-id="9a26f-482">Nejvhodnější příčinu akce při výskytu této chyby je zjistěte původní příčinu a nevytvářet mnoho poskytovatelů vnitřní chybě služby.</span><span class="sxs-lookup"><span data-stu-id="9a26f-482">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="9a26f-483">Však chyba může být převést zpět na upozornění (nebo ignorovat) prostřednictvím konfigurace na `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="9a26f-483">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="9a26f-484">Příklad:</span><span class="sxs-lookup"><span data-stu-id="9a26f-484">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="9a26f-485">Nové chování pro HasOne/HasMany volat jeden řetězec</span><span class="sxs-lookup"><span data-stu-id="9a26f-485">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="9a26f-486">Sledování problému #9171</span><span class="sxs-lookup"><span data-stu-id="9a26f-486">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="9a26f-487">Tato změna bude zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="9a26f-487">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="9a26f-488">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-488">**Old behavior**</span></span>

<span data-ttu-id="9a26f-489">Před EF Core 3.0 kód volání `HasOne` nebo `HasMany` s jeden řetězec byl interpretovat matoucí způsobem.</span><span class="sxs-lookup"><span data-stu-id="9a26f-489">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpretted in a confusing way.</span></span>
<span data-ttu-id="9a26f-490">Příklad:</span><span class="sxs-lookup"><span data-stu-id="9a26f-490">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="9a26f-491">Kód vypadá je týkající se `Samurai` některé jiné entity typu použití `Entrance` navigační vlastnost, která může být privátní.</span><span class="sxs-lookup"><span data-stu-id="9a26f-491">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="9a26f-492">Ve skutečnosti, tento kód se pokouší vytvořit relaci některé typ entity s názvem `Entrance` se žádné navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="9a26f-492">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="9a26f-493">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-493">**New behavior**</span></span>

<span data-ttu-id="9a26f-494">Od verze EF Core 3.0, výše uvedený kód nyní dělá co podívali jako její by měl mít byla činnosti před.</span><span class="sxs-lookup"><span data-stu-id="9a26f-494">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="9a26f-495">**Proč**</span><span class="sxs-lookup"><span data-stu-id="9a26f-495">**Why**</span></span>

<span data-ttu-id="9a26f-496">Staré chování bylo velmi matoucí, zejména při čtení konfigurace kód a hledání chyb.</span><span class="sxs-lookup"><span data-stu-id="9a26f-496">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="9a26f-497">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="9a26f-497">**Mitigations**</span></span>

<span data-ttu-id="9a26f-498">Tímto přerušíte pouze aplikace, které jsou explicitně konfigurace relace používání řetězců pro názvy typů a bez explicitním zadáním navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="9a26f-498">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="9a26f-499">Není běžné.</span><span class="sxs-lookup"><span data-stu-id="9a26f-499">This is not common.</span></span>
<span data-ttu-id="9a26f-500">Předchozí chování můžete získat prostřednictvím explicitně předávání `null` pro název navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="9a26f-500">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="9a26f-501">Příklad:</span><span class="sxs-lookup"><span data-stu-id="9a26f-501">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="9a26f-502">Návratový typ pro několik asynchronních metod se změnil z úlohy na ValueTask</span><span class="sxs-lookup"><span data-stu-id="9a26f-502">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="9a26f-503">Sledování problému #15184</span><span class="sxs-lookup"><span data-stu-id="9a26f-503">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="9a26f-504">Tato změna bude zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="9a26f-504">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="9a26f-505">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-505">**Old behavior**</span></span>

<span data-ttu-id="9a26f-506">Dříve vrátila následující asynchronní metody `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="9a26f-506">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="9a26f-507">`ValueGenerator.NextValueAsync()` (a odvozených tříd)</span><span class="sxs-lookup"><span data-stu-id="9a26f-507">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="9a26f-508">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-508">**New behavior**</span></span>

<span data-ttu-id="9a26f-509">Výše uvedené metody nyní vrací `ValueTask<T>` přes stejné `T` stejně jako předtím.</span><span class="sxs-lookup"><span data-stu-id="9a26f-509">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="9a26f-510">**Proč**</span><span class="sxs-lookup"><span data-stu-id="9a26f-510">**Why**</span></span>

<span data-ttu-id="9a26f-511">Tato změna snižuje počet přidělení haldy vzniklé při volání těchto metod, zlepšení obecné informace o výkonu.</span><span class="sxs-lookup"><span data-stu-id="9a26f-511">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="9a26f-512">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="9a26f-512">**Mitigations**</span></span>

<span data-ttu-id="9a26f-513">Aplikace jednoduše čeká na výše uvedené rozhraní API pouze muset být překompilovány - žádné změny zdroje jsou nezbytné.</span><span class="sxs-lookup"><span data-stu-id="9a26f-513">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="9a26f-514">Složitější využití (například předání vráceného `Task` k `Task.WhenAny()`) obvykle vyžadují, aby vráceného `ValueTask<T>` převedou na hodnoty `Task<T>` voláním `AsTask()` na něm.</span><span class="sxs-lookup"><span data-stu-id="9a26f-514">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="9a26f-515">Všimněte si, že to Neguje snížení přidělení, které tato změna přináší.</span><span class="sxs-lookup"><span data-stu-id="9a26f-515">Note that this negates the allocation reduction that this change brings.</span></span>

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="9a26f-516">Poznámka relační: TypeMapping je teď stejně TypeMapping</span><span class="sxs-lookup"><span data-stu-id="9a26f-516">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="9a26f-517">Sledování problému #9913</span><span class="sxs-lookup"><span data-stu-id="9a26f-517">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="9a26f-518">Tato změna byla zavedená v EF Core 3.0 – náhled 2.</span><span class="sxs-lookup"><span data-stu-id="9a26f-518">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="9a26f-519">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-519">**Old behavior**</span></span>

<span data-ttu-id="9a26f-520">Název poznámky pro mapování anotace typu byl "Relační: TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="9a26f-520">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="9a26f-521">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-521">**New behavior**</span></span>

<span data-ttu-id="9a26f-522">Název poznámky pro mapování anotace typu je nyní "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="9a26f-522">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="9a26f-523">**Proč**</span><span class="sxs-lookup"><span data-stu-id="9a26f-523">**Why**</span></span>

<span data-ttu-id="9a26f-524">Mapování typu jsou teď používá pro více než jen relační databáze poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="9a26f-524">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="9a26f-525">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="9a26f-525">**Mitigations**</span></span>

<span data-ttu-id="9a26f-526">Tímto přerušíte jenom aplikace s přístupem k mapování typů přímo jako poznámka, která není běžné.</span><span class="sxs-lookup"><span data-stu-id="9a26f-526">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="9a26f-527">Nejvhodnější opatření na opravu se má používat rovinu rozhraní API pro mapování typů přístupu spíše než přímo pomocí anotace.</span><span class="sxs-lookup"><span data-stu-id="9a26f-527">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="9a26f-528">Vyvolá výjimku, ToTable na odvozený typ.</span><span class="sxs-lookup"><span data-stu-id="9a26f-528">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="9a26f-529">Sledování problému #11811</span><span class="sxs-lookup"><span data-stu-id="9a26f-529">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="9a26f-530">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="9a26f-530">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="9a26f-531">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-531">**Old behavior**</span></span>

<span data-ttu-id="9a26f-532">Před EF Core 3.0 `ToTable()` volalo odvozený typ bude ignorovat, protože pouze dědičnosti mapování strategie byl TPH, kdy to není platný.</span><span class="sxs-lookup"><span data-stu-id="9a26f-532">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="9a26f-533">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-533">**New behavior**</span></span>

<span data-ttu-id="9a26f-534">Spouští se s EF Core 3.0 a při přípravě na přidání TPT a TPC podporují v pozdější verzi `ToTable()` volalo odvozený typ bude nyní vyvolání výjimky v budoucnu vyhnuli o změnu neočekávané mapování.</span><span class="sxs-lookup"><span data-stu-id="9a26f-534">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="9a26f-535">**Proč**</span><span class="sxs-lookup"><span data-stu-id="9a26f-535">**Why**</span></span>

<span data-ttu-id="9a26f-536">Aktuálně není platná pro mapování odvozeného typu na jiné tabulky.</span><span class="sxs-lookup"><span data-stu-id="9a26f-536">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="9a26f-537">Tato změna se vyhnete přerušení v budoucnu, kdy bude platná věc udělat.</span><span class="sxs-lookup"><span data-stu-id="9a26f-537">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="9a26f-538">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="9a26f-538">**Mitigations**</span></span>

<span data-ttu-id="9a26f-539">Odeberte všechny pokusy o mapování odvozené typy s jinými tabulkami.</span><span class="sxs-lookup"><span data-stu-id="9a26f-539">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="9a26f-540">Nahradí HasIndex ForSqlServerHasIndex</span><span class="sxs-lookup"><span data-stu-id="9a26f-540">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="9a26f-541">Sledování problému #12366</span><span class="sxs-lookup"><span data-stu-id="9a26f-541">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="9a26f-542">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="9a26f-542">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="9a26f-543">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-543">**Old behavior**</span></span>

<span data-ttu-id="9a26f-544">Před EF Core 3.0 `ForSqlServerHasIndex().ForSqlServerInclude()` poskytuje způsob, jak konfigurovat sloupců použitých s `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="9a26f-544">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="9a26f-545">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-545">**New behavior**</span></span>

<span data-ttu-id="9a26f-546">Od verze EF Core 3.0, pomocí `Include` na indexu se teď podporuje relační úrovni.</span><span class="sxs-lookup"><span data-stu-id="9a26f-546">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="9a26f-547">Použití `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="9a26f-547">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="9a26f-548">**Proč**</span><span class="sxs-lookup"><span data-stu-id="9a26f-548">**Why**</span></span>

<span data-ttu-id="9a26f-549">Tato změna byla provedena konsolidovat rozhraní API pro indexy s `Include` do jednoho umístění pro všechny databáze poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="9a26f-549">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="9a26f-550">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="9a26f-550">**Mitigations**</span></span>

<span data-ttu-id="9a26f-551">Pomocí nového rozhraní API, jak je znázorněno výše.</span><span class="sxs-lookup"><span data-stu-id="9a26f-551">Use the new API, as shown above.</span></span>

## <a name="metadata-api-changes"></a><span data-ttu-id="9a26f-552">Změn metadat rozhraní API</span><span class="sxs-lookup"><span data-stu-id="9a26f-552">Metadata API changes</span></span>

[<span data-ttu-id="9a26f-553">Sledování problému #214</span><span class="sxs-lookup"><span data-stu-id="9a26f-553">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="9a26f-554">Tato změna bude zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="9a26f-554">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="9a26f-555">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-555">**New behavior**</span></span>

<span data-ttu-id="9a26f-556">Následující vlastnosti byly převedeny na rozšiřující metody:</span><span class="sxs-lookup"><span data-stu-id="9a26f-556">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="9a26f-557">**Proč**</span><span class="sxs-lookup"><span data-stu-id="9a26f-557">**Why**</span></span>

<span data-ttu-id="9a26f-558">Tato změna usnadňuje provádění výše uvedených rozhraní.</span><span class="sxs-lookup"><span data-stu-id="9a26f-558">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="9a26f-559">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="9a26f-559">**Mitigations**</span></span>

<span data-ttu-id="9a26f-560">Pomocí nové metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="9a26f-560">Use the new extension methods.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="9a26f-561">EF Core už odešle – Direktiva pragma pro vynucení SQLite FK</span><span class="sxs-lookup"><span data-stu-id="9a26f-561">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="9a26f-562">Sledování problému #12151</span><span class="sxs-lookup"><span data-stu-id="9a26f-562">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="9a26f-563">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="9a26f-563">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="9a26f-564">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-564">**Old behavior**</span></span>

<span data-ttu-id="9a26f-565">Před EF Core 3.0, bude posílat EF Core `PRAGMA foreign_keys = 1` při otevření připojení k SQLite.</span><span class="sxs-lookup"><span data-stu-id="9a26f-565">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="9a26f-566">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-566">**New behavior**</span></span>

<span data-ttu-id="9a26f-567">Od verze EF Core 3.0, už EF Core odešle `PRAGMA foreign_keys = 1` při otevření připojení k SQLite.</span><span class="sxs-lookup"><span data-stu-id="9a26f-567">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="9a26f-568">**Proč**</span><span class="sxs-lookup"><span data-stu-id="9a26f-568">**Why**</span></span>

<span data-ttu-id="9a26f-569">Tato změna byla provedena, protože používá EF Core `SQLitePCLRaw.bundle_e_sqlite3` ve výchozím nastavení, která zase znamená, že vynucení cizího klíče je ve výchozím nastavení zapnuté a není potřeba explicitně povolit pokaždé, když je otevřeno připojení.</span><span class="sxs-lookup"><span data-stu-id="9a26f-569">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="9a26f-570">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="9a26f-570">**Mitigations**</span></span>

<span data-ttu-id="9a26f-571">Cizí klíče jsou povolena ve výchozím nastavení SQLitePCLRaw.bundle_e_sqlite3, který se používá ve výchozím nastavení pro jádro EF Core.</span><span class="sxs-lookup"><span data-stu-id="9a26f-571">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="9a26f-572">Pro jiných případech může být povoleno cizích klíčů tak, že zadáte `Foreign Keys=True` v připojovacím řetězci.</span><span class="sxs-lookup"><span data-stu-id="9a26f-572">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="9a26f-573">Microsoft.EntityFrameworkCore.Sqlite nyní závisí na SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="9a26f-573">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="9a26f-574">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-574">**Old behavior**</span></span>

<span data-ttu-id="9a26f-575">Před EF Core 3.0 používá EF Core `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="9a26f-575">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="9a26f-576">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-576">**New behavior**</span></span>

<span data-ttu-id="9a26f-577">Od verze EF Core 3.0, pomocí EF Core `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="9a26f-577">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="9a26f-578">**Proč**</span><span class="sxs-lookup"><span data-stu-id="9a26f-578">**Why**</span></span>

<span data-ttu-id="9a26f-579">Tato změna byla provedena tak, aby používalo verzi SQLite v Iosu konzistentní s jinými platformami.</span><span class="sxs-lookup"><span data-stu-id="9a26f-579">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="9a26f-580">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="9a26f-580">**Mitigations**</span></span>

<span data-ttu-id="9a26f-581">Pokud chcete použít nativní verzi SQLite v Iosu, nakonfigurovat `Microsoft.Data.Sqlite` použít jinou `SQLitePCLRaw` sady.</span><span class="sxs-lookup"><span data-stu-id="9a26f-581">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

## <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="9a26f-582">Identifikátor GUID hodnoty jsou nyní uloženy jako TEXT na SQLite</span><span class="sxs-lookup"><span data-stu-id="9a26f-582">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="9a26f-583">Sledování problému #15078</span><span class="sxs-lookup"><span data-stu-id="9a26f-583">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="9a26f-584">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="9a26f-584">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="9a26f-585">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-585">**Old behavior**</span></span>

<span data-ttu-id="9a26f-586">Identifikátor GUID hodnoty byly dříve sored jako hodnoty objektu BLOB na SQLite.</span><span class="sxs-lookup"><span data-stu-id="9a26f-586">Guid values were previously sored as BLOB values on SQLite.</span></span>

<span data-ttu-id="9a26f-587">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-587">**New behavior**</span></span>

<span data-ttu-id="9a26f-588">Identifikátor GUID hodnoty jsou nyní sotred jako TEXT.</span><span class="sxs-lookup"><span data-stu-id="9a26f-588">Guid values are now sotred as TEXT.</span></span>

<span data-ttu-id="9a26f-589">**Proč**</span><span class="sxs-lookup"><span data-stu-id="9a26f-589">**Why**</span></span>

<span data-ttu-id="9a26f-590">Binární formát GUID není standardizované.</span><span class="sxs-lookup"><span data-stu-id="9a26f-590">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="9a26f-591">Uložení hodnot jako TEXT díky databáze více kompatibilní s jinými technologiemi.</span><span class="sxs-lookup"><span data-stu-id="9a26f-591">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="9a26f-592">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="9a26f-592">**Mitigations**</span></span>

<span data-ttu-id="9a26f-593">Spuštěním SQL takto můžete migrovat existující databáze na nový formát.</span><span class="sxs-lookup"><span data-stu-id="9a26f-593">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="9a26f-594">V EF Core můžete také pokračovat pomocí předchozí chování configuirng převaděč hodnoty těchto vlastností.</span><span class="sxs-lookup"><span data-stu-id="9a26f-594">In EF Core, you could also continue using the previous behavior by configuirng a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="9a26f-595">Microsoft.Data.Sqlite zůstává schopný načíst hodnoty identifikátoru Guid z objektu BLOB a TEXTOVÉHO sloupce; ale vzhledem k tomu, že došlo ke změně výchozího formátu pro parametry a konstant bude pravděpodobně potřeba provést akci pro většinu scénářů zahrnující identifikátory GUID.</span><span class="sxs-lookup"><span data-stu-id="9a26f-595">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="9a26f-596">Hodnoty char jsou nyní uloženy jako TEXT na SQLite</span><span class="sxs-lookup"><span data-stu-id="9a26f-596">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="9a26f-597">Sledování problému #15020</span><span class="sxs-lookup"><span data-stu-id="9a26f-597">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="9a26f-598">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="9a26f-598">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="9a26f-599">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-599">**Old behavior**</span></span>

<span data-ttu-id="9a26f-600">Hodnoty char byly dříve sored jako CELOČÍSELNÉ hodnoty na SQLite.</span><span class="sxs-lookup"><span data-stu-id="9a26f-600">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="9a26f-601">Například znak hodnotu *A* byl uložen jako celočíselnou hodnotu 65.</span><span class="sxs-lookup"><span data-stu-id="9a26f-601">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="9a26f-602">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-602">**New behavior**</span></span>

<span data-ttu-id="9a26f-603">Hodnoty char jsou nyní sotred jako TEXT.</span><span class="sxs-lookup"><span data-stu-id="9a26f-603">Char values are now sotred as TEXT.</span></span>

<span data-ttu-id="9a26f-604">**Proč**</span><span class="sxs-lookup"><span data-stu-id="9a26f-604">**Why**</span></span>

<span data-ttu-id="9a26f-605">Uložení hodnot jako TEXT je přirozenější a vytvoří databáze více kompatibilní s jinými technologiemi.</span><span class="sxs-lookup"><span data-stu-id="9a26f-605">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="9a26f-606">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="9a26f-606">**Mitigations**</span></span>

<span data-ttu-id="9a26f-607">Spuštěním SQL takto můžete migrovat existující databáze na nový formát.</span><span class="sxs-lookup"><span data-stu-id="9a26f-607">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="9a26f-608">V EF Core můžete také pokračovat pomocí předchozí chování configuirng převaděč hodnoty těchto vlastností.</span><span class="sxs-lookup"><span data-stu-id="9a26f-608">In EF Core, you could also continue using the previous behavior by configuirng a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="9a26f-609">Microsoft.Data.Sqlite také zbývá schopný načíst znakových hodnot z celé číslo a TEXT sloupců, takže určitých scénářích nevyžadují žádnou akci.</span><span class="sxs-lookup"><span data-stu-id="9a26f-609">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="9a26f-610">ID migrace jsou generovány pomocí neutrální jazykové verze kalendáře</span><span class="sxs-lookup"><span data-stu-id="9a26f-610">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="9a26f-611">Sledování problému #12978</span><span class="sxs-lookup"><span data-stu-id="9a26f-611">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="9a26f-612">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="9a26f-612">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="9a26f-613">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-613">**Old behavior**</span></span>

<span data-ttu-id="9a26f-614">ID migrace byly generovány pomocí kalendář jazykové verze currret neúmyslně.</span><span class="sxs-lookup"><span data-stu-id="9a26f-614">Migration IDs were inadvertantly generated using the currret culture's calendar.</span></span>

<span data-ttu-id="9a26f-615">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-615">**New behavior**</span></span>

<span data-ttu-id="9a26f-616">ID migrace jsou teď vždy generovány pomocí neutrální jazykové verze kalendáře (gregoriánského).</span><span class="sxs-lookup"><span data-stu-id="9a26f-616">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="9a26f-617">**Proč**</span><span class="sxs-lookup"><span data-stu-id="9a26f-617">**Why**</span></span>

<span data-ttu-id="9a26f-618">Pořadí migrace je důležité při aktualizaci databáze nebo řešení konfliktů při sloučení.</span><span class="sxs-lookup"><span data-stu-id="9a26f-618">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="9a26f-619">Pomocí neutrální kalendáře se vyhnete řazení problémy, které můžou být výsledkem členy týmu s jiným kalendáře.</span><span class="sxs-lookup"><span data-stu-id="9a26f-619">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="9a26f-620">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="9a26f-620">**Mitigations**</span></span>

<span data-ttu-id="9a26f-621">Tato změna ovlivní těm, kdo používají jiné než gregoriánské kalendářní, kde rok je větší než gregoriánském kalendáři (např. thajský buddhistický kalendář).</span><span class="sxs-lookup"><span data-stu-id="9a26f-621">This change affects anyone using a non-Gregorian calender where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="9a26f-622">Migrace stávající ID bude potřeba aktualizovat tak, aby nové migrace jsou řazeny za stávající migrace.</span><span class="sxs-lookup"><span data-stu-id="9a26f-622">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="9a26f-623">ID migrace najdete v atributu migrace soubory návrháře migrace.</span><span class="sxs-lookup"><span data-stu-id="9a26f-623">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="9a26f-624">Tabulky historie migrace je také potřeba aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="9a26f-624">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

## <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="9a26f-625">LogQueryPossibleExceptionWithAggregateOperator byl přejmenován.</span><span class="sxs-lookup"><span data-stu-id="9a26f-625">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="9a26f-626">Sledování problému #10985</span><span class="sxs-lookup"><span data-stu-id="9a26f-626">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="9a26f-627">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="9a26f-627">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="9a26f-628">**Změna**</span><span class="sxs-lookup"><span data-stu-id="9a26f-628">**Change**</span></span>

<span data-ttu-id="9a26f-629">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` byl přejmenován na `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="9a26f-629">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="9a26f-630">**Proč**</span><span class="sxs-lookup"><span data-stu-id="9a26f-630">**Why**</span></span>

<span data-ttu-id="9a26f-631">Zarovná pojmenování Tato událost upozornění s jinými událostmi upozornění.</span><span class="sxs-lookup"><span data-stu-id="9a26f-631">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="9a26f-632">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="9a26f-632">**Mitigations**</span></span>

<span data-ttu-id="9a26f-633">Použití nového názvu.</span><span class="sxs-lookup"><span data-stu-id="9a26f-633">Use the new name.</span></span> <span data-ttu-id="9a26f-634">(Všimněte si, že nedošlo ke změně číslo ID události.)</span><span class="sxs-lookup"><span data-stu-id="9a26f-634">(Note that the event ID number has not changed.)</span></span>

## <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="9a26f-635">Vysvětlení rozhraní API pro názvy omezení pro cizí klíč</span><span class="sxs-lookup"><span data-stu-id="9a26f-635">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="9a26f-636">Sledování problému #10730</span><span class="sxs-lookup"><span data-stu-id="9a26f-636">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="9a26f-637">Tato změna byla zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="9a26f-637">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="9a26f-638">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-638">**Old behavior**</span></span>

<span data-ttu-id="9a26f-639">Před EF Core 3.0 omezení pro cizí klíč názvy označovaly jako jednoduše "name".</span><span class="sxs-lookup"><span data-stu-id="9a26f-639">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="9a26f-640">Příklad:</span><span class="sxs-lookup"><span data-stu-id="9a26f-640">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="9a26f-641">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="9a26f-641">**New behavior**</span></span>

<span data-ttu-id="9a26f-642">Od verze EF Core 3.0, omezení pro cizí klíč názvy jsou dnes označovány jako "kruhového name".</span><span class="sxs-lookup"><span data-stu-id="9a26f-642">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constaint name".</span></span> <span data-ttu-id="9a26f-643">Příklad:</span><span class="sxs-lookup"><span data-stu-id="9a26f-643">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="9a26f-644">**Proč**</span><span class="sxs-lookup"><span data-stu-id="9a26f-644">**Why**</span></span>

<span data-ttu-id="9a26f-645">Tato změna přináší konzistenci pro názvy v této oblasti a také vysvětluje, že se jedná o název název cizího klíče kruhového a nikoli na sloupec nebo vlastnost, která je definována cizího klíče na.</span><span class="sxs-lookup"><span data-stu-id="9a26f-645">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constaint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="9a26f-646">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="9a26f-646">**Mitigations**</span></span>

<span data-ttu-id="9a26f-647">Použití nového názvu.</span><span class="sxs-lookup"><span data-stu-id="9a26f-647">Use the new name.</span></span>
