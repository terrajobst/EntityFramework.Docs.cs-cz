---
title: Rozšíření & nástrojů – EF Core
author: ErikEJ
ms.date: 12/17/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: bab725afffe1fbf9f8c0abeef58579ac9dc842d2
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502079"
---
# <a name="ef-core-tools--extensions"></a><span data-ttu-id="71864-102">Rozšíření & nástrojů pro EF Core</span><span class="sxs-lookup"><span data-stu-id="71864-102">EF Core Tools & Extensions</span></span>

<span data-ttu-id="71864-103">Tyto nástroje a rozšíření poskytují další funkce pro Entity Framework Core 2,1 a novější.</span><span class="sxs-lookup"><span data-stu-id="71864-103">These tools and extensions provide additional functionality for Entity Framework Core 2.1 and later.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="71864-104">Rozšíření jsou sestavená z nejrůznějších zdrojů, která nejsou udržována jako součást projektu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="71864-104">Extensions are built by a variety of sources and aren't maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="71864-105">Při zvažování rozšíření jiného výrobce nezapomeňte vyhodnotit jeho kvalitu, licencování, kompatibilitu, podporu atd., abyste zajistili, že splňuje vaše požadavky.</span><span class="sxs-lookup"><span data-stu-id="71864-105">When considering a third party extension, be sure to evaluate its quality, licensing, compatibility, support, etc. to ensure it meets your requirements.</span></span> <span data-ttu-id="71864-106">Konkrétně rozšíření sestavené pro starší verzi EF Core může vyžadovat aktualizaci před tím, než bude fungovat s nejnovějšími verzemi.</span><span class="sxs-lookup"><span data-stu-id="71864-106">In particular, an extension built for an older version of EF Core may need updating before it will work with the latest versions.</span></span>

## <a name="tools"></a><span data-ttu-id="71864-107">Nástroje</span><span class="sxs-lookup"><span data-stu-id="71864-107">Tools</span></span>

### <a name="llblgen-pro"></a><span data-ttu-id="71864-108">LLBLGen pro</span><span class="sxs-lookup"><span data-stu-id="71864-108">LLBLGen Pro</span></span>

<span data-ttu-id="71864-109">LLBLGen pro je řešení modelování entit s podporou Entity Framework a Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="71864-109">LLBLGen Pro is an entity modeling solution with support for Entity Framework and Entity Framework Core.</span></span> <span data-ttu-id="71864-110">Umožňuje snadno definovat model entity a namapovat ho do vaší databáze, použít nejprve databázi nebo model, abyste mohli začít psát dotazy hned.</span><span class="sxs-lookup"><span data-stu-id="71864-110">It lets you easily define your entity model and map it to your database, using database first or model first, so you can get started writing queries right away.</span></span> <span data-ttu-id="71864-111">Pro EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="71864-111">For EF Core: 2.</span></span>

