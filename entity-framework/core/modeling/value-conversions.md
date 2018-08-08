---
title: Převody hodnot – EF Core
author: ajcvickers
ms.author: divega
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/value-conversions
ms.openlocfilehash: d5189cef6d44fdf3fd6116a2952ce07ff3a389d4
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614396"
---
# <a name="value-conversions"></a><span data-ttu-id="4b3aa-102">Převody hodnot</span><span class="sxs-lookup"><span data-stu-id="4b3aa-102">Value Conversions</span></span>

> [!NOTE]  
> <span data-ttu-id="4b3aa-103">Tato funkce je nového v EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="4b3aa-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="4b3aa-104">Převodníky hodnot povolit hodnoty vlastností, které převedou při čtení nebo zápisu do databáze.</span><span class="sxs-lookup"><span data-stu-id="4b3aa-104">Value converters allow property values to be converted when reading from or writing to the database.</span></span> <span data-ttu-id="4b3aa-105">Tento převod může být z jedné hodnoty do jiného stejného typu (například šifrování řetězců) nebo z hodnoty jednoho typu na hodnotu jiný typ (například převod hodnoty výčtu do a z řetězce v databázi.)</span><span class="sxs-lookup"><span data-stu-id="4b3aa-105">This conversion can be from one value to another of the same type (for example, encrypting strings) or from a value of one type to a value of another type (for example, converting enum values to and from strings in the database.)</span></span>

## <a name="fundamentals"></a><span data-ttu-id="4b3aa-106">Základy</span><span class="sxs-lookup"><span data-stu-id="4b3aa-106">Fundamentals</span></span>

<span data-ttu-id="4b3aa-107">Převodníky hodnot zadávají se stanovují `ModelClrType` a `ProviderClrType`.</span><span class="sxs-lookup"><span data-stu-id="4b3aa-107">Value converters are specified in terms of a `ModelClrType` and a `ProviderClrType`.</span></span> <span data-ttu-id="4b3aa-108">Typ modelu je typ formátu .NET vlastnosti v typu entity.</span><span class="sxs-lookup"><span data-stu-id="4b3aa-108">The model type is the .NET type of the property in the entity type.</span></span> <span data-ttu-id="4b3aa-109">Typ zprostředkovatele je typ formátu .NET srozumitelné pro poskytovatele databáze.</span><span class="sxs-lookup"><span data-stu-id="4b3aa-109">The provider type is the .NET type understood by the database provider.</span></span> <span data-ttu-id="4b3aa-110">Například uložte výčty jako řetězce v databázi, je typ modelu typ výčtu a zprostředkovatele nejedná `String`.</span><span class="sxs-lookup"><span data-stu-id="4b3aa-110">For example, to save enums as strings in the database, the model type is the type of the enum, and the provider type is `String`.</span></span> <span data-ttu-id="4b3aa-111">Tyto dva typy mohou být stejné.</span><span class="sxs-lookup"><span data-stu-id="4b3aa-111">These two types can be the same.</span></span>

<span data-ttu-id="4b3aa-112">Převody jsou definovány pomocí dvou `Func` stromů výrazů: jedna z `ModelClrType` k `ProviderClrType` a druhý z `ProviderClrType` k `ModelClrType`.</span><span class="sxs-lookup"><span data-stu-id="4b3aa-112">Conversions are defined using two `Func` expression trees: one from `ModelClrType` to `ProviderClrType` and the other from `ProviderClrType` to `ModelClrType`.</span></span> <span data-ttu-id="4b3aa-113">Stromy výrazů se používají tak, aby může být zkompilovány do databáze přístupový kód pro efektivní převody.</span><span class="sxs-lookup"><span data-stu-id="4b3aa-113">Expression trees are used so that they can be compiled into the database access code for efficient conversions.</span></span> <span data-ttu-id="4b3aa-114">Pro komplexní převody stromu výrazů může být jednoduché volání metody, která provádí převod.</span><span class="sxs-lookup"><span data-stu-id="4b3aa-114">For complex conversions, the expression tree may be a simple call to a method that performs the conversion.</span></span>

## <a name="configuring-a-value-converter"></a><span data-ttu-id="4b3aa-115">Převaděč hodnoty konfigurace</span><span class="sxs-lookup"><span data-stu-id="4b3aa-115">Configuring a value converter</span></span>

