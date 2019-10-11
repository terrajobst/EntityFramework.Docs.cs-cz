---
title: Jak fungují dotazy – EF Core
author: rowanmiller
ms.date: 09/26/2018
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
uid: core/querying/how-query-works
ms.openlocfilehash: bc085755f39b1288f092a8b2df892c1bf82a89f1
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72186265"
---
# <a name="how-queries-work"></a>Jak fungují dotazy

Entity Framework Core používá k dotazování dat z databáze jazykově integrovaný dotaz (LINQ). LINQ umožňuje použít C# (nebo vlastní jazyk .NET) k zápisu silně typových dotazů na základě odvozeného kontextu a tříd entit.

## <a name="the-life-of-a-query"></a>Životnost dotazu

Následuje přehled procesu, pomocí kterého každý dotaz projde.

1. Dotaz LINQ je zpracován nástrojem Entity Framework Core k sestavení reprezentace, která je připravena ke zpracování zprostředkovatelem databáze.
   1. Výsledkem je ukládání do mezipaměti, aby se toto zpracování nemuselo provádět pokaždé, když se dotaz spustí.
2. Výsledek je předán poskytovateli databáze.
   1. Poskytovatel databáze určuje, které části dotazu je možné vyhodnotit v databázi.
   2. Tyto části dotazu jsou přeloženy do dotazovacího jazyka specifického pro databázi (například SQL pro relační databázi).
   3. Jeden nebo více dotazů je odesláno do databáze a vrácená sada výsledků (výsledky jsou hodnoty z databáze, nikoli instance entity)
3. Pro každou položku v sadě výsledků dotazu
   1. Pokud se jedná o sledovací dotaz, EF zkontroluje, jestli data představují entitu, která už je v sledování změn pro instanci kontextu.
      * Pokud ano, bude vrácena existující entita.
      * Pokud ne, vytvoří se nová entita, bude nastaveno sledování změn a vrátí se nová entita.
   2. Pokud se jedná o dotaz bez sledování, EF zkontroluje, jestli data představují entitu, která už je v sadě výsledků dotazu pro tento dotaz.
      * Pokud ano, bude vrácena existující entita <sup>(1)</sup> .
      * V takovém případě se vytvoří nová entita, která se vrátí.

<sup>(1)</sup> žádné sledovací dotazy pomocí slabých odkazů udržují přehled o entitách, které už byly vráceny. Pokud předchozí výsledek se stejnou identitou přejde mimo rozsah a spustí se uvolňování paměti, můžete získat novou instanci entity.

## <a name="when-queries-are-executed"></a>Při spuštění dotazů

Při volání operátorů LINQ stačí sestavit reprezentace dotazu v paměti. Dotaz je odeslán do databáze pouze v případě, že jsou výsledky spotřebovány.

Nejběžnější operace, které mají za následek odeslání dotazu do databáze, jsou tyto:
* Iterace výsledků ve smyčce `for`
* Použití operátoru, například `ToList`, `ToArray` `Single`, `Count`
* Vazba výsledků dotazu na uživatelské rozhraní

> [!WARNING]  
> **Vždy ověřit vstup uživatele:** I když EF Core chrání proti útokům prostřednictvím injektáže SQL pomocí parametrů a řídicích znaků v dotazech, neověřuje vstupy. Příslušné ověření podle požadavků aplikace by se mělo provést před tím, než se hodnoty z nedůvěryhodných zdrojů použijí v dotazech LINQ, přiřazené k vlastnostem entity nebo předávané jiným EF Core rozhraní API. To zahrnuje všechny vstupy uživatele používané k dynamickému vytváření dotazů. I při použití LINQ, Pokud přijímáte uživatelský vstup pro vytváření výrazů, je nutné zajistit, aby bylo možné sestavit pouze zamýšlené výrazy.
