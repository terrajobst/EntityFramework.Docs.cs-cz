---
title: Střídání mezi několika modely se stejným typem DbContext-EF Core
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/dynamic-model
ms.openlocfilehash: 156d5666cbd9352b274ddc70c99704ca62aeb1fd
ms.sourcegitcommit: 4e86f01740e407ff25e704a11b1f7d7e66bfb2a6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/09/2020
ms.locfileid: "75781128"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a><span data-ttu-id="71815-102">Střídání mezi několika modely se stejným typem DbContext</span><span class="sxs-lookup"><span data-stu-id="71815-102">Alternating between multiple models with the same DbContext type</span></span>

<span data-ttu-id="71815-103">Model sestavený v `OnModelCreating` může použít vlastnost v kontextu ke změně způsobu sestavení modelu.</span><span class="sxs-lookup"><span data-stu-id="71815-103">The model built in `OnModelCreating` can use a property on the context to change how the model is built.</span></span> <span data-ttu-id="71815-104">Předpokládejme například, že jste chtěli konfigurovat entitu odlišně v závislosti na některé vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="71815-104">For example, suppose you wanted to configure an entity differently based on some property:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnModelCreating)]

<span data-ttu-id="71815-105">Tento kód bohužel nefunguje tak, jak je, protože EF sestaví model a spustí `OnModelCreating` pouze jednou a ukládá do mezipaměti výsledek z důvodů výkonu.</span><span class="sxs-lookup"><span data-stu-id="71815-105">Unfortunately, this code wouldn't work as-is, since EF builds the model and runs `OnModelCreating` only once, caching the result for performance reasons.</span></span> <span data-ttu-id="71815-106">Je však možné připojit se k mechanismu ukládání modelu do mezipaměti, aby měl EF vědět, že vlastnost vytvářející různé modely vytváří různé modely.</span><span class="sxs-lookup"><span data-stu-id="71815-106">However, you can hook into the model caching mechanism to make EF aware of the property producing different models.</span></span>

## <a name="imodelcachekeyfactory"></a><span data-ttu-id="71815-107">IModelCacheKeyFactory</span><span class="sxs-lookup"><span data-stu-id="71815-107">IModelCacheKeyFactory</span></span>

<span data-ttu-id="71815-108">EF používá `IModelCacheKeyFactory` k vygenerování klíčů mezipaměti pro modely; ve výchozím nastavení předpokládá, že pro každý daný typ kontextu bude model stejný, takže výchozí implementace této služby vrátí klíč, který pouze obsahuje typ kontextu.</span><span class="sxs-lookup"><span data-stu-id="71815-108">EF uses the `IModelCacheKeyFactory` to generate cache keys for models; by default, EF assumes that for any given context type the model will be the same, so the default implementation of this service returns a key that just contains the context type.</span></span> <span data-ttu-id="71815-109">Chcete-li vydávat různé modely ze stejného typu kontextu, je nutné nahradit službu `IModelCacheKeyFactory` správnou implementací. vygenerovaný klíč bude porovnán s ostatními klíči modelů pomocí metody `Equals`, přičemž vezme v úvahu všechny proměnné ovlivňující model:</span><span class="sxs-lookup"><span data-stu-id="71815-109">To produce different models from the same context type, you need to replace the `IModelCacheKeyFactory` service with the correct  implementation; the generated key will be compared to other model keys using the `Equals` method, taking into account all the variables that affect the model:</span></span>

<span data-ttu-id="71815-110">Následující implementace vezme při vytváření klíče mezipaměti modelu `IgnoreIntProperty` v úvahu:</span><span class="sxs-lookup"><span data-stu-id="71815-110">The following implementation takes the `IgnoreIntProperty` into account when producing a model cache key:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicModelCacheKeyFactory.cs?name=DynamicModel)]

<span data-ttu-id="71815-111">Nakonec Zaregistrujte novou `IModelCacheKeyFactory` do `OnConfiguring`vašeho kontextu:</span><span class="sxs-lookup"><span data-stu-id="71815-111">Finally, register your new `IModelCacheKeyFactory` in your context's `OnConfiguring`:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnConfiguring)]

<span data-ttu-id="71815-112">Podívejte se na [úplný ukázkový projekt](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DynamicModel) pro další kontext.</span><span class="sxs-lookup"><span data-stu-id="71815-112">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DynamicModel) for more context.</span></span>
