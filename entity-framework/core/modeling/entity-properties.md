---
title: Vlastnosti entity – EF Core
description: Postup konfigurace a mapování vlastností entity pomocí Entity Framework Core
author: roji
ms.date: 12/10/2019
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/entity-properties
ms.openlocfilehash: b67603fbffd1f1c8506bc21f8972c851eb8eef29
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502465"
---
# <a name="entity-properties"></a>Vlastnosti entity

Každý typ entity v modelu má sadu vlastností, které EF Core budou číst a zapisovat z databáze. Pokud používáte relační databázi, vlastnosti entity se mapují na sloupce tabulky.

## <a name="included-and-excluded-properties"></a>Zahrnuté a vyloučené vlastnosti

Podle konvence budou v modelu zahrnuty všechny veřejné vlastnosti s metodami getter a setter.

Konkrétní vlastnosti lze vyloučit následujícím způsobem:

### <a name="data-annotationstabdata-annotations"></a>[Datové poznámky](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreProperty.cs?name=IgnoreProperty&highlight=6)]

### <a name="fluent-apitabfluent-api"></a>[Rozhraní Fluent API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreProperty.cs?name=IgnoreProperty&highlight=3,4)]

***

## <a name="column-names"></a>Názvy sloupců

Podle konvence při použití relační databáze jsou vlastnosti entit namapovány na sloupce tabulky se stejným názvem jako vlastnost.

Pokud chcete nakonfigurovat sloupce s různými názvy, můžete tak učinit následujícím způsobem:

### <a name="data-annotationstabdata-annotations"></a>[Datové poznámky](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ColumnName.cs?Name=ColumnName&highlight=3)]

### <a name="fluent-apitabfluent-api"></a>[Rozhraní Fluent API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ColumnName.cs?Name=ColumnName&highlight=3-5)]

***

## <a name="column-data-types"></a>Datové typy sloupců

