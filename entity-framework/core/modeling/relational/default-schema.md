---
title: Výchozí schéma – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 800551bbadd0a9e8b5eb7070a8ccf6ed2407e3d2
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995363"
---
# <a name="default-schema"></a>Výchozí schéma

> [!NOTE]  
> Obecně se vztahuje k relačním databázím konfigurace v této části. Metody rozšíření je vidět tady bude k dispozici při instalaci poskytovatele relační databáze (z důvodu sdílený *Microsoft.EntityFrameworkCore.Relational* balíček).

Výchozí schéma je schéma databáze, které objekty se vytvoří v, pokud schéma není pro daný objekt explicitně nakonfigurovat.

## <a name="conventions"></a>Konvence

Podle konvence poskytovatele databáze zvolí nejvhodnější výchozí schéma. Například se bude používat Microsoft SQL Server `dbo` schématu a SQLite nebudeme používat schéma (protože schémata nejsou podporovány v SQLite).

## <a name="data-annotations"></a>Datové poznámky

Nelze nastavit výchozí schéma používání datových poznámek.

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít k určení výchozí schéma.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/DefaultSchema.cs?highlight=7)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasDefaultSchema("blogging");
    }
}
```
