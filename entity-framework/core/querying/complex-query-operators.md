---
title: Složité operátory dotazů – EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 2e187a2a-4072-4198-9040-aaad68e424fd
uid: core/querying/complex-query-operators
ms.openlocfilehash: 350a7fa6a3ee1de16bad4b63e10842f9356a1b60
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72186258"
---
# <a name="complex-query-operators"></a><span data-ttu-id="53b89-102">Složité operátory dotazů</span><span class="sxs-lookup"><span data-stu-id="53b89-102">Complex Query Operators</span></span>

<span data-ttu-id="53b89-103">Jazykově integrovaný dotaz (LINQ) obsahuje mnoho složitých operátorů, které kombinují více zdrojů dat nebo provádí komplexní zpracování.</span><span class="sxs-lookup"><span data-stu-id="53b89-103">Language Integrated Query (LINQ) contains many complex operators, which combine multiple data sources or does complex processing.</span></span> <span data-ttu-id="53b89-104">Ne všechny operátory LINQ mají vhodné překlady na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="53b89-104">Not all LINQ operators have suitable translations on the server side.</span></span> <span data-ttu-id="53b89-105">V některých případech se dotaz v jednom formuláři přeloží na server, ale pokud je výsledek stejný, nepřeloží se i v případě, že je napsaný v jiném formuláři.</span><span class="sxs-lookup"><span data-stu-id="53b89-105">Sometimes, a query in one form translates to the server but if written in a different form doesn't translate even if the result is the same.</span></span> <span data-ttu-id="53b89-106">Tato stránka popisuje některé ze složitých operátorů a jejich podporované variace.</span><span class="sxs-lookup"><span data-stu-id="53b89-106">This page describes some of the complex operators and their supported variations.</span></span> <span data-ttu-id="53b89-107">V budoucích verzích můžeme rozpoznat více vzorů a přidat jejich odpovídající překlady.</span><span class="sxs-lookup"><span data-stu-id="53b89-107">In future releases, we may recognize more patterns and add their corresponding translations.</span></span> <span data-ttu-id="53b89-108">Je také důležité vzít v úvahu, že podpora překladu mezi poskytovateli se liší.</span><span class="sxs-lookup"><span data-stu-id="53b89-108">It's also important to keep in mind that translation support varies between providers.</span></span> <span data-ttu-id="53b89-109">Konkrétní dotaz, který je přeložen v SqlServer, nemusí fungovat pro databáze SQLite.</span><span class="sxs-lookup"><span data-stu-id="53b89-109">A particular query, which is translated in SqlServer, may not work for SQLite databases.</span></span>

> [!TIP]
> <span data-ttu-id="53b89-110">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="53b89-110">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="join"></a><span data-ttu-id="53b89-111">Spojit</span><span class="sxs-lookup"><span data-stu-id="53b89-111">Join</span></span>

<span data-ttu-id="53b89-112">Operátor LINQ JOIN umožňuje propojit dva zdroje dat na základě výběru klíče pro každý zdroj a vygenerovat řazenou kolekci hodnot, pokud se klíč shoduje.</span><span class="sxs-lookup"><span data-stu-id="53b89-112">The LINQ Join operator allows you to connect two data sources based on the key selector for each source, generating a tuple of values when the key matches.</span></span> <span data-ttu-id="53b89-113">Přirozeně se překládá na `INNER JOIN` v relačních databázích.</span><span class="sxs-lookup"><span data-stu-id="53b89-113">It naturally translates to `INNER JOIN` on relational databases.</span></span> <span data-ttu-id="53b89-114">I když má spojení LINQ k vnějšímu a základnímu selektorům klíčů, databáze vyžaduje jednu podmínku připojení.</span><span class="sxs-lookup"><span data-stu-id="53b89-114">While the LINQ Join has outer and inner key selectors, the database requires a single join condition.</span></span> <span data-ttu-id="53b89-115">Takže EF Core vygeneruje podmínku spojení porovnáním vnějšího selektoru klíče s selektorm vnitřních klíčů pro rovnost.</span><span class="sxs-lookup"><span data-stu-id="53b89-115">So EF Core generates a join condition by comparing the outer key selector to the inner key selector for equality.</span></span> <span data-ttu-id="53b89-116">Pokud jsou například selektory klíče anonymní typy, EF Core vygeneruje podmínku JOIN pro porovnání vhodné součásti rovnosti.</span><span class="sxs-lookup"><span data-stu-id="53b89-116">Further, if the key selectors are anonymous types, EF Core generates a join condition to compare equality component wise.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#Join)]

