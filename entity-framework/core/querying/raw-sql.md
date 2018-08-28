---
title: Nezpracované dotazy SQL – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: 21cb688d6775039def3b0be12768da71b5d96531
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997141"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="7c54a-102">Nezpracované dotazy SQL</span><span class="sxs-lookup"><span data-stu-id="7c54a-102">Raw SQL Queries</span></span>

<span data-ttu-id="7c54a-103">Entity Framework Core umožňuje rozevírací seznam pro nezpracované dotazy SQL při práci s relační databáze.</span><span class="sxs-lookup"><span data-stu-id="7c54a-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="7c54a-104">To může být užitečné, pokud dotaz, který chcete provést nelze vyjádřen pomocí jazyka LINQ, nebo pomocí dotazu LINQ je výsledkem neefektivní SQL odesílané do databáze.</span><span class="sxs-lookup"><span data-stu-id="7c54a-104">This can be useful if the query you want to perform can't be expressed using LINQ, or if using a LINQ query is resulting in inefficient SQL being sent to the database.</span></span> <span data-ttu-id="7c54a-105">Nezpracované dotazy SQL může vrátit typy entit nebo, počínaje EF Core 2.1 [typy dotazů](xref:core/modeling/query-types) , které jsou součástí modelu.</span><span class="sxs-lookup"><span data-stu-id="7c54a-105">Raw SQL queries can return entity types or, starting with EF Core 2.1, [query types](xref:core/modeling/query-types) that are part of your model.</span></span>

> [!TIP]  
> <span data-ttu-id="7c54a-106">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="7c54a-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="limitations"></a><span data-ttu-id="7c54a-107">Omezení</span><span class="sxs-lookup"><span data-stu-id="7c54a-107">Limitations</span></span>

<span data-ttu-id="7c54a-108">Existuje několik omezení, která je potřeba při používání nezpracované dotazy SQL:</span><span class="sxs-lookup"><span data-stu-id="7c54a-108">There are a few limitations to be aware of when using raw SQL queries:</span></span>

* <span data-ttu-id="7c54a-109">Příkaz jazyka SQL musí vracet data pro všechny vlastnosti typu entity nebo dotazu.</span><span class="sxs-lookup"><span data-stu-id="7c54a-109">The SQL query must return data for all properties of the entity or query type.</span></span>

* <span data-ttu-id="7c54a-110">Názvy sloupců v sadě výsledků musí shodovat s názvy sloupců, které vlastnosti jsou mapovány na.</span><span class="sxs-lookup"><span data-stu-id="7c54a-110">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="7c54a-111">Všimněte si, že se liší od EF6, ve kterém byl ignorován, vlastnost nebo sloupec mapování pro nezpracované dotazy SQL a názvy musí shodovat s názvy vlastností, které sloupce sady výsledků dotazu.</span><span class="sxs-lookup"><span data-stu-id="7c54a-111">Note this is different from EF6 where property/column mapping was ignored for raw SQL queries and result set column names had to match the property names.</span></span>

