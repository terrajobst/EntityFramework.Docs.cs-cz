---
title: Co je nového v EF Core 1.0 - EF Core
author: divega
ms.date: 10/27/2016
ms.assetid: 20A25111-AEBE-4BC2-83A5-3F651952DF72
uid: core/what-is-new/ef-core-1.0
ms.openlocfilehash: 2cd2a54d75ed3f0caa8b674dfb56babcfcc13592
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417522"
---
# <a name="features-included-in-ef-core-10"></a>Funkce obsažené v EF Core 1.0

## <a name="platforms"></a>Platformy

### <a name="net-framework-451"></a>.NET Framework 4.5.1

Zahrnuje konzole, WPF, WinForms, ASP.NET 4, atd.

### <a name="net-standard-13"></a>.NET Standard 1.3

Včetně ASP.NET Core cílení na rozhraní .NET Framework a .NET Core v systémech Windows, OSX a Linux.

## <a name="modelling"></a>Modelování

### <a name="basic-modelling"></a>Základní modelování

Na základě entit POCO s vlastnostmi get/set běžných skalárních typů (`int`, `string`, atd.).

### <a name="relationships-and-navigation-properties"></a>Vztahy a navigační vlastnosti

Vztahy 1:N a 1:0 nebo 1 lze zadat v modelu na základě cizího klíče. Navigační vlastnosti jednoduché kolekce nebo typy odkazů mohou být přidruženy k těmto vztahům.

### <a name="built-in-conventions"></a>Vestavěné konvence

Tyto sestavení počáteční model založený na tvaru třídy entity.

### <a name="fluent-api"></a>Fluent API

Umožňuje přepsat metodu `OnModelCreating` v kontextu a dále nakonfigurovat model, který byl zjištěn konvencí.

### <a name="data-annotations"></a>Datové poznámky

Jsou atributy, které lze přidat do třídy/vlastnosti entity a bude mít vliv na model EF. Například přidání `[Required]` umožní EF vědět, že je požadováno vlastnost.

### <a name="relational-table-mapping"></a>Mapování relační chodnících

Umožňuje mapování entit na tabulky nebo sloupce.

### <a name="key-value-generation"></a>Generování hodnoty klíče

Včetně generování na straně klienta a generování databáze.

### <a name="database-generated-values"></a>Databáze generované hodnoty

Umožňuje generovat hodnoty v databázi při vložení (výchozí hodnoty) nebo aktualizaci (vypočítané sloupce).

### <a name="sequences-in-sql-server"></a>Sekvence na serveru SQL Server

Umožňuje definovat objekty sekvence v modelu.

### <a name="unique-constraints"></a>Jedinečná omezení

Umožňuje definici alternativních klíčů a možnost definovat vztahy, které cílí na tento klíč.

### <a name="indexes"></a>Indexy

Definování indexů v modelu automaticky zavádí indexy v databázi. Jedinečné indexy jsou také podporovány.

### <a name="shadow-state-properties"></a>Vlastnosti stavu stínů

Umožňuje vlastnosti, které mají být definovány v modelu, které nejsou deklarovány a nejsou uloženy ve třídě .NET, ale mohou být sledovány a aktualizovány EF Core. Běžně používané pro vlastnosti cizího klíče při jejich vystavení v objektu není žádoucí.

### <a name="table-per-hierarchy-inheritance-pattern"></a>Vzor dědičnosti podle hierarchie

Umožňuje entity v hierarchii dědičnosti, které mají být uloženy do jedné tabulky pomocí sloupce diskriminátoru k identifikaci typu entity pro daný záznam v databázi.

### <a name="model-validation"></a>Ověření modelu

Detekuje neplatné vzory v modelu a poskytuje užitečné chybové zprávy.

## <a name="change-tracking"></a>Sledování změn

### <a name="snapshot-change-tracking"></a>Sledování změn snímku

Umožňuje automaticky zjistit změny v entitách porovnáním aktuálního stavu s kopií (snímek) původního stavu.

### <a name="notification-change-tracking"></a>Sledování změn oznámení

Umožňuje entitám upozornit sledování změn při změně hodnot vlastností.

