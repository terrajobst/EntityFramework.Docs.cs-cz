---
title: Povinné a volitelné vlastnosti – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: 62b2b3f5a761c0aacece986ecd0b2dd2f958d048
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655653"
---
# <a name="required-and-optional-properties"></a>Požadované a volitelné vlastnosti

Vlastnost je považována za volitelnou, pokud je platná pro, aby obsahovala `null`. Pokud `null` není platná hodnota, která má být přiřazena vlastnosti, považuje se za povinnou vlastnost.

Při mapování na schéma relační databáze jsou požadované vlastnosti vytvořeny jako sloupce, které neumožňují hodnotu null, a volitelné vlastnosti jsou vytvořeny jako sloupce s možnou hodnotou null.

## <a name="conventions"></a>Konvence

Podle konvence vlastnost, jejíž typ .NET může obsahovat hodnotu null, bude nakonfigurována jako volitelná, zatímco vlastnosti, jejichž typ .NET nesmí obsahovat hodnotu null, budou nakonfigurovány jako povinné. Například všechny vlastnosti s typy hodnot .NET (`int`, `decimal`, `bool`atd.) jsou nakonfigurovány jako povinné a všechny vlastnosti s Nullable typy hodnot (`int?`, `decimal?`, `bool?`atd.) jsou nakonfigurovány jako volitelné.

C#8 zavádí novou funkci nazvanou [typ odkazu s možnou hodnotou null](/dotnet/csharp/tutorials/nullable-reference-types), která umožňuje odkazování na typy odkazů, které označují, zda jsou pro ně platné hodnoty null nebo ne. Tato funkce je ve výchozím nastavení zakázaná a v případě jejího povolení upraví chování EF Core následujícím způsobem:

* Pokud jsou typy odkazů s možnou hodnotou null (výchozí), všechny vlastnosti s odkazy .NET se nakonfigurují jako volitelné podle konvence (např. `string`).
* Pokud jsou povoleny typy odkazů s možnou hodnotou null, budou vlastnosti nakonfigurovány na základě C# hodnoty null jejich typu .net: `string?` budou konfigurovány jako volitelné, zatímco `string` budou konfigurovány jako povinné.

Následující příklad znázorňuje typ entity s požadovanými a volitelnými vlastnostmi, přičemž odkazovaná funkce s možnou hodnotou null je zakázaná (výchozí) a povolená:

# <a name="without-nullable-reference-types-defaulttabwithout-nrt"></a>[Bez odkazových typů s možnou hodnotou null (výchozí)](#tab/without-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/CustomerWithoutNullableReferenceTypes.cs?name=Customer&highlight=4-8)]

# <a name="with-nullable-reference-typestabwith-nrt"></a>[S typy odkazů s možnou hodnotou null](#tab/with-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Customer.cs?name=Customer&highlight=4-6)]

***

Doporučuje se použít typy odkazů s možnou hodnotou null, protože flowuje C# hodnotu null vyjádřenou v kódu, aby EF Core model a databáze, a obviates použití rozhraní Fluent API nebo datových poznámek pro vyjádření stejného konceptu dvakrát.

> [!NOTE]
> Cvičení opatrnosti při povolování typů odkazů s možnou hodnotou null u existujícího projektu: vlastnosti referenčního typu, které byly dříve nakonfigurovány jako volitelné, budou nyní konfigurovány jako povinné, pokud nejsou explicitně opatřeny poznámkami k Nullable. Při správě schématu relační databáze to může způsobit, že se budou vygenerovat migrace, které změní hodnotu vlastnosti Database na sloupec.

Další informace o typech odkazů s možnou hodnotou null a o tom, jak je používat s EF Core, [najdete na stránce vyhrazená dokumentace pro tuto funkci](xref:core/miscellaneous/nullable-reference-types).

## <a name="configuration"></a>Konfigurace

Vlastnost, která by byla volitelná konvencí, se dá nakonfigurovat tak, aby se vyžadovala takto:

# <a name="data-annotationstabdata-annotations"></a>[Datové poznámky](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=14)]

# <a name="fluent-apitabfluent-api"></a>[Rozhraní Fluent API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=11-13)]

***
