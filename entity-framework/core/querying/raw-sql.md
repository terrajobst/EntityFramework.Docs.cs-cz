---
title: Nezpracované dotazy SQL – EF Core
author: smitpatel
ms.date: 10/08/2019
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: b7087771f1a9e8ee5e044cfea367d74a0b1c1d35
ms.sourcegitcommit: 37d0e0fd1703467918665a64837dc54ad2ec7484
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/16/2019
ms.locfileid: "72445924"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="7ef57-102">Nezpracované dotazy SQL</span><span class="sxs-lookup"><span data-stu-id="7ef57-102">Raw SQL Queries</span></span>

<span data-ttu-id="7ef57-103">Entity Framework Core umožňuje vyřadit z provozu nezpracované dotazy SQL při práci s relační databází.</span><span class="sxs-lookup"><span data-stu-id="7ef57-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="7ef57-104">Nezpracované dotazy SQL jsou užitečné v případě, že požadovaný dotaz nelze vyjádřit pomocí LINQ.</span><span class="sxs-lookup"><span data-stu-id="7ef57-104">Raw SQL queries are useful if the query you want can't be expressed using LINQ.</span></span> <span data-ttu-id="7ef57-105">Nezpracované dotazy SQL se používají také v případě, že použití dotazu LINQ má za následek neefektivní dotaz SQL.</span><span class="sxs-lookup"><span data-stu-id="7ef57-105">Raw SQL queries are also used if using a LINQ query is resulting in an inefficient SQL query.</span></span> <span data-ttu-id="7ef57-106">Nezpracované dotazy SQL mohou vracet běžné typy entit nebo [typy entit bez klíčů](xref:core/modeling/keyless-entity-types) , které jsou součástí vašeho modelu.</span><span class="sxs-lookup"><span data-stu-id="7ef57-106">Raw SQL queries can return regular entity types or [keyless entity types](xref:core/modeling/keyless-entity-types) that are part of your model.</span></span>

> [!TIP]  
> <span data-ttu-id="7ef57-107">[Ukázku](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying/) tohoto článku můžete zobrazit na GitHubu.</span><span class="sxs-lookup"><span data-stu-id="7ef57-107">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying/) on GitHub.</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="7ef57-108">Základní nezpracované dotazy SQL</span><span class="sxs-lookup"><span data-stu-id="7ef57-108">Basic raw SQL queries</span></span>

