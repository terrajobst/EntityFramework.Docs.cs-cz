---
title: Odpojené entity – EF Core
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
uid: core/saving/disconnected-entities
ms.openlocfilehash: 88c3fa8ea5b8246a932f5cf21e674bc7cc71c0ea
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656272"
---
# <a name="disconnected-entities"></a>Odpojené entity

Instance DbContext bude automaticky sledovat entity vracené z databáze. Změny provedené u těchto entit budou zjištěny při volání metody SaveChanges a databáze bude aktualizována podle potřeby. Podrobnosti najdete v tématu základní informace o [uložení](basic.md) a [související data](related-data.md) .

Někdy se ale entity dotazují pomocí jedné instance kontextu a pak se ukládají pomocí jiné instance. K tomu často dochází v případě "odpojených" scénářů, jako je například webová aplikace, ve které se tyto entity odesílají do klienta, které se odešlou do klienta, změnili, odeslali zpátky na server v žádosti a pak se uložil. V takovém případě musí druhá instance kontextu zjistit, jestli jsou entity nové (měly by být vložené) nebo existující (by se měly aktualizovat).

<!-- markdownlint-disable MD028 -->
> [!TIP]
> [Ukázku](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Disconnected/) tohoto článku můžete zobrazit na GitHubu.

> [!TIP]
> EF Core může sledovat jenom jednu instanci libovolné entity s daným hodnotou primárního klíče. Nejlepším způsobem, jak se tomuto problému vyhnout, je použití krátkodobého kontextu pro každou jednotku, aby byl kontext spuštěný, a obsahuje entity, které jsou k němu připojené, ukládají tyto entity a pak je tento kontext vyřazený a zahozený.
<!-- markdownlint-enable MD028 -->

## <a name="identifying-new-entities"></a>Identifikace nových entit

### <a name="client-identifies-new-entities"></a>Klient identifikuje nové entity.

Nejjednodušší případ, který se má řešit, je, že klient informuje server, jestli je entita nová nebo existující. Například požadavek na vložení nové entity se liší od žádosti o aktualizaci existující entity.

Zbývající část této části popisuje případy, kdy je potřeba určit nějaký jiný způsob, jak vkládat nebo aktualizovat.

### <a name="with-auto-generated-keys"></a>S automaticky generovanými klíči

Hodnota automaticky generovaného klíče se dá často použít k určení, jestli se entita musí vložit nebo aktualizovat. Pokud klíč nebyl nastaven (to znamená, že má stále výchozí hodnotu CLR null, nula atd.), musí být entita nová a musí vložit. Na druhé straně, pokud je hodnota klíče nastavená, musí se už dřív Uložit a teď je potřeba aktualizovat. Jinými slovy, pokud má klíč hodnotu, pak byla entita dotazována, odeslána klientovi a nyní se vrátila k aktualizaci.

Pokud je typ entity znám, je snadné kontrolovat nenastavené klíče:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#IsItNewSimple)]

EF má však také vestavěný způsob, jak to provést pro libovolný typ entity a typ klíče:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> Klíče jsou nastaveny, jakmile jsou entity sledovány kontextem, a to i v případě, že je entita ve stavu přidáno. To pomáhá při procházení grafu entit a rozhodování o tom, co dělat s každým, například při použití rozhraní TrackGraph API. Hodnota klíče by měla být použita pouze způsobem, který je zde zobrazen, _předtím, než_ bude provedeno volání ke sledování entity.

### <a name="with-other-keys"></a>S jinými klíči

K identifikaci nových entit je potřeba nějaký jiný mechanismus, když se klíčové hodnoty negenerují automaticky. Existují dva obecné přístupy:

* Dotaz na entitu
* Předat příznak z klienta

Chcete-li zadat dotaz na entitu, stačí použít metodu Find:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#IsItNewQuery)]

Je nad rámec tohoto dokumentu, aby zobrazoval úplný kód pro předání příznaku z klienta. Ve webové aplikaci to obvykle znamená, že různé požadavky na různé akce nebo předání nějakého stavu v žádosti a jejich extrakce v řadiči.

## <a name="saving-single-entities"></a>Ukládání jednoduchých entit

Je-li známo, zda je vyžadováno vložení nebo aktualizace, lze použít buď možnost Přidat nebo aktualizovat, a to odpovídajícím způsobem:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

Pokud však entita používá automaticky generované hodnoty klíčů, může být metoda aktualizace použita v obou případech:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

Metoda aktualizace obvykle označí entitu pro aktualizaci, nikoli INSERT. Pokud však entita obsahuje automaticky generovaný klíč a nebyla nastavena žádná hodnota klíče, je entita namísto toho automaticky označena pro vložení.

