---
title: Zahrnutí & s výjimkou vlastností – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/included-properties
ms.openlocfilehash: cd111af891ef0bbaccf515eed0c1991f105bd362
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197423"
---
# <a name="including--excluding-properties"></a><span data-ttu-id="da8a7-102">Zahrnutí a vyloučení vlastností</span><span class="sxs-lookup"><span data-stu-id="da8a7-102">Including & Excluding Properties</span></span>

<span data-ttu-id="da8a7-103">Zahrnutím vlastnosti v modelu znamená, že EF má metadata o této vlastnosti a pokusí se o čtení a zápis hodnot z databáze.</span><span class="sxs-lookup"><span data-stu-id="da8a7-103">Including a property in the model means that EF has metadata about that property and will attempt to read and write values from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="da8a7-104">Konvence</span><span class="sxs-lookup"><span data-stu-id="da8a7-104">Conventions</span></span>

<span data-ttu-id="da8a7-105">Podle konvence se do modelu zahrne veřejné vlastnosti s mechanismem getter a setter.</span><span class="sxs-lookup"><span data-stu-id="da8a7-105">By convention, public properties with a getter and a setter will be included in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="da8a7-106">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="da8a7-106">Data Annotations</span></span>

<span data-ttu-id="da8a7-107">K vyloučení vlastnosti z modelu můžete použít datové poznámky.</span><span class="sxs-lookup"><span data-stu-id="da8a7-107">You can use Data Annotations to exclude a property from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreProperty.cs?highlight=17)]

## <a name="fluent-api"></a><span data-ttu-id="da8a7-108">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="da8a7-108">Fluent API</span></span>

<span data-ttu-id="da8a7-109">Rozhraní Fluent API můžete použít k vyloučení vlastnosti z modelu.</span><span class="sxs-lookup"><span data-stu-id="da8a7-109">You can use the Fluent API to exclude a property from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreProperty.cs?highlight=12,13)]
