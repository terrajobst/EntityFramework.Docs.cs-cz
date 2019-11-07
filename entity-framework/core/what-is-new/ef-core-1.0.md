---
title: Co je nového v EF Core 1,0-EF Core
author: divega
ms.date: 10/27/2016
ms.assetid: 20A25111-AEBE-4BC2-83A5-3F651952DF72
uid: core/what-is-new/ef-core-1.0
ms.openlocfilehash: 2cd2a54d75ed3f0caa8b674dfb56babcfcc13592
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655855"
---
# <a name="features-included-in-ef-core-10"></a><span data-ttu-id="18f91-102">Funkce, které jsou součástí EF Core 1,0</span><span class="sxs-lookup"><span data-stu-id="18f91-102">Features included in EF Core 1.0</span></span>

## <a name="platforms"></a><span data-ttu-id="18f91-103">Platformy</span><span class="sxs-lookup"><span data-stu-id="18f91-103">Platforms</span></span>

### <a name="net-framework-451"></a><span data-ttu-id="18f91-104">.NET Framework 4.5.1</span><span class="sxs-lookup"><span data-stu-id="18f91-104">.NET Framework 4.5.1</span></span>

<span data-ttu-id="18f91-105">Zahrnuje konzolu, WPF, WinForms, ASP.NET 4 atd.</span><span class="sxs-lookup"><span data-stu-id="18f91-105">Includes Console, WPF, WinForms, ASP.NET 4, etc.</span></span>

### <a name="net-standard-13"></a><span data-ttu-id="18f91-106">.NET Standard 1,3</span><span class="sxs-lookup"><span data-stu-id="18f91-106">.NET Standard 1.3</span></span>

<span data-ttu-id="18f91-107">Včetně ASP.NET Core cílících na .NET Framework a .NET Core ve Windows, OSX a Linux.</span><span class="sxs-lookup"><span data-stu-id="18f91-107">Including ASP.NET Core targeting both .NET Framework and .NET Core on Windows, OSX, and Linux.</span></span>

## <a name="modelling"></a><span data-ttu-id="18f91-108">Modelovací</span><span class="sxs-lookup"><span data-stu-id="18f91-108">Modelling</span></span>

### <a name="basic-modelling"></a><span data-ttu-id="18f91-109">Základní modelování</span><span class="sxs-lookup"><span data-stu-id="18f91-109">Basic modelling</span></span>

<span data-ttu-id="18f91-110">Založené na POCO entitách pomocí vlastností get/set běžných skalárních typů (`int`, `string`atd.).</span><span class="sxs-lookup"><span data-stu-id="18f91-110">Based on POCO entities with get/set properties of common scalar types (`int`, `string`, etc.).</span></span>

### <a name="relationships-and-navigation-properties"></a><span data-ttu-id="18f91-111">Relace a navigační vlastnosti</span><span class="sxs-lookup"><span data-stu-id="18f91-111">Relationships and navigation properties</span></span>

<span data-ttu-id="18f91-112">V modelu založeném na cizím klíči lze určit relace 1: n a 1:1 nebo jedna-nula.</span><span class="sxs-lookup"><span data-stu-id="18f91-112">One-to-many and One-to-zero-or-one relationships can be specified in the model based on a foreign key.</span></span> <span data-ttu-id="18f91-113">K těmto relacím se dají přidružit navigační vlastnosti jednoduchých typů nebo odkazů.</span><span class="sxs-lookup"><span data-stu-id="18f91-113">Navigation properties of simple collection or reference types can be associated with these relationships.</span></span>

### <a name="built-in-conventions"></a><span data-ttu-id="18f91-114">Předdefinované konvence</span><span class="sxs-lookup"><span data-stu-id="18f91-114">Built-in conventions</span></span>

<span data-ttu-id="18f91-115">Tyto sestavují počáteční model na základě tvaru tříd entit.</span><span class="sxs-lookup"><span data-stu-id="18f91-115">These build an initial model based on the shape of the entity classes.</span></span>

### <a name="fluent-api"></a><span data-ttu-id="18f91-116">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="18f91-116">Fluent API</span></span>

<span data-ttu-id="18f91-117">Umožňuje přepsat metodu `OnModelCreating` v kontextu a dále konfigurovat model, který byl zjištěn podle konvence.</span><span class="sxs-lookup"><span data-stu-id="18f91-117">Allows you to override the `OnModelCreating` method on your context to further configure the model that was discovered by convention.</span></span>

### <a name="data-annotations"></a><span data-ttu-id="18f91-118">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="18f91-118">Data annotations</span></span>

