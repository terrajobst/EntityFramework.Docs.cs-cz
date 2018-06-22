---
title: Nástroje a rozšíření – EF jádra
author: ErikEJ
ms.author: divega
ms.date: 7/3/2018
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
ms.technology: entity-framework-core
uid: core/extensions/index
ms.openlocfilehash: 6c8cb3e0d8552f274118e4020b7e2e8009af7e11
ms.sourcegitcommit: fc68321c211aca38f7b9dc3a75677c6ca1b2524b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/08/2018
ms.locfileid: "29769436"
---
# <a name="ef-core-tools--extensions"></a><span data-ttu-id="87dd0-102">EF základní nástroje a rozšíření</span><span class="sxs-lookup"><span data-stu-id="87dd0-102">EF Core Tools & Extensions</span></span>

<span data-ttu-id="87dd0-103">Nástroje a rozšíření poskytují další funkce pro Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="87dd0-103">Tools and extensions provide additional functionality for Entity Framework Core.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="87dd0-104">Rozšíření jsou vytvořené různých zdrojů a není zachována jako součást projektu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="87dd0-104">Extensions are built by a variety of sources and not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="87dd0-105">Až budete zvažovat rozšíření třetích stran, ujistěte se, že jste vyhodnocení kvality, licencování, kompatibility, podpora, atd. Chcete-li zajistit, aby že odpovídaly vašim požadavkům.</span><span class="sxs-lookup"><span data-stu-id="87dd0-105">When considering a third party extension, be sure to evaluate quality, licensing, compatibility, support, etc. to ensure they meet your requirements.</span></span>

## <a name="tools"></a><span data-ttu-id="87dd0-106">Nástroje</span><span class="sxs-lookup"><span data-stu-id="87dd0-106">Tools</span></span>

### <a name="llblgen-pro"></a><span data-ttu-id="87dd0-107">LLBLGen Pro</span><span class="sxs-lookup"><span data-stu-id="87dd0-107">LLBLGen Pro</span></span>

<span data-ttu-id="87dd0-108">LLBLGen Pro je entita modelování řešení s podporou rozhraní Entity Framework a Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="87dd0-108">LLBLGen Pro is an entity modeling solution with support for Entity Framework and Entity Framework Core.</span></span> <span data-ttu-id="87dd0-109">Vám umožní snadno definovat modelu entity a mapy ke své databázi, nejprve pomocí databáze nebo model nejprve, takže můžete začít používat hned zápis dotazů.</span><span class="sxs-lookup"><span data-stu-id="87dd0-109">It lets you easily define your entity model and map it to your database, using database first or model first, so you can get started writing queries right away.</span></span>

