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
# <a name="the-load-method"></a><span data-ttu-id="148ac-102">Metoda Load</span><span class="sxs-lookup"><span data-stu-id="148ac-102">The Load Method</span></span>
<span data-ttu-id="148ac-103">K dispozici je několik scénářů, kdy můžete chtít načíst entity z databáze do kontextu, aniž by se tyto entity hned vynechaly.</span><span class="sxs-lookup"><span data-stu-id="148ac-103">There are several scenarios where you may want to load entities from the database into the context without immediately doing anything with those entities.</span></span> <span data-ttu-id="148ac-104">Dobrým příkladem toho je načítání entit pro datové vazby, jak je popsáno v tématu [místní data](~/ef6/querying/local-data.md).</span><span class="sxs-lookup"><span data-stu-id="148ac-104">A good example of this is loading entities for data binding as described in [Local Data](~/ef6/querying/local-data.md).</span></span> <span data-ttu-id="148ac-105">Jedním z běžných způsobů, jak to provést, je napsat dotaz LINQ a pak na něj zavolat ToList –, aby se seznam vytvořil okamžitě.</span><span class="sxs-lookup"><span data-stu-id="148ac-105">One common way to do this is to write a LINQ query and then call ToList on it, only to immediately discard the created list.</span></span> <span data-ttu-id="148ac-106">Metoda load Extension funguje stejně jako ToList – s tím rozdílem, že se tak vyhne vytváření seznamu zcela.</span><span class="sxs-lookup"><span data-stu-id="148ac-106">The Load extension method works just like ToList except that it avoids the creation of the list altogether.</span></span>  

<span data-ttu-id="148ac-107">Techniky uvedené v tomto tématu se vztahují rovnoměrně na modely vytvořené pomocí Code First a návrháře EF.</span><span class="sxs-lookup"><span data-stu-id="148ac-107">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="148ac-108">Tady jsou dva příklady použití metody Load.</span><span class="sxs-lookup"><span data-stu-id="148ac-108">Here are two examples of using Load.</span></span> <span data-ttu-id="148ac-109">První je pořízena z aplikace model Windows Forms Data Binding, kde se k dotazování na entity před vytvořením vazby na místní kolekci používá načtení, jak je popsáno v tématu [místní data](~/ef6/querying/local-data.md):</span><span class="sxs-lookup"><span data-stu-id="148ac-109">The first is taken from a Windows Forms data binding application where Load is used to query for entities before binding to the local collection, as described in [Local Data](~/ef6/querying/local-data.md):</span></span>  

``` csharp
protected override void OnLoad(EventArgs e)
{
    base.OnLoad(e);

    _context = new ProductContext();

    _context.Categories.Load();
    categoryBindingSource.DataSource = _context.Categories.Local.ToBindingList();
}
```  

<span data-ttu-id="148ac-110">Druhý příklad ukazuje použití Load k načtení filtrované kolekce souvisejících entit, jak je popsáno v tématu [načítání souvisejících entit](~/ef6/querying/related-data.md):</span><span class="sxs-lookup"><span data-stu-id="148ac-110">The second example shows using Load to load a filtered collection of related entities, as described in [Loading Related Entities](~/ef6/querying/related-data.md):</span></span>  

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
