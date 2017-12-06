---
title: "Maximální délka - EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
ms.technology: entity-framework-core
uid: core/modeling/max-length
ms.openlocfilehash: 7325c0c3328477473392bf9e7c82f1696bb4f424
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="maximum-length"></a>Maximální délka

Konfigurace maximální délku poskytuje nápovědu k úložišti dat o příslušný datový typ chcete použít pro danou vlastnost. Maximální délka se vztahuje pouze na pole datové typy, jako například `string` a `byte[]`.

> [!NOTE]  
> Rozhraní Entity Framework neprovádí žádné ověření maximální délku před předávání dat zprostředkovatele. Je zprostředkovatele nebo datového úložiště, ověřte, jestli je to vhodné. Například pokud cílení na SQL Server, přesahuje maximální délku bude mít za následek výjimku jako datový typ sloupce základní neumožní nadbytečná data k uložení.

## <a name="conventions"></a>Konvence

Podle konvence je ponechán až zprostředkovatel databáze k výběru typu příslušná data pro vlastnosti. Pro vlastnosti, které mají délku zprostředkovatel databáze obvykle vyberte datový typ, který umožňuje nejdelší dobu data. Například Microsoft SQL Server použije `nvarchar(max)` pro `string` vlastnosti (nebo `nvarchar(450)` Pokud sloupec slouží jako klíč).

## <a name="data-annotations"></a>Datových poznámek

Můžete nakonfigurovat maximální délku pro vlastnost datových poznámek. V tomto příkladu cílení na SQL Server, které jsou výsledkem by `nvarchar(500)` datový typ používaný.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/MaxLength.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [MaxLength(500)]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít ke konfiguraci maximální délka pro vlastnost. V tomto příkladu cílení na SQL Server, které jsou výsledkem by `nvarchar(500)` datový typ používaný.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/MaxLength.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Url)
            .HasMaxLength(500);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
