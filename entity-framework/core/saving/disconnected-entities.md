---
title: Odpojené entity – jádro EF
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
uid: core/saving/disconnected-entities
ms.openlocfilehash: 421531e68ac98c0553938f1c24892701f22fef3c
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417595"
---
# <a name="disconnected-entities"></a><span data-ttu-id="04ec3-102">Odpojené entity</span><span class="sxs-lookup"><span data-stu-id="04ec3-102">Disconnected entities</span></span>

<span data-ttu-id="04ec3-103">Instance DbContext bude automaticky sledovat entity vrácené z databáze.</span><span class="sxs-lookup"><span data-stu-id="04ec3-103">A DbContext instance will automatically track entities returned from the database.</span></span> <span data-ttu-id="04ec3-104">Změny provedené v těchto entitách pak budou zjištěny při SaveChanges je volána a databáze bude aktualizován podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="04ec3-104">Changes made to these entities will then be detected when SaveChanges is called and the database will be updated as needed.</span></span> <span data-ttu-id="04ec3-105">Podrobnosti [najdete v tématu Základní ukládání](basic.md) [a související data.](related-data.md)</span><span class="sxs-lookup"><span data-stu-id="04ec3-105">See [Basic Save](basic.md) and [Related Data](related-data.md) for details.</span></span>

<span data-ttu-id="04ec3-106">Někdy jsou však entity dotazovány pomocí jedné instance kontextu a poté uloženy pomocí jiné instance.</span><span class="sxs-lookup"><span data-stu-id="04ec3-106">However, sometimes entities are queried using one context instance and then saved using a different instance.</span></span> <span data-ttu-id="04ec3-107">To se často děje v "odpojené" scénáře, jako je například webové aplikace, kde jsou subjekty dotazován, odeslány klientovi, změněn, odeslány zpět na server v požadavku a potom uloženy.</span><span class="sxs-lookup"><span data-stu-id="04ec3-107">This often happens in "disconnected" scenarios such as a web application where the entities are queried, sent to the client, modified, sent back to the server in a request, and then saved.</span></span> <span data-ttu-id="04ec3-108">V takovém případě druhá kontextová instance potřebuje vědět, zda jsou entity nové (měly by být vloženy) nebo existující (by měly být aktualizovány).</span><span class="sxs-lookup"><span data-stu-id="04ec3-108">In this case, the second context instance needs to know whether the entities are new (should be inserted) or existing (should be updated).</span></span>

<!-- markdownlint-disable MD028 -->
> [!TIP]
> <span data-ttu-id="04ec3-109">Ukázku tohoto článku [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Disconnected/) můžete zobrazit na GitHubu.</span><span class="sxs-lookup"><span data-stu-id="04ec3-109">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Disconnected/) on GitHub.</span></span>

> [!TIP]
> <span data-ttu-id="04ec3-110">EF Core může sledovat pouze jednu instanci libovolné entity s danou hodnotou primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="04ec3-110">EF Core can only track one instance of any entity with a given primary key value.</span></span> <span data-ttu-id="04ec3-111">Nejlepší způsob, jak se tomuvyhnout problému je použít kontext s krátkou životností pro každou jednotku práce tak, aby kontext začíná prázdný, má entity k němu připojené, uloží tyto entity a pak je kontext uvolněn a zahozen.</span><span class="sxs-lookup"><span data-stu-id="04ec3-111">The best way to avoid this being an issue is to use a short-lived context for each unit-of-work such that the context starts empty, has entities attached to it, saves those entities, and then the context is disposed and discarded.</span></span>
<!-- markdownlint-enable MD028 -->

## <a name="identifying-new-entities"></a><span data-ttu-id="04ec3-112">Identifikace nových entit</span><span class="sxs-lookup"><span data-stu-id="04ec3-112">Identifying new entities</span></span>

### <a name="client-identifies-new-entities"></a><span data-ttu-id="04ec3-113">Klient identifikuje nové entity</span><span class="sxs-lookup"><span data-stu-id="04ec3-113">Client identifies new entities</span></span>

