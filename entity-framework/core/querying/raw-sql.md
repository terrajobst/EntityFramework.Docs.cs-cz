---
title: Nezpracované dotazy SQL – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: ebec5775770c0f1e297eaaf35bf644c605a69afc
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197770"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="5c679-102">Nezpracované dotazy SQL</span><span class="sxs-lookup"><span data-stu-id="5c679-102">Raw SQL Queries</span></span>

<span data-ttu-id="5c679-103">Entity Framework Core umožňuje vyřadit z provozu nezpracované dotazy SQL při práci s relační databází.</span><span class="sxs-lookup"><span data-stu-id="5c679-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="5c679-104">To může být užitečné v případě, že dotaz, který chcete provést, nelze vyjádřit pomocí LINQ, nebo pokud použití dotazu LINQ má za následek neefektivní dotaz SQL.</span><span class="sxs-lookup"><span data-stu-id="5c679-104">This can be useful if the query you want to perform can't be expressed using LINQ, or if using a LINQ query is resulting in an inefficient SQL query.</span></span> <span data-ttu-id="5c679-105">Nezpracované dotazy SQL mohou vracet běžné typy entit nebo [typy entit bez klíčů](xref:core/modeling/keyless-entity-types) , které jsou součástí vašeho modelu.</span><span class="sxs-lookup"><span data-stu-id="5c679-105">Raw SQL queries can return regular entity types or [keyless entity types](xref:core/modeling/keyless-entity-types) that are part of your model.</span></span>

> [!TIP]  
> <span data-ttu-id="5c679-106">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying/Querying/RawSQL/Sample.cs) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="5c679-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying/Querying/RawSQL/Sample.cs) on GitHub.</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="5c679-107">Základní nezpracované dotazy SQL</span><span class="sxs-lookup"><span data-stu-id="5c679-107">Basic raw SQL queries</span></span>

<span data-ttu-id="5c679-108">Můžete použít `FromSqlRaw` metodu rozšíření k zahájení dotazu LINQ na základě nezpracovaného dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="5c679-108">You can use the `FromSqlRaw` extension method to begin a LINQ query based on a raw SQL query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSqlRaw("SELECT * FROM dbo.Blogs")
    .ToList();
