---
title: Primární klíče – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: 916f3adbcd08cb1037c7fbf68e99630feb321a61
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998065"
---
# <a name="primary-keys"></a>Primární klíče

> [!NOTE]  
> Obecně se vztahuje k relačním databázím konfigurace v této části. Metody rozšíření je vidět tady bude k dispozici při instalaci poskytovatele relační databáze (z důvodu sdílený *Microsoft.EntityFrameworkCore.Relational* balíček).

Omezení primárního klíče se používá pro klíč každého typu entity.

## <a name="conventions"></a>Konvence

Podle konvence, bude mít primární klíč v databázi `PK_<type name>`.

## <a name="data-annotations"></a>Datové poznámky

Žádné konkrétní aspekty relační databáze primární klíč lze konfigurovat pomocí datových poznámek.

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít ke konfiguraci název omezení primárního klíče v databázi.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/KeyName.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasKey(b => b.BlogId)
            .HasName("PrimaryKey_BlogId");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
