---
title: Komplexní operátory dotazů – jádro EF
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 2e187a2a-4072-4198-9040-aaad68e424fd
uid: core/querying/complex-query-operators
ms.openlocfilehash: 44c2695ea003da043925740a52596fd27da638f8
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417741"
---
# <a name="complex-query-operators"></a><span data-ttu-id="224ba-102">Komplexní operátory dotazů</span><span class="sxs-lookup"><span data-stu-id="224ba-102">Complex Query Operators</span></span>

<span data-ttu-id="224ba-103">Jazyk integrovaný dotaz (LINQ) obsahuje mnoho komplexních operátorů, které kombinují více zdrojů dat nebo provádí komplexní zpracování.</span><span class="sxs-lookup"><span data-stu-id="224ba-103">Language Integrated Query (LINQ) contains many complex operators, which combine multiple data sources or does complex processing.</span></span> <span data-ttu-id="224ba-104">Ne všechny operátory LINQ mají vhodné překlady na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="224ba-104">Not all LINQ operators have suitable translations on the server side.</span></span> <span data-ttu-id="224ba-105">Někdy se dotaz v jednom formuláři překládá na server, ale pokud je napsán v jiné podobě, nepřekládá, i když je výsledek stejný.</span><span class="sxs-lookup"><span data-stu-id="224ba-105">Sometimes, a query in one form translates to the server but if written in a different form doesn't translate even if the result is the same.</span></span> <span data-ttu-id="224ba-106">Tato stránka popisuje některé komplexní operátory a jejich podporované varianty.</span><span class="sxs-lookup"><span data-stu-id="224ba-106">This page describes some of the complex operators and their supported variations.</span></span> <span data-ttu-id="224ba-107">V budoucích verzích můžeme rozpoznat další vzory a přidat jejich odpovídající překlady.</span><span class="sxs-lookup"><span data-stu-id="224ba-107">In future releases, we may recognize more patterns and add their corresponding translations.</span></span> <span data-ttu-id="224ba-108">Je také důležité mít na paměti, že podpora překladů se liší mezi poskytovateli.</span><span class="sxs-lookup"><span data-stu-id="224ba-108">It's also important to keep in mind that translation support varies between providers.</span></span> <span data-ttu-id="224ba-109">Konkrétní dotaz, který je přeložen v SqlServer, nemusí fungovat pro databáze SQLite.</span><span class="sxs-lookup"><span data-stu-id="224ba-109">A particular query, which is translated in SqlServer, may not work for SQLite databases.</span></span>

> [!TIP]
> <span data-ttu-id="224ba-110">Ukázku tohoto článku [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) můžete zobrazit na GitHubu.</span><span class="sxs-lookup"><span data-stu-id="224ba-110">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="join"></a><span data-ttu-id="224ba-111">Spojit</span><span class="sxs-lookup"><span data-stu-id="224ba-111">Join</span></span>

<span data-ttu-id="224ba-112">Operátor LINQ Join umožňuje připojit dva zdroje dat na základě voliče klíčů pro každý zdroj a generovat řazenou n-tice hodnot při shod.s.</span><span class="sxs-lookup"><span data-stu-id="224ba-112">The LINQ Join operator allows you to connect two data sources based on the key selector for each source, generating a tuple of values when the key matches.</span></span> <span data-ttu-id="224ba-113">To se přirozeně `INNER JOIN` překládá na relačních databázích.</span><span class="sxs-lookup"><span data-stu-id="224ba-113">It naturally translates to `INNER JOIN` on relational databases.</span></span> <span data-ttu-id="224ba-114">Zatímco linq spojení má vnější a vnitřní klíčové voliče, databáze vyžaduje podmínku jednoho spojení.</span><span class="sxs-lookup"><span data-stu-id="224ba-114">While the LINQ Join has outer and inner key selectors, the database requires a single join condition.</span></span> <span data-ttu-id="224ba-115">Takže EF Core generuje podmínku spojení porovnáním voliče vnějšího klíče s voličem vnitřního klíče pro rovnost.</span><span class="sxs-lookup"><span data-stu-id="224ba-115">So EF Core generates a join condition by comparing the outer key selector to the inner key selector for equality.</span></span> <span data-ttu-id="224ba-116">Dále pokud selektory klíčů jsou anonymní typy, EF Core generuje podmínku spojení porovnat součást rovnosti moudrý.</span><span class="sxs-lookup"><span data-stu-id="224ba-116">Further, if the key selectors are anonymous types, EF Core generates a join condition to compare equality component wise.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#Join)]