* <span data-ttu-id="7c54a-112">Příkaz jazyka SQL nemůže obsahovat související data.</span><span class="sxs-lookup"><span data-stu-id="7c54a-112">The SQL query cannot contain related data.</span></span> <span data-ttu-id="7c54a-113">Ale v mnoha případech můžete vytvářet nad pomocí dotazu `Include` operátor vrátí související data (naleznete v tématu [včetně souvisejících dat](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="7c54a-113">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

* <span data-ttu-id="7c54a-114">`SELECT` příkazy předaný této metodě by měl být obecně sestavitelný: pokud EF Core potřebuje k vyhodnocení operátorů další dotazu na serveru (například pro převod operátory LINQ použity po `FromSql`), zadaný SQL bude zacházeno jako s poddotaz.</span><span class="sxs-lookup"><span data-stu-id="7c54a-114">`SELECT` statements passed to this method should generally be composable: If EF Core needs to evaluate additional query operators on the server (for example, to translate LINQ operators applied after `FromSql`), the supplied SQL will be treated as a subquery.</span></span> <span data-ttu-id="7c54a-115">To znamená, že SQL předán nesmí obsahovat žádné znaky nebo možnosti, které nejsou platné v poddotazu, jako například:</span><span class="sxs-lookup"><span data-stu-id="7c54a-115">This means that the SQL passed should not contain any characters or options that are not valid on a subquery, such as:</span></span>
  * <span data-ttu-id="7c54a-116">koncovou středníkem</span><span class="sxs-lookup"><span data-stu-id="7c54a-116">a trailing semicolon</span></span>
  * <span data-ttu-id="7c54a-117">Na serveru SQL Server, koncové pomocný parametr dotazu úrovni (třeba `OPTION (HASH JOIN)`)</span><span class="sxs-lookup"><span data-stu-id="7c54a-117">On SQL Server, a trailing query-level hint (for example, `OPTION (HASH JOIN)`)</span></span>
  * <span data-ttu-id="7c54a-118">Na serveru SQL Server `ORDER BY` klauzuli, která se připojí z `TOP 100 PERCENT` v `SELECT` – klauzule</span><span class="sxs-lookup"><span data-stu-id="7c54a-118">On SQL Server, an `ORDER BY` clause that is not accompanied of `TOP 100 PERCENT` in the `SELECT` clause</span></span>

* <span data-ttu-id="7c54a-119">SQL příkazy jiných než `SELECT` jsou automaticky uznáváni jako bez možnosti složení.</span><span class="sxs-lookup"><span data-stu-id="7c54a-119">SQL statements other than `SELECT` are recognized automatically as non-composable.</span></span> <span data-ttu-id="7c54a-120">V důsledku toho úplné výsledky uložené procedury jsou vždy vrácen do klienta a jakékoli operátory LINQ použity po `FromSql` jsou vyhodnocené jako v paměti.</span><span class="sxs-lookup"><span data-stu-id="7c54a-120">As a consequence, the full results of stored procedures are always returned to the client and any LINQ operators applied after `FromSql` are evaluated in-memory.</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="7c54a-121">Základní nezpracované dotazy SQL</span><span class="sxs-lookup"><span data-stu-id="7c54a-121">Basic raw SQL queries</span></span>

<span data-ttu-id="7c54a-122">Můžete použít *FromSql* metodu rozšíření k začátku dotazu LINQ na základě nezpracované dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="7c54a-122">You can use the *FromSql* extension method to begin a LINQ query based on a raw SQL query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

<span data-ttu-id="7c54a-123">Nezpracované dotazy SQL můžete použít ke spuštění uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="7c54a-123">Raw SQL queries can be used to execute a stored procedure.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a><span data-ttu-id="7c54a-124">Předávání parametrů</span><span class="sxs-lookup"><span data-stu-id="7c54a-124">Passing parameters</span></span>

<span data-ttu-id="7c54a-125">Stejně jako u jakékoli rozhraní API, které přijímá SQL, je potřeba parametrizovat libovolný uživatelský vstup k ochraně proti útoku prostřednictvím injektáže SQL.</span><span class="sxs-lookup"><span data-stu-id="7c54a-125">As with any API that accepts SQL, it is important to parameterize any user input to protect against a SQL injection attack.</span></span> <span data-ttu-id="7c54a-126">Můžete zahrnout parametr zástupné symboly v řetězci dotazu SQL a pak zadejte hodnoty parametrů jako další argumenty.</span><span class="sxs-lookup"><span data-stu-id="7c54a-126">You can include parameter placeholders in the SQL query string and then supply parameter values as additional arguments.</span></span> <span data-ttu-id="7c54a-127">Všechny hodnoty parametrů, které zadáte, se automaticky převedou na `DbParameter`.</span><span class="sxs-lookup"><span data-stu-id="7c54a-127">Any parameter values you supply will automatically be converted to a `DbParameter`.</span></span>

<span data-ttu-id="7c54a-128">Následující příklad předá jeden parametr uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="7c54a-128">The following example passes a single parameter to a stored procedure.</span></span> <span data-ttu-id="7c54a-129">Přestože to může vypadat, jako jsou `String.Format` syntaxe, zadaná hodnota je zabalena v parametru a název vygenerovaný parametru vložen where `{0}` byl určen zástupný.</span><span class="sxs-lookup"><span data-stu-id="7c54a-129">While this may look like `String.Format` syntax, the supplied value is wrapped in a parameter and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

<span data-ttu-id="7c54a-130">Jde o stejný dotaz, ale pomocí syntaxe interpolace řetězce, který není podporovaný v EF Core 2.0 a vyšší:</span><span class="sxs-lookup"><span data-stu-id="7c54a-130">This is the same query but using string interpolation syntax, which is supported in EF Core 2.0 and above:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

<span data-ttu-id="7c54a-131">Můžete také sestavit DbParameter a zadat jako hodnotu parametru.</span><span class="sxs-lookup"><span data-stu-id="7c54a-131">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="7c54a-132">Díky tomu můžete použít pojmenované parametry v řetězci dotazu SQL</span><span class="sxs-lookup"><span data-stu-id="7c54a-132">This allows you to use named parameters in the SQL query string</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

## <a name="composing-with-linq"></a><span data-ttu-id="7c54a-133">Sestavování s jazykem LINQ</span><span class="sxs-lookup"><span data-stu-id="7c54a-133">Composing with LINQ</span></span>

<span data-ttu-id="7c54a-134">Pokud se dotaz SQL se může skládat na v databázi, potom můžete vytvářet nad rámec počáteční neupraveného dotazu SQL pomocí operátorů LINQ.</span><span class="sxs-lookup"><span data-stu-id="7c54a-134">If the SQL query can be composed on in the database, then you can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="7c54a-135">Dotazy SQL, které mohou být složené na právě s `SELECT` – klíčové slovo.</span><span class="sxs-lookup"><span data-stu-id="7c54a-135">SQL queries that can be composed on being with the `SELECT` keyword.</span></span>

<span data-ttu-id="7c54a-136">Následující příklad používá nezpracovaná dotaz SQL, který vybere z Table-Valued – funkce (TVF) a pak vytvoří v něm pomocí jazyka LINQ k provedení filtrování a řazení.</span><span class="sxs-lookup"><span data-stu-id="7c54a-136">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF) and then composes on it using LINQ to perform filtering and sorting.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

### <a name="including-related-data"></a><span data-ttu-id="7c54a-137">Včetně souvisejících dat</span><span class="sxs-lookup"><span data-stu-id="7c54a-137">Including related data</span></span>

<span data-ttu-id="7c54a-138">Sestavování s operátory LINQ, je možné zahrnout související data v dotazu.</span><span class="sxs-lookup"><span data-stu-id="7c54a-138">Composing with LINQ operators can be used to include related data in the query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

> [!WARNING]  
> <span data-ttu-id="7c54a-139">**Vždy používat Parametrizace pro nezpracované dotazy SQL:** rozhraní API, které přijímají nezpracovaná SQL, jako řetězec `FromSql` a `ExecuteSqlCommand` povolit hodnot do snadno předat jako parametry.</span><span class="sxs-lookup"><span data-stu-id="7c54a-139">**Always use parameterization for raw SQL queries:** APIs that accept a raw SQL string such as `FromSql` and `ExecuteSqlCommand` allow values to be easily passed as parameters.</span></span> <span data-ttu-id="7c54a-140">Kromě ověřování uživatelského vstupu, vždy používejte Parametrizace pro všechny hodnoty použité v nezpracované dotazu nebo příkaz SQL.</span><span class="sxs-lookup"><span data-stu-id="7c54a-140">In addition to validating user input, always use parameterization for any values used in a raw SQL query/command.</span></span> <span data-ttu-id="7c54a-141">Pokud používáte zřetězení řetězců dynamicky vytvářet všechny části řetězce dotazu, pak budete muset ověřování jakékoli vstupu pro ochranu před útoky prostřednictvím injektáže SQL.</span><span class="sxs-lookup"><span data-stu-id="7c54a-141">If you are using string concatenation to dynamically build any part of the query string then you are responsible for validating any input to protect against SQL injection attacks.</span></span>
