---
title: Migrace v prostředích Team - EF jádra
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 40cbc1c1bb0273bf733fadb884bffadcceeb162b
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
ms.locfileid: "26054724"
---
<a name="migrations-in-team-environments"></a><span data-ttu-id="6481f-102">Migrace v prostředích Team</span><span class="sxs-lookup"><span data-stu-id="6481f-102">Migrations in Team Environments</span></span>
===============================
<span data-ttu-id="6481f-103">Při práci s migrací v prostředích team, věnujte zvláštní pozornost snímku souboru modelu.</span><span class="sxs-lookup"><span data-stu-id="6481f-103">When working with Migrations in team environments, pay extra attention to the model snapshot file.</span></span> <span data-ttu-id="6481f-104">Tento soubor lze zjistit, pokud migrace vaší teammate slučuje řádně s vaším z Pokud potřebujete vyřešit konflikt znovu vytvořením migrace před její sdílení.</span><span class="sxs-lookup"><span data-stu-id="6481f-104">This file can tell you if your teammate's migration merges cleanly with yours of if you need to resolve a conflict by re-creating your migration before sharing it.</span></span>

<a name="merging"></a><span data-ttu-id="6481f-105">sloučení</span><span class="sxs-lookup"><span data-stu-id="6481f-105">Merging</span></span>
-------
<span data-ttu-id="6481f-106">Při slučování migrace z ostatními členy týmu, může dojít v konfliktu v snímku souboru modelu.</span><span class="sxs-lookup"><span data-stu-id="6481f-106">When you merge migrations from your teammates, you may get conflicts in your model snapshot file.</span></span> <span data-ttu-id="6481f-107">Pokud jsou obě změny nesouvisejícími, sloučení je jednoduchá a dvě migrace mohou existovat vedle sebe.</span><span class="sxs-lookup"><span data-stu-id="6481f-107">If both changes are unrelated, the merge is trivial and the two migrations can coexist.</span></span> <span data-ttu-id="6481f-108">Například může dojít ke konfliktu sloučení v konfiguraci typu entity zákazníka, které vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="6481f-108">For example, you may get a merge conflict in the customer entity type configuration that looks like this:</span></span>

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

<span data-ttu-id="6481f-109">Vzhledem k tomu, že obě tyto vlastnosti musí existovat v posledním modelu, dokončete sloučení přidáním obě vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="6481f-109">Since both of these properties need to exist in the final model, complete the merge by adding both properties.</span></span> <span data-ttu-id="6481f-110">V mnoha případech může váš systém správy verzí automaticky sloučení tyto změny za vás.</span><span class="sxs-lookup"><span data-stu-id="6481f-110">In many cases, your version control system may automatically merge such changes for you.</span></span>

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

<span data-ttu-id="6481f-111">V těchto případech migrace a vaše teammate migrace jsou navzájem nezávislé.</span><span class="sxs-lookup"><span data-stu-id="6481f-111">In these cases, your migration and your teammate's migration are independent of each other.</span></span> <span data-ttu-id="6481f-112">Vzhledem k tomu, že buď z nich může být použije první, nemusíte provádět žádné další změny migrace před sdílením se svým týmem.</span><span class="sxs-lookup"><span data-stu-id="6481f-112">Since either of them could be applied first, you don't need to make any additional changes to your migration before sharing it with your team.</span></span>

<a name="resolving-conflicts"></a><span data-ttu-id="6481f-113">Řešení konfliktů</span><span class="sxs-lookup"><span data-stu-id="6481f-113">Resolving conflicts</span></span>
-------------------
<span data-ttu-id="6481f-114">Někdy dojde true konflikt při slučování snímků model modelu.</span><span class="sxs-lookup"><span data-stu-id="6481f-114">Sometimes you encounter a true conflict when merging the model snapshot model.</span></span> <span data-ttu-id="6481f-115">Například můžete a vaše teammate může každý přejmenovaná stejnou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="6481f-115">For example, you and your teammate may each have renamed the same property.</span></span>

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

<span data-ttu-id="6481f-116">Pokud narazíte na tento druh konflikt vyřešte znovu vytvořením migrace.</span><span class="sxs-lookup"><span data-stu-id="6481f-116">If you encounter this kind of conflict, resolve it by re-creating your migration.</span></span> <span data-ttu-id="6481f-117">Postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="6481f-117">Follow these steps:</span></span>

1. <span data-ttu-id="6481f-118">Abort – sloučení a vrácení zpět do pracovního adresáře před sloučení</span><span class="sxs-lookup"><span data-stu-id="6481f-118">Abort the merge and rollback to your working directory before the merge</span></span>
2. <span data-ttu-id="6481f-119">Odebrat migrace (ale zachovat změny modelu)</span><span class="sxs-lookup"><span data-stu-id="6481f-119">Remove your migration (but keep your model changes)</span></span>
3. <span data-ttu-id="6481f-120">Sloučit změny vašeho teammate do pracovní adresář</span><span class="sxs-lookup"><span data-stu-id="6481f-120">Merge your teammate's changes into your working directory</span></span>
4. <span data-ttu-id="6481f-121">Znovu přidejte migrace</span><span class="sxs-lookup"><span data-stu-id="6481f-121">Re-add your migration</span></span>

<span data-ttu-id="6481f-122">Po této, můžete použít dvě migrace ve správném pořadí.</span><span class="sxs-lookup"><span data-stu-id="6481f-122">After doing this, the two migrations can be applied in the correct order.</span></span> <span data-ttu-id="6481f-123">Jejich migrace se použije první, přejmenování sloupce, který se *Alias*, následně migrace přejmenuje jeho *uživatelské jméno*.</span><span class="sxs-lookup"><span data-stu-id="6481f-123">Their migration is applied first, renaming the column to *Alias*, thereafter your migration renames it to *Username*.</span></span>

<span data-ttu-id="6481f-124">Migrace je možné bezpečně sdílet s zbytek týmu.</span><span class="sxs-lookup"><span data-stu-id="6481f-124">Your migration can safely be shared with the rest of the team.</span></span>
