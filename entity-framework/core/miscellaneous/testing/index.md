---
title: Testování součástí pomocí EF Core - EF Core
description: Různé přístupy k testování aplikací, které používají EF Core
author: ajcvickers
ms.date: 03/23/2020
uid: core/miscellaneous/testing/index
ms.openlocfilehash: b1ab37ebb0a3aae09d5d5b225f746cf83dfba170
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "80634254"
---
# <a name="testing-code-that-uses-ef-core"></a><span data-ttu-id="4a5e5-103">Testovací kód, který používá EF Core</span><span class="sxs-lookup"><span data-stu-id="4a5e5-103">Testing code that uses EF Core</span></span>

<span data-ttu-id="4a5e5-104">Testování kódu, který přistupuje k databázi, vyžaduje buď:</span><span class="sxs-lookup"><span data-stu-id="4a5e5-104">Testing code that accesses a database requires either:</span></span>
* <span data-ttu-id="4a5e5-105">Spouštění dotazů a aktualizací proti stejnému databázovému systému používanému v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-105">Running queries and updates against the same database system used in production.</span></span>
* <span data-ttu-id="4a5e5-106">Spouštění dotazů a aktualizací proti jiným snadněji spravovatelným databázovým systémem.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-106">Running queries and updates against some other easier to manage database system.</span></span>
* <span data-ttu-id="4a5e5-107">Použití test zdvojnásobí nebo nějaký jiný mechanismus, aby se zabránilo použití databáze vůbec.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-107">Using test doubles or some other mechanism to avoid using a database at all.</span></span>

<span data-ttu-id="4a5e5-108">Tento dokument popisuje kompromisy zapojené do každé z těchto voleb a ukazuje, jak ef core lze použít s každým přístupem.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-108">This document outlines the trade offs involved in each of these choices and shows how EF Core can be used with each approach.</span></span>  

## <a name="all-database-providers-are-not-equal"></a><span data-ttu-id="4a5e5-109">Všichni poskytovatelé databáze nejsou rovni.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-109">All database providers are not equal</span></span>

<span data-ttu-id="4a5e5-110">Je velmi důležité si uvědomit, že EF Core není navržen tak, aby abstraktní každý aspekt základního databázového systému.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-110">It is very important to understand that EF Core is not designed to abstract every aspect of the underlying database system.</span></span>
<span data-ttu-id="4a5e5-111">Místo toho EF Core je společná sada vzorů a konceptů, které lze použít s libovolnýdatabázový systém.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-111">Instead, EF Core is a common set of patterns and concepts that can be used with any database system.</span></span>
<span data-ttu-id="4a5e5-112">Zprostředkovatelé databáze EF Core pak vrstva databázi specifické chování a funkce přes tento společný rámec.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-112">EF Core database providers then layer database-specific behavior and functionality over this common framework.</span></span>
<span data-ttu-id="4a5e5-113">To umožňuje každému databázovému systému dělat to, co umí nejlépe, a přitom zachovat shodnost, kde je to vhodné, s jinými databázovými systémy.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-113">This allows each database system to do what it does best while still maintaining commonality, where appropriate, with other database systems.</span></span> 

<span data-ttu-id="4a5e5-114">V zásadě to znamená, že přepnutí zprostředkovatele databáze změní chování EF Core a aplikace nelze očekávat, že bude fungovat správně, pokud explicitně účty pro všechny rozdíly v chování.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-114">Fundamentally, this means that switching out the database provider will change EF Core behavior and the application can't be expected to function correctly unless it explicitly accounts for all differences in behavior.</span></span>
<span data-ttu-id="4a5e5-115">To bylo řečeno, v mnoha případech to bude fungovat, protože tam je vysoký stupeň shodnosti mezi relačnídatabáze.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-115">That being said, in many cases doing this will work because there is a high degree of commonality amongst relational databases.</span></span>
<span data-ttu-id="4a5e5-116">Tohle je dobré a špatné.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-116">This is good and bad.</span></span>
<span data-ttu-id="4a5e5-117">Dobré, protože přechod mezi databázemi může být poměrně snadné.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-117">Good because moving between databases can be relatively easy.</span></span>
<span data-ttu-id="4a5e5-118">Špatné, protože může poskytnout falešný pocit zabezpečení, pokud aplikace není plně testována proti novému databázovému systému.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-118">Bad because it can give a false sense of security if the application is not fully tested against the new database system.</span></span>  

