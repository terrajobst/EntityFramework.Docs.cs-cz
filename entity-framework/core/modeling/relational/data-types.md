---
title: Datové typy – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
uid: core/modeling/relational/data-types
ms.openlocfilehash: 26664ebe18abcdeaa2b9c8dc23a6410204f53c8e
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197179"
---
# <a name="data-types"></a><span data-ttu-id="a6e85-102">Datové typy</span><span class="sxs-lookup"><span data-stu-id="a6e85-102">Data Types</span></span>

> [!NOTE]  
> <span data-ttu-id="a6e85-103">Konfigurace v této části platí pro relační databáze obecně.</span><span class="sxs-lookup"><span data-stu-id="a6e85-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="a6e85-104">Metody rozšíření zobrazené tady budou k dispozici při instalaci zprostředkovatele relační databáze (z důvodu sdíleného balíčku *Microsoft. EntityFrameworkCore. relační* ).</span><span class="sxs-lookup"><span data-stu-id="a6e85-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="a6e85-105">Datový typ odkazuje na konkrétní typ sloupce, na který je vlastnost mapována.</span><span class="sxs-lookup"><span data-stu-id="a6e85-105">Data type refers to the database specific type of the column to which a property is mapped.</span></span>

## <a name="conventions"></a><span data-ttu-id="a6e85-106">Konvence</span><span class="sxs-lookup"><span data-stu-id="a6e85-106">Conventions</span></span>

<span data-ttu-id="a6e85-107">Podle konvence poskytovatel databáze vybere datový typ založený na typu rozhraní .NET vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="a6e85-107">By convention, the database provider selects a data type based on the .NET type of the property.</span></span> <span data-ttu-id="a6e85-108">Také vezme v úvahu jiná metadata, jako je například nakonfigurovaná [Maximální délka](../max-length.md), zda je vlastnost částí primárního klíče atd.</span><span class="sxs-lookup"><span data-stu-id="a6e85-108">It also takes into account other metadata, such as the configured [Maximum Length](../max-length.md), whether the property is part of a primary key, etc.</span></span>

<span data-ttu-id="a6e85-109">`datetime2(7)` Například SQL Server používá pro `DateTime` vlastnosti a `nvarchar(max)` pro `string` vlastnosti (nebo `nvarchar(450)` pro `string` vlastnosti, které se používají jako klíč).</span><span class="sxs-lookup"><span data-stu-id="a6e85-109">For example, SQL Server uses `datetime2(7)` for `DateTime` properties, and `nvarchar(max)` for `string` properties (or `nvarchar(450)` for `string` properties that are used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="a6e85-110">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="a6e85-110">Data Annotations</span></span>

<span data-ttu-id="a6e85-111">K určení přesného datového typu pro sloupec můžete použít datové poznámky.</span><span class="sxs-lookup"><span data-stu-id="a6e85-111">You can use Data Annotations to specify an exact data type for a column.</span></span>

<span data-ttu-id="a6e85-112">Například následující kód se nakonfiguruje `Url` jako řetězec jiný než Unicode s maximální `200` délkou a `Rating` `2`jako desítkový s přesností `5` a měřítkem.</span><span class="sxs-lookup"><span data-stu-id="a6e85-112">For example the following code configures `Url` as a non-unicode string with maximum length of `200` and `Rating` as decimal with precision of `5` and scale of `2`.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a><span data-ttu-id="a6e85-113">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="a6e85-113">Fluent API</span></span>

<span data-ttu-id="a6e85-114">Rozhraní Fluent API můžete použít také k určení stejných datových typů pro sloupce.</span><span class="sxs-lookup"><span data-stu-id="a6e85-114">You can also use the Fluent API to specify the same data types for the columns.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DataType.cs?name=Model&highlight=9-10)]
