---
title: Přerušující změny v EF Core 3,0 – EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 884cc6611b986fb213d99d3d2fc69d7bebe34aa2
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565327"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="d9bf8-102">Přerušující změny zahrnuté v EF Core 3,0 (aktuálně ve verzi Preview)</span><span class="sxs-lookup"><span data-stu-id="d9bf8-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d9bf8-103">Počítejte s tím, že sady funkcí a plány budoucích verzí se vždycky mění, a i když se pokusíme tuto stránku uchovávat v aktuálním stavu, nemusí se po celou dobu projevit naše nejnovější plány.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="d9bf8-104">Následující změny rozhraní API a chování mají možnost rušit aplikace vyvinuté pro EF Core 2.2. x při jejich upgradu na 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="d9bf8-105">Změny, které očekáváme jenom o to, aby ovlivnili pouze poskytovatele databází, jsou popsané v části [změny zprostředkovatele](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="d9bf8-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="d9bf8-106">Přerušení nových funkcí zavedených z jedné verze 3,0 Preview do jiné 3,0 verze Preview zde nejsou dokumentovány.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="summary"></a><span data-ttu-id="d9bf8-107">Souhrn</span><span class="sxs-lookup"><span data-stu-id="d9bf8-107">Summary</span></span>

| <span data-ttu-id="d9bf8-108">**Zásadní změna**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-108">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="d9bf8-109">**Dopad**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-109">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="d9bf8-110">Dotazy LINQ již nejsou vyhodnocovány na klientovi.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-110">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="d9bf8-111">Vysoká</span><span class="sxs-lookup"><span data-stu-id="d9bf8-111">High</span></span>       |
| [<span data-ttu-id="d9bf8-112">EF Core 3,0 cíle .NET Standard 2,1 místo .NET Standard 2,0</span><span class="sxs-lookup"><span data-stu-id="d9bf8-112">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="d9bf8-113">Vysoká</span><span class="sxs-lookup"><span data-stu-id="d9bf8-113">High</span></span>      |
| [<span data-ttu-id="d9bf8-114">EF Core nástroj příkazového řádku dotnet EF již není součástí .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="d9bf8-114">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="d9bf8-115">Vysoká</span><span class="sxs-lookup"><span data-stu-id="d9bf8-115">High</span></span>      |
| [<span data-ttu-id="d9bf8-116">Z tabulek, ExecuteSql a ExecuteSqlAsync byly přejmenovány.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-116">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="d9bf8-117">Vysoká</span><span class="sxs-lookup"><span data-stu-id="d9bf8-117">High</span></span>      |
| [<span data-ttu-id="d9bf8-118">Typy dotazů jsou konsolidovány s typy entit</span><span class="sxs-lookup"><span data-stu-id="d9bf8-118">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="d9bf8-119">Vysoká</span><span class="sxs-lookup"><span data-stu-id="d9bf8-119">High</span></span>      |
| [<span data-ttu-id="d9bf8-120">Entity Framework Core už není součástí sdílené ASP.NET Core architektury.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-120">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="d9bf8-121">Střední</span><span class="sxs-lookup"><span data-stu-id="d9bf8-121">Medium</span></span>      |
| [<span data-ttu-id="d9bf8-122">Odstranění kaskádových operací se teď ve výchozím nastavení provádí hned.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-122">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="d9bf8-123">Střední</span><span class="sxs-lookup"><span data-stu-id="d9bf8-123">Medium</span></span>      |
| [<span data-ttu-id="d9bf8-124">DeleteBehavior. restrict má sémantiku čištění.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-124">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="d9bf8-125">Střední</span><span class="sxs-lookup"><span data-stu-id="d9bf8-125">Medium</span></span>      |
| [<span data-ttu-id="d9bf8-126">Změnilo se konfigurační rozhraní API pro vztahy vlastněných typů.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-126">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="d9bf8-127">Střední</span><span class="sxs-lookup"><span data-stu-id="d9bf8-127">Medium</span></span>      |
| [<span data-ttu-id="d9bf8-128">Každá vlastnost používá nezávislou generaci celočíselného klíče v paměti.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-128">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="d9bf8-129">Střední</span><span class="sxs-lookup"><span data-stu-id="d9bf8-129">Medium</span></span>      |
| [<span data-ttu-id="d9bf8-130">Žádné dotazy pro sledování neprovádějí překlad identity</span><span class="sxs-lookup"><span data-stu-id="d9bf8-130">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="d9bf8-131">Střední</span><span class="sxs-lookup"><span data-stu-id="d9bf8-131">Medium</span></span>      |
| [<span data-ttu-id="d9bf8-132">Změny rozhraní API pro metadata</span><span class="sxs-lookup"><span data-stu-id="d9bf8-132">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="d9bf8-133">Střední</span><span class="sxs-lookup"><span data-stu-id="d9bf8-133">Medium</span></span>      |
| [<span data-ttu-id="d9bf8-134">Změny rozhraní API pro konkrétního zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="d9bf8-134">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="d9bf8-135">Střední</span><span class="sxs-lookup"><span data-stu-id="d9bf8-135">Medium</span></span>      |
| [<span data-ttu-id="d9bf8-136">UseRowNumberForPaging se odebral.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-136">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="d9bf8-137">Střední</span><span class="sxs-lookup"><span data-stu-id="d9bf8-137">Medium</span></span>      |
| [<span data-ttu-id="d9bf8-138">Metody Z tabulek se dají zadat jenom v kořenech dotazů.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-138">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="d9bf8-139">Nízká</span><span class="sxs-lookup"><span data-stu-id="d9bf8-139">Low</span></span>      |
| [<span data-ttu-id="d9bf8-140">~~Provádění dotazu se protokoluje na úrovni ladění~~ . Vrátit</span><span class="sxs-lookup"><span data-stu-id="d9bf8-140">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="d9bf8-141">Nízká</span><span class="sxs-lookup"><span data-stu-id="d9bf8-141">Low</span></span>      |
| [<span data-ttu-id="d9bf8-142">Dočasné hodnoty klíčů už nejsou nastavené na instance entit.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-142">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="d9bf8-143">Nízká</span><span class="sxs-lookup"><span data-stu-id="d9bf8-143">Low</span></span>      |
| [<span data-ttu-id="d9bf8-144">DetectChanges respektuje hodnoty klíčů generované úložištěm.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-144">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="d9bf8-145">Nízká</span><span class="sxs-lookup"><span data-stu-id="d9bf8-145">Low</span></span>      |
| [<span data-ttu-id="d9bf8-146">Závislé entity, které sdílí tabulku s objektem zabezpečení, jsou teď volitelné.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-146">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="d9bf8-147">Nízká</span><span class="sxs-lookup"><span data-stu-id="d9bf8-147">Low</span></span>      |
| [<span data-ttu-id="d9bf8-148">Všechny entity sdílející tabulku se sloupcem souběžného tokenu musí být namapovány na vlastnost.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-148">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="d9bf8-149">Nízká</span><span class="sxs-lookup"><span data-stu-id="d9bf8-149">Low</span></span>      |
| [<span data-ttu-id="d9bf8-150">Zděděné vlastnosti z nemapovaných typů jsou nyní namapovány na jeden sloupec pro všechny odvozené typy.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-150">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="d9bf8-151">Nízká</span><span class="sxs-lookup"><span data-stu-id="d9bf8-151">Low</span></span>      |
| [<span data-ttu-id="d9bf8-152">Konvence vlastností cizího klíče už neodpovídá stejnému názvu jako vlastnost Principal.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-152">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="d9bf8-153">Nízká</span><span class="sxs-lookup"><span data-stu-id="d9bf8-153">Low</span></span>      |
| [<span data-ttu-id="d9bf8-154">Připojení k databázi je teď zavřené, pokud už nepoužíváte dřív, než se dokončí jeho objekt TransactionScope.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-154">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="d9bf8-155">Nízká</span><span class="sxs-lookup"><span data-stu-id="d9bf8-155">Low</span></span>      |
| [<span data-ttu-id="d9bf8-156">Ve výchozím nastavení se používají pole pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-156">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="d9bf8-157">Nízká</span><span class="sxs-lookup"><span data-stu-id="d9bf8-157">Low</span></span>      |
| [<span data-ttu-id="d9bf8-158">Vyvolat, zda je nalezeno více kompatibilních zálohovaných polí</span><span class="sxs-lookup"><span data-stu-id="d9bf8-158">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="d9bf8-159">Nízká</span><span class="sxs-lookup"><span data-stu-id="d9bf8-159">Low</span></span>      |
| [<span data-ttu-id="d9bf8-160">Názvy vlastností pouze pro pole se musí shodovat s názvem pole.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-160">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="d9bf8-161">Nízká</span><span class="sxs-lookup"><span data-stu-id="d9bf8-161">Low</span></span>      |
| [<span data-ttu-id="d9bf8-162">AddDbContext/AddDbContextPool už nevolá AddLogging a AddMemoryCache.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-162">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="d9bf8-163">Nízká</span><span class="sxs-lookup"><span data-stu-id="d9bf8-163">Low</span></span>      |
| [<span data-ttu-id="d9bf8-164">DbContext. entry teď provádí místní DetectChanges.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-164">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="d9bf8-165">Nízká</span><span class="sxs-lookup"><span data-stu-id="d9bf8-165">Low</span></span>      |
| [<span data-ttu-id="d9bf8-166">Klíče řetězce a pole bajtů nejsou ve výchozím nastavení generovány klientem.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-166">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="d9bf8-167">Nízká</span><span class="sxs-lookup"><span data-stu-id="d9bf8-167">Low</span></span>      |
| [<span data-ttu-id="d9bf8-168">ILoggerFactory je teď služba s vymezeným oborem.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-168">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="d9bf8-169">Nízká</span><span class="sxs-lookup"><span data-stu-id="d9bf8-169">Low</span></span>      |
| [<span data-ttu-id="d9bf8-170">Opožděné načítání proxy serverů už nepředpokládá navigační vlastnosti, jsou plně načtené.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-170">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="d9bf8-171">Nízká</span><span class="sxs-lookup"><span data-stu-id="d9bf8-171">Low</span></span>      |
| [<span data-ttu-id="d9bf8-172">Nadměrné vytváření interních zprostředkovatelů služeb je teď ve výchozím nastavení chyba.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-172">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="d9bf8-173">Nízká</span><span class="sxs-lookup"><span data-stu-id="d9bf8-173">Low</span></span>      |
| [<span data-ttu-id="d9bf8-174">Nové chování pro HasOne/HasMany se volá s jedním řetězcem.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-174">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="d9bf8-175">Nízká</span><span class="sxs-lookup"><span data-stu-id="d9bf8-175">Low</span></span>      |
| [<span data-ttu-id="d9bf8-176">Návratový typ pro několik asynchronních metod byl změněn z úlohy na ValueTask</span><span class="sxs-lookup"><span data-stu-id="d9bf8-176">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="d9bf8-177">Nízká</span><span class="sxs-lookup"><span data-stu-id="d9bf8-177">Low</span></span>      |
| [<span data-ttu-id="d9bf8-178">Relační: anotace TypeMapping je nyní pouze TypeMapping</span><span class="sxs-lookup"><span data-stu-id="d9bf8-178">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="d9bf8-179">Nízká</span><span class="sxs-lookup"><span data-stu-id="d9bf8-179">Low</span></span>      |
| [<span data-ttu-id="d9bf8-180">ToTable na odvozeném typu vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-180">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="d9bf8-181">Nízká</span><span class="sxs-lookup"><span data-stu-id="d9bf8-181">Low</span></span>      |
| [<span data-ttu-id="d9bf8-182">EF Core už neposílá direktivu pragma pro vynucení KOFK SQLite.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-182">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="d9bf8-183">Nízká</span><span class="sxs-lookup"><span data-stu-id="d9bf8-183">Low</span></span>      |
| [<span data-ttu-id="d9bf8-184">Microsoft. EntityFrameworkCore. sqlite teď závisí na SQLitePCLRaw. bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="d9bf8-184">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="d9bf8-185">Nízká</span><span class="sxs-lookup"><span data-stu-id="d9bf8-185">Low</span></span>      |
| [<span data-ttu-id="d9bf8-186">Hodnoty GUID se teď ukládají jako TEXT na SQLite.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-186">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="d9bf8-187">Nízká</span><span class="sxs-lookup"><span data-stu-id="d9bf8-187">Low</span></span>      |
| [<span data-ttu-id="d9bf8-188">Hodnoty char se teď ukládají jako TEXT na SQLite.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-188">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="d9bf8-189">Nízká</span><span class="sxs-lookup"><span data-stu-id="d9bf8-189">Low</span></span>      |
| [<span data-ttu-id="d9bf8-190">ID migrace se teď generují pomocí kalendáře invariantní jazykové verze.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-190">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="d9bf8-191">Nízká</span><span class="sxs-lookup"><span data-stu-id="d9bf8-191">Low</span></span>      |
| [<span data-ttu-id="d9bf8-192">Informace o rozšíření/metadata se odebraly z IDbContextOptionsExtension.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-192">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="d9bf8-193">Nízká</span><span class="sxs-lookup"><span data-stu-id="d9bf8-193">Low</span></span>      |
| [<span data-ttu-id="d9bf8-194">LogQueryPossibleExceptionWithAggregateOperator se přejmenovalo.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-194">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="d9bf8-195">Nízká</span><span class="sxs-lookup"><span data-stu-id="d9bf8-195">Low</span></span>      |
| [<span data-ttu-id="d9bf8-196">Vysvětlení rozhraní API pro názvy omezení cizího klíče</span><span class="sxs-lookup"><span data-stu-id="d9bf8-196">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="d9bf8-197">Nízká</span><span class="sxs-lookup"><span data-stu-id="d9bf8-197">Low</span></span>      |
| [<span data-ttu-id="d9bf8-198">IRelationalDatabaseCreator. HasTables/HasTablesAsync byly zveřejněny.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-198">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="d9bf8-199">Nízká</span><span class="sxs-lookup"><span data-stu-id="d9bf8-199">Low</span></span>      |
| [<span data-ttu-id="d9bf8-200">Microsoft. EntityFrameworkCore. Design je teď balíček DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-200">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="d9bf8-201">Nízká</span><span class="sxs-lookup"><span data-stu-id="d9bf8-201">Low</span></span>      |
| [<span data-ttu-id="d9bf8-202">SQLitePCL. Raw aktualizováno na verzi 2.0.0</span><span class="sxs-lookup"><span data-stu-id="d9bf8-202">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="d9bf8-203">Nízká</span><span class="sxs-lookup"><span data-stu-id="d9bf8-203">Low</span></span>      |
| [<span data-ttu-id="d9bf8-204">NetTopologySuite aktualizace na verzi 2.0.0</span><span class="sxs-lookup"><span data-stu-id="d9bf8-204">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="d9bf8-205">Nízká</span><span class="sxs-lookup"><span data-stu-id="d9bf8-205">Low</span></span>      |
| [<span data-ttu-id="d9bf8-206">Je nutné nakonfigurovat více dvojznačných relací odkazujících na sebe.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-206">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="d9bf8-207">Nízká</span><span class="sxs-lookup"><span data-stu-id="d9bf8-207">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="d9bf8-208">Dotazy LINQ již nejsou vyhodnocovány na klientovi.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-208">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="d9bf8-209">[Sledování problému #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[taky zobrazit problémy #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="d9bf8-209">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="d9bf8-210">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-210">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="d9bf8-211">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-211">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-212">Před 3,0, když EF Core nedokázal převést výraz, který byl součástí dotazu na buď SQL, nebo parametr, automaticky vyhodnotí výraz na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-212">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="d9bf8-213">Ve výchozím nastavení vyhodnocování klientů u potenciálně nákladných výrazů aktivovalo pouze upozornění.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-213">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="d9bf8-214">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-214">**New behavior**</span></span>

<span data-ttu-id="d9bf8-215">Počínaje 3,0 EF Core v rámci projekce nejvyšší úrovně (posledního `Select()` volání v dotazu) jenom výrazy vyhodnotit na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-215">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="d9bf8-216">Pokud výrazy v jakékoli jiné části dotazu nelze převést na buď SQL, nebo parametr, je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-216">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="d9bf8-217">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-217">**Why**</span></span>

<span data-ttu-id="d9bf8-218">Automatické hodnocení dotazů umožňuje provádět mnoho dotazů i v případě, že není možné přeložit jejich důležité části.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-218">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="d9bf8-219">Toto chování může vést k neočekávanému a potenciálně škodlivému chování, které může být jen zřejmé v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-219">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="d9bf8-220">Například podmínka ve `Where()` volání, která se nedá přeložit, může způsobit přenesení všech řádků z databázového serveru a filtr, který se má použít na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-220">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="d9bf8-221">Tato situace může snadno přejít zpět, pokud tabulka obsahuje jenom několik řádků ve vývoji, ale při přesunu aplikace do produkčního prostředí zasáhnout, kde tabulka může obsahovat miliony řádků.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-221">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="d9bf8-222">Upozornění na vyhodnocení klientů se také během vývoje ukázala jako příliš jednoduchá.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-222">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="d9bf8-223">Kromě toho může automatické hodnocení klienta vést k problémům s tím, že vylepšení překladu dotazů pro konkrétní výrazy způsobilo nezamýšlené neúmyslné změny mezi verzemi.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-223">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="d9bf8-224">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-224">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-225">Pokud dotaz nelze plně přeložit, pak buď Přepište dotaz do formuláře, který lze přeložit, nebo použijte `AsEnumerable()`, `ToList()`nebo podobným způsobem explicitně přeneste data zpět do klienta, kde lze následně dále zpracovávat pomocí LINQ-to-Objects.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-225">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="d9bf8-226">EF Core 3,0 cíle .NET Standard 2,1 místo .NET Standard 2,0</span><span class="sxs-lookup"><span data-stu-id="d9bf8-226">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="d9bf8-227">Sledování problému #15498</span><span class="sxs-lookup"><span data-stu-id="d9bf8-227">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

<span data-ttu-id="d9bf8-228">Tato změna je představena ve verzi EF Core 3,0-Preview 7.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-228">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="d9bf8-229">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-229">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-230">Před 3,0 EF Core cílené .NET Standard 2,0 a spustí se na všech platformách, které podporují tento standard, včetně .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-230">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="d9bf8-231">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-231">**New behavior**</span></span>

<span data-ttu-id="d9bf8-232">Počínaje 3,0 se EF Core cíle .NET Standard 2,1 a spustí se na všech platformách, které podporují tento standard.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-232">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="d9bf8-233">To nezahrnuje .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-233">This does not include .NET Framework.</span></span>

<span data-ttu-id="d9bf8-234">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-234">**Why**</span></span>

<span data-ttu-id="d9bf8-235">Toto je součást strategického rozhodnutí napříč technologiemi .NET a zaměřuje se na energii na platformě .NET Core a dalších moderních platformách .NET, jako je Xamarin.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-235">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="d9bf8-236">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-236">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-237">Zvažte přechod na moderní platformu .NET.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-237">Consider moving to a modern .NET platform.</span></span> <span data-ttu-id="d9bf8-238">Pokud to není možné, pak pokračujte v používání EF Core 2,1 nebo EF Core 2,2, které podporují .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-238">If this is not possible, then continue to use EF Core 2.1 or EF Core 2.2, both of which support .NET Framework.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="d9bf8-239">Entity Framework Core už není součástí sdílené ASP.NET Core architektury.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-239">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="d9bf8-240">Sledování oznámení o problémech # 325</span><span class="sxs-lookup"><span data-stu-id="d9bf8-240">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="d9bf8-241">Tato změna je představena ve verzi ASP.NET Core 3,0-Preview 1.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-241">This change is introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="d9bf8-242">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-242">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-243">Před ASP.NET Core 3,0 se při přidání odkazu na balíček do `Microsoft.AspNetCore.App` nebo `Microsoft.AspNetCore.All`zahrnovala EF Core a někteří poskytovatelé EF Core dat jako poskytovatel SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-243">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="d9bf8-244">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-244">**New behavior**</span></span>

<span data-ttu-id="d9bf8-245">Počínaje 3,0 ASP.NET Core sdílené rozhraní nezahrnuje EF Core nebo žádné poskytovatele EF Core dat.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-245">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="d9bf8-246">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-246">**Why**</span></span>

<span data-ttu-id="d9bf8-247">Před touto změnou EF Core nutné jiné kroky v závislosti na tom, zda je aplikace cílena ASP.NET Core a SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-247">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="d9bf8-248">Upgrade ASP.NET Core taky vynutil upgrade EF Core a poskytovatele SQL Server, který není vždycky žádoucí.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-248">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="d9bf8-249">V důsledku této změny je prostředí získání EF Core stejné u všech poskytovatelů, podporovaných implementací rozhraní .NET a typů aplikací.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-249">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="d9bf8-250">Vývojáři teď můžou přesně řídit, když EF Core a EF Core se upgradují poskytovatelé dat.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-250">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="d9bf8-251">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-251">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-252">Chcete-li použít EF Core v aplikaci ASP.NET Core 3,0 nebo v jakékoli jiné podporované aplikaci, explicitně přidejte odkaz na balíček EF Coreho poskytovatele databáze, který bude aplikace používat.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-252">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="d9bf8-253">EF Core nástroj příkazového řádku dotnet EF již není součástí .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="d9bf8-253">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="d9bf8-254">Sledování problému #14016</span><span class="sxs-lookup"><span data-stu-id="d9bf8-254">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="d9bf8-255">Tato změna je představena v EF Core 3,0-Preview 4 a odpovídající verzi .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-255">This change is introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="d9bf8-256">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-256">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-257">Před 3,0 `dotnet ef` byl nástroj součástí .NET Core SDK a byl snadno dostupný pro použití z příkazového řádku z libovolného projektu bez nutnosti dalších kroků.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-257">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="d9bf8-258">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-258">**New behavior**</span></span>

<span data-ttu-id="d9bf8-259">Počínaje 3,0 sada .NET SDK nezahrnuje `dotnet ef` nástroj, takže před tím, než ho budete moct použít, je nutné explicitně nainstalovat jako místní nebo globální nástroj.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-259">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="d9bf8-260">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-260">**Why**</span></span>

<span data-ttu-id="d9bf8-261">Tato změna nám umožňuje distribuovat a aktualizovat `dotnet ef` jako běžný nástroj rozhraní .NET CLI v NuGet, a to v souladu se skutečností, že EF Core 3,0 je také vždy distribuován jako balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-261">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="d9bf8-262">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-262">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-263">Aby bylo možné spravovat migrace nebo uživatelské rozhraní a `DbContext`, nainstalujte `dotnet-ef` nástroj jako globální nástroj:</span><span class="sxs-lookup"><span data-stu-id="d9bf8-263">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef --version 3.0.0-*
  ```

<span data-ttu-id="d9bf8-264">Můžete také získat místní nástroj při obnovování závislostí projektu, který deklaruje jako závislost nástrojů pomocí [souboru manifestu nástroje](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="d9bf8-264">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="d9bf8-265">Z tabulek, ExecuteSql a ExecuteSqlAsync byly přejmenovány.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-265">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="d9bf8-266">Sledování problému #10996</span><span class="sxs-lookup"><span data-stu-id="d9bf8-266">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="d9bf8-267">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-267">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="d9bf8-268">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-268">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-269">Před EF Core 3,0 byly tyto názvy metod přetíženy, aby fungovaly s normálním řetězcem nebo řetězcem, které by měly být interpolované na SQL a parametry.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-269">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="d9bf8-270">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-270">**New behavior**</span></span>

<span data-ttu-id="d9bf8-271">Počínaje EF Core 3,0, použijte `FromSqlRaw`, `ExecuteSqlRaw`a `ExecuteSqlRawAsync` k vytvoření parametrizovaného dotazu, kde jsou parametry předány nezávisle na řetězci dotazu.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-271">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="d9bf8-272">Příklad:</span><span class="sxs-lookup"><span data-stu-id="d9bf8-272">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="d9bf8-273">Použijte `FromSqlInterpolated`, `ExecuteSqlInterpolated` a`ExecuteSqlInterpolatedAsync` k vytvoření parametrizovaného dotazu, kde jsou parametry předány jako součást interpolované řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-273">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="d9bf8-274">Příklad:</span><span class="sxs-lookup"><span data-stu-id="d9bf8-274">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="d9bf8-275">Všimněte si, že obě výše uvedené dotazy vytvoří stejný parametrizovaný SQL se stejnými parametry SQL.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-275">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="d9bf8-276">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-276">**Why**</span></span>

<span data-ttu-id="d9bf8-277">Přetížení metody, jako to, usnadňuje náhodné volání nezpracované řetězcové metody, pokud by záměr byl zavolat interpolovaná řetězcová metoda a druhá možnost kolem.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-277">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="d9bf8-278">To může vést k tomu, že dotazy nejsou parametrizované, pokud by měly být.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-278">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="d9bf8-279">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-279">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-280">Přepněte na použití nových názvů metod.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-280">Switch to use the new method names.</span></span>

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="d9bf8-281">Metody Z tabulek se dají zadat jenom v kořenech dotazů.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-281">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="d9bf8-282">Sledování problému #15704</span><span class="sxs-lookup"><span data-stu-id="d9bf8-282">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="d9bf8-283">Tato změna je představena ve verzi EF Core 3,0-Preview 6.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-283">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="d9bf8-284">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-284">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-285">Před EF Core 3,0 `FromSql` lze metodu zadat kdekoli v dotazu.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-285">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="d9bf8-286">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-286">**New behavior**</span></span>

<span data-ttu-id="d9bf8-287">Počínaje EF Core `FromSqlRaw` 3,0 lze nové metody a `FromSqlInterpolated` (které nahradí `FromSql`) zadat pouze v kořenech `DbSet<>`dotazu, tj. přímo na.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-287">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="d9bf8-288">Pokud se pokusíte zadat jiné místo jinak, dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-288">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="d9bf8-289">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-289">**Why**</span></span>

<span data-ttu-id="d9bf8-290">Určení `FromSql` kdekoli jinde než u a `DbSet` neobsahovalo žádné přidané významy nebo přidání hodnoty a může v některých scénářích způsobit nejednoznačnost.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-290">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="d9bf8-291">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-291">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-292">`FromSql`volání by se měla přesunout přímo na, `DbSet` na které se vztahují.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-292">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="d9bf8-293">Žádné dotazy pro sledování neprovádějí překlad identity</span><span class="sxs-lookup"><span data-stu-id="d9bf8-293">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="d9bf8-294">Sledování problému #13518</span><span class="sxs-lookup"><span data-stu-id="d9bf8-294">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="d9bf8-295">Tato změna je představena ve verzi EF Core 3,0-Preview 6.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-295">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="d9bf8-296">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-296">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-297">Před EF Core 3,0 se stejná instance entity používá pro všechny výskyty entity se zadaným typem a ID.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-297">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="d9bf8-298">To odpovídá chování sledovacích dotazů.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-298">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="d9bf8-299">Například tento dotaz:</span><span class="sxs-lookup"><span data-stu-id="d9bf8-299">For example, this query:</span></span>

```C#
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="d9bf8-300">vrátí stejnou `Category` instanci pro každý `Product` , který je spojen s danou kategorií.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-300">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="d9bf8-301">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-301">**New behavior**</span></span>

<span data-ttu-id="d9bf8-302">Počínaje EF Core 3,0 budou vytvořeny různé instance entit při výskytu entity se zadaným typem a ID na různých místech vráceného grafu.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-302">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="d9bf8-303">Například dotaz výše bude nyní vracet novou `Category` instanci pro každou `Product` , i když jsou ke stejné kategorii přidruženy dva produkty.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-303">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="d9bf8-304">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-304">**Why**</span></span>

<span data-ttu-id="d9bf8-305">Překlad identity (to znamená, že určení, že entita má stejný typ a ID jako dříve zjištěná entita) přidává další výkon a režii paměti.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-305">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="d9bf8-306">Obvykle se spustí čítač, aby se v prvním místě nepoužily žádné dotazy na sledování.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-306">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="d9bf8-307">I když může být v některých případech užitečný překlad identity, není potřeba, pokud se entity mají serializovat a odeslat klientovi, což je běžné pro žádné dotazy pro sledování.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-307">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="d9bf8-308">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-308">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-309">Pokud je vyžadováno rozlišení identity, použijte dotaz sledování.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-309">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="d9bf8-310">~~Provádění dotazu se protokoluje na úrovni ladění~~ . Vrátit</span><span class="sxs-lookup"><span data-stu-id="d9bf8-310">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="d9bf8-311">Sledování problému #14523</span><span class="sxs-lookup"><span data-stu-id="d9bf8-311">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="d9bf8-312">Tato změna se vrátí v EF Core 3,0-Preview 7.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-312">This change is reverted in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="d9bf8-313">Tuto změnu jsme vrátili, protože nová konfigurace v EF Core 3,0 umožňuje, aby byla úroveň protokolu pro libovolnou událost specifikována aplikací.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-313">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="d9bf8-314">Například chcete-li přepnout protokolování SQL na `Debug`, explicitně nakonfigurovat úroveň v `OnConfiguring` nebo `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="d9bf8-314">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="d9bf8-315">Dočasné hodnoty klíčů už nejsou nastavené na instance entit.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-315">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="d9bf8-316">Sledování problému #12378</span><span class="sxs-lookup"><span data-stu-id="d9bf8-316">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="d9bf8-317">Tato změna je představena ve verzi EF Core 3,0-Preview 2.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-317">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="d9bf8-318">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-318">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-319">Před EF Core 3,0 byly přiřazeny dočasné hodnoty ke všem klíčovým vlastnostem, které by později měly skutečnou hodnotu vygenerovanou databází.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-319">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="d9bf8-320">Obvykle jsou tyto dočasné hodnoty velkými zápornými čísly.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-320">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="d9bf8-321">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-321">**New behavior**</span></span>

<span data-ttu-id="d9bf8-322">Počínaje 3,0 EF Core ukládá hodnotu dočasného klíče jako součást informací o sledování entity a ponechá vlastnost klíče beze změny.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-322">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="d9bf8-323">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-323">**Why**</span></span>

<span data-ttu-id="d9bf8-324">Tato změna byla provedena, aby se předešlo omylům hodnotám klíčů v případě, že entita, která byla dříve sledována pomocí nějaké `DbContext` instance, je přesunuta do jiné `DbContext` instance.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-324">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="d9bf8-325">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-325">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-326">Aplikace, které přiřazují hodnoty primárního klíče k cizím klíčům k vytvoření přidružení mezi entitami, můžou záviset na starém chování, pokud jsou primární klíče generované úložištěm a patří do entit ve `Added` stavu.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-326">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="d9bf8-327">K tomu je možné se vyhnout:</span><span class="sxs-lookup"><span data-stu-id="d9bf8-327">This can be avoided by:</span></span>
* <span data-ttu-id="d9bf8-328">Nepoužívejte klíče generované úložištěm.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-328">Not using store-generated keys.</span></span>
* <span data-ttu-id="d9bf8-329">Nastavení vlastností navigace pro vytváření relací místo nastavení hodnot cizích klíčů.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-329">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="d9bf8-330">Získá aktuální dočasné hodnoty klíče z informací o sledování entity.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-330">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="d9bf8-331">Například vrátí dočasnou hodnotu, `context.Entry(blog).Property(e => e.Id).CurrentValue` i když `blog.Id` nebyla nastavena.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-331">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="d9bf8-332">DetectChanges respektuje hodnoty klíčů generované úložištěm.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-332">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="d9bf8-333">Sledování problému #14616</span><span class="sxs-lookup"><span data-stu-id="d9bf8-333">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="d9bf8-334">Tato změna je představena ve verzi EF Core 3,0-Preview 3.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-334">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="d9bf8-335">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-335">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-336">Předtím, než EF Core 3,0, nalezené `DetectChanges` Nesledované entity by byly sledovány `Added` ve stavu a vloženy jako nový řádek při `SaveChanges` volání.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-336">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="d9bf8-337">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-337">**New behavior**</span></span>

<span data-ttu-id="d9bf8-338">Počínaje EF Core 3,0 platí, že pokud entita používá vygenerované hodnoty klíčů a je nastavená nějaká hodnota klíče, bude se entita sledovat ve `Modified` stavu.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-338">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="d9bf8-339">To znamená, že se předpokládá, že řádek pro entitu existuje a že bude při `SaveChanges` volání aktualizována.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-339">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="d9bf8-340">Pokud hodnota klíče není nastavená, nebo pokud typ entity nepoužívá vygenerované klíče, bude se nová entita dál sledovat jako `Added` v předchozích verzích.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-340">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="d9bf8-341">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-341">**Why**</span></span>

<span data-ttu-id="d9bf8-342">Tato změna byla provedena tak, aby byla pro práci s nepřipojenými grafy entit při použití klíčů generovaných úložištěm snazší.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-342">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="d9bf8-343">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-343">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-344">Tato změna může přerušit aplikaci, pokud je typ entity nakonfigurovaný na používání vygenerovaných klíčů, ale hodnoty klíčů jsou explicitně nastavené pro nové instance.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-344">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="d9bf8-345">Opravou je explicitně nakonfigurovat klíčové vlastnosti tak, aby nepoužívaly vygenerované hodnoty.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-345">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="d9bf8-346">Například s rozhraním API Fluent:</span><span class="sxs-lookup"><span data-stu-id="d9bf8-346">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="d9bf8-347">Nebo s poznámkami k datům:</span><span class="sxs-lookup"><span data-stu-id="d9bf8-347">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="d9bf8-348">Odstranění kaskádových operací se teď ve výchozím nastavení provádí hned.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-348">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="d9bf8-349">Sledování problému #10114</span><span class="sxs-lookup"><span data-stu-id="d9bf8-349">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="d9bf8-350">Tato změna je představena ve verzi EF Core 3,0-Preview 3.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-350">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="d9bf8-351">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-351">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-352">Před 3,0 EF Core aplikovány kaskádové akce (odstraňování závislých entit, když se odstraní požadovaný objekt zabezpečení nebo když je vztah k požadovanému objektu zabezpečení vážně) nevznikl, dokud není voláno SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-352">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="d9bf8-353">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-353">**New behavior**</span></span>

<span data-ttu-id="d9bf8-354">Od 3,0 EF Core aplikuje kaskádové akce hned po zjištění spouštěcí podmínky.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-354">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="d9bf8-355">Například volání `context.Remove()` na odstranění objektu zabezpečení bude mít za následek, že jsou všechny sledované požadované závislé položky nastaveny také na `Deleted` hodnotu okamžitě.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-355">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="d9bf8-356">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-356">**Why**</span></span>

<span data-ttu-id="d9bf8-357">Tato změna byla provedena za účelem zlepšení prostředí pro scénáře vytváření datových vazeb a auditování, kde je důležité pochopit, které entity budou _před_ `SaveChanges` voláním odstraněny.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-357">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="d9bf8-358">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-358">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-359">Předchozí chování lze obnovit pomocí nastavení v `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-359">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="d9bf8-360">Příklad:</span><span class="sxs-lookup"><span data-stu-id="d9bf8-360">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="d9bf8-361">DeleteBehavior. restrict má sémantiku čištění.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-361">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="d9bf8-362">Sledování problému #12661</span><span class="sxs-lookup"><span data-stu-id="d9bf8-362">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="d9bf8-363">Tato změna je představena ve verzi EF Core 3,0-Preview 5.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-363">This change is introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="d9bf8-364">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-364">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-365">Před 3,0 se `DeleteBehavior.Restrict` v databázi vytvořily cizí klíče se `Restrict` sémantikou, ale zároveň se nezřetelně změnila interní oprava.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-365">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="d9bf8-366">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-366">**New behavior**</span></span>

<span data-ttu-id="d9bf8-367">Počínaje 3,0 `DeleteBehavior.Restrict` zajistí, že se cizí klíče vytvoří s `Restrict` sémantikou – to znamená bez kaskády; porušení omezení throw--bez vlivu na interní opravu EF.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-367">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="d9bf8-368">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-368">**Why**</span></span>

<span data-ttu-id="d9bf8-369">Tato změna byla provedena pro zlepšení prostředí pro použití `DeleteBehavior` intuitivním způsobem, a to bez neočekávaných vedlejších účinků.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-369">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="d9bf8-370">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-370">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-371">Předchozí chování lze obnovit pomocí `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-371">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="d9bf8-372">Typy dotazů jsou konsolidovány s typy entit</span><span class="sxs-lookup"><span data-stu-id="d9bf8-372">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="d9bf8-373">Sledování problému #14194</span><span class="sxs-lookup"><span data-stu-id="d9bf8-373">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="d9bf8-374">Tato změna je představena ve verzi EF Core 3,0-Preview 3.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-374">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="d9bf8-375">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-375">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-376">Před EF Core 3,0 byly [typy dotazů](xref:core/modeling/query-types) prostředkem pro dotazování na data, která nedefinují primární klíč strukturovaným způsobem.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-376">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="d9bf8-377">To znamená, že typ dotazu byl použit pro mapování typů entit bez klíčů (pravděpodobnější z pohledu, ale také z tabulky), zatímco byl použit regulární typ entity, když byl klíč k dispozici (pravděpodobnější z tabulky, ale případně z pohledu).</span><span class="sxs-lookup"><span data-stu-id="d9bf8-377">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="d9bf8-378">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-378">**New behavior**</span></span>

<span data-ttu-id="d9bf8-379">Typ dotazu se teď bude jednat jenom o typ entity bez primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-379">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="d9bf8-380">Typy entit bez klíčů mají stejné funkce jako typy dotazů v předchozích verzích.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-380">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="d9bf8-381">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-381">**Why**</span></span>

<span data-ttu-id="d9bf8-382">Tato změna byla provedena proto, aby se snížila nejasnost v souvislosti s typy dotazů.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-382">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="d9bf8-383">Konkrétně jsou bez klíčů typy entit a jsou ze své podstaty jen pro čtení, ale neměly by být použity pouze proto, že typ entity musí být jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-383">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="d9bf8-384">Podobně jsou často mapovány na zobrazení, ale to je pouze v případě, že zobrazení často nedefinují klíče.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-384">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="d9bf8-385">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-385">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-386">Následující části rozhraní API jsou teď zastaralé:</span><span class="sxs-lookup"><span data-stu-id="d9bf8-386">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="d9bf8-387">**`ModelBuilder.Query<>()`** – Místo `ModelBuilder.Entity<>().HasNoKey()` toho je nutné volat k označení typu entity, protože neobsahují žádné klíče.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-387">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="d9bf8-388">Tato služba by se ještě nenakonfigurovala pomocí konvence, aby nedocházelo k chybám konfigurace, když je primární klíč očekávaný, ale neodpovídá konvenci.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-388">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="d9bf8-389">**`DbQuery<>`** – Místo `DbSet<>` toho by se mělo použít.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-389">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="d9bf8-390">**`DbContext.Query<>()`** – Místo `DbContext.Set<>()` toho by se mělo použít.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-390">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="d9bf8-391">Změnilo se konfigurační rozhraní API pro vztahy vlastněných typů.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-391">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="d9bf8-392">[Sledování problému #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[sledování problému #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[#14153 sledování](https://github.com/aspnet/EntityFrameworkCore/issues/14153) problému</span><span class="sxs-lookup"><span data-stu-id="d9bf8-392">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="d9bf8-393">Tato změna je představena ve verzi EF Core 3,0-Preview 3.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-393">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="d9bf8-394">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-394">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-395">Před EF Core 3,0 byla konfigurace vztahu vlastníka provedena přímo po `OwnsOne` volání nebo. `OwnsMany`</span><span class="sxs-lookup"><span data-stu-id="d9bf8-395">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="d9bf8-396">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-396">**New behavior**</span></span>

<span data-ttu-id="d9bf8-397">Od EF Core 3,0 teď existuje Fluent API pro konfiguraci navigační vlastnosti pro vlastníka pomocí `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-397">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="d9bf8-398">Příklad:</span><span class="sxs-lookup"><span data-stu-id="d9bf8-398">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="d9bf8-399">Konfigurace vztahující se k vztahu mezi vlastníkem a vlastníkem by nyní měla být zřetězena `WithOwner()` podobně jako konfigurace ostatních vztahů.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-399">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="d9bf8-400">I když se konfigurace samotného vlastního typu bude i nadále zřetězit za `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-400">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="d9bf8-401">Příklad:</span><span class="sxs-lookup"><span data-stu-id="d9bf8-401">For example:</span></span>

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

<span data-ttu-id="d9bf8-402">Kromě toho `Entity()`, `HasOne()`že volání `Set()` ,, nebo s cílem cílového typu, nyní vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-402">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="d9bf8-403">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-403">**Why**</span></span>

<span data-ttu-id="d9bf8-404">Tato změna byla provedena za účelem vytvoření čisticího oddělení mezi konfigurací samotného typu a _vztahu k_ typu, který je vlastníkem.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-404">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="d9bf8-405">Tím se zase odeberou nejednoznačnosti a nejasnosti u metod, jako `HasForeignKey`je.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-405">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="d9bf8-406">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-406">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-407">Změňte konfiguraci vztahů vlastněných typů tak, aby používala novou plochu rozhraní API, jak je znázorněno v předchozím příkladu.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-407">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="d9bf8-408">Závislé entity, které sdílí tabulku s objektem zabezpečení, jsou teď volitelné.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-408">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="d9bf8-409">Sledování problému #9005</span><span class="sxs-lookup"><span data-stu-id="d9bf8-409">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="d9bf8-410">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-410">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="d9bf8-411">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-411">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-412">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="d9bf8-412">Consider the following model:</span></span>
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
<span data-ttu-id="d9bf8-413">Pokud `OrderDetails` je před EF Core 3,0 ve `Order` vlastnictví nebo explicitně namapována na stejnou tabulku `OrderDetails` , byla při přidávání nové `Order`instance vždy vyžadována instance.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-413">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="d9bf8-414">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-414">**New behavior**</span></span>

<span data-ttu-id="d9bf8-415">Počínaje 3,0 EF Core umožňuje přidat `Order` `OrderDetails` bez `OrderDetails` a mapování všech vlastností kromě primárního klíče na sloupce s možnou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-415">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="d9bf8-416">Při dotazování EF Core sady `OrderDetails` na `null` , pokud některá z jejích požadovaných vlastností nemá hodnotu, nebo pokud nemá žádné požadované vlastnosti kromě primárního klíče a všechny vlastnosti jsou `null`.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-416">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="d9bf8-417">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-417">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-418">Pokud má váš model sdílení tabulky závislé na všech volitelných sloupcích, ale u navigace, na kterou se odkazuje, se neočekává `null` , že by aplikace měla být upravena tak, aby zpracovávala případy, kdy je `null`navigace.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-418">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="d9bf8-419">Pokud to není možné, měla by být do daného typu entity přidána požadovaná vlastnost, nebo alespoň jedna vlastnost by k ní měla mít`null` přiřazenou hodnotu bez hodnoty.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-419">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="d9bf8-420">Všechny entity sdílející tabulku se sloupcem souběžného tokenu musí být namapovány na vlastnost.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-420">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="d9bf8-421">Sledování problému #14154</span><span class="sxs-lookup"><span data-stu-id="d9bf8-421">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="d9bf8-422">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-422">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="d9bf8-423">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-423">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-424">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="d9bf8-424">Consider the following model:</span></span>
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
<span data-ttu-id="d9bf8-425">Pokud `OrderDetails` je před EF Core 3,0, ve `Order` vlastnictví nebo explicitně namapovaných na `OrderDetails` stejnou tabulku, pak aktualizace nebude aktualizovat `Version` hodnotu u klienta a další aktualizace selže.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-425">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="d9bf8-426">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-426">**New behavior**</span></span>

<span data-ttu-id="d9bf8-427">Počínaje 3,0 EF Core rozšíří novou `Version` `Order` hodnotu, pokud je vlastní `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-427">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="d9bf8-428">V opačném případě je při ověřování modelu vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-428">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="d9bf8-429">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-429">**Why**</span></span>

<span data-ttu-id="d9bf8-430">Tato změna byla provedena, aby nedocházelo k zastaralé hodnotě tokenu souběžnosti, pokud je aktualizována pouze jedna z entit mapovaných na stejnou tabulku.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-430">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="d9bf8-431">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-431">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-432">Všechny entity, které sdílejí tabulku, musí obsahovat vlastnost, která je namapovaná na sloupec tokenu souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-432">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="d9bf8-433">Je možné, že ho vytvoříte ve stínovém stavu:</span><span class="sxs-lookup"><span data-stu-id="d9bf8-433">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="d9bf8-434">Zděděné vlastnosti z nemapovaných typů jsou nyní namapovány na jeden sloupec pro všechny odvozené typy.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-434">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="d9bf8-435">Sledování problému #13998</span><span class="sxs-lookup"><span data-stu-id="d9bf8-435">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="d9bf8-436">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-436">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="d9bf8-437">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-437">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-438">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="d9bf8-438">Consider the following model:</span></span>
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

<span data-ttu-id="d9bf8-439">Před EF Core 3,0 `ShippingAddress` bude vlastnost namapována na samostatné sloupce pro `BulkOrder` a `Order` ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-439">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="d9bf8-440">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-440">**New behavior**</span></span>

<span data-ttu-id="d9bf8-441">Počínaje 3,0 se EF Core pro `ShippingAddress`nedá vytvořit jenom jeden sloupec.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-441">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="d9bf8-442">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-442">**Why**</span></span>

<span data-ttu-id="d9bf8-443">Starý behavoir nebyl očekáván.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-443">The old behavoir was unexpected.</span></span>

<span data-ttu-id="d9bf8-444">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-444">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-445">Vlastnost může být stále explicitně namapována na samostatný sloupec odvozených typů:</span><span class="sxs-lookup"><span data-stu-id="d9bf8-445">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="d9bf8-446">Konvence vlastností cizího klíče už neodpovídá stejnému názvu jako vlastnost Principal.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-446">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="d9bf8-447">Sledování problému #13274</span><span class="sxs-lookup"><span data-stu-id="d9bf8-447">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="d9bf8-448">Tato změna je představena ve verzi EF Core 3,0-Preview 3.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-448">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="d9bf8-449">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-449">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-450">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="d9bf8-450">Consider the following model:</span></span>
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
<span data-ttu-id="d9bf8-451">Před EF Core 3,0 `CustomerId` se vlastnost používá pro cizí klíč podle konvence.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-451">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="d9bf8-452">Nicméně pokud `Order` je vlastněný typ, pak by to vedlo `CustomerId` také k tomu, že primární klíč a to není obvykle očekávané.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-452">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="d9bf8-453">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-453">**New behavior**</span></span>

<span data-ttu-id="d9bf8-454">Počínaje 3,0 se EF Core nepokouší použít vlastnosti pro cizí klíče podle konvence, pokud mají stejný název jako vlastnost Principal.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-454">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="d9bf8-455">Název objektu zabezpečení zřetězený s názvem vlastnosti objektu zabezpečení a název navigace zřetězený s vzory názvů hlavních vlastností se pořád shodují.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-455">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="d9bf8-456">Příklad:</span><span class="sxs-lookup"><span data-stu-id="d9bf8-456">For example:</span></span>

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

<span data-ttu-id="d9bf8-457">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-457">**Why**</span></span>

<span data-ttu-id="d9bf8-458">Tato změna byla provedena, aby nedocházelo k chybnému definování vlastností primárního klíče u vlastněných typů.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-458">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="d9bf8-459">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-459">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-460">Pokud by vlastnost měla být cizí klíč, a proto je součástí primárního klíče, pak ji explicitně nakonfigurujte jako takovou.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-460">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="d9bf8-461">Připojení k databázi je teď zavřené, pokud už nepoužíváte dřív, než se dokončí jeho objekt TransactionScope.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-461">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="d9bf8-462">Sledování problému #14218</span><span class="sxs-lookup"><span data-stu-id="d9bf8-462">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="d9bf8-463">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-463">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="d9bf8-464">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-464">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-465">Pokud kontext otevře během EF Core 3,0 připojení uvnitř `TransactionScope`, zůstane připojení otevřené, zatímco aktuální `TransactionScope` aktivní je.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-465">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="d9bf8-466">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-466">**New behavior**</span></span>

<span data-ttu-id="d9bf8-467">Od 3,0 EF Core ukončí připojení, jakmile ho dokončí jeho používání.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-467">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="d9bf8-468">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-468">**Why**</span></span>

<span data-ttu-id="d9bf8-469">Tato změna umožňuje použít více kontextů současně `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-469">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="d9bf8-470">Nové chování se také shoduje s EF6.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-470">The new behavior also matches EF6.</span></span>

<span data-ttu-id="d9bf8-471">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-471">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-472">Pokud připojení potřebuje zůstat otevřeným explicitním voláním `OpenConnection()` , zajistí, že EF Core neuzavře předčasně:</span><span class="sxs-lookup"><span data-stu-id="d9bf8-472">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="d9bf8-473">Každá vlastnost používá nezávislou generaci celočíselného klíče v paměti.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-473">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="d9bf8-474">Sledování problému #6872</span><span class="sxs-lookup"><span data-stu-id="d9bf8-474">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="d9bf8-475">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-475">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="d9bf8-476">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-476">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-477">Před EF Core 3,0 byl pro všechny vlastnosti celého čísla klíče v paměti použit jeden generátor sdílených hodnot.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-477">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="d9bf8-478">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-478">**New behavior**</span></span>

<span data-ttu-id="d9bf8-479">Počínaje EF Core 3,0 každé klíčové vlastnosti celého čísla získá vlastní generátor hodnot při použití databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-479">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="d9bf8-480">Také pokud je databáze odstraněna, generování klíče se obnoví pro všechny tabulky.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-480">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="d9bf8-481">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-481">**Why**</span></span>

<span data-ttu-id="d9bf8-482">Tato změna byla provedená tak, aby se při vytváření klíčů v paměti lépe rovnala generování klíčů v paměti a vylepšila se možnost izolovat testy od sebe při použití databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-482">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="d9bf8-483">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-483">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-484">To může poškodit aplikaci, která se spoléhá na konkrétní hodnoty klíče v paměti, které se mají nastavit.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-484">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="d9bf8-485">Místo toho se nemusíte spoléhat na konkrétní hodnoty klíčů nebo aktualizovat tak, aby odpovídaly novému chování.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-485">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="d9bf8-486">Ve výchozím nastavení se používají pole pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-486">Backing fields are used by default</span></span>

[<span data-ttu-id="d9bf8-487">Sledování problému #12430</span><span class="sxs-lookup"><span data-stu-id="d9bf8-487">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="d9bf8-488">Tato změna je představena ve verzi EF Core 3,0-Preview 2.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-488">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="d9bf8-489">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-489">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-490">Před 3,0, a to i v případě, že je známé pole zálohování pro vlastnost, EF Core ve výchozím nastavení přečetl a zapsat hodnotu vlastnosti pomocí metod getter a setter vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-490">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="d9bf8-491">Výjimkou je provedení dotazu, kde by se pole zálohování nastavilo přímo, pokud je známé.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-491">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="d9bf8-492">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-492">**New behavior**</span></span>

<span data-ttu-id="d9bf8-493">Počínaje EF Core 3,0 platí, že pokud je známé pole zálohování pro vlastnost, pak EF Core tuto vlastnost vždy přečte a zapíše pomocí pole pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-493">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="d9bf8-494">To může způsobit přerušení aplikace, pokud se aplikace spoléhá na další chování kódované v metodách getter nebo setter.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-494">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="d9bf8-495">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-495">**Why**</span></span>

<span data-ttu-id="d9bf8-496">Tato změna byla provedena EF Core proto, aby při provádění databázových operací, které obsahují entity, ve výchozím nastavení nechybně aktivovala obchodní logiku.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-496">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="d9bf8-497">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-497">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-498">Chování před 3,0 se dá obnovit pomocí konfigurace režimu přístupu vlastnosti zapnuto `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-498">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="d9bf8-499">Příklad:</span><span class="sxs-lookup"><span data-stu-id="d9bf8-499">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="d9bf8-500">Vyvolat, zda je nalezeno více kompatibilních zálohovaných polí</span><span class="sxs-lookup"><span data-stu-id="d9bf8-500">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="d9bf8-501">Sledování problému #12523</span><span class="sxs-lookup"><span data-stu-id="d9bf8-501">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="d9bf8-502">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-502">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="d9bf8-503">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-503">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-504">Pokud se v EF Core 3,0 shodovalo více polí s pravidly pro hledání zálohovacího pole vlastnosti, bude jedno pole zvoleno na základě pořadí priorit.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-504">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="d9bf8-505">To může způsobit, že se nesprávné pole použije v nejednoznačných případech.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-505">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="d9bf8-506">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-506">**New behavior**</span></span>

<span data-ttu-id="d9bf8-507">Počínaje EF Core 3,0 platí, že pokud je více polí spárováno se stejnou vlastností, je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-507">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="d9bf8-508">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-508">**Why**</span></span>

<span data-ttu-id="d9bf8-509">Tato změna byla provedena, aby nedocházelo k tichému použití jednoho pole v případě, že může být pouze jedna z nich správná.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-509">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="d9bf8-510">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-510">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-511">Vlastnosti s nejednoznačnými zálohovacími poli musí obsahovat pole, které se má explicitně použít.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-511">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="d9bf8-512">Například pomocí rozhraní Fluent API:</span><span class="sxs-lookup"><span data-stu-id="d9bf8-512">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="d9bf8-513">Názvy vlastností pouze pro pole se musí shodovat s názvem pole.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-513">Field-only property names should match the field name</span></span>

<span data-ttu-id="d9bf8-514">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-514">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="d9bf8-515">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-515">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-516">Před EF Core 3,0 může být vlastnost určena hodnotou řetězce a v případě, že v typu CLR nebyla nalezena žádná vlastnost s tímto názvem, EF Core by se pokusila ji porovnat s polem pomocí pravidel konvence.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-516">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="d9bf8-517">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-517">**New behavior**</span></span>

<span data-ttu-id="d9bf8-518">Počínaje EF Core 3,0 se vlastnost pouze pole musí přesně shodovat s názvem pole.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-518">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="d9bf8-519">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-519">**Why**</span></span>

<span data-ttu-id="d9bf8-520">Tato změna byla provedena, aby se nepoužívalo stejné pole pro dvě vlastnosti s názvem podobně, ale také pravidla pro porovnání vlastností pouze pro pole, která jsou shodná s vlastnostmi mapovanými na vlastnosti CLR.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-520">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="d9bf8-521">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-521">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-522">Vlastnosti pouze polí musí být pojmenovány stejně jako pole, na které jsou namapována.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-522">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="d9bf8-523">V novější verzi EF Core 3,0 plánujeme znovu povolit explicitní konfiguraci názvu pole, který se liší od názvu vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="d9bf8-523">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="d9bf8-524">AddDbContext/AddDbContextPool už nevolá AddLogging a AddMemoryCache.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-524">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="d9bf8-525">Sledování problému #14756</span><span class="sxs-lookup"><span data-stu-id="d9bf8-525">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="d9bf8-526">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-526">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="d9bf8-527">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-527">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-528">Před EF Core 3,0, volání `AddDbContext` nebo `AddDbContextPool` by také registrovaly služby protokolování a ukládání do mezipaměti v paměti pomocí D. I přes volání [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) a [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="d9bf8-528">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="d9bf8-529">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-529">**New behavior**</span></span>

<span data-ttu-id="d9bf8-530">Počínaje EF Core 3,0 `AddDbContext` a `AddDbContextPool` nadále se nebudou registrovat tyto služby se vkládáním závislostí (di).</span><span class="sxs-lookup"><span data-stu-id="d9bf8-530">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="d9bf8-531">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-531">**Why**</span></span>

<span data-ttu-id="d9bf8-532">EF Core 3,0 nevyžaduje, aby se tyto služby nacházejí v kontejneru aplikace DI.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-532">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="d9bf8-533">Pokud `ILoggerFactory` je však v kontejneru aplikace zaregistrován, bude nadále používána EF Core.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-533">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="d9bf8-534">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-534">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-535">Pokud vaše aplikace potřebuje tyto služby, zaregistrujte je explicitně pomocí kontejneru DI pomocí [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) nebo [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="d9bf8-535">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="d9bf8-536">DbContext. entry teď provádí místní DetectChanges.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-536">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="d9bf8-537">Sledování problému #13552</span><span class="sxs-lookup"><span data-stu-id="d9bf8-537">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="d9bf8-538">Tato změna je představena ve verzi EF Core 3,0-Preview 3.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-538">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="d9bf8-539">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-539">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-540">Před EF Core 3,0 by volání `DbContext.Entry` způsobilo zjištění změn všech sledovaných entit.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-540">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="d9bf8-541">Tím je zajištěno, že stav zpřístupněno v `EntityEntry` nástroji byl aktuální.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-541">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="d9bf8-542">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-542">**New behavior**</span></span>

<span data-ttu-id="d9bf8-543">Počínaje EF Core 3,0 se nyní volání `DbContext.Entry` pokusí zjistit změny v dané entitě a všechny sledované hlavní entity, které se k ní vztahují.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-543">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="d9bf8-544">To znamená, že změny jinde nemohly být zjištěny voláním této metody, což by mohlo mít vliv na stav aplikace.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-544">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="d9bf8-545">Všimněte si, `ChangeTracker.AutoDetectChangesEnabled` že pokud je `false` nastaveno na, tak i toto místní zjišťování změn bude zakázáno.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-545">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="d9bf8-546">Jiné metody, které způsobují detekci změn – například `ChangeTracker.Entries` a `SaveChanges`---stále zapříčiní plné `DetectChanges` ze všech sledovaných entit.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-546">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="d9bf8-547">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-547">**Why**</span></span>

<span data-ttu-id="d9bf8-548">Tato změna byla provedena za účelem zlepšení výchozího výkonu použití `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-548">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="d9bf8-549">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-549">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-550">Před `ChgangeTracker.DetectChanges()` voláním `Entry` zajistěte explicitní volání, aby bylo zajištěno chování před 3,0.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-550">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="d9bf8-551">Klíče řetězce a pole bajtů nejsou ve výchozím nastavení generovány klientem.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-551">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="d9bf8-552">Sledování problému #14617</span><span class="sxs-lookup"><span data-stu-id="d9bf8-552">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="d9bf8-553">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-553">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="d9bf8-554">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-554">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-555">Před EF Core 3,0 `string` a `byte[]` vlastnosti klíče lze použít bez explicitního nastavení hodnoty, která není null.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-555">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="d9bf8-556">V takovém případě se hodnota klíče vygeneruje na klientovi jako identifikátor GUID, který je serializovaný na bajty pro `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-556">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="d9bf8-557">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-557">**New behavior**</span></span>

<span data-ttu-id="d9bf8-558">Počínaje EF Core 3,0 bude vyvolána výjimka oznamující, že nebyla nastavena žádná hodnota klíče.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-558">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="d9bf8-559">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-559">**Why**</span></span>

<span data-ttu-id="d9bf8-560">Tato změna byla provedena, protože obecně nejsou `string` užitečné hodnoty generované / `byte[]` klientem a výchozí chování způsobilo, že je obtížné vygenerovat hodnoty klíčů běžným způsobem.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-560">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="d9bf8-561">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-561">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-562">Chování před 3,0 lze získat explicitním určením, že klíčové vlastnosti by měly používat generované hodnoty, pokud není nastavena žádná jiná hodnota, která není null.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-562">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="d9bf8-563">Například s rozhraním API Fluent:</span><span class="sxs-lookup"><span data-stu-id="d9bf8-563">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="d9bf8-564">Nebo s poznámkami k datům:</span><span class="sxs-lookup"><span data-stu-id="d9bf8-564">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="d9bf8-565">ILoggerFactory je teď služba s vymezeným oborem.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-565">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="d9bf8-566">Sledování problému #14698</span><span class="sxs-lookup"><span data-stu-id="d9bf8-566">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="d9bf8-567">Tato změna je představena ve verzi EF Core 3,0-Preview 3.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-567">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="d9bf8-568">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-568">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-569">Před EF Core 3,0 `ILoggerFactory` byl zaregistrován jako služba typu singleton.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-569">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="d9bf8-570">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-570">**New behavior**</span></span>

<span data-ttu-id="d9bf8-571">Počínaje EF Core 3,0 `ILoggerFactory` je nyní registrováno jako obor.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-571">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="d9bf8-572">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-572">**Why**</span></span>

<span data-ttu-id="d9bf8-573">Tato změna byla provedena tak, aby povolovala přidružení protokolovacího `DbContext` nástroje s instancí, která umožňuje další funkce a odebírá některé případy patologického chování, jako je například rozpad interních poskytovatelů služeb.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-573">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="d9bf8-574">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-574">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-575">Tato změna by neměla mít vliv na kód aplikace, pokud se neregistruje a nepoužívá vlastní služby pro EF Core interního poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-575">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="d9bf8-576">To není běžné.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-576">This isn't common.</span></span>
<span data-ttu-id="d9bf8-577">V těchto případech bude většina věcí pořád fungovat, ale jakákoli služba typu Singleton, která byla v závislosti `ILoggerFactory` na tom, bude muset změnit tak, `ILoggerFactory` aby získala jiný způsob.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-577">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="d9bf8-578">Pokud narazíte na takové situace, zajistěte prosím problém na [EF Core modul pro sledování problémů GitHubu](https://github.com/aspnet/EntityFrameworkCore/issues) , abychom nás věděli, jak `ILoggerFactory` ho používáte, abychom mohli lépe pochopit, jak to v budoucnu Nerušit.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-578">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="d9bf8-579">Opožděné načítání proxy serverů už nepředpokládá navigační vlastnosti, jsou plně načtené.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-579">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="d9bf8-580">Sledování problému #12780</span><span class="sxs-lookup"><span data-stu-id="d9bf8-580">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="d9bf8-581">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-581">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="d9bf8-582">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-582">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-583">Předtím, než EF Core 3,0, `DbContext` neexistuje žádný způsob, jak zjistit, zda byla daná vlastnost navigace u entity získané z tohoto kontextu plně načtena nebo ne.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-583">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="d9bf8-584">Proxy místo toho předpokládají, že je načtena odkazová navigace, pokud má hodnotu jinou než null a že je načtena navigace kolekce, pokud není prázdná.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-584">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="d9bf8-585">V těchto případech by byl pokus o opožděné načtení no-op.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-585">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="d9bf8-586">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-586">**New behavior**</span></span>

<span data-ttu-id="d9bf8-587">Počínaje EF Core 3,0 budou proxy servery sledovat, zda je načtena vlastnost navigace.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-587">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="d9bf8-588">To znamená, že při pokusu o přístup k navigační vlastnosti, která je načtena po vyřazení kontextu, bude vždy ta no-op, i když je načtená navigace prázdná nebo má hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-588">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="d9bf8-589">Naopak při pokusu o přístup k navigační vlastnosti, která není načtená, vyvolá výjimku, pokud je kontext vyřazený, i když je vlastnost navigace neprázdná kolekce.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-589">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="d9bf8-590">Pokud nastane tato situace, znamená to, že se kód aplikace pokouší použít opožděné načítání v neplatném čase a aplikace by se měla změnit, aby to nevedlo.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-590">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="d9bf8-591">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-591">**Why**</span></span>

<span data-ttu-id="d9bf8-592">Tato změna byla provedena, aby při pokusu o opožděné načtení na uvolněnou `DbContext` instanci bylo chování konzistentní a správné.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-592">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="d9bf8-593">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-593">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-594">Aktualizujte kód aplikace, aby se nepokoušel opožděné načtení s odstraněným kontextem, nebo nastavte tuto hodnotu jako No-op, jak je popsáno ve zprávě výjimky.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-594">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="d9bf8-595">Nadměrné vytváření interních zprostředkovatelů služeb je teď ve výchozím nastavení chyba.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-595">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="d9bf8-596">Sledování problému #10236</span><span class="sxs-lookup"><span data-stu-id="d9bf8-596">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="d9bf8-597">Tato změna je představena ve verzi EF Core 3,0-Preview 3.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-597">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="d9bf8-598">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-598">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-599">Před EF Core 3,0 se pro aplikaci, která vytváří patologický počet interních poskytovatelů služeb, zaprotokoluje upozornění.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-599">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="d9bf8-600">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-600">**New behavior**</span></span>

<span data-ttu-id="d9bf8-601">Počínaje EF Core 3,0 je toto upozornění nyní považováno za chybu a je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-601">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="d9bf8-602">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-602">**Why**</span></span>

<span data-ttu-id="d9bf8-603">Tato změna byla provedená tak, že se tento patologický případ výslovně zveřejňuje tak, aby se zlepšil kód aplikace.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-603">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="d9bf8-604">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-604">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-605">Nejvhodnější příčinou této chyby je pochopení hlavní příčiny a zastavení vytváření, takže mnoho interních poskytovatelů služeb.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-605">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="d9bf8-606">Chybu však lze převést zpět na varování (nebo ignorováno) prostřednictvím konfigurace na `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-606">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="d9bf8-607">Příklad:</span><span class="sxs-lookup"><span data-stu-id="d9bf8-607">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="d9bf8-608">Nové chování pro HasOne/HasMany se volá s jedním řetězcem.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-608">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="d9bf8-609">Sledování problému #9171</span><span class="sxs-lookup"><span data-stu-id="d9bf8-609">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="d9bf8-610">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-610">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="d9bf8-611">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-611">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-612">Před EF Core 3,0, volání `HasOne` kódu nebo `HasMany` s jedním řetězcem bylo interpretováno matoucím způsobem.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-612">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="d9bf8-613">Příklad:</span><span class="sxs-lookup"><span data-stu-id="d9bf8-613">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="d9bf8-614">Kód vypadá jako v `Samurai` `Entrance` souvislosti s jiným typem entity pomocí navigační vlastnosti, která může být soukromá.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-614">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="d9bf8-615">Ve skutečnosti se tento kód pokusí vytvořit relaci k některému typu entity s názvem `Entrance` bez navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-615">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="d9bf8-616">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-616">**New behavior**</span></span>

<span data-ttu-id="d9bf8-617">Počínaje EF Core 3,0 výše uvedený kód má nyní podobný vzhled jako předtím.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-617">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="d9bf8-618">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-618">**Why**</span></span>

<span data-ttu-id="d9bf8-619">Staré chování bylo velmi matoucí, zejména při čtení konfiguračního kódu a hledání chyb.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-619">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="d9bf8-620">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-620">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-621">Tím dojde pouze k přerušení aplikací, které jsou explicitně konfigurovány pomocí řetězců pro názvy typů, a bez explicitního určení vlastnosti navigace.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-621">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="d9bf8-622">To není běžné.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-622">This is not common.</span></span>
<span data-ttu-id="d9bf8-623">Předchozí chování lze získat pomocí explicitního předání `null` názvu vlastnosti navigace.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-623">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="d9bf8-624">Příklad:</span><span class="sxs-lookup"><span data-stu-id="d9bf8-624">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="d9bf8-625">Návratový typ pro několik asynchronních metod byl změněn z úlohy na ValueTask</span><span class="sxs-lookup"><span data-stu-id="d9bf8-625">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="d9bf8-626">Sledování problému #15184</span><span class="sxs-lookup"><span data-stu-id="d9bf8-626">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="d9bf8-627">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-627">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="d9bf8-628">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-628">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-629">Následující asynchronní metody dříve vrátily `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="d9bf8-629">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="d9bf8-630">`ValueGenerator.NextValueAsync()`(a odvozování tříd)</span><span class="sxs-lookup"><span data-stu-id="d9bf8-630">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="d9bf8-631">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-631">**New behavior**</span></span>

<span data-ttu-id="d9bf8-632">Výše uvedené metody nyní vrací stejnou `ValueTask<T>` `T` hodnotu jako předtím.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-632">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="d9bf8-633">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-633">**Why**</span></span>

<span data-ttu-id="d9bf8-634">Tato změna snižuje počet přidělení haldy, které vznikly při vyvolání těchto metod, což zlepšuje obecný výkon.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-634">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="d9bf8-635">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-635">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-636">Aplikace jednoduše čekají na rozhraní API, které je třeba znovu zkompilovat – nejsou nutné žádné změny zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-636">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="d9bf8-637">Složitější využití (například předání vráceného `Task` do `Task.WhenAny()`) obvykle vyžaduje, aby bylo vráceno `ValueTask<T>` převedené na a `Task<T>` voláním `AsTask()` .</span><span class="sxs-lookup"><span data-stu-id="d9bf8-637">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="d9bf8-638">Všimněte si, že se tím sníží omezení přidělení, které tato změna přináší.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-638">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="d9bf8-639">Relační: anotace TypeMapping je nyní pouze TypeMapping</span><span class="sxs-lookup"><span data-stu-id="d9bf8-639">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="d9bf8-640">Sledování problému #9913</span><span class="sxs-lookup"><span data-stu-id="d9bf8-640">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="d9bf8-641">Tato změna je představena ve verzi EF Core 3,0-Preview 2.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-641">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="d9bf8-642">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-642">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-643">Název poznámky pro poznámky k mapování typů byl "relační: TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="d9bf8-643">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="d9bf8-644">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-644">**New behavior**</span></span>

<span data-ttu-id="d9bf8-645">Název poznámky pro mapování typů je nyní "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="d9bf8-645">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="d9bf8-646">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-646">**Why**</span></span>

<span data-ttu-id="d9bf8-647">Mapování typů se nyní používají pro více než stejné poskytovatele relačních databází.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-647">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="d9bf8-648">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-648">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-649">Tím dojde pouze k přerušení aplikací, které přistupují k mapování typu přímo jako anotaci, což není běžné.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-649">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="d9bf8-650">Nejvhodnější akcí pro opravu je použití prostoru rozhraní API pro přístup k mapování typů namísto použití anotace přímo.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-650">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="d9bf8-651">ToTable na odvozeném typu vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-651">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="d9bf8-652">Sledování problému #11811</span><span class="sxs-lookup"><span data-stu-id="d9bf8-652">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="d9bf8-653">Tato změna je představena ve verzi EF Core 3,0-Preview 3.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-653">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="d9bf8-654">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-654">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-655">Před EF Core 3,0, `ToTable()` který je volán na odvozeném typu, by byl ignorován, protože pouze dědění mapování dědičnosti bylo typu TPH, který není platný.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-655">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="d9bf8-656">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-656">**New behavior**</span></span>

<span data-ttu-id="d9bf8-657">Počínaje EF Core 3,0 a přípravou pro přidání podpory TPT a TPC v novější verzi, která se `ToTable()` volá na odvozeném typu, teď vyvolá výjimku, aby se předešlo neočekávané změně mapování v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-657">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="d9bf8-658">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-658">**Why**</span></span>

<span data-ttu-id="d9bf8-659">V současné době není platný pro mapování odvozeného typu na jinou tabulku.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-659">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="d9bf8-660">Tato změna zabrání v budoucnosti v budoucnu, pokud se to stalo platným.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-660">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="d9bf8-661">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-661">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-662">Odeberte všechny pokusy o mapování odvozených typů na jiné tabulky.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-662">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="d9bf8-663">ForSqlServerHasIndex nahrazeno HasIndex</span><span class="sxs-lookup"><span data-stu-id="d9bf8-663">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="d9bf8-664">Sledování problému #12366</span><span class="sxs-lookup"><span data-stu-id="d9bf8-664">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="d9bf8-665">Tato změna je představena ve verzi EF Core 3,0-Preview 3.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-665">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="d9bf8-666">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-666">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-667">Před EF Core 3,0 jste `ForSqlServerHasIndex().ForSqlServerInclude()` získali způsob, jak nakonfigurovat sloupce používané pomocí `INCLUDE`nástroje.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-667">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="d9bf8-668">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-668">**New behavior**</span></span>

<span data-ttu-id="d9bf8-669">Počínaje EF Core 3,0 se teď použití `Include` na indexu podporuje na relační úrovni.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-669">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="d9bf8-670">Použijte `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-670">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="d9bf8-671">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-671">**Why**</span></span>

<span data-ttu-id="d9bf8-672">Tato změna byla provedena za účelem konsolidace rozhraní API pro indexy `Include` na jednom místě pro všechny poskytovatele databáze.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-672">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="d9bf8-673">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-673">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-674">Použijte nové rozhraní API, jak vidíte výše.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-674">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="d9bf8-675">Změny rozhraní API pro metadata</span><span class="sxs-lookup"><span data-stu-id="d9bf8-675">Metadata API changes</span></span>

[<span data-ttu-id="d9bf8-676">Sledování problému #214</span><span class="sxs-lookup"><span data-stu-id="d9bf8-676">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="d9bf8-677">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-677">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="d9bf8-678">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-678">**New behavior**</span></span>

<span data-ttu-id="d9bf8-679">Následující vlastnosti byly převedeny na rozšiřující metody:</span><span class="sxs-lookup"><span data-stu-id="d9bf8-679">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="d9bf8-680">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-680">**Why**</span></span>

<span data-ttu-id="d9bf8-681">Tato změna zjednodušuje implementaci výše uvedených rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-681">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="d9bf8-682">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-682">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-683">Použijte nové metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-683">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="d9bf8-684">Změny rozhraní API pro konkrétního zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="d9bf8-684">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="d9bf8-685">Sledování problému #214</span><span class="sxs-lookup"><span data-stu-id="d9bf8-685">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="d9bf8-686">Tato změna je představena ve verzi EF Core 3,0-Preview 6.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-686">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="d9bf8-687">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-687">**New behavior**</span></span>

<span data-ttu-id="d9bf8-688">Metody rozšíření specifické pro poskytovatele budou shrnuty:</span><span class="sxs-lookup"><span data-stu-id="d9bf8-688">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="d9bf8-689">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-689">**Why**</span></span>

<span data-ttu-id="d9bf8-690">Tato změna zjednodušuje implementaci výše uvedených rozšiřujících metod.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-690">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="d9bf8-691">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-691">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-692">Použijte nové metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-692">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="d9bf8-693">EF Core už neposílá direktivu pragma pro vynucení KOFK SQLite.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-693">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="d9bf8-694">Sledování problému #12151</span><span class="sxs-lookup"><span data-stu-id="d9bf8-694">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="d9bf8-695">Tato změna je představena ve verzi EF Core 3,0-Preview 3.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-695">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="d9bf8-696">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-696">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-697">Před EF Core 3,0 EF Core odeslat `PRAGMA foreign_keys = 1` při otevření připojení k sqlite.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-697">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="d9bf8-698">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-698">**New behavior**</span></span>

<span data-ttu-id="d9bf8-699">Počínaje EF Core 3,0 EF Core již neposílá `PRAGMA foreign_keys = 1` při otevření připojení k sqlite.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-699">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="d9bf8-700">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-700">**Why**</span></span>

<span data-ttu-id="d9bf8-701">Tato změna byla provedena, protože EF Core `SQLitePCLRaw.bundle_e_sqlite3` používá ve výchozím nastavení, což zase znamená, že je ve výchozím nastavení zapnuté vynucení CK a není nutné je explicitně povolit při každém otevření připojení.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-701">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="d9bf8-702">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-702">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-703">Cizí klíče jsou ve výchozím nastavení povolené v SQLitePCLRaw. bundle_e_sqlite3, který se ve výchozím nastavení používá pro EF Core.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-703">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="d9bf8-704">V ostatních případech je možné povolit cizí klíče zadáním `Foreign Keys=True` v připojovacím řetězci.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-704">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="d9bf8-705">Microsoft. EntityFrameworkCore. sqlite teď závisí na SQLitePCLRaw. bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="d9bf8-705">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="d9bf8-706">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-706">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-707">Před EF Core 3,0 se používá `SQLitePCLRaw.bundle_green`EF Core.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-707">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="d9bf8-708">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-708">**New behavior**</span></span>

<span data-ttu-id="d9bf8-709">Počínaje EF Core 3,0 EF Core používá `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-709">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="d9bf8-710">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-710">**Why**</span></span>

<span data-ttu-id="d9bf8-711">Tato změna byla provedena tak, že verze SQLiteu použitá v iOS je konzistentní s jinými platformami.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-711">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="d9bf8-712">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-712">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-713">Pokud chcete použít nativní verzi SQLite v iOS, nakonfigurujte `Microsoft.Data.Sqlite` , aby používala `SQLitePCLRaw` jiný svazek.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-713">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="d9bf8-714">Hodnoty GUID se teď ukládají jako TEXT na SQLite.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-714">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="d9bf8-715">Sledování problému #15078</span><span class="sxs-lookup"><span data-stu-id="d9bf8-715">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="d9bf8-716">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-716">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="d9bf8-717">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-717">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-718">Hodnoty GUID byly dříve uloženy jako hodnoty objektů BLOB u SQLite.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-718">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="d9bf8-719">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-719">**New behavior**</span></span>

<span data-ttu-id="d9bf8-720">Hodnoty GUID jsou nyní uloženy jako TEXT.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-720">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="d9bf8-721">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-721">**Why**</span></span>

<span data-ttu-id="d9bf8-722">Binární formát identifikátorů GUID není standardizovaný.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-722">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="d9bf8-723">Uložení hodnot jako textu zajistí, že databáze bude lépe kompatibilní s jinými technologiemi.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-723">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="d9bf8-724">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-724">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-725">Existující databáze můžete migrovat do nového formátu tím, že spustíte příkaz SQL podobně jako následující.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-725">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="d9bf8-726">V EF Core můžete i nadále používat předchozí chování nakonfigurováním převaděče hodnot na těchto vlastnostech.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-726">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="d9bf8-727">Microsoft. data. sqlite zůstává schopný přečítat hodnoty GUID z objektů BLOB a textových sloupců. vzhledem k tomu, že výchozí formát pro parametry a konstanty se změnil, bude pravděpodobně nutné provést akci u většiny scénářů, které zahrnují identifikátory GUID.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-727">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="d9bf8-728">Hodnoty char se teď ukládají jako TEXT na SQLite.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-728">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="d9bf8-729">Sledování problému #15020</span><span class="sxs-lookup"><span data-stu-id="d9bf8-729">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="d9bf8-730">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-730">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="d9bf8-731">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-731">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-732">Hodnoty typu char byly dříve sored jako CELOČÍSELNé hodnoty u SQLite.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-732">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="d9bf8-733">Například hodnota znaku *a* byla uložena jako celočíselná hodnota 65.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-733">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="d9bf8-734">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-734">**New behavior**</span></span>

<span data-ttu-id="d9bf8-735">Hodnoty typu char jsou nyní uloženy jako TEXT.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-735">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="d9bf8-736">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-736">**Why**</span></span>

<span data-ttu-id="d9bf8-737">Ukládání hodnot jako TEXT je přirozenější a databáze usnadňuje kompatibilitu s jinými technologiemi.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-737">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="d9bf8-738">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-738">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-739">Existující databáze můžete migrovat do nového formátu tím, že spustíte příkaz SQL podobně jako následující.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-739">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="d9bf8-740">V EF Core můžete i nadále používat předchozí chování nakonfigurováním převaděče hodnot na těchto vlastnostech.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-740">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="d9bf8-741">Microsoft. data. sqlite také zůstává schopný číst znakové hodnoty z CELOČÍSELNého i TEXTOVÉHO sloupce, takže některé scénáře nemusí vyžadovat žádnou akci.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-741">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="d9bf8-742">ID migrace se teď generují pomocí kalendáře invariantní jazykové verze.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-742">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="d9bf8-743">Sledování problému #12978</span><span class="sxs-lookup"><span data-stu-id="d9bf8-743">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="d9bf8-744">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-744">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="d9bf8-745">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-745">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-746">ID migrace se nechtěně vygenerovala pomocí kalendáře aktuální jazykové verze.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-746">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="d9bf8-747">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-747">**New behavior**</span></span>

<span data-ttu-id="d9bf8-748">ID migrace se nyní vždy generují pomocí kalendáře neutrální jazykové verze (gregoriánský).</span><span class="sxs-lookup"><span data-stu-id="d9bf8-748">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="d9bf8-749">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-749">**Why**</span></span>

<span data-ttu-id="d9bf8-750">Pořadí migrace je důležité při aktualizaci databáze nebo řešení konfliktů při slučování.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-750">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="d9bf8-751">Pomocí invariantního kalendáře se vyhnete problémům s řazením, které mohou být výsledkem členů týmu jiné systémové kalendáře.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-751">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="d9bf8-752">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-752">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-753">Tato změna má vliv na kohokoli, kdo používá negregoriánský kalendář, ve kterém je rok větší než gregoriánský kalendář (například thajský buddhistický kalendář).</span><span class="sxs-lookup"><span data-stu-id="d9bf8-753">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="d9bf8-754">Existující identifikátory migrace se budou muset aktualizovat, aby se nové migrace objednaly po stávajících migracích.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-754">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="d9bf8-755">ID migrace najdete v atributu migrace v souborech návrháře migrace.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-755">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="d9bf8-756">Také je potřeba aktualizovat tabulku historie migrace.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-756">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="d9bf8-757">UseRowNumberForPaging se odebral.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-757">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="d9bf8-758">Sledování problému #16400</span><span class="sxs-lookup"><span data-stu-id="d9bf8-758">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="d9bf8-759">Tato změna je představena ve verzi EF Core 3,0-Preview 6.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-759">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="d9bf8-760">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-760">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-761">Před EF Core 3,0 `UseRowNumberForPaging` lze použít k vygenerování SQL pro stránkování, které je kompatibilní s SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-761">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="d9bf8-762">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-762">**New behavior**</span></span>

<span data-ttu-id="d9bf8-763">Počínaje EF Core 3,0 bude EF generovat pouze SQL pro stránkování, které je kompatibilní pouze s novějšími verzemi SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-763">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="d9bf8-764">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-764">**Why**</span></span>

<span data-ttu-id="d9bf8-765">Tuto změnu provedeme, protože [SQL Server 2008 už není podporovaným produktem](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) a aktualizace této funkce tak, aby fungovala se změnami dotazů provedenými v EF Core 3,0 je významná práce.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-765">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="d9bf8-766">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-766">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-767">Doporučujeme aktualizovat na novější verzi SQL Server nebo pomocí vyšší úrovně kompatibility, aby byl vygenerovaný SQL podporován.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-767">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="d9bf8-768">To znamená, že pokud to nemůžete udělat, [komentář k problému](https://github.com/aspnet/EntityFrameworkCore/issues/16400) s sledováním najdete v podrobnostech.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-768">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="d9bf8-769">Toto rozhodnutí můžeme znovu navštívit na základě zpětné vazby.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-769">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="d9bf8-770">Informace o rozšíření/metadata se odebraly z IDbContextOptionsExtension.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-770">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="d9bf8-771">Sledování problému #16119</span><span class="sxs-lookup"><span data-stu-id="d9bf8-771">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="d9bf8-772">Tato změna je představena ve verzi EF Core 3,0-Preview 7.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-772">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="d9bf8-773">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-773">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-774">`IDbContextOptionsExtension`obsažené metody pro poskytování metadat o rozšíření.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-774">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="d9bf8-775">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-775">**New behavior**</span></span>

<span data-ttu-id="d9bf8-776">Tyto metody byly přesunuty na novou `DbContextOptionsExtensionInfo` abstraktní základní třídu, která je vrácena z nové `IDbContextOptionsExtension.Info` vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-776">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="d9bf8-777">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-777">**Why**</span></span>

<span data-ttu-id="d9bf8-778">V rámci vydání od 2,0 do 3,0 jsme potřebovali přidat nebo změnit tyto metody několikrát.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-778">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="d9bf8-779">Rozbalením do nové abstraktní základní třídy bude snazší vytvořit tyto změny bez přerušení stávajících rozšíření.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-779">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="d9bf8-780">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-780">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-781">Aktualizovat rozšíření tak, aby následovala nový vzor.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-781">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="d9bf8-782">Příklady naleznete v mnoha implementacích `IDbContextOptionsExtension` pro různé druhy rozšíření ve zdrojovém kódu EF Core.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-782">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="d9bf8-783">LogQueryPossibleExceptionWithAggregateOperator se přejmenovalo.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-783">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="d9bf8-784">Sledování problému #10985</span><span class="sxs-lookup"><span data-stu-id="d9bf8-784">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="d9bf8-785">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-785">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="d9bf8-786">**Mění**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-786">**Change**</span></span>

<span data-ttu-id="d9bf8-787">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator`byla přejmenována `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`na.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-787">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="d9bf8-788">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-788">**Why**</span></span>

<span data-ttu-id="d9bf8-789">Zarovná pojmenování této události varování se všemi ostatními událostmi upozornění.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-789">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="d9bf8-790">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-790">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-791">Použijte nový název.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-791">Use the new name.</span></span> <span data-ttu-id="d9bf8-792">(Všimněte si, že číslo ID události se nezměnilo.)</span><span class="sxs-lookup"><span data-stu-id="d9bf8-792">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="d9bf8-793">Vysvětlení rozhraní API pro názvy omezení cizího klíče</span><span class="sxs-lookup"><span data-stu-id="d9bf8-793">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="d9bf8-794">Sledování problému #10730</span><span class="sxs-lookup"><span data-stu-id="d9bf8-794">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="d9bf8-795">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-795">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="d9bf8-796">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-796">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-797">Před EF Core 3,0 se názvy omezení cizího klíče odkazovaly jenom na "název".</span><span class="sxs-lookup"><span data-stu-id="d9bf8-797">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="d9bf8-798">Příklad:</span><span class="sxs-lookup"><span data-stu-id="d9bf8-798">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="d9bf8-799">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-799">**New behavior**</span></span>

<span data-ttu-id="d9bf8-800">Počínaje EF Core 3,0 se názvy omezení cizích klíčů teď označují jako "název omezení".</span><span class="sxs-lookup"><span data-stu-id="d9bf8-800">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="d9bf8-801">Příklad:</span><span class="sxs-lookup"><span data-stu-id="d9bf8-801">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="d9bf8-802">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-802">**Why**</span></span>

<span data-ttu-id="d9bf8-803">Tato změna přináší konzistenci pro pojmenování v této oblasti a také vysvětluje, že se jedná o název omezení cizího klíče, a nikoli název sloupce nebo vlastnosti, ve kterém je definován cizí klíč.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-803">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="d9bf8-804">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-804">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-805">Použijte nový název.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-805">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="d9bf8-806">IRelationalDatabaseCreator. HasTables/HasTablesAsync byly zveřejněny.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-806">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="d9bf8-807">Sledování problému #15997</span><span class="sxs-lookup"><span data-stu-id="d9bf8-807">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="d9bf8-808">Tato změna je představena ve verzi EF Core 3,0-Preview 7.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-808">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="d9bf8-809">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-809">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-810">Před EF Core 3,0 byly tyto metody chráněné.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-810">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="d9bf8-811">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-811">**New behavior**</span></span>

<span data-ttu-id="d9bf8-812">Počínaje EF Core 3,0 jsou tyto metody veřejné.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-812">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="d9bf8-813">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-813">**Why**</span></span>

<span data-ttu-id="d9bf8-814">Tyto metody jsou používány EF k určení, jestli je databáze vytvořená, ale prázdná.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-814">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="d9bf8-815">To může být užitečné taky od vnějšího EF při určování, jestli se mají migrace použít.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-815">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="d9bf8-816">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-816">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-817">Změňte přístupnost všech přepsání.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-817">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="d9bf8-818">Microsoft. EntityFrameworkCore. Design je teď balíček DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-818">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="d9bf8-819">Sledování problému #11506</span><span class="sxs-lookup"><span data-stu-id="d9bf8-819">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="d9bf8-820">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-820">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="d9bf8-821">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-821">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-822">Před EF Core 3,0 byl Microsoft. EntityFrameworkCore. Design regulárním balíčkem NuGet, na kterém by mohly být na sestavení odkazovány projekty, které na něm závisejí.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-822">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="d9bf8-823">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-823">**New behavior**</span></span>

<span data-ttu-id="d9bf8-824">Počínaje EF Core 3,0 se jedná o balíček DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-824">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="d9bf8-825">To znamená, že závislost nebude nijak přesměrovat do jiných projektů a že již nemůžete ve výchozím nastavení odkazovat na své sestavení.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-825">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="d9bf8-826">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-826">**Why**</span></span>

<span data-ttu-id="d9bf8-827">Tento balíček se má použít jenom v době návrhu.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-827">This package is only intended to be used at design time.</span></span> <span data-ttu-id="d9bf8-828">Nasazené aplikace by neměli na ni odkazovat.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-828">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="d9bf8-829">Díky tomu, že balíček DevelopmentDependency, toto doporučení posiluje.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-829">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="d9bf8-830">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-830">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-831">Pokud potřebujete odkazovat na tento balíček, aby bylo možné přepsat EF Core chování při návrhu, můžete aktualizovat metadata položky aktualizovat PackageReference v projektu.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-831">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="d9bf8-832">Pokud se na balíček odkazuje přes Microsoft. EntityFrameworkCore. Tools, budete muset do balíčku přidat explicitní PackageReference, aby se změnila jeho metadata.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-832">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0-preview4.19216.3">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="d9bf8-833">SQLitePCL. Raw aktualizováno na verzi 2.0.0</span><span class="sxs-lookup"><span data-stu-id="d9bf8-833">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="d9bf8-834">Sledování problému #14824</span><span class="sxs-lookup"><span data-stu-id="d9bf8-834">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="d9bf8-835">Tato změna je představena ve verzi EF Core 3,0-Preview 7.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-835">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="d9bf8-836">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-836">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-837">Microsoft. EntityFrameworkCore. sqlite byl dřív závislý na 1.1.12 verze SQLitePCL. Raw.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-837">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="d9bf8-838">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-838">**New behavior**</span></span>

<span data-ttu-id="d9bf8-839">Náš balíček jsme aktualizovali tak, aby byl závislý na verzi 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-839">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="d9bf8-840">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-840">**Why**</span></span>

<span data-ttu-id="d9bf8-841">2\.0.0 verze SQLitePCL. Raw TARGETS .NET Standard 2,0.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-841">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="d9bf8-842">Dříve cílí na .NET Standard 1,1, které vyžadovaly, aby se při práci vytvořil velký uzávěr přenosných balíčků.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-842">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="d9bf8-843">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-843">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-844">SQLitePCL. Raw 2.0.0 verze obsahuje některé zásadní změny.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-844">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="d9bf8-845">Podrobnosti najdete v poznámkách k [verzi](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) .</span><span class="sxs-lookup"><span data-stu-id="d9bf8-845">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="d9bf8-846">NetTopologySuite aktualizace na verzi 2.0.0</span><span class="sxs-lookup"><span data-stu-id="d9bf8-846">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="d9bf8-847">Sledování problému #14825</span><span class="sxs-lookup"><span data-stu-id="d9bf8-847">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="d9bf8-848">Tato změna je představena ve verzi EF Core 3,0-Preview 7.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-848">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="d9bf8-849">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-849">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-850">Prostorové balíčky byly dříve závislé na 1.15.1 verze NetTopologySuite.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-850">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="d9bf8-851">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-851">**New behavior**</span></span>

<span data-ttu-id="d9bf8-852">Náš balíček jsme aktualizovali tak, aby byl závislý na verzi 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-852">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="d9bf8-853">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-853">**Why**</span></span>

<span data-ttu-id="d9bf8-854">2\.0.0 verze NetTopologySuite má za cíl řešit několik problémů s použitelností, ke kterým se EF Core uživatelé setkali.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-854">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="d9bf8-855">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-855">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-856">NetTopologySuite verze 2.0.0 obsahuje některé průlomové změny.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-856">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="d9bf8-857">Podrobnosti najdete v poznámkách k [verzi](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) .</span><span class="sxs-lookup"><span data-stu-id="d9bf8-857">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="d9bf8-858">Je nutné nakonfigurovat více dvojznačných relací odkazujících na sebe.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-858">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="d9bf8-859">Sledování problému #13573</span><span class="sxs-lookup"><span data-stu-id="d9bf8-859">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="d9bf8-860">Tato změna je představena ve verzi EF Core 3,0-Preview 6.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-860">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="d9bf8-861">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-861">**Old behavior**</span></span>

<span data-ttu-id="d9bf8-862">Typ entity s více jednosměrnou navigační vlastností a s vyhovující FKs byl nesprávně nakonfigurován jako jeden vztah.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-862">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="d9bf8-863">Příklad:</span><span class="sxs-lookup"><span data-stu-id="d9bf8-863">For example:</span></span>

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

<span data-ttu-id="d9bf8-864">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-864">**New behavior**</span></span>

<span data-ttu-id="d9bf8-865">Tento scénář je nyní zjištěn v sestavování modelu a je vyvolána výjimka označující, že je model dvojznačný.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-865">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="d9bf8-866">**Proč**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-866">**Why**</span></span>

<span data-ttu-id="d9bf8-867">Výsledný model byl nejednoznačný a pravděpodobně bude pro tento případ obvykle špatný.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-867">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="d9bf8-868">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="d9bf8-868">**Mitigations**</span></span>

<span data-ttu-id="d9bf8-869">Použijte úplnou konfiguraci relace.</span><span class="sxs-lookup"><span data-stu-id="d9bf8-869">Use full configuration of the relationship.</span></span> <span data-ttu-id="d9bf8-870">Příklad:</span><span class="sxs-lookup"><span data-stu-id="d9bf8-870">For example:</span></span>

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
