---
title: "Odpojené entity - EF jádra"
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
ms.technology: entity-framework-core
uid: core/saving/disconnected-entities
ms.openlocfilehash: 0ea02876b9594d54c971a7b70fcf7ce591e56ba0
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/22/2017
---
# <a name="disconnected-entities"></a>Odpojené entity

DbContext instance bude automaticky sledovat entity vrácená z databáze. Změny provedené v těchto entitách bude zjištěna, pak když je volána metoda SaveChanges a databáze bude aktualizován, podle potřeby. V tématu [základní Uložit](basic.md) a [související Data](related-data.md) podrobnosti.

Ale někdy entity jsou předmětem dotazování pomocí jedné instance kontextu a pak je uložena pomocí jiné instance. K tomu často dochází v "odpojené" scénáře, jako je webová aplikace, kde entity dotaz, může odeslat klientovi, upravit, odesílání zpět na server v požadavku a pak je uložena. V takovém případě kontext druhou instanci musí vědět, jestli entity jsou nové (musí být zařazeny) nebo existující (by měly být aktualizovány).

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) na Githubu.

## <a name="identifying-new-entities"></a>Identifikace nové entity

### <a name="client-identifies-new-entities"></a>Klient určí nové entity

Nejjednodušším případě řešit je, když klient informuje server, jestli entity jsou nové nebo existující. Například často žádost o vložení nové entity se liší od požadavku aktualizace stávající entity.

Zbytek této části popisuje případy kde je potřeba určit jiným způsobem, jestli se má vložit nebo aktualizovat.

### <a name="with-auto-generated-keys"></a>Automaticky generovaného klíče

Hodnota automaticky generovaného klíče můžete často používá k určení, jestli entity je nutné vložit nebo aktualizovat. Pokud klíč nebylo nebyl nastavený, (tj. stále má CLR výchozí hodnotu null, nula, atd.), pak musí být nové entity a potřebuje vkládání. Na druhé straně-li nastavena hodnota klíče, pak se musí již byly dříve uloženy a nyní je nutné aktualizovat. Jinými slovy Pokud klíč má hodnotu, pak entity byla dotazována může odeslat klientovi a má teď vraťte aktualizovat.

Je snadné vyhledávat nenastavené klíč, když je známý typ entity:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewSimple)]

EF má však také předdefinovaný způsob, jak to udělat pro libovolného typu entity a typ klíče:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> Klíče jsou nastaveny také entity jsou sledovány objektem kontext, i v případě, že tato entita je ve stavu Added. To pomáhá při procházení graf entity a rozhodování, co dělat s každou, například, jako při použití rozhraní API TrackGraph. Hodnota klíče by měl použít pouze ve způsobu, jakým jsou tady uvedené _před_ žádné Přišla žádost o sledování entity.

### <a name="with-other-keys"></a>Pomocí jiných klíčů

K identifikaci nové entity, když nejsou automaticky generované hodnoty klíče je nutné jinému kontrolnímu mechanismu. Existují dvě obecné přístupy k tomuto:
 * Dotaz pro entitu
 * Předat příznak z klienta

Dotaz pro entitu, stačí použijte metodu najít:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewQuery)]

Je nad rámec tohoto dokumentu zobrazit úplného kódu pro předávání příznak z klienta. Ve webové aplikaci obvykle to znamená provedení různých žádostí pro různé akce, nebo předávání některých stavu v požadavku a extrahování v kontroleru.

## <a name="saving-single-entities"></a>Ukládání jedné entity

Pokud je známo, zda je potřeba příkazu insert nebo update a potom přidat nebo aktualizovat slouží odpovídajícím způsobem:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

Ale entita používá automaticky generovaný hodnoty klíče, pak metodu aktualizace lze nastavit pro oba případy:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

Metoda aktualizace označí normálně entity pro aktualizaci, není insert. Ale pokud entita, která má automaticky generovaný klíč a žádná hodnota klíče je nastavená entity se místo toho automaticky označeny pro vložení.

