---
title: Složité operátory dotazů – EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 2e187a2a-4072-4198-9040-aaad68e424fd
uid: core/querying/complex-query-operators
ms.openlocfilehash: 44c2695ea003da043925740a52596fd27da638f8
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417741"
---
# <a name="complex-query-operators"></a><span data-ttu-id="ae236-102">Komplexní operátory dotazů</span><span class="sxs-lookup"><span data-stu-id="ae236-102">Complex Query Operators</span></span>

<span data-ttu-id="ae236-103">Jazykově integrovaný dotaz (LINQ) obsahuje mnoho složitých operátorů, které kombinují více zdrojů dat nebo provádí komplexní zpracování.</span><span class="sxs-lookup"><span data-stu-id="ae236-103">Language Integrated Query (LINQ) contains many complex operators, which combine multiple data sources or does complex processing.</span></span> <span data-ttu-id="ae236-104">Ne všechny operátory LINQ mají vhodné překlady na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="ae236-104">Not all LINQ operators have suitable translations on the server side.</span></span> <span data-ttu-id="ae236-105">V některých případech se dotaz v jednom formuláři přeloží na server, ale pokud je výsledek stejný, nepřeloží se i v případě, že je napsaný v jiném formuláři.</span><span class="sxs-lookup"><span data-stu-id="ae236-105">Sometimes, a query in one form translates to the server but if written in a different form doesn't translate even if the result is the same.</span></span> <span data-ttu-id="ae236-106">Tato stránka popisuje některé ze složitých operátorů a jejich podporované variace.</span><span class="sxs-lookup"><span data-stu-id="ae236-106">This page describes some of the complex operators and their supported variations.</span></span> <span data-ttu-id="ae236-107">V budoucích verzích můžeme rozpoznat více vzorů a přidat jejich odpovídající překlady.</span><span class="sxs-lookup"><span data-stu-id="ae236-107">In future releases, we may recognize more patterns and add their corresponding translations.</span></span> <span data-ttu-id="ae236-108">Je také důležité vzít v úvahu, že podpora překladu mezi poskytovateli se liší.</span><span class="sxs-lookup"><span data-stu-id="ae236-108">It's also important to keep in mind that translation support varies between providers.</span></span> <span data-ttu-id="ae236-109">Konkrétní dotaz, který je přeložen v SqlServer, nemusí fungovat pro databáze SQLite.</span><span class="sxs-lookup"><span data-stu-id="ae236-109">A particular query, which is translated in SqlServer, may not work for SQLite databases.</span></span>

> [!TIP]
> <span data-ttu-id="ae236-110">[Ukázku](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) tohoto článku můžete zobrazit na GitHubu.</span><span class="sxs-lookup"><span data-stu-id="ae236-110">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="join"></a><span data-ttu-id="ae236-111">Spojit</span><span class="sxs-lookup"><span data-stu-id="ae236-111">Join</span></span>

<span data-ttu-id="ae236-112">Operátor LINQ JOIN umožňuje propojit dva zdroje dat na základě výběru klíče pro každý zdroj a vygenerovat řazenou kolekci hodnot, pokud se klíč shoduje.</span><span class="sxs-lookup"><span data-stu-id="ae236-112">The LINQ Join operator allows you to connect two data sources based on the key selector for each source, generating a tuple of values when the key matches.</span></span> <span data-ttu-id="ae236-113">Přirozeně se překládá na `INNER JOIN` relačních databází.</span><span class="sxs-lookup"><span data-stu-id="ae236-113">It naturally translates to `INNER JOIN` on relational databases.</span></span> <span data-ttu-id="ae236-114">I když má spojení LINQ k vnějšímu a základnímu selektorům klíčů, databáze vyžaduje jednu podmínku připojení.</span><span class="sxs-lookup"><span data-stu-id="ae236-114">While the LINQ Join has outer and inner key selectors, the database requires a single join condition.</span></span> <span data-ttu-id="ae236-115">Takže EF Core vygeneruje podmínku spojení porovnáním vnějšího selektoru klíče s selektorm vnitřních klíčů pro rovnost.</span><span class="sxs-lookup"><span data-stu-id="ae236-115">So EF Core generates a join condition by comparing the outer key selector to the inner key selector for equality.</span></span> <span data-ttu-id="ae236-116">Pokud jsou například selektory klíče anonymní typy, EF Core vygeneruje podmínku JOIN pro porovnání vhodné součásti rovnosti.</span><span class="sxs-lookup"><span data-stu-id="ae236-116">Further, if the key selectors are anonymous types, EF Core generates a join condition to compare equality component wise.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#Join)]