## <a name="approach-1-production-database-system"></a><span data-ttu-id="4a5e5-119">Přístup 1: Systém výrobních databází</span><span class="sxs-lookup"><span data-stu-id="4a5e5-119">Approach 1: Production database system</span></span>

<span data-ttu-id="4a5e5-120">Jak je popsáno v předchozí části, jediný způsob, jak se ujistit, že testujete, co běží v produkčním prostředí je použít stejný databázový systém.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-120">As described in the previous section, the only way to be sure you are testing what runs in production is to use the same database system.</span></span>
<span data-ttu-id="4a5e5-121">Například pokud nasazená aplikace používá SQL Azure, pak testování by mělo být provedeno také proti SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-121">For example, if the deployed application uses SQL Azure, then testing should also be done against SQL Azure.</span></span>

<span data-ttu-id="4a5e5-122">Však s každý vývojář spustit testy proti SQL Azure při aktivní práci na kódu by bylo pomalé a nákladné.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-122">However, having every developer run tests against SQL Azure while actively working on the code would be both slow and expensive.</span></span>
<span data-ttu-id="4a5e5-123">To ilustruje hlavní kompromis v rámci těchto přístupů: kdy je vhodné odchýlit se od systému výrobních databází, aby se zlepšila účinnost zkoušek?</span><span class="sxs-lookup"><span data-stu-id="4a5e5-123">This illustrates the main trade off involved throughout these approaches: when is it appropriate to deviate from the production database system so as to improve test efficiency?</span></span>

<span data-ttu-id="4a5e5-124">Naštěstí v tomto případě je odpověď poměrně snadná: použijte místní nebo místní SQL Server pro testování vývojářů.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-124">Luckily, in this case the answer is quite easy: use local or on-premises SQL Server for developer testing.</span></span>
<span data-ttu-id="4a5e5-125">SQL Azure a SQL Server jsou velmi podobné, takže testování proti SQL Server je obvykle přiměřené kompromis.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-125">SQL Azure and SQL Server are extremely similar, so testing against SQL Server is usually a reasonable trade off.</span></span>
<span data-ttu-id="4a5e5-126">Jak bylo řečeno, je stále rozumné spustit testy proti SQL Azure sám před vstupem do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-126">That being said, it is still wise to run tests against SQL Azure itself before going into production.</span></span>
 
### <a name="localdb"></a><span data-ttu-id="4a5e5-127">Místní databáze</span><span class="sxs-lookup"><span data-stu-id="4a5e5-127">LocalDb</span></span> 

<span data-ttu-id="4a5e5-128">Všechny hlavní databázové systémy mají nějakou formu "Developer Edition" pro místní testování.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-128">All the major database systems have some form of "Developer Edition" for local testing.</span></span>
<span data-ttu-id="4a5e5-129">SQL Server má také funkci nazvanou [LocalDb](/sql/database-engine/configure-windows/sql-server-express-localdb?view=sql-server-ver15).</span><span class="sxs-lookup"><span data-stu-id="4a5e5-129">SQL Server also also has a feature called [LocalDb](/sql/database-engine/configure-windows/sql-server-express-localdb?view=sql-server-ver15).</span></span>
<span data-ttu-id="4a5e5-130">Hlavní výhodou LocalDb je, že se točí do instance databáze na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-130">The primary advantage of LocalDb is that it spins up the database instance on demand.</span></span>
<span data-ttu-id="4a5e5-131">Tím se zabrání spuštění databázové služby v počítači i v případě, že nespouštějí testy.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-131">This avoids having a database service running on your machine even when you're not running tests.</span></span>

<span data-ttu-id="4a5e5-132">LocalDb není bez jeho problémů:</span><span class="sxs-lookup"><span data-stu-id="4a5e5-132">LocalDb is not without it's issues:</span></span>
* <span data-ttu-id="4a5e5-133">Nepodporuje vše, co [SQL Server Developer Edition](/sql/sql-server/editions-and-components-of-sql-server-2016?view=sql-server-ver15) dělá.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-133">It doesn't support everything that [SQL Server Developer Edition](/sql/sql-server/editions-and-components-of-sql-server-2016?view=sql-server-ver15) does.</span></span>
* <span data-ttu-id="4a5e5-134">Není k dispozici na Linuxu.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-134">It isn't available on Linux.</span></span>
* <span data-ttu-id="4a5e5-135">To může způsobit zpoždění při prvním spuštění testu jako služba se točil nahoru.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-135">It can cause lag on first test run as the service is spun up.</span></span>

