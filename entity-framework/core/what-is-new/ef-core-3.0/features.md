---
title: Nové funkce v EF Core 3,0 – EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: d938f17daecd5031147951d0018602c5635de41d
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149104"
---
# <a name="new-features-included-in-ef-core-30"></a><span data-ttu-id="4594e-102">Nové funkce, které jsou součástí EF Core 3,0</span><span class="sxs-lookup"><span data-stu-id="4594e-102">New features included in EF Core 3.0</span></span>

<span data-ttu-id="4594e-103">Následující seznam obsahuje hlavní nové funkce, které jsou plánovány pro EF Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="4594e-103">The following list includes the major new features planned for EF Core 3.0.</span></span>

<span data-ttu-id="4594e-104">EF Core 3,0 je hlavní vydání a obsahuje také mnoho podstatných [změn](xref:core/what-is-new/ef-core-3.0/breaking-changes), což jsou vylepšení rozhraní API, která mohou mít negativní dopad na existující aplikace.</span><span class="sxs-lookup"><span data-stu-id="4594e-104">EF Core 3.0 is a major release and also contains numerous [breaking changes](xref:core/what-is-new/ef-core-3.0/breaking-changes), which are API improvements that may have negative impact on existing applications.</span></span>  

## <a name="linq-improvements"></a><span data-ttu-id="4594e-105">Vylepšení LINQ</span><span class="sxs-lookup"><span data-stu-id="4594e-105">LINQ improvements</span></span> 

<span data-ttu-id="4594e-106">LINQ umožňuje psát databázové dotazy bez nutnosti opustit svůj jazyk a využít bohatých informací o typech k tomu, aby nabízel IntelliSense a kontrolu typu při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="4594e-106">LINQ enables you to write database queries without leaving your language of choice, taking advantage of rich type information to offer IntelliSense and compile-time type checking.</span></span>
<span data-ttu-id="4594e-107">Ale LINQ také umožňuje napsat neomezený počet složitých dotazů, které obsahují libovolné výrazy (volání metod nebo operace).</span><span class="sxs-lookup"><span data-stu-id="4594e-107">But LINQ also enables you to write an unlimited number of complicated queries containing arbitrary expressions (method calls or operations).</span></span>
<span data-ttu-id="4594e-108">Zpracování všech těchto kombinací vždy bylo významnou výzvou pro poskytovatele LINQ.</span><span class="sxs-lookup"><span data-stu-id="4594e-108">Handling all those combinations has always been a significant challenge for LINQ providers.</span></span>
<span data-ttu-id="4594e-109">V EF Core 3,0 jsme přepsali naši implementaci LINQ tak, aby povolovala překlady dalších výrazů do SQL, aby bylo možné ve více případech generovat efektivní dotazy, aby nedocházelo k nezjistitelným dotazům, aby bylo možné postupně zavést nové dotazy. schopnosti a výkon improvementswithout přerušují stávající aplikace a poskytovatele dat.</span><span class="sxs-lookup"><span data-stu-id="4594e-109">In EF Core 3.0, we've rewritten our LINQ implementation to enable translating more expressions into SQL, to generate efficient queries in more cases, to prevent inefficient queries from going undetected, and to make it easier for us to gradually introduce new query capabilities and performance improvementswithout breaking existing applications and data providers.</span></span>

### <a name="client-evaluation"></a><span data-ttu-id="4594e-110">Vyhodnocení klientů</span><span class="sxs-lookup"><span data-stu-id="4594e-110">Client evaluation</span></span>

<span data-ttu-id="4594e-111">Hlavní změna v EF Core 3,0 musí dělat s tím, jak zpracovává výrazy LINQ, které nejde přeložit na SQL nebo parametry:</span><span class="sxs-lookup"><span data-stu-id="4594e-111">The main design change in EF Core 3.0 has to do with how it handles LINQ expressions that it cannot translate to SQL or parameters:</span></span>

