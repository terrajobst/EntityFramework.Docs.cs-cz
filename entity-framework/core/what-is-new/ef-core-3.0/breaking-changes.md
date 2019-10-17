---
title: Přerušující změny v EF Core 3,0 – EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 690c7828cfe5019f4e7ae904c92430fab4726cb9
ms.sourcegitcommit: 37d0e0fd1703467918665a64837dc54ad2ec7484
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/16/2019
ms.locfileid: "72446012"
---
# <a name="breaking-changes-included-in-ef-core-30"></a><span data-ttu-id="c2eeb-102">Přerušující změny zahrnuté v EF Core 3,0</span><span class="sxs-lookup"><span data-stu-id="c2eeb-102">Breaking changes included in EF Core 3.0</span></span>
<span data-ttu-id="c2eeb-103">Následující změny rozhraní API a chování mají možnost rušit existující aplikace při jejich upgradu na 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-103">The following API and behavior changes have the potential to break existing applications when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="c2eeb-104">Změny, které očekáváme jenom o to, aby ovlivnili pouze poskytovatele databází, jsou popsané v části [změny zprostředkovatele](xref:core/providers/provider-log).</span><span class="sxs-lookup"><span data-stu-id="c2eeb-104">Changes that we expect to only impact database providers are documented under [provider changes](xref:core/providers/provider-log).</span></span>

## <a name="summary"></a><span data-ttu-id="c2eeb-105">Souhrn</span><span class="sxs-lookup"><span data-stu-id="c2eeb-105">Summary</span></span>

