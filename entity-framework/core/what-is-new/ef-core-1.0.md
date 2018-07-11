---
title: Novinky v EF Core 1.0 – EF Core
author: divega
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 20A25111-AEBE-4BC2-83A5-3F651952DF72
ms.technology: entity-framework-core
uid: core/what-is-new/ef-core-1.0
ms.openlocfilehash: af7cf490ef2b04afb02461279fbe67c1c7fa3d95
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949019"
---
# <a name="features-included-in-ef-core-10"></a>Funkce obsažené v EF Core 1.0

## <a name="platforms"></a>Platformy
### <a name="net-framework-451"></a>.NET Framework 4.5.1
Zahrnuje konzolu, WPF, WinForms, ASP.NET 4 atd.
### <a name="net-standard-13"></a>.NET standard 1.3
Včetně ASP.NET Core, které cílí na rozhraní .NET Framework a .NET Core ve Windows, os x a Linuxu.

## <a name="modelling"></a>Modelování
### <a name="basic-modelling"></a>Základní modelování
Podle POCO entit s get a set vlastnosti společné skalárních typů (`int`, `string`atd.).
### <a name="relationships-and-navigation-properties"></a>Vztahy a navigačních vlastností
1 n a – nula nebo-relace se dá nastavit v modelu na základě cizího klíče. Navigační vlastnosti kolekce nebo odkaz typů jednoduché lze přidružit tyto vztahy.
### <a name="built-in-conventions"></a>Integrované konvence
Tyto sestavit model ve službě počáteční založené na obrazec tříd entit.
### <a name="fluent-api"></a>Rozhraní Fluent API
Umožňuje přepsat `OnModelCreating` metodu na kontext k další konfiguraci modelu, který byl zjištěn konvencí.
### <a name="data-annotations"></a>Datové poznámky
Jsou atributy, které lze přidat do vaší třídy a vlastnosti entity a ovlivní EF modelu. Například přidáním `[Required]` vám umožní EF vědět, že je vyžadována určitá vlastnost.
### <a name="relational-table-mapping"></a>Relační mapování tabulek
Umožňuje entity, které mají být namapovány na tabulky nebo sloupce.
### <a name="key-value-generation"></a>Generování klíč-hodnota
Včetně generace na straně klienta a generování databáze.
### <a name="database-generated-values"></a>Databáze vygenerovala hodnoty
Umožňuje pro hodnoty, které mají být generovány databázi při vložení (výchozí hodnoty) nebo aktualizace (vypočítané sloupce).
### <a name="sequences-in-sql-server"></a>Pořadí v systému SQL Server
Umožňuje sekvenci objektů byly definovány v modelu.
### <a name="unique-constraints"></a>Jedinečná omezení
Umožňuje definice alternativních klíčů a možnost definovat relace, které cílí tento klíč.
### <a name="indexes"></a>Indexy
Definování indexy v modelu automaticky zavádí indexy v databázi. Jedinečné indexy jsou také podporovány.
### <a name="shadow-state-properties"></a>Stínové vlastnosti stavu
Umožňuje pro vlastnosti definované v modelu, které nejsou deklarovány a nejsou uloženy ve třídě rozhraní .NET, ale může být sledovány a aktualizuje pomocí EF Core. Běžně používaná pro vlastnosti cizího klíče při vystavení v objektu není žádoucí.
### <a name="table-per-hierarchy-inheritance-pattern"></a>Model tabulka za hierarchie dědičnosti
Umožňuje entit v hierarchie dědičnosti bude uložena do jedné tabulky pomocí sloupec diskriminátoru identifikovat jejich typ entity pro daný záznam v databázi.
### <a name="model-validation"></a>Ověření modelu
Zjistí neplatné vzory v modelu a poskytuje užitečné chybové zprávy.

