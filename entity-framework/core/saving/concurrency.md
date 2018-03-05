---
title: "Zpracování konfliktů souběžnosti - EF jádra"
author: rowanmiller
ms.author: divega
ms.date: 03/03/2018
ms.technology: entity-framework-core
uid: core/saving/concurrency
ms.openlocfilehash: 288d9c6fced5ebbaa2c366248c68547502c3698e
ms.sourcegitcommit: 8f3be0a2a394253efb653388ec66bda964e5ee1b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/05/2018
---
# <a name="handling-concurrency-conflicts"></a><span data-ttu-id="0f748-102">Zpracování konfliktů souběžnosti</span><span class="sxs-lookup"><span data-stu-id="0f748-102">Handling Concurrency Conflicts</span></span>

> [!NOTE]
> <span data-ttu-id="0f748-103">Tato stránka dokumenty fungování souběžnosti v EF jádra a způsobu řešení konfliktů souběžnosti ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0f748-103">This page documents how concurrency works in EF Core and how to handle concurrency conflicts in your application.</span></span> <span data-ttu-id="0f748-104">V tématu [tokenů souběžnosti](xref:core/modeling/concurrency) podrobnosti o tom, jak nakonfigurovat tokenů souběžnosti v modelu.</span><span class="sxs-lookup"><span data-stu-id="0f748-104">See [Concurrency Tokens](xref:core/modeling/concurrency) for details on how to configure concurrency tokens in your model.</span></span>

> [!TIP]
> <span data-ttu-id="0f748-105">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="0f748-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) on GitHub.</span></span>

<span data-ttu-id="0f748-106">_Databáze souběžnosti_ odkazuje na situace, ve kterých více procesů nebo uživatelé přístup, nebo změňte stejná data v databázi ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="0f748-106">_Database concurrency_ refers to situations in which multiple processes or users access or change the same data in a database at the same time.</span></span> <span data-ttu-id="0f748-107">_Kontrola souběžnosti_ odkazuje na konkrétní mechanismy, které se používá k zajištění konzistence dat v přítomnost souběžných změny.</span><span class="sxs-lookup"><span data-stu-id="0f748-107">_Concurrency control_ refers to specific mechanisms used to ensure data consistency in presence of concurrent changes.</span></span>

<span data-ttu-id="0f748-108">Implementuje EF základní _optimistické řízení souběžného_, což znamená, že se vám umožní více procesů nebo uživatelé provádět změny nezávisle bez režie synchronizace nebo uzamčení.</span><span class="sxs-lookup"><span data-stu-id="0f748-108">EF Core implements _optimistic concurrency control_, meaning that it will let multiple processes or users make changes independently without the overhead of synchronization or locking.</span></span> <span data-ttu-id="0f748-109">V ideálním případě tyto změny nebude docházet k vzájemnému rušení a proto nebudou úspěšné.</span><span class="sxs-lookup"><span data-stu-id="0f748-109">In the ideal situation, these changes will not interfere with each other and therefore will be able to succeed.</span></span> <span data-ttu-id="0f748-110">V nejhorším scénářem případu dvě nebo více procesů se pokusí provést konfliktní změny a pouze jeden z nich by měl být úspěšné.</span><span class="sxs-lookup"><span data-stu-id="0f748-110">In the worst case scenario, two or more processes will attempt to make conflicting changes, and only one of them should succeed.</span></span>

## <a name="how-concurrency-control-works-in-ef-core"></a><span data-ttu-id="0f748-111">Jak funguje řízení souběžného zpracování EF jádra</span><span class="sxs-lookup"><span data-stu-id="0f748-111">How concurrency control works in EF Core</span></span>

<span data-ttu-id="0f748-112">Vlastnosti nakonfigurovaný jako tokenů souběžnosti slouží k implementaci optimistické řízení souběžného: vždy, když se provádí operace aktualizace nebo odstranění během `SaveChanges`, hodnota tokenu souběžnosti v databázi se porovná s původní Hodnota číst EF jádra.</span><span class="sxs-lookup"><span data-stu-id="0f748-112">Properties configured as concurrency tokens are used to implement optimistic concurrency control: whenever an update or delete operation is performed during `SaveChanges`, the value of the concurrency token on the database is compared against the original value read by EF Core.</span></span>

