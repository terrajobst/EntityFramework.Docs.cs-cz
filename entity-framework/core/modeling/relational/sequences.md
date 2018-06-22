---
title: Pořadí - EF jádra
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
ms.technology: entity-framework-core
uid: core/modeling/relational/sequences
ms.openlocfilehash: 98a40aeecbec0fd9fb9cc108d6b5f98178dea403
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054175"
---
# <a name="sequences"></a><span data-ttu-id="64d1b-102">Sekvence</span><span class="sxs-lookup"><span data-stu-id="64d1b-102">Sequences</span></span>

> [!NOTE]  
> <span data-ttu-id="64d1b-103">Obecně se vztahuje na relační databáze konfigurace v této části.</span><span class="sxs-lookup"><span data-stu-id="64d1b-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="64d1b-104">Rozšiřující metody zobrazeny zde bude k dispozici při instalaci poskytovatele relační databáze (z důvodu sdílený *Microsoft.EntityFrameworkCore.Relational* balíčku).</span><span class="sxs-lookup"><span data-stu-id="64d1b-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="64d1b-105">Posloupnost generuje sekvenční číselných hodnot v databázi.</span><span class="sxs-lookup"><span data-stu-id="64d1b-105">A sequence generates a sequential numeric values in the database.</span></span> <span data-ttu-id="64d1b-106">Pořadí nejsou přiřazeny určité tabulky.</span><span class="sxs-lookup"><span data-stu-id="64d1b-106">Sequences are not associated with a specific table.</span></span>

## <a name="conventions"></a><span data-ttu-id="64d1b-107">Konvence</span><span class="sxs-lookup"><span data-stu-id="64d1b-107">Conventions</span></span>

<span data-ttu-id="64d1b-108">Podle konvence nejsou pořadí zavedené v modelu.</span><span class="sxs-lookup"><span data-stu-id="64d1b-108">By convention, sequences are not introduced in to the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="64d1b-109">Datových poznámek</span><span class="sxs-lookup"><span data-stu-id="64d1b-109">Data Annotations</span></span>

<span data-ttu-id="64d1b-110">Nelze nakonfigurovat sekvenci pomocí datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="64d1b-110">You can not configure a sequence using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="64d1b-111">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="64d1b-111">Fluent API</span></span>

<span data-ttu-id="64d1b-112">Rozhraní Fluent API můžete použít k vytvoření pořadí v modelu.</span><span class="sxs-lookup"><span data-stu-id="64d1b-112">You can use the Fluent API to create a sequence in the model.</span></span>

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

<span data-ttu-id="64d1b-113">Můžete také nakonfigurovat další aspekt pořadí, například jeho schématu, počáteční hodnotu a přírůstku.</span><span class="sxs-lookup"><span data-stu-id="64d1b-113">You can also configure additional aspect of the sequence, such as its schema, start value, and increment.</span></span>

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

<span data-ttu-id="64d1b-114">Jakmile sekvenci zavádí, můžete ke generování hodnoty pro vlastnosti v modelu.</span><span class="sxs-lookup"><span data-stu-id="64d1b-114">Once a sequence is introduced, you can use it to generate values for properties in your model.</span></span> <span data-ttu-id="64d1b-115">Například můžete použít [výchozí hodnoty](default-values.md) vložení další hodnoty z pořadí.</span><span class="sxs-lookup"><span data-stu-id="64d1b-115">For example, you can use [Default Values](default-values.md) to insert the next value from the sequence.</span></span>

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
