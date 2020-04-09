---
title: Nástroje & rozšíření - EF Core
author: ErikEJ
ms.date: 12/17/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: e3806f7161fecfe66450d3e08f97caf3d2c84cf3
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "80634244"
---
# <a name="ef-core-tools--extensions"></a><span data-ttu-id="74530-102">Základní nástroje EF & rozšíření</span><span class="sxs-lookup"><span data-stu-id="74530-102">EF Core Tools & Extensions</span></span>

<span data-ttu-id="74530-103">Tyto nástroje a rozšíření poskytují další funkce pro entity framework core 2.1 a novější.</span><span class="sxs-lookup"><span data-stu-id="74530-103">These tools and extensions provide additional functionality for Entity Framework Core 2.1 and later.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="74530-104">Rozšíření jsou sestaveny z různých zdrojů a nejsou udržovány jako součást projektu Core entity framework.</span><span class="sxs-lookup"><span data-stu-id="74530-104">Extensions are built by a variety of sources and aren't maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="74530-105">Při zvažování rozšíření třetí strany nezapomeňte vyhodnotit jeho kvalitu, licencování, kompatibilitu, podporu atd., abyste se ujistili, že splňuje vaše požadavky.</span><span class="sxs-lookup"><span data-stu-id="74530-105">When considering a third party extension, be sure to evaluate its quality, licensing, compatibility, support, etc. to ensure it meets your requirements.</span></span> <span data-ttu-id="74530-106">Zejména rozšíření vytvořené pro starší verzi EF Core může potřebovat aktualizaci, než bude fungovat s nejnovějšími verzemi.</span><span class="sxs-lookup"><span data-stu-id="74530-106">In particular, an extension built for an older version of EF Core may need updating before it will work with the latest versions.</span></span>

## <a name="tools"></a><span data-ttu-id="74530-107">nástroje</span><span class="sxs-lookup"><span data-stu-id="74530-107">Tools</span></span>

### <a name="llblgen-pro"></a><span data-ttu-id="74530-108">LLBLGen Pro</span><span class="sxs-lookup"><span data-stu-id="74530-108">LLBLGen Pro</span></span>

<span data-ttu-id="74530-109">LLBLGen Pro je řešení modelování entit s podporou entity frameworku a entity framework core.</span><span class="sxs-lookup"><span data-stu-id="74530-109">LLBLGen Pro is an entity modeling solution with support for Entity Framework and Entity Framework Core.</span></span> <span data-ttu-id="74530-110">Umožňuje snadno definovat model entity a mapovat jej do databáze, nejprve pomocí databáze nebo modelu, takže můžete začít psát dotazy hned.</span><span class="sxs-lookup"><span data-stu-id="74530-110">It lets you easily define your entity model and map it to your database, using database first or model first, so you can get started writing queries right away.</span></span> <span data-ttu-id="74530-111">Pro EF jádro: 2.</span><span class="sxs-lookup"><span data-stu-id="74530-111">For EF Core: 2.</span></span>

