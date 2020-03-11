---
title: Automaticky zjišťovat změny – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a8d1488d-9a54-4623-a76b-e81329ff2756
ms.openlocfilehash: 9af85fd7ca48a14432a1f33c59079fc438ef8810
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416975"
---
# <a name="automatic-detect-changes"></a>Automaticky zjišťovat změny
Při použití většiny POCO entit určení způsobu, jakým se entita změnila (a proto, které aktualizace je třeba odeslat do databáze), je zpracována algoritmem rozpoznat změny. Detekce změn funguje tak, že detekuje rozdíly mezi aktuálními hodnotami vlastností entity a původními hodnotami vlastností, které jsou uloženy ve snímku, když byla entita dotazována nebo připojena. Techniky uvedené v tomto tématu se vztahují rovnoměrně na modely vytvořené pomocí Code First a návrháře EF.  

Ve výchozím nastavení Entity Framework provádí rozpoznávání změn automaticky při volání následujících metod:  

- Negenerickými. Find  
- Negenerickými. Local  
- Negenerickými. Add  
- Negenerickými. AddRange
- Negenerickými. Remove  
- Negenerickými. RemoveRange
- Negenerickými. Attach  
- DbContext.SaveChanges  
- DbContext. GetValidationErrors  
- DbContext.Entry  
- DbChangeTracker. Entries  

## <a name="disabling-automatic-detection-of-changes"></a>Zákaz automatického zjišťování změn  

Pokud sledujete velké množství entit ve vašem kontextu a ve smyčce zavoláte jednu z těchto metod několikrát, může dojít k výraznému zlepšení výkonu vypnutím detekce změn po dobu trvání smyčky. Příklad:  

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

Nezapomeňte znovu povolit detekci změn po smyčce – použili jsme try/finally, abyste zajistili, že se vždycky znovu povolí i v případě, že kód ve smyčce vyvolá výjimku.  

Alternativou k zakázání a opětovnému povolení je ponechat automatické zjišťování změn vypnuté, a to buď v kontextu volání. ChangeTracker. DetectChanges explicitně nebo použijte proxy usilovně pro sledování změn. Obě tyto možnosti jsou rozšířené a můžou do své aplikace snadno zavádět drobné chyby, takže je budete moct používat opatrně.  

Pokud potřebujete přidat nebo odebrat mnoho objektů z kontextu, zvažte použití Negenerickými. AddRange a Negenerickými. RemoveRange. Tyto metody automaticky zjišťují změny až po dokončení operací přidání nebo odebrání. 
