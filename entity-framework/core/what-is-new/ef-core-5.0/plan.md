---
title: Plán pro Entity Framework Core 5,0
author: ajcvickers
ms.date: 01/14/2020
uid: core/what-is-new/ef-core-5.0/plan.md
ms.openlocfilehash: 8b4ca32524869019c04d5a4d4d55967f68181cd7
ms.sourcegitcommit: c3b8386071d64953ee68788ef9d951144881a6ab
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/24/2020
ms.locfileid: "80136221"
---
# <a name="plan-for-entity-framework-core-50"></a><span data-ttu-id="246a9-102">Plán pro Entity Framework Core 5,0</span><span class="sxs-lookup"><span data-stu-id="246a9-102">Plan for Entity Framework Core 5.0</span></span>

<span data-ttu-id="246a9-103">Jak je popsáno v [procesu plánování](../release-planning.md), shromáždili jsme vstup od zúčastněných stran do předběžného plánu pro vydání verze EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="246a9-103">As described in the [planning process](../release-planning.md), we have gathered input from stakeholders into a tentative plan for the EF Core 5.0 release.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="246a9-104">Tento plán je stále v průběhu práce.</span><span class="sxs-lookup"><span data-stu-id="246a9-104">This plan is still a work-in-progress.</span></span> <span data-ttu-id="246a9-105">Tady není žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="246a9-105">Nothing here is a commitment.</span></span> <span data-ttu-id="246a9-106">Tento plán je výchozím bodem, který se bude vyvíjet, jakmile se dozvíte víc.</span><span class="sxs-lookup"><span data-stu-id="246a9-106">This plan is a starting point that will evolve as we learn more.</span></span> <span data-ttu-id="246a9-107">Některé věci, které nejsou aktuálně plánovány pro 5,0, mohou být získány v.</span><span class="sxs-lookup"><span data-stu-id="246a9-107">Some things not currently planned for 5.0 may get pulled in.</span></span> <span data-ttu-id="246a9-108">Některé věci, které jsou aktuálně plánovány pro 5,0, mohou punted.</span><span class="sxs-lookup"><span data-stu-id="246a9-108">Some things currently planned for 5.0 may get punted out.</span></span>

### <a name="version-number-and-release-date"></a><span data-ttu-id="246a9-109">Číslo verze a datum vydání</span><span class="sxs-lookup"><span data-stu-id="246a9-109">Version number and release date.</span></span>