[<span data-ttu-id="71864-112">Web</span><span class="sxs-lookup"><span data-stu-id="71864-112">Website</span></span>](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a><span data-ttu-id="71864-113">Vývojář entit Devart</span><span class="sxs-lookup"><span data-stu-id="71864-113">Devart Entity Developer</span></span>

<span data-ttu-id="71864-114">Vývojář entit je výkonný Návrhář ORM pro ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik přístup k datům a LINQ to SQL.</span><span class="sxs-lookup"><span data-stu-id="71864-114">Entity Developer is a powerful ORM designer for ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access, and LINQ to SQL.</span></span> <span data-ttu-id="71864-115">Podporuje navrhování EF Corech modelů vizuálně, použití prvního nebo databáze prvního přístupového C# modelu nebo generování kódu Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="71864-115">It supports designing EF Core models visually, using model first or database first approaches, and C# or Visual Basic code generation.</span></span> <span data-ttu-id="71864-116">Pro EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="71864-116">For EF Core: 2.</span></span>

[<span data-ttu-id="71864-117">Web</span><span class="sxs-lookup"><span data-stu-id="71864-117">Website</span></span>](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a><span data-ttu-id="71864-118">EF Core nástroje Power Tools</span><span class="sxs-lookup"><span data-stu-id="71864-118">EF Core Power Tools</span></span>

<span data-ttu-id="71864-119">EF Core Power Tools je rozšíření sady Visual Studio, které zpřístupňuje různé úlohy EF Core v době návrhu v jednoduchém uživatelském rozhraní.</span><span class="sxs-lookup"><span data-stu-id="71864-119">EF Core Power Tools is a Visual Studio extension that exposes various EF Core design-time tasks in a simple user interface.</span></span> <span data-ttu-id="71864-120">Zahrnuje zpětnou přípravu tříd DbContext a entit z existujících databází a [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), správu migrací databáze a vizualizace modelů.</span><span class="sxs-lookup"><span data-stu-id="71864-120">It includes reverse engineering of DbContext and entity classes from existing databases and [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), management of database migrations, and model visualizations.</span></span> <span data-ttu-id="71864-121">Pro EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="71864-121">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="71864-122">Wiki GitHubu</span><span class="sxs-lookup"><span data-stu-id="71864-122">GitHub wiki</span></span>](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a><span data-ttu-id="71864-123">Entity Framework vizuální Editor</span><span class="sxs-lookup"><span data-stu-id="71864-123">Entity Framework Visual Editor</span></span>

<span data-ttu-id="71864-124">Entity Framework Visual Editor je rozšíření sady Visual Studio, které přidává návrháře ORM pro vizuální návrh z EF 6 a EF Core třídy.</span><span class="sxs-lookup"><span data-stu-id="71864-124">Entity Framework Visual Editor is a Visual Studio extension that adds an ORM designer for visual design of EF 6, and EF Core classes.</span></span> <span data-ttu-id="71864-125">Kód se generuje pomocí šablon T4, takže ho můžete přizpůsobit podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="71864-125">Code is generated using T4 templates so can be customized to suit any needs.</span></span> <span data-ttu-id="71864-126">Podporuje dědičnost, jednosměrná a obousměrná přidružení, výčty a možnost barevně kódovat vaše třídy a přidat textové bloky, které vysvětlují potenciálně učit složité části návrhu.</span><span class="sxs-lookup"><span data-stu-id="71864-126">It supports inheritance, unidirectional and bidirectional associations, enumerations, and the ability to color-code your classes and add text blocks to explain potentially arcane parts of your design.</span></span> <span data-ttu-id="71864-127">Pro EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="71864-127">For EF Core: 2.</span></span>

[<span data-ttu-id="71864-128">Marketplace</span><span class="sxs-lookup"><span data-stu-id="71864-128">Marketplace</span></span>](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a><span data-ttu-id="71864-129">CatFactory</span><span class="sxs-lookup"><span data-stu-id="71864-129">CatFactory</span></span>

<span data-ttu-id="71864-130">CatFactory je modul pro generování uživatelského rozhraní pro .NET Core, který může automatizovat generování tříd DbContext, entit, mapování konfigurace a tříd úložiště z databáze SQL Server.</span><span class="sxs-lookup"><span data-stu-id="71864-130">CatFactory is a scaffolding engine for .NET Core that can automate the generation of DbContext classes, entities, mapping configurations, and repository classes from a SQL Server database.</span></span> <span data-ttu-id="71864-131">Pro EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="71864-131">For EF Core: 2.</span></span>

[<span data-ttu-id="71864-132">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="71864-132">GitHub repository</span></span>](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a><span data-ttu-id="71864-133">Generátor Entity Framework Core LoreSoft</span><span class="sxs-lookup"><span data-stu-id="71864-133">LoreSoft's Entity Framework Core Generator</span></span>

<span data-ttu-id="71864-134">Generátor Entity Framework Core (EFG) je .NET Core CLI nástroj, který může generovat EF Core modely z existující databáze, podobně jako `dotnet ef dbcontext scaffold`, ale také podporuje bezpečné [obnovení](https://efg.loresoft.com/en/latest/regeneration/) kódu prostřednictvím nahrazení oblastí nebo analýzou souborů mapování.</span><span class="sxs-lookup"><span data-stu-id="71864-134">Entity Framework Core Generator (efg) is a .NET Core CLI tool that can generate EF Core models from an existing database, much like `dotnet ef dbcontext scaffold`, but it also supports safe code [regeneration](https://efg.loresoft.com/en/latest/regeneration/) via region replacement or by parsing mapping files.</span></span> <span data-ttu-id="71864-135">Tento nástroj podporuje generování modelů zobrazení, ověřování a kódu mapovače objektů.</span><span class="sxs-lookup"><span data-stu-id="71864-135">This tool supports generating view models, validation, and object mapper code.</span></span> <span data-ttu-id="71864-136">Pro EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="71864-136">For EF Core: 2.</span></span>

<span data-ttu-id="71864-137">[Dokumentace](https://efg.loresoft.com/en/latest/)
[kurzu](https://www.loresoft.com/Generate-ASP-NET-Web-API)</span><span class="sxs-lookup"><span data-stu-id="71864-137">[Tutorial](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Documentation](https://efg.loresoft.com/en/latest/)</span></span>

## <a name="extensions"></a><span data-ttu-id="71864-138">Rozšíření</span><span class="sxs-lookup"><span data-stu-id="71864-138">Extensions</span></span>

### <a name="microsoftentityframeworkcoreautohistory"></a><span data-ttu-id="71864-139">Microsoft.EntityFrameworkCore.AutoHistory</span><span class="sxs-lookup"><span data-stu-id="71864-139">Microsoft.EntityFrameworkCore.AutoHistory</span></span>

<span data-ttu-id="71864-140">Knihovna modulů plug-in, která umožňuje automatické zaznamenávání změn dat provedených EF Core do tabulky historie.</span><span class="sxs-lookup"><span data-stu-id="71864-140">A plugin library that enables automatically recording the data changes performed by EF Core into a history table.</span></span> <span data-ttu-id="71864-141">Pro EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="71864-141">For EF Core: 2.</span></span>

[<span data-ttu-id="71864-142">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="71864-142">GitHub repository</span></span>](https://github.com/Arch/AutoHistory/)

### <a name="efsecondlevelcachecore"></a><span data-ttu-id="71864-143">EFSecondLevelCache.Core</span><span class="sxs-lookup"><span data-stu-id="71864-143">EFSecondLevelCache.Core</span></span>

<span data-ttu-id="71864-144">Rozšíření, které umožňuje ukládat výsledky EF Core dotazů do mezipaměti druhé úrovně, aby následné spouštění stejných dotazů se mohli vyhnout přístupu k databázi a načíst data přímo z mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="71864-144">An extension that enables storing the results of EF Core queries into a second-level cache, so that subsequent executions of the same queries can avoid accessing the database and retrieve the data directly from the cache.</span></span> <span data-ttu-id="71864-145">Pro EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="71864-145">For EF Core: 2.</span></span>

[<span data-ttu-id="71864-146">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="71864-146">GitHub repository</span></span>](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="geco"></a><span data-ttu-id="71864-147">Geco</span><span class="sxs-lookup"><span data-stu-id="71864-147">Geco</span></span>

<span data-ttu-id="71864-148">GECO (konzola generátoru) je jednoduchý generátor kódu založený na projektu konzoly, který běží na .NET Core a používá C# interpolované řetězce pro generování kódu.</span><span class="sxs-lookup"><span data-stu-id="71864-148">Geco (Generator Console) is a simple code generator based on a console project, that runs on .NET Core and uses C# interpolated strings for code generation.</span></span> <span data-ttu-id="71864-149">GECO zahrnuje generátor reverzních modelů pro EF Core s podporou pro pluralitování, jednotné navýšení a upravitelnou šablonu.</span><span class="sxs-lookup"><span data-stu-id="71864-149">Geco includes a reverse model generator for EF Core with support for pluralization, singularization, and editable templates.</span></span> <span data-ttu-id="71864-150">Poskytuje také generátor počátečních dat, spouštěč skriptů a čistič databáze.</span><span class="sxs-lookup"><span data-stu-id="71864-150">It also provides a seed data script generator, a script runner, and a database cleaner.</span></span> <span data-ttu-id="71864-151">Pro EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="71864-151">For EF Core: 2.</span></span>

[<span data-ttu-id="71864-152">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="71864-152">GitHub repository</span></span>](https://github.com/iQuarc/Geco)

### <a name="entityframeworkcorescaffoldinghandlebars"></a><span data-ttu-id="71864-153">EntityFrameworkCore. Lešeníing. handlebars</span><span class="sxs-lookup"><span data-stu-id="71864-153">EntityFrameworkCore.Scaffolding.Handlebars</span></span> 

<span data-ttu-id="71864-154">Umožňuje přizpůsobit třídy zpětnou analýzou z existující databáze pomocí Entity Framework Core sada nástrojů s handlebars šablonami.</span><span class="sxs-lookup"><span data-stu-id="71864-154">Allows customization of classes reverse engineered from an existing database using the Entity Framework Core toolchain with Handlebars templates.</span></span> <span data-ttu-id="71864-155">Pro EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="71864-155">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="71864-156">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="71864-156">GitHub repository</span></span>](https://github.com/TrackableEntities/EntityFrameworkCore.Scaffolding.Handlebars)

### <a name="neinlinqentityframeworkcore"></a><span data-ttu-id="71864-157">NeinLinq.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="71864-157">NeinLinq.EntityFrameworkCore</span></span> 

<span data-ttu-id="71864-158">NeinLinq rozšiřuje zprostředkovatele LINQ, jako je například Entity Framework, aby umožnila opakované použití funkcí, přepisování dotazů a sestavování dynamických dotazů pomocí přeložitelných predikátů a selektorů.</span><span class="sxs-lookup"><span data-stu-id="71864-158">NeinLinq extends LINQ providers such as Entity Framework to enable reusing functions, rewriting queries, and building dynamic queries using translatable predicates and selectors.</span></span> <span data-ttu-id="71864-159">Pro EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="71864-159">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="71864-160">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="71864-160">GitHub repository</span></span>](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a><span data-ttu-id="71864-161">Microsoft.EntityFrameworkCore.UnitOfWork</span><span class="sxs-lookup"><span data-stu-id="71864-161">Microsoft.EntityFrameworkCore.UnitOfWork</span></span>

<span data-ttu-id="71864-162">Modul plug-in pro Microsoft. EntityFrameworkCore pro podporu úložiště, pracovních schémat a více databází s podporou distribuované transakce.</span><span class="sxs-lookup"><span data-stu-id="71864-162">A plugin for Microsoft.EntityFrameworkCore to support repository, unit of work patterns, and multiple databases with distributed transaction supported.</span></span> <span data-ttu-id="71864-163">Pro EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="71864-163">For EF Core: 2.</span></span>

[<span data-ttu-id="71864-164">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="71864-164">GitHub repository</span></span>](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a><span data-ttu-id="71864-165">EFCore.BulkExtensions</span><span class="sxs-lookup"><span data-stu-id="71864-165">EFCore.BulkExtensions</span></span>

<span data-ttu-id="71864-166">Rozšíření EF Core pro hromadné operace (vložení, aktualizace, odstranění).</span><span class="sxs-lookup"><span data-stu-id="71864-166">EF Core extensions for Bulk operations (Insert, Update, Delete).</span></span> <span data-ttu-id="71864-167">Pro EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="71864-167">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="71864-168">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="71864-168">GitHub repository</span></span>](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a><span data-ttu-id="71864-169">Bricelam.EntityFrameworkCore.Pluralizer</span><span class="sxs-lookup"><span data-stu-id="71864-169">Bricelam.EntityFrameworkCore.Pluralizer</span></span>

<span data-ttu-id="71864-170">Přidá pluralitu v době návrhu.</span><span class="sxs-lookup"><span data-stu-id="71864-170">Adds design-time pluralization.</span></span> <span data-ttu-id="71864-171">Pro EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="71864-171">For EF Core: 2.</span></span>

[<span data-ttu-id="71864-172">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="71864-172">GitHub repository</span></span>](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="toolbeltentityframeworkcoreindexattribute"></a><span data-ttu-id="71864-173">Toolbelt.EntityFrameworkCore.IndexAttribute</span><span class="sxs-lookup"><span data-stu-id="71864-173">Toolbelt.EntityFrameworkCore.IndexAttribute</span></span>

<span data-ttu-id="71864-174">Revival atributu [index] (s příponou pro sestavení modelu).</span><span class="sxs-lookup"><span data-stu-id="71864-174">Revival of [Index] attribute (with extension for model building).</span></span> <span data-ttu-id="71864-175">Pro EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="71864-175">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="71864-176">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="71864-176">GitHub repository</span></span>](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a><span data-ttu-id="71864-177">EfCore.InMemoryHelpers</span><span class="sxs-lookup"><span data-stu-id="71864-177">EfCore.InMemoryHelpers</span></span>

<span data-ttu-id="71864-178">Poskytuje obálku kolem EF Coreho poskytovatele databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="71864-178">Provides a wrapper around the EF Core In-Memory Database Provider.</span></span> <span data-ttu-id="71864-179">Díky tomu bude fungovat podobně jako u relačního poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="71864-179">Makes it act more like a relational provider.</span></span> <span data-ttu-id="71864-180">Pro EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="71864-180">For EF Core: 2.</span></span>

[<span data-ttu-id="71864-181">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="71864-181">GitHub repository</span></span>](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a><span data-ttu-id="71864-182">EFCore.TemporalSupport</span><span class="sxs-lookup"><span data-stu-id="71864-182">EFCore.TemporalSupport</span></span>

<span data-ttu-id="71864-183">Implementace dočasné podpory.</span><span class="sxs-lookup"><span data-stu-id="71864-183">An implementation of temporal support.</span></span> <span data-ttu-id="71864-184">Pro EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="71864-184">For EF Core: 2.</span></span>

[<span data-ttu-id="71864-185">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="71864-185">GitHub repository</span></span>](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="efcoretemporaltable"></a><span data-ttu-id="71864-186">EfCoreTemporalTable</span><span class="sxs-lookup"><span data-stu-id="71864-186">EfCoreTemporalTable</span></span>

<span data-ttu-id="71864-187">Pomocí představených metod rozšíření můžete snadno provádět dočasné dotazy na oblíbené databázi: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)``AsTemporalContained(startDate, endDate)`.</span><span class="sxs-lookup"><span data-stu-id="71864-187">Easily perform temporal queries on your favourite database using introduced extension methods: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)`, `AsTemporalContained(startDate, endDate)`.</span></span> <span data-ttu-id="71864-188">Pro EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="71864-188">For EF Core: 3.</span></span>

[<span data-ttu-id="71864-189">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="71864-189">GitHub repository</span></span>](https://github.com/glautrou/EfCoreTemporalTable)

### <a name="efcoretimetraveler"></a><span data-ttu-id="71864-190">EFCore.TimeTraveler</span><span class="sxs-lookup"><span data-stu-id="71864-190">EFCore.TimeTraveler</span></span>

<span data-ttu-id="71864-191">Povolí plně funkční Entity Framework Core dotazy pro SQL Server dočasnou [historii](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) pomocí kódu EF Core, entit a mapování, které jste už definovali.</span><span class="sxs-lookup"><span data-stu-id="71864-191">Allow full-featured Entity Framework Core queries against [SQL Server Temporal History](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) using the EF Core code, entities, and mappings you already have defined.</span></span>  <span data-ttu-id="71864-192">Procházením kódu v `using (TemporalQuery.AsOf(targetDateTime)) {...}`se procházejte časem.</span><span class="sxs-lookup"><span data-stu-id="71864-192">Travel through time by wrapping your code in `using (TemporalQuery.AsOf(targetDateTime)) {...}`.</span></span> <span data-ttu-id="71864-193">Pro EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="71864-193">For EF Core: 3.</span></span>

[<span data-ttu-id="71864-194">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="71864-194">GitHub repository</span></span>](https://github.com/VantageSoftware/EFCore.TimeTraveler)


### <a name="entityframeworkcoretemporaltables"></a><span data-ttu-id="71864-195">EntityFrameworkCore.TemporalTables</span><span class="sxs-lookup"><span data-stu-id="71864-195">EntityFrameworkCore.TemporalTables</span></span>

<span data-ttu-id="71864-196">Knihovna rozšíření pro Entity Framework Core, která umožňuje vývojářům, kteří používají SQL Server, snadno používat dočasné tabulky.</span><span class="sxs-lookup"><span data-stu-id="71864-196">Extension library for Entity Framework Core which allows developers who use SQL Server to easily use temporal tables.</span></span> <span data-ttu-id="71864-197">Pro EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="71864-197">For EF Core: 2.</span></span>

[<span data-ttu-id="71864-198">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="71864-198">GitHub repository</span></span>](https://github.com/findulov/EntityFrameworkCore.TemporalTables)


### <a name="entityframeworkcorecacheable"></a><span data-ttu-id="71864-199">EntityFrameworkCore.Cacheable</span><span class="sxs-lookup"><span data-stu-id="71864-199">EntityFrameworkCore.Cacheable</span></span>

<span data-ttu-id="71864-200">Vysoce výkonná mezipaměť dotazů druhé úrovně.</span><span class="sxs-lookup"><span data-stu-id="71864-200">A high-performance second-level query cache.</span></span> <span data-ttu-id="71864-201">Pro EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="71864-201">For EF Core: 2.</span></span>

[<span data-ttu-id="71864-202">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="71864-202">GitHub repository</span></span>](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entity-framework-plus"></a><span data-ttu-id="71864-203">Entity Framework plus</span><span class="sxs-lookup"><span data-stu-id="71864-203">Entity Framework Plus</span></span>

<span data-ttu-id="71864-204">Rozšiřuje vaše DbContext o funkce, jako například: zahrnutí filtru, auditování, ukládání do mezipaměti, budoucí dotaz, dávkové odstranění, dávkové aktualizace a další.</span><span class="sxs-lookup"><span data-stu-id="71864-204">Extends your DbContext with features such as: Include Filter, Auditing, Caching, Query Future, Batch Delete, Batch Update, and more.</span></span> <span data-ttu-id="71864-205">Pro EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="71864-205">For EF Core: 2, 3.</span></span>

<span data-ttu-id="71864-206">[Web](https://entityframework-plus.net/)
[úložiště GitHub](https://github.com/zzzprojects/EntityFramework-Plus)</span><span class="sxs-lookup"><span data-stu-id="71864-206">[Website](https://entityframework-plus.net/)
[GitHub repository](https://github.com/zzzprojects/EntityFramework-Plus)</span></span>

### <a name="entity-framework-extensions"></a><span data-ttu-id="71864-207">Rozšíření Entity Framework</span><span class="sxs-lookup"><span data-stu-id="71864-207">Entity Framework Extensions</span></span>

<span data-ttu-id="71864-208">Rozšiřuje vaše DbContext o vysoce výkonné hromadné operace: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge a další.</span><span class="sxs-lookup"><span data-stu-id="71864-208">Extends your DbContext with high-performance bulk operations: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge, and more.</span></span> <span data-ttu-id="71864-209">Pro EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="71864-209">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="71864-210">Web</span><span class="sxs-lookup"><span data-stu-id="71864-210">Website</span></span>](https://entityframework-extensions.net/)
