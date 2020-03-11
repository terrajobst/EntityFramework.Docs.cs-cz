---
title: Klíče – EF Core
description: Postup při konfiguraci klíčů pro typy entit při použití Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/keys
ms.openlocfilehash: abd65a5ea079a49fd7a3bbc84a9337f6ee19fab1
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416465"
---
# <a name="keys"></a>Klíče

Klíč slouží jako jedinečný identifikátor pro každou instanci entity. Většina entit v EF má jediný klíč, který se mapuje na koncept *primárního klíče* v relačních databázích (pro entity bez klíčů, viz [entity bez klíčů](xref:core/modeling/keyless-entity-types)). Entity můžou mít další klíče nad rámec primárního klíče (Další informace najdete v tématu [alternativní klíče](#alternate-keys) ).

Podle konvence bude vlastnost s názvem `Id` nebo `<type name>Id` nakonfigurovaná jako primární klíč entity.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3,11)]

> [!NOTE]
> [Vlastní typy entit](xref:core/modeling/owned-entities) používají pro definování klíčů různá pravidla.

Jednu vlastnost můžete nastavit jako primární klíč entity následujícím způsobem:

## <a name="data-annotations"></a>[Datové poznámky](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?name=KeySingle&highlight=3)]

## <a name="fluent-api"></a>[Rozhraní Fluent API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?name=KeySingle&highlight=4)]

***

Můžete také nakonfigurovat více vlastností, které mají být klíčem entity – to se označuje jako složený klíč. Složené klíče se dají konfigurovat jenom pomocí rozhraní API Fluent. konvence nikdy nenastaví složený klíč a nelze použít datové poznámky ke konfiguraci jednoho.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?name=KeyComposite&highlight=4)]

## <a name="primary-key-name"></a>Název primárního klíče

Podle konvence se v relačních databázích vytvoří primární klíče s názvem `PK_<type name>`. Název omezení primárního klíče můžete nakonfigurovat takto:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyName.cs?name=KeyName&highlight=5)]

## <a name="key-types-and-values"></a>Typy a hodnoty klíčů

I když EF Core podporuje použití vlastností libovolného primitivního typu jako primárního klíče, včetně `string`, `Guid`, `byte[]` a dalších, ne všechny databáze podporují všechny typy jako klíče. V některých případech je možné hodnoty klíče převést na podporovaný typ automaticky, jinak by se měl převod [zadat ručně](xref:core/modeling/value-conversions).

Při přidávání nové entity do kontextu musí mít klíčové vlastnosti vždy jinou než výchozí hodnotu, ale [v databázi budou vygenerovány](xref:core/modeling/generated-properties)některé typy. V takovém případě EF se při přidání entity pro účely sledování pokusí vygenerovat dočasnou hodnotu. Po volání [metody SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) se tato dočasná hodnota nahradí hodnotou generovanou databází.

> [!Important]
> Pokud má klíčová vlastnost hodnotu vygenerovanou databází a při přidání entity je zadaná jiná než výchozí hodnota, pak EF předpokládá, že entita v databázi již existuje a pokusí se ji aktualizovat místo vložení nového. Chcete-li se vyhnout této generaci hodnoty, nebo se podívejte, [jak zadat explicitní hodnoty pro vygenerované vlastnosti](../saving/explicit-values-generated-properties.md).

## <a name="alternate-keys"></a>Alternativní klíče

Alternativní klíč slouží jako alternativní jedinečný identifikátor pro každou instanci entity kromě primárního klíče; dá se použít jako cíl relace. Při použití relační databáze se tato mapa mapuje na koncept jedinečného indexu nebo omezení pro sloupce nebo sloupce alternativních klíčů a jedno nebo více omezení cizího klíče, které odkazují na sloupce.

> [!TIP]
> Pokud chcete u sloupce jenom vyhovět jedinečnost, definujte jedinečný index místo alternativního klíče (viz [indexy](indexes.md)). V EF jsou alternativní klíče jen pro čtení a poskytují další sémantiku v rámci jedinečných indexů, protože je lze použít jako cíl cizího klíče.

V případě potřeby jsou obvykle zavedeny alternativní klíče a není nutné je ručně konfigurovat. Podle konvence se pro vás zavede alternativní klíč, když identifikujete vlastnost, která není primárním klíčem jako cíl relace.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/AlternateKey.cs?name=AlternateKey&highlight=12)]

Jednu vlastnost můžete také nakonfigurovat tak, aby byla alternativním klíčem:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeySingle.cs?name=AlternateKeySingle&highlight=4)]

Můžete také nakonfigurovat více vlastností jako alternativní klíč (označované jako složený alternativní klíč):

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeyComposite.cs?name=AlternateKeyComposite&highlight=4)]

Nakonec se podle konvence index a omezení, které jsou představené pro alternativní klíč, budou jmenovat `AK_<type name>_<property name>` (u složených alternativních klíčů `<property name>` se změní na seznam názvů vlastností oddělených znakem.) Můžete nakonfigurovat název indexu alternativního klíče a jedinečné omezení:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeyName.cs?name=AlternateKeyName&highlight=5)]
