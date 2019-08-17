---
title: Přenos z EF6 na EF Core – ověření požadavků
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 07caa39e8a56db71e493331ea9f0286309abc52a
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565343"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a>Před přenosem z EF6 do EF Core: Ověření požadavků vaší aplikace

Než zahájíte proces přenosu, je důležité ověřit, že EF Core splňuje požadavky na přístup k datům pro vaši aplikaci.

## <a name="missing-features"></a>Chybějící funkce

Ujistěte se, že EF Core obsahuje všechny funkce, které v aplikaci potřebujete použít. Podrobné porovnání toho, jak je sada funkcí v EF Core porovnána s EF6, naleznete v tématu [porovnání funkcí](../features.md) . Pokud některé z požadovaných funkcí chybí, zajistěte, abyste před přenosem na EF Core mohli kompenzovat nedostatek těchto funkcí.

## <a name="behavior-changes"></a>Změny chování

Toto je nevyčerpávající seznam některých změn v chování mezi EF6 a EF Core. Je důležité mít na paměti, že vaše aplikace může měnit způsob, jakým se aplikace chová, ale po odchodu do EF Core se nezobrazí jako chyby kompilace.

### <a name="dbsetaddattach-and-graph-behavior"></a>Negenerickými. Přidání/připojení a chování grafu

V EF6 volání `DbSet.Add()` na entitu má za následek rekurzivní vyhledávání všech entit, na které se odkazuje ve vlastnostech navigace. Všechny nalezené entity a již nejsou sledovány kontextem, jsou také označeny jako přidané. `DbSet.Attach()`se chová stejně, s výjimkou všech entit jsou označeny jako beze změny.

**EF Core provádí podobné rekurzivní vyhledávání, ale s trochu odlišnými pravidly.**

*  Kořenová entita je vždy ve vyžádaném stavu (přidáno `DbSet.Add` pro a `DbSet.Attach`beze změny).

*  **Pro entity, které byly nalezeny během rekurzivního prohledávání vlastností navigace:**

    *  **Pokud je primární klíč entity vygenerovaný jako úložiště**

        * Pokud primární klíč není nastaven na hodnotu, stav je nastaveno na přidáno. Hodnota primárního klíče se považuje za nenastavenou, pokud je přiřazena výchozí hodnota CLR pro daný typ vlastnosti ( `0` například pro `int`, `null` pro `string`atd.).

        * Pokud je primární klíč nastaven na hodnotu, stav je nastaven na nezměněný.

    *  Pokud primární klíč není vygenerovaný databází, entita se umístí do stejného stavu jako kořenový adresář.

### <a name="code-first-database-initialization"></a>Inicializace databáze Code First

**EF6 má velký výkon, který vychází z výběru připojení databáze a inicializace databáze. Mezi tato pravidla patří:**

* Pokud se neprovede žádná konfigurace, EF6 vybere databázi na SQL Express nebo LocalDb.

* Pokud je připojovací řetězec se stejným názvem jako kontext v souboru aplikace `App/Web.config` , bude použito toto připojení.

* Pokud databáze neexistuje, vytvoří se.

* Pokud v databázi neexistuje žádná z tabulek z modelu, je schéma pro aktuální model přidáno do databáze. Pokud jsou povolené migrace, použijí se k vytvoření databáze.

* Pokud databáze existuje a EF6 dříve vytvořila schéma, je u schématu kontrolována kompatibilita s aktuálním modelem. Výjimka je vyvolána, pokud se model od vytvoření schématu změnil.

**EF Core neprovádí žádné z těchto Magic.**

* Připojení k databázi musí být explicitně nakonfigurované v kódu.

* Není provedena žádná inicializace. K použití migrace `DbContext.Database.Migrate()` ( `DbContext.Database.EnsureCreated()` `EnsureDeleted()` nebo k vytvoření/odstranění databáze bez použití migrace) musíte použít.

### <a name="code-first-table-naming-convention"></a>Code First zásady vytváření názvů tabulek

EF6 spustí název třídy entity prostřednictvím služby pro pojmenování a vypočítá výchozí název tabulky, na kterou je entita namapována.

EF Core používá název `DbSet` vlastnosti, ke které je entita vystavena v odvozeném kontextu. Pokud entita `DbSet` neobsahuje vlastnost, použije se název třídy.