<span data-ttu-id="246a9-110">Verze EF Core 5,0 je aktuálně naplánována na vydání ve [stejnou dobu jako rozhraní .net 5,0](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span><span class="sxs-lookup"><span data-stu-id="246a9-110">EF Core 5.0 is currently scheduled for release at [the same time as .NET 5.0](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span></span> <span data-ttu-id="246a9-111">Verze "5,0" byla zvolena pro zarovnání s .NET 5,0.</span><span class="sxs-lookup"><span data-stu-id="246a9-111">The version "5.0" was chosen to align with .NET 5.0.</span></span>

### <a name="supported-platforms"></a><span data-ttu-id="246a9-112">Podporované platformy</span><span class="sxs-lookup"><span data-stu-id="246a9-112">Supported platforms</span></span>

<span data-ttu-id="246a9-113">EF Core 5,0 se plánuje běžet na jakékoli platformě .NET 5,0 na základě [sblížení těchto platforem až po .NET Core](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span><span class="sxs-lookup"><span data-stu-id="246a9-113">EF Core 5.0 is planned to run on any .NET 5.0 platform based on the [convergence of these platforms to .NET Core](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span></span> <span data-ttu-id="246a9-114">To znamená, že v souvislosti s .NET Standard se stále definuje vlastní použité TFM.</span><span class="sxs-lookup"><span data-stu-id="246a9-114">What this means in terms of .NET Standard and the actual TFM used is still TBD.</span></span>

<span data-ttu-id="246a9-115">EF Core 5,0 se na .NET Framework nespustí.</span><span class="sxs-lookup"><span data-stu-id="246a9-115">EF Core 5.0 will not run on .NET Framework.</span></span>

### <a name="breaking-changes"></a><span data-ttu-id="246a9-116">Změny způsobující chyby</span><span class="sxs-lookup"><span data-stu-id="246a9-116">Breaking changes</span></span>

<span data-ttu-id="246a9-117">EF Core 5,0 bude obsahovat některé zásadní změny, ale bude to mnohem méně závažné, než bylo v případě EF Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="246a9-117">EF Core 5.0 will contain some breaking changes, but these will be much less severe than was the case for EF Core 3.0.</span></span> <span data-ttu-id="246a9-118">Naším cílem je dovolit, aby se velká většina aplikací aktualizovala bez přerušení.</span><span class="sxs-lookup"><span data-stu-id="246a9-118">Our goal is to allow the vast majority of applications to update without breaking.</span></span>

<span data-ttu-id="246a9-119">Očekává se, že pro poskytovatele databáze budou zásadní změny, zejména pokud jde o podporu TPT.</span><span class="sxs-lookup"><span data-stu-id="246a9-119">It is expected that there will be some breaking changes for database providers, especially around TPT support.</span></span> <span data-ttu-id="246a9-120">Očekáváme ale, že práce, která aktualizuje zprostředkovatele pro 5,0, bude menší, než se vyžaduje aktualizace pro 3,0.</span><span class="sxs-lookup"><span data-stu-id="246a9-120">However, we expect the work to update a provider for 5.0 will be less than was required to update for 3.0.</span></span>

## <a name="themes"></a><span data-ttu-id="246a9-121">Motivy</span><span class="sxs-lookup"><span data-stu-id="246a9-121">Themes</span></span>

<span data-ttu-id="246a9-122">Extrahovali jsme několik hlavních oblastí nebo motivů, které budou tvořit základ pro velké investice do EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="246a9-122">We have extracted a few major areas or themes which will form the basis for the large investments in EF Core 5.0.</span></span>

## <a name="many-to-many-navigation-properties-aka-skip-navigations"></a><span data-ttu-id="246a9-123">Navigační vlastnosti m:n (a. k) pro přeskočení navigace (a. k)</span><span class="sxs-lookup"><span data-stu-id="246a9-123">Many-to-many navigation properties (a.k.a "skip navigations")</span></span>

<span data-ttu-id="246a9-124">Vývojářé olova: @smitpatel a @AndriySvyryd</span><span class="sxs-lookup"><span data-stu-id="246a9-124">Lead developers: @smitpatel and @AndriySvyryd</span></span>

<span data-ttu-id="246a9-125">Sledováno [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)</span><span class="sxs-lookup"><span data-stu-id="246a9-125">Tracked by [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)</span></span>

<span data-ttu-id="246a9-126">Velikost trička pro T: L</span><span class="sxs-lookup"><span data-stu-id="246a9-126">T-shirt size: L</span></span>

<span data-ttu-id="246a9-127">Stav: probíhá</span><span class="sxs-lookup"><span data-stu-id="246a9-127">Status: In-progress</span></span>

<span data-ttu-id="246a9-128">Na nevyřízených položkách GitHubu je [nejužitečnější funkce](https://github.com/aspnet/EntityFrameworkCore/issues/1368) m:n (přibližně 407 hlasy).</span><span class="sxs-lookup"><span data-stu-id="246a9-128">Many-to-many is the [most requested feature](https://github.com/aspnet/EntityFrameworkCore/issues/1368) (~407 votes) on the GitHub backlog.</span></span>

<span data-ttu-id="246a9-129">Podpora vztahů n:n v celém rozsahu je sledována jako [#10508](https://github.com/aspnet/EntityFrameworkCore/issues/10508).</span><span class="sxs-lookup"><span data-stu-id="246a9-129">Support for many-to-many relationships in their entirety is tracked as [#10508](https://github.com/aspnet/EntityFrameworkCore/issues/10508).</span></span> <span data-ttu-id="246a9-130">To může být rozdělené na tři hlavní oblasti:</span><span class="sxs-lookup"><span data-stu-id="246a9-130">This can be broken down into three major areas:</span></span>

* <span data-ttu-id="246a9-131">Přeskočit navigační vlastnosti</span><span class="sxs-lookup"><span data-stu-id="246a9-131">Skip navigation properties.</span></span> <span data-ttu-id="246a9-132">To umožňuje, aby byl model použit pro dotazy atd. bez odkazů na podkladovou entitu JOIN Table.</span><span class="sxs-lookup"><span data-stu-id="246a9-132">These allow the model to be used for queries, etc. without reference to the underlying join table entity.</span></span> <span data-ttu-id="246a9-133">([#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003))</span><span class="sxs-lookup"><span data-stu-id="246a9-133">([#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003))</span></span>
* <span data-ttu-id="246a9-134">Typy entit kontejneru vlastností.</span><span class="sxs-lookup"><span data-stu-id="246a9-134">Property-bag entity types.</span></span> <span data-ttu-id="246a9-135">Ty umožňují použití standardního typu CLR (např. `Dictionary`) pro instance entit tak, že pro každý typ entity není vyžadován explicitní typ CLR.</span><span class="sxs-lookup"><span data-stu-id="246a9-135">These allow a standard CLR type (e.g. `Dictionary`) to be used for entity instances such that an explicit CLR type is not needed for each entity type.</span></span> <span data-ttu-id="246a9-136">(Stretch for 5,0: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914).)</span><span class="sxs-lookup"><span data-stu-id="246a9-136">(Stretch for 5.0: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914).)</span></span>
* <span data-ttu-id="246a9-137">Cukr pro jednoduchou konfiguraci relací m:n.</span><span class="sxs-lookup"><span data-stu-id="246a9-137">Sugar for easy configuration of many-to-many relationships.</span></span> <span data-ttu-id="246a9-138">(Stretch for 5,0.)</span><span class="sxs-lookup"><span data-stu-id="246a9-138">(Stretch for 5.0.)</span></span>

<span data-ttu-id="246a9-139">Věříme, že nejvýznamnější blokování pro ty, kteří vyžadují podporu m:n, nedokáže používat "přirozené" vztahy, a to bez odkazování na tabulku spojení v obchodní logice, jako jsou dotazy.</span><span class="sxs-lookup"><span data-stu-id="246a9-139">We believe that the most significant blocker for those wanting many-to-many support is not being able to use the "natural" relationships, without referring to the join table, in business logic such as queries.</span></span> <span data-ttu-id="246a9-140">Typ entity tabulky join může stále existovat, ale neměl by se zobrazovat v cestě k obchodní logice.</span><span class="sxs-lookup"><span data-stu-id="246a9-140">The join table entity type may still exist, but it should not get in the way of business logic.</span></span> <span data-ttu-id="246a9-141">To je důvod, proč jsme se rozhodli Přeskočit navigační vlastnosti pro 5,0.</span><span class="sxs-lookup"><span data-stu-id="246a9-141">This is why we have chosen to tackle skip navigation properties for 5.0.</span></span>

<span data-ttu-id="246a9-142">V tuto chvíli se pro EF Core 5,0 sledují i další části m:n.</span><span class="sxs-lookup"><span data-stu-id="246a9-142">At this time the other parts of many-to-many are being pursued as a stretch goal for EF Core 5.0.</span></span> <span data-ttu-id="246a9-143">To znamená, že nejsou v současnosti v plánu pro 5,0, ale pokud se o to dobře myslí, doufáme, že si je vyžádáme.</span><span class="sxs-lookup"><span data-stu-id="246a9-143">This means they are not currently in the plan for 5.0, but if things go well we hope to pull them in.</span></span>

## <a name="table-per-type-tpt-inheritance-mapping"></a><span data-ttu-id="246a9-144">Mapování dědičnosti typu tabulka na typ (TPT)</span><span class="sxs-lookup"><span data-stu-id="246a9-144">Table-per-type (TPT) inheritance mapping</span></span>

<span data-ttu-id="246a9-145">Vedoucí vývojář: @AndriySvyryd</span><span class="sxs-lookup"><span data-stu-id="246a9-145">Lead developer: @AndriySvyryd</span></span>

<span data-ttu-id="246a9-146">Sledováno [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)</span><span class="sxs-lookup"><span data-stu-id="246a9-146">Tracked by [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)</span></span>

<span data-ttu-id="246a9-147">Velikost T-tričko: XL</span><span class="sxs-lookup"><span data-stu-id="246a9-147">T-shirt size: XL</span></span>

<span data-ttu-id="246a9-148">Stav: probíhá</span><span class="sxs-lookup"><span data-stu-id="246a9-148">Status: In-progress</span></span>

<span data-ttu-id="246a9-149">Provádíme TPT, protože se jedná o vysoce požadovanou funkci (~ 254 hlasy, celkově třetí) a protože vyžaduje některé změny nízké úrovně, které jsou vhodné pro základní povahu celkového plánu .NET 5.</span><span class="sxs-lookup"><span data-stu-id="246a9-149">We're doing TPT because it is both a highly requested feature (~254 votes; 3rd overall) and because it requires some low-level changes that we feel are appropriate for the foundational nature of the overall .NET 5 plan.</span></span> <span data-ttu-id="246a9-150">Očekáváme, že dojde k zásadním změnám u zprostředkovatelů databází, i když by měly být mnohem méně závažné, než změny požadované pro 3,0.</span><span class="sxs-lookup"><span data-stu-id="246a9-150">We expect this to result in breaking changes for database providers, although these should be much less severe than the changes required for 3.0.</span></span>

## <a name="filtered-include"></a><span data-ttu-id="246a9-151">Filtrované zahrnutí</span><span class="sxs-lookup"><span data-stu-id="246a9-151">Filtered Include</span></span>

<span data-ttu-id="246a9-152">Vedoucí vývojář: @maumar</span><span class="sxs-lookup"><span data-stu-id="246a9-152">Lead developer: @maumar</span></span>

<span data-ttu-id="246a9-153">Sledováno [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)</span><span class="sxs-lookup"><span data-stu-id="246a9-153">Tracked by [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)</span></span>

<span data-ttu-id="246a9-154">Velikost T-tričko: M</span><span class="sxs-lookup"><span data-stu-id="246a9-154">T-shirt size: M</span></span>

<span data-ttu-id="246a9-155">Stav: probíhá</span><span class="sxs-lookup"><span data-stu-id="246a9-155">Status: In-progress</span></span>

<span data-ttu-id="246a9-156">Filtrované zahrnutí je vysoce vyžádaná funkce (~ 317 hlasy, druhá celková), která není obrovským objemem práce a že se domníváte, že bude odblokovat nebo dělat jednodušší mnoho scénářů, které aktuálně vyžadují filtry na úrovni modelu nebo složitější dotazy.</span><span class="sxs-lookup"><span data-stu-id="246a9-156">Filtered Include is a highly-requested feature (~317 votes; 2nd overall) that isn't a huge amount of work, and that we believe will unblock or make easier many scenarios that currently require model-level filters or more complex queries.</span></span>

## <a name="rationalize-totable-toquery-toview-fromsql-etc"></a><span data-ttu-id="246a9-157">Racionalizovat ToTable, ToQuery, ToView, Z tabulek atd.</span><span class="sxs-lookup"><span data-stu-id="246a9-157">Rationalize ToTable, ToQuery, ToView, FromSql, etc.</span></span>

<span data-ttu-id="246a9-158">Vývojářé olova: @maumar a @smitpatel</span><span class="sxs-lookup"><span data-stu-id="246a9-158">Lead developers: @maumar and @smitpatel</span></span>

<span data-ttu-id="246a9-159">Sledováno [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)</span><span class="sxs-lookup"><span data-stu-id="246a9-159">Tracked by [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)</span></span>

<span data-ttu-id="246a9-160">Velikost trička pro T: L</span><span class="sxs-lookup"><span data-stu-id="246a9-160">T-shirt size: L</span></span>

<span data-ttu-id="246a9-161">Stav: probíhá</span><span class="sxs-lookup"><span data-stu-id="246a9-161">Status: In-progress</span></span>

<span data-ttu-id="246a9-162">V předchozích verzích jsme udělali postup k podpoře nezpracovaných SQL, bez klíčůch typů a souvisejících oblastí.</span><span class="sxs-lookup"><span data-stu-id="246a9-162">We have made progress in previous releases towards supporting raw SQL, keyless types, and related areas.</span></span> <span data-ttu-id="246a9-163">Existují však mezery i nekonzistence, jak vše funguje dohromady jako celek.</span><span class="sxs-lookup"><span data-stu-id="246a9-163">However, there are both gaps and inconsistencies in the way everything works together as a whole.</span></span> <span data-ttu-id="246a9-164">Cílem 5,0 je opravit tyto možnosti a vytvořit dobré prostředí pro definování, migraci a používání různých typů entit a jejich přidružených dotazů a artefaktů databáze.</span><span class="sxs-lookup"><span data-stu-id="246a9-164">The goal for 5.0 is to fix these and create a good experience for defining, migrating, and using different types of entities and their associated queries and database artifacts.</span></span> <span data-ttu-id="246a9-165">To může také zahrnovat aktualizace zkompilovaného rozhraní API pro dotazy.</span><span class="sxs-lookup"><span data-stu-id="246a9-165">This may also involve updates to the compiled query API.</span></span>

<span data-ttu-id="246a9-166">Všimněte si, že tato položka může vést k nějakým změnám na úrovni aplikace, protože některé z funkcí, které aktuálně máme, jsou moc opravňující, takže můžou rychle vést k pitsi selhání.</span><span class="sxs-lookup"><span data-stu-id="246a9-166">Note that this item may result in some application-level breaking changes since some of the functionality we currently have is too permissive such that it can quickly lead people into pits of failure.</span></span> <span data-ttu-id="246a9-167">Budeme nejspíš zablokovat některé z těchto funkcí společně s pokyny k tomu, co dělat.</span><span class="sxs-lookup"><span data-stu-id="246a9-167">We will likely end up blocking some of this functionality together with guidance on what to do instead.</span></span>

## <a name="general-query-enhancements"></a><span data-ttu-id="246a9-168">Obecná vylepšení dotazů</span><span class="sxs-lookup"><span data-stu-id="246a9-168">General query enhancements</span></span>

<span data-ttu-id="246a9-169">Vývojářé olova: @smitpatel a @maumar</span><span class="sxs-lookup"><span data-stu-id="246a9-169">Lead developers: @smitpatel and @maumar</span></span>

<span data-ttu-id="246a9-170">Sledováno [problémy s označením `area-query` v milníku 5,0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="246a9-170">Tracked by [issues labeled with `area-query` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="246a9-171">Velikost T-tričko: XL</span><span class="sxs-lookup"><span data-stu-id="246a9-171">T-shirt size: XL</span></span>

<span data-ttu-id="246a9-172">Stav: probíhá</span><span class="sxs-lookup"><span data-stu-id="246a9-172">Status: In-progress</span></span>

<span data-ttu-id="246a9-173">Kód pro překlad dotazů byl rozsáhle přepsaný pro EF Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="246a9-173">The query translation code was extensively rewritten for EF Core 3.0.</span></span> <span data-ttu-id="246a9-174">Z tohoto důvodu je kód dotazu obecně v mnohem robustnějším stavu.</span><span class="sxs-lookup"><span data-stu-id="246a9-174">The query code is generally in a much more robust state because of this.</span></span> <span data-ttu-id="246a9-175">Pro 5,0 neplánujeme provádět velké změny dotazů, mimo ty, které jsou potřeba k podpoře TPT a přeskočení navigačních vlastností.</span><span class="sxs-lookup"><span data-stu-id="246a9-175">For 5.0 we aren't planning on making major query changes, outside those needed to support TPT and skip navigation properties.</span></span> <span data-ttu-id="246a9-176">Nicméně stále významnou práci potřebují opravit určitou technickou pohledávku, která byla ponechána v 3,0 renovaci.</span><span class="sxs-lookup"><span data-stu-id="246a9-176">However, there is still significant work needed to fix some technical debt left over from the 3.0 overhaul.</span></span> <span data-ttu-id="246a9-177">Také plánujeme opravit spoustu chyb a implementovat malá vylepšení, aby bylo možné ještě víc zlepšit celkové prostředí dotazů.</span><span class="sxs-lookup"><span data-stu-id="246a9-177">We also plan to fix many bugs and implement small enhancements to further improve the overall query experience.</span></span>

## <a name="migrations-and-deployment-experience"></a><span data-ttu-id="246a9-178">Migrace a prostředí nasazení</span><span class="sxs-lookup"><span data-stu-id="246a9-178">Migrations and deployment experience</span></span>

<span data-ttu-id="246a9-179">Vývojářé olova: @bricelam</span><span class="sxs-lookup"><span data-stu-id="246a9-179">Lead developers: @bricelam</span></span>

<span data-ttu-id="246a9-180">Sledováno [#19587](https://github.com/dotnet/efcore/issues/19587)</span><span class="sxs-lookup"><span data-stu-id="246a9-180">Tracked by [#19587](https://github.com/dotnet/efcore/issues/19587)</span></span>

<span data-ttu-id="246a9-181">Velikost trička pro T: L</span><span class="sxs-lookup"><span data-stu-id="246a9-181">T-shirt size: L</span></span>

<span data-ttu-id="246a9-182">Stav: probíhá</span><span class="sxs-lookup"><span data-stu-id="246a9-182">Status: In-progress</span></span>

<span data-ttu-id="246a9-183">V současné době mnoho vývojářů migruje své databáze při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="246a9-183">Currently, many developers migrate their databases at application startup time.</span></span> <span data-ttu-id="246a9-184">To je jednoduché, ale nedoporučuje se to z těchto důvodů:</span><span class="sxs-lookup"><span data-stu-id="246a9-184">This is easy but is not recommended because:</span></span>

* <span data-ttu-id="246a9-185">Více vláken/procesů/serverů se může pokusit o souběžnou migraci databáze</span><span class="sxs-lookup"><span data-stu-id="246a9-185">Multiple threads/processes/servers may attempt to migrate the database concurrently</span></span>
* <span data-ttu-id="246a9-186">Aplikace se můžou pokusit o přístup k nekonzistentnímu stavu, když se děje.</span><span class="sxs-lookup"><span data-stu-id="246a9-186">Applications may try to access inconsistent state while this is happening</span></span>
* <span data-ttu-id="246a9-187">Pro provádění aplikace by se obvykle neměla udělit oprávnění k databázi pro změnu schématu.</span><span class="sxs-lookup"><span data-stu-id="246a9-187">Usually the database permissions to modify the schema should not be granted for application execution</span></span>
* <span data-ttu-id="246a9-188">Vrátit se zpátky do čistého stavu, pokud se něco pokazilo</span><span class="sxs-lookup"><span data-stu-id="246a9-188">It's hard to revert back to a clean state if something goes wrong</span></span>

<span data-ttu-id="246a9-189">Chceme doručovat toto lepší prostředí, které umožňuje snadný způsob migrace databáze v době nasazení.</span><span class="sxs-lookup"><span data-stu-id="246a9-189">We want to deliver a better experience here that allows an easy way to migrate the database at deployment time.</span></span> <span data-ttu-id="246a9-190">To by mělo:</span><span class="sxs-lookup"><span data-stu-id="246a9-190">This should:</span></span>

* <span data-ttu-id="246a9-191">Práce v systémech Linux, Mac a Windows</span><span class="sxs-lookup"><span data-stu-id="246a9-191">Work on Linux, Mac, and Windows</span></span>
* <span data-ttu-id="246a9-192">Dobré zkušenosti s příkazovým řádkem</span><span class="sxs-lookup"><span data-stu-id="246a9-192">Be a good experience on the command line</span></span>
* <span data-ttu-id="246a9-193">Scénáře podpory s kontejnery</span><span class="sxs-lookup"><span data-stu-id="246a9-193">Support scenarios with containers</span></span>
* <span data-ttu-id="246a9-194">Práce s běžně používanými nástroji/toky pro reálné nasazení</span><span class="sxs-lookup"><span data-stu-id="246a9-194">Work with commonly used real-world deployment tools/flows</span></span>
* <span data-ttu-id="246a9-195">Integrace do alespoň sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="246a9-195">Integrate into at least Visual Studio</span></span>

<span data-ttu-id="246a9-196">Výsledkem je pravděpodobně mnoho malých vylepšení EF Core (například lepší migrace na SQLite), spolu s pokyny a dlouhodobou spolupráci s ostatními týmy za účelem zlepšení kompletních prostředí, která překračují jenom EF.</span><span class="sxs-lookup"><span data-stu-id="246a9-196">The result is likely to be many small improvements in EF Core (for example, better Migrations on SQLite), together with guidance and longer-term collaborations with other teams to improve end-to-end experiences that go beyond just EF.</span></span>

## <a name="ef-core-platforms-experience"></a><span data-ttu-id="246a9-197">Prostředí pro EF Core platformy</span><span class="sxs-lookup"><span data-stu-id="246a9-197">EF Core platforms experience</span></span> 

<span data-ttu-id="246a9-198">Vývojářé olova: @roji a @bricelam</span><span class="sxs-lookup"><span data-stu-id="246a9-198">Lead developers: @roji and @bricelam</span></span>

<span data-ttu-id="246a9-199">Sledováno [#19588](https://github.com/dotnet/efcore/issues/19588)</span><span class="sxs-lookup"><span data-stu-id="246a9-199">Tracked by [#19588](https://github.com/dotnet/efcore/issues/19588)</span></span>

<span data-ttu-id="246a9-200">Velikost trička pro T: L</span><span class="sxs-lookup"><span data-stu-id="246a9-200">T-shirt size: L</span></span>

<span data-ttu-id="246a9-201">Stav: Nezahájeno</span><span class="sxs-lookup"><span data-stu-id="246a9-201">Status: Not started</span></span>

<span data-ttu-id="246a9-202">Máme dobré doprovodné materiály k používání EF Core v tradičních webových aplikacích podobných MVC.</span><span class="sxs-lookup"><span data-stu-id="246a9-202">We have good guidance for using EF Core in traditional MVC-like web applications.</span></span> <span data-ttu-id="246a9-203">Pokyny pro jiné platformy a modely aplikací buď chybí, nebo jsou zastaralé.</span><span class="sxs-lookup"><span data-stu-id="246a9-203">Guidance for other platforms and application models is either missing or out-of-date.</span></span> <span data-ttu-id="246a9-204">V případě EF Core 5,0 plánujeme prozkoumat, vylepšit a zdokumentovat prostředí pro používání EF Core pomocí těchto možností:</span><span class="sxs-lookup"><span data-stu-id="246a9-204">For EF Core 5.0, we plan to investigate, improve, and document the experience of using EF Core with:</span></span>

* <span data-ttu-id="246a9-205">Blazor</span><span class="sxs-lookup"><span data-stu-id="246a9-205">Blazor</span></span>
* <span data-ttu-id="246a9-206">Xamarin, včetně scénáře použití AOT/linkeru</span><span class="sxs-lookup"><span data-stu-id="246a9-206">Xamarin, including using the AOT/linker story</span></span>
* <span data-ttu-id="246a9-207">WinForms/WPF/WinUI a případně jiné U.I.</span><span class="sxs-lookup"><span data-stu-id="246a9-207">WinForms/WPF/WinUI and possibly other U.I.</span></span> <span data-ttu-id="246a9-208">rozhraní</span><span class="sxs-lookup"><span data-stu-id="246a9-208">frameworks</span></span>

<span data-ttu-id="246a9-209">To je pravděpodobně mnoho malých vylepšení EF Core společně s pokyny a dlouhodobou spolupráci s ostatními týmy, aby bylo možné lépe využít ucelená prostředí, která překračují jenom EF.</span><span class="sxs-lookup"><span data-stu-id="246a9-209">This is likely to be many small improvements in EF Core, together with guidance and longer-term collaborations with other teams to improve end-to-end experiences that go beyond just EF.</span></span>

<span data-ttu-id="246a9-210">Konkrétní oblasti, na které se plánujeme podívat:</span><span class="sxs-lookup"><span data-stu-id="246a9-210">Specific areas we plan to look at are:</span></span>

* <span data-ttu-id="246a9-211">Nasazení, včetně prostředí pro použití nástrojů EF, například pro migrace</span><span class="sxs-lookup"><span data-stu-id="246a9-211">Deployment, including the experience for using EF tooling such as for Migrations</span></span>
* <span data-ttu-id="246a9-212">Modely aplikací, včetně Xamarin a Blazor, a pravděpodobně dalších</span><span class="sxs-lookup"><span data-stu-id="246a9-212">Application models, including Xamarin and Blazor, and probably others</span></span>
* <span data-ttu-id="246a9-213">Prostředí SQLite, včetně prostorového prostředí a opětovného sestavování tabulek</span><span class="sxs-lookup"><span data-stu-id="246a9-213">SQLite experiences, including the spatial experience and table rebuilds</span></span>
* <span data-ttu-id="246a9-214">AOT a propojení prostředí</span><span class="sxs-lookup"><span data-stu-id="246a9-214">AOT and linking experiences</span></span>
* <span data-ttu-id="246a9-215">Integrace diagnostiky, včetně čítačů výkonu</span><span class="sxs-lookup"><span data-stu-id="246a9-215">Diagnostics integration, including perf counters</span></span>

## <a name="performance"></a><span data-ttu-id="246a9-216">Výkon</span><span class="sxs-lookup"><span data-stu-id="246a9-216">Performance</span></span>

<span data-ttu-id="246a9-217">Vedoucí vývojář: @roji</span><span class="sxs-lookup"><span data-stu-id="246a9-217">Lead developer: @roji</span></span>

<span data-ttu-id="246a9-218">Sledováno [problémy s označením `area-perf` v milníku 5,0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="246a9-218">Tracked by [issues labeled with `area-perf` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="246a9-219">Velikost trička pro T: L</span><span class="sxs-lookup"><span data-stu-id="246a9-219">T-shirt size: L</span></span>

<span data-ttu-id="246a9-220">Stav: probíhá</span><span class="sxs-lookup"><span data-stu-id="246a9-220">Status: In-progress</span></span>

<span data-ttu-id="246a9-221">Pro EF Core plánujeme zdokonalit naši sadu srovnávacích testů výkonu a zvýšit výkon na modul runtime.</span><span class="sxs-lookup"><span data-stu-id="246a9-221">For EF Core, we plan to improve our suite of performance benchmarks and make directed performance improvements to the runtime.</span></span> <span data-ttu-id="246a9-222">Kromě toho plánujeme dokončit nové rozhraní API pro dávkování ADO.NET, které bylo prototypem v průběhu cyklu vydávání 3,0.</span><span class="sxs-lookup"><span data-stu-id="246a9-222">In addition, we plan to complete the new ADO.NET batching API which was prototyped during the 3.0 release cycle.</span></span> <span data-ttu-id="246a9-223">Ve vrstvě ADO.NET máme také k dispozici další vylepšení výkonu pro poskytovatele Npgsql.</span><span class="sxs-lookup"><span data-stu-id="246a9-223">Also at the ADO.NET layer, we plan additional performance improvements to the Npgsql provider.</span></span>

<span data-ttu-id="246a9-224">V rámci této práce také plánujeme podle potřeby přidat čítače výkonu ADO.NET/EF Core a další diagnostiku.</span><span class="sxs-lookup"><span data-stu-id="246a9-224">As part of this work we also plan to add ADO.NET/EF Core performance counters and other diagnostics as appropriate.</span></span>

## <a name="architecturalcontributor-documentation"></a><span data-ttu-id="246a9-225">Dokumentace k architektuře/přispěvatelům</span><span class="sxs-lookup"><span data-stu-id="246a9-225">Architectural/contributor documentation</span></span>

<span data-ttu-id="246a9-226">Vedoucí dokumentace: @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="246a9-226">Lead documenter: @ajcvickers</span></span>

<span data-ttu-id="246a9-227">Sledováno [#1920](https://github.com/dotnet/EntityFramework.Docs/issues/1920)</span><span class="sxs-lookup"><span data-stu-id="246a9-227">Tracked by [#1920](https://github.com/dotnet/EntityFramework.Docs/issues/1920)</span></span>

<span data-ttu-id="246a9-228">Velikost trička pro T: L</span><span class="sxs-lookup"><span data-stu-id="246a9-228">T-shirt size: L</span></span>

<span data-ttu-id="246a9-229">Stav: probíhá</span><span class="sxs-lookup"><span data-stu-id="246a9-229">Status: In-progress</span></span>

<span data-ttu-id="246a9-230">Tady je postup, který usnadňuje pochopení toho, co se v vnitřních EF Corech prochází.</span><span class="sxs-lookup"><span data-stu-id="246a9-230">The idea here is to make it easier to understand what is going on in the internals of EF Core.</span></span> <span data-ttu-id="246a9-231">To může být užitečné pro kohokoli, kdo používá EF Core, ale primární motivace usnadňuje externím lidem:</span><span class="sxs-lookup"><span data-stu-id="246a9-231">This can be useful to anyone using EF Core, but the primary motivation is to make it easier for external people to:</span></span>

* <span data-ttu-id="246a9-232">Přispívání do kódu EF Core</span><span class="sxs-lookup"><span data-stu-id="246a9-232">Contribute to the EF Core code</span></span>
* <span data-ttu-id="246a9-233">Vytvořit poskytovatele databáze</span><span class="sxs-lookup"><span data-stu-id="246a9-233">Create database providers</span></span>
* <span data-ttu-id="246a9-234">Sestavení dalších rozšíření</span><span class="sxs-lookup"><span data-stu-id="246a9-234">Build other extensions</span></span>

## <a name="microsoftdatasqlite-documentation"></a><span data-ttu-id="246a9-235">Dokumentace k Microsoft. data. sqlite</span><span class="sxs-lookup"><span data-stu-id="246a9-235">Microsoft.Data.Sqlite documentation</span></span>

<span data-ttu-id="246a9-236">Vedoucí dokumentace: @bricelam</span><span class="sxs-lookup"><span data-stu-id="246a9-236">Lead documenter: @bricelam</span></span>

<span data-ttu-id="246a9-237">Sledováno [#1675](https://github.com/dotnet/EntityFramework.Docs/issues/1675)</span><span class="sxs-lookup"><span data-stu-id="246a9-237">Tracked by [#1675](https://github.com/dotnet/EntityFramework.Docs/issues/1675)</span></span>

<span data-ttu-id="246a9-238">Velikost T-tričko: M</span><span class="sxs-lookup"><span data-stu-id="246a9-238">T-shirt size: M</span></span>

<span data-ttu-id="246a9-239">Stav: dokončeno.</span><span class="sxs-lookup"><span data-stu-id="246a9-239">Status: Completed.</span></span> <span data-ttu-id="246a9-240">Nová dokumentace je [živá na webu Microsoft docs](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli).</span><span class="sxs-lookup"><span data-stu-id="246a9-240">The new documentation is [live on the Microsoft docs site](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli).</span></span>

<span data-ttu-id="246a9-241">Tým EF také vlastní poskytovatele Microsoft. data. sqlite ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="246a9-241">The EF Team also owns the Microsoft.Data.Sqlite ADO.NET provider.</span></span> <span data-ttu-id="246a9-242">V rámci vydání 5,0 plánujeme plně zdokumentovat tohoto poskytovatele.</span><span class="sxs-lookup"><span data-stu-id="246a9-242">We plan to fully document this provider as part of the 5.0 release.</span></span>

## <a name="general-documentation"></a><span data-ttu-id="246a9-243">Obecná dokumentace</span><span class="sxs-lookup"><span data-stu-id="246a9-243">General documentation</span></span>

<span data-ttu-id="246a9-244">Vedoucí dokumentace: @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="246a9-244">Lead documenter: @ajcvickers</span></span>

<span data-ttu-id="246a9-245">Sledováno [problémy v úložišti dokumentů v milníku 5,0](https://github.com/dotnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="246a9-245">Tracked by [issues in the docs repo in the 5.0 milestone](https://github.com/dotnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="246a9-246">Velikost trička pro T: L</span><span class="sxs-lookup"><span data-stu-id="246a9-246">T-shirt size: L</span></span>

<span data-ttu-id="246a9-247">Stav: probíhá</span><span class="sxs-lookup"><span data-stu-id="246a9-247">Status: In-progress</span></span>

<span data-ttu-id="246a9-248">V průběhu aktualizace dokumentace pro vydání 3,0 a 3,1 už v dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="246a9-248">We are already in the process of updating documentation for the 3.0 and 3.1 releases.</span></span> <span data-ttu-id="246a9-249">Pracujeme také na:</span><span class="sxs-lookup"><span data-stu-id="246a9-249">We are also working on:</span></span>
  * <span data-ttu-id="246a9-250">Změna dokumentů Začínáme, aby byly lépe přístupné/snazší</span><span class="sxs-lookup"><span data-stu-id="246a9-250">An overhaul of the getting started docs to make them more approachable/easier to follow</span></span>
  * <span data-ttu-id="246a9-251">Reorganizace dokumentů, aby bylo snazší najít a přidat křížové odkazy</span><span class="sxs-lookup"><span data-stu-id="246a9-251">Reorganization of docs to make things easier to find and to add cross-references</span></span>
  * <span data-ttu-id="246a9-252">Přidání dalších podrobností a objasnění stávajících dokumentů</span><span class="sxs-lookup"><span data-stu-id="246a9-252">Adding more details and clarifications to existing docs</span></span>
  * <span data-ttu-id="246a9-253">Aktualizace ukázek a přidání dalších příkladů</span><span class="sxs-lookup"><span data-stu-id="246a9-253">Updating the samples and adding more examples</span></span>

## <a name="fixing-bugs"></a><span data-ttu-id="246a9-254">Oprava chyb</span><span class="sxs-lookup"><span data-stu-id="246a9-254">Fixing bugs</span></span>

<span data-ttu-id="246a9-255">Sledováno [problémy s označením `type-bug` v milníku 5,0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)</span><span class="sxs-lookup"><span data-stu-id="246a9-255">Tracked by [issues labeled with `type-bug` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)</span></span>

<span data-ttu-id="246a9-256">Vývojáři: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="246a9-256">Developers: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span></span>

<span data-ttu-id="246a9-257">Velikost trička pro T: L</span><span class="sxs-lookup"><span data-stu-id="246a9-257">T-shirt size: L</span></span>

<span data-ttu-id="246a9-258">Stav: probíhá</span><span class="sxs-lookup"><span data-stu-id="246a9-258">Status: In-progress</span></span>

<span data-ttu-id="246a9-259">V době psaní máme 135 chyb, které jsou posouzené jako opravené ve verzi 5,0 (s již opravenou 62), ale výrazně se překrývají s výše uvedeným _obecným vylepšením dotazů_ .</span><span class="sxs-lookup"><span data-stu-id="246a9-259">At the time of writing, we have 135 bugs triaged to be fixed in the 5.0 release (with 62 already fixed), but there is significant overlap with the _General query enhancements_ section above.</span></span>

<span data-ttu-id="246a9-260">Příchozí rychlost (problémy, které končí jako práce v milníku), probíhala přibližně 23 problémů za měsíc v průběhu vydání 3,0.</span><span class="sxs-lookup"><span data-stu-id="246a9-260">The incoming rate (issues that end up as work in a milestone) was about 23 issues per month over the course of the 3.0 release.</span></span> <span data-ttu-id="246a9-261">Ne všechny tyto akce budou muset být opraveny v 5,0.</span><span class="sxs-lookup"><span data-stu-id="246a9-261">Not all of these will need to be fixed in 5.0.</span></span> <span data-ttu-id="246a9-262">V rámci hrubého odhadu plánujeme vyřešit další 150 problémů v časovém intervalu 5,0.</span><span class="sxs-lookup"><span data-stu-id="246a9-262">As a rough estimate we plan to fix an additional 150 issues in the 5.0 time frame.</span></span>

## <a name="small-enhancements"></a><span data-ttu-id="246a9-263">Malá vylepšení</span><span class="sxs-lookup"><span data-stu-id="246a9-263">Small enhancements</span></span>

<span data-ttu-id="246a9-264">Sledováno [problémy s označením `type-enhancement` v milníku 5,0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)</span><span class="sxs-lookup"><span data-stu-id="246a9-264">Tracked by [issues labeled with `type-enhancement` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)</span></span>

<span data-ttu-id="246a9-265">Vývojáři: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="246a9-265">Developers: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span></span>

<span data-ttu-id="246a9-266">Velikost trička pro T: L</span><span class="sxs-lookup"><span data-stu-id="246a9-266">T-shirt size: L</span></span>

<span data-ttu-id="246a9-267">Stav: probíhá</span><span class="sxs-lookup"><span data-stu-id="246a9-267">Status: In-progress</span></span>

<span data-ttu-id="246a9-268">Kromě větších funkcí uvedených výše je také k dispozici mnoho menších vylepšení naplánovaných na 5,0, aby bylo možné opravit "papírové řezy".</span><span class="sxs-lookup"><span data-stu-id="246a9-268">In addition to the bigger features outlined above, we also have many smaller improvements scheduled for 5.0 to fix "paper-cuts".</span></span> <span data-ttu-id="246a9-269">Všimněte si, že mnohé z těchto vylepšení se vztahují i na obecnější motivy popsané výše.</span><span class="sxs-lookup"><span data-stu-id="246a9-269">Note that many of these enhancements are also covered by the more general themes outlined above.</span></span>

## <a name="below-the-line"></a><span data-ttu-id="246a9-270">Pod řádkem</span><span class="sxs-lookup"><span data-stu-id="246a9-270">Below-the-line</span></span>

<span data-ttu-id="246a9-271">Sledováno [problémy s označením `consider-for-next-release`](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)</span><span class="sxs-lookup"><span data-stu-id="246a9-271">Tracked by [issues labeled with `consider-for-next-release`](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)</span></span>

<span data-ttu-id="246a9-272">Jedná se o opravy chyb a vylepšení, která **nejsou aktuálně** naplánovaná pro vydání 5,0, ale budeme se považovat za ambiciózní cíle v závislosti na pokroku v práci.</span><span class="sxs-lookup"><span data-stu-id="246a9-272">These are bug fixes and enhancements that are **not** currently scheduled for the 5.0 release, but we will look at as stretch goals depending on the progress made on the work above.</span></span>

<span data-ttu-id="246a9-273">Kromě toho vždy vybereme [nejbezpečnější problémy](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) při plánování.</span><span class="sxs-lookup"><span data-stu-id="246a9-273">In addition, we always consider the [most voted issues](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) when planning.</span></span> <span data-ttu-id="246a9-274">Vyjmutí všech těchto problémů z verze je vždycky bolestivý, ale pro tyto prostředky potřebujeme realističtější plán.</span><span class="sxs-lookup"><span data-stu-id="246a9-274">Cutting any of these issues from a release is always painful, but we do need a realistic plan for the resources we have.</span></span>

## <a name="feedback"></a><span data-ttu-id="246a9-275">Váš názor</span><span class="sxs-lookup"><span data-stu-id="246a9-275">Feedback</span></span>

<span data-ttu-id="246a9-276">Váš názor na plánování je důležitý.</span><span class="sxs-lookup"><span data-stu-id="246a9-276">Your feedback on planning is important.</span></span> <span data-ttu-id="246a9-277">Nejlepším způsobem, jak určit důležitost problému, je hlasovat (palec) pro daný problém na GitHubu.</span><span class="sxs-lookup"><span data-stu-id="246a9-277">The best way to indicate the importance of an issue is to vote (thumbs-up) for that issue on GitHub.</span></span> <span data-ttu-id="246a9-278">Tato data se pak budou předávat do [procesu plánování](../release-planning.md) pro další vydání.</span><span class="sxs-lookup"><span data-stu-id="246a9-278">This data will then feed into the [planning process](../release-planning.md) for the next release.</span></span>
