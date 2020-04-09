---
title: Co je nového v EF Core 5.0
description: Přehled nových funkcí v EF Core 5.0
author: ajcvickers
ms.date: 03/30/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: c047a308cadf44eea577dcab29b68b36942a50df
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "80634271"
---
# <a name="whats-new-in-ef-core-50"></a>Co je nového v EF Core 5.0

EF Core 5.0 je v současné době ve vývoji.
Tato stránka bude obsahovat přehled zajímavých změn zavedených v každém náhledu.

Tato stránka neduplikuje [plán ef core 5.0](plan.md).
Plán popisuje celková témata pro EF Core 5.0, včetně všeho, co plánujeme zahrnout před odesláním konečné verze.

Odkazy zde přidáme k oficiální dokumentaci tak, jak je zveřejněna.

## <a name="preview-2"></a>Preview 2

### <a name="use-a-c-attribute-to-specify-a-property-backing-field"></a>Použití atributu C# k určení pole pro podporu vlastností

Atribut C# lze nyní použít k určení záložního pole pro vlastnost.
Tento atribut umožňuje EF Core stále zapisovat a číst z doprovodného pole, jak by normálně dojít, i v případě, že záložní pole nelze najít automaticky.
Příklad:

```CSharp
public class Blog
{
    private string _mainTitle;

    public int Id { get; set; }

    [BackingField(nameof(_mainTitle))]
    public string Title
    {
        get => _mainTitle;
        set => _mainTitle = value;
    }
}
```

Dokumentace je sledována podle [#2230](https://github.com/dotnet/EntityFramework.Docs/issues/2230).

### <a name="complete-discriminator-mapping"></a>Kompletní mapování diskriminátoru

EF Core používá diskriminátor sloupec pro [mapování TPH hierarchie dědičnosti](/ef/core/modeling/inheritance).
Některá vylepšení výkonu jsou možná, pokud EF Core zná všechny možné hodnoty diskriminátoru.
EF Core 5.0 nyní implementuje tato vylepšení.

Například předchozí verze EF Core by vždy generovat tento SQL pro dotaz vrácení všech typů v hierarchii:

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
WHERE [a].[Discriminator] IN (N'Animal', N'Cat', N'Dog', N'Human')
```

Ef Core 5.0 nyní vygeneruje následující, když je nakonfigurováno úplné mapování diskriminátoru:

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
```

Bude to výchozí chování začínající náhledem 3.

### <a name="performance-improvements-in-microsoftdatasqlite"></a>Vylepšení výkonu v Microsoft.Data.Sqlite

Provedli jsme dvě vylepšení výkonu pro SQLIte:

* Načítání binárních a řetězcových dat pomocí GetBytes, GetChars a GetTextReader je nyní efektivnější využitím sqliteblob a datových proudů.
* Inicializace SqliteConnection je nyní opožděná.

Tato vylepšení jsou v ADO.NET microsoft.Data.Sqlite zprostředkovatele a tím také zlepšit výkon mimo EF Core.

## <a name="preview-1"></a>Náhled 1

### <a name="simple-logging"></a>Jednoduché protokolování

Tato funkce přidává funkce `Database.Log` podobné v EF6.
To znamená, že poskytuje jednoduchý způsob, jak získat protokoly z EF Core bez nutnosti konfigurovat jakýkoli druh architektury externíprotokolování.

Předběžná dokumentace je zahrnuta do [stavu EF týdně pro prosinec 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).

Další dokumentace je sledována podle [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).

### <a name="simple-way-to-get-generated-sql"></a>Jednoduchý způsob, jak získat generované SQL

EF Core 5.0 `ToQueryString` zavádí metodu rozšíření, která vrátí SQL, které EF Core vygeneruje při provádění dotazu LINQ.

Předběžná dokumentace je zahrnuta do [stavu EF týdně pro leden 9, 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).

Další dokumentace je sledována podle [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).

### <a name="use-a-c-attribute-to-indicate-that-an-entity-has-no-key"></a>Použití atributu C# k označení, že entita nemá žádný klíč

Typ entity lze nyní nakonfigurovat tak, `KeylessAttribute`aby pomocí nového .
Příklad:

```CSharp
[Keyless]
public class Address
{
    public string Street { get; set; }
    public string City { get; set; }
    public int Zip { get; set; }
}
```

Dokumentace je sledována podle [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186).

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a>Připojení nebo připojovací řetězec lze změnit na inicializovaném dbcontext

Nyní je jednodušší vytvořit instanci DbContext bez připojení nebo připojovacířetězec.
Také připojení nebo připojovací řetězec lze nyní zmutovat na instanci kontextu.
Tato funkce umožňuje stejnou instanci kontextu dynamicky se připojit k různým databázím.

Dokumentace je sledována podle [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).

### <a name="change-tracking-proxies"></a>Proxy servery pro sledování změn

EF Core nyní můžete generovat runtime proxy servery, které automaticky implementují [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) a [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).
Ty pak sestavy změny hodnoty vlastností entity přímo EF Core, vyhnout se nutnosti hledat změny.
Nicméně, proxy přijít s vlastní sadou omezení, takže nejsou pro každého.

Dokumentace je sledována podle [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).

### <a name="enhanced-debug-views"></a>Vylepšená zobrazení ladění

Ladicí zobrazení jsou snadný způsob, jak se podívat na vnitřní ef jádra při ladění problémy.
Před časem bylo implementováno zobrazení ladění modelu.
Pro EF Core 5.0 jsme usnadnili čtení zobrazení modelu a přidali jsme nové zobrazení ladění pro sledované entity ve správci stavu.

Předběžná dokumentace je zahrnuta do [stavu EF týdně pro prosinec 12, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).

Další dokumentace je sledována podle [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).

### <a name="improved-handling-of-database-null-semantics"></a>Vylepšené zpracování sémantiky null databáze

Relační databáze obvykle považují hodnotu NULL za neznámou hodnotu, a proto se nerovnají žádné jiné hodnotě NULL.
Zatímco C# zachází null jako definovanou hodnotu, která porovnává rovná se jakékoli jiné null.
EF Core ve výchozím nastavení překládá dotazy tak, aby používaly sémantiku C# null.
EF Core 5.0 výrazně zvyšuje efektivitu těchto překladů.

Dokumentace je sledována podle [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).

### <a name="indexer-properties"></a>Vlastnosti indexeru

EF Core 5.0 podporuje mapování vlastností indexeru Jazyka C#.
Tyto vlastnosti umožňují entity působit jako vlastnost tašky, kde jsou sloupce mapovány na pojmenované vlastnosti v vaku.

Dokumentace je sledována podle [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).

### <a name="generation-of-check-constraints-for-enum-mappings"></a>Generování kontrolních omezení pro mapování výčtu

Ef Core 5.0 Migrace nyní můžete generovat CHECK omezení pro mapování vlastností výčtu.
Příklad:

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN ('Useful', 'Useless', 'Unknown'))
```

Dokumentace je sledována podle [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).

### <a name="isrelational"></a>JeRelational

Kromě `IsRelational` stávající `IsSqlServer`metody , `IsSqlite`a `IsInMemory`.
Tuto metodu lze použít k testování, pokud DbContext používá libovolný zprostředkovatel relační databáze.
Příklad:

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    if (Database.IsRelational())
    {
        // Do relational-specific model configuration.
    }
}
```

