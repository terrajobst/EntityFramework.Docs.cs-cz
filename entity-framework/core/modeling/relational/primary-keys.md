---
title: Primární klíče – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: bdb31964d717c64bddc28e7f1ce55b261e285a9b
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71196948"
---
# <a name="primary-keys"></a><span data-ttu-id="5011c-102">Primární klíče</span><span class="sxs-lookup"><span data-stu-id="5011c-102">Primary Keys</span></span>

> [!NOTE]  
> <span data-ttu-id="5011c-103">Konfigurace v této části platí pro relační databáze obecně.</span><span class="sxs-lookup"><span data-stu-id="5011c-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="5011c-104">Metody rozšíření zobrazené tady budou k dispozici při instalaci zprostředkovatele relační databáze (z důvodu sdíleného balíčku *Microsoft. EntityFrameworkCore. relační* ).</span><span class="sxs-lookup"><span data-stu-id="5011c-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="5011c-105">Pro klíč každého typu entity se zavádí omezení primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="5011c-105">A primary key constraint is introduced for the key of each entity type.</span></span>

## <a name="conventions"></a><span data-ttu-id="5011c-106">Konvence</span><span class="sxs-lookup"><span data-stu-id="5011c-106">Conventions</span></span>

<span data-ttu-id="5011c-107">Podle konvence se primární klíč v databázi jmenuje `PK_<type name>`.</span><span class="sxs-lookup"><span data-stu-id="5011c-107">By convention, the primary key in the database will be named `PK_<type name>`.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="5011c-108">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="5011c-108">Data Annotations</span></span>

<span data-ttu-id="5011c-109">Žádné relační databázové aspekty primárního klíče nelze konfigurovat pomocí datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="5011c-109">No relational database specific aspects of a primary key can be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="5011c-110">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="5011c-110">Fluent API</span></span>

<span data-ttu-id="5011c-111">Rozhraní Fluent API můžete použít ke konfiguraci názvu omezení primárního klíče v databázi.</span><span class="sxs-lookup"><span data-stu-id="5011c-111">You can use the Fluent API to configure the name of the primary key constraint in the database.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/KeyName.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasKey(b => b.BlogId)
            .HasName("PrimaryKey_BlogId");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