```

<span data-ttu-id="5c679-109">Nezpracované dotazy SQL lze použít ke spuštění uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="5c679-109">Raw SQL queries can be used to execute a stored procedure.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a><span data-ttu-id="5c679-110">Předávání parametrů</span><span class="sxs-lookup"><span data-stu-id="5c679-110">Passing parameters</span></span>

> [!WARNING]
> <span data-ttu-id="5c679-111">**Vždy používat parametrizace pro nezpracované dotazy SQL**</span><span class="sxs-lookup"><span data-stu-id="5c679-111">**Always use parameterization for raw SQL queries**</span></span>
>
> <span data-ttu-id="5c679-112">Při zavádění jakýchkoli uživatelsky zadaných hodnot do nezpracovaného dotazu SQL je nutné dbát na to, aby se zabránilo útokům prostřednictvím injektáže SQL.</span><span class="sxs-lookup"><span data-stu-id="5c679-112">When introducing any user-provided values into a raw SQL query, care must be taken to avoid SQL injection attacks.</span></span> <span data-ttu-id="5c679-113">Kromě ověření, že tyto hodnoty neobsahují neplatné znaky, vždy použijte Parametrizace, který odesílá hodnoty oddělené od textu SQL.</span><span class="sxs-lookup"><span data-stu-id="5c679-113">In addition to validating that such values don't contain invalid characters, always use parameterization which sends the values separate from the SQL text.</span></span>
>
> <span data-ttu-id="5c679-114">Konkrétně nikdy nepředávejte zřetězený nebo interpolující řetězec (`$""`) s neověřenými uživatelem poskytnutými hodnotami do `FromSqlRaw` nebo `ExecuteSqlRaw`.</span><span class="sxs-lookup"><span data-stu-id="5c679-114">In particular, never pass a concatenated or interpolated string (`$""`) with unvalidated user-provided values into `FromSqlRaw` or `ExecuteSqlRaw`.</span></span> <span data-ttu-id="5c679-115">Metody `FromSqlInterpolated` a`ExecuteSqlInterpolated` umožňují použití syntaxe řetězcové interpolace způsobem, který chrání před útoky prostřednictvím injektáže SQL.</span><span class="sxs-lookup"><span data-stu-id="5c679-115">The `FromSqlInterpolated` and `ExecuteSqlInterpolated` methods allow using string interpolation syntax in a way that protects against SQL injection attacks.</span></span>

<span data-ttu-id="5c679-116">Následující příklad předává jeden parametr uložené proceduře zahrnutím zástupného symbolu parametru do řetězce dotazu SQL a zadáním dalšího argumentu.</span><span class="sxs-lookup"><span data-stu-id="5c679-116">The following example passes a single parameter to a stored procedure by including a parameter placeholder in the SQL query string and providing an additional argument.</span></span> <span data-ttu-id="5c679-117">Přestože to může vypadat jako `String.Format` syntaxe, zadaná hodnota je zabalena `DbParameter` v a a vygenerovaný název `{0}` parametru vložený, kde byl zadán zástupný symbol.</span><span class="sxs-lookup"><span data-stu-id="5c679-117">While this may look like `String.Format` syntax, the supplied value is wrapped in a `DbParameter` and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

<span data-ttu-id="5c679-118">Jako alternativu k `FromSqlRaw`, můžete použít `FromSqlInterpolated` , který umožňuje bezpečné použití řetězcové interpolace.</span><span class="sxs-lookup"><span data-stu-id="5c679-118">As an alternative to `FromSqlRaw`, you can use `FromSqlInterpolated` which allows the safe use of string interpolation.</span></span> <span data-ttu-id="5c679-119">Stejně jako v předchozím příkladu je hodnota převedena na `DbParameter` a, proto není ohrožena vložením SQL:</span><span class="sxs-lookup"><span data-stu-id="5c679-119">As with the previous example, the value is converted to a `DbParameter` and is therefore not vulnerable to SQL injection:</span></span>

> [!NOTE]
> <span data-ttu-id="5c679-120">Před verzí 3,0 `FromSqlRaw` a `FromSqlInterpolated` byly dvě přetížení s názvem `FromSql`.</span><span class="sxs-lookup"><span data-stu-id="5c679-120">Prior to version 3.0, `FromSqlRaw` and `FromSqlInterpolated` were two overloads named `FromSql`.</span></span> <span data-ttu-id="5c679-121">Další podrobnosti najdete v [části předchozí verze](#previous-versions) .</span><span class="sxs-lookup"><span data-stu-id="5c679-121">See the [previous versions section](#previous-versions) for more details.</span></span>


<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSqlInterpolated($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

<span data-ttu-id="5c679-122">Můžete také vytvořit DbParameter a dodat ji jako hodnotu parametru.</span><span class="sxs-lookup"><span data-stu-id="5c679-122">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="5c679-123">Vzhledem k tomu, že je použit běžný zástupný symbol parametru SQL, nikoli zástupný `FromSqlRaw` symbol řetězce, lze bezpečně použít:</span><span class="sxs-lookup"><span data-stu-id="5c679-123">Since a regular SQL parameter placeholder is used, rather than a string placeholder, `FromSqlRaw` can be safely used:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

<span data-ttu-id="5c679-124">To umožňuje použít pojmenované parametry v řetězci dotazu SQL, což je užitečné, pokud má uložená procedura volitelné parametry:</span><span class="sxs-lookup"><span data-stu-id="5c679-124">This allows you to use named parameters in the SQL query string, which is useful when a stored procedure has optional parameters:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogs @filterByUser=@user", user)
    .ToList();
```

## <a name="composing-with-linq"></a><span data-ttu-id="5c679-125">Vytváření pomocí LINQ</span><span class="sxs-lookup"><span data-stu-id="5c679-125">Composing with LINQ</span></span>

<span data-ttu-id="5c679-126">Pokud je možné dotaz SQL sestavit v databázi, můžete vytvořit počáteční nezpracovaný dotaz SQL pomocí operátorů LINQ.</span><span class="sxs-lookup"><span data-stu-id="5c679-126">If the SQL query can be composed on in the database, then you can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="5c679-127">Dotazy SQL, které mohou být tvořeny na začátku `SELECT` pomocí klíčového slova.</span><span class="sxs-lookup"><span data-stu-id="5c679-127">SQL queries that can be composed on begin with the `SELECT` keyword.</span></span>

