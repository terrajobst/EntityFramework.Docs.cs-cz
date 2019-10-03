---
title: Nové funkce v Entity Framework Core 3,0
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/index
ms.openlocfilehash: ce53d0fa21acfd52dc5e8b37911202cab02f00c8
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813464"
---
# <a name="new-features-in-entity-framework-core-30"></a><span data-ttu-id="3aac8-102">Nové funkce v Entity Framework Core 3,0</span><span class="sxs-lookup"><span data-stu-id="3aac8-102">New features in Entity Framework Core 3.0</span></span>

<span data-ttu-id="3aac8-103">Následující seznam obsahuje hlavní nové funkce EF Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="3aac8-103">The following list includes the major new features in EF Core 3.0.</span></span>

<span data-ttu-id="3aac8-104">V případě hlavní verze EF Core 3,0 také obsahuje několik podstatných [změn](xref:core/what-is-new/ef-core-3.0/breaking-changes), což jsou vylepšení rozhraní API, která mohou mít negativní dopad na existující aplikace.</span><span class="sxs-lookup"><span data-stu-id="3aac8-104">As a major release, EF Core 3.0 also contains several [breaking changes](xref:core/what-is-new/ef-core-3.0/breaking-changes), which are API improvements that may have negative impact on existing applications.</span></span>  

## <a name="linq-overhaul"></a><span data-ttu-id="3aac8-105">Renovace LINQ</span><span class="sxs-lookup"><span data-stu-id="3aac8-105">LINQ overhaul</span></span> 

<span data-ttu-id="3aac8-106">LINQ umožňuje psát databázové dotazy pomocí jazyka .NET, který si zvolíte, a využít bohatých informací o typech k tomu, aby nabízel IntelliSense a kontrolu typu při kompilaci.</span><span class="sxs-lookup"><span data-stu-id="3aac8-106">LINQ enables you to write database queries using your .NET language of choice, taking advantage of rich type information to offer IntelliSense and compile-time type checking.</span></span>
<span data-ttu-id="3aac8-107">Ale LINQ také umožňuje napsat neomezený počet složitých dotazů, které obsahují libovolné výrazy (volání metod nebo operace).</span><span class="sxs-lookup"><span data-stu-id="3aac8-107">But LINQ also enables you to write an unlimited number of complicated queries containing arbitrary expressions (method calls or operations).</span></span>
<span data-ttu-id="3aac8-108">Způsob zpracování všech těchto kombinací je hlavní výzvou pro poskytovatele LINQ.</span><span class="sxs-lookup"><span data-stu-id="3aac8-108">How to handle all those combinations is the main challenge for LINQ providers.</span></span>

<span data-ttu-id="3aac8-109">V EF Core 3,0 jsme od našeho poskytovatele LINQ převedli překlady dalších vzorů dotazů do SQL, generování efektivních dotazů ve více případech a zabráníte nezjištěným neefektivním dotazům.</span><span class="sxs-lookup"><span data-stu-id="3aac8-109">In EF Core 3.0, we rearchitected our LINQ provider to enable translating more query patterns into SQL, generating efficient queries in more cases, and preventing inefficient queries from going undetected.</span></span> <span data-ttu-id="3aac8-110">Nový poskytovatel LINQ je základem, ve kterém budeme moci nabízet nové možnosti dotazů a vylepšení výkonu v budoucích verzích, aniž byste museli přerušit stávající aplikace a poskytovatele dat.</span><span class="sxs-lookup"><span data-stu-id="3aac8-110">The new LINQ provider is the foundation over which we'll be able to offer new query capabilities and performance improvements in future releases, without breaking existing applications and data providers.</span></span>

### <a name="restricted-client-evaluation"></a><span data-ttu-id="3aac8-111">Omezené Hodnocení klientů</span><span class="sxs-lookup"><span data-stu-id="3aac8-111">Restricted client evaluation</span></span>
<span data-ttu-id="3aac8-112">Nejdůležitější změnou návrhu je udělat, jak zpracováváme LINQ výrazy, které se nedají převést na parametry nebo přeložit na SQL.</span><span class="sxs-lookup"><span data-stu-id="3aac8-112">The most important design change has to do with how we handle LINQ expressions that cannot be converted to parameters or translated to SQL.</span></span>

