---
title: Vytvoření a konfigurace modelu – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: 5b886226b16b5b1a1f01e6040e58d92ae8678d29
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197304"
---
# <a name="creating-and-configuring-a-model"></a><span data-ttu-id="b7ea5-102">Vytvoření a konfigurace modelu</span><span class="sxs-lookup"><span data-stu-id="b7ea5-102">Creating and configuring a model</span></span>

<span data-ttu-id="b7ea5-103">Entity Framework používá sadu konvencí pro sestavování modelu na základě tvaru tříd entit.</span><span class="sxs-lookup"><span data-stu-id="b7ea5-103">Entity Framework uses a set of conventions to build a model based on the shape of your entity classes.</span></span> <span data-ttu-id="b7ea5-104">Můžete zadat další konfiguraci pro doplnění nebo přepsání toho, co bylo zjištěno podle konvence.</span><span class="sxs-lookup"><span data-stu-id="b7ea5-104">You can specify additional configuration to supplement and/or override what was discovered by convention.</span></span>

<span data-ttu-id="b7ea5-105">Tento článek se zabývá konfigurací, kterou je možné použít pro model, který cílí na jakékoli úložiště dat a který je možné použít při cílení na jakoukoli relační databázi.</span><span class="sxs-lookup"><span data-stu-id="b7ea5-105">This article covers configuration that can be applied to a model targeting any data store and that which can be applied when targeting any relational database.</span></span> <span data-ttu-id="b7ea5-106">Poskytovatelé taky můžou povolit konfiguraci, která je specifická pro konkrétní úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="b7ea5-106">Providers may also enable configuration that is specific to a particular data store.</span></span> <span data-ttu-id="b7ea5-107">Dokumentaci ke konfiguraci specifickou pro poskytovatele najdete v části [poskytovatelé](../providers/index.md) databází.</span><span class="sxs-lookup"><span data-stu-id="b7ea5-107">For documentation on provider specific configuration see the [Database Providers](../providers/index.md) section.</span></span>

> [!TIP]  
> <span data-ttu-id="b7ea5-108"> [](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples)Ukázku tohoto článku můžete zobrazit na GitHubu.</span><span class="sxs-lookup"><span data-stu-id="b7ea5-108">You can view this article’s [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) on GitHub.</span></span>

## <a name="use-fluent-api-to-configure-a-model"></a><span data-ttu-id="b7ea5-109">Použití rozhraní Fluent API ke konfiguraci modelu</span><span class="sxs-lookup"><span data-stu-id="b7ea5-109">Use fluent API to configure a model</span></span>

<span data-ttu-id="b7ea5-110">Můžete přepsat `OnModelCreating` metoduv `ModelBuilder API` odvozeném kontextu a použít ke konfiguraci modelu.</span><span class="sxs-lookup"><span data-stu-id="b7ea5-110">You can override the `OnModelCreating` method in your derived context and use the `ModelBuilder API` to configure your model.</span></span> <span data-ttu-id="b7ea5-111">Jedná se o nejúčinnější způsob konfigurace a umožňuje zadat konfiguraci bez úprav tříd entit.</span><span class="sxs-lookup"><span data-stu-id="b7ea5-111">This is the most powerful method of configuration and allows configuration to be specified without modifying your entity classes.</span></span> <span data-ttu-id="b7ea5-112">Konfigurace rozhraní Fluent API má nejvyšší prioritu a přepíše konvence a datové poznámky.</span><span class="sxs-lookup"><span data-stu-id="b7ea5-112">Fluent API configuration has the highest precedence and will override conventions and data annotations.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=11-13)]

## <a name="use-data-annotations-to-configure-a-model"></a><span data-ttu-id="b7ea5-113">Použití datových poznámek ke konfiguraci modelu</span><span class="sxs-lookup"><span data-stu-id="b7ea5-113">Use data annotations to configure a model</span></span>

<span data-ttu-id="b7ea5-114">Můžete také použít atributy (známé jako datové poznámky) k vašim třídám a vlastnostem.</span><span class="sxs-lookup"><span data-stu-id="b7ea5-114">You can also apply attributes (known as Data Annotations) to your classes and properties.</span></span> <span data-ttu-id="b7ea5-115">Datové poznámky přepíšou konvence, ale budou přepsány konfigurací Fluent rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b7ea5-115">Data annotations will override conventions, but will be overridden by Fluent API configuration.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=14)]