<span data-ttu-id="04ec3-114">Nejjednodušší případ, který je třeba řešit, je, když klient informuje server, zda je entita nová nebo existující.</span><span class="sxs-lookup"><span data-stu-id="04ec3-114">The simplest case to deal with is when the client informs the server whether the entity is new or existing.</span></span> <span data-ttu-id="04ec3-115">Například často požadavek na vložení nové entity se liší od požadavku na aktualizaci existující entity.</span><span class="sxs-lookup"><span data-stu-id="04ec3-115">For example, often the request to insert a new entity is different from the request to update an existing entity.</span></span>

<span data-ttu-id="04ec3-116">Zbývající část této části se týká případů, kdy je nutné určit jiným způsobem, zda vložit nebo aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="04ec3-116">The remainder of this section covers the cases where it necessary to determine in some other way whether to insert or update.</span></span>

### <a name="with-auto-generated-keys"></a><span data-ttu-id="04ec3-117">S automaticky generovanými klávesami</span><span class="sxs-lookup"><span data-stu-id="04ec3-117">With auto-generated keys</span></span>

<span data-ttu-id="04ec3-118">Hodnotu automaticky generovaného klíče lze často použít k určení, zda je třeba entitu vložit nebo aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="04ec3-118">The value of an automatically generated key can often be used to determine whether an entity needs to be inserted or updated.</span></span> <span data-ttu-id="04ec3-119">Pokud klíč nebyl nastaven (to znamená, že má stále výchozí hodnotu CLR null, nula atd.), musí být entita nová a musí být potřeba vložit.</span><span class="sxs-lookup"><span data-stu-id="04ec3-119">If the key has not been set (that is, it still has the CLR default value of null, zero, etc.), then the entity must be new and needs inserting.</span></span> <span data-ttu-id="04ec3-120">Na druhou stranu, pokud byla nastavena hodnota klíče, musí již být dříve uložena a nyní potřebuje aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="04ec3-120">On the other hand, if the key value has been set, then it must have already been previously saved and now needs updating.</span></span> <span data-ttu-id="04ec3-121">Jinými slovy, pokud klíč má hodnotu, pak entita byla dotazována, odeslána klientovi a nyní se vrátil k aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="04ec3-121">In other words, if the key has a value, then the entity was queried, sent to the client, and has now come back to be updated.</span></span>

<span data-ttu-id="04ec3-122">Je snadné zkontrolovat odnastavený klíč, pokud je znám typ entity:</span><span class="sxs-lookup"><span data-stu-id="04ec3-122">It is easy to check for an unset key when the entity type is known:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#IsItNewSimple)]

<span data-ttu-id="04ec3-123">Ef má však také předdefinovaný způsob, jak to provést pro jakýkoli typ entity a typ klíče:</span><span class="sxs-lookup"><span data-stu-id="04ec3-123">However, EF also has a built-in way to do this for any entity type and key type:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> <span data-ttu-id="04ec3-124">Klíče jsou nastaveny, jakmile jsou entity sledovány kontextem, i když je entita ve stavu Přidání.</span><span class="sxs-lookup"><span data-stu-id="04ec3-124">Keys are set as soon as entities are tracked by the context, even if the entity is in the Added state.</span></span> <span data-ttu-id="04ec3-125">To pomáhá při procházení grafu entit a rozhodování o tom, co dělat s každým, například při použití rozhraní API trackgraph.</span><span class="sxs-lookup"><span data-stu-id="04ec3-125">This helps when traversing a graph of entities and deciding what to do with each, such as when using the TrackGraph API.</span></span> <span data-ttu-id="04ec3-126">Hodnota klíče by měla být použita pouze způsobem zde zobrazena _před_ jakékoli volání ke sledování entity.</span><span class="sxs-lookup"><span data-stu-id="04ec3-126">The key value should only be used in the way shown here _before_ any call is made to track the entity.</span></span>

