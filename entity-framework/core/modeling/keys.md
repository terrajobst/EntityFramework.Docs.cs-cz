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
# <a name="keys-primary"></a><span data-ttu-id="79f93-102">Klíče (primární)</span><span class="sxs-lookup"><span data-stu-id="79f93-102">Keys (primary)</span></span>

<span data-ttu-id="79f93-103">Klíč slouží jako primární jedinečný identifikátor pro každou instanci entity.</span><span class="sxs-lookup"><span data-stu-id="79f93-103">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="79f93-104">Při použití relační databáze se tato mapa mapuje na koncept *primárního klíče*.</span><span class="sxs-lookup"><span data-stu-id="79f93-104">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="79f93-105">Můžete také nakonfigurovat jedinečný identifikátor, který není primárním klíčem (Další informace najdete v tématu [alternativní klíče](alternate-keys.md) ).</span><span class="sxs-lookup"><span data-stu-id="79f93-105">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span> 

<span data-ttu-id="79f93-106">Jednu z následujících metod lze použít k nastavení nebo vytvoření primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="79f93-106">One of the following methods can be used to setup/create a primary key.</span></span>

## <a name="conventions"></a><span data-ttu-id="79f93-107">Konvence</span><span class="sxs-lookup"><span data-stu-id="79f93-107">Conventions</span></span>

<span data-ttu-id="79f93-108">Podle konvence se vlastnost s `Id` názvem `<type name>Id` nebo nakonfiguruje jako klíč entity.</span><span class="sxs-lookup"><span data-stu-id="79f93-108">By convention, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="79f93-109">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="79f93-109">Data Annotations</span></span>

<span data-ttu-id="79f93-110">Pomocí datových poznámek můžete nakonfigurovat jednu vlastnost, která bude klíč entity.</span><span class="sxs-lookup"><span data-stu-id="79f93-110">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="79f93-111">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="79f93-111">Fluent API</span></span>

<span data-ttu-id="79f93-112">Pomocí rozhraní Fluent API můžete nakonfigurovat jedinou vlastnost, která bude klíč entity.</span><span class="sxs-lookup"><span data-stu-id="79f93-112">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

<span data-ttu-id="79f93-113">Rozhraní Fluent API můžete použít také ke konfiguraci více vlastností, které budou klíč entity (označované jako složený klíč).</span><span class="sxs-lookup"><span data-stu-id="79f93-113">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="79f93-114">Složené klíče se dají nakonfigurovat jenom pomocí konvencí rozhraní API Fluent nikdy nenastaví složený klíč a nemůžete použít datové poznámky ke konfiguraci jednoho.</span><span class="sxs-lookup"><span data-stu-id="79f93-114">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]
