---
title: Klíče (primární) – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 8b32bf6417890a954c933a5973a2c90c609beeca
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197279"
---
# <a name="keys-primary"></a>Klíče (primární)

Klíč slouží jako primární jedinečný identifikátor pro každou instanci entity. Při použití relační databáze se tato mapa mapuje na koncept *primárního klíče*. Můžete také nakonfigurovat jedinečný identifikátor, který není primárním klíčem (Další informace najdete v tématu [alternativní klíče](alternate-keys.md) ). 

Jednu z následujících metod lze použít k nastavení nebo vytvoření primárního klíče.

## <a name="conventions"></a>Konvence

Podle konvence se vlastnost s `Id` názvem `<type name>Id` nebo nakonfiguruje jako klíč entity.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/KeyId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string Id { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/KeyTypeNameId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string CarId { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

## <a name="data-annotations"></a>Datové poznámky

Pomocí datových poznámek můžete nakonfigurovat jednu vlastnost, která bude klíč entity.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a>Rozhraní Fluent API

Pomocí rozhraní Fluent API můžete nakonfigurovat jedinou vlastnost, která bude klíč entity.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

Rozhraní Fluent API můžete použít také ke konfiguraci více vlastností, které budou klíč entity (označované jako složený klíč). Složené klíče se dají nakonfigurovat jenom pomocí konvencí rozhraní API Fluent nikdy nenastaví složený klíč a nemůžete použít datové poznámky ke konfiguraci jednoho.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]