<span data-ttu-id="4b3aa-116">Převody hodnot, které jsou definovány na vlastnosti v `OnModelCreating` z vaší `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="4b3aa-116">Value conversions are defined on properties in the `OnModelCreating` of your `DbContext`.</span></span> <span data-ttu-id="4b3aa-117">Představte si třeba typu výčtu a entity definován jako:</span><span class="sxs-lookup"><span data-stu-id="4b3aa-117">For example, consider an enum and entity type defined as:</span></span>
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
<span data-ttu-id="4b3aa-118">Pak převody lze definovat v `OnModelCreating` pro uložení hodnoty výčtu jako řetězce (například "Oslů", "Mul",...) v databázi:</span><span class="sxs-lookup"><span data-stu-id="4b3aa-118">Then conversions can be defined in `OnModelCreating` to store the enum values as strings (for example, "Donkey", "Mule", ...) in the database:</span></span>
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
> <span data-ttu-id="4b3aa-119">A `null` se nikdy být předána hodnota převaděč hodnoty.</span><span class="sxs-lookup"><span data-stu-id="4b3aa-119">A `null` value will never be passed to a value converter.</span></span> <span data-ttu-id="4b3aa-120">To usnadňuje provádění převodů a umožňuje jejich sdílí mezi vlastností s možnou hodnotou Null a Null.</span><span class="sxs-lookup"><span data-stu-id="4b3aa-120">This makes the implementation of conversions easier and allows them to be shared amongst nullable and non-nullable properties.</span></span>

## <a name="the-valueconverter-class"></a><span data-ttu-id="4b3aa-121">Třída ValueConverter</span><span class="sxs-lookup"><span data-stu-id="4b3aa-121">The ValueConverter class</span></span>

<span data-ttu-id="4b3aa-122">Volání `HasConversion` jak uvádíme výš, se vytvoří `ValueConverter` instance a nastavte ji na vlastnost.</span><span class="sxs-lookup"><span data-stu-id="4b3aa-122">Calling `HasConversion` as shown above will create a `ValueConverter` instance and set it on the property.</span></span> <span data-ttu-id="4b3aa-123">`ValueConverter` Lze vytvořit místo toho explicitně.</span><span class="sxs-lookup"><span data-stu-id="4b3aa-123">The `ValueConverter` can instead be created explicitly.</span></span> <span data-ttu-id="4b3aa-124">Příklad:</span><span class="sxs-lookup"><span data-stu-id="4b3aa-124">For example:</span></span>
```Csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
<span data-ttu-id="4b3aa-125">To může být užitečné, když používají stejný převod více vlastností.</span><span class="sxs-lookup"><span data-stu-id="4b3aa-125">This can be useful when multiple properties use the same conversion.</span></span>

> [!NOTE]  
> <span data-ttu-id="4b3aa-126">Aktuálně neexistuje žádný způsob, jak určit na jednom místě, že každé vlastnosti daného typu musí používat stejnou převaděč hodnoty.</span><span class="sxs-lookup"><span data-stu-id="4b3aa-126">There is currently no way to specify in one place that every property of a given type must use the same value converter.</span></span> <span data-ttu-id="4b3aa-127">Tato funkce se budou považovat za pro budoucí verzi.</span><span class="sxs-lookup"><span data-stu-id="4b3aa-127">This feature will be considered for a future release.</span></span>

## <a name="built-in-converters"></a><span data-ttu-id="4b3aa-128">Vestavěné převaděče</span><span class="sxs-lookup"><span data-stu-id="4b3aa-128">Built-in converters</span></span>

