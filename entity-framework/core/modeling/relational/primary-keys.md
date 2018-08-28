---
title: Primární klíče – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: 916f3adbcd08cb1037c7fbf68e99630feb321a61
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998065"
---
# <a name="primary-keys"></a><span data-ttu-id="47241-102">Primární klíče</span><span class="sxs-lookup"><span data-stu-id="47241-102">Primary Keys</span></span>

> [!NOTE]  
> <span data-ttu-id="47241-103">Obecně se vztahuje k relačním databázím konfigurace v této části.</span><span class="sxs-lookup"><span data-stu-id="47241-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="47241-104">Metody rozšíření je vidět tady bude k dispozici při instalaci poskytovatele relační databáze (z důvodu sdílený *Microsoft.EntityFrameworkCore.Relational* balíček).</span><span class="sxs-lookup"><span data-stu-id="47241-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="47241-105">Omezení primárního klíče se používá pro klíč každého typu entity.</span><span class="sxs-lookup"><span data-stu-id="47241-105">A primary key constraint is introduced for the key of each entity type.</span></span>

## <a name="conventions"></a><span data-ttu-id="47241-106">Konvence</span><span class="sxs-lookup"><span data-stu-id="47241-106">Conventions</span></span>

<span data-ttu-id="47241-107">Podle konvence, bude mít primární klíč v databázi `PK_<type name>`.</span><span class="sxs-lookup"><span data-stu-id="47241-107">By convention, the primary key in the database will be named `PK_<type name>`.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="47241-108">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="47241-108">Data Annotations</span></span>

<span data-ttu-id="47241-109">Žádné konkrétní aspekty relační databáze primární klíč lze konfigurovat pomocí datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="47241-109">No relational database specific aspects of a primary key can be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="47241-110">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="47241-110">Fluent API</span></span>

<span data-ttu-id="47241-111">Rozhraní Fluent API můžete použít ke konfiguraci název omezení primárního klíče v databázi.</span><span class="sxs-lookup"><span data-stu-id="47241-111">You can use the Fluent API to configure the name of the primary key constraint in the database.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/KeyName.cs?highlight=9)] -->
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
