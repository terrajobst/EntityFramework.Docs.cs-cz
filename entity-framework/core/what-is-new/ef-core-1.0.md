---
title: Co je nového v EF Core 1.0 - EF Core
author: divega
ms.date: 10/27/2016
ms.assetid: 20A25111-AEBE-4BC2-83A5-3F651952DF72
uid: core/what-is-new/ef-core-1.0
ms.openlocfilehash: 2cd2a54d75ed3f0caa8b674dfb56babcfcc13592
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417522"
---
# <a name="features-included-in-ef-core-10"></a><span data-ttu-id="ff697-102">Funkce obsažené v EF Core 1.0</span><span class="sxs-lookup"><span data-stu-id="ff697-102">Features included in EF Core 1.0</span></span>

## <a name="platforms"></a><span data-ttu-id="ff697-103">Platformy</span><span class="sxs-lookup"><span data-stu-id="ff697-103">Platforms</span></span>

### <a name="net-framework-451"></a><span data-ttu-id="ff697-104">.NET Framework 4.5.1</span><span class="sxs-lookup"><span data-stu-id="ff697-104">.NET Framework 4.5.1</span></span>

<span data-ttu-id="ff697-105">Zahrnuje konzole, WPF, WinForms, ASP.NET 4, atd.</span><span class="sxs-lookup"><span data-stu-id="ff697-105">Includes Console, WPF, WinForms, ASP.NET 4, etc.</span></span>

### <a name="net-standard-13"></a><span data-ttu-id="ff697-106">.NET Standard 1.3</span><span class="sxs-lookup"><span data-stu-id="ff697-106">.NET Standard 1.3</span></span>

<span data-ttu-id="ff697-107">Včetně ASP.NET Core cílení na rozhraní .NET Framework a .NET Core v systémech Windows, OSX a Linux.</span><span class="sxs-lookup"><span data-stu-id="ff697-107">Including ASP.NET Core targeting both .NET Framework and .NET Core on Windows, OSX, and Linux.</span></span>

## <a name="modelling"></a><span data-ttu-id="ff697-108">Modelování</span><span class="sxs-lookup"><span data-stu-id="ff697-108">Modelling</span></span>

### <a name="basic-modelling"></a><span data-ttu-id="ff697-109">Základní modelování</span><span class="sxs-lookup"><span data-stu-id="ff697-109">Basic modelling</span></span>

<span data-ttu-id="ff697-110">Na základě entit POCO s vlastnostmi get/set běžných skalárních typů (`int`, `string`, atd.).</span><span class="sxs-lookup"><span data-stu-id="ff697-110">Based on POCO entities with get/set properties of common scalar types (`int`, `string`, etc.).</span></span>

### <a name="relationships-and-navigation-properties"></a><span data-ttu-id="ff697-111">Vztahy a navigační vlastnosti</span><span class="sxs-lookup"><span data-stu-id="ff697-111">Relationships and navigation properties</span></span>

<span data-ttu-id="ff697-112">Vztahy 1:N a 1:0 nebo 1 lze zadat v modelu na základě cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="ff697-112">One-to-many and One-to-zero-or-one relationships can be specified in the model based on a foreign key.</span></span> <span data-ttu-id="ff697-113">Navigační vlastnosti jednoduché kolekce nebo typy odkazů mohou být přidruženy k těmto vztahům.</span><span class="sxs-lookup"><span data-stu-id="ff697-113">Navigation properties of simple collection or reference types can be associated with these relationships.</span></span>

### <a name="built-in-conventions"></a><span data-ttu-id="ff697-114">Vestavěné konvence</span><span class="sxs-lookup"><span data-stu-id="ff697-114">Built-in conventions</span></span>

<span data-ttu-id="ff697-115">Tyto sestavení počáteční model založený na tvaru třídy entity.</span><span class="sxs-lookup"><span data-stu-id="ff697-115">These build an initial model based on the shape of the entity classes.</span></span>