- <span data-ttu-id="0f748-113">Pokud se hodnoty shodují, můžete dokončit operaci.</span><span class="sxs-lookup"><span data-stu-id="0f748-113">If the values match, the operation can complete.</span></span>
- <span data-ttu-id="0f748-114">Pokud se hodnoty neshodují, EF základní předpokládá, že jiný uživatel provedl konfliktní operace a zruší aktuální transakci.</span><span class="sxs-lookup"><span data-stu-id="0f748-114">If the values do not match, EF Core assumes that another user has performed a conflicting operation and aborts the current transaction.</span></span>

<span data-ttu-id="0f748-115">V případě, když jiného uživatele byla provedena operace, které jsou v konfliktu s aktuální operace se označuje jako _souběžnosti konflikt_.</span><span class="sxs-lookup"><span data-stu-id="0f748-115">The situation when another user has performed an operation that conflicts with the current operation is known as _concurrency conflict_.</span></span>

<span data-ttu-id="0f748-116">Zprostředkovatelé databáze jsou zodpovědní za implementaci porovnání hodnot token souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="0f748-116">Database providers are responsible for implementing the comparison of concurrency token values.</span></span>

<span data-ttu-id="0f748-117">U relačních databází EF základní zahrnuje kontrolu hodnoty tokenu souběžnosti v `WHERE` klauzule libovolného `UPDATE` nebo `DELETE` příkazy.</span><span class="sxs-lookup"><span data-stu-id="0f748-117">On relational databases EF Core includes a check for the value of the concurrency token in the `WHERE` clause of any `UPDATE` or `DELETE` statements.</span></span> <span data-ttu-id="0f748-118">Po provedení příkazy, přečte EF základní počet řádků, které situace měla vliv na.</span><span class="sxs-lookup"><span data-stu-id="0f748-118">After executing the statements, EF Core reads the number of rows that were affected.</span></span>

<span data-ttu-id="0f748-119">Pokud jsou vliv na žádné řádky, je zjištěn konflikt souběžnosti a vyvolá EF základní `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="0f748-119">If no rows are affected, a concurrency conflict is detected, and EF Core throws `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="0f748-120">Například může chceme konfigurovat `LastName` na `Person` být token souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="0f748-120">For example, we may want to configure `LastName` on `Person` to be a concurrency token.</span></span> <span data-ttu-id="0f748-121">Potom všechny operace aktualizace na osoba bude obsahovat souběžnosti změnami `WHERE` klauzule:</span><span class="sxs-lookup"><span data-stu-id="0f748-121">Then any update operation on Person will include the concurrency check in the `WHERE` clause:</span></span>

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a><span data-ttu-id="0f748-122">Řešení konfliktů souběžnosti</span><span class="sxs-lookup"><span data-stu-id="0f748-122">Resolving concurrency conflicts</span></span>

<span data-ttu-id="0f748-123">Pokračovat na předchozí příklad, pokud se jeden uživatel se pokusí uložit některé změny `Person`, ale již změnil jiný uživatel `LastName` k výjimce.</span><span class="sxs-lookup"><span data-stu-id="0f748-123">Continuing with the previous example, if one user tries to save some changes to a `Person`, but another user has already changed the `LastName` the an exception will be thrown.</span></span>

<span data-ttu-id="0f748-124">V tomto okamžiku může aplikace jednoduše informujte uživatele, že aktualizace nebyla úspěšná kvůli konfliktu změn a přesunutí na.</span><span class="sxs-lookup"><span data-stu-id="0f748-124">At this point, the application could simply inform the user that the update was not successful due to conflicting changes and move on.</span></span> <span data-ttu-id="0f748-125">Ale může být žádoucí výzvy, ujistěte se, že tento záznam stále představuje stejná skutečná osoba a operaci opakujte.</span><span class="sxs-lookup"><span data-stu-id="0f748-125">But it may be desirable to prompt the user to ensure this record still represents the same actual person and to retry the operation.</span></span>

