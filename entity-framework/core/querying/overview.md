---
title: "Jak dotazuje pracovní – základní EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
ms.technology: entity-framework-core
uid: core/querying/overview
ms.openlocfilehash: 7fd2940d559f82016d7a8fc3fdcf3af0d5b8bc8f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="how-queries-work"></a>Jak fungují dotazy

Entity Framework Core jazykové integrují dotazu (LINQ) používá k dotazování na data z databáze. LINQ umožňuje používat C# (nebo vámi zvolený jazyk rozhraní .NET) pro zápis silného typu dotazů založených na odvozené třídy kontextu a entity.

## <a name="the-life-of-a-query"></a>Dobu životnosti dotazu

Zde je podrobný přehled procesu, který prochází každý dotaz.

1. Dotaz LINQ zpracovává Entity Framework Core vytvořit reprezentaci, který je připravena k provedení zprostředkovatelem databáze
   1. Výsledek se uloží do mezipaměti, aby toto zpracování není potřeba provést pokaždé, když je proveden dotaz
2. Výsledek je předán k poskytovateli databáze
   1. Zprostředkovatel databáze identifikuje části dotazů, které lze vyhodnotit v databázi
   2. Tyto části dotazu jsou převedeny na konkrétní dotazovacího jazyka pro databázi (např. SQL pro relační databázi)
   3. Jeden nebo více dotazy se odesílají do databáze a sada výsledků vrácená (výsledky jsou hodnoty z databáze, není instancí entit)
3. Pro každou položku v sadě výsledků dotazu
   1. Pokud je dotaz sledování, EF kontroluje, pokud data reprezentuje entitu již v nástroji Sledování změn pro instanci kontextu
      * Pokud ano, vrátí se stávající entity
      * Pokud ne, se vytvoří nové entity, sledování změn je instalační program a vrátí novou entitu
   2. Pokud je dotaz ne sledování, EF kontroluje, pokud data reprezentuje entitu již v sadě pro tento dotaz výsledků
      * Pokud ano, je vrácen stávající entity <sup>(1)</sup>
      * Pokud ne, se vytvoří a vrátí novou entitu

<sup>(1) </sup> Žádné dotazy sledování slabé odkazy použít ke sledování entit, které již byly vráceny. Pokud předchozí výsledek se stejnou identitou ocitne mimo obor, a uvolňování paměti běží, zobrazí se nové instance entity.

## <a name="when-queries-are-executed"></a>Při provádění dotazů

Při volání LINQ operátory jednoduše vytváříte až reprezentaci v paměti dotazu. Dotaz je pouze odeslal do databáze, pokud je výsledek zpracován.

Nejběžnější operace, jejichž výsledkem dotazu odesílány do databáze jsou:
* Iterace výsledky v `for` smyčky
* Pomocí operátor, jako třeba `ToList`, `ToArray`, `Single`,`Count`
* Datové vazby výsledků dotazu do uživatelského rozhraní

> [!WARNING]  
> **Vždy ověření vstupu uživatele:** při EF poskytuje ochranu před útoky vkládání SQL, neprovádí žádné obecné ověření vstupu. Proto pokud hodnoty, které jsou předávány do rozhraní API, používaná v dotazech LINQ přiřazené vlastnosti entity, atd., pocházejí z nedůvěryhodného zdroje pak příslušné ověření podle požadavků na aplikaci, je třeba provést. To zahrnuje vstupu uživatele umožňuje dynamicky vytvářet dotazy. I když se používá LINQ, pokud je přijímáte zadání uživatele ve výrazy, které potřebujete, abyste měli jistotu, než jenom zamýšlený výrazy se vytvářejí lze sestavit.
