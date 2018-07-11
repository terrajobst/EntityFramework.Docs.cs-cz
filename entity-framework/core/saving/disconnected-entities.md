---
title: Odpojené entity – EF Core
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
ms.technology: entity-framework-core
uid: core/saving/disconnected-entities
ms.openlocfilehash: a81b0a26fe98dcc1ddedc11aba2673338c8991e8
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37948981"
---
# <a name="disconnected-entities"></a><span data-ttu-id="19cad-102">Odpojené entity</span><span class="sxs-lookup"><span data-stu-id="19cad-102">Disconnected entities</span></span>

<span data-ttu-id="19cad-103">DbContext instance bude automaticky sledovat entity, které vrátil z databáze.</span><span class="sxs-lookup"><span data-stu-id="19cad-103">A DbContext instance will automatically track entities returned from the database.</span></span> <span data-ttu-id="19cad-104">Změny provedené v těchto entit bude detekován pak, když je volána metoda SaveChanges a aktualizuje databázi podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="19cad-104">Changes made to these entities will then be detected when SaveChanges is called and the database will be updated as needed.</span></span> <span data-ttu-id="19cad-105">Zobrazit [základní Uložit](basic.md) a [souvisejících dat](related-data.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="19cad-105">See [Basic Save](basic.md) and [Related Data](related-data.md) for details.</span></span>

<span data-ttu-id="19cad-106">Ale někdy entity jsou dotazovat pomocí jedné instance kontextu a uložit pomocí jiné instance.</span><span class="sxs-lookup"><span data-stu-id="19cad-106">However, sometimes entities are queried using one context instance and then saved using a different instance.</span></span> <span data-ttu-id="19cad-107">Této situaci často dochází v "odpojené" scénářů, jako je webová aplikace, kde entity, které jsou dotazovat, může odeslat klientovi, upravit, odeslat zpět na server v požadavku a pak uloží.</span><span class="sxs-lookup"><span data-stu-id="19cad-107">This often happens in "disconnected" scenarios such as a web application where the entities are queried, sent to the client, modified, sent back to the server in a request, and then saved.</span></span> <span data-ttu-id="19cad-108">V takovém případě kontextu druhé instance musí zjistit, zda jsou nové entity (by měl být vložen) nebo stávající (třeba aktualizovat).</span><span class="sxs-lookup"><span data-stu-id="19cad-108">In this case, the second context instance needs to know whether the entities are new (should be inserted) or existing (should be updated).</span></span>

> [!TIP]  
> <span data-ttu-id="19cad-109">Můžete zobrazit v tomto článku [ukázka](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="19cad-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) on GitHub.</span></span>

> [!TIP]
> <span data-ttu-id="19cad-110">EF Core můžete sledovat pouze jeden výskyt jakákoli entita s danou hodnotu primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="19cad-110">EF Core can only track one instance of any entity with a given primary key value.</span></span> <span data-ttu-id="19cad-111">Nejlepší způsob, jak tento vyhnete, problém, je použít krátkodobou kontext pro každou jednotku pracovní, tak, že kontext začíná prázdný, obsahuje entit připojený, uloží tyto entity a kontext je uvolněn a zahodí.</span><span class="sxs-lookup"><span data-stu-id="19cad-111">The best way to avoid this being an issue is to use a short-lived context for each unit-of-work such that the context starts empty, has entities attached to it, saves those entities, and then the context is disposed and discarded.</span></span>

## <a name="identifying-new-entities"></a><span data-ttu-id="19cad-112">Určení nové entity</span><span class="sxs-lookup"><span data-stu-id="19cad-112">Identifying new entities</span></span>

### <a name="client-identifies-new-entities"></a><span data-ttu-id="19cad-113">Klient identifikuje nové entity</span><span class="sxs-lookup"><span data-stu-id="19cad-113">Client identifies new entities</span></span>

<span data-ttu-id="19cad-114">Nejjednodušším případě řešit je, když klient informuje server, zda je nový nebo existující entity.</span><span class="sxs-lookup"><span data-stu-id="19cad-114">The simplest case to deal with is when the client informs the server whether the entity is new or existing.</span></span> <span data-ttu-id="19cad-115">Například často požadavek na vložení nové entity se liší od požadavek na aktualizaci existující entity.</span><span class="sxs-lookup"><span data-stu-id="19cad-115">For example, often the request to insert a new entity is different from the request to update an existing entity.</span></span>

<span data-ttu-id="19cad-116">Zbytek tohoto oddílu popisuje případy kde je třeba určit jiným způsobem, jestli se má vložit nebo aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="19cad-116">The remainder of this section covers the cases where it necessary to determine in some other way whether to insert or update.</span></span>

### <a name="with-auto-generated-keys"></a><span data-ttu-id="19cad-117">Pomocí automaticky generovaného klíče</span><span class="sxs-lookup"><span data-stu-id="19cad-117">With auto-generated keys</span></span>

<span data-ttu-id="19cad-118">Hodnota automaticky generovaného klíče lze často určit, zda entita musí přidají nebo aktualizují.</span><span class="sxs-lookup"><span data-stu-id="19cad-118">The value of an automatically generated key can often be used to determine whether an entity needs to be inserted or updated.</span></span> <span data-ttu-id="19cad-119">Pokud klíč není nastavený (to znamená, stále má CLR výchozí hodnotu null, nula, atd.), pak musí být nové entity a potřebuje vkládání.</span><span class="sxs-lookup"><span data-stu-id="19cad-119">If the key has not been set (that is, it still has the CLR default value of null, zero, etc.), then the entity must be new and needs inserting.</span></span> <span data-ttu-id="19cad-120">Na druhé straně Pokud byla nastavena hodnota klíče, potom musí mít už dříve uložil a teď se musí aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="19cad-120">On the other hand, if the key value has been set, then it must have already been previously saved and now needs updating.</span></span> <span data-ttu-id="19cad-121">Jinými slovy Pokud klíč má hodnotu a pak entity byla dotazována může odeslat klientovi a má teď vrátit aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="19cad-121">In other words, if the key has a value, then the entity was queried, sent to the client, and has now come back to be updated.</span></span>

<span data-ttu-id="19cad-122">Je snadné vyhledávat nenastavené klíč, pokud známý typ entity:</span><span class="sxs-lookup"><span data-stu-id="19cad-122">It is easy to check for an unset key when the entity type is known:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewSimple)]

