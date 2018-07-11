---
title: Odpojené entity – EF Core
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
ms.technology: entity-framework-core
uid: core/saving/disconnected-entities
ms.openlocfilehash: a81b0a26fe98dcc1ddedc11aba2673338c8991e8
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37948981"
---
# <a name="disconnected-entities"></a>Odpojené entity

DbContext instance bude automaticky sledovat entity, které vrátil z databáze. Změny provedené v těchto entit bude detekován pak, když je volána metoda SaveChanges a aktualizuje databázi podle potřeby. Zobrazit [základní Uložit](basic.md) a [souvisejících dat](related-data.md) podrobnosti.

Ale někdy entity jsou dotazovat pomocí jedné instance kontextu a uložit pomocí jiné instance. Této situaci často dochází v "odpojené" scénářů, jako je webová aplikace, kde entity, které jsou dotazovat, může odeslat klientovi, upravit, odeslat zpět na server v požadavku a pak uloží. V takovém případě kontextu druhé instance musí zjistit, zda jsou nové entity (by měl být vložen) nebo stávající (třeba aktualizovat).

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) na Githubu.

> [!TIP]
> EF Core můžete sledovat pouze jeden výskyt jakákoli entita s danou hodnotu primárního klíče. Nejlepší způsob, jak tento vyhnete, problém, je použít krátkodobou kontext pro každou jednotku pracovní, tak, že kontext začíná prázdný, obsahuje entit připojený, uloží tyto entity a kontext je uvolněn a zahodí.

## <a name="identifying-new-entities"></a>Určení nové entity

### <a name="client-identifies-new-entities"></a>Klient identifikuje nové entity

Nejjednodušším případě řešit je, když klient informuje server, zda je nový nebo existující entity. Například často požadavek na vložení nové entity se liší od požadavek na aktualizaci existující entity.

Zbytek tohoto oddílu popisuje případy kde je třeba určit jiným způsobem, jestli se má vložit nebo aktualizovat.

### <a name="with-auto-generated-keys"></a>Pomocí automaticky generovaného klíče

Hodnota automaticky generovaného klíče lze často určit, zda entita musí přidají nebo aktualizují. Pokud klíč není nastavený (to znamená, stále má CLR výchozí hodnotu null, nula, atd.), pak musí být nové entity a potřebuje vkládání. Na druhé straně Pokud byla nastavena hodnota klíče, potom musí mít už dříve uložil a teď se musí aktualizovat. Jinými slovy Pokud klíč má hodnotu a pak entity byla dotazována může odeslat klientovi a má teď vrátit aktualizovat.

Je snadné vyhledávat nenastavené klíč, pokud známý typ entity:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewSimple)]

EF má však také integrované způsob, jak to provést u každého typu entity a typ klíče:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> Jsou nastavené klíče jako entity jsou sledovány v kontextech, i v případě, že entita je ve stavu Added. To pomáhá při procházení grafu entit a rozhodování o tom, co dělat s každou, například při použití rozhraní API TrackGraph. Hodnota klíče byste měli použít pouze ve tak, jak je znázorněno zde _před_ jakékoli volání ke sledování entity.

### <a name="with-other-keys"></a>Pomocí dalších klíčů

Některé mechanismus je potřeba k identifikaci nových entit nejsou automaticky generované hodnoty klíče. Existují dva hlavní přístupy k tomuto:
 * Dotaz pro entitu
 * Předání příznaku z klienta

Dotaz pro entitu, použijte metodu Find:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewQuery)]

Jde nad rámec tohoto dokumentu chcete zobrazit celý kód pro předání příznaku z klienta. Ve webové aplikaci obvykle to znamená různých požadavků pro různé akce, nebo předávání některých stavu v požadavku a extrahování v kontroleru.

## <a name="saving-single-entities"></a>Uložení jednotlivých entit

Pokud se označuje, zda insert nebo update je potřeba, pak přidat nebo aktualizovat dá správně:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

Ale pokud entita používá automaticky generovaný hodnoty klíče, pak metody Update slouží pro oba případy:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

Metoda Update obvykle označuje entity pro aktualizaci, nelze vložit. Nicméně pokud entita obsahuje automaticky generovaný klíč a byla nastavena žádná hodnota klíče, pak entity se místo toho automaticky označen pro vložení.

> [!TIP]  
> Toto chování byla zavedená v EF Core 2.0. Pro starší verze je vždy nutné explicitně zvolte Přidat nebo aktualizovat.

Pokud subjektem není pomocí automaticky generovaného klíče, pak aplikace musíte rozhodnout, jestli mají vložit nebo aktualizovat entity: Příklad:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

