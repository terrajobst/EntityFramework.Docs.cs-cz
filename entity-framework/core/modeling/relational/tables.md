---
title: "Mapování tabulky - EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
ms.technology: entity-framework-core
uid: core/modeling/relational/tables
ms.openlocfilehash: 73957d9c77e6856bfb65e10e6b373c337101f7d9
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="table-mapping"></a>Mapování tabulek

> [!NOTE]  
> Obecně se vztahuje na relační databáze konfigurace v této části. Rozšiřující metody zobrazeny zde bude k dispozici při instalaci poskytovatele relační databáze (z důvodu sdílený *Microsoft.EntityFrameworkCore.Relational* balíčku).

Mapování tabulky identifikuje tabulky datových by měl být získaných z a uloženy v databázi.

## <a name="conventions"></a>Konvence

Podle konvence, každá entita bude instalační program k mapování na tabulku se stejným názvem jako `DbSet<TEntity>` vlastnost, která zveřejňuje se entita na odvozené kontextu. Pokud žádné `DbSet<TEntity>` je zahrnuté pro danou entitu, se používá název třídy.

## <a name="data-annotations"></a>Datových poznámek

Můžete nakonfigurovat tabulku, která se mapuje typ datových poznámek.

``` csharp
using System.ComponentModel.DataAnnotations.Schema;
```
``` csharp
[Table("blogs")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

Můžete také zadat schématu, který je v tabulce součástí.

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>Rozhraní Fluent API

Můžete nakonfigurovat v tabulce, která se mapuje typ na rozhraní Fluent API.

``` csharp
using Microsoft.EntityFrameworkCore;
```
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .ToTable("blogs");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

Můžete také zadat schématu, který je v tabulce součástí.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/TableAndSchema.cs?highlight=2)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .ToTable("blogs", schema: "blogging");
```
