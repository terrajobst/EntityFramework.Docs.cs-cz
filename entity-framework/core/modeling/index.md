---
title: "Vytvoření modelu - EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
ms.technology: entity-framework-core
uid: core/modeling/index
ms.openlocfilehash: 1ad0f6891fbc8ba2e4d102cc9997f053a9dddb66
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="creating-a-model"></a>Vytvoření modelu

Rozhraní Entity Framework vytvoří pomocí sadu pravidel pro model založený na obrazec třídy entity. Můžete zadat další konfiguraci doplnit nebo přepsat, co byla zjištěna podle konvence.

Tento článek se týká konfigurace, který lze použít k modelu cílené na jakékoli úložiště dat a které lze použít, pokud je cílem žádné relační databáze. Zprostředkovatelé může také povolit konfiguraci, která je specifická pro konkrétní datové úložiště. Dokumentaci ke konkrétní konfiguraci poskytovatele najdete v článku [zprostředkovatelů databáze](../providers/index.md) části.

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) na Githubu.

## <a name="methods-of-configuration"></a>Metody konfigurace

### <a name="fluent-api"></a>Rozhraní Fluent API

Je možné přepsat `OnModelCreating` metoda v odvozené kontextu a použití `ModelBuilder API` ke konfiguraci modelu. Toto je nejúčinnějších metoda konfigurace a umožňuje konfigurace zadat beze změny třídy entity. Konfigurace Fluent API má nejvyšší prioritu a přepíše poznámky konvence a data.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Required.cs?range=5-15&highlight=5-10)] -->

``` csharp
    class MyContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<Blog>()
                .Property(b => b.Url)
                .IsRequired();
        }
    }
```

### <a name="data-annotations"></a>Datových poznámek

Atributy (označované jako datových poznámek) může platit taky pro třídy a vlastnosti. Poznámky dat se přepíše konvence, ale budou přepsány konfigurace rozhraní Fluent API.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?range=11-16&highlight=4)] -->
``` csharp
    public class Blog
    {
        public int BlogId { get; set; }
        [Required]
        public string Url { get; set; }
    }
```