```SQL
SELECT [p].[PersonId], [p].[Name], [p].[PhotoId], [p0].[PersonPhotoId], [p0].[Caption], [p0].[Photo]
FROM [PersonPhoto] AS [p0]
INNER JOIN [Person] AS [p] ON [p0].[PersonPhotoId] = [p].[PhotoId]
```

## <a name="groupjoin"></a><span data-ttu-id="224ba-117">Skupinové spojení</span><span class="sxs-lookup"><span data-stu-id="224ba-117">GroupJoin</span></span>

<span data-ttu-id="224ba-118">Operátor LINQ GroupJoin umožňuje připojit dva zdroje dat podobné Join, ale vytvoří skupinu vnitřních hodnot pro odpovídající vnější prvky.</span><span class="sxs-lookup"><span data-stu-id="224ba-118">The LINQ GroupJoin operator allows you to connect two data sources similar to Join, but it creates a group of inner values for matching outer elements.</span></span> <span data-ttu-id="224ba-119">Spuštění dotazu, jako je následující příklad `Blog`  &  `IEnumerable<Post>`generuje výsledek .</span><span class="sxs-lookup"><span data-stu-id="224ba-119">Executing a query like the following example generates a result of `Blog` & `IEnumerable<Post>`.</span></span> <span data-ttu-id="224ba-120">Vzhledem k tomu, že databáze (zejména relační databáze) nemají způsob, jak reprezentovat kolekci objektů na straně klienta, GroupJoin nepřekládá na server v mnoha případech.</span><span class="sxs-lookup"><span data-stu-id="224ba-120">Since databases (especially relational databases) don't have a way to represent a collection of client-side objects, GroupJoin doesn't translate to the server in many cases.</span></span> <span data-ttu-id="224ba-121">Vyžaduje, abyste získat všechna data ze serveru k tomu GroupJoin bez zvláštního voliče (první dotaz níže).</span><span class="sxs-lookup"><span data-stu-id="224ba-121">It requires you to get all of the data from the server to do GroupJoin without a special selector (first query below).</span></span> <span data-ttu-id="224ba-122">Ale pokud selektor omezuje data jsou vybrána pak načítání všech dat ze serveru může způsobit problémy s výkonem (druhý dotaz níže).</span><span class="sxs-lookup"><span data-stu-id="224ba-122">But if the selector is limiting data being selected then fetching all of the data from the server may cause performance issues (second query below).</span></span> <span data-ttu-id="224ba-123">To je důvod, proč EF Core nepřekládá GroupJoin.</span><span class="sxs-lookup"><span data-stu-id="224ba-123">That's why EF Core doesn't translate GroupJoin.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoin)]

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoinComposed)]

## <a name="selectmany"></a><span data-ttu-id="224ba-124">Selectmany</span><span class="sxs-lookup"><span data-stu-id="224ba-124">SelectMany</span></span>

<span data-ttu-id="224ba-125">Linq SelectMany Operátor umožňuje výčet přes výběr kolekce pro každý vnější prvek a generovat řazené kolekce hodnot z každého zdroje dat.</span><span class="sxs-lookup"><span data-stu-id="224ba-125">The LINQ SelectMany operator allows you to enumerate over a collection selector for each outer element and generate tuples of values from each data source.</span></span> <span data-ttu-id="224ba-126">Svým způsobem je spojení, ale bez jakékoli podmínky, takže každý vnější prvek je spojen s elementem ze zdroje kolekce.</span><span class="sxs-lookup"><span data-stu-id="224ba-126">In a way, it's a join but without any condition so every outer element is connected with an element from the collection source.</span></span> <span data-ttu-id="224ba-127">V závislosti na tom, jak se volič kolekce vztahuje k vnějšímu zdroji dat SelectMany lze přeložit do různých dotazů na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="224ba-127">Depending on how the collection selector is related to the outer data source, SelectMany can translate into various different queries on the server side.</span></span>

### <a name="collection-selector-doesnt-reference-outer"></a><span data-ttu-id="224ba-128">Volič kolekce neodkazuje na vnější</span><span class="sxs-lookup"><span data-stu-id="224ba-128">Collection selector doesn't reference outer</span></span>

