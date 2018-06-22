---
title: Výchozí schéma - EF jádra
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
ms.technology: entity-framework-core
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 26106deb2d4e35ecf33e97790a83f9af77991aed
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054187"
---
# <a name="default-schema"></a><span data-ttu-id="bebae-102">Výchozí schéma</span><span class="sxs-lookup"><span data-stu-id="bebae-102">Default Schema</span></span>

> [!NOTE]  
> <span data-ttu-id="bebae-103">Obecně se vztahuje na relační databáze konfigurace v této části.</span><span class="sxs-lookup"><span data-stu-id="bebae-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="bebae-104">Rozšiřující metody zobrazeny zde bude k dispozici při instalaci poskytovatele relační databáze (z důvodu sdílený *Microsoft.EntityFrameworkCore.Relational* balíčku).</span><span class="sxs-lookup"><span data-stu-id="bebae-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="bebae-105">Výchozí schéma je schéma databáze, které objekty se mají vytvořit, pokud schéma není explicitně nakonfigurovaný pro tento objekt.</span><span class="sxs-lookup"><span data-stu-id="bebae-105">The default schema is the database schema that objects will be created in if a schema is not explicitly configured for that object.</span></span>

## <a name="conventions"></a><span data-ttu-id="bebae-106">Konvence</span><span class="sxs-lookup"><span data-stu-id="bebae-106">Conventions</span></span>

<span data-ttu-id="bebae-107">Dle konvencí se vybere zprostředkovatel databáze nejvhodnější výchozí schéma.</span><span class="sxs-lookup"><span data-stu-id="bebae-107">By convention, the database provider will choose the most appropriate default schema.</span></span> <span data-ttu-id="bebae-108">Například bude používat Microsoft SQL Server `dbo` schéma a SQLite nebude používat schéma (protože schémata nejsou podporovány v SQLite).</span><span class="sxs-lookup"><span data-stu-id="bebae-108">For example, Microsoft SQL Server will use the `dbo` schema and SQLite will not use a schema (since schemas are not supported in SQLite).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="bebae-109">Datových poznámek</span><span class="sxs-lookup"><span data-stu-id="bebae-109">Data Annotations</span></span>

<span data-ttu-id="bebae-110">Nelze nastavit výchozí schéma pomocí datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="bebae-110">You can not set the default schema using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="bebae-111">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="bebae-111">Fluent API</span></span>

<span data-ttu-id="bebae-112">Rozhraní Fluent API můžete použít k určení výchozí schéma.</span><span class="sxs-lookup"><span data-stu-id="bebae-112">You can use the Fluent API to specify a default schema.</span></span>

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
