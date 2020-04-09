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
# <a name="complex-query-operators"></a>Komplexní operátory dotazů

Jazyk integrovaný dotaz (LINQ) obsahuje mnoho komplexních operátorů, které kombinují více zdrojů dat nebo provádí komplexní zpracování. Ne všechny operátory LINQ mají vhodné překlady na straně serveru. Někdy se dotaz v jednom formuláři překládá na server, ale pokud je napsán v jiné podobě, nepřekládá, i když je výsledek stejný. Tato stránka popisuje některé komplexní operátory a jejich podporované varianty. V budoucích verzích můžeme rozpoznat další vzory a přidat jejich odpovídající překlady. Je také důležité mít na paměti, že podpora překladů se liší mezi poskytovateli. Konkrétní dotaz, který je přeložen v SqlServer, nemusí fungovat pro databáze SQLite.

> [!TIP]
> Ukázku tohoto článku [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) můžete zobrazit na GitHubu.

## <a name="join"></a>Spojit

Operátor LINQ Join umožňuje připojit dva zdroje dat na základě voliče klíčů pro každý zdroj a generovat řazenou n-tice hodnot při shod.s. To se přirozeně `INNER JOIN` překládá na relačních databázích. Zatímco linq spojení má vnější a vnitřní klíčové voliče, databáze vyžaduje podmínku jednoho spojení. Takže EF Core generuje podmínku spojení porovnáním voliče vnějšího klíče s voličem vnitřního klíče pro rovnost. Dále pokud selektory klíčů jsou anonymní typy, EF Core generuje podmínku spojení porovnat součást rovnosti moudrý.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#Join)]

```SQL
SELECT [p].[PersonId], [p].[Name], [p].[PhotoId], [p0].[PersonPhotoId], [p0].[Caption], [p0].[Photo]
FROM [PersonPhoto] AS [p0]
INNER JOIN [Person] AS [p] ON [p0].[PersonPhotoId] = [p].[PhotoId]
```

## <a name="groupjoin"></a>Skupinové spojení

Operátor LINQ GroupJoin umožňuje připojit dva zdroje dat podobné Join, ale vytvoří skupinu vnitřních hodnot pro odpovídající vnější prvky. Spuštění dotazu, jako je následující příklad `Blog`  &  `IEnumerable<Post>`generuje výsledek . Vzhledem k tomu, že databáze (zejména relační databáze) nemají způsob, jak reprezentovat kolekci objektů na straně klienta, GroupJoin nepřekládá na server v mnoha případech. Vyžaduje, abyste získat všechna data ze serveru k tomu GroupJoin bez zvláštního voliče (první dotaz níže). Ale pokud selektor omezuje data jsou vybrána pak načítání všech dat ze serveru může způsobit problémy s výkonem (druhý dotaz níže). To je důvod, proč EF Core nepřekládá GroupJoin.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoin)]

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoinComposed)]

## <a name="selectmany"></a>Selectmany

Linq SelectMany Operátor umožňuje výčet přes výběr kolekce pro každý vnější prvek a generovat řazené kolekce hodnot z každého zdroje dat. Svým způsobem je spojení, ale bez jakékoli podmínky, takže každý vnější prvek je spojen s elementem ze zdroje kolekce. V závislosti na tom, jak se volič kolekce vztahuje k vnějšímu zdroji dat SelectMany lze přeložit do různých dotazů na straně serveru.

### <a name="collection-selector-doesnt-reference-outer"></a>Volič kolekce neodkazuje na vnější

