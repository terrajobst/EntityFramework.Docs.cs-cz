---
title: Nástroje a rozšíření – EF Core
author: ErikEJ
ms.date: 7/3/2018
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: e9f9a6cbbceeb0379ddb5588b564b0d2a962795f
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995510"
---
# <a name="ef-core-tools--extensions"></a><span data-ttu-id="6a75e-102">EF Core nástroje a rozšíření</span><span class="sxs-lookup"><span data-stu-id="6a75e-102">EF Core Tools & Extensions</span></span>

<span data-ttu-id="6a75e-103">Nástroje a rozšíření poskytují další funkce pro Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="6a75e-103">Tools and extensions provide additional functionality for Entity Framework Core.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="6a75e-104">Rozšíření jsou vytvořené pomocí široké škály zdrojů a není zachována jako součást projektu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="6a75e-104">Extensions are built by a variety of sources and not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="6a75e-105">Při zvažování rozšíření třetích stran, ujistěte se, k vyhodnocení kvality, licencování, kompatibility, podpora, atd. a ujistěte se, že splňují vaše požadavky.</span><span class="sxs-lookup"><span data-stu-id="6a75e-105">When considering a third party extension, be sure to evaluate quality, licensing, compatibility, support, etc. to ensure they meet your requirements.</span></span>

## <a name="tools"></a><span data-ttu-id="6a75e-106">Nástroje</span><span class="sxs-lookup"><span data-stu-id="6a75e-106">Tools</span></span>

### <a name="llblgen-pro"></a><span data-ttu-id="6a75e-107">LLBLGen Pro</span><span class="sxs-lookup"><span data-stu-id="6a75e-107">LLBLGen Pro</span></span>

<span data-ttu-id="6a75e-108">LLBLGen Pro je entita modelování řešení s podporou pro Entity Framework a Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="6a75e-108">LLBLGen Pro is an entity modeling solution with support for Entity Framework and Entity Framework Core.</span></span> <span data-ttu-id="6a75e-109">To vám umožní snadno definovat entity model a mapování na databázi, nejprve pomocí databáze nebo model nejprve, takže můžete začít používat hned psaní dotazů.</span><span class="sxs-lookup"><span data-stu-id="6a75e-109">It lets you easily define your entity model and map it to your database, using database first or model first, so you can get started writing queries right away.</span></span>

