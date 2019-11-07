---
title: Migrace v týmových prostředích – EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/teams
ms.openlocfilehash: 6c17c56277821159962884aef72d46c624442e20
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655548"
---
# <a name="migrations-in-team-environments"></a><span data-ttu-id="5439f-102">Migrace v týmových prostředích</span><span class="sxs-lookup"><span data-stu-id="5439f-102">Migrations in Team Environments</span></span>

<span data-ttu-id="5439f-103">Při práci s migracemi v týmových prostředích věnujte zvláštní pozornost souboru snímku modelu.</span><span class="sxs-lookup"><span data-stu-id="5439f-103">When working with Migrations in team environments, pay extra attention to the model snapshot file.</span></span> <span data-ttu-id="5439f-104">Tento soubor vám může sdělit, jestli vaše migrace společník bez problémů slučuje s vámi, nebo pokud potřebujete vyřešit konflikt tím, že ho před jeho sdílením znovu vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="5439f-104">This file can tell you if your teammate's migration merges cleanly with yours or if you need to resolve a conflict by re-creating your migration before sharing it.</span></span>

## <a name="merging"></a><span data-ttu-id="5439f-105">sloučení</span><span class="sxs-lookup"><span data-stu-id="5439f-105">Merging</span></span>

<span data-ttu-id="5439f-106">Při sloučení migrace z ostatními týmu můžete v souboru snímku modelu Zobrazit konflikty.</span><span class="sxs-lookup"><span data-stu-id="5439f-106">When you merge migrations from your teammates, you may get conflicts in your model snapshot file.</span></span> <span data-ttu-id="5439f-107">Pokud se obě změny netýkají, je sloučení triviální a můžou dvě migrace koexistovat.</span><span class="sxs-lookup"><span data-stu-id="5439f-107">If both changes are unrelated, the merge is trivial and the two migrations can coexist.</span></span> <span data-ttu-id="5439f-108">Například může dojít ke konfliktu při sloučení v konfiguraci typu entity zákazníka, která vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="5439f-108">For example, you may get a merge conflict in the customer entity type configuration that looks like this:</span></span>

``` output
<<<<<<< Mine
b.Property<bool>("Deactivated");
=======
b.Property<int>("LoyaltyPoints");
>>>>>>> Theirs
```

<span data-ttu-id="5439f-109">Vzhledem k tomu, že obě tyto vlastnosti musí existovat v konečném modelu, dokončete sloučení přidáním obou vlastností.</span><span class="sxs-lookup"><span data-stu-id="5439f-109">Since both of these properties need to exist in the final model, complete the merge by adding both properties.</span></span> <span data-ttu-id="5439f-110">V mnoha případech může systém správy verzí tyto změny automaticky sloučit.</span><span class="sxs-lookup"><span data-stu-id="5439f-110">In many cases, your version control system may automatically merge such changes for you.</span></span>

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

<span data-ttu-id="5439f-111">V těchto případech je migrace a migrace společník vzájemně nezávislá.</span><span class="sxs-lookup"><span data-stu-id="5439f-111">In these cases, your migration and your teammate's migration are independent of each other.</span></span> <span data-ttu-id="5439f-112">Vzhledem k tomu, že některé z nich je možné použít jako první, nemusíte provádět žádné další změny v migraci předtím, než je budete moct sdílet se svým týmem.</span><span class="sxs-lookup"><span data-stu-id="5439f-112">Since either of them could be applied first, you don't need to make any additional changes to your migration before sharing it with your team.</span></span>

## <a name="resolving-conflicts"></a><span data-ttu-id="5439f-113">Řešení konfliktů</span><span class="sxs-lookup"><span data-stu-id="5439f-113">Resolving conflicts</span></span>

<span data-ttu-id="5439f-114">Někdy při slučování modelu snímku modelu dochází ke skutečnému konfliktu.</span><span class="sxs-lookup"><span data-stu-id="5439f-114">Sometimes you encounter a true conflict when merging the model snapshot model.</span></span> <span data-ttu-id="5439f-115">Například vaše společník může přejmenovat stejnou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="5439f-115">For example, you and your teammate may each have renamed the same property.</span></span>

``` output
<<<<<<< Mine
b.Property<string>("Username");
=======
b.Property<string>("Alias");
>>>>>>> Theirs
```

<span data-ttu-id="5439f-116">Pokud se setkáte s tímto druhem konfliktu, vyřešte ho tím, že znovu vytvoříte migraci.</span><span class="sxs-lookup"><span data-stu-id="5439f-116">If you encounter this kind of conflict, resolve it by re-creating your migration.</span></span> <span data-ttu-id="5439f-117">Postupujte podle těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="5439f-117">Follow these steps:</span></span>

1. <span data-ttu-id="5439f-118">Před sloučením přerušit sloučení a vrátit se zpátky do svého pracovního adresáře</span><span class="sxs-lookup"><span data-stu-id="5439f-118">Abort the merge and rollback to your working directory before the merge</span></span>
2. <span data-ttu-id="5439f-119">Odebrání migrace (ale zachovat změny modelu)</span><span class="sxs-lookup"><span data-stu-id="5439f-119">Remove your migration (but keep your model changes)</span></span>
3. <span data-ttu-id="5439f-120">Sloučit změny společník do pracovního adresáře</span><span class="sxs-lookup"><span data-stu-id="5439f-120">Merge your teammate's changes into your working directory</span></span>
4. <span data-ttu-id="5439f-121">Opětovné přidání migrace</span><span class="sxs-lookup"><span data-stu-id="5439f-121">Re-add your migration</span></span>

<span data-ttu-id="5439f-122">Po provedení této akce lze dvě migrace použít ve správném pořadí.</span><span class="sxs-lookup"><span data-stu-id="5439f-122">After doing this, the two migrations can be applied in the correct order.</span></span> <span data-ttu-id="5439f-123">Nejprve se použije migrace, aby se sloupec přejmenoval na *alias*, a potom se migrace přejmenuje na *uživatelské jméno*.</span><span class="sxs-lookup"><span data-stu-id="5439f-123">Their migration is applied first, renaming the column to *Alias*, thereafter your migration renames it to *Username*.</span></span>

<span data-ttu-id="5439f-124">Vaši migraci můžete bezpečně sdílet se zbytkem týmu.</span><span class="sxs-lookup"><span data-stu-id="5439f-124">Your migration can safely be shared with the rest of the team.</span></span>