| <span data-ttu-id="c2eeb-106">**Zásadní změna**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-106">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="c2eeb-107">**Vliv**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-107">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="c2eeb-108">Dotazy LINQ již nejsou vyhodnocovány na klientovi.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-108">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="c2eeb-109">Maximální</span><span class="sxs-lookup"><span data-stu-id="c2eeb-109">High</span></span>       |
| [<span data-ttu-id="c2eeb-110">EF Core 3,0 cíle .NET Standard 2,1 místo .NET Standard 2,0</span><span class="sxs-lookup"><span data-stu-id="c2eeb-110">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="c2eeb-111">Maximální</span><span class="sxs-lookup"><span data-stu-id="c2eeb-111">High</span></span>      |
| [<span data-ttu-id="c2eeb-112">EF Core nástroj příkazového řádku dotnet EF již není součástí .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="c2eeb-112">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="c2eeb-113">Maximální</span><span class="sxs-lookup"><span data-stu-id="c2eeb-113">High</span></span>      |
| [<span data-ttu-id="c2eeb-114">DetectChanges respektuje hodnoty klíčů generované úložištěm.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-114">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="c2eeb-115">Maximální</span><span class="sxs-lookup"><span data-stu-id="c2eeb-115">High</span></span>      |
| [<span data-ttu-id="c2eeb-116">Z tabulek, ExecuteSql a ExecuteSqlAsync byly přejmenovány.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-116">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="c2eeb-117">Maximální</span><span class="sxs-lookup"><span data-stu-id="c2eeb-117">High</span></span>      |
| [<span data-ttu-id="c2eeb-118">Typy dotazů jsou konsolidovány s typy entit</span><span class="sxs-lookup"><span data-stu-id="c2eeb-118">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="c2eeb-119">Maximální</span><span class="sxs-lookup"><span data-stu-id="c2eeb-119">High</span></span>      |
| [<span data-ttu-id="c2eeb-120">Entity Framework Core už není součástí sdílené ASP.NET Core architektury.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-120">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="c2eeb-121">Úrovně</span><span class="sxs-lookup"><span data-stu-id="c2eeb-121">Medium</span></span>      |
| [<span data-ttu-id="c2eeb-122">Odstranění kaskádových operací se teď ve výchozím nastavení provádí hned.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-122">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="c2eeb-123">Úrovně</span><span class="sxs-lookup"><span data-stu-id="c2eeb-123">Medium</span></span>      |
| [<span data-ttu-id="c2eeb-124">Eager načítání souvisejících entit se teď děje v jednom dotazu.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-124">Eager loading of related entities now happens in a single query</span></span>](#eager-loading-single-query) | <span data-ttu-id="c2eeb-125">Úrovně</span><span class="sxs-lookup"><span data-stu-id="c2eeb-125">Medium</span></span>      |
| [<span data-ttu-id="c2eeb-126">DeleteBehavior. restrict má sémantiku čištění.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-126">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="c2eeb-127">Úrovně</span><span class="sxs-lookup"><span data-stu-id="c2eeb-127">Medium</span></span>      |
| [<span data-ttu-id="c2eeb-128">Změnilo se konfigurační rozhraní API pro vztahy vlastněných typů.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-128">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="c2eeb-129">Úrovně</span><span class="sxs-lookup"><span data-stu-id="c2eeb-129">Medium</span></span>      |
| [<span data-ttu-id="c2eeb-130">Každá vlastnost používá nezávislou generaci celočíselného klíče v paměti.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-130">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="c2eeb-131">Úrovně</span><span class="sxs-lookup"><span data-stu-id="c2eeb-131">Medium</span></span>      |
| [<span data-ttu-id="c2eeb-132">Žádné dotazy pro sledování neprovádějí překlad identity</span><span class="sxs-lookup"><span data-stu-id="c2eeb-132">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="c2eeb-133">Úrovně</span><span class="sxs-lookup"><span data-stu-id="c2eeb-133">Medium</span></span>      |
| [<span data-ttu-id="c2eeb-134">Změny rozhraní API pro metadata</span><span class="sxs-lookup"><span data-stu-id="c2eeb-134">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="c2eeb-135">Úrovně</span><span class="sxs-lookup"><span data-stu-id="c2eeb-135">Medium</span></span>      |
| [<span data-ttu-id="c2eeb-136">Změny rozhraní API pro konkrétního zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="c2eeb-136">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="c2eeb-137">Úrovně</span><span class="sxs-lookup"><span data-stu-id="c2eeb-137">Medium</span></span>      |
| [<span data-ttu-id="c2eeb-138">UseRowNumberForPaging se odebral.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-138">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="c2eeb-139">Úrovně</span><span class="sxs-lookup"><span data-stu-id="c2eeb-139">Medium</span></span>      |
| [<span data-ttu-id="c2eeb-140">Metoda Z tabulek při použití s uloženou procedurou nemůže být složená.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-140">FromSql method when used with stored procedure cannot be composed</span></span>](#fromsqlsproc) | <span data-ttu-id="c2eeb-141">Úrovně</span><span class="sxs-lookup"><span data-stu-id="c2eeb-141">Medium</span></span>      |
| [<span data-ttu-id="c2eeb-142">Metody Z tabulek se dají zadat jenom v kořenech dotazů.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-142">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="c2eeb-143">Slab</span><span class="sxs-lookup"><span data-stu-id="c2eeb-143">Low</span></span>      |
| [<span data-ttu-id="c2eeb-144">~~Provádění dotazu se protokoluje na úrovni ladění~~ . Vrátit</span><span class="sxs-lookup"><span data-stu-id="c2eeb-144">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="c2eeb-145">Slab</span><span class="sxs-lookup"><span data-stu-id="c2eeb-145">Low</span></span>      |
| [<span data-ttu-id="c2eeb-146">Dočasné hodnoty klíčů už nejsou nastavené na instance entit.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-146">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="c2eeb-147">Slab</span><span class="sxs-lookup"><span data-stu-id="c2eeb-147">Low</span></span>      |
| [<span data-ttu-id="c2eeb-148">Závislé entity, které sdílí tabulku s objektem zabezpečení, jsou teď volitelné.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-148">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="c2eeb-149">Slab</span><span class="sxs-lookup"><span data-stu-id="c2eeb-149">Low</span></span>      |
| [<span data-ttu-id="c2eeb-150">Všechny entity sdílející tabulku se sloupcem souběžného tokenu musí být namapovány na vlastnost.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-150">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="c2eeb-151">Slab</span><span class="sxs-lookup"><span data-stu-id="c2eeb-151">Low</span></span>      |
| [<span data-ttu-id="c2eeb-152">Zděděné vlastnosti z nemapovaných typů jsou nyní namapovány na jeden sloupec pro všechny odvozené typy.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-152">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="c2eeb-153">Slab</span><span class="sxs-lookup"><span data-stu-id="c2eeb-153">Low</span></span>      |
| [<span data-ttu-id="c2eeb-154">Konvence vlastností cizího klíče už neodpovídá stejnému názvu jako vlastnost Principal.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-154">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="c2eeb-155">Slab</span><span class="sxs-lookup"><span data-stu-id="c2eeb-155">Low</span></span>      |
| [<span data-ttu-id="c2eeb-156">Připojení k databázi je teď zavřené, pokud už nepoužíváte dřív, než se dokončí jeho objekt TransactionScope.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-156">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="c2eeb-157">Slab</span><span class="sxs-lookup"><span data-stu-id="c2eeb-157">Low</span></span>      |
| [<span data-ttu-id="c2eeb-158">Ve výchozím nastavení se používají pole pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-158">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="c2eeb-159">Slab</span><span class="sxs-lookup"><span data-stu-id="c2eeb-159">Low</span></span>      |
| [<span data-ttu-id="c2eeb-160">Vyvolat, zda je nalezeno více kompatibilních zálohovaných polí</span><span class="sxs-lookup"><span data-stu-id="c2eeb-160">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="c2eeb-161">Slab</span><span class="sxs-lookup"><span data-stu-id="c2eeb-161">Low</span></span>      |
| [<span data-ttu-id="c2eeb-162">Názvy vlastností pouze pro pole se musí shodovat s názvem pole.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-162">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="c2eeb-163">Slab</span><span class="sxs-lookup"><span data-stu-id="c2eeb-163">Low</span></span>      |
| [<span data-ttu-id="c2eeb-164">AddDbContext/AddDbContextPool už nevolá AddLogging a AddMemoryCache.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-164">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="c2eeb-165">Slab</span><span class="sxs-lookup"><span data-stu-id="c2eeb-165">Low</span></span>      |
| [<span data-ttu-id="c2eeb-166">DbContext. entry teď provádí místní DetectChanges.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-166">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="c2eeb-167">Slab</span><span class="sxs-lookup"><span data-stu-id="c2eeb-167">Low</span></span>      |
| [<span data-ttu-id="c2eeb-168">Klíče řetězce a pole bajtů nejsou ve výchozím nastavení generovány klientem.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-168">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="c2eeb-169">Slab</span><span class="sxs-lookup"><span data-stu-id="c2eeb-169">Low</span></span>      |
| [<span data-ttu-id="c2eeb-170">ILoggerFactory je teď služba s vymezeným oborem.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-170">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="c2eeb-171">Slab</span><span class="sxs-lookup"><span data-stu-id="c2eeb-171">Low</span></span>      |
| [<span data-ttu-id="c2eeb-172">Opožděné načítání proxy serverů už nepředpokládá navigační vlastnosti, jsou plně načtené.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-172">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="c2eeb-173">Slab</span><span class="sxs-lookup"><span data-stu-id="c2eeb-173">Low</span></span>      |
| [<span data-ttu-id="c2eeb-174">Nadměrné vytváření interních zprostředkovatelů služeb je teď ve výchozím nastavení chyba.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-174">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="c2eeb-175">Slab</span><span class="sxs-lookup"><span data-stu-id="c2eeb-175">Low</span></span>      |
| [<span data-ttu-id="c2eeb-176">Nové chování pro HasOne/HasMany se volá s jedním řetězcem.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-176">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="c2eeb-177">Slab</span><span class="sxs-lookup"><span data-stu-id="c2eeb-177">Low</span></span>      |
| [<span data-ttu-id="c2eeb-178">Návratový typ pro několik asynchronních metod byl změněn z úlohy na ValueTask</span><span class="sxs-lookup"><span data-stu-id="c2eeb-178">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="c2eeb-179">Slab</span><span class="sxs-lookup"><span data-stu-id="c2eeb-179">Low</span></span>      |
| [<span data-ttu-id="c2eeb-180">Relační: anotace TypeMapping je nyní pouze TypeMapping</span><span class="sxs-lookup"><span data-stu-id="c2eeb-180">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="c2eeb-181">Slab</span><span class="sxs-lookup"><span data-stu-id="c2eeb-181">Low</span></span>      |
| [<span data-ttu-id="c2eeb-182">ToTable na odvozeném typu vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-182">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="c2eeb-183">Slab</span><span class="sxs-lookup"><span data-stu-id="c2eeb-183">Low</span></span>      |
| [<span data-ttu-id="c2eeb-184">EF Core už neposílá direktivu pragma pro vynucení KOFK SQLite.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-184">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="c2eeb-185">Slab</span><span class="sxs-lookup"><span data-stu-id="c2eeb-185">Low</span></span>      |
| [<span data-ttu-id="c2eeb-186">Microsoft. EntityFrameworkCore. sqlite teď závisí na SQLitePCLRaw. bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="c2eeb-186">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="c2eeb-187">Slab</span><span class="sxs-lookup"><span data-stu-id="c2eeb-187">Low</span></span>      |
| [<span data-ttu-id="c2eeb-188">Hodnoty GUID se teď ukládají jako TEXT na SQLite.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-188">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="c2eeb-189">Slab</span><span class="sxs-lookup"><span data-stu-id="c2eeb-189">Low</span></span>      |
| [<span data-ttu-id="c2eeb-190">Hodnoty char se teď ukládají jako TEXT na SQLite.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-190">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="c2eeb-191">Slab</span><span class="sxs-lookup"><span data-stu-id="c2eeb-191">Low</span></span>      |
| [<span data-ttu-id="c2eeb-192">ID migrace se teď generují pomocí kalendáře invariantní jazykové verze.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-192">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="c2eeb-193">Slab</span><span class="sxs-lookup"><span data-stu-id="c2eeb-193">Low</span></span>      |
| [<span data-ttu-id="c2eeb-194">Informace o rozšíření/metadata se odebraly z IDbContextOptionsExtension.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-194">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="c2eeb-195">Slab</span><span class="sxs-lookup"><span data-stu-id="c2eeb-195">Low</span></span>      |
| [<span data-ttu-id="c2eeb-196">LogQueryPossibleExceptionWithAggregateOperator se přejmenovalo.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-196">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="c2eeb-197">Slab</span><span class="sxs-lookup"><span data-stu-id="c2eeb-197">Low</span></span>      |
| [<span data-ttu-id="c2eeb-198">Vysvětlení rozhraní API pro názvy omezení cizího klíče</span><span class="sxs-lookup"><span data-stu-id="c2eeb-198">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="c2eeb-199">Slab</span><span class="sxs-lookup"><span data-stu-id="c2eeb-199">Low</span></span>      |
| [<span data-ttu-id="c2eeb-200">IRelationalDatabaseCreator. HasTables/HasTablesAsync byly zveřejněny.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-200">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="c2eeb-201">Slab</span><span class="sxs-lookup"><span data-stu-id="c2eeb-201">Low</span></span>      |
| [<span data-ttu-id="c2eeb-202">Microsoft. EntityFrameworkCore. Design je teď balíček DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-202">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="c2eeb-203">Slab</span><span class="sxs-lookup"><span data-stu-id="c2eeb-203">Low</span></span>      |
| [<span data-ttu-id="c2eeb-204">SQLitePCL. Raw aktualizováno na verzi 2.0.0</span><span class="sxs-lookup"><span data-stu-id="c2eeb-204">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="c2eeb-205">Slab</span><span class="sxs-lookup"><span data-stu-id="c2eeb-205">Low</span></span>      |
| [<span data-ttu-id="c2eeb-206">NetTopologySuite aktualizace na verzi 2.0.0</span><span class="sxs-lookup"><span data-stu-id="c2eeb-206">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="c2eeb-207">Slab</span><span class="sxs-lookup"><span data-stu-id="c2eeb-207">Low</span></span>      |
| [<span data-ttu-id="c2eeb-208">Je nutné nakonfigurovat více dvojznačných relací odkazujících na sebe.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-208">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="c2eeb-209">Slab</span><span class="sxs-lookup"><span data-stu-id="c2eeb-209">Low</span></span>      |
| [<span data-ttu-id="c2eeb-210">DbFunction. Schema má hodnotu null nebo je prázdný řetězec, který nakonfiguruje, aby byl ve výchozím schématu modelu.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-210">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>](#udf-empty-string) | <span data-ttu-id="c2eeb-211">Slab</span><span class="sxs-lookup"><span data-stu-id="c2eeb-211">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="c2eeb-212">Dotazy LINQ již nejsou vyhodnocovány na klientovi.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-212">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="c2eeb-213">[Sledování problému #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[také zobrazit problém #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="c2eeb-213">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="c2eeb-214">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-214">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-215">Před 3,0, když EF Core nedokázal převést výraz, který byl součástí dotazu na buď SQL, nebo parametr, automaticky vyhodnotí výraz na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-215">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="c2eeb-216">Ve výchozím nastavení vyhodnocování klientů u potenciálně nákladných výrazů aktivovalo pouze upozornění.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-216">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="c2eeb-217">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-217">**New behavior**</span></span>

<span data-ttu-id="c2eeb-218">Počínaje 3,0 EF Core povoluje vyhodnocení v klientovi jenom výrazy v projekci nejvyšší úrovně (poslední volání `Select()` v dotazu).</span><span class="sxs-lookup"><span data-stu-id="c2eeb-218">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="c2eeb-219">Pokud výrazy v jakékoli jiné části dotazu nelze převést na buď SQL, nebo parametr, je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-219">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="c2eeb-220">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-220">**Why**</span></span>

<span data-ttu-id="c2eeb-221">Automatické hodnocení dotazů umožňuje provádět mnoho dotazů i v případě, že není možné přeložit jejich důležité části.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-221">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="c2eeb-222">Toto chování může vést k neočekávanému a potenciálně škodlivému chování, které může být jen zřejmé v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-222">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="c2eeb-223">Například podmínka v volání `Where()`, která se nedá přeložit, může způsobit, že se všechny řádky z tabulky přenesou z databázového serveru a filtr, který se má použít na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-223">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="c2eeb-224">Tato situace může snadno přejít zpět, pokud tabulka obsahuje jenom několik řádků ve vývoji, ale při přesunu aplikace do produkčního prostředí zasáhnout, kde tabulka může obsahovat miliony řádků.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-224">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="c2eeb-225">Upozornění na vyhodnocení klientů se také během vývoje ukázala jako příliš jednoduchá.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-225">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="c2eeb-226">Kromě toho může automatické hodnocení klienta vést k problémům s tím, že vylepšení překladu dotazů pro konkrétní výrazy způsobilo nezamýšlené neúmyslné změny mezi verzemi.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-226">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="c2eeb-227">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-227">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-228">Pokud dotaz nelze plně přeložit, pak buď Přepište dotaz do formuláře, který lze přeložit, nebo použijte `AsEnumerable()`, `ToList()` nebo podobným způsobem, aby byla data vrácena zpět do klienta, kde lze následně dále zpracovávat pomocí LINQ-to-Objects.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-228">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="c2eeb-229">EF Core 3,0 cíle .NET Standard 2,1 místo .NET Standard 2,0</span><span class="sxs-lookup"><span data-stu-id="c2eeb-229">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="c2eeb-230">Sledování problému #15498</span><span class="sxs-lookup"><span data-stu-id="c2eeb-230">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

<span data-ttu-id="c2eeb-231">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-231">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-232">Před 3,0 EF Core cílené .NET Standard 2,0 a spustí se na všech platformách, které podporují tento standard, včetně .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-232">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="c2eeb-233">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-233">**New behavior**</span></span>

<span data-ttu-id="c2eeb-234">Počínaje 3,0 se EF Core cíle .NET Standard 2,1 a spustí se na všech platformách, které podporují tento standard.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-234">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="c2eeb-235">To nezahrnuje .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-235">This does not include .NET Framework.</span></span>

<span data-ttu-id="c2eeb-236">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-236">**Why**</span></span>

<span data-ttu-id="c2eeb-237">Toto je součást strategického rozhodnutí napříč technologiemi .NET a zaměřuje se na energii na platformě .NET Core a dalších moderních platformách .NET, jako je Xamarin.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-237">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="c2eeb-238">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-238">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-239">Zvažte přechod na moderní platformu .NET.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-239">Consider moving to a modern .NET platform.</span></span> <span data-ttu-id="c2eeb-240">Pokud to není možné, pak pokračujte v používání EF Core 2,1 nebo EF Core 2,2, které podporují .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-240">If this is not possible, then continue to use EF Core 2.1 or EF Core 2.2, both of which support .NET Framework.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="c2eeb-241">Entity Framework Core už není součástí sdílené ASP.NET Core architektury.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-241">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="c2eeb-242">Sledování oznámení o problémech # 325</span><span class="sxs-lookup"><span data-stu-id="c2eeb-242">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="c2eeb-243">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-243">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-244">Pokud jste před ASP.NET Core 3,0 přidali odkaz na balíček `Microsoft.AspNetCore.App` nebo `Microsoft.AspNetCore.All`, bude obsahovat EF Core a někteří poskytovatelé EF Core dat jako poskytovatelé SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-244">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="c2eeb-245">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-245">**New behavior**</span></span>

<span data-ttu-id="c2eeb-246">Počínaje 3,0 ASP.NET Core sdílené rozhraní nezahrnuje EF Core nebo žádné poskytovatele EF Core dat.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-246">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="c2eeb-247">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-247">**Why**</span></span>

<span data-ttu-id="c2eeb-248">Před touto změnou EF Core nutné jiné kroky v závislosti na tom, zda je aplikace cílena ASP.NET Core a SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-248">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="c2eeb-249">Upgrade ASP.NET Core taky vynutil upgrade EF Core a poskytovatele SQL Server, který není vždycky žádoucí.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-249">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="c2eeb-250">V důsledku této změny je prostředí získání EF Core stejné u všech poskytovatelů, podporovaných implementací rozhraní .NET a typů aplikací.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-250">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="c2eeb-251">Vývojáři teď můžou přesně řídit, když EF Core a EF Core se upgradují poskytovatelé dat.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-251">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="c2eeb-252">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-252">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-253">Chcete-li použít EF Core v aplikaci ASP.NET Core 3,0 nebo v jakékoli jiné podporované aplikaci, explicitně přidejte odkaz na balíček EF Coreho poskytovatele databáze, který bude aplikace používat.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-253">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="c2eeb-254">EF Core nástroj příkazového řádku dotnet EF již není součástí .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="c2eeb-254">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="c2eeb-255">Sledování problému #14016</span><span class="sxs-lookup"><span data-stu-id="c2eeb-255">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="c2eeb-256">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-256">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-257">Před 3,0 byl nástroj `dotnet ef` součástí .NET Core SDK a byl snadno dostupný pro použití z příkazového řádku z libovolného projektu bez nutnosti dalších kroků.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-257">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="c2eeb-258">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-258">**New behavior**</span></span>

<span data-ttu-id="c2eeb-259">Počínaje 3,0 sada .NET SDK neobsahuje nástroj `dotnet ef`, takže před tím, než ho bude možné použít, je nutné explicitně nainstalovat jako místní nebo globální nástroj.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-259">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="c2eeb-260">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-260">**Why**</span></span>

<span data-ttu-id="c2eeb-261">Tato změna nám umožňuje distribuovat a aktualizovat `dotnet ef` jako regulární nástroj rozhraní příkazového řádku .NET v NuGet, a to v souladu se skutečností, že EF Core 3,0 je také vždy distribuován jako balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-261">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="c2eeb-262">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-262">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-263">Aby bylo možné spravovat migrace nebo uživatelské rozhraní `DbContext`, nainstalujte `dotnet-ef` jako globální nástroj:</span><span class="sxs-lookup"><span data-stu-id="c2eeb-263">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef
  ```

<span data-ttu-id="c2eeb-264">Můžete také získat místní nástroj při obnovování závislostí projektu, který deklaruje jako závislost nástrojů pomocí [souboru manifestu nástroje](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="c2eeb-264">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="c2eeb-265">Z tabulek, ExecuteSql a ExecuteSqlAsync byly přejmenovány.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-265">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="c2eeb-266">Sledování problému #10996</span><span class="sxs-lookup"><span data-stu-id="c2eeb-266">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="c2eeb-267">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-267">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-268">Před EF Core 3,0 byly tyto názvy metod přetíženy, aby fungovaly s normálním řetězcem nebo řetězcem, které by měly být interpolované na SQL a parametry.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-268">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="c2eeb-269">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-269">**New behavior**</span></span>

<span data-ttu-id="c2eeb-270">Počínaje EF Core 3,0 použijte `FromSqlRaw`, `ExecuteSqlRaw` a `ExecuteSqlRawAsync` k vytvoření parametrizovaného dotazu, kde jsou parametry předány nezávisle na řetězci dotazu.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-270">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="c2eeb-271">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c2eeb-271">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="c2eeb-272">Pomocí `FromSqlInterpolated`, `ExecuteSqlInterpolated` a `ExecuteSqlInterpolatedAsync` vytvořte parametrizovaný dotaz, ve kterém jsou parametry předány jako součást interpolované řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-272">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="c2eeb-273">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c2eeb-273">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="c2eeb-274">Všimněte si, že obě výše uvedené dotazy vytvoří stejný parametrizovaný SQL se stejnými parametry SQL.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-274">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="c2eeb-275">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-275">**Why**</span></span>

<span data-ttu-id="c2eeb-276">Přetížení metody, jako to, usnadňuje náhodné volání nezpracované řetězcové metody, pokud by záměr byl zavolat interpolovaná řetězcová metoda a druhá možnost kolem.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-276">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="c2eeb-277">To může vést k tomu, že dotazy nejsou parametrizované, pokud by měly být.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-277">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="c2eeb-278">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-278">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-279">Přepněte na použití nových názvů metod.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-279">Switch to use the new method names.</span></span>

<a name="fromsqlsproc"></a>
### <a name="fromsql-method-when-used-with-stored-procedure-cannot-be-composed"></a><span data-ttu-id="c2eeb-280">Metoda Z tabulek při použití s uloženou procedurou nemůže být složená.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-280">FromSql method when used with stored procedure cannot be composed</span></span>

[<span data-ttu-id="c2eeb-281">Sledování problému #15392</span><span class="sxs-lookup"><span data-stu-id="c2eeb-281">Tracking Issue #15392</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15392)

<span data-ttu-id="c2eeb-282">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-282">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-283">Před EF Core 3,0 se metoda Z tabulek pokusila zjistit, zda je možné sestavit předaný SQL.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-283">Before EF Core 3.0, FromSql method tried to detect if the passed SQL can be composed upon.</span></span> <span data-ttu-id="c2eeb-284">Vyvolalo se hodnocení klienta, pokud SQL bylo bez možnosti složení, jako je uložená procedura.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-284">It did client evaluation when the SQL was non-composable like a stored procedure.</span></span> <span data-ttu-id="c2eeb-285">Následující dotaz fungoval spuštěním uložené procedury na serveru a provedením FirstOrDefault na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-285">The following query worked by running the stored procedure on the server and doing FirstOrDefault on the client side.</span></span>

```C#
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").FirstOrDefault();
```

<span data-ttu-id="c2eeb-286">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-286">**New behavior**</span></span>

<span data-ttu-id="c2eeb-287">Počínaje EF Core 3,0 se EF Core nepokusí analyzovat SQL.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-287">Starting with EF Core 3.0, EF Core will not try to parse the SQL.</span></span> <span data-ttu-id="c2eeb-288">Takže pokud vytváříte po FromSqlRaw/FromSqlInterpolated, pak EF Core vytvoří příkaz SQL, který by způsobil dotaz sub.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-288">So if you are composing after FromSqlRaw/FromSqlInterpolated, then EF Core will compose the SQL by causing sub query.</span></span> <span data-ttu-id="c2eeb-289">Takže pokud používáte uloženou proceduru se složením, zobrazí se výjimka pro neplatnou syntaxi SQL.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-289">So if you are using a stored procedure with composition then you will get an exception for invalid SQL syntax.</span></span>

<span data-ttu-id="c2eeb-290">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-290">**Why**</span></span>

<span data-ttu-id="c2eeb-291">EF Core 3,0 nepodporuje automatické hodnocení klienta, protože to bylo náchylné k chybám, jak je vysvětleno [zde](#linq-queries-are-no-longer-evaluated-on-the-client).</span><span class="sxs-lookup"><span data-stu-id="c2eeb-291">EF Core 3.0 does not support automatic client evaluation, since it was error prone as explained [here](#linq-queries-are-no-longer-evaluated-on-the-client).</span></span>

<span data-ttu-id="c2eeb-292">**Zmírnění**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-292">**Mitigation**</span></span>

<span data-ttu-id="c2eeb-293">Pokud používáte uloženou proceduru v FromSqlRaw/FromSqlInterpolated, znamená to, že se na ni nelze založit, takže můžete přidat __AsEnumerable/AsAsyncEnumerable__ hned po volání metody z tabulek, aby se zabránilo jakémukoli složení na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-293">If you are using a stored procedure in FromSqlRaw/FromSqlInterpolated, you know that it cannot be composed upon, so you can add __AsEnumerable/AsAsyncEnumerable__ right after the FromSql method call to avoid any composition on server side.</span></span>

```C#
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").AsEnumerable().FirstOrDefault();
```

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="c2eeb-294">Metody Z tabulek se dají zadat jenom v kořenech dotazů.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-294">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="c2eeb-295">Sledování problému #15704</span><span class="sxs-lookup"><span data-stu-id="c2eeb-295">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="c2eeb-296">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-296">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-297">Před EF Core 3,0 lze zadat metodu `FromSql` kdekoli v dotazu.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-297">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="c2eeb-298">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-298">**New behavior**</span></span>

<span data-ttu-id="c2eeb-299">Počínaje EF Core 3,0 se nové metody `FromSqlRaw` a `FromSqlInterpolated` (které nahrazují `FromSql`) dají zadat jenom pro kořeny dotazů, tj. přímo na `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-299">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="c2eeb-300">Pokud se pokusíte zadat jiné místo jinak, dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-300">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="c2eeb-301">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-301">**Why**</span></span>

<span data-ttu-id="c2eeb-302">Zadání `FromSql` kdekoli jinde než na `DbSet` neobsahovalo žádné přidané významy nebo přidání hodnoty a může v některých scénářích způsobit nejednoznačnost.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-302">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="c2eeb-303">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-303">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-304">volání `FromSql` by se měla přesunout přímo na `DbSet`, na kterou se vztahují.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-304">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="c2eeb-305">Žádné dotazy pro sledování neprovádějí překlad identity</span><span class="sxs-lookup"><span data-stu-id="c2eeb-305">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="c2eeb-306">Sledování problému #13518</span><span class="sxs-lookup"><span data-stu-id="c2eeb-306">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="c2eeb-307">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-307">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-308">Před EF Core 3,0 se stejná instance entity používá pro všechny výskyty entity se zadaným typem a ID.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-308">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="c2eeb-309">To odpovídá chování sledovacích dotazů.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-309">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="c2eeb-310">Například tento dotaz:</span><span class="sxs-lookup"><span data-stu-id="c2eeb-310">For example, this query:</span></span>

```C#
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="c2eeb-311">vrátí stejnou instanci `Category` pro každé `Product`, která je přidružena k dané kategorii.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-311">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="c2eeb-312">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-312">**New behavior**</span></span>

<span data-ttu-id="c2eeb-313">Počínaje EF Core 3,0 budou vytvořeny různé instance entit při výskytu entity se zadaným typem a ID na různých místech vráceného grafu.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-313">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="c2eeb-314">Například výše uvedený dotaz vrátí novou instanci `Category` pro každé `Product`, a to i v případě, že jsou ke stejné kategorii přidruženy dva produkty.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-314">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="c2eeb-315">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-315">**Why**</span></span>

<span data-ttu-id="c2eeb-316">Překlad identity (to znamená, že určení, že entita má stejný typ a ID jako dříve zjištěná entita) přidává další výkon a režii paměti.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-316">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="c2eeb-317">Obvykle se spustí čítač, aby se v prvním místě nepoužily žádné dotazy na sledování.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-317">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="c2eeb-318">I když může být v některých případech užitečný překlad identity, není potřeba, pokud se entity mají serializovat a odeslat klientovi, což je běžné pro žádné dotazy pro sledování.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-318">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="c2eeb-319">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-319">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-320">Pokud je vyžadováno rozlišení identity, použijte dotaz sledování.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-320">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="c2eeb-321">~~Provádění dotazu se protokoluje na úrovni ladění~~ . Vrátit</span><span class="sxs-lookup"><span data-stu-id="c2eeb-321">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="c2eeb-322">Sledování problému #14523</span><span class="sxs-lookup"><span data-stu-id="c2eeb-322">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="c2eeb-323">Tuto změnu jsme vrátili, protože nová konfigurace v EF Core 3,0 umožňuje, aby byla úroveň protokolu pro libovolnou událost specifikována aplikací.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-323">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="c2eeb-324">Chcete-li například přepnout protokolování SQL na `Debug`, explicitně nakonfigurujte úroveň v `OnConfiguring` nebo `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="c2eeb-324">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="c2eeb-325">Dočasné hodnoty klíčů už nejsou nastavené na instance entit.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-325">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="c2eeb-326">Sledování problému #12378</span><span class="sxs-lookup"><span data-stu-id="c2eeb-326">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="c2eeb-327">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-327">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-328">Před EF Core 3,0 byly přiřazeny dočasné hodnoty ke všem klíčovým vlastnostem, které by později měly skutečnou hodnotu vygenerovanou databází.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-328">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="c2eeb-329">Obvykle jsou tyto dočasné hodnoty velkými zápornými čísly.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-329">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="c2eeb-330">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-330">**New behavior**</span></span>

<span data-ttu-id="c2eeb-331">Počínaje 3,0 EF Core ukládá hodnotu dočasného klíče jako součást informací o sledování entity a ponechá vlastnost klíče beze změny.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-331">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="c2eeb-332">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-332">**Why**</span></span>

<span data-ttu-id="c2eeb-333">Tato změna byla provedena, aby se předešlo omylům hodnotám klíčů v případě, že entita, která byla dříve sledována jinou instancí `DbContext`, je přesunuta do jiné instance `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-333">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="c2eeb-334">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-334">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-335">Aplikace, které přiřazují hodnoty primárního klíče k cizím klíčům k vytvoření přidružení mezi entitami, můžou záviset na starém chování, pokud jsou primární klíče generované úložištěm a patří do entit ve stavu `Added`.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-335">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="c2eeb-336">K tomu je možné se vyhnout:</span><span class="sxs-lookup"><span data-stu-id="c2eeb-336">This can be avoided by:</span></span>
* <span data-ttu-id="c2eeb-337">Nepoužívejte klíče generované úložištěm.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-337">Not using store-generated keys.</span></span>
* <span data-ttu-id="c2eeb-338">Nastavení vlastností navigace pro vytváření relací místo nastavení hodnot cizích klíčů.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-338">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="c2eeb-339">Získá aktuální dočasné hodnoty klíče z informací o sledování entity.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-339">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="c2eeb-340">Například `context.Entry(blog).Property(e => e.Id).CurrentValue` vrátí dočasnou hodnotu, i když se nenastaví `blog.Id`.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-340">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="c2eeb-341">DetectChanges respektuje hodnoty klíčů generované úložištěm.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-341">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="c2eeb-342">Sledování problému #14616</span><span class="sxs-lookup"><span data-stu-id="c2eeb-342">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="c2eeb-343">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-343">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-344">Před EF Core 3,0 se Nesledovaná entita, kterou najde `DetectChanges`, sledovala ve stavu `Added` a vložila se jako nový řádek při volání `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-344">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="c2eeb-345">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-345">**New behavior**</span></span>

<span data-ttu-id="c2eeb-346">Od EF Core 3,0 platí, že pokud entita používá vygenerované hodnoty klíčů a je nastavená nějaká hodnota klíče, bude se entita sledovat ve stavu `Modified`.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-346">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="c2eeb-347">To znamená, že se předpokládá, že řádek pro entitu existuje a že se bude aktualizovat při volání `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-347">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="c2eeb-348">Pokud hodnota klíče není nastavená, nebo pokud typ entity nepoužívá vygenerované klíče, bude nová entita dál sledována jako `Added` jako v předchozích verzích.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-348">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="c2eeb-349">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-349">**Why**</span></span>

<span data-ttu-id="c2eeb-350">Tato změna byla provedena tak, aby byla pro práci s nepřipojenými grafy entit při použití klíčů generovaných úložištěm snazší.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-350">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="c2eeb-351">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-351">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-352">Tato změna může přerušit aplikaci, pokud je typ entity nakonfigurovaný na používání vygenerovaných klíčů, ale hodnoty klíčů jsou explicitně nastavené pro nové instance.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-352">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="c2eeb-353">Opravou je explicitně nakonfigurovat klíčové vlastnosti tak, aby nepoužívaly vygenerované hodnoty.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-353">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="c2eeb-354">Například s rozhraním API Fluent:</span><span class="sxs-lookup"><span data-stu-id="c2eeb-354">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="c2eeb-355">Nebo s poznámkami k datům:</span><span class="sxs-lookup"><span data-stu-id="c2eeb-355">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="c2eeb-356">Odstranění kaskádových operací se teď ve výchozím nastavení provádí hned.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-356">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="c2eeb-357">Sledování problému #10114</span><span class="sxs-lookup"><span data-stu-id="c2eeb-357">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="c2eeb-358">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-358">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-359">Před 3,0 EF Core aplikovány kaskádové akce (odstraňování závislých entit, když se odstraní požadovaný objekt zabezpečení nebo když je vztah k požadovanému objektu zabezpečení vážně) nevznikl, dokud není voláno SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-359">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="c2eeb-360">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-360">**New behavior**</span></span>

<span data-ttu-id="c2eeb-361">Od 3,0 EF Core aplikuje kaskádové akce hned po zjištění spouštěcí podmínky.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-361">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="c2eeb-362">Když například zavoláte `context.Remove()`, aby se odstranila hlavní entita, bude se u všech sledovaných závislých objektů taky nastavená také hodnota `Deleted` hned.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-362">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="c2eeb-363">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-363">**Why**</span></span>

<span data-ttu-id="c2eeb-364">Tato změna byla provedena za účelem zlepšení prostředí pro scénáře vytváření datových vazeb a auditování, kde je důležité pochopit, které entity budou odstraněny _před_ voláním `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-364">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="c2eeb-365">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-365">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-366">Předchozí chování lze obnovit pomocí nastavení na `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-366">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="c2eeb-367">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c2eeb-367">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="eager-loading-single-query"></a>
### <a name="eager-loading-of-related-entities-now-happens-in-a-single-query"></a><span data-ttu-id="c2eeb-368">Eager načítání souvisejících entit se teď děje v jednom dotazu.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-368">Eager loading of related entities now happens in a single query</span></span>

[<span data-ttu-id="c2eeb-369">Sledování problému #18022</span><span class="sxs-lookup"><span data-stu-id="c2eeb-369">Tracking issue #18022</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/18022)

<span data-ttu-id="c2eeb-370">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-370">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-371">Před 3,0 se eagerly načítání kolekcí pomocí operátorů `Include` způsobilo generování více dotazů v relační databázi, jednu pro každý typ související entity.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-371">Before 3.0, eagerly loading collection navigations via `Include` operators caused multiple queries to be generated on relational database, one for each related entity type.</span></span>

<span data-ttu-id="c2eeb-372">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-372">**New behavior**</span></span>

<span data-ttu-id="c2eeb-373">Počínaje 3,0 EF Core generuje jediný dotaz s spojeními relačních databází.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-373">Starting with 3.0, EF Core generates a single query with JOINs on relational databases.</span></span>

<span data-ttu-id="c2eeb-374">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-374">**Why**</span></span>

<span data-ttu-id="c2eeb-375">Vydání více dotazů pro implementaci jednoho dotazu LINQ způsobilo velký počet problémů, včetně negativního výkonu, protože bylo nutné použít více databázových převodů, a problémy s integritou dat v případě, že každý dotaz může sledovat jiný stav databáze.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-375">Issuing multiple queries to implement a single LINQ query caused numerous issues, including negative performance as multiple database roundtrips were necessary, and data coherency issues as each query could observe a different state of the database.</span></span>

<span data-ttu-id="c2eeb-376">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-376">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-377">I když se nejedná o zásadní změnu, může to mít výrazný vliv na výkon aplikace, když jeden dotaz obsahuje velký počet operátorů `Include` v navigaci kolekcí.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-377">While technically this is not a breaking change, it could have a considerable effect on application performance when a single query contains a large number of `Include` operator on collection navigations.</span></span> <span data-ttu-id="c2eeb-378">[Podívejte se na tento komentář](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) , kde najdete další informace a rychlé psaní dotazů.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-378">[See this comment](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) for more information and for rewriting queries in a more efficient way.</span></span>

**

<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="c2eeb-379">DeleteBehavior. restrict má sémantiku čištění.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-379">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="c2eeb-380">Sledování problému #12661</span><span class="sxs-lookup"><span data-stu-id="c2eeb-380">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="c2eeb-381">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-381">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-382">Před 3,0 `DeleteBehavior.Restrict` vytvořené cizí klíče v databázi se sémantikou `Restrict`, ale také došlo ke změně interní opravy nezřetelným způsobem.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-382">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="c2eeb-383">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-383">**New behavior**</span></span>

<span data-ttu-id="c2eeb-384">Počínaje 3,0 `DeleteBehavior.Restrict` zajistí, že se cizí klíče vytvoří s sémantikou `Restrict` – to znamená bez kaskády. porušení omezení throw – bez vlivu na interní opravu EF</span><span class="sxs-lookup"><span data-stu-id="c2eeb-384">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="c2eeb-385">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-385">**Why**</span></span>

<span data-ttu-id="c2eeb-386">Tato změna byla provedena pro zlepšení prostředí pro použití `DeleteBehavior` intuitivním způsobem, a to bez neočekávaných vedlejších účinků.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-386">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="c2eeb-387">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-387">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-388">Předchozí chování lze obnovit pomocí `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-388">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="c2eeb-389">Typy dotazů jsou konsolidovány s typy entit</span><span class="sxs-lookup"><span data-stu-id="c2eeb-389">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="c2eeb-390">Sledování problému #14194</span><span class="sxs-lookup"><span data-stu-id="c2eeb-390">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="c2eeb-391">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-391">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-392">Před EF Core 3,0 byly [typy dotazů](xref:core/modeling/keyless-entity-types) prostředkem pro dotazování na data, která nedefinují primární klíč strukturovaným způsobem.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-392">Before EF Core 3.0, [query types](xref:core/modeling/keyless-entity-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="c2eeb-393">To znamená, že typ dotazu byl použit pro mapování typů entit bez klíčů (pravděpodobnější z pohledu, ale také z tabulky), zatímco byl použit regulární typ entity, když byl klíč k dispozici (pravděpodobnější z tabulky, ale případně z pohledu).</span><span class="sxs-lookup"><span data-stu-id="c2eeb-393">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="c2eeb-394">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-394">**New behavior**</span></span>

<span data-ttu-id="c2eeb-395">Typ dotazu se teď bude jednat jenom o typ entity bez primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-395">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="c2eeb-396">Typy entit bez klíčů mají stejné funkce jako typy dotazů v předchozích verzích.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-396">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="c2eeb-397">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-397">**Why**</span></span>

<span data-ttu-id="c2eeb-398">Tato změna byla provedena proto, aby se snížila nejasnost v souvislosti s typy dotazů.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-398">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="c2eeb-399">Konkrétně jsou bez klíčů typy entit a jsou ze své podstaty jen pro čtení, ale neměly by být použity pouze proto, že typ entity musí být jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-399">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="c2eeb-400">Podobně jsou často mapovány na zobrazení, ale to je pouze v případě, že zobrazení často nedefinují klíče.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-400">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="c2eeb-401">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-401">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-402">Následující části rozhraní API jsou teď zastaralé:</span><span class="sxs-lookup"><span data-stu-id="c2eeb-402">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="c2eeb-403">**`ModelBuilder.Query<>()`** -místo toho musí být volána `ModelBuilder.Entity<>().HasNoKey()`, aby bylo možné označit typ entity jako neobsahující žádné klíče.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-403">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="c2eeb-404">Tato služba by se ještě nenakonfigurovala pomocí konvence, aby nedocházelo k chybám konfigurace, když je primární klíč očekávaný, ale neodpovídá konvenci.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-404">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="c2eeb-405">**`DbQuery<>`** -místo toho by měla být použita `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-405">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="c2eeb-406">**`DbContext.Query<>()`** -místo toho by měla být použita `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-406">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="c2eeb-407">Změnilo se konfigurační rozhraní API pro vztahy vlastněných typů.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-407">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="c2eeb-408">[Sledování problému #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)problém se sledováním 
[#9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)[#14153 problémy s sledováním](https://github.com/aspnet/EntityFrameworkCore/issues/14153) 
</span><span class="sxs-lookup"><span data-stu-id="c2eeb-408">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="c2eeb-409">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-409">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-410">Před EF Core 3,0 se konfigurace vztahu vlastníka provedla přímo po volání `OwnsOne` nebo `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-410">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="c2eeb-411">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-411">**New behavior**</span></span>

<span data-ttu-id="c2eeb-412">Počínaje EF Core 3,0 teď existuje Fluent API pro konfiguraci navigační vlastnosti pro vlastníka pomocí `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-412">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="c2eeb-413">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c2eeb-413">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="c2eeb-414">Konfigurace vztahující se k vztahu mezi vlastníkem a vlastníkem by nyní měla být zřetězena po `WithOwner()` podobně jako u dalších konfigurací vztahů.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-414">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="c2eeb-415">I když se konfigurace samotného vlastního typu bude pořád zřetězit za `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-415">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="c2eeb-416">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c2eeb-416">For example:</span></span>

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

<span data-ttu-id="c2eeb-417">Kromě toho, že volání `Entity()`, `HasOne()` nebo `Set()` s cílem vlastněného typu nyní vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-417">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="c2eeb-418">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-418">**Why**</span></span>

<span data-ttu-id="c2eeb-419">Tato změna byla provedena za účelem vytvoření čisticího oddělení mezi konfigurací samotného typu a _vztahu k_ typu, který je vlastníkem.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-419">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="c2eeb-420">Tím se zase odeberou nejednoznačnosti a nejasnosti u metod, jako je `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-420">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="c2eeb-421">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-421">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-422">Změňte konfiguraci vztahů vlastněných typů tak, aby používala novou plochu rozhraní API, jak je znázorněno v předchozím příkladu.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-422">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="c2eeb-423">Závislé entity, které sdílí tabulku s objektem zabezpečení, jsou teď volitelné.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-423">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="c2eeb-424">Sledování problému #9005</span><span class="sxs-lookup"><span data-stu-id="c2eeb-424">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="c2eeb-425">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-425">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-426">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="c2eeb-426">Consider the following model:</span></span>
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
<span data-ttu-id="c2eeb-427">Pokud je `OrderDetails` ve vlastnictví `Order` nebo explicitně namapované na stejnou tabulku, před EF Core 3,0 byla při přidávání nového `Order` vždy vyžadována instance `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-427">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="c2eeb-428">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-428">**New behavior**</span></span>

<span data-ttu-id="c2eeb-429">Počínaje 3,0 EF Core umožňuje přidat `Order` bez `OrderDetails` a namapovat všechny vlastnosti `OrderDetails` s výjimkou primárního klíče na sloupce s možnou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-429">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="c2eeb-430">Při dotazování EF Core nastaví `OrderDetails` na `null`, pokud některá z jejích požadovaných vlastností nemá hodnotu, nebo pokud se nejedná o žádné požadované vlastnosti kromě primárního klíče a všechny vlastnosti `null`.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-430">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="c2eeb-431">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-431">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-432">Pokud má váš model sdílení tabulky závislé na všech volitelných sloupcích, ale navigace ukazující na ni se neočekává `null`, aplikace by měla být upravena tak, aby zpracovávala případy, když je navigace `null`.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-432">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="c2eeb-433">Pokud to není možné, musí být do typu entity přidána požadovaná vlastnost nebo k ní @no__t musí být přiřazena alespoň jedna vlastnost.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-433">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="c2eeb-434">Všechny entity sdílející tabulku se sloupcem souběžného tokenu musí být namapovány na vlastnost.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-434">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="c2eeb-435">Sledování problému #14154</span><span class="sxs-lookup"><span data-stu-id="c2eeb-435">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="c2eeb-436">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-436">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-437">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="c2eeb-437">Consider the following model:</span></span>
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
<span data-ttu-id="c2eeb-438">Před EF Core 3,0, pokud `OrderDetails` vlastní `Order` nebo explicitně namapované na stejnou tabulku, pak aktualizace pouze `OrderDetails` nebude aktualizovat hodnotu `Version` v klientovi a další aktualizace nebude úspěšná.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-438">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="c2eeb-439">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-439">**New behavior**</span></span>

<span data-ttu-id="c2eeb-440">Počínaje 3,0 EF Core rozšíří novou hodnotu `Version` na `Order`, pokud vlastní `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-440">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="c2eeb-441">V opačném případě je při ověřování modelu vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-441">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="c2eeb-442">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-442">**Why**</span></span>

<span data-ttu-id="c2eeb-443">Tato změna byla provedena, aby nedocházelo k zastaralé hodnotě tokenu souběžnosti, pokud je aktualizována pouze jedna z entit mapovaných na stejnou tabulku.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-443">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="c2eeb-444">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-444">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-445">Všechny entity, které sdílejí tabulku, musí obsahovat vlastnost, která je namapovaná na sloupec tokenu souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-445">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="c2eeb-446">Je možné, že ho vytvoříte ve stínovém stavu:</span><span class="sxs-lookup"><span data-stu-id="c2eeb-446">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="c2eeb-447">Zděděné vlastnosti z nemapovaných typů jsou nyní namapovány na jeden sloupec pro všechny odvozené typy.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-447">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="c2eeb-448">Sledování problému #13998</span><span class="sxs-lookup"><span data-stu-id="c2eeb-448">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="c2eeb-449">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-449">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-450">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="c2eeb-450">Consider the following model:</span></span>
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

<span data-ttu-id="c2eeb-451">Před EF Core 3,0 bude vlastnost `ShippingAddress` namapována na samostatné sloupce `BulkOrder` a `Order` ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-451">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="c2eeb-452">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-452">**New behavior**</span></span>

<span data-ttu-id="c2eeb-453">Počínaje 3,0 EF Core pro `ShippingAddress` vytvoří pouze jeden sloupec.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-453">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="c2eeb-454">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-454">**Why**</span></span>

<span data-ttu-id="c2eeb-455">Starý behavoir nebyl očekáván.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-455">The old behavoir was unexpected.</span></span>

<span data-ttu-id="c2eeb-456">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-456">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-457">Vlastnost může být stále explicitně namapována na samostatný sloupec odvozených typů:</span><span class="sxs-lookup"><span data-stu-id="c2eeb-457">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="c2eeb-458">Konvence vlastností cizího klíče už neodpovídá stejnému názvu jako vlastnost Principal.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-458">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="c2eeb-459">Sledování problému #13274</span><span class="sxs-lookup"><span data-stu-id="c2eeb-459">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="c2eeb-460">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-460">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-461">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="c2eeb-461">Consider the following model:</span></span>
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
<span data-ttu-id="c2eeb-462">Před EF Core 3,0 se pro cizí klíč podle konvence použije vlastnost `CustomerId`.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-462">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="c2eeb-463">Pokud je však `Order` vlastněný typ, pak by to vedlo také k tomu, že `CustomerId` primární klíč, a to není obvykle očekávání.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-463">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="c2eeb-464">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-464">**New behavior**</span></span>

<span data-ttu-id="c2eeb-465">Počínaje 3,0 se EF Core nepokouší použít vlastnosti pro cizí klíče podle konvence, pokud mají stejný název jako vlastnost Principal.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-465">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="c2eeb-466">Název objektu zabezpečení zřetězený s názvem vlastnosti objektu zabezpečení a název navigace zřetězený s vzory názvů hlavních vlastností se pořád shodují.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-466">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="c2eeb-467">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c2eeb-467">For example:</span></span>

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

<span data-ttu-id="c2eeb-468">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-468">**Why**</span></span>

<span data-ttu-id="c2eeb-469">Tato změna byla provedena, aby nedocházelo k chybnému definování vlastností primárního klíče u vlastněných typů.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-469">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="c2eeb-470">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-470">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-471">Pokud by vlastnost měla být cizí klíč, a proto je součástí primárního klíče, pak ji explicitně nakonfigurujte jako takovou.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-471">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="c2eeb-472">Připojení k databázi je teď zavřené, pokud už nepoužíváte dřív, než se dokončí jeho objekt TransactionScope.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-472">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="c2eeb-473">Sledování problému #14218</span><span class="sxs-lookup"><span data-stu-id="c2eeb-473">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="c2eeb-474">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-474">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-475">Pokud kontext v rámci EF Core 3,0 otevře připojení v `TransactionScope`, zůstane připojení otevřené, zatímco aktuální `TransactionScope` je aktivní.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-475">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="c2eeb-476">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-476">**New behavior**</span></span>

<span data-ttu-id="c2eeb-477">Od 3,0 EF Core ukončí připojení, jakmile ho dokončí jeho používání.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-477">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="c2eeb-478">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-478">**Why**</span></span>

<span data-ttu-id="c2eeb-479">Tato změna umožňuje použít více kontextů ve stejném `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-479">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="c2eeb-480">Nové chování se také shoduje s EF6.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-480">The new behavior also matches EF6.</span></span>

<span data-ttu-id="c2eeb-481">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-481">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-482">Pokud připojení zůstane otevřené, otevřete explicitní volání `OpenConnection()`, aby se zajistilo, že EF Core nezavřou předčasně:</span><span class="sxs-lookup"><span data-stu-id="c2eeb-482">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="c2eeb-483">Každá vlastnost používá nezávislou generaci celočíselného klíče v paměti.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-483">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="c2eeb-484">Sledování problému #6872</span><span class="sxs-lookup"><span data-stu-id="c2eeb-484">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="c2eeb-485">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-485">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-486">Před EF Core 3,0 byl pro všechny vlastnosti celého čísla klíče v paměti použit jeden generátor sdílených hodnot.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-486">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="c2eeb-487">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-487">**New behavior**</span></span>

<span data-ttu-id="c2eeb-488">Počínaje EF Core 3,0 každé klíčové vlastnosti celého čísla získá vlastní generátor hodnot při použití databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-488">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="c2eeb-489">Také pokud je databáze odstraněna, generování klíče se obnoví pro všechny tabulky.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-489">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="c2eeb-490">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-490">**Why**</span></span>

<span data-ttu-id="c2eeb-491">Tato změna byla provedená tak, aby se při vytváření klíčů v paměti lépe rovnala generování klíčů v paměti a vylepšila se možnost izolovat testy od sebe při použití databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-491">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="c2eeb-492">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-492">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-493">To může poškodit aplikaci, která se spoléhá na konkrétní hodnoty klíče v paměti, které se mají nastavit.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-493">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="c2eeb-494">Místo toho se nemusíte spoléhat na konkrétní hodnoty klíčů nebo aktualizovat tak, aby odpovídaly novému chování.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-494">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="c2eeb-495">Ve výchozím nastavení se používají pole pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-495">Backing fields are used by default</span></span>

[<span data-ttu-id="c2eeb-496">Sledování problému #12430</span><span class="sxs-lookup"><span data-stu-id="c2eeb-496">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="c2eeb-497">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-497">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-498">Před 3,0, a to i v případě, že je známé pole zálohování pro vlastnost, EF Core ve výchozím nastavení přečetl a zapsat hodnotu vlastnosti pomocí metod getter a setter vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-498">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="c2eeb-499">Výjimkou je provedení dotazu, kde by se pole zálohování nastavilo přímo, pokud je známé.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-499">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="c2eeb-500">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-500">**New behavior**</span></span>

<span data-ttu-id="c2eeb-501">Počínaje EF Core 3,0 platí, že pokud je známé pole zálohování pro vlastnost, pak EF Core tuto vlastnost vždy přečte a zapíše pomocí pole pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-501">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="c2eeb-502">To může způsobit přerušení aplikace, pokud se aplikace spoléhá na další chování kódované v metodách getter nebo setter.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-502">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="c2eeb-503">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-503">**Why**</span></span>

<span data-ttu-id="c2eeb-504">Tato změna byla provedena EF Core proto, aby při provádění databázových operací, které obsahují entity, ve výchozím nastavení nechybně aktivovala obchodní logiku.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-504">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="c2eeb-505">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-505">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-506">Chování před 3,0 se dá obnovit pomocí konfigurace režimu přístupu vlastnosti na `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-506">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="c2eeb-507">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c2eeb-507">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="c2eeb-508">Vyvolat, zda je nalezeno více kompatibilních zálohovaných polí</span><span class="sxs-lookup"><span data-stu-id="c2eeb-508">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="c2eeb-509">Sledování problému #12523</span><span class="sxs-lookup"><span data-stu-id="c2eeb-509">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="c2eeb-510">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-510">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-511">Pokud se v EF Core 3,0 shodovalo více polí s pravidly pro hledání zálohovacího pole vlastnosti, bude jedno pole zvoleno na základě pořadí priorit.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-511">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="c2eeb-512">To může způsobit, že se nesprávné pole použije v nejednoznačných případech.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-512">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="c2eeb-513">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-513">**New behavior**</span></span>

<span data-ttu-id="c2eeb-514">Počínaje EF Core 3,0 platí, že pokud je více polí spárováno se stejnou vlastností, je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-514">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="c2eeb-515">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-515">**Why**</span></span>

<span data-ttu-id="c2eeb-516">Tato změna byla provedena, aby nedocházelo k tichému použití jednoho pole v případě, že může být pouze jedna z nich správná.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-516">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="c2eeb-517">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-517">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-518">Vlastnosti s nejednoznačnými zálohovacími poli musí obsahovat pole, které se má explicitně použít.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-518">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="c2eeb-519">Například pomocí rozhraní Fluent API:</span><span class="sxs-lookup"><span data-stu-id="c2eeb-519">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="c2eeb-520">Názvy vlastností pouze pro pole se musí shodovat s názvem pole.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-520">Field-only property names should match the field name</span></span>

<span data-ttu-id="c2eeb-521">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-521">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-522">Před EF Core 3,0 může být vlastnost určena hodnotou řetězce a v případě, že v typu .NET nebyla nalezena žádná vlastnost s tímto názvem, EF Core by se pokusila ji porovnat s polem pomocí pravidel konvence.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-522">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the .NET type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="c2eeb-523">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-523">**New behavior**</span></span>

<span data-ttu-id="c2eeb-524">Počínaje EF Core 3,0 se vlastnost pouze pole musí přesně shodovat s názvem pole.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-524">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="c2eeb-525">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-525">**Why**</span></span>

<span data-ttu-id="c2eeb-526">Tato změna byla provedena, aby se nepoužívalo stejné pole pro dvě vlastnosti s názvem podobně, ale také pravidla pro porovnání vlastností pouze pro pole, která jsou shodná s vlastnostmi mapovanými na vlastnosti CLR.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-526">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="c2eeb-527">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-527">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-528">Vlastnosti pouze polí musí být pojmenovány stejně jako pole, na které jsou namapována.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-528">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="c2eeb-529">V budoucí verzi EF Core po 3,0 plánujeme znovu povolit explicitní konfiguraci názvu pole, který se liší od názvu vlastnosti (viz téma věnované problému [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span><span class="sxs-lookup"><span data-stu-id="c2eeb-529">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="c2eeb-530">AddDbContext/AddDbContextPool už nevolá AddLogging a AddMemoryCache.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-530">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="c2eeb-531">Sledování problému #14756</span><span class="sxs-lookup"><span data-stu-id="c2eeb-531">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="c2eeb-532">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-532">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-533">Před EF Core 3,0 by volání `AddDbContext` nebo `AddDbContextPool` také registrovalo protokolování a služby ukládání do mezipaměti v paměti pomocí D. I přes volání [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) a [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="c2eeb-533">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="c2eeb-534">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-534">**New behavior**</span></span>

<span data-ttu-id="c2eeb-535">Počínaje EF Core 3,0 `AddDbContext` a `AddDbContextPool` nebudou nadále registrovat tyto služby se vkládáním závislostí (DI).</span><span class="sxs-lookup"><span data-stu-id="c2eeb-535">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="c2eeb-536">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-536">**Why**</span></span>

<span data-ttu-id="c2eeb-537">EF Core 3,0 nevyžaduje, aby se tyto služby nacházejí v kontejneru aplikace DI.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-537">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="c2eeb-538">Pokud je však `ILoggerFactory` registrována v kontejneru aplikace, bude nadále používána EF Core.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-538">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="c2eeb-539">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-539">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-540">Pokud vaše aplikace potřebuje tyto služby, zaregistrujte je explicitně pomocí kontejneru DI pomocí [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) nebo [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="c2eeb-540">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="c2eeb-541">DbContext. entry teď provádí místní DetectChanges.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-541">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="c2eeb-542">Sledování problému #13552</span><span class="sxs-lookup"><span data-stu-id="c2eeb-542">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="c2eeb-543">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-543">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-544">Před EF Core 3,0 by volání `DbContext.Entry` způsobilo zjištění změn všech sledovaných entit.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-544">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="c2eeb-545">Tím je zajištěno, že stav zpřístupněný v `EntityEntry` byl aktuální.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-545">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="c2eeb-546">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-546">**New behavior**</span></span>

<span data-ttu-id="c2eeb-547">Počínaje EF Core 3,0 se volání `DbContext.Entry` se nyní pokusí pouze detekovat změny v dané entitě a všech sledovaných hlavních entitách, které s ní souvisejí.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-547">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="c2eeb-548">To znamená, že změny jinde nemohly být zjištěny voláním této metody, což by mohlo mít vliv na stav aplikace.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-548">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="c2eeb-549">Všimněte si, že pokud je hodnota `ChangeTracker.AutoDetectChangesEnabled` nastavená na `false`, pak se toto místní zjišťování změn zakáže.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-549">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="c2eeb-550">Jiné metody, které způsobují detekci změn – například `ChangeTracker.Entries` a `SaveChanges`---a přesto způsobují kompletní `DetectChanges` všech sledovaných entit.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-550">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="c2eeb-551">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-551">**Why**</span></span>

<span data-ttu-id="c2eeb-552">Tato změna byla provedena za účelem zlepšení výchozího výkonu při použití `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-552">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="c2eeb-553">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-553">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-554">Před voláním `Entry` volejte `ChgangeTracker.DetectChanges()`, aby se zajistilo chování před 3,0.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-554">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="c2eeb-555">Klíče řetězce a pole bajtů nejsou ve výchozím nastavení generovány klientem.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-555">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="c2eeb-556">Sledování problému #14617</span><span class="sxs-lookup"><span data-stu-id="c2eeb-556">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="c2eeb-557">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-557">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-558">Před EF Core 3,0 lze použít vlastnosti klíče `string` a `byte[]` bez explicitního nastavení hodnoty, která není null.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-558">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="c2eeb-559">V takovém případě by se hodnota klíče vygenerovala na klientovi jako identifikátor GUID serializovaná na bajty pro `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-559">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="c2eeb-560">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-560">**New behavior**</span></span>

<span data-ttu-id="c2eeb-561">Počínaje EF Core 3,0 bude vyvolána výjimka oznamující, že nebyla nastavena žádná hodnota klíče.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-561">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="c2eeb-562">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-562">**Why**</span></span>

<span data-ttu-id="c2eeb-563">Tato změna byla provedena, protože hodnoty `string` @ no__t-1 @ no__t-2 generované klientem nejsou všeobecně užitečné a výchozí chování způsobilo obtížně generované hodnoty klíčů běžným způsobem.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-563">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="c2eeb-564">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-564">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-565">Chování před 3,0 lze získat explicitním určením, že klíčové vlastnosti by měly používat generované hodnoty, pokud není nastavena žádná jiná hodnota, která není null.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-565">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="c2eeb-566">Například s rozhraním API Fluent:</span><span class="sxs-lookup"><span data-stu-id="c2eeb-566">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="c2eeb-567">Nebo s poznámkami k datům:</span><span class="sxs-lookup"><span data-stu-id="c2eeb-567">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="c2eeb-568">ILoggerFactory je teď služba s vymezeným oborem.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-568">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="c2eeb-569">Sledování problému #14698</span><span class="sxs-lookup"><span data-stu-id="c2eeb-569">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="c2eeb-570">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-570">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-571">Před EF Core 3,0 byl `ILoggerFactory` zaregistrován jako služba s jedním prvkem.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-571">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="c2eeb-572">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-572">**New behavior**</span></span>

<span data-ttu-id="c2eeb-573">Počínaje EF Core 3,0 se nyní `ILoggerFactory` zaregistruje jako obor.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-573">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="c2eeb-574">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-574">**Why**</span></span>

<span data-ttu-id="c2eeb-575">Tato změna byla provedena, aby bylo možné povolit přidružení protokolovacího nástroje s instancí @no__t 0, která umožňuje další funkce a odstraňuje některé případy patologického chování, jako je například rozbalení interních poskytovatelů služeb.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-575">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="c2eeb-576">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-576">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-577">Tato změna by neměla mít vliv na kód aplikace, pokud se neregistruje a nepoužívá vlastní služby pro EF Core interního poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-577">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="c2eeb-578">To není běžné.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-578">This isn't common.</span></span>
<span data-ttu-id="c2eeb-579">V těchto případech bude většina věcí pořád fungovat, ale jakákoli služba typu Singleton, která byla závislá na `ILoggerFactory`, se musí změnit tak, aby `ILoggerFactory` získala jiným způsobem.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-579">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="c2eeb-580">Pokud narazíte na takové situace, zajistěte prosím problém na [EF Core sledování problémů GitHubu](https://github.com/aspnet/EntityFrameworkCore/issues) , abychom nás věděli, jak používáte `ILoggerFactory`, takže můžeme lépe porozumět tomu, jak to v budoucnu ještě Nerušit.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-580">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="c2eeb-581">Opožděné načítání proxy serverů už nepředpokládá navigační vlastnosti, jsou plně načtené.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-581">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="c2eeb-582">Sledování problému #12780</span><span class="sxs-lookup"><span data-stu-id="c2eeb-582">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="c2eeb-583">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-583">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-584">Před EF Core 3,0 se po odstranění `DbContext` nijak nevědělo, zda byla daná vlastnost navigace u entity získané z tohoto kontextu plně načtena nebo nikoli.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-584">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="c2eeb-585">Proxy místo toho předpokládají, že je načtena odkazová navigace, pokud má hodnotu jinou než null a že je načtena navigace kolekce, pokud není prázdná.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-585">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="c2eeb-586">V těchto případech by byl pokus o opožděné načtení no-op.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-586">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="c2eeb-587">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-587">**New behavior**</span></span>

<span data-ttu-id="c2eeb-588">Počínaje EF Core 3,0 budou proxy servery sledovat, zda je načtena vlastnost navigace.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-588">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="c2eeb-589">To znamená, že při pokusu o přístup k navigační vlastnosti, která je načtena po vyřazení kontextu, bude vždy ta no-op, i když je načtená navigace prázdná nebo má hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-589">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="c2eeb-590">Naopak při pokusu o přístup k navigační vlastnosti, která není načtená, vyvolá výjimku, pokud je kontext vyřazený, i když je vlastnost navigace neprázdná kolekce.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-590">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="c2eeb-591">Pokud nastane tato situace, znamená to, že se kód aplikace pokouší použít opožděné načítání v neplatném čase a aplikace by se měla změnit, aby to nevedlo.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-591">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="c2eeb-592">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-592">**Why**</span></span>

<span data-ttu-id="c2eeb-593">Tato změna byla provedena, aby při pokusu o opožděné načtení na uvolněnou instanci `DbContext` bylo chování konzistentní a správné.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-593">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="c2eeb-594">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-594">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-595">Aktualizujte kód aplikace, aby se nepokoušel opožděné načtení s odstraněným kontextem, nebo nastavte tuto hodnotu jako No-op, jak je popsáno ve zprávě výjimky.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-595">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="c2eeb-596">Nadměrné vytváření interních zprostředkovatelů služeb je teď ve výchozím nastavení chyba.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-596">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="c2eeb-597">Sledování problému #10236</span><span class="sxs-lookup"><span data-stu-id="c2eeb-597">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="c2eeb-598">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-598">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-599">Před EF Core 3,0 se pro aplikaci, která vytváří patologický počet interních poskytovatelů služeb, zaprotokoluje upozornění.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-599">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="c2eeb-600">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-600">**New behavior**</span></span>

<span data-ttu-id="c2eeb-601">Počínaje EF Core 3,0 je toto upozornění nyní považováno za chybu a je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-601">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="c2eeb-602">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-602">**Why**</span></span>

<span data-ttu-id="c2eeb-603">Tato změna byla provedená tak, že se tento patologický případ výslovně zveřejňuje tak, aby se zlepšil kód aplikace.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-603">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="c2eeb-604">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-604">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-605">Nejvhodnější příčinou této chyby je pochopení hlavní příčiny a zastavení vytváření, takže mnoho interních poskytovatelů služeb.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-605">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="c2eeb-606">Chybu však lze převést zpět na varování (nebo ignorováno) prostřednictvím konfigurace na `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-606">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="c2eeb-607">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c2eeb-607">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="c2eeb-608">Nové chování pro HasOne/HasMany se volá s jedním řetězcem.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-608">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="c2eeb-609">Sledování problému #9171</span><span class="sxs-lookup"><span data-stu-id="c2eeb-609">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="c2eeb-610">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-610">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-611">Před EF Core 3,0 byl kód volání `HasOne` nebo `HasMany` s jedním řetězcem interpretován jako matoucí.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-611">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="c2eeb-612">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c2eeb-612">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="c2eeb-613">Kód vypadá jako s tím, že se vztahuje `Samurai` na některý jiný typ entity pomocí navigační vlastnosti `Entrance`, která může být soukromá.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-613">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="c2eeb-614">Ve skutečnosti se tento kód pokusí vytvořit relaci k některému typu entity s názvem `Entrance` bez navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-614">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="c2eeb-615">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-615">**New behavior**</span></span>

<span data-ttu-id="c2eeb-616">Počínaje EF Core 3,0 výše uvedený kód má nyní podobný vzhled jako předtím.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-616">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="c2eeb-617">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-617">**Why**</span></span>

<span data-ttu-id="c2eeb-618">Staré chování bylo velmi matoucí, zejména při čtení konfiguračního kódu a hledání chyb.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-618">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="c2eeb-619">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-619">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-620">Tím dojde pouze k přerušení aplikací, které jsou explicitně konfigurovány pomocí řetězců pro názvy typů, a bez explicitního určení vlastnosti navigace.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-620">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="c2eeb-621">To není běžné.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-621">This is not common.</span></span>
<span data-ttu-id="c2eeb-622">Předchozí chování lze získat explicitním předáním `null` pro název vlastnosti navigace.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-622">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="c2eeb-623">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c2eeb-623">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="c2eeb-624">Návratový typ pro několik asynchronních metod byl změněn z úlohy na ValueTask</span><span class="sxs-lookup"><span data-stu-id="c2eeb-624">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="c2eeb-625">Sledování problému #15184</span><span class="sxs-lookup"><span data-stu-id="c2eeb-625">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="c2eeb-626">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-626">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-627">Následující asynchronní metody dříve vrátily `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="c2eeb-627">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="c2eeb-628">`ValueGenerator.NextValueAsync()` (a odvozování tříd)</span><span class="sxs-lookup"><span data-stu-id="c2eeb-628">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="c2eeb-629">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-629">**New behavior**</span></span>

<span data-ttu-id="c2eeb-630">Výše uvedené metody nyní vrací `ValueTask<T>` na stejný `T` jako předtím.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-630">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="c2eeb-631">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-631">**Why**</span></span>

<span data-ttu-id="c2eeb-632">Tato změna snižuje počet přidělení haldy, které vznikly při vyvolání těchto metod, což zlepšuje obecný výkon.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-632">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="c2eeb-633">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-633">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-634">Aplikace jednoduše čekají na rozhraní API, které je třeba znovu zkompilovat – nejsou nutné žádné změny zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-634">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="c2eeb-635">Složitější využití (například předání vráceného `Task` do `Task.WhenAny()`) obvykle vyžaduje, aby vrácený `ValueTask<T>` byl převeden na `Task<T>` voláním `AsTask()`.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-635">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="c2eeb-636">Všimněte si, že se tím sníží omezení přidělení, které tato změna přináší.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-636">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="c2eeb-637">Relační: anotace TypeMapping je nyní pouze TypeMapping</span><span class="sxs-lookup"><span data-stu-id="c2eeb-637">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="c2eeb-638">Sledování problému #9913</span><span class="sxs-lookup"><span data-stu-id="c2eeb-638">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="c2eeb-639">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-639">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-640">Název poznámky pro poznámky k mapování typů byl "relační: TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="c2eeb-640">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="c2eeb-641">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-641">**New behavior**</span></span>

<span data-ttu-id="c2eeb-642">Název poznámky pro mapování typů je nyní "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="c2eeb-642">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="c2eeb-643">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-643">**Why**</span></span>

<span data-ttu-id="c2eeb-644">Mapování typů se nyní používají pro více než stejné poskytovatele relačních databází.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-644">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="c2eeb-645">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-645">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-646">Tím dojde pouze k přerušení aplikací, které přistupují k mapování typu přímo jako anotaci, což není běžné.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-646">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="c2eeb-647">Nejvhodnější akcí pro opravu je použití prostoru rozhraní API pro přístup k mapování typů namísto použití anotace přímo.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-647">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="c2eeb-648">ToTable na odvozeném typu vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-648">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="c2eeb-649">Sledování problému #11811</span><span class="sxs-lookup"><span data-stu-id="c2eeb-649">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="c2eeb-650">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-650">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-651">Před EF Core 3,0 se `ToTable()` volání na odvozený typ ignoruje, protože pouze dědičnost dědění mapování je typu TPH, kde to není platné.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-651">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="c2eeb-652">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-652">**New behavior**</span></span>

<span data-ttu-id="c2eeb-653">Počínaje EF Core 3,0 a při přípravě na přidání podpory TPT a TPC v pozdější verzi teď `ToTable()` volána na odvozeném typu nyní vyvolá výjimku, aby se předešlo neočekávané změně mapování v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-653">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="c2eeb-654">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-654">**Why**</span></span>

<span data-ttu-id="c2eeb-655">V současné době není platný pro mapování odvozeného typu na jinou tabulku.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-655">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="c2eeb-656">Tato změna zabrání v budoucnosti v budoucnu, pokud se to stalo platným.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-656">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="c2eeb-657">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-657">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-658">Odeberte všechny pokusy o mapování odvozených typů na jiné tabulky.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-658">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="c2eeb-659">ForSqlServerHasIndex nahrazeno HasIndex</span><span class="sxs-lookup"><span data-stu-id="c2eeb-659">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="c2eeb-660">Sledování problému #12366</span><span class="sxs-lookup"><span data-stu-id="c2eeb-660">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="c2eeb-661">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-661">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-662">Před EF Core 3,0 `ForSqlServerHasIndex().ForSqlServerInclude()` poskytoval způsob, jak nakonfigurovat sloupce používané `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-662">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="c2eeb-663">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-663">**New behavior**</span></span>

<span data-ttu-id="c2eeb-664">Počínaje EF Core 3,0 se na relační úrovni teď podporuje použití `Include` na indexu.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-664">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="c2eeb-665">Použijte `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-665">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="c2eeb-666">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-666">**Why**</span></span>

<span data-ttu-id="c2eeb-667">Tato změna byla provedena za účelem konsolidace rozhraní API pro indexy s `Include` na jednom místě pro všechny poskytovatele databáze.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-667">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="c2eeb-668">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-668">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-669">Použijte nové rozhraní API, jak vidíte výše.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-669">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="c2eeb-670">Změny rozhraní API pro metadata</span><span class="sxs-lookup"><span data-stu-id="c2eeb-670">Metadata API changes</span></span>

[<span data-ttu-id="c2eeb-671">Sledování problému #214</span><span class="sxs-lookup"><span data-stu-id="c2eeb-671">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="c2eeb-672">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-672">**New behavior**</span></span>

<span data-ttu-id="c2eeb-673">Následující vlastnosti byly převedeny na rozšiřující metody:</span><span class="sxs-lookup"><span data-stu-id="c2eeb-673">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="c2eeb-674">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-674">**Why**</span></span>

<span data-ttu-id="c2eeb-675">Tato změna zjednodušuje implementaci výše uvedených rozhraní.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-675">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="c2eeb-676">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-676">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-677">Použijte nové metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-677">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="c2eeb-678">Změny rozhraní API pro konkrétního zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="c2eeb-678">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="c2eeb-679">Sledování problému #214</span><span class="sxs-lookup"><span data-stu-id="c2eeb-679">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="c2eeb-680">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-680">**New behavior**</span></span>

<span data-ttu-id="c2eeb-681">Metody rozšíření specifické pro poskytovatele budou shrnuty:</span><span class="sxs-lookup"><span data-stu-id="c2eeb-681">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="c2eeb-682">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-682">**Why**</span></span>

<span data-ttu-id="c2eeb-683">Tato změna zjednodušuje implementaci výše uvedených rozšiřujících metod.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-683">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="c2eeb-684">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-684">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-685">Použijte nové metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-685">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="c2eeb-686">EF Core už neposílá direktivu pragma pro vynucení KOFK SQLite.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-686">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="c2eeb-687">Sledování problému #12151</span><span class="sxs-lookup"><span data-stu-id="c2eeb-687">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="c2eeb-688">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-688">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-689">Před EF Core 3,0 by EF Core při otevření připojení k SQLite odeslal `PRAGMA foreign_keys = 1`.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-689">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="c2eeb-690">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-690">**New behavior**</span></span>

<span data-ttu-id="c2eeb-691">Počínaje EF Core 3,0 EF Core už při otevření připojení k SQLite neposílá `PRAGMA foreign_keys = 1`.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-691">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="c2eeb-692">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-692">**Why**</span></span>

<span data-ttu-id="c2eeb-693">Tato změna byla provedena, protože ve výchozím nastavení používá EF Core `SQLitePCLRaw.bundle_e_sqlite3`, což zase znamená, že je ve výchozím nastavení zapnuté vynucení CK a není nutné je explicitně povolit při každém otevření připojení.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-693">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="c2eeb-694">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-694">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-695">Cizí klíče jsou ve výchozím nastavení povolené v SQLitePCLRaw. bundle_e_sqlite3, který se ve výchozím nastavení používá pro EF Core.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-695">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="c2eeb-696">V ostatních případech je možné povolit cizí klíče zadáním `Foreign Keys=True` v připojovacím řetězci.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-696">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="c2eeb-697">Microsoft. EntityFrameworkCore. sqlite teď závisí na SQLitePCLRaw. bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="c2eeb-697">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="c2eeb-698">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-698">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-699">Před EF Core 3,0 EF Core použito `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-699">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="c2eeb-700">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-700">**New behavior**</span></span>

<span data-ttu-id="c2eeb-701">Počínaje EF Core 3,0 EF Core používá `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-701">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="c2eeb-702">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-702">**Why**</span></span>

<span data-ttu-id="c2eeb-703">Tato změna byla provedena tak, že verze SQLiteu použitá v iOS je konzistentní s jinými platformami.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-703">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="c2eeb-704">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-704">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-705">Chcete-li použít nativní verzi SQLite v iOS, nakonfigurujte `Microsoft.Data.Sqlite` pro použití jiné sady `SQLitePCLRaw`.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-705">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="c2eeb-706">Hodnoty GUID se teď ukládají jako TEXT na SQLite.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-706">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="c2eeb-707">Sledování problému #15078</span><span class="sxs-lookup"><span data-stu-id="c2eeb-707">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="c2eeb-708">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-708">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-709">Hodnoty GUID byly dříve uloženy jako hodnoty objektů BLOB u SQLite.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-709">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="c2eeb-710">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-710">**New behavior**</span></span>

<span data-ttu-id="c2eeb-711">Hodnoty GUID jsou nyní uloženy jako TEXT.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-711">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="c2eeb-712">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-712">**Why**</span></span>

<span data-ttu-id="c2eeb-713">Binární formát identifikátorů GUID není standardizovaný.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-713">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="c2eeb-714">Uložení hodnot jako textu zajistí, že databáze bude lépe kompatibilní s jinými technologiemi.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-714">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="c2eeb-715">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-715">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-716">Existující databáze můžete migrovat do nového formátu tím, že spustíte příkaz SQL podobně jako následující.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-716">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="c2eeb-717">V EF Core můžete i nadále používat předchozí chování nakonfigurováním převaděče hodnot na těchto vlastnostech.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-717">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="c2eeb-718">Microsoft. data. sqlite zůstává schopný přečítat hodnoty GUID z objektů BLOB a textových sloupců. vzhledem k tomu, že výchozí formát pro parametry a konstanty se změnil, bude pravděpodobně nutné provést akci u většiny scénářů, které zahrnují identifikátory GUID.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-718">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="c2eeb-719">Hodnoty char se teď ukládají jako TEXT na SQLite.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-719">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="c2eeb-720">Sledování problému #15020</span><span class="sxs-lookup"><span data-stu-id="c2eeb-720">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="c2eeb-721">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-721">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-722">Hodnoty typu char byly dříve sored jako CELOČÍSELNé hodnoty u SQLite.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-722">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="c2eeb-723">Například hodnota znaku *a* byla uložena jako celočíselná hodnota 65.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-723">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="c2eeb-724">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-724">**New behavior**</span></span>

<span data-ttu-id="c2eeb-725">Hodnoty typu char jsou nyní uloženy jako TEXT.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-725">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="c2eeb-726">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-726">**Why**</span></span>

<span data-ttu-id="c2eeb-727">Ukládání hodnot jako TEXT je přirozenější a databáze usnadňuje kompatibilitu s jinými technologiemi.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-727">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="c2eeb-728">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-728">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-729">Existující databáze můžete migrovat do nového formátu tím, že spustíte příkaz SQL podobně jako následující.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-729">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="c2eeb-730">V EF Core můžete i nadále používat předchozí chování nakonfigurováním převaděče hodnot na těchto vlastnostech.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-730">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="c2eeb-731">Microsoft. data. sqlite také zůstává schopný číst znakové hodnoty z CELOČÍSELNého i TEXTOVÉHO sloupce, takže některé scénáře nemusí vyžadovat žádnou akci.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-731">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="c2eeb-732">ID migrace se teď generují pomocí kalendáře invariantní jazykové verze.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-732">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="c2eeb-733">Sledování problému #12978</span><span class="sxs-lookup"><span data-stu-id="c2eeb-733">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="c2eeb-734">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-734">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-735">ID migrace se nechtěně vygenerovala pomocí kalendáře aktuální jazykové verze.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-735">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="c2eeb-736">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-736">**New behavior**</span></span>

<span data-ttu-id="c2eeb-737">ID migrace se nyní vždy generují pomocí kalendáře neutrální jazykové verze (gregoriánský).</span><span class="sxs-lookup"><span data-stu-id="c2eeb-737">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="c2eeb-738">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-738">**Why**</span></span>

<span data-ttu-id="c2eeb-739">Pořadí migrace je důležité při aktualizaci databáze nebo řešení konfliktů při slučování.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-739">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="c2eeb-740">Pomocí invariantního kalendáře se vyhnete problémům s řazením, které mohou být výsledkem členů týmu jiné systémové kalendáře.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-740">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="c2eeb-741">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-741">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-742">Tato změna má vliv na kohokoli, kdo používá negregoriánský kalendář, ve kterém je rok větší než gregoriánský kalendář (například thajský buddhistický kalendář).</span><span class="sxs-lookup"><span data-stu-id="c2eeb-742">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="c2eeb-743">Existující identifikátory migrace se budou muset aktualizovat, aby se nové migrace objednaly po stávajících migracích.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-743">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="c2eeb-744">ID migrace najdete v atributu migrace v souborech návrháře migrace.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-744">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="c2eeb-745">Také je potřeba aktualizovat tabulku historie migrace.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-745">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="c2eeb-746">UseRowNumberForPaging se odebral.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-746">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="c2eeb-747">Sledování problému #16400</span><span class="sxs-lookup"><span data-stu-id="c2eeb-747">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="c2eeb-748">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-748">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-749">Před EF Core 3,0 se `UseRowNumberForPaging` dá použít k vygenerování SQL pro stránkování, které je kompatibilní s SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-749">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="c2eeb-750">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-750">**New behavior**</span></span>

<span data-ttu-id="c2eeb-751">Počínaje EF Core 3,0 bude EF generovat pouze SQL pro stránkování, které je kompatibilní pouze s novějšími verzemi SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-751">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="c2eeb-752">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-752">**Why**</span></span>

<span data-ttu-id="c2eeb-753">Tuto změnu provedeme, protože [SQL Server 2008 už není podporovaným produktem](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) a aktualizace této funkce tak, aby fungovala se změnami dotazů provedenými v EF Core 3,0 je významná práce.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-753">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="c2eeb-754">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-754">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-755">Doporučujeme aktualizovat na novější verzi SQL Server nebo pomocí vyšší úrovně kompatibility, aby byl vygenerovaný SQL podporován.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-755">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="c2eeb-756">To znamená, že pokud to nemůžete udělat, [komentář k problému s sledováním](https://github.com/aspnet/EntityFrameworkCore/issues/16400) najdete v podrobnostech.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-756">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="c2eeb-757">Toto rozhodnutí můžeme znovu navštívit na základě zpětné vazby.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-757">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="c2eeb-758">Informace o rozšíření/metadata se odebraly z IDbContextOptionsExtension.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-758">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="c2eeb-759">Sledování problému #16119</span><span class="sxs-lookup"><span data-stu-id="c2eeb-759">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="c2eeb-760">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-760">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-761">`IDbContextOptionsExtension` obsahuje metody pro poskytování metadat o rozšíření.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-761">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="c2eeb-762">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-762">**New behavior**</span></span>

<span data-ttu-id="c2eeb-763">Tyto metody byly přesunuty do nové abstraktní základní třídy `DbContextOptionsExtensionInfo`, která je vrácena z nové vlastnosti `IDbContextOptionsExtension.Info`.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-763">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="c2eeb-764">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-764">**Why**</span></span>

<span data-ttu-id="c2eeb-765">V rámci vydání od 2,0 do 3,0 jsme potřebovali přidat nebo změnit tyto metody několikrát.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-765">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="c2eeb-766">Rozbalením do nové abstraktní základní třídy bude snazší vytvořit tyto změny bez přerušení stávajících rozšíření.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-766">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="c2eeb-767">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-767">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-768">Aktualizovat rozšíření tak, aby následovala nový vzor.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-768">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="c2eeb-769">Příklady najdete v mnoha implementacích `IDbContextOptionsExtension` pro různé druhy rozšíření ve zdrojovém kódu EF Core.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-769">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="c2eeb-770">LogQueryPossibleExceptionWithAggregateOperator se přejmenovalo.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-770">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="c2eeb-771">Sledování problému #10985</span><span class="sxs-lookup"><span data-stu-id="c2eeb-771">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="c2eeb-772">**Mění**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-772">**Change**</span></span>

<span data-ttu-id="c2eeb-773">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` bylo přejmenováno na `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-773">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="c2eeb-774">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-774">**Why**</span></span>

<span data-ttu-id="c2eeb-775">Zarovná pojmenování této události varování se všemi ostatními událostmi upozornění.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-775">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="c2eeb-776">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-776">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-777">Použijte nový název.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-777">Use the new name.</span></span> <span data-ttu-id="c2eeb-778">(Všimněte si, že číslo ID události se nezměnilo.)</span><span class="sxs-lookup"><span data-stu-id="c2eeb-778">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="c2eeb-779">Vysvětlení rozhraní API pro názvy omezení cizího klíče</span><span class="sxs-lookup"><span data-stu-id="c2eeb-779">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="c2eeb-780">Sledování problému #10730</span><span class="sxs-lookup"><span data-stu-id="c2eeb-780">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="c2eeb-781">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-781">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-782">Před EF Core 3,0 se názvy omezení cizího klíče odkazovaly jenom na "název".</span><span class="sxs-lookup"><span data-stu-id="c2eeb-782">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="c2eeb-783">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c2eeb-783">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="c2eeb-784">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-784">**New behavior**</span></span>

<span data-ttu-id="c2eeb-785">Počínaje EF Core 3,0 se názvy omezení cizích klíčů teď označují jako "název omezení".</span><span class="sxs-lookup"><span data-stu-id="c2eeb-785">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="c2eeb-786">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c2eeb-786">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="c2eeb-787">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-787">**Why**</span></span>

<span data-ttu-id="c2eeb-788">Tato změna přináší konzistenci pro pojmenování v této oblasti a také vysvětluje, že se jedná o název omezení cizího klíče, a nikoli název sloupce nebo vlastnosti, ve kterém je definován cizí klíč.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-788">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="c2eeb-789">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-789">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-790">Použijte nový název.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-790">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="c2eeb-791">IRelationalDatabaseCreator. HasTables/HasTablesAsync byly zveřejněny.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-791">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="c2eeb-792">Sledování problému #15997</span><span class="sxs-lookup"><span data-stu-id="c2eeb-792">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="c2eeb-793">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-793">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-794">Před EF Core 3,0 byly tyto metody chráněné.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-794">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="c2eeb-795">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-795">**New behavior**</span></span>

<span data-ttu-id="c2eeb-796">Počínaje EF Core 3,0 jsou tyto metody veřejné.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-796">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="c2eeb-797">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-797">**Why**</span></span>

<span data-ttu-id="c2eeb-798">Tyto metody jsou používány EF k určení, jestli je databáze vytvořená, ale prázdná.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-798">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="c2eeb-799">To může být užitečné taky od vnějšího EF při určování, jestli se mají migrace použít.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-799">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="c2eeb-800">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-800">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-801">Změňte přístupnost všech přepsání.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-801">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="c2eeb-802">Microsoft. EntityFrameworkCore. Design je teď balíček DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-802">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="c2eeb-803">Sledování problému #11506</span><span class="sxs-lookup"><span data-stu-id="c2eeb-803">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="c2eeb-804">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-804">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-805">Před EF Core 3,0 byl Microsoft. EntityFrameworkCore. Design regulárním balíčkem NuGet, na kterém by mohly být na sestavení odkazovány projekty, které na něm závisejí.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-805">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="c2eeb-806">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-806">**New behavior**</span></span>

<span data-ttu-id="c2eeb-807">Počínaje EF Core 3,0 se jedná o balíček DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-807">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="c2eeb-808">To znamená, že závislost nebude nijak přesměrovat do jiných projektů a že již nemůžete ve výchozím nastavení odkazovat na své sestavení.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-808">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="c2eeb-809">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-809">**Why**</span></span>

<span data-ttu-id="c2eeb-810">Tento balíček se má použít jenom v době návrhu.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-810">This package is only intended to be used at design time.</span></span> <span data-ttu-id="c2eeb-811">Nasazené aplikace by neměli na ni odkazovat.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-811">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="c2eeb-812">Díky tomu, že balíček DevelopmentDependency, toto doporučení posiluje.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-812">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="c2eeb-813">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-813">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-814">Pokud potřebujete odkazovat na tento balíček, aby bylo možné přepsat EF Core chování při návrhu, můžete aktualizovat metadata položky aktualizovat PackageReference v projektu.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-814">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="c2eeb-815">Pokud se na balíček odkazuje přes Microsoft. EntityFrameworkCore. Tools, budete muset do balíčku přidat explicitní PackageReference, aby se změnila jeho metadata.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-815">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="c2eeb-816">SQLitePCL. Raw aktualizováno na verzi 2.0.0</span><span class="sxs-lookup"><span data-stu-id="c2eeb-816">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="c2eeb-817">Sledování problému #14824</span><span class="sxs-lookup"><span data-stu-id="c2eeb-817">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="c2eeb-818">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-818">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-819">Microsoft. EntityFrameworkCore. sqlite byl dřív závislý na 1.1.12 verze SQLitePCL. Raw.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-819">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="c2eeb-820">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-820">**New behavior**</span></span>

<span data-ttu-id="c2eeb-821">Náš balíček jsme aktualizovali tak, aby byl závislý na verzi 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-821">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="c2eeb-822">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-822">**Why**</span></span>

<span data-ttu-id="c2eeb-823">2\.0.0 verze SQLitePCL. Raw TARGETS .NET Standard 2,0.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-823">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="c2eeb-824">Dříve cílí na .NET Standard 1,1, které vyžadovaly, aby se při práci vytvořil velký uzávěr přenosných balíčků.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-824">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="c2eeb-825">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-825">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-826">SQLitePCL. Raw 2.0.0 verze obsahuje některé zásadní změny.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-826">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="c2eeb-827">Podrobnosti najdete v [poznámkách k verzi](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) .</span><span class="sxs-lookup"><span data-stu-id="c2eeb-827">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="c2eeb-828">NetTopologySuite aktualizace na verzi 2.0.0</span><span class="sxs-lookup"><span data-stu-id="c2eeb-828">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="c2eeb-829">Sledování problému #14825</span><span class="sxs-lookup"><span data-stu-id="c2eeb-829">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="c2eeb-830">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-830">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-831">Prostorové balíčky byly dříve závislé na 1.15.1 verze NetTopologySuite.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-831">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="c2eeb-832">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-832">**New behavior**</span></span>

<span data-ttu-id="c2eeb-833">Náš balíček jsme aktualizovali tak, aby byl závislý na verzi 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-833">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="c2eeb-834">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-834">**Why**</span></span>

<span data-ttu-id="c2eeb-835">2\.0.0 verze NetTopologySuite má za cíl řešit několik problémů s použitelností, ke kterým se EF Core uživatelé setkali.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-835">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="c2eeb-836">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-836">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-837">NetTopologySuite verze 2.0.0 obsahuje některé průlomové změny.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-837">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="c2eeb-838">Podrobnosti najdete v [poznámkách k verzi](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) .</span><span class="sxs-lookup"><span data-stu-id="c2eeb-838">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="c2eeb-839">Je nutné nakonfigurovat více dvojznačných relací odkazujících na sebe.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-839">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="c2eeb-840">Sledování problému #13573</span><span class="sxs-lookup"><span data-stu-id="c2eeb-840">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="c2eeb-841">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-841">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-842">Typ entity s více jednosměrnou navigační vlastností a s vyhovující FKs byl nesprávně nakonfigurován jako jeden vztah.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-842">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="c2eeb-843">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c2eeb-843">For example:</span></span>

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

<span data-ttu-id="c2eeb-844">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-844">**New behavior**</span></span>

<span data-ttu-id="c2eeb-845">Tento scénář je nyní zjištěn v sestavování modelu a je vyvolána výjimka označující, že je model dvojznačný.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-845">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="c2eeb-846">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-846">**Why**</span></span>

<span data-ttu-id="c2eeb-847">Výsledný model byl nejednoznačný a pravděpodobně bude pro tento případ obvykle špatný.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-847">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="c2eeb-848">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-848">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-849">Použijte úplnou konfiguraci relace.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-849">Use full configuration of the relationship.</span></span> <span data-ttu-id="c2eeb-850">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c2eeb-850">For example:</span></span>

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
### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a><span data-ttu-id="c2eeb-851">DbFunction. Schema má hodnotu null nebo je prázdný řetězec, který nakonfiguruje, aby byl ve výchozím schématu modelu.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-851">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>

[<span data-ttu-id="c2eeb-852">Sledování problému #12757</span><span class="sxs-lookup"><span data-stu-id="c2eeb-852">Tracking Issue #12757</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12757)

<span data-ttu-id="c2eeb-853">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-853">**Old behavior**</span></span>

<span data-ttu-id="c2eeb-854">DbFunction nakonfigurovaný se schématem jako prázdný řetězec byl považován za vestavěnou funkci bez schématu.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-854">A DbFunction configured with schema as an empty string was treated as built-in function without a schema.</span></span> <span data-ttu-id="c2eeb-855">Například následující kód bude mapován `DatePart` funkce CLR na `DATEPART` vestavěnou funkci na SqlServer.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-855">For example following code will map `DatePart` CLR function to `DATEPART` built-in function on SqlServer.</span></span>

```C#
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

<span data-ttu-id="c2eeb-856">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-856">**New behavior**</span></span>

<span data-ttu-id="c2eeb-857">Všechna mapování DbFunction se považují za namapovaná na uživatelsky definované funkce.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-857">All DbFunction mappings are considered to be mapped to user defined functions.</span></span> <span data-ttu-id="c2eeb-858">Proto je prázdná hodnota řetězce vložena do výchozího schématu pro model.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-858">Hence empty string value would put the function inside the default schema for the model.</span></span> <span data-ttu-id="c2eeb-859">V opačném případě se může jednat o schéma nakonfigurované explicitně prostřednictvím rozhraní Fluent API `modelBuilder.HasDefaultSchema()` nebo `dbo`.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-859">Which could be the schema configured explicitly via fluent API `modelBuilder.HasDefaultSchema()` or `dbo` otherwise.</span></span>

<span data-ttu-id="c2eeb-860">**Proč**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-860">**Why**</span></span>

<span data-ttu-id="c2eeb-861">Dříve prázdné schéma bylo způsobem, jak se zacházet s touto funkcí, ale tato funkce je k dispozici pouze pro SqlServer, kde předdefinované funkce nepatří do žádného schématu.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-861">Previously schema being empty was a way to treat that function is built-in but that logic is only applicable for SqlServer where built-in functions do not belong to any schema.</span></span>

<span data-ttu-id="c2eeb-862">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="c2eeb-862">**Mitigations**</span></span>

<span data-ttu-id="c2eeb-863">Nakonfigurujte převod DbFunction ručně, abyste ho namapovali na vestavěnou funkci.</span><span class="sxs-lookup"><span data-stu-id="c2eeb-863">Configure DbFunction's translation manually to map it to a built-in function.</span></span>

```C#
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```
