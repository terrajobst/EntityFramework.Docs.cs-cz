---
title: Mapování tabulky – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
uid: core/modeling/relational/tables
ms.openlocfilehash: 474c49aca4c65cd5d58b184b1f3c2d30e7abff84
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656102"
---
# <a name="table-mapping"></a><span data-ttu-id="ecf5f-102">Mapování tabulek</span><span class="sxs-lookup"><span data-stu-id="ecf5f-102">Table Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="ecf5f-103">Konfigurace v této části platí pro relační databáze obecně.</span><span class="sxs-lookup"><span data-stu-id="ecf5f-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="ecf5f-104">Metody rozšíření zobrazené tady budou k dispozici při instalaci zprostředkovatele relační databáze (z důvodu sdíleného balíčku *Microsoft. EntityFrameworkCore. relační* ).</span><span class="sxs-lookup"><span data-stu-id="ecf5f-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="ecf5f-105">Mapování tabulky určuje, která data tabulky by se měla dotazovat a Uložit do databáze.</span><span class="sxs-lookup"><span data-stu-id="ecf5f-105">Table mapping identifies which table data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="ecf5f-106">Konvence</span><span class="sxs-lookup"><span data-stu-id="ecf5f-106">Conventions</span></span>

<span data-ttu-id="ecf5f-107">Podle konvence se Každá entita nastaví tak, aby se namapovala na tabulku se stejným názvem, jako má vlastnost `DbSet<TEntity>`, která zpřístupňuje entitu v odvozeném kontextu.</span><span class="sxs-lookup"><span data-stu-id="ecf5f-107">By convention, each entity will be set up to map to a table with the same name as the `DbSet<TEntity>` property that exposes the entity on the derived context.</span></span> <span data-ttu-id="ecf5f-108">Pokud není pro danou entitu zahrnutá žádná `DbSet<TEntity>`, použije se název třídy.</span><span class="sxs-lookup"><span data-stu-id="ecf5f-108">If no `DbSet<TEntity>` is included for the given entity, the class name is used.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="ecf5f-109">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="ecf5f-109">Data Annotations</span></span>

<span data-ttu-id="ecf5f-110">K nakonfigurování tabulky, na kterou je typ mapován, můžete použít datové poznámky.</span><span class="sxs-lookup"><span data-stu-id="ecf5f-110">You can use Data Annotations to configure the table that a type maps to.</span></span>

``` csharp
using System.ComponentModel.DataAnnotations.Schema;

[Table("blogs")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="ecf5f-111">Můžete také zadat schéma, do kterého tabulka patří.</span><span class="sxs-lookup"><span data-stu-id="ecf5f-111">You can also specify a schema that the table belongs to.</span></span>

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="ecf5f-112">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="ecf5f-112">Fluent API</span></span>

<span data-ttu-id="ecf5f-113">Rozhraní Fluent API můžete použít ke konfiguraci tabulky, na kterou se typ mapuje.</span><span class="sxs-lookup"><span data-stu-id="ecf5f-113">You can use the Fluent API to configure the table that a type maps to.</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;

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

<span data-ttu-id="ecf5f-114">Můžete také zadat schéma, do kterého tabulka patří.</span><span class="sxs-lookup"><span data-stu-id="ecf5f-114">You can also specify a schema that the table belongs to.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/TableAndSchema.cs?name=Table&highlight=2)]
