---
title: Co je nového v EF Core 1,0-EF Core
author: divega
ms.date: 10/27/2016
ms.assetid: 20A25111-AEBE-4BC2-83A5-3F651952DF72
uid: core/what-is-new/ef-core-1.0
ms.openlocfilehash: 2cd2a54d75ed3f0caa8b674dfb56babcfcc13592
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417522"
---
# <a name="features-included-in-ef-core-10"></a>Funkce, které jsou součástí EF Core 1,0

## <a name="platforms"></a>Platformy

### <a name="net-framework-451"></a>.NET Framework 4.5.1

Zahrnuje konzolu, WPF, WinForms, ASP.NET 4 atd.

### <a name="net-standard-13"></a>.NET Standard 1,3

Včetně ASP.NET Core cílících na .NET Framework a .NET Core ve Windows, OSX a Linux.

## <a name="modelling"></a>Modelovací

### <a name="basic-modelling"></a>Základní modelování

Založené na POCO entitách pomocí vlastností get/set běžných skalárních typů (`int`, `string`atd.).

### <a name="relationships-and-navigation-properties"></a>Relace a navigační vlastnosti

V modelu založeném na cizím klíči lze určit relace 1: n a 1:1 nebo jedna-nula. K těmto relacím se dají přidružit navigační vlastnosti jednoduchých typů nebo odkazů.

### <a name="built-in-conventions"></a>Předdefinované konvence

Tyto sestavují počáteční model na základě tvaru tříd entit.

### <a name="fluent-api"></a>Rozhraní Fluent API

Umožňuje přepsat metodu `OnModelCreating` v kontextu a dále konfigurovat model, který byl zjištěn podle konvence.

### <a name="data-annotations"></a>Datové poznámky

Jsou atributy, které mohou být přidány do tříd nebo vlastností entit a budou mít vliv na model EF. Přidání `[Required]` například umožní, aby EF věděl, že je požadovaná vlastnost.

### <a name="relational-table-mapping"></a>Mapování relační tabulky

Umožňuje mapovat entity na tabulky nebo sloupce.

### <a name="key-value-generation"></a>Generování hodnoty klíče

Včetně generování a generování databáze na straně klienta.

### <a name="database-generated-values"></a>Databáze vygenerovaly hodnoty

Umožňuje generovat hodnoty z databáze při vložení (výchozí hodnoty) nebo aktualizovat (počítané sloupce).

### <a name="sequences-in-sql-server"></a>Sekvence v SQL Server

Umožňuje definovat objekty sekvence v modelu.

### <a name="unique-constraints"></a>Jedinečná omezení

Umožňuje definování alternativních klíčů a možnosti definovat vztahy, které cílí na tento klíč.

### <a name="indexes"></a>Indexy

Definováním indexů v modelu automaticky zavádí indexy v databázi. Podporují se taky jedinečné indexy.

### <a name="shadow-state-properties"></a>Vlastnosti stavu stínu

Umožňuje definovat vlastnosti v modelu, který není deklarovaný a který není uložený ve třídě .NET, ale dá se sledovat a aktualizovat pomocí EF Core. Běžně používané pro vlastnosti cizích klíčů při vystavování těchto objektů v objektu není žádoucí.

### <a name="table-per-hierarchy-inheritance-pattern"></a>Vzor dědičnosti tabulky na hierarchii

Umožňuje, aby se entity v hierarchii dědičnosti ukládaly do jedné tabulky pomocí sloupce diskriminátoru, které identifikují typ entity pro daný záznam v databázi.

### <a name="model-validation"></a>Ověření modelu

Detekuje neplatné vzory v modelu a poskytuje užitečné chybové zprávy.

## <a name="change-tracking"></a>Sledování změn

### <a name="snapshot-change-tracking"></a>Sledování změn snímků

Umožňuje, aby se změny v entitách zjistily automaticky porovnáním aktuálního stavu s kopií (snímkem) původního stavu.

### <a name="notification-change-tracking"></a>Sledování změn oznámení

Umožňuje entitám upozorňování sledování změn při změně hodnot vlastností.

