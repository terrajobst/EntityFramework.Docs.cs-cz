---
title: Převody hodnot - EF jádra
author: ajcvickers
ms.author: divega
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/value-conversions
ms.openlocfilehash: 3e97c05a87ad9b4817c03f446031ea6c74704f5b
ms.sourcegitcommit: 605e42232854ce44bae09624a6eebc35b8e2473b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/16/2018
ms.locfileid: "34191113"
---
# <a name="value-conversions"></a><span data-ttu-id="5b095-102">Převody hodnot</span><span class="sxs-lookup"><span data-stu-id="5b095-102">Value Conversions</span></span>

> [!NOTE]  
> <span data-ttu-id="5b095-103">Tato funkce je nového v EF základní 2.1.</span><span class="sxs-lookup"><span data-stu-id="5b095-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="5b095-104">Převodníky hodnot povolit hodnoty vlastností, které má být převeden při čtení nebo zápisu do databáze.</span><span class="sxs-lookup"><span data-stu-id="5b095-104">Value converters allow property values to be converted when reading from or writing to the database.</span></span> <span data-ttu-id="5b095-105">Tento převod může být z jednu hodnotu do jiné stejného typu (například šifrování řetězce), nebo z hodnoty jednoho typu na hodnotu jiného typu (například převod hodnoty výčtu do a z řetězce v databázi.)</span><span class="sxs-lookup"><span data-stu-id="5b095-105">This conversion can be from one value to another of the same type (for example, encrypting strings) or from a value of one type to a value of another type (for example, converting enum values to and from strings in the database.)</span></span>

## <a name="fundamentals"></a><span data-ttu-id="5b095-106">Základy</span><span class="sxs-lookup"><span data-stu-id="5b095-106">Fundamentals</span></span>

<span data-ttu-id="5b095-107">Převodníky hodnot nejsou zadány z hlediska `ModelClrType` a `ProviderClrType`.</span><span class="sxs-lookup"><span data-stu-id="5b095-107">Value converters are specified in terms of a `ModelClrType` and a `ProviderClrType`.</span></span> <span data-ttu-id="5b095-108">Typ modelu je typ formátu .NET vlastnosti v typu entity.</span><span class="sxs-lookup"><span data-stu-id="5b095-108">The model type is the .NET type of the property in the entity type.</span></span> <span data-ttu-id="5b095-109">Typ zprostředkovatele je typ formátu .NET rozumí zprostředkovatelem databáze.</span><span class="sxs-lookup"><span data-stu-id="5b095-109">The provider type is the .NET type understood by the database provider.</span></span> <span data-ttu-id="5b095-110">Například uložit výčty jako řetězce v databázi, typu modelu je typ výčtového typu, a je typ poskytovatele `String`.</span><span class="sxs-lookup"><span data-stu-id="5b095-110">For example, to save enums as strings in the database, the model type is the type of the enum, and the provider type is `String`.</span></span> <span data-ttu-id="5b095-111">Tyto dva typy může být stejné.</span><span class="sxs-lookup"><span data-stu-id="5b095-111">These two types can be the same.</span></span>

<span data-ttu-id="5b095-112">Převody jsou definovány pomocí dvou `Func` stromů výrazů: jeden z `ModelClrType` k `ProviderClrType` a dalších z `ProviderClrType` k `ModelClrType`.</span><span class="sxs-lookup"><span data-stu-id="5b095-112">Conversions are defined using two `Func` expression trees: one from `ModelClrType` to `ProviderClrType` and the other from `ProviderClrType` to `ModelClrType`.</span></span> <span data-ttu-id="5b095-113">Stromy výrazů se používají tak, aby mohou být zkompilovány do kódu přístup k databázi pro efektivní převody.</span><span class="sxs-lookup"><span data-stu-id="5b095-113">Expression trees are used so that they can be compiled into the database access code for efficient conversions.</span></span> <span data-ttu-id="5b095-114">Pro komplexní převody strom výrazu může být jednoduché volání metody, která provádí převod.</span><span class="sxs-lookup"><span data-stu-id="5b095-114">For complex conversions, the expression tree may be a simple call to a method that performs the conversion.</span></span>

## <a name="configuring-a-value-converter"></a><span data-ttu-id="5b095-115">Převaděč hodnoty konfigurace</span><span class="sxs-lookup"><span data-stu-id="5b095-115">Configuring a value converter</span></span>

