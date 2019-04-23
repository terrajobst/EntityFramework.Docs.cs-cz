---
title: Klíče (primární) – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 51d163b867085f42f415dbd7afa9e311ab1781a0
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929833"
---
# <a name="keys-primary"></a>Klíče (primární)

Klíč slouží jako primární jedinečný identifikátor pro každou instanci entity. Při používání relační databáze, to se mapuje na konceptu *primární klíč*. Můžete také nakonfigurovat jedinečný identifikátor, který není primární klíč (viz [alternativní klíče](alternate-keys.md) Další informace). 

Jeden z následujících metod slouží k nastavení/vytvoření primární klíč.

## <a name="conventions"></a>Konvence

Podle konvence je vlastnost s názvem `Id` nebo `<type name>Id` se nakonfigurují jako klíč entity.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/KeyId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string Id { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/KeyTypeNameId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string CarId { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

## <a name="data-annotations"></a>Datové poznámky

Anotací dat můžete použít ke konfiguraci jedné vlastnosti se klíč entity.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít ke konfiguraci jedné vlastnosti se klíč entity.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/KeySingle.cs?highlight=11,12)]

Můžete také použít rozhraní Fluent API nakonfigurovat několik vlastností tak, aby klíče entity (označované jako složený klíč). Složené klíče se dá nakonfigurovat jenom pomocí rozhraní Fluent API – konvence nikdy nastavíte složený klíč a anotacemi dat nelze použít ke konfiguraci jednoho.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/KeyComposite.cs?highlight=11,12)]
