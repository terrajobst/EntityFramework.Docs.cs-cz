---
title: Co je nového v EF Core 5,0
author: ajcvickers
ms.date: 03/15/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: 08a93555fd76d8a9f6d3011f59d9a34f76d0b22f
ms.sourcegitcommit: c3b8386071d64953ee68788ef9d951144881a6ab
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/24/2020
ms.locfileid: "80136247"
---
# <a name="whats-new-in-ef-core-50"></a>Co je nového v EF Core 5,0

EF Core 5,0 je aktuálně ve vývoji.
Tato stránka bude obsahovat přehled zajímavých změn představených v každé verzi Preview.

Tato stránka neduplikuje [plán pro EF Core 5,0](plan.md).
Plán popisuje celkový motiv EF Core 5,0, včetně všeho, co plánujeme zahrnout, před odesláním finální verze.

Odkazy odsud budeme přidávat do oficiální dokumentace, která je publikovaná.

## <a name="preview-1"></a>Ve verzi Preview 1

### <a name="simple-logging"></a>Jednoduché protokolování

Tato funkce přidá funkce podobné `Database.Log` v EF6.
To znamená, že poskytuje jednoduchý způsob, jak získat protokoly z EF Core, aniž by bylo nutné konfigurovat žádný druh externího protokolovacího rozhraní.

Předběžná dokumentace je součástí [týdenního stavu EF pro 5. prosince 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).

Další dokumentaci sleduje problém [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).

### <a name="simple-way-to-get-generated-sql"></a>Jednoduchý způsob, jak získat vygenerovaný SQL

EF Core 5,0 zavádí metodu rozšíření `ToQueryString`, která vrátí SQL, který EF Core generuje při provádění dotazu LINQ.

Do [9. ledna 2020 se do týdenního stavu EF](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246)zahrnuje předběžná dokumentace.

Další dokumentaci sleduje problém [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).

### <a name="use-a-c-attribute-to-indicate-that-an-entity-has-no-key"></a>Označení, C# že entita nemá žádný klíč, pomocí atributu

Typ entity se teď dá nakonfigurovat tak, aby neobsahoval žádný klíč pomocí nového `KeylessAttribute`.
Například:

```CSharp
[Keyless]
public class Address
{
    public string Street { get; set; }
    public string City { get; set; }
    public int Zip { get; set; }
}
```

Dokumentace je sledována pomocí [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186)problému.

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a>V inicializovaných DbContext se dá změnit připojení nebo připojovací řetězec.

Nyní je snazší vytvořit instanci DbContext bez připojení nebo připojovacího řetězce.
Připojení nebo připojovací řetězec se teď dá v instanci kontextu taky připojit.
To umožňuje stejné instanci kontextu dynamicky se připojovat k různým databázím.

Dokumentace je sledována pomocí [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075)problému.

### <a name="change-tracking-proxies"></a>Proxy se sledováním změn

EF Core teď můžou generovat proxy a moduly runtime, které automaticky implementují [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) a [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).
Ty pak nahlásí změny hodnot u vlastností entity přímo na EF Core a nemusíte přitom Hledat změny.
Nicméně proxy se dodávají s vlastní sadou omezení, takže nejsou pro všechny.

Dokumentace je sledována pomocí [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076)problému.

### <a name="enhanced-debug-views"></a>Rozšířená zobrazení ladění

Zobrazení ladění představují snadný způsob, jak se při ladění problémů podívat na vnitřní EF Core.
Zobrazení ladění pro model bylo implementováno před nějakým časem.
Pro EF Core 5,0 jsme provedli zobrazení modelu pro čtení a přidání nového zobrazení ladění pro sledované entity ve Správci stavu.

Předběžná dokumentace je zahrnutá ve [stavu týden EF pro 12. prosince 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).

Další dokumentaci sleduje problém [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).

### <a name="improved-handling-of-database-null-semantics"></a>Vylepšené zpracování sémantiky s hodnotou null databáze

