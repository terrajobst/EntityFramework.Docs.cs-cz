---
title: "Co je nového v EF základní 2.1 - EF jádra"
author: divega
ms.author: divega
ms.date: 2/20/2018
ms.assetid: 585F90A3-4D5A-4DD1-92D8-5243B14E0FEC
ms.technology: entity-framework-core
uid: core/what-is-new/ef-core-2.1
ms.openlocfilehash: bb1e691e0f22bd36467d58c02bde22c63067207e
ms.sourcegitcommit: fcaeaf085171dfb5c080ec42df1d1df8dfe204fb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
# <a name="new-features-in-ef-core-21"></a>Nové funkce v EF základní 2.1
> [!NOTE]  
> Tato verze je stále ve verzi preview.

Kromě malé množství vylepšení a opravy chyb pro více než 100 produktu EF základní 2.1 obsahuje několik nových funkcí:

## <a name="lazy-loading"></a>opožděného načítání
Základní EF nyní obsahuje stavební bloky potřebné pro každý, kdo k vytváření tříd entit, které můžete načíst jejich navigační vlastnosti na vyžádání. Také jsme vytvořili nový balíček, Microsoft.EntityFrameworkCore.Proxies, která využívá tyto stavební bloky k vytvoření opožděného načítání proxy třídy na základě minimálně upravit tříd entit (např. tříd pomocí virtuální navigační vlastnosti).

