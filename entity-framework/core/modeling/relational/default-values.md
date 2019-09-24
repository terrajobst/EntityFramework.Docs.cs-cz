---
title: Výchozí hodnoty – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e541366a-130f-47dd-9997-1b110a11febe
uid: core/modeling/relational/default-values
ms.openlocfilehash: 0d3613606f21a78e22cfe0ee752ea982a6a17f93
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71196982"
---
# <a name="default-values"></a><span data-ttu-id="87e1e-102">Výchozí hodnoty</span><span class="sxs-lookup"><span data-stu-id="87e1e-102">Default Values</span></span>

> [!NOTE]  
> <span data-ttu-id="87e1e-103">Konfigurace v této části platí pro relační databáze obecně.</span><span class="sxs-lookup"><span data-stu-id="87e1e-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="87e1e-104">Metody rozšíření zobrazené tady budou k dispozici při instalaci zprostředkovatele relační databáze (z důvodu sdíleného balíčku *Microsoft. EntityFrameworkCore. relační* ).</span><span class="sxs-lookup"><span data-stu-id="87e1e-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="87e1e-105">Výchozí hodnota sloupce je hodnota, která bude vložena při vložení nového řádku, ale pro sloupec není zadána žádná hodnota.</span><span class="sxs-lookup"><span data-stu-id="87e1e-105">The default value of a column is the value that will be inserted if a new row is inserted but no value is specified for the column.</span></span>

## <a name="conventions"></a><span data-ttu-id="87e1e-106">Konvence</span><span class="sxs-lookup"><span data-stu-id="87e1e-106">Conventions</span></span>

<span data-ttu-id="87e1e-107">Podle konvence není výchozí hodnota nakonfigurovaná.</span><span class="sxs-lookup"><span data-stu-id="87e1e-107">By convention, a default value is not configured.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="87e1e-108">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="87e1e-108">Data Annotations</span></span>

<span data-ttu-id="87e1e-109">Výchozí hodnotu nelze nastavit pomocí datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="87e1e-109">You can not set a default value using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="87e1e-110">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="87e1e-110">Fluent API</span></span>

<span data-ttu-id="87e1e-111">Rozhraní Fluent API můžete použít k určení výchozí hodnoty pro vlastnost.</span><span class="sxs-lookup"><span data-stu-id="87e1e-111">You can use the Fluent API to specify the default value for a property.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/DefaultValue.cs?highlight=9)] -->
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

<span data-ttu-id="87e1e-112">Můžete také zadat fragment SQL, který se použije k výpočtu výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="87e1e-112">You can also specify a SQL fragment that is used to calculate the default value.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/DefaultValueSql.cs?highlight=9)] -->
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