<span data-ttu-id="4594e-112">V prvních několika verzích EF Core jednoduše zjistili, jaké části dotazu by mohly být přeloženy do SQL a aby se na klientovi spustil zbytek dotazu.</span><span class="sxs-lookup"><span data-stu-id="4594e-112">In the first few versions, EF Core simply figured out what portions of a query could be translated to SQL, and executed the rest of the query on the client.</span></span>
<span data-ttu-id="4594e-113">Tento typ spuštění na straně klienta může být v některých situacích žádoucí, ale v mnoha dalších případech může docházet k neefektivním dotazům.</span><span class="sxs-lookup"><span data-stu-id="4594e-113">This type of client-side execution can be desirable in some situations, but in many other cases it can result in inefficient queries.</span></span>
<span data-ttu-id="4594e-114">Například pokud EF Core 2,2 nemohl převést predikát ve `Where()` volání, provedl příkaz SQL bez filtru, přečte všechny řádky z databáze a pak je vyfiltruje v paměti.</span><span class="sxs-lookup"><span data-stu-id="4594e-114">For example, if EF Core 2.2 couldn't translate a predicate in a `Where()` call, it executed a SQL statement without a filter, read all all the rows from the database, and then filtered them in-memory.</span></span>
<span data-ttu-id="4594e-115">To může být přijatelné, pokud databáze obsahuje malý počet řádků, ale může způsobit významné problémy s výkonem nebo i selhání aplikace, pokud databáze obsahuje velké číslo nebo řádky.</span><span class="sxs-lookup"><span data-stu-id="4594e-115">That may be acceptable if the database contains a small number of rows, but can result in significant performance issues or even application failure if the database contains a large number or rows.</span></span>
<span data-ttu-id="4594e-116">V EF Core 3,0 jsme omezili Hodnocení klientů jenom na projekci nejvyšší úrovně (poslední volání do `Select()`).</span><span class="sxs-lookup"><span data-stu-id="4594e-116">In EF Core 3.0 we have restricted client evaluation to only happen on the top-level projection (the last call to `Select()`).</span></span>
<span data-ttu-id="4594e-117">Když EF Core 3,0 detekuje výrazy, které nelze přeložit nikde jinde v dotazu, vyvolá výjimku za běhu.</span><span class="sxs-lookup"><span data-stu-id="4594e-117">When EF Core 3.0 detects expressions that cannot be translated anywhere else in the query, it throws a runtime exception.</span></span>

## <a name="cosmos-db-support"></a><span data-ttu-id="4594e-118">Podpora Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="4594e-118">Cosmos DB support</span></span> 

<span data-ttu-id="4594e-119">Poskytovatel Cosmos DB pro EF Core umožňuje vývojářům, kteří znají model programu EF, snadno cílit Azure Cosmos DB jako databázi aplikace.</span><span class="sxs-lookup"><span data-stu-id="4594e-119">The Cosmos DB provider for EF Core enables developers familiar with the EF programing model to easily target Azure Cosmos DB as an application database.</span></span>
<span data-ttu-id="4594e-120">Cílem je udělat si některé z výhod Cosmos DB, jako je globální distribuce, "Always On", pružná škálovatelnost a nízká latence, dokonce i přístup k vývojářům v rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="4594e-120">The goal is to make some of the advantages of Cosmos DB, like global distribution, "always on" availability, elastic scalability, and low latency, even more accessible to .NET developers.</span></span>
<span data-ttu-id="4594e-121">Zprostředkovatel povolí většinu funkcí EF Core, jako je automatické sledování změn, LINQ a převod hodnot, s rozhraním SQL API v Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="4594e-121">The provider will enable most EF Core features, like automatic change tracking, LINQ, and value conversions, against the SQL API in Cosmos DB.</span></span>

## <a name="c-80-support"></a><span data-ttu-id="4594e-122">C#podpora 8,0</span><span class="sxs-lookup"><span data-stu-id="4594e-122">C# 8.0 support</span></span>