<span data-ttu-id="4a5e5-136">Osobně jsem nikdy nenašel to problém s databázovou službu běží na mém dev stroji a já bych obecně doporučujeme používat Developer Edition místo.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-136">Personally, I've never found it a problem having a database service running on my dev machine and I would generally recommend using Developer Edition instead.</span></span>
<span data-ttu-id="4a5e5-137">Nicméně, to může být vhodné pro některé lidi, zejména na méně výkonné dev stroje.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-137">However, it may be appropriate for some people, especially on less powerful dev machines.</span></span>  

## <a name="approach-2-sqlite"></a><span data-ttu-id="4a5e5-138">Přístup 2: SQLite</span><span class="sxs-lookup"><span data-stu-id="4a5e5-138">Approach 2: SQLite</span></span>

<span data-ttu-id="4a5e5-139">EF Core testuje zprostředkovatele SERVERU SQL Server především spuštěním proti místní instanci serveru SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-139">EF Core tests the SQL Server provider primarily by running it against a local SQL Server instance.</span></span>
<span data-ttu-id="4a5e5-140">Tyto testy spustit desítky tisíc dotazů během několika minut na rychlé mši.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-140">These tests run tens of thousands of queries in a couple of minutes on a fast machine.</span></span>
<span data-ttu-id="4a5e5-141">To ukazuje, že použití reálného databázového systému může být performant řešení.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-141">This illustrates that using the real database system can be a performant solution.</span></span>
<span data-ttu-id="4a5e5-142">Je mýtus, že použití některých lehčí-váha databáze je jediný způsob, jak spustit testy rychle.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-142">It is a myth that using some lighter-weight database is the only way to run tests quickly.</span></span>

