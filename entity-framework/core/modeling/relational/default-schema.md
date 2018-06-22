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
# <a name="default-schema"></a>Výchozí schéma

> [!NOTE]  
> Obecně se vztahuje na relační databáze konfigurace v této části. Rozšiřující metody zobrazeny zde bude k dispozici při instalaci poskytovatele relační databáze (z důvodu sdílený *Microsoft.EntityFrameworkCore.Relational* balíčku).

Výchozí schéma je schéma databáze, které objekty se mají vytvořit, pokud schéma není explicitně nakonfigurovaný pro tento objekt.

## <a name="conventions"></a>Konvence

Dle konvencí se vybere zprostředkovatel databáze nejvhodnější výchozí schéma. Například bude používat Microsoft SQL Server `dbo` schéma a SQLite nebude používat schéma (protože schémata nejsou podporovány v SQLite).

## <a name="data-annotations"></a>Datových poznámek

Nelze nastavit výchozí schéma pomocí datových poznámek.

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít k určení výchozí schéma.

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