<span data-ttu-id="4594e-123">EF Core 3,0 využívá výhod některých nových funkcí v C# 8,0:</span><span class="sxs-lookup"><span data-stu-id="4594e-123">EF Core 3.0 takes advantage of some of the new features in C# 8.0:</span></span>

### <a name="asynchronous-streams"></a><span data-ttu-id="4594e-124">Asynchronní proudy</span><span class="sxs-lookup"><span data-stu-id="4594e-124">Asynchronous streams</span></span>

<span data-ttu-id="4594e-125">Výsledky asynchronního dotazu jsou nyní zpřístupněny pomocí nového `IAsyncEnumerable<T>` standardního rozhraní a lze je spotřebovat pomocí. `await foreach`</span><span class="sxs-lookup"><span data-stu-id="4594e-125">Asynchronous query results are now exposed using the new standard `IAsyncEnumerable<T>` interface and can be consumed using `await foreach`.</span></span>

``` csharp
var orders = 
  from o in context.Orders
  where o.Status == OrderStatus.Pending
  select o;

await foreach(var o in orders)
{
  Process(o);
} 
```

### <a name="nullable-reference-types"></a><span data-ttu-id="4594e-126">Odkazové typy s možnou hodnotou null</span><span class="sxs-lookup"><span data-stu-id="4594e-126">Nullable reference types</span></span> 

<span data-ttu-id="4594e-127">Pokud je tato nová funkce ve vašem kódu povolená, EF Core může mít důvod k hodnotě null vlastností refrence typů (buď primitivních typů jako řetězcové nebo navigační vlastnosti), aby se rozhodla hodnota null sloupců a relací v databázi.</span><span class="sxs-lookup"><span data-stu-id="4594e-127">When this new feature is enabled in your code, EF Core can reason about the nullability of properties of refrence types (either of primitive types like string or navigation properties) to decide the nullability of columns and relationships in the database.</span></span>

## <a name="interception"></a><span data-ttu-id="4594e-128">Zachycení</span><span class="sxs-lookup"><span data-stu-id="4594e-128">Interception</span></span>

<span data-ttu-id="4594e-129">Nové rozhraní API zachycení v EF Core 3,0 umožňuje programově a úpravu výsledku databázových operací nízké úrovně, ke kterým dochází jako součást běžné operace EF Core, jako je například otevření připojení, transakce initating a provádění příkazů.</span><span class="sxs-lookup"><span data-stu-id="4594e-129">The new interception API in EF Core 3.0 allows programatically observing and modifying the outcome of low-level database operations that occur as part of the normal operation of EF Core, such as opening connections, initating transactions, and executing commands.</span></span> 

## <a name="reverse-engineering-of-database-views"></a><span data-ttu-id="4594e-130">Zpětná analýza zobrazení databáze</span><span class="sxs-lookup"><span data-stu-id="4594e-130">Reverse engineering of database views</span></span>

<span data-ttu-id="4594e-131">Typy entit bez klíčů (dříve označované jako [typy dotazů](xref:core/modeling/keyless-entity-types)) reprezentují data, která je možné číst z databáze, ale nelze je aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="4594e-131">Entity types without keys (previously known as [query types](xref:core/modeling/keyless-entity-types)) represent data that can be read from the database, but cannot be updated.</span></span>
<span data-ttu-id="4594e-132">Tato vlastnost nabízí vynikající možnosti mapování zobrazení databáze ve většině scénářů, takže jsme při zpětné analýze zobrazení databáze automatizováni vytváření typů entit bez použití klíčů.</span><span class="sxs-lookup"><span data-stu-id="4594e-132">This characteristic makes them an excellent fit for mapping database views in most scenarios, so we automated the creation of entity types without keys when reverse engineering database views.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="4594e-133">Závislé entity, které sdílí tabulku s objektem zabezpečení, jsou teď volitelné.</span><span class="sxs-lookup"><span data-stu-id="4594e-133">Dependent entities sharing the table with the principal are now optional</span></span>

