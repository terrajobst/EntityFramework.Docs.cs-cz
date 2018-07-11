---
title: Základní uložení – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
ms.technology: entity-framework-core
uid: core/saving/basic
ms.openlocfilehash: ecf8f344a5baae37a5e7255a4affb1085f1b3ff3
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949402"
---
# <a name="basic-save"></a><span data-ttu-id="05a0d-102">Základní uložení</span><span class="sxs-lookup"><span data-stu-id="05a0d-102">Basic Save</span></span>

<span data-ttu-id="05a0d-103">Zjistěte, jak přidávat, upravovat a odebírat dat pomocí třídy kontextu a entity.</span><span class="sxs-lookup"><span data-stu-id="05a0d-103">Learn how to add, modify, and remove data using your context and entity classes.</span></span>

> [!TIP]  
> <span data-ttu-id="05a0d-104">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="05a0d-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) on GitHub.</span></span>

## <a name="adding-data"></a><span data-ttu-id="05a0d-105">Přidání dat</span><span class="sxs-lookup"><span data-stu-id="05a0d-105">Adding Data</span></span>

<span data-ttu-id="05a0d-106">Použití *DbSet.Add* způsob, jak přidat nové instance třídy entity.</span><span class="sxs-lookup"><span data-stu-id="05a0d-106">Use the *DbSet.Add* method to add new instances of your entity classes.</span></span> <span data-ttu-id="05a0d-107">Data se vloží do databáze při volání *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="05a0d-107">The data will be inserted in the database when you call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> <span data-ttu-id="05a0d-108">Metody přidat, připojit a aktualizovat všechny práce na úplný graf entity předaný k nim, jak je popsáno v [souvisejících dat](related-data.md) oddílu.</span><span class="sxs-lookup"><span data-stu-id="05a0d-108">The Add, Attach, and Update methods all work on the full graph of entities passed to them, as described in the [Related Data](related-data.md) section.</span></span> <span data-ttu-id="05a0d-109">Alternativně EntityEntry.State vlastnost lze nastavit stav pouze jednu entitu.</span><span class="sxs-lookup"><span data-stu-id="05a0d-109">Alternately, the EntityEntry.State property can be used to set the state of just a single entity.</span></span> <span data-ttu-id="05a0d-110">Například `context.Entry(blog).State = EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="05a0d-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="updating-data"></a><span data-ttu-id="05a0d-111">Aktualizace dat</span><span class="sxs-lookup"><span data-stu-id="05a0d-111">Updating Data</span></span>

<span data-ttu-id="05a0d-112">EF automaticky rozpozná změny provedené na existující entitu, která je sledována podle kontextu.</span><span class="sxs-lookup"><span data-stu-id="05a0d-112">EF will automatically detect changes made to an existing entity that is tracked by the context.</span></span> <span data-ttu-id="05a0d-113">To zahrnuje entity, které můžete načíst/dotazu z databáze a entity, které byly dříve přidány a uloženy do databáze.</span><span class="sxs-lookup"><span data-stu-id="05a0d-113">This includes entities that you load/query from the database, and entities that were previously added and saved to the database.</span></span>

<span data-ttu-id="05a0d-114">Stačí upravit hodnoty přiřazené k vlastnosti a pak vyvolejte *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="05a0d-114">Simply modify the values assigned to properties and then call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a><span data-ttu-id="05a0d-115">Odstranění dat</span><span class="sxs-lookup"><span data-stu-id="05a0d-115">Deleting Data</span></span>

<span data-ttu-id="05a0d-116">Použití *DbSet.Remove* metodu k odstranění instancí tříd entit.</span><span class="sxs-lookup"><span data-stu-id="05a0d-116">Use the *DbSet.Remove* method to delete instances of your entity classes.</span></span>

<span data-ttu-id="05a0d-117">Pokud entita již existuje v databázi, bude odstraněna během *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="05a0d-117">If the entity already exists in the database, it will be deleted during *SaveChanges*.</span></span> <span data-ttu-id="05a0d-118">Pokud entita ještě nebyla uložena do databáze (to znamená, ho je sledovat přidávání), se odeberou z kontextu a nebude vložen při *SaveChanges* je volána.</span><span class="sxs-lookup"><span data-stu-id="05a0d-118">If the entity has not yet been saved to the database (that is, it is tracked as added) then it will be removed from the context and will no longer be inserted when *SaveChanges* is called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a><span data-ttu-id="05a0d-119">Více operací v jedné SaveChanges</span><span class="sxs-lookup"><span data-stu-id="05a0d-119">Multiple Operations in a single SaveChanges</span></span>

<span data-ttu-id="05a0d-120">Můžete zkombinovat několik operací přidání/aktualizaci/odebrání do jednoho volání *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="05a0d-120">You can combine multiple Add/Update/Remove operations into a single call to *SaveChanges*.</span></span>

> [!NOTE]  
> <span data-ttu-id="05a0d-121">Pro většinu poskytovatelů databáze *SaveChanges* je transakční.</span><span class="sxs-lookup"><span data-stu-id="05a0d-121">For most database providers, *SaveChanges* is transactional.</span></span> <span data-ttu-id="05a0d-122">To znamená, že všechny operace bude úspěch nebo neúspěch a operace se nikdy vlevo použijí částečně.</span><span class="sxs-lookup"><span data-stu-id="05a0d-122">This means  all the operations will either succeed or fail and the operations will never be left partially applied.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#MultipleOperations)]