<span data-ttu-id="7ef57-109">Pomocí metody rozšíření `FromSqlRaw` můžete spustit dotaz LINQ založený na nezpracovaném dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="7ef57-109">You can use the `FromSqlRaw` extension method to begin a LINQ query based on a raw SQL query.</span></span> <span data-ttu-id="7ef57-110">`FromSqlRaw` lze použít pouze v kořenech dotazu, který je přímo na `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="7ef57-110">`FromSqlRaw` can only be used on query roots, that is directly on the `DbSet<>`.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRaw)]

<span data-ttu-id="7ef57-111">Nezpracované dotazy SQL lze použít ke spuštění uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="7ef57-111">Raw SQL queries can be used to execute a stored procedure.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedure)]

## <a name="passing-parameters"></a><span data-ttu-id="7ef57-112">Předávání parametrů</span><span class="sxs-lookup"><span data-stu-id="7ef57-112">Passing parameters</span></span>

> [!WARNING]
> <span data-ttu-id="7ef57-113">**Vždy používat parametrizace pro nezpracované dotazy SQL**</span><span class="sxs-lookup"><span data-stu-id="7ef57-113">**Always use parameterization for raw SQL queries**</span></span>
>
> <span data-ttu-id="7ef57-114">Při zavádění jakýchkoli uživatelsky zadaných hodnot do nezpracovaného dotazu SQL je nutné dbát na to, aby se zabránilo útokům prostřednictvím injektáže SQL.</span><span class="sxs-lookup"><span data-stu-id="7ef57-114">When introducing any user-provided values into a raw SQL query, care must be taken to avoid SQL injection attacks.</span></span> <span data-ttu-id="7ef57-115">Kromě ověření, že tyto hodnoty neobsahují neplatné znaky, vždy použijte Parametrizace, který odesílá hodnoty oddělené od textu SQL.</span><span class="sxs-lookup"><span data-stu-id="7ef57-115">In addition to validating that such values don't contain invalid characters, always use parameterization which sends the values separate from the SQL text.</span></span>
>
> <span data-ttu-id="7ef57-116">Konkrétně nikdy předejte zřetězený nebo interpolující řetězec (`$""`) s neověřenými uživatelskými hodnotami do `FromSqlRaw` nebo `ExecuteSqlRaw`.</span><span class="sxs-lookup"><span data-stu-id="7ef57-116">In particular, never pass a concatenated or interpolated string (`$""`) with non-validated user-provided values into `FromSqlRaw` or `ExecuteSqlRaw`.</span></span> <span data-ttu-id="7ef57-117">Metody `FromSqlInterpolated` a `ExecuteSqlInterpolated` umožňují použití syntaxe řetězcového interpolace způsobem, který chrání před útoky prostřednictvím injektáže SQL.</span><span class="sxs-lookup"><span data-stu-id="7ef57-117">The `FromSqlInterpolated` and `ExecuteSqlInterpolated` methods allow using string interpolation syntax in a way that protects against SQL injection attacks.</span></span>

<span data-ttu-id="7ef57-118">Následující příklad předává jeden parametr uložené proceduře zahrnutím zástupného symbolu parametru do řetězce dotazu SQL a zadáním dalšího argumentu.</span><span class="sxs-lookup"><span data-stu-id="7ef57-118">The following example passes a single parameter to a stored procedure by including a parameter placeholder in the SQL query string and providing an additional argument.</span></span> <span data-ttu-id="7ef57-119">I když tato syntaxe může vypadat jako syntaxe `String.Format`, zadaná hodnota je zabalená do `DbParameter` a vygenerovaný název parametru vložený tam, kde byl zadán zástupný symbol `{0}`.</span><span class="sxs-lookup"><span data-stu-id="7ef57-119">While this syntax may look like `String.Format` syntax, the supplied value is wrapped in a `DbParameter` and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureParameter)]

<span data-ttu-id="7ef57-120">`FromSqlInterpolated` se podobá `FromSqlRaw`, ale umožňuje použít syntaxi řetězcové interpolace.</span><span class="sxs-lookup"><span data-stu-id="7ef57-120">`FromSqlInterpolated` is similar to `FromSqlRaw` but allows you to use string interpolation syntax.</span></span> <span data-ttu-id="7ef57-121">Stejně jako `FromSqlRaw` `FromSqlInterpolated` lze použít pouze v kořenech dotazů.</span><span class="sxs-lookup"><span data-stu-id="7ef57-121">Just like `FromSqlRaw`, `FromSqlInterpolated` can only be used on query roots.</span></span> <span data-ttu-id="7ef57-122">Stejně jako v předchozím příkladu je hodnota převedena na `DbParameter` a není zranitelná při injektáže SQL.</span><span class="sxs-lookup"><span data-stu-id="7ef57-122">As with the previous example, the value is converted to a `DbParameter` and isn't vulnerable to SQL injection.</span></span>

> [!NOTE]
> <span data-ttu-id="7ef57-123">Před verzí 3,0 byly dvě přetížení s názvem `FromSql` `FromSqlRaw` a `FromSqlInterpolated`.</span><span class="sxs-lookup"><span data-stu-id="7ef57-123">Prior to version 3.0, `FromSqlRaw` and `FromSqlInterpolated` were two overloads named `FromSql`.</span></span> <span data-ttu-id="7ef57-124">Další informace najdete v [části předchozí verze](#previous-versions).</span><span class="sxs-lookup"><span data-stu-id="7ef57-124">For more information, see the [previous versions section](#previous-versions).</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedStoredProcedureParameter)]

<span data-ttu-id="7ef57-125">Můžete také vytvořit DbParameter a dodat ji jako hodnotu parametru.</span><span class="sxs-lookup"><span data-stu-id="7ef57-125">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="7ef57-126">Vzhledem k tomu, že se místo zástupného symbolu řetězce používá regulární zástupný parametr SQL, `FromSqlRaw` se dá bezpečně použít:</span><span class="sxs-lookup"><span data-stu-id="7ef57-126">Since a regular SQL parameter placeholder is used, rather than a string placeholder, `FromSqlRaw` can be safely used:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureSqlParameter)]

<span data-ttu-id="7ef57-127">@no__t – 0 umožňuje použít pojmenované parametry v řetězci dotazu SQL, což je užitečné, pokud má uložená procedura volitelné parametry:</span><span class="sxs-lookup"><span data-stu-id="7ef57-127">`FromSqlRaw` allows you to use named parameters in the SQL query string, which is useful when a stored procedure has optional parameters:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureNamedSqlParameter)]

## <a name="composing-with-linq"></a><span data-ttu-id="7ef57-128">Vytváření pomocí LINQ</span><span class="sxs-lookup"><span data-stu-id="7ef57-128">Composing with LINQ</span></span>

<span data-ttu-id="7ef57-129">Pomocí operátorů LINQ můžete vytvořit počáteční nezpracovaný dotaz SQL.</span><span class="sxs-lookup"><span data-stu-id="7ef57-129">You can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="7ef57-130">EF Core se bude považovat za poddotaz a pak je v databázi přetvořit.</span><span class="sxs-lookup"><span data-stu-id="7ef57-130">EF Core will treat it as subquery and compose over it in the database.</span></span> <span data-ttu-id="7ef57-131">Následující příklad používá nezpracovaný dotaz SQL, který se vybere z funkce vracející tabulku (TVF).</span><span class="sxs-lookup"><span data-stu-id="7ef57-131">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF).</span></span> <span data-ttu-id="7ef57-132">A pak je vytvoří pomocí LINQ k filtrování a řazení.</span><span class="sxs-lookup"><span data-stu-id="7ef57-132">And then composes on it using LINQ to do filtering and sorting.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedComposed)]

<span data-ttu-id="7ef57-133">Výše uvedený dotaz generuje následující příkaz SQL:</span><span class="sxs-lookup"><span data-stu-id="7ef57-133">Above query generates following SQL:</span></span>

```sql
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url]
FROM (
    SELECT * FROM dbo.SearchBlogs(@p0)
) AS [b]
WHERE [b].[Rating] > 3
ORDER BY [b].[Rating] DESC
```

### <a name="including-related-data"></a><span data-ttu-id="7ef57-134">Včetně souvisejících dat</span><span class="sxs-lookup"><span data-stu-id="7ef57-134">Including related data</span></span>

<span data-ttu-id="7ef57-135">Metodu `Include` lze použít k zahrnutí souvisejících dat, stejně jako u jakéhokoli jiného dotazu LINQ:</span><span class="sxs-lookup"><span data-stu-id="7ef57-135">The `Include` method can be used to include related data, just like with any other LINQ query:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedInclude)]

<span data-ttu-id="7ef57-136">Sestavování pomocí LINQ vyžaduje, aby váš nezpracovaný dotaz SQL byl sestavitelný, protože EF Core považuje dodaný příkaz SQL za poddotaz.</span><span class="sxs-lookup"><span data-stu-id="7ef57-136">Composing with LINQ requires your raw SQL query to be composable since EF Core will treat the supplied SQL as a subquery.</span></span> <span data-ttu-id="7ef57-137">Dotazy SQL, které mohou být tvořeny na začátku s klíčovým slovem `SELECT`.</span><span class="sxs-lookup"><span data-stu-id="7ef57-137">SQL queries that can be composed on begin with the `SELECT` keyword.</span></span> <span data-ttu-id="7ef57-138">Předaný SQL by neměl obsahovat žádné znaky ani možnosti, které nejsou platné pro poddotaz, například:</span><span class="sxs-lookup"><span data-stu-id="7ef57-138">Further, SQL passed shouldn't contain any characters or options that aren't valid on a subquery, such as:</span></span>

- <span data-ttu-id="7ef57-139">Koncový středník</span><span class="sxs-lookup"><span data-stu-id="7ef57-139">A trailing semicolon</span></span>
- <span data-ttu-id="7ef57-140">Na SQL Server, koncové doporučení na úrovni dotazu (například `OPTION (HASH JOIN)`)</span><span class="sxs-lookup"><span data-stu-id="7ef57-140">On SQL Server, a trailing query-level hint (for example, `OPTION (HASH JOIN)`)</span></span>
- <span data-ttu-id="7ef57-141">V SQL Server klauzule `ORDER BY`, která se v klauzuli `SELECT` nepoužívá `OFFSET 0` nebo `TOP 100 PERCENT`</span><span class="sxs-lookup"><span data-stu-id="7ef57-141">On SQL Server, an `ORDER BY` clause that isn't used with `OFFSET 0` OR `TOP 100 PERCENT` in the `SELECT` clause</span></span>

<span data-ttu-id="7ef57-142">SQL Server neumožňuje sestavovat volání uložených procedur, takže případný pokus o použití dalších operátorů dotazu na takové volání způsobí neplatnost SQL.</span><span class="sxs-lookup"><span data-stu-id="7ef57-142">SQL Server doesn't allow composing over stored procedure calls, so any attempt to apply additional query operators to such a call will result in invalid SQL.</span></span> <span data-ttu-id="7ef57-143">Použijte metodu `AsEnumerable` nebo `AsAsyncEnumerable` přímo po `FromSqlRaw` nebo `FromSqlInterpolated`, abyste se ujistili, že se EF Core nepokusí vytvořit uloženou proceduru.</span><span class="sxs-lookup"><span data-stu-id="7ef57-143">Use `AsEnumerable` or `AsAsyncEnumerable` method right after `FromSqlRaw` or `FromSqlInterpolated` methods to make sure that EF Core doesn't try to compose over a stored procedure.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="7ef57-144">Sledování změn</span><span class="sxs-lookup"><span data-stu-id="7ef57-144">Change Tracking</span></span>

<span data-ttu-id="7ef57-145">Dotazy, které používají metody `FromSqlRaw` nebo `FromSqlInterpolated`, dodržují přesná stejná pravidla sledování změn jako všechny ostatní dotazy LINQ v EF Core.</span><span class="sxs-lookup"><span data-stu-id="7ef57-145">Queries that use the `FromSqlRaw` or `FromSqlInterpolated` methods follow the exact same change tracking rules as any other LINQ query in EF Core.</span></span> <span data-ttu-id="7ef57-146">Například pokud se jedná o typy entit dotazování, výsledky budou ve výchozím nastavení sledovány.</span><span class="sxs-lookup"><span data-stu-id="7ef57-146">For example, if the query projects entity types, the results will be tracked by default.</span></span>

<span data-ttu-id="7ef57-147">Následující příklad používá nezpracovaný dotaz SQL, který se vybere z funkce vracející tabulku (TVF), a potom zakáže sledování změn s voláním `AsNoTracking`:</span><span class="sxs-lookup"><span data-stu-id="7ef57-147">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF), then disables change tracking with the call to `AsNoTracking`:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedAsNoTracking)]

## <a name="limitations"></a><span data-ttu-id="7ef57-148">Omezení</span><span class="sxs-lookup"><span data-stu-id="7ef57-148">Limitations</span></span>

<span data-ttu-id="7ef57-149">Při použití nezpracovaných dotazů SQL je potřeba vědět o několika omezeních:</span><span class="sxs-lookup"><span data-stu-id="7ef57-149">There are a few limitations to be aware of when using raw SQL queries:</span></span>

- <span data-ttu-id="7ef57-150">Dotaz SQL musí vracet data pro všechny vlastnosti typu entity.</span><span class="sxs-lookup"><span data-stu-id="7ef57-150">The SQL query must return data for all properties of the entity type.</span></span>
- <span data-ttu-id="7ef57-151">Názvy sloupců v sadě výsledků se musí shodovat s názvy sloupců, na které jsou vlastnosti namapovány.</span><span class="sxs-lookup"><span data-stu-id="7ef57-151">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="7ef57-152">Všimněte si, že toto chování se liší od EF6.</span><span class="sxs-lookup"><span data-stu-id="7ef57-152">Note this behavior is different from EF6.</span></span> <span data-ttu-id="7ef57-153">EF6 ignorovaná vlastnost na mapování sloupce pro nezpracované dotazy SQL a názvy sloupců sady výsledků musí odpovídat názvům vlastností.</span><span class="sxs-lookup"><span data-stu-id="7ef57-153">EF6 ignored property to column mapping for raw SQL queries and result set column names had to match the property names.</span></span>
- <span data-ttu-id="7ef57-154">Dotaz SQL nemůže obsahovat související data.</span><span class="sxs-lookup"><span data-stu-id="7ef57-154">The SQL query can't contain related data.</span></span> <span data-ttu-id="7ef57-155">V mnoha případech však můžete vytvořit dotaz nad dotazem pomocí operátoru `Include`, který vrátí související data (viz [zahrnutí souvisejících dat](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="7ef57-155">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

## <a name="previous-versions"></a><span data-ttu-id="7ef57-156">Předchozí verze</span><span class="sxs-lookup"><span data-stu-id="7ef57-156">Previous versions</span></span>

<span data-ttu-id="7ef57-157">EF Core verze 2,2 a starší obsahovaly dvě přetížení metody s názvem `FromSql`, která se chovají stejným způsobem jako novější `FromSqlRaw` a `FromSqlInterpolated`.</span><span class="sxs-lookup"><span data-stu-id="7ef57-157">EF Core version 2.2 and earlier had two overloads of method named `FromSql`, which behaved in the same way as the newer `FromSqlRaw` and `FromSqlInterpolated`.</span></span> <span data-ttu-id="7ef57-158">Nechtěně zavolejte metodu nezpracovaného řetězce, pokud by byl záměr volal interpolovaná řetězcová metoda a druhým způsobem.</span><span class="sxs-lookup"><span data-stu-id="7ef57-158">It was easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span> <span data-ttu-id="7ef57-159">Volání chybného přetížení by mohlo vést k tomu, že dotazy nejsou parametrizované, pokud by měly být.</span><span class="sxs-lookup"><span data-stu-id="7ef57-159">Calling wrong overload accidentally could result in queries not being parameterized when they should have been.</span></span>