<span data-ttu-id="224ba-129">Pokud volič kolekce neodkazuje na nic z vnějšího zdroje, výsledkem je kartézský produkt obou zdrojů dat.</span><span class="sxs-lookup"><span data-stu-id="224ba-129">When the collection selector isn't referencing anything from the outer source, the result is a cartesian product of both data sources.</span></span> <span data-ttu-id="224ba-130">To se `CROSS JOIN` překládá do relačních databází.</span><span class="sxs-lookup"><span data-stu-id="224ba-130">It translates to `CROSS JOIN` in relational databases.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToCrossJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
CROSS JOIN [Posts] AS [p]
```

### <a name="collection-selector-references-outer-in-a-where-clause"></a><span data-ttu-id="224ba-131">Výběr kolekce odkazuje na vnější v where klauzule</span><span class="sxs-lookup"><span data-stu-id="224ba-131">Collection selector references outer in a where clause</span></span>

<span data-ttu-id="224ba-132">Pokud má volič kolekce klauzuli where, která odkazuje na vnější prvek, pak EF Core převede na spojení databáze a použije predikát jako podmínku spojení.</span><span class="sxs-lookup"><span data-stu-id="224ba-132">When the collection selector has a where clause, which references the outer element, then EF Core translates it to a database join and uses the predicate as the join condition.</span></span> <span data-ttu-id="224ba-133">Obvykle tento případ nastane při použití kolekce navigace na vnější prvek jako volič kolekce.</span><span class="sxs-lookup"><span data-stu-id="224ba-133">Normally this case arises when using collection navigation on the outer element as the collection selector.</span></span> <span data-ttu-id="224ba-134">Pokud kolekce je prázdný pro vnější prvek, pak žádné výsledky by být generovány pro tento vnější prvek.</span><span class="sxs-lookup"><span data-stu-id="224ba-134">If the collection is empty for an outer element, then no results would be generated for that outer element.</span></span> <span data-ttu-id="224ba-135">Ale `DefaultIfEmpty` pokud je použita na voliče kolekce pak vnější prvek bude spojen s výchozí hodnotou vnitřní prvek.</span><span class="sxs-lookup"><span data-stu-id="224ba-135">But if `DefaultIfEmpty` is applied on the collection selector then the outer element will be connected with a default value of the inner element.</span></span> <span data-ttu-id="224ba-136">Z důvodu tohoto rozlišení tento druh dotazů překládá `INNER JOIN` v nepřítomnosti `DefaultIfEmpty` a `LEFT JOIN` při `DefaultIfEmpty` použití.</span><span class="sxs-lookup"><span data-stu-id="224ba-136">Because of this distinction, this kind of queries translates to `INNER JOIN` in the absence of `DefaultIfEmpty` and `LEFT JOIN` when `DefaultIfEmpty` is applied.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
INNER JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

### <a name="collection-selector-references-outer-in-a-non-where-case"></a><span data-ttu-id="224ba-137">Volič kolekce odkazuje na vnější v případě, že</span><span class="sxs-lookup"><span data-stu-id="224ba-137">Collection selector references outer in a non-where case</span></span>

<span data-ttu-id="224ba-138">Když selekátor kolekce odkazuje na vnější prvek, který není v where klauzule (jako případ výše), nepřekládá do spojení databáze.</span><span class="sxs-lookup"><span data-stu-id="224ba-138">When the collection selector references the outer element, which isn't in a where clause (as the case above), it doesn't translate to a database join.</span></span> <span data-ttu-id="224ba-139">To je důvod, proč musíme vyhodnotit výběr kolekce pro každý vnější prvek.</span><span class="sxs-lookup"><span data-stu-id="224ba-139">That's why we need to evaluate the collection selector for each outer element.</span></span> <span data-ttu-id="224ba-140">Překládá se `APPLY` na operace v mnoha relačních databázích.</span><span class="sxs-lookup"><span data-stu-id="224ba-140">It translates to `APPLY` operations in many relational databases.</span></span> <span data-ttu-id="224ba-141">Pokud kolekce je prázdný pro vnější prvek, pak žádné výsledky by být generovány pro tento vnější prvek.</span><span class="sxs-lookup"><span data-stu-id="224ba-141">If the collection is empty for an outer element, then no results would be generated for that outer element.</span></span> <span data-ttu-id="224ba-142">Ale `DefaultIfEmpty` pokud je použita na voliče kolekce pak vnější prvek bude spojen s výchozí hodnotou vnitřní prvek.</span><span class="sxs-lookup"><span data-stu-id="224ba-142">But if `DefaultIfEmpty` is applied on the collection selector then the outer element will be connected with a default value of the inner element.</span></span> <span data-ttu-id="224ba-143">Z důvodu tohoto rozlišení tento druh dotazů překládá `CROSS APPLY` v nepřítomnosti `DefaultIfEmpty` a `OUTER APPLY` při `DefaultIfEmpty` použití.</span><span class="sxs-lookup"><span data-stu-id="224ba-143">Because of this distinction, this kind of queries translates to `CROSS APPLY` in the absence of `DefaultIfEmpty` and `OUTER APPLY` when `DefaultIfEmpty` is applied.</span></span> <span data-ttu-id="224ba-144">Některé databáze jako SQLite nepodporují `APPLY` operátory, takže tento druh dotazu nemusí být přeložen.</span><span class="sxs-lookup"><span data-stu-id="224ba-144">Certain databases like SQLite don't support `APPLY` operators so this kind of query may not be translated.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToApply)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
CROSS APPLY [Posts] AS [p]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
OUTER APPLY [Posts] AS [p]
```