### <a name="fluent-api"></a><span data-ttu-id="ff697-116">Fluent API</span><span class="sxs-lookup"><span data-stu-id="ff697-116">Fluent API</span></span>

<span data-ttu-id="ff697-117">Umožňuje přepsat metodu `OnModelCreating` v kontextu a dále nakonfigurovat model, který byl zjištěn konvencí.</span><span class="sxs-lookup"><span data-stu-id="ff697-117">Allows you to override the `OnModelCreating` method on your context to further configure the model that was discovered by convention.</span></span>

### <a name="data-annotations"></a><span data-ttu-id="ff697-118">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="ff697-118">Data annotations</span></span>

<span data-ttu-id="ff697-119">Jsou atributy, které lze přidat do třídy/vlastnosti entity a bude mít vliv na model EF.</span><span class="sxs-lookup"><span data-stu-id="ff697-119">Are attributes that can be added to your entity classes/properties and will influence the EF model.</span></span> <span data-ttu-id="ff697-120">Například přidání `[Required]` umožní EF vědět, že je požadováno vlastnost.</span><span class="sxs-lookup"><span data-stu-id="ff697-120">For example, adding `[Required]` will let EF know that a property is required.</span></span>

### <a name="relational-table-mapping"></a><span data-ttu-id="ff697-121">Mapování relační chodnících</span><span class="sxs-lookup"><span data-stu-id="ff697-121">Relational Table mapping</span></span>

<span data-ttu-id="ff697-122">Umožňuje mapování entit na tabulky nebo sloupce.</span><span class="sxs-lookup"><span data-stu-id="ff697-122">Allows entities to be mapped to tables/columns.</span></span>

### <a name="key-value-generation"></a><span data-ttu-id="ff697-123">Generování hodnoty klíče</span><span class="sxs-lookup"><span data-stu-id="ff697-123">Key value generation</span></span>

<span data-ttu-id="ff697-124">Včetně generování na straně klienta a generování databáze.</span><span class="sxs-lookup"><span data-stu-id="ff697-124">Including client-side generation and database generation.</span></span>

### <a name="database-generated-values"></a><span data-ttu-id="ff697-125">Databáze generované hodnoty</span><span class="sxs-lookup"><span data-stu-id="ff697-125">Database generated values</span></span>

<span data-ttu-id="ff697-126">Umožňuje generovat hodnoty v databázi při vložení (výchozí hodnoty) nebo aktualizaci (vypočítané sloupce).</span><span class="sxs-lookup"><span data-stu-id="ff697-126">Allows for values to be generated by the database on insert (default values) or update (computed columns).</span></span>

### <a name="sequences-in-sql-server"></a><span data-ttu-id="ff697-127">Sekvence na serveru SQL Server</span><span class="sxs-lookup"><span data-stu-id="ff697-127">Sequences in SQL Server</span></span>

<span data-ttu-id="ff697-128">Umožňuje definovat objekty sekvence v modelu.</span><span class="sxs-lookup"><span data-stu-id="ff697-128">Allows for sequence objects to be defined in the model.</span></span>

### <a name="unique-constraints"></a><span data-ttu-id="ff697-129">Jedinečná omezení</span><span class="sxs-lookup"><span data-stu-id="ff697-129">Unique constraints</span></span>

<span data-ttu-id="ff697-130">Umožňuje definici alternativních klíčů a možnost definovat vztahy, které cílí na tento klíč.</span><span class="sxs-lookup"><span data-stu-id="ff697-130">Allows for the definition of alternate keys and the ability to define relationships that target that key.</span></span>

### <a name="indexes"></a><span data-ttu-id="ff697-131">Indexy</span><span class="sxs-lookup"><span data-stu-id="ff697-131">Indexes</span></span>

