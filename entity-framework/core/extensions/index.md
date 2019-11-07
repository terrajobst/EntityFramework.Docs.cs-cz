---
title: Rozšíření & nástrojů – EF Core
author: ErikEJ
ms.date: 01/07/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: e70011b42818e4df1ec5b9b88d7adb9d36bb26f1
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73654803"
---
# <a name="ef-core-tools--extensions"></a><span data-ttu-id="c392f-102">Rozšíření & nástrojů pro EF Core</span><span class="sxs-lookup"><span data-stu-id="c392f-102">EF Core Tools & Extensions</span></span>

<span data-ttu-id="c392f-103">Tyto nástroje a rozšíření poskytují další funkce pro Entity Framework Core 2,0 a novější.</span><span class="sxs-lookup"><span data-stu-id="c392f-103">These tools and extensions provide additional functionality for Entity Framework Core 2.0 and later.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="c392f-104">Rozšíření jsou sestavená z nejrůznějších zdrojů, která nejsou udržována jako součást projektu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="c392f-104">Extensions are built by a variety of sources and aren't maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="c392f-105">Při zvažování rozšíření jiného výrobce nezapomeňte vyhodnotit jeho kvalitu, licencování, kompatibilitu, podporu atd., abyste zajistili, že splňuje vaše požadavky.</span><span class="sxs-lookup"><span data-stu-id="c392f-105">When considering a third party extension, be sure to evaluate its quality, licensing, compatibility, support, etc. to ensure it meets your requirements.</span></span>

## <a name="tools"></a><span data-ttu-id="c392f-106">Nástroje</span><span class="sxs-lookup"><span data-stu-id="c392f-106">Tools</span></span>

### <a name="llblgen-pro"></a><span data-ttu-id="c392f-107">LLBLGen pro</span><span class="sxs-lookup"><span data-stu-id="c392f-107">LLBLGen Pro</span></span>

<span data-ttu-id="c392f-108">LLBLGen pro je řešení modelování entit s podporou Entity Framework a Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="c392f-108">LLBLGen Pro is an entity modeling solution with support for Entity Framework and Entity Framework Core.</span></span> <span data-ttu-id="c392f-109">Umožňuje snadno definovat model entity a namapovat ho do vaší databáze, použít nejprve databázi nebo model, abyste mohli začít psát dotazy hned.</span><span class="sxs-lookup"><span data-stu-id="c392f-109">It lets you easily define your entity model and map it to your database, using database first or model first, so you can get started writing queries right away.</span></span>

