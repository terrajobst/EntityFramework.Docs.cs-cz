---
title: Co je nového v EF Core 2,1-EF Core
author: divega
ms.date: 02/20/2018
ms.assetid: 585F90A3-4D5A-4DD1-92D8-5243B14E0FEC
uid: core/what-is-new/ef-core-2.1
ms.openlocfilehash: 5f97015f0228387574e3a19fb20cae1bdb403410
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149171"
---
# <a name="new-features-in-ef-core-21"></a>Nové funkce v EF Core 2,1

Kromě množství oprav chyb a malých funkčních a výkonných vylepšení EF Core 2,1 obsahuje i některé přesvědčivé nové funkce:

## <a name="lazy-loading"></a>opožděné načítání
EF Core nyní obsahuje potřebné stavební bloky pro kohokoli, aby mohli vytvářet třídy entit, které mohou načíst své navigační vlastnosti na vyžádání. Vytvořili jsme také nový balíček Microsoft. EntityFrameworkCore. proxy, který využívá tyto stavební bloky k vytvoření opožděného načítání tříd proxy na základě minimálních upravených tříd entit (například třídy s vlastnostmi virtuální navigace).

Další informace o tomto tématu najdete v [části o opožděném načítání](xref:core/querying/related-data#lazy-loading) .

## <a name="parameters-in-entity-constructors"></a>Parametry v konstruktorech entit
Jako jeden z požadovaných stavebních bloků pro opožděné načítání jsme povolili vytváření entit, které přijímají parametry ve svých konstruktorech. Parametry můžete použít k vložení hodnot vlastností, delegátů opožděného načítání a služeb.

Další informace o tomto tématu najdete [v části v konstruktoru entity s parametry](xref:core/modeling/constructors) .

## <a name="value-conversions"></a>Převody hodnot
Až do této chvíle EF Core možné mapovat vlastnosti typů nativně podporovaných příslušným zprostředkovatelem databáze. Hodnoty byly zkopírovány zpátky mezi sloupci a vlastnostmi bez jakékoli transformace. Počínaje EF Core 2,1 je možné použít převody hodnot k transformaci hodnot získaných ze sloupců předtím, než se použijí na vlastnosti, a naopak. Máme spoustu převodů, které může podle potřeby použít konvence, a také explicitní rozhraní API pro konfiguraci, které umožňuje registraci vlastních převodů mezi sloupci a vlastnostmi. Některé aplikace této funkce jsou:

- Ukládání výčtů jako řetězců
- Mapování celých čísel bez znaménka pomocí SQL Server
- Automatické šifrování a dešifrování hodnot vlastností

Další informace o tomto tématu najdete [v části věnované převodům hodnot](xref:core/modeling/value-conversions) .  

## <a name="linq-groupby-translation"></a>Překlad LINQ GroupBy
Před verzí 2,1 je v EF Core operátor GroupBy LINQ vždy vyhodnocen v paměti. Nyní podporujeme překlad do klauzule SQL GROUP BY ve většině běžných případů.

Tento příklad ukazuje dotaz pomocí GroupBy, který slouží k výpočtu různých agregačních funkcí:

``` csharp
var query = context.Orders
    .GroupBy(o => new { o.CustomerId, o.EmployeeId })
    .Select(g => new
        {
          g.Key.CustomerId,
          g.Key.EmployeeId,
          Sum = g.Sum(o => o.Amount),
          Min = g.Min(o => o.Amount),
          Max = g.Max(o => o.Amount),
          Avg = g.Average(o => o.Amount)
        });
```

Odpovídající překlad SQL vypadá takto:

``` SQL
SELECT [o].[CustomerId], [o].[EmployeeId],
    SUM([o].[Amount]), MIN([o].[Amount]), MAX([o].[Amount]), AVG([o].[Amount])
FROM [Orders] AS [o]
GROUP BY [o].[CustomerId], [o].[EmployeeId];
```

## <a name="data-seeding"></a>Osazení dat
V nové verzi bude možné zadat počáteční data k naplnění databáze. Na rozdíl od EF6 jsou data osazení přidružena k typu entity jako součást konfigurace modelu. Pak EF Core migrace může automaticky vypočítat, které operace vložení, aktualizace nebo odstranění se musí použít při upgradu databáze na novou verzi modelu.

Jako příklad můžete použít ke konfiguraci počátečních dat pro příspěvek v `OnModelCreating`:

``` csharp
modelBuilder.Entity<Post>().HasData(new Post{ Id = 1, Text = "Hello World!" });
```

Další informace o tomto tématu najdete v [části popisující osazení dat](xref:core/modeling/data-seeding) .  

## <a name="query-types"></a>Typy dotazů
EF Core model teď může zahrnovat typy dotazů. Na rozdíl od typů entit nejsou v typech dotazů definovány klíče a nelze je vkládat, odstraňovat ani aktualizovat (to znamená, že jsou jen pro čtení), ale mohou být vráceny přímo pomocí dotazů. Mezi scénáře použití pro typy dotazů patří:

- Mapování na zobrazení bez primárních klíčů
- Mapování na tabulky bez primárních klíčů
- Mapování na dotazy definované v modelu
- Obsluha jako návratový typ pro `FromSql()` dotazy

Další informace o tomto tématu najdete [v části o typech dotazů](xref:core/modeling/keyless-entity-types) .

## <a name="include-for-derived-types"></a>Zahrnout pro odvozené typy
Při zápisu výrazů pro `Include` metodu nyní bude možné zadat navigační vlastnosti, které jsou definovány pro odvozené typy. Pro verzi `Include`silného typu podporujeme buď použití explicitního přetypování, `as` nebo operátoru. Nyní podporujeme odkazy na názvy navigačních vlastností definovaných u odvozených typů v řetězcové verzi `Include`:

``` csharp
var option1 = context.People.Include(p => ((Student)p).School);
var option2 = context.People.Include(p => (p as Student).School);
var option3 = context.People.Include("School");
```

Další informace o tomto tématu najdete v [části Zahrnutí s odvozenými typy](xref:core/querying/related-data#include-on-derived-types) .

## <a name="systemtransactions-support"></a>Podpora System. Transactions
Přidali jsme možnost pracovat s funkcemi System. Transactions, jako je například objekt TransactionScope. Tato akce bude fungovat jak pro .NET Framework, tak pro .NET Core při použití poskytovatelů databáze, kteří ho podporují.

Další informace o tomto tématu najdete [v části na stránce System. Transactions](xref:core/saving/transactions#using-systemtransactions) .

## <a name="better-column-ordering-in-initial-migration"></a>Lepší řazení sloupců při počáteční migraci
Na základě zpětné vazby od zákazníků jsme aktualizovali migrace na počáteční generování sloupců pro tabulky ve stejném pořadí jako vlastnosti deklarované ve třídách. Všimněte si, že EF Core nemůže změnit pořadí při přidání nových členů po počátečním vytvoření tabulky.

## <a name="optimization-of-correlated-subqueries"></a>Optimalizace korelačních poddotazů
Vylepšili jsme naše překlady dotazů tak, aby nedocházelo k provádění dotazů SQL N + 1 v mnoha běžných scénářích, ve kterých použití navigační vlastnosti v projekci vede k propojení dat z kořenového dotazu s daty z korelačního poddotazu. Optimalizace vyžaduje ukládání výsledků z poddotazu do vyrovnávací paměti, a vyžadujeme, abyste dotaz upravili, aby se zavedlo nové chování.

Příklad: následující dotaz normálně je přeložen do jednoho dotazu pro zákazníky, plus N (kde "N" je počet vrácených zákazníků) samostatné dotazy pro objednávky:

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount));
```

Zahrnutím `ToList()` na správné místo označíte, že ukládání do vyrovnávací paměti je vhodné pro objednávky, které umožňují optimalizaci:

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount).ToList());
```

Všimněte si, že tento dotaz bude přeložen pouze na dva dotazy SQL: Jednu pro zákazníky a druhou pro objednávky.

## <a name="owned-attribute"></a>[Vlastněno] – atribut

Je teď možné nakonfigurovat [vlastní typy entit](xref:core/modeling/owned-entities) tak, že jednoduše přiřadíte typ s `[Owned]` a pak zajistíte, aby se do modelu přidala entita vlastníka:

``` csharp
[Owned]
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}
```

## <a name="command-line-tool-dotnet-ef-included-in-net-core-sdk"></a>Nástroj příkazového řádku dotnet-EF zahrnutý v .NET Core SDK

Příkazy _dotnet-EF_ jsou nyní součástí .NET Core SDK, proto již nebude nutné používat DotNetCliToolReference v projektu, aby bylo možné použít migrace nebo pro vytvoření uživatelského rozhraní DbContext z existující databáze.

Další podrobnosti o tom, jak povolit nástroje příkazového řádku pro různé verze .NET Core SDK a EF Core, najdete v části věnované [instalaci nástrojů](xref:core/miscellaneous/cli/dotnet#installing-the-tools) .

## <a name="microsoftentityframeworkcoreabstractions-package"></a>Balíček Microsoft. EntityFrameworkCore. Abstractions
Nový balíček obsahuje atributy a rozhraní, které můžete ve svých projektech použít k detekci EF Corech funkcí, aniž byste museli pracovat s EF Core jako celek. Například atribut [vlastněno] a rozhraní ILazyLoader jsou umístěny zde.

## <a name="state-change-events"></a>Události změny stavu

Nové `Tracked` události `StateChanged` asedajípoužítkzápisulogiky,kteráreagujenaentity,kterévstupujídoDbContextneboměníjejichstav.`ChangeTracker`

## <a name="raw-sql-parameter-analyzer"></a>Nezpracovaný analyzátor parametrů SQL

K dispozici je nový analyzátor kódu EF Core, který detekuje potenciálně nebezpečná použití našich nezpracovaných rozhraní API, `FromSql` jako `ExecuteSqlCommand`je například nebo. Například pro následující dotaz se zobrazí upozornění, protože _minAge_ není parametrizované:

``` csharp
var sql = $"SELECT * FROM People WHERE Age > {minAge}";
var query = context.People.FromSql(sql);
```

## <a name="database-provider-compatibility"></a>Kompatibilita poskytovatele databáze

Doporučujeme použít EF Core 2,1 s poskytovateli, kteří byli aktualizováni nebo alespoň testováni pro práci s EF Core 2,1.

> [!TIP]
> Pokud v nových funkcích zjistíte jakoukoli neočekávanou nekompatibilitu nebo nějaký problém, nebo pokud na ně máte svůj názor, ohlaste ji prosím pomocí [sledování problémů](https://github.com/aspnet/EntityFrameworkCore/issues/new).
