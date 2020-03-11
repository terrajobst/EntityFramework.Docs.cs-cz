---
title: Převody hodnot – EF Core
author: ajcvickers
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/value-conversions
ms.openlocfilehash: 93774bc1bc3887f982faeac151825a6643c1107c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417200"
---
# <a name="value-conversions"></a><span data-ttu-id="3b9df-102">Převody hodnot</span><span class="sxs-lookup"><span data-stu-id="3b9df-102">Value Conversions</span></span>

> [!NOTE]  
> <span data-ttu-id="3b9df-103">Tato funkce je v EF Core 2,1 novinkou.</span><span class="sxs-lookup"><span data-stu-id="3b9df-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="3b9df-104">Převaděče hodnot umožňují převod hodnot vlastností při čtení nebo zápisu do databáze.</span><span class="sxs-lookup"><span data-stu-id="3b9df-104">Value converters allow property values to be converted when reading from or writing to the database.</span></span> <span data-ttu-id="3b9df-105">Tento převod může být z jedné hodnoty na jiný stejného typu (například šifrovací řetězce) nebo z hodnoty jednoho typu na hodnotu jiného typu (například převod hodnot výčtu na a z řetězců v databázi.)</span><span class="sxs-lookup"><span data-stu-id="3b9df-105">This conversion can be from one value to another of the same type (for example, encrypting strings) or from a value of one type to a value of another type (for example, converting enum values to and from strings in the database.)</span></span>

## <a name="fundamentals"></a><span data-ttu-id="3b9df-106">Základy</span><span class="sxs-lookup"><span data-stu-id="3b9df-106">Fundamentals</span></span>

<span data-ttu-id="3b9df-107">Převaděče hodnot jsou zadány na základě `ModelClrType` a `ProviderClrType`.</span><span class="sxs-lookup"><span data-stu-id="3b9df-107">Value converters are specified in terms of a `ModelClrType` and a `ProviderClrType`.</span></span> <span data-ttu-id="3b9df-108">Typ modelu je typ .NET vlastnosti v typu entity.</span><span class="sxs-lookup"><span data-stu-id="3b9df-108">The model type is the .NET type of the property in the entity type.</span></span> <span data-ttu-id="3b9df-109">Typ zprostředkovatele je typ .NET, který rozumí poskytovatel databáze.</span><span class="sxs-lookup"><span data-stu-id="3b9df-109">The provider type is the .NET type understood by the database provider.</span></span> <span data-ttu-id="3b9df-110">Chcete-li například uložit výčty jako řetězce v databázi, typ modelu je typ výčtu a typ poskytovatele `String`.</span><span class="sxs-lookup"><span data-stu-id="3b9df-110">For example, to save enums as strings in the database, the model type is the type of the enum, and the provider type is `String`.</span></span> <span data-ttu-id="3b9df-111">Tyto dva typy mohou být stejné.</span><span class="sxs-lookup"><span data-stu-id="3b9df-111">These two types can be the same.</span></span>

<span data-ttu-id="3b9df-112">Převody jsou definovány pomocí dvou `Func` stromů výrazů: jeden z `ModelClrType` na `ProviderClrType` a druhý od `ProviderClrType` až po `ModelClrType`.</span><span class="sxs-lookup"><span data-stu-id="3b9df-112">Conversions are defined using two `Func` expression trees: one from `ModelClrType` to `ProviderClrType` and the other from `ProviderClrType` to `ModelClrType`.</span></span> <span data-ttu-id="3b9df-113">Stromy výrazů se používají tak, aby je bylo možné kompilovat do kódu přístupu k databázi pro efektivní převody.</span><span class="sxs-lookup"><span data-stu-id="3b9df-113">Expression trees are used so that they can be compiled into the database access code for efficient conversions.</span></span> <span data-ttu-id="3b9df-114">U složitých převodů může být strom výrazů jednoduchým voláním metody, která provádí převod.</span><span class="sxs-lookup"><span data-stu-id="3b9df-114">For complex conversions, the expression tree may be a simple call to a method that performs the conversion.</span></span>

## <a name="configuring-a-value-converter"></a><span data-ttu-id="3b9df-115">Konfigurace převaděče hodnot</span><span class="sxs-lookup"><span data-stu-id="3b9df-115">Configuring a value converter</span></span>

