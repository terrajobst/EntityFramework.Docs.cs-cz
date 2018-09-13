---
title: Automatické zjištění změn - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a8d1488d-9a54-4623-a76b-e81329ff2756
ms.openlocfilehash: 9af85fd7ca48a14432a1f33c59079fc438ef8810
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490984"
---
# <a name="automatic-detect-changes"></a>Automatické zjištění změn
Při použití většiny entit POCO určení jak došlo ke změně entity (a proto aktualizací musí být odeslán do databáze) zařizuje služba algoritmů detekovat změny. Rozpoznat změny funguje zjišťuje rozdíly mezi aktuální hodnoty vlastností entity a původní hodnoty vlastností, které jsou uloženy v snímku, když se entita dotazovat nebo připojené. Postupy uvedené v tomto tématu se vztahují jak na modely vytvořené pomocí EF designeru a Code First.  

Ve výchozím nastavení Entity Framework provádí automaticky rozpoznat změny, při volání těchto metod:  

- DbSet.Find  
- DbSet.Local  
- DbSet.Add  
- DbSet.AddRange
- DbSet.Remove  
- DbSet.RemoveRange
- DbSet.Attach  
- DbContext.SaveChanges  
- DbContext.GetValidationErrors  
- DbContext.Entry  
- DbChangeTracker.Entries  

## <a name="disabling-automatic-detection-of-changes"></a>Zakázání automatického zjišťování změn  

Pokud sledujete mnoho entit v kontext a volání jedné z těchto metod v mnoha případech ve smyčce, může získat výrazné zlepšení výkonu tím, že vypíná zjišťování změn po dobu trvání smyčky. Příklad:  

``` csharp
using (var context = new BloggingContext())
{
    try
    {
        context.Configuration.AutoDetectChangesEnabled = false;

        // Make many calls in a loop
        foreach (var blog in aLotOfBlogs)
        {
            context.Blogs.Add(blog);
        }
    }
    finally
    {
        context.Configuration.AutoDetectChangesEnabled = true;
    }
}
```  

Nezapomeňte znovu povolit zjišťování změn po smyčce – jsme použili try/finally zajistit, je vždy znovu povoleno i v případě, že kód ve smyčce vyvolá výjimku.  

Alternativa k zakázání a opětovného povolení je ponechat automatické zjišťování změn vypnuto na celou dobu a buď kontext volání. ChangeTracker.DetectChanges explicitně nebo použití usilovně změnit proxy služby sledování. Obě tyto možnosti jsou rozšířené a můžou snadno způsobit drobné chyby do své aplikace tak používána opatrně.  

Pokud potřebujete přidat nebo odebrat mnoho objektů z kontextu, zvažte použití DbSet.AddRange a DbSet.RemoveRange. Tyto metody automaticky pouze jednou zjištění změn, po dokončení operace přidat nebo odebrat. 