> [!TIP]  
> Toto chování bylo zavedeno v EF Core 2,0. V dřívějších verzích je vždycky nutné explicitně zvolit možnost Přidat nebo aktualizovat.

Pokud entita nepoužívá automaticky generované klíče, musí se rozhodnout, zda má být entita vložena nebo aktualizována: například:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

Postup najdete tady:

* Pokud funkce Find vrátí hodnotu null, databáze již neobsahuje blog s tímto ID, proto zavolejte metodu Add, kterou označíte pro vložení.
* Pokud funkce Find vrátí entitu, pak v databázi existuje a kontext teď sleduje existující entitu.
  * Pak pomocí vlastností SetValue nastavíme hodnoty všech vlastností u této entity na ty, které pocházejí z klienta.
  * Volání NastavitHodnotu označí entitu, která se má aktualizovat podle potřeby.

> [!TIP]  
> SetValues bude označovat jenom jako změněné vlastnosti, které mají jiné hodnoty jako ty ve sledované entitě. To znamená, že při odeslání aktualizace se aktualizují pouze sloupce, které se skutečně změnily. (A pokud se nic nezměnilo, nepošle se vůbec žádná aktualizace.)

## <a name="working-with-graphs"></a>Práce s grafy

### <a name="identity-resolution"></a>Překlad identity

Jak je uvedeno výše, EF Core může sledovat jenom jednu instanci libovolné entity s daným hodnotou primárního klíče. Při práci s grafy by se graf měl ideálně vytvořit tak, že se zachová tato invariantní a kontext by měl být použit pouze pro jednu jednotku v práci. Pokud graf obsahuje duplicity, pak bude nutné zpracovat graf před jeho odesláním do EF, aby bylo možné konsolidovat více instancí do jednoho. Nemusí se jednat o triviální, kde mají instance konfliktní hodnoty a relace, takže by se měly v kanálu aplikace co nejdřív dělat konsolidace duplicit, aby se zabránilo řešení konfliktů.

### <a name="all-newall-existing-entities"></a>Všechny nové/všechny existující entity

Příkladem práce s grafy je vložení nebo aktualizace blogu spolu s kolekcí přidružených příspěvků. Pokud by měly být všechny entity v grafu vložené nebo by se měly aktualizovat všechny, pak je tento proces stejný, jak je popsáno výše u jednotlivých entit. Například graf blogů a příspěvků vytvořených takto:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

může být vložena takto:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertGraph)]

Volání metody Add bude označovat blog a všechny příspěvky, které mají být vloženy.

Podobně platí, že pokud je potřeba aktualizovat všechny entity v grafu, můžete použít aktualizaci:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#UpdateGraph)]

Blog a všechny jeho příspěvky budou označeny jako aktualizované.

### <a name="mix-of-new-and-existing-entities"></a>Kombinace nových a stávajících entit

U automaticky generovaných klíčů se dá aktualizace znovu použít pro vložení i aktualizace i v případě, že graf obsahuje kombinaci entit, které vyžadují vložení a ty, které vyžadují aktualizaci:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

Aktualizace označí každou entitu v grafu, blogu nebo příspěvku, pokud nemá nastavenou hodnotu klíče, zatímco všechny ostatní entity jsou označené k aktualizaci.

Stejně jako dřív, pokud nepoužíváte automaticky generované klíče, je možné použít dotaz a některé zpracování:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a>Zpracování odstranění

Odstranění může být obtížné zvládnout, protože často absence entity znamená, že by se mělo odstranit. Jedním ze způsobů, jak se toho vypořádat, je použít "obnovitelné odstranění" tak, aby se entita označila jako Odstraněná, a ne skutečně odstranit. Odstranění se pak bude shodovat s aktualizacemi. Obnovitelné odstranění lze implementovat pomocí [filtrů dotazů](xref:core/querying/filters).

Pro skutečný způsob odstranění je běžným vzorem použití rozšíření vzoru dotazu k provedení toho, co je v podstatě rozdíl v grafu. Příklad:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a>TrackGraph

Interně, přidejte, připojte a aktualizujte použít graf-prochází se stanovením pro každou entitu jako na to, jestli by měla být označená jako přidaná (pro vložení), upravená (k aktualizaci), beze změny (nedělat nic) nebo smazána (k odstranění). Tento mechanismus se zveřejňuje prostřednictvím rozhraní TrackGraph API. Předpokládejme například, že když klient pošle zpátky graf entit, nastaví u každé entity příznak, který označuje, jak by se měl zpracovat. TrackGraph se pak dá použít ke zpracování tohoto příznaku:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#TrackGraph)]

Příznaky jsou zobrazeny pouze jako součást entity pro zjednodušení příkladu. Příznaky by byly typicky součástí DTO nebo nějakého jiného stavu zahrnutého v žádosti.
