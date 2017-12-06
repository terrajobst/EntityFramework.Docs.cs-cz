---
title: "Indexy – EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
ms.technology: entity-framework-core
uid: core/modeling/relational/indexes
ms.openlocfilehash: 683b580bb155e0416f13c5d63e3280078fbcee21
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="indexes"></a><span data-ttu-id="f70f6-102">Indexy</span><span class="sxs-lookup"><span data-stu-id="f70f6-102">Indexes</span></span>

> [!NOTE]  
> <span data-ttu-id="f70f6-103">Obecně se vztahuje na relační databáze konfigurace v této části.</span><span class="sxs-lookup"><span data-stu-id="f70f6-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="f70f6-104">Rozšiřující metody zobrazeny zde bude k dispozici při instalaci poskytovatele relační databáze (z důvodu sdílený *Microsoft.EntityFrameworkCore.Relational* balíčku).</span><span class="sxs-lookup"><span data-stu-id="f70f6-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="f70f6-105">Index v relační databázi se mapuje na stejný koncept jako index v základní rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f70f6-105">An index in a relational database maps to the same concept as an index in the core of Entity Framework.</span></span>

## <a name="conventions"></a><span data-ttu-id="f70f6-106">Konvence</span><span class="sxs-lookup"><span data-stu-id="f70f6-106">Conventions</span></span>

<span data-ttu-id="f70f6-107">Podle konvence, jsou pojmenované indexy `IX_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="f70f6-107">By convention, indexes are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="f70f6-108">Pro složené indexy `<property name>` stane podtržítka oddělený seznam názvů vlastností.</span><span class="sxs-lookup"><span data-stu-id="f70f6-108">For composite indexes `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="f70f6-109">Datových poznámek</span><span class="sxs-lookup"><span data-stu-id="f70f6-109">Data Annotations</span></span>

<span data-ttu-id="f70f6-110">Indexy nelze konfigurovat pomocí datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="f70f6-110">Indexes can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="f70f6-111">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="f70f6-111">Fluent API</span></span>

<span data-ttu-id="f70f6-112">Rozhraní Fluent API můžete použít ke konfiguraci název indexu.</span><span class="sxs-lookup"><span data-stu-id="f70f6-112">You can use the Fluent API to configure the name of an index.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/IndexName.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .HasName("Index_Url");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
