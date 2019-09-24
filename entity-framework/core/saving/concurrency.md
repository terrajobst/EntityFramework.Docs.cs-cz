---
title: Zpracování konfliktů souběžnosti – EF Core
author: rowanmiller
ms.date: 03/03/2018
uid: core/saving/concurrency
ms.openlocfilehash: 4d6ff24e58caa0b228e9c1e4313beda78d1025fc
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197828"
---
# <a name="handling-concurrency-conflicts"></a><span data-ttu-id="e05ca-102">Zpracování konfliktů souběžnosti</span><span class="sxs-lookup"><span data-stu-id="e05ca-102">Handling Concurrency Conflicts</span></span>

> [!NOTE]
> <span data-ttu-id="e05ca-103">Tato stránka popisuje, jak souběžnost funguje v EF Core a jak zpracovávat konflikty souběžnosti ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e05ca-103">This page documents how concurrency works in EF Core and how to handle concurrency conflicts in your application.</span></span> <span data-ttu-id="e05ca-104">Podrobnosti o tom, jak konfigurovat tokeny souběžnosti v modelu, najdete v tématu [tokeny souběžnosti](xref:core/modeling/concurrency) .</span><span class="sxs-lookup"><span data-stu-id="e05ca-104">See [Concurrency Tokens](xref:core/modeling/concurrency) for details on how to configure concurrency tokens in your model.</span></span>

> [!TIP]
> <span data-ttu-id="e05ca-105">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Concurrency/) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="e05ca-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Concurrency/) on GitHub.</span></span>

<span data-ttu-id="e05ca-106">_Souběžnost databáze_ odkazuje na situace, ve kterých více procesů nebo uživatelů přistupuje nebo mění stejná data v databázi ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="e05ca-106">_Database concurrency_ refers to situations in which multiple processes or users access or change the same data in a database at the same time.</span></span> <span data-ttu-id="e05ca-107">_Řízení souběžnosti_ odkazuje na konkrétní mechanismy, které slouží k zajištění konzistence dat v přítomnosti souběžných změn.</span><span class="sxs-lookup"><span data-stu-id="e05ca-107">_Concurrency control_ refers to specific mechanisms used to ensure data consistency in presence of concurrent changes.</span></span>

<span data-ttu-id="e05ca-108">EF Core implementuje _optimistické řízení souběžnosti_, což znamená, že umožní více procesů nebo uživatelům provádět změny nezávisle bez režie synchronizace nebo uzamčení.</span><span class="sxs-lookup"><span data-stu-id="e05ca-108">EF Core implements _optimistic concurrency control_, meaning that it will let multiple processes or users make changes independently without the overhead of synchronization or locking.</span></span> <span data-ttu-id="e05ca-109">V ideálním případě tyto změny nebudou navzájem ovlivněny, a proto budou moci být úspěšné.</span><span class="sxs-lookup"><span data-stu-id="e05ca-109">In the ideal situation, these changes will not interfere with each other and therefore will be able to succeed.</span></span> <span data-ttu-id="e05ca-110">V nejhorším případě se dva nebo více procesů pokusí provést konfliktní změny a pouze jeden z nich by měl být úspěšný.</span><span class="sxs-lookup"><span data-stu-id="e05ca-110">In the worst case scenario, two or more processes will attempt to make conflicting changes, and only one of them should succeed.</span></span>

## <a name="how-concurrency-control-works-in-ef-core"></a><span data-ttu-id="e05ca-111">Jak funguje řízení souběžnosti v EF Core</span><span class="sxs-lookup"><span data-stu-id="e05ca-111">How concurrency control works in EF Core</span></span>

<span data-ttu-id="e05ca-112">Vlastnosti konfigurované jako tokeny souběžnosti se používají k implementaci optimistického řízení souběžnosti: kdykoli se provede `SaveChanges`operace Update nebo DELETE, hodnota tokenu souběžnosti v databázi se porovná s původní hodnotou. hodnota čtená EF Core.</span><span class="sxs-lookup"><span data-stu-id="e05ca-112">Properties configured as concurrency tokens are used to implement optimistic concurrency control: whenever an update or delete operation is performed during `SaveChanges`, the value of the concurrency token on the database is compared against the original value read by EF Core.</span></span>

