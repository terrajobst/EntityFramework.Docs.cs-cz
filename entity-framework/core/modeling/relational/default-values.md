---
title: Výchozí hodnoty – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e541366a-130f-47dd-9997-1b110a11febe
uid: core/modeling/relational/default-values
ms.openlocfilehash: 341f243ddddc345bb4236e5c34f814694b71e32a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996249"
---
# <a name="default-values"></a>Výchozí hodnoty

> [!NOTE]  
> Obecně se vztahuje k relačním databázím konfigurace v této části. Metody rozšíření je vidět tady bude k dispozici při instalaci poskytovatele relační databáze (z důvodu sdílený *Microsoft.EntityFrameworkCore.Relational* balíček).

Výchozí hodnota pro sloupec je hodnota, která bude vložen, pokud se vloží nový řádek, ale není zadaná žádná hodnota pro sloupec.

## <a name="conventions"></a>Konvence

Podle konvence není nakonfigurovaný výchozí hodnotu.

## <a name="data-annotations"></a>Datové poznámky

Nelze nastavit výchozí hodnotu pomocí datových poznámek.

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete zadat výchozí hodnotu pro vlastnost.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/DefaultValue.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Rating)
            .HasDefaultValue(3);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public int Rating { get; set; }
}
```

Můžete také zadat fragment SQL, který se používá k výpočtu výchozí hodnotu.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/DefaultValueSql.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Created)
            .HasDefaultValueSql("getdate()");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public DateTime Created { get; set; }
}
```
