---
title: Přerušující změny v EF Core 3,0 – EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 1f63593631017a61c39ccab9216adbc4663700e7
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/20/2019
ms.locfileid: "71148906"
---
# <a name="breaking-changes-included-in-ef-core-30"></a><span data-ttu-id="5ac0e-102">Přerušující změny zahrnuté v EF Core 3,0</span><span class="sxs-lookup"><span data-stu-id="5ac0e-102">Breaking changes included in EF Core 3.0</span></span>
<span data-ttu-id="5ac0e-103">Následující změny rozhraní API a chování mají možnost rušit existující aplikace při jejich upgradu na 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-103">The following API and behavior changes have the potential to break existing applications when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="5ac0e-104">Změny, které očekáváme jenom o to, aby ovlivnili pouze poskytovatele databází, jsou popsané v části [změny zprostředkovatele](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="5ac0e-104">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="5ac0e-105">Přerušení z jedné verze 3,0 Preview do jiné 3,0 Preview nejsou popsané tady.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-105">Breaks from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="summary"></a><span data-ttu-id="5ac0e-106">Souhrn</span><span class="sxs-lookup"><span data-stu-id="5ac0e-106">Summary</span></span>

| <span data-ttu-id="5ac0e-107">**Zásadní změna**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-107">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="5ac0e-108">**Dopad**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-108">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="5ac0e-109">Dotazy LINQ již nejsou vyhodnocovány na klientovi.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-109">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="5ac0e-110">Vysoká</span><span class="sxs-lookup"><span data-stu-id="5ac0e-110">High</span></span>       |
| [<span data-ttu-id="5ac0e-111">EF Core 3,0 cíle .NET Standard 2,1 místo .NET Standard 2,0</span><span class="sxs-lookup"><span data-stu-id="5ac0e-111">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="5ac0e-112">Vysoká</span><span class="sxs-lookup"><span data-stu-id="5ac0e-112">High</span></span>      |
| [<span data-ttu-id="5ac0e-113">EF Core nástroj příkazového řádku dotnet EF již není součástí .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="5ac0e-113">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="5ac0e-114">Vysoká</span><span class="sxs-lookup"><span data-stu-id="5ac0e-114">High</span></span>      |
| [<span data-ttu-id="5ac0e-115">Z tabulek, ExecuteSql a ExecuteSqlAsync byly přejmenovány.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-115">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="5ac0e-116">Vysoká</span><span class="sxs-lookup"><span data-stu-id="5ac0e-116">High</span></span>      |
| [<span data-ttu-id="5ac0e-117">Typy dotazů jsou konsolidovány s typy entit</span><span class="sxs-lookup"><span data-stu-id="5ac0e-117">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="5ac0e-118">Vysoká</span><span class="sxs-lookup"><span data-stu-id="5ac0e-118">High</span></span>      |
| [<span data-ttu-id="5ac0e-119">Entity Framework Core už není součástí sdílené ASP.NET Core architektury.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-119">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="5ac0e-120">Střední</span><span class="sxs-lookup"><span data-stu-id="5ac0e-120">Medium</span></span>      |
| [<span data-ttu-id="5ac0e-121">Odstranění kaskádových operací se teď ve výchozím nastavení provádí hned.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-121">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="5ac0e-122">Střední</span><span class="sxs-lookup"><span data-stu-id="5ac0e-122">Medium</span></span>      |
| [<span data-ttu-id="5ac0e-123">DeleteBehavior. restrict má sémantiku čištění.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-123">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="5ac0e-124">Střední</span><span class="sxs-lookup"><span data-stu-id="5ac0e-124">Medium</span></span>      |
| [<span data-ttu-id="5ac0e-125">Změnilo se konfigurační rozhraní API pro vztahy vlastněných typů.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-125">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="5ac0e-126">Střední</span><span class="sxs-lookup"><span data-stu-id="5ac0e-126">Medium</span></span>      |
| [<span data-ttu-id="5ac0e-127">Každá vlastnost používá nezávislou generaci celočíselného klíče v paměti.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-127">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="5ac0e-128">Střední</span><span class="sxs-lookup"><span data-stu-id="5ac0e-128">Medium</span></span>      |
| [<span data-ttu-id="5ac0e-129">Žádné dotazy pro sledování neprovádějí překlad identity</span><span class="sxs-lookup"><span data-stu-id="5ac0e-129">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="5ac0e-130">Střední</span><span class="sxs-lookup"><span data-stu-id="5ac0e-130">Medium</span></span>      |
| [<span data-ttu-id="5ac0e-131">Změny rozhraní API pro metadata</span><span class="sxs-lookup"><span data-stu-id="5ac0e-131">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="5ac0e-132">Střední</span><span class="sxs-lookup"><span data-stu-id="5ac0e-132">Medium</span></span>      |
| [<span data-ttu-id="5ac0e-133">Změny rozhraní API pro konkrétního zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="5ac0e-133">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="5ac0e-134">Střední</span><span class="sxs-lookup"><span data-stu-id="5ac0e-134">Medium</span></span>      |
| [<span data-ttu-id="5ac0e-135">UseRowNumberForPaging se odebral.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-135">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="5ac0e-136">Střední</span><span class="sxs-lookup"><span data-stu-id="5ac0e-136">Medium</span></span>      |
| [<span data-ttu-id="5ac0e-137">Metody Z tabulek se dají zadat jenom v kořenech dotazů.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-137">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="5ac0e-138">Nízká</span><span class="sxs-lookup"><span data-stu-id="5ac0e-138">Low</span></span>      |
| [<span data-ttu-id="5ac0e-139">~~Provádění dotazu se protokoluje na úrovni ladění~~ . Vrátit</span><span class="sxs-lookup"><span data-stu-id="5ac0e-139">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="5ac0e-140">Nízká</span><span class="sxs-lookup"><span data-stu-id="5ac0e-140">Low</span></span>      |
| [<span data-ttu-id="5ac0e-141">Dočasné hodnoty klíčů už nejsou nastavené na instance entit.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-141">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="5ac0e-142">Nízká</span><span class="sxs-lookup"><span data-stu-id="5ac0e-142">Low</span></span>      |
| [<span data-ttu-id="5ac0e-143">DetectChanges respektuje hodnoty klíčů generované úložištěm.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-143">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="5ac0e-144">Nízká</span><span class="sxs-lookup"><span data-stu-id="5ac0e-144">Low</span></span>      |
| [<span data-ttu-id="5ac0e-145">Závislé entity, které sdílí tabulku s objektem zabezpečení, jsou teď volitelné.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-145">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="5ac0e-146">Nízká</span><span class="sxs-lookup"><span data-stu-id="5ac0e-146">Low</span></span>      |
| [<span data-ttu-id="5ac0e-147">Všechny entity sdílející tabulku se sloupcem souběžného tokenu musí být namapovány na vlastnost.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-147">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="5ac0e-148">Nízká</span><span class="sxs-lookup"><span data-stu-id="5ac0e-148">Low</span></span>      |
| [<span data-ttu-id="5ac0e-149">Zděděné vlastnosti z nemapovaných typů jsou nyní namapovány na jeden sloupec pro všechny odvozené typy.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-149">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="5ac0e-150">Nízká</span><span class="sxs-lookup"><span data-stu-id="5ac0e-150">Low</span></span>      |
| [<span data-ttu-id="5ac0e-151">Konvence vlastností cizího klíče už neodpovídá stejnému názvu jako vlastnost Principal.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-151">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="5ac0e-152">Nízká</span><span class="sxs-lookup"><span data-stu-id="5ac0e-152">Low</span></span>      |
| [<span data-ttu-id="5ac0e-153">Připojení k databázi je teď zavřené, pokud už nepoužíváte dřív, než se dokončí jeho objekt TransactionScope.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-153">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="5ac0e-154">Nízká</span><span class="sxs-lookup"><span data-stu-id="5ac0e-154">Low</span></span>      |
| [<span data-ttu-id="5ac0e-155">Ve výchozím nastavení se používají pole pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-155">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="5ac0e-156">Nízká</span><span class="sxs-lookup"><span data-stu-id="5ac0e-156">Low</span></span>      |
| [<span data-ttu-id="5ac0e-157">Vyvolat, zda je nalezeno více kompatibilních zálohovaných polí</span><span class="sxs-lookup"><span data-stu-id="5ac0e-157">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="5ac0e-158">Nízká</span><span class="sxs-lookup"><span data-stu-id="5ac0e-158">Low</span></span>      |
| [<span data-ttu-id="5ac0e-159">Názvy vlastností pouze pro pole se musí shodovat s názvem pole.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-159">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="5ac0e-160">Nízká</span><span class="sxs-lookup"><span data-stu-id="5ac0e-160">Low</span></span>      |
| [<span data-ttu-id="5ac0e-161">AddDbContext/AddDbContextPool už nevolá AddLogging a AddMemoryCache.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-161">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="5ac0e-162">Nízká</span><span class="sxs-lookup"><span data-stu-id="5ac0e-162">Low</span></span>      |
| [<span data-ttu-id="5ac0e-163">DbContext. entry teď provádí místní DetectChanges.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-163">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="5ac0e-164">Nízká</span><span class="sxs-lookup"><span data-stu-id="5ac0e-164">Low</span></span>      |
| [<span data-ttu-id="5ac0e-165">Klíče řetězce a pole bajtů nejsou ve výchozím nastavení generovány klientem.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-165">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="5ac0e-166">Nízká</span><span class="sxs-lookup"><span data-stu-id="5ac0e-166">Low</span></span>      |
| [<span data-ttu-id="5ac0e-167">ILoggerFactory je teď služba s vymezeným oborem.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-167">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="5ac0e-168">Nízká</span><span class="sxs-lookup"><span data-stu-id="5ac0e-168">Low</span></span>      |
| [<span data-ttu-id="5ac0e-169">Opožděné načítání proxy serverů už nepředpokládá navigační vlastnosti, jsou plně načtené.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-169">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="5ac0e-170">Nízká</span><span class="sxs-lookup"><span data-stu-id="5ac0e-170">Low</span></span>      |
| [<span data-ttu-id="5ac0e-171">Nadměrné vytváření interních zprostředkovatelů služeb je teď ve výchozím nastavení chyba.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-171">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="5ac0e-172">Nízká</span><span class="sxs-lookup"><span data-stu-id="5ac0e-172">Low</span></span>      |
| [<span data-ttu-id="5ac0e-173">Nové chování pro HasOne/HasMany se volá s jedním řetězcem.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-173">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="5ac0e-174">Nízká</span><span class="sxs-lookup"><span data-stu-id="5ac0e-174">Low</span></span>      |
| [<span data-ttu-id="5ac0e-175">Návratový typ pro několik asynchronních metod byl změněn z úlohy na ValueTask</span><span class="sxs-lookup"><span data-stu-id="5ac0e-175">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="5ac0e-176">Nízká</span><span class="sxs-lookup"><span data-stu-id="5ac0e-176">Low</span></span>      |
| [<span data-ttu-id="5ac0e-177">Relační: anotace TypeMapping je nyní pouze TypeMapping</span><span class="sxs-lookup"><span data-stu-id="5ac0e-177">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="5ac0e-178">Nízká</span><span class="sxs-lookup"><span data-stu-id="5ac0e-178">Low</span></span>      |
| [<span data-ttu-id="5ac0e-179">ToTable na odvozeném typu vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-179">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="5ac0e-180">Nízká</span><span class="sxs-lookup"><span data-stu-id="5ac0e-180">Low</span></span>      |
| [<span data-ttu-id="5ac0e-181">EF Core už neposílá direktivu pragma pro vynucení KOFK SQLite.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-181">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="5ac0e-182">Nízká</span><span class="sxs-lookup"><span data-stu-id="5ac0e-182">Low</span></span>      |
| [<span data-ttu-id="5ac0e-183">Microsoft. EntityFrameworkCore. sqlite teď závisí na SQLitePCLRaw. bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="5ac0e-183">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="5ac0e-184">Nízká</span><span class="sxs-lookup"><span data-stu-id="5ac0e-184">Low</span></span>      |
| [<span data-ttu-id="5ac0e-185">Hodnoty GUID se teď ukládají jako TEXT na SQLite.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-185">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="5ac0e-186">Nízká</span><span class="sxs-lookup"><span data-stu-id="5ac0e-186">Low</span></span>      |
| [<span data-ttu-id="5ac0e-187">Hodnoty char se teď ukládají jako TEXT na SQLite.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-187">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="5ac0e-188">Nízká</span><span class="sxs-lookup"><span data-stu-id="5ac0e-188">Low</span></span>      |
| [<span data-ttu-id="5ac0e-189">ID migrace se teď generují pomocí kalendáře invariantní jazykové verze.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-189">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="5ac0e-190">Nízká</span><span class="sxs-lookup"><span data-stu-id="5ac0e-190">Low</span></span>      |
| [<span data-ttu-id="5ac0e-191">Informace o rozšíření/metadata se odebraly z IDbContextOptionsExtension.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-191">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="5ac0e-192">Nízká</span><span class="sxs-lookup"><span data-stu-id="5ac0e-192">Low</span></span>      |
| [<span data-ttu-id="5ac0e-193">LogQueryPossibleExceptionWithAggregateOperator se přejmenovalo.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-193">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="5ac0e-194">Nízká</span><span class="sxs-lookup"><span data-stu-id="5ac0e-194">Low</span></span>      |
| [<span data-ttu-id="5ac0e-195">Vysvětlení rozhraní API pro názvy omezení cizího klíče</span><span class="sxs-lookup"><span data-stu-id="5ac0e-195">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="5ac0e-196">Nízká</span><span class="sxs-lookup"><span data-stu-id="5ac0e-196">Low</span></span>      |
| [<span data-ttu-id="5ac0e-197">IRelationalDatabaseCreator. HasTables/HasTablesAsync byly zveřejněny.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-197">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="5ac0e-198">Nízká</span><span class="sxs-lookup"><span data-stu-id="5ac0e-198">Low</span></span>      |
| [<span data-ttu-id="5ac0e-199">Microsoft. EntityFrameworkCore. Design je teď balíček DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-199">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="5ac0e-200">Nízká</span><span class="sxs-lookup"><span data-stu-id="5ac0e-200">Low</span></span>      |
| [<span data-ttu-id="5ac0e-201">SQLitePCL. Raw aktualizováno na verzi 2.0.0</span><span class="sxs-lookup"><span data-stu-id="5ac0e-201">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="5ac0e-202">Nízká</span><span class="sxs-lookup"><span data-stu-id="5ac0e-202">Low</span></span>      |
| [<span data-ttu-id="5ac0e-203">NetTopologySuite aktualizace na verzi 2.0.0</span><span class="sxs-lookup"><span data-stu-id="5ac0e-203">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="5ac0e-204">Nízká</span><span class="sxs-lookup"><span data-stu-id="5ac0e-204">Low</span></span>      |
| [<span data-ttu-id="5ac0e-205">Je nutné nakonfigurovat více dvojznačných relací odkazujících na sebe.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-205">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="5ac0e-206">Nízká</span><span class="sxs-lookup"><span data-stu-id="5ac0e-206">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="5ac0e-207">Dotazy LINQ již nejsou vyhodnocovány na klientovi.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-207">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="5ac0e-208">[Sledování problému #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[taky zobrazit problémy #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="5ac0e-208">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="5ac0e-209">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-209">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="5ac0e-210">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-210">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-211">Před 3,0, když EF Core nedokázal převést výraz, který byl součástí dotazu na buď SQL, nebo parametr, automaticky vyhodnotí výraz na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-211">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="5ac0e-212">Ve výchozím nastavení vyhodnocování klientů u potenciálně nákladných výrazů aktivovalo pouze upozornění.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-212">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="5ac0e-213">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-213">**New behavior**</span></span>

<span data-ttu-id="5ac0e-214">Počínaje 3,0 EF Core v rámci projekce nejvyšší úrovně (posledního `Select()` volání v dotazu) jenom výrazy vyhodnotit na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-214">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="5ac0e-215">Pokud výrazy v jakékoli jiné části dotazu nelze převést na buď SQL, nebo parametr, je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-215">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="5ac0e-216">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-216">**Why**</span></span>

<span data-ttu-id="5ac0e-217">Automatické hodnocení dotazů umožňuje provádět mnoho dotazů i v případě, že není možné přeložit jejich důležité části.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-217">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="5ac0e-218">Toto chování může vést k neočekávanému a potenciálně škodlivému chování, které může být jen zřejmé v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-218">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="5ac0e-219">Například podmínka ve `Where()` volání, která se nedá přeložit, může způsobit přenesení všech řádků z databázového serveru a filtr, který se má použít na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-219">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="5ac0e-220">Tato situace může snadno přejít zpět, pokud tabulka obsahuje jenom několik řádků ve vývoji, ale při přesunu aplikace do produkčního prostředí zasáhnout, kde tabulka může obsahovat miliony řádků.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-220">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="5ac0e-221">Upozornění na vyhodnocení klientů se také během vývoje ukázala jako příliš jednoduchá.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-221">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="5ac0e-222">Kromě toho může automatické hodnocení klienta vést k problémům s tím, že vylepšení překladu dotazů pro konkrétní výrazy způsobilo nezamýšlené neúmyslné změny mezi verzemi.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-222">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="5ac0e-223">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-223">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-224">Pokud dotaz nelze plně přeložit, pak buď Přepište dotaz do formuláře, který lze přeložit, nebo použijte `AsEnumerable()`, `ToList()`nebo podobným způsobem explicitně přeneste data zpět do klienta, kde lze následně dále zpracovávat pomocí LINQ-to-Objects.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-224">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="5ac0e-225">EF Core 3,0 cíle .NET Standard 2,1 místo .NET Standard 2,0</span><span class="sxs-lookup"><span data-stu-id="5ac0e-225">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="5ac0e-226">Sledování problému #15498</span><span class="sxs-lookup"><span data-stu-id="5ac0e-226">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

<span data-ttu-id="5ac0e-227">Tato změna je představena ve verzi EF Core 3,0-Preview 7.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-227">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="5ac0e-228">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-228">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-229">Před 3,0 EF Core cílené .NET Standard 2,0 a spustí se na všech platformách, které podporují tento standard, včetně .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-229">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="5ac0e-230">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-230">**New behavior**</span></span>

<span data-ttu-id="5ac0e-231">Počínaje 3,0 se EF Core cíle .NET Standard 2,1 a spustí se na všech platformách, které podporují tento standard.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-231">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="5ac0e-232">To nezahrnuje .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-232">This does not include .NET Framework.</span></span>

<span data-ttu-id="5ac0e-233">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-233">**Why**</span></span>

<span data-ttu-id="5ac0e-234">Toto je součást strategického rozhodnutí napříč technologiemi .NET a zaměřuje se na energii na platformě .NET Core a dalších moderních platformách .NET, jako je Xamarin.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-234">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="5ac0e-235">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-235">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-236">Zvažte přechod na moderní platformu .NET.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-236">Consider moving to a modern .NET platform.</span></span> <span data-ttu-id="5ac0e-237">Pokud to není možné, pak pokračujte v používání EF Core 2,1 nebo EF Core 2,2, které podporují .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-237">If this is not possible, then continue to use EF Core 2.1 or EF Core 2.2, both of which support .NET Framework.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="5ac0e-238">Entity Framework Core už není součástí sdílené ASP.NET Core architektury.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-238">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="5ac0e-239">Sledování oznámení o problémech # 325</span><span class="sxs-lookup"><span data-stu-id="5ac0e-239">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="5ac0e-240">Tato změna je představena ve verzi ASP.NET Core 3,0-Preview 1.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-240">This change is introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="5ac0e-241">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-241">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-242">Před ASP.NET Core 3,0 se při přidání odkazu na balíček do `Microsoft.AspNetCore.App` nebo `Microsoft.AspNetCore.All`zahrnovala EF Core a někteří poskytovatelé EF Core dat jako poskytovatel SQL Server.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-242">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="5ac0e-243">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-243">**New behavior**</span></span>

<span data-ttu-id="5ac0e-244">Počínaje 3,0 ASP.NET Core sdílené rozhraní nezahrnuje EF Core nebo žádné poskytovatele EF Core dat.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-244">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="5ac0e-245">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-245">**Why**</span></span>

<span data-ttu-id="5ac0e-246">Před touto změnou EF Core nutné jiné kroky v závislosti na tom, zda je aplikace cílena ASP.NET Core a SQL Server.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-246">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="5ac0e-247">Upgrade ASP.NET Core taky vynutil upgrade EF Core a poskytovatele SQL Server, který není vždycky žádoucí.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-247">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="5ac0e-248">V důsledku této změny je prostředí získání EF Core stejné u všech poskytovatelů, podporovaných implementací rozhraní .NET a typů aplikací.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-248">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="5ac0e-249">Vývojáři teď můžou přesně řídit, když EF Core a EF Core se upgradují poskytovatelé dat.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-249">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="5ac0e-250">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-250">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-251">Chcete-li použít EF Core v aplikaci ASP.NET Core 3,0 nebo v jakékoli jiné podporované aplikaci, explicitně přidejte odkaz na balíček EF Coreho poskytovatele databáze, který bude aplikace používat.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-251">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="5ac0e-252">EF Core nástroj příkazového řádku dotnet EF již není součástí .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="5ac0e-252">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="5ac0e-253">Sledování problému #14016</span><span class="sxs-lookup"><span data-stu-id="5ac0e-253">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="5ac0e-254">Tato změna je představena v EF Core 3,0-Preview 4 a odpovídající verzi .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-254">This change is introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="5ac0e-255">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-255">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-256">Před 3,0 `dotnet ef` byl nástroj součástí .NET Core SDK a byl snadno dostupný pro použití z příkazového řádku z libovolného projektu bez nutnosti dalších kroků.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-256">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="5ac0e-257">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-257">**New behavior**</span></span>

<span data-ttu-id="5ac0e-258">Počínaje 3,0 sada .NET SDK nezahrnuje `dotnet ef` nástroj, takže před tím, než ho budete moct použít, je nutné explicitně nainstalovat jako místní nebo globální nástroj.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-258">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="5ac0e-259">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-259">**Why**</span></span>

<span data-ttu-id="5ac0e-260">Tato změna nám umožňuje distribuovat a aktualizovat `dotnet ef` jako běžný nástroj rozhraní .NET CLI v NuGet, a to v souladu se skutečností, že EF Core 3,0 je také vždy distribuován jako balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-260">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="5ac0e-261">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-261">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-262">Aby bylo možné spravovat migrace nebo uživatelské rozhraní a `DbContext`, nainstalujte `dotnet-ef` nástroj jako globální nástroj:</span><span class="sxs-lookup"><span data-stu-id="5ac0e-262">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef --version 3.0.0-*
  ```

<span data-ttu-id="5ac0e-263">Můžete také získat místní nástroj při obnovování závislostí projektu, který deklaruje jako závislost nástrojů pomocí [souboru manifestu nástroje](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="5ac0e-263">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="5ac0e-264">Z tabulek, ExecuteSql a ExecuteSqlAsync byly přejmenovány.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-264">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="5ac0e-265">Sledování problému #10996</span><span class="sxs-lookup"><span data-stu-id="5ac0e-265">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="5ac0e-266">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-266">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="5ac0e-267">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-267">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-268">Před EF Core 3,0 byly tyto názvy metod přetíženy, aby fungovaly s normálním řetězcem nebo řetězcem, které by měly být interpolované na SQL a parametry.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-268">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="5ac0e-269">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-269">**New behavior**</span></span>

<span data-ttu-id="5ac0e-270">Počínaje EF Core 3,0, použijte `FromSqlRaw`, `ExecuteSqlRaw`a `ExecuteSqlRawAsync` k vytvoření parametrizovaného dotazu, kde jsou parametry předány nezávisle na řetězci dotazu.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-270">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="5ac0e-271">Příklad:</span><span class="sxs-lookup"><span data-stu-id="5ac0e-271">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="5ac0e-272">Použijte `FromSqlInterpolated`, `ExecuteSqlInterpolated` a`ExecuteSqlInterpolatedAsync` k vytvoření parametrizovaného dotazu, kde jsou parametry předány jako součást interpolované řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-272">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="5ac0e-273">Příklad:</span><span class="sxs-lookup"><span data-stu-id="5ac0e-273">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="5ac0e-274">Všimněte si, že obě výše uvedené dotazy vytvoří stejný parametrizovaný SQL se stejnými parametry SQL.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-274">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="5ac0e-275">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-275">**Why**</span></span>

<span data-ttu-id="5ac0e-276">Přetížení metody, jako to, usnadňuje náhodné volání nezpracované řetězcové metody, pokud by záměr byl zavolat interpolovaná řetězcová metoda a druhá možnost kolem.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-276">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="5ac0e-277">To může vést k tomu, že dotazy nejsou parametrizované, pokud by měly být.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-277">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="5ac0e-278">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-278">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-279">Přepněte na použití nových názvů metod.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-279">Switch to use the new method names.</span></span>

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="5ac0e-280">Metody Z tabulek se dají zadat jenom v kořenech dotazů.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-280">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="5ac0e-281">Sledování problému #15704</span><span class="sxs-lookup"><span data-stu-id="5ac0e-281">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="5ac0e-282">Tato změna je představena ve verzi EF Core 3,0-Preview 6.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-282">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="5ac0e-283">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-283">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-284">Před EF Core 3,0 `FromSql` lze metodu zadat kdekoli v dotazu.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-284">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="5ac0e-285">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-285">**New behavior**</span></span>

<span data-ttu-id="5ac0e-286">Počínaje EF Core `FromSqlRaw` 3,0 lze nové metody a `FromSqlInterpolated` (které nahradí `FromSql`) zadat pouze v kořenech `DbSet<>`dotazu, tj. přímo na.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-286">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="5ac0e-287">Pokud se pokusíte zadat jiné místo jinak, dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-287">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="5ac0e-288">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-288">**Why**</span></span>

<span data-ttu-id="5ac0e-289">Určení `FromSql` kdekoli jinde než u a `DbSet` neobsahovalo žádné přidané významy nebo přidání hodnoty a může v některých scénářích způsobit nejednoznačnost.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-289">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="5ac0e-290">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-290">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-291">`FromSql`volání by se měla přesunout přímo na, `DbSet` na které se vztahují.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-291">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="5ac0e-292">Žádné dotazy pro sledování neprovádějí překlad identity</span><span class="sxs-lookup"><span data-stu-id="5ac0e-292">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="5ac0e-293">Sledování problému #13518</span><span class="sxs-lookup"><span data-stu-id="5ac0e-293">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="5ac0e-294">Tato změna je představena ve verzi EF Core 3,0-Preview 6.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-294">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="5ac0e-295">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-295">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-296">Před EF Core 3,0 se stejná instance entity používá pro všechny výskyty entity se zadaným typem a ID.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-296">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="5ac0e-297">To odpovídá chování sledovacích dotazů.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-297">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="5ac0e-298">Například tento dotaz:</span><span class="sxs-lookup"><span data-stu-id="5ac0e-298">For example, this query:</span></span>

```C#
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="5ac0e-299">vrátí stejnou `Category` instanci pro každý `Product` , který je spojen s danou kategorií.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-299">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="5ac0e-300">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-300">**New behavior**</span></span>

<span data-ttu-id="5ac0e-301">Počínaje EF Core 3,0 budou vytvořeny různé instance entit při výskytu entity se zadaným typem a ID na různých místech vráceného grafu.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-301">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="5ac0e-302">Například dotaz výše bude nyní vracet novou `Category` instanci pro každou `Product` , i když jsou ke stejné kategorii přidruženy dva produkty.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-302">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="5ac0e-303">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-303">**Why**</span></span>

<span data-ttu-id="5ac0e-304">Překlad identity (to znamená, že určení, že entita má stejný typ a ID jako dříve zjištěná entita) přidává další výkon a režii paměti.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-304">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="5ac0e-305">Obvykle se spustí čítač, aby se v prvním místě nepoužily žádné dotazy na sledování.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-305">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="5ac0e-306">I když může být v některých případech užitečný překlad identity, není potřeba, pokud se entity mají serializovat a odeslat klientovi, což je běžné pro žádné dotazy pro sledování.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-306">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="5ac0e-307">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-307">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-308">Pokud je vyžadováno rozlišení identity, použijte dotaz sledování.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-308">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="5ac0e-309">~~Provádění dotazu se protokoluje na úrovni ladění~~ . Vrátit</span><span class="sxs-lookup"><span data-stu-id="5ac0e-309">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="5ac0e-310">Sledování problému #14523</span><span class="sxs-lookup"><span data-stu-id="5ac0e-310">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="5ac0e-311">Tato změna se vrátí v EF Core 3,0-Preview 7.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-311">This change is reverted in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="5ac0e-312">Tuto změnu jsme vrátili, protože nová konfigurace v EF Core 3,0 umožňuje, aby byla úroveň protokolu pro libovolnou událost specifikována aplikací.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-312">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="5ac0e-313">Například chcete-li přepnout protokolování SQL na `Debug`, explicitně nakonfigurovat úroveň v `OnConfiguring` nebo `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="5ac0e-313">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="5ac0e-314">Dočasné hodnoty klíčů už nejsou nastavené na instance entit.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-314">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="5ac0e-315">Sledování problému #12378</span><span class="sxs-lookup"><span data-stu-id="5ac0e-315">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="5ac0e-316">Tato změna je představena ve verzi EF Core 3,0-Preview 2.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-316">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="5ac0e-317">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-317">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-318">Před EF Core 3,0 byly přiřazeny dočasné hodnoty ke všem klíčovým vlastnostem, které by později měly skutečnou hodnotu vygenerovanou databází.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-318">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="5ac0e-319">Obvykle jsou tyto dočasné hodnoty velkými zápornými čísly.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-319">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="5ac0e-320">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-320">**New behavior**</span></span>

<span data-ttu-id="5ac0e-321">Počínaje 3,0 EF Core ukládá hodnotu dočasného klíče jako součást informací o sledování entity a ponechá vlastnost klíče beze změny.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-321">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="5ac0e-322">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-322">**Why**</span></span>

<span data-ttu-id="5ac0e-323">Tato změna byla provedena, aby se předešlo omylům hodnotám klíčů v případě, že entita, která byla dříve sledována pomocí nějaké `DbContext` instance, je přesunuta do jiné `DbContext` instance.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-323">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="5ac0e-324">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-324">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-325">Aplikace, které přiřazují hodnoty primárního klíče k cizím klíčům k vytvoření přidružení mezi entitami, můžou záviset na starém chování, pokud jsou primární klíče generované úložištěm a patří do entit ve `Added` stavu.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-325">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="5ac0e-326">K tomu je možné se vyhnout:</span><span class="sxs-lookup"><span data-stu-id="5ac0e-326">This can be avoided by:</span></span>
* <span data-ttu-id="5ac0e-327">Nepoužívejte klíče generované úložištěm.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-327">Not using store-generated keys.</span></span>
* <span data-ttu-id="5ac0e-328">Nastavení vlastností navigace pro vytváření relací místo nastavení hodnot cizích klíčů.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-328">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="5ac0e-329">Získá aktuální dočasné hodnoty klíče z informací o sledování entity.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-329">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="5ac0e-330">Například vrátí dočasnou hodnotu, `context.Entry(blog).Property(e => e.Id).CurrentValue` i když `blog.Id` nebyla nastavena.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-330">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="5ac0e-331">DetectChanges respektuje hodnoty klíčů generované úložištěm.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-331">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="5ac0e-332">Sledování problému #14616</span><span class="sxs-lookup"><span data-stu-id="5ac0e-332">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="5ac0e-333">Tato změna je představena ve verzi EF Core 3,0-Preview 3.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-333">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="5ac0e-334">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-334">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-335">Předtím, než EF Core 3,0, nalezené `DetectChanges` Nesledované entity by byly sledovány `Added` ve stavu a vloženy jako nový řádek při `SaveChanges` volání.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-335">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="5ac0e-336">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-336">**New behavior**</span></span>

<span data-ttu-id="5ac0e-337">Počínaje EF Core 3,0 platí, že pokud entita používá vygenerované hodnoty klíčů a je nastavená nějaká hodnota klíče, bude se entita sledovat ve `Modified` stavu.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-337">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="5ac0e-338">To znamená, že se předpokládá, že řádek pro entitu existuje a že bude při `SaveChanges` volání aktualizována.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-338">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="5ac0e-339">Pokud hodnota klíče není nastavená, nebo pokud typ entity nepoužívá vygenerované klíče, bude se nová entita dál sledovat jako `Added` v předchozích verzích.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-339">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="5ac0e-340">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-340">**Why**</span></span>

<span data-ttu-id="5ac0e-341">Tato změna byla provedena tak, aby byla pro práci s nepřipojenými grafy entit při použití klíčů generovaných úložištěm snazší.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-341">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="5ac0e-342">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-342">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-343">Tato změna může přerušit aplikaci, pokud je typ entity nakonfigurovaný na používání vygenerovaných klíčů, ale hodnoty klíčů jsou explicitně nastavené pro nové instance.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-343">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="5ac0e-344">Opravou je explicitně nakonfigurovat klíčové vlastnosti tak, aby nepoužívaly vygenerované hodnoty.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-344">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="5ac0e-345">Například s rozhraním API Fluent:</span><span class="sxs-lookup"><span data-stu-id="5ac0e-345">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="5ac0e-346">Nebo s poznámkami k datům:</span><span class="sxs-lookup"><span data-stu-id="5ac0e-346">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="5ac0e-347">Odstranění kaskádových operací se teď ve výchozím nastavení provádí hned.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-347">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="5ac0e-348">Sledování problému #10114</span><span class="sxs-lookup"><span data-stu-id="5ac0e-348">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="5ac0e-349">Tato změna je představena ve verzi EF Core 3,0-Preview 3.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-349">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="5ac0e-350">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-350">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-351">Před 3,0 EF Core aplikovány kaskádové akce (odstraňování závislých entit, když se odstraní požadovaný objekt zabezpečení nebo když je vztah k požadovanému objektu zabezpečení vážně) nevznikl, dokud není voláno SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-351">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="5ac0e-352">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-352">**New behavior**</span></span>

<span data-ttu-id="5ac0e-353">Od 3,0 EF Core aplikuje kaskádové akce hned po zjištění spouštěcí podmínky.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-353">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="5ac0e-354">Například volání `context.Remove()` na odstranění objektu zabezpečení bude mít za následek, že jsou všechny sledované požadované závislé položky nastaveny také na `Deleted` hodnotu okamžitě.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-354">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="5ac0e-355">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-355">**Why**</span></span>

<span data-ttu-id="5ac0e-356">Tato změna byla provedena za účelem zlepšení prostředí pro scénáře vytváření datových vazeb a auditování, kde je důležité pochopit, které entity budou _před_ `SaveChanges` voláním odstraněny.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-356">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="5ac0e-357">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-357">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-358">Předchozí chování lze obnovit pomocí nastavení v `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-358">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="5ac0e-359">Příklad:</span><span class="sxs-lookup"><span data-stu-id="5ac0e-359">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="5ac0e-360">DeleteBehavior. restrict má sémantiku čištění.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-360">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="5ac0e-361">Sledování problému #12661</span><span class="sxs-lookup"><span data-stu-id="5ac0e-361">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="5ac0e-362">Tato změna je představena ve verzi EF Core 3,0-Preview 5.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-362">This change is introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="5ac0e-363">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-363">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-364">Před 3,0 se `DeleteBehavior.Restrict` v databázi vytvořily cizí klíče se `Restrict` sémantikou, ale zároveň se nezřetelně změnila interní oprava.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-364">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="5ac0e-365">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-365">**New behavior**</span></span>

<span data-ttu-id="5ac0e-366">Počínaje 3,0 `DeleteBehavior.Restrict` zajistí, že se cizí klíče vytvoří s `Restrict` sémantikou – to znamená bez kaskády; porušení omezení throw--bez vlivu na interní opravu EF.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-366">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="5ac0e-367">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-367">**Why**</span></span>

<span data-ttu-id="5ac0e-368">Tato změna byla provedena pro zlepšení prostředí pro použití `DeleteBehavior` intuitivním způsobem, a to bez neočekávaných vedlejších účinků.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-368">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="5ac0e-369">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-369">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-370">Předchozí chování lze obnovit pomocí `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-370">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="5ac0e-371">Typy dotazů jsou konsolidovány s typy entit</span><span class="sxs-lookup"><span data-stu-id="5ac0e-371">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="5ac0e-372">Sledování problému #14194</span><span class="sxs-lookup"><span data-stu-id="5ac0e-372">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="5ac0e-373">Tato změna je představena ve verzi EF Core 3,0-Preview 3.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-373">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="5ac0e-374">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-374">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-375">Před EF Core 3,0 byly [typy dotazů](xref:core/modeling/keyless-entity-types) prostředkem pro dotazování na data, která nedefinují primární klíč strukturovaným způsobem.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-375">Before EF Core 3.0, [query types](xref:core/modeling/keyless-entity-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="5ac0e-376">To znamená, že typ dotazu byl použit pro mapování typů entit bez klíčů (pravděpodobnější z pohledu, ale také z tabulky), zatímco byl použit regulární typ entity, když byl klíč k dispozici (pravděpodobnější z tabulky, ale případně z pohledu).</span><span class="sxs-lookup"><span data-stu-id="5ac0e-376">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="5ac0e-377">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-377">**New behavior**</span></span>

<span data-ttu-id="5ac0e-378">Typ dotazu se teď bude jednat jenom o typ entity bez primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-378">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="5ac0e-379">Typy entit bez klíčů mají stejné funkce jako typy dotazů v předchozích verzích.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-379">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="5ac0e-380">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-380">**Why**</span></span>

<span data-ttu-id="5ac0e-381">Tato změna byla provedena proto, aby se snížila nejasnost v souvislosti s typy dotazů.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-381">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="5ac0e-382">Konkrétně jsou bez klíčů typy entit a jsou ze své podstaty jen pro čtení, ale neměly by být použity pouze proto, že typ entity musí být jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-382">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="5ac0e-383">Podobně jsou často mapovány na zobrazení, ale to je pouze v případě, že zobrazení často nedefinují klíče.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-383">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="5ac0e-384">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-384">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-385">Následující části rozhraní API jsou teď zastaralé:</span><span class="sxs-lookup"><span data-stu-id="5ac0e-385">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="5ac0e-386">**`ModelBuilder.Query<>()`** – Místo `ModelBuilder.Entity<>().HasNoKey()` toho je nutné volat k označení typu entity, protože neobsahují žádné klíče.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-386">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="5ac0e-387">Tato služba by se ještě nenakonfigurovala pomocí konvence, aby nedocházelo k chybám konfigurace, když je primární klíč očekávaný, ale neodpovídá konvenci.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-387">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="5ac0e-388">**`DbQuery<>`** – Místo `DbSet<>` toho by se mělo použít.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-388">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="5ac0e-389">**`DbContext.Query<>()`** – Místo `DbContext.Set<>()` toho by se mělo použít.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-389">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="5ac0e-390">Změnilo se konfigurační rozhraní API pro vztahy vlastněných typů.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-390">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="5ac0e-391">[Sledování problému #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[sledování problému #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[#14153 sledování](https://github.com/aspnet/EntityFrameworkCore/issues/14153) problému</span><span class="sxs-lookup"><span data-stu-id="5ac0e-391">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="5ac0e-392">Tato změna je představena ve verzi EF Core 3,0-Preview 3.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-392">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="5ac0e-393">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-393">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-394">Před EF Core 3,0 byla konfigurace vztahu vlastníka provedena přímo po `OwnsOne` volání nebo. `OwnsMany`</span><span class="sxs-lookup"><span data-stu-id="5ac0e-394">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="5ac0e-395">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-395">**New behavior**</span></span>

<span data-ttu-id="5ac0e-396">Od EF Core 3,0 teď existuje Fluent API pro konfiguraci navigační vlastnosti pro vlastníka pomocí `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-396">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="5ac0e-397">Příklad:</span><span class="sxs-lookup"><span data-stu-id="5ac0e-397">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="5ac0e-398">Konfigurace vztahující se k vztahu mezi vlastníkem a vlastníkem by nyní měla být zřetězena `WithOwner()` podobně jako konfigurace ostatních vztahů.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-398">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="5ac0e-399">I když se konfigurace samotného vlastního typu bude i nadále zřetězit za `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-399">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="5ac0e-400">Příklad:</span><span class="sxs-lookup"><span data-stu-id="5ac0e-400">For example:</span></span>

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

<span data-ttu-id="5ac0e-401">Kromě toho `Entity()`, `HasOne()`že volání `Set()` ,, nebo s cílem cílového typu, nyní vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-401">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="5ac0e-402">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-402">**Why**</span></span>

<span data-ttu-id="5ac0e-403">Tato změna byla provedena za účelem vytvoření čisticího oddělení mezi konfigurací samotného typu a _vztahu k_ typu, který je vlastníkem.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-403">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="5ac0e-404">Tím se zase odeberou nejednoznačnosti a nejasnosti u metod, jako `HasForeignKey`je.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-404">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="5ac0e-405">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-405">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-406">Změňte konfiguraci vztahů vlastněných typů tak, aby používala novou plochu rozhraní API, jak je znázorněno v předchozím příkladu.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-406">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="5ac0e-407">Závislé entity, které sdílí tabulku s objektem zabezpečení, jsou teď volitelné.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-407">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="5ac0e-408">Sledování problému #9005</span><span class="sxs-lookup"><span data-stu-id="5ac0e-408">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="5ac0e-409">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-409">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="5ac0e-410">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-410">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-411">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="5ac0e-411">Consider the following model:</span></span>
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
<span data-ttu-id="5ac0e-412">Pokud `OrderDetails` je před EF Core 3,0 ve `Order` vlastnictví nebo explicitně namapována na stejnou tabulku `OrderDetails` , byla při přidávání nové `Order`instance vždy vyžadována instance.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-412">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="5ac0e-413">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-413">**New behavior**</span></span>

<span data-ttu-id="5ac0e-414">Počínaje 3,0 EF Core umožňuje přidat `Order` `OrderDetails` bez `OrderDetails` a mapování všech vlastností kromě primárního klíče na sloupce s možnou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-414">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="5ac0e-415">Při dotazování EF Core sady `OrderDetails` na `null` , pokud některá z jejích požadovaných vlastností nemá hodnotu, nebo pokud nemá žádné požadované vlastnosti kromě primárního klíče a všechny vlastnosti jsou `null`.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-415">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="5ac0e-416">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-416">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-417">Pokud má váš model sdílení tabulky závislé na všech volitelných sloupcích, ale u navigace, na kterou se odkazuje, se neočekává `null` , že by aplikace měla být upravena tak, aby zpracovávala případy, kdy je `null`navigace.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-417">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="5ac0e-418">Pokud to není možné, měla by být do daného typu entity přidána požadovaná vlastnost, nebo alespoň jedna vlastnost by k ní měla mít`null` přiřazenou hodnotu bez hodnoty.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-418">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="5ac0e-419">Všechny entity sdílející tabulku se sloupcem souběžného tokenu musí být namapovány na vlastnost.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-419">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="5ac0e-420">Sledování problému #14154</span><span class="sxs-lookup"><span data-stu-id="5ac0e-420">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="5ac0e-421">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-421">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="5ac0e-422">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-422">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-423">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="5ac0e-423">Consider the following model:</span></span>
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
<span data-ttu-id="5ac0e-424">Pokud `OrderDetails` je před EF Core 3,0, ve `Order` vlastnictví nebo explicitně namapovaných na `OrderDetails` stejnou tabulku, pak aktualizace nebude aktualizovat `Version` hodnotu u klienta a další aktualizace selže.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-424">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="5ac0e-425">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-425">**New behavior**</span></span>

<span data-ttu-id="5ac0e-426">Počínaje 3,0 EF Core rozšíří novou `Version` `Order` hodnotu, pokud je vlastní `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-426">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="5ac0e-427">V opačném případě je při ověřování modelu vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-427">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="5ac0e-428">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-428">**Why**</span></span>

<span data-ttu-id="5ac0e-429">Tato změna byla provedena, aby nedocházelo k zastaralé hodnotě tokenu souběžnosti, pokud je aktualizována pouze jedna z entit mapovaných na stejnou tabulku.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-429">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="5ac0e-430">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-430">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-431">Všechny entity, které sdílejí tabulku, musí obsahovat vlastnost, která je namapovaná na sloupec tokenu souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-431">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="5ac0e-432">Je možné, že ho vytvoříte ve stínovém stavu:</span><span class="sxs-lookup"><span data-stu-id="5ac0e-432">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="5ac0e-433">Zděděné vlastnosti z nemapovaných typů jsou nyní namapovány na jeden sloupec pro všechny odvozené typy.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-433">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="5ac0e-434">Sledování problému #13998</span><span class="sxs-lookup"><span data-stu-id="5ac0e-434">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="5ac0e-435">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-435">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="5ac0e-436">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-436">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-437">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="5ac0e-437">Consider the following model:</span></span>
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

<span data-ttu-id="5ac0e-438">Před EF Core 3,0 `ShippingAddress` bude vlastnost namapována na samostatné sloupce pro `BulkOrder` a `Order` ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-438">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="5ac0e-439">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-439">**New behavior**</span></span>

<span data-ttu-id="5ac0e-440">Počínaje 3,0 se EF Core pro `ShippingAddress`nedá vytvořit jenom jeden sloupec.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-440">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="5ac0e-441">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-441">**Why**</span></span>

<span data-ttu-id="5ac0e-442">Starý behavoir nebyl očekáván.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-442">The old behavoir was unexpected.</span></span>

<span data-ttu-id="5ac0e-443">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-443">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-444">Vlastnost může být stále explicitně namapována na samostatný sloupec odvozených typů:</span><span class="sxs-lookup"><span data-stu-id="5ac0e-444">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="5ac0e-445">Konvence vlastností cizího klíče už neodpovídá stejnému názvu jako vlastnost Principal.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-445">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="5ac0e-446">Sledování problému #13274</span><span class="sxs-lookup"><span data-stu-id="5ac0e-446">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="5ac0e-447">Tato změna je představena ve verzi EF Core 3,0-Preview 3.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-447">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="5ac0e-448">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-448">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-449">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="5ac0e-449">Consider the following model:</span></span>
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
<span data-ttu-id="5ac0e-450">Před EF Core 3,0 `CustomerId` se vlastnost používá pro cizí klíč podle konvence.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-450">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="5ac0e-451">Nicméně pokud `Order` je vlastněný typ, pak by to vedlo `CustomerId` také k tomu, že primární klíč a to není obvykle očekávané.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-451">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="5ac0e-452">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-452">**New behavior**</span></span>

<span data-ttu-id="5ac0e-453">Počínaje 3,0 se EF Core nepokouší použít vlastnosti pro cizí klíče podle konvence, pokud mají stejný název jako vlastnost Principal.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-453">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="5ac0e-454">Název objektu zabezpečení zřetězený s názvem vlastnosti objektu zabezpečení a název navigace zřetězený s vzory názvů hlavních vlastností se pořád shodují.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-454">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="5ac0e-455">Příklad:</span><span class="sxs-lookup"><span data-stu-id="5ac0e-455">For example:</span></span>

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

<span data-ttu-id="5ac0e-456">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-456">**Why**</span></span>

<span data-ttu-id="5ac0e-457">Tato změna byla provedena, aby nedocházelo k chybnému definování vlastností primárního klíče u vlastněných typů.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-457">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="5ac0e-458">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-458">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-459">Pokud by vlastnost měla být cizí klíč, a proto je součástí primárního klíče, pak ji explicitně nakonfigurujte jako takovou.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-459">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="5ac0e-460">Připojení k databázi je teď zavřené, pokud už nepoužíváte dřív, než se dokončí jeho objekt TransactionScope.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-460">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="5ac0e-461">Sledování problému #14218</span><span class="sxs-lookup"><span data-stu-id="5ac0e-461">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="5ac0e-462">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-462">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="5ac0e-463">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-463">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-464">Pokud kontext otevře během EF Core 3,0 připojení uvnitř `TransactionScope`, zůstane připojení otevřené, zatímco aktuální `TransactionScope` aktivní je.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-464">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="5ac0e-465">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-465">**New behavior**</span></span>

<span data-ttu-id="5ac0e-466">Od 3,0 EF Core ukončí připojení, jakmile ho dokončí jeho používání.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-466">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="5ac0e-467">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-467">**Why**</span></span>

<span data-ttu-id="5ac0e-468">Tato změna umožňuje použít více kontextů současně `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-468">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="5ac0e-469">Nové chování se také shoduje s EF6.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-469">The new behavior also matches EF6.</span></span>

<span data-ttu-id="5ac0e-470">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-470">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-471">Pokud připojení potřebuje zůstat otevřeným explicitním voláním `OpenConnection()` , zajistí, že EF Core neuzavře předčasně:</span><span class="sxs-lookup"><span data-stu-id="5ac0e-471">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="5ac0e-472">Každá vlastnost používá nezávislou generaci celočíselného klíče v paměti.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-472">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="5ac0e-473">Sledování problému #6872</span><span class="sxs-lookup"><span data-stu-id="5ac0e-473">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="5ac0e-474">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-474">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="5ac0e-475">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-475">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-476">Před EF Core 3,0 byl pro všechny vlastnosti celého čísla klíče v paměti použit jeden generátor sdílených hodnot.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-476">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="5ac0e-477">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-477">**New behavior**</span></span>

<span data-ttu-id="5ac0e-478">Počínaje EF Core 3,0 každé klíčové vlastnosti celého čísla získá vlastní generátor hodnot při použití databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-478">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="5ac0e-479">Také pokud je databáze odstraněna, generování klíče se obnoví pro všechny tabulky.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-479">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="5ac0e-480">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-480">**Why**</span></span>

<span data-ttu-id="5ac0e-481">Tato změna byla provedená tak, aby se při vytváření klíčů v paměti lépe rovnala generování klíčů v paměti a vylepšila se možnost izolovat testy od sebe při použití databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-481">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="5ac0e-482">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-482">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-483">To může poškodit aplikaci, která se spoléhá na konkrétní hodnoty klíče v paměti, které se mají nastavit.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-483">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="5ac0e-484">Místo toho se nemusíte spoléhat na konkrétní hodnoty klíčů nebo aktualizovat tak, aby odpovídaly novému chování.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-484">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="5ac0e-485">Ve výchozím nastavení se používají pole pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-485">Backing fields are used by default</span></span>

[<span data-ttu-id="5ac0e-486">Sledování problému #12430</span><span class="sxs-lookup"><span data-stu-id="5ac0e-486">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="5ac0e-487">Tato změna je představena ve verzi EF Core 3,0-Preview 2.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-487">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="5ac0e-488">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-488">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-489">Před 3,0, a to i v případě, že je známé pole zálohování pro vlastnost, EF Core ve výchozím nastavení přečetl a zapsat hodnotu vlastnosti pomocí metod getter a setter vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-489">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="5ac0e-490">Výjimkou je provedení dotazu, kde by se pole zálohování nastavilo přímo, pokud je známé.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-490">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="5ac0e-491">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-491">**New behavior**</span></span>

<span data-ttu-id="5ac0e-492">Počínaje EF Core 3,0 platí, že pokud je známé pole zálohování pro vlastnost, pak EF Core tuto vlastnost vždy přečte a zapíše pomocí pole pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-492">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="5ac0e-493">To může způsobit přerušení aplikace, pokud se aplikace spoléhá na další chování kódované v metodách getter nebo setter.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-493">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="5ac0e-494">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-494">**Why**</span></span>

<span data-ttu-id="5ac0e-495">Tato změna byla provedena EF Core proto, aby při provádění databázových operací, které obsahují entity, ve výchozím nastavení nechybně aktivovala obchodní logiku.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-495">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="5ac0e-496">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-496">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-497">Chování před 3,0 se dá obnovit pomocí konfigurace režimu přístupu vlastnosti zapnuto `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-497">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="5ac0e-498">Příklad:</span><span class="sxs-lookup"><span data-stu-id="5ac0e-498">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="5ac0e-499">Vyvolat, zda je nalezeno více kompatibilních zálohovaných polí</span><span class="sxs-lookup"><span data-stu-id="5ac0e-499">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="5ac0e-500">Sledování problému #12523</span><span class="sxs-lookup"><span data-stu-id="5ac0e-500">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="5ac0e-501">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-501">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="5ac0e-502">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-502">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-503">Pokud se v EF Core 3,0 shodovalo více polí s pravidly pro hledání zálohovacího pole vlastnosti, bude jedno pole zvoleno na základě pořadí priorit.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-503">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="5ac0e-504">To může způsobit, že se nesprávné pole použije v nejednoznačných případech.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-504">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="5ac0e-505">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-505">**New behavior**</span></span>

<span data-ttu-id="5ac0e-506">Počínaje EF Core 3,0 platí, že pokud je více polí spárováno se stejnou vlastností, je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-506">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="5ac0e-507">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-507">**Why**</span></span>

<span data-ttu-id="5ac0e-508">Tato změna byla provedena, aby nedocházelo k tichému použití jednoho pole v případě, že může být pouze jedna z nich správná.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-508">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="5ac0e-509">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-509">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-510">Vlastnosti s nejednoznačnými zálohovacími poli musí obsahovat pole, které se má explicitně použít.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-510">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="5ac0e-511">Například pomocí rozhraní Fluent API:</span><span class="sxs-lookup"><span data-stu-id="5ac0e-511">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="5ac0e-512">Názvy vlastností pouze pro pole se musí shodovat s názvem pole.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-512">Field-only property names should match the field name</span></span>

<span data-ttu-id="5ac0e-513">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-513">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="5ac0e-514">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-514">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-515">Před EF Core 3,0 může být vlastnost určena hodnotou řetězce a v případě, že v typu .NET nebyla nalezena žádná vlastnost s tímto názvem, EF Core by se pokusila ji porovnat s polem pomocí pravidel konvence.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-515">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the .NET type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="5ac0e-516">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-516">**New behavior**</span></span>

<span data-ttu-id="5ac0e-517">Počínaje EF Core 3,0 se vlastnost pouze pole musí přesně shodovat s názvem pole.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-517">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="5ac0e-518">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-518">**Why**</span></span>

<span data-ttu-id="5ac0e-519">Tato změna byla provedena, aby se nepoužívalo stejné pole pro dvě vlastnosti s názvem podobně, ale také pravidla pro porovnání vlastností pouze pro pole, která jsou shodná s vlastnostmi mapovanými na vlastnosti CLR.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-519">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="5ac0e-520">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-520">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-521">Vlastnosti pouze polí musí být pojmenovány stejně jako pole, na které jsou namapována.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-521">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="5ac0e-522">V budoucí verzi EF Core po 3,0 plánujeme znovu povolit explicitní konfiguraci názvu pole, který se liší od názvu vlastnosti (viz téma věnované problému [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span><span class="sxs-lookup"><span data-stu-id="5ac0e-522">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="5ac0e-523">AddDbContext/AddDbContextPool už nevolá AddLogging a AddMemoryCache.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-523">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="5ac0e-524">Sledování problému #14756</span><span class="sxs-lookup"><span data-stu-id="5ac0e-524">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="5ac0e-525">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-525">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="5ac0e-526">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-526">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-527">Před EF Core 3,0, volání `AddDbContext` nebo `AddDbContextPool` by také registrovaly služby protokolování a ukládání do mezipaměti v paměti pomocí D. I přes volání [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) a [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="5ac0e-527">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="5ac0e-528">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-528">**New behavior**</span></span>

<span data-ttu-id="5ac0e-529">Počínaje EF Core 3,0 `AddDbContext` a `AddDbContextPool` nadále se nebudou registrovat tyto služby se vkládáním závislostí (di).</span><span class="sxs-lookup"><span data-stu-id="5ac0e-529">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="5ac0e-530">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-530">**Why**</span></span>

<span data-ttu-id="5ac0e-531">EF Core 3,0 nevyžaduje, aby se tyto služby nacházejí v kontejneru aplikace DI.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-531">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="5ac0e-532">Pokud `ILoggerFactory` je však v kontejneru aplikace zaregistrován, bude nadále používána EF Core.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-532">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="5ac0e-533">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-533">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-534">Pokud vaše aplikace potřebuje tyto služby, zaregistrujte je explicitně pomocí kontejneru DI pomocí [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) nebo [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="5ac0e-534">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="5ac0e-535">DbContext. entry teď provádí místní DetectChanges.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-535">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="5ac0e-536">Sledování problému #13552</span><span class="sxs-lookup"><span data-stu-id="5ac0e-536">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="5ac0e-537">Tato změna je představena ve verzi EF Core 3,0-Preview 3.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-537">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="5ac0e-538">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-538">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-539">Před EF Core 3,0 by volání `DbContext.Entry` způsobilo zjištění změn všech sledovaných entit.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-539">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="5ac0e-540">Tím je zajištěno, že stav zpřístupněno v `EntityEntry` nástroji byl aktuální.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-540">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="5ac0e-541">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-541">**New behavior**</span></span>

<span data-ttu-id="5ac0e-542">Počínaje EF Core 3,0 se nyní volání `DbContext.Entry` pokusí zjistit změny v dané entitě a všechny sledované hlavní entity, které se k ní vztahují.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-542">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="5ac0e-543">To znamená, že změny jinde nemohly být zjištěny voláním této metody, což by mohlo mít vliv na stav aplikace.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-543">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="5ac0e-544">Všimněte si, `ChangeTracker.AutoDetectChangesEnabled` že pokud je `false` nastaveno na, tak i toto místní zjišťování změn bude zakázáno.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-544">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="5ac0e-545">Jiné metody, které způsobují detekci změn – například `ChangeTracker.Entries` a `SaveChanges`---stále zapříčiní plné `DetectChanges` ze všech sledovaných entit.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-545">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="5ac0e-546">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-546">**Why**</span></span>

<span data-ttu-id="5ac0e-547">Tato změna byla provedena za účelem zlepšení výchozího výkonu použití `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-547">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="5ac0e-548">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-548">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-549">Před `ChgangeTracker.DetectChanges()` voláním `Entry` zajistěte explicitní volání, aby bylo zajištěno chování před 3,0.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-549">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="5ac0e-550">Klíče řetězce a pole bajtů nejsou ve výchozím nastavení generovány klientem.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-550">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="5ac0e-551">Sledování problému #14617</span><span class="sxs-lookup"><span data-stu-id="5ac0e-551">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="5ac0e-552">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-552">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="5ac0e-553">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-553">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-554">Před EF Core 3,0 `string` a `byte[]` vlastnosti klíče lze použít bez explicitního nastavení hodnoty, která není null.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-554">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="5ac0e-555">V takovém případě se hodnota klíče vygeneruje na klientovi jako identifikátor GUID, který je serializovaný na bajty pro `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-555">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="5ac0e-556">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-556">**New behavior**</span></span>

<span data-ttu-id="5ac0e-557">Počínaje EF Core 3,0 bude vyvolána výjimka oznamující, že nebyla nastavena žádná hodnota klíče.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-557">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="5ac0e-558">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-558">**Why**</span></span>

<span data-ttu-id="5ac0e-559">Tato změna byla provedena, protože obecně nejsou `string` užitečné hodnoty generované / `byte[]` klientem a výchozí chování způsobilo, že je obtížné vygenerovat hodnoty klíčů běžným způsobem.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-559">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="5ac0e-560">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-560">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-561">Chování před 3,0 lze získat explicitním určením, že klíčové vlastnosti by měly používat generované hodnoty, pokud není nastavena žádná jiná hodnota, která není null.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-561">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="5ac0e-562">Například s rozhraním API Fluent:</span><span class="sxs-lookup"><span data-stu-id="5ac0e-562">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="5ac0e-563">Nebo s poznámkami k datům:</span><span class="sxs-lookup"><span data-stu-id="5ac0e-563">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="5ac0e-564">ILoggerFactory je teď služba s vymezeným oborem.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-564">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="5ac0e-565">Sledování problému #14698</span><span class="sxs-lookup"><span data-stu-id="5ac0e-565">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="5ac0e-566">Tato změna je představena ve verzi EF Core 3,0-Preview 3.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-566">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="5ac0e-567">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-567">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-568">Před EF Core 3,0 `ILoggerFactory` byl zaregistrován jako služba typu singleton.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-568">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="5ac0e-569">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-569">**New behavior**</span></span>

<span data-ttu-id="5ac0e-570">Počínaje EF Core 3,0 `ILoggerFactory` je nyní registrováno jako obor.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-570">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="5ac0e-571">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-571">**Why**</span></span>

<span data-ttu-id="5ac0e-572">Tato změna byla provedena tak, aby povolovala přidružení protokolovacího `DbContext` nástroje s instancí, která umožňuje další funkce a odebírá některé případy patologického chování, jako je například rozpad interních poskytovatelů služeb.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-572">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="5ac0e-573">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-573">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-574">Tato změna by neměla mít vliv na kód aplikace, pokud se neregistruje a nepoužívá vlastní služby pro EF Core interního poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-574">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="5ac0e-575">To není běžné.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-575">This isn't common.</span></span>
<span data-ttu-id="5ac0e-576">V těchto případech bude většina věcí pořád fungovat, ale jakákoli služba typu Singleton, která byla v závislosti `ILoggerFactory` na tom, bude muset změnit tak, `ILoggerFactory` aby získala jiný způsob.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-576">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="5ac0e-577">Pokud narazíte na takové situace, zajistěte prosím problém na [EF Core modul pro sledování problémů GitHubu](https://github.com/aspnet/EntityFrameworkCore/issues) , abychom nás věděli, jak `ILoggerFactory` ho používáte, abychom mohli lépe pochopit, jak to v budoucnu Nerušit.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-577">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="5ac0e-578">Opožděné načítání proxy serverů už nepředpokládá navigační vlastnosti, jsou plně načtené.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-578">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="5ac0e-579">Sledování problému #12780</span><span class="sxs-lookup"><span data-stu-id="5ac0e-579">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="5ac0e-580">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-580">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="5ac0e-581">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-581">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-582">Předtím, než EF Core 3,0, `DbContext` neexistuje žádný způsob, jak zjistit, zda byla daná vlastnost navigace u entity získané z tohoto kontextu plně načtena nebo ne.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-582">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="5ac0e-583">Proxy místo toho předpokládají, že je načtena odkazová navigace, pokud má hodnotu jinou než null a že je načtena navigace kolekce, pokud není prázdná.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-583">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="5ac0e-584">V těchto případech by byl pokus o opožděné načtení no-op.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-584">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="5ac0e-585">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-585">**New behavior**</span></span>

<span data-ttu-id="5ac0e-586">Počínaje EF Core 3,0 budou proxy servery sledovat, zda je načtena vlastnost navigace.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-586">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="5ac0e-587">To znamená, že při pokusu o přístup k navigační vlastnosti, která je načtena po vyřazení kontextu, bude vždy ta no-op, i když je načtená navigace prázdná nebo má hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-587">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="5ac0e-588">Naopak při pokusu o přístup k navigační vlastnosti, která není načtená, vyvolá výjimku, pokud je kontext vyřazený, i když je vlastnost navigace neprázdná kolekce.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-588">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="5ac0e-589">Pokud nastane tato situace, znamená to, že se kód aplikace pokouší použít opožděné načítání v neplatném čase a aplikace by se měla změnit, aby to nevedlo.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-589">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="5ac0e-590">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-590">**Why**</span></span>

<span data-ttu-id="5ac0e-591">Tato změna byla provedena, aby při pokusu o opožděné načtení na uvolněnou `DbContext` instanci bylo chování konzistentní a správné.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-591">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="5ac0e-592">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-592">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-593">Aktualizujte kód aplikace, aby se nepokoušel opožděné načtení s odstraněným kontextem, nebo nastavte tuto hodnotu jako No-op, jak je popsáno ve zprávě výjimky.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-593">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="5ac0e-594">Nadměrné vytváření interních zprostředkovatelů služeb je teď ve výchozím nastavení chyba.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-594">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="5ac0e-595">Sledování problému #10236</span><span class="sxs-lookup"><span data-stu-id="5ac0e-595">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="5ac0e-596">Tato změna je představena ve verzi EF Core 3,0-Preview 3.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-596">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="5ac0e-597">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-597">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-598">Před EF Core 3,0 se pro aplikaci, která vytváří patologický počet interních poskytovatelů služeb, zaprotokoluje upozornění.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-598">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="5ac0e-599">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-599">**New behavior**</span></span>

<span data-ttu-id="5ac0e-600">Počínaje EF Core 3,0 je toto upozornění nyní považováno za chybu a je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-600">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="5ac0e-601">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-601">**Why**</span></span>

<span data-ttu-id="5ac0e-602">Tato změna byla provedená tak, že se tento patologický případ výslovně zveřejňuje tak, aby se zlepšil kód aplikace.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-602">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="5ac0e-603">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-603">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-604">Nejvhodnější příčinou této chyby je pochopení hlavní příčiny a zastavení vytváření, takže mnoho interních poskytovatelů služeb.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-604">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="5ac0e-605">Chybu však lze převést zpět na varování (nebo ignorováno) prostřednictvím konfigurace na `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-605">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="5ac0e-606">Příklad:</span><span class="sxs-lookup"><span data-stu-id="5ac0e-606">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="5ac0e-607">Nové chování pro HasOne/HasMany se volá s jedním řetězcem.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-607">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="5ac0e-608">Sledování problému #9171</span><span class="sxs-lookup"><span data-stu-id="5ac0e-608">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="5ac0e-609">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-609">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="5ac0e-610">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-610">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-611">Před EF Core 3,0, volání `HasOne` kódu nebo `HasMany` s jedním řetězcem bylo interpretováno matoucím způsobem.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-611">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="5ac0e-612">Příklad:</span><span class="sxs-lookup"><span data-stu-id="5ac0e-612">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="5ac0e-613">Kód vypadá jako v `Samurai` `Entrance` souvislosti s jiným typem entity pomocí navigační vlastnosti, která může být soukromá.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-613">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="5ac0e-614">Ve skutečnosti se tento kód pokusí vytvořit relaci k některému typu entity s názvem `Entrance` bez navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-614">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="5ac0e-615">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-615">**New behavior**</span></span>

<span data-ttu-id="5ac0e-616">Počínaje EF Core 3,0 výše uvedený kód má nyní podobný vzhled jako předtím.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-616">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="5ac0e-617">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-617">**Why**</span></span>

<span data-ttu-id="5ac0e-618">Staré chování bylo velmi matoucí, zejména při čtení konfiguračního kódu a hledání chyb.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-618">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="5ac0e-619">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-619">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-620">Tím dojde pouze k přerušení aplikací, které jsou explicitně konfigurovány pomocí řetězců pro názvy typů, a bez explicitního určení vlastnosti navigace.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-620">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="5ac0e-621">To není běžné.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-621">This is not common.</span></span>
<span data-ttu-id="5ac0e-622">Předchozí chování lze získat pomocí explicitního předání `null` názvu vlastnosti navigace.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-622">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="5ac0e-623">Příklad:</span><span class="sxs-lookup"><span data-stu-id="5ac0e-623">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="5ac0e-624">Návratový typ pro několik asynchronních metod byl změněn z úlohy na ValueTask</span><span class="sxs-lookup"><span data-stu-id="5ac0e-624">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="5ac0e-625">Sledování problému #15184</span><span class="sxs-lookup"><span data-stu-id="5ac0e-625">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="5ac0e-626">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-626">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="5ac0e-627">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-627">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-628">Následující asynchronní metody dříve vrátily `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="5ac0e-628">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="5ac0e-629">`ValueGenerator.NextValueAsync()`(a odvozování tříd)</span><span class="sxs-lookup"><span data-stu-id="5ac0e-629">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="5ac0e-630">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-630">**New behavior**</span></span>

<span data-ttu-id="5ac0e-631">Výše uvedené metody nyní vrací stejnou `ValueTask<T>` `T` hodnotu jako předtím.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-631">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="5ac0e-632">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-632">**Why**</span></span>

<span data-ttu-id="5ac0e-633">Tato změna snižuje počet přidělení haldy, které vznikly při vyvolání těchto metod, což zlepšuje obecný výkon.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-633">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="5ac0e-634">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-634">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-635">Aplikace jednoduše čekají na rozhraní API, které je třeba znovu zkompilovat – nejsou nutné žádné změny zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-635">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="5ac0e-636">Složitější využití (například předání vráceného `Task` do `Task.WhenAny()`) obvykle vyžaduje, aby bylo vráceno `ValueTask<T>` převedené na a `Task<T>` voláním `AsTask()` .</span><span class="sxs-lookup"><span data-stu-id="5ac0e-636">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="5ac0e-637">Všimněte si, že se tím sníží omezení přidělení, které tato změna přináší.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-637">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="5ac0e-638">Relační: anotace TypeMapping je nyní pouze TypeMapping</span><span class="sxs-lookup"><span data-stu-id="5ac0e-638">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="5ac0e-639">Sledování problému #9913</span><span class="sxs-lookup"><span data-stu-id="5ac0e-639">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="5ac0e-640">Tato změna je představena ve verzi EF Core 3,0-Preview 2.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-640">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="5ac0e-641">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-641">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-642">Název poznámky pro poznámky k mapování typů byl "relační: TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="5ac0e-642">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="5ac0e-643">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-643">**New behavior**</span></span>

<span data-ttu-id="5ac0e-644">Název poznámky pro mapování typů je nyní "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="5ac0e-644">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="5ac0e-645">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-645">**Why**</span></span>

<span data-ttu-id="5ac0e-646">Mapování typů se nyní používají pro více než stejné poskytovatele relačních databází.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-646">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="5ac0e-647">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-647">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-648">Tím dojde pouze k přerušení aplikací, které přistupují k mapování typu přímo jako anotaci, což není běžné.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-648">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="5ac0e-649">Nejvhodnější akcí pro opravu je použití prostoru rozhraní API pro přístup k mapování typů namísto použití anotace přímo.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-649">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="5ac0e-650">ToTable na odvozeném typu vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-650">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="5ac0e-651">Sledování problému #11811</span><span class="sxs-lookup"><span data-stu-id="5ac0e-651">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="5ac0e-652">Tato změna je představena ve verzi EF Core 3,0-Preview 3.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-652">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="5ac0e-653">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-653">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-654">Před EF Core 3,0, `ToTable()` který je volán na odvozeném typu, by byl ignorován, protože pouze dědění mapování dědičnosti bylo typu TPH, který není platný.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-654">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="5ac0e-655">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-655">**New behavior**</span></span>

<span data-ttu-id="5ac0e-656">Počínaje EF Core 3,0 a přípravou pro přidání podpory TPT a TPC v novější verzi, která se `ToTable()` volá na odvozeném typu, teď vyvolá výjimku, aby se předešlo neočekávané změně mapování v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-656">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="5ac0e-657">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-657">**Why**</span></span>

<span data-ttu-id="5ac0e-658">V současné době není platný pro mapování odvozeného typu na jinou tabulku.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-658">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="5ac0e-659">Tato změna zabrání v budoucnosti v budoucnu, pokud se to stalo platným.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-659">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="5ac0e-660">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-660">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-661">Odeberte všechny pokusy o mapování odvozených typů na jiné tabulky.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-661">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="5ac0e-662">ForSqlServerHasIndex nahrazeno HasIndex</span><span class="sxs-lookup"><span data-stu-id="5ac0e-662">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="5ac0e-663">Sledování problému #12366</span><span class="sxs-lookup"><span data-stu-id="5ac0e-663">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="5ac0e-664">Tato změna je představena ve verzi EF Core 3,0-Preview 3.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-664">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="5ac0e-665">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-665">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-666">Před EF Core 3,0 jste `ForSqlServerHasIndex().ForSqlServerInclude()` získali způsob, jak nakonfigurovat sloupce používané pomocí `INCLUDE`nástroje.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-666">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="5ac0e-667">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-667">**New behavior**</span></span>

<span data-ttu-id="5ac0e-668">Počínaje EF Core 3,0 se teď použití `Include` na indexu podporuje na relační úrovni.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-668">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="5ac0e-669">Použijte `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-669">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="5ac0e-670">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-670">**Why**</span></span>

<span data-ttu-id="5ac0e-671">Tato změna byla provedena za účelem konsolidace rozhraní API pro indexy `Include` na jednom místě pro všechny poskytovatele databáze.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-671">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="5ac0e-672">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-672">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-673">Použijte nové rozhraní API, jak vidíte výše.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-673">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="5ac0e-674">Změny rozhraní API pro metadata</span><span class="sxs-lookup"><span data-stu-id="5ac0e-674">Metadata API changes</span></span>

[<span data-ttu-id="5ac0e-675">Sledování problému #214</span><span class="sxs-lookup"><span data-stu-id="5ac0e-675">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="5ac0e-676">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-676">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="5ac0e-677">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-677">**New behavior**</span></span>

<span data-ttu-id="5ac0e-678">Následující vlastnosti byly převedeny na rozšiřující metody:</span><span class="sxs-lookup"><span data-stu-id="5ac0e-678">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="5ac0e-679">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-679">**Why**</span></span>

<span data-ttu-id="5ac0e-680">Tato změna zjednodušuje implementaci výše uvedených rozhraní.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-680">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="5ac0e-681">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-681">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-682">Použijte nové metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-682">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="5ac0e-683">Změny rozhraní API pro konkrétního zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="5ac0e-683">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="5ac0e-684">Sledování problému #214</span><span class="sxs-lookup"><span data-stu-id="5ac0e-684">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="5ac0e-685">Tato změna je představena ve verzi EF Core 3,0-Preview 6.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-685">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="5ac0e-686">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-686">**New behavior**</span></span>

<span data-ttu-id="5ac0e-687">Metody rozšíření specifické pro poskytovatele budou shrnuty:</span><span class="sxs-lookup"><span data-stu-id="5ac0e-687">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="5ac0e-688">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-688">**Why**</span></span>

<span data-ttu-id="5ac0e-689">Tato změna zjednodušuje implementaci výše uvedených rozšiřujících metod.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-689">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="5ac0e-690">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-690">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-691">Použijte nové metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-691">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="5ac0e-692">EF Core už neposílá direktivu pragma pro vynucení KOFK SQLite.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-692">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="5ac0e-693">Sledování problému #12151</span><span class="sxs-lookup"><span data-stu-id="5ac0e-693">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="5ac0e-694">Tato změna je představena ve verzi EF Core 3,0-Preview 3.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-694">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="5ac0e-695">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-695">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-696">Před EF Core 3,0 EF Core odeslat `PRAGMA foreign_keys = 1` při otevření připojení k sqlite.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-696">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="5ac0e-697">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-697">**New behavior**</span></span>

<span data-ttu-id="5ac0e-698">Počínaje EF Core 3,0 EF Core již neposílá `PRAGMA foreign_keys = 1` při otevření připojení k sqlite.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-698">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="5ac0e-699">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-699">**Why**</span></span>

<span data-ttu-id="5ac0e-700">Tato změna byla provedena, protože EF Core `SQLitePCLRaw.bundle_e_sqlite3` používá ve výchozím nastavení, což zase znamená, že je ve výchozím nastavení zapnuté vynucení CK a není nutné je explicitně povolit při každém otevření připojení.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-700">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="5ac0e-701">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-701">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-702">Cizí klíče jsou ve výchozím nastavení povolené v SQLitePCLRaw. bundle_e_sqlite3, který se ve výchozím nastavení používá pro EF Core.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-702">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="5ac0e-703">V ostatních případech je možné povolit cizí klíče zadáním `Foreign Keys=True` v připojovacím řetězci.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-703">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="5ac0e-704">Microsoft. EntityFrameworkCore. sqlite teď závisí na SQLitePCLRaw. bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="5ac0e-704">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="5ac0e-705">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-705">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-706">Před EF Core 3,0 se používá `SQLitePCLRaw.bundle_green`EF Core.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-706">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="5ac0e-707">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-707">**New behavior**</span></span>

<span data-ttu-id="5ac0e-708">Počínaje EF Core 3,0 EF Core používá `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-708">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="5ac0e-709">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-709">**Why**</span></span>

<span data-ttu-id="5ac0e-710">Tato změna byla provedena tak, že verze SQLiteu použitá v iOS je konzistentní s jinými platformami.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-710">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="5ac0e-711">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-711">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-712">Pokud chcete použít nativní verzi SQLite v iOS, nakonfigurujte `Microsoft.Data.Sqlite` , aby používala `SQLitePCLRaw` jiný svazek.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-712">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="5ac0e-713">Hodnoty GUID se teď ukládají jako TEXT na SQLite.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-713">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="5ac0e-714">Sledování problému #15078</span><span class="sxs-lookup"><span data-stu-id="5ac0e-714">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="5ac0e-715">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-715">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="5ac0e-716">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-716">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-717">Hodnoty GUID byly dříve uloženy jako hodnoty objektů BLOB u SQLite.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-717">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="5ac0e-718">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-718">**New behavior**</span></span>

<span data-ttu-id="5ac0e-719">Hodnoty GUID jsou nyní uloženy jako TEXT.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-719">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="5ac0e-720">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-720">**Why**</span></span>

<span data-ttu-id="5ac0e-721">Binární formát identifikátorů GUID není standardizovaný.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-721">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="5ac0e-722">Uložení hodnot jako textu zajistí, že databáze bude lépe kompatibilní s jinými technologiemi.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-722">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="5ac0e-723">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-723">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-724">Existující databáze můžete migrovat do nového formátu tím, že spustíte příkaz SQL podobně jako následující.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-724">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="5ac0e-725">V EF Core můžete i nadále používat předchozí chování nakonfigurováním převaděče hodnot na těchto vlastnostech.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-725">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="5ac0e-726">Microsoft. data. sqlite zůstává schopný přečítat hodnoty GUID z objektů BLOB a textových sloupců. vzhledem k tomu, že výchozí formát pro parametry a konstanty se změnil, bude pravděpodobně nutné provést akci u většiny scénářů, které zahrnují identifikátory GUID.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-726">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="5ac0e-727">Hodnoty char se teď ukládají jako TEXT na SQLite.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-727">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="5ac0e-728">Sledování problému #15020</span><span class="sxs-lookup"><span data-stu-id="5ac0e-728">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="5ac0e-729">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-729">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="5ac0e-730">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-730">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-731">Hodnoty typu char byly dříve sored jako CELOČÍSELNé hodnoty u SQLite.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-731">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="5ac0e-732">Například hodnota znaku *a* byla uložena jako celočíselná hodnota 65.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-732">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="5ac0e-733">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-733">**New behavior**</span></span>

<span data-ttu-id="5ac0e-734">Hodnoty typu char jsou nyní uloženy jako TEXT.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-734">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="5ac0e-735">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-735">**Why**</span></span>

<span data-ttu-id="5ac0e-736">Ukládání hodnot jako TEXT je přirozenější a databáze usnadňuje kompatibilitu s jinými technologiemi.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-736">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="5ac0e-737">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-737">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-738">Existující databáze můžete migrovat do nového formátu tím, že spustíte příkaz SQL podobně jako následující.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-738">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="5ac0e-739">V EF Core můžete i nadále používat předchozí chování nakonfigurováním převaděče hodnot na těchto vlastnostech.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-739">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="5ac0e-740">Microsoft. data. sqlite také zůstává schopný číst znakové hodnoty z CELOČÍSELNého i TEXTOVÉHO sloupce, takže některé scénáře nemusí vyžadovat žádnou akci.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-740">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="5ac0e-741">ID migrace se teď generují pomocí kalendáře invariantní jazykové verze.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-741">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="5ac0e-742">Sledování problému #12978</span><span class="sxs-lookup"><span data-stu-id="5ac0e-742">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="5ac0e-743">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-743">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="5ac0e-744">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-744">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-745">ID migrace se nechtěně vygenerovala pomocí kalendáře aktuální jazykové verze.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-745">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="5ac0e-746">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-746">**New behavior**</span></span>

<span data-ttu-id="5ac0e-747">ID migrace se nyní vždy generují pomocí kalendáře neutrální jazykové verze (gregoriánský).</span><span class="sxs-lookup"><span data-stu-id="5ac0e-747">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="5ac0e-748">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-748">**Why**</span></span>

<span data-ttu-id="5ac0e-749">Pořadí migrace je důležité při aktualizaci databáze nebo řešení konfliktů při slučování.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-749">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="5ac0e-750">Pomocí invariantního kalendáře se vyhnete problémům s řazením, které mohou být výsledkem členů týmu jiné systémové kalendáře.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-750">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="5ac0e-751">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-751">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-752">Tato změna má vliv na kohokoli, kdo používá negregoriánský kalendář, ve kterém je rok větší než gregoriánský kalendář (například thajský buddhistický kalendář).</span><span class="sxs-lookup"><span data-stu-id="5ac0e-752">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="5ac0e-753">Existující identifikátory migrace se budou muset aktualizovat, aby se nové migrace objednaly po stávajících migracích.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-753">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="5ac0e-754">ID migrace najdete v atributu migrace v souborech návrháře migrace.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-754">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="5ac0e-755">Také je potřeba aktualizovat tabulku historie migrace.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-755">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="5ac0e-756">UseRowNumberForPaging se odebral.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-756">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="5ac0e-757">Sledování problému #16400</span><span class="sxs-lookup"><span data-stu-id="5ac0e-757">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="5ac0e-758">Tato změna je představena ve verzi EF Core 3,0-Preview 6.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-758">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="5ac0e-759">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-759">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-760">Před EF Core 3,0 `UseRowNumberForPaging` lze použít k vygenerování SQL pro stránkování, které je kompatibilní s SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-760">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="5ac0e-761">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-761">**New behavior**</span></span>

<span data-ttu-id="5ac0e-762">Počínaje EF Core 3,0 bude EF generovat pouze SQL pro stránkování, které je kompatibilní pouze s novějšími verzemi SQL Server.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-762">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="5ac0e-763">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-763">**Why**</span></span>

<span data-ttu-id="5ac0e-764">Tuto změnu provedeme, protože [SQL Server 2008 už není podporovaným produktem](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) a aktualizace této funkce tak, aby fungovala se změnami dotazů provedenými v EF Core 3,0 je významná práce.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-764">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="5ac0e-765">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-765">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-766">Doporučujeme aktualizovat na novější verzi SQL Server nebo pomocí vyšší úrovně kompatibility, aby byl vygenerovaný SQL podporován.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-766">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="5ac0e-767">To znamená, že pokud to nemůžete udělat, [komentář k problému s sledováním](https://github.com/aspnet/EntityFrameworkCore/issues/16400) najdete v podrobnostech.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-767">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="5ac0e-768">Toto rozhodnutí můžeme znovu navštívit na základě zpětné vazby.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-768">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="5ac0e-769">Informace o rozšíření/metadata se odebraly z IDbContextOptionsExtension.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-769">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="5ac0e-770">Sledování problému #16119</span><span class="sxs-lookup"><span data-stu-id="5ac0e-770">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="5ac0e-771">Tato změna je představena ve verzi EF Core 3,0-Preview 7.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-771">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="5ac0e-772">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-772">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-773">`IDbContextOptionsExtension`obsažené metody pro poskytování metadat o rozšíření.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-773">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="5ac0e-774">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-774">**New behavior**</span></span>

<span data-ttu-id="5ac0e-775">Tyto metody byly přesunuty na novou `DbContextOptionsExtensionInfo` abstraktní základní třídu, která je vrácena z nové `IDbContextOptionsExtension.Info` vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-775">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="5ac0e-776">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-776">**Why**</span></span>

<span data-ttu-id="5ac0e-777">V rámci vydání od 2,0 do 3,0 jsme potřebovali přidat nebo změnit tyto metody několikrát.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-777">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="5ac0e-778">Rozbalením do nové abstraktní základní třídy bude snazší vytvořit tyto změny bez přerušení stávajících rozšíření.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-778">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="5ac0e-779">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-779">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-780">Aktualizovat rozšíření tak, aby následovala nový vzor.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-780">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="5ac0e-781">Příklady naleznete v mnoha implementacích `IDbContextOptionsExtension` pro různé druhy rozšíření ve zdrojovém kódu EF Core.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-781">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="5ac0e-782">LogQueryPossibleExceptionWithAggregateOperator se přejmenovalo.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-782">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="5ac0e-783">Sledování problému #10985</span><span class="sxs-lookup"><span data-stu-id="5ac0e-783">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="5ac0e-784">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-784">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="5ac0e-785">**Mění**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-785">**Change**</span></span>

<span data-ttu-id="5ac0e-786">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator`byla přejmenována `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`na.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-786">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="5ac0e-787">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-787">**Why**</span></span>

<span data-ttu-id="5ac0e-788">Zarovná pojmenování této události varování se všemi ostatními událostmi upozornění.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-788">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="5ac0e-789">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-789">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-790">Použijte nový název.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-790">Use the new name.</span></span> <span data-ttu-id="5ac0e-791">(Všimněte si, že číslo ID události se nezměnilo.)</span><span class="sxs-lookup"><span data-stu-id="5ac0e-791">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="5ac0e-792">Vysvětlení rozhraní API pro názvy omezení cizího klíče</span><span class="sxs-lookup"><span data-stu-id="5ac0e-792">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="5ac0e-793">Sledování problému #10730</span><span class="sxs-lookup"><span data-stu-id="5ac0e-793">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="5ac0e-794">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-794">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="5ac0e-795">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-795">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-796">Před EF Core 3,0 se názvy omezení cizího klíče odkazovaly jenom na "název".</span><span class="sxs-lookup"><span data-stu-id="5ac0e-796">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="5ac0e-797">Příklad:</span><span class="sxs-lookup"><span data-stu-id="5ac0e-797">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="5ac0e-798">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-798">**New behavior**</span></span>

<span data-ttu-id="5ac0e-799">Počínaje EF Core 3,0 se názvy omezení cizích klíčů teď označují jako "název omezení".</span><span class="sxs-lookup"><span data-stu-id="5ac0e-799">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="5ac0e-800">Příklad:</span><span class="sxs-lookup"><span data-stu-id="5ac0e-800">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="5ac0e-801">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-801">**Why**</span></span>

<span data-ttu-id="5ac0e-802">Tato změna přináší konzistenci pro pojmenování v této oblasti a také vysvětluje, že se jedná o název omezení cizího klíče, a nikoli název sloupce nebo vlastnosti, ve kterém je definován cizí klíč.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-802">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="5ac0e-803">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-803">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-804">Použijte nový název.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-804">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="5ac0e-805">IRelationalDatabaseCreator. HasTables/HasTablesAsync byly zveřejněny.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-805">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="5ac0e-806">Sledování problému #15997</span><span class="sxs-lookup"><span data-stu-id="5ac0e-806">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="5ac0e-807">Tato změna je představena ve verzi EF Core 3,0-Preview 7.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-807">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="5ac0e-808">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-808">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-809">Před EF Core 3,0 byly tyto metody chráněné.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-809">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="5ac0e-810">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-810">**New behavior**</span></span>

<span data-ttu-id="5ac0e-811">Počínaje EF Core 3,0 jsou tyto metody veřejné.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-811">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="5ac0e-812">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-812">**Why**</span></span>

<span data-ttu-id="5ac0e-813">Tyto metody jsou používány EF k určení, jestli je databáze vytvořená, ale prázdná.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-813">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="5ac0e-814">To může být užitečné taky od vnějšího EF při určování, jestli se mají migrace použít.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-814">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="5ac0e-815">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-815">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-816">Změňte přístupnost všech přepsání.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-816">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="5ac0e-817">Microsoft. EntityFrameworkCore. Design je teď balíček DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-817">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="5ac0e-818">Sledování problému #11506</span><span class="sxs-lookup"><span data-stu-id="5ac0e-818">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="5ac0e-819">Tato změna je představena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-819">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="5ac0e-820">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-820">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-821">Před EF Core 3,0 byl Microsoft. EntityFrameworkCore. Design regulárním balíčkem NuGet, na kterém by mohly být na sestavení odkazovány projekty, které na něm závisejí.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-821">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="5ac0e-822">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-822">**New behavior**</span></span>

<span data-ttu-id="5ac0e-823">Počínaje EF Core 3,0 se jedná o balíček DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-823">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="5ac0e-824">To znamená, že závislost nebude nijak přesměrovat do jiných projektů a že již nemůžete ve výchozím nastavení odkazovat na své sestavení.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-824">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="5ac0e-825">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-825">**Why**</span></span>

<span data-ttu-id="5ac0e-826">Tento balíček se má použít jenom v době návrhu.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-826">This package is only intended to be used at design time.</span></span> <span data-ttu-id="5ac0e-827">Nasazené aplikace by neměli na ni odkazovat.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-827">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="5ac0e-828">Díky tomu, že balíček DevelopmentDependency, toto doporučení posiluje.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-828">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="5ac0e-829">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-829">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-830">Pokud potřebujete odkazovat na tento balíček, aby bylo možné přepsat EF Core chování při návrhu, můžete aktualizovat metadata položky aktualizovat PackageReference v projektu.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-830">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="5ac0e-831">Pokud se na balíček odkazuje přes Microsoft. EntityFrameworkCore. Tools, budete muset do balíčku přidat explicitní PackageReference, aby se změnila jeho metadata.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-831">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0-preview4.19216.3">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="5ac0e-832">SQLitePCL. Raw aktualizováno na verzi 2.0.0</span><span class="sxs-lookup"><span data-stu-id="5ac0e-832">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="5ac0e-833">Sledování problému #14824</span><span class="sxs-lookup"><span data-stu-id="5ac0e-833">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="5ac0e-834">Tato změna je představena ve verzi EF Core 3,0-Preview 7.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-834">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="5ac0e-835">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-835">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-836">Microsoft. EntityFrameworkCore. sqlite byl dřív závislý na 1.1.12 verze SQLitePCL. Raw.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-836">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="5ac0e-837">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-837">**New behavior**</span></span>

<span data-ttu-id="5ac0e-838">Náš balíček jsme aktualizovali tak, aby byl závislý na verzi 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-838">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="5ac0e-839">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-839">**Why**</span></span>

<span data-ttu-id="5ac0e-840">2\.0.0 verze SQLitePCL. Raw TARGETS .NET Standard 2,0.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-840">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="5ac0e-841">Dříve cílí na .NET Standard 1,1, které vyžadovaly, aby se při práci vytvořil velký uzávěr přenosných balíčků.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-841">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="5ac0e-842">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-842">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-843">SQLitePCL. Raw 2.0.0 verze obsahuje některé zásadní změny.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-843">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="5ac0e-844">Podrobnosti najdete v [poznámkách k verzi](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) .</span><span class="sxs-lookup"><span data-stu-id="5ac0e-844">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="5ac0e-845">NetTopologySuite aktualizace na verzi 2.0.0</span><span class="sxs-lookup"><span data-stu-id="5ac0e-845">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="5ac0e-846">Sledování problému #14825</span><span class="sxs-lookup"><span data-stu-id="5ac0e-846">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="5ac0e-847">Tato změna je představena ve verzi EF Core 3,0-Preview 7.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-847">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="5ac0e-848">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-848">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-849">Prostorové balíčky byly dříve závislé na 1.15.1 verze NetTopologySuite.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-849">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="5ac0e-850">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-850">**New behavior**</span></span>

<span data-ttu-id="5ac0e-851">Náš balíček jsme aktualizovali tak, aby byl závislý na verzi 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-851">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="5ac0e-852">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-852">**Why**</span></span>

<span data-ttu-id="5ac0e-853">2\.0.0 verze NetTopologySuite má za cíl řešit několik problémů s použitelností, ke kterým se EF Core uživatelé setkali.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-853">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="5ac0e-854">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-854">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-855">NetTopologySuite verze 2.0.0 obsahuje některé průlomové změny.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-855">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="5ac0e-856">Podrobnosti najdete v [poznámkách k verzi](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) .</span><span class="sxs-lookup"><span data-stu-id="5ac0e-856">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="5ac0e-857">Je nutné nakonfigurovat více dvojznačných relací odkazujících na sebe.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-857">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="5ac0e-858">Sledování problému #13573</span><span class="sxs-lookup"><span data-stu-id="5ac0e-858">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="5ac0e-859">Tato změna je představena ve verzi EF Core 3,0-Preview 6.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-859">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="5ac0e-860">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-860">**Old behavior**</span></span>

<span data-ttu-id="5ac0e-861">Typ entity s více jednosměrnou navigační vlastností a s vyhovující FKs byl nesprávně nakonfigurován jako jeden vztah.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-861">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="5ac0e-862">Příklad:</span><span class="sxs-lookup"><span data-stu-id="5ac0e-862">For example:</span></span>

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

<span data-ttu-id="5ac0e-863">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-863">**New behavior**</span></span>

<span data-ttu-id="5ac0e-864">Tento scénář je nyní zjištěn v sestavování modelu a je vyvolána výjimka označující, že je model dvojznačný.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-864">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="5ac0e-865">**Proč**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-865">**Why**</span></span>

<span data-ttu-id="5ac0e-866">Výsledný model byl nejednoznačný a pravděpodobně bude pro tento případ obvykle špatný.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-866">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="5ac0e-867">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="5ac0e-867">**Mitigations**</span></span>

<span data-ttu-id="5ac0e-868">Použijte úplnou konfiguraci relace.</span><span class="sxs-lookup"><span data-stu-id="5ac0e-868">Use full configuration of the relationship.</span></span> <span data-ttu-id="5ac0e-869">Příklad:</span><span class="sxs-lookup"><span data-stu-id="5ac0e-869">For example:</span></span>

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