[<span data-ttu-id="6a75e-110">Web</span><span class="sxs-lookup"><span data-stu-id="6a75e-110">website</span></span>](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a><span data-ttu-id="6a75e-111">Devart Entity pro vývojáře</span><span class="sxs-lookup"><span data-stu-id="6a75e-111">Devart Entity Developer</span></span>

<span data-ttu-id="6a75e-112">Pro vývojáře entity je výkonný Návrhář ORM pro ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access a LINQ to SQL.</span><span class="sxs-lookup"><span data-stu-id="6a75e-112">Entity Developer is a powerful ORM designer for ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access, and LINQ to SQL.</span></span> <span data-ttu-id="6a75e-113">Můžete použít Model-First a Database-First přístupy k návrhu modelu ORM a generování kódu C# nebo Visual Basic .NET pro něj.</span><span class="sxs-lookup"><span data-stu-id="6a75e-113">You can use  Model-First and Database-First approaches to design your ORM model and generate C# or Visual Basic .NET code for it.</span></span> <span data-ttu-id="6a75e-114">Zavádí nové přístupy pro navrhování modelů ORM, zvyšuje produktivitu a usnadňuje vývoj aplikací databáze.</span><span class="sxs-lookup"><span data-stu-id="6a75e-114">It introduces new approaches for designing ORM models, boosts productivity, and facilitates the development of database applications.</span></span>

[<span data-ttu-id="6a75e-115">Web</span><span class="sxs-lookup"><span data-stu-id="6a75e-115">website</span></span>](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a><span data-ttu-id="6a75e-116">EF Core Power Tools</span><span class="sxs-lookup"><span data-stu-id="6a75e-116">EF Core Power Tools</span></span>

<span data-ttu-id="6a75e-117">Visual Studio 2017 + rozšíření.</span><span class="sxs-lookup"><span data-stu-id="6a75e-117">Visual Studio 2017+ extension.</span></span> <span data-ttu-id="6a75e-118">Můžete provést zpětnou analýzu třídy DbContext a POCO z existující databáze nebo databázový projekt SQL Server a vizualizovat a kontrolovat vaše DbContext různými způsoby.</span><span class="sxs-lookup"><span data-stu-id="6a75e-118">You can reverse engineer of DbContext and POCO classes from an existing database or SQL Server Database project, and visualize and inspect your DbContext in various ways.</span></span>

[<span data-ttu-id="6a75e-119">Wiki úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="6a75e-119">GitHub wiki</span></span>](https://github.com/ErikEJ/SqlCeToolbox/wiki/EF-Core-Power-Tools)

## <a name="extensions"></a><span data-ttu-id="6a75e-120">Rozšíření</span><span class="sxs-lookup"><span data-stu-id="6a75e-120">Extensions</span></span>

### <a name="microsoftentityframeworkcoreautohistory"></a><span data-ttu-id="6a75e-121">Microsoft.EntityFrameworkCore.AutoHistory</span><span class="sxs-lookup"><span data-stu-id="6a75e-121">Microsoft.EntityFrameworkCore.AutoHistory</span></span>

<span data-ttu-id="6a75e-122">Modul plug-in pro Microsoft.EntityFrameworkCore podporují nahrávání dat automaticky změní historie.</span><span class="sxs-lookup"><span data-stu-id="6a75e-122">A plugin for Microsoft.EntityFrameworkCore to support automatically recording data changes history.</span></span>

[<span data-ttu-id="6a75e-123">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="6a75e-123">GitHub repository</span></span>](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a><span data-ttu-id="6a75e-124">Microsoft.EntityFrameworkCore.DynamicLinq</span><span class="sxs-lookup"><span data-stu-id="6a75e-124">Microsoft.EntityFrameworkCore.DynamicLinq</span></span>

<span data-ttu-id="6a75e-125">Dynamické rozšíření Linq pro Microsoft.EntityFrameworkCore, který přidává podpory asynchronních operací</span><span class="sxs-lookup"><span data-stu-id="6a75e-125">Dynamic Linq extensions for Microsoft.EntityFrameworkCore which adds Async support</span></span>

 [<span data-ttu-id="6a75e-126">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="6a75e-126">GitHub repository</span></span>](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efcorepractices"></a><span data-ttu-id="6a75e-127">EFCore.Practices</span><span class="sxs-lookup"><span data-stu-id="6a75e-127">EFCore.Practices</span></span>

<span data-ttu-id="6a75e-128">Pokus o zaznamenat některé dobré nebo osvědčené postupy v rozhraní API, které podporuje testování – včetně malé architektura pro N + 1 dotazy vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="6a75e-128">Attempt to capture some good or best practices in an API that supports testing – including a small framework to scan for N+1 queries.</span></span>

[<span data-ttu-id="6a75e-129">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="6a75e-129">GitHub repository</span></span>](https://github.com/riezebosch/efcore-practices/tree/master/src/EFCore.Practices/)

### <a name="efsecondlevelcachecore"></a><span data-ttu-id="6a75e-130">EFSecondLevelCache.Core</span><span class="sxs-lookup"><span data-stu-id="6a75e-130">EFSecondLevelCache.Core</span></span>

<span data-ttu-id="6a75e-131">Druhé úrovně, ukládání do mezipaměti knihovny.</span><span class="sxs-lookup"><span data-stu-id="6a75e-131">Second Level Caching Library.</span></span> <span data-ttu-id="6a75e-132">Druhé úrovně mezipaměti se mezipaměť dotazů.</span><span class="sxs-lookup"><span data-stu-id="6a75e-132">Second level caching is a query cache.</span></span> <span data-ttu-id="6a75e-133">Výsledky příkazů EF se uloží do mezipaměti, takže stejné příkazy EF se data načítají z mezipaměti namísto jejich spuštění na databázi znovu.</span><span class="sxs-lookup"><span data-stu-id="6a75e-133">The results of EF commands will be stored in the cache, so that the same EF commands will retrieve their data from the cache rather than executing them against the database again.</span></span>

[<span data-ttu-id="6a75e-134">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="6a75e-134">GitHub repository</span></span>](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="detachedentityframework"></a><span data-ttu-id="6a75e-135">Detached.EntityFramework</span><span class="sxs-lookup"><span data-stu-id="6a75e-135">Detached.EntityFramework</span></span>

<span data-ttu-id="6a75e-136">Načte a uloží celý odpojené entity grafy (entita s jejich podřízené entity a seznamy).</span><span class="sxs-lookup"><span data-stu-id="6a75e-136">Loads and saves entire detached entity graphs (the entity with their child entities and lists).</span></span> <span data-ttu-id="6a75e-137">Inspirovat [GraphDiff](https://github.com/refactorthis/GraphDiff/).</span><span class="sxs-lookup"><span data-stu-id="6a75e-137">Inspired by [GraphDiff](https://github.com/refactorthis/GraphDiff/).</span></span> <span data-ttu-id="6a75e-138">Cílem je také přidat simplificate některé moduly plug-in některých opakujících se úloh, jako je auditování a stránkování.</span><span class="sxs-lookup"><span data-stu-id="6a75e-138">The idea is also add some plugins to simplificate some repetitive tasks, like auditing and pagination.</span></span>

[<span data-ttu-id="6a75e-139">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="6a75e-139">GitHub repository</span></span>](https://github.com/leonardoporro/Detached/)

### <a name="entityframeworkcoreprimarykey"></a><span data-ttu-id="6a75e-140">EntityFrameworkCore.PrimaryKey</span><span class="sxs-lookup"><span data-stu-id="6a75e-140">EntityFrameworkCore.PrimaryKey</span></span>

<span data-ttu-id="6a75e-141">Načtěte primární klíč (včetně složených klíčů) z jakékoli entity jako slovník.</span><span class="sxs-lookup"><span data-stu-id="6a75e-141">Retrieve the primary key (including composite keys) from any entity as a dictionary.</span></span>

[<span data-ttu-id="6a75e-142">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="6a75e-142">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcorerx"></a><span data-ttu-id="6a75e-143">EntityFrameworkCore.Rx</span><span class="sxs-lookup"><span data-stu-id="6a75e-143">EntityFrameworkCore.Rx</span></span>

<span data-ttu-id="6a75e-144">Reaktivní rozšíření obálky pro aktivní pozorovatelné objekty Entity Framework entit.</span><span class="sxs-lookup"><span data-stu-id="6a75e-144">Reactive extension wrappers for hot observables of Entity Framework entities.</span></span>

[<span data-ttu-id="6a75e-145">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="6a75e-145">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.Rx/)

### <a name="entityframeworkcoretriggers"></a><span data-ttu-id="6a75e-146">EntityFrameworkCore.Triggers</span><span class="sxs-lookup"><span data-stu-id="6a75e-146">EntityFrameworkCore.Triggers</span></span>

<span data-ttu-id="6a75e-147">Přidáte aktivační události pro entity s vložit, aktualizovat a odstraňovat události.</span><span class="sxs-lookup"><span data-stu-id="6a75e-147">Add triggers to your entities with insert, update, and delete events.</span></span> <span data-ttu-id="6a75e-148">Existují tři události pro každou: před, po a nebude úspěšná.</span><span class="sxs-lookup"><span data-stu-id="6a75e-148">There are three events for each: before, after, and upon failure.</span></span>

[<span data-ttu-id="6a75e-149">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="6a75e-149">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.Triggers/)

### <a name="entityframeworkcoretypedoriginalvalues"></a><span data-ttu-id="6a75e-150">EntityFrameworkCore.TypedOriginalValues</span><span class="sxs-lookup"><span data-stu-id="6a75e-150">EntityFrameworkCore.TypedOriginalValues</span></span>

<span data-ttu-id="6a75e-151">Zadaný přístup k původní hodnota vlastnosti vaší entity.</span><span class="sxs-lookup"><span data-stu-id="6a75e-151">Get typed access to the OriginalValue of your entity properties.</span></span> <span data-ttu-id="6a75e-152">Jednoduché a komplexní vlastnosti jsou podporovány, navigaci nebo kolekce nejsou.</span><span class="sxs-lookup"><span data-stu-id="6a75e-152">Simple and complex properties are supported, navigation/collections are not.</span></span>

[<span data-ttu-id="6a75e-153">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="6a75e-153">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a><span data-ttu-id="6a75e-154">Geco</span><span class="sxs-lookup"><span data-stu-id="6a75e-154">Geco</span></span>

<span data-ttu-id="6a75e-155">Geco poskytuje generátor Reverse modelu s podporou Pluralizace/Singularization a upravovat šablony založené na jazyce C# 6.0 interpolovaných řetězců a běží na.Net Core.</span><span class="sxs-lookup"><span data-stu-id="6a75e-155">Geco provides a Reverse Model generator with support for Pluralization/Singularization and editable templates based on C# 6.0 interpolated strings and running on .Net Core.</span></span> <span data-ttu-id="6a75e-156">Také poskytuje generátor skript počáteční hodnoty se skripty SQL sloučení a runner skriptu.</span><span class="sxs-lookup"><span data-stu-id="6a75e-156">It also provides an Seed script generator with SQL Merge scripts and an script runner.</span></span>

[<span data-ttu-id="6a75e-157">Úložiště Github</span><span class="sxs-lookup"><span data-stu-id="6a75e-157">Github repository</span></span>](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a><span data-ttu-id="6a75e-158">LinqKit.Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="6a75e-158">LinqKit.Microsoft.EntityFrameworkCore</span></span>

<span data-ttu-id="6a75e-159">LinqKit.Microsoft.EntityFrameworkCore je bezplatná sada rozšíření pro LINQ na SQL a EntityFrameworkCore zkušeným uživatelům.</span><span class="sxs-lookup"><span data-stu-id="6a75e-159">LinqKit.Microsoft.EntityFrameworkCore is a free set of extensions for LINQ to SQL and EntityFrameworkCore power users.</span></span> <span data-ttu-id="6a75e-160">S podporou Include(...) a IDbAsync.</span><span class="sxs-lookup"><span data-stu-id="6a75e-160">With Include(...) and IDbAsync support.</span></span>

[<span data-ttu-id="6a75e-161">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="6a75e-161">GitHub repository</span></span>](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a><span data-ttu-id="6a75e-162">NeinLinq.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="6a75e-162">NeinLinq.EntityFrameworkCore</span></span>

<span data-ttu-id="6a75e-163">NeinLinq.EntityFrameworkCore poskytuje užitečná rozšíření pro používání zprostředkovatelů LINQ jako je například rozhraní Entity Framework, které podporují pouze menší podmnožinu funkcí .NET opětovné použití funkce, přepisování dotazy, díky kterým i, hodnoty Null a sestavování dynamických dotazů pomocí Přeložitelné predikáty a selektory.</span><span class="sxs-lookup"><span data-stu-id="6a75e-163">NeinLinq.EntityFrameworkCore provides helpful extensions for using LINQ providers such as Entity Framework that support only a minor subset of .NET functions, reusing functions, rewriting queries, even making them null-safe, and building dynamic queries using translatable predicates and selectors.</span></span>

[<span data-ttu-id="6a75e-164">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="6a75e-164">GitHub repository</span></span>](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a><span data-ttu-id="6a75e-165">Microsoft.EntityFrameworkCore.UnitOfWork</span><span class="sxs-lookup"><span data-stu-id="6a75e-165">Microsoft.EntityFrameworkCore.UnitOfWork</span></span>

<span data-ttu-id="6a75e-166">Modul plug-in pro Microsoft.EntityFrameworkCore zajistit podporu pro úložiště a jednotky pracovních vzorů a více databáze s distribuovanou transakci nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="6a75e-166">A plugin for Microsoft.EntityFrameworkCore to support repository, unit of work patterns, and multiple database with distributed transaction supported.</span></span>

[<span data-ttu-id="6a75e-167">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="6a75e-167">GitHub repository</span></span>](https://github.com/Arch/UnitOfWork/)

### <a name="entityframeworklazyloading"></a><span data-ttu-id="6a75e-168">EntityFramework.LazyLoading</span><span class="sxs-lookup"><span data-stu-id="6a75e-168">EntityFramework.LazyLoading</span></span>

<span data-ttu-id="6a75e-169">Opožděné načtení pro jádro EF Core 1.1</span><span class="sxs-lookup"><span data-stu-id="6a75e-169">Lazy Loading for EF Core 1.1</span></span>

[<span data-ttu-id="6a75e-170">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="6a75e-170">GitHub repository</span></span>](https://github.com/darxis/EntityFramework.LazyLoading)

### <a name="efcorebulkextensions"></a><span data-ttu-id="6a75e-171">EFCore.BulkExtensions</span><span class="sxs-lookup"><span data-stu-id="6a75e-171">EFCore.BulkExtensions</span></span>

<span data-ttu-id="6a75e-172">EntityFrameworkCore rozšíření pro hromadné operace (Insert, Update, Delete).</span><span class="sxs-lookup"><span data-stu-id="6a75e-172">EntityFrameworkCore extensions for Bulk operations (Insert, Update, Delete).</span></span>

[<span data-ttu-id="6a75e-173">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="6a75e-173">GitHub repository</span></span>](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a><span data-ttu-id="6a75e-174">Bricelam.EntityFrameworkCore.Pluralizer</span><span class="sxs-lookup"><span data-stu-id="6a75e-174">Bricelam.EntityFrameworkCore.Pluralizer</span></span>

<span data-ttu-id="6a75e-175">Přidá pluralizace návrhu na EF Core.</span><span class="sxs-lookup"><span data-stu-id="6a75e-175">Adds design-time pluralization to EF Core.</span></span>

[<span data-ttu-id="6a75e-176">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="6a75e-176">GitHub repository</span></span>](https://github.com/bricelam/EFCore.Pluralizer)
