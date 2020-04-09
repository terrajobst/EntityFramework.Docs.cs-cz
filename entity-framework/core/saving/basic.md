---
title: Základní uložení - EF core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
uid: core/saving/basic
ms.openlocfilehash: 066d67d6104316832a33f5a3648f1f2fa6cc9c50
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417633"
---
# <a name="basic-save"></a><span data-ttu-id="79956-102">Základní uložení</span><span class="sxs-lookup"><span data-stu-id="79956-102">Basic Save</span></span>

<span data-ttu-id="79956-103">Zjistěte, jak přidávat, upravovat a odebírat data pomocí tříd kontextu a entit.</span><span class="sxs-lookup"><span data-stu-id="79956-103">Learn how to add, modify, and remove data using your context and entity classes.</span></span>

> [!TIP]  
> <span data-ttu-id="79956-104">Ukázku tohoto článku [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Basics/) můžete zobrazit na GitHubu.</span><span class="sxs-lookup"><span data-stu-id="79956-104">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Basics/) on GitHub.</span></span>

## <a name="adding-data"></a><span data-ttu-id="79956-105">Přidání dat</span><span class="sxs-lookup"><span data-stu-id="79956-105">Adding Data</span></span>

<span data-ttu-id="79956-106">Pomocí metody *DbSet.Add* přidejte nové instance tříd entit.</span><span class="sxs-lookup"><span data-stu-id="79956-106">Use the *DbSet.Add* method to add new instances of your entity classes.</span></span> <span data-ttu-id="79956-107">Data budou vložena do databáze při volání *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="79956-107">The data will be inserted in the database when you call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> <span data-ttu-id="79956-108">Metody Add, Attach a Update všechny práce na úplný graf entit, které jim byly předány, jak je popsáno v části [Související data.](related-data.md)</span><span class="sxs-lookup"><span data-stu-id="79956-108">The Add, Attach, and Update methods all work on the full graph of entities passed to them, as described in the [Related Data](related-data.md) section.</span></span> <span data-ttu-id="79956-109">Alternativně EntityEntry.State vlastnost lze nastavit stav pouze jednu entitu.</span><span class="sxs-lookup"><span data-stu-id="79956-109">Alternately, the EntityEntry.State property can be used to set the state of just a single entity.</span></span> <span data-ttu-id="79956-110">Například, `context.Entry(blog).State = EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="79956-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="updating-data"></a><span data-ttu-id="79956-111">Aktualizace dat</span><span class="sxs-lookup"><span data-stu-id="79956-111">Updating Data</span></span>

<span data-ttu-id="79956-112">EF automaticky rozpozná změny provedené v existující entitě, která je sledována kontextem.</span><span class="sxs-lookup"><span data-stu-id="79956-112">EF will automatically detect changes made to an existing entity that is tracked by the context.</span></span> <span data-ttu-id="79956-113">To zahrnuje entity, které načtete nebo dotazujete z databáze, a entity, které byly dříve přidány a uloženy do databáze.</span><span class="sxs-lookup"><span data-stu-id="79956-113">This includes entities that you load/query from the database, and entities that were previously added and saved to the database.</span></span>

<span data-ttu-id="79956-114">Jednoduše upravte hodnoty přiřazené vlastnostem a pak zavolejte *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="79956-114">Simply modify the values assigned to properties and then call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a><span data-ttu-id="79956-115">Odstranění dat</span><span class="sxs-lookup"><span data-stu-id="79956-115">Deleting Data</span></span>

<span data-ttu-id="79956-116">Pomocí metody *DbSet.Remove* odstraňte instance tříd entit.</span><span class="sxs-lookup"><span data-stu-id="79956-116">Use the *DbSet.Remove* method to delete instances of your entity classes.</span></span>

<span data-ttu-id="79956-117">Pokud entita již v databázi existuje, bude během *savechanges odstraněna*.</span><span class="sxs-lookup"><span data-stu-id="79956-117">If the entity already exists in the database, it will be deleted during *SaveChanges*.</span></span> <span data-ttu-id="79956-118">Pokud entita ještě nebyla uložena do databáze (to znamená, že je sledována jako přidána), bude odebrána z kontextu a již nebude vložena při volání *SaveChanges.*</span><span class="sxs-lookup"><span data-stu-id="79956-118">If the entity has not yet been saved to the database (that is, it is tracked as added) then it will be removed from the context and will no longer be inserted when *SaveChanges* is called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a><span data-ttu-id="79956-119">Více operací v jednom SaveChanges</span><span class="sxs-lookup"><span data-stu-id="79956-119">Multiple Operations in a single SaveChanges</span></span>

<span data-ttu-id="79956-120">Můžete kombinovat více Přidat nebo aktualizovat nebo odebrat operace do jednoho volání *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="79956-120">You can combine multiple Add/Update/Remove operations into a single call to *SaveChanges*.</span></span>

> [!NOTE]  
> <span data-ttu-id="79956-121">Pro většinu poskytovatelů databáze *SaveChanges* je transakční.</span><span class="sxs-lookup"><span data-stu-id="79956-121">For most database providers, *SaveChanges* is transactional.</span></span> <span data-ttu-id="79956-122">To znamená, že všechny operace budou úspěšné nebo neúspěšné a operace nebudou nikdy částečně použity.</span><span class="sxs-lookup"><span data-stu-id="79956-122">This means  all the operations will either succeed or fail and the operations will never be left partially applied.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#MultipleOperations)]