<span data-ttu-id="5b095-116">Převody hodnot jsou definovány na vlastnosti v OnModelCreating z vaší DbContext.</span><span class="sxs-lookup"><span data-stu-id="5b095-116">Value conversions are defined on properties in the OnModelCreating of your DbContext.</span></span> <span data-ttu-id="5b095-117">Představte si třeba definovaný jako typ výčtu a entity:</span><span class="sxs-lookup"><span data-stu-id="5b095-117">For example, consider an enum and entity type defined as:</span></span>
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
<span data-ttu-id="5b095-118">Pak lze definovat převody v OnModelCreating ukládání hodnot výčtu jako řetězce (např.) "Oslů", "Mul",...) v databázi:</span><span class="sxs-lookup"><span data-stu-id="5b095-118">Then conversions can be defined in OnModelCreating to store the enum values as strings (e.g. "Donkey", "Mule", ...) in the database:</span></span>
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
> <span data-ttu-id="5b095-119">A `null` hodnota se nikdy předá převaděč hodnoty.</span><span class="sxs-lookup"><span data-stu-id="5b095-119">A `null` value will never be passed to a value converter.</span></span> <span data-ttu-id="5b095-120">To usnadňuje provádění převody a umožňuje jejich sdílení mezi vlastnosti s možnou hodnotou Null a neumožňuje hodnotu Null.</span><span class="sxs-lookup"><span data-stu-id="5b095-120">This makes the implementation of conversions easier and allows them to be shared amongst nullable and non-nullable properties.</span></span>

## <a name="the-valueconverter-class"></a><span data-ttu-id="5b095-121">ValueConverter – třída</span><span class="sxs-lookup"><span data-stu-id="5b095-121">The ValueConverter class</span></span>