```SQL
SELECT [p].[PersonId], [p].[Name], [p].[PhotoId], [p0].[PersonPhotoId], [p0].[Caption], [p0].[Photo]
FROM [PersonPhoto] AS [p0]
INNER JOIN [Person] AS [p] ON [p0].[PersonPhotoId] = [p].[PhotoId]
```

## <a name="groupjoin"></a><span data-ttu-id="ae236-117">GroupJoin</span><span class="sxs-lookup"><span data-stu-id="ae236-117">GroupJoin</span></span>

<span data-ttu-id="ae236-118">Operátor LINQ GroupJoin vám umožňuje propojit dva zdroje dat, podobně jako Join, ale vytvoří skupinu vnitřních hodnot pro porovnání vnějších prvků.</span><span class="sxs-lookup"><span data-stu-id="ae236-118">The LINQ GroupJoin operator allows you to connect two data sources similar to Join, but it creates a group of inner values for matching outer elements.</span></span> <span data-ttu-id="ae236-119">Provedení dotazu, jako je následující příklad, generuje výsledek `Blog` & `IEnumerable<Post>`.</span><span class="sxs-lookup"><span data-stu-id="ae236-119">Executing a query like the following example generates a result of `Blog` & `IEnumerable<Post>`.</span></span> <span data-ttu-id="ae236-120">Vzhledem k tomu, že databáze (zejména relační databáze) nemají způsob reprezentace kolekce objektů na straně klienta, GroupJoin nepřevádí na server v mnoha případech.</span><span class="sxs-lookup"><span data-stu-id="ae236-120">Since databases (especially relational databases) don't have a way to represent a collection of client-side objects, GroupJoin doesn't translate to the server in many cases.</span></span> <span data-ttu-id="ae236-121">Vyžaduje, abyste získali všechna data ze serveru tak, aby se GroupJoin bez speciálního selektoru (první dotaz níže).</span><span class="sxs-lookup"><span data-stu-id="ae236-121">It requires you to get all of the data from the server to do GroupJoin without a special selector (first query below).</span></span> <span data-ttu-id="ae236-122">Pokud však selektor omezuje vybraná data, může dojít k problémům s výkonem (druhý dotaz níže) a načtení všech dat ze serveru.</span><span class="sxs-lookup"><span data-stu-id="ae236-122">But if the selector is limiting data being selected then fetching all of the data from the server may cause performance issues (second query below).</span></span> <span data-ttu-id="ae236-123">To je důvod, proč EF Core nepřekládat GroupJoin.</span><span class="sxs-lookup"><span data-stu-id="ae236-123">That's why EF Core doesn't translate GroupJoin.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoin)]

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoinComposed)]

## <a name="selectmany"></a><span data-ttu-id="ae236-124">Operátor SelectMany</span><span class="sxs-lookup"><span data-stu-id="ae236-124">SelectMany</span></span>

<span data-ttu-id="ae236-125">Operátor LINQ operátor SelectMany umožňuje vytvořit výčet v selektoru kolekce pro každý vnější prvek a generovat řazené kolekce členů hodnot z každého zdroje dat.</span><span class="sxs-lookup"><span data-stu-id="ae236-125">The LINQ SelectMany operator allows you to enumerate over a collection selector for each outer element and generate tuples of values from each data source.</span></span> <span data-ttu-id="ae236-126">Způsobem je to spojení, ale bez jakékoliv podmínky, takže každý vnější prvek je připojen pomocí elementu ze zdroje kolekce.</span><span class="sxs-lookup"><span data-stu-id="ae236-126">In a way, it's a join but without any condition so every outer element is connected with an element from the collection source.</span></span> <span data-ttu-id="ae236-127">V závislosti na tom, jak je selektor kolekce v relaci s vnějším zdrojem dat, může operátor SelectMany překládat do různých různých dotazů na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="ae236-127">Depending on how the collection selector is related to the outer data source, SelectMany can translate into various different queries on the server side.</span></span>

### <a name="collection-selector-doesnt-reference-outer"></a><span data-ttu-id="ae236-128">Selektor kolekce neodkazuje na vnější</span><span class="sxs-lookup"><span data-stu-id="ae236-128">Collection selector doesn't reference outer</span></span>

