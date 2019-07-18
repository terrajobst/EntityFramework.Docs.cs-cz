---
title: Nezpracované dotazy SQL – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: 91592ea9f7c73f10446993282c1874c852000871
ms.sourcegitcommit: c9c3e00c2d445b784423469838adc071a946e7c9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/18/2019
ms.locfileid: "68306546"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="5c0ff-102">Nezpracované dotazy SQL</span><span class="sxs-lookup"><span data-stu-id="5c0ff-102">Raw SQL Queries</span></span>

<span data-ttu-id="5c0ff-103">Entity Framework Core umožňuje vyřadit z provozu nezpracované dotazy SQL při práci s relační databází.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="5c0ff-104">To může být užitečné v případě, že dotaz, který chcete provést, nelze vyjádřit pomocí LINQ, nebo pokud použití dotazu LINQ má za následek neefektivní dotazy SQL.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-104">This can be useful if the query you want to perform can't be expressed using LINQ, or if using a LINQ query is resulting in inefficient SQL queries.</span></span> <span data-ttu-id="5c0ff-105">Nezpracované dotazy SQL mohou vracet typy entit nebo, počínaje EF Core 2,1, [typy dotazů](xref:core/modeling/query-types) , které jsou součástí vašeho modelu.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-105">Raw SQL queries can return entity types or, starting with EF Core 2.1, [query types](xref:core/modeling/query-types) that are part of your model.</span></span>

> [!TIP]  
> <span data-ttu-id="5c0ff-106">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="5c0ff-107">Základní nezpracované dotazy SQL</span><span class="sxs-lookup"><span data-stu-id="5c0ff-107">Basic raw SQL queries</span></span>

<span data-ttu-id="5c0ff-108">Metodu rozšíření *z tabulek* můžete použít k zahájení dotazu LINQ na základě nezpracovaného dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-108">You can use the *FromSql* extension method to begin a LINQ query based on a raw SQL query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

<span data-ttu-id="5c0ff-109">Nezpracované dotazy SQL lze použít ke spuštění uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-109">Raw SQL queries can be used to execute a stored procedure.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a><span data-ttu-id="5c0ff-110">Předávání parametrů</span><span class="sxs-lookup"><span data-stu-id="5c0ff-110">Passing parameters</span></span>

<span data-ttu-id="5c0ff-111">Stejně jako u libovolného rozhraní API, které podporuje SQL, je důležité parametrizovat libovolný vstup uživatele, aby chránil proti útoku prostřednictvím injektáže SQL.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-111">As with any API that accepts SQL, it is important to parameterize any user input to protect against a SQL injection attack.</span></span> <span data-ttu-id="5c0ff-112">Do řetězce dotazu SQL můžete zahrnout zástupné symboly parametrů a pak zadat hodnoty parametrů jako další argumenty.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-112">You can include parameter placeholders in the SQL query string and then supply parameter values as additional arguments.</span></span> <span data-ttu-id="5c0ff-113">Všechny hodnoty parametrů, které zadáte, budou automaticky převedeny `DbParameter`na.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-113">Any parameter values you supply will automatically be converted to a `DbParameter`.</span></span>

<span data-ttu-id="5c0ff-114">Následující příklad předává jeden parametr uložené proceduře.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-114">The following example passes a single parameter to a stored procedure.</span></span> <span data-ttu-id="5c0ff-115">I když to může vypadat `String.Format` jako syntaxe, zadaná hodnota je zabalena v parametru a vygenerovaný název parametru vložený, `{0}` kde byl zadán zástupný symbol.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-115">While this may look like `String.Format` syntax, the supplied value is wrapped in a parameter and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

<span data-ttu-id="5c0ff-116">Jedná se o stejný dotaz, ale používá syntaxi řetězcové interpolace, která je podporovaná v EF Core 2,0 a novějších verzích:</span><span class="sxs-lookup"><span data-stu-id="5c0ff-116">This is the same query but using string interpolation syntax, which is supported in EF Core 2.0 and above:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

<span data-ttu-id="5c0ff-117">Můžete také vytvořit DbParameter a dodat ji jako hodnotu parametru:</span><span class="sxs-lookup"><span data-stu-id="5c0ff-117">You can also construct a DbParameter and supply it as a parameter value:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

<span data-ttu-id="5c0ff-118">To umožňuje použít pojmenované parametry v řetězci dotazu SQL, což je užitečné, pokud má uložená procedura volitelné parametry:</span><span class="sxs-lookup"><span data-stu-id="5c0ff-118">This allows you to use named parameters in the SQL query string, which is useful when a stored procedure has optional parameters:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs @filterByUser=@user", user)
    .ToList();
