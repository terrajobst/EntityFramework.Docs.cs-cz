---
title: Zpracování konfliktů souběžnosti – EF Core
author: rowanmiller
ms.date: 03/03/2018
uid: core/saving/concurrency
ms.openlocfilehash: e050b17bfa31a4785161c700bc0355e83162b405
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993109"
---
# <a name="handling-concurrency-conflicts"></a><span data-ttu-id="9f935-102">Zpracování konfliktů souběžnosti</span><span class="sxs-lookup"><span data-stu-id="9f935-102">Handling Concurrency Conflicts</span></span>

> [!NOTE]
> <span data-ttu-id="9f935-103">Tato stránka dokumenty fungování souběžnosti v EF Core a způsob zpracování konfliktů souběžnosti v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9f935-103">This page documents how concurrency works in EF Core and how to handle concurrency conflicts in your application.</span></span> <span data-ttu-id="9f935-104">Zobrazit [tokeny souběžnosti](xref:core/modeling/concurrency) podrobnosti o tom, jak nakonfigurovat tokeny souběžnosti v modelu.</span><span class="sxs-lookup"><span data-stu-id="9f935-104">See [Concurrency Tokens](xref:core/modeling/concurrency) for details on how to configure concurrency tokens in your model.</span></span>

> [!TIP]
> <span data-ttu-id="9f935-105">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="9f935-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) on GitHub.</span></span>

<span data-ttu-id="9f935-106">_Databáze souběžnosti_ odkazuje na situace, ve kterých několik procesů nebo uživatelé přístup nebo změna stejná data v databázi ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="9f935-106">_Database concurrency_ refers to situations in which multiple processes or users access or change the same data in a database at the same time.</span></span> <span data-ttu-id="9f935-107">_Řízení souběžnosti_ odkazuje na konkrétní mechanismus používaný k zajištění konzistence dat za přítomnosti souběžných změn.</span><span class="sxs-lookup"><span data-stu-id="9f935-107">_Concurrency control_ refers to specific mechanisms used to ensure data consistency in presence of concurrent changes.</span></span>

<span data-ttu-id="9f935-108">EF Core implementuje _optimistického řízení souběžnosti_, což znamená, že umožní více procesů nebo uživatelé provádět změny nezávisle na sobě bez režie synchronizace nebo uzamčení.</span><span class="sxs-lookup"><span data-stu-id="9f935-108">EF Core implements _optimistic concurrency control_, meaning that it will let multiple processes or users make changes independently without the overhead of synchronization or locking.</span></span> <span data-ttu-id="9f935-109">V ideálním případě tyto změny nebudou konfliktu mezi sebou a proto nebudete mít k úspěšnému.</span><span class="sxs-lookup"><span data-stu-id="9f935-109">In the ideal situation, these changes will not interfere with each other and therefore will be able to succeed.</span></span> <span data-ttu-id="9f935-110">V nejhorším případě případu dvě nebo více procesů se pokusí provést konfliktní změny a pouze jeden z nich uspěli.</span><span class="sxs-lookup"><span data-stu-id="9f935-110">In the worst case scenario, two or more processes will attempt to make conflicting changes, and only one of them should succeed.</span></span>

## <a name="how-concurrency-control-works-in-ef-core"></a><span data-ttu-id="9f935-111">Jak funguje řízení souběžnosti v EF Core</span><span class="sxs-lookup"><span data-stu-id="9f935-111">How concurrency control works in EF Core</span></span>

<span data-ttu-id="9f935-112">Vlastnosti nakonfigurovat, které se používají tokeny souběžnosti implementace optimistického řízení souběžnosti: vždy, když se provádí operaci update nebo delete během `SaveChanges`, hodnota tokenem souběžnosti v databázi se porovná s původní hodnota čtení pomocí EF Core.</span><span class="sxs-lookup"><span data-stu-id="9f935-112">Properties configured as concurrency tokens are used to implement optimistic concurrency control: whenever an update or delete operation is performed during `SaveChanges`, the value of the concurrency token on the database is compared against the original value read by EF Core.</span></span>

