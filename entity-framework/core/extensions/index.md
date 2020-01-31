---
title: Rozšíření & nástrojů – EF Core
author: ErikEJ
ms.date: 12/17/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: 99f59153a452a2f4aad5811110ebc5b5da7717ef
ms.sourcegitcommit: b3cf5d2e3cb170b9916795d1d8c88678269639b1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2020
ms.locfileid: "76888042"
---
# <a name="ef-core-tools--extensions"></a><span data-ttu-id="3d6d2-102">Rozšíření & nástrojů pro EF Core</span><span class="sxs-lookup"><span data-stu-id="3d6d2-102">EF Core Tools & Extensions</span></span>

<span data-ttu-id="3d6d2-103">Tyto nástroje a rozšíření poskytují další funkce pro Entity Framework Core 2,1 a novější.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-103">These tools and extensions provide additional functionality for Entity Framework Core 2.1 and later.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="3d6d2-104">Rozšíření jsou sestavená z nejrůznějších zdrojů, která nejsou udržována jako součást projektu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-104">Extensions are built by a variety of sources and aren't maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="3d6d2-105">Při zvažování rozšíření jiného výrobce nezapomeňte vyhodnotit jeho kvalitu, licencování, kompatibilitu, podporu atd., abyste zajistili, že splňuje vaše požadavky.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-105">When considering a third party extension, be sure to evaluate its quality, licensing, compatibility, support, etc. to ensure it meets your requirements.</span></span> <span data-ttu-id="3d6d2-106">Konkrétně rozšíření sestavené pro starší verzi EF Core může vyžadovat aktualizaci před tím, než bude fungovat s nejnovějšími verzemi.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-106">In particular, an extension built for an older version of EF Core may need updating before it will work with the latest versions.</span></span>

## <a name="tools"></a><span data-ttu-id="3d6d2-107">Nástroje</span><span class="sxs-lookup"><span data-stu-id="3d6d2-107">Tools</span></span>

### <a name="llblgen-pro"></a><span data-ttu-id="3d6d2-108">LLBLGen pro</span><span class="sxs-lookup"><span data-stu-id="3d6d2-108">LLBLGen Pro</span></span>

<span data-ttu-id="3d6d2-109">LLBLGen pro je řešení modelování entit s podporou Entity Framework a Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-109">LLBLGen Pro is an entity modeling solution with support for Entity Framework and Entity Framework Core.</span></span> <span data-ttu-id="3d6d2-110">Umožňuje snadno definovat model entity a namapovat ho do vaší databáze, použít nejprve databázi nebo model, abyste mohli začít psát dotazy hned.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-110">It lets you easily define your entity model and map it to your database, using database first or model first, so you can get started writing queries right away.</span></span> <span data-ttu-id="3d6d2-111">Pro EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-111">For EF Core: 2.</span></span>

