---
title: Metoda Load - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 03c5a069-b7b4-455f-a16f-ee3b96cc4e28
ms.openlocfilehash: bcea8ab2477f44281cd5de824457a72a84ccc766
ms.sourcegitcommit: 4a795285004612ac03ab26532ac09ca333cb4c8f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2018
ms.locfileid: "50123814"
---
# <a name="the-load-method"></a>Metoda Load
Existuje několik scénářů, kde můžete chtít načtení entit z databáze do kontextu bez hned teď zrovna nic nedělá s těmito entitami. Dobrým příkladem je načítání entit pro datovou vazbu, jak je popsáno v [místních dat](~/ef6/querying/local-data.md). Jedním z běžných způsobů k tomu je psaní dotazu LINQ a pak zavolat ToList, pouze se okamžitě zahodit vytvořeného seznamu. Metoda rozšíření zatížení funguje stejně jako ToList s tím rozdílem, že vytvoření seznamu úplně vyhnete.  

Postupy uvedené v tomto tématu se vztahují jak na modely vytvořené pomocí EF designeru a Code First.  

Tady jsou dva příklady použití zatížení. První je převzata z aplikace Windows Forms datové vazby, kde se zatížení používá k dotazování pro entity před vazby do místní kolekce, jak je popsáno v [místních dat](~/ef6/querying/local-data.md):  

``` csharp
protected override void OnLoad(EventArgs e)
{
    base.OnLoad(e);

    _context = new ProductContext();

    _context.Categories.Load();
    categoryBindingSource.DataSource = _context.Categories.Local.ToBindingList();
}
```  

Druhý příklad ukazuje použití zatížení načíst filtrovanou sadu souvisejících entit, jak je popsáno v [načtení souvisejících entit](~/ef6/querying/related-data.md):  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog
    context.Entry(blog)
        .Collection(b => b.Posts)
        .Query()
        .Where(p => p.Tags.Contains("entity-framework"))
        .Load();
}
```  
