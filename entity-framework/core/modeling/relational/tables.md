---
title: Mapování tabulek – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
uid: core/modeling/relational/tables
ms.openlocfilehash: 32c5e3cc0e498005ce8e6be1f1ee7e8ddf9b510d
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994134"
---
# <a name="table-mapping"></a><span data-ttu-id="3d202-102">Mapování tabulek</span><span class="sxs-lookup"><span data-stu-id="3d202-102">Table Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="3d202-103">Obecně se vztahuje k relačním databázím konfigurace v této části.</span><span class="sxs-lookup"><span data-stu-id="3d202-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="3d202-104">Metody rozšíření je vidět tady bude k dispozici při instalaci poskytovatele relační databáze (z důvodu sdílený *Microsoft.EntityFrameworkCore.Relational* balíček).</span><span class="sxs-lookup"><span data-stu-id="3d202-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="3d202-105">Mapování tabulek identifikuje, jaká data tabulky by měl posílat dotaz z a uloženy v databázi.</span><span class="sxs-lookup"><span data-stu-id="3d202-105">Table mapping identifies which table data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="3d202-106">Konvence</span><span class="sxs-lookup"><span data-stu-id="3d202-106">Conventions</span></span>

<span data-ttu-id="3d202-107">Podle konvence, každá entita nastaví se pro mapování na tabulku s názvem, který `DbSet<TEntity>` vlastnost, která zveřejňuje entity v kontextu odvozené.</span><span class="sxs-lookup"><span data-stu-id="3d202-107">By convention, each entity will be set up to map to a table with the same name as the `DbSet<TEntity>` property that exposes the entity on the derived context.</span></span> <span data-ttu-id="3d202-108">Pokud ne `DbSet<TEntity>` je zahrnuté pro danou entitu, se používá název třídy.</span><span class="sxs-lookup"><span data-stu-id="3d202-108">If no `DbSet<TEntity>` is included for the given entity, the class name is used.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="3d202-109">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="3d202-109">Data Annotations</span></span>

<span data-ttu-id="3d202-110">Vám pomůže anotacemi dat konfigurace, která se mapuje typ tabulky.</span><span class="sxs-lookup"><span data-stu-id="3d202-110">You can use Data Annotations to configure the table that a type maps to.</span></span>

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

<span data-ttu-id="3d202-111">Můžete také určit schématu, který patří tabulky.</span><span class="sxs-lookup"><span data-stu-id="3d202-111">You can also specify a schema that the table belongs to.</span></span>

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="3d202-112">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="3d202-112">Fluent API</span></span>

<span data-ttu-id="3d202-113">Rozhraní Fluent API můžete použít ke konfiguraci, která se mapuje typ tabulka.</span><span class="sxs-lookup"><span data-stu-id="3d202-113">You can use the Fluent API to configure the table that a type maps to.</span></span>

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

<span data-ttu-id="3d202-114">Můžete také určit schématu, který patří tabulky.</span><span class="sxs-lookup"><span data-stu-id="3d202-114">You can also specify a schema that the table belongs to.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/TableAndSchema.cs?highlight=2)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .ToTable("blogs", schema: "blogging");
```
