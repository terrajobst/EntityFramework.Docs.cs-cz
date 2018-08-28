---
title: Vypočítané sloupce – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: b88efdf69e5100e4eff55f3a41925d2d8e7c3178
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993950"
---
# <a name="computed-columns"></a>Vypočítané sloupce

> [!NOTE]  
> Obecně se vztahuje k relačním databázím konfigurace v této části. Metody rozšíření je vidět tady bude k dispozici při instalaci poskytovatele relační databáze (z důvodu sdílený *Microsoft.EntityFrameworkCore.Relational* balíček).

Počítaný sloupec je sloupec, jehož hodnota je vypočítána v databázi. Počítaný sloupec můžete použít jiné sloupce v tabulce k výpočtu jeho hodnotu.

## <a name="conventions"></a>Konvence

Podle konvence nejsou počítané sloupce vytvořené v modelu.

## <a name="data-annotations"></a>Datové poznámky

S anotacemi dat nelze konfigurovat vypočítané sloupce.

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít k určení, že vlastnost by měla být mapována k počítanému sloupci.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/ComputedColumn.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Person> People { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Person>()
            .Property(p => p.DisplayName)
            .HasComputedColumnSql("[LastName] + ', ' + [FirstName]");
    }
}

public class Person
{
    public int PersonId { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string DisplayName { get; set; }
}
```
