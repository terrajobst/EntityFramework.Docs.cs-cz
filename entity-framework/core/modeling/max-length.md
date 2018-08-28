---
title: Maximální délka – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
uid: core/modeling/max-length
ms.openlocfilehash: e54d3671f378b96a49eaf4cb312e72072813fc6d
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996188"
---
# <a name="maximum-length"></a>Maximální délka

Konfigurace maximální délku poskytuje nápovědu pro úložiště dat o správný typ dat chcete použít pro danou vlastnost. Maximální délka platí jenom pro pole datové typy, jako například `string` a `byte[]`.

> [!NOTE]  
> Entity Framework neprovádí žádné ověření s maximální délkou před předáním dat do zprostředkovatele. Záleží poskytovatele nebo úložišti dat pro ověření v případě potřeby. Například při cílení na SQL serveru, překračuje maximální délku způsobí výjimku jako datový typ základního sloupce neumožní nadbytečná data k uložení.

## <a name="conventions"></a>Konvence

Podle konvence je ponecháno až poskytovatel databáze vybrat příslušný datový typ pro vlastnosti. Pro vlastnosti, které mají délku poskytovatel databáze obecně zvolte datový typ, který umožňuje nejdelší dobu data. Například se bude používat Microsoft SQL Server `nvarchar(max)` pro `string` vlastnosti (nebo `nvarchar(450)` Pokud sloupec se používá jako klíč).

## <a name="data-annotations"></a>Datové poznámky

Anotacemi dat můžete použít ke konfiguraci maximální délka pro vlastnost. V tomto příkladu cílení na systém SQL Server, výsledkem by `nvarchar(500)` datový typ používá.

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

Rozhraní Fluent API můžete použít ke konfiguraci maximální délka pro vlastnost. V tomto příkladu cílení na systém SQL Server, výsledkem by `nvarchar(500)` datový typ používá.

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