<span data-ttu-id="ae236-129">Když selektor kolekce neodkazuje na cokoli od vnějšího zdroje, výsledkem je kartézském produkt obou zdrojů dat.</span><span class="sxs-lookup"><span data-stu-id="ae236-129">When the collection selector isn't referencing anything from the outer source, the result is a cartesian product of both data sources.</span></span> <span data-ttu-id="ae236-130">Překládá se `CROSS JOIN` v relačních databázích.</span><span class="sxs-lookup"><span data-stu-id="ae236-130">It translates to `CROSS JOIN` in relational databases.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToCrossJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
CROSS JOIN [Posts] AS [p]
```

### <a name="collection-selector-references-outer-in-a-where-clause"></a><span data-ttu-id="ae236-131">Selektor kolekce odkazuje vnější v klauzuli WHERE.</span><span class="sxs-lookup"><span data-stu-id="ae236-131">Collection selector references outer in a where clause</span></span>

<span data-ttu-id="ae236-132">Když selektor kolekce obsahuje klauzuli WHERE, která odkazuje na vnější prvek, pak EF Core přeloží na připojení k databázi a použije predikát jako podmínku JOIN.</span><span class="sxs-lookup"><span data-stu-id="ae236-132">When the collection selector has a where clause, which references the outer element, then EF Core translates it to a database join and uses the predicate as the join condition.</span></span> <span data-ttu-id="ae236-133">Tento případ se obvykle vyskytne při použití navigace kolekce na vnějším prvku jako selektor kolekce.</span><span class="sxs-lookup"><span data-stu-id="ae236-133">Normally this case arises when using collection navigation on the outer element as the collection selector.</span></span> <span data-ttu-id="ae236-134">Pokud je kolekce prázdná pro vnější prvek, nebudou pro tento vnější prvek generovány žádné výsledky.</span><span class="sxs-lookup"><span data-stu-id="ae236-134">If the collection is empty for an outer element, then no results would be generated for that outer element.</span></span> <span data-ttu-id="ae236-135">Pokud je však v selektoru kolekce použita `DefaultIfEmpty`, vnější prvek bude propojen s výchozí hodnotou vnitřního prvku.</span><span class="sxs-lookup"><span data-stu-id="ae236-135">But if `DefaultIfEmpty` is applied on the collection selector then the outer element will be connected with a default value of the inner element.</span></span> <span data-ttu-id="ae236-136">Z tohoto důvodu tento druh dotazů překládá `INNER JOIN` nepřítomnost `DefaultIfEmpty` a `LEFT JOIN` při použití `DefaultIfEmpty`.</span><span class="sxs-lookup"><span data-stu-id="ae236-136">Because of this distinction, this kind of queries translates to `INNER JOIN` in the absence of `DefaultIfEmpty` and `LEFT JOIN` when `DefaultIfEmpty` is applied.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
INNER JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

### <a name="collection-selector-references-outer-in-a-non-where-case"></a><span data-ttu-id="ae236-137">Selektor kolekce odkazuje na vnější v případě, že ne.</span><span class="sxs-lookup"><span data-stu-id="ae236-137">Collection selector references outer in a non-where case</span></span>

<span data-ttu-id="ae236-138">Když selektor kolekce odkazuje na vnější prvek, který není v klauzuli WHERE (v případě výše uvedeného případu), nepřeloží se na připojení k databázi.</span><span class="sxs-lookup"><span data-stu-id="ae236-138">When the collection selector references the outer element, which isn't in a where clause (as the case above), it doesn't translate to a database join.</span></span> <span data-ttu-id="ae236-139">To je důvod, proč musíme vyhodnotit selektor kolekce pro každý vnější element.</span><span class="sxs-lookup"><span data-stu-id="ae236-139">That's why we need to evaluate the collection selector for each outer element.</span></span> <span data-ttu-id="ae236-140">Překládá se na operace `APPLY` v mnoha relačních databázích.</span><span class="sxs-lookup"><span data-stu-id="ae236-140">It translates to `APPLY` operations in many relational databases.</span></span> <span data-ttu-id="ae236-141">Pokud je kolekce prázdná pro vnější prvek, nebudou pro tento vnější prvek generovány žádné výsledky.</span><span class="sxs-lookup"><span data-stu-id="ae236-141">If the collection is empty for an outer element, then no results would be generated for that outer element.</span></span> <span data-ttu-id="ae236-142">Pokud je však v selektoru kolekce použita `DefaultIfEmpty`, vnější prvek bude propojen s výchozí hodnotou vnitřního prvku.</span><span class="sxs-lookup"><span data-stu-id="ae236-142">But if `DefaultIfEmpty` is applied on the collection selector then the outer element will be connected with a default value of the inner element.</span></span> <span data-ttu-id="ae236-143">Z tohoto důvodu tento druh dotazů překládá `CROSS APPLY` nepřítomnost `DefaultIfEmpty` a `OUTER APPLY` při použití `DefaultIfEmpty`.</span><span class="sxs-lookup"><span data-stu-id="ae236-143">Because of this distinction, this kind of queries translates to `CROSS APPLY` in the absence of `DefaultIfEmpty` and `OUTER APPLY` when `DefaultIfEmpty` is applied.</span></span> <span data-ttu-id="ae236-144">Některé databáze, jako je SQLite, nepodporují `APPLY` operátory, takže tento typ dotazu není možné přeložit.</span><span class="sxs-lookup"><span data-stu-id="ae236-144">Certain databases like SQLite don't support `APPLY` operators so this kind of query may not be translated.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToApply)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
CROSS APPLY [Posts] AS [p]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
OUTER APPLY [Posts] AS [p]
```

