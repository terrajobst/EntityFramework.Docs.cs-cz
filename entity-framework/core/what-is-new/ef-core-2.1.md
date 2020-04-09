---
title: Co je nového v EF Core 2.1 - EF Core
author: divega
ms.date: 02/20/2018
ms.assetid: 585F90A3-4D5A-4DD1-92D8-5243B14E0FEC
uid: core/what-is-new/ef-core-2.1
ms.openlocfilehash: ba3a26bcd76cd0b9615b13f32456e7280afe533a
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417480"
---
# <a name="new-features-in-ef-core-21"></a>Nové funkce v EF Core 2.1

Kromě mnoha oprav chyb a malých funkčních a výkonnostních vylepšení obsahuje EF Core 2.1 některé přesvědčivé nové funkce:

## <a name="lazy-loading"></a>Opožděné načtení

EF Core nyní obsahuje nezbytné stavební bloky pro každého, kdo autor entity třídy, které můžete načíst jejich navigační vlastnosti na vyžádání. Také jsme vytvořili nový balíček, Microsoft.EntityFrameworkCore.Proxies, který využívá tyto stavební bloky k výrobě opožděné načítání proxy třídy založené na minimálně upravené entity třídy (například třídy s vlastnostmi virtuální navigace).

Další informace o tomto tématu naleznete v [části opožděné načítání.](xref:core/querying/related-data#lazy-loading)

## <a name="parameters-in-entity-constructors"></a>Parametry v konstruktorech entit

Jako jeden z požadovaných stavebních bloků pro opožděné načítání jsme povolili vytváření entit, které berou parametry v jejich konstruktorech. Parametry můžete použít k vložení hodnoty vlastností, opožděné načítání delegátů a služeb.

Další informace o tomto tématu naleznete v [části o konstruktoru entit s parametry.](xref:core/modeling/constructors)

## <a name="value-conversions"></a>Převody hodnot

Až do teď EF Core mohl mapovat pouze vlastnosti typů nativně podporovaných zprostředkovatelem základní databáze. Hodnoty byly zkopírovány tam a zpět mezi sloupci a vlastnostmi bez jakékoli transformace. Počínaje EF Core 2.1, převody hodnot lze použít k transformaci hodnoty získané ze sloupců před jejich použitím na vlastnosti a naopak. Máme řadu převodů, které lze použít podle konvence podle potřeby, stejně jako explicitní konfigurace rozhraní API, které umožňuje registraci vlastní převody mezi sloupci a vlastnostmi. Některé z použití této funkce jsou:

- Ukládání výčtů jako řetězců
- Mapování nepodepsaných intel s SQL Serverem
- Automatické šifrování a dešifrování hodnot vlastností

Další informace o tomto tématu naleznete v [části o převodech hodnot.](xref:core/modeling/value-conversions)  

## <a name="linq-groupby-translation"></a>LINQ GroupPodle překladu

Před verzí 2.1 v EF Core groupby linq operátor by vždy vyhodnoceny v paměti. Nyní podporujeme překlad do klauzule SQL GROUP BY ve většině běžných případů.

Tento příklad ukazuje dotaz s GroupBy slouží k výpočtu různých agregačních funkcí:

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

## <a name="data-seeding"></a>Předvyplnění dat

S novou verzí bude možné poskytnout počáteční data k naplnění databáze. Na rozdíl od EF6 jsou data osiva přidružena k typu entity jako součást konfigurace modelu. Migrace EF Core pak mohou automaticky vypočítat, jaké operace vložení, aktualizace nebo odstranění je třeba použít při upgradu databáze na novou verzi modelu.

Jako příklad můžete použít ke konfiguraci osiva `OnModelCreating`data pro post v :

``` csharp
modelBuilder.Entity<Post>().HasData(new Post{ Id = 1, Text = "Hello World!" });
```

Další informace o tomto tématu naleznete v části o [osivu dat.](xref:core/modeling/data-seeding)  

## <a name="query-types"></a>Typy dotazů

Model EF Core nyní může obsahovat typy dotazů. Na rozdíl od typů entit typy typů nemají klíče definované na nich a nelze vložit, odstranit nebo aktualizovat (to znamená, že jsou jen pro čtení), ale mohou být vráceny přímo dotazy. Některé scénáře použití pro typy dotazů jsou:

- Mapování na zobrazení bez primárních klíčů
- Mapování na tabulky bez primárních klíčů
- Mapování na dotazy definované v modelu
- Slouží jako návratový `FromSql()` typ pro dotazy

Další informace o tomto tématu naleznete v [části o typech dotazů.](xref:core/modeling/keyless-entity-types)

## <a name="include-for-derived-types"></a>Zahrnout pro odvozené typy

Nyní bude možné zadat navigační vlastnosti definované pouze na odvozených `Include` typech při psaní výrazů pro metodu. Pro verzi silného `Include`typu , podporujeme pomocí explicitní `as` přetypovačky nebo operátor. Nyní také podporujeme odkazování na názvy vlastností navigace definovaných na odvozených typech `Include`ve verzi řetězce :

``` csharp
var option1 = context.People.Include(p => ((Student)p).School);
var option2 = context.People.Include(p => (p as Student).School);
var option3 = context.People.Include("School");
```

Další informace o tomto tématu naleznete v [části Zahrnout s odvozenými typy.](xref:core/querying/related-data#include-on-derived-types)

## <a name="systemtransactions-support"></a>Podpora system.transactions

Přidali jsme možnost pracovat s Funkcemi System.Transactions, jako je TransactionScope. To bude fungovat na rozhraní .NET Framework a .NET Core při použití poskytovatelů databáze, které ji podporují.

Další informace o tomto tématu naleznete v [části System.Transactions.](xref:core/saving/transactions#using-systemtransactions)

## <a name="better-column-ordering-in-initial-migration"></a>Lepší řazení sloupců při počáteční migraci

Na základě zpětné vazby od zákazníků jsme aktualizovali migrace, abychom zpočátku generovali sloupce pro tabulky ve stejném pořadí jako vlastnosti deklarované ve třídách. Všimněte si, že EF Core nelze změnit pořadí při přidání nových členů po vytvoření počáteční tabulky.

## <a name="optimization-of-correlated-subqueries"></a>Optimalizace korelovaných poddotazů

Vylepšili jsme překlad dotazů, abychom se vyhnuli provádění dotazů SQL "N + 1" v mnoha běžných scénářích, ve kterých použití navigační vlastnosti v projekci vede k připojení dat z kořenového dotazu s daty z korelovaného poddotazu. Optimalizace vyžaduje ukládání výsledků do vyrovnávací paměti z poddotazu a požadujeme, abyste upravit dotaz opt-in nové chování.

Jako příklad následující dotaz obvykle získá přeloženy do jednoho dotazu pro zákazníky plus N (kde "N" je počet zákazníků vrátil) samostatné dotazy pro objednávky:

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount));
```

Zahrnutím `ToList()` na správném místě označíte, že ukládání do vyrovnávací paměti je vhodné pro objednávky, které umožňují optimalizaci:

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount).ToList());
```

Všimněte si, že tento dotaz bude přeložen pouze na dva dotazy SQL: Jeden pro zákazníky a další pro objednávky.

## <a name="owned-attribute"></a>[Vlastněný] atribut

Nyní je možné nakonfigurovat [typy vlastněných entit](xref:core/modeling/owned-entities) jednoduše `[Owned]` anotací typu a ujistěte se, že entita vlastníka je přidána do modelu:

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

## <a name="command-line-tool-dotnet-ef-included-in-net-core-sdk"></a>Nástroj příkazového řádku dotnet-ef obsažený v sadě .NET Core SDK

Příkazy _dotnet-ef_ jsou nyní součástí sady .NET Core SDK, proto již nebude nutné použít DotNetCliToolReference v projektu, aby bylo možné použít migrace nebo vytvořit uživatelské rozhraní DbContext z existující databáze.

Další podrobnosti o povolení nástrojů příkazového řádku pro různé verze sady .NET Core SDK a EF Core najdete v části [o instalaci nástrojů.](xref:core/miscellaneous/cli/dotnet#installing-the-tools)

## <a name="microsoftentityframeworkcoreabstractions-package"></a>Balíček Microsoft.EntityFrameworkCore.Abstractions

Nový balíček obsahuje atributy a rozhraní, které můžete použít ve svých projektech k osvětlení funkcí EF Core bez závislosti na EF Core jako celku. Zde jsou například umístěny atribut [Owned] a rozhraní ILazyLoader.

## <a name="state-change-events"></a>Události změny stavu

New `Tracked` `StateChanged` And `ChangeTracker` události na lze napsat logiku, která reaguje na entity zadání DbContext nebo změny jejich stavu.

## <a name="raw-sql-parameter-analyzer"></a>Analyzátor nezpracovaných parametrů SQL

Nový analyzátor kódu je součástí EF Core, který detekuje potenciálně nebezpečné použití `FromSql` našich nezpracovaných SQL API, jako je nebo `ExecuteSqlCommand`. Například pro následující dotaz se zobrazí upozornění, protože _minAge_ není parametrizován:

``` csharp
var sql = $"SELECT * FROM People WHERE Age > {minAge}";
var query = context.People.FromSql(sql);
```

## <a name="database-provider-compatibility"></a>Kompatibilita zprostředkovatele databáze

Doporučujeme používat EF Core 2.1 s poskytovateli, kteří byli aktualizováni nebo alespoň testováni pro práci s EF Core 2.1.

> [!TIP]
> Pokud v nových funkcích zjistíte neočekávanou nekompatibilitu nebo jakýkoli problém nebo máte-li k nim zpětnou vazbu, nahlaste ji pomocí [našeho nástroje pro sledování problémů](https://github.com/aspnet/EntityFrameworkCore/issues/new).