[<span data-ttu-id="87dd0-110">website</span><span class="sxs-lookup"><span data-stu-id="87dd0-110">website</span></span>](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a><span data-ttu-id="87dd0-111">Devart Entity vývojáře</span><span class="sxs-lookup"><span data-stu-id="87dd0-111">Devart Entity Developer</span></span>

<span data-ttu-id="87dd0-112">Entity vývojáře je výkonný návrháře ORM ADO.NET Entity Framework, NHibernate, LinqConnect, přístup k datům webu Telerik a technologie LINQ to SQL.</span><span class="sxs-lookup"><span data-stu-id="87dd0-112">Entity Developer is a powerful ORM designer for ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access, and LINQ to SQL.</span></span> <span data-ttu-id="87dd0-113">Můžete použít první Model a první databáze blíží k návrhu modelu ORM a generování kódu jazyka C# nebo Visual Basic .NET pro ni.</span><span class="sxs-lookup"><span data-stu-id="87dd0-113">You can use  Model-First and Database-First approaches to design your ORM model and generate C# or Visual Basic .NET code for it.</span></span> <span data-ttu-id="87dd0-114">Zavádí nové přístupy k navrhování modely ORM, zvyšuje produktivitu a usnadňuje vývoj aplikací databáze.</span><span class="sxs-lookup"><span data-stu-id="87dd0-114">It introduces new approaches for designing ORM models, boosts productivity, and facilitates the development of database applications.</span></span>

[<span data-ttu-id="87dd0-115">website</span><span class="sxs-lookup"><span data-stu-id="87dd0-115">website</span></span>](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a><span data-ttu-id="87dd0-116">EF základní výkonné nástroje</span><span class="sxs-lookup"><span data-stu-id="87dd0-116">EF Core Power Tools</span></span>

<span data-ttu-id="87dd0-117">Visual Studio 2017 + rozšíření.</span><span class="sxs-lookup"><span data-stu-id="87dd0-117">Visual Studio 2017+ extension.</span></span> <span data-ttu-id="87dd0-118">Můžete zpětnou třídy DbContext a objektů POCO z existující databáze nebo databáze systému SQL Server projektu a vizualizovat a zkontrolovat vaše DbContext různými způsoby.</span><span class="sxs-lookup"><span data-stu-id="87dd0-118">You can reverse engineer of DbContext and POCO classes from an existing database or SQL Server Database project, and visualize and inspect your DbContext in various ways.</span></span>

[<span data-ttu-id="87dd0-119">Wiki Githubu</span><span class="sxs-lookup"><span data-stu-id="87dd0-119">GitHub wiki</span></span>](https://github.com/ErikEJ/SqlCeToolbox/wiki/EF-Core-Power-Tools)

## <a name="extensions"></a><span data-ttu-id="87dd0-120">Rozšíření</span><span class="sxs-lookup"><span data-stu-id="87dd0-120">Extensions</span></span>

### <a name="microsoftentityframeworkcoreautohistory"></a><span data-ttu-id="87dd0-121">Microsoft.EntityFrameworkCore.AutoHistory</span><span class="sxs-lookup"><span data-stu-id="87dd0-121">Microsoft.EntityFrameworkCore.AutoHistory</span></span>

<span data-ttu-id="87dd0-122">Modul plug-in pro Microsoft.EntityFrameworkCore pro podporu automaticky historie změn dat záznam.</span><span class="sxs-lookup"><span data-stu-id="87dd0-122">A plugin for Microsoft.EntityFrameworkCore to support automatically recording data changes history.</span></span>

[<span data-ttu-id="87dd0-123">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="87dd0-123">GitHub repository</span></span>](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a><span data-ttu-id="87dd0-124">Microsoft.EntityFrameworkCore.DynamicLinq</span><span class="sxs-lookup"><span data-stu-id="87dd0-124">Microsoft.EntityFrameworkCore.DynamicLinq</span></span>

<span data-ttu-id="87dd0-125">Dynamické Linq rozšíření pro Microsoft.EntityFrameworkCore, který přidává podporu asynchronní</span><span class="sxs-lookup"><span data-stu-id="87dd0-125">Dynamic Linq extensions for Microsoft.EntityFrameworkCore which adds Async support</span></span>

 [<span data-ttu-id="87dd0-126">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="87dd0-126">GitHub repository</span></span>](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efcorepractices"></a><span data-ttu-id="87dd0-127">EFCore.Practices</span><span class="sxs-lookup"><span data-stu-id="87dd0-127">EFCore.Practices</span></span>

<span data-ttu-id="87dd0-128">Pokus o zaznamenat některé funkční nebo osvědčené postupy v rozhraní API, která podporuje testování – včetně malé rozhraní vyhledávání N + 1 dotazy.</span><span class="sxs-lookup"><span data-stu-id="87dd0-128">Attempt to capture some good or best practices in an API that supports testing – including a small framework to scan for N+1 queries.</span></span>

[<span data-ttu-id="87dd0-129">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="87dd0-129">GitHub repository</span></span>](https://github.com/riezebosch/efcore-practices/tree/master/src/EFCore.Practices/)

### <a name="efsecondlevelcachecore"></a><span data-ttu-id="87dd0-130">EFSecondLevelCache.Core</span><span class="sxs-lookup"><span data-stu-id="87dd0-130">EFSecondLevelCache.Core</span></span>

<span data-ttu-id="87dd0-131">Knihovna druhé úrovně ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="87dd0-131">Second Level Caching Library.</span></span> <span data-ttu-id="87dd0-132">Druhé úrovně mezipaměti je mezipaměť a dotazu.</span><span class="sxs-lookup"><span data-stu-id="87dd0-132">Second level caching is a query cache.</span></span> <span data-ttu-id="87dd0-133">Výsledky EF příkazy se uloží do mezipaměti, aby stejné příkazy EF načte data z mezipaměti namísto je znovu prováděna v databázi.</span><span class="sxs-lookup"><span data-stu-id="87dd0-133">The results of EF commands will be stored in the cache, so that the same EF commands will retrieve their data from the cache rather than executing them against the database again.</span></span>

[<span data-ttu-id="87dd0-134">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="87dd0-134">GitHub repository</span></span>](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="detachedentityframework"></a><span data-ttu-id="87dd0-135">Detached.EntityFramework</span><span class="sxs-lookup"><span data-stu-id="87dd0-135">Detached.EntityFramework</span></span>

<span data-ttu-id="87dd0-136">Načítá a ukládá grafy celý odpojit entity (entita s jejich podřízených entit a seznamy).</span><span class="sxs-lookup"><span data-stu-id="87dd0-136">Loads and saves entire detached entity graphs (the entity with their child entities and lists).</span></span> <span data-ttu-id="87dd0-137">INSPIROVANÉ [GraphDiff](https://github.com/refactorthis/GraphDiff/).</span><span class="sxs-lookup"><span data-stu-id="87dd0-137">Inspired by [GraphDiff](https://github.com/refactorthis/GraphDiff/).</span></span> <span data-ttu-id="87dd0-138">Cílem je také přidat simplificate některé moduly plug-in některé opakované úkoly, jako je auditování a stránkování.</span><span class="sxs-lookup"><span data-stu-id="87dd0-138">The idea is also add some plugins to simplificate some repetitive tasks, like auditing and pagination.</span></span>

[<span data-ttu-id="87dd0-139">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="87dd0-139">GitHub repository</span></span>](https://github.com/leonardoporro/Detached/)

### <a name="entityframeworkcoreprimarykey"></a><span data-ttu-id="87dd0-140">EntityFrameworkCore.PrimaryKey</span><span class="sxs-lookup"><span data-stu-id="87dd0-140">EntityFrameworkCore.PrimaryKey</span></span>

<span data-ttu-id="87dd0-141">Primární klíč (včetně složené klíče) ze všechny entity načíst jako slovník.</span><span class="sxs-lookup"><span data-stu-id="87dd0-141">Retrieve the primary key (including composite keys) from any entity as a dictionary.</span></span>

[<span data-ttu-id="87dd0-142">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="87dd0-142">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcorerx"></a><span data-ttu-id="87dd0-143">EntityFrameworkCore.Rx</span><span class="sxs-lookup"><span data-stu-id="87dd0-143">EntityFrameworkCore.Rx</span></span>

<span data-ttu-id="87dd0-144">Přepnutí do reaktivního rozšíření obálek pro aktivní pozorovatelné objekty rozhraní Entity Framework entit.</span><span class="sxs-lookup"><span data-stu-id="87dd0-144">Reactive extension wrappers for hot observables of Entity Framework entities.</span></span>

[<span data-ttu-id="87dd0-145">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="87dd0-145">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.Rx/)

### <a name="entityframeworkcoretriggers"></a><span data-ttu-id="87dd0-146">EntityFrameworkCore.Triggers</span><span class="sxs-lookup"><span data-stu-id="87dd0-146">EntityFrameworkCore.Triggers</span></span>

<span data-ttu-id="87dd0-147">Přidejte aktivační události na váš entity s vložit, aktualizovat a odstraňovat události.</span><span class="sxs-lookup"><span data-stu-id="87dd0-147">Add triggers to your entities with insert, update, and delete events.</span></span> <span data-ttu-id="87dd0-148">Existují tři události pro každou: před, po a při selhání.</span><span class="sxs-lookup"><span data-stu-id="87dd0-148">There are three events for each: before, after, and upon failure.</span></span>

[<span data-ttu-id="87dd0-149">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="87dd0-149">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.Triggers/)

### <a name="entityframeworkcoretypedoriginalvalues"></a><span data-ttu-id="87dd0-150">EntityFrameworkCore.TypedOriginalValues</span><span class="sxs-lookup"><span data-stu-id="87dd0-150">EntityFrameworkCore.TypedOriginalValues</span></span>

<span data-ttu-id="87dd0-151">Získáte typové přístup k původní hodnota vlastnosti vaší entity.</span><span class="sxs-lookup"><span data-stu-id="87dd0-151">Get typed access to the OriginalValue of your entity properties.</span></span> <span data-ttu-id="87dd0-152">Jsou podporovány jednoduché a komplexní vlastností, navigační nebo kolekce nejsou.</span><span class="sxs-lookup"><span data-stu-id="87dd0-152">Simple and complex properties are supported, navigation/collections are not.</span></span>

[<span data-ttu-id="87dd0-153">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="87dd0-153">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a><span data-ttu-id="87dd0-154">Geco</span><span class="sxs-lookup"><span data-stu-id="87dd0-154">Geco</span></span>

<span data-ttu-id="87dd0-155">Geco poskytuje generátor Reverse modelu s podporou Pluralizační/Singularization a upravovat šablony podle jazyka C# 6.0 interpolované řetězce a spuštěná na .net Core.</span><span class="sxs-lookup"><span data-stu-id="87dd0-155">Geco provides a Reverse Model generator with support for Pluralization/Singularization and editable templates based on C# 6.0 interpolated strings and running on .Net Core.</span></span> <span data-ttu-id="87dd0-156">Poskytuje také generátor skriptů počáteční hodnoty s skripty SQL sloučení a runner skriptu.</span><span class="sxs-lookup"><span data-stu-id="87dd0-156">It also provides an Seed script generator with SQL Merge scripts and an script runner.</span></span>

[<span data-ttu-id="87dd0-157">Úložiště Github</span><span class="sxs-lookup"><span data-stu-id="87dd0-157">Github repository</span></span>](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a><span data-ttu-id="87dd0-158">LinqKit.Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="87dd0-158">LinqKit.Microsoft.EntityFrameworkCore</span></span>

<span data-ttu-id="87dd0-159">LinqKit.Microsoft.EntityFrameworkCore je bezplatná sada rozšíření pro výrazy LINQ SQL a EntityFrameworkCore skupiny power users.</span><span class="sxs-lookup"><span data-stu-id="87dd0-159">LinqKit.Microsoft.EntityFrameworkCore is a free set of extensions for LINQ to SQL and EntityFrameworkCore power users.</span></span> <span data-ttu-id="87dd0-160">S podporou Include(...) a IDbAsync.</span><span class="sxs-lookup"><span data-stu-id="87dd0-160">With Include(...) and IDbAsync support.</span></span>

[<span data-ttu-id="87dd0-161">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="87dd0-161">GitHub repository</span></span>](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a><span data-ttu-id="87dd0-162">NeinLinq.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="87dd0-162">NeinLinq.EntityFrameworkCore</span></span>

<span data-ttu-id="87dd0-163">NeinLinq.EntityFrameworkCore poskytuje rozšíření užitečná pro používání LINQ zprostředkovatelů například Entity Framework podporují pouze menší podmnožinu funkcí rozhraní .NET, opětovné použití funkce přepisování dotazy, i přitom bezpečných hodnotu null a sestavování dynamických dotazů pomocí nepřeložitelná predikáty a selektorů.</span><span class="sxs-lookup"><span data-stu-id="87dd0-163">NeinLinq.EntityFrameworkCore provides helpful extensions for using LINQ providers such as Entity Framework that support only a minor subset of .NET functions, reusing functions, rewriting queries, even making them null-safe, and building dynamic queries using translatable predicates and selectors.</span></span>

[<span data-ttu-id="87dd0-164">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="87dd0-164">GitHub repository</span></span>](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a><span data-ttu-id="87dd0-165">Microsoft.EntityFrameworkCore.UnitOfWork</span><span class="sxs-lookup"><span data-stu-id="87dd0-165">Microsoft.EntityFrameworkCore.UnitOfWork</span></span>

<span data-ttu-id="87dd0-166">Modul plug-in pro Microsoft.EntityFrameworkCore pro podporu více databáze, úložiště a jednotky pracovních vzorů s distribuované transakce podporován.</span><span class="sxs-lookup"><span data-stu-id="87dd0-166">A plugin for Microsoft.EntityFrameworkCore to support repository, unit of work patterns, and multiple database with distributed transaction supported.</span></span>

[<span data-ttu-id="87dd0-167">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="87dd0-167">GitHub repository</span></span>](https://github.com/Arch/UnitOfWork/)

### <a name="entityframeworklazyloading"></a><span data-ttu-id="87dd0-168">EntityFramework.LazyLoading</span><span class="sxs-lookup"><span data-stu-id="87dd0-168">EntityFramework.LazyLoading</span></span>

<span data-ttu-id="87dd0-169">Opožděného načítání pro základní EF 1.1</span><span class="sxs-lookup"><span data-stu-id="87dd0-169">Lazy Loading for EF Core 1.1</span></span>

[<span data-ttu-id="87dd0-170">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="87dd0-170">GitHub repository</span></span>](https://github.com/darxis/EntityFramework.LazyLoading)

### <a name="efcorebulkextensions"></a><span data-ttu-id="87dd0-171">EFCore.BulkExtensions</span><span class="sxs-lookup"><span data-stu-id="87dd0-171">EFCore.BulkExtensions</span></span>

<span data-ttu-id="87dd0-172">EntityFrameworkCore rozšíření pro hromadné operace (příkaz Insert, Update, Delete).</span><span class="sxs-lookup"><span data-stu-id="87dd0-172">EntityFrameworkCore extensions for Bulk operations (Insert, Update, Delete).</span></span>

[<span data-ttu-id="87dd0-173">Úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="87dd0-173">GitHub repository</span></span>](https://github.com/borisdj/EFCore.BulkExtensions)
