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
# <a name="default-values"></a><span data-ttu-id="e50e1-102">Výchozí hodnoty</span><span class="sxs-lookup"><span data-stu-id="e50e1-102">Default Values</span></span>

> [!NOTE]  
> <span data-ttu-id="e50e1-103">Obecně se vztahuje k relačním databázím konfigurace v této části.</span><span class="sxs-lookup"><span data-stu-id="e50e1-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="e50e1-104">Metody rozšíření je vidět tady bude k dispozici při instalaci poskytovatele relační databáze (z důvodu sdílený *Microsoft.EntityFrameworkCore.Relational* balíček).</span><span class="sxs-lookup"><span data-stu-id="e50e1-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="e50e1-105">Výchozí hodnota pro sloupec je hodnota, která bude vložen, pokud se vloží nový řádek, ale není zadaná žádná hodnota pro sloupec.</span><span class="sxs-lookup"><span data-stu-id="e50e1-105">The default value of a column is the value that will be inserted if a new row is inserted but no value is specified for the column.</span></span>

## <a name="conventions"></a><span data-ttu-id="e50e1-106">Konvence</span><span class="sxs-lookup"><span data-stu-id="e50e1-106">Conventions</span></span>

<span data-ttu-id="e50e1-107">Podle konvence není nakonfigurovaný výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="e50e1-107">By convention, a default value is not configured.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="e50e1-108">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="e50e1-108">Data Annotations</span></span>

<span data-ttu-id="e50e1-109">Nelze nastavit výchozí hodnotu pomocí datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="e50e1-109">You can not set a default value using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="e50e1-110">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="e50e1-110">Fluent API</span></span>

<span data-ttu-id="e50e1-111">Rozhraní Fluent API můžete zadat výchozí hodnotu pro vlastnost.</span><span class="sxs-lookup"><span data-stu-id="e50e1-111">You can use the Fluent API to specify the default value for a property.</span></span>

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

<span data-ttu-id="e50e1-112">Můžete také zadat fragment SQL, který se používá k výpočtu výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="e50e1-112">You can also specify a SQL fragment that is used to calculate the default value.</span></span>

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