<span data-ttu-id="5c679-128">V následujícím příkladu se používá nezpracovaný dotaz SQL, který se vybere z funkce vracející tabulku (TVF), a pak se na něj vytvoří pomocí LINQ k filtrování a řazení.</span><span class="sxs-lookup"><span data-stu-id="5c679-128">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF) and then composes on it using LINQ to perform filtering and sorting.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

<span data-ttu-id="5c679-129">Tím se vytvoří následující dotaz SQL:</span><span class="sxs-lookup"><span data-stu-id="5c679-129">This will produce the following SQL query:</span></span>

``` sql
SELECT [b].[Id], [b].[Name], [b].[Rating]
        FROM (
            SELECT * FROM dbo.SearchBlogs(@p0)
        ) AS b
        WHERE b."Rating" > 3
        ORDER BY b."Rating" DESC
```

## <a name="change-tracking"></a><span data-ttu-id="5c679-130">Sledování změn</span><span class="sxs-lookup"><span data-stu-id="5c679-130">Change Tracking</span></span>

<span data-ttu-id="5c679-131">Dotazy, které používají `FromSql` metody, dodržují stejná pravidla sledování změn jako jakýkoli jiný dotaz LINQ v EF Core.</span><span class="sxs-lookup"><span data-stu-id="5c679-131">Queries that use the `FromSql` methods follow the exact same change tracking rules as any other LINQ query in EF Core.</span></span> <span data-ttu-id="5c679-132">Například pokud se jedná o typy entit dotazování, výsledky budou ve výchozím nastavení sledovány.</span><span class="sxs-lookup"><span data-stu-id="5c679-132">For example, if the query projects entity types, the results will be tracked by default.</span></span>

<span data-ttu-id="5c679-133">V následujícím příkladu je použit nezpracovaný dotaz SQL, který se vybere z funkce vracející tabulku (TVF), a potom zakáže sledování změn voláním `AsNoTracking`:</span><span class="sxs-lookup"><span data-stu-id="5c679-133">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF), then disables change tracking with the call to `AsNoTracking`:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Query<SearchBlogsDto>()
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .AsNoTracking()
    .ToList();