### <a name="with-other-keys"></a><span data-ttu-id="04ec3-127">S dalšími klávesami</span><span class="sxs-lookup"><span data-stu-id="04ec3-127">With other keys</span></span>

<span data-ttu-id="04ec3-128">Některé další mechanismus je potřeba k identifikaci nových entit, když hodnoty klíče nejsou generovány automaticky.</span><span class="sxs-lookup"><span data-stu-id="04ec3-128">Some other mechanism is needed to identify new entities when key values are not generated automatically.</span></span> <span data-ttu-id="04ec3-129">Existují dva obecné přístupy k tomuto:</span><span class="sxs-lookup"><span data-stu-id="04ec3-129">There are two general approaches to this:</span></span>

* <span data-ttu-id="04ec3-130">Dotaz na entitu</span><span class="sxs-lookup"><span data-stu-id="04ec3-130">Query for the entity</span></span>
* <span data-ttu-id="04ec3-131">Předání příznaku od klienta</span><span class="sxs-lookup"><span data-stu-id="04ec3-131">Pass a flag from the client</span></span>

<span data-ttu-id="04ec3-132">Chcete-li zadat dotaz na entitu, použijte metodu Najít:</span><span class="sxs-lookup"><span data-stu-id="04ec3-132">To query for the entity, just use the Find method:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#IsItNewQuery)]

<span data-ttu-id="04ec3-133">Je nad rámec tohoto dokumentu zobrazit úplný kód pro předávání příznak u klienta.</span><span class="sxs-lookup"><span data-stu-id="04ec3-133">It is beyond the scope of this document to show the full code for passing a flag from a client.</span></span> <span data-ttu-id="04ec3-134">Ve webové aplikaci obvykle znamená provádění různých požadavků na různé akce nebo předání některého stavu v požadavku a jeho extrahování v řadiči.</span><span class="sxs-lookup"><span data-stu-id="04ec3-134">In a web app, it usually means making different requests for different actions, or passing some state in the request then extracting it in the controller.</span></span>

## <a name="saving-single-entities"></a><span data-ttu-id="04ec3-135">Uložení jednotlivých entit</span><span class="sxs-lookup"><span data-stu-id="04ec3-135">Saving single entities</span></span>

<span data-ttu-id="04ec3-136">Pokud je známo, zda je potřeba vložit nebo aktualizovat, pak buď Přidat nebo Aktualizovat lze použít odpovídajícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="04ec3-136">If it is known whether or not an insert or update is needed, then either Add or Update can be used appropriately:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

<span data-ttu-id="04ec3-137">Pokud však entita používá automaticky generované hodnoty klíče, lze pro oba případy použít metodu Update:</span><span class="sxs-lookup"><span data-stu-id="04ec3-137">However, if the entity uses auto-generated key values, then the Update method can be used for both cases:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

<span data-ttu-id="04ec3-138">Metoda Update obvykle označí entitu pro aktualizaci, nikoli pro vložení.</span><span class="sxs-lookup"><span data-stu-id="04ec3-138">The Update method normally marks the entity for update, not insert.</span></span> <span data-ttu-id="04ec3-139">Pokud však entita má automaticky generovaný klíč a nebyla nastavena žádná hodnota klíče, je entita místo toho automaticky označena pro vložení.</span><span class="sxs-lookup"><span data-stu-id="04ec3-139">However, if the entity has a auto-generated key, and no key value has been set, then the entity is instead automatically marked for insert.</span></span>

> [!TIP]  
> <span data-ttu-id="04ec3-140">Toto chování bylo zavedeno v EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="04ec3-140">This behavior was introduced in EF Core 2.0.</span></span> <span data-ttu-id="04ec3-141">Pro dřívější verze je vždy nutné explicitně zvolit přidat nebo aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="04ec3-141">For earlier releases it is always necessary to explicitly choose either Add or Update.</span></span>

<span data-ttu-id="04ec3-142">Pokud entita nepoužívá automaticky generované klíče, musí aplikace rozhodnout, zda má být entita vložena nebo aktualizována: Například:</span><span class="sxs-lookup"><span data-stu-id="04ec3-142">If the entity is not using auto-generated keys, then the application must decide whether the entity should be inserted or updated: For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

