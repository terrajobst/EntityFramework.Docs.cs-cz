---
title: Maximální délka – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
uid: core/modeling/max-length
ms.openlocfilehash: 3220518cb0a409b6e802d2f3a98acdb949ffbf56
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929846"
---
# <a name="maximum-length"></a><span data-ttu-id="e5626-102">Maximální délka</span><span class="sxs-lookup"><span data-stu-id="e5626-102">Maximum Length</span></span>

<span data-ttu-id="e5626-103">Konfigurace maximální délku poskytuje nápovědu pro úložiště dat o správný typ dat chcete použít pro danou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="e5626-103">Configuring a maximum length provides a hint to the data store about the appropriate data type to use for a given property.</span></span> <span data-ttu-id="e5626-104">Maximální délka platí jenom pro pole datové typy, jako například `string` a `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="e5626-104">Maximum length only applies to array data types, such as `string` and `byte[]`.</span></span>

> [!NOTE]  
> <span data-ttu-id="e5626-105">Entity Framework neprovádí žádné ověření s maximální délkou před předáním dat do zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="e5626-105">Entity Framework does not do any validation of maximum length before passing data to the provider.</span></span> <span data-ttu-id="e5626-106">Záleží poskytovatele nebo úložišti dat pro ověření v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="e5626-106">It is up to the provider or data store to validate if appropriate.</span></span> <span data-ttu-id="e5626-107">Například při cílení na SQL serveru, překračuje maximální délku způsobí výjimku jako datový typ základního sloupce neumožní nadbytečná data k uložení.</span><span class="sxs-lookup"><span data-stu-id="e5626-107">For example, when targeting SQL Server, exceeding the maximum length will result in an exception as the data type of the underlying column will not allow excess data to be stored.</span></span>

## <a name="conventions"></a><span data-ttu-id="e5626-108">Konvence</span><span class="sxs-lookup"><span data-stu-id="e5626-108">Conventions</span></span>

<span data-ttu-id="e5626-109">Podle konvence je ponecháno až poskytovatel databáze vybrat příslušný datový typ pro vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="e5626-109">By convention, it is left up to the database provider to choose an appropriate data type for properties.</span></span> <span data-ttu-id="e5626-110">Pro vlastnosti, které mají délku poskytovatel databáze obecně zvolte datový typ, který umožňuje nejdelší dobu data.</span><span class="sxs-lookup"><span data-stu-id="e5626-110">For properties that have a length, the database provider will generally choose a data type that allows for the longest length of data.</span></span> <span data-ttu-id="e5626-111">Například se bude používat Microsoft SQL Server `nvarchar(max)` pro `string` vlastnosti (nebo `nvarchar(450)` Pokud sloupec se používá jako klíč).</span><span class="sxs-lookup"><span data-stu-id="e5626-111">For example, Microsoft SQL Server will use `nvarchar(max)` for `string` properties (or `nvarchar(450)` if the column is used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="e5626-112">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="e5626-112">Data Annotations</span></span>

<span data-ttu-id="e5626-113">Anotacemi dat můžete použít ke konfiguraci maximální délka pro vlastnost.</span><span class="sxs-lookup"><span data-stu-id="e5626-113">You can use the Data Annotations to configure a maximum length for a property.</span></span> <span data-ttu-id="e5626-114">V tomto příkladu cílení na systém SQL Server, výsledkem by `nvarchar(500)` datový typ používá.</span><span class="sxs-lookup"><span data-stu-id="e5626-114">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/MaxLength.cs?highlight=14)]

## <a name="fluent-api"></a><span data-ttu-id="e5626-115">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="e5626-115">Fluent API</span></span>

<span data-ttu-id="e5626-116">Rozhraní Fluent API můžete použít ke konfiguraci maximální délka pro vlastnost.</span><span class="sxs-lookup"><span data-stu-id="e5626-116">You can use the Fluent API to configure a maximum length for a property.</span></span> <span data-ttu-id="e5626-117">V tomto příkladu cílení na systém SQL Server, výsledkem by `nvarchar(500)` datový typ používá.</span><span class="sxs-lookup"><span data-stu-id="e5626-117">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/MaxLength.cs?highlight=11-13)]