<span data-ttu-id="4b3aa-129">EF Core se dodává se sadou předem definovanou `ValueConverter` třídy, v rámci `Microsoft.EntityFrameworkCore.Storage.ValueConversion` oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="4b3aa-129">EF Core ships with a set of pre-defined `ValueConverter` classes, found in the `Microsoft.EntityFrameworkCore.Storage.ValueConversion` namespace.</span></span> <span data-ttu-id="4b3aa-130">Toto jsou:</span><span class="sxs-lookup"><span data-stu-id="4b3aa-130">These are:</span></span>
* <span data-ttu-id="4b3aa-131">`BoolToZeroOneConverter` – Bool na 0 a 1</span><span class="sxs-lookup"><span data-stu-id="4b3aa-131">`BoolToZeroOneConverter` - Bool to zero and one</span></span>
* <span data-ttu-id="4b3aa-132">`BoolToStringConverter` – Bool na řetězce jako je například "Y" a "N"</span><span class="sxs-lookup"><span data-stu-id="4b3aa-132">`BoolToStringConverter` - Bool to strings such as "Y" and "N"</span></span>
* <span data-ttu-id="4b3aa-133">`BoolToTwoValuesConverter` – Bool na jakékoli dvě hodnoty</span><span class="sxs-lookup"><span data-stu-id="4b3aa-133">`BoolToTwoValuesConverter` - Bool to any two values</span></span>
* <span data-ttu-id="4b3aa-134">`BytesToStringConverter` -Bajtové pole na řetězec kódovaný ve formátu Base64</span><span class="sxs-lookup"><span data-stu-id="4b3aa-134">`BytesToStringConverter` - Byte array to Base64-encoded string</span></span>
* <span data-ttu-id="4b3aa-135">`CastingConverter` -Převody, které vyžadují pouze přetypování Csharp</span><span class="sxs-lookup"><span data-stu-id="4b3aa-135">`CastingConverter` - Conversions that require only a Csharp cast</span></span>
* <span data-ttu-id="4b3aa-136">`CharToStringConverter` -Char jeden znak řetězec</span><span class="sxs-lookup"><span data-stu-id="4b3aa-136">`CharToStringConverter` - Char to single character string</span></span>
* <span data-ttu-id="4b3aa-137">`DateTimeOffsetToBinaryConverter` – DateTimeOffset zakódovaná binární hodnotu 64-bit</span><span class="sxs-lookup"><span data-stu-id="4b3aa-137">`DateTimeOffsetToBinaryConverter` - DateTimeOffset to binary-encoded 64-bit value</span></span>
* <span data-ttu-id="4b3aa-138">`DateTimeOffsetToBytesConverter` – DateTimeOffset na pole bajtů</span><span class="sxs-lookup"><span data-stu-id="4b3aa-138">`DateTimeOffsetToBytesConverter` - DateTimeOffset to byte array</span></span>
* <span data-ttu-id="4b3aa-139">`DateTimeOffsetToStringConverter` -Typu DateTimeOffset mimo řetězec</span><span class="sxs-lookup"><span data-stu-id="4b3aa-139">`DateTimeOffsetToStringConverter` - DateTimeOffset to string</span></span>
* <span data-ttu-id="4b3aa-140">`DateTimeToBinaryConverter` -Datum a čas na 64 bitů hodnotu včetně DateTimeKind</span><span class="sxs-lookup"><span data-stu-id="4b3aa-140">`DateTimeToBinaryConverter` - DateTime to 64-bit value including DateTimeKind</span></span>
* <span data-ttu-id="4b3aa-141">`DateTimeToStringConverter` -Datum a čas na řetězec</span><span class="sxs-lookup"><span data-stu-id="4b3aa-141">`DateTimeToStringConverter` - DateTime to string</span></span>
* <span data-ttu-id="4b3aa-142">`DateTimeToTicksConverter` -Datum a čas k značky</span><span class="sxs-lookup"><span data-stu-id="4b3aa-142">`DateTimeToTicksConverter` - DateTime to ticks</span></span>
* <span data-ttu-id="4b3aa-143">`EnumToNumberConverter` – Enum základní číslo</span><span class="sxs-lookup"><span data-stu-id="4b3aa-143">`EnumToNumberConverter` - Enum to underlying number</span></span>
* <span data-ttu-id="4b3aa-144">`EnumToStringConverter` – Enum řetězec</span><span class="sxs-lookup"><span data-stu-id="4b3aa-144">`EnumToStringConverter` - Enum to string</span></span>
* <span data-ttu-id="4b3aa-145">`GuidToBytesConverter` -Identifikátor Guid bajtové pole</span><span class="sxs-lookup"><span data-stu-id="4b3aa-145">`GuidToBytesConverter` - Guid to byte array</span></span>
* <span data-ttu-id="4b3aa-146">`GuidToStringConverter` -Identifikátor Guid do řetězce</span><span class="sxs-lookup"><span data-stu-id="4b3aa-146">`GuidToStringConverter` - Guid to string</span></span>
* <span data-ttu-id="4b3aa-147">`NumberToBytesConverter` – Jakékoli číselné hodnoty do bajtového pole</span><span class="sxs-lookup"><span data-stu-id="4b3aa-147">`NumberToBytesConverter` - Any numerical value to byte array</span></span>
* <span data-ttu-id="4b3aa-148">`NumberToStringConverter` – Libovolná číselnou hodnotu na řetězec</span><span class="sxs-lookup"><span data-stu-id="4b3aa-148">`NumberToStringConverter` - Any numerical value to string</span></span>
* <span data-ttu-id="4b3aa-149">`StringToBytesConverter` -Řetězec, který se bajty UTF8</span><span class="sxs-lookup"><span data-stu-id="4b3aa-149">`StringToBytesConverter` - String to UTF8 bytes</span></span>
* <span data-ttu-id="4b3aa-150">`TimeSpanToStringConverter` -Časový interval na řetězec</span><span class="sxs-lookup"><span data-stu-id="4b3aa-150">`TimeSpanToStringConverter` - TimeSpan to string</span></span>
* <span data-ttu-id="4b3aa-151">`TimeSpanToTicksConverter` -Časový interval má značky</span><span class="sxs-lookup"><span data-stu-id="4b3aa-151">`TimeSpanToTicksConverter` - TimeSpan to ticks</span></span>