<span data-ttu-id="19cad-123">EF má však také integrované způsob, jak to provést u každého typu entity a typ klíče:</span><span class="sxs-lookup"><span data-stu-id="19cad-123">However, EF also has a built-in way to do this for any entity type and key type:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> <span data-ttu-id="19cad-124">Jsou nastavené klíče jako entity jsou sledovány v kontextech, i v případě, že entita je ve stavu Added.</span><span class="sxs-lookup"><span data-stu-id="19cad-124">Keys are set as soon as entities are tracked by the context, even if the entity is in the Added state.</span></span> <span data-ttu-id="19cad-125">To pomáhá při procházení grafu entit a rozhodování o tom, co dělat s každou, například při použití rozhraní API TrackGraph.</span><span class="sxs-lookup"><span data-stu-id="19cad-125">This helps when traversing a graph of entities and deciding what to do with each, such as when using the TrackGraph API.</span></span> <span data-ttu-id="19cad-126">Hodnota klíče byste měli použít pouze ve tak, jak je znázorněno zde _před_ jakékoli volání ke sledování entity.</span><span class="sxs-lookup"><span data-stu-id="19cad-126">The key value should only be used in the way shown here _before_ any call is made to track the entity.</span></span>

### <a name="with-other-keys"></a><span data-ttu-id="19cad-127">Pomocí dalších klíčů</span><span class="sxs-lookup"><span data-stu-id="19cad-127">With other keys</span></span>