```SQL
SELECT [p].[PersonId], [p].[Name], [p].[PhotoId], [p0].[PersonPhotoId], [p0].[Caption], [p0].[Photo]
FROM [PersonPhoto] AS [p0]
INNER JOIN [Person] AS [p] ON [p0].[PersonPhotoId] = [p].[PhotoId]
```

## <a name="groupjoin"></a><span data-ttu-id="53b89-117">GroupJoin</span><span class="sxs-lookup"><span data-stu-id="53b89-117">GroupJoin</span></span>

<span data-ttu-id="53b89-118">Operátor LINQ GroupJoin vám umožňuje propojit dva zdroje dat, podobně jako Join, ale vytvoří skupinu vnitřních hodnot pro porovnání vnějších prvků.</span><span class="sxs-lookup"><span data-stu-id="53b89-118">The LINQ GroupJoin operator allows you to connect two data sources similar to Join, but it creates a group of inner values for matching outer elements.</span></span> <span data-ttu-id="53b89-119">Provedení dotazu, jako je následující příklad, generuje výsledek `Blog` @ no__t-1 @ no__t-2.</span><span class="sxs-lookup"><span data-stu-id="53b89-119">Executing a query like the following example generates a result of `Blog` & `IEnumerable<Post>`.</span></span> <span data-ttu-id="53b89-120">Vzhledem k tomu, že databáze (zejména relační databáze) nemají způsob reprezentace kolekce objektů na straně klienta, GroupJoin nepřevádí na server v mnoha případech.</span><span class="sxs-lookup"><span data-stu-id="53b89-120">Since databases (especially relational databases) don't have a way to represent a collection of client-side objects, GroupJoin doesn't translate to the server in many cases.</span></span> <span data-ttu-id="53b89-121">Vyžaduje, abyste získali všechna data ze serveru tak, aby se GroupJoin bez speciálního selektoru (první dotaz níže).</span><span class="sxs-lookup"><span data-stu-id="53b89-121">It requires you to get all of the data from the server to do GroupJoin without a special selector (first query below).</span></span> <span data-ttu-id="53b89-122">Pokud však selektor omezuje vybraná data, může dojít k problémům s výkonem (druhý dotaz níže) a načtení všech dat ze serveru.</span><span class="sxs-lookup"><span data-stu-id="53b89-122">But if the selector is limiting data being selected then fetching all of the data from the server may cause performance issues (second query below).</span></span> <span data-ttu-id="53b89-123">To je důvod, proč EF Core nepřekládat GroupJoin.</span><span class="sxs-lookup"><span data-stu-id="53b89-123">That's why EF Core doesn't translate GroupJoin.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoin)]

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoinComposed)]

## <a name="selectmany"></a><span data-ttu-id="53b89-124">Operátor SelectMany</span><span class="sxs-lookup"><span data-stu-id="53b89-124">SelectMany</span></span>

<span data-ttu-id="53b89-125">Operátor LINQ operátor SelectMany umožňuje vytvořit výčet v selektoru kolekce pro každý vnější prvek a generovat řazené kolekce členů hodnot z každého zdroje dat.</span><span class="sxs-lookup"><span data-stu-id="53b89-125">The LINQ SelectMany operator allows you to enumerate over a collection selector for each outer element and generate tuples of values from each data source.</span></span> <span data-ttu-id="53b89-126">Způsobem je to spojení, ale bez jakékoliv podmínky, takže každý vnější prvek je připojen pomocí elementu ze zdroje kolekce.</span><span class="sxs-lookup"><span data-stu-id="53b89-126">In a way, it's a join but without any condition so every outer element is connected with an element from the collection source.</span></span> <span data-ttu-id="53b89-127">V závislosti na tom, jak je selektor kolekce v relaci s vnějším zdrojem dat, může operátor SelectMany překládat do různých různých dotazů na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="53b89-127">Depending on how the collection selector is related to the outer data source, SelectMany can translate into various different queries on the server side.</span></span>

### <a name="collection-selector-doesnt-reference-outer"></a><span data-ttu-id="53b89-128">Selektor kolekce neodkazuje na vnější</span><span class="sxs-lookup"><span data-stu-id="53b89-128">Collection selector doesn't reference outer</span></span>

