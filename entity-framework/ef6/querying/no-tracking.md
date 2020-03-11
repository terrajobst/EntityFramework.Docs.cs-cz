---
title: Žádné dotazy sledování – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: f80ac260-c2dc-484d-94a3-3424fd862f8b
ms.openlocfilehash: 44d58e14a2550bd08a8edd68b467237f6f5b5978
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417100"
---
# <a name="no-tracking-queries"></a><span data-ttu-id="d903f-102">Žádné dotazy pro sledování</span><span class="sxs-lookup"><span data-stu-id="d903f-102">No-Tracking Queries</span></span>
<span data-ttu-id="d903f-103">Někdy je možné, že budete chtít získat entity zpátky z dotazu, ale nechcete, aby tyto entity byly sledovány kontextem.</span><span class="sxs-lookup"><span data-stu-id="d903f-103">Sometimes you may want to get entities back from a query but not have those entities be tracked by the context.</span></span> <span data-ttu-id="d903f-104">To může mít za následek lepší výkon při dotazování na velký počet entit ve scénářích jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="d903f-104">This may result in better performance when querying for large numbers of entities in read-only scenarios.</span></span> <span data-ttu-id="d903f-105">Techniky uvedené v tomto tématu se vztahují rovnoměrně na modely vytvořené pomocí Code First a návrháře EF.</span><span class="sxs-lookup"><span data-stu-id="d903f-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="d903f-106">Nová rozšiřující metoda AsNoTracking umožňuje spustit libovolný dotaz tímto způsobem.</span><span class="sxs-lookup"><span data-stu-id="d903f-106">A new extension method AsNoTracking allows any query to be run in this way.</span></span> <span data-ttu-id="d903f-107">Příklad:</span><span class="sxs-lookup"><span data-stu-id="d903f-107">For example:</span></span>  

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