<span data-ttu-id="ff697-132">Definování indexů v modelu automaticky zavádí indexy v databázi.</span><span class="sxs-lookup"><span data-stu-id="ff697-132">Defining indexes in the model automatically introduces indexes in the database.</span></span> <span data-ttu-id="ff697-133">Jedinečné indexy jsou také podporovány.</span><span class="sxs-lookup"><span data-stu-id="ff697-133">Unique indexes are also supported.</span></span>

### <a name="shadow-state-properties"></a><span data-ttu-id="ff697-134">Vlastnosti stavu stínů</span><span class="sxs-lookup"><span data-stu-id="ff697-134">Shadow state properties</span></span>

<span data-ttu-id="ff697-135">Umožňuje vlastnosti, které mají být definovány v modelu, které nejsou deklarovány a nejsou uloženy ve třídě .NET, ale mohou být sledovány a aktualizovány EF Core.</span><span class="sxs-lookup"><span data-stu-id="ff697-135">Allows for properties to be defined in the model that are not declared and are not stored in the .NET class but can be tracked and updated by EF Core.</span></span> <span data-ttu-id="ff697-136">Běžně používané pro vlastnosti cizího klíče při jejich vystavení v objektu není žádoucí.</span><span class="sxs-lookup"><span data-stu-id="ff697-136">Commonly used for foreign key properties when exposing these in the object is not desired.</span></span>

### <a name="table-per-hierarchy-inheritance-pattern"></a><span data-ttu-id="ff697-137">Vzor dědičnosti podle hierarchie</span><span class="sxs-lookup"><span data-stu-id="ff697-137">Table-Per-Hierarchy inheritance pattern</span></span>

<span data-ttu-id="ff697-138">Umožňuje entity v hierarchii dědičnosti, které mají být uloženy do jedné tabulky pomocí sloupce diskriminátoru k identifikaci typu entity pro daný záznam v databázi.</span><span class="sxs-lookup"><span data-stu-id="ff697-138">Allows entities in an inheritance hierarchy to be saved to a single table using a discriminator column to identify they entity type for a given record in the database.</span></span>

### <a name="model-validation"></a><span data-ttu-id="ff697-139">Ověření modelu</span><span class="sxs-lookup"><span data-stu-id="ff697-139">Model validation</span></span>

<span data-ttu-id="ff697-140">Detekuje neplatné vzory v modelu a poskytuje užitečné chybové zprávy.</span><span class="sxs-lookup"><span data-stu-id="ff697-140">Detects invalid patterns in the model and provides helpful error messages.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="ff697-141">Sledování změn</span><span class="sxs-lookup"><span data-stu-id="ff697-141">Change tracking</span></span>

### <a name="snapshot-change-tracking"></a><span data-ttu-id="ff697-142">Sledování změn snímku</span><span class="sxs-lookup"><span data-stu-id="ff697-142">Snapshot change tracking</span></span>

<span data-ttu-id="ff697-143">Umožňuje automaticky zjistit změny v entitách porovnáním aktuálního stavu s kopií (snímek) původního stavu.</span><span class="sxs-lookup"><span data-stu-id="ff697-143">Allows changes in entities to be detected automatically by comparing current state against a copy (snapshot) of the original state.</span></span>

### <a name="notification-change-tracking"></a><span data-ttu-id="ff697-144">Sledování změn oznámení</span><span class="sxs-lookup"><span data-stu-id="ff697-144">Notification change tracking</span></span>

<span data-ttu-id="ff697-145">Umožňuje entitám upozornit sledování změn při změně hodnot vlastností.</span><span class="sxs-lookup"><span data-stu-id="ff697-145">Allows your entities to notify the change tracker when property values are modified.</span></span>

### <a name="accessing-tracked-state"></a><span data-ttu-id="ff697-146">Přístup ke sledovanému stavu</span><span class="sxs-lookup"><span data-stu-id="ff697-146">Accessing tracked state</span></span>

