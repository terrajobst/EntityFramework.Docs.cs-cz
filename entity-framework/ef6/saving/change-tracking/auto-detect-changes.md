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
# <a name="automatic-detect-changes"></a><span data-ttu-id="168eb-102">Automatické zjištění změn</span><span class="sxs-lookup"><span data-stu-id="168eb-102">Automatic detect changes</span></span>
<span data-ttu-id="168eb-103">Při použití většiny entit POCO určení jak došlo ke změně entity (a proto aktualizací musí být odeslán do databáze) zařizuje služba algoritmů detekovat změny.</span><span class="sxs-lookup"><span data-stu-id="168eb-103">When using most POCO entities the determination of how an entity has changed (and therefore which updates need to be sent to the database) is handled by the Detect Changes algorithm.</span></span> <span data-ttu-id="168eb-104">Rozpoznat změny funguje zjišťuje rozdíly mezi aktuální hodnoty vlastností entity a původní hodnoty vlastností, které jsou uloženy v snímku, když se entita dotazovat nebo připojené.</span><span class="sxs-lookup"><span data-stu-id="168eb-104">Detect Changes works by detecting the differences between the current property values of the entity and the original property values that are stored in a snapshot when the entity was queried or attached.</span></span> <span data-ttu-id="168eb-105">Postupy uvedené v tomto tématu se vztahují jak na modely vytvořené pomocí EF designeru a Code First.</span><span class="sxs-lookup"><span data-stu-id="168eb-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="168eb-106">Ve výchozím nastavení Entity Framework provádí automaticky rozpoznat změny, při volání těchto metod:</span><span class="sxs-lookup"><span data-stu-id="168eb-106">By default, Entity Framework performs Detect Changes automatically when the following methods are called:</span></span>  

- <span data-ttu-id="168eb-107">DbSet.Find</span><span class="sxs-lookup"><span data-stu-id="168eb-107">DbSet.Find</span></span>  
- <span data-ttu-id="168eb-108">DbSet.Local</span><span class="sxs-lookup"><span data-stu-id="168eb-108">DbSet.Local</span></span>  
- <span data-ttu-id="168eb-109">DbSet.Add</span><span class="sxs-lookup"><span data-stu-id="168eb-109">DbSet.Add</span></span>  
- <span data-ttu-id="168eb-110">DbSet.AddRange</span><span class="sxs-lookup"><span data-stu-id="168eb-110">DbSet.AddRange</span></span>
- <span data-ttu-id="168eb-111">DbSet.Remove</span><span class="sxs-lookup"><span data-stu-id="168eb-111">DbSet.Remove</span></span>  
- <span data-ttu-id="168eb-112">DbSet.RemoveRange</span><span class="sxs-lookup"><span data-stu-id="168eb-112">DbSet.RemoveRange</span></span>
- <span data-ttu-id="168eb-113">DbSet.Attach</span><span class="sxs-lookup"><span data-stu-id="168eb-113">DbSet.Attach</span></span>  
- <span data-ttu-id="168eb-114">DbContext.SaveChanges</span><span class="sxs-lookup"><span data-stu-id="168eb-114">DbContext.SaveChanges</span></span>  
- <span data-ttu-id="168eb-115">DbContext.GetValidationErrors</span><span class="sxs-lookup"><span data-stu-id="168eb-115">DbContext.GetValidationErrors</span></span>  
- <span data-ttu-id="168eb-116">DbContext.Entry</span><span class="sxs-lookup"><span data-stu-id="168eb-116">DbContext.Entry</span></span>  
- <span data-ttu-id="168eb-117">DbChangeTracker.Entries</span><span class="sxs-lookup"><span data-stu-id="168eb-117">DbChangeTracker.Entries</span></span>  

## <a name="disabling-automatic-detection-of-changes"></a><span data-ttu-id="168eb-118">Zakázání automatického zjišťování změn</span><span class="sxs-lookup"><span data-stu-id="168eb-118">Disabling automatic detection of changes</span></span>  

<span data-ttu-id="168eb-119">Pokud sledujete mnoho entit v kontext a volání jedné z těchto metod v mnoha případech ve smyčce, může získat výrazné zlepšení výkonu tím, že vypíná zjišťování změn po dobu trvání smyčky.</span><span class="sxs-lookup"><span data-stu-id="168eb-119">If you are tracking a lot of entities in your context and you call one of these methods many times in a loop, then you may get significant performance improvements by turning off detection of changes for the duration of the loop.</span></span> <span data-ttu-id="168eb-120">Příklad:</span><span class="sxs-lookup"><span data-stu-id="168eb-120">For example:</span></span>  

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

<span data-ttu-id="168eb-121">Nezapomeňte znovu povolit zjišťování změn po smyčce – jsme použili try/finally zajistit, je vždy znovu povoleno i v případě, že kód ve smyčce vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="168eb-121">Don’t forget to re-enable detection of changes after the loop — We've used a try/finally to ensure it is always re-enabled even if code in the loop throws an exception.</span></span>  

<span data-ttu-id="168eb-122">Alternativa k zakázání a opětovného povolení je ponechat automatické zjišťování změn vypnuto na celou dobu a buď kontext volání. ChangeTracker.DetectChanges explicitně nebo použití usilovně změnit proxy služby sledování.</span><span class="sxs-lookup"><span data-stu-id="168eb-122">An alternative to disabling and re-enabling is to leave automatic detection of changes turned off at all times and either call context.ChangeTracker.DetectChanges explicitly or use change tracking proxies diligently.</span></span> <span data-ttu-id="168eb-123">Obě tyto možnosti jsou rozšířené a můžou snadno způsobit drobné chyby do své aplikace tak používána opatrně.</span><span class="sxs-lookup"><span data-stu-id="168eb-123">Both of these options are advanced and can easily introduce subtle bugs into your application so use them with care.</span></span>  

<span data-ttu-id="168eb-124">Pokud potřebujete přidat nebo odebrat mnoho objektů z kontextu, zvažte použití DbSet.AddRange a DbSet.RemoveRange.</span><span class="sxs-lookup"><span data-stu-id="168eb-124">If you need to add or remove many objects from a context, consider using DbSet.AddRange and DbSet.RemoveRange.</span></span> <span data-ttu-id="168eb-125">Tyto metody automaticky pouze jednou zjištění změn, po dokončení operace přidat nebo odebrat.</span><span class="sxs-lookup"><span data-stu-id="168eb-125">This methods automatically detect changes only once after the add or remove operations are completed.</span></span> 
