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
# <a name="complex-query-operators"></a>Složité operátory dotazů

Jazykově integrovaný dotaz (LINQ) obsahuje mnoho složitých operátorů, které kombinují více zdrojů dat nebo provádí komplexní zpracování. Ne všechny operátory LINQ mají vhodné překlady na straně serveru. V některých případech se dotaz v jednom formuláři přeloží na server, ale pokud je výsledek stejný, nepřeloží se i v případě, že je napsaný v jiném formuláři. Tato stránka popisuje některé ze složitých operátorů a jejich podporované variace. V budoucích verzích můžeme rozpoznat více vzorů a přidat jejich odpovídající překlady. Je také důležité vzít v úvahu, že podpora překladu mezi poskytovateli se liší. Konkrétní dotaz, který je přeložen v SqlServer, nemusí fungovat pro databáze SQLite.

> [!TIP]
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) na Githubu.

## <a name="join"></a>Spojit

Operátor LINQ JOIN umožňuje propojit dva zdroje dat na základě výběru klíče pro každý zdroj a vygenerovat řazenou kolekci hodnot, pokud se klíč shoduje. Přirozeně se překládá na `INNER JOIN` v relačních databázích. I když má spojení LINQ k vnějšímu a základnímu selektorům klíčů, databáze vyžaduje jednu podmínku připojení. Takže EF Core vygeneruje podmínku spojení porovnáním vnějšího selektoru klíče s selektorm vnitřních klíčů pro rovnost. Pokud jsou například selektory klíče anonymní typy, EF Core vygeneruje podmínku JOIN pro porovnání vhodné součásti rovnosti.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#Join)]

```SQL
SELECT [p].[PersonId], [p].[Name], [p].[PhotoId], [p0].[PersonPhotoId], [p0].[Caption], [p0].[Photo]
FROM [PersonPhoto] AS [p0]
INNER JOIN [Person] AS [p] ON [p0].[PersonPhotoId] = [p].[PhotoId]
```

## <a name="groupjoin"></a>GroupJoin

Operátor LINQ GroupJoin vám umožňuje propojit dva zdroje dat, podobně jako Join, ale vytvoří skupinu vnitřních hodnot pro porovnání vnějších prvků. Provedení dotazu, jako je následující příklad, generuje výsledek `Blog` @ no__t-1 @ no__t-2. Vzhledem k tomu, že databáze (zejména relační databáze) nemají způsob reprezentace kolekce objektů na straně klienta, GroupJoin nepřevádí na server v mnoha případech. Vyžaduje, abyste získali všechna data ze serveru tak, aby se GroupJoin bez speciálního selektoru (první dotaz níže). Pokud však selektor omezuje vybraná data, může dojít k problémům s výkonem (druhý dotaz níže) a načtení všech dat ze serveru. To je důvod, proč EF Core nepřekládat GroupJoin.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoin)]

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoinComposed)]

## <a name="selectmany"></a>Operátor SelectMany

Operátor LINQ operátor SelectMany umožňuje vytvořit výčet v selektoru kolekce pro každý vnější prvek a generovat řazené kolekce členů hodnot z každého zdroje dat. Způsobem je to spojení, ale bez jakékoliv podmínky, takže každý vnější prvek je připojen pomocí elementu ze zdroje kolekce. V závislosti na tom, jak je selektor kolekce v relaci s vnějším zdrojem dat, může operátor SelectMany překládat do různých různých dotazů na straně serveru.

### <a name="collection-selector-doesnt-reference-outer"></a>Selektor kolekce neodkazuje na vnější

