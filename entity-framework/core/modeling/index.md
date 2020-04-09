---
title: Vytvoření a konfigurace modelu - EF Core
author: rowanmiller
ms.date: 11/05/2019
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: 0f44d9684ca5c8435d83085f9038860309bd82a2
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416415"
---
# <a name="creating-and-configuring-a-model"></a><span data-ttu-id="9e19f-102">Vytvoření a konfigurace modelu</span><span class="sxs-lookup"><span data-stu-id="9e19f-102">Creating and configuring a model</span></span>

<span data-ttu-id="9e19f-103">Entity Framework používá sadu konvencí k vytvoření modelu na základě tvaru tříd entity.</span><span class="sxs-lookup"><span data-stu-id="9e19f-103">Entity Framework uses a set of conventions to build a model based on the shape of your entity classes.</span></span> <span data-ttu-id="9e19f-104">Můžete zadat další konfiguraci k doplnění nebo přepsání toho, co bylo zjištěno konvencí.</span><span class="sxs-lookup"><span data-stu-id="9e19f-104">You can specify additional configuration to supplement and/or override what was discovered by convention.</span></span>

<span data-ttu-id="9e19f-105">Tento článek popisuje konfiguraci, kterou lze použít u modelu, který cílí na jakékoli úložiště dat, a na konfiguraci, kterou lze použít při cílení na libovolnou relační databázi.</span><span class="sxs-lookup"><span data-stu-id="9e19f-105">This article covers configuration that can be applied to a model targeting any data store and that which can be applied when targeting any relational database.</span></span> <span data-ttu-id="9e19f-106">Zprostředkovatelé mohou také povolit konfiguraci, která je specifická pro konkrétní úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="9e19f-106">Providers may also enable configuration that is specific to a particular data store.</span></span> <span data-ttu-id="9e19f-107">Dokumentace ke konfiguraci specifické pro zprostředkovatele naleznete v části [Zprostředkovatelé](../providers/index.md) databáze.</span><span class="sxs-lookup"><span data-stu-id="9e19f-107">For documentation on provider specific configuration see the [Database Providers](../providers/index.md) section.</span></span>

> [!TIP]  
> <span data-ttu-id="9e19f-108">Ukázku tohoto článku [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples) můžete zobrazit na GitHubu.</span><span class="sxs-lookup"><span data-stu-id="9e19f-108">You can view this article’s [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples) on GitHub.</span></span>

## <a name="use-fluent-api-to-configure-a-model"></a><span data-ttu-id="9e19f-109">Konfigurace modelu pomocí plynulého rozhraní API</span><span class="sxs-lookup"><span data-stu-id="9e19f-109">Use fluent API to configure a model</span></span>

<span data-ttu-id="9e19f-110">Můžete přepsat metodu `OnModelCreating` v odvozeném kontextu `ModelBuilder API` a použít ke konfiguraci modelu.</span><span class="sxs-lookup"><span data-stu-id="9e19f-110">You can override the `OnModelCreating` method in your derived context and use the `ModelBuilder API` to configure your model.</span></span> <span data-ttu-id="9e19f-111">Toto je nejvýkonnější způsob konfigurace a umožňuje konfiguraci, která má být zadána bez úprav tříd entit.</span><span class="sxs-lookup"><span data-stu-id="9e19f-111">This is the most powerful method of configuration and allows configuration to be specified without modifying your entity classes.</span></span> <span data-ttu-id="9e19f-112">Konfigurace rozhraní Fluent API má nejvyšší prioritu a přepíše konvence a poznámky k datům.</span><span class="sxs-lookup"><span data-stu-id="9e19f-112">Fluent API configuration has the highest precedence and will override conventions and data annotations.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=12-14)]

## <a name="use-data-annotations-to-configure-a-model"></a><span data-ttu-id="9e19f-113">Konfigurace modelu pomocí datových anotací</span><span class="sxs-lookup"><span data-stu-id="9e19f-113">Use data annotations to configure a model</span></span>

<span data-ttu-id="9e19f-114">Můžete také použít atributy (označované jako datové poznámky) na vaše třídy a vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="9e19f-114">You can also apply attributes (known as Data Annotations) to your classes and properties.</span></span> <span data-ttu-id="9e19f-115">Anotace dat budou přepsat konvence, ale budou přepsány konfigurací rozhraní Fluent API.</span><span class="sxs-lookup"><span data-stu-id="9e19f-115">Data annotations will override conventions, but will be overridden by Fluent API configuration.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=15)]