Dokumentace je sledována podle [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185).

### <a name="cosmos-optimistic-concurrency-with-etags"></a>Cosmos optimistická souběžnost s ETags

Poskytovatel databáze Azure Cosmos DB teď podporuje optimistickou souběžnost pomocí etags.
Ke konfiguraci eTagu použijte tvůrce modelů v aplikaci OnModelCreating:

```CSharp
builder.Entity<Customer>().Property(c => c.ETag).IsEtagConcurrency();
```

SaveChanges pak vyvolá `DbUpdateConcurrencyException` konflikt souběžnosti, který [může být zpracován](https://docs.microsoft.com/ef/core/saving/concurrency) k implementaci opakování atd.

Dokumentace je sledována podle [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).

### <a name="query-translations-for-more-datetime-constructs"></a>Překlady dotazů pro další konstrukce DateTime

Dotazy obsahující novou konstrukci DateTime jsou nyní přeloženy.

Kromě toho jsou nyní mapovány následující funkce serveru SQL Server:

* DatumDiffWeek
* DateFromParts

Příklad:

```CSharp
var count = context.Orders.Count(c => date > EF.Functions.DateFromParts(DateTime.Now.Year, 12, 25));

```

Dokumentace je sledována podle [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).

### <a name="query-translations-for-more-byte-array-constructs"></a>Překlady dotazů pro více konstrukcí bajtového pole

Dotazy pomocí contains, length, sequenceequal atd.

Předběžná dokumentace je zahrnuta do [stavu EF týdně pro prosinec 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).

Další dokumentace je sledována podle [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).

### <a name="query-translation-for-reverse"></a>Překlad dotazu pro reverzní

Dotazy pomocí `Reverse` jsou nyní přeloženy.
Příklad:

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

Dokumentace je sledována podle [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).

### <a name="query-translation-for-bitwise-operators"></a>Překlad dotazu pro bitové operátory

Dotazy pomocí bitových operátorů jsou nyní přeloženy ve více případech Například:

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

Dokumentace je sledována podle [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).

### <a name="query-translation-for-strings-on-cosmos"></a>Překlad dotazu pro řetězce v Cosmos

Dotazy, které používají metody řetězce Contains, StartsWith a EndsWith, jsou nyní přeloženy při použití zprostředkovatele Azure Cosmos DB.

Dokumentace je sledována podle [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).
