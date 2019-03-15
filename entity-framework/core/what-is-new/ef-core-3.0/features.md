---
title: Novinky v EF Core 3.0 – EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: b6774f615b04bf9579aac5dea217e7321631da0c
ms.sourcegitcommit: a709054b2bc7a8365201d71f59325891aacd315f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/14/2019
ms.locfileid: "57829184"
---
# <a name="new-features-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="d0c51-102">Novým funkcím zahrnutým v EF Core 3.0 (aktuálně ve verzi preview)</span><span class="sxs-lookup"><span data-stu-id="d0c51-102">New features included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d0c51-103">Mějte prosím na paměti, že sady funkcí a plány budoucích verzí se vždy mohou změnit a přestože se snažíme se zachovat aktuální, nemusí odrážet naše nejnovější plány vůbec na této stránce vyprší.</span><span class="sxs-lookup"><span data-stu-id="d0c51-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="d0c51-104">Následující seznam obsahuje hlavní nové funkce pro EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d0c51-104">The following list includes the major new features planned for EF Core 3.0.</span></span>
<span data-ttu-id="d0c51-105">Většina těchto funkcí nejsou zahrnuté v aktuální verzi preview, ale bude k dispozici během postupu směrem k RTM.</span><span class="sxs-lookup"><span data-stu-id="d0c51-105">Most of these features are not included in the current preview, but will become available as we make progress towards RTM.</span></span>

<span data-ttu-id="d0c51-106">Důvodem je, že na začátku uvolnění se Zaměřujeme na implementaci plánované [rozbíjející změny v](xref:core/what-is-new/ef-core-3.0/breaking-changes).</span><span class="sxs-lookup"><span data-stu-id="d0c51-106">The reason is that at the beginning of the release we are focusing on implementing planned [breaking changes](xref:core/what-is-new/ef-core-3.0/breaking-changes).</span></span>
<span data-ttu-id="d0c51-107">Mnohé z těchto novinkách jsou vylepšení na EF Core na své vlastní.</span><span class="sxs-lookup"><span data-stu-id="d0c51-107">Many of these breaking changes are improvements to EF Core on their own.</span></span>
<span data-ttu-id="d0c51-108">Řada dalších vyžadovaných pro odblokování dalších vylepšení.</span><span class="sxs-lookup"><span data-stu-id="d0c51-108">Many others are required to unblock further improvements.</span></span> 

<span data-ttu-id="d0c51-109">Úplný seznam oprav chyb a vylepšení probíhá, zobrazí se [tento dotaz v našich sledování problémů](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc).</span><span class="sxs-lookup"><span data-stu-id="d0c51-109">For a complete list of bug fixes and enhancements underway, you can see [this query in our issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc).</span></span>

## <a name="linq-improvements"></a><span data-ttu-id="d0c51-110">Vylepšení LINQ</span><span class="sxs-lookup"><span data-stu-id="d0c51-110">LINQ improvements</span></span> 