<span data-ttu-id="0f748-126">Tento proces je příkladem _vyřešení konfliktu souběžnosti_.</span><span class="sxs-lookup"><span data-stu-id="0f748-126">This process is an example of _resolving a concurrency conflict_.</span></span>

<span data-ttu-id="0f748-127">Řešení konfliktů souběžnosti zahrnuje slučování čekajících změn z aktuální `DbContext` s hodnotami v databázi.</span><span class="sxs-lookup"><span data-stu-id="0f748-127">Resolving a concurrency conflict involves merging the pending changes from the current `DbContext` with the values in the database.</span></span> <span data-ttu-id="0f748-128">Jaké hodnoty nesloučí budou lišit v závislosti na aplikaci a může přesměrováni vstup uživatele.</span><span class="sxs-lookup"><span data-stu-id="0f748-128">What values get merged will vary based on the application and may be directed by user input.</span></span>

<span data-ttu-id="0f748-129">**Existují tři sady hodnot, které vám pomůžou vyřešit konflikt souběžnosti:**</span><span class="sxs-lookup"><span data-stu-id="0f748-129">**There are three sets of values available to help resolve a concurrency conflict:**</span></span>

* <span data-ttu-id="0f748-130">**Aktuální hodnoty** jsou hodnoty, které aplikace při pokusu o zápis do databáze.</span><span class="sxs-lookup"><span data-stu-id="0f748-130">**Current values** are the values that the application was attempting to write to the database.</span></span>

* <span data-ttu-id="0f748-131">**Původní hodnoty** jsou hodnoty, které byly původně načteny z databáze, před jakoukoli úpravou byly provedeny.</span><span class="sxs-lookup"><span data-stu-id="0f748-131">**Original values** are the values that were originally retrieved from the database, before any edits were made.</span></span>

* <span data-ttu-id="0f748-132">**Databáze hodnoty** jsou hodnoty, které jsou aktuálně uloženy v databázi.</span><span class="sxs-lookup"><span data-stu-id="0f748-132">**Database values** are the values currently stored in the database.</span></span>

<span data-ttu-id="0f748-133">Obecné postup pro zpracování konfliktů souběžnosti je:</span><span class="sxs-lookup"><span data-stu-id="0f748-133">The general approach to handle a concurrency conflicts is:</span></span>

1. <span data-ttu-id="0f748-134">Catch – `DbUpdateConcurrencyException` během `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="0f748-134">Catch `DbUpdateConcurrencyException` during `SaveChanges`.</span></span>
2. <span data-ttu-id="0f748-135">Použití `DbUpdateConcurrencyException.Entries` připravit novou sadu změn pro ovlivněné entity.</span><span class="sxs-lookup"><span data-stu-id="0f748-135">Use `DbUpdateConcurrencyException.Entries` to prepare a new set of changes for the affected entities.</span></span>
3. <span data-ttu-id="0f748-136">Obnovte původní hodnoty tokenu souběžnosti tak, aby odrážela aktuálních hodnot v databázi.</span><span class="sxs-lookup"><span data-stu-id="0f748-136">Refresh the original values of the concurrency token to reflect the current values in the database.</span></span>
4. <span data-ttu-id="0f748-137">Proces opakujte, dokud dojít ke konfliktům.</span><span class="sxs-lookup"><span data-stu-id="0f748-137">Retry the process until no conflicts occur.</span></span>

<span data-ttu-id="0f748-138">V následujícím příkladu `Person.FirstName` a `Person.LastName` se instalační program jako tokenů souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="0f748-138">In the following example, `Person.FirstName` and `Person.LastName` are setup as concurrency tokens.</span></span> <span data-ttu-id="0f748-139">Je `// TODO:` komentář do umístění, kde zahrnete určitou logiku aplikace zvolte hodnotu uložit.</span><span class="sxs-lookup"><span data-stu-id="0f748-139">There is a `// TODO:` comment in the location where you include application specific logic to choose the value to be saved.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
