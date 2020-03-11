---
title: Přerušující změny v EF Core 3,0 – EF Core
author: ajcvickers
ms.date: 12/03/2019
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 6e0c17a22b56b206f18e47f678e3e237d5c42375
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417459"
---
# <a name="breaking-changes-included-in-ef-core-30"></a><span data-ttu-id="2be1f-102">Přerušující změny zahrnuté v EF Core 3,0</span><span class="sxs-lookup"><span data-stu-id="2be1f-102">Breaking changes included in EF Core 3.0</span></span>

<span data-ttu-id="2be1f-103">Následující změny rozhraní API a chování mají možnost rušit existující aplikace při jejich upgradu na 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="2be1f-103">The following API and behavior changes have the potential to break existing applications when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="2be1f-104">Změny, které očekáváme jenom o to, aby ovlivnili pouze poskytovatele databází, jsou popsané v části [změny zprostředkovatele](xref:core/providers/provider-log).</span><span class="sxs-lookup"><span data-stu-id="2be1f-104">Changes that we expect to only impact database providers are documented under [provider changes](xref:core/providers/provider-log).</span></span>

## <a name="summary"></a><span data-ttu-id="2be1f-105">Souhrn</span><span class="sxs-lookup"><span data-stu-id="2be1f-105">Summary</span></span>

