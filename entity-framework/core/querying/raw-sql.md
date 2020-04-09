---
title: Nezpracované dotazy SQL – jádro EF
author: smitpatel
ms.date: 10/08/2019
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: a54bb67c0fce9d621382f6372e70fe4cdca48a20
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417668"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="7dbc2-102">Nezpracované dotazy SQL</span><span class="sxs-lookup"><span data-stu-id="7dbc2-102">Raw SQL Queries</span></span>

<span data-ttu-id="7dbc2-103">Entity Framework Core umožňuje rozbalit na nezpracované dotazy SQL při práci s relační databází.</span><span class="sxs-lookup"><span data-stu-id="7dbc2-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="7dbc2-104">Nezpracované dotazy SQL jsou užitečné, pokud dotaz, který chcete nelze vyjádřit pomocí LINQ.</span><span class="sxs-lookup"><span data-stu-id="7dbc2-104">Raw SQL queries are useful if the query you want can't be expressed using LINQ.</span></span> <span data-ttu-id="7dbc2-105">Nezpracovaná dotazy SQL se také používají, pokud použití dotazu LINQ vede k neefektivnímu dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="7dbc2-105">Raw SQL queries are also used if using a LINQ query is resulting in an inefficient SQL query.</span></span> <span data-ttu-id="7dbc2-106">Nezpracovaná dotazy SQL můžete vrátit normální typy entit nebo [bezklíčové typy entit,](xref:core/modeling/keyless-entity-types) které jsou součástí vašeho modelu.</span><span class="sxs-lookup"><span data-stu-id="7dbc2-106">Raw SQL queries can return regular entity types or [keyless entity types](xref:core/modeling/keyless-entity-types) that are part of your model.</span></span>

> [!TIP]  
> <span data-ttu-id="7dbc2-107">Ukázku tohoto článku [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying/) můžete zobrazit na GitHubu.</span><span class="sxs-lookup"><span data-stu-id="7dbc2-107">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying/) on GitHub.</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="7dbc2-108">Základní nezpracované dotazy SQL</span><span class="sxs-lookup"><span data-stu-id="7dbc2-108">Basic raw SQL queries</span></span>