<span data-ttu-id="18f91-119">Jsou atributy, které mohou být přidány do tříd nebo vlastností entit a budou mít vliv na model EF.</span><span class="sxs-lookup"><span data-stu-id="18f91-119">Are attributes that can be added to your entity classes/properties and will influence the EF model.</span></span> <span data-ttu-id="18f91-120">Přidání `[Required]` například umožní, aby EF věděl, že je požadovaná vlastnost.</span><span class="sxs-lookup"><span data-stu-id="18f91-120">For example, adding `[Required]` will let EF know that a property is required.</span></span>

### <a name="relational-table-mapping"></a><span data-ttu-id="18f91-121">Mapování relační tabulky</span><span class="sxs-lookup"><span data-stu-id="18f91-121">Relational Table mapping</span></span>

<span data-ttu-id="18f91-122">Umožňuje mapovat entity na tabulky nebo sloupce.</span><span class="sxs-lookup"><span data-stu-id="18f91-122">Allows entities to be mapped to tables/columns.</span></span>

### <a name="key-value-generation"></a><span data-ttu-id="18f91-123">Generování hodnoty klíče</span><span class="sxs-lookup"><span data-stu-id="18f91-123">Key value generation</span></span>

<span data-ttu-id="18f91-124">Včetně generování a generování databáze na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="18f91-124">Including client-side generation and database generation.</span></span>

### <a name="database-generated-values"></a><span data-ttu-id="18f91-125">Databáze vygenerovaly hodnoty</span><span class="sxs-lookup"><span data-stu-id="18f91-125">Database generated values</span></span>

<span data-ttu-id="18f91-126">Umožňuje generovat hodnoty z databáze při vložení (výchozí hodnoty) nebo aktualizovat (počítané sloupce).</span><span class="sxs-lookup"><span data-stu-id="18f91-126">Allows for values to be generated by the database on insert (default values) or update (computed columns).</span></span>

### <a name="sequences-in-sql-server"></a><span data-ttu-id="18f91-127">Sekvence v SQL Server</span><span class="sxs-lookup"><span data-stu-id="18f91-127">Sequences in SQL Server</span></span>

<span data-ttu-id="18f91-128">Umožňuje definovat objekty sekvence v modelu.</span><span class="sxs-lookup"><span data-stu-id="18f91-128">Allows for sequence objects to be defined in the model.</span></span>

### <a name="unique-constraints"></a><span data-ttu-id="18f91-129">Jedinečná omezení</span><span class="sxs-lookup"><span data-stu-id="18f91-129">Unique constraints</span></span>

<span data-ttu-id="18f91-130">Umožňuje definování alternativních klíčů a možnosti definovat vztahy, které cílí na tento klíč.</span><span class="sxs-lookup"><span data-stu-id="18f91-130">Allows for the definition of alternate keys and the ability to define relationships that target that key.</span></span>

### <a name="indexes"></a><span data-ttu-id="18f91-131">Indexy</span><span class="sxs-lookup"><span data-stu-id="18f91-131">Indexes</span></span>

<span data-ttu-id="18f91-132">Definováním indexů v modelu automaticky zavádí indexy v databázi.</span><span class="sxs-lookup"><span data-stu-id="18f91-132">Defining indexes in the model automatically introduces indexes in the database.</span></span> <span data-ttu-id="18f91-133">Podporují se taky jedinečné indexy.</span><span class="sxs-lookup"><span data-stu-id="18f91-133">Unique indexes are also supported.</span></span>

### <a name="shadow-state-properties"></a><span data-ttu-id="18f91-134">Vlastnosti stavu stínu</span><span class="sxs-lookup"><span data-stu-id="18f91-134">Shadow state properties</span></span>

<span data-ttu-id="18f91-135">Umožňuje definovat vlastnosti v modelu, který není deklarovaný a který není uložený ve třídě .NET, ale dá se sledovat a aktualizovat pomocí EF Core.</span><span class="sxs-lookup"><span data-stu-id="18f91-135">Allows for properties to be defined in the model that are not declared and are not stored in the .NET class but can be tracked and updated by EF Core.</span></span> <span data-ttu-id="18f91-136">Běžně používané pro vlastnosti cizích klíčů při vystavování těchto objektů v objektu není žádoucí.</span><span class="sxs-lookup"><span data-stu-id="18f91-136">Commonly used for foreign key properties when exposing these in the object is not desired.</span></span>

### <a name="table-per-hierarchy-inheritance-pattern"></a><span data-ttu-id="18f91-137">Vzor dědičnosti tabulky na hierarchii</span><span class="sxs-lookup"><span data-stu-id="18f91-137">Table-Per-Hierarchy inheritance pattern</span></span>

