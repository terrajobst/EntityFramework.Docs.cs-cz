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
# <a name="indexes"></a>Indexy

Indexy představují společný pojem v mnoha úložištích dat. I když se jejich implementace v úložišti dat může lišit, používají se k tomu, aby vyhledávání na základě sloupce (nebo sady sloupců) bylo efektivnější.

## <a name="conventions"></a>Konvence

Podle konvence se v každé vlastnosti (nebo sadě vlastností) vytvoří index, který se používá jako cizí klíč.

## <a name="data-annotations"></a>Datové poznámky

Indexy nelze vytvořit pomocí datových poznámek.

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít k určení indexu pro jednu vlastnost. Ve výchozím nastavení indexy nejsou jedinečné.

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

Můžete také určit, že by měl být index jedinečný, což znamená, že žádné dvě entity nemohou mít stejné hodnoty pro danou vlastnost (y).

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

Index můžete zadat také pro více než jeden sloupec.

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
> Pro jednu jedinečnou sadu vlastností je k dispozici pouze jeden index. Pokud použijete rozhraní Fluent API ke konfiguraci indexu pro sadu vlastností, které už mají definovaný index, buď podle konvence, nebo z předchozí konfigurace, bude změna definice tohoto indexu. To je užitečné, pokud chcete dále konfigurovat index, který byl vytvořen pomocí konvence.