```

## <a name="composing-with-linq"></a><span data-ttu-id="5c0ff-119">Vytváření pomocí LINQ</span><span class="sxs-lookup"><span data-stu-id="5c0ff-119">Composing with LINQ</span></span>

<span data-ttu-id="5c0ff-120">Pokud je možné dotaz SQL sestavit v databázi, můžete vytvořit počáteční nezpracovaný dotaz SQL pomocí operátorů LINQ.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-120">If the SQL query can be composed on in the database, then you can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="5c0ff-121">Dotazy SQL, které mohou být tvořeny na začátku `SELECT` pomocí klíčového slova.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-121">SQL queries that can be composed on begin with the `SELECT` keyword.</span></span>

<span data-ttu-id="5c0ff-122">V následujícím příkladu se používá nezpracovaný dotaz SQL, který se vybere z funkce vracející tabulku (TVF), a pak se na něj vytvoří pomocí LINQ k filtrování a řazení.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-122">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF) and then composes on it using LINQ to perform filtering and sorting.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

## <a name="change-tracking"></a><span data-ttu-id="5c0ff-123">Sledování změn</span><span class="sxs-lookup"><span data-stu-id="5c0ff-123">Change Tracking</span></span>

<span data-ttu-id="5c0ff-124">Dotazy, které používají `FromSql()` přesná stejná pravidla sledování změn jako všechny ostatní dotazy LINQ v EF Core.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-124">Queries that use the `FromSql()` follow the exact same change tracking rules as any other LINQ query in EF Core.</span></span> <span data-ttu-id="5c0ff-125">Například pokud se jedná o typy entit dotazování, výsledky budou ve výchozím nastavení sledovány.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-125">For example, if the query projects entity types, the results will be tracked by default.</span></span>  

<span data-ttu-id="5c0ff-126">V následujícím příkladu je použit nezpracovaný dotaz SQL, který se vybere z funkce vracející tabulku (TVF), a potom zakáže sledování změn s voláním. AsNoTracking():</span><span class="sxs-lookup"><span data-stu-id="5c0ff-126">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF), then disables change tracking with the call to .AsNoTracking():</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Query<SearchBlogsDto>()
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .AsNoTracking()
    .ToList();
```

## <a name="including-related-data"></a><span data-ttu-id="5c0ff-127">Včetně souvisejících dat</span><span class="sxs-lookup"><span data-stu-id="5c0ff-127">Including related data</span></span>

<span data-ttu-id="5c0ff-128">`Include()` Metodu lze použít k zahrnutí souvisejících dat, stejně jako u jakéhokoli jiného dotazu LINQ:</span><span class="sxs-lookup"><span data-stu-id="5c0ff-128">The `Include()` method can be used to include related data, just like with any other LINQ query:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

## <a name="limitations"></a><span data-ttu-id="5c0ff-129">Omezení</span><span class="sxs-lookup"><span data-stu-id="5c0ff-129">Limitations</span></span>

<span data-ttu-id="5c0ff-130">Při použití nezpracovaných dotazů SQL je potřeba vědět o několika omezeních:</span><span class="sxs-lookup"><span data-stu-id="5c0ff-130">There are a few limitations to be aware of when using raw SQL queries:</span></span>

* <span data-ttu-id="5c0ff-131">Dotaz SQL musí vracet data pro všechny vlastnosti entity nebo typu dotazu.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-131">The SQL query must return data for all properties of the entity or query type.</span></span>

* <span data-ttu-id="5c0ff-132">Názvy sloupců v sadě výsledků se musí shodovat s názvy sloupců, na které jsou vlastnosti namapovány.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-132">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="5c0ff-133">Všimněte si, že se liší od EF6, kde bylo mapování vlastností nebo sloupců pro nezpracované dotazy SQL ignorováno a názvy sloupců sady výsledků musely odpovídat názvům vlastností.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-133">Note this is different from EF6 where property/column mapping was ignored for raw SQL queries and result set column names had to match the property names.</span></span>

* <span data-ttu-id="5c0ff-134">Dotaz SQL nemůže obsahovat související data.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-134">The SQL query cannot contain related data.</span></span> <span data-ttu-id="5c0ff-135">V mnoha případech však můžete vytvořit dotaz nad dotazem pomocí `Include` operátoru, který vrátí související data (viz [zahrnutí souvisejících dat](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="5c0ff-135">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

