---
title: Mapování tabulky – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
uid: core/modeling/relational/tables
ms.openlocfilehash: 62dce317b901bc862b3c7d20ed1d176805bb24dd
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71196967"
---
# <a name="table-mapping"></a>Mapování tabulek

> [!NOTE]  
> Konfigurace v této části platí pro relační databáze obecně. Metody rozšíření zobrazené tady budou k dispozici při instalaci zprostředkovatele relační databáze (z důvodu sdíleného balíčku *Microsoft. EntityFrameworkCore. relační* ).

Mapování tabulky určuje, která data tabulky by se měla dotazovat a Uložit do databáze.

## <a name="conventions"></a>Konvence

Podle konvence se Každá entita nastaví tak, aby se namapovala na tabulku se stejným názvem, `DbSet<TEntity>` jako má vlastnost, která zpřístupňuje entitu v odvozeném kontextu. Pokud pro danou entitu nenízahrnuto,použijesenázevtřídy.`DbSet<TEntity>`

## <a name="data-annotations"></a>Datové poznámky

K nakonfigurování tabulky, na kterou je typ mapován, můžete použít datové poznámky.

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

Můžete také zadat schéma, do kterého tabulka patří.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/TableAndSchema.cs?highlight=2)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .ToTable("blogs", schema: "blogging");
```