<span data-ttu-id="19cad-128">Některé mechanismus je potřeba k identifikaci nových entit nejsou automaticky generované hodnoty klíče.</span><span class="sxs-lookup"><span data-stu-id="19cad-128">Some other mechanism is needed to identify new entities when key values are not generated automatically.</span></span> <span data-ttu-id="19cad-129">Existují dva hlavní přístupy k tomuto:</span><span class="sxs-lookup"><span data-stu-id="19cad-129">There are two general approaches to this:</span></span>
 * <span data-ttu-id="19cad-130">Dotaz pro entitu</span><span class="sxs-lookup"><span data-stu-id="19cad-130">Query for the entity</span></span>
 * <span data-ttu-id="19cad-131">Předání příznaku z klienta</span><span class="sxs-lookup"><span data-stu-id="19cad-131">Pass a flag from the client</span></span>

<span data-ttu-id="19cad-132">Dotaz pro entitu, použijte metodu Find:</span><span class="sxs-lookup"><span data-stu-id="19cad-132">To query for the entity, just use the Find method:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewQuery)]

<span data-ttu-id="19cad-133">Jde nad rámec tohoto dokumentu chcete zobrazit celý kód pro předání příznaku z klienta.</span><span class="sxs-lookup"><span data-stu-id="19cad-133">It is beyond the scope of this document to show the full code for passing a flag from a client.</span></span> <span data-ttu-id="19cad-134">Ve webové aplikaci obvykle to znamená různých požadavků pro různé akce, nebo předávání některých stavu v požadavku a extrahování v kontroleru.</span><span class="sxs-lookup"><span data-stu-id="19cad-134">In a web app, it usually means making different requests for different actions, or passing some state in the request then extracting it in the controller.</span></span>

## <a name="saving-single-entities"></a><span data-ttu-id="19cad-135">Uložení jednotlivých entit</span><span class="sxs-lookup"><span data-stu-id="19cad-135">Saving single entities</span></span>

<span data-ttu-id="19cad-136">Pokud se označuje, zda insert nebo update je potřeba, pak přidat nebo aktualizovat dá správně:</span><span class="sxs-lookup"><span data-stu-id="19cad-136">If it is known whether or not an insert or update is needed, then either Add or Update can be used appropriately:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

<span data-ttu-id="19cad-137">Ale pokud entita používá automaticky generovaný hodnoty klíče, pak metody Update slouží pro oba případy:</span><span class="sxs-lookup"><span data-stu-id="19cad-137">However, if the entity uses auto-generated key values, then the Update method can be used for both cases:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

<span data-ttu-id="19cad-138">Metoda Update obvykle označuje entity pro aktualizaci, nelze vložit.</span><span class="sxs-lookup"><span data-stu-id="19cad-138">The Update method normally marks the entity for update, not insert.</span></span> <span data-ttu-id="19cad-139">Nicméně pokud entita obsahuje automaticky generovaný klíč a byla nastavena žádná hodnota klíče, pak entity se místo toho automaticky označen pro vložení.</span><span class="sxs-lookup"><span data-stu-id="19cad-139">However, if the entity has a auto-generated key, and no key value has been set, then the entity is instead automatically marked for insert.</span></span>

> [!TIP]  
> <span data-ttu-id="19cad-140">Toto chování byla zavedená v EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="19cad-140">This behavior was introduced in EF Core 2.0.</span></span> <span data-ttu-id="19cad-141">Pro starší verze je vždy nutné explicitně zvolte Přidat nebo aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="19cad-141">For earlier releases it is always necessary to explicitly choose either Add or Update.</span></span>

<span data-ttu-id="19cad-142">Pokud subjektem není pomocí automaticky generovaného klíče, pak aplikace musíte rozhodnout, jestli mají vložit nebo aktualizovat entity: Příklad:</span><span class="sxs-lookup"><span data-stu-id="19cad-142">If the entity is not using auto-generated keys, then the application must decide whether the entity should be inserted or updated: For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

