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
# <a name="automatic-detect-changes"></a><span data-ttu-id="64455-102">Automaticky zjišťovat změny</span><span class="sxs-lookup"><span data-stu-id="64455-102">Automatic detect changes</span></span>
<span data-ttu-id="64455-103">Při použití většiny POCO entit určení způsobu, jakým se entita změnila (a proto, které aktualizace je třeba odeslat do databáze), je zpracována algoritmem rozpoznat změny.</span><span class="sxs-lookup"><span data-stu-id="64455-103">When using most POCO entities the determination of how an entity has changed (and therefore which updates need to be sent to the database) is handled by the Detect Changes algorithm.</span></span> <span data-ttu-id="64455-104">Detekce změn funguje tak, že detekuje rozdíly mezi aktuálními hodnotami vlastností entity a původními hodnotami vlastností, které jsou uloženy ve snímku, když byla entita dotazována nebo připojena.</span><span class="sxs-lookup"><span data-stu-id="64455-104">Detect Changes works by detecting the differences between the current property values of the entity and the original property values that are stored in a snapshot when the entity was queried or attached.</span></span> <span data-ttu-id="64455-105">Techniky uvedené v tomto tématu se vztahují rovnoměrně na modely vytvořené pomocí Code First a návrháře EF.</span><span class="sxs-lookup"><span data-stu-id="64455-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="64455-106">Ve výchozím nastavení Entity Framework provádí rozpoznávání změn automaticky při volání následujících metod:</span><span class="sxs-lookup"><span data-stu-id="64455-106">By default, Entity Framework performs Detect Changes automatically when the following methods are called:</span></span>  

- <span data-ttu-id="64455-107">Negenerickými. Find</span><span class="sxs-lookup"><span data-stu-id="64455-107">DbSet.Find</span></span>  
- <span data-ttu-id="64455-108">Negenerickými. Local</span><span class="sxs-lookup"><span data-stu-id="64455-108">DbSet.Local</span></span>  
- <span data-ttu-id="64455-109">Negenerickými. Add</span><span class="sxs-lookup"><span data-stu-id="64455-109">DbSet.Add</span></span>  
- <span data-ttu-id="64455-110">Negenerickými. AddRange</span><span class="sxs-lookup"><span data-stu-id="64455-110">DbSet.AddRange</span></span>
- <span data-ttu-id="64455-111">Negenerickými. Remove</span><span class="sxs-lookup"><span data-stu-id="64455-111">DbSet.Remove</span></span>  
- <span data-ttu-id="64455-112">Negenerickými. RemoveRange</span><span class="sxs-lookup"><span data-stu-id="64455-112">DbSet.RemoveRange</span></span>
- <span data-ttu-id="64455-113">Negenerickými. Attach</span><span class="sxs-lookup"><span data-stu-id="64455-113">DbSet.Attach</span></span>  
- <span data-ttu-id="64455-114">DbContext.SaveChanges</span><span class="sxs-lookup"><span data-stu-id="64455-114">DbContext.SaveChanges</span></span>  
- <span data-ttu-id="64455-115">DbContext. GetValidationErrors</span><span class="sxs-lookup"><span data-stu-id="64455-115">DbContext.GetValidationErrors</span></span>  
- <span data-ttu-id="64455-116">DbContext.Entry</span><span class="sxs-lookup"><span data-stu-id="64455-116">DbContext.Entry</span></span>  
- <span data-ttu-id="64455-117">DbChangeTracker. Entries</span><span class="sxs-lookup"><span data-stu-id="64455-117">DbChangeTracker.Entries</span></span>  

## <a name="disabling-automatic-detection-of-changes"></a><span data-ttu-id="64455-118">Zákaz automatického zjišťování změn</span><span class="sxs-lookup"><span data-stu-id="64455-118">Disabling automatic detection of changes</span></span>  

<span data-ttu-id="64455-119">Pokud sledujete velké množství entit ve vašem kontextu a ve smyčce zavoláte jednu z těchto metod několikrát, může dojít k výraznému zlepšení výkonu vypnutím detekce změn po dobu trvání smyčky.</span><span class="sxs-lookup"><span data-stu-id="64455-119">If you are tracking a lot of entities in your context and you call one of these methods many times in a loop, then you may get significant performance improvements by turning off detection of changes for the duration of the loop.</span></span> <span data-ttu-id="64455-120">Příklad:</span><span class="sxs-lookup"><span data-stu-id="64455-120">For example:</span></span>  

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

<span data-ttu-id="64455-121">Nezapomeňte znovu povolit detekci změn po smyčce – použili jsme try/finally, abyste zajistili, že se vždycky znovu povolí i v případě, že kód ve smyčce vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="64455-121">Don’t forget to re-enable detection of changes after the loop — We've used a try/finally to ensure it is always re-enabled even if code in the loop throws an exception.</span></span>  

<span data-ttu-id="64455-122">Alternativou k zakázání a opětovnému povolení je ponechat automatické zjišťování změn vypnuté, a to buď v kontextu volání. ChangeTracker. DetectChanges explicitně nebo použijte proxy usilovně pro sledování změn.</span><span class="sxs-lookup"><span data-stu-id="64455-122">An alternative to disabling and re-enabling is to leave automatic detection of changes turned off at all times and either call context.ChangeTracker.DetectChanges explicitly or use change tracking proxies diligently.</span></span> <span data-ttu-id="64455-123">Obě tyto možnosti jsou rozšířené a můžou do své aplikace snadno zavádět drobné chyby, takže je budete moct používat opatrně.</span><span class="sxs-lookup"><span data-stu-id="64455-123">Both of these options are advanced and can easily introduce subtle bugs into your application so use them with care.</span></span>  

<span data-ttu-id="64455-124">Pokud potřebujete přidat nebo odebrat mnoho objektů z kontextu, zvažte použití Negenerickými. AddRange a Negenerickými. RemoveRange.</span><span class="sxs-lookup"><span data-stu-id="64455-124">If you need to add or remove many objects from a context, consider using DbSet.AddRange and DbSet.RemoveRange.</span></span> <span data-ttu-id="64455-125">Tyto metody automaticky zjišťují změny až po dokončení operací přidání nebo odebrání.</span><span class="sxs-lookup"><span data-stu-id="64455-125">This methods automatically detect changes only once after the add or remove operations are completed.</span></span> 
