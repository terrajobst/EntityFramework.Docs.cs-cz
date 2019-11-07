---
title: Klíče (primární) – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 66c64c389294e8e109a614a2bea8311932660dea
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655951"
---
# <a name="keys-primary"></a>Klíče (primární)

Klíč slouží jako primární jedinečný identifikátor pro každou instanci entity. Při použití relační databáze se tato mapa mapuje na koncept *primárního klíče*. Můžete také nakonfigurovat jedinečný identifikátor, který není primárním klíčem (Další informace najdete v tématu [alternativní klíče](alternate-keys.md) ).

Jednu z následujících metod lze použít k nastavení nebo vytvoření primárního klíče.

## <a name="conventions"></a>Konvence

Podle konvence bude vlastnost s názvem `Id` nebo `<type name>Id` nakonfigurovaná jako klíč entity.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3)]

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyTypeNameId.cs?name=KeyIdhighlight=3)]

## <a name="data-annotations"></a>Datové poznámky

Pomocí datových poznámek můžete nakonfigurovat jednu vlastnost, která bude klíč entity.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a>Rozhraní Fluent API

Pomocí rozhraní Fluent API můžete nakonfigurovat jedinou vlastnost, která bude klíč entity.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

Rozhraní Fluent API můžete použít také ke konfiguraci více vlastností, které budou klíč entity (označované jako složený klíč). Složené klíče se dají nakonfigurovat jenom pomocí konvencí rozhraní API Fluent nikdy nenastaví složený klíč a nemůžete použít datové poznámky ke konfiguraci jednoho.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]
