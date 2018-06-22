---
title: Datové typy - EF jádra
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
ms.technology: entity-framework-core
uid: core/modeling/relational/data-types
ms.openlocfilehash: fd4668a3f9554eb9d3b1161d5dddce2fcdcac712
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/05/2017
ms.locfileid: "26054544"
---
# <a name="data-types"></a><span data-ttu-id="03433-102">Datové typy</span><span class="sxs-lookup"><span data-stu-id="03433-102">Data Types</span></span>

> [!NOTE]  
> <span data-ttu-id="03433-103">Obecně se vztahuje na relační databáze konfigurace v této části.</span><span class="sxs-lookup"><span data-stu-id="03433-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="03433-104">Rozšiřující metody zobrazeny zde bude k dispozici při instalaci poskytovatele relační databáze (z důvodu sdílený *Microsoft.EntityFrameworkCore.Relational* balíčku).</span><span class="sxs-lookup"><span data-stu-id="03433-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="03433-105">Datový typ odkazuje na konkrétní typ databáze sloupce, na kterou vlastnost namapovaná.</span><span class="sxs-lookup"><span data-stu-id="03433-105">Data type refers to the database specific type of the column to which a property is mapped.</span></span>

## <a name="conventions"></a><span data-ttu-id="03433-106">Konvence</span><span class="sxs-lookup"><span data-stu-id="03433-106">Conventions</span></span>

<span data-ttu-id="03433-107">Dle konvencí se vybere zprostředkovatel databáze typu dat na základě typu CLR vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="03433-107">By convention, the database provider selects a data type based on the CLR type of the property.</span></span> <span data-ttu-id="03433-108">Také bere v úvahu další metadata, například nakonfigurované [maximální délku](../max-length.md), zda vlastnost je částí primární klíč atd.</span><span class="sxs-lookup"><span data-stu-id="03433-108">It also takes into account other metadata, such as the configured [Maximum Length](../max-length.md), whether the property is part of a primary key, etc.</span></span>

<span data-ttu-id="03433-109">Například používá systém SQL Server `datetime2(7)` pro `DateTime` vlastnosti, a `nvarchar(max)` pro `string` vlastnosti (nebo `nvarchar(450)` pro `string` vlastnosti, které se používají jako klíč).</span><span class="sxs-lookup"><span data-stu-id="03433-109">For example, SQL Server uses `datetime2(7)` for `DateTime` properties, and `nvarchar(max)` for `string` properties (or `nvarchar(450)` for `string` properties that are used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="03433-110">Datových poznámek</span><span class="sxs-lookup"><span data-stu-id="03433-110">Data Annotations</span></span>

<span data-ttu-id="03433-111">Můžete zadat přesný datový typ pro sloupec datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="03433-111">You can use Data Annotations to specify an exact data type for a column.</span></span>

<span data-ttu-id="03433-112">Například následující kód konfiguruje `Url` jako řetězec kódování unicode s maximální délkou `200` a `Rating` jako desetinné přesnosti `5` a škálovat z `2`.</span><span class="sxs-lookup"><span data-stu-id="03433-112">For example the following code configures `Url` as a non-unicode string with maximum length of `200` and `Rating` as decimal with precision of `5` and scale of `2`.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a><span data-ttu-id="03433-113">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="03433-113">Fluent API</span></span>

<span data-ttu-id="03433-114">Rozhraní Fluent API můžete použít také k určení stejné typy dat sloupců.</span><span class="sxs-lookup"><span data-stu-id="03433-114">You can also use the Fluent API to specify the same data types for the columns.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
