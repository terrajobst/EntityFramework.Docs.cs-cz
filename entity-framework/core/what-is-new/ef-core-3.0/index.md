---
title: Nové funkce v core entity frameworku 3.0 – EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/index
ms.openlocfilehash: ebc676930ffc396aa70bb8afb91cf5a0cd43e04d
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417942"
---
# <a name="new-features-in-entity-framework-core-30"></a><span data-ttu-id="44980-102">Nové funkce v jádru entity frameworku 3.0</span><span class="sxs-lookup"><span data-stu-id="44980-102">New features in Entity Framework Core 3.0</span></span>

<span data-ttu-id="44980-103">Následující seznam obsahuje hlavní nové funkce v EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="44980-103">The following list includes the major new features in EF Core 3.0.</span></span>

<span data-ttu-id="44980-104">Jako hlavní verze EF Core 3.0 obsahuje také několik [narušující změny](xref:core/what-is-new/ef-core-3.0/breaking-changes), které jsou vylepšení rozhraní API, které mohou mít negativní dopad na stávající aplikace.</span><span class="sxs-lookup"><span data-stu-id="44980-104">As a major release, EF Core 3.0 also contains several [breaking changes](xref:core/what-is-new/ef-core-3.0/breaking-changes), which are API improvements that may have negative impact on existing applications.</span></span>  

## <a name="linq-overhaul"></a><span data-ttu-id="44980-105">Generální oprava LINQ</span><span class="sxs-lookup"><span data-stu-id="44980-105">LINQ overhaul</span></span>

<span data-ttu-id="44980-106">LINQ umožňuje psát databázové dotazy pomocí zvoleného jazyka .NET s využitím rozšířených informací o typu a nabízí kontrolu typu IntelliSense a kompilace.</span><span class="sxs-lookup"><span data-stu-id="44980-106">LINQ enables you to write database queries using your .NET language of choice, taking advantage of rich type information to offer IntelliSense and compile-time type checking.</span></span>
<span data-ttu-id="44980-107">Ale LINQ také umožňuje psát neomezený počet složitých dotazů obsahujících libovolné výrazy (volání metod nebo operace).</span><span class="sxs-lookup"><span data-stu-id="44980-107">But LINQ also enables you to write an unlimited number of complicated queries containing arbitrary expressions (method calls or operations).</span></span>
<span data-ttu-id="44980-108">Jak zpracovat všechny tyto kombinace je hlavní výzvou pro poskytovatele LINQ.</span><span class="sxs-lookup"><span data-stu-id="44980-108">How to handle all those combinations is the main challenge for LINQ providers.</span></span>

<span data-ttu-id="44980-109">V EF Core 3.0 jsme překontovali našeho poskytovatele LINQ, abychom umožnili překlad dalších vzorů dotazů do SQL, generování efektivních dotazů ve více případech a zabránění neefektivním dotazům, aby nezjistili.</span><span class="sxs-lookup"><span data-stu-id="44980-109">In EF Core 3.0, we rearchitected our LINQ provider to enable translating more query patterns into SQL, generating efficient queries in more cases, and preventing inefficient queries from going undetected.</span></span> <span data-ttu-id="44980-110">Nový poskytovatel LINQ je základem, nad kterým budeme moci nabídnout nové možnosti dotazu a vylepšení výkonu v budoucích verzích, aniž bychom porušili stávající aplikace a poskytovatele dat.</span><span class="sxs-lookup"><span data-stu-id="44980-110">The new LINQ provider is the foundation over which we'll be able to offer new query capabilities and performance improvements in future releases, without breaking existing applications and data providers.</span></span>

### <a name="restricted-client-evaluation"></a><span data-ttu-id="44980-111">Omezené vyhodnocení klienta</span><span class="sxs-lookup"><span data-stu-id="44980-111">Restricted client evaluation</span></span>

<span data-ttu-id="44980-112">Nejdůležitější změna návrhu má co do činění s tím, jak zpracováváme výrazy LINQ, které nelze převést na parametry nebo přeložit do SQL.</span><span class="sxs-lookup"><span data-stu-id="44980-112">The most important design change has to do with how we handle LINQ expressions that cannot be converted to parameters or translated to SQL.</span></span>

