---
title: Co je nového v EF Core 5,0
author: ajcvickers
ms.date: 01/29/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: e858379cc46abbef999fd32a3685e1d522524889
ms.sourcegitcommit: 89567d08c9d8bf9c33bb55a62f17067094a4065a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/06/2020
ms.locfileid: "77052030"
---
# <a name="whats-new-in-ef-core-50"></a>Co je nového v EF Core 5,0

EF Core 5,0 je aktuálně ve vývoji.
Tato stránka bude obsahovat přehled zajímavých změn představených v každé verzi Preview.
První verze Preview EF Core 5,0 se nezávazně očekává v prvním čtvrtletí 2020.

Tato stránka neduplikuje [plán pro EF Core 5,0](plan.md).
Plán popisuje celkový motiv EF Core 5,0, včetně všeho, co plánujeme zahrnout, před odesláním finální verze.

Odkazy odsud budeme přidávat do oficiální dokumentace, která je publikovaná.

## <a name="preview-1-not-yet-shipped"></a>Preview 1 (ještě Neexpedováno)

### <a name="simple-logging"></a>Jednoduché protokolování

Tato funkce přidá funkce podobné `Database.Log` v EF6.
To znamená, že poskytuje jednoduchý způsob, jak získat protokoly z EF Core, aniž by bylo nutné konfigurovat žádný druh externího protokolovacího rozhraní.

Předběžná dokumentace je součástí [týdenního stavu EF pro 5. prosince 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).

Další dokumentaci sleduje problém [#2085](https://github.com/aspnet/EntityFramework.Docs/issues/2085).

### <a name="simple-way-to-get-generated-sql"></a>Jednoduchý způsob, jak získat vygenerovaný SQL

EF Core 5,0 zavádí metodu rozšíření `ToQueryString`, která vrátí SQL, který EF Core generuje při provádění dotazu LINQ.

Do [9. ledna 2020 se do týdenního stavu EF](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246)zahrnuje předběžná dokumentace.

Další dokumentaci sleduje problém [#1331](https://github.com/aspnet/EntityFramework.Docs/issues/1331).

### <a name="enhanced-debug-views"></a>Rozšířená zobrazení ladění

Zobrazení ladění představují snadný způsob, jak se při ladění problémů podívat na vnitřní EF Core.
Zobrazení ladění pro model bylo implementováno před nějakým časem.
Pro EF Core 5,0 jsme provedli zobrazení modelu pro čtení a přidání nového zobrazení ladění pro sledované entity ve Správci stavu.

Předběžná dokumentace je zahrnutá ve [stavu týden EF pro 12. prosince 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).

Další dokumentaci sleduje problém [#2086](https://github.com/aspnet/EntityFramework.Docs/issues/2086).

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a>V inicializovaných DbContext se dá změnit připojení nebo připojovací řetězec.

Nyní je snazší vytvořit instanci DbContext bez připojení nebo připojovacího řetězce.
Připojení nebo připojovací řetězec se teď dá v instanci kontextu taky připojit.
To umožňuje stejné instanci kontextu dynamicky se připojovat k různým databázím.

Dokumentace je sledována pomocí [#2075](https://github.com/aspnet/EntityFramework.Docs/issues/2075)problému.

### <a name="change-tracking-proxies"></a>Proxy se sledováním změn

EF Core teď můžou generovat proxy a moduly runtime, které automaticky implementují [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) a [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).
Ty pak nahlásí změny hodnot u vlastností entity přímo na EF Core a nemusíte přitom Hledat změny.
Nicméně proxy se dodávají s vlastní sadou omezení, takže nejsou pro všechny.

Dokumentace je sledována pomocí [#2076](https://github.com/aspnet/EntityFramework.Docs/issues/2076)problému.

### <a name="improved-handling-of-database-null-semantics"></a>Vylepšené zpracování sémantiky s hodnotou null databáze

Relační databáze obvykle považují hodnotu NULL za neznámou hodnotu, takže se nerovná žádnému jiné hodnotě NULL.
C#na druhé straně zachází s hodnotou null jako s definovanou hodnotou, která je porovnána s jinou hodnotou null.
EF Core ve výchozím nastavení překládá dotazy tak, aby používaly C# sémantiku null.
EF Core 5,0 významně vylepšuje efektivitu těchto překladů.

Dokumentace je sledována pomocí [#1612](https://github.com/aspnet/EntityFramework.Docs/issues/1612)problému.

### <a name="indexer-properties"></a>Vlastnosti indexeru

EF Core 5,0 podporuje mapování vlastností C# indexeru.
To umožňuje entitám fungovat jako vaky objektů a dat, kde jsou sloupce namapovány na pojmenované vlastnosti v kontejneru.

Dokumentace je sledována pomocí [#2018](https://github.com/aspnet/EntityFramework.Docs/issues/2018)problému.

### <a name="generation-of-check-constraints-for-enum-mappings"></a>Generování omezení CHECK pro mapování výčtu

Migrace EF Core 5,0 teď můžou generovat kontrolní omezení pro mapování vlastností výčtu.
Například:

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN('Useful', 'Useless', 'Unknown'))
```

Dokumentace je sledována pomocí [#2082](https://github.com/aspnet/EntityFramework.Docs/issues/2082)problému.

### <a name="query-translations-for-more-datetime-constructs"></a>Překlady dotazů pro další konstrukce DateTime

Dotazy obsahující nové konstrukce DataTime jsou nyní přeloženy.
Také SQL Server funkce DateDiffWeek je nyní namapována.

Dokumentace je sledována pomocí [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079)problému.

### <a name="query-translations-for-more-byte-array-constructs"></a>Překlady dotazů pro další konstrukce bajtových polí

Dotazy používající vlastnosti Contains, Length, SequenceEqual atd. on Byte [] jsou nyní přeloženy na SQL.

Předběžná dokumentace je součástí [týdenního stavu EF pro 5. prosince 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).

Další dokumentaci sleduje problém [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079).

### <a name="query-translation-for-reverse"></a>Přesměrovat dotaz na zpětný překlad

Dotazy používající `Reverse` jsou nyní přeloženy.
Například:

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

Dokumentace je sledována pomocí [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079)problému.

### <a name="query-translation-for-bitwise-operators"></a>Převod dotazů pro bitové operátory

Dotazy používající bitové operátory jsou nyní přeloženy ve více případech například:

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

Dokumentace je sledována pomocí [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079)problému.

### <a name="query-translation-for-strings-on-cosmos"></a>Překlad dotazů pro řetězce v Cosmos

Dotazy, které používají metody řetězce Contains, StartsWith a EndsWith, jsou nyní přeloženy při použití poskytovatele Azure Cosmos DB.

Dokumentace je sledována pomocí [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079)problému.
