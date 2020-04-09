---
title: Jak fungují dotazy – ef core
author: ajcvickers
ms.date: 03/17/2020
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
uid: core/querying/how-query-works
ms.openlocfilehash: e8a50efe31468ea8df211602636dd474550bc0ef
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "80136244"
---
# <a name="how-queries-work"></a>Jak dotazy fungují

Jádro entity framework používá jazykový integrovaný dotaz (LINQ) k dotazování dat z databáze. LINQ umožňuje používat C# (nebo váš jazyk .NET volby) k zápisu dotazů silného typu na základě odvozeného kontextu a tříd entity.

## <a name="the-life-of-a-query"></a>Životnost dotazu

Následuje přehled na vysoké úrovni procesu, kterým každý dotaz prochází.

1. Dotaz LINQ je zpracován jádrem entity frameworku k vytvoření reprezentace, která je připravena ke zpracování poskytovatelem databáze.
   1. Výsledek je uložen do mezipaměti, takže toto zpracování není nutné provádět při každém spuštění dotazu
2. Výsledek je předán poskytovateli databáze.
   1. Poskytovatel databáze identifikuje, které části dotazu lze vyhodnocovat v databázi.
   2. Tyto části dotazu jsou přeloženy do databázového dotazovacího jazyka (například SQL pro relační databázi).
   3. Dotaz je odeslán do databáze a vrácena sada výsledků (výsledky jsou hodnoty z databáze, nikoli instance entit)
3. Pro každou položku v sadě výsledků
   1. Pokud se jedná o dotaz sledování, EF zkontroluje, zda data představují entitu již v sledování změn pro instanci kontextu
      * Pokud ano, je stávající účetní jednotka vrácena
      * Pokud tomu tak není, je vytvořena nová entita, nastavení sledování změn a vrácena nová entita.
   2. Pokud se jedná o dotaz bez sledování, je vždy vytvořena a vrácena nová entita.

## <a name="when-queries-are-executed"></a>Při provádění dotazů

Při volání linq operátory, jsou jednoduše vytváření reprezentace v paměti dotazu. Dotaz je odeslán do databáze pouze v případě, že jsou spotřebovány výsledky.

Nejběžnější operace, které vedou k dotazu odesílaných do databáze jsou:

* Iterace výsledků ve `for` smyčce
* Použití operátoru, `ToList` `ToArray`například `Count` , , `Single`nebo ekvivalentního asynchronního přetížení

> [!WARNING]  
> **Vždy ověřte vstup uživatele:** Zatímco EF Core chrání před útoky injektáže SQL pomocí parametrů a escaping literály v dotazech, neověřuje vstupy. Vhodné ověření podle požadavků aplikace by mělo být provedeno před použitím hodnot z nedůvěryhodných zdrojů v dotazech LINQ, přiřazených k vlastnostem entity nebo předáno jiným klíčovým rozhraním API EF. To zahrnuje všechny vstupy uživatele slouží k dynamicky vytvářet dotazy. I při použití LINQ, pokud přijímáte vstup uživatele k vytváření výrazů, musíte se ujistit, že lze sestavit pouze zamýšlené výrazy.