<span data-ttu-id="4594e-134">Počínaje EF Core 3,0, pokud `OrderDetails` je `Order` vlastněná nebo explicitně namapovaná na stejnou tabulku, bude možné přidat `Order` bez `OrderDetails` a všechny `OrderDetails` vlastnosti, s výjimkou, že primární klíč bude namapován na sloupce s možnou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="4594e-134">Starting with EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table, it will be possible to add an `Order` without an `OrderDetails` and all of the `OrderDetails` properties, except the primary key will be mapped to nullable columns.</span></span>

<span data-ttu-id="4594e-135">Při dotazování se EF Core nastaví `OrderDetails` na `null` , pokud některá z jejích požadovaných vlastností nemá hodnotu, nebo pokud se kromě primárního `null`klíče a všech vlastností nevyžadují žádné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="4594e-135">When querying, EF Core will set `OrderDetails` to `null` if any of its required properties doesn't have a value, or if it has no required properties besides the primary key and all properties are `null`.</span></span>

``` csharp
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

## <a name="ef-63-on-net-core"></a><span data-ttu-id="4594e-136">EF 6,3 pro .NET Core</span><span class="sxs-lookup"><span data-stu-id="4594e-136">EF 6.3 on .NET Core</span></span>

<span data-ttu-id="4594e-137">Chápeme, že mnoho existujících aplikací používá předchozí verze EF a že je bude EF Core jenom k tomu, aby využila výhody .NET Core, může někdy vyžadovat značné úsilí.</span><span class="sxs-lookup"><span data-stu-id="4594e-137">We understand that many existing applications use previous versions of EF, and that porting them to EF Core only to take advantage of .NET Core can sometimes require a significant effort.</span></span>
<span data-ttu-id="4594e-138">Z tohoto důvodu jsme povolili použití verze newewst EF 6 v .NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="4594e-138">For that reason, we have enabled the newewst version of EF 6 to run on .NET Core 3.0.</span></span>
<span data-ttu-id="4594e-139">Existují určitá omezení, například:</span><span class="sxs-lookup"><span data-stu-id="4594e-139">There are some limitations, for example:</span></span>
- <span data-ttu-id="4594e-140">Pro práci na .NET Core se vyžadují Noví poskytovatelé.</span><span class="sxs-lookup"><span data-stu-id="4594e-140">New providers are required to work on .NET Core</span></span>
- <span data-ttu-id="4594e-141">Prostorová podpora v SQL Server nebude povolena.</span><span class="sxs-lookup"><span data-stu-id="4594e-141">Spatial support with SQL Server won't be enabled</span></span>

## <a name="postponed-features"></a><span data-ttu-id="4594e-142">Odložené funkce</span><span class="sxs-lookup"><span data-stu-id="4594e-142">Postponed features</span></span>

<span data-ttu-id="4594e-143">Některé funkce původně plánované pro EF Core 3,0 byly odloženy do budoucích verzí:</span><span class="sxs-lookup"><span data-stu-id="4594e-143">Some features originally planned for EF Core 3.0 were postponed to future releases:</span></span> 

- <span data-ttu-id="4594e-144">Možnost ingore části modelu v migracích, které jsou sledovány jako [#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725).</span><span class="sxs-lookup"><span data-stu-id="4594e-144">Ability to ingore parts of a model in migrations, tracked as [#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725).</span></span>
- <span data-ttu-id="4594e-145">Entity kontejneru objektů a dat, které se sledují jako dva samostatné problémy: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) o entitách Shared type a [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) o podpoře mapování indexovaných vlastností.</span><span class="sxs-lookup"><span data-stu-id="4594e-145">Property bag entities, tracked as two separate issues: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) about shared-type entities and [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) about indexed property mapping support.</span></span>