> [!TIP]  
> Toto chování byla zavedena v EF základní 2.0. Pro starší verze je vždy nutné explicitně vyberte Přidat nebo aktualizovat.

Pokud entita není pomocí automaticky generovaného klíče, pak aplikace musíte rozhodnout, zda by měla být vložit nebo aktualizovat entitu: Příklad:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

Postup v tomto poli se:
* Pokud hledání vrátí hodnotu null, pak databázi už neobsahuje blog s tímto ID, se nazývá přidat ji označte pro vložení.
* Pokud hledání vrací entity, pak existuje v databázi a kontext je nyní sledování stávající entity
  * Potom používáme SetValues nastavit hodnoty pro všechny vlastnosti pro tuto entitu na ty, které pocházejí z klienta.
  * Volání SetValues označíte entity, která má být aktualizována podle potřeby.

> [!TIP]  
> SetValues pouze označíte jako upravená vlastnosti, které mají různé hodnoty na hodnoty v sledovaných entity. To znamená, pokud jsou odesílány, je aktualizován pouze sloupce, které se změnily ve skutečnosti. (A pokud se nic změnila, pak žádná aktualizace se budou odesílat vůbec.)

## <a name="working-with-graphs"></a>Práce s grafy

### <a name="all-newall-existing-entities"></a>Všechny nové nebo žádná existující entity

Příkladem práci s grafy je vložení nebo aktualizace společně s jeho kolekce přidružené příspěvky blogu. Pokud má být vložen všechny entity v grafu nebo by měly být aktualizovány všechny, pak proces je stejné, jak je popsáno výše pro jedné entity. Například graf blogy a příspěvcích vytvořit takto:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

můžete vložit takto:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertGraph)]

Volání přidat označíte blogu a v příspěvcích má být vložen.

Podobně pokud všechny entity v grafu je nutné aktualizovat, pak aktualizace lze použít:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#UpdateGraph)]

Blog a všechny jeho příspěvky budou označeny aktualizovat.

### <a name="mix-of-new-and-existing-entities"></a>Kombinace nových nebo stávajících entit

Automaticky generovaného klíče aktualizace můžete znovu použít pro vložení a aktualizace, i v případě, že graf obsahuje kombinaci entity, které vyžadují vkládání a ty, které vyžadují aktualizaci:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

Aktualizace označí všechny entity v grafu, blogu nebo post pro vložení, pokud nemá sadu hodnotu klíče, zatímco ostatní entity jsou označena k aktualizaci.

Jako dříve, pokud nepoužíváte automaticky generovaného klíče dotazu a některé zpracování lze použít:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a>Zpracování odstranění

Odstranění může být složité zpracování od často znamená absenci entitu, měla by být odstraněna. Jedním ze způsobů, jak nakládat s to je použití "obnovitelného odstranění" tak, že entita je označena k odstranění místo ve skutečnosti odstraňuje. Odstraní se pak stane stejná jako aktualizace. Obnovitelného odstranění můžou se implementovat v pomocí [dotaz filtry](xref:core/querying/filters).

Pro odstranění hodnotu true má pomocí rozšíření dotazu vzoru provádět, co je v podstatě graf rozdíl běžný vzor Příklad:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a>TrackGraph

Interně, přidat, připojit a aktualizace pomocí graf traversal zjištění vytvářeny pro každou entitu, jestli ho by měl být označen jako Added (Chcete-li vložit) upravené (k aktualizaci) Unchanged (nedělat nic), nebo odstraněné (Chcete-li odstranit). Tento mechanismus je vystaven prostřednictvím rozhraní API TrackGraph. Například předpokládejme, že když klient odešle zpět graf entit nastaví některé příznak u každé entity, která určuje, jak by měly zpracovávat. TrackGraph pak umožňuje zpracovat tento příznak:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#TrackGraph)]

V příznacích se zobrazují pouze v rámci entity, pro jednoduchost v příkladu. Obvykle příznaků by být součástí DTO nebo jiný stav součástí požadavku.