## <a name="change-tracking"></a>Sledování změn
### <a name="snapshot-change-tracking"></a>Snímek řešení change tracking
Umožňuje, aby změny v entitách rozpoznat automaticky pomocí porovnání aktuálního stavu proti kopie (snímek) původního stavu.
### <a name="notification-change-tracking"></a>Sledování změn je oznámení
Umožňuje entity upozornit modul sledování změny při změně hodnoty vlastnosti.
### <a name="accessing-tracked-state"></a>Přístup k sledované stavu
Prostřednictvím `DbContext.Entry` a `DbContext.ChangeTracker`.
### <a name="attaching-detached-entitiesgraphs"></a>Připojení odpojených entit a grafy
Nové `DbContext.AttachGraph` API pomáhá, aby bylo možné uložit nových/upravených entity připojí entity do kontextu.

## <a name="saving-data"></a>Ukládání dat
### <a name="basic-save-functionality"></a>Základní funkce uložení
Umožňuje měnit instancí entit natrvalo do databáze.
### <a name="optimistic-concurrency"></a>Optimistická souběžnost
Chrání proti přepsání změny provedené jinými uživateli, protože se načetla data z databáze.
### <a name="async-savechanges"></a>Asynchronní metoda SaveChanges
Můžete uvolnit tak aktuální vlákno zpracovávat další požadavky, zatímco databáze zpracovává příkazy vydané z `SaveChanges`.
### <a name="database-transactions"></a>Databázové transakce
Znamená, že `SaveChanges` je vždy atomic (tj. je buď zcela úspěšná nebo nebudou provedeny žádné změny k databázi). Existují také transakce související s rozhraním API umožňující sdílení transakce mezi místní instancí atd.
### <a name="relational-batching-of-statements"></a>Relační: Dávkové zpracování příkazů
Poskytuje lepší výkon pomocí dávkování více příkazy INSERT/UPDATE/DELETE v jedné odezvě do databáze.

## <a name="query"></a>Dotazy
### <a name="basic-linq-support"></a>Podpora na úrovni Basic LINQ
Poskytuje možnost použití LINQ k načtení dat z databáze.
### <a name="mixed-clientserver-evaluation"></a>Vyhodnocení smíšené klient/server
Umožňuje dotazům obsahují logiku, která nelze vyhodnotit v databázi a musí být vyhodnocena proto po načtení dat do paměti.
### <a name="notracking"></a>NoTracking
Dotazy umožňuje rychlejší provádění dotazu, když není potřeba kontextu monitorování změn instancí entit (to je užitečné, pokud jsou výsledky jen pro čtení).
### <a name="eager-loading"></a>Předběžné načítání
Poskytuje `Include` a `ThenInclude` metody pro identifikaci související data, která by měla načíst také při dotazování.
### <a name="async-query"></a>Asynchronní dotazu
Můžete uvolnit aktuální vlákno (a související prostředky) zpracovávat další požadavky při databáze zpracovává dotazu.
### <a name="raw-sql-queries"></a>Nezpracované dotazy SQL
Poskytuje `DbSet.FromSql` metodu použít nezpracovaná SQL dotazuje se načíst data. Tyto dotazy je možné také skládání na pomocí jazyka LINQ.

## <a name="database-schema-management"></a>Správa schématu databáze       
### <a name="database-creationdeletion-apis"></a>Databáze vytváření/odstraňování rozhraní API
Většinou jsou navržené pro testování, ve které chcete rychle vytvářet a odstraňovat databáze bez použití migrace.
### <a name="relational-database-migrations"></a>Migrace relačních databází
Povolte schéma relační databáze vyvíjí přesčas jako změny modelu.
### <a name="reverse-engineer-from-database"></a>Zpětná analýza z databáze
Nástroj scaffold modelu EF podle schéma stávající relační databáze.

## <a name="database-providers"></a>Poskytovatelé databází
### <a name="sql-server"></a>SQL Server
Připojení k Microsoft SQL Server 2008 a vyšší.
### <a name="sqlite"></a>SQLite
Se připojí k databázi SQLite 3.
### <a name="in-memory"></a>V paměti
Umožňuje snadno testování bez připojení ke skutečné databázi.
### <a name="3rd-party-providers"></a>Zprostředkovatelé 3.
Několik poskytovatelů jsou k dispozici pro ostatní databázové stroje. Zobrazit [poskytovatelé databází](../providers/index.md) úplný seznam.