<span data-ttu-id="ff697-147">Via `DbContext.Entry` `DbContext.ChangeTracker`a .</span><span class="sxs-lookup"><span data-stu-id="ff697-147">Via `DbContext.Entry` and `DbContext.ChangeTracker`.</span></span>

### <a name="attaching-detached-entitiesgraphs"></a><span data-ttu-id="ff697-148">Připojení odpojených entit/grafů</span><span class="sxs-lookup"><span data-stu-id="ff697-148">Attaching detached entities/graphs</span></span>

<span data-ttu-id="ff697-149">Nové `DbContext.AttachGraph` rozhraní API pomáhá znovu připojit entity k kontextu za účelem uložení nových/změněných entit.</span><span class="sxs-lookup"><span data-stu-id="ff697-149">The new `DbContext.AttachGraph` API helps re-attach entities to a context in order to save new/modified entities.</span></span>

## <a name="saving-data"></a><span data-ttu-id="ff697-150">Ukládání dat</span><span class="sxs-lookup"><span data-stu-id="ff697-150">Saving data</span></span>

### <a name="basic-save-functionality"></a><span data-ttu-id="ff697-151">Základní funkce ukládání</span><span class="sxs-lookup"><span data-stu-id="ff697-151">Basic save functionality</span></span>

<span data-ttu-id="ff697-152">Umožňuje zachovat změny instancí entit v databázi.</span><span class="sxs-lookup"><span data-stu-id="ff697-152">Allows changes to entity instances to be persisted to the database.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="ff697-153">Optimistická metoda souběžného zpracování</span><span class="sxs-lookup"><span data-stu-id="ff697-153">Optimistic Concurrency</span></span>

<span data-ttu-id="ff697-154">Chrání před přepsáním změny provedené jiným uživatelem od data byla načtena z databáze.</span><span class="sxs-lookup"><span data-stu-id="ff697-154">Protects against overwriting changes made by another user since data was fetched from the database.</span></span>

### <a name="async-savechanges"></a><span data-ttu-id="ff697-155">Asynchronní savechanges</span><span class="sxs-lookup"><span data-stu-id="ff697-155">Async SaveChanges</span></span>

<span data-ttu-id="ff697-156">Může uvolnit aktuální vlákno pro zpracování dalších požadavků, zatímco databáze `SaveChanges`zpracovává příkazy vydané z .</span><span class="sxs-lookup"><span data-stu-id="ff697-156">Can free up the current thread to process other requests while the database processes the commands issued from `SaveChanges`.</span></span>

### <a name="database-transactions"></a><span data-ttu-id="ff697-157">Databázové transakce</span><span class="sxs-lookup"><span data-stu-id="ff697-157">Database Transactions</span></span>

<span data-ttu-id="ff697-158">Prostředky, `SaveChanges` které je vždy atomické (což znamená, že buď zcela úspěšné, nebo žádné změny jsou provedeny v databázi).</span><span class="sxs-lookup"><span data-stu-id="ff697-158">Means that `SaveChanges` is always atomic (meaning it either completely succeeds, or no changes are made to the database).</span></span> <span data-ttu-id="ff697-159">Existují také možnosti API související s transakcemi, které umožňují sdílení transakcí mezi instancemi kontextu atd.</span><span class="sxs-lookup"><span data-stu-id="ff697-159">There are also transaction related APIs to allow sharing transactions between context instances etc.</span></span>

### <a name="relational-batching-of-statements"></a><span data-ttu-id="ff697-160">Relační: Dávkování výpisů</span><span class="sxs-lookup"><span data-stu-id="ff697-160">Relational: Batching of statements</span></span>

<span data-ttu-id="ff697-161">Poskytuje lepší výkon dávkováním více příkazů INSERT/UPDATE/DELETE do jednoho roundtripu do databáze.</span><span class="sxs-lookup"><span data-stu-id="ff697-161">Provides better performance by batching up multiple INSERT/UPDATE/DELETE commands into a single roundtrip to the database.</span></span>

