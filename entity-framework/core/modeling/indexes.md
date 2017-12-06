---
title: "Indexy – EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
ms.technology: entity-framework-core
uid: core/modeling/indexes
ms.openlocfilehash: f57b545d53613cec6887734bf434958ee8fff4d8
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="indexes"></a><span data-ttu-id="247a4-102">Indexy</span><span class="sxs-lookup"><span data-stu-id="247a4-102">Indexes</span></span>

<span data-ttu-id="247a4-103">Indexy jsou běžným pojmem mezi mnoha datová úložiště.</span><span class="sxs-lookup"><span data-stu-id="247a4-103">Indexes are a common concept across many data stores.</span></span> <span data-ttu-id="247a4-104">Implementace v úložišti dat se může lišit, se používají aby vyhledávání na základě sloupec (nebo sadu sloupců) více efektivní.</span><span class="sxs-lookup"><span data-stu-id="247a4-104">While their implementation in the data store may vary, they are used to make lookups based on a column (or set of columns) more efficient.</span></span>

## <a name="conventions"></a><span data-ttu-id="247a4-105">Konvence</span><span class="sxs-lookup"><span data-stu-id="247a4-105">Conventions</span></span>

<span data-ttu-id="247a4-106">Podle konvence index se vytvoří v každé vlastnosti (nebo sadu vlastností) používané jako cizí klíč.</span><span class="sxs-lookup"><span data-stu-id="247a4-106">By convention, an index is created in each property (or set of properties) that are used as a foreign key.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="247a4-107">Datových poznámek</span><span class="sxs-lookup"><span data-stu-id="247a4-107">Data Annotations</span></span>

<span data-ttu-id="247a4-108">Indexy nelze vytvořit pomocí datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="247a4-108">Indexes can not be created using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="247a4-109">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="247a4-109">Fluent API</span></span>

<span data-ttu-id="247a4-110">Rozhraní Fluent API můžete použít k určení indexu na jedinou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="247a4-110">You can use the Fluent API to specify an index on a single property.</span></span> <span data-ttu-id="247a4-111">Indexy jsou ve výchozím nastavení není jedinečný.</span><span class="sxs-lookup"><span data-stu-id="247a4-111">By default, indexes are non-unique.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Index.cs?highlight=7,8)] -->
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

<span data-ttu-id="247a4-112">Můžete také zadat, index musí být jedinečné, což znamená, že žádné dvě entity, může mít stejné hodnoty pro danou vlastností.</span><span class="sxs-lookup"><span data-stu-id="247a4-112">You can also specify that an index should be unique, meaning that no two entities can have the same value(s) for the given property(s).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

<span data-ttu-id="247a4-113">Můžete také zadat indexu přes víc než jeden sloupec.</span><span class="sxs-lookup"><span data-stu-id="247a4-113">You can also specify an index over more than one column.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexComposite.cs?highlight=7,8)] -->
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
> <span data-ttu-id="247a4-114">Není k dispozici pouze jeden index za odlišnou sadu vlastností.</span><span class="sxs-lookup"><span data-stu-id="247a4-114">There is only one index per distinct set of properties.</span></span> <span data-ttu-id="247a4-115">Pokud používáte rozhraní Fluent API ke konfiguraci indexu na sadu vlastností, které už má index definované, buď convention nebo předchozí konfiguraci, pak budete moci změnit definici tohoto indexu.</span><span class="sxs-lookup"><span data-stu-id="247a4-115">If you use the Fluent API to configure an index on a set of properties that already has an index defined, either by convention or previous configuration, then you will be changing the definition of that index.</span></span> <span data-ttu-id="247a4-116">To je užitečné, pokud chcete nakonfigurovat další index, který byl vytvořen pomocí konvence.</span><span class="sxs-lookup"><span data-stu-id="247a4-116">This is useful if you want to further configure an index that was created by convention.</span></span>
