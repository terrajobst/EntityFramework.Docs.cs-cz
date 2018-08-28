---
title: Sledování bez dotazy - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: f80ac260-c2dc-484d-94a3-3424fd862f8b
ms.openlocfilehash: dba4127ade9481b40d4fd3c4323532ddfedf6980
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994237"
---
# <a name="no-tracking-queries"></a><span data-ttu-id="42312-102">Sledování bez dotazy</span><span class="sxs-lookup"><span data-stu-id="42312-102">No-Tracking Queries</span></span>
<span data-ttu-id="42312-103">Někdy může chtít vrátit entity z dotazu, ale není nutné tyto entity sledovat pomocí kontextu.</span><span class="sxs-lookup"><span data-stu-id="42312-103">Sometimes you may want to get entities back from a query but not have those entities be tracked by the context.</span></span> <span data-ttu-id="42312-104">To může mít za následek lepší výkon při dotazování u velkého počtu entit ve scénářích jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="42312-104">This may result in better performance when querying for large numbers of entities in read-only scenarios.</span></span> <span data-ttu-id="42312-105">Postupy uvedené v tomto tématu se vztahují jak na modely vytvořené pomocí EF designeru a Code First.</span><span class="sxs-lookup"><span data-stu-id="42312-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="42312-106">Novou metodu rozšíření AsNoTracking umožňuje jakýkoli dotaz, který má být spuštěn tímto způsobem.</span><span class="sxs-lookup"><span data-stu-id="42312-106">A new extension method AsNoTracking allows any query to be run in this way.</span></span> <span data-ttu-id="42312-107">Příklad:</span><span class="sxs-lookup"><span data-stu-id="42312-107">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Query for all blogs without tracking them
    var blogs1 = context.Blogs.AsNoTracking();

    // Query for some blogs without tracking them
    var blogs2 = context.Blogs
                        .Where(b => b.Name.Contains(".NET"))
                        .AsNoTracking()
                        .ToList();
}
```  
