---
title: Přenos z EF6 do EF Core-EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 826b58bd-77b0-4bbc-bfcd-24d1ed3a8f38
uid: efcore-and-ef6/porting/index
ms.openlocfilehash: 77096b9bffba6b8c2a3d7bfb0c2e41e2d170a7db
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182094"
---
# <a name="porting-from-ef6-to-ef-core"></a>Přenesení z EF6 do EF Core

Z důvodu zásadních změn v EF Core nedoporučujeme migraci z EF6 na EF Core, pokud pro takovou změnu nemáte dobrý důvod.
Měli byste zobrazit přesun z EF6, aby se místo upgradu EF Core jako port.

> [!IMPORTANT]
> Než zahájíte proces přenosu, je důležité ověřit, že EF Core splňuje požadavky na přístup k datům pro vaši aplikaci.

## <a name="missing-features"></a>Chybějící funkce

Ujistěte se, že EF Core obsahuje všechny funkce, které v aplikaci potřebujete použít. Podrobné porovnání toho, jak je sada funkcí v EF Core porovnána s EF6, naleznete v tématu [porovnání funkcí](xref:efcore-and-ef6/index) . Pokud některé z požadovaných funkcí chybí, zajistěte, abyste před přenosem na EF Core mohli kompenzovat nedostatek těchto funkcí.

## <a name="behavior-changes"></a>Změny chování

Toto je nevyčerpávající seznam některých změn v chování mezi EF6 a EF Core. Je důležité mít na paměti, že vaše aplikace může měnit způsob, jakým se aplikace chová, ale po odchodu do EF Core se nezobrazí jako chyby kompilace.

### <a name="dbsetaddattach-and-graph-behavior"></a>Negenerickými. Přidání/připojení a chování grafu

V EF6 volání `DbSet.Add()` na entitu má za následek rekurzivní vyhledávání všech entit, na které se odkazuje ve vlastnostech navigace. Všechny nalezené entity a již nejsou sledovány kontextem, jsou také označeny jako přidané. `DbSet.Attach()` se chová stejně, s výjimkou všech entit jsou označeny jako beze změny.

**EF Core provádí podobné rekurzivní vyhledávání, ale s trochu odlišnými pravidly.**

*  Kořenová entita je vždy v požadovaném stavu (přidáno pro `DbSet.Add` a nezměněná pro `DbSet.Attach`).

*  **Pro entity, které byly nalezeny během rekurzivního prohledávání vlastností navigace:**

    *  **Pokud je primární klíč entity vygenerovaný jako úložiště**

        * Pokud primární klíč není nastaven na hodnotu, stav je nastaveno na přidáno. Hodnota primárního klíče se považuje za nenastavenou, pokud je přiřazena výchozí hodnota CLR pro daný typ vlastnosti (například `0` `int`, `null` pro `string`atd.).

        * Pokud je primární klíč nastaven na hodnotu, stav je nastaven na nezměněný.

    *  Pokud primární klíč není vygenerovaný databází, entita se umístí do stejného stavu jako kořenový adresář.

### <a name="code-first-database-initialization"></a>Inicializace databáze Code First

**EF6 má velký výkon, který vychází z výběru připojení databáze a inicializace databáze. Mezi tato pravidla patří:**

* Pokud se neprovede žádná konfigurace, EF6 vybere databázi na SQL Express nebo LocalDb.

* Pokud je připojovací řetězec se stejným názvem jako kontext v souboru aplikace `App/Web.config`, bude použito toto připojení.

* Pokud databáze neexistuje, vytvoří se.

* Pokud v databázi neexistuje žádná z tabulek z modelu, je schéma pro aktuální model přidáno do databáze. Pokud jsou povolené migrace, použijí se k vytvoření databáze.

* Pokud databáze existuje a EF6 dříve vytvořila schéma, je u schématu kontrolována kompatibilita s aktuálním modelem. Výjimka je vyvolána, pokud se model od vytvoření schématu změnil.

**EF Core neprovádí žádné z těchto Magic.**

* Připojení k databázi musí být explicitně nakonfigurované v kódu.

* Není provedena žádná inicializace. Chcete-li použít migraci (nebo `DbContext.Database.EnsureCreated()` a `EnsureDeleted()` k vytvoření nebo odstranění databáze bez použití migrace), je nutné použít `DbContext.Database.Migrate()`.

### <a name="code-first-table-naming-convention"></a>Code First zásady vytváření názvů tabulek

EF6 spustí název třídy entity prostřednictvím služby pro pojmenování a vypočítá výchozí název tabulky, na kterou je entita namapována.

EF Core používá název vlastnosti `DbSet`, ke které je entita vystavena v odvozeném kontextu. Pokud entita nemá vlastnost `DbSet`, použije se název třídy.
