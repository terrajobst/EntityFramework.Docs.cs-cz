---
title: Odpojené entity – jádro EF
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
uid: core/saving/disconnected-entities
ms.openlocfilehash: 421531e68ac98c0553938f1c24892701f22fef3c
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417595"
---
# <a name="disconnected-entities"></a>Odpojené entity

Instance DbContext bude automaticky sledovat entity vrácené z databáze. Změny provedené v těchto entitách pak budou zjištěny při SaveChanges je volána a databáze bude aktualizován podle potřeby. Podrobnosti [najdete v tématu Základní ukládání](basic.md) [a související data.](related-data.md)

Někdy jsou však entity dotazovány pomocí jedné instance kontextu a poté uloženy pomocí jiné instance. To se často děje v "odpojené" scénáře, jako je například webové aplikace, kde jsou subjekty dotazován, odeslány klientovi, změněn, odeslány zpět na server v požadavku a potom uloženy. V takovém případě druhá kontextová instance potřebuje vědět, zda jsou entity nové (měly by být vloženy) nebo existující (by měly být aktualizovány).

<!-- markdownlint-disable MD028 -->
> [!TIP]
> Ukázku tohoto článku [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Disconnected/) můžete zobrazit na GitHubu.

> [!TIP]
> EF Core může sledovat pouze jednu instanci libovolné entity s danou hodnotou primárního klíče. Nejlepší způsob, jak se tomuvyhnout problému je použít kontext s krátkou životností pro každou jednotku práce tak, aby kontext začíná prázdný, má entity k němu připojené, uloží tyto entity a pak je kontext uvolněn a zahozen.
<!-- markdownlint-enable MD028 -->

## <a name="identifying-new-entities"></a>Identifikace nových entit

### <a name="client-identifies-new-entities"></a>Klient identifikuje nové entity

Nejjednodušší případ, který je třeba řešit, je, když klient informuje server, zda je entita nová nebo existující. Například často požadavek na vložení nové entity se liší od požadavku na aktualizaci existující entity.

Zbývající část této části se týká případů, kdy je nutné určit jiným způsobem, zda vložit nebo aktualizovat.

### <a name="with-auto-generated-keys"></a>S automaticky generovanými klávesami

Hodnotu automaticky generovaného klíče lze často použít k určení, zda je třeba entitu vložit nebo aktualizovat. Pokud klíč nebyl nastaven (to znamená, že má stále výchozí hodnotu CLR null, nula atd.), musí být entita nová a musí být potřeba vložit. Na druhou stranu, pokud byla nastavena hodnota klíče, musí již být dříve uložena a nyní potřebuje aktualizaci. Jinými slovy, pokud klíč má hodnotu, pak entita byla dotazována, odeslána klientovi a nyní se vrátil k aktualizaci.

Je snadné zkontrolovat odnastavený klíč, pokud je znám typ entity:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#IsItNewSimple)]

Ef má však také předdefinovaný způsob, jak to provést pro jakýkoli typ entity a typ klíče:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> Klíče jsou nastaveny, jakmile jsou entity sledovány kontextem, i když je entita ve stavu Přidání. To pomáhá při procházení grafu entit a rozhodování o tom, co dělat s každým, například při použití rozhraní API trackgraph. Hodnota klíče by měla být použita pouze způsobem zde zobrazena _před_ jakékoli volání ke sledování entity.

### <a name="with-other-keys"></a>S dalšími klávesami

Některé další mechanismus je potřeba k identifikaci nových entit, když hodnoty klíče nejsou generovány automaticky. Existují dva obecné přístupy k tomuto:

* Dotaz na entitu
* Předání příznaku od klienta

Chcete-li zadat dotaz na entitu, použijte metodu Najít:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#IsItNewQuery)]

Je nad rámec tohoto dokumentu zobrazit úplný kód pro předávání příznak u klienta. Ve webové aplikaci obvykle znamená provádění různých požadavků na různé akce nebo předání některého stavu v požadavku a jeho extrahování v řadiči.

## <a name="saving-single-entities"></a>Uložení jednotlivých entit

Pokud je známo, zda je potřeba vložit nebo aktualizovat, pak buď Přidat nebo Aktualizovat lze použít odpovídajícím způsobem:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

Pokud však entita používá automaticky generované hodnoty klíče, lze pro oba případy použít metodu Update:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

Metoda Update obvykle označí entitu pro aktualizaci, nikoli pro vložení. Pokud však entita má automaticky generovaný klíč a nebyla nastavena žádná hodnota klíče, je entita místo toho automaticky označena pro vložení.