<span data-ttu-id="5b095-122">Volání metody `HasConversion` jako v příkladu nahoře, vytvoří `ValueConverter` instance a nastavte ji na vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="5b095-122">Calling `HasConversion` as shown above will create a `ValueConverter` instance and set it on the property.</span></span> <span data-ttu-id="5b095-123">`ValueConverter` Lze vytvořit místo explicitně.</span><span class="sxs-lookup"><span data-stu-id="5b095-123">The `ValueConverter` can instead be created explicitly.</span></span> <span data-ttu-id="5b095-124">Příklad:</span><span class="sxs-lookup"><span data-stu-id="5b095-124">For example:</span></span>
```Csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
<span data-ttu-id="5b095-125">To může být užitečné, když používají stejné převod více vlastností.</span><span class="sxs-lookup"><span data-stu-id="5b095-125">This can be useful when multiple properties use the same conversion.</span></span>

> [!NOTE]  
> <span data-ttu-id="5b095-126">Aktuálně neexistuje žádný způsob, jak určit na jednom místě, že všechny vlastnosti daného typu musí používat stejné převaděč hodnoty.</span><span class="sxs-lookup"><span data-stu-id="5b095-126">There is currently no way to specify in one place that every property of a given type must use the same value converter.</span></span> <span data-ttu-id="5b095-127">Tato funkce bude považovat za pro budoucí použití.</span><span class="sxs-lookup"><span data-stu-id="5b095-127">This feature will be considered for a future release.</span></span>

## <a name="built-in-converters"></a><span data-ttu-id="5b095-128">Vestavěné převaděče</span><span class="sxs-lookup"><span data-stu-id="5b095-128">Built-in converters</span></span>

<span data-ttu-id="5b095-129">Předem definované EF základní dodává se sadou `ValueConverter` třídy, které jsou součástí `Microsoft.EntityFrameworkCore.Storage.ValueConversion` oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="5b095-129">EF Core ships with a set of pre-defined `ValueConverter` classes, found in the `Microsoft.EntityFrameworkCore.Storage.ValueConversion` namespace.</span></span> <span data-ttu-id="5b095-130">Ty jsou:</span><span class="sxs-lookup"><span data-stu-id="5b095-130">These are:</span></span>
* <span data-ttu-id="5b095-131">`BoolToZeroOneConverter` – Bool na 0 a 1</span><span class="sxs-lookup"><span data-stu-id="5b095-131">`BoolToZeroOneConverter` - Bool to zero and one</span></span>
* <span data-ttu-id="5b095-132">`BoolToStringConverter` – Bool do řetězců, jako je například "Y" a "N"</span><span class="sxs-lookup"><span data-stu-id="5b095-132">`BoolToStringConverter` - Bool to strings such as "Y" and "N"</span></span>
* <span data-ttu-id="5b095-133">`BoolToTwoValuesConverter` – Bool jakékoli dvě hodnoty</span><span class="sxs-lookup"><span data-stu-id="5b095-133">`BoolToTwoValuesConverter` - Bool to any two values</span></span>
* <span data-ttu-id="5b095-134">`BytesToStringConverter` -Bajtové pole pro řetězec s kódováním Base64</span><span class="sxs-lookup"><span data-stu-id="5b095-134">`BytesToStringConverter` - Byte array to Base64-encoded string</span></span>
* <span data-ttu-id="5b095-135">`CastingConverter` – Převody, které vyžadují jenom přetypování Csharp</span><span class="sxs-lookup"><span data-stu-id="5b095-135">`CastingConverter` - Conversions that require only a Csharp cast</span></span>
* <span data-ttu-id="5b095-136">`CharToStringConverter` -Char jednoho znaku řetězec</span><span class="sxs-lookup"><span data-stu-id="5b095-136">`CharToStringConverter` - Char to single character string</span></span>
* <span data-ttu-id="5b095-137">`DateTimeOffsetToBinaryConverter` – DateTimeOffset kódováním binární hodnotu 64-bit</span><span class="sxs-lookup"><span data-stu-id="5b095-137">`DateTimeOffsetToBinaryConverter` - DateTimeOffset to binary-encoded 64-bit value</span></span>
* <span data-ttu-id="5b095-138">`DateTimeOffsetToBytesConverter` – DateTimeOffset do bajtového pole</span><span class="sxs-lookup"><span data-stu-id="5b095-138">`DateTimeOffsetToBytesConverter` - DateTimeOffset to byte array</span></span>
* <span data-ttu-id="5b095-139">`DateTimeOffsetToStringConverter` – DateTimeOffset řetězec</span><span class="sxs-lookup"><span data-stu-id="5b095-139">`DateTimeOffsetToStringConverter` - DateTimeOffset to string</span></span>
* <span data-ttu-id="5b095-140">`DateTimeToBinaryConverter` -Datum a čas 64 bitů hodnotu včetně parametr DateTimeKind</span><span class="sxs-lookup"><span data-stu-id="5b095-140">`DateTimeToBinaryConverter` - DateTime to 64-bit value including DateTimeKind</span></span>
* <span data-ttu-id="5b095-141">`DateTimeToStringConverter` -Datum a čas na řetězec</span><span class="sxs-lookup"><span data-stu-id="5b095-141">`DateTimeToStringConverter` - DateTime to string</span></span>
* <span data-ttu-id="5b095-142">`DateTimeToTicksConverter` -Datum a čas k rysky</span><span class="sxs-lookup"><span data-stu-id="5b095-142">`DateTimeToTicksConverter` - DateTime to ticks</span></span>
* <span data-ttu-id="5b095-143">`EnumToNumberConverter` – Enum základní číslo</span><span class="sxs-lookup"><span data-stu-id="5b095-143">`EnumToNumberConverter` - Enum to underlying number</span></span>
* <span data-ttu-id="5b095-144">`EnumToStringConverter` – Enum řetězec</span><span class="sxs-lookup"><span data-stu-id="5b095-144">`EnumToStringConverter` - Enum to string</span></span>
* <span data-ttu-id="5b095-145">`GuidToBytesConverter` -Guid do bajtového pole</span><span class="sxs-lookup"><span data-stu-id="5b095-145">`GuidToBytesConverter` - Guid to byte array</span></span>
* <span data-ttu-id="5b095-146">`GuidToStringConverter` -Guid do řetězce</span><span class="sxs-lookup"><span data-stu-id="5b095-146">`GuidToStringConverter` - Guid to string</span></span>
* <span data-ttu-id="5b095-147">`NumberToBytesConverter` -Jakékoli číselné hodnoty do bajtového pole</span><span class="sxs-lookup"><span data-stu-id="5b095-147">`NumberToBytesConverter` - Any numerical value to byte array</span></span>
* <span data-ttu-id="5b095-148">`NumberToStringConverter` -Všechny číselnou hodnotu na řetězec</span><span class="sxs-lookup"><span data-stu-id="5b095-148">`NumberToStringConverter` - Any numerical value to string</span></span>
* <span data-ttu-id="5b095-149">`StringToBytesConverter` -Řetězce UTF8 bajtů</span><span class="sxs-lookup"><span data-stu-id="5b095-149">`StringToBytesConverter` - String to UTF8 bytes</span></span>
* <span data-ttu-id="5b095-150">`TimeSpanToStringConverter` -Časový interval na řetězec</span><span class="sxs-lookup"><span data-stu-id="5b095-150">`TimeSpanToStringConverter` - TimeSpan to string</span></span>
* <span data-ttu-id="5b095-151">`TimeSpanToTicksConverter` -Časový interval na dílky</span><span class="sxs-lookup"><span data-stu-id="5b095-151">`TimeSpanToTicksConverter` - TimeSpan to ticks</span></span>

<span data-ttu-id="5b095-152">Všimněte si, že `EnumToStringConverter` je zahrnuta v tomto seznamu.</span><span class="sxs-lookup"><span data-stu-id="5b095-152">Notice that `EnumToStringConverter` is included in this list.</span></span> <span data-ttu-id="5b095-153">To znamená, že je nutné specifikovat převod explicitně, jak je uvedeno výše.</span><span class="sxs-lookup"><span data-stu-id="5b095-153">This means that there is no need to specify the conversion explicitly, as shown above.</span></span> <span data-ttu-id="5b095-154">Místo toho použijte předdefinovaný převaděč:</span><span class="sxs-lookup"><span data-stu-id="5b095-154">Instead, just use the built-in converter:</span></span>
```Csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
<span data-ttu-id="5b095-155">Upozorňujeme, že jsou všechny vestavěné převaděče bezstavové a tak jediné instance lze bezpečně sdílet mezi více vlastností.</span><span class="sxs-lookup"><span data-stu-id="5b095-155">Note that all the built-in converters are stateless and so a single instance can be safely shared by multiple properties.</span></span>

