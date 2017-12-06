---
title: "Co je nového v EF základní 1.0 - EF jádra"
author: divega
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 20A25111-AEBE-4BC2-83A5-3F651952DF72
ms.technology: entity-framework-core
uid: core/what-is-new/ef-core-1.0
ms.openlocfilehash: e5b9e57a01ff302b1d7bd0fc5419aa5b8213865e
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="features-included-in-ef-core-10"></a>Funkce obsažené v EF základní 1.0

## <a name="platforms"></a>Platformy
### <a name="net-framework-451"></a>.NET Framework 4.5.1
Zahrnuje konzoly, WPF, WinForms, technologii ASP.NET 4 atd.
### <a name="net-standard-13"></a>Standardní 1.3 rozhraní .NET
Včetně ASP.NET Core cílení na rozhraní .NET Framework a .NET Core v systému Windows, na OSX a Linux.

## <a name="modelling"></a>Modelování
### <a name="basic-modelling"></a>Základní modelování
Podle entity objektů POCO, get nebo nastavení vlastností běžné Skalární typy (`int`, `string`atd.).
### <a name="relationships-and-navigation-properties"></a>Vztahy a navigační vlastnosti
V modelu podle cizí klíč, který lze zadat jeden mnoho a vztahy-nula nebo 1. Navigační vlastnosti jednoduché typy kolekce nebo odkaz může být přidružen tyto relace.
### <a name="built-in-conventions"></a>Předdefinované konvence
Toto sestavení model počáteční podle tvaru tříd entit.
### <a name="fluent-api"></a>Rozhraní Fluent API
Umožňuje potlačit `OnModelCreating` metodu na váš kontext další konfiguraci modelu, který byl zjištěn podle konvence.
### <a name="data-annotations"></a>Datové poznámky
Jsou atributy, které mohou být přidány do tříd nebo vlastností entity a bude ovlivnit model EF (tj. Přidání [požadované] bude umožní EF vědět, že je vyžadována určitá vlastnost).
### <a name="relational-table-mapping"></a>Relační mapování tabulek
Umožňuje entity nejde mapovat na tabulky nebo sloupce.
### <a name="key-value-generation"></a>Hodnota klíče generování
Včetně generování klienta a generování databáze.
### <a name="database-generated-values"></a>Databáze generované hodnoty
Umožňuje hodnoty, které mají být generována databázi na vložení (výchozí hodnoty) nebo aktualizace (počítaných sloupcích).
### <a name="sequences-in-sql-server"></a>Pořadí v systému SQL Server
Umožňuje pořadí objekty být definován v modelu.
### <a name="unique-constraints"></a>Jedinečná omezení
Umožňuje pro danou definici alternativní klíčů a schopnost definovat vztahy cílených klíči.
### <a name="indexes"></a>Indexy
Definování indexy v modelu automaticky zavádí indexy v databázi. Jedinečné indexy jsou také podporovány.
### <a name="shadow-state-properties"></a>Vlastnosti stínové stavu
Umožňuje pro vlastnosti být definován v modelu, které nejsou deklarovány a nejsou uložené v rozhraní .NET třídy, ale je možné sledovat a aktualizovat EF jádra. Nejčastěji používané pro vlastnosti cizího klíče, pokud tyto v objektu vystavení není žádoucí.
### <a name="table-per-hierarchy-inheritance-pattern"></a>Vzor tabulky za hierarchie dědičnosti
Umožňuje entit v hierarchii dědičnosti uložit do jedné tabulky s použitím sloupce diskriminátoru k identifikaci jejich typ entity pro daný záznam v databázi.
### <a name="model-validation"></a>ověření modelu
Zjistí neplatný vzory v modelu a poskytuje užitečné chybové zprávy.