<span data-ttu-id="7dbc2-109">Metodu `FromSqlRaw` rozšíření můžete použít k zahájení dotazu LINQ na základě nezpracovaného dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="7dbc2-109">You can use the `FromSqlRaw` extension method to begin a LINQ query based on a raw SQL query.</span></span> <span data-ttu-id="7dbc2-110">`FromSqlRaw`lze použít pouze pro kořeny dotazu, který je přímo na `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="7dbc2-110">`FromSqlRaw` can only be used on query roots, that is directly on the `DbSet<>`.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRaw)]

<span data-ttu-id="7dbc2-111">Nezpracované dotazy SQL lze použít ke spuštění uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="7dbc2-111">Raw SQL queries can be used to execute a stored procedure.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedure)]

## <a name="passing-parameters"></a><span data-ttu-id="7dbc2-112">Předávání parametrů</span><span class="sxs-lookup"><span data-stu-id="7dbc2-112">Passing parameters</span></span>

> [!WARNING]
> <span data-ttu-id="7dbc2-113">**Vždy používat parametrizaci pro nezpracované dotazy SQL**</span><span class="sxs-lookup"><span data-stu-id="7dbc2-113">**Always use parameterization for raw SQL queries**</span></span>
>
> <span data-ttu-id="7dbc2-114">Při zavádění všech uživatelských zadaných hodnot do nezpracovaného dotazu SQL je třeba dbát na to, aby se zabránilo útokům injektáže SQL.</span><span class="sxs-lookup"><span data-stu-id="7dbc2-114">When introducing any user-provided values into a raw SQL query, care must be taken to avoid SQL injection attacks.</span></span> <span data-ttu-id="7dbc2-115">Kromě ověření, že tyto hodnoty neobsahují neplatné znaky, vždy používejte parametrizaci, která odesílá hodnoty oddělené od textu SQL.</span><span class="sxs-lookup"><span data-stu-id="7dbc2-115">In addition to validating that such values don't contain invalid characters, always use parameterization which sends the values separate from the SQL text.</span></span>
>
> <span data-ttu-id="7dbc2-116">Zejména nikdy nepředávat zřetězený nebo interpolovaný řetězec (`$""`) s neověřenými hodnotami poskytnutými uživatelem do `FromSqlRaw` nebo `ExecuteSqlRaw`.</span><span class="sxs-lookup"><span data-stu-id="7dbc2-116">In particular, never pass a concatenated or interpolated string (`$""`) with non-validated user-provided values into `FromSqlRaw` or `ExecuteSqlRaw`.</span></span> <span data-ttu-id="7dbc2-117">Metody `FromSqlInterpolated` `ExecuteSqlInterpolated` a umožňují použití syntaxe interpolace řetězce způsobem, který chrání před útoky injektáže SQL.</span><span class="sxs-lookup"><span data-stu-id="7dbc2-117">The `FromSqlInterpolated` and `ExecuteSqlInterpolated` methods allow using string interpolation syntax in a way that protects against SQL injection attacks.</span></span>

<span data-ttu-id="7dbc2-118">Následující příklad předá jeden parametr uložené proceduře zahrnutím zástupného symbolu parametru do řetězce dotazu SQL a poskytnutím dalšího argumentu.</span><span class="sxs-lookup"><span data-stu-id="7dbc2-118">The following example passes a single parameter to a stored procedure by including a parameter placeholder in the SQL query string and providing an additional argument.</span></span> <span data-ttu-id="7dbc2-119">Zatímco tato syntaxe `String.Format` může vypadat jako syntaxe, `DbParameter` zadaná hodnota je zabalena v a a vložen ý název generovaného parametru, kde byl zadán `{0}` zástupný symbol.</span><span class="sxs-lookup"><span data-stu-id="7dbc2-119">While this syntax may look like `String.Format` syntax, the supplied value is wrapped in a `DbParameter` and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureParameter)]

<span data-ttu-id="7dbc2-120">`FromSqlInterpolated`je podobná, `FromSqlRaw` ale umožňuje použít syntaxi interpolace řetězců.</span><span class="sxs-lookup"><span data-stu-id="7dbc2-120">`FromSqlInterpolated` is similar to `FromSqlRaw` but allows you to use string interpolation syntax.</span></span> <span data-ttu-id="7dbc2-121">Stejně `FromSqlRaw` `FromSqlInterpolated` jako , lze použít pouze na kořeny dotazu.</span><span class="sxs-lookup"><span data-stu-id="7dbc2-121">Just like `FromSqlRaw`, `FromSqlInterpolated` can only be used on query roots.</span></span> <span data-ttu-id="7dbc2-122">Stejně jako v předchozím příkladu je `DbParameter` hodnota převedena na a není ohrožena injektáží SQL.</span><span class="sxs-lookup"><span data-stu-id="7dbc2-122">As with the previous example, the value is converted to a `DbParameter` and isn't vulnerable to SQL injection.</span></span>

> [!NOTE]
> <span data-ttu-id="7dbc2-123">Před verzí 3.0 `FromSqlRaw` `FromSqlInterpolated` a byly dvě `FromSql`přetížení s názvem .</span><span class="sxs-lookup"><span data-stu-id="7dbc2-123">Prior to version 3.0, `FromSqlRaw` and `FromSqlInterpolated` were two overloads named `FromSql`.</span></span> <span data-ttu-id="7dbc2-124">Další informace naleznete v [části předchozí verze](#previous-versions).</span><span class="sxs-lookup"><span data-stu-id="7dbc2-124">For more information, see the [previous versions section](#previous-versions).</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedStoredProcedureParameter)]

<span data-ttu-id="7dbc2-125">Můžete také sestavit DbParameter a zadat jako hodnotu parametru.</span><span class="sxs-lookup"><span data-stu-id="7dbc2-125">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="7dbc2-126">Vzhledem k tomu, že se používá zástupný `FromSqlRaw` symbol běžného parametru SQL, nikoli zástupný symbol řetězce, lze bezpečně použít:</span><span class="sxs-lookup"><span data-stu-id="7dbc2-126">Since a regular SQL parameter placeholder is used, rather than a string placeholder, `FromSqlRaw` can be safely used:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureSqlParameter)]

<span data-ttu-id="7dbc2-127">`FromSqlRaw`umožňuje použít pojmenované parametry v řetězci dotazu SQL, což je užitečné, když uložená procedura má volitelné parametry:</span><span class="sxs-lookup"><span data-stu-id="7dbc2-127">`FromSqlRaw` allows you to use named parameters in the SQL query string, which is useful when a stored procedure has optional parameters:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureNamedSqlParameter)]

## <a name="composing-with-linq"></a><span data-ttu-id="7dbc2-128">Skládání s LINQ</span><span class="sxs-lookup"><span data-stu-id="7dbc2-128">Composing with LINQ</span></span>

<span data-ttu-id="7dbc2-129">Můžete sestavit na počáteční nezpracovaný dotaz SQL pomocí linq operátory.</span><span class="sxs-lookup"><span data-stu-id="7dbc2-129">You can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="7dbc2-130">EF Core bude považovat za poddotaz a skládání přes něj v databázi.</span><span class="sxs-lookup"><span data-stu-id="7dbc2-130">EF Core will treat it as subquery and compose over it in the database.</span></span> <span data-ttu-id="7dbc2-131">Následující příklad používá nezpracovaný dotaz SQL, který vybírá z funkce s hodnotou tabulky (TVF).</span><span class="sxs-lookup"><span data-stu-id="7dbc2-131">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF).</span></span> <span data-ttu-id="7dbc2-132">A pak skládá na to pomocí LINQ dělat filtrování a třídění.</span><span class="sxs-lookup"><span data-stu-id="7dbc2-132">And then composes on it using LINQ to do filtering and sorting.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedComposed)]

<span data-ttu-id="7dbc2-133">Výše uvedený dotaz generuje následující SQL:</span><span class="sxs-lookup"><span data-stu-id="7dbc2-133">Above query generates following SQL:</span></span>

```sql
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url]
FROM (
    SELECT * FROM dbo.SearchBlogs(@p0)
) AS [b]
WHERE [b].[Rating] > 3
ORDER BY [b].[Rating] DESC
```

### <a name="including-related-data"></a><span data-ttu-id="7dbc2-134">Včetně souvisejících údajů</span><span class="sxs-lookup"><span data-stu-id="7dbc2-134">Including related data</span></span>

<span data-ttu-id="7dbc2-135">Metodu `Include` lze použít k zahrnutí souvisejících dat, stejně jako u jiných dotazů LINQ:</span><span class="sxs-lookup"><span data-stu-id="7dbc2-135">The `Include` method can be used to include related data, just like with any other LINQ query:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedInclude)]

<span data-ttu-id="7dbc2-136">Skládání s LINQ vyžaduje, aby byl váš nezpracovaný dotaz SQL kompozitelný, protože EF Core bude považovat zadaný SQL jako poddotaz.</span><span class="sxs-lookup"><span data-stu-id="7dbc2-136">Composing with LINQ requires your raw SQL query to be composable since EF Core will treat the supplied SQL as a subquery.</span></span> <span data-ttu-id="7dbc2-137">Dotazy SQL, které mohou být `SELECT` složeny na začít s klíčovým slovem.</span><span class="sxs-lookup"><span data-stu-id="7dbc2-137">SQL queries that can be composed on begin with the `SELECT` keyword.</span></span> <span data-ttu-id="7dbc2-138">Předání SQL by dále nemělo obsahovat žádné znaky nebo možnosti, které nejsou platné v poddotazu, například:</span><span class="sxs-lookup"><span data-stu-id="7dbc2-138">Further, SQL passed shouldn't contain any characters or options that aren't valid on a subquery, such as:</span></span>

- <span data-ttu-id="7dbc2-139">Koncový středník</span><span class="sxs-lookup"><span data-stu-id="7dbc2-139">A trailing semicolon</span></span>
- <span data-ttu-id="7dbc2-140">Na serveru SQL Server je to ukázka `OPTION (HASH JOIN)`koncové úrovně dotazu (například)</span><span class="sxs-lookup"><span data-stu-id="7dbc2-140">On SQL Server, a trailing query-level hint (for example, `OPTION (HASH JOIN)`)</span></span>
- <span data-ttu-id="7dbc2-141">Na serveru SQL `ORDER BY` Server klauzule, `OFFSET 0` která `TOP 100 PERCENT` není `SELECT` použita s OR v klauzuli</span><span class="sxs-lookup"><span data-stu-id="7dbc2-141">On SQL Server, an `ORDER BY` clause that isn't used with `OFFSET 0` OR `TOP 100 PERCENT` in the `SELECT` clause</span></span>

<span data-ttu-id="7dbc2-142">SQL Server neumožňuje skládání přes volání uložené procedury, takže jakýkoli pokus o použití dalších operátorů dotazu na takové volání bude mít za následek neplatný SQL.</span><span class="sxs-lookup"><span data-stu-id="7dbc2-142">SQL Server doesn't allow composing over stored procedure calls, so any attempt to apply additional query operators to such a call will result in invalid SQL.</span></span> <span data-ttu-id="7dbc2-143">Použijte `AsEnumerable` `AsAsyncEnumerable` nebo metoda `FromSqlRaw` `FromSqlInterpolated` hned po nebo metody, aby se ujistil, že EF Core nepokouší vytvořit přes uloženou proceduru.</span><span class="sxs-lookup"><span data-stu-id="7dbc2-143">Use `AsEnumerable` or `AsAsyncEnumerable` method right after `FromSqlRaw` or `FromSqlInterpolated` methods to make sure that EF Core doesn't try to compose over a stored procedure.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="7dbc2-144">Sledování změn</span><span class="sxs-lookup"><span data-stu-id="7dbc2-144">Change Tracking</span></span>

<span data-ttu-id="7dbc2-145">Dotazy, které `FromSqlRaw` používají `FromSqlInterpolated` metody nebo, se řídí přesně stejnými pravidly sledování změn jako jakýkoli jiný dotaz LINQ v EF Core.</span><span class="sxs-lookup"><span data-stu-id="7dbc2-145">Queries that use the `FromSqlRaw` or `FromSqlInterpolated` methods follow the exact same change tracking rules as any other LINQ query in EF Core.</span></span> <span data-ttu-id="7dbc2-146">Pokud například typy entit projektů dotazu, budou výsledky ve výchozím nastavení sledovány.</span><span class="sxs-lookup"><span data-stu-id="7dbc2-146">For example, if the query projects entity types, the results will be tracked by default.</span></span>

<span data-ttu-id="7dbc2-147">Následující příklad používá nezpracovaný dotaz SQL, který vybere z funkce s hodnotou tabulky (TVF), pak zakáže sledování změn s voláním `AsNoTracking`:</span><span class="sxs-lookup"><span data-stu-id="7dbc2-147">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF), then disables change tracking with the call to `AsNoTracking`:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedAsNoTracking)]

## <a name="limitations"></a><span data-ttu-id="7dbc2-148">Omezení</span><span class="sxs-lookup"><span data-stu-id="7dbc2-148">Limitations</span></span>

<span data-ttu-id="7dbc2-149">Při použití nezpracovaných dotazů SQL je třeba znát několik omezení:</span><span class="sxs-lookup"><span data-stu-id="7dbc2-149">There are a few limitations to be aware of when using raw SQL queries:</span></span>

- <span data-ttu-id="7dbc2-150">Dotaz SQL musí vrátit data pro všechny vlastnosti typu entity.</span><span class="sxs-lookup"><span data-stu-id="7dbc2-150">The SQL query must return data for all properties of the entity type.</span></span>
- <span data-ttu-id="7dbc2-151">Názvy sloupců v sadě výsledků se musí shodovat s názvy sloupců, na které jsou namapovány vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="7dbc2-151">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="7dbc2-152">Všimněte si, že toto chování se liší od EF6.</span><span class="sxs-lookup"><span data-stu-id="7dbc2-152">Note this behavior is different from EF6.</span></span> <span data-ttu-id="7dbc2-153">EF6 ignoroval vlastnost na sloupce mapování pro nezpracované dotazy SQL a názvy sloupců sady výsledků musel odpovídat názvy vlastností.</span><span class="sxs-lookup"><span data-stu-id="7dbc2-153">EF6 ignored property to column mapping for raw SQL queries and result set column names had to match the property names.</span></span>
- <span data-ttu-id="7dbc2-154">Dotaz SQL nemůže obsahovat související data.</span><span class="sxs-lookup"><span data-stu-id="7dbc2-154">The SQL query can't contain related data.</span></span> <span data-ttu-id="7dbc2-155">V mnoha případech však můžete vytvořit nad dotaz `Include` pomocí operátoru vrátit související data (viz [Včetně souvisejících dat](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="7dbc2-155">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

## <a name="previous-versions"></a><span data-ttu-id="7dbc2-156">Předchozí verze</span><span class="sxs-lookup"><span data-stu-id="7dbc2-156">Previous versions</span></span>

<span data-ttu-id="7dbc2-157">EF Core verze 2.2 a dříve měl `FromSql`dvě přetížení metody s názvem , `FromSqlRaw` které `FromSqlInterpolated`se chovaly stejným způsobem jako novější a .</span><span class="sxs-lookup"><span data-stu-id="7dbc2-157">EF Core version 2.2 and earlier had two overloads of method named `FromSql`, which behaved in the same way as the newer `FromSqlRaw` and `FromSqlInterpolated`.</span></span> <span data-ttu-id="7dbc2-158">Bylo snadné náhodně volat metodu raw string, když záměrem bylo volání interpolované metody řetězce a naopak.</span><span class="sxs-lookup"><span data-stu-id="7dbc2-158">It was easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span> <span data-ttu-id="7dbc2-159">Volání nesprávné přetížení náhodně může mít za následek dotazy nejsou parametrizovány, když by měly být.</span><span class="sxs-lookup"><span data-stu-id="7dbc2-159">Calling wrong overload accidentally could result in queries not being parameterized when they should have been.</span></span>