Pro čtení [část opožděného načítání](xref:core/querying/related-data#lazy-loading) Další informace o tomto tématu.

## <a name="parameters-in-entity-constructors"></a>Parametry v konstruktorech entity
Jako jeden z požadovaných stavební bloky pro opožděného načítání jsme povoleno vytváření entit, které vzít parametry v jejich konstruktory. Parametry můžete vložit hodnoty vlastností, opožděného načítání Delegáti a služby.

Pro čtení [části na entity konstruktor s parametry](xref:core/modeling/constructors) Další informace o tomto tématu.

## <a name="value-conversions"></a>Převody hodnot
Až do této chvíle může EF základní mapují jenom vlastnosti typů nativně podporuje základní zprostředkovatel databáze. Hodnoty byly zkopírovány přepínat mezi sloupci a vlastnosti bez jakékoli transformace. Počínaje EF základní 2.1, převody hodnot můžete použít k transformaci hodnot zjištěných z sloupců, než je použijete k vlastnosti a naopak. Máme několik převody, které mohou být použity podle konvence podle potřeby, jakož i rozhraní API explicitní konfigurace, který umožňuje registraci vlastní převody mezi sloupci a vlastnosti. Některé aplikace této funkce jsou:

- Ukládání výčty jako řetězce
- Mapování nepodepsané celých čísel s SQL serverem
- Automatické šifrování a dešifrování hodnot vlastností

Pro čtení [část převody hodnot](xref:core/modeling/value-conversions) Další informace o tomto tématu.  

## <a name="linq-groupby-translation"></a>Překlad LINQ GroupBy
Před vyhodnocením verze 2.1, které jsou v základní EF operátor GroupBy LINQ byl vždy v paměti. Teď podporují překladu klauzule SQL GROUP BY ve nejběžnější případy.

Tento příklad ukazuje dotaz s GroupBy slouží k výpočtu různé agregační funkce:

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

## <a name="data-seeding"></a>Data synchronizace replik indexů
S novou verzí je možné zajistit počáteční data k naplnění databáze. Na rozdíl od v EF6, synchronizace replik indexů dat je přidružena k typu entity jako součást konfigurace modelu. Potom základní EF migrace můžete vypočítat automaticky, co vložit, aktualizovat nebo odstranit potřeba operations použijí při upgradu databáze na novou verzi modelu.

Jako příklad, můžete to konfigurovat počáteční data pro metodu Post v `OnModelCreating`:

``` csharp
modelBuilder.Entity<Post>().SeedData(new Post{ Id = 1, Text = "Hello World!" });
```

Pro čtení [část dat synchronizace replik indexů](xref:core/modeling/data-seeding) Další informace o tomto tématu.  

## <a name="query-types"></a>Typy dotazů
Model EF základní můžete nyní zahrnují typy dotazů. Na rozdíl od typy entit, typy dotazů není mít definované na nich klíče a nelze vložit, odstranit nebo aktualizovat (tj. jsou jen pro čtení), ale mohou být vráceny přímo pomocí dotazů. Některé scénáře použití pro typy dotazů jsou:

- Mapování na zobrazení bez primárních klíčů
- Mapování tabulek bez primárních klíčů
- Mapování na dotazy, které jsou definované v modelu
- Slouží jako návratový typ pro `FromSql()` dotazy

Pro čtení [části na typy dotazů](xref:core/modeling/query-types) Další informace o tomto tématu.

## <a name="include-for-derived-types"></a>Zahrnout pro odvozené typy
Je teď možné zadat definována pouze na navigační vlastnosti odvozených typů při zápisu výrazy pro `Include` metoda. Pro verzi silného typu `Include`, podporujeme pomocí explicitní přetypování nebo `as` operátor. Nyní také podporují odkazování na názvy navigační vlastnost definovanou u odvozených typů v řetězec verze `Include`:

``` csharp
var option1 = context.People.Include(p => ((Student)p).School);
var option2 = context.People.Include(p => (p as Student).School);
var option3 = context.People.Include("School");
```

Pro čtení [části na zahrnout u odvozených typů](xref:core/querying/related-data#include-on-derived-types) Další informace o tomto tématu.

## <a name="systemtransactions-support"></a>System.Transactions – podpora
Jsme přidali schopnost pracovat s funkcemi System.Transactions – například TransactionScope. To bude fungovat na rozhraní .NET Framework a .NET Core, při použití zprostředkovatele databáze, které ji podporují.

Pro čtení [část System.Transactions –](xref:core/saving/transactions#using-systemtransactions) Další informace o tomto tématu.

## <a name="better-column-ordering-in-initial-migration"></a>Lepší pořadí sloupců v počáteční migrace
Na základě názorů zákazníků, jsme jste aktualizovali migrace na začátku generovat sloupce pro tabulky ve stejném pořadí jako vlastnosti jsou deklarované v třídách. Všimněte si, že EF základní nelze změnit pořadí při přidání nové členy po vytvoření počáteční tabulky.

## <a name="optimization-of-correlated-subqueries"></a>Optimalizace korelační poddotazy
Jsme vylepšili naše překlad dotazu, aby se zabránilo spouštění "N + 1" dotazy SQL v mnoha běžným scénářům, ve kterých využití navigační vlastnosti v projekci vede k spojování dat z dotazu kořenové s daty z korelační poddotaz. Optimalizace vyžaduje ukládání do vyrovnávací paměti výsledky tvoří poddotaz a je nutné upravit dotaz tak, aby výslovný souhlas nové chování.

Jako příklad následující dotaz normálně získá přeložit na jeden dotaz pro zákazníky, plus N (kde "N" je počet zákazníků vrátil) oddělit dotazy pro objednávky:

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount));
```

Zahrnutím `ToList()` na správném místě znamenat, že ukládání do vyrovnávací paměti je vhodné pro objednávky, které umožňují optimalizace:

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount).ToList());
```

Všimněte si, že tento dotaz bude možné přeložit pouze dva dotazy SQL: jeden pro zákazníky a dalším pro objednávky.

## <a name="ownedattribute"></a>OwnedAttribute

Nyní je možné nakonfigurovat [vlastní typy entit](xref:core/modeling/owned-entities) podle jednoduše zadávání poznámek k na typ s `[Owned]` a pak zajistit, owner entity přidají do modelu:

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

## <a name="database-provider-compatibility"></a>Zprostředkovatel kompatibility databáze

Základní EF 2.1 byla navržená tak, aby byl kompatibilní s poskytovateli databáze vytvořené pro EF základní 2.0. Zatímco některé funkce popsané výše (např. převody hodnot) vyžadují poskytovatele aktualizované, ostatní (např. opožděného načítání) bude light zprostředkovatelům existující.

> [!TIP]
> Pokud zjistíte, že jsou všechny neočekávané nekompatibilita žádný problém nové funkce nebo pokud máte zpětnou vazbu na, nahlaste jej pomocí [náš sledovací modul problém](https://github.com/aspnet/EntityFrameworkCore/issues/new).