<span data-ttu-id="4a5e5-143">Jak již bylo řečeno, co když z nějakého důvodu nemůžete spustit testy proti něčemu, co se blíží vašemu produkčnímu databázovému systému?</span><span class="sxs-lookup"><span data-stu-id="4a5e5-143">That being said, what if for whatever reason you can't run tests against something close to your production database system?</span></span>
<span data-ttu-id="4a5e5-144">Další nejlepší volbou je použít něco s podobnou funkčností.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-144">The next best choice is to use something with similar functionality.</span></span>
<span data-ttu-id="4a5e5-145">To obvykle znamená další relační databáze, pro které [SQLite](https://sqlite.org/index.html) je zřejmá volba.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-145">This usually means another relational database, for which [SQLite](https://sqlite.org/index.html) is the obvious choice.</span></span>

<span data-ttu-id="4a5e5-146">SQLite je dobrá volba, protože:</span><span class="sxs-lookup"><span data-stu-id="4a5e5-146">SQLite is a good choice because:</span></span>
* <span data-ttu-id="4a5e5-147">Běží v procesu s vaší aplikací a tak má nízkou režii.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-147">It runs in-process with your application and so has low overhead.</span></span>
* <span data-ttu-id="4a5e5-148">Používá jednoduché, automaticky vytvořené soubory pro databáze, a proto nevyžaduje správu databáze.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-148">It uses simple, automatically created files for databases, and so doesn't require database management.</span></span>
* <span data-ttu-id="4a5e5-149">Má režim v paměti, který zabraňuje i vytvoření souboru.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-149">It has an in-memory mode that avoids even the file creation.</span></span>

<span data-ttu-id="4a5e5-150">Nezapomeňte však, že:</span><span class="sxs-lookup"><span data-stu-id="4a5e5-150">However, remember that:</span></span>
* <span data-ttu-id="4a5e5-151">Nevyhnutelnost SQLite nepodporuje vše, co váš produkční databázový systém dělá.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-151">SQLite inevitability doesn't support everything that your production database system does.</span></span>
* <span data-ttu-id="4a5e5-152">SQLite se bude chovat jinak než produkční databázový systém pro některé dotazy.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-152">SQLite will behave differently than your production database system for some queries.</span></span>

<span data-ttu-id="4a5e5-153">Takže pokud používáte SQLite pro některé testování, ujistěte se, že také test proti skutečnédatabázový systém.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-153">So if you do use SQLite for some testing, make sure to also test against your real database system.</span></span>

<span data-ttu-id="4a5e5-154">Viz [testování s SQLite](xref:core/miscellaneous/testing/sqlite) pro EF Core specifické pokyny.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-154">See [Testing with SQLite](xref:core/miscellaneous/testing/sqlite) for EF Core specific guidance.</span></span> 

## <a name="approach-3-the-ef-core-in-memory-database"></a><span data-ttu-id="4a5e5-155">Přístup 3: Ef Core v paměti databáze</span><span class="sxs-lookup"><span data-stu-id="4a5e5-155">Approach 3: The EF Core in-memory database</span></span>

<span data-ttu-id="4a5e5-156">EF Core je dodáván s databází v paměti, kterou používáme pro interní testování samotného EF Core.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-156">EF Core comes with an in-memory database that we use for internal testing of EF Core itself.</span></span>
<span data-ttu-id="4a5e5-157">Tato databáze obecně **není vhodná jako náhrada za testovací aplikace, které používají EF Core**.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-157">This database is in general **not suitable as a substitute for testing applications that use EF Core**.</span></span> <span data-ttu-id="4a5e5-158">Konkrétně:</span><span class="sxs-lookup"><span data-stu-id="4a5e5-158">Specifically:</span></span>
* <span data-ttu-id="4a5e5-159">Nejedná se o relační databázi</span><span class="sxs-lookup"><span data-stu-id="4a5e5-159">It is not a relational database</span></span>
* <span data-ttu-id="4a5e5-160">Nepodporuje transakce</span><span class="sxs-lookup"><span data-stu-id="4a5e5-160">It doesn't support transactions</span></span>
* <span data-ttu-id="4a5e5-161">Není optimalizován pro výkon</span><span class="sxs-lookup"><span data-stu-id="4a5e5-161">It is not optimized for performance</span></span>

<span data-ttu-id="4a5e5-162">Nic z toho není velmi důležité při testování vnitřních souborů EF Core, protože ji používáme konkrétně tam, kde je databáze irelevantní pro test.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-162">None of this is very important when testing EF Core internals because we use it specifically where the database is irrelevant to the test.</span></span>
<span data-ttu-id="4a5e5-163">Na druhou stranu tyto věci mají tendenci být velmi důležité při testování aplikace, která používá EF Core.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-163">On the other hand, these things tend to be very important when testing an application that uses EF Core.</span></span>

## <a name="unit-testing"></a><span data-ttu-id="4a5e5-164">Testování jednotek</span><span class="sxs-lookup"><span data-stu-id="4a5e5-164">Unit testing</span></span>

<span data-ttu-id="4a5e5-165">Zvažte testování obchodní logiky, která může být nutné použít některá data z databáze, ale není ze své podstaty testování interakcí databáze.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-165">Consider testing a piece of business logic that might need to use some data from a database, but is not inherently testing the database interactions.</span></span>
<span data-ttu-id="4a5e5-166">Jednou z možností je použít [test double,](https://en.wikipedia.org/wiki/Test_double) jako je například falešný nebo falešný.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-166">One option is to use a [test double](https://en.wikipedia.org/wiki/Test_double) such as a mock or fake.</span></span>

<span data-ttu-id="4a5e5-167">Pro interní testování EF Core používáme zkušební dvojinky.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-167">We use test doubles for internal testing of EF Core.</span></span>
<span data-ttu-id="4a5e5-168">Však nikdy se snažíme zesměšňovat DbContext nebo IQueryable.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-168">However, we never try to mock DbContext or IQueryable.</span></span>
<span data-ttu-id="4a5e5-169">Je to obtížné, těžkopádné a křehké.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-169">Doing so is difficult, cumbersome, and fragile.</span></span>
<span data-ttu-id="4a5e5-170">**Nedělej to.**</span><span class="sxs-lookup"><span data-stu-id="4a5e5-170">**Don't do it.**</span></span>

<span data-ttu-id="4a5e5-171">Místo toho používáme databázi v paměti při testování částí něco, co používá DbContext.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-171">Instead we use the in-memory database when unit testing something that uses DbContext.</span></span>
<span data-ttu-id="4a5e5-172">V tomto případě je vhodné používat databázi v paměti, protože test není závislý na chování databáze.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-172">In this case using the in-memory database is appropriate because the test is not dependent on database behavior.</span></span>
<span data-ttu-id="4a5e5-173">Jen to nedělají k testování skutečných databázových dotazů nebo aktualizací.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-173">Just don't do this to test actual database queries or updates.</span></span>   

<span data-ttu-id="4a5e5-174">Viz [Testování s poskytovatelem v paměti](xref:core/miscellaneous/testing/in-memory) pro EF Core specifické pokyny pro použití databáze v paměti pro testování částí.</span><span class="sxs-lookup"><span data-stu-id="4a5e5-174">See [Testing with the in-memory provider](xref:core/miscellaneous/testing/in-memory) for EF Core specific guidance on using the in-memory database for unit testing.</span></span>