- <span data-ttu-id="e05ca-113">Pokud se hodnoty shodují, operace může být dokončena.</span><span class="sxs-lookup"><span data-stu-id="e05ca-113">If the values match, the operation can complete.</span></span>
- <span data-ttu-id="e05ca-114">Pokud se hodnoty neshodují, EF Core předpokládá, že jiný uživatel provedl konfliktní operaci a přeruší aktuální transakci.</span><span class="sxs-lookup"><span data-stu-id="e05ca-114">If the values do not match, EF Core assumes that another user has performed a conflicting operation and aborts the current transaction.</span></span>

<span data-ttu-id="e05ca-115">Situace, kdy jiný uživatel provedl operaci, která je v konfliktu s aktuální operací, se označuje jako _konflikt souběžnosti_.</span><span class="sxs-lookup"><span data-stu-id="e05ca-115">The situation when another user has performed an operation that conflicts with the current operation is known as _concurrency conflict_.</span></span>

<span data-ttu-id="e05ca-116">Poskytovatelé databáze zodpovídají za implementaci porovnání hodnot tokenu souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="e05ca-116">Database providers are responsible for implementing the comparison of concurrency token values.</span></span>

<span data-ttu-id="e05ca-117">V relačních databázích EF Core obsahuje kontrolu hodnoty tokenu souběžnosti v `WHERE` klauzuli libovolných `UPDATE` příkazů nebo `DELETE` .</span><span class="sxs-lookup"><span data-stu-id="e05ca-117">On relational databases EF Core includes a check for the value of the concurrency token in the `WHERE` clause of any `UPDATE` or `DELETE` statements.</span></span> <span data-ttu-id="e05ca-118">Po provedení příkazů EF Core přečte počet ovlivněných řádků.</span><span class="sxs-lookup"><span data-stu-id="e05ca-118">After executing the statements, EF Core reads the number of rows that were affected.</span></span>

<span data-ttu-id="e05ca-119">Pokud nejsou ovlivněny žádné řádky, je zjištěn konflikt souběžnosti a EF Core vyvolá `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="e05ca-119">If no rows are affected, a concurrency conflict is detected, and EF Core throws `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="e05ca-120">Můžeme například chtít nakonfigurovat `LastName` `Person` , aby byl token souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="e05ca-120">For example, we may want to configure `LastName` on `Person` to be a concurrency token.</span></span> <span data-ttu-id="e05ca-121">Pak všechny operace aktualizace na osobu budou zahrnovat kontrolu souběžnosti v `WHERE` klauzuli:</span><span class="sxs-lookup"><span data-stu-id="e05ca-121">Then any update operation on Person will include the concurrency check in the `WHERE` clause:</span></span>

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a><span data-ttu-id="e05ca-122">Řešení konfliktů souběžnosti</span><span class="sxs-lookup"><span data-stu-id="e05ca-122">Resolving concurrency conflicts</span></span>

<span data-ttu-id="e05ca-123">Pokud budete pokračovat s předchozím příkladem, pokud se jeden uživatel pokusí uložit některé změny do `Person`, ale jiný uživatel je již `LastName`změnil, bude vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="e05ca-123">Continuing with the previous example, if one user tries to save some changes to a `Person`, but another user has already changed the `LastName`, then an exception will be thrown.</span></span>

<span data-ttu-id="e05ca-124">V tomto okamžiku může aplikace jednoduše informovat uživatele o tom, že aktualizace nebyla úspěšná kvůli konfliktním změnám a přesunutím.</span><span class="sxs-lookup"><span data-stu-id="e05ca-124">At this point, the application could simply inform the user that the update was not successful due to conflicting changes and move on.</span></span> <span data-ttu-id="e05ca-125">Může být ale žádoucí vyzvat uživatele, aby tento záznam stále představoval stejnou skutečnou osobu a operaci zkuste zopakovat.</span><span class="sxs-lookup"><span data-stu-id="e05ca-125">But it may be desirable to prompt the user to ensure this record still represents the same actual person and to retry the operation.</span></span>