<span data-ttu-id="3b9df-116">Převody hodnot jsou definovány ve vlastnostech `OnModelCreating` `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="3b9df-116">Value conversions are defined on properties in the `OnModelCreating` of your `DbContext`.</span></span> <span data-ttu-id="3b9df-117">Představte si například výčet a typ entity definovaný jako:</span><span class="sxs-lookup"><span data-stu-id="3b9df-117">For example, consider an enum and entity type defined as:</span></span>

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

<span data-ttu-id="3b9df-118">Pak lze převody definovat v `OnModelCreating` pro uložení hodnot výčtu jako řetězců (například "Donkey", "Mule",...) v databázi:</span><span class="sxs-lookup"><span data-stu-id="3b9df-118">Then conversions can be defined in `OnModelCreating` to store the enum values as strings (for example, "Donkey", "Mule", ...) in the database:</span></span>

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
> <span data-ttu-id="3b9df-119">Hodnota `null` nebude nikdy předána převaděči hodnot.</span><span class="sxs-lookup"><span data-stu-id="3b9df-119">A `null` value will never be passed to a value converter.</span></span> <span data-ttu-id="3b9df-120">Díky tomu je implementace převodů jednodušší a umožňuje, aby se sdílely mezi vlastnosti s možnou hodnotou null a nepovolující hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="3b9df-120">This makes the implementation of conversions easier and allows them to be shared amongst nullable and non-nullable properties.</span></span>

## <a name="the-valueconverter-class"></a><span data-ttu-id="3b9df-121">Třída ValueConverter</span><span class="sxs-lookup"><span data-stu-id="3b9df-121">The ValueConverter class</span></span>

<span data-ttu-id="3b9df-122">Volání `HasConversion` jak je uvedeno výše, vytvoří instanci `ValueConverter` a nastaví ji na vlastnost.</span><span class="sxs-lookup"><span data-stu-id="3b9df-122">Calling `HasConversion` as shown above will create a `ValueConverter` instance and set it on the property.</span></span> <span data-ttu-id="3b9df-123">Místo toho `ValueConverter` lze vytvořit explicitně.</span><span class="sxs-lookup"><span data-stu-id="3b9df-123">The `ValueConverter` can instead be created explicitly.</span></span> <span data-ttu-id="3b9df-124">Příklad:</span><span class="sxs-lookup"><span data-stu-id="3b9df-124">For example:</span></span>

``` csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```

<span data-ttu-id="3b9df-125">To může být užitečné, pokud více vlastností používá stejný převod.</span><span class="sxs-lookup"><span data-stu-id="3b9df-125">This can be useful when multiple properties use the same conversion.</span></span>

> [!NOTE]  
> <span data-ttu-id="3b9df-126">V současné době neexistuje způsob, jak zadat na jednom místě, kde musí každá vlastnost daného typu používat stejný konvertor hodnoty.</span><span class="sxs-lookup"><span data-stu-id="3b9df-126">There is currently no way to specify in one place that every property of a given type must use the same value converter.</span></span> <span data-ttu-id="3b9df-127">Tato funkce se bude považovat za budoucí verzi.</span><span class="sxs-lookup"><span data-stu-id="3b9df-127">This feature will be considered for a future release.</span></span>

## <a name="built-in-converters"></a><span data-ttu-id="3b9df-128">Předdefinované převaděče</span><span class="sxs-lookup"><span data-stu-id="3b9df-128">Built-in converters</span></span>

<span data-ttu-id="3b9df-129">EF Core se dodává se sadou předem definovaných `ValueConverter` tříd, která se nachází v oboru názvů `Microsoft.EntityFrameworkCore.Storage.ValueConversion`.</span><span class="sxs-lookup"><span data-stu-id="3b9df-129">EF Core ships with a set of pre-defined `ValueConverter` classes, found in the `Microsoft.EntityFrameworkCore.Storage.ValueConversion` namespace.</span></span> <span data-ttu-id="3b9df-130">Jsou to:</span><span class="sxs-lookup"><span data-stu-id="3b9df-130">These are:</span></span>

* <span data-ttu-id="3b9df-131">`BoolToZeroOneConverter` – bool na nulu a jeden</span><span class="sxs-lookup"><span data-stu-id="3b9df-131">`BoolToZeroOneConverter` - Bool to zero and one</span></span>
* <span data-ttu-id="3b9df-132">`BoolToStringConverter` – bool pro řetězce, jako například "Y" a "N"</span><span class="sxs-lookup"><span data-stu-id="3b9df-132">`BoolToStringConverter` - Bool to strings such as "Y" and "N"</span></span>
* <span data-ttu-id="3b9df-133">`BoolToTwoValuesConverter` – bool pro libovolné dvě hodnoty</span><span class="sxs-lookup"><span data-stu-id="3b9df-133">`BoolToTwoValuesConverter` - Bool to any two values</span></span>
* <span data-ttu-id="3b9df-134">`BytesToStringConverter` bajtové pole na řetězec kódovaný v kódování Base64</span><span class="sxs-lookup"><span data-stu-id="3b9df-134">`BytesToStringConverter` - Byte array to Base64-encoded string</span></span>
* <span data-ttu-id="3b9df-135">`CastingConverter` – převody, které vyžadují pouze přetypování typu</span><span class="sxs-lookup"><span data-stu-id="3b9df-135">`CastingConverter` - Conversions that require only a type cast</span></span>
* <span data-ttu-id="3b9df-136">`CharToStringConverter`-char do řetězce s jedním znakem</span><span class="sxs-lookup"><span data-stu-id="3b9df-136">`CharToStringConverter` - Char to single character string</span></span>
* <span data-ttu-id="3b9df-137">`DateTimeOffsetToBinaryConverter`-DateTimeOffset až do binární hodnoty s kódováním 64</span><span class="sxs-lookup"><span data-stu-id="3b9df-137">`DateTimeOffsetToBinaryConverter` - DateTimeOffset to binary-encoded 64-bit value</span></span>
* <span data-ttu-id="3b9df-138">pole `DateTimeOffsetToBytesConverter`-DateTimeOffset až bajtů</span><span class="sxs-lookup"><span data-stu-id="3b9df-138">`DateTimeOffsetToBytesConverter` - DateTimeOffset to byte array</span></span>
* <span data-ttu-id="3b9df-139">`DateTimeOffsetToStringConverter`-DateTimeOffset do řetězce</span><span class="sxs-lookup"><span data-stu-id="3b9df-139">`DateTimeOffsetToStringConverter` - DateTimeOffset to string</span></span>
* <span data-ttu-id="3b9df-140">`DateTimeToBinaryConverter`-hodnota DateTime až 64, včetně DateTimeKind</span><span class="sxs-lookup"><span data-stu-id="3b9df-140">`DateTimeToBinaryConverter` - DateTime to 64-bit value including DateTimeKind</span></span>
* <span data-ttu-id="3b9df-141">`DateTimeToStringConverter`-DateTime do řetězce</span><span class="sxs-lookup"><span data-stu-id="3b9df-141">`DateTimeToStringConverter` - DateTime to string</span></span>
* <span data-ttu-id="3b9df-142">`DateTimeToTicksConverter`-DateTime na takty</span><span class="sxs-lookup"><span data-stu-id="3b9df-142">`DateTimeToTicksConverter` - DateTime to ticks</span></span>
* <span data-ttu-id="3b9df-143">`EnumToNumberConverter` – výčet pro základní číslo</span><span class="sxs-lookup"><span data-stu-id="3b9df-143">`EnumToNumberConverter` - Enum to underlying number</span></span>
* <span data-ttu-id="3b9df-144">`EnumToStringConverter` – výčet do řetězce</span><span class="sxs-lookup"><span data-stu-id="3b9df-144">`EnumToStringConverter` - Enum to string</span></span>
* <span data-ttu-id="3b9df-145">`GuidToBytesConverter` – identifikátor GUID pro pole bajtů</span><span class="sxs-lookup"><span data-stu-id="3b9df-145">`GuidToBytesConverter` - Guid to byte array</span></span>
* <span data-ttu-id="3b9df-146">`GuidToStringConverter`-GUID k řetězci</span><span class="sxs-lookup"><span data-stu-id="3b9df-146">`GuidToStringConverter` - Guid to string</span></span>
* <span data-ttu-id="3b9df-147">`NumberToBytesConverter` – jakákoli číselná hodnota k poli bajtů</span><span class="sxs-lookup"><span data-stu-id="3b9df-147">`NumberToBytesConverter` - Any numerical value to byte array</span></span>
* <span data-ttu-id="3b9df-148">`NumberToStringConverter` – jakákoli číselná hodnota k řetězci</span><span class="sxs-lookup"><span data-stu-id="3b9df-148">`NumberToStringConverter` - Any numerical value to string</span></span>
* <span data-ttu-id="3b9df-149">`StringToBytesConverter` – řetězec na bajty UTF8</span><span class="sxs-lookup"><span data-stu-id="3b9df-149">`StringToBytesConverter` - String to UTF8 bytes</span></span>
* <span data-ttu-id="3b9df-150">`TimeSpanToStringConverter`-TimeSpan do řetězce</span><span class="sxs-lookup"><span data-stu-id="3b9df-150">`TimeSpanToStringConverter` - TimeSpan to string</span></span>
* <span data-ttu-id="3b9df-151">`TimeSpanToTicksConverter`-TimeSpan k taktům</span><span class="sxs-lookup"><span data-stu-id="3b9df-151">`TimeSpanToTicksConverter` - TimeSpan to ticks</span></span>

<span data-ttu-id="3b9df-152">Všimněte si, že `EnumToStringConverter` je součástí tohoto seznamu.</span><span class="sxs-lookup"><span data-stu-id="3b9df-152">Notice that `EnumToStringConverter` is included in this list.</span></span> <span data-ttu-id="3b9df-153">To znamená, že není nutné explicitně zadávat převod, jak je uvedeno výše.</span><span class="sxs-lookup"><span data-stu-id="3b9df-153">This means that there is no need to specify the conversion explicitly, as shown above.</span></span> <span data-ttu-id="3b9df-154">Místo toho stačí použít vestavěný převaděč:</span><span class="sxs-lookup"><span data-stu-id="3b9df-154">Instead, just use the built-in converter:</span></span>

``` csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```

<span data-ttu-id="3b9df-155">Všimněte si, že všechny předdefinované převaděče jsou bezstavové, takže jednu instanci lze bezpečně sdílet pomocí více vlastností.</span><span class="sxs-lookup"><span data-stu-id="3b9df-155">Note that all the built-in converters are stateless and so a single instance can be safely shared by multiple properties.</span></span>

## <a name="pre-defined-conversions"></a><span data-ttu-id="3b9df-156">Předem definované převody</span><span class="sxs-lookup"><span data-stu-id="3b9df-156">Pre-defined conversions</span></span>

<span data-ttu-id="3b9df-157">Pro běžné převody, pro které existuje vestavěný převaděč, není nutné explicitně zadávat převaděč.</span><span class="sxs-lookup"><span data-stu-id="3b9df-157">For common conversions for which a built-in converter exists there is no need to specify the converter explicitly.</span></span> <span data-ttu-id="3b9df-158">Místo toho stačí nakonfigurovat, který typ poskytovatele má být použit, a EF bude automaticky používat odpovídající vestavěný převaděč.</span><span class="sxs-lookup"><span data-stu-id="3b9df-158">Instead, just configure which provider type should be used and EF will automatically use the appropriate built-in converter.</span></span> <span data-ttu-id="3b9df-159">Výčty na převody řetězců se používají jako příklad výše, ale EF to vlastně provede automaticky, pokud je nakonfigurovaný typ zprostředkovatele:</span><span class="sxs-lookup"><span data-stu-id="3b9df-159">Enum to string conversions are used as an example above, but EF will actually do this automatically if the provider type is configured:</span></span>

``` csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```

<span data-ttu-id="3b9df-160">Stejné věci je možné dosáhnout explicitním určením typu sloupce.</span><span class="sxs-lookup"><span data-stu-id="3b9df-160">The same thing can be achieved by explicitly specifying the column type.</span></span> <span data-ttu-id="3b9df-161">Například pokud je typ entity definovaný jako příklad:</span><span class="sxs-lookup"><span data-stu-id="3b9df-161">For example, if the entity type is defined like so:</span></span>

``` csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```

<span data-ttu-id="3b9df-162">Hodnoty výčtu pak budou uloženy jako řetězce v databázi bez další konfigurace v `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="3b9df-162">Then the enum values will be saved as strings in the database without any further configuration in `OnModelCreating`.</span></span>

