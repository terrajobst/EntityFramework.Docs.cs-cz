---
title: Přenos z EF6 na EF Core – EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 826b58bd-77b0-4bbc-bfcd-24d1ed3a8f38
uid: efcore-and-ef6/porting/index
ms.openlocfilehash: 77096b9bffba6b8c2a3d7bfb0c2e41e2d170a7db
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416950"
---
# <a name="porting-from-ef6-to-ef-core"></a>Přenesení z EF6 do EF Core

Vzhledem k zásadní změny v EF Core nedoporučujeme pokoušet se přesunout ef6 aplikace EFCore, pokud máte pádný důvod provést změnu.
Měli byste zobrazit přechod z EF6 na EF Core jako port, nikoli upgrade.

> [!IMPORTANT]
> Před zahájením procesu přenosu je důležité ověřit, zda EF Core splňuje požadavky na přístup k datům pro vaši aplikaci.

## <a name="missing-features"></a>Chybějící funkce

Ujistěte se, že EF Core má všechny funkce, které potřebujete použít ve vaší aplikaci. Podrobné porovnání vlastností sady funkcí v EF Core s ef6 najdete v tématu [Porovnání funkcí.](xref:efcore-and-ef6/index) Pokud některé požadované funkce chybí, ujistěte se, že můžete kompenzovat nedostatek těchto funkcí před přenesením na EF Core.

## <a name="behavior-changes"></a>Změny chování

Toto je nevyčerpávající seznam některých změn v chování mezi EF6 a EF Core. Je důležité mít na paměti, jako port aplikace, protože mohou změnit způsob, jakým se vaše aplikace chová, ale nezobrazí se jako chyby kompilace po přepnutí na EF Core.

### <a name="dbsetaddattach-and-graph-behavior"></a>DbSet.Add/Attach a chování grafu

V EF6 `DbSet.Add()` volání entity má za následek rekurzivní hledání pro všechny entity odkazované v jeho navigační vlastnosti. Všechny entity, které jsou nalezeny a nejsou již sledovány kontextem, jsou také označeny jako přidané. `DbSet.Attach()`chová se stejně, s výjimkou všech entit jsou označeny jako nezměněné.

**EF Core provádí podobné rekurzivní vyhledávání, ale s některými mírně odlišnými pravidly.**

*  Kořenová entita je vždy v `DbSet.Add` požadovaném `DbSet.Attach`stavu (přidána pro a nezměněná pro ).

*  **Pro entity, které se nacházejí během rekurzivního hledání navigačních vlastností:**

    *  **Pokud je vygenerován primární klíč entity**

        * Pokud primární klíč není nastaven na hodnotu, stav je nastaven na přidán. `0` Hodnota primárního klíče je považována za "není nastavena", pokud je přiřazena `int`výchozí `null` `string`hodnota CLR pro typ vlastnosti (například pro , for , atd.).

        * Pokud je primární klíč nastaven na hodnotu, stav je nastaven na nezměněné.

    *  Pokud primární klíč není generován databáze, entita je umístěn ve stejném stavu jako kořen.

### <a name="code-first-database-initialization"></a>První inicializace první databáze kódu

**EF6 má značné množství magie provádí kolem výběru připojení k databázi a inicializace databáze. Některá z těchto pravidel zahrnují:**

* Pokud není provedena žádná konfigurace, EF6 vybere databázi na SQL Express nebo LocalDb.

* Pokud je v souboru aplikace `App/Web.config` připojovací řetězec se stejným názvem jako kontext, bude použito toto připojení.

* Pokud databáze neexistuje, je vytvořena.

* Pokud žádná z tabulek z modelu existují v databázi, schéma pro aktuální model je přidán do databáze. Pokud jsou povoleny migrace, pak se používají k vytvoření databáze.

* Pokud databáze existuje a EF6 dříve vytvořil schéma, pak je schéma zkontrolováno kompatibilitu s aktuálním modelem. Výjimka je vyvolána, pokud se model změnil od vytvoření schématu.

**EF Core neprovádí žádné z této magie.**

* Připojení databáze musí být explicitně nakonfigurováno v kódu.

* Není provedena žádná inicializace. Je nutné `DbContext.Database.Migrate()` použít migrace `DbContext.Database.EnsureCreated()` (nebo `EnsureDeleted()` vytvořit nebo odstranit databázi bez použití migrace).

### <a name="code-first-table-naming-convention"></a>Konvence pojmenování první tabulky kódu

EF6 spustí název třídy entity prostřednictvím služby pluralizace k výpočtu výchozího názvu tabulky, na který je entita mapována.

EF Core používá název `DbSet` vlastnosti, která je vystavena v na odvozeném kontextu. Pokud entita nemá `DbSet` vlastnost, použije se název třídy.
