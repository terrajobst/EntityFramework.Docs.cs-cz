---
title: Zahrnutí a vyloučení vlastností – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/included-properties
ms.openlocfilehash: 022534091bb48e491c8808791a401216a339d7b0
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929820"
---
# <a name="including--excluding-properties"></a><span data-ttu-id="1efdd-102">Zahrnutí a vyloučení vlastností</span><span class="sxs-lookup"><span data-stu-id="1efdd-102">Including & Excluding Properties</span></span>

<span data-ttu-id="1efdd-103">Včetně vlastnost v modelu znamená, že EF má metadata o tuto vlastnost a pokusí se čtení a zápis dat z/do databáze.</span><span class="sxs-lookup"><span data-stu-id="1efdd-103">Including a property in the model means that EF has metadata about that property and will attempt to read and write values from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="1efdd-104">Konvence</span><span class="sxs-lookup"><span data-stu-id="1efdd-104">Conventions</span></span>

<span data-ttu-id="1efdd-105">Podle konvence se zahrnou veřejné vlastnosti metody getter a setter v modelu.</span><span class="sxs-lookup"><span data-stu-id="1efdd-105">By convention, public properties with a getter and a setter will be included in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="1efdd-106">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="1efdd-106">Data Annotations</span></span>

<span data-ttu-id="1efdd-107">Datové poznámky můžete být z modelu vyloučena určitá vlastnost.</span><span class="sxs-lookup"><span data-stu-id="1efdd-107">You can use Data Annotations to exclude a property from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/IgnoreProperty.cs?highlight=17)]

## <a name="fluent-api"></a><span data-ttu-id="1efdd-108">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="1efdd-108">Fluent API</span></span>

<span data-ttu-id="1efdd-109">Rozhraní Fluent API můžete být z modelu vyloučena určitá vlastnost.</span><span class="sxs-lookup"><span data-stu-id="1efdd-109">You can use the Fluent API to exclude a property from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/IgnoreProperty.cs?highlight=12,13)]