## <a name="change-tracking"></a>sledování změn
### <a name="snapshot-change-tracking"></a>Sledování změn snímku
Umožňuje změny v entity k byla automaticky zjištěna tak, že porovnáte aktuální stav původního stavu před kopírováním (snímek).
### <a name="notification-change-tracking"></a>Sledování změn oznámení
Umožňuje vaší entity upozornit modul sledování změny při změně hodnoty vlastnosti.
### <a name="accessing-tracked-state"></a>Přístup k sledovaných stavu
Prostřednictvím `DbContext.Entry` a `DbContext.ChangeTracker`.
### <a name="attaching-detached-entitiesgraphs"></a>Připojení odpojit entit nebo grafy
Nové `DbContext.AttachGraph` rozhraní API pomáhá znovu připojit entity do kontextu a uložte nové nebo změněné entity.

## <a name="saving-data"></a>Ukládání dat
### <a name="basic-save-functionality"></a>Základní funkce uložení
Umožňuje změny instance entity, které chcete nastavit jako trvalý, do databáze.
### <a name="optimistic-concurrency"></a>Optimistickou metodu souběžného zpracování
Chrání před přepsání změny provedené jiným uživatelem, protože tabulky se načetla data z databáze.
### <a name="async-savechanges"></a>Asynchronní SaveChanges
Můžete uvolnit aktuální vlákno zpracovat další požadavky při databázi zpracovává příkazy vydané z `SaveChanges`.
### <a name="database-transactions"></a>Databázové transakce
Znamená, že `SaveChanges` je vždy atomic (tj. buď úplně operace proběhne úspěšně, nebo jsou provedeny žádné změny k databázi). Existují také transakce související rozhraní API umožňující sdílení transakce mezi instance kontextu atd.
### <a name="relational-batching-of-statements"></a>Relační: Dávkování příkazy
Poskytuje lepší výkon tím, že dávkování až více příkazů INSERT, UPDATE nebo odstranění do jednoho zpětné transformace do databáze.

## <a name="query"></a>Dotazy
### <a name="basic-linq-support"></a>Základní podpora LINQ
Poskytuje možnost použít LINQ k načtení dat z databáze.
### <a name="mixed-clientserver-evaluation"></a>Vyhodnocení smíšený klient server
Umožňuje dotazy tak, aby obsahovala logiky, která nelze vyhodnotit v databázi a je proto nutné vyhodnotit po načtení dat do paměti.
### <a name="notracking"></a>NoTracking
Dotazy umožňuje rychlejší spouštění dotazů, když kontextu není nutné monitorování změn instancí entit (tj. výsledky jsou jen pro čtení).
### <a name="eager-loading"></a>přes načítání
Poskytuje `Include` a `ThenInclude` metody pro identifikaci související data, která mají přenášet také při dotazování.
### <a name="async-query"></a>Asynchronní dotazu
Můžete uvolnit aktuální vlákno (a související prostředky) zpracovávat další požadavky při databázi zpracuje dotaz.
### <a name="raw-sql-queries"></a>Nezpracovaná dotazy SQL
Poskytuje `DbSet.FromSql` metodu použít nezpracovaná SQL dotazuje načíst data. Tyto dotazy můžete sestavit také na pomocí LINQ.

## <a name="database-schema-management"></a>Správa schématu databáze       
### <a name="database-creationdeletion-apis"></a>Vytvoření nebo odstranění databáze rozhraní API
Jsou většinou navrženy pro testování, kam chcete rychle vytvořit nebo odstranit databázi bez použití migrace.
### <a name="relational-database-migrations"></a>Migrace relační databáze
Povolit schéma relační databáze vyvíjí přesčasová jako změny modelu.
### <a name="reverse-engineer-from-database"></a>Zpětné analýzy z databáze
Scaffold EF model založené na stávajícím schématu relační databáze.

## <a name="database-providers"></a>Zprostředkovatelé databáze
### <a name="sql-server"></a>SQL Server
Připojí k serveru Microsoft SQL Server 2008 a vyšší.
### <a name="sqlite"></a>SQLite
Připojí se k databázi SQLite 3.
### <a name="in-memory"></a>V paměti
Umožňuje snadno bez připojení k databázi skutečné testování.
### <a name="3rd-party-providers"></a>poskytovatelé 3.
Několik poskytovatelů jsou k dispozici pro jiné databázové stroje. V tématu [zprostředkovatelů databáze](../providers/index.md) úplný seznam.