### <a name="accessing-tracked-state"></a>Přístup ke sledovanému stavu

Via `DbContext.Entry` `DbContext.ChangeTracker`a .

### <a name="attaching-detached-entitiesgraphs"></a>Připojení odpojených entit/grafů

Nové `DbContext.AttachGraph` rozhraní API pomáhá znovu připojit entity k kontextu za účelem uložení nových/změněných entit.

## <a name="saving-data"></a>Ukládání dat

### <a name="basic-save-functionality"></a>Základní funkce ukládání

Umožňuje zachovat změny instancí entit v databázi.

### <a name="optimistic-concurrency"></a>Optimistická metoda souběžného zpracování

Chrání před přepsáním změny provedené jiným uživatelem od data byla načtena z databáze.

### <a name="async-savechanges"></a>Asynchronní savechanges

Může uvolnit aktuální vlákno pro zpracování dalších požadavků, zatímco databáze `SaveChanges`zpracovává příkazy vydané z .

### <a name="database-transactions"></a>Databázové transakce

Prostředky, `SaveChanges` které je vždy atomické (což znamená, že buď zcela úspěšné, nebo žádné změny jsou provedeny v databázi). Existují také možnosti API související s transakcemi, které umožňují sdílení transakcí mezi instancemi kontextu atd.

### <a name="relational-batching-of-statements"></a>Relační: Dávkování výpisů

Poskytuje lepší výkon dávkováním více příkazů INSERT/UPDATE/DELETE do jednoho roundtripu do databáze.

## <a name="query"></a>Dotaz

### <a name="basic-linq-support"></a>Základní podpora LINQ

Poskytuje možnost použít LINQ k načtení dat z databáze.

### <a name="mixed-clientserver-evaluation"></a>Smíšené vyhodnocení klienta/serveru

Umožňuje dotazy obsahovat logiku, která nelze vyhodnotit v databázi, a proto musí být vyhodnocena po načtení dat do paměti.

### <a name="notracking"></a>NoTracking

Dotazy umožňuje rychlejší spuštění dotazu, když kontext není nutné sledovat změny instance entity (to je užitečné, pokud jsou výsledky jen pro čtení).

### <a name="eager-loading"></a>Dychtivé načítání

Poskytuje `Include` a `ThenInclude` metody k identifikaci souvisejících dat, které by měly být také načteny při dotazování.

### <a name="async-query"></a>Asynchronní dotaz

Můžete uvolnit aktuální vlákno (a je přidružené prostředky) ke zpracování dalších požadavků, zatímco databáze zpracovává dotaz.

### <a name="raw-sql-queries"></a>Nezpracované dotazy SQL

Poskytuje `DbSet.FromSql` metodu pro použití nezpracovaných dotazů SQL k načtení dat. Tyto dotazy lze také skládá při použití LINQ.

## <a name="database-schema-management"></a>Správa schématu databáze

### <a name="database-creationdeletion-apis"></a>Vytváření a odstraňování api pro vytváření a odstraňování databáze

Jsou většinou určeny pro testování, kde chcete rychle vytvořit nebo odstranit databázi bez použití migrace.

### <a name="relational-database-migrations"></a>Migrace relačních databází

Povolte schéma relační databáze vyvíjet přesčas jako váš model změny.

### <a name="reverse-engineer-from-database"></a>Zpětná inženýrská verze z databáze

Ve spouštění systému souborů EF modelu na základě existující schéma relační databáze.

## <a name="database-providers"></a>Poskytovatelé databází

### <a name="sql-server"></a>SQL Server

Připojí se k serveru Microsoft SQL Server 2008 a dále.

### <a name="sqlite"></a>SQLite

Připojuje se k databázi SQLite 3.

### <a name="in-memory"></a>V paměti

Je navržen tak, aby snadno povolit testování bez připojení k reálné databázi.

### <a name="3rd-party-providers"></a>Poskytovatelé třetích stran

Několik zprostředkovatelů jsou k dispozici pro jiné databázové stroje. Úplný seznam naleznete v tématu [Zprostředkovatelé databáze.](../providers/index.md)
