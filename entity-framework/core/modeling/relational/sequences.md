---
title: Sekvence – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
uid: core/modeling/relational/sequences
ms.openlocfilehash: ce02b9840e58102a60c1d8eacf6810365104d7d7
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71196906"
---
# <a name="sequences"></a><span data-ttu-id="4e9e3-102">Sekvence</span><span class="sxs-lookup"><span data-stu-id="4e9e3-102">Sequences</span></span>

> [!NOTE]  
> <span data-ttu-id="4e9e3-103">Konfigurace v této části platí pro relační databáze obecně.</span><span class="sxs-lookup"><span data-stu-id="4e9e3-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="4e9e3-104">Metody rozšíření zobrazené tady budou k dispozici při instalaci zprostředkovatele relační databáze (z důvodu sdíleného balíčku *Microsoft. EntityFrameworkCore. relační* ).</span><span class="sxs-lookup"><span data-stu-id="4e9e3-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="4e9e3-105">Sekvence generuje sekvenční číselné hodnoty v databázi.</span><span class="sxs-lookup"><span data-stu-id="4e9e3-105">A sequence generates a sequential numeric values in the database.</span></span> <span data-ttu-id="4e9e3-106">Sekvence nejsou spojeny s konkrétní tabulkou.</span><span class="sxs-lookup"><span data-stu-id="4e9e3-106">Sequences are not associated with a specific table.</span></span>

## <a name="conventions"></a><span data-ttu-id="4e9e3-107">Konvence</span><span class="sxs-lookup"><span data-stu-id="4e9e3-107">Conventions</span></span>

<span data-ttu-id="4e9e3-108">Podle konvence nejsou sekvence do modelu zavedeny.</span><span class="sxs-lookup"><span data-stu-id="4e9e3-108">By convention, sequences are not introduced in to the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="4e9e3-109">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="4e9e3-109">Data Annotations</span></span>

<span data-ttu-id="4e9e3-110">Nelze konfigurovat sekvenci pomocí datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="4e9e3-110">You can not configure a sequence using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="4e9e3-111">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="4e9e3-111">Fluent API</span></span>

<span data-ttu-id="4e9e3-112">Rozhraní Fluent API můžete použít k vytvoření sekvence v modelu.</span><span class="sxs-lookup"><span data-stu-id="4e9e3-112">You can use the Fluent API to create a sequence in the model.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/Sequence.cs?highlight=7)] -->
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

<span data-ttu-id="4e9e3-113">Můžete také nakonfigurovat další aspekt sekvence, například její schéma, počáteční hodnotu a přírůstek.</span><span class="sxs-lookup"><span data-stu-id="4e9e3-113">You can also configure additional aspect of the sequence, such as its schema, start value, and increment.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/SequenceConfigured.cs?highlight=7,8,9)] -->
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

<span data-ttu-id="4e9e3-114">Po zavedení sekvence je můžete použít ke generování hodnot vlastností v modelu.</span><span class="sxs-lookup"><span data-stu-id="4e9e3-114">Once a sequence is introduced, you can use it to generate values for properties in your model.</span></span> <span data-ttu-id="4e9e3-115">Můžete například použít [výchozí hodnoty](default-values.md) pro vložení další hodnoty z sekvence.</span><span class="sxs-lookup"><span data-stu-id="4e9e3-115">For example, you can use [Default Values](default-values.md) to insert the next value from the sequence.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/SequenceUsed.cs?highlight=11,12,13)] -->
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