* <span data-ttu-id="5c0ff-136">`SELECT`příkazy předané do této metody by obecně měly být sestavitelné: Pokud EF Core potřebuje vyhodnotit další operátory pro dotazování na serveru (například převést operátory LINQ použité po `FromSql`), bude zadaný SQL považován za poddotaz.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-136">`SELECT` statements passed to this method should generally be composable: If EF Core needs to evaluate additional query operators on the server (for example, to translate LINQ operators applied after `FromSql`), the supplied SQL will be treated as a subquery.</span></span> <span data-ttu-id="5c0ff-137">To znamená, že předaný SQL by neměl obsahovat žádné znaky ani možnosti, které nejsou platné pro poddotaz, například:</span><span class="sxs-lookup"><span data-stu-id="5c0ff-137">This means that the SQL passed should not contain any characters or options that are not valid on a subquery, such as:</span></span>
  * <span data-ttu-id="5c0ff-138">koncový středník</span><span class="sxs-lookup"><span data-stu-id="5c0ff-138">a trailing semicolon</span></span>
  * <span data-ttu-id="5c0ff-139">Na SQL Server, na koncové doporučení na úrovni dotazu (například `OPTION (HASH JOIN)`)</span><span class="sxs-lookup"><span data-stu-id="5c0ff-139">On SQL Server, a trailing query-level hint (for example, `OPTION (HASH JOIN)`)</span></span>
  * <span data-ttu-id="5c0ff-140">V SQL Server, `ORDER BY` klauzule, která není doprovázena `OFFSET 0` `SELECT` klauzulí nebo `TOP 100 PERCENT` v klauzuli</span><span class="sxs-lookup"><span data-stu-id="5c0ff-140">On SQL Server, an `ORDER BY` clause that is not accompanied of `OFFSET 0` OR `TOP 100 PERCENT` in the `SELECT` clause</span></span>

* <span data-ttu-id="5c0ff-141">Příkazy jazyka SQL jiné `SELECT` než jsou rozpoznány automaticky jako nevyhovující.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-141">SQL statements other than `SELECT` are recognized automatically as non-composable.</span></span> <span data-ttu-id="5c0ff-142">V důsledku toho jsou všechny výsledky uložených procedur vždycky vraceny klientovi a všechny operátory LINQ použité po `FromSql` jsou vyhodnocovány v paměti.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-142">As a consequence, the full results of stored procedures are always returned to the client and any LINQ operators applied after `FromSql` are evaluated in-memory.</span></span>

> [!WARNING]  
> <span data-ttu-id="5c0ff-143">**Vždy používat parametrizace pro nezpracované dotazy SQL:** Kromě ověřování vstupu uživatele vždy použijte parametrizace pro všechny hodnoty používané v nezpracovaných dotazech nebo příkazech jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-143">**Always use parameterization for raw SQL queries:** In addition to validating user input, always use parameterization for any values used in a raw SQL query/command.</span></span> <span data-ttu-id="5c0ff-144">Rozhraní API, která přijímají Nezpracovaný řetězec `FromSql` SQL `ExecuteSqlCommand` jako a umožňují snadné předání hodnot jako parametrů.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-144">APIs that accept a raw SQL string such as `FromSql` and `ExecuteSqlCommand` allow values to be easily passed as parameters.</span></span> <span data-ttu-id="5c0ff-145">Přetížení `FromSql` a`ExecuteSqlCommand` přijímající FormattableString také umožňují použití syntaxe řetězcové interpolace způsobem, který pomáhá chránit před útoky prostřednictvím injektáže SQL.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-145">Overloads of `FromSql` and `ExecuteSqlCommand` that accept FormattableString also allow using string interpolation syntax in a way that helps protect against SQL injection attacks.</span></span> 
> 
> <span data-ttu-id="5c0ff-146">Pokud používáte zřetězení řetězců nebo interpolace k dynamickému sestavení jakékoli části řetězce dotazu nebo předání vstupu uživatele k příkazům nebo uloženým procedurám, které mohou spustit tyto vstupy jako dynamické SQL, pak zodpovídáte za ověření jakéhokoli vstupu na Chraňte proti útokům prostřednictvím injektáže SQL.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-146">If you are using string concatenation or interpolation to dynamically build any part of the query string, or passing user input to statements or stored procedures that can execute those inputs as dynamic SQL, then you are responsible for validating any input to protect against SQL injection attacks.</span></span>
