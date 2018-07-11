---
title: Mapování sloupce – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
ms.technology: entity-framework-core
uid: core/modeling/relational/columns
ms.openlocfilehash: ac3ab2ce3faa54eb8e862d01dcecb48cb0d1f811
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949408"
---
# <a name="column-mapping"></a><span data-ttu-id="79e2c-102">Mapování sloupce</span><span class="sxs-lookup"><span data-stu-id="79e2c-102">Column Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="79e2c-103">Obecně se vztahuje k relačním databázím konfigurace v této části.</span><span class="sxs-lookup"><span data-stu-id="79e2c-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="79e2c-104">Metody rozšíření je vidět tady bude k dispozici při instalaci poskytovatele relační databáze (z důvodu sdílený *Microsoft.EntityFrameworkCore.Relational* balíček).</span><span class="sxs-lookup"><span data-stu-id="79e2c-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="79e2c-105">Mapování sloupce určuje, jaká data sloupec by měl posílat dotaz z a uloženy v databázi.</span><span class="sxs-lookup"><span data-stu-id="79e2c-105">Column mapping identifies which column data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="79e2c-106">Konvence</span><span class="sxs-lookup"><span data-stu-id="79e2c-106">Conventions</span></span>

<span data-ttu-id="79e2c-107">Podle konvence se k mapování sloupců se stejným názvem jako vlastnost nastavení každou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="79e2c-107">By convention, each property will be set up to map to a column with the same name as the property.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="79e2c-108">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="79e2c-108">Data Annotations</span></span>

<span data-ttu-id="79e2c-109">Anotací dat můžete použít ke konfiguraci sloupců, ke kterému je namapována vlastnost.</span><span class="sxs-lookup"><span data-stu-id="79e2c-109">You can use Data Annotations to configure the column to which a property is mapped.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/DataAnnotations/Samples/Relational/Column.cs?highlight=3)] -->
``` csharp
public class Blog
{
    [Column("blog_id")]
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="79e2c-110">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="79e2c-110">Fluent API</span></span>

<span data-ttu-id="79e2c-111">Rozhraní Fluent API můžete použít ke konfiguraci sloupců, ke kterému je namapována vlastnost.</span><span class="sxs-lookup"><span data-stu-id="79e2c-111">You can use the Fluent API to configure the column to which a property is mapped.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/Column.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.BlogId)
            .HasColumnName("blog_id");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
