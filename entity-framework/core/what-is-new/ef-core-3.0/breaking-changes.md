---
title: Přerušující změny v EF Core 3,0 – EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: c73663412efcd93c04892f193d4f5a2485724e22
ms.sourcegitcommit: 755a15a789631cc4ea581e2262a2dcc49c219eef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2019
ms.locfileid: "68497532"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="bf8ed-102">Přerušující změny zahrnuté v EF Core 3,0 (aktuálně ve verzi Preview)</span><span class="sxs-lookup"><span data-stu-id="bf8ed-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bf8ed-103">Počítejte s tím, že sady funkcí a plány budoucích verzí se vždycky mění, a i když se pokusíme tuto stránku uchovávat v aktuálním stavu, nemusí se po celou dobu projevit naše nejnovější plány.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="bf8ed-104">Následující změny rozhraní API a chování mají možnost rušit aplikace vyvinuté pro EF Core 2.2. x při jejich upgradu na 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="bf8ed-105">Změny, které očekáváme jenom o to, aby ovlivnili pouze poskytovatele databází, jsou popsané v části [změny zprostředkovatele](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="bf8ed-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="bf8ed-106">Přerušení nových funkcí zavedených z jedné verze 3,0 Preview do jiné 3,0 verze Preview zde nejsou dokumentovány.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="summary"></a><span data-ttu-id="bf8ed-107">Souhrn</span><span class="sxs-lookup"><span data-stu-id="bf8ed-107">Summary</span></span>

| <span data-ttu-id="bf8ed-108">**Zásadní změna**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-108">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="bf8ed-109">**Dopad**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-109">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="bf8ed-110">Dotazy LINQ již nejsou vyhodnocovány na klientovi.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-110">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="bf8ed-111">Vysoká</span><span class="sxs-lookup"><span data-stu-id="bf8ed-111">High</span></span>       |
| [<span data-ttu-id="bf8ed-112">EF Core nástroj příkazového řádku dotnet EF již není součástí .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="bf8ed-112">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="bf8ed-113">Vysoká</span><span class="sxs-lookup"><span data-stu-id="bf8ed-113">High</span></span>      |
| [<span data-ttu-id="bf8ed-114">Z tabulek, ExecuteSql a ExecuteSqlAsync byly přejmenovány.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-114">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="bf8ed-115">Vysoká</span><span class="sxs-lookup"><span data-stu-id="bf8ed-115">High</span></span>      |
| [<span data-ttu-id="bf8ed-116">Typy dotazů jsou konsolidovány s typy entit</span><span class="sxs-lookup"><span data-stu-id="bf8ed-116">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="bf8ed-117">Vysoká</span><span class="sxs-lookup"><span data-stu-id="bf8ed-117">High</span></span>      |
| [<span data-ttu-id="bf8ed-118">Entity Framework Core už není součástí sdílené ASP.NET Core architektury.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-118">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="bf8ed-119">Střední</span><span class="sxs-lookup"><span data-stu-id="bf8ed-119">Medium</span></span>      |
| [<span data-ttu-id="bf8ed-120">Odstranění kaskádových operací se teď ve výchozím nastavení provádí hned.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-120">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="bf8ed-121">Střední</span><span class="sxs-lookup"><span data-stu-id="bf8ed-121">Medium</span></span>      |
| [<span data-ttu-id="bf8ed-122">DeleteBehavior. restrict má sémantiku čištění.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-122">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="bf8ed-123">Střední</span><span class="sxs-lookup"><span data-stu-id="bf8ed-123">Medium</span></span>      |
| [<span data-ttu-id="bf8ed-124">Změnilo se konfigurační rozhraní API pro vztahy vlastněných typů.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-124">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="bf8ed-125">Střední</span><span class="sxs-lookup"><span data-stu-id="bf8ed-125">Medium</span></span>      |
| [<span data-ttu-id="bf8ed-126">Každá vlastnost používá nezávislou generaci celočíselného klíče v paměti.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-126">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="bf8ed-127">Střední</span><span class="sxs-lookup"><span data-stu-id="bf8ed-127">Medium</span></span>      |
| [<span data-ttu-id="bf8ed-128">Změny rozhraní API pro metadata</span><span class="sxs-lookup"><span data-stu-id="bf8ed-128">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="bf8ed-129">Střední</span><span class="sxs-lookup"><span data-stu-id="bf8ed-129">Medium</span></span>      |
| [<span data-ttu-id="bf8ed-130">Změny rozhraní API pro konkrétního zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="bf8ed-130">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="bf8ed-131">Střední</span><span class="sxs-lookup"><span data-stu-id="bf8ed-131">Medium</span></span>      |
| [<span data-ttu-id="bf8ed-132">UseRowNumberForPaging se odebral.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-132">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="bf8ed-133">Střední</span><span class="sxs-lookup"><span data-stu-id="bf8ed-133">Medium</span></span>      |
| [<span data-ttu-id="bf8ed-134">Metody Z tabulek se dají zadat jenom v kořenech dotazů.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-134">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="bf8ed-135">Nízká</span><span class="sxs-lookup"><span data-stu-id="bf8ed-135">Low</span></span>      |
| [<span data-ttu-id="bf8ed-136">~~Provádění dotazu se protokoluje na úrovni ladění~~ . Vrátit</span><span class="sxs-lookup"><span data-stu-id="bf8ed-136">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="bf8ed-137">Nízká</span><span class="sxs-lookup"><span data-stu-id="bf8ed-137">Low</span></span>      |
| [<span data-ttu-id="bf8ed-138">Dočasné hodnoty klíčů už nejsou nastavené na instance entit.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-138">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="bf8ed-139">Nízká</span><span class="sxs-lookup"><span data-stu-id="bf8ed-139">Low</span></span>      |
| [<span data-ttu-id="bf8ed-140">DetectChanges respektuje hodnoty klíčů generované úložištěm.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-140">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="bf8ed-141">Nízká</span><span class="sxs-lookup"><span data-stu-id="bf8ed-141">Low</span></span>      |
| [<span data-ttu-id="bf8ed-142">Závislé entity, které sdílí tabulku s objektem zabezpečení, jsou teď volitelné.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-142">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="bf8ed-143">Nízká</span><span class="sxs-lookup"><span data-stu-id="bf8ed-143">Low</span></span>      |
| [<span data-ttu-id="bf8ed-144">Všechny entity sdílející tabulku se sloupcem souběžného tokenu musí být namapovány na vlastnost.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-144">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="bf8ed-145">Nízká</span><span class="sxs-lookup"><span data-stu-id="bf8ed-145">Low</span></span>      |
| [<span data-ttu-id="bf8ed-146">Zděděné vlastnosti z nemapovaných typů jsou nyní namapovány na jeden sloupec pro všechny odvozené typy.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-146">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="bf8ed-147">Nízká</span><span class="sxs-lookup"><span data-stu-id="bf8ed-147">Low</span></span>      |
| [<span data-ttu-id="bf8ed-148">Konvence vlastností cizího klíče už neodpovídá stejnému názvu jako vlastnost Principal.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-148">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="bf8ed-149">Nízká</span><span class="sxs-lookup"><span data-stu-id="bf8ed-149">Low</span></span>      |
| [<span data-ttu-id="bf8ed-150">Připojení k databázi je teď zavřené, pokud už nepoužíváte dřív, než se dokončí jeho objekt TransactionScope.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-150">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="bf8ed-151">Nízká</span><span class="sxs-lookup"><span data-stu-id="bf8ed-151">Low</span></span>      |
| [<span data-ttu-id="bf8ed-152">Ve výchozím nastavení se používají pole pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-152">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="bf8ed-153">Nízká</span><span class="sxs-lookup"><span data-stu-id="bf8ed-153">Low</span></span>      |
| [<span data-ttu-id="bf8ed-154">Vyvolat, zda je nalezeno více kompatibilních zálohovaných polí</span><span class="sxs-lookup"><span data-stu-id="bf8ed-154">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="bf8ed-155">Nízká</span><span class="sxs-lookup"><span data-stu-id="bf8ed-155">Low</span></span>      |
| [<span data-ttu-id="bf8ed-156">Názvy vlastností pouze pro pole se musí shodovat s názvem pole.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-156">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="bf8ed-157">Nízká</span><span class="sxs-lookup"><span data-stu-id="bf8ed-157">Low</span></span>      |
| [<span data-ttu-id="bf8ed-158">AddDbContext/AddDbContextPool už nevolá AddLogging a AddMemoryCache.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-158">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="bf8ed-159">Nízká</span><span class="sxs-lookup"><span data-stu-id="bf8ed-159">Low</span></span>      |
| [<span data-ttu-id="bf8ed-160">DbContext. entry teď provádí místní DetectChanges.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-160">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="bf8ed-161">Nízká</span><span class="sxs-lookup"><span data-stu-id="bf8ed-161">Low</span></span>      |
| [<span data-ttu-id="bf8ed-162">Klíče řetězce a pole bajtů nejsou ve výchozím nastavení generovány klientem.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-162">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="bf8ed-163">Nízká</span><span class="sxs-lookup"><span data-stu-id="bf8ed-163">Low</span></span>      |
| [<span data-ttu-id="bf8ed-164">ILoggerFactory je teď služba s vymezeným oborem.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-164">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="bf8ed-165">Nízká</span><span class="sxs-lookup"><span data-stu-id="bf8ed-165">Low</span></span>      |
| [<span data-ttu-id="bf8ed-166">Opožděné načítání proxy serverů už nepředpokládá navigační vlastnosti, jsou plně načtené.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-166">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="bf8ed-167">Nízká</span><span class="sxs-lookup"><span data-stu-id="bf8ed-167">Low</span></span>      |
| [<span data-ttu-id="bf8ed-168">Nadměrné vytváření interních zprostředkovatelů služeb je teď ve výchozím nastavení chyba.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-168">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="bf8ed-169">Nízká</span><span class="sxs-lookup"><span data-stu-id="bf8ed-169">Low</span></span>      |
| [<span data-ttu-id="bf8ed-170">Nové chování pro HasOne/HasMany se volá s jedním řetězcem.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-170">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="bf8ed-171">Nízká</span><span class="sxs-lookup"><span data-stu-id="bf8ed-171">Low</span></span>      |
| [<span data-ttu-id="bf8ed-172">Návratový typ pro několik asynchronních metod byl změněn z úlohy na ValueTask</span><span class="sxs-lookup"><span data-stu-id="bf8ed-172">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="bf8ed-173">Nízká</span><span class="sxs-lookup"><span data-stu-id="bf8ed-173">Low</span></span>      |
| [<span data-ttu-id="bf8ed-174">Relační: anotace TypeMapping je nyní pouze TypeMapping</span><span class="sxs-lookup"><span data-stu-id="bf8ed-174">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="bf8ed-175">Nízká</span><span class="sxs-lookup"><span data-stu-id="bf8ed-175">Low</span></span>      |
| [<span data-ttu-id="bf8ed-176">ToTable na odvozeném typu vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-176">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="bf8ed-177">Nízká</span><span class="sxs-lookup"><span data-stu-id="bf8ed-177">Low</span></span>      |
| [<span data-ttu-id="bf8ed-178">EF Core už neposílá direktivu pragma pro vynucení KOFK SQLite.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-178">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="bf8ed-179">Nízká</span><span class="sxs-lookup"><span data-stu-id="bf8ed-179">Low</span></span>      |
| [<span data-ttu-id="bf8ed-180">Microsoft. EntityFrameworkCore. sqlite teď závisí na SQLitePCLRaw. bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="bf8ed-180">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="bf8ed-181">Nízká</span><span class="sxs-lookup"><span data-stu-id="bf8ed-181">Low</span></span>      |
| [<span data-ttu-id="bf8ed-182">Hodnoty GUID se teď ukládají jako TEXT na SQLite.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-182">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="bf8ed-183">Nízká</span><span class="sxs-lookup"><span data-stu-id="bf8ed-183">Low</span></span>      |
| [<span data-ttu-id="bf8ed-184">Hodnoty char se teď ukládají jako TEXT na SQLite.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-184">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="bf8ed-185">Nízká</span><span class="sxs-lookup"><span data-stu-id="bf8ed-185">Low</span></span>      |
| [<span data-ttu-id="bf8ed-186">ID migrace se teď generují pomocí kalendáře invariantní jazykové verze.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-186">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="bf8ed-187">Nízká</span><span class="sxs-lookup"><span data-stu-id="bf8ed-187">Low</span></span>      |
| [<span data-ttu-id="bf8ed-188">Informace o rozšíření/metadata se odebraly z IDbContextOptionsExtension.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-188">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="bf8ed-189">Nízká</span><span class="sxs-lookup"><span data-stu-id="bf8ed-189">Low</span></span>      |
| [<span data-ttu-id="bf8ed-190">LogQueryPossibleExceptionWithAggregateOperator se přejmenovalo.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-190">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="bf8ed-191">Nízká</span><span class="sxs-lookup"><span data-stu-id="bf8ed-191">Low</span></span>      |
| [<span data-ttu-id="bf8ed-192">Vysvětlení rozhraní API pro názvy omezení cizího klíče</span><span class="sxs-lookup"><span data-stu-id="bf8ed-192">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="bf8ed-193">Nízká</span><span class="sxs-lookup"><span data-stu-id="bf8ed-193">Low</span></span>      |
| [<span data-ttu-id="bf8ed-194">IRelationalDatabaseCreator. HasTables/HasTablesAsync byly zveřejněny.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-194">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="bf8ed-195">Nízká</span><span class="sxs-lookup"><span data-stu-id="bf8ed-195">Low</span></span>      |
| [<span data-ttu-id="bf8ed-196">Microsoft. EntityFrameworkCore. Design je teď balíček DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-196">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="bf8ed-197">Nízká</span><span class="sxs-lookup"><span data-stu-id="bf8ed-197">Low</span></span>      |
| [<span data-ttu-id="bf8ed-198">SQLitePCL. Raw aktualizováno na verzi 2.0.0</span><span class="sxs-lookup"><span data-stu-id="bf8ed-198">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="bf8ed-199">Nízká</span><span class="sxs-lookup"><span data-stu-id="bf8ed-199">Low</span></span>      |
| [<span data-ttu-id="bf8ed-200">NetTopologySuite aktualizace na verzi 2.0.0</span><span class="sxs-lookup"><span data-stu-id="bf8ed-200">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="bf8ed-201">Nízká</span><span class="sxs-lookup"><span data-stu-id="bf8ed-201">Low</span></span>      |
| [<span data-ttu-id="bf8ed-202">Je nutné nakonfigurovat více dvojznačných relací odkazujících na sebe.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-202">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="bf8ed-203">Nízká</span><span class="sxs-lookup"><span data-stu-id="bf8ed-203">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="bf8ed-204">Dotazy LINQ již nejsou vyhodnocovány na klientovi.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-204">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="bf8ed-205">[Sledování problému #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[taky zobrazit problémy #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="bf8ed-205">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="bf8ed-206">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-206">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="bf8ed-207">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-207">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-208">Před 3,0, když EF Core nedokázal převést výraz, který byl součástí dotazu na buď SQL, nebo parametr, automaticky vyhodnotí výraz na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-208">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="bf8ed-209">Ve výchozím nastavení vyhodnocování klientů u potenciálně nákladných výrazů aktivovalo pouze upozornění.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-209">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="bf8ed-210">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-210">**New behavior**</span></span>

<span data-ttu-id="bf8ed-211">Počínaje 3,0 EF Core v rámci projekce nejvyšší úrovně (posledního `Select()` volání v dotazu) jenom výrazy vyhodnotit na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-211">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="bf8ed-212">Pokud výrazy v jakékoli jiné části dotazu nelze převést na buď SQL, nebo parametr, je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-212">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="bf8ed-213">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-213">**Why**</span></span>

<span data-ttu-id="bf8ed-214">Automatické hodnocení dotazů umožňuje provádět mnoho dotazů i v případě, že není možné přeložit jejich důležité části.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-214">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="bf8ed-215">Toto chování může vést k neočekávanému a potenciálně škodlivému chování, které může být jen zřejmé v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-215">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="bf8ed-216">Například podmínka ve `Where()` volání, která se nedá přeložit, může způsobit přenesení všech řádků z databázového serveru a filtr, který se má použít na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-216">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="bf8ed-217">Tato situace může snadno přejít zpět, pokud tabulka obsahuje jenom několik řádků ve vývoji, ale při přesunu aplikace do produkčního prostředí zasáhnout, kde tabulka může obsahovat miliony řádků.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-217">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="bf8ed-218">Upozornění na vyhodnocení klientů se také během vývoje ukázala jako příliš jednoduchá.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-218">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="bf8ed-219">Kromě toho může automatické hodnocení klienta vést k problémům s tím, že vylepšení překladu dotazů pro konkrétní výrazy způsobilo nezamýšlené neúmyslné změny mezi verzemi.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-219">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="bf8ed-220">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-220">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-221">Pokud dotaz nelze plně přeložit, pak buď Přepište dotaz do formuláře, který lze přeložit, nebo použijte `AsEnumerable()`, `ToList()`nebo podobným způsobem explicitně přeneste data zpět do klienta, kde lze následně dále zpracovávat pomocí LINQ-to-Objects.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-221">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="bf8ed-222">Entity Framework Core už není součástí sdílené ASP.NET Core architektury.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-222">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="bf8ed-223">Sledování oznámení o problémech # 325</span><span class="sxs-lookup"><span data-stu-id="bf8ed-223">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="bf8ed-224">Tato změna je představena ve verzi ASP.NET Core 3,0-Preview 1.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-224">This change is introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="bf8ed-225">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-225">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-226">Před ASP.NET Core 3,0 se při přidání odkazu na balíček do `Microsoft.AspNetCore.App` nebo `Microsoft.AspNetCore.All`zahrnovala EF Core a někteří poskytovatelé EF Core dat jako poskytovatel SQL Server.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-226">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="bf8ed-227">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-227">**New behavior**</span></span>

<span data-ttu-id="bf8ed-228">Počínaje 3,0 ASP.NET Core sdílené rozhraní nezahrnuje EF Core nebo žádné poskytovatele EF Core dat.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-228">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="bf8ed-229">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-229">**Why**</span></span>

<span data-ttu-id="bf8ed-230">Před touto změnou EF Core nutné jiné kroky v závislosti na tom, zda je aplikace cílena ASP.NET Core a SQL Server.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-230">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="bf8ed-231">Upgrade ASP.NET Core taky vynutil upgrade EF Core a poskytovatele SQL Server, který není vždycky žádoucí.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-231">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="bf8ed-232">V důsledku této změny je prostředí získání EF Core stejné u všech poskytovatelů, podporovaných implementací rozhraní .NET a typů aplikací.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-232">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="bf8ed-233">Vývojáři teď můžou přesně řídit, když EF Core a EF Core se upgradují poskytovatelé dat.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-233">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="bf8ed-234">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-234">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-235">Chcete-li použít EF Core v aplikaci ASP.NET Core 3,0 nebo v jakékoli jiné podporované aplikaci, explicitně přidejte odkaz na balíček EF Coreho poskytovatele databáze, který bude aplikace používat.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-235">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="bf8ed-236">EF Core nástroj příkazového řádku dotnet EF již není součástí .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="bf8ed-236">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="bf8ed-237">Sledování problému #14016</span><span class="sxs-lookup"><span data-stu-id="bf8ed-237">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="bf8ed-238">Tato změna je představena v EF Core 3,0-Preview 4 a odpovídající verzi .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-238">This change is introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="bf8ed-239">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-239">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-240">Před 3,0 `dotnet ef` byl nástroj součástí .NET Core SDK a byl snadno dostupný pro použití z příkazového řádku z libovolného projektu bez nutnosti dalších kroků.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-240">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="bf8ed-241">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-241">**New behavior**</span></span>

<span data-ttu-id="bf8ed-242">Počínaje 3,0 sada .NET SDK nezahrnuje `dotnet ef` nástroj, takže před tím, než ho budete moct použít, je nutné explicitně nainstalovat jako místní nebo globální nástroj.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-242">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="bf8ed-243">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-243">**Why**</span></span>

<span data-ttu-id="bf8ed-244">Tato změna nám umožňuje distribuovat a aktualizovat `dotnet ef` jako běžný nástroj rozhraní .NET CLI v NuGet, a to v souladu se skutečností, že EF Core 3,0 je také vždy distribuován jako balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-244">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="bf8ed-245">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-245">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-246">Aby bylo možné spravovat migrace nebo uživatelské rozhraní a `DbContext`, nainstalujte `dotnet-ef` nástroj jako globální nástroj:</span><span class="sxs-lookup"><span data-stu-id="bf8ed-246">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef --version 3.0.0-*
  ```

<span data-ttu-id="bf8ed-247">Můžete také získat místní nástroj při obnovování závislostí projektu, který deklaruje jako závislost nástrojů pomocí [souboru manifestu nástroje](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="bf8ed-247">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="bf8ed-248">Z tabulek, ExecuteSql a ExecuteSqlAsync byly přejmenovány.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-248">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="bf8ed-249">Sledování problému #10996</span><span class="sxs-lookup"><span data-stu-id="bf8ed-249">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="bf8ed-250">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-250">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="bf8ed-251">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-251">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-252">Před EF Core 3,0 byly tyto názvy metod přetíženy, aby fungovaly s normálním řetězcem nebo řetězcem, které by měly být interpolované na SQL a parametry.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-252">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="bf8ed-253">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-253">**New behavior**</span></span>

<span data-ttu-id="bf8ed-254">Počínaje EF Core 3,0, použijte `FromSqlRaw`, `ExecuteSqlRaw`a `ExecuteSqlRawAsync` k vytvoření parametrizovaného dotazu, kde jsou parametry předány nezávisle na řetězci dotazu.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-254">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="bf8ed-255">Příklad:</span><span class="sxs-lookup"><span data-stu-id="bf8ed-255">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="bf8ed-256">Použijte `FromSqlInterpolated`, `ExecuteSqlInterpolated` a`ExecuteSqlInterpolatedAsync` k vytvoření parametrizovaného dotazu, kde jsou parametry předány jako součást interpolované řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-256">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="bf8ed-257">Příklad:</span><span class="sxs-lookup"><span data-stu-id="bf8ed-257">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="bf8ed-258">Všimněte si, že obě výše uvedené dotazy vytvoří stejný parametrizovaný SQL se stejnými parametry SQL.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-258">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="bf8ed-259">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-259">**Why**</span></span>

<span data-ttu-id="bf8ed-260">Přetížení metody, jako to, usnadňuje náhodné volání nezpracované řetězcové metody, pokud by záměr byl zavolat interpolovaná řetězcová metoda a druhá možnost kolem.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-260">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="bf8ed-261">To může vést k tomu, že dotazy nejsou parametrizované, pokud by měly být.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-261">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="bf8ed-262">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-262">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-263">Přepněte na použití nových názvů metod.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-263">Switch to use the new method names.</span></span>

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="bf8ed-264">Metody Z tabulek se dají zadat jenom v kořenech dotazů.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-264">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="bf8ed-265">Sledování problému #15704</span><span class="sxs-lookup"><span data-stu-id="bf8ed-265">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="bf8ed-266">Tato změna je představena ve verzi EF Core 3,0-Preview 6.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-266">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="bf8ed-267">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-267">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-268">Před EF Core 3,0 `FromSql` lze metodu zadat kdekoli v dotazu.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-268">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="bf8ed-269">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-269">**New behavior**</span></span>

<span data-ttu-id="bf8ed-270">Počínaje EF Core `FromSqlRaw` 3,0 lze nové metody a `FromSqlInterpolated` (které nahradí `FromSql`) zadat pouze v kořenech `DbSet<>`dotazu, tj. přímo na.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-270">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="bf8ed-271">Pokud se pokusíte zadat jiné místo jinak, dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-271">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="bf8ed-272">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-272">**Why**</span></span>

<span data-ttu-id="bf8ed-273">Určení `FromSql` kdekoli jinde než u a `DbSet` neobsahovalo žádné přidané významy nebo přidání hodnoty a může v některých scénářích způsobit nejednoznačnost.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-273">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="bf8ed-274">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-274">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-275">`FromSql`volání by se měla přesunout přímo na, `DbSet` na které se vztahují.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-275">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="bf8ed-276">~~Provádění dotazu se protokoluje na úrovni ladění~~ . Vrátit</span><span class="sxs-lookup"><span data-stu-id="bf8ed-276">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="bf8ed-277">Sledování problému #14523</span><span class="sxs-lookup"><span data-stu-id="bf8ed-277">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="bf8ed-278">Tato změna se vrátí v EF Core 3,0-Preview 7.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-278">This change is reverted in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="bf8ed-279">Tuto změnu jsme vrátili, protože nová konfigurace v EF Core 3,0 umožňuje, aby byla úroveň protokolu pro libovolnou událost specifikována aplikací.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-279">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="bf8ed-280">Například chcete-li přepnout protokolování SQL na `Debug`, explicitně nakonfigurovat úroveň v `OnConfiguring` nebo `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="bf8ed-280">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="bf8ed-281">Dočasné hodnoty klíčů už nejsou nastavené na instance entit.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-281">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="bf8ed-282">Sledování problému #12378</span><span class="sxs-lookup"><span data-stu-id="bf8ed-282">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="bf8ed-283">Tato změna je představena ve verzi EF Core 3,0-Preview 2.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-283">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="bf8ed-284">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-284">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-285">Před EF Core 3,0 byly přiřazeny dočasné hodnoty ke všem klíčovým vlastnostem, které by později měly skutečnou hodnotu vygenerovanou databází.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-285">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="bf8ed-286">Obvykle jsou tyto dočasné hodnoty velkými zápornými čísly.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-286">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="bf8ed-287">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-287">**New behavior**</span></span>

<span data-ttu-id="bf8ed-288">Počínaje 3,0 EF Core ukládá hodnotu dočasného klíče jako součást informací o sledování entity a ponechá vlastnost klíče beze změny.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-288">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="bf8ed-289">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-289">**Why**</span></span>

<span data-ttu-id="bf8ed-290">Tato změna byla provedena, aby se předešlo omylům hodnotám klíčů v případě, že entita, která byla dříve sledována pomocí nějaké `DbContext` instance, je přesunuta do jiné `DbContext` instance.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-290">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="bf8ed-291">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-291">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-292">Aplikace, které přiřazují hodnoty primárního klíče k cizím klíčům k vytvoření přidružení mezi entitami, můžou záviset na starém chování, pokud jsou primární klíče generované úložištěm a patří do entit ve `Added` stavu.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-292">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="bf8ed-293">K tomu je možné se vyhnout:</span><span class="sxs-lookup"><span data-stu-id="bf8ed-293">This can be avoided by:</span></span>
* <span data-ttu-id="bf8ed-294">Nepoužívejte klíče generované úložištěm.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-294">Not using store-generated keys.</span></span>
* <span data-ttu-id="bf8ed-295">Nastavení vlastností navigace pro vytváření relací místo nastavení hodnot cizích klíčů.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-295">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="bf8ed-296">Získá aktuální dočasné hodnoty klíče z informací o sledování entity.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-296">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="bf8ed-297">Například vrátí dočasnou hodnotu, `context.Entry(blog).Property(e => e.Id).CurrentValue` i když `blog.Id` nebyla nastavena.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-297">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="bf8ed-298">DetectChanges respektuje hodnoty klíčů generované úložištěm.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-298">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="bf8ed-299">Sledování problému #14616</span><span class="sxs-lookup"><span data-stu-id="bf8ed-299">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="bf8ed-300">Tato změna je představena ve verzi EF Core 3,0-Preview 3.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-300">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="bf8ed-301">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-301">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-302">Předtím, než EF Core 3,0, nalezené `DetectChanges` Nesledované entity by byly sledovány `Added` ve stavu a vloženy jako nový řádek při `SaveChanges` volání.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-302">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="bf8ed-303">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-303">**New behavior**</span></span>

<span data-ttu-id="bf8ed-304">Počínaje EF Core 3,0 platí, že pokud entita používá vygenerované hodnoty klíčů a je nastavená nějaká hodnota klíče, bude se entita sledovat ve `Modified` stavu.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-304">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="bf8ed-305">To znamená, že se předpokládá, že řádek pro entitu existuje a že bude při `SaveChanges` volání aktualizována.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-305">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="bf8ed-306">Pokud hodnota klíče není nastavená, nebo pokud typ entity nepoužívá vygenerované klíče, bude se nová entita dál sledovat jako `Added` v předchozích verzích.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-306">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="bf8ed-307">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-307">**Why**</span></span>

<span data-ttu-id="bf8ed-308">Tato změna byla provedena tak, aby byla pro práci s nepřipojenými grafy entit při použití klíčů generovaných úložištěm snazší.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-308">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="bf8ed-309">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-309">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-310">Tato změna může přerušit aplikaci, pokud je typ entity nakonfigurovaný na používání vygenerovaných klíčů, ale hodnoty klíčů jsou explicitně nastavené pro nové instance.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-310">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="bf8ed-311">Opravou je explicitně nakonfigurovat klíčové vlastnosti tak, aby nepoužívaly vygenerované hodnoty.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-311">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="bf8ed-312">Například s rozhraním API Fluent:</span><span class="sxs-lookup"><span data-stu-id="bf8ed-312">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="bf8ed-313">Nebo s poznámkami k datům:</span><span class="sxs-lookup"><span data-stu-id="bf8ed-313">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="bf8ed-314">Odstranění kaskádových operací se teď ve výchozím nastavení provádí hned.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-314">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="bf8ed-315">Sledování problému #10114</span><span class="sxs-lookup"><span data-stu-id="bf8ed-315">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="bf8ed-316">Tato změna je představena ve verzi EF Core 3,0-Preview 3.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-316">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="bf8ed-317">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-317">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-318">Před 3,0 EF Core aplikovány kaskádové akce (odstraňování závislých entit, když se odstraní požadovaný objekt zabezpečení nebo když je vztah k požadovanému objektu zabezpečení vážně) nevznikl, dokud není voláno SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-318">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="bf8ed-319">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-319">**New behavior**</span></span>

<span data-ttu-id="bf8ed-320">Od 3,0 EF Core aplikuje kaskádové akce hned po zjištění spouštěcí podmínky.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-320">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="bf8ed-321">Například volání `context.Remove()` na odstranění objektu zabezpečení bude mít za následek, že jsou všechny sledované požadované závislé položky nastaveny také na `Deleted` hodnotu okamžitě.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-321">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="bf8ed-322">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-322">**Why**</span></span>

<span data-ttu-id="bf8ed-323">Tato změna byla provedena za účelem zlepšení prostředí pro scénáře vytváření datových vazeb a auditování, kde je důležité pochopit, které entity budou _před_ `SaveChanges` voláním odstraněny.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-323">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="bf8ed-324">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-324">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-325">Předchozí chování lze obnovit pomocí nastavení v `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-325">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="bf8ed-326">Příklad:</span><span class="sxs-lookup"><span data-stu-id="bf8ed-326">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="bf8ed-327">DeleteBehavior. restrict má sémantiku čištění.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-327">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="bf8ed-328">Sledování problému #12661</span><span class="sxs-lookup"><span data-stu-id="bf8ed-328">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="bf8ed-329">Tato změna je představena ve verzi EF Core 3,0-Preview 5.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-329">This change is introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="bf8ed-330">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-330">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-331">Před 3,0 se `DeleteBehavior.Restrict` v databázi vytvořily cizí klíče se `Restrict` sémantikou, ale zároveň se nezřetelně změnila interní oprava.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-331">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="bf8ed-332">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-332">**New behavior**</span></span>

<span data-ttu-id="bf8ed-333">Počínaje 3,0 `DeleteBehavior.Restrict` zajistí, že se cizí klíče vytvoří s `Restrict` sémantikou – to znamená bez kaskády; porušení omezení throw--bez vlivu na interní opravu EF.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-333">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="bf8ed-334">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-334">**Why**</span></span>

<span data-ttu-id="bf8ed-335">Tato změna byla provedena pro zlepšení prostředí pro použití `DeleteBehavior` intuitivním způsobem, a to bez neočekávaných vedlejších účinků.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-335">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="bf8ed-336">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-336">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-337">Předchozí chování lze obnovit pomocí `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-337">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="bf8ed-338">Typy dotazů jsou konsolidovány s typy entit</span><span class="sxs-lookup"><span data-stu-id="bf8ed-338">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="bf8ed-339">Sledování problému #14194</span><span class="sxs-lookup"><span data-stu-id="bf8ed-339">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="bf8ed-340">Tato změna je představena ve verzi EF Core 3,0-Preview 3.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-340">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="bf8ed-341">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-341">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-342">Před EF Core 3,0 byly [typy dotazů](xref:core/modeling/query-types) prostředkem pro dotazování na data, která nedefinují primární klíč strukturovaným způsobem.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-342">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="bf8ed-343">To znamená, že typ dotazu byl použit pro mapování typů entit bez klíčů (pravděpodobnější z pohledu, ale také z tabulky), zatímco byl použit regulární typ entity, když byl klíč k dispozici (pravděpodobnější z tabulky, ale případně z pohledu).</span><span class="sxs-lookup"><span data-stu-id="bf8ed-343">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="bf8ed-344">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-344">**New behavior**</span></span>

<span data-ttu-id="bf8ed-345">Typ dotazu se teď bude jednat jenom o typ entity bez primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-345">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="bf8ed-346">Typy entit bez klíčů mají stejné funkce jako typy dotazů v předchozích verzích.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-346">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="bf8ed-347">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-347">**Why**</span></span>

<span data-ttu-id="bf8ed-348">Tato změna byla provedena proto, aby se snížila nejasnost v souvislosti s typy dotazů.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-348">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="bf8ed-349">Konkrétně jsou bez klíčů typy entit a jsou ze své podstaty jen pro čtení, ale neměly by být použity pouze proto, že typ entity musí být jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-349">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="bf8ed-350">Podobně jsou často mapovány na zobrazení, ale to je pouze v případě, že zobrazení často nedefinují klíče.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-350">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="bf8ed-351">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-351">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-352">Následující části rozhraní API jsou teď zastaralé:</span><span class="sxs-lookup"><span data-stu-id="bf8ed-352">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="bf8ed-353">**`ModelBuilder.Query<>()`** – Místo `ModelBuilder.Entity<>().HasNoKey()` toho je nutné volat k označení typu entity, protože neobsahují žádné klíče.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-353">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="bf8ed-354">Tato služba by se ještě nenakonfigurovala pomocí konvence, aby nedocházelo k chybám konfigurace, když je primární klíč očekávaný, ale neodpovídá konvenci.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-354">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="bf8ed-355">**`DbQuery<>`** – Místo `DbSet<>` toho by se mělo použít.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-355">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="bf8ed-356">**`DbContext.Query<>()`** – Místo `DbContext.Set<>()` toho by se mělo použít.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-356">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="bf8ed-357">Změnilo se konfigurační rozhraní API pro vztahy vlastněných typů.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-357">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="bf8ed-358">[Sledování problému #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[sledování problému #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[#14153 sledování](https://github.com/aspnet/EntityFrameworkCore/issues/14153) problému</span><span class="sxs-lookup"><span data-stu-id="bf8ed-358">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="bf8ed-359">Tato změna je představena ve verzi EF Core 3,0-Preview 3.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-359">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="bf8ed-360">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-360">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-361">Před EF Core 3,0 byla konfigurace vztahu vlastníka provedena přímo po `OwnsOne` volání nebo. `OwnsMany`</span><span class="sxs-lookup"><span data-stu-id="bf8ed-361">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="bf8ed-362">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-362">**New behavior**</span></span>

<span data-ttu-id="bf8ed-363">Od EF Core 3,0 teď existuje Fluent API pro konfiguraci navigační vlastnosti pro vlastníka pomocí `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-363">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="bf8ed-364">Příklad:</span><span class="sxs-lookup"><span data-stu-id="bf8ed-364">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="bf8ed-365">Konfigurace vztahující se k vztahu mezi vlastníkem a vlastníkem by nyní měla být zřetězena `WithOwner()` podobně jako konfigurace ostatních vztahů.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-365">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="bf8ed-366">I když se konfigurace samotného vlastního typu bude i nadále zřetězit za `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-366">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="bf8ed-367">Příklad:</span><span class="sxs-lookup"><span data-stu-id="bf8ed-367">For example:</span></span>

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

<span data-ttu-id="bf8ed-368">Kromě toho `Entity()`, `HasOne()`že volání `Set()` ,, nebo s cílem cílového typu, nyní vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-368">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="bf8ed-369">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-369">**Why**</span></span>

<span data-ttu-id="bf8ed-370">Tato změna byla provedena za účelem vytvoření čisticího oddělení mezi konfigurací samotného typu a _vztahu k_ typu, který je vlastníkem.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-370">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="bf8ed-371">Tím se zase odeberou nejednoznačnosti a nejasnosti u metod, jako `HasForeignKey`je.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-371">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="bf8ed-372">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-372">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-373">Změňte konfiguraci vztahů vlastněných typů tak, aby používala novou plochu rozhraní API, jak je znázorněno v předchozím příkladu.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-373">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="bf8ed-374">Závislé entity, které sdílí tabulku s objektem zabezpečení, jsou teď volitelné.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-374">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="bf8ed-375">Sledování problému #9005</span><span class="sxs-lookup"><span data-stu-id="bf8ed-375">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="bf8ed-376">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-376">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="bf8ed-377">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-377">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-378">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="bf8ed-378">Consider the following model:</span></span>
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
<span data-ttu-id="bf8ed-379">Pokud `OrderDetails` je před EF Core 3,0 ve `Order` vlastnictví nebo explicitně namapována na stejnou tabulku `OrderDetails` , byla při přidávání nové `Order`instance vždy vyžadována instance.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-379">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="bf8ed-380">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-380">**New behavior**</span></span>

<span data-ttu-id="bf8ed-381">Počínaje 3,0 EF Core umožňuje přidat `Order` `OrderDetails` bez `OrderDetails` a mapování všech vlastností kromě primárního klíče na sloupce s možnou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-381">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="bf8ed-382">Při dotazování EF Core sady `OrderDetails` na `null` , pokud některá z jejích požadovaných vlastností nemá hodnotu, nebo pokud nemá žádné požadované vlastnosti kromě primárního klíče a všechny vlastnosti jsou `null`.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-382">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="bf8ed-383">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-383">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-384">Pokud má váš model sdílení tabulky závislé na všech volitelných sloupcích, ale u navigace, na kterou se odkazuje, se neočekává `null` , že by aplikace měla být upravena tak, aby zpracovávala případy, kdy je `null`navigace.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-384">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="bf8ed-385">Pokud to není možné, měla by být do daného typu entity přidána požadovaná vlastnost, nebo alespoň jedna vlastnost by k ní měla mít`null` přiřazenou hodnotu bez hodnoty.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-385">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="bf8ed-386">Všechny entity sdílející tabulku se sloupcem souběžného tokenu musí být namapovány na vlastnost.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-386">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="bf8ed-387">Sledování problému #14154</span><span class="sxs-lookup"><span data-stu-id="bf8ed-387">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="bf8ed-388">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-388">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="bf8ed-389">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-389">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-390">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="bf8ed-390">Consider the following model:</span></span>
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
<span data-ttu-id="bf8ed-391">Pokud `OrderDetails` je před EF Core 3,0, ve `Order` vlastnictví nebo explicitně namapovaných na `OrderDetails` stejnou tabulku, pak aktualizace nebude aktualizovat `Version` hodnotu u klienta a další aktualizace selže.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-391">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="bf8ed-392">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-392">**New behavior**</span></span>

<span data-ttu-id="bf8ed-393">Počínaje 3,0 EF Core rozšíří novou `Version` `Order` hodnotu, pokud je vlastní `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-393">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="bf8ed-394">V opačném případě je při ověřování modelu vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-394">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="bf8ed-395">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-395">**Why**</span></span>

<span data-ttu-id="bf8ed-396">Tato změna byla provedena, aby nedocházelo k zastaralé hodnotě tokenu souběžnosti, pokud je aktualizována pouze jedna z entit mapovaných na stejnou tabulku.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-396">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="bf8ed-397">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-397">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-398">Všechny entity, které sdílejí tabulku, musí obsahovat vlastnost, která je namapovaná na sloupec tokenu souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-398">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="bf8ed-399">Je možné, že ho vytvoříte ve stínovém stavu:</span><span class="sxs-lookup"><span data-stu-id="bf8ed-399">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="bf8ed-400">Zděděné vlastnosti z nemapovaných typů jsou nyní namapovány na jeden sloupec pro všechny odvozené typy.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-400">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="bf8ed-401">Sledování problému #13998</span><span class="sxs-lookup"><span data-stu-id="bf8ed-401">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="bf8ed-402">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-402">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="bf8ed-403">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-403">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-404">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="bf8ed-404">Consider the following model:</span></span>
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

<span data-ttu-id="bf8ed-405">Před EF Core 3,0 `ShippingAddress` bude vlastnost namapována na samostatné sloupce pro `BulkOrder` a `Order` ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-405">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="bf8ed-406">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-406">**New behavior**</span></span>

<span data-ttu-id="bf8ed-407">Počínaje 3,0 se EF Core pro `ShippingAddress`nedá vytvořit jenom jeden sloupec.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-407">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="bf8ed-408">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-408">**Why**</span></span>

<span data-ttu-id="bf8ed-409">Starý behavoir nebyl očekáván.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-409">The old behavoir was unexpected.</span></span>

<span data-ttu-id="bf8ed-410">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-410">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-411">Vlastnost může být stále explicitně namapována na samostatný sloupec odvozených typů:</span><span class="sxs-lookup"><span data-stu-id="bf8ed-411">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

<a name="fkp"></a>

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="bf8ed-412">Konvence vlastností cizího klíče už neodpovídá stejnému názvu jako vlastnost Principal.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-412">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="bf8ed-413">Sledování problému #13274</span><span class="sxs-lookup"><span data-stu-id="bf8ed-413">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="bf8ed-414">Tato změna je představena ve verzi EF Core 3,0-Preview 3.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-414">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="bf8ed-415">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-415">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-416">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="bf8ed-416">Consider the following model:</span></span>
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
<span data-ttu-id="bf8ed-417">Před EF Core 3,0 `CustomerId` se vlastnost používá pro cizí klíč podle konvence.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-417">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="bf8ed-418">Nicméně pokud `Order` je vlastněný typ, pak by to vedlo `CustomerId` také k tomu, že primární klíč a to není obvykle očekávané.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-418">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="bf8ed-419">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-419">**New behavior**</span></span>

<span data-ttu-id="bf8ed-420">Počínaje 3,0 se EF Core nepokouší použít vlastnosti pro cizí klíče podle konvence, pokud mají stejný název jako vlastnost Principal.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-420">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="bf8ed-421">Název objektu zabezpečení zřetězený s názvem vlastnosti objektu zabezpečení a název navigace zřetězený s vzory názvů hlavních vlastností se pořád shodují.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-421">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="bf8ed-422">Příklad:</span><span class="sxs-lookup"><span data-stu-id="bf8ed-422">For example:</span></span>

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

<span data-ttu-id="bf8ed-423">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-423">**Why**</span></span>

<span data-ttu-id="bf8ed-424">Tato změna byla provedena, aby nedocházelo k chybnému definování vlastností primárního klíče u vlastněných typů.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-424">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="bf8ed-425">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-425">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-426">Pokud by vlastnost měla být cizí klíč, a proto je součástí primárního klíče, pak ji explicitně nakonfigurujte jako takovou.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-426">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="bf8ed-427">Připojení k databázi je teď zavřené, pokud už nepoužíváte dřív, než se dokončí jeho objekt TransactionScope.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-427">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="bf8ed-428">Sledování problému #14218</span><span class="sxs-lookup"><span data-stu-id="bf8ed-428">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="bf8ed-429">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-429">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="bf8ed-430">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-430">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-431">Pokud kontext otevře během EF Core 3,0 připojení uvnitř `TransactionScope`, zůstane připojení otevřené, zatímco aktuální `TransactionScope` aktivní je.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-431">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="bf8ed-432">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-432">**New behavior**</span></span>

<span data-ttu-id="bf8ed-433">Od 3,0 EF Core ukončí připojení, jakmile ho dokončí jeho používání.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-433">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="bf8ed-434">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-434">**Why**</span></span>

<span data-ttu-id="bf8ed-435">Tato změna umožňuje použít více kontextů současně `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-435">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="bf8ed-436">Nové chování se také shoduje s EF6.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-436">The new behavior also matches EF6.</span></span>

<span data-ttu-id="bf8ed-437">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-437">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-438">Pokud připojení potřebuje zůstat otevřeným explicitním voláním `OpenConnection()` , zajistí, že EF Core neuzavře předčasně:</span><span class="sxs-lookup"><span data-stu-id="bf8ed-438">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

<a name="each"></a>

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="bf8ed-439">Každá vlastnost používá nezávislou generaci celočíselného klíče v paměti.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-439">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="bf8ed-440">Sledování problému #6872</span><span class="sxs-lookup"><span data-stu-id="bf8ed-440">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="bf8ed-441">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-441">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="bf8ed-442">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-442">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-443">Před EF Core 3,0 byl pro všechny vlastnosti celého čísla klíče v paměti použit jeden generátor sdílených hodnot.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-443">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="bf8ed-444">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-444">**New behavior**</span></span>

<span data-ttu-id="bf8ed-445">Počínaje EF Core 3,0 každé klíčové vlastnosti celého čísla získá vlastní generátor hodnot při použití databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-445">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="bf8ed-446">Také pokud je databáze odstraněna, generování klíče se obnoví pro všechny tabulky.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-446">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="bf8ed-447">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-447">**Why**</span></span>

<span data-ttu-id="bf8ed-448">Tato změna byla provedená tak, aby se při vytváření klíčů v paměti lépe rovnala generování klíčů v paměti a vylepšila se možnost izolovat testy od sebe při použití databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-448">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="bf8ed-449">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-449">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-450">To může poškodit aplikaci, která se spoléhá na konkrétní hodnoty klíče v paměti, které se mají nastavit.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-450">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="bf8ed-451">Místo toho se nemusíte spoléhat na konkrétní hodnoty klíčů nebo aktualizovat tak, aby odpovídaly novému chování.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-451">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="bf8ed-452">Ve výchozím nastavení se používají pole pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-452">Backing fields are used by default</span></span>

[<span data-ttu-id="bf8ed-453">Sledování problému #12430</span><span class="sxs-lookup"><span data-stu-id="bf8ed-453">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="bf8ed-454">Tato změna je představena ve verzi EF Core 3,0-Preview 2.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-454">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="bf8ed-455">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-455">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-456">Před 3,0, a to i v případě, že je známé pole zálohování pro vlastnost, EF Core ve výchozím nastavení přečetl a zapsat hodnotu vlastnosti pomocí metod getter a setter vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-456">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="bf8ed-457">Výjimkou je provedení dotazu, kde by se pole zálohování nastavilo přímo, pokud je známé.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-457">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="bf8ed-458">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-458">**New behavior**</span></span>

<span data-ttu-id="bf8ed-459">Počínaje EF Core 3,0 platí, že pokud je známé pole zálohování pro vlastnost, pak EF Core tuto vlastnost vždy přečte a zapíše pomocí pole pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-459">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="bf8ed-460">To může způsobit přerušení aplikace, pokud se aplikace spoléhá na další chování kódované v metodách getter nebo setter.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-460">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="bf8ed-461">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-461">**Why**</span></span>

<span data-ttu-id="bf8ed-462">Tato změna byla provedena EF Core proto, aby při provádění databázových operací, které obsahují entity, ve výchozím nastavení nechybně aktivovala obchodní logiku.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-462">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="bf8ed-463">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-463">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-464">Chování před 3,0 se dá obnovit pomocí konfigurace režimu přístupu vlastnosti zapnuto `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-464">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="bf8ed-465">Příklad:</span><span class="sxs-lookup"><span data-stu-id="bf8ed-465">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="bf8ed-466">Vyvolat, zda je nalezeno více kompatibilních zálohovaných polí</span><span class="sxs-lookup"><span data-stu-id="bf8ed-466">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="bf8ed-467">Sledování problému #12523</span><span class="sxs-lookup"><span data-stu-id="bf8ed-467">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="bf8ed-468">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-468">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="bf8ed-469">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-469">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-470">Pokud se v EF Core 3,0 shodovalo více polí s pravidly pro hledání zálohovacího pole vlastnosti, bude jedno pole zvoleno na základě pořadí priorit.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-470">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="bf8ed-471">To může způsobit, že se nesprávné pole použije v nejednoznačných případech.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-471">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="bf8ed-472">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-472">**New behavior**</span></span>

<span data-ttu-id="bf8ed-473">Počínaje EF Core 3,0 platí, že pokud je více polí spárováno se stejnou vlastností, je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-473">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="bf8ed-474">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-474">**Why**</span></span>

<span data-ttu-id="bf8ed-475">Tato změna byla provedena, aby nedocházelo k tichému použití jednoho pole v případě, že může být pouze jedna z nich správná.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-475">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="bf8ed-476">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-476">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-477">Vlastnosti s nejednoznačnými zálohovacími poli musí obsahovat pole, které se má explicitně použít.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-477">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="bf8ed-478">Například pomocí rozhraní Fluent API:</span><span class="sxs-lookup"><span data-stu-id="bf8ed-478">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="bf8ed-479">Názvy vlastností pouze pro pole se musí shodovat s názvem pole.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-479">Field-only property names should match the field name</span></span>

<span data-ttu-id="bf8ed-480">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-480">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="bf8ed-481">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-481">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-482">Před EF Core 3,0 může být vlastnost určena hodnotou řetězce a v případě, že v typu CLR nebyla nalezena žádná vlastnost s tímto názvem, EF Core by se pokusila ji porovnat s polem pomocí pravidel konvence.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-482">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="bf8ed-483">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-483">**New behavior**</span></span>

<span data-ttu-id="bf8ed-484">Počínaje EF Core 3,0 se vlastnost pouze pole musí přesně shodovat s názvem pole.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-484">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="bf8ed-485">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-485">**Why**</span></span>

<span data-ttu-id="bf8ed-486">Tato změna byla provedena, aby se nepoužívalo stejné pole pro dvě vlastnosti s názvem podobně, ale také pravidla pro porovnání vlastností pouze pro pole, která jsou shodná s vlastnostmi mapovanými na vlastnosti CLR.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-486">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="bf8ed-487">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-487">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-488">Vlastnosti pouze polí musí být pojmenovány stejně jako pole, na které jsou namapována.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-488">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="bf8ed-489">V novější verzi EF Core 3,0 plánujeme znovu povolit explicitní konfiguraci názvu pole, který se liší od názvu vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="bf8ed-489">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="bf8ed-490">AddDbContext/AddDbContextPool už nevolá AddLogging a AddMemoryCache.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-490">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="bf8ed-491">Sledování problému #14756</span><span class="sxs-lookup"><span data-stu-id="bf8ed-491">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="bf8ed-492">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-492">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="bf8ed-493">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-493">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-494">Před EF Core 3,0, volání `AddDbContext` nebo `AddDbContextPool` by také registrovaly služby protokolování a ukládání do mezipaměti v paměti pomocí D. I přes volání [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) a [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="bf8ed-494">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="bf8ed-495">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-495">**New behavior**</span></span>

<span data-ttu-id="bf8ed-496">Počínaje EF Core 3,0 `AddDbContext` a `AddDbContextPool` nadále se nebudou registrovat tyto služby se vkládáním závislostí (di).</span><span class="sxs-lookup"><span data-stu-id="bf8ed-496">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="bf8ed-497">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-497">**Why**</span></span>

<span data-ttu-id="bf8ed-498">EF Core 3,0 nevyžaduje, aby se tyto služby nacházejí v kontejneru aplikace DI.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-498">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="bf8ed-499">Pokud `ILoggerFactory` je však v kontejneru aplikace zaregistrován, bude nadále používána EF Core.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-499">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="bf8ed-500">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-500">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-501">Pokud vaše aplikace potřebuje tyto služby, zaregistrujte je explicitně pomocí kontejneru DI pomocí [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) nebo [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="bf8ed-501">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="bf8ed-502">DbContext. entry teď provádí místní DetectChanges.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-502">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="bf8ed-503">Sledování problému #13552</span><span class="sxs-lookup"><span data-stu-id="bf8ed-503">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="bf8ed-504">Tato změna je představena ve verzi EF Core 3,0-Preview 3.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-504">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="bf8ed-505">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-505">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-506">Před EF Core 3,0 by volání `DbContext.Entry` způsobilo zjištění změn všech sledovaných entit.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-506">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="bf8ed-507">Tím je zajištěno, že stav zpřístupněno v `EntityEntry` nástroji byl aktuální.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-507">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="bf8ed-508">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-508">**New behavior**</span></span>

<span data-ttu-id="bf8ed-509">Počínaje EF Core 3,0 se nyní volání `DbContext.Entry` pokusí zjistit změny v dané entitě a všechny sledované hlavní entity, které se k ní vztahují.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-509">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="bf8ed-510">To znamená, že změny jinde nemohly být zjištěny voláním této metody, což by mohlo mít vliv na stav aplikace.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-510">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="bf8ed-511">Všimněte si, `ChangeTracker.AutoDetectChangesEnabled` že pokud je `false` nastaveno na, tak i toto místní zjišťování změn bude zakázáno.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-511">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="bf8ed-512">Jiné metody, které způsobují detekci změn – například `ChangeTracker.Entries` a `SaveChanges`---stále zapříčiní plné `DetectChanges` ze všech sledovaných entit.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-512">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="bf8ed-513">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-513">**Why**</span></span>

<span data-ttu-id="bf8ed-514">Tato změna byla provedena za účelem zlepšení výchozího výkonu použití `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-514">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="bf8ed-515">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-515">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-516">Před `ChgangeTracker.DetectChanges()` voláním `Entry` zajistěte explicitní volání, aby bylo zajištěno chování před 3,0.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-516">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="bf8ed-517">Klíče řetězce a pole bajtů nejsou ve výchozím nastavení generovány klientem.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-517">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="bf8ed-518">Sledování problému #14617</span><span class="sxs-lookup"><span data-stu-id="bf8ed-518">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="bf8ed-519">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-519">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="bf8ed-520">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-520">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-521">Před EF Core 3,0 `string` a `byte[]` vlastnosti klíče lze použít bez explicitního nastavení hodnoty, která není null.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-521">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="bf8ed-522">V takovém případě se hodnota klíče vygeneruje na klientovi jako identifikátor GUID, který je serializovaný na bajty pro `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-522">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="bf8ed-523">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-523">**New behavior**</span></span>

<span data-ttu-id="bf8ed-524">Počínaje EF Core 3,0 bude vyvolána výjimka oznamující, že nebyla nastavena žádná hodnota klíče.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-524">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="bf8ed-525">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-525">**Why**</span></span>

<span data-ttu-id="bf8ed-526">Tato změna byla provedena, protože obecně nejsou `string` užitečné hodnoty generované / `byte[]` klientem a výchozí chování způsobilo, že je obtížné vygenerovat hodnoty klíčů běžným způsobem.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-526">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="bf8ed-527">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-527">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-528">Chování před 3,0 lze získat explicitním určením, že klíčové vlastnosti by měly používat generované hodnoty, pokud není nastavena žádná jiná hodnota, která není null.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-528">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="bf8ed-529">Například s rozhraním API Fluent:</span><span class="sxs-lookup"><span data-stu-id="bf8ed-529">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="bf8ed-530">Nebo s poznámkami k datům:</span><span class="sxs-lookup"><span data-stu-id="bf8ed-530">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="bf8ed-531">ILoggerFactory je teď služba s vymezeným oborem.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-531">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="bf8ed-532">Sledování problému #14698</span><span class="sxs-lookup"><span data-stu-id="bf8ed-532">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="bf8ed-533">Tato změna je představena ve verzi EF Core 3,0-Preview 3.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-533">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="bf8ed-534">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-534">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-535">Před EF Core 3,0 `ILoggerFactory` byl zaregistrován jako služba typu singleton.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-535">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="bf8ed-536">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-536">**New behavior**</span></span>

<span data-ttu-id="bf8ed-537">Počínaje EF Core 3,0 `ILoggerFactory` je nyní registrováno jako obor.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-537">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="bf8ed-538">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-538">**Why**</span></span>

<span data-ttu-id="bf8ed-539">Tato změna byla provedena tak, aby povolovala přidružení protokolovacího `DbContext` nástroje s instancí, která umožňuje další funkce a odebírá některé případy patologického chování, jako je například rozpad interních poskytovatelů služeb.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-539">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="bf8ed-540">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-540">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-541">Tato změna by neměla mít vliv na kód aplikace, pokud se neregistruje a nepoužívá vlastní služby pro EF Core interního poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-541">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="bf8ed-542">To není běžné.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-542">This isn't common.</span></span>
<span data-ttu-id="bf8ed-543">V těchto případech bude většina věcí pořád fungovat, ale jakákoli služba typu Singleton, která byla v závislosti `ILoggerFactory` na tom, bude muset změnit tak, `ILoggerFactory` aby získala jiný způsob.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-543">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="bf8ed-544">Pokud narazíte na takové situace, zajistěte prosím problém na [EF Core modul pro sledování problémů GitHubu](https://github.com/aspnet/EntityFrameworkCore/issues) , abychom nás věděli, jak `ILoggerFactory` ho používáte, abychom mohli lépe pochopit, jak to v budoucnu Nerušit.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-544">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="bf8ed-545">Opožděné načítání proxy serverů už nepředpokládá navigační vlastnosti, jsou plně načtené.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-545">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="bf8ed-546">Sledování problému #12780</span><span class="sxs-lookup"><span data-stu-id="bf8ed-546">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="bf8ed-547">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-547">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="bf8ed-548">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-548">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-549">Předtím, než EF Core 3,0, `DbContext` neexistuje žádný způsob, jak zjistit, zda byla daná vlastnost navigace u entity získané z tohoto kontextu plně načtena nebo ne.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-549">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="bf8ed-550">Proxy místo toho předpokládají, že je načtena odkazová navigace, pokud má hodnotu jinou než null a že je načtena navigace kolekce, pokud není prázdná.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-550">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="bf8ed-551">V těchto případech by byl pokus o opožděné načtení no-op.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-551">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="bf8ed-552">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-552">**New behavior**</span></span>

<span data-ttu-id="bf8ed-553">Počínaje EF Core 3,0 budou proxy servery sledovat, zda je načtena vlastnost navigace.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-553">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="bf8ed-554">To znamená, že při pokusu o přístup k navigační vlastnosti, která je načtena po vyřazení kontextu, bude vždy ta no-op, i když je načtená navigace prázdná nebo má hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-554">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="bf8ed-555">Naopak při pokusu o přístup k navigační vlastnosti, která není načtená, vyvolá výjimku, pokud je kontext vyřazený, i když je vlastnost navigace neprázdná kolekce.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-555">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="bf8ed-556">Pokud nastane tato situace, znamená to, že se kód aplikace pokouší použít opožděné načítání v neplatném čase a aplikace by se měla změnit, aby to nevedlo.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-556">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="bf8ed-557">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-557">**Why**</span></span>

<span data-ttu-id="bf8ed-558">Tato změna byla provedena, aby při pokusu o opožděné načtení na uvolněnou `DbContext` instanci bylo chování konzistentní a správné.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-558">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="bf8ed-559">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-559">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-560">Aktualizujte kód aplikace, aby se nepokoušel opožděné načtení s odstraněným kontextem, nebo nastavte tuto hodnotu jako No-op, jak je popsáno ve zprávě výjimky.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-560">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="bf8ed-561">Nadměrné vytváření interních zprostředkovatelů služeb je teď ve výchozím nastavení chyba.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-561">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="bf8ed-562">Sledování problému #10236</span><span class="sxs-lookup"><span data-stu-id="bf8ed-562">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="bf8ed-563">Tato změna je představena ve verzi EF Core 3,0-Preview 3.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-563">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="bf8ed-564">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-564">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-565">Před EF Core 3,0 se pro aplikaci, která vytváří patologický počet interních poskytovatelů služeb, zaprotokoluje upozornění.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-565">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="bf8ed-566">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-566">**New behavior**</span></span>

<span data-ttu-id="bf8ed-567">Počínaje EF Core 3,0 je toto upozornění nyní považováno za chybu a je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-567">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="bf8ed-568">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-568">**Why**</span></span>

<span data-ttu-id="bf8ed-569">Tato změna byla provedená tak, že se tento patologický případ výslovně zveřejňuje tak, aby se zlepšil kód aplikace.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-569">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="bf8ed-570">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-570">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-571">Nejvhodnější příčinou této chyby je pochopení hlavní příčiny a zastavení vytváření, takže mnoho interních poskytovatelů služeb.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-571">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="bf8ed-572">Chybu však lze převést zpět na varování (nebo ignorováno) prostřednictvím konfigurace na `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-572">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="bf8ed-573">Příklad:</span><span class="sxs-lookup"><span data-stu-id="bf8ed-573">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="bf8ed-574">Nové chování pro HasOne/HasMany se volá s jedním řetězcem.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-574">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="bf8ed-575">Sledování problému #9171</span><span class="sxs-lookup"><span data-stu-id="bf8ed-575">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="bf8ed-576">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-576">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="bf8ed-577">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-577">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-578">Před EF Core 3,0, volání `HasOne` kódu nebo `HasMany` s jedním řetězcem bylo interpretováno matoucím způsobem.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-578">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="bf8ed-579">Příklad:</span><span class="sxs-lookup"><span data-stu-id="bf8ed-579">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="bf8ed-580">Kód vypadá jako v `Samurai` `Entrance` souvislosti s jiným typem entity pomocí navigační vlastnosti, která může být soukromá.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-580">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="bf8ed-581">Ve skutečnosti se tento kód pokusí vytvořit relaci k některému typu entity s názvem `Entrance` bez navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-581">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="bf8ed-582">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-582">**New behavior**</span></span>

<span data-ttu-id="bf8ed-583">Počínaje EF Core 3,0 výše uvedený kód má nyní podobný vzhled jako předtím.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-583">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="bf8ed-584">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-584">**Why**</span></span>

<span data-ttu-id="bf8ed-585">Staré chování bylo velmi matoucí, zejména při čtení konfiguračního kódu a hledání chyb.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-585">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="bf8ed-586">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-586">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-587">Tím dojde pouze k přerušení aplikací, které jsou explicitně konfigurovány pomocí řetězců pro názvy typů, a bez explicitního určení vlastnosti navigace.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-587">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="bf8ed-588">To není běžné.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-588">This is not common.</span></span>
<span data-ttu-id="bf8ed-589">Předchozí chování lze získat pomocí explicitního předání `null` názvu vlastnosti navigace.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-589">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="bf8ed-590">Příklad:</span><span class="sxs-lookup"><span data-stu-id="bf8ed-590">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="bf8ed-591">Návratový typ pro několik asynchronních metod byl změněn z úlohy na ValueTask</span><span class="sxs-lookup"><span data-stu-id="bf8ed-591">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="bf8ed-592">Sledování problému #15184</span><span class="sxs-lookup"><span data-stu-id="bf8ed-592">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="bf8ed-593">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-593">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="bf8ed-594">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-594">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-595">Následující asynchronní metody dříve vrátily `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="bf8ed-595">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="bf8ed-596">`ValueGenerator.NextValueAsync()`(a odvozování tříd)</span><span class="sxs-lookup"><span data-stu-id="bf8ed-596">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="bf8ed-597">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-597">**New behavior**</span></span>

<span data-ttu-id="bf8ed-598">Výše uvedené metody nyní vrací stejnou `ValueTask<T>` `T` hodnotu jako předtím.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-598">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="bf8ed-599">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-599">**Why**</span></span>

<span data-ttu-id="bf8ed-600">Tato změna snižuje počet přidělení haldy, které vznikly při vyvolání těchto metod, což zlepšuje obecný výkon.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-600">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="bf8ed-601">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-601">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-602">Aplikace jednoduše čekají na rozhraní API, které je třeba znovu zkompilovat – nejsou nutné žádné změny zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-602">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="bf8ed-603">Složitější využití (například předání vráceného `Task` do `Task.WhenAny()`) obvykle vyžaduje, aby bylo vráceno `ValueTask<T>` převedené na a `Task<T>` voláním `AsTask()` .</span><span class="sxs-lookup"><span data-stu-id="bf8ed-603">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="bf8ed-604">Všimněte si, že se tím sníží omezení přidělení, které tato změna přináší.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-604">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="bf8ed-605">Relační: anotace TypeMapping je nyní pouze TypeMapping</span><span class="sxs-lookup"><span data-stu-id="bf8ed-605">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="bf8ed-606">Sledování problému #9913</span><span class="sxs-lookup"><span data-stu-id="bf8ed-606">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="bf8ed-607">Tato změna je představena ve verzi EF Core 3,0-Preview 2.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-607">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="bf8ed-608">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-608">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-609">Název poznámky pro poznámky k mapování typů byl "relační: TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="bf8ed-609">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="bf8ed-610">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-610">**New behavior**</span></span>

<span data-ttu-id="bf8ed-611">Název poznámky pro mapování typů je nyní "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="bf8ed-611">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="bf8ed-612">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-612">**Why**</span></span>

<span data-ttu-id="bf8ed-613">Mapování typů se nyní používají pro více než stejné poskytovatele relačních databází.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-613">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="bf8ed-614">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-614">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-615">Tím dojde pouze k přerušení aplikací, které přistupují k mapování typu přímo jako anotaci, což není běžné.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-615">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="bf8ed-616">Nejvhodnější akcí pro opravu je použití prostoru rozhraní API pro přístup k mapování typů namísto použití anotace přímo.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-616">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="bf8ed-617">ToTable na odvozeném typu vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-617">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="bf8ed-618">Sledování problému #11811</span><span class="sxs-lookup"><span data-stu-id="bf8ed-618">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="bf8ed-619">Tato změna je představena ve verzi EF Core 3,0-Preview 3.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-619">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="bf8ed-620">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-620">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-621">Před EF Core 3,0, `ToTable()` který je volán na odvozeném typu, by byl ignorován, protože pouze dědění mapování dědičnosti bylo typu TPH, který není platný.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-621">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="bf8ed-622">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-622">**New behavior**</span></span>

<span data-ttu-id="bf8ed-623">Počínaje EF Core 3,0 a přípravou pro přidání podpory TPT a TPC v novější verzi, která se `ToTable()` volá na odvozeném typu, teď vyvolá výjimku, aby se předešlo neočekávané změně mapování v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-623">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="bf8ed-624">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-624">**Why**</span></span>

<span data-ttu-id="bf8ed-625">V současné době není platný pro mapování odvozeného typu na jinou tabulku.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-625">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="bf8ed-626">Tato změna zabrání v budoucnosti v budoucnu, pokud se to stalo platným.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-626">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="bf8ed-627">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-627">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-628">Odeberte všechny pokusy o mapování odvozených typů na jiné tabulky.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-628">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="bf8ed-629">ForSqlServerHasIndex nahrazeno HasIndex</span><span class="sxs-lookup"><span data-stu-id="bf8ed-629">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="bf8ed-630">Sledování problému #12366</span><span class="sxs-lookup"><span data-stu-id="bf8ed-630">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="bf8ed-631">Tato změna je představena ve verzi EF Core 3,0-Preview 3.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-631">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="bf8ed-632">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-632">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-633">Před EF Core 3,0 jste `ForSqlServerHasIndex().ForSqlServerInclude()` získali způsob, jak nakonfigurovat sloupce používané pomocí `INCLUDE`nástroje.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-633">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="bf8ed-634">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-634">**New behavior**</span></span>

<span data-ttu-id="bf8ed-635">Počínaje EF Core 3,0 se teď použití `Include` na indexu podporuje na relační úrovni.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-635">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="bf8ed-636">Použijte `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-636">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="bf8ed-637">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-637">**Why**</span></span>

<span data-ttu-id="bf8ed-638">Tato změna byla provedena za účelem konsolidace rozhraní API pro indexy `Include` na jednom místě pro všechny poskytovatele databáze.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-638">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="bf8ed-639">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-639">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-640">Použijte nové rozhraní API, jak vidíte výše.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-640">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="bf8ed-641">Změny rozhraní API pro metadata</span><span class="sxs-lookup"><span data-stu-id="bf8ed-641">Metadata API changes</span></span>

[<span data-ttu-id="bf8ed-642">Sledování problému #214</span><span class="sxs-lookup"><span data-stu-id="bf8ed-642">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="bf8ed-643">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-643">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="bf8ed-644">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-644">**New behavior**</span></span>

<span data-ttu-id="bf8ed-645">Následující vlastnosti byly převedeny na rozšiřující metody:</span><span class="sxs-lookup"><span data-stu-id="bf8ed-645">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="bf8ed-646">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-646">**Why**</span></span>

<span data-ttu-id="bf8ed-647">Tato změna zjednodušuje implementaci výše uvedených rozhraní.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-647">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="bf8ed-648">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-648">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-649">Použijte nové metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-649">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="bf8ed-650">Změny rozhraní API pro konkrétního zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="bf8ed-650">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="bf8ed-651">Sledování problému #214</span><span class="sxs-lookup"><span data-stu-id="bf8ed-651">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="bf8ed-652">Tato změna je představena ve verzi EF Core 3,0-Preview 6.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-652">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="bf8ed-653">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-653">**New behavior**</span></span>

<span data-ttu-id="bf8ed-654">Metody rozšíření specifické pro poskytovatele budou shrnuty:</span><span class="sxs-lookup"><span data-stu-id="bf8ed-654">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.GetSqlServerIsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.ForSqlServerUseIdentityColumn()`

<span data-ttu-id="bf8ed-655">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-655">**Why**</span></span>

<span data-ttu-id="bf8ed-656">Tato změna zjednodušuje implementaci výše uvedených rozšiřujících metod.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-656">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="bf8ed-657">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-657">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-658">Použijte nové metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-658">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="bf8ed-659">EF Core už neposílá direktivu pragma pro vynucení KOFK SQLite.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-659">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="bf8ed-660">Sledování problému #12151</span><span class="sxs-lookup"><span data-stu-id="bf8ed-660">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="bf8ed-661">Tato změna je představena ve verzi EF Core 3,0-Preview 3.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-661">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="bf8ed-662">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-662">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-663">Před EF Core 3,0 EF Core odeslat `PRAGMA foreign_keys = 1` při otevření připojení k sqlite.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-663">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="bf8ed-664">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-664">**New behavior**</span></span>

<span data-ttu-id="bf8ed-665">Počínaje EF Core 3,0 EF Core již neposílá `PRAGMA foreign_keys = 1` při otevření připojení k sqlite.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-665">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="bf8ed-666">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-666">**Why**</span></span>

<span data-ttu-id="bf8ed-667">Tato změna byla provedena, protože EF Core `SQLitePCLRaw.bundle_e_sqlite3` používá ve výchozím nastavení, což zase znamená, že je ve výchozím nastavení zapnuté vynucení CK a není nutné je explicitně povolit při každém otevření připojení.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-667">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="bf8ed-668">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-668">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-669">Cizí klíče jsou ve výchozím nastavení povolené v SQLitePCLRaw. bundle_e_sqlite3, který se ve výchozím nastavení používá pro EF Core.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-669">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="bf8ed-670">V ostatních případech je možné povolit cizí klíče zadáním `Foreign Keys=True` v připojovacím řetězci.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-670">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="bf8ed-671">Microsoft. EntityFrameworkCore. sqlite teď závisí na SQLitePCLRaw. bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="bf8ed-671">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="bf8ed-672">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-672">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-673">Před EF Core 3,0 se používá `SQLitePCLRaw.bundle_green`EF Core.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-673">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="bf8ed-674">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-674">**New behavior**</span></span>

<span data-ttu-id="bf8ed-675">Počínaje EF Core 3,0 EF Core používá `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-675">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="bf8ed-676">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-676">**Why**</span></span>

<span data-ttu-id="bf8ed-677">Tato změna byla provedena tak, že verze SQLiteu použitá v iOS je konzistentní s jinými platformami.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-677">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="bf8ed-678">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-678">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-679">Pokud chcete použít nativní verzi SQLite v iOS, nakonfigurujte `Microsoft.Data.Sqlite` , aby používala `SQLitePCLRaw` jiný svazek.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-679">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="bf8ed-680">Hodnoty GUID se teď ukládají jako TEXT na SQLite.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-680">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="bf8ed-681">Sledování problému #15078</span><span class="sxs-lookup"><span data-stu-id="bf8ed-681">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="bf8ed-682">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-682">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="bf8ed-683">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-683">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-684">Hodnoty GUID byly dříve uloženy jako hodnoty objektů BLOB u SQLite.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-684">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="bf8ed-685">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-685">**New behavior**</span></span>

<span data-ttu-id="bf8ed-686">Hodnoty GUID jsou nyní uloženy jako TEXT.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-686">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="bf8ed-687">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-687">**Why**</span></span>

<span data-ttu-id="bf8ed-688">Binární formát identifikátorů GUID není standardizovaný.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-688">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="bf8ed-689">Uložení hodnot jako textu zajistí, že databáze bude lépe kompatibilní s jinými technologiemi.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-689">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="bf8ed-690">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-690">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-691">Existující databáze můžete migrovat do nového formátu tím, že spustíte příkaz SQL podobně jako následující.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-691">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="bf8ed-692">V EF Core můžete i nadále používat předchozí chování nakonfigurováním převaděče hodnot na těchto vlastnostech.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-692">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="bf8ed-693">Microsoft. data. sqlite zůstává schopný přečítat hodnoty GUID z objektů BLOB a textových sloupců. vzhledem k tomu, že výchozí formát pro parametry a konstanty se změnil, bude pravděpodobně nutné provést akci u většiny scénářů, které zahrnují identifikátory GUID.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-693">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="bf8ed-694">Hodnoty char se teď ukládají jako TEXT na SQLite.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-694">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="bf8ed-695">Sledování problému #15020</span><span class="sxs-lookup"><span data-stu-id="bf8ed-695">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="bf8ed-696">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-696">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="bf8ed-697">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-697">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-698">Hodnoty typu char byly dříve sored jako CELOČÍSELNé hodnoty u SQLite.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-698">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="bf8ed-699">Například hodnota znaku *a* byla uložena jako celočíselná hodnota 65.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-699">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="bf8ed-700">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-700">**New behavior**</span></span>

<span data-ttu-id="bf8ed-701">Hodnoty typu char jsou nyní uloženy jako TEXT.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-701">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="bf8ed-702">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-702">**Why**</span></span>

<span data-ttu-id="bf8ed-703">Ukládání hodnot jako TEXT je přirozenější a databáze usnadňuje kompatibilitu s jinými technologiemi.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-703">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="bf8ed-704">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-704">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-705">Existující databáze můžete migrovat do nového formátu tím, že spustíte příkaz SQL podobně jako následující.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-705">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="bf8ed-706">V EF Core můžete i nadále používat předchozí chování nakonfigurováním převaděče hodnot na těchto vlastnostech.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-706">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="bf8ed-707">Microsoft. data. sqlite také zůstává schopný číst znakové hodnoty z CELOČÍSELNého i TEXTOVÉHO sloupce, takže některé scénáře nemusí vyžadovat žádnou akci.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-707">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="bf8ed-708">ID migrace se teď generují pomocí kalendáře invariantní jazykové verze.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-708">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="bf8ed-709">Sledování problému #12978</span><span class="sxs-lookup"><span data-stu-id="bf8ed-709">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="bf8ed-710">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-710">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="bf8ed-711">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-711">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-712">ID migrace se nechtěně vygenerovala pomocí kalendáře aktuální jazykové verze.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-712">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="bf8ed-713">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-713">**New behavior**</span></span>

<span data-ttu-id="bf8ed-714">ID migrace se nyní vždy generují pomocí kalendáře neutrální jazykové verze (gregoriánský).</span><span class="sxs-lookup"><span data-stu-id="bf8ed-714">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="bf8ed-715">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-715">**Why**</span></span>

<span data-ttu-id="bf8ed-716">Pořadí migrace je důležité při aktualizaci databáze nebo řešení konfliktů při slučování.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-716">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="bf8ed-717">Pomocí invariantního kalendáře se vyhnete problémům s řazením, které mohou být výsledkem členů týmu jiné systémové kalendáře.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-717">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="bf8ed-718">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-718">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-719">Tato změna má vliv na kohokoli, kdo používá negregoriánský kalendář, ve kterém je rok větší než gregoriánský kalendář (například thajský buddhistický kalendář).</span><span class="sxs-lookup"><span data-stu-id="bf8ed-719">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="bf8ed-720">Existující identifikátory migrace se budou muset aktualizovat, aby se nové migrace objednaly po stávajících migracích.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-720">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="bf8ed-721">ID migrace najdete v atributu migrace v souborech návrháře migrace.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-721">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="bf8ed-722">Také je potřeba aktualizovat tabulku historie migrace.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-722">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="bf8ed-723">UseRowNumberForPaging se odebral.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-723">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="bf8ed-724">Sledování problému #16400</span><span class="sxs-lookup"><span data-stu-id="bf8ed-724">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="bf8ed-725">Tato změna je představena ve verzi EF Core 3,0-Preview 6.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-725">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="bf8ed-726">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-726">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-727">Před EF Core 3,0 `UseRowNumberForPaging` lze použít k vygenerování SQL pro stránkování, které je kompatibilní s SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-727">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="bf8ed-728">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-728">**New behavior**</span></span>

<span data-ttu-id="bf8ed-729">Počínaje EF Core 3,0 bude EF generovat pouze SQL pro stránkování, které je kompatibilní pouze s novějšími verzemi SQL Server.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-729">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="bf8ed-730">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-730">**Why**</span></span>

<span data-ttu-id="bf8ed-731">Tuto změnu provedeme, protože [SQL Server 2008 už není podporovaným produktem](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) a aktualizace této funkce tak, aby fungovala se změnami dotazů provedenými v EF Core 3,0 je významná práce.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-731">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="bf8ed-732">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-732">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-733">Doporučujeme aktualizovat na novější verzi SQL Server nebo pomocí vyšší úrovně kompatibility, aby byl vygenerovaný SQL podporován.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-733">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="bf8ed-734">To znamená, že pokud to nemůžete udělat, [komentář k problému](https://github.com/aspnet/EntityFrameworkCore/issues/16400) s sledováním najdete v podrobnostech.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-734">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="bf8ed-735">Toto rozhodnutí můžeme znovu navštívit na základě zpětné vazby.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-735">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="bf8ed-736">Informace o rozšíření/metadata se odebraly z IDbContextOptionsExtension.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-736">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="bf8ed-737">Sledování problému #16119</span><span class="sxs-lookup"><span data-stu-id="bf8ed-737">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="bf8ed-738">Tato změna je představena ve verzi EF Core 3,0-Preview 7.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-738">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="bf8ed-739">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-739">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-740">`IDbContextOptionsExtension`obsažené metody pro poskytování metadat o rozšíření.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-740">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="bf8ed-741">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-741">**New behavior**</span></span>

<span data-ttu-id="bf8ed-742">Tyto metody byly přesunuty na novou `DbContextOptionsExtensionInfo` abstraktní základní třídu, která je vrácena z nové `IDbContextOptionsExtension.Info` vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-742">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="bf8ed-743">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-743">**Why**</span></span>

<span data-ttu-id="bf8ed-744">V rámci vydání od 2,0 do 3,0 jsme potřebovali přidat nebo změnit tyto metody několikrát.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-744">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="bf8ed-745">Rozbalením do nové abstraktní základní třídy bude snazší vytvořit tyto změny bez přerušení stávajících rozšíření.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-745">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="bf8ed-746">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-746">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-747">Aktualizovat rozšíření tak, aby následovala nový vzor.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-747">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="bf8ed-748">Příklady naleznete v mnoha implementacích `IDbContextOptionsExtension` pro různé druhy rozšíření ve zdrojovém kódu EF Core.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-748">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="bf8ed-749">LogQueryPossibleExceptionWithAggregateOperator se přejmenovalo.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-749">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="bf8ed-750">Sledování problému #10985</span><span class="sxs-lookup"><span data-stu-id="bf8ed-750">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="bf8ed-751">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-751">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="bf8ed-752">**Mění**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-752">**Change**</span></span>

<span data-ttu-id="bf8ed-753">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator`byla přejmenována `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`na.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-753">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="bf8ed-754">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-754">**Why**</span></span>

<span data-ttu-id="bf8ed-755">Zarovná pojmenování této události varování se všemi ostatními událostmi upozornění.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-755">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="bf8ed-756">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-756">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-757">Použijte nový název.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-757">Use the new name.</span></span> <span data-ttu-id="bf8ed-758">(Všimněte si, že číslo ID události se nezměnilo.)</span><span class="sxs-lookup"><span data-stu-id="bf8ed-758">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="bf8ed-759">Vysvětlení rozhraní API pro názvy omezení cizího klíče</span><span class="sxs-lookup"><span data-stu-id="bf8ed-759">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="bf8ed-760">Sledování problému #10730</span><span class="sxs-lookup"><span data-stu-id="bf8ed-760">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="bf8ed-761">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-761">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="bf8ed-762">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-762">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-763">Před EF Core 3,0 se názvy omezení cizího klíče odkazovaly jenom na "název".</span><span class="sxs-lookup"><span data-stu-id="bf8ed-763">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="bf8ed-764">Příklad:</span><span class="sxs-lookup"><span data-stu-id="bf8ed-764">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="bf8ed-765">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-765">**New behavior**</span></span>

<span data-ttu-id="bf8ed-766">Počínaje EF Core 3,0 se názvy omezení cizích klíčů teď označují jako "název omezení".</span><span class="sxs-lookup"><span data-stu-id="bf8ed-766">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="bf8ed-767">Příklad:</span><span class="sxs-lookup"><span data-stu-id="bf8ed-767">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="bf8ed-768">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-768">**Why**</span></span>

<span data-ttu-id="bf8ed-769">Tato změna přináší konzistenci pro pojmenování v této oblasti a také vysvětluje, že se jedná o název omezení cizího klíče, a nikoli název sloupce nebo vlastnosti, ve kterém je definován cizí klíč.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-769">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="bf8ed-770">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-770">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-771">Použijte nový název.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-771">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="bf8ed-772">IRelationalDatabaseCreator. HasTables/HasTablesAsync byly zveřejněny.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-772">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="bf8ed-773">Sledování problému #15997</span><span class="sxs-lookup"><span data-stu-id="bf8ed-773">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="bf8ed-774">Tato změna je představena ve verzi EF Core 3,0-Preview 7.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-774">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="bf8ed-775">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-775">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-776">Před EF Core 3,0 byly tyto metody chráněné.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-776">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="bf8ed-777">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-777">**New behavior**</span></span>

<span data-ttu-id="bf8ed-778">Počínaje EF Core 3,0 jsou tyto metody veřejné.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-778">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="bf8ed-779">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-779">**Why**</span></span>

<span data-ttu-id="bf8ed-780">Tyto metody jsou používány EF k určení, jestli je databáze vytvořená, ale prázdná.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-780">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="bf8ed-781">To může být užitečné taky od vnějšího EF při určování, jestli se mají migrace použít.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-781">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="bf8ed-782">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-782">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-783">Změňte přístupnost všech přepsání.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-783">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="bf8ed-784">Microsoft. EntityFrameworkCore. Design je teď balíček DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-784">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="bf8ed-785">Sledování problému #11506</span><span class="sxs-lookup"><span data-stu-id="bf8ed-785">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="bf8ed-786">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-786">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="bf8ed-787">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-787">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-788">Před EF Core 3,0 byl Microsoft. EntityFrameworkCore. Design regulárním balíčkem NuGet, na kterém by mohly být na sestavení odkazovány projekty, které na něm závisejí.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-788">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="bf8ed-789">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-789">**New behavior**</span></span>

<span data-ttu-id="bf8ed-790">Počínaje EF Core 3,0 se jedná o balíček DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-790">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="bf8ed-791">To znamená, že závislost nebude nijak přesměrovat do jiných projektů a že již nemůžete ve výchozím nastavení odkazovat na své sestavení.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-791">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="bf8ed-792">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-792">**Why**</span></span>

<span data-ttu-id="bf8ed-793">Tento balíček se má použít jenom v době návrhu.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-793">This package is only intended to be used at design time.</span></span> <span data-ttu-id="bf8ed-794">Nasazené aplikace by neměli na ni odkazovat.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-794">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="bf8ed-795">Díky tomu, že balíček DevelopmentDependency, toto doporučení posiluje.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-795">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="bf8ed-796">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-796">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-797">Pokud potřebujete odkazovat na tento balíček, aby bylo možné přepsat EF Core chování při návrhu, můžete aktualizovat metadata položky aktualizovat PackageReference v projektu.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-797">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="bf8ed-798">Pokud se na balíček odkazuje přes Microsoft. EntityFrameworkCore. Tools, budete muset do balíčku přidat explicitní PackageReference, aby se změnila jeho metadata.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-798">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0-preview4.19216.3">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="bf8ed-799">SQLitePCL. Raw aktualizováno na verzi 2.0.0</span><span class="sxs-lookup"><span data-stu-id="bf8ed-799">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="bf8ed-800">Sledování problému #14824</span><span class="sxs-lookup"><span data-stu-id="bf8ed-800">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="bf8ed-801">Tato změna je představena ve verzi EF Core 3,0-Preview 7.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-801">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="bf8ed-802">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-802">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-803">Microsoft. EntityFrameworkCore. sqlite byl dřív závislý na 1.1.12 verze SQLitePCL. Raw.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-803">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="bf8ed-804">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-804">**New behavior**</span></span>

<span data-ttu-id="bf8ed-805">Náš balíček jsme aktualizovali tak, aby byl závislý na verzi 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-805">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="bf8ed-806">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-806">**Why**</span></span>

<span data-ttu-id="bf8ed-807">2\.0.0 verze SQLitePCL. Raw TARGETS .NET Standard 2,0.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-807">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="bf8ed-808">Dříve cílí na .NET Standard 1,1, které vyžadovaly, aby se při práci vytvořil velký uzávěr přenosných balíčků.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-808">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="bf8ed-809">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-809">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-810">SQLitePCL. Raw 2.0.0 verze obsahuje některé zásadní změny.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-810">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="bf8ed-811">Podrobnosti najdete v poznámkách k [verzi](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) .</span><span class="sxs-lookup"><span data-stu-id="bf8ed-811">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="bf8ed-812">NetTopologySuite aktualizace na verzi 2.0.0</span><span class="sxs-lookup"><span data-stu-id="bf8ed-812">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="bf8ed-813">Sledování problému #14825</span><span class="sxs-lookup"><span data-stu-id="bf8ed-813">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="bf8ed-814">Tato změna je představena ve verzi EF Core 3,0-Preview 7.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-814">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="bf8ed-815">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-815">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-816">Prostorové balíčky byly dříve závislé na 1.15.1 verze NetTopologySuite.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-816">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="bf8ed-817">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-817">**New behavior**</span></span>

<span data-ttu-id="bf8ed-818">Náš balíček jsme aktualizovali tak, aby byl závislý na verzi 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-818">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="bf8ed-819">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-819">**Why**</span></span>

<span data-ttu-id="bf8ed-820">2\.0.0 verze NetTopologySuite má za cíl řešit několik problémů s použitelností, ke kterým se EF Core uživatelé setkali.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-820">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="bf8ed-821">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-821">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-822">NetTopologySuite verze 2.0.0 obsahuje některé průlomové změny.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-822">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="bf8ed-823">Podrobnosti najdete v poznámkách k [verzi](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) .</span><span class="sxs-lookup"><span data-stu-id="bf8ed-823">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="bf8ed-824">Je nutné nakonfigurovat více dvojznačných relací odkazujících na sebe.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-824">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="bf8ed-825">Sledování problému #13573</span><span class="sxs-lookup"><span data-stu-id="bf8ed-825">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="bf8ed-826">Tato změna je představena ve verzi EF Core 3,0-Preview 6.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-826">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="bf8ed-827">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-827">**Old behavior**</span></span>

<span data-ttu-id="bf8ed-828">Typ entity s více jednosměrnou navigační vlastností a s vyhovující FKs byl nesprávně nakonfigurován jako jeden vztah.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-828">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="bf8ed-829">Příklad:</span><span class="sxs-lookup"><span data-stu-id="bf8ed-829">For example:</span></span>

```C#
public class User 
{
        public Guid Id { get; set; }
        public User CreatedBy { get; set; }
        public User UpdatedBy { get; set; }
        public Guid CreatedById { get; set; }
        public Guid? UpdatedById { get; set; }
}
```

<span data-ttu-id="bf8ed-830">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-830">**New behavior**</span></span>

<span data-ttu-id="bf8ed-831">Tento scénář je nyní zjištěn v sestavování modelu a je vyvolána výjimka označující, že je model dvojznačný.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-831">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="bf8ed-832">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-832">**Why**</span></span>

<span data-ttu-id="bf8ed-833">Výsledný model byl nejednoznačný a pravděpodobně bude pro tento případ obvykle špatný.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-833">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="bf8ed-834">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="bf8ed-834">**Mitigations**</span></span>

<span data-ttu-id="bf8ed-835">Použijte úplnou konfiguraci relace.</span><span class="sxs-lookup"><span data-stu-id="bf8ed-835">Use full configuration of the relationship.</span></span> <span data-ttu-id="bf8ed-836">Příklad:</span><span class="sxs-lookup"><span data-stu-id="bf8ed-836">For example:</span></span>

```C#
modelBuilder
     .Entity<User>()
     .HasOne(e => e.CreatedBy)
     .WithMany();
 
 modelBuilder
     .Entity<User>()
     .HasOne(e => e.UpdatedBy)
     .WithMany();
```