<span data-ttu-id="4b3aa-152">Všimněte si, že `EnumToStringConverter` je zahrnuta v tomto seznamu.</span><span class="sxs-lookup"><span data-stu-id="4b3aa-152">Notice that `EnumToStringConverter` is included in this list.</span></span> <span data-ttu-id="4b3aa-153">To znamená, že není potřeba specifikovat převod explicitně, jak je znázorněno výše.</span><span class="sxs-lookup"><span data-stu-id="4b3aa-153">This means that there is no need to specify the conversion explicitly, as shown above.</span></span> <span data-ttu-id="4b3aa-154">Místo toho použijte integrované převaděč:</span><span class="sxs-lookup"><span data-stu-id="4b3aa-154">Instead, just use the built-in converter:</span></span>
```Csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
<span data-ttu-id="4b3aa-155">Všimněte si, že všechny vestavěné převaděče bezstavové a tak jediné instance může bezpečně sdílet víc vlastností.</span><span class="sxs-lookup"><span data-stu-id="4b3aa-155">Note that all the built-in converters are stateless and so a single instance can be safely shared by multiple properties.</span></span>

## <a name="pre-defined-conversions"></a><span data-ttu-id="4b3aa-156">Předem definované převody</span><span class="sxs-lookup"><span data-stu-id="4b3aa-156">Pre-defined conversions</span></span>

<span data-ttu-id="4b3aa-157">Běžné převody, pro které existuje integrované převaděč není nutné explicitně zadat převaděč.</span><span class="sxs-lookup"><span data-stu-id="4b3aa-157">For common conversions for which a built-in converter exists there is no need to specify the converter explicitly.</span></span> <span data-ttu-id="4b3aa-158">Místo toho stačí pouze nakonfigurovat poskytovatele typu by měla sloužit a automaticky se použije odpovídající integrované převaděč EF.</span><span class="sxs-lookup"><span data-stu-id="4b3aa-158">Instead, just configure which provider type should be used and EF will automatically use the appropriate built-in converter.</span></span> <span data-ttu-id="4b3aa-159">Výčet pro převod řetězců se používají jako příklad výše, ale EF ve skutečnosti to automaticky v případě nakonfigurovaný typ poskytovatele:</span><span class="sxs-lookup"><span data-stu-id="4b3aa-159">Enum to string conversions are used as an example above, but EF will actually do this automatically if the provider type is configured:</span></span>
```Csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```
<span data-ttu-id="4b3aa-160">Totéž lze dosáhnout explicitního určení typu sloupce.</span><span class="sxs-lookup"><span data-stu-id="4b3aa-160">The same thing can be achieved by explicitly specifying the column type.</span></span> <span data-ttu-id="4b3aa-161">Například pokud je definován typ entity, jako jsou proto:</span><span class="sxs-lookup"><span data-stu-id="4b3aa-161">For example, if the entity type is defined like so:</span></span>
```Csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```
<span data-ttu-id="4b3aa-162">Potom hodnoty výčtu se uloží jako řetězce v databázi bez jakékoli další konfigurace v `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="4b3aa-162">Then the enum values will be saved as strings in the database without any further configuration in `OnModelCreating`.</span></span>

## <a name="limitations"></a><span data-ttu-id="4b3aa-163">Omezení</span><span class="sxs-lookup"><span data-stu-id="4b3aa-163">Limitations</span></span>

<span data-ttu-id="4b3aa-164">Existuje několik známých aktuální omezení systému převod hodnoty:</span><span class="sxs-lookup"><span data-stu-id="4b3aa-164">There are a few known current limitations of the value conversion system:</span></span>
* <span data-ttu-id="4b3aa-165">Jak bylo uvedeno výše, `null` nelze převést.</span><span class="sxs-lookup"><span data-stu-id="4b3aa-165">As noted above, `null` cannot be converted.</span></span>
* <span data-ttu-id="4b3aa-166">Aktuálně neexistuje žádný způsob, jak rozšířit převod jednoho vlastnosti na více sloupců nebo naopak.</span><span class="sxs-lookup"><span data-stu-id="4b3aa-166">There is currently no way to spread a conversion of one property to multiple columns or vice-versa.</span></span>
* <span data-ttu-id="4b3aa-167">Použití převody hodnot může mít vliv na schopnost EF Core překlad výrazů jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="4b3aa-167">Use of value conversions may impact the ability of EF Core to translate expressions to SQL.</span></span> <span data-ttu-id="4b3aa-168">Upozornění se protokolují pro tyto případy.</span><span class="sxs-lookup"><span data-stu-id="4b3aa-168">A warning will be logged for such cases.</span></span>
<span data-ttu-id="4b3aa-169">Odebrání těchto omezení se považují za pro budoucí verzi.</span><span class="sxs-lookup"><span data-stu-id="4b3aa-169">Removal of these limitations is being considered for a future release.</span></span>
