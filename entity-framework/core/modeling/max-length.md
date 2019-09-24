---
title: Maximální délka – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
uid: core/modeling/max-length
ms.openlocfilehash: b6f0594fed0c491b4f79dcda5273cdebe9ecf35f
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197222"
---
# <a name="maximum-length"></a><span data-ttu-id="17230-102">Maximální délka</span><span class="sxs-lookup"><span data-stu-id="17230-102">Maximum Length</span></span>

<span data-ttu-id="17230-103">Konfigurace maximální délky poskytuje nápovědu pro úložiště dat o vhodném datovém typu, který se má pro danou vlastnost použít.</span><span class="sxs-lookup"><span data-stu-id="17230-103">Configuring a maximum length provides a hint to the data store about the appropriate data type to use for a given property.</span></span> <span data-ttu-id="17230-104">Maximální délka se vztahuje pouze na datové typy pole, například `string` a `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="17230-104">Maximum length only applies to array data types, such as `string` and `byte[]`.</span></span>

> [!NOTE]  
> <span data-ttu-id="17230-105">Entity Framework neprovádí žádné ověření maximální délky před předáním dat poskytovateli.</span><span class="sxs-lookup"><span data-stu-id="17230-105">Entity Framework does not do any validation of maximum length before passing data to the provider.</span></span> <span data-ttu-id="17230-106">Pokud je to vhodné, je až do poskytovatele nebo úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="17230-106">It is up to the provider or data store to validate if appropriate.</span></span> <span data-ttu-id="17230-107">Například při cílení na SQL Server, což překračuje maximální délku, dojde k výjimce, protože datový typ podkladového sloupce nebude moci ukládat nadbytečné údaje.</span><span class="sxs-lookup"><span data-stu-id="17230-107">For example, when targeting SQL Server, exceeding the maximum length will result in an exception as the data type of the underlying column will not allow excess data to be stored.</span></span>

## <a name="conventions"></a><span data-ttu-id="17230-108">Konvence</span><span class="sxs-lookup"><span data-stu-id="17230-108">Conventions</span></span>

<span data-ttu-id="17230-109">Podle konvence je ponecháno až od poskytovatele databáze, aby bylo možné zvolit vhodný datový typ pro vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="17230-109">By convention, it is left up to the database provider to choose an appropriate data type for properties.</span></span> <span data-ttu-id="17230-110">U vlastností, které mají délku, bude poskytovatel databáze obecně vybírat datový typ, který umožňuje nejdelší délku dat.</span><span class="sxs-lookup"><span data-stu-id="17230-110">For properties that have a length, the database provider will generally choose a data type that allows for the longest length of data.</span></span> <span data-ttu-id="17230-111">Například Microsoft SQL Server budou použity `nvarchar(max)` pro `string` vlastnosti (nebo `nvarchar(450)` Pokud je sloupec použit jako klíč).</span><span class="sxs-lookup"><span data-stu-id="17230-111">For example, Microsoft SQL Server will use `nvarchar(max)` for `string` properties (or `nvarchar(450)` if the column is used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="17230-112">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="17230-112">Data Annotations</span></span>

<span data-ttu-id="17230-113">Pomocí datových poznámek můžete nakonfigurovat maximální délku vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="17230-113">You can use the Data Annotations to configure a maximum length for a property.</span></span> <span data-ttu-id="17230-114">V tomto příkladu, který cílí na SQL Server, by to `nvarchar(500)` vedlo k použití datového typu.</span><span class="sxs-lookup"><span data-stu-id="17230-114">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/MaxLength.cs?highlight=14)]

## <a name="fluent-api"></a><span data-ttu-id="17230-115">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="17230-115">Fluent API</span></span>

<span data-ttu-id="17230-116">K nakonfigurování maximální délky vlastnosti můžete použít rozhraní Fluent API.</span><span class="sxs-lookup"><span data-stu-id="17230-116">You can use the Fluent API to configure a maximum length for a property.</span></span> <span data-ttu-id="17230-117">V tomto příkladu, který cílí na SQL Server, by to `nvarchar(500)` vedlo k použití datového typu.</span><span class="sxs-lookup"><span data-stu-id="17230-117">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/MaxLength.cs?highlight=11-13)]