<span data-ttu-id="3aac8-113">V předchozích verzích EF Core identifikovaly, které části dotazu by mohly být přeloženy na SQL, a spustilo zbytek dotazu na klientovi.</span><span class="sxs-lookup"><span data-stu-id="3aac8-113">In previous versions, EF Core identified what portions of a query could be translated to SQL, and executed the rest of the query on the client.</span></span>
<span data-ttu-id="3aac8-114">Tento typ spuštění na straně klienta je v některých situacích žádoucí, ale v mnoha dalších případech může docházet k neefektivním dotazům.</span><span class="sxs-lookup"><span data-stu-id="3aac8-114">This type of client-side execution is desirable in some situations, but in many other cases it can result in inefficient queries.</span></span>

<span data-ttu-id="3aac8-115">Například pokud EF Core 2,2 nemohl přeložit predikát ve `Where()` volání, provedl příkaz SQL bez filtru, přenesl z databáze všechny řádky a pak je vyfiltroval v paměti:</span><span class="sxs-lookup"><span data-stu-id="3aac8-115">For example, if EF Core 2.2 couldn't translate a predicate in a `Where()` call, it executed an SQL statement without a filter, transferred all the rows from the database, and then filtered them in-memory:</span></span>

``` csharp
var specialCustomers = 
  context.Customers
    .Where(c => c.Name.StartsWith(n) && IsSpecialCustomer(c));
```

<span data-ttu-id="3aac8-116">To může být přijatelné, pokud databáze obsahuje malý počet řádků, ale může způsobit významné problémy s výkonem nebo i selhání aplikace, pokud databáze obsahuje velké číslo nebo řádky.</span><span class="sxs-lookup"><span data-stu-id="3aac8-116">That may be acceptable if the database contains a small number of rows but can result in significant performance issues or even application failure if the database contains a large number or rows.</span></span>

<span data-ttu-id="3aac8-117">V EF Core 3,0 jsme omezili Hodnocení klientů jenom na projekci nejvyšší úrovně (v podstatě na poslední volání `Select()`).</span><span class="sxs-lookup"><span data-stu-id="3aac8-117">In EF Core 3.0, we've restricted client evaluation to only happen on the top-level projection (essentially, the last call to `Select()`).</span></span>
<span data-ttu-id="3aac8-118">Když EF Core 3,0 detekuje výrazy, které nelze přeložit nikde jinde v dotazu, vyvolá výjimku za běhu.</span><span class="sxs-lookup"><span data-stu-id="3aac8-118">When EF Core 3.0 detects expressions that can't be translated anywhere else in the query, it throws a runtime exception.</span></span>

<span data-ttu-id="3aac8-119">Aby vývojáři mohli vyhodnotit podmínku predikátu na klientovi jako v předchozím příkladu, je teď potřeba explicitně přepínat vyhodnocení dotazu na LINQ to Objects:</span><span class="sxs-lookup"><span data-stu-id="3aac8-119">To evaluate a predicate condition on the client as in the previous example, developers now need to explicitly switch evaluation of the query to LINQ to Objects:</span></span> 

``` csharp
var specialCustomers =
  context.Customers
    .Where(c => c.Name.StartsWith(n)) 
    .AsEnumerable() // switches to LINQ to Objects
    .Where(c => IsSpecialCustomer(c));
```

