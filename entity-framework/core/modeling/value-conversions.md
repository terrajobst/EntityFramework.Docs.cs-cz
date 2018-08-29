---
title: Převody hodnot – EF Core
author: ajcvickers
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/value-conversions
ms.openlocfilehash: 2a1956221ecc920feba796e4d95cc97259e89c53
ms.sourcegitcommit: 0cef7d448e1e47bdb333002e2254ed42d57b45b6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2018
ms.locfileid: "43152507"
---
# <a name="value-conversions"></a>Převody hodnot

> [!NOTE]  
> Tato funkce je nového v EF Core 2.1.

Převodníky hodnot povolit hodnoty vlastností, které převedou při čtení nebo zápisu do databáze. Tento převod může být z jedné hodnoty do jiného stejného typu (například šifrování řetězců) nebo z hodnoty jednoho typu na hodnotu jiný typ (například převod hodnoty výčtu do a z řetězce v databázi.)

## <a name="fundamentals"></a>Základy

Převodníky hodnot zadávají se stanovují `ModelClrType` a `ProviderClrType`. Typ modelu je typ formátu .NET vlastnosti v typu entity. Typ zprostředkovatele je typ formátu .NET srozumitelné pro poskytovatele databáze. Například uložte výčty jako řetězce v databázi, je typ modelu typ výčtu a zprostředkovatele nejedná `String`. Tyto dva typy mohou být stejné.

Převody jsou definovány pomocí dvou `Func` stromů výrazů: jedna z `ModelClrType` k `ProviderClrType` a druhý z `ProviderClrType` k `ModelClrType`. Stromy výrazů se používají tak, aby může být zkompilovány do databáze přístupový kód pro efektivní převody. Pro komplexní převody stromu výrazů může být jednoduché volání metody, která provádí převod.

## <a name="configuring-a-value-converter"></a>Převaděč hodnoty konfigurace

Převody hodnot, které jsou definovány na vlastnosti v `OnModelCreating` z vaší `DbContext`. Představte si třeba typu výčtu a entity definován jako:
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
Pak převody lze definovat v `OnModelCreating` pro uložení hodnoty výčtu jako řetězce (například "Oslů", "Mul",...) v databázi:
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
> A `null` se nikdy být předána hodnota převaděč hodnoty. To usnadňuje provádění převodů a umožňuje jejich sdílí mezi vlastností s možnou hodnotou Null a Null.

## <a name="the-valueconverter-class"></a>Třída ValueConverter

Volání `HasConversion` jak uvádíme výš, se vytvoří `ValueConverter` instance a nastavte ji na vlastnost. `ValueConverter` Lze vytvořit místo toho explicitně. Příklad:
``` csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
To může být užitečné, když používají stejný převod více vlastností.

> [!NOTE]  
> Aktuálně neexistuje žádný způsob, jak určit na jednom místě, že každé vlastnosti daného typu musí používat stejnou převaděč hodnoty. Tato funkce se budou považovat za pro budoucí verzi.

## <a name="built-in-converters"></a>Vestavěné převaděče

EF Core se dodává se sadou předem definovanou `ValueConverter` třídy, v rámci `Microsoft.EntityFrameworkCore.Storage.ValueConversion` oboru názvů. Toto jsou:
* `BoolToZeroOneConverter` – Bool na 0 a 1
* `BoolToStringConverter` – Bool na řetězce jako je například "Y" a "N"
* `BoolToTwoValuesConverter` – Bool na jakékoli dvě hodnoty
* `BytesToStringConverter` -Bajtové pole na řetězec kódovaný ve formátu Base64
* `CastingConverter` -Převody, které vyžadují pouze přetypování typu
* `CharToStringConverter` -Char jeden znak řetězec
* `DateTimeOffsetToBinaryConverter` – DateTimeOffset zakódovaná binární hodnotu 64-bit
* `DateTimeOffsetToBytesConverter` – DateTimeOffset na pole bajtů
* `DateTimeOffsetToStringConverter` -Typu DateTimeOffset mimo řetězec
* `DateTimeToBinaryConverter` -Datum a čas na 64 bitů hodnotu včetně DateTimeKind
* `DateTimeToStringConverter` -Datum a čas na řetězec
* `DateTimeToTicksConverter` -Datum a čas k značky
* `EnumToNumberConverter` – Enum základní číslo
* `EnumToStringConverter` – Enum řetězec
* `GuidToBytesConverter` -Identifikátor Guid bajtové pole
* `GuidToStringConverter` -Identifikátor Guid do řetězce
* `NumberToBytesConverter` – Jakékoli číselné hodnoty do bajtového pole
* `NumberToStringConverter` – Libovolná číselnou hodnotu na řetězec
* `StringToBytesConverter` -Řetězec, který se bajty UTF8
* `TimeSpanToStringConverter` -Časový interval na řetězec
* `TimeSpanToTicksConverter` -Časový interval má značky

Všimněte si, že `EnumToStringConverter` je zahrnuta v tomto seznamu. To znamená, že není potřeba specifikovat převod explicitně, jak je znázorněno výše. Místo toho použijte integrované převaděč:
``` csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
Všimněte si, že všechny vestavěné převaděče bezstavové a tak jediné instance může bezpečně sdílet víc vlastností.

## <a name="pre-defined-conversions"></a>Předem definované převody

Běžné převody, pro které existuje integrované převaděč není nutné explicitně zadat převaděč. Místo toho stačí pouze nakonfigurovat poskytovatele typu by měla sloužit a automaticky se použije odpovídající integrované převaděč EF. Výčet pro převod řetězců se používají jako příklad výše, ale EF ve skutečnosti to automaticky v případě nakonfigurovaný typ poskytovatele:
``` csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```
Totéž lze dosáhnout explicitního určení typu sloupce. Například pokud je definován typ entity, jako jsou proto:
``` csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```
Potom hodnoty výčtu se uloží jako řetězce v databázi bez jakékoli další konfigurace v `OnModelCreating`.

## <a name="limitations"></a>Omezení

Existuje několik známých aktuální omezení systému převod hodnoty:
* Jak bylo uvedeno výše, `null` nelze převést.
* Aktuálně neexistuje žádný způsob, jak rozšířit převod jednoho vlastnosti na více sloupců nebo naopak.
* Použití převody hodnot může mít vliv na schopnost EF Core překlad výrazů jazyka SQL. Upozornění se protokolují pro tyto případy.
Odebrání těchto omezení se považují za pro budoucí verzi.