<span data-ttu-id="19cad-143">Jsou zde uvedené kroky:</span><span class="sxs-lookup"><span data-stu-id="19cad-143">The steps here are:</span></span>
* <span data-ttu-id="19cad-144">Přidat hledání vrátí hodnotu null, pak databáze již neobsahuje blog s tímto ID, takže označujeme je jako označte ji pro vložení.</span><span class="sxs-lookup"><span data-stu-id="19cad-144">If Find returns null, then the database doesn't already contain the blog with this ID, so we call Add mark it for insertion.</span></span>
* <span data-ttu-id="19cad-145">Vrátí-li najít entitu, pak existuje v databázi a kontext nyní sleduje existující entity</span><span class="sxs-lookup"><span data-stu-id="19cad-145">If Find returns an entity, then it exists in the database and the context is now tracking the existing entity</span></span>
  * <span data-ttu-id="19cad-146">Potom použijeme SetValues k nastavení hodnot pro všechny vlastnosti v této entitě na ty, které pocházejí z klienta.</span><span class="sxs-lookup"><span data-stu-id="19cad-146">We then use SetValues to set the values for all properties on this entity to those that came from the client.</span></span>
  * <span data-ttu-id="19cad-147">Volání SetValues označí entita, která má být aktualizována podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="19cad-147">The SetValues call will mark the entity to be updated as needed.</span></span>

> [!TIP]  
> <span data-ttu-id="19cad-148">SetValues pouze označíte jako změny vlastností, které mají různé hodnoty na hodnoty v sledované entity.</span><span class="sxs-lookup"><span data-stu-id="19cad-148">SetValues will only mark as modified the properties that have different values to those in the tracked entity.</span></span> <span data-ttu-id="19cad-149">To znamená, že při odeslání aktualizace, budou aktualizovány pouze sloupce, které se ve skutečnosti změnily.</span><span class="sxs-lookup"><span data-stu-id="19cad-149">This means that when the update is sent, only those columns that have actually changed will be updated.</span></span> <span data-ttu-id="19cad-150">(A pokud se nic nezměnilo, pak žádná aktualizace se budou odesílat vůbec.)</span><span class="sxs-lookup"><span data-stu-id="19cad-150">(And if nothing has changed, then no update will be sent at all.)</span></span>

## <a name="working-with-graphs"></a><span data-ttu-id="19cad-151">Práce s grafy</span><span class="sxs-lookup"><span data-stu-id="19cad-151">Working with graphs</span></span>

### <a name="identity-resolution"></a><span data-ttu-id="19cad-152">Rozlišení identity</span><span class="sxs-lookup"><span data-stu-id="19cad-152">Identity resolution</span></span>

<span data-ttu-id="19cad-153">Jak bylo uvedeno výše, EF Core můžete sledovat pouze jeden výskyt jakákoli entita s danou hodnotu primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="19cad-153">As noted above, EF Core can only track one instance of any entity with a given primary key value.</span></span> <span data-ttu-id="19cad-154">Při práci s grafy by měl vytvoří graf v ideálním případě tak, že tento invariantní zachovaný a kontextu má být použita pro pouze jednu jednotku pracovní.</span><span class="sxs-lookup"><span data-stu-id="19cad-154">When working with graphs the graph should ideally be created such that this invariant is maintained, and the context should be used for only one unit-of-work.</span></span> <span data-ttu-id="19cad-155">Pokud graf obsahuje duplicitní hodnoty, pak bude potřeba zpracovat grafu před odesláním do EF provést konsolidaci více instancí do jedné.</span><span class="sxs-lookup"><span data-stu-id="19cad-155">If the graph does contain duplicates, then it will be necessary to process the graph before sending it to EF to consolidate multiple instances into one.</span></span> <span data-ttu-id="19cad-156">Toto video asi triviální kde instance mají konfliktní hodnoty a vztahy, tak konsolidace duplicitní položky by mělo být provedeno co nejdříve v kanálu aplikace, aby řešení konfliktů.</span><span class="sxs-lookup"><span data-stu-id="19cad-156">This may not be trivial where instances have conflicting values and relationships, so consolidating duplicates should be done as soon as possible in your application pipeline to avoid conflict resolution.</span></span>

