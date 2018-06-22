---
title: Klíče (primární) - EF jádra
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
ms.technology: entity-framework-core
uid: core/modeling/keys
ms.openlocfilehash: f3bf3c7f2a28e065b350fe000a5164406cd5ca08
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054109"
---
# <a name="keys-primary"></a><span data-ttu-id="b4173-102">Klíče (primární)</span><span class="sxs-lookup"><span data-stu-id="b4173-102">Keys (primary)</span></span>

<span data-ttu-id="b4173-103">Klíč slouží jako primární jedinečný identifikátor pro každou instanci entity.</span><span class="sxs-lookup"><span data-stu-id="b4173-103">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="b4173-104">Při použití relační databáze to se mapuje na konceptu *primární klíč*.</span><span class="sxs-lookup"><span data-stu-id="b4173-104">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="b4173-105">Můžete také konfigurovat jedinečný identifikátor, který není primární klíč (viz [alternativní klíče](alternate-keys.md) Další informace).</span><span class="sxs-lookup"><span data-stu-id="b4173-105">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

## <a name="conventions"></a><span data-ttu-id="b4173-106">Konvence</span><span class="sxs-lookup"><span data-stu-id="b4173-106">Conventions</span></span>

<span data-ttu-id="b4173-107">Podle konvence je vlastnost s názvem `Id` nebo `<type name>Id` bude nakonfigurován jako klíč entity.</span><span class="sxs-lookup"><span data-stu-id="b4173-107">By convention, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="b4173-108">Datových poznámek</span><span class="sxs-lookup"><span data-stu-id="b4173-108">Data Annotations</span></span>

<span data-ttu-id="b4173-109">Můžete nakonfigurovat jednu vlastnost, která má být klíč entity datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="b4173-109">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

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

## <a name="fluent-api"></a><span data-ttu-id="b4173-110">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="b4173-110">Fluent API</span></span>

<span data-ttu-id="b4173-111">Rozhraní Fluent API můžete nakonfigurovat jednu vlastnost, která má být klíč entity.</span><span class="sxs-lookup"><span data-stu-id="b4173-111">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

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

<span data-ttu-id="b4173-112">Rozhraní Fluent API můžete také použít ke konfiguraci více vlastností se klíč entity (označované jako složený klíč).</span><span class="sxs-lookup"><span data-stu-id="b4173-112">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="b4173-113">Složené klíče se dá nakonfigurovat jenom pomocí rozhraní Fluent API – konvence nikdy instalační program složený klíč a pomocí datových poznámek nelze nakonfigurovat jednu.</span><span class="sxs-lookup"><span data-stu-id="b4173-113">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

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
