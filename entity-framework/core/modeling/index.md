---
title: Vytvoření modelu – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: e4eed480178ce43cbc5ece8db8e584032da7b2b9
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250345"
---
# <a name="creating-and-configuring-a-model"></a><span data-ttu-id="becf3-102">Vytváření a konfiguraci modelu</span><span class="sxs-lookup"><span data-stu-id="becf3-102">Creating and configuring a Model</span></span>

<span data-ttu-id="becf3-103">Entity Framework používá sadu konvence sestavit model založený na obrazec tříd entit.</span><span class="sxs-lookup"><span data-stu-id="becf3-103">Entity Framework uses a set of conventions to build a model based on the shape of your entity classes.</span></span> <span data-ttu-id="becf3-104">Můžete zadat další konfiguraci k doplnění a/nebo přepsat, co bylo zjištěno konvencí.</span><span class="sxs-lookup"><span data-stu-id="becf3-104">You can specify additional configuration to supplement and/or override what was discovered by convention.</span></span>

<span data-ttu-id="becf3-105">Tento článek se týká konfigurace, který lze použít k modelu, který cílí na jakékoli úložiště dat a který, který je možné použít při cílení na jakoukoli relační databázi.</span><span class="sxs-lookup"><span data-stu-id="becf3-105">This article covers configuration that can be applied to a model targeting any data store and that which can be applied when targeting any relational database.</span></span> <span data-ttu-id="becf3-106">Poskytovatelé může také umožnit konfiguraci, která je specifická pro konkrétní datové úložiště.</span><span class="sxs-lookup"><span data-stu-id="becf3-106">Providers may also enable configuration that is specific to a particular data store.</span></span> <span data-ttu-id="becf3-107">Dokumentace ke službě na konkrétní konfiguraci poskytovatele najdete v článku [poskytovatelé databází](../providers/index.md) oddílu.</span><span class="sxs-lookup"><span data-stu-id="becf3-107">For documentation on provider specific configuration see the [Database Providers](../providers/index.md) section.</span></span>

> [!TIP]  
> <span data-ttu-id="becf3-108">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="becf3-108">You can view this article’s [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) on GitHub.</span></span>

## <a name="use-fluent-api-to-configure-a-model"></a><span data-ttu-id="becf3-109">Použití rozhraní fluent API pro konfiguraci modelu</span><span class="sxs-lookup"><span data-stu-id="becf3-109">Use fluent API to configure a model</span></span>

<span data-ttu-id="becf3-110">Je možné přepsat `OnModelCreating` metoda v odvozené kontextu a použití `ModelBuilder API` ke konfiguraci modelu.</span><span class="sxs-lookup"><span data-stu-id="becf3-110">You can override the `OnModelCreating` method in your derived context and use the `ModelBuilder API` to configure your model.</span></span> <span data-ttu-id="becf3-111">Toto je nejvýkonnější metody konfigurace a umožňuje konfiguraci zadat bez změny vašich tříd entit.</span><span class="sxs-lookup"><span data-stu-id="becf3-111">This is the most powerful method of configuration and allows configuration to be specified without modifying your entity classes.</span></span> <span data-ttu-id="becf3-112">Konfigurace Fluent API má nejvyšší prioritu a přepíše poznámky konvence a data.</span><span class="sxs-lookup"><span data-stu-id="becf3-112">Fluent API configuration has the highest precedence and will override conventions and data annotations.</span></span>

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

## <a name="use-data-annotations-to-configure-a-model"></a><span data-ttu-id="becf3-113">Použití anotací dat při konfiguraci modelu</span><span class="sxs-lookup"><span data-stu-id="becf3-113">Use data annotations to configure a model</span></span>

<span data-ttu-id="becf3-114">Můžete také použít atributy (označuje se jako datové poznámky) do vaší třídy a vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="becf3-114">You can also apply attributes (known as Data Annotations) to your classes and properties.</span></span> <span data-ttu-id="becf3-115">Datové poznámky přepíše konvence, ale rozhraní Fluent API konfigurace se přepíše.</span><span class="sxs-lookup"><span data-stu-id="becf3-115">Data annotations will override conventions, but will be overwritten by Fluent API configuration.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?range=11-16&highlight=4)] -->
``` csharp
    public class Blog
    {
        public int BlogId { get; set; }
        [Required]
        public string Url { get; set; }
    }
```