Pokud volič kolekce neodkazuje na nic z vnějšího zdroje, výsledkem je kartézský produkt obou zdrojů dat. To se `CROSS JOIN` překládá do relačních databází.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToCrossJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
CROSS JOIN [Posts] AS [p]
```

### <a name="collection-selector-references-outer-in-a-where-clause"></a>Výběr kolekce odkazuje na vnější v where klauzule

Pokud má volič kolekce klauzuli where, která odkazuje na vnější prvek, pak EF Core převede na spojení databáze a použije predikát jako podmínku spojení. Obvykle tento případ nastane při použití kolekce navigace na vnější prvek jako volič kolekce. Pokud kolekce je prázdný pro vnější prvek, pak žádné výsledky by být generovány pro tento vnější prvek. Ale `DefaultIfEmpty` pokud je použita na voliče kolekce pak vnější prvek bude spojen s výchozí hodnotou vnitřní prvek. Z důvodu tohoto rozlišení tento druh dotazů překládá `INNER JOIN` v nepřítomnosti `DefaultIfEmpty` a `LEFT JOIN` při `DefaultIfEmpty` použití.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
INNER JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

### <a name="collection-selector-references-outer-in-a-non-where-case"></a>Volič kolekce odkazuje na vnější v případě, že

Když selekátor kolekce odkazuje na vnější prvek, který není v where klauzule (jako případ výše), nepřekládá do spojení databáze. To je důvod, proč musíme vyhodnotit výběr kolekce pro každý vnější prvek. Překládá se `APPLY` na operace v mnoha relačních databázích. Pokud kolekce je prázdný pro vnější prvek, pak žádné výsledky by být generovány pro tento vnější prvek. Ale `DefaultIfEmpty` pokud je použita na voliče kolekce pak vnější prvek bude spojen s výchozí hodnotou vnitřní prvek. Z důvodu tohoto rozlišení tento druh dotazů překládá `CROSS APPLY` v nepřítomnosti `DefaultIfEmpty` a `OUTER APPLY` při `DefaultIfEmpty` použití. Některé databáze jako SQLite nepodporují `APPLY` operátory, takže tento druh dotazu nemusí být přeložen.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToApply)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
CROSS APPLY [Posts] AS [p]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
OUTER APPLY [Posts] AS [p]
```

## <a name="groupby"></a>GroupBy

LINQ GroupBy operátory vytvořit `IGrouping<TKey, TElement>` `TKey` výsledek `TElement` typu, kde a může být libovolný typ. Kromě toho `IGrouping` implementuje `IEnumerable<TElement>`, což znamená, že můžete sestavit přes něj pomocí libovolného operátoru LINQ po seskupení. Vzhledem k `IGrouping`tomu, že žádná struktura databáze může představovat , GroupBy operátory nemají žádný překlad ve většině případů. Při agregační operátor je použita pro každou skupinu, která `GROUP BY` vrátí skalární, může být přeložen do SQL v relačních databázích. SQL `GROUP BY` je také omezující. Vyžaduje, abyste seskupit pouze skalární hodnoty. Projekce může obsahovat pouze seskupení klíčových sloupců nebo všechny agregáty aplikované přes sloupec. EF Core identifikuje tento vzor a překládá jej na server, jako v následujícím příkladu:

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupBy)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
```

EF Core také překládá dotazy, kde agregační operátor na seskupení se zobrazí v Kde nebo OrderBy (nebo jiné řazení) LINQ operátor. Používá `HAVING` klauzuli v SQL pro where klauzule. Část dotazu před použitím GroupBy operátor může být libovolný složitý dotaz tak dlouho, dokud jej lze přeložit na server. Kromě toho po použití agregační operátory na dotaz seskupení odebrat seskupení z výsledného zdroje, můžete vytvořit nad ním jako jakýkoli jiný dotaz.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupByFilter)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
HAVING COUNT(*) > 0
ORDER BY [p].[AuthorId]
```

Agregované operátory EF Core podporuje jsou následující

- Průměr
- Počet
- Dlouhý počet
- Maximum
- Minimum
- Součet

## <a name="left-join"></a>Levé spojení

Zatímco left join není operátor LINQ, relační databáze mají koncept levého spojení, který se často používá v dotazech. Konkrétní vzor v linq dotazy poskytuje stejný `LEFT JOIN` výsledek jako na serveru. EF Core identifikuje tyto vzory a `LEFT JOIN` generuje ekvivalent na straně serveru. Vzor zahrnuje vytvoření GroupJoin mezi oba zdroje dat a potom sloučení seskupení pomocí SelectMany operátor s DefaultIfEmpty na zdroj seskupení tak, aby odpovídaly null, když vnitřní nemá související prvek. Následující příklad ukazuje, jak tento vzor vypadá a co generuje.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#LeftJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

Výše uvedený vzorek vytvoří komplexní strukturu ve stromu výrazů. Z tohoto důvodu EF Core vyžaduje, abyste vyrovnat výsledky seskupení GroupJoin operátor v kroku bezprostředně po operátor. I v případě, že GroupJoin-DefaultIfEmpty-SelectMany se používá, ale v jiném vzoru, nemusíme identifikovat jako levé spojení.
