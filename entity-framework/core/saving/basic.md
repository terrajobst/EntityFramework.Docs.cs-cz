---
title: Základní uložit - EF jádra
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
ms.technology: entity-framework-core
uid: core/saving/basic
ms.openlocfilehash: 35bf14af43289ad6308a49482d3f45a7a8be9067
ms.sourcegitcommit: 72e59e6af86b568653e1b29727529dfd7f65d312
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34754393"
---
# <a name="basic-save"></a><span data-ttu-id="87c97-102">Základní uložit</span><span class="sxs-lookup"><span data-stu-id="87c97-102">Basic Save</span></span>

<span data-ttu-id="87c97-103">Naučte se přidávat, upravovat a odebírat dat pomocí třídy kontextu a entity.</span><span class="sxs-lookup"><span data-stu-id="87c97-103">Learn how to add, modify, and remove data using your context and entity classes.</span></span>

> [!TIP]  
> <span data-ttu-id="87c97-104">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="87c97-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) on GitHub.</span></span>

## <a name="adding-data"></a><span data-ttu-id="87c97-105">Přidávání dat</span><span class="sxs-lookup"><span data-stu-id="87c97-105">Adding Data</span></span>

<span data-ttu-id="87c97-106">Použití *DbSet.Add* metoda pro přidání nové instance třídy entity.</span><span class="sxs-lookup"><span data-stu-id="87c97-106">Use the *DbSet.Add* method to add new instances of your entity classes.</span></span> <span data-ttu-id="87c97-107">Data se vloží v databázi při volání *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="87c97-107">The data will be inserted in the database when you call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> <span data-ttu-id="87c97-108">Metody přidat, připojit a aktualizace všech pracovních na úplné graf entit předaný, jak je popsáno v [související Data](related-data.md) části.</span><span class="sxs-lookup"><span data-stu-id="87c97-108">The Add, Attach, and Update methods all work on the full graph of entities passed to them, as described in the [Related Data](related-data.md) section.</span></span> <span data-ttu-id="87c97-109">Alternativně vlastnost EntityEntry.State slouží k nastavení stavu právě jednu entitu.</span><span class="sxs-lookup"><span data-stu-id="87c97-109">Alternately, the EntityEntry.State property can be used to set the state of just a single entity.</span></span> <span data-ttu-id="87c97-110">Například `context.Entry(blog).State = EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="87c97-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="updating-data"></a><span data-ttu-id="87c97-111">Aktualizace dat</span><span class="sxs-lookup"><span data-stu-id="87c97-111">Updating Data</span></span>

<span data-ttu-id="87c97-112">EF automaticky zjistí změny provedené v existující entita, která se sleduje kontextu.</span><span class="sxs-lookup"><span data-stu-id="87c97-112">EF will automatically detect changes made to an existing entity that is tracked by the context.</span></span> <span data-ttu-id="87c97-113">To zahrnuje entity, které můžete načíst nebo dotazu z databáze a entity, které byly dříve přidány a uloženy do databáze.</span><span class="sxs-lookup"><span data-stu-id="87c97-113">This includes entities that you load/query from the database, and entities that were previously added and saved to the database.</span></span>

<span data-ttu-id="87c97-114">Stačí upravit hodnot přiřazených vlastnosti a pak zavolají *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="87c97-114">Simply modify the values assigned to properties and then call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a><span data-ttu-id="87c97-115">Odstraňování dat</span><span class="sxs-lookup"><span data-stu-id="87c97-115">Deleting Data</span></span>

<span data-ttu-id="87c97-116">Použití *DbSet.Remove* metoda odstranění instancí tříd entity.</span><span class="sxs-lookup"><span data-stu-id="87c97-116">Use the *DbSet.Remove* method to delete instances of your entity classes.</span></span>

<span data-ttu-id="87c97-117">Pokud entita již existuje v databázi, se odstraní při *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="87c97-117">If the entity already exists in the database, it will be deleted during *SaveChanges*.</span></span> <span data-ttu-id="87c97-118">Pokud entita ještě nebyl uložen do databáze (tj. jeho sledování jak byl přidán) pak ji bude odebrána z kontextu a bude vložen při *SaveChanges* je volána.</span><span class="sxs-lookup"><span data-stu-id="87c97-118">If the entity has not yet been saved to the database (i.e. it is tracked as added) then it will be removed from the context and will no longer be inserted when *SaveChanges* is called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a><span data-ttu-id="87c97-119">Více operací v jednom SaveChanges</span><span class="sxs-lookup"><span data-stu-id="87c97-119">Multiple Operations in a single SaveChanges</span></span>

<span data-ttu-id="87c97-120">Můžete kombinovat více operací přidat/aktualizovat nebo odebrat do jediné volání *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="87c97-120">You can combine multiple Add/Update/Remove operations into a single call to *SaveChanges*.</span></span>

> [!NOTE]  
> <span data-ttu-id="87c97-121">Pro většinu poskytovatelů databáze *SaveChanges* transakční.</span><span class="sxs-lookup"><span data-stu-id="87c97-121">For most database providers, *SaveChanges* is transactional.</span></span> <span data-ttu-id="87c97-122">To znamená, že všechny operace bude úspěch nebo neúspěch a operace se nikdy vlevo použijí částečně.</span><span class="sxs-lookup"><span data-stu-id="87c97-122">This means  all the operations will either succeed or fail and the operations will never be left partially applied.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#MultipleOperations)]