| <span data-ttu-id="2be1f-106">**Zásadní změna**</span><span class="sxs-lookup"><span data-stu-id="2be1f-106">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="2be1f-107">**Dopad**</span><span class="sxs-lookup"><span data-stu-id="2be1f-107">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="2be1f-108">Dotazy LINQ již nejsou vyhodnocovány na klientovi.</span><span class="sxs-lookup"><span data-stu-id="2be1f-108">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="2be1f-109">Vysoký</span><span class="sxs-lookup"><span data-stu-id="2be1f-109">High</span></span>       |
| [<span data-ttu-id="2be1f-110">EF Core 3,0 cíle .NET Standard 2,1 místo .NET Standard 2,0</span><span class="sxs-lookup"><span data-stu-id="2be1f-110">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="2be1f-111">Vysoký</span><span class="sxs-lookup"><span data-stu-id="2be1f-111">High</span></span>      |
| [<span data-ttu-id="2be1f-112">EF Core nástroj příkazového řádku dotnet EF již není součástí .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="2be1f-112">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="2be1f-113">Vysoký</span><span class="sxs-lookup"><span data-stu-id="2be1f-113">High</span></span>      |
| [<span data-ttu-id="2be1f-114">DetectChanges respektuje hodnoty klíčů generované úložištěm.</span><span class="sxs-lookup"><span data-stu-id="2be1f-114">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="2be1f-115">Vysoký</span><span class="sxs-lookup"><span data-stu-id="2be1f-115">High</span></span>      |
| [<span data-ttu-id="2be1f-116">Z tabulek, ExecuteSql a ExecuteSqlAsync byly přejmenovány.</span><span class="sxs-lookup"><span data-stu-id="2be1f-116">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="2be1f-117">Vysoký</span><span class="sxs-lookup"><span data-stu-id="2be1f-117">High</span></span>      |
| [<span data-ttu-id="2be1f-118">Typy dotazů jsou konsolidovány s typy entit</span><span class="sxs-lookup"><span data-stu-id="2be1f-118">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="2be1f-119">Vysoký</span><span class="sxs-lookup"><span data-stu-id="2be1f-119">High</span></span>      |
| [<span data-ttu-id="2be1f-120">Entity Framework Core už není součástí sdílené ASP.NET Core architektury.</span><span class="sxs-lookup"><span data-stu-id="2be1f-120">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="2be1f-121">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="2be1f-121">Medium</span></span>      |
| [<span data-ttu-id="2be1f-122">Odstranění kaskádových operací se teď ve výchozím nastavení provádí hned.</span><span class="sxs-lookup"><span data-stu-id="2be1f-122">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="2be1f-123">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="2be1f-123">Medium</span></span>      |
| [<span data-ttu-id="2be1f-124">Eager načítání souvisejících entit se teď děje v jednom dotazu.</span><span class="sxs-lookup"><span data-stu-id="2be1f-124">Eager loading of related entities now happens in a single query</span></span>](#eager-loading-single-query) | <span data-ttu-id="2be1f-125">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="2be1f-125">Medium</span></span>      |
| [<span data-ttu-id="2be1f-126">DeleteBehavior. restrict má sémantiku čištění.</span><span class="sxs-lookup"><span data-stu-id="2be1f-126">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="2be1f-127">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="2be1f-127">Medium</span></span>      |
| [<span data-ttu-id="2be1f-128">Změnilo se konfigurační rozhraní API pro vztahy vlastněných typů.</span><span class="sxs-lookup"><span data-stu-id="2be1f-128">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="2be1f-129">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="2be1f-129">Medium</span></span>      |
| [<span data-ttu-id="2be1f-130">Každá vlastnost používá nezávislou generaci celočíselného klíče v paměti.</span><span class="sxs-lookup"><span data-stu-id="2be1f-130">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="2be1f-131">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="2be1f-131">Medium</span></span>      |
| [<span data-ttu-id="2be1f-132">Žádné dotazy pro sledování neprovádějí překlad identity</span><span class="sxs-lookup"><span data-stu-id="2be1f-132">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="2be1f-133">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="2be1f-133">Medium</span></span>      |
| [<span data-ttu-id="2be1f-134">Změny rozhraní API pro metadata</span><span class="sxs-lookup"><span data-stu-id="2be1f-134">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="2be1f-135">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="2be1f-135">Medium</span></span>      |
| [<span data-ttu-id="2be1f-136">Změny rozhraní API pro konkrétního zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="2be1f-136">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="2be1f-137">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="2be1f-137">Medium</span></span>      |
| [<span data-ttu-id="2be1f-138">UseRowNumberForPaging se odebral.</span><span class="sxs-lookup"><span data-stu-id="2be1f-138">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="2be1f-139">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="2be1f-139">Medium</span></span>      |
| [<span data-ttu-id="2be1f-140">Metoda Z tabulek při použití s uloženou procedurou nemůže být složená.</span><span class="sxs-lookup"><span data-stu-id="2be1f-140">FromSql method when used with stored procedure cannot be composed</span></span>](#fromsqlsproc) | <span data-ttu-id="2be1f-141">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="2be1f-141">Medium</span></span>      |
| [<span data-ttu-id="2be1f-142">Metody Z tabulek se dají zadat jenom v kořenech dotazů.</span><span class="sxs-lookup"><span data-stu-id="2be1f-142">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="2be1f-143">Nízká</span><span class="sxs-lookup"><span data-stu-id="2be1f-143">Low</span></span>      |
| [<span data-ttu-id="2be1f-144">~~Provádění dotazu se protokoluje na úrovni ladění~~ . Vrátit</span><span class="sxs-lookup"><span data-stu-id="2be1f-144">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="2be1f-145">Nízká</span><span class="sxs-lookup"><span data-stu-id="2be1f-145">Low</span></span>      |
| [<span data-ttu-id="2be1f-146">Dočasné hodnoty klíčů už nejsou nastavené na instance entit.</span><span class="sxs-lookup"><span data-stu-id="2be1f-146">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="2be1f-147">Nízká</span><span class="sxs-lookup"><span data-stu-id="2be1f-147">Low</span></span>      |
| [<span data-ttu-id="2be1f-148">Závislé entity, které sdílí tabulku s objektem zabezpečení, jsou teď volitelné.</span><span class="sxs-lookup"><span data-stu-id="2be1f-148">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="2be1f-149">Nízká</span><span class="sxs-lookup"><span data-stu-id="2be1f-149">Low</span></span>      |
| [<span data-ttu-id="2be1f-150">Všechny entity sdílející tabulku se sloupcem souběžného tokenu musí být namapovány na vlastnost.</span><span class="sxs-lookup"><span data-stu-id="2be1f-150">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="2be1f-151">Nízká</span><span class="sxs-lookup"><span data-stu-id="2be1f-151">Low</span></span>      |
| [<span data-ttu-id="2be1f-152">Na vlastněné entity se nedá dotazovat bez vlastníka pomocí sledovacího dotazu.</span><span class="sxs-lookup"><span data-stu-id="2be1f-152">Owned entities cannot be queried without the owner using a tracking query</span></span>](#owned-query) | <span data-ttu-id="2be1f-153">Nízká</span><span class="sxs-lookup"><span data-stu-id="2be1f-153">Low</span></span>      |
| [<span data-ttu-id="2be1f-154">Zděděné vlastnosti z nemapovaných typů jsou nyní namapovány na jeden sloupec pro všechny odvozené typy.</span><span class="sxs-lookup"><span data-stu-id="2be1f-154">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="2be1f-155">Nízká</span><span class="sxs-lookup"><span data-stu-id="2be1f-155">Low</span></span>      |
| [<span data-ttu-id="2be1f-156">Konvence vlastností cizího klíče už neodpovídá stejnému názvu jako vlastnost Principal.</span><span class="sxs-lookup"><span data-stu-id="2be1f-156">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="2be1f-157">Nízká</span><span class="sxs-lookup"><span data-stu-id="2be1f-157">Low</span></span>      |
| [<span data-ttu-id="2be1f-158">Připojení k databázi je teď zavřené, pokud už nepoužíváte dřív, než se dokončí jeho objekt TransactionScope.</span><span class="sxs-lookup"><span data-stu-id="2be1f-158">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="2be1f-159">Nízká</span><span class="sxs-lookup"><span data-stu-id="2be1f-159">Low</span></span>      |
| [<span data-ttu-id="2be1f-160">Ve výchozím nastavení se používají pole pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="2be1f-160">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="2be1f-161">Nízká</span><span class="sxs-lookup"><span data-stu-id="2be1f-161">Low</span></span>      |
| [<span data-ttu-id="2be1f-162">Vyvolat, zda je nalezeno více kompatibilních zálohovaných polí</span><span class="sxs-lookup"><span data-stu-id="2be1f-162">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="2be1f-163">Nízká</span><span class="sxs-lookup"><span data-stu-id="2be1f-163">Low</span></span>      |
| [<span data-ttu-id="2be1f-164">Názvy vlastností pouze pro pole se musí shodovat s názvem pole.</span><span class="sxs-lookup"><span data-stu-id="2be1f-164">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="2be1f-165">Nízká</span><span class="sxs-lookup"><span data-stu-id="2be1f-165">Low</span></span>      |
| [<span data-ttu-id="2be1f-166">AddDbContext/AddDbContextPool už nevolá AddLogging a AddMemoryCache.</span><span class="sxs-lookup"><span data-stu-id="2be1f-166">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="2be1f-167">Nízká</span><span class="sxs-lookup"><span data-stu-id="2be1f-167">Low</span></span>      |
| [<span data-ttu-id="2be1f-168">AddEntityFramework \* přidá IMemoryCache s omezením velikosti.</span><span class="sxs-lookup"><span data-stu-id="2be1f-168">AddEntityFramework\* adds IMemoryCache with a size limit</span></span>](#addentityframework-adds-imemorycache-with-a-size-limit) | <span data-ttu-id="2be1f-169">Nízká</span><span class="sxs-lookup"><span data-stu-id="2be1f-169">Low</span></span>      |
| [<span data-ttu-id="2be1f-170">DbContext. entry teď provádí místní DetectChanges.</span><span class="sxs-lookup"><span data-stu-id="2be1f-170">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="2be1f-171">Nízká</span><span class="sxs-lookup"><span data-stu-id="2be1f-171">Low</span></span>      |
| [<span data-ttu-id="2be1f-172">Klíče řetězce a pole bajtů nejsou ve výchozím nastavení generovány klientem.</span><span class="sxs-lookup"><span data-stu-id="2be1f-172">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="2be1f-173">Nízká</span><span class="sxs-lookup"><span data-stu-id="2be1f-173">Low</span></span>      |
| [<span data-ttu-id="2be1f-174">ILoggerFactory je teď služba s vymezeným oborem.</span><span class="sxs-lookup"><span data-stu-id="2be1f-174">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="2be1f-175">Nízká</span><span class="sxs-lookup"><span data-stu-id="2be1f-175">Low</span></span>      |
| [<span data-ttu-id="2be1f-176">Opožděné načítání proxy serverů už nepředpokládá navigační vlastnosti, jsou plně načtené.</span><span class="sxs-lookup"><span data-stu-id="2be1f-176">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="2be1f-177">Nízká</span><span class="sxs-lookup"><span data-stu-id="2be1f-177">Low</span></span>      |
| [<span data-ttu-id="2be1f-178">Nadměrné vytváření interních zprostředkovatelů služeb je teď ve výchozím nastavení chyba.</span><span class="sxs-lookup"><span data-stu-id="2be1f-178">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="2be1f-179">Nízká</span><span class="sxs-lookup"><span data-stu-id="2be1f-179">Low</span></span>      |
| [<span data-ttu-id="2be1f-180">Nové chování pro HasOne/HasMany se volá s jedním řetězcem.</span><span class="sxs-lookup"><span data-stu-id="2be1f-180">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="2be1f-181">Nízká</span><span class="sxs-lookup"><span data-stu-id="2be1f-181">Low</span></span>      |
| [<span data-ttu-id="2be1f-182">Návratový typ pro několik asynchronních metod byl změněn z úlohy na ValueTask</span><span class="sxs-lookup"><span data-stu-id="2be1f-182">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="2be1f-183">Nízká</span><span class="sxs-lookup"><span data-stu-id="2be1f-183">Low</span></span>      |
| [<span data-ttu-id="2be1f-184">Relační: anotace TypeMapping je nyní pouze TypeMapping</span><span class="sxs-lookup"><span data-stu-id="2be1f-184">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="2be1f-185">Nízká</span><span class="sxs-lookup"><span data-stu-id="2be1f-185">Low</span></span>      |
| [<span data-ttu-id="2be1f-186">ToTable na odvozeném typu vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="2be1f-186">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="2be1f-187">Nízká</span><span class="sxs-lookup"><span data-stu-id="2be1f-187">Low</span></span>      |
| [<span data-ttu-id="2be1f-188">EF Core už neposílá direktivu pragma pro vynucení KOFK SQLite.</span><span class="sxs-lookup"><span data-stu-id="2be1f-188">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="2be1f-189">Nízká</span><span class="sxs-lookup"><span data-stu-id="2be1f-189">Low</span></span>      |
| [<span data-ttu-id="2be1f-190">Microsoft. EntityFrameworkCore. sqlite teď závisí na SQLitePCLRaw. bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="2be1f-190">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="2be1f-191">Nízká</span><span class="sxs-lookup"><span data-stu-id="2be1f-191">Low</span></span>      |
| [<span data-ttu-id="2be1f-192">Hodnoty GUID se teď ukládají jako TEXT na SQLite.</span><span class="sxs-lookup"><span data-stu-id="2be1f-192">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="2be1f-193">Nízká</span><span class="sxs-lookup"><span data-stu-id="2be1f-193">Low</span></span>      |
| [<span data-ttu-id="2be1f-194">Hodnoty char se teď ukládají jako TEXT na SQLite.</span><span class="sxs-lookup"><span data-stu-id="2be1f-194">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="2be1f-195">Nízká</span><span class="sxs-lookup"><span data-stu-id="2be1f-195">Low</span></span>      |
| [<span data-ttu-id="2be1f-196">ID migrace se teď generují pomocí kalendáře invariantní jazykové verze.</span><span class="sxs-lookup"><span data-stu-id="2be1f-196">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="2be1f-197">Nízká</span><span class="sxs-lookup"><span data-stu-id="2be1f-197">Low</span></span>      |
| [<span data-ttu-id="2be1f-198">Informace o rozšíření/metadata se odebraly z IDbContextOptionsExtension.</span><span class="sxs-lookup"><span data-stu-id="2be1f-198">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="2be1f-199">Nízká</span><span class="sxs-lookup"><span data-stu-id="2be1f-199">Low</span></span>      |
| [<span data-ttu-id="2be1f-200">LogQueryPossibleExceptionWithAggregateOperator se přejmenovalo.</span><span class="sxs-lookup"><span data-stu-id="2be1f-200">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="2be1f-201">Nízká</span><span class="sxs-lookup"><span data-stu-id="2be1f-201">Low</span></span>      |
| [<span data-ttu-id="2be1f-202">Vysvětlení rozhraní API pro názvy omezení cizího klíče</span><span class="sxs-lookup"><span data-stu-id="2be1f-202">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="2be1f-203">Nízká</span><span class="sxs-lookup"><span data-stu-id="2be1f-203">Low</span></span>      |
| [<span data-ttu-id="2be1f-204">IRelationalDatabaseCreator. HasTables/HasTablesAsync byly zveřejněny.</span><span class="sxs-lookup"><span data-stu-id="2be1f-204">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="2be1f-205">Nízká</span><span class="sxs-lookup"><span data-stu-id="2be1f-205">Low</span></span>      |
| [<span data-ttu-id="2be1f-206">Microsoft. EntityFrameworkCore. Design je teď balíček DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="2be1f-206">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="2be1f-207">Nízká</span><span class="sxs-lookup"><span data-stu-id="2be1f-207">Low</span></span>      |
| [<span data-ttu-id="2be1f-208">SQLitePCL. Raw aktualizováno na verzi 2.0.0</span><span class="sxs-lookup"><span data-stu-id="2be1f-208">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="2be1f-209">Nízká</span><span class="sxs-lookup"><span data-stu-id="2be1f-209">Low</span></span>      |
| [<span data-ttu-id="2be1f-210">NetTopologySuite aktualizace na verzi 2.0.0</span><span class="sxs-lookup"><span data-stu-id="2be1f-210">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="2be1f-211">Nízká</span><span class="sxs-lookup"><span data-stu-id="2be1f-211">Low</span></span>      |
| [<span data-ttu-id="2be1f-212">Místo typu System. data. SqlClient se používá Microsoft. data. SqlClient.</span><span class="sxs-lookup"><span data-stu-id="2be1f-212">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>](#SqlClient) | <span data-ttu-id="2be1f-213">Nízká</span><span class="sxs-lookup"><span data-stu-id="2be1f-213">Low</span></span>      |
| [<span data-ttu-id="2be1f-214">Je nutné nakonfigurovat více dvojznačných relací odkazujících na sebe.</span><span class="sxs-lookup"><span data-stu-id="2be1f-214">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="2be1f-215">Nízká</span><span class="sxs-lookup"><span data-stu-id="2be1f-215">Low</span></span>      |
| [<span data-ttu-id="2be1f-216">DbFunction. Schema má hodnotu null nebo je prázdný řetězec, který nakonfiguruje, aby byl ve výchozím schématu modelu.</span><span class="sxs-lookup"><span data-stu-id="2be1f-216">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>](#udf-empty-string) | <span data-ttu-id="2be1f-217">Nízká</span><span class="sxs-lookup"><span data-stu-id="2be1f-217">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="2be1f-218">Dotazy LINQ již nejsou vyhodnocovány na klientovi.</span><span class="sxs-lookup"><span data-stu-id="2be1f-218">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="2be1f-219">[Sledování problému #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[také zobrazit #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="2be1f-219">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="2be1f-220">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-220">**Old behavior**</span></span>

<span data-ttu-id="2be1f-221">Před 3,0, když EF Core nedokázal převést výraz, který byl součástí dotazu na buď SQL, nebo parametr, automaticky vyhodnotí výraz na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="2be1f-221">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="2be1f-222">Ve výchozím nastavení vyhodnocování klientů u potenciálně nákladných výrazů aktivovalo pouze upozornění.</span><span class="sxs-lookup"><span data-stu-id="2be1f-222">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="2be1f-223">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-223">**New behavior**</span></span>

<span data-ttu-id="2be1f-224">Počínaje 3,0 EF Core v projekci nejvyšší úrovně povolit jenom výrazy (poslední `Select()` volání v dotazu), které se mají vyhodnotit na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="2be1f-224">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="2be1f-225">Pokud výrazy v jakékoli jiné části dotazu nelze převést na buď SQL, nebo parametr, je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="2be1f-225">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="2be1f-226">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-226">**Why**</span></span>

<span data-ttu-id="2be1f-227">Automatické hodnocení dotazů umožňuje provádět mnoho dotazů i v případě, že není možné přeložit jejich důležité části.</span><span class="sxs-lookup"><span data-stu-id="2be1f-227">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="2be1f-228">Toto chování může vést k neočekávanému a potenciálně škodlivému chování, které může být jen zřejmé v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="2be1f-228">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="2be1f-229">Například podmínka ve volání `Where()`, která se nedá přeložit, může způsobit, že se všechny řádky z tabulky přenesou z databázového serveru a filtr, který se má použít na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="2be1f-229">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="2be1f-230">Tato situace může snadno přejít zpět, pokud tabulka obsahuje jenom několik řádků ve vývoji, ale při přesunu aplikace do produkčního prostředí zasáhnout, kde tabulka může obsahovat miliony řádků.</span><span class="sxs-lookup"><span data-stu-id="2be1f-230">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="2be1f-231">Upozornění na vyhodnocení klientů se také během vývoje ukázala jako příliš jednoduchá.</span><span class="sxs-lookup"><span data-stu-id="2be1f-231">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="2be1f-232">Kromě toho může automatické hodnocení klienta vést k problémům s tím, že vylepšení překladu dotazů pro konkrétní výrazy způsobilo nezamýšlené neúmyslné změny mezi verzemi.</span><span class="sxs-lookup"><span data-stu-id="2be1f-232">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="2be1f-233">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-233">**Mitigations**</span></span>

<span data-ttu-id="2be1f-234">Pokud dotaz nelze plně přeložit, přepište ho do formuláře, který se dá přeložit, nebo použijte `AsEnumerable()`, `ToList()`nebo podobným způsobem, aby se data vrátila zpět do klienta, kde je pak možné dále zpracovávat pomocí LINQ-to-Objects.</span><span class="sxs-lookup"><span data-stu-id="2be1f-234">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="2be1f-235">EF Core 3,0 cíle .NET Standard 2,1 místo .NET Standard 2,0</span><span class="sxs-lookup"><span data-stu-id="2be1f-235">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="2be1f-236">Sledování problému #15498</span><span class="sxs-lookup"><span data-stu-id="2be1f-236">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

> [!IMPORTANT] 
> <span data-ttu-id="2be1f-237">EF Core 3,1 cílů .NET Standard 2,0 znovu.</span><span class="sxs-lookup"><span data-stu-id="2be1f-237">EF Core 3.1 targets .NET Standard 2.0 again.</span></span> <span data-ttu-id="2be1f-238">To přináší zpětnou podporu pro .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="2be1f-238">This brings back support for .NET Framework.</span></span>

<span data-ttu-id="2be1f-239">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-239">**Old behavior**</span></span>

<span data-ttu-id="2be1f-240">Před 3,0 EF Core cílené .NET Standard 2,0 a spustí se na všech platformách, které podporují tento standard, včetně .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="2be1f-240">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="2be1f-241">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-241">**New behavior**</span></span>

<span data-ttu-id="2be1f-242">Počínaje 3,0 se EF Core cíle .NET Standard 2,1 a spustí se na všech platformách, které podporují tento standard.</span><span class="sxs-lookup"><span data-stu-id="2be1f-242">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="2be1f-243">To nezahrnuje .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="2be1f-243">This does not include .NET Framework.</span></span>

<span data-ttu-id="2be1f-244">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-244">**Why**</span></span>

<span data-ttu-id="2be1f-245">Toto je součást strategického rozhodnutí napříč technologiemi .NET a zaměřuje se na energii na platformě .NET Core a dalších moderních platformách .NET, jako je Xamarin.</span><span class="sxs-lookup"><span data-stu-id="2be1f-245">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="2be1f-246">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-246">**Mitigations**</span></span>

<span data-ttu-id="2be1f-247">Použijte EF Core 3,1.</span><span class="sxs-lookup"><span data-stu-id="2be1f-247">Use EF Core 3.1.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="2be1f-248">Entity Framework Core už není součástí sdílené ASP.NET Core architektury.</span><span class="sxs-lookup"><span data-stu-id="2be1f-248">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="2be1f-249">Sledování oznámení o problémech # 325</span><span class="sxs-lookup"><span data-stu-id="2be1f-249">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="2be1f-250">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-250">**Old behavior**</span></span>

<span data-ttu-id="2be1f-251">Před ASP.NET Core 3,0 se při přidání odkazu na balíček do `Microsoft.AspNetCore.App` nebo `Microsoft.AspNetCore.All`zahrnuly EF Core a někteří poskytovatelé EF Core dat jako poskytovatelé SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2be1f-251">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="2be1f-252">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-252">**New behavior**</span></span>

<span data-ttu-id="2be1f-253">Počínaje 3,0 ASP.NET Core sdílené rozhraní nezahrnuje EF Core nebo žádné poskytovatele EF Core dat.</span><span class="sxs-lookup"><span data-stu-id="2be1f-253">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="2be1f-254">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-254">**Why**</span></span>

<span data-ttu-id="2be1f-255">Před touto změnou EF Core nutné jiné kroky v závislosti na tom, zda je aplikace cílena ASP.NET Core a SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2be1f-255">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="2be1f-256">Upgrade ASP.NET Core taky vynutil upgrade EF Core a poskytovatele SQL Server, který není vždycky žádoucí.</span><span class="sxs-lookup"><span data-stu-id="2be1f-256">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="2be1f-257">V důsledku této změny je prostředí získání EF Core stejné u všech poskytovatelů, podporovaných implementací rozhraní .NET a typů aplikací.</span><span class="sxs-lookup"><span data-stu-id="2be1f-257">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="2be1f-258">Vývojáři teď můžou přesně řídit, když EF Core a EF Core se upgradují poskytovatelé dat.</span><span class="sxs-lookup"><span data-stu-id="2be1f-258">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="2be1f-259">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-259">**Mitigations**</span></span>

<span data-ttu-id="2be1f-260">Chcete-li použít EF Core v aplikaci ASP.NET Core 3,0 nebo v jakékoli jiné podporované aplikaci, explicitně přidejte odkaz na balíček EF Coreho poskytovatele databáze, který bude aplikace používat.</span><span class="sxs-lookup"><span data-stu-id="2be1f-260">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="2be1f-261">EF Core nástroj příkazového řádku dotnet EF již není součástí .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="2be1f-261">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="2be1f-262">Sledování problému #14016</span><span class="sxs-lookup"><span data-stu-id="2be1f-262">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="2be1f-263">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-263">**Old behavior**</span></span>

<span data-ttu-id="2be1f-264">Před 3,0 byl nástroj `dotnet ef` součástí .NET Core SDK a byl snadno dostupný pro použití z příkazového řádku z libovolného projektu bez nutnosti dalších kroků.</span><span class="sxs-lookup"><span data-stu-id="2be1f-264">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="2be1f-265">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-265">**New behavior**</span></span>

<span data-ttu-id="2be1f-266">Počínaje 3,0 sada .NET SDK neobsahuje nástroj `dotnet ef`, takže před tím, než ho bude možné použít, je nutné explicitně nainstalovat jako místní nebo globální nástroj.</span><span class="sxs-lookup"><span data-stu-id="2be1f-266">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="2be1f-267">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-267">**Why**</span></span>

<span data-ttu-id="2be1f-268">Tato změna nám umožňuje distribuovat a aktualizovat `dotnet ef` jako regulární nástroj rozhraní příkazového řádku .NET v NuGet, a to v souladu se skutečností, že EF Core 3,0 je také vždy distribuován jako balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="2be1f-268">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="2be1f-269">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-269">**Mitigations**</span></span>

<span data-ttu-id="2be1f-270">Aby bylo možné spravovat migrace nebo `DbContext`uživatelského rozhraní, nainstalujte `dotnet-ef` jako globální nástroj:</span><span class="sxs-lookup"><span data-stu-id="2be1f-270">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef
  ```

<span data-ttu-id="2be1f-271">Můžete také získat místní nástroj při obnovování závislostí projektu, který deklaruje jako závislost nástrojů pomocí [souboru manifestu nástroje](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="2be1f-271">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="2be1f-272">Z tabulek, ExecuteSql a ExecuteSqlAsync byly přejmenovány.</span><span class="sxs-lookup"><span data-stu-id="2be1f-272">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="2be1f-273">Sledování problému #10996</span><span class="sxs-lookup"><span data-stu-id="2be1f-273">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="2be1f-274">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-274">**Old behavior**</span></span>

<span data-ttu-id="2be1f-275">Před EF Core 3,0 byly tyto názvy metod přetíženy, aby fungovaly s normálním řetězcem nebo řetězcem, které by měly být interpolované na SQL a parametry.</span><span class="sxs-lookup"><span data-stu-id="2be1f-275">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="2be1f-276">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-276">**New behavior**</span></span>

<span data-ttu-id="2be1f-277">Počínaje EF Core 3,0 použijte `FromSqlRaw`, `ExecuteSqlRaw`a `ExecuteSqlRawAsync` k vytvoření parametrizovaného dotazu, kde jsou parametry předány nezávisle na řetězci dotazu.</span><span class="sxs-lookup"><span data-stu-id="2be1f-277">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="2be1f-278">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2be1f-278">For example:</span></span>

```csharp
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="2be1f-279">Pomocí `FromSqlInterpolated`, `ExecuteSqlInterpolated`a `ExecuteSqlInterpolatedAsync` vytvořte parametrizovaný dotaz, ve kterém jsou parametry předány jako součást interpolované řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="2be1f-279">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="2be1f-280">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2be1f-280">For example:</span></span>

```csharp
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="2be1f-281">Všimněte si, že obě výše uvedené dotazy vytvoří stejný parametrizovaný SQL se stejnými parametry SQL.</span><span class="sxs-lookup"><span data-stu-id="2be1f-281">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="2be1f-282">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-282">**Why**</span></span>

<span data-ttu-id="2be1f-283">Přetížení metody, jako to, usnadňuje náhodné volání nezpracované řetězcové metody, pokud by záměr byl zavolat interpolovaná řetězcová metoda a druhá možnost kolem.</span><span class="sxs-lookup"><span data-stu-id="2be1f-283">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="2be1f-284">To může vést k tomu, že dotazy nejsou parametrizované, pokud by měly být.</span><span class="sxs-lookup"><span data-stu-id="2be1f-284">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="2be1f-285">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-285">**Mitigations**</span></span>

<span data-ttu-id="2be1f-286">Přepněte na použití nových názvů metod.</span><span class="sxs-lookup"><span data-stu-id="2be1f-286">Switch to use the new method names.</span></span>

<a name="fromsqlsproc"></a>
### <a name="fromsql-method-when-used-with-stored-procedure-cannot-be-composed"></a><span data-ttu-id="2be1f-287">Metoda Z tabulek při použití s uloženou procedurou nemůže být složená.</span><span class="sxs-lookup"><span data-stu-id="2be1f-287">FromSql method when used with stored procedure cannot be composed</span></span>

[<span data-ttu-id="2be1f-288">Sledování problému #15392</span><span class="sxs-lookup"><span data-stu-id="2be1f-288">Tracking Issue #15392</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15392)

<span data-ttu-id="2be1f-289">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-289">**Old behavior**</span></span>

<span data-ttu-id="2be1f-290">Před EF Core 3,0 se metoda Z tabulek pokusila zjistit, zda je možné sestavit předaný SQL.</span><span class="sxs-lookup"><span data-stu-id="2be1f-290">Before EF Core 3.0, FromSql method tried to detect if the passed SQL can be composed upon.</span></span> <span data-ttu-id="2be1f-291">Vyvolalo se hodnocení klienta, pokud SQL bylo bez možnosti složení, jako je uložená procedura.</span><span class="sxs-lookup"><span data-stu-id="2be1f-291">It did client evaluation when the SQL was non-composable like a stored procedure.</span></span> <span data-ttu-id="2be1f-292">Následující dotaz fungoval spuštěním uložené procedury na serveru a provedením FirstOrDefault na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="2be1f-292">The following query worked by running the stored procedure on the server and doing FirstOrDefault on the client side.</span></span>

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").FirstOrDefault();
```

<span data-ttu-id="2be1f-293">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-293">**New behavior**</span></span>

<span data-ttu-id="2be1f-294">Počínaje EF Core 3,0 se EF Core nepokusí analyzovat SQL.</span><span class="sxs-lookup"><span data-stu-id="2be1f-294">Starting with EF Core 3.0, EF Core will not try to parse the SQL.</span></span> <span data-ttu-id="2be1f-295">Takže pokud vytváříte po FromSqlRaw/FromSqlInterpolated, pak EF Core vytvoří příkaz SQL, který by způsobil dotaz sub.</span><span class="sxs-lookup"><span data-stu-id="2be1f-295">So if you are composing after FromSqlRaw/FromSqlInterpolated, then EF Core will compose the SQL by causing sub query.</span></span> <span data-ttu-id="2be1f-296">Takže pokud používáte uloženou proceduru se složením, zobrazí se výjimka pro neplatnou syntaxi SQL.</span><span class="sxs-lookup"><span data-stu-id="2be1f-296">So if you are using a stored procedure with composition then you will get an exception for invalid SQL syntax.</span></span>

<span data-ttu-id="2be1f-297">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-297">**Why**</span></span>

<span data-ttu-id="2be1f-298">EF Core 3,0 nepodporuje automatické hodnocení klienta, protože to bylo náchylné k chybám, jak je vysvětleno [zde](#linq-queries-are-no-longer-evaluated-on-the-client).</span><span class="sxs-lookup"><span data-stu-id="2be1f-298">EF Core 3.0 does not support automatic client evaluation, since it was error prone as explained [here](#linq-queries-are-no-longer-evaluated-on-the-client).</span></span>

<span data-ttu-id="2be1f-299">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-299">**Mitigation**</span></span>

<span data-ttu-id="2be1f-300">Pokud používáte uloženou proceduru v FromSqlRaw/FromSqlInterpolated, znamená to, že se na ni nelze založit, takže můžete přidat __AsEnumerable/AsAsyncEnumerable__ hned po volání metody z tabulek, aby se zabránilo jakémukoli složení na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="2be1f-300">If you are using a stored procedure in FromSqlRaw/FromSqlInterpolated, you know that it cannot be composed upon, so you can add __AsEnumerable/AsAsyncEnumerable__ right after the FromSql method call to avoid any composition on server side.</span></span>

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").AsEnumerable().FirstOrDefault();
```

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="2be1f-301">Metody Z tabulek se dají zadat jenom v kořenech dotazů.</span><span class="sxs-lookup"><span data-stu-id="2be1f-301">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="2be1f-302">Sledování problému #15704</span><span class="sxs-lookup"><span data-stu-id="2be1f-302">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="2be1f-303">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-303">**Old behavior**</span></span>

<span data-ttu-id="2be1f-304">Před EF Core 3,0 lze zadat `FromSql` metodu, která je kdekoli v dotazu.</span><span class="sxs-lookup"><span data-stu-id="2be1f-304">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="2be1f-305">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-305">**New behavior**</span></span>

<span data-ttu-id="2be1f-306">Počínaje EF Core 3,0 lze nové metody `FromSqlRaw` a `FromSqlInterpolated` (které nahrazují `FromSql`) zadat pouze v kořenech dotazu, tj. přímo na `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="2be1f-306">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="2be1f-307">Pokud se pokusíte zadat jiné místo jinak, dojde k chybě kompilace.</span><span class="sxs-lookup"><span data-stu-id="2be1f-307">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="2be1f-308">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-308">**Why**</span></span>

<span data-ttu-id="2be1f-309">Určení `FromSql` kdekoli jinde než na `DbSet` neobsahovalo žádný význam nebo přidaná hodnota a v některých scénářích může způsobit nejednoznačnost.</span><span class="sxs-lookup"><span data-stu-id="2be1f-309">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="2be1f-310">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-310">**Mitigations**</span></span>

<span data-ttu-id="2be1f-311">volání `FromSql` by se měla přesunout přímo na `DbSet`, na které se vztahují.</span><span class="sxs-lookup"><span data-stu-id="2be1f-311">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="2be1f-312">Žádné dotazy pro sledování neprovádějí překlad identity</span><span class="sxs-lookup"><span data-stu-id="2be1f-312">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="2be1f-313">Sledování problému #13518</span><span class="sxs-lookup"><span data-stu-id="2be1f-313">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="2be1f-314">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-314">**Old behavior**</span></span>

<span data-ttu-id="2be1f-315">Před EF Core 3,0 se stejná instance entity používá pro všechny výskyty entity se zadaným typem a ID.</span><span class="sxs-lookup"><span data-stu-id="2be1f-315">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="2be1f-316">To odpovídá chování sledovacích dotazů.</span><span class="sxs-lookup"><span data-stu-id="2be1f-316">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="2be1f-317">Například tento dotaz:</span><span class="sxs-lookup"><span data-stu-id="2be1f-317">For example, this query:</span></span>

```csharp
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="2be1f-318">vrátí stejnou instanci `Category` pro každý `Product` přidruženou k dané kategorii.</span><span class="sxs-lookup"><span data-stu-id="2be1f-318">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="2be1f-319">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-319">**New behavior**</span></span>

<span data-ttu-id="2be1f-320">Počínaje EF Core 3,0 budou vytvořeny různé instance entit při výskytu entity se zadaným typem a ID na různých místech vráceného grafu.</span><span class="sxs-lookup"><span data-stu-id="2be1f-320">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="2be1f-321">Například dotaz výše bude nyní vracet novou instanci `Category` pro každý `Product`, i když jsou ke stejné kategorii přidruženy dva produkty.</span><span class="sxs-lookup"><span data-stu-id="2be1f-321">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="2be1f-322">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-322">**Why**</span></span>

<span data-ttu-id="2be1f-323">Překlad identity (to znamená, že určení, že entita má stejný typ a ID jako dříve zjištěná entita) přidává další výkon a režii paměti.</span><span class="sxs-lookup"><span data-stu-id="2be1f-323">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="2be1f-324">Obvykle se spustí čítač, aby se v prvním místě nepoužily žádné dotazy na sledování.</span><span class="sxs-lookup"><span data-stu-id="2be1f-324">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="2be1f-325">I když může být v některých případech užitečný překlad identity, není potřeba, pokud se entity mají serializovat a odeslat klientovi, což je běžné pro žádné dotazy pro sledování.</span><span class="sxs-lookup"><span data-stu-id="2be1f-325">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="2be1f-326">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-326">**Mitigations**</span></span>

<span data-ttu-id="2be1f-327">Pokud je vyžadováno rozlišení identity, použijte dotaz sledování.</span><span class="sxs-lookup"><span data-stu-id="2be1f-327">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="2be1f-328">~~Provádění dotazu se protokoluje na úrovni ladění~~ . Vrátit</span><span class="sxs-lookup"><span data-stu-id="2be1f-328">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="2be1f-329">Sledování problému #14523</span><span class="sxs-lookup"><span data-stu-id="2be1f-329">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="2be1f-330">Tuto změnu jsme vrátili, protože nová konfigurace v EF Core 3,0 umožňuje, aby byla úroveň protokolu pro libovolnou událost specifikována aplikací.</span><span class="sxs-lookup"><span data-stu-id="2be1f-330">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="2be1f-331">Chcete-li například přepnout protokolování SQL na `Debug`, explicitně nakonfigurujte úroveň v `OnConfiguring` nebo `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="2be1f-331">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="2be1f-332">Dočasné hodnoty klíčů už nejsou nastavené na instance entit.</span><span class="sxs-lookup"><span data-stu-id="2be1f-332">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="2be1f-333">Sledování problému #12378</span><span class="sxs-lookup"><span data-stu-id="2be1f-333">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="2be1f-334">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-334">**Old behavior**</span></span>

<span data-ttu-id="2be1f-335">Před EF Core 3,0 byly přiřazeny dočasné hodnoty ke všem klíčovým vlastnostem, které by později měly skutečnou hodnotu vygenerovanou databází.</span><span class="sxs-lookup"><span data-stu-id="2be1f-335">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="2be1f-336">Obvykle jsou tyto dočasné hodnoty velkými zápornými čísly.</span><span class="sxs-lookup"><span data-stu-id="2be1f-336">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="2be1f-337">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-337">**New behavior**</span></span>

<span data-ttu-id="2be1f-338">Počínaje 3,0 EF Core ukládá hodnotu dočasného klíče jako součást informací o sledování entity a ponechá vlastnost klíče beze změny.</span><span class="sxs-lookup"><span data-stu-id="2be1f-338">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="2be1f-339">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-339">**Why**</span></span>

<span data-ttu-id="2be1f-340">Tato změna byla provedena, aby se předešlo omylům hodnotám klíčů v případě, že entita, která byla dříve sledována instancí `DbContext`, je přesunuta do jiné instance `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="2be1f-340">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="2be1f-341">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-341">**Mitigations**</span></span>

<span data-ttu-id="2be1f-342">Aplikace, které přiřazují hodnoty primárního klíče k cizím klíčům k vytvoření přidružení mezi entitami, můžou záviset na starém chování, pokud jsou primární klíče generované úložištěm a patří do entit ve stavu `Added`.</span><span class="sxs-lookup"><span data-stu-id="2be1f-342">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="2be1f-343">K tomu je možné se vyhnout:</span><span class="sxs-lookup"><span data-stu-id="2be1f-343">This can be avoided by:</span></span>
* <span data-ttu-id="2be1f-344">Nepoužívejte klíče generované úložištěm.</span><span class="sxs-lookup"><span data-stu-id="2be1f-344">Not using store-generated keys.</span></span>
* <span data-ttu-id="2be1f-345">Nastavení vlastností navigace pro vytváření relací místo nastavení hodnot cizích klíčů.</span><span class="sxs-lookup"><span data-stu-id="2be1f-345">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="2be1f-346">Získá aktuální dočasné hodnoty klíče z informací o sledování entity.</span><span class="sxs-lookup"><span data-stu-id="2be1f-346">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="2be1f-347">Například `context.Entry(blog).Property(e => e.Id).CurrentValue` vrátí dočasnou hodnotu, i když `blog.Id` sama nebyla nastavena.</span><span class="sxs-lookup"><span data-stu-id="2be1f-347">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="2be1f-348">DetectChanges respektuje hodnoty klíčů generované úložištěm.</span><span class="sxs-lookup"><span data-stu-id="2be1f-348">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="2be1f-349">Sledování problému #14616</span><span class="sxs-lookup"><span data-stu-id="2be1f-349">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="2be1f-350">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-350">**Old behavior**</span></span>

<span data-ttu-id="2be1f-351">Před EF Core 3,0 se Nesledovaná entita, kterou najde `DetectChanges`, sledovala ve stavu `Added` a vložila se jako nový řádek při volání `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="2be1f-351">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="2be1f-352">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-352">**New behavior**</span></span>

<span data-ttu-id="2be1f-353">Od EF Core 3,0 platí, že pokud entita používá vygenerované hodnoty klíčů a je nastavená nějaká hodnota klíče, bude se entita sledovat ve stavu `Modified`.</span><span class="sxs-lookup"><span data-stu-id="2be1f-353">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="2be1f-354">To znamená, že se předpokládá, že řádek pro entitu existuje a že se bude aktualizovat při volání `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="2be1f-354">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="2be1f-355">Pokud hodnota klíče není nastavená, nebo pokud typ entity nepoužívá vygenerované klíče, bude nová entita dál sledována jako `Added` jako v předchozích verzích.</span><span class="sxs-lookup"><span data-stu-id="2be1f-355">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="2be1f-356">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-356">**Why**</span></span>

<span data-ttu-id="2be1f-357">Tato změna byla provedena tak, aby byla pro práci s nepřipojenými grafy entit při použití klíčů generovaných úložištěm snazší.</span><span class="sxs-lookup"><span data-stu-id="2be1f-357">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="2be1f-358">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-358">**Mitigations**</span></span>

<span data-ttu-id="2be1f-359">Tato změna může přerušit aplikaci, pokud je typ entity nakonfigurovaný na používání vygenerovaných klíčů, ale hodnoty klíčů jsou explicitně nastavené pro nové instance.</span><span class="sxs-lookup"><span data-stu-id="2be1f-359">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="2be1f-360">Opravou je explicitně nakonfigurovat klíčové vlastnosti tak, aby nepoužívaly vygenerované hodnoty.</span><span class="sxs-lookup"><span data-stu-id="2be1f-360">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="2be1f-361">Například s rozhraním API Fluent:</span><span class="sxs-lookup"><span data-stu-id="2be1f-361">For example, with the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="2be1f-362">Nebo s poznámkami k datům:</span><span class="sxs-lookup"><span data-stu-id="2be1f-362">Or with data annotations:</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="2be1f-363">Odstranění kaskádových operací se teď ve výchozím nastavení provádí hned.</span><span class="sxs-lookup"><span data-stu-id="2be1f-363">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="2be1f-364">Sledování problému #10114</span><span class="sxs-lookup"><span data-stu-id="2be1f-364">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="2be1f-365">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-365">**Old behavior**</span></span>

<span data-ttu-id="2be1f-366">Před 3,0 EF Core aplikovány kaskádové akce (odstraňování závislých entit, když se odstraní požadovaný objekt zabezpečení nebo když je vztah k požadovanému objektu zabezpečení vážně) nevznikl, dokud není voláno SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="2be1f-366">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="2be1f-367">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-367">**New behavior**</span></span>

<span data-ttu-id="2be1f-368">Od 3,0 EF Core aplikuje kaskádové akce hned po zjištění spouštěcí podmínky.</span><span class="sxs-lookup"><span data-stu-id="2be1f-368">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="2be1f-369">Například volání `context.Remove()` k odstranění objektu zabezpečení bude mít za následek, že jsou všechny sledované související závislé položky nastaveny také pro `Deleted` hned.</span><span class="sxs-lookup"><span data-stu-id="2be1f-369">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="2be1f-370">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-370">**Why**</span></span>

<span data-ttu-id="2be1f-371">Tato změna byla provedena za účelem zlepšení prostředí pro scénáře vytváření datových vazeb a auditování, kde je důležité pochopit, které entity budou odstraněny _před_ zavoláním `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="2be1f-371">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="2be1f-372">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-372">**Mitigations**</span></span>

<span data-ttu-id="2be1f-373">Předchozí chování lze obnovit pomocí nastavení na `context.ChangeTracker`.</span><span class="sxs-lookup"><span data-stu-id="2be1f-373">The previous behavior can be restored through settings on `context.ChangeTracker`.</span></span>
<span data-ttu-id="2be1f-374">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2be1f-374">For example:</span></span>

```csharp
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="eager-loading-single-query"></a>
### <a name="eager-loading-of-related-entities-now-happens-in-a-single-query"></a><span data-ttu-id="2be1f-375">Eager načítání souvisejících entit se teď děje v jednom dotazu.</span><span class="sxs-lookup"><span data-stu-id="2be1f-375">Eager loading of related entities now happens in a single query</span></span>

[<span data-ttu-id="2be1f-376">Sledování problému #18022</span><span class="sxs-lookup"><span data-stu-id="2be1f-376">Tracking issue #18022</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/18022)

<span data-ttu-id="2be1f-377">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-377">**Old behavior**</span></span>

<span data-ttu-id="2be1f-378">Před 3,0 eagerly načítání navigace kolekce prostřednictvím operátorů `Include` způsobilo, že se v relační databázi generují vícenásobné dotazy, jednu pro každý typ související entity.</span><span class="sxs-lookup"><span data-stu-id="2be1f-378">Before 3.0, eagerly loading collection navigations via `Include` operators caused multiple queries to be generated on relational database, one for each related entity type.</span></span>

<span data-ttu-id="2be1f-379">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-379">**New behavior**</span></span>

<span data-ttu-id="2be1f-380">Počínaje 3,0 EF Core generuje jediný dotaz s spojeními relačních databází.</span><span class="sxs-lookup"><span data-stu-id="2be1f-380">Starting with 3.0, EF Core generates a single query with JOINs on relational databases.</span></span>

<span data-ttu-id="2be1f-381">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-381">**Why**</span></span>

<span data-ttu-id="2be1f-382">Vydání více dotazů pro implementaci jednoho dotazu LINQ způsobilo velký počet problémů, včetně negativního výkonu, protože bylo nutné použít více databázových převodů, a problémy s integritou dat v případě, že každý dotaz může sledovat jiný stav databáze.</span><span class="sxs-lookup"><span data-stu-id="2be1f-382">Issuing multiple queries to implement a single LINQ query caused numerous issues, including negative performance as multiple database roundtrips were necessary, and data coherency issues as each query could observe a different state of the database.</span></span>

<span data-ttu-id="2be1f-383">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-383">**Mitigations**</span></span>

<span data-ttu-id="2be1f-384">I když se jedná o zásadní změnu, může to mít výrazný vliv na výkon aplikace, když jeden dotaz obsahuje velký počet `Include`ch operátorů v navigaci kolekcí.</span><span class="sxs-lookup"><span data-stu-id="2be1f-384">While technically this is not a breaking change, it could have a considerable effect on application performance when a single query contains a large number of `Include` operator on collection navigations.</span></span> <span data-ttu-id="2be1f-385">[Podívejte se na tento komentář](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) , kde najdete další informace a rychlé psaní dotazů.</span><span class="sxs-lookup"><span data-stu-id="2be1f-385">[See this comment](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) for more information and for rewriting queries in a more efficient way.</span></span>

**

<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="2be1f-386">DeleteBehavior. restrict má sémantiku čištění.</span><span class="sxs-lookup"><span data-stu-id="2be1f-386">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="2be1f-387">Sledování problému #12661</span><span class="sxs-lookup"><span data-stu-id="2be1f-387">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="2be1f-388">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-388">**Old behavior**</span></span>

<span data-ttu-id="2be1f-389">Před 3,0 `DeleteBehavior.Restrict` vytvořili cizí klíče v databázi se sémantikou `Restrict`, ale zároveň jsme změnili interní opravu nezřetelným způsobem.</span><span class="sxs-lookup"><span data-stu-id="2be1f-389">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="2be1f-390">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-390">**New behavior**</span></span>

<span data-ttu-id="2be1f-391">Počínaje 3,0 `DeleteBehavior.Restrict` zajišťuje, aby se cizí klíče vytvořily s `Restrict` sémantikou – tedy bez kaskády. porušení omezení throw – bez vlivu na interní opravu EF</span><span class="sxs-lookup"><span data-stu-id="2be1f-391">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="2be1f-392">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-392">**Why**</span></span>

<span data-ttu-id="2be1f-393">Tato změna byla provedena pro zlepšení prostředí pro použití `DeleteBehavior` intuitivním způsobem bez neočekávaných vedlejších účinků.</span><span class="sxs-lookup"><span data-stu-id="2be1f-393">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="2be1f-394">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-394">**Mitigations**</span></span>

<span data-ttu-id="2be1f-395">Předchozí chování lze obnovit pomocí `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="2be1f-395">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="2be1f-396">Typy dotazů jsou konsolidovány s typy entit</span><span class="sxs-lookup"><span data-stu-id="2be1f-396">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="2be1f-397">Sledování problému #14194</span><span class="sxs-lookup"><span data-stu-id="2be1f-397">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="2be1f-398">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-398">**Old behavior**</span></span>

<span data-ttu-id="2be1f-399">Před EF Core 3,0 byly [typy dotazů](xref:core/modeling/keyless-entity-types) prostředkem pro dotazování na data, která nedefinují primární klíč strukturovaným způsobem.</span><span class="sxs-lookup"><span data-stu-id="2be1f-399">Before EF Core 3.0, [query types](xref:core/modeling/keyless-entity-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="2be1f-400">To znamená, že typ dotazu byl použit pro mapování typů entit bez klíčů (pravděpodobnější z pohledu, ale také z tabulky), zatímco byl použit regulární typ entity, když byl klíč k dispozici (pravděpodobnější z tabulky, ale případně z pohledu).</span><span class="sxs-lookup"><span data-stu-id="2be1f-400">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="2be1f-401">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-401">**New behavior**</span></span>

<span data-ttu-id="2be1f-402">Typ dotazu se teď bude jednat jenom o typ entity bez primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="2be1f-402">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="2be1f-403">Typy entit bez klíčů mají stejné funkce jako typy dotazů v předchozích verzích.</span><span class="sxs-lookup"><span data-stu-id="2be1f-403">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="2be1f-404">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-404">**Why**</span></span>

<span data-ttu-id="2be1f-405">Tato změna byla provedena proto, aby se snížila nejasnost v souvislosti s typy dotazů.</span><span class="sxs-lookup"><span data-stu-id="2be1f-405">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="2be1f-406">Konkrétně jsou bez klíčů typy entit a jsou ze své podstaty jen pro čtení, ale neměly by být použity pouze proto, že typ entity musí být jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="2be1f-406">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="2be1f-407">Podobně jsou často mapovány na zobrazení, ale to je pouze v případě, že zobrazení často nedefinují klíče.</span><span class="sxs-lookup"><span data-stu-id="2be1f-407">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="2be1f-408">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-408">**Mitigations**</span></span>

<span data-ttu-id="2be1f-409">Následující části rozhraní API jsou teď zastaralé:</span><span class="sxs-lookup"><span data-stu-id="2be1f-409">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="2be1f-410">**`ModelBuilder.Query<>()`** – místo toho je nutné volat `ModelBuilder.Entity<>().HasNoKey()` pro označení typu entity, protože neobsahují žádné klíče.</span><span class="sxs-lookup"><span data-stu-id="2be1f-410">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="2be1f-411">Tato služba by se ještě nenakonfigurovala pomocí konvence, aby nedocházelo k chybám konfigurace, když je primární klíč očekávaný, ale neodpovídá konvenci.</span><span class="sxs-lookup"><span data-stu-id="2be1f-411">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="2be1f-412">**`DbQuery<>`** – místo toho by měla být použita `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="2be1f-412">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="2be1f-413">**`DbContext.Query<>()`** – místo toho by měla být použita `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="2be1f-413">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="2be1f-414">Změnilo se konfigurační rozhraní API pro vztahy vlastněných typů.</span><span class="sxs-lookup"><span data-stu-id="2be1f-414">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="2be1f-415">[Sledování problému #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[sledování problému #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[sledování problému #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="2be1f-415">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="2be1f-416">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-416">**Old behavior**</span></span>

<span data-ttu-id="2be1f-417">Před EF Core 3,0 byla konfigurace vztahu vlastníka provedena přímo po volání `OwnsOne` nebo `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="2be1f-417">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="2be1f-418">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-418">**New behavior**</span></span>

<span data-ttu-id="2be1f-419">Od EF Core 3,0 teď existuje Fluent API pro konfiguraci navigační vlastnosti pro vlastníka pomocí `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="2be1f-419">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="2be1f-420">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2be1f-420">For example:</span></span>

```csharp
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="2be1f-421">Konfigurace vztahující se k vztahu mezi vlastníkem a vlastníkem by nyní měla být zřetězena po `WithOwner()` podobným způsobem, jakým jsou konfigurovány jiné vztahy.</span><span class="sxs-lookup"><span data-stu-id="2be1f-421">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="2be1f-422">I když se konfigurace samotného vlastního typu bude i nadále zřetězit po `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="2be1f-422">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="2be1f-423">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2be1f-423">For example:</span></span>

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

<span data-ttu-id="2be1f-424">Kromě toho, že volání `Entity()`, `HasOne()`nebo `Set()` s cílem vlastněného typu teď vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="2be1f-424">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="2be1f-425">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-425">**Why**</span></span>

<span data-ttu-id="2be1f-426">Tato změna byla provedena za účelem vytvoření čisticího oddělení mezi konfigurací samotného typu a _vztahu k_ typu, který je vlastníkem.</span><span class="sxs-lookup"><span data-stu-id="2be1f-426">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="2be1f-427">Tím se zase odeberou nejednoznačnosti a nejasnosti u metod, jako je `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="2be1f-427">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="2be1f-428">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-428">**Mitigations**</span></span>

<span data-ttu-id="2be1f-429">Změňte konfiguraci vztahů vlastněných typů tak, aby používala novou plochu rozhraní API, jak je znázorněno v předchozím příkladu.</span><span class="sxs-lookup"><span data-stu-id="2be1f-429">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="2be1f-430">Závislé entity, které sdílí tabulku s objektem zabezpečení, jsou teď volitelné.</span><span class="sxs-lookup"><span data-stu-id="2be1f-430">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="2be1f-431">Sledování problému #9005</span><span class="sxs-lookup"><span data-stu-id="2be1f-431">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="2be1f-432">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-432">**Old behavior**</span></span>

<span data-ttu-id="2be1f-433">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="2be1f-433">Consider the following model:</span></span>
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
<span data-ttu-id="2be1f-434">Před EF Core 3,0 platí, že pokud `OrderDetails` vlastní `Order` nebo explicitně namapované na stejnou tabulku, při přidávání nového `Order`se vždycky vyžadovala instance `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="2be1f-434">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="2be1f-435">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-435">**New behavior**</span></span>

<span data-ttu-id="2be1f-436">Počínaje 3,0 EF Core umožňuje přidat `Order` bez `OrderDetails` a provede mapování všech `OrderDetails`ch vlastností s výjimkou primárního klíče na sloupce s možnou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="2be1f-436">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="2be1f-437">Při dotazování EF Core sady `OrderDetails` na `null`, pokud některá z jejích požadovaných vlastností nemá hodnotu, nebo pokud se kromě primárního klíče nevyžadují žádné vlastnosti, a `null`.</span><span class="sxs-lookup"><span data-stu-id="2be1f-437">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="2be1f-438">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-438">**Mitigations**</span></span>

<span data-ttu-id="2be1f-439">Pokud má váš model sdílení tabulky závislé na všech volitelných sloupcích, ale navigace ukazující na něj se neočekává `null` pak by aplikace měla být upravena tak, aby zpracovávala případy, kdy je navigace `null`.</span><span class="sxs-lookup"><span data-stu-id="2be1f-439">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="2be1f-440">Pokud to není možné, měla by být do typu entity přidána požadovaná vlastnost, nebo alespoň jedna vlastnost musí mít přiřazenou jinou ne`null` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="2be1f-440">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="2be1f-441">Všechny entity sdílející tabulku se sloupcem souběžného tokenu musí být namapovány na vlastnost.</span><span class="sxs-lookup"><span data-stu-id="2be1f-441">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="2be1f-442">Sledování problému #14154</span><span class="sxs-lookup"><span data-stu-id="2be1f-442">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="2be1f-443">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-443">**Old behavior**</span></span>

<span data-ttu-id="2be1f-444">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="2be1f-444">Consider the following model:</span></span>
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
<span data-ttu-id="2be1f-445">Před EF Core 3,0 platí, že pokud `OrderDetails` vlastní `Order` nebo explicitně namapované na stejnou tabulku, aktualizace pouze `OrderDetails` nebude aktualizovat `Version` hodnotu v klientovi a další aktualizace nebude úspěšná.</span><span class="sxs-lookup"><span data-stu-id="2be1f-445">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="2be1f-446">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-446">**New behavior**</span></span>

<span data-ttu-id="2be1f-447">Počínaje 3,0 EF Core rozšíří novou hodnotu `Version` na `Order`, pokud vlastní `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="2be1f-447">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="2be1f-448">V opačném případě je při ověřování modelu vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="2be1f-448">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="2be1f-449">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-449">**Why**</span></span>

<span data-ttu-id="2be1f-450">Tato změna byla provedena, aby nedocházelo k zastaralé hodnotě tokenu souběžnosti, pokud je aktualizována pouze jedna z entit mapovaných na stejnou tabulku.</span><span class="sxs-lookup"><span data-stu-id="2be1f-450">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="2be1f-451">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-451">**Mitigations**</span></span>

<span data-ttu-id="2be1f-452">Všechny entity, které sdílejí tabulku, musí obsahovat vlastnost, která je namapovaná na sloupec tokenu souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="2be1f-452">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="2be1f-453">Je možné, že ho vytvoříte ve stínovém stavu:</span><span class="sxs-lookup"><span data-stu-id="2be1f-453">It's possible the create one in shadow-state:</span></span>
```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="owned-query"></a>

### <a name="owned-entities-cannot-be-queried-without-the-owner-using-a-tracking-query"></a><span data-ttu-id="2be1f-454">Na vlastněné entity se nedá dotazovat bez vlastníka pomocí sledovacího dotazu.</span><span class="sxs-lookup"><span data-stu-id="2be1f-454">Owned entities cannot be queried without the owner using a tracking query</span></span>

[<span data-ttu-id="2be1f-455">Sledování problému #18876</span><span class="sxs-lookup"><span data-stu-id="2be1f-455">Tracking Issue #18876</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/18876)

<span data-ttu-id="2be1f-456">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-456">**Old behavior**</span></span>

<span data-ttu-id="2be1f-457">Před EF Core 3,0 se jako jakákoli jiná navigace mohla zadat dotaz na vlastněné entity.</span><span class="sxs-lookup"><span data-stu-id="2be1f-457">Before EF Core 3.0, the owned entities could be queried as any other navigation.</span></span>

```csharp
context.People.Select(p => p.Address);
```

<span data-ttu-id="2be1f-458">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-458">**New behavior**</span></span>

<span data-ttu-id="2be1f-459">Počínaje 3,0 EF Core vyvolá výjimku, pokud dotaz sledování provede vlastní entitu bez vlastníka.</span><span class="sxs-lookup"><span data-stu-id="2be1f-459">Starting with 3.0, EF Core will throw if a tracking query projects an owned entity without the owner.</span></span>

<span data-ttu-id="2be1f-460">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-460">**Why**</span></span>

<span data-ttu-id="2be1f-461">Vlastněné entity nemůžou manipulovat bez vlastníka, takže v převážné většině případů se na ně dotazuje tímto způsobem je chyba.</span><span class="sxs-lookup"><span data-stu-id="2be1f-461">Owned entities cannot be manipulated without the owner, so in the vast majority of cases querying them in this way is an error.</span></span>

<span data-ttu-id="2be1f-462">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-462">**Mitigations**</span></span>

<span data-ttu-id="2be1f-463">Pokud by měla být vlastní entita sledována tak, aby se změnila jakýmkoli způsobem, měl by být vlastník zahrnut v dotazu.</span><span class="sxs-lookup"><span data-stu-id="2be1f-463">If the owned entity should be tracked to be modified in any way later then the owner should be included in the query.</span></span>

<span data-ttu-id="2be1f-464">V opačném případě přidejte `AsNoTracking()` volání:</span><span class="sxs-lookup"><span data-stu-id="2be1f-464">Otherwise add an `AsNoTracking()` call:</span></span>

```csharp
context.People.Select(p => p.Address).AsNoTracking();
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="2be1f-465">Zděděné vlastnosti z nemapovaných typů jsou nyní namapovány na jeden sloupec pro všechny odvozené typy.</span><span class="sxs-lookup"><span data-stu-id="2be1f-465">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="2be1f-466">Sledování problému #13998</span><span class="sxs-lookup"><span data-stu-id="2be1f-466">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="2be1f-467">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-467">**Old behavior**</span></span>

<span data-ttu-id="2be1f-468">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="2be1f-468">Consider the following model:</span></span>
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

<span data-ttu-id="2be1f-469">Před EF Core 3,0 bude vlastnost `ShippingAddress` namapována na samostatné sloupce pro `BulkOrder` a `Order` ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="2be1f-469">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="2be1f-470">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-470">**New behavior**</span></span>

<span data-ttu-id="2be1f-471">Počínaje 3,0 EF Core pro `ShippingAddress`vytvoří pouze jeden sloupec.</span><span class="sxs-lookup"><span data-stu-id="2be1f-471">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="2be1f-472">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-472">**Why**</span></span>

<span data-ttu-id="2be1f-473">Starý behavoir nebyl očekáván.</span><span class="sxs-lookup"><span data-stu-id="2be1f-473">The old behavoir was unexpected.</span></span>

<span data-ttu-id="2be1f-474">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-474">**Mitigations**</span></span>

<span data-ttu-id="2be1f-475">Vlastnost může být stále explicitně namapována na samostatný sloupec odvozených typů:</span><span class="sxs-lookup"><span data-stu-id="2be1f-475">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="2be1f-476">Konvence vlastností cizího klíče už neodpovídá stejnému názvu jako vlastnost Principal.</span><span class="sxs-lookup"><span data-stu-id="2be1f-476">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="2be1f-477">Sledování problému #13274</span><span class="sxs-lookup"><span data-stu-id="2be1f-477">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="2be1f-478">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-478">**Old behavior**</span></span>

<span data-ttu-id="2be1f-479">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="2be1f-479">Consider the following model:</span></span>
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
<span data-ttu-id="2be1f-480">Před EF Core 3,0 se pro cizí klíč podle konvence použije vlastnost `CustomerId`.</span><span class="sxs-lookup"><span data-stu-id="2be1f-480">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="2be1f-481">Pokud je však `Order` vlastněný typ, pak by to mělo také `CustomerId` primární klíč a to není obvykle očekávané.</span><span class="sxs-lookup"><span data-stu-id="2be1f-481">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="2be1f-482">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-482">**New behavior**</span></span>

<span data-ttu-id="2be1f-483">Počínaje 3,0 se EF Core nepokouší použít vlastnosti pro cizí klíče podle konvence, pokud mají stejný název jako vlastnost Principal.</span><span class="sxs-lookup"><span data-stu-id="2be1f-483">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="2be1f-484">Název objektu zabezpečení zřetězený s názvem vlastnosti objektu zabezpečení a název navigace zřetězený s vzory názvů hlavních vlastností se pořád shodují.</span><span class="sxs-lookup"><span data-stu-id="2be1f-484">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="2be1f-485">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2be1f-485">For example:</span></span>

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

<span data-ttu-id="2be1f-486">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-486">**Why**</span></span>

<span data-ttu-id="2be1f-487">Tato změna byla provedena, aby nedocházelo k chybnému definování vlastností primárního klíče u vlastněných typů.</span><span class="sxs-lookup"><span data-stu-id="2be1f-487">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="2be1f-488">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-488">**Mitigations**</span></span>

<span data-ttu-id="2be1f-489">Pokud by vlastnost měla být cizí klíč, a proto je součástí primárního klíče, pak ji explicitně nakonfigurujte jako takovou.</span><span class="sxs-lookup"><span data-stu-id="2be1f-489">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="2be1f-490">Připojení k databázi je teď zavřené, pokud už nepoužíváte dřív, než se dokončí jeho objekt TransactionScope.</span><span class="sxs-lookup"><span data-stu-id="2be1f-490">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="2be1f-491">Sledování problému #14218</span><span class="sxs-lookup"><span data-stu-id="2be1f-491">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="2be1f-492">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-492">**Old behavior**</span></span>

<span data-ttu-id="2be1f-493">Pokud se během EF Core 3,0 v rámci kontextu otevře připojení v `TransactionScope`, zůstane připojení otevřené, zatímco je aktivní aktuální `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="2be1f-493">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="2be1f-494">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-494">**New behavior**</span></span>

<span data-ttu-id="2be1f-495">Od 3,0 EF Core ukončí připojení, jakmile ho dokončí jeho používání.</span><span class="sxs-lookup"><span data-stu-id="2be1f-495">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="2be1f-496">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-496">**Why**</span></span>

<span data-ttu-id="2be1f-497">Tato změna umožňuje použít více kontextů ve stejném `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="2be1f-497">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="2be1f-498">Nové chování se také shoduje s EF6.</span><span class="sxs-lookup"><span data-stu-id="2be1f-498">The new behavior also matches EF6.</span></span>

<span data-ttu-id="2be1f-499">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-499">**Mitigations**</span></span>

<span data-ttu-id="2be1f-500">Pokud připojení potřebuje zůstat otevřeným explicitním voláním `OpenConnection()`, zajistíte, že EF Core nebude předčasně ukončen:</span><span class="sxs-lookup"><span data-stu-id="2be1f-500">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="2be1f-501">Každá vlastnost používá nezávislou generaci celočíselného klíče v paměti.</span><span class="sxs-lookup"><span data-stu-id="2be1f-501">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="2be1f-502">Sledování problému #6872</span><span class="sxs-lookup"><span data-stu-id="2be1f-502">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="2be1f-503">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-503">**Old behavior**</span></span>

<span data-ttu-id="2be1f-504">Před EF Core 3,0 byl pro všechny vlastnosti celého čísla klíče v paměti použit jeden generátor sdílených hodnot.</span><span class="sxs-lookup"><span data-stu-id="2be1f-504">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="2be1f-505">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-505">**New behavior**</span></span>

<span data-ttu-id="2be1f-506">Počínaje EF Core 3,0 každé klíčové vlastnosti celého čísla získá vlastní generátor hodnot při použití databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="2be1f-506">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="2be1f-507">Také pokud je databáze odstraněna, generování klíče se obnoví pro všechny tabulky.</span><span class="sxs-lookup"><span data-stu-id="2be1f-507">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="2be1f-508">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-508">**Why**</span></span>

<span data-ttu-id="2be1f-509">Tato změna byla provedená tak, aby se při vytváření klíčů v paměti lépe rovnala generování klíčů v paměti a vylepšila se možnost izolovat testy od sebe při použití databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="2be1f-509">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="2be1f-510">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-510">**Mitigations**</span></span>

<span data-ttu-id="2be1f-511">To může poškodit aplikaci, která se spoléhá na konkrétní hodnoty klíče v paměti, které se mají nastavit.</span><span class="sxs-lookup"><span data-stu-id="2be1f-511">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="2be1f-512">Místo toho se nemusíte spoléhat na konkrétní hodnoty klíčů nebo aktualizovat tak, aby odpovídaly novému chování.</span><span class="sxs-lookup"><span data-stu-id="2be1f-512">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="2be1f-513">Ve výchozím nastavení se používají pole pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="2be1f-513">Backing fields are used by default</span></span>

[<span data-ttu-id="2be1f-514">Sledování problému #12430</span><span class="sxs-lookup"><span data-stu-id="2be1f-514">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="2be1f-515">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-515">**Old behavior**</span></span>

<span data-ttu-id="2be1f-516">Před 3,0, a to i v případě, že je známé pole zálohování pro vlastnost, EF Core ve výchozím nastavení přečetl a zapsat hodnotu vlastnosti pomocí metod getter a setter vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2be1f-516">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="2be1f-517">Výjimkou je provedení dotazu, kde by se pole zálohování nastavilo přímo, pokud je známé.</span><span class="sxs-lookup"><span data-stu-id="2be1f-517">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="2be1f-518">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-518">**New behavior**</span></span>

<span data-ttu-id="2be1f-519">Počínaje EF Core 3,0 platí, že pokud je známé pole zálohování pro vlastnost, pak EF Core tuto vlastnost vždy přečte a zapíše pomocí pole pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="2be1f-519">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="2be1f-520">To může způsobit přerušení aplikace, pokud se aplikace spoléhá na další chování kódované v metodách getter nebo setter.</span><span class="sxs-lookup"><span data-stu-id="2be1f-520">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="2be1f-521">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-521">**Why**</span></span>

<span data-ttu-id="2be1f-522">Tato změna byla provedena EF Core proto, aby při provádění databázových operací, které obsahují entity, ve výchozím nastavení nechybně aktivovala obchodní logiku.</span><span class="sxs-lookup"><span data-stu-id="2be1f-522">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="2be1f-523">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-523">**Mitigations**</span></span>

<span data-ttu-id="2be1f-524">Chování před 3,0 se dá obnovit pomocí konfigurace režimu přístupu vlastnosti na `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="2be1f-524">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="2be1f-525">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2be1f-525">For example:</span></span>

```csharp
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="2be1f-526">Vyvolat, zda je nalezeno více kompatibilních zálohovaných polí</span><span class="sxs-lookup"><span data-stu-id="2be1f-526">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="2be1f-527">Sledování problému #12523</span><span class="sxs-lookup"><span data-stu-id="2be1f-527">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="2be1f-528">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-528">**Old behavior**</span></span>

<span data-ttu-id="2be1f-529">Pokud se v EF Core 3,0 shodovalo více polí s pravidly pro hledání zálohovacího pole vlastnosti, bude jedno pole zvoleno na základě pořadí priorit.</span><span class="sxs-lookup"><span data-stu-id="2be1f-529">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="2be1f-530">To může způsobit, že se nesprávné pole použije v nejednoznačných případech.</span><span class="sxs-lookup"><span data-stu-id="2be1f-530">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="2be1f-531">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-531">**New behavior**</span></span>

<span data-ttu-id="2be1f-532">Počínaje EF Core 3,0 platí, že pokud je více polí spárováno se stejnou vlastností, je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="2be1f-532">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="2be1f-533">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-533">**Why**</span></span>

<span data-ttu-id="2be1f-534">Tato změna byla provedena, aby nedocházelo k tichému použití jednoho pole v případě, že může být pouze jedna z nich správná.</span><span class="sxs-lookup"><span data-stu-id="2be1f-534">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="2be1f-535">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-535">**Mitigations**</span></span>

<span data-ttu-id="2be1f-536">Vlastnosti s nejednoznačnými zálohovacími poli musí obsahovat pole, které se má explicitně použít.</span><span class="sxs-lookup"><span data-stu-id="2be1f-536">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="2be1f-537">Například pomocí rozhraní Fluent API:</span><span class="sxs-lookup"><span data-stu-id="2be1f-537">For example, using the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="2be1f-538">Názvy vlastností pouze pro pole se musí shodovat s názvem pole.</span><span class="sxs-lookup"><span data-stu-id="2be1f-538">Field-only property names should match the field name</span></span>

<span data-ttu-id="2be1f-539">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-539">**Old behavior**</span></span>

<span data-ttu-id="2be1f-540">Před EF Core 3,0 může být vlastnost určena hodnotou řetězce a v případě, že v typu .NET nebyla nalezena žádná vlastnost s tímto názvem, EF Core by se pokusila ji porovnat s polem pomocí pravidel konvence.</span><span class="sxs-lookup"><span data-stu-id="2be1f-540">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the .NET type then EF Core would try to match it to a field using convention rules.</span></span>

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

<span data-ttu-id="2be1f-541">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-541">**New behavior**</span></span>

<span data-ttu-id="2be1f-542">Počínaje EF Core 3,0 se vlastnost pouze pole musí přesně shodovat s názvem pole.</span><span class="sxs-lookup"><span data-stu-id="2be1f-542">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="2be1f-543">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-543">**Why**</span></span>

<span data-ttu-id="2be1f-544">Tato změna byla provedena, aby se nepoužívalo stejné pole pro dvě vlastnosti s názvem podobně, ale také pravidla pro porovnání vlastností pouze pro pole, která jsou shodná s vlastnostmi mapovanými na vlastnosti CLR.</span><span class="sxs-lookup"><span data-stu-id="2be1f-544">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="2be1f-545">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-545">**Mitigations**</span></span>

<span data-ttu-id="2be1f-546">Vlastnosti pouze polí musí být pojmenovány stejně jako pole, na které jsou namapována.</span><span class="sxs-lookup"><span data-stu-id="2be1f-546">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="2be1f-547">V budoucí verzi EF Core po 3,0 plánujeme znovu povolit explicitní konfiguraci názvu pole, který se liší od názvu vlastnosti (viz téma věnované problému [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span><span class="sxs-lookup"><span data-stu-id="2be1f-547">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="2be1f-548">AddDbContext/AddDbContextPool už nevolá AddLogging a AddMemoryCache.</span><span class="sxs-lookup"><span data-stu-id="2be1f-548">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="2be1f-549">Sledování problému #14756</span><span class="sxs-lookup"><span data-stu-id="2be1f-549">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="2be1f-550">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-550">**Old behavior**</span></span>

<span data-ttu-id="2be1f-551">Před EF Core 3,0 by volání `AddDbContext` nebo `AddDbContextPool` také registrovalo protokolování a služby ukládání do mezipaměti v paměti s DI prostřednictvím volání [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) a [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="2be1f-551">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with DI through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="2be1f-552">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-552">**New behavior**</span></span>

<span data-ttu-id="2be1f-553">Počínaje EF Core 3,0 se `AddDbContext` a `AddDbContextPool` nebudou nadále registrovat tyto služby se vkládáním závislostí (DI).</span><span class="sxs-lookup"><span data-stu-id="2be1f-553">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="2be1f-554">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-554">**Why**</span></span>

<span data-ttu-id="2be1f-555">EF Core 3,0 nevyžaduje, aby se tyto služby nacházejí v kontejneru aplikace DI.</span><span class="sxs-lookup"><span data-stu-id="2be1f-555">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="2be1f-556">Pokud je však v kontejneru aplikace `ILoggerFactory` zaregistrován, bude nadále používána EF Core.</span><span class="sxs-lookup"><span data-stu-id="2be1f-556">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="2be1f-557">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-557">**Mitigations**</span></span>

<span data-ttu-id="2be1f-558">Pokud vaše aplikace potřebuje tyto služby, zaregistrujte je explicitně pomocí kontejneru DI pomocí [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) nebo [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="2be1f-558">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

### <a name="addentityframework-adds-imemorycache-with-a-size-limit"></a><span data-ttu-id="2be1f-559">AddEntityFramework \* přidá IMemoryCache s omezením velikosti.</span><span class="sxs-lookup"><span data-stu-id="2be1f-559">AddEntityFramework\* adds IMemoryCache with a size limit</span></span>

[<span data-ttu-id="2be1f-560">Sledování problému #12905</span><span class="sxs-lookup"><span data-stu-id="2be1f-560">Tracking Issue #12905</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12905)

<span data-ttu-id="2be1f-561">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-561">**Old behavior**</span></span>

<span data-ttu-id="2be1f-562">Před EF Core 3,0 by volání `AddEntityFramework*`ch metod také registrovalo služby ukládání do mezipaměti paměti s DI bez omezení velikosti.</span><span class="sxs-lookup"><span data-stu-id="2be1f-562">Before EF Core 3.0, calling `AddEntityFramework*` methods would also register memory caching services with DI without a size limit.</span></span>

<span data-ttu-id="2be1f-563">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-563">**New behavior**</span></span>

<span data-ttu-id="2be1f-564">Počínaje EF Core 3,0 `AddEntityFramework*` zaregistruje službu IMemoryCache s omezením velikosti.</span><span class="sxs-lookup"><span data-stu-id="2be1f-564">Starting with EF Core 3.0, `AddEntityFramework*` will register an IMemoryCache service with a size limit.</span></span> <span data-ttu-id="2be1f-565">Pokud se jakékoli jiné služby přidané následně závisejí na IMemoryCache, můžou rychle dosáhnout výchozího limitu, který způsobuje výjimky nebo snížení výkonu.</span><span class="sxs-lookup"><span data-stu-id="2be1f-565">If any other services added afterwards depend on IMemoryCache they can quickly reach the default limit causing exceptions or degraded performance.</span></span>

<span data-ttu-id="2be1f-566">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-566">**Why**</span></span>

<span data-ttu-id="2be1f-567">Použití IMemoryCache bez omezení by mohlo vést k neřízenému využití paměti, pokud dojde k chybě v logice mezipaměti dotazů nebo když jsou dotazy generovány dynamicky.</span><span class="sxs-lookup"><span data-stu-id="2be1f-567">Using IMemoryCache without a limit could result in uncontrolled memory usage if there is a bug in query caching logic or the queries are generated dynamically.</span></span> <span data-ttu-id="2be1f-568">Výchozí omezení snižuje potenciální útok DoS.</span><span class="sxs-lookup"><span data-stu-id="2be1f-568">Having a default limit mitigates a potential DoS attack.</span></span>

<span data-ttu-id="2be1f-569">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-569">**Mitigations**</span></span>

<span data-ttu-id="2be1f-570">Ve většině případů volání `AddEntityFramework*` není nutné, pokud se volá také `AddDbContext` nebo `AddDbContextPool`.</span><span class="sxs-lookup"><span data-stu-id="2be1f-570">In most cases calling `AddEntityFramework*` is not necessary if `AddDbContext` or `AddDbContextPool` is called as well.</span></span> <span data-ttu-id="2be1f-571">Z toho důvodu je nejlepší zmírnit odebrání `AddEntityFramework*` volání.</span><span class="sxs-lookup"><span data-stu-id="2be1f-571">Therefore, the best mitigation is to remove the `AddEntityFramework*` call.</span></span>

<span data-ttu-id="2be1f-572">Pokud vaše aplikace potřebuje tyto služby, zaregistrujte implementaci IMemoryCache explicitně pomocí kontejneru DI předem pomocí [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="2be1f-572">If your application needs these services, then register a IMemoryCache implementation explicitly with the DI container beforehand using [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="2be1f-573">DbContext. entry teď provádí místní DetectChanges.</span><span class="sxs-lookup"><span data-stu-id="2be1f-573">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="2be1f-574">Sledování problému #13552</span><span class="sxs-lookup"><span data-stu-id="2be1f-574">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="2be1f-575">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-575">**Old behavior**</span></span>

<span data-ttu-id="2be1f-576">Před EF Core 3,0 by volání `DbContext.Entry` způsobilo zjištění změn všech sledovaných entit.</span><span class="sxs-lookup"><span data-stu-id="2be1f-576">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="2be1f-577">Tím je zajištěno, že stav zpřístupněný v `EntityEntry` byl aktuální.</span><span class="sxs-lookup"><span data-stu-id="2be1f-577">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="2be1f-578">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-578">**New behavior**</span></span>

<span data-ttu-id="2be1f-579">Počínaje EF Core 3,0 se volání `DbContext.Entry` nyní pokusí zjistit pouze změny v dané entitě a všechny sledované hlavní entity, které se k ní vztahují.</span><span class="sxs-lookup"><span data-stu-id="2be1f-579">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="2be1f-580">To znamená, že změny jinde nemohly být zjištěny voláním této metody, což by mohlo mít vliv na stav aplikace.</span><span class="sxs-lookup"><span data-stu-id="2be1f-580">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="2be1f-581">Všimněte si, že pokud je `ChangeTracker.AutoDetectChangesEnabled` nastaveno na `false` pak i toto místní zjišťování změn bude zakázáno.</span><span class="sxs-lookup"><span data-stu-id="2be1f-581">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="2be1f-582">Jiné metody, které způsobují detekci změn – například `ChangeTracker.Entries` a `SaveChanges`, stále způsobují kompletní `DetectChanges` všech sledovaných entit.</span><span class="sxs-lookup"><span data-stu-id="2be1f-582">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="2be1f-583">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-583">**Why**</span></span>

<span data-ttu-id="2be1f-584">Tato změna byla provedena za účelem zlepšení výchozího výkonu použití `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="2be1f-584">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="2be1f-585">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-585">**Mitigations**</span></span>

<span data-ttu-id="2be1f-586">Před voláním `Entry` zajistěte, aby bylo zajištěno chování před 3,0m voláním `ChangeTracker.DetectChanges()` explicitně.</span><span class="sxs-lookup"><span data-stu-id="2be1f-586">Call `ChangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="2be1f-587">Klíče řetězce a pole bajtů nejsou ve výchozím nastavení generovány klientem.</span><span class="sxs-lookup"><span data-stu-id="2be1f-587">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="2be1f-588">Sledování problému #14617</span><span class="sxs-lookup"><span data-stu-id="2be1f-588">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="2be1f-589">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-589">**Old behavior**</span></span>

<span data-ttu-id="2be1f-590">Před EF Core 3,0 lze použít `string` a `byte[]` vlastnosti klíče, aniž byste museli explicitně nastavit hodnotu, která není null.</span><span class="sxs-lookup"><span data-stu-id="2be1f-590">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="2be1f-591">V takovém případě se hodnota klíče vygeneruje na klientovi jako identifikátor GUID serializovaný na bajty pro `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="2be1f-591">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="2be1f-592">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-592">**New behavior**</span></span>

<span data-ttu-id="2be1f-593">Počínaje EF Core 3,0 bude vyvolána výjimka oznamující, že nebyla nastavena žádná hodnota klíče.</span><span class="sxs-lookup"><span data-stu-id="2be1f-593">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="2be1f-594">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-594">**Why**</span></span>

<span data-ttu-id="2be1f-595">Tato změna byla provedena, protože /`byte[]` hodnoty `string`generované klientem nejsou všeobecně užitečné a výchozí chování způsobilo, že je obtížné vygenerovat hodnoty klíčů běžným způsobem.</span><span class="sxs-lookup"><span data-stu-id="2be1f-595">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="2be1f-596">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-596">**Mitigations**</span></span>

<span data-ttu-id="2be1f-597">Chování před 3,0 lze získat explicitním určením, že klíčové vlastnosti by měly používat generované hodnoty, pokud není nastavena žádná jiná hodnota, která není null.</span><span class="sxs-lookup"><span data-stu-id="2be1f-597">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="2be1f-598">Například s rozhraním API Fluent:</span><span class="sxs-lookup"><span data-stu-id="2be1f-598">For example, with the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="2be1f-599">Nebo s poznámkami k datům:</span><span class="sxs-lookup"><span data-stu-id="2be1f-599">Or with data annotations:</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="2be1f-600">ILoggerFactory je teď služba s vymezeným oborem.</span><span class="sxs-lookup"><span data-stu-id="2be1f-600">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="2be1f-601">Sledování problému #14698</span><span class="sxs-lookup"><span data-stu-id="2be1f-601">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="2be1f-602">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-602">**Old behavior**</span></span>

<span data-ttu-id="2be1f-603">Před EF Core 3,0 byl `ILoggerFactory` zaregistrován jako služba s jedním prvkem.</span><span class="sxs-lookup"><span data-stu-id="2be1f-603">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="2be1f-604">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-604">**New behavior**</span></span>

<span data-ttu-id="2be1f-605">Počínaje EF Core 3,0 se `ILoggerFactory` nyní zaregistroval jako vymezený obor.</span><span class="sxs-lookup"><span data-stu-id="2be1f-605">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="2be1f-606">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-606">**Why**</span></span>

<span data-ttu-id="2be1f-607">Tato změna byla provedena, aby bylo možné povolit přidružení protokolovacího nástroje k instanci `DbContext`, která umožňuje další funkce a odebírá některé případy patologického chování, jako je například rozbalení interních poskytovatelů služeb.</span><span class="sxs-lookup"><span data-stu-id="2be1f-607">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="2be1f-608">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-608">**Mitigations**</span></span>

<span data-ttu-id="2be1f-609">Tato změna by neměla mít vliv na kód aplikace, pokud se neregistruje a nepoužívá vlastní služby pro EF Core interního poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="2be1f-609">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="2be1f-610">To není běžné.</span><span class="sxs-lookup"><span data-stu-id="2be1f-610">This isn't common.</span></span>
<span data-ttu-id="2be1f-611">V těchto případech bude většina věcí pořád fungovat, ale jakákoliv služba typu Singleton, která byla v závislosti na `ILoggerFactory`, musí být změněna tak, aby získala `ILoggerFactory` jiným způsobem.</span><span class="sxs-lookup"><span data-stu-id="2be1f-611">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="2be1f-612">Pokud narazíte na takové situace, zajistěte prosím problém na [EF Core sledování problémů GitHubu](https://github.com/aspnet/EntityFrameworkCore/issues) , abychom nás věděli, jak používáte `ILoggerFactory` tak, abychom mohli lépe porozumět tomu, jak toto řešení v budoucnu zrušit.</span><span class="sxs-lookup"><span data-stu-id="2be1f-612">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="2be1f-613">Opožděné načítání proxy serverů už nepředpokládá navigační vlastnosti, jsou plně načtené.</span><span class="sxs-lookup"><span data-stu-id="2be1f-613">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="2be1f-614">Sledování problému #12780</span><span class="sxs-lookup"><span data-stu-id="2be1f-614">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="2be1f-615">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-615">**Old behavior**</span></span>

<span data-ttu-id="2be1f-616">Před EF Core 3,0 byla po zrušení `DbContext` nijak nevěděla, zda byla daná vlastnost navigace u entity získané z tohoto kontextu plně načtena nebo nikoli.</span><span class="sxs-lookup"><span data-stu-id="2be1f-616">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="2be1f-617">Proxy místo toho předpokládají, že je načtena odkazová navigace, pokud má hodnotu jinou než null a že je načtena navigace kolekce, pokud není prázdná.</span><span class="sxs-lookup"><span data-stu-id="2be1f-617">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="2be1f-618">V těchto případech by byl pokus o opožděné načtení no-op.</span><span class="sxs-lookup"><span data-stu-id="2be1f-618">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="2be1f-619">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-619">**New behavior**</span></span>

<span data-ttu-id="2be1f-620">Počínaje EF Core 3,0 budou proxy servery sledovat, zda je načtena vlastnost navigace.</span><span class="sxs-lookup"><span data-stu-id="2be1f-620">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="2be1f-621">To znamená, že při pokusu o přístup k navigační vlastnosti, která je načtena po vyřazení kontextu, bude vždy ta no-op, i když je načtená navigace prázdná nebo má hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="2be1f-621">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="2be1f-622">Naopak při pokusu o přístup k navigační vlastnosti, která není načtená, vyvolá výjimku, pokud je kontext vyřazený, i když je vlastnost navigace neprázdná kolekce.</span><span class="sxs-lookup"><span data-stu-id="2be1f-622">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="2be1f-623">Pokud nastane tato situace, znamená to, že se kód aplikace pokouší použít opožděné načítání v neplatném čase a aplikace by se měla změnit, aby to nevedlo.</span><span class="sxs-lookup"><span data-stu-id="2be1f-623">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="2be1f-624">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-624">**Why**</span></span>

<span data-ttu-id="2be1f-625">Tato změna byla provedena, aby při pokusu o opožděné načtení na uvolněnou instanci `DbContext` bylo chování konzistentní a správné.</span><span class="sxs-lookup"><span data-stu-id="2be1f-625">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="2be1f-626">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-626">**Mitigations**</span></span>

<span data-ttu-id="2be1f-627">Aktualizujte kód aplikace, aby se nepokoušel opožděné načtení s odstraněným kontextem, nebo nastavte tuto hodnotu jako No-op, jak je popsáno ve zprávě výjimky.</span><span class="sxs-lookup"><span data-stu-id="2be1f-627">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="2be1f-628">Nadměrné vytváření interních zprostředkovatelů služeb je teď ve výchozím nastavení chyba.</span><span class="sxs-lookup"><span data-stu-id="2be1f-628">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="2be1f-629">Sledování problému #10236</span><span class="sxs-lookup"><span data-stu-id="2be1f-629">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="2be1f-630">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-630">**Old behavior**</span></span>

<span data-ttu-id="2be1f-631">Před EF Core 3,0 se pro aplikaci, která vytváří patologický počet interních poskytovatelů služeb, zaprotokoluje upozornění.</span><span class="sxs-lookup"><span data-stu-id="2be1f-631">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="2be1f-632">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-632">**New behavior**</span></span>

<span data-ttu-id="2be1f-633">Počínaje EF Core 3,0 je toto upozornění nyní považováno za chybu a je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="2be1f-633">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="2be1f-634">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-634">**Why**</span></span>

<span data-ttu-id="2be1f-635">Tato změna byla provedená tak, že se tento patologický případ výslovně zveřejňuje tak, aby se zlepšil kód aplikace.</span><span class="sxs-lookup"><span data-stu-id="2be1f-635">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="2be1f-636">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-636">**Mitigations**</span></span>

<span data-ttu-id="2be1f-637">Nejvhodnější příčinou této chyby je pochopení hlavní příčiny a zastavení vytváření, takže mnoho interních poskytovatelů služeb.</span><span class="sxs-lookup"><span data-stu-id="2be1f-637">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="2be1f-638">Chybu však lze převést zpět na varování (nebo ignorováno) prostřednictvím konfigurace na `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="2be1f-638">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="2be1f-639">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2be1f-639">For example:</span></span>

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="2be1f-640">Nové chování pro HasOne/HasMany se volá s jedním řetězcem.</span><span class="sxs-lookup"><span data-stu-id="2be1f-640">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="2be1f-641">Sledování problému #9171</span><span class="sxs-lookup"><span data-stu-id="2be1f-641">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="2be1f-642">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-642">**Old behavior**</span></span>

<span data-ttu-id="2be1f-643">Před EF Core 3,0 byl kód volání `HasOne` nebo `HasMany` s jedním řetězcem interpretován jako matoucí způsob.</span><span class="sxs-lookup"><span data-stu-id="2be1f-643">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="2be1f-644">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2be1f-644">For example:</span></span>
```csharp
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="2be1f-645">Kód vypadá jako v souvislosti s `Samurai` pro jiný typ entity pomocí navigační vlastnosti `Entrance`, která může být soukromá.</span><span class="sxs-lookup"><span data-stu-id="2be1f-645">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="2be1f-646">Ve skutečnosti se tento kód pokusí vytvořit relaci k některému typu entity s názvem `Entrance` bez vlastnosti navigace.</span><span class="sxs-lookup"><span data-stu-id="2be1f-646">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="2be1f-647">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-647">**New behavior**</span></span>

<span data-ttu-id="2be1f-648">Počínaje EF Core 3,0 výše uvedený kód má nyní podobný vzhled jako předtím.</span><span class="sxs-lookup"><span data-stu-id="2be1f-648">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="2be1f-649">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-649">**Why**</span></span>

<span data-ttu-id="2be1f-650">Staré chování bylo velmi matoucí, zejména při čtení konfiguračního kódu a hledání chyb.</span><span class="sxs-lookup"><span data-stu-id="2be1f-650">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="2be1f-651">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-651">**Mitigations**</span></span>

<span data-ttu-id="2be1f-652">Tím dojde pouze k přerušení aplikací, které jsou explicitně konfigurovány pomocí řetězců pro názvy typů, a bez explicitního určení vlastnosti navigace.</span><span class="sxs-lookup"><span data-stu-id="2be1f-652">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="2be1f-653">To není běžné.</span><span class="sxs-lookup"><span data-stu-id="2be1f-653">This is not common.</span></span>
<span data-ttu-id="2be1f-654">Předchozí chování se dá získat pomocí explicitního předání `null` názvu vlastnosti navigace.</span><span class="sxs-lookup"><span data-stu-id="2be1f-654">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="2be1f-655">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2be1f-655">For example:</span></span>

```csharp
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="2be1f-656">Návratový typ pro několik asynchronních metod byl změněn z úlohy na ValueTask</span><span class="sxs-lookup"><span data-stu-id="2be1f-656">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="2be1f-657">Sledování problému #15184</span><span class="sxs-lookup"><span data-stu-id="2be1f-657">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="2be1f-658">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-658">**Old behavior**</span></span>

<span data-ttu-id="2be1f-659">Následující asynchronní metody dříve vrátily `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="2be1f-659">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="2be1f-660">`ValueGenerator.NextValueAsync()` (a odvozování tříd)</span><span class="sxs-lookup"><span data-stu-id="2be1f-660">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="2be1f-661">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-661">**New behavior**</span></span>

<span data-ttu-id="2be1f-662">Výše uvedené metody nyní vrací `ValueTask<T>` přes stejný `T` jako předtím.</span><span class="sxs-lookup"><span data-stu-id="2be1f-662">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="2be1f-663">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-663">**Why**</span></span>

<span data-ttu-id="2be1f-664">Tato změna snižuje počet přidělení haldy, které vznikly při vyvolání těchto metod, což zlepšuje obecný výkon.</span><span class="sxs-lookup"><span data-stu-id="2be1f-664">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="2be1f-665">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-665">**Mitigations**</span></span>

<span data-ttu-id="2be1f-666">Aplikace jednoduše čekají na rozhraní API, které je třeba znovu zkompilovat – nejsou nutné žádné změny zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="2be1f-666">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="2be1f-667">Složitější využití (například předání vrácených `Task` do `Task.WhenAny()`) obvykle vyžaduje, aby vrácený `ValueTask<T>` byl převeden na `Task<T>` voláním `AsTask()`.</span><span class="sxs-lookup"><span data-stu-id="2be1f-667">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="2be1f-668">Všimněte si, že se tím sníží omezení přidělení, které tato změna přináší.</span><span class="sxs-lookup"><span data-stu-id="2be1f-668">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="2be1f-669">Relační: anotace TypeMapping je nyní pouze TypeMapping</span><span class="sxs-lookup"><span data-stu-id="2be1f-669">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="2be1f-670">Sledování problému #9913</span><span class="sxs-lookup"><span data-stu-id="2be1f-670">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="2be1f-671">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-671">**Old behavior**</span></span>

<span data-ttu-id="2be1f-672">Název poznámky pro poznámky k mapování typů byl "relační: TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="2be1f-672">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="2be1f-673">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-673">**New behavior**</span></span>

<span data-ttu-id="2be1f-674">Název poznámky pro mapování typů je nyní "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="2be1f-674">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="2be1f-675">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-675">**Why**</span></span>

<span data-ttu-id="2be1f-676">Mapování typů se nyní používají pro více než stejné poskytovatele relačních databází.</span><span class="sxs-lookup"><span data-stu-id="2be1f-676">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="2be1f-677">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-677">**Mitigations**</span></span>

<span data-ttu-id="2be1f-678">Tím dojde pouze k přerušení aplikací, které přistupují k mapování typu přímo jako anotaci, což není běžné.</span><span class="sxs-lookup"><span data-stu-id="2be1f-678">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="2be1f-679">Nejvhodnější akcí pro opravu je použití prostoru rozhraní API pro přístup k mapování typů namísto použití anotace přímo.</span><span class="sxs-lookup"><span data-stu-id="2be1f-679">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="2be1f-680">ToTable na odvozeném typu vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="2be1f-680">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="2be1f-681">Sledování problému #11811</span><span class="sxs-lookup"><span data-stu-id="2be1f-681">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="2be1f-682">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-682">**Old behavior**</span></span>

<span data-ttu-id="2be1f-683">Před EF Core 3,0 by byl `ToTable()` volaný pro odvozený typ ignorován, protože pouze dědičnost dědičnosti mapování je typu TPH, který není platný.</span><span class="sxs-lookup"><span data-stu-id="2be1f-683">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="2be1f-684">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-684">**New behavior**</span></span>

<span data-ttu-id="2be1f-685">Počínaje EF Core 3,0 a při přípravě na přidání podpory TPT a TPC v novější verzi nyní `ToTable()` volána na odvozeném typu vyvolá výjimku, aby nedošlo k neočekávané změně mapování v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="2be1f-685">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="2be1f-686">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-686">**Why**</span></span>

<span data-ttu-id="2be1f-687">V současné době není platný pro mapování odvozeného typu na jinou tabulku.</span><span class="sxs-lookup"><span data-stu-id="2be1f-687">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="2be1f-688">Tato změna zabrání v budoucnosti v budoucnu, pokud se to stalo platným.</span><span class="sxs-lookup"><span data-stu-id="2be1f-688">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="2be1f-689">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-689">**Mitigations**</span></span>

<span data-ttu-id="2be1f-690">Odeberte všechny pokusy o mapování odvozených typů na jiné tabulky.</span><span class="sxs-lookup"><span data-stu-id="2be1f-690">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="2be1f-691">ForSqlServerHasIndex nahrazeno HasIndex</span><span class="sxs-lookup"><span data-stu-id="2be1f-691">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="2be1f-692">Sledování problému #12366</span><span class="sxs-lookup"><span data-stu-id="2be1f-692">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="2be1f-693">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-693">**Old behavior**</span></span>

<span data-ttu-id="2be1f-694">Před EF Core 3,0 `ForSqlServerHasIndex().ForSqlServerInclude()` poskytnout způsob, jak nakonfigurovat sloupce používané `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="2be1f-694">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="2be1f-695">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-695">**New behavior**</span></span>

<span data-ttu-id="2be1f-696">Počínaje EF Core 3,0 se teď na relační úrovni podporuje použití `Include` na indexu.</span><span class="sxs-lookup"><span data-stu-id="2be1f-696">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="2be1f-697">Použijte `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="2be1f-697">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="2be1f-698">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-698">**Why**</span></span>

<span data-ttu-id="2be1f-699">Tato změna byla provedena za účelem konsolidace rozhraní API pro indexy s `Include` na jednom místě pro všechny poskytovatele databáze.</span><span class="sxs-lookup"><span data-stu-id="2be1f-699">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="2be1f-700">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-700">**Mitigations**</span></span>

<span data-ttu-id="2be1f-701">Použijte nové rozhraní API, jak vidíte výše.</span><span class="sxs-lookup"><span data-stu-id="2be1f-701">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="2be1f-702">Změny rozhraní API pro metadata</span><span class="sxs-lookup"><span data-stu-id="2be1f-702">Metadata API changes</span></span>

[<span data-ttu-id="2be1f-703">Sledování problému #214</span><span class="sxs-lookup"><span data-stu-id="2be1f-703">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="2be1f-704">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-704">**New behavior**</span></span>

<span data-ttu-id="2be1f-705">Následující vlastnosti byly převedeny na rozšiřující metody:</span><span class="sxs-lookup"><span data-stu-id="2be1f-705">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="2be1f-706">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-706">**Why**</span></span>

<span data-ttu-id="2be1f-707">Tato změna zjednodušuje implementaci výše uvedených rozhraní.</span><span class="sxs-lookup"><span data-stu-id="2be1f-707">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="2be1f-708">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-708">**Mitigations**</span></span>

<span data-ttu-id="2be1f-709">Použijte nové metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="2be1f-709">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="2be1f-710">Změny rozhraní API pro konkrétního zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="2be1f-710">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="2be1f-711">Sledování problému #214</span><span class="sxs-lookup"><span data-stu-id="2be1f-711">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="2be1f-712">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-712">**New behavior**</span></span>

<span data-ttu-id="2be1f-713">Metody rozšíření specifické pro poskytovatele budou shrnuty:</span><span class="sxs-lookup"><span data-stu-id="2be1f-713">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="2be1f-714">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-714">**Why**</span></span>

<span data-ttu-id="2be1f-715">Tato změna zjednodušuje implementaci výše uvedených rozšiřujících metod.</span><span class="sxs-lookup"><span data-stu-id="2be1f-715">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="2be1f-716">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-716">**Mitigations**</span></span>

<span data-ttu-id="2be1f-717">Použijte nové metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="2be1f-717">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="2be1f-718">EF Core už neposílá direktivu pragma pro vynucení KOFK SQLite.</span><span class="sxs-lookup"><span data-stu-id="2be1f-718">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="2be1f-719">Sledování problému #12151</span><span class="sxs-lookup"><span data-stu-id="2be1f-719">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="2be1f-720">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-720">**Old behavior**</span></span>

<span data-ttu-id="2be1f-721">Před EF Core 3,0 by EF Core při otevření připojení k SQLite odeslal `PRAGMA foreign_keys = 1`.</span><span class="sxs-lookup"><span data-stu-id="2be1f-721">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="2be1f-722">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-722">**New behavior**</span></span>

<span data-ttu-id="2be1f-723">Počínaje EF Core 3,0 EF Core již neposílá `PRAGMA foreign_keys = 1` při otevření připojení k SQLite.</span><span class="sxs-lookup"><span data-stu-id="2be1f-723">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="2be1f-724">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-724">**Why**</span></span>

<span data-ttu-id="2be1f-725">Tato změna byla provedena, protože ve výchozím nastavení používá EF Core `SQLitePCLRaw.bundle_e_sqlite3`, což zase znamená, že je ve výchozím nastavení zapnuté vynucení CK a není nutné je explicitně povolit při každém otevření připojení.</span><span class="sxs-lookup"><span data-stu-id="2be1f-725">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="2be1f-726">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-726">**Mitigations**</span></span>

<span data-ttu-id="2be1f-727">Cizí klíče jsou ve výchozím nastavení povolené v SQLitePCLRaw. bundle_e_sqlite3, která se ve výchozím nastavení používá pro EF Core.</span><span class="sxs-lookup"><span data-stu-id="2be1f-727">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="2be1f-728">V ostatních případech je možné povolit cizí klíče zadáním `Foreign Keys=True` v připojovacím řetězci.</span><span class="sxs-lookup"><span data-stu-id="2be1f-728">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="2be1f-729">Microsoft. EntityFrameworkCore. sqlite teď závisí na SQLitePCLRaw. bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="2be1f-729">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="2be1f-730">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-730">**Old behavior**</span></span>

<span data-ttu-id="2be1f-731">Před EF Core 3,0 EF Core použito `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="2be1f-731">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="2be1f-732">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-732">**New behavior**</span></span>

<span data-ttu-id="2be1f-733">Počínaje EF Core 3,0 EF Core používá `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="2be1f-733">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="2be1f-734">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-734">**Why**</span></span>

<span data-ttu-id="2be1f-735">Tato změna byla provedena tak, že verze SQLiteu použitá v iOS je konzistentní s jinými platformami.</span><span class="sxs-lookup"><span data-stu-id="2be1f-735">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="2be1f-736">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-736">**Mitigations**</span></span>

<span data-ttu-id="2be1f-737">Pokud chcete použít nativní verzi SQLite v iOS, nakonfigurujte `Microsoft.Data.Sqlite` tak, aby používala jiný svazek `SQLitePCLRaw`.</span><span class="sxs-lookup"><span data-stu-id="2be1f-737">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="2be1f-738">Hodnoty GUID se teď ukládají jako TEXT na SQLite.</span><span class="sxs-lookup"><span data-stu-id="2be1f-738">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="2be1f-739">Sledování problému #15078</span><span class="sxs-lookup"><span data-stu-id="2be1f-739">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="2be1f-740">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-740">**Old behavior**</span></span>

<span data-ttu-id="2be1f-741">Hodnoty GUID byly dříve uloženy jako hodnoty objektů BLOB u SQLite.</span><span class="sxs-lookup"><span data-stu-id="2be1f-741">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="2be1f-742">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-742">**New behavior**</span></span>

<span data-ttu-id="2be1f-743">Hodnoty GUID jsou nyní uloženy jako TEXT.</span><span class="sxs-lookup"><span data-stu-id="2be1f-743">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="2be1f-744">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-744">**Why**</span></span>

<span data-ttu-id="2be1f-745">Binární formát identifikátorů GUID není standardizovaný.</span><span class="sxs-lookup"><span data-stu-id="2be1f-745">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="2be1f-746">Uložení hodnot jako textu zajistí, že databáze bude lépe kompatibilní s jinými technologiemi.</span><span class="sxs-lookup"><span data-stu-id="2be1f-746">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="2be1f-747">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-747">**Mitigations**</span></span>

<span data-ttu-id="2be1f-748">Existující databáze můžete migrovat do nového formátu tím, že spustíte příkaz SQL podobně jako následující.</span><span class="sxs-lookup"><span data-stu-id="2be1f-748">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="2be1f-749">V EF Core můžete i nadále používat předchozí chování nakonfigurováním převaděče hodnot na těchto vlastnostech.</span><span class="sxs-lookup"><span data-stu-id="2be1f-749">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="2be1f-750">Microsoft. data. sqlite zůstává schopný přečítat hodnoty GUID z objektů BLOB a textových sloupců. vzhledem k tomu, že výchozí formát pro parametry a konstanty se změnil, bude pravděpodobně nutné provést akci u většiny scénářů, které zahrnují identifikátory GUID.</span><span class="sxs-lookup"><span data-stu-id="2be1f-750">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="2be1f-751">Hodnoty char se teď ukládají jako TEXT na SQLite.</span><span class="sxs-lookup"><span data-stu-id="2be1f-751">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="2be1f-752">Sledování problému #15020</span><span class="sxs-lookup"><span data-stu-id="2be1f-752">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="2be1f-753">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-753">**Old behavior**</span></span>

<span data-ttu-id="2be1f-754">Hodnoty typu char byly dříve sored jako CELOČÍSELNé hodnoty u SQLite.</span><span class="sxs-lookup"><span data-stu-id="2be1f-754">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="2be1f-755">Například hodnota znaku *a* byla uložena jako celočíselná hodnota 65.</span><span class="sxs-lookup"><span data-stu-id="2be1f-755">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="2be1f-756">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-756">**New behavior**</span></span>

<span data-ttu-id="2be1f-757">Hodnoty typu char jsou nyní uloženy jako TEXT.</span><span class="sxs-lookup"><span data-stu-id="2be1f-757">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="2be1f-758">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-758">**Why**</span></span>

<span data-ttu-id="2be1f-759">Ukládání hodnot jako TEXT je přirozenější a databáze usnadňuje kompatibilitu s jinými technologiemi.</span><span class="sxs-lookup"><span data-stu-id="2be1f-759">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="2be1f-760">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-760">**Mitigations**</span></span>

<span data-ttu-id="2be1f-761">Existující databáze můžete migrovat do nového formátu tím, že spustíte příkaz SQL podobně jako následující.</span><span class="sxs-lookup"><span data-stu-id="2be1f-761">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="2be1f-762">V EF Core můžete i nadále používat předchozí chování nakonfigurováním převaděče hodnot na těchto vlastnostech.</span><span class="sxs-lookup"><span data-stu-id="2be1f-762">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="2be1f-763">Microsoft. data. sqlite také zůstává schopný číst znakové hodnoty z CELOČÍSELNého i TEXTOVÉHO sloupce, takže některé scénáře nemusí vyžadovat žádnou akci.</span><span class="sxs-lookup"><span data-stu-id="2be1f-763">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="2be1f-764">ID migrace se teď generují pomocí kalendáře invariantní jazykové verze.</span><span class="sxs-lookup"><span data-stu-id="2be1f-764">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="2be1f-765">Sledování problému #12978</span><span class="sxs-lookup"><span data-stu-id="2be1f-765">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="2be1f-766">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-766">**Old behavior**</span></span>

<span data-ttu-id="2be1f-767">ID migrace se nechtěně vygenerovala pomocí kalendáře aktuální jazykové verze.</span><span class="sxs-lookup"><span data-stu-id="2be1f-767">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="2be1f-768">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-768">**New behavior**</span></span>

<span data-ttu-id="2be1f-769">ID migrace se nyní vždy generují pomocí kalendáře neutrální jazykové verze (gregoriánský).</span><span class="sxs-lookup"><span data-stu-id="2be1f-769">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="2be1f-770">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-770">**Why**</span></span>

<span data-ttu-id="2be1f-771">Pořadí migrace je důležité při aktualizaci databáze nebo řešení konfliktů při slučování.</span><span class="sxs-lookup"><span data-stu-id="2be1f-771">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="2be1f-772">Pomocí invariantního kalendáře se vyhnete problémům s řazením, které mohou být výsledkem členů týmu jiné systémové kalendáře.</span><span class="sxs-lookup"><span data-stu-id="2be1f-772">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="2be1f-773">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-773">**Mitigations**</span></span>

<span data-ttu-id="2be1f-774">Tato změna má vliv na kohokoli, kdo používá negregoriánský kalendář, ve kterém je rok větší než gregoriánský kalendář (například thajský buddhistický kalendář).</span><span class="sxs-lookup"><span data-stu-id="2be1f-774">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="2be1f-775">Existující identifikátory migrace se budou muset aktualizovat, aby se nové migrace objednaly po stávajících migracích.</span><span class="sxs-lookup"><span data-stu-id="2be1f-775">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="2be1f-776">ID migrace najdete v atributu migrace v souborech návrháře migrace.</span><span class="sxs-lookup"><span data-stu-id="2be1f-776">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="2be1f-777">Také je potřeba aktualizovat tabulku historie migrace.</span><span class="sxs-lookup"><span data-stu-id="2be1f-777">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="2be1f-778">UseRowNumberForPaging se odebral.</span><span class="sxs-lookup"><span data-stu-id="2be1f-778">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="2be1f-779">Sledování problému #16400</span><span class="sxs-lookup"><span data-stu-id="2be1f-779">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="2be1f-780">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-780">**Old behavior**</span></span>

<span data-ttu-id="2be1f-781">Před EF Core 3,0 lze pomocí `UseRowNumberForPaging` vygenerovat SQL pro stránkování, které je kompatibilní s SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="2be1f-781">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="2be1f-782">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-782">**New behavior**</span></span>

<span data-ttu-id="2be1f-783">Počínaje EF Core 3,0 bude EF generovat pouze SQL pro stránkování, které je kompatibilní pouze s novějšími verzemi SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2be1f-783">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="2be1f-784">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-784">**Why**</span></span>

<span data-ttu-id="2be1f-785">Tuto změnu provedeme, protože [SQL Server 2008 už není podporovaným produktem](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) a aktualizace této funkce tak, aby fungovala se změnami dotazů provedenými v EF Core 3,0 je významná práce.</span><span class="sxs-lookup"><span data-stu-id="2be1f-785">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="2be1f-786">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-786">**Mitigations**</span></span>

<span data-ttu-id="2be1f-787">Doporučujeme aktualizovat na novější verzi SQL Server nebo pomocí vyšší úrovně kompatibility, aby byl vygenerovaný SQL podporován.</span><span class="sxs-lookup"><span data-stu-id="2be1f-787">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="2be1f-788">To znamená, že pokud to nemůžete udělat, [komentář k problému s sledováním](https://github.com/aspnet/EntityFrameworkCore/issues/16400) najdete v podrobnostech.</span><span class="sxs-lookup"><span data-stu-id="2be1f-788">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="2be1f-789">Toto rozhodnutí můžeme znovu navštívit na základě zpětné vazby.</span><span class="sxs-lookup"><span data-stu-id="2be1f-789">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="2be1f-790">Informace o rozšíření/metadata se odebraly z IDbContextOptionsExtension.</span><span class="sxs-lookup"><span data-stu-id="2be1f-790">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="2be1f-791">Sledování problému #16119</span><span class="sxs-lookup"><span data-stu-id="2be1f-791">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="2be1f-792">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-792">**Old behavior**</span></span>

<span data-ttu-id="2be1f-793">`IDbContextOptionsExtension` obsahovaly metody pro poskytování metadat o rozšíření.</span><span class="sxs-lookup"><span data-stu-id="2be1f-793">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="2be1f-794">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-794">**New behavior**</span></span>

<span data-ttu-id="2be1f-795">Tyto metody byly přesunuty do nové `DbContextOptionsExtensionInfo` abstraktní základní třídy, která je vrácena z nové vlastnosti `IDbContextOptionsExtension.Info`.</span><span class="sxs-lookup"><span data-stu-id="2be1f-795">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="2be1f-796">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-796">**Why**</span></span>

<span data-ttu-id="2be1f-797">V rámci vydání od 2,0 do 3,0 jsme potřebovali přidat nebo změnit tyto metody několikrát.</span><span class="sxs-lookup"><span data-stu-id="2be1f-797">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="2be1f-798">Rozbalením do nové abstraktní základní třídy bude snazší vytvořit tyto změny bez přerušení stávajících rozšíření.</span><span class="sxs-lookup"><span data-stu-id="2be1f-798">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="2be1f-799">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-799">**Mitigations**</span></span>

<span data-ttu-id="2be1f-800">Aktualizovat rozšíření tak, aby následovala nový vzor.</span><span class="sxs-lookup"><span data-stu-id="2be1f-800">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="2be1f-801">Příklady najdete v mnoha implementacích `IDbContextOptionsExtension` různých druhů rozšíření ve zdrojovém kódu EF Core.</span><span class="sxs-lookup"><span data-stu-id="2be1f-801">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="2be1f-802">LogQueryPossibleExceptionWithAggregateOperator se přejmenovalo.</span><span class="sxs-lookup"><span data-stu-id="2be1f-802">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="2be1f-803">Sledování problému #10985</span><span class="sxs-lookup"><span data-stu-id="2be1f-803">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="2be1f-804">**Mění**</span><span class="sxs-lookup"><span data-stu-id="2be1f-804">**Change**</span></span>

<span data-ttu-id="2be1f-805">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` bylo přejmenováno na `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="2be1f-805">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="2be1f-806">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-806">**Why**</span></span>

<span data-ttu-id="2be1f-807">Zarovná pojmenování této události varování se všemi ostatními událostmi upozornění.</span><span class="sxs-lookup"><span data-stu-id="2be1f-807">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="2be1f-808">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-808">**Mitigations**</span></span>

<span data-ttu-id="2be1f-809">Použijte nový název.</span><span class="sxs-lookup"><span data-stu-id="2be1f-809">Use the new name.</span></span> <span data-ttu-id="2be1f-810">(Všimněte si, že číslo ID události se nezměnilo.)</span><span class="sxs-lookup"><span data-stu-id="2be1f-810">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="2be1f-811">Vysvětlení rozhraní API pro názvy omezení cizího klíče</span><span class="sxs-lookup"><span data-stu-id="2be1f-811">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="2be1f-812">Sledování problému #10730</span><span class="sxs-lookup"><span data-stu-id="2be1f-812">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="2be1f-813">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-813">**Old behavior**</span></span>

<span data-ttu-id="2be1f-814">Před EF Core 3,0 se názvy omezení cizího klíče odkazovaly jenom na "název".</span><span class="sxs-lookup"><span data-stu-id="2be1f-814">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="2be1f-815">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2be1f-815">For example:</span></span>

```csharp
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="2be1f-816">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-816">**New behavior**</span></span>

<span data-ttu-id="2be1f-817">Počínaje EF Core 3,0 se názvy omezení cizích klíčů teď označují jako "název omezení".</span><span class="sxs-lookup"><span data-stu-id="2be1f-817">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="2be1f-818">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2be1f-818">For example:</span></span>

```csharp
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="2be1f-819">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-819">**Why**</span></span>

<span data-ttu-id="2be1f-820">Tato změna přináší konzistenci pro pojmenování v této oblasti a také vysvětluje, že se jedná o název omezení cizího klíče, a nikoli název sloupce nebo vlastnosti, ve kterém je definován cizí klíč.</span><span class="sxs-lookup"><span data-stu-id="2be1f-820">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="2be1f-821">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-821">**Mitigations**</span></span>

<span data-ttu-id="2be1f-822">Použijte nový název.</span><span class="sxs-lookup"><span data-stu-id="2be1f-822">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="2be1f-823">IRelationalDatabaseCreator. HasTables/HasTablesAsync byly zveřejněny.</span><span class="sxs-lookup"><span data-stu-id="2be1f-823">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="2be1f-824">Sledování problému #15997</span><span class="sxs-lookup"><span data-stu-id="2be1f-824">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="2be1f-825">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-825">**Old behavior**</span></span>

<span data-ttu-id="2be1f-826">Před EF Core 3,0 byly tyto metody chráněné.</span><span class="sxs-lookup"><span data-stu-id="2be1f-826">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="2be1f-827">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-827">**New behavior**</span></span>

<span data-ttu-id="2be1f-828">Počínaje EF Core 3,0 jsou tyto metody veřejné.</span><span class="sxs-lookup"><span data-stu-id="2be1f-828">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="2be1f-829">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-829">**Why**</span></span>

<span data-ttu-id="2be1f-830">Tyto metody jsou používány EF k určení, jestli je databáze vytvořená, ale prázdná.</span><span class="sxs-lookup"><span data-stu-id="2be1f-830">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="2be1f-831">To může být užitečné taky od vnějšího EF při určování, jestli se mají migrace použít.</span><span class="sxs-lookup"><span data-stu-id="2be1f-831">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="2be1f-832">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-832">**Mitigations**</span></span>

<span data-ttu-id="2be1f-833">Změňte přístupnost všech přepsání.</span><span class="sxs-lookup"><span data-stu-id="2be1f-833">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="2be1f-834">Microsoft. EntityFrameworkCore. Design je teď balíček DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="2be1f-834">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="2be1f-835">Sledování problému #11506</span><span class="sxs-lookup"><span data-stu-id="2be1f-835">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="2be1f-836">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-836">**Old behavior**</span></span>

<span data-ttu-id="2be1f-837">Před EF Core 3,0 byl Microsoft. EntityFrameworkCore. Design regulárním balíčkem NuGet, na kterém by mohly být na sestavení odkazovány projekty, které na něm závisejí.</span><span class="sxs-lookup"><span data-stu-id="2be1f-837">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="2be1f-838">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-838">**New behavior**</span></span>

<span data-ttu-id="2be1f-839">Počínaje EF Core 3,0 se jedná o balíček DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="2be1f-839">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="2be1f-840">To znamená, že závislost nebude nijak přesměrovat do jiných projektů a že již ve výchozím nastavení nemůžete, aby odkazoval na jeho sestavení.</span><span class="sxs-lookup"><span data-stu-id="2be1f-840">This means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="2be1f-841">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-841">**Why**</span></span>

<span data-ttu-id="2be1f-842">Tento balíček se má použít jenom v době návrhu.</span><span class="sxs-lookup"><span data-stu-id="2be1f-842">This package is only intended to be used at design time.</span></span> <span data-ttu-id="2be1f-843">Nasazené aplikace by neměli na ni odkazovat.</span><span class="sxs-lookup"><span data-stu-id="2be1f-843">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="2be1f-844">Díky tomu, že balíček DevelopmentDependency, toto doporučení posiluje.</span><span class="sxs-lookup"><span data-stu-id="2be1f-844">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="2be1f-845">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-845">**Mitigations**</span></span>

<span data-ttu-id="2be1f-846">Pokud potřebujete odkazovat na tento balíček, aby bylo možné přepsat EF Core chování při návrhu, můžete aktualizovat metadata položky PackageReference v projektu.</span><span class="sxs-lookup"><span data-stu-id="2be1f-846">If you need to reference this package to override EF Core's design-time behavior, then you can update PackageReference item metadata in your project.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<span data-ttu-id="2be1f-847">Pokud se na balíček odkazuje přes Microsoft. EntityFrameworkCore. Tools, budete muset do balíčku přidat explicitní PackageReference, aby se změnila jeho metadata.</span><span class="sxs-lookup"><span data-stu-id="2be1f-847">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span> <span data-ttu-id="2be1f-848">Takový explicitní odkaz musí být přidán do jakéhokoli projektu, kde jsou požadovány typy z balíčku.</span><span class="sxs-lookup"><span data-stu-id="2be1f-848">Such an explicit reference must be added to any project where the types from the package are needed.</span></span>

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="2be1f-849">SQLitePCL. Raw aktualizováno na verzi 2.0.0</span><span class="sxs-lookup"><span data-stu-id="2be1f-849">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="2be1f-850">Sledování problému #14824</span><span class="sxs-lookup"><span data-stu-id="2be1f-850">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="2be1f-851">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-851">**Old behavior**</span></span>

<span data-ttu-id="2be1f-852">Microsoft. EntityFrameworkCore. sqlite byl dřív závislý na 1.1.12 verze SQLitePCL. Raw.</span><span class="sxs-lookup"><span data-stu-id="2be1f-852">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="2be1f-853">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-853">**New behavior**</span></span>

<span data-ttu-id="2be1f-854">Aktualizovali jsme náš balíček tak, aby byl závislý na verzi 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="2be1f-854">We've updated our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="2be1f-855">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-855">**Why**</span></span>

<span data-ttu-id="2be1f-856">2\.0.0 verze SQLitePCL. Raw TARGETS .NET Standard 2,0.</span><span class="sxs-lookup"><span data-stu-id="2be1f-856">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="2be1f-857">Dříve cílí na .NET Standard 1,1, které vyžadovaly, aby se při práci vytvořil velký uzávěr přenosných balíčků.</span><span class="sxs-lookup"><span data-stu-id="2be1f-857">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="2be1f-858">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-858">**Mitigations**</span></span>

<span data-ttu-id="2be1f-859">SQLitePCL. Raw 2.0.0 verze obsahuje některé zásadní změny.</span><span class="sxs-lookup"><span data-stu-id="2be1f-859">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="2be1f-860">Podrobnosti najdete v [poznámkách k verzi](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) .</span><span class="sxs-lookup"><span data-stu-id="2be1f-860">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="2be1f-861">NetTopologySuite aktualizace na verzi 2.0.0</span><span class="sxs-lookup"><span data-stu-id="2be1f-861">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="2be1f-862">Sledování problému #14825</span><span class="sxs-lookup"><span data-stu-id="2be1f-862">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="2be1f-863">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-863">**Old behavior**</span></span>

<span data-ttu-id="2be1f-864">Prostorové balíčky byly dříve závislé na 1.15.1 verze NetTopologySuite.</span><span class="sxs-lookup"><span data-stu-id="2be1f-864">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="2be1f-865">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-865">**New behavior**</span></span>

<span data-ttu-id="2be1f-866">Náš balíček jsme aktualizovali tak, aby byl závislý na verzi 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="2be1f-866">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="2be1f-867">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-867">**Why**</span></span>

<span data-ttu-id="2be1f-868">2\.0.0 verze NetTopologySuite má za cíl řešit několik problémů s použitelností, ke kterým se EF Core uživatelé setkali.</span><span class="sxs-lookup"><span data-stu-id="2be1f-868">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="2be1f-869">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-869">**Mitigations**</span></span>

<span data-ttu-id="2be1f-870">NetTopologySuite verze 2.0.0 obsahuje některé průlomové změny.</span><span class="sxs-lookup"><span data-stu-id="2be1f-870">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="2be1f-871">Podrobnosti najdete v [poznámkách k verzi](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) .</span><span class="sxs-lookup"><span data-stu-id="2be1f-871">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="SqlClient"></a>

### <a name="microsoftdatasqlclient-is-used-instead-of-systemdatasqlclient"></a><span data-ttu-id="2be1f-872">Místo typu System. data. SqlClient se používá Microsoft. data. SqlClient.</span><span class="sxs-lookup"><span data-stu-id="2be1f-872">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>

[<span data-ttu-id="2be1f-873">Sledování problému #15636</span><span class="sxs-lookup"><span data-stu-id="2be1f-873">Tracking Issue #15636</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15636)

<span data-ttu-id="2be1f-874">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-874">**Old behavior**</span></span>

<span data-ttu-id="2be1f-875">Microsoft. EntityFrameworkCore. SqlServer byl dřív závislý na System. data. SqlClient.</span><span class="sxs-lookup"><span data-stu-id="2be1f-875">Microsoft.EntityFrameworkCore.SqlServer previously depended on System.Data.SqlClient.</span></span>

<span data-ttu-id="2be1f-876">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-876">**New behavior**</span></span>

<span data-ttu-id="2be1f-877">Balíček jsme aktualizovali tak, aby byl závislý na Microsoft. data. SqlClient.</span><span class="sxs-lookup"><span data-stu-id="2be1f-877">We've updated our package to depend on Microsoft.Data.SqlClient.</span></span>

<span data-ttu-id="2be1f-878">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-878">**Why**</span></span>

<span data-ttu-id="2be1f-879">Microsoft. data. SqlClient je nejdůležitější ovladač pro přístup k datům, který je k dispozici pro SQL Server a System. data. SqlClient již není zaměřuje na vývoj.</span><span class="sxs-lookup"><span data-stu-id="2be1f-879">Microsoft.Data.SqlClient is the flagship data access driver for SQL Server going forward, and System.Data.SqlClient no longer be the focus of development.</span></span>
<span data-ttu-id="2be1f-880">Některé důležité funkce, například Always Encrypted, jsou k dispozici pouze v Microsoft. data. SqlClient.</span><span class="sxs-lookup"><span data-stu-id="2be1f-880">Some important features, such as Always Encrypted, are only available on Microsoft.Data.SqlClient.</span></span>

<span data-ttu-id="2be1f-881">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-881">**Mitigations**</span></span>

<span data-ttu-id="2be1f-882">Pokud váš kód používá přímou závislost na System. data. SqlClient, musíte ho změnit tak, aby odkazoval na Microsoft. data. SqlClient místo toho. vzhledem k tomu, že oba balíčky udržují velmi vysoký stupeň kompatibility rozhraní API, mělo by to být jenom jednoduchý balíček a Změna oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="2be1f-882">If your code takes a direct dependency on System.Data.SqlClient, you must change it to reference Microsoft.Data.SqlClient instead; as the two packages maintain a very high degree of API compatibility, this should only be a simple package and namespace change.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="2be1f-883">Je nutné nakonfigurovat více dvojznačných relací odkazujících na sebe.</span><span class="sxs-lookup"><span data-stu-id="2be1f-883">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="2be1f-884">Sledování problému #13573</span><span class="sxs-lookup"><span data-stu-id="2be1f-884">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="2be1f-885">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-885">**Old behavior**</span></span>

<span data-ttu-id="2be1f-886">Typ entity s více jednosměrnou navigační vlastností a s vyhovující FKs byl nesprávně nakonfigurován jako jeden vztah.</span><span class="sxs-lookup"><span data-stu-id="2be1f-886">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="2be1f-887">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2be1f-887">For example:</span></span>

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

<span data-ttu-id="2be1f-888">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-888">**New behavior**</span></span>

<span data-ttu-id="2be1f-889">Tento scénář je nyní zjištěn v sestavování modelu a je vyvolána výjimka označující, že je model dvojznačný.</span><span class="sxs-lookup"><span data-stu-id="2be1f-889">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="2be1f-890">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-890">**Why**</span></span>

<span data-ttu-id="2be1f-891">Výsledný model byl nejednoznačný a pravděpodobně bude pro tento případ obvykle špatný.</span><span class="sxs-lookup"><span data-stu-id="2be1f-891">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="2be1f-892">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-892">**Mitigations**</span></span>

<span data-ttu-id="2be1f-893">Použijte úplnou konfiguraci relace.</span><span class="sxs-lookup"><span data-stu-id="2be1f-893">Use full configuration of the relationship.</span></span> <span data-ttu-id="2be1f-894">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2be1f-894">For example:</span></span>

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
### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a><span data-ttu-id="2be1f-895">DbFunction. Schema má hodnotu null nebo je prázdný řetězec, který nakonfiguruje, aby byl ve výchozím schématu modelu.</span><span class="sxs-lookup"><span data-stu-id="2be1f-895">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>

[<span data-ttu-id="2be1f-896">Sledování problému #12757</span><span class="sxs-lookup"><span data-stu-id="2be1f-896">Tracking Issue #12757</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12757)

<span data-ttu-id="2be1f-897">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-897">**Old behavior**</span></span>

<span data-ttu-id="2be1f-898">DbFunction nakonfigurovaný se schématem jako prázdný řetězec byl považován za vestavěnou funkci bez schématu.</span><span class="sxs-lookup"><span data-stu-id="2be1f-898">A DbFunction configured with schema as an empty string was treated as built-in function without a schema.</span></span> <span data-ttu-id="2be1f-899">Například následující kód bude mapován `DatePart` funkci CLR na `DATEPART` vestavěnou funkci na SqlServer.</span><span class="sxs-lookup"><span data-stu-id="2be1f-899">For example following code will map `DatePart` CLR function to `DATEPART` built-in function on SqlServer.</span></span>

```csharp
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

<span data-ttu-id="2be1f-900">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2be1f-900">**New behavior**</span></span>

<span data-ttu-id="2be1f-901">Všechna mapování DbFunction se považují za namapovaná na uživatelsky definované funkce.</span><span class="sxs-lookup"><span data-stu-id="2be1f-901">All DbFunction mappings are considered to be mapped to user defined functions.</span></span> <span data-ttu-id="2be1f-902">Proto je prázdná hodnota řetězce vložena do výchozího schématu pro model.</span><span class="sxs-lookup"><span data-stu-id="2be1f-902">Hence empty string value would put the function inside the default schema for the model.</span></span> <span data-ttu-id="2be1f-903">To může být schéma, které je explicitně nakonfigurované prostřednictvím rozhraní Fluent API `modelBuilder.HasDefaultSchema()` nebo `dbo` jinak.</span><span class="sxs-lookup"><span data-stu-id="2be1f-903">Which could be the schema configured explicitly via fluent API `modelBuilder.HasDefaultSchema()` or `dbo` otherwise.</span></span>

<span data-ttu-id="2be1f-904">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2be1f-904">**Why**</span></span>

<span data-ttu-id="2be1f-905">Dříve prázdné schéma bylo způsobem, jak se zacházet s touto funkcí, ale tato funkce je k dispozici pouze pro SqlServer, kde předdefinované funkce nepatří do žádného schématu.</span><span class="sxs-lookup"><span data-stu-id="2be1f-905">Previously schema being empty was a way to treat that function is built-in but that logic is only applicable for SqlServer where built-in functions do not belong to any schema.</span></span>

<span data-ttu-id="2be1f-906">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2be1f-906">**Mitigations**</span></span>

<span data-ttu-id="2be1f-907">Nakonfigurujte převod DbFunction ručně, abyste ho namapovali na vestavěnou funkci.</span><span class="sxs-lookup"><span data-stu-id="2be1f-907">Configure DbFunction's translation manually to map it to a built-in function.</span></span>

```csharp
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```
