---
title: EF Core pro ukládání na úrovni Basic
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
uid: core/saving/basic
ms.openlocfilehash: 066d67d6104316832a33f5a3648f1f2fa6cc9c50
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417633"
---
# <a name="basic-save"></a><span data-ttu-id="da74a-102">Základní uložení</span><span class="sxs-lookup"><span data-stu-id="da74a-102">Basic Save</span></span>

<span data-ttu-id="da74a-103">Naučte se přidávat, upravovat a odebírat data pomocí tříd kontextu a entit.</span><span class="sxs-lookup"><span data-stu-id="da74a-103">Learn how to add, modify, and remove data using your context and entity classes.</span></span>

> [!TIP]  
> <span data-ttu-id="da74a-104">[Ukázku](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Basics/) tohoto článku můžete zobrazit na GitHubu.</span><span class="sxs-lookup"><span data-stu-id="da74a-104">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Basics/) on GitHub.</span></span>

## <a name="adding-data"></a><span data-ttu-id="da74a-105">Přidávání dat</span><span class="sxs-lookup"><span data-stu-id="da74a-105">Adding Data</span></span>

<span data-ttu-id="da74a-106">K přidání nových instancí tříd entit použijte metodu *negenerickými. Add* .</span><span class="sxs-lookup"><span data-stu-id="da74a-106">Use the *DbSet.Add* method to add new instances of your entity classes.</span></span> <span data-ttu-id="da74a-107">Data budou vložena do databáze při volání *metody SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="da74a-107">The data will be inserted in the database when you call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> <span data-ttu-id="da74a-108">Metody přidat, připojit a aktualizovat všechny pracují na úplných grafech entit, které jsou předány do nich, jak je popsáno v části [související data](related-data.md) .</span><span class="sxs-lookup"><span data-stu-id="da74a-108">The Add, Attach, and Update methods all work on the full graph of entities passed to them, as described in the [Related Data](related-data.md) section.</span></span> <span data-ttu-id="da74a-109">Alternativně lze vlastnost EntityEntry. State použít k nastavení stavu pouze jedné entity.</span><span class="sxs-lookup"><span data-stu-id="da74a-109">Alternately, the EntityEntry.State property can be used to set the state of just a single entity.</span></span> <span data-ttu-id="da74a-110">například `context.Entry(blog).State = EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="da74a-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="updating-data"></a><span data-ttu-id="da74a-111">Aktualizace dat</span><span class="sxs-lookup"><span data-stu-id="da74a-111">Updating Data</span></span>

<span data-ttu-id="da74a-112">EF automaticky zjistí změny provedené u existující entity, která je sledována kontextem.</span><span class="sxs-lookup"><span data-stu-id="da74a-112">EF will automatically detect changes made to an existing entity that is tracked by the context.</span></span> <span data-ttu-id="da74a-113">To zahrnuje entity, které načtete nebo dotazují z databáze, a entity, které byly dříve přidány a uloženy do databáze.</span><span class="sxs-lookup"><span data-stu-id="da74a-113">This includes entities that you load/query from the database, and entities that were previously added and saved to the database.</span></span>

<span data-ttu-id="da74a-114">Jednoduše upravte hodnoty přiřazené vlastnostem a potom zavolejte metodu *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="da74a-114">Simply modify the values assigned to properties and then call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a><span data-ttu-id="da74a-115">Odstranění dat</span><span class="sxs-lookup"><span data-stu-id="da74a-115">Deleting Data</span></span>

<span data-ttu-id="da74a-116">K odstranění instancí tříd entit použijte metodu *negenerickými. Remove* .</span><span class="sxs-lookup"><span data-stu-id="da74a-116">Use the *DbSet.Remove* method to delete instances of your entity classes.</span></span>

<span data-ttu-id="da74a-117">Pokud entita v databázi již existuje, bude odstraněna během *metody SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="da74a-117">If the entity already exists in the database, it will be deleted during *SaveChanges*.</span></span> <span data-ttu-id="da74a-118">Pokud entita ještě nebyla uložena do databáze (to znamená, že je sledována jako přidaná), bude odebrána z kontextu a nebude již vložena při volání *metody SaveChanges* .</span><span class="sxs-lookup"><span data-stu-id="da74a-118">If the entity has not yet been saved to the database (that is, it is tracked as added) then it will be removed from the context and will no longer be inserted when *SaveChanges* is called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a><span data-ttu-id="da74a-119">Více operací v jednom SaveChanges</span><span class="sxs-lookup"><span data-stu-id="da74a-119">Multiple Operations in a single SaveChanges</span></span>

<span data-ttu-id="da74a-120">Můžete zkombinovat více operací Přidat/aktualizovat/odebrat do jednoho volání *metody SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="da74a-120">You can combine multiple Add/Update/Remove operations into a single call to *SaveChanges*.</span></span>

> [!NOTE]  
> <span data-ttu-id="da74a-121">Pro většinu poskytovatelů databáze je *SaveChanges* transakční.</span><span class="sxs-lookup"><span data-stu-id="da74a-121">For most database providers, *SaveChanges* is transactional.</span></span> <span data-ttu-id="da74a-122">To znamená, že všechny operace budou buď úspěšné, nebo selžou a operace se nikdy nepoužijí částečně.</span><span class="sxs-lookup"><span data-stu-id="da74a-122">This means  all the operations will either succeed or fail and the operations will never be left partially applied.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#MultipleOperations)]
