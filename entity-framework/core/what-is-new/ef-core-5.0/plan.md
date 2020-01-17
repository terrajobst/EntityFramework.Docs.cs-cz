---
title: Plán pro Entity Framework Core 5,0
author: ajcvickers
ms.date: 01/14/2020
uid: core/what-is-new/ef-core-5.0/plan.md
ms.openlocfilehash: 0472841fdcd105ec8ea38db062c6768510b8735d
ms.sourcegitcommit: f2a38c086291699422d8b28a72d9611d1b24ad0d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/16/2020
ms.locfileid: "76125380"
---
# <a name="plan-for-entity-framework-core-50"></a><span data-ttu-id="7b543-102">Plán pro Entity Framework Core 5,0</span><span class="sxs-lookup"><span data-stu-id="7b543-102">Plan for Entity Framework Core 5.0</span></span>

<span data-ttu-id="7b543-103">Jak je popsáno v [procesu plánování](../release-planning.md), shromáždili jsme vstup od zúčastněných stran do předběžného plánu pro vydání verze EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="7b543-103">As described in the [planning process](../release-planning.md), we have gathered input from stakeholders into a tentative plan for the EF Core 5.0 release.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="7b543-104">Tento plán je stále v průběhu práce.</span><span class="sxs-lookup"><span data-stu-id="7b543-104">This plan is still a work-in-progress.</span></span> <span data-ttu-id="7b543-105">Tady není žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="7b543-105">Nothing here is a commitment.</span></span> <span data-ttu-id="7b543-106">Tento plán je výchozím bodem, který se bude vyvíjet, jakmile se dozvíte víc.</span><span class="sxs-lookup"><span data-stu-id="7b543-106">This plan is a starting point that will evolve as we learn more.</span></span> <span data-ttu-id="7b543-107">Některé věci, které nejsou aktuálně plánovány pro 5,0, mohou být získány v.</span><span class="sxs-lookup"><span data-stu-id="7b543-107">Some things not currently planned for 5.0 may get pulled in.</span></span> <span data-ttu-id="7b543-108">Některé věci, které jsou aktuálně plánovány pro 5,0, mohou punted.</span><span class="sxs-lookup"><span data-stu-id="7b543-108">Some things currently planned for 5.0 may get punted out.</span></span>

### <a name="version-number-and-release-date"></a><span data-ttu-id="7b543-109">Číslo verze a datum vydání</span><span class="sxs-lookup"><span data-stu-id="7b543-109">Version number and release date.</span></span>