## <a name="groupby"></a><span data-ttu-id="ae236-145">GroupBy</span><span class="sxs-lookup"><span data-stu-id="ae236-145">GroupBy</span></span>

<span data-ttu-id="ae236-146">Operátory LINQ GroupBy vytvoří výsledek typu `IGrouping<TKey, TElement>`, kde `TKey` a `TElement` by mohly být libovolného typu.</span><span class="sxs-lookup"><span data-stu-id="ae236-146">LINQ GroupBy operators create a result of type `IGrouping<TKey, TElement>` where `TKey` and `TElement` could be any arbitrary type.</span></span> <span data-ttu-id="ae236-147">Kromě toho `IGrouping` implementuje `IEnumerable<TElement>`, což znamená, že ho můžete po seskupení použít pomocí libovolného operátoru LINQ.</span><span class="sxs-lookup"><span data-stu-id="ae236-147">Furthermore, `IGrouping` implements `IEnumerable<TElement>`, which means you can compose over it using any LINQ operator after the grouping.</span></span> <span data-ttu-id="ae236-148">Vzhledem k tomu, že žádná struktura databáze nemůže představovat `IGrouping`, operátory GroupBy nemají ve většině případů žádný převod.</span><span class="sxs-lookup"><span data-stu-id="ae236-148">Since no database structure can represent an `IGrouping`, GroupBy operators have no translation in most cases.</span></span> <span data-ttu-id="ae236-149">Při použití agregačního operátoru na každou skupinu, která vrací skalární hodnotu, lze ji přeložit na SQL `GROUP BY` v relačních databázích.</span><span class="sxs-lookup"><span data-stu-id="ae236-149">When an aggregate operator is applied to each group, which returns a scalar, it can be translated to SQL `GROUP BY` in relational databases.</span></span> <span data-ttu-id="ae236-150">`GROUP BY` SQL je omezující.</span><span class="sxs-lookup"><span data-stu-id="ae236-150">The SQL `GROUP BY` is restrictive too.</span></span> <span data-ttu-id="ae236-151">Vyžaduje, abyste je mohli seskupit pouze pomocí skalárních hodnot.</span><span class="sxs-lookup"><span data-stu-id="ae236-151">It requires you to group only by scalar values.</span></span> <span data-ttu-id="ae236-152">Projekce může obsahovat pouze sloupce klíče seskupení nebo agregace použité na sloupec.</span><span class="sxs-lookup"><span data-stu-id="ae236-152">The projection can only contain grouping key columns or any aggregate applied over a column.</span></span> <span data-ttu-id="ae236-153">EF Core tento model identifikuje a překládá ho na server, jak je uvedeno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="ae236-153">EF Core identifies this pattern and translates it to the server, as in the following example:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupBy)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
```

<span data-ttu-id="ae236-154">EF Core také přeloží dotazy, kde se agregační operátor ve skupině zobrazuje v operátoru LINQ nebo OrderBy (nebo jiném řazení).</span><span class="sxs-lookup"><span data-stu-id="ae236-154">EF Core also translates queries where an aggregate operator on the grouping appears in a Where or OrderBy (or other ordering) LINQ operator.</span></span> <span data-ttu-id="ae236-155">Používá klauzuli `HAVING` v SQL pro klauzuli WHERE.</span><span class="sxs-lookup"><span data-stu-id="ae236-155">It uses `HAVING` clause in SQL for the where clause.</span></span> <span data-ttu-id="ae236-156">Část dotazu před použitím operátoru GroupBy může být jakýkoli složitý dotaz, pokud jej lze přeložit na server.</span><span class="sxs-lookup"><span data-stu-id="ae236-156">The part of the query before applying the GroupBy operator can be any complex query as long as it can be translated to server.</span></span> <span data-ttu-id="ae236-157">Kromě toho, když použijete agregační operátory pro dotaz seskupení a odeberete seskupení ze výsledného zdroje, můžete vytvořit jejich sestavování jako jakýkoliv jiný dotaz.</span><span class="sxs-lookup"><span data-stu-id="ae236-157">Furthermore, once you apply aggregate operators on a grouping query to remove groupings from the resulting source, you can compose on top of it like any other query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupByFilter)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
HAVING COUNT(*) > 0
ORDER BY [p].[AuthorId]
```