## <a name="limitations"></a><span data-ttu-id="3b9df-163">Omezení</span><span class="sxs-lookup"><span data-stu-id="3b9df-163">Limitations</span></span>

<span data-ttu-id="3b9df-164">K dispozici je několik známých omezení systému převodu hodnot:</span><span class="sxs-lookup"><span data-stu-id="3b9df-164">There are a few known current limitations of the value conversion system:</span></span>

* <span data-ttu-id="3b9df-165">Jak bylo uvedeno výše, `null` nelze převést.</span><span class="sxs-lookup"><span data-stu-id="3b9df-165">As noted above, `null` cannot be converted.</span></span>
* <span data-ttu-id="3b9df-166">V současné době neexistuje způsob, jak rozložit převod jedné vlastnosti do více sloupců nebo naopak.</span><span class="sxs-lookup"><span data-stu-id="3b9df-166">There is currently no way to spread a conversion of one property to multiple columns or vice-versa.</span></span>
* <span data-ttu-id="3b9df-167">Použití převodů hodnot může mít vliv na schopnost EF Core přeložit výrazy do jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="3b9df-167">Use of value conversions may impact the ability of EF Core to translate expressions to SQL.</span></span> <span data-ttu-id="3b9df-168">V takových případech bude zaznamenáno upozornění.</span><span class="sxs-lookup"><span data-stu-id="3b9df-168">A warning will be logged for such cases.</span></span>
<span data-ttu-id="3b9df-169">Odebrání těchto omezení se považuje za budoucí verzi.</span><span class="sxs-lookup"><span data-stu-id="3b9df-169">Removal of these limitations is being considered for a future release.</span></span>