## <a name="query"></a><span data-ttu-id="ff697-162">Dotaz</span><span class="sxs-lookup"><span data-stu-id="ff697-162">Query</span></span>

### <a name="basic-linq-support"></a><span data-ttu-id="ff697-163">Základní podpora LINQ</span><span class="sxs-lookup"><span data-stu-id="ff697-163">Basic LINQ support</span></span>

<span data-ttu-id="ff697-164">Poskytuje možnost použít LINQ k načtení dat z databáze.</span><span class="sxs-lookup"><span data-stu-id="ff697-164">Provides the ability to use LINQ to retrieve data from the database.</span></span>

### <a name="mixed-clientserver-evaluation"></a><span data-ttu-id="ff697-165">Smíšené vyhodnocení klienta/serveru</span><span class="sxs-lookup"><span data-stu-id="ff697-165">Mixed client/server evaluation</span></span>

<span data-ttu-id="ff697-166">Umožňuje dotazy obsahovat logiku, která nelze vyhodnotit v databázi, a proto musí být vyhodnocena po načtení dat do paměti.</span><span class="sxs-lookup"><span data-stu-id="ff697-166">Enables queries to contain logic that cannot be evaluated in the database, and must therefore be evaluated after the data is retrieved into memory.</span></span>

### <a name="notracking"></a><span data-ttu-id="ff697-167">NoTracking</span><span class="sxs-lookup"><span data-stu-id="ff697-167">NoTracking</span></span>

<span data-ttu-id="ff697-168">Dotazy umožňuje rychlejší spuštění dotazu, když kontext není nutné sledovat změny instance entity (to je užitečné, pokud jsou výsledky jen pro čtení).</span><span class="sxs-lookup"><span data-stu-id="ff697-168">Queries enables quicker query execution when the context does not need to monitor for changes to the entity instances (this is useful if the results are read-only).</span></span>

### <a name="eager-loading"></a><span data-ttu-id="ff697-169">Dychtivé načítání</span><span class="sxs-lookup"><span data-stu-id="ff697-169">Eager loading</span></span>

<span data-ttu-id="ff697-170">Poskytuje `Include` a `ThenInclude` metody k identifikaci souvisejících dat, které by měly být také načteny při dotazování.</span><span class="sxs-lookup"><span data-stu-id="ff697-170">Provides the `Include` and `ThenInclude` methods to identify related data that should also be fetched when querying.</span></span>

### <a name="async-query"></a><span data-ttu-id="ff697-171">Asynchronní dotaz</span><span class="sxs-lookup"><span data-stu-id="ff697-171">Async query</span></span>