### <a name="all-newall-existing-entities"></a><span data-ttu-id="19cad-157">Všechny nové/všechny existující entity</span><span class="sxs-lookup"><span data-stu-id="19cad-157">All new/all existing entities</span></span>

<span data-ttu-id="19cad-158">Příklad práci s grafy je vkládání nebo aktualizaci blogu společně s jeho kolekce přidružené příspěvky.</span><span class="sxs-lookup"><span data-stu-id="19cad-158">An example of working with graphs is inserting or updating a blog together with its collection of associated posts.</span></span> <span data-ttu-id="19cad-159">Pokud by měl být vložen všechny entity v grafu nebo by měly být aktualizovány všechny, pak proces je stejné jako nastavení popsané výše pro jednotlivé entity.</span><span class="sxs-lookup"><span data-stu-id="19cad-159">If all the entities in the graph should be inserted, or all should be updated, then the process is the same as described above for single entities.</span></span> <span data-ttu-id="19cad-160">Například graf blogů a příspěvky vytvořené tímto způsobem:</span><span class="sxs-lookup"><span data-stu-id="19cad-160">For example, a graph of blogs and posts created like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

<span data-ttu-id="19cad-161">můžete vložit tímto způsobem:</span><span class="sxs-lookup"><span data-stu-id="19cad-161">can be inserted like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertGraph)]

<span data-ttu-id="19cad-162">Volání přidat označí blogu a všechny příspěvky, které má být vložen.</span><span class="sxs-lookup"><span data-stu-id="19cad-162">The call to Add will mark the blog and all the posts to be inserted.</span></span>

<span data-ttu-id="19cad-163">Podobně pokud potřebujete aktualizovat všechny entity v grafu, pak aktualizací je možné:</span><span class="sxs-lookup"><span data-stu-id="19cad-163">Likewise, if all the entities in a graph need to be updated, then Update can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#UpdateGraph)]

<span data-ttu-id="19cad-164">Aktualizovat budou označeny v blogu a všech jeho příspěvků.</span><span class="sxs-lookup"><span data-stu-id="19cad-164">The blog and all its posts will be marked to be updated.</span></span>

### <a name="mix-of-new-and-existing-entities"></a><span data-ttu-id="19cad-165">Kombinace nová a existující entity</span><span class="sxs-lookup"><span data-stu-id="19cad-165">Mix of new and existing entities</span></span>

<span data-ttu-id="19cad-166">Pomocí automaticky generovaného klíče můžete aktualizace znovu použijí pro vkládání a aktualizace, i v případě, že graf obsahuje entity, které vyžadují vkládání i těch, které vyžadují aktualizaci:</span><span class="sxs-lookup"><span data-stu-id="19cad-166">With auto-generated keys, Update can again be used for both inserts and updates, even if the graph contains a mix of entities that require inserting and those that require updating:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

<span data-ttu-id="19cad-167">Aktualizace označí entitu v grafu, blogu nebo příspěvek pro vložení, pokud nemá sadu klíč-hodnota, zatímco jiné entity jsou označeny pro aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="19cad-167">Update will mark any entity in the graph, blog or post, for insertion if it does not have a key value set, while all other entities are marked for update.</span></span>

<span data-ttu-id="19cad-168">Jako dříve, kdy se nepoužívá automaticky generovaného klíče dotazu a nějaké zpracování je možné:</span><span class="sxs-lookup"><span data-stu-id="19cad-168">As before, when not using auto-generated keys, a query and some processing can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a><span data-ttu-id="19cad-169">Zpracování odstranění</span><span class="sxs-lookup"><span data-stu-id="19cad-169">Handling deletes</span></span>

