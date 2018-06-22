---
title: Výchozí hodnoty - EF jádra
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e541366a-130f-47dd-9997-1b110a11febe
ms.technology: entity-framework-core
uid: core/modeling/relational/default-values
ms.openlocfilehash: 73b916b6d9f9c984c8ea010f2319eafa7d031a58
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054190"
---
# <a name="default-values"></a><span data-ttu-id="13cd4-102">Výchozí hodnoty</span><span class="sxs-lookup"><span data-stu-id="13cd4-102">Default Values</span></span>

> [!NOTE]  
> <span data-ttu-id="13cd4-103">Obecně se vztahuje na relační databáze konfigurace v této části.</span><span class="sxs-lookup"><span data-stu-id="13cd4-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="13cd4-104">Rozšiřující metody zobrazeny zde bude k dispozici při instalaci poskytovatele relační databáze (z důvodu sdílený *Microsoft.EntityFrameworkCore.Relational* balíčku).</span><span class="sxs-lookup"><span data-stu-id="13cd4-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="13cd4-105">Výchozí hodnota sloupce je hodnota, která bude vložen, pokud je vložit nový řádek, ale není zadaná žádná hodnota pro sloupec.</span><span class="sxs-lookup"><span data-stu-id="13cd4-105">The default value of a column is the value that will be inserted if a new row is inserted but no value is specified for the column.</span></span>

## <a name="conventions"></a><span data-ttu-id="13cd4-106">Konvence</span><span class="sxs-lookup"><span data-stu-id="13cd4-106">Conventions</span></span>

<span data-ttu-id="13cd4-107">Podle konvence není nakonfigurován výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="13cd4-107">By convention, a default value is not configured.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="13cd4-108">Datových poznámek</span><span class="sxs-lookup"><span data-stu-id="13cd4-108">Data Annotations</span></span>

<span data-ttu-id="13cd4-109">Nelze nastavit výchozí hodnotu pomocí datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="13cd4-109">You can not set a default value using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="13cd4-110">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="13cd4-110">Fluent API</span></span>

<span data-ttu-id="13cd4-111">Rozhraní Fluent API můžete určit výchozí hodnotu pro vlastnost.</span><span class="sxs-lookup"><span data-stu-id="13cd4-111">You can use the Fluent API to specify the default value for a property.</span></span>

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

<span data-ttu-id="13cd4-112">Můžete také zadat fragment SQL, který se používá k výpočtu výchozí hodnota.</span><span class="sxs-lookup"><span data-stu-id="13cd4-112">You can also specify a SQL fragment that is used to calculate the default value.</span></span>

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