> [!TIP]  
> Toto chování bylo zavedeno v EF Core 2.0. Pro dřívější verze je vždy nutné explicitně zvolit přidat nebo aktualizovat.

Pokud entita nepoužívá automaticky generované klíče, musí aplikace rozhodnout, zda má být entita vložena nebo aktualizována: Například:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

Kroky jsou zde:

* Pokud najít vrátí null, pak databáze již neobsahuje blog s tímto ID, takže voláme Přidat značku pro vložení.
* Pokud najít vrátí entitu, pak existuje v databázi a kontext je nyní sledování existující entity
  * Potom použijeme SetValues k nastavení hodnot pro všechny vlastnosti této entity na ty, které přišly z klienta.
  * Volání SetValues označí entitu, která má být podle potřeby aktualizována.

> [!TIP]  
> SetValues bude označovat pouze jako upravené vlastnosti, které mají jiné hodnoty než v sledované entitě. To znamená, že při odeslání aktualizace budou aktualizovány pouze sloupce, které byly skutečně změněny. (A pokud se nic nezměnilo, nebude odeslána žádná aktualizace.)

## <a name="working-with-graphs"></a>Práce s grafy

### <a name="identity-resolution"></a>Rozlišení identity

Jak je uvedeno výše, EF Core můžete sledovat pouze jednu instanci libovolné entity s danou hodnotou primárního klíče. Při práci s grafy graf by měl být v ideálním případě vytvořen tak, aby tato invarianta je zachována a kontext by měl být použit pouze pro jednu jednotku práce. Pokud graf obsahuje duplikáty, bude nutné graf zpracovat před odesláním do EF, aby se konsolidovalo více instancí do jednoho. To nemusí být triviální, pokud instance mají konfliktní hodnoty a vztahy, takže konsolidace duplicity by mělo být provedeno co nejdříve v kanálu aplikace, aby se zabránilo řešení konfliktů.

### <a name="all-newall-existing-entities"></a>Všechny nové/všechny existující entity

Příkladem práce s grafy je vložení nebo aktualizace blogu spolu s jeho sbírkou přidružených příspěvků. Pokud by měly být vloženy všechny entity v grafu nebo všechny by měly být aktualizovány, pak je proces stejný, jak je popsáno výše pro jednotlivé entity. Například graf blogů a příspěvků vytvořených takto:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

lze vložit takto:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertGraph)]

Výzva přidat označí blog a všechny příspěvky, které mají být vloženy.

Podobně pokud je třeba aktualizovat všechny entity v grafu, lze použít aktualizaci:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#UpdateGraph)]

Blog a všechny jeho příspěvky budou označeny jako aktualizované.

### <a name="mix-of-new-and-existing-entities"></a>Kombinace nových a stávajících subjektů

S automaticky generovanými klíči lze aktualizaci znovu použít pro vložení i aktualizace, i když graf obsahuje kombinaci entit, které vyžadují vložení, a těch, které vyžadují aktualizaci:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

Aktualizace označí libovolnou entitu v grafu, blogu nebo příspěvku pro vložení, pokud nemá nastavenou hodnotu klíče, zatímco všechny ostatní entity jsou označeny pro aktualizaci.

Stejně jako dříve, pokud nepoužíváte automaticky generované klíče, lze použít dotaz a některé zpracování:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a>Zpracování odstranění

Odstranění může být obtížné zpracovat, protože často absence entity znamená, že by měla být odstraněna. Jedním ze způsobů, jak se s tím vypořádat, je použití "měkkých odstranění", takže entita je označena jako odstraněná, nikoli skutečně odstraněna. Odstraní pak se stane stejným jako aktualizace. Obnovitelné odstranění lze implementovat pomocí [filtrů dotazů](xref:core/querying/filters).

Pro true odstraní, společný vzor je použít rozšíření vzorku dotazu k provedení co je v podstatě rozdíl grafu. Příklad:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a>TrackGraph

Interně Přidat, Připojit a Aktualizovat použít graf-traversal s určením pro každou entitu, zda by měla být označena jako Přidáno (vložit), Změněno (aktualizovat), Nezměněno (nedělat nic) nebo Odstraněno (odstranit). Tento mechanismus je vystaven prostřednictvím rozhraní API trackgraphu. Předpokládejme například, že když klient odešle zpět graf entit, nastaví u každé entity určitý příznak označující, jak by měl být zpracován. TrackGraph pak lze použít ke zpracování tohoto příznaku:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#TrackGraph)]

Příznaky jsou zobrazeny pouze jako součást entity pro jednoduchost příkladu. Obvykle příznaky by být součástí DTO nebo nějaký jiný stav zahrnuty v požadavku.