<span data-ttu-id="18f91-138">Umožňuje, aby se entity v hierarchii dědičnosti ukládaly do jedné tabulky pomocí sloupce diskriminátoru, které identifikují typ entity pro daný záznam v databázi.</span><span class="sxs-lookup"><span data-stu-id="18f91-138">Allows entities in an inheritance hierarchy to be saved to a single table using a discriminator column to identify they entity type for a given record in the database.</span></span>

### <a name="model-validation"></a><span data-ttu-id="18f91-139">Ověření modelu</span><span class="sxs-lookup"><span data-stu-id="18f91-139">Model validation</span></span>

<span data-ttu-id="18f91-140">Detekuje neplatné vzory v modelu a poskytuje užitečné chybové zprávy.</span><span class="sxs-lookup"><span data-stu-id="18f91-140">Detects invalid patterns in the model and provides helpful error messages.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="18f91-141">Sledování změn</span><span class="sxs-lookup"><span data-stu-id="18f91-141">Change tracking</span></span>

### <a name="snapshot-change-tracking"></a><span data-ttu-id="18f91-142">Sledování změn snímků</span><span class="sxs-lookup"><span data-stu-id="18f91-142">Snapshot change tracking</span></span>

<span data-ttu-id="18f91-143">Umožňuje, aby se změny v entitách zjistily automaticky porovnáním aktuálního stavu s kopií (snímkem) původního stavu.</span><span class="sxs-lookup"><span data-stu-id="18f91-143">Allows changes in entities to be detected automatically by comparing current state against a copy (snapshot) of the original state.</span></span>

### <a name="notification-change-tracking"></a><span data-ttu-id="18f91-144">Sledování změn oznámení</span><span class="sxs-lookup"><span data-stu-id="18f91-144">Notification change tracking</span></span>

<span data-ttu-id="18f91-145">Umožňuje entitám upozorňování sledování změn při změně hodnot vlastností.</span><span class="sxs-lookup"><span data-stu-id="18f91-145">Allows your entities to notify the change tracker when property values are modified.</span></span>

### <a name="accessing-tracked-state"></a><span data-ttu-id="18f91-146">Přístup ke sledovanému stavu</span><span class="sxs-lookup"><span data-stu-id="18f91-146">Accessing tracked state</span></span>

<span data-ttu-id="18f91-147">Prostřednictvím `DbContext.Entry` a `DbContext.ChangeTracker`.</span><span class="sxs-lookup"><span data-stu-id="18f91-147">Via `DbContext.Entry` and `DbContext.ChangeTracker`.</span></span>

### <a name="attaching-detached-entitiesgraphs"></a><span data-ttu-id="18f91-148">Připojení odpojených entit a grafů</span><span class="sxs-lookup"><span data-stu-id="18f91-148">Attaching detached entities/graphs</span></span>

<span data-ttu-id="18f91-149">Nové rozhraní `DbContext.AttachGraph` API pomáhá znovu připojovat entity k kontextu, aby bylo možné ukládat nové a upravené entity.</span><span class="sxs-lookup"><span data-stu-id="18f91-149">The new `DbContext.AttachGraph` API helps re-attach entities to a context in order to save new/modified entities.</span></span>

## <a name="saving-data"></a><span data-ttu-id="18f91-150">Ukládání dat</span><span class="sxs-lookup"><span data-stu-id="18f91-150">Saving data</span></span>

### <a name="basic-save-functionality"></a><span data-ttu-id="18f91-151">Základní funkce ukládání</span><span class="sxs-lookup"><span data-stu-id="18f91-151">Basic save functionality</span></span>

<span data-ttu-id="18f91-152">Umožňuje, aby se změny instancí entity zachovaly v databázi.</span><span class="sxs-lookup"><span data-stu-id="18f91-152">Allows changes to entity instances to be persisted to the database.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="18f91-153">Optimistická metoda souběžného zpracování</span><span class="sxs-lookup"><span data-stu-id="18f91-153">Optimistic Concurrency</span></span>

<span data-ttu-id="18f91-154">Chrání před přepsáním změn provedených jiným uživatelem od načtení dat z databáze.</span><span class="sxs-lookup"><span data-stu-id="18f91-154">Protects against overwriting changes made by another user since data was fetched from the database.</span></span>

### <a name="async-savechanges"></a><span data-ttu-id="18f91-155">Asynchronní metody SaveChanges</span><span class="sxs-lookup"><span data-stu-id="18f91-155">Async SaveChanges</span></span>