[<span data-ttu-id="c392f-110">Webu</span><span class="sxs-lookup"><span data-stu-id="c392f-110">Website</span></span>](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a><span data-ttu-id="c392f-111">Vývojář entit Devart</span><span class="sxs-lookup"><span data-stu-id="c392f-111">Devart Entity Developer</span></span>

<span data-ttu-id="c392f-112">Vývojář entit je výkonný Návrhář ORM pro ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik přístup k datům a LINQ to SQL.</span><span class="sxs-lookup"><span data-stu-id="c392f-112">Entity Developer is a powerful ORM designer for ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access, and LINQ to SQL.</span></span> <span data-ttu-id="c392f-113">Podporuje navrhování EF Corech modelů vizuálně, použití prvního nebo databáze prvního přístupového C# modelu nebo generování kódu Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="c392f-113">It supports designing EF Core models visually, using model first or database first approaches, and C# or Visual Basic code generation.</span></span>

[<span data-ttu-id="c392f-114">Webu</span><span class="sxs-lookup"><span data-stu-id="c392f-114">Website</span></span>](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a><span data-ttu-id="c392f-115">EF Core nástroje Power Tools</span><span class="sxs-lookup"><span data-stu-id="c392f-115">EF Core Power Tools</span></span>

<span data-ttu-id="c392f-116">EF Core Power Tools je rozšíření sady Visual Studio 2017, které zpřístupňuje různé úlohy EF Core v době návrhu v jednoduchém uživatelském rozhraní.</span><span class="sxs-lookup"><span data-stu-id="c392f-116">EF Core Power Tools is a Visual Studio 2017 extension that exposes various EF Core design-time tasks in a simple user interface.</span></span> <span data-ttu-id="c392f-117">Zahrnuje zpětnou přípravu tříd DbContext a entit z existujících databází a [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), správu migrací databáze a vizualizace modelů.</span><span class="sxs-lookup"><span data-stu-id="c392f-117">It includes reverse engineering of DbContext and entity classes from existing databases and [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), management of database migrations, and model visualizations.</span></span>

[<span data-ttu-id="c392f-118">Wiki GitHubu</span><span class="sxs-lookup"><span data-stu-id="c392f-118">GitHub wiki</span></span>](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a><span data-ttu-id="c392f-119">Entity Framework vizuální Editor</span><span class="sxs-lookup"><span data-stu-id="c392f-119">Entity Framework Visual Editor</span></span>

<span data-ttu-id="c392f-120">Entity Framework Visual Editor je rozšíření sady Visual Studio, které přidává návrháře ORM pro vizuální návrh z EF 6 a EF Core třídy.</span><span class="sxs-lookup"><span data-stu-id="c392f-120">Entity Framework Visual Editor is a Visual Studio extension that adds an ORM designer for visual design of EF 6, and EF Core classes.</span></span> <span data-ttu-id="c392f-121">Kód se generuje pomocí šablon T4, takže ho můžete přizpůsobit podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="c392f-121">Code is generated using T4 templates so can be customized to suit any needs.</span></span> <span data-ttu-id="c392f-122">Podporuje dědičnost, jednosměrná a obousměrná přidružení, výčty a možnost barevně kódovat vaše třídy a přidat textové bloky, které vysvětlují potenciálně učit složité části návrhu.</span><span class="sxs-lookup"><span data-stu-id="c392f-122">It supports inheritance, unidirectional and bidirectional associations, enumerations, and the ability to color-code your classes and add text blocks to explain potentially arcane parts of your design.</span></span>

[<span data-ttu-id="c392f-123">Marketplace</span><span class="sxs-lookup"><span data-stu-id="c392f-123">Marketplace</span></span>](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a><span data-ttu-id="c392f-124">CatFactory</span><span class="sxs-lookup"><span data-stu-id="c392f-124">CatFactory</span></span>

<span data-ttu-id="c392f-125">CatFactory je modul pro generování uživatelského rozhraní pro .NET Core, který může automatizovat generování tříd DbContext, entit, mapování konfigurace a tříd úložiště z databáze SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c392f-125">CatFactory is a scaffolding engine for .NET Core that can automate the generation of DbContext classes, entities, mapping configurations, and repository classes from a SQL Server database.</span></span>

[<span data-ttu-id="c392f-126">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="c392f-126">GitHub repository</span></span>](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a><span data-ttu-id="c392f-127">Generátor Entity Framework Core LoreSoft</span><span class="sxs-lookup"><span data-stu-id="c392f-127">LoreSoft's Entity Framework Core Generator</span></span>

<span data-ttu-id="c392f-128">Generátor Entity Framework Core (EFG) je .NET Core CLI nástroj, který může generovat EF Core modely z existující databáze, podobně jako `dotnet ef dbcontext scaffold`, ale také podporuje bezpečné [obnovení](https://efg.loresoft.com/en/latest/regeneration/) kódu prostřednictvím nahrazení oblastí nebo analýzou souborů mapování.</span><span class="sxs-lookup"><span data-stu-id="c392f-128">Entity Framework Core Generator (efg) is a .NET Core CLI tool that can generate EF Core models from an existing database, much like `dotnet ef dbcontext scaffold`, but it also supports safe code [regeneration](https://efg.loresoft.com/en/latest/regeneration/) via region replacement or by parsing mapping files.</span></span> <span data-ttu-id="c392f-129">Tento nástroj podporuje generování modelů zobrazení, ověřování a kódu mapovače objektů.</span><span class="sxs-lookup"><span data-stu-id="c392f-129">This tool supports generating view models, validation, and object mapper code.</span></span>

<span data-ttu-id="c392f-130">[Dokumentace](https://efg.loresoft.com/en/latest/)
[kurzu](https://www.loresoft.com/Generate-ASP-NET-Web-API)</span><span class="sxs-lookup"><span data-stu-id="c392f-130">[Tutorial](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Documentation](https://efg.loresoft.com/en/latest/)</span></span>

## <a name="extensions"></a><span data-ttu-id="c392f-131">SND</span><span class="sxs-lookup"><span data-stu-id="c392f-131">Extensions</span></span>

### <a name="microsoftentityframeworkcoreautohistory"></a><span data-ttu-id="c392f-132">Microsoft. EntityFrameworkCore. AutoHistory</span><span class="sxs-lookup"><span data-stu-id="c392f-132">Microsoft.EntityFrameworkCore.AutoHistory</span></span>

<span data-ttu-id="c392f-133">Knihovna modulů plug-in, která umožňuje automatické zaznamenávání změn dat provedených EF Core do tabulky historie.</span><span class="sxs-lookup"><span data-stu-id="c392f-133">A plugin library that enables automatically recording the data changes performed by EF Core into a history table.</span></span>

[<span data-ttu-id="c392f-134">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="c392f-134">GitHub repository</span></span>](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a><span data-ttu-id="c392f-135">Microsoft. EntityFrameworkCore. DynamicLinq</span><span class="sxs-lookup"><span data-stu-id="c392f-135">Microsoft.EntityFrameworkCore.DynamicLinq</span></span>

<span data-ttu-id="c392f-136">Port .NET Core/.NET Standard System. Linq. Dynamic, který zahrnuje asynchronní podporu s EF Core.</span><span class="sxs-lookup"><span data-stu-id="c392f-136">A .NET Core / .NET Standard port of System.Linq.Dynamic that includes async support with EF Core.</span></span>
<span data-ttu-id="c392f-137">System. Linq. Dynamic pochází jako ukázka společnosti Microsoft, která ukazuje, jak dynamicky vytvářet dotazy LINQ z řetězcových výrazů namísto kódu.</span><span class="sxs-lookup"><span data-stu-id="c392f-137">System.Linq.Dynamic originated as a Microsoft sample that shows how to construct LINQ queries dynamically from string expressions rather than code.</span></span>

[<span data-ttu-id="c392f-138">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="c392f-138">GitHub repository</span></span>](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efsecondlevelcachecore"></a><span data-ttu-id="c392f-139">EFSecondLevelCache. Core</span><span class="sxs-lookup"><span data-stu-id="c392f-139">EFSecondLevelCache.Core</span></span>

<span data-ttu-id="c392f-140">Rozšíření, které umožňuje ukládat výsledky EF Core dotazů do mezipaměti druhé úrovně, aby následné spouštění stejných dotazů se mohli vyhnout přístupu k databázi a načíst data přímo z mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="c392f-140">An extension that enables storing the results of EF Core queries into a second-level cache, so that subsequent executions of the same queries can avoid accessing the database and retrieve the data directly from the cache.</span></span>

[<span data-ttu-id="c392f-141">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="c392f-141">GitHub repository</span></span>](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="entityframeworkcoreprimarykey"></a><span data-ttu-id="c392f-142">EntityFrameworkCore. PrimaryKey</span><span class="sxs-lookup"><span data-stu-id="c392f-142">EntityFrameworkCore.PrimaryKey</span></span>

<span data-ttu-id="c392f-143">Tato knihovna umožňuje načíst hodnoty primárního klíče (včetně složených klíčů) z libovolné entity jako slovníku.</span><span class="sxs-lookup"><span data-stu-id="c392f-143">This library allows retrieving the values of primary key (including composite keys) from any entity as a dictionary.</span></span>

[<span data-ttu-id="c392f-144">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="c392f-144">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcoretypedoriginalvalues"></a><span data-ttu-id="c392f-145">EntityFrameworkCore.TypedOriginalValues</span><span class="sxs-lookup"><span data-stu-id="c392f-145">EntityFrameworkCore.TypedOriginalValues</span></span>

<span data-ttu-id="c392f-146">Tato knihovna umožňuje silného typu přístupu k původním hodnotám vlastností entit.</span><span class="sxs-lookup"><span data-stu-id="c392f-146">This library enables strongly typed access to the original values of entity properties.</span></span>

[<span data-ttu-id="c392f-147">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="c392f-147">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a><span data-ttu-id="c392f-148">Geco</span><span class="sxs-lookup"><span data-stu-id="c392f-148">Geco</span></span>

<span data-ttu-id="c392f-149">GECO (konzola generátoru) je jednoduchý generátor kódu založený na projektu konzoly, který běží na .NET Core a používá C# interpolované řetězce pro generování kódu.</span><span class="sxs-lookup"><span data-stu-id="c392f-149">Geco (Generator Console) is a simple code generator based on a console project, that runs on .NET Core and uses C# interpolated strings for code generation.</span></span> <span data-ttu-id="c392f-150">GECO zahrnuje generátor reverzních modelů pro EF Core s podporou pro pluralitování, jednotné navýšení a upravitelnou šablonu.</span><span class="sxs-lookup"><span data-stu-id="c392f-150">Geco includes a reverse model generator for EF Core with support for pluralization, singularization, and editable templates.</span></span> <span data-ttu-id="c392f-151">Poskytuje také generátor počátečních dat, spouštěč skriptů a čistič databáze.</span><span class="sxs-lookup"><span data-stu-id="c392f-151">It also provides a seed data script generator, a script runner, and a database cleaner.</span></span>

[<span data-ttu-id="c392f-152">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="c392f-152">GitHub repository</span></span>](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a><span data-ttu-id="c392f-153">LinqKit. Microsoft. EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="c392f-153">LinqKit.Microsoft.EntityFrameworkCore</span></span>

<span data-ttu-id="c392f-154">LinqKit. Microsoft. EntityFrameworkCore je verze knihovny LINQKit kompatibilní s EF Core.</span><span class="sxs-lookup"><span data-stu-id="c392f-154">LinqKit.Microsoft.EntityFrameworkCore is an EF Core-compatible version of the LINQKit library.</span></span> <span data-ttu-id="c392f-155">LINQKit je bezplatná sada rozšíření pro LINQ to SQL a Entity Framework Power Users.</span><span class="sxs-lookup"><span data-stu-id="c392f-155">LINQKit is a free set of extensions for LINQ to SQL and Entity Framework power users.</span></span> <span data-ttu-id="c392f-156">Umožňuje pokročilé funkce, jako je dynamické sestavování výrazů predikátů, a používání proměnných výrazu v poddotazech.</span><span class="sxs-lookup"><span data-stu-id="c392f-156">It enables advanced functionality like dynamic building of predicate expressions, and using expression variables in subqueries.</span></span>  

[<span data-ttu-id="c392f-157">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="c392f-157">GitHub repository</span></span>](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a><span data-ttu-id="c392f-158">NeinLinq.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="c392f-158">NeinLinq.EntityFrameworkCore</span></span>

<span data-ttu-id="c392f-159">NeinLinq rozšiřuje zprostředkovatele LINQ, jako je například Entity Framework, aby umožnila opakované použití funkcí, přepisování dotazů a sestavování dynamických dotazů pomocí přeložitelných predikátů a selektorů.</span><span class="sxs-lookup"><span data-stu-id="c392f-159">NeinLinq extends LINQ providers such as Entity Framework to enable reusing functions, rewriting queries, and building dynamic queries using translatable predicates and selectors.</span></span>

[<span data-ttu-id="c392f-160">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="c392f-160">GitHub repository</span></span>](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a><span data-ttu-id="c392f-161">Microsoft. EntityFrameworkCore. UnitOfWork</span><span class="sxs-lookup"><span data-stu-id="c392f-161">Microsoft.EntityFrameworkCore.UnitOfWork</span></span>

<span data-ttu-id="c392f-162">Modul plug-in pro Microsoft. EntityFrameworkCore pro podporu úložiště, pracovních schémat a více databází s podporou distribuované transakce.</span><span class="sxs-lookup"><span data-stu-id="c392f-162">A plugin for Microsoft.EntityFrameworkCore to support repository, unit of work patterns, and multiple databases with distributed transaction supported.</span></span>

[<span data-ttu-id="c392f-163">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="c392f-163">GitHub repository</span></span>](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a><span data-ttu-id="c392f-164">EFCore.BulkExtensions</span><span class="sxs-lookup"><span data-stu-id="c392f-164">EFCore.BulkExtensions</span></span>

<span data-ttu-id="c392f-165">Rozšíření EF Core pro hromadné operace (vložení, aktualizace, odstranění).</span><span class="sxs-lookup"><span data-stu-id="c392f-165">EF Core extensions for Bulk operations (Insert, Update, Delete).</span></span>

[<span data-ttu-id="c392f-166">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="c392f-166">GitHub repository</span></span>](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a><span data-ttu-id="c392f-167">Bricelam.EntityFrameworkCore.Pluralizer</span><span class="sxs-lookup"><span data-stu-id="c392f-167">Bricelam.EntityFrameworkCore.Pluralizer</span></span>

<span data-ttu-id="c392f-168">Přidá do EF Core dobu trvání návrhu.</span><span class="sxs-lookup"><span data-stu-id="c392f-168">Adds design-time pluralization to EF Core.</span></span>

[<span data-ttu-id="c392f-169">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="c392f-169">GitHub repository</span></span>](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="pomelofoundationpomeloentityframeworkcoreextensionstosql"></a><span data-ttu-id="c392f-170">PomeloFoundation/pomelo. EntityFrameworkCore. Extensions. ToSql</span><span class="sxs-lookup"><span data-stu-id="c392f-170">PomeloFoundation/Pomelo.EntityFrameworkCore.Extensions.ToSql</span></span>

<span data-ttu-id="c392f-171">Jednoduchá rozšiřující metoda, která získá příkaz SQL EF Core by generovala pro daný dotaz LINQ v jednoduchých scénářích.</span><span class="sxs-lookup"><span data-stu-id="c392f-171">A simple extension method that obtains the SQL statement EF Core would generate for a given LINQ query in simple scenarios.</span></span> <span data-ttu-id="c392f-172">Metoda ToSql je omezená na jednoduché scénáře, protože EF Core může generovat více než jeden příkaz SQL pro jeden dotaz LINQ a různé příkazy SQL závisející na hodnotách parametrů.</span><span class="sxs-lookup"><span data-stu-id="c392f-172">The ToSql method is limited to simple scenarios because EF Core can generate more than one SQL statement for a single LINQ query, and different SQL statements depending on parameter values.</span></span>

[<span data-ttu-id="c392f-173">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="c392f-173">GitHub repository</span></span>](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.Extensions.ToSql)

### <a name="toolbeltentityframeworkcoreindexattribute"></a><span data-ttu-id="c392f-174">Toolbelt.EntityFrameworkCore.IndexAttribute</span><span class="sxs-lookup"><span data-stu-id="c392f-174">Toolbelt.EntityFrameworkCore.IndexAttribute</span></span>

<span data-ttu-id="c392f-175">Revival atributu [index] pro EF Core (s příponou pro sestavení modelu).</span><span class="sxs-lookup"><span data-stu-id="c392f-175">Revival of [Index] attribute for EF Core (with extension for model building).</span></span>

[<span data-ttu-id="c392f-176">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="c392f-176">GitHub repository</span></span>](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a><span data-ttu-id="c392f-177">EfCore.InMemoryHelpers</span><span class="sxs-lookup"><span data-stu-id="c392f-177">EfCore.InMemoryHelpers</span></span>

<span data-ttu-id="c392f-178">Poskytuje obálku kolem EF Coreho poskytovatele databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="c392f-178">Provides a wrapper around the EF Core In-Memory Database Provider.</span></span> <span data-ttu-id="c392f-179">Díky tomu bude fungovat podobně jako u relačního poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="c392f-179">Makes it act more like a relational provider.</span></span>

[<span data-ttu-id="c392f-180">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="c392f-180">GitHub repository</span></span>](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a><span data-ttu-id="c392f-181">EFCore.TemporalSupport</span><span class="sxs-lookup"><span data-stu-id="c392f-181">EFCore.TemporalSupport</span></span>

<span data-ttu-id="c392f-182">Implementace dočasné podpory pro EF Core.</span><span class="sxs-lookup"><span data-stu-id="c392f-182">An implementation of temporal support for EF Core.</span></span>

[<span data-ttu-id="c392f-183">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="c392f-183">GitHub repository</span></span>](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="entityframeworkcorecacheable"></a><span data-ttu-id="c392f-184">EntityFrameworkCore. Cached</span><span class="sxs-lookup"><span data-stu-id="c392f-184">EntityFrameworkCore.Cacheable</span></span>

<span data-ttu-id="c392f-185">Vysoce výkonná mezipaměť dotazů druhé úrovně pro EF Core.</span><span class="sxs-lookup"><span data-stu-id="c392f-185">A high-performance second-level query cache for EF Core.</span></span>

[<span data-ttu-id="c392f-186">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="c392f-186">GitHub repository</span></span>](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entity-framework-plus"></a><span data-ttu-id="c392f-187">Entity Framework plus</span><span class="sxs-lookup"><span data-stu-id="c392f-187">Entity Framework Plus</span></span>

<span data-ttu-id="c392f-188">Rozšiřuje vaše DbContext o funkce, jako například: zahrnutí filtru, auditování, ukládání do mezipaměti, budoucí dotaz, dávkové odstranění, dávkové aktualizace a další.</span><span class="sxs-lookup"><span data-stu-id="c392f-188">Extends your DbContext with features such as: Include Filter, Auditing, Caching, Query Future, Batch Delete, Batch Update, and more.</span></span>

<span data-ttu-id="c392f-189">[Web](https://entityframework-plus.net/)
[úložiště GitHub](https://github.com/zzzprojects/EntityFramework-Plus)</span><span class="sxs-lookup"><span data-stu-id="c392f-189">[Website](https://entityframework-plus.net/)
[GitHub repository](https://github.com/zzzprojects/EntityFramework-Plus)</span></span>

### <a name="entity-framework-extensions"></a><span data-ttu-id="c392f-190">Rozšíření Entity Framework</span><span class="sxs-lookup"><span data-stu-id="c392f-190">Entity Framework Extensions</span></span>

<span data-ttu-id="c392f-191">Rozšiřuje vaše DbContext o vysoce výkonné hromadné operace: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge a další.</span><span class="sxs-lookup"><span data-stu-id="c392f-191">Extends your DbContext with high-performance bulk operations: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge, and more.</span></span>

[<span data-ttu-id="c392f-192">Webu</span><span class="sxs-lookup"><span data-stu-id="c392f-192">Website</span></span>](https://entityframework-extensions.net/)

### <a name="reconciler"></a><span data-ttu-id="c392f-193">Odsouhlasení</span><span class="sxs-lookup"><span data-stu-id="c392f-193">Reconciler</span></span>

<span data-ttu-id="c392f-194">Umožňuje aktualizovat graf entit v obchodě na daný, a to vložením, aktualizací a odebráním příslušných entit.</span><span class="sxs-lookup"><span data-stu-id="c392f-194">Update an entity graph in store to a given one by inserting, updating and removing the respective entities.</span></span>

[<span data-ttu-id="c392f-195">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="c392f-195">GitHub repository</span></span>](https://github.com/jtheisen/reconciler)
