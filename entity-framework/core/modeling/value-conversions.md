---
title: "Převody hodnot - EF jádra"
author: ajcvickers
ms.author: divega
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/value-conversions
ms.openlocfilehash: 50acba39cdec16caa9300fcaf47ab6242a4f69fb
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="value-conversions"></a>Převody hodnot

> [!NOTE]  
> Tato funkce je nového v EF základní 2.1.

Převodníky hodnot povolit hodnoty vlastností, které má být převeden při čtení nebo zápisu do databáze. Tento převod může být z jednu hodnotu do jiné stejného typu (například šifrování řetězce), nebo z hodnoty jednoho typu na hodnotu jiného typu (například převod hodnoty výčtu do a z řetězce v databázi.)

## <a name="fundamentals"></a>Základy

Převodníky hodnot nejsou zadány z hlediska `ModelClrType` a `ProviderClrType`. Typ modelu je typ formátu .NET vlastnosti v typu entity. Typ zprostředkovatele je typ formátu .NET rozumí zprostředkovatelem databáze. Například uložit výčty jako řetězce v databázi, typu modelu je typ výčtového typu, a je typ poskytovatele `String`. Tyto dva typy může být stejné.

Převody jsou definovány pomocí dvou `Func` stromů výrazů: jeden z `ModelClrType` k `ProviderClrType` a dalších z `ProviderClrType` k `ModelClrType`. Stromy výrazů se používají tak, aby mohou být zkompilovány do kódu přístup k databázi pro efektivní převody. Pro komplexní převody strom výrazu může být jednoduché volání metody, která provádí převod.

## <a name="configuring-a-value-converter"></a>Převaděč hodnoty konfigurace

Převody hodnot jsou definovány na vlastnosti v OnModelCreating z vaší DbContext. Představte si třeba definovaný jako typ výčtu a entity:
```Csharp
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
Pak lze definovat převody v OnModelCreating ukládání hodnot výčtu jako řetězce (např.) "Oslů", "Mul",...) v databázi:
```Csharp
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
> A `null` hodnota se nikdy předá převaděč hodnoty. To usnadňuje provádění převody a umožňuje jejich sdílení mezi vlastnosti s možnou hodnotou Null a neumožňuje hodnotu Null.

## <a name="the-valueconverter-class"></a>ValueConverter – třída

Volání metody `HasConversion` jako v příkladu nahoře, vytvoří `ValueConverter` instance a nastavte ji na vlastnosti. `ValueConverter` Lze vytvořit místo explicitně. Příklad:
```Csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
To může být užitečné, když používají stejné převod více vlastností.

> [!NOTE]  
> Aktuálně neexistuje žádný způsob, jak určit na jednom místě, že všechny vlastnosti daného typu musí používat stejné převaděč hodnoty. Tato funkce bude považovat za pro budoucí použití.

## <a name="built-in-converters"></a>Vestavěné převaděče

Předem definované EF základní dodává se sadou `ValueConverter` třídy, které jsou součástí `Microsoft.EntityFrameworkCore.Storage.Converters` oboru názvů. Jsou to:
* `BoolToZeroOneConverter` – Bool na 0 a 1
* `BoolToStringConverter` – Bool do řetězců, jako je například "Y" a "N"
* `BoolToTwoValuesConverter` – Bool jakékoli dvě hodnoty
* `BytesToStringConverter` -Bajtové pole pro řetězec s kódováním Base64
* `CastingConverter` – Převody, které vyžadují jenom přetypování Csharp
* `CharToStringConverter` -Char jednoho znaku řetězec
* `DateTimeOffsetToBinaryConverter` – DateTimeOffset kódováním binární hodnotu 64-bit
* `DateTimeOffsetToBytesConverter` – DateTimeOffset do bajtového pole
* `DateTimeOffsetToStringConverter` – DateTimeOffset řetězec
* `DateTimeToBinaryConverter` -Datum a čas 64 bitů hodnotu včetně parametr DateTimeKind
* `DateTimeToStringConverter` -Datum a čas na řetězec
* `DateTimeToTicksConverter` -Datum a čas k rysky
* `EnumToNumberConverter` – Enum základní číslo
* `EnumToStringConverter` – Enum řetězec
* `GuidToBytesConverter` -Guid do bajtového pole
* `GuidToStringConverter` -Guid do řetězce
* `NumberToBytesConverter` -Jakékoli číselné hodnoty do bajtového pole
* `NumberToStringConverter` -Všechny číselnou hodnotu na řetězec
* `StringToBytesConverter` -Řetězce UTF8 bajtů
* `TimeSpanToStringConverter` -Časový interval na řetězec
* `TimeSpanToTicksConverter` -Časový interval na dílky

Všimněte si, že `EnumToStringConverter` je zahrnuta v tomto seznamu. To znamená, že je nutné specifikovat převod explicitně, jak je uvedeno výše. Místo toho použijte předdefinovaný převaděč:
```Csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
Upozorňujeme, že jsou všechny vestavěné převaděče bezstavové a tak jediné instance lze bezpečně sdílet mezi více vlastností.

## <a name="pre-defined-conversions"></a>Předem definované převody

Pro běžné převody, pro které existuje předdefinované převaděč není nutné explicitně zadat převaděč. Místo toho stačí pouze nakonfigurovat které typ zprostředkovatele má být použit a EF budou automaticky používat odpovídající sestavení převaděč. Enum pro převod řetězců slouží jako příklad výše, ale EF ve skutečnosti to automaticky v případě nastaven typ poskytovatele:
```Csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```
Samé lze dosáhnout explicitním zadáním typ sloupce. Například pokud je definován typ entity typu tak:
```Csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```
Hodnoty výčtu pak bude uložena jako řetězce v databázi bez další konfigurace v OnModelCreating.

## <a name="limitations"></a>Omezení

Existuje několik známá aktuální omezení systému převod hodnoty:
* Jak jsme uvedli výše, `null` nelze převést.
* Aktuálně neexistuje žádný způsob, jak šíří převod jednu vlastnost multuple sloupce nebo naopak.
* Použití převody hodnot může mít vliv na schopnost EF základní převede výrazy jazyka SQL. Upozornění bude protokolována takových případech.
Odebrání těchto omezení se považuje za pro budoucí použití.