[<span data-ttu-id="3d6d2-112">Web</span><span class="sxs-lookup"><span data-stu-id="3d6d2-112">Website</span></span>](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a><span data-ttu-id="3d6d2-113">Vývojář entit Devart</span><span class="sxs-lookup"><span data-stu-id="3d6d2-113">Devart Entity Developer</span></span>

<span data-ttu-id="3d6d2-114">Vývojář entit je výkonný Návrhář ORM pro ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik přístup k datům a LINQ to SQL.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-114">Entity Developer is a powerful ORM designer for ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access, and LINQ to SQL.</span></span> <span data-ttu-id="3d6d2-115">Podporuje navrhování EF Corech modelů vizuálně, použití prvního nebo databáze prvního přístupového C# modelu nebo generování kódu Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-115">It supports designing EF Core models visually, using model first or database first approaches, and C# or Visual Basic code generation.</span></span> <span data-ttu-id="3d6d2-116">Pro EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-116">For EF Core: 2.</span></span>

[<span data-ttu-id="3d6d2-117">Web</span><span class="sxs-lookup"><span data-stu-id="3d6d2-117">Website</span></span>](https://www.devart.com/entitydeveloper/)

### <a name="nhydrate-orm-for-entity-framework"></a><span data-ttu-id="3d6d2-118">nHydrate ORM pro Entity Framework</span><span class="sxs-lookup"><span data-stu-id="3d6d2-118">nHydrate ORM for Entity Framework</span></span>

<span data-ttu-id="3d6d2-119">Typ ORM, který vytváří silně typové a rozšířené třídy pro Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-119">An ORM that creates strongly-typed, extendable classes for Entity Framework.</span></span> <span data-ttu-id="3d6d2-120">Vygenerovaný kód je Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-120">The generated code is Entity Framework Core.</span></span> <span data-ttu-id="3d6d2-121">Neexistují žádné rozdíly.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-121">There is no difference.</span></span> <span data-ttu-id="3d6d2-122">Nejedná se o náhradu za EF nebo vlastní ORM.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-122">This is not a replacement for EF or a custom ORM.</span></span> <span data-ttu-id="3d6d2-123">Je to vizuální vrstva modelování, která umožňuje týmu spravovat složitá databázová schémata.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-123">It is a visual, modeling layer that allows a team to manage complex database schemas.</span></span> <span data-ttu-id="3d6d2-124">Funguje dobře se softwarem SCM, jako je git, a umožňuje tak přístup k vašemu modelu s minimálními konflikty pomocí více uživatelů.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-124">It works well with SCM software like Git, allowing multi-user access to your model with minimal conflicts.</span></span> <span data-ttu-id="3d6d2-125">Instalační program sleduje změny modelu a vytvoří skripty pro upgrade.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-125">The installer tracks model changes and creates upgrade scripts.</span></span> <span data-ttu-id="3d6d2-126">Pro EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-126">For EF Core: 3.</span></span>

[<span data-ttu-id="3d6d2-127">Web GitHubu</span><span class="sxs-lookup"><span data-stu-id="3d6d2-127">Github site</span></span>](https://github.com/nHydrate/nHydrate)

### <a name="ef-core-power-tools"></a><span data-ttu-id="3d6d2-128">EF Core nástroje Power Tools</span><span class="sxs-lookup"><span data-stu-id="3d6d2-128">EF Core Power Tools</span></span>

<span data-ttu-id="3d6d2-129">EF Core Power Tools je rozšíření sady Visual Studio, které zpřístupňuje různé úlohy EF Core v době návrhu v jednoduchém uživatelském rozhraní.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-129">EF Core Power Tools is a Visual Studio extension that exposes various EF Core design-time tasks in a simple user interface.</span></span> <span data-ttu-id="3d6d2-130">Zahrnuje zpětnou přípravu tříd DbContext a entit z existujících databází a [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), správu migrací databáze a vizualizace modelů.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-130">It includes reverse engineering of DbContext and entity classes from existing databases and [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), management of database migrations, and model visualizations.</span></span> <span data-ttu-id="3d6d2-131">Pro EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-131">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="3d6d2-132">Wiki GitHubu</span><span class="sxs-lookup"><span data-stu-id="3d6d2-132">GitHub wiki</span></span>](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a><span data-ttu-id="3d6d2-133">Entity Framework vizuální Editor</span><span class="sxs-lookup"><span data-stu-id="3d6d2-133">Entity Framework Visual Editor</span></span>

<span data-ttu-id="3d6d2-134">Entity Framework Visual Editor je rozšíření sady Visual Studio, které přidává návrháře ORM pro vizuální návrh z EF 6 a EF Core třídy.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-134">Entity Framework Visual Editor is a Visual Studio extension that adds an ORM designer for visual design of EF 6, and EF Core classes.</span></span> <span data-ttu-id="3d6d2-135">Kód se generuje pomocí šablon T4, takže ho můžete přizpůsobit podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-135">Code is generated using T4 templates so can be customized to suit any needs.</span></span> <span data-ttu-id="3d6d2-136">Podporuje dědičnost, jednosměrná a obousměrná přidružení, výčty a možnost barevně kódovat vaše třídy a přidat textové bloky, které vysvětlují potenciálně učit složité části návrhu.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-136">It supports inheritance, unidirectional and bidirectional associations, enumerations, and the ability to color-code your classes and add text blocks to explain potentially arcane parts of your design.</span></span> <span data-ttu-id="3d6d2-137">Pro EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-137">For EF Core: 2.</span></span>

[<span data-ttu-id="3d6d2-138">Marketplace</span><span class="sxs-lookup"><span data-stu-id="3d6d2-138">Marketplace</span></span>](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a><span data-ttu-id="3d6d2-139">CatFactory</span><span class="sxs-lookup"><span data-stu-id="3d6d2-139">CatFactory</span></span>

<span data-ttu-id="3d6d2-140">CatFactory je modul pro generování uživatelského rozhraní pro .NET Core, který může automatizovat generování tříd DbContext, entit, mapování konfigurace a tříd úložiště z databáze SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-140">CatFactory is a scaffolding engine for .NET Core that can automate the generation of DbContext classes, entities, mapping configurations, and repository classes from a SQL Server database.</span></span> <span data-ttu-id="3d6d2-141">Pro EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-141">For EF Core: 2.</span></span>

[<span data-ttu-id="3d6d2-142">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="3d6d2-142">GitHub repository</span></span>](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a><span data-ttu-id="3d6d2-143">Generátor Entity Framework Core LoreSoft</span><span class="sxs-lookup"><span data-stu-id="3d6d2-143">LoreSoft's Entity Framework Core Generator</span></span>

<span data-ttu-id="3d6d2-144">Generátor Entity Framework Core (EFG) je .NET Core CLI nástroj, který může generovat EF Core modely z existující databáze, podobně jako `dotnet ef dbcontext scaffold`, ale také podporuje bezpečné [obnovení](https://efg.loresoft.com/en/latest/regeneration/) kódu prostřednictvím nahrazení oblastí nebo analýzou souborů mapování.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-144">Entity Framework Core Generator (efg) is a .NET Core CLI tool that can generate EF Core models from an existing database, much like `dotnet ef dbcontext scaffold`, but it also supports safe code [regeneration](https://efg.loresoft.com/en/latest/regeneration/) via region replacement or by parsing mapping files.</span></span> <span data-ttu-id="3d6d2-145">Tento nástroj podporuje generování modelů zobrazení, ověřování a kódu mapovače objektů.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-145">This tool supports generating view models, validation, and object mapper code.</span></span> <span data-ttu-id="3d6d2-146">Pro EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-146">For EF Core: 2.</span></span>

<span data-ttu-id="3d6d2-147">[Dokumentace](https://efg.loresoft.com/en/latest/)
[kurzu](https://www.loresoft.com/Generate-ASP-NET-Web-API)</span><span class="sxs-lookup"><span data-stu-id="3d6d2-147">[Tutorial](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Documentation](https://efg.loresoft.com/en/latest/)</span></span>

## <a name="extensions"></a><span data-ttu-id="3d6d2-148">Rozšíření</span><span class="sxs-lookup"><span data-stu-id="3d6d2-148">Extensions</span></span>

### <a name="microsoftentityframeworkcoreautohistory"></a><span data-ttu-id="3d6d2-149">Microsoft.EntityFrameworkCore.AutoHistory</span><span class="sxs-lookup"><span data-stu-id="3d6d2-149">Microsoft.EntityFrameworkCore.AutoHistory</span></span>

<span data-ttu-id="3d6d2-150">Knihovna modulů plug-in, která umožňuje automatické zaznamenávání změn dat provedených EF Core do tabulky historie.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-150">A plugin library that enables automatically recording the data changes performed by EF Core into a history table.</span></span> <span data-ttu-id="3d6d2-151">Pro EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-151">For EF Core: 2.</span></span>

[<span data-ttu-id="3d6d2-152">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="3d6d2-152">GitHub repository</span></span>](https://github.com/Arch/AutoHistory/)

### <a name="efsecondlevelcachecore"></a><span data-ttu-id="3d6d2-153">EFSecondLevelCache.Core</span><span class="sxs-lookup"><span data-stu-id="3d6d2-153">EFSecondLevelCache.Core</span></span>

<span data-ttu-id="3d6d2-154">Rozšíření, které umožňuje ukládat výsledky EF Core dotazů do mezipaměti druhé úrovně, aby následné spouštění stejných dotazů se mohli vyhnout přístupu k databázi a načíst data přímo z mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-154">An extension that enables storing the results of EF Core queries into a second-level cache, so that subsequent executions of the same queries can avoid accessing the database and retrieve the data directly from the cache.</span></span> <span data-ttu-id="3d6d2-155">Pro EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-155">For EF Core: 2.</span></span>

[<span data-ttu-id="3d6d2-156">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="3d6d2-156">GitHub repository</span></span>](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="geco"></a><span data-ttu-id="3d6d2-157">Geco</span><span class="sxs-lookup"><span data-stu-id="3d6d2-157">Geco</span></span>

<span data-ttu-id="3d6d2-158">GECO (konzola generátoru) je jednoduchý generátor kódu založený na projektu konzoly, který běží na .NET Core a používá C# interpolované řetězce pro generování kódu.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-158">Geco (Generator Console) is a simple code generator based on a console project, that runs on .NET Core and uses C# interpolated strings for code generation.</span></span> <span data-ttu-id="3d6d2-159">GECO zahrnuje generátor reverzních modelů pro EF Core s podporou pro pluralitování, jednotné navýšení a upravitelnou šablonu.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-159">Geco includes a reverse model generator for EF Core with support for pluralization, singularization, and editable templates.</span></span> <span data-ttu-id="3d6d2-160">Poskytuje také generátor počátečních dat, spouštěč skriptů a čistič databáze.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-160">It also provides a seed data script generator, a script runner, and a database cleaner.</span></span> <span data-ttu-id="3d6d2-161">Pro EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-161">For EF Core: 2.</span></span>

[<span data-ttu-id="3d6d2-162">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="3d6d2-162">GitHub repository</span></span>](https://github.com/iQuarc/Geco)

### <a name="entityframeworkcorescaffoldinghandlebars"></a><span data-ttu-id="3d6d2-163">EntityFrameworkCore. Lešeníing. handlebars</span><span class="sxs-lookup"><span data-stu-id="3d6d2-163">EntityFrameworkCore.Scaffolding.Handlebars</span></span> 

<span data-ttu-id="3d6d2-164">Umožňuje přizpůsobit třídy zpětnou analýzou z existující databáze pomocí Entity Framework Core sada nástrojů s handlebars šablonami.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-164">Allows customization of classes reverse engineered from an existing database using the Entity Framework Core toolchain with Handlebars templates.</span></span> <span data-ttu-id="3d6d2-165">Pro EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-165">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="3d6d2-166">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="3d6d2-166">GitHub repository</span></span>](https://github.com/TrackableEntities/EntityFrameworkCore.Scaffolding.Handlebars)

### <a name="neinlinqentityframeworkcore"></a><span data-ttu-id="3d6d2-167">NeinLinq.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="3d6d2-167">NeinLinq.EntityFrameworkCore</span></span> 

<span data-ttu-id="3d6d2-168">NeinLinq rozšiřuje zprostředkovatele LINQ, jako je například Entity Framework, aby umožnila opakované použití funkcí, přepisování dotazů a sestavování dynamických dotazů pomocí přeložitelných predikátů a selektorů.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-168">NeinLinq extends LINQ providers such as Entity Framework to enable reusing functions, rewriting queries, and building dynamic queries using translatable predicates and selectors.</span></span> <span data-ttu-id="3d6d2-169">Pro EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-169">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="3d6d2-170">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="3d6d2-170">GitHub repository</span></span>](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a><span data-ttu-id="3d6d2-171">Microsoft.EntityFrameworkCore.UnitOfWork</span><span class="sxs-lookup"><span data-stu-id="3d6d2-171">Microsoft.EntityFrameworkCore.UnitOfWork</span></span>

<span data-ttu-id="3d6d2-172">Modul plug-in pro Microsoft. EntityFrameworkCore pro podporu úložiště, pracovních schémat a více databází s podporou distribuované transakce.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-172">A plugin for Microsoft.EntityFrameworkCore to support repository, unit of work patterns, and multiple databases with distributed transaction supported.</span></span> <span data-ttu-id="3d6d2-173">Pro EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-173">For EF Core: 2.</span></span>

[<span data-ttu-id="3d6d2-174">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="3d6d2-174">GitHub repository</span></span>](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a><span data-ttu-id="3d6d2-175">EFCore.BulkExtensions</span><span class="sxs-lookup"><span data-stu-id="3d6d2-175">EFCore.BulkExtensions</span></span>

<span data-ttu-id="3d6d2-176">Rozšíření EF Core pro hromadné operace (vložení, aktualizace, odstranění).</span><span class="sxs-lookup"><span data-stu-id="3d6d2-176">EF Core extensions for Bulk operations (Insert, Update, Delete).</span></span> <span data-ttu-id="3d6d2-177">Pro EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-177">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="3d6d2-178">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="3d6d2-178">GitHub repository</span></span>](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a><span data-ttu-id="3d6d2-179">Bricelam.EntityFrameworkCore.Pluralizer</span><span class="sxs-lookup"><span data-stu-id="3d6d2-179">Bricelam.EntityFrameworkCore.Pluralizer</span></span>

<span data-ttu-id="3d6d2-180">Přidá pluralitu v době návrhu.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-180">Adds design-time pluralization.</span></span> <span data-ttu-id="3d6d2-181">Pro EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-181">For EF Core: 2.</span></span>

[<span data-ttu-id="3d6d2-182">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="3d6d2-182">GitHub repository</span></span>](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="toolbeltentityframeworkcoreindexattribute"></a><span data-ttu-id="3d6d2-183">Toolbelt.EntityFrameworkCore.IndexAttribute</span><span class="sxs-lookup"><span data-stu-id="3d6d2-183">Toolbelt.EntityFrameworkCore.IndexAttribute</span></span>

<span data-ttu-id="3d6d2-184">Revival atributu [index] (s příponou pro sestavení modelu).</span><span class="sxs-lookup"><span data-stu-id="3d6d2-184">Revival of [Index] attribute (with extension for model building).</span></span> <span data-ttu-id="3d6d2-185">Pro EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-185">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="3d6d2-186">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="3d6d2-186">GitHub repository</span></span>](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a><span data-ttu-id="3d6d2-187">EfCore.InMemoryHelpers</span><span class="sxs-lookup"><span data-stu-id="3d6d2-187">EfCore.InMemoryHelpers</span></span>

<span data-ttu-id="3d6d2-188">Poskytuje obálku kolem EF Coreho poskytovatele databáze v paměti.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-188">Provides a wrapper around the EF Core In-Memory Database Provider.</span></span> <span data-ttu-id="3d6d2-189">Díky tomu bude fungovat podobně jako u relačního poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-189">Makes it act more like a relational provider.</span></span> <span data-ttu-id="3d6d2-190">Pro EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-190">For EF Core: 2.</span></span>

[<span data-ttu-id="3d6d2-191">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="3d6d2-191">GitHub repository</span></span>](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a><span data-ttu-id="3d6d2-192">EFCore.TemporalSupport</span><span class="sxs-lookup"><span data-stu-id="3d6d2-192">EFCore.TemporalSupport</span></span>

<span data-ttu-id="3d6d2-193">Implementace dočasné podpory.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-193">An implementation of temporal support.</span></span> <span data-ttu-id="3d6d2-194">Pro EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-194">For EF Core: 2.</span></span>

[<span data-ttu-id="3d6d2-195">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="3d6d2-195">GitHub repository</span></span>](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="efcoretemporaltable"></a><span data-ttu-id="3d6d2-196">EfCoreTemporalTable</span><span class="sxs-lookup"><span data-stu-id="3d6d2-196">EfCoreTemporalTable</span></span>

<span data-ttu-id="3d6d2-197">Pomocí představených metod rozšíření můžete snadno provádět dočasné dotazy na oblíbené databázi: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)``AsTemporalContained(startDate, endDate)`.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-197">Easily perform temporal queries on your favourite database using introduced extension methods: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)`, `AsTemporalContained(startDate, endDate)`.</span></span> <span data-ttu-id="3d6d2-198">Pro EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-198">For EF Core: 3.</span></span>

[<span data-ttu-id="3d6d2-199">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="3d6d2-199">GitHub repository</span></span>](https://github.com/glautrou/EfCoreTemporalTable)

### <a name="efcoretimetraveler"></a><span data-ttu-id="3d6d2-200">EFCore.TimeTraveler</span><span class="sxs-lookup"><span data-stu-id="3d6d2-200">EFCore.TimeTraveler</span></span>

<span data-ttu-id="3d6d2-201">Povolí plně funkční Entity Framework Core dotazy pro SQL Server dočasnou [historii](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) pomocí kódu EF Core, entit a mapování, které jste už definovali.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-201">Allow full-featured Entity Framework Core queries against [SQL Server Temporal History](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) using the EF Core code, entities, and mappings you already have defined.</span></span>  <span data-ttu-id="3d6d2-202">Procházením kódu v `using (TemporalQuery.AsOf(targetDateTime)) {...}`se procházejte časem.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-202">Travel through time by wrapping your code in `using (TemporalQuery.AsOf(targetDateTime)) {...}`.</span></span> <span data-ttu-id="3d6d2-203">Pro EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-203">For EF Core: 3.</span></span>

[<span data-ttu-id="3d6d2-204">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="3d6d2-204">GitHub repository</span></span>](https://github.com/VantageSoftware/EFCore.TimeTraveler)


### <a name="entityframeworkcoretemporaltables"></a><span data-ttu-id="3d6d2-205">EntityFrameworkCore.TemporalTables</span><span class="sxs-lookup"><span data-stu-id="3d6d2-205">EntityFrameworkCore.TemporalTables</span></span>

<span data-ttu-id="3d6d2-206">Knihovna rozšíření pro Entity Framework Core, která umožňuje vývojářům, kteří používají SQL Server, snadno používat dočasné tabulky.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-206">Extension library for Entity Framework Core which allows developers who use SQL Server to easily use temporal tables.</span></span> <span data-ttu-id="3d6d2-207">Pro EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-207">For EF Core: 2.</span></span>

[<span data-ttu-id="3d6d2-208">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="3d6d2-208">GitHub repository</span></span>](https://github.com/findulov/EntityFrameworkCore.TemporalTables)


### <a name="entityframeworkcorecacheable"></a><span data-ttu-id="3d6d2-209">EntityFrameworkCore.Cacheable</span><span class="sxs-lookup"><span data-stu-id="3d6d2-209">EntityFrameworkCore.Cacheable</span></span>

<span data-ttu-id="3d6d2-210">Vysoce výkonná mezipaměť dotazů druhé úrovně.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-210">A high-performance second-level query cache.</span></span> <span data-ttu-id="3d6d2-211">Pro EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-211">For EF Core: 2.</span></span>

[<span data-ttu-id="3d6d2-212">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="3d6d2-212">GitHub repository</span></span>](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entity-framework-plus"></a><span data-ttu-id="3d6d2-213">Entity Framework plus</span><span class="sxs-lookup"><span data-stu-id="3d6d2-213">Entity Framework Plus</span></span>

<span data-ttu-id="3d6d2-214">Rozšiřuje vaše DbContext o funkce, jako například: zahrnutí filtru, auditování, ukládání do mezipaměti, budoucí dotaz, dávkové odstranění, dávkové aktualizace a další.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-214">Extends your DbContext with features such as: Include Filter, Auditing, Caching, Query Future, Batch Delete, Batch Update, and more.</span></span> <span data-ttu-id="3d6d2-215">Pro EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-215">For EF Core: 2, 3.</span></span>

<span data-ttu-id="3d6d2-216">[Web](https://entityframework-plus.net/)
[úložiště GitHub](https://github.com/zzzprojects/EntityFramework-Plus)</span><span class="sxs-lookup"><span data-stu-id="3d6d2-216">[Website](https://entityframework-plus.net/)
[GitHub repository](https://github.com/zzzprojects/EntityFramework-Plus)</span></span>

### <a name="entity-framework-extensions"></a><span data-ttu-id="3d6d2-217">Rozšíření Entity Framework</span><span class="sxs-lookup"><span data-stu-id="3d6d2-217">Entity Framework Extensions</span></span>

<span data-ttu-id="3d6d2-218">Rozšiřuje vaše DbContext o vysoce výkonné hromadné operace: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge a další.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-218">Extends your DbContext with high-performance bulk operations: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge, and more.</span></span> <span data-ttu-id="3d6d2-219">Pro EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-219">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="3d6d2-220">Web</span><span class="sxs-lookup"><span data-stu-id="3d6d2-220">Website</span></span>](https://entityframework-extensions.net/)

### <a name="expressionify"></a><span data-ttu-id="3d6d2-221">Expressionify</span><span class="sxs-lookup"><span data-stu-id="3d6d2-221">Expressionify</span></span>

<span data-ttu-id="3d6d2-222">Přidání podpory pro volání metod rozšíření ve výrazech LINQ lambda.</span><span class="sxs-lookup"><span data-stu-id="3d6d2-222">Add support for calling extension methods in linq lambdas.</span></span> <span data-ttu-id="3d6d2-223">Pro EF Core: 3,1</span><span class="sxs-lookup"><span data-stu-id="3d6d2-223">For EF Core: 3.1</span></span>

[<span data-ttu-id="3d6d2-224">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="3d6d2-224">GitHub repository</span></span>](https://github.com/ClaveConsulting/Expressionify)
