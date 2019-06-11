---
title: Zásadní změny v EF Core 3.0 – EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 9112d8d235237e68232aac54453d584af0edb524
ms.sourcegitcommit: b188194a1901f4d086d05765cbc5c9b8c9dc5eed
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/11/2019
ms.locfileid: "66829488"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="23d0c-102">Rozbíjející změny zahrnuté v EF Core 3.0 (aktuálně ve verzi preview)</span><span class="sxs-lookup"><span data-stu-id="23d0c-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="23d0c-103">Mějte prosím na paměti, že sady funkcí a plány budoucích verzí se vždy mohou změnit a přestože se snažíme se zachovat aktuální, nemusí odrážet naše nejnovější plány vůbec na této stránce vyprší.</span><span class="sxs-lookup"><span data-stu-id="23d0c-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="23d0c-104">Následující změny chování a rozhraní API by mohly přerušit aplikace vyvinuté pro jádro EF Core 2.2.x při upgradu na 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="23d0c-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="23d0c-105">Změny, které chceme ovlivní jen poskytovatelé databází jsou popsány v části [poskytovatel změny](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="23d0c-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="23d0c-106">Tady nejsou uvedené konce v nové vlastnosti představené z jednoho 3.0 ve verzi preview další 3.0 ve verzi Preview.</span><span class="sxs-lookup"><span data-stu-id="23d0c-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="23d0c-107">Dotazy LINQ se už nevyhodnocuje na straně klienta</span><span class="sxs-lookup"><span data-stu-id="23d0c-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="23d0c-108">[Sledování problému #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[také zjistit problém #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="23d0c-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="23d0c-109">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="23d0c-109">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="23d0c-110">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-110">**Old behavior**</span></span>

<span data-ttu-id="23d0c-111">Než 3.0 když EF Core nelze převést výraz, který byl součástí sady dotazů SQL nebo parametr, automaticky vyhodnotí výraz na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="23d0c-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="23d0c-112">Ve výchozím nastavení hodnocení klientů potencionálně nákladným výrazů aktivuje pouze upozornění.</span><span class="sxs-lookup"><span data-stu-id="23d0c-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="23d0c-113">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-113">**New behavior**</span></span>

<span data-ttu-id="23d0c-114">Od verze 3.0, EF Core umožňuje pouze výrazy v projekci nejvyšší úrovně (poslední `Select()` volání v dotazu) k vyhodnocení na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="23d0c-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="23d0c-115">Když výrazy v další části dotazu nelze převést na SQL nebo parametr, je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="23d0c-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="23d0c-116">**Proč**</span><span class="sxs-lookup"><span data-stu-id="23d0c-116">**Why**</span></span>

<span data-ttu-id="23d0c-117">Vyhodnocení automatickou klientskou dotazů umožňuje mnoho dotazů, který se spustí i v případě, že je důležité části nelze přeložit.</span><span class="sxs-lookup"><span data-stu-id="23d0c-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="23d0c-118">Toto chování může způsobit neočekávané a potenciálně škodlivé chování, které může přestat pouze zřejmé v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="23d0c-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="23d0c-119">Například podmínku v `Where()` volání, které nelze přeložit může způsobit, že všechny řádky z tabulky přesunou z databázového serveru a filtr, který bude použit na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="23d0c-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="23d0c-120">Tato situace může pokračovat nezjištěné po snadno Pokud tabulka obsahuje jenom pár řádků v vývoje, ale přístupů pevné přesun aplikace do produkčního prostředí, kde tabulka může obsahovat miliony řádků.</span><span class="sxs-lookup"><span data-stu-id="23d0c-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="23d0c-121">Upozornění vyhodnocení klienta dokázaly také příliš jednoduché ignorovat během vývoje.</span><span class="sxs-lookup"><span data-stu-id="23d0c-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="23d0c-122">Kromě toho může automatickou klientskou vyhodnocení způsobit problémy, ve kterých vylepšení překladu dotazu pro konkrétní výrazy způsobila neúmyslnému zásadní změny mezi verzí.</span><span class="sxs-lookup"><span data-stu-id="23d0c-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="23d0c-123">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="23d0c-123">**Mitigations**</span></span>

<span data-ttu-id="23d0c-124">Pokud dotaz nelze přeložit plně, pak buď přepište dotaz ve formuláři, který lze přeložit, nebo použijte `AsEnumerable()`, `ToList()`, nebo podobný explicitně přenést data zpět do klienta ve kterém pak může být dále zpracovány pomocí LINQ na objekty.</span><span class="sxs-lookup"><span data-stu-id="23d0c-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="23d0c-125">Entity Framework Core už nejsou součástí sdíleného rozhraní ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="23d0c-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="23d0c-126">Oznámení týkající se sledování problému #325</span><span class="sxs-lookup"><span data-stu-id="23d0c-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="23d0c-127">Tato změna se používá v ASP.NET Core 3.0 ve verzi preview 1.</span><span class="sxs-lookup"><span data-stu-id="23d0c-127">This change is introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="23d0c-128">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-128">**Old behavior**</span></span>

<span data-ttu-id="23d0c-129">Před ASP.NET Core 3.0, když se přidá odkaz na balíček pro `Microsoft.AspNetCore.App` nebo `Microsoft.AspNetCore.All`, měl by obsahovat EF Core a některé EF Core zprostředkovatele dat, jako jsou zprostředkovatele SQL Server.</span><span class="sxs-lookup"><span data-stu-id="23d0c-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="23d0c-130">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-130">**New behavior**</span></span>

<span data-ttu-id="23d0c-131">Od verze 3.0, rozhraní ASP.NET Core sdílené neobsahuje EF Core nebo žádným zprostředkovatelům dat. EF Core.</span><span class="sxs-lookup"><span data-stu-id="23d0c-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="23d0c-132">**Proč**</span><span class="sxs-lookup"><span data-stu-id="23d0c-132">**Why**</span></span>

<span data-ttu-id="23d0c-133">Před touto změnou získávání EF Core vyžaduje jiný postup v závislosti na tom, jestli aplikace cílené ASP.NET Core a SQL Server, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="23d0c-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="23d0c-134">Upgrade ASP.NET Core také vynutit upgrade EF Core a zprostředkovatele SQL Server, který nemusí být vždy žádoucí.</span><span class="sxs-lookup"><span data-stu-id="23d0c-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="23d0c-135">Díky této změně se možnosti načítání EF Core je stejná ve všech zprostředkovatelů, podporovaná implementace .NET a typy aplikací.</span><span class="sxs-lookup"><span data-stu-id="23d0c-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="23d0c-136">Vývojáři také nyní mohou ovládat přesně při upgradu EF Core a EF Core zprostředkovatelé dat.</span><span class="sxs-lookup"><span data-stu-id="23d0c-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="23d0c-137">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="23d0c-137">**Mitigations**</span></span>

<span data-ttu-id="23d0c-138">V aplikaci ASP.NET Core 3.0 nebo jakékoli jiné podporované aplikace použít EF Core, explicitně přidáte odkaz na balíček k poskytovateli databáze EF Core, který bude aplikace používat.</span><span class="sxs-lookup"><span data-stu-id="23d0c-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="23d0c-139">EF Core nástroji příkazového řádku dotnet ef, už nejsou součástí sady .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="23d0c-139">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="23d0c-140">Sledování problému #14016</span><span class="sxs-lookup"><span data-stu-id="23d0c-140">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="23d0c-141">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4 a odpovídající verze sady .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="23d0c-141">This change is introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="23d0c-142">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-142">**Old behavior**</span></span>

<span data-ttu-id="23d0c-143">Než 3.0 `dotnet ef` nástroj je zahrnutý v .NET Core SDK a se snadno k dispozici pro použití z příkazového řádku z libovolného projektu bez nutnosti pár kroků navíc.</span><span class="sxs-lookup"><span data-stu-id="23d0c-143">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="23d0c-144">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-144">**New behavior**</span></span>

<span data-ttu-id="23d0c-145">Od verze 3.0, sady .NET SDK se nenachází `dotnet ef` nástroj, takže než budete moct použít, budete mít explicitně ji nainstalovat jako nástroj pro místní nebo globální.</span><span class="sxs-lookup"><span data-stu-id="23d0c-145">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="23d0c-146">**Proč**</span><span class="sxs-lookup"><span data-stu-id="23d0c-146">**Why**</span></span>

<span data-ttu-id="23d0c-147">Tato změna umožňuje distribuci a aktualizaci `dotnet ef` jako regulární nástroj rozhraní příkazového řádku .NET na webu NuGet, konzistentní s skutečnost, že je EF Core 3.0 také vždy distribuován jako balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="23d0c-147">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="23d0c-148">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="23d0c-148">**Mitigations**</span></span>

<span data-ttu-id="23d0c-149">Abyste mohli spravovat migrace nebo vygenerované uživatelské rozhraní `DbContext`, nainstalovat `dotnet-ef` pomocí `dotnet tool install` příkazu.</span><span class="sxs-lookup"><span data-stu-id="23d0c-149">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` using the `dotnet tool install` command.</span></span>
<span data-ttu-id="23d0c-150">Například můžete ji nainstalovat jako globální nástroj, zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="23d0c-150">For example, to install it as a global tool, you can type this command:</span></span>

  ``` console
  $ dotnet tool install --global dotnet-ef --version <exact-version>
  ```

<span data-ttu-id="23d0c-151">Můžete také získat jeho místní nástroj při obnovování závislosti projektu, který deklaruje jako závislost pomocí nástroje [soubor manifestu nástroje](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="23d0c-151">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

## <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="23d0c-152">Byla přejmenovaná FromSql ExecuteSql a ExecuteSqlAsync</span><span class="sxs-lookup"><span data-stu-id="23d0c-152">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="23d0c-153">Sledování problému #10996</span><span class="sxs-lookup"><span data-stu-id="23d0c-153">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="23d0c-154">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="23d0c-154">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="23d0c-155">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-155">**Old behavior**</span></span>

<span data-ttu-id="23d0c-156">Před EF Core 3.0 byly tyto názvy metod přetížení pro práci s normální řetězec nebo řetězec, který by měl být interpolovaných do SQL a parametry.</span><span class="sxs-lookup"><span data-stu-id="23d0c-156">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="23d0c-157">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-157">**New behavior**</span></span>

<span data-ttu-id="23d0c-158">Od verze EF Core 3.0, použijte `FromSqlRaw`, `ExecuteSqlRaw`, a `ExecuteSqlRawAsync` vytvořit parametrický dotaz, kde parametry jsou předány samostatně z řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="23d0c-158">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="23d0c-159">Příklad:</span><span class="sxs-lookup"><span data-stu-id="23d0c-159">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="23d0c-160">Použití `FromSqlInterpolated`, `ExecuteSqlInterpolated`, a `ExecuteSqlInterpolatedAsync` vytvořit parametrický dotaz, kde parametry jsou předány jako součást dotazu interpolované řetězce.</span><span class="sxs-lookup"><span data-stu-id="23d0c-160">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="23d0c-161">Příklad:</span><span class="sxs-lookup"><span data-stu-id="23d0c-161">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="23d0c-162">Všimněte si, že oba dotazy výše uvedené vytvoří stejný parametrizovaného dotazu SQL se stejnými parametry SQL.</span><span class="sxs-lookup"><span data-stu-id="23d0c-162">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="23d0c-163">**Proč**</span><span class="sxs-lookup"><span data-stu-id="23d0c-163">**Why**</span></span>

<span data-ttu-id="23d0c-164">Přetížení metody, jako je to velmi usnadňují omylem volat metodu nezpracovaného řetězce, pokud bylo záměrem pro volání metody interpolovaný řetězec a naopak.</span><span class="sxs-lookup"><span data-stu-id="23d0c-164">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="23d0c-165">To může vést k dotazům naprostou když měla být parametrizovány.</span><span class="sxs-lookup"><span data-stu-id="23d0c-165">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="23d0c-166">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="23d0c-166">**Mitigations**</span></span>

<span data-ttu-id="23d0c-167">Přepněte na nové názvy metod.</span><span class="sxs-lookup"><span data-stu-id="23d0c-167">Switch to use the new method names.</span></span>

## <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="23d0c-168">FromSql metody lze zadat pouze na dotaz kořeny</span><span class="sxs-lookup"><span data-stu-id="23d0c-168">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="23d0c-169">Sledování problému #15704</span><span class="sxs-lookup"><span data-stu-id="23d0c-169">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="23d0c-170">Tato změna je zavedená v EF Core 3.0-preview 6.</span><span class="sxs-lookup"><span data-stu-id="23d0c-170">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="23d0c-171">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-171">**Old behavior**</span></span>

<span data-ttu-id="23d0c-172">Před EF Core 3.0 `FromSql` může být zadaná metoda kdekoli v dotazu.</span><span class="sxs-lookup"><span data-stu-id="23d0c-172">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="23d0c-173">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-173">**New behavior**</span></span>

<span data-ttu-id="23d0c-174">Od verze EF Core 3.0, nový `FromSqlRaw` a `FromSqlInterpolated` metody (které nahradit `FromSql`) lze zadat pouze na dotaz kořeny, to znamená přímo na `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="23d0c-174">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="23d0c-175">Pokus zadat je někde jinde způsobí chybu kompilace.</span><span class="sxs-lookup"><span data-stu-id="23d0c-175">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="23d0c-176">**Proč**</span><span class="sxs-lookup"><span data-stu-id="23d0c-176">**Why**</span></span>

<span data-ttu-id="23d0c-177">Určení `FromSql` kdekoli jiné než na `DbSet` bez přidání význam nebo přidanou hodnotu a v některých případech může způsobit nejednoznačnost.</span><span class="sxs-lookup"><span data-stu-id="23d0c-177">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="23d0c-178">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="23d0c-178">**Mitigations**</span></span>

<span data-ttu-id="23d0c-179">`FromSql` volání by měl být přímo na přesunout `DbSet` na které se vztahují.</span><span class="sxs-lookup"><span data-stu-id="23d0c-179">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

## <a name="query-execution-is-logged-at-debug-level"></a><span data-ttu-id="23d0c-180">Provádění dotazu se protokoluje při ladění na úrovni</span><span class="sxs-lookup"><span data-stu-id="23d0c-180">Query execution is logged at Debug level</span></span>

[<span data-ttu-id="23d0c-181">Sledování problému #14523</span><span class="sxs-lookup"><span data-stu-id="23d0c-181">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="23d0c-182">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="23d0c-182">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="23d0c-183">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-183">**Old behavior**</span></span>

<span data-ttu-id="23d0c-184">Před EF Core 3.0, provádění dotazů a jiných příkazů protokolu byla zaznamenána v `Info` úroveň.</span><span class="sxs-lookup"><span data-stu-id="23d0c-184">Before EF Core 3.0, execution of queries and other commands was logged at the `Info` level.</span></span>

<span data-ttu-id="23d0c-185">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-185">**New behavior**</span></span>

<span data-ttu-id="23d0c-186">Od verze EF Core 3.0, protokolování spuštění příkazu/SQL je na `Debug` úroveň.</span><span class="sxs-lookup"><span data-stu-id="23d0c-186">Starting with EF Core 3.0, logging of command/SQL execution is at the `Debug` level.</span></span>

<span data-ttu-id="23d0c-187">**Proč**</span><span class="sxs-lookup"><span data-stu-id="23d0c-187">**Why**</span></span>

<span data-ttu-id="23d0c-188">Tato změna byla provedena jak snížit šum na `Info` úrovně protokolování.</span><span class="sxs-lookup"><span data-stu-id="23d0c-188">This change was made to reduce the noise at the `Info` log level.</span></span>

<span data-ttu-id="23d0c-189">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="23d0c-189">**Mitigations**</span></span>

<span data-ttu-id="23d0c-190">Tato událost protokolování je definována `RelationalEventId.CommandExecuting` s ID události 20100.</span><span class="sxs-lookup"><span data-stu-id="23d0c-190">This logging event is defined by `RelationalEventId.CommandExecuting` with event ID 20100.</span></span>
<span data-ttu-id="23d0c-191">Do protokolu SQL na `Info` úroveň znovu, explicitně nakonfigurovat na úrovni `OnConfiguring` nebo `AddDbContext`.</span><span class="sxs-lookup"><span data-stu-id="23d0c-191">To log SQL at the `Info` level again, explicitly configure the level in `OnConfiguring` or `AddDbContext`.</span></span>
<span data-ttu-id="23d0c-192">Příklad:</span><span class="sxs-lookup"><span data-stu-id="23d0c-192">For example:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Info)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="23d0c-193">Dočasné hodnoty klíče už nejsou nastavené na instancí entit</span><span class="sxs-lookup"><span data-stu-id="23d0c-193">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="23d0c-194">Sledování problému #12378</span><span class="sxs-lookup"><span data-stu-id="23d0c-194">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="23d0c-195">Tato změna je zavedená v EF Core 3.0 – náhled 2.</span><span class="sxs-lookup"><span data-stu-id="23d0c-195">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="23d0c-196">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-196">**Old behavior**</span></span>

<span data-ttu-id="23d0c-197">Před EF Core 3.0 dočasné hodnoty přiřadily se všechny vlastnosti klíče, které by později skutečné hodnoty generován databází.</span><span class="sxs-lookup"><span data-stu-id="23d0c-197">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="23d0c-198">Obvykle tyto dočasné hodnoty byly velké záporná čísla.</span><span class="sxs-lookup"><span data-stu-id="23d0c-198">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="23d0c-199">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-199">**New behavior**</span></span>

<span data-ttu-id="23d0c-200">Od verze 3.0, EF Core ukládá dočasné hodnotu klíče jako součást informací o sledování entity a ponechá samotné beze změny vlastnosti klíče.</span><span class="sxs-lookup"><span data-stu-id="23d0c-200">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="23d0c-201">**Proč**</span><span class="sxs-lookup"><span data-stu-id="23d0c-201">**Why**</span></span>

<span data-ttu-id="23d0c-202">Tato změna byla provedena zabránit chybně stávají trvalé při entita, která byla dříve sledován pomocí funkce některé dočasné hodnoty klíče `DbContext` instance se přesune na jiný `DbContext` instance.</span><span class="sxs-lookup"><span data-stu-id="23d0c-202">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="23d0c-203">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="23d0c-203">**Mitigations**</span></span>

<span data-ttu-id="23d0c-204">Na staré chování může záviset aplikace, které přiřazují hodnoty primárního klíče na cizí klíče formuláře přidružení mezi entitami, pokud primární klíče generované úložištěm a patří do entity v `Added` stavu.</span><span class="sxs-lookup"><span data-stu-id="23d0c-204">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="23d0c-205">Toho se lze vyvarovat podle:</span><span class="sxs-lookup"><span data-stu-id="23d0c-205">This can be avoided by:</span></span>
* <span data-ttu-id="23d0c-206">Bez použití klíče generované úložištěm.</span><span class="sxs-lookup"><span data-stu-id="23d0c-206">Not using store-generated keys.</span></span>
* <span data-ttu-id="23d0c-207">Nastavení vlastnosti navigace na vztahů namísto nastavování hodnot cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="23d0c-207">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="23d0c-208">Získejte skutečné hodnoty dočasné klíče z informací o sledování entity.</span><span class="sxs-lookup"><span data-stu-id="23d0c-208">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="23d0c-209">Například `context.Entry(blog).Property(e => e.Id).CurrentValue` i v případě vrátí hodnotu dočasné `blog.Id` samotný nebyla nastavena.</span><span class="sxs-lookup"><span data-stu-id="23d0c-209">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="23d0c-210">Metoda DetectChanges respektuje klíčových hodnot generovaných úložištěm</span><span class="sxs-lookup"><span data-stu-id="23d0c-210">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="23d0c-211">Sledování problému #14616</span><span class="sxs-lookup"><span data-stu-id="23d0c-211">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="23d0c-212">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="23d0c-212">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="23d0c-213">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-213">**Old behavior**</span></span>

<span data-ttu-id="23d0c-214">Před EF Core 3.0 Nesledované entity objevila `DetectChanges` mají sledovat v `Added` stavu a vložené jako nový řádek, kdy `SaveChanges` je volána.</span><span class="sxs-lookup"><span data-stu-id="23d0c-214">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="23d0c-215">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-215">**New behavior**</span></span>

<span data-ttu-id="23d0c-216">Od verze EF Core 3.0, pokud používá entity generované hodnoty klíče a některá z hodnot klíče je nastavena, pak entity se bude sledovat v `Modified` stavu.</span><span class="sxs-lookup"><span data-stu-id="23d0c-216">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="23d0c-217">To znamená, že řádek entity se předpokládá, že existují a budou aktualizace `SaveChanges` je volána.</span><span class="sxs-lookup"><span data-stu-id="23d0c-217">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="23d0c-218">Pokud není nastavena hodnota klíče, nebo pokud není typ entity pomocí vygenerované klíče, pak nová entita se bude dál sledovat jako `Added` stejně jako v předchozích verzích.</span><span class="sxs-lookup"><span data-stu-id="23d0c-218">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="23d0c-219">**Proč**</span><span class="sxs-lookup"><span data-stu-id="23d0c-219">**Why**</span></span>

<span data-ttu-id="23d0c-220">Tato změna byla provedena na umožňují snadněji a konzistentnější pro práci s grafy odpojené entity při použití klíče generované úložištěm.</span><span class="sxs-lookup"><span data-stu-id="23d0c-220">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="23d0c-221">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="23d0c-221">**Mitigations**</span></span>

<span data-ttu-id="23d0c-222">Tato změna může přerušit aplikace, pokud typ entity je nakonfigurovaný na použití vygenerovat klíče, ale hodnoty klíče jsou explicitně nastavené pro nové instance.</span><span class="sxs-lookup"><span data-stu-id="23d0c-222">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="23d0c-223">Oprava je explicitně konfigurovat klíčové vlastnosti nejsou generovány pomocí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="23d0c-223">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="23d0c-224">Například pomocí rozhraní fluent API:</span><span class="sxs-lookup"><span data-stu-id="23d0c-224">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="23d0c-225">Nebo s anotacemi dat:</span><span class="sxs-lookup"><span data-stu-id="23d0c-225">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="23d0c-226">Kaskádové odstranění teď dojde okamžitě ve výchozím nastavení</span><span class="sxs-lookup"><span data-stu-id="23d0c-226">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="23d0c-227">Sledování problému #10114</span><span class="sxs-lookup"><span data-stu-id="23d0c-227">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="23d0c-228">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="23d0c-228">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="23d0c-229">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-229">**Old behavior**</span></span>

<span data-ttu-id="23d0c-230">Než 3.0 EF Core použít kaskádové akce (odstranění závislých položek při odstranění požadovaný instanční objekt nebo když porušeno vztah k požadované instanční objekt) nebyly provedeny dokud byla volána metoda SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="23d0c-230">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="23d0c-231">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-231">**New behavior**</span></span>

<span data-ttu-id="23d0c-232">Od verze 3.0, EF Core provede kaskádové akce ihned po zjištění spouštěcí podmínky.</span><span class="sxs-lookup"><span data-stu-id="23d0c-232">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="23d0c-233">Například volání `context.Remove()` odstranit instančního objektu entity se výsledek ve všech sledovat související požadované položky závislé na také nastavena na `Deleted` okamžitě.</span><span class="sxs-lookup"><span data-stu-id="23d0c-233">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="23d0c-234">**Proč**</span><span class="sxs-lookup"><span data-stu-id="23d0c-234">**Why**</span></span>

<span data-ttu-id="23d0c-235">Tato změna byla provedena na zlepšení uživatelského rozhraní pro datových vazbách a auditování scénáře, kdy je potřeba vysvětlit entit, které se odstraní _před_ `SaveChanges` je volána.</span><span class="sxs-lookup"><span data-stu-id="23d0c-235">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="23d0c-236">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="23d0c-236">**Mitigations**</span></span>

<span data-ttu-id="23d0c-237">Předchozí chování lze obnovit nastavením na `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="23d0c-237">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="23d0c-238">Příklad:</span><span class="sxs-lookup"><span data-stu-id="23d0c-238">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="23d0c-239">DeleteBehavior.Restrict má sémantiku čisticího modulu</span><span class="sxs-lookup"><span data-stu-id="23d0c-239">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="23d0c-240">Sledování problému #12661</span><span class="sxs-lookup"><span data-stu-id="23d0c-240">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="23d0c-241">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 5.</span><span class="sxs-lookup"><span data-stu-id="23d0c-241">This change is introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="23d0c-242">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-242">**Old behavior**</span></span>

<span data-ttu-id="23d0c-243">Než 3.0 `DeleteBehavior.Restrict` vytvořit cizí klíče v databázi s `Restrict` sémantiku, ale také změněné interní opravy není zřejmé způsobem.</span><span class="sxs-lookup"><span data-stu-id="23d0c-243">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="23d0c-244">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-244">**New behavior**</span></span>

<span data-ttu-id="23d0c-245">Od verze 3.0, `DeleteBehavior.Restrict` zajistí, že cizí klíče jsou vytvořeny pomocí `Restrict` sémantiku – tedy žádná cascades; throw na porušení omezení – bez vliv i na EF interní opravy.</span><span class="sxs-lookup"><span data-stu-id="23d0c-245">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="23d0c-246">**Proč**</span><span class="sxs-lookup"><span data-stu-id="23d0c-246">**Why**</span></span>

<span data-ttu-id="23d0c-247">Tato změna byla provedena na zlepšení uživatelského rozhraní pro použití `DeleteBehavior` intuitivní způsobem, bez neočekávané vedlejší účinky.</span><span class="sxs-lookup"><span data-stu-id="23d0c-247">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="23d0c-248">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="23d0c-248">**Mitigations**</span></span>

<span data-ttu-id="23d0c-249">Předchozí chování můžete obnovit pomocí `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="23d0c-249">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="23d0c-250">Typy dotazů jsou spojeny s typy entit</span><span class="sxs-lookup"><span data-stu-id="23d0c-250">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="23d0c-251">Sledování problému #14194</span><span class="sxs-lookup"><span data-stu-id="23d0c-251">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="23d0c-252">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="23d0c-252">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="23d0c-253">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-253">**Old behavior**</span></span>

<span data-ttu-id="23d0c-254">Před EF Core 3.0 [typy dotazů](xref:core/modeling/query-types) byly prostředky provádět dotazy na data, která nedefinuje primární klíč strukturovaným způsobem.</span><span class="sxs-lookup"><span data-stu-id="23d0c-254">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="23d0c-255">To znamená typ dotazu byla použita pro mapování typů entit bez klíčů (pravděpodobně ze zobrazení, ale pravděpodobně z tabulky) při pravidelných entity typ byl použit při klíč byl k dispozici (více pravděpodobně z tabulky, ale pravděpodobně ze zobrazení).</span><span class="sxs-lookup"><span data-stu-id="23d0c-255">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="23d0c-256">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-256">**New behavior**</span></span>

<span data-ttu-id="23d0c-257">Typ dotazu teď bude pouze typ entity bez primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="23d0c-257">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="23d0c-258">Typy entit bez kódu mají stejné funkce jako typy dotazů v předchozích verzích.</span><span class="sxs-lookup"><span data-stu-id="23d0c-258">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="23d0c-259">**Proč**</span><span class="sxs-lookup"><span data-stu-id="23d0c-259">**Why**</span></span>

<span data-ttu-id="23d0c-260">Tato změna byla provedena, abyste snížili nejasnosti kolem účel typy dotazů.</span><span class="sxs-lookup"><span data-stu-id="23d0c-260">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="23d0c-261">Konkrétně jsou typy bez klíčů entit a z tohoto důvodu jsou ze své podstaty jen pro čtení, ale by neměl být použit pouze z důvodu typu entity musí být jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="23d0c-261">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="23d0c-262">Podobně jsou často namapované na zobrazení, ale je to jenom, protože zobrazení není často definují klíče.</span><span class="sxs-lookup"><span data-stu-id="23d0c-262">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="23d0c-263">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="23d0c-263">**Mitigations**</span></span>

<span data-ttu-id="23d0c-264">Následující části rozhraní API jsou teď zastaralé:</span><span class="sxs-lookup"><span data-stu-id="23d0c-264">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="23d0c-265">**`ModelBuilder.Query<>()`** – Místo toho `ModelBuilder.Entity<>().HasNoKey()` musí být volána k označení typu entity tak, že má žádné klíče.</span><span class="sxs-lookup"><span data-stu-id="23d0c-265">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="23d0c-266">To by stále probíhá konfigurace podle konvence se vyhnete chybné konfigurace, když primární klíč se očekává, ale neodpovídá konvenci.</span><span class="sxs-lookup"><span data-stu-id="23d0c-266">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="23d0c-267">**`DbQuery<>`** – Místo toho `DbSet<>` by měla sloužit.</span><span class="sxs-lookup"><span data-stu-id="23d0c-267">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="23d0c-268">**`DbContext.Query<>()`** – Místo toho `DbContext.Set<>()` by měla sloužit.</span><span class="sxs-lookup"><span data-stu-id="23d0c-268">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="23d0c-269">Došlo ke změně konfigurace rozhraní API pro vlastní typ relace</span><span class="sxs-lookup"><span data-stu-id="23d0c-269">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="23d0c-270">[Sledování problému #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[sledování problému #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[sledování problému #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="23d0c-270">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="23d0c-271">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="23d0c-271">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="23d0c-272">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-272">**Old behavior**</span></span>

<span data-ttu-id="23d0c-273">Před EF Core 3.0 byla provedena konfigurace vlastněné vztah přímo po `OwnsOne` nebo `OwnsMany` volání.</span><span class="sxs-lookup"><span data-stu-id="23d0c-273">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="23d0c-274">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-274">**New behavior**</span></span>

<span data-ttu-id="23d0c-275">Od verze EF Core 3.0, že už fluent API pro konfiguraci vlastnosti navigace vlastníkovi použitím `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="23d0c-275">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="23d0c-276">Příklad:</span><span class="sxs-lookup"><span data-stu-id="23d0c-276">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="23d0c-277">Konfigurace související se o vztah mezi vlastníka a vlastní by měl být zřetězené teď po `WithOwner()` podobně jako na konfiguraci jiné vztahy.</span><span class="sxs-lookup"><span data-stu-id="23d0c-277">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="23d0c-278">Při konfiguraci pro vlastní typ, samotný by stále možné zřetězit po `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="23d0c-278">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="23d0c-279">Příklad:</span><span class="sxs-lookup"><span data-stu-id="23d0c-279">For example:</span></span>

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

<span data-ttu-id="23d0c-280">Kromě volá `Entity()`, `HasOne()`, nebo `Set()` s typem vlastnictví cíl se vyvolají výjimku.</span><span class="sxs-lookup"><span data-stu-id="23d0c-280">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="23d0c-281">**Proč**</span><span class="sxs-lookup"><span data-stu-id="23d0c-281">**Why**</span></span>

<span data-ttu-id="23d0c-282">Tato změna byla provedena vytvořit jasnější oddělení mezi konfigurací samotného typu vlastnictví a _vztah k_ typ vlastnictví.</span><span class="sxs-lookup"><span data-stu-id="23d0c-282">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="23d0c-283">Tím zase nejednoznačnosti a záměny kolem metody, jako je `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="23d0c-283">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="23d0c-284">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="23d0c-284">**Mitigations**</span></span>

<span data-ttu-id="23d0c-285">Změna konfigurace vlastněné typ vztahů používat nové plochy rozhraní API, jak je znázorněno v příkladu výše.</span><span class="sxs-lookup"><span data-stu-id="23d0c-285">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="23d0c-286">Závislých položek sdílení v tabulce k objektu zabezpečení jsou teď nepovinné.</span><span class="sxs-lookup"><span data-stu-id="23d0c-286">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="23d0c-287">Sledování problému #9005</span><span class="sxs-lookup"><span data-stu-id="23d0c-287">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="23d0c-288">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="23d0c-288">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="23d0c-289">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-289">**Old behavior**</span></span>

<span data-ttu-id="23d0c-290">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="23d0c-290">Consider the following model:</span></span>
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
<span data-ttu-id="23d0c-291">Před EF Core 3.0, pokud `OrderDetails` vlastní `Order` nebo explicitně namapované na stejnou tabulku a `OrderDetails` instance byla požadována vždy při přidání nového `Order`.</span><span class="sxs-lookup"><span data-stu-id="23d0c-291">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="23d0c-292">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-292">**New behavior**</span></span>

<span data-ttu-id="23d0c-293">Od verze 3.0, EF Core umožňuje přidat `Order` bez `OrderDetails` a mapuje všechny `OrderDetails` vlastnosti s výjimkou primární klíč pro sloupce s možnou hodnotou Null.</span><span class="sxs-lookup"><span data-stu-id="23d0c-293">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="23d0c-294">Při dotazování na EF Core sady `OrderDetails` k `null` Pokud některou z jejích požadovaných vlastností nemá žádnou hodnotu nebo nemá žádné požadované vlastnosti kromě primárního klíče a všechny vlastnosti jsou `null`.</span><span class="sxs-lookup"><span data-stu-id="23d0c-294">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="23d0c-295">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="23d0c-295">**Mitigations**</span></span>

<span data-ttu-id="23d0c-296">Pokud váš model obsahuje tabulku sdílení závislé se všemi sloupci volitelné, ale navigace ukazatel není má být `null` pak aplikace by měla upravit tak, aby zpracovat případy při navigaci `null`.</span><span class="sxs-lookup"><span data-stu-id="23d0c-296">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="23d0c-297">Pokud to není možné požadovanou vlastnost měla být přidána do typu entity nebo musí mít alespoň jednu vlastnost non -`null` přiřazena hodnota.</span><span class="sxs-lookup"><span data-stu-id="23d0c-297">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

## <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="23d0c-298">Všechny entity sdílení tabulku se sloupcem tokenů souběžnosti mít mapování na vlastnost</span><span class="sxs-lookup"><span data-stu-id="23d0c-298">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="23d0c-299">Sledování problému #14154</span><span class="sxs-lookup"><span data-stu-id="23d0c-299">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="23d0c-300">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="23d0c-300">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="23d0c-301">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-301">**Old behavior**</span></span>

<span data-ttu-id="23d0c-302">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="23d0c-302">Consider the following model:</span></span>
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
<span data-ttu-id="23d0c-303">Před EF Core 3.0, pokud `OrderDetails` vlastní `Order` nebo explicitně namapované na stejnou tabulku následně aktualizovat jenom `OrderDetails` neaktualizuje `Version` hodnoty na klientovi a příští aktualizace se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="23d0c-303">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="23d0c-304">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-304">**New behavior**</span></span>

<span data-ttu-id="23d0c-305">Od verze 3.0, rozšíří EF Core nové `Version` hodnota, která se `Order` Pokud vlastní `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="23d0c-305">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="23d0c-306">V opačném případě dojde k výjimce během ověření modelu.</span><span class="sxs-lookup"><span data-stu-id="23d0c-306">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="23d0c-307">**Proč**</span><span class="sxs-lookup"><span data-stu-id="23d0c-307">**Why**</span></span>

<span data-ttu-id="23d0c-308">Tato změna byla provedena, když se aktualizuje jenom jeden z entity, které jsou namapované na stejnou tabulku, aby hodnota tokenu zastaralé souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="23d0c-308">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="23d0c-309">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="23d0c-309">**Mitigations**</span></span>

<span data-ttu-id="23d0c-310">Všechny entity v tabulce pro sdílení obsahu se mají zahrnout vlastnost, která je namapovaná na sloupci tokenů souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="23d0c-310">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="23d0c-311">Je možné vytvořit, jeden v stínové stavu:</span><span class="sxs-lookup"><span data-stu-id="23d0c-311">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

## <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="23d0c-312">Vlastnosti zděděné nenamapované typy jsou nyní mapovány do jednoho sloupce pro všechny odvozené typy</span><span class="sxs-lookup"><span data-stu-id="23d0c-312">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="23d0c-313">Sledování problému #13998</span><span class="sxs-lookup"><span data-stu-id="23d0c-313">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="23d0c-314">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="23d0c-314">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="23d0c-315">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-315">**Old behavior**</span></span>

<span data-ttu-id="23d0c-316">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="23d0c-316">Consider the following model:</span></span>
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

<span data-ttu-id="23d0c-317">Před EF Core 3.0 `ShippingAddress` vlastnost by být mapována k oddělení sloupců pro `BulkOrder` a `Order` ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="23d0c-317">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="23d0c-318">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-318">**New behavior**</span></span>

<span data-ttu-id="23d0c-319">Od verze 3.0, EF Core vytvoří pouze jeden sloupec pro `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="23d0c-319">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="23d0c-320">**Proč**</span><span class="sxs-lookup"><span data-stu-id="23d0c-320">**Why**</span></span>

<span data-ttu-id="23d0c-321">Staré chování nebyl očekáván.</span><span class="sxs-lookup"><span data-stu-id="23d0c-321">The old behavoir was unexpected.</span></span>

<span data-ttu-id="23d0c-322">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="23d0c-322">**Mitigations**</span></span>

<span data-ttu-id="23d0c-323">Vlastnost je stále explicitně mapovat pro oddělení sloupců v odvozených typů:</span><span class="sxs-lookup"><span data-stu-id="23d0c-323">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="23d0c-324">Konvence vlastnost cizího klíče už neodpovídá stejný název jako vlastnost instančního objektu</span><span class="sxs-lookup"><span data-stu-id="23d0c-324">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="23d0c-325">Sledování problému #13274</span><span class="sxs-lookup"><span data-stu-id="23d0c-325">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="23d0c-326">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="23d0c-326">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="23d0c-327">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-327">**Old behavior**</span></span>

<span data-ttu-id="23d0c-328">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="23d0c-328">Consider the following model:</span></span>
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
<span data-ttu-id="23d0c-329">Před EF Core 3.0 `CustomerId` vlastnost se použije pro cizí klíč konvencí.</span><span class="sxs-lookup"><span data-stu-id="23d0c-329">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="23d0c-330">Nicméně pokud `Order` je typ vlastnictví, a potom to by také provést `CustomerId` primární klíč a to se většinou očekává.</span><span class="sxs-lookup"><span data-stu-id="23d0c-330">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="23d0c-331">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-331">**New behavior**</span></span>

<span data-ttu-id="23d0c-332">Od verze 3.0, EF Core nesnaží při využívání vlastností cizího klíče podle konvence, pokud mají stejný název jako vlastnost instančního objektu.</span><span class="sxs-lookup"><span data-stu-id="23d0c-332">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="23d0c-333">Název instančního objektu typu zřetězený s názvem hlavní vlastnosti a název navigační zřetězená s vzory názvů vlastnosti principal budou stále odpovídat.</span><span class="sxs-lookup"><span data-stu-id="23d0c-333">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="23d0c-334">Příklad:</span><span class="sxs-lookup"><span data-stu-id="23d0c-334">For example:</span></span>

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

<span data-ttu-id="23d0c-335">**Proč**</span><span class="sxs-lookup"><span data-stu-id="23d0c-335">**Why**</span></span>

<span data-ttu-id="23d0c-336">Tato změna byla provedena na chybně nedefinujte vlastnost primárního klíče typ vlastnictví.</span><span class="sxs-lookup"><span data-stu-id="23d0c-336">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="23d0c-337">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="23d0c-337">**Mitigations**</span></span>

<span data-ttu-id="23d0c-338">Pokud byla vlastnost má být cizí klíč a proto část primárního klíče a pak explicitně jako takový ho nakonfigurujte.</span><span class="sxs-lookup"><span data-stu-id="23d0c-338">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="23d0c-339">Připojení k databázi je nyní uzavřeno, pokud není využito už před objektu TransactionScope byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="23d0c-339">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="23d0c-340">Sledování problému #14218</span><span class="sxs-lookup"><span data-stu-id="23d0c-340">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="23d0c-341">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="23d0c-341">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="23d0c-342">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-342">**Old behavior**</span></span>

<span data-ttu-id="23d0c-343">Před EF Core 3.0, pokud kontext otevře připojení k uvnitř `TransactionScope`, připojení zůstane otevřená, při aktuální `TransactionScope` je aktivní.</span><span class="sxs-lookup"><span data-stu-id="23d0c-343">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="23d0c-344">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-344">**New behavior**</span></span>

<span data-ttu-id="23d0c-345">Od verze 3.0, EF Core uzavře připojení co nejdříve po dokončení jeho použití.</span><span class="sxs-lookup"><span data-stu-id="23d0c-345">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="23d0c-346">**Proč**</span><span class="sxs-lookup"><span data-stu-id="23d0c-346">**Why**</span></span>

<span data-ttu-id="23d0c-347">Tato změna umožňuje použití několika kontextech v rámci stejného `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="23d0c-347">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="23d0c-348">Nové chování také odpovídající EF6.</span><span class="sxs-lookup"><span data-stu-id="23d0c-348">The new behavior also matches EF6.</span></span>

<span data-ttu-id="23d0c-349">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="23d0c-349">**Mitigations**</span></span>

<span data-ttu-id="23d0c-350">Pokud je nutné připojení zůstat otevřené explicitní volání konstruktoru `OpenConnection()` zajistí, že EF Core nezavírá je předčasně ukončena:</span><span class="sxs-lookup"><span data-stu-id="23d0c-350">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="23d0c-351">Jednotlivé vlastnosti pomocí generování klíčů nezávislé celé číslo v paměti</span><span class="sxs-lookup"><span data-stu-id="23d0c-351">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="23d0c-352">Sledování problému #6872</span><span class="sxs-lookup"><span data-stu-id="23d0c-352">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="23d0c-353">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="23d0c-353">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="23d0c-354">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-354">**Old behavior**</span></span>

<span data-ttu-id="23d0c-355">Před EF Core 3.0 se použil jeden generátor sdíleného hodnot pro všechny vlastnosti klíče celé číslo v paměti.</span><span class="sxs-lookup"><span data-stu-id="23d0c-355">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="23d0c-356">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-356">**New behavior**</span></span>

<span data-ttu-id="23d0c-357">Od verze EF Core 3.0, každé celé číslo klíčová vlastnost získá vlastní generátor hodnot při použití databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="23d0c-357">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="23d0c-358">Také pokud se odstraní databáze, pak generování klíčů je obnovit pro všechny tabulky.</span><span class="sxs-lookup"><span data-stu-id="23d0c-358">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="23d0c-359">**Proč**</span><span class="sxs-lookup"><span data-stu-id="23d0c-359">**Why**</span></span>

<span data-ttu-id="23d0c-360">Tato změna byla provedena zarovnat generování klíče v paměti blíže k generování klíčů skutečná databáze a zlepšit schopnost izolace testů od sebe navzájem při použití databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="23d0c-360">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="23d0c-361">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="23d0c-361">**Mitigations**</span></span>

<span data-ttu-id="23d0c-362">Toto může rozbít aplikaci, která se spoléhá na konkrétní hodnoty klíče v paměti nastavit.</span><span class="sxs-lookup"><span data-stu-id="23d0c-362">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="23d0c-363">Zvažte místo toho spoléhat na konkrétní hodnoty klíče, ani aktualizaci tak, aby odpovídala nové chování.</span><span class="sxs-lookup"><span data-stu-id="23d0c-363">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="23d0c-364">Základní pole se používají ve výchozím nastavení</span><span class="sxs-lookup"><span data-stu-id="23d0c-364">Backing fields are used by default</span></span>

[<span data-ttu-id="23d0c-365">Sledování problému #12430</span><span class="sxs-lookup"><span data-stu-id="23d0c-365">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="23d0c-366">Tato změna je zavedená v EF Core 3.0 – náhled 2.</span><span class="sxs-lookup"><span data-stu-id="23d0c-366">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="23d0c-367">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-367">**Old behavior**</span></span>

<span data-ttu-id="23d0c-368">Než 3.0 i v případě, že pomocné pole vlastnosti zná, EF Core by stále ve výchozím nastavení čtení a zápisu hodnoty vlastnosti pomocí metody getter a setter vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="23d0c-368">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="23d0c-369">K tomuto výjimka provádění dotazů, kde pomocné pole se nastavuje přímo Pokud jsou známé.</span><span class="sxs-lookup"><span data-stu-id="23d0c-369">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="23d0c-370">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-370">**New behavior**</span></span>

<span data-ttu-id="23d0c-371">Počínaje EF Core 3.0, pokud jsou známé vlastnosti pole zálohování, potom EF Core budou vždy číst a Zapisovat vlastnosti pomocí pole zálohování.</span><span class="sxs-lookup"><span data-stu-id="23d0c-371">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="23d0c-372">Pokud aplikace se spoléhá na další chování zakódovaný do metody getter nebo setter to může způsobit přerušení aplikace.</span><span class="sxs-lookup"><span data-stu-id="23d0c-372">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="23d0c-373">**Proč**</span><span class="sxs-lookup"><span data-stu-id="23d0c-373">**Why**</span></span>

<span data-ttu-id="23d0c-374">Tato změna byla provedena zabránit EF Core chybně aktivací obchodní logiky ve výchozím nastavení při provádění databázových operací zahrnující entity.</span><span class="sxs-lookup"><span data-stu-id="23d0c-374">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="23d0c-375">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="23d0c-375">**Mitigations**</span></span>

<span data-ttu-id="23d0c-376">Pre-3.0 chování lze obnovit prostřednictvím konfigurace režim přístupu k vlastnosti na `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="23d0c-376">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="23d0c-377">Příklad:</span><span class="sxs-lookup"><span data-stu-id="23d0c-377">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="23d0c-378">Vyvolání výjimky, pokud je nalezeno více polí kompatibilní zálohování</span><span class="sxs-lookup"><span data-stu-id="23d0c-378">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="23d0c-379">Sledování problému #12523</span><span class="sxs-lookup"><span data-stu-id="23d0c-379">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="23d0c-380">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="23d0c-380">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="23d0c-381">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-381">**Old behavior**</span></span>

<span data-ttu-id="23d0c-382">Před EF Core 3.0 Pokud odpovídá více polí pravidel pro vyhledání pomocným polem vlastnosti, pak vybere jedno pole na základě některých pořadí priority.</span><span class="sxs-lookup"><span data-stu-id="23d0c-382">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="23d0c-383">To může způsobit nesprávné pole pro použití v případech, nejednoznačný.</span><span class="sxs-lookup"><span data-stu-id="23d0c-383">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="23d0c-384">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-384">**New behavior**</span></span>

<span data-ttu-id="23d0c-385">Od verze EF Core 3.0, pokud více polí budou odpovídat na stejnou vlastnost, pak je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="23d0c-385">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="23d0c-386">**Proč**</span><span class="sxs-lookup"><span data-stu-id="23d0c-386">**Why**</span></span>

<span data-ttu-id="23d0c-387">Tato změna byla provedena, abyste se vyhnuli použití tiše jedno pole před jiným při může obsahovat jen jedno správné.</span><span class="sxs-lookup"><span data-stu-id="23d0c-387">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="23d0c-388">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="23d0c-388">**Mitigations**</span></span>

<span data-ttu-id="23d0c-389">Vlastnosti s nejednoznačný základní pole musí mít pole pro použití explicitně zadán.</span><span class="sxs-lookup"><span data-stu-id="23d0c-389">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="23d0c-390">Například pomocí rozhraní fluent API:</span><span class="sxs-lookup"><span data-stu-id="23d0c-390">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="23d0c-391">Vlastnost jen pro pole názvů by měl odpovídat názvu pole</span><span class="sxs-lookup"><span data-stu-id="23d0c-391">Field-only property names should match the field name</span></span>

<span data-ttu-id="23d0c-392">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="23d0c-392">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="23d0c-393">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-393">**Old behavior**</span></span>

<span data-ttu-id="23d0c-394">Před EF Core 3.0 vlastnost může být určeno hodnotu řetězce a pokud nebyla nalezena žádná vlastnost s tímto názvem na typu CLR EF Core by opakujte tak, aby odpovídaly na pole pomocí pravidel pro vytváření názvů.</span><span class="sxs-lookup"><span data-stu-id="23d0c-394">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="23d0c-395">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-395">**New behavior**</span></span>

<span data-ttu-id="23d0c-396">Od verze EF Core 3.0, vlastnost jen pro pole musí odpovídat názvu pole přesně.</span><span class="sxs-lookup"><span data-stu-id="23d0c-396">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="23d0c-397">**Proč**</span><span class="sxs-lookup"><span data-stu-id="23d0c-397">**Why**</span></span>

<span data-ttu-id="23d0c-398">Tato změna byla provedena, abyste se vyhnuli použití stejné pole pro dvě vlastnosti s názvem podobně, také udržuje pravidla pro porovnávání vlastností pouze pole je stejný jako u vlastnosti, které jsou namapovány na vlastnosti CLR.</span><span class="sxs-lookup"><span data-stu-id="23d0c-398">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="23d0c-399">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="23d0c-399">**Mitigations**</span></span>

<span data-ttu-id="23d0c-400">Vlastnosti pouze pro pole musí mít název jako pole, které jsou mapovány na stejný.</span><span class="sxs-lookup"><span data-stu-id="23d0c-400">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="23d0c-401">V novější verzi preview EF Core 3.0 Plánujeme znovu povolit explicitně konfigurace název pole, které se liší od názvu vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="23d0c-401">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="23d0c-402">AddDbContext/AddDbContextPool zavolejte už AddLogging a AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="23d0c-402">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="23d0c-403">Sledování problému #14756</span><span class="sxs-lookup"><span data-stu-id="23d0c-403">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="23d0c-404">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="23d0c-404">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="23d0c-405">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-405">**Old behavior**</span></span>

<span data-ttu-id="23d0c-406">Před EF Core 3.0, volání `AddDbContext` nebo `AddDbContextPool` by také zaregistrovat protokolování a ukládání služeb s D.I prostřednictvím volání do mezipaměti [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) a [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="23d0c-406">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="23d0c-407">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-407">**New behavior**</span></span>

<span data-ttu-id="23d0c-408">Od verze EF Core 3.0, `AddDbContext` a `AddDbContextPool` nebude registru tyto služby s DI (Dependency Injection).</span><span class="sxs-lookup"><span data-stu-id="23d0c-408">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="23d0c-409">**Proč**</span><span class="sxs-lookup"><span data-stu-id="23d0c-409">**Why**</span></span>

<span data-ttu-id="23d0c-410">EF Core 3.0 nevyžaduje, že tyto služby jsou v kontejneru DI vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="23d0c-410">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="23d0c-411">Nicméně pokud `ILoggerFactory` je zaregistrovaný v aplikačním kontejneru DI, pak bude i nadále využívat EF Core.</span><span class="sxs-lookup"><span data-stu-id="23d0c-411">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="23d0c-412">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="23d0c-412">**Mitigations**</span></span>

<span data-ttu-id="23d0c-413">Pokud vaše aplikace potřebuje tyto služby, registrujte je explicitně s využitím kontejnerů DI [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) nebo [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="23d0c-413">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="23d0c-414">DbContext.Entry teď provádí místní metoda DetectChanges</span><span class="sxs-lookup"><span data-stu-id="23d0c-414">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="23d0c-415">Sledování problému #13552</span><span class="sxs-lookup"><span data-stu-id="23d0c-415">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="23d0c-416">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="23d0c-416">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="23d0c-417">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-417">**Old behavior**</span></span>

<span data-ttu-id="23d0c-418">Před EF Core 3.0, volání `DbContext.Entry` způsobí změny, aby se rozpoznal pro všechny sledované entity.</span><span class="sxs-lookup"><span data-stu-id="23d0c-418">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="23d0c-419">Tím zajistíte, že stav zpřístupněn v `EntityEntry` byla aktuální.</span><span class="sxs-lookup"><span data-stu-id="23d0c-419">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="23d0c-420">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-420">**New behavior**</span></span>

<span data-ttu-id="23d0c-421">Spouští se s EF Core 3.0, volání `DbContext.Entry` se teď jenom pokus o zjištění změn v dané entitě a všechny sledované instančního objektu entity, které s ním souvisejí.</span><span class="sxs-lookup"><span data-stu-id="23d0c-421">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="23d0c-422">To znamená, že se změní jinde nemusí byl zjištěn zavoláním této metody, které by mohly mít vliv na stav aplikace.</span><span class="sxs-lookup"><span data-stu-id="23d0c-422">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="23d0c-423">Všimněte si, že pokud `ChangeTracker.AutoDetectChangesEnabled` je nastavena na `false` pak i tato detekce místní změny se deaktivuje.</span><span class="sxs-lookup"><span data-stu-id="23d0c-423">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="23d0c-424">Jiné metody, které způsobují detekce změn – například `ChangeTracker.Entries` a `SaveChanges`– způsobí úplné `DetectChanges` všech sledovat entity.</span><span class="sxs-lookup"><span data-stu-id="23d0c-424">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="23d0c-425">**Proč**</span><span class="sxs-lookup"><span data-stu-id="23d0c-425">**Why**</span></span>

<span data-ttu-id="23d0c-426">Tato změna byla provedena pro zlepšení výkonu výchozí použití `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="23d0c-426">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="23d0c-427">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="23d0c-427">**Mitigations**</span></span>

<span data-ttu-id="23d0c-428">Volání `ChgangeTracker.DetectChanges()` explicitně před voláním `Entry` k zajištění chování pre-3.0.</span><span class="sxs-lookup"><span data-stu-id="23d0c-428">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="23d0c-429">Řetězec a bajtové pole klíčů se nerozlišují klientem generovaná ve výchozím nastavení</span><span class="sxs-lookup"><span data-stu-id="23d0c-429">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="23d0c-430">Sledování problému #14617</span><span class="sxs-lookup"><span data-stu-id="23d0c-430">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="23d0c-431">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="23d0c-431">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="23d0c-432">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-432">**Old behavior**</span></span>

<span data-ttu-id="23d0c-433">Před EF Core 3.0 `string` a `byte[]` klíčové vlastnosti použití explicitním nastavením nenulová hodnota.</span><span class="sxs-lookup"><span data-stu-id="23d0c-433">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="23d0c-434">V takovém případě by se vygenerovala hodnota klíče na klientovi jako identifikátor GUID serializovat do bajtů pro `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="23d0c-434">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="23d0c-435">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-435">**New behavior**</span></span>

<span data-ttu-id="23d0c-436">Od verze EF Core 3.0 výjimka bude vyvolána označující, že nebyla nastavena žádná hodnota klíče.</span><span class="sxs-lookup"><span data-stu-id="23d0c-436">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="23d0c-437">**Proč**</span><span class="sxs-lookup"><span data-stu-id="23d0c-437">**Why**</span></span>

<span data-ttu-id="23d0c-438">Tato změna byla provedena, protože klient generován `string` / `byte[]` hodnoty nejsou obecně užitečné a výchozí chování dostal pevné argumentovat o generované hodnoty klíče v běžným způsobem.</span><span class="sxs-lookup"><span data-stu-id="23d0c-438">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="23d0c-439">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="23d0c-439">**Mitigations**</span></span>

<span data-ttu-id="23d0c-440">Chování pre-3.0 je možné získat tak, že explicitně zadáte, by měl klíčové vlastnosti použít generované hodnoty, pokud je nastavena žádná hodnota jiná než null.</span><span class="sxs-lookup"><span data-stu-id="23d0c-440">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="23d0c-441">Například pomocí rozhraní fluent API:</span><span class="sxs-lookup"><span data-stu-id="23d0c-441">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="23d0c-442">Nebo s anotacemi dat:</span><span class="sxs-lookup"><span data-stu-id="23d0c-442">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="23d0c-443">Implementaci třídy ILoggerFactory je nyní vymezené služby</span><span class="sxs-lookup"><span data-stu-id="23d0c-443">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="23d0c-444">Sledování problému #14698</span><span class="sxs-lookup"><span data-stu-id="23d0c-444">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="23d0c-445">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="23d0c-445">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="23d0c-446">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-446">**Old behavior**</span></span>

<span data-ttu-id="23d0c-447">Před EF Core 3.0 `ILoggerFactory` bylo zaregistrováno jako služba typu singleton.</span><span class="sxs-lookup"><span data-stu-id="23d0c-447">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="23d0c-448">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-448">**New behavior**</span></span>

<span data-ttu-id="23d0c-449">Od verze EF Core 3.0, `ILoggerFactory` je teď zaregistrovaný jako obor.</span><span class="sxs-lookup"><span data-stu-id="23d0c-449">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="23d0c-450">**Proč**</span><span class="sxs-lookup"><span data-stu-id="23d0c-450">**Why**</span></span>

<span data-ttu-id="23d0c-451">Tato změna byla provedena umožňující přidružení protokolovací nástroj se `DbContext` instanci, která povoluje další funkce a odebírá někdy patologických chování, jako je například obrovské množství poskytovatelů vnitřní chybě služby.</span><span class="sxs-lookup"><span data-stu-id="23d0c-451">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="23d0c-452">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="23d0c-452">**Mitigations**</span></span>

<span data-ttu-id="23d0c-453">Tato změna by neměla mít vliv kód aplikace, pokud není registrace a použití vlastních služeb v poskytovateli EF Core vnitřní chybě služby.</span><span class="sxs-lookup"><span data-stu-id="23d0c-453">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="23d0c-454">Tato akce není běžné.</span><span class="sxs-lookup"><span data-stu-id="23d0c-454">This isn't common.</span></span>
<span data-ttu-id="23d0c-455">V těchto případech většinu toho, co bude dál fungovat, ale žádné služby typu singleton, která byla v závislosti na `ILoggerFactory` bude nutné změnit tak, aby získat `ILoggerFactory` jiným způsobem.</span><span class="sxs-lookup"><span data-stu-id="23d0c-455">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="23d0c-456">Pokud narazíte na situace tímto způsobem, založte prosím problém na na [sledování problémů Githubu EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) a dejte nám vědět, jak používáte `ILoggerFactory` tak, aby nám můžete lépe porozumět nechcete v budoucnu znovu rozdělit.</span><span class="sxs-lookup"><span data-stu-id="23d0c-456">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="idbcontextoptionsextensionwithdebuginfo-merged-into-idbcontextoptionsextension"></a><span data-ttu-id="23d0c-457">Sloučí IDbContextOptionsExtension IDbContextOptionsExtensionWithDebugInfo</span><span class="sxs-lookup"><span data-stu-id="23d0c-457">IDbContextOptionsExtensionWithDebugInfo merged into IDbContextOptionsExtension</span></span>

[<span data-ttu-id="23d0c-458">Sledování problému #13552</span><span class="sxs-lookup"><span data-stu-id="23d0c-458">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="23d0c-459">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="23d0c-459">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="23d0c-460">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-460">**Old behavior**</span></span>

<span data-ttu-id="23d0c-461">`IDbContextOptionsExtensionWithDebugInfo` Další volitelné rozhraní byla prodloužena z `IDbContextOptionsExtension` pro vyvarování rozbíjející změně rozhraní během cyklu vydání verze 2.x.</span><span class="sxs-lookup"><span data-stu-id="23d0c-461">`IDbContextOptionsExtensionWithDebugInfo` was an additional optional interface extended from `IDbContextOptionsExtension` to avoid making a breaking change to the interface during the 2.x release cycle.</span></span>

<span data-ttu-id="23d0c-462">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-462">**New behavior**</span></span>

<span data-ttu-id="23d0c-463">Rozhraní jsou nyní sloučeny do `IDbContextOptionsExtension`.</span><span class="sxs-lookup"><span data-stu-id="23d0c-463">The interfaces are now merged together into `IDbContextOptionsExtension`.</span></span>

<span data-ttu-id="23d0c-464">**Proč**</span><span class="sxs-lookup"><span data-stu-id="23d0c-464">**Why**</span></span>

<span data-ttu-id="23d0c-465">Tato změna byla provedena, protože rozhraní jsou koncepčně jednou.</span><span class="sxs-lookup"><span data-stu-id="23d0c-465">This change was made because the interfaces are conceptually one.</span></span>

<span data-ttu-id="23d0c-466">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="23d0c-466">**Mitigations**</span></span>

<span data-ttu-id="23d0c-467">Žádné implementace `IDbContextOptionsExtension` bude muset být aktualizované kvůli podpoře nového člena.</span><span class="sxs-lookup"><span data-stu-id="23d0c-467">Any implementations of `IDbContextOptionsExtension` will need to be updated to support the new member.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="23d0c-468">Opožděné načtení proxy už předpokládají, že jsou plně načteny navigační vlastnosti</span><span class="sxs-lookup"><span data-stu-id="23d0c-468">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="23d0c-469">Sledování problému #12780</span><span class="sxs-lookup"><span data-stu-id="23d0c-469">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="23d0c-470">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="23d0c-470">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="23d0c-471">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-471">**Old behavior**</span></span>

<span data-ttu-id="23d0c-472">Před EF Core 3.0, jednou `DbContext` byl odstraněn neexistoval způsob, jak zjistit, zda danou navigační vlastnost s entitou získané z daného kontextu byl plně načten či nikoli.</span><span class="sxs-lookup"><span data-stu-id="23d0c-472">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="23d0c-473">Proxy by místo toho se předpokládá, že je načtena navigační odkaz, pokud má nenulovou hodnotu a, že je načtena navigace kolekce, pokud není prázdná.</span><span class="sxs-lookup"><span data-stu-id="23d0c-473">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="23d0c-474">V těchto případech by pokus o opožděné načtení no-op.</span><span class="sxs-lookup"><span data-stu-id="23d0c-474">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="23d0c-475">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-475">**New behavior**</span></span>

<span data-ttu-id="23d0c-476">Od verze EF Core 3.0, proxy servery sledovat, jestli je načtena navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="23d0c-476">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="23d0c-477">To znamená, že při pokusu o přístup k navigační vlastnost, která je načtena po kontext se vyřadil. bude vždycky no-op, i v případě, že je načteno navigace je prázdný nebo mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="23d0c-477">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="23d0c-478">Naopak pokusu o přístup k navigační vlastnosti, které není načteno vyvolají výjimku pokud uvolnění kontextu, i když je navigační vlastnost kolekce není prázdná.</span><span class="sxs-lookup"><span data-stu-id="23d0c-478">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="23d0c-479">Pokud k této situaci dochází, znamená to kód aplikace se pokouší použít opožděné načtení na neplatný čas a aplikace by měla být změněna na tuto akci.</span><span class="sxs-lookup"><span data-stu-id="23d0c-479">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="23d0c-480">**Proč**</span><span class="sxs-lookup"><span data-stu-id="23d0c-480">**Why**</span></span>

<span data-ttu-id="23d0c-481">Tato změna byla provedena, aby chování konzistentní a správné při pokusu o opožděné načtení na vyřazený `DbContext` instance.</span><span class="sxs-lookup"><span data-stu-id="23d0c-481">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="23d0c-482">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="23d0c-482">**Mitigations**</span></span>

<span data-ttu-id="23d0c-483">Kód aplikace, který nebude pokoušet opožděné načtení vyřazený kontextu, nebo nakonfigurovat to být no-op. jak je popsáno v zpráva o výjimce.</span><span class="sxs-lookup"><span data-stu-id="23d0c-483">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="23d0c-484">Nadměrné vytváření zprostředkovatelů vnitřní chybě služby je teď k chybě ve výchozím nastavení</span><span class="sxs-lookup"><span data-stu-id="23d0c-484">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="23d0c-485">Sledování problému #10236</span><span class="sxs-lookup"><span data-stu-id="23d0c-485">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="23d0c-486">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="23d0c-486">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="23d0c-487">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-487">**Old behavior**</span></span>

<span data-ttu-id="23d0c-488">Upozornění by před EF Core 3.0 zaznamenávané aplikace vytváří patologických několik poskytovatelů vnitřní chybě služby.</span><span class="sxs-lookup"><span data-stu-id="23d0c-488">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="23d0c-489">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-489">**New behavior**</span></span>

<span data-ttu-id="23d0c-490">Od verze EF Core 3.0, toto upozornění se teď považuje za a chyby a výjimky je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="23d0c-490">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="23d0c-491">**Proč**</span><span class="sxs-lookup"><span data-stu-id="23d0c-491">**Why**</span></span>

<span data-ttu-id="23d0c-492">Tato změna byla provedena na připravovat lepší kód aplikace prostřednictvím vystavení takovém patologických více explicitně.</span><span class="sxs-lookup"><span data-stu-id="23d0c-492">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="23d0c-493">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="23d0c-493">**Mitigations**</span></span>

<span data-ttu-id="23d0c-494">Nejvhodnější příčinu akce při výskytu této chyby je zjistěte původní příčinu a nevytvářet mnoho poskytovatelů vnitřní chybě služby.</span><span class="sxs-lookup"><span data-stu-id="23d0c-494">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="23d0c-495">Však chyba může být převést zpět na upozornění (nebo ignorovat) prostřednictvím konfigurace na `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="23d0c-495">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="23d0c-496">Příklad:</span><span class="sxs-lookup"><span data-stu-id="23d0c-496">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="23d0c-497">Nové chování pro HasOne/HasMany volat jeden řetězec</span><span class="sxs-lookup"><span data-stu-id="23d0c-497">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="23d0c-498">Sledování problému #9171</span><span class="sxs-lookup"><span data-stu-id="23d0c-498">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="23d0c-499">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="23d0c-499">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="23d0c-500">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-500">**Old behavior**</span></span>

<span data-ttu-id="23d0c-501">Před EF Core 3.0 kód volání `HasOne` nebo `HasMany` s jeden řetězec byl interpretován tak matoucí.</span><span class="sxs-lookup"><span data-stu-id="23d0c-501">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="23d0c-502">Příklad:</span><span class="sxs-lookup"><span data-stu-id="23d0c-502">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="23d0c-503">Kód vypadá je týkající se `Samurai` některé jiné entity typu použití `Entrance` navigační vlastnost, která může být privátní.</span><span class="sxs-lookup"><span data-stu-id="23d0c-503">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="23d0c-504">Ve skutečnosti, tento kód se pokouší vytvořit relaci některé typ entity s názvem `Entrance` se žádné navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="23d0c-504">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="23d0c-505">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-505">**New behavior**</span></span>

<span data-ttu-id="23d0c-506">Od verze EF Core 3.0, výše uvedený kód nyní dělá co podívali jako její by měl mít byla činnosti před.</span><span class="sxs-lookup"><span data-stu-id="23d0c-506">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="23d0c-507">**Proč**</span><span class="sxs-lookup"><span data-stu-id="23d0c-507">**Why**</span></span>

<span data-ttu-id="23d0c-508">Staré chování bylo velmi matoucí, zejména při čtení konfigurace kód a hledání chyb.</span><span class="sxs-lookup"><span data-stu-id="23d0c-508">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="23d0c-509">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="23d0c-509">**Mitigations**</span></span>

<span data-ttu-id="23d0c-510">Tímto přerušíte pouze aplikace, které jsou explicitně konfigurace relace používání řetězců pro názvy typů a bez explicitním zadáním navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="23d0c-510">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="23d0c-511">Není běžné.</span><span class="sxs-lookup"><span data-stu-id="23d0c-511">This is not common.</span></span>
<span data-ttu-id="23d0c-512">Předchozí chování můžete získat prostřednictvím explicitně předávání `null` pro název navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="23d0c-512">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="23d0c-513">Příklad:</span><span class="sxs-lookup"><span data-stu-id="23d0c-513">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="23d0c-514">Návratový typ pro několik asynchronních metod se změnil z úlohy na ValueTask</span><span class="sxs-lookup"><span data-stu-id="23d0c-514">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="23d0c-515">Sledování problému #15184</span><span class="sxs-lookup"><span data-stu-id="23d0c-515">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="23d0c-516">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="23d0c-516">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="23d0c-517">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-517">**Old behavior**</span></span>

<span data-ttu-id="23d0c-518">Dříve vrátila následující asynchronní metody `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="23d0c-518">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="23d0c-519">`ValueGenerator.NextValueAsync()` (a odvozených tříd)</span><span class="sxs-lookup"><span data-stu-id="23d0c-519">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="23d0c-520">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-520">**New behavior**</span></span>

<span data-ttu-id="23d0c-521">Výše uvedené metody nyní vrací `ValueTask<T>` přes stejné `T` stejně jako předtím.</span><span class="sxs-lookup"><span data-stu-id="23d0c-521">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="23d0c-522">**Proč**</span><span class="sxs-lookup"><span data-stu-id="23d0c-522">**Why**</span></span>

<span data-ttu-id="23d0c-523">Tato změna snižuje počet přidělení haldy vzniklé při volání těchto metod, zlepšení obecné informace o výkonu.</span><span class="sxs-lookup"><span data-stu-id="23d0c-523">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="23d0c-524">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="23d0c-524">**Mitigations**</span></span>

<span data-ttu-id="23d0c-525">Aplikace jednoduše čeká na výše uvedené rozhraní API pouze muset být překompilovány - žádné změny zdroje jsou nezbytné.</span><span class="sxs-lookup"><span data-stu-id="23d0c-525">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="23d0c-526">Složitější využití (například předání vráceného `Task` k `Task.WhenAny()`) obvykle vyžadují, aby vráceného `ValueTask<T>` převedou na hodnoty `Task<T>` voláním `AsTask()` na něm.</span><span class="sxs-lookup"><span data-stu-id="23d0c-526">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="23d0c-527">Všimněte si, že to Neguje snížení přidělení, které tato změna přináší.</span><span class="sxs-lookup"><span data-stu-id="23d0c-527">Note that this negates the allocation reduction that this change brings.</span></span>

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="23d0c-528">Poznámka relační: TypeMapping je teď stejně TypeMapping</span><span class="sxs-lookup"><span data-stu-id="23d0c-528">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="23d0c-529">Sledování problému #9913</span><span class="sxs-lookup"><span data-stu-id="23d0c-529">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="23d0c-530">Tato změna je zavedená v EF Core 3.0 – náhled 2.</span><span class="sxs-lookup"><span data-stu-id="23d0c-530">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="23d0c-531">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-531">**Old behavior**</span></span>

<span data-ttu-id="23d0c-532">Název poznámky pro mapování anotace typu byl "Relační: TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="23d0c-532">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="23d0c-533">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-533">**New behavior**</span></span>

<span data-ttu-id="23d0c-534">Název poznámky pro mapování anotace typu je nyní "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="23d0c-534">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="23d0c-535">**Proč**</span><span class="sxs-lookup"><span data-stu-id="23d0c-535">**Why**</span></span>

<span data-ttu-id="23d0c-536">Mapování typu jsou teď používá pro více než jen relační databáze poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="23d0c-536">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="23d0c-537">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="23d0c-537">**Mitigations**</span></span>

<span data-ttu-id="23d0c-538">Tímto přerušíte jenom aplikace s přístupem k mapování typů přímo jako poznámka, která není běžné.</span><span class="sxs-lookup"><span data-stu-id="23d0c-538">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="23d0c-539">Nejvhodnější opatření na opravu se má používat rovinu rozhraní API pro mapování typů přístupu spíše než přímo pomocí anotace.</span><span class="sxs-lookup"><span data-stu-id="23d0c-539">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="23d0c-540">Vyvolá výjimku, ToTable na odvozený typ.</span><span class="sxs-lookup"><span data-stu-id="23d0c-540">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="23d0c-541">Sledování problému #11811</span><span class="sxs-lookup"><span data-stu-id="23d0c-541">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="23d0c-542">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="23d0c-542">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="23d0c-543">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-543">**Old behavior**</span></span>

<span data-ttu-id="23d0c-544">Před EF Core 3.0 `ToTable()` volalo odvozený typ bude ignorovat, protože pouze dědičnosti mapování strategie byl TPH, kdy to není platný.</span><span class="sxs-lookup"><span data-stu-id="23d0c-544">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="23d0c-545">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-545">**New behavior**</span></span>

<span data-ttu-id="23d0c-546">Spouští se s EF Core 3.0 a při přípravě na přidání TPT a TPC podporují v pozdější verzi `ToTable()` volalo odvozený typ bude nyní vyvolání výjimky v budoucnu vyhnuli o změnu neočekávané mapování.</span><span class="sxs-lookup"><span data-stu-id="23d0c-546">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="23d0c-547">**Proč**</span><span class="sxs-lookup"><span data-stu-id="23d0c-547">**Why**</span></span>

<span data-ttu-id="23d0c-548">Aktuálně není platná pro mapování odvozeného typu na jiné tabulky.</span><span class="sxs-lookup"><span data-stu-id="23d0c-548">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="23d0c-549">Tato změna se vyhnete přerušení v budoucnu, kdy bude platná věc udělat.</span><span class="sxs-lookup"><span data-stu-id="23d0c-549">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="23d0c-550">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="23d0c-550">**Mitigations**</span></span>

<span data-ttu-id="23d0c-551">Odeberte všechny pokusy o mapování odvozené typy s jinými tabulkami.</span><span class="sxs-lookup"><span data-stu-id="23d0c-551">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="23d0c-552">Nahradí HasIndex ForSqlServerHasIndex</span><span class="sxs-lookup"><span data-stu-id="23d0c-552">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="23d0c-553">Sledování problému #12366</span><span class="sxs-lookup"><span data-stu-id="23d0c-553">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="23d0c-554">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="23d0c-554">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="23d0c-555">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-555">**Old behavior**</span></span>

<span data-ttu-id="23d0c-556">Před EF Core 3.0 `ForSqlServerHasIndex().ForSqlServerInclude()` poskytuje způsob, jak konfigurovat sloupců použitých s `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="23d0c-556">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="23d0c-557">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-557">**New behavior**</span></span>

<span data-ttu-id="23d0c-558">Od verze EF Core 3.0, pomocí `Include` na indexu se teď podporuje relační úrovni.</span><span class="sxs-lookup"><span data-stu-id="23d0c-558">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="23d0c-559">Použití `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="23d0c-559">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="23d0c-560">**Proč**</span><span class="sxs-lookup"><span data-stu-id="23d0c-560">**Why**</span></span>

<span data-ttu-id="23d0c-561">Tato změna byla provedena konsolidovat rozhraní API pro indexy s `Include` do jednoho umístění pro všechny databáze poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="23d0c-561">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="23d0c-562">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="23d0c-562">**Mitigations**</span></span>

<span data-ttu-id="23d0c-563">Pomocí nového rozhraní API, jak je znázorněno výše.</span><span class="sxs-lookup"><span data-stu-id="23d0c-563">Use the new API, as shown above.</span></span>

## <a name="metadata-api-changes"></a><span data-ttu-id="23d0c-564">Změn metadat rozhraní API</span><span class="sxs-lookup"><span data-stu-id="23d0c-564">Metadata API changes</span></span>

[<span data-ttu-id="23d0c-565">Sledování problému #214</span><span class="sxs-lookup"><span data-stu-id="23d0c-565">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="23d0c-566">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="23d0c-566">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="23d0c-567">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-567">**New behavior**</span></span>

<span data-ttu-id="23d0c-568">Následující vlastnosti byly převedeny na rozšiřující metody:</span><span class="sxs-lookup"><span data-stu-id="23d0c-568">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="23d0c-569">**Proč**</span><span class="sxs-lookup"><span data-stu-id="23d0c-569">**Why**</span></span>

<span data-ttu-id="23d0c-570">Tato změna usnadňuje provádění výše uvedených rozhraní.</span><span class="sxs-lookup"><span data-stu-id="23d0c-570">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="23d0c-571">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="23d0c-571">**Mitigations**</span></span>

<span data-ttu-id="23d0c-572">Pomocí nové metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="23d0c-572">Use the new extension methods.</span></span>

## <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="23d0c-573">Změny specifickým pro zprostředkovatele metadat rozhraní API</span><span class="sxs-lookup"><span data-stu-id="23d0c-573">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="23d0c-574">Sledování problému #214</span><span class="sxs-lookup"><span data-stu-id="23d0c-574">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="23d0c-575">Tato změna je zavedená v EF Core 3.0-preview 6.</span><span class="sxs-lookup"><span data-stu-id="23d0c-575">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="23d0c-576">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-576">**New behavior**</span></span>

<span data-ttu-id="23d0c-577">Metody rozšíření specifické pro zprostředkovatele bude zjednodušen navýšení kapacity:</span><span class="sxs-lookup"><span data-stu-id="23d0c-577">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.GetSqlServerIsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.ForSqlServerUseIdentityColumn()`

<span data-ttu-id="23d0c-578">**Proč**</span><span class="sxs-lookup"><span data-stu-id="23d0c-578">**Why**</span></span>

<span data-ttu-id="23d0c-579">Tato změna usnadňuje provádění metody výše uvedená rozšíření.</span><span class="sxs-lookup"><span data-stu-id="23d0c-579">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="23d0c-580">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="23d0c-580">**Mitigations**</span></span>

<span data-ttu-id="23d0c-581">Pomocí nové metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="23d0c-581">Use the new extension methods.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="23d0c-582">EF Core už odešle – Direktiva pragma pro vynucení SQLite FK</span><span class="sxs-lookup"><span data-stu-id="23d0c-582">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="23d0c-583">Sledování problému #12151</span><span class="sxs-lookup"><span data-stu-id="23d0c-583">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="23d0c-584">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="23d0c-584">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="23d0c-585">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-585">**Old behavior**</span></span>

<span data-ttu-id="23d0c-586">Před EF Core 3.0, bude posílat EF Core `PRAGMA foreign_keys = 1` při otevření připojení k SQLite.</span><span class="sxs-lookup"><span data-stu-id="23d0c-586">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="23d0c-587">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-587">**New behavior**</span></span>

<span data-ttu-id="23d0c-588">Od verze EF Core 3.0, už EF Core odešle `PRAGMA foreign_keys = 1` při otevření připojení k SQLite.</span><span class="sxs-lookup"><span data-stu-id="23d0c-588">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="23d0c-589">**Proč**</span><span class="sxs-lookup"><span data-stu-id="23d0c-589">**Why**</span></span>

<span data-ttu-id="23d0c-590">Tato změna byla provedena, protože používá EF Core `SQLitePCLRaw.bundle_e_sqlite3` ve výchozím nastavení, která zase znamená, že vynucení cizího klíče je ve výchozím nastavení zapnuté a není potřeba explicitně povolit pokaždé, když je otevřeno připojení.</span><span class="sxs-lookup"><span data-stu-id="23d0c-590">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="23d0c-591">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="23d0c-591">**Mitigations**</span></span>

<span data-ttu-id="23d0c-592">Cizí klíče jsou povolena ve výchozím nastavení SQLitePCLRaw.bundle_e_sqlite3, který se používá ve výchozím nastavení pro jádro EF Core.</span><span class="sxs-lookup"><span data-stu-id="23d0c-592">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="23d0c-593">Pro jiných případech může být povoleno cizích klíčů tak, že zadáte `Foreign Keys=True` v připojovacím řetězci.</span><span class="sxs-lookup"><span data-stu-id="23d0c-593">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="23d0c-594">Microsoft.EntityFrameworkCore.Sqlite nyní závisí na SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="23d0c-594">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="23d0c-595">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-595">**Old behavior**</span></span>

<span data-ttu-id="23d0c-596">Před EF Core 3.0 používá EF Core `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="23d0c-596">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="23d0c-597">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-597">**New behavior**</span></span>

<span data-ttu-id="23d0c-598">Od verze EF Core 3.0, pomocí EF Core `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="23d0c-598">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="23d0c-599">**Proč**</span><span class="sxs-lookup"><span data-stu-id="23d0c-599">**Why**</span></span>

<span data-ttu-id="23d0c-600">Tato změna byla provedena tak, aby používalo verzi SQLite v Iosu konzistentní s jinými platformami.</span><span class="sxs-lookup"><span data-stu-id="23d0c-600">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="23d0c-601">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="23d0c-601">**Mitigations**</span></span>

<span data-ttu-id="23d0c-602">Pokud chcete použít nativní verzi SQLite v Iosu, nakonfigurovat `Microsoft.Data.Sqlite` použít jinou `SQLitePCLRaw` sady.</span><span class="sxs-lookup"><span data-stu-id="23d0c-602">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

## <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="23d0c-603">Identifikátor GUID hodnoty jsou nyní uloženy jako TEXT na SQLite</span><span class="sxs-lookup"><span data-stu-id="23d0c-603">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="23d0c-604">Sledování problému #15078</span><span class="sxs-lookup"><span data-stu-id="23d0c-604">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="23d0c-605">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="23d0c-605">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="23d0c-606">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-606">**Old behavior**</span></span>

<span data-ttu-id="23d0c-607">Identifikátor GUID hodnoty byly dříve sored jako hodnoty objektu BLOB na SQLite.</span><span class="sxs-lookup"><span data-stu-id="23d0c-607">Guid values were previously sored as BLOB values on SQLite.</span></span>

<span data-ttu-id="23d0c-608">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-608">**New behavior**</span></span>

<span data-ttu-id="23d0c-609">Identifikátor GUID hodnoty jsou nyní uloženy jako TEXT.</span><span class="sxs-lookup"><span data-stu-id="23d0c-609">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="23d0c-610">**Proč**</span><span class="sxs-lookup"><span data-stu-id="23d0c-610">**Why**</span></span>

<span data-ttu-id="23d0c-611">Binární formát GUID není standardizované.</span><span class="sxs-lookup"><span data-stu-id="23d0c-611">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="23d0c-612">Uložení hodnot jako TEXT díky databáze více kompatibilní s jinými technologiemi.</span><span class="sxs-lookup"><span data-stu-id="23d0c-612">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="23d0c-613">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="23d0c-613">**Mitigations**</span></span>

<span data-ttu-id="23d0c-614">Spuštěním SQL takto můžete migrovat existující databáze na nový formát.</span><span class="sxs-lookup"><span data-stu-id="23d0c-614">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="23d0c-615">V EF Core můžete také pokračovat pomocí předchozí chování nakonfigurováním převaděč hodnoty na tyto vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="23d0c-615">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="23d0c-616">Microsoft.Data.Sqlite zůstává schopný načíst hodnoty identifikátoru Guid z objektu BLOB a TEXTOVÉHO sloupce; ale vzhledem k tomu, že došlo ke změně výchozího formátu pro parametry a konstant bude pravděpodobně potřeba provést akci pro většinu scénářů zahrnující identifikátory GUID.</span><span class="sxs-lookup"><span data-stu-id="23d0c-616">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="23d0c-617">Hodnoty char jsou nyní uloženy jako TEXT na SQLite</span><span class="sxs-lookup"><span data-stu-id="23d0c-617">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="23d0c-618">Sledování problému #15020</span><span class="sxs-lookup"><span data-stu-id="23d0c-618">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="23d0c-619">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="23d0c-619">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="23d0c-620">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-620">**Old behavior**</span></span>

<span data-ttu-id="23d0c-621">Hodnoty char byly dříve sored jako CELOČÍSELNÉ hodnoty na SQLite.</span><span class="sxs-lookup"><span data-stu-id="23d0c-621">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="23d0c-622">Například znak hodnotu *A* byl uložen jako celočíselnou hodnotu 65.</span><span class="sxs-lookup"><span data-stu-id="23d0c-622">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="23d0c-623">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-623">**New behavior**</span></span>

<span data-ttu-id="23d0c-624">Hodnoty char jsou nyní uloženy jako TEXT.</span><span class="sxs-lookup"><span data-stu-id="23d0c-624">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="23d0c-625">**Proč**</span><span class="sxs-lookup"><span data-stu-id="23d0c-625">**Why**</span></span>

<span data-ttu-id="23d0c-626">Uložení hodnot jako TEXT je přirozenější a vytvoří databáze více kompatibilní s jinými technologiemi.</span><span class="sxs-lookup"><span data-stu-id="23d0c-626">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="23d0c-627">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="23d0c-627">**Mitigations**</span></span>

<span data-ttu-id="23d0c-628">Spuštěním SQL takto můžete migrovat existující databáze na nový formát.</span><span class="sxs-lookup"><span data-stu-id="23d0c-628">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="23d0c-629">V EF Core můžete také pokračovat pomocí předchozí chování nakonfigurováním převaděč hodnoty na tyto vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="23d0c-629">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="23d0c-630">Microsoft.Data.Sqlite také zbývá schopný načíst znakových hodnot z celé číslo a TEXT sloupců, takže určitých scénářích nevyžadují žádnou akci.</span><span class="sxs-lookup"><span data-stu-id="23d0c-630">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="23d0c-631">ID migrace jsou generovány pomocí neutrální jazykové verze kalendáře</span><span class="sxs-lookup"><span data-stu-id="23d0c-631">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="23d0c-632">Sledování problému #12978</span><span class="sxs-lookup"><span data-stu-id="23d0c-632">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="23d0c-633">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="23d0c-633">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="23d0c-634">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-634">**Old behavior**</span></span>

<span data-ttu-id="23d0c-635">ID migrace neúmyslně byly vygenerovány pomocí kalendář aktuální jazykové verze.</span><span class="sxs-lookup"><span data-stu-id="23d0c-635">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="23d0c-636">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-636">**New behavior**</span></span>

<span data-ttu-id="23d0c-637">ID migrace jsou teď vždy generovány pomocí neutrální jazykové verze kalendáře (gregoriánského).</span><span class="sxs-lookup"><span data-stu-id="23d0c-637">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="23d0c-638">**Proč**</span><span class="sxs-lookup"><span data-stu-id="23d0c-638">**Why**</span></span>

<span data-ttu-id="23d0c-639">Pořadí migrace je důležité při aktualizaci databáze nebo řešení konfliktů při sloučení.</span><span class="sxs-lookup"><span data-stu-id="23d0c-639">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="23d0c-640">Pomocí neutrální kalendáře se vyhnete řazení problémy, které můžou být výsledkem členy týmu s jiným kalendáře.</span><span class="sxs-lookup"><span data-stu-id="23d0c-640">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="23d0c-641">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="23d0c-641">**Mitigations**</span></span>

<span data-ttu-id="23d0c-642">Tato změna ovlivní těm, kdo používají jiných gregoriánském kalendáři, kde rok je větší než gregoriánském kalendáři (např. thajský buddhistický kalendář).</span><span class="sxs-lookup"><span data-stu-id="23d0c-642">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="23d0c-643">Migrace stávající ID bude potřeba aktualizovat tak, aby nové migrace jsou řazeny za stávající migrace.</span><span class="sxs-lookup"><span data-stu-id="23d0c-643">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="23d0c-644">ID migrace najdete v atributu migrace soubory návrháře migrace.</span><span class="sxs-lookup"><span data-stu-id="23d0c-644">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="23d0c-645">Tabulky historie migrace je také potřeba aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="23d0c-645">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

## <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="23d0c-646">LogQueryPossibleExceptionWithAggregateOperator byl přejmenován.</span><span class="sxs-lookup"><span data-stu-id="23d0c-646">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="23d0c-647">Sledování problému #10985</span><span class="sxs-lookup"><span data-stu-id="23d0c-647">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="23d0c-648">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="23d0c-648">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="23d0c-649">**Změna**</span><span class="sxs-lookup"><span data-stu-id="23d0c-649">**Change**</span></span>

<span data-ttu-id="23d0c-650">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` byl přejmenován na `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="23d0c-650">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="23d0c-651">**Proč**</span><span class="sxs-lookup"><span data-stu-id="23d0c-651">**Why**</span></span>

<span data-ttu-id="23d0c-652">Zarovná pojmenování Tato událost upozornění s jinými událostmi upozornění.</span><span class="sxs-lookup"><span data-stu-id="23d0c-652">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="23d0c-653">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="23d0c-653">**Mitigations**</span></span>

<span data-ttu-id="23d0c-654">Použití nového názvu.</span><span class="sxs-lookup"><span data-stu-id="23d0c-654">Use the new name.</span></span> <span data-ttu-id="23d0c-655">(Všimněte si, že nedošlo ke změně číslo ID události.)</span><span class="sxs-lookup"><span data-stu-id="23d0c-655">(Note that the event ID number has not changed.)</span></span>

## <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="23d0c-656">Vysvětlení rozhraní API pro názvy omezení pro cizí klíč</span><span class="sxs-lookup"><span data-stu-id="23d0c-656">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="23d0c-657">Sledování problému #10730</span><span class="sxs-lookup"><span data-stu-id="23d0c-657">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="23d0c-658">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="23d0c-658">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="23d0c-659">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-659">**Old behavior**</span></span>

<span data-ttu-id="23d0c-660">Před EF Core 3.0 omezení pro cizí klíč názvy označovaly jako jednoduše "name".</span><span class="sxs-lookup"><span data-stu-id="23d0c-660">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="23d0c-661">Příklad:</span><span class="sxs-lookup"><span data-stu-id="23d0c-661">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="23d0c-662">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="23d0c-662">**New behavior**</span></span>

<span data-ttu-id="23d0c-663">Od verze EF Core 3.0, omezení pro cizí klíč názvy jsou dnes označovány jako "název omezení".</span><span class="sxs-lookup"><span data-stu-id="23d0c-663">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="23d0c-664">Příklad:</span><span class="sxs-lookup"><span data-stu-id="23d0c-664">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="23d0c-665">**Proč**</span><span class="sxs-lookup"><span data-stu-id="23d0c-665">**Why**</span></span>

<span data-ttu-id="23d0c-666">Tato změna přináší konzistenci pro názvy v této oblasti a také vysvětluje, že se jedná o název název cizího klíče omezení a nikoli na sloupec nebo vlastnost, která je definována cizího klíče na.</span><span class="sxs-lookup"><span data-stu-id="23d0c-666">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="23d0c-667">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="23d0c-667">**Mitigations**</span></span>

<span data-ttu-id="23d0c-668">Použití nového názvu.</span><span class="sxs-lookup"><span data-stu-id="23d0c-668">Use the new name.</span></span>
