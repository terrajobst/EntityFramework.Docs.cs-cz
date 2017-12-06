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
# <a name="creating-a-model"></a><span data-ttu-id="21f57-102">Vytvoření modelu</span><span class="sxs-lookup"><span data-stu-id="21f57-102">Creating a Model</span></span>

<span data-ttu-id="21f57-103">Rozhraní Entity Framework vytvoří pomocí sadu pravidel pro model založený na obrazec třídy entity.</span><span class="sxs-lookup"><span data-stu-id="21f57-103">Entity Framework uses a set of conventions to build a model based on the shape of your entity classes.</span></span> <span data-ttu-id="21f57-104">Můžete zadat další konfiguraci doplnit nebo přepsat, co byla zjištěna podle konvence.</span><span class="sxs-lookup"><span data-stu-id="21f57-104">You can specify additional configuration to supplement and/or override what was discovered by convention.</span></span>

<span data-ttu-id="21f57-105">Tento článek se týká konfigurace, který lze použít k modelu cílené na jakékoli úložiště dat a které lze použít, pokud je cílem žádné relační databáze.</span><span class="sxs-lookup"><span data-stu-id="21f57-105">This article covers configuration that can be applied to a model targeting any data store and that which can be applied when targeting any relational database.</span></span> <span data-ttu-id="21f57-106">Zprostředkovatelé může také povolit konfiguraci, která je specifická pro konkrétní datové úložiště.</span><span class="sxs-lookup"><span data-stu-id="21f57-106">Providers may also enable configuration that is specific to a particular data store.</span></span> <span data-ttu-id="21f57-107">Dokumentaci ke konkrétní konfiguraci poskytovatele najdete v článku [zprostředkovatelů databáze](../providers/index.md) části.</span><span class="sxs-lookup"><span data-stu-id="21f57-107">For documentation on provider specific configuration see the [Database Providers](../providers/index.md) section.</span></span>

> [!TIP]  
> <span data-ttu-id="21f57-108">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="21f57-108">You can view this article’s [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) on GitHub.</span></span>

## <a name="methods-of-configuration"></a><span data-ttu-id="21f57-109">Metody konfigurace</span><span class="sxs-lookup"><span data-stu-id="21f57-109">Methods of configuration</span></span>

### <a name="fluent-api"></a><span data-ttu-id="21f57-110">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="21f57-110">Fluent API</span></span>

<span data-ttu-id="21f57-111">Je možné přepsat `OnModelCreating` metoda v odvozené kontextu a použití `ModelBuilder API` ke konfiguraci modelu.</span><span class="sxs-lookup"><span data-stu-id="21f57-111">You can override the `OnModelCreating` method in your derived context and use the `ModelBuilder API` to configure your model.</span></span> <span data-ttu-id="21f57-112">Toto je nejúčinnějších metoda konfigurace a umožňuje konfigurace zadat beze změny třídy entity.</span><span class="sxs-lookup"><span data-stu-id="21f57-112">This is the most powerful method of configuration and allows configuration to be specified without modifying your entity classes.</span></span> <span data-ttu-id="21f57-113">Konfigurace Fluent API má nejvyšší prioritu a přepíše poznámky konvence a data.</span><span class="sxs-lookup"><span data-stu-id="21f57-113">Fluent API configuration has the highest precedence and will override conventions and data annotations.</span></span>

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

### <a name="data-annotations"></a><span data-ttu-id="21f57-114">Datových poznámek</span><span class="sxs-lookup"><span data-stu-id="21f57-114">Data Annotations</span></span>

<span data-ttu-id="21f57-115">Atributy (označované jako datových poznámek) může platit taky pro třídy a vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="21f57-115">You can also apply attributes (known as Data Annotations) to your classes and properties.</span></span> <span data-ttu-id="21f57-116">Poznámky dat se přepíše konvence, ale budou přepsány konfigurace rozhraní Fluent API.</span><span class="sxs-lookup"><span data-stu-id="21f57-116">Data annotations will override conventions, but will be overwritten by Fluent API configuration.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?range=11-16&highlight=4)] -->
``` csharp
    public class Blog
    {
        public int BlogId { get; set; }
        [Required]
        public string Url { get; set; }
    }
```
