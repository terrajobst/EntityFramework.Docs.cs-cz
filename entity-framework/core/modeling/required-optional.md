---
title: Požadované/volitelné vlastnosti – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: 7200cd2eeeba2f22365ef09b1f50edd077240130
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149153"
---
# <a name="required-and-optional-properties"></a><span data-ttu-id="4c04f-102">Povinné a volitelné vlastnosti</span><span class="sxs-lookup"><span data-stu-id="4c04f-102">Required and Optional Properties</span></span>

<span data-ttu-id="4c04f-103">Vlastnost je považována za volitelnou, pokud je platná pro její `null`zahrnutí.</span><span class="sxs-lookup"><span data-stu-id="4c04f-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="4c04f-104">Pokud `null` není platná hodnota, která má být přiřazena vlastnosti, je považována za požadovanou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="4c04f-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

## <a name="conventions"></a><span data-ttu-id="4c04f-105">Konvence</span><span class="sxs-lookup"><span data-stu-id="4c04f-105">Conventions</span></span>

<span data-ttu-id="4c04f-106">Podle konvence vlastnost, jejíž typ .NET může obsahovat hodnotu null, bude nakonfigurována jako volitelná `int?`( `byte[]``string`,, atd.).</span><span class="sxs-lookup"><span data-stu-id="4c04f-106">By convention, a property whose .NET type can contain null will be configured as optional (`string`, `int?`, `byte[]`, etc.).</span></span> <span data-ttu-id="4c04f-107">Vlastnosti, jejichž typ CLR nemůže obsahovat hodnotu null, budou nakonfigurovány`int`jako `decimal`povinné `bool`(,, atd.).</span><span class="sxs-lookup"><span data-stu-id="4c04f-107">Properties whose CLR type cannot contain null will be configured as required (`int`, `decimal`, `bool`, etc.).</span></span>

> [!NOTE]  
> <span data-ttu-id="4c04f-108">Vlastnost, jejíž typ .NET nemůže obsahovat hodnotu null, nelze nakonfigurovat jako volitelnou.</span><span class="sxs-lookup"><span data-stu-id="4c04f-108">A property whose .NET type cannot contain null cannot be configured as optional.</span></span> <span data-ttu-id="4c04f-109">Vlastnost bude vždy posouzena jako požadovaná Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="4c04f-109">The property will always be considered required by Entity Framework.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="4c04f-110">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="4c04f-110">Data Annotations</span></span>

<span data-ttu-id="4c04f-111">Můžete použít datové poznámky k indikaci, že vlastnost je povinná.</span><span class="sxs-lookup"><span data-stu-id="4c04f-111">You can use Data Annotations to indicate that a property is required.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=14)]

## <a name="fluent-api"></a><span data-ttu-id="4c04f-112">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="4c04f-112">Fluent API</span></span>

<span data-ttu-id="4c04f-113">Rozhraní Fluent API můžete použít k indikaci, že vlastnost je povinná.</span><span class="sxs-lookup"><span data-stu-id="4c04f-113">You can use the Fluent API to indicate that a property is required.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=11-13)]