[<span data-ttu-id="d0c51-111">Sledování problému #12795</span><span class="sxs-lookup"><span data-stu-id="d0c51-111">Tracking Issue #12795</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

<span data-ttu-id="d0c51-112">Bylo zahájeno práci na tuto funkci, ale není zahrnutý v aktuální verzi preview.</span><span class="sxs-lookup"><span data-stu-id="d0c51-112">Work on this feature has started but it isn't included in the current preview.</span></span>

<span data-ttu-id="d0c51-113">LINQ umožňuje psát dotazy databáze, aniž byste museli opustit váš jazyk podle vlastní volby, využití výhod bohaté zadejte informace, které pomůžou technologie IntelliSense a kontrola typu v době kompilace.</span><span class="sxs-lookup"><span data-stu-id="d0c51-113">LINQ enables you to write database queries without leaving your language of choice, taking advantage of rich type information to get IntelliSense and compile-time type checking.</span></span>
<span data-ttu-id="d0c51-114">Ale LINQ také umožňuje psát neomezený počet složité dotazy a, který byl vždy velkým problémem pro zprostředkovatele LINQ.</span><span class="sxs-lookup"><span data-stu-id="d0c51-114">But LINQ also enables you to write an unlimited number of complicated queries, and that has always been a huge challenge for LINQ providers.</span></span>
<span data-ttu-id="d0c51-115">V prvních několika verzích EF Core jsme vyřešit, v části tím, jaké části dotazu může být přeloženy na SQL a tím, že zbývající části dotazu ke spuštění v paměti na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="d0c51-115">In the first few versions of EF Core, we solved that in part by figuring out what portions of a query could be translated to SQL, and then by allowing the rest of the query to execute in memory on the client.</span></span>
<span data-ttu-id="d0c51-116">Toto spuštění na straně klienta může být žádoucí v některých situacích ale v mnoha jiných případech může vést neefektivní dotazy, které nejsou označené, dokud je aplikace nasazená do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="d0c51-116">This client-side execution can be desirable in some situations, but in many other cases it can result in inefficient queries that may not be identified until an application is deployed to production.</span></span>
<span data-ttu-id="d0c51-117">V EF Core 3.0 plánujeme změnit velký fungování naše implementace LINQ, a jak můžeme otestovat.</span><span class="sxs-lookup"><span data-stu-id="d0c51-117">In EF Core 3.0, we're planning to make profound changes to how our LINQ implementation works, and how we test it.</span></span>
<span data-ttu-id="d0c51-118">Cíle jsou k němu robustnější (třeba, aby se zabránilo přerušení dotazů v aktualizací), Povolit překlad více výrazů správně do databáze SQL, vygenerujte efektivní dotazy ve více případech a zabránit v přechodu nezjištěné neefektivní dotazy.</span><span class="sxs-lookup"><span data-stu-id="d0c51-118">The goals are to make it more robust (for example, to avoid breaking queries in patch releases), to enable translating more expressions correctly into SQL, to generate efficient queries in more cases, and to prevent inefficient queries from going undetected.</span></span>

## <a name="cosmos-db-support"></a><span data-ttu-id="d0c51-119">Podpora služby cosmos DB</span><span class="sxs-lookup"><span data-stu-id="d0c51-119">Cosmos DB support</span></span> 

[<span data-ttu-id="d0c51-120">Sledování problému #8443</span><span class="sxs-lookup"><span data-stu-id="d0c51-120">Tracking Issue #8443</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/8443)

<span data-ttu-id="d0c51-121">Tato funkce je zahrnutá v aktuální verzi preview, ale ještě není dokončena.</span><span class="sxs-lookup"><span data-stu-id="d0c51-121">This feature is included in the current preview, but isn't complete yet.</span></span> 

<span data-ttu-id="d0c51-122">Pracujeme na poskytovatele služby Cosmos DB pro jádro EF Core a umožňuje vývojářům znáte model programování na EF snadno cílit na jako databáze aplikace služby Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="d0c51-122">We're working on a Cosmos DB provider for EF Core, to enable developers familiar with the EF programing model to easily target Azure Cosmos DB as an application database.</span></span>
<span data-ttu-id="d0c51-123">Cílem je, že některé z výhod Cosmos DB, jako jsou globální distribuce "always on" dostupnost, elastické škálovatelnosti a nízké latenci, ještě více přístupné pro vývojáře na platformě .NET.</span><span class="sxs-lookup"><span data-stu-id="d0c51-123">The goal is to make some of the advantages of Cosmos DB, like global distribution, "always on" availability, elastic scalability, and low latency, even more accessible to .NET developers.</span></span>
<span data-ttu-id="d0c51-124">Zprostředkovatel umožní většina EF Core funkcí, jako je sledování automatických změn, LINQ a převody hodnot, s využitím rozhraní SQL API ve službě Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="d0c51-124">The provider will enable most EF Core features, like automatic change tracking, LINQ, and value conversions, against the SQL API in Cosmos DB.</span></span>
<span data-ttu-id="d0c51-125">Začali jsme snaha před EF Core 2.2 a [jsme se rozhodli některé verze zprostředkovatele k dispozici ve verzi preview](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).</span><span class="sxs-lookup"><span data-stu-id="d0c51-125">We started this effort before EF Core 2.2, and [we have made some preview versions of the provider available](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).</span></span>
<span data-ttu-id="d0c51-126">Nový plán má pokračovat ve vývoji poskytovatele spolu s EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d0c51-126">The new plan is to continue developing the provider alongside EF Core 3.0.</span></span> 

## <a name="c-80-support"></a><span data-ttu-id="d0c51-127">C#Podpora 8.0</span><span class="sxs-lookup"><span data-stu-id="d0c51-127">C# 8.0 support</span></span>

<span data-ttu-id="d0c51-128">[Sledování problému #12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047)
[sledování problému #10347](https://github.com/aspnet/EntityFrameworkCore/issues/10347)</span><span class="sxs-lookup"><span data-stu-id="d0c51-128">[Tracking Issue #12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047)
[Tracking Issue #10347](https://github.com/aspnet/EntityFrameworkCore/issues/10347)</span></span>

<span data-ttu-id="d0c51-129">Bylo zahájeno práci na tuto funkci, ale není zahrnutý v aktuální verzi preview.</span><span class="sxs-lookup"><span data-stu-id="d0c51-129">Work on this feature has started but it isn't included in the current preview.</span></span>

<span data-ttu-id="d0c51-130">Chceme našim zákazníkům využívat některé části [nové funkce v C# 8.0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/) , jako jsou asynchronní datové proudy (včetně `await foreach`) a typy s možnou hodnotou Null odkazů pomocí EF Core.</span><span class="sxs-lookup"><span data-stu-id="d0c51-130">We want our customers to take advantage of some of the [new features coming in C# 8.0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/) like async streams (including `await foreach`) and nullable reference types while using EF Core.</span></span>

## <a name="reverse-engineering-of-database-views"></a><span data-ttu-id="d0c51-131">Zpětná analýza zobrazení databáze</span><span class="sxs-lookup"><span data-stu-id="d0c51-131">Reverse engineering of database views</span></span>

[<span data-ttu-id="d0c51-132">Sledování problému #1679</span><span class="sxs-lookup"><span data-stu-id="d0c51-132">Tracking Issue #1679</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/1679)

<span data-ttu-id="d0c51-133">Tato funkce není součástí aktuální verzi preview.</span><span class="sxs-lookup"><span data-stu-id="d0c51-133">This feature isn't included in the current preview.</span></span>

<span data-ttu-id="d0c51-134">[Typy dotazů](xref:core/modeling/query-types), zavedená v EF Core 2.1 a považován za typy entit bez klíčů v EF Core 3.0, představují data, která mohou číst z databáze, ale nejde aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="d0c51-134">[Query types](xref:core/modeling/query-types), introduced in EF Core 2.1 and considered entity types without keys in EF Core 3.0, represent data that can be read from the database, but cannot be updated.</span></span>
<span data-ttu-id="d0c51-135">Tato vlastnost je mezi nimi vlastně ideálně se hodí pro zobrazení databáze ve většině scénářů, abychom plánovat k automatizaci vytváření typů entit bez klíčů při zpětné analýze zobrazení databáze.</span><span class="sxs-lookup"><span data-stu-id="d0c51-135">This characteristic makes them an excellent fit for database views in most scenarios, so we plan to automate the creation of entity types without keys when reverse engineering database views.</span></span>

## <a name="property-bag-entities"></a><span data-ttu-id="d0c51-136">Vlastnosti kontejneru objektů a dat entit</span><span class="sxs-lookup"><span data-stu-id="d0c51-136">Property bag entities</span></span> 

<span data-ttu-id="d0c51-137">[Sledování problému #13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) a [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914)</span><span class="sxs-lookup"><span data-stu-id="d0c51-137">[Tracking Issue #13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) and [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914)</span></span>

<span data-ttu-id="d0c51-138">Bylo zahájeno práci na tuto funkci, ale není zahrnutý v aktuální verzi preview.</span><span class="sxs-lookup"><span data-stu-id="d0c51-138">Work on this feature has started but it isn't included in the current preview.</span></span> 

<span data-ttu-id="d0c51-139">Tato funkce je o povolení entity, které ukládají data v indexované vlastnosti namísto regulární vlastností a také o bude možné použít instance stejné třídy .NET (potenciálně něco jednoduchého jako `Dictionary<string, object>`) k reprezentaci entit různých typů ve stejném modelu EF Core.</span><span class="sxs-lookup"><span data-stu-id="d0c51-139">This feature is about enabling entities that store data in indexed properties instead of regular properties, and also about being able to use instances of the same .NET class (potentially something as simple as a `Dictionary<string, object>`) to represent different entity types in the same EF Core model.</span></span>
<span data-ttu-id="d0c51-140">Tato funkce je odrazový můstek k podpoře many-to-many relací bez připojení k entitě ([vydat #1368](https://github.com/aspnet/EntityFrameworkCore/issues/1368)), což je jedna z nejžádanějších vylepšení pro jádro EF Core.</span><span class="sxs-lookup"><span data-stu-id="d0c51-140">This feature is a stepping stone to support many-to-many relationships without a join entity ([issue #1368](https://github.com/aspnet/EntityFrameworkCore/issues/1368)), which is one of the most requested improvements for EF Core.</span></span>

## <a name="ef-63-on-net-core"></a><span data-ttu-id="d0c51-141">EF 6.3 v rozhraní .NET Core</span><span class="sxs-lookup"><span data-stu-id="d0c51-141">EF 6.3 on .NET Core</span></span> 

[<span data-ttu-id="d0c51-142">Sledování problému EF6 #271</span><span class="sxs-lookup"><span data-stu-id="d0c51-142">Tracking Issue EF6#271</span></span>](https://github.com/aspnet/EntityFramework6/issues/271)

<span data-ttu-id="d0c51-143">Bylo zahájeno práci na tuto funkci, ale není zahrnutý v aktuální verzi preview.</span><span class="sxs-lookup"><span data-stu-id="d0c51-143">Work on this feature has started but it isn't included in the current preview.</span></span> 

<span data-ttu-id="d0c51-144">Chápeme, že mnoho existujících aplikací pomocí předchozí verze EF a že přenesení do EF Core jenom, abyste mohli využívat výhod .NET Core může někdy vyžadovat značné úsilí.</span><span class="sxs-lookup"><span data-stu-id="d0c51-144">We understand that many existing applications use previous versions of EF, and that porting them to EF Core only to take advantage of .NET Core can sometimes require a significant effort.</span></span>
<span data-ttu-id="d0c51-145">Z tohoto důvodu jsme se přizpůsobení další verze EF 6 a spustit na .NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d0c51-145">For that reason, we will be adapting the next version of EF 6 to run on .NET Core 3.0.</span></span>
<span data-ttu-id="d0c51-146">To usnadňuje přenos stávající aplikace s minimálními změnami provádíme.</span><span class="sxs-lookup"><span data-stu-id="d0c51-146">We are doing this to facilitate porting existing applications with minimal changes.</span></span>
<span data-ttu-id="d0c51-147">Existuje budou představovat určitá omezení.</span><span class="sxs-lookup"><span data-stu-id="d0c51-147">There are going to be some limitations.</span></span> <span data-ttu-id="d0c51-148">Příklad:</span><span class="sxs-lookup"><span data-stu-id="d0c51-148">For example:</span></span>
- <span data-ttu-id="d0c51-149">Bude vyžadovat noví zprostředkovatelé pro práci s jinými databázemi kromě podpory systému SQL Server na .NET Core</span><span class="sxs-lookup"><span data-stu-id="d0c51-149">It will require new providers to work with other databases besides the included SQL Server support on .NET Core</span></span>
- <span data-ttu-id="d0c51-150">Prostorový podporu systému SQL Server nebude povolen</span><span class="sxs-lookup"><span data-stu-id="d0c51-150">Spatial support with SQL Server won't be enabled</span></span>

<span data-ttu-id="d0c51-151">Všimněte si také, že neexistují žádné nové plánované funkce pro EF 6 v tomto okamžiku.</span><span class="sxs-lookup"><span data-stu-id="d0c51-151">Note also that there are no new features planned for EF 6 at this point.</span></span>
