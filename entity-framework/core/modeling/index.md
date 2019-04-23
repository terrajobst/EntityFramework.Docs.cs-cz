---
title: Vytvoření modelu – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: 78a8ffd2393a914edf737104f14e41f8a9074ad5
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929895"
---
# <a name="creating-and-configuring-a-model"></a><span data-ttu-id="675f9-102">Vytváření a konfiguraci modelu</span><span class="sxs-lookup"><span data-stu-id="675f9-102">Creating and configuring a Model</span></span>

<span data-ttu-id="675f9-103">Entity Framework používá sadu konvence sestavit model založený na obrazec tříd entit.</span><span class="sxs-lookup"><span data-stu-id="675f9-103">Entity Framework uses a set of conventions to build a model based on the shape of your entity classes.</span></span> <span data-ttu-id="675f9-104">Můžete zadat další konfiguraci k doplnění a/nebo přepsat, co bylo zjištěno konvencí.</span><span class="sxs-lookup"><span data-stu-id="675f9-104">You can specify additional configuration to supplement and/or override what was discovered by convention.</span></span>

<span data-ttu-id="675f9-105">Tento článek se týká konfigurace, který lze použít k modelu, který cílí na jakékoli úložiště dat a který, který je možné použít při cílení na jakoukoli relační databázi.</span><span class="sxs-lookup"><span data-stu-id="675f9-105">This article covers configuration that can be applied to a model targeting any data store and that which can be applied when targeting any relational database.</span></span> <span data-ttu-id="675f9-106">Poskytovatelé může také umožnit konfiguraci, která je specifická pro konkrétní datové úložiště.</span><span class="sxs-lookup"><span data-stu-id="675f9-106">Providers may also enable configuration that is specific to a particular data store.</span></span> <span data-ttu-id="675f9-107">Dokumentace ke službě na konkrétní konfiguraci poskytovatele najdete v článku [poskytovatelé databází](../providers/index.md) oddílu.</span><span class="sxs-lookup"><span data-stu-id="675f9-107">For documentation on provider specific configuration see the [Database Providers](../providers/index.md) section.</span></span>

> [!TIP]  
> <span data-ttu-id="675f9-108">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="675f9-108">You can view this article’s [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) on GitHub.</span></span>

## <a name="use-fluent-api-to-configure-a-model"></a><span data-ttu-id="675f9-109">Použití rozhraní fluent API pro konfiguraci modelu</span><span class="sxs-lookup"><span data-stu-id="675f9-109">Use fluent API to configure a model</span></span>

<span data-ttu-id="675f9-110">Je možné přepsat `OnModelCreating` metoda v odvozené kontextu a použití `ModelBuilder API` ke konfiguraci modelu.</span><span class="sxs-lookup"><span data-stu-id="675f9-110">You can override the `OnModelCreating` method in your derived context and use the `ModelBuilder API` to configure your model.</span></span> <span data-ttu-id="675f9-111">Toto je nejvýkonnější metody konfigurace a umožňuje konfiguraci zadat bez změny vašich tříd entit.</span><span class="sxs-lookup"><span data-stu-id="675f9-111">This is the most powerful method of configuration and allows configuration to be specified without modifying your entity classes.</span></span> <span data-ttu-id="675f9-112">Konfigurace Fluent API má nejvyšší prioritu a přepíše poznámky konvence a data.</span><span class="sxs-lookup"><span data-stu-id="675f9-112">Fluent API configuration has the highest precedence and will override conventions and data annotations.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=11-13)]

## <a name="use-data-annotations-to-configure-a-model"></a><span data-ttu-id="675f9-113">Použití anotací dat při konfiguraci modelu</span><span class="sxs-lookup"><span data-stu-id="675f9-113">Use data annotations to configure a model</span></span>

<span data-ttu-id="675f9-114">Můžete také použít atributy (označuje se jako datové poznámky) do vaší třídy a vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="675f9-114">You can also apply attributes (known as Data Annotations) to your classes and properties.</span></span> <span data-ttu-id="675f9-115">Datové poznámky přepíše konvence, ale rozhraní Fluent API konfigurace se přepíše.</span><span class="sxs-lookup"><span data-stu-id="675f9-115">Data annotations will override conventions, but will be overridden by Fluent API configuration.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=14)]
