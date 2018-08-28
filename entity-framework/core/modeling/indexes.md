---
title: Indexy – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: 87fe893243377e3ab83d419ae9bedf813ca50c3f
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995477"
---
# <a name="indexes"></a><span data-ttu-id="6dd45-102">Indexy</span><span class="sxs-lookup"><span data-stu-id="6dd45-102">Indexes</span></span>

<span data-ttu-id="6dd45-103">Indexy jsou běžným pojmem mezi mnoha úložišť dat.</span><span class="sxs-lookup"><span data-stu-id="6dd45-103">Indexes are a common concept across many data stores.</span></span> <span data-ttu-id="6dd45-104">Při jejich implementace v úložišti dat může lišit, slouží k širší možnosti vyhledávání na základě sloupec (nebo sadu sloupců) efektivní.</span><span class="sxs-lookup"><span data-stu-id="6dd45-104">While their implementation in the data store may vary, they are used to make lookups based on a column (or set of columns) more efficient.</span></span>

## <a name="conventions"></a><span data-ttu-id="6dd45-105">Konvence</span><span class="sxs-lookup"><span data-stu-id="6dd45-105">Conventions</span></span>

<span data-ttu-id="6dd45-106">Podle konvence index se vytvoří v každé vlastnosti (nebo sadu vlastností), které se používají jako cizí klíč.</span><span class="sxs-lookup"><span data-stu-id="6dd45-106">By convention, an index is created in each property (or set of properties) that are used as a foreign key.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="6dd45-107">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="6dd45-107">Data Annotations</span></span>

<span data-ttu-id="6dd45-108">Indexy nelze vytvořit pomocí datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="6dd45-108">Indexes can not be created using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="6dd45-109">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="6dd45-109">Fluent API</span></span>

<span data-ttu-id="6dd45-110">Rozhraní Fluent API můžete použít k určení indexu podle jedné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="6dd45-110">You can use the Fluent API to specify an index on a single property.</span></span> <span data-ttu-id="6dd45-111">Indexy jsou ve výchozím nastavení, není jedinečný.</span><span class="sxs-lookup"><span data-stu-id="6dd45-111">By default, indexes are non-unique.</span></span>

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

<span data-ttu-id="6dd45-112">Můžete také zadat, že index musí být jedinečný, to znamená, že žádné dvě entity, které mají stejné hodnoty pro dané vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="6dd45-112">You can also specify that an index should be unique, meaning that no two entities can have the same value(s) for the given property(s).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

<span data-ttu-id="6dd45-113">Můžete také zadat indexu přes víc než jeden sloupec.</span><span class="sxs-lookup"><span data-stu-id="6dd45-113">You can also specify an index over more than one column.</span></span>

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
> <span data-ttu-id="6dd45-114">Existuje pouze jeden index na různé sady vlastností.</span><span class="sxs-lookup"><span data-stu-id="6dd45-114">There is only one index per distinct set of properties.</span></span> <span data-ttu-id="6dd45-115">Pokud používáte rozhraní Fluent API nakonfigurovat indexu na sadu vlastností, které už má definované, index, buď pomocí konvence nebo předchozí konfiguraci, pak budete moci změnit definici indexu.</span><span class="sxs-lookup"><span data-stu-id="6dd45-115">If you use the Fluent API to configure an index on a set of properties that already has an index defined, either by convention or previous configuration, then you will be changing the definition of that index.</span></span> <span data-ttu-id="6dd45-116">To je užitečné, pokud chcete provést další konfiguraci index, který byl vytvořen konvencí.</span><span class="sxs-lookup"><span data-stu-id="6dd45-116">This is useful if you want to further configure an index that was created by convention.</span></span>
