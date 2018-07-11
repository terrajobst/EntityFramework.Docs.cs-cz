---
title: Jak dotazuje Work – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
ms.technology: entity-framework-core
uid: core/querying/overview
ms.openlocfilehash: 1d28d215302625cf2b6788359527a93a77b7e9fd
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949386"
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
> **Vždy ověření vstupu uživatele:** při EF zajišťuje ochranu před útoky prostřednictvím injektáže SQL, není nutné žádné obecné ověření vstupu. Proto pokud hodnoty předávaný do rozhraní API, používat v dotazech LINQ, přiřazeno k vlastnosti entity, atd., pocházejí z nedůvěryhodných pak odpovídající ověření podle požadavků vaší aplikace, je třeba provést. To zahrnuje vstupu uživatele použít k vytvoření dotazů. I když se používá LINQ, pokud je přijímáte, že k sestavení výrazy, které potřebujete k Ujistěte se, že než pouze odpovídající výrazy lze sestavit vstup uživatele.
