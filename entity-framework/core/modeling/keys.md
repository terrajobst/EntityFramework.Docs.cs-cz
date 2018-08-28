---
title: Klíče (primární) – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 9e6946100ebabc6ba57cb792b3672219098b1e21
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994018"
---
# <a name="keys-primary"></a><span data-ttu-id="77cb9-102">Klíče (primární)</span><span class="sxs-lookup"><span data-stu-id="77cb9-102">Keys (primary)</span></span>

<span data-ttu-id="77cb9-103">Klíč slouží jako primární jedinečný identifikátor pro každou instanci entity.</span><span class="sxs-lookup"><span data-stu-id="77cb9-103">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="77cb9-104">Při používání relační databáze, to se mapuje na konceptu *primární klíč*.</span><span class="sxs-lookup"><span data-stu-id="77cb9-104">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="77cb9-105">Můžete také nakonfigurovat jedinečný identifikátor, který není primární klíč (viz [alternativní klíče](alternate-keys.md) Další informace).</span><span class="sxs-lookup"><span data-stu-id="77cb9-105">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

## <a name="conventions"></a><span data-ttu-id="77cb9-106">Konvence</span><span class="sxs-lookup"><span data-stu-id="77cb9-106">Conventions</span></span>

<span data-ttu-id="77cb9-107">Podle konvence je vlastnost s názvem `Id` nebo `<type name>Id` se nakonfigurují jako klíč entity.</span><span class="sxs-lookup"><span data-stu-id="77cb9-107">By convention, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="77cb9-108">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="77cb9-108">Data Annotations</span></span>

<span data-ttu-id="77cb9-109">Anotací dat můžete použít ke konfiguraci jedné vlastnosti se klíč entity.</span><span class="sxs-lookup"><span data-stu-id="77cb9-109">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/KeySingle.cs?highlight=3,4)] -->
``` csharp
class Car
{
    [Key]
    public string LicensePlate { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="77cb9-110">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="77cb9-110">Fluent API</span></span>

<span data-ttu-id="77cb9-111">Rozhraní Fluent API můžete použít ke konfiguraci jedné vlastnosti se klíč entity.</span><span class="sxs-lookup"><span data-stu-id="77cb9-111">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/KeySingle.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasKey(c => c.LicensePlate);
    }
}

class Car
{
    public string LicensePlate { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

<span data-ttu-id="77cb9-112">Můžete také použít rozhraní Fluent API nakonfigurovat několik vlastností tak, aby klíče entity (označované jako složený klíč).</span><span class="sxs-lookup"><span data-stu-id="77cb9-112">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="77cb9-113">Složené klíče se dá nakonfigurovat jenom pomocí rozhraní Fluent API – konvence nikdy nastavíte složený klíč a anotacemi dat nelze použít ke konfiguraci jednoho.</span><span class="sxs-lookup"><span data-stu-id="77cb9-113">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/KeyComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasKey(c => new { c.State, c.LicensePlate });
    }
}

class Car
{
    public string State { get; set; }
    public string LicensePlate { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```
