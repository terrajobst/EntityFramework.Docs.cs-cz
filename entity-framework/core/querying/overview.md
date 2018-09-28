---
title: Jak dotazuje Work – EF Core
author: rowanmiller
ms.date: 09/26/2018
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
uid: core/querying/overview
ms.openlocfilehash: 23d26f9c0ac17fc0df744f5339946947ea366911
ms.sourcegitcommit: 15022dd06d919c29b1189c82611ea32f9fdc6617
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/27/2018
ms.locfileid: "47415728"
---
# <a name="how-queries-work"></a>Jak fungují dotazy

Entity Framework Core Language Integrated Query (LINQ) používá k dotazování na data z databáze. LINQ umožňuje používat C# (nebo svůj jazyk .NET) pro zápis silného typu dotazů založených na odvozené třídy kontextu a entity.

## <a name="the-life-of-a-query"></a>Běžný dotaz

Tady je obecný přehled procesu, který prochází každý dotaz.

1. Entity Framework Core k sestavení, která jsou připravená pro zpracování poskytovatele databáze reprezentaci zpracovává dotazu LINQ
   1. Výsledek je uložen do mezipaměti tak, aby toto zpracování není nutné provést při každém spuštění dotazu
2. Výsledek je předán k poskytovateli databáze
   1. Poskytovatel databáze identifikuje části dotazů, které mohou být vyhodnoceny v databázi
   2. Tyto části dotazu jsou přeloženy do databáze konkrétní dotazovacího jazyka (například SQL pro relační databáze)
   3. Jeden nebo více dotazy se odesílají do databáze a sada výsledků vrácená (výsledky jsou hodnoty z databáze není instancí entit)
3. Pro každou položku v sadě výsledků
   1. Pokud je to dotazu sledování, EF kontroluje, pokud data představuje entitu již v nástroji Sledování změn pro instance kontextu
      * Pokud ano, vrátí se existující entity
      * V opačném případě se vytvoří nové entity, sledování změn je nastavená a vrátí novou entitu
   2. Pokud je to dotazu bez sledování, EF kontroluje, pokud data představuje entitu již v sadě pro tento dotaz výsledků
      * Pokud ano, je vrácena existující entity <sup>(1)</sup>
      * V opačném případě se vytvoří a vrátí novou entitu

<sup>(1) </sup> Žádné dotazy sledování můžete sledovat, entity, které již byly vráceny slabé odkazy. Pokud předchozí výsledek se stejnou identitou dostane mimo rozsah a uvolňování paměti běží, může se zobrazit nová instance entity.

## <a name="when-queries-are-executed"></a>Při provádění dotazů

Při volání operátory LINQ, jednoduše vytváříte nahoru v paměti reprezentace dotazu. Dotaz je odeslán do databáze, pouze při výsledky se spotřebuje.

Nejběžnější operace, jejichž výsledkem dotazů odesílaných do databáze jsou:
* Výsledky v iteraci `for` smyčky
* Například použití operátoru `ToList`, `ToArray`, `Single`, `Count`
* Vázání dat výsledků dotazu do uživatelského rozhraní

> [!WARNING]  
> **Vždy ověření vstupu uživatele:** při EF Core chrání před útoky prostřednictvím injektáže SQL s použitím parametrů a uvození literály v dotazech, neověřuje při vstupy. Odpovídající ověření podle požadavků vaší aplikace, je třeba provést před použít v dotazech LINQ, přiřazená vlastností entity nebo předán pro jiná rozhraní API EF Core hodnoty z nedůvěryhodných zdrojů. To zahrnuje vstupu uživatele použít k vytvoření dotazů. I když se používá LINQ, pokud jsou přijímat uživatelský vstup k sestavení výrazy, budete muset Ujistěte se, že lze sestavit pouze odpovídající výrazy.