Když selektor kolekce neodkazuje na cokoli od vnějšího zdroje, výsledkem je kartézském produkt obou zdrojů dat. Překládá se na `CROSS JOIN` v relačních databázích.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToCrossJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
CROSS JOIN [Posts] AS [p]
```

### <a name="collection-selector-references-outer-in-a-where-clause"></a>Selektor kolekce odkazuje vnější v klauzuli WHERE.

Když selektor kolekce obsahuje klauzuli WHERE, která odkazuje na vnější prvek, pak EF Core přeloží na připojení k databázi a použije predikát jako podmínku JOIN. Tento případ se obvykle vyskytne při použití navigace kolekce na vnějším prvku jako selektor kolekce. Pokud je kolekce prázdná pro vnější prvek, nebudou pro tento vnější prvek generovány žádné výsledky. Pokud je však v selektoru kolekcí použito `DefaultIfEmpty`, vnější prvek bude propojen s výchozí hodnotou vnitřního prvku. Z tohoto důvodu tento druh dotazů překládá `INNER JOIN` při nepřítomnosti `DefaultIfEmpty` a `LEFT JOIN` při použití `DefaultIfEmpty`.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
INNER JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

### <a name="collection-selector-references-outer-in-a-non-where-case"></a>Selektor kolekce odkazuje na vnější v případě, že ne.

Když selektor kolekce odkazuje na vnější prvek, který není v klauzuli WHERE (v případě výše uvedeného případu), nepřeloží se na připojení k databázi. To je důvod, proč musíme vyhodnotit selektor kolekce pro každý vnější element. Překládá se na operace `APPLY` v mnoha relačních databázích. Pokud je kolekce prázdná pro vnější prvek, nebudou pro tento vnější prvek generovány žádné výsledky. Pokud je však v selektoru kolekcí použito `DefaultIfEmpty`, vnější prvek bude propojen s výchozí hodnotou vnitřního prvku. Z tohoto důvodu tento druh dotazů překládá `CROSS APPLY` při nepřítomnosti `DefaultIfEmpty` a `OUTER APPLY` při použití `DefaultIfEmpty`. Některé databáze, jako je SQLite, nepodporují operátory `APPLY`, takže tento typ dotazu není možné přeložit.

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

Operátory LINQ GroupBy vytvoří výsledek typu `IGrouping<TKey, TElement>`, kde `TKey` a `TElement` by mohly být libovolného typu. Kromě toho `IGrouping` implementuje `IEnumerable<TElement>`, což znamená, že ho můžete po seskupení použít pomocí libovolného operátoru LINQ. Vzhledem k tomu, že žádná struktura databáze nemůže představovat `IGrouping`, operátory GroupBy nemají ve většině případů žádný převod. Při použití agregačního operátoru na každou skupinu, která vrací skalární hodnotu, lze ji přeložit na SQL `GROUP BY` v relačních databázích. @No__t SQL-0 je omezující. Vyžaduje, abyste je mohli seskupit pouze pomocí skalárních hodnot. Projekce může obsahovat pouze sloupce klíče seskupení nebo agregace použité na sloupec. EF Core tento model identifikuje a překládá ho na server, jak je uvedeno v následujícím příkladu:

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupBy)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
```

EF Core také přeloží dotazy, kde se agregační operátor ve skupině zobrazuje v operátoru LINQ nebo OrderBy (nebo jiném řazení). Používá klauzuli `HAVING` v SQL pro klauzuli WHERE. Část dotazu před použitím operátoru GroupBy může být jakýkoli složitý dotaz, pokud jej lze přeložit na server. Kromě toho, když použijete agregační operátory pro dotaz seskupení a odeberete seskupení ze výsledného zdroje, můžete vytvořit jejich sestavování jako jakýkoliv jiný dotaz.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupByFilter)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
HAVING COUNT(*) > 0
ORDER BY [p].[AuthorId]
```

Agregační operátory EF Core podporovány jsou následující.

- Average
- Count
- LongCount
- Maximum
- Minimum
- Součet

## <a name="left-join"></a>Vlevo připojit

Zatímco levé spojení není operátor LINQ, relační databáze mají koncept levého spojení, který se často používá v dotazech. Konkrétní vzor v dotazech LINQ poskytuje stejný výsledek jako `LEFT JOIN` na serveru. EF Core identifikuje takové vzory a na straně serveru vygeneruje ekvivalentní `LEFT JOIN`. Tento vzor zahrnuje vytvoření GroupJoin mezi zdroji dat a následné sloučení skupin pomocí operátoru operátor SelectMany s DefaultIfEmpty ve zdroji seskupení, aby odpovídal hodnotě null, pokud vnitřní nemá související prvek. Následující příklad ukazuje, jak tento vzor vypadá a co generuje.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#LeftJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

Výše uvedený vzor vytvoří komplexní strukturu ve stromu výrazu. Z tohoto důvodu EF Core vyžaduje, abyste v kroku hned za operátorem naseskupili výsledky seskupení GroupJoin operátoru. I v případě, že se používá GroupJoin-DefaultIfEmpty-operátor SelectMany, ale v jiném vzoru, nemůžeme ho identifikovat jako levou spojení.
