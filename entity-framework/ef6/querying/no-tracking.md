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
# <a name="no-tracking-queries"></a>Sledování bez dotazy
Někdy může chtít vrátit entity z dotazu, ale není nutné tyto entity sledovat pomocí kontextu. To může mít za následek lepší výkon při dotazování u velkého počtu entit ve scénářích jen pro čtení. Postupy uvedené v tomto tématu se vztahují jak na modely vytvořené pomocí EF designeru a Code First.  

Novou metodu rozšíření AsNoTracking umožňuje jakýkoli dotaz, který má být spuštěn tímto způsobem. Příklad:  

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
