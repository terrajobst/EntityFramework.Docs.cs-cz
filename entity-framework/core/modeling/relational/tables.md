---
title: Mapování tabulky – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
uid: core/modeling/relational/tables
ms.openlocfilehash: 474c49aca4c65cd5d58b184b1f3c2d30e7abff84
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656102"
---
# <a name="table-mapping"></a>Mapování tabulek

> [!NOTE]  
> Konfigurace v této části platí pro relační databáze obecně. Metody rozšíření zobrazené tady budou k dispozici při instalaci zprostředkovatele relační databáze (z důvodu sdíleného balíčku *Microsoft. EntityFrameworkCore. relační* ).

Mapování tabulky určuje, která data tabulky by se měla dotazovat a Uložit do databáze.

## <a name="conventions"></a>Konvence

Podle konvence se Každá entita nastaví tak, aby se namapovala na tabulku se stejným názvem, jako má vlastnost `DbSet<TEntity>`, která zpřístupňuje entitu v odvozeném kontextu. Pokud není pro danou entitu zahrnutá žádná `DbSet<TEntity>`, použije se název třídy.

## <a name="data-annotations"></a>Datové poznámky

K nakonfigurování tabulky, na kterou je typ mapován, můžete použít datové poznámky.

``` csharp
using System.ComponentModel.DataAnnotations.Schema;

[Table("blogs")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

Můžete také zadat schéma, do kterého tabulka patří.

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít ke konfiguraci tabulky, na kterou se typ mapuje.

``` csharp
using Microsoft.EntityFrameworkCore;

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

Můžete také zadat schéma, do kterého tabulka patří.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/TableAndSchema.cs?name=Table&highlight=2)]
