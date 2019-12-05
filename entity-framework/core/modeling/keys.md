---
title: Klíče (primární) – EF Core
description: Postup při konfiguraci klíčů pro typy entit při použití Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/keys
ms.openlocfilehash: fdaccb42259ba9dad97a05c626edd0291ca96cb0
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824618"
---
# <a name="keys-primary"></a>Klíče (primární)

Klíč slouží jako primární jedinečný identifikátor pro každou instanci entity. Při použití relační databáze se tato mapa mapuje na koncept *primárního klíče*. Můžete také nakonfigurovat jedinečný identifikátor, který není primárním klíčem (Další informace najdete v tématu [alternativní klíče](alternate-keys.md) ).

Jednu z následujících metod lze použít k nastavení nebo vytvoření primárního klíče.

## <a name="conventions"></a>Konvence

Ve výchozím nastavení bude vlastnost s názvem `Id` nebo `<type name>Id` nakonfigurována jako klíč entity.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3)]

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyTypeNameId.cs?name=KeyId&highlight=3)]

> [!NOTE]
> [Vlastní typy entit](xref:core/modeling/owned-entities) používají pro definování klíčů různá pravidla.

## <a name="data-annotations"></a>Datové poznámky

Pomocí datových poznámek můžete nakonfigurovat jednu vlastnost, která bude klíč entity.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a>Rozhraní Fluent API

Pomocí rozhraní Fluent API můžete nakonfigurovat jedinou vlastnost, která bude klíč entity.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

Rozhraní Fluent API můžete použít také ke konfiguraci více vlastností, které budou klíč entity (označované jako složený klíč). Složené klíče se dají nakonfigurovat jenom pomocí konvencí rozhraní API Fluent nikdy nenastaví složený klíč a nemůžete použít datové poznámky ke konfiguraci jednoho.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]

## <a name="key-types-and-values"></a>Typy a hodnoty klíčů

EF Core podporuje použití vlastností libovolného primitivního typu jako primárního klíče, včetně `string`, `Guid`, `byte[]` a dalších. Ale ne všechny databáze je podporují. V některých případech je možné hodnoty klíče převést na podporovaný typ automaticky, jinak by se měl převod [zadat ručně](xref:core/modeling/value-conversions).

Při přidávání nové entity do kontextu musí mít klíčové vlastnosti vždy jinou než výchozí hodnotu, ale [v databázi budou vygenerovány](xref:core/modeling/generated-properties)některé typy. V takovém případě EF se při přidání entity pro účely sledování pokusí vygenerovat dočasnou hodnotu. Po volání [metody SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) se tato dočasná hodnota nahradí hodnotou generovanou databází.

> [!Important]
> Pokud je vlastnost klíče vygenerovaná databází a při přidání entity je zadaná jiná než výchozí hodnota, pak EF předpokládá, že entita v databázi již existuje a pokusí se ji aktualizovat místo vložení nového. Chcete-li se vyhnout této generaci hodnoty, nebo se podívejte, [jak zadat explicitní hodnoty pro vygenerované vlastnosti](../saving/explicit-values-generated-properties.md).