Jsou zde uvedené kroky:
* Přidat hledání vrátí hodnotu null, pak databáze již neobsahuje blog s tímto ID, takže označujeme je jako označte ji pro vložení.
* Vrátí-li najít entitu, pak existuje v databázi a kontext nyní sleduje existující entity
  * Potom použijeme SetValues k nastavení hodnot pro všechny vlastnosti v této entitě na ty, které pocházejí z klienta.
  * Volání SetValues označí entita, která má být aktualizována podle potřeby.

> [!TIP]  
> SetValues pouze označíte jako změny vlastností, které mají různé hodnoty na hodnoty v sledované entity. To znamená, že při odeslání aktualizace, budou aktualizovány pouze sloupce, které se ve skutečnosti změnily. (A pokud se nic nezměnilo, pak žádná aktualizace se budou odesílat vůbec.)

## <a name="working-with-graphs"></a>Práce s grafy

### <a name="identity-resolution"></a>Rozlišení identity

Jak bylo uvedeno výše, EF Core můžete sledovat pouze jeden výskyt jakákoli entita s danou hodnotu primárního klíče. Při práci s grafy by měl vytvoří graf v ideálním případě tak, že tento invariantní zachovaný a kontextu má být použita pro pouze jednu jednotku pracovní. Pokud graf obsahuje duplicitní hodnoty, pak bude potřeba zpracovat grafu před odesláním do EF provést konsolidaci více instancí do jedné. Toto video asi triviální kde instance mají konfliktní hodnoty a vztahy, tak konsolidace duplicitní položky by mělo být provedeno co nejdříve v kanálu aplikace, aby řešení konfliktů.

### <a name="all-newall-existing-entities"></a>Všechny nové/všechny existující entity

Příklad práci s grafy je vkládání nebo aktualizaci blogu společně s jeho kolekce přidružené příspěvky. Pokud by měl být vložen všechny entity v grafu nebo by měly být aktualizovány všechny, pak proces je stejné jako nastavení popsané výše pro jednotlivé entity. Například graf blogů a příspěvky vytvořené tímto způsobem:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

můžete vložit tímto způsobem:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertGraph)]

Volání přidat označí blogu a všechny příspěvky, které má být vložen.

Podobně pokud potřebujete aktualizovat všechny entity v grafu, pak aktualizací je možné:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#UpdateGraph)]

Aktualizovat budou označeny v blogu a všech jeho příspěvků.

### <a name="mix-of-new-and-existing-entities"></a>Kombinace nová a existující entity

Pomocí automaticky generovaného klíče můžete aktualizace znovu použijí pro vkládání a aktualizace, i v případě, že graf obsahuje entity, které vyžadují vkládání i těch, které vyžadují aktualizaci:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

Aktualizace označí entitu v grafu, blogu nebo příspěvek pro vložení, pokud nemá sadu klíč-hodnota, zatímco jiné entity jsou označeny pro aktualizaci.

Jako dříve, kdy se nepoužívá automaticky generovaného klíče dotazu a nějaké zpracování je možné:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a>Zpracování odstranění

Odstranění může být velmi obtížné zpracovat od často neexistence entity znamená, že je nutné ji odstranit. Jeden způsob, jak to vyřešit, je použití "obnovitelné odstranění" tak, aby entita je označen jako odstraněný místo ve skutečnosti odstraňuje. Odstraní se pak stane stejný jako aktualizace. Obnovitelné odstranění je možné implementovat pomocí [dotazování filtry](xref:core/querying/filters).

Pro true odstraní běžným postupem je použití rozšíření vzorku dotazu provádět, co je v podstatě rozdíl grafu Příklad:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a>TrackGraph

Interně, přidat, připojit a aktualizace pomocí procházení graf rozhodnutí provedli pro každou entitu v tom, jestli ho musí být označené jako přidanou (Chcete-li vložit), Modified (Chcete-li aktualizovat), Unchanged (Neprovádět žádnou akci), nebo odstraněné (Chcete-li odstranit). Tento mechanismus je zveřejněný prostřednictvím rozhraní API TrackGraph. Například předpokládejme, že když klient odešle zpět graf entit nastaví některá příznak u každé entity označující, jak ji by měl být zpracována. TrackGraph je pak možné zpracovat tento příznak:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#TrackGraph)]

Příznaky se zobrazují pouze jako součást entity pro zjednodušení tento příklad. Obvykle příznaků by být součástí objekt DTO nebo jiný stav zahrnutý v požadavku.
