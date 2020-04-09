---
title: Nejnovější změny v EF Core 3.0 - EF Core
author: ajcvickers
ms.date: 12/03/2019
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 6e0c17a22b56b206f18e47f678e3e237d5c42375
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417459"
---
# <a name="breaking-changes-included-in-ef-core-30"></a><span data-ttu-id="2a2ba-102">Nejnovější změny zahrnuté v EF Core 3.0</span><span class="sxs-lookup"><span data-stu-id="2a2ba-102">Breaking changes included in EF Core 3.0</span></span>

<span data-ttu-id="2a2ba-103">Následující změny rozhraní API a chování mají potenciál přerušit existující aplikace při jejich upgradu na 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-103">The following API and behavior changes have the potential to break existing applications when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="2a2ba-104">Změny, které očekáváme, že ovlivní pouze zprostředkovatele databáze, jsou dokumentovány v rámci [změn zprostředkovatele](xref:core/providers/provider-log).</span><span class="sxs-lookup"><span data-stu-id="2a2ba-104">Changes that we expect to only impact database providers are documented under [provider changes](xref:core/providers/provider-log).</span></span>

## <a name="summary"></a><span data-ttu-id="2a2ba-105">Souhrn</span><span class="sxs-lookup"><span data-stu-id="2a2ba-105">Summary</span></span>

