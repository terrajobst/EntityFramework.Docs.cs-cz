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
# <a name="indexes"></a>Indexy

Indexy jsou běžným pojmem mezi mnoha úložišť dat. Při jejich implementace v úložišti dat může lišit, slouží k širší možnosti vyhledávání na základě sloupec (nebo sadu sloupců) efektivní.

## <a name="conventions"></a>Konvence

Podle konvence index se vytvoří v každé vlastnosti (nebo sadu vlastností), které se používají jako cizí klíč.

## <a name="data-annotations"></a>Datové poznámky

Indexy nelze vytvořit pomocí datových poznámek.

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít k určení indexu podle jedné vlastnosti. Indexy jsou ve výchozím nastavení, není jedinečný.

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

Můžete také zadat, že index musí být jedinečný, to znamená, že žádné dvě entity, které mají stejné hodnoty pro dané vlastnosti.

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
> Existuje pouze jeden index na různé sady vlastností. Pokud používáte rozhraní Fluent API nakonfigurovat indexu na sadu vlastností, které už má definované, index, buď pomocí konvence nebo předchozí konfiguraci, pak budete moci změnit definici indexu. To je užitečné, pokud chcete provést další konfiguraci index, který byl vytvořen konvencí.
