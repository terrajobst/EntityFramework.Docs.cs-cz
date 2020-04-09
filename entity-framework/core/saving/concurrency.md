---
title: Zpracování konfliktů souběžnosti – základní ef
author: rowanmiller
ms.date: 03/03/2018
uid: core/saving/concurrency
ms.openlocfilehash: a1d1a5a11d482f9104691aa3c072dbd1c548e9f1
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417588"
---
# <a name="handling-concurrency-conflicts"></a><span data-ttu-id="4f780-102">Zpracování konfliktů souběžnosti</span><span class="sxs-lookup"><span data-stu-id="4f780-102">Handling Concurrency Conflicts</span></span>

> [!NOTE]
> <span data-ttu-id="4f780-103">Tato stránka dokumentuje, jak funguje souběžnost v EF Core a jak zpracovat konflikty souběžnosti ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4f780-103">This page documents how concurrency works in EF Core and how to handle concurrency conflicts in your application.</span></span> <span data-ttu-id="4f780-104">Podrobnosti o konfiguraci tokenů souběžnosti ve vašem modelu najdete v tématu [Tokeny souběžnosti.](xref:core/modeling/concurrency)</span><span class="sxs-lookup"><span data-stu-id="4f780-104">See [Concurrency Tokens](xref:core/modeling/concurrency) for details on how to configure concurrency tokens in your model.</span></span>

> [!TIP]
> <span data-ttu-id="4f780-105">Ukázku tohoto článku [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Concurrency/) můžete zobrazit na GitHubu.</span><span class="sxs-lookup"><span data-stu-id="4f780-105">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Concurrency/) on GitHub.</span></span>

<span data-ttu-id="4f780-106">_Souběžnost databáze_ odkazuje na situace, ve kterých více procesů nebo uživatelů přístup nebo změnit stejná data v databázi ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="4f780-106">_Database concurrency_ refers to situations in which multiple processes or users access or change the same data in a database at the same time.</span></span> <span data-ttu-id="4f780-107">_Řízení souběžnosti_ odkazuje na konkrétní mechanismy používané k zajištění konzistence dat v přítomnosti souběžných změn.</span><span class="sxs-lookup"><span data-stu-id="4f780-107">_Concurrency control_ refers to specific mechanisms used to ensure data consistency in presence of concurrent changes.</span></span>

<span data-ttu-id="4f780-108">EF Core implementuje _optimistické řízení souběžnosti_, což znamená, že umožní více procesů nebo uživatelů provádět změny nezávisle bez režie synchronizace nebo uzamčení.</span><span class="sxs-lookup"><span data-stu-id="4f780-108">EF Core implements _optimistic concurrency control_, meaning that it will let multiple processes or users make changes independently without the overhead of synchronization or locking.</span></span> <span data-ttu-id="4f780-109">V ideální situaci, tyto změny nebudou zasahovat do sebe, a proto budou moci uspět.</span><span class="sxs-lookup"><span data-stu-id="4f780-109">In the ideal situation, these changes will not interfere with each other and therefore will be able to succeed.</span></span> <span data-ttu-id="4f780-110">V nejhorším případě se dva nebo více procesů pokusí provést konfliktní změny a pouze jeden z nich by měl být úspěšný.</span><span class="sxs-lookup"><span data-stu-id="4f780-110">In the worst case scenario, two or more processes will attempt to make conflicting changes, and only one of them should succeed.</span></span>

## <a name="how-concurrency-control-works-in-ef-core"></a><span data-ttu-id="4f780-111">Jak funguje řízení souběžnosti v ef core</span><span class="sxs-lookup"><span data-stu-id="4f780-111">How concurrency control works in EF Core</span></span>

<span data-ttu-id="4f780-112">Vlastnosti nakonfigurované jako tokeny souběžnosti se používají k implementaci `SaveChanges`optimistické řízení souběžnosti: při každém provedení operace aktualizace nebo odstranění během , hodnota tokenu souběžnosti v databázi je porovnána s původní hodnotou přečtenou EF Core.</span><span class="sxs-lookup"><span data-stu-id="4f780-112">Properties configured as concurrency tokens are used to implement optimistic concurrency control: whenever an update or delete operation is performed during `SaveChanges`, the value of the concurrency token on the database is compared against the original value read by EF Core.</span></span>