| <span data-ttu-id="2a2ba-106">**Narušující změny**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-106">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="2a2ba-107">**Dopad**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-107">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="2a2ba-108">Dotazy LINQ již nejsou vyhodnocovány na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-108">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="2a2ba-109">Vysoká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-109">High</span></span>       |
| [<span data-ttu-id="2a2ba-110">Cíle EF Core 3.0 .NET Standard 2.1 spíše než .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="2a2ba-110">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="2a2ba-111">Vysoká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-111">High</span></span>      |
| [<span data-ttu-id="2a2ba-112">Nástroj příkazového řádku EF Core, dotnet ef, již není součástí sady .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="2a2ba-112">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="2a2ba-113">Vysoká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-113">High</span></span>      |
| [<span data-ttu-id="2a2ba-114">DetectChanges respektuje hodnoty klíče generované úložištěm</span><span class="sxs-lookup"><span data-stu-id="2a2ba-114">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="2a2ba-115">Vysoká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-115">High</span></span>      |
| [<span data-ttu-id="2a2ba-116">FromSql, ExecuteSql a ExecuteSqlAsync byly přejmenovány</span><span class="sxs-lookup"><span data-stu-id="2a2ba-116">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="2a2ba-117">Vysoká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-117">High</span></span>      |
| [<span data-ttu-id="2a2ba-118">Typy dotazů jsou konsolidovány s typy entit.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-118">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="2a2ba-119">Vysoká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-119">High</span></span>      |
| [<span data-ttu-id="2a2ba-120">Core rámce účetních údajů již není součástí sdíleného rámce ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2a2ba-120">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="2a2ba-121">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="2a2ba-121">Medium</span></span>      |
| [<span data-ttu-id="2a2ba-122">Kaskádové odstranění se nyní ve výchozím nastavení děje okamžitě</span><span class="sxs-lookup"><span data-stu-id="2a2ba-122">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="2a2ba-123">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="2a2ba-123">Medium</span></span>      |
| [<span data-ttu-id="2a2ba-124">Eager načítání souvisejících entit se nyní děje v jednom dotazu</span><span class="sxs-lookup"><span data-stu-id="2a2ba-124">Eager loading of related entities now happens in a single query</span></span>](#eager-loading-single-query) | <span data-ttu-id="2a2ba-125">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="2a2ba-125">Medium</span></span>      |
| [<span data-ttu-id="2a2ba-126">DeleteBehavior.Restrict má čistší sémantiku.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-126">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="2a2ba-127">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="2a2ba-127">Medium</span></span>      |
| [<span data-ttu-id="2a2ba-128">Konfigurační rozhraní API pro vztahy vlastněných typů bylo změněno.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-128">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="2a2ba-129">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="2a2ba-129">Medium</span></span>      |
| [<span data-ttu-id="2a2ba-130">Každá vlastnost používá nezávislé generování celého klíče v paměti</span><span class="sxs-lookup"><span data-stu-id="2a2ba-130">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="2a2ba-131">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="2a2ba-131">Medium</span></span>      |
| [<span data-ttu-id="2a2ba-132">Dotazy bez sledování již neprovádějí rozlišení identity</span><span class="sxs-lookup"><span data-stu-id="2a2ba-132">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="2a2ba-133">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="2a2ba-133">Medium</span></span>      |
| [<span data-ttu-id="2a2ba-134">Změny rozhraní API metadat</span><span class="sxs-lookup"><span data-stu-id="2a2ba-134">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="2a2ba-135">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="2a2ba-135">Medium</span></span>      |
| [<span data-ttu-id="2a2ba-136">Změny rozhraní METADAT SPECIFICKÉ Pro zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="2a2ba-136">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="2a2ba-137">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="2a2ba-137">Medium</span></span>      |
| [<span data-ttu-id="2a2ba-138">UseRowNumberForPaging byl odebrán.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-138">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="2a2ba-139">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="2a2ba-139">Medium</span></span>      |
| [<span data-ttu-id="2a2ba-140">FromSql metodu při použití s uloženou proceduru nelze skládat</span><span class="sxs-lookup"><span data-stu-id="2a2ba-140">FromSql method when used with stored procedure cannot be composed</span></span>](#fromsqlsproc) | <span data-ttu-id="2a2ba-141">Střednědobé používání</span><span class="sxs-lookup"><span data-stu-id="2a2ba-141">Medium</span></span>      |
| [<span data-ttu-id="2a2ba-142">FromSql metody lze zadat pouze na kořeny dotazu</span><span class="sxs-lookup"><span data-stu-id="2a2ba-142">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="2a2ba-143">Nízká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-143">Low</span></span>      |
| [<span data-ttu-id="2a2ba-144">~~Spuštění dotazu je zaznamenáno na úrovni ladění.~~ Vráceny</span><span class="sxs-lookup"><span data-stu-id="2a2ba-144">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="2a2ba-145">Nízká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-145">Low</span></span>      |
| [<span data-ttu-id="2a2ba-146">Hodnoty dočasného klíče již nejsou nastaveny na instance entit.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-146">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="2a2ba-147">Nízká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-147">Low</span></span>      |
| [<span data-ttu-id="2a2ba-148">Závislé entity sdílející tabulku s objektem zabezpečení jsou nyní volitelné.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-148">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="2a2ba-149">Nízká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-149">Low</span></span>      |
| [<span data-ttu-id="2a2ba-150">Všechny entity sdílející tabulku se sloupcem tokenu souběžnosti musí mapovat na vlastnost</span><span class="sxs-lookup"><span data-stu-id="2a2ba-150">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="2a2ba-151">Nízká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-151">Low</span></span>      |
| [<span data-ttu-id="2a2ba-152">Vlastněné entity nelze dotazovat bez vlastníka pomocí sledovacího dotazu.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-152">Owned entities cannot be queried without the owner using a tracking query</span></span>](#owned-query) | <span data-ttu-id="2a2ba-153">Nízká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-153">Low</span></span>      |
| [<span data-ttu-id="2a2ba-154">Zděděné vlastnosti z nemapovaných typů jsou nyní mapovány na jeden sloupec pro všechny odvozené typy.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-154">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="2a2ba-155">Nízká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-155">Low</span></span>      |
| [<span data-ttu-id="2a2ba-156">Konvence vlastností cizího klíče již neodpovídá stejnému názvu jako hlavní vlastnost.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-156">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="2a2ba-157">Nízká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-157">Low</span></span>      |
| [<span data-ttu-id="2a2ba-158">Připojení databáze je nyní uzavřena, pokud již není použit a před dokončením TransactionScope</span><span class="sxs-lookup"><span data-stu-id="2a2ba-158">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="2a2ba-159">Nízká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-159">Low</span></span>      |
| [<span data-ttu-id="2a2ba-160">Ve výchozím nastavení se používají záložní pole.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-160">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="2a2ba-161">Nízká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-161">Low</span></span>      |
| [<span data-ttu-id="2a2ba-162">Vyvolání, pokud je nalezeno více kompatibilních doprovodových polí</span><span class="sxs-lookup"><span data-stu-id="2a2ba-162">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="2a2ba-163">Nízká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-163">Low</span></span>      |
| [<span data-ttu-id="2a2ba-164">Názvy vlastností pouze pro pole by měly odpovídat názvu pole.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-164">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="2a2ba-165">Nízká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-165">Low</span></span>      |
| [<span data-ttu-id="2a2ba-166">AddDbContext/AddDbContextPool již nevolá addlogging a addmemorycache</span><span class="sxs-lookup"><span data-stu-id="2a2ba-166">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="2a2ba-167">Nízká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-167">Low</span></span>      |
| [<span data-ttu-id="2a2ba-168">AddEntityFramework\* přidá IMemoryCache s limitem velikosti</span><span class="sxs-lookup"><span data-stu-id="2a2ba-168">AddEntityFramework\* adds IMemoryCache with a size limit</span></span>](#addentityframework-adds-imemorycache-with-a-size-limit) | <span data-ttu-id="2a2ba-169">Nízká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-169">Low</span></span>      |
| [<span data-ttu-id="2a2ba-170">DbContext.Entry nyní provádí místní DetectChanges</span><span class="sxs-lookup"><span data-stu-id="2a2ba-170">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="2a2ba-171">Nízká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-171">Low</span></span>      |
| [<span data-ttu-id="2a2ba-172">Klíče pole řetězce a bajtů nejsou ve výchozím nastavení generovány klientem.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-172">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="2a2ba-173">Nízká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-173">Low</span></span>      |
| [<span data-ttu-id="2a2ba-174">ILoggerFactory je nyní oborová služba</span><span class="sxs-lookup"><span data-stu-id="2a2ba-174">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="2a2ba-175">Nízká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-175">Low</span></span>      |
| [<span data-ttu-id="2a2ba-176">Opožděné načítání proxy servery již předpokládat, navigační vlastnosti jsou plně načteny</span><span class="sxs-lookup"><span data-stu-id="2a2ba-176">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="2a2ba-177">Nízká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-177">Low</span></span>      |
| [<span data-ttu-id="2a2ba-178">Nadměrné vytváření interních poskytovatelů služeb je nyní ve výchozím nastavení chybou</span><span class="sxs-lookup"><span data-stu-id="2a2ba-178">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="2a2ba-179">Nízká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-179">Low</span></span>      |
| [<span data-ttu-id="2a2ba-180">Nové chování pro HasOne/HasMany volána s jedním řetězcem</span><span class="sxs-lookup"><span data-stu-id="2a2ba-180">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="2a2ba-181">Nízká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-181">Low</span></span>      |
| [<span data-ttu-id="2a2ba-182">Návratový typ pro několik asynchronních metod byl změněn z Task na ValueTask.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-182">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="2a2ba-183">Nízká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-183">Low</span></span>      |
| [<span data-ttu-id="2a2ba-184">Relační a textová poznámka je nyní pouze typemapping</span><span class="sxs-lookup"><span data-stu-id="2a2ba-184">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="2a2ba-185">Nízká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-185">Low</span></span>      |
| [<span data-ttu-id="2a2ba-186">ToTable na odvozený typ vyvolá výjimku</span><span class="sxs-lookup"><span data-stu-id="2a2ba-186">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="2a2ba-187">Nízká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-187">Low</span></span>      |
| [<span data-ttu-id="2a2ba-188">EF Core již neodesílá pragma pro vynucení SQLite FK</span><span class="sxs-lookup"><span data-stu-id="2a2ba-188">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="2a2ba-189">Nízká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-189">Low</span></span>      |
| [<span data-ttu-id="2a2ba-190">Microsoft.EntityFrameworkCore.Sqlite nyní závisí na SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="2a2ba-190">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="2a2ba-191">Nízká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-191">Low</span></span>      |
| [<span data-ttu-id="2a2ba-192">Hodnoty guid jsou nyní uloženy jako TEXT na SQLite</span><span class="sxs-lookup"><span data-stu-id="2a2ba-192">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="2a2ba-193">Nízká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-193">Low</span></span>      |
| [<span data-ttu-id="2a2ba-194">Hodnoty Char jsou nyní uloženy jako TEXT na SQLite</span><span class="sxs-lookup"><span data-stu-id="2a2ba-194">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="2a2ba-195">Nízká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-195">Low</span></span>      |
| [<span data-ttu-id="2a2ba-196">ID migrace jsou nyní generovány pomocí kalendáře invariantní jazykové verze</span><span class="sxs-lookup"><span data-stu-id="2a2ba-196">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="2a2ba-197">Nízká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-197">Low</span></span>      |
| [<span data-ttu-id="2a2ba-198">Informace/metadata rozšíření byly odebrány z rozšíření IDbContextOptionsExtension.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-198">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="2a2ba-199">Nízká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-199">Low</span></span>      |
| [<span data-ttu-id="2a2ba-200">LogQueryPossibleExceptionWithAggregateOperator byl přejmenován.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-200">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="2a2ba-201">Nízká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-201">Low</span></span>      |
| [<span data-ttu-id="2a2ba-202">Objasnit rozhraní API pro názvy omezení cizího klíče</span><span class="sxs-lookup"><span data-stu-id="2a2ba-202">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="2a2ba-203">Nízká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-203">Low</span></span>      |
| [<span data-ttu-id="2a2ba-204">IRelationalDatabaseCreator.HasTables/HasTablesAsync byly zveřejněny</span><span class="sxs-lookup"><span data-stu-id="2a2ba-204">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="2a2ba-205">Nízká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-205">Low</span></span>      |
| [<span data-ttu-id="2a2ba-206">Microsoft.EntityFrameworkCore.Design je nyní balíček DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="2a2ba-206">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="2a2ba-207">Nízká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-207">Low</span></span>      |
| [<span data-ttu-id="2a2ba-208">SQLitePCL.raw aktualizován na verzi 2.0.0</span><span class="sxs-lookup"><span data-stu-id="2a2ba-208">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="2a2ba-209">Nízká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-209">Low</span></span>      |
| [<span data-ttu-id="2a2ba-210">NetTopologySuite aktualizován na verzi 2.0.0</span><span class="sxs-lookup"><span data-stu-id="2a2ba-210">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="2a2ba-211">Nízká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-211">Low</span></span>      |
| [<span data-ttu-id="2a2ba-212">Místo klienta System.Data.SqlClient se používá místo klienta System.Data.SqlClient</span><span class="sxs-lookup"><span data-stu-id="2a2ba-212">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>](#SqlClient) | <span data-ttu-id="2a2ba-213">Nízká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-213">Low</span></span>      |
| [<span data-ttu-id="2a2ba-214">Musí být nakonfigurováno více nejednoznačných vztahů, které samy odkazují na sebe.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-214">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="2a2ba-215">Nízká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-215">Low</span></span>      |
| [<span data-ttu-id="2a2ba-216">DbFunction.Schema je null nebo prázdný řetězec nakonfiguruje, aby byl ve výchozím schématu modelu</span><span class="sxs-lookup"><span data-stu-id="2a2ba-216">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>](#udf-empty-string) | <span data-ttu-id="2a2ba-217">Nízká</span><span class="sxs-lookup"><span data-stu-id="2a2ba-217">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="2a2ba-218">Dotazy LINQ již nejsou vyhodnocovány na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-218">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="2a2ba-219">[Sledování problému #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Také zobrazit #12795 problému](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="2a2ba-219">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="2a2ba-220">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-220">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-221">Před 3.0, když EF Core nelze převést výraz, který byl součástí dotazu buď SQL nebo parametr, automaticky vyhodnocenvýrazv klientovi.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-221">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="2a2ba-222">Ve výchozím nastavení klienthodnocení potenciálně nákladné výrazy pouze spustila upozornění.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-222">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="2a2ba-223">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-223">**New behavior**</span></span>

<span data-ttu-id="2a2ba-224">Počínaje 3.0 EF Core umožňuje pouze výrazy v projekci nejvyšší úrovně (poslední `Select()` volání v dotazu) vyhodnoceny na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-224">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="2a2ba-225">Pokud výrazy v jakékoli jiné části dotazu nelze převést na SQL nebo parametr, je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-225">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="2a2ba-226">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-226">**Why**</span></span>

<span data-ttu-id="2a2ba-227">Automatické vyhodnocení dotazů klienta umožňuje mnoho dotazů, které mají být provedeny i v případě, že důležité části z nich nelze přeložit.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-227">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="2a2ba-228">Toto chování může mít za následek neočekávané a potenciálně škodlivé chování, které může být zřejmé pouze v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-228">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="2a2ba-229">Například podmínka ve `Where()` volání, které nelze přeložit, může způsobit přenos všech řádků z tabulky z databázového serveru a filtru, který má být použit na klienta.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-229">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="2a2ba-230">Tato situace může snadno přejít nezjištěný, pokud tabulka obsahuje pouze několik řádků ve vývoji, ale tvrdě přístupů při aplikaci přesune do produkčního prostředí, kde tabulka může obsahovat miliony řádků.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-230">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="2a2ba-231">Upozornění na vyhodnocení klienta se také ukázala jako příliš snadno ignorovat během vývoje.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-231">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="2a2ba-232">Kromě toho automatické vyhodnocení klienta může vést k problémům, ve kterých zlepšení překladu dotazů pro konkrétní výrazy způsobily nechtěné změny mezi verzemi.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-232">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="2a2ba-233">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-233">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-234">Pokud dotaz nelze plně přeložit, přepište dotaz ve formuláři, který lze přeložit, `AsEnumerable()` `ToList()`nebo použijte , nebo podobně jako explicitně převést data zpět do klienta, kde je pak možné dále zpracovat pomocí LINQ-to-Objects.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-234">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="2a2ba-235">Cíle EF Core 3.0 .NET Standard 2.1 spíše než .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="2a2ba-235">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="2a2ba-236">Sledování #15498 problému</span><span class="sxs-lookup"><span data-stu-id="2a2ba-236">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

> [!IMPORTANT] 
> <span data-ttu-id="2a2ba-237">EF Core 3.1 opět cílí na standard .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-237">EF Core 3.1 targets .NET Standard 2.0 again.</span></span> <span data-ttu-id="2a2ba-238">To přináší zpět podporu pro rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-238">This brings back support for .NET Framework.</span></span>

<span data-ttu-id="2a2ba-239">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-239">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-240">Před 3.0 EF Core cílené .NET Standard 2.0 a bude fungovat na všech platformách, které podporují tento standard, včetně rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-240">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="2a2ba-241">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-241">**New behavior**</span></span>

<span data-ttu-id="2a2ba-242">Počínaje 3.0, EF Core cíle .NET Standard 2.1 a bude spuštěna na všech platformách, které podporují tento standard.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-242">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="2a2ba-243">To nezahrnuje rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-243">This does not include .NET Framework.</span></span>

<span data-ttu-id="2a2ba-244">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-244">**Why**</span></span>

<span data-ttu-id="2a2ba-245">Toto je součástí strategického rozhodnutí napříč technologiemi .NET zaměřit energii na .NET Core a další moderní .NET platformy, jako je například Xamarin.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-245">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="2a2ba-246">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-246">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-247">Použijte EF Core 3.1.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-247">Use EF Core 3.1.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="2a2ba-248">Core rámce účetních údajů již není součástí sdíleného rámce ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2a2ba-248">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="2a2ba-249">Sledování oznámení o problému#325</span><span class="sxs-lookup"><span data-stu-id="2a2ba-249">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="2a2ba-250">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-250">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-251">Před ASP.NET Core 3.0, když jste `Microsoft.AspNetCore.App` `Microsoft.AspNetCore.All`přidali odkaz na balíček nebo , by to zahrnovalo EF Core a některé zprostředkovatele dat EF Core, jako je poskytovatel serveru SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-251">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="2a2ba-252">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-252">**New behavior**</span></span>

<span data-ttu-id="2a2ba-253">Počínaje 3.0 ASP.NET základní sdílené rozhraní nezahrnuje EF Core nebo zprostředkovatele dat EF Core.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-253">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="2a2ba-254">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-254">**Why**</span></span>

<span data-ttu-id="2a2ba-255">Před touto změnou, získání EF Core vyžaduje různé kroky v závislosti na tom, zda aplikace cílené ASP.NET Core a SQL Server nebo ne.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-255">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="2a2ba-256">Také inovace ASP.NET Core vynutila upgrade EF Core a zprostředkovatele SQL Server, což není vždy žádoucí.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-256">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="2a2ba-257">S touto změnou je prostředí získávání EF Core stejné napříč všemi poskytovateli, podporovanými implementacemi rozhraní .NET a typy aplikací.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-257">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="2a2ba-258">Vývojáři teď taky můžou přesně řídit, kdy jsou upgradováni zprostředkovatelé dat EF Core a EF Core.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-258">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="2a2ba-259">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-259">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-260">Chcete-li použít EF Core v ASP.NET core 3.0 aplikace nebo jiné podporované aplikace, explicitně přidejte odkaz na balíček zprostředkovatele databáze EF Core, který bude vaše aplikace používat.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-260">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="2a2ba-261">Nástroj příkazového řádku EF Core, dotnet ef, již není součástí sady .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="2a2ba-261">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="2a2ba-262">#14016 problému sledování</span><span class="sxs-lookup"><span data-stu-id="2a2ba-262">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="2a2ba-263">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-263">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-264">Před 3.0 `dotnet ef` byl nástroj zahrnut do sady .NET Core SDK a byl snadno dostupný z příkazového řádku z libovolného projektu bez nutnosti dalších kroků.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-264">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="2a2ba-265">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-265">**New behavior**</span></span>

<span data-ttu-id="2a2ba-266">Počínaje 3.0 sada .NET SDK neobsahuje `dotnet ef` nástroj, takže před použitím jej musíte explicitně nainstalovat jako místní nebo globální nástroj.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-266">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="2a2ba-267">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-267">**Why**</span></span>

<span data-ttu-id="2a2ba-268">Tato změna nám umožňuje `dotnet ef` distribuovat a aktualizovat jako běžný nástroj .NET CLI na NuGet, konzistentní se skutečností, že EF Core 3.0 je také vždy distribuován jako balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-268">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="2a2ba-269">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-269">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-270">Chcete-li spravovat migrace nebo lešení `DbContext` `dotnet-ef` a , nainstalujte jako globální nástroj:</span><span class="sxs-lookup"><span data-stu-id="2a2ba-270">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef
  ```

<span data-ttu-id="2a2ba-271">Můžete také získat místní nástroj při obnovení závislostí projektu, který deklaruje jako závislost nástroje pomocí [souboru manifestu nástroje](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="2a2ba-271">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="2a2ba-272">FromSql, ExecuteSql a ExecuteSqlAsync byly přejmenovány</span><span class="sxs-lookup"><span data-stu-id="2a2ba-272">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="2a2ba-273">#10996 sledování problému</span><span class="sxs-lookup"><span data-stu-id="2a2ba-273">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="2a2ba-274">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-274">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-275">Před EF Core 3.0 byly tyto názvy metod přetíženy, aby pracovaly s normálním řetězcem nebo řetězcem, který by měl být interpolován do SQL a parametrů.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-275">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="2a2ba-276">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-276">**New behavior**</span></span>

<span data-ttu-id="2a2ba-277">Počínaje EF Core 3.0, `ExecuteSqlRaw`použijte `ExecuteSqlRawAsync` `FromSqlRaw`, a vytvořit parametrizovaný dotaz, kde jsou parametry předány odděleně od řetězce dotazu.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-277">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="2a2ba-278">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2a2ba-278">For example:</span></span>

```csharp
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="2a2ba-279">Použijte `FromSqlInterpolated` `ExecuteSqlInterpolated`, `ExecuteSqlInterpolatedAsync` a vytvořte parametrizovaný dotaz, kde jsou parametry předány jako součást řetězce interpolovaného dotazu.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-279">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="2a2ba-280">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2a2ba-280">For example:</span></span>

```csharp
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="2a2ba-281">Všimněte si, že oba výše uvedené dotazy vytvoří stejné parametrizované SQL se stejnými parametry SQL.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-281">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="2a2ba-282">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-282">**Why**</span></span>

<span data-ttu-id="2a2ba-283">Přetížení metody, jako je tento, usnadňují náhodné volání metody raw string, když záměrem bylo volání interpolované metody řetězce a naopak.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-283">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="2a2ba-284">To může mít za následek dotazy nejsou parametrizovány, když by měly být.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-284">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="2a2ba-285">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-285">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-286">Přepněte, abyste použili nové názvy metod.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-286">Switch to use the new method names.</span></span>

<a name="fromsqlsproc"></a>
### <a name="fromsql-method-when-used-with-stored-procedure-cannot-be-composed"></a><span data-ttu-id="2a2ba-287">FromSql metodu při použití s uloženou proceduru nelze skládat</span><span class="sxs-lookup"><span data-stu-id="2a2ba-287">FromSql method when used with stored procedure cannot be composed</span></span>

[<span data-ttu-id="2a2ba-288">Sledování #15392 problému</span><span class="sxs-lookup"><span data-stu-id="2a2ba-288">Tracking Issue #15392</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15392)

<span data-ttu-id="2a2ba-289">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-289">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-290">Před EF Core 3.0, FromSql metoda se pokusil zjistit, pokud předané SQL lze skládat na.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-290">Before EF Core 3.0, FromSql method tried to detect if the passed SQL can be composed upon.</span></span> <span data-ttu-id="2a2ba-291">Provedla vyhodnocení klienta, když sql byl non-composable jako uložená procedura.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-291">It did client evaluation when the SQL was non-composable like a stored procedure.</span></span> <span data-ttu-id="2a2ba-292">Následující dotaz fungoval spuštěním uložené procedury na serveru a provedením FirstOrDefault na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-292">The following query worked by running the stored procedure on the server and doing FirstOrDefault on the client side.</span></span>

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").FirstOrDefault();
```

<span data-ttu-id="2a2ba-293">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-293">**New behavior**</span></span>

<span data-ttu-id="2a2ba-294">Počínaje EF Core 3.0 EF Core se nepokusí analyzovat SQL.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-294">Starting with EF Core 3.0, EF Core will not try to parse the SQL.</span></span> <span data-ttu-id="2a2ba-295">Takže pokud jste skládání po FromSqlRaw/FromSqlInterpolated, pak EF Core bude skládat SQL tím, že způsobí dílčí dotaz.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-295">So if you are composing after FromSqlRaw/FromSqlInterpolated, then EF Core will compose the SQL by causing sub query.</span></span> <span data-ttu-id="2a2ba-296">Takže pokud používáte uloženou proceduru s kompozicí, pak získáte výjimku pro neplatnou syntaxi SQL.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-296">So if you are using a stored procedure with composition then you will get an exception for invalid SQL syntax.</span></span>

<span data-ttu-id="2a2ba-297">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-297">**Why**</span></span>

<span data-ttu-id="2a2ba-298">EF Core 3.0 nepodporuje automatické hodnocení klienta, protože byl náchylný k chybám, jak je vysvětleno [zde](#linq-queries-are-no-longer-evaluated-on-the-client).</span><span class="sxs-lookup"><span data-stu-id="2a2ba-298">EF Core 3.0 does not support automatic client evaluation, since it was error prone as explained [here](#linq-queries-are-no-longer-evaluated-on-the-client).</span></span>

<span data-ttu-id="2a2ba-299">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-299">**Mitigation**</span></span>

<span data-ttu-id="2a2ba-300">Pokud používáte uloženou proceduru v FromSqlRaw/FromSqlInterpolated, víte, že ji nelze skládat, takže můžete přidat __AsEnumerable/AsAsyncEnumerable__ hned po volání metody FromSql, abyste se vyhnuli jakékoli kompozici na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-300">If you are using a stored procedure in FromSqlRaw/FromSqlInterpolated, you know that it cannot be composed upon, so you can add __AsEnumerable/AsAsyncEnumerable__ right after the FromSql method call to avoid any composition on server side.</span></span>

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").AsEnumerable().FirstOrDefault();
```

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="2a2ba-301">FromSql metody lze zadat pouze na kořeny dotazu</span><span class="sxs-lookup"><span data-stu-id="2a2ba-301">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="2a2ba-302">#15704 sledování problému</span><span class="sxs-lookup"><span data-stu-id="2a2ba-302">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="2a2ba-303">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-303">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-304">Před EF Core `FromSql` 3.0, metoda může být zadán kdekoli v dotazu.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-304">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="2a2ba-305">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-305">**New behavior**</span></span>

<span data-ttu-id="2a2ba-306">Počínaje EF Core 3.0, `FromSqlRaw` `FromSqlInterpolated` nové a `FromSql`metody (které nahrazují ) lze zadat pouze na `DbSet<>`kořeny dotazu, tj.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-306">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="2a2ba-307">Pokus o jejich zadání kdekoli jinde bude mít za následek chybu kompilace.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-307">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="2a2ba-308">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-308">**Why**</span></span>

<span data-ttu-id="2a2ba-309">Zadání `FromSql` kdekoli jinde než `DbSet` na neměl žádný význam nebo přidanou hodnotu a může způsobit nejednoznačnost v určitých scénářích.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-309">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="2a2ba-310">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-310">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-311">`FromSql`by měla být přesunuta tak, `DbSet` aby byla přímo na základě toho, na které se vztahují.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-311">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="2a2ba-312">Dotazy bez sledování již neprovádějí rozlišení identity</span><span class="sxs-lookup"><span data-stu-id="2a2ba-312">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="2a2ba-313">Sledování #13518 problému</span><span class="sxs-lookup"><span data-stu-id="2a2ba-313">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="2a2ba-314">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-314">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-315">Před EF Core 3.0 by se stejná instance entity použila pro každý výskyt entity s daným typem a ID.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-315">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="2a2ba-316">To odpovídá chování sledování dotazů.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-316">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="2a2ba-317">Například tento dotaz:</span><span class="sxs-lookup"><span data-stu-id="2a2ba-317">For example, this query:</span></span>

```csharp
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="2a2ba-318">vrátí stejnou `Category` instanci `Product` pro každou, která je přidružena k dané kategorii.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-318">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="2a2ba-319">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-319">**New behavior**</span></span>

<span data-ttu-id="2a2ba-320">Počínaje EF Core 3.0, budou vytvořeny různé instance entity, když se na různých místech vráceného grafu objeví entita s daným typem a ID.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-320">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="2a2ba-321">Například výše uvedený dotaz nyní `Category` vrátí novou `Product` instanci pro každou z nich i v případě, že dva produkty jsou přidruženy ke stejné kategorii.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-321">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="2a2ba-322">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-322">**Why**</span></span>

<span data-ttu-id="2a2ba-323">Rozlišení identity (to znamená určení, že entita má stejný typ a ID jako dříve zjištěná entita) přidává další výkon a nároky na paměť.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-323">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="2a2ba-324">To obvykle běží v rozporu s tím, proč žádné sledování dotazy jsou používány na prvním místě.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-324">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="2a2ba-325">Také zatímco překlad identity může být někdy užitečné, není potřeba, pokud entity mají být serializovány a odeslány klientovi, což je běžné pro dotazy bez sledování.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-325">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="2a2ba-326">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-326">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-327">Pokud je vyžadováno rozlišení identity, použijte sledovací dotaz.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-327">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="2a2ba-328">~~Spuštění dotazu je zaznamenáno na úrovni ladění.~~ Vráceny</span><span class="sxs-lookup"><span data-stu-id="2a2ba-328">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="2a2ba-329">#14523 sledování problémů</span><span class="sxs-lookup"><span data-stu-id="2a2ba-329">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="2a2ba-330">Tuto změnu jsme vrátili zpět, protože nová konfigurace v EF Core 3.0 umožňuje úroveň protokolu pro každou událost, která má být určena aplikací.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-330">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="2a2ba-331">Chcete-li například přepnout `Debug`protokolování sql `OnConfiguring` na `AddDbContext`, explicitně nakonfigurujte úroveň v nebo :</span><span class="sxs-lookup"><span data-stu-id="2a2ba-331">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="2a2ba-332">Hodnoty dočasného klíče již nejsou nastaveny na instance entit.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-332">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="2a2ba-333">#12378 sledování problému</span><span class="sxs-lookup"><span data-stu-id="2a2ba-333">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="2a2ba-334">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-334">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-335">Před EF Core 3.0 dočasné hodnoty byly přiřazeny ke všem vlastnostem klíče, které by později mít reálnou hodnotu generovanou databází.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-335">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="2a2ba-336">Obvykle tyto dočasné hodnoty byly velké záporná čísla.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-336">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="2a2ba-337">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-337">**New behavior**</span></span>

<span data-ttu-id="2a2ba-338">Počínaje 3.0 EF Core ukládá hodnotu dočasného klíče jako součást informací o sledování entity a ponechá samotnou vlastnost klíče beze změny.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-338">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="2a2ba-339">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-339">**Why**</span></span>

<span data-ttu-id="2a2ba-340">Tato změna byla provedena, aby se zabránilo chybně dočasným hodnotám klíče, aby `DbContext` se chybně staly `DbContext` trvalými, když je entita, která byla dříve sledována nějakou instancí, přesunuta do jiné instance.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-340">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="2a2ba-341">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-341">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-342">Aplikace, které přiřazují hodnoty primárního klíče cizím klíčům k vytvoření přidružení mezi entitami, `Added` mohou záviset na starém chování, pokud jsou primární klíče generovány v úložišti a patří k entitám ve státě.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-342">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="2a2ba-343">Tomu se lze vyhnout:</span><span class="sxs-lookup"><span data-stu-id="2a2ba-343">This can be avoided by:</span></span>
* <span data-ttu-id="2a2ba-344">Nepoužíváte klíče generované úložištěm.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-344">Not using store-generated keys.</span></span>
* <span data-ttu-id="2a2ba-345">Nastavení navigačních vlastností na vytvoření relací namísto nastavení hodnot cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-345">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="2a2ba-346">Získejte skutečné hodnoty dočasného klíče z informací o sledování entity.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-346">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="2a2ba-347">Například `context.Entry(blog).Property(e => e.Id).CurrentValue` vrátí dočasnou hodnotu, i když `blog.Id` sama nebyla nastavena.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-347">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="2a2ba-348">DetectChanges respektuje hodnoty klíče generované úložištěm</span><span class="sxs-lookup"><span data-stu-id="2a2ba-348">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="2a2ba-349">#14616 problému sledování</span><span class="sxs-lookup"><span data-stu-id="2a2ba-349">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="2a2ba-350">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-350">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-351">Před EF Core 3.0, nesledované `DetectChanges` entity nalezeny by `Added` být sledovány ve stavu `SaveChanges` a vloženy jako nový řádek, když je volána.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-351">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="2a2ba-352">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-352">**New behavior**</span></span>

<span data-ttu-id="2a2ba-353">Počínaje EF Core 3.0, pokud entita používá generované hodnoty klíče a je nastavena některá hodnota klíče, bude entita sledována ve `Modified` stavu.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-353">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="2a2ba-354">To znamená, že se předpokládá, že existuje řádek `SaveChanges` pro entitu a bude aktualizován, když je volána.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-354">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="2a2ba-355">Pokud není nastavena hodnota klíče nebo pokud typ entity nepoužívá generované klíče, bude nová entita stále sledována jako `Added` v předchozích verzích.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-355">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="2a2ba-356">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-356">**Why**</span></span>

<span data-ttu-id="2a2ba-357">Tato změna byla provedena, aby bylo snazší a konzistentnější pracovat s grafy odpojených entit při použití klíčů generovaných úložištěm.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-357">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="2a2ba-358">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-358">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-359">Tato změna může přerušit aplikaci, pokud je typ entity nakonfigurován pro použití generovaných klíčů, ale hodnoty klíčů jsou explicitně nastaveny pro nové instance.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-359">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="2a2ba-360">Oprava je explicitně nakonfigurovat vlastnosti klíče nepoužívat generované hodnoty.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-360">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="2a2ba-361">Například s fluent API:</span><span class="sxs-lookup"><span data-stu-id="2a2ba-361">For example, with the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="2a2ba-362">Nebo s poznámkami o datech:</span><span class="sxs-lookup"><span data-stu-id="2a2ba-362">Or with data annotations:</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="2a2ba-363">Kaskádové odstranění se nyní ve výchozím nastavení děje okamžitě</span><span class="sxs-lookup"><span data-stu-id="2a2ba-363">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="2a2ba-364">Sledování #10114 problému</span><span class="sxs-lookup"><span data-stu-id="2a2ba-364">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="2a2ba-365">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-365">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-366">Před 3.0 EF Core použít kaskádové akce (odstranění závislých entit při odstranění požadované ho objektu zabezpečení nebo při přerušeném vztahu k požadované jistiny) nedošlo, dokud SaveChanges byla volána.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-366">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="2a2ba-367">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-367">**New behavior**</span></span>

<span data-ttu-id="2a2ba-368">Počínaje 3.0, EF Core použije kaskádové akce, jakmile je zjištěna spouštěcí podmínka.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-368">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="2a2ba-369">Například volání `context.Remove()` k odstranění hlavní entity bude mít za následek všechny `Deleted` sledované související požadované závislé osoby také nastavena na okamžitě.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-369">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="2a2ba-370">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-370">**Why**</span></span>

<span data-ttu-id="2a2ba-371">Tato změna byla provedena ke zlepšení prostředí pro data vazby a auditování scénáře, kde je důležité pochopit, které entity budou odstraněny _před_ `SaveChanges` nazývá.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-371">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="2a2ba-372">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-372">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-373">Předchozí chování lze obnovit pomocí `context.ChangeTracker`nastavení na .</span><span class="sxs-lookup"><span data-stu-id="2a2ba-373">The previous behavior can be restored through settings on `context.ChangeTracker`.</span></span>
<span data-ttu-id="2a2ba-374">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2a2ba-374">For example:</span></span>

```csharp
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="eager-loading-single-query"></a>
### <a name="eager-loading-of-related-entities-now-happens-in-a-single-query"></a><span data-ttu-id="2a2ba-375">Eager načítání souvisejících entit se nyní děje v jednom dotazu</span><span class="sxs-lookup"><span data-stu-id="2a2ba-375">Eager loading of related entities now happens in a single query</span></span>

[<span data-ttu-id="2a2ba-376">Sledování #18022 problému</span><span class="sxs-lookup"><span data-stu-id="2a2ba-376">Tracking issue #18022</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/18022)

<span data-ttu-id="2a2ba-377">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-377">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-378">Před 3.0 dychtivě načítání kolekce `Include` navigace prostřednictvím operátorů způsobil více dotazů, které mají být generovány na relační databáze, jeden pro každý typ související entity.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-378">Before 3.0, eagerly loading collection navigations via `Include` operators caused multiple queries to be generated on relational database, one for each related entity type.</span></span>

<span data-ttu-id="2a2ba-379">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-379">**New behavior**</span></span>

<span data-ttu-id="2a2ba-380">Počínaje 3.0, EF Core generuje jeden dotaz s JOINs na relační databáze.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-380">Starting with 3.0, EF Core generates a single query with JOINs on relational databases.</span></span>

<span data-ttu-id="2a2ba-381">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-381">**Why**</span></span>

<span data-ttu-id="2a2ba-382">Vydávání více dotazů k implementaci jednoho dotazu LINQ způsobil řadu problémů, včetně negativní výkon jako více databázových roundtrips byly nezbytné a problémy s koherence dat jako každý dotaz mohl sledovat jiný stav databáze.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-382">Issuing multiple queries to implement a single LINQ query caused numerous issues, including negative performance as multiple database roundtrips were necessary, and data coherency issues as each query could observe a different state of the database.</span></span>

<span data-ttu-id="2a2ba-383">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-383">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-384">I když technicky to není narušující změny, může mít značný vliv na výkon `Include` aplikace, pokud jeden dotaz obsahuje velký počet operátorna navigační služby kolekce.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-384">While technically this is not a breaking change, it could have a considerable effect on application performance when a single query contains a large number of `Include` operator on collection navigations.</span></span> <span data-ttu-id="2a2ba-385">Další informace a přepisování dotazů efektivněji naleznete v [tomto komentáři.](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085)</span><span class="sxs-lookup"><span data-stu-id="2a2ba-385">[See this comment](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) for more information and for rewriting queries in a more efficient way.</span></span>

**

<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="2a2ba-386">DeleteBehavior.Restrict má čistší sémantiku.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-386">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="2a2ba-387">Sledování #12661 problému</span><span class="sxs-lookup"><span data-stu-id="2a2ba-387">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="2a2ba-388">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-388">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-389">Před 3.0, `DeleteBehavior.Restrict` vytvořil cizí klíče `Restrict` v databázi s sémantiku, ale také změnil vnitřní opravu v non-zřejmým způsobem.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-389">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="2a2ba-390">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-390">**New behavior**</span></span>

<span data-ttu-id="2a2ba-391">Počínaje 3.0 `DeleteBehavior.Restrict` zajišťuje, že cizí klíče `Restrict` jsou vytvořeny sémantikou -- to znamená, že žádné kaskády; vyvolat porušení omezení – bez dopadu také EF interní opravy.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-391">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="2a2ba-392">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-392">**Why**</span></span>

<span data-ttu-id="2a2ba-393">Tato změna byla provedena s `DeleteBehavior` cílem zlepšit zkušenosti pro použití intuitivním způsobem, bez neočekávaných vedlejších účinků.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-393">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="2a2ba-394">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-394">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-395">Předchozí chování lze obnovit pomocí `DeleteBehavior.ClientNoAction`aplikace .</span><span class="sxs-lookup"><span data-stu-id="2a2ba-395">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="2a2ba-396">Typy dotazů jsou konsolidovány s typy entit.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-396">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="2a2ba-397">Sledování #14194 problému</span><span class="sxs-lookup"><span data-stu-id="2a2ba-397">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="2a2ba-398">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-398">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-399">Před EF Core 3.0, [typy dotazů](xref:core/modeling/keyless-entity-types) byly prostředkem pro dotazování dat, která nedefinuje primární klíč strukturovaným způsobem.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-399">Before EF Core 3.0, [query types](xref:core/modeling/keyless-entity-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="2a2ba-400">To znamená, že typ dotazu byl použit pro mapování typů entit bez klíčů (spíše ze zobrazení, ale případně z tabulky), zatímco normální typ entity byl použit, když byl k dispozici klíč (pravděpodobně z tabulky, ale případně ze zobrazení).</span><span class="sxs-lookup"><span data-stu-id="2a2ba-400">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="2a2ba-401">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-401">**New behavior**</span></span>

<span data-ttu-id="2a2ba-402">Typ dotazu se nyní stane pouze typem entity bez primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-402">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="2a2ba-403">Typy bezklíčových entit mají stejné funkce jako typy dotazů v předchozích verzích.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-403">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="2a2ba-404">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-404">**Why**</span></span>

<span data-ttu-id="2a2ba-405">Tato změna byla provedena snížit nejasnosti kolem účelu typů dotazů.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-405">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="2a2ba-406">Konkrétně jsou bezklíčové typy entit a jsou ze své podstaty jen pro čtení z tohoto důvodu, ale neměly by být použity pouze proto, že typ entity musí být jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-406">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="2a2ba-407">Podobně jsou často mapovány na zobrazení, ale je to jen proto, že zobrazení často nedefinují klíče.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-407">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="2a2ba-408">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-408">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-409">Následující části rozhraní API jsou nyní zastaralé:</span><span class="sxs-lookup"><span data-stu-id="2a2ba-409">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="2a2ba-410">**`ModelBuilder.Query<>()`**- `ModelBuilder.Entity<>().HasNoKey()` Místo toho je třeba volat označit typ entity jako bez klíčů.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-410">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="2a2ba-411">To by stále není nakonfigurován podle konvence, aby se zabránilo chybné konfiguraci, když je očekáván primární klíč, ale neodpovídá konvenci.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-411">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="2a2ba-412">**`DbQuery<>`**- `DbSet<>` Místo toho by měl být použit.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-412">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="2a2ba-413">**`DbContext.Query<>()`**- `DbContext.Set<>()` Místo toho by měl být použit.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-413">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="2a2ba-414">Konfigurační rozhraní API pro vztahy vlastněných typů bylo změněno.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-414">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="2a2ba-415">[Sledování problému #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[sledování problému #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[#14153 sledování](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="2a2ba-415">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="2a2ba-416">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-416">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-417">Před EF Core 3.0 konfigurace vztahu vlastněné `OwnsOne` byla `OwnsMany` provedena přímo po nebo volání.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-417">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="2a2ba-418">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-418">**New behavior**</span></span>

<span data-ttu-id="2a2ba-419">Počínaje EF Core 3.0, je nyní fluent ROZHRANÍ API pro `WithOwner()`konfiguraci navigační vlastnost vlastníka pomocí .</span><span class="sxs-lookup"><span data-stu-id="2a2ba-419">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="2a2ba-420">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2a2ba-420">For example:</span></span>

```csharp
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="2a2ba-421">Konfigurace související se vztahem mezi vlastníkem a vlastněným by nyní měla být zřetězena podobně jako `WithOwner()` ostatní vztahy.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-421">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="2a2ba-422">Zatímco konfigurace pro vlastněný typ sám by `OwnsOne()/OwnsMany()`stále být zřetězené po .</span><span class="sxs-lookup"><span data-stu-id="2a2ba-422">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="2a2ba-423">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2a2ba-423">For example:</span></span>

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

<span data-ttu-id="2a2ba-424">Navíc volání `Entity()` `HasOne()`, `Set()` nebo s cílem typu vlastněného nyní vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-424">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="2a2ba-425">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-425">**Why**</span></span>

<span data-ttu-id="2a2ba-426">Tato změna byla provedena k vytvoření čistší oddělení mezi konfigurací vlastněného typu samotného a _vztah k_ vlastněného typu.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-426">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="2a2ba-427">To zase odstraňuje nejednoznačnost a `HasForeignKey`zmatek kolem metod, jako je .</span><span class="sxs-lookup"><span data-stu-id="2a2ba-427">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="2a2ba-428">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-428">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-429">Změňte konfiguraci vztahů vlastněných typů tak, aby používaly nový povrch rozhraní API, jak je znázorněno ve výše uvedeném příkladu.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-429">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="2a2ba-430">Závislé entity sdílející tabulku s objektem zabezpečení jsou nyní volitelné.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-430">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="2a2ba-431">Sledování #9005</span><span class="sxs-lookup"><span data-stu-id="2a2ba-431">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="2a2ba-432">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-432">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-433">Zvažte následující model:</span><span class="sxs-lookup"><span data-stu-id="2a2ba-433">Consider the following model:</span></span>
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
<span data-ttu-id="2a2ba-434">Před EF Core `OrderDetails` 3.0, `Order` pokud je vlastněna nebo explicitně mapována na stejnou tabulku, byla `OrderDetails` instance vždy vyžadována při přidávání nového `Order`.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-434">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="2a2ba-435">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-435">**New behavior**</span></span>

<span data-ttu-id="2a2ba-436">Počínaje 3.0, EF Core umožňuje `Order` přidat `OrderDetails` bez a `OrderDetails` mapuje všechny vlastnosti s výjimkou primárního klíče na sloupce s možnou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-436">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="2a2ba-437">Při dotazování EF `OrderDetails` `null` Core sady, pokud některý z jeho požadovaných vlastností nemá hodnotu nebo pokud `null`nemá žádné požadované vlastnosti kromě primárního klíče a všechny vlastnosti jsou .</span><span class="sxs-lookup"><span data-stu-id="2a2ba-437">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="2a2ba-438">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-438">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-439">Pokud váš model má tabulka sdílení závislé se všemi volitelnými sloupci, ale navigace ukazující na něj se neočekává, že bude `null` pak aplikace by měla být upravena tak, aby zpracování případů, kdy navigace je `null`.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-439">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="2a2ba-440">Pokud to není možné, požadovaná vlastnost by měla být přidána k`null` typu entity nebo alespoň jedna vlastnost by měla mít non-hodnota přiřazena k němu.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-440">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="2a2ba-441">Všechny entity sdílející tabulku se sloupcem tokenu souběžnosti musí mapovat na vlastnost</span><span class="sxs-lookup"><span data-stu-id="2a2ba-441">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="2a2ba-442">#14154 problému sledování</span><span class="sxs-lookup"><span data-stu-id="2a2ba-442">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="2a2ba-443">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-443">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-444">Zvažte následující model:</span><span class="sxs-lookup"><span data-stu-id="2a2ba-444">Consider the following model:</span></span>
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
<span data-ttu-id="2a2ba-445">Před EF Core `OrderDetails` 3.0, `Order` pokud je vlastněna nebo explicitně `OrderDetails` mapována na stejnou tabulku, pak aktualizace prostě nebude aktualizovat `Version` hodnotu na straně klienta a další aktualizace se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-445">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="2a2ba-446">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-446">**New behavior**</span></span>

<span data-ttu-id="2a2ba-447">Počínaje 3.0, EF Core rozšíří `Version` novou `Order` hodnotu, `OrderDetails`pokud vlastní .</span><span class="sxs-lookup"><span data-stu-id="2a2ba-447">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="2a2ba-448">V opačném případě je vyvolána výjimka během ověřování modelu.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-448">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="2a2ba-449">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-449">**Why**</span></span>

<span data-ttu-id="2a2ba-450">Tato změna byla provedena, aby se zabránilo zatuchlé hodnota tokenu souběžnosti při aktualizaci pouze jedné entity mapované na stejnou tabulku.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-450">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="2a2ba-451">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-451">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-452">Všechny entity sdílející tabulku musí obsahovat vlastnost, která je mapována na sloupec tokenu souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-452">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="2a2ba-453">Je možné, že vytvořit jeden ve stínovém stavu:</span><span class="sxs-lookup"><span data-stu-id="2a2ba-453">It's possible the create one in shadow-state:</span></span>
```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="owned-query"></a>

### <a name="owned-entities-cannot-be-queried-without-the-owner-using-a-tracking-query"></a><span data-ttu-id="2a2ba-454">Vlastněné entity nelze dotazovat bez vlastníka pomocí sledovacího dotazu.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-454">Owned entities cannot be queried without the owner using a tracking query</span></span>

[<span data-ttu-id="2a2ba-455">Sledování #18876 problému</span><span class="sxs-lookup"><span data-stu-id="2a2ba-455">Tracking Issue #18876</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/18876)

<span data-ttu-id="2a2ba-456">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-456">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-457">Před EF Core 3.0, vlastněné entity mohou být dotazovánjako jakékoli jiné navigace.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-457">Before EF Core 3.0, the owned entities could be queried as any other navigation.</span></span>

```csharp
context.People.Select(p => p.Address);
```

<span data-ttu-id="2a2ba-458">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-458">**New behavior**</span></span>

<span data-ttu-id="2a2ba-459">Počínaje 3.0 EF Core vyvolá, pokud dotaz sledování projekty vlastněné entity bez vlastníka.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-459">Starting with 3.0, EF Core will throw if a tracking query projects an owned entity without the owner.</span></span>

<span data-ttu-id="2a2ba-460">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-460">**Why**</span></span>

<span data-ttu-id="2a2ba-461">S vlastněnými entitami nelze manipulovat bez vlastníka, takže v převážné většině případů je dotazování tímto způsobem chybou.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-461">Owned entities cannot be manipulated without the owner, so in the vast majority of cases querying them in this way is an error.</span></span>

<span data-ttu-id="2a2ba-462">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-462">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-463">Pokud by měla být vlastněná entita sledována, aby byla později změněna, měl by být do dotazu zahrnut vlastník.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-463">If the owned entity should be tracked to be modified in any way later then the owner should be included in the query.</span></span>

<span data-ttu-id="2a2ba-464">V opačném `AsNoTracking()` případě přidejte volání:</span><span class="sxs-lookup"><span data-stu-id="2a2ba-464">Otherwise add an `AsNoTracking()` call:</span></span>

```csharp
context.People.Select(p => p.Address).AsNoTracking();
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="2a2ba-465">Zděděné vlastnosti z nemapovaných typů jsou nyní mapovány na jeden sloupec pro všechny odvozené typy.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-465">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="2a2ba-466">Sledování #13998 problému</span><span class="sxs-lookup"><span data-stu-id="2a2ba-466">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="2a2ba-467">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-467">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-468">Zvažte následující model:</span><span class="sxs-lookup"><span data-stu-id="2a2ba-468">Consider the following model:</span></span>
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

<span data-ttu-id="2a2ba-469">Před EF Core 3.0 by `ShippingAddress` vlastnost byla mapována na samostatné sloupce pro `BulkOrder` a `Order` ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-469">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="2a2ba-470">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-470">**New behavior**</span></span>

<span data-ttu-id="2a2ba-471">Počínaje 3.0, EF Core vytvoří `ShippingAddress`pouze jeden sloupec pro .</span><span class="sxs-lookup"><span data-stu-id="2a2ba-471">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="2a2ba-472">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-472">**Why**</span></span>

<span data-ttu-id="2a2ba-473">Starý behavoir byl nečekaný.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-473">The old behavoir was unexpected.</span></span>

<span data-ttu-id="2a2ba-474">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-474">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-475">Vlastnost může být stále explicitně mapována na samostatný sloupec na odvozených typech:</span><span class="sxs-lookup"><span data-stu-id="2a2ba-475">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="2a2ba-476">Konvence vlastností cizího klíče již neodpovídá stejnému názvu jako hlavní vlastnost.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-476">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="2a2ba-477">#13274 problému sledování</span><span class="sxs-lookup"><span data-stu-id="2a2ba-477">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="2a2ba-478">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-478">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-479">Zvažte následující model:</span><span class="sxs-lookup"><span data-stu-id="2a2ba-479">Consider the following model:</span></span>
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
<span data-ttu-id="2a2ba-480">Před EF Core 3.0, `CustomerId` vlastnost by být použity pro cizí klíč podle konvence.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-480">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="2a2ba-481">Pokud `Order` je však vlastněný typ, pak `CustomerId` by to také primární klíč, a to není obvykle očekávání.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-481">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="2a2ba-482">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-482">**New behavior**</span></span>

<span data-ttu-id="2a2ba-483">Počínaje 3.0 EF Core nepokouší použít vlastnosti pro cizí klíče podle konvence, pokud mají stejný název jako hlavní vlastnost.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-483">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="2a2ba-484">Název hlavního typu spojený s názvem hlavní vlastnosti a název navigace spojený se vzory názvů hlavní vlastnosti jsou stále spárovány.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-484">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="2a2ba-485">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2a2ba-485">For example:</span></span>

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

<span data-ttu-id="2a2ba-486">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-486">**Why**</span></span>

<span data-ttu-id="2a2ba-487">Tato změna byla provedena, aby se zabránilo chybné definování vlastnosti primárního klíče na vlastněném typu.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-487">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="2a2ba-488">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-488">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-489">Pokud vlastnost byla určena jako cizí klíč a proto součástí primárního klíče, pak explicitně nakonfigurovat jako takové.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-489">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="2a2ba-490">Připojení databáze je nyní uzavřena, pokud již není použit a před dokončením TransactionScope</span><span class="sxs-lookup"><span data-stu-id="2a2ba-490">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="2a2ba-491">Sledování #14218 problému</span><span class="sxs-lookup"><span data-stu-id="2a2ba-491">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="2a2ba-492">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-492">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-493">Před EF Core 3.0, pokud kontext `TransactionScope`otevře připojení uvnitř , `TransactionScope` připojení zůstane otevřené, zatímco aktuální je aktivní.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-493">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="2a2ba-494">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-494">**New behavior**</span></span>

<span data-ttu-id="2a2ba-495">Počínaje 3.0 EF Core ukončí připojení, jakmile je to hotovo.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-495">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="2a2ba-496">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-496">**Why**</span></span>

<span data-ttu-id="2a2ba-497">Tato změna umožňuje použít více kontextů ve stejném `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-497">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="2a2ba-498">Nové chování také odpovídá EF6.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-498">The new behavior also matches EF6.</span></span>

<span data-ttu-id="2a2ba-499">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-499">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-500">Pokud připojení musí zůstat otevřené `OpenConnection()` explicitní volání zajistí, že EF Core nezavře předčasně:</span><span class="sxs-lookup"><span data-stu-id="2a2ba-500">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="2a2ba-501">Každá vlastnost používá nezávislé generování celého klíče v paměti</span><span class="sxs-lookup"><span data-stu-id="2a2ba-501">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="2a2ba-502">#6872 sledování problému</span><span class="sxs-lookup"><span data-stu-id="2a2ba-502">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="2a2ba-503">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-503">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-504">Před EF Core 3.0 jeden generátor sdílených hodnot byl použit pro všechny vlastnosti klíče v paměti celé číslo.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-504">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="2a2ba-505">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-505">**New behavior**</span></span>

<span data-ttu-id="2a2ba-506">Počínaje EF Core 3.0, každá vlastnost celočíselný klíč získá vlastní generátor hodnot při použití databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-506">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="2a2ba-507">Také pokud je databáze odstraněna, generování klíčů se resetuje pro všechny tabulky.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-507">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="2a2ba-508">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-508">**Why**</span></span>

<span data-ttu-id="2a2ba-509">Tato změna byla provedena za účelem těsnějšího zarovnání generování klíče v paměti s generováním skutečného klíče databáze a zlepšením možnosti izolovat testy od sebe navzájem při použití databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-509">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="2a2ba-510">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-510">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-511">To může přerušit aplikaci, která je závislá na konkrétní hodnoty klíče v paměti, které mají být nastaveny.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-511">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="2a2ba-512">Zvažte místo toho není spoléhat na konkrétní hodnoty klíče nebo aktualizace tak, aby odpovídaly nové chování.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-512">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="2a2ba-513">Ve výchozím nastavení se používají záložní pole.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-513">Backing fields are used by default</span></span>

[<span data-ttu-id="2a2ba-514">Sledování #12430 problému</span><span class="sxs-lookup"><span data-stu-id="2a2ba-514">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="2a2ba-515">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-515">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-516">Před 3.0, i v případě, že bylo známo záložní pole pro vlastnost, EF Core by stále ve výchozím nastavení číst a zapisovat hodnotu vlastnosti pomocí metody getter vlastnost a setter.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-516">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="2a2ba-517">Výjimkou bylo spuštění dotazu, kde by bylo záložní pole nastaveno přímo, pokud je známo.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-517">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="2a2ba-518">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-518">**New behavior**</span></span>

<span data-ttu-id="2a2ba-519">Počínaje EF Core 3.0, pokud je známé záložní pole pro vlastnost, pak EF Core bude vždy číst a zapisovat tuto vlastnost pomocí záložního pole.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-519">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="2a2ba-520">To může způsobit přerušení aplikace, pokud aplikace spoléhá na další chování kódované do metody getter nebo setter.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-520">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="2a2ba-521">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-521">**Why**</span></span>

<span data-ttu-id="2a2ba-522">Tato změna byla provedena, aby se zabránilo EF Core z chybně spouštění obchodní logiky ve výchozím nastavení při provádění databázových operací zahrnujících entity.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-522">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="2a2ba-523">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-523">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-524">Chování pre-3.0 lze obnovit prostřednictvím konfigurace režimu `ModelBuilder`přístupu vlastnosti na .</span><span class="sxs-lookup"><span data-stu-id="2a2ba-524">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="2a2ba-525">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2a2ba-525">For example:</span></span>

```csharp
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="2a2ba-526">Vyvolání, pokud je nalezeno více kompatibilních doprovodových polí</span><span class="sxs-lookup"><span data-stu-id="2a2ba-526">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="2a2ba-527">#12523 sledování problémů</span><span class="sxs-lookup"><span data-stu-id="2a2ba-527">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="2a2ba-528">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-528">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-529">Před EF Core 3.0, pokud více polí uzavřeno pravidla pro hledání záložní pole vlastnosti, pak jedno pole by bylo vybráno na základě některé pořadí priorit.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-529">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="2a2ba-530">To může způsobit nesprávné pole, které mají být použity v nejednoznačných případech.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-530">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="2a2ba-531">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-531">**New behavior**</span></span>

<span data-ttu-id="2a2ba-532">Počínaje EF Core 3.0, pokud více polí jsou spárovány se stejnou vlastností, pak je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-532">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="2a2ba-533">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-533">**Why**</span></span>

<span data-ttu-id="2a2ba-534">Tato změna byla provedena, aby se zabránilo tichépoužití jednoho pole přes jiné, když pouze jeden může být správné.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-534">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="2a2ba-535">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-535">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-536">Vlastnosti s nejednoznačnými opěrnými poli musí mít zadané pole, které má být explicitně určeno.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-536">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="2a2ba-537">Například pomocí fluent API:</span><span class="sxs-lookup"><span data-stu-id="2a2ba-537">For example, using the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="2a2ba-538">Názvy vlastností pouze pro pole by měly odpovídat názvu pole.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-538">Field-only property names should match the field name</span></span>

<span data-ttu-id="2a2ba-539">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-539">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-540">Před EF Core 3.0, vlastnost může být určena řetězcovou hodnotou a pokud žádná vlastnost s tímto názvem byla nalezena na typu .NET pak EF Core by se pokusit porovnat s polem pomocí pravidel konvence.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-540">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the .NET type then EF Core would try to match it to a field using convention rules.</span></span>

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

<span data-ttu-id="2a2ba-541">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-541">**New behavior**</span></span>

<span data-ttu-id="2a2ba-542">Počínaje EF Core 3.0, vlastnost pouze pole musí přesně odpovídat názvu pole.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-542">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="2a2ba-543">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-543">**Why**</span></span>

<span data-ttu-id="2a2ba-544">Tato změna byla provedena, aby se zabránilo použití stejné pole pro dvě vlastnosti pojmenované podobně, také umožňuje odpovídající pravidla pro vlastnosti pouze pole stejné jako pro vlastnosti mapované na clr vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-544">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="2a2ba-545">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-545">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-546">Vlastnosti pouze pro pole musí být pojmenovány stejně jako pole, na které jsou mapovány.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-546">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="2a2ba-547">V budoucí verzi EF Core po 3.0 plánujeme znovu povolit explicitně konfiguraci názvu pole, který se liší od názvu vlastnosti (viz problém [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span><span class="sxs-lookup"><span data-stu-id="2a2ba-547">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="2a2ba-548">AddDbContext/AddDbContextPool již nevolá addlogging a addmemorycache</span><span class="sxs-lookup"><span data-stu-id="2a2ba-548">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="2a2ba-549">#14756 problému sledování</span><span class="sxs-lookup"><span data-stu-id="2a2ba-549">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="2a2ba-550">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-550">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-551">Před EF Core `AddDbContext` 3.0, volání nebo `AddDbContextPool` by také zaregistrovat protokolování a ukládání do mezipaměti služby s DI prostřednictvím volání [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) a [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="2a2ba-551">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with DI through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="2a2ba-552">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-552">**New behavior**</span></span>

<span data-ttu-id="2a2ba-553">Počínaje EF Core `AddDbContext` 3.0 `AddDbContextPool` a již nebude registrovat tyto služby s vkládání závislostí (DI).</span><span class="sxs-lookup"><span data-stu-id="2a2ba-553">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="2a2ba-554">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-554">**Why**</span></span>

<span data-ttu-id="2a2ba-555">EF Core 3.0 nevyžaduje, aby tyto služby jsou v kontejneru DI aplikace.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-555">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="2a2ba-556">Však `ILoggerFactory` pokud je registrována v kontejneru DI aplikace, pak bude stále používán EF Core.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-556">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="2a2ba-557">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-557">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-558">Pokud vaše aplikace potřebuje tyto služby, zaregistrujte je explicitně pomocí kontejneru DI pomocí [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) nebo [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="2a2ba-558">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

### <a name="addentityframework-adds-imemorycache-with-a-size-limit"></a><span data-ttu-id="2a2ba-559">AddEntityFramework\* přidá IMemoryCache s limitem velikosti</span><span class="sxs-lookup"><span data-stu-id="2a2ba-559">AddEntityFramework\* adds IMemoryCache with a size limit</span></span>

[<span data-ttu-id="2a2ba-560">#12905 problému sledování</span><span class="sxs-lookup"><span data-stu-id="2a2ba-560">Tracking Issue #12905</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12905)

<span data-ttu-id="2a2ba-561">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-561">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-562">Před EF Core 3.0 by volající `AddEntityFramework*` metody také zaregistrovat služby ukládání do mezipaměti paměti s DI bez omezení velikosti.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-562">Before EF Core 3.0, calling `AddEntityFramework*` methods would also register memory caching services with DI without a size limit.</span></span>

<span data-ttu-id="2a2ba-563">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-563">**New behavior**</span></span>

<span data-ttu-id="2a2ba-564">Počínaje EF Core 3.0, `AddEntityFramework*` zaregistruje službu IMemoryCache s limitem velikosti.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-564">Starting with EF Core 3.0, `AddEntityFramework*` will register an IMemoryCache service with a size limit.</span></span> <span data-ttu-id="2a2ba-565">Pokud některé další služby přidané později závisí na IMemoryCache mohou rychle dosáhnout výchozího limitu způsobuje výjimky nebo snížený výkon.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-565">If any other services added afterwards depend on IMemoryCache they can quickly reach the default limit causing exceptions or degraded performance.</span></span>

<span data-ttu-id="2a2ba-566">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-566">**Why**</span></span>

<span data-ttu-id="2a2ba-567">Použití IMemoryCache bez omezení může mít za následek nekontrolované využití paměti, pokud je chyba v dotazu ukládání do mezipaměti logiky nebo dotazy jsou generovány dynamicky.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-567">Using IMemoryCache without a limit could result in uncontrolled memory usage if there is a bug in query caching logic or the queries are generated dynamically.</span></span> <span data-ttu-id="2a2ba-568">Výchozí limit zmírňuje potenciální útok DoS.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-568">Having a default limit mitigates a potential DoS attack.</span></span>

<span data-ttu-id="2a2ba-569">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-569">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-570">Ve většině `AddEntityFramework*` případů volání `AddDbContext` není `AddDbContextPool` nutné, pokud nebo je volána také.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-570">In most cases calling `AddEntityFramework*` is not necessary if `AddDbContext` or `AddDbContextPool` is called as well.</span></span> <span data-ttu-id="2a2ba-571">Proto nejlepší zmírnění je odebrat `AddEntityFramework*` volání.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-571">Therefore, the best mitigation is to remove the `AddEntityFramework*` call.</span></span>

<span data-ttu-id="2a2ba-572">Pokud vaše aplikace potřebuje tyto služby, zaregistrujte implementaci IMemoryCache explicitně s kontejnerem DI předem pomocí [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="2a2ba-572">If your application needs these services, then register a IMemoryCache implementation explicitly with the DI container beforehand using [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="2a2ba-573">DbContext.Entry nyní provádí místní DetectChanges</span><span class="sxs-lookup"><span data-stu-id="2a2ba-573">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="2a2ba-574">Sledování #13552 problému</span><span class="sxs-lookup"><span data-stu-id="2a2ba-574">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="2a2ba-575">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-575">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-576">Před EF Core 3.0 volání `DbContext.Entry` by způsobit změny, které mají být zjištěny pro všechny sledované entity.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-576">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="2a2ba-577">To zajistilo, že `EntityEntry` stav vystavený v aktuálním stavu.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-577">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="2a2ba-578">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-578">**New behavior**</span></span>

<span data-ttu-id="2a2ba-579">Počínaje EF Core 3.0 `DbContext.Entry` volání se nyní pokusí pouze zjistit změny v dané entitě a všechny sledované hlavní entity s ním související.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-579">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="2a2ba-580">To znamená, že změny jinde nemusí být zjištěny voláním této metody, což může mít vliv na stav aplikace.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-580">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="2a2ba-581">Všimněte `ChangeTracker.AutoDetectChangesEnabled` si, `false` že pokud je nastavena na pak i toto místní zjišťování změn bude zakázáno.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-581">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="2a2ba-582">Jiné metody, které způsobují `ChangeTracker.Entries` zjišťování `SaveChanges`změn-- například a --stále způsobují plné `DetectChanges` všech sledovaných entit.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-582">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="2a2ba-583">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-583">**Why**</span></span>

<span data-ttu-id="2a2ba-584">Tato změna byla provedena ke zlepšení `context.Entry`výchozího výkonu použití .</span><span class="sxs-lookup"><span data-stu-id="2a2ba-584">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="2a2ba-585">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-585">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-586">Volání `ChangeTracker.DetectChanges()` explicitně `Entry` před voláním zajistit pre-3.0 chování.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-586">Call `ChangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="2a2ba-587">Klíče pole řetězce a bajtů nejsou ve výchozím nastavení generovány klientem.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-587">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="2a2ba-588">Sledování #14617 problému</span><span class="sxs-lookup"><span data-stu-id="2a2ba-588">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="2a2ba-589">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-589">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-590">Před EF Core 3.0 `string` a `byte[]` vlastnosti klíče lze použít bez explicitního nastavení hodnoty bez null.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-590">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="2a2ba-591">V takovém případě by hodnota klíče byla generována na straně klienta jako identifikátor `byte[]`GUID serializovaný na bajty pro .</span><span class="sxs-lookup"><span data-stu-id="2a2ba-591">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="2a2ba-592">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-592">**New behavior**</span></span>

<span data-ttu-id="2a2ba-593">Počínaje EF Core 3.0 bude vyvolána výjimka označující, že nebyla nastavena žádná hodnota klíče.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-593">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="2a2ba-594">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-594">**Why**</span></span>

<span data-ttu-id="2a2ba-595">Tato změna byla provedena, `string` / `byte[]` protože klient generované hodnoty obecně nejsou užitečné a výchozí chování ztěžovalo důvod o generovaných hodnotklíče běžným způsobem.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-595">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="2a2ba-596">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-596">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-597">Chování pre-3.0 lze získat explicitním zadáním, že vlastnosti klíče by měly používat generované hodnoty, pokud není nastavena žádná jiná hodnota bez nuly.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-597">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="2a2ba-598">Například s fluent API:</span><span class="sxs-lookup"><span data-stu-id="2a2ba-598">For example, with the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="2a2ba-599">Nebo s poznámkami o datech:</span><span class="sxs-lookup"><span data-stu-id="2a2ba-599">Or with data annotations:</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="2a2ba-600">ILoggerFactory je nyní oborová služba</span><span class="sxs-lookup"><span data-stu-id="2a2ba-600">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="2a2ba-601">#14698 problému sledování</span><span class="sxs-lookup"><span data-stu-id="2a2ba-601">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="2a2ba-602">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-602">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-603">Před EF Core 3.0, `ILoggerFactory` byl registrován jako singleton služby.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-603">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="2a2ba-604">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-604">**New behavior**</span></span>

<span data-ttu-id="2a2ba-605">Počínaje EF Core 3.0, `ILoggerFactory` je nyní registrována jako vymezené.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-605">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="2a2ba-606">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-606">**Why**</span></span>

<span data-ttu-id="2a2ba-607">Tato změna byla provedena tak, aby `DbContext` přidružení protokolování s instancí, která umožňuje další funkce a odstraňuje některé případy patologického chování, jako je například rozpad interních poskytovatelů služeb.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-607">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="2a2ba-608">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-608">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-609">Tato změna by neměla mít vliv na kód aplikace, pokud není registrace a používání vlastních služeb na zprostředkovatele interních služeb EF Core.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-609">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="2a2ba-610">To není běžné.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-610">This isn't common.</span></span>
<span data-ttu-id="2a2ba-611">V těchto případech bude většina věcí stále fungovat, ale `ILoggerFactory` jakákoli služba singleton, `ILoggerFactory` která byla závislá na, bude muset být změněna, aby získala jiným způsobem.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-611">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="2a2ba-612">Pokud narazíte na situace, jako je tato, podejte prosím problém na EF Core `ILoggerFactory` [GitHub problém tracker,](https://github.com/aspnet/EntityFrameworkCore/issues) dejte nám vědět, jak používáte tak, že můžeme lépe pochopit, jak to v budoucnu znovu nerozbít.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-612">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="2a2ba-613">Opožděné načítání proxy servery již předpokládat, navigační vlastnosti jsou plně načteny</span><span class="sxs-lookup"><span data-stu-id="2a2ba-613">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="2a2ba-614">Sledování #12780 problému</span><span class="sxs-lookup"><span data-stu-id="2a2ba-614">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="2a2ba-615">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-615">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-616">Před EF Core 3.0, jakmile `DbContext` byl vyřazen neexistuje žádný způsob, jak zjistit, zda dané navigační vlastnost na entitu získané z tohoto kontextu byla plně načtena nebo ne.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-616">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="2a2ba-617">Proxy servery by místo toho předpokládat, že navigace odkazu je načten, pokud má hodnotu non-null a že navigace kolekce je načten, pokud není prázdný.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-617">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="2a2ba-618">V těchto případech pokus o opožděné zatížení by bez operace.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-618">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="2a2ba-619">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-619">**New behavior**</span></span>

<span data-ttu-id="2a2ba-620">Počínaje EF Core 3.0 proxy sledovat, zda je načten navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-620">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="2a2ba-621">To znamená, že pokus o přístup k navigační vlastnosti, která je načtena po zpřístupnění kontextu bude vždy no-op, i když načtená navigace je prázdná nebo null.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-621">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="2a2ba-622">Naopak pokus o přístup k navigační vlastnost, která není načten vyvolá výjimku, pokud je uvolněn kontext i v případě, že navigační vlastnost je neprázdná kolekce.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-622">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="2a2ba-623">Pokud k této situaci nastane, znamená to, že kód aplikace se pokouší použít opožděné načítání v neplatný čas a aplikace by měla být změněna tak, aby to neudělala.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-623">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="2a2ba-624">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-624">**Why**</span></span>

<span data-ttu-id="2a2ba-625">Tato změna byla provedena tak, aby chování konzistentní a správné při `DbContext` pokusu opožděné zatížení na vyřazené instance.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-625">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="2a2ba-626">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-626">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-627">Aktualizujte kód aplikace tak, aby se nepokoušel opožděné načítání s vyřazeným kontextem, nebo nakonfigurujte tento kód jako no-op, jak je popsáno ve zprávě o výjimce.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-627">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="2a2ba-628">Nadměrné vytváření interních poskytovatelů služeb je nyní ve výchozím nastavení chybou</span><span class="sxs-lookup"><span data-stu-id="2a2ba-628">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="2a2ba-629">Sledování #10236 problému</span><span class="sxs-lookup"><span data-stu-id="2a2ba-629">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="2a2ba-630">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-630">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-631">Před EF Core 3.0 by bylo zaznamenáno upozornění pro aplikaci, která vytváří patologický počet interních poskytovatelů služeb.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-631">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="2a2ba-632">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-632">**New behavior**</span></span>

<span data-ttu-id="2a2ba-633">Počínaje EF Core 3.0, toto upozornění je nyní považováno za a je vyvolána chyba a je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-633">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="2a2ba-634">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-634">**Why**</span></span>

<span data-ttu-id="2a2ba-635">Tato změna byla provedena řídit lepší kód aplikace prostřednictvím vystavení tohoto patologického případu explicitněji.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-635">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="2a2ba-636">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-636">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-637">Nejvhodnější příčinou akce při výskytu této chyby je pochopit hlavní příčinu a zastavit vytváření tolik interních poskytovatelů služeb.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-637">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="2a2ba-638">Chybu však lze převést zpět na upozornění (nebo ignorovány) prostřednictvím konfigurace na `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-638">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="2a2ba-639">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2a2ba-639">For example:</span></span>

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="2a2ba-640">Nové chování pro HasOne/HasMany volána s jedním řetězcem</span><span class="sxs-lookup"><span data-stu-id="2a2ba-640">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="2a2ba-641">Sledování #9171 problému</span><span class="sxs-lookup"><span data-stu-id="2a2ba-641">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="2a2ba-642">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-642">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-643">Před EF Core 3.0, volání `HasOne` kódu nebo `HasMany` s jedním řetězcem byla interpretována matoucím způsobem.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-643">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="2a2ba-644">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2a2ba-644">For example:</span></span>
```csharp
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="2a2ba-645">Kód vypadá, že se `Samurai` týká některého jiného `Entrance` typu entity pomocí navigační vlastnosti, která může být soukromá.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-645">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="2a2ba-646">Ve skutečnosti se tento kód pokusí vytvořit vztah `Entrance` k nějakému typu entity volané bez navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-646">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="2a2ba-647">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-647">**New behavior**</span></span>

<span data-ttu-id="2a2ba-648">Počínaje EF Core 3.0, kód výše nyní dělá to, co to vypadalo, že by měl dělat dříve.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-648">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="2a2ba-649">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-649">**Why**</span></span>

<span data-ttu-id="2a2ba-650">Staré chování bylo velmi matoucí, zejména při čtení konfiguračního kódu a hledání chyb.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-650">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="2a2ba-651">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-651">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-652">Tím se přeruší pouze aplikace, které explicitně konfigurují vztahy pomocí řetězců pro názvy typů a bez explicitního zadání vlastnosti navigace.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-652">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="2a2ba-653">To není běžné.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-653">This is not common.</span></span>
<span data-ttu-id="2a2ba-654">Předchozí chování lze získat prostřednictvím explicitní předávání `null` pro název vlastnosti navigace.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-654">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="2a2ba-655">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2a2ba-655">For example:</span></span>

```csharp
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="2a2ba-656">Návratový typ pro několik asynchronních metod byl změněn z Task na ValueTask.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-656">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="2a2ba-657">Sledování #15184 problému</span><span class="sxs-lookup"><span data-stu-id="2a2ba-657">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="2a2ba-658">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-658">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-659">Následující asynchronní metody `Task<T>`dříve vrátily :</span><span class="sxs-lookup"><span data-stu-id="2a2ba-659">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="2a2ba-660">`ValueGenerator.NextValueAsync()`(a odvozené třídy)</span><span class="sxs-lookup"><span data-stu-id="2a2ba-660">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="2a2ba-661">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-661">**New behavior**</span></span>

<span data-ttu-id="2a2ba-662">Výše uvedené metody nyní vrátit `ValueTask<T>` více než `T` stejné jako dříve.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-662">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="2a2ba-663">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-663">**Why**</span></span>

<span data-ttu-id="2a2ba-664">Tato změna snižuje počet přidělení haldy vzniklé při vyvolání těchto metod, zlepšení obecného výkonu.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-664">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="2a2ba-665">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-665">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-666">Aplikace, které jednoduše čekají na výše uvedená api, je třeba pouze překompilovat - nejsou nutné žádné změny zdroje.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-666">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="2a2ba-667">Složitější použití (např. předání vrácené `Task` `Task.WhenAny()`do) obvykle vyžaduje, `ValueTask<T>` aby vrácené `Task<T>` převedeny na volání `AsTask()` na něj.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-667">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="2a2ba-668">Všimněte si, že to neguje snížení přidělení, které tato změna přináší.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-668">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="2a2ba-669">Relační a textová poznámka je nyní pouze typemapping</span><span class="sxs-lookup"><span data-stu-id="2a2ba-669">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="2a2ba-670">Sledování #9913 problému</span><span class="sxs-lookup"><span data-stu-id="2a2ba-670">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="2a2ba-671">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-671">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-672">Název poznámky pro poznámky mapování typů byl "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="2a2ba-672">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="2a2ba-673">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-673">**New behavior**</span></span>

<span data-ttu-id="2a2ba-674">Název poznámky pro poznámky mapování typů je nyní "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="2a2ba-674">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="2a2ba-675">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-675">**Why**</span></span>

<span data-ttu-id="2a2ba-676">Mapování typů se nyní používá pro více než jen zprostředkovatelé relační databáze.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-676">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="2a2ba-677">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-677">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-678">Tím se pouze poruší aplikace, které přistupují k mapování typů přímo jako poznámku, která není běžná.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-678">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="2a2ba-679">Nejvhodnější akcí k opravě je použití povrchu rozhraní API pro přístup k mapování typu, nikoli k přímému použití poznámky.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-679">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="2a2ba-680">ToTable na odvozený typ vyvolá výjimku</span><span class="sxs-lookup"><span data-stu-id="2a2ba-680">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="2a2ba-681">Sledování #11811 problému</span><span class="sxs-lookup"><span data-stu-id="2a2ba-681">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="2a2ba-682">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-682">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-683">Před EF Core 3.0, `ToTable()` volal na odvozený typ by být ignorována, protože pouze mapování dědičnosti strategie byla TPH, kde to není platný.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-683">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="2a2ba-684">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-684">**New behavior**</span></span>

<span data-ttu-id="2a2ba-685">Počínaje EF Core 3.0 a v rámci přípravy na přidání `ToTable()` podpory TPT a TPC v novější verzi, volal na odvozený typ bude nyní vyvolat výjimku, aby se zabránilo neočekávané změny mapování v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-685">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="2a2ba-686">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-686">**Why**</span></span>

<span data-ttu-id="2a2ba-687">V současné době není platný mapovat odvozený typ do jiné tabulky.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-687">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="2a2ba-688">Tato změna se zabrání lámání v budoucnu, když se stane platnou věc udělat.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-688">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="2a2ba-689">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-689">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-690">Odeberte všechny pokusy o mapování odvozených typů do jiných tabulek.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-690">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="2a2ba-691">ForSqlServerHasIndex nahrazen HasIndex</span><span class="sxs-lookup"><span data-stu-id="2a2ba-691">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="2a2ba-692">Sledování #12366</span><span class="sxs-lookup"><span data-stu-id="2a2ba-692">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="2a2ba-693">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-693">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-694">Před EF Core 3.0, za předpokladu, že způsob, `ForSqlServerHasIndex().ForSqlServerInclude()` jak nakonfigurovat sloupce používané s `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-694">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="2a2ba-695">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-695">**New behavior**</span></span>

<span data-ttu-id="2a2ba-696">Počínaje EF Core 3.0, pomocí `Include` na indexu je nyní podporována na relační úrovni.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-696">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="2a2ba-697">Použijte `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-697">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="2a2ba-698">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-698">**Why**</span></span>

<span data-ttu-id="2a2ba-699">Tato změna byla provedena ke konsolidaci `Include` rozhraní API pro indexy s na jednom místě pro všechny poskytovatele databáze.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-699">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="2a2ba-700">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-700">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-701">Použijte nové rozhraní API, jak je znázorněno výše.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-701">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="2a2ba-702">Změny rozhraní API metadat</span><span class="sxs-lookup"><span data-stu-id="2a2ba-702">Metadata API changes</span></span>

[<span data-ttu-id="2a2ba-703">Sledování #214 problému</span><span class="sxs-lookup"><span data-stu-id="2a2ba-703">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="2a2ba-704">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-704">**New behavior**</span></span>

<span data-ttu-id="2a2ba-705">Následující vlastnosti byly převedeny na metody rozšíření:</span><span class="sxs-lookup"><span data-stu-id="2a2ba-705">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="2a2ba-706">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-706">**Why**</span></span>

<span data-ttu-id="2a2ba-707">Tato změna zjednodušuje implementaci výše uvedených rozhraní.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-707">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="2a2ba-708">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-708">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-709">Použijte nové metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-709">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="2a2ba-710">Změny rozhraní METADAT SPECIFICKÉ Pro zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="2a2ba-710">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="2a2ba-711">Sledování #214 problému</span><span class="sxs-lookup"><span data-stu-id="2a2ba-711">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="2a2ba-712">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-712">**New behavior**</span></span>

<span data-ttu-id="2a2ba-713">Metody rozšíření specifické pro zprostředkovatele budou srovnány se sloučí:</span><span class="sxs-lookup"><span data-stu-id="2a2ba-713">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="2a2ba-714">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-714">**Why**</span></span>

<span data-ttu-id="2a2ba-715">Tato změna zjednodušuje implementaci výše uvedených metod rozšíření.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-715">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="2a2ba-716">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-716">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-717">Použijte nové metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-717">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="2a2ba-718">EF Core již neodesílá pragma pro vynucení SQLite FK</span><span class="sxs-lookup"><span data-stu-id="2a2ba-718">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="2a2ba-719">Sledování #12151 problému</span><span class="sxs-lookup"><span data-stu-id="2a2ba-719">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="2a2ba-720">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-720">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-721">Před EF Core 3.0 EF `PRAGMA foreign_keys = 1` Core by odeslat při otevření připojení k SQLite.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-721">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="2a2ba-722">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-722">**New behavior**</span></span>

<span data-ttu-id="2a2ba-723">Počínaje EF Core 3.0 EF Core `PRAGMA foreign_keys = 1` již odešle při otevření připojení k SQLite.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-723">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="2a2ba-724">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-724">**Why**</span></span>

<span data-ttu-id="2a2ba-725">Tato změna byla provedena, `SQLitePCLRaw.bundle_e_sqlite3` protože EF Core používá ve výchozím nastavení, což zase znamená, že vynucení FK je ve výchozím nastavení zapnuto a nemusí být explicitně povoleno při každém otevření připojení.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-725">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="2a2ba-726">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-726">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-727">Cizí klíče jsou ve výchozím nastavení povoleny v souboru SQLitePCLRaw.bundle_e_sqlite3, který se ve výchozím nastavení používá pro EF Core.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-727">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="2a2ba-728">V ostatních případech lze povolit `Foreign Keys=True` cizí klíče zadáním v připojovacím řetězci.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-728">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="2a2ba-729">Microsoft.EntityFrameworkCore.Sqlite nyní závisí na SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="2a2ba-729">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="2a2ba-730">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-730">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-731">Před EF Core 3.0 `SQLitePCLRaw.bundle_green`použil EF Core .</span><span class="sxs-lookup"><span data-stu-id="2a2ba-731">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="2a2ba-732">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-732">**New behavior**</span></span>

<span data-ttu-id="2a2ba-733">Počínaje EF Core 3.0, `SQLitePCLRaw.bundle_e_sqlite3`EF Core používá .</span><span class="sxs-lookup"><span data-stu-id="2a2ba-733">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="2a2ba-734">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-734">**Why**</span></span>

<span data-ttu-id="2a2ba-735">Tato změna byla provedena tak, aby verze SQLite používané v systému iOS v souladu s jinými platformami.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-735">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="2a2ba-736">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-736">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-737">Chcete-li použít nativní verzi SQLite `Microsoft.Data.Sqlite` v systému iOS, nakonfigurujte použití jiného `SQLitePCLRaw` balíčku.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-737">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="2a2ba-738">Hodnoty guid jsou nyní uloženy jako TEXT na SQLite</span><span class="sxs-lookup"><span data-stu-id="2a2ba-738">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="2a2ba-739">Sledování #15078 problému</span><span class="sxs-lookup"><span data-stu-id="2a2ba-739">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="2a2ba-740">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-740">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-741">Guid hodnoty byly dříve uloženy jako hodnoty BLOB na SQLite.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-741">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="2a2ba-742">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-742">**New behavior**</span></span>

<span data-ttu-id="2a2ba-743">Hodnoty guid jsou nyní uloženy jako TEXT.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-743">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="2a2ba-744">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-744">**Why**</span></span>

<span data-ttu-id="2a2ba-745">Binární formát identifikátorů Guids není standardizován.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-745">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="2a2ba-746">Ukládání hodnot jako TEXT umožňuje databázi více kompatibilní s jinými technologiemi.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-746">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="2a2ba-747">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-747">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-748">Existující databáze můžete migrovat do nového formátu spuštěním sql jako následující.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-748">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="2a2ba-749">V EF Core můžete také pokračovat v používání předchozí chování konfigurací převaděče hodnot na tyto vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-749">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="2a2ba-750">Microsoft.Data.Sqlite zůstává schopen číst guid hodnoty z blob a TEXT sloupce; vzhledem k tomu, že výchozí formát parametrů a konstant se však změnil, budete pravděpodobně muset provést akci pro většinu scénářů zahrnujících identifikátory GUID.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-750">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="2a2ba-751">Hodnoty Char jsou nyní uloženy jako TEXT na SQLite</span><span class="sxs-lookup"><span data-stu-id="2a2ba-751">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="2a2ba-752">Sledování #15020 problému</span><span class="sxs-lookup"><span data-stu-id="2a2ba-752">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="2a2ba-753">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-753">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-754">Char hodnoty byly dříve sored jako integer hodnoty na SQLite.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-754">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="2a2ba-755">Například hodnota char *A* byla uložena jako celá hodnota 65.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-755">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="2a2ba-756">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-756">**New behavior**</span></span>

<span data-ttu-id="2a2ba-757">Hodnoty Char jsou nyní uloženy jako TEXT.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-757">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="2a2ba-758">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-758">**Why**</span></span>

<span data-ttu-id="2a2ba-759">Ukládání hodnot jako TEXT je přirozenější a databáze je více kompatibilní s jinými technologiemi.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-759">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="2a2ba-760">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-760">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-761">Existující databáze můžete migrovat do nového formátu spuštěním sql jako následující.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-761">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="2a2ba-762">V EF Core můžete také pokračovat v používání předchozí chování konfigurací převaděče hodnot na tyto vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-762">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="2a2ba-763">Microsoft.Data.Sqlite také zůstává schopen číst hodnoty znaků z obou INTEGER a TEXT sloupce, takže některé scénáře nemusí vyžadovat žádnou akci.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-763">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="2a2ba-764">ID migrace jsou nyní generovány pomocí kalendáře invariantní jazykové verze</span><span class="sxs-lookup"><span data-stu-id="2a2ba-764">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="2a2ba-765">Sledování #12978 problému</span><span class="sxs-lookup"><span data-stu-id="2a2ba-765">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="2a2ba-766">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-766">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-767">ID migrace byly neúmyslně generovány pomocí kalendáře aktuální jazykové verze.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-767">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="2a2ba-768">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-768">**New behavior**</span></span>

<span data-ttu-id="2a2ba-769">ID migrace jsou nyní vždy generovány pomocí kalendáře invariantní jazykové verze (gregoriánský).</span><span class="sxs-lookup"><span data-stu-id="2a2ba-769">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="2a2ba-770">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-770">**Why**</span></span>

<span data-ttu-id="2a2ba-771">Pořadí migrace je důležité při aktualizaci databáze nebo řešení konfliktů sloučení.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-771">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="2a2ba-772">Použití invariantní kalendář vyhýbá řazení problémy, které mohou vyplývat z členů týmu, které mají různé systémové kalendáře.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-772">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="2a2ba-773">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-773">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-774">Tato změna ovlivní každého, kdo používá kalendář mimo gregoriánský, kde je rok větší než gregoriánský kalendář (například thajský buddhistický kalendář).</span><span class="sxs-lookup"><span data-stu-id="2a2ba-774">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="2a2ba-775">Existující ID migrace bude nutné aktualizovat tak, aby nové migrace jsou seřazeny po existující migrace.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-775">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="2a2ba-776">ID migrace lze nalézt v atributu Migrace v souborech návrháře migrace.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-776">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="2a2ba-777">Tabulka historie migrace je také třeba aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-777">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="2a2ba-778">UseRowNumberForPaging byl odebrán.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-778">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="2a2ba-779">Sledování #16400 problému</span><span class="sxs-lookup"><span data-stu-id="2a2ba-779">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="2a2ba-780">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-780">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-781">Před EF Core 3.0, `UseRowNumberForPaging` lze použít ke generování SQL pro stránkování, který je kompatibilní s SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-781">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="2a2ba-782">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-782">**New behavior**</span></span>

<span data-ttu-id="2a2ba-783">Počínaje EF Core 3.0 ef bude generovat pouze SQL pro stránkování, který je kompatibilní pouze s novějšími verzemi SERVERU SQL.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-783">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="2a2ba-784">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-784">**Why**</span></span>

<span data-ttu-id="2a2ba-785">Tuto změnu provádíme, protože [SQL Server 2008 již není podporovaným produktem](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) a aktualizace této funkce pro práci se změnami dotazu provedenými v EF Core 3.0 je významná práce.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-785">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="2a2ba-786">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-786">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-787">Doporučujeme aktualizovat na novější verzi SERVERU SQL Nebo pomocí vyšší úrovně kompatibility, aby byl podporován generovaný SQL.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-787">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="2a2ba-788">Jak bylo řečeno, pokud to nemůžete udělat, pak prosím [komentujte problém se sledováním](https://github.com/aspnet/EntityFrameworkCore/issues/16400) s podrobnostmi.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-788">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="2a2ba-789">Toto rozhodnutí můžeme přehodnotit na základě zpětné vazby.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-789">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="2a2ba-790">Informace/metadata rozšíření byly odebrány z rozšíření IDbContextOptionsExtension.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-790">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="2a2ba-791">#16119 problému sledování</span><span class="sxs-lookup"><span data-stu-id="2a2ba-791">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="2a2ba-792">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-792">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-793">`IDbContextOptionsExtension`obsažené metody pro poskytování metadat o rozšíření.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-793">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="2a2ba-794">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-794">**New behavior**</span></span>

<span data-ttu-id="2a2ba-795">Tyto metody byly přesunuty `DbContextOptionsExtensionInfo` do nové abstraktní základní třídy, která je vrácena z nové `IDbContextOptionsExtension.Info` vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-795">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="2a2ba-796">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-796">**Why**</span></span>

<span data-ttu-id="2a2ba-797">Během vydání od 2.0 do 3.0 jsme potřebovali přidat nebo změnit tyto metody několikrát.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-797">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="2a2ba-798">Jejich rozdělení do nové abstraktní základní třídy usnadní provádění těchto změn bez přerušení existujících rozšíření.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-798">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="2a2ba-799">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-799">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-800">Aktualizujte rozšíření tak, aby sledovala nový vzor.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-800">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="2a2ba-801">Příklady jsou nalezeny v `IDbContextOptionsExtension` mnoha implementací pro různé druhy rozšíření ve zdrojovém kódu EF Core.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-801">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="2a2ba-802">LogQueryPossibleExceptionWithAggregateOperator byl přejmenován.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-802">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="2a2ba-803">Sledování #10985 problému</span><span class="sxs-lookup"><span data-stu-id="2a2ba-803">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="2a2ba-804">**Změnit**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-804">**Change**</span></span>

<span data-ttu-id="2a2ba-805">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator`byl přejmenován `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`na .</span><span class="sxs-lookup"><span data-stu-id="2a2ba-805">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="2a2ba-806">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-806">**Why**</span></span>

<span data-ttu-id="2a2ba-807">Zarovná pojmenování této události upozornění se všemi ostatními událostmi upozornění.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-807">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="2a2ba-808">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-808">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-809">Použijte nový název.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-809">Use the new name.</span></span> <span data-ttu-id="2a2ba-810">(Všimněte si, že číslo ID události se nezměnilo.)</span><span class="sxs-lookup"><span data-stu-id="2a2ba-810">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="2a2ba-811">Objasnit rozhraní API pro názvy omezení cizího klíče</span><span class="sxs-lookup"><span data-stu-id="2a2ba-811">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="2a2ba-812">Sledování #10730 problému</span><span class="sxs-lookup"><span data-stu-id="2a2ba-812">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="2a2ba-813">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-813">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-814">Před EF Core 3.0, názvy omezení cizího klíče byly označovány jako jednoduše "název".</span><span class="sxs-lookup"><span data-stu-id="2a2ba-814">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="2a2ba-815">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2a2ba-815">For example:</span></span>

```csharp
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="2a2ba-816">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-816">**New behavior**</span></span>

<span data-ttu-id="2a2ba-817">Počínaje EF Core 3.0, názvy omezení cizího klíče jsou nyní označovány jako "název omezení".</span><span class="sxs-lookup"><span data-stu-id="2a2ba-817">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="2a2ba-818">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2a2ba-818">For example:</span></span>

```csharp
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="2a2ba-819">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-819">**Why**</span></span>

<span data-ttu-id="2a2ba-820">Tato změna přináší konzistenci pojmenování v této oblasti a také objasňuje, že se jedná o název omezení cizího klíče a nikoli název sloupce nebo vlastnosti, na který je cizí klíč definován.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-820">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="2a2ba-821">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-821">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-822">Použijte nový název.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-822">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="2a2ba-823">IRelationalDatabaseCreator.HasTables/HasTablesAsync byly zveřejněny</span><span class="sxs-lookup"><span data-stu-id="2a2ba-823">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="2a2ba-824">Sledování #15997 problému</span><span class="sxs-lookup"><span data-stu-id="2a2ba-824">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="2a2ba-825">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-825">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-826">Před EF Core 3.0 byly tyto metody chráněny.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-826">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="2a2ba-827">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-827">**New behavior**</span></span>

<span data-ttu-id="2a2ba-828">Počínaje EF Core 3.0 jsou tyto metody veřejné.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-828">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="2a2ba-829">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-829">**Why**</span></span>

<span data-ttu-id="2a2ba-830">Tyto metody jsou používány EF k určení, pokud je databáze vytvořena, ale prázdné.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-830">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="2a2ba-831">To může být také užitečné z mimo EF při určování, zda použít migrace.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-831">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="2a2ba-832">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-832">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-833">Změňte přístupnost všech přepsání.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-833">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="2a2ba-834">Microsoft.EntityFrameworkCore.Design je nyní balíček DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="2a2ba-834">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="2a2ba-835">Sledování #11506 problému</span><span class="sxs-lookup"><span data-stu-id="2a2ba-835">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="2a2ba-836">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-836">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-837">Před EF Core 3.0, Microsoft.EntityFrameworkCore.Design byl pravidelný Balíček NuGet, jehož sestavení může být odkazováno projekty, které na něm závisely.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-837">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="2a2ba-838">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-838">**New behavior**</span></span>

<span data-ttu-id="2a2ba-839">Počínaje EF Core 3.0, je balíček DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-839">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="2a2ba-840">To znamená, že závislost nebude toku přechodně do jiných projektů a že již nelze ve výchozím nastavení odkazovat na jeho sestavení.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-840">This means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="2a2ba-841">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-841">**Why**</span></span>

<span data-ttu-id="2a2ba-842">Tento balíček je určen pouze k použití v době návrhu.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-842">This package is only intended to be used at design time.</span></span> <span data-ttu-id="2a2ba-843">Nasazené aplikace by na něj neměly odkazovat.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-843">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="2a2ba-844">Vytvoření balíčku DevelopmentDependency posiluje toto doporučení.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-844">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="2a2ba-845">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-845">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-846">Pokud potřebujete odkazovat na tento balíček přepsat EF Core chování návrhu, pak můžete aktualizovat PackageReference metadata položky ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-846">If you need to reference this package to override EF Core's design-time behavior, then you can update PackageReference item metadata in your project.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<span data-ttu-id="2a2ba-847">Pokud je balíček odkazován přechodně prostřednictvím Microsoft.EntityFrameworkCore.Tools, budete muset přidat explicitní PackageReference do balíčku změnit jeho metadata.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-847">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span> <span data-ttu-id="2a2ba-848">Takový explicitní odkaz musí být přidán do každého projektu, kde jsou potřebné typy z balíčku.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-848">Such an explicit reference must be added to any project where the types from the package are needed.</span></span>

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="2a2ba-849">SQLitePCL.raw aktualizován na verzi 2.0.0</span><span class="sxs-lookup"><span data-stu-id="2a2ba-849">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="2a2ba-850">Sledování #14824 problému</span><span class="sxs-lookup"><span data-stu-id="2a2ba-850">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="2a2ba-851">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-851">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-852">Microsoft.EntityFrameworkCore.Sqlite dříve závisel na verzi 1.1.12 SQLitePCL.raw.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-852">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="2a2ba-853">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-853">**New behavior**</span></span>

<span data-ttu-id="2a2ba-854">Aktualizovali jsme náš balíček tak, aby závisel na verzi 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-854">We've updated our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="2a2ba-855">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-855">**Why**</span></span>

<span data-ttu-id="2a2ba-856">Verze 2.0.0 cílů SQLitePCL.raw .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-856">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="2a2ba-857">Dříve se zaměřila na standard .NET Standard 1.1, který vyžadoval rozsáhlé uzavření přenositých balíčků.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-857">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="2a2ba-858">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-858">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-859">SQLitePCL.raw verze 2.0.0 obsahuje některé změny.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-859">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="2a2ba-860">Podrobnosti najdete v [poznámkách k verzi.](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md)</span><span class="sxs-lookup"><span data-stu-id="2a2ba-860">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="2a2ba-861">NetTopologySuite aktualizován na verzi 2.0.0</span><span class="sxs-lookup"><span data-stu-id="2a2ba-861">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="2a2ba-862">Sledování #14825 problému</span><span class="sxs-lookup"><span data-stu-id="2a2ba-862">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="2a2ba-863">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-863">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-864">Prostorové balíčky dříve závisely na verzi 1.15.1 NetTopologySuite.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-864">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="2a2ba-865">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-865">**New behavior**</span></span>

<span data-ttu-id="2a2ba-866">Aktualizovali jsme náš balíček tak, aby závisel na verzi 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-866">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="2a2ba-867">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-867">**Why**</span></span>

<span data-ttu-id="2a2ba-868">Verze 2.0.0 NetTopologySuite si klade za cíl řešit několik problémů použitelnosti, s nimiž se setkávají uživatelé EF Core.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-868">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="2a2ba-869">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-869">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-870">NetTopologySuite verze 2.0.0 obsahuje některé změny.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-870">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="2a2ba-871">Podrobnosti najdete v [poznámkách k verzi.](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001)</span><span class="sxs-lookup"><span data-stu-id="2a2ba-871">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="SqlClient"></a>

### <a name="microsoftdatasqlclient-is-used-instead-of-systemdatasqlclient"></a><span data-ttu-id="2a2ba-872">Místo klienta System.Data.SqlClient se používá místo klienta System.Data.SqlClient</span><span class="sxs-lookup"><span data-stu-id="2a2ba-872">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>

[<span data-ttu-id="2a2ba-873">Sledování #15636 problému</span><span class="sxs-lookup"><span data-stu-id="2a2ba-873">Tracking Issue #15636</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15636)

<span data-ttu-id="2a2ba-874">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-874">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-875">Microsoft.EntityFrameworkCore.SqlServer dříve závisel na systému.Data.SqlClient.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-875">Microsoft.EntityFrameworkCore.SqlServer previously depended on System.Data.SqlClient.</span></span>

<span data-ttu-id="2a2ba-876">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-876">**New behavior**</span></span>

<span data-ttu-id="2a2ba-877">Aktualizovali jsme náš balíček tak, aby závisel na microsoft.Data.SqlClient.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-877">We've updated our package to depend on Microsoft.Data.SqlClient.</span></span>

<span data-ttu-id="2a2ba-878">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-878">**Why**</span></span>

<span data-ttu-id="2a2ba-879">Microsoft.Data.SqlClient je vlajkovou lodí ovladač přístupu k datům pro SQL Server do budoucna a System.Data.SqlClient již není středem vývoje.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-879">Microsoft.Data.SqlClient is the flagship data access driver for SQL Server going forward, and System.Data.SqlClient no longer be the focus of development.</span></span>
<span data-ttu-id="2a2ba-880">Některé důležité funkce, například Vždy šifrované, jsou k dispozici pouze na Microsoft.Data.SqlClient.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-880">Some important features, such as Always Encrypted, are only available on Microsoft.Data.SqlClient.</span></span>

<span data-ttu-id="2a2ba-881">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-881">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-882">Pokud váš kód přebírá přímou závislost na System.Data.SqlClient, musíte jej změnit tak, aby odkazoval microsoft.Data.SqlClient místo; jako dva balíčky udržovat velmi vysoký stupeň kompatibility rozhraní API, by to mělo být pouze jednoduchý balíček a změna oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-882">If your code takes a direct dependency on System.Data.SqlClient, you must change it to reference Microsoft.Data.SqlClient instead; as the two packages maintain a very high degree of API compatibility, this should only be a simple package and namespace change.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="2a2ba-883">Musí být nakonfigurováno více nejednoznačných vztahů, které samy odkazují na sebe.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-883">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="2a2ba-884">#13573 problému sledování</span><span class="sxs-lookup"><span data-stu-id="2a2ba-884">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="2a2ba-885">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-885">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-886">Typ entity s více samoodkazujícími jednosměrnými navigačními vlastnostmi a odpovídajícími jednotkami FK byl nesprávně nakonfigurován jako jeden vztah.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-886">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="2a2ba-887">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2a2ba-887">For example:</span></span>

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

<span data-ttu-id="2a2ba-888">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-888">**New behavior**</span></span>

<span data-ttu-id="2a2ba-889">Tento scénář je nyní zjištěna v budově modelu a je vyvolána výjimka označující, že model je nejednoznačný.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-889">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="2a2ba-890">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-890">**Why**</span></span>

<span data-ttu-id="2a2ba-891">Výsledný model byl nejednoznačný a bude pravděpodobně obvykle špatné pro tento případ.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-891">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="2a2ba-892">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-892">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-893">Použijte úplnou konfiguraci relace.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-893">Use full configuration of the relationship.</span></span> <span data-ttu-id="2a2ba-894">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2a2ba-894">For example:</span></span>

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
### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a><span data-ttu-id="2a2ba-895">DbFunction.Schema je null nebo prázdný řetězec nakonfiguruje, aby byl ve výchozím schématu modelu</span><span class="sxs-lookup"><span data-stu-id="2a2ba-895">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>

[<span data-ttu-id="2a2ba-896">Sledování #12757 problému</span><span class="sxs-lookup"><span data-stu-id="2a2ba-896">Tracking Issue #12757</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12757)

<span data-ttu-id="2a2ba-897">**Staré chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-897">**Old behavior**</span></span>

<span data-ttu-id="2a2ba-898">Funkce DbFunction nakonfigurovaná se schématem jako prázdný řetězec byla považována za vestavěnou funkci bez schématu.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-898">A DbFunction configured with schema as an empty string was treated as built-in function without a schema.</span></span> <span data-ttu-id="2a2ba-899">Například následující kód `DatePart` namapuje `DATEPART` funkci CLR na vestavěnou funkci na serveru SqlServer.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-899">For example following code will map `DatePart` CLR function to `DATEPART` built-in function on SqlServer.</span></span>

```csharp
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

<span data-ttu-id="2a2ba-900">**Nové chování**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-900">**New behavior**</span></span>

<span data-ttu-id="2a2ba-901">Všechna mapování funkce DbFunction jsou považována za mapovaná na uživatelem definované funkce.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-901">All DbFunction mappings are considered to be mapped to user defined functions.</span></span> <span data-ttu-id="2a2ba-902">Proto prázdná hodnota řetězce by umístit funkci uvnitř výchozí schéma pro model.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-902">Hence empty string value would put the function inside the default schema for the model.</span></span> <span data-ttu-id="2a2ba-903">Což by mohlo být schéma nakonfigurované explicitně prostřednictvím fluent API `modelBuilder.HasDefaultSchema()` nebo `dbo` jinak.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-903">Which could be the schema configured explicitly via fluent API `modelBuilder.HasDefaultSchema()` or `dbo` otherwise.</span></span>

<span data-ttu-id="2a2ba-904">**Proč**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-904">**Why**</span></span>

<span data-ttu-id="2a2ba-905">Dříve schéma prázdné byl způsob, jak zacházet s funkcí je vestavěný, ale tato logika je použitelná pouze pro SqlServer, kde vestavěné funkce nepatří do žádné schéma.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-905">Previously schema being empty was a way to treat that function is built-in but that logic is only applicable for SqlServer where built-in functions do not belong to any schema.</span></span>

<span data-ttu-id="2a2ba-906">**Omezení rizik**</span><span class="sxs-lookup"><span data-stu-id="2a2ba-906">**Mitigations**</span></span>

<span data-ttu-id="2a2ba-907">Nakonfigurujte překlad dbfunction ručně, abyste jej namapovali na vestavěnou funkci.</span><span class="sxs-lookup"><span data-stu-id="2a2ba-907">Configure DbFunction's translation manually to map it to a built-in function.</span></span>

```csharp
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```
