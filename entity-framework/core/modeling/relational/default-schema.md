---
title: Výchozí schéma – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: ae903ed7200859430aecc55073651236759bc6ce
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197127"
---
# <a name="default-schema"></a>Výchozí schéma

> [!NOTE]  
> Konfigurace v této části platí pro relační databáze obecně. Metody rozšíření zobrazené tady budou k dispozici při instalaci zprostředkovatele relační databáze (z důvodu sdíleného balíčku *Microsoft. EntityFrameworkCore. relační* ).

Výchozím schématem je schéma databáze, ve kterém budou objekty vytvořeny, pokud není pro daný objekt explicitně nakonfigurováno schéma.

## <a name="conventions"></a>Konvence

Podle konvence poskytovatel databáze zvolí nejvhodnější výchozí schéma. Například Microsoft SQL Server použije `dbo` schéma a SQLite nepoužije schéma (vzhledem k tomu, že schémata nejsou v SQLite podporovaná).

## <a name="data-annotations"></a>Datové poznámky

Výchozí schéma nelze nastavit pomocí datových poznámek.

## <a name="fluent-api"></a>Rozhraní Fluent API

Rozhraní Fluent API můžete použít k určení výchozího schématu.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/DefaultSchema.cs?highlight=7)] -->
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