Relační databáze obvykle považují hodnotu NULL za neznámou hodnotu, takže se nerovná žádnému jiné hodnotě NULL.
C#na druhé straně zachází s hodnotou null jako s definovanou hodnotou, která je porovnána s jinou hodnotou null.
EF Core ve výchozím nastavení překládá dotazy tak, aby používaly C# sémantiku null.
EF Core 5,0 významně vylepšuje efektivitu těchto překladů.

Dokumentace je sledována pomocí [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612)problému.

### <a name="indexer-properties"></a>Vlastnosti indexeru

EF Core 5,0 podporuje mapování vlastností C# indexeru.
To umožňuje entitám fungovat jako vaky objektů a dat, kde jsou sloupce namapovány na pojmenované vlastnosti v kontejneru.

Dokumentace je sledována pomocí [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018)problému.

### <a name="generation-of-check-constraints-for-enum-mappings"></a>Generování omezení CHECK pro mapování výčtu

Migrace EF Core 5,0 teď můžou generovat kontrolní omezení pro mapování vlastností výčtu.
Například:

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN('Useful', 'Useless', 'Unknown'))
```

Dokumentace je sledována pomocí [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082)problému.

### <a name="isrelational"></a>Relace

Kromě stávajících `IsSqlServer`, `IsSqlite`a `IsInMemory`byla přidána nová `IsRelational` metoda.
To se dá použít k otestování, jestli DbContext používá jakéhokoli poskytovatele relačních databází.
Například:

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    if (Database.IsRelational())
    {
        // Do relational-specific model configuration.
    }
}
```

Dokumentace je sledována pomocí [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185)problému.

### <a name="cosmos-optimistic-concurrency-with-etags"></a>Cosmos Optimistická souběžnost se značkami ETag

Zprostředkovatel databáze Azure Cosmos DB nyní podporuje optimistickou souběžnost pomocí značek ETag.
Pomocí Tvůrce modelů v OnModelCreating nakonfigurujte ETag:

```CSharp
builder.Entity<Customer>().Property(c => c.ETag).IsEtagConcurrency();
```

SaveChanges pak vygeneruje `DbUpdateConcurrencyException` v konfliktu souběžnosti, který [může být zpracován](https://docs.microsoft.com/ef/core/saving/concurrency) pro implementaci opakování, atd.


Dokumentace je sledována pomocí [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099)problému.

### <a name="query-translations-for-more-datetime-constructs"></a>Překlady dotazů pro další konstrukce DateTime

Dotazy obsahující nové konstrukce DateTime jsou nyní přeloženy.

Kromě toho jsou nyní namapovány následující funkce SQL Server:
* DateDiffWeek
* DateFromParts

Například:

```CSharp
var count = context.Orders.Count(c => date > EF.Functions.DateFromParts(DateTime.Now.Year, 12, 25));

```

Dokumentace je sledována pomocí [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)problému.

### <a name="query-translations-for-more-byte-array-constructs"></a>Překlady dotazů pro další konstrukce bajtových polí

Dotazy používající vlastnosti Contains, Length, SequenceEqual atd. on Byte [] jsou nyní přeloženy na SQL.

Předběžná dokumentace je součástí [týdenního stavu EF pro 5. prosince 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).

Další dokumentaci sleduje problém [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).

### <a name="query-translation-for-reverse"></a>Přesměrovat dotaz na zpětný překlad

Dotazy používající `Reverse` jsou nyní přeloženy.
Například:

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

Dokumentace je sledována pomocí [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)problému.

### <a name="query-translation-for-bitwise-operators"></a>Převod dotazů pro bitové operátory

Dotazy používající bitové operátory jsou nyní přeloženy ve více případech například:

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

Dokumentace je sledována pomocí [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)problému.

### <a name="query-translation-for-strings-on-cosmos"></a>Překlad dotazů pro řetězce v Cosmos

Dotazy, které používají metody řetězce Contains, StartsWith a EndsWith, jsou nyní přeloženy při použití poskytovatele Azure Cosmos DB.

Dokumentace je sledována pomocí [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)problému.
