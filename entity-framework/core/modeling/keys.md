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
# <a name="keys-primary"></a><span data-ttu-id="7140a-102">Klíče (primární)</span><span class="sxs-lookup"><span data-stu-id="7140a-102">Keys (primary)</span></span>

<span data-ttu-id="7140a-103">Klíč slouží jako primární jedinečný identifikátor pro každou instanci entity.</span><span class="sxs-lookup"><span data-stu-id="7140a-103">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="7140a-104">Při používání relační databáze, to se mapuje na konceptu *primární klíč*.</span><span class="sxs-lookup"><span data-stu-id="7140a-104">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="7140a-105">Můžete také nakonfigurovat jedinečný identifikátor, který není primární klíč (viz [alternativní klíče](alternate-keys.md) Další informace).</span><span class="sxs-lookup"><span data-stu-id="7140a-105">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span> 

<span data-ttu-id="7140a-106">Jeden z následujících metod slouží k nastavení/vytvoření primární klíč.</span><span class="sxs-lookup"><span data-stu-id="7140a-106">One of the following methods can be used to setup/create a primary key.</span></span>

## <a name="conventions"></a><span data-ttu-id="7140a-107">Konvence</span><span class="sxs-lookup"><span data-stu-id="7140a-107">Conventions</span></span>

<span data-ttu-id="7140a-108">Podle konvence je vlastnost s názvem `Id` nebo `<type name>Id` se nakonfigurují jako klíč entity.</span><span class="sxs-lookup"><span data-stu-id="7140a-108">By convention, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="7140a-109">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="7140a-109">Data Annotations</span></span>

<span data-ttu-id="7140a-110">Anotací dat můžete použít ke konfiguraci jedné vlastnosti se klíč entity.</span><span class="sxs-lookup"><span data-stu-id="7140a-110">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="7140a-111">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="7140a-111">Fluent API</span></span>

<span data-ttu-id="7140a-112">Rozhraní Fluent API můžete použít ke konfiguraci jedné vlastnosti se klíč entity.</span><span class="sxs-lookup"><span data-stu-id="7140a-112">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/KeySingle.cs?highlight=11,12)]

<span data-ttu-id="7140a-113">Můžete také použít rozhraní Fluent API nakonfigurovat několik vlastností tak, aby klíče entity (označované jako složený klíč).</span><span class="sxs-lookup"><span data-stu-id="7140a-113">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="7140a-114">Složené klíče se dá nakonfigurovat jenom pomocí rozhraní Fluent API – konvence nikdy nastavíte složený klíč a anotacemi dat nelze použít ke konfiguraci jednoho.</span><span class="sxs-lookup"><span data-stu-id="7140a-114">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/KeyComposite.cs?highlight=11,12)]