## <a name="groupby"></a><span data-ttu-id="224ba-145">GroupBy</span><span class="sxs-lookup"><span data-stu-id="224ba-145">GroupBy</span></span>

<span data-ttu-id="224ba-146">LINQ GroupBy operátory vytvořit `IGrouping<TKey, TElement>` `TKey` výsledek `TElement` typu, kde a může být libovolný typ.</span><span class="sxs-lookup"><span data-stu-id="224ba-146">LINQ GroupBy operators create a result of type `IGrouping<TKey, TElement>` where `TKey` and `TElement` could be any arbitrary type.</span></span> <span data-ttu-id="224ba-147">Kromě toho `IGrouping` implementuje `IEnumerable<TElement>`, což znamená, že můžete sestavit přes něj pomocí libovolného operátoru LINQ po seskupení.</span><span class="sxs-lookup"><span data-stu-id="224ba-147">Furthermore, `IGrouping` implements `IEnumerable<TElement>`, which means you can compose over it using any LINQ operator after the grouping.</span></span> <span data-ttu-id="224ba-148">Vzhledem k `IGrouping`tomu, že žádná struktura databáze může představovat , GroupBy operátory nemají žádný překlad ve většině případů.</span><span class="sxs-lookup"><span data-stu-id="224ba-148">Since no database structure can represent an `IGrouping`, GroupBy operators have no translation in most cases.</span></span> <span data-ttu-id="224ba-149">Při agregační operátor je použita pro každou skupinu, která `GROUP BY` vrátí skalární, může být přeložen do SQL v relačních databázích.</span><span class="sxs-lookup"><span data-stu-id="224ba-149">When an aggregate operator is applied to each group, which returns a scalar, it can be translated to SQL `GROUP BY` in relational databases.</span></span> <span data-ttu-id="224ba-150">SQL `GROUP BY` je také omezující.</span><span class="sxs-lookup"><span data-stu-id="224ba-150">The SQL `GROUP BY` is restrictive too.</span></span> <span data-ttu-id="224ba-151">Vyžaduje, abyste seskupit pouze skalární hodnoty.</span><span class="sxs-lookup"><span data-stu-id="224ba-151">It requires you to group only by scalar values.</span></span> <span data-ttu-id="224ba-152">Projekce může obsahovat pouze seskupení klíčových sloupců nebo všechny agregáty aplikované přes sloupec.</span><span class="sxs-lookup"><span data-stu-id="224ba-152">The projection can only contain grouping key columns or any aggregate applied over a column.</span></span> <span data-ttu-id="224ba-153">EF Core identifikuje tento vzor a překládá jej na server, jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="224ba-153">EF Core identifies this pattern and translates it to the server, as in the following example:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupBy)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
```

<span data-ttu-id="224ba-154">EF Core také překládá dotazy, kde agregační operátor na seskupení se zobrazí v Kde nebo OrderBy (nebo jiné řazení) LINQ operátor.</span><span class="sxs-lookup"><span data-stu-id="224ba-154">EF Core also translates queries where an aggregate operator on the grouping appears in a Where or OrderBy (or other ordering) LINQ operator.</span></span> <span data-ttu-id="224ba-155">Používá `HAVING` klauzuli v SQL pro where klauzule.</span><span class="sxs-lookup"><span data-stu-id="224ba-155">It uses `HAVING` clause in SQL for the where clause.</span></span> <span data-ttu-id="224ba-156">Část dotazu před použitím GroupBy operátor může být libovolný složitý dotaz tak dlouho, dokud jej lze přeložit na server.</span><span class="sxs-lookup"><span data-stu-id="224ba-156">The part of the query before applying the GroupBy operator can be any complex query as long as it can be translated to server.</span></span> <span data-ttu-id="224ba-157">Kromě toho po použití agregační operátory na dotaz seskupení odebrat seskupení z výsledného zdroje, můžete vytvořit nad ním jako jakýkoli jiný dotaz.</span><span class="sxs-lookup"><span data-stu-id="224ba-157">Furthermore, once you apply aggregate operators on a grouping query to remove groupings from the resulting source, you can compose on top of it like any other query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupByFilter)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
HAVING COUNT(*) > 0
ORDER BY [p].[AuthorId]
```

