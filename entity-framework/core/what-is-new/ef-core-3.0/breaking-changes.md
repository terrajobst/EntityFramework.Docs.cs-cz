---
title: Přerušující změny v EF Core 3,0 – EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: f02825f5303959997dca6e14e4efe64020b3cb22
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655881"
---
# <a name="breaking-changes-included-in-ef-core-30"></a><span data-ttu-id="b5388-102">Přerušující změny zahrnuté v EF Core 3,0</span><span class="sxs-lookup"><span data-stu-id="b5388-102">Breaking changes included in EF Core 3.0</span></span>

<span data-ttu-id="b5388-103">Následující změny rozhraní API a chování mají možnost rušit existující aplikace při jejich upgradu na 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="b5388-103">The following API and behavior changes have the potential to break existing applications when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="b5388-104">Změny, které očekáváme jenom o to, aby ovlivnili pouze poskytovatele databází, jsou popsané v části [změny zprostředkovatele](xref:core/providers/provider-log).</span><span class="sxs-lookup"><span data-stu-id="b5388-104">Changes that we expect to only impact database providers are documented under [provider changes](xref:core/providers/provider-log).</span></span>

## <a name="summary"></a><span data-ttu-id="b5388-105">Souhrn</span><span class="sxs-lookup"><span data-stu-id="b5388-105">Summary</span></span>