<span data-ttu-id="04ec3-143">Kroky jsou zde:</span><span class="sxs-lookup"><span data-stu-id="04ec3-143">The steps here are:</span></span>

* <span data-ttu-id="04ec3-144">Pokud najít vrátí null, pak databáze již neobsahuje blog s tímto ID, takže voláme Přidat značku pro vložení.</span><span class="sxs-lookup"><span data-stu-id="04ec3-144">If Find returns null, then the database doesn't already contain the blog with this ID, so we call Add mark it for insertion.</span></span>
* <span data-ttu-id="04ec3-145">Pokud najít vrátí entitu, pak existuje v databázi a kontext je nyní sledování existující entity</span><span class="sxs-lookup"><span data-stu-id="04ec3-145">If Find returns an entity, then it exists in the database and the context is now tracking the existing entity</span></span>
  * <span data-ttu-id="04ec3-146">Potom použijeme SetValues k nastavení hodnot pro všechny vlastnosti této entity na ty, které přišly z klienta.</span><span class="sxs-lookup"><span data-stu-id="04ec3-146">We then use SetValues to set the values for all properties on this entity to those that came from the client.</span></span>
  * <span data-ttu-id="04ec3-147">Volání SetValues označí entitu, která má být podle potřeby aktualizována.</span><span class="sxs-lookup"><span data-stu-id="04ec3-147">The SetValues call will mark the entity to be updated as needed.</span></span>

> [!TIP]  
> <span data-ttu-id="04ec3-148">SetValues bude označovat pouze jako upravené vlastnosti, které mají jiné hodnoty než v sledované entitě.</span><span class="sxs-lookup"><span data-stu-id="04ec3-148">SetValues will only mark as modified the properties that have different values to those in the tracked entity.</span></span> <span data-ttu-id="04ec3-149">To znamená, že při odeslání aktualizace budou aktualizovány pouze sloupce, které byly skutečně změněny.</span><span class="sxs-lookup"><span data-stu-id="04ec3-149">This means that when the update is sent, only those columns that have actually changed will be updated.</span></span> <span data-ttu-id="04ec3-150">(A pokud se nic nezměnilo, nebude odeslána žádná aktualizace.)</span><span class="sxs-lookup"><span data-stu-id="04ec3-150">(And if nothing has changed, then no update will be sent at all.)</span></span>

## <a name="working-with-graphs"></a><span data-ttu-id="04ec3-151">Práce s grafy</span><span class="sxs-lookup"><span data-stu-id="04ec3-151">Working with graphs</span></span>

### <a name="identity-resolution"></a><span data-ttu-id="04ec3-152">Rozlišení identity</span><span class="sxs-lookup"><span data-stu-id="04ec3-152">Identity resolution</span></span>

<span data-ttu-id="04ec3-153">Jak je uvedeno výše, EF Core můžete sledovat pouze jednu instanci libovolné entity s danou hodnotou primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="04ec3-153">As noted above, EF Core can only track one instance of any entity with a given primary key value.</span></span> <span data-ttu-id="04ec3-154">Při práci s grafy graf by měl být v ideálním případě vytvořen tak, aby tato invarianta je zachována a kontext by měl být použit pouze pro jednu jednotku práce.</span><span class="sxs-lookup"><span data-stu-id="04ec3-154">When working with graphs the graph should ideally be created such that this invariant is maintained, and the context should be used for only one unit-of-work.</span></span> <span data-ttu-id="04ec3-155">Pokud graf obsahuje duplikáty, bude nutné graf zpracovat před odesláním do EF, aby se konsolidovalo více instancí do jednoho.</span><span class="sxs-lookup"><span data-stu-id="04ec3-155">If the graph does contain duplicates, then it will be necessary to process the graph before sending it to EF to consolidate multiple instances into one.</span></span> <span data-ttu-id="04ec3-156">To nemusí být triviální, pokud instance mají konfliktní hodnoty a vztahy, takže konsolidace duplicity by mělo být provedeno co nejdříve v kanálu aplikace, aby se zabránilo řešení konfliktů.</span><span class="sxs-lookup"><span data-stu-id="04ec3-156">This may not be trivial where instances have conflicting values and relationships, so consolidating duplicates should be done as soon as possible in your application pipeline to avoid conflict resolution.</span></span>

