---
title: Přerušující změny v EF Core 3,0 – EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 0dd4c5c4aa1a5d241fb48abf1372a678d0f7a7a3
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813623"
---
# <a name="breaking-changes-included-in-ef-core-30"></a><span data-ttu-id="8b2e3-102">Přerušující změny zahrnuté v EF Core 3,0</span><span class="sxs-lookup"><span data-stu-id="8b2e3-102">Breaking changes included in EF Core 3.0</span></span>
<span data-ttu-id="8b2e3-103">Následující změny rozhraní API a chování mají možnost rušit existující aplikace při jejich upgradu na 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-103">The following API and behavior changes have the potential to break existing applications when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="8b2e3-104">Změny, které očekáváme jenom o to, aby ovlivnili pouze poskytovatele databází, jsou popsané v části [změny zprostředkovatele](xref:core/providers/provider-log).</span><span class="sxs-lookup"><span data-stu-id="8b2e3-104">Changes that we expect to only impact database providers are documented under [provider changes](xref:core/providers/provider-log).</span></span>

## <a name="summary"></a><span data-ttu-id="8b2e3-105">Souhrn</span><span class="sxs-lookup"><span data-stu-id="8b2e3-105">Summary</span></span>

| <span data-ttu-id="8b2e3-106">**Zásadní změna**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-106">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="8b2e3-107">**Dopad**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-107">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="8b2e3-108">Dotazy LINQ již nejsou vyhodnocovány na klientovi.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-108">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="8b2e3-109">Vysoká</span><span class="sxs-lookup"><span data-stu-id="8b2e3-109">High</span></span>       |
| [<span data-ttu-id="8b2e3-110">EF Core 3,0 cíle .NET Standard 2,1 místo .NET Standard 2,0</span><span class="sxs-lookup"><span data-stu-id="8b2e3-110">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="8b2e3-111">Vysoká</span><span class="sxs-lookup"><span data-stu-id="8b2e3-111">High</span></span>      |
| [<span data-ttu-id="8b2e3-112">EF Core nástroj příkazového řádku dotnet EF již není součástí .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="8b2e3-112">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="8b2e3-113">Vysoká</span><span class="sxs-lookup"><span data-stu-id="8b2e3-113">High</span></span>      |
| [<span data-ttu-id="8b2e3-114">DetectChanges respektuje hodnoty klíčů generované úložištěm.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-114">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="8b2e3-115">Vysoká</span><span class="sxs-lookup"><span data-stu-id="8b2e3-115">High</span></span>      |
| [<span data-ttu-id="8b2e3-116">Z tabulek, ExecuteSql a ExecuteSqlAsync byly přejmenovány.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-116">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="8b2e3-117">Vysoká</span><span class="sxs-lookup"><span data-stu-id="8b2e3-117">High</span></span>      |
| [<span data-ttu-id="8b2e3-118">Typy dotazů jsou konsolidovány s typy entit</span><span class="sxs-lookup"><span data-stu-id="8b2e3-118">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="8b2e3-119">Vysoká</span><span class="sxs-lookup"><span data-stu-id="8b2e3-119">High</span></span>      |
| [<span data-ttu-id="8b2e3-120">Entity Framework Core už není součástí sdílené ASP.NET Core architektury.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-120">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="8b2e3-121">Střední</span><span class="sxs-lookup"><span data-stu-id="8b2e3-121">Medium</span></span>      |
| [<span data-ttu-id="8b2e3-122">Odstranění kaskádových operací se teď ve výchozím nastavení provádí hned.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-122">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="8b2e3-123">Střední</span><span class="sxs-lookup"><span data-stu-id="8b2e3-123">Medium</span></span>      |
| [<span data-ttu-id="8b2e3-124">DeleteBehavior. restrict má sémantiku čištění.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-124">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="8b2e3-125">Střední</span><span class="sxs-lookup"><span data-stu-id="8b2e3-125">Medium</span></span>      |
| [<span data-ttu-id="8b2e3-126">Změnilo se konfigurační rozhraní API pro vztahy vlastněných typů.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-126">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="8b2e3-127">Střední</span><span class="sxs-lookup"><span data-stu-id="8b2e3-127">Medium</span></span>      |
| [<span data-ttu-id="8b2e3-128">Každá vlastnost používá nezávislou generaci celočíselného klíče v paměti.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-128">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="8b2e3-129">Střední</span><span class="sxs-lookup"><span data-stu-id="8b2e3-129">Medium</span></span>      |
| [<span data-ttu-id="8b2e3-130">Žádné dotazy pro sledování neprovádějí překlad identity</span><span class="sxs-lookup"><span data-stu-id="8b2e3-130">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="8b2e3-131">Střední</span><span class="sxs-lookup"><span data-stu-id="8b2e3-131">Medium</span></span>      |
| [<span data-ttu-id="8b2e3-132">Změny rozhraní API pro metadata</span><span class="sxs-lookup"><span data-stu-id="8b2e3-132">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="8b2e3-133">Střední</span><span class="sxs-lookup"><span data-stu-id="8b2e3-133">Medium</span></span>      |
| [<span data-ttu-id="8b2e3-134">Změny rozhraní API pro konkrétního zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="8b2e3-134">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="8b2e3-135">Střední</span><span class="sxs-lookup"><span data-stu-id="8b2e3-135">Medium</span></span>      |
| [<span data-ttu-id="8b2e3-136">UseRowNumberForPaging se odebral.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-136">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="8b2e3-137">Střední</span><span class="sxs-lookup"><span data-stu-id="8b2e3-137">Medium</span></span>      |
| [<span data-ttu-id="8b2e3-138">Metody Z tabulek se dají zadat jenom v kořenech dotazů.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-138">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="8b2e3-139">Nízká</span><span class="sxs-lookup"><span data-stu-id="8b2e3-139">Low</span></span>      |
| [<span data-ttu-id="8b2e3-140">~~Provádění dotazu se protokoluje na úrovni ladění~~ . Vrátit</span><span class="sxs-lookup"><span data-stu-id="8b2e3-140">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="8b2e3-141">Nízká</span><span class="sxs-lookup"><span data-stu-id="8b2e3-141">Low</span></span>      |
| [<span data-ttu-id="8b2e3-142">Dočasné hodnoty klíčů už nejsou nastavené na instance entit.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-142">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="8b2e3-143">Nízká</span><span class="sxs-lookup"><span data-stu-id="8b2e3-143">Low</span></span>      |
| [<span data-ttu-id="8b2e3-144">Závislé entity, které sdílí tabulku s objektem zabezpečení, jsou teď volitelné.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-144">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="8b2e3-145">Nízká</span><span class="sxs-lookup"><span data-stu-id="8b2e3-145">Low</span></span>      |
| [<span data-ttu-id="8b2e3-146">Všechny entity sdílející tabulku se sloupcem souběžného tokenu musí být namapovány na vlastnost.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-146">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="8b2e3-147">Nízká</span><span class="sxs-lookup"><span data-stu-id="8b2e3-147">Low</span></span>      |
| [<span data-ttu-id="8b2e3-148">Zděděné vlastnosti z nemapovaných typů jsou nyní namapovány na jeden sloupec pro všechny odvozené typy.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-148">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="8b2e3-149">Nízká</span><span class="sxs-lookup"><span data-stu-id="8b2e3-149">Low</span></span>      |
| [<span data-ttu-id="8b2e3-150">Konvence vlastností cizího klíče už neodpovídá stejnému názvu jako vlastnost Principal.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-150">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="8b2e3-151">Nízká</span><span class="sxs-lookup"><span data-stu-id="8b2e3-151">Low</span></span>      |
| [<span data-ttu-id="8b2e3-152">Připojení k databázi je teď zavřené, pokud už nepoužíváte dřív, než se dokončí jeho objekt TransactionScope.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-152">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="8b2e3-153">Nízká</span><span class="sxs-lookup"><span data-stu-id="8b2e3-153">Low</span></span>      |
| [<span data-ttu-id="8b2e3-154">Ve výchozím nastavení se používají pole pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-154">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="8b2e3-155">Nízká</span><span class="sxs-lookup"><span data-stu-id="8b2e3-155">Low</span></span>      |
| [<span data-ttu-id="8b2e3-156">Vyvolat, zda je nalezeno více kompatibilních zálohovaných polí</span><span class="sxs-lookup"><span data-stu-id="8b2e3-156">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="8b2e3-157">Nízká</span><span class="sxs-lookup"><span data-stu-id="8b2e3-157">Low</span></span>      |
| [<span data-ttu-id="8b2e3-158">Názvy vlastností pouze pro pole se musí shodovat s názvem pole.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-158">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="8b2e3-159">Nízká</span><span class="sxs-lookup"><span data-stu-id="8b2e3-159">Low</span></span>      |
| [<span data-ttu-id="8b2e3-160">AddDbContext/AddDbContextPool už nevolá AddLogging a AddMemoryCache.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-160">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="8b2e3-161">Nízká</span><span class="sxs-lookup"><span data-stu-id="8b2e3-161">Low</span></span>      |
| [<span data-ttu-id="8b2e3-162">DbContext. entry teď provádí místní DetectChanges.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-162">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="8b2e3-163">Nízká</span><span class="sxs-lookup"><span data-stu-id="8b2e3-163">Low</span></span>      |
| [<span data-ttu-id="8b2e3-164">Klíče řetězce a pole bajtů nejsou ve výchozím nastavení generovány klientem.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-164">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="8b2e3-165">Nízká</span><span class="sxs-lookup"><span data-stu-id="8b2e3-165">Low</span></span>      |
| [<span data-ttu-id="8b2e3-166">ILoggerFactory je teď služba s vymezeným oborem.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-166">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="8b2e3-167">Nízká</span><span class="sxs-lookup"><span data-stu-id="8b2e3-167">Low</span></span>      |
| [<span data-ttu-id="8b2e3-168">Opožděné načítání proxy serverů už nepředpokládá navigační vlastnosti, jsou plně načtené.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-168">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="8b2e3-169">Nízká</span><span class="sxs-lookup"><span data-stu-id="8b2e3-169">Low</span></span>      |
| [<span data-ttu-id="8b2e3-170">Nadměrné vytváření interních zprostředkovatelů služeb je teď ve výchozím nastavení chyba.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-170">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="8b2e3-171">Nízká</span><span class="sxs-lookup"><span data-stu-id="8b2e3-171">Low</span></span>      |
| [<span data-ttu-id="8b2e3-172">Nové chování pro HasOne/HasMany se volá s jedním řetězcem.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-172">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="8b2e3-173">Nízká</span><span class="sxs-lookup"><span data-stu-id="8b2e3-173">Low</span></span>      |
| [<span data-ttu-id="8b2e3-174">Návratový typ pro několik asynchronních metod byl změněn z úlohy na ValueTask</span><span class="sxs-lookup"><span data-stu-id="8b2e3-174">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="8b2e3-175">Nízká</span><span class="sxs-lookup"><span data-stu-id="8b2e3-175">Low</span></span>      |
| [<span data-ttu-id="8b2e3-176">Relační: anotace TypeMapping je nyní pouze TypeMapping</span><span class="sxs-lookup"><span data-stu-id="8b2e3-176">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="8b2e3-177">Nízká</span><span class="sxs-lookup"><span data-stu-id="8b2e3-177">Low</span></span>      |
| [<span data-ttu-id="8b2e3-178">ToTable na odvozeném typu vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-178">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="8b2e3-179">Nízká</span><span class="sxs-lookup"><span data-stu-id="8b2e3-179">Low</span></span>      |
| [<span data-ttu-id="8b2e3-180">EF Core už neposílá direktivu pragma pro vynucení KOFK SQLite.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-180">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="8b2e3-181">Nízká</span><span class="sxs-lookup"><span data-stu-id="8b2e3-181">Low</span></span>      |
| [<span data-ttu-id="8b2e3-182">Microsoft. EntityFrameworkCore. sqlite teď závisí na SQLitePCLRaw. bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="8b2e3-182">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="8b2e3-183">Nízká</span><span class="sxs-lookup"><span data-stu-id="8b2e3-183">Low</span></span>      |
| [<span data-ttu-id="8b2e3-184">Hodnoty GUID se teď ukládají jako TEXT na SQLite.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-184">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="8b2e3-185">Nízká</span><span class="sxs-lookup"><span data-stu-id="8b2e3-185">Low</span></span>      |
| [<span data-ttu-id="8b2e3-186">Hodnoty char se teď ukládají jako TEXT na SQLite.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-186">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="8b2e3-187">Nízká</span><span class="sxs-lookup"><span data-stu-id="8b2e3-187">Low</span></span>      |
| [<span data-ttu-id="8b2e3-188">ID migrace se teď generují pomocí kalendáře invariantní jazykové verze.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-188">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="8b2e3-189">Nízká</span><span class="sxs-lookup"><span data-stu-id="8b2e3-189">Low</span></span>      |
| [<span data-ttu-id="8b2e3-190">Informace o rozšíření/metadata se odebraly z IDbContextOptionsExtension.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-190">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="8b2e3-191">Nízká</span><span class="sxs-lookup"><span data-stu-id="8b2e3-191">Low</span></span>      |
| [<span data-ttu-id="8b2e3-192">LogQueryPossibleExceptionWithAggregateOperator se přejmenovalo.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-192">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="8b2e3-193">Nízká</span><span class="sxs-lookup"><span data-stu-id="8b2e3-193">Low</span></span>      |
| [<span data-ttu-id="8b2e3-194">Vysvětlení rozhraní API pro názvy omezení cizího klíče</span><span class="sxs-lookup"><span data-stu-id="8b2e3-194">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="8b2e3-195">Nízká</span><span class="sxs-lookup"><span data-stu-id="8b2e3-195">Low</span></span>      |
| [<span data-ttu-id="8b2e3-196">IRelationalDatabaseCreator. HasTables/HasTablesAsync byly zveřejněny.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-196">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="8b2e3-197">Nízká</span><span class="sxs-lookup"><span data-stu-id="8b2e3-197">Low</span></span>      |
| [<span data-ttu-id="8b2e3-198">Microsoft. EntityFrameworkCore. Design je teď balíček DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-198">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="8b2e3-199">Nízká</span><span class="sxs-lookup"><span data-stu-id="8b2e3-199">Low</span></span>      |
| [<span data-ttu-id="8b2e3-200">SQLitePCL. Raw aktualizováno na verzi 2.0.0</span><span class="sxs-lookup"><span data-stu-id="8b2e3-200">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="8b2e3-201">Nízká</span><span class="sxs-lookup"><span data-stu-id="8b2e3-201">Low</span></span>      |
| [<span data-ttu-id="8b2e3-202">NetTopologySuite aktualizace na verzi 2.0.0</span><span class="sxs-lookup"><span data-stu-id="8b2e3-202">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="8b2e3-203">Nízká</span><span class="sxs-lookup"><span data-stu-id="8b2e3-203">Low</span></span>      |
| [<span data-ttu-id="8b2e3-204">Je nutné nakonfigurovat více dvojznačných relací odkazujících na sebe.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-204">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="8b2e3-205">Nízká</span><span class="sxs-lookup"><span data-stu-id="8b2e3-205">Low</span></span>      |
| [<span data-ttu-id="8b2e3-206">DbFunction. Schema má hodnotu null nebo je prázdný řetězec, který nakonfiguruje, aby byl ve výchozím schématu modelu.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-206">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>](#udf-empty-string) | <span data-ttu-id="8b2e3-207">Nízká</span><span class="sxs-lookup"><span data-stu-id="8b2e3-207">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="8b2e3-208">Dotazy LINQ již nejsou vyhodnocovány na klientovi.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-208">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="8b2e3-209">[Sledování problému #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[taky zobrazit problémy #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="8b2e3-209">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="8b2e3-210">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-210">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-211">Před 3,0, když EF Core nedokázal převést výraz, který byl součástí dotazu na buď SQL, nebo parametr, automaticky vyhodnotí výraz na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-211">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="8b2e3-212">Ve výchozím nastavení vyhodnocování klientů u potenciálně nákladných výrazů aktivovalo pouze upozornění.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-212">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="8b2e3-213">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-213">**New behavior**</span></span>

<span data-ttu-id="8b2e3-214">Počínaje 3,0 EF Core v rámci projekce nejvyšší úrovně (posledního `Select()` volání v dotazu) jenom výrazy vyhodnotit na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-214">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="8b2e3-215">Pokud výrazy v jakékoli jiné části dotazu nelze převést na buď SQL, nebo parametr, je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-215">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="8b2e3-216">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-216">**Why**</span></span>

<span data-ttu-id="8b2e3-217">Automatické hodnocení dotazů umožňuje provádět mnoho dotazů i v případě, že není možné přeložit jejich důležité části.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-217">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="8b2e3-218">Toto chování může vést k neočekávanému a potenciálně škodlivému chování, které může být jen zřejmé v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-218">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="8b2e3-219">Například podmínka ve `Where()` volání, která se nedá přeložit, může způsobit přenesení všech řádků z databázového serveru a filtr, který se má použít na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-219">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="8b2e3-220">Tato situace může snadno přejít zpět, pokud tabulka obsahuje jenom několik řádků ve vývoji, ale při přesunu aplikace do produkčního prostředí zasáhnout, kde tabulka může obsahovat miliony řádků.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-220">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="8b2e3-221">Upozornění na vyhodnocení klientů se také během vývoje ukázala jako příliš jednoduchá.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-221">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="8b2e3-222">Kromě toho může automatické hodnocení klienta vést k problémům s tím, že vylepšení překladu dotazů pro konkrétní výrazy způsobilo nezamýšlené neúmyslné změny mezi verzemi.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-222">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="8b2e3-223">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-223">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-224">Pokud dotaz nelze plně přeložit, pak buď Přepište dotaz do formuláře, který lze přeložit, nebo použijte `AsEnumerable()`, `ToList()`nebo podobným způsobem explicitně přeneste data zpět do klienta, kde lze následně dále zpracovávat pomocí LINQ-to-Objects.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-224">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="8b2e3-225">EF Core 3,0 cíle .NET Standard 2,1 místo .NET Standard 2,0</span><span class="sxs-lookup"><span data-stu-id="8b2e3-225">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="8b2e3-226">Sledování problému #15498</span><span class="sxs-lookup"><span data-stu-id="8b2e3-226">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

<span data-ttu-id="8b2e3-227">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-227">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-228">Před 3,0 EF Core cílené .NET Standard 2,0 a spustí se na všech platformách, které podporují tento standard, včetně .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-228">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="8b2e3-229">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-229">**New behavior**</span></span>

<span data-ttu-id="8b2e3-230">Počínaje 3,0 se EF Core cíle .NET Standard 2,1 a spustí se na všech platformách, které podporují tento standard.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-230">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="8b2e3-231">To nezahrnuje .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-231">This does not include .NET Framework.</span></span>

<span data-ttu-id="8b2e3-232">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-232">**Why**</span></span>

<span data-ttu-id="8b2e3-233">Toto je součást strategického rozhodnutí napříč technologiemi .NET a zaměřuje se na energii na platformě .NET Core a dalších moderních platformách .NET, jako je Xamarin.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-233">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="8b2e3-234">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-234">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-235">Zvažte přechod na moderní platformu .NET.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-235">Consider moving to a modern .NET platform.</span></span> <span data-ttu-id="8b2e3-236">Pokud to není možné, pak pokračujte v používání EF Core 2,1 nebo EF Core 2,2, které podporují .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-236">If this is not possible, then continue to use EF Core 2.1 or EF Core 2.2, both of which support .NET Framework.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="8b2e3-237">Entity Framework Core už není součástí sdílené ASP.NET Core architektury.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-237">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="8b2e3-238">Sledování oznámení o problémech # 325</span><span class="sxs-lookup"><span data-stu-id="8b2e3-238">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="8b2e3-239">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-239">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-240">Před ASP.NET Core 3,0 se při přidání odkazu na balíček do `Microsoft.AspNetCore.App` nebo `Microsoft.AspNetCore.All`zahrnovala EF Core a někteří poskytovatelé EF Core dat jako poskytovatel SQL Server.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-240">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="8b2e3-241">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-241">**New behavior**</span></span>

<span data-ttu-id="8b2e3-242">Počínaje 3,0 ASP.NET Core sdílené rozhraní nezahrnuje EF Core nebo žádné poskytovatele EF Core dat.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-242">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="8b2e3-243">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-243">**Why**</span></span>

<span data-ttu-id="8b2e3-244">Před touto změnou EF Core nutné jiné kroky v závislosti na tom, zda je aplikace cílena ASP.NET Core a SQL Server.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-244">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="8b2e3-245">Upgrade ASP.NET Core taky vynutil upgrade EF Core a poskytovatele SQL Server, který není vždycky žádoucí.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-245">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="8b2e3-246">V důsledku této změny je prostředí získání EF Core stejné u všech poskytovatelů, podporovaných implementací rozhraní .NET a typů aplikací.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-246">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="8b2e3-247">Vývojáři teď můžou přesně řídit, když EF Core a EF Core se upgradují poskytovatelé dat.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-247">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="8b2e3-248">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-248">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-249">Chcete-li použít EF Core v aplikaci ASP.NET Core 3,0 nebo v jakékoli jiné podporované aplikaci, explicitně přidejte odkaz na balíček EF Coreho poskytovatele databáze, který bude aplikace používat.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-249">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="8b2e3-250">EF Core nástroj příkazového řádku dotnet EF již není součástí .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="8b2e3-250">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="8b2e3-251">Sledování problému #14016</span><span class="sxs-lookup"><span data-stu-id="8b2e3-251">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="8b2e3-252">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-252">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-253">Před 3,0 `dotnet ef` byl nástroj součástí .NET Core SDK a byl snadno dostupný pro použití z příkazového řádku z libovolného projektu bez nutnosti dalších kroků.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-253">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="8b2e3-254">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-254">**New behavior**</span></span>

<span data-ttu-id="8b2e3-255">Počínaje 3,0 sada .NET SDK nezahrnuje `dotnet ef` nástroj, takže před tím, než ho budete moct použít, je nutné explicitně nainstalovat jako místní nebo globální nástroj.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-255">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="8b2e3-256">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-256">**Why**</span></span>

<span data-ttu-id="8b2e3-257">Tato změna nám umožňuje distribuovat a aktualizovat `dotnet ef` jako běžný nástroj rozhraní .NET CLI v NuGet, a to v souladu se skutečností, že EF Core 3,0 je také vždy distribuován jako balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-257">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="8b2e3-258">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-258">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-259">Aby bylo možné spravovat migrace nebo uživatelské rozhraní a `DbContext`, nainstalujte `dotnet-ef` nástroj jako globální nástroj:</span><span class="sxs-lookup"><span data-stu-id="8b2e3-259">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef
  ```

<span data-ttu-id="8b2e3-260">Můžete také získat místní nástroj při obnovování závislostí projektu, který deklaruje jako závislost nástrojů pomocí [souboru manifestu nástroje](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="8b2e3-260">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="8b2e3-261">Z tabulek, ExecuteSql a ExecuteSqlAsync byly přejmenovány.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-261">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="8b2e3-262">Sledování problému #10996</span><span class="sxs-lookup"><span data-stu-id="8b2e3-262">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="8b2e3-263">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-263">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-264">Před EF Core 3,0 byly tyto názvy metod přetíženy, aby fungovaly s normálním řetězcem nebo řetězcem, které by měly být interpolované na SQL a parametry.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-264">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="8b2e3-265">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-265">**New behavior**</span></span>

<span data-ttu-id="8b2e3-266">Počínaje EF Core 3,0, použijte `FromSqlRaw`, `ExecuteSqlRaw`a `ExecuteSqlRawAsync` k vytvoření parametrizovaného dotazu, kde jsou parametry předány nezávisle na řetězci dotazu.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-266">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="8b2e3-267">Příklad:</span><span class="sxs-lookup"><span data-stu-id="8b2e3-267">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="8b2e3-268">Použijte `FromSqlInterpolated`, `ExecuteSqlInterpolated` a`ExecuteSqlInterpolatedAsync` k vytvoření parametrizovaného dotazu, kde jsou parametry předány jako součást interpolované řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-268">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="8b2e3-269">Příklad:</span><span class="sxs-lookup"><span data-stu-id="8b2e3-269">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="8b2e3-270">Všimněte si, že obě výše uvedené dotazy vytvoří stejný parametrizovaný SQL se stejnými parametry SQL.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-270">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="8b2e3-271">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-271">**Why**</span></span>

<span data-ttu-id="8b2e3-272">Přetížení metody, jako to, usnadňuje náhodné volání nezpracované řetězcové metody, pokud by záměr byl zavolat interpolovaná řetězcová metoda a druhá možnost kolem.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-272">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="8b2e3-273">To může vést k tomu, že dotazy nejsou parametrizované, pokud by měly být.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-273">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="8b2e3-274">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-274">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-275">Přepněte na použití nových názvů metod.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-275">Switch to use the new method names.</span></span>

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="8b2e3-276">Metody Z tabulek se dají zadat jenom v kořenech dotazů.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-276">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="8b2e3-277">Sledování problému #15704</span><span class="sxs-lookup"><span data-stu-id="8b2e3-277">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="8b2e3-278">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-278">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-279">Před EF Core 3,0 `FromSql` lze metodu zadat kdekoli v dotazu.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-279">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="8b2e3-280">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-280">**New behavior**</span></span>

<span data-ttu-id="8b2e3-281">Počínaje EF Core `FromSqlRaw` 3,0 lze nové metody a `FromSqlInterpolated` (které nahradí `FromSql`) zadat pouze v kořenech `DbSet<>`dotazu, tj. přímo na.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-281">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="8b2e3-282">Pokud se pokusíte zadat jiné místo jinak, dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-282">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="8b2e3-283">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-283">**Why**</span></span>

<span data-ttu-id="8b2e3-284">Určení `FromSql` kdekoli jinde než u a `DbSet` neobsahovalo žádné přidané významy nebo přidání hodnoty a může v některých scénářích způsobit nejednoznačnost.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-284">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="8b2e3-285">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-285">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-286">`FromSql`volání by se měla přesunout přímo na, `DbSet` na které se vztahují.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-286">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="8b2e3-287">Žádné dotazy pro sledování neprovádějí překlad identity</span><span class="sxs-lookup"><span data-stu-id="8b2e3-287">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="8b2e3-288">Sledování problému #13518</span><span class="sxs-lookup"><span data-stu-id="8b2e3-288">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="8b2e3-289">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-289">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-290">Před EF Core 3,0 se stejná instance entity používá pro všechny výskyty entity se zadaným typem a ID.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-290">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="8b2e3-291">To odpovídá chování sledovacích dotazů.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-291">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="8b2e3-292">Například tento dotaz:</span><span class="sxs-lookup"><span data-stu-id="8b2e3-292">For example, this query:</span></span>

```C#
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="8b2e3-293">vrátí stejnou `Category` instanci pro každý `Product` , který je spojen s danou kategorií.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-293">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="8b2e3-294">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-294">**New behavior**</span></span>

<span data-ttu-id="8b2e3-295">Počínaje EF Core 3,0 budou vytvořeny různé instance entit při výskytu entity se zadaným typem a ID na různých místech vráceného grafu.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-295">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="8b2e3-296">Například dotaz výše bude nyní vracet novou `Category` instanci pro každou `Product` , i když jsou ke stejné kategorii přidruženy dva produkty.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-296">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="8b2e3-297">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-297">**Why**</span></span>

<span data-ttu-id="8b2e3-298">Překlad identity (to znamená, že určení, že entita má stejný typ a ID jako dříve zjištěná entita) přidává další výkon a režii paměti.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-298">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="8b2e3-299">Obvykle se spustí čítač, aby se v prvním místě nepoužily žádné dotazy na sledování.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-299">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="8b2e3-300">I když může být v některých případech užitečný překlad identity, není potřeba, pokud se entity mají serializovat a odeslat klientovi, což je běžné pro žádné dotazy pro sledování.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-300">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="8b2e3-301">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-301">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-302">Pokud je vyžadováno rozlišení identity, použijte dotaz sledování.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-302">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="8b2e3-303">~~Provádění dotazu se protokoluje na úrovni ladění~~ . Vrátit</span><span class="sxs-lookup"><span data-stu-id="8b2e3-303">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="8b2e3-304">Sledování problému #14523</span><span class="sxs-lookup"><span data-stu-id="8b2e3-304">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="8b2e3-305">Tuto změnu jsme vrátili, protože nová konfigurace v EF Core 3,0 umožňuje, aby byla úroveň protokolu pro libovolnou událost specifikována aplikací.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-305">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="8b2e3-306">Například chcete-li přepnout protokolování SQL na `Debug`, explicitně nakonfigurovat úroveň v `OnConfiguring` nebo `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="8b2e3-306">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="8b2e3-307">Dočasné hodnoty klíčů už nejsou nastavené na instance entit.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-307">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="8b2e3-308">Sledování problému #12378</span><span class="sxs-lookup"><span data-stu-id="8b2e3-308">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="8b2e3-309">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-309">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-310">Před EF Core 3,0 byly přiřazeny dočasné hodnoty ke všem klíčovým vlastnostem, které by později měly skutečnou hodnotu vygenerovanou databází.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-310">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="8b2e3-311">Obvykle jsou tyto dočasné hodnoty velkými zápornými čísly.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-311">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="8b2e3-312">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-312">**New behavior**</span></span>

<span data-ttu-id="8b2e3-313">Počínaje 3,0 EF Core ukládá hodnotu dočasného klíče jako součást informací o sledování entity a ponechá vlastnost klíče beze změny.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-313">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="8b2e3-314">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-314">**Why**</span></span>

<span data-ttu-id="8b2e3-315">Tato změna byla provedena, aby se předešlo omylům hodnotám klíčů v případě, že entita, která byla dříve sledována pomocí nějaké `DbContext` instance, je přesunuta do jiné `DbContext` instance.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-315">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="8b2e3-316">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-316">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-317">Aplikace, které přiřazují hodnoty primárního klíče k cizím klíčům k vytvoření přidružení mezi entitami, můžou záviset na starém chování, pokud jsou primární klíče generované úložištěm a patří do entit ve `Added` stavu.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-317">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="8b2e3-318">K tomu je možné se vyhnout:</span><span class="sxs-lookup"><span data-stu-id="8b2e3-318">This can be avoided by:</span></span>
* <span data-ttu-id="8b2e3-319">Nepoužívejte klíče generované úložištěm.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-319">Not using store-generated keys.</span></span>
* <span data-ttu-id="8b2e3-320">Nastavení vlastností navigace pro vytváření relací místo nastavení hodnot cizích klíčů.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-320">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="8b2e3-321">Získá aktuální dočasné hodnoty klíče z informací o sledování entity.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-321">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="8b2e3-322">Například vrátí dočasnou hodnotu, `context.Entry(blog).Property(e => e.Id).CurrentValue` i když `blog.Id` nebyla nastavena.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-322">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="8b2e3-323">DetectChanges respektuje hodnoty klíčů generované úložištěm.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-323">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="8b2e3-324">Sledování problému #14616</span><span class="sxs-lookup"><span data-stu-id="8b2e3-324">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="8b2e3-325">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-325">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-326">Předtím, než EF Core 3,0, nalezené `DetectChanges` Nesledované entity by byly sledovány `Added` ve stavu a vloženy jako nový řádek při `SaveChanges` volání.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-326">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="8b2e3-327">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-327">**New behavior**</span></span>

<span data-ttu-id="8b2e3-328">Počínaje EF Core 3,0 platí, že pokud entita používá vygenerované hodnoty klíčů a je nastavená nějaká hodnota klíče, bude se entita sledovat ve `Modified` stavu.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-328">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="8b2e3-329">To znamená, že se předpokládá, že řádek pro entitu existuje a že bude při `SaveChanges` volání aktualizována.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-329">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="8b2e3-330">Pokud hodnota klíče není nastavená, nebo pokud typ entity nepoužívá vygenerované klíče, bude se nová entita dál sledovat jako `Added` v předchozích verzích.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-330">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="8b2e3-331">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-331">**Why**</span></span>

<span data-ttu-id="8b2e3-332">Tato změna byla provedena tak, aby byla pro práci s nepřipojenými grafy entit při použití klíčů generovaných úložištěm snazší.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-332">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="8b2e3-333">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-333">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-334">Tato změna může přerušit aplikaci, pokud je typ entity nakonfigurovaný na používání vygenerovaných klíčů, ale hodnoty klíčů jsou explicitně nastavené pro nové instance.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-334">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="8b2e3-335">Opravou je explicitně nakonfigurovat klíčové vlastnosti tak, aby nepoužívaly vygenerované hodnoty.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-335">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="8b2e3-336">Například s rozhraním API Fluent:</span><span class="sxs-lookup"><span data-stu-id="8b2e3-336">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="8b2e3-337">Nebo s poznámkami k datům:</span><span class="sxs-lookup"><span data-stu-id="8b2e3-337">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="8b2e3-338">Odstranění kaskádových operací se teď ve výchozím nastavení provádí hned.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-338">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="8b2e3-339">Sledování problému #10114</span><span class="sxs-lookup"><span data-stu-id="8b2e3-339">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="8b2e3-340">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-340">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-341">Před 3,0 EF Core aplikovány kaskádové akce (odstraňování závislých entit, když se odstraní požadovaný objekt zabezpečení nebo když je vztah k požadovanému objektu zabezpečení vážně) nevznikl, dokud není voláno SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-341">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="8b2e3-342">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-342">**New behavior**</span></span>

<span data-ttu-id="8b2e3-343">Od 3,0 EF Core aplikuje kaskádové akce hned po zjištění spouštěcí podmínky.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-343">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="8b2e3-344">Například volání `context.Remove()` na odstranění objektu zabezpečení bude mít za následek, že jsou všechny sledované požadované závislé položky nastaveny také na `Deleted` hodnotu okamžitě.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-344">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="8b2e3-345">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-345">**Why**</span></span>

<span data-ttu-id="8b2e3-346">Tato změna byla provedena za účelem zlepšení prostředí pro scénáře vytváření datových vazeb a auditování, kde je důležité pochopit, které entity budou _před_ `SaveChanges` voláním odstraněny.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-346">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="8b2e3-347">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-347">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-348">Předchozí chování lze obnovit pomocí nastavení v `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-348">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="8b2e3-349">Příklad:</span><span class="sxs-lookup"><span data-stu-id="8b2e3-349">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="8b2e3-350">DeleteBehavior. restrict má sémantiku čištění.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-350">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="8b2e3-351">Sledování problému #12661</span><span class="sxs-lookup"><span data-stu-id="8b2e3-351">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="8b2e3-352">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-352">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-353">Před 3,0 se `DeleteBehavior.Restrict` v databázi vytvořily cizí klíče se `Restrict` sémantikou, ale zároveň se nezřetelně změnila interní oprava.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-353">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="8b2e3-354">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-354">**New behavior**</span></span>

<span data-ttu-id="8b2e3-355">Počínaje 3,0 `DeleteBehavior.Restrict` zajistí, že se cizí klíče vytvoří s `Restrict` sémantikou – to znamená bez kaskády; porušení omezení throw--bez vlivu na interní opravu EF.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-355">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="8b2e3-356">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-356">**Why**</span></span>

<span data-ttu-id="8b2e3-357">Tato změna byla provedena pro zlepšení prostředí pro použití `DeleteBehavior` intuitivním způsobem, a to bez neočekávaných vedlejších účinků.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-357">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="8b2e3-358">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-358">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-359">Předchozí chování lze obnovit pomocí `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-359">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="8b2e3-360">Typy dotazů jsou konsolidovány s typy entit</span><span class="sxs-lookup"><span data-stu-id="8b2e3-360">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="8b2e3-361">Sledování problému #14194</span><span class="sxs-lookup"><span data-stu-id="8b2e3-361">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="8b2e3-362">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-362">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-363">Před EF Core 3,0 byly [typy dotazů](xref:core/modeling/keyless-entity-types) prostředkem pro dotazování na data, která nedefinují primární klíč strukturovaným způsobem.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-363">Before EF Core 3.0, [query types](xref:core/modeling/keyless-entity-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="8b2e3-364">To znamená, že typ dotazu byl použit pro mapování typů entit bez klíčů (pravděpodobnější z pohledu, ale také z tabulky), zatímco byl použit regulární typ entity, když byl klíč k dispozici (pravděpodobnější z tabulky, ale případně z pohledu).</span><span class="sxs-lookup"><span data-stu-id="8b2e3-364">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="8b2e3-365">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-365">**New behavior**</span></span>

<span data-ttu-id="8b2e3-366">Typ dotazu se teď bude jednat jenom o typ entity bez primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-366">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="8b2e3-367">Typy entit bez klíčů mají stejné funkce jako typy dotazů v předchozích verzích.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-367">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="8b2e3-368">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-368">**Why**</span></span>

<span data-ttu-id="8b2e3-369">Tato změna byla provedena proto, aby se snížila nejasnost v souvislosti s typy dotazů.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-369">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="8b2e3-370">Konkrétně jsou bez klíčů typy entit a jsou ze své podstaty jen pro čtení, ale neměly by být použity pouze proto, že typ entity musí být jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-370">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="8b2e3-371">Podobně jsou často mapovány na zobrazení, ale to je pouze v případě, že zobrazení často nedefinují klíče.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-371">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="8b2e3-372">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-372">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-373">Následující části rozhraní API jsou teď zastaralé:</span><span class="sxs-lookup"><span data-stu-id="8b2e3-373">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="8b2e3-374">**`ModelBuilder.Query<>()`** – Místo `ModelBuilder.Entity<>().HasNoKey()` toho je nutné volat k označení typu entity, protože neobsahují žádné klíče.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-374">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="8b2e3-375">Tato služba by se ještě nenakonfigurovala pomocí konvence, aby nedocházelo k chybám konfigurace, když je primární klíč očekávaný, ale neodpovídá konvenci.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-375">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="8b2e3-376">**`DbQuery<>`** – Místo `DbSet<>` toho by se mělo použít.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-376">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="8b2e3-377">**`DbContext.Query<>()`** – Místo `DbContext.Set<>()` toho by se mělo použít.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-377">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="8b2e3-378">Změnilo se konfigurační rozhraní API pro vztahy vlastněných typů.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-378">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="8b2e3-379">[Sledování problému #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[sledování problému #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[#14153 sledování](https://github.com/aspnet/EntityFrameworkCore/issues/14153) problému</span><span class="sxs-lookup"><span data-stu-id="8b2e3-379">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="8b2e3-380">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-380">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-381">Před EF Core 3,0 byla konfigurace vztahu vlastníka provedena přímo po `OwnsOne` volání nebo. `OwnsMany`</span><span class="sxs-lookup"><span data-stu-id="8b2e3-381">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="8b2e3-382">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-382">**New behavior**</span></span>

<span data-ttu-id="8b2e3-383">Od EF Core 3,0 teď existuje Fluent API pro konfiguraci navigační vlastnosti pro vlastníka pomocí `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-383">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="8b2e3-384">Příklad:</span><span class="sxs-lookup"><span data-stu-id="8b2e3-384">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="8b2e3-385">Konfigurace vztahující se k vztahu mezi vlastníkem a vlastníkem by nyní měla být zřetězena `WithOwner()` podobně jako konfigurace ostatních vztahů.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-385">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="8b2e3-386">I když se konfigurace samotného vlastního typu bude i nadále zřetězit za `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-386">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="8b2e3-387">Příklad:</span><span class="sxs-lookup"><span data-stu-id="8b2e3-387">For example:</span></span>

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

<span data-ttu-id="8b2e3-388">Kromě toho `Entity()`, `HasOne()`že volání `Set()` ,, nebo s cílem cílového typu, nyní vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-388">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="8b2e3-389">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-389">**Why**</span></span>

<span data-ttu-id="8b2e3-390">Tato změna byla provedena za účelem vytvoření čisticího oddělení mezi konfigurací samotného typu a _vztahu k_ typu, který je vlastníkem.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-390">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="8b2e3-391">Tím se zase odeberou nejednoznačnosti a nejasnosti u metod, jako `HasForeignKey`je.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-391">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="8b2e3-392">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-392">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-393">Změňte konfiguraci vztahů vlastněných typů tak, aby používala novou plochu rozhraní API, jak je znázorněno v předchozím příkladu.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-393">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="8b2e3-394">Závislé entity, které sdílí tabulku s objektem zabezpečení, jsou teď volitelné.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-394">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="8b2e3-395">Sledování problému #9005</span><span class="sxs-lookup"><span data-stu-id="8b2e3-395">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="8b2e3-396">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-396">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-397">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="8b2e3-397">Consider the following model:</span></span>
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
<span data-ttu-id="8b2e3-398">Pokud `OrderDetails` je před EF Core 3,0 ve `Order` vlastnictví nebo explicitně namapována na stejnou tabulku `OrderDetails` , byla při přidávání nové `Order`instance vždy vyžadována instance.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-398">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="8b2e3-399">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-399">**New behavior**</span></span>

<span data-ttu-id="8b2e3-400">Počínaje 3,0 EF Core umožňuje přidat `Order` `OrderDetails` bez `OrderDetails` a mapování všech vlastností kromě primárního klíče na sloupce s možnou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-400">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="8b2e3-401">Při dotazování EF Core sady `OrderDetails` na `null` , pokud některá z jejích požadovaných vlastností nemá hodnotu, nebo pokud nemá žádné požadované vlastnosti kromě primárního klíče a všechny vlastnosti jsou `null`.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-401">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="8b2e3-402">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-402">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-403">Pokud má váš model sdílení tabulky závislé na všech volitelných sloupcích, ale u navigace, na kterou se odkazuje, se neočekává `null` , že by aplikace měla být upravena tak, aby zpracovávala případy, kdy je `null`navigace.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-403">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="8b2e3-404">Pokud to není možné, měla by být do daného typu entity přidána požadovaná vlastnost, nebo alespoň jedna vlastnost by k ní měla mít`null` přiřazenou hodnotu bez hodnoty.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-404">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="8b2e3-405">Všechny entity sdílející tabulku se sloupcem souběžného tokenu musí být namapovány na vlastnost.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-405">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="8b2e3-406">Sledování problému #14154</span><span class="sxs-lookup"><span data-stu-id="8b2e3-406">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="8b2e3-407">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-407">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-408">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="8b2e3-408">Consider the following model:</span></span>
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
<span data-ttu-id="8b2e3-409">Pokud `OrderDetails` je před EF Core 3,0, ve `Order` vlastnictví nebo explicitně namapovaných na `OrderDetails` stejnou tabulku, pak aktualizace nebude aktualizovat `Version` hodnotu u klienta a další aktualizace selže.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-409">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="8b2e3-410">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-410">**New behavior**</span></span>

<span data-ttu-id="8b2e3-411">Počínaje 3,0 EF Core rozšíří novou `Version` `Order` hodnotu, pokud je vlastní `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-411">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="8b2e3-412">V opačném případě je při ověřování modelu vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-412">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="8b2e3-413">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-413">**Why**</span></span>

<span data-ttu-id="8b2e3-414">Tato změna byla provedena, aby nedocházelo k zastaralé hodnotě tokenu souběžnosti, pokud je aktualizována pouze jedna z entit mapovaných na stejnou tabulku.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-414">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="8b2e3-415">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-415">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-416">Všechny entity, které sdílejí tabulku, musí obsahovat vlastnost, která je namapovaná na sloupec tokenu souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-416">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="8b2e3-417">Je možné, že ho vytvoříte ve stínovém stavu:</span><span class="sxs-lookup"><span data-stu-id="8b2e3-417">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="8b2e3-418">Zděděné vlastnosti z nemapovaných typů jsou nyní namapovány na jeden sloupec pro všechny odvozené typy.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-418">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="8b2e3-419">Sledování problému #13998</span><span class="sxs-lookup"><span data-stu-id="8b2e3-419">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="8b2e3-420">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-420">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-421">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="8b2e3-421">Consider the following model:</span></span>
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

<span data-ttu-id="8b2e3-422">Před EF Core 3,0 `ShippingAddress` bude vlastnost namapována na samostatné sloupce pro `BulkOrder` a `Order` ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-422">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="8b2e3-423">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-423">**New behavior**</span></span>

<span data-ttu-id="8b2e3-424">Počínaje 3,0 se EF Core pro `ShippingAddress`nedá vytvořit jenom jeden sloupec.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-424">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="8b2e3-425">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-425">**Why**</span></span>

<span data-ttu-id="8b2e3-426">Starý behavoir nebyl očekáván.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-426">The old behavoir was unexpected.</span></span>

<span data-ttu-id="8b2e3-427">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-427">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-428">Vlastnost může být stále explicitně namapována na samostatný sloupec odvozených typů:</span><span class="sxs-lookup"><span data-stu-id="8b2e3-428">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="8b2e3-429">Konvence vlastností cizího klíče už neodpovídá stejnému názvu jako vlastnost Principal.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-429">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="8b2e3-430">Sledování problému #13274</span><span class="sxs-lookup"><span data-stu-id="8b2e3-430">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="8b2e3-431">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-431">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-432">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="8b2e3-432">Consider the following model:</span></span>
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
<span data-ttu-id="8b2e3-433">Před EF Core 3,0 `CustomerId` se vlastnost používá pro cizí klíč podle konvence.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-433">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="8b2e3-434">Nicméně pokud `Order` je vlastněný typ, pak by to vedlo `CustomerId` také k tomu, že primární klíč a to není obvykle očekávané.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-434">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="8b2e3-435">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-435">**New behavior**</span></span>

<span data-ttu-id="8b2e3-436">Počínaje 3,0 se EF Core nepokouší použít vlastnosti pro cizí klíče podle konvence, pokud mají stejný název jako vlastnost Principal.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-436">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="8b2e3-437">Název objektu zabezpečení zřetězený s názvem vlastnosti objektu zabezpečení a název navigace zřetězený s vzory názvů hlavních vlastností se pořád shodují.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-437">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="8b2e3-438">Příklad:</span><span class="sxs-lookup"><span data-stu-id="8b2e3-438">For example:</span></span>

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

<span data-ttu-id="8b2e3-439">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-439">**Why**</span></span>

<span data-ttu-id="8b2e3-440">Tato změna byla provedena, aby nedocházelo k chybnému definování vlastností primárního klíče u vlastněných typů.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-440">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="8b2e3-441">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-441">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-442">Pokud by vlastnost měla být cizí klíč, a proto je součástí primárního klíče, pak ji explicitně nakonfigurujte jako takovou.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-442">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="8b2e3-443">Připojení k databázi je teď zavřené, pokud už nepoužíváte dřív, než se dokončí jeho objekt TransactionScope.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-443">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="8b2e3-444">Sledování problému #14218</span><span class="sxs-lookup"><span data-stu-id="8b2e3-444">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="8b2e3-445">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-445">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-446">Pokud kontext otevře během EF Core 3,0 připojení uvnitř `TransactionScope`, zůstane připojení otevřené, zatímco aktuální `TransactionScope` aktivní je.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-446">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="8b2e3-447">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-447">**New behavior**</span></span>

<span data-ttu-id="8b2e3-448">Od 3,0 EF Core ukončí připojení, jakmile ho dokončí jeho používání.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-448">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="8b2e3-449">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-449">**Why**</span></span>

<span data-ttu-id="8b2e3-450">Tato změna umožňuje použít více kontextů současně `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-450">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="8b2e3-451">Nové chování se také shoduje s EF6.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-451">The new behavior also matches EF6.</span></span>

<span data-ttu-id="8b2e3-452">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-452">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-453">Pokud připojení potřebuje zůstat otevřeným explicitním voláním `OpenConnection()` , zajistí, že EF Core neuzavře předčasně:</span><span class="sxs-lookup"><span data-stu-id="8b2e3-453">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="8b2e3-454">Každá vlastnost používá nezávislou generaci celočíselného klíče v paměti.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-454">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="8b2e3-455">Sledování problému #6872</span><span class="sxs-lookup"><span data-stu-id="8b2e3-455">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="8b2e3-456">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-456">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-457">Před EF Core 3,0 byl pro všechny vlastnosti celého čísla klíče v paměti použit jeden generátor sdílených hodnot.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-457">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="8b2e3-458">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-458">**New behavior**</span></span>

<span data-ttu-id="8b2e3-459">Počínaje EF Core 3,0 každé klíčové vlastnosti celého čísla získá vlastní generátor hodnot při použití databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-459">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="8b2e3-460">Také pokud je databáze odstraněna, generování klíče se obnoví pro všechny tabulky.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-460">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="8b2e3-461">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-461">**Why**</span></span>

<span data-ttu-id="8b2e3-462">Tato změna byla provedená tak, aby se při vytváření klíčů v paměti lépe rovnala generování klíčů v paměti a vylepšila se možnost izolovat testy od sebe při použití databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-462">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="8b2e3-463">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-463">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-464">To může poškodit aplikaci, která se spoléhá na konkrétní hodnoty klíče v paměti, které se mají nastavit.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-464">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="8b2e3-465">Místo toho se nemusíte spoléhat na konkrétní hodnoty klíčů nebo aktualizovat tak, aby odpovídaly novému chování.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-465">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="8b2e3-466">Ve výchozím nastavení se používají pole pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-466">Backing fields are used by default</span></span>

[<span data-ttu-id="8b2e3-467">Sledování problému #12430</span><span class="sxs-lookup"><span data-stu-id="8b2e3-467">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="8b2e3-468">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-468">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-469">Před 3,0, a to i v případě, že je známé pole zálohování pro vlastnost, EF Core ve výchozím nastavení přečetl a zapsat hodnotu vlastnosti pomocí metod getter a setter vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-469">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="8b2e3-470">Výjimkou je provedení dotazu, kde by se pole zálohování nastavilo přímo, pokud je známé.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-470">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="8b2e3-471">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-471">**New behavior**</span></span>

<span data-ttu-id="8b2e3-472">Počínaje EF Core 3,0 platí, že pokud je známé pole zálohování pro vlastnost, pak EF Core tuto vlastnost vždy přečte a zapíše pomocí pole pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-472">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="8b2e3-473">To může způsobit přerušení aplikace, pokud se aplikace spoléhá na další chování kódované v metodách getter nebo setter.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-473">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="8b2e3-474">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-474">**Why**</span></span>

<span data-ttu-id="8b2e3-475">Tato změna byla provedena EF Core proto, aby při provádění databázových operací, které obsahují entity, ve výchozím nastavení nechybně aktivovala obchodní logiku.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-475">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="8b2e3-476">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-476">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-477">Chování před 3,0 se dá obnovit pomocí konfigurace režimu přístupu vlastnosti zapnuto `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-477">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="8b2e3-478">Příklad:</span><span class="sxs-lookup"><span data-stu-id="8b2e3-478">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="8b2e3-479">Vyvolat, zda je nalezeno více kompatibilních zálohovaných polí</span><span class="sxs-lookup"><span data-stu-id="8b2e3-479">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="8b2e3-480">Sledování problému #12523</span><span class="sxs-lookup"><span data-stu-id="8b2e3-480">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="8b2e3-481">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-481">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-482">Pokud se v EF Core 3,0 shodovalo více polí s pravidly pro hledání zálohovacího pole vlastnosti, bude jedno pole zvoleno na základě pořadí priorit.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-482">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="8b2e3-483">To může způsobit, že se nesprávné pole použije v nejednoznačných případech.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-483">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="8b2e3-484">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-484">**New behavior**</span></span>

<span data-ttu-id="8b2e3-485">Počínaje EF Core 3,0 platí, že pokud je více polí spárováno se stejnou vlastností, je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-485">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="8b2e3-486">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-486">**Why**</span></span>

<span data-ttu-id="8b2e3-487">Tato změna byla provedena, aby nedocházelo k tichému použití jednoho pole v případě, že může být pouze jedna z nich správná.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-487">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="8b2e3-488">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-488">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-489">Vlastnosti s nejednoznačnými zálohovacími poli musí obsahovat pole, které se má explicitně použít.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-489">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="8b2e3-490">Například pomocí rozhraní Fluent API:</span><span class="sxs-lookup"><span data-stu-id="8b2e3-490">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="8b2e3-491">Názvy vlastností pouze pro pole se musí shodovat s názvem pole.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-491">Field-only property names should match the field name</span></span>

<span data-ttu-id="8b2e3-492">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-492">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-493">Před EF Core 3,0 může být vlastnost určena hodnotou řetězce a v případě, že v typu .NET nebyla nalezena žádná vlastnost s tímto názvem, EF Core by se pokusila ji porovnat s polem pomocí pravidel konvence.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-493">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the .NET type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="8b2e3-494">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-494">**New behavior**</span></span>

<span data-ttu-id="8b2e3-495">Počínaje EF Core 3,0 se vlastnost pouze pole musí přesně shodovat s názvem pole.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-495">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="8b2e3-496">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-496">**Why**</span></span>

<span data-ttu-id="8b2e3-497">Tato změna byla provedena, aby se nepoužívalo stejné pole pro dvě vlastnosti s názvem podobně, ale také pravidla pro porovnání vlastností pouze pro pole, která jsou shodná s vlastnostmi mapovanými na vlastnosti CLR.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-497">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="8b2e3-498">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-498">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-499">Vlastnosti pouze polí musí být pojmenovány stejně jako pole, na které jsou namapována.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-499">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="8b2e3-500">V budoucí verzi EF Core po 3,0 plánujeme znovu povolit explicitní konfiguraci názvu pole, který se liší od názvu vlastnosti (viz téma věnované problému [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span><span class="sxs-lookup"><span data-stu-id="8b2e3-500">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="8b2e3-501">AddDbContext/AddDbContextPool už nevolá AddLogging a AddMemoryCache.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-501">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="8b2e3-502">Sledování problému #14756</span><span class="sxs-lookup"><span data-stu-id="8b2e3-502">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="8b2e3-503">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-503">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-504">Před EF Core 3,0, volání `AddDbContext` nebo `AddDbContextPool` by také registrovaly služby protokolování a ukládání do mezipaměti v paměti pomocí D. I přes volání [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) a [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="8b2e3-504">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="8b2e3-505">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-505">**New behavior**</span></span>

<span data-ttu-id="8b2e3-506">Počínaje EF Core 3,0 `AddDbContext` a `AddDbContextPool` nadále se nebudou registrovat tyto služby se vkládáním závislostí (di).</span><span class="sxs-lookup"><span data-stu-id="8b2e3-506">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="8b2e3-507">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-507">**Why**</span></span>

<span data-ttu-id="8b2e3-508">EF Core 3,0 nevyžaduje, aby se tyto služby nacházejí v kontejneru aplikace DI.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-508">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="8b2e3-509">Pokud `ILoggerFactory` je však v kontejneru aplikace zaregistrován, bude nadále používána EF Core.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-509">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="8b2e3-510">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-510">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-511">Pokud vaše aplikace potřebuje tyto služby, zaregistrujte je explicitně pomocí kontejneru DI pomocí [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) nebo [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="8b2e3-511">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="8b2e3-512">DbContext. entry teď provádí místní DetectChanges.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-512">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="8b2e3-513">Sledování problému #13552</span><span class="sxs-lookup"><span data-stu-id="8b2e3-513">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="8b2e3-514">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-514">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-515">Před EF Core 3,0 by volání `DbContext.Entry` způsobilo zjištění změn všech sledovaných entit.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-515">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="8b2e3-516">Tím je zajištěno, že stav zpřístupněno v `EntityEntry` nástroji byl aktuální.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-516">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="8b2e3-517">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-517">**New behavior**</span></span>

<span data-ttu-id="8b2e3-518">Počínaje EF Core 3,0 se nyní volání `DbContext.Entry` pokusí zjistit změny v dané entitě a všechny sledované hlavní entity, které se k ní vztahují.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-518">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="8b2e3-519">To znamená, že změny jinde nemohly být zjištěny voláním této metody, což by mohlo mít vliv na stav aplikace.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-519">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="8b2e3-520">Všimněte si, `ChangeTracker.AutoDetectChangesEnabled` že pokud je `false` nastaveno na, tak i toto místní zjišťování změn bude zakázáno.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-520">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="8b2e3-521">Jiné metody, které způsobují detekci změn – například `ChangeTracker.Entries` a `SaveChanges`---stále zapříčiní plné `DetectChanges` ze všech sledovaných entit.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-521">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="8b2e3-522">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-522">**Why**</span></span>

<span data-ttu-id="8b2e3-523">Tato změna byla provedena za účelem zlepšení výchozího výkonu použití `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-523">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="8b2e3-524">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-524">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-525">Před `ChgangeTracker.DetectChanges()` voláním `Entry` zajistěte explicitní volání, aby bylo zajištěno chování před 3,0.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-525">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="8b2e3-526">Klíče řetězce a pole bajtů nejsou ve výchozím nastavení generovány klientem.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-526">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="8b2e3-527">Sledování problému #14617</span><span class="sxs-lookup"><span data-stu-id="8b2e3-527">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="8b2e3-528">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-528">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-529">Před EF Core 3,0 `string` a `byte[]` vlastnosti klíče lze použít bez explicitního nastavení hodnoty, která není null.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-529">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="8b2e3-530">V takovém případě se hodnota klíče vygeneruje na klientovi jako identifikátor GUID, který je serializovaný na bajty pro `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-530">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="8b2e3-531">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-531">**New behavior**</span></span>

<span data-ttu-id="8b2e3-532">Počínaje EF Core 3,0 bude vyvolána výjimka oznamující, že nebyla nastavena žádná hodnota klíče.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-532">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="8b2e3-533">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-533">**Why**</span></span>

<span data-ttu-id="8b2e3-534">Tato změna byla provedena, protože obecně nejsou `string` užitečné hodnoty generované / `byte[]` klientem a výchozí chování způsobilo, že je obtížné vygenerovat hodnoty klíčů běžným způsobem.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-534">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="8b2e3-535">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-535">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-536">Chování před 3,0 lze získat explicitním určením, že klíčové vlastnosti by měly používat generované hodnoty, pokud není nastavena žádná jiná hodnota, která není null.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-536">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="8b2e3-537">Například s rozhraním API Fluent:</span><span class="sxs-lookup"><span data-stu-id="8b2e3-537">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="8b2e3-538">Nebo s poznámkami k datům:</span><span class="sxs-lookup"><span data-stu-id="8b2e3-538">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="8b2e3-539">ILoggerFactory je teď služba s vymezeným oborem.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-539">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="8b2e3-540">Sledování problému #14698</span><span class="sxs-lookup"><span data-stu-id="8b2e3-540">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="8b2e3-541">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-541">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-542">Před EF Core 3,0 `ILoggerFactory` byl zaregistrován jako služba typu singleton.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-542">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="8b2e3-543">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-543">**New behavior**</span></span>

<span data-ttu-id="8b2e3-544">Počínaje EF Core 3,0 `ILoggerFactory` je nyní registrováno jako obor.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-544">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="8b2e3-545">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-545">**Why**</span></span>

<span data-ttu-id="8b2e3-546">Tato změna byla provedena tak, aby povolovala přidružení protokolovacího `DbContext` nástroje s instancí, která umožňuje další funkce a odebírá některé případy patologického chování, jako je například rozpad interních poskytovatelů služeb.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-546">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="8b2e3-547">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-547">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-548">Tato změna by neměla mít vliv na kód aplikace, pokud se neregistruje a nepoužívá vlastní služby pro EF Core interního poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-548">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="8b2e3-549">To není běžné.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-549">This isn't common.</span></span>
<span data-ttu-id="8b2e3-550">V těchto případech bude většina věcí pořád fungovat, ale jakákoli služba typu Singleton, která byla v závislosti `ILoggerFactory` na tom, bude muset změnit tak, `ILoggerFactory` aby získala jiný způsob.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-550">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="8b2e3-551">Pokud narazíte na takové situace, zajistěte prosím problém na [EF Core modul pro sledování problémů GitHubu](https://github.com/aspnet/EntityFrameworkCore/issues) , abychom nás věděli, jak `ILoggerFactory` ho používáte, abychom mohli lépe pochopit, jak to v budoucnu Nerušit.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-551">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="8b2e3-552">Opožděné načítání proxy serverů už nepředpokládá navigační vlastnosti, jsou plně načtené.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-552">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="8b2e3-553">Sledování problému #12780</span><span class="sxs-lookup"><span data-stu-id="8b2e3-553">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="8b2e3-554">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-554">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-555">Předtím, než EF Core 3,0, `DbContext` neexistuje žádný způsob, jak zjistit, zda byla daná vlastnost navigace u entity získané z tohoto kontextu plně načtena nebo ne.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-555">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="8b2e3-556">Proxy místo toho předpokládají, že je načtena odkazová navigace, pokud má hodnotu jinou než null a že je načtena navigace kolekce, pokud není prázdná.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-556">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="8b2e3-557">V těchto případech by byl pokus o opožděné načtení no-op.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-557">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="8b2e3-558">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-558">**New behavior**</span></span>

<span data-ttu-id="8b2e3-559">Počínaje EF Core 3,0 budou proxy servery sledovat, zda je načtena vlastnost navigace.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-559">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="8b2e3-560">To znamená, že při pokusu o přístup k navigační vlastnosti, která je načtena po vyřazení kontextu, bude vždy ta no-op, i když je načtená navigace prázdná nebo má hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-560">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="8b2e3-561">Naopak při pokusu o přístup k navigační vlastnosti, která není načtená, vyvolá výjimku, pokud je kontext vyřazený, i když je vlastnost navigace neprázdná kolekce.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-561">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="8b2e3-562">Pokud nastane tato situace, znamená to, že se kód aplikace pokouší použít opožděné načítání v neplatném čase a aplikace by se měla změnit, aby to nevedlo.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-562">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="8b2e3-563">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-563">**Why**</span></span>

<span data-ttu-id="8b2e3-564">Tato změna byla provedena, aby při pokusu o opožděné načtení na uvolněnou `DbContext` instanci bylo chování konzistentní a správné.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-564">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="8b2e3-565">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-565">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-566">Aktualizujte kód aplikace, aby se nepokoušel opožděné načtení s odstraněným kontextem, nebo nastavte tuto hodnotu jako No-op, jak je popsáno ve zprávě výjimky.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-566">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="8b2e3-567">Nadměrné vytváření interních zprostředkovatelů služeb je teď ve výchozím nastavení chyba.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-567">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="8b2e3-568">Sledování problému #10236</span><span class="sxs-lookup"><span data-stu-id="8b2e3-568">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="8b2e3-569">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-569">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-570">Před EF Core 3,0 se pro aplikaci, která vytváří patologický počet interních poskytovatelů služeb, zaprotokoluje upozornění.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-570">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="8b2e3-571">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-571">**New behavior**</span></span>

<span data-ttu-id="8b2e3-572">Počínaje EF Core 3,0 je toto upozornění nyní považováno za chybu a je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-572">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="8b2e3-573">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-573">**Why**</span></span>

<span data-ttu-id="8b2e3-574">Tato změna byla provedená tak, že se tento patologický případ výslovně zveřejňuje tak, aby se zlepšil kód aplikace.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-574">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="8b2e3-575">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-575">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-576">Nejvhodnější příčinou této chyby je pochopení hlavní příčiny a zastavení vytváření, takže mnoho interních poskytovatelů služeb.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-576">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="8b2e3-577">Chybu však lze převést zpět na varování (nebo ignorováno) prostřednictvím konfigurace na `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-577">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="8b2e3-578">Příklad:</span><span class="sxs-lookup"><span data-stu-id="8b2e3-578">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="8b2e3-579">Nové chování pro HasOne/HasMany se volá s jedním řetězcem.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-579">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="8b2e3-580">Sledování problému #9171</span><span class="sxs-lookup"><span data-stu-id="8b2e3-580">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="8b2e3-581">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-581">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-582">Před EF Core 3,0, volání `HasOne` kódu nebo `HasMany` s jedním řetězcem bylo interpretováno matoucím způsobem.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-582">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="8b2e3-583">Příklad:</span><span class="sxs-lookup"><span data-stu-id="8b2e3-583">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="8b2e3-584">Kód vypadá jako v `Samurai` `Entrance` souvislosti s jiným typem entity pomocí navigační vlastnosti, která může být soukromá.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-584">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="8b2e3-585">Ve skutečnosti se tento kód pokusí vytvořit relaci k některému typu entity s názvem `Entrance` bez navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-585">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="8b2e3-586">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-586">**New behavior**</span></span>

<span data-ttu-id="8b2e3-587">Počínaje EF Core 3,0 výše uvedený kód má nyní podobný vzhled jako předtím.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-587">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="8b2e3-588">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-588">**Why**</span></span>

<span data-ttu-id="8b2e3-589">Staré chování bylo velmi matoucí, zejména při čtení konfiguračního kódu a hledání chyb.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-589">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="8b2e3-590">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-590">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-591">Tím dojde pouze k přerušení aplikací, které jsou explicitně konfigurovány pomocí řetězců pro názvy typů, a bez explicitního určení vlastnosti navigace.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-591">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="8b2e3-592">To není běžné.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-592">This is not common.</span></span>
<span data-ttu-id="8b2e3-593">Předchozí chování lze získat pomocí explicitního předání `null` názvu vlastnosti navigace.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-593">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="8b2e3-594">Příklad:</span><span class="sxs-lookup"><span data-stu-id="8b2e3-594">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="8b2e3-595">Návratový typ pro několik asynchronních metod byl změněn z úlohy na ValueTask</span><span class="sxs-lookup"><span data-stu-id="8b2e3-595">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="8b2e3-596">Sledování problému #15184</span><span class="sxs-lookup"><span data-stu-id="8b2e3-596">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="8b2e3-597">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-597">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-598">Následující asynchronní metody dříve vrátily `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="8b2e3-598">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="8b2e3-599">`ValueGenerator.NextValueAsync()`(a odvozování tříd)</span><span class="sxs-lookup"><span data-stu-id="8b2e3-599">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="8b2e3-600">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-600">**New behavior**</span></span>

<span data-ttu-id="8b2e3-601">Výše uvedené metody nyní vrací stejnou `ValueTask<T>` `T` hodnotu jako předtím.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-601">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="8b2e3-602">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-602">**Why**</span></span>

<span data-ttu-id="8b2e3-603">Tato změna snižuje počet přidělení haldy, které vznikly při vyvolání těchto metod, což zlepšuje obecný výkon.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-603">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="8b2e3-604">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-604">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-605">Aplikace jednoduše čekají na rozhraní API, které je třeba znovu zkompilovat – nejsou nutné žádné změny zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-605">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="8b2e3-606">Složitější využití (například předání vráceného `Task` do `Task.WhenAny()`) obvykle vyžaduje, aby bylo vráceno `ValueTask<T>` převedené na a `Task<T>` voláním `AsTask()` .</span><span class="sxs-lookup"><span data-stu-id="8b2e3-606">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="8b2e3-607">Všimněte si, že se tím sníží omezení přidělení, které tato změna přináší.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-607">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="8b2e3-608">Relační: anotace TypeMapping je nyní pouze TypeMapping</span><span class="sxs-lookup"><span data-stu-id="8b2e3-608">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="8b2e3-609">Sledování problému #9913</span><span class="sxs-lookup"><span data-stu-id="8b2e3-609">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="8b2e3-610">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-610">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-611">Název poznámky pro poznámky k mapování typů byl "relační: TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="8b2e3-611">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="8b2e3-612">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-612">**New behavior**</span></span>

<span data-ttu-id="8b2e3-613">Název poznámky pro mapování typů je nyní "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="8b2e3-613">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="8b2e3-614">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-614">**Why**</span></span>

<span data-ttu-id="8b2e3-615">Mapování typů se nyní používají pro více než stejné poskytovatele relačních databází.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-615">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="8b2e3-616">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-616">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-617">Tím dojde pouze k přerušení aplikací, které přistupují k mapování typu přímo jako anotaci, což není běžné.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-617">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="8b2e3-618">Nejvhodnější akcí pro opravu je použití prostoru rozhraní API pro přístup k mapování typů namísto použití anotace přímo.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-618">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="8b2e3-619">ToTable na odvozeném typu vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-619">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="8b2e3-620">Sledování problému #11811</span><span class="sxs-lookup"><span data-stu-id="8b2e3-620">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="8b2e3-621">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-621">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-622">Před EF Core 3,0, `ToTable()` který je volán na odvozeném typu, by byl ignorován, protože pouze dědění mapování dědičnosti bylo typu TPH, který není platný.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-622">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="8b2e3-623">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-623">**New behavior**</span></span>

<span data-ttu-id="8b2e3-624">Počínaje EF Core 3,0 a přípravou pro přidání podpory TPT a TPC v novější verzi, která se `ToTable()` volá na odvozeném typu, teď vyvolá výjimku, aby se předešlo neočekávané změně mapování v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-624">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="8b2e3-625">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-625">**Why**</span></span>

<span data-ttu-id="8b2e3-626">V současné době není platný pro mapování odvozeného typu na jinou tabulku.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-626">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="8b2e3-627">Tato změna zabrání v budoucnosti v budoucnu, pokud se to stalo platným.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-627">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="8b2e3-628">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-628">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-629">Odeberte všechny pokusy o mapování odvozených typů na jiné tabulky.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-629">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="8b2e3-630">ForSqlServerHasIndex nahrazeno HasIndex</span><span class="sxs-lookup"><span data-stu-id="8b2e3-630">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="8b2e3-631">Sledování problému #12366</span><span class="sxs-lookup"><span data-stu-id="8b2e3-631">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="8b2e3-632">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-632">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-633">Před EF Core 3,0 jste `ForSqlServerHasIndex().ForSqlServerInclude()` získali způsob, jak nakonfigurovat sloupce používané pomocí `INCLUDE`nástroje.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-633">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="8b2e3-634">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-634">**New behavior**</span></span>

<span data-ttu-id="8b2e3-635">Počínaje EF Core 3,0 se teď použití `Include` na indexu podporuje na relační úrovni.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-635">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="8b2e3-636">Použijte `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-636">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="8b2e3-637">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-637">**Why**</span></span>

<span data-ttu-id="8b2e3-638">Tato změna byla provedena za účelem konsolidace rozhraní API pro indexy `Include` na jednom místě pro všechny poskytovatele databáze.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-638">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="8b2e3-639">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-639">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-640">Použijte nové rozhraní API, jak vidíte výše.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-640">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="8b2e3-641">Změny rozhraní API pro metadata</span><span class="sxs-lookup"><span data-stu-id="8b2e3-641">Metadata API changes</span></span>

[<span data-ttu-id="8b2e3-642">Sledování problému #214</span><span class="sxs-lookup"><span data-stu-id="8b2e3-642">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="8b2e3-643">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-643">**New behavior**</span></span>

<span data-ttu-id="8b2e3-644">Následující vlastnosti byly převedeny na rozšiřující metody:</span><span class="sxs-lookup"><span data-stu-id="8b2e3-644">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="8b2e3-645">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-645">**Why**</span></span>

<span data-ttu-id="8b2e3-646">Tato změna zjednodušuje implementaci výše uvedených rozhraní.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-646">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="8b2e3-647">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-647">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-648">Použijte nové metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-648">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="8b2e3-649">Změny rozhraní API pro konkrétního zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="8b2e3-649">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="8b2e3-650">Sledování problému #214</span><span class="sxs-lookup"><span data-stu-id="8b2e3-650">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="8b2e3-651">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-651">**New behavior**</span></span>

<span data-ttu-id="8b2e3-652">Metody rozšíření specifické pro poskytovatele budou shrnuty:</span><span class="sxs-lookup"><span data-stu-id="8b2e3-652">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="8b2e3-653">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-653">**Why**</span></span>

<span data-ttu-id="8b2e3-654">Tato změna zjednodušuje implementaci výše uvedených rozšiřujících metod.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-654">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="8b2e3-655">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-655">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-656">Použijte nové metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-656">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="8b2e3-657">EF Core už neposílá direktivu pragma pro vynucení KOFK SQLite.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-657">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="8b2e3-658">Sledování problému #12151</span><span class="sxs-lookup"><span data-stu-id="8b2e3-658">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="8b2e3-659">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-659">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-660">Před EF Core 3,0 EF Core odeslat `PRAGMA foreign_keys = 1` při otevření připojení k sqlite.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-660">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="8b2e3-661">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-661">**New behavior**</span></span>

<span data-ttu-id="8b2e3-662">Počínaje EF Core 3,0 EF Core již neposílá `PRAGMA foreign_keys = 1` při otevření připojení k sqlite.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-662">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="8b2e3-663">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-663">**Why**</span></span>

<span data-ttu-id="8b2e3-664">Tato změna byla provedena, protože EF Core `SQLitePCLRaw.bundle_e_sqlite3` používá ve výchozím nastavení, což zase znamená, že je ve výchozím nastavení zapnuté vynucení CK a není nutné je explicitně povolit při každém otevření připojení.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-664">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="8b2e3-665">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-665">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-666">Cizí klíče jsou ve výchozím nastavení povolené v SQLitePCLRaw. bundle_e_sqlite3, který se ve výchozím nastavení používá pro EF Core.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-666">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="8b2e3-667">V ostatních případech je možné povolit cizí klíče zadáním `Foreign Keys=True` v připojovacím řetězci.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-667">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="8b2e3-668">Microsoft. EntityFrameworkCore. sqlite teď závisí na SQLitePCLRaw. bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="8b2e3-668">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="8b2e3-669">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-669">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-670">Před EF Core 3,0 se používá `SQLitePCLRaw.bundle_green`EF Core.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-670">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="8b2e3-671">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-671">**New behavior**</span></span>

<span data-ttu-id="8b2e3-672">Počínaje EF Core 3,0 EF Core používá `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-672">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="8b2e3-673">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-673">**Why**</span></span>

<span data-ttu-id="8b2e3-674">Tato změna byla provedena tak, že verze SQLiteu použitá v iOS je konzistentní s jinými platformami.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-674">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="8b2e3-675">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-675">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-676">Pokud chcete použít nativní verzi SQLite v iOS, nakonfigurujte `Microsoft.Data.Sqlite` , aby používala `SQLitePCLRaw` jiný svazek.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-676">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="8b2e3-677">Hodnoty GUID se teď ukládají jako TEXT na SQLite.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-677">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="8b2e3-678">Sledování problému #15078</span><span class="sxs-lookup"><span data-stu-id="8b2e3-678">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="8b2e3-679">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-679">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-680">Hodnoty GUID byly dříve uloženy jako hodnoty objektů BLOB u SQLite.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-680">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="8b2e3-681">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-681">**New behavior**</span></span>

<span data-ttu-id="8b2e3-682">Hodnoty GUID jsou nyní uloženy jako TEXT.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-682">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="8b2e3-683">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-683">**Why**</span></span>

<span data-ttu-id="8b2e3-684">Binární formát identifikátorů GUID není standardizovaný.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-684">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="8b2e3-685">Uložení hodnot jako textu zajistí, že databáze bude lépe kompatibilní s jinými technologiemi.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-685">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="8b2e3-686">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-686">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-687">Existující databáze můžete migrovat do nového formátu tím, že spustíte příkaz SQL podobně jako následující.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-687">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="8b2e3-688">V EF Core můžete i nadále používat předchozí chování nakonfigurováním převaděče hodnot na těchto vlastnostech.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-688">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="8b2e3-689">Microsoft. data. sqlite zůstává schopný přečítat hodnoty GUID z objektů BLOB a textových sloupců. vzhledem k tomu, že výchozí formát pro parametry a konstanty se změnil, bude pravděpodobně nutné provést akci u většiny scénářů, které zahrnují identifikátory GUID.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-689">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="8b2e3-690">Hodnoty char se teď ukládají jako TEXT na SQLite.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-690">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="8b2e3-691">Sledování problému #15020</span><span class="sxs-lookup"><span data-stu-id="8b2e3-691">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="8b2e3-692">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-692">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-693">Hodnoty typu char byly dříve sored jako CELOČÍSELNé hodnoty u SQLite.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-693">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="8b2e3-694">Například hodnota znaku *a* byla uložena jako celočíselná hodnota 65.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-694">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="8b2e3-695">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-695">**New behavior**</span></span>

<span data-ttu-id="8b2e3-696">Hodnoty typu char jsou nyní uloženy jako TEXT.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-696">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="8b2e3-697">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-697">**Why**</span></span>

<span data-ttu-id="8b2e3-698">Ukládání hodnot jako TEXT je přirozenější a databáze usnadňuje kompatibilitu s jinými technologiemi.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-698">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="8b2e3-699">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-699">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-700">Existující databáze můžete migrovat do nového formátu tím, že spustíte příkaz SQL podobně jako následující.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-700">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="8b2e3-701">V EF Core můžete i nadále používat předchozí chování nakonfigurováním převaděče hodnot na těchto vlastnostech.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-701">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="8b2e3-702">Microsoft. data. sqlite také zůstává schopný číst znakové hodnoty z CELOČÍSELNého i TEXTOVÉHO sloupce, takže některé scénáře nemusí vyžadovat žádnou akci.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-702">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="8b2e3-703">ID migrace se teď generují pomocí kalendáře invariantní jazykové verze.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-703">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="8b2e3-704">Sledování problému #12978</span><span class="sxs-lookup"><span data-stu-id="8b2e3-704">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="8b2e3-705">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-705">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-706">ID migrace se nechtěně vygenerovala pomocí kalendáře aktuální jazykové verze.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-706">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="8b2e3-707">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-707">**New behavior**</span></span>

<span data-ttu-id="8b2e3-708">ID migrace se nyní vždy generují pomocí kalendáře neutrální jazykové verze (gregoriánský).</span><span class="sxs-lookup"><span data-stu-id="8b2e3-708">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="8b2e3-709">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-709">**Why**</span></span>

<span data-ttu-id="8b2e3-710">Pořadí migrace je důležité při aktualizaci databáze nebo řešení konfliktů při slučování.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-710">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="8b2e3-711">Pomocí invariantního kalendáře se vyhnete problémům s řazením, které mohou být výsledkem členů týmu jiné systémové kalendáře.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-711">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="8b2e3-712">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-712">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-713">Tato změna má vliv na kohokoli, kdo používá negregoriánský kalendář, ve kterém je rok větší než gregoriánský kalendář (například thajský buddhistický kalendář).</span><span class="sxs-lookup"><span data-stu-id="8b2e3-713">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="8b2e3-714">Existující identifikátory migrace se budou muset aktualizovat, aby se nové migrace objednaly po stávajících migracích.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-714">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="8b2e3-715">ID migrace najdete v atributu migrace v souborech návrháře migrace.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-715">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="8b2e3-716">Také je potřeba aktualizovat tabulku historie migrace.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-716">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="8b2e3-717">UseRowNumberForPaging se odebral.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-717">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="8b2e3-718">Sledování problému #16400</span><span class="sxs-lookup"><span data-stu-id="8b2e3-718">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="8b2e3-719">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-719">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-720">Před EF Core 3,0 `UseRowNumberForPaging` lze použít k vygenerování SQL pro stránkování, které je kompatibilní s SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-720">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="8b2e3-721">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-721">**New behavior**</span></span>

<span data-ttu-id="8b2e3-722">Počínaje EF Core 3,0 bude EF generovat pouze SQL pro stránkování, které je kompatibilní pouze s novějšími verzemi SQL Server.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-722">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="8b2e3-723">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-723">**Why**</span></span>

<span data-ttu-id="8b2e3-724">Tuto změnu provedeme, protože [SQL Server 2008 už není podporovaným produktem](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) a aktualizace této funkce tak, aby fungovala se změnami dotazů provedenými v EF Core 3,0 je významná práce.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-724">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="8b2e3-725">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-725">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-726">Doporučujeme aktualizovat na novější verzi SQL Server nebo pomocí vyšší úrovně kompatibility, aby byl vygenerovaný SQL podporován.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-726">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="8b2e3-727">To znamená, že pokud to nemůžete udělat, [komentář k problému s sledováním](https://github.com/aspnet/EntityFrameworkCore/issues/16400) najdete v podrobnostech.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-727">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="8b2e3-728">Toto rozhodnutí můžeme znovu navštívit na základě zpětné vazby.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-728">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="8b2e3-729">Informace o rozšíření/metadata se odebraly z IDbContextOptionsExtension.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-729">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="8b2e3-730">Sledování problému #16119</span><span class="sxs-lookup"><span data-stu-id="8b2e3-730">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="8b2e3-731">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-731">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-732">`IDbContextOptionsExtension`obsažené metody pro poskytování metadat o rozšíření.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-732">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="8b2e3-733">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-733">**New behavior**</span></span>

<span data-ttu-id="8b2e3-734">Tyto metody byly přesunuty na novou `DbContextOptionsExtensionInfo` abstraktní základní třídu, která je vrácena z nové `IDbContextOptionsExtension.Info` vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-734">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="8b2e3-735">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-735">**Why**</span></span>

<span data-ttu-id="8b2e3-736">V rámci vydání od 2,0 do 3,0 jsme potřebovali přidat nebo změnit tyto metody několikrát.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-736">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="8b2e3-737">Rozbalením do nové abstraktní základní třídy bude snazší vytvořit tyto změny bez přerušení stávajících rozšíření.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-737">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="8b2e3-738">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-738">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-739">Aktualizovat rozšíření tak, aby následovala nový vzor.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-739">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="8b2e3-740">Příklady naleznete v mnoha implementacích `IDbContextOptionsExtension` pro různé druhy rozšíření ve zdrojovém kódu EF Core.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-740">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="8b2e3-741">LogQueryPossibleExceptionWithAggregateOperator se přejmenovalo.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-741">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="8b2e3-742">Sledování problému #10985</span><span class="sxs-lookup"><span data-stu-id="8b2e3-742">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="8b2e3-743">**Mění**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-743">**Change**</span></span>

<span data-ttu-id="8b2e3-744">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator`byla přejmenována `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`na.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-744">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="8b2e3-745">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-745">**Why**</span></span>

<span data-ttu-id="8b2e3-746">Zarovná pojmenování této události varování se všemi ostatními událostmi upozornění.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-746">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="8b2e3-747">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-747">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-748">Použijte nový název.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-748">Use the new name.</span></span> <span data-ttu-id="8b2e3-749">(Všimněte si, že číslo ID události se nezměnilo.)</span><span class="sxs-lookup"><span data-stu-id="8b2e3-749">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="8b2e3-750">Vysvětlení rozhraní API pro názvy omezení cizího klíče</span><span class="sxs-lookup"><span data-stu-id="8b2e3-750">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="8b2e3-751">Sledování problému #10730</span><span class="sxs-lookup"><span data-stu-id="8b2e3-751">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="8b2e3-752">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-752">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-753">Před EF Core 3,0 se názvy omezení cizího klíče odkazovaly jenom na "název".</span><span class="sxs-lookup"><span data-stu-id="8b2e3-753">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="8b2e3-754">Příklad:</span><span class="sxs-lookup"><span data-stu-id="8b2e3-754">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="8b2e3-755">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-755">**New behavior**</span></span>

<span data-ttu-id="8b2e3-756">Počínaje EF Core 3,0 se názvy omezení cizích klíčů teď označují jako "název omezení".</span><span class="sxs-lookup"><span data-stu-id="8b2e3-756">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="8b2e3-757">Příklad:</span><span class="sxs-lookup"><span data-stu-id="8b2e3-757">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="8b2e3-758">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-758">**Why**</span></span>

<span data-ttu-id="8b2e3-759">Tato změna přináší konzistenci pro pojmenování v této oblasti a také vysvětluje, že se jedná o název omezení cizího klíče, a nikoli název sloupce nebo vlastnosti, ve kterém je definován cizí klíč.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-759">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="8b2e3-760">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-760">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-761">Použijte nový název.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-761">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="8b2e3-762">IRelationalDatabaseCreator. HasTables/HasTablesAsync byly zveřejněny.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-762">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="8b2e3-763">Sledování problému #15997</span><span class="sxs-lookup"><span data-stu-id="8b2e3-763">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="8b2e3-764">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-764">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-765">Před EF Core 3,0 byly tyto metody chráněné.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-765">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="8b2e3-766">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-766">**New behavior**</span></span>

<span data-ttu-id="8b2e3-767">Počínaje EF Core 3,0 jsou tyto metody veřejné.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-767">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="8b2e3-768">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-768">**Why**</span></span>

<span data-ttu-id="8b2e3-769">Tyto metody jsou používány EF k určení, jestli je databáze vytvořená, ale prázdná.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-769">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="8b2e3-770">To může být užitečné taky od vnějšího EF při určování, jestli se mají migrace použít.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-770">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="8b2e3-771">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-771">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-772">Změňte přístupnost všech přepsání.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-772">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="8b2e3-773">Microsoft. EntityFrameworkCore. Design je teď balíček DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-773">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="8b2e3-774">Sledování problému #11506</span><span class="sxs-lookup"><span data-stu-id="8b2e3-774">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="8b2e3-775">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-775">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-776">Před EF Core 3,0 byl Microsoft. EntityFrameworkCore. Design regulárním balíčkem NuGet, na kterém by mohly být na sestavení odkazovány projekty, které na něm závisejí.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-776">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="8b2e3-777">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-777">**New behavior**</span></span>

<span data-ttu-id="8b2e3-778">Počínaje EF Core 3,0 se jedná o balíček DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-778">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="8b2e3-779">To znamená, že závislost nebude nijak přesměrovat do jiných projektů a že již nemůžete ve výchozím nastavení odkazovat na své sestavení.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-779">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="8b2e3-780">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-780">**Why**</span></span>

<span data-ttu-id="8b2e3-781">Tento balíček se má použít jenom v době návrhu.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-781">This package is only intended to be used at design time.</span></span> <span data-ttu-id="8b2e3-782">Nasazené aplikace by neměli na ni odkazovat.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-782">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="8b2e3-783">Díky tomu, že balíček DevelopmentDependency, toto doporučení posiluje.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-783">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="8b2e3-784">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-784">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-785">Pokud potřebujete odkazovat na tento balíček, aby bylo možné přepsat EF Core chování při návrhu, můžete aktualizovat metadata položky aktualizovat PackageReference v projektu.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-785">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="8b2e3-786">Pokud se na balíček odkazuje přes Microsoft. EntityFrameworkCore. Tools, budete muset do balíčku přidat explicitní PackageReference, aby se změnila jeho metadata.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-786">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="8b2e3-787">SQLitePCL. Raw aktualizováno na verzi 2.0.0</span><span class="sxs-lookup"><span data-stu-id="8b2e3-787">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="8b2e3-788">Sledování problému #14824</span><span class="sxs-lookup"><span data-stu-id="8b2e3-788">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="8b2e3-789">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-789">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-790">Microsoft. EntityFrameworkCore. sqlite byl dřív závislý na 1.1.12 verze SQLitePCL. Raw.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-790">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="8b2e3-791">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-791">**New behavior**</span></span>

<span data-ttu-id="8b2e3-792">Náš balíček jsme aktualizovali tak, aby byl závislý na verzi 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-792">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="8b2e3-793">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-793">**Why**</span></span>

<span data-ttu-id="8b2e3-794">2\.0.0 verze SQLitePCL. Raw TARGETS .NET Standard 2,0.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-794">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="8b2e3-795">Dříve cílí na .NET Standard 1,1, které vyžadovaly, aby se při práci vytvořil velký uzávěr přenosných balíčků.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-795">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="8b2e3-796">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-796">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-797">SQLitePCL. Raw 2.0.0 verze obsahuje některé zásadní změny.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-797">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="8b2e3-798">Podrobnosti najdete v [poznámkách k verzi](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) .</span><span class="sxs-lookup"><span data-stu-id="8b2e3-798">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="8b2e3-799">NetTopologySuite aktualizace na verzi 2.0.0</span><span class="sxs-lookup"><span data-stu-id="8b2e3-799">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="8b2e3-800">Sledování problému #14825</span><span class="sxs-lookup"><span data-stu-id="8b2e3-800">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="8b2e3-801">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-801">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-802">Prostorové balíčky byly dříve závislé na 1.15.1 verze NetTopologySuite.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-802">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="8b2e3-803">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-803">**New behavior**</span></span>

<span data-ttu-id="8b2e3-804">Náš balíček jsme aktualizovali tak, aby byl závislý na verzi 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-804">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="8b2e3-805">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-805">**Why**</span></span>

<span data-ttu-id="8b2e3-806">2\.0.0 verze NetTopologySuite má za cíl řešit několik problémů s použitelností, ke kterým se EF Core uživatelé setkali.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-806">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="8b2e3-807">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-807">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-808">NetTopologySuite verze 2.0.0 obsahuje některé průlomové změny.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-808">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="8b2e3-809">Podrobnosti najdete v [poznámkách k verzi](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) .</span><span class="sxs-lookup"><span data-stu-id="8b2e3-809">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="8b2e3-810">Je nutné nakonfigurovat více dvojznačných relací odkazujících na sebe.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-810">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="8b2e3-811">Sledování problému #13573</span><span class="sxs-lookup"><span data-stu-id="8b2e3-811">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="8b2e3-812">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-812">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-813">Typ entity s více jednosměrnou navigační vlastností a s vyhovující FKs byl nesprávně nakonfigurován jako jeden vztah.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-813">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="8b2e3-814">Příklad:</span><span class="sxs-lookup"><span data-stu-id="8b2e3-814">For example:</span></span>

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

<span data-ttu-id="8b2e3-815">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-815">**New behavior**</span></span>

<span data-ttu-id="8b2e3-816">Tento scénář je nyní zjištěn v sestavování modelu a je vyvolána výjimka označující, že je model dvojznačný.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-816">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="8b2e3-817">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-817">**Why**</span></span>

<span data-ttu-id="8b2e3-818">Výsledný model byl nejednoznačný a pravděpodobně bude pro tento případ obvykle špatný.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-818">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="8b2e3-819">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-819">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-820">Použijte úplnou konfiguraci relace.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-820">Use full configuration of the relationship.</span></span> <span data-ttu-id="8b2e3-821">Příklad:</span><span class="sxs-lookup"><span data-stu-id="8b2e3-821">For example:</span></span>

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

<a name="udf-empty-string"></a>
### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a><span data-ttu-id="8b2e3-822">DbFunction. Schema má hodnotu null nebo je prázdný řetězec, který nakonfiguruje, aby byl ve výchozím schématu modelu.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-822">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>

[<span data-ttu-id="8b2e3-823">Sledování problému #12757</span><span class="sxs-lookup"><span data-stu-id="8b2e3-823">Tracking Issue #12757</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12757)

<span data-ttu-id="8b2e3-824">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-824">**Old behavior**</span></span>

<span data-ttu-id="8b2e3-825">DbFunction nakonfigurovaný se schématem jako prázdný řetězec byl považován za vestavěnou funkci bez schématu.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-825">A DbFunction configured with schema as an empty string was treated as built-in function without a schema.</span></span> <span data-ttu-id="8b2e3-826">Například následující kód bude mapovat `DatePart` funkci CLR na `DATEPART` vestavěnou funkci na SQLServer.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-826">For example following code will map `DatePart` CLR function to `DATEPART` built-in function on SqlServer.</span></span>

```C#
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

<span data-ttu-id="8b2e3-827">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-827">**New behavior**</span></span>

<span data-ttu-id="8b2e3-828">Všechna mapování DbFunction se považují za namapovaná na uživatelsky definované funkce.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-828">All DbFunction mappings are considered to be mapped to user defined functions.</span></span> <span data-ttu-id="8b2e3-829">Proto je prázdná hodnota řetězce vložena do výchozího schématu pro model.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-829">Hence empty string value would put the function inside the default schema for the model.</span></span> <span data-ttu-id="8b2e3-830">To může být schéma, které je explicitně nakonfigurované `modelBuilder.HasDefaultSchema()` přes `dbo` rozhraní Fluent API, nebo jinak.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-830">Which could be the schema configured explicitly via fluent API `modelBuilder.HasDefaultSchema()` or `dbo` otherwise.</span></span>

<span data-ttu-id="8b2e3-831">**Proč**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-831">**Why**</span></span>

<span data-ttu-id="8b2e3-832">Dříve prázdné schéma bylo způsobem, jak se zacházet s touto funkcí, ale tato funkce je k dispozici pouze pro SqlServer, kde předdefinované funkce nepatří do žádného schématu.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-832">Previously schema being empty was a way to treat that function is built-in but that logic is only applicable for SqlServer where built-in functions do not belong to any schema.</span></span>

<span data-ttu-id="8b2e3-833">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="8b2e3-833">**Mitigations**</span></span>

<span data-ttu-id="8b2e3-834">Nakonfigurujte převod DbFunction ručně, abyste ho namapovali na vestavěnou funkci.</span><span class="sxs-lookup"><span data-stu-id="8b2e3-834">Configure DbFunction's translation manually to map it to a built-in function.</span></span>

```C#
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```
