---
title: Nezpracované dotazy SQL – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: ad7ac3099cfd4c49b88acfbbff61f2af9294b6ec
ms.sourcegitcommit: a013e243a14f384999ceccaf9c779b8c1ae3b936
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2019
ms.locfileid: "57463240"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="ddb94-102">Nezpracované dotazy SQL</span><span class="sxs-lookup"><span data-stu-id="ddb94-102">Raw SQL Queries</span></span>

<span data-ttu-id="ddb94-103">Entity Framework Core umožňuje rozevírací seznam pro nezpracované dotazy SQL při práci s relační databáze.</span><span class="sxs-lookup"><span data-stu-id="ddb94-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="ddb94-104">To může být užitečné, pokud dotaz, který chcete provést nelze vyjádřen pomocí jazyka LINQ, nebo pomocí dotazu LINQ je výsledkem neefektivní dotazy SQL.</span><span class="sxs-lookup"><span data-stu-id="ddb94-104">This can be useful if the query you want to perform can't be expressed using LINQ, or if using a LINQ query is resulting in inefficient SQL queries.</span></span> <span data-ttu-id="ddb94-105">Nezpracované dotazy SQL může vrátit typy entit nebo, počínaje EF Core 2.1 [typy dotazů](xref:core/modeling/query-types) , které jsou součástí modelu.</span><span class="sxs-lookup"><span data-stu-id="ddb94-105">Raw SQL queries can return entity types or, starting with EF Core 2.1, [query types](xref:core/modeling/query-types) that are part of your model.</span></span>

> [!TIP]  
> <span data-ttu-id="ddb94-106">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="ddb94-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="ddb94-107">Základní nezpracované dotazy SQL</span><span class="sxs-lookup"><span data-stu-id="ddb94-107">Basic raw SQL queries</span></span>

<span data-ttu-id="ddb94-108">Můžete použít *FromSql* metodu rozšíření k začátku dotazu LINQ na základě nezpracované dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="ddb94-108">You can use the *FromSql* extension method to begin a LINQ query based on a raw SQL query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

<span data-ttu-id="ddb94-109">Nezpracované dotazy SQL můžete použít ke spuštění uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="ddb94-109">Raw SQL queries can be used to execute a stored procedure.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a><span data-ttu-id="ddb94-110">Předávání parametrů</span><span class="sxs-lookup"><span data-stu-id="ddb94-110">Passing parameters</span></span>

<span data-ttu-id="ddb94-111">Stejně jako u jakékoli rozhraní API, které přijímá SQL, je potřeba parametrizovat libovolný uživatelský vstup k ochraně proti útoku prostřednictvím injektáže SQL.</span><span class="sxs-lookup"><span data-stu-id="ddb94-111">As with any API that accepts SQL, it is important to parameterize any user input to protect against a SQL injection attack.</span></span> <span data-ttu-id="ddb94-112">Můžete zahrnout parametr zástupné symboly v řetězci dotazu SQL a pak zadejte hodnoty parametrů jako další argumenty.</span><span class="sxs-lookup"><span data-stu-id="ddb94-112">You can include parameter placeholders in the SQL query string and then supply parameter values as additional arguments.</span></span> <span data-ttu-id="ddb94-113">Všechny hodnoty parametrů, které zadáte, se automaticky převedou na `DbParameter`.</span><span class="sxs-lookup"><span data-stu-id="ddb94-113">Any parameter values you supply will automatically be converted to a `DbParameter`.</span></span>

<span data-ttu-id="ddb94-114">Následující příklad předá jeden parametr uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="ddb94-114">The following example passes a single parameter to a stored procedure.</span></span> <span data-ttu-id="ddb94-115">Přestože to může vypadat, jako jsou `String.Format` syntaxe, zadaná hodnota je zabalena v parametru a název vygenerovaný parametru vložen where `{0}` byl určen zástupný.</span><span class="sxs-lookup"><span data-stu-id="ddb94-115">While this may look like `String.Format` syntax, the supplied value is wrapped in a parameter and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

<span data-ttu-id="ddb94-116">Jde o stejný dotaz, ale pomocí syntaxe interpolace řetězce, který není podporovaný v EF Core 2.0 a vyšší:</span><span class="sxs-lookup"><span data-stu-id="ddb94-116">This is the same query but using string interpolation syntax, which is supported in EF Core 2.0 and above:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

<span data-ttu-id="ddb94-117">Můžete také sestavit DbParameter a zadat jako hodnotu parametru.</span><span class="sxs-lookup"><span data-stu-id="ddb94-117">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="ddb94-118">To umožňuje použití pojmenovaných parametrů v řetězci dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="ddb94-118">This allows you to use named parameters in the SQL query string.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