<span data-ttu-id="7b543-110">Verze EF Core 5,0 je aktuálně naplánována na vydání ve [stejnou dobu jako rozhraní .net 5,0](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span><span class="sxs-lookup"><span data-stu-id="7b543-110">EF Core 5.0 is currently scheduled for release at [the same time as .NET 5.0](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span></span> <span data-ttu-id="7b543-111">Verze "5,0" byla zvolena pro zarovnání s .NET 5,0.</span><span class="sxs-lookup"><span data-stu-id="7b543-111">The version "5.0" was chosen to align with .NET 5.0.</span></span>

### <a name="supported-platforms"></a><span data-ttu-id="7b543-112">Podporované platformy</span><span class="sxs-lookup"><span data-stu-id="7b543-112">Supported platforms</span></span>

<span data-ttu-id="7b543-113">EF Core 5,0 se plánuje běžet na jakékoli platformě .NET 5,0 na základě [sblížení těchto platforem až po .NET Core](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span><span class="sxs-lookup"><span data-stu-id="7b543-113">EF Core 5.0 is planned to run on any .NET 5.0 platform based on the [convergence of these platforms to .NET Core](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span></span> <span data-ttu-id="7b543-114">To znamená, že v souvislosti s .NET Standard se stále definuje vlastní použité TFM.</span><span class="sxs-lookup"><span data-stu-id="7b543-114">What this means in terms of .NET Standard and the actual TFM used is still TBD.</span></span>

<span data-ttu-id="7b543-115">EF Core 5,0 se na .NET Framework nespustí.</span><span class="sxs-lookup"><span data-stu-id="7b543-115">EF Core 5.0 will not run on .NET Framework.</span></span>

### <a name="breaking-changes"></a><span data-ttu-id="7b543-116">Změny způsobující chyby</span><span class="sxs-lookup"><span data-stu-id="7b543-116">Breaking changes</span></span>

<span data-ttu-id="7b543-117">EF Core 5,0 bude obsahovat některé zásadní změny, ale bude to mnohem méně závažné, než bylo v případě EF Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="7b543-117">EF Core 5.0 will contain some breaking changes, but these will be much less severe than was the case for EF Core 3.0.</span></span> <span data-ttu-id="7b543-118">Naším cílem je dovolit, aby se velká většina aplikací aktualizovala bez přerušení.</span><span class="sxs-lookup"><span data-stu-id="7b543-118">Our goal is to allow the vast majority of applications to update without breaking.</span></span>

<span data-ttu-id="7b543-119">Očekává se, že pro poskytovatele databáze budou zásadní změny, zejména pokud jde o podporu TPT.</span><span class="sxs-lookup"><span data-stu-id="7b543-119">It is expected that there will be some breaking changes for database providers, especially around TPT support.</span></span> <span data-ttu-id="7b543-120">Očekáváme ale, že práce, která aktualizuje zprostředkovatele pro 5,0, bude menší, než se vyžaduje aktualizace pro 3,0.</span><span class="sxs-lookup"><span data-stu-id="7b543-120">However, we expect the work to update a provider for 5.0 will be less than was required to update for 3.0.</span></span>

## <a name="themes"></a><span data-ttu-id="7b543-121">Motivy</span><span class="sxs-lookup"><span data-stu-id="7b543-121">Themes</span></span>

<span data-ttu-id="7b543-122">Extrahovali jsme několik hlavních oblastí nebo motivů, které budou tvořit základ pro velké investice do EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="7b543-122">We have extracted a few major areas or themes which will form the basis for the large investments in EF Core 5.0.</span></span>

## <a name="many-to-many-navigation-properties-aka-skip-navigations"></a><span data-ttu-id="7b543-123">Navigační vlastnosti m:n (a. k) pro přeskočení navigace (a. k)</span><span class="sxs-lookup"><span data-stu-id="7b543-123">Many-to-many navigation properties (a.k.a "skip navigations")</span></span>

<span data-ttu-id="7b543-124">Vývojářé olova: @smitpatel a @AndriySvyryd</span><span class="sxs-lookup"><span data-stu-id="7b543-124">Lead developers: @smitpatel and @AndriySvyryd</span></span>

<span data-ttu-id="7b543-125">Sledováno [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)</span><span class="sxs-lookup"><span data-stu-id="7b543-125">Tracked by [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)</span></span>

<span data-ttu-id="7b543-126">Velikost trička pro T: L</span><span class="sxs-lookup"><span data-stu-id="7b543-126">T-shirt size: L</span></span>

<span data-ttu-id="7b543-127">Stav: probíhá</span><span class="sxs-lookup"><span data-stu-id="7b543-127">Status: In-progress</span></span>

<span data-ttu-id="7b543-128">Na nevyřízených položkách GitHubu je nejužitečnější funkce m:n (přibližně 407 hlasy).</span><span class="sxs-lookup"><span data-stu-id="7b543-128">Many-to-many is the most requested feature (~407 votes) on the GitHub backlog.</span></span> <span data-ttu-id="7b543-129">Podpora vztahů m:n je možné rozdělit na tři hlavní oblasti:</span><span class="sxs-lookup"><span data-stu-id="7b543-129">Support for many-to-many relationships can be broken down into three major areas:</span></span>

* <span data-ttu-id="7b543-130">Přeskočit navigační vlastnosti</span><span class="sxs-lookup"><span data-stu-id="7b543-130">Skip navigation properties.</span></span> <span data-ttu-id="7b543-131">To umožňuje, aby byl model použit pro dotazy atd. bez odkazů na podkladovou entitu JOIN Table.</span><span class="sxs-lookup"><span data-stu-id="7b543-131">These allow the model to be used for queries, etc. without reference to the underlying join table entity.</span></span>
* <span data-ttu-id="7b543-132">Typy entit kontejneru vlastností.</span><span class="sxs-lookup"><span data-stu-id="7b543-132">Property-bag entity types.</span></span> <span data-ttu-id="7b543-133">Ty umožňují použití standardního typu CLR (např. `Dictionary`) pro instance entit tak, že pro každý typ entity není vyžadován explicitní typ CLR.</span><span class="sxs-lookup"><span data-stu-id="7b543-133">These allow a standard CLR type (e.g. `Dictionary`) to be used for entity instances such that an explicit CLR type is not needed for each entity type.</span></span>
* <span data-ttu-id="7b543-134">Cukr pro jednoduchou konfiguraci relací m:n.</span><span class="sxs-lookup"><span data-stu-id="7b543-134">Sugar for easy configuration of many-to-many relationships.</span></span>

<span data-ttu-id="7b543-135">Věříme, že nejvýznamnější blokování pro ty, kteří vyžadují podporu m:n, nedokáže používat "přirozené" vztahy, a to bez odkazování na tabulku spojení v obchodní logice, jako jsou dotazy.</span><span class="sxs-lookup"><span data-stu-id="7b543-135">We believe that the most significant blocker for those wanting many-to-many support is not being able to use the "natural" relationships, without referring to the join table, in business logic such as queries.</span></span> <span data-ttu-id="7b543-136">Typ entity tabulky join může stále existovat, ale neměl by se zobrazovat v cestě k obchodní logice.</span><span class="sxs-lookup"><span data-stu-id="7b543-136">The join table entity type may still exist, but it should not get in the way of business logic.</span></span> <span data-ttu-id="7b543-137">To je důvod, proč jsme se rozhodli Přeskočit navigační vlastnosti pro 5,0.</span><span class="sxs-lookup"><span data-stu-id="7b543-137">This is why we have chosen to tackle skip navigation properties for 5.0.</span></span>

<span data-ttu-id="7b543-138">V tuto chvíli se pro EF Core 5,0 sledují i další části m:n.</span><span class="sxs-lookup"><span data-stu-id="7b543-138">At this time the other parts of many-to-many are being pursued as a stretch goal for EF Core 5.0.</span></span> <span data-ttu-id="7b543-139">To znamená, že nejsou v současnosti v plánu pro 5,0, ale pokud se o to dobře myslí, doufáme, že si je vyžádáme.</span><span class="sxs-lookup"><span data-stu-id="7b543-139">This means they are not currently in the plan for 5.0, but if things go well we hope to pull them in.</span></span>

## <a name="table-per-type-tpt-inheritance-mapping"></a><span data-ttu-id="7b543-140">Mapování dědičnosti typu tabulka na typ (TPT)</span><span class="sxs-lookup"><span data-stu-id="7b543-140">Table-per-type (TPT) inheritance mapping</span></span>

<span data-ttu-id="7b543-141">Vedoucí vývojář: @AndriySvyryd</span><span class="sxs-lookup"><span data-stu-id="7b543-141">Lead developer: @AndriySvyryd</span></span>

<span data-ttu-id="7b543-142">Sledováno [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)</span><span class="sxs-lookup"><span data-stu-id="7b543-142">Tracked by [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)</span></span>

<span data-ttu-id="7b543-143">Velikost T-tričko: XL</span><span class="sxs-lookup"><span data-stu-id="7b543-143">T-shirt size: XL</span></span>

<span data-ttu-id="7b543-144">Stav: Nezahájeno</span><span class="sxs-lookup"><span data-stu-id="7b543-144">Status: Not started</span></span>

<span data-ttu-id="7b543-145">Provádíme TPT, protože se jedná o vysoce požadovanou funkci (~ 254 hlasy, celkově třetí) a protože vyžaduje některé změny nízké úrovně, které jsou vhodné pro základní povahu celkového plánu .NET 5.</span><span class="sxs-lookup"><span data-stu-id="7b543-145">We're doing TPT because it is both a highly requested feature (~254 votes; 3rd overall) and because it requires some low-level changes that we feel are appropriate for the foundational nature of the overall .NET 5 plan.</span></span> <span data-ttu-id="7b543-146">Očekáváme, že dojde k zásadním změnám u zprostředkovatelů databází, i když by měly být mnohem méně závažné, než změny požadované pro 3,0.</span><span class="sxs-lookup"><span data-stu-id="7b543-146">We expect this to result in breaking changes for database providers, although these should be much less severe than the changes required for 3.0.</span></span>

## <a name="filtered-include"></a><span data-ttu-id="7b543-147">Filtrované zahrnutí</span><span class="sxs-lookup"><span data-stu-id="7b543-147">Filtered Include</span></span>

<span data-ttu-id="7b543-148">Vedoucí vývojář: @maumar</span><span class="sxs-lookup"><span data-stu-id="7b543-148">Lead developer: @maumar</span></span>

<span data-ttu-id="7b543-149">Sledováno [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)</span><span class="sxs-lookup"><span data-stu-id="7b543-149">Tracked by [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)</span></span>

<span data-ttu-id="7b543-150">Velikost T-tričko: M</span><span class="sxs-lookup"><span data-stu-id="7b543-150">T-shirt size: M</span></span>

<span data-ttu-id="7b543-151">Stav: Nezahájeno</span><span class="sxs-lookup"><span data-stu-id="7b543-151">Status: Not started</span></span>

<span data-ttu-id="7b543-152">Filtrované zahrnutí je vysoce vyžádaná funkce (~ 317 hlasy, druhá celková), která není obrovským objemem práce a že se domníváte, že bude odblokovat nebo dělat jednodušší mnoho scénářů, které aktuálně vyžadují filtry na úrovni modelu nebo složitější dotazy.</span><span class="sxs-lookup"><span data-stu-id="7b543-152">Filtered Include is a highly-requested feature (~317 votes; 2nd overall) that isn't a huge amount of work, and that we believe will unblock or make easier many scenarios that currently require model-level filters or more complex queries.</span></span>

## <a name="rationalize-totable-toquery-toview-fromsql-etc"></a><span data-ttu-id="7b543-153">Racionalizovat ToTable, ToQuery, ToView, Z tabulek atd.</span><span class="sxs-lookup"><span data-stu-id="7b543-153">Rationalize ToTable, ToQuery, ToView, FromSql, etc.</span></span>

<span data-ttu-id="7b543-154">Vývojářé olova: @maumar a @smitpatel</span><span class="sxs-lookup"><span data-stu-id="7b543-154">Lead developers: @maumar and @smitpatel</span></span>

<span data-ttu-id="7b543-155">Sledováno [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)</span><span class="sxs-lookup"><span data-stu-id="7b543-155">Tracked by [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)</span></span>

<span data-ttu-id="7b543-156">Velikost trička pro T: L</span><span class="sxs-lookup"><span data-stu-id="7b543-156">T-shirt size: L</span></span>

<span data-ttu-id="7b543-157">Stav: Nezahájeno</span><span class="sxs-lookup"><span data-stu-id="7b543-157">Status: Not started</span></span>

<span data-ttu-id="7b543-158">V předchozích verzích jsme udělali postup k podpoře nezpracovaných SQL, bez klíčůch typů a souvisejících oblastí.</span><span class="sxs-lookup"><span data-stu-id="7b543-158">We have made progress in previous releases towards supporting raw SQL, keyless types, and related areas.</span></span> <span data-ttu-id="7b543-159">Existují však mezery i nekonzistence, jak vše funguje dohromady jako celek.</span><span class="sxs-lookup"><span data-stu-id="7b543-159">However, there are both gaps and inconsistencies in the way everything works together as a whole.</span></span> <span data-ttu-id="7b543-160">Cílem 5,0 je opravit tyto možnosti a vytvořit dobré prostředí pro definování, migraci a používání různých typů entit a jejich přidružených dotazů a artefaktů databáze.</span><span class="sxs-lookup"><span data-stu-id="7b543-160">The goal for 5.0 is to fix these and create a good experience for defining, migrating, and using different types of entities and their associated queries and database artifacts.</span></span> <span data-ttu-id="7b543-161">To může také zahrnovat aktualizace zkompilovaného rozhraní API pro dotazy.</span><span class="sxs-lookup"><span data-stu-id="7b543-161">This may also involve updates to the compiled query API.</span></span>

<span data-ttu-id="7b543-162">Všimněte si, že tato položka může vést k nějakým změnám na úrovni aplikace, protože některé z funkcí, které aktuálně máme, jsou moc opravňující, takže můžou rychle vést k pitsi selhání.</span><span class="sxs-lookup"><span data-stu-id="7b543-162">Note that this item may result in some application-level breaking changes since some of the functionality we currently have is too permissive such that it can quickly lead people into pits of failure.</span></span> <span data-ttu-id="7b543-163">Budeme nejspíš zablokovat některé z těchto funkcí společně s pokyny k tomu, co dělat.</span><span class="sxs-lookup"><span data-stu-id="7b543-163">We will likely end up blocking some of this functionality together with guidance on what to do instead.</span></span>

## <a name="general-query-enhancements"></a><span data-ttu-id="7b543-164">Obecná vylepšení dotazů</span><span class="sxs-lookup"><span data-stu-id="7b543-164">General query enhancements</span></span>

<span data-ttu-id="7b543-165">Vývojářé olova: @smitpatel a @maumar</span><span class="sxs-lookup"><span data-stu-id="7b543-165">Lead developers: @smitpatel and @maumar</span></span>

<span data-ttu-id="7b543-166">Sledováno [problémy s označením `area-query` v milníku 5,0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="7b543-166">Tracked by [issues labeled with `area-query` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="7b543-167">Velikost T-tričko: XL</span><span class="sxs-lookup"><span data-stu-id="7b543-167">T-shirt size: XL</span></span>

<span data-ttu-id="7b543-168">Stav: probíhá</span><span class="sxs-lookup"><span data-stu-id="7b543-168">Status: In-progress</span></span>

<span data-ttu-id="7b543-169">Kód pro překlad dotazů byl rozsáhle přepsaný pro EF Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="7b543-169">The query translation code was extensively rewritten for EF Core 3.0.</span></span> <span data-ttu-id="7b543-170">Z tohoto důvodu je kód dotazu obecně v mnohem robustnějším stavu.</span><span class="sxs-lookup"><span data-stu-id="7b543-170">The query code is generally in a much more robust state because of this.</span></span> <span data-ttu-id="7b543-171">Pro 5,0 neplánujeme provádět velké změny dotazů, mimo ty, které jsou potřeba k podpoře TPT a přeskočení navigačních vlastností.</span><span class="sxs-lookup"><span data-stu-id="7b543-171">For 5.0 we aren't planning on making major query changes, outside those needed to support TPT and skip navigation properties.</span></span> <span data-ttu-id="7b543-172">Nicméně stále významnou práci potřebují opravit určitou technickou pohledávku, která byla ponechána v 3,0 renovaci.</span><span class="sxs-lookup"><span data-stu-id="7b543-172">However, there is still significant work needed to fix some technical debt left over from the 3.0 overhaul.</span></span> <span data-ttu-id="7b543-173">Také plánujeme opravit spoustu chyb a implementovat malá vylepšení, aby bylo možné ještě víc zlepšit celkové prostředí dotazů.</span><span class="sxs-lookup"><span data-stu-id="7b543-173">We also plan to fix many bugs and implement small enhancements to further improve the overall query experience.</span></span>

## <a name="migrations-and-deployment-experience"></a><span data-ttu-id="7b543-174">Migrace a prostředí nasazení</span><span class="sxs-lookup"><span data-stu-id="7b543-174">Migrations and deployment experience</span></span>

<span data-ttu-id="7b543-175">Vývojářé olova: @bricelam</span><span class="sxs-lookup"><span data-stu-id="7b543-175">Lead developers: @bricelam</span></span>

<span data-ttu-id="7b543-176">Sledováno [#19587](https://github.com/dotnet/efcore/issues/19587)</span><span class="sxs-lookup"><span data-stu-id="7b543-176">Tracked by [#19587](https://github.com/dotnet/efcore/issues/19587)</span></span>

<span data-ttu-id="7b543-177">Velikost trička pro T: L</span><span class="sxs-lookup"><span data-stu-id="7b543-177">T-shirt size: L</span></span>

<span data-ttu-id="7b543-178">Stav: probíhá</span><span class="sxs-lookup"><span data-stu-id="7b543-178">Status: In-progress</span></span>

<span data-ttu-id="7b543-179">V současné době mnoho vývojářů migruje své databáze při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="7b543-179">Currently, many developers migrate their databases at application startup time.</span></span> <span data-ttu-id="7b543-180">To je jednoduché, ale nedoporučuje se to z těchto důvodů:</span><span class="sxs-lookup"><span data-stu-id="7b543-180">This is easy but is not recommended because:</span></span>

* <span data-ttu-id="7b543-181">Více vláken/procesů/serverů se může pokusit o souběžnou migraci databáze</span><span class="sxs-lookup"><span data-stu-id="7b543-181">Multiple threads/processes/servers may attempt to migrate the database concurrently</span></span>
* <span data-ttu-id="7b543-182">Aplikace se můžou pokusit o přístup k nekonzistentnímu stavu, když se děje.</span><span class="sxs-lookup"><span data-stu-id="7b543-182">Applications may try to access inconsistent state while this is happening</span></span>
* <span data-ttu-id="7b543-183">Pro provádění aplikace by se obvykle neměla udělit oprávnění k databázi pro změnu schématu.</span><span class="sxs-lookup"><span data-stu-id="7b543-183">Usually the database permissions to modify the schema should not be granted for application execution</span></span>
* <span data-ttu-id="7b543-184">Vrátit zpět do čistého stavu, pokud se něco pokazilo</span><span class="sxs-lookup"><span data-stu-id="7b543-184">Its hard to revert back to a clean state if something goes wrong</span></span>

<span data-ttu-id="7b543-185">Chceme doručovat toto lepší prostředí, které umožňuje snadný způsob migrace databáze v době nasazení.</span><span class="sxs-lookup"><span data-stu-id="7b543-185">We want to deliver a better experience here that allows an easy way to migrate the database at deployment time.</span></span> <span data-ttu-id="7b543-186">To by mělo:</span><span class="sxs-lookup"><span data-stu-id="7b543-186">This should:</span></span>

* <span data-ttu-id="7b543-187">Práce v systémech Linux, Mac a Windows</span><span class="sxs-lookup"><span data-stu-id="7b543-187">Work on Linux, Mac, and Windows</span></span>
* <span data-ttu-id="7b543-188">Dobré zkušenosti s příkazovým řádkem</span><span class="sxs-lookup"><span data-stu-id="7b543-188">Be a good experience on the command line</span></span>
* <span data-ttu-id="7b543-189">Scénáře podpory s kontejnery</span><span class="sxs-lookup"><span data-stu-id="7b543-189">Support scenarios with containers</span></span>
* <span data-ttu-id="7b543-190">Práce s běžně používanými nástroji/toky pro reálné nasazení</span><span class="sxs-lookup"><span data-stu-id="7b543-190">Work with commonly used real-world deployment tools/flows</span></span>
* <span data-ttu-id="7b543-191">Integrace do alespoň sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7b543-191">Integrate into at least Visual Studio</span></span>

<span data-ttu-id="7b543-192">Výsledkem je pravděpodobně mnoho malých vylepšení EF Core (například lepší migrace na SQLite), spolu s pokyny a dlouhodobou spolupráci s ostatními týmy za účelem zlepšení kompletních prostředí, která překračují jenom EF.</span><span class="sxs-lookup"><span data-stu-id="7b543-192">The result is likely to be many small improvements in EF Core (for example, better Migrations on SQLite), together with guidance and longer-term collaborations with other teams to improve end-to-end experiences that go beyond just EF.</span></span>

## <a name="ef-core-platforms-experience"></a><span data-ttu-id="7b543-193">Prostředí pro EF Core platformy</span><span class="sxs-lookup"><span data-stu-id="7b543-193">EF Core platforms experience</span></span> 

<span data-ttu-id="7b543-194">Vývojářé olova: @roji a @bricelam</span><span class="sxs-lookup"><span data-stu-id="7b543-194">Lead developers: @roji and @bricelam</span></span>

<span data-ttu-id="7b543-195">Sledováno [#19588](https://github.com/dotnet/efcore/issues/19588)</span><span class="sxs-lookup"><span data-stu-id="7b543-195">Tracked by [#19588](https://github.com/dotnet/efcore/issues/19588)</span></span>

<span data-ttu-id="7b543-196">Velikost trička pro T: L</span><span class="sxs-lookup"><span data-stu-id="7b543-196">T-shirt size: L</span></span>

<span data-ttu-id="7b543-197">Stav: Nezahájeno</span><span class="sxs-lookup"><span data-stu-id="7b543-197">Status: Not started</span></span>

<span data-ttu-id="7b543-198">Máme dobré doprovodné materiály k používání EF Core v tradičních webových aplikacích podobných MVC.</span><span class="sxs-lookup"><span data-stu-id="7b543-198">We have good guidance for using EF Core in traditional MVC-like web applications.</span></span> <span data-ttu-id="7b543-199">Pokyny pro jiné platformy a modely aplikací buď chybí, nebo jsou zastaralé.</span><span class="sxs-lookup"><span data-stu-id="7b543-199">Guidance for other platforms and application models is either missing or out-of-date.</span></span> <span data-ttu-id="7b543-200">Pro EF Core 5,0 plánujeme prozkoumat, vylepšit a zdokumentovat prostředí používání EF Core pomocí:</span><span class="sxs-lookup"><span data-stu-id="7b543-200">For EF Core 5.0 we plan to investigate, improve, and document the experience of using EF Core with:</span></span>

* <span data-ttu-id="7b543-201">Blazor</span><span class="sxs-lookup"><span data-stu-id="7b543-201">Blazor</span></span>
* <span data-ttu-id="7b543-202">Xamarin, včetně scénáře použití AOT/linkeru</span><span class="sxs-lookup"><span data-stu-id="7b543-202">Xamarin, including using the AOT/linker story</span></span>
* <span data-ttu-id="7b543-203">WinForms/WPF/WinUI a případně jiné U.I.</span><span class="sxs-lookup"><span data-stu-id="7b543-203">WinForms/WPF/WinUI and possibly other U.I.</span></span> <span data-ttu-id="7b543-204">rozhraní</span><span class="sxs-lookup"><span data-stu-id="7b543-204">frameworks</span></span>

<span data-ttu-id="7b543-205">To je pravděpodobně mnoho malých vylepšení EF Core společně s pokyny a dlouhodobou spolupráci s ostatními týmy, aby bylo možné lépe využít ucelená prostředí, která překračují jenom EF.</span><span class="sxs-lookup"><span data-stu-id="7b543-205">This is likely to be many small improvements in EF Core, together with guidance and longer-term collaborations with other teams to improve end-to-end experiences that go beyond just EF.</span></span>

<span data-ttu-id="7b543-206">Konkrétní oblasti, na které se plánujeme podívat:</span><span class="sxs-lookup"><span data-stu-id="7b543-206">Specific areas we plan to look at are:</span></span>

* <span data-ttu-id="7b543-207">Nasazení, včetně prostředí pro použití nástrojů EF, například pro migrace</span><span class="sxs-lookup"><span data-stu-id="7b543-207">Deployment, including the experience for using EF tooling such as for Migrations</span></span>
* <span data-ttu-id="7b543-208">Modely aplikací, včetně Xamarin a Blazor, a pravděpodobně dalších</span><span class="sxs-lookup"><span data-stu-id="7b543-208">Application models, including Xamarin and Blazor, and probably others</span></span>
* <span data-ttu-id="7b543-209">Prostředí SQLite, včetně prostorového prostředí a opětovného sestavování tabulek</span><span class="sxs-lookup"><span data-stu-id="7b543-209">SQLite experiences, including the spatial experience and table rebuilds</span></span>
* <span data-ttu-id="7b543-210">AOT a propojení prostředí</span><span class="sxs-lookup"><span data-stu-id="7b543-210">AOT and linking experiences</span></span>
* <span data-ttu-id="7b543-211">Integrace diagnostiky, včetně čítačů výkonu</span><span class="sxs-lookup"><span data-stu-id="7b543-211">Diagnostics integration, including perf counters</span></span>

## <a name="performance"></a><span data-ttu-id="7b543-212">Výkon</span><span class="sxs-lookup"><span data-stu-id="7b543-212">Performance</span></span>

<span data-ttu-id="7b543-213">Vedoucí vývojář: @roji</span><span class="sxs-lookup"><span data-stu-id="7b543-213">Lead developer: @roji</span></span>

<span data-ttu-id="7b543-214">Sledováno [problémy s označením `area-perf` v milníku 5,0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="7b543-214">Tracked by [issues labeled with `area-perf` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="7b543-215">Velikost trička pro T: L</span><span class="sxs-lookup"><span data-stu-id="7b543-215">T-shirt size: L</span></span>

<span data-ttu-id="7b543-216">Stav: probíhá</span><span class="sxs-lookup"><span data-stu-id="7b543-216">Status: In-progress</span></span>

<span data-ttu-id="7b543-217">Pro EF Core plánujeme vylepšit naši sadu srovnávacích testů výkonu a zvýšit výkon na modul runtime.</span><span class="sxs-lookup"><span data-stu-id="7b543-217">For EF Core we plan to improve our suite of performance benchmarks and make directed performance improvements to the runtime.</span></span> <span data-ttu-id="7b543-218">Kromě toho plánujeme dokončit nové rozhraní API pro dávkování ADO.NET, které bylo prototypem v průběhu cyklu vydávání 3,0.</span><span class="sxs-lookup"><span data-stu-id="7b543-218">In addition, we plan to complete the new ADO.NET batching API which was prototyped during the 3.0 release cycle.</span></span> <span data-ttu-id="7b543-219">Ve vrstvě ADO.NET máme také k dispozici další vylepšení výkonu pro poskytovatele Npgsql.</span><span class="sxs-lookup"><span data-stu-id="7b543-219">Also at the ADO.NET layer, we plan additional performance improvements to the Npgsql provider.</span></span>

<span data-ttu-id="7b543-220">V rámci této práce také plánujeme podle potřeby přidat čítače výkonu ADO.NET/EF Core a další diagnostiku.</span><span class="sxs-lookup"><span data-stu-id="7b543-220">As part of this work we also plan to add ADO.NET/EF Core performance counters and other diagnostics as appropriate.</span></span>

## <a name="architecturalcontributor-documentation"></a><span data-ttu-id="7b543-221">Dokumentace k architektuře/přispěvatelům</span><span class="sxs-lookup"><span data-stu-id="7b543-221">Architectural/contributor documentation</span></span>

<span data-ttu-id="7b543-222">Vedoucí dokumentace: @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="7b543-222">Lead documenter: @ajcvickers</span></span>

<span data-ttu-id="7b543-223">Sledováno [#1920](https://github.com/aspnet/EntityFramework.Docs/issues/1920)</span><span class="sxs-lookup"><span data-stu-id="7b543-223">Tracked by [#1920](https://github.com/aspnet/EntityFramework.Docs/issues/1920)</span></span>

<span data-ttu-id="7b543-224">Velikost trička pro T: L</span><span class="sxs-lookup"><span data-stu-id="7b543-224">T-shirt size: L</span></span>

<span data-ttu-id="7b543-225">Stav: Nezahájeno</span><span class="sxs-lookup"><span data-stu-id="7b543-225">Status: Not started</span></span>

<span data-ttu-id="7b543-226">Tady je postup, který usnadňuje pochopení toho, co se v vnitřních EF Corech prochází.</span><span class="sxs-lookup"><span data-stu-id="7b543-226">The idea here is to make it easier to understand what is going on in the internals of EF Core.</span></span> <span data-ttu-id="7b543-227">To může být užitečné pro kohokoli, kdo používá EF Core, ale primární motivace usnadňuje externím lidem:</span><span class="sxs-lookup"><span data-stu-id="7b543-227">This can be useful to anyone using EF Core, but the primary motivation is to make it easier for external people to:</span></span>

* <span data-ttu-id="7b543-228">Přispívání do kódu EF Core</span><span class="sxs-lookup"><span data-stu-id="7b543-228">Contribute to the EF Core code</span></span>
* <span data-ttu-id="7b543-229">Vytvořit poskytovatele databáze</span><span class="sxs-lookup"><span data-stu-id="7b543-229">Create database providers</span></span>
* <span data-ttu-id="7b543-230">Sestavení dalších rozšíření</span><span class="sxs-lookup"><span data-stu-id="7b543-230">Build other extensions</span></span>

## <a name="microsoftdatasqlite-documentation"></a><span data-ttu-id="7b543-231">Dokumentace k Microsoft. data. sqlite</span><span class="sxs-lookup"><span data-stu-id="7b543-231">Microsoft.Data.Sqlite documentation</span></span>

<span data-ttu-id="7b543-232">Vedoucí dokumentace: @bricelam</span><span class="sxs-lookup"><span data-stu-id="7b543-232">Lead documenter: @bricelam</span></span>

<span data-ttu-id="7b543-233">Sledováno [#1675](https://github.com/aspnet/EntityFramework.Docs/issues/1675)</span><span class="sxs-lookup"><span data-stu-id="7b543-233">Tracked by [#1675](https://github.com/aspnet/EntityFramework.Docs/issues/1675)</span></span>

<span data-ttu-id="7b543-234">Velikost T-tričko: M</span><span class="sxs-lookup"><span data-stu-id="7b543-234">T-shirt size: M</span></span>

<span data-ttu-id="7b543-235">Stav: dokončeno.</span><span class="sxs-lookup"><span data-stu-id="7b543-235">Status: Completed.</span></span> <span data-ttu-id="7b543-236">Nová dokumentace je [živá na webu Microsoft docs](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli).</span><span class="sxs-lookup"><span data-stu-id="7b543-236">The new documentation is [live on the Microsoft docs site](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli).</span></span>

<span data-ttu-id="7b543-237">Tým EF také vlastní poskytovatele Microsoft. data. sqlite ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="7b543-237">The EF Team also owns the Microsoft.Data.Sqlite ADO.NET provider.</span></span> <span data-ttu-id="7b543-238">V rámci vydání 5,0 plánujeme plně zdokumentovat tohoto poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="7b543-238">We plan to fully document this provider as part of the 5.0 release.</span></span>

## <a name="general-documentation"></a><span data-ttu-id="7b543-239">Obecná dokumentace</span><span class="sxs-lookup"><span data-stu-id="7b543-239">General documentation</span></span>

<span data-ttu-id="7b543-240">Vedoucí dokumentace: @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="7b543-240">Lead documenter: @ajcvickers</span></span>

<span data-ttu-id="7b543-241">Sledováno [problémy v úložišti dokumentů v milníku 5,0](https://github.com/aspnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="7b543-241">Tracked by [issues in the docs repo in the 5.0 milestone](https://github.com/aspnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="7b543-242">Velikost trička pro T: L</span><span class="sxs-lookup"><span data-stu-id="7b543-242">T-shirt size: L</span></span>

<span data-ttu-id="7b543-243">Stav: probíhá</span><span class="sxs-lookup"><span data-stu-id="7b543-243">Status: In-progress</span></span>

<span data-ttu-id="7b543-244">V průběhu aktualizace dokumentace pro vydání 3,0 a 3,1 už v dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="7b543-244">We are already in the process of updating documentation for the 3.0 and 3.1 releases.</span></span> <span data-ttu-id="7b543-245">Pracujeme také na:</span><span class="sxs-lookup"><span data-stu-id="7b543-245">We are also working on:</span></span>
  * <span data-ttu-id="7b543-246">Změna dokumentů Začínáme, aby byly lépe přístupné/snazší</span><span class="sxs-lookup"><span data-stu-id="7b543-246">An overhaul of the getting started docs to make them more approachable/easier to follow</span></span>
  * <span data-ttu-id="7b543-247">Reorganizace dokumentů, aby bylo snazší najít a přidat křížové odkazy</span><span class="sxs-lookup"><span data-stu-id="7b543-247">Reorganization of docs to make things easier to find and to add cross-references</span></span>
  * <span data-ttu-id="7b543-248">Přidání dalších podrobností a objasnění stávajících dokumentů</span><span class="sxs-lookup"><span data-stu-id="7b543-248">Adding more details and clarifications to existing docs</span></span>
  * <span data-ttu-id="7b543-249">Aktualizace ukázek a přidání dalších příkladů</span><span class="sxs-lookup"><span data-stu-id="7b543-249">Updating the samples and adding more examples</span></span>

## <a name="fixing-bugs"></a><span data-ttu-id="7b543-250">Oprava chyb</span><span class="sxs-lookup"><span data-stu-id="7b543-250">Fixing bugs</span></span>

<span data-ttu-id="7b543-251">Sledováno [problémy s označením `type-bug` v milníku 5,0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)</span><span class="sxs-lookup"><span data-stu-id="7b543-251">Tracked by [issues labeled with `type-bug` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)</span></span>

<span data-ttu-id="7b543-252">Vývojáři: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="7b543-252">Developers: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span></span>

<span data-ttu-id="7b543-253">Velikost trička pro T: L</span><span class="sxs-lookup"><span data-stu-id="7b543-253">T-shirt size: L</span></span>

<span data-ttu-id="7b543-254">Stav: probíhá</span><span class="sxs-lookup"><span data-stu-id="7b543-254">Status: In-progress</span></span>

<span data-ttu-id="7b543-255">V době psaní máme 135 chyb, které jsou posouzené jako opravené ve verzi 5,0 (s již opravenou 62), ale výrazně se překrývají s výše uvedeným _obecným vylepšením dotazů_ .</span><span class="sxs-lookup"><span data-stu-id="7b543-255">At the time of writing, we have 135 bugs triaged to be fixed in the 5.0 release (with 62 already fixed), but there is significant overlap with the _General query enhancements_ section above.</span></span>

<span data-ttu-id="7b543-256">Příchozí rychlost (problémy, které končí jako práce v milníku), probíhala přibližně 23 problémů za měsíc v průběhu vydání 3,0.</span><span class="sxs-lookup"><span data-stu-id="7b543-256">The incoming rate (issues that end up as work in a milestone) was about 23 issues per month over the course of the 3.0 release.</span></span> <span data-ttu-id="7b543-257">Ne všechny tyto akce budou muset být opraveny v 5,0.</span><span class="sxs-lookup"><span data-stu-id="7b543-257">Not all of these will need to be fixed in 5.0.</span></span> <span data-ttu-id="7b543-258">V rámci hrubého odhadu plánujeme vyřešit další 150 problémů v časovém intervalu 5,0.</span><span class="sxs-lookup"><span data-stu-id="7b543-258">As a rough estimate we plan to fix an additional 150 issues in the 5.0 time frame.</span></span>

## <a name="small-enhancements"></a><span data-ttu-id="7b543-259">Malá vylepšení</span><span class="sxs-lookup"><span data-stu-id="7b543-259">Small enhancements</span></span>

<span data-ttu-id="7b543-260">Sledováno [problémy s označením `type-enhancement` v milníku 5,0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)</span><span class="sxs-lookup"><span data-stu-id="7b543-260">Tracked by [issues labeled with `type-enhancement` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)</span></span>

<span data-ttu-id="7b543-261">Vývojáři: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="7b543-261">Developers: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span></span>

<span data-ttu-id="7b543-262">Velikost trička pro T: L</span><span class="sxs-lookup"><span data-stu-id="7b543-262">T-shirt size: L</span></span>

<span data-ttu-id="7b543-263">Stav: probíhá</span><span class="sxs-lookup"><span data-stu-id="7b543-263">Status: In-progress</span></span>

<span data-ttu-id="7b543-264">Kromě větších funkcí uvedených výše je také k dispozici mnoho menších vylepšení naplánovaných na 5,0, aby bylo možné opravit "papírové řezy".</span><span class="sxs-lookup"><span data-stu-id="7b543-264">In addition to the bigger features outlined above, we also have many smaller improvements scheduled for 5.0 to fix "paper-cuts".</span></span> <span data-ttu-id="7b543-265">Všimněte si, že mnohé z těchto vylepšení se vztahují i na obecnější motivy popsané výše.</span><span class="sxs-lookup"><span data-stu-id="7b543-265">Note that many of these enhancements are also covered by the more general themes outlined above.</span></span>

## <a name="below-the-line"></a><span data-ttu-id="7b543-266">Pod řádkem</span><span class="sxs-lookup"><span data-stu-id="7b543-266">Below-the-line</span></span>

<span data-ttu-id="7b543-267">Sledováno [problémy s označením `consider-for-next-release`](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)</span><span class="sxs-lookup"><span data-stu-id="7b543-267">Tracked by [issues labeled with `consider-for-next-release`](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)</span></span>

<span data-ttu-id="7b543-268">Jedná se o opravy chyb a vylepšení, která **nejsou aktuálně** naplánovaná pro vydání 5,0, ale budeme se považovat za ambiciózní cíle v závislosti na pokroku v práci.</span><span class="sxs-lookup"><span data-stu-id="7b543-268">These are bug fixes and enhancements that are **not** currently scheduled for the 5.0 release, but we will look at as stretch goals depending on the progress made on the work above.</span></span>

<span data-ttu-id="7b543-269">Kromě toho vždy vybereme [nejbezpečnější problémy](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) při plánování.</span><span class="sxs-lookup"><span data-stu-id="7b543-269">In addition, we always consider the [most voted issues](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) when planning.</span></span> <span data-ttu-id="7b543-270">Vyjmutí všech těchto problémů z verze je vždycky bolestivý, ale pro tyto prostředky potřebujeme realističtější plán.</span><span class="sxs-lookup"><span data-stu-id="7b543-270">Cutting any of these issues from a release is always painful, but we do need a realistic plan for the resources we have.</span></span>

## <a name="feedback"></a><span data-ttu-id="7b543-271">Názor</span><span class="sxs-lookup"><span data-stu-id="7b543-271">Feedback</span></span>

<span data-ttu-id="7b543-272">Váš názor na plánování je důležitý.</span><span class="sxs-lookup"><span data-stu-id="7b543-272">Your feedback on planning is important.</span></span> <span data-ttu-id="7b543-273">Nejlepším způsobem, jak určit důležitost problému, je hlasovat (palec) pro daný problém na GitHubu.</span><span class="sxs-lookup"><span data-stu-id="7b543-273">The best way to indicate the importance of an issue is to vote (thumbs-up) for that issue on GitHub.</span></span> <span data-ttu-id="7b543-274">Tato data se pak budou předávat do [procesu plánování](../release-planning.md) pro další vydání.</span><span class="sxs-lookup"><span data-stu-id="7b543-274">This data will then feed into the [planning process](../release-planning.md) for the next release.</span></span>