<span data-ttu-id="ff697-172">Můžete uvolnit aktuální vlákno (a je přidružené prostředky) ke zpracování dalších požadavků, zatímco databáze zpracovává dotaz.</span><span class="sxs-lookup"><span data-stu-id="ff697-172">Can free up the current thread (and it's associated resources) to process other requests while the database processes the query.</span></span>

### <a name="raw-sql-queries"></a><span data-ttu-id="ff697-173">Nezpracované dotazy SQL</span><span class="sxs-lookup"><span data-stu-id="ff697-173">Raw SQL queries</span></span>

<span data-ttu-id="ff697-174">Poskytuje `DbSet.FromSql` metodu pro použití nezpracovaných dotazů SQL k načtení dat.</span><span class="sxs-lookup"><span data-stu-id="ff697-174">Provides the `DbSet.FromSql` method to use raw SQL queries to fetch data.</span></span> <span data-ttu-id="ff697-175">Tyto dotazy lze také skládá při použití LINQ.</span><span class="sxs-lookup"><span data-stu-id="ff697-175">These queries can also be composed on using LINQ.</span></span>

## <a name="database-schema-management"></a><span data-ttu-id="ff697-176">Správa schématu databáze</span><span class="sxs-lookup"><span data-stu-id="ff697-176">Database schema management</span></span>

### <a name="database-creationdeletion-apis"></a><span data-ttu-id="ff697-177">Vytváření a odstraňování api pro vytváření a odstraňování databáze</span><span class="sxs-lookup"><span data-stu-id="ff697-177">Database creation/deletion APIs</span></span>

<span data-ttu-id="ff697-178">Jsou většinou určeny pro testování, kde chcete rychle vytvořit nebo odstranit databázi bez použití migrace.</span><span class="sxs-lookup"><span data-stu-id="ff697-178">Are mostly designed for testing where you want to quickly create/delete the database without using migrations.</span></span>

### <a name="relational-database-migrations"></a><span data-ttu-id="ff697-179">Migrace relačních databází</span><span class="sxs-lookup"><span data-stu-id="ff697-179">Relational database migrations</span></span>

<span data-ttu-id="ff697-180">Povolte schéma relační databáze vyvíjet přesčas jako váš model změny.</span><span class="sxs-lookup"><span data-stu-id="ff697-180">Allow a relational database schema to evolve overtime as your model changes.</span></span>

### <a name="reverse-engineer-from-database"></a><span data-ttu-id="ff697-181">Zpětná inženýrská verze z databáze</span><span class="sxs-lookup"><span data-stu-id="ff697-181">Reverse engineer from database</span></span>

<span data-ttu-id="ff697-182">Ve spouštění systému souborů EF modelu na základě existující schéma relační databáze.</span><span class="sxs-lookup"><span data-stu-id="ff697-182">Scaffolds an EF model based on an existing relational database schema.</span></span>

## <a name="database-providers"></a><span data-ttu-id="ff697-183">Poskytovatelé databází</span><span class="sxs-lookup"><span data-stu-id="ff697-183">Database providers</span></span>

### <a name="sql-server"></a><span data-ttu-id="ff697-184">SQL Server</span><span class="sxs-lookup"><span data-stu-id="ff697-184">SQL Server</span></span>

<span data-ttu-id="ff697-185">Připojí se k serveru Microsoft SQL Server 2008 a dále.</span><span class="sxs-lookup"><span data-stu-id="ff697-185">Connects to Microsoft SQL Server 2008 onwards.</span></span>

### <a name="sqlite"></a><span data-ttu-id="ff697-186">SQLite</span><span class="sxs-lookup"><span data-stu-id="ff697-186">SQLite</span></span>

<span data-ttu-id="ff697-187">Připojuje se k databázi SQLite 3.</span><span class="sxs-lookup"><span data-stu-id="ff697-187">Connects to a SQLite 3 database.</span></span>

### <a name="in-memory"></a><span data-ttu-id="ff697-188">V paměti</span><span class="sxs-lookup"><span data-stu-id="ff697-188">In-Memory</span></span>

<span data-ttu-id="ff697-189">Je navržen tak, aby snadno povolit testování bez připojení k reálné databázi.</span><span class="sxs-lookup"><span data-stu-id="ff697-189">Is designed to easily enable testing without connecting to a real database.</span></span>

### <a name="3rd-party-providers"></a><span data-ttu-id="ff697-190">Poskytovatelé třetích stran</span><span class="sxs-lookup"><span data-stu-id="ff697-190">3rd party providers</span></span>

<span data-ttu-id="ff697-191">Několik zprostředkovatelů jsou k dispozici pro jiné databázové stroje.</span><span class="sxs-lookup"><span data-stu-id="ff697-191">Several providers are available for other database engines.</span></span> <span data-ttu-id="ff697-192">Úplný seznam naleznete v tématu [Zprostředkovatelé databáze.](../providers/index.md)</span><span class="sxs-lookup"><span data-stu-id="ff697-192">See [Database Providers](../providers/index.md) for a complete list.</span></span>