## <a name="composing-with-linq"></a><span data-ttu-id="ddb94-119">Sestavování s jazykem LINQ</span><span class="sxs-lookup"><span data-stu-id="ddb94-119">Composing with LINQ</span></span>

<span data-ttu-id="ddb94-120">Pokud se dotaz SQL se může skládat na v databázi, potom můžete vytvářet nad rámec počáteční neupraveného dotazu SQL pomocí operátorů LINQ.</span><span class="sxs-lookup"><span data-stu-id="ddb94-120">If the SQL query can be composed on in the database, then you can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="ddb94-121">Dotazy SQL, které mohou být složené na začínat `SELECT` – klíčové slovo.</span><span class="sxs-lookup"><span data-stu-id="ddb94-121">SQL queries that can be composed on begin with the `SELECT` keyword.</span></span>

<span data-ttu-id="ddb94-122">Následující příklad používá nezpracovaná dotaz SQL, který vybere z Table-Valued – funkce (TVF) a pak vytvoří v něm pomocí jazyka LINQ k provedení filtrování a řazení.</span><span class="sxs-lookup"><span data-stu-id="ddb94-122">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF) and then composes on it using LINQ to perform filtering and sorting.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

## <a name="change-tracking"></a><span data-ttu-id="ddb94-123">Sledování změn</span><span class="sxs-lookup"><span data-stu-id="ddb94-123">Change Tracking</span></span>

<span data-ttu-id="ddb94-124">Dotazy, které používají `FromSql()` postupovat podle přesně stejnou změnu sledování pravidla jako jakýkoli jiný dotaz LINQ v EF Core.</span><span class="sxs-lookup"><span data-stu-id="ddb94-124">Queries that use the `FromSql()` follow the exact same change tracking rules as any other LINQ query in EF Core.</span></span> <span data-ttu-id="ddb94-125">Například pokud dotaz projekty typy entit, výsledky budou sledovány ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="ddb94-125">For example, if the query projects entity types, the results will be tracked by default.</span></span>  

<span data-ttu-id="ddb94-126">Následující příklad pomocí neupraveného dotazu SQL, který vybere z Table-Valued – funkce (TVF) a zakáže změňte sledování volání. AsNoTracking():</span><span class="sxs-lookup"><span data-stu-id="ddb94-126">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF), then disables change tracking with teh call to .AsNoTracking():</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Query<SearchBlogsDto>()
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .AsNoTracking()
    .ToList();
