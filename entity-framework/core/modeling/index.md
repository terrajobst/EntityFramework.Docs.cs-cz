---
title: Vytvoření modelu – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: 9f702d5833b88e6eb77c0afefdae0ed3bc162ec8
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993930"
---
# <a name="creating-a-model"></a>Vytvoření modelu

Entity Framework používá sadu konvence sestavit model založený na obrazec tříd entit. Můžete zadat další konfiguraci k doplnění a/nebo přepsat, co bylo zjištěno konvencí.

Tento článek se týká konfigurace, který lze použít k modelu, který cílí na jakékoli úložiště dat a který, který je možné použít při cílení na jakoukoli relační databázi. Poskytovatelé může také umožnit konfiguraci, která je specifická pro konkrétní datové úložiště. Dokumentace ke službě na konkrétní konfiguraci poskytovatele najdete v článku [poskytovatelé databází](../providers/index.md) oddílu.

> [!TIP]  
> Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) na Githubu.

## <a name="methods-of-configuration"></a>Metodě konfigurace

### <a name="fluent-api"></a>Rozhraní Fluent API

Je možné přepsat `OnModelCreating` metoda v odvozené kontextu a použití `ModelBuilder API` ke konfiguraci modelu. Toto je nejvýkonnější metody konfigurace a umožňuje konfiguraci zadat bez změny vašich tříd entit. Konfigurace Fluent API má nejvyšší prioritu a přepíše poznámky konvence a data.

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

### <a name="data-annotations"></a>Datové poznámky

Můžete také použít atributy (označuje se jako datové poznámky) do vaší třídy a vlastnosti. Datové poznámky přepíše konvence, ale rozhraní Fluent API konfigurace se přepíše.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?range=11-16&highlight=4)] -->
``` csharp
    public class Blog
    {
        public int BlogId { get; set; }
        [Required]
        public string Url { get; set; }
    }
```