### <a name="accessing-tracked-state"></a>Přístup ke sledovanému stavu

Prostřednictvím `DbContext.Entry` a `DbContext.ChangeTracker`.

### <a name="attaching-detached-entitiesgraphs"></a>Připojení odpojených entit a grafů

Nové rozhraní `DbContext.AttachGraph` API pomáhá znovu připojovat entity k kontextu, aby bylo možné ukládat nové a upravené entity.

## <a name="saving-data"></a>Ukládání dat

### <a name="basic-save-functionality"></a>Základní funkce ukládání

Umožňuje, aby se změny instancí entity zachovaly v databázi.

### <a name="optimistic-concurrency"></a>Optimistická metoda souběžného zpracování

Chrání před přepsáním změn provedených jiným uživatelem od načtení dat z databáze.

### <a name="async-savechanges"></a>Asynchronní metody SaveChanges

Může uvolnit aktuální vlákno pro zpracování dalších požadavků, zatímco databáze zpracovává příkazy vydané z `SaveChanges`.

### <a name="database-transactions"></a>Transakce databáze

Znamená, že `SaveChanges` je vždy atomická (to znamená, že buď zcela uspěje, nebo nejsou provedeny žádné změny v databázi). K dispozici jsou také rozhraní API související s transakcemi umožňující sdílení transakcí mezi instancemi kontextu atd.

### <a name="relational-batching-of-statements"></a>Relační: dávkování příkazů

Poskytuje lepší výkon tím, že dávkuje více příkazů pro vložení, aktualizaci a odstranění do jediného převodu do databáze.

## <a name="query"></a>Dotaz

### <a name="basic-linq-support"></a>Základní podpora LINQ

Poskytuje možnost použít LINQ k načtení dat z databáze.

### <a name="mixed-clientserver-evaluation"></a>Smíšené vyhodnocení klientů/serverů

Umožňuje dotazům obsahovat logiku, která se v databázi nedá vyhodnotit, a po načtení dat do paměti je proto nutné vyhodnotit.

### <a name="notracking"></a>NoTracking

Dotazy umožňují rychlejší provádění dotazů, pokud kontext nemusí sledovat změny instancí entit (to je užitečné, pokud jsou výsledky jen pro čtení).

### <a name="eager-loading"></a>Eager načítání

Poskytuje metody `Include` a `ThenInclude` k identifikaci souvisejících dat, která by se měla načíst také při dotazování.

### <a name="async-query"></a>Asynchronní dotaz

Může uvolnit aktuální vlákno (a přidružené prostředky) pro zpracování dalších požadavků, zatímco databáze zpracovává dotaz.

### <a name="raw-sql-queries"></a>Nezpracované dotazy SQL

Poskytuje metodu `DbSet.FromSql` pro použití nezpracovaných dotazů SQL k načtení dat. Tyto dotazy mohou být také vytvořeny pomocí LINQ.

## <a name="database-schema-management"></a>Správa schématu databáze

### <a name="database-creationdeletion-apis"></a>Rozhraní API pro vytváření a odstraňování databází

Jsou převážně navržené pro testování, kde chcete rychle vytvořit nebo odstranit databázi bez použití migrace.

### <a name="relational-database-migrations"></a>Migrace relačních databází

Povolí schématu relační databáze vyvíjet přesčasovou práci při změnách modelu.

### <a name="reverse-engineer-from-database"></a>Zpětná analýza z databáze

Generuje základní generátory modelu EF na základě stávajícího schématu relační databáze.

## <a name="database-providers"></a>Poskytovatelé databází

### <a name="sql-server"></a>SQL Server

Připojí se k Microsoft SQL Server 2008 a vyšší.

### <a name="sqlite"></a>SQLite

Připojí se k databázi SQLite 3.

### <a name="in-memory"></a>V paměti

Je navržený tak, aby se testování snadno povolilo bez připojení ke skutečné databázi.

### <a name="3rd-party-providers"></a>poskytovatelé třetích stran

K dispozici je několik poskytovatelů pro jiné databázové moduly. Úplný seznam najdete v tématu [poskytovatelé databáze](../providers/index.md) .
