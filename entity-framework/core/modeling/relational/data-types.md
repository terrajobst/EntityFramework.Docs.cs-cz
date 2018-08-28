---
title: Datové typy – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
uid: core/modeling/relational/data-types
ms.openlocfilehash: 9060f66c752be01090ce40be6bf3a32f348ce571
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993518"
---
# <a name="data-types"></a><span data-ttu-id="12a5c-102">Datové typy</span><span class="sxs-lookup"><span data-stu-id="12a5c-102">Data Types</span></span>

> [!NOTE]  
> <span data-ttu-id="12a5c-103">Obecně se vztahuje k relačním databázím konfigurace v této části.</span><span class="sxs-lookup"><span data-stu-id="12a5c-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="12a5c-104">Metody rozšíření je vidět tady bude k dispozici při instalaci poskytovatele relační databáze (z důvodu sdílený *Microsoft.EntityFrameworkCore.Relational* balíček).</span><span class="sxs-lookup"><span data-stu-id="12a5c-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="12a5c-105">Datový typ odkazuje na konkrétní typ databáze sloupce, ke kterému je namapována vlastnost.</span><span class="sxs-lookup"><span data-stu-id="12a5c-105">Data type refers to the database specific type of the column to which a property is mapped.</span></span>

## <a name="conventions"></a><span data-ttu-id="12a5c-106">Konvence</span><span class="sxs-lookup"><span data-stu-id="12a5c-106">Conventions</span></span>

<span data-ttu-id="12a5c-107">Podle konvence vybere poskytovatele databáze datový typ na základě typu CLR vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="12a5c-107">By convention, the database provider selects a data type based on the CLR type of the property.</span></span> <span data-ttu-id="12a5c-108">Také bere v úvahu další metadata, například nakonfigurovaným [maximální délku](../max-length.md), zda vlastnost je částí primárního klíče, atd.</span><span class="sxs-lookup"><span data-stu-id="12a5c-108">It also takes into account other metadata, such as the configured [Maximum Length](../max-length.md), whether the property is part of a primary key, etc.</span></span>

<span data-ttu-id="12a5c-109">Například SQL Server používá `datetime2(7)` pro `DateTime` vlastnosti, a `nvarchar(max)` pro `string` vlastnosti (nebo `nvarchar(450)` pro `string` vlastnosti, které slouží jako klíč).</span><span class="sxs-lookup"><span data-stu-id="12a5c-109">For example, SQL Server uses `datetime2(7)` for `DateTime` properties, and `nvarchar(max)` for `string` properties (or `nvarchar(450)` for `string` properties that are used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="12a5c-110">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="12a5c-110">Data Annotations</span></span>

<span data-ttu-id="12a5c-111">Datové poznámky můžete zadat přesný datový typ pro sloupec.</span><span class="sxs-lookup"><span data-stu-id="12a5c-111">You can use Data Annotations to specify an exact data type for a column.</span></span>

<span data-ttu-id="12a5c-112">Například následující kód konfiguruje `Url` jako kódování unicode řetězec s maximální délkou `200` a `Rating` jako desetinné přesnosti `5` a škálovat z `2`.</span><span class="sxs-lookup"><span data-stu-id="12a5c-112">For example the following code configures `Url` as a non-unicode string with maximum length of `200` and `Rating` as decimal with precision of `5` and scale of `2`.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a><span data-ttu-id="12a5c-113">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="12a5c-113">Fluent API</span></span>

<span data-ttu-id="12a5c-114">Rozhraní Fluent API můžete použít také k určení stejné datové typy sloupců.</span><span class="sxs-lookup"><span data-stu-id="12a5c-114">You can also use the Fluent API to specify the same data types for the columns.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
