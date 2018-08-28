---
title: Migrace v prostředích Team – EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: e8ff7f468d5ab6dbd6285f1abf9199e413288d10
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997692"
---
<a name="migrations-in-team-environments"></a><span data-ttu-id="22b60-102">Migrace v prostředích Team</span><span class="sxs-lookup"><span data-stu-id="22b60-102">Migrations in Team Environments</span></span>
===============================
<span data-ttu-id="22b60-103">Při práci s migrací v týmu prostředí je třeba věnujte zvláštní pozornost snímku souboru modelu.</span><span class="sxs-lookup"><span data-stu-id="22b60-103">When working with Migrations in team environments, pay extra attention to the model snapshot file.</span></span> <span data-ttu-id="22b60-104">Tento soubor můžete říct, pokud se migrace vašich programujete sloučí čistě s vaším nebo pokud je potřeba vyřešit konflikt tak, že znovu vytvoříte migraci před jejich sdílením.</span><span class="sxs-lookup"><span data-stu-id="22b60-104">This file can tell you if your teammate's migration merges cleanly with yours or if you need to resolve a conflict by re-creating your migration before sharing it.</span></span>

<a name="merging"></a><span data-ttu-id="22b60-105">sloučení</span><span class="sxs-lookup"><span data-stu-id="22b60-105">Merging</span></span>
-------
<span data-ttu-id="22b60-106">Při sloučení migrace z vašeho týmu, může zobrazit je v konfliktu v modelu snímku souboru.</span><span class="sxs-lookup"><span data-stu-id="22b60-106">When you merge migrations from your teammates, you may get conflicts in your model snapshot file.</span></span> <span data-ttu-id="22b60-107">Pokud jsou obě změny nesouvisející, sloučení je jednoduché a dvě migrace mohou existovat vedle sebe.</span><span class="sxs-lookup"><span data-stu-id="22b60-107">If both changes are unrelated, the merge is trivial and the two migrations can coexist.</span></span> <span data-ttu-id="22b60-108">Například může získat konfliktu při slučování v konfiguraci typu entity zákazník, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="22b60-108">For example, you may get a merge conflict in the customer entity type configuration that looks like this:</span></span>

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

<span data-ttu-id="22b60-109">Protože obě tyto vlastnosti musí existovat v finálního modelu, sloučit tak, že přidáte obě vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="22b60-109">Since both of these properties need to exist in the final model, complete the merge by adding both properties.</span></span> <span data-ttu-id="22b60-110">V mnoha případech může váš systém správy verzí automaticky sloučit tyto změny za vás.</span><span class="sxs-lookup"><span data-stu-id="22b60-110">In many cases, your version control system may automatically merge such changes for you.</span></span>

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

<span data-ttu-id="22b60-111">V těchto případech se migrace a migrace vašich programujete nezávisle na sobě navzájem.</span><span class="sxs-lookup"><span data-stu-id="22b60-111">In these cases, your migration and your teammate's migration are independent of each other.</span></span> <span data-ttu-id="22b60-112">Protože některou z nich můžete uplatnit v první, není nutné provádět žádné další změny migraci před jejich sdílením se svým týmem.</span><span class="sxs-lookup"><span data-stu-id="22b60-112">Since either of them could be applied first, you don't need to make any additional changes to your migration before sharing it with your team.</span></span>

<a name="resolving-conflicts"></a><span data-ttu-id="22b60-113">Řešení konfliktů</span><span class="sxs-lookup"><span data-stu-id="22b60-113">Resolving conflicts</span></span>
-------------------
<span data-ttu-id="22b60-114">Někdy narazíte na true konfliktu při slučování snímků model modelu.</span><span class="sxs-lookup"><span data-stu-id="22b60-114">Sometimes you encounter a true conflict when merging the model snapshot model.</span></span> <span data-ttu-id="22b60-115">Například vám a vaší programujete může každý přejmenovali stejnou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="22b60-115">For example, you and your teammate may each have renamed the same property.</span></span>

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

<span data-ttu-id="22b60-116">Pokud narazíte na tento druh konflikt vyřešte opětovné vytvoření migrace.</span><span class="sxs-lookup"><span data-stu-id="22b60-116">If you encounter this kind of conflict, resolve it by re-creating your migration.</span></span> <span data-ttu-id="22b60-117">Postupujte podle těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="22b60-117">Follow these steps:</span></span>

1. <span data-ttu-id="22b60-118">Zrušit sloučení a vrácení zpět do pracovního adresáře ještě před sloučením</span><span class="sxs-lookup"><span data-stu-id="22b60-118">Abort the merge and rollback to your working directory before the merge</span></span>
2. <span data-ttu-id="22b60-119">Odebrat migrace (ale zachovat změny modelu)</span><span class="sxs-lookup"><span data-stu-id="22b60-119">Remove your migration (but keep your model changes)</span></span>
3. <span data-ttu-id="22b60-120">Vaše programujete změny sloučíte do pracovního adresáře</span><span class="sxs-lookup"><span data-stu-id="22b60-120">Merge your teammate's changes into your working directory</span></span>
4. <span data-ttu-id="22b60-121">Migrace je znovu přidat</span><span class="sxs-lookup"><span data-stu-id="22b60-121">Re-add your migration</span></span>

<span data-ttu-id="22b60-122">Po této, můžete použít dvě migrace ve správném pořadí.</span><span class="sxs-lookup"><span data-stu-id="22b60-122">After doing this, the two migrations can be applied in the correct order.</span></span> <span data-ttu-id="22b60-123">Jejich migrace se použije první, přejmenování sloupce za účelem *Alias*, po tomto datu migrace přejmenuje na *uživatelské jméno*.</span><span class="sxs-lookup"><span data-stu-id="22b60-123">Their migration is applied first, renaming the column to *Alias*, thereafter your migration renames it to *Username*.</span></span>

<span data-ttu-id="22b60-124">Migrace je bezpečně sdílet s ostatními členy týmu.</span><span class="sxs-lookup"><span data-stu-id="22b60-124">Your migration can safely be shared with the rest of the team.</span></span>
