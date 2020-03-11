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
# <a name="no-tracking-queries"></a>Žádné dotazy pro sledování
Někdy je možné, že budete chtít získat entity zpátky z dotazu, ale nechcete, aby tyto entity byly sledovány kontextem. To může mít za následek lepší výkon při dotazování na velký počet entit ve scénářích jen pro čtení. Techniky uvedené v tomto tématu se vztahují rovnoměrně na modely vytvořené pomocí Code First a návrháře EF.  

Nová rozšiřující metoda AsNoTracking umožňuje spustit libovolný dotaz tímto způsobem. Příklad:  

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