<span data-ttu-id="53b89-129">Když selektor kolekce neodkazuje na cokoli od vnějšího zdroje, výsledkem je kartézském produkt obou zdrojů dat.</span><span class="sxs-lookup"><span data-stu-id="53b89-129">When the collection selector isn't referencing anything from the outer source, the result is a cartesian product of both data sources.</span></span> <span data-ttu-id="53b89-130">Překládá se na `CROSS JOIN` v relačních databázích.</span><span class="sxs-lookup"><span data-stu-id="53b89-130">It translates to `CROSS JOIN` in relational databases.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToCrossJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
CROSS JOIN [Posts] AS [p]
```

### <a name="collection-selector-references-outer-in-a-where-clause"></a><span data-ttu-id="53b89-131">Selektor kolekce odkazuje vnější v klauzuli WHERE.</span><span class="sxs-lookup"><span data-stu-id="53b89-131">Collection selector references outer in a where clause</span></span>

<span data-ttu-id="53b89-132">Když selektor kolekce obsahuje klauzuli WHERE, která odkazuje na vnější prvek, pak EF Core přeloží na připojení k databázi a použije predikát jako podmínku JOIN.</span><span class="sxs-lookup"><span data-stu-id="53b89-132">When the collection selector has a where clause, which references the outer element, then EF Core translates it to a database join and uses the predicate as the join condition.</span></span> <span data-ttu-id="53b89-133">Tento případ se obvykle vyskytne při použití navigace kolekce na vnějším prvku jako selektor kolekce.</span><span class="sxs-lookup"><span data-stu-id="53b89-133">Normally this case arises when using collection navigation on the outer element as the collection selector.</span></span> <span data-ttu-id="53b89-134">Pokud je kolekce prázdná pro vnější prvek, nebudou pro tento vnější prvek generovány žádné výsledky.</span><span class="sxs-lookup"><span data-stu-id="53b89-134">If the collection is empty for an outer element, then no results would be generated for that outer element.</span></span> <span data-ttu-id="53b89-135">Pokud je však v selektoru kolekcí použito `DefaultIfEmpty`, vnější prvek bude propojen s výchozí hodnotou vnitřního prvku.</span><span class="sxs-lookup"><span data-stu-id="53b89-135">But if `DefaultIfEmpty` is applied on the collection selector then the outer element will be connected with a default value of the inner element.</span></span> <span data-ttu-id="53b89-136">Z tohoto důvodu tento druh dotazů překládá `INNER JOIN` při nepřítomnosti `DefaultIfEmpty` a `LEFT JOIN` při použití `DefaultIfEmpty`.</span><span class="sxs-lookup"><span data-stu-id="53b89-136">Because of this distinction, this kind of queries translates to `INNER JOIN` in the absence of `DefaultIfEmpty` and `LEFT JOIN` when `DefaultIfEmpty` is applied.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
INNER JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

### <a name="collection-selector-references-outer-in-a-non-where-case"></a><span data-ttu-id="53b89-137">Selektor kolekce odkazuje na vnější v případě, že ne.</span><span class="sxs-lookup"><span data-stu-id="53b89-137">Collection selector references outer in a non-where case</span></span>

<span data-ttu-id="53b89-138">Když selektor kolekce odkazuje na vnější prvek, který není v klauzuli WHERE (v případě výše uvedeného případu), nepřeloží se na připojení k databázi.</span><span class="sxs-lookup"><span data-stu-id="53b89-138">When the collection selector references the outer element, which isn't in a where clause (as the case above), it doesn't translate to a database join.</span></span> <span data-ttu-id="53b89-139">To je důvod, proč musíme vyhodnotit selektor kolekce pro každý vnější element.</span><span class="sxs-lookup"><span data-stu-id="53b89-139">That's why we need to evaluate the collection selector for each outer element.</span></span> <span data-ttu-id="53b89-140">Překládá se na operace `APPLY` v mnoha relačních databázích.</span><span class="sxs-lookup"><span data-stu-id="53b89-140">It translates to `APPLY` operations in many relational databases.</span></span> <span data-ttu-id="53b89-141">Pokud je kolekce prázdná pro vnější prvek, nebudou pro tento vnější prvek generovány žádné výsledky.</span><span class="sxs-lookup"><span data-stu-id="53b89-141">If the collection is empty for an outer element, then no results would be generated for that outer element.</span></span> <span data-ttu-id="53b89-142">Pokud je však v selektoru kolekcí použito `DefaultIfEmpty`, vnější prvek bude propojen s výchozí hodnotou vnitřního prvku.</span><span class="sxs-lookup"><span data-stu-id="53b89-142">But if `DefaultIfEmpty` is applied on the collection selector then the outer element will be connected with a default value of the inner element.</span></span> <span data-ttu-id="53b89-143">Z tohoto důvodu tento druh dotazů překládá `CROSS APPLY` při nepřítomnosti `DefaultIfEmpty` a `OUTER APPLY` při použití `DefaultIfEmpty`.</span><span class="sxs-lookup"><span data-stu-id="53b89-143">Because of this distinction, this kind of queries translates to `CROSS APPLY` in the absence of `DefaultIfEmpty` and `OUTER APPLY` when `DefaultIfEmpty` is applied.</span></span> <span data-ttu-id="53b89-144">Některé databáze, jako je SQLite, nepodporují operátory `APPLY`, takže tento typ dotazu není možné přeložit.</span><span class="sxs-lookup"><span data-stu-id="53b89-144">Certain databases like SQLite don't support `APPLY` operators so this kind of query may not be translated.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToApply)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
CROSS APPLY [Posts] AS [p]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
OUTER APPLY [Posts] AS [p]
```