<span data-ttu-id="44980-113">V předchozích verzích EF Core identifikoval, jaké části dotazu by mohly být přeloženy do SQL a provedeny zbytek dotazu na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="44980-113">In previous versions, EF Core identified what portions of a query could be translated to SQL, and executed the rest of the query on the client.</span></span>
<span data-ttu-id="44980-114">Tento typ spuštění na straně klienta je žádoucí v některých situacích, ale v mnoha jiných případech může mít za následek neefektivní dotazy.</span><span class="sxs-lookup"><span data-stu-id="44980-114">This type of client-side execution is desirable in some situations, but in many other cases it can result in inefficient queries.</span></span>

<span data-ttu-id="44980-115">Například pokud EF Core 2.2 nelze přeložit predikát ve `Where()` volání, provedl příkaz SQL bez filtru, převedl všechny řádky z databáze a pak je filtroval v paměti:</span><span class="sxs-lookup"><span data-stu-id="44980-115">For example, if EF Core 2.2 couldn't translate a predicate in a `Where()` call, it executed an SQL statement without a filter, transferred all the rows from the database, and then filtered them in-memory:</span></span>

``` csharp
var specialCustomers = context.Customers
    .Where(c => c.Name.StartsWith(n) && IsSpecialCustomer(c));
```

<span data-ttu-id="44980-116">To může být přijatelné, pokud databáze obsahuje malý počet řádků, ale může mít za následek významné problémy s výkonem nebo dokonce selhání aplikace, pokud databáze obsahuje velký počet řádků.</span><span class="sxs-lookup"><span data-stu-id="44980-116">That may be acceptable if the database contains a small number of rows but can result in significant performance issues or even application failure if the database contains a large number of rows.</span></span>

<span data-ttu-id="44980-117">V EF Core 3.0 jsme omezili hodnocení klientů, aby se stalo pouze v `Select()`projekci nejvyšší úrovně (v podstatě poslední volání).</span><span class="sxs-lookup"><span data-stu-id="44980-117">In EF Core 3.0, we've restricted client evaluation to only happen on the top-level projection (essentially, the last call to `Select()`).</span></span>
<span data-ttu-id="44980-118">Když EF Core 3.0 zjistí výrazy, které nelze přeložit nikde jinde v dotazu, vyvolá výjimku za běhu.</span><span class="sxs-lookup"><span data-stu-id="44980-118">When EF Core 3.0 detects expressions that can't be translated anywhere else in the query, it throws a runtime exception.</span></span>

<span data-ttu-id="44980-119">Chcete-li vyhodnotit podmínku predikátu na straně klienta jako v předchozím příkladu, vývojáři nyní musí explicitně přepnout vyhodnocení dotazu na LINQ na objekty:</span><span class="sxs-lookup"><span data-stu-id="44980-119">To evaluate a predicate condition on the client as in the previous example, developers now need to explicitly switch evaluation of the query to LINQ to Objects:</span></span>

``` csharp
var specialCustomers = context.Customers
    .Where(c => c.Name.StartsWith(n))
    .AsEnumerable() // switches to LINQ to Objects
    .Where(c => IsSpecialCustomer(c));
```

