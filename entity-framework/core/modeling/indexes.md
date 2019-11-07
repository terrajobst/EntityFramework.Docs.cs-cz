---
title: Indexy – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: d1b5cd6853cd24f7e1aa3aace2f0a3455c657cc1
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655702"
---
# <a name="indexes"></a><span data-ttu-id="08b2c-102">Indexy</span><span class="sxs-lookup"><span data-stu-id="08b2c-102">Indexes</span></span>

<span data-ttu-id="08b2c-103">Indexy představují společný pojem v mnoha úložištích dat.</span><span class="sxs-lookup"><span data-stu-id="08b2c-103">Indexes are a common concept across many data stores.</span></span> <span data-ttu-id="08b2c-104">I když se jejich implementace v úložišti dat může lišit, používají se k tomu, aby vyhledávání na základě sloupce (nebo sady sloupců) bylo efektivnější.</span><span class="sxs-lookup"><span data-stu-id="08b2c-104">While their implementation in the data store may vary, they are used to make lookups based on a column (or set of columns) more efficient.</span></span>

## <a name="conventions"></a><span data-ttu-id="08b2c-105">Konvence</span><span class="sxs-lookup"><span data-stu-id="08b2c-105">Conventions</span></span>

<span data-ttu-id="08b2c-106">Podle konvence se v každé vlastnosti (nebo sadě vlastností) vytvoří index, který se používá jako cizí klíč.</span><span class="sxs-lookup"><span data-stu-id="08b2c-106">By convention, an index is created in each property (or set of properties) that are used as a foreign key.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="08b2c-107">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="08b2c-107">Data Annotations</span></span>

<span data-ttu-id="08b2c-108">Indexy nelze vytvořit pomocí datových poznámek.</span><span class="sxs-lookup"><span data-stu-id="08b2c-108">Indexes can not be created using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="08b2c-109">Rozhraní Fluent API</span><span class="sxs-lookup"><span data-stu-id="08b2c-109">Fluent API</span></span>

<span data-ttu-id="08b2c-110">Rozhraní Fluent API můžete použít k určení indexu pro jednu vlastnost.</span><span class="sxs-lookup"><span data-stu-id="08b2c-110">You can use the Fluent API to specify an index on a single property.</span></span> <span data-ttu-id="08b2c-111">Ve výchozím nastavení indexy nejsou jedinečné.</span><span class="sxs-lookup"><span data-stu-id="08b2c-111">By default, indexes are non-unique.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Index.cs?name=Index&highlight=7,8)]

<span data-ttu-id="08b2c-112">Můžete také určit, že by měl být index jedinečný, což znamená, že žádné dvě entity nemohou mít stejné hodnoty pro danou vlastnost (y).</span><span class="sxs-lookup"><span data-stu-id="08b2c-112">You can also specify that an index should be unique, meaning that no two entities can have the same value(s) for the given property(s).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexUnique.cs?name=ModelBuilder&highlight=3)]

<span data-ttu-id="08b2c-113">Index můžete zadat také pro více než jeden sloupec.</span><span class="sxs-lookup"><span data-stu-id="08b2c-113">You can also specify an index over more than one column.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexComposite.cs?name=Composite&highlight=7,8)]

> [!TIP]  
> <span data-ttu-id="08b2c-114">Pro jednu jedinečnou sadu vlastností je k dispozici pouze jeden index.</span><span class="sxs-lookup"><span data-stu-id="08b2c-114">There is only one index per distinct set of properties.</span></span> <span data-ttu-id="08b2c-115">Pokud použijete rozhraní Fluent API ke konfiguraci indexu pro sadu vlastností, které už mají definovaný index, buď podle konvence, nebo z předchozí konfigurace, bude změna definice tohoto indexu.</span><span class="sxs-lookup"><span data-stu-id="08b2c-115">If you use the Fluent API to configure an index on a set of properties that already has an index defined, either by convention or previous configuration, then you will be changing the definition of that index.</span></span> <span data-ttu-id="08b2c-116">To je užitečné, pokud chcete dále konfigurovat index, který byl vytvořen pomocí konvence.</span><span class="sxs-lookup"><span data-stu-id="08b2c-116">This is useful if you want to further configure an index that was created by convention.</span></span>