<span data-ttu-id="ae236-158">Agregační operátory EF Core podporovány jsou následující.</span><span class="sxs-lookup"><span data-stu-id="ae236-158">The aggregate operators EF Core supports are as follows</span></span>

- <span data-ttu-id="ae236-159">Průměr</span><span class="sxs-lookup"><span data-stu-id="ae236-159">Average</span></span>
- <span data-ttu-id="ae236-160">Počet</span><span class="sxs-lookup"><span data-stu-id="ae236-160">Count</span></span>
- <span data-ttu-id="ae236-161">LongCount</span><span class="sxs-lookup"><span data-stu-id="ae236-161">LongCount</span></span>
- <span data-ttu-id="ae236-162">Maximum</span><span class="sxs-lookup"><span data-stu-id="ae236-162">Max</span></span>
- <span data-ttu-id="ae236-163">Minimum</span><span class="sxs-lookup"><span data-stu-id="ae236-163">Min</span></span>
- <span data-ttu-id="ae236-164">Součet</span><span class="sxs-lookup"><span data-stu-id="ae236-164">Sum</span></span>

## <a name="left-join"></a><span data-ttu-id="ae236-165">Vlevo připojit</span><span class="sxs-lookup"><span data-stu-id="ae236-165">Left Join</span></span>

<span data-ttu-id="ae236-166">Zatímco levé spojení není operátor LINQ, relační databáze mají koncept levého spojení, který se často používá v dotazech.</span><span class="sxs-lookup"><span data-stu-id="ae236-166">While Left Join isn't a LINQ operator, relational databases have the concept of a Left Join which is frequently used in queries.</span></span> <span data-ttu-id="ae236-167">Konkrétní vzor v dotazech LINQ poskytuje stejný výsledek jako `LEFT JOIN` na serveru.</span><span class="sxs-lookup"><span data-stu-id="ae236-167">A particular pattern in LINQ queries gives the same result as a `LEFT JOIN` on the server.</span></span> <span data-ttu-id="ae236-168">EF Core identifikuje takové vzory a vygeneruje ekvivalentní `LEFT JOIN` na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="ae236-168">EF Core identifies such patterns and generates the equivalent `LEFT JOIN` on the server side.</span></span> <span data-ttu-id="ae236-169">Tento vzor zahrnuje vytvoření GroupJoin mezi zdroji dat a následné sloučení skupin pomocí operátoru operátor SelectMany s DefaultIfEmpty ve zdroji seskupení, aby odpovídal hodnotě null, pokud vnitřní nemá související prvek.</span><span class="sxs-lookup"><span data-stu-id="ae236-169">The pattern involves creating a GroupJoin between both the data sources and then flattening out the grouping by using the SelectMany operator with DefaultIfEmpty on the grouping source to match null when the inner doesn't have a related element.</span></span> <span data-ttu-id="ae236-170">Následující příklad ukazuje, jak tento vzor vypadá a co generuje.</span><span class="sxs-lookup"><span data-stu-id="ae236-170">The following example shows what that pattern looks like and what it generates.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#LeftJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

<span data-ttu-id="ae236-171">Výše uvedený vzor vytvoří komplexní strukturu ve stromu výrazu.</span><span class="sxs-lookup"><span data-stu-id="ae236-171">The above pattern creates a complex structure in the expression tree.</span></span> <span data-ttu-id="ae236-172">Z tohoto důvodu EF Core vyžaduje, abyste v kroku hned za operátorem naseskupili výsledky seskupení GroupJoin operátoru.</span><span class="sxs-lookup"><span data-stu-id="ae236-172">Because of that, EF Core requires you to flatten out the grouping results of the GroupJoin operator in a step immediately following the operator.</span></span> <span data-ttu-id="ae236-173">I v případě, že se používá GroupJoin-DefaultIfEmpty-operátor SelectMany, ale v jiném vzoru, nemůžeme ho identifikovat jako levou spojení.</span><span class="sxs-lookup"><span data-stu-id="ae236-173">Even if the GroupJoin-DefaultIfEmpty-SelectMany is used but in a different pattern, we may not identify it as a Left Join.</span></span>