- <span data-ttu-id="9f935-113">Pokud se hodnoty shodují, můžete dokončit operaci.</span><span class="sxs-lookup"><span data-stu-id="9f935-113">If the values match, the operation can complete.</span></span>
- <span data-ttu-id="9f935-114">Pokud se hodnoty neshodují, EF Core předpokládá, že jiný uživatel provedl konfliktní operace a zruší aktuální transakce.</span><span class="sxs-lookup"><span data-stu-id="9f935-114">If the values do not match, EF Core assumes that another user has performed a conflicting operation and aborts the current transaction.</span></span>

<span data-ttu-id="9f935-115">Situace, při které jiný uživatel provedl operace, která je v konfliktu s aktuální operace se označuje jako _konfliktů souběžnosti_.</span><span class="sxs-lookup"><span data-stu-id="9f935-115">The situation when another user has performed an operation that conflicts with the current operation is known as _concurrency conflict_.</span></span>

<span data-ttu-id="9f935-116">Poskytovatelé databází zodpovídají za implementaci porovnání hodnoty tokenu souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="9f935-116">Database providers are responsible for implementing the comparison of concurrency token values.</span></span>

<span data-ttu-id="9f935-117">U relačních databází EF Core zahrnuje kontrolu hodnoty tokenu souběžnosti v `WHERE` klauzule žádné `UPDATE` nebo `DELETE` příkazy.</span><span class="sxs-lookup"><span data-stu-id="9f935-117">On relational databases EF Core includes a check for the value of the concurrency token in the `WHERE` clause of any `UPDATE` or `DELETE` statements.</span></span> <span data-ttu-id="9f935-118">Po spuštění příkazů, přečte EF Core počet řádků, které byly ovlivněny.</span><span class="sxs-lookup"><span data-stu-id="9f935-118">After executing the statements, EF Core reads the number of rows that were affected.</span></span>

<span data-ttu-id="9f935-119">Pokud jsou ovlivněny žádné řádky, zjištění konfliktu souběžnosti a EF Core vyvolá `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="9f935-119">If no rows are affected, a concurrency conflict is detected, and EF Core throws `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="9f935-120">Například může chceme konfigurovat `LastName` na `Person` bude tokenem souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="9f935-120">For example, we may want to configure `LastName` on `Person` to be a concurrency token.</span></span> <span data-ttu-id="9f935-121">Pak všechny operace aktualizace na osobu, budou obsahovat kontrolu souběžnosti v `WHERE` klauzule:</span><span class="sxs-lookup"><span data-stu-id="9f935-121">Then any update operation on Person will include the concurrency check in the `WHERE` clause:</span></span>

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a><span data-ttu-id="9f935-122">Řešení konfliktů souběžnosti</span><span class="sxs-lookup"><span data-stu-id="9f935-122">Resolving concurrency conflicts</span></span>

<span data-ttu-id="9f935-123">Pokračování v předchozím příkladu, v případě, že jeden uživatel pokusí uložit některé změny `Person`, ale již byl změněn jiným uživatelem `LastName`, pak bude vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="9f935-123">Continuing with the previous example, if one user tries to save some changes to a `Person`, but another user has already changed the `LastName`, then an exception will be thrown.</span></span>

<span data-ttu-id="9f935-124">V tomto okamžiku může aplikace jednoduše informovat uživatele, že aktualizace nebyla úspěšná kvůli konfliktním změnám a přesunout.</span><span class="sxs-lookup"><span data-stu-id="9f935-124">At this point, the application could simply inform the user that the update was not successful due to conflicting changes and move on.</span></span> <span data-ttu-id="9f935-125">Ale může být vhodné požádat uživatele, ujistěte se, že tento záznam představuje stále stejná skutečná osoba a zkuste operaci zopakovat.</span><span class="sxs-lookup"><span data-stu-id="9f935-125">But it may be desirable to prompt the user to ensure this record still represents the same actual person and to retry the operation.</span></span>

