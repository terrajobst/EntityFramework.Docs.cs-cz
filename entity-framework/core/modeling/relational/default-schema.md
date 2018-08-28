---
title: Výchozí schéma – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 800551bbadd0a9e8b5eb7070a8ccf6ed2407e3d2
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995363"
---
# <a name="default-schema"></a><span data-ttu-id="3d226-102">Výchozí schéma</span><span class="sxs-lookup"><span data-stu-id="3d226-102">Default Schema</span></span>

> [!NOTE]  
> <span data-ttu-id="3d226-103">Obecně se vztahuje k relačním databázím konfigurace v této části.</span><span class="sxs-lookup"><span data-stu-id="3d226-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="3d226-104">Metody rozšíření je vidět tady bude k dispozici při instalaci poskytovatele relační databáze (z důvodu sdílený *Microsoft.EntityFrameworkCore.Relational* balíček).</span><span class="sxs-lookup"><span data-stu-id="3d226-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="3d226-105">Výchozí schéma je schéma databáze, které objekty se vytvoří v, pokud schéma není pro daný objekt explicitně nakonfigurovat.</span><span class="sxs-lookup"><span data-stu-id="3d226-105">The default schema is the database schema that objects will be created in if a schema is not explicitly configured for that object.</span></span>

## <a name="conventions"></a><span data-ttu-id="3d226-106">Konvence</span><span class="sxs-lookup"><span data-stu-id="3d226-106">Conventions</span></span>

<span data-ttu-id="3d226-107">Podle konvence poskytovatele databáze zvolí nejvhodnější výchozí schéma.</span><span class="sxs-lookup"><span data-stu-id="3d226-107">By convention, the database provider will choose the most appropriate default schema.</span></span> <span data-ttu-id="3d226-108">Například se bude používat Microsoft SQL Server `dbo` schématu a SQLite nebudeme používat schéma (protože schémata nejsou podporovány v SQLite).</span><span class="sxs-lookup"><span data-stu-id="3d226-108">For example, Microsoft SQL Server will use the `dbo` schema and SQLite will not use a schema (since schemas are not supported in SQLite).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="3d226-109">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="3d226-109">Data Annotations</span></span>

<span data-ttu-id="3d226-110">Nelze nastavit výchozí schéma používání datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="3d226-110">You can not set the default schema using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="3d226-111">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="3d226-111">Fluent API</span></span>

<span data-ttu-id="3d226-112">Rozhraní Fluent API můžete použít k určení výchozí schéma.</span><span class="sxs-lookup"><span data-stu-id="3d226-112">You can use the Fluent API to specify a default schema.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/DefaultSchema.cs?highlight=7)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasDefaultSchema("blogging");
    }
}
```
