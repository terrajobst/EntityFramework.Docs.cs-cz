---
title: Zásadní změny v EF Core 3.0 – EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 96586808862c4373168dcd34a5f00c9f2f7563c3
ms.sourcegitcommit: 9bd64a1a71b7f7aeb044aeecc7c4785b57db1ec9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/26/2019
ms.locfileid: "67394832"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="45e8c-102">Rozbíjející změny zahrnuté v EF Core 3.0 (aktuálně ve verzi preview)</span><span class="sxs-lookup"><span data-stu-id="45e8c-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="45e8c-103">Mějte prosím na paměti, že sady funkcí a plány budoucích verzí se vždy mohou změnit a přestože se snažíme se zachovat aktuální, nemusí odrážet naše nejnovější plány vůbec na této stránce vyprší.</span><span class="sxs-lookup"><span data-stu-id="45e8c-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="45e8c-104">Následující změny chování a rozhraní API by mohly přerušit aplikace vyvinuté pro jádro EF Core 2.2.x při upgradu na 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="45e8c-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="45e8c-105">Změny, které chceme ovlivní jen poskytovatelé databází jsou popsány v části [poskytovatel změny](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="45e8c-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="45e8c-106">Tady nejsou uvedené konce v nové vlastnosti představené z jednoho 3.0 ve verzi preview další 3.0 ve verzi Preview.</span><span class="sxs-lookup"><span data-stu-id="45e8c-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="45e8c-107">Dotazy LINQ se už nevyhodnocuje na straně klienta</span><span class="sxs-lookup"><span data-stu-id="45e8c-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="45e8c-108">[Sledování problému #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[také zjistit problém #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="45e8c-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="45e8c-109">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="45e8c-109">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="45e8c-110">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-110">**Old behavior**</span></span>

<span data-ttu-id="45e8c-111">Než 3.0 když EF Core nelze převést výraz, který byl součástí sady dotazů SQL nebo parametr, automaticky vyhodnotí výraz na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="45e8c-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="45e8c-112">Ve výchozím nastavení hodnocení klientů potencionálně nákladným výrazů aktivuje pouze upozornění.</span><span class="sxs-lookup"><span data-stu-id="45e8c-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="45e8c-113">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-113">**New behavior**</span></span>

<span data-ttu-id="45e8c-114">Od verze 3.0, EF Core umožňuje pouze výrazy v projekci nejvyšší úrovně (poslední `Select()` volání v dotazu) k vyhodnocení na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="45e8c-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="45e8c-115">Když výrazy v další části dotazu nelze převést na SQL nebo parametr, je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="45e8c-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="45e8c-116">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-116">**Why**</span></span>

<span data-ttu-id="45e8c-117">Vyhodnocení automatickou klientskou dotazů umožňuje mnoho dotazů, který se spustí i v případě, že je důležité části nelze přeložit.</span><span class="sxs-lookup"><span data-stu-id="45e8c-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="45e8c-118">Toto chování může způsobit neočekávané a potenciálně škodlivé chování, které může přestat pouze zřejmé v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="45e8c-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="45e8c-119">Například podmínku v `Where()` volání, které nelze přeložit může způsobit, že všechny řádky z tabulky přesunou z databázového serveru a filtr, který bude použit na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="45e8c-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="45e8c-120">Tato situace může pokračovat nezjištěné po snadno Pokud tabulka obsahuje jenom pár řádků v vývoje, ale přístupů pevné přesun aplikace do produkčního prostředí, kde tabulka může obsahovat miliony řádků.</span><span class="sxs-lookup"><span data-stu-id="45e8c-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="45e8c-121">Upozornění vyhodnocení klienta dokázaly také příliš jednoduché ignorovat během vývoje.</span><span class="sxs-lookup"><span data-stu-id="45e8c-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="45e8c-122">Kromě toho může automatickou klientskou vyhodnocení způsobit problémy, ve kterých vylepšení překladu dotazu pro konkrétní výrazy způsobila neúmyslnému zásadní změny mezi verzí.</span><span class="sxs-lookup"><span data-stu-id="45e8c-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="45e8c-123">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-123">**Mitigations**</span></span>

<span data-ttu-id="45e8c-124">Pokud dotaz nelze přeložit plně, pak buď přepište dotaz ve formuláři, který lze přeložit, nebo použijte `AsEnumerable()`, `ToList()`, nebo podobný explicitně přenést data zpět do klienta ve kterém pak může být dále zpracovány pomocí LINQ na objekty.</span><span class="sxs-lookup"><span data-stu-id="45e8c-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="45e8c-125">Entity Framework Core už nejsou součástí sdíleného rozhraní ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="45e8c-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="45e8c-126">Oznámení týkající se sledování problému #325</span><span class="sxs-lookup"><span data-stu-id="45e8c-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="45e8c-127">Tato změna se používá v ASP.NET Core 3.0 ve verzi preview 1.</span><span class="sxs-lookup"><span data-stu-id="45e8c-127">This change is introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="45e8c-128">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-128">**Old behavior**</span></span>

<span data-ttu-id="45e8c-129">Před ASP.NET Core 3.0, když se přidá odkaz na balíček pro `Microsoft.AspNetCore.App` nebo `Microsoft.AspNetCore.All`, měl by obsahovat EF Core a některé EF Core zprostředkovatele dat, jako jsou zprostředkovatele SQL Server.</span><span class="sxs-lookup"><span data-stu-id="45e8c-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="45e8c-130">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-130">**New behavior**</span></span>

<span data-ttu-id="45e8c-131">Od verze 3.0, rozhraní ASP.NET Core sdílené neobsahuje EF Core nebo žádným zprostředkovatelům dat. EF Core.</span><span class="sxs-lookup"><span data-stu-id="45e8c-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="45e8c-132">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-132">**Why**</span></span>

<span data-ttu-id="45e8c-133">Před touto změnou získávání EF Core vyžaduje jiný postup v závislosti na tom, jestli aplikace cílené ASP.NET Core a SQL Server, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="45e8c-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="45e8c-134">Upgrade ASP.NET Core také vynutit upgrade EF Core a zprostředkovatele SQL Server, který nemusí být vždy žádoucí.</span><span class="sxs-lookup"><span data-stu-id="45e8c-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="45e8c-135">Díky této změně se možnosti načítání EF Core je stejná ve všech zprostředkovatelů, podporovaná implementace .NET a typy aplikací.</span><span class="sxs-lookup"><span data-stu-id="45e8c-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="45e8c-136">Vývojáři také nyní mohou ovládat přesně při upgradu EF Core a EF Core zprostředkovatelé dat.</span><span class="sxs-lookup"><span data-stu-id="45e8c-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="45e8c-137">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-137">**Mitigations**</span></span>

<span data-ttu-id="45e8c-138">V aplikaci ASP.NET Core 3.0 nebo jakékoli jiné podporované aplikace použít EF Core, explicitně přidáte odkaz na balíček k poskytovateli databáze EF Core, který bude aplikace používat.</span><span class="sxs-lookup"><span data-stu-id="45e8c-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="45e8c-139">EF Core nástroji příkazového řádku dotnet ef, už nejsou součástí sady .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="45e8c-139">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="45e8c-140">Sledování problému #14016</span><span class="sxs-lookup"><span data-stu-id="45e8c-140">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="45e8c-141">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4 a odpovídající verze sady .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="45e8c-141">This change is introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="45e8c-142">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-142">**Old behavior**</span></span>

<span data-ttu-id="45e8c-143">Než 3.0 `dotnet ef` nástroj je zahrnutý v .NET Core SDK a se snadno k dispozici pro použití z příkazového řádku z libovolného projektu bez nutnosti pár kroků navíc.</span><span class="sxs-lookup"><span data-stu-id="45e8c-143">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="45e8c-144">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-144">**New behavior**</span></span>

<span data-ttu-id="45e8c-145">Od verze 3.0, sady .NET SDK se nenachází `dotnet ef` nástroj, takže než budete moct použít, budete mít explicitně ji nainstalovat jako nástroj pro místní nebo globální.</span><span class="sxs-lookup"><span data-stu-id="45e8c-145">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="45e8c-146">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-146">**Why**</span></span>

<span data-ttu-id="45e8c-147">Tato změna umožňuje distribuci a aktualizaci `dotnet ef` jako regulární nástroj rozhraní příkazového řádku .NET na webu NuGet, konzistentní s skutečnost, že je EF Core 3.0 také vždy distribuován jako balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="45e8c-147">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="45e8c-148">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-148">**Mitigations**</span></span>

<span data-ttu-id="45e8c-149">Abyste mohli spravovat migrace nebo vygenerované uživatelské rozhraní `DbContext`, nainstalovat `dotnet-ef` pomocí `dotnet tool install` příkazu.</span><span class="sxs-lookup"><span data-stu-id="45e8c-149">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` using the `dotnet tool install` command.</span></span>
<span data-ttu-id="45e8c-150">Například můžete ji nainstalovat jako globální nástroj, zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="45e8c-150">For example, to install it as a global tool, you can type this command:</span></span>

  ``` console
  $ dotnet tool install --global dotnet-ef --version <exact-version>
  ```

<span data-ttu-id="45e8c-151">Můžete také získat jeho místní nástroj při obnovování závislosti projektu, který deklaruje jako závislost pomocí nástroje [soubor manifestu nástroje](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="45e8c-151">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

## <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="45e8c-152">Byla přejmenovaná FromSql ExecuteSql a ExecuteSqlAsync</span><span class="sxs-lookup"><span data-stu-id="45e8c-152">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="45e8c-153">Sledování problému #10996</span><span class="sxs-lookup"><span data-stu-id="45e8c-153">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="45e8c-154">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="45e8c-154">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="45e8c-155">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-155">**Old behavior**</span></span>

<span data-ttu-id="45e8c-156">Před EF Core 3.0 byly tyto názvy metod přetížení pro práci s normální řetězec nebo řetězec, který by měl být interpolovaných do SQL a parametry.</span><span class="sxs-lookup"><span data-stu-id="45e8c-156">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="45e8c-157">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-157">**New behavior**</span></span>

<span data-ttu-id="45e8c-158">Od verze EF Core 3.0, použijte `FromSqlRaw`, `ExecuteSqlRaw`, a `ExecuteSqlRawAsync` vytvořit parametrický dotaz, kde parametry jsou předány samostatně z řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="45e8c-158">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="45e8c-159">Příklad:</span><span class="sxs-lookup"><span data-stu-id="45e8c-159">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="45e8c-160">Použití `FromSqlInterpolated`, `ExecuteSqlInterpolated`, a `ExecuteSqlInterpolatedAsync` vytvořit parametrický dotaz, kde parametry jsou předány jako součást dotazu interpolované řetězce.</span><span class="sxs-lookup"><span data-stu-id="45e8c-160">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="45e8c-161">Příklad:</span><span class="sxs-lookup"><span data-stu-id="45e8c-161">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="45e8c-162">Všimněte si, že oba dotazy výše uvedené vytvoří stejný parametrizovaného dotazu SQL se stejnými parametry SQL.</span><span class="sxs-lookup"><span data-stu-id="45e8c-162">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="45e8c-163">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-163">**Why**</span></span>

<span data-ttu-id="45e8c-164">Přetížení metody, jako je to velmi usnadňují omylem volat metodu nezpracovaného řetězce, pokud bylo záměrem pro volání metody interpolovaný řetězec a naopak.</span><span class="sxs-lookup"><span data-stu-id="45e8c-164">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="45e8c-165">To může vést k dotazům naprostou když měla být parametrizovány.</span><span class="sxs-lookup"><span data-stu-id="45e8c-165">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="45e8c-166">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-166">**Mitigations**</span></span>

<span data-ttu-id="45e8c-167">Přepněte na nové názvy metod.</span><span class="sxs-lookup"><span data-stu-id="45e8c-167">Switch to use the new method names.</span></span>

## <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="45e8c-168">FromSql metody lze zadat pouze na dotaz kořeny</span><span class="sxs-lookup"><span data-stu-id="45e8c-168">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="45e8c-169">Sledování problému #15704</span><span class="sxs-lookup"><span data-stu-id="45e8c-169">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="45e8c-170">Tato změna je zavedená v EF Core 3.0-preview 6.</span><span class="sxs-lookup"><span data-stu-id="45e8c-170">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="45e8c-171">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-171">**Old behavior**</span></span>

<span data-ttu-id="45e8c-172">Před EF Core 3.0 `FromSql` může být zadaná metoda kdekoli v dotazu.</span><span class="sxs-lookup"><span data-stu-id="45e8c-172">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="45e8c-173">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-173">**New behavior**</span></span>

<span data-ttu-id="45e8c-174">Od verze EF Core 3.0, nový `FromSqlRaw` a `FromSqlInterpolated` metody (které nahradit `FromSql`) lze zadat pouze na dotaz kořeny, to znamená přímo na `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="45e8c-174">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="45e8c-175">Pokus zadat je někde jinde způsobí chybu kompilace.</span><span class="sxs-lookup"><span data-stu-id="45e8c-175">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="45e8c-176">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-176">**Why**</span></span>

<span data-ttu-id="45e8c-177">Určení `FromSql` kdekoli jiné než na `DbSet` bez přidání význam nebo přidanou hodnotu a v některých případech může způsobit nejednoznačnost.</span><span class="sxs-lookup"><span data-stu-id="45e8c-177">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="45e8c-178">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-178">**Mitigations**</span></span>

<span data-ttu-id="45e8c-179">`FromSql` volání by měl být přímo na přesunout `DbSet` na které se vztahují.</span><span class="sxs-lookup"><span data-stu-id="45e8c-179">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

## <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="45e8c-180">~~Provádění dotazu se protokoluje při ladění na úrovni~~ obnoveny</span><span class="sxs-lookup"><span data-stu-id="45e8c-180">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="45e8c-181">Sledování problému #14523</span><span class="sxs-lookup"><span data-stu-id="45e8c-181">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="45e8c-182">Tato změna se vrátí zpět v EF Core 3.0 – ve verzi preview 7.</span><span class="sxs-lookup"><span data-stu-id="45e8c-182">This change is reverted in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="45e8c-183">Jsme vrátit tuto změnu, protože nové konfigurace v EF Core 3.0 umožňuje úroveň protokolování pro události, které chcete být určená aplikací.</span><span class="sxs-lookup"><span data-stu-id="45e8c-183">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="45e8c-184">Například chcete-li přepnout protokolování SQL `Debug`, explicitně nakonfigurovat na úrovni `OnConfiguring` nebo `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="45e8c-184">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="45e8c-185">Dočasné hodnoty klíče už nejsou nastavené na instancí entit</span><span class="sxs-lookup"><span data-stu-id="45e8c-185">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="45e8c-186">Sledování problému #12378</span><span class="sxs-lookup"><span data-stu-id="45e8c-186">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="45e8c-187">Tato změna je zavedená v EF Core 3.0 – náhled 2.</span><span class="sxs-lookup"><span data-stu-id="45e8c-187">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="45e8c-188">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-188">**Old behavior**</span></span>

<span data-ttu-id="45e8c-189">Před EF Core 3.0 dočasné hodnoty přiřadily se všechny vlastnosti klíče, které by později skutečné hodnoty generován databází.</span><span class="sxs-lookup"><span data-stu-id="45e8c-189">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="45e8c-190">Obvykle tyto dočasné hodnoty byly velké záporná čísla.</span><span class="sxs-lookup"><span data-stu-id="45e8c-190">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="45e8c-191">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-191">**New behavior**</span></span>

<span data-ttu-id="45e8c-192">Od verze 3.0, EF Core ukládá dočasné hodnotu klíče jako součást informací o sledování entity a ponechá samotné beze změny vlastnosti klíče.</span><span class="sxs-lookup"><span data-stu-id="45e8c-192">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="45e8c-193">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-193">**Why**</span></span>

<span data-ttu-id="45e8c-194">Tato změna byla provedena zabránit chybně stávají trvalé při entita, která byla dříve sledován pomocí funkce některé dočasné hodnoty klíče `DbContext` instance se přesune na jiný `DbContext` instance.</span><span class="sxs-lookup"><span data-stu-id="45e8c-194">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="45e8c-195">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-195">**Mitigations**</span></span>

<span data-ttu-id="45e8c-196">Na staré chování může záviset aplikace, které přiřazují hodnoty primárního klíče na cizí klíče formuláře přidružení mezi entitami, pokud primární klíče generované úložištěm a patří do entity v `Added` stavu.</span><span class="sxs-lookup"><span data-stu-id="45e8c-196">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="45e8c-197">Toho se lze vyvarovat podle:</span><span class="sxs-lookup"><span data-stu-id="45e8c-197">This can be avoided by:</span></span>
* <span data-ttu-id="45e8c-198">Bez použití klíče generované úložištěm.</span><span class="sxs-lookup"><span data-stu-id="45e8c-198">Not using store-generated keys.</span></span>
* <span data-ttu-id="45e8c-199">Nastavení vlastnosti navigace na vztahů namísto nastavování hodnot cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="45e8c-199">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="45e8c-200">Získejte skutečné hodnoty dočasné klíče z informací o sledování entity.</span><span class="sxs-lookup"><span data-stu-id="45e8c-200">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="45e8c-201">Například `context.Entry(blog).Property(e => e.Id).CurrentValue` i v případě vrátí hodnotu dočasné `blog.Id` samotný nebyla nastavena.</span><span class="sxs-lookup"><span data-stu-id="45e8c-201">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="45e8c-202">Metoda DetectChanges respektuje klíčových hodnot generovaných úložištěm</span><span class="sxs-lookup"><span data-stu-id="45e8c-202">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="45e8c-203">Sledování problému #14616</span><span class="sxs-lookup"><span data-stu-id="45e8c-203">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="45e8c-204">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="45e8c-204">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="45e8c-205">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-205">**Old behavior**</span></span>

<span data-ttu-id="45e8c-206">Před EF Core 3.0 Nesledované entity objevila `DetectChanges` mají sledovat v `Added` stavu a vložené jako nový řádek, kdy `SaveChanges` je volána.</span><span class="sxs-lookup"><span data-stu-id="45e8c-206">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="45e8c-207">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-207">**New behavior**</span></span>

<span data-ttu-id="45e8c-208">Od verze EF Core 3.0, pokud používá entity generované hodnoty klíče a některá z hodnot klíče je nastavena, pak entity se bude sledovat v `Modified` stavu.</span><span class="sxs-lookup"><span data-stu-id="45e8c-208">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="45e8c-209">To znamená, že řádek entity se předpokládá, že existují a budou aktualizace `SaveChanges` je volána.</span><span class="sxs-lookup"><span data-stu-id="45e8c-209">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="45e8c-210">Pokud není nastavena hodnota klíče, nebo pokud není typ entity pomocí vygenerované klíče, pak nová entita se bude dál sledovat jako `Added` stejně jako v předchozích verzích.</span><span class="sxs-lookup"><span data-stu-id="45e8c-210">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="45e8c-211">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-211">**Why**</span></span>

<span data-ttu-id="45e8c-212">Tato změna byla provedena na umožňují snadněji a konzistentnější pro práci s grafy odpojené entity při použití klíče generované úložištěm.</span><span class="sxs-lookup"><span data-stu-id="45e8c-212">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="45e8c-213">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-213">**Mitigations**</span></span>

<span data-ttu-id="45e8c-214">Tato změna může přerušit aplikace, pokud typ entity je nakonfigurovaný na použití vygenerovat klíče, ale hodnoty klíče jsou explicitně nastavené pro nové instance.</span><span class="sxs-lookup"><span data-stu-id="45e8c-214">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="45e8c-215">Oprava je explicitně konfigurovat klíčové vlastnosti nejsou generovány pomocí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="45e8c-215">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="45e8c-216">Například pomocí rozhraní fluent API:</span><span class="sxs-lookup"><span data-stu-id="45e8c-216">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="45e8c-217">Nebo s anotacemi dat:</span><span class="sxs-lookup"><span data-stu-id="45e8c-217">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="45e8c-218">Kaskádové odstranění teď dojde okamžitě ve výchozím nastavení</span><span class="sxs-lookup"><span data-stu-id="45e8c-218">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="45e8c-219">Sledování problému #10114</span><span class="sxs-lookup"><span data-stu-id="45e8c-219">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="45e8c-220">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="45e8c-220">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="45e8c-221">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-221">**Old behavior**</span></span>

<span data-ttu-id="45e8c-222">Než 3.0 EF Core použít kaskádové akce (odstranění závislých položek při odstranění požadovaný instanční objekt nebo když porušeno vztah k požadované instanční objekt) nebyly provedeny dokud byla volána metoda SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="45e8c-222">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="45e8c-223">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-223">**New behavior**</span></span>

<span data-ttu-id="45e8c-224">Od verze 3.0, EF Core provede kaskádové akce ihned po zjištění spouštěcí podmínky.</span><span class="sxs-lookup"><span data-stu-id="45e8c-224">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="45e8c-225">Například volání `context.Remove()` odstranit instančního objektu entity se výsledek ve všech sledovat související požadované položky závislé na také nastavena na `Deleted` okamžitě.</span><span class="sxs-lookup"><span data-stu-id="45e8c-225">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="45e8c-226">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-226">**Why**</span></span>

<span data-ttu-id="45e8c-227">Tato změna byla provedena na zlepšení uživatelského rozhraní pro datových vazbách a auditování scénáře, kdy je potřeba vysvětlit entit, které se odstraní _před_ `SaveChanges` je volána.</span><span class="sxs-lookup"><span data-stu-id="45e8c-227">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="45e8c-228">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-228">**Mitigations**</span></span>

<span data-ttu-id="45e8c-229">Předchozí chování lze obnovit nastavením na `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="45e8c-229">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="45e8c-230">Příklad:</span><span class="sxs-lookup"><span data-stu-id="45e8c-230">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="45e8c-231">DeleteBehavior.Restrict má sémantiku čisticího modulu</span><span class="sxs-lookup"><span data-stu-id="45e8c-231">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="45e8c-232">Sledování problému #12661</span><span class="sxs-lookup"><span data-stu-id="45e8c-232">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="45e8c-233">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 5.</span><span class="sxs-lookup"><span data-stu-id="45e8c-233">This change is introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="45e8c-234">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-234">**Old behavior**</span></span>

<span data-ttu-id="45e8c-235">Než 3.0 `DeleteBehavior.Restrict` vytvořit cizí klíče v databázi s `Restrict` sémantiku, ale také změněné interní opravy není zřejmé způsobem.</span><span class="sxs-lookup"><span data-stu-id="45e8c-235">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="45e8c-236">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-236">**New behavior**</span></span>

<span data-ttu-id="45e8c-237">Od verze 3.0, `DeleteBehavior.Restrict` zajistí, že cizí klíče jsou vytvořeny pomocí `Restrict` sémantiku – tedy žádná cascades; throw na porušení omezení – bez vliv i na EF interní opravy.</span><span class="sxs-lookup"><span data-stu-id="45e8c-237">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="45e8c-238">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-238">**Why**</span></span>

<span data-ttu-id="45e8c-239">Tato změna byla provedena na zlepšení uživatelského rozhraní pro použití `DeleteBehavior` intuitivní způsobem, bez neočekávané vedlejší účinky.</span><span class="sxs-lookup"><span data-stu-id="45e8c-239">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="45e8c-240">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-240">**Mitigations**</span></span>

<span data-ttu-id="45e8c-241">Předchozí chování můžete obnovit pomocí `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="45e8c-241">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="45e8c-242">Typy dotazů jsou spojeny s typy entit</span><span class="sxs-lookup"><span data-stu-id="45e8c-242">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="45e8c-243">Sledování problému #14194</span><span class="sxs-lookup"><span data-stu-id="45e8c-243">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="45e8c-244">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="45e8c-244">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="45e8c-245">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-245">**Old behavior**</span></span>

<span data-ttu-id="45e8c-246">Před EF Core 3.0 [typy dotazů](xref:core/modeling/query-types) byly prostředky provádět dotazy na data, která nedefinuje primární klíč strukturovaným způsobem.</span><span class="sxs-lookup"><span data-stu-id="45e8c-246">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="45e8c-247">To znamená typ dotazu byla použita pro mapování typů entit bez klíčů (pravděpodobně ze zobrazení, ale pravděpodobně z tabulky) při pravidelných entity typ byl použit při klíč byl k dispozici (více pravděpodobně z tabulky, ale pravděpodobně ze zobrazení).</span><span class="sxs-lookup"><span data-stu-id="45e8c-247">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="45e8c-248">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-248">**New behavior**</span></span>

<span data-ttu-id="45e8c-249">Typ dotazu teď bude pouze typ entity bez primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="45e8c-249">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="45e8c-250">Typy entit bez kódu mají stejné funkce jako typy dotazů v předchozích verzích.</span><span class="sxs-lookup"><span data-stu-id="45e8c-250">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="45e8c-251">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-251">**Why**</span></span>

<span data-ttu-id="45e8c-252">Tato změna byla provedena, abyste snížili nejasnosti kolem účel typy dotazů.</span><span class="sxs-lookup"><span data-stu-id="45e8c-252">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="45e8c-253">Konkrétně jsou typy bez klíčů entit a z tohoto důvodu jsou ze své podstaty jen pro čtení, ale by neměl být použit pouze z důvodu typu entity musí být jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="45e8c-253">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="45e8c-254">Podobně jsou často namapované na zobrazení, ale je to jenom, protože zobrazení není často definují klíče.</span><span class="sxs-lookup"><span data-stu-id="45e8c-254">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="45e8c-255">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-255">**Mitigations**</span></span>

<span data-ttu-id="45e8c-256">Následující části rozhraní API jsou teď zastaralé:</span><span class="sxs-lookup"><span data-stu-id="45e8c-256">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="45e8c-257">**`ModelBuilder.Query<>()`** – Místo toho `ModelBuilder.Entity<>().HasNoKey()` musí být volána k označení typu entity tak, že má žádné klíče.</span><span class="sxs-lookup"><span data-stu-id="45e8c-257">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="45e8c-258">To by stále probíhá konfigurace podle konvence se vyhnete chybné konfigurace, když primární klíč se očekává, ale neodpovídá konvenci.</span><span class="sxs-lookup"><span data-stu-id="45e8c-258">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="45e8c-259">**`DbQuery<>`** – Místo toho `DbSet<>` by měla sloužit.</span><span class="sxs-lookup"><span data-stu-id="45e8c-259">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="45e8c-260">**`DbContext.Query<>()`** – Místo toho `DbContext.Set<>()` by měla sloužit.</span><span class="sxs-lookup"><span data-stu-id="45e8c-260">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="45e8c-261">Došlo ke změně konfigurace rozhraní API pro vlastní typ relace</span><span class="sxs-lookup"><span data-stu-id="45e8c-261">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="45e8c-262">[Sledování problému #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[sledování problému #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[sledování problému #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="45e8c-262">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="45e8c-263">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="45e8c-263">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="45e8c-264">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-264">**Old behavior**</span></span>

<span data-ttu-id="45e8c-265">Před EF Core 3.0 byla provedena konfigurace vlastněné vztah přímo po `OwnsOne` nebo `OwnsMany` volání.</span><span class="sxs-lookup"><span data-stu-id="45e8c-265">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="45e8c-266">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-266">**New behavior**</span></span>

<span data-ttu-id="45e8c-267">Od verze EF Core 3.0, že už fluent API pro konfiguraci vlastnosti navigace vlastníkovi použitím `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="45e8c-267">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="45e8c-268">Příklad:</span><span class="sxs-lookup"><span data-stu-id="45e8c-268">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="45e8c-269">Konfigurace související se o vztah mezi vlastníka a vlastní by měl být zřetězené teď po `WithOwner()` podobně jako na konfiguraci jiné vztahy.</span><span class="sxs-lookup"><span data-stu-id="45e8c-269">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="45e8c-270">Při konfiguraci pro vlastní typ, samotný by stále možné zřetězit po `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="45e8c-270">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="45e8c-271">Příklad:</span><span class="sxs-lookup"><span data-stu-id="45e8c-271">For example:</span></span>

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

<span data-ttu-id="45e8c-272">Kromě volá `Entity()`, `HasOne()`, nebo `Set()` s typem vlastnictví cíl se vyvolají výjimku.</span><span class="sxs-lookup"><span data-stu-id="45e8c-272">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="45e8c-273">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-273">**Why**</span></span>

<span data-ttu-id="45e8c-274">Tato změna byla provedena vytvořit jasnější oddělení mezi konfigurací samotného typu vlastnictví a _vztah k_ typ vlastnictví.</span><span class="sxs-lookup"><span data-stu-id="45e8c-274">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="45e8c-275">Tím zase nejednoznačnosti a záměny kolem metody, jako je `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="45e8c-275">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="45e8c-276">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-276">**Mitigations**</span></span>

<span data-ttu-id="45e8c-277">Změna konfigurace vlastněné typ vztahů používat nové plochy rozhraní API, jak je znázorněno v příkladu výše.</span><span class="sxs-lookup"><span data-stu-id="45e8c-277">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="45e8c-278">Závislých položek sdílení v tabulce k objektu zabezpečení jsou teď nepovinné.</span><span class="sxs-lookup"><span data-stu-id="45e8c-278">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="45e8c-279">Sledování problému #9005</span><span class="sxs-lookup"><span data-stu-id="45e8c-279">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="45e8c-280">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="45e8c-280">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="45e8c-281">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-281">**Old behavior**</span></span>

<span data-ttu-id="45e8c-282">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="45e8c-282">Consider the following model:</span></span>
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
<span data-ttu-id="45e8c-283">Před EF Core 3.0, pokud `OrderDetails` vlastní `Order` nebo explicitně namapované na stejnou tabulku a `OrderDetails` instance byla požadována vždy při přidání nového `Order`.</span><span class="sxs-lookup"><span data-stu-id="45e8c-283">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="45e8c-284">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-284">**New behavior**</span></span>

<span data-ttu-id="45e8c-285">Od verze 3.0, EF Core umožňuje přidat `Order` bez `OrderDetails` a mapuje všechny `OrderDetails` vlastnosti s výjimkou primární klíč pro sloupce s možnou hodnotou Null.</span><span class="sxs-lookup"><span data-stu-id="45e8c-285">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="45e8c-286">Při dotazování na EF Core sady `OrderDetails` k `null` Pokud některou z jejích požadovaných vlastností nemá žádnou hodnotu nebo nemá žádné požadované vlastnosti kromě primárního klíče a všechny vlastnosti jsou `null`.</span><span class="sxs-lookup"><span data-stu-id="45e8c-286">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="45e8c-287">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-287">**Mitigations**</span></span>

<span data-ttu-id="45e8c-288">Pokud váš model obsahuje tabulku sdílení závislé se všemi sloupci volitelné, ale navigace ukazatel není má být `null` pak aplikace by měla upravit tak, aby zpracovat případy při navigaci `null`.</span><span class="sxs-lookup"><span data-stu-id="45e8c-288">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="45e8c-289">Pokud to není možné požadovanou vlastnost měla být přidána do typu entity nebo musí mít alespoň jednu vlastnost non -`null` přiřazena hodnota.</span><span class="sxs-lookup"><span data-stu-id="45e8c-289">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

## <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="45e8c-290">Všechny entity sdílení tabulku se sloupcem tokenů souběžnosti mít mapování na vlastnost</span><span class="sxs-lookup"><span data-stu-id="45e8c-290">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="45e8c-291">Sledování problému #14154</span><span class="sxs-lookup"><span data-stu-id="45e8c-291">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="45e8c-292">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="45e8c-292">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="45e8c-293">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-293">**Old behavior**</span></span>

<span data-ttu-id="45e8c-294">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="45e8c-294">Consider the following model:</span></span>
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
<span data-ttu-id="45e8c-295">Před EF Core 3.0, pokud `OrderDetails` vlastní `Order` nebo explicitně namapované na stejnou tabulku následně aktualizovat jenom `OrderDetails` neaktualizuje `Version` hodnoty na klientovi a příští aktualizace se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="45e8c-295">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="45e8c-296">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-296">**New behavior**</span></span>

<span data-ttu-id="45e8c-297">Od verze 3.0, rozšíří EF Core nové `Version` hodnota, která se `Order` Pokud vlastní `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="45e8c-297">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="45e8c-298">V opačném případě dojde k výjimce během ověření modelu.</span><span class="sxs-lookup"><span data-stu-id="45e8c-298">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="45e8c-299">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-299">**Why**</span></span>

<span data-ttu-id="45e8c-300">Tato změna byla provedena, když se aktualizuje jenom jeden z entity, které jsou namapované na stejnou tabulku, aby hodnota tokenu zastaralé souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="45e8c-300">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="45e8c-301">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-301">**Mitigations**</span></span>

<span data-ttu-id="45e8c-302">Všechny entity v tabulce pro sdílení obsahu se mají zahrnout vlastnost, která je namapovaná na sloupci tokenů souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="45e8c-302">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="45e8c-303">Je možné vytvořit, jeden v stínové stavu:</span><span class="sxs-lookup"><span data-stu-id="45e8c-303">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

## <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="45e8c-304">Vlastnosti zděděné nenamapované typy jsou nyní mapovány do jednoho sloupce pro všechny odvozené typy</span><span class="sxs-lookup"><span data-stu-id="45e8c-304">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="45e8c-305">Sledování problému #13998</span><span class="sxs-lookup"><span data-stu-id="45e8c-305">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="45e8c-306">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="45e8c-306">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="45e8c-307">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-307">**Old behavior**</span></span>

<span data-ttu-id="45e8c-308">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="45e8c-308">Consider the following model:</span></span>
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

<span data-ttu-id="45e8c-309">Před EF Core 3.0 `ShippingAddress` vlastnost by být mapována k oddělení sloupců pro `BulkOrder` a `Order` ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="45e8c-309">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="45e8c-310">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-310">**New behavior**</span></span>

<span data-ttu-id="45e8c-311">Od verze 3.0, EF Core vytvoří pouze jeden sloupec pro `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="45e8c-311">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="45e8c-312">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-312">**Why**</span></span>

<span data-ttu-id="45e8c-313">Staré chování nebyl očekáván.</span><span class="sxs-lookup"><span data-stu-id="45e8c-313">The old behavoir was unexpected.</span></span>

<span data-ttu-id="45e8c-314">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-314">**Mitigations**</span></span>

<span data-ttu-id="45e8c-315">Vlastnost je stále explicitně mapovat pro oddělení sloupců v odvozených typů:</span><span class="sxs-lookup"><span data-stu-id="45e8c-315">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="45e8c-316">Konvence vlastnost cizího klíče už neodpovídá stejný název jako vlastnost instančního objektu</span><span class="sxs-lookup"><span data-stu-id="45e8c-316">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="45e8c-317">Sledování problému #13274</span><span class="sxs-lookup"><span data-stu-id="45e8c-317">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="45e8c-318">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="45e8c-318">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="45e8c-319">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-319">**Old behavior**</span></span>

<span data-ttu-id="45e8c-320">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="45e8c-320">Consider the following model:</span></span>
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
<span data-ttu-id="45e8c-321">Před EF Core 3.0 `CustomerId` vlastnost se použije pro cizí klíč konvencí.</span><span class="sxs-lookup"><span data-stu-id="45e8c-321">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="45e8c-322">Nicméně pokud `Order` je typ vlastnictví, a potom to by také provést `CustomerId` primární klíč a to se většinou očekává.</span><span class="sxs-lookup"><span data-stu-id="45e8c-322">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="45e8c-323">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-323">**New behavior**</span></span>

<span data-ttu-id="45e8c-324">Od verze 3.0, EF Core nesnaží při využívání vlastností cizího klíče podle konvence, pokud mají stejný název jako vlastnost instančního objektu.</span><span class="sxs-lookup"><span data-stu-id="45e8c-324">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="45e8c-325">Název instančního objektu typu zřetězený s názvem hlavní vlastnosti a název navigační zřetězená s vzory názvů vlastnosti principal budou stále odpovídat.</span><span class="sxs-lookup"><span data-stu-id="45e8c-325">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="45e8c-326">Příklad:</span><span class="sxs-lookup"><span data-stu-id="45e8c-326">For example:</span></span>

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

<span data-ttu-id="45e8c-327">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-327">**Why**</span></span>

<span data-ttu-id="45e8c-328">Tato změna byla provedena na chybně nedefinujte vlastnost primárního klíče typ vlastnictví.</span><span class="sxs-lookup"><span data-stu-id="45e8c-328">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="45e8c-329">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-329">**Mitigations**</span></span>

<span data-ttu-id="45e8c-330">Pokud byla vlastnost má být cizí klíč a proto část primárního klíče a pak explicitně jako takový ho nakonfigurujte.</span><span class="sxs-lookup"><span data-stu-id="45e8c-330">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="45e8c-331">Připojení k databázi je nyní uzavřeno, pokud není využito už před objektu TransactionScope byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="45e8c-331">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="45e8c-332">Sledování problému #14218</span><span class="sxs-lookup"><span data-stu-id="45e8c-332">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="45e8c-333">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="45e8c-333">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="45e8c-334">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-334">**Old behavior**</span></span>

<span data-ttu-id="45e8c-335">Před EF Core 3.0, pokud kontext otevře připojení k uvnitř `TransactionScope`, připojení zůstane otevřená, při aktuální `TransactionScope` je aktivní.</span><span class="sxs-lookup"><span data-stu-id="45e8c-335">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="45e8c-336">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-336">**New behavior**</span></span>

<span data-ttu-id="45e8c-337">Od verze 3.0, EF Core uzavře připojení co nejdříve po dokončení jeho použití.</span><span class="sxs-lookup"><span data-stu-id="45e8c-337">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="45e8c-338">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-338">**Why**</span></span>

<span data-ttu-id="45e8c-339">Tato změna umožňuje použití několika kontextech v rámci stejného `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="45e8c-339">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="45e8c-340">Nové chování také odpovídající EF6.</span><span class="sxs-lookup"><span data-stu-id="45e8c-340">The new behavior also matches EF6.</span></span>

<span data-ttu-id="45e8c-341">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-341">**Mitigations**</span></span>

<span data-ttu-id="45e8c-342">Pokud je nutné připojení zůstat otevřené explicitní volání konstruktoru `OpenConnection()` zajistí, že EF Core nezavírá je předčasně ukončena:</span><span class="sxs-lookup"><span data-stu-id="45e8c-342">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="45e8c-343">Jednotlivé vlastnosti pomocí generování klíčů nezávislé celé číslo v paměti</span><span class="sxs-lookup"><span data-stu-id="45e8c-343">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="45e8c-344">Sledování problému #6872</span><span class="sxs-lookup"><span data-stu-id="45e8c-344">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="45e8c-345">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="45e8c-345">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="45e8c-346">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-346">**Old behavior**</span></span>

<span data-ttu-id="45e8c-347">Před EF Core 3.0 se použil jeden generátor sdíleného hodnot pro všechny vlastnosti klíče celé číslo v paměti.</span><span class="sxs-lookup"><span data-stu-id="45e8c-347">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="45e8c-348">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-348">**New behavior**</span></span>

<span data-ttu-id="45e8c-349">Od verze EF Core 3.0, každé celé číslo klíčová vlastnost získá vlastní generátor hodnot při použití databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="45e8c-349">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="45e8c-350">Také pokud se odstraní databáze, pak generování klíčů je obnovit pro všechny tabulky.</span><span class="sxs-lookup"><span data-stu-id="45e8c-350">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="45e8c-351">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-351">**Why**</span></span>

<span data-ttu-id="45e8c-352">Tato změna byla provedena zarovnat generování klíče v paměti blíže k generování klíčů skutečná databáze a zlepšit schopnost izolace testů od sebe navzájem při použití databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="45e8c-352">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="45e8c-353">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-353">**Mitigations**</span></span>

<span data-ttu-id="45e8c-354">Toto může rozbít aplikaci, která se spoléhá na konkrétní hodnoty klíče v paměti nastavit.</span><span class="sxs-lookup"><span data-stu-id="45e8c-354">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="45e8c-355">Zvažte místo toho spoléhat na konkrétní hodnoty klíče, ani aktualizaci tak, aby odpovídala nové chování.</span><span class="sxs-lookup"><span data-stu-id="45e8c-355">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="45e8c-356">Základní pole se používají ve výchozím nastavení</span><span class="sxs-lookup"><span data-stu-id="45e8c-356">Backing fields are used by default</span></span>

[<span data-ttu-id="45e8c-357">Sledování problému #12430</span><span class="sxs-lookup"><span data-stu-id="45e8c-357">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="45e8c-358">Tato změna je zavedená v EF Core 3.0 – náhled 2.</span><span class="sxs-lookup"><span data-stu-id="45e8c-358">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="45e8c-359">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-359">**Old behavior**</span></span>

<span data-ttu-id="45e8c-360">Než 3.0 i v případě, že pomocné pole vlastnosti zná, EF Core by stále ve výchozím nastavení čtení a zápisu hodnoty vlastnosti pomocí metody getter a setter vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="45e8c-360">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="45e8c-361">K tomuto výjimka provádění dotazů, kde pomocné pole se nastavuje přímo Pokud jsou známé.</span><span class="sxs-lookup"><span data-stu-id="45e8c-361">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="45e8c-362">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-362">**New behavior**</span></span>

<span data-ttu-id="45e8c-363">Počínaje EF Core 3.0, pokud jsou známé vlastnosti pole zálohování, potom EF Core budou vždy číst a Zapisovat vlastnosti pomocí pole zálohování.</span><span class="sxs-lookup"><span data-stu-id="45e8c-363">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="45e8c-364">Pokud aplikace se spoléhá na další chování zakódovaný do metody getter nebo setter to může způsobit přerušení aplikace.</span><span class="sxs-lookup"><span data-stu-id="45e8c-364">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="45e8c-365">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-365">**Why**</span></span>

<span data-ttu-id="45e8c-366">Tato změna byla provedena zabránit EF Core chybně aktivací obchodní logiky ve výchozím nastavení při provádění databázových operací zahrnující entity.</span><span class="sxs-lookup"><span data-stu-id="45e8c-366">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="45e8c-367">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-367">**Mitigations**</span></span>

<span data-ttu-id="45e8c-368">Pre-3.0 chování lze obnovit prostřednictvím konfigurace režim přístupu k vlastnosti na `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="45e8c-368">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="45e8c-369">Příklad:</span><span class="sxs-lookup"><span data-stu-id="45e8c-369">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="45e8c-370">Vyvolání výjimky, pokud je nalezeno více polí kompatibilní zálohování</span><span class="sxs-lookup"><span data-stu-id="45e8c-370">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="45e8c-371">Sledování problému #12523</span><span class="sxs-lookup"><span data-stu-id="45e8c-371">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="45e8c-372">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="45e8c-372">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="45e8c-373">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-373">**Old behavior**</span></span>

<span data-ttu-id="45e8c-374">Před EF Core 3.0 Pokud odpovídá více polí pravidel pro vyhledání pomocným polem vlastnosti, pak vybere jedno pole na základě některých pořadí priority.</span><span class="sxs-lookup"><span data-stu-id="45e8c-374">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="45e8c-375">To může způsobit nesprávné pole pro použití v případech, nejednoznačný.</span><span class="sxs-lookup"><span data-stu-id="45e8c-375">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="45e8c-376">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-376">**New behavior**</span></span>

<span data-ttu-id="45e8c-377">Od verze EF Core 3.0, pokud více polí budou odpovídat na stejnou vlastnost, pak je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="45e8c-377">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="45e8c-378">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-378">**Why**</span></span>

<span data-ttu-id="45e8c-379">Tato změna byla provedena, abyste se vyhnuli použití tiše jedno pole před jiným při může obsahovat jen jedno správné.</span><span class="sxs-lookup"><span data-stu-id="45e8c-379">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="45e8c-380">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-380">**Mitigations**</span></span>

<span data-ttu-id="45e8c-381">Vlastnosti s nejednoznačný základní pole musí mít pole pro použití explicitně zadán.</span><span class="sxs-lookup"><span data-stu-id="45e8c-381">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="45e8c-382">Například pomocí rozhraní fluent API:</span><span class="sxs-lookup"><span data-stu-id="45e8c-382">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="45e8c-383">Vlastnost jen pro pole názvů by měl odpovídat názvu pole</span><span class="sxs-lookup"><span data-stu-id="45e8c-383">Field-only property names should match the field name</span></span>

<span data-ttu-id="45e8c-384">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="45e8c-384">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="45e8c-385">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-385">**Old behavior**</span></span>

<span data-ttu-id="45e8c-386">Před EF Core 3.0 vlastnost může být určeno hodnotu řetězce a pokud nebyla nalezena žádná vlastnost s tímto názvem na typu CLR EF Core by opakujte tak, aby odpovídaly na pole pomocí pravidel pro vytváření názvů.</span><span class="sxs-lookup"><span data-stu-id="45e8c-386">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="45e8c-387">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-387">**New behavior**</span></span>

<span data-ttu-id="45e8c-388">Od verze EF Core 3.0, vlastnost jen pro pole musí odpovídat názvu pole přesně.</span><span class="sxs-lookup"><span data-stu-id="45e8c-388">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="45e8c-389">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-389">**Why**</span></span>

<span data-ttu-id="45e8c-390">Tato změna byla provedena, abyste se vyhnuli použití stejné pole pro dvě vlastnosti s názvem podobně, také udržuje pravidla pro porovnávání vlastností pouze pole je stejný jako u vlastnosti, které jsou namapovány na vlastnosti CLR.</span><span class="sxs-lookup"><span data-stu-id="45e8c-390">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="45e8c-391">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-391">**Mitigations**</span></span>

<span data-ttu-id="45e8c-392">Vlastnosti pouze pro pole musí mít název jako pole, které jsou mapovány na stejný.</span><span class="sxs-lookup"><span data-stu-id="45e8c-392">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="45e8c-393">V novější verzi preview EF Core 3.0 Plánujeme znovu povolit explicitně konfigurace název pole, které se liší od názvu vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="45e8c-393">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="45e8c-394">AddDbContext/AddDbContextPool zavolejte už AddLogging a AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="45e8c-394">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="45e8c-395">Sledování problému #14756</span><span class="sxs-lookup"><span data-stu-id="45e8c-395">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="45e8c-396">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="45e8c-396">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="45e8c-397">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-397">**Old behavior**</span></span>

<span data-ttu-id="45e8c-398">Před EF Core 3.0, volání `AddDbContext` nebo `AddDbContextPool` by také zaregistrovat protokolování a ukládání služeb s D.I prostřednictvím volání do mezipaměti [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) a [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="45e8c-398">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="45e8c-399">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-399">**New behavior**</span></span>

<span data-ttu-id="45e8c-400">Od verze EF Core 3.0, `AddDbContext` a `AddDbContextPool` nebude registru tyto služby s DI (Dependency Injection).</span><span class="sxs-lookup"><span data-stu-id="45e8c-400">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="45e8c-401">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-401">**Why**</span></span>

<span data-ttu-id="45e8c-402">EF Core 3.0 nevyžaduje, že tyto služby jsou v kontejneru DI vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="45e8c-402">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="45e8c-403">Nicméně pokud `ILoggerFactory` je zaregistrovaný v aplikačním kontejneru DI, pak bude i nadále využívat EF Core.</span><span class="sxs-lookup"><span data-stu-id="45e8c-403">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="45e8c-404">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-404">**Mitigations**</span></span>

<span data-ttu-id="45e8c-405">Pokud vaše aplikace potřebuje tyto služby, registrujte je explicitně s využitím kontejnerů DI [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) nebo [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="45e8c-405">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="45e8c-406">DbContext.Entry teď provádí místní metoda DetectChanges</span><span class="sxs-lookup"><span data-stu-id="45e8c-406">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="45e8c-407">Sledování problému #13552</span><span class="sxs-lookup"><span data-stu-id="45e8c-407">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="45e8c-408">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="45e8c-408">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="45e8c-409">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-409">**Old behavior**</span></span>

<span data-ttu-id="45e8c-410">Před EF Core 3.0, volání `DbContext.Entry` způsobí změny, aby se rozpoznal pro všechny sledované entity.</span><span class="sxs-lookup"><span data-stu-id="45e8c-410">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="45e8c-411">Tím zajistíte, že stav zpřístupněn v `EntityEntry` byla aktuální.</span><span class="sxs-lookup"><span data-stu-id="45e8c-411">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="45e8c-412">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-412">**New behavior**</span></span>

<span data-ttu-id="45e8c-413">Spouští se s EF Core 3.0, volání `DbContext.Entry` se teď jenom pokus o zjištění změn v dané entitě a všechny sledované instančního objektu entity, které s ním souvisejí.</span><span class="sxs-lookup"><span data-stu-id="45e8c-413">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="45e8c-414">To znamená, že se změní jinde nemusí byl zjištěn zavoláním této metody, které by mohly mít vliv na stav aplikace.</span><span class="sxs-lookup"><span data-stu-id="45e8c-414">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="45e8c-415">Všimněte si, že pokud `ChangeTracker.AutoDetectChangesEnabled` je nastavena na `false` pak i tato detekce místní změny se deaktivuje.</span><span class="sxs-lookup"><span data-stu-id="45e8c-415">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="45e8c-416">Jiné metody, které způsobují detekce změn – například `ChangeTracker.Entries` a `SaveChanges`– způsobí úplné `DetectChanges` všech sledovat entity.</span><span class="sxs-lookup"><span data-stu-id="45e8c-416">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="45e8c-417">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-417">**Why**</span></span>

<span data-ttu-id="45e8c-418">Tato změna byla provedena pro zlepšení výkonu výchozí použití `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="45e8c-418">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="45e8c-419">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-419">**Mitigations**</span></span>

<span data-ttu-id="45e8c-420">Volání `ChgangeTracker.DetectChanges()` explicitně před voláním `Entry` k zajištění chování pre-3.0.</span><span class="sxs-lookup"><span data-stu-id="45e8c-420">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="45e8c-421">Řetězec a bajtové pole klíčů se nerozlišují klientem generovaná ve výchozím nastavení</span><span class="sxs-lookup"><span data-stu-id="45e8c-421">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="45e8c-422">Sledování problému #14617</span><span class="sxs-lookup"><span data-stu-id="45e8c-422">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="45e8c-423">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="45e8c-423">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="45e8c-424">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-424">**Old behavior**</span></span>

<span data-ttu-id="45e8c-425">Před EF Core 3.0 `string` a `byte[]` klíčové vlastnosti použití explicitním nastavením nenulová hodnota.</span><span class="sxs-lookup"><span data-stu-id="45e8c-425">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="45e8c-426">V takovém případě by se vygenerovala hodnota klíče na klientovi jako identifikátor GUID serializovat do bajtů pro `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="45e8c-426">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="45e8c-427">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-427">**New behavior**</span></span>

<span data-ttu-id="45e8c-428">Od verze EF Core 3.0 výjimka bude vyvolána označující, že nebyla nastavena žádná hodnota klíče.</span><span class="sxs-lookup"><span data-stu-id="45e8c-428">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="45e8c-429">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-429">**Why**</span></span>

<span data-ttu-id="45e8c-430">Tato změna byla provedena, protože klient generován `string` / `byte[]` hodnoty nejsou obecně užitečné a výchozí chování dostal pevné argumentovat o generované hodnoty klíče v běžným způsobem.</span><span class="sxs-lookup"><span data-stu-id="45e8c-430">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="45e8c-431">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-431">**Mitigations**</span></span>

<span data-ttu-id="45e8c-432">Chování pre-3.0 je možné získat tak, že explicitně zadáte, by měl klíčové vlastnosti použít generované hodnoty, pokud je nastavena žádná hodnota jiná než null.</span><span class="sxs-lookup"><span data-stu-id="45e8c-432">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="45e8c-433">Například pomocí rozhraní fluent API:</span><span class="sxs-lookup"><span data-stu-id="45e8c-433">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="45e8c-434">Nebo s anotacemi dat:</span><span class="sxs-lookup"><span data-stu-id="45e8c-434">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="45e8c-435">Implementaci třídy ILoggerFactory je nyní vymezené služby</span><span class="sxs-lookup"><span data-stu-id="45e8c-435">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="45e8c-436">Sledování problému #14698</span><span class="sxs-lookup"><span data-stu-id="45e8c-436">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="45e8c-437">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="45e8c-437">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="45e8c-438">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-438">**Old behavior**</span></span>

<span data-ttu-id="45e8c-439">Před EF Core 3.0 `ILoggerFactory` bylo zaregistrováno jako služba typu singleton.</span><span class="sxs-lookup"><span data-stu-id="45e8c-439">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="45e8c-440">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-440">**New behavior**</span></span>

<span data-ttu-id="45e8c-441">Od verze EF Core 3.0, `ILoggerFactory` je teď zaregistrovaný jako obor.</span><span class="sxs-lookup"><span data-stu-id="45e8c-441">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="45e8c-442">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-442">**Why**</span></span>

<span data-ttu-id="45e8c-443">Tato změna byla provedena umožňující přidružení protokolovací nástroj se `DbContext` instanci, která povoluje další funkce a odebírá někdy patologických chování, jako je například obrovské množství poskytovatelů vnitřní chybě služby.</span><span class="sxs-lookup"><span data-stu-id="45e8c-443">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="45e8c-444">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-444">**Mitigations**</span></span>

<span data-ttu-id="45e8c-445">Tato změna by neměla mít vliv kód aplikace, pokud není registrace a použití vlastních služeb v poskytovateli EF Core vnitřní chybě služby.</span><span class="sxs-lookup"><span data-stu-id="45e8c-445">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="45e8c-446">Tato akce není běžné.</span><span class="sxs-lookup"><span data-stu-id="45e8c-446">This isn't common.</span></span>
<span data-ttu-id="45e8c-447">V těchto případech většinu toho, co bude dál fungovat, ale žádné služby typu singleton, která byla v závislosti na `ILoggerFactory` bude nutné změnit tak, aby získat `ILoggerFactory` jiným způsobem.</span><span class="sxs-lookup"><span data-stu-id="45e8c-447">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="45e8c-448">Pokud narazíte na situace tímto způsobem, založte prosím problém na na [sledování problémů Githubu EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) a dejte nám vědět, jak používáte `ILoggerFactory` tak, aby nám můžete lépe porozumět nechcete v budoucnu znovu rozdělit.</span><span class="sxs-lookup"><span data-stu-id="45e8c-448">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="45e8c-449">Opožděné načtení proxy už předpokládají, že jsou plně načteny navigační vlastnosti</span><span class="sxs-lookup"><span data-stu-id="45e8c-449">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="45e8c-450">Sledování problému #12780</span><span class="sxs-lookup"><span data-stu-id="45e8c-450">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="45e8c-451">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="45e8c-451">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="45e8c-452">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-452">**Old behavior**</span></span>

<span data-ttu-id="45e8c-453">Před EF Core 3.0, jednou `DbContext` byl odstraněn neexistoval způsob, jak zjistit, zda danou navigační vlastnost s entitou získané z daného kontextu byl plně načten či nikoli.</span><span class="sxs-lookup"><span data-stu-id="45e8c-453">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="45e8c-454">Proxy by místo toho se předpokládá, že je načtena navigační odkaz, pokud má nenulovou hodnotu a, že je načtena navigace kolekce, pokud není prázdná.</span><span class="sxs-lookup"><span data-stu-id="45e8c-454">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="45e8c-455">V těchto případech by pokus o opožděné načtení no-op.</span><span class="sxs-lookup"><span data-stu-id="45e8c-455">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="45e8c-456">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-456">**New behavior**</span></span>

<span data-ttu-id="45e8c-457">Od verze EF Core 3.0, proxy servery sledovat, jestli je načtena navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="45e8c-457">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="45e8c-458">To znamená, že při pokusu o přístup k navigační vlastnost, která je načtena po kontext se vyřadil. bude vždycky no-op, i v případě, že je načteno navigace je prázdný nebo mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="45e8c-458">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="45e8c-459">Naopak pokusu o přístup k navigační vlastnosti, které není načteno vyvolají výjimku pokud uvolnění kontextu, i když je navigační vlastnost kolekce není prázdná.</span><span class="sxs-lookup"><span data-stu-id="45e8c-459">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="45e8c-460">Pokud k této situaci dochází, znamená to kód aplikace se pokouší použít opožděné načtení na neplatný čas a aplikace by měla být změněna na tuto akci.</span><span class="sxs-lookup"><span data-stu-id="45e8c-460">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="45e8c-461">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-461">**Why**</span></span>

<span data-ttu-id="45e8c-462">Tato změna byla provedena, aby chování konzistentní a správné při pokusu o opožděné načtení na vyřazený `DbContext` instance.</span><span class="sxs-lookup"><span data-stu-id="45e8c-462">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="45e8c-463">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-463">**Mitigations**</span></span>

<span data-ttu-id="45e8c-464">Kód aplikace, který nebude pokoušet opožděné načtení vyřazený kontextu, nebo nakonfigurovat to být no-op. jak je popsáno v zpráva o výjimce.</span><span class="sxs-lookup"><span data-stu-id="45e8c-464">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="45e8c-465">Nadměrné vytváření zprostředkovatelů vnitřní chybě služby je teď k chybě ve výchozím nastavení</span><span class="sxs-lookup"><span data-stu-id="45e8c-465">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="45e8c-466">Sledování problému #10236</span><span class="sxs-lookup"><span data-stu-id="45e8c-466">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="45e8c-467">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="45e8c-467">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="45e8c-468">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-468">**Old behavior**</span></span>

<span data-ttu-id="45e8c-469">Upozornění by před EF Core 3.0 zaznamenávané aplikace vytváří patologických několik poskytovatelů vnitřní chybě služby.</span><span class="sxs-lookup"><span data-stu-id="45e8c-469">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="45e8c-470">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-470">**New behavior**</span></span>

<span data-ttu-id="45e8c-471">Od verze EF Core 3.0, toto upozornění se teď považuje za a chyby a výjimky je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="45e8c-471">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="45e8c-472">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-472">**Why**</span></span>

<span data-ttu-id="45e8c-473">Tato změna byla provedena na připravovat lepší kód aplikace prostřednictvím vystavení takovém patologických více explicitně.</span><span class="sxs-lookup"><span data-stu-id="45e8c-473">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="45e8c-474">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-474">**Mitigations**</span></span>

<span data-ttu-id="45e8c-475">Nejvhodnější příčinu akce při výskytu této chyby je zjistěte původní příčinu a nevytvářet mnoho poskytovatelů vnitřní chybě služby.</span><span class="sxs-lookup"><span data-stu-id="45e8c-475">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="45e8c-476">Však chyba může být převést zpět na upozornění (nebo ignorovat) prostřednictvím konfigurace na `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="45e8c-476">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="45e8c-477">Příklad:</span><span class="sxs-lookup"><span data-stu-id="45e8c-477">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="45e8c-478">Nové chování pro HasOne/HasMany volat jeden řetězec</span><span class="sxs-lookup"><span data-stu-id="45e8c-478">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="45e8c-479">Sledování problému #9171</span><span class="sxs-lookup"><span data-stu-id="45e8c-479">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="45e8c-480">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="45e8c-480">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="45e8c-481">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-481">**Old behavior**</span></span>

<span data-ttu-id="45e8c-482">Před EF Core 3.0 kód volání `HasOne` nebo `HasMany` s jeden řetězec byl interpretován tak matoucí.</span><span class="sxs-lookup"><span data-stu-id="45e8c-482">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="45e8c-483">Příklad:</span><span class="sxs-lookup"><span data-stu-id="45e8c-483">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="45e8c-484">Kód vypadá je týkající se `Samurai` některé jiné entity typu použití `Entrance` navigační vlastnost, která může být privátní.</span><span class="sxs-lookup"><span data-stu-id="45e8c-484">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="45e8c-485">Ve skutečnosti, tento kód se pokouší vytvořit relaci některé typ entity s názvem `Entrance` se žádné navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="45e8c-485">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="45e8c-486">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-486">**New behavior**</span></span>

<span data-ttu-id="45e8c-487">Od verze EF Core 3.0, výše uvedený kód nyní dělá co podívali jako její by měl mít byla činnosti před.</span><span class="sxs-lookup"><span data-stu-id="45e8c-487">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="45e8c-488">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-488">**Why**</span></span>

<span data-ttu-id="45e8c-489">Staré chování bylo velmi matoucí, zejména při čtení konfigurace kód a hledání chyb.</span><span class="sxs-lookup"><span data-stu-id="45e8c-489">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="45e8c-490">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-490">**Mitigations**</span></span>

<span data-ttu-id="45e8c-491">Tímto přerušíte pouze aplikace, které jsou explicitně konfigurace relace používání řetězců pro názvy typů a bez explicitním zadáním navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="45e8c-491">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="45e8c-492">Není běžné.</span><span class="sxs-lookup"><span data-stu-id="45e8c-492">This is not common.</span></span>
<span data-ttu-id="45e8c-493">Předchozí chování můžete získat prostřednictvím explicitně předávání `null` pro název navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="45e8c-493">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="45e8c-494">Příklad:</span><span class="sxs-lookup"><span data-stu-id="45e8c-494">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="45e8c-495">Návratový typ pro několik asynchronních metod se změnil z úlohy na ValueTask</span><span class="sxs-lookup"><span data-stu-id="45e8c-495">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="45e8c-496">Sledování problému #15184</span><span class="sxs-lookup"><span data-stu-id="45e8c-496">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="45e8c-497">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="45e8c-497">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="45e8c-498">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-498">**Old behavior**</span></span>

<span data-ttu-id="45e8c-499">Dříve vrátila následující asynchronní metody `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="45e8c-499">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="45e8c-500">`ValueGenerator.NextValueAsync()` (a odvozených tříd)</span><span class="sxs-lookup"><span data-stu-id="45e8c-500">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="45e8c-501">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-501">**New behavior**</span></span>

<span data-ttu-id="45e8c-502">Výše uvedené metody nyní vrací `ValueTask<T>` přes stejné `T` stejně jako předtím.</span><span class="sxs-lookup"><span data-stu-id="45e8c-502">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="45e8c-503">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-503">**Why**</span></span>

<span data-ttu-id="45e8c-504">Tato změna snižuje počet přidělení haldy vzniklé při volání těchto metod, zlepšení obecné informace o výkonu.</span><span class="sxs-lookup"><span data-stu-id="45e8c-504">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="45e8c-505">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-505">**Mitigations**</span></span>

<span data-ttu-id="45e8c-506">Aplikace jednoduše čeká na výše uvedené rozhraní API pouze muset být překompilovány - žádné změny zdroje jsou nezbytné.</span><span class="sxs-lookup"><span data-stu-id="45e8c-506">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="45e8c-507">Složitější využití (například předání vráceného `Task` k `Task.WhenAny()`) obvykle vyžadují, aby vráceného `ValueTask<T>` převedou na hodnoty `Task<T>` voláním `AsTask()` na něm.</span><span class="sxs-lookup"><span data-stu-id="45e8c-507">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="45e8c-508">Všimněte si, že to Neguje snížení přidělení, které tato změna přináší.</span><span class="sxs-lookup"><span data-stu-id="45e8c-508">Note that this negates the allocation reduction that this change brings.</span></span>

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="45e8c-509">Poznámka relační: TypeMapping je teď stejně TypeMapping</span><span class="sxs-lookup"><span data-stu-id="45e8c-509">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="45e8c-510">Sledování problému #9913</span><span class="sxs-lookup"><span data-stu-id="45e8c-510">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="45e8c-511">Tato změna je zavedená v EF Core 3.0 – náhled 2.</span><span class="sxs-lookup"><span data-stu-id="45e8c-511">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="45e8c-512">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-512">**Old behavior**</span></span>

<span data-ttu-id="45e8c-513">Název poznámky pro mapování anotace typu byl "Relační: TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="45e8c-513">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="45e8c-514">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-514">**New behavior**</span></span>

<span data-ttu-id="45e8c-515">Název poznámky pro mapování anotace typu je nyní "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="45e8c-515">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="45e8c-516">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-516">**Why**</span></span>

<span data-ttu-id="45e8c-517">Mapování typu jsou teď používá pro více než jen relační databáze poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="45e8c-517">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="45e8c-518">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-518">**Mitigations**</span></span>

<span data-ttu-id="45e8c-519">Tímto přerušíte jenom aplikace s přístupem k mapování typů přímo jako poznámka, která není běžné.</span><span class="sxs-lookup"><span data-stu-id="45e8c-519">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="45e8c-520">Nejvhodnější opatření na opravu se má používat rovinu rozhraní API pro mapování typů přístupu spíše než přímo pomocí anotace.</span><span class="sxs-lookup"><span data-stu-id="45e8c-520">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="45e8c-521">Vyvolá výjimku, ToTable na odvozený typ.</span><span class="sxs-lookup"><span data-stu-id="45e8c-521">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="45e8c-522">Sledování problému #11811</span><span class="sxs-lookup"><span data-stu-id="45e8c-522">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="45e8c-523">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="45e8c-523">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="45e8c-524">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-524">**Old behavior**</span></span>

<span data-ttu-id="45e8c-525">Před EF Core 3.0 `ToTable()` volalo odvozený typ bude ignorovat, protože pouze dědičnosti mapování strategie byl TPH, kdy to není platný.</span><span class="sxs-lookup"><span data-stu-id="45e8c-525">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="45e8c-526">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-526">**New behavior**</span></span>

<span data-ttu-id="45e8c-527">Spouští se s EF Core 3.0 a při přípravě na přidání TPT a TPC podporují v pozdější verzi `ToTable()` volalo odvozený typ bude nyní vyvolání výjimky v budoucnu vyhnuli o změnu neočekávané mapování.</span><span class="sxs-lookup"><span data-stu-id="45e8c-527">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="45e8c-528">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-528">**Why**</span></span>

<span data-ttu-id="45e8c-529">Aktuálně není platná pro mapování odvozeného typu na jiné tabulky.</span><span class="sxs-lookup"><span data-stu-id="45e8c-529">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="45e8c-530">Tato změna se vyhnete přerušení v budoucnu, kdy bude platná věc udělat.</span><span class="sxs-lookup"><span data-stu-id="45e8c-530">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="45e8c-531">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-531">**Mitigations**</span></span>

<span data-ttu-id="45e8c-532">Odeberte všechny pokusy o mapování odvozené typy s jinými tabulkami.</span><span class="sxs-lookup"><span data-stu-id="45e8c-532">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="45e8c-533">Nahradí HasIndex ForSqlServerHasIndex</span><span class="sxs-lookup"><span data-stu-id="45e8c-533">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="45e8c-534">Sledování problému #12366</span><span class="sxs-lookup"><span data-stu-id="45e8c-534">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="45e8c-535">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="45e8c-535">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="45e8c-536">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-536">**Old behavior**</span></span>

<span data-ttu-id="45e8c-537">Před EF Core 3.0 `ForSqlServerHasIndex().ForSqlServerInclude()` poskytuje způsob, jak konfigurovat sloupců použitých s `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="45e8c-537">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="45e8c-538">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-538">**New behavior**</span></span>

<span data-ttu-id="45e8c-539">Od verze EF Core 3.0, pomocí `Include` na indexu se teď podporuje relační úrovni.</span><span class="sxs-lookup"><span data-stu-id="45e8c-539">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="45e8c-540">Použití `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="45e8c-540">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="45e8c-541">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-541">**Why**</span></span>

<span data-ttu-id="45e8c-542">Tato změna byla provedena konsolidovat rozhraní API pro indexy s `Include` do jednoho umístění pro všechny databáze poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="45e8c-542">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="45e8c-543">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-543">**Mitigations**</span></span>

<span data-ttu-id="45e8c-544">Pomocí nového rozhraní API, jak je znázorněno výše.</span><span class="sxs-lookup"><span data-stu-id="45e8c-544">Use the new API, as shown above.</span></span>

## <a name="metadata-api-changes"></a><span data-ttu-id="45e8c-545">Změn metadat rozhraní API</span><span class="sxs-lookup"><span data-stu-id="45e8c-545">Metadata API changes</span></span>

[<span data-ttu-id="45e8c-546">Sledování problému #214</span><span class="sxs-lookup"><span data-stu-id="45e8c-546">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="45e8c-547">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="45e8c-547">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="45e8c-548">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-548">**New behavior**</span></span>

<span data-ttu-id="45e8c-549">Následující vlastnosti byly převedeny na rozšiřující metody:</span><span class="sxs-lookup"><span data-stu-id="45e8c-549">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="45e8c-550">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-550">**Why**</span></span>

<span data-ttu-id="45e8c-551">Tato změna usnadňuje provádění výše uvedených rozhraní.</span><span class="sxs-lookup"><span data-stu-id="45e8c-551">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="45e8c-552">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-552">**Mitigations**</span></span>

<span data-ttu-id="45e8c-553">Pomocí nové metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="45e8c-553">Use the new extension methods.</span></span>

## <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="45e8c-554">Změny specifickým pro zprostředkovatele metadat rozhraní API</span><span class="sxs-lookup"><span data-stu-id="45e8c-554">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="45e8c-555">Sledování problému #214</span><span class="sxs-lookup"><span data-stu-id="45e8c-555">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="45e8c-556">Tato změna je zavedená v EF Core 3.0-preview 6.</span><span class="sxs-lookup"><span data-stu-id="45e8c-556">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="45e8c-557">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-557">**New behavior**</span></span>

<span data-ttu-id="45e8c-558">Metody rozšíření specifické pro zprostředkovatele bude zjednodušen navýšení kapacity:</span><span class="sxs-lookup"><span data-stu-id="45e8c-558">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.GetSqlServerIsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.ForSqlServerUseIdentityColumn()`

<span data-ttu-id="45e8c-559">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-559">**Why**</span></span>

<span data-ttu-id="45e8c-560">Tato změna usnadňuje provádění metody výše uvedená rozšíření.</span><span class="sxs-lookup"><span data-stu-id="45e8c-560">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="45e8c-561">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-561">**Mitigations**</span></span>

<span data-ttu-id="45e8c-562">Pomocí nové metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="45e8c-562">Use the new extension methods.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="45e8c-563">EF Core už odešle – Direktiva pragma pro vynucení SQLite FK</span><span class="sxs-lookup"><span data-stu-id="45e8c-563">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="45e8c-564">Sledování problému #12151</span><span class="sxs-lookup"><span data-stu-id="45e8c-564">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="45e8c-565">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 3.</span><span class="sxs-lookup"><span data-stu-id="45e8c-565">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="45e8c-566">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-566">**Old behavior**</span></span>

<span data-ttu-id="45e8c-567">Před EF Core 3.0, bude posílat EF Core `PRAGMA foreign_keys = 1` při otevření připojení k SQLite.</span><span class="sxs-lookup"><span data-stu-id="45e8c-567">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="45e8c-568">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-568">**New behavior**</span></span>

<span data-ttu-id="45e8c-569">Od verze EF Core 3.0, už EF Core odešle `PRAGMA foreign_keys = 1` při otevření připojení k SQLite.</span><span class="sxs-lookup"><span data-stu-id="45e8c-569">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="45e8c-570">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-570">**Why**</span></span>

<span data-ttu-id="45e8c-571">Tato změna byla provedena, protože používá EF Core `SQLitePCLRaw.bundle_e_sqlite3` ve výchozím nastavení, která zase znamená, že vynucení cizího klíče je ve výchozím nastavení zapnuté a není potřeba explicitně povolit pokaždé, když je otevřeno připojení.</span><span class="sxs-lookup"><span data-stu-id="45e8c-571">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="45e8c-572">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-572">**Mitigations**</span></span>

<span data-ttu-id="45e8c-573">Cizí klíče jsou povolena ve výchozím nastavení SQLitePCLRaw.bundle_e_sqlite3, který se používá ve výchozím nastavení pro jádro EF Core.</span><span class="sxs-lookup"><span data-stu-id="45e8c-573">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="45e8c-574">Pro jiných případech může být povoleno cizích klíčů tak, že zadáte `Foreign Keys=True` v připojovacím řetězci.</span><span class="sxs-lookup"><span data-stu-id="45e8c-574">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="45e8c-575">Microsoft.EntityFrameworkCore.Sqlite nyní závisí na SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="45e8c-575">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="45e8c-576">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-576">**Old behavior**</span></span>

<span data-ttu-id="45e8c-577">Před EF Core 3.0 používá EF Core `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="45e8c-577">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="45e8c-578">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-578">**New behavior**</span></span>

<span data-ttu-id="45e8c-579">Od verze EF Core 3.0, pomocí EF Core `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="45e8c-579">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="45e8c-580">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-580">**Why**</span></span>

<span data-ttu-id="45e8c-581">Tato změna byla provedena tak, aby používalo verzi SQLite v Iosu konzistentní s jinými platformami.</span><span class="sxs-lookup"><span data-stu-id="45e8c-581">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="45e8c-582">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-582">**Mitigations**</span></span>

<span data-ttu-id="45e8c-583">Pokud chcete použít nativní verzi SQLite v Iosu, nakonfigurovat `Microsoft.Data.Sqlite` použít jinou `SQLitePCLRaw` sady.</span><span class="sxs-lookup"><span data-stu-id="45e8c-583">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

## <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="45e8c-584">Identifikátor GUID hodnoty jsou nyní uloženy jako TEXT na SQLite</span><span class="sxs-lookup"><span data-stu-id="45e8c-584">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="45e8c-585">Sledování problému #15078</span><span class="sxs-lookup"><span data-stu-id="45e8c-585">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="45e8c-586">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="45e8c-586">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="45e8c-587">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-587">**Old behavior**</span></span>

<span data-ttu-id="45e8c-588">Identifikátor GUID hodnoty byly dříve sored jako hodnoty objektu BLOB na SQLite.</span><span class="sxs-lookup"><span data-stu-id="45e8c-588">Guid values were previously sored as BLOB values on SQLite.</span></span>

<span data-ttu-id="45e8c-589">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-589">**New behavior**</span></span>

<span data-ttu-id="45e8c-590">Identifikátor GUID hodnoty jsou nyní uloženy jako TEXT.</span><span class="sxs-lookup"><span data-stu-id="45e8c-590">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="45e8c-591">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-591">**Why**</span></span>

<span data-ttu-id="45e8c-592">Binární formát GUID není standardizované.</span><span class="sxs-lookup"><span data-stu-id="45e8c-592">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="45e8c-593">Uložení hodnot jako TEXT díky databáze více kompatibilní s jinými technologiemi.</span><span class="sxs-lookup"><span data-stu-id="45e8c-593">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="45e8c-594">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-594">**Mitigations**</span></span>

<span data-ttu-id="45e8c-595">Spuštěním SQL takto můžete migrovat existující databáze na nový formát.</span><span class="sxs-lookup"><span data-stu-id="45e8c-595">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="45e8c-596">V EF Core můžete také pokračovat pomocí předchozí chování nakonfigurováním převaděč hodnoty na tyto vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="45e8c-596">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="45e8c-597">Microsoft.Data.Sqlite zůstává schopný načíst hodnoty identifikátoru Guid z objektu BLOB a TEXTOVÉHO sloupce; ale vzhledem k tomu, že došlo ke změně výchozího formátu pro parametry a konstant bude pravděpodobně potřeba provést akci pro většinu scénářů zahrnující identifikátory GUID.</span><span class="sxs-lookup"><span data-stu-id="45e8c-597">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="45e8c-598">Hodnoty char jsou nyní uloženy jako TEXT na SQLite</span><span class="sxs-lookup"><span data-stu-id="45e8c-598">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="45e8c-599">Sledování problému #15020</span><span class="sxs-lookup"><span data-stu-id="45e8c-599">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="45e8c-600">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="45e8c-600">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="45e8c-601">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-601">**Old behavior**</span></span>

<span data-ttu-id="45e8c-602">Hodnoty char byly dříve sored jako CELOČÍSELNÉ hodnoty na SQLite.</span><span class="sxs-lookup"><span data-stu-id="45e8c-602">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="45e8c-603">Například znak hodnotu *A* byl uložen jako celočíselnou hodnotu 65.</span><span class="sxs-lookup"><span data-stu-id="45e8c-603">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="45e8c-604">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-604">**New behavior**</span></span>

<span data-ttu-id="45e8c-605">Hodnoty char jsou nyní uloženy jako TEXT.</span><span class="sxs-lookup"><span data-stu-id="45e8c-605">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="45e8c-606">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-606">**Why**</span></span>

<span data-ttu-id="45e8c-607">Uložení hodnot jako TEXT je přirozenější a vytvoří databáze více kompatibilní s jinými technologiemi.</span><span class="sxs-lookup"><span data-stu-id="45e8c-607">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="45e8c-608">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-608">**Mitigations**</span></span>

<span data-ttu-id="45e8c-609">Spuštěním SQL takto můžete migrovat existující databáze na nový formát.</span><span class="sxs-lookup"><span data-stu-id="45e8c-609">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="45e8c-610">V EF Core můžete také pokračovat pomocí předchozí chování nakonfigurováním převaděč hodnoty na tyto vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="45e8c-610">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="45e8c-611">Microsoft.Data.Sqlite také zbývá schopný načíst znakových hodnot z celé číslo a TEXT sloupců, takže určitých scénářích nevyžadují žádnou akci.</span><span class="sxs-lookup"><span data-stu-id="45e8c-611">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="45e8c-612">ID migrace jsou generovány pomocí neutrální jazykové verze kalendáře</span><span class="sxs-lookup"><span data-stu-id="45e8c-612">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="45e8c-613">Sledování problému #12978</span><span class="sxs-lookup"><span data-stu-id="45e8c-613">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="45e8c-614">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="45e8c-614">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="45e8c-615">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-615">**Old behavior**</span></span>

<span data-ttu-id="45e8c-616">ID migrace neúmyslně byly vygenerovány pomocí kalendář aktuální jazykové verze.</span><span class="sxs-lookup"><span data-stu-id="45e8c-616">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="45e8c-617">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-617">**New behavior**</span></span>

<span data-ttu-id="45e8c-618">ID migrace jsou teď vždy generovány pomocí neutrální jazykové verze kalendáře (gregoriánského).</span><span class="sxs-lookup"><span data-stu-id="45e8c-618">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="45e8c-619">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-619">**Why**</span></span>

<span data-ttu-id="45e8c-620">Pořadí migrace je důležité při aktualizaci databáze nebo řešení konfliktů při sloučení.</span><span class="sxs-lookup"><span data-stu-id="45e8c-620">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="45e8c-621">Pomocí neutrální kalendáře se vyhnete řazení problémy, které můžou být výsledkem členy týmu s jiným kalendáře.</span><span class="sxs-lookup"><span data-stu-id="45e8c-621">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="45e8c-622">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-622">**Mitigations**</span></span>

<span data-ttu-id="45e8c-623">Tato změna ovlivní těm, kdo používají jiných gregoriánském kalendáři, kde rok je větší než gregoriánském kalendáři (např. thajský buddhistický kalendář).</span><span class="sxs-lookup"><span data-stu-id="45e8c-623">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="45e8c-624">Migrace stávající ID bude potřeba aktualizovat tak, aby nové migrace jsou řazeny za stávající migrace.</span><span class="sxs-lookup"><span data-stu-id="45e8c-624">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="45e8c-625">ID migrace najdete v atributu migrace soubory návrháře migrace.</span><span class="sxs-lookup"><span data-stu-id="45e8c-625">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="45e8c-626">Tabulky historie migrace je také potřeba aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="45e8c-626">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

## <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="45e8c-627">Informace o rozšíření nebo metadata byla odebrána z IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="45e8c-627">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="45e8c-628">Sledování problému #16119</span><span class="sxs-lookup"><span data-stu-id="45e8c-628">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="45e8c-629">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 7.</span><span class="sxs-lookup"><span data-stu-id="45e8c-629">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="45e8c-630">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-630">**Old behavior**</span></span>

<span data-ttu-id="45e8c-631">`IDbContextOptionsExtension` obsahuje metody pro získání metadat o rozšíření.</span><span class="sxs-lookup"><span data-stu-id="45e8c-631">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="45e8c-632">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-632">**New behavior**</span></span>

<span data-ttu-id="45e8c-633">Tyto metody byly přesunuty do nové `DbContextOptionsExtensionInfo` abstraktní základní třída, která je vrácena z nového `IDbContextOptionsExtension.Info` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="45e8c-633">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="45e8c-634">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-634">**Why**</span></span>

<span data-ttu-id="45e8c-635">Nad verzí z verze 2.0, 3.0, potřebujeme přidat nebo změnit tyto metody několikrát.</span><span class="sxs-lookup"><span data-stu-id="45e8c-635">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="45e8c-636">Do nové abstraktní základní třída je zásadní navýšení kapacity vám usnadní vytvořit tento druh změny bez narušení existující rozšíření.</span><span class="sxs-lookup"><span data-stu-id="45e8c-636">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="45e8c-637">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-637">**Mitigations**</span></span>

<span data-ttu-id="45e8c-638">Aktualizujte rozšíření mají nový tvar.</span><span class="sxs-lookup"><span data-stu-id="45e8c-638">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="45e8c-639">Příklady jsou součástí různými implementacemi tohoto `IDbContextOptionsExtension` pro různé typy rozšíření v EF Core zdrojový kód.</span><span class="sxs-lookup"><span data-stu-id="45e8c-639">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

## <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="45e8c-640">LogQueryPossibleExceptionWithAggregateOperator byl přejmenován.</span><span class="sxs-lookup"><span data-stu-id="45e8c-640">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="45e8c-641">Sledování problému #10985</span><span class="sxs-lookup"><span data-stu-id="45e8c-641">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="45e8c-642">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="45e8c-642">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="45e8c-643">**Změna**</span><span class="sxs-lookup"><span data-stu-id="45e8c-643">**Change**</span></span>

<span data-ttu-id="45e8c-644">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` byl přejmenován na `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="45e8c-644">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="45e8c-645">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-645">**Why**</span></span>

<span data-ttu-id="45e8c-646">Zarovná pojmenování Tato událost upozornění s jinými událostmi upozornění.</span><span class="sxs-lookup"><span data-stu-id="45e8c-646">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="45e8c-647">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-647">**Mitigations**</span></span>

<span data-ttu-id="45e8c-648">Použití nového názvu.</span><span class="sxs-lookup"><span data-stu-id="45e8c-648">Use the new name.</span></span> <span data-ttu-id="45e8c-649">(Všimněte si, že nedošlo ke změně číslo ID události.)</span><span class="sxs-lookup"><span data-stu-id="45e8c-649">(Note that the event ID number has not changed.)</span></span>

## <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="45e8c-650">Vysvětlení rozhraní API pro názvy omezení pro cizí klíč</span><span class="sxs-lookup"><span data-stu-id="45e8c-650">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="45e8c-651">Sledování problému #10730</span><span class="sxs-lookup"><span data-stu-id="45e8c-651">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="45e8c-652">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="45e8c-652">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="45e8c-653">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-653">**Old behavior**</span></span>

<span data-ttu-id="45e8c-654">Před EF Core 3.0 omezení pro cizí klíč názvy označovaly jako jednoduše "name".</span><span class="sxs-lookup"><span data-stu-id="45e8c-654">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="45e8c-655">Příklad:</span><span class="sxs-lookup"><span data-stu-id="45e8c-655">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="45e8c-656">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-656">**New behavior**</span></span>

<span data-ttu-id="45e8c-657">Od verze EF Core 3.0, omezení pro cizí klíč názvy jsou dnes označovány jako "název omezení".</span><span class="sxs-lookup"><span data-stu-id="45e8c-657">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="45e8c-658">Příklad:</span><span class="sxs-lookup"><span data-stu-id="45e8c-658">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="45e8c-659">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-659">**Why**</span></span>

<span data-ttu-id="45e8c-660">Tato změna přináší konzistenci pro názvy v této oblasti a také vysvětluje, že se jedná o název název cizího klíče omezení a nikoli na sloupec nebo vlastnost, která je definována cizího klíče na.</span><span class="sxs-lookup"><span data-stu-id="45e8c-660">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="45e8c-661">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-661">**Mitigations**</span></span>

<span data-ttu-id="45e8c-662">Použití nového názvu.</span><span class="sxs-lookup"><span data-stu-id="45e8c-662">Use the new name.</span></span>

## <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="45e8c-663">IRelationalDatabaseCreator.HasTables/HasTablesAsync byly provedeny veřejné</span><span class="sxs-lookup"><span data-stu-id="45e8c-663">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="45e8c-664">Sledování problému #15997</span><span class="sxs-lookup"><span data-stu-id="45e8c-664">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="45e8c-665">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 7.</span><span class="sxs-lookup"><span data-stu-id="45e8c-665">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="45e8c-666">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-666">**Old behavior**</span></span>

<span data-ttu-id="45e8c-667">Před EF Core 3.0 byly chráněné tyto metody.</span><span class="sxs-lookup"><span data-stu-id="45e8c-667">Before EF Core 3.0, these methods were protected.</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="45e8c-668">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-668">**New behavior**</span></span>

<span data-ttu-id="45e8c-669">Od verze EF Core 3.0, tyto metody jsou veřejné.</span><span class="sxs-lookup"><span data-stu-id="45e8c-669">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="45e8c-670">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-670">**Why**</span></span>

<span data-ttu-id="45e8c-671">Tyto metody jsou používány EF k určení, zda je databáze vytvořená, ale prázdný.</span><span class="sxs-lookup"><span data-stu-id="45e8c-671">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="45e8c-672">To může také být užitečné z mimo EF při určování, jestli se mají použít migrace.</span><span class="sxs-lookup"><span data-stu-id="45e8c-672">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="45e8c-673">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-673">**Mitigations**</span></span>

<span data-ttu-id="45e8c-674">Změňte přístupnost nějaká přepsání.</span><span class="sxs-lookup"><span data-stu-id="45e8c-674">Change the accessibility of any overrides.</span></span>

## <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="45e8c-675">Microsoft.EntityFrameworkCore.Design je nyní DevelopmentDependency balíček</span><span class="sxs-lookup"><span data-stu-id="45e8c-675">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="45e8c-676">Sledování problému #11506</span><span class="sxs-lookup"><span data-stu-id="45e8c-676">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="45e8c-677">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 4.</span><span class="sxs-lookup"><span data-stu-id="45e8c-677">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="45e8c-678">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-678">**Old behavior**</span></span>

<span data-ttu-id="45e8c-679">Před EF Core 3.0 byl Microsoft.EntityFrameworkCore.Design regulární balíčku NuGet, jejichž sestavení mohou být odkazovány podle projektů, které na ní závisí.</span><span class="sxs-lookup"><span data-stu-id="45e8c-679">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="45e8c-680">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-680">**New behavior**</span></span>

<span data-ttu-id="45e8c-681">Od verze EF Core 3.0, jedná se o DevelopmentDependency balíček.</span><span class="sxs-lookup"><span data-stu-id="45e8c-681">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="45e8c-682">To znamená, závislost nebude přechodně tok do jiné projekty a že již nejsou ve výchozím nastavení, můžete odkazovat na jeho sestavení.</span><span class="sxs-lookup"><span data-stu-id="45e8c-682">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="45e8c-683">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-683">**Why**</span></span>

<span data-ttu-id="45e8c-684">Tento balíček je určena pouze pro použití v době návrhu.</span><span class="sxs-lookup"><span data-stu-id="45e8c-684">This package is only intended to be used at design time.</span></span> <span data-ttu-id="45e8c-685">Nasazené aplikace by neměl odkazovat.</span><span class="sxs-lookup"><span data-stu-id="45e8c-685">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="45e8c-686">Vytváření balíčku DevelopmentDependency posiluje toto doporučení.</span><span class="sxs-lookup"><span data-stu-id="45e8c-686">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="45e8c-687">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-687">**Mitigations**</span></span>

<span data-ttu-id="45e8c-688">Pokud potřebujete odkazovat na tento balíček do EF Core návrhu chování přepsat, můžete aktualizovat aktualizace PackageReference položky metadat ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="45e8c-688">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="45e8c-689">Pokud balíček je se odkazovalo přechodně prostřednictvím Microsoft.EntityFrameworkCore.Tools, je potřeba přidat explicitní PackageReference pro balíček, který má změnit jeho metadata.</span><span class="sxs-lookup"><span data-stu-id="45e8c-689">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0-preview4.19216.3">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

## <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="45e8c-690">SQLitePCL.raw aktualizována na verzi 2.0.0</span><span class="sxs-lookup"><span data-stu-id="45e8c-690">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="45e8c-691">Sledování problému #14824</span><span class="sxs-lookup"><span data-stu-id="45e8c-691">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="45e8c-692">Tato změna je zavedená v EF Core 3.0 – ve verzi preview 7.</span><span class="sxs-lookup"><span data-stu-id="45e8c-692">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="45e8c-693">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-693">**Old behavior**</span></span>

<span data-ttu-id="45e8c-694">Microsoft.EntityFrameworkCore.Sqlite dříve závisí na verzi 1.1.12 SQLitePCL.raw.</span><span class="sxs-lookup"><span data-stu-id="45e8c-694">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="45e8c-695">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="45e8c-695">**New behavior**</span></span>

<span data-ttu-id="45e8c-696">Aktualizujeme naše balíček závisí na verze 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="45e8c-696">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="45e8c-697">**Proč**</span><span class="sxs-lookup"><span data-stu-id="45e8c-697">**Why**</span></span>

<span data-ttu-id="45e8c-698">Verze 2.0.0 SQLitePCL.raw cílí na .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="45e8c-698">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="45e8c-699">Dříve cílený .NET Standard 1.1, který vyžaduje velké uzavření tranzitivní balíčky pro práci.</span><span class="sxs-lookup"><span data-stu-id="45e8c-699">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="45e8c-700">**Zmírnění rizik**</span><span class="sxs-lookup"><span data-stu-id="45e8c-700">**Mitigations**</span></span>

<span data-ttu-id="45e8c-701">Zahrnuje SQLitePCL.raw verze 2.0.0 odstraňuje některé narušující změny.</span><span class="sxs-lookup"><span data-stu-id="45e8c-701">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="45e8c-702">Zobrazit [poznámky k verzi](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="45e8c-702">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>