<span data-ttu-id="9f935-126">Tento proces je příkladem _vyřešení konfliktu souběžnosti_.</span><span class="sxs-lookup"><span data-stu-id="9f935-126">This process is an example of _resolving a concurrency conflict_.</span></span>

<span data-ttu-id="9f935-127">Řešení konfliktu souběžnosti zahrnuje sloučení čekající změny z aktuální `DbContext` s hodnotami v databázi.</span><span class="sxs-lookup"><span data-stu-id="9f935-127">Resolving a concurrency conflict involves merging the pending changes from the current `DbContext` with the values in the database.</span></span> <span data-ttu-id="9f935-128">Provedeno sloučení hodnoty se liší v závislosti na aplikaci a může přesměrováni vstup uživatele.</span><span class="sxs-lookup"><span data-stu-id="9f935-128">What values get merged will vary based on the application and may be directed by user input.</span></span>

<span data-ttu-id="9f935-129">**Existují tři sady hodnot, které jsou k dispozici pomáhající při řešení konfliktů souběžného zpracování:**</span><span class="sxs-lookup"><span data-stu-id="9f935-129">**There are three sets of values available to help resolve a concurrency conflict:**</span></span>

* <span data-ttu-id="9f935-130">**Aktuální hodnoty** jsou hodnoty, které aplikace se pokusila zapisovat do databáze.</span><span class="sxs-lookup"><span data-stu-id="9f935-130">**Current values** are the values that the application was attempting to write to the database.</span></span>

* <span data-ttu-id="9f935-131">**Původní hodnoty** jsou hodnoty, které byly původně načten z databáze, dříve, než byly provedeny žádné změny.</span><span class="sxs-lookup"><span data-stu-id="9f935-131">**Original values** are the values that were originally retrieved from the database, before any edits were made.</span></span>

* <span data-ttu-id="9f935-132">**Databáze hodnoty** jsou hodnoty aktuálně uložené v databázi.</span><span class="sxs-lookup"><span data-stu-id="9f935-132">**Database values** are the values currently stored in the database.</span></span>

<span data-ttu-id="9f935-133">Je obecný postup pro zpracování konfliktů souběžnosti:</span><span class="sxs-lookup"><span data-stu-id="9f935-133">The general approach to handle a concurrency conflicts is:</span></span>

1. <span data-ttu-id="9f935-134">Zachytit `DbUpdateConcurrencyException` během `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="9f935-134">Catch `DbUpdateConcurrencyException` during `SaveChanges`.</span></span>
2. <span data-ttu-id="9f935-135">Použití `DbUpdateConcurrencyException.Entries` připravit novou sadu změn pro ovlivněné entity.</span><span class="sxs-lookup"><span data-stu-id="9f935-135">Use `DbUpdateConcurrencyException.Entries` to prepare a new set of changes for the affected entities.</span></span>
3. <span data-ttu-id="9f935-136">Obnovte původní hodnoty tokenem souběžnosti tak, aby odrážela aktuální hodnoty v databázi.</span><span class="sxs-lookup"><span data-stu-id="9f935-136">Refresh the original values of the concurrency token to reflect the current values in the database.</span></span>
4. <span data-ttu-id="9f935-137">Proces opakujte, dokud nedojde ke konfliktům dojít.</span><span class="sxs-lookup"><span data-stu-id="9f935-137">Retry the process until no conflicts occur.</span></span>

<span data-ttu-id="9f935-138">V následujícím příkladu `Person.FirstName` a `Person.LastName` jsou nastavené jako tokeny souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="9f935-138">In the following example, `Person.FirstName` and `Person.LastName` are setup as concurrency tokens.</span></span> <span data-ttu-id="9f935-139">Je `// TODO:` komentář v umístění, kde zahrnout konkrétní logiku aplikace zvolit hodnota, která má být uložen.</span><span class="sxs-lookup"><span data-stu-id="9f935-139">There is a `// TODO:` comment in the location where you include application specific logic to choose the value to be saved.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