<span data-ttu-id="3aac8-120">Další podrobnosti o tom, jak to může ovlivnit existující aplikace, najdete v [dokumentaci k přerušující změny](xref:core/what-is-new/ef-core-3.0/breaking-changes#linq-queries-are-no-longer-evaluated-on-the-client) .</span><span class="sxs-lookup"><span data-stu-id="3aac8-120">See the [breaking changes documentation](xref:core/what-is-new/ef-core-3.0/breaking-changes#linq-queries-are-no-longer-evaluated-on-the-client) for more details about how this can affect existing applications.</span></span>

### <a name="single-sql-statement-per-linq-query"></a><span data-ttu-id="3aac8-121">Jeden příkaz SQL na dotaz LINQ</span><span class="sxs-lookup"><span data-stu-id="3aac8-121">Single SQL statement per LINQ query</span></span>

<span data-ttu-id="3aac8-122">Dalším aspektem návrhu, který se významně změnil v 3,0, je, že nyní vždy generujeme jeden příkaz SQL na dotaz LINQ.</span><span class="sxs-lookup"><span data-stu-id="3aac8-122">Another aspect of the design that changed significantly in 3.0 is that we now always generate a single SQL statement per LINQ query.</span></span> <span data-ttu-id="3aac8-123">V předchozích verzích jsme v určitých případech vygenerovali několik příkazů SQL, jako je třeba překládat `Include()` volání na vlastnosti navigace kolekce a překládat dotazy, které za určité vzory používají poddotazy.</span><span class="sxs-lookup"><span data-stu-id="3aac8-123">In previous versions, we used to generate multiple SQL statements in certain cases, like to translate `Include()` calls on collection navigation properties and to translate queries that followed certain patterns with subqueries.</span></span> <span data-ttu-id="3aac8-124">I když je to v některých případech pohodlné a pro `Include()` it i pomoc při Vyhněte se posílání redundantních dat po drátě, implementace byla složitá, což vedlo k nějakým mimořádně neefektivnímu chování (N + 1 dotazů) a v situacích, ve kterých se data vrácená mezi více dotazy mohou být nekonzistentní.</span><span class="sxs-lookup"><span data-stu-id="3aac8-124">Although this was in some cases convenient, and for `Include()` it even helped avoid sending redundant data over the wire, the implementation was complex, it resulted in some extremely inefficient behaviors (N+1 queries), and there was situations in which the data returned across multiple queries could be inconsistent.</span></span>

<span data-ttu-id="3aac8-125">Podobně jako u hodnocení klienta, pokud EF Core 3,0 nemůže přeložit dotaz LINQ na jeden příkaz jazyka SQL, vyvolá výjimku za běhu.</span><span class="sxs-lookup"><span data-stu-id="3aac8-125">Similarly to client evaluation, if EF Core 3.0 can't translate a LINQ query into a single SQL statement, it throws a runtime exception.</span></span> <span data-ttu-id="3aac8-126">Ale nastavili jsme EF Core schopné přeložit mnoho běžných vzorů, které se používají ke generování několika dotazů na jeden dotaz s spojeními.</span><span class="sxs-lookup"><span data-stu-id="3aac8-126">But we made EF Core capable of translating many of the common patterns that used to generate multiple queries to a single query with JOINs.</span></span>

## <a name="cosmos-db-support"></a><span data-ttu-id="3aac8-127">Podpora Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="3aac8-127">Cosmos DB support</span></span> 

<span data-ttu-id="3aac8-128">Poskytovatel Cosmos DB pro EF Core umožňuje vývojářům, kteří znají model programu EF, snadno cílit Azure Cosmos DB jako databázi aplikace.</span><span class="sxs-lookup"><span data-stu-id="3aac8-128">The Cosmos DB provider for EF Core enables developers familiar with the EF programing model to easily target Azure Cosmos DB as an application database.</span></span> <span data-ttu-id="3aac8-129">Cílem je udělat si některé z výhod Cosmos DB, jako je globální distribuce, "Always On", pružná škálovatelnost a nízká latence, dokonce i přístup k vývojářům v rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="3aac8-129">The goal is to make some of the advantages of Cosmos DB, like global distribution, "always on" availability, elastic scalability, and low latency, even more accessible to .NET developers.</span></span> <span data-ttu-id="3aac8-130">Zprostředkovatel umožňuje většinu funkcí EF Core, jako je automatické sledování změn, LINQ a převod hodnot, oproti rozhraní SQL API v Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="3aac8-130">The provider enables most EF Core features, like automatic change tracking, LINQ, and value conversions, against the SQL API in Cosmos DB.</span></span>

<span data-ttu-id="3aac8-131">Další podrobnosti najdete v [dokumentaci poskytovatele Cosmos DB](xref:core/providers/cosmos/index) .</span><span class="sxs-lookup"><span data-stu-id="3aac8-131">See the [Cosmos DB provider documentation](xref:core/providers/cosmos/index) for more details.</span></span>

## <a name="c-80-support"></a><span data-ttu-id="3aac8-132">C#podpora 8,0</span><span class="sxs-lookup"><span data-stu-id="3aac8-132">C# 8.0 support</span></span>

<span data-ttu-id="3aac8-133">EF Core 3,0 využívá několik [nových funkcí v C# 8,0](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8):</span><span class="sxs-lookup"><span data-stu-id="3aac8-133">EF Core 3.0 takes advantage of a couple of the [new features in C# 8.0](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8):</span></span>

### <a name="asynchronous-streams"></a><span data-ttu-id="3aac8-134">Asynchronní proudy</span><span class="sxs-lookup"><span data-stu-id="3aac8-134">Asynchronous streams</span></span>

<span data-ttu-id="3aac8-135">Výsledky asynchronního dotazu jsou nyní zpřístupněny pomocí nového `IAsyncEnumerable<T>` standardního rozhraní a lze je spotřebovat pomocí. `await foreach`</span><span class="sxs-lookup"><span data-stu-id="3aac8-135">Asynchronous query results are now exposed using the new standard `IAsyncEnumerable<T>` interface and can be consumed using `await foreach`.</span></span>

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

<span data-ttu-id="3aac8-136">Další podrobnosti najdete [v tématu asynchronní C# datové proudy v dokumentaci](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8#asynchronous-streams) .</span><span class="sxs-lookup"><span data-stu-id="3aac8-136">See the [asynchronous streams in the C# documentation](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8#asynchronous-streams) for more details.</span></span>

### <a name="nullable-reference-types"></a><span data-ttu-id="3aac8-137">Odkazové typy s možnou hodnotou null</span><span class="sxs-lookup"><span data-stu-id="3aac8-137">Nullable reference types</span></span> 

<span data-ttu-id="3aac8-138">Pokud je tato nová funkce ve vašem kódu povolená, EF Core prověřuje vlastnost s hodnotou null vlastností typu odkazu a použije ji pro odpovídající sloupce a relace v databázi: vlastnosti odkazů na typy, které neumožňují hodnotu null, se považují za, jako kdyby měly `[Required]` atribut datové poznámky</span><span class="sxs-lookup"><span data-stu-id="3aac8-138">When this new feature is enabled in your code, EF Core examines the nullability of reference type properties and applies it to corresponding columns and relationships in the database: properties of non-nullable references types are treated as if they had the `[Required]` data annotation attribute.</span></span>

<span data-ttu-id="3aac8-139">Například v následující třídě jsou vlastnosti označené jako typ `string?` konfigurovány jako volitelné, zatímco `string` budou nakonfigurovány jako povinné:</span><span class="sxs-lookup"><span data-stu-id="3aac8-139">For example, in the following class, properties marked as of type `string?` will be configured as optional, whereas `string` will be configured as required:</span></span>

``` csharp
public class Customer
{
  public int Id { get; set; }
  public string FirstName { get; set; }
  public string LastName { get; set; }
  public string? MiddleName { get; set; }
}
```

<span data-ttu-id="3aac8-140">Další podrobnosti najdete v tématu [práce s typy odkazů s možnou hodnotou null](xref:core/miscellaneous/nullable-reference-types) v dokumentaci k EF Core.</span><span class="sxs-lookup"><span data-stu-id="3aac8-140">See [Working with nullable reference types](xref:core/miscellaneous/nullable-reference-types) in the EF Core documentation for more details.</span></span>

## <a name="interception-of-database-operations"></a><span data-ttu-id="3aac8-141">Zachycení databázových operací</span><span class="sxs-lookup"><span data-stu-id="3aac8-141">Interception of database operations</span></span>

<span data-ttu-id="3aac8-142">Nové rozhraní API zachycení v EF Core 3,0 umožňuje, aby se vlastní logika vyvolala automaticky, kdykoli dojde k provozu databáze nízké úrovně jako součást běžné operace EF Core.</span><span class="sxs-lookup"><span data-stu-id="3aac8-142">The new interception API in EF Core 3.0 allows providing custom logic to be invoked automatically whenever low-level database operations occur as part of the normal operation of EF Core.</span></span> <span data-ttu-id="3aac8-143">Například při otevírání připojení, potvrzování transakcí nebo provádění příkazů.</span><span class="sxs-lookup"><span data-stu-id="3aac8-143">For example, when opening connections, committing transactions, or executing commands.</span></span> 

<span data-ttu-id="3aac8-144">Podobně jako funkce zachycení, které existovaly v EF 6, můžou zachytit operace zachycení operací před nebo po nich.</span><span class="sxs-lookup"><span data-stu-id="3aac8-144">Similarly to the interception features that existed in EF 6, interceptors allow you to intercept operations before or after they happen.</span></span> <span data-ttu-id="3aac8-145">Pokud je zachytíte předtím, než k nim dojde, můžete provést překonání a zadat alternativní výsledky z logiky zachycení.</span><span class="sxs-lookup"><span data-stu-id="3aac8-145">When you intercept them before they happen, you are allowed to by-pass execution and supply alternate results from the interception logic.</span></span> 

<span data-ttu-id="3aac8-146">Například pro manipulaci s textem příkazu můžete vytvořit `IDbCommandInterceptor`:</span><span class="sxs-lookup"><span data-stu-id="3aac8-146">For example, to manipulate command text, you can create an `IDbCommandInterceptor`:</span></span>

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

<span data-ttu-id="3aac8-147">A zaregistrujte ji `DbContext`:</span><span class="sxs-lookup"><span data-stu-id="3aac8-147">And register it with your `DbContext`:</span></span>

``` csharp
services.AddDbContext(b => b
  .UseSqlServer(connectionString)
  .AddInterceptors(new HintCommandInterceptor()));
```

## <a name="reverse-engineering-of-database-views"></a><span data-ttu-id="3aac8-148">Zpětná analýza zobrazení databáze</span><span class="sxs-lookup"><span data-stu-id="3aac8-148">Reverse engineering of database views</span></span>

<span data-ttu-id="3aac8-149">Typy dotazů, které reprezentují data, která je možné číst z databáze, ale nejsou aktualizované, byly přejmenovány na [bez klíčů typu entity](xref:core/modeling/keyless-entity-types).</span><span class="sxs-lookup"><span data-stu-id="3aac8-149">Query types, which represent data that can be read from the database but not updated, have been renamed to [keyless entity types](xref:core/modeling/keyless-entity-types).</span></span> <span data-ttu-id="3aac8-150">Vzhledem k tomu, že se jedná o vynikající možnosti mapování zobrazení databáze ve většině scénářů, EF Core nyní automaticky vytvoří typy entit bez klíčů při zpětné analýze zobrazení databáze.</span><span class="sxs-lookup"><span data-stu-id="3aac8-150">As they are an excellent fit for mapping database views in most scenarios, EF Core now automatically creates keyless entity types when reverse engineering database views.</span></span>

<span data-ttu-id="3aac8-151">Například pomocí [nástroje příkazového řádku dotnet EF](xref:core/miscellaneous/cli/dotnet) můžete zadat:</span><span class="sxs-lookup"><span data-stu-id="3aac8-151">For example, using the [dotnet ef command-line tool](xref:core/miscellaneous/cli/dotnet) you can type:</span></span>

``` console
dotnet ef dbcontext scaffold "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="3aac8-152">A nástroj nyní automaticky vytvoří pro zobrazení a tabulky bez klíčů následující typy generování uživatelského rozhraní:</span><span class="sxs-lookup"><span data-stu-id="3aac8-152">And the tool will now automatically scaffold types for views and tables without keys:</span></span>

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

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="3aac8-153">Závislé entity, které sdílí tabulku s objektem zabezpečení, jsou teď volitelné.</span><span class="sxs-lookup"><span data-stu-id="3aac8-153">Dependent entities sharing the table with the principal are now optional</span></span>

<span data-ttu-id="3aac8-154">Počínaje EF Core 3,0, pokud `OrderDetails` je `Order` vlastněná nebo explicitně namapovaná na stejnou tabulku, bude možné přidat `Order` bez `OrderDetails` a všechny `OrderDetails` vlastnosti, s výjimkou, že primární klíč bude namapován na sloupce s možnou hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="3aac8-154">Starting with EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table, it will be possible to add an `Order` without an `OrderDetails` and all of the `OrderDetails` properties, except the primary key will be mapped to nullable columns.</span></span>

<span data-ttu-id="3aac8-155">Při dotazování se EF Core nastaví `OrderDetails` na `null` , pokud některá z jejích požadovaných vlastností nemá hodnotu, nebo pokud se kromě primárního `null`klíče a všech vlastností nevyžadují žádné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="3aac8-155">When querying, EF Core will set `OrderDetails` to `null` if any of its required properties doesn't have a value, or if it has no required properties besides the primary key and all properties are `null`.</span></span>

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

## <a name="ef-63-on-net-core"></a><span data-ttu-id="3aac8-156">EF 6,3 pro .NET Core</span><span class="sxs-lookup"><span data-stu-id="3aac8-156">EF 6.3 on .NET Core</span></span>

<span data-ttu-id="3aac8-157">Nejedná se skutečně o funkci EF Core 3,0, ale myslíme si, že je důležité mít mnoho našich současných zákazníků.</span><span class="sxs-lookup"><span data-stu-id="3aac8-157">This isn't really an EF Core 3.0 feature, but we think it is important to many of our current customers.</span></span> 

<span data-ttu-id="3aac8-158">Chápeme, že mnoho existujících aplikací používá předchozí verze EF a že je bude EF Core jenom k tomu, aby využila výhody .NET Core, může vyžadovat značné úsilí.</span><span class="sxs-lookup"><span data-stu-id="3aac8-158">We understand that many existing applications use previous versions of EF, and that porting them to EF Core only to take advantage of .NET Core can require a significant effort.</span></span> <span data-ttu-id="3aac8-159">Z tohoto důvodu jsme se rozhodli vystavit nejnovější verzi EF 6, která bude běžet v .NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="3aac8-159">For that reason, we decided to port the newest version of EF 6 to run on .NET Core 3.0.</span></span> 

<span data-ttu-id="3aac8-160">Další podrobnosti najdete v tématu [co je nového v EF 6](xref:ef6/what-is-new/index).</span><span class="sxs-lookup"><span data-stu-id="3aac8-160">For more details, see [what's new in EF 6](xref:ef6/what-is-new/index).</span></span>

## <a name="postponed-features"></a><span data-ttu-id="3aac8-161">Odložené funkce</span><span class="sxs-lookup"><span data-stu-id="3aac8-161">Postponed features</span></span>

<span data-ttu-id="3aac8-162">Některé funkce původně plánované pro EF Core 3,0 byly odloženy do budoucích verzí:</span><span class="sxs-lookup"><span data-stu-id="3aac8-162">Some features originally planned for EF Core 3.0 were postponed to future releases:</span></span>

- <span data-ttu-id="3aac8-163">Možnost Ignorovat části modelu v migracích, které jsou sledovány jako [#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725).</span><span class="sxs-lookup"><span data-stu-id="3aac8-163">Ability to ignore parts of a model in migrations, tracked as [#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725).</span></span>
- <span data-ttu-id="3aac8-164">Entity kontejneru objektů a dat, které se sledují jako dva samostatné problémy: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) o entitách Shared type a [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) o podpoře mapování indexovaných vlastností.</span><span class="sxs-lookup"><span data-stu-id="3aac8-164">Property bag entities, tracked as two separate issues: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) about shared-type entities and [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) about indexed property mapping support.</span></span>