- <span data-ttu-id="4f780-113">Pokud se hodnoty shodují, operace může být dokončena.</span><span class="sxs-lookup"><span data-stu-id="4f780-113">If the values match, the operation can complete.</span></span>
- <span data-ttu-id="4f780-114">Pokud se hodnoty neshodují, EF Core předpokládá, že jiný uživatel provedl konfliktní operaci a přeruší aktuální transakci.</span><span class="sxs-lookup"><span data-stu-id="4f780-114">If the values do not match, EF Core assumes that another user has performed a conflicting operation and aborts the current transaction.</span></span>

<span data-ttu-id="4f780-115">Situace, kdy jiný uživatel provedl operaci, která je v konfliktu s aktuální operací, se označuje jako _konflikt souběžnosti_.</span><span class="sxs-lookup"><span data-stu-id="4f780-115">The situation when another user has performed an operation that conflicts with the current operation is known as _concurrency conflict_.</span></span>

<span data-ttu-id="4f780-116">Poskytovatelé databáze jsou zodpovědní za implementaci porovnání hodnot tokenu souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="4f780-116">Database providers are responsible for implementing the comparison of concurrency token values.</span></span>

<span data-ttu-id="4f780-117">Na relačních databázích EF Core obsahuje kontrolu hodnoty `WHERE` tokenu `UPDATE` souběžnosti v klauzuli any nebo `DELETE` statements.</span><span class="sxs-lookup"><span data-stu-id="4f780-117">On relational databases EF Core includes a check for the value of the concurrency token in the `WHERE` clause of any `UPDATE` or `DELETE` statements.</span></span> <span data-ttu-id="4f780-118">Po provedení příkazy EF Core přečte počet řádků, které byly ovlivněny.</span><span class="sxs-lookup"><span data-stu-id="4f780-118">After executing the statements, EF Core reads the number of rows that were affected.</span></span>

<span data-ttu-id="4f780-119">Pokud nejsou ovlivněny žádné řádky, je zjištěn konflikt souběžnosti a EF Core vyvolá `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="4f780-119">If no rows are affected, a concurrency conflict is detected, and EF Core throws `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="4f780-120">Například můžeme chtít `LastName` nakonfigurovat `Person` na token souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="4f780-120">For example, we may want to configure `LastName` on `Person` to be a concurrency token.</span></span> <span data-ttu-id="4f780-121">Pak všechny operace aktualizace na osobu bude `WHERE` zahrnovat kontrolu souběžnosti v klauzuli:</span><span class="sxs-lookup"><span data-stu-id="4f780-121">Then any update operation on Person will include the concurrency check in the `WHERE` clause:</span></span>

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a><span data-ttu-id="4f780-122">Řešení konfliktů souběžnosti</span><span class="sxs-lookup"><span data-stu-id="4f780-122">Resolving concurrency conflicts</span></span>

<span data-ttu-id="4f780-123">Pokračování v předchozím příkladu, pokud jeden uživatel pokusí `Person`uložit některé změny , `LastName`ale jiný uživatel již změnil , pak bude vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="4f780-123">Continuing with the previous example, if one user tries to save some changes to a `Person`, but another user has already changed the `LastName`, then an exception will be thrown.</span></span>

<span data-ttu-id="4f780-124">V tomto okamžiku aplikace může jednoduše informovat uživatele, že aktualizace nebyla úspěšná z důvodu konfliktních změn a přejít.</span><span class="sxs-lookup"><span data-stu-id="4f780-124">At this point, the application could simply inform the user that the update was not successful due to conflicting changes and move on.</span></span> <span data-ttu-id="4f780-125">Ale může být žádoucí vyzvat uživatele, aby zajistily, že tento záznam stále představuje stejnou skutečnou osobu a opakovat operaci.</span><span class="sxs-lookup"><span data-stu-id="4f780-125">But it may be desirable to prompt the user to ensure this record still represents the same actual person and to retry the operation.</span></span>

