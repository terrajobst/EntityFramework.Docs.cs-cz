---
title: Omezení cizího klíče – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: d7ed4466f4df9ec01267b048ba1bbcc6e8bbdad5
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197070"
---
# <a name="foreign-key-constraints"></a><span data-ttu-id="2bdc3-102">Omezení cizího klíče</span><span class="sxs-lookup"><span data-stu-id="2bdc3-102">Foreign Key Constraints</span></span>

> [!NOTE]  
> <span data-ttu-id="2bdc3-103">Konfigurace v této části platí pro relační databáze obecně.</span><span class="sxs-lookup"><span data-stu-id="2bdc3-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="2bdc3-104">Metody rozšíření zobrazené tady budou k dispozici při instalaci zprostředkovatele relační databáze (z důvodu sdíleného balíčku *Microsoft. EntityFrameworkCore. relační* ).</span><span class="sxs-lookup"><span data-stu-id="2bdc3-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="2bdc3-105">Pro každou relaci v modelu je představeno omezení cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="2bdc3-105">A foreign key constraint is introduced for each relationship in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="2bdc3-106">Konvence</span><span class="sxs-lookup"><span data-stu-id="2bdc3-106">Conventions</span></span>

<span data-ttu-id="2bdc3-107">Podle konvence jsou pojmenována `FK_<dependent type name>_<principal type name>_<foreign key property name>`omezení cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="2bdc3-107">By convention, foreign key constraints are named `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span></span> <span data-ttu-id="2bdc3-108">Pro složené cizí klíče `<foreign key property name>` se jako podtržítko oddělí seznam názvů vlastností cizích klíčů.</span><span class="sxs-lookup"><span data-stu-id="2bdc3-108">For composite foreign keys `<foreign key property name>` becomes an underscore separated list of foreign key property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="2bdc3-109">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="2bdc3-109">Data Annotations</span></span>

<span data-ttu-id="2bdc3-110">Názvy omezení cizího klíče nelze konfigurovat pomocí datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="2bdc3-110">Foreign key constraint names cannot be configured using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="2bdc3-111">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="2bdc3-111">Fluent API</span></span>

<span data-ttu-id="2bdc3-112">Rozhraní Fluent API můžete použít ke konfiguraci názvu omezení cizího klíče pro relaci.</span><span class="sxs-lookup"><span data-stu-id="2bdc3-112">You can use the Fluent API to configure the foreign key constraint name for a relationship.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/RelationshipConstraintName.cs?highlight=12)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .HasForeignKey(p => p.BlogId)
            .HasConstraintName("ForeignKey_Post_Blog");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```
