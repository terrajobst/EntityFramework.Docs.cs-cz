---
title: Primární klíče - EF jádra
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
ms.technology: entity-framework-core
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: fcb1871149c0f20a2576864028b4171904de1982
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054178"
---
# <a name="primary-keys"></a><span data-ttu-id="efb73-102">Primární klíče</span><span class="sxs-lookup"><span data-stu-id="efb73-102">Primary Keys</span></span>

> [!NOTE]  
> <span data-ttu-id="efb73-103">Obecně se vztahuje na relační databáze konfigurace v této části.</span><span class="sxs-lookup"><span data-stu-id="efb73-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="efb73-104">Rozšiřující metody zobrazeny zde bude k dispozici při instalaci poskytovatele relační databáze (z důvodu sdílený *Microsoft.EntityFrameworkCore.Relational* balíčku).</span><span class="sxs-lookup"><span data-stu-id="efb73-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="efb73-105">Omezení primárního klíče se zavedl pro klíč každému typu entity.</span><span class="sxs-lookup"><span data-stu-id="efb73-105">A primary key constraint is introduced for the key of each entity type.</span></span>

## <a name="conventions"></a><span data-ttu-id="efb73-106">Konvence</span><span class="sxs-lookup"><span data-stu-id="efb73-106">Conventions</span></span>

<span data-ttu-id="efb73-107">Podle konvence, bude mít název primární klíč v databázi `PK_<type name>`.</span><span class="sxs-lookup"><span data-stu-id="efb73-107">By convention, the primary key in the database will be named `PK_<type name>`.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="efb73-108">Datových poznámek</span><span class="sxs-lookup"><span data-stu-id="efb73-108">Data Annotations</span></span>

<span data-ttu-id="efb73-109">Žádné specifické aspekty relační databáze primární klíč můžete konfigurovat pomocí datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="efb73-109">No relational database specific aspects of a primary key can be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="efb73-110">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="efb73-110">Fluent API</span></span>

<span data-ttu-id="efb73-111">Rozhraní Fluent API můžete použít ke konfiguraci název omezení primárního klíče v databázi.</span><span class="sxs-lookup"><span data-stu-id="efb73-111">You can use the Fluent API to configure the name of the primary key constraint in the database.</span></span>

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