Při použití relační databáze poskytovatel databáze vybere datový typ založený na typu rozhraní .NET vlastnosti. Také vezme v úvahu jiná metadata, jako je například nakonfigurovaná [Maximální délka](#maximum-length), zda je vlastnost částí primárního klíče atd.

Například SQL Server map `DateTime` vlastnosti do `datetime2(7)` sloupců a `string` vlastnosti `nvarchar(max)` sloupců (nebo `nvarchar(450)` pro vlastnosti, které se používají jako klíč).

Sloupce můžete také nakonfigurovat tak, aby pro sloupec určily přesný datový typ. Například následující kód nakonfiguruje `Url` jako řetězec jiný než Unicode s maximální délkou `200` a `Rating` jako desetinné číslo s přesností `5` a škálováním `2`:

### <a name="data-annotationstabdata-annotations"></a>[Datové poznámky](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ColumnDataType.cs?name=ColumnDataType&highlight=4,6)]

### <a name="fluent-apitabfluent-api"></a>[Rozhraní Fluent API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ColumnDataType.cs?name=ColumnDataType&highlight=5-6)]

***

### <a name="maximum-length"></a>Maximální délka

Konfigurace maximální délky poskytuje nápovědu pro poskytovatele databáze o příslušném datovém typu sloupce, který se má zvolit pro danou vlastnost. Maximální délka se vztahuje pouze na datové typy pole, například `string` a `byte[]`.

> [!NOTE]
> Entity Framework neprovádí žádné ověření maximální délky před předáním dat poskytovateli. Pokud je to vhodné, je až do poskytovatele nebo úložiště dat. Například při cílení na SQL Server, což překračuje maximální délku, dojde k výjimce, protože datový typ podkladového sloupce nebude moci ukládat nadbytečné údaje.

V následujícím příkladu způsobí konfigurace maximální délky 500 sloupec typu `nvarchar(500)`, který se má vytvořit v SQL Server:

#### <a name="data-annotationstabdata-annotations"></a>[Datové poznámky](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/MaxLength.cs?name=MaxLength&highlight=4)]

#### <a name="fluent-apitabfluent-api"></a>[Rozhraní Fluent API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/MaxLength.cs?name=MaxLength&highlight=3-5)]

***

## <a name="required-and-optional-properties"></a>Povinné a volitelné vlastnosti

Vlastnost je považována za volitelnou, pokud je platná pro, aby obsahovala `null`. Pokud `null` není platná hodnota, která má být přiřazena vlastnosti, považuje se za povinnou vlastnost. Při mapování na schéma relační databáze jsou požadované vlastnosti vytvořeny jako sloupce, které neumožňují hodnotu null, a volitelné vlastnosti jsou vytvořeny jako sloupce s možnou hodnotou null.

### <a name="conventions"></a>Konvence

Podle konvence vlastnost, jejíž typ .NET může obsahovat hodnotu null, bude nakonfigurována jako volitelná, zatímco vlastnosti, jejichž typ .NET nesmí obsahovat hodnotu null, budou nakonfigurovány jako povinné. Například všechny vlastnosti s typy hodnot .NET (`int`, `decimal`, `bool`atd.) jsou nakonfigurovány jako povinné a všechny vlastnosti s Nullable typy hodnot (`int?`, `decimal?`, `bool?`atd.) jsou nakonfigurovány jako volitelné.

C#8 zavádí novou funkci nazvanou [typ odkazu s možnou hodnotou null](/dotnet/csharp/tutorials/nullable-reference-types), která umožňuje odkazování na typy odkazů, které označují, zda jsou pro ně platné hodnoty null nebo ne. Tato funkce je ve výchozím nastavení zakázaná a v případě jejího povolení upraví chování EF Core následujícím způsobem:

* Pokud jsou typy odkazů s možnou hodnotou null (výchozí), všechny vlastnosti s odkazy .NET se nakonfigurují jako volitelné podle konvence (např. `string`).
* Pokud jsou povoleny typy odkazů s možnou hodnotou null, budou vlastnosti nakonfigurovány na základě C# hodnoty null jejich typu .net: `string?` budou konfigurovány jako volitelné, zatímco `string` budou konfigurovány jako povinné.

Následující příklad znázorňuje typ entity s požadovanými a volitelnými vlastnostmi, přičemž odkazovaná funkce s možnou hodnotou null je zakázaná (výchozí) a povolená:

#### <a name="without-nullable-reference-types-defaulttabwithout-nrt"></a>[Bez odkazových typů s možnou hodnotou null (výchozí)](#tab/without-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/CustomerWithoutNullableReferenceTypes.cs?name=Customer&highlight=4-8)]

#### <a name="with-nullable-reference-typestabwith-nrt"></a>[S typy odkazů s možnou hodnotou null](#tab/with-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Customer.cs?name=Customer&highlight=4-6)]

***

Doporučuje se použít typy odkazů s možnou hodnotou null, protože flowuje C# hodnotu null vyjádřenou v kódu, aby EF Core model a databáze, a obviates použití rozhraní Fluent API nebo datových poznámek pro vyjádření stejného konceptu dvakrát.

> [!NOTE]
> Cvičení opatrnosti při povolování typů odkazů s možnou hodnotou null u existujícího projektu: vlastnosti referenčního typu, které byly dříve nakonfigurovány jako volitelné, budou nyní konfigurovány jako povinné, pokud nejsou explicitně opatřeny poznámkami k Nullable. Při správě schématu relační databáze to může způsobit, že se budou vygenerovat migrace, které změní hodnotu vlastnosti Database na sloupec.

Další informace o typech odkazů s možnou hodnotou null a o tom, jak je používat s EF Core, [najdete na stránce vyhrazená dokumentace pro tuto funkci](xref:core/miscellaneous/nullable-reference-types).

### <a name="explicit-configuration"></a>Explicitní konfigurace

Vlastnost, která by byla volitelná konvencí, se dá nakonfigurovat tak, aby se vyžadovala takto:

#### <a name="data-annotationstabdata-annotations"></a>[Datové poznámky](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?name=Required&highlight=4)]

#### <a name="fluent-apitabfluent-api"></a>[Rozhraní Fluent API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?name=Required&highlight=3-5)]

***