<span data-ttu-id="44980-120">Další [podrobnosti](xref:core/what-is-new/ef-core-3.0/breaking-changes#linq-queries-are-no-longer-evaluated-on-the-client) o tom, jak to může ovlivnit stávající aplikace, naleznete v dokumentaci k nejnovějším změnám.</span><span class="sxs-lookup"><span data-stu-id="44980-120">See the [breaking changes documentation](xref:core/what-is-new/ef-core-3.0/breaking-changes#linq-queries-are-no-longer-evaluated-on-the-client) for more details about how this can affect existing applications.</span></span>

### <a name="single-sql-statement-per-linq-query"></a><span data-ttu-id="44980-121">Jeden příkaz SQL na dotaz LINQ</span><span class="sxs-lookup"><span data-stu-id="44980-121">Single SQL statement per LINQ query</span></span>

<span data-ttu-id="44980-122">Dalším aspektem návrhu, který se výrazně změnil v 3.0 je, že nyní vždy generovat jeden příkaz SQL na linq dotazu.</span><span class="sxs-lookup"><span data-stu-id="44980-122">Another aspect of the design that changed significantly in 3.0 is that we now always generate a single SQL statement per LINQ query.</span></span> <span data-ttu-id="44980-123">V předchozích verzích jsme v určitých případech generovali více příkazů SQL, překládali `Include()` volání navigačních vlastností kolekce a překládali dotazy, které sledovaly určité vzory s poddotazy.</span><span class="sxs-lookup"><span data-stu-id="44980-123">In previous versions, we used to generate multiple SQL statements in certain cases, translated `Include()` calls on collection navigation properties and translated queries that followed certain patterns with subqueries.</span></span> <span data-ttu-id="44980-124">I když to bylo v `Include()` některých případech pohodlné a dokonce pomohlo vyhnout se odesílání nadbytečných dat po drátě, implementace byla složitá a vyústila v některé extrémně neefektivní chování (dotazy N + 1).</span><span class="sxs-lookup"><span data-stu-id="44980-124">Although this was in some cases convenient, and for `Include()` it even helped avoid sending redundant data over the wire, the implementation was complex, and it resulted in some extremely inefficient behaviors (N+1 queries).</span></span> <span data-ttu-id="44980-125">Byly situace, ve kterých data vrácená přes více dotazů byla potenciálně nekonzistentní.</span><span class="sxs-lookup"><span data-stu-id="44980-125">There were situations in which the data returned across multiple queries was potentially inconsistent.</span></span>

<span data-ttu-id="44980-126">Podobně jako hodnocení klienta, pokud EF Core 3.0 nelze přeložit linq dotaz do jednoho příkazu SQL, vyvolá výjimku modulu runtime.</span><span class="sxs-lookup"><span data-stu-id="44980-126">Similarly to client evaluation, if EF Core 3.0 can't translate a LINQ query into a single SQL statement, it throws a runtime exception.</span></span> <span data-ttu-id="44980-127">Ale udělali jsme EF Core schopné překládat mnoho běžných vzorů, které slouží ke generování více dotazů na jeden dotaz s JOINs.</span><span class="sxs-lookup"><span data-stu-id="44980-127">But we made EF Core capable of translating many of the common patterns that used to generate multiple queries to a single query with JOINs.</span></span>

## <a name="cosmos-db-support"></a><span data-ttu-id="44980-128">Podpora pro Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="44980-128">Cosmos DB support</span></span>

<span data-ttu-id="44980-129">Zprostředkovatel Cosmos DB pro EF Core umožňuje vývojářům obeznámeným s modelem programování EF snadno cílit na Azure Cosmos DB jako aplikační databázi.</span><span class="sxs-lookup"><span data-stu-id="44980-129">The Cosmos DB provider for EF Core enables developers familiar with the EF programing model to easily target Azure Cosmos DB as an application database.</span></span> <span data-ttu-id="44980-130">Cílem je, aby některé z výhod Cosmos DB, jako je globální distribuce, "vždy na" dostupnost, elastická škálovatelnost a nízká latence, ještě přístupnější pro vývojáře .NET.</span><span class="sxs-lookup"><span data-stu-id="44980-130">The goal is to make some of the advantages of Cosmos DB, like global distribution, "always on" availability, elastic scalability, and low latency, even more accessible to .NET developers.</span></span> <span data-ttu-id="44980-131">Zprostředkovatel umožňuje většinu funkcí EF Core, jako je automatické sledování změn, LINQ a převody hodnot, proti SQL API v Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="44980-131">The provider enables most EF Core features, like automatic change tracking, LINQ, and value conversions, against the SQL API in Cosmos DB.</span></span>

<span data-ttu-id="44980-132">Další podrobnosti najdete v [dokumentaci k poskytovateli Cosmos DB.](xref:core/providers/cosmos/index)</span><span class="sxs-lookup"><span data-stu-id="44980-132">See the [Cosmos DB provider documentation](xref:core/providers/cosmos/index) for more details.</span></span>

## <a name="c-80-support"></a><span data-ttu-id="44980-133">Podpora pro C# 8.0</span><span class="sxs-lookup"><span data-stu-id="44980-133">C# 8.0 support</span></span>

<span data-ttu-id="44980-134">EF Core 3.0 využívá několik [nových funkcí v C# 8.0](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8):</span><span class="sxs-lookup"><span data-stu-id="44980-134">EF Core 3.0 takes advantage of a couple of the [new features in C# 8.0](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8):</span></span>

### <a name="asynchronous-streams"></a><span data-ttu-id="44980-135">Asynchronní datové proudy</span><span class="sxs-lookup"><span data-stu-id="44980-135">Asynchronous streams</span></span>

<span data-ttu-id="44980-136">Výsledky asynchronních dotazů jsou nyní `IAsyncEnumerable<T>` vystaveny pomocí nového `await foreach`standardního rozhraní a mohou být spotřebovány pomocí .</span><span class="sxs-lookup"><span data-stu-id="44980-136">Asynchronous query results are now exposed using the new standard `IAsyncEnumerable<T>` interface and can be consumed using `await foreach`.</span></span>

``` csharp
var orders =
    from o in context.Orders
    where o.Status == OrderStatus.Pending
    select o;

await foreach(var o in orders.AsAsyncEnumerable())
{
    Process(o);
}
```

<span data-ttu-id="44980-137">Další [podrobnosti naleznete v asynchronních proudech v dokumentaci jazyka C#.](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8#asynchronous-streams)</span><span class="sxs-lookup"><span data-stu-id="44980-137">See the [asynchronous streams in the C# documentation](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8#asynchronous-streams) for more details.</span></span>

### <a name="nullable-reference-types"></a><span data-ttu-id="44980-138">Odkazové typy s možnou hodnotou null</span><span class="sxs-lookup"><span data-stu-id="44980-138">Nullable reference types</span></span>

<span data-ttu-id="44980-139">Pokud je tato nová funkce povolena ve vašem kódu, EF Core zkoumá nullability vlastnosti typu odkazu a použije ji na odpovídající sloupce a vztahy `[Required]` v databázi: vlastnosti typů odkazů s nenulovou hodnotou jsou považovány za atribut anotace dat.</span><span class="sxs-lookup"><span data-stu-id="44980-139">When this new feature is enabled in your code, EF Core examines the nullability of reference type properties and applies it to corresponding columns and relationships in the database: properties of non-nullable references types are treated as if they had the `[Required]` data annotation attribute.</span></span>

<span data-ttu-id="44980-140">Například v následující třídě budou vlastnosti `string?` označené jako typ nakonfigurovány jako volitelné, zatímco budou konfigurovány `string` podle potřeby:</span><span class="sxs-lookup"><span data-stu-id="44980-140">For example, in the following class, properties marked as of type `string?` will be configured as optional, whereas `string` will be configured as required:</span></span>

``` csharp
public class Customer
{
    public int Id { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string? MiddleName { get; set; }
}
```

<span data-ttu-id="44980-141">Další podrobnosti naleznete v [tématu Práce s typy odkazů s možnou hodnotou null](xref:core/miscellaneous/nullable-reference-types) v dokumentaci EF Core.</span><span class="sxs-lookup"><span data-stu-id="44980-141">See [Working with nullable reference types](xref:core/miscellaneous/nullable-reference-types) in the EF Core documentation for more details.</span></span>

## <a name="interception-of-database-operations"></a><span data-ttu-id="44980-142">Zachycení databázových operací</span><span class="sxs-lookup"><span data-stu-id="44980-142">Interception of database operations</span></span>

<span data-ttu-id="44980-143">Nové rozhraní API pro zachycení v EF Core 3.0 umožňuje poskytovat vlastní logiku, která má být vyvolána automaticky vždy, když dojde k operacím databáze nižší úrovně jako součást normální operace EF Core.</span><span class="sxs-lookup"><span data-stu-id="44980-143">The new interception API in EF Core 3.0 allows providing custom logic to be invoked automatically whenever low-level database operations occur as part of the normal operation of EF Core.</span></span> <span data-ttu-id="44980-144">Například při otevírání připojení, potvrzení transakcí nebo provádění příkazů.</span><span class="sxs-lookup"><span data-stu-id="44980-144">For example, when opening connections, committing transactions, or executing commands.</span></span>

<span data-ttu-id="44980-145">Podobně jako funkce zachycení, které existovaly v EF 6, zachycovače umožňují zachytit operace před nebo po jejich dosažení.</span><span class="sxs-lookup"><span data-stu-id="44980-145">Similarly to the interception features that existed in EF 6, interceptors allow you to intercept operations before or after they happen.</span></span> <span data-ttu-id="44980-146">Když je zachytíte dříve, než k nim dojde, můžete obcházet spuštění a zadat alternativní výsledky z logiky zachycení.</span><span class="sxs-lookup"><span data-stu-id="44980-146">When you intercept them before they happen, you are allowed to by-pass execution and supply alternate results from the interception logic.</span></span>

<span data-ttu-id="44980-147">Chcete-li například manipulovat s textem `IDbCommandInterceptor`příkazu, můžete vytvořit :</span><span class="sxs-lookup"><span data-stu-id="44980-147">For example, to manipulate command text, you can create an `IDbCommandInterceptor`:</span></span>

``` csharp
public class HintCommandInterceptor : DbCommandInterceptor
{
    public override InterceptionResult ReaderExecuting(
        DbCommand command,
        CommandEventData eventData,
        InterceptionResult result)
    {
        // Manipulate the command text, etc. here...
        command.CommandText += " OPTION (OPTIMIZE FOR UNKNOWN)";
        return result;
    }
}
```

<span data-ttu-id="44980-148">A zaregistrujte `DbContext`jej u svého :</span><span class="sxs-lookup"><span data-stu-id="44980-148">And register it with your `DbContext`:</span></span>

``` csharp
services.AddDbContext(b => b
    .UseSqlServer(connectionString)
    .AddInterceptors(new HintCommandInterceptor()));
```

## <a name="reverse-engineering-of-database-views"></a><span data-ttu-id="44980-149">Reverzní inženýrství zobrazení databáze</span><span class="sxs-lookup"><span data-stu-id="44980-149">Reverse engineering of database views</span></span>

<span data-ttu-id="44980-150">Typy dotazů, které představují data, která lze číst z databáze, ale nejsou aktualizována, byly přejmenovány na [typy bezklíčových entit](xref:core/modeling/keyless-entity-types).</span><span class="sxs-lookup"><span data-stu-id="44980-150">Query types, which represent data that can be read from the database but not updated, have been renamed to [keyless entity types](xref:core/modeling/keyless-entity-types).</span></span>
<span data-ttu-id="44980-151">Vzhledem k tomu, že jsou vynikající pro mapování zobrazení databáze ve většině scénářů, EF Core nyní automaticky vytvoří bezklíčové typy entit při zobrazení databáze zpětné analýzy.</span><span class="sxs-lookup"><span data-stu-id="44980-151">As they are an excellent fit for mapping database views in most scenarios, EF Core now automatically creates keyless entity types when reverse engineering database views.</span></span>

<span data-ttu-id="44980-152">Například pomocí [nástroje příkazového řádku dotnet ef](xref:core/miscellaneous/cli/dotnet) můžete zadat:</span><span class="sxs-lookup"><span data-stu-id="44980-152">For example, using the [dotnet ef command-line tool](xref:core/miscellaneous/cli/dotnet) you can type:</span></span>

```dotnetcli
dotnet ef dbcontext scaffold "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="44980-153">A nástroj bude nyní automaticky přilebené typy pro pohledy a tabulky bez klíčů:</span><span class="sxs-lookup"><span data-stu-id="44980-153">And the tool will now automatically scaffold types for views and tables without keys:</span></span>

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Names>(entity =>
    {
        entity.HasNoKey();
        entity.ToView("Names");
    });

    modelBuilder.Entity<Things>(entity =>
    {
        entity.HasNoKey();
    });
}
```

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="44980-154">Závislé entity sdílející tabulku s objektem zabezpečení jsou nyní volitelné.</span><span class="sxs-lookup"><span data-stu-id="44980-154">Dependent entities sharing the table with the principal are now optional</span></span>

<span data-ttu-id="44980-155">Počínaje EF Core 3.0, `OrderDetails` pokud `Order` je vlastněna nebo explicitně mapována na `Order` stejnou `OrderDetails` tabulku, `OrderDetails` bude možné přidat bez a všechny vlastnosti, s výjimkou primární klíč bude mapován na sloupce s možnou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="44980-155">Starting with EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table, it will be possible to add an `Order` without an `OrderDetails` and all of the `OrderDetails` properties, except the primary key will be mapped to nullable columns.</span></span>

<span data-ttu-id="44980-156">Při dotazování ef core `OrderDetails` `null` nastaví, pokud některé z jeho požadovaných vlastností nemá hodnotu, nebo pokud `null`nemá žádné požadované vlastnosti kromě primárního klíče a všechny vlastnosti jsou .</span><span class="sxs-lookup"><span data-stu-id="44980-156">When querying, EF Core will set `OrderDetails` to `null` if any of its required properties doesn't have a value, or if it has no required properties besides the primary key and all properties are `null`.</span></span>

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

## <a name="ef-63-on-net-core"></a><span data-ttu-id="44980-157">EF 6.3 na jádru rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="44980-157">EF 6.3 on .NET Core</span></span>

<span data-ttu-id="44980-158">Ve skutečnosti se nejedná o funkci EF Core 3.0, ale myslíme si, že je důležitá pro mnoho našich současných zákazníků.</span><span class="sxs-lookup"><span data-stu-id="44980-158">This isn't really an EF Core 3.0 feature, but we think it is important to many of our current customers.</span></span>

<span data-ttu-id="44980-159">Chápeme, že mnoho existujících aplikací používá předchozí verze EF a že jejich přenos na EF Core pouze pro využití .NET Core může vyžadovat značné úsilí.</span><span class="sxs-lookup"><span data-stu-id="44980-159">We understand that many existing applications use previous versions of EF, and that porting them to EF Core only to take advantage of .NET Core can require a significant effort.</span></span>
<span data-ttu-id="44980-160">Z tohoto důvodu jsme se rozhodli portovat nejnovější verzi EF 6 pro spuštění na .NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="44980-160">For that reason, we decided to port the newest version of EF 6 to run on .NET Core 3.0.</span></span>

<span data-ttu-id="44980-161">Další podrobnosti naleznete [v tématu Co je nového v EF 6](xref:ef6/what-is-new/index).</span><span class="sxs-lookup"><span data-stu-id="44980-161">For more details, see [what's new in EF 6](xref:ef6/what-is-new/index).</span></span>

## <a name="postponed-features"></a><span data-ttu-id="44980-162">Odložené funkce</span><span class="sxs-lookup"><span data-stu-id="44980-162">Postponed features</span></span>

<span data-ttu-id="44980-163">Některé funkce původně plánované pro EF Core 3.0 byly odloženy na budoucí verze:</span><span class="sxs-lookup"><span data-stu-id="44980-163">Some features originally planned for EF Core 3.0 were postponed to future releases:</span></span>

- <span data-ttu-id="44980-164">Možnost ignorovat části modelu při migraci, sledována jako [#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725).</span><span class="sxs-lookup"><span data-stu-id="44980-164">Ability to ignore parts of a model in migrations, tracked as [#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725).</span></span>
- <span data-ttu-id="44980-165">Entity vačků vlastností, sledované jako dva samostatné problémy: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) o entitách sdíleného typu a [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) o podpoře mapování indexovaných vlastností.</span><span class="sxs-lookup"><span data-stu-id="44980-165">Property bag entities, tracked as two separate issues: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) about shared-type entities and [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) about indexed property mapping support.</span></span>