```

## <a name="including-related-data"></a><span data-ttu-id="ddb94-127">Včetně souvisejících dat</span><span class="sxs-lookup"><span data-stu-id="ddb94-127">Including related data</span></span>

<span data-ttu-id="ddb94-128">`Include()` Metodu je možné zahrnout související data, jak je jakýkoli jiný dotaz LINQ:</span><span class="sxs-lookup"><span data-stu-id="ddb94-128">The `Include()` method can be used to include related data, just like with any other LINQ query:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

## <a name="limitations"></a><span data-ttu-id="ddb94-129">Omezení</span><span class="sxs-lookup"><span data-stu-id="ddb94-129">Limitations</span></span>

<span data-ttu-id="ddb94-130">Existuje několik omezení, která je potřeba při používání nezpracované dotazy SQL:</span><span class="sxs-lookup"><span data-stu-id="ddb94-130">There are a few limitations to be aware of when using raw SQL queries:</span></span>

* <span data-ttu-id="ddb94-131">Příkaz jazyka SQL musí vracet data pro všechny vlastnosti typu entity nebo dotazu.</span><span class="sxs-lookup"><span data-stu-id="ddb94-131">The SQL query must return data for all properties of the entity or query type.</span></span>

* <span data-ttu-id="ddb94-132">Názvy sloupců v sadě výsledků musí shodovat s názvy sloupců, které vlastnosti jsou mapovány na.</span><span class="sxs-lookup"><span data-stu-id="ddb94-132">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="ddb94-133">Všimněte si, že se liší od EF6, ve kterém byl ignorován, vlastnost nebo sloupec mapování pro nezpracované dotazy SQL a názvy musí shodovat s názvy vlastností, které sloupce sady výsledků dotazu.</span><span class="sxs-lookup"><span data-stu-id="ddb94-133">Note this is different from EF6 where property/column mapping was ignored for raw SQL queries and result set column names had to match the property names.</span></span>

* <span data-ttu-id="ddb94-134">Příkaz jazyka SQL nemůže obsahovat související data.</span><span class="sxs-lookup"><span data-stu-id="ddb94-134">The SQL query cannot contain related data.</span></span> <span data-ttu-id="ddb94-135">Ale v mnoha případech můžete vytvářet nad pomocí dotazu `Include` operátor vrátí související data (naleznete v tématu [včetně souvisejících dat](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="ddb94-135">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

* <span data-ttu-id="ddb94-136">`SELECT` příkazy předaný této metodě by měl být obecně sestavitelný: Pokud EF Core potřebuje k vyhodnocení operátorů další dotazu na serveru (například pro převod operátory LINQ použity po `FromSql`), zadaný SQL bude zacházeno jako s poddotaz.</span><span class="sxs-lookup"><span data-stu-id="ddb94-136">`SELECT` statements passed to this method should generally be composable: If EF Core needs to evaluate additional query operators on the server (for example, to translate LINQ operators applied after `FromSql`), the supplied SQL will be treated as a subquery.</span></span> <span data-ttu-id="ddb94-137">To znamená, že SQL předán nesmí obsahovat žádné znaky nebo možnosti, které nejsou platné v poddotazu, jako například:</span><span class="sxs-lookup"><span data-stu-id="ddb94-137">This means that the SQL passed should not contain any characters or options that are not valid on a subquery, such as:</span></span>
  * <span data-ttu-id="ddb94-138">koncovou středníkem</span><span class="sxs-lookup"><span data-stu-id="ddb94-138">a trailing semicolon</span></span>
  * <span data-ttu-id="ddb94-139">Na serveru SQL Server, koncové pomocný parametr dotazu úrovni (třeba `OPTION (HASH JOIN)`)</span><span class="sxs-lookup"><span data-stu-id="ddb94-139">On SQL Server, a trailing query-level hint (for example, `OPTION (HASH JOIN)`)</span></span>
  * <span data-ttu-id="ddb94-140">Na serveru SQL Server `ORDER BY` klauzuli, která se připojí z `TOP 100 PERCENT` v `SELECT` – klauzule</span><span class="sxs-lookup"><span data-stu-id="ddb94-140">On SQL Server, an `ORDER BY` clause that is not accompanied of `TOP 100 PERCENT` in the `SELECT` clause</span></span>

* <span data-ttu-id="ddb94-141">SQL příkazy jiných než `SELECT` jsou automaticky uznáváni jako bez možnosti složení.</span><span class="sxs-lookup"><span data-stu-id="ddb94-141">SQL statements other than `SELECT` are recognized automatically as non-composable.</span></span> <span data-ttu-id="ddb94-142">V důsledku toho úplné výsledky uložené procedury jsou vždy vrácen do klienta a jakékoli operátory LINQ použity po `FromSql` jsou vyhodnocené jako v paměti.</span><span class="sxs-lookup"><span data-stu-id="ddb94-142">As a consequence, the full results of stored procedures are always returned to the client and any LINQ operators applied after `FromSql` are evaluated in-memory.</span></span>

> [!WARNING]  
> <span data-ttu-id="ddb94-143">**Vždy používejte Parametrizace pro nezpracované dotazy SQL:** Kromě ověřování uživatelského vstupu, vždy používejte Parametrizace pro všechny hodnoty použité v nezpracované dotazu nebo příkaz SQL.</span><span class="sxs-lookup"><span data-stu-id="ddb94-143">**Always use parameterization for raw SQL queries:** In addition to validating user input, always use parameterization for any values used in a raw SQL query/command.</span></span> <span data-ttu-id="ddb94-144">Rozhraní API, které přijímají nezpracovaná SQL, jako řetězec `FromSql` a `ExecuteSqlCommand` povolit hodnot do snadno předat jako parametry.</span><span class="sxs-lookup"><span data-stu-id="ddb94-144">APIs that accept a raw SQL string such as `FromSql` and `ExecuteSqlCommand` allow values to be easily passed as parameters.</span></span> <span data-ttu-id="ddb94-145">Přetížení `FromSql` a `ExecuteSqlCommand` , která přijímají FormattableString také povolit pomocí syntaxt interpolace řetězců tak, že pomáhá chránit před útoky prostřednictvím injektáže SQL.</span><span class="sxs-lookup"><span data-stu-id="ddb94-145">Overloads of `FromSql` and `ExecuteSqlCommand` that accept FormattableString also allow using string interpolation syntaxt in a way that helps protect against SQL injection attacks.</span></span> 
> 
> <span data-ttu-id="ddb94-146">Pokud se dynamicky vytvářet všechny části řetězce dotazu pomocí zřetězení řetězců nebo interpolace nebo předání vstupu uživatele pro příkazy nebo uložené procedury, které můžete provést tyto vstupy jako dynamic SQL, pak budete muset pro jakýkoli vstup do ověření Ochrana před útoky prostřednictvím injektáže SQL.</span><span class="sxs-lookup"><span data-stu-id="ddb94-146">If you are using string concatenation or interpolation to dynamically build any part of the query string, or passing user input to statements or stored procedures that can execute those inputs as dynamic SQL, then you are responsible for validating any input to protect against SQL injection attacks.</span></span>
