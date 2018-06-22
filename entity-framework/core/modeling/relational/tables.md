---
title: Mapování tabulky - EF jádra
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
ms.technology: entity-framework-core
uid: core/modeling/relational/tables
ms.openlocfilehash: 73957d9c77e6856bfb65e10e6b373c337101f7d9
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054181"
---
# <a name="table-mapping"></a><span data-ttu-id="adba0-102">Mapování tabulek</span><span class="sxs-lookup"><span data-stu-id="adba0-102">Table Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="adba0-103">Obecně se vztahuje na relační databáze konfigurace v této části.</span><span class="sxs-lookup"><span data-stu-id="adba0-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="adba0-104">Rozšiřující metody zobrazeny zde bude k dispozici při instalaci poskytovatele relační databáze (z důvodu sdílený *Microsoft.EntityFrameworkCore.Relational* balíčku).</span><span class="sxs-lookup"><span data-stu-id="adba0-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="adba0-105">Mapování tabulky identifikuje tabulky datových by měl být získaných z a uloženy v databázi.</span><span class="sxs-lookup"><span data-stu-id="adba0-105">Table mapping identifies which table data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="adba0-106">Konvence</span><span class="sxs-lookup"><span data-stu-id="adba0-106">Conventions</span></span>

<span data-ttu-id="adba0-107">Podle konvence, každá entita bude instalační program k mapování na tabulku se stejným názvem jako `DbSet<TEntity>` vlastnost, která zveřejňuje se entita na odvozené kontextu.</span><span class="sxs-lookup"><span data-stu-id="adba0-107">By convention, each entity will be setup to map to a table with the same name as the `DbSet<TEntity>` property that exposes the entity on the derived context.</span></span> <span data-ttu-id="adba0-108">Pokud žádné `DbSet<TEntity>` je zahrnuté pro danou entitu, se používá název třídy.</span><span class="sxs-lookup"><span data-stu-id="adba0-108">If no `DbSet<TEntity>` is included for the given entity, the class name is used.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="adba0-109">Datových poznámek</span><span class="sxs-lookup"><span data-stu-id="adba0-109">Data Annotations</span></span>

<span data-ttu-id="adba0-110">Můžete nakonfigurovat tabulku, která se mapuje typ datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="adba0-110">You can use Data Annotations to configure the table that a type maps to.</span></span>

``` csharp
using System.ComponentModel.DataAnnotations.Schema;
```
``` csharp
[Table("blogs")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="adba0-111">Můžete také zadat schématu, který je v tabulce součástí.</span><span class="sxs-lookup"><span data-stu-id="adba0-111">You can also specify a schema that the table belongs to.</span></span>

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="adba0-112">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="adba0-112">Fluent API</span></span>

<span data-ttu-id="adba0-113">Můžete nakonfigurovat v tabulce, která se mapuje typ na rozhraní Fluent API.</span><span class="sxs-lookup"><span data-stu-id="adba0-113">You can use the Fluent API to configure the table that a type maps to.</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
```
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .ToTable("blogs");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="adba0-114">Můžete také zadat schématu, který je v tabulce součástí.</span><span class="sxs-lookup"><span data-stu-id="adba0-114">You can also specify a schema that the table belongs to.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/TableAndSchema.cs?highlight=2)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .ToTable("blogs", schema: "blogging");
```