<span data-ttu-id="224ba-158">Agregované operátory EF Core podporuje jsou následující</span><span class="sxs-lookup"><span data-stu-id="224ba-158">The aggregate operators EF Core supports are as follows</span></span>

- <span data-ttu-id="224ba-159">Průměr</span><span class="sxs-lookup"><span data-stu-id="224ba-159">Average</span></span>
- <span data-ttu-id="224ba-160">Počet</span><span class="sxs-lookup"><span data-stu-id="224ba-160">Count</span></span>
- <span data-ttu-id="224ba-161">Dlouhý počet</span><span class="sxs-lookup"><span data-stu-id="224ba-161">LongCount</span></span>
- <span data-ttu-id="224ba-162">Maximum</span><span class="sxs-lookup"><span data-stu-id="224ba-162">Max</span></span>
- <span data-ttu-id="224ba-163">Minimum</span><span class="sxs-lookup"><span data-stu-id="224ba-163">Min</span></span>
- <span data-ttu-id="224ba-164">Součet</span><span class="sxs-lookup"><span data-stu-id="224ba-164">Sum</span></span>

## <a name="left-join"></a><span data-ttu-id="224ba-165">Levé spojení</span><span class="sxs-lookup"><span data-stu-id="224ba-165">Left Join</span></span>

<span data-ttu-id="224ba-166">Zatímco left join není operátor LINQ, relační databáze mají koncept levého spojení, který se často používá v dotazech.</span><span class="sxs-lookup"><span data-stu-id="224ba-166">While Left Join isn't a LINQ operator, relational databases have the concept of a Left Join which is frequently used in queries.</span></span> <span data-ttu-id="224ba-167">Konkrétní vzor v linq dotazy poskytuje stejný `LEFT JOIN` výsledek jako na serveru.</span><span class="sxs-lookup"><span data-stu-id="224ba-167">A particular pattern in LINQ queries gives the same result as a `LEFT JOIN` on the server.</span></span> <span data-ttu-id="224ba-168">EF Core identifikuje tyto vzory a `LEFT JOIN` generuje ekvivalent na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="224ba-168">EF Core identifies such patterns and generates the equivalent `LEFT JOIN` on the server side.</span></span> <span data-ttu-id="224ba-169">Vzor zahrnuje vytvoření GroupJoin mezi oba zdroje dat a potom sloučení seskupení pomocí SelectMany operátor s DefaultIfEmpty na zdroj seskupení tak, aby odpovídaly null, když vnitřní nemá související prvek.</span><span class="sxs-lookup"><span data-stu-id="224ba-169">The pattern involves creating a GroupJoin between both the data sources and then flattening out the grouping by using the SelectMany operator with DefaultIfEmpty on the grouping source to match null when the inner doesn't have a related element.</span></span> <span data-ttu-id="224ba-170">Následující příklad ukazuje, jak tento vzor vypadá a co generuje.</span><span class="sxs-lookup"><span data-stu-id="224ba-170">The following example shows what that pattern looks like and what it generates.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#LeftJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

<span data-ttu-id="224ba-171">Výše uvedený vzorek vytvoří komplexní strukturu ve stromu výrazů.</span><span class="sxs-lookup"><span data-stu-id="224ba-171">The above pattern creates a complex structure in the expression tree.</span></span> <span data-ttu-id="224ba-172">Z tohoto důvodu EF Core vyžaduje, abyste vyrovnat výsledky seskupení GroupJoin operátor v kroku bezprostředně po operátor.</span><span class="sxs-lookup"><span data-stu-id="224ba-172">Because of that, EF Core requires you to flatten out the grouping results of the GroupJoin operator in a step immediately following the operator.</span></span> <span data-ttu-id="224ba-173">I v případě, že GroupJoin-DefaultIfEmpty-SelectMany se používá, ale v jiném vzoru, nemusíme identifikovat jako levé spojení.</span><span class="sxs-lookup"><span data-stu-id="224ba-173">Even if the GroupJoin-DefaultIfEmpty-SelectMany is used but in a different pattern, we may not identify it as a Left Join.</span></span>
