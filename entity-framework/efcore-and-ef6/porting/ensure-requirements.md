---
title: Přenesení z EF6 do EF Core – ověření požadavků na
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: fd378c72a3c8de4a441861b3c52b94eba5f7e5b4
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993437"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a>Před přenesení z EF6 do EF Core: ověření podle požadavků vaší aplikace

Před zahájením procesu přenos je důležité, abyste ověřili, že EF Core splňuje požadavky na datový přístup pro vaši aplikaci.

## <a name="missing-features"></a>Chybějící funkce

Ujistěte se, že EF Core má všechny funkce, které je potřeba použít ve vaší aplikaci. Zobrazit [porovnání funkcí](../features.md) jak funkce nastavit v EF Core porovná s EF6 podrobné porovnání. Pokud nebyly nalezeny všechny požadované funkce, ujistěte se, že může kompenzovat chybějící tyto funkce před přenesení do EF Core.

## <a name="behavior-changes"></a>Změny chování

Toto je seznam není vyčerpávající některé změny v chování mezi EF6 a EF Core. Je důležité udržovat v paměti jako port aplikace jako mění způsob, jak vaše aplikace chová, ale nezobrazí jako chyby při kompilaci po záměně na EF Core.

### <a name="dbsetaddattach-and-graph-behavior"></a>Graf a DbSet.Add/Attach chování

V EF6 volání `DbSet.Add()` na entity výsledkem rekurzivní hledání pro všechny entity odkazovat v jeho navigační vlastnosti. Žádné entity, které se nacházejí a nejsou již sledován pomocí funkce kontextu, jsou také označit jako přidali. `DbSet.Attach()` se chová stejně, s výjimkou všechny entity jsou označeny jako beze změny.

**EF Core provádí podobné rekurzivní hledání, ale některé mírně odlišná pravidla.**

*  Kořenová entita je vždy v požadovaného stavu (přidat pro `DbSet.Add` a beze změny pro `DbSet.Attach`).

*  **Pro entity, které byly nalezeny během rekurzivní hledání navigační vlastnosti:**

    *  **Pokud je úložiště vygenerovat primární klíč entity**

        * Pokud primární klíč není nastavena na hodnotu, stav je nastaven na přidání. Hodnota primárního klíče je považován za "Nenastaveno" Pokud je přiřazen výchozí hodnotu CLR pro tento typ vlastnosti (například `0` pro `int`, `null` pro `string`atd.).

        * Pokud primární klíč je nastavena na hodnotu, stav je nastaven na beze změny.

    *  Pokud primární klíč není databáze, které jsou generovány, entita přejde do stejného stavu jako kořen.

### <a name="code-first-database-initialization"></a>Kód první inicializaci databáze

**EF6 má významné množství magic, které provádí po výběru připojení k databázi a inicializaci databáze. Mezi tyto pravidla patří:**

* Pokud žádná konfigurace se provádí, vybere EF6 databáze na SQL Express nebo LocalDb.

* Pokud je řetězec připojení se stejným názvem jako kontext v aplikacích `App/Web.config` souboru, se použije toto připojení.

* Pokud databáze buď neexistuje, vytvoří se.

* Pokud žádná z tabulek z modelu v databázi neexistuje, schéma pro aktuální model je přidána do databáze. Pokud migrace jsou povolené, pak se používají k vytvoření databáze.

* Pokud existuje databáze a EF6 už dřív vytvořil schématu, schéma se kontroluje u kompatibility aktuálního modelu. Pokud model byl změněn od vytvoření schématu je vyvolána výjimka.

**EF Core neprovede žádné tento kouzla.**

* Připojení k databázi musí být explicitně nakonfigurované v kódu.

* Není inicializace provedena. Je nutné použít `DbContext.Database.Migrate()` použití migrace (nebo `DbContext.Database.EnsureCreated()` a `EnsureDeleted()` k vytvoření/odstranění databáze bez použití migrace).

### <a name="code-first-table-naming-convention"></a>První tabulka kódu zásady vytváření názvů

EF6 projde pluralizace služby, která vypočítá výchozí název tabulky, která entita je namapována na název třídy entity.

EF Core používá název `DbSet` vlastnost, která entity zpřístupněn v rámci odvozené. Pokud nemá žádné entity `DbSet` vlastnost a potom název třídy se používá.
