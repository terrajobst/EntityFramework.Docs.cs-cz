---
title: Nové funkce v EF Core 3,0 – EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: d61fa884f4669daa220ffc96ae59dd63518e6d5a
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921678"
---
# <a name="new-features-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="6c5a6-102">Nové funkce, které jsou součástí EF Core 3,0 (aktuálně ve verzi Preview)</span><span class="sxs-lookup"><span data-stu-id="6c5a6-102">New features included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6c5a6-103">Počítejte s tím, že sady funkcí a plány budoucích verzí se vždycky mění, a i když se pokusíme tuto stránku uchovávat v aktuálním stavu, nemusí se po celou dobu projevit naše nejnovější plány.</span><span class="sxs-lookup"><span data-stu-id="6c5a6-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="6c5a6-104">Následující seznam obsahuje hlavní nové funkce, které jsou plánovány pro EF Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="6c5a6-104">The following list includes the major new features planned for EF Core 3.0.</span></span>
<span data-ttu-id="6c5a6-105">Většina těchto funkcí není součástí aktuální verze Preview, ale bude k dispozici v průběhu vývoje směrem k RTM.</span><span class="sxs-lookup"><span data-stu-id="6c5a6-105">Most of these features are not included in the current preview, but will become available as we make progress towards RTM.</span></span>

<span data-ttu-id="6c5a6-106">Důvodem je to, že na začátku vydání se zaměřujeme na implementaci plánovaných [nejnovějších změn](xref:core/what-is-new/ef-core-3.0/breaking-changes).</span><span class="sxs-lookup"><span data-stu-id="6c5a6-106">The reason is that at the beginning of the release we are focusing on implementing planned [breaking changes](xref:core/what-is-new/ef-core-3.0/breaking-changes).</span></span>
<span data-ttu-id="6c5a6-107">Mnohé z těchto nejnovějších změn jsou vylepšení EF Core vlastními.</span><span class="sxs-lookup"><span data-stu-id="6c5a6-107">Many of these breaking changes are improvements to EF Core on their own.</span></span>
<span data-ttu-id="6c5a6-108">K odblokování dalších vylepšení je potřeba řada dalších.</span><span class="sxs-lookup"><span data-stu-id="6c5a6-108">Many others are required to unblock further improvements.</span></span> 

<span data-ttu-id="6c5a6-109">Úplný seznam oprav chyb a vylepšení, které probíhají, najdete [v našem sledování problémů](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc).</span><span class="sxs-lookup"><span data-stu-id="6c5a6-109">For a complete list of bug fixes and enhancements underway, you can see [this query in our issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc).</span></span>

## <a name="linq-improvements"></a><span data-ttu-id="6c5a6-110">Vylepšení LINQ</span><span class="sxs-lookup"><span data-stu-id="6c5a6-110">LINQ improvements</span></span> 

