---
title: Mapování sloupce – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
uid: core/modeling/relational/columns
ms.openlocfilehash: bca9ca22d211aa58a3bba00f6e4d54b8fe4a0df8
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996201"
---
# <a name="column-mapping"></a><span data-ttu-id="b1395-102">Mapování sloupce</span><span class="sxs-lookup"><span data-stu-id="b1395-102">Column Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="b1395-103">Obecně se vztahuje k relačním databázím konfigurace v této části.</span><span class="sxs-lookup"><span data-stu-id="b1395-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="b1395-104">Metody rozšíření je vidět tady bude k dispozici při instalaci poskytovatele relační databáze (z důvodu sdílený *Microsoft.EntityFrameworkCore.Relational* balíček).</span><span class="sxs-lookup"><span data-stu-id="b1395-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="b1395-105">Mapování sloupce určuje, jaká data sloupec by měl posílat dotaz z a uloženy v databázi.</span><span class="sxs-lookup"><span data-stu-id="b1395-105">Column mapping identifies which column data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="b1395-106">Konvence</span><span class="sxs-lookup"><span data-stu-id="b1395-106">Conventions</span></span>

<span data-ttu-id="b1395-107">Podle konvence se k mapování sloupců se stejným názvem jako vlastnost nastavení každou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="b1395-107">By convention, each property will be set up to map to a column with the same name as the property.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="b1395-108">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="b1395-108">Data Annotations</span></span>

<span data-ttu-id="b1395-109">Anotací dat můžete použít ke konfiguraci sloupců, ke kterému je namapována vlastnost.</span><span class="sxs-lookup"><span data-stu-id="b1395-109">You can use Data Annotations to configure the column to which a property is mapped.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/DataAnnotations/Samples/Relational/Column.cs?highlight=3)] -->
``` csharp
public class Blog
{
    [Column("blog_id")]
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="b1395-110">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="b1395-110">Fluent API</span></span>

<span data-ttu-id="b1395-111">Rozhraní Fluent API můžete použít ke konfiguraci sloupců, ke kterému je namapována vlastnost.</span><span class="sxs-lookup"><span data-stu-id="b1395-111">You can use the Fluent API to configure the column to which a property is mapped.</span></span>

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