### <a name="all-newall-existing-entities"></a><span data-ttu-id="04ec3-157">Všechny nové/všechny existující entity</span><span class="sxs-lookup"><span data-stu-id="04ec3-157">All new/all existing entities</span></span>

<span data-ttu-id="04ec3-158">Příkladem práce s grafy je vložení nebo aktualizace blogu spolu s jeho sbírkou přidružených příspěvků.</span><span class="sxs-lookup"><span data-stu-id="04ec3-158">An example of working with graphs is inserting or updating a blog together with its collection of associated posts.</span></span> <span data-ttu-id="04ec3-159">Pokud by měly být vloženy všechny entity v grafu nebo všechny by měly být aktualizovány, pak je proces stejný, jak je popsáno výše pro jednotlivé entity.</span><span class="sxs-lookup"><span data-stu-id="04ec3-159">If all the entities in the graph should be inserted, or all should be updated, then the process is the same as described above for single entities.</span></span> <span data-ttu-id="04ec3-160">Například graf blogů a příspěvků vytvořených takto:</span><span class="sxs-lookup"><span data-stu-id="04ec3-160">For example, a graph of blogs and posts created like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

<span data-ttu-id="04ec3-161">lze vložit takto:</span><span class="sxs-lookup"><span data-stu-id="04ec3-161">can be inserted like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertGraph)]

<span data-ttu-id="04ec3-162">Výzva přidat označí blog a všechny příspěvky, které mají být vloženy.</span><span class="sxs-lookup"><span data-stu-id="04ec3-162">The call to Add will mark the blog and all the posts to be inserted.</span></span>

<span data-ttu-id="04ec3-163">Podobně pokud je třeba aktualizovat všechny entity v grafu, lze použít aktualizaci:</span><span class="sxs-lookup"><span data-stu-id="04ec3-163">Likewise, if all the entities in a graph need to be updated, then Update can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#UpdateGraph)]

<span data-ttu-id="04ec3-164">Blog a všechny jeho příspěvky budou označeny jako aktualizované.</span><span class="sxs-lookup"><span data-stu-id="04ec3-164">The blog and all its posts will be marked to be updated.</span></span>

### <a name="mix-of-new-and-existing-entities"></a><span data-ttu-id="04ec3-165">Kombinace nových a stávajících subjektů</span><span class="sxs-lookup"><span data-stu-id="04ec3-165">Mix of new and existing entities</span></span>

<span data-ttu-id="04ec3-166">S automaticky generovanými klíči lze aktualizaci znovu použít pro vložení i aktualizace, i když graf obsahuje kombinaci entit, které vyžadují vložení, a těch, které vyžadují aktualizaci:</span><span class="sxs-lookup"><span data-stu-id="04ec3-166">With auto-generated keys, Update can again be used for both inserts and updates, even if the graph contains a mix of entities that require inserting and those that require updating:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

<span data-ttu-id="04ec3-167">Aktualizace označí libovolnou entitu v grafu, blogu nebo příspěvku pro vložení, pokud nemá nastavenou hodnotu klíče, zatímco všechny ostatní entity jsou označeny pro aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="04ec3-167">Update will mark any entity in the graph, blog or post, for insertion if it does not have a key value set, while all other entities are marked for update.</span></span>

<span data-ttu-id="04ec3-168">Stejně jako dříve, pokud nepoužíváte automaticky generované klíče, lze použít dotaz a některé zpracování:</span><span class="sxs-lookup"><span data-stu-id="04ec3-168">As before, when not using auto-generated keys, a query and some processing can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a><span data-ttu-id="04ec3-169">Zpracování odstranění</span><span class="sxs-lookup"><span data-stu-id="04ec3-169">Handling deletes</span></span>

