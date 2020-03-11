---
title: Metoda load – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 03c5a069-b7b4-455f-a16f-ee3b96cc4e28
ms.openlocfilehash: bcea8ab2477f44281cd5de824457a72a84ccc766
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417121"
---
# <a name="the-load-method"></a>Metoda Load
K dispozici je několik scénářů, kdy můžete chtít načíst entity z databáze do kontextu, aniž by se tyto entity hned vynechaly. Dobrým příkladem toho je načítání entit pro datové vazby, jak je popsáno v tématu [místní data](~/ef6/querying/local-data.md). Jedním z běžných způsobů, jak to provést, je napsat dotaz LINQ a pak na něj zavolat ToList –, aby se seznam vytvořil okamžitě. Metoda load Extension funguje stejně jako ToList – s tím rozdílem, že se tak vyhne vytváření seznamu zcela.  

Techniky uvedené v tomto tématu se vztahují rovnoměrně na modely vytvořené pomocí Code First a návrháře EF.  

Tady jsou dva příklady použití metody Load. První je pořízena z aplikace model Windows Forms Data Binding, kde se k dotazování na entity před vytvořením vazby na místní kolekci používá načtení, jak je popsáno v tématu [místní data](~/ef6/querying/local-data.md):  

``` csharp
protected override void OnLoad(EventArgs e)
{
    base.OnLoad(e);

    _context = new ProductContext();

    _context.Categories.Load();
    categoryBindingSource.DataSource = _context.Categories.Local.ToBindingList();
}
```  

Druhý příklad ukazuje použití Load k načtení filtrované kolekce souvisejících entit, jak je popsáno v tématu [načítání souvisejících entit](~/ef6/querying/related-data.md):  

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
