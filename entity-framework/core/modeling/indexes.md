---
title: Indexy – EF jádra
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
ms.locfileid: "26054076"
---
# <a name="indexes"></a>Indexy

Indexy jsou běžným pojmem mezi mnoha datová úložiště. Implementace v úložišti dat se může lišit, se používají aby vyhledávání na základě sloupec (nebo sadu sloupců) více efektivní.

## <a name="conventions"></a>Konvence

Podle konvence index se vytvoří v každé vlastnosti (nebo sadu vlastností) používané jako cizí klíč.

## <a name="data-annotations"></a>Datových poznámek

Indexy nelze vytvořit pomocí datových poznámek.

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít k určení indexu na jedinou vlastnost. Indexy jsou ve výchozím nastavení není jedinečný.

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

Můžete také zadat, index musí být jedinečné, což znamená, že žádné dvě entity, může mít stejné hodnoty pro danou vlastností.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

Můžete také zadat indexu přes víc než jeden sloupec.

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
> Není k dispozici pouze jeden index za odlišnou sadu vlastností. Pokud používáte rozhraní Fluent API ke konfiguraci indexu na sadu vlastností, které už má index definované, buď convention nebo předchozí konfiguraci, pak budete moci změnit definici tohoto indexu. To je užitečné, pokud chcete nakonfigurovat další index, který byl vytvořen pomocí konvence.
