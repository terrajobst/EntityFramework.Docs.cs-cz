---
title: Pořadí – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
uid: core/modeling/relational/sequences
ms.openlocfilehash: eb9d9896966af0ad6b778047a1ed6af7358e8eb2
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994513"
---
# <a name="sequences"></a><span data-ttu-id="5d456-102">Sekvence</span><span class="sxs-lookup"><span data-stu-id="5d456-102">Sequences</span></span>

> [!NOTE]  
> <span data-ttu-id="5d456-103">Obecně se vztahuje k relačním databázím konfigurace v této části.</span><span class="sxs-lookup"><span data-stu-id="5d456-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="5d456-104">Metody rozšíření je vidět tady bude k dispozici při instalaci poskytovatele relační databáze (z důvodu sdílený *Microsoft.EntityFrameworkCore.Relational* balíček).</span><span class="sxs-lookup"><span data-stu-id="5d456-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="5d456-105">Sekvence generuje sekvenční číselných hodnot v databázi.</span><span class="sxs-lookup"><span data-stu-id="5d456-105">A sequence generates a sequential numeric values in the database.</span></span> <span data-ttu-id="5d456-106">Sekvence nejsou spojeny s konkrétní tabulky.</span><span class="sxs-lookup"><span data-stu-id="5d456-106">Sequences are not associated with a specific table.</span></span>

## <a name="conventions"></a><span data-ttu-id="5d456-107">Konvence</span><span class="sxs-lookup"><span data-stu-id="5d456-107">Conventions</span></span>

<span data-ttu-id="5d456-108">Podle konvence nejsou pořadí zavedené v modelu.</span><span class="sxs-lookup"><span data-stu-id="5d456-108">By convention, sequences are not introduced in to the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="5d456-109">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="5d456-109">Data Annotations</span></span>

<span data-ttu-id="5d456-110">Pořadí použití anotací dat nelze konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="5d456-110">You can not configure a sequence using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="5d456-111">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="5d456-111">Fluent API</span></span>

<span data-ttu-id="5d456-112">Rozhraní Fluent API můžete použít k vytvoření sekvence v modelu.</span><span class="sxs-lookup"><span data-stu-id="5d456-112">You can use the Fluent API to create a sequence in the model.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/Sequence.cs?highlight=7)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Order> Orders { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasSequence<int>("OrderNumbers");
    }
}

public class Order
{
    public int OrderId { get; set; }
    public int OrderNo { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="5d456-113">Můžete také nakonfigurovat další aspekty pořadí, například jeho schématu, počáteční hodnotu a přírůstku.</span><span class="sxs-lookup"><span data-stu-id="5d456-113">You can also configure additional aspect of the sequence, such as its schema, start value, and increment.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/SequenceConfigured.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Order> Orders { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasSequence<int>("OrderNumbers", schema: "shared")
            .StartsAt(1000)
            .IncrementsBy(5);
    }
}
```

<span data-ttu-id="5d456-114">Jakmile se zavedl až sekvenci, můžete ke generování hodnoty pro vlastnosti v modelu.</span><span class="sxs-lookup"><span data-stu-id="5d456-114">Once a sequence is introduced, you can use it to generate values for properties in your model.</span></span> <span data-ttu-id="5d456-115">Například můžete použít [výchozí hodnoty](default-values.md) vložíte následující hodnotu ze sekvence.</span><span class="sxs-lookup"><span data-stu-id="5d456-115">For example, you can use [Default Values](default-values.md) to insert the next value from the sequence.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/SequenceUsed.cs?highlight=11,12,13)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Order> Orders { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasSequence<int>("OrderNumbers", schema: "shared")
            .StartsAt(1000)
            .IncrementsBy(5);

        modelBuilder.Entity<Order>()
            .Property(o => o.OrderNo)
            .HasDefaultValueSql("NEXT VALUE FOR shared.OrderNumbers");
    }
}

public class Order
{
    public int OrderId { get; set; }
    public int OrderNo { get; set; }
    public string Url { get; set; }
}
```
