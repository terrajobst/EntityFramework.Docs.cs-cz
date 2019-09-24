---
title: Indexy – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: b6f11401b69bd8e8795f6b22e5392ba16fc9ba2e
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197242"
---
# <a name="indexes"></a><span data-ttu-id="f88b2-102">Indexy</span><span class="sxs-lookup"><span data-stu-id="f88b2-102">Indexes</span></span>

<span data-ttu-id="f88b2-103">Indexy představují společný pojem v mnoha úložištích dat.</span><span class="sxs-lookup"><span data-stu-id="f88b2-103">Indexes are a common concept across many data stores.</span></span> <span data-ttu-id="f88b2-104">I když se jejich implementace v úložišti dat může lišit, používají se k tomu, aby vyhledávání na základě sloupce (nebo sady sloupců) bylo efektivnější.</span><span class="sxs-lookup"><span data-stu-id="f88b2-104">While their implementation in the data store may vary, they are used to make lookups based on a column (or set of columns) more efficient.</span></span>

## <a name="conventions"></a><span data-ttu-id="f88b2-105">Konvence</span><span class="sxs-lookup"><span data-stu-id="f88b2-105">Conventions</span></span>

<span data-ttu-id="f88b2-106">Podle konvence se v každé vlastnosti (nebo sadě vlastností) vytvoří index, který se používá jako cizí klíč.</span><span class="sxs-lookup"><span data-stu-id="f88b2-106">By convention, an index is created in each property (or set of properties) that are used as a foreign key.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="f88b2-107">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="f88b2-107">Data Annotations</span></span>

<span data-ttu-id="f88b2-108">Indexy nelze vytvořit pomocí datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="f88b2-108">Indexes can not be created using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="f88b2-109">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="f88b2-109">Fluent API</span></span>

<span data-ttu-id="f88b2-110">Rozhraní Fluent API můžete použít k určení indexu pro jednu vlastnost.</span><span class="sxs-lookup"><span data-stu-id="f88b2-110">You can use the Fluent API to specify an index on a single property.</span></span> <span data-ttu-id="f88b2-111">Ve výchozím nastavení indexy nejsou jedinečné.</span><span class="sxs-lookup"><span data-stu-id="f88b2-111">By default, indexes are non-unique.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Index.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="f88b2-112">Můžete také určit, že by měl být index jedinečný, což znamená, že žádné dvě entity nemohou mít stejné hodnoty pro danou vlastnost (y).</span><span class="sxs-lookup"><span data-stu-id="f88b2-112">You can also specify that an index should be unique, meaning that no two entities can have the same value(s) for the given property(s).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

<span data-ttu-id="f88b2-113">Index můžete zadat také pro více než jeden sloupec.</span><span class="sxs-lookup"><span data-stu-id="f88b2-113">You can also specify an index over more than one column.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/IndexComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Person> People { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Person>()
            .HasIndex(p => new { p.FirstName, p.LastName });
    }
}

public class Person
{
    public int PersonId { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
}
```

> [!TIP]  
> <span data-ttu-id="f88b2-114">Pro jednu jedinečnou sadu vlastností je k dispozici pouze jeden index.</span><span class="sxs-lookup"><span data-stu-id="f88b2-114">There is only one index per distinct set of properties.</span></span> <span data-ttu-id="f88b2-115">Pokud použijete rozhraní Fluent API ke konfiguraci indexu pro sadu vlastností, které už mají definovaný index, buď podle konvence, nebo z předchozí konfigurace, bude změna definice tohoto indexu.</span><span class="sxs-lookup"><span data-stu-id="f88b2-115">If you use the Fluent API to configure an index on a set of properties that already has an index defined, either by convention or previous configuration, then you will be changing the definition of that index.</span></span> <span data-ttu-id="f88b2-116">To je užitečné, pokud chcete dále konfigurovat index, který byl vytvořen pomocí konvence.</span><span class="sxs-lookup"><span data-stu-id="f88b2-116">This is useful if you want to further configure an index that was created by convention.</span></span>