<span data-ttu-id="19cad-170">Odstranění může být velmi obtížné zpracovat od často neexistence entity znamená, že je nutné ji odstranit.</span><span class="sxs-lookup"><span data-stu-id="19cad-170">Delete can be tricky to handle since often the absence of an entity means that it should be deleted.</span></span> <span data-ttu-id="19cad-171">Jeden způsob, jak to vyřešit, je použití "obnovitelné odstranění" tak, aby entita je označen jako odstraněný místo ve skutečnosti odstraňuje.</span><span class="sxs-lookup"><span data-stu-id="19cad-171">One way to deal with this is to use "soft deletes" such that the entity is marked as deleted rather than actually being deleted.</span></span> <span data-ttu-id="19cad-172">Odstraní se pak stane stejný jako aktualizace.</span><span class="sxs-lookup"><span data-stu-id="19cad-172">Deletes then becomes the same as updates.</span></span> <span data-ttu-id="19cad-173">Obnovitelné odstranění je možné implementovat pomocí [dotazování filtry](xref:core/querying/filters).</span><span class="sxs-lookup"><span data-stu-id="19cad-173">Soft deletes can be implemented in using [query filters](xref:core/querying/filters).</span></span>

<span data-ttu-id="19cad-174">Pro true odstraní běžným postupem je použití rozšíření vzorku dotazu provádět, co je v podstatě rozdíl grafu Příklad:</span><span class="sxs-lookup"><span data-stu-id="19cad-174">For true deletes, a common pattern is to use an extension of the query pattern to perform what is essentially a graph diff. For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a><span data-ttu-id="19cad-175">TrackGraph</span><span class="sxs-lookup"><span data-stu-id="19cad-175">TrackGraph</span></span>

<span data-ttu-id="19cad-176">Interně, přidat, připojit a aktualizace pomocí procházení graf rozhodnutí provedli pro každou entitu v tom, jestli ho musí být označené jako přidanou (Chcete-li vložit), Modified (Chcete-li aktualizovat), Unchanged (Neprovádět žádnou akci), nebo odstraněné (Chcete-li odstranit).</span><span class="sxs-lookup"><span data-stu-id="19cad-176">Internally, Add, Attach, and Update use graph-traversal with a determination made for each entity as to whether it should be marked as Added (to insert), Modified (to update), Unchanged (do nothing), or Deleted (to delete).</span></span> <span data-ttu-id="19cad-177">Tento mechanismus je zveřejněný prostřednictvím rozhraní API TrackGraph.</span><span class="sxs-lookup"><span data-stu-id="19cad-177">This mechanism is exposed via the TrackGraph API.</span></span> <span data-ttu-id="19cad-178">Například předpokládejme, že když klient odešle zpět graf entit nastaví některá příznak u každé entity označující, jak ji by měl být zpracována.</span><span class="sxs-lookup"><span data-stu-id="19cad-178">For example, let's assume that when the client sends back a graph of entities it sets some flag on each entity indicating how it should be handled.</span></span> <span data-ttu-id="19cad-179">TrackGraph je pak možné zpracovat tento příznak:</span><span class="sxs-lookup"><span data-stu-id="19cad-179">TrackGraph can then be used to process this flag:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#TrackGraph)]

<span data-ttu-id="19cad-180">Příznaky se zobrazují pouze jako součást entity pro zjednodušení tento příklad.</span><span class="sxs-lookup"><span data-stu-id="19cad-180">The flags are only shown as part of the entity for simplicity of the example.</span></span> <span data-ttu-id="19cad-181">Obvykle příznaků by být součástí objekt DTO nebo jiný stav zahrnutý v požadavku.</span><span class="sxs-lookup"><span data-stu-id="19cad-181">Typically the flags would be part of a DTO or some other state included in the request.</span></span>