<span data-ttu-id="e05ca-126">Tento proces je příkladem _řešení konfliktu souběžnosti_.</span><span class="sxs-lookup"><span data-stu-id="e05ca-126">This process is an example of _resolving a concurrency conflict_.</span></span>

<span data-ttu-id="e05ca-127">Vyřešení konfliktu souběžnosti zahrnuje sloučení nedokončených změn z aktuální `DbContext` hodnoty s hodnotami v databázi.</span><span class="sxs-lookup"><span data-stu-id="e05ca-127">Resolving a concurrency conflict involves merging the pending changes from the current `DbContext` with the values in the database.</span></span> <span data-ttu-id="e05ca-128">Hodnoty, které se sloučí, se budou lišit v závislosti na aplikaci a můžou být směrovány uživatelským vstupem.</span><span class="sxs-lookup"><span data-stu-id="e05ca-128">What values get merged will vary based on the application and may be directed by user input.</span></span>

<span data-ttu-id="e05ca-129">**Existují tři sady hodnot, které umožňují vyřešit konflikt souběžnosti:**</span><span class="sxs-lookup"><span data-stu-id="e05ca-129">**There are three sets of values available to help resolve a concurrency conflict:**</span></span>

* <span data-ttu-id="e05ca-130">**Aktuální hodnoty** jsou hodnoty, které aplikace pokoušela zapisovat do databáze.</span><span class="sxs-lookup"><span data-stu-id="e05ca-130">**Current values** are the values that the application was attempting to write to the database.</span></span>

* <span data-ttu-id="e05ca-131">**Původní hodnoty** jsou hodnoty, které byly původně načteny z databáze před provedením jakýchkoli úprav.</span><span class="sxs-lookup"><span data-stu-id="e05ca-131">**Original values** are the values that were originally retrieved from the database, before any edits were made.</span></span>

* <span data-ttu-id="e05ca-132">**Hodnoty databáze** jsou hodnoty, které jsou aktuálně uloženy v databázi.</span><span class="sxs-lookup"><span data-stu-id="e05ca-132">**Database values** are the values currently stored in the database.</span></span>

<span data-ttu-id="e05ca-133">Obecný přístup ke zpracování konfliktů souběžnosti je:</span><span class="sxs-lookup"><span data-stu-id="e05ca-133">The general approach to handle a concurrency conflicts is:</span></span>

1. <span data-ttu-id="e05ca-134">Zachytit `DbUpdateConcurrencyException` během `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="e05ca-134">Catch `DbUpdateConcurrencyException` during `SaveChanges`.</span></span>
2. <span data-ttu-id="e05ca-135">Slouží `DbUpdateConcurrencyException.Entries` k přípravě nové sady změn pro ovlivněné entity.</span><span class="sxs-lookup"><span data-stu-id="e05ca-135">Use `DbUpdateConcurrencyException.Entries` to prepare a new set of changes for the affected entities.</span></span>
3. <span data-ttu-id="e05ca-136">Aktualizuje původní hodnoty tokenu souběžnosti tak, aby odrážely aktuální hodnoty v databázi.</span><span class="sxs-lookup"><span data-stu-id="e05ca-136">Refresh the original values of the concurrency token to reflect the current values in the database.</span></span>
4. <span data-ttu-id="e05ca-137">Opakujte tento postup, dokud nedojde k žádnému konfliktu.</span><span class="sxs-lookup"><span data-stu-id="e05ca-137">Retry the process until no conflicts occur.</span></span>

<span data-ttu-id="e05ca-138">V následujícím příkladu `Person.FirstName` a `Person.LastName` jsou nastavení jako tokeny souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="e05ca-138">In the following example, `Person.FirstName` and `Person.LastName` are setup as concurrency tokens.</span></span> <span data-ttu-id="e05ca-139">K dispozici `// TODO:` je komentář v umístění, kam zahrnete logiku specifickou pro aplikaci pro výběr hodnoty, která se má uložit.</span><span class="sxs-lookup"><span data-stu-id="e05ca-139">There is a `// TODO:` comment in the location where you include application specific logic to choose the value to be saved.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
