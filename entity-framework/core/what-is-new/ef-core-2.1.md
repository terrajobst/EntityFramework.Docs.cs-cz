---
title: Novinky v EF Core 2.1 – EF Core
author: divega
ms.date: 2/20/2018
ms.assetid: 585F90A3-4D5A-4DD1-92D8-5243B14E0FEC
uid: core/what-is-new/ef-core-2.1
ms.openlocfilehash: ac39d3b7bc001231c5d76f489931b86c108adde2
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995866"
---
# <a name="new-features-in-ef-core-21"></a>Novinky v EF Core 2.1

Kromě řadu oprav chyb a malé vylepšení funkčnosti a výkonu EF Core 2.1 obsahuje některé zajímavé nové funkce:

## <a name="lazy-loading"></a>Opožděné načtení
EF Core teď obsahuje stavební bloky potřebné pro každého, kdo k vytváření tříd entit, které můžete načíst jejich vlastnosti navigace na vyžádání. Vytvořili jsme také nový balíček, Microsoft.EntityFrameworkCore.Proxies, která využívá tyto stavební bloky k vytvoření proxy opožděné načtení třídy na základě minimálně upravit tříd entit (například třídy s virtuální navigační vlastnosti).

Přečtěte si [věnované opožděné načtení](xref:core/querying/related-data#lazy-loading) Další informace o tomto tématu.

## <a name="parameters-in-entity-constructors"></a>Parametry v konstruktorech entity
Jako jeden z požadovaných stavební bloky pro opožděné načtení jsme povolili vytvoření entity, které přijímají parametry v jejich konstruktory. Parametry můžete použít k vložení hodnoty vlastností, opožděné načtení delegátů a služeb.

Čtení [věnované entity konstruktor s parametry](xref:core/modeling/constructors) Další informace o tomto tématu.

## <a name="value-conversions"></a>Převody hodnot
Až doteď EF Core namapovat pouze vlastnosti typů nativně podporuje základní zprostředkovatel databáze. Hodnoty byly zkopírovány vpřed a zpět mezi sloupci a vlastnosti bez jakékoli transformace. Od verze EF Core 2.1, převody hodnot lze použít k transformaci hodnoty získané ze sloupců, než tato nastavení použijí do vlastností a naopak. Máme několik převodů, které se dají nastavit podle konvence, podle potřeby, jakož i rozhraní API explicitní konfigurace, které umožňuje zaregistrovat vlastní převody mezi sloupci a vlastnosti. Zde jsou některé aplikace tuto funkci:

- Ukládání výčty jako řetězce
- Mapování typu unsigned integer s SQL serverem
- Automatické šifrování a dešifrování hodnot vlastností

Přečtěte si [věnované převody hodnot](xref:core/modeling/value-conversions) Další informace o tomto tématu.  

## <a name="linq-groupby-translation"></a>Překlad LINQ GroupBy
Dříve než ve verzi 2.1 v EF Core – operátor GroupBy LINQ vždy vyhodnocována v paměti. Nyní podporujeme překladu klauzule GROUP BY jazyka SQL ve nejběžnější případy.

Tento příklad ukazuje dotaz s GroupBy slouží k výpočtu různých agregační funkce:

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
          Avg = g.Average(o => Amount)
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
V nové verzi je možné poskytnout počátečních dat k naplnění databáze. Na rozdíl od v EF6, předvyplnění dat je přidružena k typu entity jako součást konfigurace modelu. Pak můžete automaticky výpočetní migrace EF Core co vložit, aktualizovat nebo odstranit operací nutné použít při upgradu databáze na novou verzi modelu.

Jako příklad, můžete nakonfigurovat počáteční data pro příspěvek ve `OnModelCreating`:

``` csharp
modelBuilder.Entity<Post>().HasData(new Post{ Id = 1, Text = "Hello World!" });
```

Čtení [věnované předvyplnění dat](xref:core/modeling/data-seeding) Další informace o tomto tématu.  

## <a name="query-types"></a>Typy dotazů
Model, pomocí EF Core teď může obsahovat typy dotazů. Na rozdíl od typy entit, typy dotazů není definována zasílají klíče mají a nelze vložit, odstranit nebo aktualizovat (to znamená, že jsou jen pro čtení), ale mohou být vráceny přímo pomocí dotazů. Mezi scénáře použití pro typy dotazů, patří:

- Mapování na zobrazení bez primárních klíčů
- Mapování tabulek bez primárních klíčů
- Mapování na dotazy definované v modelu
- Slouží jako návratový typ pro `FromSql()` dotazy

Čtení [části na typy dotazů](xref:core/modeling/query-types) Další informace o tomto tématu.

## <a name="include-for-derived-types"></a>Zahrnout pro odvozené typy
Bude nyní specifikovat navigační vlastnosti definována pouze na odvozené typy při psaní výrazů pro `Include` metody. Pro verzi silného typu `Include`, podporujeme používání explicitní přetypování nebo `as` operátor. Také teď podporují odkazování na název navigační vlastnosti definované v odvozených typů ve verzi řetězec `Include`:

``` csharp
var option1 = context.People.Include(p => ((Student)p).School);
var option2 = context.People.Include(p => (p as Student).School);
var option3 = context.People.Include("School");
```

Čtení [věnované zahrnout u odvozených typů](xref:core/querying/related-data#include-on-derived-types) Další informace o tomto tématu.

## <a name="systemtransactions-support"></a>Podpora System.Transactions
Přidali jsme možnost pro práci s System.Transactions funkce, jako je objekt TransactionScope. To bude fungovat na rozhraní .NET Framework a .NET Core, při použití poskytovatelé databází, které ho podporují.

Čtení [věnované System.Transactions](xref:core/saving/transactions#using-systemtransactions) Další informace o tomto tématu.

## <a name="better-column-ordering-in-initial-migration"></a>Lepší pořadí sloupců v počáteční migraci
Na základě názorů zákazníků, aktualizovali jsme migrace se zpočátku vygenerovat sloupce tabulky ve stejném pořadí, protože vlastnosti jsou deklarovány ve třídách. Všimněte si, že EF Core nelze změnit pořadí při přidání nových členů po vytvoření počátečního tabulky.

## <a name="optimization-of-correlated-subqueries"></a>Optimalizace korelační poddotazů
Vylepšili jsme naše překladu dotazu, aby se zabránilo spouštění "N + 1" dotazy SQL v mnoha běžných scénářů, ve kterých využití vlastnost navigace v projekci vede k spojování dat z dotazu kořenové s daty z korelační poddotazu. Optimalizace vyžaduje ukládání do vyrovnávací paměti výsledky poddotazu a je potřeba se, že změníte dotaz k vyjádření výslovného souhlasu nové chování.

Například následující dotaz obvykle převedeny do jednoho dotazu pro zákazníky, a navíc N (kde "N" je počet zákazníků vrátil) samostatné dotazy pro objednávky:

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount));
```

Zahrnutím `ToList()` na správném místě, určujete, že ukládání do vyrovnávací paměti je vhodný pro objednávky, které umožňují optimalizace:

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount).ToList());
```

Všimněte si, že tento dotaz se přeložit na pouze dva dotazy SQL: jeden pro zákazníky a další příkaz za objednávky.

## <a name="owned-attribute"></a>Atribut [vlastní]

Nyní je možné nakonfigurovat [vlastněné typy entit](xref:core/modeling/owned-entities) jednoduše anotací typu s `[Owned]` a pak zajistit, owner entity se přidá do modelu:

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

## <a name="command-line-tool-dotnet-ef-included-in-net-core-sdk"></a>Nástroj příkazového řádku dotnet-ef zahrnutý v sadě SDK .NET Core

_Dotnet ef_ příkazy jsou teď součástí sady .NET Core SDK, proto už bude nutné použít DotNetCliToolReference v projektu mohli používat migrace nebo scaffold DbContext z existující databáze.

Naleznete v části [instalace nástrojů](xref:core/miscellaneous/cli/dotnet#installing-the-tools) podrobné informace o tom, jak povolit nástroje příkazového řádku pro různé verze sady SDK .NET Core a EF Core.

## <a name="microsoftentityframeworkcoreabstractions-package"></a>Microsoft.EntityFrameworkCore.Abstractions balíčku
Nový balíček obsahuje atributy a rozhraní, které můžete použít ve vašich projektech, aby bylo možné EF Core funkce bez nutnosti přepínat závislost na EF Core jako celek. Například atribut [vlastněno] a ILazyLoader interface jsou umístěny zde.

## <a name="state-change-events"></a>Události změny stavu

Nové `Tracked` a `StateChanged` události na `ChangeTracker` můžete použít pro zapsání logiku, která reaguje na entity zadávání uvolněn objekt DbContext nebo změnit jejich stav.

## <a name="raw-sql-parameter-analyzer"></a>Nezpracovaná analyzátoru parametr SQL

Nový analyzátor kódu je součástí EF Core, který zjistí potenciálně nebezpečná použití rozhraní API nezpracovaná SQL, jako je třeba `FromSql` nebo `ExecuteSqlCommand`. Například následující dotaz, zobrazí se upozornění protože _minAge_ není s parametry:

``` csharp
var sql = $"SELECT * FROM People WHERE Age > {minAge}";
var query = context.People.FromSql(sql);
```

## <a name="database-provider-compatibility"></a>Zprostředkovatel kompatibility databáze

Doporučuje se, že používáte EF Core 2.1 s poskytovateli, které byly aktualizovány nebo alespoň testovat pro práci s EF Core 2.1.

> [!TIP]
> Pokud zjistíte všechny neočekávané nekompatibility ani žádnou vydat v nových funkcí nebo pokud máte nějakou zpětnou vazbu, oznamte ji pomocí [naše sledování problémů](https://github.com/aspnet/EntityFrameworkCore/issues/new).