## <a name="groupby"></a><span data-ttu-id="53b89-145">GroupBy</span><span class="sxs-lookup"><span data-stu-id="53b89-145">GroupBy</span></span>

<span data-ttu-id="53b89-146">Operátory LINQ GroupBy vytvoří výsledek typu `IGrouping<TKey, TElement>`, kde `TKey` a `TElement` by mohly být libovolného typu.</span><span class="sxs-lookup"><span data-stu-id="53b89-146">LINQ GroupBy operators create a result of type `IGrouping<TKey, TElement>` where `TKey` and `TElement` could be any arbitrary type.</span></span> <span data-ttu-id="53b89-147">Kromě toho `IGrouping` implementuje `IEnumerable<TElement>`, což znamená, že ho můžete po seskupení použít pomocí libovolného operátoru LINQ.</span><span class="sxs-lookup"><span data-stu-id="53b89-147">Furthermore, `IGrouping` implements `IEnumerable<TElement>`, which means you can compose over it using any LINQ operator after the grouping.</span></span> <span data-ttu-id="53b89-148">Vzhledem k tomu, že žádná struktura databáze nemůže představovat `IGrouping`, operátory GroupBy nemají ve většině případů žádný převod.</span><span class="sxs-lookup"><span data-stu-id="53b89-148">Since no database structure can represent an `IGrouping`, GroupBy operators have no translation in most cases.</span></span> <span data-ttu-id="53b89-149">Při použití agregačního operátoru na každou skupinu, která vrací skalární hodnotu, lze ji přeložit na SQL `GROUP BY` v relačních databázích.</span><span class="sxs-lookup"><span data-stu-id="53b89-149">When an aggregate operator is applied to each group, which returns a scalar, it can be translated to SQL `GROUP BY` in relational databases.</span></span> <span data-ttu-id="53b89-150">@No__t SQL-0 je omezující.</span><span class="sxs-lookup"><span data-stu-id="53b89-150">The SQL `GROUP BY` is restrictive too.</span></span> <span data-ttu-id="53b89-151">Vyžaduje, abyste je mohli seskupit pouze pomocí skalárních hodnot.</span><span class="sxs-lookup"><span data-stu-id="53b89-151">It requires you to group only by scalar values.</span></span> <span data-ttu-id="53b89-152">Projekce může obsahovat pouze sloupce klíče seskupení nebo agregace použité na sloupec.</span><span class="sxs-lookup"><span data-stu-id="53b89-152">The projection can only contain grouping key columns or any aggregate applied over a column.</span></span> <span data-ttu-id="53b89-153">EF Core tento model identifikuje a překládá ho na server, jak je uvedeno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="53b89-153">EF Core identifies this pattern and translates it to the server, as in the following example:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupBy)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
```

<span data-ttu-id="53b89-154">EF Core také přeloží dotazy, kde se agregační operátor ve skupině zobrazuje v operátoru LINQ nebo OrderBy (nebo jiném řazení).</span><span class="sxs-lookup"><span data-stu-id="53b89-154">EF Core also translates queries where an aggregate operator on the grouping appears in a Where or OrderBy (or other ordering) LINQ operator.</span></span> <span data-ttu-id="53b89-155">Používá klauzuli `HAVING` v SQL pro klauzuli WHERE.</span><span class="sxs-lookup"><span data-stu-id="53b89-155">It uses `HAVING` clause in SQL for the where clause.</span></span> <span data-ttu-id="53b89-156">Část dotazu před použitím operátoru GroupBy může být jakýkoli složitý dotaz, pokud jej lze přeložit na server.</span><span class="sxs-lookup"><span data-stu-id="53b89-156">The part of the query before applying the GroupBy operator can be any complex query as long as it can be translated to server.</span></span> <span data-ttu-id="53b89-157">Kromě toho, když použijete agregační operátory pro dotaz seskupení a odeberete seskupení ze výsledného zdroje, můžete vytvořit jejich sestavování jako jakýkoliv jiný dotaz.</span><span class="sxs-lookup"><span data-stu-id="53b89-157">Furthermore, once you apply aggregate operators on a grouping query to remove groupings from the resulting source, you can compose on top of it like any other query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupByFilter)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
HAVING COUNT(*) > 0
ORDER BY [p].[AuthorId]
```

