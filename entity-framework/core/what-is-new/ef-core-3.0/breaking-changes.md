---
title: Přerušující změny v EF Core 3,0 – EF Core
author: ajcvickers
ms.date: 12/03/2019
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: d614103169837238810fabd0a8889043c851ef14
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824863"
---
# <a name="breaking-changes-included-in-ef-core-30"></a><span data-ttu-id="bd2dc-102">Přerušující změny zahrnuté v EF Core 3,0</span><span class="sxs-lookup"><span data-stu-id="bd2dc-102">Breaking changes included in EF Core 3.0</span></span>

<span data-ttu-id="bd2dc-103">Následující změny rozhraní API a chování mají možnost rušit existující aplikace při jejich upgradu na 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-103">The following API and behavior changes have the potential to break existing applications when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="bd2dc-104">Změny, které očekáváme jenom o to, aby ovlivnili pouze poskytovatele databází, jsou popsané v části [změny zprostředkovatele](xref:core/providers/provider-log).</span><span class="sxs-lookup"><span data-stu-id="bd2dc-104">Changes that we expect to only impact database providers are documented under [provider changes](xref:core/providers/provider-log).</span></span>

## <a name="summary"></a><span data-ttu-id="bd2dc-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="bd2dc-105">Summary</span></span>

| <span data-ttu-id="bd2dc-106">**Zásadní změna**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-106">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="bd2dc-107">**Dopad**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-107">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="bd2dc-108">Dotazy LINQ již nejsou vyhodnocovány na klientovi.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-108">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="bd2dc-109">Vysoký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-109">High</span></span>       |
| [<span data-ttu-id="bd2dc-110">EF Core 3,0 cíle .NET Standard 2,1 místo .NET Standard 2,0</span><span class="sxs-lookup"><span data-stu-id="bd2dc-110">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="bd2dc-111">Vysoký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-111">High</span></span>      |
| [<span data-ttu-id="bd2dc-112">EF Core nástroj příkazového řádku dotnet EF již není součástí .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="bd2dc-112">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="bd2dc-113">Vysoký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-113">High</span></span>      |
| [<span data-ttu-id="bd2dc-114">DetectChanges respektuje hodnoty klíčů generované úložištěm.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-114">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="bd2dc-115">Vysoký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-115">High</span></span>      |
| [<span data-ttu-id="bd2dc-116">Z tabulek, ExecuteSql a ExecuteSqlAsync byly přejmenovány.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-116">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="bd2dc-117">Vysoký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-117">High</span></span>      |
| [<span data-ttu-id="bd2dc-118">Typy dotazů jsou konsolidovány s typy entit</span><span class="sxs-lookup"><span data-stu-id="bd2dc-118">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="bd2dc-119">Vysoký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-119">High</span></span>      |
| [<span data-ttu-id="bd2dc-120">Entity Framework Core už není součástí sdílené ASP.NET Core architektury.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-120">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="bd2dc-121">Střední</span><span class="sxs-lookup"><span data-stu-id="bd2dc-121">Medium</span></span>      |
| [<span data-ttu-id="bd2dc-122">Odstranění kaskádových operací se teď ve výchozím nastavení provádí hned.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-122">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="bd2dc-123">Střední</span><span class="sxs-lookup"><span data-stu-id="bd2dc-123">Medium</span></span>      |
| [<span data-ttu-id="bd2dc-124">Eager načítání souvisejících entit se teď děje v jednom dotazu.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-124">Eager loading of related entities now happens in a single query</span></span>](#eager-loading-single-query) | <span data-ttu-id="bd2dc-125">Střední</span><span class="sxs-lookup"><span data-stu-id="bd2dc-125">Medium</span></span>      |
| [<span data-ttu-id="bd2dc-126">DeleteBehavior. restrict má sémantiku čištění.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-126">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="bd2dc-127">Střední</span><span class="sxs-lookup"><span data-stu-id="bd2dc-127">Medium</span></span>      |
| [<span data-ttu-id="bd2dc-128">Změnilo se konfigurační rozhraní API pro vztahy vlastněných typů.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-128">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="bd2dc-129">Střední</span><span class="sxs-lookup"><span data-stu-id="bd2dc-129">Medium</span></span>      |
| [<span data-ttu-id="bd2dc-130">Každá vlastnost používá nezávislou generaci celočíselného klíče v paměti.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-130">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="bd2dc-131">Střední</span><span class="sxs-lookup"><span data-stu-id="bd2dc-131">Medium</span></span>      |
| [<span data-ttu-id="bd2dc-132">Žádné dotazy pro sledování neprovádějí překlad identity</span><span class="sxs-lookup"><span data-stu-id="bd2dc-132">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="bd2dc-133">Střední</span><span class="sxs-lookup"><span data-stu-id="bd2dc-133">Medium</span></span>      |
| [<span data-ttu-id="bd2dc-134">Změny rozhraní API pro metadata</span><span class="sxs-lookup"><span data-stu-id="bd2dc-134">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="bd2dc-135">Střední</span><span class="sxs-lookup"><span data-stu-id="bd2dc-135">Medium</span></span>      |
| [<span data-ttu-id="bd2dc-136">Změny rozhraní API pro konkrétního zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="bd2dc-136">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="bd2dc-137">Střední</span><span class="sxs-lookup"><span data-stu-id="bd2dc-137">Medium</span></span>      |
| [<span data-ttu-id="bd2dc-138">UseRowNumberForPaging se odebral.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-138">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="bd2dc-139">Střední</span><span class="sxs-lookup"><span data-stu-id="bd2dc-139">Medium</span></span>      |
| [<span data-ttu-id="bd2dc-140">Metoda Z tabulek při použití s uloženou procedurou nemůže být složená.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-140">FromSql method when used with stored procedure cannot be composed</span></span>](#fromsqlsproc) | <span data-ttu-id="bd2dc-141">Střední</span><span class="sxs-lookup"><span data-stu-id="bd2dc-141">Medium</span></span>      |
| [<span data-ttu-id="bd2dc-142">Metody Z tabulek se dají zadat jenom v kořenech dotazů.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-142">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="bd2dc-143">Nízký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-143">Low</span></span>      |
| [<span data-ttu-id="bd2dc-144">~~Provádění dotazu se protokoluje na úrovni ladění~~ . Vrátit</span><span class="sxs-lookup"><span data-stu-id="bd2dc-144">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="bd2dc-145">Nízký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-145">Low</span></span>      |
| [<span data-ttu-id="bd2dc-146">Dočasné hodnoty klíčů už nejsou nastavené na instance entit.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-146">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="bd2dc-147">Nízký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-147">Low</span></span>      |
| [<span data-ttu-id="bd2dc-148">Závislé entity, které sdílí tabulku s objektem zabezpečení, jsou teď volitelné.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-148">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="bd2dc-149">Nízký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-149">Low</span></span>      |
| [<span data-ttu-id="bd2dc-150">Všechny entity sdílející tabulku se sloupcem souběžného tokenu musí být namapovány na vlastnost.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-150">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="bd2dc-151">Nízký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-151">Low</span></span>      |
| [<span data-ttu-id="bd2dc-152">Na vlastněné entity se nedá dotazovat bez vlastníka pomocí sledovacího dotazu.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-152">Owned entities cannot be queried without the owner using a tracking query</span></span>](#owned-query) | <span data-ttu-id="bd2dc-153">Nízký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-153">Low</span></span>      |
| [<span data-ttu-id="bd2dc-154">Zděděné vlastnosti z nemapovaných typů jsou nyní namapovány na jeden sloupec pro všechny odvozené typy.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-154">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="bd2dc-155">Nízký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-155">Low</span></span>      |
| [<span data-ttu-id="bd2dc-156">Konvence vlastností cizího klíče už neodpovídá stejnému názvu jako vlastnost Principal.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-156">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="bd2dc-157">Nízký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-157">Low</span></span>      |
| [<span data-ttu-id="bd2dc-158">Připojení k databázi je teď zavřené, pokud už nepoužíváte dřív, než se dokončí jeho objekt TransactionScope.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-158">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="bd2dc-159">Nízký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-159">Low</span></span>      |
| [<span data-ttu-id="bd2dc-160">Ve výchozím nastavení se používají pole pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-160">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="bd2dc-161">Nízký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-161">Low</span></span>      |
| [<span data-ttu-id="bd2dc-162">Vyvolat, zda je nalezeno více kompatibilních zálohovaných polí</span><span class="sxs-lookup"><span data-stu-id="bd2dc-162">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="bd2dc-163">Nízký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-163">Low</span></span>      |
| [<span data-ttu-id="bd2dc-164">Názvy vlastností pouze pro pole se musí shodovat s názvem pole.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-164">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="bd2dc-165">Nízký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-165">Low</span></span>      |
| [<span data-ttu-id="bd2dc-166">AddDbContext/AddDbContextPool už nevolá AddLogging a AddMemoryCache.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-166">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="bd2dc-167">Nízký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-167">Low</span></span>      |
| [<span data-ttu-id="bd2dc-168">AddEntityFramework \* přidá IMemoryCache s omezením velikosti.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-168">AddEntityFramework\* adds IMemoryCache with a size limit</span></span>](#addentityframework-adds-imemorycache-with-a-size-limit) | <span data-ttu-id="bd2dc-169">Nízký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-169">Low</span></span>      |
| [<span data-ttu-id="bd2dc-170">DbContext. entry teď provádí místní DetectChanges.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-170">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="bd2dc-171">Nízký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-171">Low</span></span>      |
| [<span data-ttu-id="bd2dc-172">Klíče řetězce a pole bajtů nejsou ve výchozím nastavení generovány klientem.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-172">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="bd2dc-173">Nízký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-173">Low</span></span>      |
| [<span data-ttu-id="bd2dc-174">ILoggerFactory je teď služba s vymezeným oborem.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-174">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="bd2dc-175">Nízký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-175">Low</span></span>      |
| [<span data-ttu-id="bd2dc-176">Opožděné načítání proxy serverů už nepředpokládá navigační vlastnosti, jsou plně načtené.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-176">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="bd2dc-177">Nízký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-177">Low</span></span>      |
| [<span data-ttu-id="bd2dc-178">Nadměrné vytváření interních zprostředkovatelů služeb je teď ve výchozím nastavení chyba.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-178">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="bd2dc-179">Nízký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-179">Low</span></span>      |
| [<span data-ttu-id="bd2dc-180">Nové chování pro HasOne/HasMany se volá s jedním řetězcem.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-180">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="bd2dc-181">Nízký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-181">Low</span></span>      |
| [<span data-ttu-id="bd2dc-182">Návratový typ pro několik asynchronních metod byl změněn z úlohy na ValueTask</span><span class="sxs-lookup"><span data-stu-id="bd2dc-182">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="bd2dc-183">Nízký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-183">Low</span></span>      |
| [<span data-ttu-id="bd2dc-184">Relační: anotace TypeMapping je nyní pouze TypeMapping</span><span class="sxs-lookup"><span data-stu-id="bd2dc-184">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="bd2dc-185">Nízký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-185">Low</span></span>      |
| [<span data-ttu-id="bd2dc-186">ToTable na odvozeném typu vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-186">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="bd2dc-187">Nízký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-187">Low</span></span>      |
| [<span data-ttu-id="bd2dc-188">EF Core už neposílá direktivu pragma pro vynucení KOFK SQLite.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-188">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="bd2dc-189">Nízký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-189">Low</span></span>      |
| [<span data-ttu-id="bd2dc-190">Microsoft. EntityFrameworkCore. sqlite teď závisí na SQLitePCLRaw. bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="bd2dc-190">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="bd2dc-191">Nízký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-191">Low</span></span>      |
| [<span data-ttu-id="bd2dc-192">Hodnoty GUID se teď ukládají jako TEXT na SQLite.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-192">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="bd2dc-193">Nízký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-193">Low</span></span>      |
| [<span data-ttu-id="bd2dc-194">Hodnoty char se teď ukládají jako TEXT na SQLite.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-194">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="bd2dc-195">Nízký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-195">Low</span></span>      |
| [<span data-ttu-id="bd2dc-196">ID migrace se teď generují pomocí kalendáře invariantní jazykové verze.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-196">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="bd2dc-197">Nízký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-197">Low</span></span>      |
| [<span data-ttu-id="bd2dc-198">Informace o rozšíření/metadata se odebraly z IDbContextOptionsExtension.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-198">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="bd2dc-199">Nízký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-199">Low</span></span>      |
| [<span data-ttu-id="bd2dc-200">LogQueryPossibleExceptionWithAggregateOperator se přejmenovalo.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-200">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="bd2dc-201">Nízký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-201">Low</span></span>      |
| [<span data-ttu-id="bd2dc-202">Vysvětlení rozhraní API pro názvy omezení cizího klíče</span><span class="sxs-lookup"><span data-stu-id="bd2dc-202">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="bd2dc-203">Nízký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-203">Low</span></span>      |
| [<span data-ttu-id="bd2dc-204">IRelationalDatabaseCreator. HasTables/HasTablesAsync byly zveřejněny.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-204">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="bd2dc-205">Nízký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-205">Low</span></span>      |
| [<span data-ttu-id="bd2dc-206">Microsoft. EntityFrameworkCore. Design je teď balíček DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-206">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="bd2dc-207">Nízký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-207">Low</span></span>      |
| [<span data-ttu-id="bd2dc-208">SQLitePCL. Raw aktualizováno na verzi 2.0.0</span><span class="sxs-lookup"><span data-stu-id="bd2dc-208">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="bd2dc-209">Nízký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-209">Low</span></span>      |
| [<span data-ttu-id="bd2dc-210">NetTopologySuite aktualizace na verzi 2.0.0</span><span class="sxs-lookup"><span data-stu-id="bd2dc-210">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="bd2dc-211">Nízký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-211">Low</span></span>      |
| [<span data-ttu-id="bd2dc-212">Místo typu System. data. SqlClient se používá Microsoft. data. SqlClient.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-212">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>](#SqlClient) | <span data-ttu-id="bd2dc-213">Nízký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-213">Low</span></span>      |
| [<span data-ttu-id="bd2dc-214">Je nutné nakonfigurovat více dvojznačných relací odkazujících na sebe.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-214">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="bd2dc-215">Nízký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-215">Low</span></span>      |
| [<span data-ttu-id="bd2dc-216">DbFunction. Schema má hodnotu null nebo je prázdný řetězec, který nakonfiguruje, aby byl ve výchozím schématu modelu.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-216">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>](#udf-empty-string) | <span data-ttu-id="bd2dc-217">Nízký</span><span class="sxs-lookup"><span data-stu-id="bd2dc-217">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="bd2dc-218">Dotazy LINQ již nejsou vyhodnocovány na klientovi.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-218">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="bd2dc-219">[Sledování problému #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[také zobrazit #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="bd2dc-219">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="bd2dc-220">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-220">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-221">Před 3,0, když EF Core nedokázal převést výraz, který byl součástí dotazu na buď SQL, nebo parametr, automaticky vyhodnotí výraz na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-221">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="bd2dc-222">Ve výchozím nastavení vyhodnocování klientů u potenciálně nákladných výrazů aktivovalo pouze upozornění.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-222">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="bd2dc-223">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-223">**New behavior**</span></span>

<span data-ttu-id="bd2dc-224">Počínaje 3,0 EF Core v projekci nejvyšší úrovně povolit jenom výrazy (poslední `Select()` volání v dotazu), které se mají vyhodnotit na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-224">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="bd2dc-225">Pokud výrazy v jakékoli jiné části dotazu nelze převést na buď SQL, nebo parametr, je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-225">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="bd2dc-226">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-226">**Why**</span></span>

<span data-ttu-id="bd2dc-227">Automatické hodnocení dotazů umožňuje provádět mnoho dotazů i v případě, že není možné přeložit jejich důležité části.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-227">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="bd2dc-228">Toto chování může vést k neočekávanému a potenciálně škodlivému chování, které může být jen zřejmé v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-228">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="bd2dc-229">Například podmínka ve volání `Where()`, která se nedá přeložit, může způsobit, že se všechny řádky z tabulky přenesou z databázového serveru a filtr, který se má použít na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-229">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="bd2dc-230">Tato situace může snadno přejít zpět, pokud tabulka obsahuje jenom několik řádků ve vývoji, ale při přesunu aplikace do produkčního prostředí zasáhnout, kde tabulka může obsahovat miliony řádků.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-230">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="bd2dc-231">Upozornění na vyhodnocení klientů se také během vývoje ukázala jako příliš jednoduchá.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-231">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="bd2dc-232">Kromě toho může automatické hodnocení klienta vést k problémům s tím, že vylepšení překladu dotazů pro konkrétní výrazy způsobilo nezamýšlené neúmyslné změny mezi verzemi.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-232">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="bd2dc-233">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-233">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-234">Pokud dotaz nelze plně přeložit, přepište ho do formuláře, který se dá přeložit, nebo použijte `AsEnumerable()`, `ToList()`nebo podobným způsobem, aby se data vrátila zpět do klienta, kde je pak možné dále zpracovávat pomocí LINQ-to-Objects.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-234">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="bd2dc-235">EF Core 3,0 cíle .NET Standard 2,1 místo .NET Standard 2,0</span><span class="sxs-lookup"><span data-stu-id="bd2dc-235">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="bd2dc-236">Sledování problému #15498</span><span class="sxs-lookup"><span data-stu-id="bd2dc-236">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

<span data-ttu-id="bd2dc-237">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-237">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-238">Před 3,0 EF Core cílené .NET Standard 2,0 a spustí se na všech platformách, které podporují tento standard, včetně .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-238">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="bd2dc-239">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-239">**New behavior**</span></span>

<span data-ttu-id="bd2dc-240">Počínaje 3,0 se EF Core cíle .NET Standard 2,1 a spustí se na všech platformách, které podporují tento standard.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-240">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="bd2dc-241">To nezahrnuje .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-241">This does not include .NET Framework.</span></span>

<span data-ttu-id="bd2dc-242">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-242">**Why**</span></span>

<span data-ttu-id="bd2dc-243">Toto je součást strategického rozhodnutí napříč technologiemi .NET a zaměřuje se na energii na platformě .NET Core a dalších moderních platformách .NET, jako je Xamarin.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-243">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="bd2dc-244">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-244">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-245">Zvažte přechod na moderní platformu .NET.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-245">Consider moving to a modern .NET platform.</span></span> <span data-ttu-id="bd2dc-246">Pokud to není možné, pak pokračujte v používání EF Core 2,1 nebo EF Core 2,2, které podporují .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-246">If this is not possible, then continue to use EF Core 2.1 or EF Core 2.2, both of which support .NET Framework.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="bd2dc-247">Entity Framework Core už není součástí sdílené ASP.NET Core architektury.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-247">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="bd2dc-248">Sledování oznámení o problémech # 325</span><span class="sxs-lookup"><span data-stu-id="bd2dc-248">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="bd2dc-249">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-249">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-250">Před ASP.NET Core 3,0 se při přidání odkazu na balíček do `Microsoft.AspNetCore.App` nebo `Microsoft.AspNetCore.All`zahrnuly EF Core a někteří poskytovatelé EF Core dat jako poskytovatelé SQL Server.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-250">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="bd2dc-251">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-251">**New behavior**</span></span>

<span data-ttu-id="bd2dc-252">Počínaje 3,0 ASP.NET Core sdílené rozhraní nezahrnuje EF Core nebo žádné poskytovatele EF Core dat.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-252">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="bd2dc-253">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-253">**Why**</span></span>

<span data-ttu-id="bd2dc-254">Před touto změnou EF Core nutné jiné kroky v závislosti na tom, zda je aplikace cílena ASP.NET Core a SQL Server.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-254">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="bd2dc-255">Upgrade ASP.NET Core taky vynutil upgrade EF Core a poskytovatele SQL Server, který není vždycky žádoucí.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-255">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="bd2dc-256">V důsledku této změny je prostředí získání EF Core stejné u všech poskytovatelů, podporovaných implementací rozhraní .NET a typů aplikací.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-256">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="bd2dc-257">Vývojáři teď můžou přesně řídit, když EF Core a EF Core se upgradují poskytovatelé dat.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-257">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="bd2dc-258">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-258">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-259">Chcete-li použít EF Core v aplikaci ASP.NET Core 3,0 nebo v jakékoli jiné podporované aplikaci, explicitně přidejte odkaz na balíček EF Coreho poskytovatele databáze, který bude aplikace používat.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-259">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="bd2dc-260">EF Core nástroj příkazového řádku dotnet EF již není součástí .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="bd2dc-260">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="bd2dc-261">Sledování problému #14016</span><span class="sxs-lookup"><span data-stu-id="bd2dc-261">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="bd2dc-262">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-262">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-263">Před 3,0 byl nástroj `dotnet ef` součástí .NET Core SDK a byl snadno dostupný pro použití z příkazového řádku z libovolného projektu bez nutnosti dalších kroků.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-263">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="bd2dc-264">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-264">**New behavior**</span></span>

<span data-ttu-id="bd2dc-265">Počínaje 3,0 sada .NET SDK neobsahuje nástroj `dotnet ef`, takže před tím, než ho bude možné použít, je nutné explicitně nainstalovat jako místní nebo globální nástroj.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-265">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="bd2dc-266">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-266">**Why**</span></span>

<span data-ttu-id="bd2dc-267">Tato změna nám umožňuje distribuovat a aktualizovat `dotnet ef` jako regulární nástroj rozhraní příkazového řádku .NET v NuGet, a to v souladu se skutečností, že EF Core 3,0 je také vždy distribuován jako balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-267">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="bd2dc-268">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-268">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-269">Aby bylo možné spravovat migrace nebo `DbContext`uživatelského rozhraní, nainstalujte `dotnet-ef` jako globální nástroj:</span><span class="sxs-lookup"><span data-stu-id="bd2dc-269">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef
  ```

<span data-ttu-id="bd2dc-270">Můžete také získat místní nástroj při obnovování závislostí projektu, který deklaruje jako závislost nástrojů pomocí [souboru manifestu nástroje](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="bd2dc-270">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="bd2dc-271">Z tabulek, ExecuteSql a ExecuteSqlAsync byly přejmenovány.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-271">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="bd2dc-272">Sledování problému #10996</span><span class="sxs-lookup"><span data-stu-id="bd2dc-272">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="bd2dc-273">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-273">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-274">Před EF Core 3,0 byly tyto názvy metod přetíženy, aby fungovaly s normálním řetězcem nebo řetězcem, které by měly být interpolované na SQL a parametry.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-274">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="bd2dc-275">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-275">**New behavior**</span></span>

<span data-ttu-id="bd2dc-276">Počínaje EF Core 3,0 použijte `FromSqlRaw`, `ExecuteSqlRaw`a `ExecuteSqlRawAsync` k vytvoření parametrizovaného dotazu, kde jsou parametry předány nezávisle na řetězci dotazu.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-276">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="bd2dc-277">Příklad:</span><span class="sxs-lookup"><span data-stu-id="bd2dc-277">For example:</span></span>

```csharp
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="bd2dc-278">Pomocí `FromSqlInterpolated`, `ExecuteSqlInterpolated`a `ExecuteSqlInterpolatedAsync` vytvořte parametrizovaný dotaz, ve kterém jsou parametry předány jako součást interpolované řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-278">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="bd2dc-279">Příklad:</span><span class="sxs-lookup"><span data-stu-id="bd2dc-279">For example:</span></span>

```csharp
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="bd2dc-280">Všimněte si, že obě výše uvedené dotazy vytvoří stejný parametrizovaný SQL se stejnými parametry SQL.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-280">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="bd2dc-281">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-281">**Why**</span></span>

<span data-ttu-id="bd2dc-282">Přetížení metody, jako to, usnadňuje náhodné volání nezpracované řetězcové metody, pokud by záměr byl zavolat interpolovaná řetězcová metoda a druhá možnost kolem.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-282">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="bd2dc-283">To může vést k tomu, že dotazy nejsou parametrizované, pokud by měly být.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-283">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="bd2dc-284">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-284">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-285">Přepněte na použití nových názvů metod.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-285">Switch to use the new method names.</span></span>

<a name="fromsqlsproc"></a>
### <a name="fromsql-method-when-used-with-stored-procedure-cannot-be-composed"></a><span data-ttu-id="bd2dc-286">Metoda Z tabulek při použití s uloženou procedurou nemůže být složená.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-286">FromSql method when used with stored procedure cannot be composed</span></span>

[<span data-ttu-id="bd2dc-287">Sledování problému #15392</span><span class="sxs-lookup"><span data-stu-id="bd2dc-287">Tracking Issue #15392</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15392)

<span data-ttu-id="bd2dc-288">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-288">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-289">Před EF Core 3,0 se metoda Z tabulek pokusila zjistit, zda je možné sestavit předaný SQL.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-289">Before EF Core 3.0, FromSql method tried to detect if the passed SQL can be composed upon.</span></span> <span data-ttu-id="bd2dc-290">Vyvolalo se hodnocení klienta, pokud SQL bylo bez možnosti složení, jako je uložená procedura.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-290">It did client evaluation when the SQL was non-composable like a stored procedure.</span></span> <span data-ttu-id="bd2dc-291">Následující dotaz fungoval spuštěním uložené procedury na serveru a provedením FirstOrDefault na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-291">The following query worked by running the stored procedure on the server and doing FirstOrDefault on the client side.</span></span>

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").FirstOrDefault();
```

<span data-ttu-id="bd2dc-292">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-292">**New behavior**</span></span>

<span data-ttu-id="bd2dc-293">Počínaje EF Core 3,0 se EF Core nepokusí analyzovat SQL.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-293">Starting with EF Core 3.0, EF Core will not try to parse the SQL.</span></span> <span data-ttu-id="bd2dc-294">Takže pokud vytváříte po FromSqlRaw/FromSqlInterpolated, pak EF Core vytvoří příkaz SQL, který by způsobil dotaz sub.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-294">So if you are composing after FromSqlRaw/FromSqlInterpolated, then EF Core will compose the SQL by causing sub query.</span></span> <span data-ttu-id="bd2dc-295">Takže pokud používáte uloženou proceduru se složením, zobrazí se výjimka pro neplatnou syntaxi SQL.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-295">So if you are using a stored procedure with composition then you will get an exception for invalid SQL syntax.</span></span>

<span data-ttu-id="bd2dc-296">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-296">**Why**</span></span>

<span data-ttu-id="bd2dc-297">EF Core 3,0 nepodporuje automatické hodnocení klienta, protože to bylo náchylné k chybám, jak je vysvětleno [zde](#linq-queries-are-no-longer-evaluated-on-the-client).</span><span class="sxs-lookup"><span data-stu-id="bd2dc-297">EF Core 3.0 does not support automatic client evaluation, since it was error prone as explained [here](#linq-queries-are-no-longer-evaluated-on-the-client).</span></span>

<span data-ttu-id="bd2dc-298">**Zmírnění**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-298">**Mitigation**</span></span>

<span data-ttu-id="bd2dc-299">Pokud používáte uloženou proceduru v FromSqlRaw/FromSqlInterpolated, znamená to, že se na ni nelze založit, takže můžete přidat __AsEnumerable/AsAsyncEnumerable__ hned po volání metody z tabulek, aby se zabránilo jakémukoli složení na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-299">If you are using a stored procedure in FromSqlRaw/FromSqlInterpolated, you know that it cannot be composed upon, so you can add __AsEnumerable/AsAsyncEnumerable__ right after the FromSql method call to avoid any composition on server side.</span></span>

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").AsEnumerable().FirstOrDefault();
```

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="bd2dc-300">Metody Z tabulek se dají zadat jenom v kořenech dotazů.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-300">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="bd2dc-301">Sledování problému #15704</span><span class="sxs-lookup"><span data-stu-id="bd2dc-301">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="bd2dc-302">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-302">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-303">Před EF Core 3,0 lze zadat `FromSql` metodu, která je kdekoli v dotazu.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-303">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="bd2dc-304">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-304">**New behavior**</span></span>

<span data-ttu-id="bd2dc-305">Počínaje EF Core 3,0 lze nové metody `FromSqlRaw` a `FromSqlInterpolated` (které nahrazují `FromSql`) zadat pouze v kořenech dotazu, tj. přímo na `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-305">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="bd2dc-306">Pokud se pokusíte zadat jiné místo jinak, dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-306">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="bd2dc-307">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-307">**Why**</span></span>

<span data-ttu-id="bd2dc-308">Určení `FromSql` kdekoli jinde než na `DbSet` neobsahovalo žádný význam nebo přidaná hodnota a v některých scénářích může způsobit nejednoznačnost.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-308">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="bd2dc-309">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-309">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-310">volání `FromSql` by se měla přesunout přímo na `DbSet`, na které se vztahují.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-310">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="bd2dc-311">Žádné dotazy pro sledování neprovádějí překlad identity</span><span class="sxs-lookup"><span data-stu-id="bd2dc-311">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="bd2dc-312">Sledování problému #13518</span><span class="sxs-lookup"><span data-stu-id="bd2dc-312">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="bd2dc-313">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-313">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-314">Před EF Core 3,0 se stejná instance entity používá pro všechny výskyty entity se zadaným typem a ID.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-314">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="bd2dc-315">To odpovídá chování sledovacích dotazů.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-315">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="bd2dc-316">Například tento dotaz:</span><span class="sxs-lookup"><span data-stu-id="bd2dc-316">For example, this query:</span></span>

```csharp
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="bd2dc-317">vrátí stejnou instanci `Category` pro každý `Product` přidruženou k dané kategorii.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-317">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="bd2dc-318">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-318">**New behavior**</span></span>

<span data-ttu-id="bd2dc-319">Počínaje EF Core 3,0 budou vytvořeny různé instance entit při výskytu entity se zadaným typem a ID na různých místech vráceného grafu.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-319">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="bd2dc-320">Například dotaz výše bude nyní vracet novou instanci `Category` pro každý `Product`, i když jsou ke stejné kategorii přidruženy dva produkty.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-320">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="bd2dc-321">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-321">**Why**</span></span>

<span data-ttu-id="bd2dc-322">Překlad identity (to znamená, že určení, že entita má stejný typ a ID jako dříve zjištěná entita) přidává další výkon a režii paměti.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-322">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="bd2dc-323">Obvykle se spustí čítač, aby se v prvním místě nepoužily žádné dotazy na sledování.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-323">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="bd2dc-324">I když může být v některých případech užitečný překlad identity, není potřeba, pokud se entity mají serializovat a odeslat klientovi, což je běžné pro žádné dotazy pro sledování.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-324">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="bd2dc-325">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-325">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-326">Pokud je vyžadováno rozlišení identity, použijte dotaz sledování.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-326">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="bd2dc-327">~~Provádění dotazu se protokoluje na úrovni ladění~~ . Vrátit</span><span class="sxs-lookup"><span data-stu-id="bd2dc-327">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="bd2dc-328">Sledování problému #14523</span><span class="sxs-lookup"><span data-stu-id="bd2dc-328">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="bd2dc-329">Tuto změnu jsme vrátili, protože nová konfigurace v EF Core 3,0 umožňuje, aby byla úroveň protokolu pro libovolnou událost specifikována aplikací.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-329">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="bd2dc-330">Chcete-li například přepnout protokolování SQL na `Debug`, explicitně nakonfigurujte úroveň v `OnConfiguring` nebo `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="bd2dc-330">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="bd2dc-331">Dočasné hodnoty klíčů už nejsou nastavené na instance entit.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-331">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="bd2dc-332">Sledování problému #12378</span><span class="sxs-lookup"><span data-stu-id="bd2dc-332">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="bd2dc-333">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-333">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-334">Před EF Core 3,0 byly přiřazeny dočasné hodnoty ke všem klíčovým vlastnostem, které by později měly skutečnou hodnotu vygenerovanou databází.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-334">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="bd2dc-335">Obvykle jsou tyto dočasné hodnoty velkými zápornými čísly.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-335">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="bd2dc-336">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-336">**New behavior**</span></span>

<span data-ttu-id="bd2dc-337">Počínaje 3,0 EF Core ukládá hodnotu dočasného klíče jako součást informací o sledování entity a ponechá vlastnost klíče beze změny.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-337">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="bd2dc-338">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-338">**Why**</span></span>

<span data-ttu-id="bd2dc-339">Tato změna byla provedena, aby se předešlo omylům hodnotám klíčů v případě, že entita, která byla dříve sledována instancí `DbContext`, je přesunuta do jiné instance `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-339">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="bd2dc-340">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-340">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-341">Aplikace, které přiřazují hodnoty primárního klíče k cizím klíčům k vytvoření přidružení mezi entitami, můžou záviset na starém chování, pokud jsou primární klíče generované úložištěm a patří do entit ve stavu `Added`.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-341">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="bd2dc-342">K tomu je možné se vyhnout:</span><span class="sxs-lookup"><span data-stu-id="bd2dc-342">This can be avoided by:</span></span>
* <span data-ttu-id="bd2dc-343">Nepoužívejte klíče generované úložištěm.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-343">Not using store-generated keys.</span></span>
* <span data-ttu-id="bd2dc-344">Nastavení vlastností navigace pro vytváření relací místo nastavení hodnot cizích klíčů.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-344">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="bd2dc-345">Získá aktuální dočasné hodnoty klíče z informací o sledování entity.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-345">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="bd2dc-346">Například `context.Entry(blog).Property(e => e.Id).CurrentValue` vrátí dočasnou hodnotu, i když `blog.Id` sama nebyla nastavena.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-346">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="bd2dc-347">DetectChanges respektuje hodnoty klíčů generované úložištěm.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-347">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="bd2dc-348">Sledování problému #14616</span><span class="sxs-lookup"><span data-stu-id="bd2dc-348">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="bd2dc-349">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-349">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-350">Před EF Core 3,0 se Nesledovaná entita, kterou najde `DetectChanges`, sledovala ve stavu `Added` a vložila se jako nový řádek při volání `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-350">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="bd2dc-351">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-351">**New behavior**</span></span>

<span data-ttu-id="bd2dc-352">Od EF Core 3,0 platí, že pokud entita používá vygenerované hodnoty klíčů a je nastavená nějaká hodnota klíče, bude se entita sledovat ve stavu `Modified`.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-352">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="bd2dc-353">To znamená, že se předpokládá, že řádek pro entitu existuje a že se bude aktualizovat při volání `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-353">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="bd2dc-354">Pokud hodnota klíče není nastavená, nebo pokud typ entity nepoužívá vygenerované klíče, bude nová entita dál sledována jako `Added` jako v předchozích verzích.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-354">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="bd2dc-355">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-355">**Why**</span></span>

<span data-ttu-id="bd2dc-356">Tato změna byla provedena tak, aby byla pro práci s nepřipojenými grafy entit při použití klíčů generovaných úložištěm snazší.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-356">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="bd2dc-357">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-357">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-358">Tato změna může přerušit aplikaci, pokud je typ entity nakonfigurovaný na používání vygenerovaných klíčů, ale hodnoty klíčů jsou explicitně nastavené pro nové instance.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-358">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="bd2dc-359">Opravou je explicitně nakonfigurovat klíčové vlastnosti tak, aby nepoužívaly vygenerované hodnoty.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-359">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="bd2dc-360">Například s rozhraním API Fluent:</span><span class="sxs-lookup"><span data-stu-id="bd2dc-360">For example, with the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="bd2dc-361">Nebo s poznámkami k datům:</span><span class="sxs-lookup"><span data-stu-id="bd2dc-361">Or with data annotations:</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="bd2dc-362">Odstranění kaskádových operací se teď ve výchozím nastavení provádí hned.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-362">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="bd2dc-363">Sledování problému #10114</span><span class="sxs-lookup"><span data-stu-id="bd2dc-363">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="bd2dc-364">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-364">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-365">Před 3,0 EF Core aplikovány kaskádové akce (odstraňování závislých entit, když se odstraní požadovaný objekt zabezpečení nebo když je vztah k požadovanému objektu zabezpečení vážně) nevznikl, dokud není voláno SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-365">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="bd2dc-366">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-366">**New behavior**</span></span>

<span data-ttu-id="bd2dc-367">Od 3,0 EF Core aplikuje kaskádové akce hned po zjištění spouštěcí podmínky.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-367">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="bd2dc-368">Například volání `context.Remove()` k odstranění objektu zabezpečení bude mít za následek, že jsou všechny sledované související závislé položky nastaveny také pro `Deleted` hned.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-368">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="bd2dc-369">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-369">**Why**</span></span>

<span data-ttu-id="bd2dc-370">Tato změna byla provedena za účelem zlepšení prostředí pro scénáře vytváření datových vazeb a auditování, kde je důležité pochopit, které entity budou odstraněny _před_ zavoláním `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-370">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="bd2dc-371">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-371">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-372">Předchozí chování lze obnovit pomocí nastavení na `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-372">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="bd2dc-373">Příklad:</span><span class="sxs-lookup"><span data-stu-id="bd2dc-373">For example:</span></span>

```csharp
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="eager-loading-single-query"></a>
### <a name="eager-loading-of-related-entities-now-happens-in-a-single-query"></a><span data-ttu-id="bd2dc-374">Eager načítání souvisejících entit se teď děje v jednom dotazu.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-374">Eager loading of related entities now happens in a single query</span></span>

[<span data-ttu-id="bd2dc-375">Sledování problému #18022</span><span class="sxs-lookup"><span data-stu-id="bd2dc-375">Tracking issue #18022</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/18022)

<span data-ttu-id="bd2dc-376">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-376">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-377">Před 3,0 eagerly načítání navigace kolekce prostřednictvím operátorů `Include` způsobilo, že se v relační databázi generují vícenásobné dotazy, jednu pro každý typ související entity.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-377">Before 3.0, eagerly loading collection navigations via `Include` operators caused multiple queries to be generated on relational database, one for each related entity type.</span></span>

<span data-ttu-id="bd2dc-378">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-378">**New behavior**</span></span>

<span data-ttu-id="bd2dc-379">Počínaje 3,0 EF Core generuje jediný dotaz s spojeními relačních databází.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-379">Starting with 3.0, EF Core generates a single query with JOINs on relational databases.</span></span>

<span data-ttu-id="bd2dc-380">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-380">**Why**</span></span>

<span data-ttu-id="bd2dc-381">Vydání více dotazů pro implementaci jednoho dotazu LINQ způsobilo velký počet problémů, včetně negativního výkonu, protože bylo nutné použít více databázových převodů, a problémy s integritou dat v případě, že každý dotaz může sledovat jiný stav databáze.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-381">Issuing multiple queries to implement a single LINQ query caused numerous issues, including negative performance as multiple database roundtrips were necessary, and data coherency issues as each query could observe a different state of the database.</span></span>

<span data-ttu-id="bd2dc-382">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-382">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-383">I když se jedná o zásadní změnu, může to mít výrazný vliv na výkon aplikace, když jeden dotaz obsahuje velký počet `Include`ch operátorů v navigaci kolekcí.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-383">While technically this is not a breaking change, it could have a considerable effect on application performance when a single query contains a large number of `Include` operator on collection navigations.</span></span> <span data-ttu-id="bd2dc-384">[Podívejte se na tento komentář](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) , kde najdete další informace a rychlé psaní dotazů.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-384">[See this comment](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) for more information and for rewriting queries in a more efficient way.</span></span>

**

<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="bd2dc-385">DeleteBehavior. restrict má sémantiku čištění.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-385">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="bd2dc-386">Sledování problému #12661</span><span class="sxs-lookup"><span data-stu-id="bd2dc-386">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="bd2dc-387">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-387">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-388">Před 3,0 `DeleteBehavior.Restrict` vytvořili cizí klíče v databázi se sémantikou `Restrict`, ale zároveň jsme změnili interní opravu nezřetelným způsobem.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-388">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="bd2dc-389">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-389">**New behavior**</span></span>

<span data-ttu-id="bd2dc-390">Počínaje 3,0 `DeleteBehavior.Restrict` zajišťuje, aby se cizí klíče vytvořily s `Restrict` sémantikou – tedy bez kaskády. porušení omezení throw – bez vlivu na interní opravu EF</span><span class="sxs-lookup"><span data-stu-id="bd2dc-390">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="bd2dc-391">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-391">**Why**</span></span>

<span data-ttu-id="bd2dc-392">Tato změna byla provedena pro zlepšení prostředí pro použití `DeleteBehavior` intuitivním způsobem bez neočekávaných vedlejších účinků.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-392">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="bd2dc-393">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-393">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-394">Předchozí chování lze obnovit pomocí `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-394">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="bd2dc-395">Typy dotazů jsou konsolidovány s typy entit</span><span class="sxs-lookup"><span data-stu-id="bd2dc-395">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="bd2dc-396">Sledování problému #14194</span><span class="sxs-lookup"><span data-stu-id="bd2dc-396">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="bd2dc-397">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-397">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-398">Před EF Core 3,0 byly [typy dotazů](xref:core/modeling/keyless-entity-types) prostředkem pro dotazování na data, která nedefinují primární klíč strukturovaným způsobem.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-398">Before EF Core 3.0, [query types](xref:core/modeling/keyless-entity-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="bd2dc-399">To znamená, že typ dotazu byl použit pro mapování typů entit bez klíčů (pravděpodobnější z pohledu, ale také z tabulky), zatímco byl použit regulární typ entity, když byl klíč k dispozici (pravděpodobnější z tabulky, ale případně z pohledu).</span><span class="sxs-lookup"><span data-stu-id="bd2dc-399">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="bd2dc-400">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-400">**New behavior**</span></span>

<span data-ttu-id="bd2dc-401">Typ dotazu se teď bude jednat jenom o typ entity bez primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-401">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="bd2dc-402">Typy entit bez klíčů mají stejné funkce jako typy dotazů v předchozích verzích.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-402">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="bd2dc-403">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-403">**Why**</span></span>

<span data-ttu-id="bd2dc-404">Tato změna byla provedena proto, aby se snížila nejasnost v souvislosti s typy dotazů.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-404">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="bd2dc-405">Konkrétně jsou bez klíčů typy entit a jsou ze své podstaty jen pro čtení, ale neměly by být použity pouze proto, že typ entity musí být jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-405">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="bd2dc-406">Podobně jsou často mapovány na zobrazení, ale to je pouze v případě, že zobrazení často nedefinují klíče.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-406">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="bd2dc-407">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-407">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-408">Následující části rozhraní API jsou teď zastaralé:</span><span class="sxs-lookup"><span data-stu-id="bd2dc-408">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="bd2dc-409">**`ModelBuilder.Query<>()`** – místo toho je nutné volat `ModelBuilder.Entity<>().HasNoKey()` pro označení typu entity, protože neobsahují žádné klíče.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-409">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="bd2dc-410">Tato služba by se ještě nenakonfigurovala pomocí konvence, aby nedocházelo k chybám konfigurace, když je primární klíč očekávaný, ale neodpovídá konvenci.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-410">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="bd2dc-411">**`DbQuery<>`** – místo toho by měla být použita `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-411">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="bd2dc-412">**`DbContext.Query<>()`** – místo toho by měla být použita `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-412">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="bd2dc-413">Změnilo se konfigurační rozhraní API pro vztahy vlastněných typů.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-413">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="bd2dc-414">[Sledování problému #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[sledování problému #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[sledování problému #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="bd2dc-414">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="bd2dc-415">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-415">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-416">Před EF Core 3,0 byla konfigurace vztahu vlastníka provedena přímo po volání `OwnsOne` nebo `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-416">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="bd2dc-417">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-417">**New behavior**</span></span>

<span data-ttu-id="bd2dc-418">Od EF Core 3,0 teď existuje Fluent API pro konfiguraci navigační vlastnosti pro vlastníka pomocí `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-418">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="bd2dc-419">Příklad:</span><span class="sxs-lookup"><span data-stu-id="bd2dc-419">For example:</span></span>

```csharp
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="bd2dc-420">Konfigurace vztahující se k vztahu mezi vlastníkem a vlastníkem by nyní měla být zřetězena po `WithOwner()` podobným způsobem, jakým jsou konfigurovány jiné vztahy.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-420">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="bd2dc-421">I když se konfigurace samotného vlastního typu bude i nadále zřetězit po `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-421">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="bd2dc-422">Příklad:</span><span class="sxs-lookup"><span data-stu-id="bd2dc-422">For example:</span></span>

```csharp
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

<span data-ttu-id="bd2dc-423">Kromě toho, že volání `Entity()`, `HasOne()`nebo `Set()` s cílem vlastněného typu teď vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-423">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="bd2dc-424">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-424">**Why**</span></span>

<span data-ttu-id="bd2dc-425">Tato změna byla provedena za účelem vytvoření čisticího oddělení mezi konfigurací samotného typu a _vztahu k_ typu, který je vlastníkem.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-425">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="bd2dc-426">Tím se zase odeberou nejednoznačnosti a nejasnosti u metod, jako je `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-426">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="bd2dc-427">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-427">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-428">Změňte konfiguraci vztahů vlastněných typů tak, aby používala novou plochu rozhraní API, jak je znázorněno v předchozím příkladu.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-428">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="bd2dc-429">Závislé entity, které sdílí tabulku s objektem zabezpečení, jsou teď volitelné.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-429">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="bd2dc-430">Sledování problému #9005</span><span class="sxs-lookup"><span data-stu-id="bd2dc-430">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="bd2dc-431">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-431">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-432">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="bd2dc-432">Consider the following model:</span></span>
```csharp
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
<span data-ttu-id="bd2dc-433">Před EF Core 3,0 platí, že pokud `OrderDetails` vlastní `Order` nebo explicitně namapované na stejnou tabulku, při přidávání nového `Order`se vždycky vyžadovala instance `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-433">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="bd2dc-434">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-434">**New behavior**</span></span>

<span data-ttu-id="bd2dc-435">Počínaje 3,0 EF Core umožňuje přidat `Order` bez `OrderDetails` a provede mapování všech `OrderDetails`ch vlastností s výjimkou primárního klíče na sloupce s možnou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-435">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="bd2dc-436">Při dotazování EF Core sady `OrderDetails` na `null`, pokud některá z jejích požadovaných vlastností nemá hodnotu, nebo pokud se kromě primárního klíče nevyžadují žádné vlastnosti, a `null`.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-436">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="bd2dc-437">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-437">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-438">Pokud má váš model sdílení tabulky závislé na všech volitelných sloupcích, ale navigace ukazující na něj se neočekává `null` pak by aplikace měla být upravena tak, aby zpracovávala případy, kdy je navigace `null`.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-438">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="bd2dc-439">Pokud to není možné, měla by být do typu entity přidána požadovaná vlastnost, nebo alespoň jedna vlastnost musí mít přiřazenou jinou ne`null` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-439">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="bd2dc-440">Všechny entity sdílející tabulku se sloupcem souběžného tokenu musí být namapovány na vlastnost.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-440">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="bd2dc-441">Sledování problému #14154</span><span class="sxs-lookup"><span data-stu-id="bd2dc-441">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="bd2dc-442">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-442">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-443">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="bd2dc-443">Consider the following model:</span></span>
```csharp
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
<span data-ttu-id="bd2dc-444">Před EF Core 3,0 platí, že pokud `OrderDetails` vlastní `Order` nebo explicitně namapované na stejnou tabulku, aktualizace pouze `OrderDetails` nebude aktualizovat `Version` hodnotu v klientovi a další aktualizace nebude úspěšná.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-444">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="bd2dc-445">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-445">**New behavior**</span></span>

<span data-ttu-id="bd2dc-446">Počínaje 3,0 EF Core rozšíří novou hodnotu `Version` na `Order`, pokud vlastní `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-446">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="bd2dc-447">V opačném případě je při ověřování modelu vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-447">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="bd2dc-448">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-448">**Why**</span></span>

<span data-ttu-id="bd2dc-449">Tato změna byla provedena, aby nedocházelo k zastaralé hodnotě tokenu souběžnosti, pokud je aktualizována pouze jedna z entit mapovaných na stejnou tabulku.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-449">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="bd2dc-450">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-450">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-451">Všechny entity, které sdílejí tabulku, musí obsahovat vlastnost, která je namapovaná na sloupec tokenu souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-451">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="bd2dc-452">Je možné, že ho vytvoříte ve stínovém stavu:</span><span class="sxs-lookup"><span data-stu-id="bd2dc-452">It's possible the create one in shadow-state:</span></span>
```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="owned-query"></a>

### <a name="owned-entities-cannot-be-queried-without-the-owner-using-a-tracking-query"></a><span data-ttu-id="bd2dc-453">Na vlastněné entity se nedá dotazovat bez vlastníka pomocí sledovacího dotazu.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-453">Owned entities cannot be queried without the owner using a tracking query</span></span>

[<span data-ttu-id="bd2dc-454">Sledování problému #18876</span><span class="sxs-lookup"><span data-stu-id="bd2dc-454">Tracking Issue #18876</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/18876)

<span data-ttu-id="bd2dc-455">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-455">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-456">Před EF Core 3,0 se jako jakákoli jiná navigace mohla zadat dotaz na vlastněné entity.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-456">Before EF Core 3.0, the owned entities could be queried as any other navigation.</span></span>

```csharp
context.People.Select(p => p.Address);
```

<span data-ttu-id="bd2dc-457">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-457">**New behavior**</span></span>

<span data-ttu-id="bd2dc-458">Počínaje 3,0 EF Core vyvolá výjimku, pokud dotaz sledování provede vlastní entitu bez vlastníka.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-458">Starting with 3.0, EF Core will throw if a tracking query projects an owned entity without the owner.</span></span>

<span data-ttu-id="bd2dc-459">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-459">**Why**</span></span>

<span data-ttu-id="bd2dc-460">Vlastněné entity nemůžou manipulovat bez vlastníka, takže v převážné většině případů se na ně dotazuje tímto způsobem je chyba.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-460">Owned entities cannot be manipulated without the owner, so in the vast majority of cases querying them in this way is an error.</span></span>

<span data-ttu-id="bd2dc-461">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-461">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-462">Pokud by měla být vlastní entita sledována tak, aby se změnila jakýmkoli způsobem, měl by být vlastník zahrnut v dotazu.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-462">If the owned entity should be tracked to be modified in any way later then the owner should be included in the query.</span></span>

<span data-ttu-id="bd2dc-463">V opačném případě přidejte `AsNoTracking()` volání:</span><span class="sxs-lookup"><span data-stu-id="bd2dc-463">Otherwise add an `AsNoTracking()` call:</span></span>

```csharp
context.People.Select(p => p.Address).AsNoTracking();
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="bd2dc-464">Zděděné vlastnosti z nemapovaných typů jsou nyní namapovány na jeden sloupec pro všechny odvozené typy.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-464">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="bd2dc-465">Sledování problému #13998</span><span class="sxs-lookup"><span data-stu-id="bd2dc-465">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="bd2dc-466">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-466">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-467">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="bd2dc-467">Consider the following model:</span></span>
```csharp
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

<span data-ttu-id="bd2dc-468">Před EF Core 3,0 bude vlastnost `ShippingAddress` namapována na samostatné sloupce pro `BulkOrder` a `Order` ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-468">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="bd2dc-469">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-469">**New behavior**</span></span>

<span data-ttu-id="bd2dc-470">Počínaje 3,0 EF Core pro `ShippingAddress`vytvoří pouze jeden sloupec.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-470">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="bd2dc-471">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-471">**Why**</span></span>

<span data-ttu-id="bd2dc-472">Starý behavoir nebyl očekáván.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-472">The old behavoir was unexpected.</span></span>

<span data-ttu-id="bd2dc-473">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-473">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-474">Vlastnost může být stále explicitně namapována na samostatný sloupec odvozených typů:</span><span class="sxs-lookup"><span data-stu-id="bd2dc-474">The property can still be explicitly mapped to separate column on the derived types:</span></span>

```csharp
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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="bd2dc-475">Konvence vlastností cizího klíče už neodpovídá stejnému názvu jako vlastnost Principal.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-475">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="bd2dc-476">Sledování problému #13274</span><span class="sxs-lookup"><span data-stu-id="bd2dc-476">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="bd2dc-477">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-477">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-478">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="bd2dc-478">Consider the following model:</span></span>
```csharp
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
<span data-ttu-id="bd2dc-479">Před EF Core 3,0 se pro cizí klíč podle konvence použije vlastnost `CustomerId`.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-479">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="bd2dc-480">Pokud je však `Order` vlastněný typ, pak by to mělo také `CustomerId` primární klíč a to není obvykle očekávané.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-480">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="bd2dc-481">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-481">**New behavior**</span></span>

<span data-ttu-id="bd2dc-482">Počínaje 3,0 se EF Core nepokouší použít vlastnosti pro cizí klíče podle konvence, pokud mají stejný název jako vlastnost Principal.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-482">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="bd2dc-483">Název objektu zabezpečení zřetězený s názvem vlastnosti objektu zabezpečení a název navigace zřetězený s vzory názvů hlavních vlastností se pořád shodují.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-483">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="bd2dc-484">Příklad:</span><span class="sxs-lookup"><span data-stu-id="bd2dc-484">For example:</span></span>

```csharp
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

```csharp
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

<span data-ttu-id="bd2dc-485">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-485">**Why**</span></span>

<span data-ttu-id="bd2dc-486">Tato změna byla provedena, aby nedocházelo k chybnému definování vlastností primárního klíče u vlastněných typů.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-486">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="bd2dc-487">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-487">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-488">Pokud by vlastnost měla být cizí klíč, a proto je součástí primárního klíče, pak ji explicitně nakonfigurujte jako takovou.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-488">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="bd2dc-489">Připojení k databázi je teď zavřené, pokud už nepoužíváte dřív, než se dokončí jeho objekt TransactionScope.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-489">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="bd2dc-490">Sledování problému #14218</span><span class="sxs-lookup"><span data-stu-id="bd2dc-490">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="bd2dc-491">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-491">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-492">Pokud se během EF Core 3,0 v rámci kontextu otevře připojení v `TransactionScope`, zůstane připojení otevřené, zatímco je aktivní aktuální `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-492">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

```csharp
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

<span data-ttu-id="bd2dc-493">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-493">**New behavior**</span></span>

<span data-ttu-id="bd2dc-494">Od 3,0 EF Core ukončí připojení, jakmile ho dokončí jeho používání.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-494">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="bd2dc-495">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-495">**Why**</span></span>

<span data-ttu-id="bd2dc-496">Tato změna umožňuje použít více kontextů ve stejném `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-496">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="bd2dc-497">Nové chování se také shoduje s EF6.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-497">The new behavior also matches EF6.</span></span>

<span data-ttu-id="bd2dc-498">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-498">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-499">Pokud připojení potřebuje zůstat otevřeným explicitním voláním `OpenConnection()`, zajistíte, že EF Core nebude předčasně ukončen:</span><span class="sxs-lookup"><span data-stu-id="bd2dc-499">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

```csharp
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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="bd2dc-500">Každá vlastnost používá nezávislou generaci celočíselného klíče v paměti.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-500">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="bd2dc-501">Sledování problému #6872</span><span class="sxs-lookup"><span data-stu-id="bd2dc-501">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="bd2dc-502">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-502">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-503">Před EF Core 3,0 byl pro všechny vlastnosti celého čísla klíče v paměti použit jeden generátor sdílených hodnot.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-503">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="bd2dc-504">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-504">**New behavior**</span></span>

<span data-ttu-id="bd2dc-505">Počínaje EF Core 3,0 každé klíčové vlastnosti celého čísla získá vlastní generátor hodnot při použití databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-505">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="bd2dc-506">Také pokud je databáze odstraněna, generování klíče se obnoví pro všechny tabulky.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-506">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="bd2dc-507">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-507">**Why**</span></span>

<span data-ttu-id="bd2dc-508">Tato změna byla provedená tak, aby se při vytváření klíčů v paměti lépe rovnala generování klíčů v paměti a vylepšila se možnost izolovat testy od sebe při použití databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-508">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="bd2dc-509">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-509">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-510">To může poškodit aplikaci, která se spoléhá na konkrétní hodnoty klíče v paměti, které se mají nastavit.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-510">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="bd2dc-511">Místo toho se nemusíte spoléhat na konkrétní hodnoty klíčů nebo aktualizovat tak, aby odpovídaly novému chování.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-511">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="bd2dc-512">Ve výchozím nastavení se používají pole pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-512">Backing fields are used by default</span></span>

[<span data-ttu-id="bd2dc-513">Sledování problému #12430</span><span class="sxs-lookup"><span data-stu-id="bd2dc-513">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="bd2dc-514">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-514">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-515">Před 3,0, a to i v případě, že je známé pole zálohování pro vlastnost, EF Core ve výchozím nastavení přečetl a zapsat hodnotu vlastnosti pomocí metod getter a setter vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-515">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="bd2dc-516">Výjimkou je provedení dotazu, kde by se pole zálohování nastavilo přímo, pokud je známé.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-516">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="bd2dc-517">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-517">**New behavior**</span></span>

<span data-ttu-id="bd2dc-518">Počínaje EF Core 3,0 platí, že pokud je známé pole zálohování pro vlastnost, pak EF Core tuto vlastnost vždy přečte a zapíše pomocí pole pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-518">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="bd2dc-519">To může způsobit přerušení aplikace, pokud se aplikace spoléhá na další chování kódované v metodách getter nebo setter.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-519">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="bd2dc-520">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-520">**Why**</span></span>

<span data-ttu-id="bd2dc-521">Tato změna byla provedena EF Core proto, aby při provádění databázových operací, které obsahují entity, ve výchozím nastavení nechybně aktivovala obchodní logiku.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-521">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="bd2dc-522">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-522">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-523">Chování před 3,0 se dá obnovit pomocí konfigurace režimu přístupu vlastnosti na `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-523">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="bd2dc-524">Příklad:</span><span class="sxs-lookup"><span data-stu-id="bd2dc-524">For example:</span></span>

```csharp
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="bd2dc-525">Vyvolat, zda je nalezeno více kompatibilních zálohovaných polí</span><span class="sxs-lookup"><span data-stu-id="bd2dc-525">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="bd2dc-526">Sledování problému #12523</span><span class="sxs-lookup"><span data-stu-id="bd2dc-526">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="bd2dc-527">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-527">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-528">Pokud se v EF Core 3,0 shodovalo více polí s pravidly pro hledání zálohovacího pole vlastnosti, bude jedno pole zvoleno na základě pořadí priorit.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-528">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="bd2dc-529">To může způsobit, že se nesprávné pole použije v nejednoznačných případech.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-529">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="bd2dc-530">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-530">**New behavior**</span></span>

<span data-ttu-id="bd2dc-531">Počínaje EF Core 3,0 platí, že pokud je více polí spárováno se stejnou vlastností, je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-531">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="bd2dc-532">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-532">**Why**</span></span>

<span data-ttu-id="bd2dc-533">Tato změna byla provedena, aby nedocházelo k tichému použití jednoho pole v případě, že může být pouze jedna z nich správná.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-533">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="bd2dc-534">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-534">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-535">Vlastnosti s nejednoznačnými zálohovacími poli musí obsahovat pole, které se má explicitně použít.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-535">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="bd2dc-536">Například pomocí rozhraní Fluent API:</span><span class="sxs-lookup"><span data-stu-id="bd2dc-536">For example, using the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="bd2dc-537">Názvy vlastností pouze pro pole se musí shodovat s názvem pole.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-537">Field-only property names should match the field name</span></span>

<span data-ttu-id="bd2dc-538">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-538">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-539">Před EF Core 3,0 může být vlastnost určena hodnotou řetězce a v případě, že v typu .NET nebyla nalezena žádná vlastnost s tímto názvem, EF Core by se pokusila ji porovnat s polem pomocí pravidel konvence.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-539">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the .NET type then EF Core would try to match it to a field using convention rules.</span></span>

```csharp
private class Blog
{
    private int _id;
    public string Name { get; set; }
}
```

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("Id");
```

<span data-ttu-id="bd2dc-540">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-540">**New behavior**</span></span>

<span data-ttu-id="bd2dc-541">Počínaje EF Core 3,0 se vlastnost pouze pole musí přesně shodovat s názvem pole.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-541">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="bd2dc-542">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-542">**Why**</span></span>

<span data-ttu-id="bd2dc-543">Tato změna byla provedena, aby se nepoužívalo stejné pole pro dvě vlastnosti s názvem podobně, ale také pravidla pro porovnání vlastností pouze pro pole, která jsou shodná s vlastnostmi mapovanými na vlastnosti CLR.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-543">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="bd2dc-544">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-544">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-545">Vlastnosti pouze polí musí být pojmenovány stejně jako pole, na které jsou namapována.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-545">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="bd2dc-546">V budoucí verzi EF Core po 3,0 plánujeme znovu povolit explicitní konfiguraci názvu pole, který se liší od názvu vlastnosti (viz téma věnované problému [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span><span class="sxs-lookup"><span data-stu-id="bd2dc-546">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="bd2dc-547">AddDbContext/AddDbContextPool už nevolá AddLogging a AddMemoryCache.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-547">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="bd2dc-548">Sledování problému #14756</span><span class="sxs-lookup"><span data-stu-id="bd2dc-548">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="bd2dc-549">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-549">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-550">Před EF Core 3,0 by volání `AddDbContext` nebo `AddDbContextPool` také registrovalo protokolování a služby ukládání do mezipaměti v paměti s DI prostřednictvím volání [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) a [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="bd2dc-550">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with DI through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="bd2dc-551">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-551">**New behavior**</span></span>

<span data-ttu-id="bd2dc-552">Počínaje EF Core 3,0 se `AddDbContext` a `AddDbContextPool` nebudou nadále registrovat tyto služby se vkládáním závislostí (DI).</span><span class="sxs-lookup"><span data-stu-id="bd2dc-552">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="bd2dc-553">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-553">**Why**</span></span>

<span data-ttu-id="bd2dc-554">EF Core 3,0 nevyžaduje, aby se tyto služby nacházejí v kontejneru aplikace DI.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-554">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="bd2dc-555">Pokud je však v kontejneru aplikace `ILoggerFactory` zaregistrován, bude nadále používána EF Core.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-555">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="bd2dc-556">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-556">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-557">Pokud vaše aplikace potřebuje tyto služby, zaregistrujte je explicitně pomocí kontejneru DI pomocí [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) nebo [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="bd2dc-557">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

### <a name="addentityframework-adds-imemorycache-with-a-size-limit"></a><span data-ttu-id="bd2dc-558">AddEntityFramework \* přidá IMemoryCache s omezením velikosti.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-558">AddEntityFramework\* adds IMemoryCache with a size limit</span></span>

[<span data-ttu-id="bd2dc-559">Sledování problému #12905</span><span class="sxs-lookup"><span data-stu-id="bd2dc-559">Tracking Issue #12905</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12905)

<span data-ttu-id="bd2dc-560">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-560">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-561">Před EF Core 3,0 by volání `AddEntityFramework*`ch metod také registrovalo služby ukládání do mezipaměti paměti s DI bez omezení velikosti.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-561">Before EF Core 3.0, calling `AddEntityFramework*` methods would also register memory caching services with DI without a size limit.</span></span>

<span data-ttu-id="bd2dc-562">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-562">**New behavior**</span></span>

<span data-ttu-id="bd2dc-563">Počínaje EF Core 3,0 `AddEntityFramework*` zaregistruje službu IMemoryCache s omezením velikosti.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-563">Starting with EF Core 3.0, `AddEntityFramework*` will register an IMemoryCache service with a size limit.</span></span> <span data-ttu-id="bd2dc-564">Pokud se jakékoli jiné služby přidané následně závisejí na IMemoryCache, můžou rychle dosáhnout výchozího limitu, který způsobuje výjimky nebo snížení výkonu.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-564">If any other services added afterwards depend on IMemoryCache they can quickly reach the default limit causing exceptions or degraded performance.</span></span>

<span data-ttu-id="bd2dc-565">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-565">**Why**</span></span>

<span data-ttu-id="bd2dc-566">Použití IMemoryCache bez omezení by mohlo vést k neřízenému využití paměti, pokud dojde k chybě v logice mezipaměti dotazů nebo když jsou dotazy generovány dynamicky.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-566">Using IMemoryCache without a limit could result in uncontrolled memory usage if there is a bug in query caching logic or the queries are generated dynamically.</span></span> <span data-ttu-id="bd2dc-567">Výchozí omezení snižuje potenciální útok DoS.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-567">Having a default limit mitigates a potential DoS attack.</span></span>

<span data-ttu-id="bd2dc-568">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-568">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-569">Ve většině případů volání `AddEntityFramework*` není nutné, pokud se volá také `AddDbContext` nebo `AddDbContextPool`.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-569">In most cases calling `AddEntityFramework*` is not necessary if `AddDbContext` or `AddDbContextPool` is called as well.</span></span> <span data-ttu-id="bd2dc-570">Z toho důvodu je nejlepší zmírnit odebrání `AddEntityFramework*` volání.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-570">Therefore, the best mitigation is to remove the `AddEntityFramework*` call.</span></span>

<span data-ttu-id="bd2dc-571">Pokud vaše aplikace potřebuje tyto služby, zaregistrujte implementaci IMemoryCache explicitně pomocí kontejneru DI předem pomocí [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="bd2dc-571">If your application needs these services, then register a IMemoryCache implementation explicitly with the DI container beforehand using [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="bd2dc-572">DbContext. entry teď provádí místní DetectChanges.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-572">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="bd2dc-573">Sledování problému #13552</span><span class="sxs-lookup"><span data-stu-id="bd2dc-573">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="bd2dc-574">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-574">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-575">Před EF Core 3,0 by volání `DbContext.Entry` způsobilo zjištění změn všech sledovaných entit.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-575">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="bd2dc-576">Tím je zajištěno, že stav zpřístupněný v `EntityEntry` byl aktuální.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-576">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="bd2dc-577">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-577">**New behavior**</span></span>

<span data-ttu-id="bd2dc-578">Počínaje EF Core 3,0 se volání `DbContext.Entry` nyní pokusí zjistit pouze změny v dané entitě a všechny sledované hlavní entity, které se k ní vztahují.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-578">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="bd2dc-579">To znamená, že změny jinde nemohly být zjištěny voláním této metody, což by mohlo mít vliv na stav aplikace.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-579">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="bd2dc-580">Všimněte si, že pokud je `ChangeTracker.AutoDetectChangesEnabled` nastaveno na `false` pak i toto místní zjišťování změn bude zakázáno.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-580">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="bd2dc-581">Jiné metody, které způsobují detekci změn – například `ChangeTracker.Entries` a `SaveChanges`, stále způsobují kompletní `DetectChanges` všech sledovaných entit.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-581">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="bd2dc-582">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-582">**Why**</span></span>

<span data-ttu-id="bd2dc-583">Tato změna byla provedena za účelem zlepšení výchozího výkonu použití `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-583">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="bd2dc-584">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-584">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-585">Před voláním `Entry` zajistěte, aby bylo zajištěno chování před 3,0m voláním `ChgangeTracker.DetectChanges()` explicitně.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-585">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="bd2dc-586">Klíče řetězce a pole bajtů nejsou ve výchozím nastavení generovány klientem.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-586">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="bd2dc-587">Sledování problému #14617</span><span class="sxs-lookup"><span data-stu-id="bd2dc-587">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="bd2dc-588">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-588">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-589">Před EF Core 3,0 lze použít `string` a `byte[]` vlastnosti klíče, aniž byste museli explicitně nastavit hodnotu, která není null.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-589">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="bd2dc-590">V takovém případě se hodnota klíče vygeneruje na klientovi jako identifikátor GUID serializovaný na bajty pro `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-590">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="bd2dc-591">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-591">**New behavior**</span></span>

<span data-ttu-id="bd2dc-592">Počínaje EF Core 3,0 bude vyvolána výjimka oznamující, že nebyla nastavena žádná hodnota klíče.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-592">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="bd2dc-593">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-593">**Why**</span></span>

<span data-ttu-id="bd2dc-594">Tato změna byla provedena, protože /`byte[]` hodnoty `string`generované klientem nejsou všeobecně užitečné a výchozí chování způsobilo, že je obtížné vygenerovat hodnoty klíčů běžným způsobem.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-594">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="bd2dc-595">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-595">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-596">Chování před 3,0 lze získat explicitním určením, že klíčové vlastnosti by měly používat generované hodnoty, pokud není nastavena žádná jiná hodnota, která není null.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-596">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="bd2dc-597">Například s rozhraním API Fluent:</span><span class="sxs-lookup"><span data-stu-id="bd2dc-597">For example, with the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="bd2dc-598">Nebo s poznámkami k datům:</span><span class="sxs-lookup"><span data-stu-id="bd2dc-598">Or with data annotations:</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="bd2dc-599">ILoggerFactory je teď služba s vymezeným oborem.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-599">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="bd2dc-600">Sledování problému #14698</span><span class="sxs-lookup"><span data-stu-id="bd2dc-600">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="bd2dc-601">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-601">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-602">Před EF Core 3,0 byl `ILoggerFactory` zaregistrován jako služba s jedním prvkem.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-602">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="bd2dc-603">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-603">**New behavior**</span></span>

<span data-ttu-id="bd2dc-604">Počínaje EF Core 3,0 se `ILoggerFactory` nyní zaregistroval jako vymezený obor.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-604">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="bd2dc-605">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-605">**Why**</span></span>

<span data-ttu-id="bd2dc-606">Tato změna byla provedena, aby bylo možné povolit přidružení protokolovacího nástroje k instanci `DbContext`, která umožňuje další funkce a odebírá některé případy patologického chování, jako je například rozbalení interních poskytovatelů služeb.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-606">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="bd2dc-607">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-607">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-608">Tato změna by neměla mít vliv na kód aplikace, pokud se neregistruje a nepoužívá vlastní služby pro EF Core interního poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-608">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="bd2dc-609">To není běžné.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-609">This isn't common.</span></span>
<span data-ttu-id="bd2dc-610">V těchto případech bude většina věcí pořád fungovat, ale jakákoliv služba typu Singleton, která byla v závislosti na `ILoggerFactory`, musí být změněna tak, aby získala `ILoggerFactory` jiným způsobem.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-610">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="bd2dc-611">Pokud narazíte na takové situace, zajistěte prosím problém na [EF Core sledování problémů GitHubu](https://github.com/aspnet/EntityFrameworkCore/issues) , abychom nás věděli, jak používáte `ILoggerFactory` tak, abychom mohli lépe porozumět tomu, jak toto řešení v budoucnu zrušit.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-611">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="bd2dc-612">Opožděné načítání proxy serverů už nepředpokládá navigační vlastnosti, jsou plně načtené.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-612">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="bd2dc-613">Sledování problému #12780</span><span class="sxs-lookup"><span data-stu-id="bd2dc-613">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="bd2dc-614">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-614">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-615">Před EF Core 3,0 byla po zrušení `DbContext` nijak nevěděla, zda byla daná vlastnost navigace u entity získané z tohoto kontextu plně načtena nebo nikoli.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-615">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="bd2dc-616">Proxy místo toho předpokládají, že je načtena odkazová navigace, pokud má hodnotu jinou než null a že je načtena navigace kolekce, pokud není prázdná.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-616">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="bd2dc-617">V těchto případech by byl pokus o opožděné načtení no-op.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-617">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="bd2dc-618">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-618">**New behavior**</span></span>

<span data-ttu-id="bd2dc-619">Počínaje EF Core 3,0 budou proxy servery sledovat, zda je načtena vlastnost navigace.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-619">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="bd2dc-620">To znamená, že při pokusu o přístup k navigační vlastnosti, která je načtena po vyřazení kontextu, bude vždy ta no-op, i když je načtená navigace prázdná nebo má hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-620">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="bd2dc-621">Naopak při pokusu o přístup k navigační vlastnosti, která není načtená, vyvolá výjimku, pokud je kontext vyřazený, i když je vlastnost navigace neprázdná kolekce.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-621">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="bd2dc-622">Pokud nastane tato situace, znamená to, že se kód aplikace pokouší použít opožděné načítání v neplatném čase a aplikace by se měla změnit, aby to nevedlo.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-622">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="bd2dc-623">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-623">**Why**</span></span>

<span data-ttu-id="bd2dc-624">Tato změna byla provedena, aby při pokusu o opožděné načtení na uvolněnou instanci `DbContext` bylo chování konzistentní a správné.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-624">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="bd2dc-625">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-625">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-626">Aktualizujte kód aplikace, aby se nepokoušel opožděné načtení s odstraněným kontextem, nebo nastavte tuto hodnotu jako No-op, jak je popsáno ve zprávě výjimky.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-626">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="bd2dc-627">Nadměrné vytváření interních zprostředkovatelů služeb je teď ve výchozím nastavení chyba.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-627">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="bd2dc-628">Sledování problému #10236</span><span class="sxs-lookup"><span data-stu-id="bd2dc-628">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="bd2dc-629">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-629">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-630">Před EF Core 3,0 se pro aplikaci, která vytváří patologický počet interních poskytovatelů služeb, zaprotokoluje upozornění.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-630">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="bd2dc-631">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-631">**New behavior**</span></span>

<span data-ttu-id="bd2dc-632">Počínaje EF Core 3,0 je toto upozornění nyní považováno za chybu a je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-632">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="bd2dc-633">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-633">**Why**</span></span>

<span data-ttu-id="bd2dc-634">Tato změna byla provedená tak, že se tento patologický případ výslovně zveřejňuje tak, aby se zlepšil kód aplikace.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-634">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="bd2dc-635">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-635">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-636">Nejvhodnější příčinou této chyby je pochopení hlavní příčiny a zastavení vytváření, takže mnoho interních poskytovatelů služeb.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-636">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="bd2dc-637">Chybu však lze převést zpět na varování (nebo ignorováno) prostřednictvím konfigurace na `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-637">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="bd2dc-638">Příklad:</span><span class="sxs-lookup"><span data-stu-id="bd2dc-638">For example:</span></span>

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="bd2dc-639">Nové chování pro HasOne/HasMany se volá s jedním řetězcem.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-639">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="bd2dc-640">Sledování problému #9171</span><span class="sxs-lookup"><span data-stu-id="bd2dc-640">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="bd2dc-641">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-641">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-642">Před EF Core 3,0 byl kód volání `HasOne` nebo `HasMany` s jedním řetězcem interpretován jako matoucí způsob.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-642">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="bd2dc-643">Příklad:</span><span class="sxs-lookup"><span data-stu-id="bd2dc-643">For example:</span></span>
```csharp
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="bd2dc-644">Kód vypadá jako v souvislosti s `Samurai` pro jiný typ entity pomocí navigační vlastnosti `Entrance`, která může být soukromá.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-644">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="bd2dc-645">Ve skutečnosti se tento kód pokusí vytvořit relaci k některému typu entity s názvem `Entrance` bez vlastnosti navigace.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-645">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="bd2dc-646">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-646">**New behavior**</span></span>

<span data-ttu-id="bd2dc-647">Počínaje EF Core 3,0 výše uvedený kód má nyní podobný vzhled jako předtím.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-647">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="bd2dc-648">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-648">**Why**</span></span>

<span data-ttu-id="bd2dc-649">Staré chování bylo velmi matoucí, zejména při čtení konfiguračního kódu a hledání chyb.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-649">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="bd2dc-650">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-650">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-651">Tím dojde pouze k přerušení aplikací, které jsou explicitně konfigurovány pomocí řetězců pro názvy typů, a bez explicitního určení vlastnosti navigace.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-651">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="bd2dc-652">To není běžné.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-652">This is not common.</span></span>
<span data-ttu-id="bd2dc-653">Předchozí chování se dá získat pomocí explicitního předání `null` názvu vlastnosti navigace.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-653">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="bd2dc-654">Příklad:</span><span class="sxs-lookup"><span data-stu-id="bd2dc-654">For example:</span></span>

```csharp
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="bd2dc-655">Návratový typ pro několik asynchronních metod byl změněn z úlohy na ValueTask</span><span class="sxs-lookup"><span data-stu-id="bd2dc-655">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="bd2dc-656">Sledování problému #15184</span><span class="sxs-lookup"><span data-stu-id="bd2dc-656">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="bd2dc-657">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-657">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-658">Následující asynchronní metody dříve vrátily `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="bd2dc-658">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="bd2dc-659">`ValueGenerator.NextValueAsync()` (a odvozování tříd)</span><span class="sxs-lookup"><span data-stu-id="bd2dc-659">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="bd2dc-660">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-660">**New behavior**</span></span>

<span data-ttu-id="bd2dc-661">Výše uvedené metody nyní vrací `ValueTask<T>` přes stejný `T` jako předtím.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-661">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="bd2dc-662">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-662">**Why**</span></span>

<span data-ttu-id="bd2dc-663">Tato změna snižuje počet přidělení haldy, které vznikly při vyvolání těchto metod, což zlepšuje obecný výkon.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-663">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="bd2dc-664">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-664">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-665">Aplikace jednoduše čekají na rozhraní API, které je třeba znovu zkompilovat – nejsou nutné žádné změny zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-665">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="bd2dc-666">Složitější využití (například předání vrácených `Task` do `Task.WhenAny()`) obvykle vyžaduje, aby vrácený `ValueTask<T>` byl převeden na `Task<T>` voláním `AsTask()`.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-666">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="bd2dc-667">Všimněte si, že se tím sníží omezení přidělení, které tato změna přináší.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-667">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="bd2dc-668">Relační: anotace TypeMapping je nyní pouze TypeMapping</span><span class="sxs-lookup"><span data-stu-id="bd2dc-668">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="bd2dc-669">Sledování problému #9913</span><span class="sxs-lookup"><span data-stu-id="bd2dc-669">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="bd2dc-670">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-670">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-671">Název poznámky pro poznámky k mapování typů byl "relační: TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="bd2dc-671">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="bd2dc-672">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-672">**New behavior**</span></span>

<span data-ttu-id="bd2dc-673">Název poznámky pro mapování typů je nyní "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="bd2dc-673">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="bd2dc-674">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-674">**Why**</span></span>

<span data-ttu-id="bd2dc-675">Mapování typů se nyní používají pro více než stejné poskytovatele relačních databází.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-675">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="bd2dc-676">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-676">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-677">Tím dojde pouze k přerušení aplikací, které přistupují k mapování typu přímo jako anotaci, což není běžné.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-677">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="bd2dc-678">Nejvhodnější akcí pro opravu je použití prostoru rozhraní API pro přístup k mapování typů namísto použití anotace přímo.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-678">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="bd2dc-679">ToTable na odvozeném typu vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-679">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="bd2dc-680">Sledování problému #11811</span><span class="sxs-lookup"><span data-stu-id="bd2dc-680">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="bd2dc-681">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-681">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-682">Před EF Core 3,0 by byl `ToTable()` volaný pro odvozený typ ignorován, protože pouze dědičnost dědičnosti mapování je typu TPH, který není platný.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-682">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="bd2dc-683">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-683">**New behavior**</span></span>

<span data-ttu-id="bd2dc-684">Počínaje EF Core 3,0 a při přípravě na přidání podpory TPT a TPC v novější verzi nyní `ToTable()` volána na odvozeném typu vyvolá výjimku, aby nedošlo k neočekávané změně mapování v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-684">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="bd2dc-685">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-685">**Why**</span></span>

<span data-ttu-id="bd2dc-686">V současné době není platný pro mapování odvozeného typu na jinou tabulku.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-686">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="bd2dc-687">Tato změna zabrání v budoucnosti v budoucnu, pokud se to stalo platným.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-687">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="bd2dc-688">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-688">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-689">Odeberte všechny pokusy o mapování odvozených typů na jiné tabulky.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-689">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="bd2dc-690">ForSqlServerHasIndex nahrazeno HasIndex</span><span class="sxs-lookup"><span data-stu-id="bd2dc-690">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="bd2dc-691">Sledování problému #12366</span><span class="sxs-lookup"><span data-stu-id="bd2dc-691">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="bd2dc-692">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-692">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-693">Před EF Core 3,0 `ForSqlServerHasIndex().ForSqlServerInclude()` poskytnout způsob, jak nakonfigurovat sloupce používané `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-693">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="bd2dc-694">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-694">**New behavior**</span></span>

<span data-ttu-id="bd2dc-695">Počínaje EF Core 3,0 se teď na relační úrovni podporuje použití `Include` na indexu.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-695">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="bd2dc-696">Použijte `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-696">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="bd2dc-697">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-697">**Why**</span></span>

<span data-ttu-id="bd2dc-698">Tato změna byla provedena za účelem konsolidace rozhraní API pro indexy s `Include` na jednom místě pro všechny poskytovatele databáze.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-698">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="bd2dc-699">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-699">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-700">Použijte nové rozhraní API, jak vidíte výše.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-700">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="bd2dc-701">Změny rozhraní API pro metadata</span><span class="sxs-lookup"><span data-stu-id="bd2dc-701">Metadata API changes</span></span>

[<span data-ttu-id="bd2dc-702">Sledování problému #214</span><span class="sxs-lookup"><span data-stu-id="bd2dc-702">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="bd2dc-703">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-703">**New behavior**</span></span>

<span data-ttu-id="bd2dc-704">Následující vlastnosti byly převedeny na rozšiřující metody:</span><span class="sxs-lookup"><span data-stu-id="bd2dc-704">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="bd2dc-705">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-705">**Why**</span></span>

<span data-ttu-id="bd2dc-706">Tato změna zjednodušuje implementaci výše uvedených rozhraní.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-706">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="bd2dc-707">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-707">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-708">Použijte nové metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-708">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="bd2dc-709">Změny rozhraní API pro konkrétního zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="bd2dc-709">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="bd2dc-710">Sledování problému #214</span><span class="sxs-lookup"><span data-stu-id="bd2dc-710">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="bd2dc-711">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-711">**New behavior**</span></span>

<span data-ttu-id="bd2dc-712">Metody rozšíření specifické pro poskytovatele budou shrnuty:</span><span class="sxs-lookup"><span data-stu-id="bd2dc-712">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="bd2dc-713">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-713">**Why**</span></span>

<span data-ttu-id="bd2dc-714">Tato změna zjednodušuje implementaci výše uvedených rozšiřujících metod.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-714">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="bd2dc-715">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-715">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-716">Použijte nové metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-716">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="bd2dc-717">EF Core už neposílá direktivu pragma pro vynucení KOFK SQLite.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-717">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="bd2dc-718">Sledování problému #12151</span><span class="sxs-lookup"><span data-stu-id="bd2dc-718">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="bd2dc-719">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-719">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-720">Před EF Core 3,0 by EF Core při otevření připojení k SQLite odeslal `PRAGMA foreign_keys = 1`.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-720">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="bd2dc-721">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-721">**New behavior**</span></span>

<span data-ttu-id="bd2dc-722">Počínaje EF Core 3,0 EF Core již neposílá `PRAGMA foreign_keys = 1` při otevření připojení k SQLite.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-722">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="bd2dc-723">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-723">**Why**</span></span>

<span data-ttu-id="bd2dc-724">Tato změna byla provedena, protože ve výchozím nastavení používá EF Core `SQLitePCLRaw.bundle_e_sqlite3`, což zase znamená, že je ve výchozím nastavení zapnuté vynucení CK a není nutné je explicitně povolit při každém otevření připojení.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-724">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="bd2dc-725">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-725">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-726">Cizí klíče jsou ve výchozím nastavení povolené v SQLitePCLRaw. bundle_e_sqlite3, která se ve výchozím nastavení používá pro EF Core.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-726">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="bd2dc-727">V ostatních případech je možné povolit cizí klíče zadáním `Foreign Keys=True` v připojovacím řetězci.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-727">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="bd2dc-728">Microsoft. EntityFrameworkCore. sqlite teď závisí na SQLitePCLRaw. bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="bd2dc-728">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="bd2dc-729">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-729">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-730">Před EF Core 3,0 EF Core použito `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-730">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="bd2dc-731">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-731">**New behavior**</span></span>

<span data-ttu-id="bd2dc-732">Počínaje EF Core 3,0 EF Core používá `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-732">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="bd2dc-733">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-733">**Why**</span></span>

<span data-ttu-id="bd2dc-734">Tato změna byla provedena tak, že verze SQLiteu použitá v iOS je konzistentní s jinými platformami.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-734">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="bd2dc-735">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-735">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-736">Pokud chcete použít nativní verzi SQLite v iOS, nakonfigurujte `Microsoft.Data.Sqlite` tak, aby používala jiný svazek `SQLitePCLRaw`.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-736">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="bd2dc-737">Hodnoty GUID se teď ukládají jako TEXT na SQLite.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-737">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="bd2dc-738">Sledování problému #15078</span><span class="sxs-lookup"><span data-stu-id="bd2dc-738">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="bd2dc-739">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-739">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-740">Hodnoty GUID byly dříve uloženy jako hodnoty objektů BLOB u SQLite.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-740">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="bd2dc-741">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-741">**New behavior**</span></span>

<span data-ttu-id="bd2dc-742">Hodnoty GUID jsou nyní uloženy jako TEXT.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-742">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="bd2dc-743">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-743">**Why**</span></span>

<span data-ttu-id="bd2dc-744">Binární formát identifikátorů GUID není standardizovaný.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-744">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="bd2dc-745">Uložení hodnot jako textu zajistí, že databáze bude lépe kompatibilní s jinými technologiemi.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-745">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="bd2dc-746">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-746">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-747">Existující databáze můžete migrovat do nového formátu tím, že spustíte příkaz SQL podobně jako následující.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-747">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="bd2dc-748">V EF Core můžete i nadále používat předchozí chování nakonfigurováním převaděče hodnot na těchto vlastnostech.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-748">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="bd2dc-749">Microsoft. data. sqlite zůstává schopný přečítat hodnoty GUID z objektů BLOB a textových sloupců. vzhledem k tomu, že výchozí formát pro parametry a konstanty se změnil, bude pravděpodobně nutné provést akci u většiny scénářů, které zahrnují identifikátory GUID.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-749">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="bd2dc-750">Hodnoty char se teď ukládají jako TEXT na SQLite.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-750">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="bd2dc-751">Sledování problému #15020</span><span class="sxs-lookup"><span data-stu-id="bd2dc-751">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="bd2dc-752">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-752">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-753">Hodnoty typu char byly dříve sored jako CELOČÍSELNé hodnoty u SQLite.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-753">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="bd2dc-754">Například hodnota znaku *a* byla uložena jako celočíselná hodnota 65.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-754">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="bd2dc-755">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-755">**New behavior**</span></span>

<span data-ttu-id="bd2dc-756">Hodnoty typu char jsou nyní uloženy jako TEXT.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-756">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="bd2dc-757">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-757">**Why**</span></span>

<span data-ttu-id="bd2dc-758">Ukládání hodnot jako TEXT je přirozenější a databáze usnadňuje kompatibilitu s jinými technologiemi.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-758">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="bd2dc-759">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-759">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-760">Existující databáze můžete migrovat do nového formátu tím, že spustíte příkaz SQL podobně jako následující.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-760">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="bd2dc-761">V EF Core můžete i nadále používat předchozí chování nakonfigurováním převaděče hodnot na těchto vlastnostech.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-761">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="bd2dc-762">Microsoft. data. sqlite také zůstává schopný číst znakové hodnoty z CELOČÍSELNého i TEXTOVÉHO sloupce, takže některé scénáře nemusí vyžadovat žádnou akci.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-762">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="bd2dc-763">ID migrace se teď generují pomocí kalendáře invariantní jazykové verze.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-763">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="bd2dc-764">Sledování problému #12978</span><span class="sxs-lookup"><span data-stu-id="bd2dc-764">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="bd2dc-765">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-765">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-766">ID migrace se nechtěně vygenerovala pomocí kalendáře aktuální jazykové verze.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-766">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="bd2dc-767">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-767">**New behavior**</span></span>

<span data-ttu-id="bd2dc-768">ID migrace se nyní vždy generují pomocí kalendáře neutrální jazykové verze (gregoriánský).</span><span class="sxs-lookup"><span data-stu-id="bd2dc-768">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="bd2dc-769">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-769">**Why**</span></span>

<span data-ttu-id="bd2dc-770">Pořadí migrace je důležité při aktualizaci databáze nebo řešení konfliktů při slučování.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-770">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="bd2dc-771">Pomocí invariantního kalendáře se vyhnete problémům s řazením, které mohou být výsledkem členů týmu jiné systémové kalendáře.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-771">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="bd2dc-772">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-772">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-773">Tato změna má vliv na kohokoli, kdo používá negregoriánský kalendář, ve kterém je rok větší než gregoriánský kalendář (například thajský buddhistický kalendář).</span><span class="sxs-lookup"><span data-stu-id="bd2dc-773">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="bd2dc-774">Existující identifikátory migrace se budou muset aktualizovat, aby se nové migrace objednaly po stávajících migracích.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-774">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="bd2dc-775">ID migrace najdete v atributu migrace v souborech návrháře migrace.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-775">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="bd2dc-776">Také je potřeba aktualizovat tabulku historie migrace.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-776">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="bd2dc-777">UseRowNumberForPaging se odebral.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-777">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="bd2dc-778">Sledování problému #16400</span><span class="sxs-lookup"><span data-stu-id="bd2dc-778">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="bd2dc-779">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-779">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-780">Před EF Core 3,0 lze pomocí `UseRowNumberForPaging` vygenerovat SQL pro stránkování, které je kompatibilní s SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-780">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="bd2dc-781">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-781">**New behavior**</span></span>

<span data-ttu-id="bd2dc-782">Počínaje EF Core 3,0 bude EF generovat pouze SQL pro stránkování, které je kompatibilní pouze s novějšími verzemi SQL Server.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-782">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="bd2dc-783">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-783">**Why**</span></span>

<span data-ttu-id="bd2dc-784">Tuto změnu provedeme, protože [SQL Server 2008 už není podporovaným produktem](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) a aktualizace této funkce tak, aby fungovala se změnami dotazů provedenými v EF Core 3,0 je významná práce.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-784">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="bd2dc-785">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-785">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-786">Doporučujeme aktualizovat na novější verzi SQL Server nebo pomocí vyšší úrovně kompatibility, aby byl vygenerovaný SQL podporován.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-786">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="bd2dc-787">To znamená, že pokud to nemůžete udělat, [komentář k problému s sledováním](https://github.com/aspnet/EntityFrameworkCore/issues/16400) najdete v podrobnostech.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-787">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="bd2dc-788">Toto rozhodnutí můžeme znovu navštívit na základě zpětné vazby.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-788">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="bd2dc-789">Informace o rozšíření/metadata se odebraly z IDbContextOptionsExtension.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-789">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="bd2dc-790">Sledování problému #16119</span><span class="sxs-lookup"><span data-stu-id="bd2dc-790">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="bd2dc-791">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-791">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-792">`IDbContextOptionsExtension` obsahovaly metody pro poskytování metadat o rozšíření.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-792">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="bd2dc-793">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-793">**New behavior**</span></span>

<span data-ttu-id="bd2dc-794">Tyto metody byly přesunuty do nové `DbContextOptionsExtensionInfo` abstraktní základní třídy, která je vrácena z nové vlastnosti `IDbContextOptionsExtension.Info`.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-794">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="bd2dc-795">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-795">**Why**</span></span>

<span data-ttu-id="bd2dc-796">V rámci vydání od 2,0 do 3,0 jsme potřebovali přidat nebo změnit tyto metody několikrát.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-796">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="bd2dc-797">Rozbalením do nové abstraktní základní třídy bude snazší vytvořit tyto změny bez přerušení stávajících rozšíření.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-797">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="bd2dc-798">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-798">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-799">Aktualizovat rozšíření tak, aby následovala nový vzor.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-799">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="bd2dc-800">Příklady najdete v mnoha implementacích `IDbContextOptionsExtension` různých druhů rozšíření ve zdrojovém kódu EF Core.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-800">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="bd2dc-801">LogQueryPossibleExceptionWithAggregateOperator se přejmenovalo.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-801">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="bd2dc-802">Sledování problému #10985</span><span class="sxs-lookup"><span data-stu-id="bd2dc-802">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="bd2dc-803">**Mění**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-803">**Change**</span></span>

<span data-ttu-id="bd2dc-804">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` bylo přejmenováno na `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-804">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="bd2dc-805">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-805">**Why**</span></span>

<span data-ttu-id="bd2dc-806">Zarovná pojmenování této události varování se všemi ostatními událostmi upozornění.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-806">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="bd2dc-807">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-807">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-808">Použijte nový název.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-808">Use the new name.</span></span> <span data-ttu-id="bd2dc-809">(Všimněte si, že číslo ID události se nezměnilo.)</span><span class="sxs-lookup"><span data-stu-id="bd2dc-809">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="bd2dc-810">Vysvětlení rozhraní API pro názvy omezení cizího klíče</span><span class="sxs-lookup"><span data-stu-id="bd2dc-810">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="bd2dc-811">Sledování problému #10730</span><span class="sxs-lookup"><span data-stu-id="bd2dc-811">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="bd2dc-812">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-812">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-813">Před EF Core 3,0 se názvy omezení cizího klíče odkazovaly jenom na "název".</span><span class="sxs-lookup"><span data-stu-id="bd2dc-813">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="bd2dc-814">Příklad:</span><span class="sxs-lookup"><span data-stu-id="bd2dc-814">For example:</span></span>

```csharp
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="bd2dc-815">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-815">**New behavior**</span></span>

<span data-ttu-id="bd2dc-816">Počínaje EF Core 3,0 se názvy omezení cizích klíčů teď označují jako "název omezení".</span><span class="sxs-lookup"><span data-stu-id="bd2dc-816">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="bd2dc-817">Příklad:</span><span class="sxs-lookup"><span data-stu-id="bd2dc-817">For example:</span></span>

```csharp
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="bd2dc-818">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-818">**Why**</span></span>

<span data-ttu-id="bd2dc-819">Tato změna přináší konzistenci pro pojmenování v této oblasti a také vysvětluje, že se jedná o název omezení cizího klíče, a nikoli název sloupce nebo vlastnosti, ve kterém je definován cizí klíč.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-819">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="bd2dc-820">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-820">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-821">Použijte nový název.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-821">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="bd2dc-822">IRelationalDatabaseCreator. HasTables/HasTablesAsync byly zveřejněny.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-822">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="bd2dc-823">Sledování problému #15997</span><span class="sxs-lookup"><span data-stu-id="bd2dc-823">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="bd2dc-824">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-824">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-825">Před EF Core 3,0 byly tyto metody chráněné.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-825">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="bd2dc-826">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-826">**New behavior**</span></span>

<span data-ttu-id="bd2dc-827">Počínaje EF Core 3,0 jsou tyto metody veřejné.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-827">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="bd2dc-828">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-828">**Why**</span></span>

<span data-ttu-id="bd2dc-829">Tyto metody jsou používány EF k určení, jestli je databáze vytvořená, ale prázdná.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-829">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="bd2dc-830">To může být užitečné taky od vnějšího EF při určování, jestli se mají migrace použít.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-830">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="bd2dc-831">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-831">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-832">Změňte přístupnost všech přepsání.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-832">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="bd2dc-833">Microsoft. EntityFrameworkCore. Design je teď balíček DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-833">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="bd2dc-834">Sledování problému #11506</span><span class="sxs-lookup"><span data-stu-id="bd2dc-834">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="bd2dc-835">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-835">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-836">Před EF Core 3,0 byl Microsoft. EntityFrameworkCore. Design regulárním balíčkem NuGet, na kterém by mohly být na sestavení odkazovány projekty, které na něm závisejí.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-836">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="bd2dc-837">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-837">**New behavior**</span></span>

<span data-ttu-id="bd2dc-838">Počínaje EF Core 3,0 se jedná o balíček DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-838">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="bd2dc-839">To znamená, že závislost nebude nijak přesměrovat do jiných projektů a že již nemůžete ve výchozím nastavení odkazovat na své sestavení.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-839">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="bd2dc-840">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-840">**Why**</span></span>

<span data-ttu-id="bd2dc-841">Tento balíček se má použít jenom v době návrhu.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-841">This package is only intended to be used at design time.</span></span> <span data-ttu-id="bd2dc-842">Nasazené aplikace by neměli na ni odkazovat.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-842">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="bd2dc-843">Díky tomu, že balíček DevelopmentDependency, toto doporučení posiluje.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-843">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="bd2dc-844">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-844">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-845">Pokud potřebujete odkazovat na tento balíček, aby bylo možné přepsat EF Core chování při návrhu, můžete aktualizovat metadata položky aktualizovat PackageReference v projektu.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-845">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="bd2dc-846">Pokud se na balíček odkazuje přes Microsoft. EntityFrameworkCore. Tools, budete muset do balíčku přidat explicitní PackageReference, aby se změnila jeho metadata.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-846">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="bd2dc-847">SQLitePCL. Raw aktualizováno na verzi 2.0.0</span><span class="sxs-lookup"><span data-stu-id="bd2dc-847">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="bd2dc-848">Sledování problému #14824</span><span class="sxs-lookup"><span data-stu-id="bd2dc-848">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="bd2dc-849">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-849">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-850">Microsoft. EntityFrameworkCore. sqlite byl dřív závislý na 1.1.12 verze SQLitePCL. Raw.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-850">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="bd2dc-851">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-851">**New behavior**</span></span>

<span data-ttu-id="bd2dc-852">Aktualizovali jsme náš balíček tak, aby byl závislý na verzi 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-852">We've updated our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="bd2dc-853">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-853">**Why**</span></span>

<span data-ttu-id="bd2dc-854">2\.0.0 verze SQLitePCL. Raw TARGETS .NET Standard 2,0.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-854">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="bd2dc-855">Dříve cílí na .NET Standard 1,1, které vyžadovaly, aby se při práci vytvořil velký uzávěr přenosných balíčků.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-855">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="bd2dc-856">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-856">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-857">SQLitePCL. Raw 2.0.0 verze obsahuje některé zásadní změny.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-857">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="bd2dc-858">Podrobnosti najdete v [poznámkách k verzi](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) .</span><span class="sxs-lookup"><span data-stu-id="bd2dc-858">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="bd2dc-859">NetTopologySuite aktualizace na verzi 2.0.0</span><span class="sxs-lookup"><span data-stu-id="bd2dc-859">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="bd2dc-860">Sledování problému #14825</span><span class="sxs-lookup"><span data-stu-id="bd2dc-860">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="bd2dc-861">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-861">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-862">Prostorové balíčky byly dříve závislé na 1.15.1 verze NetTopologySuite.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-862">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="bd2dc-863">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-863">**New behavior**</span></span>

<span data-ttu-id="bd2dc-864">Náš balíček jsme aktualizovali tak, aby byl závislý na verzi 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-864">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="bd2dc-865">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-865">**Why**</span></span>

<span data-ttu-id="bd2dc-866">2\.0.0 verze NetTopologySuite má za cíl řešit několik problémů s použitelností, ke kterým se EF Core uživatelé setkali.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-866">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="bd2dc-867">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-867">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-868">NetTopologySuite verze 2.0.0 obsahuje některé průlomové změny.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-868">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="bd2dc-869">Podrobnosti najdete v [poznámkách k verzi](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) .</span><span class="sxs-lookup"><span data-stu-id="bd2dc-869">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="SqlClient"></a>

### <a name="microsoftdatasqlclient-is-used-instead-of-systemdatasqlclient"></a><span data-ttu-id="bd2dc-870">Místo typu System. data. SqlClient se používá Microsoft. data. SqlClient.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-870">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>

[<span data-ttu-id="bd2dc-871">Sledování problému #15636</span><span class="sxs-lookup"><span data-stu-id="bd2dc-871">Tracking Issue #15636</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15636)

<span data-ttu-id="bd2dc-872">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-872">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-873">Microsoft. EntityFrameworkCore. SqlServer byl dřív závislý na System. data. SqlClient.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-873">Microsoft.EntityFrameworkCore.SqlServer previously depended on System.Data.SqlClient.</span></span>

<span data-ttu-id="bd2dc-874">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-874">**New behavior**</span></span>

<span data-ttu-id="bd2dc-875">Balíček jsme aktualizovali tak, aby byl závislý na Microsoft. data. SqlClient.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-875">We've updated our package to depend on Microsoft.Data.SqlClient.</span></span>

<span data-ttu-id="bd2dc-876">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-876">**Why**</span></span>

<span data-ttu-id="bd2dc-877">Microsoft. data. SqlClient je nejdůležitější ovladač pro přístup k datům, který je k dispozici pro SQL Server a System. data. SqlClient již není zaměřuje na vývoj.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-877">Microsoft.Data.SqlClient is the flagship data access driver for SQL Server going forward, and System.Data.SqlClient no longer be the focus of development.</span></span>
<span data-ttu-id="bd2dc-878">Některé důležité funkce, například Always Encrypted, jsou k dispozici pouze v Microsoft. data. SqlClient.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-878">Some important features, such as Always Encrypted, are only available on Microsoft.Data.SqlClient.</span></span>

<span data-ttu-id="bd2dc-879">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-879">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-880">Pokud váš kód používá přímou závislost na System. data. SqlClient, musíte ho změnit tak, aby odkazoval na Microsoft. data. SqlClient místo toho. vzhledem k tomu, že oba balíčky udržují velmi vysoký stupeň kompatibility rozhraní API, mělo by to být jenom jednoduchý balíček a Změna oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-880">If your code takes a direct dependency on System.Data.SqlClient, you must change it to reference Microsoft.Data.SqlClient instead; as the two packages maintain a very high degree of API compatibility, this should only be a simple package and namespace change.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="bd2dc-881">Je nutné nakonfigurovat více dvojznačných relací odkazujících na sebe.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-881">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="bd2dc-882">Sledování problému #13573</span><span class="sxs-lookup"><span data-stu-id="bd2dc-882">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="bd2dc-883">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-883">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-884">Typ entity s více jednosměrnou navigační vlastností a s vyhovující FKs byl nesprávně nakonfigurován jako jeden vztah.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-884">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="bd2dc-885">Příklad:</span><span class="sxs-lookup"><span data-stu-id="bd2dc-885">For example:</span></span>

```csharp
public class User 
{
        public Guid Id { get; set; }
        public User CreatedBy { get; set; }
        public User UpdatedBy { get; set; }
        public Guid CreatedById { get; set; }
        public Guid? UpdatedById { get; set; }
}
```

<span data-ttu-id="bd2dc-886">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-886">**New behavior**</span></span>

<span data-ttu-id="bd2dc-887">Tento scénář je nyní zjištěn v sestavování modelu a je vyvolána výjimka označující, že je model dvojznačný.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-887">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="bd2dc-888">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-888">**Why**</span></span>

<span data-ttu-id="bd2dc-889">Výsledný model byl nejednoznačný a pravděpodobně bude pro tento případ obvykle špatný.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-889">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="bd2dc-890">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-890">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-891">Použijte úplnou konfiguraci relace.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-891">Use full configuration of the relationship.</span></span> <span data-ttu-id="bd2dc-892">Příklad:</span><span class="sxs-lookup"><span data-stu-id="bd2dc-892">For example:</span></span>

```csharp
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
### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a><span data-ttu-id="bd2dc-893">DbFunction. Schema má hodnotu null nebo je prázdný řetězec, který nakonfiguruje, aby byl ve výchozím schématu modelu.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-893">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>

[<span data-ttu-id="bd2dc-894">Sledování problému #12757</span><span class="sxs-lookup"><span data-stu-id="bd2dc-894">Tracking Issue #12757</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12757)

<span data-ttu-id="bd2dc-895">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-895">**Old behavior**</span></span>

<span data-ttu-id="bd2dc-896">DbFunction nakonfigurovaný se schématem jako prázdný řetězec byl považován za vestavěnou funkci bez schématu.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-896">A DbFunction configured with schema as an empty string was treated as built-in function without a schema.</span></span> <span data-ttu-id="bd2dc-897">Například následující kód bude mapován `DatePart` funkci CLR na `DATEPART` vestavěnou funkci na SqlServer.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-897">For example following code will map `DatePart` CLR function to `DATEPART` built-in function on SqlServer.</span></span>

```csharp
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

<span data-ttu-id="bd2dc-898">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-898">**New behavior**</span></span>

<span data-ttu-id="bd2dc-899">Všechna mapování DbFunction se považují za namapovaná na uživatelsky definované funkce.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-899">All DbFunction mappings are considered to be mapped to user defined functions.</span></span> <span data-ttu-id="bd2dc-900">Proto je prázdná hodnota řetězce vložena do výchozího schématu pro model.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-900">Hence empty string value would put the function inside the default schema for the model.</span></span> <span data-ttu-id="bd2dc-901">To může být schéma, které je explicitně nakonfigurované prostřednictvím rozhraní Fluent API `modelBuilder.HasDefaultSchema()` nebo `dbo` jinak.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-901">Which could be the schema configured explicitly via fluent API `modelBuilder.HasDefaultSchema()` or `dbo` otherwise.</span></span>

<span data-ttu-id="bd2dc-902">**Proč**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-902">**Why**</span></span>

<span data-ttu-id="bd2dc-903">Dříve prázdné schéma bylo způsobem, jak se zacházet s touto funkcí, ale tato funkce je k dispozici pouze pro SqlServer, kde předdefinované funkce nepatří do žádného schématu.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-903">Previously schema being empty was a way to treat that function is built-in but that logic is only applicable for SqlServer where built-in functions do not belong to any schema.</span></span>

<span data-ttu-id="bd2dc-904">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="bd2dc-904">**Mitigations**</span></span>

<span data-ttu-id="bd2dc-905">Nakonfigurujte převod DbFunction ručně, abyste ho namapovali na vestavěnou funkci.</span><span class="sxs-lookup"><span data-stu-id="bd2dc-905">Configure DbFunction's translation manually to map it to a built-in function.</span></span>

```csharp
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```