| <span data-ttu-id="b5388-106">**Zásadní změna**</span><span class="sxs-lookup"><span data-stu-id="b5388-106">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="b5388-107">**Vliv**</span><span class="sxs-lookup"><span data-stu-id="b5388-107">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="b5388-108">Dotazy LINQ již nejsou vyhodnocovány na klientovi.</span><span class="sxs-lookup"><span data-stu-id="b5388-108">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="b5388-109">Maximální</span><span class="sxs-lookup"><span data-stu-id="b5388-109">High</span></span>       |
| [<span data-ttu-id="b5388-110">EF Core 3,0 cíle .NET Standard 2,1 místo .NET Standard 2,0</span><span class="sxs-lookup"><span data-stu-id="b5388-110">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="b5388-111">Maximální</span><span class="sxs-lookup"><span data-stu-id="b5388-111">High</span></span>      |
| [<span data-ttu-id="b5388-112">EF Core nástroj příkazového řádku dotnet EF již není součástí .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="b5388-112">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="b5388-113">Maximální</span><span class="sxs-lookup"><span data-stu-id="b5388-113">High</span></span>      |
| [<span data-ttu-id="b5388-114">DetectChanges respektuje hodnoty klíčů generované úložištěm.</span><span class="sxs-lookup"><span data-stu-id="b5388-114">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="b5388-115">Maximální</span><span class="sxs-lookup"><span data-stu-id="b5388-115">High</span></span>      |
| [<span data-ttu-id="b5388-116">Z tabulek, ExecuteSql a ExecuteSqlAsync byly přejmenovány.</span><span class="sxs-lookup"><span data-stu-id="b5388-116">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="b5388-117">Maximální</span><span class="sxs-lookup"><span data-stu-id="b5388-117">High</span></span>      |
| [<span data-ttu-id="b5388-118">Typy dotazů jsou konsolidovány s typy entit</span><span class="sxs-lookup"><span data-stu-id="b5388-118">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="b5388-119">Maximální</span><span class="sxs-lookup"><span data-stu-id="b5388-119">High</span></span>      |
| [<span data-ttu-id="b5388-120">Entity Framework Core už není součástí sdílené ASP.NET Core architektury.</span><span class="sxs-lookup"><span data-stu-id="b5388-120">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="b5388-121">Úrovně</span><span class="sxs-lookup"><span data-stu-id="b5388-121">Medium</span></span>      |
| [<span data-ttu-id="b5388-122">Odstranění kaskádových operací se teď ve výchozím nastavení provádí hned.</span><span class="sxs-lookup"><span data-stu-id="b5388-122">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="b5388-123">Úrovně</span><span class="sxs-lookup"><span data-stu-id="b5388-123">Medium</span></span>      |
| [<span data-ttu-id="b5388-124">Eager načítání souvisejících entit se teď děje v jednom dotazu.</span><span class="sxs-lookup"><span data-stu-id="b5388-124">Eager loading of related entities now happens in a single query</span></span>](#eager-loading-single-query) | <span data-ttu-id="b5388-125">Úrovně</span><span class="sxs-lookup"><span data-stu-id="b5388-125">Medium</span></span>      |
| [<span data-ttu-id="b5388-126">DeleteBehavior. restrict má sémantiku čištění.</span><span class="sxs-lookup"><span data-stu-id="b5388-126">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="b5388-127">Úrovně</span><span class="sxs-lookup"><span data-stu-id="b5388-127">Medium</span></span>      |
| [<span data-ttu-id="b5388-128">Změnilo se konfigurační rozhraní API pro vztahy vlastněných typů.</span><span class="sxs-lookup"><span data-stu-id="b5388-128">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="b5388-129">Úrovně</span><span class="sxs-lookup"><span data-stu-id="b5388-129">Medium</span></span>      |
| [<span data-ttu-id="b5388-130">Každá vlastnost používá nezávislou generaci celočíselného klíče v paměti.</span><span class="sxs-lookup"><span data-stu-id="b5388-130">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="b5388-131">Úrovně</span><span class="sxs-lookup"><span data-stu-id="b5388-131">Medium</span></span>      |
| [<span data-ttu-id="b5388-132">Žádné dotazy pro sledování neprovádějí překlad identity</span><span class="sxs-lookup"><span data-stu-id="b5388-132">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="b5388-133">Úrovně</span><span class="sxs-lookup"><span data-stu-id="b5388-133">Medium</span></span>      |
| [<span data-ttu-id="b5388-134">Změny rozhraní API pro metadata</span><span class="sxs-lookup"><span data-stu-id="b5388-134">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="b5388-135">Úrovně</span><span class="sxs-lookup"><span data-stu-id="b5388-135">Medium</span></span>      |
| [<span data-ttu-id="b5388-136">Změny rozhraní API pro konkrétního zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="b5388-136">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="b5388-137">Úrovně</span><span class="sxs-lookup"><span data-stu-id="b5388-137">Medium</span></span>      |
| [<span data-ttu-id="b5388-138">UseRowNumberForPaging se odebral.</span><span class="sxs-lookup"><span data-stu-id="b5388-138">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="b5388-139">Úrovně</span><span class="sxs-lookup"><span data-stu-id="b5388-139">Medium</span></span>      |
| [<span data-ttu-id="b5388-140">Metoda Z tabulek při použití s uloženou procedurou nemůže být složená.</span><span class="sxs-lookup"><span data-stu-id="b5388-140">FromSql method when used with stored procedure cannot be composed</span></span>](#fromsqlsproc) | <span data-ttu-id="b5388-141">Úrovně</span><span class="sxs-lookup"><span data-stu-id="b5388-141">Medium</span></span>      |
| [<span data-ttu-id="b5388-142">Metody Z tabulek se dají zadat jenom v kořenech dotazů.</span><span class="sxs-lookup"><span data-stu-id="b5388-142">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="b5388-143">Slab</span><span class="sxs-lookup"><span data-stu-id="b5388-143">Low</span></span>      |
| [<span data-ttu-id="b5388-144">~~Provádění dotazu se protokoluje na úrovni ladění~~ . Vrátit</span><span class="sxs-lookup"><span data-stu-id="b5388-144">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="b5388-145">Slab</span><span class="sxs-lookup"><span data-stu-id="b5388-145">Low</span></span>      |
| [<span data-ttu-id="b5388-146">Dočasné hodnoty klíčů už nejsou nastavené na instance entit.</span><span class="sxs-lookup"><span data-stu-id="b5388-146">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="b5388-147">Slab</span><span class="sxs-lookup"><span data-stu-id="b5388-147">Low</span></span>      |
| [<span data-ttu-id="b5388-148">Závislé entity, které sdílí tabulku s objektem zabezpečení, jsou teď volitelné.</span><span class="sxs-lookup"><span data-stu-id="b5388-148">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="b5388-149">Slab</span><span class="sxs-lookup"><span data-stu-id="b5388-149">Low</span></span>      |
| [<span data-ttu-id="b5388-150">Všechny entity sdílející tabulku se sloupcem souběžného tokenu musí být namapovány na vlastnost.</span><span class="sxs-lookup"><span data-stu-id="b5388-150">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="b5388-151">Slab</span><span class="sxs-lookup"><span data-stu-id="b5388-151">Low</span></span>      |
| [<span data-ttu-id="b5388-152">Zděděné vlastnosti z nemapovaných typů jsou nyní namapovány na jeden sloupec pro všechny odvozené typy.</span><span class="sxs-lookup"><span data-stu-id="b5388-152">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="b5388-153">Slab</span><span class="sxs-lookup"><span data-stu-id="b5388-153">Low</span></span>      |
| [<span data-ttu-id="b5388-154">Konvence vlastností cizího klíče už neodpovídá stejnému názvu jako vlastnost Principal.</span><span class="sxs-lookup"><span data-stu-id="b5388-154">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="b5388-155">Slab</span><span class="sxs-lookup"><span data-stu-id="b5388-155">Low</span></span>      |
| [<span data-ttu-id="b5388-156">Připojení k databázi je teď zavřené, pokud už nepoužíváte dřív, než se dokončí jeho objekt TransactionScope.</span><span class="sxs-lookup"><span data-stu-id="b5388-156">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="b5388-157">Slab</span><span class="sxs-lookup"><span data-stu-id="b5388-157">Low</span></span>      |
| [<span data-ttu-id="b5388-158">Ve výchozím nastavení se používají pole pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="b5388-158">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="b5388-159">Slab</span><span class="sxs-lookup"><span data-stu-id="b5388-159">Low</span></span>      |
| [<span data-ttu-id="b5388-160">Vyvolat, zda je nalezeno více kompatibilních zálohovaných polí</span><span class="sxs-lookup"><span data-stu-id="b5388-160">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="b5388-161">Slab</span><span class="sxs-lookup"><span data-stu-id="b5388-161">Low</span></span>      |
| [<span data-ttu-id="b5388-162">Názvy vlastností pouze pro pole se musí shodovat s názvem pole.</span><span class="sxs-lookup"><span data-stu-id="b5388-162">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="b5388-163">Slab</span><span class="sxs-lookup"><span data-stu-id="b5388-163">Low</span></span>      |
| [<span data-ttu-id="b5388-164">AddDbContext/AddDbContextPool už nevolá AddLogging a AddMemoryCache.</span><span class="sxs-lookup"><span data-stu-id="b5388-164">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="b5388-165">Slab</span><span class="sxs-lookup"><span data-stu-id="b5388-165">Low</span></span>      |
| [<span data-ttu-id="b5388-166">DbContext. entry teď provádí místní DetectChanges.</span><span class="sxs-lookup"><span data-stu-id="b5388-166">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="b5388-167">Slab</span><span class="sxs-lookup"><span data-stu-id="b5388-167">Low</span></span>      |
| [<span data-ttu-id="b5388-168">Klíče řetězce a pole bajtů nejsou ve výchozím nastavení generovány klientem.</span><span class="sxs-lookup"><span data-stu-id="b5388-168">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="b5388-169">Slab</span><span class="sxs-lookup"><span data-stu-id="b5388-169">Low</span></span>      |
| [<span data-ttu-id="b5388-170">ILoggerFactory je teď služba s vymezeným oborem.</span><span class="sxs-lookup"><span data-stu-id="b5388-170">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="b5388-171">Slab</span><span class="sxs-lookup"><span data-stu-id="b5388-171">Low</span></span>      |
| [<span data-ttu-id="b5388-172">Opožděné načítání proxy serverů už nepředpokládá navigační vlastnosti, jsou plně načtené.</span><span class="sxs-lookup"><span data-stu-id="b5388-172">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="b5388-173">Slab</span><span class="sxs-lookup"><span data-stu-id="b5388-173">Low</span></span>      |
| [<span data-ttu-id="b5388-174">Nadměrné vytváření interních zprostředkovatelů služeb je teď ve výchozím nastavení chyba.</span><span class="sxs-lookup"><span data-stu-id="b5388-174">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="b5388-175">Slab</span><span class="sxs-lookup"><span data-stu-id="b5388-175">Low</span></span>      |
| [<span data-ttu-id="b5388-176">Nové chování pro HasOne/HasMany se volá s jedním řetězcem.</span><span class="sxs-lookup"><span data-stu-id="b5388-176">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="b5388-177">Slab</span><span class="sxs-lookup"><span data-stu-id="b5388-177">Low</span></span>      |
| [<span data-ttu-id="b5388-178">Návratový typ pro několik asynchronních metod byl změněn z úlohy na ValueTask</span><span class="sxs-lookup"><span data-stu-id="b5388-178">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="b5388-179">Slab</span><span class="sxs-lookup"><span data-stu-id="b5388-179">Low</span></span>      |
| [<span data-ttu-id="b5388-180">Relační: anotace TypeMapping je nyní pouze TypeMapping</span><span class="sxs-lookup"><span data-stu-id="b5388-180">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="b5388-181">Slab</span><span class="sxs-lookup"><span data-stu-id="b5388-181">Low</span></span>      |
| [<span data-ttu-id="b5388-182">ToTable na odvozeném typu vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="b5388-182">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="b5388-183">Slab</span><span class="sxs-lookup"><span data-stu-id="b5388-183">Low</span></span>      |
| [<span data-ttu-id="b5388-184">EF Core už neposílá direktivu pragma pro vynucení KOFK SQLite.</span><span class="sxs-lookup"><span data-stu-id="b5388-184">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="b5388-185">Slab</span><span class="sxs-lookup"><span data-stu-id="b5388-185">Low</span></span>      |
| [<span data-ttu-id="b5388-186">Microsoft. EntityFrameworkCore. sqlite teď závisí na SQLitePCLRaw. bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="b5388-186">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="b5388-187">Slab</span><span class="sxs-lookup"><span data-stu-id="b5388-187">Low</span></span>      |
| [<span data-ttu-id="b5388-188">Hodnoty GUID se teď ukládají jako TEXT na SQLite.</span><span class="sxs-lookup"><span data-stu-id="b5388-188">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="b5388-189">Slab</span><span class="sxs-lookup"><span data-stu-id="b5388-189">Low</span></span>      |
| [<span data-ttu-id="b5388-190">Hodnoty char se teď ukládají jako TEXT na SQLite.</span><span class="sxs-lookup"><span data-stu-id="b5388-190">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="b5388-191">Slab</span><span class="sxs-lookup"><span data-stu-id="b5388-191">Low</span></span>      |
| [<span data-ttu-id="b5388-192">ID migrace se teď generují pomocí kalendáře invariantní jazykové verze.</span><span class="sxs-lookup"><span data-stu-id="b5388-192">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="b5388-193">Slab</span><span class="sxs-lookup"><span data-stu-id="b5388-193">Low</span></span>      |
| [<span data-ttu-id="b5388-194">Informace o rozšíření/metadata se odebraly z IDbContextOptionsExtension.</span><span class="sxs-lookup"><span data-stu-id="b5388-194">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="b5388-195">Slab</span><span class="sxs-lookup"><span data-stu-id="b5388-195">Low</span></span>      |
| [<span data-ttu-id="b5388-196">LogQueryPossibleExceptionWithAggregateOperator se přejmenovalo.</span><span class="sxs-lookup"><span data-stu-id="b5388-196">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="b5388-197">Slab</span><span class="sxs-lookup"><span data-stu-id="b5388-197">Low</span></span>      |
| [<span data-ttu-id="b5388-198">Vysvětlení rozhraní API pro názvy omezení cizího klíče</span><span class="sxs-lookup"><span data-stu-id="b5388-198">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="b5388-199">Slab</span><span class="sxs-lookup"><span data-stu-id="b5388-199">Low</span></span>      |
| [<span data-ttu-id="b5388-200">IRelationalDatabaseCreator. HasTables/HasTablesAsync byly zveřejněny.</span><span class="sxs-lookup"><span data-stu-id="b5388-200">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="b5388-201">Slab</span><span class="sxs-lookup"><span data-stu-id="b5388-201">Low</span></span>      |
| [<span data-ttu-id="b5388-202">Microsoft. EntityFrameworkCore. Design je teď balíček DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="b5388-202">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="b5388-203">Slab</span><span class="sxs-lookup"><span data-stu-id="b5388-203">Low</span></span>      |
| [<span data-ttu-id="b5388-204">SQLitePCL. Raw aktualizováno na verzi 2.0.0</span><span class="sxs-lookup"><span data-stu-id="b5388-204">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="b5388-205">Slab</span><span class="sxs-lookup"><span data-stu-id="b5388-205">Low</span></span>      |
| [<span data-ttu-id="b5388-206">NetTopologySuite aktualizace na verzi 2.0.0</span><span class="sxs-lookup"><span data-stu-id="b5388-206">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="b5388-207">Slab</span><span class="sxs-lookup"><span data-stu-id="b5388-207">Low</span></span>      |
| [<span data-ttu-id="b5388-208">Místo typu System. data. SqlClient se používá Microsoft. data. SqlClient.</span><span class="sxs-lookup"><span data-stu-id="b5388-208">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>](#SqlClient) | <span data-ttu-id="b5388-209">Slab</span><span class="sxs-lookup"><span data-stu-id="b5388-209">Low</span></span>      |
| [<span data-ttu-id="b5388-210">Je nutné nakonfigurovat více dvojznačných relací odkazujících na sebe.</span><span class="sxs-lookup"><span data-stu-id="b5388-210">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="b5388-211">Slab</span><span class="sxs-lookup"><span data-stu-id="b5388-211">Low</span></span>      |
| [<span data-ttu-id="b5388-212">DbFunction. Schema má hodnotu null nebo je prázdný řetězec, který nakonfiguruje, aby byl ve výchozím schématu modelu.</span><span class="sxs-lookup"><span data-stu-id="b5388-212">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>](#udf-empty-string) | <span data-ttu-id="b5388-213">Slab</span><span class="sxs-lookup"><span data-stu-id="b5388-213">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="b5388-214">Dotazy LINQ již nejsou vyhodnocovány na klientovi.</span><span class="sxs-lookup"><span data-stu-id="b5388-214">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="b5388-215">[Sledování problému #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[také zobrazit problém #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="b5388-215">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="b5388-216">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-216">**Old behavior**</span></span>

<span data-ttu-id="b5388-217">Před 3,0, když EF Core nedokázal převést výraz, který byl součástí dotazu na buď SQL, nebo parametr, automaticky vyhodnotí výraz na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="b5388-217">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="b5388-218">Ve výchozím nastavení vyhodnocování klientů u potenciálně nákladných výrazů aktivovalo pouze upozornění.</span><span class="sxs-lookup"><span data-stu-id="b5388-218">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="b5388-219">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-219">**New behavior**</span></span>

<span data-ttu-id="b5388-220">Počínaje 3,0 EF Core povoluje vyhodnocení v klientovi jenom výrazy v projekci nejvyšší úrovně (poslední volání `Select()` v dotazu).</span><span class="sxs-lookup"><span data-stu-id="b5388-220">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="b5388-221">Pokud výrazy v jakékoli jiné části dotazu nelze převést na buď SQL, nebo parametr, je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="b5388-221">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="b5388-222">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-222">**Why**</span></span>

<span data-ttu-id="b5388-223">Automatické hodnocení dotazů umožňuje provádět mnoho dotazů i v případě, že není možné přeložit jejich důležité části.</span><span class="sxs-lookup"><span data-stu-id="b5388-223">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="b5388-224">Toto chování může vést k neočekávanému a potenciálně škodlivému chování, které může být jen zřejmé v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="b5388-224">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="b5388-225">Například podmínka v volání `Where()`, která se nedá přeložit, může způsobit, že se všechny řádky z tabulky přenesou z databázového serveru a filtr, který se má použít na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="b5388-225">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="b5388-226">Tato situace může snadno přejít zpět, pokud tabulka obsahuje jenom několik řádků ve vývoji, ale při přesunu aplikace do produkčního prostředí zasáhnout, kde tabulka může obsahovat miliony řádků.</span><span class="sxs-lookup"><span data-stu-id="b5388-226">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="b5388-227">Upozornění na vyhodnocení klientů se také během vývoje ukázala jako příliš jednoduchá.</span><span class="sxs-lookup"><span data-stu-id="b5388-227">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="b5388-228">Kromě toho může automatické hodnocení klienta vést k problémům s tím, že vylepšení překladu dotazů pro konkrétní výrazy způsobilo nezamýšlené neúmyslné změny mezi verzemi.</span><span class="sxs-lookup"><span data-stu-id="b5388-228">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="b5388-229">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-229">**Mitigations**</span></span>

<span data-ttu-id="b5388-230">Pokud dotaz nelze plně přeložit, pak buď Přepište dotaz do formuláře, který lze přeložit, nebo použijte `AsEnumerable()`, `ToList()` nebo podobným způsobem, aby byla data vrácena zpět do klienta, kde lze následně dále zpracovávat pomocí LINQ-to-Objects.</span><span class="sxs-lookup"><span data-stu-id="b5388-230">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="b5388-231">EF Core 3,0 cíle .NET Standard 2,1 místo .NET Standard 2,0</span><span class="sxs-lookup"><span data-stu-id="b5388-231">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="b5388-232">Sledování problému #15498</span><span class="sxs-lookup"><span data-stu-id="b5388-232">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

<span data-ttu-id="b5388-233">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-233">**Old behavior**</span></span>

<span data-ttu-id="b5388-234">Před 3,0 EF Core cílené .NET Standard 2,0 a spustí se na všech platformách, které podporují tento standard, včetně .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="b5388-234">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="b5388-235">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-235">**New behavior**</span></span>

<span data-ttu-id="b5388-236">Počínaje 3,0 se EF Core cíle .NET Standard 2,1 a spustí se na všech platformách, které podporují tento standard.</span><span class="sxs-lookup"><span data-stu-id="b5388-236">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="b5388-237">To nezahrnuje .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="b5388-237">This does not include .NET Framework.</span></span>

<span data-ttu-id="b5388-238">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-238">**Why**</span></span>

<span data-ttu-id="b5388-239">Toto je součást strategického rozhodnutí napříč technologiemi .NET a zaměřuje se na energii na platformě .NET Core a dalších moderních platformách .NET, jako je Xamarin.</span><span class="sxs-lookup"><span data-stu-id="b5388-239">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="b5388-240">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-240">**Mitigations**</span></span>

<span data-ttu-id="b5388-241">Zvažte přechod na moderní platformu .NET.</span><span class="sxs-lookup"><span data-stu-id="b5388-241">Consider moving to a modern .NET platform.</span></span> <span data-ttu-id="b5388-242">Pokud to není možné, pak pokračujte v používání EF Core 2,1 nebo EF Core 2,2, které podporují .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="b5388-242">If this is not possible, then continue to use EF Core 2.1 or EF Core 2.2, both of which support .NET Framework.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="b5388-243">Entity Framework Core už není součástí sdílené ASP.NET Core architektury.</span><span class="sxs-lookup"><span data-stu-id="b5388-243">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="b5388-244">Sledování oznámení o problémech # 325</span><span class="sxs-lookup"><span data-stu-id="b5388-244">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="b5388-245">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-245">**Old behavior**</span></span>

<span data-ttu-id="b5388-246">Pokud jste před ASP.NET Core 3,0 přidali odkaz na balíček `Microsoft.AspNetCore.App` nebo `Microsoft.AspNetCore.All`, bude obsahovat EF Core a někteří poskytovatelé EF Core dat jako poskytovatelé SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b5388-246">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="b5388-247">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-247">**New behavior**</span></span>

<span data-ttu-id="b5388-248">Počínaje 3,0 ASP.NET Core sdílené rozhraní nezahrnuje EF Core nebo žádné poskytovatele EF Core dat.</span><span class="sxs-lookup"><span data-stu-id="b5388-248">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="b5388-249">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-249">**Why**</span></span>

<span data-ttu-id="b5388-250">Před touto změnou EF Core nutné jiné kroky v závislosti na tom, zda je aplikace cílena ASP.NET Core a SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b5388-250">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="b5388-251">Upgrade ASP.NET Core taky vynutil upgrade EF Core a poskytovatele SQL Server, který není vždycky žádoucí.</span><span class="sxs-lookup"><span data-stu-id="b5388-251">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="b5388-252">V důsledku této změny je prostředí získání EF Core stejné u všech poskytovatelů, podporovaných implementací rozhraní .NET a typů aplikací.</span><span class="sxs-lookup"><span data-stu-id="b5388-252">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="b5388-253">Vývojáři teď můžou přesně řídit, když EF Core a EF Core se upgradují poskytovatelé dat.</span><span class="sxs-lookup"><span data-stu-id="b5388-253">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="b5388-254">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-254">**Mitigations**</span></span>

<span data-ttu-id="b5388-255">Chcete-li použít EF Core v aplikaci ASP.NET Core 3,0 nebo v jakékoli jiné podporované aplikaci, explicitně přidejte odkaz na balíček EF Coreho poskytovatele databáze, který bude aplikace používat.</span><span class="sxs-lookup"><span data-stu-id="b5388-255">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="b5388-256">EF Core nástroj příkazového řádku dotnet EF již není součástí .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="b5388-256">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="b5388-257">Sledování problému #14016</span><span class="sxs-lookup"><span data-stu-id="b5388-257">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="b5388-258">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-258">**Old behavior**</span></span>

<span data-ttu-id="b5388-259">Před 3,0 byl nástroj `dotnet ef` součástí .NET Core SDK a byl snadno dostupný pro použití z příkazového řádku z libovolného projektu bez nutnosti dalších kroků.</span><span class="sxs-lookup"><span data-stu-id="b5388-259">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="b5388-260">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-260">**New behavior**</span></span>

<span data-ttu-id="b5388-261">Počínaje 3,0 sada .NET SDK neobsahuje nástroj `dotnet ef`, takže před tím, než ho bude možné použít, je nutné explicitně nainstalovat jako místní nebo globální nástroj.</span><span class="sxs-lookup"><span data-stu-id="b5388-261">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="b5388-262">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-262">**Why**</span></span>

<span data-ttu-id="b5388-263">Tato změna nám umožňuje distribuovat a aktualizovat `dotnet ef` jako regulární nástroj rozhraní příkazového řádku .NET v NuGet, a to v souladu se skutečností, že EF Core 3,0 je také vždy distribuován jako balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="b5388-263">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="b5388-264">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-264">**Mitigations**</span></span>

<span data-ttu-id="b5388-265">Aby bylo možné spravovat migrace nebo uživatelské rozhraní `DbContext`, nainstalujte `dotnet-ef` jako globální nástroj:</span><span class="sxs-lookup"><span data-stu-id="b5388-265">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef
  ```

<span data-ttu-id="b5388-266">Můžete také získat místní nástroj při obnovování závislostí projektu, který deklaruje jako závislost nástrojů pomocí [souboru manifestu nástroje](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="b5388-266">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="b5388-267">Z tabulek, ExecuteSql a ExecuteSqlAsync byly přejmenovány.</span><span class="sxs-lookup"><span data-stu-id="b5388-267">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="b5388-268">Sledování problému #10996</span><span class="sxs-lookup"><span data-stu-id="b5388-268">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="b5388-269">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-269">**Old behavior**</span></span>

<span data-ttu-id="b5388-270">Před EF Core 3,0 byly tyto názvy metod přetíženy, aby fungovaly s normálním řetězcem nebo řetězcem, které by měly být interpolované na SQL a parametry.</span><span class="sxs-lookup"><span data-stu-id="b5388-270">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="b5388-271">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-271">**New behavior**</span></span>

<span data-ttu-id="b5388-272">Počínaje EF Core 3,0 použijte `FromSqlRaw`, `ExecuteSqlRaw` a `ExecuteSqlRawAsync` k vytvoření parametrizovaného dotazu, kde jsou parametry předány nezávisle na řetězci dotazu.</span><span class="sxs-lookup"><span data-stu-id="b5388-272">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="b5388-273">Příklad:</span><span class="sxs-lookup"><span data-stu-id="b5388-273">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="b5388-274">Pomocí `FromSqlInterpolated`, `ExecuteSqlInterpolated` a `ExecuteSqlInterpolatedAsync` vytvořte parametrizovaný dotaz, ve kterém jsou parametry předány jako součást interpolované řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="b5388-274">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="b5388-275">Příklad:</span><span class="sxs-lookup"><span data-stu-id="b5388-275">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="b5388-276">Všimněte si, že obě výše uvedené dotazy vytvoří stejný parametrizovaný SQL se stejnými parametry SQL.</span><span class="sxs-lookup"><span data-stu-id="b5388-276">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="b5388-277">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-277">**Why**</span></span>

<span data-ttu-id="b5388-278">Přetížení metody, jako to, usnadňuje náhodné volání nezpracované řetězcové metody, pokud by záměr byl zavolat interpolovaná řetězcová metoda a druhá možnost kolem.</span><span class="sxs-lookup"><span data-stu-id="b5388-278">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="b5388-279">To může vést k tomu, že dotazy nejsou parametrizované, pokud by měly být.</span><span class="sxs-lookup"><span data-stu-id="b5388-279">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="b5388-280">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-280">**Mitigations**</span></span>

<span data-ttu-id="b5388-281">Přepněte na použití nových názvů metod.</span><span class="sxs-lookup"><span data-stu-id="b5388-281">Switch to use the new method names.</span></span>

<a name="fromsqlsproc"></a>
### <a name="fromsql-method-when-used-with-stored-procedure-cannot-be-composed"></a><span data-ttu-id="b5388-282">Metoda Z tabulek při použití s uloženou procedurou nemůže být složená.</span><span class="sxs-lookup"><span data-stu-id="b5388-282">FromSql method when used with stored procedure cannot be composed</span></span>

[<span data-ttu-id="b5388-283">Sledování problému #15392</span><span class="sxs-lookup"><span data-stu-id="b5388-283">Tracking Issue #15392</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15392)

<span data-ttu-id="b5388-284">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-284">**Old behavior**</span></span>

<span data-ttu-id="b5388-285">Před EF Core 3,0 se metoda Z tabulek pokusila zjistit, zda je možné sestavit předaný SQL.</span><span class="sxs-lookup"><span data-stu-id="b5388-285">Before EF Core 3.0, FromSql method tried to detect if the passed SQL can be composed upon.</span></span> <span data-ttu-id="b5388-286">Vyvolalo se hodnocení klienta, pokud SQL bylo bez možnosti složení, jako je uložená procedura.</span><span class="sxs-lookup"><span data-stu-id="b5388-286">It did client evaluation when the SQL was non-composable like a stored procedure.</span></span> <span data-ttu-id="b5388-287">Následující dotaz fungoval spuštěním uložené procedury na serveru a provedením FirstOrDefault na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="b5388-287">The following query worked by running the stored procedure on the server and doing FirstOrDefault on the client side.</span></span>

```C#
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").FirstOrDefault();
```

<span data-ttu-id="b5388-288">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-288">**New behavior**</span></span>

<span data-ttu-id="b5388-289">Počínaje EF Core 3,0 se EF Core nepokusí analyzovat SQL.</span><span class="sxs-lookup"><span data-stu-id="b5388-289">Starting with EF Core 3.0, EF Core will not try to parse the SQL.</span></span> <span data-ttu-id="b5388-290">Takže pokud vytváříte po FromSqlRaw/FromSqlInterpolated, pak EF Core vytvoří příkaz SQL, který by způsobil dotaz sub.</span><span class="sxs-lookup"><span data-stu-id="b5388-290">So if you are composing after FromSqlRaw/FromSqlInterpolated, then EF Core will compose the SQL by causing sub query.</span></span> <span data-ttu-id="b5388-291">Takže pokud používáte uloženou proceduru se složením, zobrazí se výjimka pro neplatnou syntaxi SQL.</span><span class="sxs-lookup"><span data-stu-id="b5388-291">So if you are using a stored procedure with composition then you will get an exception for invalid SQL syntax.</span></span>

<span data-ttu-id="b5388-292">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-292">**Why**</span></span>

<span data-ttu-id="b5388-293">EF Core 3,0 nepodporuje automatické hodnocení klienta, protože to bylo náchylné k chybám, jak je vysvětleno [zde](#linq-queries-are-no-longer-evaluated-on-the-client).</span><span class="sxs-lookup"><span data-stu-id="b5388-293">EF Core 3.0 does not support automatic client evaluation, since it was error prone as explained [here](#linq-queries-are-no-longer-evaluated-on-the-client).</span></span>

<span data-ttu-id="b5388-294">**Zmírnění**</span><span class="sxs-lookup"><span data-stu-id="b5388-294">**Mitigation**</span></span>

<span data-ttu-id="b5388-295">Pokud používáte uloženou proceduru v FromSqlRaw/FromSqlInterpolated, znamená to, že se na ni nelze založit, takže můžete přidat __AsEnumerable/AsAsyncEnumerable__ hned po volání metody z tabulek, aby se zabránilo jakémukoli složení na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="b5388-295">If you are using a stored procedure in FromSqlRaw/FromSqlInterpolated, you know that it cannot be composed upon, so you can add __AsEnumerable/AsAsyncEnumerable__ right after the FromSql method call to avoid any composition on server side.</span></span>

```C#
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").AsEnumerable().FirstOrDefault();
```

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="b5388-296">Metody Z tabulek se dají zadat jenom v kořenech dotazů.</span><span class="sxs-lookup"><span data-stu-id="b5388-296">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="b5388-297">Sledování problému #15704</span><span class="sxs-lookup"><span data-stu-id="b5388-297">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="b5388-298">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-298">**Old behavior**</span></span>

<span data-ttu-id="b5388-299">Před EF Core 3,0 lze zadat metodu `FromSql` kdekoli v dotazu.</span><span class="sxs-lookup"><span data-stu-id="b5388-299">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="b5388-300">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-300">**New behavior**</span></span>

<span data-ttu-id="b5388-301">Počínaje EF Core 3,0 se nové metody `FromSqlRaw` a `FromSqlInterpolated` (které nahrazují `FromSql`) dají zadat jenom pro kořeny dotazů, tj. přímo na `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="b5388-301">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="b5388-302">Pokud se pokusíte zadat jiné místo jinak, dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="b5388-302">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="b5388-303">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-303">**Why**</span></span>

<span data-ttu-id="b5388-304">Zadání `FromSql` kdekoli jinde než na `DbSet` neobsahovalo žádné přidané významy nebo přidání hodnoty a může v některých scénářích způsobit nejednoznačnost.</span><span class="sxs-lookup"><span data-stu-id="b5388-304">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="b5388-305">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-305">**Mitigations**</span></span>

<span data-ttu-id="b5388-306">volání `FromSql` by se měla přesunout přímo na `DbSet`, na kterou se vztahují.</span><span class="sxs-lookup"><span data-stu-id="b5388-306">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="b5388-307">Žádné dotazy pro sledování neprovádějí překlad identity</span><span class="sxs-lookup"><span data-stu-id="b5388-307">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="b5388-308">Sledování problému #13518</span><span class="sxs-lookup"><span data-stu-id="b5388-308">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="b5388-309">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-309">**Old behavior**</span></span>

<span data-ttu-id="b5388-310">Před EF Core 3,0 se stejná instance entity používá pro všechny výskyty entity se zadaným typem a ID.</span><span class="sxs-lookup"><span data-stu-id="b5388-310">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="b5388-311">To odpovídá chování sledovacích dotazů.</span><span class="sxs-lookup"><span data-stu-id="b5388-311">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="b5388-312">Například tento dotaz:</span><span class="sxs-lookup"><span data-stu-id="b5388-312">For example, this query:</span></span>

```C#
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="b5388-313">vrátí stejnou instanci `Category` pro každé `Product`, která je přidružena k dané kategorii.</span><span class="sxs-lookup"><span data-stu-id="b5388-313">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="b5388-314">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-314">**New behavior**</span></span>

<span data-ttu-id="b5388-315">Počínaje EF Core 3,0 budou vytvořeny různé instance entit při výskytu entity se zadaným typem a ID na různých místech vráceného grafu.</span><span class="sxs-lookup"><span data-stu-id="b5388-315">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="b5388-316">Například výše uvedený dotaz vrátí novou instanci `Category` pro každé `Product`, a to i v případě, že jsou ke stejné kategorii přidruženy dva produkty.</span><span class="sxs-lookup"><span data-stu-id="b5388-316">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="b5388-317">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-317">**Why**</span></span>

<span data-ttu-id="b5388-318">Překlad identity (to znamená, že určení, že entita má stejný typ a ID jako dříve zjištěná entita) přidává další výkon a režii paměti.</span><span class="sxs-lookup"><span data-stu-id="b5388-318">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="b5388-319">Obvykle se spustí čítač, aby se v prvním místě nepoužily žádné dotazy na sledování.</span><span class="sxs-lookup"><span data-stu-id="b5388-319">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="b5388-320">I když může být v některých případech užitečný překlad identity, není potřeba, pokud se entity mají serializovat a odeslat klientovi, což je běžné pro žádné dotazy pro sledování.</span><span class="sxs-lookup"><span data-stu-id="b5388-320">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="b5388-321">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-321">**Mitigations**</span></span>

<span data-ttu-id="b5388-322">Pokud je vyžadováno rozlišení identity, použijte dotaz sledování.</span><span class="sxs-lookup"><span data-stu-id="b5388-322">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="b5388-323">~~Provádění dotazu se protokoluje na úrovni ladění~~ . Vrátit</span><span class="sxs-lookup"><span data-stu-id="b5388-323">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="b5388-324">Sledování problému #14523</span><span class="sxs-lookup"><span data-stu-id="b5388-324">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="b5388-325">Tuto změnu jsme vrátili, protože nová konfigurace v EF Core 3,0 umožňuje, aby byla úroveň protokolu pro libovolnou událost specifikována aplikací.</span><span class="sxs-lookup"><span data-stu-id="b5388-325">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="b5388-326">Chcete-li například přepnout protokolování SQL na `Debug`, explicitně nakonfigurujte úroveň v `OnConfiguring` nebo `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="b5388-326">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="b5388-327">Dočasné hodnoty klíčů už nejsou nastavené na instance entit.</span><span class="sxs-lookup"><span data-stu-id="b5388-327">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="b5388-328">Sledování problému #12378</span><span class="sxs-lookup"><span data-stu-id="b5388-328">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="b5388-329">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-329">**Old behavior**</span></span>

<span data-ttu-id="b5388-330">Před EF Core 3,0 byly přiřazeny dočasné hodnoty ke všem klíčovým vlastnostem, které by později měly skutečnou hodnotu vygenerovanou databází.</span><span class="sxs-lookup"><span data-stu-id="b5388-330">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="b5388-331">Obvykle jsou tyto dočasné hodnoty velkými zápornými čísly.</span><span class="sxs-lookup"><span data-stu-id="b5388-331">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="b5388-332">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-332">**New behavior**</span></span>

<span data-ttu-id="b5388-333">Počínaje 3,0 EF Core ukládá hodnotu dočasného klíče jako součást informací o sledování entity a ponechá vlastnost klíče beze změny.</span><span class="sxs-lookup"><span data-stu-id="b5388-333">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="b5388-334">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-334">**Why**</span></span>

<span data-ttu-id="b5388-335">Tato změna byla provedena, aby se předešlo omylům hodnotám klíčů v případě, že entita, která byla dříve sledována jinou instancí `DbContext`, je přesunuta do jiné instance `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="b5388-335">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="b5388-336">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-336">**Mitigations**</span></span>

<span data-ttu-id="b5388-337">Aplikace, které přiřazují hodnoty primárního klíče k cizím klíčům k vytvoření přidružení mezi entitami, můžou záviset na starém chování, pokud jsou primární klíče generované úložištěm a patří do entit ve stavu `Added`.</span><span class="sxs-lookup"><span data-stu-id="b5388-337">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="b5388-338">K tomu je možné se vyhnout:</span><span class="sxs-lookup"><span data-stu-id="b5388-338">This can be avoided by:</span></span>
* <span data-ttu-id="b5388-339">Nepoužívejte klíče generované úložištěm.</span><span class="sxs-lookup"><span data-stu-id="b5388-339">Not using store-generated keys.</span></span>
* <span data-ttu-id="b5388-340">Nastavení vlastností navigace pro vytváření relací místo nastavení hodnot cizích klíčů.</span><span class="sxs-lookup"><span data-stu-id="b5388-340">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="b5388-341">Získá aktuální dočasné hodnoty klíče z informací o sledování entity.</span><span class="sxs-lookup"><span data-stu-id="b5388-341">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="b5388-342">Například `context.Entry(blog).Property(e => e.Id).CurrentValue` vrátí dočasnou hodnotu, i když se nenastaví `blog.Id`.</span><span class="sxs-lookup"><span data-stu-id="b5388-342">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="b5388-343">DetectChanges respektuje hodnoty klíčů generované úložištěm.</span><span class="sxs-lookup"><span data-stu-id="b5388-343">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="b5388-344">Sledování problému #14616</span><span class="sxs-lookup"><span data-stu-id="b5388-344">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="b5388-345">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-345">**Old behavior**</span></span>

<span data-ttu-id="b5388-346">Před EF Core 3,0 se Nesledovaná entita, kterou najde `DetectChanges`, sledovala ve stavu `Added` a vložila se jako nový řádek při volání `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="b5388-346">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="b5388-347">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-347">**New behavior**</span></span>

<span data-ttu-id="b5388-348">Od EF Core 3,0 platí, že pokud entita používá vygenerované hodnoty klíčů a je nastavená nějaká hodnota klíče, bude se entita sledovat ve stavu `Modified`.</span><span class="sxs-lookup"><span data-stu-id="b5388-348">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="b5388-349">To znamená, že se předpokládá, že řádek pro entitu existuje a že se bude aktualizovat při volání `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="b5388-349">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="b5388-350">Pokud hodnota klíče není nastavená, nebo pokud typ entity nepoužívá vygenerované klíče, bude nová entita dál sledována jako `Added` jako v předchozích verzích.</span><span class="sxs-lookup"><span data-stu-id="b5388-350">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="b5388-351">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-351">**Why**</span></span>

<span data-ttu-id="b5388-352">Tato změna byla provedena tak, aby byla pro práci s nepřipojenými grafy entit při použití klíčů generovaných úložištěm snazší.</span><span class="sxs-lookup"><span data-stu-id="b5388-352">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="b5388-353">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-353">**Mitigations**</span></span>

<span data-ttu-id="b5388-354">Tato změna může přerušit aplikaci, pokud je typ entity nakonfigurovaný na používání vygenerovaných klíčů, ale hodnoty klíčů jsou explicitně nastavené pro nové instance.</span><span class="sxs-lookup"><span data-stu-id="b5388-354">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="b5388-355">Opravou je explicitně nakonfigurovat klíčové vlastnosti tak, aby nepoužívaly vygenerované hodnoty.</span><span class="sxs-lookup"><span data-stu-id="b5388-355">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="b5388-356">Například s rozhraním API Fluent:</span><span class="sxs-lookup"><span data-stu-id="b5388-356">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="b5388-357">Nebo s poznámkami k datům:</span><span class="sxs-lookup"><span data-stu-id="b5388-357">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="b5388-358">Odstranění kaskádových operací se teď ve výchozím nastavení provádí hned.</span><span class="sxs-lookup"><span data-stu-id="b5388-358">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="b5388-359">Sledování problému #10114</span><span class="sxs-lookup"><span data-stu-id="b5388-359">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="b5388-360">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-360">**Old behavior**</span></span>

<span data-ttu-id="b5388-361">Před 3,0 EF Core aplikovány kaskádové akce (odstraňování závislých entit, když se odstraní požadovaný objekt zabezpečení nebo když je vztah k požadovanému objektu zabezpečení vážně) nevznikl, dokud není voláno SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="b5388-361">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="b5388-362">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-362">**New behavior**</span></span>

<span data-ttu-id="b5388-363">Od 3,0 EF Core aplikuje kaskádové akce hned po zjištění spouštěcí podmínky.</span><span class="sxs-lookup"><span data-stu-id="b5388-363">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="b5388-364">Když například zavoláte `context.Remove()`, aby se odstranila hlavní entita, bude se u všech sledovaných závislých objektů taky nastavená také hodnota `Deleted` hned.</span><span class="sxs-lookup"><span data-stu-id="b5388-364">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="b5388-365">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-365">**Why**</span></span>

<span data-ttu-id="b5388-366">Tato změna byla provedena za účelem zlepšení prostředí pro scénáře vytváření datových vazeb a auditování, kde je důležité pochopit, které entity budou odstraněny _před_ voláním `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="b5388-366">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="b5388-367">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-367">**Mitigations**</span></span>

<span data-ttu-id="b5388-368">Předchozí chování lze obnovit pomocí nastavení na `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="b5388-368">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="b5388-369">Příklad:</span><span class="sxs-lookup"><span data-stu-id="b5388-369">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="eager-loading-single-query"></a>
### <a name="eager-loading-of-related-entities-now-happens-in-a-single-query"></a><span data-ttu-id="b5388-370">Eager načítání souvisejících entit se teď děje v jednom dotazu.</span><span class="sxs-lookup"><span data-stu-id="b5388-370">Eager loading of related entities now happens in a single query</span></span>

[<span data-ttu-id="b5388-371">Sledování problému #18022</span><span class="sxs-lookup"><span data-stu-id="b5388-371">Tracking issue #18022</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/18022)

<span data-ttu-id="b5388-372">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-372">**Old behavior**</span></span>

<span data-ttu-id="b5388-373">Před 3,0 se eagerly načítání kolekcí pomocí operátorů `Include` způsobilo generování více dotazů v relační databázi, jednu pro každý typ související entity.</span><span class="sxs-lookup"><span data-stu-id="b5388-373">Before 3.0, eagerly loading collection navigations via `Include` operators caused multiple queries to be generated on relational database, one for each related entity type.</span></span>

<span data-ttu-id="b5388-374">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-374">**New behavior**</span></span>

<span data-ttu-id="b5388-375">Počínaje 3,0 EF Core generuje jediný dotaz s spojeními relačních databází.</span><span class="sxs-lookup"><span data-stu-id="b5388-375">Starting with 3.0, EF Core generates a single query with JOINs on relational databases.</span></span>

<span data-ttu-id="b5388-376">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-376">**Why**</span></span>

<span data-ttu-id="b5388-377">Vydání více dotazů pro implementaci jednoho dotazu LINQ způsobilo velký počet problémů, včetně negativního výkonu, protože bylo nutné použít více databázových převodů, a problémy s integritou dat v případě, že každý dotaz může sledovat jiný stav databáze.</span><span class="sxs-lookup"><span data-stu-id="b5388-377">Issuing multiple queries to implement a single LINQ query caused numerous issues, including negative performance as multiple database roundtrips were necessary, and data coherency issues as each query could observe a different state of the database.</span></span>

<span data-ttu-id="b5388-378">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-378">**Mitigations**</span></span>

<span data-ttu-id="b5388-379">I když se nejedná o zásadní změnu, může to mít výrazný vliv na výkon aplikace, když jeden dotaz obsahuje velký počet operátorů `Include` v navigaci kolekcí.</span><span class="sxs-lookup"><span data-stu-id="b5388-379">While technically this is not a breaking change, it could have a considerable effect on application performance when a single query contains a large number of `Include` operator on collection navigations.</span></span> <span data-ttu-id="b5388-380">[Podívejte se na tento komentář](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) , kde najdete další informace a rychlé psaní dotazů.</span><span class="sxs-lookup"><span data-stu-id="b5388-380">[See this comment](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) for more information and for rewriting queries in a more efficient way.</span></span>

**

<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="b5388-381">DeleteBehavior. restrict má sémantiku čištění.</span><span class="sxs-lookup"><span data-stu-id="b5388-381">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="b5388-382">Sledování problému #12661</span><span class="sxs-lookup"><span data-stu-id="b5388-382">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="b5388-383">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-383">**Old behavior**</span></span>

<span data-ttu-id="b5388-384">Před 3,0 `DeleteBehavior.Restrict` vytvořené cizí klíče v databázi se sémantikou `Restrict`, ale také došlo ke změně interní opravy nezřetelným způsobem.</span><span class="sxs-lookup"><span data-stu-id="b5388-384">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="b5388-385">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-385">**New behavior**</span></span>

<span data-ttu-id="b5388-386">Počínaje 3,0 `DeleteBehavior.Restrict` zajistí, že se cizí klíče vytvoří s sémantikou `Restrict` – to znamená bez kaskády. porušení omezení throw – bez vlivu na interní opravu EF</span><span class="sxs-lookup"><span data-stu-id="b5388-386">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="b5388-387">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-387">**Why**</span></span>

<span data-ttu-id="b5388-388">Tato změna byla provedena pro zlepšení prostředí pro použití `DeleteBehavior` intuitivním způsobem, a to bez neočekávaných vedlejších účinků.</span><span class="sxs-lookup"><span data-stu-id="b5388-388">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="b5388-389">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-389">**Mitigations**</span></span>

<span data-ttu-id="b5388-390">Předchozí chování lze obnovit pomocí `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="b5388-390">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="b5388-391">Typy dotazů jsou konsolidovány s typy entit</span><span class="sxs-lookup"><span data-stu-id="b5388-391">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="b5388-392">Sledování problému #14194</span><span class="sxs-lookup"><span data-stu-id="b5388-392">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="b5388-393">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-393">**Old behavior**</span></span>

<span data-ttu-id="b5388-394">Před EF Core 3,0 byly [typy dotazů](xref:core/modeling/keyless-entity-types) prostředkem pro dotazování na data, která nedefinují primární klíč strukturovaným způsobem.</span><span class="sxs-lookup"><span data-stu-id="b5388-394">Before EF Core 3.0, [query types](xref:core/modeling/keyless-entity-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="b5388-395">To znamená, že typ dotazu byl použit pro mapování typů entit bez klíčů (pravděpodobnější z pohledu, ale také z tabulky), zatímco byl použit regulární typ entity, když byl klíč k dispozici (pravděpodobnější z tabulky, ale případně z pohledu).</span><span class="sxs-lookup"><span data-stu-id="b5388-395">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="b5388-396">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-396">**New behavior**</span></span>

<span data-ttu-id="b5388-397">Typ dotazu se teď bude jednat jenom o typ entity bez primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="b5388-397">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="b5388-398">Typy entit bez klíčů mají stejné funkce jako typy dotazů v předchozích verzích.</span><span class="sxs-lookup"><span data-stu-id="b5388-398">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="b5388-399">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-399">**Why**</span></span>

<span data-ttu-id="b5388-400">Tato změna byla provedena proto, aby se snížila nejasnost v souvislosti s typy dotazů.</span><span class="sxs-lookup"><span data-stu-id="b5388-400">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="b5388-401">Konkrétně jsou bez klíčů typy entit a jsou ze své podstaty jen pro čtení, ale neměly by být použity pouze proto, že typ entity musí být jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="b5388-401">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="b5388-402">Podobně jsou často mapovány na zobrazení, ale to je pouze v případě, že zobrazení často nedefinují klíče.</span><span class="sxs-lookup"><span data-stu-id="b5388-402">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="b5388-403">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-403">**Mitigations**</span></span>

<span data-ttu-id="b5388-404">Následující části rozhraní API jsou teď zastaralé:</span><span class="sxs-lookup"><span data-stu-id="b5388-404">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="b5388-405">**`ModelBuilder.Query<>()`** -místo toho musí být volána `ModelBuilder.Entity<>().HasNoKey()`, aby bylo možné označit typ entity jako neobsahující žádné klíče.</span><span class="sxs-lookup"><span data-stu-id="b5388-405">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="b5388-406">Tato služba by se ještě nenakonfigurovala pomocí konvence, aby nedocházelo k chybám konfigurace, když je primární klíč očekávaný, ale neodpovídá konvenci.</span><span class="sxs-lookup"><span data-stu-id="b5388-406">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="b5388-407">**`DbQuery<>`** -místo toho by měla být použita `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="b5388-407">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="b5388-408">**`DbContext.Query<>()`** -místo toho by měla být použita `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="b5388-408">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="b5388-409">Změnilo se konfigurační rozhraní API pro vztahy vlastněných typů.</span><span class="sxs-lookup"><span data-stu-id="b5388-409">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="b5388-410">[Sledování problému #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)problém se sledováním 
[#9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)[#14153 problémy s sledováním](https://github.com/aspnet/EntityFrameworkCore/issues/14153) 
</span><span class="sxs-lookup"><span data-stu-id="b5388-410">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="b5388-411">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-411">**Old behavior**</span></span>

<span data-ttu-id="b5388-412">Před EF Core 3,0 se konfigurace vztahu vlastníka provedla přímo po volání `OwnsOne` nebo `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="b5388-412">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="b5388-413">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-413">**New behavior**</span></span>

<span data-ttu-id="b5388-414">Počínaje EF Core 3,0 teď existuje Fluent API pro konfiguraci navigační vlastnosti pro vlastníka pomocí `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="b5388-414">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="b5388-415">Příklad:</span><span class="sxs-lookup"><span data-stu-id="b5388-415">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="b5388-416">Konfigurace vztahující se k vztahu mezi vlastníkem a vlastníkem by nyní měla být zřetězena po `WithOwner()` podobně jako u dalších konfigurací vztahů.</span><span class="sxs-lookup"><span data-stu-id="b5388-416">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="b5388-417">I když se konfigurace samotného vlastního typu bude pořád zřetězit za `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="b5388-417">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="b5388-418">Příklad:</span><span class="sxs-lookup"><span data-stu-id="b5388-418">For example:</span></span>

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

<span data-ttu-id="b5388-419">Kromě toho, že volání `Entity()`, `HasOne()` nebo `Set()` s cílem vlastněného typu nyní vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="b5388-419">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="b5388-420">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-420">**Why**</span></span>

<span data-ttu-id="b5388-421">Tato změna byla provedena za účelem vytvoření čisticího oddělení mezi konfigurací samotného typu a _vztahu k_ typu, který je vlastníkem.</span><span class="sxs-lookup"><span data-stu-id="b5388-421">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="b5388-422">Tím se zase odeberou nejednoznačnosti a nejasnosti u metod, jako je `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="b5388-422">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="b5388-423">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-423">**Mitigations**</span></span>

<span data-ttu-id="b5388-424">Změňte konfiguraci vztahů vlastněných typů tak, aby používala novou plochu rozhraní API, jak je znázorněno v předchozím příkladu.</span><span class="sxs-lookup"><span data-stu-id="b5388-424">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="b5388-425">Závislé entity, které sdílí tabulku s objektem zabezpečení, jsou teď volitelné.</span><span class="sxs-lookup"><span data-stu-id="b5388-425">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="b5388-426">Sledování problému #9005</span><span class="sxs-lookup"><span data-stu-id="b5388-426">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="b5388-427">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-427">**Old behavior**</span></span>

<span data-ttu-id="b5388-428">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="b5388-428">Consider the following model:</span></span>
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
<span data-ttu-id="b5388-429">Pokud je `OrderDetails` ve vlastnictví `Order` nebo explicitně namapované na stejnou tabulku, před EF Core 3,0 byla při přidávání nového `Order` vždy vyžadována instance `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="b5388-429">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="b5388-430">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-430">**New behavior**</span></span>

<span data-ttu-id="b5388-431">Počínaje 3,0 EF Core umožňuje přidat `Order` bez `OrderDetails` a namapovat všechny vlastnosti `OrderDetails` s výjimkou primárního klíče na sloupce s možnou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="b5388-431">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="b5388-432">Při dotazování EF Core nastaví `OrderDetails` na `null`, pokud některá z jejích požadovaných vlastností nemá hodnotu, nebo pokud se nejedná o žádné požadované vlastnosti kromě primárního klíče a všechny vlastnosti `null`.</span><span class="sxs-lookup"><span data-stu-id="b5388-432">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="b5388-433">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-433">**Mitigations**</span></span>

<span data-ttu-id="b5388-434">Pokud má váš model sdílení tabulky závislé na všech volitelných sloupcích, ale navigace ukazující na ni se neočekává `null`, aplikace by měla být upravena tak, aby zpracovávala případy, když je navigace `null`.</span><span class="sxs-lookup"><span data-stu-id="b5388-434">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="b5388-435">Pokud to není možné, měla by být do typu entity přidána požadovaná vlastnost, nebo alespoň jedna vlastnost musí mít přiřazenou jinou ne`null` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="b5388-435">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="b5388-436">Všechny entity sdílející tabulku se sloupcem souběžného tokenu musí být namapovány na vlastnost.</span><span class="sxs-lookup"><span data-stu-id="b5388-436">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="b5388-437">Sledování problému #14154</span><span class="sxs-lookup"><span data-stu-id="b5388-437">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="b5388-438">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-438">**Old behavior**</span></span>

<span data-ttu-id="b5388-439">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="b5388-439">Consider the following model:</span></span>
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
<span data-ttu-id="b5388-440">Před EF Core 3,0, pokud `OrderDetails` vlastní `Order` nebo explicitně namapované na stejnou tabulku, pak aktualizace pouze `OrderDetails` nebude aktualizovat hodnotu `Version` v klientovi a další aktualizace nebude úspěšná.</span><span class="sxs-lookup"><span data-stu-id="b5388-440">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="b5388-441">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-441">**New behavior**</span></span>

<span data-ttu-id="b5388-442">Počínaje 3,0 EF Core rozšíří novou hodnotu `Version` na `Order`, pokud vlastní `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="b5388-442">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="b5388-443">V opačném případě je při ověřování modelu vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="b5388-443">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="b5388-444">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-444">**Why**</span></span>

<span data-ttu-id="b5388-445">Tato změna byla provedena, aby nedocházelo k zastaralé hodnotě tokenu souběžnosti, pokud je aktualizována pouze jedna z entit mapovaných na stejnou tabulku.</span><span class="sxs-lookup"><span data-stu-id="b5388-445">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="b5388-446">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-446">**Mitigations**</span></span>

<span data-ttu-id="b5388-447">Všechny entity, které sdílejí tabulku, musí obsahovat vlastnost, která je namapovaná na sloupec tokenu souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="b5388-447">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="b5388-448">Je možné, že ho vytvoříte ve stínovém stavu:</span><span class="sxs-lookup"><span data-stu-id="b5388-448">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="b5388-449">Zděděné vlastnosti z nemapovaných typů jsou nyní namapovány na jeden sloupec pro všechny odvozené typy.</span><span class="sxs-lookup"><span data-stu-id="b5388-449">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="b5388-450">Sledování problému #13998</span><span class="sxs-lookup"><span data-stu-id="b5388-450">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="b5388-451">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-451">**Old behavior**</span></span>

<span data-ttu-id="b5388-452">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="b5388-452">Consider the following model:</span></span>
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

<span data-ttu-id="b5388-453">Před EF Core 3,0 bude vlastnost `ShippingAddress` namapována na samostatné sloupce `BulkOrder` a `Order` ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="b5388-453">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="b5388-454">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-454">**New behavior**</span></span>

<span data-ttu-id="b5388-455">Počínaje 3,0 EF Core pro `ShippingAddress` vytvoří pouze jeden sloupec.</span><span class="sxs-lookup"><span data-stu-id="b5388-455">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="b5388-456">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-456">**Why**</span></span>

<span data-ttu-id="b5388-457">Starý behavoir nebyl očekáván.</span><span class="sxs-lookup"><span data-stu-id="b5388-457">The old behavoir was unexpected.</span></span>

<span data-ttu-id="b5388-458">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-458">**Mitigations**</span></span>

<span data-ttu-id="b5388-459">Vlastnost může být stále explicitně namapována na samostatný sloupec odvozených typů:</span><span class="sxs-lookup"><span data-stu-id="b5388-459">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="b5388-460">Konvence vlastností cizího klíče už neodpovídá stejnému názvu jako vlastnost Principal.</span><span class="sxs-lookup"><span data-stu-id="b5388-460">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="b5388-461">Sledování problému #13274</span><span class="sxs-lookup"><span data-stu-id="b5388-461">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="b5388-462">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-462">**Old behavior**</span></span>

<span data-ttu-id="b5388-463">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="b5388-463">Consider the following model:</span></span>
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
<span data-ttu-id="b5388-464">Před EF Core 3,0 se pro cizí klíč podle konvence použije vlastnost `CustomerId`.</span><span class="sxs-lookup"><span data-stu-id="b5388-464">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="b5388-465">Pokud je však `Order` vlastněný typ, pak by to vedlo také k tomu, že `CustomerId` primární klíč, a to není obvykle očekávání.</span><span class="sxs-lookup"><span data-stu-id="b5388-465">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="b5388-466">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-466">**New behavior**</span></span>

<span data-ttu-id="b5388-467">Počínaje 3,0 se EF Core nepokouší použít vlastnosti pro cizí klíče podle konvence, pokud mají stejný název jako vlastnost Principal.</span><span class="sxs-lookup"><span data-stu-id="b5388-467">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="b5388-468">Název objektu zabezpečení zřetězený s názvem vlastnosti objektu zabezpečení a název navigace zřetězený s vzory názvů hlavních vlastností se pořád shodují.</span><span class="sxs-lookup"><span data-stu-id="b5388-468">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="b5388-469">Příklad:</span><span class="sxs-lookup"><span data-stu-id="b5388-469">For example:</span></span>

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

<span data-ttu-id="b5388-470">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-470">**Why**</span></span>

<span data-ttu-id="b5388-471">Tato změna byla provedena, aby nedocházelo k chybnému definování vlastností primárního klíče u vlastněných typů.</span><span class="sxs-lookup"><span data-stu-id="b5388-471">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="b5388-472">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-472">**Mitigations**</span></span>

<span data-ttu-id="b5388-473">Pokud by vlastnost měla být cizí klíč, a proto je součástí primárního klíče, pak ji explicitně nakonfigurujte jako takovou.</span><span class="sxs-lookup"><span data-stu-id="b5388-473">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="b5388-474">Připojení k databázi je teď zavřené, pokud už nepoužíváte dřív, než se dokončí jeho objekt TransactionScope.</span><span class="sxs-lookup"><span data-stu-id="b5388-474">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="b5388-475">Sledování problému #14218</span><span class="sxs-lookup"><span data-stu-id="b5388-475">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="b5388-476">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-476">**Old behavior**</span></span>

<span data-ttu-id="b5388-477">Pokud kontext v rámci EF Core 3,0 otevře připojení v `TransactionScope`, zůstane připojení otevřené, zatímco aktuální `TransactionScope` je aktivní.</span><span class="sxs-lookup"><span data-stu-id="b5388-477">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="b5388-478">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-478">**New behavior**</span></span>

<span data-ttu-id="b5388-479">Od 3,0 EF Core ukončí připojení, jakmile ho dokončí jeho používání.</span><span class="sxs-lookup"><span data-stu-id="b5388-479">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="b5388-480">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-480">**Why**</span></span>

<span data-ttu-id="b5388-481">Tato změna umožňuje použít více kontextů ve stejném `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="b5388-481">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="b5388-482">Nové chování se také shoduje s EF6.</span><span class="sxs-lookup"><span data-stu-id="b5388-482">The new behavior also matches EF6.</span></span>

<span data-ttu-id="b5388-483">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-483">**Mitigations**</span></span>

<span data-ttu-id="b5388-484">Pokud připojení zůstane otevřené, otevřete explicitní volání `OpenConnection()`, aby se zajistilo, že EF Core nezavřou předčasně:</span><span class="sxs-lookup"><span data-stu-id="b5388-484">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="b5388-485">Každá vlastnost používá nezávislou generaci celočíselného klíče v paměti.</span><span class="sxs-lookup"><span data-stu-id="b5388-485">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="b5388-486">Sledování problému #6872</span><span class="sxs-lookup"><span data-stu-id="b5388-486">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="b5388-487">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-487">**Old behavior**</span></span>

<span data-ttu-id="b5388-488">Před EF Core 3,0 byl pro všechny vlastnosti celého čísla klíče v paměti použit jeden generátor sdílených hodnot.</span><span class="sxs-lookup"><span data-stu-id="b5388-488">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="b5388-489">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-489">**New behavior**</span></span>

<span data-ttu-id="b5388-490">Počínaje EF Core 3,0 každé klíčové vlastnosti celého čísla získá vlastní generátor hodnot při použití databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="b5388-490">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="b5388-491">Také pokud je databáze odstraněna, generování klíče se obnoví pro všechny tabulky.</span><span class="sxs-lookup"><span data-stu-id="b5388-491">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="b5388-492">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-492">**Why**</span></span>

<span data-ttu-id="b5388-493">Tato změna byla provedená tak, aby se při vytváření klíčů v paměti lépe rovnala generování klíčů v paměti a vylepšila se možnost izolovat testy od sebe při použití databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="b5388-493">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="b5388-494">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-494">**Mitigations**</span></span>

<span data-ttu-id="b5388-495">To může poškodit aplikaci, která se spoléhá na konkrétní hodnoty klíče v paměti, které se mají nastavit.</span><span class="sxs-lookup"><span data-stu-id="b5388-495">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="b5388-496">Místo toho se nemusíte spoléhat na konkrétní hodnoty klíčů nebo aktualizovat tak, aby odpovídaly novému chování.</span><span class="sxs-lookup"><span data-stu-id="b5388-496">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="b5388-497">Ve výchozím nastavení se používají pole pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="b5388-497">Backing fields are used by default</span></span>

[<span data-ttu-id="b5388-498">Sledování problému #12430</span><span class="sxs-lookup"><span data-stu-id="b5388-498">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="b5388-499">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-499">**Old behavior**</span></span>

<span data-ttu-id="b5388-500">Před 3,0, a to i v případě, že je známé pole zálohování pro vlastnost, EF Core ve výchozím nastavení přečetl a zapsat hodnotu vlastnosti pomocí metod getter a setter vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="b5388-500">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="b5388-501">Výjimkou je provedení dotazu, kde by se pole zálohování nastavilo přímo, pokud je známé.</span><span class="sxs-lookup"><span data-stu-id="b5388-501">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="b5388-502">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-502">**New behavior**</span></span>

<span data-ttu-id="b5388-503">Počínaje EF Core 3,0 platí, že pokud je známé pole zálohování pro vlastnost, pak EF Core tuto vlastnost vždy přečte a zapíše pomocí pole pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="b5388-503">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="b5388-504">To může způsobit přerušení aplikace, pokud se aplikace spoléhá na další chování kódované v metodách getter nebo setter.</span><span class="sxs-lookup"><span data-stu-id="b5388-504">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="b5388-505">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-505">**Why**</span></span>

<span data-ttu-id="b5388-506">Tato změna byla provedena EF Core proto, aby při provádění databázových operací, které obsahují entity, ve výchozím nastavení nechybně aktivovala obchodní logiku.</span><span class="sxs-lookup"><span data-stu-id="b5388-506">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="b5388-507">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-507">**Mitigations**</span></span>

<span data-ttu-id="b5388-508">Chování před 3,0 se dá obnovit pomocí konfigurace režimu přístupu vlastnosti na `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="b5388-508">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="b5388-509">Příklad:</span><span class="sxs-lookup"><span data-stu-id="b5388-509">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="b5388-510">Vyvolat, zda je nalezeno více kompatibilních zálohovaných polí</span><span class="sxs-lookup"><span data-stu-id="b5388-510">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="b5388-511">Sledování problému #12523</span><span class="sxs-lookup"><span data-stu-id="b5388-511">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="b5388-512">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-512">**Old behavior**</span></span>

<span data-ttu-id="b5388-513">Pokud se v EF Core 3,0 shodovalo více polí s pravidly pro hledání zálohovacího pole vlastnosti, bude jedno pole zvoleno na základě pořadí priorit.</span><span class="sxs-lookup"><span data-stu-id="b5388-513">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="b5388-514">To může způsobit, že se nesprávné pole použije v nejednoznačných případech.</span><span class="sxs-lookup"><span data-stu-id="b5388-514">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="b5388-515">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-515">**New behavior**</span></span>

<span data-ttu-id="b5388-516">Počínaje EF Core 3,0 platí, že pokud je více polí spárováno se stejnou vlastností, je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="b5388-516">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="b5388-517">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-517">**Why**</span></span>

<span data-ttu-id="b5388-518">Tato změna byla provedena, aby nedocházelo k tichému použití jednoho pole v případě, že může být pouze jedna z nich správná.</span><span class="sxs-lookup"><span data-stu-id="b5388-518">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="b5388-519">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-519">**Mitigations**</span></span>

<span data-ttu-id="b5388-520">Vlastnosti s nejednoznačnými zálohovacími poli musí obsahovat pole, které se má explicitně použít.</span><span class="sxs-lookup"><span data-stu-id="b5388-520">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="b5388-521">Například pomocí rozhraní Fluent API:</span><span class="sxs-lookup"><span data-stu-id="b5388-521">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="b5388-522">Názvy vlastností pouze pro pole se musí shodovat s názvem pole.</span><span class="sxs-lookup"><span data-stu-id="b5388-522">Field-only property names should match the field name</span></span>

<span data-ttu-id="b5388-523">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-523">**Old behavior**</span></span>

<span data-ttu-id="b5388-524">Před EF Core 3,0 může být vlastnost určena hodnotou řetězce a v případě, že v typu .NET nebyla nalezena žádná vlastnost s tímto názvem, EF Core by se pokusila ji porovnat s polem pomocí pravidel konvence.</span><span class="sxs-lookup"><span data-stu-id="b5388-524">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the .NET type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="b5388-525">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-525">**New behavior**</span></span>

<span data-ttu-id="b5388-526">Počínaje EF Core 3,0 se vlastnost pouze pole musí přesně shodovat s názvem pole.</span><span class="sxs-lookup"><span data-stu-id="b5388-526">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="b5388-527">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-527">**Why**</span></span>

<span data-ttu-id="b5388-528">Tato změna byla provedena, aby se nepoužívalo stejné pole pro dvě vlastnosti s názvem podobně, ale také pravidla pro porovnání vlastností pouze pro pole, která jsou shodná s vlastnostmi mapovanými na vlastnosti CLR.</span><span class="sxs-lookup"><span data-stu-id="b5388-528">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="b5388-529">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-529">**Mitigations**</span></span>

<span data-ttu-id="b5388-530">Vlastnosti pouze polí musí být pojmenovány stejně jako pole, na které jsou namapována.</span><span class="sxs-lookup"><span data-stu-id="b5388-530">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="b5388-531">V budoucí verzi EF Core po 3,0 plánujeme znovu povolit explicitní konfiguraci názvu pole, který se liší od názvu vlastnosti (viz téma věnované problému [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span><span class="sxs-lookup"><span data-stu-id="b5388-531">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="b5388-532">AddDbContext/AddDbContextPool už nevolá AddLogging a AddMemoryCache.</span><span class="sxs-lookup"><span data-stu-id="b5388-532">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="b5388-533">Sledování problému #14756</span><span class="sxs-lookup"><span data-stu-id="b5388-533">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="b5388-534">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-534">**Old behavior**</span></span>

<span data-ttu-id="b5388-535">Před EF Core 3,0 by volání `AddDbContext` nebo `AddDbContextPool` také registrovalo protokolování a služby ukládání do mezipaměti v paměti pomocí D. I přes volání [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) a [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="b5388-535">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="b5388-536">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-536">**New behavior**</span></span>

<span data-ttu-id="b5388-537">Počínaje EF Core 3,0 `AddDbContext` a `AddDbContextPool` nebudou nadále registrovat tyto služby se vkládáním závislostí (DI).</span><span class="sxs-lookup"><span data-stu-id="b5388-537">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="b5388-538">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-538">**Why**</span></span>

<span data-ttu-id="b5388-539">EF Core 3,0 nevyžaduje, aby se tyto služby nacházejí v kontejneru aplikace DI.</span><span class="sxs-lookup"><span data-stu-id="b5388-539">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="b5388-540">Pokud je však `ILoggerFactory` registrována v kontejneru aplikace, bude nadále používána EF Core.</span><span class="sxs-lookup"><span data-stu-id="b5388-540">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="b5388-541">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-541">**Mitigations**</span></span>

<span data-ttu-id="b5388-542">Pokud vaše aplikace potřebuje tyto služby, zaregistrujte je explicitně pomocí kontejneru DI pomocí [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) nebo [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="b5388-542">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="b5388-543">DbContext. entry teď provádí místní DetectChanges.</span><span class="sxs-lookup"><span data-stu-id="b5388-543">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="b5388-544">Sledování problému #13552</span><span class="sxs-lookup"><span data-stu-id="b5388-544">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="b5388-545">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-545">**Old behavior**</span></span>

<span data-ttu-id="b5388-546">Před EF Core 3,0 by volání `DbContext.Entry` způsobilo zjištění změn všech sledovaných entit.</span><span class="sxs-lookup"><span data-stu-id="b5388-546">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="b5388-547">Tím je zajištěno, že stav zpřístupněný v `EntityEntry` byl aktuální.</span><span class="sxs-lookup"><span data-stu-id="b5388-547">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="b5388-548">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-548">**New behavior**</span></span>

<span data-ttu-id="b5388-549">Počínaje EF Core 3,0 se volání `DbContext.Entry` se nyní pokusí pouze detekovat změny v dané entitě a všech sledovaných hlavních entitách, které s ní souvisejí.</span><span class="sxs-lookup"><span data-stu-id="b5388-549">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="b5388-550">To znamená, že změny jinde nemohly být zjištěny voláním této metody, což by mohlo mít vliv na stav aplikace.</span><span class="sxs-lookup"><span data-stu-id="b5388-550">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="b5388-551">Všimněte si, že pokud je hodnota `ChangeTracker.AutoDetectChangesEnabled` nastavená na `false`, pak se toto místní zjišťování změn zakáže.</span><span class="sxs-lookup"><span data-stu-id="b5388-551">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="b5388-552">Jiné metody, které způsobují detekci změn – například `ChangeTracker.Entries` a `SaveChanges`---a přesto způsobují kompletní `DetectChanges` všech sledovaných entit.</span><span class="sxs-lookup"><span data-stu-id="b5388-552">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="b5388-553">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-553">**Why**</span></span>

<span data-ttu-id="b5388-554">Tato změna byla provedena za účelem zlepšení výchozího výkonu při použití `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="b5388-554">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="b5388-555">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-555">**Mitigations**</span></span>

<span data-ttu-id="b5388-556">Před voláním `Entry` volejte `ChgangeTracker.DetectChanges()`, aby se zajistilo chování před 3,0.</span><span class="sxs-lookup"><span data-stu-id="b5388-556">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="b5388-557">Klíče řetězce a pole bajtů nejsou ve výchozím nastavení generovány klientem.</span><span class="sxs-lookup"><span data-stu-id="b5388-557">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="b5388-558">Sledování problému #14617</span><span class="sxs-lookup"><span data-stu-id="b5388-558">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="b5388-559">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-559">**Old behavior**</span></span>

<span data-ttu-id="b5388-560">Před EF Core 3,0 lze použít `string` a `byte[]` vlastnosti klíče, aniž byste museli explicitně nastavit hodnotu, která není null.</span><span class="sxs-lookup"><span data-stu-id="b5388-560">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="b5388-561">V takovém případě se hodnota klíče vygeneruje na klientovi jako identifikátor GUID serializovaný na bajty pro `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="b5388-561">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="b5388-562">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-562">**New behavior**</span></span>

<span data-ttu-id="b5388-563">Počínaje EF Core 3,0 bude vyvolána výjimka oznamující, že nebyla nastavena žádná hodnota klíče.</span><span class="sxs-lookup"><span data-stu-id="b5388-563">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="b5388-564">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-564">**Why**</span></span>

<span data-ttu-id="b5388-565">Tato změna byla provedena, protože /`byte[]` hodnoty `string`generované klientem nejsou všeobecně užitečné a výchozí chování způsobilo, že je obtížné vygenerovat hodnoty klíčů běžným způsobem.</span><span class="sxs-lookup"><span data-stu-id="b5388-565">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="b5388-566">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-566">**Mitigations**</span></span>

<span data-ttu-id="b5388-567">Chování před 3,0 lze získat explicitním určením, že klíčové vlastnosti by měly používat generované hodnoty, pokud není nastavena žádná jiná hodnota, která není null.</span><span class="sxs-lookup"><span data-stu-id="b5388-567">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="b5388-568">Například s rozhraním API Fluent:</span><span class="sxs-lookup"><span data-stu-id="b5388-568">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="b5388-569">Nebo s poznámkami k datům:</span><span class="sxs-lookup"><span data-stu-id="b5388-569">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="b5388-570">ILoggerFactory je teď služba s vymezeným oborem.</span><span class="sxs-lookup"><span data-stu-id="b5388-570">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="b5388-571">Sledování problému #14698</span><span class="sxs-lookup"><span data-stu-id="b5388-571">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="b5388-572">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-572">**Old behavior**</span></span>

<span data-ttu-id="b5388-573">Před EF Core 3,0 byl `ILoggerFactory` zaregistrován jako služba s jedním prvkem.</span><span class="sxs-lookup"><span data-stu-id="b5388-573">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="b5388-574">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-574">**New behavior**</span></span>

<span data-ttu-id="b5388-575">Počínaje EF Core 3,0 se nyní `ILoggerFactory` zaregistruje jako obor.</span><span class="sxs-lookup"><span data-stu-id="b5388-575">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="b5388-576">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-576">**Why**</span></span>

<span data-ttu-id="b5388-577">Tato změna byla provedena, aby bylo možné povolit přidružení protokolovacího nástroje k instanci `DbContext`, která umožňuje další funkce a odebírá některé případy patologického chování, jako je například rozbalení interních poskytovatelů služeb.</span><span class="sxs-lookup"><span data-stu-id="b5388-577">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="b5388-578">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-578">**Mitigations**</span></span>

<span data-ttu-id="b5388-579">Tato změna by neměla mít vliv na kód aplikace, pokud se neregistruje a nepoužívá vlastní služby pro EF Core interního poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="b5388-579">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="b5388-580">To není běžné.</span><span class="sxs-lookup"><span data-stu-id="b5388-580">This isn't common.</span></span>
<span data-ttu-id="b5388-581">V těchto případech bude většina věcí pořád fungovat, ale jakákoli služba typu Singleton, která byla závislá na `ILoggerFactory`, se musí změnit tak, aby `ILoggerFactory` získala jiným způsobem.</span><span class="sxs-lookup"><span data-stu-id="b5388-581">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="b5388-582">Pokud narazíte na takové situace, zajistěte prosím problém na [EF Core sledování problémů GitHubu](https://github.com/aspnet/EntityFrameworkCore/issues) , abychom nás věděli, jak používáte `ILoggerFactory`, takže můžeme lépe porozumět tomu, jak to v budoucnu ještě Nerušit.</span><span class="sxs-lookup"><span data-stu-id="b5388-582">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="b5388-583">Opožděné načítání proxy serverů už nepředpokládá navigační vlastnosti, jsou plně načtené.</span><span class="sxs-lookup"><span data-stu-id="b5388-583">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="b5388-584">Sledování problému #12780</span><span class="sxs-lookup"><span data-stu-id="b5388-584">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="b5388-585">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-585">**Old behavior**</span></span>

<span data-ttu-id="b5388-586">Před EF Core 3,0 se po odstranění `DbContext` nijak nevědělo, zda byla daná vlastnost navigace u entity získané z tohoto kontextu plně načtena nebo nikoli.</span><span class="sxs-lookup"><span data-stu-id="b5388-586">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="b5388-587">Proxy místo toho předpokládají, že je načtena odkazová navigace, pokud má hodnotu jinou než null a že je načtena navigace kolekce, pokud není prázdná.</span><span class="sxs-lookup"><span data-stu-id="b5388-587">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="b5388-588">V těchto případech by byl pokus o opožděné načtení no-op.</span><span class="sxs-lookup"><span data-stu-id="b5388-588">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="b5388-589">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-589">**New behavior**</span></span>

<span data-ttu-id="b5388-590">Počínaje EF Core 3,0 budou proxy servery sledovat, zda je načtena vlastnost navigace.</span><span class="sxs-lookup"><span data-stu-id="b5388-590">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="b5388-591">To znamená, že při pokusu o přístup k navigační vlastnosti, která je načtena po vyřazení kontextu, bude vždy ta no-op, i když je načtená navigace prázdná nebo má hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="b5388-591">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="b5388-592">Naopak při pokusu o přístup k navigační vlastnosti, která není načtená, vyvolá výjimku, pokud je kontext vyřazený, i když je vlastnost navigace neprázdná kolekce.</span><span class="sxs-lookup"><span data-stu-id="b5388-592">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="b5388-593">Pokud nastane tato situace, znamená to, že se kód aplikace pokouší použít opožděné načítání v neplatném čase a aplikace by se měla změnit, aby to nevedlo.</span><span class="sxs-lookup"><span data-stu-id="b5388-593">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="b5388-594">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-594">**Why**</span></span>

<span data-ttu-id="b5388-595">Tato změna byla provedena, aby při pokusu o opožděné načtení na uvolněnou instanci `DbContext` bylo chování konzistentní a správné.</span><span class="sxs-lookup"><span data-stu-id="b5388-595">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="b5388-596">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-596">**Mitigations**</span></span>

<span data-ttu-id="b5388-597">Aktualizujte kód aplikace, aby se nepokoušel opožděné načtení s odstraněným kontextem, nebo nastavte tuto hodnotu jako No-op, jak je popsáno ve zprávě výjimky.</span><span class="sxs-lookup"><span data-stu-id="b5388-597">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="b5388-598">Nadměrné vytváření interních zprostředkovatelů služeb je teď ve výchozím nastavení chyba.</span><span class="sxs-lookup"><span data-stu-id="b5388-598">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="b5388-599">Sledování problému #10236</span><span class="sxs-lookup"><span data-stu-id="b5388-599">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="b5388-600">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-600">**Old behavior**</span></span>

<span data-ttu-id="b5388-601">Před EF Core 3,0 se pro aplikaci, která vytváří patologický počet interních poskytovatelů služeb, zaprotokoluje upozornění.</span><span class="sxs-lookup"><span data-stu-id="b5388-601">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="b5388-602">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-602">**New behavior**</span></span>

<span data-ttu-id="b5388-603">Počínaje EF Core 3,0 je toto upozornění nyní považováno za chybu a je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="b5388-603">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="b5388-604">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-604">**Why**</span></span>

<span data-ttu-id="b5388-605">Tato změna byla provedená tak, že se tento patologický případ výslovně zveřejňuje tak, aby se zlepšil kód aplikace.</span><span class="sxs-lookup"><span data-stu-id="b5388-605">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="b5388-606">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-606">**Mitigations**</span></span>

<span data-ttu-id="b5388-607">Nejvhodnější příčinou této chyby je pochopení hlavní příčiny a zastavení vytváření, takže mnoho interních poskytovatelů služeb.</span><span class="sxs-lookup"><span data-stu-id="b5388-607">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="b5388-608">Chybu však lze převést zpět na varování (nebo ignorováno) prostřednictvím konfigurace na `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="b5388-608">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="b5388-609">Příklad:</span><span class="sxs-lookup"><span data-stu-id="b5388-609">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="b5388-610">Nové chování pro HasOne/HasMany se volá s jedním řetězcem.</span><span class="sxs-lookup"><span data-stu-id="b5388-610">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="b5388-611">Sledování problému #9171</span><span class="sxs-lookup"><span data-stu-id="b5388-611">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="b5388-612">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-612">**Old behavior**</span></span>

<span data-ttu-id="b5388-613">Před EF Core 3,0 byl kód volání `HasOne` nebo `HasMany` s jedním řetězcem interpretován jako matoucí.</span><span class="sxs-lookup"><span data-stu-id="b5388-613">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="b5388-614">Příklad:</span><span class="sxs-lookup"><span data-stu-id="b5388-614">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="b5388-615">Kód vypadá jako s tím, že se vztahuje `Samurai` na některý jiný typ entity pomocí navigační vlastnosti `Entrance`, která může být soukromá.</span><span class="sxs-lookup"><span data-stu-id="b5388-615">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="b5388-616">Ve skutečnosti se tento kód pokusí vytvořit relaci k některému typu entity s názvem `Entrance` bez navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="b5388-616">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="b5388-617">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-617">**New behavior**</span></span>

<span data-ttu-id="b5388-618">Počínaje EF Core 3,0 výše uvedený kód má nyní podobný vzhled jako předtím.</span><span class="sxs-lookup"><span data-stu-id="b5388-618">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="b5388-619">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-619">**Why**</span></span>

<span data-ttu-id="b5388-620">Staré chování bylo velmi matoucí, zejména při čtení konfiguračního kódu a hledání chyb.</span><span class="sxs-lookup"><span data-stu-id="b5388-620">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="b5388-621">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-621">**Mitigations**</span></span>

<span data-ttu-id="b5388-622">Tím dojde pouze k přerušení aplikací, které jsou explicitně konfigurovány pomocí řetězců pro názvy typů, a bez explicitního určení vlastnosti navigace.</span><span class="sxs-lookup"><span data-stu-id="b5388-622">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="b5388-623">To není běžné.</span><span class="sxs-lookup"><span data-stu-id="b5388-623">This is not common.</span></span>
<span data-ttu-id="b5388-624">Předchozí chování lze získat explicitním předáním `null` pro název vlastnosti navigace.</span><span class="sxs-lookup"><span data-stu-id="b5388-624">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="b5388-625">Příklad:</span><span class="sxs-lookup"><span data-stu-id="b5388-625">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="b5388-626">Návratový typ pro několik asynchronních metod byl změněn z úlohy na ValueTask</span><span class="sxs-lookup"><span data-stu-id="b5388-626">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="b5388-627">Sledování problému #15184</span><span class="sxs-lookup"><span data-stu-id="b5388-627">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="b5388-628">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-628">**Old behavior**</span></span>

<span data-ttu-id="b5388-629">Následující asynchronní metody dříve vrátily `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="b5388-629">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="b5388-630">`ValueGenerator.NextValueAsync()` (a odvozování tříd)</span><span class="sxs-lookup"><span data-stu-id="b5388-630">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="b5388-631">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-631">**New behavior**</span></span>

<span data-ttu-id="b5388-632">Výše uvedené metody nyní vrací `ValueTask<T>` na stejný `T` jako předtím.</span><span class="sxs-lookup"><span data-stu-id="b5388-632">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="b5388-633">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-633">**Why**</span></span>

<span data-ttu-id="b5388-634">Tato změna snižuje počet přidělení haldy, které vznikly při vyvolání těchto metod, což zlepšuje obecný výkon.</span><span class="sxs-lookup"><span data-stu-id="b5388-634">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="b5388-635">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-635">**Mitigations**</span></span>

<span data-ttu-id="b5388-636">Aplikace jednoduše čekají na rozhraní API, které je třeba znovu zkompilovat – nejsou nutné žádné změny zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="b5388-636">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="b5388-637">Složitější využití (například předání vráceného `Task` do `Task.WhenAny()`) obvykle vyžaduje, aby vrácený `ValueTask<T>` byl převeden na `Task<T>` voláním `AsTask()`.</span><span class="sxs-lookup"><span data-stu-id="b5388-637">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="b5388-638">Všimněte si, že se tím sníží omezení přidělení, které tato změna přináší.</span><span class="sxs-lookup"><span data-stu-id="b5388-638">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="b5388-639">Relační: anotace TypeMapping je nyní pouze TypeMapping</span><span class="sxs-lookup"><span data-stu-id="b5388-639">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="b5388-640">Sledování problému #9913</span><span class="sxs-lookup"><span data-stu-id="b5388-640">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="b5388-641">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-641">**Old behavior**</span></span>

<span data-ttu-id="b5388-642">Název poznámky pro poznámky k mapování typů byl "relační: TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="b5388-642">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="b5388-643">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-643">**New behavior**</span></span>

<span data-ttu-id="b5388-644">Název poznámky pro mapování typů je nyní "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="b5388-644">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="b5388-645">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-645">**Why**</span></span>

<span data-ttu-id="b5388-646">Mapování typů se nyní používají pro více než stejné poskytovatele relačních databází.</span><span class="sxs-lookup"><span data-stu-id="b5388-646">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="b5388-647">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-647">**Mitigations**</span></span>

<span data-ttu-id="b5388-648">Tím dojde pouze k přerušení aplikací, které přistupují k mapování typu přímo jako anotaci, což není běžné.</span><span class="sxs-lookup"><span data-stu-id="b5388-648">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="b5388-649">Nejvhodnější akcí pro opravu je použití prostoru rozhraní API pro přístup k mapování typů namísto použití anotace přímo.</span><span class="sxs-lookup"><span data-stu-id="b5388-649">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="b5388-650">ToTable na odvozeném typu vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="b5388-650">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="b5388-651">Sledování problému #11811</span><span class="sxs-lookup"><span data-stu-id="b5388-651">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="b5388-652">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-652">**Old behavior**</span></span>

<span data-ttu-id="b5388-653">Před EF Core 3,0 se `ToTable()` volání na odvozený typ ignoruje, protože pouze dědičnost dědění mapování je typu TPH, kde to není platné.</span><span class="sxs-lookup"><span data-stu-id="b5388-653">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="b5388-654">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-654">**New behavior**</span></span>

<span data-ttu-id="b5388-655">Počínaje EF Core 3,0 a při přípravě na přidání podpory TPT a TPC v pozdější verzi teď `ToTable()` volána na odvozeném typu nyní vyvolá výjimku, aby se předešlo neočekávané změně mapování v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="b5388-655">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="b5388-656">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-656">**Why**</span></span>

<span data-ttu-id="b5388-657">V současné době není platný pro mapování odvozeného typu na jinou tabulku.</span><span class="sxs-lookup"><span data-stu-id="b5388-657">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="b5388-658">Tato změna zabrání v budoucnosti v budoucnu, pokud se to stalo platným.</span><span class="sxs-lookup"><span data-stu-id="b5388-658">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="b5388-659">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-659">**Mitigations**</span></span>

<span data-ttu-id="b5388-660">Odeberte všechny pokusy o mapování odvozených typů na jiné tabulky.</span><span class="sxs-lookup"><span data-stu-id="b5388-660">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="b5388-661">ForSqlServerHasIndex nahrazeno HasIndex</span><span class="sxs-lookup"><span data-stu-id="b5388-661">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="b5388-662">Sledování problému #12366</span><span class="sxs-lookup"><span data-stu-id="b5388-662">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="b5388-663">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-663">**Old behavior**</span></span>

<span data-ttu-id="b5388-664">Před EF Core 3,0 `ForSqlServerHasIndex().ForSqlServerInclude()` poskytoval způsob, jak nakonfigurovat sloupce používané `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="b5388-664">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="b5388-665">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-665">**New behavior**</span></span>

<span data-ttu-id="b5388-666">Počínaje EF Core 3,0 se na relační úrovni teď podporuje použití `Include` na indexu.</span><span class="sxs-lookup"><span data-stu-id="b5388-666">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="b5388-667">Použijte `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="b5388-667">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="b5388-668">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-668">**Why**</span></span>

<span data-ttu-id="b5388-669">Tato změna byla provedena za účelem konsolidace rozhraní API pro indexy s `Include` na jednom místě pro všechny poskytovatele databáze.</span><span class="sxs-lookup"><span data-stu-id="b5388-669">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="b5388-670">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-670">**Mitigations**</span></span>

<span data-ttu-id="b5388-671">Použijte nové rozhraní API, jak vidíte výše.</span><span class="sxs-lookup"><span data-stu-id="b5388-671">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="b5388-672">Změny rozhraní API pro metadata</span><span class="sxs-lookup"><span data-stu-id="b5388-672">Metadata API changes</span></span>

[<span data-ttu-id="b5388-673">Sledování problému #214</span><span class="sxs-lookup"><span data-stu-id="b5388-673">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="b5388-674">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-674">**New behavior**</span></span>

<span data-ttu-id="b5388-675">Následující vlastnosti byly převedeny na rozšiřující metody:</span><span class="sxs-lookup"><span data-stu-id="b5388-675">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="b5388-676">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-676">**Why**</span></span>

<span data-ttu-id="b5388-677">Tato změna zjednodušuje implementaci výše uvedených rozhraní.</span><span class="sxs-lookup"><span data-stu-id="b5388-677">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="b5388-678">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-678">**Mitigations**</span></span>

<span data-ttu-id="b5388-679">Použijte nové metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="b5388-679">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="b5388-680">Změny rozhraní API pro konkrétního zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="b5388-680">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="b5388-681">Sledování problému #214</span><span class="sxs-lookup"><span data-stu-id="b5388-681">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="b5388-682">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-682">**New behavior**</span></span>

<span data-ttu-id="b5388-683">Metody rozšíření specifické pro poskytovatele budou shrnuty:</span><span class="sxs-lookup"><span data-stu-id="b5388-683">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="b5388-684">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-684">**Why**</span></span>

<span data-ttu-id="b5388-685">Tato změna zjednodušuje implementaci výše uvedených rozšiřujících metod.</span><span class="sxs-lookup"><span data-stu-id="b5388-685">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="b5388-686">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-686">**Mitigations**</span></span>

<span data-ttu-id="b5388-687">Použijte nové metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="b5388-687">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="b5388-688">EF Core už neposílá direktivu pragma pro vynucení KOFK SQLite.</span><span class="sxs-lookup"><span data-stu-id="b5388-688">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="b5388-689">Sledování problému #12151</span><span class="sxs-lookup"><span data-stu-id="b5388-689">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="b5388-690">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-690">**Old behavior**</span></span>

<span data-ttu-id="b5388-691">Před EF Core 3,0 by EF Core při otevření připojení k SQLite odeslal `PRAGMA foreign_keys = 1`.</span><span class="sxs-lookup"><span data-stu-id="b5388-691">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="b5388-692">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-692">**New behavior**</span></span>

<span data-ttu-id="b5388-693">Počínaje EF Core 3,0 EF Core již neposílá `PRAGMA foreign_keys = 1` při otevření připojení k SQLite.</span><span class="sxs-lookup"><span data-stu-id="b5388-693">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="b5388-694">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-694">**Why**</span></span>

<span data-ttu-id="b5388-695">Tato změna byla provedena, protože ve výchozím nastavení používá EF Core `SQLitePCLRaw.bundle_e_sqlite3`, což zase znamená, že je ve výchozím nastavení zapnuté vynucení CK a není nutné je explicitně povolit při každém otevření připojení.</span><span class="sxs-lookup"><span data-stu-id="b5388-695">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="b5388-696">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-696">**Mitigations**</span></span>

<span data-ttu-id="b5388-697">Cizí klíče jsou ve výchozím nastavení povolené v SQLitePCLRaw. bundle_e_sqlite3, který se ve výchozím nastavení používá pro EF Core.</span><span class="sxs-lookup"><span data-stu-id="b5388-697">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="b5388-698">V ostatních případech je možné povolit cizí klíče zadáním `Foreign Keys=True` v připojovacím řetězci.</span><span class="sxs-lookup"><span data-stu-id="b5388-698">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="b5388-699">Microsoft. EntityFrameworkCore. sqlite teď závisí na SQLitePCLRaw. bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="b5388-699">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="b5388-700">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-700">**Old behavior**</span></span>

<span data-ttu-id="b5388-701">Před EF Core 3,0 EF Core použito `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="b5388-701">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="b5388-702">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-702">**New behavior**</span></span>

<span data-ttu-id="b5388-703">Počínaje EF Core 3,0 EF Core používá `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="b5388-703">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="b5388-704">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-704">**Why**</span></span>

<span data-ttu-id="b5388-705">Tato změna byla provedena tak, že verze SQLiteu použitá v iOS je konzistentní s jinými platformami.</span><span class="sxs-lookup"><span data-stu-id="b5388-705">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="b5388-706">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-706">**Mitigations**</span></span>

<span data-ttu-id="b5388-707">Chcete-li použít nativní verzi SQLite v iOS, nakonfigurujte `Microsoft.Data.Sqlite` pro použití jiné sady `SQLitePCLRaw`.</span><span class="sxs-lookup"><span data-stu-id="b5388-707">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="b5388-708">Hodnoty GUID se teď ukládají jako TEXT na SQLite.</span><span class="sxs-lookup"><span data-stu-id="b5388-708">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="b5388-709">Sledování problému #15078</span><span class="sxs-lookup"><span data-stu-id="b5388-709">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="b5388-710">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-710">**Old behavior**</span></span>

<span data-ttu-id="b5388-711">Hodnoty GUID byly dříve uloženy jako hodnoty objektů BLOB u SQLite.</span><span class="sxs-lookup"><span data-stu-id="b5388-711">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="b5388-712">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-712">**New behavior**</span></span>

<span data-ttu-id="b5388-713">Hodnoty GUID jsou nyní uloženy jako TEXT.</span><span class="sxs-lookup"><span data-stu-id="b5388-713">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="b5388-714">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-714">**Why**</span></span>

<span data-ttu-id="b5388-715">Binární formát identifikátorů GUID není standardizovaný.</span><span class="sxs-lookup"><span data-stu-id="b5388-715">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="b5388-716">Uložení hodnot jako textu zajistí, že databáze bude lépe kompatibilní s jinými technologiemi.</span><span class="sxs-lookup"><span data-stu-id="b5388-716">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="b5388-717">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-717">**Mitigations**</span></span>

<span data-ttu-id="b5388-718">Existující databáze můžete migrovat do nového formátu tím, že spustíte příkaz SQL podobně jako následující.</span><span class="sxs-lookup"><span data-stu-id="b5388-718">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="b5388-719">V EF Core můžete i nadále používat předchozí chování nakonfigurováním převaděče hodnot na těchto vlastnostech.</span><span class="sxs-lookup"><span data-stu-id="b5388-719">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="b5388-720">Microsoft. data. sqlite zůstává schopný přečítat hodnoty GUID z objektů BLOB a textových sloupců. vzhledem k tomu, že výchozí formát pro parametry a konstanty se změnil, bude pravděpodobně nutné provést akci u většiny scénářů, které zahrnují identifikátory GUID.</span><span class="sxs-lookup"><span data-stu-id="b5388-720">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="b5388-721">Hodnoty char se teď ukládají jako TEXT na SQLite.</span><span class="sxs-lookup"><span data-stu-id="b5388-721">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="b5388-722">Sledování problému #15020</span><span class="sxs-lookup"><span data-stu-id="b5388-722">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="b5388-723">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-723">**Old behavior**</span></span>

<span data-ttu-id="b5388-724">Hodnoty typu char byly dříve sored jako CELOČÍSELNé hodnoty u SQLite.</span><span class="sxs-lookup"><span data-stu-id="b5388-724">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="b5388-725">Například hodnota znaku *a* byla uložena jako celočíselná hodnota 65.</span><span class="sxs-lookup"><span data-stu-id="b5388-725">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="b5388-726">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-726">**New behavior**</span></span>

<span data-ttu-id="b5388-727">Hodnoty typu char jsou nyní uloženy jako TEXT.</span><span class="sxs-lookup"><span data-stu-id="b5388-727">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="b5388-728">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-728">**Why**</span></span>

<span data-ttu-id="b5388-729">Ukládání hodnot jako TEXT je přirozenější a databáze usnadňuje kompatibilitu s jinými technologiemi.</span><span class="sxs-lookup"><span data-stu-id="b5388-729">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="b5388-730">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-730">**Mitigations**</span></span>

<span data-ttu-id="b5388-731">Existující databáze můžete migrovat do nového formátu tím, že spustíte příkaz SQL podobně jako následující.</span><span class="sxs-lookup"><span data-stu-id="b5388-731">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="b5388-732">V EF Core můžete i nadále používat předchozí chování nakonfigurováním převaděče hodnot na těchto vlastnostech.</span><span class="sxs-lookup"><span data-stu-id="b5388-732">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="b5388-733">Microsoft. data. sqlite také zůstává schopný číst znakové hodnoty z CELOČÍSELNého i TEXTOVÉHO sloupce, takže některé scénáře nemusí vyžadovat žádnou akci.</span><span class="sxs-lookup"><span data-stu-id="b5388-733">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="b5388-734">ID migrace se teď generují pomocí kalendáře invariantní jazykové verze.</span><span class="sxs-lookup"><span data-stu-id="b5388-734">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="b5388-735">Sledování problému #12978</span><span class="sxs-lookup"><span data-stu-id="b5388-735">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="b5388-736">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-736">**Old behavior**</span></span>

<span data-ttu-id="b5388-737">ID migrace se nechtěně vygenerovala pomocí kalendáře aktuální jazykové verze.</span><span class="sxs-lookup"><span data-stu-id="b5388-737">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="b5388-738">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-738">**New behavior**</span></span>

<span data-ttu-id="b5388-739">ID migrace se nyní vždy generují pomocí kalendáře neutrální jazykové verze (gregoriánský).</span><span class="sxs-lookup"><span data-stu-id="b5388-739">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="b5388-740">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-740">**Why**</span></span>

<span data-ttu-id="b5388-741">Pořadí migrace je důležité při aktualizaci databáze nebo řešení konfliktů při slučování.</span><span class="sxs-lookup"><span data-stu-id="b5388-741">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="b5388-742">Pomocí invariantního kalendáře se vyhnete problémům s řazením, které mohou být výsledkem členů týmu jiné systémové kalendáře.</span><span class="sxs-lookup"><span data-stu-id="b5388-742">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="b5388-743">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-743">**Mitigations**</span></span>

<span data-ttu-id="b5388-744">Tato změna má vliv na kohokoli, kdo používá negregoriánský kalendář, ve kterém je rok větší než gregoriánský kalendář (například thajský buddhistický kalendář).</span><span class="sxs-lookup"><span data-stu-id="b5388-744">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="b5388-745">Existující identifikátory migrace se budou muset aktualizovat, aby se nové migrace objednaly po stávajících migracích.</span><span class="sxs-lookup"><span data-stu-id="b5388-745">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="b5388-746">ID migrace najdete v atributu migrace v souborech návrháře migrace.</span><span class="sxs-lookup"><span data-stu-id="b5388-746">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="b5388-747">Také je potřeba aktualizovat tabulku historie migrace.</span><span class="sxs-lookup"><span data-stu-id="b5388-747">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="b5388-748">UseRowNumberForPaging se odebral.</span><span class="sxs-lookup"><span data-stu-id="b5388-748">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="b5388-749">Sledování problému #16400</span><span class="sxs-lookup"><span data-stu-id="b5388-749">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="b5388-750">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-750">**Old behavior**</span></span>

<span data-ttu-id="b5388-751">Před EF Core 3,0 se `UseRowNumberForPaging` dá použít k vygenerování SQL pro stránkování, které je kompatibilní s SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="b5388-751">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="b5388-752">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-752">**New behavior**</span></span>

<span data-ttu-id="b5388-753">Počínaje EF Core 3,0 bude EF generovat pouze SQL pro stránkování, které je kompatibilní pouze s novějšími verzemi SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b5388-753">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="b5388-754">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-754">**Why**</span></span>

<span data-ttu-id="b5388-755">Tuto změnu provedeme, protože [SQL Server 2008 už není podporovaným produktem](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) a aktualizace této funkce tak, aby fungovala se změnami dotazů provedenými v EF Core 3,0 je významná práce.</span><span class="sxs-lookup"><span data-stu-id="b5388-755">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="b5388-756">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-756">**Mitigations**</span></span>

<span data-ttu-id="b5388-757">Doporučujeme aktualizovat na novější verzi SQL Server nebo pomocí vyšší úrovně kompatibility, aby byl vygenerovaný SQL podporován.</span><span class="sxs-lookup"><span data-stu-id="b5388-757">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="b5388-758">To znamená, že pokud to nemůžete udělat, [komentář k problému s sledováním](https://github.com/aspnet/EntityFrameworkCore/issues/16400) najdete v podrobnostech.</span><span class="sxs-lookup"><span data-stu-id="b5388-758">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="b5388-759">Toto rozhodnutí můžeme znovu navštívit na základě zpětné vazby.</span><span class="sxs-lookup"><span data-stu-id="b5388-759">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="b5388-760">Informace o rozšíření/metadata se odebraly z IDbContextOptionsExtension.</span><span class="sxs-lookup"><span data-stu-id="b5388-760">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="b5388-761">Sledování problému #16119</span><span class="sxs-lookup"><span data-stu-id="b5388-761">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="b5388-762">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-762">**Old behavior**</span></span>

<span data-ttu-id="b5388-763">`IDbContextOptionsExtension` obsahuje metody pro poskytování metadat o rozšíření.</span><span class="sxs-lookup"><span data-stu-id="b5388-763">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="b5388-764">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-764">**New behavior**</span></span>

<span data-ttu-id="b5388-765">Tyto metody byly přesunuty do nové abstraktní základní třídy `DbContextOptionsExtensionInfo`, která je vrácena z nové vlastnosti `IDbContextOptionsExtension.Info`.</span><span class="sxs-lookup"><span data-stu-id="b5388-765">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="b5388-766">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-766">**Why**</span></span>

<span data-ttu-id="b5388-767">V rámci vydání od 2,0 do 3,0 jsme potřebovali přidat nebo změnit tyto metody několikrát.</span><span class="sxs-lookup"><span data-stu-id="b5388-767">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="b5388-768">Rozbalením do nové abstraktní základní třídy bude snazší vytvořit tyto změny bez přerušení stávajících rozšíření.</span><span class="sxs-lookup"><span data-stu-id="b5388-768">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="b5388-769">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-769">**Mitigations**</span></span>

<span data-ttu-id="b5388-770">Aktualizovat rozšíření tak, aby následovala nový vzor.</span><span class="sxs-lookup"><span data-stu-id="b5388-770">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="b5388-771">Příklady najdete v mnoha implementacích `IDbContextOptionsExtension` pro různé druhy rozšíření ve zdrojovém kódu EF Core.</span><span class="sxs-lookup"><span data-stu-id="b5388-771">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="b5388-772">LogQueryPossibleExceptionWithAggregateOperator se přejmenovalo.</span><span class="sxs-lookup"><span data-stu-id="b5388-772">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="b5388-773">Sledování problému #10985</span><span class="sxs-lookup"><span data-stu-id="b5388-773">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="b5388-774">**Mění**</span><span class="sxs-lookup"><span data-stu-id="b5388-774">**Change**</span></span>

<span data-ttu-id="b5388-775">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` bylo přejmenováno na `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="b5388-775">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="b5388-776">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-776">**Why**</span></span>

<span data-ttu-id="b5388-777">Zarovná pojmenování této události varování se všemi ostatními událostmi upozornění.</span><span class="sxs-lookup"><span data-stu-id="b5388-777">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="b5388-778">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-778">**Mitigations**</span></span>

<span data-ttu-id="b5388-779">Použijte nový název.</span><span class="sxs-lookup"><span data-stu-id="b5388-779">Use the new name.</span></span> <span data-ttu-id="b5388-780">(Všimněte si, že číslo ID události se nezměnilo.)</span><span class="sxs-lookup"><span data-stu-id="b5388-780">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="b5388-781">Vysvětlení rozhraní API pro názvy omezení cizího klíče</span><span class="sxs-lookup"><span data-stu-id="b5388-781">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="b5388-782">Sledování problému #10730</span><span class="sxs-lookup"><span data-stu-id="b5388-782">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="b5388-783">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-783">**Old behavior**</span></span>

<span data-ttu-id="b5388-784">Před EF Core 3,0 se názvy omezení cizího klíče odkazovaly jenom na "název".</span><span class="sxs-lookup"><span data-stu-id="b5388-784">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="b5388-785">Příklad:</span><span class="sxs-lookup"><span data-stu-id="b5388-785">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="b5388-786">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-786">**New behavior**</span></span>

<span data-ttu-id="b5388-787">Počínaje EF Core 3,0 se názvy omezení cizích klíčů teď označují jako "název omezení".</span><span class="sxs-lookup"><span data-stu-id="b5388-787">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="b5388-788">Příklad:</span><span class="sxs-lookup"><span data-stu-id="b5388-788">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="b5388-789">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-789">**Why**</span></span>

<span data-ttu-id="b5388-790">Tato změna přináší konzistenci pro pojmenování v této oblasti a také vysvětluje, že se jedná o název omezení cizího klíče, a nikoli název sloupce nebo vlastnosti, ve kterém je definován cizí klíč.</span><span class="sxs-lookup"><span data-stu-id="b5388-790">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="b5388-791">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-791">**Mitigations**</span></span>

<span data-ttu-id="b5388-792">Použijte nový název.</span><span class="sxs-lookup"><span data-stu-id="b5388-792">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="b5388-793">IRelationalDatabaseCreator. HasTables/HasTablesAsync byly zveřejněny.</span><span class="sxs-lookup"><span data-stu-id="b5388-793">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="b5388-794">Sledování problému #15997</span><span class="sxs-lookup"><span data-stu-id="b5388-794">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="b5388-795">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-795">**Old behavior**</span></span>

<span data-ttu-id="b5388-796">Před EF Core 3,0 byly tyto metody chráněné.</span><span class="sxs-lookup"><span data-stu-id="b5388-796">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="b5388-797">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-797">**New behavior**</span></span>

<span data-ttu-id="b5388-798">Počínaje EF Core 3,0 jsou tyto metody veřejné.</span><span class="sxs-lookup"><span data-stu-id="b5388-798">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="b5388-799">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-799">**Why**</span></span>

<span data-ttu-id="b5388-800">Tyto metody jsou používány EF k určení, jestli je databáze vytvořená, ale prázdná.</span><span class="sxs-lookup"><span data-stu-id="b5388-800">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="b5388-801">To může být užitečné taky od vnějšího EF při určování, jestli se mají migrace použít.</span><span class="sxs-lookup"><span data-stu-id="b5388-801">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="b5388-802">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-802">**Mitigations**</span></span>

<span data-ttu-id="b5388-803">Změňte přístupnost všech přepsání.</span><span class="sxs-lookup"><span data-stu-id="b5388-803">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="b5388-804">Microsoft. EntityFrameworkCore. Design je teď balíček DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="b5388-804">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="b5388-805">Sledování problému #11506</span><span class="sxs-lookup"><span data-stu-id="b5388-805">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="b5388-806">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-806">**Old behavior**</span></span>

<span data-ttu-id="b5388-807">Před EF Core 3,0 byl Microsoft. EntityFrameworkCore. Design regulárním balíčkem NuGet, na kterém by mohly být na sestavení odkazovány projekty, které na něm závisejí.</span><span class="sxs-lookup"><span data-stu-id="b5388-807">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="b5388-808">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-808">**New behavior**</span></span>

<span data-ttu-id="b5388-809">Počínaje EF Core 3,0 se jedná o balíček DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="b5388-809">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="b5388-810">To znamená, že závislost nebude nijak přesměrovat do jiných projektů a že již nemůžete ve výchozím nastavení odkazovat na své sestavení.</span><span class="sxs-lookup"><span data-stu-id="b5388-810">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="b5388-811">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-811">**Why**</span></span>

<span data-ttu-id="b5388-812">Tento balíček se má použít jenom v době návrhu.</span><span class="sxs-lookup"><span data-stu-id="b5388-812">This package is only intended to be used at design time.</span></span> <span data-ttu-id="b5388-813">Nasazené aplikace by neměli na ni odkazovat.</span><span class="sxs-lookup"><span data-stu-id="b5388-813">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="b5388-814">Díky tomu, že balíček DevelopmentDependency, toto doporučení posiluje.</span><span class="sxs-lookup"><span data-stu-id="b5388-814">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="b5388-815">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-815">**Mitigations**</span></span>

<span data-ttu-id="b5388-816">Pokud potřebujete odkazovat na tento balíček, aby bylo možné přepsat EF Core chování při návrhu, můžete aktualizovat metadata položky aktualizovat PackageReference v projektu.</span><span class="sxs-lookup"><span data-stu-id="b5388-816">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="b5388-817">Pokud se na balíček odkazuje přes Microsoft. EntityFrameworkCore. Tools, budete muset do balíčku přidat explicitní PackageReference, aby se změnila jeho metadata.</span><span class="sxs-lookup"><span data-stu-id="b5388-817">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="b5388-818">SQLitePCL. Raw aktualizováno na verzi 2.0.0</span><span class="sxs-lookup"><span data-stu-id="b5388-818">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="b5388-819">Sledování problému #14824</span><span class="sxs-lookup"><span data-stu-id="b5388-819">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="b5388-820">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-820">**Old behavior**</span></span>

<span data-ttu-id="b5388-821">Microsoft. EntityFrameworkCore. sqlite byl dřív závislý na 1.1.12 verze SQLitePCL. Raw.</span><span class="sxs-lookup"><span data-stu-id="b5388-821">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="b5388-822">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-822">**New behavior**</span></span>

<span data-ttu-id="b5388-823">Aktualizovali jsme náš balíček tak, aby byl závislý na verzi 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="b5388-823">We've updated our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="b5388-824">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-824">**Why**</span></span>

<span data-ttu-id="b5388-825">2\.0.0 verze SQLitePCL. Raw TARGETS .NET Standard 2,0.</span><span class="sxs-lookup"><span data-stu-id="b5388-825">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="b5388-826">Dříve cílí na .NET Standard 1,1, které vyžadovaly, aby se při práci vytvořil velký uzávěr přenosných balíčků.</span><span class="sxs-lookup"><span data-stu-id="b5388-826">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="b5388-827">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-827">**Mitigations**</span></span>

<span data-ttu-id="b5388-828">SQLitePCL. Raw 2.0.0 verze obsahuje některé zásadní změny.</span><span class="sxs-lookup"><span data-stu-id="b5388-828">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="b5388-829">Podrobnosti najdete v [poznámkách k verzi](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) .</span><span class="sxs-lookup"><span data-stu-id="b5388-829">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="b5388-830">NetTopologySuite aktualizace na verzi 2.0.0</span><span class="sxs-lookup"><span data-stu-id="b5388-830">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="b5388-831">Sledování problému #14825</span><span class="sxs-lookup"><span data-stu-id="b5388-831">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="b5388-832">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-832">**Old behavior**</span></span>

<span data-ttu-id="b5388-833">Prostorové balíčky byly dříve závislé na 1.15.1 verze NetTopologySuite.</span><span class="sxs-lookup"><span data-stu-id="b5388-833">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="b5388-834">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-834">**New behavior**</span></span>

<span data-ttu-id="b5388-835">Náš balíček jsme aktualizovali tak, aby byl závislý na verzi 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="b5388-835">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="b5388-836">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-836">**Why**</span></span>

<span data-ttu-id="b5388-837">2\.0.0 verze NetTopologySuite má za cíl řešit několik problémů s použitelností, ke kterým se EF Core uživatelé setkali.</span><span class="sxs-lookup"><span data-stu-id="b5388-837">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="b5388-838">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-838">**Mitigations**</span></span>

<span data-ttu-id="b5388-839">NetTopologySuite verze 2.0.0 obsahuje některé průlomové změny.</span><span class="sxs-lookup"><span data-stu-id="b5388-839">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="b5388-840">Podrobnosti najdete v [poznámkách k verzi](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) .</span><span class="sxs-lookup"><span data-stu-id="b5388-840">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="SqlClient"></a>

### <a name="microsoftdatasqlclient-is-used-instead-of-systemdatasqlclient"></a><span data-ttu-id="b5388-841">Místo typu System. data. SqlClient se používá Microsoft. data. SqlClient.</span><span class="sxs-lookup"><span data-stu-id="b5388-841">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>

[<span data-ttu-id="b5388-842">Sledování problému #15636</span><span class="sxs-lookup"><span data-stu-id="b5388-842">Tracking Issue #15636</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15636)

<span data-ttu-id="b5388-843">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-843">**Old behavior**</span></span>

<span data-ttu-id="b5388-844">Microsoft. EntityFrameworkCore. SqlServer byl dřív závislý na System. data. SqlClient.</span><span class="sxs-lookup"><span data-stu-id="b5388-844">Microsoft.EntityFrameworkCore.SqlServer previously depended on System.Data.SqlClient.</span></span>

<span data-ttu-id="b5388-845">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-845">**New behavior**</span></span>

<span data-ttu-id="b5388-846">Balíček jsme aktualizovali tak, aby byl závislý na Microsoft. data. SqlClient.</span><span class="sxs-lookup"><span data-stu-id="b5388-846">We've updated our package to depend on Microsoft.Data.SqlClient.</span></span>

<span data-ttu-id="b5388-847">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-847">**Why**</span></span>

<span data-ttu-id="b5388-848">Microsoft. data. SqlClient je nejdůležitější ovladač pro přístup k datům, který je k dispozici pro SQL Server a System. data. SqlClient již není zaměřuje na vývoj.</span><span class="sxs-lookup"><span data-stu-id="b5388-848">Microsoft.Data.SqlClient is the flagship data access driver for SQL Server going forward, and System.Data.SqlClient no longer be the focus of development.</span></span>
<span data-ttu-id="b5388-849">Některé důležité funkce, například Always Encrypted, jsou k dispozici pouze v Microsoft. data. SqlClient.</span><span class="sxs-lookup"><span data-stu-id="b5388-849">Some important features, such as Always Encrypted, are only available on Microsoft.Data.SqlClient.</span></span>

<span data-ttu-id="b5388-850">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-850">**Mitigations**</span></span>

<span data-ttu-id="b5388-851">Pokud váš kód používá přímou závislost na System. data. SqlClient, musíte ho změnit tak, aby odkazoval na Microsoft. data. SqlClient místo toho. vzhledem k tomu, že oba balíčky udržují velmi vysoký stupeň kompatibility rozhraní API, mělo by to být jenom jednoduchý balíček a Změna oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="b5388-851">If your code takes a direct dependency on System.Data.SqlClient, you must change it to reference Microsoft.Data.SqlClient instead; as the two packages maintain a very high degree of API compatibility, this should only be a simple package and namespace change.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="b5388-852">Je nutné nakonfigurovat více dvojznačných relací odkazujících na sebe.</span><span class="sxs-lookup"><span data-stu-id="b5388-852">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="b5388-853">Sledování problému #13573</span><span class="sxs-lookup"><span data-stu-id="b5388-853">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="b5388-854">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-854">**Old behavior**</span></span>

<span data-ttu-id="b5388-855">Typ entity s více jednosměrnou navigační vlastností a s vyhovující FKs byl nesprávně nakonfigurován jako jeden vztah.</span><span class="sxs-lookup"><span data-stu-id="b5388-855">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="b5388-856">Příklad:</span><span class="sxs-lookup"><span data-stu-id="b5388-856">For example:</span></span>

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

<span data-ttu-id="b5388-857">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-857">**New behavior**</span></span>

<span data-ttu-id="b5388-858">Tento scénář je nyní zjištěn v sestavování modelu a je vyvolána výjimka označující, že je model dvojznačný.</span><span class="sxs-lookup"><span data-stu-id="b5388-858">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="b5388-859">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-859">**Why**</span></span>

<span data-ttu-id="b5388-860">Výsledný model byl nejednoznačný a pravděpodobně bude pro tento případ obvykle špatný.</span><span class="sxs-lookup"><span data-stu-id="b5388-860">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="b5388-861">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-861">**Mitigations**</span></span>

<span data-ttu-id="b5388-862">Použijte úplnou konfiguraci relace.</span><span class="sxs-lookup"><span data-stu-id="b5388-862">Use full configuration of the relationship.</span></span> <span data-ttu-id="b5388-863">Příklad:</span><span class="sxs-lookup"><span data-stu-id="b5388-863">For example:</span></span>

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
### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a><span data-ttu-id="b5388-864">DbFunction. Schema má hodnotu null nebo je prázdný řetězec, který nakonfiguruje, aby byl ve výchozím schématu modelu.</span><span class="sxs-lookup"><span data-stu-id="b5388-864">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>

[<span data-ttu-id="b5388-865">Sledování problému #12757</span><span class="sxs-lookup"><span data-stu-id="b5388-865">Tracking Issue #12757</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12757)

<span data-ttu-id="b5388-866">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-866">**Old behavior**</span></span>

<span data-ttu-id="b5388-867">DbFunction nakonfigurovaný se schématem jako prázdný řetězec byl považován za vestavěnou funkci bez schématu.</span><span class="sxs-lookup"><span data-stu-id="b5388-867">A DbFunction configured with schema as an empty string was treated as built-in function without a schema.</span></span> <span data-ttu-id="b5388-868">Například následující kód bude mapován `DatePart` funkce CLR na `DATEPART` vestavěnou funkci na SqlServer.</span><span class="sxs-lookup"><span data-stu-id="b5388-868">For example following code will map `DatePart` CLR function to `DATEPART` built-in function on SqlServer.</span></span>

```C#
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

<span data-ttu-id="b5388-869">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="b5388-869">**New behavior**</span></span>

<span data-ttu-id="b5388-870">Všechna mapování DbFunction se považují za namapovaná na uživatelsky definované funkce.</span><span class="sxs-lookup"><span data-stu-id="b5388-870">All DbFunction mappings are considered to be mapped to user defined functions.</span></span> <span data-ttu-id="b5388-871">Proto je prázdná hodnota řetězce vložena do výchozího schématu pro model.</span><span class="sxs-lookup"><span data-stu-id="b5388-871">Hence empty string value would put the function inside the default schema for the model.</span></span> <span data-ttu-id="b5388-872">V opačném případě se může jednat o schéma nakonfigurované explicitně prostřednictvím rozhraní Fluent API `modelBuilder.HasDefaultSchema()` nebo `dbo`.</span><span class="sxs-lookup"><span data-stu-id="b5388-872">Which could be the schema configured explicitly via fluent API `modelBuilder.HasDefaultSchema()` or `dbo` otherwise.</span></span>

<span data-ttu-id="b5388-873">**Proč**</span><span class="sxs-lookup"><span data-stu-id="b5388-873">**Why**</span></span>

<span data-ttu-id="b5388-874">Dříve prázdné schéma bylo způsobem, jak se zacházet s touto funkcí, ale tato funkce je k dispozici pouze pro SqlServer, kde předdefinované funkce nepatří do žádného schématu.</span><span class="sxs-lookup"><span data-stu-id="b5388-874">Previously schema being empty was a way to treat that function is built-in but that logic is only applicable for SqlServer where built-in functions do not belong to any schema.</span></span>

<span data-ttu-id="b5388-875">**Hrozeb**</span><span class="sxs-lookup"><span data-stu-id="b5388-875">**Mitigations**</span></span>

<span data-ttu-id="b5388-876">Nakonfigurujte převod DbFunction ručně, abyste ho namapovali na vestavěnou funkci.</span><span class="sxs-lookup"><span data-stu-id="b5388-876">Configure DbFunction's translation manually to map it to a built-in function.</span></span>

```C#
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```