<span data-ttu-id="53b89-158">Agregační operátory EF Core podporovány jsou následující.</span><span class="sxs-lookup"><span data-stu-id="53b89-158">The aggregate operators EF Core supports are as follows</span></span>

- <span data-ttu-id="53b89-159">Average</span><span class="sxs-lookup"><span data-stu-id="53b89-159">Average</span></span>
- <span data-ttu-id="53b89-160">Count</span><span class="sxs-lookup"><span data-stu-id="53b89-160">Count</span></span>
- <span data-ttu-id="53b89-161">LongCount</span><span class="sxs-lookup"><span data-stu-id="53b89-161">LongCount</span></span>
- <span data-ttu-id="53b89-162">Maximum</span><span class="sxs-lookup"><span data-stu-id="53b89-162">Max</span></span>
- <span data-ttu-id="53b89-163">Minimum</span><span class="sxs-lookup"><span data-stu-id="53b89-163">Min</span></span>
- <span data-ttu-id="53b89-164">Součet</span><span class="sxs-lookup"><span data-stu-id="53b89-164">Sum</span></span>

## <a name="left-join"></a><span data-ttu-id="53b89-165">Vlevo připojit</span><span class="sxs-lookup"><span data-stu-id="53b89-165">Left Join</span></span>

<span data-ttu-id="53b89-166">Zatímco levé spojení není operátor LINQ, relační databáze mají koncept levého spojení, který se často používá v dotazech.</span><span class="sxs-lookup"><span data-stu-id="53b89-166">While Left Join isn't a LINQ operator, relational databases have the concept of a Left Join which is frequently used in queries.</span></span> <span data-ttu-id="53b89-167">Konkrétní vzor v dotazech LINQ poskytuje stejný výsledek jako `LEFT JOIN` na serveru.</span><span class="sxs-lookup"><span data-stu-id="53b89-167">A particular pattern in LINQ queries gives the same result as a `LEFT JOIN` on the server.</span></span> <span data-ttu-id="53b89-168">EF Core identifikuje takové vzory a na straně serveru vygeneruje ekvivalentní `LEFT JOIN`.</span><span class="sxs-lookup"><span data-stu-id="53b89-168">EF Core identifies such patterns and generates the equivalent `LEFT JOIN` on the server side.</span></span> <span data-ttu-id="53b89-169">Tento vzor zahrnuje vytvoření GroupJoin mezi zdroji dat a následné sloučení skupin pomocí operátoru operátor SelectMany s DefaultIfEmpty ve zdroji seskupení, aby odpovídal hodnotě null, pokud vnitřní nemá související prvek.</span><span class="sxs-lookup"><span data-stu-id="53b89-169">The pattern involves creating a GroupJoin between both the data sources and then flattening out the grouping by using the SelectMany operator with DefaultIfEmpty on the grouping source to match null when the inner doesn't have a related element.</span></span> <span data-ttu-id="53b89-170">Následující příklad ukazuje, jak tento vzor vypadá a co generuje.</span><span class="sxs-lookup"><span data-stu-id="53b89-170">The following example shows what that pattern looks like and what it generates.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#LeftJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

<span data-ttu-id="53b89-171">Výše uvedený vzor vytvoří komplexní strukturu ve stromu výrazu.</span><span class="sxs-lookup"><span data-stu-id="53b89-171">The above pattern creates a complex structure in the expression tree.</span></span> <span data-ttu-id="53b89-172">Z tohoto důvodu EF Core vyžaduje, abyste v kroku hned za operátorem naseskupili výsledky seskupení GroupJoin operátoru.</span><span class="sxs-lookup"><span data-stu-id="53b89-172">Because of that, EF Core requires you to flatten out the grouping results of the GroupJoin operator in a step immediately following the operator.</span></span> <span data-ttu-id="53b89-173">I v případě, že se používá GroupJoin-DefaultIfEmpty-operátor SelectMany, ale v jiném vzoru, nemůžeme ho identifikovat jako levou spojení.</span><span class="sxs-lookup"><span data-stu-id="53b89-173">Even if the GroupJoin-DefaultIfEmpty-SelectMany is used but in a different pattern, we may not identify it as a Left Join.</span></span>