[<span data-ttu-id="6c5a6-111">Sledování problému #12795</span><span class="sxs-lookup"><span data-stu-id="6c5a6-111">Tracking Issue #12795</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

<span data-ttu-id="6c5a6-112">Práce na této funkci začala, ale není součástí aktuální verze Preview.</span><span class="sxs-lookup"><span data-stu-id="6c5a6-112">Work on this feature has started but it isn't included in the current preview.</span></span>

<span data-ttu-id="6c5a6-113">LINQ umožňuje psát databázové dotazy bez nutnosti opustit svůj jazyk a využít bohatých informací o typech k získání IntelliSense a kontrole typu při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="6c5a6-113">LINQ enables you to write database queries without leaving your language of choice, taking advantage of rich type information to get IntelliSense and compile-time type checking.</span></span>
<span data-ttu-id="6c5a6-114">Ale LINQ také umožňuje napsat neomezený počet složitých dotazů a který má vždycky pro poskytovatele LINQ velmi velkou výzvu.</span><span class="sxs-lookup"><span data-stu-id="6c5a6-114">But LINQ also enables you to write an unlimited number of complicated queries, and that has always been a huge challenge for LINQ providers.</span></span>
<span data-ttu-id="6c5a6-115">V prvních několika verzích EF Core jsme vyřešili, které části dotazu by mohly být přeložené do SQL, a pak tím, že se zbytek dotazu spustí v paměti na klientovi.</span><span class="sxs-lookup"><span data-stu-id="6c5a6-115">In the first few versions of EF Core, we solved that in part by figuring out what portions of a query could be translated to SQL, and then by allowing the rest of the query to execute in memory on the client.</span></span>
<span data-ttu-id="6c5a6-116">Toto spuštění na straně klienta může být v některých situacích žádoucí, ale v mnoha dalších případech může dojít k neefektivním dotazům, které nemusí být identifikované, dokud se aplikace nasadí do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="6c5a6-116">This client-side execution can be desirable in some situations, but in many other cases it can result in inefficient queries that may not be identified until an application is deployed to production.</span></span>
<span data-ttu-id="6c5a6-117">V EF Core 3,0 plánujeme, abychom provedli velmi snadné změny v tom, jak naše implementace LINQ funguje a jak ji testujeme.</span><span class="sxs-lookup"><span data-stu-id="6c5a6-117">In EF Core 3.0, we're planning to make profound changes to how our LINQ implementation works, and how we test it.</span></span>
<span data-ttu-id="6c5a6-118">Cílem je zvýšit robustnost (například aby se zabránilo neúmyslnému dotazování ve verzích oprav), aby se povolilo překládání dalších výrazů do SQL, aby se vytvořily efektivní dotazy ve více případech a aby nedocházelo k nezjistitelným dotazům.</span><span class="sxs-lookup"><span data-stu-id="6c5a6-118">The goals are to make it more robust (for example, to avoid breaking queries in patch releases), to enable translating more expressions correctly into SQL, to generate efficient queries in more cases, and to prevent inefficient queries from going undetected.</span></span>

## <a name="cosmos-db-support"></a><span data-ttu-id="6c5a6-119">Podpora Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="6c5a6-119">Cosmos DB support</span></span> 

[<span data-ttu-id="6c5a6-120">Sledování problému #8443</span><span class="sxs-lookup"><span data-stu-id="6c5a6-120">Tracking Issue #8443</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/8443)

<span data-ttu-id="6c5a6-121">Tato funkce je součástí aktuální verze Preview, ale ještě není dokončená.</span><span class="sxs-lookup"><span data-stu-id="6c5a6-121">This feature is included in the current preview, but isn't complete yet.</span></span> 

<span data-ttu-id="6c5a6-122">Pracujeme na poskytovateli Cosmos DB pro EF Core, aby mohli vývojáři, kteří znají model programu EF, snadno cílit Azure Cosmos DB jako databázi aplikace.</span><span class="sxs-lookup"><span data-stu-id="6c5a6-122">We're working on a Cosmos DB provider for EF Core, to enable developers familiar with the EF programing model to easily target Azure Cosmos DB as an application database.</span></span>
<span data-ttu-id="6c5a6-123">Cílem je udělat si některé z výhod Cosmos DB, jako je globální distribuce, "Always On", pružná škálovatelnost a nízká latence, dokonce i přístup k vývojářům v rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="6c5a6-123">The goal is to make some of the advantages of Cosmos DB, like global distribution, "always on" availability, elastic scalability, and low latency, even more accessible to .NET developers.</span></span>
<span data-ttu-id="6c5a6-124">Zprostředkovatel povolí většinu funkcí EF Core, jako je automatické sledování změn, LINQ a převod hodnot, s rozhraním SQL API v Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="6c5a6-124">The provider will enable most EF Core features, like automatic change tracking, LINQ, and value conversions, against the SQL API in Cosmos DB.</span></span>
<span data-ttu-id="6c5a6-125">Tuto snahu jsme zahájili před EF Core 2,2 a [provedli jsme několik verzí Preview, které poskytovatel nabízí](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).</span><span class="sxs-lookup"><span data-stu-id="6c5a6-125">We started this effort before EF Core 2.2, and [we have made some preview versions of the provider available](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).</span></span>
<span data-ttu-id="6c5a6-126">Novým plánem je pokračovat ve vývoji poskytovatele vedle EF Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="6c5a6-126">The new plan is to continue developing the provider alongside EF Core 3.0.</span></span> 

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="6c5a6-127">Závislé entity, které sdílí tabulku s objektem zabezpečení, jsou teď volitelné.</span><span class="sxs-lookup"><span data-stu-id="6c5a6-127">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="6c5a6-128">Sledování problému #9005</span><span class="sxs-lookup"><span data-stu-id="6c5a6-128">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="6c5a6-129">Tato funkce bude zavedena ve verzi EF Core 3,0-Preview 4.</span><span class="sxs-lookup"><span data-stu-id="6c5a6-129">This feature will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="6c5a6-130">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="6c5a6-130">Consider the following model:</span></span>
```C#
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

[Owned]
public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```

<span data-ttu-id="6c5a6-131">Počínaje EF Core 3,0, pokud `OrderDetails` je `Order` vlastněná nebo explicitně namapovaná na stejnou tabulku, bude možné přidat `Order` bez `OrderDetails` a všechny `OrderDetails` vlastnosti, s výjimkou, že primární klíč bude namapován na sloupce s možnou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="6c5a6-131">Starting with EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table, it will be possible to add an `Order` without an `OrderDetails` and all of the `OrderDetails` properties, except the primary key will be mapped to nullable columns.</span></span>

<span data-ttu-id="6c5a6-132">Při dotazování se EF Core nastaví `OrderDetails` na `null` , pokud některá z jejích požadovaných vlastností nemá hodnotu, nebo pokud se kromě primárního `null`klíče a všech vlastností nevyžadují žádné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="6c5a6-132">When querying, EF Core will set `OrderDetails` to `null` if any of its required properties doesn't have a value, or if it has no required properties besides the primary key and all properties are `null`.</span></span>

## <a name="c-80-support"></a><span data-ttu-id="6c5a6-133">C#podpora 8,0</span><span class="sxs-lookup"><span data-stu-id="6c5a6-133">C# 8.0 support</span></span>

<span data-ttu-id="6c5a6-134">[Sledování problému #12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047)
[sledování problému #10347](https://github.com/aspnet/EntityFrameworkCore/issues/10347)</span><span class="sxs-lookup"><span data-stu-id="6c5a6-134">[Tracking Issue #12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047)
[Tracking Issue #10347](https://github.com/aspnet/EntityFrameworkCore/issues/10347)</span></span>

<span data-ttu-id="6c5a6-135">Práce na této funkci začala, ale není součástí aktuální verze Preview.</span><span class="sxs-lookup"><span data-stu-id="6c5a6-135">Work on this feature has started but it isn't included in the current preview.</span></span>

<span data-ttu-id="6c5a6-136">Chceme, aby naši zákazníci využili výhod některých [nových funkcí v C# 8,0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/) , jako jsou asynchronní streamy (včetně `await foreach`) a typy s možnou hodnotou null při použití EF Core.</span><span class="sxs-lookup"><span data-stu-id="6c5a6-136">We want our customers to take advantage of some of the [new features coming in C# 8.0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/) like async streams (including `await foreach`) and nullable reference types while using EF Core.</span></span>

## <a name="reverse-engineering-of-database-views"></a><span data-ttu-id="6c5a6-137">Zpětná analýza zobrazení databáze</span><span class="sxs-lookup"><span data-stu-id="6c5a6-137">Reverse engineering of database views</span></span>

[<span data-ttu-id="6c5a6-138">Sledování problému #1679</span><span class="sxs-lookup"><span data-stu-id="6c5a6-138">Tracking Issue #1679</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/1679)

<span data-ttu-id="6c5a6-139">Tato funkce není součástí aktuální verze Preview.</span><span class="sxs-lookup"><span data-stu-id="6c5a6-139">This feature isn't included in the current preview.</span></span>

<span data-ttu-id="6c5a6-140">[Typy dotazů](xref:core/modeling/query-types), představené v EF Core 2,1 a považované za typy entit bez klíčů v EF Core 3,0, reprezentují data, která je možné číst z databáze, ale nelze je aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="6c5a6-140">[Query types](xref:core/modeling/query-types), introduced in EF Core 2.1 and considered entity types without keys in EF Core 3.0, represent data that can be read from the database, but cannot be updated.</span></span>
<span data-ttu-id="6c5a6-141">Tato vlastnost je ve většině scénářů vhodná pro zobrazení databáze, takže při zpětné analýze zobrazení databáze plánujeme automatizaci vytváření typů entit bez použití klíčů.</span><span class="sxs-lookup"><span data-stu-id="6c5a6-141">This characteristic makes them an excellent fit for database views in most scenarios, so we plan to automate the creation of entity types without keys when reverse engineering database views.</span></span>

## <a name="ef-63-on-net-core"></a><span data-ttu-id="6c5a6-142">EF 6,3 pro .NET Core</span><span class="sxs-lookup"><span data-stu-id="6c5a6-142">EF 6.3 on .NET Core</span></span>

[<span data-ttu-id="6c5a6-143">Sledování problému EF6 # 271</span><span class="sxs-lookup"><span data-stu-id="6c5a6-143">Tracking Issue EF6#271</span></span>](https://github.com/aspnet/EntityFramework6/issues/271)

<span data-ttu-id="6c5a6-144">Práce na této funkci začala, ale není součástí aktuální verze Preview.</span><span class="sxs-lookup"><span data-stu-id="6c5a6-144">Work on this feature has started but it isn't included in the current preview.</span></span> 

<span data-ttu-id="6c5a6-145">Chápeme, že mnoho existujících aplikací používá předchozí verze EF a že je bude EF Core jenom k tomu, aby využila výhody .NET Core, může někdy vyžadovat značné úsilí.</span><span class="sxs-lookup"><span data-stu-id="6c5a6-145">We understand that many existing applications use previous versions of EF, and that porting them to EF Core only to take advantage of .NET Core can sometimes require a significant effort.</span></span>
<span data-ttu-id="6c5a6-146">Z tohoto důvodu budeme přizpůsobovat další verzi EF 6, která bude běžet v .NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="6c5a6-146">For that reason, we will be adapting the next version of EF 6 to run on .NET Core 3.0.</span></span>
<span data-ttu-id="6c5a6-147">Provedeme to, abychom usnadnili přenos stávajících aplikací s minimálními změnami.</span><span class="sxs-lookup"><span data-stu-id="6c5a6-147">We are doing this to facilitate porting existing applications with minimal changes.</span></span>
<span data-ttu-id="6c5a6-148">Existují určitá omezení.</span><span class="sxs-lookup"><span data-stu-id="6c5a6-148">There are going to be some limitations.</span></span> <span data-ttu-id="6c5a6-149">Příklad:</span><span class="sxs-lookup"><span data-stu-id="6c5a6-149">For example:</span></span>
- <span data-ttu-id="6c5a6-150">Bude vyžadovat, aby noví poskytovatelé pracovali s dalšími databázemi kromě zahrnuté podpory SQL Server podpoře .NET Core.</span><span class="sxs-lookup"><span data-stu-id="6c5a6-150">It will require new providers to work with other databases besides the included SQL Server support on .NET Core</span></span>
- <span data-ttu-id="6c5a6-151">Prostorová podpora v SQL Server nebude povolena.</span><span class="sxs-lookup"><span data-stu-id="6c5a6-151">Spatial support with SQL Server won't be enabled</span></span>

<span data-ttu-id="6c5a6-152">Všimněte si také, že v tomto okamžiku nejsou plánovány žádné nové funkce pro EF 6.</span><span class="sxs-lookup"><span data-stu-id="6c5a6-152">Note also that there are no new features planned for EF 6 at this point.</span></span>