[<span data-ttu-id="74530-112">Web</span><span class="sxs-lookup"><span data-stu-id="74530-112">Website</span></span>](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a><span data-ttu-id="74530-113">Devart Entity Developer</span><span class="sxs-lookup"><span data-stu-id="74530-113">Devart Entity Developer</span></span>

<span data-ttu-id="74530-114">Developer entity je výkonný návrhář ORM pro ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access a LINQ to SQL.</span><span class="sxs-lookup"><span data-stu-id="74530-114">Entity Developer is a powerful ORM designer for ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access, and LINQ to SQL.</span></span> <span data-ttu-id="74530-115">Podporuje navrhování ef core modely vizuálně, pomocí modelu první nebo první přístupy databáze a Generování kódu Jazyka C# nebo Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="74530-115">It supports designing EF Core models visually, using model first or database first approaches, and C# or Visual Basic code generation.</span></span> <span data-ttu-id="74530-116">Pro EF jádro: 2.</span><span class="sxs-lookup"><span data-stu-id="74530-116">For EF Core: 2.</span></span>

[<span data-ttu-id="74530-117">Web</span><span class="sxs-lookup"><span data-stu-id="74530-117">Website</span></span>](https://www.devart.com/entitydeveloper/)

### <a name="nhydrate-orm-for-entity-framework"></a><span data-ttu-id="74530-118">nHydratační orm pro entity framework</span><span class="sxs-lookup"><span data-stu-id="74530-118">nHydrate ORM for Entity Framework</span></span>

<span data-ttu-id="74530-119">ORM, který vytváří silného typu, rozšiřitelné třídy pro entity framework.</span><span class="sxs-lookup"><span data-stu-id="74530-119">An ORM that creates strongly-typed, extendable classes for Entity Framework.</span></span> <span data-ttu-id="74530-120">Generovaný kód je jádro entity frameworku.</span><span class="sxs-lookup"><span data-stu-id="74530-120">The generated code is Entity Framework Core.</span></span> <span data-ttu-id="74530-121">Není v tom žádný rozdíl.</span><span class="sxs-lookup"><span data-stu-id="74530-121">There is no difference.</span></span> <span data-ttu-id="74530-122">Toto není náhrada za EF nebo vlastní ORM.</span><span class="sxs-lookup"><span data-stu-id="74530-122">This is not a replacement for EF or a custom ORM.</span></span> <span data-ttu-id="74530-123">Jedná se o vizuální vrstvu modelování, která umožňuje týmu spravovat komplexní schémata databáze.</span><span class="sxs-lookup"><span data-stu-id="74530-123">It is a visual, modeling layer that allows a team to manage complex database schemas.</span></span> <span data-ttu-id="74530-124">Funguje dobře se softwarem SCM, jako je Git, který umožňuje přístup k vašemu modelu pro více uživatelů s minimálními konflikty.</span><span class="sxs-lookup"><span data-stu-id="74530-124">It works well with SCM software like Git, allowing multi-user access to your model with minimal conflicts.</span></span> <span data-ttu-id="74530-125">Instalační program sleduje změny modelu a vytváří skripty upgradu.</span><span class="sxs-lookup"><span data-stu-id="74530-125">The installer tracks model changes and creates upgrade scripts.</span></span> <span data-ttu-id="74530-126">Pro EF core: 3.</span><span class="sxs-lookup"><span data-stu-id="74530-126">For EF Core: 3.</span></span>

[<span data-ttu-id="74530-127">Web Githubu</span><span class="sxs-lookup"><span data-stu-id="74530-127">Github site</span></span>](https://github.com/nHydrate/nHydrate)

### <a name="ef-core-power-tools"></a><span data-ttu-id="74530-128">Základní elektrické nástroje EF</span><span class="sxs-lookup"><span data-stu-id="74530-128">EF Core Power Tools</span></span>

<span data-ttu-id="74530-129">EF Core Power Tools je rozšíření sady Visual Studio, které zpřístupňuje různé úlohy návrhu EF Core v jednoduchém uživatelském rozhraní.</span><span class="sxs-lookup"><span data-stu-id="74530-129">EF Core Power Tools is a Visual Studio extension that exposes various EF Core design-time tasks in a simple user interface.</span></span> <span data-ttu-id="74530-130">Zahrnuje reverzní inženýrství DbContext a entity třídy z existujících databází a [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), správa migrace databáze a vizualizace modelu.</span><span class="sxs-lookup"><span data-stu-id="74530-130">It includes reverse engineering of DbContext and entity classes from existing databases and [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), management of database migrations, and model visualizations.</span></span> <span data-ttu-id="74530-131">Pro EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="74530-131">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="74530-132">GitHub wiki</span><span class="sxs-lookup"><span data-stu-id="74530-132">GitHub wiki</span></span>](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a><span data-ttu-id="74530-133">Vizuální editor Entity Framework</span><span class="sxs-lookup"><span data-stu-id="74530-133">Entity Framework Visual Editor</span></span>

<span data-ttu-id="74530-134">Entity Framework Visual Editor je rozšíření Visual Studio, které přidává návrháře ORM pro vizuální návrh EF 6 a EF Core tříd.</span><span class="sxs-lookup"><span data-stu-id="74530-134">Entity Framework Visual Editor is a Visual Studio extension that adds an ORM designer for visual design of EF 6, and EF Core classes.</span></span> <span data-ttu-id="74530-135">Kód je generován pomocí šablon T4, takže jej lze přizpůsobit tak, aby vyhovoval všem potřebám.</span><span class="sxs-lookup"><span data-stu-id="74530-135">Code is generated using T4 templates so can be customized to suit any needs.</span></span> <span data-ttu-id="74530-136">Podporuje dědičnost, jednosměrné a obousměrné přidružení, výčty a schopnost barevně kódovat vaše třídy a přidávat textové bloky, které vysvětlují potenciálně tajemné části návrhu.</span><span class="sxs-lookup"><span data-stu-id="74530-136">It supports inheritance, unidirectional and bidirectional associations, enumerations, and the ability to color-code your classes and add text blocks to explain potentially arcane parts of your design.</span></span> <span data-ttu-id="74530-137">Pro EF jádro: 2.</span><span class="sxs-lookup"><span data-stu-id="74530-137">For EF Core: 2.</span></span>

[<span data-ttu-id="74530-138">Marketplace</span><span class="sxs-lookup"><span data-stu-id="74530-138">Marketplace</span></span>](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a><span data-ttu-id="74530-139">CatFactory</span><span class="sxs-lookup"><span data-stu-id="74530-139">CatFactory</span></span>

<span data-ttu-id="74530-140">CatFactory je modul generování uživatelského rozhraní pro .NET Core, který může automatizovat generování tříd DbContext, entit, konfigurací mapování a tříd úložiště z databáze serveru SQL Server.</span><span class="sxs-lookup"><span data-stu-id="74530-140">CatFactory is a scaffolding engine for .NET Core that can automate the generation of DbContext classes, entities, mapping configurations, and repository classes from a SQL Server database.</span></span> <span data-ttu-id="74530-141">Pro EF jádro: 2.</span><span class="sxs-lookup"><span data-stu-id="74530-141">For EF Core: 2.</span></span>

[<span data-ttu-id="74530-142">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="74530-142">GitHub repository</span></span>](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a><span data-ttu-id="74530-143">LoreSoft entity framework core generátor</span><span class="sxs-lookup"><span data-stu-id="74530-143">LoreSoft's Entity Framework Core Generator</span></span>

<span data-ttu-id="74530-144">Entity Framework Core Generator (efg) je nástroj .NET Core CLI, který může `dotnet ef dbcontext scaffold`generovat ef core modely z existující databáze, podobně jako , ale také podporuje bezpečné [regeneraci](https://efg.loresoft.com/en/latest/regeneration/) kódu prostřednictvím nahrazení oblasti nebo analýzou mapovacích souborů.</span><span class="sxs-lookup"><span data-stu-id="74530-144">Entity Framework Core Generator (efg) is a .NET Core CLI tool that can generate EF Core models from an existing database, much like `dotnet ef dbcontext scaffold`, but it also supports safe code [regeneration](https://efg.loresoft.com/en/latest/regeneration/) via region replacement or by parsing mapping files.</span></span> <span data-ttu-id="74530-145">Tento nástroj podporuje generování modelů zobrazení, ověřování a kódu mapovače objektů.</span><span class="sxs-lookup"><span data-stu-id="74530-145">This tool supports generating view models, validation, and object mapper code.</span></span> <span data-ttu-id="74530-146">Pro EF jádro: 2.</span><span class="sxs-lookup"><span data-stu-id="74530-146">For EF Core: 2.</span></span>

<span data-ttu-id="74530-147">[Dokumentace kurzu](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Documentation](https://efg.loresoft.com/en/latest/)</span><span class="sxs-lookup"><span data-stu-id="74530-147">[Tutorial](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Documentation](https://efg.loresoft.com/en/latest/)</span></span>

## <a name="extensions"></a><span data-ttu-id="74530-148">Rozšíření</span><span class="sxs-lookup"><span data-stu-id="74530-148">Extensions</span></span>

### <a name="microsoftentityframeworkcoreautohistory"></a><span data-ttu-id="74530-149">Microsoft.EntityFrameworkCore.AutoHistory</span><span class="sxs-lookup"><span data-stu-id="74530-149">Microsoft.EntityFrameworkCore.AutoHistory</span></span>

<span data-ttu-id="74530-150">Knihovna pluginů, která umožňuje automatické zaznamenávání změn dat provedených EF Core do tabulky historie.</span><span class="sxs-lookup"><span data-stu-id="74530-150">A plugin library that enables automatically recording the data changes performed by EF Core into a history table.</span></span> <span data-ttu-id="74530-151">Pro EF jádro: 2.</span><span class="sxs-lookup"><span data-stu-id="74530-151">For EF Core: 2.</span></span>

[<span data-ttu-id="74530-152">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="74530-152">GitHub repository</span></span>](https://github.com/Arch/AutoHistory/)

### <a name="efsecondlevelcachecore"></a><span data-ttu-id="74530-153">EFSecondLevelCache.Core</span><span class="sxs-lookup"><span data-stu-id="74530-153">EFSecondLevelCache.Core</span></span>

<span data-ttu-id="74530-154">Rozšíření, které umožňuje ukládání výsledků dotazů EF Core do mezipaměti druhé úrovně, takže následné spuštění stejných dotazů se může vyhnout přístupu k databázi a načíst data přímo z mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="74530-154">An extension that enables storing the results of EF Core queries into a second-level cache, so that subsequent executions of the same queries can avoid accessing the database and retrieve the data directly from the cache.</span></span> <span data-ttu-id="74530-155">Pro EF jádro: 2.</span><span class="sxs-lookup"><span data-stu-id="74530-155">For EF Core: 2.</span></span>

[<span data-ttu-id="74530-156">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="74530-156">GitHub repository</span></span>](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="geco"></a><span data-ttu-id="74530-157">Geco (Geco)</span><span class="sxs-lookup"><span data-stu-id="74530-157">Geco</span></span>

<span data-ttu-id="74530-158">Geco (Generator Console) je jednoduchý generátor kódu založený na projektu konzoly, který běží na .NET Core a používá interpolované řetězce C# pro generování kódu.</span><span class="sxs-lookup"><span data-stu-id="74530-158">Geco (Generator Console) is a simple code generator based on a console project, that runs on .NET Core and uses C# interpolated strings for code generation.</span></span> <span data-ttu-id="74530-159">Geco obsahuje generátor reverzního modelu pro EF Core s podporou pluralizace, singularizace a upravitelných šablon.</span><span class="sxs-lookup"><span data-stu-id="74530-159">Geco includes a reverse model generator for EF Core with support for pluralization, singularization, and editable templates.</span></span> <span data-ttu-id="74530-160">Poskytuje také generátor skriptů seed, skript ový běh a čistič databáze.</span><span class="sxs-lookup"><span data-stu-id="74530-160">It also provides a seed data script generator, a script runner, and a database cleaner.</span></span> <span data-ttu-id="74530-161">Pro EF jádro: 2.</span><span class="sxs-lookup"><span data-stu-id="74530-161">For EF Core: 2.</span></span>

[<span data-ttu-id="74530-162">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="74530-162">GitHub repository</span></span>](https://github.com/iQuarc/Geco)

### <a name="entityframeworkcorescaffoldinghandlebars"></a><span data-ttu-id="74530-163">EntityFrameworkCore.Scaffolding.Řídítka</span><span class="sxs-lookup"><span data-stu-id="74530-163">EntityFrameworkCore.Scaffolding.Handlebars</span></span> 

<span data-ttu-id="74530-164">Umožňuje přizpůsobení tříd zpětně analyzovány z existující databáze pomocí entity framework core toolchain se šablonami řídítek.</span><span class="sxs-lookup"><span data-stu-id="74530-164">Allows customization of classes reverse engineered from an existing database using the Entity Framework Core toolchain with Handlebars templates.</span></span> <span data-ttu-id="74530-165">Pro EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="74530-165">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="74530-166">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="74530-166">GitHub repository</span></span>](https://github.com/TrackableEntities/EntityFrameworkCore.Scaffolding.Handlebars)

### <a name="neinlinqentityframeworkcore"></a><span data-ttu-id="74530-167">NeinLinq.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="74530-167">NeinLinq.EntityFrameworkCore</span></span> 

<span data-ttu-id="74530-168">NeinLinq rozšiřuje linq zprostředkovatele, jako je entity Framework povolit opakované použití funkcí, přepisování dotazů a vytváření dynamických dotazů pomocí přeložitelné predikáty a voliči.</span><span class="sxs-lookup"><span data-stu-id="74530-168">NeinLinq extends LINQ providers such as Entity Framework to enable reusing functions, rewriting queries, and building dynamic queries using translatable predicates and selectors.</span></span> <span data-ttu-id="74530-169">Pro EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="74530-169">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="74530-170">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="74530-170">GitHub repository</span></span>](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a><span data-ttu-id="74530-171">Microsoft.EntityFrameworkCore.UnitOfWork</span><span class="sxs-lookup"><span data-stu-id="74530-171">Microsoft.EntityFrameworkCore.UnitOfWork</span></span>

<span data-ttu-id="74530-172">Plugin pro Microsoft.EntityFrameworkCore pro podporu úložiště, jednotky pracovních vzorů a více databází s podporou distribuovaných transakcí.</span><span class="sxs-lookup"><span data-stu-id="74530-172">A plugin for Microsoft.EntityFrameworkCore to support repository, unit of work patterns, and multiple databases with distributed transaction supported.</span></span> <span data-ttu-id="74530-173">Pro EF jádro: 2.</span><span class="sxs-lookup"><span data-stu-id="74530-173">For EF Core: 2.</span></span>

[<span data-ttu-id="74530-174">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="74530-174">GitHub repository</span></span>](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a><span data-ttu-id="74530-175">EFCore.BulkExtensions</span><span class="sxs-lookup"><span data-stu-id="74530-175">EFCore.BulkExtensions</span></span>

<span data-ttu-id="74530-176">Ef Core rozšíření pro hromadné operace (Vložit, Aktualizovat, Odstranit).</span><span class="sxs-lookup"><span data-stu-id="74530-176">EF Core extensions for Bulk operations (Insert, Update, Delete).</span></span> <span data-ttu-id="74530-177">Pro EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="74530-177">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="74530-178">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="74530-178">GitHub repository</span></span>](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a><span data-ttu-id="74530-179">Bricelam.EntityFrameworkCore.Pluralizer</span><span class="sxs-lookup"><span data-stu-id="74530-179">Bricelam.EntityFrameworkCore.Pluralizer</span></span>

<span data-ttu-id="74530-180">Přidá pluralizaci návrhu.</span><span class="sxs-lookup"><span data-stu-id="74530-180">Adds design-time pluralization.</span></span> <span data-ttu-id="74530-181">Pro EF jádro: 2.</span><span class="sxs-lookup"><span data-stu-id="74530-181">For EF Core: 2.</span></span>

[<span data-ttu-id="74530-182">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="74530-182">GitHub repository</span></span>](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="toolbeltentityframeworkcoreindexattribute"></a><span data-ttu-id="74530-183">Toolbelt.EntityFrameworkCore.IndexAttribute</span><span class="sxs-lookup"><span data-stu-id="74530-183">Toolbelt.EntityFrameworkCore.IndexAttribute</span></span>

<span data-ttu-id="74530-184">Obnovení atributu [Index] (s rozšířením pro vytváření modelů).</span><span class="sxs-lookup"><span data-stu-id="74530-184">Revival of [Index] attribute (with extension for model building).</span></span> <span data-ttu-id="74530-185">Pro EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="74530-185">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="74530-186">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="74530-186">GitHub repository</span></span>](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a><span data-ttu-id="74530-187">EfCore.InMemoryHelpers</span><span class="sxs-lookup"><span data-stu-id="74530-187">EfCore.InMemoryHelpers</span></span>

<span data-ttu-id="74530-188">Poskytuje obálku kolem zprostředkovatele databáze základního ef v paměti.</span><span class="sxs-lookup"><span data-stu-id="74530-188">Provides a wrapper around the EF Core In-Memory Database Provider.</span></span> <span data-ttu-id="74530-189">Díky tomu se chová spíše jako relační poskytovatel.</span><span class="sxs-lookup"><span data-stu-id="74530-189">Makes it act more like a relational provider.</span></span> <span data-ttu-id="74530-190">Pro EF jádro: 2.</span><span class="sxs-lookup"><span data-stu-id="74530-190">For EF Core: 2.</span></span>

[<span data-ttu-id="74530-191">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="74530-191">GitHub repository</span></span>](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a><span data-ttu-id="74530-192">EFCore.TemporalSupport</span><span class="sxs-lookup"><span data-stu-id="74530-192">EFCore.TemporalSupport</span></span>

<span data-ttu-id="74530-193">Implementace časové podpory.</span><span class="sxs-lookup"><span data-stu-id="74530-193">An implementation of temporal support.</span></span> <span data-ttu-id="74530-194">Pro EF jádro: 2.</span><span class="sxs-lookup"><span data-stu-id="74530-194">For EF Core: 2.</span></span>

[<span data-ttu-id="74530-195">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="74530-195">GitHub repository</span></span>](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="efcoretemporaltable"></a><span data-ttu-id="74530-196">EfCoreTemporalTabulka</span><span class="sxs-lookup"><span data-stu-id="74530-196">EfCoreTemporalTable</span></span>

<span data-ttu-id="74530-197">Pomocí zavedených rozšiřujících metod můžete snadno provádět `AsTemporalAll()`časové `AsTemporalAsOf(date)` `AsTemporalFrom(startDate, endDate)`dotazy `AsTemporalBetween(startDate, endDate)` `AsTemporalContained(startDate, endDate)`na svou oblíbenou databázi: , , , , .</span><span class="sxs-lookup"><span data-stu-id="74530-197">Easily perform temporal queries on your favourite database using introduced extension methods: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)`, `AsTemporalContained(startDate, endDate)`.</span></span> <span data-ttu-id="74530-198">Pro EF core: 3.</span><span class="sxs-lookup"><span data-stu-id="74530-198">For EF Core: 3.</span></span>

[<span data-ttu-id="74530-199">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="74530-199">GitHub repository</span></span>](https://github.com/glautrou/EfCoreTemporalTable)

### <a name="efcoretimetraveler"></a><span data-ttu-id="74530-200">EFCore.TimeTraveler</span><span class="sxs-lookup"><span data-stu-id="74530-200">EFCore.TimeTraveler</span></span>

<span data-ttu-id="74530-201">Povolit plně doporučené entity Framework Core dotazy proti [SQL Server časové historie](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) pomocí KÓDU EF core, entity a mapování, které jste již definovali.</span><span class="sxs-lookup"><span data-stu-id="74530-201">Allow full-featured Entity Framework Core queries against [SQL Server Temporal History](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) using the EF Core code, entities, and mappings you already have defined.</span></span>  <span data-ttu-id="74530-202">Cestujte časem zabalením kódu do aplikace `using (TemporalQuery.AsOf(targetDateTime)) {...}`.</span><span class="sxs-lookup"><span data-stu-id="74530-202">Travel through time by wrapping your code in `using (TemporalQuery.AsOf(targetDateTime)) {...}`.</span></span> <span data-ttu-id="74530-203">Pro EF core: 3.</span><span class="sxs-lookup"><span data-stu-id="74530-203">For EF Core: 3.</span></span>

[<span data-ttu-id="74530-204">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="74530-204">GitHub repository</span></span>](https://github.com/VantageSoftware/EFCore.TimeTraveler)


### <a name="entityframeworkcoretemporaltables"></a><span data-ttu-id="74530-205">EntityFrameworkCore.TemporalTables</span><span class="sxs-lookup"><span data-stu-id="74530-205">EntityFrameworkCore.TemporalTables</span></span>

<span data-ttu-id="74530-206">Knihovna rozšíření pro entity framework core, který umožňuje vývojářům, kteří používají SQL Server snadno používat časové tabulky.</span><span class="sxs-lookup"><span data-stu-id="74530-206">Extension library for Entity Framework Core which allows developers who use SQL Server to easily use temporal tables.</span></span> <span data-ttu-id="74530-207">Pro EF jádro: 2.</span><span class="sxs-lookup"><span data-stu-id="74530-207">For EF Core: 2.</span></span>

[<span data-ttu-id="74530-208">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="74530-208">GitHub repository</span></span>](https://github.com/findulov/EntityFrameworkCore.TemporalTables)


### <a name="entityframeworkcorecacheable"></a><span data-ttu-id="74530-209">EntityFrameworkCore.Cacheable</span><span class="sxs-lookup"><span data-stu-id="74530-209">EntityFrameworkCore.Cacheable</span></span>

<span data-ttu-id="74530-210">Vysoce výkonná mezipaměť dotazů druhé úrovně.</span><span class="sxs-lookup"><span data-stu-id="74530-210">A high-performance second-level query cache.</span></span> <span data-ttu-id="74530-211">Pro EF jádro: 2.</span><span class="sxs-lookup"><span data-stu-id="74530-211">For EF Core: 2.</span></span>

[<span data-ttu-id="74530-212">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="74530-212">GitHub repository</span></span>](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entity-framework-plus"></a><span data-ttu-id="74530-213">Entity Framework Plus</span><span class="sxs-lookup"><span data-stu-id="74530-213">Entity Framework Plus</span></span>

<span data-ttu-id="74530-214">Rozšiřuje dbcontext s funkcemi, jako jsou: Zahrnout filtr, auditování, ukládání do mezipaměti, Dotaz budoucí, Dávkové odstranění, Dávková aktualizace a další.</span><span class="sxs-lookup"><span data-stu-id="74530-214">Extends your DbContext with features such as: Include Filter, Auditing, Caching, Query Future, Batch Delete, Batch Update, and more.</span></span> <span data-ttu-id="74530-215">Pro EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="74530-215">For EF Core: 2, 3.</span></span>

<span data-ttu-id="74530-216">[Úložiště GitHub na webu](https://entityframework-plus.net/)
[GitHub repository](https://github.com/zzzprojects/EntityFramework-Plus)</span><span class="sxs-lookup"><span data-stu-id="74530-216">[Website](https://entityframework-plus.net/)
[GitHub repository](https://github.com/zzzprojects/EntityFramework-Plus)</span></span>

### <a name="entity-framework-extensions"></a><span data-ttu-id="74530-217">Rozšíření rámce entit</span><span class="sxs-lookup"><span data-stu-id="74530-217">Entity Framework Extensions</span></span>

<span data-ttu-id="74530-218">Rozšiřuje dbcontext s vysoce výkonné hromadné operace: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkDelete, BulkMerge a další.</span><span class="sxs-lookup"><span data-stu-id="74530-218">Extends your DbContext with high-performance bulk operations: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge, and more.</span></span> <span data-ttu-id="74530-219">Pro EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="74530-219">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="74530-220">Web</span><span class="sxs-lookup"><span data-stu-id="74530-220">Website</span></span>](https://entityframework-extensions.net/)

### <a name="expressionify"></a><span data-ttu-id="74530-221">Výrazy</span><span class="sxs-lookup"><span data-stu-id="74530-221">Expressionify</span></span>

<span data-ttu-id="74530-222">Přidejte podporu pro volání metod rozšíření v linq lambdas.</span><span class="sxs-lookup"><span data-stu-id="74530-222">Add support for calling extension methods in linq lambdas.</span></span> <span data-ttu-id="74530-223">Pro EF Core: 3.1</span><span class="sxs-lookup"><span data-stu-id="74530-223">For EF Core: 3.1</span></span>

[<span data-ttu-id="74530-224">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="74530-224">GitHub repository</span></span>](https://github.com/ClaveConsulting/Expressionify)

### <a name="xlinq"></a><span data-ttu-id="74530-225">XLinq</span><span class="sxs-lookup"><span data-stu-id="74530-225">XLinq</span></span>

<span data-ttu-id="74530-226">Technologie language integrated query (LINQ) pro relační databáze.</span><span class="sxs-lookup"><span data-stu-id="74530-226">Language Integrated Query (LINQ) technology for relational databases.</span></span> <span data-ttu-id="74530-227">Umožňuje používat c# k zápisu dotazů silného typu.</span><span class="sxs-lookup"><span data-stu-id="74530-227">It allows you to use C# to write strongly typed queries.</span></span> <span data-ttu-id="74530-228">Pro EF Core: 3.1</span><span class="sxs-lookup"><span data-stu-id="74530-228">For EF Core: 3.1</span></span>

- <span data-ttu-id="74530-229">Plná podpora Jazyka C# pro vytváření dotazů: více příkazů uvnitř lambda, proměnné, funkce atd.</span><span class="sxs-lookup"><span data-stu-id="74530-229">Full C# support for query creation: multiple statements inside lambda, variables, functions, etc.</span></span>
- <span data-ttu-id="74530-230">Žádná sémantická mezera s SQL.</span><span class="sxs-lookup"><span data-stu-id="74530-230">No semantic gap with SQL.</span></span> <span data-ttu-id="74530-231">XLinq deklaruje `SELECT` `FROM`příkazy SQL (jako , , `WHERE`) jako metody c# první třídy, kombinující známou syntaxi s intellisense, bezpečnost typů a refaktoring.</span><span class="sxs-lookup"><span data-stu-id="74530-231">XLinq declares SQL statements (like `SELECT`, `FROM`, `WHERE`) as first class C# methods, combining familiar syntax with intellisense, type safety and refactoring.</span></span>

<span data-ttu-id="74530-232">V důsledku toho SQL se stává jen "další" knihovna třídy vystavuje své rozhraní API místně, doslova *"Jazyk integrovaný SQL"*.</span><span class="sxs-lookup"><span data-stu-id="74530-232">As a result SQL becomes just "another" class library exposing its API locally, literally *"Language Integrated SQL"*.</span></span>

[<span data-ttu-id="74530-233">Web</span><span class="sxs-lookup"><span data-stu-id="74530-233">Website</span></span>](http://xlinq.live/)