## <a name="pre-defined-conversions"></a><span data-ttu-id="5b095-156">Předem definované převody</span><span class="sxs-lookup"><span data-stu-id="5b095-156">Pre-defined conversions</span></span>

<span data-ttu-id="5b095-157">Pro běžné převody, pro které existuje předdefinované převaděč není nutné explicitně zadat převaděč.</span><span class="sxs-lookup"><span data-stu-id="5b095-157">For common conversions for which a built-in converter exists there is no need to specify the converter explicitly.</span></span> <span data-ttu-id="5b095-158">Místo toho stačí pouze nakonfigurovat které typ zprostředkovatele má být použit a EF budou automaticky používat odpovídající sestavení převaděč.</span><span class="sxs-lookup"><span data-stu-id="5b095-158">Instead, just configure which provider type should be used and EF will automatically use the appropriate build-in converter.</span></span> <span data-ttu-id="5b095-159">Enum pro převod řetězců slouží jako příklad výše, ale EF ve skutečnosti to automaticky v případě nastaven typ poskytovatele:</span><span class="sxs-lookup"><span data-stu-id="5b095-159">Enum to string conversions are used as an example above, but EF will actually do this automatically if the provider type is configured:</span></span>
```Csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```
<span data-ttu-id="5b095-160">Samé lze dosáhnout explicitním zadáním typ sloupce.</span><span class="sxs-lookup"><span data-stu-id="5b095-160">The same thing can be achieved by explicitly specifying the column type.</span></span> <span data-ttu-id="5b095-161">Například pokud je definován typ entity typu tak:</span><span class="sxs-lookup"><span data-stu-id="5b095-161">For example, if the entity type is defined like so:</span></span>
```Csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```
<span data-ttu-id="5b095-162">Hodnoty výčtu pak bude uložena jako řetězce v databázi bez další konfigurace v OnModelCreating.</span><span class="sxs-lookup"><span data-stu-id="5b095-162">Then the enum values will be saved as strings in the database without any further configuration in OnModelCreating.</span></span>

## <a name="limitations"></a><span data-ttu-id="5b095-163">Omezení</span><span class="sxs-lookup"><span data-stu-id="5b095-163">Limitations</span></span>

<span data-ttu-id="5b095-164">Existuje několik známá aktuální omezení systému převod hodnoty:</span><span class="sxs-lookup"><span data-stu-id="5b095-164">There are a few known current limitations of the value convertion system:</span></span>
* <span data-ttu-id="5b095-165">Jak jsme uvedli výše, `null` nelze převést.</span><span class="sxs-lookup"><span data-stu-id="5b095-165">As noted above, `null` cannot be converted.</span></span>
* <span data-ttu-id="5b095-166">Aktuálně neexistuje žádný způsob, jak šíří převodu z jedné vlastnosti do více sloupců nebo naopak.</span><span class="sxs-lookup"><span data-stu-id="5b095-166">There is currently no way to spread a conversion of one property to multiple columns or vice-versa.</span></span>
* <span data-ttu-id="5b095-167">Použití převody hodnot může mít vliv na schopnost EF základní převede výrazy jazyka SQL.</span><span class="sxs-lookup"><span data-stu-id="5b095-167">Use of value conversions may impact the ability of EF Core to translate expressions to SQL.</span></span> <span data-ttu-id="5b095-168">Upozornění bude protokolována takových případech.</span><span class="sxs-lookup"><span data-stu-id="5b095-168">A warning will be logged for such cases.</span></span>
<span data-ttu-id="5b095-169">Odebrání těchto omezení se považuje za pro budoucí použití.</span><span class="sxs-lookup"><span data-stu-id="5b095-169">Removal of these limitations is being considered for a future release.</span></span>