<span data-ttu-id="18f91-156">Může uvolnit aktuální vlákno pro zpracování dalších požadavků, zatímco databáze zpracovává příkazy vydané z `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="18f91-156">Can free up the current thread to process other requests while the database processes the commands issued from `SaveChanges`.</span></span>

### <a name="database-transactions"></a><span data-ttu-id="18f91-157">Transakce databáze</span><span class="sxs-lookup"><span data-stu-id="18f91-157">Database Transactions</span></span>

<span data-ttu-id="18f91-158">Znamená, že `SaveChanges` je vždy atomická (to znamená, že buď zcela uspěje, nebo nejsou provedeny žádné změny v databázi).</span><span class="sxs-lookup"><span data-stu-id="18f91-158">Means that `SaveChanges` is always atomic (meaning it either completely succeeds, or no changes are made to the database).</span></span> <span data-ttu-id="18f91-159">K dispozici jsou také rozhraní API související s transakcemi umožňující sdílení transakcí mezi instancemi kontextu atd.</span><span class="sxs-lookup"><span data-stu-id="18f91-159">There are also transaction related APIs to allow sharing transactions between context instances etc.</span></span>

### <a name="relational-batching-of-statements"></a><span data-ttu-id="18f91-160">Relační: dávkování příkazů</span><span class="sxs-lookup"><span data-stu-id="18f91-160">Relational: Batching of statements</span></span>

<span data-ttu-id="18f91-161">Poskytuje lepší výkon tím, že dávkuje více příkazů pro vložení, aktualizaci a odstranění do jediného převodu do databáze.</span><span class="sxs-lookup"><span data-stu-id="18f91-161">Provides better performance by batching up multiple INSERT/UPDATE/DELETE commands into a single roundtrip to the database.</span></span>

## <a name="query"></a><span data-ttu-id="18f91-162">Dotazy</span><span class="sxs-lookup"><span data-stu-id="18f91-162">Query</span></span>

### <a name="basic-linq-support"></a><span data-ttu-id="18f91-163">Základní podpora LINQ</span><span class="sxs-lookup"><span data-stu-id="18f91-163">Basic LINQ support</span></span>

<span data-ttu-id="18f91-164">Poskytuje možnost použít LINQ k načtení dat z databáze.</span><span class="sxs-lookup"><span data-stu-id="18f91-164">Provides the ability to use LINQ to retrieve data from the database.</span></span>

### <a name="mixed-clientserver-evaluation"></a><span data-ttu-id="18f91-165">Smíšené vyhodnocení klientů/serverů</span><span class="sxs-lookup"><span data-stu-id="18f91-165">Mixed client/server evaluation</span></span>

<span data-ttu-id="18f91-166">Umožňuje dotazům obsahovat logiku, která se v databázi nedá vyhodnotit, a po načtení dat do paměti je proto nutné vyhodnotit.</span><span class="sxs-lookup"><span data-stu-id="18f91-166">Enables queries to contain logic that cannot be evaluated in the database, and must therefore be evaluated after the data is retrieved into memory.</span></span>

### <a name="notracking"></a><span data-ttu-id="18f91-167">NoTracking</span><span class="sxs-lookup"><span data-stu-id="18f91-167">NoTracking</span></span>

<span data-ttu-id="18f91-168">Dotazy umožňují rychlejší provádění dotazů, pokud kontext nemusí sledovat změny instancí entit (to je užitečné, pokud jsou výsledky jen pro čtení).</span><span class="sxs-lookup"><span data-stu-id="18f91-168">Queries enables quicker query execution when the context does not need to monitor for changes to the entity instances (this is useful if the results are read-only).</span></span>

### <a name="eager-loading"></a><span data-ttu-id="18f91-169">Eager načítání</span><span class="sxs-lookup"><span data-stu-id="18f91-169">Eager loading</span></span>

<span data-ttu-id="18f91-170">Poskytuje metody `Include` a `ThenInclude` k identifikaci souvisejících dat, která by se měla načíst také při dotazování.</span><span class="sxs-lookup"><span data-stu-id="18f91-170">Provides the `Include` and `ThenInclude` methods to identify related data that should also be fetched when querying.</span></span>

### <a name="async-query"></a><span data-ttu-id="18f91-171">Asynchronní dotaz</span><span class="sxs-lookup"><span data-stu-id="18f91-171">Async query</span></span>

<span data-ttu-id="18f91-172">Může uvolnit aktuální vlákno (a přidružené prostředky) pro zpracování dalších požadavků, zatímco databáze zpracovává dotaz.</span><span class="sxs-lookup"><span data-stu-id="18f91-172">Can free up the current thread (and it's associated resources) to process other requests while the database processes the query.</span></span>

### <a name="raw-sql-queries"></a><span data-ttu-id="18f91-173">Nezpracované dotazy SQL</span><span class="sxs-lookup"><span data-stu-id="18f91-173">Raw SQL queries</span></span>

<span data-ttu-id="18f91-174">Poskytuje metodu `DbSet.FromSql` pro použití nezpracovaných dotazů SQL k načtení dat.</span><span class="sxs-lookup"><span data-stu-id="18f91-174">Provides the `DbSet.FromSql` method to use raw SQL queries to fetch data.</span></span> <span data-ttu-id="18f91-175">Tyto dotazy mohou být také vytvořeny pomocí LINQ.</span><span class="sxs-lookup"><span data-stu-id="18f91-175">These queries can also be composed on using LINQ.</span></span>

## <a name="database-schema-management"></a><span data-ttu-id="18f91-176">Správa schématu databáze</span><span class="sxs-lookup"><span data-stu-id="18f91-176">Database schema management</span></span>

### <a name="database-creationdeletion-apis"></a><span data-ttu-id="18f91-177">Rozhraní API pro vytváření a odstraňování databází</span><span class="sxs-lookup"><span data-stu-id="18f91-177">Database creation/deletion APIs</span></span>

<span data-ttu-id="18f91-178">Jsou převážně navržené pro testování, kde chcete rychle vytvořit nebo odstranit databázi bez použití migrace.</span><span class="sxs-lookup"><span data-stu-id="18f91-178">Are mostly designed for testing where you want to quickly create/delete the database without using migrations.</span></span>

### <a name="relational-database-migrations"></a><span data-ttu-id="18f91-179">Migrace relačních databází</span><span class="sxs-lookup"><span data-stu-id="18f91-179">Relational database migrations</span></span>

<span data-ttu-id="18f91-180">Povolí schématu relační databáze vyvíjet přesčasovou práci při změnách modelu.</span><span class="sxs-lookup"><span data-stu-id="18f91-180">Allow a relational database schema to evolve overtime as your model changes.</span></span>

### <a name="reverse-engineer-from-database"></a><span data-ttu-id="18f91-181">Zpětná analýza z databáze</span><span class="sxs-lookup"><span data-stu-id="18f91-181">Reverse engineer from database</span></span>

<span data-ttu-id="18f91-182">Generuje základní generátory modelu EF na základě stávajícího schématu relační databáze.</span><span class="sxs-lookup"><span data-stu-id="18f91-182">Scaffolds an EF model based on an existing relational database schema.</span></span>

## <a name="database-providers"></a><span data-ttu-id="18f91-183">Poskytovatelé databází</span><span class="sxs-lookup"><span data-stu-id="18f91-183">Database providers</span></span>

### <a name="sql-server"></a><span data-ttu-id="18f91-184">SQL Server</span><span class="sxs-lookup"><span data-stu-id="18f91-184">SQL Server</span></span>

<span data-ttu-id="18f91-185">Připojí se k Microsoft SQL Server 2008 a vyšší.</span><span class="sxs-lookup"><span data-stu-id="18f91-185">Connects to Microsoft SQL Server 2008 onwards.</span></span>

### <a name="sqlite"></a><span data-ttu-id="18f91-186">SQLite</span><span class="sxs-lookup"><span data-stu-id="18f91-186">SQLite</span></span>

<span data-ttu-id="18f91-187">Připojí se k databázi SQLite 3.</span><span class="sxs-lookup"><span data-stu-id="18f91-187">Connects to a SQLite 3 database.</span></span>

### <a name="in-memory"></a><span data-ttu-id="18f91-188">V paměti</span><span class="sxs-lookup"><span data-stu-id="18f91-188">In-Memory</span></span>

<span data-ttu-id="18f91-189">Je navržený tak, aby se testování snadno povolilo bez připojení ke skutečné databázi.</span><span class="sxs-lookup"><span data-stu-id="18f91-189">Is designed to easily enable testing without connecting to a real database.</span></span>

### <a name="3rd-party-providers"></a><span data-ttu-id="18f91-190">poskytovatelé třetích stran</span><span class="sxs-lookup"><span data-stu-id="18f91-190">3rd party providers</span></span>

<span data-ttu-id="18f91-191">K dispozici je několik poskytovatelů pro jiné databázové moduly.</span><span class="sxs-lookup"><span data-stu-id="18f91-191">Several providers are available for other database engines.</span></span> <span data-ttu-id="18f91-192">Úplný seznam najdete v tématu [poskytovatelé databáze](../providers/index.md) .</span><span class="sxs-lookup"><span data-stu-id="18f91-192">See [Database Providers](../providers/index.md) for a complete list.</span></span>
