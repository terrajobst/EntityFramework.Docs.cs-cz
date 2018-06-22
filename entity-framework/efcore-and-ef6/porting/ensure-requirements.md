---
title: Portování ze EF6 na jádro EF – ověří, jestli požadavky
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 2f45039e63546d266ec6ce0bfa66ef7e9fb3d7e7
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054253"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a>Před Portování ze EF6 na jádro EF: ověření požadavků vaší aplikace

Před zahájením procesu přenosem je potřeba ověřit, že základní EF splňuje požadavky na přístup pro vaši aplikaci.

## <a name="missing-features"></a>Chybějící funkce

Ujistěte se, že EF základní obsahuje všechny funkce, které budete muset použít v aplikaci. V tématu [porovnání funkcí](../features.md) podrobné porovnání jak funkci nastavena v základní EF porovnává EF6. Pokud chybí všechny požadované funkce, ujistěte se, nedostatečná tyto funkce vám může kompenzovat před portování do EF základní.

## <a name="behavior-changes"></a>Změny chování

Toto je seznam některé změny v chování mezi EF6 a EF základní doplňovat. Je důležité udržovat v paměti jako portů aplikaci jako se můžou změnit způsob, jakým aplikace se chová, ale nebude zobrazují jako chyby při kompilaci po odkládací na EF jádro.

### <a name="dbsetaddattach-and-graph-behavior"></a>Chování DbSet.Add/Attach a graf

V EF6 volání `DbSet.Add()` o výsledcích entity v rekurzivní hledání pro všechny entity, kterou se odkazuje v jeho navigační vlastnosti. Všechny entity, které se nacházejí a nejsou již sledován pomocí kontextu, jsou také označit jako přidat. `DbSet.Attach()`se chová stejně, s výjimkou všechny entity jsou označeny jako beze změny.

**Základní EF provede podobné rekurzivní hledání, ale některé mírně odlišná pravidla.**

*  Entita kořenové je vždy v požadovaný stav (přidat pro `DbSet.Add` a nezměněné pro `DbSet.Attach`).

*  **Pro entity, které se nacházejí během rekurzivní hledání navigační vlastnosti:**

    *  **Pokud je primární klíč entity generovaný úložištěm**

        * Pokud primární klíč není nastavena na hodnotu, stav nastaven na přidaný. Hodnotu primárního klíče považuje za "není nastavena" Pokud má přiřazenou CLR výchozí hodnota pro typ vlastnosti (tj. `0` pro `int`, `null` pro `string`atd.).

        * Pokud primární klíč je nastavena na hodnotu, stav nastaven beze změny.

    *  Pokud není primární klíč generovaný databází, je entita umístit do stejného stavu jako kořen.

### <a name="code-first-database-initialization"></a>Kód první inicializaci databáze

**EF6 má významné množství magic, které provádí kolem výběrem připojení k databázi a inicializace databáze. Mezi tato pravidla patří:**

* Pokud žádná konfigurace se provádí, vybere EF6 databáze na SQL Express nebo LocalDb.

* Pokud je řetězec připojení se stejným názvem jako kontext v aplikacích `App/Web.config` souboru, použije se toto připojení.

* Pokud databáze neexistuje, vytvoří se.

* Pokud žádná z tabulek z modelu neexistují v databázi, schéma pro aktuální model se přidá do databáze. Pokud jsou povolené migrace, pak se používají k vytvoření databáze.

* Pokud existuje databáze a EF6 dříve vytvořili schéma, schéma se kontroluje k zajištění kompatibility se aktuální model. Pokud model změnil od chvíle vytvoření schématu, je vyvolána výjimka.

**Základní EF neprovádí žádné z této magic.**

* Připojení k databázi musí být explicitně nakonfigurovaný v kódu.

* Neprobíhá žádná inicializace. Je nutné použít `DbContext.Database.Migrate()` použití migrace (nebo `DbContext.Database.EnsureCreated()` a `EnsureDeleted()` k vytvoření nebo odstranění databáze bez použití migrace).

### <a name="code-first-table-naming-convention"></a>První tabulka kód zásady vytváření názvů

EF6 spouští prostřednictvím pluralizační služby k výpočtu výchozí název tabulky, který entita je namapovaná na název třídy entity.

Základní EF používá název `DbSet` vlastnost, která entity je zpřístupněná na odvozené kontextu. Pokud nemá entity `DbSet` vlastnost a potom název třídy se používá.