<span data-ttu-id="4f780-126">Tento proces je příkladem _řešení konfliktu souběžnosti_.</span><span class="sxs-lookup"><span data-stu-id="4f780-126">This process is an example of _resolving a concurrency conflict_.</span></span>

<span data-ttu-id="4f780-127">Řešení konfliktu souběžnosti zahrnuje sloučení čekající změny z `DbContext` aktuální s hodnotami v databázi.</span><span class="sxs-lookup"><span data-stu-id="4f780-127">Resolving a concurrency conflict involves merging the pending changes from the current `DbContext` with the values in the database.</span></span> <span data-ttu-id="4f780-128">Jaké hodnoty se sloučí se bude lišit v závislosti na aplikaci a mohou být řízeny vstupem uživatele.</span><span class="sxs-lookup"><span data-stu-id="4f780-128">What values get merged will vary based on the application and may be directed by user input.</span></span>

<span data-ttu-id="4f780-129">**K dispozici jsou tři sady hodnot, které pomáhají vyřešit konflikt souběžnosti:**</span><span class="sxs-lookup"><span data-stu-id="4f780-129">**There are three sets of values available to help resolve a concurrency conflict:**</span></span>

- <span data-ttu-id="4f780-130">**Aktuální hodnoty** jsou hodnoty, které se aplikace pokoušela zapsat do databáze.</span><span class="sxs-lookup"><span data-stu-id="4f780-130">**Current values** are the values that the application was attempting to write to the database.</span></span>
- <span data-ttu-id="4f780-131">**Původní hodnoty** jsou hodnoty, které byly původně načteny z databáze, před všechny úpravy byly provedeny.</span><span class="sxs-lookup"><span data-stu-id="4f780-131">**Original values** are the values that were originally retrieved from the database, before any edits were made.</span></span>
- <span data-ttu-id="4f780-132">**Hodnoty databáze** jsou hodnoty aktuálně uložené v databázi.</span><span class="sxs-lookup"><span data-stu-id="4f780-132">**Database values** are the values currently stored in the database.</span></span>

<span data-ttu-id="4f780-133">Obecný přístup ke zpracování konfliktů souběžnosti je:</span><span class="sxs-lookup"><span data-stu-id="4f780-133">The general approach to handle a concurrency conflicts is:</span></span>

1. <span data-ttu-id="4f780-134">Úlovek `DbUpdateConcurrencyException` `SaveChanges`během .</span><span class="sxs-lookup"><span data-stu-id="4f780-134">Catch `DbUpdateConcurrencyException` during `SaveChanges`.</span></span>
2. <span data-ttu-id="4f780-135">Slouží `DbUpdateConcurrencyException.Entries` k přípravě nové sady změn pro ovlivněné entity.</span><span class="sxs-lookup"><span data-stu-id="4f780-135">Use `DbUpdateConcurrencyException.Entries` to prepare a new set of changes for the affected entities.</span></span>
3. <span data-ttu-id="4f780-136">Aktualizujte původní hodnoty tokenu souběžnosti tak, aby odrážely aktuální hodnoty v databázi.</span><span class="sxs-lookup"><span data-stu-id="4f780-136">Refresh the original values of the concurrency token to reflect the current values in the database.</span></span>
4. <span data-ttu-id="4f780-137">Opakujte proces, dokud nedojde ke konfliktům.</span><span class="sxs-lookup"><span data-stu-id="4f780-137">Retry the process until no conflicts occur.</span></span>

<span data-ttu-id="4f780-138">V následujícím `Person.FirstName` příkladu `Person.LastName` a jsou nastaveny jako tokeny souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="4f780-138">In the following example, `Person.FirstName` and `Person.LastName` are set up as concurrency tokens.</span></span> <span data-ttu-id="4f780-139">V umístění `// TODO:` je poznámka, kde zahrnete logiku specifickou pro aplikaci a zvolte hodnotu, která má být uložena.</span><span class="sxs-lookup"><span data-stu-id="4f780-139">There is a `// TODO:` comment in the location where you include application specific logic to choose the value to be saved.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
