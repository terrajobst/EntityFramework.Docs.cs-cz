---
title: Metoda Load - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 03c5a069-b7b4-455f-a16f-ee3b96cc4e28
ms.openlocfilehash: 3a0d11552b6bfd8b83f15c58c6cb9f945d9d4536
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490893"
---
# <a name="the-load-method"></a><span data-ttu-id="5c802-102">Metoda Load</span><span class="sxs-lookup"><span data-stu-id="5c802-102">The Load Method</span></span>
<span data-ttu-id="5c802-103">Existuje několik scénářů, kde můžete chtít načtení entit z databáze do kontextu bez hned teď zrovna nic nedělá s těmito entitami.</span><span class="sxs-lookup"><span data-stu-id="5c802-103">There are several scenarios where you may want to load entities from the database into the context without immediately doing anything with those entities.</span></span> <span data-ttu-id="5c802-104">Dobrým příkladem je načítání entit pro datovou vazbu, jak je popsáno v [místních dat](~/ef6/querying/local-data.md).</span><span class="sxs-lookup"><span data-stu-id="5c802-104">A good example of this is loading entities for data binding as described in [Local Data](~/ef6/querying/local-data.md).</span></span> <span data-ttu-id="5c802-105">Jedním z běžných způsobů k tomu je psaní dotazu LINQ a pak zavolat ToList, pouze se okamžitě zahodit vytvořeného seznamu.</span><span class="sxs-lookup"><span data-stu-id="5c802-105">One common way to do this is to write a LINQ query and then call ToList on it, only to immediately discard the created list.</span></span> <span data-ttu-id="5c802-106">Metoda rozšíření zatížení funguje stejně jako ToList s tím rozdílem, že vytvoření seznamu úplně vyhnete.</span><span class="sxs-lookup"><span data-stu-id="5c802-106">The Load extension method works just like ToList except that it avoids the creation of the list altogether.</span></span>  

<span data-ttu-id="5c802-107">Postupy uvedené v tomto tématu se vztahují jak na modely vytvořené pomocí EF designeru a Code First.</span><span class="sxs-lookup"><span data-stu-id="5c802-107">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="5c802-108">Tady jsou dva příklady použití zatížení.</span><span class="sxs-lookup"><span data-stu-id="5c802-108">Here are two examples of using Load.</span></span> <span data-ttu-id="5c802-109">První je převzata z aplikace Windows Forms datové vazby, kde se zatížení používá k dotazování pro entity před vazby do místní kolekce, jak je popsáno v [místních dat](~/ef6/querying/local-data.md):</span><span class="sxs-lookup"><span data-stu-id="5c802-109">The first is taken from a Windows Forms data binding application where Load is used to query for entities before binding to the local collection, as described in [Local Data](~/ef6/querying/local-data.md):</span></span>  

``` csharp
protected override void OnLoad(EventArgs e)
{
    base.OnLoad(e);

    _context = new ProductContext();

    _context.Categories.Load();
    categoryBindingSource.DataSource = _context.Categories.Local.ToBindingList();
}
```  

<span data-ttu-id="5c802-110">Druhý příklad ukazuje použití zatížení načíst filtrovanou sadu souvisejících entit, jak je popsáno v [načtení souvisejících entit](~/ef6/querying/related-data.md):</span><span class="sxs-lookup"><span data-stu-id="5c802-110">The second example shows using Load to load a filtered collection of related entities, as described in [Loading Related Entities](~/ef6/querying/related-data.md):</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog
    context.Entry(blog)
        .Collection(b => b.Posts)
        .Query()
        .Where(p => p.Tags.Contains("entity-framework")
        .Load();
}
```  
