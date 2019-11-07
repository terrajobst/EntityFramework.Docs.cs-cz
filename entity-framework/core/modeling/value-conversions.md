---
title: Převody hodnot – EF Core
author: ajcvickers
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/value-conversions
ms.openlocfilehash: 93774bc1bc3887f982faeac151825a6643c1107c
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73654786"
---
# <a name="value-conversions"></a>Převody hodnot

> [!NOTE]  
> Tato funkce je v EF Core 2,1 novinkou.

Převaděče hodnot umožňují převod hodnot vlastností při čtení nebo zápisu do databáze. Tento převod může být z jedné hodnoty na jiný stejného typu (například šifrovací řetězce) nebo z hodnoty jednoho typu na hodnotu jiného typu (například převod hodnot výčtu na a z řetězců v databázi.)

## <a name="fundamentals"></a>Základy

Převaděče hodnot jsou zadány na základě `ModelClrType` a `ProviderClrType`. Typ modelu je typ .NET vlastnosti v typu entity. Typ zprostředkovatele je typ .NET, který rozumí poskytovatel databáze. Chcete-li například uložit výčty jako řetězce v databázi, typ modelu je typ výčtu a typ poskytovatele `String`. Tyto dva typy mohou být stejné.

Převody jsou definovány pomocí dvou `Func` stromů výrazů: jeden z `ModelClrType` na `ProviderClrType` a druhý od `ProviderClrType` až po `ModelClrType`. Stromy výrazů se používají tak, aby je bylo možné kompilovat do kódu přístupu k databázi pro efektivní převody. U složitých převodů může být strom výrazů jednoduchým voláním metody, která provádí převod.

## <a name="configuring-a-value-converter"></a>Konfigurace převaděče hodnot

Převody hodnot jsou definovány ve vlastnostech `OnModelCreating` `DbContext`. Představte si například výčet a typ entity definovaný jako:

``` csharp
public class Rider
{
    public int Id { get; set; }
    public EquineBeast Mount { get; set; }
}

public enum EquineBeast
{
    Donkey,
    Mule,
    Horse,
    Unicorn
}
```

Pak lze převody definovat v `OnModelCreating` pro uložení hodnot výčtu jako řetězců (například "Donkey", "Mule",...) v databázi:

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder
        .Entity<Rider>()
        .Property(e => e.Mount)
        .HasConversion(
            v => v.ToString(),
            v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));
}
```

> [!NOTE]  
> Hodnota `null` nebude nikdy předána převaděči hodnot. Díky tomu je implementace převodů jednodušší a umožňuje, aby se sdílely mezi vlastnosti s možnou hodnotou null a nepovolující hodnotu null.

## <a name="the-valueconverter-class"></a>Třída ValueConverter

Volání `HasConversion` jak je uvedeno výše, vytvoří instanci `ValueConverter` a nastaví ji na vlastnost. Místo toho `ValueConverter` lze vytvořit explicitně. Příklad:

``` csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```

To může být užitečné, pokud více vlastností používá stejný převod.

> [!NOTE]  
> V současné době neexistuje způsob, jak zadat na jednom místě, kde musí každá vlastnost daného typu používat stejný konvertor hodnoty. Tato funkce se bude považovat za budoucí verzi.

## <a name="built-in-converters"></a>Předdefinované převaděče

EF Core se dodává se sadou předem definovaných `ValueConverter` tříd, která se nachází v oboru názvů `Microsoft.EntityFrameworkCore.Storage.ValueConversion`. Jsou to tyto:

* `BoolToZeroOneConverter` – bool na nulu a jeden
* `BoolToStringConverter` – bool pro řetězce, jako například "Y" a "N"
* `BoolToTwoValuesConverter` – bool pro libovolné dvě hodnoty
* `BytesToStringConverter` bajtové pole na řetězec kódovaný v kódování Base64
* `CastingConverter` – převody, které vyžadují pouze přetypování typu
* `CharToStringConverter`-char do řetězce s jedním znakem
* `DateTimeOffsetToBinaryConverter`-DateTimeOffset až do binární hodnoty s kódováním 64
* pole `DateTimeOffsetToBytesConverter`-DateTimeOffset až bajtů
* `DateTimeOffsetToStringConverter`-DateTimeOffset do řetězce
* `DateTimeToBinaryConverter`-hodnota DateTime až 64, včetně DateTimeKind
* `DateTimeToStringConverter`-DateTime do řetězce
* `DateTimeToTicksConverter`-DateTime na takty
* `EnumToNumberConverter` – výčet pro základní číslo
* `EnumToStringConverter` – výčet do řetězce
* `GuidToBytesConverter` – identifikátor GUID pro pole bajtů
* `GuidToStringConverter`-GUID k řetězci
* `NumberToBytesConverter` – jakákoli číselná hodnota k poli bajtů
* `NumberToStringConverter` – jakákoli číselná hodnota k řetězci
* `StringToBytesConverter` – řetězec na bajty UTF8
* `TimeSpanToStringConverter`-TimeSpan do řetězce
* `TimeSpanToTicksConverter`-TimeSpan k taktům

Všimněte si, že `EnumToStringConverter` je součástí tohoto seznamu. To znamená, že není nutné explicitně zadávat převod, jak je uvedeno výše. Místo toho stačí použít vestavěný převaděč:

``` csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```

Všimněte si, že všechny předdefinované převaděče jsou bezstavové, takže jednu instanci lze bezpečně sdílet pomocí více vlastností.

## <a name="pre-defined-conversions"></a>Předem definované převody

Pro běžné převody, pro které existuje vestavěný převaděč, není nutné explicitně zadávat převaděč. Místo toho stačí nakonfigurovat, který typ poskytovatele má být použit, a EF bude automaticky používat odpovídající vestavěný převaděč. Výčty na převody řetězců se používají jako příklad výše, ale EF to vlastně provede automaticky, pokud je nakonfigurovaný typ zprostředkovatele:

``` csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```

Stejné věci je možné dosáhnout explicitním určením typu sloupce. Například pokud je typ entity definovaný jako příklad:

``` csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```

Hodnoty výčtu pak budou uloženy jako řetězce v databázi bez další konfigurace v `OnModelCreating`.

## <a name="limitations"></a>Omezení

K dispozici je několik známých omezení systému převodu hodnot:

* Jak bylo uvedeno výše, `null` nelze převést.
* V současné době neexistuje způsob, jak rozložit převod jedné vlastnosti do více sloupců nebo naopak.
* Použití převodů hodnot může mít vliv na schopnost EF Core přeložit výrazy do jazyka SQL. V takových případech bude zaznamenáno upozornění.
Odebrání těchto omezení se považuje za budoucí verzi.