<span data-ttu-id="04ec3-170">Odstranění může být obtížné zpracovat, protože často absence entity znamená, že by měla být odstraněna.</span><span class="sxs-lookup"><span data-stu-id="04ec3-170">Delete can be tricky to handle since often the absence of an entity means that it should be deleted.</span></span> <span data-ttu-id="04ec3-171">Jedním ze způsobů, jak se s tím vypořádat, je použití "měkkých odstranění", takže entita je označena jako odstraněná, nikoli skutečně odstraněna.</span><span class="sxs-lookup"><span data-stu-id="04ec3-171">One way to deal with this is to use "soft deletes" such that the entity is marked as deleted rather than actually being deleted.</span></span> <span data-ttu-id="04ec3-172">Odstraní pak se stane stejným jako aktualizace.</span><span class="sxs-lookup"><span data-stu-id="04ec3-172">Deletes then becomes the same as updates.</span></span> <span data-ttu-id="04ec3-173">Obnovitelné odstranění lze implementovat pomocí [filtrů dotazů](xref:core/querying/filters).</span><span class="sxs-lookup"><span data-stu-id="04ec3-173">Soft deletes can be implemented in using [query filters](xref:core/querying/filters).</span></span>

<span data-ttu-id="04ec3-174">Pro true odstraní, společný vzor je použít rozšíření vzorku dotazu k provedení co je v podstatě rozdíl grafu.</span><span class="sxs-lookup"><span data-stu-id="04ec3-174">For true deletes, a common pattern is to use an extension of the query pattern to perform what is essentially a graph diff.</span></span> <span data-ttu-id="04ec3-175">Příklad:</span><span class="sxs-lookup"><span data-stu-id="04ec3-175">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a><span data-ttu-id="04ec3-176">TrackGraph</span><span class="sxs-lookup"><span data-stu-id="04ec3-176">TrackGraph</span></span>

<span data-ttu-id="04ec3-177">Interně Přidat, Připojit a Aktualizovat použít graf-traversal s určením pro každou entitu, zda by měla být označena jako Přidáno (vložit), Změněno (aktualizovat), Nezměněno (nedělat nic) nebo Odstraněno (odstranit).</span><span class="sxs-lookup"><span data-stu-id="04ec3-177">Internally, Add, Attach, and Update use graph-traversal with a determination made for each entity as to whether it should be marked as Added (to insert), Modified (to update), Unchanged (do nothing), or Deleted (to delete).</span></span> <span data-ttu-id="04ec3-178">Tento mechanismus je vystaven prostřednictvím rozhraní API trackgraphu.</span><span class="sxs-lookup"><span data-stu-id="04ec3-178">This mechanism is exposed via the TrackGraph API.</span></span> <span data-ttu-id="04ec3-179">Předpokládejme například, že když klient odešle zpět graf entit, nastaví u každé entity určitý příznak označující, jak by měl být zpracován.</span><span class="sxs-lookup"><span data-stu-id="04ec3-179">For example, let's assume that when the client sends back a graph of entities it sets some flag on each entity indicating how it should be handled.</span></span> <span data-ttu-id="04ec3-180">TrackGraph pak lze použít ke zpracování tohoto příznaku:</span><span class="sxs-lookup"><span data-stu-id="04ec3-180">TrackGraph can then be used to process this flag:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#TrackGraph)]

<span data-ttu-id="04ec3-181">Příznaky jsou zobrazeny pouze jako součást entity pro jednoduchost příkladu.</span><span class="sxs-lookup"><span data-stu-id="04ec3-181">The flags are only shown as part of the entity for simplicity of the example.</span></span> <span data-ttu-id="04ec3-182">Obvykle příznaky by být součástí DTO nebo nějaký jiný stav zahrnuty v požadavku.</span><span class="sxs-lookup"><span data-stu-id="04ec3-182">Typically the flags would be part of a DTO or some other state included in the request.</span></span>