```

## <a name="including-related-data"></a><span data-ttu-id="5c679-134">Včetně souvisejících dat</span><span class="sxs-lookup"><span data-stu-id="5c679-134">Including related data</span></span>

<span data-ttu-id="5c679-135">`Include` Metodu lze použít k zahrnutí souvisejících dat, stejně jako u jakéhokoli jiného dotazu LINQ:</span><span class="sxs-lookup"><span data-stu-id="5c679-135">The `Include` method can be used to include related data, just like with any other LINQ query:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

<span data-ttu-id="5c679-136">Všimněte si, že to vyžaduje, aby byl nezpracovaný dotaz SQL sestavitelný; nebude to zejména fungovat s voláními uložených procedur.</span><span class="sxs-lookup"><span data-stu-id="5c679-136">Note that this requires your raw SQL query to be composable; it will notably not work with stored procedure calls.</span></span> <span data-ttu-id="5c679-137">V části [omezení](#limitations)si přečtěte poznámky k možnosti vytváření.</span><span class="sxs-lookup"><span data-stu-id="5c679-137">See notes on composability under [Limitations](#limitations)).</span></span>

## <a name="limitations"></a><span data-ttu-id="5c679-138">Omezení</span><span class="sxs-lookup"><span data-stu-id="5c679-138">Limitations</span></span>

<span data-ttu-id="5c679-139">Při použití nezpracovaných dotazů SQL je potřeba vědět o několika omezeních:</span><span class="sxs-lookup"><span data-stu-id="5c679-139">There are a few limitations to be aware of when using raw SQL queries:</span></span>

* <span data-ttu-id="5c679-140">Dotaz SQL musí vracet data pro všechny vlastnosti typu entity.</span><span class="sxs-lookup"><span data-stu-id="5c679-140">The SQL query must return data for all properties of the entity type.</span></span>

* <span data-ttu-id="5c679-141">Názvy sloupců v sadě výsledků se musí shodovat s názvy sloupců, na které jsou vlastnosti namapovány.</span><span class="sxs-lookup"><span data-stu-id="5c679-141">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="5c679-142">Všimněte si, že se liší od EF6, kde bylo mapování vlastností nebo sloupců pro nezpracované dotazy SQL ignorováno a názvy sloupců sady výsledků musely odpovídat názvům vlastností.</span><span class="sxs-lookup"><span data-stu-id="5c679-142">Note this is different from EF6 where property/column mapping was ignored for raw SQL queries and result set column names had to match the property names.</span></span>

* <span data-ttu-id="5c679-143">Dotaz SQL nemůže obsahovat související data.</span><span class="sxs-lookup"><span data-stu-id="5c679-143">The SQL query cannot contain related data.</span></span> <span data-ttu-id="5c679-144">V mnoha případech však můžete vytvořit dotaz nad dotazem pomocí `Include` operátoru, který vrátí související data (viz [zahrnutí souvisejících dat](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="5c679-144">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

* <span data-ttu-id="5c679-145">`SELECT`příkazy předané do této metody by obecně měly být sestavitelné: Pokud EF Core potřebuje vyhodnotit další operátory pro dotazování na serveru (například převést operátory LINQ použité po `FromSql` metodách), bude zadaný SQL považován za poddotaz.</span><span class="sxs-lookup"><span data-stu-id="5c679-145">`SELECT` statements passed to this method should generally be composable: If EF Core needs to evaluate additional query operators on the server (for example, to translate LINQ operators applied after `FromSql` methods), the supplied SQL will be treated as a subquery.</span></span> <span data-ttu-id="5c679-146">To znamená, že předaný SQL by neměl obsahovat žádné znaky ani možnosti, které nejsou platné pro poddotaz, například:</span><span class="sxs-lookup"><span data-stu-id="5c679-146">This means that the SQL passed should not contain any characters or options that are not valid on a subquery, such as:</span></span>
  * <span data-ttu-id="5c679-147">koncový středník</span><span class="sxs-lookup"><span data-stu-id="5c679-147">A trailing semicolon</span></span>
  * <span data-ttu-id="5c679-148">Na SQL Server, na koncové doporučení na úrovni dotazu (například `OPTION (HASH JOIN)`)</span><span class="sxs-lookup"><span data-stu-id="5c679-148">On SQL Server, a trailing query-level hint (for example, `OPTION (HASH JOIN)`)</span></span>
  * <span data-ttu-id="5c679-149">V SQL Server, `ORDER BY` klauzule, která není doprovázena `OFFSET 0` `SELECT` klauzulí nebo `TOP 100 PERCENT` v klauzuli</span><span class="sxs-lookup"><span data-stu-id="5c679-149">On SQL Server, an `ORDER BY` clause that is not accompanied of `OFFSET 0` OR `TOP 100 PERCENT` in the `SELECT` clause</span></span>

* <span data-ttu-id="5c679-150">Všimněte si, že SQL Server nepovoluje sestavování prostřednictvím volání uložených procedur, takže případný pokus o použití dalších operátorů dotazu na takové volání způsobí neplatnost SQL.</span><span class="sxs-lookup"><span data-stu-id="5c679-150">Note that SQL Server does not allow composing over stored procedure calls, so any attempt to apply additional query operators to such a call will result in invalid SQL.</span></span> <span data-ttu-id="5c679-151">Operátory dotazů lze zavést po `AsEnumerable()` vyhodnocení klientů.</span><span class="sxs-lookup"><span data-stu-id="5c679-151">Query operators may be introduced after `AsEnumerable()` for client evaluation.</span></span>

# <a name="previous-versions"></a><span data-ttu-id="5c679-152">Předchozí verze</span><span class="sxs-lookup"><span data-stu-id="5c679-152">Previous versions</span></span>

<span data-ttu-id="5c679-153">EF Core verze 2,2 a starší obsahovala dvě přetížení s názvem `FromSql` , která se chovají stejným způsobem jako novější `FromSqlRaw` a `FromSqlInterpolated`.</span><span class="sxs-lookup"><span data-stu-id="5c679-153">EF Core version 2.2 and earlier had two overloads named `FromSql` which behaved in the same way as the newer `FromSqlRaw` and `FromSqlInterpolated`.</span></span> <span data-ttu-id="5c679-154">To usnadňuje náhodné volání metody nezpracovaného řetězce, pokud by záměr byl zavolat metodu interpolované řetězce a druhým způsobem.</span><span class="sxs-lookup"><span data-stu-id="5c679-154">This made it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span> <span data-ttu-id="5c679-155">To může vést k tomu, že dotazy nejsou parametrizované, pokud by měly být.</span><span class="sxs-lookup"><span data-stu-id="5c679-155">This could result in queries not being parameterized when they should have been.</span></span>
